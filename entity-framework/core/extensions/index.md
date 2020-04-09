---
title: Tools und Erweiterungen – EF Core
author: ErikEJ
ms.date: 12/17/2019
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: e3806f7161fecfe66450d3e08f97caf3d2c84cf3
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/07/2020
ms.locfileid: "80634237"
---
# <a name="ef-core-tools--extensions"></a><span data-ttu-id="6550d-102">EF Core Tools und -Erweiterungen</span><span class="sxs-lookup"><span data-stu-id="6550d-102">EF Core Tools & Extensions</span></span>

<span data-ttu-id="6550d-103">Diese Tools und Erweiterungen bieten zusätzliche Funktionen für Entity Framework Core 2.1 und höher.</span><span class="sxs-lookup"><span data-stu-id="6550d-103">These tools and extensions provide additional functionality for Entity Framework Core 2.1 and later.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="6550d-104">Erweiterungen werden durch eine Vielzahl von Quellen erstellt und nicht als Teil des Entity Framework Core-Projekts verwaltet.</span><span class="sxs-lookup"><span data-stu-id="6550d-104">Extensions are built by a variety of sources and aren't maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="6550d-105">Wenn Sie die Erweiterung eines Drittanbieters in Betracht ziehen, sollten Sie Qualität, Lizenzierung, Kompatibilität, Support usw. auswerten, um sicherzustellen, dass diese Ihren Anforderungen entspricht.</span><span class="sxs-lookup"><span data-stu-id="6550d-105">When considering a third party extension, be sure to evaluate its quality, licensing, compatibility, support, etc. to ensure it meets your requirements.</span></span> <span data-ttu-id="6550d-106">Bei Erweiterungen, die für eine ältere EF Core-Version erstellt wurden, ist insbesondere darauf zu achten, dass sie möglicherweise aktualisiert werden müssen, bevor sie mit den neuesten Versionen kompatibel sind.</span><span class="sxs-lookup"><span data-stu-id="6550d-106">In particular, an extension built for an older version of EF Core may need updating before it will work with the latest versions.</span></span>

## <a name="tools"></a><span data-ttu-id="6550d-107">Tools</span><span class="sxs-lookup"><span data-stu-id="6550d-107">Tools</span></span>

### <a name="llblgen-pro"></a><span data-ttu-id="6550d-108">LLBLGen Pro</span><span class="sxs-lookup"><span data-stu-id="6550d-108">LLBLGen Pro</span></span>

<span data-ttu-id="6550d-109">LLBLGen Pro ist eine Entitätsmodelllösung, die Entity Framework und Entity Framework Core unterstützt.</span><span class="sxs-lookup"><span data-stu-id="6550d-109">LLBLGen Pro is an entity modeling solution with support for Entity Framework and Entity Framework Core.</span></span> <span data-ttu-id="6550d-110">Sie können problemlos ihr Entitätsmodell definieren und es Ihrer Datenbank zuordnen, indem Sie Database First oder Model First verwenden, sodass Sie sofort mit dem Schreiben von Abfragen beginnen können.</span><span class="sxs-lookup"><span data-stu-id="6550d-110">It lets you easily define your entity model and map it to your database, using database first or model first, so you can get started writing queries right away.</span></span> <span data-ttu-id="6550d-111">Für EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="6550d-111">For EF Core: 2.</span></span>

[<span data-ttu-id="6550d-112">Website</span><span class="sxs-lookup"><span data-stu-id="6550d-112">Website</span></span>](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a><span data-ttu-id="6550d-113">Devart Entity Developer</span><span class="sxs-lookup"><span data-stu-id="6550d-113">Devart Entity Developer</span></span>

<span data-ttu-id="6550d-114">Entity Developer ist ein leistungsstarker ORM-Designer für ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access und LINQ to SQL.</span><span class="sxs-lookup"><span data-stu-id="6550d-114">Entity Developer is a powerful ORM designer for ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access, and LINQ to SQL.</span></span> <span data-ttu-id="6550d-115">Sie können damit EF Core-Modelle mithilfe von Model First oder Database First visuell entwerfen sowie C#- oder Visual Basic-Code generieren.</span><span class="sxs-lookup"><span data-stu-id="6550d-115">It supports designing EF Core models visually, using model first or database first approaches, and C# or Visual Basic code generation.</span></span> <span data-ttu-id="6550d-116">Für EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="6550d-116">For EF Core: 2.</span></span>

[<span data-ttu-id="6550d-117">Website</span><span class="sxs-lookup"><span data-stu-id="6550d-117">Website</span></span>](https://www.devart.com/entitydeveloper/)

### <a name="nhydrate-orm-for-entity-framework"></a><span data-ttu-id="6550d-118">nHydrate ORM für Entity Framework</span><span class="sxs-lookup"><span data-stu-id="6550d-118">nHydrate ORM for Entity Framework</span></span>

<span data-ttu-id="6550d-119">Ein ORM, der stark typisierte, erweiterbare Klassen für Entity Framework erstellt.</span><span class="sxs-lookup"><span data-stu-id="6550d-119">An ORM that creates strongly-typed, extendable classes for Entity Framework.</span></span> <span data-ttu-id="6550d-120">Der generierte Code ist Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="6550d-120">The generated code is Entity Framework Core.</span></span> <span data-ttu-id="6550d-121">Es besteht kein Unterschied.</span><span class="sxs-lookup"><span data-stu-id="6550d-121">There is no difference.</span></span> <span data-ttu-id="6550d-122">Dies ist kein Ersatz für EF oder einen benutzerdefinierten ORM.</span><span class="sxs-lookup"><span data-stu-id="6550d-122">This is not a replacement for EF or a custom ORM.</span></span> <span data-ttu-id="6550d-123">Dabei handelt es sich um eine visuelle, Modellierungsebene, die einem Team ermöglicht, komplexe Datenbankschemas zu verwalten.</span><span class="sxs-lookup"><span data-stu-id="6550d-123">It is a visual, modeling layer that allows a team to manage complex database schemas.</span></span> <span data-ttu-id="6550d-124">Dies funktioniert gut mit SCM-Software wie Git, sodass mehrere Benutzer mit minimalen Konflikten auf Ihr Modell zugreifen können.</span><span class="sxs-lookup"><span data-stu-id="6550d-124">It works well with SCM software like Git, allowing multi-user access to your model with minimal conflicts.</span></span> <span data-ttu-id="6550d-125">Das Installationsprogramm verfolgt Modelländerungen und erstellt Upgradeskripts.</span><span class="sxs-lookup"><span data-stu-id="6550d-125">The installer tracks model changes and creates upgrade scripts.</span></span> <span data-ttu-id="6550d-126">Für EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="6550d-126">For EF Core: 3.</span></span>

[<span data-ttu-id="6550d-127">GitHub-Website</span><span class="sxs-lookup"><span data-stu-id="6550d-127">Github site</span></span>](https://github.com/nHydrate/nHydrate)

### <a name="ef-core-power-tools"></a><span data-ttu-id="6550d-128">EF Core Power Tools</span><span class="sxs-lookup"><span data-stu-id="6550d-128">EF Core Power Tools</span></span>

<span data-ttu-id="6550d-129">EF Core Power Tools ist eine Erweiterung von Visual Studio, die verschiedene Aufgaben von EF Core zur Entwurfszeit in einer einfachen Benutzeroberfläche verfügbar macht.</span><span class="sxs-lookup"><span data-stu-id="6550d-129">EF Core Power Tools is a Visual Studio extension that exposes various EF Core design-time tasks in a simple user interface.</span></span> <span data-ttu-id="6550d-130">Dadurch wird Reverse Engineering (Zurückentwicklung) von DbContext- und Entitätsklassen aus vorhandenen Datenbanken und [SQL Server-DACPACs](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), die Verwaltung von Datenbankmigrationen und das Vornehmen von Modellvisualisierungen ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="6550d-130">It includes reverse engineering of DbContext and entity classes from existing databases and [SQL Server DACPACs](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), management of database migrations, and model visualizations.</span></span> <span data-ttu-id="6550d-131">Für EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="6550d-131">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="6550d-132">GitHub-Wiki</span><span class="sxs-lookup"><span data-stu-id="6550d-132">GitHub wiki</span></span>](https://github.com/ErikEJ/EFCorePowerTools/wiki)

### <a name="entity-framework-visual-editor"></a><span data-ttu-id="6550d-133">Visueller Editor für Entity Framework</span><span class="sxs-lookup"><span data-stu-id="6550d-133">Entity Framework Visual Editor</span></span>

<span data-ttu-id="6550d-134">Entity Framework Visual Editor ist eine Erweiterung von Visual Studio, mit der ein ORM-Designer für visuelle Entwürfe von EF 6- und EF Core-Klassen hinzugefügt werden kann.</span><span class="sxs-lookup"><span data-stu-id="6550d-134">Entity Framework Visual Editor is a Visual Studio extension that adds an ORM designer for visual design of EF 6, and EF Core classes.</span></span> <span data-ttu-id="6550d-135">Code wird mithilfe von T4-Vorlagen generiert und kann somit vollständig an die Anforderungen des Benutzers angepasst werden.</span><span class="sxs-lookup"><span data-stu-id="6550d-135">Code is generated using T4 templates so can be customized to suit any needs.</span></span> <span data-ttu-id="6550d-136">Vererbung, uni- und bidirektionale Zuordnungen, Enumerationen, Farbcode für Klassen und das Hinzufügen von Textblöcken für Erklärungen zu möglicherweise schwer durchschaubaren Bereichen Ihres Designs werden unterstützt.</span><span class="sxs-lookup"><span data-stu-id="6550d-136">It supports inheritance, unidirectional and bidirectional associations, enumerations, and the ability to color-code your classes and add text blocks to explain potentially arcane parts of your design.</span></span> <span data-ttu-id="6550d-137">Für EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="6550d-137">For EF Core: 2.</span></span>

[<span data-ttu-id="6550d-138">Marketplace</span><span class="sxs-lookup"><span data-stu-id="6550d-138">Marketplace</span></span>](https://marketplace.visualstudio.com/items?itemName=michaelsawczyn.EFDesigner)

### <a name="catfactory"></a><span data-ttu-id="6550d-139">CatFactory</span><span class="sxs-lookup"><span data-stu-id="6550d-139">CatFactory</span></span>

<span data-ttu-id="6550d-140">CatFactory ist eine Gerüstbau-Engine für .NET Core, mit der die Generierung von DbContext-Klassen und -Entitäten, Zuordnungskonfigurationen und Repositoryklassen aus einer SQL Server-Datenbank automatisiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="6550d-140">CatFactory is a scaffolding engine for .NET Core that can automate the generation of DbContext classes, entities, mapping configurations, and repository classes from a SQL Server database.</span></span> <span data-ttu-id="6550d-141">Für EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="6550d-141">For EF Core: 2.</span></span>

[<span data-ttu-id="6550d-142">GitHub-Repository</span><span class="sxs-lookup"><span data-stu-id="6550d-142">GitHub repository</span></span>](https://github.com/hherzl/CatFactory.EntityFrameworkCore)

### <a name="loresofts-entity-framework-core-generator"></a><span data-ttu-id="6550d-143">Entity Framework Core-Generator von LoreSoft</span><span class="sxs-lookup"><span data-stu-id="6550d-143">LoreSoft's Entity Framework Core Generator</span></span>

<span data-ttu-id="6550d-144">Entity Framework Core-Generator (efg) ist ein .NET Core-CLI-Tool, mit dem EF Core-Modelle aus einer vorhandenen Datenbank generiert werden können, wie beispielsweise `dotnet ef dbcontext scaffold`. Es unterstützt aber auch die [Neugenerierung](https://efg.loresoft.com/en/latest/regeneration/) sicheren Codes durch das Ersetzen eines Bereichs oder durch das Analysieren von Zuordnungsdateien.</span><span class="sxs-lookup"><span data-stu-id="6550d-144">Entity Framework Core Generator (efg) is a .NET Core CLI tool that can generate EF Core models from an existing database, much like `dotnet ef dbcontext scaffold`, but it also supports safe code [regeneration](https://efg.loresoft.com/en/latest/regeneration/) via region replacement or by parsing mapping files.</span></span> <span data-ttu-id="6550d-145">Dieses Tool unterstützt das Generieren von Ansichtsmodellen, die Validierung und Objektzuordnungscode.</span><span class="sxs-lookup"><span data-stu-id="6550d-145">This tool supports generating view models, validation, and object mapper code.</span></span> <span data-ttu-id="6550d-146">Für EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="6550d-146">For EF Core: 2.</span></span>

<span data-ttu-id="6550d-147">[Tutorial](https://www.loresoft.com/Generate-ASP-NET-Web-API)
[Dokumentation](https://efg.loresoft.com/en/latest/)</span><span class="sxs-lookup"><span data-stu-id="6550d-147">[Tutorial](https://www.loresoft.com/Generate-ASP-NET-Web-API)
[Documentation](https://efg.loresoft.com/en/latest/)</span></span>

## <a name="extensions"></a><span data-ttu-id="6550d-148">Erweiterungen</span><span class="sxs-lookup"><span data-stu-id="6550d-148">Extensions</span></span>

### <a name="microsoftentityframeworkcoreautohistory"></a><span data-ttu-id="6550d-149">Microsoft.EntityFrameworkCore.AutoHistory</span><span class="sxs-lookup"><span data-stu-id="6550d-149">Microsoft.EntityFrameworkCore.AutoHistory</span></span>

<span data-ttu-id="6550d-150">Eine Plug-in-Bibliothek, mit deren Hilfe automatisch von EF Core durchgeführte Änderungen an den Daten in einer Verlaufstabelle aufgezeichnet werden.</span><span class="sxs-lookup"><span data-stu-id="6550d-150">A plugin library that enables automatically recording the data changes performed by EF Core into a history table.</span></span> <span data-ttu-id="6550d-151">Für EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="6550d-151">For EF Core: 2.</span></span>

[<span data-ttu-id="6550d-152">GitHub-Repository</span><span class="sxs-lookup"><span data-stu-id="6550d-152">GitHub repository</span></span>](https://github.com/Arch/AutoHistory/)

### <a name="efsecondlevelcachecore"></a><span data-ttu-id="6550d-153">EFSecondLevelCache.Core</span><span class="sxs-lookup"><span data-stu-id="6550d-153">EFSecondLevelCache.Core</span></span>

<span data-ttu-id="6550d-154">Eine Erweiterung, die es ermöglicht, die Ergebnisse von EF Core-Abfragen in einem Cache zweiter Ebene zu speichern, sodass nachfolgende Ausführungen der gleichen Abfragen nicht auf die Datenbank zugreifen müssen, sondern die Daten direkt aus dem Cache abrufen können.</span><span class="sxs-lookup"><span data-stu-id="6550d-154">An extension that enables storing the results of EF Core queries into a second-level cache, so that subsequent executions of the same queries can avoid accessing the database and retrieve the data directly from the cache.</span></span> <span data-ttu-id="6550d-155">Für EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="6550d-155">For EF Core: 2.</span></span>

[<span data-ttu-id="6550d-156">GitHub-Repository</span><span class="sxs-lookup"><span data-stu-id="6550d-156">GitHub repository</span></span>](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="geco"></a><span data-ttu-id="6550d-157">Geco</span><span class="sxs-lookup"><span data-stu-id="6550d-157">Geco</span></span>

<span data-ttu-id="6550d-158">Geco (Generator-Konsole) ist ein einfacher Code-Generator, der auf einem Konsolenprojekt basiert, das auf .NET Core ausgeführt wird und interpolierte Zeichenfolgen in C# für die Codegenerierung verwendet.</span><span class="sxs-lookup"><span data-stu-id="6550d-158">Geco (Generator Console) is a simple code generator based on a console project, that runs on .NET Core and uses C# interpolated strings for code generation.</span></span> <span data-ttu-id="6550d-159">Geco enthält einen Reversemodell-Generator für EF Core, der die Pluralisierung, Singularisierung und bearbeitbare Vorlagen unterstützt.</span><span class="sxs-lookup"><span data-stu-id="6550d-159">Geco includes a reverse model generator for EF Core with support for pluralization, singularization, and editable templates.</span></span> <span data-ttu-id="6550d-160">Er enthält auch einen Startdatenskript-Generator, einen Skript-Runner sowie eine Datenbankbereinigung.</span><span class="sxs-lookup"><span data-stu-id="6550d-160">It also provides a seed data script generator, a script runner, and a database cleaner.</span></span> <span data-ttu-id="6550d-161">Für EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="6550d-161">For EF Core: 2.</span></span>

[<span data-ttu-id="6550d-162">GitHub-Repository</span><span class="sxs-lookup"><span data-stu-id="6550d-162">GitHub repository</span></span>](https://github.com/iQuarc/Geco)

### <a name="entityframeworkcorescaffoldinghandlebars"></a><span data-ttu-id="6550d-163">EntityFrameworkCore.Scaffolding.Handlebars</span><span class="sxs-lookup"><span data-stu-id="6550d-163">EntityFrameworkCore.Scaffolding.Handlebars</span></span> 

<span data-ttu-id="6550d-164">Mit dieser Erweiterung können Klassen angepasst werden, die per Reverse Engineering mithilfe der Entity Framework Core-Toolkette mit Handlebars-Vorlagen aus einer vorhandenen Datenbank erstellt wurden.</span><span class="sxs-lookup"><span data-stu-id="6550d-164">Allows customization of classes reverse engineered from an existing database using the Entity Framework Core toolchain with Handlebars templates.</span></span> <span data-ttu-id="6550d-165">Für EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="6550d-165">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="6550d-166">GitHub-Repository</span><span class="sxs-lookup"><span data-stu-id="6550d-166">GitHub repository</span></span>](https://github.com/TrackableEntities/EntityFrameworkCore.Scaffolding.Handlebars)

### <a name="neinlinqentityframeworkcore"></a><span data-ttu-id="6550d-167">NeinLinq.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="6550d-167">NeinLinq.EntityFrameworkCore</span></span> 

<span data-ttu-id="6550d-168">NeinLinq ist eine Erweiterung für LINQ-Anbieter wie Entity Framework, um Funktionen wiederverwenden zu können, Abfragen erneut schreiben zu können und dynamische Abfragen mithilfe von übersetzbaren Prädikaten und Selektoren erstellen zu können.</span><span class="sxs-lookup"><span data-stu-id="6550d-168">NeinLinq extends LINQ providers such as Entity Framework to enable reusing functions, rewriting queries, and building dynamic queries using translatable predicates and selectors.</span></span> <span data-ttu-id="6550d-169">Für EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="6550d-169">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="6550d-170">GitHub-Repository</span><span class="sxs-lookup"><span data-stu-id="6550d-170">GitHub repository</span></span>](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a><span data-ttu-id="6550d-171">Microsoft.EntityFrameworkCore.UnitOfWork</span><span class="sxs-lookup"><span data-stu-id="6550d-171">Microsoft.EntityFrameworkCore.UnitOfWork</span></span>

<span data-ttu-id="6550d-172">Ein Plug-In für Microsoft.EntityFrameworkCore zur Unterstützung von Repositorys, Arbeitseinheitsmustern und mehreren Datenbanken, die verteilte Transaktionen unterstützen.</span><span class="sxs-lookup"><span data-stu-id="6550d-172">A plugin for Microsoft.EntityFrameworkCore to support repository, unit of work patterns, and multiple databases with distributed transaction supported.</span></span> <span data-ttu-id="6550d-173">Für EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="6550d-173">For EF Core: 2.</span></span>

[<span data-ttu-id="6550d-174">GitHub-Repository</span><span class="sxs-lookup"><span data-stu-id="6550d-174">GitHub repository</span></span>](https://github.com/Arch/UnitOfWork/)

### <a name="efcorebulkextensions"></a><span data-ttu-id="6550d-175">EFCore.BulkExtensions</span><span class="sxs-lookup"><span data-stu-id="6550d-175">EFCore.BulkExtensions</span></span>

<span data-ttu-id="6550d-176">EF Core-Erweiterungen für Massenvorgänge (Einfügen, Aktualisieren, Löschen)</span><span class="sxs-lookup"><span data-stu-id="6550d-176">EF Core extensions for Bulk operations (Insert, Update, Delete).</span></span> <span data-ttu-id="6550d-177">Für EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="6550d-177">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="6550d-178">GitHub-Repository</span><span class="sxs-lookup"><span data-stu-id="6550d-178">GitHub repository</span></span>](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a><span data-ttu-id="6550d-179">Bricelam.EntityFrameworkCore.Pluralizer</span><span class="sxs-lookup"><span data-stu-id="6550d-179">Bricelam.EntityFrameworkCore.Pluralizer</span></span>

<span data-ttu-id="6550d-180">Diese Erweiterung fügt die Pluralisierung zur Entwurfszeit hinzu.</span><span class="sxs-lookup"><span data-stu-id="6550d-180">Adds design-time pluralization.</span></span> <span data-ttu-id="6550d-181">Für EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="6550d-181">For EF Core: 2.</span></span>

[<span data-ttu-id="6550d-182">GitHub-Repository</span><span class="sxs-lookup"><span data-stu-id="6550d-182">GitHub repository</span></span>](https://github.com/bricelam/EFCore.Pluralizer)

### <a name="toolbeltentityframeworkcoreindexattribute"></a><span data-ttu-id="6550d-183">Toolbelt.EntityFrameworkCore.IndexAttribute</span><span class="sxs-lookup"><span data-stu-id="6550d-183">Toolbelt.EntityFrameworkCore.IndexAttribute</span></span>

<span data-ttu-id="6550d-184">Über diese Erweiterung ist das [Index]-Attribut mit der Erweiterung für die Modellerstellung wieder verfügbar.</span><span class="sxs-lookup"><span data-stu-id="6550d-184">Revival of [Index] attribute (with extension for model building).</span></span> <span data-ttu-id="6550d-185">Für EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="6550d-185">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="6550d-186">GitHub-Repository</span><span class="sxs-lookup"><span data-stu-id="6550d-186">GitHub repository</span></span>](https://github.com/jsakamoto/EntityFrameworkCore.IndexAttribute)

### <a name="efcoreinmemoryhelpers"></a><span data-ttu-id="6550d-187">EfCore.InMemoryHelpers</span><span class="sxs-lookup"><span data-stu-id="6550d-187">EfCore.InMemoryHelpers</span></span>

<span data-ttu-id="6550d-188">Bietet einen Wrapper, der den In-Memory-Datenbankanbieter für EF Core umschließt.</span><span class="sxs-lookup"><span data-stu-id="6550d-188">Provides a wrapper around the EF Core In-Memory Database Provider.</span></span> <span data-ttu-id="6550d-189">Fungiert dadurch mehr als relationaler Anbieter.</span><span class="sxs-lookup"><span data-stu-id="6550d-189">Makes it act more like a relational provider.</span></span> <span data-ttu-id="6550d-190">Für EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="6550d-190">For EF Core: 2.</span></span>

[<span data-ttu-id="6550d-191">GitHub-Repository</span><span class="sxs-lookup"><span data-stu-id="6550d-191">GitHub repository</span></span>](https://github.com/SimonCropp/EfCore.InMemoryHelpers)

### <a name="efcoretemporalsupport"></a><span data-ttu-id="6550d-192">EFCore.TemporalSupport</span><span class="sxs-lookup"><span data-stu-id="6550d-192">EFCore.TemporalSupport</span></span>

<span data-ttu-id="6550d-193">Mit dieser Erweiterung wird die Unterstützung temporaler Daten implementiert.</span><span class="sxs-lookup"><span data-stu-id="6550d-193">An implementation of temporal support.</span></span> <span data-ttu-id="6550d-194">Für EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="6550d-194">For EF Core: 2.</span></span>

[<span data-ttu-id="6550d-195">GitHub-Repository</span><span class="sxs-lookup"><span data-stu-id="6550d-195">GitHub repository</span></span>](https://github.com/cpoDesign/EFCore.TemporalSupport)

### <a name="efcoretemporaltable"></a><span data-ttu-id="6550d-196">EfCoreTemporalTable</span><span class="sxs-lookup"><span data-stu-id="6550d-196">EfCoreTemporalTable</span></span>

<span data-ttu-id="6550d-197">Mit dieser Erweiterung können Sie mithilfe bereits eingeführter Erweiterungsmethoden temporale Abfragen für eine Datenbank Ihrer Wahl ausführen: `AsTemporalAll()`, `AsTemporalAsOf(date)`, `AsTemporalFrom(startDate, endDate)`, `AsTemporalBetween(startDate, endDate)`, `AsTemporalContained(startDate, endDate)`.</span><span class="sxs-lookup"><span data-stu-id="6550d-197">Easily perform temporal queries on your favourite database using introduced extension methods: `AsTemporalAll()`, `AsTemporalAsOf(date)`, `AsTemporalFrom(startDate, endDate)`, `AsTemporalBetween(startDate, endDate)`, `AsTemporalContained(startDate, endDate)`.</span></span> <span data-ttu-id="6550d-198">Für EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="6550d-198">For EF Core: 3.</span></span>

[<span data-ttu-id="6550d-199">GitHub-Repository</span><span class="sxs-lookup"><span data-stu-id="6550d-199">GitHub repository</span></span>](https://github.com/glautrou/EfCoreTemporalTable)

### <a name="efcoretimetraveler"></a><span data-ttu-id="6550d-200">EFCore.TimeTraveler</span><span class="sxs-lookup"><span data-stu-id="6550d-200">EFCore.TimeTraveler</span></span>

<span data-ttu-id="6550d-201">Diese Erweiterung ermöglicht vollständige Entity Framework Core-Abfragen von [temporalen SQL Server-Verlaufsdaten](/sql/relational-databases/tables/temporal-table-usage-scenarios#point-in-time-analysis-time-travel). Dazu werden der von Ihnen bereits definierte EF Core-Code sowie die dazugehörigen Entitäten und Zuordnungen verwendet.</span><span class="sxs-lookup"><span data-stu-id="6550d-201">Allow full-featured Entity Framework Core queries against [SQL Server Temporal History](/sql/relational-databases/tables/temporal-table-usage-scenarios#point-in-time-analysis-time-travel) using the EF Core code, entities, and mappings you already have defined.</span></span>  <span data-ttu-id="6550d-202">Führen Sie Zeitpunktanalysen (Zeitreise) durch, indem Sie Ihren Code mithilfe der Anweisung `using (TemporalQuery.AsOf(targetDateTime)) {...}` umschließen.</span><span class="sxs-lookup"><span data-stu-id="6550d-202">Travel through time by wrapping your code in `using (TemporalQuery.AsOf(targetDateTime)) {...}`.</span></span> <span data-ttu-id="6550d-203">Für EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="6550d-203">For EF Core: 3.</span></span>

[<span data-ttu-id="6550d-204">GitHub-Repository</span><span class="sxs-lookup"><span data-stu-id="6550d-204">GitHub repository</span></span>](https://github.com/VantageSoftware/EFCore.TimeTraveler)


### <a name="entityframeworkcoretemporaltables"></a><span data-ttu-id="6550d-205">EntityFrameworkCore.TemporalTables</span><span class="sxs-lookup"><span data-stu-id="6550d-205">EntityFrameworkCore.TemporalTables</span></span>

<span data-ttu-id="6550d-206">Eine Erweiterungsbibliothek für Entity Framework Core, mit der Entwickler, die SQL Server nutzen, temporale Tabellen einfach verwenden können.</span><span class="sxs-lookup"><span data-stu-id="6550d-206">Extension library for Entity Framework Core which allows developers who use SQL Server to easily use temporal tables.</span></span> <span data-ttu-id="6550d-207">Für EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="6550d-207">For EF Core: 2.</span></span>

[<span data-ttu-id="6550d-208">GitHub-Repository</span><span class="sxs-lookup"><span data-stu-id="6550d-208">GitHub repository</span></span>](https://github.com/findulov/EntityFrameworkCore.TemporalTables)


### <a name="entityframeworkcorecacheable"></a><span data-ttu-id="6550d-209">EntityFrameworkCore.Cacheable</span><span class="sxs-lookup"><span data-stu-id="6550d-209">EntityFrameworkCore.Cacheable</span></span>

<span data-ttu-id="6550d-210">Ein Abfragecache auf zweiter Ebene mit hoher Leistung.</span><span class="sxs-lookup"><span data-stu-id="6550d-210">A high-performance second-level query cache.</span></span> <span data-ttu-id="6550d-211">Für EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="6550d-211">For EF Core: 2.</span></span>

[<span data-ttu-id="6550d-212">GitHub-Repository</span><span class="sxs-lookup"><span data-stu-id="6550d-212">GitHub repository</span></span>](https://github.com/SteffenMangold/EntityFrameworkCore.Cacheable)

### <a name="entity-framework-plus"></a><span data-ttu-id="6550d-213">Entity Framework Plus</span><span class="sxs-lookup"><span data-stu-id="6550d-213">Entity Framework Plus</span></span>

<span data-ttu-id="6550d-214">Erweitert Ihren DbContext mit Features wie den folgenden: Include Filter, Auditing, Caching, Query Future, Batch Delete, Batch Update und vielen weiteren.</span><span class="sxs-lookup"><span data-stu-id="6550d-214">Extends your DbContext with features such as: Include Filter, Auditing, Caching, Query Future, Batch Delete, Batch Update, and more.</span></span> <span data-ttu-id="6550d-215">Für EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="6550d-215">For EF Core: 2, 3.</span></span>

<span data-ttu-id="6550d-216">[Website](https://entityframework-plus.net/)
[GitHub-Repository](https://github.com/zzzprojects/EntityFramework-Plus)</span><span class="sxs-lookup"><span data-stu-id="6550d-216">[Website](https://entityframework-plus.net/)
[GitHub repository](https://github.com/zzzprojects/EntityFramework-Plus)</span></span>

### <a name="entity-framework-extensions"></a><span data-ttu-id="6550d-217">Entity Framework-Erweiterungen</span><span class="sxs-lookup"><span data-stu-id="6550d-217">Entity Framework Extensions</span></span>

<span data-ttu-id="6550d-218">Erweitert Ihren DbContext mit hochleistungsfähigen Massenvorgängen: BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge und vielen weiteren.</span><span class="sxs-lookup"><span data-stu-id="6550d-218">Extends your DbContext with high-performance bulk operations: BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge, and more.</span></span> <span data-ttu-id="6550d-219">Für EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="6550d-219">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="6550d-220">Website</span><span class="sxs-lookup"><span data-stu-id="6550d-220">Website</span></span>](https://entityframework-extensions.net/)

### <a name="expressionify"></a><span data-ttu-id="6550d-221">Expressionify</span><span class="sxs-lookup"><span data-stu-id="6550d-221">Expressionify</span></span>

<span data-ttu-id="6550d-222">Hinzufügen von Unterstützung für das Aufrufen von Erweiterungsmethoden in LINQ-Lambdas.</span><span class="sxs-lookup"><span data-stu-id="6550d-222">Add support for calling extension methods in linq lambdas.</span></span> <span data-ttu-id="6550d-223">Für EF Core: 3.1</span><span class="sxs-lookup"><span data-stu-id="6550d-223">For EF Core: 3.1</span></span>

[<span data-ttu-id="6550d-224">GitHub-Repository</span><span class="sxs-lookup"><span data-stu-id="6550d-224">GitHub repository</span></span>](https://github.com/ClaveConsulting/Expressionify)

### <a name="xlinq"></a><span data-ttu-id="6550d-225">XLinq</span><span class="sxs-lookup"><span data-stu-id="6550d-225">XLinq</span></span>

<span data-ttu-id="6550d-226">LINQ-Technologie (Language Integrated Query) für relationale Datenbanken.</span><span class="sxs-lookup"><span data-stu-id="6550d-226">Language Integrated Query (LINQ) technology for relational databases.</span></span> <span data-ttu-id="6550d-227">Sie ermöglicht Ihnen, mit C# stark typisierte Abfragen zu schreiben.</span><span class="sxs-lookup"><span data-stu-id="6550d-227">It allows you to use C# to write strongly typed queries.</span></span> <span data-ttu-id="6550d-228">Für EF Core: 3.1</span><span class="sxs-lookup"><span data-stu-id="6550d-228">For EF Core: 3.1</span></span>

- <span data-ttu-id="6550d-229">Volle Unterstützung von C# für das Erstellen von Abfragen: mehrere Anweisungen innerhalb von Lambdaausdrücken, Variablen, Funktionen usw.</span><span class="sxs-lookup"><span data-stu-id="6550d-229">Full C# support for query creation: multiple statements inside lambda, variables, functions, etc.</span></span>
- <span data-ttu-id="6550d-230">Keine semantische Lücke zu SQL.</span><span class="sxs-lookup"><span data-stu-id="6550d-230">No semantic gap with SQL.</span></span> <span data-ttu-id="6550d-231">XLinq deklariert SQL-Anweisungen (wie `SELECT`, `FROM`, `WHERE`) als erstklassige C#-Methoden, wobei die vertraute Syntax mit Intellisense, Typsicherheit und Refactoring kombiniert wird.</span><span class="sxs-lookup"><span data-stu-id="6550d-231">XLinq declares SQL statements (like `SELECT`, `FROM`, `WHERE`) as first class C# methods, combining familiar syntax with intellisense, type safety and refactoring.</span></span>

<span data-ttu-id="6550d-232">Infolgedessen wird SQL einfach zu einer „weiteren“ Klassenbibliothek, die ihre API lokal verfügbar macht, wörtlich *Sprachintegrierte SQL*.</span><span class="sxs-lookup"><span data-stu-id="6550d-232">As a result SQL becomes just "another" class library exposing its API locally, literally *"Language Integrated SQL"*.</span></span>

[<span data-ttu-id="6550d-233">Website</span><span class="sxs-lookup"><span data-stu-id="6550d-233">Website</span></span>](http://xlinq.live/)
