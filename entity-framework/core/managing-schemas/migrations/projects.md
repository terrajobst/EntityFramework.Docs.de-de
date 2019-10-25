---
title: Verwenden eines separaten Migrations Projekts-EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
uid: core/managing-schemas/migrations/projects
ms.openlocfilehash: 0082b0af2905fe9e5c3c6509516f622c9d4f8370
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812029"
---
# <a name="using-a-separate-migrations-project"></a>Verwenden eines separaten Migrations Projekts

Möglicherweise möchten Sie Ihre Migrationen in einer anderen Assembly speichern, als die, die ihre `DbContext`enthält. Sie können diese Strategie auch verwenden, um mehrere Sätze von Migrationen beizubehalten, z. b. eine für die Entwicklung und eine andere für releaseaktualisierungen.

Aufgabe

1. Erstellen Sie eine neue Klassenbibliothek.

2. Fügen Sie einen Verweis auf die dbcontext-Assembly hinzu.

3. Verschieben Sie die Migrationen und die Modell Momentaufnahme-Dateien in die Klassenbibliothek.
   > [!TIP]
   > Wenn keine Migrationen vorhanden sind, generieren Sie eine im Projekt, das den dbcontext enthält, und verschieben Sie Sie dann.
   > Dies ist wichtig, denn wenn die Migrationsassembly keine vorhandene Migration enthält, kann der Befehl "Add-Migration" den dbcontext nicht finden.

4. Konfigurieren Sie die Migrations Assembly:

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. Fügen Sie einen Verweis auf ihre Migrationen-Assembly aus der Startassembly hinzu.
   * Wenn dies eine zirkuläre Abhängigkeit verursacht, aktualisieren Sie den Ausgabepfad der Klassenbibliothek:

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStartupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

Wenn Sie alles ordnungsgemäß durchgeführt haben, sollten Sie dem Projekt neue Migrationen hinzufügen können.

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```

``` Console
dotnet ef migrations add NewMigration --project MyApp.Migrations
```
