---
title: Microsoft SQL Server Datenbankanbieter-Speicher optimierte Tabellen-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/memory-optimized-tables
ms.openlocfilehash: 7383e74b3f83172f9b8e0eaf9bd09d4e187e87f8
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813493"
---
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a><span data-ttu-id="38c10-102">Unterstützung für Speicher optimierte Tabellen in SQL Server EF Core-Datenbankanbieter</span><span class="sxs-lookup"><span data-stu-id="38c10-102">Memory-Optimized Tables support in SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="38c10-103">[Speicher optimierte Tabellen](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) sind eine Funktion von SQL Server, in der sich die gesamte Tabelle im Arbeitsspeicher befindet.</span><span class="sxs-lookup"><span data-stu-id="38c10-103">[Memory-Optimized Tables](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) are a feature of SQL Server where the entire table resides in memory.</span></span> <span data-ttu-id="38c10-104">Eine zweite Kopie der Tabellendaten wird auf dem Datenträger beibehalten, aber nur aus Gründen der Dauerhaftigkeits Dauer.</span><span class="sxs-lookup"><span data-stu-id="38c10-104">A second copy of the table data is maintained on disk, but only for durability purposes.</span></span> <span data-ttu-id="38c10-105">Daten in Speicher optimierten Tabellen werden nur während der Daten Bank Wiederherstellung vom Datenträger gelesen.</span><span class="sxs-lookup"><span data-stu-id="38c10-105">Data in memory-optimized tables is only read from disk during database recovery.</span></span> <span data-ttu-id="38c10-106">Beispielsweise nach einem Server Neustart.</span><span class="sxs-lookup"><span data-stu-id="38c10-106">For example, after a server restart.</span></span>

## <a name="configuring-a-memory-optimized-table"></a><span data-ttu-id="38c10-107">Konfigurieren einer Speicher optimierten Tabelle</span><span class="sxs-lookup"><span data-stu-id="38c10-107">Configuring a memory-optimized table</span></span>

<span data-ttu-id="38c10-108">Sie können angeben, dass es sich bei der einer Entität zugeordneten Tabelle um eine speicheroptimierte Tabelle handelt.</span><span class="sxs-lookup"><span data-stu-id="38c10-108">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="38c10-109">Beim Einsatz von EF Core zum Erstellen und Verwalten einer auf Ihrem Modell basierenden Datenbank (entweder mit Migrationen oder `Database.EnsureCreated()`) wird eine speicheroptimierte Tabelle für diese Entitäten erstellt.</span><span class="sxs-lookup"><span data-stu-id="38c10-109">When using EF Core to create and maintain a database based on your model (either with migrations or `Database.EnsureCreated()`), a memory-optimized table will be created for these entities.</span></span>

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .ForSqlServerIsMemoryOptimized();
}
```
