---
title: Erstellen und Löschen von APIs – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/10/2017
ms.openlocfilehash: 336f6fd655603a2474a58dfef377e121d9b04c3a
ms.sourcegitcommit: a088421ecac4f5dc5213208170490181ae2f5f0f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2018
ms.locfileid: "51285638"
---
# <a name="create-and-drop-apis"></a>Erstellen und Löschen von APIs

Die Methoden EnsureCreated und EnsureDeleted bieten eine einfache Alternative zur [Migrationen](migrations/index.md) für die Verwaltung des Datenbankschemas. Dies ist in Szenarien nützlich, wenn die Daten vorübergehend ist und gelöscht werden, können Wenn das Schema ändert. Beispielsweise während der Erstellung von Prototypen, Tests oder für den lokalen Caches.

Einige Anbieter (insbesondere nicht relationale diejenigen) unterstützen keine Migrationen. Für diese ist EnsureCreated oft die einfachste Möglichkeit zum Initialisieren des Datenbankschemas.

> [!WARNING]
> EnsureCreated und Migrationen funktionieren nicht gut zusammen. Wenn Sie Migrationen verwenden, verwenden Sie keine EnsureCreated um das Schema zu initialisieren.

Übergang von EnsureCreated zu Migrationen ist nicht nahtlos. Die Simpelest Möglichkeit hierzu ist die Datenbank löschen und neu erstellen mithilfe von Migrationen. Wenn sich abzeichnet, Migrationen in Zukunft verwenden, empfiehlt es sich, nur mit Migrationen zu beginnen, anstatt EnsureCreated.

## <a name="ensuredeleted"></a>EnsureDeleted

Die Ensuredeleted--Methode wird die Datenbank löschen, wenn es vorhanden ist. Wenn Sie nicht über die entsprechenden Berechtigungen verfügen, wird eine Ausnahme ausgelöst.

``` csharp
// Drop the database if it exists
dbContext.Database.EnsureDeleted();
```

## <a name="ensurecreated"></a>EnsureCreated

EnsureCreated wird die Datenbank erstellt, wenn er nicht vorhanden, und initialisieren das Datenbankschema. Wenn Tabellen vorhanden sind wird nicht durch das (einschließlich der Tabellen für ein anderes DbContext-Klasse) Schema initialisiert werden.

``` csharp
// Create the database if it doesn't exist
dbContext.Database.EnsureCreated();
```

> [!TIP]
> Asynchrone Versionen dieser Methoden sind auch verfügbar.

## <a name="sql-script"></a>SQL-Skript

Rufen Sie die SQL von EnsureCreated verwendet, können Sie die GenerateCreateScript-Methode verwenden.

``` csharp
var sql = dbContext.Database.GenerateCreateScript();
```

## <a name="multiple-dbcontext-classes"></a>Mehrere "DbContext"-Klassen

EnsureCreated funktioniert nur, wenn keine Tabellen in der Datenbank vorhanden sind. Bei Bedarf können Sie Ihre eigene Überprüfung, um festzustellen, ob das Schema initialisiert werden muss, schreiben und verwenden den zugrunde liegenden IRelationalDatabaseCreator-Dienst, um das Schema zu initialisieren.

``` csharp
// TODO: Check whether the schema needs to be initialized

// Initialize the schema for this DbContext
var databaseCreator = dbContext.GetService<IRelationalDatabaseCreator>();
databaseCreator.CreateTables();
```
