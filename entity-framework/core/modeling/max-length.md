---
title: "Maximale Länge - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c39c5d43-018d-48b8-94f2-b8bc7c686c69
ms.technology: entity-framework-core
uid: core/modeling/max-length
ms.openlocfilehash: 7325c0c3328477473392bf9e7c82f1696bb4f424
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
---
# <a name="maximum-length"></a><span data-ttu-id="fa13b-102">Maximale Länge</span><span class="sxs-lookup"><span data-stu-id="fa13b-102">Maximum Length</span></span>

<span data-ttu-id="fa13b-103">Konfigurieren einer maximalen Länge von enthält einen Hinweis für den Datenspeicher über den entsprechenden Datentyp, der für eine bestimmte Eigenschaft verwendet.</span><span class="sxs-lookup"><span data-stu-id="fa13b-103">Configuring a maximum length provides a hint to the data store about the appropriate data type to use for a given property.</span></span> <span data-ttu-id="fa13b-104">Maximale Länge gilt nur für Arraytypen, z. B. `string` und `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="fa13b-104">Maximum length only applies to array data types, such as `string` and `byte[]`.</span></span>

> [!NOTE]  
> <span data-ttu-id="fa13b-105">Entity Framework führt keine Validierung maximale Länge, die vor der Übergabe von Daten an den Anbieter aus.</span><span class="sxs-lookup"><span data-stu-id="fa13b-105">Entity Framework does not do any validation of maximum length before passing data to the provider.</span></span> <span data-ttu-id="fa13b-106">Es liegt im Ermessen der Anbieter oder Datenspeicher So überprüfen Sie bei Bedarf.</span><span class="sxs-lookup"><span data-stu-id="fa13b-106">It is up to the provider or data store to validate if appropriate.</span></span> <span data-ttu-id="fa13b-107">Z. B. unterbindet für SQL Server, überschreiten die maximale Länge in den Datentyp der zugrunde liegende Spalte eine Ausnahme führt nicht zu speichernden überschüssige Daten.</span><span class="sxs-lookup"><span data-stu-id="fa13b-107">For example, when targeting SQL Server, exceeding the maximum length will result in an exception as the data type of the underlying column will not allow excess data to be stored.</span></span>

## <a name="conventions"></a><span data-ttu-id="fa13b-108">Konventionen</span><span class="sxs-lookup"><span data-stu-id="fa13b-108">Conventions</span></span>

<span data-ttu-id="fa13b-109">Gemäß der Konvention wird bis zu der Datenbankanbieter auf einen entsprechenden Datentyp für die Eigenschaften angezeigt.</span><span class="sxs-lookup"><span data-stu-id="fa13b-109">By convention, it is left up to the database provider to choose an appropriate data type for properties.</span></span> <span data-ttu-id="fa13b-110">Für Eigenschaften, die eine Länge haben, wird der Datenbankanbieter im Allgemeinen einen Datentyp auswählen, der die längste Länge der Daten zulässt.</span><span class="sxs-lookup"><span data-stu-id="fa13b-110">For properties that have a length, the database provider will generally choose a data type that allows for the longest length of data.</span></span> <span data-ttu-id="fa13b-111">Microsoft SQL Server wird verwendet, z. B. `nvarchar(max)` für `string` Eigenschaften (oder `nvarchar(450)` , wenn die Spalte als Schlüssel verwendet wird).</span><span class="sxs-lookup"><span data-stu-id="fa13b-111">For example, Microsoft SQL Server will use `nvarchar(max)` for `string` properties (or `nvarchar(450)` if the column is used as a key).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="fa13b-112">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="fa13b-112">Data Annotations</span></span>

<span data-ttu-id="fa13b-113">Sie können die Datenanmerkungen verwenden, so konfigurieren Sie eine maximale Länge für eine Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="fa13b-113">You can use the Data Annotations to configure a maximum length for a property.</span></span> <span data-ttu-id="fa13b-114">In diesem Beispiel als Ziel in SQL Server dies würde die `nvarchar(500)` -Datentyp verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="fa13b-114">In this example, targeting SQL Server this would result in the `nvarchar(500)` data type being used.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/MaxLength.cs?highlight=4)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    [MaxLength(500)]
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="fa13b-115">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="fa13b-115">Fluent API</span></span>

<span data-ttu-id="fa13b-116">Sie können die Fluent-API verwenden, so konfigurieren Sie eine maximale Länge für eine Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="fa13b-116">You can use the Fluent API to configure a maximum length for a property.</span></span> <span data-ttu-id="fa13b-117">In diesem Beispiel als Ziel in SQL Server dies würde die `nvarchar(500)` -Datentyp verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="fa13b-117">In this example, targeting SQL Server this would result in the `nvarchar(500)` data type being used.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/MaxLength.cs?highlight=7,8,9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.Url)
            .HasMaxLength(500);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
