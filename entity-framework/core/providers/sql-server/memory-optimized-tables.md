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
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a>Unterstützung für Speicheroptimierte Tabellen in SQL Server-EF Core-Datenbankanbieter

> [!NOTE]  
>
> Dieses Feature wurde in EF Core 1.1 eingeführt.

[Speicheroptimierte Tabellen](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) sind ein Feature von SQL Server, in dem die gesamte Tabelle im Speicher befindet. Eine zweite Kopie der Tabellendaten wird auf dem Datenträger, aber nur zu beibehalten. Daten in speicheroptimierten Tabellen ist nur während der datenbankwiederherstellung vom Datenträger gelesen. Z. B. nach einem Serverneustart.

## <a name="configuring-a-memory-optimized-table"></a>Konfigurieren einer speicheroptimierten Tabelle

Sie können angeben, dass es sich bei der einer Entität zugeordneten Tabelle um eine speicheroptimierte Tabelle handelt. Beim Einsatz von EF Core zum Erstellen und Verwalten einer auf Ihrem Modell basierenden Datenbank (entweder mit Migrationen oder `Database.EnsureCreated()`) wird eine speicheroptimierte Tabelle für diese Entitäten erstellt.

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .ForSqlServerIsMemoryOptimized();
}
```
