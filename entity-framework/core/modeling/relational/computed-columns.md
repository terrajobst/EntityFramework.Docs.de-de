---
title: Berechnete Spalten - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e9d81f06-805d-45c9-97c2-3546df654829
ms.technology: entity-framework-core
uid: core/modeling/relational/computed-columns
ms.openlocfilehash: 95312504286bd34cc666b5a21273835c4b35d379
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
---
# <a name="computed-columns"></a>Berechnete Spalten

> [!NOTE]  
> Die Konfiguration in diesem Abschnitt ist im Allgemeinen gilt für relationale Datenbanken. Die Erweiterungsmethoden, die hier gezeigten werden verfügbar, wenn Sie einen relationale Datenbank-Anbieter installieren (aufgrund der freigegebenen *Microsoft.EntityFrameworkCore.Relational* Paket).

Eine berechnete Spalte ist eine Spalte in der Datenbank, deren Wert berechnet wird. Eine berechnete Spalte kann andere Spalten in der Tabelle verwenden, um seinen Wert zu berechnen.

## <a name="conventions"></a>Konventionen

Gemäß der Konvention werden berechnete Spalten nicht im Modell erstellt.

## <a name="data-annotations"></a>Datenanmerkungen

Berechnete Spalten können nicht mit Datenanmerkungen konfiguriert werden.

## <a name="fluent-api"></a>Fluent-API

Sie können die Fluent-API verwenden, um anzugeben, dass eine Eigenschaft einer berechneten Spalte zugeordnet werden sollen.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/ComputedColumn.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Person> People { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Person>()
            .Property(p => p.DisplayName)
            .HasComputedColumnSql("[LastName] + ', ' + [FirstName]");
    }
}

public class Person
{
    public int PersonId { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public string DisplayName { get; set; }
}
```
