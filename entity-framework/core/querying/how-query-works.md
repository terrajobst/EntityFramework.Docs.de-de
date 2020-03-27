---
title: Funktionsweise von Abfragen – EF Core
author: ajcvickers
ms.date: 03/17/2020
ms.assetid: de2e34cd-659b-4cab-b5ed-7a979c6bf120
uid: core/querying/how-query-works
ms.openlocfilehash: e8a50efe31468ea8df211602636dd474550bc0ef
ms.sourcegitcommit: c3b8386071d64953ee68788ef9d951144881a6ab
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2020
ms.locfileid: "80136238"
---
# <a name="how-queries-work"></a><span data-ttu-id="47e01-102">Funktionsweise von Abfragen</span><span class="sxs-lookup"><span data-stu-id="47e01-102">How Queries Work</span></span>

<span data-ttu-id="47e01-103">Entity Framework Core verwendet Language Integrated Query (LINQ), um Daten von der Datenbank abzufragen.</span><span class="sxs-lookup"><span data-stu-id="47e01-103">Entity Framework Core uses Language Integrated Query (LINQ) to query data from the database.</span></span> <span data-ttu-id="47e01-104">LINQ ermöglicht Ihnen, mit C# (oder Ihrer bevorzugten .NET-Sprache) stark typisierte Abfragen basierend auf Ihrem abgeleiteten Kontext und Entitätsklassen zu schreiben.</span><span class="sxs-lookup"><span data-stu-id="47e01-104">LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries based on your derived context and entity classes.</span></span>

## <a name="the-life-of-a-query"></a><span data-ttu-id="47e01-105">Der Abfragezyklus</span><span class="sxs-lookup"><span data-stu-id="47e01-105">The life of a query</span></span>

<span data-ttu-id="47e01-106">Nachstehend finden Sie eine grobe Übersicht über die verschiedenen Phasen des Abfragevorgangs.</span><span class="sxs-lookup"><span data-stu-id="47e01-106">The following is a high level overview of the process each query goes through.</span></span>

1. <span data-ttu-id="47e01-107">Die LINQ-Abfrage wird von Entity Framework Core verarbeitet. Dabei wird eine Darstellung erstellt, die wiederum vom Datenbankanbieter verarbeitet wird.</span><span class="sxs-lookup"><span data-stu-id="47e01-107">The LINQ query is processed by Entity Framework Core to build a representation that is ready to be processed by the database provider</span></span>
   1. <span data-ttu-id="47e01-108">Das Ergebnis wird zwischengespeichert, damit dieser Vorgang nicht jedes Mal ausgeführt werden muss, wenn die Abfrage ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="47e01-108">The result is cached so that this processing does not need to be done every time the query is executed</span></span>
2. <span data-ttu-id="47e01-109">Das Ergebnis wird an den Datenbankanbieter übergeben.</span><span class="sxs-lookup"><span data-stu-id="47e01-109">The result is passed to the database provider</span></span>
   1. <span data-ttu-id="47e01-110">Der Datenbankanbieter ermittelt, welche Teile der Abfrage in der Datenbank ausgewertet werden können.</span><span class="sxs-lookup"><span data-stu-id="47e01-110">The database provider identifies which parts of the query can be evaluated in the database</span></span>
   2. <span data-ttu-id="47e01-111">Diese Teile der Abfrage werden in die datenbankspezifische Abfragesprache übersetzt, z.B. SQL für relationale Datenbanken.</span><span class="sxs-lookup"><span data-stu-id="47e01-111">These parts of the query are translated to database specific query language (for example, SQL for a relational database)</span></span>
   3. <span data-ttu-id="47e01-112">Eine Abfrage wird an die Datenbank gesendet, und das Resultset wird zurückgegeben (Ergebnisse sind Datenbankwerte, keine Entitätsinstanzen).</span><span class="sxs-lookup"><span data-stu-id="47e01-112">A query is sent to the database and the result set returned (results are values from the database, not entity instances)</span></span>
3. <span data-ttu-id="47e01-113">Für jedes Element im Resultset wird Folgendes ausgeführt:</span><span class="sxs-lookup"><span data-stu-id="47e01-113">For each item in the result set</span></span>
   1. <span data-ttu-id="47e01-114">Wenn es sich um eine Überwachungsabfrage handelt, überprüft EF, ob die Daten eine Entität darstellen, die bereits in der Änderungsnachverfolgung für die Kontextinstanz vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="47e01-114">If this is a tracking query, EF checks if the data represents an entity already in the change tracker for the context instance</span></span>
      * <span data-ttu-id="47e01-115">Wenn dies der Fall ist, wird die vorhandene Entität zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="47e01-115">If so, the existing entity is returned</span></span>
      * <span data-ttu-id="47e01-116">Wenn dies nicht der Fall ist, wird eine neue Entität erstellt, die Änderungsnachverfolgung wird eingerichtet, und die neue Entität wird zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="47e01-116">If not, a new entity is created, change tracking is setup, and the new entity is returned</span></span>
   2. <span data-ttu-id="47e01-117">Wenn es sich um eine nicht nachverfolgungsbezogene Abfrage handelt, wird immer eine neue Entität erstellt und zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="47e01-117">If this is a no-tracking query, then a new entity is always created and returned</span></span>

## <a name="when-queries-are-executed"></a><span data-ttu-id="47e01-118">Ausführen von Abfragen</span><span class="sxs-lookup"><span data-stu-id="47e01-118">When queries are executed</span></span>

<span data-ttu-id="47e01-119">Wenn Sie LINQ-Operatoren aufrufen, erstellen Sie einfach eine speicherinterne Darstellung der Abfrage.</span><span class="sxs-lookup"><span data-stu-id="47e01-119">When you call LINQ operators, you are simply building up an in-memory representation of the query.</span></span> <span data-ttu-id="47e01-120">Die Abfrage wird nur an die Datenbank gesendet, wenn die Ergebnisse verarbeitet werden.</span><span class="sxs-lookup"><span data-stu-id="47e01-120">The query is only sent to the database when the results are consumed.</span></span>

<span data-ttu-id="47e01-121">Dies sind die üblichsten Vorgänge, aufgrund derer die Abfrage wird an die Datenbank gesendet wird:</span><span class="sxs-lookup"><span data-stu-id="47e01-121">The most common operations that result in the query being sent to the database are:</span></span>

* <span data-ttu-id="47e01-122">Durchlaufen die Ergebnisse in einer `for`-Schleife</span><span class="sxs-lookup"><span data-stu-id="47e01-122">Iterating the results in a `for` loop</span></span>
* <span data-ttu-id="47e01-123">Verwenden eines Operators wie `ToList`, `ToArray`, `Single`, `Count` oder die äquivalenten asynchronen Überladungen</span><span class="sxs-lookup"><span data-stu-id="47e01-123">Using an operator such as `ToList`, `ToArray`, `Single`, `Count` or the equivalent async overloads</span></span>

> [!WARNING]  
> <span data-ttu-id="47e01-124">**Überprüfen Sie Benutzereingaben immer:** Zwar schützt EF Core mithilfe von Parametern und Escapezeichen für Literale in Abfragen vor Angriffen durch Einschleusung von SQL-Befehlen, jedoch werden Eingaben nicht überprüft.</span><span class="sxs-lookup"><span data-stu-id="47e01-124">**Always validate user input:** While EF Core protects against SQL injection attacks by using parameters and escaping literals in queries, it does not validate inputs.</span></span> <span data-ttu-id="47e01-125">Geeignete Validierung gemäß den Anforderungen der Anwendung sollte erfolgen, bevor Werte aus nicht vertrauenswürdigen Quellen in LINQ-Abfragen verwendet, Entitätseigenschaften zugewiesen oder an andere EF Core-APIs übergeben werden.</span><span class="sxs-lookup"><span data-stu-id="47e01-125">Appropriate validation, per the application's requirements, should be performed before values from un-trusted sources are used in LINQ queries, assigned to entity properties, or passed to other EF Core APIs.</span></span> <span data-ttu-id="47e01-126">Dies schließt alle Benutzereingaben ein, mit denen Abfragen dynamisch erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="47e01-126">This includes any user input used to dynamically construct queries.</span></span> <span data-ttu-id="47e01-127">Wenn Sie Benutzereingaben zum Erstellen von Ausdrücken zulassen, müssen Sie selbst bei LINQ sicherstellen, dass nur beabsichtigte Ausdrücke erstellt werden können.</span><span class="sxs-lookup"><span data-stu-id="47e01-127">Even when using LINQ, if you are accepting user input to build expressions, you need to make sure that only intended expressions can be constructed.</span></span>
