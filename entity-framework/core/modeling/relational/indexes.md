---
title: Indizes - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
ms.technology: entity-framework-core
uid: core/modeling/relational/indexes
ms.openlocfilehash: f577fccfefc6908edf2ac47ae630323d7a9f5f2b
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="indexes"></a><span data-ttu-id="4501e-102">Indizes</span><span class="sxs-lookup"><span data-stu-id="4501e-102">Indexes</span></span>

> [!NOTE]  
> <span data-ttu-id="4501e-103">Die Konfiguration in diesem Abschnitt gilt allgemein für relationale Datenbanken.</span><span class="sxs-lookup"><span data-stu-id="4501e-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="4501e-104">Die hier gezeigten Erweiterungsmethoden werden verfügbar, wenn Sie einen relationalen Datenbankanbieter installieren (aufgrund des gemeinsam genutzten Pakets *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="4501e-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="4501e-105">Ein Index in einer relationalen Datenbank ordnet dasselbe Konzept wie ein Index im Kern des Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="4501e-105">An index in a relational database maps to the same concept as an index in the core of Entity Framework.</span></span>

## <a name="conventions"></a><span data-ttu-id="4501e-106">Konventionen</span><span class="sxs-lookup"><span data-stu-id="4501e-106">Conventions</span></span>

<span data-ttu-id="4501e-107">Indizes werden gemäß der Konvention benannt `IX_<type name>_<property name>`.</span><span class="sxs-lookup"><span data-stu-id="4501e-107">By convention, indexes are named `IX_<type name>_<property name>`.</span></span> <span data-ttu-id="4501e-108">Bei zusammengesetzten Indizes `<property name>` wird eine Unterstrich getrennte Liste von Eigenschaftsnamen.</span><span class="sxs-lookup"><span data-stu-id="4501e-108">For composite indexes `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="4501e-109">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="4501e-109">Data Annotations</span></span>

<span data-ttu-id="4501e-110">Indizes können nicht mithilfe von Datenanmerkungen konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="4501e-110">Indexes can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="4501e-111">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="4501e-111">Fluent API</span></span>

<span data-ttu-id="4501e-112">So konfigurieren Sie den Namen eines Indexes können Sie die Fluent-API verwenden.</span><span class="sxs-lookup"><span data-stu-id="4501e-112">You can use the Fluent API to configure the name of an index.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexName.cs?name=Model&highlight=9)]

<span data-ttu-id="4501e-113">Sie können auch einen Filter angeben.</span><span class="sxs-lookup"><span data-stu-id="4501e-113">You can also specify a filter.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexFilter.cs?name=Model&highlight=9)]

<span data-ttu-id="4501e-114">Fügt mithilfe des SQL Server-Anbieters EF filtern Sie bei einem "IS NOT NULL" für alle Spalten, die Teil eines eindeutigen Indexes sind.</span><span class="sxs-lookup"><span data-stu-id="4501e-114">When using the SQL Server provider EF adds a 'IS NOT NULL' filter for all nullable columns that are part of a unique index.</span></span> <span data-ttu-id="4501e-115">Diese Konvention überschreiben, Sie angeben können, ein `null` Wert.</span><span class="sxs-lookup"><span data-stu-id="4501e-115">To override this convention you can supply a `null` value.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexNoFilter.cs?name=Model&highlight=10)]
