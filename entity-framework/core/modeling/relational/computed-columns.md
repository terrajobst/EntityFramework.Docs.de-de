---
title: Berechnete Spalten – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9d81f06-805d-45c9-97c2-3546df654829
uid: core/modeling/relational/computed-columns
ms.openlocfilehash: b88efdf69e5100e4eff55f3a41925d2d8e7c3178
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993952"
---
# <a name="computed-columns"></a><span data-ttu-id="05564-102">Berechnete Spalten</span><span class="sxs-lookup"><span data-stu-id="05564-102">Computed Columns</span></span>

> [!NOTE]  
> <span data-ttu-id="05564-103">Die Konfiguration in diesem Abschnitt gilt allgemein für relationale Datenbanken.</span><span class="sxs-lookup"><span data-stu-id="05564-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="05564-104">Die hier gezeigten Erweiterungsmethoden werden verfügbar, wenn Sie einen relationalen Datenbankanbieter installieren (aufgrund des gemeinsam genutzten Pakets *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="05564-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="05564-105">Eine berechnete Spalte ist eine Spalte, deren Wert in der Datenbank berechnet wird.</span><span class="sxs-lookup"><span data-stu-id="05564-105">A computed column is a column whose value is calculated in the database.</span></span> <span data-ttu-id="05564-106">Eine berechnete Spalte kann andere Spalten in der Tabelle verwenden, um den Wert zu berechnen.</span><span class="sxs-lookup"><span data-stu-id="05564-106">A computed column can use other columns in the table to calculate its value.</span></span>

## <a name="conventions"></a><span data-ttu-id="05564-107">Konventionen</span><span class="sxs-lookup"><span data-stu-id="05564-107">Conventions</span></span>

<span data-ttu-id="05564-108">Gemäß der Konvention werden berechnete Spalten nicht im Modell erstellt.</span><span class="sxs-lookup"><span data-stu-id="05564-108">By convention, computed columns are not created in the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="05564-109">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="05564-109">Data Annotations</span></span>

<span data-ttu-id="05564-110">Berechnete Spalten können nicht mit Datenanmerkungen konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="05564-110">Computed columns can not be configured with Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="05564-111">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="05564-111">Fluent API</span></span>

<span data-ttu-id="05564-112">Sie können die Fluent-API verwenden, um anzugeben, dass eine Eigenschaft an eine berechnete Spalte zugeordnet werden soll.</span><span class="sxs-lookup"><span data-stu-id="05564-112">You can use the Fluent API to specify that a property should map to a computed column.</span></span>

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
