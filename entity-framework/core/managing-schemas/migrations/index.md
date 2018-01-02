---
title: "Migrationen – EF Core"
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
uid: core/managing-schemas/migrations/index
ms.openlocfilehash: 24fbe344eba9b99929d905ac2b9e49c68a1a4323
ms.sourcegitcommit: ced2637bf8cc5964c6daa6c7fcfce501bf9ef6e8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/22/2017
---
<a name="migrations"></a>Migrationen
==========
Migrationen stellen eine Möglichkeit dar, Schemaänderungen schrittweise auf die Datenbank anzuwenden, um diese mit Ihrem EF Core-Modell zu synchronisieren und gleichzeitig vorhandene Daten in der Datenbank zu speichern.

<a name="creating-the-database"></a>Erstellen der Datenbank
---------------------
Nachdem Sie [Ihr Ausgangsmodell definiert][1] haben, können Sie nun die Datenbank erstellen. Fügen Sie hierfür eine erste Migration hinzu.
Installieren Sie [EF Core Tools][2], und führen Sie den entsprechenden Befehl aus.

``` powershell
Add-Migration InitialCreate
```
``` Console
dotnet ef migrations add InitialCreate
```

Drei Dateien werden Ihrem Projekt im Verzeichnis **Migrationen** hinzugefügt:

* **00000000000000_InitialCreate.cs** – Die wichtigste Migrationsdatei. Enthält die Vorgänge, die zum Durchführen der Migration (in `Up()`) und Rückgängigmachen (in `Down()`) erforderlich sind.
* **00000000000000_InitialCreate.Designer.cs** – Die Metadatendatei für die Migration. Enthält Informationen, die vom EF verwendet werden.
* **MyContextModelSnapshot.cs** – Eine Momentaufnahme des aktuellen Modells. Diese wird verwendet, um festzustellen, was sich beim Hinzufügen der nächsten Migration geändert hat.

Mit dem Zeitstempel im Dateinamen können Migrationen chronologisch angeordnet werden, sodass Sie den Fortschritt der Änderungen mitverfolgen können.

> [!TIP]
> Es steht Ihnen frei, Migrationsdateien zu verschieben und ihren Namespace zu ändern. Neue Migrationen werden als gleichgeordnete Elemente der letzten Migration erstellt.

Als Nächstes führen Sie für die Datenbank die Migration durch, um das Schema zu erstellen.

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="adding-another-migration"></a>Hinzufügen einer anderen Migration
------------------------
Nachdem Sie Änderungen an Ihrem EF Core-Modell vorgenommen haben, ist das Datenbankschema nicht mehr synchron. Fügen Sie eine andere Migration hinzu, um es auf dem neuesten Stand zu bringen. Der Migrationsname kann wie eine Commitnachricht in einem Versionskontrollsystem verwendet werden. Wenn zum Beispiel Änderungen vorgenommen wurden, um Kundenbewertungen von Produkten zu speichern, wäre z.B. *AddProductReviews* auswählbar.

``` powershell
Add-Migration AddProductReviews
```
``` Console
dotnet ef migrations add AddProductReviews
```

Sobald das Gerüst für die Migration erstellt wurde, sollten Sie sie auf ihre Genauigkeit überprüfen und alle zusätzlichen Vorgänge hinzufügen, die für die ordnungsgemäße Durchführung erforderlich sind. Beispielsweise könnte Ihre Migration die folgenden Vorgänge umfassen:

``` csharp
migrationBuilder.DropColumn(
    name: "FirstName",
    table: "Customer");

migrationBuilder.DropColumn(
    name: "LastName",
    table: "Customer");

migrationBuilder.AddColumn<string>(
    name: "Name",
    table: "Customer",
    nullable: true);
```

Durch diese Vorgänge wird zwar Kompatibilität mit dem Datenbankschema hergestellt, die bestehenden Kundennamen werden jedoch nicht gespeichert. Um dies zu korrigieren, schreiben Sie es wie folgt neu:

``` csharp
migrationBuilder.AddColumn<string>(
    name: "Name",
    table: "Customer",
    nullable: true);

migrationBuilder.Sql(
@"
    UPDATE Customer
    SET Name = FirstName + ' ' + LastName;
");

migrationBuilder.DropColumn(
    name: "FirstName",
    table: "Customer");

migrationBuilder.DropColumn(
    name: "LastName",
    table: "Customer");
```

> [!TIP]
> Beim Hinzufügen einer neuen Migration wird eine Warnung ausgegeben, wenn ein Gerüst für einen Vorgang erstellt wurde, der zu Datenverlusten führen kann (z.B. beim Löschen einer Spalte). Achten Sie besonders darauf, diese Migrationen auf ihre Genauigkeit hin zu überprüfen.

Führen Sie für die Datenbank die Migration mit dem entsprechenden Befehl durch.

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="removing-a-migration"></a>Entfernen einer Migration
--------------------
Es kann vorkommen, dass Sie eine Migration hinzufügen und feststellen, dass Sie zusätzliche Änderungen an Ihrem EF Core-Modell vornehmen müssen, bevor Sie diese durchführen.
Um die letzte Migration zu entfernen, verwenden Sie diesen Befehl.

``` powershell
Remove-Migration
```
``` Console
dotnet ef migrations remove
```

Nachdem Sie diese entfernt haben, können Sie die zusätzlichen Modelländerungen vornehmen und es erneut hinzufügen.

<a name="reverting-a-migration"></a>Rückgängigmachen einer Migration
---------------------
Wenn Sie bereits eine Migration (oder mehrere Migrationen) für die Datenbank durchgeführt haben, diese jedoch rückgängig machen müssen, können Sie mit demselben Befehl Migrationen durchführen, aber den Namen der Migration angeben, für die Sie ein Rollback ausführen möchten.

``` powershell
Update-Database LastGoodMigration
```
``` Console
dotnet ef database update LastGoodMigration
```

<a name="empty-migrations"></a>Leere Migrationen
----------------
Manchmal ist es sinnvoll, eine Migration hinzuzufügen, ohne Modelländerungen vorzunehmen. In diesem Fall wird durch das Hinzufügen einer neuen Migration eine leere Migration erstellt. Sie können diese Migration anpassen, um Vorgänge durchzuführen, die sich nicht direkt auf das EF Core-Modell beziehen.
Einige Dinge, die Sie vielleicht auf diese Weise handhaben wollen, sind Folgende:

* Volltextsuche
* Funktionen
* Gespeicherte Prozeduren
* Trigger
* Ansichten
* usw.

<a name="generating-a-sql-script"></a>Generieren eines SQL-Skripts
-----------------------
Wenn Sie Ihre Migrationen debuggen oder in einer Produktionsdatenbank bereitstellen, ist es sinnvoll, ein SQL-Skript zu generieren. Das Skript kann dann weiter auf Genauigkeit überprüft und auf die Anforderungen einer Produktionsdatenbank abgestimmt werden. Das Skript kann auch in Verbindung mit einer Bereitstellungstechnologie verwendet werden. Der grundlegende Befehl lautet wie folgt:

``` powershell
Script-Migration
```
``` Console
dotnet ef migrations script
```

Es gibt mehrere Optionen für diesen Befehl.

Die **from**-Migration sollte die letzte Migration sein, die vor der Skriptausführung für die Datenbank durchgeführt wurde. Wenn keine Migrationen durchgeführt wurden, geben Sie `0` an (dies ist die Standardeinstellung).

Die **to**-Migration ist die letzte Migration, die nach der Skriptausführung für die Datenbank durchgeführt wurde. Dies ist standardmäßig die letzte Migration in Ihrem Projekt.

Optional kann ein **idempotent**-Skript generiert werden. Dieses Skript wendet nur Migrationen an, wenn sie nicht bereits für die Datenbank durchgeführt wurden. Dies ist hilfreich, wenn Sie nicht genau wissen, welche Migration für die Datenbank zuletzt durchgeführt wurde oder wenn Bereitstellungen in mehreren Datenbanken, die sich jeweils in unterschiedlichen Migrationen befinden, erfolgen.

<a name="applying-migrations-at-runtime"></a>Durchführen von Migrationen zur Runtime
------------------------------
Einige Apps sollten beim Start oder bei der ersten Ausführung Migrationen zur Runtime durchführen. Dies ist mit der Methode `Migrate()` möglich.

Achten Sie darauf, dass diese Vorgehensweise nicht für alle geeignet ist. Diese eignet sich zwar ideal für Apps mit einer lokalen Datenbank, doch die meisten Anwendungen erfordern zuverlässigere Bereitstellungsstrategien wie etwa durch das Generieren von SQL-Skripts.

``` csharp
myDbContext.Database.Migrate();
```

> [!WARNING]
> Rufen Sie `EnsureCreated()` nicht vor `Migrate()` auf. `EnsureCreated()` umgeht Migrationen zum Erstellen von Schemas und führt dazu, dass `Migrate()` fehlerhaft ist.

> [!NOTE]
> Diese Methode basiert auf dem `IMigrator`-Dienst, der für erweiterte Szenarien verwendet werden kann. Verwenden Sie `DbContext.GetService<IMigrator>()`, um darauf zugreifen.


  [1]: ../../modeling/index.md
  [2]: ../../miscellaneous/cli/index.md
