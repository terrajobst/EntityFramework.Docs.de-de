---
title: "Erstellen eines Modells – EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 88253ff3-174e-485c-b3f8-768243d01ee1
ms.technology: entity-framework-core
uid: core/modeling/index
ms.openlocfilehash: 1ad0f6891fbc8ba2e4d102cc9997f053a9dddb66
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
---
# <a name="creating-a-model"></a>Erstellen eines Modells

Entity Framework verwendet eine Reihe von Konventionen, um ein Modell basierend auf der Anordnung Ihrer Entitätsklassen zu erstellen. Sie können zusätzliche Konfigurationen angeben, um die gemäß den Konventionen gewonnenen Ergebnisse zu ergänzen und/oder zu überschreiben.

Dieser Artikel behandelt die Konfiguration, die auf ein Modell für einen beliebigen Datenspeicher und auf jede relationale Datenbank angewendet werden kann. Anbieter können auch eine Konfiguration aktivieren, die speziell für einen bestimmten Datenspeicher gilt. Dokumentation zu anbieterspezifischen Konfigurationen finden Sie im Abschnitt [Datenbankanbieter](../providers/index.md).

> [!TIP]  
> Das in diesem Artikel verwendete [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples) finden Sie auf GitHub.

## <a name="methods-of-configuration"></a>Konfigurationsmethoden

### <a name="fluent-api"></a>Fluent-API

Sie können die `OnModelCreating`-Methode in Ihrem abgeleiteten Kontext überschreiben und mit `ModelBuilder API` Ihr Modell konfigurieren. Dies ist die wirksamste Konfigurationsmethode und erlaubt die Angabe der Konfiguration, ohne die Entitätsklassen zu verändern. Die Fluent-API-Konfiguration hat die höchste Priorität und überschreibt Konventionen und Datenanmerkungen.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Required.cs?range=5-15&highlight=5-10)] -->

``` csharp
    class MyContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }

        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            modelBuilder.Entity<Blog>()
                .Property(b => b.Url)
                .IsRequired();
        }
    }
```

### <a name="data-annotations"></a>Datenanmerkungen

Sie können auch Attribute (sogenannte „Datenanmerkungen“) auf Ihre Klassen und Eigenschaften anwenden. Datenanmerkungen überschreiben Konventionen, werden aber von der Fluent-API-Konfiguration überschrieben.

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/Required.cs?range=11-16&highlight=4)] -->
``` csharp
    public class Blog
    {
        public int BlogId { get; set; }
        [Required]
        public string Url { get; set; }
    }
```
