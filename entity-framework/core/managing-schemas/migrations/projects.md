---
title: Migrationen mit mehreren Projekten - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: e059deb6c7555a4f6732fe7942855e95b3f9a34c
ms.sourcegitcommit: b467368cc350e6059fdc0949e042a41cb11e61d9
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
<a name="using-a-separate-project"></a>Verwenden ein separates Projekt
========================
Empfiehlt es sich zum Speichern Ihrer Migrations in einer anderen Assembly als eine enthält die `DbContext`. Sie können auch diese Strategie mehrere Sätze von Migrationen, z. B. darin, dass für Entwicklung und andere für Upgrades von Version zu Version.

Aufgabe

1. Erstellen Sie eine neue Klassenbibliothek.

2. Fügen Sie einen Verweis auf die DbContext-Assembly hinzu.

3. Verschieben Sie die Migrationen und Modelldateien für die Momentaufnahme auf die Klassenbibliothek.
   * Wenn Sie alle noch nicht hinzugefügt, fügen Sie der DbContext-Projekt einen Verweis hinzu hinzu, und verschieben Sie sie.

4. Die Assembly Migrationen zu konfigurieren:

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. Fügen Sie einen Verweis auf die Assembly Migrationen aus der Startassembly hinzu.
   * Wenn dies bewirkt, eine zirkuläre Abhängigkeit dass, aktualisieren Sie den Ausgabepfad der Klassenbibliothek:

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStarupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

Wenn Sie alles richtig gemacht haben, sollten Sie neue Migrationen zum Projekt hinzufügen können.

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```
``` Console
dotnet ef migraitons add NewMigration --project MyApp.Migrations
```
