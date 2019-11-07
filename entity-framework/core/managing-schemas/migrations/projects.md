---
title: Verwenden eines separaten Migrations Projekts-EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
uid: core/managing-schemas/migrations/projects
ms.openlocfilehash: 0c08855db77470d28e23f9ef1d147497dfcdff83
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655555"
---
# <a name="using-a-separate-migrations-project"></a><span data-ttu-id="d97a1-102">Verwenden eines separaten Migrations Projekts</span><span class="sxs-lookup"><span data-stu-id="d97a1-102">Using a Separate Migrations Project</span></span>

<span data-ttu-id="d97a1-103">Möglicherweise möchten Sie Ihre Migrationen in einer anderen Assembly speichern, als die, die ihre `DbContext`enthält.</span><span class="sxs-lookup"><span data-stu-id="d97a1-103">You may want to store your migrations in a different assembly than the one containing your `DbContext`.</span></span> <span data-ttu-id="d97a1-104">Sie können diese Strategie auch verwenden, um mehrere Sätze von Migrationen beizubehalten, z. b. eine für die Entwicklung und eine andere für releaseaktualisierungen.</span><span class="sxs-lookup"><span data-stu-id="d97a1-104">You can also use this strategy to maintain multiple sets of migrations, for example, one for development and another for release-to-release upgrades.</span></span>

<span data-ttu-id="d97a1-105">Aufgabe</span><span class="sxs-lookup"><span data-stu-id="d97a1-105">To do this...</span></span>

1. <span data-ttu-id="d97a1-106">Erstellen Sie eine neue Klassenbibliothek.</span><span class="sxs-lookup"><span data-stu-id="d97a1-106">Create a new class library.</span></span>

2. <span data-ttu-id="d97a1-107">Fügen Sie einen Verweis auf die dbcontext-Assembly hinzu.</span><span class="sxs-lookup"><span data-stu-id="d97a1-107">Add a reference to your DbContext assembly.</span></span>

3. <span data-ttu-id="d97a1-108">Verschieben Sie die Migrationen und die Modell Momentaufnahme-Dateien in die Klassenbibliothek.</span><span class="sxs-lookup"><span data-stu-id="d97a1-108">Move the migrations and model snapshot files to the class library.</span></span>
   > [!TIP]
   > <span data-ttu-id="d97a1-109">Wenn keine Migrationen vorhanden sind, generieren Sie eine im Projekt, das den dbcontext enthält, und verschieben Sie Sie dann.</span><span class="sxs-lookup"><span data-stu-id="d97a1-109">If you have no existing migrations, generate one in the project containing the DbContext then move it.</span></span>
   > <span data-ttu-id="d97a1-110">Dies ist wichtig, denn wenn die Migrationsassembly keine vorhandene Migration enthält, kann der Befehl "Add-Migration" den dbcontext nicht finden.</span><span class="sxs-lookup"><span data-stu-id="d97a1-110">This is important because if the migrations assembly does not contain an existing migration, the Add-Migration command will be unable to find the DbContext.</span></span>

4. <span data-ttu-id="d97a1-111">Konfigurieren Sie die Migrations Assembly:</span><span class="sxs-lookup"><span data-stu-id="d97a1-111">Configure the migrations assembly:</span></span>

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. <span data-ttu-id="d97a1-112">Fügen Sie einen Verweis auf ihre Migrationen-Assembly aus der Startassembly hinzu.</span><span class="sxs-lookup"><span data-stu-id="d97a1-112">Add a reference to your migrations assembly from the startup assembly.</span></span>
   * <span data-ttu-id="d97a1-113">Wenn dies eine zirkuläre Abhängigkeit verursacht, aktualisieren Sie den Ausgabepfad der Klassenbibliothek:</span><span class="sxs-lookup"><span data-stu-id="d97a1-113">If this causes a circular dependency, update the output path of the class library:</span></span>

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStartupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

<span data-ttu-id="d97a1-114">Wenn Sie alles ordnungsgemäß durchgeführt haben, sollten Sie dem Projekt neue Migrationen hinzufügen können.</span><span class="sxs-lookup"><span data-stu-id="d97a1-114">If you did everything correctly, you should be able to add new migrations to the project.</span></span>

## <a name="net-core-clitabdotnet-core-cli"></a>[<span data-ttu-id="d97a1-115">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="d97a1-115">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

``` Console
dotnet ef migrations add NewMigration --project MyApp.Migrations
```

## <a name="visual-studiotabvs"></a>[<span data-ttu-id="d97a1-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d97a1-116">Visual Studio</span></span>](#tab/vs)

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```

***
