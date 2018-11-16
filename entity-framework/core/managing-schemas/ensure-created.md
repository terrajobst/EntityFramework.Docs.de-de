---
title: Erstellen und Löschen von APIs – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/7/2018
ms.openlocfilehash: 40d9e3aa0aba1bf2bc341f01dd815ed7cb7b48fa
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688628"
---
# <a name="create-and-drop-apis"></a>Erstellen und Löschen von APIs

Die Methoden EnsureCreated und EnsureDeleted bieten eine einfache Alternative zur [Migrationen](migrations/index.md) für die Verwaltung des Datenbankschemas. Diese Methoden sind in Szenarien nützlich, wenn die Daten vorübergehend ist und gelöscht werden, können Wenn das Schema ändert. Beispielsweise während der Erstellung von Prototypen, Tests oder für den lokalen Caches.

Einige Anbieter (insbesondere nicht relationale diejenigen) unterstützen keine Migrationen. Bei diesen Anbietern lautet ist EnsureCreated oft die einfachste Möglichkeit zum Initialisieren des Datenbankschemas.

> [!WARNING]
> EnsureCreated und Migrationen funktionieren nicht gut zusammen. Wenn Sie Migrationen verwenden, verwenden Sie keine EnsureCreated um das Schema zu initialisieren.

Übergang von EnsureCreated zu Migrationen ist nicht nahtlos. Die einfachste Möglichkeit dafür ist die Datenbank löschen und erneut mithilfe von Migrationen erstellen. Wenn sich abzeichnet, Migrationen in Zukunft verwenden, empfiehlt es sich, nur mit Migrationen zu beginnen, anstatt EnsureCreated.

## <a name="ensuredeleted"></a>EnsureDeleted

Die Ensuredeleted--Methode wird die Datenbank löschen, wenn es vorhanden ist. Wenn Sie die entsprechenden Berechtigungen haben, wird eine Ausnahme ausgelöst.

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
