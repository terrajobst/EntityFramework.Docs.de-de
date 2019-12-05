---
title: Microsoft SQL Server Datenbankanbieter-Speicher optimierte Tabellen-EF Core
description: Verwenden von Speicher optimierten Tabellen mit dem SQL Server Entity Framework Core-Datenbankanbieter
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/sql-server/memory-optimized-tables
ms.openlocfilehash: a504fb3487aea6dd36abf204a7427095e3d29118
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824648"
---
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a><span data-ttu-id="d955b-103">Unterstützung für Speicher optimierte Tabellen in SQL Server EF Core-Datenbankanbieter</span><span class="sxs-lookup"><span data-stu-id="d955b-103">Memory-Optimized Tables support in SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="d955b-104">[Speicher optimierte Tabellen](/sql/relational-databases/in-memory-oltp/memory-optimized-tables) sind eine Funktion von SQL Server, in der sich die gesamte Tabelle im Arbeitsspeicher befindet.</span><span class="sxs-lookup"><span data-stu-id="d955b-104">[Memory-Optimized Tables](/sql/relational-databases/in-memory-oltp/memory-optimized-tables) are a feature of SQL Server where the entire table resides in memory.</span></span> <span data-ttu-id="d955b-105">Eine zweite Kopie der Tabellendaten wird auf Festplatte gespeichert, aber nur zu Dauerhaftigkeitszwecken.</span><span class="sxs-lookup"><span data-stu-id="d955b-105">A second copy of the table data is maintained on disk, but only for durability purposes.</span></span> <span data-ttu-id="d955b-106">Daten in speicheroptimierten Tabellen werden nur während der Datenbankwiederherstellung vom Datenträger gelesen.</span><span class="sxs-lookup"><span data-stu-id="d955b-106">Data in memory-optimized tables is only read from disk during database recovery.</span></span> <span data-ttu-id="d955b-107">Dies ist beispielsweise nach einem Neustart des Servers der Fall.</span><span class="sxs-lookup"><span data-stu-id="d955b-107">For example, after a server restart.</span></span>

## <a name="configuring-a-memory-optimized-table"></a><span data-ttu-id="d955b-108">Konfigurieren einer Speicher optimierten Tabelle</span><span class="sxs-lookup"><span data-stu-id="d955b-108">Configuring a memory-optimized table</span></span>

<span data-ttu-id="d955b-109">Sie können angeben, dass es sich bei der einer Entität zugeordneten Tabelle um eine speicheroptimierte Tabelle handelt.</span><span class="sxs-lookup"><span data-stu-id="d955b-109">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="d955b-110">Wenn Sie EF Core verwenden, um eine Datenbank basierend auf Ihrem Modell zu erstellen und zu verwalten (entweder mit [Migrationen](xref:core/managing-schemas/migrations/index) oder [ensucreated](/dotnet/api/Microsoft.EntityFrameworkCore.Storage.IDatabaseCreator.EnsureCreated)), wird eine Speicher optimierte Tabelle für diese Entitäten erstellt.</span><span class="sxs-lookup"><span data-stu-id="d955b-110">When using EF Core to create and maintain a database based on your model (either with [migrations](xref:core/managing-schemas/migrations/index) or [EnsureCreated](/dotnet/api/Microsoft.EntityFrameworkCore.Storage.IDatabaseCreator.EnsureCreated)), a memory-optimized table will be created for these entities.</span></span>

[!code-csharp[IsMemoryOptimized](../../../../samples/core/SqlServer/InMemory/InMemoryContext.cs?name=IsMemoryOptimized)]
