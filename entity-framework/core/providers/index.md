---
title: Datenbankanbieter – EF Core
author: rowanmiller
ms.author: divega
ms.date: 2/23/2018
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
ms.technology: entity-framework-core
uid: core/providers/index
ms.openlocfilehash: d7313451f324a5e26ae327478996861e31364e7d
ms.sourcegitcommit: 89edee21606083d01154766e0c4249cec38957f7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2018
---
# <a name="database-providers"></a>Datenbankanbieter

Entity Framework Core kann viele verschiedene Datenbanken über Plug-in-Bibliotheken erreichen, die als „Datenbankanbieter“ bezeichnet werden.

## <a name="current-providers"></a>Aktuelle Anbieter
> [!IMPORTANT]  
> EF Core-Anbieter werden durch eine Vielzahl von Quellen erstellt. Nicht alle Anbieter werden im Rahmen des Entity [Entity Framework Core-Projekts](https://github.com/aspnet/EntityFrameworkCore) verwaltet. Wenn Sie einen Anbieter in Betracht ziehen, sollten Sie Aspekte wie Qualität, Lizenzierung und Support auswerten, um sicherzustellen, dass dieser Ihren Anforderungen entspricht. Lesen Sie außerdem in jedem Fall die ausführlichen Informationen zur Versionskompatibilität in der Dokumentation der einzelnen Anbieter.

| NuGet-Paket                                                                                                        | Unterstützte Datenbank-Engines | Maintainer/Anbieter                                                           | Hinweise/Anforderungen             | Nützliche Links                                                                                                                                                                                       |
|:---------------------------------------------------------------------------------------------------------------------|:---------------------------|:------------------------------------------------------------------------------|:---------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer)    | SQL Server 2008 oder höher    | [EF Core-Projekt](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) |                                  | [docs](xref:core/providers/sql-server/index)                                                                                                                                                       |
| [Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite)          | SQLite 3.7 oder höher         | [EF Core-Projekt](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) |                                  | [docs](xref:core/providers/sqlite/index)                                                                                                                                                           |
| [Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory)      | EF Core-In-Memory-Datenbank | [EF Core-Projekt](https://github.com/aspnet/EntityFrameworkCore/) (Microsoft) | Nur für Tests                 | [docs](xref:core/providers/in-memory/index)                                                                                                                                                        |
| [Npgsql.EntityFrameworkCore.PostgreSQL](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer)      | PostgreSQL                 | [Npgsql-Entwicklungsteam](https://github.com/npgsql)                          |                                  | [docs](http://www.npgsql.org/efcore/index.html)                                                                                                                                                    |
| [Pomelo.EntityFrameworkCore.MySql](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MySql)                  | MySQL, MariaDB             | [Pomelo Foundation-Projekt](https://github.com/PomeloFoundation)              |                                  | [readme](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql/blob/master/README.md)                                                                                               |
| [Pomelo.EntityFrameworkCore.MyCat](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MyCat)                  | MyCAT-Server               | [Pomelo Foundation-Projekt](https://github.com/PomeloFoundation)              | Vorabrelease, bis EF Core 1.1   | [readme](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MyCat/blob/master/README.md)                                                                                               |
| [EntityFrameworkCore.SqlServerCompact40](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact40)      | SQL Server Compact 4.0     | [Erik Ejlskov Jensen](https://github.com/ErikEJ/)                             | .NET Framework                   | [wiki](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)                                                     |
| [EntityFrameworkCore.SqlServerCompact35](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact35)      | SQL Server Compact 3,5     | [Erik Ejlskov Jensen](https://github.com/ErikEJ/)                             | .NET Framework                   | [wiki](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)                                                     |
| [MySql.Data.EntityFrameworkCore](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore)                      | MySQL                      | [MySQL-Projekt](http://dev.mysql.com) (Oracle)                                | Vorabrelease                      | [docs](https://dev.mysql.com/doc/connector-net/en/)                                                                                                                                                |
| [FirebirdSql.EntityFrameworkCore.Firebird](https://www.nuget.org/packages/FirebirdSql.EntityFrameworkCore.Firebird/) | Firebird 2.5 und 3.x       | [Jiří Činčura](https://github.com/cincuranet)                                 | EF Core 2.0 oder höher, Vorabrelease | [Blog](https://www.tabsoverspaces.com/233653-preview-of-entity-framework-core-2-0-support-for-firebird-and-firebirdclient-6-0/)                                                                    |
| [EntityFrameworkCore.FirebirdSql](https://www.nuget.org/packages/EntityFrameworkCore.FirebirdSql/)                   | Firebird 2.5 und 3.x       | [Rafael Almeida](https://github.com/ralmsdeveloper)                           | EF Core 2.0 oder höher              | [wiki](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL/wiki)                                                                                                                     |
| [IBM.EntityFrameworkCore](https://www.nuget.org/packages/IBM.EntityFrameworkCore)                                    | DB2, Informix              | [IBM](https://ibm.com)                                                        | Windows-Version                  | [Blog](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Creating_Entity_Data_Model_using_IBM_Data_Server_providers_for_Entity_Framework_Core?lang=en) |
| [IBM.EntityFrameworkCore-lnx](https://www.nuget.org/packages/IBM.EntityFrameworkCore-lnx)                            | DB2, Informix              | [IBM](https://ibm.com)                                                        | Linux-Version                    | [Blog](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Creating_Entity_Data_Model_using_IBM_Data_Server_providers_for_Entity_Framework_Core?lang=en) |
| [IBM.EntityFrameworkCore-osx](https://www.nuget.org/packages/IBM.EntityFrameworkCore-osx)                            | DB2, Informix              | [IBM](https://ibm.com)                                                        | macOS-Version                    | [Blog](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/Creating_Entity_Data_Model_using_IBM_Data_Server_providers_for_Entity_Framework_Core?lang=en) |
| [Devart.Data.Oracle.EFCore](https://www.nuget.org/packages/Devart.Data.Oracle.EFCore/)                               | Oracle 9.2.0.4 oder höher     | [DevArt](https://www.devart.com/)                                             | Bezahlt                             | [docs](https://www.devart.com/dotconnect/oracle/docs/)                                                                                                                                             |
| [Devart.Data.PostgreSql.EFCore](https://www.nuget.org/packages/Devart.Data.PostgreSql.EFCore/)                       | PostgreSQL 8.0 oder höher     | [DevArt](https://www.devart.com/)                                             | Bezahlt                             | [docs](https://www.devart.com/dotconnect/postgresql/docs/)                                                                                                                                         |
| [Devart.Data.SQLite.EFCore](https://www.nuget.org/packages/Devart.Data.SQLite.EFCore/)                               | SQLite 3 oder höher           | [DevArt](https://www.devart.com/)                                             | Bezahlt                             | [docs](https://www.devart.com/dotconnect/sqlite/docs/)                                                                                                                                             |
| [Devart.Data.MySql.EFCore](https://www.nuget.org/packages/Devart.Data.MySql.EFCore/)                                 | MySQL 5 oder höher            | [DevArt](https://www.devart.com/)                                             | Bezahlt                             | [docs](https://www.devart.com/dotconnect/mysql/docs/)                                                                                                                                              |
| [EntityFrameworkCore.Jet](https://www.nuget.org/packages/EntityFrameworkCore.Jet/)                                   | Microsoft Access-Dateien     | [Bubi](https://github.com/bubibubi)                                           | EF Core 2.0, .NET Framework      | [readme](https://github.com/bubibubi/EntityFrameworkCore.Jet/blob/master/docs/README.md)                                                                                                           |

## <a name="future-providers"></a>Zukünftige Anbieter

### <a name="cosmos-db"></a>Cosmos DB

Wir haben einen Core EF-Anbieter für die DocumentDB-API in Cosmos DB entwickelt. Dies ist der erste vollständige dokumentorientierte Datenbankanbieter, den wir erstellt haben, und die Erkenntnisse aus dieser Übung werden Bestandteil der Verbesserungen im Entwurf des nachfolgenden Release nach 2.1 sein. Der aktuelle Plan besteht darin, eine vorzeitige Vorschauversion des Anbieters im Zeitraum 2.1 zu veröffentlichen.

### <a name="oracle"></a>Oracle
Das Oracle .NET-Team hat angekündigt, dass voraussichtlich ungefähr im dritten Quartal von 2018 einen Erstanbieter für EF Core 2.0 veröffentlichen wird. Weitere Informationen finden Sie in der [Absichtserklärung für .NET Core und Entity Framework Core](http://www.oracle.com/technetwork/topics/dotnet/tech-info/odpnet-dotnet-ef-core-sod-4395108.pdf).
Bei Fragen zu diesem Anbieter, einschließlich der Releasezeitachse, besuchen Sie sich die [Website der Oracle-Community](https://community.oracle.com/).

In der Zwischenzeit hat das EF-Team einen [EF Core-Beispielanbieter für Oracle-Datenbanken](https://github.com/aspnet/EntityFrameworkCore/blob/dev/samples/OracleProvider/README.md) erzeugt. Der Zweck des Projekts besteht nicht darin, einen im Besitz von Microsoft befindlichen EF Core-Anbieter zu erzeugen, sondern Lücken in den relationalen und Basisfunktionen von EF Core aufzudecken, die wir zur besseren Unterstützung für Oracle behandeln müssen, und die Entwicklung von anderen Oracle-Anbietern für EF Core durch Oracle oder durch Drittanbieter zu beschleunigen.

Wir werden Beiträge berücksichtigen, die die Beispielimplementierung verbessern. Auch würden wir es begrüßen und unterstützen, wenn eine Communityinitiative zum Erstellen eines Open Source-basierten Oracle-Anbieters für EF Core mit dem Beispiel als Ausgangspunkt zustande käme.

## <a name="adding-a-database-provider-to-your-application"></a>Hinzufügen eines Datenbankanbieters zu Ihrer Anwendung

Die meisten Datenbankanbieter für EF Core werden als NuGet-Pakete verteilt. Dies bedeutet, dass sie mithilfe des `dotnet`-Tools in der Befehlszeile installiert werden können:

``` console
dotnet add package provider_package_name
```

Alternativ können Sie die NuGet-Paket-Manager-Konsole in Visual Studio verwenden:

``` powershell
install-package provider_package_name
```

Nach Abschluss der Installation konfigurieren Sie den Anbieter in `DbContext` entweder in der `OnConfiguring`-Methode oder in der `AddDbContext`-Methode, wenn Sie einen Abhängigkeitsinjektionscontainer verwenden. Die folgende Zeile konfiguriert z.B. den SQL Server-Anbieter mit der übergebenen Verbindungszeichenfolge:

``` csharp
optionsBuilder.UseSqlServer(
    "Server=(localdb)\mssqllocaldb;Database=MyDatabase;Trusted_Connection=True;");
```  

Datenbankanbieter können EF Core erweitern, um datenbankspezifische Funktionen zu aktivieren. Einige Konzepte gelten für die meisten Datenbanken und sind im Leistungsumfang der primären EF Core-Komponenten inbegriffen. Zu diesen Konzepten gehören das Ausdrücken von Abfragen in LINQ, Transaktionen und das Nachverfolgen von Änderungen an Objekten, nachdem sie aus der Datenbank geladen wurden. Einige Konzepte gelten speziell für einen bestimmten Anbieter. Der SQL Server-Anbieter ermöglicht es Ihnen beispielsweise, [speicheroptimierte Tabellen (ein für SQL Server spezifisches Feature) zu konfigurieren](xref:core/providers/sql-server/memory-optimized-tables). Andere Konzepte gelten speziell für eine Klasse von Anbietern. Beispielsweise bauen EF Core-Anbieter für relationale Datenbanken auf der gemeinsamen `Microsoft.EntityFrameworkCore.Relational`-Bibliothek auf, die u.a. APIs für die Konfiguration von Tabellen- und Spaltenzuordnungen und Fremdschlüsseleinschränkungen bereitstellt. Anbieter werden in der Regel als NuGet-Pakete verteilt.

> [!IMPORTANT]  
> Eine neue veröffentlichte Patchversion von EF Core enthält häufig Updates für das `Microsoft.EntityFrameworkCore.Relational`-Paket. Wenn Sie einen relationale Datenbankanbieter hinzufügen, wird dieses Paket zu einer transitiven Abhängigkeit Ihrer Anwendung. Viele Anbieter werden jedoch unabhängig von EF Core veröffentlicht und können nicht dahingehend aktualisiert werden, dass sie von der neueren Patchversion dieses Pakets abhängen. Um sicherzustellen, dass Sie von allen Fehlerbehebungen profitieren, wird empfohlen, die Patchversion von `Microsoft.EntityFrameworkCore.Relational` als direkte Abhängigkeit von Ihrer Anwendung hinzuzufügen.
