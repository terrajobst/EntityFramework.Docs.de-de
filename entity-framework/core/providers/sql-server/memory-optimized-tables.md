---
title: Anbieter - Memory-Optimized Tables - EF Kernkomponente der Microsoft SQL Server-Datenbank
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
ms.technology: entity-framework-core
uid: core/providers/sql-server/memory-optimized-tables
ms.openlocfilehash: 83a0decffc5946d036903b8b8add59f0ea31b21f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052640"
---
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a><span data-ttu-id="a6e18-102">Speicheroptimierte Tabellen unterstützen, Datenbank-Anbieter für SQL Server EF Core</span><span class="sxs-lookup"><span data-stu-id="a6e18-102">Memory-Optimized Tables support in SQL Server EF Core Database Provider</span></span>

> [!NOTE]  
>
> <span data-ttu-id="a6e18-103">Diese Funktion wurde in der EF Core 1.1 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="a6e18-103">This feature was introduced in EF Core 1.1.</span></span>

<span data-ttu-id="a6e18-104">[Speicheroptimierte Tabellen](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) sind eine Funktion von SQL Server, auf dem die gesamte Tabelle im Arbeitsspeicher gespeichert ist.</span><span class="sxs-lookup"><span data-stu-id="a6e18-104">[Memory-Optimized Tables](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) are a feature of SQL Server where the entire table resides in memory.</span></span> <span data-ttu-id="a6e18-105">Eine zweite Kopie der Tabellendaten wird auf Festplatte gespeichert, aber nur zu Dauerhaftigkeitszwecken.</span><span class="sxs-lookup"><span data-stu-id="a6e18-105">A second copy of the table data is maintained on disk, but only for durability purposes.</span></span> <span data-ttu-id="a6e18-106">Daten in speicheroptimierten Tabellen werden nur während der Datenbankwiederherstellung vom Datenträger gelesen.</span><span class="sxs-lookup"><span data-stu-id="a6e18-106">Data in memory-optimized tables is only read from disk during database recovery.</span></span> <span data-ttu-id="a6e18-107">Dies ist beispielsweise nach einem Neustart des Servers der Fall.</span><span class="sxs-lookup"><span data-stu-id="a6e18-107">For example, after a server restart.</span></span>

## <a name="configuring-a-memory-optimized-table"></a><span data-ttu-id="a6e18-108">Konfigurieren einer speicheroptimierten Tabellenstatus</span><span class="sxs-lookup"><span data-stu-id="a6e18-108">Configuring a memory-optimized table</span></span>

<span data-ttu-id="a6e18-109">Sie können angeben, dass die Tabelle, der eine Entität zugeordnet ist speicheroptimiert ist.</span><span class="sxs-lookup"><span data-stu-id="a6e18-109">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="a6e18-110">Bei Verwendung von EF Core erstellen und Verwalten einer Datenbank basierend auf Ihrem Modell (entweder mit Migrationen oder `Database.EnsureCreated()`), wird eine Speicheroptimierte Tabelle für diese Entitäten erstellt.</span><span class="sxs-lookup"><span data-stu-id="a6e18-110">When using EF Core to create and maintain a database based on your model (either with migrations or `Database.EnsureCreated()`), a memory-optimized table will be created for these entities.</span></span>

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .ForSqlServerIsMemoryOptimized();
}
```
