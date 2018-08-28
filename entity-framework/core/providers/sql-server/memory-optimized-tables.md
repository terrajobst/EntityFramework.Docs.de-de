---
title: Microsoft SQL Server-Datenbank-Anbieter - Memory-Optimized Tables – Entity Framework Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/memory-optimized-tables
ms.openlocfilehash: 63d2cbf8b69e4f1945ad60914e284fb42c48e8db
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995801"
---
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a><span data-ttu-id="7575b-102">Unterstützung für Speicheroptimierte Tabellen in SQL Server-EF Core-Datenbankanbieter</span><span class="sxs-lookup"><span data-stu-id="7575b-102">Memory-Optimized Tables support in SQL Server EF Core Database Provider</span></span>

> [!NOTE]  
>
> <span data-ttu-id="7575b-103">Dieses Feature wurde in EF Core 1.1 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="7575b-103">This feature was introduced in EF Core 1.1.</span></span>

<span data-ttu-id="7575b-104">[Speicheroptimierte Tabellen](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) sind ein Feature von SQL Server, in dem die gesamte Tabelle im Speicher befindet.</span><span class="sxs-lookup"><span data-stu-id="7575b-104">[Memory-Optimized Tables](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) are a feature of SQL Server where the entire table resides in memory.</span></span> <span data-ttu-id="7575b-105">Eine zweite Kopie der Tabellendaten wird auf dem Datenträger, aber nur zu beibehalten.</span><span class="sxs-lookup"><span data-stu-id="7575b-105">A second copy of the table data is maintained on disk, but only for durability purposes.</span></span> <span data-ttu-id="7575b-106">Daten in speicheroptimierten Tabellen ist nur während der datenbankwiederherstellung vom Datenträger gelesen.</span><span class="sxs-lookup"><span data-stu-id="7575b-106">Data in memory-optimized tables is only read from disk during database recovery.</span></span> <span data-ttu-id="7575b-107">Z. B. nach einem Serverneustart.</span><span class="sxs-lookup"><span data-stu-id="7575b-107">For example, after a server restart.</span></span>

## <a name="configuring-a-memory-optimized-table"></a><span data-ttu-id="7575b-108">Konfigurieren einer speicheroptimierten Tabelle</span><span class="sxs-lookup"><span data-stu-id="7575b-108">Configuring a memory-optimized table</span></span>

<span data-ttu-id="7575b-109">Sie können angeben, dass es sich bei der einer Entität zugeordneten Tabelle um eine speicheroptimierte Tabelle handelt.</span><span class="sxs-lookup"><span data-stu-id="7575b-109">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="7575b-110">Beim Einsatz von EF Core zum Erstellen und Verwalten einer auf Ihrem Modell basierenden Datenbank (entweder mit Migrationen oder `Database.EnsureCreated()`) wird eine speicheroptimierte Tabelle für diese Entitäten erstellt.</span><span class="sxs-lookup"><span data-stu-id="7575b-110">When using EF Core to create and maintain a database based on your model (either with migrations or `Database.EnsureCreated()`), a memory-optimized table will be created for these entities.</span></span>

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .ForSqlServerIsMemoryOptimized();
}
```
