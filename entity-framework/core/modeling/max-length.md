---
title: Maximale Länge - EF Core
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
ms.locfileid: "26052670"
---
# <a name="maximum-length"></a>Maximale Länge

Konfigurieren einer maximalen Länge von enthält einen Hinweis für den Datenspeicher über den entsprechenden Datentyp, der für eine bestimmte Eigenschaft verwendet. Maximale Länge gilt nur für Arraytypen, z. B. `string` und `byte[]`.

> [!NOTE]  
> Entity Framework führt keine Validierung maximale Länge, die vor der Übergabe von Daten an den Anbieter aus. Es liegt im Ermessen der Anbieter oder Datenspeicher So überprüfen Sie bei Bedarf. Z. B. unterbindet für SQL Server, überschreiten die maximale Länge in den Datentyp der zugrunde liegende Spalte eine Ausnahme führt nicht zu speichernden überschüssige Daten.

## <a name="conventions"></a>Konventionen

Gemäß der Konvention wird bis zu der Datenbankanbieter auf einen entsprechenden Datentyp für die Eigenschaften angezeigt. Für Eigenschaften, die eine Länge haben, wird der Datenbankanbieter im Allgemeinen einen Datentyp auswählen, der die längste Länge der Daten zulässt. Microsoft SQL Server wird verwendet, z. B. `nvarchar(max)` für `string` Eigenschaften (oder `nvarchar(450)` , wenn die Spalte als Schlüssel verwendet wird).

## <a name="data-annotations"></a>Datenanmerkungen

Sie können die Datenanmerkungen verwenden, so konfigurieren Sie eine maximale Länge für eine Eigenschaft. In diesem Beispiel als Ziel in SQL Server dies würde die `nvarchar(500)` -Datentyp verwendet wird.

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/MaxLength.cs?highlight=4)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    [MaxLength(500)]
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a>Fluent-API

Sie können die Fluent-API verwenden, so konfigurieren Sie eine maximale Länge für eine Eigenschaft. In diesem Beispiel als Ziel in SQL Server dies würde die `nvarchar(500)` -Datentyp verwendet wird.

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
