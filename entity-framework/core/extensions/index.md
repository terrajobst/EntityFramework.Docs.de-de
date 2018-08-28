---
title: Tools und Erweiterungen – EF Core
author: ErikEJ
ms.date: 7/3/2018
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: e9f9a6cbbceeb0379ddb5588b564b0d2a962795f
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995512"
---
# <a name="ef-core-tools--extensions"></a><span data-ttu-id="7099a-102">EF Core Tools und -Erweiterungen</span><span class="sxs-lookup"><span data-stu-id="7099a-102">EF Core Tools & Extensions</span></span>

<span data-ttu-id="7099a-103">Tools und Erweiterungen bieten zusätzliche Funktionen für Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="7099a-103">Tools and extensions provide additional functionality for Entity Framework Core.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="7099a-104">Erweiterungen werden durch eine Vielzahl von Quellen erstellt und nicht als Teil des Entity Framework Core-Projekts verwaltet.</span><span class="sxs-lookup"><span data-stu-id="7099a-104">Extensions are built by a variety of sources and not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="7099a-105">Wenn Sie die Erweiterung eines Drittanbieters in Betracht ziehen, sollten Sie Qualität, Lizenzierung, Kompatibilität, Support usw. auswerten, um sicherzustellen, dass diese Ihren Anforderungen entspricht.</span><span class="sxs-lookup"><span data-stu-id="7099a-105">When considering a third party extension, be sure to evaluate quality, licensing, compatibility, support, etc. to ensure they meet your requirements.</span></span>

## <a name="tools"></a><span data-ttu-id="7099a-106">Tools</span><span class="sxs-lookup"><span data-stu-id="7099a-106">Tools</span></span>

### <a name="llblgen-pro"></a><span data-ttu-id="7099a-107">LLBLGen Pro</span><span class="sxs-lookup"><span data-stu-id="7099a-107">LLBLGen Pro</span></span>

<span data-ttu-id="7099a-108">LLBLGen Pro ist eine Entitätsmodelllösung, die Entity Framework und Entity Framework Core unterstützt.</span><span class="sxs-lookup"><span data-stu-id="7099a-108">LLBLGen Pro is an entity modeling solution with support for Entity Framework and Entity Framework Core.</span></span> <span data-ttu-id="7099a-109">Sie können problemlos ihr Entitätsmodell definieren und es Ihrer Datenbank zuordnen, indem Sie Database First oder Model First verwenden, sodass Sie sofort mit dem Schreiben von Abfragen beginnen können.</span><span class="sxs-lookup"><span data-stu-id="7099a-109">It lets you easily define your entity model and map it to your database, using database first or model first, so you can get started writing queries right away.</span></span>

[<span data-ttu-id="7099a-110">Website</span><span class="sxs-lookup"><span data-stu-id="7099a-110">website</span></span>](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a><span data-ttu-id="7099a-111">Devart Entity Developer</span><span class="sxs-lookup"><span data-stu-id="7099a-111">Devart Entity Developer</span></span>

<span data-ttu-id="7099a-112">Entity Developer ist ein leistungsstarker ORM-Designer für ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access und LINQ to SQL.</span><span class="sxs-lookup"><span data-stu-id="7099a-112">Entity Developer is a powerful ORM designer for ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access, and LINQ to SQL.</span></span> <span data-ttu-id="7099a-113">Sie können Model First- und Database First-Ansätze zum Entwerfen Ihres ORM-Modells verwenden und einen C#- oder Visual Basic .NET-Code für sie generieren.</span><span class="sxs-lookup"><span data-stu-id="7099a-113">You can use  Model-First and Database-First approaches to design your ORM model and generate C# or Visual Basic .NET code for it.</span></span> <span data-ttu-id="7099a-114">Es werden neue Ansätze für ORM-Modelle eingeführt, die Produktivität wird gesteigert, und die Entwicklung von Datenbankanwendungen wird erleichtert.</span><span class="sxs-lookup"><span data-stu-id="7099a-114">It introduces new approaches for designing ORM models, boosts productivity, and facilitates the development of database applications.</span></span>

[<span data-ttu-id="7099a-115">Website</span><span class="sxs-lookup"><span data-stu-id="7099a-115">website</span></span>](https://www.devart.com/entitydeveloper/)

### <a name="ef-core-power-tools"></a><span data-ttu-id="7099a-116">EF Core Power Tools</span><span class="sxs-lookup"><span data-stu-id="7099a-116">EF Core Power Tools</span></span>

<span data-ttu-id="7099a-117">Erweiterung von Visual Studio 2017 und höher</span><span class="sxs-lookup"><span data-stu-id="7099a-117">Visual Studio 2017+ extension.</span></span> <span data-ttu-id="7099a-118">Sie können DbContext- und POCO-Klassen einer existierenden Datenbank oder eines SQL Server-Datenbankprojekts zurückentwickeln und DbContext auf verschiedene Art und Weise visualisieren und überprüfen.</span><span class="sxs-lookup"><span data-stu-id="7099a-118">You can reverse engineer of DbContext and POCO classes from an existing database or SQL Server Database project, and visualize and inspect your DbContext in various ways.</span></span>

[<span data-ttu-id="7099a-119">GitHub-Wiki</span><span class="sxs-lookup"><span data-stu-id="7099a-119">GitHub wiki</span></span>](https://github.com/ErikEJ/SqlCeToolbox/wiki/EF-Core-Power-Tools)

## <a name="extensions"></a><span data-ttu-id="7099a-120">Erweiterungen</span><span class="sxs-lookup"><span data-stu-id="7099a-120">Extensions</span></span>

### <a name="microsoftentityframeworkcoreautohistory"></a><span data-ttu-id="7099a-121">Microsoft.EntityFrameworkCore.AutoHistory</span><span class="sxs-lookup"><span data-stu-id="7099a-121">Microsoft.EntityFrameworkCore.AutoHistory</span></span>

<span data-ttu-id="7099a-122">Ein Plug-In für Microsoft.EntityFrameworkCore zur Unterstützung der automatischen Aufzeichnung des Verlauf von Datenänderungen.</span><span class="sxs-lookup"><span data-stu-id="7099a-122">A plugin for Microsoft.EntityFrameworkCore to support automatically recording data changes history.</span></span>

[<span data-ttu-id="7099a-123">GitHub-Repository</span><span class="sxs-lookup"><span data-stu-id="7099a-123">GitHub repository</span></span>](https://github.com/Arch/AutoHistory/)

### <a name="microsoftentityframeworkcoredynamiclinq"></a><span data-ttu-id="7099a-124">Microsoft.EntityFrameworkCore.DynamicLinq</span><span class="sxs-lookup"><span data-stu-id="7099a-124">Microsoft.EntityFrameworkCore.DynamicLinq</span></span>

<span data-ttu-id="7099a-125">Dynamic Linq-Erweiterungen für Microsoft.EntityFrameworkCore, das asynchrone Unterstützung hinzufügt.</span><span class="sxs-lookup"><span data-stu-id="7099a-125">Dynamic Linq extensions for Microsoft.EntityFrameworkCore which adds Async support</span></span>

 [<span data-ttu-id="7099a-126">GitHub-Repository</span><span class="sxs-lookup"><span data-stu-id="7099a-126">GitHub repository</span></span>](https://github.com/StefH/System.Linq.Dynamic.Core/)

### <a name="efcorepractices"></a><span data-ttu-id="7099a-127">EFCore.Practices</span><span class="sxs-lookup"><span data-stu-id="7099a-127">EFCore.Practices</span></span>

<span data-ttu-id="7099a-128">Versuchen Sie gute oder bewährte Methoden in einer Tests unterstützenden API zu erfassen, einschließlich eines kleinen Frameworks für die Suche nach N+1-Abfragen.</span><span class="sxs-lookup"><span data-stu-id="7099a-128">Attempt to capture some good or best practices in an API that supports testing – including a small framework to scan for N+1 queries.</span></span>

[<span data-ttu-id="7099a-129">GitHub-Repository</span><span class="sxs-lookup"><span data-stu-id="7099a-129">GitHub repository</span></span>](https://github.com/riezebosch/efcore-practices/tree/master/src/EFCore.Practices/)

### <a name="efsecondlevelcachecore"></a><span data-ttu-id="7099a-130">EFSecondLevelCache.Core</span><span class="sxs-lookup"><span data-stu-id="7099a-130">EFSecondLevelCache.Core</span></span>

<span data-ttu-id="7099a-131">Zwischenspeicherungsbibliothek zweiter Ebene</span><span class="sxs-lookup"><span data-stu-id="7099a-131">Second Level Caching Library.</span></span> <span data-ttu-id="7099a-132">Das Zwischenspeichern zweiter Ebene ist ein Abfragecache.</span><span class="sxs-lookup"><span data-stu-id="7099a-132">Second level caching is a query cache.</span></span> <span data-ttu-id="7099a-133">Die Ergebnisse von EF-Befehlen werden im Cache gespeichert, sodass die gleichen EF-Befehle ihre Daten eher aus dem Cache abrufen, als die Datenbank erneut zu durchsuchen.</span><span class="sxs-lookup"><span data-stu-id="7099a-133">The results of EF commands will be stored in the cache, so that the same EF commands will retrieve their data from the cache rather than executing them against the database again.</span></span>

[<span data-ttu-id="7099a-134">GitHub-Repository</span><span class="sxs-lookup"><span data-stu-id="7099a-134">GitHub repository</span></span>](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="detachedentityframework"></a><span data-ttu-id="7099a-135">Detached.EntityFramework</span><span class="sxs-lookup"><span data-stu-id="7099a-135">Detached.EntityFramework</span></span>

<span data-ttu-id="7099a-136">Lädt und speichert die gesamten getrennten Entitätsdiagramme (die Entität mit ihren untergeordneten Entitäten und Listen).</span><span class="sxs-lookup"><span data-stu-id="7099a-136">Loads and saves entire detached entity graphs (the entity with their child entities and lists).</span></span> <span data-ttu-id="7099a-137">Inspiriert von [GraphDiff](https://github.com/refactorthis/GraphDiff/).</span><span class="sxs-lookup"><span data-stu-id="7099a-137">Inspired by [GraphDiff](https://github.com/refactorthis/GraphDiff/).</span></span> <span data-ttu-id="7099a-138">Es sollen einige Plug-Ins hinzugefügt werden, um sich wiederholende Aufgaben wie eine Überprüfung oder Paginierung zu vereinfachen.</span><span class="sxs-lookup"><span data-stu-id="7099a-138">The idea is also add some plugins to simplificate some repetitive tasks, like auditing and pagination.</span></span>

[<span data-ttu-id="7099a-139">GitHub-Repository</span><span class="sxs-lookup"><span data-stu-id="7099a-139">GitHub repository</span></span>](https://github.com/leonardoporro/Detached/)

### <a name="entityframeworkcoreprimarykey"></a><span data-ttu-id="7099a-140">EntityFrameworkCore.PrimaryKey</span><span class="sxs-lookup"><span data-stu-id="7099a-140">EntityFrameworkCore.PrimaryKey</span></span>

<span data-ttu-id="7099a-141">Den Primärschlüssel (inklusive zusammengesetzter Schlüssel) einer Entität als Wörterbuch abrufen.</span><span class="sxs-lookup"><span data-stu-id="7099a-141">Retrieve the primary key (including composite keys) from any entity as a dictionary.</span></span>

[<span data-ttu-id="7099a-142">GitHub-Repository</span><span class="sxs-lookup"><span data-stu-id="7099a-142">GitHub repository</span></span>](https://github.com/NickStrupat/EntityFramework.PrimaryKey/)

### <a name="entityframeworkcorerx"></a><span data-ttu-id="7099a-143">EntityFrameworkCore.Rx</span><span class="sxs-lookup"><span data-stu-id="7099a-143">EntityFrameworkCore.Rx</span></span>

<span data-ttu-id="7099a-144">Reaktive Erweiterungswrapper für heiße Observables von Entity Framework-Entitäten.</span><span class="sxs-lookup"><span data-stu-id="7099a-144">Reactive extension wrappers for hot observables of Entity Framework entities.</span></span>

[<span data-ttu-id="7099a-145">GitHub-Repository</span><span class="sxs-lookup"><span data-stu-id="7099a-145">GitHub repository</span></span>](https://github.com/NickStrupat/EntityFramework.Rx/)

### <a name="entityframeworkcoretriggers"></a><span data-ttu-id="7099a-146">EntityFrameworkCore.Triggers</span><span class="sxs-lookup"><span data-stu-id="7099a-146">EntityFrameworkCore.Triggers</span></span>

<span data-ttu-id="7099a-147">Fügen Sie Ihren Entitäten mit Einfügen-, Aktualisieren-, und Löschereignissen Trigger hinzu.</span><span class="sxs-lookup"><span data-stu-id="7099a-147">Add triggers to your entities with insert, update, and delete events.</span></span> <span data-ttu-id="7099a-148">Für jeden gibt es drei Ereignisse: vor, nach und bei Fehler.</span><span class="sxs-lookup"><span data-stu-id="7099a-148">There are three events for each: before, after, and upon failure.</span></span>

[<span data-ttu-id="7099a-149">GitHub-Repository</span><span class="sxs-lookup"><span data-stu-id="7099a-149">GitHub repository</span></span>](https://github.com/NickStrupat/EntityFramework.Triggers/)

### <a name="entityframeworkcoretypedoriginalvalues"></a><span data-ttu-id="7099a-150">EntityFrameworkCore.TypedOriginalValues</span><span class="sxs-lookup"><span data-stu-id="7099a-150">EntityFrameworkCore.TypedOriginalValues</span></span>

<span data-ttu-id="7099a-151">Erhalten Sie typisierten Zugriff auf den OriginalValue Ihrer Entitätseigenschaften.</span><span class="sxs-lookup"><span data-stu-id="7099a-151">Get typed access to the OriginalValue of your entity properties.</span></span> <span data-ttu-id="7099a-152">Im Gegensatz zu Navigation und Auflistungen werden einfache und komplexe Eigenschaften unterstützt.</span><span class="sxs-lookup"><span data-stu-id="7099a-152">Simple and complex properties are supported, navigation/collections are not.</span></span>

[<span data-ttu-id="7099a-153">GitHub-Repository</span><span class="sxs-lookup"><span data-stu-id="7099a-153">GitHub repository</span></span>](https://github.com/NickStrupat/EntityFramework.TypedOriginalValues/)

### <a name="geco"></a><span data-ttu-id="7099a-154">Geco</span><span class="sxs-lookup"><span data-stu-id="7099a-154">Geco</span></span>

<span data-ttu-id="7099a-155">Geco stellt einen Reversemodellgenerator bereit, der sowohl die Pluralisierung und Singularisierung, als auch bearbeitbare Vorlagen unterstützt, die auf von C# 6.0 interpolierten Zeichenfolgen basieren und unter .Net Core ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="7099a-155">Geco provides a Reverse Model generator with support for Pluralization/Singularization and editable templates based on C# 6.0 interpolated strings and running on .Net Core.</span></span> <span data-ttu-id="7099a-156">Es stellt auch einen Seed-Skript-Generator mit SQL-Mergeskripten und einem Skript Runner zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="7099a-156">It also provides an Seed script generator with SQL Merge scripts and an script runner.</span></span>

[<span data-ttu-id="7099a-157">GitHub-Repository</span><span class="sxs-lookup"><span data-stu-id="7099a-157">Github repository</span></span>](https://github.com/iQuarc/Geco)

### <a name="linqkitmicrosoftentityframeworkcore"></a><span data-ttu-id="7099a-158">LinqKit.Microsoft.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="7099a-158">LinqKit.Microsoft.EntityFrameworkCore</span></span>

<span data-ttu-id="7099a-159">LinqKit.Microsoft.EntityFrameworkCore sind Erweiterungen für Hauptbenutzer von LINQ to SQL und EntityFrameworkCore.</span><span class="sxs-lookup"><span data-stu-id="7099a-159">LinqKit.Microsoft.EntityFrameworkCore is a free set of extensions for LINQ to SQL and EntityFrameworkCore power users.</span></span> <span data-ttu-id="7099a-160">Mit Include(...)- und IDbAsync-Support</span><span class="sxs-lookup"><span data-stu-id="7099a-160">With Include(...) and IDbAsync support.</span></span>

[<span data-ttu-id="7099a-161">GitHub-Repository</span><span class="sxs-lookup"><span data-stu-id="7099a-161">GitHub repository</span></span>](https://github.com/scottksmith95/LINQKit/)

### <a name="neinlinqentityframeworkcore"></a><span data-ttu-id="7099a-162">NeinLinq.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="7099a-162">NeinLinq.EntityFrameworkCore</span></span>

<span data-ttu-id="7099a-163">NeinLinq.EntityFrameworkCore stellt hilfreiche Erweiterungen für die Verwendung von LINQ-Anbietern, wie z.B. Entity Framework, bereit, die nur eine geringe Teilmenge von .NET-Funktionen unterstützen, Funktionen wiederverwenden, Abfragen neu schreiben, sie sogar NULL-sicher machen und mithilfe von übersetzbaren Prädikaten und Selektoren dynamische Abfragen erstellen.</span><span class="sxs-lookup"><span data-stu-id="7099a-163">NeinLinq.EntityFrameworkCore provides helpful extensions for using LINQ providers such as Entity Framework that support only a minor subset of .NET functions, reusing functions, rewriting queries, even making them null-safe, and building dynamic queries using translatable predicates and selectors.</span></span>

[<span data-ttu-id="7099a-164">GitHub-Repository</span><span class="sxs-lookup"><span data-stu-id="7099a-164">GitHub repository</span></span>](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a><span data-ttu-id="7099a-165">Microsoft.EntityFrameworkCore.UnitOfWork</span><span class="sxs-lookup"><span data-stu-id="7099a-165">Microsoft.EntityFrameworkCore.UnitOfWork</span></span>

<span data-ttu-id="7099a-166">Ein Plug-In für Microsoft.EntityFrameworkCore zur Unterstützung von Repositorys, Arbeitseinheitenmustern und mehreren Datenbanken, die verteilte Transaktionen unterstützen.</span><span class="sxs-lookup"><span data-stu-id="7099a-166">A plugin for Microsoft.EntityFrameworkCore to support repository, unit of work patterns, and multiple database with distributed transaction supported.</span></span>

[<span data-ttu-id="7099a-167">GitHub-Repository</span><span class="sxs-lookup"><span data-stu-id="7099a-167">GitHub repository</span></span>](https://github.com/Arch/UnitOfWork/)

### <a name="entityframeworklazyloading"></a><span data-ttu-id="7099a-168">EntityFramework.LazyLoading</span><span class="sxs-lookup"><span data-stu-id="7099a-168">EntityFramework.LazyLoading</span></span>

<span data-ttu-id="7099a-169">Lazy Loading für EF Core 1.1</span><span class="sxs-lookup"><span data-stu-id="7099a-169">Lazy Loading for EF Core 1.1</span></span>

[<span data-ttu-id="7099a-170">GitHub-Repository</span><span class="sxs-lookup"><span data-stu-id="7099a-170">GitHub repository</span></span>](https://github.com/darxis/EntityFramework.LazyLoading)

### <a name="efcorebulkextensions"></a><span data-ttu-id="7099a-171">EFCore.BulkExtensions</span><span class="sxs-lookup"><span data-stu-id="7099a-171">EFCore.BulkExtensions</span></span>

<span data-ttu-id="7099a-172">EntityFrameworkCore-Erweiterungen für Massenvorgänge (Einfügen, Aktualisieren, Löschen)</span><span class="sxs-lookup"><span data-stu-id="7099a-172">EntityFrameworkCore extensions for Bulk operations (Insert, Update, Delete).</span></span>

[<span data-ttu-id="7099a-173">GitHub-Repository</span><span class="sxs-lookup"><span data-stu-id="7099a-173">GitHub repository</span></span>](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a><span data-ttu-id="7099a-174">Bricelam.EntityFrameworkCore.Pluralizer</span><span class="sxs-lookup"><span data-stu-id="7099a-174">Bricelam.EntityFrameworkCore.Pluralizer</span></span>

<span data-ttu-id="7099a-175">Fügt EF Core die Pluralisierung zur Entwurfszeit hinzu.</span><span class="sxs-lookup"><span data-stu-id="7099a-175">Adds design-time pluralization to EF Core.</span></span>

[<span data-ttu-id="7099a-176">GitHub-Repository</span><span class="sxs-lookup"><span data-stu-id="7099a-176">GitHub repository</span></span>](https://github.com/bricelam/EFCore.Pluralizer)
