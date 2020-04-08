---
title: Abfragen von Daten – EF Core
author: smitpatel
ms.date: 10/03/2019
ms.assetid: 7c65ec3e-46c8-48f8-8232-9e31f96c277b
uid: core/querying/index
ms.openlocfilehash: 0e1e50d1a3f647d65301552d0a447f9fcae81438
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/07/2020
ms.locfileid: "78413115"
---
# <a name="querying-data"></a><span data-ttu-id="f246d-102">Abfrage von Daten</span><span class="sxs-lookup"><span data-stu-id="f246d-102">Querying Data</span></span>

<span data-ttu-id="f246d-103">Entity Framework Core verwendet Language Integrated Query (LINQ), um Daten von der Datenbank abzufragen.</span><span class="sxs-lookup"><span data-stu-id="f246d-103">Entity Framework Core uses Language Integrated Query (LINQ) to query data from the database.</span></span> <span data-ttu-id="f246d-104">LINQ ermöglicht Ihnen, mit C# (oder Ihrer bevorzugten .NET-Sprache) stark typisierte Abfragen zu schreiben.</span><span class="sxs-lookup"><span data-stu-id="f246d-104">LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries.</span></span> <span data-ttu-id="f246d-105">Dabei werden der abgeleitete Kontext und Entitätsklassen verwendet, um auf Datenbankobjekte zu verweisen.</span><span class="sxs-lookup"><span data-stu-id="f246d-105">It uses your derived context and entity classes to reference database objects.</span></span> <span data-ttu-id="f246d-106">EF Core übergibt eine Darstellung der LINQ-Abfrage an den Datenbankanbieter.</span><span class="sxs-lookup"><span data-stu-id="f246d-106">EF Core passes a representation of the LINQ query to the database provider.</span></span> <span data-ttu-id="f246d-107">Die Datenbankanbieter übersetzen diese dann in die datenbankspezifische Abfragesprache, z. B. SQL für relationale Datenbanken.</span><span class="sxs-lookup"><span data-stu-id="f246d-107">Database providers in turn translate it to database-specific query language (for example, SQL for a relational database).</span></span>

> [!TIP]
> <span data-ttu-id="f246d-108">Das in diesem Artikel verwendete [Beispiel](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) finden Sie auf GitHub.</span><span class="sxs-lookup"><span data-stu-id="f246d-108">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

<span data-ttu-id="f246d-109">Auf den folgenden Ausschnitten werden einige Beispiele für das Ausführen gängiger Aufgaben mit EF Core dargestellt.</span><span class="sxs-lookup"><span data-stu-id="f246d-109">The following snippets show a few examples of how to achieve common tasks with Entity Framework Core.</span></span>

## <a name="loading-all-data"></a><span data-ttu-id="f246d-110">Laden aller Daten</span><span class="sxs-lookup"><span data-stu-id="f246d-110">Loading all data</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#LoadingAllData)]

## <a name="loading-a-single-entity"></a><span data-ttu-id="f246d-111">Laden einer einzelnen Entität</span><span class="sxs-lookup"><span data-stu-id="f246d-111">Loading a single entity</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#LoadingSingleEntity)]

## <a name="filtering"></a><span data-ttu-id="f246d-112">Filterung</span><span class="sxs-lookup"><span data-stu-id="f246d-112">Filtering</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#Filtering)]

## <a name="further-readings"></a><span data-ttu-id="f246d-113">Weiterführende Themen</span><span class="sxs-lookup"><span data-stu-id="f246d-113">Further readings</span></span>

- <span data-ttu-id="f246d-114">Weitere Informationen zu [LINQ-Abfrageausdrücken](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations)</span><span class="sxs-lookup"><span data-stu-id="f246d-114">Learn more about [LINQ query expressions](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations)</span></span>
- <span data-ttu-id="f246d-115">Ausführlichere Informationen zur Verarbeitung einer Abfrage finden Sie unter [Funktionsweise von Abfragen](xref:core/querying/how-query-works).</span><span class="sxs-lookup"><span data-stu-id="f246d-115">For more detailed information on how a query is processed in EF Core, see [How Query Works](xref:core/querying/how-query-works).</span></span>
