---
title: Vererbung (relationale Datenbank)-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
uid: core/modeling/relational/inheritance
ms.openlocfilehash: 381d1878007bb78b359eb49649f4356f1e5eb04a
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655629"
---
# <a name="inheritance-relational-database"></a>Vererbung (relationale Datenbank)

> [!NOTE]  
> Die Konfiguration in diesem Abschnitt gilt allgemein für relationale Datenbanken. Die hier gezeigten Erweiterungsmethoden werden verfügbar, wenn Sie einen relationalen Datenbankanbieter installieren (aufgrund des gemeinsam genutzten Pakets *Microsoft.EntityFrameworkCore.Relational*).

Die Vererbung im EF-Modell wird verwendet, um zu steuern, wie die Vererbung in den Entitäts Klassen in der Datenbank dargestellt wird.

> [!NOTE]  
> Derzeit wird nur das TPH-Muster (Table-per Hierarchy) in EF Core implementiert. Andere gängige Muster wie Tabelle pro Typ (TPT) und Table-per-konkrete-Type (TPC) sind noch nicht verfügbar.

## <a name="conventions"></a>Konventionen

Gemäß der Konvention wird die Vererbung mithilfe des TPH-Musters (Table-per Hierarchy) zugeordnet. TPH verwendet eine einzelne Tabelle, um die Daten für alle Typen in der Hierarchie zu speichern. Eine diskriminatorspalte wird verwendet, um den Typ zu identifizieren, den jede Zeile darstellt.

EF Core wird nur dann eine Vererbung eingerichtet, wenn mindestens zwei geerbte Typen explizit in das Modell eingeschlossen werden (Weitere Informationen finden Sie unter [Vererbung](../inheritance.md) ).

Im folgenden finden Sie ein Beispiel für ein einfaches Vererbungs Szenario und die in einer relationalen Datenbanktabelle gespeicherten Daten mithilfe des TPH-Musters. Die *diskriminatorspalte* gibt an, welcher Typ von *Blog* in den einzelnen Zeilen gespeichert wird.

[!code-csharp[Main](../../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs#Model)]

![Bild](_static/inheritance-tph-data.png)

>[!NOTE]
> Bei Verwendung der TPH-Zuordnung werden bei Bedarf automatisch NULL-Werte für Daten Bank Spalten festgelegt.

## <a name="data-annotations"></a>Datenanmerkungen

Zum Konfigurieren der Vererbung können keine Daten Anmerkungen verwendet werden.

## <a name="fluent-api"></a>Fluent-API

Sie können die fließende API verwenden, um den Namen und den Typ der diskriminatorspalte und die Werte zu konfigurieren, die zum Identifizieren der einzelnen Typen in der Hierarchie verwendet werden.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/InheritanceTPHDiscriminator.cs#Inheritance)]

## <a name="configuring-the-discriminator-property"></a>Konfigurieren der diskriminatoreigenschaft

In den obigen Beispielen wird der Diskriminator als [Schatten Eigenschaft](xref:core/modeling/shadow-properties) für die Basis Entität der Hierarchie erstellt. Da es sich um eine Eigenschaft im Modell handelt, kann Sie genau wie andere Eigenschaften konfiguriert werden. So legen Sie z. b. die maximale Länge fest, wenn der standardmäßige Diskriminator verwendet wird:

```C#
modelBuilder.Entity<Blog>()
    .Property("Discriminator")
    .HasMaxLength(200);
```

Der Diskriminator kann auch einer tatsächlichen CLR-Eigenschaft in der Entität zugeordnet werden. Beispiel:

```C#
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasDiscriminator<string>("BlogType");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
    public string BlogType { get; set; }
}

public class RssBlog : Blog
{
    public string RssUrl { get; set; }
}
```

Wenn Sie diese beiden Elemente miteinander kombinieren, ist es möglich, den Diskriminator einer echten Eigenschaft zuzuordnen und zu konfigurieren:

```C#
modelBuilder.Entity<Blog>(b =>
{
    b.HasDiscriminator<string>("BlogType");

    b.Property(e => e.BlogType)
        .HasMaxLength(200)
        .HasColumnName("blog_type");
});
```
