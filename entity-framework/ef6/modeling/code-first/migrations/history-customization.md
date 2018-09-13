---
title: Anpassen der Verlaufstabelle Migrationen – EF 6
author: divega
ms.date: 10/23/2016
ms.assetid: ed5518f0-a9a6-454e-9e98-a4fa7748c8d0
ms.openlocfilehash: 6644bf2b0ac703a9f3a779b17b31d79d40cc5b69
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489207"
---
# <a name="customizing-the-migrations-history-table"></a>Anpassen der Verlaufstabelle Migrationen
> [!NOTE]
> **Nur EF6 und höher:** Die Features, APIs usw., die auf dieser Seite erläutert werden, wurden in Entity Framework 6 eingeführt. Wenn Sie eine frühere Version verwenden, gelten manche Informationen nicht.

> [!NOTE]
> In diesem Artikel wird davon ausgegangen, dass Sie wissen, wie Code First-Migrationen in grundlegenden Szenarien verwenden. Wenn Sie dies nicht tun, müssen Sie lesen [Code First-Migrationen](~/ef6/modeling/code-first/migrations/index.md) vor dem fortfahren.

## <a name="what-is-migrations-history-table"></a>Was ist Migrations History Table?

Migrationen Verlaufstabelle ist eine Tabelle, die durch Code First-Migrationen verwendet werden, um Details zu Migrationen angewendet werden, in der Datenbank speichern. Standardmäßig ist der Name der Tabelle in der Datenbank \_ \_MigrationHistory und es wird erstellt, wenn die erste Migration führen Sie die Datenbank anwenden. In Entity Framework 5 wurde in dieser Tabelle eine Systemtabelle aus, wenn die Anwendung Microsoft Sql Server-Datenbank verwendet. Dies wurde geändert in Entity Framework 6 jedoch, die Migrationen Verlaufstabelle wird nicht mehr eine Systemtabelle markiert.

## <a name="why-customize-migrations-history-table"></a>Warum sollte werden Migrations History Table angepasst?

Migrationen Verlaufstabelle ausschließlich von Code First-Migrationen verwendet werden soll, und es manuell zu ändern, kann Migrationen unterbrechen. Jedoch manchmal die Standardkonfiguration ist nicht geeignet, und die Tabelle muss, z. B. angepasst werden:

-   So ändern Sie Namen und/oder Facets von Spalten, um eine 3 aktivieren benötigten<sup>rd</sup> Migrationen-Anbieters
-   Der Name der Tabelle geändert werden soll
-   Sie benötigen ein nicht dem Standard-Schema für die \_ \_Tabelle "MigrationHistory"
-   Sie müssen zusätzliche Daten für eine bestimmte Version des Kontexts zu speichern und aus diesem Grund müssen Sie eine zusätzliche Spalte zur Tabelle hinzufügen

## <a name="words-of-precaution"></a>Wörter vorsorglich

Ändern der migrationsverlaufstabelle ist leistungsstark, aber Sie müssen darauf achten, nicht übertreiben. EF Runtime überprüft, ob die angepasste Migrationen-Verlaufstabelle mit der Laufzeit kompatibel ist derzeit nicht. Wenn es sich nicht um Ihre Anwendung ist möglicherweise zur Laufzeit unterbrechen oder auf unvorhersehbare Weise Verhalten. Dies ist sogar noch wichtiger, wenn Sie mehrere Kontexte pro Datenbank verwenden in diesem Fall mehrere Kontexte der gleichen migrationsverlaufstabelle verwenden können zum Speichern von Informationen zu Migrationen.

## <a name="how-to-customize-migrations-history-table"></a>Wie Migrations History Table angepasst?

Bevor Sie beginnen, müssen Sie wissen, dass Sie die Verlaufstabelle Migrationen anpassen können, nur verwendet werden, bevor Sie die erste Migration anwenden. Jetzt, um den Code.

Zunächst müssen Sie zum Erstellen einer Klasse, die von System.Data.Entity.Migrations.History.HistoryContext-Klasse abgeleitet. Die Klasse "historycontext" wird von der Klasse "DbContext" abgeleitet werden, so konfigurieren die Verlaufstabelle Migrationen zum Konfigurieren von EF-Modellen mit fluent-API sehr ähnlich ist. Sie müssen nur zum Überschreiben der OnModelCreating-Methode aus, und Verwenden von fluent-API so konfigurieren Sie in der Tabelle.

>[!NOTE]
> Beim Konfigurieren der EF-Modellen müssen Sie in der Regel keine Basisklasse aufrufen. OnModelCreating() aus der außer Kraft gesetzte OnModelCreating-Methode, da die DbContext.OnModelCreating() weist einen leeren Text. Dies ist nicht der Fall, wenn Sie die Verlaufstabelle Migrationen zu konfigurieren. In diesem Fall die erste ist in der Überschreibung OnModelCreating() tatsächlich Basisklasse aufrufen. OnModelCreating(). Dadurch wird die Verlaufstabelle Migrationen in die Standardmethode für konfiguriert, die Sie, klicken Sie dann in der überschreibenden Methode anpassen.

Angenommen, Sie verwenden möchten, benennen Sie die Verlaufstabelle Migrationen und platzieren Sie ihn in ein benutzerdefiniertes Schema namens "Admin". Darüber hinaus Ihr Datenbankadministrator möchten Sie die MigrationId-Spalte umbenennen, auf die Migration zu\_-ID.  Sie können dies erreichen, erstellen Sie die folgende Klasse, die von "historycontext" abgeleitet:

``` csharp
    using System.Data.Common;
    using System.Data.Entity;
    using System.Data.Entity.Migrations.History;

    namespace CustomizableMigrationsHistoryTableSample
    {
        public class MyHistoryContext : HistoryContext
        {
            public MyHistoryContext(DbConnection dbConnection, string defaultSchema)
                : base(dbConnection, defaultSchema)
            {
            }

            protected override void OnModelCreating(DbModelBuilder modelBuilder)
            {
                base.OnModelCreating(modelBuilder);
                modelBuilder.Entity<HistoryRow>().ToTable(tableName: "MigrationHistory", schemaName: "admin");
                modelBuilder.Entity<HistoryRow>().Property(p => p.MigrationId).HasColumnName("Migration_ID");
            }
        }
    }
```

Sobald Ihr benutzerdefinierte "historycontext" bereit ist, müssen Sie EF davon aufmerksam zu machen, durch die Registrierung über [codebasierte Konfiguration](http://msdn.com/data/jj680699):

``` csharp
    using System.Data.Entity;

    namespace CustomizableMigrationsHistoryTableSample
    {
        public class ModelConfiguration : DbConfiguration
        {
            public ModelConfiguration()
            {
                this.SetHistoryContext("System.Data.SqlClient",
                    (connection, defaultSchema) => new MyHistoryContext(connection, defaultSchema));
            }
        }
    }
```

Das ist ziemlich alles. Nachdem Sie auf der Paket-Manager-Konsole, Enable-Migrations, zugreifen können Add-Migration und schließlich die Datenbank aktualisieren. Daraufhin sollte in der Datenbank, die eine Verlaufstabelle Migrationen entsprechend den Details der konfiguriert wird, die Sie angegeben haben, in der "historycontext" abgeleitete Klasse hinzufügen.

![Datenbank](~/ef6/media/database.png)
