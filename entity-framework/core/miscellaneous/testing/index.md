---
title: Testen von Komponenten mit EF Core
description: Unterschiedliche Ansätze zum Testen von Anwendungen, die EF Core verwenden
author: ajcvickers
ms.date: 03/23/2020
uid: core/miscellaneous/testing/index
ms.openlocfilehash: b1ab37ebb0a3aae09d5d5b225f746cf83dfba170
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/07/2020
ms.locfileid: "80634250"
---
# <a name="testing-code-that-uses-ef-core"></a><span data-ttu-id="8c7e4-103">Testen von Code, der EF Core verwendet</span><span class="sxs-lookup"><span data-stu-id="8c7e4-103">Testing code that uses EF Core</span></span>

<span data-ttu-id="8c7e4-104">Das Testen von Code, der auf eine Datenbank zugreift, erfordert Folgendes:</span><span class="sxs-lookup"><span data-stu-id="8c7e4-104">Testing code that accesses a database requires either:</span></span>
* <span data-ttu-id="8c7e4-105">Das Ausführen von Abfragen und Aktualisierungen in dem Datenbanksystem, das auch in der Produktion verwendet wird, oder</span><span class="sxs-lookup"><span data-stu-id="8c7e4-105">Running queries and updates against the same database system used in production.</span></span>
* <span data-ttu-id="8c7e4-106">Das Ausführen von Abfragen und Aktualisierungen in einem andere einfacher zu verwaltenden Datenbanksystem oder</span><span class="sxs-lookup"><span data-stu-id="8c7e4-106">Running queries and updates against some other easier to manage database system.</span></span>
* <span data-ttu-id="8c7e4-107">Das Verwenden von Testdoubles oder eines anderen Mechanismus, um die Verwendung einer Datenbank grundsätzlich zu vermeiden</span><span class="sxs-lookup"><span data-stu-id="8c7e4-107">Using test doubles or some other mechanism to avoid using a database at all.</span></span>

<span data-ttu-id="8c7e4-108">In diesem Dokument werden die Abwägungen dargelegt, die mit jeder dieser Optionen verbunden sind, und es wird gezeigt, wie EF Core bei jedem Ansatz eingesetzt werden kann.</span><span class="sxs-lookup"><span data-stu-id="8c7e4-108">This document outlines the trade offs involved in each of these choices and shows how EF Core can be used with each approach.</span></span>  

## <a name="all-database-providers-are-not-equal"></a><span data-ttu-id="8c7e4-109">Alle Datenbankanbieter sind nicht gleich</span><span class="sxs-lookup"><span data-stu-id="8c7e4-109">All database providers are not equal</span></span>

<span data-ttu-id="8c7e4-110">Es ist sehr wichtig zu verstehen, dass EF Core nicht darauf ausgelegt ist, jeden Aspekt des zugrunde liegenden Datenbanksystems zu abstrahieren.</span><span class="sxs-lookup"><span data-stu-id="8c7e4-110">It is very important to understand that EF Core is not designed to abstract every aspect of the underlying database system.</span></span>
<span data-ttu-id="8c7e4-111">Stattdessen handelt es sich bei EF Core um einen einheitlichen Satz von Mustern und Konzepten, der bei jedem Datenbanksystem verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="8c7e4-111">Instead, EF Core is a common set of patterns and concepts that can be used with any database system.</span></span>
<span data-ttu-id="8c7e4-112">EF Core-Datenbankanbieter schichten dann datenbankspezifisches Verhalten und Funktionalität über diesem allgemeinen Framework.</span><span class="sxs-lookup"><span data-stu-id="8c7e4-112">EF Core database providers then layer database-specific behavior and functionality over this common framework.</span></span>
<span data-ttu-id="8c7e4-113">Auf diese Weise kann jedes Datenbanksystem das tun, was es am besten kann, während gleichzeitig die Gemeinsamkeiten mit anderen Datenbanksystemen, falls angebracht, gewahrt bleiben.</span><span class="sxs-lookup"><span data-stu-id="8c7e4-113">This allows each database system to do what it does best while still maintaining commonality, where appropriate, with other database systems.</span></span> 

<span data-ttu-id="8c7e4-114">Grundsätzlich bedeutet dies, dass ein Austauschen des Datenbankanbieters das Verhalten von EF Core verändert, und es kann nicht erwartet werden, dass die Anwendung ordnungsgemäß funktioniert, wenn sie nicht explizit alle Unterschiede im Verhalten berücksichtigt.</span><span class="sxs-lookup"><span data-stu-id="8c7e4-114">Fundamentally, this means that switching out the database provider will change EF Core behavior and the application can't be expected to function correctly unless it explicitly accounts for all differences in behavior.</span></span>
<span data-ttu-id="8c7e4-115">Dennoch wird dies in vielen Fällen funktionieren, weil es ein hohes Maß an Gemeinsamkeiten unter relationalen Datenbanken gibt.</span><span class="sxs-lookup"><span data-stu-id="8c7e4-115">That being said, in many cases doing this will work because there is a high degree of commonality amongst relational databases.</span></span>
<span data-ttu-id="8c7e4-116">Das ist gut und schlecht zugleich.</span><span class="sxs-lookup"><span data-stu-id="8c7e4-116">This is good and bad.</span></span>
<span data-ttu-id="8c7e4-117">Gut, da das Wechseln von Datenbanken relativ einfach erfolgen kann.</span><span class="sxs-lookup"><span data-stu-id="8c7e4-117">Good because moving between databases can be relatively easy.</span></span>
<span data-ttu-id="8c7e4-118">Schlecht, weil es ein falsches Gefühl der Sicherheit vermitteln kann, wenn die Anwendung nicht vollständig mit dem neuen Datenbanksystem getestet wird.</span><span class="sxs-lookup"><span data-stu-id="8c7e4-118">Bad because it can give a false sense of security if the application is not fully tested against the new database system.</span></span>  

## <a name="approach-1-production-database-system"></a><span data-ttu-id="8c7e4-119">Ansatz 1: Produktions-Datenbanksystem</span><span class="sxs-lookup"><span data-stu-id="8c7e4-119">Approach 1: Production database system</span></span>

<span data-ttu-id="8c7e4-120">Wie im vorigen Abschnitt beschrieben, ist die einzige Möglichkeit, sicher zu sein, dass das getestet wird, was in der Produktion ausgeführt wird, die Verwendung desselben Datenbanksystems.</span><span class="sxs-lookup"><span data-stu-id="8c7e4-120">As described in the previous section, the only way to be sure you are testing what runs in production is to use the same database system.</span></span>
<span data-ttu-id="8c7e4-121">Wenn die bereitgestellte Anwendung beispielsweise SQL Azure verwendet, müssen die Tests auch mit SQL Azure erfolgen.</span><span class="sxs-lookup"><span data-stu-id="8c7e4-121">For example, if the deployed application uses SQL Azure, then testing should also be done against SQL Azure.</span></span>

<span data-ttu-id="8c7e4-122">Allerdings wäre es sowohl langsam als auch teuer, wenn jeder Entwickler Tests mit SQL Azure durchführen würde, während er aktiv am Code arbeitet.</span><span class="sxs-lookup"><span data-stu-id="8c7e4-122">However, having every developer run tests against SQL Azure while actively working on the code would be both slow and expensive.</span></span>
<span data-ttu-id="8c7e4-123">Dies veranschaulicht den wesentlichen Haken bei diesen Ansätzen: Wann ist es angebracht, vom Produktions-Datenbanksystem abzuweichen, um die Testeffizienz zu verbessern?</span><span class="sxs-lookup"><span data-stu-id="8c7e4-123">This illustrates the main trade off involved throughout these approaches: when is it appropriate to deviate from the production database system so as to improve test efficiency?</span></span>

<span data-ttu-id="8c7e4-124">Glücklicherweise ist die Antwort in diesem Fall recht einfach, nämlich das Verwenden einer lokal SQL Server-Instanz für Entwicklertests.</span><span class="sxs-lookup"><span data-stu-id="8c7e4-124">Luckily, in this case the answer is quite easy: use local or on-premises SQL Server for developer testing.</span></span>
<span data-ttu-id="8c7e4-125">SQL Azure und SQL Server sind sehr ähnlich, sodass das Testen mit SQL Server in der Regel ein angemessener Kompromiss ist.</span><span class="sxs-lookup"><span data-stu-id="8c7e4-125">SQL Azure and SQL Server are extremely similar, so testing against SQL Server is usually a reasonable trade off.</span></span>
<span data-ttu-id="8c7e4-126">Dennoch ist es ratsam, vor dem Aufnehmen der Produktion Tests mit SQL Azure selbst durchzuführen.</span><span class="sxs-lookup"><span data-stu-id="8c7e4-126">That being said, it is still wise to run tests against SQL Azure itself before going into production.</span></span>
 
### <a name="localdb"></a><span data-ttu-id="8c7e4-127">LocalDb</span><span class="sxs-lookup"><span data-stu-id="8c7e4-127">LocalDb</span></span> 

<span data-ttu-id="8c7e4-128">Alle wichtigen Datenbanksysteme verfügen über eine Form von „Developer Edition“ für lokales Testen.</span><span class="sxs-lookup"><span data-stu-id="8c7e4-128">All the major database systems have some form of "Developer Edition" for local testing.</span></span>
<span data-ttu-id="8c7e4-129">Bei SQL Server heißt dieses Feature [LocalDb](/sql/database-engine/configure-windows/sql-server-express-localdb?view=sql-server-ver15).</span><span class="sxs-lookup"><span data-stu-id="8c7e4-129">SQL Server also also has a feature called [LocalDb](/sql/database-engine/configure-windows/sql-server-express-localdb?view=sql-server-ver15).</span></span>
<span data-ttu-id="8c7e4-130">Der Hauptvorteil von LocalDb besteht darin, dass die Datenbankinstanz bei Bedarf hochgefahren wird.</span><span class="sxs-lookup"><span data-stu-id="8c7e4-130">The primary advantage of LocalDb is that it spins up the database instance on demand.</span></span>
<span data-ttu-id="8c7e4-131">Dadurch wird vermieden, dass ein Datenbankdienst auf Ihrem Computer läuft, auch wenn gerade keine Tests ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="8c7e4-131">This avoids having a database service running on your machine even when you're not running tests.</span></span>

<span data-ttu-id="8c7e4-132">LocalDb ist nicht ohne Probleme:</span><span class="sxs-lookup"><span data-stu-id="8c7e4-132">LocalDb is not without it's issues:</span></span>
* <span data-ttu-id="8c7e4-133">Sie unterstützt nicht alles, was von [SQL Server Developer Edition](/sql/sql-server/editions-and-components-of-sql-server-2016?view=sql-server-ver15) unterstützt wird.</span><span class="sxs-lookup"><span data-stu-id="8c7e4-133">It doesn't support everything that [SQL Server Developer Edition](/sql/sql-server/editions-and-components-of-sql-server-2016?view=sql-server-ver15) does.</span></span>
* <span data-ttu-id="8c7e4-134">Sie ist unter Linux nicht verfügbar.</span><span class="sxs-lookup"><span data-stu-id="8c7e4-134">It isn't available on Linux.</span></span>
* <span data-ttu-id="8c7e4-135">Sie kann beim ersten Testlauf Verzögerungen verursachen, wenn der Dienst hochgefahren wird.</span><span class="sxs-lookup"><span data-stu-id="8c7e4-135">It can cause lag on first test run as the service is spun up.</span></span>

<span data-ttu-id="8c7e4-136">Ich persönlich habe es nie als Problem empfunden, dass ein Datenbankdienst auf meinem Entwicklungscomputer läuft, und würde generell empfehlen, stattdessen die Developer Edition zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="8c7e4-136">Personally, I've never found it a problem having a database service running on my dev machine and I would generally recommend using Developer Edition instead.</span></span>
<span data-ttu-id="8c7e4-137">Allerdings kann dies für manche Entwickler geeignet sein, insbesondere auf weniger leistungsfähigen Entwicklungscomputern.</span><span class="sxs-lookup"><span data-stu-id="8c7e4-137">However, it may be appropriate for some people, especially on less powerful dev machines.</span></span>  

## <a name="approach-2-sqlite"></a><span data-ttu-id="8c7e4-138">Ansatz 2: SQLite</span><span class="sxs-lookup"><span data-stu-id="8c7e4-138">Approach 2: SQLite</span></span>

<span data-ttu-id="8c7e4-139">EF Core testet den SQL Server-Anbieter hauptsächlich durch Ausführen in einer lokalen SQL Server-Instanz.</span><span class="sxs-lookup"><span data-stu-id="8c7e4-139">EF Core tests the SQL Server provider primarily by running it against a local SQL Server instance.</span></span>
<span data-ttu-id="8c7e4-140">Bei diesen Tests werden in ein paar Minuten Zehntausende von Abfragen auf einem schnellen Computer ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="8c7e4-140">These tests run tens of thousands of queries in a couple of minutes on a fast machine.</span></span>
<span data-ttu-id="8c7e4-141">Dies veranschaulicht, dass die Verwendung des realen Datenbanksystems eine leistungsstarke Lösung sein kann.</span><span class="sxs-lookup"><span data-stu-id="8c7e4-141">This illustrates that using the real database system can be a performant solution.</span></span>
<span data-ttu-id="8c7e4-142">Es ist ein Mythos, dass die Verwendung einer schlankeren Datenbank die einzige Möglichkeit ist, Tests schnell durchzuführen.</span><span class="sxs-lookup"><span data-stu-id="8c7e4-142">It is a myth that using some lighter-weight database is the only way to run tests quickly.</span></span>

<span data-ttu-id="8c7e4-143">Davon abgesehen, was ist, wenn Sie aus welchem Grund auch immer keine Tests mit etwas durchführen können, das Ihrem Produktions-Datenbanksystem nahe kommt?</span><span class="sxs-lookup"><span data-stu-id="8c7e4-143">That being said, what if for whatever reason you can't run tests against something close to your production database system?</span></span>
<span data-ttu-id="8c7e4-144">Die nächstbeste Wahl ist die Verwendung von etwas mit ähnlicher Funktionalität.</span><span class="sxs-lookup"><span data-stu-id="8c7e4-144">The next best choice is to use something with similar functionality.</span></span>
<span data-ttu-id="8c7e4-145">Dies bedeutet in der Regel eine andere relationale Datenbank, wofür [SQLite](https://sqlite.org/index.html) die offensichtliche Wahl ist.</span><span class="sxs-lookup"><span data-stu-id="8c7e4-145">This usually means another relational database, for which [SQLite](https://sqlite.org/index.html) is the obvious choice.</span></span>

<span data-ttu-id="8c7e4-146">SQLite ist aus folgenden Gründen eine gute Wahl:</span><span class="sxs-lookup"><span data-stu-id="8c7e4-146">SQLite is a good choice because:</span></span>
* <span data-ttu-id="8c7e4-147">Sie wird In-Process mit Ihrer Anwendung ausgeführt und hat daher einen geringen Verarbeitungsaufwand.</span><span class="sxs-lookup"><span data-stu-id="8c7e4-147">It runs in-process with your application and so has low overhead.</span></span>
* <span data-ttu-id="8c7e4-148">Sie verwendet einfache, automatisch erstellte Dateien für Datenbanken und erfordert daher keine Datenbankverwaltung.</span><span class="sxs-lookup"><span data-stu-id="8c7e4-148">It uses simple, automatically created files for databases, and so doesn't require database management.</span></span>
* <span data-ttu-id="8c7e4-149">Sie hat einen InMemory-Modus, der sogar die Dateierstellung vermeidet.</span><span class="sxs-lookup"><span data-stu-id="8c7e4-149">It has an in-memory mode that avoids even the file creation.</span></span>

<span data-ttu-id="8c7e4-150">Beachten Sie jedoch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="8c7e4-150">However, remember that:</span></span>
* <span data-ttu-id="8c7e4-151">SQLite unterstützt nicht zwangsläufig alles, was Ihr Produktions-Datenbanksystem leistet.</span><span class="sxs-lookup"><span data-stu-id="8c7e4-151">SQLite inevitability doesn't support everything that your production database system does.</span></span>
* <span data-ttu-id="8c7e4-152">SQLite verhält sich bei einigen Abfragen anders als Ihr Produktions-Datenbanksystem.</span><span class="sxs-lookup"><span data-stu-id="8c7e4-152">SQLite will behave differently than your production database system for some queries.</span></span>

<span data-ttu-id="8c7e4-153">Wenn Sie also SQLite für einige Tests verwenden, stellen Sie sicher, dass Sie die Test auch mit Ihrem tatsächlichen Datenbanksystem durchführen.</span><span class="sxs-lookup"><span data-stu-id="8c7e4-153">So if you do use SQLite for some testing, make sure to also test against your real database system.</span></span>

<span data-ttu-id="8c7e4-154">Unter [Testen mit SQLite](xref:core/miscellaneous/testing/sqlite) finden Sie eine EF Core-spezifische Anleitung.</span><span class="sxs-lookup"><span data-stu-id="8c7e4-154">See [Testing with SQLite](xref:core/miscellaneous/testing/sqlite) for EF Core specific guidance.</span></span> 

## <a name="approach-3-the-ef-core-in-memory-database"></a><span data-ttu-id="8c7e4-155">Ansatz 3: Die InMemory-Datenbank von EF Core</span><span class="sxs-lookup"><span data-stu-id="8c7e4-155">Approach 3: The EF Core in-memory database</span></span>

<span data-ttu-id="8c7e4-156">EF Core bietet eine InMemory-Datenbank, die wir für interne Tests von EF Core selbst verwenden.</span><span class="sxs-lookup"><span data-stu-id="8c7e4-156">EF Core comes with an in-memory database that we use for internal testing of EF Core itself.</span></span>
<span data-ttu-id="8c7e4-157">Diese Datenbank ist im Allgemeinen **nicht als Ersatz für das Testen von Anwendungen geeignet, die EF Core verwenden**.</span><span class="sxs-lookup"><span data-stu-id="8c7e4-157">This database is in general **not suitable as a substitute for testing applications that use EF Core**.</span></span> <span data-ttu-id="8c7e4-158">Insbesondere gilt:</span><span class="sxs-lookup"><span data-stu-id="8c7e4-158">Specifically:</span></span>
* <span data-ttu-id="8c7e4-159">Es handelt sich nicht um eine relationale Datenbank.</span><span class="sxs-lookup"><span data-stu-id="8c7e4-159">It is not a relational database</span></span>
* <span data-ttu-id="8c7e4-160">Transaktionen werden nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="8c7e4-160">It doesn't support transactions</span></span>
* <span data-ttu-id="8c7e4-161">Sie ist nicht für Leistung optimiert.</span><span class="sxs-lookup"><span data-stu-id="8c7e4-161">It is not optimized for performance</span></span>

<span data-ttu-id="8c7e4-162">Nichts davon ist beim Testen interner EF Core-Aspekte sehr wichtig, weil wir es speziell dort einsetzen, wo die Datenbank für den Test irrelevant ist.</span><span class="sxs-lookup"><span data-stu-id="8c7e4-162">None of this is very important when testing EF Core internals because we use it specifically where the database is irrelevant to the test.</span></span>
<span data-ttu-id="8c7e4-163">Auf der anderen Seite sind diese Dinge beim Testen einer Anwendung, die EF Core verwendet, in der Regel sehr wichtig.</span><span class="sxs-lookup"><span data-stu-id="8c7e4-163">On the other hand, these things tend to be very important when testing an application that uses EF Core.</span></span>

## <a name="unit-testing"></a><span data-ttu-id="8c7e4-164">Komponententest</span><span class="sxs-lookup"><span data-stu-id="8c7e4-164">Unit testing</span></span>

<span data-ttu-id="8c7e4-165">Erwägen Sie das Testen eines Teils der Geschäftslogik, der möglicherweise einige Daten aus einer Datenbank verwenden muss, aber nicht inhärent die Datenbankinteraktionen testet.</span><span class="sxs-lookup"><span data-stu-id="8c7e4-165">Consider testing a piece of business logic that might need to use some data from a database, but is not inherently testing the database interactions.</span></span>
<span data-ttu-id="8c7e4-166">Eine Möglichkeit ist die Verwendung eines [Testdoubles](https://en.wikipedia.org/wiki/Test_double), z. B. eines simulierten oder nachgemachten Elements.</span><span class="sxs-lookup"><span data-stu-id="8c7e4-166">One option is to use a [test double](https://en.wikipedia.org/wiki/Test_double) such as a mock or fake.</span></span>

<span data-ttu-id="8c7e4-167">Wir verwenden Testdoubles für interne Tests von EF Core.</span><span class="sxs-lookup"><span data-stu-id="8c7e4-167">We use test doubles for internal testing of EF Core.</span></span>
<span data-ttu-id="8c7e4-168">Wir versuchen jedoch nie, DbContext oder IQueryable zu simulieren.</span><span class="sxs-lookup"><span data-stu-id="8c7e4-168">However, we never try to mock DbContext or IQueryable.</span></span>
<span data-ttu-id="8c7e4-169">Dies ist schwierig, umständlich und instabil.</span><span class="sxs-lookup"><span data-stu-id="8c7e4-169">Doing so is difficult, cumbersome, and fragile.</span></span>
<span data-ttu-id="8c7e4-170">**Machen Sie es nicht.**</span><span class="sxs-lookup"><span data-stu-id="8c7e4-170">**Don't do it.**</span></span>

<span data-ttu-id="8c7e4-171">Stattdessen verwenden wir die InMemory-Datenbank, wenn wir Komponententests mit etwas durchführen, das DbContext verwendet.</span><span class="sxs-lookup"><span data-stu-id="8c7e4-171">Instead we use the in-memory database when unit testing something that uses DbContext.</span></span>
<span data-ttu-id="8c7e4-172">In diesem Fall ist die Verwendung der InMemory-Datenbank angemessen, da der Test nicht vom Datenbankverhalten abhängig ist.</span><span class="sxs-lookup"><span data-stu-id="8c7e4-172">In this case using the in-memory database is appropriate because the test is not dependent on database behavior.</span></span>
<span data-ttu-id="8c7e4-173">Tun Sie dies bloß nicht, um tatsächliche Datenbankabfragen oder -aktualisierungen zu testen.</span><span class="sxs-lookup"><span data-stu-id="8c7e4-173">Just don't do this to test actual database queries or updates.</span></span>   

<span data-ttu-id="8c7e4-174">Eine EF Core-spezifische Anleitung zur Verwendung der InMemory-Datenbank für Komponententests finden Sie unter [Testen mit dem InMemory-Anbieter](xref:core/miscellaneous/testing/in-memory).</span><span class="sxs-lookup"><span data-stu-id="8c7e4-174">See [Testing with the in-memory provider](xref:core/miscellaneous/testing/in-memory) for EF Core specific guidance on using the in-memory database for unit testing.</span></span>
