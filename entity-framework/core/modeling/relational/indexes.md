---
title: Indizes – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
uid: core/modeling/relational/indexes
ms.openlocfilehash: 605b30ce710d9034deab97f695496ec66a576565
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993216"
---
# <a name="indexes"></a><span data-ttu-id="25b71-102">Indizes</span><span class="sxs-lookup"><span data-stu-id="25b71-102">Indexes</span></span>

> [!NOTE]  
> <span data-ttu-id="25b71-103">Die Konfiguration in diesem Abschnitt gilt allgemein für relationale Datenbanken.</span><span class="sxs-lookup"><span data-stu-id="25b71-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="25b71-104">Die hier gezeigten Erweiterungsmethoden werden verfügbar, wenn Sie einen relationalen Datenbankanbieter installieren (aufgrund des gemeinsam genutzten Pakets *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="25b71-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="25b71-105">Das gleiche Konzept als Index in der Kern von Entity Framework ist ein Index in einer relationalen Datenbank zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="25b71-105">An index in a relational database maps to the same concept as an index in the core of Entity Framework.</span></span>

## <a name="conventions"></a><span data-ttu-id="25b71-106">Konventionen</span><span class="sxs-lookup"><span data-stu-id="25b71-106">Conventions</span></span>

<span data-ttu-id="25b71-107">Indizes werden gemäß der Konvention benannt `IX_<type name>_<property name>`.</span><span class="sxs-lookup"><span data-stu-id="25b71-107">By convention, indexes are named `IX_<type name>_<property name>`.</span></span> <span data-ttu-id="25b71-108">Bei zusammengesetzten Indizes `<property name>` wird eine Unterstrich getrennt Liste von Eigenschaftsnamen.</span><span class="sxs-lookup"><span data-stu-id="25b71-108">For composite indexes `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="25b71-109">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="25b71-109">Data Annotations</span></span>

<span data-ttu-id="25b71-110">Indizes können nicht mithilfe von Datenanmerkungen konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="25b71-110">Indexes can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="25b71-111">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="25b71-111">Fluent API</span></span>

<span data-ttu-id="25b71-112">Sie können die Fluent-API verwenden, so konfigurieren Sie den Namen eines Indexes.</span><span class="sxs-lookup"><span data-stu-id="25b71-112">You can use the Fluent API to configure the name of an index.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexName.cs?name=Model&highlight=9)]

<span data-ttu-id="25b71-113">Sie können auch einen Filter angeben.</span><span class="sxs-lookup"><span data-stu-id="25b71-113">You can also specify a filter.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexFilter.cs?name=Model&highlight=9)]

<span data-ttu-id="25b71-114">Wenn durch die Verwendung des SQL Server-Ressourcenanbieters EF Filtern eine 'IS NOT NULL"für alle auf NULL festlegbare Spalten, die Teil eines eindeutigen Indexes sind.</span><span class="sxs-lookup"><span data-stu-id="25b71-114">When using the SQL Server provider EF adds a 'IS NOT NULL' filter for all nullable columns that are part of a unique index.</span></span> <span data-ttu-id="25b71-115">Diese Konvention überschreiben, Sie angeben können, ein `null` Wert.</span><span class="sxs-lookup"><span data-stu-id="25b71-115">To override this convention you can supply a `null` value.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexNoFilter.cs?name=Model&highlight=10)]
