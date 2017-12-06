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
---
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a>Speicheroptimierte Tabellen unterstützen, Datenbank-Anbieter für SQL Server EF Core

> [!NOTE]  
>
> Diese Funktion wurde in der EF Core 1.1 eingeführt.

[Speicheroptimierte Tabellen](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) sind eine Funktion von SQL Server, auf dem die gesamte Tabelle im Arbeitsspeicher gespeichert ist. Eine zweite Kopie der Tabellendaten wird auf Festplatte gespeichert, aber nur zu Dauerhaftigkeitszwecken. Daten in speicheroptimierten Tabellen werden nur während der Datenbankwiederherstellung vom Datenträger gelesen. Dies ist beispielsweise nach einem Neustart des Servers der Fall.

## <a name="configuring-a-memory-optimized-table"></a>Konfigurieren einer speicheroptimierten Tabellenstatus

Sie können angeben, dass die Tabelle, der eine Entität zugeordnet ist speicheroptimiert ist. Bei Verwendung von EF Core erstellen und Verwalten einer Datenbank basierend auf Ihrem Modell (entweder mit Migrationen oder `Database.EnsureCreated()`), wird eine Speicheroptimierte Tabelle für diese Entitäten erstellt.

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .ForSqlServerIsMemoryOptimized();
}
```
