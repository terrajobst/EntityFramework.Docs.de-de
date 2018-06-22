---
title: Migrationen mit mehreren Projekten - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 3684e86cce0005056380d89604d038c734054d14
ms.sourcegitcommit: ced2637bf8cc5964c6daa6c7fcfce501bf9ef6e8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/22/2017
ms.locfileid: "27161226"
---
<a name="using-a-separate-project"></a><span data-ttu-id="c2bb6-102">Verwenden ein separates Projekt</span><span class="sxs-lookup"><span data-stu-id="c2bb6-102">Using a Separate Project</span></span>
========================
<span data-ttu-id="c2bb6-103">Empfiehlt es sich zum Speichern Ihrer Migrations in einer anderen Assembly als eine enthält die `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="c2bb6-103">You may want to store your migrations in a different assembly than the one containing your `DbContext`.</span></span> <span data-ttu-id="c2bb6-104">Sie können auch diese Strategie mehrere Sätze von Migrationen, z. B. darin, dass für Entwicklung und andere für Upgrades von Version zu Version.</span><span class="sxs-lookup"><span data-stu-id="c2bb6-104">You can also use this strategy to maintain multiple sets of migrations, for example, one for development and another for release-to-release upgrades.</span></span>

<span data-ttu-id="c2bb6-105">Aufgabe</span><span class="sxs-lookup"><span data-stu-id="c2bb6-105">To do this...</span></span>

1. <span data-ttu-id="c2bb6-106">Erstellen Sie eine neue Klassenbibliothek.</span><span class="sxs-lookup"><span data-stu-id="c2bb6-106">Create a new class library.</span></span>

2. <span data-ttu-id="c2bb6-107">Fügen Sie einen Verweis auf die DbContext-Assembly hinzu.</span><span class="sxs-lookup"><span data-stu-id="c2bb6-107">Add a reference to your DbContext assembly.</span></span>

3. <span data-ttu-id="c2bb6-108">Verschieben Sie die Migrationen und Modelldateien für die Momentaufnahme auf die Klassenbibliothek.</span><span class="sxs-lookup"><span data-stu-id="c2bb6-108">Move the migrations and model snapshot files to the class library.</span></span>
   * <span data-ttu-id="c2bb6-109">Wenn Sie alle noch nicht hinzugefügt, fügen Sie der DbContext-Projekt einen Verweis hinzu hinzu, und verschieben Sie sie.</span><span class="sxs-lookup"><span data-stu-id="c2bb6-109">If you haven't added any, add one to the DbContext project then move it.</span></span>

4. <span data-ttu-id="c2bb6-110">Die Assembly Migrationen zu konfigurieren:</span><span class="sxs-lookup"><span data-stu-id="c2bb6-110">Configure the migrations assembly:</span></span>

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. <span data-ttu-id="c2bb6-111">Fügen Sie einen Verweis auf die Assembly Migrationen aus der Startassembly hinzu.</span><span class="sxs-lookup"><span data-stu-id="c2bb6-111">Add a reference to your migrations assembly from the startup assembly.</span></span>
   * <span data-ttu-id="c2bb6-112">Wenn dies bewirkt, eine zirkuläre Abhängigkeit dass, aktualisieren Sie den Ausgabepfad der Klassenbibliothek:</span><span class="sxs-lookup"><span data-stu-id="c2bb6-112">If this causes a circular dependency, update the output path of the class library:</span></span>

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStarupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

<span data-ttu-id="c2bb6-113">Wenn Sie alles richtig gemacht haben, sollten Sie neue Migrationen zum Projekt hinzufügen können.</span><span class="sxs-lookup"><span data-stu-id="c2bb6-113">If you did everything correctly, you should be able to add new migrations to the project.</span></span>

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```
``` Console
dotnet ef migrations add NewMigration --project MyApp.Migrations
```
