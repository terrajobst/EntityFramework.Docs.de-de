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
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a>Unterstützung für Speicher optimierte Tabellen in SQL Server EF Core-Datenbankanbieter

[Speicher optimierte Tabellen](/sql/relational-databases/in-memory-oltp/memory-optimized-tables) sind eine Funktion von SQL Server, in der sich die gesamte Tabelle im Arbeitsspeicher befindet. Eine zweite Kopie der Tabellendaten wird auf Festplatte gespeichert, aber nur zu Dauerhaftigkeitszwecken. Daten in speicheroptimierten Tabellen werden nur während der Datenbankwiederherstellung vom Datenträger gelesen. Dies ist beispielsweise nach einem Neustart des Servers der Fall.

## <a name="configuring-a-memory-optimized-table"></a>Konfigurieren einer Speicher optimierten Tabelle

Sie können angeben, dass es sich bei der einer Entität zugeordneten Tabelle um eine speicheroptimierte Tabelle handelt. Wenn Sie EF Core verwenden, um eine Datenbank basierend auf Ihrem Modell zu erstellen und zu verwalten (entweder mit [Migrationen](xref:core/managing-schemas/migrations/index) oder [ensucreated](/dotnet/api/Microsoft.EntityFrameworkCore.Storage.IDatabaseCreator.EnsureCreated)), wird eine Speicher optimierte Tabelle für diese Entitäten erstellt.

[!code-csharp[IsMemoryOptimized](../../../../samples/core/SqlServer/InMemory/InMemoryContext.cs?name=IsMemoryOptimized)]
