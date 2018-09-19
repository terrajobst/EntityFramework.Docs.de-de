---
title: Vorab generierte Mapping-Ansichten – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 917ba9c8-6ddf-4631-ab8c-c4fb378c2fcd
ms.openlocfilehash: da5d59ba5a899a0ee3a1eec3db0da1b4ece871d8
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2018
ms.locfileid: "46284108"
---
# <a name="pre-generated-mapping-views"></a>Vorab generierte Mapping-Ansichten
Bevor das Entity Framework einer Abfrage ausführen oder Änderungen an der Datenquelle speichern können, müssen sie einen Satz von Ansichten der Zuordnung Zugriff auf die Datenbank erstellen. Diese Zuordnung Ansichten sind eine Reihe von Entity SQL-Anweisung, die die Datenbank auf abstrakte Weise darstellen, und sind Teil der Metadaten, die pro Anwendungsdomäne zwischengespeichert wird. Wenn Sie mehrere Instanzen desselben Kontexts in der gleichen Anwendungsdomäne erstellen, werden diese Zuordnung Ansichten aus objektkontextinstanzen, statt die zwischengespeicherten Metadaten wiederverwendet. Da Zuordnung Generieren von Sichten einen signifikanten Teil der Gesamtkosten der Ausführung der ersten Abfrage ist, ermöglicht dem Entity Framework vorgenerieren von Ansichten der Zuordnung, und schließen Sie sie in das kompilierte Projekt an. Weitere Informationen finden Sie unter [Überlegungen zur Leistung (Entity Framework)](~/ef6/fundamentals/performance/perf-whitepaper.md).

## <a name="generating-mapping-views-with-the-ef-power-tools"></a>Generieren von Ansichten mit EF Powertools zuordnen

Die einfachste Möglichkeit zum vorgenerieren von Ansichten ist die Verwendung der [Entity Framework Power Tools](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d). Nachdem Sie die Power-Tools installiert haben, müssen Sie eine Menüoption zum Generieren von Sichten, wie unten gezeigt.

-   Für **Code First** Modelle mit der rechten Maustaste auf die Codedatei, die Ihr "DbContext"-Klasse enthält.
-   Für **EF Designer** Modelle mit der rechten Maustaste auf die EDMX-Datei.

![Generieren von Sichten](~/ef6/media/generateviews.png)

Sobald der Vorgang abgeschlossen ist, müssen Sie eine Klasse, die etwa wie folgt generiert

![generierte Ansichten](~/ef6/media/generatedviews.png)

Jetzt beim Ausführen verwendet Ihrer Anwendung EF diese Klasse Ansichten nach Bedarf geladen. Wenn Ihr Modell ändert und Sie nicht diese Klasse erneut generieren, wird EF eine Ausnahme ausgelöst.

## <a name="generating-mapping-views-from-code---ef6-onwards"></a>Generieren von Zuordnen von Ansichten von Code – EF6 oder höher

Die andere Möglichkeit zum Generieren von Sichten ist zur Verwendung der APIs, die EF bereitstellt. Bei Verwendung dieser Methode müssen Sie die Freiheit gibt, die Ansichten zu serialisieren, aber wie Sie allerdings Sie auch die Ansichten selbst laden müssen.

> [!NOTE]
> **EF6 oder höher, nur** -der APIs, die in diesem Abschnitt gezeigten in Entity Framework 6 eingeführt wurden. Diese Informationen gelten nicht, wenn Sie eine frühere Version verwenden.

### <a name="generating-views"></a>Generieren von Sichten

Die APIs zum Generieren von Ansichten sind für die System.Data.Entity.Core.Mapping.StorageMappingItemCollection-Klasse. Sie können eine StorageMappingCollection für einen Kontext abrufen, mit der MetadataWorkspace von ObjectContext. Wenn Sie die neueren DbContext-API verwenden, können Sie dies mithilfe von auf zugreifen können der IObjectContextAdapter wie die folgende, in diesem Code haben wir eine Instanz von Ihr abgeleiteten "DbContext" und "DbContext" aufgerufen:

``` csharp
    var objectContext = ((IObjectContextAdapter) dbContext).ObjectContext;
    var  mappingCollection = (StorageMappingItemCollection)objectContext.MetadataWorkspace
                                                                        .GetItemCollection(DataSpace.CSSpace);
```

Nachdem Sie die StorageMappingItemCollection haben, erhalten Sie Zugriff auf die GenerateViews und ComputeMappingHashValue-Methoden.

``` csharp
    public Dictionary\<EntitySetBase, DbMappingView> GenerateViews(IList<EdmSchemaError> errors)
    public string ComputeMappingHashValue()
```

Die erste Methode erstellt ein Wörterbuch mit einem Eintrag für jede Ansicht in der Zuordnung des Containers. Die zweite Methode berechnet einen Hashwert für die Zuordnung eines einzelnen Containers und dient zur Laufzeit zum Überprüfen, dass das Modell nicht geändert wurde, da die Ansichten vorab generiert wurden. Überschreibungen der beiden Methoden werden für komplexe Szenarien im Zusammenhang mit mehreren schutzcontainerzuordnungen bereitgestellt.

Beim Generieren von Sichten, die Sie GenerateViews Methode aufrufen, und klicken Sie dann die resultierende EntitySetBase und DbMappingView schreiben werden. Sie müssen auch die von der-Methode ComputeMappingHashValue generierten Hash speichern.

### <a name="loading-views"></a>Laden von Ansichten

Um die von der-Methode GenerateViews generierten Ansichten zu laden, können Sie EF mit einer Klasse bereitstellen, die von der abstrakten DbMappingViewCache-Klasse erbt. DbMappingViewCache gibt zwei Methoden, die Sie implementieren müssen:

``` csharp
    public abstract string MappingHashValue { get; }
    public abstract DbMappingView GetView(EntitySetBase extent);
```

Die MappingHashValue-Eigenschaft muss den von der-Methode ComputeMappingHashValue generierten Hash zurückgeben. Wenn EF ist jetzt für Ansichten stellen sie zunächst generiert und vergleichen den Hashwert des Modells mit dem Hashwert, der von dieser Eigenschaft zurückgegebene. Wenn sie nicht übereinstimmen dann löst EF EntityCommandCompilationException eine Ausnahme.

Die GetView-Methode akzeptiert eine EntitySetBase aus, und Sie müssen zurückgegeben, dass eine DbMappingVIew, enthält die EntitySql, die dafür generiert wurde der angegebene EntitySetBase in das Wörterbuch, das von der-Methode GenerateViews generiert zugeordnet war. Eine Ansicht, dass Sie nicht dann GetView verfügen sollte null zurückgeben, wenn EF anfordert.

Im folgenden finden einen Auszug aus der DbMappingViewCache, die wie oben beschrieben sehen Sie eine Möglichkeit zum Speichern und Abrufen der EntitySql erforderlich sind, sich mit den Power Tools generiert wird.

``` csharp
    public override string MappingHashValue
    {
        get { return "a0b843f03dd29abee99789e190a6fb70ce8e93dc97945d437d9a58fb8e2afd2e"; }
    }

    public override DbMappingView GetView(EntitySetBase extent)
    {
        if (extent == null)
        {
            throw new ArgumentNullException("extent");
        }

        var extentName = extent.EntityContainer.Name + "." + extent.Name;

        if (extentName == "BlogContext.Blogs")
        {
            return GetView2();
        }

        if (extentName == "BlogContext.Posts")
        {
            return GetView3();
        }

        return null;
    }

    private static DbMappingView GetView2()
    {
        return new DbMappingView(@"
            SELECT VALUE -- Constructing Blogs
            [BlogApp.Models.Blog](T1.Blog_BlogId, T1.Blog_Test, T1.Blog_title, T1.Blog_Active, T1.Blog_SomeDecimal)
            FROM (
            SELECT
                T.BlogId AS Blog_BlogId,
                T.Test AS Blog_Test,
                T.title AS Blog_title,
                T.Active AS Blog_Active,
                T.SomeDecimal AS Blog_SomeDecimal,
                True AS _from0
            FROM CodeFirstDatabase.Blog AS T
            ) AS T1");
    }
```

Damit EF verwenden Ihre DbMappingViewCache, die Sie hinzufügen, verwenden Sie die DbMappingViewCacheTypeAttribute, Angeben des Kontexts, dem sie für erstellt wurde. Im folgenden Code ordnen wir den BlogContext, mit der MyMappingViewCache-Klasse.

``` csharp
    [assembly: DbMappingViewCacheType(typeof(BlogContext), typeof(MyMappingViewCache))]
```

Für komplexere Szenarien können Zuordnung anzeigen-Cache-Instanzen bereitgestellt werden, durch Angeben einer Zuordnung Ansicht Cache-Factory. Dies kann durch die Implementierung der abstrakten Klasse System.Data.Entity.Infrastructure.MappingViews.DbMappingViewCacheFactory erfolgen. Die Instanz der Zuordnung anzeigen Cache-Factory, die verwendet wird, kann abgerufen oder mithilfe der StorageMappingItemCollection.MappingViewCacheFactoryproperty festgelegt werden.
