---
title: Verbindungszeichenfolgen und -Modelle – EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 294bb138-978f-4fe2-8491-fdf3cd3c60c4
caps.latest.revision: 3
ms.openlocfilehash: ca597e68a5b3e2085612669ee81da10ba6969eeb
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2018
ms.locfileid: "39121469"
---
# <a name="connection-strings-and-models"></a>Verbindungszeichenfolgen und Modelle
Dieses Thema behandelt wie Entity Framework die, die zu verwendende datenbankverbindung ermittelt und wie Sie sie ändern können. Mit Code First und dem EF Designer erstellte Modelle, die beide in diesem Thema behandelt werden.  

Eine Entity Framework-Anwendung verwendet in der Regel eine von DbContext abgeleitete Klasse. Diese abgeleitete Klasse ruft einer der Konstruktoren für die DbContext-Basisklasse, um zu steuern:  

- Wie der Kontext mit einer Datenbank hergestellt werden – gefunden "/" verwendet, also wie eine Verbindungszeichenfolge ist  
- Unabhängig davon, ob der Kontext wird Berechnen eines Modells mit Code First oder ein Modell erstellt, mit dem EF Designer laden  
- Zusätzliche erweiterte Optionen  

Die folgenden Fragmente zeigen, dass einige der Möglichkeiten der "DbContext"-Konstruktor verwendet werden kann.  

## <a name="use-code-first-with-connection-by-convention"></a>Code First mit Verbindung gemäß der Konvention verwenden  

Wenn Sie eine beliebige andere Konfiguration in Ihrer Anwendung noch nicht getan haben, verursacht die rufen Sie dann des parameterlosen Konstruktors, klicken Sie auf "DbContext", dass "DbContext" mit einer Verbindung mit Datenbank gemäß der Konvention erstellt im Code-First-Modus ausgeführt. Zum Beispiel:  

``` csharp  
namespace Demo.EF
{
    public class BloggingContext : DbContext
    {
        public BloggingContext()
        // C# will call base class parameterless constructor by default
        {
        }
    }
}
```  

In diesem Beispiel "DbContext" verwendet einen Namespace gekennzeichneten Namen von Ihrem abgeleiteten Kontext class—Demo.EF.BloggingContext—as der Name der Datenbank und erstellt eine Verbindungszeichenfolge für diese Datenbank mit SQL Express oder LocalDB. Wenn beide installiert werden, wird SQL Express verwendet werden.  

Visual Studio 2010 enthält SQL Express, indem Sie Standard- und Visual Studio 2012 und höher umfasst LocalDB. Während der Installation überprüft EntityFramework NuGet-Paket an, welche Datenbankserver verfügbar ist. Das NuGet-Paket aktualisiert dann die Konfigurationsdatei durch Festlegen der Standarddatenbankserver, die Code First verwendet, wenn Sie eine Verbindung gemäß der Konvention zu erstellen. Wenn Sie SQL Express ausgeführt wird, wird er verwendet werden. Wenn Sie SQL Express nicht verfügbar ist wird LocalDB stattdessen als Standard registriert. In der Konfigurationsdatei werden keine Änderungen vorgenommen, wenn sie bereits über eine Einstellung für die standardverbindungsfactory enthält.  

## <a name="use-code-first-with-connection-by-convention-and-specified-database-name"></a>Verwenden Sie Code First mit einer Verbindung von der Aufrufkonvention und der angegebene Datenbankname  

Wenn Sie eine beliebige andere Konfiguration in Ihrer Anwendung noch nicht getan haben, bewirkt rufen Sie dann die String-Konstruktors, auf "DbContext", mit dem Datenbanknamen, die, den Sie verwenden möchten "DbContext" im Code-First-Modus ausgeführt, mit einer Verbindung mit Datenbank erstellt, die gemäß der Konvention, mit der Datenbank des, dass Dieser Name. Zum Beispiel:  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingDatabase")
    {
    }
}
```  

In diesem Beispiel "DbContext" verwendet "BloggingDatabase" als Namen der Datenbank und erstellt eine Verbindungszeichenfolge für diese Datenbank mit SQL Express (installiert mit Visual Studio 2010) oder LocalDB (installiert mit Visual Studio 2012). Wenn beide installiert werden, wird SQL Express verwendet werden.  

## <a name="use-code-first-with-connection-string-in-appconfigwebconfig-file"></a>Verwenden Sie Code First, mit der Verbindungszeichenfolge in app.config/web.config-Datei  

Sie können auswählen, um eine Verbindungszeichenfolge in der Datei "App.config" oder "Web.config" einfügen. Zum Beispiel:  

``` xml  
<configuration>
  <connectionStrings>
    <add name="BloggingCompactDatabase"
         providerName="System.Data.SqlServerCe.4.0"
         connectionString="Data Source=Blogging.sdf"/>
  </connectionStrings>
</configuration>
```  

Dies ist eine einfache Möglichkeit zum Teilen von "DbContext" einen Datenbankserver als SQL Express oder LocalDB verwenden – das obige Beispiel gibt eine SQL Server Compact Edition-Datenbank an.  

Wenn der Name der Verbindungszeichenfolge den Namen Ihres Kontexts (mit oder ohne namespacenennung) übereinstimmt wird dann es von "DbContext" gefunden bei der parameterlose Konstruktor verwendet wird. Wenn der Name der Verbindungszeichenfolge den Namen Ihres Kontexts unterscheidet, können Sie "DbContext", um diese Verbindung in Code First-Modus zu verwenden, indem der Name der Verbindungszeichenfolge an den "DbContext"-Konstruktor übergeben feststellen. Zum Beispiel:  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingCompactDatabase")
    {
    }
}
```  

Alternativ können Sie die Form "Name =\<Verbindungszeichenfolgenname\>" für die Zeichenfolge, die an den "DbContext"-Konstruktor übergeben. Zum Beispiel:  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("name=BloggingCompactDatabase")
    {
    }
}
```  

Dieses Formular vereinfacht die explizite Sie erwarten, dass die Verbindungszeichenfolge in der Config-Datei gefunden werden. Wenn eine Verbindungszeichenfolge mit dem angegebenen Namen nicht gefunden wird, wird eine Ausnahme ausgelöst werden.  

## <a name="databasemodel-first-with-connection-string-in-appconfigwebconfig-file"></a>Datenbankmodell/erste Verbindungszeichenfolge in app.config/web.config-Datei  

Mit dem EF Designer erstellte Modelle unterscheiden sich von Code First, darin, dass Ihr Modell bereits vorhanden und nicht aus Code generiert wird, wenn die Anwendung ausgeführt wird. Das Modell ist in der Regel als eine EDMX-Datei in Ihrem Projekt vorhanden.  

Der Designer Fügt eine EF-Verbindungszeichenfolge zu Ihrer Datei "App.config" oder "Web.config". Diese Verbindungszeichenfolge Sonderregeln darin, dass es sich um Informationen zur Verwendung finden Sie die Informationen in der EDMX-Datei enthält. Zum Beispiel:  

``` xml  
<configuration>  
  <connectionStrings>  
    <add name="Northwind_Entities"  
         connectionString="metadata=res://*/Northwind.csdl|  
                                    res://*/Northwind.ssdl|  
                                    res://*/Northwind.msl;  
                           provider=System.Data.SqlClient;  
                           provider connection string=  
                               &quot;Data Source=.\sqlexpress;  
                                     Initial Catalog=Northwind;  
                                     Integrated Security=True;  
                                     MultipleActiveResultSets=True&quot;"  
         providerName="System.Data.EntityClient"/>  
  </connectionStrings>  
</configuration>
```  

Die EF-Designer generiert außerdem Code, der angibt, "DbContext", um diese Verbindung verwendet werden, indem der Name der Verbindungszeichenfolge an die "DbContext"-Konstruktor übergeben. Zum Beispiel:  

``` csharp  
public class NorthwindContext : DbContext
{
    public NorthwindContext()
        : base("name=Northwind_Entities")
    {
    }
}
```  

DbContext weiß, um das vorhandene Modell (statt in Code berechnet mithilfe von Code First) zu laden, da die Verbindungszeichenfolge einer EF-Verbindungszeichenfolge, enthält die Details des Modells verwendet wird.  

## <a name="other-dbcontext-constructor-options"></a>Andere Optionen für "DbContext"-Konstruktor  

Die DbContext-Klasse enthält andere Konstruktoren und die Verwendungsmuster, die einige erweiterte Szenarios zu ermöglichen. Einige davon sind:  

- Sie können die DbModelBuilder-Klasse verwenden, ein Code First-Modell zu erstellen, ohne einen "DbContext"-Instanz instanziieren. Das Ergebnis davon ist ein DbModel-Objekt. Sie können dieses DbModel-Objekt klicken Sie dann auf eine der "DbContext"-Konstruktoren übergeben, wenn Sie bereit sind, Ihr "DbContext"-Instanz zu erstellen.  
- Sie können eine vollständige Verbindungszeichenfolge auf "DbContext" statt nur für die Verwendung der Datenbank oder die Verbindung Zeichenfolgenname übergeben. Standardmäßig wird diese Verbindungszeichenfolge mit den Anbieter "System.Data.SqlClient" verwendet. Dies kann geändert werden, indem Sie eine andere Implementierung der IConnectionFactory auf Kontext festlegen. Database.DefaultConnectionFactory.  
- Sie können ein vorhandenes DbConnection-Objekt durch Übergabe an einen "DbContext"-Konstruktor verwenden. Wenn das Verbindungsobjekt eine Instanz des EntityConnection-Objekt, und klicken Sie dann das Modell in der Verbindung angegebene werden verwendet, anstatt die Berechnung eines Modells mit Code First. Wenn das Objekt eine Instanz eines anderen Typs ist – z. B. SqlConnection, der Kontext für Code First-Modus verwendet.  
- Sie können einen vorhandenen ObjectContext an einen "DbContext"-Konstruktor zum Erstellen eines "DbContext" umschließen die vorhandene Kontextfilter übergeben. Dies kann verwendet werden für vorhandene Anwendungen, die ObjectContext verwenden, aber die "DbContext" in einige Teile der Anwendung nutzen möchten.  
