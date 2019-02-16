---
title: Codebasierte Konfiguration – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 13886d24-2c74-4a00-89eb-aa0dee328d83
ms.openlocfilehash: c317f112f713612f7b9aef3764a0bd004fef5424
ms.sourcegitcommit: 735715f10cc8a231c213e4f055d79f0effd86570
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/16/2019
ms.locfileid: "56325352"
---
# <a name="code-based-configuration"></a>Codebasierte Konfiguration
> [!NOTE]
> **Nur EF6 und höher:** Die Features, APIs usw., die auf dieser Seite erläutert werden, wurden in Entity Framework 6 eingeführt. Wenn Sie eine frühere Version verwenden, gelten manche Informationen nicht.  

Konfiguration für eine Entity Framework-Anwendung kann in einer Konfigurationsdatei (app.config/web.config) oder durch Code angegeben werden. Letzteres wird als codebasierte Konfiguration bezeichnet.  

Konfiguration in einer Konfigurationsdatei finden Sie auf eine [separaten Artikel](config-file.md). Codebasierte Konfiguration hat Vorrang vor die Config-Datei. Das heißt, wenn eine Konfigurationsoption in sowohl Code und in der Konfigurationsdatei festgelegt ist, wird die Einstellung in der Config-Datei verwendet.  

## <a name="using-dbconfiguration"></a>Mithilfe von "dbconfiguration"  

Codebasierte Konfiguration in EF 6 und höher wird erreicht, indem Sie eine Unterklasse von System.Data.Entity.Config.DbConfiguration erstellen. Die folgenden Richtlinien sollten eingehalten werden, wenn Unterklassen aus den "dbconfiguration":  

- Erstellen Sie für Ihre Anwendung nur eine "dbconfiguration"-Klasse. Diese Klasse gibt die Anwendungsdomäne websiteweite Einstellungen.  
- Platzieren Sie die DbConfiguration-Klasse, in der gleichen Assembly wie die Klasse "DbContext". (Finden Sie unter den *verschieben "dbconfiguration"* Abschnitt, wenn Sie dies ändern möchten.)  
- Geben Sie der DbConfiguration-Klasse einen öffentlichen parameterlosen Konstruktor.  
- Optionen für die Konfiguration durch Aufrufen von geschützten "dbconfiguration"-Methoden in diesem Konstruktor festgelegt.  

Befolgen diese Richtlinien können EF, um zu ermitteln und verwenden die Konfiguration automatisch durch beide Tools, die das Modell zugreifen muss und wenn Ihre Anwendung ausgeführt wird.  

## <a name="example"></a>Beispiel  

Eine von "dbconfiguration" abgeleitete Klasse könnte wie folgt aussehen:  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.SqlServer;

namespace MyNamespace
{
    public class MyConfiguration : DbConfiguration
    {
        public MyConfiguration()
        {
            SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
            SetDefaultConnectionFactory(new LocalDbConnectionFactory("mssqllocaldb"));
        }
    }
}
```  

Diese Klasse stellt EF so verwenden die Ausführungsstrategie für SQL Azure - Fehler bei Datenbankvorgängen - automatisch wiederholt und Local DB für Datenbanken verwenden, die gemäß der Konvention von Code First erstellt werden.  

## <a name="moving-dbconfiguration"></a>"Dbconfiguration" verschieben  

Es gibt Fälle, in denen es nicht möglich, die DbConfiguration-Klasse in der gleichen Assembly wie die DbContext-Klasse platziert ist. Angenommen, Sie jedes zwei "DbContext"-Klassen in verschiedenen Assemblys möglicherweise. Es gibt zwei Möglichkeiten zum Durchführen dieser.  

Die erste Option ist die Verwendung die Config-Datei, die zu verwendende "dbconfiguration"-Instanz an. Legen Sie zu diesem Zweck das CodeConfigurationType-Attribut für den Abschnitt "EntityFramework" ein. Zum Beispiel:  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyDbConfiguration, MyAssembly">
    ...Your EF config...
</entityFramework>
```  

Der Wert des CodeConfigurationType muss die Assembly und einen Namespace qualifizierten Namen der Klasse "dbconfiguration" sein.  

Die zweite Option besteht, DbConfigurationTypeAttribute auf Ihr Context-Klasse zu platzieren. Zum Beispiel:  

``` csharp  
[DbConfigurationType(typeof(MyDbConfiguration))]
public class MyContextContext : DbContext
{
}
```  

Der übergebene Wert für das Attribut kann entweder der Typ "dbconfiguration" – Siehe oben - oder die Assembly und den Namespace-qualifizierten Typnamenzeichenfolge. Zum Beispiel:  

``` csharp
[DbConfigurationType("MyNamespace.MyDbConfiguration, MyAssembly")]
public class MyContextContext : DbContext
{
}
```  

## <a name="setting-dbconfiguration-explicitly"></a>"Dbconfiguration" festlegen explizit  

Es gibt einige Situationen, in denen Konfiguration erforderlich sind, möglicherweise um einen beliebigen Typ von "DbContext" verwendet wurde. Beispiele hierfür sind:  

- Verwenden zum Erstellen eines Modells ohne Kontext DbModelBuilder  
- Mithilfe von anderen Framework-Hilfsprogramm Code, der einen "DbContext" verwendet, in diesem Kontext verwendet wird, bevor Kontext Ihrer Anwendung verwendet wird  

In solchen Situationen EF nicht die Konfiguration automatisch zu erkennen ist, und stattdessen müssen Sie eine der folgenden:  

- Legen Sie den "dbconfiguration"-Typ in der Config-Datei an, wie beschrieben in der *verschieben "dbconfiguration"* weiter oben
- Rufen Sie die statische DbConfiguration.SetConfiguration-Methode während des Anwendungsstarts  

## <a name="overriding-dbconfiguration"></a>Überschreiben von "dbconfiguration"  

Es gibt einige Situationen, in dem Sie die Konfiguration, legen Sie in den "dbconfiguration" überschreiben müssen. Dies erfolgt nicht in der Regel von Anwendungsentwicklern, sondern durch Drittanbieter-Anbieter und -Plug-ins, die eine abgeleitete Klasse von "dbconfiguration" nicht verwenden können.  

Hierzu kann EntityFramework einen Ereignishandler registriert werden, der vorhandene Konfiguration ändern können, kurz bevor es gesperrt wird.  Darüber hinaus eine Sugar-Methode speziell für ersetzen alle Dienste, die von der EF-Service-Locator zurückgegeben. Dies ist, wie sie verwendet werden soll:  

- Beim Starten der app (bevor EF verwendet wird) das plug-in oder der Anbieter sollte die Ereignishandlermethode für dieses Ereignis zu registrieren. (Beachten Sie, dass dies geschehen muss, bevor die Anwendung auf EF verwendet.)  
- Der Ereignishandler ruft ReplaceService für jeden Dienst, der ersetzt werden muss.  

Repalce IDbConnectionFactory und DbProviderService würden Sie beispielsweise einen Handler könnte folgendermaßen aussehen registrieren:  

``` csharp
DbConfiguration.Loaded += (_, a) =>
   {
       a.ReplaceService<DbProviderServices>((s, k) => new MyProviderServices(s));
       a.ReplaceService<IDbConnectionFactory>((s, k) => new MyConnectionFactory(s));
   };
```  

Stellen Sie im Code über MyProviderServices und MyConnectionFactory Ihre Implementierungen des Diensts dar.  

Sie können auch zusätzliche Abhängigkeit-Ereignishandler, um dasselbe Ergebnis erreichen Sie hinzufügen.  

Beachten Sie, dass Sie konnte "DbProviderFactory" auch auf diese Weise umschließen, aber dies daher sich nur Entity Framework und der "DbProviderFactory" außerhalb von EF nicht verwendet wirkt. Aus diesem Grund sollten Sie wahrscheinlich "DbProviderFactory" umschließen, wie Sie vor dem fortfahren zu können.  

Sie sollten auch Beachten Sie die Dienste behalten, die Sie extern zu Ihrer Anwendung – z. B. beim Ausführen von Migrationen über die Paket-Manager-Konsole ausführen. Migrieren Sie beim Ausführen über die Konsole mit dem versucht wird, Ihre "dbconfiguration" suchen. Hängt jedoch davon, ob den Wrapper-Dienst abgerufen werden sollen wo der Ereignishandler, die sie registriert. Wenn es im Rahmen der Erstellung des Ihre "dbconfiguration" registriert ist sollte der Code ausgeführt werden, und der Dienst sollte umschlossen zu erhalten. In der Regel wird dies nicht der Fall, und dies bedeutet, dass die Tools den umschlossenen Dienst erhalten.  
