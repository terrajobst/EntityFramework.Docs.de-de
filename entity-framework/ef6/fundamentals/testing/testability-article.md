---
title: Testability und Entity Framework 4,0-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9430e2ab-261c-4e8e-8545-2ebc52d7a247
ms.openlocfilehash: 28ec5446ce9faf98fb8fff141832236d70b29daf
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181580"
---
# <a name="testability-and-entity-framework-40"></a><span data-ttu-id="b2c70-102">Testability und Entity Framework 4,0</span><span class="sxs-lookup"><span data-stu-id="b2c70-102">Testability and Entity Framework 4.0</span></span>
<span data-ttu-id="b2c70-103">Scott allen</span><span class="sxs-lookup"><span data-stu-id="b2c70-103">Scott Allen</span></span>

<span data-ttu-id="b2c70-104">Veröffentlicht: Mai 2010</span><span class="sxs-lookup"><span data-stu-id="b2c70-104">Published: May 2010</span></span>

## <a name="introduction"></a><span data-ttu-id="b2c70-105">Einführung</span><span class="sxs-lookup"><span data-stu-id="b2c70-105">Introduction</span></span>

<span data-ttu-id="b2c70-106">In diesem Whitepaper wird beschrieben und veranschaulicht, wie Sie testbaren Code mit den ADO.NET-Entity Framework 4,0 und Visual Studio 2010 schreiben können.</span><span class="sxs-lookup"><span data-stu-id="b2c70-106">This white paper describes and demonstrates how to write testable code with the ADO.NET Entity Framework 4.0 and Visual Studio 2010.</span></span> <span data-ttu-id="b2c70-107">In diesem Whitepaper wird nicht versucht, sich auf eine bestimmte Testmethode zu konzentrieren, wie z. b. Test gesteuerte Entwürfe (Test-gesteuerte Design) oder BDD.</span><span class="sxs-lookup"><span data-stu-id="b2c70-107">This paper does not try to focus on a specific testing methodology, like test-driven design (TDD) or behavior-driven design (BDD).</span></span> <span data-ttu-id="b2c70-108">Stattdessen konzentriert sich dieses Whitepaper darauf, wie Sie Code schreiben können, der den ADO.NET-Entity Framework verwendet, um die Isolierung und den Test auf automatisierte Weise zu testen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-108">Instead this paper will focus on how to write code that uses the ADO.NET Entity Framework yet remains easy to isolate and test in an automated fashion.</span></span> <span data-ttu-id="b2c70-109">Wir betrachten allgemeine Entwurfsmuster, die das Testen in Datenzugriffs Szenarien vereinfachen und sehen, wie diese Muster bei der Verwendung des Frameworks angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="b2c70-109">We’ll look at common design patterns that facilitate testing in data access scenarios and see how to apply those patterns when using the framework.</span></span> <span data-ttu-id="b2c70-110">Wir betrachten auch bestimmte Features des Frameworks, um zu sehen, wie diese Features in testbarem Code funktionieren können.</span><span class="sxs-lookup"><span data-stu-id="b2c70-110">We’ll also look at specific features of the framework to see how those features can work in testable code.</span></span>

## <a name="what-is-testable-code"></a><span data-ttu-id="b2c70-111">Was ist prüfbarer Code?</span><span class="sxs-lookup"><span data-stu-id="b2c70-111">What Is Testable Code?</span></span>

<span data-ttu-id="b2c70-112">Die Möglichkeit, eine Software Software mithilfe automatisierter Komponententests zu überprüfen, bietet zahlreiche Vorteile.</span><span class="sxs-lookup"><span data-stu-id="b2c70-112">The ability to verify a piece of software using automated unit tests offers many desirable benefits.</span></span> <span data-ttu-id="b2c70-113">Jeder weiß, dass gute Tests die Anzahl der Software Mängel in einer Anwendung reduzieren und die Qualität der Anwendung erhöhen, aber das vorhanden sein von Komponententests geht weit über das Auffinden von Fehlern hinaus.</span><span class="sxs-lookup"><span data-stu-id="b2c70-113">Everyone knows that good tests will reduce the number of software defects in an application and increase the application’s quality - but having unit tests in place goes far beyond just finding bugs.</span></span>

<span data-ttu-id="b2c70-114">Eine gute Komponenten Test Suite ermöglicht einem Entwicklungsteam, Zeit zu sparen, und die Kontrolle über die von Ihnen erstellte Software zu behalten.</span><span class="sxs-lookup"><span data-stu-id="b2c70-114">A good unit test suite allows a development team to save time and remain in control of the software they create.</span></span> <span data-ttu-id="b2c70-115">Ein Team kann Änderungen an vorhandenem Code vornehmen, die Software umgestalten, umgestalten und umstrukturieren, um neue Anforderungen zu erfüllen, und neue Komponenten zu einer Anwendung hinzufügen, während Sie wissen, dass die Test Sammlung das Verhalten der Anwendung überprüfen kann.</span><span class="sxs-lookup"><span data-stu-id="b2c70-115">A team can make changes to existing code, refactor, redesign, and restructure software to meet new requirements, and add new components into an application all while knowing the test suite can verify the application’s behavior.</span></span> <span data-ttu-id="b2c70-116">Komponententests sind Teil eines kurzen Feedbacks, um Änderungen zu vereinfachen und die Verwaltbarkeit von Software zu gewährleisten, wenn sich die Komplexität erhöht.</span><span class="sxs-lookup"><span data-stu-id="b2c70-116">Unit tests are part of a quick feedback cycle to facilitate change and preserve the maintainability of software as complexity increases.</span></span>

<span data-ttu-id="b2c70-117">Komponententests verfügen jedoch über einen Preis.</span><span class="sxs-lookup"><span data-stu-id="b2c70-117">Unit testing comes with a price, however.</span></span> <span data-ttu-id="b2c70-118">Ein Team muss die Zeit investieren, um Komponententests zu erstellen und zu verwalten.</span><span class="sxs-lookup"><span data-stu-id="b2c70-118">A team has to invest the time to create and maintain unit tests.</span></span> <span data-ttu-id="b2c70-119">Der Aufwand, der zum Erstellen dieser Tests erforderlich ist, hängt direkt mit der **Prüfbarkeit** der zugrunde liegenden Software zusammen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-119">The amount of effort required to create these tests is directly related to the **testability** of the underlying software.</span></span> <span data-ttu-id="b2c70-120">Wie einfach ist die Software zu testen?</span><span class="sxs-lookup"><span data-stu-id="b2c70-120">How easy is the software to test?</span></span> <span data-ttu-id="b2c70-121">Ein Team, das Software mit Prüfbarkeit im Hinterkopf entwirft, erstellt effektive Tests schneller als das Team, das mit der nicht Test fähigen Software arbeitet.</span><span class="sxs-lookup"><span data-stu-id="b2c70-121">A team designing software with testability in mind will create effective tests faster than the team working with un-testable software.</span></span>

<span data-ttu-id="b2c70-122">Microsoft hat den ADO.NET Entity Framework 4,0 (EF4) mit Prüfbarkeit entworfen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-122">Microsoft designed the ADO.NET Entity Framework 4.0 (EF4) with testability in mind.</span></span> <span data-ttu-id="b2c70-123">Dies bedeutet nicht, dass Entwickler Komponententests für den Frameworkcode selbst schreiben werden.</span><span class="sxs-lookup"><span data-stu-id="b2c70-123">This doesn’t mean developers will be writing unit tests against framework code itself.</span></span> <span data-ttu-id="b2c70-124">Stattdessen erleichtern die Testability-Ziele für EF4 das Erstellen von prüfbarem Code, der auf dem Framework aufbaut.</span><span class="sxs-lookup"><span data-stu-id="b2c70-124">Instead, the testability goals for EF4 make it easy to create testable code that builds on top of the framework.</span></span> <span data-ttu-id="b2c70-125">Bevor wir uns mit bestimmten Beispielen beschäftigen, ist es sinnvoll, die Qualität des testbaren Codes zu verstehen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-125">Before we look at specific examples, it’s worthwhile to understand the qualities of testable code.</span></span>

### <a name="the-qualities-of-testable-code"></a><span data-ttu-id="b2c70-126">Die Qualitäten von testablem Code</span><span class="sxs-lookup"><span data-stu-id="b2c70-126">The Qualities of Testable Code</span></span>

<span data-ttu-id="b2c70-127">Code, der leicht zu testen ist, enthält immer mindestens zwei Merkmale.</span><span class="sxs-lookup"><span data-stu-id="b2c70-127">Code that is easy to test will always exhibit at least two traits.</span></span> <span data-ttu-id="b2c70-128">Zuerst ist testbarer Code leicht zu **beobachten**.</span><span class="sxs-lookup"><span data-stu-id="b2c70-128">First, testable code is easy to **observe**.</span></span> <span data-ttu-id="b2c70-129">Bei einem bestimmten Satz von Eingaben sollte die Ausgabe des Codes leicht zu beobachten sein.</span><span class="sxs-lookup"><span data-stu-id="b2c70-129">Given some set of inputs, it should be easy to observe the output of the code.</span></span> <span data-ttu-id="b2c70-130">Beispielsweise ist das Testen der folgenden Methode einfach, da die-Methode das Ergebnis einer Berechnung direkt zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="b2c70-130">For example, testing the following method is easy because the method directly returns the result of a calculation.</span></span>

``` csharp
    public int Add(int x, int y) {
        return x + y;
    }
```

<span data-ttu-id="b2c70-131">Das Testen einer Methode ist schwierig, wenn die-Methode den berechneten Wert in einen Netzwerk Socket, eine Datenbanktabelle oder eine Datei wie den folgenden Code schreibt.</span><span class="sxs-lookup"><span data-stu-id="b2c70-131">Testing a method is difficult if the method writes the computed value into a network socket, a database table, or a file like the following code.</span></span> <span data-ttu-id="b2c70-132">Der Test muss zusätzliche Arbeitsschritte ausführen, um den Wert abzurufen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-132">The test has to perform additional work to retrieve the value.</span></span>

``` csharp
    public void AddAndSaveToFile(int x, int y) {
         var results = string.Format("The answer is {0}", x + y);
         File.WriteAllText("results.txt", results);
    }
```

<span data-ttu-id="b2c70-133">Zweitens lässt sich der prüfbare Code leicht **isolieren**.</span><span class="sxs-lookup"><span data-stu-id="b2c70-133">Secondly, testable code is easy to **isolate**.</span></span> <span data-ttu-id="b2c70-134">Wir verwenden den folgenden Pseudo Code als ungültiges Beispiel für testbaren Code.</span><span class="sxs-lookup"><span data-stu-id="b2c70-134">Let’s use the following pseudo-code as a bad example of testable code.</span></span>

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

<span data-ttu-id="b2c70-135">Die Methode ist leicht zu beobachten – wir können eine Versicherungsrichtlinie übergeben und überprüfen, ob der Rückgabewert mit einem erwarteten Ergebnis übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="b2c70-135">The method is easy to observe – we can pass in an insurance policy and verify the return value matches an expected result.</span></span> <span data-ttu-id="b2c70-136">Zum Testen der Methode muss jedoch eine Datenbank mit dem richtigen Schema installiert werden, und der SMTP-Server muss konfiguriert werden, falls die Methode versucht, eine e-Mail zu senden.</span><span class="sxs-lookup"><span data-stu-id="b2c70-136">However, to test the method we’ll need to have a database installed with the correct schema, and configure the SMTP server in case the method tries to send an email.</span></span>

<span data-ttu-id="b2c70-137">Der Komponenten Test möchte nur die Berechnungs Logik innerhalb der Methode überprüfen, aber der Test schlägt möglicherweise fehl, weil der e-Mail-Server offline ist oder der Datenbankserver verschoben wurde.</span><span class="sxs-lookup"><span data-stu-id="b2c70-137">The unit test only wants to verify the calculation logic inside the method, but the test might fail because the email server is offline, or because the database server moved.</span></span> <span data-ttu-id="b2c70-138">Beide Fehler stehen nicht im Zusammenhang mit dem Verhalten, das der Test überprüfen soll.</span><span class="sxs-lookup"><span data-stu-id="b2c70-138">Both of these failures are unrelated to the behavior the test wants to verify.</span></span> <span data-ttu-id="b2c70-139">Das Verhalten ist schwierig zu isolieren.</span><span class="sxs-lookup"><span data-stu-id="b2c70-139">The behavior is difficult to isolate.</span></span>

<span data-ttu-id="b2c70-140">Software Entwickler, die sich bemühen, testbaren Code zu schreiben, werden häufig bestrebt sein, Probleme im Code, den Sie schreiben, zu trennen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-140">Software developers who strive to write testable code often strive to maintain a separation of concerns in the code they write.</span></span> <span data-ttu-id="b2c70-141">Die obige Methode sollte sich auf die Geschäfts Berechnungen konzentrieren und die Datenbank-und e-Mail-Implementierungsdetails an andere Komponenten delegieren.</span><span class="sxs-lookup"><span data-stu-id="b2c70-141">The above method should focus on the business calculations and delegate the database and email implementation details to other components.</span></span> <span data-ttu-id="b2c70-142">Robert C. Martin nennt dies das Prinzip der einzigen Verantwortung.</span><span class="sxs-lookup"><span data-stu-id="b2c70-142">Robert C. Martin calls this the Single Responsibility Principle.</span></span> <span data-ttu-id="b2c70-143">Ein Objekt muss eine einzelne, schmale Verantwortung Kapseln, wie z. b. die Berechnung des Werts einer Richtlinie.</span><span class="sxs-lookup"><span data-stu-id="b2c70-143">An object should encapsulate a single, narrow responsibility, like calculating the value of a policy.</span></span> <span data-ttu-id="b2c70-144">Alle anderen Datenbank-und Benachrichtigungs arbeiten sollten die Verantwortung für ein anderes Objekt haben.</span><span class="sxs-lookup"><span data-stu-id="b2c70-144">All other database and notification work should be the responsibility of some other object.</span></span> <span data-ttu-id="b2c70-145">Auf diese Weise geschriebener Code ist leichter zu isolieren, da er sich auf eine einzelne Aufgabe konzentriert.</span><span class="sxs-lookup"><span data-stu-id="b2c70-145">Code written in this fashion is easier to isolate because it is focused on a single task.</span></span>

<span data-ttu-id="b2c70-146">In .net gibt es die Abstraktionen, die wir benötigen, um das Prinzip der einzelnen Verantwortung einzuhalten und Isolation zu erzielen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-146">In .NET we have the abstractions we need to follow the Single Responsibility Principle and achieve isolation.</span></span> <span data-ttu-id="b2c70-147">Wir können Schnittstellendefinitionen verwenden und erzwingen, dass der Code die Schnittstellen Abstraktion anstelle eines konkreten Typs verwendet.</span><span class="sxs-lookup"><span data-stu-id="b2c70-147">We can use interface definitions and force the code to use the interface abstraction instead of a concrete type.</span></span> <span data-ttu-id="b2c70-148">Später in diesem Whitepaper wird erläutert, wie eine Methode wie das ungültige Beispiel mit Schnittstellen funktionieren kann, die so *aussehen* , als Sie mit der Datenbank kommunizieren.</span><span class="sxs-lookup"><span data-stu-id="b2c70-148">Later in this paper we’ll see how a method like the bad example presented above can work with interfaces that *look* like they will talk to the database.</span></span> <span data-ttu-id="b2c70-149">Zum Testzeitpunkt können wir jedoch eine dummyimplementierung ersetzen, die nicht mit der Datenbank kommuniziert, sondern stattdessen Daten im Arbeitsspeicher speichert.</span><span class="sxs-lookup"><span data-stu-id="b2c70-149">At test time, however, we can substitute a dummy implementation that doesn’t talk to the database but instead holds data in memory.</span></span> <span data-ttu-id="b2c70-150">Diese Dummy-Implementierung isoliert den Code von nicht verknüpften Problemen im Datenzugriffs Code oder in der Daten Bank Konfiguration.</span><span class="sxs-lookup"><span data-stu-id="b2c70-150">This dummy implementation will isolate the code from unrelated problems in the data access code or database configuration.</span></span>

<span data-ttu-id="b2c70-151">Es gibt weitere Vorteile bei der Isolierung.</span><span class="sxs-lookup"><span data-stu-id="b2c70-151">There are additional benefits to isolation.</span></span> <span data-ttu-id="b2c70-152">Die Ausführung der Geschäfts Berechnung in der letzten Methode sollte nur wenige Millisekunden dauern, aber der Test selbst kann mehrere Sekunden dauern, da der Code das Netzwerk überspringt und mit verschiedenen Servern kommuniziert.</span><span class="sxs-lookup"><span data-stu-id="b2c70-152">The business calculation in the last method should only take a few milliseconds to execute, but the test itself might run for several seconds as the code hops around the network and talks to various servers.</span></span> <span data-ttu-id="b2c70-153">Komponententests sollten schnell ausgeführt werden, um kleine Änderungen zu vereinfachen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-153">Unit tests should run fast to facilitate small changes.</span></span> <span data-ttu-id="b2c70-154">Komponententests sollten ebenfalls wiederholbar sein und nicht fehlschlagen, da eine Komponente, die nicht mit dem Test verknüpft ist, ein Problem aufweist.</span><span class="sxs-lookup"><span data-stu-id="b2c70-154">Unit tests should also be repeatable and not fail because a component unrelated to the test has a problem.</span></span> <span data-ttu-id="b2c70-155">Das Schreiben von Code, der leicht zu beobachten und zu isolieren ist, bedeutet, dass Entwickler einen einfacheren Zeitaufwand für das Schreiben von Tests für den Code haben, weniger Zeit mit dem warten auf die Ausführung von Tests verbringen und noch wichtiger ist, dass weniger Zeit für das Nachverfolgen von Fehlern aufgewendet wird</span><span class="sxs-lookup"><span data-stu-id="b2c70-155">Writing code that is easy to observe and to isolate means developers will have an easier time writing tests for the code, spend less time waiting for tests to execute, and more importantly, spend less time tracking down bugs that do not exist.</span></span>

<span data-ttu-id="b2c70-156">Wir hoffen, dass Sie die Vorteile der Tests und der Qualität der von Test fähige Code ausgestellten Qualitäten schätzen können.</span><span class="sxs-lookup"><span data-stu-id="b2c70-156">Hopefully you can appreciate the benefits of testing and understand the qualities that testable code exhibits.</span></span> <span data-ttu-id="b2c70-157">Wir sind im Begriff, zu erfahren, wie Sie Code schreiben können, der mit EF4 verwendet wird, um Daten in einer Datenbank zu speichern, während Sie sichtbar und leicht zu isolieren ist, aber zuerst wird der Fokus auf die Erörterung testbarer Entwürfe für den Datenzugriff eingrenzen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-157">We are about to address how to write code that works with EF4 to save data into a database while remaining observable and easy to isolate, but first we’ll narrow our focus to discuss testable designs for data access.</span></span>

## <a name="design-patterns-for-data-persistence"></a><span data-ttu-id="b2c70-158">Entwurfsmuster für die Daten Persistenz</span><span class="sxs-lookup"><span data-stu-id="b2c70-158">Design Patterns for Data Persistence</span></span>

<span data-ttu-id="b2c70-159">Beide ungültigen Beispiele waren zu viele Zuständigkeiten.</span><span class="sxs-lookup"><span data-stu-id="b2c70-159">Both of the bad examples presented earlier had too many responsibilities.</span></span> <span data-ttu-id="b2c70-160">Das erste ungültige Beispiel war das Ausführen einer Berechnung *und* das Schreiben in eine Datei.</span><span class="sxs-lookup"><span data-stu-id="b2c70-160">The first bad example had to perform a calculation *and* write to a file.</span></span> <span data-ttu-id="b2c70-161">Das zweite ungültige Beispiel war das Lesen von Daten aus einer Datenbank *und* das Ausführen einer Geschäfts Berechnung *und* das Senden von e-Mails.</span><span class="sxs-lookup"><span data-stu-id="b2c70-161">The second bad example had to read data from a database *and* perform a business calculation *and* send email.</span></span> <span data-ttu-id="b2c70-162">Wenn Sie kleinere Methoden entwerfen, die die Belange voneinander trennen und die Verantwortung an andere Komponenten delegieren, machen Sie große Fortschritte beim Schreiben von testbarem Code.</span><span class="sxs-lookup"><span data-stu-id="b2c70-162">By designing smaller methods that separate concerns and delegate responsibility to other components you’ll make great strides towards writing testable code.</span></span> <span data-ttu-id="b2c70-163">Das Ziel besteht darin, die Funktionalität zu erstellen, indem Aktionen aus kleinen und fokussierten Abstraktionen verfasst werden.</span><span class="sxs-lookup"><span data-stu-id="b2c70-163">The goal is to build functionality by composing actions from small and focused abstractions.</span></span>

<span data-ttu-id="b2c70-164">Wenn es um die Daten Persistenz geht, sind die kleinen und ausgerichteten Abstraktionen, nach denen wir suchen, so häufig, dass Sie als Entwurfsmuster dokumentiert wurden.</span><span class="sxs-lookup"><span data-stu-id="b2c70-164">When it comes to data persistence the small and focused abstractions we are looking for are so common they’ve been documented as design patterns.</span></span> <span data-ttu-id="b2c70-165">Martin Fowler Book Patterns of Enterprise Application Architecture war die erste Aufgabe, diese Muster im Druck zu beschreiben.</span><span class="sxs-lookup"><span data-stu-id="b2c70-165">Martin Fowler’s book Patterns of Enterprise Application Architecture was the first work to describe these patterns in print.</span></span> <span data-ttu-id="b2c70-166">In den folgenden Abschnitten wird eine kurze Beschreibung dieser Muster bereitgestellt, bevor gezeigt wird, wie diese ADO.NET Entity Framework implementiert und mit diesen Mustern arbeitet.</span><span class="sxs-lookup"><span data-stu-id="b2c70-166">We’ll provide a brief description of these patterns in the following sections before we show how these ADO.NET Entity Framework implements and works with these patterns.</span></span>

### <a name="the-repository-pattern"></a><span data-ttu-id="b2c70-167">Repositorymuster</span><span class="sxs-lookup"><span data-stu-id="b2c70-167">The Repository Pattern</span></span>

<span data-ttu-id="b2c70-168">Fowler sagt, dass ein Repository zwischen den Domänen-und Daten Zustellungs Ebenen mithilfe einer Sammlungs ähnlichen Schnittstelle für den Zugriff auf Domänen Objekte mediiert.</span><span class="sxs-lookup"><span data-stu-id="b2c70-168">Fowler says a repository “mediates between the domain and data mapping layers using a collection-like interface for accessing domain objects”.</span></span> <span data-ttu-id="b2c70-169">Das Ziel des Repository-Musters besteht darin, Code aus den Minutiae des Datenzugriffs zu isolieren. wie bereits erwähnt wurde, ist die Isolation ein erforderliches Merkmal für die Prüfbarkeit.</span><span class="sxs-lookup"><span data-stu-id="b2c70-169">The goal of the repository pattern is to isolate code from the minutiae of data access, and as we saw earlier isolation is a required trait for testability.</span></span>

<span data-ttu-id="b2c70-170">Der Schlüssel zur Isolation ist, wie das Repository Objekte mithilfe einer Auflistungs ähnlichen Schnittstelle verfügbar macht.</span><span class="sxs-lookup"><span data-stu-id="b2c70-170">The key to the isolation is how the repository exposes objects using a collection-like interface.</span></span> <span data-ttu-id="b2c70-171">Die Logik, die Sie schreiben, um das Repository zu verwenden, hat keine Idee, wie das Repository die von Ihnen angeforderten Objekte materialisieren soll.</span><span class="sxs-lookup"><span data-stu-id="b2c70-171">The logic you write to use the repository has no idea how the repository will materialize the objects you request.</span></span> <span data-ttu-id="b2c70-172">Das Repository kann mit einer Datenbank kommunizieren, oder es können nur Objekte aus einer Auflistung im Speicher zurückgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="b2c70-172">The repository might talk to a database, or it might just return objects from an in-memory collection.</span></span> <span data-ttu-id="b2c70-173">Der gesamte Code muss wissen, dass das Repository die Auflistung beibehält, und Sie können Objekte aus der Auflistung abrufen, hinzufügen und löschen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-173">All your code needs to know is that the repository appears to maintain the collection, and you can retrieve, add, and delete objects from the collection.</span></span>

<span data-ttu-id="b2c70-174">In vorhandenen .NET-Anwendungen erbt ein konkretes Repository häufig von einer generischen Schnittstelle wie der folgenden:</span><span class="sxs-lookup"><span data-stu-id="b2c70-174">In existing .NET applications a concrete repository often inherits from a generic interface like the following:</span></span>

``` csharp
    public interface IRepository<T> {       
        IEnumerable<T> FindAll();
        IEnumerable<T> FindBy(Expression<Func\<T, bool>> predicate);
        T FindById(int id);
        void Add(T newEntity);
        void Remove(T entity);
    }
```

<span data-ttu-id="b2c70-175">Wir nehmen einige Änderungen an der Schnittstellen Definition vor, wenn wir eine Implementierung für EF4 bereitstellen, das grundlegende Konzept bleibt jedoch unverändert.</span><span class="sxs-lookup"><span data-stu-id="b2c70-175">We’ll make a few changes to the interface definition when we provide an implementation for EF4, but the basic concept remains the same.</span></span> <span data-ttu-id="b2c70-176">Code kann ein konkretes Repository verwenden, das diese Schnittstelle implementiert, um eine Entität nach ihrem Primärschlüssel Wert abzurufen, eine Auflistung von Entitäten basierend auf der Auswertung eines Prädikats abzurufen oder einfach alle verfügbaren Entitäten abzurufen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-176">Code can use a concrete repository implementing this interface to retrieve an entity by its primary key value, to retrieve a collection of entities based on the evaluation of a predicate, or simply retrieve all available entities.</span></span> <span data-ttu-id="b2c70-177">Der Code kann auch Entitäten über die Repository-Schnittstelle hinzufügen und entfernen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-177">The code can also add and remove entities through the repository interface.</span></span>

<span data-ttu-id="b2c70-178">Bei einem IRepository von Employee-Objekten kann Code die folgenden Vorgänge ausführen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-178">Given an IRepository of Employee objects, code can perform the following operations.</span></span>

``` csharp
    var employeesNamedScott =
        repository
            .FindBy(e => e.Name == "Scott")
            .OrderBy(e => e.HireDate);
    var firstEmployee = repository.FindById(1);
    var newEmployee = new Employee() {/*... */};
    repository.Add(newEmployee);
```

<span data-ttu-id="b2c70-179">Da der Code eine Schnittstelle (IRepository of Employee) verwendet, können wir den Code mit verschiedenen Implementierungen der Schnittstelle bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-179">Since the code is using an interface (IRepository of Employee), we can provide the code with different implementations of the interface.</span></span> <span data-ttu-id="b2c70-180">Eine Implementierung kann eine Implementierung sein, die von EF4 und das Beibehalten von Objekten in einer Microsoft SQL Server Datenbank unterstützt wird.</span><span class="sxs-lookup"><span data-stu-id="b2c70-180">One implementation might be an implementation backed by EF4 and persisting objects into a Microsoft SQL Server database.</span></span> <span data-ttu-id="b2c70-181">Eine andere-Implementierung (die während des Tests verwendet wird) kann durch eine in-Memory-Liste von Employee-Objekten unterstützt werden.</span><span class="sxs-lookup"><span data-stu-id="b2c70-181">A different implementation (one we use during testing) might be backed by an in-memory List of Employee objects.</span></span> <span data-ttu-id="b2c70-182">Die-Schnittstelle unterstützt Sie dabei, die Isolation im Code zu erreichen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-182">The interface will help to achieve isolation in the code.</span></span>

<span data-ttu-id="b2c70-183">Beachten Sie, dass die IRepository-&lt;t-&gt; Schnittstelle keinen Speichervorgang verfügbar macht.</span><span class="sxs-lookup"><span data-stu-id="b2c70-183">Notice the IRepository&lt;T&gt; interface does not expose a Save operation.</span></span> <span data-ttu-id="b2c70-184">Wie werden vorhandene Objekte aktualisiert?</span><span class="sxs-lookup"><span data-stu-id="b2c70-184">How do we update existing objects?</span></span> <span data-ttu-id="b2c70-185">Sie können über IRepository-Definitionen verfügen, die den Speichervorgang einschließen, und Implementierungen dieser Depots müssen ein Objekt direkt in der Datenbank speichern.</span><span class="sxs-lookup"><span data-stu-id="b2c70-185">You might come across IRepository definitions that do include the Save operation, and implementations of these repositories will need to immediately persist an object into the database.</span></span> <span data-ttu-id="b2c70-186">In vielen Anwendungen möchten wir Objekte jedoch nicht einzeln beibehalten.</span><span class="sxs-lookup"><span data-stu-id="b2c70-186">However, in many applications we don’t want to persist objects individually.</span></span> <span data-ttu-id="b2c70-187">Stattdessen möchten wir Objekte zum Leben bringen, vielleicht aus verschiedenen Depots, die Objekte im Rahmen einer Geschäftsaktivität ändern und dann alle Objekte im Rahmen eines einzelnen atomarischen Vorgangs beibehalten.</span><span class="sxs-lookup"><span data-stu-id="b2c70-187">Instead, we want to bring objects to life, perhaps from different repositories, modify those objects as part of a business activity, and then persist all the objects as part of a single, atomic operation.</span></span> <span data-ttu-id="b2c70-188">Glücklicherweise gibt es ein Muster, das diese Art von Verhalten zulässt.</span><span class="sxs-lookup"><span data-stu-id="b2c70-188">Fortunately, there is a pattern to allow this type of behavior.</span></span>

### <a name="the-unit-of-work-pattern"></a><span data-ttu-id="b2c70-189">Das Arbeits Einheits Muster</span><span class="sxs-lookup"><span data-stu-id="b2c70-189">The Unit of Work Pattern</span></span>

<span data-ttu-id="b2c70-190">Fowler sagt, dass eine Arbeitseinheit eine Liste von Objekten verwaltet, die von einer Geschäftstransaktion betroffen sind, und koordiniert das Schreiben von Änderungen und die Auflösung von Parallelitäts Problemen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-190">Fowler says a unit of work will “maintain a list of objects affected by a business transaction and coordinates the writing out of changes and the resolution of concurrency problems”.</span></span> <span data-ttu-id="b2c70-191">Es liegt in der Verantwortung der Arbeitseinheit, Änderungen an den Objekten zu verfolgen, die wir aus einem Repository in den Lebenszyklus bringen, und die Änderungen beizubehalten, die wir an den Objekten vorgenommen haben, wenn wir die Arbeitseinheit anweisen, die Änderungen zu übernehmen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-191">It is the responsibility of the unit of work to track changes to the objects we bring to life from a repository and persist any changes we’ve made to the objects when we tell the unit of work to commit the changes.</span></span> <span data-ttu-id="b2c70-192">Außerdem liegt es in der Verantwortung der Arbeitseinheit, die neuen Objekte zu übernehmen, die wir allen Depots hinzugefügt haben, und die Objekte in eine Datenbank einzufügen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-192">It’s also the responsibility of the unit of work to take the new objects we’ve added to all repositories and insert the objects into a database, and also mange deletion.</span></span>

<span data-ttu-id="b2c70-193">Wenn Sie schon einmal mit ADO.NET DataSets gearbeitet haben, sind Sie bereits mit dem Unit of Work-Muster vertraut.</span><span class="sxs-lookup"><span data-stu-id="b2c70-193">If you’ve ever done any work with ADO.NET DataSets then you’ll already be familiar with the unit of work pattern.</span></span> <span data-ttu-id="b2c70-194">ADO.NET Datasets konnten unsere Updates, Löschungen und Einfügungen von DataRow-Objekten nachverfolgen und konnten (mit einem TableAdapter) alle Änderungen an einer Datenbank abgleichen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-194">ADO.NET DataSets had the ability to track our updates, deletions, and insertion of DataRow objects and could (with the help of a TableAdapter) reconcile all our changes to a database.</span></span> <span data-ttu-id="b2c70-195">DataSet-Objekte modellieren jedoch eine getrennte Teilmenge der zugrunde liegenden Datenbank.</span><span class="sxs-lookup"><span data-stu-id="b2c70-195">However, DataSet objects model a disconnected subset of the underlying database.</span></span> <span data-ttu-id="b2c70-196">Das Arbeitseinheits Muster weist das gleiche Verhalten auf, funktioniert jedoch mit Geschäftsobjekten und Domänen Objekten, die vom Datenzugriffs Code isoliert sind und nicht über die Datenbank verfügen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-196">The unit of work pattern exhibits the same behavior, but works with business objects and domain objects that are isolated from data access code and unaware of the database.</span></span>

<span data-ttu-id="b2c70-197">Eine Abstraktion, um die Arbeitseinheit in .NET-Code zu modellieren, könnte wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="b2c70-197">An abstraction to model the unit of work in .NET code might look like the following:</span></span>

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<Order> Orders { get; }
        IRepository<Customer> Customers { get; }
        void Commit();
    }
```

<span data-ttu-id="b2c70-198">Durch das verfügbar machen von Repository-verweisen aus der Arbeitseinheit kann sichergestellt werden, dass ein einzelnes Arbeitseinheiten Objekt über die Möglichkeit verfügt, alle Entitäten zu verfolgen, die während einer Geschäftstransaktion materialisiert werden.</span><span class="sxs-lookup"><span data-stu-id="b2c70-198">By exposing repository references from the unit of work we can ensure a single unit of work object has the ability to track all entities materialized during a business transaction.</span></span> <span data-ttu-id="b2c70-199">Die Implementierung der Commit-Methode für eine echte Arbeitseinheit besteht darin, dass der gesamte Magic-Vorgang im Arbeitsspeicher mit der Datenbank abgeglichen wird.</span><span class="sxs-lookup"><span data-stu-id="b2c70-199">The implementation of the Commit method for a real unit of work is where all the magic happens to reconcile in-memory changes with the database.</span></span> 

<span data-ttu-id="b2c70-200">Bei einem iunitofwork-Verweis kann Codeänderungen an Geschäftsobjekten vornehmen, die aus einem oder mehreren Depots abgerufen wurden, und alle Änderungen mithilfe des atomarischen Commitvorgangs speichern.</span><span class="sxs-lookup"><span data-stu-id="b2c70-200">Given an IUnitOfWork reference, code can make changes to business objects retrieved from one or more repositories and save all the changes using the atomic Commit operation.</span></span>

``` csharp
    var firstEmployee = unitofWork.Employees.FindById(1);
    var firstCustomer = unitofWork.Customers.FindById(1);
    firstEmployee.Name = "Alex";
    firstCustomer.Name = "Christopher";
    unitofWork.Commit();
```

### <a name="the-lazy-load-pattern"></a><span data-ttu-id="b2c70-201">Das Lazy Load-Muster</span><span class="sxs-lookup"><span data-stu-id="b2c70-201">The Lazy Load Pattern</span></span>

<span data-ttu-id="b2c70-202">Fowler verwendet den Namen Lazy Load, um "ein Objekt zu beschreiben, das nicht alle benötigten Daten enthält, aber weiß, wie es zu erhalten ist".</span><span class="sxs-lookup"><span data-stu-id="b2c70-202">Fowler uses the name lazy load to describe “an object that doesn’t contain all of the data you need but knows how to get it”.</span></span> <span data-ttu-id="b2c70-203">Transparente Lazy Loading ist ein wichtiges Feature, das beim Schreiben von testbarem Geschäfts Code und beim Arbeiten mit einer relationalen Datenbank erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="b2c70-203">Transparent lazy loading is an important feature to have when writing testable business code and working with a relational database.</span></span> <span data-ttu-id="b2c70-204">Sehen Sie sich als Beispiel den folgenden Code an:</span><span class="sxs-lookup"><span data-stu-id="b2c70-204">As an example, consider the following code.</span></span>

``` csharp
    var employee = repository.FindById(id);
    // ... and later ...
    foreach(var timeCard in employee.TimeCards) {
        // .. manipulate the timeCard
    }
```

<span data-ttu-id="b2c70-205">Wie wird die Sammlung "TIMECARDS" aufgefüllt?</span><span class="sxs-lookup"><span data-stu-id="b2c70-205">How is the TimeCards collection populated?</span></span> <span data-ttu-id="b2c70-206">Es gibt zwei mögliche Antworten.</span><span class="sxs-lookup"><span data-stu-id="b2c70-206">There are two possible answers.</span></span> <span data-ttu-id="b2c70-207">Eine Antwort ist, dass das Employee-Repository bei der Aufforderung zum Abrufen eines Mitarbeiters eine Abfrage ausgibt, um den Mitarbeiter zusammen mit den zugehörigen Zeitkarten Informationen des Mitarbeiters abzurufen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-207">One answer is that the employee repository, when asked to fetch an employee, issues a query to retrieve both the employee along with the employee’s associated time card information.</span></span> <span data-ttu-id="b2c70-208">In relationalen Datenbanken erfordert dies in der Regel eine Abfrage mit einer Join-Klausel und kann dazu führen, dass mehr Informationen als die Anwendungsanforderungen abgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="b2c70-208">In relational databases this generally requires a query with a JOIN clause and may result in retrieving more information than an application needs.</span></span> <span data-ttu-id="b2c70-209">Was geschieht, wenn die Anwendung die TIMECARDS-Eigenschaft nie berühren muss?</span><span class="sxs-lookup"><span data-stu-id="b2c70-209">What if the application never needs to touch the TimeCards property?</span></span>

<span data-ttu-id="b2c70-210">Eine zweite Antwort besteht darin, die TIMECARDS-Eigenschaft "Bedarfs gesteuert" zu laden.</span><span class="sxs-lookup"><span data-stu-id="b2c70-210">A second answer is to load the TimeCards property “on demand”.</span></span> <span data-ttu-id="b2c70-211">Diese Lazy Loading ist implizit und transparent für die Geschäftslogik, da der Code keine speziellen APIs aufruft, um Zeitkarten Informationen abzurufen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-211">This lazy loading is implicit and transparent to the business logic because the code does not invoke special APIs to retrieve time card information.</span></span> <span data-ttu-id="b2c70-212">Der Code setzt voraus, dass die Zeitkarten Informationen bei Bedarf vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="b2c70-212">The code assumes the time card information is present when needed.</span></span> <span data-ttu-id="b2c70-213">Es gibt eine Reihe von Magic-Lazy Loading, die im Allgemeinen das Lauf Zeit Abfangen von Methoden aufrufen umfasst.</span><span class="sxs-lookup"><span data-stu-id="b2c70-213">There is some magic involved with lazy loading that generally involves runtime interception of method invocations.</span></span> <span data-ttu-id="b2c70-214">Der abzurufende Code ist für die Kommunikation mit der Datenbank und das Abrufen von Zeitkarten Informationen verantwortlich, während die Geschäftslogik kostenlos als Geschäftslogik belassen wird.</span><span class="sxs-lookup"><span data-stu-id="b2c70-214">The intercepting code is responsible for talking to the database and retrieving time card information while leaving the business logic free to be business logic.</span></span> <span data-ttu-id="b2c70-215">Diese Lazy Load-Magic ermöglicht es dem Geschäfts Code, sich selbst von Datenabruf Vorgängen zu isolieren, und führt zu einem besser prüfbaren Code.</span><span class="sxs-lookup"><span data-stu-id="b2c70-215">This lazy load magic allows the business code to isolate itself from data retrieval operations and results in more testable code.</span></span>

<span data-ttu-id="b2c70-216">Der Nachteil eines verzögerten Ladens besteht darin, dass der Code eine zusätzliche Abfrage ausführt, wenn eine Anwendung die Zeitkarten *Informationen benötigt.*</span><span class="sxs-lookup"><span data-stu-id="b2c70-216">The drawback to a lazy load is that when an application *does* need the time card information the code will execute an additional query.</span></span> <span data-ttu-id="b2c70-217">Dies ist für viele Anwendungen nicht von Bedeutung, aber für Leistungs relevante Anwendungen oder Anwendungen, die eine Reihe von Mitarbeiter Objekten durchlaufen und eine Abfrage ausführen, um Zeitkarten während der Iterationen der Schleife abzurufen (ein Problem, das häufig als "N + 1" bezeichnet wird). Abfrage Problem), Lazy Loading ein Drag-Vorgang.</span><span class="sxs-lookup"><span data-stu-id="b2c70-217">This isn’t a concern for many applications, but for performance sensitive applications or applications looping through a number of employee objects and executing a query to retrieve time cards during each iteration of the loop (a problem often referred to as the N+1 query problem), lazy loading is a drag.</span></span> <span data-ttu-id="b2c70-218">In diesen Szenarien kann es vorkommen, dass eine Anwendung Zeitkarten Informationen auf möglichst effiziente Weise lädt.</span><span class="sxs-lookup"><span data-stu-id="b2c70-218">In these scenarios an application might want to eagerly load time card information in the most efficient manner possible.</span></span>

<span data-ttu-id="b2c70-219">Glücklicherweise werden wir sehen, wie EF4 sowohl implizite Lazy Loads als auch effiziente, sorgfältige Auslastungen unterstützt, wenn wir mit dem nächsten Abschnitt fortfahren und diese Muster implementieren.</span><span class="sxs-lookup"><span data-stu-id="b2c70-219">Fortunately, we’ll see how EF4 supports both implicit lazy loads and efficient eager loads as we move into the next section and implement these patterns.</span></span>

## <a name="implementing-patterns-with-the-entity-framework"></a><span data-ttu-id="b2c70-220">Implementieren von Mustern mit dem Entity Framework</span><span class="sxs-lookup"><span data-stu-id="b2c70-220">Implementing Patterns with the Entity Framework</span></span>

<span data-ttu-id="b2c70-221">Die gute Nachricht ist, dass alle Entwurfsmuster, die wir im letzten Abschnitt beschrieben haben, einfach mit EF4 implementiert werden können.</span><span class="sxs-lookup"><span data-stu-id="b2c70-221">The good news is that all of the design patterns we described in the last section are straightforward to implement with EF4.</span></span> <span data-ttu-id="b2c70-222">Um zu veranschaulichen, dass wir eine einfache ASP.NET MVC-Anwendung verwenden, um Mitarbeiter und die dazugehörigen Zeitkarten Informationen zu bearbeiten und anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-222">To demonstrate we are going to use a simple ASP.NET MVC application to edit and display Employees and their associated time card information.</span></span> <span data-ttu-id="b2c70-223">Wir beginnen mit der Verwendung der folgenden "Plain Old CLR Objects" (POCOS).</span><span class="sxs-lookup"><span data-stu-id="b2c70-223">We’ll start by using the following “plain old CLR objects” (POCOs).</span></span> 

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

<span data-ttu-id="b2c70-224">Diese Klassendefinitionen ändern sich geringfügig, da wir unterschiedliche Ansätze und Features von EF4 untersuchen, aber die Absicht besteht darin, diese Klassen möglichst persistent zu halten.</span><span class="sxs-lookup"><span data-stu-id="b2c70-224">These class definitions will change slightly as we explore different approaches and features of EF4, but the intent is to keep these classes as persistence ignorant (PI) as possible.</span></span> <span data-ttu-id="b2c70-225">Ein PI-Objekt weiß nicht, *wie*oder auch *Wenn*der Zustand, den er enthält, innerhalb einer Datenbank ist.</span><span class="sxs-lookup"><span data-stu-id="b2c70-225">A PI object doesn’t know *how*, or even *if*, the state it holds lives inside a database.</span></span> <span data-ttu-id="b2c70-226">Pi und POCOS sind mit Test fähiger Software Hand.</span><span class="sxs-lookup"><span data-stu-id="b2c70-226">PI and POCOs go hand in hand with testable software.</span></span> <span data-ttu-id="b2c70-227">Objekte, die einen poco-Ansatz verwenden, sind weniger eingeschränkt, flexibler und leichter zu testen, da Sie ohne eine vorhandene Datenbank betrieben werden können.</span><span class="sxs-lookup"><span data-stu-id="b2c70-227">Objects using a POCO approach are less constrained, more flexible, and easier to test because they can operate without a database present.</span></span>

<span data-ttu-id="b2c70-228">Mit den POCOS können wir eine Entity Data Model (EDM) in Visual Studio erstellen (siehe Abbildung 1).</span><span class="sxs-lookup"><span data-stu-id="b2c70-228">With the POCOs in place we can create an Entity Data Model (EDM) in Visual Studio (see figure 1).</span></span> <span data-ttu-id="b2c70-229">Der EDM wird nicht zum Generieren von Code für unsere Entitäten verwendet.</span><span class="sxs-lookup"><span data-stu-id="b2c70-229">We will not use the EDM to generate code for our entities.</span></span> <span data-ttu-id="b2c70-230">Stattdessen möchten wir die Entitäten verwenden, die wir in Hand haben.</span><span class="sxs-lookup"><span data-stu-id="b2c70-230">Instead, we want to use the entities we lovingly craft by hand.</span></span> <span data-ttu-id="b2c70-231">Wir verwenden das EDM nur zum Generieren des Datenbankschemas und zur Bereitstellung der Metadaten EF4 muss Objekte der Datenbank zuordnen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-231">We will only use the EDM to generate our database schema and provide the metadata EF4 needs to map objects into the database.</span></span>

![EF-test_01](~/ef6/media/eftest-01.jpg)

<span data-ttu-id="b2c70-233">**Abbildung 1**</span><span class="sxs-lookup"><span data-stu-id="b2c70-233">**Figure 1**</span></span>

<span data-ttu-id="b2c70-234">Hinweis: Wenn Sie das EDM-Modell zuerst entwickeln möchten, ist es möglich, sauberen, Poco-Code aus dem EDM zu generieren.</span><span class="sxs-lookup"><span data-stu-id="b2c70-234">Note: if you want to develop the EDM model first, it is possible to generate clean, POCO code from the EDM.</span></span> <span data-ttu-id="b2c70-235">Dies können Sie mit einer Erweiterung von Visual Studio 2010 durchführen, die vom datenprogrammierbarkeits-Team bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="b2c70-235">You can do this with a Visual Studio 2010 extension provided by the Data Programmability team.</span></span> <span data-ttu-id="b2c70-236">Um die Erweiterung herunterzuladen, starten Sie den Erweiterungs-Manager über das Menü Extras in Visual Studio, und Durchsuchen Sie den Onlinekatalog der Vorlagen nach "poco" (siehe Abbildung 2).</span><span class="sxs-lookup"><span data-stu-id="b2c70-236">To download the extension, launch the Extension Manager from the Tools menu in Visual Studio and search the online gallery of templates for “POCO” (See figure 2).</span></span> <span data-ttu-id="b2c70-237">Für EF sind mehrere poco-Vorlagen verfügbar.</span><span class="sxs-lookup"><span data-stu-id="b2c70-237">There are several POCO templates available for EF.</span></span> <span data-ttu-id="b2c70-238">Weitere Informationen zur Verwendung der Vorlage finden Sie unter "Exemplarische Vorgehensweise [: POCO-Vorlage für die Entity Framework](https://blogs.msdn.com/adonet/pages/walkthrough-poco-template-for-the-entity-framework.aspx)".</span><span class="sxs-lookup"><span data-stu-id="b2c70-238">For more information on using the template, see “ [Walkthrough: POCO Template for the Entity Framework](https://blogs.msdn.com/adonet/pages/walkthrough-poco-template-for-the-entity-framework.aspx)”.</span></span>

![EF-test_02](~/ef6/media/eftest-02.png)

<span data-ttu-id="b2c70-240">**Abbildung 2**</span><span class="sxs-lookup"><span data-stu-id="b2c70-240">**Figure 2**</span></span>

<span data-ttu-id="b2c70-241">Von diesem poco-Ausgangspunkt aus untersuchen wir zwei unterschiedliche Ansätze für Test fähigen Code.</span><span class="sxs-lookup"><span data-stu-id="b2c70-241">From this POCO starting point we will explore two different approaches to testable code.</span></span> <span data-ttu-id="b2c70-242">Der erste Ansatz ist der EF-Ansatz, da er Abstraktionen von der Entity Framework-API nutzt, um Arbeitseinheiten und Depots zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="b2c70-242">The first approach I call the EF approach because it leverages abstractions from the Entity Framework API to implement units of work and repositories.</span></span> <span data-ttu-id="b2c70-243">In der zweiten Vorgehensweise werden wir unsere eigenen benutzerdefinierten Repository-Abstraktionen erstellen und dann die vor-und Nachteile der einzelnen Ansätze sehen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-243">In the second approach we will create our own custom repository abstractions and then see the advantages and disadvantages of each approach.</span></span> <span data-ttu-id="b2c70-244">Wir beginnen mit dem EF-Ansatz.</span><span class="sxs-lookup"><span data-stu-id="b2c70-244">We’ll start by exploring the EF approach.</span></span>  

### <a name="an-ef-centric-implementation"></a><span data-ttu-id="b2c70-245">Eine EF-zentrierte Implementierung</span><span class="sxs-lookup"><span data-stu-id="b2c70-245">An EF Centric Implementation</span></span>

<span data-ttu-id="b2c70-246">Sehen Sie sich die folgende Controller Aktion aus einem ASP.NET-MVC-Projekt an.</span><span class="sxs-lookup"><span data-stu-id="b2c70-246">Consider the following controller action from an ASP.NET MVC project.</span></span> <span data-ttu-id="b2c70-247">Mit der Aktion wird ein Employee-Objekt abgerufen, und es wird ein Ergebnis zurückgegeben, um eine detaillierte Ansicht des Mitarbeiters anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-247">The action retrieves an Employee object and returns a result to display a detailed view of the employee.</span></span>

``` csharp
    public ViewResult Details(int id) {
        var employee = _unitOfWork.Employees
                                  .Single(e => e.Id == id);
        return View(employee);
    }
```

<span data-ttu-id="b2c70-248">Ist der Code testable?</span><span class="sxs-lookup"><span data-stu-id="b2c70-248">Is the code testable?</span></span> <span data-ttu-id="b2c70-249">Es gibt mindestens zwei Tests, die wir benötigen, um das Verhalten der Aktion zu überprüfen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-249">There are at least two tests we’d need to verify the action’s behavior.</span></span> <span data-ttu-id="b2c70-250">Zuerst möchten wir überprüfen, ob die Aktion die korrekte Ansicht zurückgibt – einen einfachen Test.</span><span class="sxs-lookup"><span data-stu-id="b2c70-250">First, we’d like to verify the action returns the correct view – an easy test.</span></span> <span data-ttu-id="b2c70-251">Wir möchten auch einen Test schreiben, um zu überprüfen, ob die Aktion den richtigen Mitarbeiter abruft, und wir möchten dies tun, ohne Code zum Abfragen der Datenbank auszuführen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-251">We’d also want to write a test to verify the action retrieves the correct employee, and we’d like to do this without executing code to query the database.</span></span> <span data-ttu-id="b2c70-252">Denken Sie daran, dass der zu testende Code isoliert werden soll.</span><span class="sxs-lookup"><span data-stu-id="b2c70-252">Remember we want to isolate the code under test.</span></span> <span data-ttu-id="b2c70-253">Durch die Isolation wird sichergestellt, dass der Test aufgrund eines Fehlers im Datenzugriffs Code oder in der Daten Bank Konfiguration nicht fehlschlägt.</span><span class="sxs-lookup"><span data-stu-id="b2c70-253">Isolation will ensure the test doesn’t fail because of a bug in the data access code or database configuration.</span></span> <span data-ttu-id="b2c70-254">Wenn der Test fehlschlägt, wissen wir, dass ein Fehler in der Controller Logik und nicht in einer anderen Systemkomponente vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="b2c70-254">If the test fails, we will know we have a bug in the controller logic, and not in some lower level system component.</span></span>

<span data-ttu-id="b2c70-255">Um die Isolation zu erreichen, benötigen wir einige Abstraktionen, wie z. b. die Schnittstellen, die wir zuvor für die Depots und Arbeitseinheiten</span><span class="sxs-lookup"><span data-stu-id="b2c70-255">To achieve isolation we’ll need some abstractions like the interfaces we presented earlier for repositories and units of work.</span></span> <span data-ttu-id="b2c70-256">Denken Sie daran, dass das Repository-Muster für die Vermittlung zwischen Domänen Objekten und der Datenzuordnung konzipiert ist.</span><span class="sxs-lookup"><span data-stu-id="b2c70-256">Remember the repository pattern is designed to mediate between domain objects and the data mapping layer.</span></span> <span data-ttu-id="b2c70-257">In diesem Szenario *ist* EF4 die Daten Zustellungs Ebene und stellt bereits eine Repository-ähnliche Abstraktion namens IObjectSet&lt;t&gt; (aus dem Namespace System. Data. Objects) bereit.</span><span class="sxs-lookup"><span data-stu-id="b2c70-257">In this scenario EF4 *is* the data mapping layer, and already provides a repository-like abstraction named IObjectSet&lt;T&gt; (from the System.Data.Objects namespace).</span></span> <span data-ttu-id="b2c70-258">Die Schnittstellen Definition sieht wie folgt aus.</span><span class="sxs-lookup"><span data-stu-id="b2c70-258">The interface definition looks like the following.</span></span>

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

<span data-ttu-id="b2c70-259">IObjectSet&lt;t&gt; erfüllt die Anforderungen für ein Repository, da es einer Auflistung von Objekten ähnelt (über IEnumerable&lt;t&gt;) und Methoden zum Hinzufügen und Entfernen von Objekten aus der simulierten Auflistung bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="b2c70-259">IObjectSet&lt;T&gt; meets the requirements for a repository because it resembles a collection of objects (via IEnumerable&lt;T&gt;) and provides methods to add and remove objects from the simulated collection.</span></span> <span data-ttu-id="b2c70-260">Die Anfüge-und Trennmethoden machen zusätzliche Funktionen der EF4-API verfügbar.</span><span class="sxs-lookup"><span data-stu-id="b2c70-260">The Attach and Detach methods expose additional capabilities of the EF4 API.</span></span> <span data-ttu-id="b2c70-261">Um IObjectSet&lt;t&gt; als die Schnittstelle für die Repository-Schnittstelle zu verwenden, benötigen wir eine Arbeitseinheits-Abstraktion, um das Repository zu binden.</span><span class="sxs-lookup"><span data-stu-id="b2c70-261">To use IObjectSet&lt;T&gt; as the interface for repositories we need a unit of work abstraction to bind repositories together.</span></span>

``` csharp
    public interface IUnitOfWork {
        IObjectSet<Employee> Employees { get; }
        IObjectSet<TimeCard> TimeCards { get; }
        void Commit();
    }
```

<span data-ttu-id="b2c70-262">Eine konkrete Implementierung dieser Schnittstelle wird mit SQL Server kommunizieren und ist einfach zu erstellen, indem die ObjectContext-Klasse aus EF4 verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="b2c70-262">One concrete implementation of this interface will talk to SQL Server and is easy to create using the ObjectContext class from EF4.</span></span> <span data-ttu-id="b2c70-263">Die ObjectContext-Klasse ist die tatsächliche Arbeitseinheit in der EF4-API.</span><span class="sxs-lookup"><span data-stu-id="b2c70-263">The ObjectContext class is the real unit of work in the EF4 API.</span></span>

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

<span data-ttu-id="b2c70-264">Das Abrufen einer IObjectSet-&lt;t-&gt; zum Leben ist so einfach wie das Aufrufen der Methode "| ateobjectset" des Objekts "ObjectContext".</span><span class="sxs-lookup"><span data-stu-id="b2c70-264">Bringing an IObjectSet&lt;T&gt; to life is as easy as invoking the CreateObjectSet method of the ObjectContext object.</span></span> <span data-ttu-id="b2c70-265">Im Hintergrund verwendet das Framework die Metadaten, die wir im EDM bereitgestellt haben, um einen konkreten ObjectSet&lt;t&gt;zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="b2c70-265">Behind the scenes the framework will use the metadata we provided in the EDM to produce a concrete ObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="b2c70-266">Wir halten uns an die Rückgabe der IObjectSet-&lt;t&gt;-Schnittstelle, da Sie dabei helfen soll, die Prüfbarkeit im Client Code beizubehalten.</span><span class="sxs-lookup"><span data-stu-id="b2c70-266">We’ll stick with returning the IObjectSet&lt;T&gt; interface because it will help preserve testability in client code.</span></span>

<span data-ttu-id="b2c70-267">Diese konkrete Implementierung ist in der Produktion nützlich, aber wir müssen uns darauf konzentrieren, wie wir unsere iuniyfwork-Abstraktion verwenden, um Tests zu vereinfachen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-267">This concrete implementation is useful in production, but we need to focus on how we’ll use our IUnitOfWork abstraction to facilitate testing.</span></span>

### <a name="the-test-doubles"></a><span data-ttu-id="b2c70-268">Die Test Doubles</span><span class="sxs-lookup"><span data-stu-id="b2c70-268">The Test Doubles</span></span>

<span data-ttu-id="b2c70-269">Um die Controller Aktion zu isolieren, benötigen wir die Möglichkeit, zwischen der realen Arbeitseinheit (gestützt durch einen ObjectContext) und einer Test Double-oder "Fake"-Arbeitseinheit zu wechseln (Durchführung von in-Memory-Vorgängen).</span><span class="sxs-lookup"><span data-stu-id="b2c70-269">To isolate the controller action we’ll need the ability to switch between the real unit of work (backed by an ObjectContext) and a test double or “fake” unit of work (performing in-memory operations).</span></span> <span data-ttu-id="b2c70-270">Der gängige Ansatz zum Durchführen dieser Art von Umschaltung besteht darin, dass der MVC-Controller nicht eine Arbeitseinheit instanziiert, sondern stattdessen die Arbeitseinheit als Konstruktorparameter an den Controller übergibt.</span><span class="sxs-lookup"><span data-stu-id="b2c70-270">The common approach to perform this type of switching is to not let the MVC controller instantiate a unit of work, but instead pass the unit of work into the controller as a constructor parameter.</span></span>

``` csharp
    class EmployeeController : Controller {
      publicEmployeeController(IUnitOfWork unitOfWork)  {
          _unitOfWork = unitOfWork;
      }
      ...
    }
```

<span data-ttu-id="b2c70-271">Der obige Code ist ein Beispiel für eine Abhängigkeitsinjektion.</span><span class="sxs-lookup"><span data-stu-id="b2c70-271">The above code is an example of dependency injection.</span></span> <span data-ttu-id="b2c70-272">Wir gestatten dem Controller nicht, seine Abhängigkeit (die Arbeitseinheit) zu erstellen, sondern die Abhängigkeit in den Controller einzufügen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-272">We don’t allow the controller to create it’s dependency (the unit of work) but inject the dependency into the controller.</span></span> <span data-ttu-id="b2c70-273">In einem MVC-Projekt ist es üblich, eine benutzerdefinierte Controller-Factory in Kombination mit einem IOC-Container (Inversion of Control) zu verwenden, um die Abhängigkeitsinjektion zu automatisieren.</span><span class="sxs-lookup"><span data-stu-id="b2c70-273">In an MVC project it is common to use a custom controller factory in combination with an inversion of control (IoC) container to automate dependency injection.</span></span> <span data-ttu-id="b2c70-274">Diese Themen gehen über den Rahmen dieses Artikels hinaus, aber Sie können weitere Informationen lesen, indem Sie die Verweise am Ende dieses Artikels befolgen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-274">These topics are beyond the scope of this article, but you can read more by following the references at the end of this article.</span></span>

<span data-ttu-id="b2c70-275">Eine gefälschte Arbeitseinheit, die wir für Tests verwenden können, könnte wie folgt aussehen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-275">A fake unit of work implementation that we can use for testing might look like the following.</span></span>

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

<span data-ttu-id="b2c70-276">Beachten Sie, dass die gefälschte Arbeitseinheit eine durch ein Commit getrennte Eigenschaft verfügbar macht.</span><span class="sxs-lookup"><span data-stu-id="b2c70-276">Notice the fake unit of work exposes a Commited property.</span></span> <span data-ttu-id="b2c70-277">Es ist manchmal hilfreich, einer gefälschten Klasse Features hinzuzufügen, die das Testen vereinfachen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-277">It’s sometimes useful to add features to a fake class that facilitate testing.</span></span> <span data-ttu-id="b2c70-278">In diesem Fall ist es leicht zu beobachten, ob der Code einen Commit für eine Arbeitseinheit durchführt, indem er die Eigenschaft "komprimiert" überprüft.</span><span class="sxs-lookup"><span data-stu-id="b2c70-278">In this case it is easy to observe if code commits a unit of work by checking the Commited property.</span></span>

<span data-ttu-id="b2c70-279">Wir benötigen auch eine gefälschte IObjectSet-&lt;t-&gt;, um Employee-und Timecard-Objekte im Arbeitsspeicher zu speichern.</span><span class="sxs-lookup"><span data-stu-id="b2c70-279">We’ll also need a fake IObjectSet&lt;T&gt; to hold Employee and TimeCard objects in memory.</span></span> <span data-ttu-id="b2c70-280">Wir können eine einzelne Implementierung mit Generika bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-280">We can provide a single implementation using generics.</span></span>

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

<span data-ttu-id="b2c70-281">Bei diesem Test wird der größte Teil der Arbeit an ein zugrunde liegendes HashSet&lt;t-&gt; Objekt delegiert.</span><span class="sxs-lookup"><span data-stu-id="b2c70-281">This test double delegates most of its work to an underlying HashSet&lt;T&gt; object.</span></span> <span data-ttu-id="b2c70-282">Beachten Sie, dass die IObjectSet&lt;t-&gt; eine generische Einschränkung erfordert, die t als Klasse (einen Verweistyp) erzwingt. Außerdem erzwingt wir, dass Sie iquervable&lt;t&gt;implementieren.</span><span class="sxs-lookup"><span data-stu-id="b2c70-282">Note that IObjectSet&lt;T&gt; requires a generic constraint enforcing T as a class (a reference type), and also forces us to implement IQueryable&lt;T&gt;.</span></span> <span data-ttu-id="b2c70-283">Es ist einfach, eine in-Memory-Auflistung als iquerable&lt;t&gt; zu verwenden, indem der standardmäßige LINQ-Operator verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="b2c70-283">It is easy to make an in-memory collection appear as an IQueryable&lt;T&gt; using the standard LINQ operator AsQueryable.</span></span>

### <a name="the-tests"></a><span data-ttu-id="b2c70-284">Die Tests</span><span class="sxs-lookup"><span data-stu-id="b2c70-284">The Tests</span></span>

<span data-ttu-id="b2c70-285">Bei herkömmlichen Unittests wird eine einzelne Testklasse verwendet, um alle Tests für alle Aktionen in einem einzelnen MVC-Controller aufzunehmen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-285">Traditional unit tests will use a single test class to hold all of the tests for all of the actions in a single MVC controller.</span></span> <span data-ttu-id="b2c70-286">Wir können diese Tests oder eine beliebige Art von Komponenten Test mithilfe der in Arbeitsspeicher-Fakes, die wir erstellt haben, schreiben.</span><span class="sxs-lookup"><span data-stu-id="b2c70-286">We can write these tests, or any type of unit test, using the in memory fakes we’ve built.</span></span> <span data-ttu-id="b2c70-287">In diesem Artikel vermeiden wir jedoch die monolithische Test Klassen Herangehensweise und Gruppieren unsere Tests, um sich auf eine bestimmte Funktionalität zu konzentrieren.</span><span class="sxs-lookup"><span data-stu-id="b2c70-287">However, for this article we will avoid the monolithic test class approach and instead group our tests to focus on a specific piece of functionality.</span></span><span data-ttu-id="b2c70-288">  Beispielsweise könnte "Create New Employee" die Funktionalität sein, die wir testen möchten. Daher verwenden wir eine einzelne Testklasse, um die einzige Controller Aktion zu überprüfen, die für die Erstellung eines neuen Mitarbeiters zuständig ist.</span><span class="sxs-lookup"><span data-stu-id="b2c70-288">  For example, “create new employee” might be the functionality we want to test, so we will use a single test class to verify the single controller action responsible for creating a new employee.</span></span>

<span data-ttu-id="b2c70-289">Es gibt einige gängige Setup Codes, die wir für alle diese differenzierten Test Klassen benötigen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-289">There is some common setup code we need for all these fine grained test classes.</span></span> <span data-ttu-id="b2c70-290">Beispielsweise müssen wir immer unsere in-Memory-Repository und gefälschte Arbeitseinheit erstellen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-290">For example, we always need to create our in-memory repositories and fake unit of work.</span></span> <span data-ttu-id="b2c70-291">Außerdem benötigen wir eine Instanz des Employee-Controllers, bei der die gefälschte Arbeitseinheit eingefügt wurde.</span><span class="sxs-lookup"><span data-stu-id="b2c70-291">We also need an instance of the employee controller with the fake unit of work injected.</span></span> <span data-ttu-id="b2c70-292">Wir geben diesen allgemeinen Setup Code über Test Klassen hinweg mithilfe einer Basisklasse frei.</span><span class="sxs-lookup"><span data-stu-id="b2c70-292">We’ll share this common setup code across test classes by using a base class.</span></span>

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

<span data-ttu-id="b2c70-293">Die "Objekt-Mutter", die in der Basisklasse verwendet wird, ist ein gängiges Muster zum Erstellen von Testdaten.</span><span class="sxs-lookup"><span data-stu-id="b2c70-293">The “object mother” we use in the base class is one common pattern for creating test data.</span></span> <span data-ttu-id="b2c70-294">Eine Objekt-Mutter enthält Factorymethoden zum Instanziieren von Test Entitäten für die Verwendung in mehreren Test Vorrichtungen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-294">An object mother contains factory methods to instantiate test entities for use across multiple test fixtures.</span></span>

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

<span data-ttu-id="b2c70-295">Wir können "Mitarbeiter Controller TestBase" als Basisklasse für eine Reihe von Test Vorrichtungen verwenden (siehe Abbildung 3).</span><span class="sxs-lookup"><span data-stu-id="b2c70-295">We can use the EmployeeControllerTestBase as the base class for a number of test fixtures (see figure 3).</span></span> <span data-ttu-id="b2c70-296">Jede Test Fixierung testet eine bestimmte Controller Aktion.</span><span class="sxs-lookup"><span data-stu-id="b2c70-296">Each test fixture will test a specific controller action.</span></span> <span data-ttu-id="b2c70-297">Beispielsweise konzentriert sich eine Test Fixierung auf das Testen der Erstellungs Aktion, die während einer HTTP GET-Anforderung verwendet wird (um die Ansicht zum Erstellen eines Mitarbeiters anzuzeigen), und eine andere Fixierung konzentriert sich auf die CREATE-Aktion, die in einer HTTP POST-Anforderung verwendet wird (um die vom Benutzer, der einen Mitarbeiter erstellen soll.</span><span class="sxs-lookup"><span data-stu-id="b2c70-297">For example, one test fixture will focus on testing the Create action used during an HTTP GET request (to display the view for creating an employee), and a different fixture will focus on the Create action used in an HTTP POST request (to take information submitted by the user to create an employee).</span></span> <span data-ttu-id="b2c70-298">Jede abgeleitete Klasse ist nur für das Setup verantwortlich, das in Ihrem speziellen Kontext benötigt wird, und um die Assertionen bereitzustellen, die zum Überprüfen der Ergebnisse für den jeweiligen Test Kontext erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="b2c70-298">Each derived class is only responsible for the setup needed in its specific context, and to provide the assertions needed to verify the outcomes for its specific test context.</span></span>

![EF-test_03](~/ef6/media/eftest-03.png)

<span data-ttu-id="b2c70-300">**Abbildung 3**</span><span class="sxs-lookup"><span data-stu-id="b2c70-300">**Figure 3**</span></span>

<span data-ttu-id="b2c70-301">Die hier dargestellten Benennungs Konventionen und teststile sind für testbaren Code nicht erforderlich – es handelt sich nur um einen Ansatz.</span><span class="sxs-lookup"><span data-stu-id="b2c70-301">The naming convention and test style presented here isn’t required for testable code – it’s just one approach.</span></span> <span data-ttu-id="b2c70-302">Abbildung 4 zeigt die Tests, die im Jet Brains reschärfere Test Runner-Plug-in für Visual Studio 2010 ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="b2c70-302">Figure 4 shows the tests running in the Jet Brains Resharper test runner plugin for Visual Studio 2010.</span></span>

![EF-test_04](~/ef6/media/eftest-04.png)

<span data-ttu-id="b2c70-304">**Abbildung 4**</span><span class="sxs-lookup"><span data-stu-id="b2c70-304">**Figure 4**</span></span>

<span data-ttu-id="b2c70-305">Mit einer Basisklasse zur Handhabung des freigegebenen Setup Codes sind die Komponententests für die einzelnen Controller Aktionen klein und leicht zu schreiben.</span><span class="sxs-lookup"><span data-stu-id="b2c70-305">With a base class to handle the shared setup code, the unit tests for each controller action are small and easy to write.</span></span> <span data-ttu-id="b2c70-306">Die Tests werden schnell ausgeführt (da wir in-Memory-Vorgänge durchführen) und sollten aufgrund von nicht verknüpften Infrastrukturen oder umweltbezogenen Belangen fehlschlagen (da wir die zu testende Einheit isoliert haben).</span><span class="sxs-lookup"><span data-stu-id="b2c70-306">The tests will execute quickly (since we are performing in-memory operations), and shouldn’t fail because of unrelated infrastructure or environmental concerns (because we’ve isolated the unit under test).</span></span>

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

<span data-ttu-id="b2c70-307">In diesen Tests führt die Basisklasse den größten Teil der Einrichtung aus.</span><span class="sxs-lookup"><span data-stu-id="b2c70-307">In these tests, the base class does most of the setup work.</span></span> <span data-ttu-id="b2c70-308">Beachten Sie, dass der Basisklassenkonstruktor das in-Memory-Repository, eine gefälschte Arbeitseinheit und eine Instanz der Klasse Mitarbeiter Controller erstellt.</span><span class="sxs-lookup"><span data-stu-id="b2c70-308">Remember the base class constructor creates the in-memory repository, a fake unit of work, and an instance of the EmployeeController class.</span></span> <span data-ttu-id="b2c70-309">Die Testklasse wird von dieser Basisklasse abgeleitet und konzentriert sich auf die Besonderheiten beim Testen der Create-Methode.</span><span class="sxs-lookup"><span data-stu-id="b2c70-309">The test class derives from this base class and focuses on the specifics of testing the Create method.</span></span> <span data-ttu-id="b2c70-310">In diesem Fall werden die Besonderheiten in den Schritten "anordnen, Act und Assert" angezeigt, die Sie bei jedem Komponenten Testverfahren sehen:</span><span class="sxs-lookup"><span data-stu-id="b2c70-310">In this case the specifics boil down to the “arrange, act, and assert” steps you’ll see in any unit testing procedure:</span></span>

-   <span data-ttu-id="b2c70-311">Erstellen Sie ein netwemployee-Objekt, um eingehende Daten zu simulieren.</span><span class="sxs-lookup"><span data-stu-id="b2c70-311">Create a newEmployee object to simulate incoming data.</span></span>
-   <span data-ttu-id="b2c70-312">Rufen Sie die CREATE-Aktion von Mitarbeiter Controller auf, und übergeben Sie die Datei "netwemployee".</span><span class="sxs-lookup"><span data-stu-id="b2c70-312">Invoke the Create action of the EmployeeController and pass in the newEmployee.</span></span>
-   <span data-ttu-id="b2c70-313">Überprüfen Sie, ob die CREATE-Aktion die erwarteten Ergebnisse erzeugt (der Mitarbeiter wird im Repository angezeigt).</span><span class="sxs-lookup"><span data-stu-id="b2c70-313">Verify the Create action produces the expected results (the employee appears in the repository).</span></span>

<span data-ttu-id="b2c70-314">Wir haben die Möglichkeit, alle Mitarbeiter zu testen, die wir erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="b2c70-314">What we’ve built allows us to test any of the EmployeeController actions.</span></span> <span data-ttu-id="b2c70-315">Wenn wir z. b. Tests für die Index Aktion des Employee-Controllers schreiben, können wir von der Test Basisklasse erben, um das gleiche Basis Setup für unsere Tests einzurichten.</span><span class="sxs-lookup"><span data-stu-id="b2c70-315">For example, when we write tests for the Index action of the Employee controller we can inherit from the test base class to establish the same base setup for our tests.</span></span> <span data-ttu-id="b2c70-316">Erneut erstellt die Basisklasse das in-Memory-Repository, die gefälschte Arbeitseinheit und eine Instanz von Mitarbeiter Controller.</span><span class="sxs-lookup"><span data-stu-id="b2c70-316">Again the base class will create the in-memory repository, the fake unit of work, and an instance of the EmployeeController.</span></span> <span data-ttu-id="b2c70-317">Die Tests für die Index Aktion müssen sich nur auf das Aufrufen der Index Aktion und das Testen der Qualitäten des Modells konzentrieren, das von der Aktion zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="b2c70-317">The tests for the Index action only need to focus on invoking the Index action and testing the qualities of the model the action returns.</span></span>

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

<span data-ttu-id="b2c70-318">Die Tests, die wir mit in-Memory-Fakes erstellen, sind darauf ausgerichtet, den *Zustand* der Software zu testen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-318">The tests we are creating with in-memory fakes are oriented towards testing the *state* of the software.</span></span> <span data-ttu-id="b2c70-319">Wenn Sie z. b. die Erstellungs Aktion testen möchten, soll der Status des Repository nach der Ausführung der CREATE-Aktion überprüft werden – enthält das Repository den neuen Mitarbeiter?</span><span class="sxs-lookup"><span data-stu-id="b2c70-319">For example, when testing the Create action we want to inspect the state of the repository after the create action executes – does the repository hold the new employee?</span></span>

``` csharp
    [TestMethod]
    public void ShouldAddNewEmployeeToRepository() {
        _controller.Create(_newEmployee);
        Assert.IsTrue(_repository.Contains(_newEmployee));
    }
```

<span data-ttu-id="b2c70-320">Später befassen wir uns mit Interaktions basierten Tests.</span><span class="sxs-lookup"><span data-stu-id="b2c70-320">Later we’ll look at interaction based testing.</span></span> <span data-ttu-id="b2c70-321">Durch Interaktions basierte Tests wird gefragt, ob der zu testende Code die richtigen Methoden für unsere Objekte aufgerufen hat und die richtigen Parameter übergebenen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-321">Interaction based testing will ask if the code under test invoked the proper methods on our objects and passed the correct parameters.</span></span> <span data-ttu-id="b2c70-322">Im folgenden wird ein weiteres Entwurfsmuster behandelt – das verzögerte Laden.</span><span class="sxs-lookup"><span data-stu-id="b2c70-322">For now we’ll move on the cover another design pattern – the lazy load.</span></span>

## <a name="eager-loading-and-lazy-loading"></a><span data-ttu-id="b2c70-323">Unverzügliches Laden und Lazy Load</span><span class="sxs-lookup"><span data-stu-id="b2c70-323">Eager Loading and Lazy Loading</span></span>

<span data-ttu-id="b2c70-324">An einem bestimmten Punkt in der ASP.NET MVC-Webanwendung möchten wir möglicherweise die Informationen eines Mitarbeiters anzeigen und die zugeordneten Zeitkarten des Mitarbeiters einschließen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-324">At some point in the ASP.NET  MVC web application we might wish to display an employee’s information and include the employee’s associated time cards.</span></span> <span data-ttu-id="b2c70-325">Beispielsweise kann eine Zeitkarten-Übersichts Anzeige angezeigt werden, in der der Name des Mitarbeiters und die Gesamtzahl der Zeitkarten im System angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="b2c70-325">For example, we might have a time card summary display that shows the employee’s name and the total number of time cards in the system.</span></span> <span data-ttu-id="b2c70-326">Es gibt mehrere Ansätze, die wir zur Implementierung dieser Funktion verwenden können.</span><span class="sxs-lookup"><span data-stu-id="b2c70-326">There are several approaches we can take to implement this feature.</span></span>

### <a name="projection"></a><span data-ttu-id="b2c70-327">Projection</span><span class="sxs-lookup"><span data-stu-id="b2c70-327">Projection</span></span>

<span data-ttu-id="b2c70-328">Ein einfacher Ansatz zum Erstellen der Zusammenfassung besteht darin, ein Modell zu erstellen, das für die Informationen vorgesehen ist, die in der Ansicht angezeigt werden sollen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-328">One easy approach to create the summary is to construct a model dedicated to the information we want to display in the view.</span></span> <span data-ttu-id="b2c70-329">In diesem Szenario könnte das Modell wie folgt aussehen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-329">In this scenario the model might look like the following.</span></span>

``` csharp
    public class EmployeeSummaryViewModel {
        public string Name { get; set; }
        public int TotalTimeCards { get; set; }
    }
```

<span data-ttu-id="b2c70-330">Beachten Sie, dass es sich bei "Mitarbeiter Name" nicht um eine Entität handelt – das heißt, dass es sich nicht um etwas handelt, das in der Datenbank persistent gespeichert werden soll.</span><span class="sxs-lookup"><span data-stu-id="b2c70-330">Note that the EmployeeSummaryViewModel is not an entity – in other words it is not something we want to persist in the database.</span></span> <span data-ttu-id="b2c70-331">Diese Klasse wird nur verwendet, um Daten in einer stark typisierten Weise in die Ansicht zu mischen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-331">We are only going to use this class to shuffle data into the view in a strongly typed manner.</span></span> <span data-ttu-id="b2c70-332">Das Ansichts Modell ist wie ein Datenübertragungs Objekt (Data Transfer Object, dto), da es kein Verhalten (keine Methoden) – nur Eigenschaften enthält.</span><span class="sxs-lookup"><span data-stu-id="b2c70-332">The view model is like a data transfer object (DTO) because it contains no behavior (no methods) – only properties.</span></span> <span data-ttu-id="b2c70-333">Die Eigenschaften enthalten die Daten, die wir verschieben müssen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-333">The properties will hold the data we need to move.</span></span> <span data-ttu-id="b2c70-334">Es ist einfach, dieses Ansichts Modell mit dem Standard Projektions Operator von LINQ – dem Select-Operator zu instanziieren.</span><span class="sxs-lookup"><span data-stu-id="b2c70-334">It is easy to instantiate this view model using LINQ’s standard projection operator – the Select operator.</span></span>

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

<span data-ttu-id="b2c70-335">Der obige Code enthält zwei wichtige Features.</span><span class="sxs-lookup"><span data-stu-id="b2c70-335">There are two notable features to the above code.</span></span> <span data-ttu-id="b2c70-336">Zuerst – der Code ist einfach zu testen, da er immer noch leicht zu beobachten und zu isolieren ist.</span><span class="sxs-lookup"><span data-stu-id="b2c70-336">First – the code is easy to test because it is still easy to observe and isolate.</span></span> <span data-ttu-id="b2c70-337">Der Select-Operator funktioniert genauso gut wie bei den in-Memory-Fakes, wie es bei der realen Arbeitseinheit der Fall ist.</span><span class="sxs-lookup"><span data-stu-id="b2c70-337">The Select operator works just as well against our in-memory fakes as it does against the real unit of work.</span></span>

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

<span data-ttu-id="b2c70-338">Das zweite wichtige Feature ist, wie der Code es EF4 ermöglicht, eine einzelne, effiziente Abfrage zu generieren, um Mitarbeiter-und Zeitkarten Informationen zusammenzustellen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-338">The second notable feature is how the code allows EF4 to generate a single, efficient query to assemble employee and time card information together.</span></span> <span data-ttu-id="b2c70-339">Wir haben Mitarbeiter Informationen und Zeitkarten Informationen in dasselbe Objekt geladen, ohne dass spezielle APIs verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="b2c70-339">We’ve loaded employee information and time card information into the same object without using any special APIs.</span></span> <span data-ttu-id="b2c70-340">Der Code hat lediglich die erforderlichen Informationen zum Verwenden von standardmäßigen LINQ-Operatoren ausgedrückt, die für in-Memory-Datenquellen und Remote Datenquellen geeignet sind.</span><span class="sxs-lookup"><span data-stu-id="b2c70-340">The code merely expressed the information it requires using standard LINQ operators that work against in-memory data sources as well as remote data sources.</span></span> <span data-ttu-id="b2c70-341">EF4 konnte die von der LINQ-Abfrage und dem C-\# Compiler generierten Ausdrucks Baumstrukturen in eine einzelne und effiziente t-SQL-Abfrage übersetzen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-341">EF4 was able to translate the expression trees generated by the LINQ query and C\# compiler into a single and efficient T-SQL query.</span></span>

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
         )  AS [Project1]
    )  AS [Limit1]
```

<span data-ttu-id="b2c70-342">In anderen Zeiten möchten wir nicht mit einem Ansichts Modell oder DTO-Objekt arbeiten, sondern mit echten Entitäten.</span><span class="sxs-lookup"><span data-stu-id="b2c70-342">There are other times when we don’t want to work with a view model or DTO object, but with real entities.</span></span> <span data-ttu-id="b2c70-343">Wenn wir wissen, dass wir einen Mitarbeiter *und* die Zeitkarten des Mitarbeiters benötigen, können wir die verknüpften Daten auf unaufdringliche und effiziente Weise laden.</span><span class="sxs-lookup"><span data-stu-id="b2c70-343">When we know we need an employee *and* the employee’s time cards, we can eagerly load the related data in an unobtrusive and efficient manner.</span></span>

### <a name="explicit-eager-loading"></a><span data-ttu-id="b2c70-344">Explizites Laden</span><span class="sxs-lookup"><span data-stu-id="b2c70-344">Explicit Eager Loading</span></span>

<span data-ttu-id="b2c70-345">Wenn Sie zusammen gehörige Entitäts Informationen laden möchten, benötigen wir einige Mechanismen für die Geschäftslogik (oder in diesem Szenario, Controller Aktions Logik), um den Wunsch zum Repository auszudrücken.</span><span class="sxs-lookup"><span data-stu-id="b2c70-345">When we want to eagerly load related entity information we need some mechanism for business logic (or in this scenario, controller action logic) to express its desire to the repository.</span></span> <span data-ttu-id="b2c70-346">Die EF4 ObjectQuery&lt;t&gt; Klasse definiert eine include-Methode, um die verknüpften Objekte anzugeben, die während einer Abfrage abgerufen werden sollen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-346">The EF4 ObjectQuery&lt;T&gt; class defines an Include method to specify the related objects to retrieve during a query.</span></span> <span data-ttu-id="b2c70-347">Beachten Sie, dass EF4 ObjectContext Entitäten über die konkrete ObjectSet-&lt;t&gt; Klasse verfügbar macht, die von ObjectQuery&lt;t&gt;erbt.</span><span class="sxs-lookup"><span data-stu-id="b2c70-347">Remember the EF4 ObjectContext exposes entities via the concrete ObjectSet&lt;T&gt; class which inherits from ObjectQuery&lt;T&gt;.</span></span><span data-ttu-id="b2c70-348">  Wenn wir ObjectSet&lt;t&gt; Verweise in unserer Controller Aktion verwenden, könnten wir den folgenden Code schreiben, um die Zeit Karteninformationen für die einzelnen Mitarbeiter zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="b2c70-348">  If we were using ObjectSet&lt;T&gt; references in our controller action we could write the following code to specify an eager load of time card information for each employee.</span></span>

``` csharp
    _employees.Include("TimeCards")
              .Where(e => e.HireDate.Year > 2009);
```

<span data-ttu-id="b2c70-349">Da wir jedoch versuchen, den Code testfähig zu halten, machen wir ObjectSet&lt;t&gt; nicht von außerhalb der realen Arbeitseinheiten Klasse verfügbar.</span><span class="sxs-lookup"><span data-stu-id="b2c70-349">However, since we are trying to keep our code testable we are not exposing ObjectSet&lt;T&gt; from outside the real unit of work class.</span></span> <span data-ttu-id="b2c70-350">Stattdessen wird die IObjectSet-&lt;t-&gt; Schnittstelle verwendet, die einfacher zu fälschen ist, aber IObjectSet&lt;t&gt; definiert keine Include-Methode.</span><span class="sxs-lookup"><span data-stu-id="b2c70-350">Instead, we rely on the IObjectSet&lt;T&gt; interface which is easier to fake, but IObjectSet&lt;T&gt; does not define an Include method.</span></span> <span data-ttu-id="b2c70-351">Die Schönheit von LINQ besteht darin, dass wir unseren eigenen Include-Operator erstellen können.</span><span class="sxs-lookup"><span data-stu-id="b2c70-351">The beauty of LINQ is that we can create our own Include operator.</span></span>

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

<span data-ttu-id="b2c70-352">Beachten Sie, dass dieser Include-Operator als Erweiterungsmethode für iquerable&lt;t&gt; anstelle von IObjectSet&lt;t&gt;definiert ist.</span><span class="sxs-lookup"><span data-stu-id="b2c70-352">Notice this Include operator is defined as an extension method for IQueryable&lt;T&gt; instead of IObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="b2c70-353">Dies ermöglicht uns die Verwendung der-Methode mit einer breiteren Palette möglicher Typen, einschließlich iquerable&lt;t&gt;, IObjectSet&lt;t&gt;, ObjectQuery&lt;t&gt;und ObjectSet&lt;t&gt;.</span><span class="sxs-lookup"><span data-stu-id="b2c70-353">This gives us the ability to use the method with a wider range of possible types, including IQueryable&lt;T&gt;, IObjectSet&lt;T&gt;, ObjectQuery&lt;T&gt;, and ObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="b2c70-354">Wenn die zugrunde liegende Sequenz keine echte EF4 ObjectQuery&lt;t&gt;ist, wird keine Beschädigung durchgeführt, und der Include-Operator ist kein op.</span><span class="sxs-lookup"><span data-stu-id="b2c70-354">In the event the underlying sequence is not a genuine EF4 ObjectQuery&lt;T&gt;, then there is no harm done and the Include operator is a no-op.</span></span> <span data-ttu-id="b2c70-355">Wenn die zugrunde liegende Sequenz eine ObjectQuery-&lt;t&gt; (oder von ObjectQuery&lt;t&gt;) abgeleitet *ist* , wird für EF4 die Anforderung zusätzlicher Daten angezeigt und die richtige SQL-Abfrage formuliert.</span><span class="sxs-lookup"><span data-stu-id="b2c70-355">If the underlying sequence *is* an ObjectQuery&lt;T&gt; (or derived from ObjectQuery&lt;T&gt;), then EF4 will see our requirement for additional data and formulate the proper SQL query.</span></span>

<span data-ttu-id="b2c70-356">Mit diesem neuen Operator können wir explizit eine Zeitkarten Informationen aus dem Repository anfordern.</span><span class="sxs-lookup"><span data-stu-id="b2c70-356">With this new operator in place we can explicitly request an eager load of time card information from the repository.</span></span>

``` csharp
    public ViewResult Index() {
        var model = _unitOfWork.Employees
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

<span data-ttu-id="b2c70-357">Beim Ausführen für einen echten ObjectContext erzeugt der Code die folgende einzelne Abfrage.</span><span class="sxs-lookup"><span data-stu-id="b2c70-357">When run against a real ObjectContext, the code produces the following single query.</span></span> <span data-ttu-id="b2c70-358">Die Abfrage sammelt in einer einzigen Fahrt genügend Informationen aus der Datenbank, um die Mitarbeiter Objekte zu materialisieren und ihre TIMECARDS-Eigenschaft vollständig aufzufüllen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-358">The query gathers enough information from the database in one trip to materialize the employee objects and fully populate their TimeCards property.</span></span>

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
         FROM  [dbo].[Employees] AS [Extent1]
         LEFT OUTER JOIN [dbo].[TimeCards] AS [Extent2] ON [Extent1].[Id] = [Extent2].[EmployeeTimeCard_TimeCard_Id]
    )  AS [Project1]
    ORDER BY [Project1].[HireDate] ASC,
             [Project1].[Id] ASC, [Project1].[C1] ASC
```

<span data-ttu-id="b2c70-359">Die großartige Nachricht ist, dass der Code innerhalb der Aktionsmethode vollständig getestet werden kann.</span><span class="sxs-lookup"><span data-stu-id="b2c70-359">The great news is the code inside the action method remains fully testable.</span></span> <span data-ttu-id="b2c70-360">Wir müssen keine zusätzlichen Features für unsere Fakes bereitstellen, um den Include-Operator zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-360">We don’t need to provide any additional features for our fakes to support the Include operator.</span></span> <span data-ttu-id="b2c70-361">Die schlechte Nachricht ist, dass wir den Include-Operator in dem Code verwenden mussten, der persistent bleiben sollte.</span><span class="sxs-lookup"><span data-stu-id="b2c70-361">The bad news is we had to use the Include operator inside of the code we wanted to keep persistence ignorant.</span></span> <span data-ttu-id="b2c70-362">Dies ist ein gutes Beispiel für die Art von vor-und Nachteile, die Sie bei der Erstellung von Test barem Code auswerten müssen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-362">This is a prime example of the type of tradeoffs you’ll need to evaluate when building testable code.</span></span> <span data-ttu-id="b2c70-363">In manchen Zeiten ist es erforderlich, dass Persistenzprobleme außerhalb der Repository-Abstraktion verbleibt, um Leistungsziele zu erreichen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-363">There are times when you need to let persistence concerns leak outside the repository abstraction to meet performance goals.</span></span>

<span data-ttu-id="b2c70-364">Die Alternative zum Eager Loading ist Lazy Loading.</span><span class="sxs-lookup"><span data-stu-id="b2c70-364">The alternative to eager loading is lazy loading.</span></span> <span data-ttu-id="b2c70-365">Lazy Load bedeutet, dass der Geschäfts Code *nicht* erforderlich ist, um die Anforderung für die zugehörigen Daten explizit anzukündigen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-365">Lazy loading means we do *not* need our business code to explicitly announce the requirement for associated data.</span></span> <span data-ttu-id="b2c70-366">Stattdessen werden unsere Entitäten in der Anwendung verwendet, und wenn zusätzliche Daten benötigt werden Entity Framework werden die Daten bei Bedarf geladen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-366">Instead, we use our entities in the application and if additional data is needed Entity Framework will load the data on demand.</span></span>

### <a name="lazy-loading"></a><span data-ttu-id="b2c70-367">Lazy Loading</span><span class="sxs-lookup"><span data-stu-id="b2c70-367">Lazy Loading</span></span>

<span data-ttu-id="b2c70-368">Es ist leicht vorstellbar, ein Szenario zu finden, in dem wir nicht wissen, welche Daten eine Geschäftslogik benötigt.</span><span class="sxs-lookup"><span data-stu-id="b2c70-368">It’s easy to imagine a scenario where we don’t know what data a piece of business logic will need.</span></span> <span data-ttu-id="b2c70-369">Möglicherweise wissen Sie, dass die Logik ein Employee-Objekt benötigt, aber wir können in verschiedene Ausführungs Pfade verzweigen, in denen einige dieser Pfade Zeitkarten Informationen vom Mitarbeiter erfordern, und andere nicht.</span><span class="sxs-lookup"><span data-stu-id="b2c70-369">We might know the logic needs an employee object, but we may branch into different execution paths where some of those paths require time card information from the employee, and some do not.</span></span> <span data-ttu-id="b2c70-370">Szenarios wie diese eignen sich hervorragend für implizite Lazy Loading, da Daten nach Bedarf nach Bedarf angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="b2c70-370">Scenarios like this are perfect for implicit lazy loading because data magically appears on an as-needed basis.</span></span>

<span data-ttu-id="b2c70-371">Lazy Load, auch als verzögertes Laden bezeichnet, erfüllt einige Anforderungen an unsere Entitäts Objekte.</span><span class="sxs-lookup"><span data-stu-id="b2c70-371">Lazy loading, also known as deferred loading, does place some requirements on our entity objects.</span></span> <span data-ttu-id="b2c70-372">POCOS mit der wahren Persistenz von Persistenz würde keinerlei Anforderungen von der Persistenzebene genügen, aber es ist praktisch unmöglich, eine echte Persistenz zu gewährleisten.</span><span class="sxs-lookup"><span data-stu-id="b2c70-372">POCOs with true persistence ignorance would not face any requirements from the persistence layer, but true persistence ignorance is practically impossible to achieve.</span></span><span data-ttu-id="b2c70-373">  Stattdessen messen wir die Persistenz der Persistenz in relativen Grad.</span><span class="sxs-lookup"><span data-stu-id="b2c70-373">  Instead we measure persistence ignorance in relative degrees.</span></span> <span data-ttu-id="b2c70-374">Es wäre bedauerlich, wenn wir von einer persistenzorientierten Basisklasse erben oder eine spezialisierte Sammlung verwenden müssen, um Lazy Loading in POCOS zu erzielen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-374">It would be unfortunate if we needed to inherit from a persistence oriented base class or use a specialized collection to achieve lazy loading in POCOs.</span></span> <span data-ttu-id="b2c70-375">Glücklicherweise hat EF4 eine weniger eindringliche Lösung.</span><span class="sxs-lookup"><span data-stu-id="b2c70-375">Fortunately, EF4 has a less intrusive solution.</span></span>

### <a name="virtually-undetectable"></a><span data-ttu-id="b2c70-376">Nahezu nicht erkennbar</span><span class="sxs-lookup"><span data-stu-id="b2c70-376">Virtually Undetectable</span></span>

<span data-ttu-id="b2c70-377">Wenn poco-Objekte verwendet werden, kann EF4 dynamisch Lauf Zeit Proxys für Entitäten generieren.</span><span class="sxs-lookup"><span data-stu-id="b2c70-377">When using POCO objects, EF4 can dynamically generate runtime proxies for entities.</span></span> <span data-ttu-id="b2c70-378">Diese Proxys wrappen die materialisierten POCOS unsichtbar und bieten zusätzliche Dienste, indem jeder Get-und Set-Vorgang der einzelnen Eigenschaften abgefangen wird, um zusätzliche Aufgaben auszuführen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-378">These proxies invisibly wrap the materialized POCOs and provide additional services by intercepting each property get and set operation to perform additional work.</span></span> <span data-ttu-id="b2c70-379">Ein solcher Dienst ist das Lazy Loading Feature, nach dem wir suchen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-379">One such service is the lazy loading feature we are looking for.</span></span> <span data-ttu-id="b2c70-380">Ein anderer Dienst ist ein effizienter Mechanismus zur Änderungs Nachverfolgung, der aufzeichnen kann, wann das Programm die Eigenschaftswerte einer Entität ändert.</span><span class="sxs-lookup"><span data-stu-id="b2c70-380">Another service is an efficient change tracking mechanism which can record when the program changes the property values of an entity.</span></span> <span data-ttu-id="b2c70-381">Die Liste der Änderungen wird von ObjectContext während der SaveChanges-Methode verwendet, um alle geänderten Entitäten mithilfe von Update-Befehlen beizubehalten.</span><span class="sxs-lookup"><span data-stu-id="b2c70-381">The list of changes is used by the ObjectContext during the SaveChanges method to persist any modified entities using UPDATE commands.</span></span>

<span data-ttu-id="b2c70-382">Damit diese Proxys funktionieren, benötigen Sie jedoch eine Möglichkeit, mit den Get-und Set-Vorgängen für eine Entität zu verbinden, und die Proxys erreichen dieses Ziel durch Überschreiben von virtuellen Membern.</span><span class="sxs-lookup"><span data-stu-id="b2c70-382">For these proxies to work, however, they need a way to hook into property get and set operations on an entity, and the proxies achieve this goal by overriding virtual members.</span></span> <span data-ttu-id="b2c70-383">Wenn wir also implizites Lazy Loading und eine effiziente Änderungs Nachverfolgung wünschen, müssen wir zu den poco-Klassendefinitionen zurückkehren und Eigenschaften als virtuell markieren.</span><span class="sxs-lookup"><span data-stu-id="b2c70-383">Thus, if we want to have implicit lazy loading and efficient change tracking we need to go back to our POCO class definitions and mark properties as virtual.</span></span>

``` csharp
    public class Employee {
        public virtual int Id { get; set; }
        public virtual string Name { get; set; }
        public virtual DateTime HireDate { get; set; }
        public virtual ICollection<TimeCard> TimeCards { get; set; }
    }
```

<span data-ttu-id="b2c70-384">Wir können weiterhin sagen, dass die Mitarbeiter Entität größtenteils persistent ist.</span><span class="sxs-lookup"><span data-stu-id="b2c70-384">We can still say the Employee entity is mostly persistence ignorant.</span></span> <span data-ttu-id="b2c70-385">Die einzige Anforderung besteht darin, virtuelle Member zu verwenden, und dies hat keine Auswirkung auf die Testability des Codes.</span><span class="sxs-lookup"><span data-stu-id="b2c70-385">The only requirement is to use virtual members and this does not impact the testability of the code.</span></span> <span data-ttu-id="b2c70-386">Wir müssen nicht von einer speziellen Basisklasse ableiten oder sogar eine spezielle Sammlung verwenden, die für die Lazy Loading reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="b2c70-386">We don’t need to derive from any special base class, or even use a special collection dedicated to lazy loading.</span></span> <span data-ttu-id="b2c70-387">Wie der Code zeigt, steht jede Klasse, die ICollection-&lt;t&gt; implementiert, zum Speichern verwandter Entitäten zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="b2c70-387">As the code demonstrates, any class implementing ICollection&lt;T&gt; is available to hold related entities.</span></span>

<span data-ttu-id="b2c70-388">Es gibt auch eine geringfügige Änderung, die wir in unserer Arbeitseinheit vornehmen müssen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-388">There is also one minor change we need to make inside our unit of work.</span></span> <span data-ttu-id="b2c70-389">Lazy *Load ist Standard* mäßig deaktiviert, wenn Sie direkt mit einem ObjectContext-Objekt arbeiten.</span><span class="sxs-lookup"><span data-stu-id="b2c70-389">Lazy loading is *off* by default when working directly with an ObjectContext object.</span></span> <span data-ttu-id="b2c70-390">Es gibt eine Eigenschaft, die für die ContextOptions-Eigenschaft festgelegt werden kann, um verzögertes Laden zu ermöglichen, und wir können diese Eigenschaft in unserer echten Arbeitseinheit festlegen, wenn wir Lazy Loading überall aktivieren möchten.</span><span class="sxs-lookup"><span data-stu-id="b2c70-390">There is a property we can set on the ContextOptions property to enable deferred loading, and we can set this property inside our real unit of work if we want to enable lazy loading everywhere.</span></span>

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

<span data-ttu-id="b2c70-391">Wenn implizites Lazy Loading aktiviert ist, kann der Anwendungscode einen Mitarbeiter und die zugeordneten Zeitkarten des Mitarbeiters verwenden, während der verbleibende Arbeitsaufwand, den EF zum Laden zusätzlicher Daten benötigt, nicht vollständig unbekannt ist.</span><span class="sxs-lookup"><span data-stu-id="b2c70-391">With implicit lazy loading enabled, application code can use an employee and the employee’s associated time cards while remaining blissfully unaware of the work required for EF to load the extra data.</span></span>

``` csharp
    var employee = _unitOfWork.Employees
                              .Single(e => e.Id == id);
    foreach (var card in employee.TimeCards) {
        // ...
    }
```

<span data-ttu-id="b2c70-392">Lazy Load vereinfacht das Schreiben des Anwendungs Codes, und mit dem Proxy Magic bleibt der Code vollständig prüfbar.</span><span class="sxs-lookup"><span data-stu-id="b2c70-392">Lazy loading makes the application code easier to write, and with the proxy magic the code remains completely testable.</span></span> <span data-ttu-id="b2c70-393">In-Memory-Fakes der Arbeitseinheit können bei Bedarf einfach gefälschte Entitäten mit zugeordneten Daten vorab geladen werden.</span><span class="sxs-lookup"><span data-stu-id="b2c70-393">In-memory fakes of the unit of work can simply preload fake entities with associated data when needed during a test.</span></span>

<span data-ttu-id="b2c70-394">An dieser Stelle wird das Entwickeln von Depots mithilfe von IObjectSet&lt;t&gt; und die Abstraktion von Abstraktionen, um alle Vorzeichen des persistenzframe Works auszublenden.</span><span class="sxs-lookup"><span data-stu-id="b2c70-394">At this point we’ll turn our attention from building repositories using IObjectSet&lt;T&gt; and look at abstractions to hide all signs of the persistence framework.</span></span>

## <a name="custom-repositories"></a><span data-ttu-id="b2c70-395">Benutzerdefinierte Depots</span><span class="sxs-lookup"><span data-stu-id="b2c70-395">Custom Repositories</span></span>

<span data-ttu-id="b2c70-396">Als wir zuerst das Entwurfsmuster für Arbeitseinheiten in diesem Artikel vorgestellt haben, haben wir einen Beispielcode dafür bereitgestellt, wie die Arbeitseinheit aussehen könnte.</span><span class="sxs-lookup"><span data-stu-id="b2c70-396">When we first presented the unit of work design pattern in this article we provided some sample code for what the unit of work might look like.</span></span> <span data-ttu-id="b2c70-397">Wir stellen diese ursprüngliche Idee mit dem Zeitkarten Szenario für Mitarbeiter und Mitarbeiter wieder her, mit dem wir gearbeitet haben.</span><span class="sxs-lookup"><span data-stu-id="b2c70-397">Let’s re-present this original idea using the employee and employee time card scenario we’ve been working with.</span></span>

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<TimeCard> TimeCards { get;  }
        void Commit();
    }
```

<span data-ttu-id="b2c70-398">Der Hauptunterschied zwischen dieser Arbeitseinheit und der Arbeitseinheit, die wir im letzten Abschnitt erstellt haben, besteht darin, dass diese Arbeitseinheit keine Abstraktionen aus EF4 Framework verwendet (es gibt keine IObjectSet-&lt;t&gt;).</span><span class="sxs-lookup"><span data-stu-id="b2c70-398">The primary difference between this unit of work and the unit of work we created in the last section is how this unit of work does not use any abstractions from the EF4 framework (there is no IObjectSet&lt;T&gt;).</span></span> <span data-ttu-id="b2c70-399">IObjectSet&lt;t&gt; funktioniert gut als Repository-Schnittstelle, aber die API, die Sie verfügbar macht, ist möglicherweise nicht perfekt an die Anforderungen unserer Anwendung angepasst.</span><span class="sxs-lookup"><span data-stu-id="b2c70-399">IObjectSet&lt;T&gt; works well as a repository interface, but the API it exposes might not perfectly align with our application’s needs.</span></span> <span data-ttu-id="b2c70-400">In diesem bevorstehenden Ansatz stellen wir die Depots mithilfe eines benutzerdefinierten IRepository&lt;t&gt; Abstraktion dar.</span><span class="sxs-lookup"><span data-stu-id="b2c70-400">In this upcoming approach we will represent repositories using a custom IRepository&lt;T&gt; abstraction.</span></span>

<span data-ttu-id="b2c70-401">Viele Entwickler, die auf Test gesteuerte Entwurfs-, Verhaltens gesteuerte Entwurfs-und Domänen gesteuerte Methodologien basieren, bevorzugen den Ansatz von IRepository&lt;t&gt; aus verschiedenen Gründen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-401">Many developers who follow test-driven design, behavior-driven design, and domain driven methodologies design prefer the IRepository&lt;T&gt; approach for several reasons.</span></span> <span data-ttu-id="b2c70-402">Zuerst stellt die IRepository-&lt;t-&gt; Schnittstelle eine "antibeschädigungsebene" dar.</span><span class="sxs-lookup"><span data-stu-id="b2c70-402">First, the IRepository&lt;T&gt; interface represents an “anti-corruption” layer.</span></span> <span data-ttu-id="b2c70-403">Wie von Eric Evans in seinem domänengesteuerten Entwurfs Buch beschrieben, behält eine antibeschädigungs Schicht Ihren Domänen Code von Infrastruktur-APIs, wie z. b. eine persistenzapi, fern</span><span class="sxs-lookup"><span data-stu-id="b2c70-403">As described by Eric Evans in his Domain Driven Design book an anti-corruption layer keeps your domain code away from infrastructure APIs, like a persistence API.</span></span> <span data-ttu-id="b2c70-404">Zweitens können Entwickler Methoden im Repository erstellen, die genau den Anforderungen einer Anwendung entsprechen (wie beim Schreiben von Tests erkannt).</span><span class="sxs-lookup"><span data-stu-id="b2c70-404">Secondly, developers can build methods into the repository that meet the exact needs of an application (as discovered while writing tests).</span></span> <span data-ttu-id="b2c70-405">Beispielsweise kann es vorkommen, dass wir häufig eine einzelne Entität mit einem ID-Wert suchen müssen, damit wir der Repository-Schnittstelle eine findByID-Methode hinzufügen können.</span><span class="sxs-lookup"><span data-stu-id="b2c70-405">For example, we might frequently need to locate a single entity using an ID value, so we can add a FindById method to the repository interface.</span></span><span data-ttu-id="b2c70-406">  Die IRepository-&lt;t-&gt; Definition sieht wie folgt aus.</span><span class="sxs-lookup"><span data-stu-id="b2c70-406">  Our IRepository&lt;T&gt; definition will look like the following.</span></span>

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

<span data-ttu-id="b2c70-407">Beachten Sie, dass Sie eine iquerable&lt;t&gt;-Schnittstelle verwenden, um Entitäts Sammlungen verfügbar zu machen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-407">Notice we’ll drop back to using an IQueryable&lt;T&gt; interface to expose entity collections.</span></span> <span data-ttu-id="b2c70-408">Iquerable&lt;t&gt; ermöglicht, dass LINQ-Ausdrucks Baumstrukturen in den EF4-Anbieter fließen und dem Anbieter eine ganzheitliche Ansicht der Abfrage zur Verfügung steht.</span><span class="sxs-lookup"><span data-stu-id="b2c70-408">IQueryable&lt;T&gt; allows LINQ expression trees to flow into the EF4 provider and give the provider a holistic view of the query.</span></span> <span data-ttu-id="b2c70-409">Eine zweite Option besteht darin, IEnumerable&lt;t&gt;zurückzugeben. Dies bedeutet, dass der EF4 LINQ-Anbieter nur die innerhalb des Repository erstellten Ausdrücke sehen kann.</span><span class="sxs-lookup"><span data-stu-id="b2c70-409">A second option would be to return IEnumerable&lt;T&gt;, which means the EF4 LINQ provider will only see the expressions built inside of the repository.</span></span> <span data-ttu-id="b2c70-410">Alle Gruppierungen, Reihenfolge und Projektionen, die außerhalb des Repository durchgeführt werden, werden nicht in den SQL-Befehl zusammengefasst, der an die Datenbank gesendet wird. Dies kann die Leistung beeinträchtigen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-410">Any grouping, ordering, and projection done outside of the repository will not be composed into the SQL command sent to the database, which can hurt performance.</span></span> <span data-ttu-id="b2c70-411">Andererseits wird ein Repository, das nur IEnumerable&lt;t&gt; Ergebnisse zurückgibt, nie einen neuen SQL-Befehl überraschen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-411">On the other hand, a repository returning only IEnumerable&lt;T&gt; results will never surprise you with a new SQL command.</span></span> <span data-ttu-id="b2c70-412">Beide Ansätze funktionieren, und beide Ansätze bleiben prüfbar.</span><span class="sxs-lookup"><span data-stu-id="b2c70-412">Both approaches will work, and both approaches remain testable.</span></span>

<span data-ttu-id="b2c70-413">Es ist einfach, eine einzige Implementierung der IRepository-&lt;t-&gt; Schnittstelle mithilfe von Generika und der EF4 ObjectContext-API bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-413">It’s straightforward to provide a single implementation of the IRepository&lt;T&gt; interface using generics and the EF4 ObjectContext API.</span></span>

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

<span data-ttu-id="b2c70-414">Der IRepository&lt;t-&gt; Ansatz bietet uns eine zusätzliche Kontrolle über unsere Abfragen, da ein Client eine Methode aufrufen muss, um eine Entität zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="b2c70-414">The IRepository&lt;T&gt; approach gives us some additional control over our queries because a client has to invoke a method to get to an entity.</span></span> <span data-ttu-id="b2c70-415">In der-Methode könnten wir zusätzliche Überprüfungen und LINQ-Operatoren zur Durchsetzung von Anwendungs Einschränkungen bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-415">Inside the method we could provide additional checks and LINQ operators to enforce application constraints.</span></span> <span data-ttu-id="b2c70-416">Beachten Sie, dass die Schnittstelle über zwei Einschränkungen für den generischen Typparameter verfügt.</span><span class="sxs-lookup"><span data-stu-id="b2c70-416">Notice the interface has two constraints on the generic type parameter.</span></span> <span data-ttu-id="b2c70-417">Die erste Einschränkung ist die Klasse Cons beeinträchtigen, die für ObjectSet&lt;t&gt;erforderlich ist, und die zweite Einschränkung zwingt unsere Entitäten, IEntity – eine Abstraktion zu implementieren, die für die Anwendung erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="b2c70-417">The first constraint is the class cons taint required by ObjectSet&lt;T&gt;, and the second constraint forces our entities to implement IEntity – an abstraction created for the application.</span></span> <span data-ttu-id="b2c70-418">Die IEntity-Schnittstelle erzwingt Entitäten, dass eine lesbare ID-Eigenschaft vorhanden ist, und wir können diese Eigenschaft dann in der findByID-Methode verwenden.</span><span class="sxs-lookup"><span data-stu-id="b2c70-418">The IEntity interface forces entities to have a readable Id property, and we can then use this property in the FindById method.</span></span> <span data-ttu-id="b2c70-419">IEntity ist mit folgendem Code definiert.</span><span class="sxs-lookup"><span data-stu-id="b2c70-419">IEntity is defined with the following code.</span></span>

``` csharp
    public interface IEntity {
        int Id { get; }
    }
```

<span data-ttu-id="b2c70-420">IEntity könnte als geringfügige Verletzung der Persistenz bei der Persistenz angesehen werden, da unsere Entitäten zum Implementieren dieser Schnittstelle erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="b2c70-420">IEntity could be considered a small violation of persistence ignorance since our entities are required to implement this interface.</span></span> <span data-ttu-id="b2c70-421">Denken Sie daran, dass Persistenz bei der Persistenz von Kompromisse liegt, und für viele der findByID-Funktionen wird die von der-Schnittstelle festgelegten Einschränkung</span><span class="sxs-lookup"><span data-stu-id="b2c70-421">Remember persistence ignorance is about tradeoffs, and for many the FindById functionality will outweigh the constraint imposed by the interface.</span></span> <span data-ttu-id="b2c70-422">Die-Schnittstelle hat keine Auswirkung auf die Testability.</span><span class="sxs-lookup"><span data-stu-id="b2c70-422">The interface has no impact on testability.</span></span>

<span data-ttu-id="b2c70-423">Zum Instanziieren einer aktiven IRepository-&lt;t&gt; ist ein EF4 ObjectContext erforderlich, sodass die Instanziierung durch eine konkrete Arbeitseinheits Implementierung verwaltet werden muss.</span><span class="sxs-lookup"><span data-stu-id="b2c70-423">Instantiating a live IRepository&lt;T&gt; requires an EF4 ObjectContext, so a concrete unit of work implementation should manage the instantiation.</span></span>

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

### <a name="using-the-custom-repository"></a><span data-ttu-id="b2c70-424">Verwenden des benutzerdefinierten Repository</span><span class="sxs-lookup"><span data-stu-id="b2c70-424">Using the Custom Repository</span></span>

<span data-ttu-id="b2c70-425">Die Verwendung des benutzerdefinierten Repository unterscheidet sich nicht wesentlich von der Verwendung des Repository auf der Grundlage von IObjectSet&lt;t&gt;.</span><span class="sxs-lookup"><span data-stu-id="b2c70-425">Using our custom repository is not significantly different from using the repository based on IObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="b2c70-426">Anstatt LINQ-Operatoren direkt auf eine Eigenschaft anzuwenden, müssen wir zuerst eine der Repository-Methoden aufrufen, um einen iquerable&lt;t-&gt; Verweis zu erfassen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-426">Instead of applying LINQ operators directly to a property, we’ll first need to invoke one the repository’s methods to grab an IQueryable&lt;T&gt; reference.</span></span>

``` csharp
    public ViewResult Index() {
        var model = _repository.FindAll()
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

<span data-ttu-id="b2c70-427">Beachten Sie, dass der zuvor implementierte benutzerdefinierte Include-Operator unverändert funktioniert.</span><span class="sxs-lookup"><span data-stu-id="b2c70-427">Notice the custom Include operator we implemented previously will work without change.</span></span> <span data-ttu-id="b2c70-428">Die findByID-Methode des Repository entfernt duplizierte Logik von Aktionen, die versuchen, eine einzelne Entität abzurufen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-428">The repository’s FindById method removes duplicated logic from actions trying to retrieve a single entity.</span></span>

``` csharp
    public ViewResult Details(int id) {
        var model = _repository.FindById(id);
        return View(model);
    }
```

<span data-ttu-id="b2c70-429">Es gibt keinen signifikanten Unterschied bei der Testability der beiden Ansätze, die wir untersucht haben.</span><span class="sxs-lookup"><span data-stu-id="b2c70-429">There is no significant difference in the testability of the two approaches we’ve examined.</span></span> <span data-ttu-id="b2c70-430">Wir könnten gefälschte Implementierungen von IRepository&lt;t-&gt; bereitstellen, indem wir konkrete Klassen erstellen, die durch das HashSet&lt;Employee&gt; unterstützt werden, genau wie im letzten Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="b2c70-430">We could provide fake implementations of IRepository&lt;T&gt; by building concrete classes backed by HashSet&lt;Employee&gt; - just like what we did in the last section.</span></span> <span data-ttu-id="b2c70-431">Allerdings bevorzugen einige Entwickler die Verwendung von Mockobjekten und Pseudo Objekt-Frameworks, anstatt Fakes zu entwickeln.</span><span class="sxs-lookup"><span data-stu-id="b2c70-431">However, some developers prefer to use mock objects and mock object frameworks instead of building fakes.</span></span> <span data-ttu-id="b2c70-432">Wir werden uns mit der Verwendung von Mock beschäftigen, um unsere Implementierung zu testen und die Unterschiede zwischen den Pseudo-und Fakes im nächsten Abschnitt zu erörtern.</span><span class="sxs-lookup"><span data-stu-id="b2c70-432">We’ll look at using mocks to test our implementation and discuss the differences between mocks and fakes in the next section.</span></span>

### <a name="testing-with-mocks"></a><span data-ttu-id="b2c70-433">Testen mit Mock</span><span class="sxs-lookup"><span data-stu-id="b2c70-433">Testing with Mocks</span></span>

<span data-ttu-id="b2c70-434">Es gibt verschiedene Ansätze zum Entwickeln von Martin Fowler, der einen "Test Double" aufruft.</span><span class="sxs-lookup"><span data-stu-id="b2c70-434">There are different approaches to building what Martin Fowler calls a “test double”.</span></span> <span data-ttu-id="b2c70-435">Ein Test Double (z. b. ein Movie Stuntdouble) ist ein Objekt, das Sie für die "Position" bei echten Produktions Objekten während der Tests erstellen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-435">A test double (like a movie stunt double) is an object you build to “stand in” for real, production objects during tests.</span></span> <span data-ttu-id="b2c70-436">Bei den in-Memory-Depots, die wir erstellt haben, handelt es sich um Test Doubles für die Depots, die SQL Server</span><span class="sxs-lookup"><span data-stu-id="b2c70-436">The in-memory repositories we created are test doubles for the repositories that talk to SQL Server.</span></span> <span data-ttu-id="b2c70-437">Wir haben gesehen, wie diese Test-Doubles während der Komponententests verwendet werden, um den Code zu isolieren und die Tests schnell ausführen zu lassen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-437">We’ve seen how to use these test-doubles during the unit tests to isolate code and keep tests running fast.</span></span>

<span data-ttu-id="b2c70-438">Die Test Doubles, die wir erstellt haben, verfügen über echte, funktionierende Implementierungen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-438">The test doubles we’ve built have real, working implementations.</span></span> <span data-ttu-id="b2c70-439">Im Hintergrund speichert jedes eine konkrete Auflistung von Objekten, und Sie fügen Objekte aus dieser Auflistung hinzu und entfernen Sie, während das Repository während eines Tests bearbeitet wird.</span><span class="sxs-lookup"><span data-stu-id="b2c70-439">Behind the scenes each one stores a concrete collection of objects, and they will add and remove objects from this collection as we manipulate the repository during a test.</span></span> <span data-ttu-id="b2c70-440">Einige Entwickler, die Ihre Tests erstellen möchten, verdoppeln diese Art – mit echtem Code und funktionierenden Implementierungen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-440">Some developers like to build their test doubles this way – with real code and working implementations.</span></span><span data-ttu-id="b2c70-441">  Diese Test Doubles werden als *Fakes*bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="b2c70-441">  These test doubles are what we call *fakes*.</span></span> <span data-ttu-id="b2c70-442">Sie verfügen über funktionierende Implementierungen, sind aber nicht für die Verwendung in der Produktion ausreichend.</span><span class="sxs-lookup"><span data-stu-id="b2c70-442">They have working implementations, but they aren’t real enough for production use.</span></span> <span data-ttu-id="b2c70-443">Das Fake-Repository schreibt tatsächlich nicht in die Datenbank.</span><span class="sxs-lookup"><span data-stu-id="b2c70-443">The fake repository doesn’t actually write to the database.</span></span> <span data-ttu-id="b2c70-444">Der gefälschte SMTP-Server sendet tatsächlich keine e-Mail-Nachricht über das Netzwerk.</span><span class="sxs-lookup"><span data-stu-id="b2c70-444">The fake SMTP server doesn’t actually send an email message over the network.</span></span>

### <a name="mocks-versus-fakes"></a><span data-ttu-id="b2c70-445">Pseudo-und Fakes</span><span class="sxs-lookup"><span data-stu-id="b2c70-445">Mocks versus Fakes</span></span>

<span data-ttu-id="b2c70-446">Es gibt einen anderen Typ von Test Double, der als *Mock*bezeichnet wird.</span><span class="sxs-lookup"><span data-stu-id="b2c70-446">There is another type of test double known as a *mock*.</span></span> <span data-ttu-id="b2c70-447">Obwohl Fakes über funktionierende Implementierungen verfügen, gibt es keine Implementierung.</span><span class="sxs-lookup"><span data-stu-id="b2c70-447">While fakes have working implementations, mocks come with no implementation.</span></span> <span data-ttu-id="b2c70-448">Mithilfe eines Pseudo Objekt Frameworks erstellen wir diese Mockobjekte zur Laufzeit und verwenden Sie als Test Doubles.</span><span class="sxs-lookup"><span data-stu-id="b2c70-448">With the help of a mock object framework we construct these mock objects at run time and use them as test doubles.</span></span> <span data-ttu-id="b2c70-449">In diesem Abschnitt verwenden wir das Open Source-Framework für das Microsoft-Framework.</span><span class="sxs-lookup"><span data-stu-id="b2c70-449">In this section we’ll be using the open source mocking framework Moq.</span></span> <span data-ttu-id="b2c70-450">Im folgenden finden Sie ein einfaches Beispiel für die dynamische Erstellung eines Test Double für ein Employee-Repository mithilfe von "muq".</span><span class="sxs-lookup"><span data-stu-id="b2c70-450">Here is a simple example of using Moq to dynamically create a test double for an employee repository.</span></span>

``` csharp
    Mock<IRepository<Employee>> mock =
        new Mock<IRepository<Employee>>();
    IRepository<Employee> repository = mock.Object;
    repository.Add(new Employee());
    var employee = repository.FindById(1);
```

<span data-ttu-id="b2c70-451">Wir bitten um die Implementierung eines IRepository&lt;Mitarbeiter&gt; der Implementierung von Microsoft.</span><span class="sxs-lookup"><span data-stu-id="b2c70-451">We ask Moq for an IRepository&lt;Employee&gt; implementation and it builds one dynamically.</span></span> <span data-ttu-id="b2c70-452">Wir können zu dem Objekt gelangen, das IRepository&lt;Mitarbeiter&gt; implementiert, indem wir auf die Object-Eigenschaft des Mock&lt;t&gt;-Objekts zugreifen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-452">We can get to the object implementing IRepository&lt;Employee&gt; by accessing the Object property of the Mock&lt;T&gt; object.</span></span> <span data-ttu-id="b2c70-453">Es ist das innere Objekt, das wir an unsere Controller übergeben können, und Sie wissen nicht, ob es sich um ein Test Double oder das echte Repository handelt.</span><span class="sxs-lookup"><span data-stu-id="b2c70-453">It is this inner object we can pass into our controllers, and they won’t know if this is a test double or the real repository.</span></span> <span data-ttu-id="b2c70-454">Wir können Methoden für das Objekt so aufrufen, wie Methoden für ein Objekt mit einer echten Implementierung aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="b2c70-454">We can invoke methods on the object just like we would invoke methods on an object with a real implementation.</span></span>

<span data-ttu-id="b2c70-455">Beim Aufrufen der Add-Methode müssen Sie sich Fragen, was das mockrepository tun wird.</span><span class="sxs-lookup"><span data-stu-id="b2c70-455">You must be wondering what the mock repository will do when we invoke the Add method.</span></span> <span data-ttu-id="b2c70-456">Da keine Implementierung hinter dem Mock-Objekt vorhanden ist, wird von Add nichts unterstützt.</span><span class="sxs-lookup"><span data-stu-id="b2c70-456">Since there is no implementation behind the mock object, Add does nothing.</span></span> <span data-ttu-id="b2c70-457">Es gibt keine konkrete Sammlung im Hintergrund, wie wir es mit den von uns geschriebenen Fakes getan haben, sodass der Mitarbeiter verworfen wird.</span><span class="sxs-lookup"><span data-stu-id="b2c70-457">There is no concrete collection behind the scenes like we had with the fakes we wrote, so the employee is discarded.</span></span> <span data-ttu-id="b2c70-458">Wie sieht es mit dem Rückgabewert von "findByID" aus?</span><span class="sxs-lookup"><span data-stu-id="b2c70-458">What about the return value of FindById?</span></span> <span data-ttu-id="b2c70-459">In diesem Fall übernimmt das Mock-Objekt das einzige, was es tun kann. es wird ein Standardwert zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="b2c70-459">In this case the mock object does the only thing it can do, which is return a default value.</span></span> <span data-ttu-id="b2c70-460">Da wir einen Verweistyp (einen Mitarbeiter) zurückgeben, ist der Rückgabewert ein NULL-Wert.</span><span class="sxs-lookup"><span data-stu-id="b2c70-460">Since we are returning a reference type (an Employee), the return value is a null value.</span></span>

<span data-ttu-id="b2c70-461">Mock klingen möglicherweise wertlos. Allerdings gibt es noch zwei weitere Features, über die wir nicht gesprochen haben.</span><span class="sxs-lookup"><span data-stu-id="b2c70-461">Mocks might sound worthless; however, there are two more features of mocks we haven’t talked about.</span></span> <span data-ttu-id="b2c70-462">Zuerst zeichnet das MOQ-Framework alle Aufrufe auf dem Mock-Objekt auf.</span><span class="sxs-lookup"><span data-stu-id="b2c70-462">First, the Moq framework records all the calls made on the mock object.</span></span> <span data-ttu-id="b2c70-463">Zu einem späteren Zeitpunkt im Code können Sie die Frage stellen, ob jemand die Add-Methode aufgerufen hat, oder ob jemand die findByID-Methode aufgerufen hat.</span><span class="sxs-lookup"><span data-stu-id="b2c70-463">Later in the code we can ask Moq if anyone invoked the Add method, or if anyone invoked the FindById method.</span></span> <span data-ttu-id="b2c70-464">Wir werden später sehen, wie diese "Black Box"-Aufzeichnungsfunktion in Tests verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="b2c70-464">We’ll see later how we can use this “black box” recording feature in tests.</span></span>

<span data-ttu-id="b2c70-465">Das zweite hervorragend ist, wie wir MOQ verwenden können, um ein Mock-Objekt mit *Erwartungen*zu programmieren.</span><span class="sxs-lookup"><span data-stu-id="b2c70-465">The second great feature is how we can use Moq to program a mock object with *expectations*.</span></span> <span data-ttu-id="b2c70-466">Eine Erwartungs Angabe weist das Mock-Objekt an, wie auf eine bestimmte Interaktion reagiert werden soll.</span><span class="sxs-lookup"><span data-stu-id="b2c70-466">An expectation tells the mock object how to respond to any given interaction.</span></span> <span data-ttu-id="b2c70-467">Beispielsweise können wir eine Erwartungs Annahme in unser Mock programmieren und Sie darüber informieren, dass ein Employee-Objekt zurückgegeben wird, wenn ein Benutzer "findByID" aufruft</span><span class="sxs-lookup"><span data-stu-id="b2c70-467">For example, we can program an expectation into our mock and tell it to return an employee object when someone invokes FindById.</span></span> <span data-ttu-id="b2c70-468">Das-Framework verwendet eine Setup-API und Lambda-Ausdrücke, um diese Erwartungen zu programmieren.</span><span class="sxs-lookup"><span data-stu-id="b2c70-468">The Moq framework uses a Setup API and lambda expressions to program these expectations.</span></span>

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

<span data-ttu-id="b2c70-469">In diesem Beispiel stellen wir fest, dass Sie ein Repository dynamisch erstellen und dann das Repository mit einer Erwartung programmieren.</span><span class="sxs-lookup"><span data-stu-id="b2c70-469">In this sample we ask Moq to dynamically build a repository, and then we program the repository with an expectation.</span></span> <span data-ttu-id="b2c70-470">Die Erwartungs Angabe weist das Mock-Objekt an, ein neues Employee-Objekt mit dem ID-Wert 5 zurückzugeben, wenn jemand die findByID-Methode aufruft und den Wert 5 übergibt.</span><span class="sxs-lookup"><span data-stu-id="b2c70-470">The expectation tells the mock object to return a new employee object with an Id value of 5 when someone invokes the FindById method passing a value of 5.</span></span> <span data-ttu-id="b2c70-471">Dieser Test wird erfolgreich durchlaufen, und es ist nicht erforderlich, eine vollständige Implementierung für die gefälschte IRepository-&lt;t&gt;zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-471">This test passes, and we didn’t need to build a full implementation to fake IRepository&lt;T&gt;.</span></span>

<span data-ttu-id="b2c70-472">Schauen wir uns noch einmal die Tests an, die wir zuvor geschrieben haben, und arbeiten Sie zur Verwendung von Pseudo-anstelle von Fakes.</span><span class="sxs-lookup"><span data-stu-id="b2c70-472">Let’s revisit the tests we wrote earlier and rework them to use mocks instead of fakes.</span></span> <span data-ttu-id="b2c70-473">Wie zuvor verwenden wir eine Basisklasse, um die gemeinsamen Teile der Infrastruktur einzurichten, die wir für alle Tests des Controllers benötigen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-473">Just like before, we’ll use a base class to setup the common pieces of infrastructure we need for all of the controller’s tests.</span></span>

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

<span data-ttu-id="b2c70-474">Der Setup Code bleibt größtenteils unverändert.</span><span class="sxs-lookup"><span data-stu-id="b2c70-474">The setup code remains mostly the same.</span></span> <span data-ttu-id="b2c70-475">Anstatt Fakes zu verwenden, verwenden wir MOQ, um Mock-Objekte zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-475">Instead of using fakes, we’ll use Moq to construct mock objects.</span></span> <span data-ttu-id="b2c70-476">Die-Basisklasse ordnet an, dass die Mock-Arbeitseinheit ein Mock-Repository zurückgibt, wenn der Code die Employees-Eigenschaft aufruft.</span><span class="sxs-lookup"><span data-stu-id="b2c70-476">The base class arranges for the mock unit of work to return a mock repository when code invokes the Employees property.</span></span> <span data-ttu-id="b2c70-477">Der Rest des Mock-Setups erfolgt in den Test Vorrichtungen, die für jedes bestimmte Szenario vorgesehen sind.</span><span class="sxs-lookup"><span data-stu-id="b2c70-477">The rest of the mock setup will take place inside the test fixtures dedicated to each specific scenario.</span></span> <span data-ttu-id="b2c70-478">Beispielsweise wird durch die Test Fixierung für die Index Aktion das mockrepository so eingerichtet, dass eine Liste von Mitarbeitern zurückgegeben wird, wenn die Aktion die FindAll-Methode des mockrepository aufruft.</span><span class="sxs-lookup"><span data-stu-id="b2c70-478">For example, the test fixture for the Index action will setup the mock repository to return a list of employees when the action invokes the FindAll method of the mock repository.</span></span>

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

<span data-ttu-id="b2c70-479">Mit Ausnahme der Erwartungen sehen unsere Tests den Tests ähnlich wie zuvor.</span><span class="sxs-lookup"><span data-stu-id="b2c70-479">Except for the expectations, our tests look similar to the tests we had before.</span></span> <span data-ttu-id="b2c70-480">Mit der Aufzeichnungs Fähigkeit eines Mock-Frameworks können wir die Tests jedoch in einem anderen Winkel durchgehen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-480">However, with the recording ability of a mock framework we can approach testing from a different angle.</span></span> <span data-ttu-id="b2c70-481">Wir betrachten diese neue Perspektive im nächsten Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="b2c70-481">We’ll look at this new perspective in the next section.</span></span>

### <a name="state-versus-interaction-testing"></a><span data-ttu-id="b2c70-482">Testzustand im Vergleich zu Interaktion</span><span class="sxs-lookup"><span data-stu-id="b2c70-482">State versus Interaction Testing</span></span>

<span data-ttu-id="b2c70-483">Es gibt verschiedene Techniken, die Sie zum Testen von Software mit Mock-Objekten verwenden können.</span><span class="sxs-lookup"><span data-stu-id="b2c70-483">There are different techniques you can use to test software with mock objects.</span></span> <span data-ttu-id="b2c70-484">Ein Ansatz besteht in der Verwendung von Zustands basierten Tests, was in diesem Artikel bisher geschehen ist.</span><span class="sxs-lookup"><span data-stu-id="b2c70-484">One approach is to use state based testing, which is what we have done in this paper so far.</span></span> <span data-ttu-id="b2c70-485">Zustands basierte Tests machen Assertionen zum Zustand der Software.</span><span class="sxs-lookup"><span data-stu-id="b2c70-485">State based testing makes assertions about the state of the software.</span></span> <span data-ttu-id="b2c70-486">Im letzten Test haben wir eine Aktionsmethode auf dem Controller aufgerufen und eine Aussage über das Modell vorgenommen, das erstellt werden soll.</span><span class="sxs-lookup"><span data-stu-id="b2c70-486">In the last test we invoked an action method on the controller and made an assertion about the model it should build.</span></span> <span data-ttu-id="b2c70-487">Hier sind einige weitere Beispiele für den Test Status:</span><span class="sxs-lookup"><span data-stu-id="b2c70-487">Here are some other examples of testing state:</span></span>

-   <span data-ttu-id="b2c70-488">Vergewissern Sie sich, dass das Repository das neue Mitarbeiter Objekt enthält, nachdem Create ausgeführt wurde.</span><span class="sxs-lookup"><span data-stu-id="b2c70-488">Verify the repository contains the new employee object after Create executes.</span></span>
-   <span data-ttu-id="b2c70-489">Vergewissern Sie sich, dass das Modell nach der Ausführung des Indexes eine Liste aller Mitarbeiter enthält.</span><span class="sxs-lookup"><span data-stu-id="b2c70-489">Verify the model holds a list of all employees after Index executes.</span></span>
-   <span data-ttu-id="b2c70-490">Vergewissern Sie sich, dass das Repository nach der Ausführung von DELETE keinen bestimmten Mitarbeiter enthält.</span><span class="sxs-lookup"><span data-stu-id="b2c70-490">Verify the repository does not contain a given employee after Delete executes.</span></span>

<span data-ttu-id="b2c70-491">Ein weiterer Ansatz, den Sie mit Mock-Objekten erkennen, ist das Überprüfen von *Interaktionen*.</span><span class="sxs-lookup"><span data-stu-id="b2c70-491">Another approach you’ll see with mock objects is to verify *interactions*.</span></span> <span data-ttu-id="b2c70-492">Während Zustands basierte Tests Assertionen zum Status von Objekten machen, werden durch Interaktions basierte Tests Assertionen für die Interaktion von Objekten durchführt.</span><span class="sxs-lookup"><span data-stu-id="b2c70-492">While state based testing makes assertions about the state of objects, interaction based testing makes assertions about how objects interact.</span></span> <span data-ttu-id="b2c70-493">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="b2c70-493">For example:</span></span>

-   <span data-ttu-id="b2c70-494">Vergewissern Sie sich, dass der Controller beim Ausführen von CREATE die Add-Methode des Repository aufruft.</span><span class="sxs-lookup"><span data-stu-id="b2c70-494">Verify the controller invokes the repository’s Add method when Create executes.</span></span>
-   <span data-ttu-id="b2c70-495">Vergewissern Sie sich, dass der Controller die FindAll-Methode des Repository aufruft, wenn Index ausgeführt wird</span><span class="sxs-lookup"><span data-stu-id="b2c70-495">Verify the controller invokes the repository’s FindAll method when Index executes.</span></span>
-   <span data-ttu-id="b2c70-496">Vergewissern Sie sich, dass der Controller die Commit-Methode der Arbeitseinheit aufruft, um beim Ausführen der Bearbeitung Änderungen zu speichern.</span><span class="sxs-lookup"><span data-stu-id="b2c70-496">Verify the controller invokes the unit of work’s Commit method to save changes when Edit executes.</span></span>

<span data-ttu-id="b2c70-497">Interaktions Tests erfordern häufig weniger Testdaten, da wir nicht innerhalb von Auflistungen stehen und die Anzahl überprüfen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-497">Interaction testing often requires less test data, because we aren’t poking inside of collections and verifying counts.</span></span> <span data-ttu-id="b2c70-498">Wenn wir z. b. wissen, dass die Aktion "Details" die findByID-Methode eines Repository mit dem korrekten Wert aufruft, verhält sich die Aktion wahrscheinlich ordnungsgemäß.</span><span class="sxs-lookup"><span data-stu-id="b2c70-498">For example, if we know the Details action invokes a repository’s FindById method with the correct value - then the action is probably behaving correctly.</span></span> <span data-ttu-id="b2c70-499">Wir können dieses Verhalten überprüfen, ohne Testdaten einzurichten, die von findByID zurückgegeben werden sollen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-499">We can verify this behavior without setting up any test data to return from FindById.</span></span>

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

<span data-ttu-id="b2c70-500">Das einzige Setup, das in der obigen Test Fixierung erforderlich ist, ist das von der Basisklasse bereitgestellte Setup.</span><span class="sxs-lookup"><span data-stu-id="b2c70-500">The only setup required in the above test fixture is the setup provided by the base class.</span></span> <span data-ttu-id="b2c70-501">Wenn wir die Controller Aktion aufrufen, zeichnet MOQ die Interaktionen mit dem Mock-Repository auf.</span><span class="sxs-lookup"><span data-stu-id="b2c70-501">When we invoke the controller action, Moq will record the interactions with the mock repository.</span></span> <span data-ttu-id="b2c70-502">Mithilfe der Verify-API von WQ können wir die Frage stellen, ob der Controller "findByID" mit dem richtigen ID-Wert aufgerufen hat.</span><span class="sxs-lookup"><span data-stu-id="b2c70-502">Using the Verify API of Moq, we can ask Moq if the controller invoked FindById with the proper ID value.</span></span> <span data-ttu-id="b2c70-503">Wenn der Controller die Methode nicht aufgerufen hat oder die Methode mit einem unerwarteten Parameterwert aufgerufen hat, löst die Verify-Methode eine Ausnahme aus, und der Test schlägt fehl.</span><span class="sxs-lookup"><span data-stu-id="b2c70-503">If the controller did not invoke the method, or invoked the method with an unexpected parameter value, the Verify method will throw an exception and the test will fail.</span></span>

<span data-ttu-id="b2c70-504">Es folgt ein weiteres Beispiel, um zu überprüfen, ob die CREATE-Aktion Commit für die aktuelle Arbeitseinheit aufruft.</span><span class="sxs-lookup"><span data-stu-id="b2c70-504">Here is another example to verify the Create action invokes Commit on the current unit of work.</span></span>

``` csharp
    [TestMethod]
    public void ShouldCommitUnitOfWork() {
        _controller.Create(_newEmployee);
        _unitOfWork.Verify(u => u.Commit());
    }
```

<span data-ttu-id="b2c70-505">Eine Gefahr beim Testen der Interaktion ist die Tendenz, Interaktionen zu überschreiten.</span><span class="sxs-lookup"><span data-stu-id="b2c70-505">One danger with interaction testing is the tendency to over specify interactions.</span></span> <span data-ttu-id="b2c70-506">Die Fähigkeit des Mockobjekts, jede Interaktion mit dem Mock-Objekt aufzuzeichnen und zu überprüfen, bedeutet nicht, dass der Test versuchen soll, jede Interaktion zu überprüfen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-506">The ability of the mock object to record and verify every interaction with the mock object doesn’t mean the test should try to verify every interaction.</span></span> <span data-ttu-id="b2c70-507">Einige Interaktionen sind Implementierungsdetails, und Sie sollten nur die Interaktionen überprüfen, die zum erfüllen des aktuellen Tests *erforderlich* sind.</span><span class="sxs-lookup"><span data-stu-id="b2c70-507">Some interactions are implementation details and you should only verify the interactions *required* to satisfy the current test.</span></span>

<span data-ttu-id="b2c70-508">Die Wahl zwischen den Mocken oder Fakes hängt größtenteils von dem System ab, das Sie testen, und Ihren persönlichen (oder Team-) Voreinstellungen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-508">The choice between mocks or fakes largely depends on the system you are testing and your personal (or team) preferences.</span></span> <span data-ttu-id="b2c70-509">Mock-Objekte können die Menge an Code, den Sie zum Implementieren von Test Doubles benötigen, drastisch reduzieren, aber nicht jeder ist für die Programmierung von Erwartungen und das Überprüfen von Interaktionen</span><span class="sxs-lookup"><span data-stu-id="b2c70-509">Mock objects can drastically reduce the amount of code you need to implement test doubles, but not everyone is comfortable programming expectations and verifying interactions.</span></span>

## <a name="conclusions"></a><span data-ttu-id="b2c70-510">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="b2c70-510">Conclusions</span></span>

<span data-ttu-id="b2c70-511">In diesem Whitepaper haben wir verschiedene Ansätze zum Erstellen von testbarem Code gezeigt, während wir die ADO.NET-Entity Framework für die Daten Persistenz verwenden.</span><span class="sxs-lookup"><span data-stu-id="b2c70-511">In this paper we’ve demonstrated several approaches to creating testable code while using the ADO.NET Entity Framework for data persistence.</span></span> <span data-ttu-id="b2c70-512">Wir können integrierte Abstraktionen wie IObjectSet&lt;t&gt;nutzen oder eigene Abstraktionen wie IRepository&lt;t&gt;erstellen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-512">We can leverage built in abstractions like IObjectSet&lt;T&gt;, or create our own abstractions like IRepository&lt;T&gt;.</span></span><span data-ttu-id="b2c70-513">  In beiden Fällen ermöglicht die poco-Unterstützung im ADO.NET-Entity Framework 4,0, dass die Consumer dieser Abstraktionen permanent ignoriert werden und hochgradig testfähig bleiben.</span><span class="sxs-lookup"><span data-stu-id="b2c70-513">  In both cases, the POCO support in the ADO.NET Entity Framework 4.0 allows the consumers of these abstractions to remain persistent ignorant and highly testable.</span></span> <span data-ttu-id="b2c70-514">Zusätzliche EF4-Features wie implizites Lazy Loading ermöglichen, dass Geschäfts-und Anwendungs Dienst Code funktionieren, ohne sich Gedanken über die Details eines relationalen Datenspeicher zu machen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-514">Additional EF4 features like implicit lazy loading allows business and application service code to work without worrying about the details of a relational data store.</span></span> <span data-ttu-id="b2c70-515">Schließlich können die von uns erstellten Abstraktionen leicht in Komponententests hinein oder gefälscht werden, und wir können diese Test Doubles verwenden, um schnelle, hochgradig isolierte und zuverlässige Tests zu erzielen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-515">Finally, the abstractions we create are easy to mock or fake inside of unit tests, and we can use these test doubles to achieve fast running, highly isolated, and reliable tests.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="b2c70-516">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="b2c70-516">Additional Resources</span></span>

-   <span data-ttu-id="b2c70-517">Robert C. Martin " [das Prinzip der einzelnen Verantwortung](https://www.objectmentor.com/resources/articles/srp.pdf)"</span><span class="sxs-lookup"><span data-stu-id="b2c70-517">Robert C. Martin, “ [The Single Responsibility Principle](https://www.objectmentor.com/resources/articles/srp.pdf)”</span></span>
-   <span data-ttu-id="b2c70-518">Martin Fowler, [Katalog mit Mustern](https://www.martinfowler.com/eaaCatalog/index.html) aus *Mustern der Unternehmens Anwendungsarchitektur*</span><span class="sxs-lookup"><span data-stu-id="b2c70-518">Martin Fowler, [Catalog of Patterns](https://www.martinfowler.com/eaaCatalog/index.html) from *Patterns of Enterprise Application Architecture*</span></span>
-   <span data-ttu-id="b2c70-519">Griffin Caprio, " [Abhängigkeitsinjektion](https://msdn.microsoft.com/magazine/cc163739.aspx)"</span><span class="sxs-lookup"><span data-stu-id="b2c70-519">Griffin Caprio, “ [Dependency Injection](https://msdn.microsoft.com/magazine/cc163739.aspx)”</span></span>
-   <span data-ttu-id="b2c70-520">Blog zur Daten Programmierbarkeit, "Exemplarische Vorgehensweise [: Test gesteuerte Entwicklung mit dem Entity Framework 4,0](https://blogs.msdn.com/adonet/pages/walkthrough-test-driven-development-with-the-entity-framework-4-0.aspx)".</span><span class="sxs-lookup"><span data-stu-id="b2c70-520">Data Programmability Blog, “ [Walkthrough: Test Driven Development with the Entity Framework 4.0](https://blogs.msdn.com/adonet/pages/walkthrough-test-driven-development-with-the-entity-framework-4-0.aspx)”.</span></span>
-   <span data-ttu-id="b2c70-521">Daten Programmierbarkeits Blog " [using Repository and Unit of Work Patterns with Entity Framework 4,0](https://blogs.msdn.com/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)"</span><span class="sxs-lookup"><span data-stu-id="b2c70-521">Data Programmability Blog, “ [Using Repository and Unit of Work patterns with Entity Framework 4.0](https://blogs.msdn.com/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)”</span></span>
-   <span data-ttu-id="b2c70-522">Aaron Jensen, " [Einführung in Computerspezifikationen](http://codebetter.com/blogs/aaron.jensen/archive/2008/05/08/introducing-machine-specifications-or-mspec-for-short.aspx)"</span><span class="sxs-lookup"><span data-stu-id="b2c70-522">Aaron Jensen, “ [Introducing Machine Specifications](http://codebetter.com/blogs/aaron.jensen/archive/2008/05/08/introducing-machine-specifications-or-mspec-for-short.aspx)”</span></span>
-   <span data-ttu-id="b2c70-523">Eric Lee, " [BDD with MSTest](https://blogs.msdn.com/elee/archive/2009/01/20/bdd-with-mstest.aspx)"</span><span class="sxs-lookup"><span data-stu-id="b2c70-523">Eric Lee, “ [BDD with MSTest](https://blogs.msdn.com/elee/archive/2009/01/20/bdd-with-mstest.aspx)”</span></span>
-   <span data-ttu-id="b2c70-524">Eric Evans, " [Domain-gesteuerte Design](https://books.google.com/books?id=7dlaMs0SECsC&printsec=frontcover&dq=evans%20domain%20driven%20design&hl=en&ei=cHztS6C8KIaglAfA_dS1CA&sa=X&oi=book_result&ct=result&resnum=1&ved=0CCoQ6AEwAA)"</span><span class="sxs-lookup"><span data-stu-id="b2c70-524">Eric Evans, “ [Domain Driven Design](https://books.google.com/books?id=7dlaMs0SECsC&printsec=frontcover&dq=evans%20domain%20driven%20design&hl=en&ei=cHztS6C8KIaglAfA_dS1CA&sa=X&oi=book_result&ct=result&resnum=1&ved=0CCoQ6AEwAA)”</span></span>
-   <span data-ttu-id="b2c70-525">Martin Fowler, "Mock [sind keine Stuf"](https://martinfowler.com/articles/mocksArentStubs.html)</span><span class="sxs-lookup"><span data-stu-id="b2c70-525">Martin Fowler, “ [Mocks Aren’t Stubs](https://martinfowler.com/articles/mocksArentStubs.html)”</span></span>
-   <span data-ttu-id="b2c70-526">Martin Fowler, " [Test Double](https://martinfowler.com/bliki/TestDouble.html)"</span><span class="sxs-lookup"><span data-stu-id="b2c70-526">Martin Fowler, “ [Test Double](https://martinfowler.com/bliki/TestDouble.html)”</span></span>
-   [<span data-ttu-id="b2c70-527">MOQ</span><span class="sxs-lookup"><span data-stu-id="b2c70-527">Moq</span></span>](https://code.google.com/p/moq/)

### <a name="biography"></a><span data-ttu-id="b2c70-528">Biografie</span><span class="sxs-lookup"><span data-stu-id="b2c70-528">Biography</span></span>

<span data-ttu-id="b2c70-529">Scott allen ist Mitglied des technischen Personals bei Pluralsight und der Gründer von OdeToCode.com.</span><span class="sxs-lookup"><span data-stu-id="b2c70-529">Scott Allen is a member of the technical staff at Pluralsight and the founder of OdeToCode.com.</span></span> <span data-ttu-id="b2c70-530">In 15 Jahren kommerzieller Softwareentwicklung hat Scott an Lösungen für alles von 8-Bit Embedded-Geräten bis hin zu hochgradig skalierbaren ASP.NET-Webanwendungen gearbeitet.</span><span class="sxs-lookup"><span data-stu-id="b2c70-530">In 15 years of commercial software development, Scott has worked on solutions for everything from 8-bit embedded devices to highly scalable ASP.NET web applications.</span></span> <span data-ttu-id="b2c70-531">Sie können Scott im Blog von odeycode oder auf Twitter unter [https://twitter.com/OdeToCode](https://twitter.com/OdeToCode)erreichen.</span><span class="sxs-lookup"><span data-stu-id="b2c70-531">You can reach Scott on his blog at OdeToCode, or on Twitter at [https://twitter.com/OdeToCode](https://twitter.com/OdeToCode).</span></span>
