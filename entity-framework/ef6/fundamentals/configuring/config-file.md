---
title: Dienstkonfigurations-Dateieinstellungen - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 000044c6-1d32-4cf7-ae1f-ea21d86ebf8f
ms.openlocfilehash: 949ad4f030205753c5fbf9b95f4d183d8c0d1fe7
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490871"
---
# <a name="configuration-file-settings"></a>Dienstkonfigurations-Dateieinstellungen
Entitätsframework ermöglicht eine Reihe von Einstellungen aus der Konfigurationsdatei angegeben werden. EF befolgt in der Regel ein "Konvention geht vor Konfiguration"-Prinzip: alle Einstellungen, die in diesem Post erörterten über ein Standardverhalten verfügen, müssen Sie nur Gedanken um die Einstellung ändern, wenn der Standardwert nicht mehr auf Ihre Anforderungen erfüllt.  

## <a name="a-code-based-alternative"></a>Eine Alternative codebasierte  

Alle diese Einstellungen können auch mithilfe von Code angewendet werden. Ab EF6 eingeführt [codebasierte Konfiguration](code-based.md), die eine zentrale Methode zum Anwenden der Konfiguration von Code bereitstellt. Vor EF6 kann Konfiguration über Code angewendet werden, jedoch müssen Sie verschiedene APIs verwenden, um verschiedene Bereiche zu konfigurieren. Die Konfigurationsdatei-Option kann diese Einstellungen während der Bereitstellung einfach geändert werden, ohne Aktualisierung des Codes.

## <a name="the-entity-framework-configuration-section"></a>Der Konfigurationsabschnitt für Entity Framework  

Beginnend mit ef4. 1 konnte legen Sie den Datenbankinitialisierer für einen Kontext mit den **"appSettings"** Abschnitt der Konfigurationsdatei. In EF 4.3 eingeführte wir das benutzerdefinierte **EntityFramework** Abschnitt aus, um die neuen Einstellungen zu behandeln. Entitätsframework erkennt weiterhin Datenbankinitialisierer, die über das alte Format festgelegt, aber wir empfehlen den Umstieg auf das neue Format, wenn möglich.

Die **EntityFramework** Abschnitt wurde in die Konfigurationsdatei des Projekts automatisch hinzugefügt, wenn Sie das EntityFramework NuGet-Paket installiert.  

``` xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <configSections>
    <!-- For more information on Entity Framework configuration, visit http://go.microsoft.com/fwlink/?LinkID=237468 -->
    <section name="entityFramework"
       type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, EntityFramework, Version=4.3.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />
  </configSections>
</configuration>
```  

## <a name="connection-strings"></a>Verbindungszeichenfolgen  

[Auf dieser Seite](~/ef6/fundamentals/configuring/connection-strings.md) finden Sie weitere Details darüber, wie Entity Framework die Datenbank verwendet werden, einschließlich der Verbindungszeichenfolgen in der Konfigurationsdatei.  

Verbindungszeichenfolgen wechseln Sie in der standardmäßigen **ConnectionStrings** Element und erfordern keine der **EntityFramework** Abschnitt.  

Zuerst Codemodelle verwenden normalen ADO.NET-Verbindungszeichenfolgen. Zum Beispiel:  

``` xml
<connectionStrings>
  <add name="BlogContext"  
        providerName="System.Data.SqlClient"  
        connectionString="Server=.\SQLEXPRESS;Database=Blogging;Integrated Security=True;"/>
</connectionStrings>
```  

EF Designer basierte Modelle verwenden spezielle EF-Verbindungszeichenfolgen. Zum Beispiel:  

``` xml  
<connectionStrings>
  <add name="BlogContext"  
    connectionString=
      "metadata=
        res://*/BloggingModel.csdl|
        res://*/BloggingModel.ssdl|
        res://*/BloggingModel.msl;
      provider=System.Data.SqlClient
      provider connection string=
        &quot;data source=(localdb)\mssqllocaldb;
        initial catalog=Blogging;
        integrated security=True;
        multipleactiveresultsets=True;&quot;"
     providerName="System.Data.EntityClient" />
</connectionStrings>
```

## <a name="code-based-configuration-type-ef6-onwards"></a>Typ der codebasierte Konfiguration (EF6 oder höher)  

Ab mit EF6 können Sie angeben, die "dbconfiguration" für Entity Framework für die Verwendung [codebasierte Konfiguration](code-based.md) in Ihrer Anwendung. In den meisten Fällen müssen Sie diese Einstellung angeben, wie EF automatisch Ihre "dbconfiguration" ermittelt. Details der beim müssen Sie möglicherweise "dbconfiguration" in der Config-Datei angeben, finden Sie unter den **verschieben "dbconfiguration"** Abschnitt [codebasierte Konfiguration](code-based.md).  

Um einen Typ "dbconfiguration" festzulegen, geben Sie die Assembly qualifizierte Typname in der **CodeConfigurationType** Element.  

> [!NOTE]
> Ein qualifizierter Name der Assembly ist, einen Namespace gekennzeichneten Namen, gefolgt von einem Komma, klicken Sie dann auf die Assembly, die der Typ befindet. Sie können optional auch angeben, die Assemblyversion, Kultur und Token des öffentlichen Schlüssels.  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyConfiguration, MyAssembly">
</entityFramework>
```  

## <a name="ef-database-providers-ef6-onwards"></a>EF-Datenbankanbieter (EF6 oder höher)  

Vor EF6 Entity Framework-spezifische Teile eines Datenbankanbieters als Bestandteil des ADO.NET-Anbieters Core werden musste. EF6 ab, sind separat die bestimmte Teile von EF jetzt verwaltet und registriert.  

Sie müssen normalerweise nicht Anbieter sich zu registrieren. Dies wird in der Regel vom Anbieter ausgeführt werden, bei der Installation.  

Anbieter werden registriert, dazu einen **Anbieter** Element unter der **Anbieter** Unterabschnitt von der **EntityFramework** Abschnitt. Es gibt zwei obligatorische Attribute für einen Anbietereintrag:  

- **InvariantName** identifiziert den ADO.NET-Anbieter Core, das dieser Ziele der EF-Anbieter  
- **Typ** ist der vollqualifizierte Assemblyname der EF-Anbieter-Implementierung  

> [!NOTE]
> Ein qualifizierter Name der Assembly ist, einen Namespace gekennzeichneten Namen, gefolgt von einem Komma, klicken Sie dann auf die Assembly, die der Typ befindet. Sie können optional auch angeben, die Assemblyversion, Kultur und Token des öffentlichen Schlüssels.  

Als Beispiel hier ist der Eintrag erstellt, um die standardmäßige SQL Server-Ressourcenanbieter registrieren, bei der Installation von Entity Framework.  

``` xml  
<providers>
  <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
</providers>
```  

## <a name="interceptors-ef61-onwards"></a>Interceptors (EF6.1 oder höher)  

Starten Sie mit EF6.1 kann Interceptors in der Konfigurationsdatei registrieren. Interceptors können Sie zusätzlichen Logik ausgeführt werden, wenn EF leistet großartiges und bestimmte Vorgänge wie das Ausführen von Datenbankabfragen, das Sie öffnen, Verbindungen usw.  

Interceptors werden registriert, indem einschließlich ein **Interceptor** Element unter den **Interceptors** Unterabschnitt von der **EntityFramework** Abschnitt. Die folgende Konfiguration registriert z. B. die integrierte **DatabaseLogger** Interceptor, den alle Datenbankvorgänge an der Konsole protokolliert werden.  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework"/>
</interceptors>
```  

### <a name="logging-database-operations-to-a-file-ef61-onwards"></a>Protokollierung von Datenbankvorgängen in einer Datei (EF6.1 oder höher)  

Interceptors über die Config-Datei registrieren ist besonders nützlich, wenn Sie die Protokollierung hinzuzufügen, zu einer vorhandenen Anwendung ein Problem debuggen möchten. **DatabaseLogger** unterstützt die Protokollierung in eine Datei durch Angabe der Dateiname als Konstruktorparameter.  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
    <parameters>
      <parameter value="C:\Temp\LogOutput.txt"/>
    </parameters>
  </interceptor>
</interceptors>
```  

Standardmäßig wird dadurch die Protokolldatei mit einer neuen Datei jedes Mal überschrieben werden, die app gestartet wird. Wenn Sie stattdessen in das Protokoll anfügen Datei bereits vorhanden ist verwenden etwa:  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
    <parameters>
      <parameter value="C:\Temp\LogOutput.txt"/>
      <parameter value="true" type="System.Boolean"/>
    </parameters>
  </interceptor>
</interceptors>
```  

Weitere Informationen zu **DatabaseLogger** und Registrieren von Interceptors finden Sie im Blogbeitrag [EF 6.1: das Aktivieren der Protokollierung ohne erneute Kompilierung](https://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/).  

## <a name="code-first-default-connection-factory"></a>Erste Standardverbindungsfactory Code  

Der Konfigurationsabschnitt können Sie eine standardverbindungsfactory angeben, die Code First verwenden sollten, finden Sie eine Datenbank, die für einen Kontext verwendet. Die standardverbindungsfactory wird nur verwendet, wenn keine Verbindungszeichenfolge in die Konfigurationsdatei für einen Kontext hinzugefügt wurde.  

Bei der Installation von EF-NuGet-Paket wurde eine standardverbindungsfactory registriert, der auf SQL Express oder LocalDB, je nachdem, welche Schlüssel Sie installiert haben verweist.  

Um ein verbindungsfactory festzulegen, geben Sie die Assembly qualifizierte Typname in der **DeafultConnectionFactory** Element.  

> [!NOTE]
> Ein qualifizierter Name der Assembly ist, einen Namespace gekennzeichneten Namen, gefolgt von einem Komma, klicken Sie dann auf die Assembly, die der Typ befindet. Sie können optional auch angeben, die Assemblyversion, Kultur und Token des öffentlichen Schlüssels.  

Hier ist ein Beispiel für Ihre eigenen standardverbindungsfactory festlegen:  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="MyNamespace.MyCustomFactory, MyAssembly"/>
</entityFramework>
```  

Das obige Beispiel erfordert die benutzerdefinierte Factory, einen parameterlosen Konstruktor verfügt. Bei Bedarf können Sie angeben, dass Konstruktorparameter, die mit der **Parameter** Element.  

Beispielsweise erfordert die SqlCeConnectionFactory, die in Entity Framework enthalten ist, aufgefordert, einen invarianten Anbieternamen an den Konstruktor. Der invariante Anbietername identifiziert die Version von SQL Compact, die Sie verwenden möchten. Die folgende Konfiguration führt dazu, dass Kontexte SQL Compact Version 4.0 wird standardmäßig verwendet.  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlCeConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="System.Data.SqlServerCe.4.0" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

Wenn Sie nicht, dass eine standardmäßige verbindungsfactory festlegen, Code First verwendet die SqlConnectionFactory, die auf `.\SQLEXPRESS`. SqlConnectionFactory verfügt auch über einen Konstruktor, der Ihnen ermöglicht, die Teile der Verbindungszeichenfolge zu überschreiben. Wenn Sie eine SQL Server-Instanz nicht verwenden möchten `.\SQLEXPRESS` verwenden Sie diesen Konstruktor, um den Server festzulegen.  

Die folgende Konfiguration führt dazu, dass Code First mit **Datenbankserver** für Kontexte, die eine explizite Verbindungszeichenfolge festgelegt haben.  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="Data Source=MyDatabaseServer; Integrated Security=True; MultipleActiveResultSets=True" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

Standardmäßig wird davon ausgegangen, dass der Konstruktorargumente vom Typzeichenfolge sind. Sie können das Type-Attribut verwenden, um dies zu ändern.  

``` xml
<parameter value="2" type="System.Int32" />
```  

## <a name="database-initializers"></a>Datenbankinitialisierer  

Datenbankinitialisierer werden auf einer Basis pro Kontext konfiguriert. Sie können festgelegt werden, in der Konfiguration mithilfe der **Kontext** Element. Dieses Element verwendet die Assembly qualifizierter Name zum Identifizieren des Kontexts, der konfiguriert wird.  

Standardmäßig sind Code First-Kontexten zur Verwendung von "createDatabaseIfNotExists" Initialisierer konfiguriert. Gibt es eine **DisableDatabaseInitialization** -Attribut für die **Kontext** -Element, das zum Deaktivieren der Initialisierung der Datenbank verwendet werden kann.  

Die folgende Konfiguration deaktiviert z. B. die Initialisierung der Datenbank für den Blogging.BlogContext-Kontext, der in "MeineAssembly.dll" definiert.  

``` xml  
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly" disableDatabaseInitialization="true" />
</contexts>
```  

Sie können die **DatabaseInitializer** Element, das einen benutzerdefinierten Initialisierer festgelegt.  

``` xml
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="Blogging.MyCustomBlogInitializer, MyAssembly" />
  </context>
</contexts>
```  

Konstruktorparameter verwenden dieselbe Syntax wie standardmäßige Verbindung Factorys.  

``` xml  
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="Blogging.MyCustomBlogInitializer, MyAssembly">
      <parameters>
        <parameter value="MyConstructorParameter" />
      </parameters>
    </databaseInitializer>
  </context>
</contexts>
```  

Sie können eines der allgemeinen Datenbank-Initialisierer konfigurieren, die in Entity Framework enthalten sind. Die **Typ** Attribut verwendet das .NET Framework-Format für generische Typen.  

Z. B. Wenn Sie Code First-Migrationen verwenden, können Sie die Datenbank konfigurieren, die migriert werden automatisch mit der `MigrateDatabaseToLatestVersion<TContext, TMigrationsConfiguration>` Initialisierer.  

``` xml
<contexts>
  <context type="Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="System.Data.Entity.MigrateDatabaseToLatestVersion`2[[Blogging.BlogContext, MyAssembly], [Blogging.Migrations.Configuration, MyAssembly]], EntityFramework" />
  </context>
</contexts>
```
