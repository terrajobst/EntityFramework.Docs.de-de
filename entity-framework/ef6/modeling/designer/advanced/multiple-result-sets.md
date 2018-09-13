---
title: Gespeicherte Prozeduren mit mehreren Resultsets – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 1b3797f9-cd3d-4752-a55e-47b84b399dc1
ms.openlocfilehash: 098ed88ba52e211965baf3660f0e51bd74c71efd
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489309"
---
# <a name="stored-procedures-with-multiple-result-sets"></a>Gespeicherte Prozeduren mit mehreren Resultsets
Legen manchmal bei der Verwendung von gespeicherten Prozeduren, die Sie benötigen, um mehr als ein Ergebnis zurückzugeben. Dieses Szenario wird häufig zum Reduzieren der Anzahl der Roundtrips erforderlich, um einen einzelnen Bildschirm zu erstellen. Vor dem EF5 Entity Framework die gespeicherte Prozedur aufgerufen werden können, aber würde nur das erste Resultset an den aufrufenden Code zurück.

In diesem Artikel zeigt Ihnen zwei Möglichkeiten, die Sie verwenden können, auf mehr als ein Resultset aus einer gespeicherten Prozedur im Entity Framework. Eine, die nur aus Code verwendet und funktioniert mit beiden Code zuerst und dem EF Designer und eine, die nur mit dem EF Designer funktioniert. Die Tools und die API-Unterstützung für diese sollte in zukünftigen Versionen von Entity Framework verbessern.

## <a name="model"></a>Modell

In die Beispielen in diesem Artikel verwenden Sie einen einfachen Blog und Beiträge-Modells, die, in denen ein Blog viele Beiträge und eines Beitrags kann, in einem einzelnen Blog gehört. Wir verwenden eine gespeicherte Prozedur in der Datenbank, die alle Blogs und Beiträgen könnte folgendermaßen aussehen zurückgibt:

``` SQL
    CREATE PROCEDURE [dbo].[GetAllBlogsAndPosts]
    AS
        SELECT * FROM dbo.Blogs
        SELECT * FROM dbo.Posts
```

## <a name="accessing-multiple-result-sets-with-code"></a>Legt den Zugriff auf mehrere Resultsets, mit Code

Führen wir verwenden von Code zum Ausführen von eine unformatierte SQL‑Befehl aus, um unsere gespeicherte Prozedur auszuführen. Der Vorteil dieses Ansatzes ist, dass sie zuerst mit sowohl Code funktioniert und dem EF Designer.

Legt fest arbeiten, die wir in der ObjectContext-API durch Ablegen mithilfe der Benutzeroberfläche IObjectContextAdapter müssen, um mehrere Resultsets abzurufen.

Nachdem wir einen ObjectContext haben können wir die Translate-Methode verwenden, um die Ergebnisse der unsere gespeicherte Prozedur in Entitäten zu übersetzen, die nachverfolgt, und wie gewohnt in EF verwendet werden können. Im folgenden Codebeispiel wird veranschaulicht, dies in Aktion.

``` csharp
    using (var db = new BloggingContext())
    {
        // If using Code First we need to make sure the model is built before we open the connection
        // This isn't required for models created with the EF Designer
        db.Database.Initialize(force: false);

        // Create a SQL command to execute the sproc
        var cmd = db.Database.Connection.CreateCommand();
        cmd.CommandText = "[dbo].[GetAllBlogsAndPosts]";

        try
        {

            db.Database.Connection.Open();
            // Run the sproc
            var reader = cmd.ExecuteReader();

            // Read Blogs from the first result set
            var blogs = ((IObjectContextAdapter)db)
                .ObjectContext
                .Translate<Blog>(reader, "Blogs", MergeOption.AppendOnly);   


            foreach (var item in blogs)
            {
                Console.WriteLine(item.Name);
            }        

            // Move to second result set and read Posts
            reader.NextResult();
            var posts = ((IObjectContextAdapter)db)
                .ObjectContext
                .Translate<Post>(reader, "Posts", MergeOption.AppendOnly);


            foreach (var item in posts)
            {
                Console.WriteLine(item.Title);
            }
        }
        finally
        {
            db.Database.Connection.Close();
        }
    }
```

Die Translate-Methode akzeptiert den Reader an, dem wir erhalten, wenn wir die Prozedur, ein EntitySet-Namen und eine MergeOption ausgeführt. Der EntitySet-Name wird als "DbSet"-Eigenschaft auf Ihrem abgeleiteten Kontext übereinstimmen. Die Enumeration MergeOption steuert, wie die Ergebnisse verarbeitet werden, wenn die gleiche Entität bereits im Arbeitsspeicher vorhanden ist.

Hier durchlaufen wir die Sammlung von Blogs, bevor wir dies ist wichtig, dem obigen Code NextResult, aufrufen, da das erste Resultset verarbeitet werden muss, bevor Sie fortfahren, um das nächste Resultset.

Nachdem die beiden übersetzt werden Methoden aufgerufen, und klicken Sie dann die Blog und Post-Entitäten genauso wie andere Einrichtungen, die von EF nachverfolgt werden, und daher geändert oder gelöscht und wie gewohnt gespeichert werden kann.

>[!NOTE]
> EF wird keine Zuordnung berücksichtigt, beim Erstellen von Entitäten mit dem die Translate-Methode. Es wird einfach die Spaltennamen im Resultset mit Eigenschaftsnamen für die Klassen übereinstimmen.

>[!NOTE]
> Wenn Sie lazy Loading aktiviert, haben Zugriff auf die Beiträge-Eigenschaft auf eine der Entitäten Blog dann EF eine mit der Datenbank, um alle Beiträge, verzögert zu laden Verbindung, obwohl wir alle bereits geladen wurde. Dies ist da EF nicht bekannt ist, unabhängig davon, ob Sie alle Beiträge geladen haben oder wenn es mehr in der Datenbank. Wenn Sie dieses Problem zu vermeiden möchten, müssen Sie lazy Loading deaktiviert.

## <a name="multiple-result-sets-with-configured-in-edmx"></a>Mehrere Resultsets mit der Konfiguration in EDMX-Datei

>[!NOTE]
> Sie müssen als Ziel .NET Framework 4.5, um mehrere Resultsets in EDMX-Datei konfigurieren zu können. Wenn Sie .NET 4.0 verwenden möchten, können Sie die codebasierte-Methode, die im vorherigen Abschnitt gezeigt.

Wenn Sie dem EF Designer verwenden, können Sie Ihr Modell auch ändern, damit bekannt ist, Informationen zu den verschiedenen Resultsets, die zurückgegeben werden. Dabei ist zu wissen, bevor die Seite ist, dass die Tools nicht mehrere Resultsets festgelegt darüber im Klaren, daher Sie die Edmx-Datei manuell bearbeiten müssen. Bearbeiten die Edmx-Datei an, wie dies funktioniert, aber sie auch die Überprüfung des Modells in Visual Studio verlieren. Und wenn Sie überprüfen, das Modell ob immer Fehler.

-   Zu diesem Zweck müssen Sie die gespeicherte Prozedur wie bei der ein einzelnes Ergebnis Festlegen der Abfrage aus dem Modell hinzufügen.
-   Sobald Sie dies haben, müssen Sie ein, klicken Sie mit der rechten Maustaste auf das Modell, und wählen Sie **Öffnen mit...** Klicken Sie dann **Xml**

    ![Öffnen Sie als](~/ef6/media/openas.png)

Sobald Sie haben das Modell im XML-Format geöffnet wird, müssen Sie die folgenden Schritte ausführen:

-   Suchen Sie den komplexen Typ und Funktion Import in Ihrem Modell:

``` xml
    <!-- CSDL content -->
    <edmx:ConceptualModels>

    ...

      <FunctionImport Name="GetAllBlogsAndPosts" ReturnType="Collection(BlogModel.GetAllBlogsAndPosts_Result)" />

    ...

      <ComplexType Name="GetAllBlogsAndPosts_Result">
        <Property Type="Int32" Name="BlogId" Nullable="false" />
        <Property Type="String" Name="Name" Nullable="false" MaxLength="255" />
        <Property Type="String" Name="Description" Nullable="true" />
      </ComplexType>

    ...

    </edmx:ConceptualModels>
```

 

-   Entfernen Sie den komplexen Typ
-   Aktualisieren Sie die Funktion zu importieren, sodass es für Ihre Entitäten, in unserem Fall es zugeordnet ist wie folgt aussehen:

``` xml
    <FunctionImport Name="GetAllBlogsAndPosts">
      <ReturnType EntitySet="Blogs" Type="Collection(BlogModel.Blog)" />
      <ReturnType EntitySet="Posts" Type="Collection(BlogModel.Post)" />
    </FunctionImport>
```

Dies weist dem Modell, dass die gespeicherte Prozedur zwei Auflistungen, Blogeinträge und der Post-Einträge zurückgegeben wird.

-   Suchen Sie die Funktion Mapping-Element:

``` xml
    <!-- C-S mapping content -->
    <edmx:Mappings>

    ...

      <FunctionImportMapping FunctionImportName="GetAllBlogsAndPosts" FunctionName="BlogModel.Store.GetAllBlogsAndPosts">
        <ResultMapping>
          <ComplexTypeMapping TypeName="BlogModel.GetAllBlogsAndPosts_Result">
            <ScalarProperty Name="BlogId" ColumnName="BlogId" />
            <ScalarProperty Name="Name" ColumnName="Name" />
            <ScalarProperty Name="Description" ColumnName="Description" />
          </ComplexTypeMapping>
        </ResultMapping>
      </FunctionImportMapping>

    ...

    </edmx:Mappings>
```

-   Ersetzen Sie die Ergebnis-Zuordnung durch eine für jede Entität, die zurückgegeben wird, wie z. B. Folgendes ein:

``` xml
    <ResultMapping>
      <EntityTypeMapping TypeName ="BlogModel.Blog">
        <ScalarProperty Name="BlogId" ColumnName="BlogId" />
        <ScalarProperty Name="Name" ColumnName="Name" />
        <ScalarProperty Name="Description" ColumnName="Description" />
      </EntityTypeMapping>
    </ResultMapping>
    <ResultMapping>
      <EntityTypeMapping TypeName="BlogModel.Post">
        <ScalarProperty Name="BlogId" ColumnName="BlogId" />
        <ScalarProperty Name="PostId" ColumnName="PostId"/>
        <ScalarProperty Name="Title" ColumnName="Title" />
        <ScalarProperty Name="Text" ColumnName="Text" />
      </EntityTypeMapping>
    </ResultMapping>
```

Es ist auch möglich, komplexe Typen, z. B. die, die standardmäßig erstellt das Resultset mit Vorwärtscursor zuzuordnen. Dazu müssen Sie einen neuen komplexen Typ, statt zu entfernen, erstellen und verwenden Sie die komplexen Typen überall, dass Sie die Namen der Entitäten in den Beispielen oben verwendet haben.

Sobald diese Zuordnungen können Sie das Modell speichern und führen Sie den folgenden Code zum Verwenden der gespeicherten Prozedur geändert wurden:

``` csharp
    using (var db = new BlogEntities())
    {
        var results = db.GetAllBlogsAndPosts();

        foreach (var result in results)
        {
            Console.WriteLine("Blog: " + result.Name);
        }

        var posts = results.GetNextResult<Post>();

        foreach (var result in posts)
        {
            Console.WriteLine("Post: " + result.Title);
        }

        Console.ReadLine();
    }
```

>[!NOTE]
> Wenn Sie die Edmx-Datei für Ihr Modell manuell bearbeiten wird sie überschrieben werden, wenn Sie das Modell aus der Datenbank immer neu generieren.

## <a name="summary"></a>Zusammenfassung

Hier haben wir gezeigt, zwei verschiedene Methoden für den Zugriff auf mehrere Sätze unter Verwendung von Entity Framework. Beide Angaben sind gleichermaßen gültig ist, je nach Situation und Einstellungen, und Sie sollten auswählen, die für Ihre Situation am besten geeignet ist. Es ist geplant, dass die Unterstützung für mehrere Resultsets, die Gruppen werden in zukünftigen Versionen von Entity Framework verbessert und den Schritten in diesem Dokument nicht mehr notwendig sein werden.
