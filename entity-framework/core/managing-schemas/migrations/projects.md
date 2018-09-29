---
title: Migrationen mit mehreren Projekten – EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
uid: core/managing-schemas/migrations/projects
ms.openlocfilehash: 30a6afad1488e74ce2585be3d780186311379a97
ms.sourcegitcommit: ad1bdea58ed35d0f19791044efe9f72f94189c18
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/28/2018
ms.locfileid: "47447143"
---
<a name="using-a-separate-project"></a>Verwenden eines separaten Projekts
========================
Empfiehlt, speichern Ihre Migrationen in einer anderen Assembly als die mit Ihrer `DbContext`. Sie können auch diese Strategie, um mehrere Sätze von Migrationen zu verwalten, z. B., eine für die Entwicklung und eine andere Version-zu-Release-Upgrades verwenden.

Aufgabe

1. Erstellen Sie eine neue Klassenbibliothek.

2. Fügen Sie einen Verweis auf die "DbContext"-Assembly hinzu.

3. Verschieben Sie die Migrationen und Momentaufnahme-Modelldateien, die Klassenbibliothek.
   > [!TIP]
   > Wenn Sie keine vorhandenen Migrationen haben, generieren Sie, in dem die "DbContext", und verschieben Sie es. Dies ist wichtig, denn wenn die Assembly Migrationen nicht mit eine Migration von vorhandene enthält, der Befehl Add-Migration wurde nicht gefunden "DbContext".

4. Konfigurieren Sie die Assembly Migrationen:

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. Fügen Sie einen Verweis auf die Assembly für die Migration aus der Startassembly hinzu.
   * Wenn dies bewirkt, eine ringabhängigkeit dass, aktualisieren Sie den Ausgabepfad der Klassenbibliothek:

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStartupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

Wenn Sie alles richtig gemacht haben, sollten Sie neue Migrationen zum Projekt hinzufügen können.

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```
``` Console
dotnet ef migrations add NewMigration --project MyApp.Migrations
```
