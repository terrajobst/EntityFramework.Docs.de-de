---
title: Tools und Erweiterungen – EF Core
author: ErikEJ
ms.date: 12/17/2019
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: bab725afffe1fbf9f8c0abeef58579ac9dc842d2
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502081"
---
# <a name="ef-core-tools--extensions"></a><span data-ttu-id="c9178-102">EF Core Tools und -Erweiterungen</span><span class="sxs-lookup"><span data-stu-id="c9178-102">EF Core Tools & Extensions</span></span>

<span data-ttu-id="c9178-103">Diese Tools und Erweiterungen bieten zusätzliche Funktionen für Entity Framework Core 2.1 und höher.</span><span class="sxs-lookup"><span data-stu-id="c9178-103">These tools and extensions provide additional functionality for Entity Framework Core 2.1 and later.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="c9178-104">Erweiterungen werden durch eine Vielzahl von Quellen erstellt und nicht als Teil des Entity Framework Core-Projekts verwaltet.</span><span class="sxs-lookup"><span data-stu-id="c9178-104">Extensions are built by a variety of sources and aren't maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="c9178-105">Wenn Sie die Erweiterung eines Drittanbieters in Betracht ziehen, sollten Sie Qualität, Lizenzierung, Kompatibilität, Support usw. auswerten, um sicherzustellen, dass diese Ihren Anforderungen entspricht.</span><span class="sxs-lookup"><span data-stu-id="c9178-105">When considering a third party extension, be sure to evaluate its quality, licensing, compatibility, support, etc. to ensure it meets your requirements.</span></span> <span data-ttu-id="c9178-106">Bei Erweiterungen, die für eine ältere EF Core-Version erstellt wurden, ist insbesondere darauf zu achten, dass sie möglicherweise aktualisiert werden müssen, bevor sie mit den neuesten Versionen kompatibel sind.</span><span class="sxs-lookup"><span data-stu-id="c9178-106">In particular, an extension built for an older version of EF Core may need updating before it will work with the latest versions.</span></span>

## <a name="tools"></a><span data-ttu-id="c9178-107">Tools</span><span class="sxs-lookup"><span data-stu-id="c9178-107">Tools</span></span>

### <a name="llblgen-pro"></a><span data-ttu-id="c9178-108">LLBLGen Pro</span><span class="sxs-lookup"><span data-stu-id="c9178-108">LLBLGen Pro</span></span>

<span data-ttu-id="c9178-109">LLBLGen Pro ist eine Entitätsmodelllösung, die Entity Framework und Entity Framework Core unterstützt.</span><span class="sxs-lookup"><span data-stu-id="c9178-109">LLBLGen Pro is an entity modeling solution with support for Entity Framework and Entity Framework Core.</span></span> <span data-ttu-id="c9178-110">Sie können problemlos ihr Entitätsmodell definieren und es Ihrer Datenbank zuordnen, indem Sie Database First oder Model First verwenden, sodass Sie sofort mit dem Schreiben von Abfragen beginnen können.</span><span class="sxs-lookup"><span data-stu-id="c9178-110">It lets you easily define your entity model and map it to your database, using database first or model first, so you can get started writing queries right away.</span></span> <span data-ttu-id="c9178-111">Für EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="c9178-111">For EF Core: 2.</span></span>

[<span data-ttu-id="c9178-112">Website</span><span class="sxs-lookup"><span data-stu-id="c9178-112">Website</span></span>](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a><span data-ttu-id="c9178-113">Devart Entity Developer</span><span class="sxs-lookup"><span data-stu-id="c9178-113">Devart Entity Developer</span></span>

<span data-ttu-id="c9178-114">Entity Developer ist ein leistungsstarker ORM-Designer für ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access und LINQ to SQL.</span><span class="sxs-lookup"><span data-stu-id="c9178-114">Entity Developer is a powerful ORM designer for ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access, and LINQ to SQL.</span></span> <span data-ttu-id="c9178-115">Sie können damit EF Core-Modelle mithilfe von Model First oder Database First visuell entwerfen sowie C#- oder Visual Basic-Code generieren.</span><span class="sxs-lookup"><span data-stu-id="c9178-115">It supports designing EF Core models visually, using model first or database first approaches, and C# or Visual Basic code generation.</span></span> <span data-ttu-id="c9178-116">Für EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="c9178-116">For EF Core: 2.</span></span>

[<span data-ttu-id="c9178-117">Website</span><span class="sxs-lookup"><span data-stu-id="c9178-117">Website</span></span>](https://www.devart.com/entitydeveloper/)

### <a name="ef-core-power-tools"></a><span data-ttu-id="c9178-118">EF Core Power Tools</span><span class="sxs-lookup"><span data-stu-id="c9178-118">EF Core Power Tools</span></span>

<span data-ttu-id="c9178-119">EF Core Power Tools ist eine Erweiterung von Visual Studio, die verschiedene Aufgaben von EF Core zur Entwurfszeit in einer einfachen Benutzeroberfläche verfügbar macht.</span><span class="sxs-lookup"><span data-stu-id="c9178-119">EF Core Power Tools is a Visual Studio extension that exposes various EF Core design-time tasks in a simple user interface.</span></span> <span data-ttu-id="c9178-120">Dadurch wird Reverse Engineering (Zurückentwicklung) von DbContext- und Entitätsklassen aus vorhandenen Datenbanken und [SQL Server-DACPACs](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), die Verwaltung von Datenbankmigrationen und das Vornehmen von Modellvisualisierungen ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="c9178-120">It includes reverse engineering of DbContext and entity classes from existing databases and [SQL Server DACPACs](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), management of database migrations, and model visualizations.</span></span> <span data-ttu-id="c9178-121">Für EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="c9178-121">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="c9178-122">GitHub-Wiki</span><span class="sxs-lookup"><span data-stu-id="c9178-122">GitHub wiki</span></span>](https://github.com/ErikEJ/EFCorePowerTools/wiki)

### <a name="entity-framework-visual-editor"></a><span data-ttu-id="c9178-123">Visueller Editor für Entity Framework</span><span class="sxs-lookup"><span data-stu-id="c9178-123">Entity Framework Visual Editor</span></span>

<span data-ttu-id="c9178-124">Entity Framework Visual Editor ist eine Erweiterung von Visual Studio, mit der ein ORM-Designer für visuelle Entwürfe von EF 6- und EF Core-Klassen hinzugefügt werden kann.</span><span class="sxs-lookup"><span data-stu-id="c9178-124">Entity Framework Visual Editor is a Visual Studio extension that adds an ORM designer for visual design of EF 6, and EF Core classes.</span></span> <span data-ttu-id="c9178-125">Code wird mithilfe von T4-Vorlagen generiert und kann somit vollständig an die Anforderungen des Benutzers angepasst werden.</span><span class="sxs-lookup"><span data-stu-id="c9178-125">Code is generated using T4 templates so can be customized to suit any needs.</span></span> <span data-ttu-id="c9178-126">Vererbung, uni- und bidirektionale Zuordnungen, Enumerationen, Farbcode für Klassen und das Hinzufügen von Textblöcken für Erklärungen zu möglicherweise schwer durchschaubaren Bereichen Ihres Designs werden unterstützt.</span><span class="sxs-lookup"><span data-stu-id="c9178-126">It supports inheritance, unidirectional and bidirectional associations, enumerations, and the ability to color-code your classes and add text blocks to explain potentially arcane parts of your design.</span></span> <span data-ttu-id="c9178-127">Für EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="c9178-127">For EF Core: 2.</span></span>

[<span data-ttu-id="c9178-128">Marketplace</span><span class="sxs-lookup"><span data-stu-id="c9178-128">Marketplace</span></span>](https://marketplace.visualstudio.com/items?itemName=michaelsawczyn.EFDesigner)

### <a name="catfactory"></a><span data-ttu-id="c9178-129">CatFactory</span><span class="sxs-lookup"><span data-stu-id="c9178-129">CatFactory</span></span>

<span data-ttu-id="c9178-130">CatFactory ist eine Gerüstbau-Engine für .NET Core, mit der die Generierung von DbContext-Klassen und -Entitäten, Zuordnungskonfigurationen und Repositoryklassen aus einer SQL Server-Datenbank automatisiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="c9178-130">CatFactory is a scaffolding engine for .NET Core that can automate the generation of DbContext classes, entities, mapping configurations, and repository classes from a SQL Server database.</span></span> <span data-ttu-id="c9178-131">Für EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="c9178-131">For EF Core: 2.</span></span>

[<span data-ttu-id="c9178-132">GitHub-Repository</span><span class="sxs-lookup"><span data-stu-id="c9178-132">GitHub repository</span></span>](https://github.com/hherzl/CatFactory.EntityFrameworkCore)

### <a name="loresofts-entity-framework-core-generator"></a><span data-ttu-id="c9178-133">Entity Framework Core-Generator von LoreSoft</span><span class="sxs-lookup"><span data-stu-id="c9178-133">LoreSoft's Entity Framework Core Generator</span></span>

<span data-ttu-id="c9178-134">Entity Framework Core-Generator (efg) ist ein .NET Core-CLI-Tool, mit dem EF Core-Modelle aus einer vorhandenen Datenbank generiert werden können, wie beispielsweise `dotnet ef dbcontext scaffold`. Es unterstützt aber auch die [Neugenerierung](https://efg.loresoft.com/en/latest/regeneration/) sicheren Codes durch das Ersetzen eines Bereichs oder durch das Analysieren von Zuordnungsdateien.</span><span class="sxs-lookup"><span data-stu-id="c9178-134">Entity Framework Core Generator (efg) is a .NET Core CLI tool that can generate EF Core models from an existing database, much like `dotnet ef dbcontext scaffold`, but it also supports safe code [regeneration](https://efg.loresoft.com/en/latest/regeneration/) via region replacement or by parsing mapping files.</span></span> <span data-ttu-id="c9178-135">Dieses Tool unterstützt das Generieren von Ansichtsmodellen, die Validierung und Objektzuordnungscode.</span><span class="sxs-lookup"><span data-stu-id="c9178-135">This tool supports generating view models, validation, and object mapper code.</span></span> <span data-ttu-id="c9178-136">Für EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="c9178-136">For EF Core: 2.</span></span>

<span data-ttu-id="c9178-137">[Tutorial](https://www.loresoft.com/Generate-ASP-NET-Web-API)
[Dokumentation](https://efg.loresoft.com/en/latest/)</span><span class="sxs-lookup"><span data-stu-id="c9178-137">[Tutorial](https://www.loresoft.com/Generate-ASP-NET-Web-API)
[Documentation](https://efg.loresoft.com/en/latest/)</span></span>

## <a name="extensions"></a><span data-ttu-id="c9178-138">Erweiterungen</span><span class="sxs-lookup"><span data-stu-id="c9178-138">Extensions</span></span>

### <a name="microsoftentityframeworkcoreautohistory"></a><span data-ttu-id="c9178-139">Microsoft.EntityFrameworkCore.AutoHistory</span><span class="sxs-lookup"><span data-stu-id="c9178-139">Microsoft.EntityFrameworkCore.AutoHistory</span></span>

<span data-ttu-id="c9178-140">Eine Plug-in-Bibliothek, mit deren Hilfe automatisch von EF Core durchgeführte Änderungen an den Daten in einer Verlaufstabelle aufgezeichnet werden.</span><span class="sxs-lookup"><span data-stu-id="c9178-140">A plugin library that enables automatically recording the data changes performed by EF Core into a history table.</span></span> <span data-ttu-id="c9178-141">Für EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="c9178-141">For EF Core: 2.</span></span>

[<span data-ttu-id="c9178-142">GitHub-Repository</span><span class="sxs-lookup"><span data-stu-id="c9178-142">GitHub repository</span></span>](https://github.com/Arch/AutoHistory/)

### <a name="efsecondlevelcachecore"></a><span data-ttu-id="c9178-143">EFSecondLevelCache.Core</span><span class="sxs-lookup"><span data-stu-id="c9178-143">EFSecondLevelCache.Core</span></span>

<span data-ttu-id="c9178-144">Eine Erweiterung, die es ermöglicht, die Ergebnisse von EF Core-Abfragen in einem Cache zweiter Ebene zu speichern, sodass nachfolgende Ausführungen der gleichen Abfragen nicht auf die Datenbank zugreifen müssen, sondern die Daten direkt aus dem Cache abrufen können.</span><span class="sxs-lookup"><span data-stu-id="c9178-144">An extension that enables storing the results of EF Core queries into a second-level cache, so that subsequent executions of the same queries can avoid accessing the database and retrieve the data directly from the cache.</span></span> <span data-ttu-id="c9178-145">Für EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="c9178-145">For EF Core: 2.</span></span>

[<span data-ttu-id="c9178-146">GitHub-Repository</span><span class="sxs-lookup"><span data-stu-id="c9178-146">GitHub repository</span></span>](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="geco"></a><span data-ttu-id="c9178-147">Geco</span><span class="sxs-lookup"><span data-stu-id="c9178-147">Geco</span></span>

<span data-ttu-id="c9178-148">Geco (Generator-Konsole) ist ein einfacher Code-Generator, der auf einem Konsolenprojekt basiert, das auf .NET Core ausgeführt wird und interpolierte Zeichenfolgen in C# für die Codegenerierung verwendet.</span><span class="sxs-lookup"><span data-stu-id="c9178-148">Geco (Generator Console) is a simple code generator based on a console project, that runs on .NET Core and uses C# interpolated strings for code generation.</span></span> <span data-ttu-id="c9178-149">Geco enthält einen Reversemodell-Generator für EF Core, der die Pluralisierung, Singularisierung und bearbeitbare Vorlagen unterstützt.</span><span class="sxs-lookup"><span data-stu-id="c9178-149">Geco includes a reverse model generator for EF Core with support for pluralization, singularization, and editable templates.</span></span> <span data-ttu-id="c9178-150">Er enthält auch einen Startdatenskript-Generator, einen Skript-Runner sowie eine Datenbankbereinigung.</span><span class="sxs-lookup"><span data-stu-id="c9178-150">It also provides a seed data script generator, a script runner, and a database cleaner.</span></span> <span data-ttu-id="c9178-151">Für EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="c9178-151">For EF Core: 2.</span></span>

[<span data-ttu-id="c9178-152">GitHub-Repository</span><span class="sxs-lookup"><span data-stu-id="c9178-152">GitHub repository</span></span>](https://github.com/iQuarc/Geco)

### <a name="entityframeworkcorescaffoldinghandlebars"></a><span data-ttu-id="c9178-153">EntityFrameworkCore.Scaffolding.Handlebars</span><span class="sxs-lookup"><span data-stu-id="c9178-153">EntityFrameworkCore.Scaffolding.Handlebars</span></span> 

<span data-ttu-id="c9178-154">Mit dieser Erweiterung können Klassen angepasst werden, die per Reverse Engineering mithilfe der Entity Framework Core-Toolkette mit Handlebars-Vorlagen aus einer vorhandenen Datenbank erstellt wurden.</span><span class="sxs-lookup"><span data-stu-id="c9178-154">Allows customization of classes reverse engineered from an existing database using the Entity Framework Core toolchain with Handlebars templates.</span></span> <span data-ttu-id="c9178-155">Für EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="c9178-155">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="c9178-156">GitHub-Repository</span><span class="sxs-lookup"><span data-stu-id="c9178-156">GitHub repository</span></span>](https://github.com/TrackableEntities/EntityFrameworkCore.Scaffolding.Handlebars)

### <a name="neinlinqentityframeworkcore"></a><span data-ttu-id="c9178-157">NeinLinq.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="c9178-157">NeinLinq.EntityFrameworkCore</span></span> 

<span data-ttu-id="c9178-158">NeinLinq ist eine Erweiterung für LINQ-Anbieter wie Entity Framework, um Funktionen wiederverwenden zu können, Abfragen erneut schreiben zu können und dynamische Abfragen mithilfe von übersetzbaren Prädikaten und Selektoren erstellen zu können.</span><span class="sxs-lookup"><span data-stu-id="c9178-158">NeinLinq extends LINQ providers such as Entity Framework to enable reusing functions, rewriting queries, and building dynamic queries using translatable predicates and selectors.</span></span> <span data-ttu-id="c9178-159">Für EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="c9178-159">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="c9178-160">GitHub-Repository</span><span class="sxs-lookup"><span data-stu-id="c9178-160">GitHub repository</span></span>](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a><span data-ttu-id="c9178-161">Microsoft.EntityFrameworkCore.UnitOfWork</span><span class="sxs-lookup"><span data-stu-id="c9178-161">Microsoft.EntityFrameworkCore.UnitOfWork</span></span>

<span data-ttu-id="c9178-162">Ein Plug-In für Microsoft.EntityFrameworkCore zur Unterstützung von Repositorys, Arbeitseinheitsmustern und mehreren Datenbanken, die verteilte Transaktionen unterstützen.</span><span class="sxs-lookup"><span data-stu-id="c9178-162">A plugin for Microsoft.EntityFrameworkCore to support repository, unit of work patterns, and multiple databases with distributed transaction supported.</span></span> <span data-ttu-id="c9178-163">Für EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="c9178-163">For EF Core: 2.</span></span>

[<span data-ttu-id="c9178-164">GitHub-Repository</span><span class="sxs-lookup"><span data-stu-id="c9178-164">GitHub repository</span></span>](https://github.com/Arch/UnitOfWork/)

### <a name="efcorebulkextensions"></a><span data-ttu-id="c9178-165">EFCore.BulkExtensions</span><span class="sxs-lookup"><span data-stu-id="c9178-165">EFCore.BulkExtensions</span></span>

<span data-ttu-id="c9178-166">EF Core-Erweiterungen für Massenvorgänge (Einfügen, Aktualisieren, Löschen)</span><span class="sxs-lookup"><span data-stu-id="c9178-166">EF Core extensions for Bulk operations (Insert, Update, Delete).</span></span> <span data-ttu-id="c9178-167">Für EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="c9178-167">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="c9178-168">GitHub-Repository</span><span class="sxs-lookup"><span data-stu-id="c9178-168">GitHub repository</span></span>](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a><span data-ttu-id="c9178-169">Bricelam.EntityFrameworkCore.Pluralizer</span><span class="sxs-lookup"><span data-stu-id="c9178-169">Bricelam.EntityFrameworkCore.Pluralizer</span></span>

<span data-ttu-id="c9178-170">Diese Erweiterung fügt die Pluralisierung zur Entwurfszeit hinzu.</span><span class="sxs-lookup"><span data-stu-id="c9178-170">Adds design-time pluralization.</span></span> <span data-ttu-id="c9178-171">Für EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="c9178-171">For EF Core: 2.</span></span>

[<span data-ttu-id="c9178-172">GitHub-Repository</span><span class="sxs-lookup"><span data-stu-id="c9178-172">GitHub repository</span></span>](https://github.com/bricelam/EFCore.Pluralizer)

### <a name="toolbeltentityframeworkcoreindexattribute"></a><span data-ttu-id="c9178-173">Toolbelt.EntityFrameworkCore.IndexAttribute</span><span class="sxs-lookup"><span data-stu-id="c9178-173">Toolbelt.EntityFrameworkCore.IndexAttribute</span></span>

<span data-ttu-id="c9178-174">Über diese Erweiterung ist das [Index]-Attribut mit der Erweiterung für die Modellerstellung wieder verfügbar.</span><span class="sxs-lookup"><span data-stu-id="c9178-174">Revival of [Index] attribute (with extension for model building).</span></span> <span data-ttu-id="c9178-175">Für EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="c9178-175">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="c9178-176">GitHub-Repository</span><span class="sxs-lookup"><span data-stu-id="c9178-176">GitHub repository</span></span>](https://github.com/jsakamoto/EntityFrameworkCore.IndexAttribute)

### <a name="efcoreinmemoryhelpers"></a><span data-ttu-id="c9178-177">EfCore.InMemoryHelpers</span><span class="sxs-lookup"><span data-stu-id="c9178-177">EfCore.InMemoryHelpers</span></span>

<span data-ttu-id="c9178-178">Bietet einen Wrapper, der den In-Memory-Datenbankanbieter für EF Core umschließt.</span><span class="sxs-lookup"><span data-stu-id="c9178-178">Provides a wrapper around the EF Core In-Memory Database Provider.</span></span> <span data-ttu-id="c9178-179">Fungiert dadurch mehr als relationaler Anbieter.</span><span class="sxs-lookup"><span data-stu-id="c9178-179">Makes it act more like a relational provider.</span></span> <span data-ttu-id="c9178-180">Für EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="c9178-180">For EF Core: 2.</span></span>

[<span data-ttu-id="c9178-181">GitHub-Repository</span><span class="sxs-lookup"><span data-stu-id="c9178-181">GitHub repository</span></span>](https://github.com/SimonCropp/EfCore.InMemoryHelpers)

### <a name="efcoretemporalsupport"></a><span data-ttu-id="c9178-182">EFCore.TemporalSupport</span><span class="sxs-lookup"><span data-stu-id="c9178-182">EFCore.TemporalSupport</span></span>

<span data-ttu-id="c9178-183">Mit dieser Erweiterung wird die Unterstützung temporaler Daten implementiert.</span><span class="sxs-lookup"><span data-stu-id="c9178-183">An implementation of temporal support.</span></span> <span data-ttu-id="c9178-184">Für EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="c9178-184">For EF Core: 2.</span></span>

[<span data-ttu-id="c9178-185">GitHub-Repository</span><span class="sxs-lookup"><span data-stu-id="c9178-185">GitHub repository</span></span>](https://github.com/cpoDesign/EFCore.TemporalSupport)

### <a name="efcoretemporaltable"></a><span data-ttu-id="c9178-186">EfCoreTemporalTable</span><span class="sxs-lookup"><span data-stu-id="c9178-186">EfCoreTemporalTable</span></span>

<span data-ttu-id="c9178-187">Mit dieser Erweiterung können Sie mithilfe bereits eingeführter Erweiterungsmethoden temporale Abfragen für eine Datenbank Ihrer Wahl ausführen: `AsTemporalAll()`, `AsTemporalAsOf(date)`, `AsTemporalFrom(startDate, endDate)`, `AsTemporalBetween(startDate, endDate)`, `AsTemporalContained(startDate, endDate)`.</span><span class="sxs-lookup"><span data-stu-id="c9178-187">Easily perform temporal queries on your favourite database using introduced extension methods: `AsTemporalAll()`, `AsTemporalAsOf(date)`, `AsTemporalFrom(startDate, endDate)`, `AsTemporalBetween(startDate, endDate)`, `AsTemporalContained(startDate, endDate)`.</span></span> <span data-ttu-id="c9178-188">Für EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="c9178-188">For EF Core: 3.</span></span>

[<span data-ttu-id="c9178-189">GitHub-Repository</span><span class="sxs-lookup"><span data-stu-id="c9178-189">GitHub repository</span></span>](https://github.com/glautrou/EfCoreTemporalTable)

### <a name="efcoretimetraveler"></a><span data-ttu-id="c9178-190">EFCore.TimeTraveler</span><span class="sxs-lookup"><span data-stu-id="c9178-190">EFCore.TimeTraveler</span></span>

<span data-ttu-id="c9178-191">Diese Erweiterung ermöglicht vollständige Entity Framework Core-Abfragen von [temporalen SQL Server-Verlaufsdaten](/sql/relational-databases/tables/temporal-table-usage-scenarios#point-in-time-analysis-time-travel). Dazu werden der von Ihnen bereits definierte EF Core-Code sowie die dazugehörigen Entitäten und Zuordnungen verwendet.</span><span class="sxs-lookup"><span data-stu-id="c9178-191">Allow full-featured Entity Framework Core queries against [SQL Server Temporal History](/sql/relational-databases/tables/temporal-table-usage-scenarios#point-in-time-analysis-time-travel) using the EF Core code, entities, and mappings you already have defined.</span></span>  <span data-ttu-id="c9178-192">Führen Sie Zeitpunktanalysen (Zeitreise) durch, indem Sie Ihren Code mithilfe der Anweisung `using (TemporalQuery.AsOf(targetDateTime)) {...}` umschließen.</span><span class="sxs-lookup"><span data-stu-id="c9178-192">Travel through time by wrapping your code in `using (TemporalQuery.AsOf(targetDateTime)) {...}`.</span></span> <span data-ttu-id="c9178-193">Für EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="c9178-193">For EF Core: 3.</span></span>

[<span data-ttu-id="c9178-194">GitHub-Repository</span><span class="sxs-lookup"><span data-stu-id="c9178-194">GitHub repository</span></span>](https://github.com/VantageSoftware/EFCore.TimeTraveler)


### <a name="entityframeworkcoretemporaltables"></a><span data-ttu-id="c9178-195">EntityFrameworkCore.TemporalTables</span><span class="sxs-lookup"><span data-stu-id="c9178-195">EntityFrameworkCore.TemporalTables</span></span>

<span data-ttu-id="c9178-196">Eine Erweiterungsbibliothek für Entity Framework Core, mit der Entwickler, die SQL Server nutzen, temporale Tabellen einfach verwenden können.</span><span class="sxs-lookup"><span data-stu-id="c9178-196">Extension library for Entity Framework Core which allows developers who use SQL Server to easily use temporal tables.</span></span> <span data-ttu-id="c9178-197">Für EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="c9178-197">For EF Core: 2.</span></span>

[<span data-ttu-id="c9178-198">GitHub-Repository</span><span class="sxs-lookup"><span data-stu-id="c9178-198">GitHub repository</span></span>](https://github.com/findulov/EntityFrameworkCore.TemporalTables)


### <a name="entityframeworkcorecacheable"></a><span data-ttu-id="c9178-199">EntityFrameworkCore.Cacheable</span><span class="sxs-lookup"><span data-stu-id="c9178-199">EntityFrameworkCore.Cacheable</span></span>

<span data-ttu-id="c9178-200">Ein Abfragecache auf zweiter Ebene mit hoher Leistung.</span><span class="sxs-lookup"><span data-stu-id="c9178-200">A high-performance second-level query cache.</span></span> <span data-ttu-id="c9178-201">Für EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="c9178-201">For EF Core: 2.</span></span>

[<span data-ttu-id="c9178-202">GitHub-Repository</span><span class="sxs-lookup"><span data-stu-id="c9178-202">GitHub repository</span></span>](https://github.com/SteffenMangold/EntityFrameworkCore.Cacheable)

### <a name="entity-framework-plus"></a><span data-ttu-id="c9178-203">Entity Framework Plus</span><span class="sxs-lookup"><span data-stu-id="c9178-203">Entity Framework Plus</span></span>

<span data-ttu-id="c9178-204">Erweitert Ihren DbContext mit Features wie den folgenden: Include Filter, Auditing, Caching, Query Future, Batch Delete, Batch Update und vielen weiteren.</span><span class="sxs-lookup"><span data-stu-id="c9178-204">Extends your DbContext with features such as: Include Filter, Auditing, Caching, Query Future, Batch Delete, Batch Update, and more.</span></span> <span data-ttu-id="c9178-205">Für EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="c9178-205">For EF Core: 2, 3.</span></span>

<span data-ttu-id="c9178-206">[Website](https://entityframework-plus.net/)
[GitHub-Repository](https://github.com/zzzprojects/EntityFramework-Plus)</span><span class="sxs-lookup"><span data-stu-id="c9178-206">[Website](https://entityframework-plus.net/)
[GitHub repository](https://github.com/zzzprojects/EntityFramework-Plus)</span></span>

### <a name="entity-framework-extensions"></a><span data-ttu-id="c9178-207">Entity Framework-Erweiterungen</span><span class="sxs-lookup"><span data-stu-id="c9178-207">Entity Framework Extensions</span></span>

<span data-ttu-id="c9178-208">Erweitert Ihren DbContext mit hochleistungsfähigen Massenvorgängen: BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge und vielen weiteren.</span><span class="sxs-lookup"><span data-stu-id="c9178-208">Extends your DbContext with high-performance bulk operations: BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge, and more.</span></span> <span data-ttu-id="c9178-209">Für EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="c9178-209">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="c9178-210">Website</span><span class="sxs-lookup"><span data-stu-id="c9178-210">Website</span></span>](https://entityframework-extensions.net/)
