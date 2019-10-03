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
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a>Unterstützung für Speicher optimierte Tabellen in SQL Server EF Core-Datenbankanbieter

[Speicher optimierte Tabellen](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) sind eine Funktion von SQL Server, in der sich die gesamte Tabelle im Arbeitsspeicher befindet. Eine zweite Kopie der Tabellendaten wird auf dem Datenträger beibehalten, aber nur aus Gründen der Dauerhaftigkeits Dauer. Daten in Speicher optimierten Tabellen werden nur während der Daten Bank Wiederherstellung vom Datenträger gelesen. Beispielsweise nach einem Server Neustart.

## <a name="configuring-a-memory-optimized-table"></a>Konfigurieren einer Speicher optimierten Tabelle

Sie können angeben, dass es sich bei der einer Entität zugeordneten Tabelle um eine speicheroptimierte Tabelle handelt. Beim Einsatz von EF Core zum Erstellen und Verwalten einer auf Ihrem Modell basierenden Datenbank (entweder mit Migrationen oder `Database.EnsureCreated()`) wird eine speicheroptimierte Tabelle für diese Entitäten erstellt.

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .ForSqlServerIsMemoryOptimized();
}
```
