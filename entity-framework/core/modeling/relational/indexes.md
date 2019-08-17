---
title: Indizes-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
uid: core/modeling/relational/indexes
ms.openlocfilehash: dfada7446f812f3c277572cc1338441272e8f448
ms.sourcegitcommit: 7b7f774a5966b20d2aed5435a672a1edbe73b6fb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2019
ms.locfileid: "69565356"
---
# <a name="indexes"></a><span data-ttu-id="7630f-102">Indizes</span><span class="sxs-lookup"><span data-stu-id="7630f-102">Indexes</span></span>

> [!NOTE]  
> <span data-ttu-id="7630f-103">Die Konfiguration in diesem Abschnitt gilt allgemein für relationale Datenbanken.</span><span class="sxs-lookup"><span data-stu-id="7630f-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="7630f-104">Die hier gezeigten Erweiterungsmethoden werden verfügbar, wenn Sie einen relationalen Datenbankanbieter installieren (aufgrund des gemeinsam genutzten Pakets *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="7630f-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="7630f-105">Ein Index in einer relationalen Datenbank wird dem gleichen Konzept zugeordnet wie ein Index im Kern der Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="7630f-105">An index in a relational database maps to the same concept as an index in the core of Entity Framework.</span></span>

## <a name="conventions"></a><span data-ttu-id="7630f-106">Konventionen</span><span class="sxs-lookup"><span data-stu-id="7630f-106">Conventions</span></span>

<span data-ttu-id="7630f-107">Gemäß der Konvention werden Indizes benannt `IX_<type name>_<property name>`.</span><span class="sxs-lookup"><span data-stu-id="7630f-107">By convention, indexes are named `IX_<type name>_<property name>`.</span></span> <span data-ttu-id="7630f-108">Bei zusammengesetzten `<property name>` Indizes wird eine durch Trennzeichen getrennte Liste mit Eigenschaftsnamen.</span><span class="sxs-lookup"><span data-stu-id="7630f-108">For composite indexes `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="7630f-109">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="7630f-109">Data Annotations</span></span>

<span data-ttu-id="7630f-110">Indizes können nicht mithilfe von Daten Anmerkungen konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="7630f-110">Indexes can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="7630f-111">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="7630f-111">Fluent API</span></span>

<span data-ttu-id="7630f-112">Sie können die fließende API verwenden, um den Namen eines Indexes zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="7630f-112">You can use the Fluent API to configure the name of an index.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexName.cs?name=Model&highlight=9)]

<span data-ttu-id="7630f-113">Sie können auch einen Filter angeben.</span><span class="sxs-lookup"><span data-stu-id="7630f-113">You can also specify a filter.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexFilter.cs?name=Model&highlight=9)]

<span data-ttu-id="7630f-114">Bei Verwendung des SQL Server-Anbieters fügt EF einen ' is not NULL '-Filter für alle Spalten hinzu, die NULL-Werte zulassen, die Teil eines eindeutigen Indexes sind.</span><span class="sxs-lookup"><span data-stu-id="7630f-114">When using the SQL Server provider EF adds a 'IS NOT NULL' filter for all nullable columns that are part of a unique index.</span></span> <span data-ttu-id="7630f-115">Um diese Konvention zu überschreiben, können `null` Sie einen Wert angeben.</span><span class="sxs-lookup"><span data-stu-id="7630f-115">To override this convention you can supply a `null` value.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexNoFilter.cs?name=Model&highlight=10)]

### <a name="include-columns-in-sql-server-indexes"></a><span data-ttu-id="7630f-116">Einschließen von Spalten in SQL Server Indizes</span><span class="sxs-lookup"><span data-stu-id="7630f-116">Include Columns in SQL Server Indexes</span></span>

<span data-ttu-id="7630f-117">Sie können [Indizes mit inklusivspalten](https://docs.microsoft.com/sql/relational-databases/indexes/create-indexes-with-included-columns) konfigurieren, um die Abfrageleistung erheblich zu verbessern, wenn alle Spalten in der Abfrage als Schlüssel-oder nicht Schlüssel Spalten in den Index aufgenommen werden.</span><span class="sxs-lookup"><span data-stu-id="7630f-117">You can configure [indexes with included columns](https://docs.microsoft.com/sql/relational-databases/indexes/create-indexes-with-included-columns) to significantly improve query performance when all columns in the query are included in the index as key or non-key columns.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/ForSqlServerHasIndex.cs?name=Model)]
