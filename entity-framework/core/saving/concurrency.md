---
title: "Parallelitätskonflikte - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 03/03/2018
ms.technology: entity-framework-core
uid: core/saving/concurrency
ms.openlocfilehash: 288d9c6fced5ebbaa2c366248c68547502c3698e
ms.sourcegitcommit: 8f3be0a2a394253efb653388ec66bda964e5ee1b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2018
---
# <a name="handling-concurrency-conflicts"></a><span data-ttu-id="d33f8-102">Behandlung von Parallelitätskonflikten</span><span class="sxs-lookup"><span data-stu-id="d33f8-102">Handling Concurrency Conflicts</span></span>

> [!NOTE]
> <span data-ttu-id="d33f8-103">Auf dieser Seite werden die Funktionsweise von Parallelität in EF Core und Parallelitätskonflikte in Ihrer Anwendung zu behandeln.</span><span class="sxs-lookup"><span data-stu-id="d33f8-103">This page documents how concurrency works in EF Core and how to handle concurrency conflicts in your application.</span></span> <span data-ttu-id="d33f8-104">Finden Sie unter [Parallelitätstoken](xref:core/modeling/concurrency) Weitere Informationen zum Konfigurieren von parallelitätstoken in Ihrem Modell.</span><span class="sxs-lookup"><span data-stu-id="d33f8-104">See [Concurrency Tokens](xref:core/modeling/concurrency) for details on how to configure concurrency tokens in your model.</span></span>

> [!TIP]
> <span data-ttu-id="d33f8-105">Das in diesem Artikel verwendete [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Concurrency/) finden Sie auf GitHub.</span><span class="sxs-lookup"><span data-stu-id="d33f8-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Concurrency/) on GitHub.</span></span>

<span data-ttu-id="d33f8-106">_Datenbankparallelität_ bezieht sich auf Situationen, in dem mehrere Prozesse oder Benutzer Zugriff auf ein, oder ändern die gleichen Daten in einer Datenbank zur gleichen Zeit.</span><span class="sxs-lookup"><span data-stu-id="d33f8-106">_Database concurrency_ refers to situations in which multiple processes or users access or change the same data in a database at the same time.</span></span> <span data-ttu-id="d33f8-107">_Parallelitätssteuerung_ bezieht sich auf bestimmte Mechanismen verwendet, um die Datenkonsistenz in Anwesenheit von gleichzeitigen Änderungen sicherzustellen.</span><span class="sxs-lookup"><span data-stu-id="d33f8-107">_Concurrency control_ refers to specific mechanisms used to ensure data consistency in presence of concurrent changes.</span></span>

<span data-ttu-id="d33f8-108">EF Core implementiert _Steuerung durch vollständige Parallelität_, d. h., es mehrere Prozesse können oder Änderungen für Benutzer unabhängig ohne den Aufwand für die Synchronisierung oder sperren.</span><span class="sxs-lookup"><span data-stu-id="d33f8-108">EF Core implements _optimistic concurrency control_, meaning that it will let multiple processes or users make changes independently without the overhead of synchronization or locking.</span></span> <span data-ttu-id="d33f8-109">Im Idealfall werden diese Änderungen wirkt sich nicht miteinander und aus diesem Grund werden in der Lage erfolgreich ausgeführt werden kann.</span><span class="sxs-lookup"><span data-stu-id="d33f8-109">In the ideal situation, these changes will not interfere with each other and therefore will be able to succeed.</span></span> <span data-ttu-id="d33f8-110">Im schlimmsten Fall kann zwei oder mehr Prozesse versucht, die in Konflikt stehenden Änderungen vornehmen, und nur eine Kopie sollte erfolgreich sein.</span><span class="sxs-lookup"><span data-stu-id="d33f8-110">In the worst case scenario, two or more processes will attempt to make conflicting changes, and only one of them should succeed.</span></span>

## <a name="how-concurrency-control-works-in-ef-core"></a><span data-ttu-id="d33f8-111">Funktionsweise der parallelitätssteuerung in EF Core</span><span class="sxs-lookup"><span data-stu-id="d33f8-111">How concurrency control works in EF Core</span></span>

<span data-ttu-id="d33f8-112">Eigenschaften, die als parallelitätstoken verwendet werden, um die Steuerung durch vollständige Parallelität implementieren konfiguriert: immer ein Update- oder Delete-Vorgang ausgeführt wird, während der `SaveChanges`, der Wert des parallelitätstokens für die Datenbank mit der ursprünglichen verglichen wird der Wert von EF Core gelesen werden.</span><span class="sxs-lookup"><span data-stu-id="d33f8-112">Properties configured as concurrency tokens are used to implement optimistic concurrency control: whenever an update or delete operation is performed during `SaveChanges`, the value of the concurrency token on the database is compared against the original value read by EF Core.</span></span>

- <span data-ttu-id="d33f8-113">Wenn die Werte übereinstimmen, kann der Vorgang abgeschlossen.</span><span class="sxs-lookup"><span data-stu-id="d33f8-113">If the values match, the operation can complete.</span></span>
- <span data-ttu-id="d33f8-114">Wenn die Werte nicht übereinstimmen, wird EF Core davon ausgegangen, dass ein anderer Benutzer einen Konflikt beteiligte Vorgang ausgeführt hat und bricht die aktuelle Transaktion ab.</span><span class="sxs-lookup"><span data-stu-id="d33f8-114">If the values do not match, EF Core assumes that another user has performed a conflicting operation and aborts the current transaction.</span></span>

<span data-ttu-id="d33f8-115">Die Situation, wenn ein anderer Benutzer einen Vorgang, der in Konflikt steht, mit der aktuelle Vorgang ausgeführt hat, wird als bezeichnet _Parallelitätskonflikt_.</span><span class="sxs-lookup"><span data-stu-id="d33f8-115">The situation when another user has performed an operation that conflicts with the current operation is known as _concurrency conflict_.</span></span>

<span data-ttu-id="d33f8-116">Datenbankanbieter sind verantwortlich für den Vergleich von Tokenwerten Parallelität implementieren.</span><span class="sxs-lookup"><span data-stu-id="d33f8-116">Database providers are responsible for implementing the comparison of concurrency token values.</span></span>

<span data-ttu-id="d33f8-117">Für relationale Datenbanken EF Core umfasst eine Überprüfung der für den Wert des parallelitätstokens in der `WHERE` Klausel beliebiger `UPDATE` oder `DELETE` Anweisungen.</span><span class="sxs-lookup"><span data-stu-id="d33f8-117">On relational databases EF Core includes a check for the value of the concurrency token in the `WHERE` clause of any `UPDATE` or `DELETE` statements.</span></span> <span data-ttu-id="d33f8-118">Nach dem Ausführen der Anweisungen, liest EF Core die Anzahl der Zeilen, die betroffen sind.</span><span class="sxs-lookup"><span data-stu-id="d33f8-118">After executing the statements, EF Core reads the number of rows that were affected.</span></span>

<span data-ttu-id="d33f8-119">Wenn keine Zeilen betroffen sind, ein Parallelitätskonflikt erkannt wird, und EF Core löst `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="d33f8-119">If no rows are affected, a concurrency conflict is detected, and EF Core throws `DbUpdateConcurrencyException`.</span></span>

<span data-ttu-id="d33f8-120">Angenommen, wir möchten konfigurieren `LastName` auf `Person` ein parallelitätstoken ist.</span><span class="sxs-lookup"><span data-stu-id="d33f8-120">For example, we may want to configure `LastName` on `Person` to be a concurrency token.</span></span> <span data-ttu-id="d33f8-121">Alle Update-Vorgang auf Person die Überprüfung auf Parallelität in einschließen, wird die `WHERE` Klausel:</span><span class="sxs-lookup"><span data-stu-id="d33f8-121">Then any update operation on Person will include the concurrency check in the `WHERE` clause:</span></span>

``` sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="resolving-concurrency-conflicts"></a><span data-ttu-id="d33f8-122">Auflösen von Parallelitätskonflikten</span><span class="sxs-lookup"><span data-stu-id="d33f8-122">Resolving concurrency conflicts</span></span>

<span data-ttu-id="d33f8-123">Im vorherigen Beispiel fortsetzen, wenn ein Benutzer versucht, speichern einige Änderungen an einer `Person`, jedoch bereits einem anderen Benutzer geändert wurde die `LastName` der wird eine Ausnahme ausgelöst werden.</span><span class="sxs-lookup"><span data-stu-id="d33f8-123">Continuing with the previous example, if one user tries to save some changes to a `Person`, but another user has already changed the `LastName` the an exception will be thrown.</span></span>

<span data-ttu-id="d33f8-124">Die Anwendung konnte zu diesem Zeitpunkt einfach informiert den Benutzer darüber, dass das Update nicht aufgrund von in Konflikt stehenden Änderungen erfolgreich war und verschieben.</span><span class="sxs-lookup"><span data-stu-id="d33f8-124">At this point, the application could simply inform the user that the update was not successful due to conflicting changes and move on.</span></span> <span data-ttu-id="d33f8-125">Aber es kann wünschenswert sein, fordert den Benutzer aus, um sicherzustellen, dass dieser Datensatz weiterhin dieselbe tatsächlichen Person darstellt, und wiederholen den Vorgang.</span><span class="sxs-lookup"><span data-stu-id="d33f8-125">But it may be desirable to prompt the user to ensure this record still represents the same actual person and to retry the operation.</span></span>

<span data-ttu-id="d33f8-126">Dieser Vorgang ist ein Beispiel für _Auflösen von einem Parallelitätskonflikt_.</span><span class="sxs-lookup"><span data-stu-id="d33f8-126">This process is an example of _resolving a concurrency conflict_.</span></span>

<span data-ttu-id="d33f8-127">Lösen eines Parallelitätskonflikts umfasst das Zusammenführen von die ausstehenden Änderungen aus dem aktuellen `DbContext` mit den Werten in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="d33f8-127">Resolving a concurrency conflict involves merging the pending changes from the current `DbContext` with the values in the database.</span></span> <span data-ttu-id="d33f8-128">Welche Werte zusammengeführt abrufen variiert, je nachdem, auf die Anwendung, und durch die Benutzereingabe gerichtet sein.</span><span class="sxs-lookup"><span data-stu-id="d33f8-128">What values get merged will vary based on the application and may be directed by user input.</span></span>

<span data-ttu-id="d33f8-129">**Es gibt drei Gruppen von Werten verfügbar, in denen einen Parallelitätskonflikt gelöst wird:**</span><span class="sxs-lookup"><span data-stu-id="d33f8-129">**There are three sets of values available to help resolve a concurrency conflict:**</span></span>

* <span data-ttu-id="d33f8-130">**Aktuelle Werte** sind die Werte, die die Anwendung versucht hat, in die Datenbank geschrieben.</span><span class="sxs-lookup"><span data-stu-id="d33f8-130">**Current values** are the values that the application was attempting to write to the database.</span></span>

* <span data-ttu-id="d33f8-131">**Ursprüngliche Werte** sind die Werte, die ursprünglich aus der Datenbank abgerufen wurden, bevor Änderungen vorgenommen wurden.</span><span class="sxs-lookup"><span data-stu-id="d33f8-131">**Original values** are the values that were originally retrieved from the database, before any edits were made.</span></span>

* <span data-ttu-id="d33f8-132">**Datenbank-Werte** sind die Werte, die derzeit in der Datenbank gespeichert.</span><span class="sxs-lookup"><span data-stu-id="d33f8-132">**Database values** are the values currently stored in the database.</span></span>

<span data-ttu-id="d33f8-133">Die allgemeine Vorgehensweise zum Behandeln einer Parallelitätskonflikte ist:</span><span class="sxs-lookup"><span data-stu-id="d33f8-133">The general approach to handle a concurrency conflicts is:</span></span>

1. <span data-ttu-id="d33f8-134">Abfangen `DbUpdateConcurrencyException` während `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="d33f8-134">Catch `DbUpdateConcurrencyException` during `SaveChanges`.</span></span>
2. <span data-ttu-id="d33f8-135">Verwendung `DbUpdateConcurrencyException.Entries` So bereiten Sie einen neuen Satz von Änderungen für die betroffenen Entitäten vor.</span><span class="sxs-lookup"><span data-stu-id="d33f8-135">Use `DbUpdateConcurrencyException.Entries` to prepare a new set of changes for the affected entities.</span></span>
3. <span data-ttu-id="d33f8-136">Aktualisieren Sie die ursprünglichen Werte der im parallelitätstoken, die aktuellen Werte in der Datenbank widerspiegeln.</span><span class="sxs-lookup"><span data-stu-id="d33f8-136">Refresh the original values of the concurrency token to reflect the current values in the database.</span></span>
4. <span data-ttu-id="d33f8-137">Wiederholen Sie den Prozess aus, bis keine Konflikte auftreten.</span><span class="sxs-lookup"><span data-stu-id="d33f8-137">Retry the process until no conflicts occur.</span></span>

<span data-ttu-id="d33f8-138">Im folgenden Beispiel `Person.FirstName` und `Person.LastName` Setup als parallelitätstoken sind.</span><span class="sxs-lookup"><span data-stu-id="d33f8-138">In the following example, `Person.FirstName` and `Person.LastName` are setup as concurrency tokens.</span></span> <span data-ttu-id="d33f8-139">Es ist ein `// TODO:` Kommentar in den Speicherort, bei denen Sie enthalten anwendungsspezifische Logik zum Auswählen des Wert gespeichert werden soll.</span><span class="sxs-lookup"><span data-stu-id="d33f8-139">There is a `// TODO:` comment in the location where you include application specific logic to choose the value to be saved.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Concurrency/Sample.cs?name=ConcurrencyHandlingCode&highlight=34-35)]
