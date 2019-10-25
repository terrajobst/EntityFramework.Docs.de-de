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
# <a name="using-a-separate-migrations-project"></a><span data-ttu-id="c7050-102">Verwenden eines separaten Migrations Projekts</span><span class="sxs-lookup"><span data-stu-id="c7050-102">Using a Separate Migrations Project</span></span>

<span data-ttu-id="c7050-103">Möglicherweise möchten Sie Ihre Migrationen in einer anderen Assembly speichern, als die, die ihre `DbContext`enthält.</span><span class="sxs-lookup"><span data-stu-id="c7050-103">You may want to store your migrations in a different assembly than the one containing your `DbContext`.</span></span> <span data-ttu-id="c7050-104">Sie können diese Strategie auch verwenden, um mehrere Sätze von Migrationen beizubehalten, z. b. eine für die Entwicklung und eine andere für releaseaktualisierungen.</span><span class="sxs-lookup"><span data-stu-id="c7050-104">You can also use this strategy to maintain multiple sets of migrations, for example, one for development and another for release-to-release upgrades.</span></span>

<span data-ttu-id="c7050-105">Aufgabe</span><span class="sxs-lookup"><span data-stu-id="c7050-105">To do this...</span></span>

1. <span data-ttu-id="c7050-106">Erstellen Sie eine neue Klassenbibliothek.</span><span class="sxs-lookup"><span data-stu-id="c7050-106">Create a new class library.</span></span>

2. <span data-ttu-id="c7050-107">Fügen Sie einen Verweis auf die dbcontext-Assembly hinzu.</span><span class="sxs-lookup"><span data-stu-id="c7050-107">Add a reference to your DbContext assembly.</span></span>

3. <span data-ttu-id="c7050-108">Verschieben Sie die Migrationen und die Modell Momentaufnahme-Dateien in die Klassenbibliothek.</span><span class="sxs-lookup"><span data-stu-id="c7050-108">Move the migrations and model snapshot files to the class library.</span></span>
   > [!TIP]
   > <span data-ttu-id="c7050-109">Wenn keine Migrationen vorhanden sind, generieren Sie eine im Projekt, das den dbcontext enthält, und verschieben Sie Sie dann.</span><span class="sxs-lookup"><span data-stu-id="c7050-109">If you have no existing migrations, generate one in the project containing the DbContext then move it.</span></span>
   > <span data-ttu-id="c7050-110">Dies ist wichtig, denn wenn die Migrationsassembly keine vorhandene Migration enthält, kann der Befehl "Add-Migration" den dbcontext nicht finden.</span><span class="sxs-lookup"><span data-stu-id="c7050-110">This is important because if the migrations assembly does not contain an existing migration, the Add-Migration command will be unable to find the DbContext.</span></span>

4. <span data-ttu-id="c7050-111">Konfigurieren Sie die Migrations Assembly:</span><span class="sxs-lookup"><span data-stu-id="c7050-111">Configure the migrations assembly:</span></span>

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. <span data-ttu-id="c7050-112">Fügen Sie einen Verweis auf ihre Migrationen-Assembly aus der Startassembly hinzu.</span><span class="sxs-lookup"><span data-stu-id="c7050-112">Add a reference to your migrations assembly from the startup assembly.</span></span>
   * <span data-ttu-id="c7050-113">Wenn dies eine zirkuläre Abhängigkeit verursacht, aktualisieren Sie den Ausgabepfad der Klassenbibliothek:</span><span class="sxs-lookup"><span data-stu-id="c7050-113">If this causes a circular dependency, update the output path of the class library:</span></span>

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStartupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

<span data-ttu-id="c7050-114">Wenn Sie alles ordnungsgemäß durchgeführt haben, sollten Sie dem Projekt neue Migrationen hinzufügen können.</span><span class="sxs-lookup"><span data-stu-id="c7050-114">If you did everything correctly, you should be able to add new migrations to the project.</span></span>

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```

``` Console
dotnet ef migrations add NewMigration --project MyApp.Migrations
```
