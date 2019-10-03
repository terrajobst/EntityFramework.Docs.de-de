---
title: 'Erstellen und Löschen von APIs: EF Core'
author: bricelam
ms.author: bricelam
ms.date: 11/07/2018
ms.openlocfilehash: 88c1403d2fae740ad78bb7c41d404b0dd91e86ae
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813439"
---
# <a name="create-and-drop-apis"></a>Erstellen und Löschen von APIs

Die ensuneu-und ensuredeleted-Methoden stellen eine leichte Alternative zu [Migrationen](migrations/index.md) zum Verwalten des Datenbankschemas dar. Diese Methoden sind in Szenarios nützlich, in denen die Daten vorübergehend sind und gelöscht werden können, wenn sich das Schema ändert. Beispielsweise während der Prototyperstellung in Tests oder für lokale Caches.

Einige Anbieter (insbesondere nicht relationale) unterstützen keine Migrationen. Für diese Anbieter ist "ensuneu" oft die einfachste Möglichkeit, das Datenbankschema zu initialisieren.

> [!WARNING]
> Ensuneu erstellte und Migrationen funktionieren nicht gut zusammen. Wenn Sie Migrationen verwenden, verwenden Sie "ensurecreated" nicht, um das Schema zu initialisieren.

Der Übergang von ensuneu in Migrationen ist nicht nahtlos. Die einfachste Möglichkeit hierzu besteht darin, die Datenbank zu löschen und Sie mithilfe von Migrationen neu zu erstellen. Wenn Sie die Verwendung von Migrationen in der Zukunft erwarten, empfiehlt es sich, einfach mit Migrationen zu beginnen, anstatt "ensuneu" zu verwenden.

## <a name="ensuredeleted"></a>Ensuredeleted

Die ensuredeleted-Methode löscht die Datenbank, wenn Sie vorhanden ist. Wenn Sie nicht über die entsprechenden Berechtigungen verfügen, wird eine Ausnahme ausgelöst.

``` csharp
// Drop the database if it exists
dbContext.Database.EnsureDeleted();
```

## <a name="ensurecreated"></a>Ensuneu erstellt

Ensuneu erstellt erstellt die Datenbank, wenn Sie nicht vorhanden ist, und initialisiert das Datenbankschema. Wenn Tabellen vorhanden sind (einschließlich Tabellen für eine andere dbcontext-Klasse), wird das Schema nicht initialisiert.

``` csharp
// Create the database if it doesn't exist
dbContext.Database.EnsureCreated();
```

> [!TIP]
> Asynchrone Versionen dieser Methoden sind ebenfalls verfügbar.

## <a name="sql-script"></a>SQL-Skript

Um das von ensuneu verwendete SQL zu erhalten, können Sie die generatecreatescript-Methode verwenden.

``` csharp
var sql = dbContext.Database.GenerateCreateScript();
```

## <a name="multiple-dbcontext-classes"></a>Mehrere dbcontext-Klassen

Ensuneu erstellt funktioniert nur, wenn keine Tabellen in der Datenbank vorhanden sind. Bei Bedarf können Sie eine eigene Überprüfung schreiben, um festzustellen, ob das Schema initialisiert werden muss, und den zugrunde liegenden irelationaldatabasecreator-Dienst verwenden, um das Schema zu initialisieren.

``` csharp
// TODO: Check whether the schema needs to be initialized

// Initialize the schema for this DbContext
var databaseCreator = dbContext.GetService<IRelationalDatabaseCreator>();
databaseCreator.CreateTables();
```
