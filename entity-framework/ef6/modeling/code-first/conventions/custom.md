---
title: Benutzerdefinierte Code First-Konventionen - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: dd2bdbd9-ae9e-470a-aeb8-d0ba160499b7
ms.openlocfilehash: cfd7f7cad532dca5227793c04d7d91e977ea5e4e
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489843"
---
# <a name="custom-code-first-conventions"></a>Benutzerdefinierte Code First-Konventionen
> [!NOTE]
> **Nur EF6 und höher:** Die Features, APIs usw., die auf dieser Seite erläutert werden, wurden in Entity Framework 6 eingeführt. Wenn Sie eine frühere Version verwenden, gelten manche Informationen nicht.

Wenn Sie Code First verwenden das Modell aus den Klassen, die mit einem Satz von Konventionen berechnet wird. Der Standardwert [Code First-Konventionen](~/ef6/modeling/code-first/conventions/built-in.md) wie die Eigenschaft wird der Primärschlüssel einer Entität, die den Namen des in der Tabelle eine Entität zugeordnet und welche Genauigkeit und Dezimalstellenanzahl verfügt über Spalten im Dezimalformat standardmäßig zu ermitteln.

Dieser Standardkonventionen sind manchmal nicht optimal geeignet für Ihr Modell, und Sie diese umgehen, indem viele einzelne Elemente, die mithilfe von Datenanmerkungen oder der Fluent-API konfigurieren müssen. Benutzerdefinierte Code First-Konventionen können Sie Ihre eigenen Konventionen zu definieren, die Standardeinstellungen der Konfiguration für Ihr Modell bereitstellen. In dieser exemplarischen Vorgehensweise erforschen wir die verschiedenen Typen von benutzerdefinierten Konventionen und wie Sie jeweils zu erstellen.


## <a name="model-based-conventions"></a>Modellbasierten Konventionen

Auf dieser Seite werden der DbModelBuilder-API für benutzerdefinierte Konventionen behandelt. Diese API sollte für die Erstellung von den meisten benutzerdefinierter Konventionen ausreichend sein. Es ist jedoch auch die Möglichkeit zum Erstellen von modellbasierten Konventionen - Konventionen, die das endgültige Modell bearbeiten, nachdem es erstellt wurde – erweiterte Szenarien behandelt. Weitere Informationen finden Sie unter [modellbasierten Konventionen](~/ef6/modeling/code-first/conventions/model.md).

 

## <a name="our-model"></a>Unser Modell

Zuerst definieren wir ein einfaches Modell, das wir mit unserer Konventionen verwenden können. Fügen Sie Ihrem Projekt die folgenden Klassen hinzu.

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Data.Entity;
    using System.Linq;

    public class ProductContext : DbContext
    {
        static ProductContext()
        {
            Database.SetInitializer(new DropCreateDatabaseIfModelChanges<ProductContext>());
        }

        public DbSet<Product> Products { get; set; }
    }

    public class Product
    {
        public int Key { get; set; }
        public string Name { get; set; }
        public decimal? Price { get; set; }
        public DateTime? ReleaseDate { get; set; }
        public ProductCategory Category { get; set; }
    }

    public class ProductCategory
    {
        public int Key { get; set; }
        public string Name { get; set; }
        public List<Product> Products { get; set; }
    }
```

 

## <a name="introducing-custom-conventions"></a>Einführung in benutzerdefinierte Konventionen

Schreiben wir nun eine Konvention, die eine Eigenschaft mit dem Namen Taste, um den primären Schlüssel für den Entitätstyp werden konfiguriert.

Konventionen sind auf den Modell-Generator aktiviert, die durch das Überschreiben von "onmodelcreating" im Kontext zugegriffen werden kann. Aktualisieren Sie die ProductContext-Klasse wie folgt:

``` csharp
    public class ProductContext : DbContext
    {
        static ProductContext()
        {
            Database.SetInitializer(new DropCreateDatabaseIfModelChanges<ProductContext>());
        }

        public DbSet<Product> Products { get; set; }

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
            modelBuilder.Properties()
                        .Where(p => p.Name == "Key")
                        .Configure(p => p.IsKey());
        }
    }
```

Jetzt wird jede Eigenschaft in unserem Modell benannten Schlüssel konfiguriert als primären Schlüssel für beliebige Entität seinen Teil.

Wir konnten stellen Sie außerdem unsere Konventionen spezifischere durch den Typ der Eigenschaft, die an das Konfigurieren von Filtern:

``` csharp
    modelBuilder.Properties<int>()
                .Where(p => p.Name == "Key")
                .Configure(p => p.IsKey());
```

Dadurch wird konfiguriert, dass alle Eigenschaften, die aufgerufen werden Schlüssel, um den primären Schlüssel, der die Entität, jedoch nur, wenn sie eine ganze Zahl sind.

Ein interessantes Feature der IsKey-Methode besteht darin, dass additiv. Das bedeutet, dass, wenn Sie mehrere Eigenschaften IsKey aufrufen und alle Teil eines zusammengesetzten Schlüssels werden. Ein Nachteil dafür ist, dass wenn Sie mehrere Eigenschaften für einen Schlüssel angeben, auch einen Auftrag für diese Eigenschaften angeben müssen. Dazu können Sie die Methode wie die folgende HasColumnOrder aufrufen:

``` csharp
    modelBuilder.Properties<int>()
                .Where(x => x.Name == "Key")
                .Configure(x => x.IsKey().HasColumnOrder(1));

    modelBuilder.Properties()
                .Where(x => x.Name == "Name")
                .Configure(x => x.IsKey().HasColumnOrder(2));
```

Dieser Code konfiguriert die Typen in unserem Modell haben Sie einen zusammengesetzten Schlüssel, der aus die Int-Key-Spalte und die Namensspalte Zeichenfolge besteht. Wenn wir das Modell im Designer anzuzeigen, würde es folgendermaßen aussehen:

![zusammengesetzter Schlüssel](~/ef6/media/compositekey.png)

Ein weiteres Beispiel für Eigenschaft Konventionen ist so konfigurieren Sie alle Eigenschaften von "DateTime" in meinem Modell in den datetime2-Typ in SQL Server anstelle von "DateTime" zugeordnet. Sie erreichen dies durch den folgenden:

``` csharp
    modelBuilder.Properties<DateTime>()
                .Configure(c => c.HasColumnType("datetime2"));
```

 

## <a name="convention-classes"></a>Konvention-Klassen

Eine weitere Möglichkeit zum Definieren von Konventionen ist eine Konvention-Klasse verwendet, um Ihre Konvention zu kapseln. Bei Verwendung einer Konvention-Klasse erstellen Sie einen Typ, der von der Konvention-Klasse im Namespace System.Data.Entity.ModelConfiguration.Conventions erbt.

Wir können eine Konvention-Klasse mit der datetime2-Konvention erstellen, die wir weiter oben gezeigt habe, wie folgt:

``` csharp
    public class DateTime2Convention : Convention
    {
        public DateTime2Convention()
        {
            this.Properties<DateTime>()
                .Configure(c => c.HasColumnType("datetime2"));        
        }
    }
```

Informieren von EF diese Konvention verwenden Sie es der Konventionen-Auflistung in "OnModelCreating" hinzufügen, wenn Sie zusammen mit der exemplarischen Vorgehensweise durchgearbeitet haben so aussieht:

``` csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Properties<int>()
                    .Where(p => p.Name.EndsWith("Key"))
                    .Configure(p => p.IsKey());

        modelBuilder.Conventions.Add(new DateTime2Convention());
    }
```

Wie Sie, dass wir eine Instanz von unserem Konvention der Konventionen Auflistung hinzufügen sehen können. Erben von Konvention bietet eine bequeme Möglichkeit, einer Gruppierung und Konventionen für Teams oder Projekte freigegeben. Sie könnten z. B. eine Klassenbibliothek mit einem gemeinsamen Satz von Konventionen, dass alle von Ihrer Organisation Projekte haben.

 

## <a name="custom-attributes"></a>Benutzerdefinierte Attribute

Weiteres können Sie Konventionen ist so aktivieren Sie neue Attribute verwendet werden, wenn Sie ein Modell zu konfigurieren. Um dies zu veranschaulichen, wir erstellen ein Attribut, das wir zum Markieren von Zeichenfolgeneigenschaften als nicht-Unicode verwenden können.

``` csharp
    [AttributeUsage(AttributeTargets.Property, AllowMultiple = false)]
    public class NonUnicode : Attribute
    {
    }
```

Nun erstellen wir eine Konvention, um dieses Attribut auf unserem Modell anwenden:

``` csharp
    modelBuilder.Properties()
                .Where(x => x.GetCustomAttributes(false).OfType<NonUnicode>().Any())
                .Configure(c => c.IsUnicode(false));
```

Diese Konvention können wir hinzufügen, dass das nicht-Unicode-Attribut auf unsere Zeichenfolgeneigenschaften, was bedeutet die Spalte in der Datenbank als Varchar, anstatt Nvarchar gespeichert wird.

Informationen zu dieser Konvention zu beachten ist, die, wenn Sie das nicht-Unicode-Attribut für etwas anderes als eine String-Eigenschaft festlegen, und klicken Sie dann eine Ausnahme ausgelöst wird. Es liegt daran, dass Sie nicht auf einem anderen Typ als eine Zeichenfolge IsUnicode konfigurieren können. In diesem Fall können Sie Ihre Konvention genauer gesagt ausführen, damit es etwas herausfiltert, die nicht von einer Zeichenfolge.

Die oben genannten Konvention zum Definieren von benutzerdefinierter Attributs funktioniert wird es eine andere API, die viel einfacher zu verwenden, insbesondere wenn Sie möchten die Eigenschaften der Attributklasse verwenden werden können.

In diesem Beispiel werden wir unseren Attribut aktualisieren und ändern Sie ihn in ein IsUnicode-Attribut, sodass sie wie folgt aussieht:

``` csharp
    [AttributeUsage(AttributeTargets.Property, AllowMultiple = false)]
    internal class IsUnicode : Attribute
    {
        public bool Unicode { get; set; }

        public IsUnicode(bool isUnicode)
        {
            Unicode = isUnicode;
        }
    }
```

Sobald wir dies haben, können wir einen booleschen Wert festlegen, auf unsere Attribut der Konvention mitteilen, ob eine Eigenschaft Unicode sein sollte. Wir könnten dies in der Konvention haben wir bereits durch den Zugriff auf die ClrProperty der Configuration-Klasse wie folgt vorgehen:

``` csharp
    modelBuilder.Properties()
                .Where(x => x.GetCustomAttributes(false).OfType<IsUnicode>().Any())
                .Configure(c => c.IsUnicode(c.ClrPropertyInfo.GetCustomAttribute<IsUnicode>().Unicode));
```

Dies ist recht einfach, aber es ist eine kompaktere Möglichkeit zum erreichen Sie dies mithilfe der Having-Methode der den API-Konventionen. Die Having Methode hat einen Parameter vom Typ Func&lt;PropertyInfo, T&gt; akzeptiert der PropertyInfo identisch mit der Where-Methode, aber erwartet wird, um ein Objekt zurückzugeben. Wenn das zurückgegebene Objekt null ist, und klicken Sie dann die Eigenschaft nicht konfiguriert werden soll, das bedeutet, dass Eigenschaften, die mit ihm wie Where filtern kann, aber es unterscheidet sich auch das zurückgegebene Objekt zu erfassen und an die Configure-Methode übergeben wird. Dies funktioniert folgendermaßen:

``` csharp
    modelBuilder.Properties()
                .Having(x =>x.GetCustomAttributes(false).OfType<IsUnicode>().FirstOrDefault())
                .Configure((config, att) => config.IsUnicode(att.Unicode));
```

Benutzerdefinierte Attribute sind nicht der einzige Grund für das Verwenden der Having-Methode, es ist nützlich, überall, die etwas befassen müssen, die Sie beim Konfigurieren Ihrer Typen oder Eigenschaften filtern möchten.

 

## <a name="configuring-types"></a>Konfigurieren von Typen

Bisher wurden aller unserer Konventionen für Eigenschaften, aber einen anderen Bereich der Konventionen API besteht, für die Konfiguration der Typen im Modell. Die Benutzeroberfläche ähnelt der Konventionen, die wir eben gesehen haben, aber die Optionen innerhalb konfigurieren, findet sich auf der Entität anstatt nach Eigenschaft auf.

Eines der Dinge, denen Ebene für Parametertypen besonders nützlich sein können wird die Tabelle-Namenskonvention gilt, um ein vorhandenes Schema zuzuordnen, die von den EF-Standard abweicht oder zum Erstellen einer neuen Datenbank mit einer anderen Namenskonvention geändert. Zu diesem Zweck benötigen wir zuerst eine Methode, die akzeptieren die TypeInfo für einen Typ in unserem Modell kann, und zurückgeben, was der Tabellennamen für diesen Typ werden soll:

``` csharp
    private string GetTableName(Type type)
    {
        var result = Regex.Replace(type.Name, ".[A-Z]", m => m.Value[0] + "_" + m.Value[1]);

        return result.ToLower();
    }
```

Diese Methode akzeptiert einen Typ und gibt eine Zeichenfolge, die Kleinbuchstaben mit Unterstrichen anstelle von CamelCase verwendet. In unserem Modell bedeutet dies, dass die ProductCategory-Klasse, eine Tabelle namens Produkt zugeordnet werden soll\_Kategorie anstelle von ProductCategories.

Nachdem wir diese Methode erstellt haben, können wir es in eine Konvention wie folgt aufrufen:

``` csharp
    modelBuilder.Types()
                .Configure(c => c.ToTable(GetTableName(c.ClrType)));
```

Diese Konvention konfiguriert jeden Typ in unserem Modell auf den Namen der Tabelle zugeordnet, die von unseren GetTableName-Methode zurückgegeben wird. Diese Konvention entspricht dem Aufruf der ToTable-Methode für jede Entität im Modell mithilfe der Fluent-API.

Informationen hierzu zu beachten ist aus, dass beim Aufrufen ToTable EF die Zeichenfolge, die Sie als genaue Tabellennamen, ohne die Pluralisierung bereitstellen, die sie normalerweise, beim Tabellennamen zu bestimmen ausführen würden. Daher ist der Tabellenname aus unserer Konvention Produkt\_Kategorie anstelle von Produkt\_Kategorien. Wir können, die in unserer Konvention Anruf an den pluralisierungsdienst selbst beheben.

In den folgenden Code verwenden wir die [Abhängigkeitsauflösung](~/ef6/fundamentals/configuring/dependency-resolution.md) Feature hinzugefügt, die in EF6 die Pluralisierung abrufen, die EF verwendet haben, wird in den Singular unsere Tabellenname und.

``` csharp
    private string GetTableName(Type type)
    {
        var pluralizationService = DbConfiguration.DependencyResolver.GetService<IPluralizationService>();

        var result = pluralizationService.Pluralize(type.Name);

        result = Regex.Replace(result, ".[A-Z]", m => m.Value[0] + "_" + m.Value[1]);

        return result.ToLower();
    }
```

> [!NOTE]
> Die generische Version von "GetService" ist eine Erweiterungsmethode im System.Data.Entity.Infrastructure.DependencyResolution-Namespace, müssen Sie zum Hinzufügen einer using-Anweisung den Kontext, um sie zu verwenden.

### <a name="totable-and-inheritance"></a>ToTable und Vererbung

Ein weiterer wichtiger Aspekt der ToTable ist, sofern explizit eines Typs für eine bestimmte Tabelle zuordnen, und klicken Sie dann Sie die Zuordnungsstrategie für die ändern können, die EF verwenden. Wenn Sie für jeden Typ in einer Vererbungshierarchie ToTable aufrufen, werden der Typname als den Namen der Tabelle übergeben wird, wie oben, Sie Zuordnung die Standardstrategie-Tabelle pro Hierarchie (TPH), Tabelle pro Typ (TPT) ändern. Die beste Möglichkeit, dies ist innerhalb eines konkreten Beispiels:

``` csharp
    public class Employee
    {
        public int Id { get; set; }
        public string Name { get; set; }
    }

    public class Manager : Employee
    {
        public string SectionManaged { get; set; }
    }
```

Standardmäßig sind sowohl Mitarbeiter und Manager, die der gleichen Tabelle (Mitarbeiter) in der Datenbank zugeordnet. Die Tabelle enthält, Mitarbeitern und Managern mit der eine Unterscheidungsspalte, die Sie darüber informiert, welche Art von Instanz, die in jeder Zeile gespeichert ist. Dies ist eine TPH-Zuordnung, wie eine einzelne Tabelle für die Hierarchie vorhanden ist. Allerdings, wenn Sie für beide Classe ToTable aufrufen, wird Klicken Sie dann jeden Typ stattdessen zugeordnet werden, auch bekannt als TPT, da jeder eine eigene Tabelle verfügt über eine eigene Tabelle.

``` csharp
    modelBuilder.Types()
                .Configure(c=>c.ToTable(c.ClrType.Name));
```

Der obige Code wird die Struktur einer Tabelle zugeordnet sein, die wie folgt aussieht:

![TPT-Beispiel](~/ef6/media/tptexample.jpg)

Sie können dies verhindern, und die standardmäßig TPH-Zuordnung, auf unterschiedliche Weise verwalten:

1.  Rufen Sie ToTable, mit dem gleichen Tabellennamen für jeden Typ in der Hierarchie.
2.  Rufen Sie ToTable nur für die Basisklasse der Hierarchie, in unserem Beispiel, die Mitarbeiter sind.

 

## <a name="execution-order"></a>Ausführungsreihenfolge

Konventionen, die in letzten Wins Weise identisch mit der Fluent-API ausgeführt werden. Das bedeutet, ist, wenn Sie zwei Konventionen, die die gleiche Möglichkeit, die gleiche Eigenschaft konfigurieren, und klicken Sie dann auf die letzte Lektion zum Ausführen von Wins schreiben. Beispielsweise können in den folgenden Code wird die maximale Länge aller Zeichenfolgen auf 500 festgelegt, aber wir konfigurieren Sie dann alle Eigenschaften, die mit dem Namen Name im Modell zu eine maximale Länge von 250 haben.

``` csharp
    modelBuilder.Properties<string>()
                .Configure(c => c.HasMaxLength(500));

    modelBuilder.Properties<string>()
                .Where(x => x.Name == "Name")
                .Configure(c => c.HasMaxLength(250));
```

Da Konvention, die die maximalen Länge auf 250 festgelegt nach der ist, die alle Zeichenfolgen auf 500 festgelegt, werden alle Eigenschaften, die mit dem Namen Name in unserem Modell weist eine maximale Länge 250, während andere Zeichenfolgen, z. B. Beschreibungen, wäre 500. Mithilfe von Konventionen auf diese Weise bedeutet, dass Sie eine allgemeine Konvention für Typen oder Eigenschaften in Ihr Modell und erstellt dann einen override aufgenommen bereitstellen, können Sie Teilmengen, die unterschiedlich sind.

Die Fluent-API und die Datenanmerkungen kann auch verwendet werden, um eine Konvention in bestimmten Fällen zu überschreiben. Im Beispiel oben hätten wir die Fluent-API verwendet, legen Sie die maximale Länge einer Eigenschaft konnte klicken Sie dann wir es vor oder nach der Konvention eingefügt haben, da die spezifischere Fluent-API über die allgemeine Konfiguration Konvention gewinnen wird.

 

## <a name="built-in-conventions"></a>Integrierte Konventionen

Da benutzerdefinierte Konventionen von den Standardkonventionen für Code First auswirken könnten, kann es nützlich, um das Hinzufügen von Konventionen, die vor oder nach einem anderen Konvention ausgeführt sein. Zu diesem Zweck können Sie die Methoden "AddBefore" und AddAfter des der Auflistung der Konventionen in Ihrer abgeleiteten DbContext. Der folgende Code würde fügen Sie die Konvention-Klasse, die wir zuvor erstellt haben, damit er vor den integrierten ausgeführt wird in der Ermittlung des Konvention.

``` csharp
    modelBuilder.Conventions.AddBefore<IdKeyDiscoveryConvention>(new DateTime2Convention());
```

Hier werden die meisten nützlich sein, wenn Konventionen hinzufügen, die vor oder nach der integrierten Konventionen ausführen müssen, eine Liste der integrierten Konventionen finden Sie hier: [System.Data.Entity.ModelConfiguration.Conventions Namespace](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx) .

Sie können auch entfernen Konventionen, die Sie nicht möchten, auf das Modell angewendet. Um eine Konvention zu entfernen, verwenden Sie die Remove-Methode. Hier ist ein Beispiel für die PluralizingTableNameConvention entfernen.

``` csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
    }
```
