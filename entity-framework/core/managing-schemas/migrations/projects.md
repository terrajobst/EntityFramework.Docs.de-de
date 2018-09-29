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
<a name="using-a-separate-project"></a><span data-ttu-id="1c58c-102">Verwenden eines separaten Projekts</span><span class="sxs-lookup"><span data-stu-id="1c58c-102">Using a Separate Project</span></span>
========================
<span data-ttu-id="1c58c-103">Empfiehlt, speichern Ihre Migrationen in einer anderen Assembly als die mit Ihrer `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="1c58c-103">You may want to store your migrations in a different assembly than the one containing your `DbContext`.</span></span> <span data-ttu-id="1c58c-104">Sie können auch diese Strategie, um mehrere Sätze von Migrationen zu verwalten, z. B., eine für die Entwicklung und eine andere Version-zu-Release-Upgrades verwenden.</span><span class="sxs-lookup"><span data-stu-id="1c58c-104">You can also use this strategy to maintain multiple sets of migrations, for example, one for development and another for release-to-release upgrades.</span></span>

<span data-ttu-id="1c58c-105">Aufgabe</span><span class="sxs-lookup"><span data-stu-id="1c58c-105">To do this...</span></span>

1. <span data-ttu-id="1c58c-106">Erstellen Sie eine neue Klassenbibliothek.</span><span class="sxs-lookup"><span data-stu-id="1c58c-106">Create a new class library.</span></span>

2. <span data-ttu-id="1c58c-107">Fügen Sie einen Verweis auf die "DbContext"-Assembly hinzu.</span><span class="sxs-lookup"><span data-stu-id="1c58c-107">Add a reference to your DbContext assembly.</span></span>

3. <span data-ttu-id="1c58c-108">Verschieben Sie die Migrationen und Momentaufnahme-Modelldateien, die Klassenbibliothek.</span><span class="sxs-lookup"><span data-stu-id="1c58c-108">Move the migrations and model snapshot files to the class library.</span></span>
   > [!TIP]
   > <span data-ttu-id="1c58c-109">Wenn Sie keine vorhandenen Migrationen haben, generieren Sie, in dem die "DbContext", und verschieben Sie es.</span><span class="sxs-lookup"><span data-stu-id="1c58c-109">If you have no existing migrations, generate one in the project containing the DbContext then move it.</span></span> <span data-ttu-id="1c58c-110">Dies ist wichtig, denn wenn die Assembly Migrationen nicht mit eine Migration von vorhandene enthält, der Befehl Add-Migration wurde nicht gefunden "DbContext".</span><span class="sxs-lookup"><span data-stu-id="1c58c-110">This is important because if the migrations assembly does not contain an existing migration, the Add-Migration command will be unable to find the DbContext.</span></span>

4. <span data-ttu-id="1c58c-111">Konfigurieren Sie die Assembly Migrationen:</span><span class="sxs-lookup"><span data-stu-id="1c58c-111">Configure the migrations assembly:</span></span>

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. <span data-ttu-id="1c58c-112">Fügen Sie einen Verweis auf die Assembly für die Migration aus der Startassembly hinzu.</span><span class="sxs-lookup"><span data-stu-id="1c58c-112">Add a reference to your migrations assembly from the startup assembly.</span></span>
   * <span data-ttu-id="1c58c-113">Wenn dies bewirkt, eine ringabhängigkeit dass, aktualisieren Sie den Ausgabepfad der Klassenbibliothek:</span><span class="sxs-lookup"><span data-stu-id="1c58c-113">If this causes a circular dependency, update the output path of the class library:</span></span>

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStartupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

<span data-ttu-id="1c58c-114">Wenn Sie alles richtig gemacht haben, sollten Sie neue Migrationen zum Projekt hinzufügen können.</span><span class="sxs-lookup"><span data-stu-id="1c58c-114">If you did everything correctly, you should be able to add new migrations to the project.</span></span>

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```
``` Console
dotnet ef migrations add NewMigration --project MyApp.Migrations
```
