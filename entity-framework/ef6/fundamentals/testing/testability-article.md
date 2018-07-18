---
title: Prüfbarkeit und Entitätsframework 4.0
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 9430e2ab-261c-4e8e-8545-2ebc52d7a247
caps.latest.revision: 3
ms.openlocfilehash: 61773f8a23ff54ddb78aeeb5656c669b12f91478
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2018
ms.locfileid: "39121186"
---
# <a name="testability-and-entity-framework-40"></a><span data-ttu-id="2b0b0-102">Prüfbarkeit und Entitätsframework 4.0</span><span class="sxs-lookup"><span data-stu-id="2b0b0-102">Testability and Entity Framework 4.0</span></span>
<span data-ttu-id="2b0b0-103">Scott Allen</span><span class="sxs-lookup"><span data-stu-id="2b0b0-103">Scott Allen</span></span>

<span data-ttu-id="2b0b0-104">Veröffentlicht: Mai 2010</span><span class="sxs-lookup"><span data-stu-id="2b0b0-104">Published: May 2010</span></span>

## <a name="introduction"></a><span data-ttu-id="2b0b0-105">Einführung</span><span class="sxs-lookup"><span data-stu-id="2b0b0-105">Introduction</span></span>

<span data-ttu-id="2b0b0-106">Dieses Whitepaper beschreibt und veranschaulicht das Schreiben von testbarem Code mit dem ADO.NET Entity Framework 4.0 und Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-106">This white paper describes and demonstrates how to write testable code with the ADO.NET Entity Framework 4.0 and Visual Studio 2010.</span></span> <span data-ttu-id="2b0b0-107">In diesem Artikel versucht nicht, sich auf eine bestimmte Testverfahren vor, wie z. B. Test-driven Design (TDD) oder verhaltensgesteuerter Entwurf (BDD) konzentrieren.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-107">This paper does not try to focus on a specific testing methodology, like test-driven design (TDD) or behavior-driven design (BDD).</span></span> <span data-ttu-id="2b0b0-108">Stattdessen in diesem Artikel liegt der Schwerpunkt auf Code schreiben, das ADO.NET Entity Framework verwendet, die noch bleibt, einfach zu isolieren und auf automatisierte Weise zu testen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-108">Instead this paper will focus on how to write code that uses the ADO.NET Entity Framework yet remains easy to isolate and test in an automated fashion.</span></span> <span data-ttu-id="2b0b0-109">Sehen wir uns allgemeine Entwurfsmuster, die zu erleichtern, Testszenarien in Daten zugreifen, und erfahren Sie, wie diese Muster bei der Verwendung des Frameworks zu übernehmen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-109">We’ll look at common design patterns that facilitate testing in data access scenarios and see how to apply those patterns when using the framework.</span></span> <span data-ttu-id="2b0b0-110">Außerdem betrachten wir bestimmte Features des Frameworks zu sehen, wie diese Features in testfähiger Codeabschnitt ordnungsgemäß funktionieren können.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-110">We’ll also look at specific features of the framework to see how those features can work in testable code.</span></span>

## <a name="what-is-testable-code"></a><span data-ttu-id="2b0b0-111">Was ist testbarem Code?</span><span class="sxs-lookup"><span data-stu-id="2b0b0-111">What Is Testable Code?</span></span>

<span data-ttu-id="2b0b0-112">Die Möglichkeit, um zu überprüfen, ob einen Teil der Software, die automatisierte Komponententests bietet viele Vorteile, die wünschenswert.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-112">The ability to verify a piece of software using automated unit tests offers many desirable benefits.</span></span> <span data-ttu-id="2b0b0-113">Jeder weiß, dass gute Tests die Anzahl der von Softwarefehlern in einer Anwendung und die Erhöhung der Anwendung Codequalität – aber wenn Komponententests vorhanden geht weit über schon das Auffinden von Fehlern verringern werden.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-113">Everyone knows that good tests will reduce the number of software defects in an application and increase the application’s quality - but having unit tests in place goes far beyond just finding bugs.</span></span>

<span data-ttu-id="2b0b0-114">Eine gute Komponententest-Suite ermöglicht ein Entwicklungsteam, Zeit zu sparen, und behalten die Kontrolle der Software, die sie erstellen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-114">A good unit test suite allows a development team to save time and remain in control of the software they create.</span></span> <span data-ttu-id="2b0b0-115">Ein Team kann Änderungen vornehmen, vorhandenen Code, Umgestalten, Umgestaltung und Umstrukturierung von Software an neue Anforderungen zu erfüllen, und neue Komponenten in einer Anwendung gleichzeitig zu wissen, die Testsammlung hinzufügen, kann das Verhalten der Anwendung überprüfen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-115">A team can make changes to existing code, refactor, redesign, and restructure software to meet new requirements, and add new components into an application all while knowing the test suite can verify the application’s behavior.</span></span> <span data-ttu-id="2b0b0-116">Komponententests sind Teil eines Zyklus dann schnell rückmeldungen leicht geändert und die Verwaltbarkeit von Software als vergrößert sich die Komplexität zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-116">Unit tests are part of a quick feedback cycle to facilitate change and preserve the maintainability of software as complexity increases.</span></span>

<span data-ttu-id="2b0b0-117">Komponententests ist jedoch mit einem Preis.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-117">Unit testing comes with a price, however.</span></span> <span data-ttu-id="2b0b0-118">Ein Team muss die Zeit zum Erstellen und Verwalten von Komponententests zu investieren.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-118">A team has to invest the time to create and maintain unit tests.</span></span> <span data-ttu-id="2b0b0-119">Die erforderlichen Aufwand zum Erstellen dieser Tests steht in direkter Beziehung zu den **Testability** der zugrunde liegenden Software.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-119">The amount of effort required to create these tests is directly related to the **testability** of the underlying software.</span></span> <span data-ttu-id="2b0b0-120">Wie einfach die Software, um zu testen ist?</span><span class="sxs-lookup"><span data-stu-id="2b0b0-120">How easy is the software to test?</span></span> <span data-ttu-id="2b0b0-121">Softwareentwicklung mit Prüfbarkeit Teams werden effektive Tests schneller als das Team arbeiten mit nicht testbare Software erstellen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-121">A team designing software with testability in mind will create effective tests faster than the team working with un-testable software.</span></span>

<span data-ttu-id="2b0b0-122">Microsoft hat das ADO.NET Entity Framework 4.0 (EF4) mit Prüfbarkeit entwickelt.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-122">Microsoft designed the ADO.NET Entity Framework 4.0 (EF4) with testability in mind.</span></span> <span data-ttu-id="2b0b0-123">Dies bedeutet nicht, dass Entwickler Komponententests für Frameworkcode selbst geschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-123">This doesn’t mean developers will be writing unit tests against framework code itself.</span></span> <span data-ttu-id="2b0b0-124">Stattdessen erleichtern die Testability-Ziele für EF4 testbarem Code zu erstellen, die basierend auf dem Framework erstellt.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-124">Instead, the testability goals for EF4 make it easy to create testable code that builds on top of the framework.</span></span> <span data-ttu-id="2b0b0-125">Bevor wir die spezifische Beispiele betrachten, lohnt es sich um Qualitäten testfähiger Codeabschnitt zu verstehen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-125">Before we look at specific examples, it’s worthwhile to understand the qualities of testable code.</span></span>

### <a name="the-qualities-of-testable-code"></a><span data-ttu-id="2b0b0-126">Die Qualitäten von testbarem Code</span><span class="sxs-lookup"><span data-stu-id="2b0b0-126">The Qualities of Testable Code</span></span>

<span data-ttu-id="2b0b0-127">Code, der einfach zu testen ist zeigt immer mindestens zwei "traits".</span><span class="sxs-lookup"><span data-stu-id="2b0b0-127">Code that is easy to test will always exhibit at least two traits.</span></span> <span data-ttu-id="2b0b0-128">Erstens ist testfähig Code ist einfach, **beobachten**.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-128">First, testable code is easy to **observe**.</span></span> <span data-ttu-id="2b0b0-129">Wenn eine Gruppe von Eingaben, sollte es einfach, um die Ausgabe des Codes zu beobachten sein.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-129">Given some set of inputs, it should be easy to observe the output of the code.</span></span> <span data-ttu-id="2b0b0-130">Testen die folgende Methode ist beispielsweise einfach, da die Methode direkt das Ergebnis einer Berechnung zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-130">For example, testing the following method is easy because the method directly returns the result of a calculation.</span></span>

``` csharp
    public int Add(int x, int y) {
        return x + y;
    }
```

<span data-ttu-id="2b0b0-131">Eine Methode testen ist schwierig, wenn die Methode den berechneten Wert in ein Netzwerksocket, einer Datenbanktabelle oder eine Datei, wie der folgende Code schreibt.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-131">Testing a method is difficult if the method writes the computed value into a network socket, a database table, or a file like the following code.</span></span> <span data-ttu-id="2b0b0-132">Der Test muss sich um zusätzliche Vorgänge zum Abrufen des Werts auszuführen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-132">The test has to perform additional work to retrieve the value.</span></span>

``` csharp
    public void AddAndSaveToFile(int x, int y) {
         var results = string.Format("The answer is {0}", x + y);
         File.WriteAllText("results.txt", results);
    }
```

<span data-ttu-id="2b0b0-133">Zweitens testbarem Code ist einfach, **isolieren**.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-133">Secondly, testable code is easy to **isolate**.</span></span> <span data-ttu-id="2b0b0-134">Verwenden Sie im folgenden Pseudocode wir als Beispiel für fehlerhafte testbarem Code.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-134">Let’s use the following pseudo-code as a bad example of testable code.</span></span>

``` csharp
    public int ComputePolicyValue(InsurancePolicy policy) {
        using (var connection = new SqlConnection("dbConnection"))
        using (var command = new SqlCommand(query, connection)) {

            // business calculations omitted ...               

            if (totalValue > notificationThreshold) {
                var message = new MailMessage();
                message.Subject = "Warning!";
                var client = new SmtpClient();
                client.Send(message);
            }
        }
        return totalValue;
    }
```

<span data-ttu-id="2b0b0-135">Die Methode ist leicht zu sehen – wir können in einer Versicherungspolice zu übergeben und stellen Sie sicher, dass der Rückgabewert ein erwartetes Ergebnis übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-135">The method is easy to observe – we can pass in an insurance policy and verify the return value matches an expected result.</span></span> <span data-ttu-id="2b0b0-136">Allerdings zum Testen der Methode müssen wir eine Datenbank mit dem korrekten Schema installiert haben, und konfigurieren den SMTP-Server aus, für den Fall, dass die Methode versucht, eine e-Mail zu senden.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-136">However, to test the method we’ll need to have a database installed with the correct schema, and configure the SMTP server in case the method tries to send an email.</span></span>

<span data-ttu-id="2b0b0-137">Der Komponententest wünscht, dass nur um zu überprüfen, ob die Berechnungslogik innerhalb der Methode, aber der Test schlägt möglicherweise fehl, weil es sich bei der e-Mail-Server offline ist, oder der Datenbankserver befindet.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-137">The unit test only wants to verify the calculation logic inside the method, but the test might fail because the email server is offline, or because the database server moved.</span></span> <span data-ttu-id="2b0b0-138">Beide dieser Fehler sind nicht mit dem Verhalten, das möchte, dass der Test überprüfen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-138">Both of these failures are unrelated to the behavior the test wants to verify.</span></span> <span data-ttu-id="2b0b0-139">Das Verhalten ist schwierig, zu isolieren.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-139">The behavior is difficult to isolate.</span></span>

<span data-ttu-id="2b0b0-140">Softwareentwickler, die sich bemühen, Schreiben von testbarem Code häufig sich bemühen, warten eine Trennung der Zuständigkeiten im Code, dass sie schreiben.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-140">Software developers who strive to write testable code often strive to maintain a separation of concerns in the code they write.</span></span> <span data-ttu-id="2b0b0-141">Die oben dargestellte Methode sollte konzentrieren, die Geschäfts-Berechnungen und delegieren die Implementierungsdetails für Datenbank "und"-e-Mail an andere Komponenten.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-141">The above method should focus on the business calculations and delegate the database and email implementation details to other components.</span></span> <span data-ttu-id="2b0b0-142">Martin ruft dies auf dem Prinzip der einzigen Verantwortung.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-142">Robert C. Martin calls this the Single Responsibility Principle.</span></span> <span data-ttu-id="2b0b0-143">Ein Objekt muss eine einzelne, schmale Verantwortung, z. B. Berechnen des Werts von einer Richtlinie kapseln.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-143">An object should encapsulate a single, narrow responsibility, like calculating the value of a policy.</span></span> <span data-ttu-id="2b0b0-144">Alle anderen Datenbank und Benachrichtigung arbeiten, sollten die Verantwortung für ein anderes Objekt sein.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-144">All other database and notification work should be the responsibility of some other object.</span></span> <span data-ttu-id="2b0b0-145">Auf diese Weise erstellter Code ist einfacher zu isolieren, da sie auf eine einzelne Aufgabe fokussiert ist.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-145">Code written in this fashion is easier to isolate because it is focused on a single task.</span></span>

<span data-ttu-id="2b0b0-146">.NET enthält die Abstraktionen, die wir müssen dem Prinzip der einzigen Verantwortung folgen und erreichen die Isolierung.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-146">In .NET we have the abstractions we need to follow the Single Responsibility Principle and achieve isolation.</span></span> <span data-ttu-id="2b0b0-147">Schnittstellendefinitionen verwenden Sie können und den Code so-schnittstellenabstraktion, die anstelle eines konkreten Typs zu erzwingen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-147">We can use interface definitions and force the code to use the interface abstraction instead of a concrete type.</span></span> <span data-ttu-id="2b0b0-148">Weiter unten in diesem Artikel werden wir sehen, wie eine Methode, wie Sie mit der ungültige oben angeführte Beispiel arbeiten kann, die Schnittstellen *suchen* , wie sie mit der Datenbank kommunizieren werden.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-148">Later in this paper we’ll see how a method like the bad example presented above can work with interfaces that *look* like they will talk to the database.</span></span> <span data-ttu-id="2b0b0-149">Während der Tests können wir jedoch eine dummy-Implementierung ersetzen, die nicht mit der Datenbank kommunizieren, sondern Daten im Arbeitsspeicher enthält.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-149">At test time, however, we can substitute a dummy implementation that doesn’t talk to the database but instead holds data in memory.</span></span> <span data-ttu-id="2b0b0-150">Dieser dummy-Implementierung wird der Code nicht verknüpfte Probleme beheben, die in der Datenbankkonfiguration oder Code für den Datenzugriff isolieren.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-150">This dummy implementation will isolate the code from unrelated problems in the data access code or database configuration.</span></span>

<span data-ttu-id="2b0b0-151">Es gibt weitere Vorteile bei der Isolation.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-151">There are additional benefits to isolation.</span></span> <span data-ttu-id="2b0b0-152">Die verwendete Berechnung in der letzten Methode dauert nur wenige Millisekunden zum Ausführen, aber der Test selbst möglicherweise mehrere Sekunden lang wie der Code-Hops für das Netzwerk und kommuniziert mit verschiedenen Servern ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-152">The business calculation in the last method should only take a few milliseconds to execute, but the test itself might run for several seconds as the code hops around the network and talks to various servers.</span></span> <span data-ttu-id="2b0b0-153">Komponententests sollten schnell um kleine Änderungen zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-153">Unit tests should run fast to facilitate small changes.</span></span> <span data-ttu-id="2b0b0-154">Komponententests sollten auch wiederholbar sein und nicht fehl, da eine Komponente, die nicht mit den Test, ein Problem auftritt.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-154">Unit tests should also be repeatable and not fail because a component unrelated to the test has a problem.</span></span> <span data-ttu-id="2b0b0-155">Schreiben, dass Code, der einfach, das überwacht werden und zum Isolieren von bedeutet, dass Entwickler zum Schreiben von Tests für den Code weniger aufwendig, verbringen Sie weniger Zeit mit dem Warten auf auszuführende Tests und mehr wichtig ist, dass weniger Zeit für das Aufspüren von Fehlern, die nicht vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-155">Writing code that is easy to observe and to isolate means developers will have an easier time writing tests for the code, spend less time waiting for tests to execute, and more importantly, spend less time tracking down bugs that do not exist.</span></span>

<span data-ttu-id="2b0b0-156">Hoffentlich können, die testbarem Code weist Qualitäten Sie freuen uns über die Vorteile von Tests und verstehen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-156">Hopefully you can appreciate the benefits of testing and understand the qualities that testable code exhibits.</span></span> <span data-ttu-id="2b0b0-157">Wir sind dabei zu beschrieben, wie Sie Code schreiben, die mit EF4 zum Speichern von Daten in eine Datenbank während des Bestehens Observable- und einfach, zu isolieren, aber wir zuerst schränken wir konzentrieren uns um die testbarer Entwürfe für den Datenzugriff zu erörtern.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-157">We are about to address how to write code that works with EF4 to save data into a database while remaining observable and easy to isolate, but first we’ll narrow our focus to discuss testable designs for data access.</span></span>

## <a name="design-patterns-for-data-persistence"></a><span data-ttu-id="2b0b0-158">Entwurfsmuster für Dauerhaftigkeit von Daten</span><span class="sxs-lookup"><span data-stu-id="2b0b0-158">Design Patterns for Data Persistence</span></span>

<span data-ttu-id="2b0b0-159">Beide fehlerhaften zuvor präsentierten Beispiele haben zu viele Aufgaben.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-159">Both of the bad examples presented earlier had too many responsibilities.</span></span> <span data-ttu-id="2b0b0-160">Im erste Beispiel für die fehlerhafte Ausführen einer Berechnung mussten *und* in eine Datei schreiben.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-160">The first bad example had to perform a calculation *and* write to a file.</span></span> <span data-ttu-id="2b0b0-161">Im zweite Beispiel für die fehlerhafte musste zum Lesen von Daten aus einer Datenbank *und* führt eine Berechnung durch Business *und* -e-Mail zu senden.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-161">The second bad example had to read data from a database *and* perform a business calculation *and* send email.</span></span> <span data-ttu-id="2b0b0-162">Mit dem Entwerfen von kleineren Methoden, die Trennen von Concerns, und diese Aufgabe an andere Komponenten delegieren, stellen Sie große Fortschritte für das Schreiben von testbarem Code.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-162">By designing smaller methods that separate concerns and delegate responsibility to other components you’ll make great strides towards writing testable code.</span></span> <span data-ttu-id="2b0b0-163">Das Ziel ist, um Funktionalität zu erstellen, durch das Erstellen von Aktionen über Abstraktionen für klein und konzentriert.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-163">The goal is to build functionality by composing actions from small and focused abstractions.</span></span>

<span data-ttu-id="2b0b0-164">Wenn es darum, Datenpersistenz den kleinen geht und fokussierte Abstraktionen, die wir suchen so gängig, haben sie als Entwurfsmuster dokumentiert.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-164">When it comes to data persistence the small and focused abstractions we are looking for are so common they’ve been documented as design patterns.</span></span> <span data-ttu-id="2b0b0-165">Fowlers Buch Patterns of Enterprise Application Architecture war das erste Arbeitselement zum Beschreiben dieser Muster gedruckt.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-165">Martin Fowler’s book Patterns of Enterprise Application Architecture was the first work to describe these patterns in print.</span></span> <span data-ttu-id="2b0b0-166">Wir gebe eine kurze Beschreibung dieser Muster in den folgenden Abschnitten, bevor wir zeigen, wie diese ADO.NET Entity Framework implementiert und kann mit dieser Muster.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-166">We’ll provide a brief description of these patterns in the following sections before we show how these ADO.NET Entity Framework implements and works with these patterns.</span></span>

### <a name="the-repository-pattern"></a><span data-ttu-id="2b0b0-167">Das Repositorymuster</span><span class="sxs-lookup"><span data-stu-id="2b0b0-167">The Repository Pattern</span></span>

<span data-ttu-id="2b0b0-168">Fowler sagt, ein Repository "zwischen der Domäne und die Daten mithilfe einer sammlungsähnlichen Schnittstelle für den Zugriff auf die Domänenobjekte zuordnungsebenen vermittelt".</span><span class="sxs-lookup"><span data-stu-id="2b0b0-168">Fowler says a repository “mediates between the domain and data mapping layers using a collection-like interface for accessing domain objects”.</span></span> <span data-ttu-id="2b0b0-169">Das Ziel des Repositorymusters zum Isolieren von Code über eine umfassende Datenzugriff ist und wie wir früher Isolation gesehen haben, ist eine erforderliche Eigenschaft für testbarkeit.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-169">The goal of the repository pattern is to isolate code from the minutiae of data access, and as we saw earlier isolation is a required trait for testability.</span></span>

<span data-ttu-id="2b0b0-170">Die Taste, um die Isolation ist wie das Repository auf Objekte, die mithilfe einer sammlungsähnlichen Schnittstelle verfügbar macht.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-170">The key to the isolation is how the repository exposes objects using a collection-like interface.</span></span> <span data-ttu-id="2b0b0-171">Die Logik, die Sie schreiben, verwenden Sie das Repository weiß nicht, wie das Repository werden die Objekte materialisieren, die Sie anfordern.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-171">The logic you write to use the repository has no idea how the repository will materialize the objects you request.</span></span> <span data-ttu-id="2b0b0-172">Das Repository mit einer Datenbank kommunizieren kann, oder es kann nur Objekte zurückgeben, aus einer in-Memory-Sammlung.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-172">The repository might talk to a database, or it might just return objects from an in-memory collection.</span></span> <span data-ttu-id="2b0b0-173">Alles, was Ihr Code muss wissen, ist, dass das Repository wird angezeigt, um die Auflistung zu verwalten, können Sie abrufen, hinzufügen und Löschen von Objekten aus der Auflistung.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-173">All your code needs to know is that the repository appears to maintain the collection, and you can retrieve, add, and delete objects from the collection.</span></span>

<span data-ttu-id="2b0b0-174">In vorhandenen .NET-Anwendungen erbt eine konkrete Repository häufig von einer generischen Schnittstelle wie folgt:</span><span class="sxs-lookup"><span data-stu-id="2b0b0-174">In existing .NET applications a concrete repository often inherits from a generic interface like the following:</span></span>

``` csharp
    public interface IRepository<T> {       
        IEnumerable<T> FindAll();
        IEnumerable<T> FindBy(Expression<Func\<T, bool>> predicate);
        T FindById(int id);
        void Add(T newEntity);
        void Remove(T entity);
    }
```

<span data-ttu-id="2b0b0-175">Wir erstellen ein paar Änderungen zur Schnittstellendefinition Wir stellen eine Implementierung für EF4, aber das grundlegende Konzept bleibt unverändert.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-175">We’ll make a few changes to the interface definition when we provide an implementation for EF4, but the basic concept remains the same.</span></span> <span data-ttu-id="2b0b0-176">Code kann eine konkrete Repository, die Implementierung dieser Schnittstelle verwenden, um eine Entität von der primären Schlüsselwert, zum Abrufen einer Auflistung von Entitäten, die basierend auf der Auswertung eines Prädikats, abzurufen, oder rufen Sie einfach alle verfügbare Entitäten.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-176">Code can use a concrete repository implementing this interface to retrieve an entity by its primary key value, to retrieve a collection of entities based on the evaluation of a predicate, or simply retrieve all available entities.</span></span> <span data-ttu-id="2b0b0-177">Der Code kann auch hinzufügen und Entfernen von Entitäten über die Repositoryschnittstelle.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-177">The code can also add and remove entities through the repository interface.</span></span>

<span data-ttu-id="2b0b0-178">Wenn eine IRepository der Employee-Objekten, kann Code die folgenden Vorgänge ausführen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-178">Given an IRepository of Employee objects, code can perform the following operations.</span></span>

``` csharp
    var employeesNamedScott =
        repository
            .FindBy(e => e.Name == "Scott")
            .OrderBy(e => e.HireDate);
    var firstEmployee = repository.FindById(1);
    var newEmployee = new Employee() {/*... */};
    repository.Add(newEmployee);
```

<span data-ttu-id="2b0b0-179">Da der Code eine Schnittstelle (IRepository der Mitarbeiter) verwendet wird, können wir den Code mit verschiedenen Implementierungen der Schnittstelle bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-179">Since the code is using an interface (IRepository of Employee), we can provide the code with different implementations of the interface.</span></span> <span data-ttu-id="2b0b0-180">Eine Implementierung ist möglicherweise eine Implementierung von EF4 und Beibehalten von Objekten in einer Microsoft SQL Server-Datenbank gesichert.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-180">One implementation might be an implementation backed by EF4 and persisting objects into a Microsoft SQL Server database.</span></span> <span data-ttu-id="2b0b0-181">Eine andere Implementierung (eine, die während der Tests verwendet werden) kann von einer Liste von Employee-Objekte im Arbeitsspeicher unterstützt werden.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-181">A different implementation (one we use during testing) might be backed by an in-memory List of Employee objects.</span></span> <span data-ttu-id="2b0b0-182">Die Schnittstelle trägt zur Isolierung im Code zu erzielen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-182">The interface will help to achieve isolation in the code.</span></span>

<span data-ttu-id="2b0b0-183">Beachten Sie, dass die IRepository&lt;T&gt; Schnittstelle macht ein Speichervorgang nicht verfügbar.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-183">Notice the IRepository&lt;T&gt; interface does not expose a Save operation.</span></span> <span data-ttu-id="2b0b0-184">Wie aktualisieren wir vorhandene Objekte?</span><span class="sxs-lookup"><span data-stu-id="2b0b0-184">How do we update existing objects?</span></span> <span data-ttu-id="2b0b0-185">Gelangen Sie möglicherweise über IRepository-Definitionen, die den Speichervorgang enthalten sind, und Implementierungen der folgenden Repositorys sofort ein Objekt in der Datenbank beibehalten werden müssen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-185">You might come across IRepository definitions that do include the Save operation, and implementations of these repositories will need to immediately persist an object into the database.</span></span> <span data-ttu-id="2b0b0-186">In vielen Anwendungen sollten sich jedoch nicht die Objekte einzeln beibehalten werden.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-186">However, in many applications we don’t want to persist objects individually.</span></span> <span data-ttu-id="2b0b0-187">Stattdessen möchten wir setzen Sie Objekte, vielleicht, diese Objekte als Teil einer Geschäftsaktivität aus anderen Repositorys, ändern und dann persistent gespeichert werden alle Objekte als Teil einer einzelnen atomaren Vorgang.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-187">Instead, we want to bring objects to life, perhaps from different repositories, modify those objects as part of a business activity, and then persist all the objects as part of a single, atomic operation.</span></span> <span data-ttu-id="2b0b0-188">Es ist ein Muster für diese Art von Verhalten zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-188">Fortunately, there is a pattern to allow this type of behavior.</span></span>

### <a name="the-unit-of-work-pattern"></a><span data-ttu-id="2b0b0-189">Das Arbeitseinheitsmuster</span><span class="sxs-lookup"><span data-stu-id="2b0b0-189">The Unit of Work Pattern</span></span>

<span data-ttu-id="2b0b0-190">Fowler sagt eine Arbeitseinheit "verwaltet eine Liste von Objekten einer Geschäftstransaktion betroffen und koordiniert das Schreiben aus Änderungen und die Auflösung von Parallelitätsproblemen".</span><span class="sxs-lookup"><span data-stu-id="2b0b0-190">Fowler says a unit of work will “maintain a list of objects affected by a business transaction and coordinates the writing out of changes and the resolution of concurrency problems”.</span></span> <span data-ttu-id="2b0b0-191">Es ist die Verantwortung für die Arbeitseinheit zum Nachverfolgen von Änderungen an den Objekten, wir setzen aus einem Repository, und speichern alle Änderungen, die wir an den Objekten vorgenommen haben, wenn wir sagen, dass die Arbeitseinheit um die Änderungen zu übernehmen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-191">It is the responsibility of the unit of work to track changes to the objects we bring to life from a repository and persist any changes we’ve made to the objects when we tell the unit of work to commit the changes.</span></span> <span data-ttu-id="2b0b0-192">Es ist auch die Verantwortung für die Arbeitseinheit die neuen Objekte werden, die wir alle Repositorys hinzugefügt haben, und fügen Sie die Objekte in die Datenbank, und auch verwalten löschen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-192">It’s also the responsibility of the unit of work to take the new objects we’ve added to all repositories and insert the objects into a database, and also mange deletion.</span></span>

<span data-ttu-id="2b0b0-193">Wenn Sie bisher keine Arbeit mit ADO.NET DataSets durchgeführt haben wird bereits mit der arbeitseinheitsmuster vertraut sein.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-193">If you’ve ever done any work with ADO.NET DataSets then you’ll already be familiar with the unit of work pattern.</span></span> <span data-ttu-id="2b0b0-194">ADO.NET DataSets haben die Möglichkeit zur nachverfolgung von den Updates, löschungen und Einfügen von DataRow-Objekten und kann (mithilfe eines TableAdapter) abstimmen alle unsere Änderungen in einer Datenbank.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-194">ADO.NET DataSets had the ability to track our updates, deletions, and insertion of DataRow objects and could (with the help of a TableAdapter) reconcile all our changes to a database.</span></span> <span data-ttu-id="2b0b0-195">DataSet-Objekte Modell jedoch einen getrennten Teil der zugrunde liegenden Datenbank.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-195">However, DataSet objects model a disconnected subset of the underlying database.</span></span> <span data-ttu-id="2b0b0-196">Das arbeitseinheitsmuster weist dasselbe Verhalten, aber mit Geschäftsobjekten und Domänenobjekte, die vom Datenzugriffscode isoliert und der Datenbank nicht bewusst sind.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-196">The unit of work pattern exhibits the same behavior, but works with business objects and domain objects that are isolated from data access code and unaware of the database.</span></span>

<span data-ttu-id="2b0b0-197">Eine Abstraktion für die Arbeitseinheit in .NET-Code Modell könnte folgendermaßen aussehen:</span><span class="sxs-lookup"><span data-stu-id="2b0b0-197">An abstraction to model the unit of work in .NET code might look like the following:</span></span>

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<Order> Orders { get; }
        IRepository<Customer> Customers { get; }
        void Commit();
    }
```

<span data-ttu-id="2b0b0-198">Durch Verfügbarmachen von Repository-Verweise, aus die Arbeitseinheit, die eine einzelne Arbeitseinheit sichergestellt hat Objekt die Möglichkeit, alle Entitäten, die während einer Geschäftstransaktion materialisiert nachverfolgen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-198">By exposing repository references from the unit of work we can ensure a single unit of work object has the ability to track all entities materialized during a business transaction.</span></span> <span data-ttu-id="2b0b0-199">Die Implementierung der Commit-Methode für eine echte Arbeitseinheit ist, in dem alle der Zauberstab Abstimmen von Änderungen im Speicher mit der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-199">The implementation of the Commit method for a real unit of work is where all the magic happens to reconcile in-memory changes with the database.</span></span> 

<span data-ttu-id="2b0b0-200">Wenn einen IUnitOfWork-Verweis, Code nehmen Sie Änderungen an Geschäftsobjekte aus ein oder mehrere Repositorys abgerufen und speichert alle Änderungen, die mithilfe des atomic-Commit-Vorgangs.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-200">Given an IUnitOfWork reference, code can make changes to business objects retrieved from one or more repositories and save all the changes using the atomic Commit operation.</span></span>

``` csharp
    var firstEmployee = unitofWork.Employees.FindById(1);
    var firstCustomer = unitofWork.Customers.FindById(1);
    firstEmployee.Name = "Alex";
    firstCustomer.Name = "Christopher";
    unitofWork.Commit();
```

### <a name="the-lazy-load-pattern"></a><span data-ttu-id="2b0b0-201">Das Lazy Loading-Muster</span><span class="sxs-lookup"><span data-stu-id="2b0b0-201">The Lazy Load Pattern</span></span>

<span data-ttu-id="2b0b0-202">Fowler verwendet den Name-lazy-Load "ein Objekt, das alle Daten enthalten keine Sie benötigen, sondern weiß, wie Sie es herunterladen".</span><span class="sxs-lookup"><span data-stu-id="2b0b0-202">Fowler uses the name lazy load to describe “an object that doesn’t contain all of the data you need but knows how to get it”.</span></span> <span data-ttu-id="2b0b0-203">Lazy Load transparent ist ein wichtiges Feature damit beim Schreiben von testfähigen Business-Code von und Arbeiten mit einer relationalen Datenbank.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-203">Transparent lazy loading is an important feature to have when writing testable business code and working with a relational database.</span></span> <span data-ttu-id="2b0b0-204">Betrachten Sie beispielsweise den folgenden Code ein.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-204">As an example, consider the following code.</span></span>

``` csharp
    var employee = repository.FindById(id);
    // ... and later ...
    foreach(var timeCard in employee.TimeCards) {
        // .. manipulate the timeCard
    }
```

<span data-ttu-id="2b0b0-205">Wie wird die Rückkehr Auflistung aufgefüllt?</span><span class="sxs-lookup"><span data-stu-id="2b0b0-205">How is the TimeCards collection populated?</span></span> <span data-ttu-id="2b0b0-206">Es gibt zwei mögliche Antworten.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-206">There are two possible answers.</span></span> <span data-ttu-id="2b0b0-207">Eine Antwort ist, dass der Mitarbeiter-Repository, wenn Sie gefragt werden, um Mitarbeiter abzurufen, eine Abfrage zum Abrufen sowohl des Mitarbeiters sowie des Mitarbeiters zugeordneten Zeit Karteninformationen ausgibt.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-207">One answer is that the employee repository, when asked to fetch an employee, issues a query to retrieve both the employee along with the employee’s associated time card information.</span></span> <span data-ttu-id="2b0b0-208">In relationale Datenbanken, die dies in der Regel erfordert eine Abfrage mit einer JOIN-Klausel und kann zu mehr Informationen als eine Anwendung abrufen müssen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-208">In relational databases this generally requires a query with a JOIN clause and may result in retrieving more information than an application needs.</span></span> <span data-ttu-id="2b0b0-209">Was geschieht, wenn die Anwendung nie muss die Eigenschaft für die Rückkehr zu berühren?</span><span class="sxs-lookup"><span data-stu-id="2b0b0-209">What if the application never needs to touch the TimeCards property?</span></span>

<span data-ttu-id="2b0b0-210">Eine zweite Antwort ist die Rückkehr-Eigenschaft "bei Nachfrage" geladen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-210">A second answer is to load the TimeCards property “on demand”.</span></span> <span data-ttu-id="2b0b0-211">Diese Lazy Load ist implizit und transparent für die Geschäftslogik, da es sich bei spezielle APIs zum Abrufen von Karteninformationen Zeit nicht in der Code aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-211">This lazy loading is implicit and transparent to the business logic because the code does not invoke special APIs to retrieve time card information.</span></span> <span data-ttu-id="2b0b0-212">Der Code wird davon ausgegangen, dass der Zeit Informationen vorhanden, wenn erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-212">The code assumes the time card information is present when needed.</span></span> <span data-ttu-id="2b0b0-213">Stellt Magie lazy Loading, die Common Language Runtime Abfangen der Methodenaufrufe in der Regel umfasst.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-213">There is some magic involved with lazy loading that generally involves runtime interception of method invocations.</span></span> <span data-ttu-id="2b0b0-214">Der abfangender Code ist für die Kommunikation mit der Datenbank und Abrufen von Informationen einmal-Karte, bleiben die Geschäftslogik können Geschäftslogik verantwortlich.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-214">The intercepting code is responsible for talking to the database and retrieving time card information while leaving the business logic free to be business logic.</span></span> <span data-ttu-id="2b0b0-215">Dieser lazy Loading-Magic-Befehl können den Code selbst Datenabrufvorgängen und führt weitere testbarem Code zu isolieren.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-215">This lazy load magic allows the business code to isolate itself from data retrieval operations and results in more testable code.</span></span>

<span data-ttu-id="2b0b0-216">Der Nachteil ein lazy Load ist, die, wenn eine Anwendung *ist* benötigen die der Zeit Karteninformationen, die eine weitere Abfrage wird der Code ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-216">The drawback to a lazy load is that when an application *does* need the time card information the code will execute an additional query.</span></span> <span data-ttu-id="2b0b0-217">Dies ist ein Problem für viele Anwendungen, sondern für leistungsabhängige Anwendungen oder Anwendungen durchläuft eine Anzahl von Objekten für Mitarbeiter und Ausführen einer Abfrage zum Abrufen von Karten der Zeit während jeder Iteration der Schleife (ein Problem häufig als N + 1 bezeichnet nicht. abfrageproblem), lazy Loading steht ein Ziehvorgang.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-217">This isn’t a concern for many applications, but for performance sensitive applications or applications looping through a number of employee objects and executing a query to retrieve time cards during each iteration of the loop (a problem often referred to as the N+1 query problem), lazy loading is a drag.</span></span> <span data-ttu-id="2b0b0-218">In diesen Szenarios möchten eine Anwendung laden Zeit Karteninformationen auf möglichst effiziente Weise möglich.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-218">In these scenarios an application might want to eagerly load time card information in the most efficient manner possible.</span></span>

<span data-ttu-id="2b0b0-219">Zum Glück, erfahren Sie, wie EF4 unterstützt sowohl implizite verzögerten Laden und effiziente Eager Load geladen. wie wir in den nächsten Abschnitt verschieben und diese Muster zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-219">Fortunately, we’ll see how EF4 supports both implicit lazy loads and efficient eager loads as we move into the next section and implement these patterns.</span></span>

## <a name="implementing-patterns-with-the-entity-framework"></a><span data-ttu-id="2b0b0-220">Implementieren von Mustern, die mit dem Entitätsframework</span><span class="sxs-lookup"><span data-stu-id="2b0b0-220">Implementing Patterns with the Entity Framework</span></span>

<span data-ttu-id="2b0b0-221">Die gute Nachricht ist, dass alle Entwurfsmuster, die wir im letzten Abschnitt beschriebenen einfach mit EF4 zu implementieren sind.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-221">The good news is that all of the design patterns we described in the last section are straightforward to implement with EF4.</span></span> <span data-ttu-id="2b0b0-222">Zur Veranschaulichung werden wir eine einfache ASP.NET MVC-Anwendung verwenden, bearbeiten und Mitarbeiter und ihre zugeordneten Zeit Karte-Informationen angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-222">To demonstrate we are going to use a simple ASP.NET MVC application to edit and display Employees and their associated time card information.</span></span> <span data-ttu-id="2b0b0-223">Wir beginnen mit dem folgenden "plain old CLR Objects" (POCOs).</span><span class="sxs-lookup"><span data-stu-id="2b0b0-223">We’ll start by using the following “plain old CLR objects” (POCOs).</span></span> 

``` csharp
    public class Employee {
        public int Id { get; set; }
        public string Name { get; set; }
        public DateTime HireDate { get; set; }
        public ICollection<TimeCard> TimeCards { get; set; }
    }

    public class TimeCard {
        public int Id { get; set; }
        public int Hours { get; set; }
        public DateTime EffectiveDate { get; set; }
    }
```

<span data-ttu-id="2b0b0-224">Diese Definitionen der Klasse ändert sich etwas untersucht, Ansätze und Funktionen von EF4, aber die Absicht ist, um diese Klassen als Persistenzignoranz (PI) wie möglich zu halten.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-224">These class definitions will change slightly as we explore different approaches and features of EF4, but the intent is to keep these classes as persistence ignorant (PI) as possible.</span></span> <span data-ttu-id="2b0b0-225">Ein Objekt PI nicht weiß, *wie*, oder sogar *Wenn*, der Zustand enthält befindet sich in einer Datenbank.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-225">A PI object doesn’t know *how*, or even *if*, the state it holds lives inside a database.</span></span> <span data-ttu-id="2b0b0-226">PI und POCOs gehen hand in hand mit testbare Software.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-226">PI and POCOs go hand in hand with testable software.</span></span> <span data-ttu-id="2b0b0-227">Objekte, die mit einem POCO-Ansatz sind weniger eingeschränkte, flexibler und einfacher zu testen, da sie ohne vorhanden Datenbank ausgeführt werden können.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-227">Objects using a POCO approach are less constrained, more flexible, and easier to test because they can operate without a database present.</span></span>

<span data-ttu-id="2b0b0-228">Wir können ein Entity Data Model (EDM) in Visual Studio erstellen, mit der POCOs eingerichtet (siehe Abbildung 1).</span><span class="sxs-lookup"><span data-stu-id="2b0b0-228">With the POCOs in place we can create an Entity Data Model (EDM) in Visual Studio (see figure 1).</span></span> <span data-ttu-id="2b0b0-229">Wir werden nicht das EDM verwenden, um Code für die Entitäten zu generieren.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-229">We will not use the EDM to generate code for our entities.</span></span> <span data-ttu-id="2b0b0-230">Stattdessen möchten wir die Entitäten verwenden, die es Entwicklern auch liebevoll manuell erstellen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-230">Instead, we want to use the entities we lovingly craft by hand.</span></span> <span data-ttu-id="2b0b0-231">Wir verwenden nur des EDM auf unsere Datenbankschema zu generieren, und geben Sie die Metadaten, die ef4 benötigt, um Objekte in der Datenbank zuzuordnen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-231">We will only use the EDM to generate our database schema and provide the metadata EF4 needs to map objects into the database.</span></span>

![eftest_01](~/ef6/media/eftest-01.jpg)

<span data-ttu-id="2b0b0-233">**Abbildung 1**</span><span class="sxs-lookup"><span data-stu-id="2b0b0-233">**Figure 1**</span></span>

<span data-ttu-id="2b0b0-234">Hinweis: sollten Sie zunächst das EDM-Modell zu entwickeln, ist es möglich, bereinigen, POCO-Code aus dem EDM zu generieren.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-234">Note: if you want to develop the EDM model first, it is possible to generate clean, POCO code from the EDM.</span></span> <span data-ttu-id="2b0b0-235">Dies ist mit der Erweiterung Visual Studio 2010, die vom Data Programmability-Team bereitgestellten möglich.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-235">You can do this with a Visual Studio 2010 extension provided by the Data Programmability team.</span></span> <span data-ttu-id="2b0b0-236">Um die Erweiterung heruntergeladen haben, starten Sie den Erweiterungs-Manager in Visual Studio im Menü Extras, und suchen Sie im Onlinekatalog von Vorlagen für "POCO" (siehe Abbildung 2).</span><span class="sxs-lookup"><span data-stu-id="2b0b0-236">To download the extension, launch the Extension Manager from the Tools menu in Visual Studio and search the online gallery of templates for “POCO” (See figure 2).</span></span> <span data-ttu-id="2b0b0-237">Es sind mehrere POCO-Vorlagen für EF verfügbar.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-237">There are several POCO templates available for EF.</span></span> <span data-ttu-id="2b0b0-238">Weitere Informationen zur Verwendung der Vorlage finden Sie unter " [Exemplarische Vorgehensweise: POCO-Vorlage für das Entity Framework](http://blogs.msdn.com/adonet/pages/walkthrough-poco-template-for-the-entity-framework.aspx)".</span><span class="sxs-lookup"><span data-stu-id="2b0b0-238">For more information on using the template, see “ [Walkthrough: POCO Template for the Entity Framework](http://blogs.msdn.com/adonet/pages/walkthrough-poco-template-for-the-entity-framework.aspx)”.</span></span>

![eftest_02](~/ef6/media/eftest-02.png)

<span data-ttu-id="2b0b0-240">**Abbildung 2**</span><span class="sxs-lookup"><span data-stu-id="2b0b0-240">**Figure 2**</span></span>

<span data-ttu-id="2b0b0-241">Von diesem Ausgangspunkt POCO erforschen wir zwei verschiedene Ansätze zum testbarem Code.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-241">From this POCO starting point we will explore two different approaches to testable code.</span></span> <span data-ttu-id="2b0b0-242">Der erste Ansatz rufe ich die EF-Ansatz, da datenabstraktionen in Einheiten von Arbeit und Repositorys implementiert die Entity Framework-API nutzt.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-242">The first approach I call the EF approach because it leverages abstractions from the Entity Framework API to implement units of work and repositories.</span></span> <span data-ttu-id="2b0b0-243">Bei der zweiten Methode wir unserer eigenen benutzerdefinierten repositoryabstraktionen zu erstellen, und klicken Sie dann die vor- und Nachteile jeder Vorgehensweise.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-243">In the second approach we will create our own custom repository abstractions and then see the advantages and disadvantages of each approach.</span></span> <span data-ttu-id="2b0b0-244">Wir beginnen, mithilfe des EF-Ansatzes.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-244">We’ll start by exploring the EF approach.</span></span>  

### <a name="an-ef-centric-implementation"></a><span data-ttu-id="2b0b0-245">Eine zentrierte EF-Implementierung</span><span class="sxs-lookup"><span data-stu-id="2b0b0-245">An EF Centric Implementation</span></span>

<span data-ttu-id="2b0b0-246">Erwägen Sie die folgende Controlleraktion aus einem ASP.NET MVC-Projekt aus.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-246">Consider the following controller action from an ASP.NET MVC project.</span></span> <span data-ttu-id="2b0b0-247">Die Aktion ruft ein Employee-Objekt ab und gibt ein Ergebnis, um eine detaillierte Ansicht des Mitarbeiters anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-247">The action retrieves an Employee object and returns a result to display a detailed view of the employee.</span></span>

``` csharp
    public ViewResult Details(int id) {
        var employee = _unitOfWork.Employees
                                  .Single(e => e.Id == id);
        return View(employee);
    }
```

<span data-ttu-id="2b0b0-248">Ist der Code getestet werden?</span><span class="sxs-lookup"><span data-stu-id="2b0b0-248">Is the code testable?</span></span> <span data-ttu-id="2b0b0-249">Es sind mindestens zwei Tests, die wir müssten, um das Verhalten der Aktion zu überprüfen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-249">There are at least two tests we’d need to verify the action’s behavior.</span></span> <span data-ttu-id="2b0b0-250">Zuerst möchten wir überprüfen, ob die Aktion der richtigen Ansicht – einen einfachen Test zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-250">First, we’d like to verify the action returns the correct view – an easy test.</span></span> <span data-ttu-id="2b0b0-251">Wir möchten auch einen Test aus, um zu überprüfen, ob die Aktion ruft die richtigen Mitarbeiter ab, und wir möchten dies tun, ohne Ausführung von Code zum Abfragen der Datenbank schreiben.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-251">We’d also want to write a test to verify the action retrieves the correct employee, and we’d like to do this without executing code to query the database.</span></span> <span data-ttu-id="2b0b0-252">Denken Sie daran, dass den getestete Code isoliert werden soll.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-252">Remember we want to isolate the code under test.</span></span> <span data-ttu-id="2b0b0-253">Isolation sorgt, dass der Test nicht aufgrund eines Fehlers in der Datenbankkonfiguration oder den Datenzugriffscode fehlschlägt.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-253">Isolation will ensure the test doesn’t fail because of a bug in the data access code or database configuration.</span></span> <span data-ttu-id="2b0b0-254">Wenn der Test fehlschlägt, werden wir wissen, dass wir einen Fehler in der Controllerlogik und nicht in eine niedrigere Ebene Systemkomponente verfügen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-254">If the test fails, we will know we have a bug in the controller logic, and not in some lower level system component.</span></span>

<span data-ttu-id="2b0b0-255">Um Isolierung zu erzielen, wir benötigen einige Abstraktionen, wie die Schnittstellen, die wir präsentiert weiter oben für Repositorys und Arbeitseinheiten.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-255">To achieve isolation we’ll need some abstractions like the interfaces we presented earlier for repositories and units of work.</span></span> <span data-ttu-id="2b0b0-256">Denken Sie daran, dass das Repositorymuster für die Vermittlung zwischen Domänenobjekten und der Datenschicht für die Zuordnung ausgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-256">Remember the repository pattern is designed to mediate between domain objects and the data mapping layer.</span></span> <span data-ttu-id="2b0b0-257">In diesem Szenario EF4 *ist* Zuordnung layer und stellt bereits eine Repository-ähnliche Abstraktion, die mit dem Namen IObjectSet&lt;T&gt; (aus dem Namespace System.Data.Objects).</span><span class="sxs-lookup"><span data-stu-id="2b0b0-257">In this scenario EF4 *is* the data mapping layer, and already provides a repository-like abstraction named IObjectSet&lt;T&gt; (from the System.Data.Objects namespace).</span></span> <span data-ttu-id="2b0b0-258">Die Schnittstellendefinition sieht folgendermaßen aus.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-258">The interface definition looks like the following.</span></span>

``` csharp
    public interface IObjectSet<TEntity> :
                     IQueryable<TEntity>,
                     IEnumerable<TEntity>,
                     IQueryable,
                     IEnumerable
                     where TEntity : class
    {
        void AddObject(TEntity entity);
        void Attach(TEntity entity);
        void DeleteObject(TEntity entity);
        void Detach(TEntity entity);
    }
```

<span data-ttu-id="2b0b0-259">IObjectSet&lt;T&gt; erfüllt die Anforderungen für ein Repository aus, da es sich um eine Auflistung von Objekten ähnelt (über "IEnumerable"&lt;T&gt;) und stellt Methoden zum Hinzufügen und Entfernen von Objekten aus der simulierten Auflistung bereit.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-259">IObjectSet&lt;T&gt; meets the requirements for a repository because it resembles a collection of objects (via IEnumerable&lt;T&gt;) and provides methods to add and remove objects from the simulated collection.</span></span> <span data-ttu-id="2b0b0-260">Die Anfügen und trennen-Methoden verfügbar machen, den Funktionsumfang der EF4-API.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-260">The Attach and Detach methods expose additional capabilities of the EF4 API.</span></span> <span data-ttu-id="2b0b0-261">Verwenden von IObjectSet&lt;T&gt; als Schnittstelle für Repositorys, die wir benötigen einer Einheit der Arbeit Abstraktion Repositorys miteinander verbunden.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-261">To use IObjectSet&lt;T&gt; as the interface for repositories we need a unit of work abstraction to bind repositories together.</span></span>

``` csharp
    public interface IUnitOfWork {
        IObjectSet<Employee> Employees { get; }
        IObjectSet<TimeCard> TimeCards { get; }
        void Commit();
    }
```

<span data-ttu-id="2b0b0-262">Eine konkrete Implementierung dieser Schnittstelle wird wenden Sie sich an SQL Server und ist einfach, mit der ObjectContext-Klasse von EF4 zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-262">One concrete implementation of this interface will talk to SQL Server and is easy to create using the ObjectContext class from EF4.</span></span> <span data-ttu-id="2b0b0-263">Die ObjectContext-Klasse ist die echten Arbeitseinheit in der EF4-API.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-263">The ObjectContext class is the real unit of work in the EF4 API.</span></span>

``` csharp
    public class SqlUnitOfWork : IUnitOfWork {
        public SqlUnitOfWork() {
            var connectionString =
                ConfigurationManager
                    .ConnectionStrings[ConnectionStringName]
                    .ConnectionString;
            _context = new ObjectContext(connectionString);
        }

        public IObjectSet<Employee> Employees {
            get { return _context.CreateObjectSet<Employee>(); }
        }

        public IObjectSet<TimeCard> TimeCards {
            get { return _context.CreateObjectSet<TimeCard>(); }
        }

        public void Commit() {
            _context.SaveChanges();
        }

        readonly ObjectContext _context;
        const string ConnectionStringName = "EmployeeDataModelContainer";
    }
```

<span data-ttu-id="2b0b0-264">Schalten ein IObjectSet&lt;T&gt; zum Leben ist so einfach wie das Aufrufen der CreateObjectSet-Methode von ObjectContext-Objekt.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-264">Bringing an IObjectSet&lt;T&gt; to life is as easy as invoking the CreateObjectSet method of the ObjectContext object.</span></span> <span data-ttu-id="2b0b0-265">Hinter den Kulissen verwendet das Framework die Metadaten wir bereitgestellt haben, im EDM erstellen Sie eine konkrete "ObjectSet"&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-265">Behind the scenes the framework will use the metadata we provided in the EDM to produce a concrete ObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="2b0b0-266">Wir bei der Rückgabe der IObjectSet bleibe&lt;T&gt; Schnittstelle, da verhindern Testability im Clientcode beibehalten.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-266">We’ll stick with returning the IObjectSet&lt;T&gt; interface because it will help preserve testability in client code.</span></span>

<span data-ttu-id="2b0b0-267">Dieser konkreten Implementierung eignet sich in der Produktion, aber wir müssen sich auf das konzentrieren, wie wir unsere IUnitOfWork-Abstraktion zum Vereinfachen der Tests verwenden.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-267">This concrete implementation is useful in production, but we need to focus on how we’ll use our IUnitOfWork abstraction to facilitate testing.</span></span>

### <a name="the-test-doubles"></a><span data-ttu-id="2b0b0-268">Der Test-Doubles</span><span class="sxs-lookup"><span data-stu-id="2b0b0-268">The Test Doubles</span></span>

<span data-ttu-id="2b0b0-269">Um die Controlleraktion zu isolieren benötigen wir die Möglichkeit zum Wechseln zwischen der echten Arbeitseinheit (unterstützt durch einen ObjectContext) und einen Test doppelte oder "gefälschtes" Arbeitseinheit (in-Memory-Vorgänge ausführen).</span><span class="sxs-lookup"><span data-stu-id="2b0b0-269">To isolate the controller action we’ll need the ability to switch between the real unit of work (backed by an ObjectContext) and a test double or “fake” unit of work (performing in-memory operations).</span></span> <span data-ttu-id="2b0b0-270">Die übliche Methode zum Führen Sie diese Art von wechseln besteht darin, dass nicht die MVC-Controller eine Arbeitseinheit, sondern übergeben die Arbeitseinheit in den Controller als Konstruktorparameter instanziieren.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-270">The common approach to perform this type of switching is to not let the MVC controller instantiate a unit of work, but instead pass the unit of work into the controller as a constructor parameter.</span></span>

``` csharp
    class EmployeeController : Controller {
      publicEmployeeController(IUnitOfWork unitOfWork)  {
          _unitOfWork = unitOfWork;
      }
      ...
    }
```

<span data-ttu-id="2b0b0-271">Der obige Code ist ein Beispiel der Abhängigkeitsinjektion.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-271">The above code is an example of dependency injection.</span></span> <span data-ttu-id="2b0b0-272">Wir zulassen nicht, dass den Controller, erstellen sie die Abhängigkeit (die Arbeitseinheit), aber die Abhängigkeit in den Controller injizieren.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-272">We don’t allow the controller to create it’s dependency (the unit of work) but inject the dependency into the controller.</span></span> <span data-ttu-id="2b0b0-273">In einem MVC-Projekt ist es üblich, eine benutzerdefinierten Controllerfactory in Kombination mit Inversion of Control-Container (IoC) zu verwenden, um die Abhängigkeitsinjektion zu automatisieren.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-273">In an MVC project it is common to use a custom controller factory in combination with an inversion of control (IoC) container to automate dependency injection.</span></span> <span data-ttu-id="2b0b0-274">Diese Themen können Sie über den Rahmen dieses Artikels hinaus, aber Sie können mehr durch nach den verweisen am Ende dieses Artikels lesen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-274">These topics are beyond the scope of this article, but you can read more by following the references at the end of this article.</span></span>

<span data-ttu-id="2b0b0-275">Eine gefälschte Einheit arbeiten-Implementierung, die wir für Tests verwenden können, kann wie folgt aussehen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-275">A fake unit of work implementation that we can use for testing might look like the following.</span></span>

``` csharp
    public class InMemoryUnitOfWork : IUnitOfWork {
        public InMemoryUnitOfWork() {
            Committed = false;
        }
        public IObjectSet<Employee> Employees {
            get;
            set;
        }

        public IObjectSet<TimeCard> TimeCards {
            get;
            set;
        }

        public bool Committed { get; set; }
        public void Commit() {
            Committed = true;
        }
    }
```

<span data-ttu-id="2b0b0-276">Beachten Sie, dass die gefälschte Arbeitseinheit eine Commit-Eigenschaft verfügbar macht.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-276">Notice the fake unit of work exposes a Commited property.</span></span> <span data-ttu-id="2b0b0-277">Manchmal ist es nützlich, um das Hinzufügen von Funktionen zu einer gefälschten-Klasse, die Tests zu erleichtern.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-277">It’s sometimes useful to add features to a fake class that facilitate testing.</span></span> <span data-ttu-id="2b0b0-278">In diesem Fall ist es leicht zu sehen, wenn der Code eine Arbeitseinheit committet durch Überprüfen der-Eigenschaft ein Commit ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-278">In this case it is easy to observe if code commits a unit of work by checking the Commited property.</span></span>

<span data-ttu-id="2b0b0-279">Wir benötigen auch eine gefälschte IObjectSet&lt;T&gt; , Employee "und" Zeitkartenverwaltung Objekte im Arbeitsspeicher.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-279">We’ll also need a fake IObjectSet&lt;T&gt; to hold Employee and TimeCard objects in memory.</span></span> <span data-ttu-id="2b0b0-280">Wir bieten eine einzelne Implementierung verwenden von Generika.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-280">We can provide a single implementation using generics.</span></span>

``` csharp
    public class InMemoryObjectSet<T> : IObjectSet<T> where T : class
        public InMemoryObjectSet()
            : this(Enumerable.Empty<T>()) {
        }
        public InMemoryObjectSet(IEnumerable<T> entities) {
            _set = new HashSet<T>();
            foreach (var entity in entities) {
                _set.Add(entity);
            }
            _queryableSet = _set.AsQueryable();
        }
        public void AddObject(T entity) {
            _set.Add(entity);
        }
        public void Attach(T entity) {
            _set.Add(entity);
        }
        public void DeleteObject(T entity) {
            _set.Remove(entity);
        }
        public void Detach(T entity) {
            _set.Remove(entity);
        }
        public Type ElementType {
            get { return _queryableSet.ElementType; }
        }
        public Expression Expression {
            get { return _queryableSet.Expression; }
        }
        public IQueryProvider Provider {
            get { return _queryableSet.Provider; }
        }
        public IEnumerator<T> GetEnumerator() {
            return _set.GetEnumerator();
        }
        IEnumerator IEnumerable.GetEnumerator() {
            return GetEnumerator();
        }

        readonly HashSet<T> _set;
        readonly IQueryable<T> _queryableSet;
    }
```

<span data-ttu-id="2b0b0-281">Dieses Testdouble delegiert die meisten seiner Arbeit an einer zugrunde liegenden HashSet&lt;T&gt; Objekt.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-281">This test double delegates most of its work to an underlying HashSet&lt;T&gt; object.</span></span> <span data-ttu-id="2b0b0-282">Beachten Sie, IObjectSet&lt;T&gt; erfordert eine generische Einschränkung T als eine Klasse (Referenztyp) erzwingen und erzwingt auch die "IQueryable" implementieren wir&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-282">Note that IObjectSet&lt;T&gt; requires a generic constraint enforcing T as a class (a reference type), and also forces us to implement IQueryable&lt;T&gt;.</span></span> <span data-ttu-id="2b0b0-283">Es ist einfach eine in-Memory-Auflistung, die als ein "IQueryable" angezeigt werden,&lt;T&gt; mit dem standardmäßigen LINQ-Operator AsQueryable.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-283">It is easy to make an in-memory collection appear as an IQueryable&lt;T&gt; using the standard LINQ operator AsQueryable.</span></span>

### <a name="the-tests"></a><span data-ttu-id="2b0b0-284">Die Tests</span><span class="sxs-lookup"><span data-stu-id="2b0b0-284">The Tests</span></span>

<span data-ttu-id="2b0b0-285">Herkömmlicher Komponententests werden eine einzelnen Testklasse für alle Tests für alle Aktionen in einem einzelnen MVC-Controller verwenden.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-285">Traditional unit tests will use a single test class to hold all of the tests for all of the actions in a single MVC controller.</span></span> <span data-ttu-id="2b0b0-286">Können schreiben wir diese Tests oder jede Art von Komponententests mit den in-Memory-fakes erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-286">We can write these tests, or any type of unit test, using the in memory fakes we’ve built.</span></span> <span data-ttu-id="2b0b0-287">Allerdings für diesen Artikel, die wir zu vermeiden, wird der monolithischen Anwendung Klasse Ansatz zu testen und gruppieren stattdessen unsere Tests auf einen bestimmten Teil der Funktionalität konzentrieren.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-287">However, for this article we will avoid the monolithic test class approach and instead group our tests to focus on a specific piece of functionality.</span></span>  <span data-ttu-id="2b0b0-288">Gebilligt z. B. "erstellen" möglicherweise die Funktionalität, die getestet werden soll, damit wir eine einzelnen Testklasse verwenden wird, um zu überprüfen, ob die einzelnen Controlleraktion, die verantwortlich für das Erstellen eines neuen Mitarbeiters.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-288">For example, “create new employee” might be the functionality we want to test, so we will use a single test class to verify the single controller action responsible for creating a new employee.</span></span>

<span data-ttu-id="2b0b0-289">Es gibt einige allgemeine Setupcode, die für alle diese differenzierte Testklassen erforderlich.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-289">There is some common setup code we need for all these fine grained test classes.</span></span> <span data-ttu-id="2b0b0-290">Beispielsweise müssen wir immer unsere in-Memory-Repositorys und gefälschte Unit of Work, zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-290">For example, we always need to create our in-memory repositories and fake unit of work.</span></span> <span data-ttu-id="2b0b0-291">Wir benötigen auch eine Instanz des Controllers Mitarbeiter die gefälschte Arbeitseinheit eingefügt.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-291">We also need an instance of the employee controller with the fake unit of work injected.</span></span> <span data-ttu-id="2b0b0-292">Wir werden diese allgemeine Setupcode für Testklassen freigeben, mithilfe von einer Basisklasse.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-292">We’ll share this common setup code across test classes by using a base class.</span></span>

``` csharp
    public class EmployeeControllerTestBase {
        public EmployeeControllerTestBase() {
            _employeeData = EmployeeObjectMother.CreateEmployees()
                                                .ToList();
            _repository = new InMemoryObjectSet<Employee>(_employeeData);
            _unitOfWork = new InMemoryUnitOfWork();
            _unitOfWork.Employees = _repository;
            _controller = new EmployeeController(_unitOfWork);
        }

        protected IList<Employee> _employeeData;
        protected EmployeeController _controller;
        protected InMemoryObjectSet<Employee> _repository;
        protected InMemoryUnitOfWork _unitOfWork;
    }
```

<span data-ttu-id="2b0b0-293">Wir in der Basisklasse verwenden "-Objekt Mutter" ist ein häufiges Muster für das Erstellen von Testdaten.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-293">The “object mother” we use in the base class is one common pattern for creating test data.</span></span> <span data-ttu-id="2b0b0-294">Ein Objekt Mutter enthält Factorymethoden zum testentitäten für die Verwendung in mehreren prüfvorrichtungen zu instanziieren.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-294">An object mother contains factory methods to instantiate test entities for use across multiple test fixtures.</span></span>

``` csharp
    public static class EmployeeObjectMother {
        public static IEnumerable<Employee> CreateEmployees() {
            yield return new Employee() {
                Id = 1, Name = "Scott", HireDate=new DateTime(2002, 1, 1)
            };
            yield return new Employee() {
                Id = 2, Name = "Poonam", HireDate=new DateTime(2001, 1, 1)
            };
            yield return new Employee() {
                Id = 3, Name = "Simon", HireDate=new DateTime(2008, 1, 1)
            };
        }
        // ... more fake data for different scenarios
    }
```

<span data-ttu-id="2b0b0-295">Wir können die EmployeeControllerTestBase als Basisklasse für eine Reihe von prüfvorrichtungen verwenden (siehe Abbildung 3).</span><span class="sxs-lookup"><span data-stu-id="2b0b0-295">We can use the EmployeeControllerTestBase as the base class for a number of test fixtures (see figure 3).</span></span> <span data-ttu-id="2b0b0-296">Jeder Testfixture testet eine bestimmte Controlleraktion.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-296">Each test fixture will test a specific controller action.</span></span> <span data-ttu-id="2b0b0-297">Z. B. eine Testfixture konzentriert sich auf den Test der erstellen-Aktion, die während einer HTTP GET-Anforderung (für die Ansicht für die Erstellung eines Mitarbeiters Anzeige) verwendet und eine andere Fixierung konzentriert sich auf die Create-Aktion, die in einer HTTP POST-Anforderung verwendet (von übermittelt Informationen zu den Benutzer, der einen Mitarbeiter zu erstellen).</span><span class="sxs-lookup"><span data-stu-id="2b0b0-297">For example, one test fixture will focus on testing the Create action used during an HTTP GET request (to display the view for creating an employee), and a different fixture will focus on the Create action used in an HTTP POST request (to take information submitted by the user to create an employee).</span></span> <span data-ttu-id="2b0b0-298">Jede abgeleitete Klasse ist nur für das Setup erforderlich sind, in der spezifischen Kontext und geben Sie die Assertionen, die erforderlich sind, um zu überprüfen, ob die Ergebnisse für die Kontext-spezifischer Test verantwortlich.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-298">Each derived class is only responsible for the setup needed in its specific context, and to provide the assertions needed to verify the outcomes for its specific test context.</span></span>

![eftest_03](~/ef6/media/eftest-03.png)

<span data-ttu-id="2b0b0-300">**Abbildung 3**</span><span class="sxs-lookup"><span data-stu-id="2b0b0-300">**Figure 3**</span></span>

<span data-ttu-id="2b0b0-301">Die Aufrufkonvention und der Test Benennungsstil hier vorgestellten nicht für testbarem Code erforderlich ist – es nur einen Ansatz ist.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-301">The naming convention and test style presented here isn’t required for testable code – it’s just one approach.</span></span> <span data-ttu-id="2b0b0-302">Abbildung 4 zeigt, dass die Tests ausgeführt werden, in der Jet-Gehirne Resharper Runner-Plug-In für Visual Studio 2010 testen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-302">Figure 4 shows the tests running in the Jet Brains Resharper test runner plugin for Visual Studio 2010.</span></span>

![eftest_04](~/ef6/media/eftest-04.png)

<span data-ttu-id="2b0b0-304">**Abbildung 4**</span><span class="sxs-lookup"><span data-stu-id="2b0b0-304">**Figure 4**</span></span>

<span data-ttu-id="2b0b0-305">Sind mit einer Basisklasse, die freigegebene Setupcode zu behandeln die Komponententests für jede Controlleraktion, klein und einfach zu schreiben.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-305">With a base class to handle the shared setup code, the unit tests for each controller action are small and easy to write.</span></span> <span data-ttu-id="2b0b0-306">Die Tests schnell (da wir in-Memory-Vorgängen durchgeführt wird) ausgeführt werden, und sollte nicht aufgrund von-Infrastruktur oder umweltbeeinträchtigungen bei fehlschlagen, (da wir die Komponenten unter testbedingungen isoliert haben).</span><span class="sxs-lookup"><span data-stu-id="2b0b0-306">The tests will execute quickly (since we are performing in-memory operations), and shouldn’t fail because of unrelated infrastructure or environmental concerns (because we’ve isolated the unit under test).</span></span>

``` csharp
    [TestClass]
    public class EmployeeControllerCreateActionPostTests
               : EmployeeControllerTestBase {
        [TestMethod]
        public void ShouldAddNewEmployeeToRepository() {
            _controller.Create(_newEmployee);
            Assert.IsTrue(_repository.Contains(_newEmployee));
        }
        [TestMethod]
        public void ShouldCommitUnitOfWork() {
            _controller.Create(_newEmployee);
            Assert.IsTrue(_unitOfWork.Committed);
        }
        // ... more tests

        Employee _newEmployee = new Employee() {
            Name = "NEW EMPLOYEE",
            HireDate = new System.DateTime(2010, 1, 1)
        };
    }
```

<span data-ttu-id="2b0b0-307">Bei diesen Tests wird die Basisklasse die meiste Arbeit Setup.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-307">In these tests, the base class does most of the setup work.</span></span> <span data-ttu-id="2b0b0-308">Denken Sie daran, dass der Konstruktor der Basisklasse der in-Memory-Repository, eine gefälschte Arbeitseinheit und eine Instanz der Klasse EmployeeController erstellt.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-308">Remember the base class constructor creates the in-memory repository, a fake unit of work, and an instance of the EmployeeController class.</span></span> <span data-ttu-id="2b0b0-309">Die Testklasse wird von dieser Basisklasse abgeleitet und konzentriert sich auf die Einzelheiten die Create-Methode zu testen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-309">The test class derives from this base class and focuses on the specifics of testing the Create method.</span></span> <span data-ttu-id="2b0b0-310">In diesem Fall erzwingungsebenen die Besonderheiten, die "anordnen, agieren und Assertion"-Schritte, die Sie in alle Verfahren für die Komponententests sehen:</span><span class="sxs-lookup"><span data-stu-id="2b0b0-310">In this case the specifics boil down to the “arrange, act, and assert” steps you’ll see in any unit testing procedure:</span></span>

-   <span data-ttu-id="2b0b0-311">Erstellen Sie ein NeuerMitarbeiter-Objekt, um eingehende Daten zu simulieren.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-311">Create a newEmployee object to simulate incoming data.</span></span>
-   <span data-ttu-id="2b0b0-312">Rufen Sie die Create-Aktion, der die EmployeeController, und die NeuerMitarbeiter übergeben.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-312">Invoke the Create action of the EmployeeController and pass in the newEmployee.</span></span>
-   <span data-ttu-id="2b0b0-313">Stellen Sie sicher, dass die Aktion "Create" die erwarteten Ergebnisse erzeugt werden (der Mitarbeiter, die in das Repository angezeigt wird).</span><span class="sxs-lookup"><span data-stu-id="2b0b0-313">Verify the Create action produces the expected results (the employee appears in the repository).</span></span>

<span data-ttu-id="2b0b0-314">Was wir erstellt haben, können wir die EmployeeController Aktionen zu testen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-314">What we’ve built allows us to test any of the EmployeeController actions.</span></span> <span data-ttu-id="2b0b0-315">Wenn wir Schreiben von Tests für die Index-Aktion des Controllers Mitarbeiter können wir beispielsweise von der Basisklasse "Test" zu, um die gleiche grundlegende Einrichtung für unsere Tests herzustellen erben.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-315">For example, when we write tests for the Index action of the Employee controller we can inherit from the test base class to establish the same base setup for our tests.</span></span> <span data-ttu-id="2b0b0-316">Die Basisklasse wird erneut das Repository im Arbeitsspeicher, die falsche Arbeitseinheit und einer Instanz von der EmployeeController erstellt.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-316">Again the base class will create the in-memory repository, the fake unit of work, and an instance of the EmployeeController.</span></span> <span data-ttu-id="2b0b0-317">Die Tests für die Index-Aktion nur konzentrieren, die Index-Aktion aufrufen müssen, und gibt die Qualität des Modells die Aktion zu testen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-317">The tests for the Index action only need to focus on invoking the Index action and testing the qualities of the model the action returns.</span></span>

``` csharp
    [TestClass]
    public class EmployeeControllerIndexActionTests
               : EmployeeControllerTestBase {
        [TestMethod]
        public void ShouldBuildModelWithAllEmployees() {
            var result = _controller.Index();
            var model = result.ViewData.Model
                          as IEnumerable<Employee>;
            Assert.IsTrue(model.Count() == _employeeData.Count);
        }
        [TestMethod]
        public void ShouldOrderModelByHiredateAscending() {
            var result = _controller.Index();
            var model = result.ViewData.Model
                         as IEnumerable<Employee>;
            Assert.IsTrue(model.SequenceEqual(
                           _employeeData.OrderBy(e => e.HireDate)));
        }
        // ...
    }
```

<span data-ttu-id="2b0b0-318">Die Tests, die erstellt wird, wird mithilfe von Fakes für im Arbeitsspeicher sind auf die Tests ausgerichtet der *Zustand* der Software.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-318">The tests we are creating with in-memory fakes are oriented towards testing the *state* of the software.</span></span> <span data-ttu-id="2b0b0-319">Beispielsweise enthalten beim Testen der Aktion "Create", untersuchen Sie den Status des Repositorys ein, nachdem die Aktion "Create" ausgeführt wird – sollten, im Repository des neuen Mitarbeiters?</span><span class="sxs-lookup"><span data-stu-id="2b0b0-319">For example, when testing the Create action we want to inspect the state of the repository after the create action executes – does the repository hold the new employee?</span></span>

``` csharp
    [TestMethod]
    public void ShouldAddNewEmployeeToRepository() {
        _controller.Create(_newEmployee);
        Assert.IsTrue(_repository.Contains(_newEmployee));
    }
```

<span data-ttu-id="2b0b0-320">Später werden wir Interaktion basierten testen betrachten.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-320">Later we’ll look at interaction based testing.</span></span> <span data-ttu-id="2b0b0-321">Testen der Interaktion basierend fragt, ob es sich bei der getesteten Code die entsprechenden Methoden für unsere Objekte aufgerufen und die richtigen Parameter übergeben.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-321">Interaction based testing will ask if the code under test invoked the proper methods on our objects and passed the correct parameters.</span></span> <span data-ttu-id="2b0b0-322">Jetzt werden wir die Abdeckung einen anderen-Entwurfsmuster – lazy Loading verschieben.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-322">For now we’ll move on the cover another design pattern – the lazy load.</span></span>

## <a name="eager-loading-and-lazy-loading"></a><span data-ttu-id="2b0b0-323">Eager Load und Lazy Loading</span><span class="sxs-lookup"><span data-stu-id="2b0b0-323">Eager Loading and Lazy Loading</span></span>

<span data-ttu-id="2b0b0-324">An einem bestimmten Punkt in der ASP.NET MVC-Web zugeordnete Anwendung, die wir möglicherweise eines Mitarbeiters-Informationen anzeigen und des Mitarbeiters einschließen möchten Zeit Karten an.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-324">At some point in the ASP.NET  MVC web application we might wish to display an employee’s information and include the employee’s associated time cards.</span></span> <span data-ttu-id="2b0b0-325">Wir möglicherweise z. B. eine Zeitpunkt-Karte angezeigt werden sollen, die den Namen des Mitarbeiters und die Gesamtzahl der Zeit Karten im System angezeigt.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-325">For example, we might have a time card summary display that shows the employee’s name and the total number of time cards in the system.</span></span> <span data-ttu-id="2b0b0-326">Es gibt verschiedene Ansätze, die wir ausführen können, um dieses Feature zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-326">There are several approaches we can take to implement this feature.</span></span>

### <a name="projection"></a><span data-ttu-id="2b0b0-327">Projection</span><span class="sxs-lookup"><span data-stu-id="2b0b0-327">Projection</span></span>

<span data-ttu-id="2b0b0-328">Ein einfacher Ansatz zum Erstellen der Zusammenfassung ist zum Erstellen eines Modells, die speziell für die Informationen, die wir in der Ansicht angezeigt werden soll.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-328">One easy approach to create the summary is to construct a model dedicated to the information we want to display in the view.</span></span> <span data-ttu-id="2b0b0-329">In diesem Szenario kann das Modell wie folgt aussehen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-329">In this scenario the model might look like the following.</span></span>

``` csharp
    public class EmployeeSummaryViewModel {
        public string Name { get; set; }
        public int TotalTimeCards { get; set; }
    }
```

<span data-ttu-id="2b0b0-330">Beachten Sie, dass die EmployeeSummaryViewModel keine Entität ist – anders ausgedrückt: Es ist nicht etwas, das wir in der Datenbank beibehalten möchten.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-330">Note that the EmployeeSummaryViewModel is not an entity – in other words it is not something we want to persist in the database.</span></span> <span data-ttu-id="2b0b0-331">Wir werden nur diese Klasse verwenden, um Daten in die Ansicht in einem strikter Typzuordnung zu mischen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-331">We are only going to use this class to shuffle data into the view in a strongly typed manner.</span></span> <span data-ttu-id="2b0b0-332">Das Ansichtsmodell ist wie eine Daten übertragen (Object, DTO), da er kein Verhalten (keine Methoden) – nur Eigenschaften enthält.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-332">The view model is like a data transfer object (DTO) because it contains no behavior (no methods) – only properties.</span></span> <span data-ttu-id="2b0b0-333">Die Eigenschaften werden die Daten aufzunehmen, die wir verschieben müssen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-333">The properties will hold the data we need to move.</span></span> <span data-ttu-id="2b0b0-334">Es ist einfach, dieses Modell anzeigen, die mithilfe LINQs standard projektionsoperator – der Select-Operator zu instanziieren.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-334">It is easy to instantiate this view model using LINQ’s standard projection operator – the Select operator.</span></span>

``` csharp
    public ViewResult Summary(int id) {
        var model = _unitOfWork.Employees
                               .Where(e => e.Id == id)
                               .Select(e => new EmployeeSummaryViewModel
                                  {
                                    Name = e.Name,
                                    TotalTimeCards = e.TimeCards.Count()
                                  })
                               .Single();
        return View(model);
    }
```

<span data-ttu-id="2b0b0-335">Es gibt zwei wichtige Features, dem obigen Code.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-335">There are two notable features to the above code.</span></span> <span data-ttu-id="2b0b0-336">Zuerst – ist der Code einfach zu testen, da es immer noch leicht ist zu überwachen und zu isolieren.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-336">First – the code is easy to test because it is still easy to observe and isolate.</span></span> <span data-ttu-id="2b0b0-337">Der Select-Operator funktioniert ebenso gut für unsere in-Memory-Fakes wie für die echten Arbeitseinheit.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-337">The Select operator works just as well against our in-memory fakes as it does against the real unit of work.</span></span>

``` csharp
    [TestClass]
    public class EmployeeControllerSummaryActionTests
               : EmployeeControllerTestBase {
        [TestMethod]
        public void ShouldBuildModelWithCorrectEmployeeSummary() {
            var id = 1;
            var result = _controller.Summary(id);
            var model = result.ViewData.Model as EmployeeSummaryViewModel;
            Assert.IsTrue(model.TotalTimeCards == 3);
        }
        // ...
    }
```

<span data-ttu-id="2b0b0-338">Die zweite wichtige Funktion ist wie der Code EF4, generieren Sie eine einmalige und effiziente-Abfrage, um Informationen für Mitarbeiter und Zeit-Karte gemeinsam zusammenstellen kann.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-338">The second notable feature is how the code allows EF4 to generate a single, efficient query to assemble employee and time card information together.</span></span> <span data-ttu-id="2b0b0-339">Wir haben Informationen zu Mitarbeitern und Zeit-Karteninformationen in das gleiche Objekt geladen, ohne spezielle APIs verwenden.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-339">We’ve loaded employee information and time card information into the same object without using any special APIs.</span></span> <span data-ttu-id="2b0b0-340">Der Code ausgedrückt lediglich die Informationen, die es erfordert die Verwendung der standardmäßiger LINQ-Standardabfrageoperatoren, die in-Memory-Datenquellen sowie Remotedatenquellen entgegenwirken.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-340">The code merely expressed the information it requires using standard LINQ operators that work against in-memory data sources as well as remote data sources.</span></span> <span data-ttu-id="2b0b0-341">EF4 wurde die Ausdrucksbaumstrukturen, die von der LINQ-Abfrage und C generiert übersetzen\# Compiler in eine einmalige und effiziente T-SQL-Abfrage.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-341">EF4 was able to translate the expression trees generated by the LINQ query and C\# compiler into a single and efficient T-SQL query.</span></span>

``` SQL
    SELECT
    [Limit1].[Id] AS [Id],
    [Limit1].[Name] AS [Name],
    [Limit1].[C1] AS [C1]
    FROM (SELECT TOP (2)
      [Project1].[Id] AS [Id],
      [Project1].[Name] AS [Name],
      [Project1].[C1] AS [C1]
      FROM (SELECT
        [Extent1].[Id] AS [Id],
        [Extent1].[Name] AS [Name],
        (SELECT COUNT(1) AS [A1]
         FROM [dbo].[TimeCards] AS [Extent2]
         WHERE [Extent1].[Id] =
               [Extent2].[EmployeeTimeCard_TimeCard_Id]) AS [C1]
              FROM [dbo].[Employees] AS [Extent1]
               WHERE [Extent1].[Id] = @p__linq__0
         )  AS [Project1]
    )  AS [Limit1]
```

<span data-ttu-id="2b0b0-342">Es gibt in anderen Fällen, wenn es nicht mit einem Ansichtsmodell oder DTO-Objekt, jedoch mit echten Entitäten verwenden möchten.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-342">There are other times when we don’t want to work with a view model or DTO object, but with real entities.</span></span> <span data-ttu-id="2b0b0-343">Wenn wir wissen, dass wir einen Mitarbeiter benötigen *und* des Mitarbeiters Zeitkarten, wir können die verwandten Daten sorgfältig auf unaufdringliche und effiziente Weise laden.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-343">When we know we need an employee *and* the employee’s time cards, we can eagerly load the related data in an unobtrusive and efficient manner.</span></span>

### <a name="explicit-eager-loading"></a><span data-ttu-id="2b0b0-344">Explizite Eager Loading</span><span class="sxs-lookup"><span data-stu-id="2b0b0-344">Explicit Eager Loading</span></span>

<span data-ttu-id="2b0b0-345">Wenn wir die verknüpfte Entität-Informationen, die wir benötigen einen Mechanismus, für die Geschäftslogik (oder in diesem Szenario wird die Aktion der Controllerlogik) laden möchten, ihren Wunsch an das Repository auszudrücken.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-345">When we want to eagerly load related entity information we need some mechanism for business logic (or in this scenario, controller action logic) to express its desire to the repository.</span></span> <span data-ttu-id="2b0b0-346">Die EF4 ObjectQuery&lt;T&gt; -Klasse definiert eine Include-Methode, um anzugeben, die verknüpften Objekte während der Abfrage abgerufen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-346">The EF4 ObjectQuery&lt;T&gt; class defines an Include method to specify the related objects to retrieve during a query.</span></span> <span data-ttu-id="2b0b0-347">Denken Sie daran, EF4 ObjectContext verfügbar macht, Entitäten über die konkreten "ObjectSet"&lt;T&gt; die von ObjectQuery erbt&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-347">Remember the EF4 ObjectContext exposes entities via the concrete ObjectSet&lt;T&gt; class which inherits from ObjectQuery&lt;T&gt;.</span></span>  <span data-ttu-id="2b0b0-348">Wenn wir mit "ObjectSet" verwendeten&lt;T&gt; Verweise in unserer Controlleraktion schreiben wir den folgenden Code hinzu, geben ein unverzüglichem Laden von Time-Karteninformationen für jeden Mitarbeiter.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-348">If we were using ObjectSet&lt;T&gt; references in our controller action we could write the following code to specify an eager load of time card information for each employee.</span></span>

``` csharp
    _employees.Include("TimeCards")
              .Where(e => e.HireDate.Year > 2009);
```

<span data-ttu-id="2b0b0-349">Aber da wir unseren Code testbar möchten wir nicht offen "ObjectSet"&lt;T&gt; von außerhalb der echten Maßeinheit Arbeit-Klasse.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-349">However, since we are trying to keep our code testable we are not exposing ObjectSet&lt;T&gt; from outside the real unit of work class.</span></span> <span data-ttu-id="2b0b0-350">Stattdessen wir abhängig von der IObjectSet&lt;T&gt; -Schnittstelle, um falsche vereinfacht wird, aber IObjectSet&lt;T&gt; definiert eine "Include"-Methode nicht.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-350">Instead, we rely on the IObjectSet&lt;T&gt; interface which is easier to fake, but IObjectSet&lt;T&gt; does not define an Include method.</span></span> <span data-ttu-id="2b0b0-351">Das Schöne an LINQ ist, dass wir unseren eigenen Include-Operator erstellen können.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-351">The beauty of LINQ is that we can create our own Include operator.</span></span>

``` csharp
    public static class QueryableExtensions {
        public static IQueryable<T> Include<T>
                (this IQueryable<T> sequence, string path) {
            var objectQuery = sequence as ObjectQuery<T>;
            if(objectQuery != null)
            {
                return objectQuery.Include(path);
            }
            return sequence;
        }
    }
```

<span data-ttu-id="2b0b0-352">Beachten Sie, dass dieser Include-Operator wird als eine Erweiterungsmethode definiert, für die "IQueryable"&lt;T&gt; IObjectSet&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-352">Notice this Include operator is defined as an extension method for IQueryable&lt;T&gt; instead of IObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="2b0b0-353">Dies ergibt die Möglichkeit, verwenden die Methode mit einer größeren Auswahl der möglichen Typen, einschließlich "IQueryable"&lt;T&gt;, IObjectSet&lt;T&gt;, ObjectQuery&lt;T&gt;, und "ObjectSet"&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-353">This gives us the ability to use the method with a wider range of possible types, including IQueryable&lt;T&gt;, IObjectSet&lt;T&gt;, ObjectQuery&lt;T&gt;, and ObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="2b0b0-354">Falls die zugrunde liegenden Sequenz keine echte EF4 ObjectQuery ist&lt;T&gt;, gibt es keinen Schaden durchgeführt und der Include-Operator ist keine Aktion.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-354">In the event the underlying sequence is not a genuine EF4 ObjectQuery&lt;T&gt;, then there is no harm done and the Include operator is a no-op.</span></span> <span data-ttu-id="2b0b0-355">Wenn die zugrunde liegende sequenzieren *ist* ObjectQuery&lt;T&gt; (oder ObjectQuery abgeleitet&lt;T&gt;), EF4 finden Sie unter der Anforderung für zusätzliche Daten und formulieren die richtige SQL wird Abfrage.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-355">If the underlying sequence *is* an ObjectQuery&lt;T&gt; (or derived from ObjectQuery&lt;T&gt;), then EF4 will see our requirement for additional data and formulate the proper SQL query.</span></span>

<span data-ttu-id="2b0b0-356">Mit dieser neuen Operator direktes können wir ein unverzüglichem Laden von Karteninformationen Zeit explizit aus dem Repository anfordern.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-356">With this new operator in place we can explicitly request an eager load of time card information from the repository.</span></span>

``` csharp
    public ViewResult Index() {
        var model = _unitOfWork.Employees
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

<span data-ttu-id="2b0b0-357">Wenn für eine echte ObjectContext ausführen, erstellt der Code die folgende Abfrage für die einzelne.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-357">When run against a real ObjectContext, the code produces the following single query.</span></span> <span data-ttu-id="2b0b0-358">Die Abfrage sammelt genug Informationen aus der Datenbank in einen Trip zur materialisieren der Employee-Objekten und vollständig auffüllen ihrer Rückkehr-Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-358">The query gathers enough information from the database in one trip to materialize the employee objects and fully populate their TimeCards property.</span></span>

``` SQL
    SELECT
    [Project1].[Id] AS [Id],
    [Project1].[Name] AS [Name],
    [Project1].[HireDate] AS [HireDate],
    [Project1].[C1] AS [C1],
    [Project1].[Id1] AS [Id1],
    [Project1].[Hours] AS [Hours],
    [Project1].[EffectiveDate] AS [EffectiveDate],
    [Project1].[EmployeeTimeCard_TimeCard_Id] AS [EmployeeTimeCard_TimeCard_Id]
    FROM ( SELECT
         [Extent1].[Id] AS [Id],
         [Extent1].[Name] AS [Name],
         [Extent1].[HireDate] AS [HireDate],
         [Extent2].[Id] AS [Id1],
         [Extent2].[Hours] AS [Hours],
         [Extent2].[EffectiveDate] AS [EffectiveDate],
         [Extent2].[EmployeeTimeCard_TimeCard_Id] AS
                    [EmployeeTimeCard_TimeCard_Id],
         CASE WHEN ([Extent2].[Id] IS NULL) THEN CAST(NULL AS int)
         ELSE 1 END AS [C1]
         FROM  [dbo].[Employees] AS [Extent1]
         LEFT OUTER JOIN [dbo].[TimeCards] AS [Extent2] ON [Extent1].[Id] = [Extent2].[EmployeeTimeCard_TimeCard_Id]
    )  AS [Project1]
    ORDER BY [Project1].[HireDate] ASC,
             [Project1].[Id] ASC, [Project1].[C1] ASC
```

<span data-ttu-id="2b0b0-359">Die gute Nachricht ist, dass der Code innerhalb der Aktionsmethode bleibt, die vollständig getestet werden.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-359">The great news is the code inside the action method remains fully testable.</span></span> <span data-ttu-id="2b0b0-360">Wir müssen keine zusätzlichen Funktionen für unsere Fakes zur Unterstützung des Include-Operators angeben.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-360">We don’t need to provide any additional features for our fakes to support the Include operator.</span></span> <span data-ttu-id="2b0b0-361">Die schlechte Nachricht ist, dass den Include-Operator in der Code verwendet, wollten wir auch Persistenz ignorierende, werden musste.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-361">The bad news is we had to use the Include operator inside of the code we wanted to keep persistence ignorant.</span></span> <span data-ttu-id="2b0b0-362">Dies ist ein hervorragendes Beispiel für den Typ der Kompromisse, die Sie beim Entwickeln testfähigen Codeabschnitt ordnungsgemäß ausgewertet werden müssen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-362">This is a prime example of the type of tradeoffs you’ll need to evaluate when building testable code.</span></span> <span data-ttu-id="2b0b0-363">Es gibt Situationen, Sie Persistenz Bedenken Speicherverlusten außerhalb der Abstraktion Repository für die Leistungsziele zu ermöglichen müssen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-363">There are times when you need to let persistence concerns leak outside the repository abstraction to meet performance goals.</span></span>

<span data-ttu-id="2b0b0-364">Die Alternative zur Eager Load ist lazy Loading.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-364">The alternative to eager loading is lazy loading.</span></span> <span data-ttu-id="2b0b0-365">Lazy loading-bedeutet, dass wir tun *nicht* benötigen unsere Business-Code, um die Anforderung für die zugehörigen Daten explizit bekanntgeben zu können.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-365">Lazy loading means we do *not* need our business code to explicitly announce the requirement for associated data.</span></span> <span data-ttu-id="2b0b0-366">Stattdessen verwenden wir die Entitäten in der Anwendung und zusätzliche Daten benötigt Entity Framework lädt die Daten nach Bedarf.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-366">Instead, we use our entities in the application and if additional data is needed Entity Framework will load the data on demand.</span></span>

### <a name="lazy-loading"></a><span data-ttu-id="2b0b0-367">Lazy Loading</span><span class="sxs-lookup"><span data-stu-id="2b0b0-367">Lazy Loading</span></span>

<span data-ttu-id="2b0b0-368">Es ist einfach, stellen Sie sich ein Szenario vor, in denen wir nicht wissen, welche Daten ein Teil der Geschäftslogik müssen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-368">It’s easy to imagine a scenario where we don’t know what data a piece of business logic will need.</span></span> <span data-ttu-id="2b0b0-369">Wir unter Umständen kennen Sie die Logik benötigt ein Employee-Objekt, aber wir Verzweigung in unterschiedliche Ausführungspfade, in denen einige dieser Pfade erfordern Zeit Informationen aus der Mitarbeiter und andere nicht.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-369">We might know the logic needs an employee object, but we may branch into different execution paths where some of those paths require time card information from the employee, and some do not.</span></span> <span data-ttu-id="2b0b0-370">Szenarios wie diese sind ideal für implizite verzögerten Laden, da Daten wie von Zauberhand auf einen Bedarf angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-370">Scenarios like this are perfect for implicit lazy loading because data magically appears on an as-needed basis.</span></span>

<span data-ttu-id="2b0b0-371">Lazy Loading, auch bekannt als verzögert geladen, einige Anforderungen für unsere Entitätsobjekte platzieren.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-371">Lazy loading, also known as deferred loading, does place some requirements on our entity objects.</span></span> <span data-ttu-id="2b0b0-372">POCOs mit "true" Ignorieren der Persistenz würde keine Anforderungen von der persistenzschicht zur Verfügung stehen, aber "true" Ignorieren der Persistenz ist praktisch unmöglich, zu erreichen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-372">POCOs with true persistence ignorance would not face any requirements from the persistence layer, but true persistence ignorance is practically impossible to achieve.</span></span>  <span data-ttu-id="2b0b0-373">Stattdessen messen wir die Persistenz in relativen Grad.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-373">Instead we measure persistence ignorance in relative degrees.</span></span> <span data-ttu-id="2b0b0-374">Es wäre unglücklich, wenn wir brauchten eine dienstorientierte Persistenz-Basisklasse erben, oder verwenden eine spezielle Auflistung lazy loadings in POCOs erreichen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-374">It would be unfortunate if we needed to inherit from a persistence oriented base class or use a specialized collection to achieve lazy loading in POCOs.</span></span> <span data-ttu-id="2b0b0-375">Glücklicherweise verfügt EF4 eine Lösung mit weniger intrusive.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-375">Fortunately, EF4 has a less intrusive solution.</span></span>

### <a name="virtually-undetectable"></a><span data-ttu-id="2b0b0-376">Praktisch nicht aufzuspüren</span><span class="sxs-lookup"><span data-stu-id="2b0b0-376">Virtually Undetectable</span></span>

<span data-ttu-id="2b0b0-377">Wenn Sie POCO-Objekte zu verwenden, können EF4 dynamisch Laufzeit Proxys für Entitäten generiert werden.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-377">When using POCO objects, EF4 can dynamically generate runtime proxies for entities.</span></span> <span data-ttu-id="2b0b0-378">Diese Proxys unsichtbar umschließen die materialisierte POCOs angeben, dass zusätzliche Dienste durch Abfangen jeder Eigenschaft erhalten und set-Vorgang, um zusätzliche Vorgänge auszuführen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-378">These proxies invisibly wrap the materialized POCOs and provide additional services by intercepting each property get and set operation to perform additional work.</span></span> <span data-ttu-id="2b0b0-379">Ein solcher Dienst ist die lazy Loading-Funktion, die wir suchen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-379">One such service is the lazy loading feature we are looking for.</span></span> <span data-ttu-id="2b0b0-380">Ein anderer Dienst ist, einen effizienten änderungsnachverfolgungsmechanismus die aufzeichnen können, wenn das Programm die Eigenschaftswerte einer Entität geändert wird.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-380">Another service is an efficient change tracking mechanism which can record when the program changes the property values of an entity.</span></span> <span data-ttu-id="2b0b0-381">Die Liste der Änderungen wird vom ObjectContext während SaveChanges-Methode verwendet, alle geänderten Entitäten, die mithilfe der UPDATE-Befehle beibehalten werden.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-381">The list of changes is used by the ObjectContext during the SaveChanges method to persist any modified entities using UPDATE commands.</span></span>

<span data-ttu-id="2b0b0-382">Für diese Proxys zur Arbeit fahren allerdings sie benötigen eine Möglichkeit, Sie hook Eigenschaft Get und Set-Vorgänge für eine Entität und die Proxys dieses Ziel erreichen, indem virtuelle Member überschreiben.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-382">For these proxies to work, however, they need a way to hook into property get and set operations on an entity, and the proxies achieve this goal by overriding virtual members.</span></span> <span data-ttu-id="2b0b0-383">Daher sollten wir implizite lazy Loading und effizienten änderungsnachverfolgung haben müssen wir wechseln zurück zu unserem POCO-Klassendefinitionen und Eigenschaften als virtuell markiert.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-383">Thus, if we want to have implicit lazy loading and efficient change tracking we need to go back to our POCO class definitions and mark properties as virtual.</span></span>

``` csharp
    public class Employee {
        public virtual int Id { get; set; }
        public virtual string Name { get; set; }
        public virtual DateTime HireDate { get; set; }
        public virtual ICollection<TimeCard> TimeCards { get; set; }
    }
```

<span data-ttu-id="2b0b0-384">Wir können noch sagen, dass die Employee-Entität vor allem die keine Persistenz ist.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-384">We can still say the Employee entity is mostly persistence ignorant.</span></span> <span data-ttu-id="2b0b0-385">Die einzige Anforderung ist die Verwendung von virtuellen Member und dies hat keine Auswirkungen auf die testmöglichkeiten von Code.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-385">The only requirement is to use virtual members and this does not impact the testability of the code.</span></span> <span data-ttu-id="2b0b0-386">Wir müssen keine spezielle Basisklasse abgeleitet werden soll, oder verwenden Sie auch eine spezielle Auflistung, die speziell für verzögertes Laden.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-386">We don’t need to derive from any special base class, or even use a special collection dedicated to lazy loading.</span></span> <span data-ttu-id="2b0b0-387">Wie der Code, zeigt jede Klasse, die ICollection implementieren&lt;T&gt; ist verfügbar, um verknüpfte Entitäten zu speichern.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-387">As the code demonstrates, any class implementing ICollection&lt;T&gt; is available to hold related entities.</span></span>

<span data-ttu-id="2b0b0-388">Es gibt auch einen kleine Änderung, die wir in unserer Unit of Work, machen müssen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-388">There is also one minor change we need to make inside our unit of work.</span></span> <span data-ttu-id="2b0b0-389">Mit Lazy Loading steht *aus* standardmäßig bei der Arbeit direkt mit einem ObjectContext-Objekt.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-389">Lazy loading is *off* by default when working directly with an ObjectContext object.</span></span> <span data-ttu-id="2b0b0-390">Es ist eine Eigenschaft, die wir für die ContextOptions-Eigenschaft festlegen können, aktivieren Sie verzögertes Laden, und wir können diese Eigenschaft in unsere tatsächlichen Unit of Work, festlegen, wenn Sie möchten, aktivieren Sie das verzögerte Laden überall.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-390">There is a property we can set on the ContextOptions property to enable deferred loading, and we can set this property inside our real unit of work if we want to enable lazy loading everywhere.</span></span>

``` csharp
    public class SqlUnitOfWork : IUnitOfWork {
         public SqlUnitOfWork() {
             // ...
             _context = new ObjectContext(connectionString);
             _context.ContextOptions.LazyLoadingEnabled = true;
         }    
         // ...
     }
```

<span data-ttu-id="2b0b0-391">Implizite lazy Loading aktiviert, kann Anwendungscode einen Mitarbeiter und des Mitarbeiters Zeit Karten während des Bestehens der Arbeitsaufwand für EF zum Laden der zusätzlichen Daten nicht bekannt.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-391">With implicit lazy loading enabled, application code can use an employee and the employee’s associated time cards while remaining blissfully unaware of the work required for EF to load the extra data.</span></span>

``` csharp
    var employee = _unitOfWork.Employees
                              .Single(e => e.Id == id);
    foreach (var card in employee.TimeCards) {
        // ...
    }
```

<span data-ttu-id="2b0b0-392">Lazy Loading vereinfacht den Anwendungscode schreiben, und mit dem Proxy-Magic bleibt des Codes vollständig getestet werden.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-392">Lazy loading makes the application code easier to write, and with the proxy magic the code remains completely testable.</span></span> <span data-ttu-id="2b0b0-393">In-Memory-Fakes, die Arbeitseinheit können einfach vorab laden, gefälschte Entitäten mit zugeordneten Daten bei Bedarf während eines Tests.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-393">In-memory fakes of the unit of work can simply preload fake entities with associated data when needed during a test.</span></span>

<span data-ttu-id="2b0b0-394">An diesem Punkt werden wir unsere Aufmerksamkeit von der Erstellung des Repositorys mithilfe des IObjectSet aktivieren&lt;T&gt; und sehen Sie sich Abstraktionen, um alle Anzeichen für das persistenzframework auszublenden.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-394">At this point we’ll turn our attention from building repositories using IObjectSet&lt;T&gt; and look at abstractions to hide all signs of the persistence framework.</span></span>

## <a name="custom-repositories"></a><span data-ttu-id="2b0b0-395">Benutzerdefinierte Repositorys</span><span class="sxs-lookup"><span data-stu-id="2b0b0-395">Custom Repositories</span></span>

<span data-ttu-id="2b0b0-396">Wenn wir zunächst die Einheit der Entwurfsmuster für die Arbeit in diesem Artikel vorgestellt bereitgestellt wurde Beispielcode, für die Arbeitseinheit könnte folgendermaßen aussehen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-396">When we first presented the unit of work design pattern in this article we provided some sample code for what the unit of work might look like.</span></span> <span data-ttu-id="2b0b0-397">Erneut stellen wir diese ursprünglichen Idee, die mit dem Mitarbeiter und Mitarbeiter Zeit Karte-Szenario, das wir gearbeitet haben.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-397">Let’s re-present this original idea using the employee and employee time card scenario we’ve been working with.</span></span>

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<TimeCard> TimeCards { get;  }
        void Commit();
    }
```

<span data-ttu-id="2b0b0-398">Der Hauptunterschied zwischen dieser Arbeitseinheit und jene Arbeitseinheit, die wir im letzten Abschnitt erstellt wird, wie dieser Arbeitseinheit aller Abstraktionen von EF4 Framework nicht verwendet wird (es ist nicht IObjectSet&lt;T&gt;).</span><span class="sxs-lookup"><span data-stu-id="2b0b0-398">The primary difference between this unit of work and the unit of work we created in the last section is how this unit of work does not use any abstractions from the EF4 framework (there is no IObjectSet&lt;T&gt;).</span></span> <span data-ttu-id="2b0b0-399">IObjectSet&lt;T&gt; funktioniert gut als einer Repository-Schnittstelle, aber die API macht möglicherweise nicht perfekt im Einklang mit der Anwendung Bedürfnissen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-399">IObjectSet&lt;T&gt; works well as a repository interface, but the API it exposes might not perfectly align with our application’s needs.</span></span> <span data-ttu-id="2b0b0-400">Bei diesem Ansatz bevorstehende-Repositorys mithilfe einer benutzerdefinierten IRepository dargestellt werden&lt;T&gt; Abstraktion.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-400">In this upcoming approach we will represent repositories using a custom IRepository&lt;T&gt; abstraction.</span></span>

<span data-ttu-id="2b0b0-401">Viele Entwickler, die Test-driven Design, verhaltensgesteuerter Entwurf und Entwurf von Domain-driven Methoden führen Sie es vorziehen, die IRepository&lt;T&gt; Ansatz, verschiedene Ursachen haben.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-401">Many developers who follow test-driven design, behavior-driven design, and domain driven methodologies design prefer the IRepository&lt;T&gt; approach for several reasons.</span></span> <span data-ttu-id="2b0b0-402">Zuerst die IRepository&lt;T&gt; Schnittstelle stellt eine "antibeschädigungsebene" dar.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-402">First, the IRepository&lt;T&gt; interface represents an “anti-corruption” layer.</span></span> <span data-ttu-id="2b0b0-403">Wie in seinem Domain-Driven Design-Buch von Eric Evans beschrieben behält eine Anti-Corruption-Ebene Ihren domänencode enthalten von Infrastruktur-APIs, wie ein Persistenz-API.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-403">As described by Eric Evans in his Domain Driven Design book an anti-corruption layer keeps your domain code away from infrastructure APIs, like a persistence API.</span></span> <span data-ttu-id="2b0b0-404">Zweitens können Entwickler Methoden erstellen, in das Repository, die eine Anwendung die genauen Anforderungen erfüllen (wie beim Schreiben von Tests ermittelt).</span><span class="sxs-lookup"><span data-stu-id="2b0b0-404">Secondly, developers can build methods into the repository that meet the exact needs of an application (as discovered while writing tests).</span></span> <span data-ttu-id="2b0b0-405">Beispielsweise können wir häufig müssen eine einzelne Entität, die mit einem ID-Wert, damit die Repositoryschnittstelle eine FindById-Methode hinzugefügt werden können.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-405">For example, we might frequently need to locate a single entity using an ID value, so we can add a FindById method to the repository interface.</span></span>  <span data-ttu-id="2b0b0-406">Unsere IRepository&lt;T&gt; Definition wird wie folgt aussehen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-406">Our IRepository&lt;T&gt; definition will look like the following.</span></span>

``` csharp
    public interface IRepository<T>
                    where T : class, IEntity {
        IQueryable<T> FindAll();
        IQueryable<T> FindWhere(Expression<Func\<T, bool>> predicate);
        T FindById(int id);       
        void Add(T newEntity);
        void Remove(T entity);
    }
```

<span data-ttu-id="2b0b0-407">Beachten Sie, dass legen wir zurück zur Verwendung einer "IQueryable"&lt;T&gt; entitätsauflistungen verfügbar gemacht werden.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-407">Notice we’ll drop back to using an IQueryable&lt;T&gt; interface to expose entity collections.</span></span> <span data-ttu-id="2b0b0-408">"IQueryable"&lt;T&gt; ermöglicht LINQ-Ausdrucksbaumstrukturen flow in den EF4-Anbieter und des Anbieters eine ganzheitliche Sicht der Abfrage.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-408">IQueryable&lt;T&gt; allows LINQ expression trees to flow into the EF4 provider and give the provider a holistic view of the query.</span></span> <span data-ttu-id="2b0b0-409">Die zweite Möglichkeit wäre die "IEnumerable" zurückgeben&lt;T&gt;, was bedeutet, dass den EF4 LINQ-Anbieter wird nur angezeigt, die Ausdrücke, die in das Repository erstellt.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-409">A second option would be to return IEnumerable&lt;T&gt;, which means the EF4 LINQ provider will only see the expressions built inside of the repository.</span></span> <span data-ttu-id="2b0b0-410">Gruppierung, Sortierung und Projektion ausgeführt wird, außerhalb des Repositorys werden nicht erstellt werden, in der SQL-Befehl gesendet, um die Datenbank, die Leistung beeinträchtigt werden kann.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-410">Any grouping, ordering, and projection done outside of the repository will not be composed into the SQL command sent to the database, which can hurt performance.</span></span> <span data-ttu-id="2b0b0-411">Andererseits, ein Repository nur "IEnumerable" zurückgeben&lt;T&gt; Ergebnisse überraschen Sie nie mit einem neuen SQL‑Befehl.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-411">On the other hand, a repository returning only IEnumerable&lt;T&gt; results will never surprise you with a new SQL command.</span></span> <span data-ttu-id="2b0b0-412">Beide Ansätze funktioniert, und beide Ansätze testfähig bleiben.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-412">Both approaches will work, and both approaches remain testable.</span></span>

<span data-ttu-id="2b0b0-413">Es ist einfach, geben Sie eine einzelne Implementierung der IRepository&lt;T&gt; Schnittstelle mit Generika und der ObjectContext-API von EF4.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-413">It’s straightforward to provide a single implementation of the IRepository&lt;T&gt; interface using generics and the EF4 ObjectContext API.</span></span>

``` csharp
    public class SqlRepository<T> : IRepository<T>
                                    where T : class, IEntity {
        public SqlRepository(ObjectContext context) {
            _objectSet = context.CreateObjectSet<T>();
        }
        public IQueryable<T> FindAll() {
            return _objectSet;
        }
        public IQueryable<T> FindWhere(
                               Expression<Func\<T, bool>> predicate) {
            return _objectSet.Where(predicate);
        }
        public T FindById(int id) {
            return _objectSet.Single(o => o.Id == id);
        }
        public void Add(T newEntity) {
            _objectSet.AddObject(newEntity);
        }
        public void Remove(T entity) {
            _objectSet.DeleteObject(entity);
        }
        protected ObjectSet<T> _objectSet;
    }
```

<span data-ttu-id="2b0b0-414">Der IRepository&lt;T&gt; Ansatz haben wir einige zusätzliche Kontrolle darüber unserer Abfragen da sich ein Client zum Aufrufen einer Methode, um eine Entität zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-414">The IRepository&lt;T&gt; approach gives us some additional control over our queries because a client has to invoke a method to get to an entity.</span></span> <span data-ttu-id="2b0b0-415">Innerhalb der Methode können wir zusätzliche Prüfungen und LINQ-Operatoren zum Erzwingen von anwendungseinschränkungen bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-415">Inside the method we could provide additional checks and LINQ operators to enforce application constraints.</span></span> <span data-ttu-id="2b0b0-416">Beachten Sie, dass die Schnittstelle verfügt über zwei Einschränkungen in den generischen Typparameter.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-416">Notice the interface has two constraints on the generic type parameter.</span></span> <span data-ttu-id="2b0b0-417">Die erste Einschränkung ist die Klasse Nachteile Qualitätsmerkmale "ObjectSet" erforderlichen&lt;T&gt;, und die zweite Einschränkung erzwingt, dass die Entitäten IEntity – eine Abstraktion für die Anwendung erstellt zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-417">The first constraint is the class cons taint required by ObjectSet&lt;T&gt;, and the second constraint forces our entities to implement IEntity – an abstraction created for the application.</span></span> <span data-ttu-id="2b0b0-418">IEntity-Schnittstelle erzwingt, dass Entitäten, die eine lesbare Id-Eigenschaft aufweisen, und wir können Sie diese Eigenschaft in der FindById-Methode.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-418">The IEntity interface forces entities to have a readable Id property, and we can then use this property in the FindById method.</span></span> <span data-ttu-id="2b0b0-419">IEntity ist mit den folgenden Code definiert.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-419">IEntity is defined with the following code.</span></span>

``` csharp
    public interface IEntity {
        int Id { get; }
    }
```

<span data-ttu-id="2b0b0-420">IEntity kann die eine kleine Verletzung ignorieren der Persistenz angesehen werden, da die Entitäten erforderlich sind, diese Schnittstelle implementieren.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-420">IEntity could be considered a small violation of persistence ignorance since our entities are required to implement this interface.</span></span> <span data-ttu-id="2b0b0-421">Denken Sie daran ignorieren der Persistenz geht es um Kompromisse und für viele die FindById-Funktionalität überwiegen die Einschränkung seitens der Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-421">Remember persistence ignorance is about tradeoffs, and for many the FindById functionality will outweigh the constraint imposed by the interface.</span></span> <span data-ttu-id="2b0b0-422">Die Schnittstelle hat keine Auswirkungen auf die testbarkeit.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-422">The interface has no impact on testability.</span></span>

<span data-ttu-id="2b0b0-423">Instanziieren eine live IRepository&lt;T&gt; erfordert einen ObjectContext EF4 eine konkrete Einheit arbeiten-Implementierung die Instanziierung verwalten soll.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-423">Instantiating a live IRepository&lt;T&gt; requires an EF4 ObjectContext, so a concrete unit of work implementation should manage the instantiation.</span></span>

``` csharp
    public class SqlUnitOfWork : IUnitOfWork {
        public SqlUnitOfWork() {
            var connectionString =
                ConfigurationManager
                    .ConnectionStrings[ConnectionStringName]
                    .ConnectionString;

            _context = new ObjectContext(connectionString);
            _context.ContextOptions.LazyLoadingEnabled = true;
        }

        public IRepository<Employee> Employees {
            get {
                if (_employees == null) {
                    _employees = new SqlRepository<Employee>(_context);
                }
                return _employees;
            }
        }

        public IRepository<TimeCard> TimeCards {
            get {
                if (_timeCards == null) {
                    _timeCards = new SqlRepository<TimeCard>(_context);
                }
                return _timeCards;
            }
        }

        public void Commit() {
            _context.SaveChanges();
        }

        SqlRepository<Employee> _employees = null;
        SqlRepository<TimeCard> _timeCards = null;
        readonly ObjectContext _context;
        const string ConnectionStringName = "EmployeeDataModelContainer";
    }
```

### <a name="using-the-custom-repository"></a><span data-ttu-id="2b0b0-424">Verwenden das benutzerdefinierte Repository</span><span class="sxs-lookup"><span data-stu-id="2b0b0-424">Using the Custom Repository</span></span>

<span data-ttu-id="2b0b0-425">Mit unseren benutzerdefinierten Repositorys ist nicht grundlegend von mithilfe des Repositorys, die basierend auf IObjectSet&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-425">Using our custom repository is not significantly different from using the repository based on IObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="2b0b0-426">Anstelle von LINQ-Operatoren direkt auf eine Eigenschaft anwenden, wir müssen zunächst eine des Repositorys-Methoden zum Abrufen einer "IQueryable" aufrufen&lt;T&gt; Verweis.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-426">Instead of applying LINQ operators directly to a property, we’ll first need to invoke one the repository’s methods to grab an IQueryable&lt;T&gt; reference.</span></span>

``` csharp
    public ViewResult Index() {
        var model = _repository.FindAll()
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

<span data-ttu-id="2b0b0-427">Beachten Sie, dass die benutzerdefinierte Include-Operator, den wir zuvor implementiert ohne Änderung funktionieren.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-427">Notice the custom Include operator we implemented previously will work without change.</span></span> <span data-ttu-id="2b0b0-428">FindById-Methode des Repositorys entfernt doppelte Logik aus Aktionen, die versuchen, eine einzelne Entität abrufen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-428">The repository’s FindById method removes duplicated logic from actions trying to retrieve a single entity.</span></span>

``` csharp
    public ViewResult Details(int id) {
        var model = _repository.FindById(id);
        return View(model);
    }
```

<span data-ttu-id="2b0b0-429">Es ist kein wesentlicher Unterschied zwischen der testbarkeit der beiden Methoden, die wir untersucht haben.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-429">There is no significant difference in the testability of the two approaches we’ve examined.</span></span> <span data-ttu-id="2b0b0-430">Bieten wir falsche Implementierungen IRepository&lt;T&gt; erstellen konkrete Klassen, die durch HashSet gesichert&lt;Mitarbeiter&gt; – wie was wir im letzten Abschnitt hat.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-430">We could provide fake implementations of IRepository&lt;T&gt; by building concrete classes backed by HashSet&lt;Employee&gt; - just like what we did in the last section.</span></span> <span data-ttu-id="2b0b0-431">Einige Entwickler bevorzugen es jedoch verwenden Sie Mockobjekte und Modellieren von Objekt-Frameworks, anstatt zu Fakes zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-431">However, some developers prefer to use mock objects and mock object frameworks instead of building fakes.</span></span> <span data-ttu-id="2b0b0-432">Betrachten wir mit Mocks testen unsere Implementierung und die Unterschiede zwischen Mocks und Fakes im nächsten Abschnitt erläutert.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-432">We’ll look at using mocks to test our implementation and discuss the differences between mocks and fakes in the next section.</span></span>

### <a name="testing-with-mocks"></a><span data-ttu-id="2b0b0-433">Testen mit Mocks</span><span class="sxs-lookup"><span data-stu-id="2b0b0-433">Testing with Mocks</span></span>

<span data-ttu-id="2b0b0-434">Es gibt verschiedene Ansätze zum Erstellen von welche Aufrufe von Martin Fowler ein "Testdouble".</span><span class="sxs-lookup"><span data-stu-id="2b0b0-434">There are different approaches to building what Martin Fowler calls a “test double”.</span></span> <span data-ttu-id="2b0b0-435">Ein Testdouble (z. B. einen Film Testdouble) ist ein Objekt, das Sie erstellen, um "für real stehen" Produktions-Objekte während der Tests.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-435">A test double (like a movie stunt double) is an object you build to “stand in” for real, production objects during tests.</span></span> <span data-ttu-id="2b0b0-436">Die in-Memory-Repositorys, die wir erstellt haben, sind Testdoubles für die Repositorys, die mit SQL Server kommunizieren.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-436">The in-memory repositories we created are test doubles for the repositories that talk to SQL Server.</span></span> <span data-ttu-id="2b0b0-437">Wir haben gesehen, wie Sie verwenden diese Testdoubles während der Komponententests, Code zu isolieren, und halten die Tests, die schnell ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-437">We’ve seen how to use these test-doubles during the unit tests to isolate code and keep tests running fast.</span></span>

<span data-ttu-id="2b0b0-438">Der Test-Doubles an, die wir erstellt haben über real, funktionsfähiges Implementierungen verfügen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-438">The test doubles we’ve built have real, working implementations.</span></span> <span data-ttu-id="2b0b0-439">Hinter den Kulissen jeweils eine konkrete Auflistung von Objekten speichert, und sie hinzufügen und entfernen Sie Objekte aus dieser Auflistung, wie wir das Repository während eines Tests bearbeiten.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-439">Behind the scenes each one stores a concrete collection of objects, and they will add and remove objects from this collection as we manipulate the repository during a test.</span></span> <span data-ttu-id="2b0b0-440">Einige Entwickler möchten ihre Test-Doubles auf diese Weise – mit echten Code und arbeiten Implementierungen erstellen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-440">Some developers like to build their test doubles this way – with real code and working implementations.</span></span>  <span data-ttu-id="2b0b0-441">Diese Testdoubles werden nennen wir *fakes*.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-441">These test doubles are what we call *fakes*.</span></span> <span data-ttu-id="2b0b0-442">Weisen sie Arbeit Implementierungen, aber sie sind nicht ausreichend "realitätsnah" für die Produktion.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-442">They have working implementations, but they aren’t real enough for production use.</span></span> <span data-ttu-id="2b0b0-443">Das fake-Repository nicht tatsächlich in die Datenbank geschrieben.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-443">The fake repository doesn’t actually write to the database.</span></span> <span data-ttu-id="2b0b0-444">Die gefälschte SMTP-Server nicht tatsächlich eine e-Mail-Nachricht über das Netzwerk gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-444">The fake SMTP server doesn’t actually send an email message over the network.</span></span>

### <a name="mocks-versus-fakes"></a><span data-ttu-id="2b0b0-445">Mocks und Fakes</span><span class="sxs-lookup"><span data-stu-id="2b0b0-445">Mocks versus Fakes</span></span>

<span data-ttu-id="2b0b0-446">Es ist eine andere Art des Tests, die als double bekannt. eine *simulieren*.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-446">There is another type of test double known as a *mock*.</span></span> <span data-ttu-id="2b0b0-447">Während der Fakes arbeiten Implementierungen verfügen, enthalten Mocks keine Implementierung.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-447">While fakes have working implementations, mocks come with no implementation.</span></span> <span data-ttu-id="2b0b0-448">Mithilfe eines pseudoobjektframeworks, wenn wir diese simulierte Objekte zur Laufzeit erstellen und als Test-Doubles verwenden können.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-448">With the help of a mock object framework we construct these mock objects at run time and use them as test doubles.</span></span> <span data-ttu-id="2b0b0-449">In diesem Abschnitt wird die open Source simulationsframework Moq verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-449">In this section we’ll be using the open source mocking framework Moq.</span></span> <span data-ttu-id="2b0b0-450">Hier ist ein einfaches Beispiel mit Moq dynamisch einen Test für ein Repository für die Mitarbeiter doppelte erstellen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-450">Here is a simple example of using Moq to dynamically create a test double for an employee repository.</span></span>

``` csharp
    Mock<IRepository<Employee>> mock =
        new Mock<IRepository<Employee>>();
    IRepository<Employee> repository = mock.Object;
    repository.Add(new Employee());
    var employee = repository.FindById(1);
```

<span data-ttu-id="2b0b0-451">Wir bitten Moq IRepository&lt;Mitarbeiter&gt; eine Implementierung und dynamisch erstellt.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-451">We ask Moq for an IRepository&lt;Employee&gt; implementation and it builds one dynamically.</span></span> <span data-ttu-id="2b0b0-452">Wir können auf das Objekt, das Implementieren von IRepository&lt;Mitarbeiter&gt; durch den Zugriff auf die Objekteigenschaft des Mock&lt;T&gt; Objekt.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-452">We can get to the object implementing IRepository&lt;Employee&gt; by accessing the Object property of the Mock&lt;T&gt; object.</span></span> <span data-ttu-id="2b0b0-453">Es ist dieses interne Objekt, die wir in unserem Controller übergeben können, und sie wissen nicht, ist dies ein Testdouble oder die echten.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-453">It is this inner object we can pass into our controllers, and they won’t know if this is a test double or the real repository.</span></span> <span data-ttu-id="2b0b0-454">Wir können die Methoden für das Objekt aufrufen, wie wir Aufrufen von Methoden auf ein Objekt mit einer echten Implementierung würde.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-454">We can invoke methods on the object just like we would invoke methods on an object with a real implementation.</span></span>

<span data-ttu-id="2b0b0-455">Sie müssen wissen, welche Aktionen das pseudorepository ausgeführt werden, wenn wir die Add-Methode aufrufen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-455">You must be wondering what the mock repository will do when we invoke the Add method.</span></span> <span data-ttu-id="2b0b0-456">Da keine Implementierung hinter das mock-Objekt ist, führt keine Aktion hinzufügen aus.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-456">Since there is no implementation behind the mock object, Add does nothing.</span></span> <span data-ttu-id="2b0b0-457">Es gibt keine konkreten Sammlung hinter den Kulissen wie musste mit den Fakes, die wir geschrieben haben, die damit der Mitarbeiter verworfen wird.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-457">There is no concrete collection behind the scenes like we had with the fakes we wrote, so the employee is discarded.</span></span> <span data-ttu-id="2b0b0-458">Was ist der Rückgabewert von FindById?</span><span class="sxs-lookup"><span data-stu-id="2b0b0-458">What about the return value of FindById?</span></span> <span data-ttu-id="2b0b0-459">In diesem Fall ist das mock-Objekt dem einzigen, was, das Sie tun kann, das zurückgegeben wird einen Standardwert.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-459">In this case the mock object does the only thing it can do, which is return a default value.</span></span> <span data-ttu-id="2b0b0-460">Da wir einen Verweistyp handelt (ein Mitarbeiter) zurückgeben, ist der Rückgabewert ein null-Wert.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-460">Since we are returning a reference type (an Employee), the return value is a null value.</span></span>

<span data-ttu-id="2b0b0-461">Mocks mag nutzlos; Es gibt jedoch zwei weitere Funktionen Mocks, die wir gesprochen noch nicht.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-461">Mocks might sound worthless; however, there are two more features of mocks we haven’t talked about.</span></span> <span data-ttu-id="2b0b0-462">Das Framework Moq zeichnet zuerst alle Aufrufe für das mock-Objekt.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-462">First, the Moq framework records all the calls made on the mock object.</span></span> <span data-ttu-id="2b0b0-463">Später im Code können wir Moq bitten, wenn jemand die Add-Methode aufgerufen, oder wenn jemand die FindById-Methode aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-463">Later in the code we can ask Moq if anyone invoked the Add method, or if anyone invoked the FindById method.</span></span> <span data-ttu-id="2b0b0-464">Wir sehen die erläutert, wie wir dieses aufzeichnungsfeature "Blackbox" in Tests verwenden können.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-464">We’ll see later how we can use this “black box” recording feature in tests.</span></span>

<span data-ttu-id="2b0b0-465">Die zweite großartiges Feature ist, wie wir Moq verwenden können, ein mock-Objekt mit Programmieren *Erwartungen*.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-465">The second great feature is how we can use Moq to program a mock object with *expectations*.</span></span> <span data-ttu-id="2b0b0-466">Eine Erwartung weist dem mock-Objekt zum Reagieren auf bestimmte eingreifen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-466">An expectation tells the mock object how to respond to any given interaction.</span></span> <span data-ttu-id="2b0b0-467">Beispielsweise können wir Programmieren in unserem Mock eine Erwartung und anweisen, ein Employee-Objekt zurück, wenn jemand FindById aufruft.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-467">For example, we can program an expectation into our mock and tell it to return an employee object when someone invokes FindById.</span></span> <span data-ttu-id="2b0b0-468">Das Moq-Framework verwendet ein Setup-API und Lambda-Ausdrücke, diese Erwartungen zu programmieren.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-468">The Moq framework uses a Setup API and lambda expressions to program these expectations.</span></span>

``` csharp
    [TestMethod]
    public void MockSample() {
        Mock<IRepository<Employee>> mock =
            new Mock<IRepository<Employee>>();
        mock.Setup(m => m.FindById(5))
            .Returns(new Employee {Id = 5});
        IRepository<Employee> repository = mock.Object;
        var employee = repository.FindById(5);
        Assert.IsTrue(employee.Id == 5);
    }
```

<span data-ttu-id="2b0b0-469">In diesem Beispiel bitten wir Moq, um dynamisch ein Repository zu erstellen, und klicken Sie dann das Repository mit der Erwartung programmieren.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-469">In this sample we ask Moq to dynamically build a repository, and then we program the repository with an expectation.</span></span> <span data-ttu-id="2b0b0-470">Der Annahme weist das mock-Objekt, ein neues Employee-Objekt mit Id-Wert 5 zurück, wenn jemand die übergeben des Werts 5 FindById-Methode aufruft.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-470">The expectation tells the mock object to return a new employee object with an Id value of 5 when someone invokes the FindById method passing a value of 5.</span></span> <span data-ttu-id="2b0b0-471">Dieser Test erfolgreich verläuft, und wir mussten erstellen Sie eine vollständige Implementierung, um gefälschte IRepository&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-471">This test passes, and we didn’t need to build a full implementation to fake IRepository&lt;T&gt;.</span></span>

<span data-ttu-id="2b0b0-472">Wir rufen Sie erneut auf die Tests, die wir zuvor geschrieben hat, und überarbeiten, um anstelle von Fakes Mocks zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-472">Let’s revisit the tests we wrote earlier and rework them to use mocks instead of fakes.</span></span> <span data-ttu-id="2b0b0-473">Genau wie verwenden zuvor eine Basisklasse wir die allgemeinen Teile der Infrastruktur einrichten, die für alle des Controllers Tests erforderlich.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-473">Just like before, we’ll use a base class to setup the common pieces of infrastructure we need for all of the controller’s tests.</span></span>

``` csharp
    public class EmployeeControllerTestBase {
        public EmployeeControllerTestBase() {
            _employeeData = EmployeeObjectMother.CreateEmployees()
                                                .AsQueryable();
            _repository = new Mock<IRepository<Employee>>();
            _unitOfWork = new Mock<IUnitOfWork>();
            _unitOfWork.Setup(u => u.Employees)
                       .Returns(_repository.Object);
            _controller = new EmployeeController(_unitOfWork.Object);
        }

        protected IQueryable<Employee> _employeeData;
        protected Mock<IUnitOfWork> _unitOfWork;
        protected EmployeeController _controller;
        protected Mock<IRepository<Employee>> _repository;
    }
```

<span data-ttu-id="2b0b0-474">Durch den Setupcode bleibt größtenteils gleich.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-474">The setup code remains mostly the same.</span></span> <span data-ttu-id="2b0b0-475">Anstatt Sie Fakes verwenden, verwenden wir Moq mock-Objekte erstellen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-475">Instead of using fakes, we’ll use Moq to construct mock objects.</span></span> <span data-ttu-id="2b0b0-476">Die Basisklasse ordnet für das mock Arbeitseinheit, die ein pseudorepository zurückgegeben werden soll, wenn der Code wird aufgerufen, die Mitarbeiter-Eigenschaft an.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-476">The base class arranges for the mock unit of work to return a mock repository when code invokes the Employees property.</span></span> <span data-ttu-id="2b0b0-477">Die restlichen das mock-Setup wird in der prüfvorrichtungen, die speziell für jeden bestimmten Szenario stattfinden.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-477">The rest of the mock setup will take place inside the test fixtures dedicated to each specific scenario.</span></span> <span data-ttu-id="2b0b0-478">Die Testfixture für die Index-Aktion wird z. B. das pseudorepository, um eine Liste der Mitarbeiter zurückzugeben, wenn die Aktion die FindAll-Methode, der das pseudorepository aufruft eingerichtet.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-478">For example, the test fixture for the Index action will setup the mock repository to return a list of employees when the action invokes the FindAll method of the mock repository.</span></span>

``` csharp
    [TestClass]
    public class EmployeeControllerIndexActionTests
               : EmployeeControllerTestBase {
        public EmployeeControllerIndexActionTests() {
            _repository.Setup(r => r.FindAll())
                        .Returns(_employeeData);
        }
        // .. tests
        [TestMethod]
        public void ShouldBuildModelWithAllEmployees() {
            var result = _controller.Index();
            var model = result.ViewData.Model
                          as IEnumerable<Employee>;
            Assert.IsTrue(model.Count() == _employeeData.Count());
        }
        // .. and more tests
    }
```

<span data-ttu-id="2b0b0-479">Außer den Erwartungen ähneln sich unsere Tests für die Tests, die, denen wir vorher hatten.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-479">Except for the expectations, our tests look similar to the tests we had before.</span></span> <span data-ttu-id="2b0b0-480">Allerdings können die Aufzeichnung Möglichkeit ein mock Framework wir Herangehensweisen zum Testen aus einem anderen Winkel.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-480">However, with the recording ability of a mock framework we can approach testing from a different angle.</span></span> <span data-ttu-id="2b0b0-481">Wir werden im nächsten Abschnitt dieses neue Perspektive betrachten.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-481">We’ll look at this new perspective in the next section.</span></span>

### <a name="state-versus-interaction-testing"></a><span data-ttu-id="2b0b0-482">Status im Vergleich zur Interaktion testen</span><span class="sxs-lookup"><span data-stu-id="2b0b0-482">State versus Interaction Testing</span></span>

<span data-ttu-id="2b0b0-483">Es gibt verschiedene Techniken, die Sie zum Testen von Software mit Pseudoobjekten verwenden können.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-483">There are different techniques you can use to test software with mock objects.</span></span> <span data-ttu-id="2b0b0-484">Ein Ansatz basiert auf Zustands testen, d.h., was wir in diesem Artikel bisher getan haben.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-484">One approach is to use state based testing, which is what we have done in this paper so far.</span></span> <span data-ttu-id="2b0b0-485">Zustandsbasierte Test macht Assertionen über den Status der Software.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-485">State based testing makes assertions about the state of the software.</span></span> <span data-ttu-id="2b0b0-486">In den letzten Test haben wir eine Aktionsmethode im Controller aufgerufen und versucht eine Assertion zum Modell, dass er erstellen soll.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-486">In the last test we invoked an action method on the controller and made an assertion about the model it should build.</span></span> <span data-ttu-id="2b0b0-487">Hier sind einige weitere Beispiele für Testzustands:</span><span class="sxs-lookup"><span data-stu-id="2b0b0-487">Here are some other examples of testing state:</span></span>

-   <span data-ttu-id="2b0b0-488">Stellen Sie sicher, dass das Repository das neue Mitarbeiterobjekt enthält, nach dem Erstellen ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-488">Verify the repository contains the new employee object after Create executes.</span></span>
-   <span data-ttu-id="2b0b0-489">Stellen Sie sicher, dass das Modell enthält eine Liste aller Mitarbeiter nach Index-Anweisung ausführt.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-489">Verify the model holds a list of all employees after Index executes.</span></span>
-   <span data-ttu-id="2b0b0-490">Stellen Sie sicher, dass das Repository keinen Mitarbeiter enthält, nach dem Löschen wird ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-490">Verify the repository does not contain a given employee after Delete executes.</span></span>

<span data-ttu-id="2b0b0-491">Eine weitere Möglichkeit, die Ihnen mit Pseudoobjekten angezeigt wird, um zu überprüfen *Interaktionen*.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-491">Another approach you’ll see with mock objects is to verify *interactions*.</span></span> <span data-ttu-id="2b0b0-492">Während der Zustandsbasierte testen macht Assertionen zu den Zustand von Objekten basierend Interaktion Test macht Assertionen über die Interaktion von Objekten an.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-492">While state based testing makes assertions about the state of objects, interaction based testing makes assertions about how objects interact.</span></span> <span data-ttu-id="2b0b0-493">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="2b0b0-493">For example:</span></span>

-   <span data-ttu-id="2b0b0-494">Stellen Sie sicher, dass der Controller des Repository-Add-Methode aufruft, bei der Ausführung erstellen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-494">Verify the controller invokes the repository’s Add method when Create executes.</span></span>
-   <span data-ttu-id="2b0b0-495">Stellen Sie sicher, dass der Controller des Repositorys FindAll-Methode ruft, wenn der Index-Anweisung ausführt.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-495">Verify the controller invokes the repository’s FindAll method when Index executes.</span></span>
-   <span data-ttu-id="2b0b0-496">Stellen Sie sicher, dass der Controller Ruft die Einheit der Arbeit des Commit-Methode, um Änderungen zu speichern, wenn Bearbeiten ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-496">Verify the controller invokes the unit of work’s Commit method to save changes when Edit executes.</span></span>

<span data-ttu-id="2b0b0-497">Testen der Interaktion erfordert häufig weniger Testdaten, da wir nicht innerhalb von Sammlungen Herumstochern und überprüfen die Anzahl.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-497">Interaction testing often requires less test data, because we aren’t poking inside of collections and verifying counts.</span></span> <span data-ttu-id="2b0b0-498">Z. B. wenn wir wissen, dass die Aktion für die Details des Repositorys eine FindById-Methode mit dem richtigen Wert - ruft ist dann die Aktion wahrscheinlich ordnungsgemäß verhält.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-498">For example, if we know the Details action invokes a repository’s FindById method with the correct value - then the action is probably behaving correctly.</span></span> <span data-ttu-id="2b0b0-499">Wir können dies überprüfen, ohne alle Testdaten aus FindById zurückgegeben einrichten zu müssen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-499">We can verify this behavior without setting up any test data to return from FindById.</span></span>

``` csharp
    [TestClass]
    public class EmployeeControllerDetailsActionTests
               : EmployeeControllerTestBase {
         // ...
        [TestMethod]
        public void ShouldInvokeRepositoryToFindEmployee() {
            var result = _controller.Details(_detailsId);
            _repository.Verify(r => r.FindById(_detailsId));
        }
        int _detailsId = 1;
    }
```

<span data-ttu-id="2b0b0-500">Das einzige Setup in der oben genannten Testfixture ist das Setup von der Basisklasse bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-500">The only setup required in the above test fixture is the setup provided by the base class.</span></span> <span data-ttu-id="2b0b0-501">Wenn wir die Controlleraktion aufrufen, wird die Moq die Interaktionen mit das pseudorepository aufgezeichnet.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-501">When we invoke the controller action, Moq will record the interactions with the mock repository.</span></span> <span data-ttu-id="2b0b0-502">Verwenden den überprüfen-API von Moq, können wir Moq bitten, wenn der Controller FindById mit der richtigen ID-Wert aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-502">Using the Verify API of Moq, we can ask Moq if the controller invoked FindById with the proper ID value.</span></span> <span data-ttu-id="2b0b0-503">Wenn der Controller nicht die Methode aufgerufen oder die Methode mit einem Wert unerwarteter Parameter aufgerufen, die Verify-Methode löst eine Ausnahme aus, und der Test schlägt fehl.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-503">If the controller did not invoke the method, or invoked the method with an unexpected parameter value, the Verify method will throw an exception and the test will fail.</span></span>

<span data-ttu-id="2b0b0-504">Hier ist ein weiteres Beispiel, um sicherzustellen, dass die Aktion "Create" Commit für die aktuelle Arbeitseinheit aufruft.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-504">Here is another example to verify the Create action invokes Commit on the current unit of work.</span></span>

``` csharp
    [TestMethod]
    public void ShouldCommitUnitOfWork() {
        _controller.Create(_newEmployee);
        _unitOfWork.Verify(u => u.Commit());
    }
```

<span data-ttu-id="2b0b0-505">Eine Gefahr besteht, mit dem Testen der Interaktion ist die Tendenz, über die Interaktionen bestimmen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-505">One danger with interaction testing is the tendency to over specify interactions.</span></span> <span data-ttu-id="2b0b0-506">Die Fähigkeit zum Aufzeichnen und überprüfen, ob es sich bei jeder Interaktion mit dem Mockobjekt bedeutet, dass den Test nicht das mock-Objekt sollten versuchen, um zu überprüfen, ob jede Interaktion.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-506">The ability of the mock object to record and verify every interaction with the mock object doesn’t mean the test should try to verify every interaction.</span></span> <span data-ttu-id="2b0b0-507">Einige Aktivitäten sind Implementierungsdetails und sollten Sie nur überprüfen Sie, ob die Interaktionen *erforderlichen* um den aktuellen Test zu erfüllen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-507">Some interactions are implementation details and you should only verify the interactions *required* to satisfy the current test.</span></span>

<span data-ttu-id="2b0b0-508">Die Wahl zwischen Mocks oder Fakes hängt zum größten Teil des Systems, die Sie testen und Ihren persönlichen (oder Team) Einstellungen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-508">The choice between mocks or fakes largely depends on the system you are testing and your personal (or team) preferences.</span></span> <span data-ttu-id="2b0b0-509">Pseudoobjekten können die Menge des Codes, die Sie Test-Doubles implementieren müssen, erheblich senken, aber nicht jeder Programmierung Erwartungen, und überprüfen die Interaktionen vertraut ist.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-509">Mock objects can drastically reduce the amount of code you need to implement test doubles, but not everyone is comfortable programming expectations and verifying interactions.</span></span>

## <a name="conclusions"></a><span data-ttu-id="2b0b0-510">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="2b0b0-510">Conclusions</span></span>

<span data-ttu-id="2b0b0-511">In diesem Artikel haben wir verschiedene Ansätze zum Erstellen von testbarem Code bei der Verwendung von ADO.NET Entity Framework für Dauerhaftigkeit von Daten veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-511">In this paper we’ve demonstrated several approaches to creating testable code while using the ADO.NET Entity Framework for data persistence.</span></span> <span data-ttu-id="2b0b0-512">Wir können die integrierte Abstraktionen wie IObjectSet nutzen&lt;T&gt;, oder erstellen Sie eigene Abstraktionen wie IRepository&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-512">We can leverage built in abstractions like IObjectSet&lt;T&gt;, or create our own abstractions like IRepository&lt;T&gt;.</span></span>  <span data-ttu-id="2b0b0-513">In beiden Fällen kann die POCO-Unterstützung in ADO.NET Entity Framework 4.0 der Consumer diese Abstraktionen persistent bleiben, ignorieren und leicht zu testendes.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-513">In both cases, the POCO support in the ADO.NET Entity Framework 4.0 allows the consumers of these abstractions to remain persistent ignorant and highly testable.</span></span> <span data-ttu-id="2b0b0-514">Zusätzliche EF4-Features wie Lazy Load implizite Geschäfts- und -Dienst ermöglicht es code arbeiten, ohne sich Gedanken über die Details eines relationalen Datenspeichers machen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-514">Additional EF4 features like implicit lazy loading allows business and application service code to work without worrying about the details of a relational data store.</span></span> <span data-ttu-id="2b0b0-515">Abschließend die von uns erstellte Abstraktionen sind einfach zu simulieren oder imitieren in Komponententests, und wir können diese Testdoubles schnell ausgeführt wird, hoch isoliert, erreichen und zuverlässige Tests verwenden.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-515">Finally, the abstractions we create are easy to mock or fake inside of unit tests, and we can use these test doubles to achieve fast running, highly isolated, and reliable tests.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="2b0b0-516">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="2b0b0-516">Additional Resources</span></span>

-   <span data-ttu-id="2b0b0-517">Martin, " [Prinzip der einzigen Verantwortung](http://www.objectmentor.com/resources/articles/srp.pdf)"</span><span class="sxs-lookup"><span data-stu-id="2b0b0-517">Robert C. Martin, “ [The Single Responsibility Principle](http://www.objectmentor.com/resources/articles/srp.pdf)”</span></span>
-   <span data-ttu-id="2b0b0-518">Martin Fowler [Musterkatalog](http://www.martinfowler.com/eaaCatalog/index.html) aus *Muster der Unternehmensanwendungsarchitektur*</span><span class="sxs-lookup"><span data-stu-id="2b0b0-518">Martin Fowler, [Catalog of Patterns](http://www.martinfowler.com/eaaCatalog/index.html) from *Patterns of Enterprise Application Architecture*</span></span>
-   <span data-ttu-id="2b0b0-519">Griffin Caprio, " [Abhängigkeitsinjektion](https://msdn.microsoft.com/magazine/cc163739.aspx)"</span><span class="sxs-lookup"><span data-stu-id="2b0b0-519">Griffin Caprio, “ [Dependency Injection](https://msdn.microsoft.com/magazine/cc163739.aspx)”</span></span>
-   <span data-ttu-id="2b0b0-520">Data Programmability-Blog " [Exemplarische Vorgehensweise: testgesteuerte Entwicklung mit Entitätsframework 4.0](http://blogs.msdn.com/adonet/pages/walkthrough-test-driven-development-with-the-entity-framework-4-0.aspx)".</span><span class="sxs-lookup"><span data-stu-id="2b0b0-520">Data Programmability Blog, “ [Walkthrough: Test Driven Development with the Entity Framework 4.0](http://blogs.msdn.com/adonet/pages/walkthrough-test-driven-development-with-the-entity-framework-4-0.aspx)”.</span></span>
-   <span data-ttu-id="2b0b0-521">Data Programmability-Blog " [mithilfe von Repository und Arbeitseinheit Muster mit Entity Framework 4.0](http://blogs.msdn.com/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)"</span><span class="sxs-lookup"><span data-stu-id="2b0b0-521">Data Programmability Blog, “ [Using Repository and Unit of Work patterns with Entity Framework 4.0](http://blogs.msdn.com/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)”</span></span>
-   <span data-ttu-id="2b0b0-522">Dave Astels, " [BDD Einführung](http://blog.daveastels.com/files/BDD_Intro.pdf)"</span><span class="sxs-lookup"><span data-stu-id="2b0b0-522">Dave Astels, “ [BDD Intro](http://blog.daveastels.com/files/BDD_Intro.pdf)”</span></span>
-   <span data-ttu-id="2b0b0-523">Aaron Jensen, " [Einführung in die Spezifikationen des Computers](http://codebetter.com/blogs/aaron.jensen/archive/2008/05/08/introducing-machine-specifications-or-mspec-for-short.aspx)"</span><span class="sxs-lookup"><span data-stu-id="2b0b0-523">Aaron Jensen, “ [Introducing Machine Specifications](http://codebetter.com/blogs/aaron.jensen/archive/2008/05/08/introducing-machine-specifications-or-mspec-for-short.aspx)”</span></span>
-   <span data-ttu-id="2b0b0-524">Eric Lee, " [BDD mit MSTest](http://blogs.msdn.com/elee/archive/2009/01/20/bdd-with-mstest.aspx)"</span><span class="sxs-lookup"><span data-stu-id="2b0b0-524">Eric Lee, “ [BDD with MSTest](http://blogs.msdn.com/elee/archive/2009/01/20/bdd-with-mstest.aspx)”</span></span>
-   <span data-ttu-id="2b0b0-525">Eric Evans " [Domain-Driven Design](http://books.google.com/books?id=7dlaMs0SECsC&printsec=frontcover&dq=evans%20domain%20driven%20design&hl=en&ei=cHztS6C8KIaglAfA_dS1CA&sa=X&oi=book_result&ct=result&resnum=1&ved=0CCoQ6AEwAA)"</span><span class="sxs-lookup"><span data-stu-id="2b0b0-525">Eric Evans, “ [Domain Driven Design](http://books.google.com/books?id=7dlaMs0SECsC&printsec=frontcover&dq=evans%20domain%20driven%20design&hl=en&ei=cHztS6C8KIaglAfA_dS1CA&sa=X&oi=book_result&ct=result&resnum=1&ved=0CCoQ6AEwAA)”</span></span>
-   <span data-ttu-id="2b0b0-526">Martin Fowler " [Pseudoobjekte sind keine Stubs](http://martinfowler.com/articles/mocksArentStubs.html)"</span><span class="sxs-lookup"><span data-stu-id="2b0b0-526">Martin Fowler, “ [Mocks Aren’t Stubs](http://martinfowler.com/articles/mocksArentStubs.html)”</span></span>
-   <span data-ttu-id="2b0b0-527">Martin Fowler " [testen Double](http://martinfowler.com/bliki/TestDouble.html)"</span><span class="sxs-lookup"><span data-stu-id="2b0b0-527">Martin Fowler, “ [Test Double](http://martinfowler.com/bliki/TestDouble.html)”</span></span>
-   <span data-ttu-id="2b0b0-528">Jeremy Miller " [Status im Vergleich zur Interaktion testen](http://codebetter.com/blogs/jeremy.miller/articles/129544.aspx)"</span><span class="sxs-lookup"><span data-stu-id="2b0b0-528">Jeremy Miller, “ [State versus Interaction Testing](http://codebetter.com/blogs/jeremy.miller/articles/129544.aspx)”</span></span>
-   <span data-ttu-id="2b0b0-529">Moq, \<http://code.google.com/p/moq/></span><span class="sxs-lookup"><span data-stu-id="2b0b0-529">Moq, \<http://code.google.com/p/moq/></span></span>

### <a name="biography"></a><span data-ttu-id="2b0b0-530">Biografie</span><span class="sxs-lookup"><span data-stu-id="2b0b0-530">Biography</span></span>

<span data-ttu-id="2b0b0-531">Scott Allen ist ein Mitglied des technischen Personals bei Pluralsight und Gründer von OdeToCode.com.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-531">Scott Allen is a member of the technical staff at Pluralsight and the founder of OdeToCode.com.</span></span> <span data-ttu-id="2b0b0-532">In den 15 Jahren der Entwicklung von kommerziellen Software arbeitete Scott an Lösungen für alle Elemente aus 8-Bit-embedded-Geräte in hochgradig skalierbaren ASP.NET-Webanwendungen.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-532">In 15 years of commercial software development, Scott has worked on solutions for everything from 8-bit embedded devices to highly scalable ASP.NET web applications.</span></span> <span data-ttu-id="2b0b0-533">Sie können Scott auf Twitter unter oder über seinen Blog unter OdeToCode erreichen \< http://twitter.com/OdeToCode>.</span><span class="sxs-lookup"><span data-stu-id="2b0b0-533">You can reach Scott on his blog at OdeToCode, or on Twitter at \<http://twitter.com/OdeToCode>.</span></span>
