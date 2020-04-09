---
title: Datenbindung mit WPF - EF6
author: divega
ms.date: 04/02/2020
ms.assetid: e90d48e6-bea7785-47ef-b756-7b89cce4daf0
ms.openlocfilehash: 6908e2a7597d0c199620c6015ed3ea06226c5ea9
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2020
ms.locfileid: "80639145"
---
> [!IMPORTANT]
> **Dieses Dokument ist nur für WPF auf .NET Framework gültig.**
>
> In diesem Dokument wird die Datenbindung für WPF in .NET Framework beschrieben. Für neue .NET Core-Projekte wird empfohlen, [EF Core](/ef/core) anstelle von Entity Framework 6 zu verwenden. Die Dokumentation zur Datenbindung in EF Core wird unter [Ausgabe #778](https://github.com/dotnet/EntityFramework.Docs/issues/778)nachverfolgt.

# <a name="databinding-with-wpf"></a>Datenbindung mit WPF
In dieser schrittweisen exemplarischen Vorgehensweise wird gezeigt, wie POCO-Typen in einem "Master-Detail"-Formular an WPF-Steuerelemente gebunden werden. Die Anwendung verwendet die Entity Framework-APIs, um Objekte mit Daten aus der Datenbank aufzufüllen, Änderungen nachzuverfolgen und Daten in der Datenbank beizubehalten.

Das Modell definiert zwei Typen, die an einer 1:n-Beziehung beteiligt\\sind: **Kategorie** (Prinzipalmaster)\\und **Produkt** (abhängiges Detail). Anschließend werden die Visual Studio-Tools verwendet, um die im Modell definierten Typen an die WPF-Steuerelemente zu binden. Das WPF-Datenbindungsframework ermöglicht die Navigation zwischen verwandten Objekten: Durch das Auswählen von Zeilen in der Masteransicht wird die Detailansicht mit den entsprechenden untergeordneten Daten aktualisiert.

Die Screenshots und Codeauflistungen in dieser exemplarischen Vorgehensweise stammen aus Visual Studio 2013, Sie können diese exemplarische Vorgehensweise jedoch mit Visual Studio 2012 oder Visual Studio 2010 abschließen.

## <a name="use-the-object-option-for-creating-wpf-data-sources"></a>Verwenden der Option 'Objekt' zum Erstellen von WPF-Datenquellen

Mit der vorherigen Version von Entity Framework haben wir empfohlen, die **Option Datenbank** zu verwenden, wenn eine neue Datenquelle basierend auf einem Modell erstellt wird, das mit dem EF-Designer erstellt wurde. Dies liegt daran, dass der Designer einen Kontext generiert, der von ObjectContext und Entitätsklassen abgeleitet wurde, die von EntityObject abgeleitet wurden. Wenn Sie die Option Datenbank verwenden, können Sie den besten Code für die Interaktion mit dieser API-Oberfläche schreiben.

Die EF-Designer für Visual Studio 2012 und Visual Studio 2013 generieren einen Kontext, der von DbContext zusammen mit einfachen POCO-Entitätsklassen abstammt. Mit Visual Studio 2010 wird empfohlen, eine Codegenerierungsvorlage auszutauschen, die DbContext verwendet, wie weiter unten in dieser exemplarischen Vorgehensweise beschrieben.

Wenn Sie die DbContext-API-Oberfläche verwenden, sollten Sie beim Erstellen einer neuen Datenquelle die Option **Objekt** verwenden, wie in dieser exemplarischen Vorgehensweise gezeigt.

Bei Bedarf können Sie für Modelle, die mit dem EF-Designer erstellt wurden, [auf ObjectContext-basierte Codegenerierung zurückgreifen.](~/ef6/modeling/designer/codegen/legacy-objectcontext.md)

## <a name="pre-requisites"></a>Voraussetzungen

Sie müssen Visual Studio 2013, Visual Studio 2012 oder Visual Studio 2010 installiert haben, um diese exemplarische Vorgehensweise abzuschließen.

Wenn Sie Visual Studio 2010 verwenden, müssen Sie auch NuGet installieren. Weitere Informationen finden Sie unter [Installieren von NuGet](https://docs.microsoft.com/nuget/install-nuget-client-tools).  

## <a name="create-the-application"></a>Erstellen der Anwendung

-   Öffnen Sie Visual Studio.
-   **Datei&gt; -&gt; Neu - Projekt....**
-   Wählen Sie **Windows** im linken Bereich und **WPFApplication** im rechten Bereich aus.
-   Geben Sie **WPFwithEFSample** als Namen ein
-   Wählen Sie **OK**

## <a name="install-the-entity-framework-nuget-package"></a>Installieren des Entity Framework NuGet-Pakets

-   Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das **WinFormswithEFSample-Projekt**
-   **NuGet-Pakete verwalten...**
-   Wählen Sie im Dialogfeld NuGet-Pakete verwalten die Registerkarte **Online** aus, und wählen Sie das **EntityFramework-Paket** aus.
-   Klicken Sie auf **Installieren**  
    >[!NOTE]
    > Zusätzlich zur EntityFramework-Assembly wird auch ein Verweis auf System.ComponentModel.DataAnnotations hinzugefügt. Wenn das Projekt einen Verweis auf System.Data.Entity enthält, wird es entfernt, wenn das EntityFramework-Paket installiert wird. Die System.Data.Entity-Assembly wird nicht mehr für Entity Framework 6-Anwendungen verwendet.

## <a name="define-a-model"></a>Definieren eines Modells

In dieser exemplarischen Vorgehensweise können Sie ein Modell mit Code First oder ef Designer implementieren. Führen Sie einen der beiden folgenden Abschnitte aus.

### <a name="option-1-define-a-model-using-code-first"></a>Option 1: Definieren eines Modells mit Code First

In diesem Abschnitt wird gezeigt, wie Sie ein Modell und die zugehörige Datenbank mit Code First erstellen. Fahren Sie mit dem nächsten Abschnitt fort **(Option 2: Definieren eines Modells mit Database First),** wenn Sie lieber Database First verwenden, um Ihr Modell mithilfe des EF-Designers aus einer Datenbank zurückzuentwickeln.

Wenn Sie Code First-Entwicklung verwenden, beginnen Sie in der Regel mit dem Schreiben von .NET Framework-Klassen, die Ihr konzeptionelles (Domänen-)Modell definieren.

-   Fügen Sie dem **WPFwithEFSample** eine neue Klasse hinzu:
    -   Rechtsklick auf den Projektnamen
    -   Wählen **Sie Hinzufügen**, dann Neues **Element**
    -   Klasse **Class** auswählen und **Produkt** für den Klassennamen eingeben
-   Ersetzen Sie die **Produktklassendefinition** durch den folgenden Code:

``` csharp
    namespace WPFwithEFSample
    {
        public class Product
        {
            public int ProductId { get; set; }
            public string Name { get; set; }

            public int CategoryId { get; set; }
            public virtual Category Category { get; set; }
        }
    }

-   Add a **Category** class with the following definition:

    using System.Collections.ObjectModel;

    namespace WPFwithEFSample
    {
        public class Category
        {
            public Category()
            {
                this.Products = new ObservableCollection<Product>();
            }

            public int CategoryId { get; set; }
            public string Name { get; set; }

            public virtual ObservableCollection<Product> Products { get; private set; }
        }
    }
```

Die **Products-Eigenschaft** für die **Kategorieklasse** und **die Kategorie-Eigenschaft** in der **Produktklasse** sind Navigationseigenschaften. In Entity Framework bieten Navigationseigenschaften eine Möglichkeit, eine Beziehung zwischen zwei Entitätstypen zu navigieren.

Zusätzlich zum Definieren von Entitäten müssen Sie eine Klasse definieren, die&lt;von&gt; DbContext abstammt und DbSet TEntity-Eigenschaften verfügbar macht. Die DbSet&lt;&gt; TEntity-Eigenschaften informieren den Kontext darüber, welche Typen sie in das Modell aufnehmen möchten.

Eine Instanz des abgeleiteten DbContext-Typs verwaltet die Entitätsobjekte während der Laufzeit, die das Auffüllen von Objekten mit Daten aus einer Datenbank, die Änderungsnachverfolgung und das Beibehalten von Daten in der Datenbank umfasst.

-   Fügen Sie dem Projekt eine neue **ProductContext-Klasse** mit der folgenden Definition hinzu:

``` csharp
    using System.Data.Entity;

    namespace WPFwithEFSample
    {
        public class ProductContext : DbContext
        {
            public DbSet<Category> Categories { get; set; }
            public DbSet<Product> Products { get; set; }
        }
    }
```

Kompilieren Sie das Projekt.

### <a name="option-2-define-a-model-using-database-first"></a>Option 2: Definieren eines Modells mithilfe von Database First

In diesem Abschnitt wird gezeigt, wie Sie Database First verwenden, um Ihr Modell mithilfe des EF-Designers aus einer Datenbank zurückzuentwickeln. Wenn Sie den vorherigen Abschnitt abgeschlossen haben **(Option 1: Definieren eines Modells mit Code First),** überspringen Sie diesen Abschnitt, und gehen Sie direkt zum Abschnitt **Lazy Loading.**

#### <a name="create-an-existing-database"></a>Erstellen einer vorhandenen Datenbank

Wenn Sie auf eine vorhandene Datenbank abzielen, wird sie in der Regel bereits erstellt, aber für diese exemplarische Vorgehensweise müssen wir eine Datenbank für den Zugriff erstellen.

Der Datenbankserver, der mit Visual Studio installiert wird, unterscheidet sich je nach installierter Version von Visual Studio:

-   Wenn Sie Visual Studio 2010 verwenden, erstellen Sie eine SQL Express-Datenbank.
-   Wenn Sie Visual Studio 2012 verwenden, erstellen Sie eine [LocalDB-Datenbank.](https://msdn.microsoft.com/library/hh510202.aspx)

Lassen Sie uns fortfahren und die Datenbank generieren.

-   **Ansicht&gt; - Server-Explorer**
-   Rechtsklick auf **Datenverbindungen -&gt; Verbindung hinzufügen...**
-   Wenn Sie keine Verbindung mit einer Datenbank aus Server Explorer hergestellt haben, müssen Sie Microsoft SQL Server als Datenquelle auswählen.

    ![Datenquelle ändern](~/ef6/media/changedatasource.png)

-   Stellen Sie eine Verbindung mit LocalDB oder SQL Express her, je nachdem, welche installiert wurde, und geben Sie **Produkte** als Datenbanknamen ein.

    ![Hinzufügen von Connection LocalDB](~/ef6/media/addconnectionlocaldb.png)

    ![Hinzufügen von Connection Express](~/ef6/media/addconnectionexpress.png)

-   Wählen Sie **OK** und Sie werden gefragt, ob Sie eine neue Datenbank erstellen möchten, wählen Sie **Ja**

    ![Erstellen einer Datenbank](~/ef6/media/createdatabase.png)

-   Die neue Datenbank wird nun im Server Explorer angezeigt, mit der rechten Maustaste darauf geklickt und **Neue Abfrage** ausgewählt.
-   Kopieren Sie die folgende SQL-Abfrage in die neue Abfrage, klicken Sie dann mit der rechten Maustaste auf die Abfrage, und wählen Sie **Ausführen aus.**

``` SQL
    CREATE TABLE [dbo].[Categories] (
        [CategoryId] [int] NOT NULL IDENTITY,
        [Name] [nvarchar](max),
        CONSTRAINT [PK_dbo.Categories] PRIMARY KEY ([CategoryId])
    )

    CREATE TABLE [dbo].[Products] (
        [ProductId] [int] NOT NULL IDENTITY,
        [Name] [nvarchar](max),
        [CategoryId] [int] NOT NULL,
        CONSTRAINT [PK_dbo.Products] PRIMARY KEY ([ProductId])
    )

    CREATE INDEX [IX_CategoryId] ON [dbo].[Products]([CategoryId])

    ALTER TABLE [dbo].[Products] ADD CONSTRAINT [FK_dbo.Products_dbo.Categories_CategoryId] FOREIGN KEY ([CategoryId]) REFERENCES [dbo].[Categories] ([CategoryId]) ON DELETE CASCADE
```

#### <a name="reverse-engineer-model"></a>Reverse Engineering-Modell

Wir verwenden Entity Framework Designer, der als Teil von Visual Studio enthalten ist, um unser Modell zu erstellen.

-   **Projekt&gt; - Neues Element hinzufügen...**
-   Wählen Sie **Daten** aus dem linken Menü aus, und **ADO.NET Entitätsdatenmodell**
-   Geben Sie **ProductModel** als Namen ein und klicken Sie auf **OK**
-   Dadurch wird der **Entitätsdatenmodell-Assistent** gestartet.
-   Wählen **Sie Aus Datenbank generieren** aus, und klicken Sie auf **Weiter**

    ![Auswählen von Modellinhalten](~/ef6/media/choosemodelcontents.png)

-   Wählen Sie die Verbindung zur Datenbank aus, die Sie im ersten Abschnitt erstellt haben, geben Sie **ProductContext** als Namen der Verbindungszeichenfolge ein, und klicken Sie auf **Weiter.**

    ![Wählen Sie Ihre Verbindung](~/ef6/media/chooseyourconnection.png)

-   Klicken Sie auf das Kontrollkästchen neben "Tabellen", um alle Tabellen zu importieren, und klicken Sie auf "Fertig stellen".

    ![Wählen Sie Ihre Objekte](~/ef6/media/chooseyourobjects.png)

Sobald der Reverse Engineering-Prozess abgeschlossen ist, wird das neue Modell zu Ihrem Projekt hinzugefügt und für die Anzeige im Entity Framework Designer geöffnet. Außerdem wurde Ihrem Projekt eine App.config-Datei mit den Verbindungsdetails für die Datenbank hinzugefügt.

#### <a name="additional-steps-in-visual-studio-2010"></a>Weitere Schritte in Visual Studio 2010

Wenn Sie in Visual Studio 2010 arbeiten, müssen Sie den EF-Designer aktualisieren, um die EF6-Codegenerierung zu verwenden.

-   Klicken Sie mit der rechten Maustaste auf eine leere Stelle Ihres Modells im EF-Designer und wählen Sie **Codegenerierungselement hinzufügen...**
-   Wählen Sie **Online-Vorlagen** aus dem linken Menü und suchen Sie nach **DbContext**
-   Wählen Sie den **EF 6.x\#DbContext Generator für C** , geben Sie **ProductsModel** als Namen ein und klicken Sie auf Hinzufügen

#### <a name="updating-code-generation-for-data-binding"></a>Aktualisieren der Codegenerierung für die Datenbindung

EF generiert Code aus Ihrem Modell mithilfe von T4-Vorlagen. Die Vorlagen, die mit Visual Studio ausgeliefert oder aus dem Visual Studio-Katalog heruntergeladen wurden, sind für den allgemeinen Gebrauch vorgesehen. Dies bedeutet, dass die aus diesen&lt;Vorlagen&gt; generierten Entitäten über einfache ICollection T-Eigenschaften verfügen. Beim Festlegen der Datenbindung mit WPF ist es jedoch wünschenswert, **ObservableCollection** für Auflistungseigenschaften zu verwenden, damit WPF die an den Auflistungen vorgenommenen Änderungen nachverfolgen kann. Zu diesem Zweck werden wir die Vorlagen so ändern, dass sie ObservableCollection verwenden.

-   Öffnen Sie den **Projektmappen-Explorer,** und suchen Sie die **Datei ProductModel.edmx**
-   Suchen Sie die **ProductModel.tt** Datei, die unter der Datei ProductModel.edmx verschachtelt wird.

    ![WPF-Produktmodellvorlage](~/ef6/media/wpfproductmodeltemplate.png)

-   Doppelklicken Sie auf die ProductModel.tt Datei, um sie im Visual Studio-Editor zu öffnen.
-   Suchen und ersetzen Sie die beiden Vorkommen von "**ICollection**" durch "**ObservableCollection**". Diese befinden sich etwa an den Linien 296 und 484.
-   Suchen und ersetzen Sie das erste Vorkommen von "**HashSet**" durch "**ObservableCollection**". Dieses Vorkommen befindet sich ungefähr in Zeile 50. **Ersetzen Sie nicht** das zweite Vorkommen von HashSet, das später im Code gefunden wurde.
-   Suchen und ersetzen Sie das einzige Vorkommen von "**System.Collections.Generic**" mit "**System.Collections.ObjectModel**". Dieser befindet sich ungefähr an der Linie 424.
-   Speichern Sie die ProductModel.tt Datei. Dies sollte dazu führen, dass der Code für Entitäten neu generiert wird. Wenn sich der Code nicht automatisch regeneriert, klicken Sie mit der rechten Maustaste auf ProductModel.tt und wählen Sie "Custom Tool ausführen".

Wenn Sie nun die Category.cs Datei öffnen (die unter ProductModel.tt verschachtelt ist), sollten Sie sehen, dass die Produkte-Auflistung den Typ **ObservableCollection&lt;&gt;Product**hat.

Kompilieren Sie das Projekt.

## <a name="lazy-loading"></a>Lazy Loading

Die **Products-Eigenschaft** für die **Kategorieklasse** und **die Kategorie-Eigenschaft** in der **Produktklasse** sind Navigationseigenschaften. In Entity Framework bieten Navigationseigenschaften eine Möglichkeit, eine Beziehung zwischen zwei Entitätstypen zu navigieren.

EF bietet Ihnen die Möglichkeit, verwandte Entitäten automatisch aus der Datenbank zu laden, wenn Sie zum ersten Mal auf die Navigationseigenschaft zugreifen. Beachten Sie bei dieser Art des Ladens (als "Lazy Loading" bezeichnet), dass beim ersten Zugriff auf jede Navigationseigenschaft eine separate Abfrage für die Datenbank ausgeführt wird, wenn sich der Inhalt nicht bereits im Kontext befindet.

Bei der Verwendung von POCO-Entitätstypen erreicht EF das verzögerte Laden, indem Instanzen abgeleiteter Proxytypen während der Laufzeit erstellt und dann virtuelle Eigenschaften in Ihren Klassen überschreiben, um den Lade-Hook hinzuzufügen. Um das verzögerte Laden verwandter Objekte zu erhalten, müssen Sie Navigationseigenschaften-Getter als **öffentlich** und **virtuell** deklarieren **(überschreibbar** in Visual Basic), und Ihre Klasse darf nicht **versiegelt** werden **(NotOverridable** in Visual Basic). Bei Verwendung von Database First werden Navigationseigenschaften automatisch virtuell gemacht, um das verzögerte Laden zu ermöglichen. Im Abschnitt Code First haben wir uns entschieden, die Navigationseigenschaften aus dem gleichen Grund virtuell zu machen

## <a name="bind-object-to-controls"></a>Binden von Objekt an Steuerelemente

Fügen Sie die Klassen hinzu, die im Modell als Datenquellen für diese WPF-Anwendung definiert sind.

-   Doppelklicken Sie im Projektmappen-Explorer auf **MainWindow.xaml,** um das Hauptformular zu öffnen.
-   Wählen Sie im Hauptmenü **Projekt -&gt; Neue Datenquelle hinzufügen ...**
    (In Visual Studio 2010 müssen Sie Daten auswählen **-&gt; Neue Datenquelle hinzufügen...**)
-   Wählen Sie im Fenster Datenquellentyp auswählen **Objekt** aus, und klicken Sie auf **Weiter**
-   Im Dialog "Datenobjekte auswählen" das **WPFwithEFSample** zweimal aufundschließen und **Kategorie**  
    *Es ist nicht erforderlich, die **Produktdatenquelle** auszuwählen, da wir über die Eigenschaft des **Produkts**in der **Kategorie-Datenquelle** darauf gelangen.*  

    ![Datenobjekte auswählen](~/ef6/media/selectdataobjects.png)

-   Klicken Sie auf **Fertig stellen**.
-   Das Fenster Datenquellen wird neben dem Fenster MainWindow.xaml geöffnet *Wenn das Fenster Datenquellen nicht angezeigt wird, wählen Sie **Ansicht -&gt; Andere Windows-Datenquellen&gt; ** * aus.
-   Drücken Sie das Pin-Symbol, damit das Fenster Datenquellen nicht automatisch ausgeblendet wird. Möglicherweise müssen Sie auf die Schaltfläche "Aktualisieren" klicken, wenn das Fenster bereits sichtbar war.

    ![Projektmappen-Explorer](~/ef6/media/datasources.png)

-   Wählen Sie die **Kategorie-Datenquelle** aus, und ziehen Sie sie auf das Formular.

Das Folgende ist passiert, als wir diese Quelle gezogen haben:

-   Die **resourceViewSource-Ressource** und das **categoryDataGrid-Steuerelement** wurden XAML hinzugefügt. 
-   Die DataContext-Eigenschaft für das übergeordnete Grid-Element wurde auf "-StaticResource **categoryViewSource"** festgelegt.Die **categoryViewSource-Ressource** dient als Bindungsquelle für das äußere\\übergeordnete Grid-Element. Die inneren Grid-Elemente erben dann den DataContext-Wert vom übergeordneten Grid (die ItemsSource-Eigenschaft von categoryDataGrid ist auf "-Bindung" festgelegt).

``` xml
    <Window.Resources>
        <CollectionViewSource x:Key="categoryViewSource"
                                d:DesignSource="{d:DesignInstance {x:Type local:Category}, CreateList=True}"/>
    </Window.Resources>
    <Grid DataContext="{StaticResource categoryViewSource}">
        <DataGrid x:Name="categoryDataGrid" AutoGenerateColumns="False" EnableRowVirtualization="True"
                    ItemsSource="{Binding}" Margin="13,13,43,191"
                    RowDetailsVisibilityMode="VisibleWhenSelected">
            <DataGrid.Columns>
                <DataGridTextColumn x:Name="categoryIdColumn" Binding="{Binding CategoryId}"
                                    Header="Category Id" Width="SizeToHeader"/>
                <DataGridTextColumn x:Name="nameColumn" Binding="{Binding Name}"
                                    Header="Name" Width="SizeToHeader"/>
            </DataGrid.Columns>
        </DataGrid>
    </Grid>
```

## <a name="adding-a-details-grid"></a>Hinzufügen eines Details-Rasters

Nun, da wir ein Raster zum Anzeigen von Kategorien haben, fügen wir ein Detailraster hinzu, um die zugehörigen Produkte anzuzeigen.

-   Wählen Sie die **Products-Eigenschaft** unter der **Kategorie-Datenquelle** aus, und ziehen Sie sie auf das Formular.
    -   Die **KategorieProductsViewSource-Ressource** und **das ProductDataGrid-Raster** werden XAML hinzugefügt.
    -   Der Bindungspfad für diese Ressource ist auf Produkte festgelegt.
    -   Das WPF-Datenbindungsframework stellt sicher, dass nur Produkte, die sich auf die ausgewählte Kategorie beziehen, in **productDataGrid** angezeigt werden.
-   Ziehen Sie in der Toolbox die **Schaltfläche** auf das Formular. Legen Sie die **Name-Eigenschaft** auf **buttonSave** und die **Content-Eigenschaft** auf **Speichern**fest.

Das Formular sollte wie folgt aussehen:

![Designer](~/ef6/media/designer.png) 

## <a name="add-code-that-handles-data-interaction"></a>Code hinzufügen, der die Dateninteraktion behandelt

Es ist an der Zeit, dem Hauptfenster einige Ereignishandler hinzuzufügen.

-   Klicken Sie im XAML-Fenster auf das ** &lt;Fensterelement,** das das Hauptfenster
-   Wählen Sie im **Eigenschaftenfenster** **Ereignisse** oben rechts aus, und doppelklicken Sie dann auf das Textfeld rechts neben der **Beschriftung geladen**

    ![Eigenschaften des Hauptfensters](~/ef6/media/mainwindowproperties.png)

-   Fügen Sie auch das **Click-Ereignis** für die **Schaltfläche Speichern** hinzu, indem Sie im Designer auf die Schaltfläche Speichern doppelklicken. 

Dadurch gelangen Sie zum Code dahinter für das Formular, wir bearbeiten nun den Code, um den ProductContext zum Ausführen des Datenzugriffs zu verwenden. Aktualisieren Sie den Code für das MainWindow wie unten gezeigt.

Der Code deklariert eine lang andauernde Instanz von **ProductContext**. Das **ProductContext-Objekt** wird zum Abfragen und Speichern von Daten in der Datenbank verwendet. Die **Dispose()** auf der **ProductContext-Instanz** wird dann von der überschriebenen **OnClosing-Methode** aufgerufen.Die Codekommentare enthalten Details dazu, was der Code tut.

``` csharp
    using System.Data.Entity;
    using System.Linq;
    using System.Windows;

    namespace WPFwithEFSample
    {
        public partial class MainWindow : Window
        {
            private ProductContext _context = new ProductContext();
            public MainWindow()
            {
                InitializeComponent();
            }

            private void Window_Loaded(object sender, RoutedEventArgs e)
            {
                System.Windows.Data.CollectionViewSource categoryViewSource =
                    ((System.Windows.Data.CollectionViewSource)(this.FindResource("categoryViewSource")));

                // Load is an extension method on IQueryable,
                // defined in the System.Data.Entity namespace.
                // This method enumerates the results of the query,
                // similar to ToList but without creating a list.
                // When used with Linq to Entities this method
                // creates entity objects and adds them to the context.
                _context.Categories.Load();

                // After the data is loaded call the DbSet<T>.Local property
                // to use the DbSet<T> as a binding source.
                categoryViewSource.Source = _context.Categories.Local;
            }

            private void buttonSave_Click(object sender, RoutedEventArgs e)
            {
                // When you delete an object from the related entities collection
                // (in this case Products), the Entity Framework doesn’t mark
                // these child entities as deleted.
                // Instead, it removes the relationship between the parent and the child
                // by setting the parent reference to null.
                // So we manually have to delete the products
                // that have a Category reference set to null.

                // The following code uses LINQ to Objects
                // against the Local collection of Products.
                // The ToList call is required because otherwise the collection will be modified
                // by the Remove call while it is being enumerated.
                // In most other situations you can use LINQ to Objects directly
                // against the Local property without using ToList first.
                foreach (var product in _context.Products.Local.ToList())
                {
                    if (product.Category == null)
                    {
                        _context.Products.Remove(product);
                    }
                }

                _context.SaveChanges();
                // Refresh the grids so the database generated values show up.
                this.categoryDataGrid.Items.Refresh();
                this.productsDataGrid.Items.Refresh();
            }

            protected override void OnClosing(System.ComponentModel.CancelEventArgs e)
            {
                base.OnClosing(e);
                this._context.Dispose();
            }
        }

    }
```

## <a name="test-the-wpf-application"></a>Testen der WPF-Anwendung

-   Kompilieren Sie die Anwendung, und führen Sie sie aus. Wenn Sie Code First verwendet haben, werden Sie feststellen, dass eine **WPFwithEFSample.ProductContext-Datenbank** für Sie erstellt wird.
-   Geben Sie einen Kategorienamen im oberen Raster und Die Produktnamen im unteren Raster *ein, geben Sie nichts in ID-Spalten ein, da der Primärschlüssel von der Datenbank generiert wird.*

    ![Hauptfenster mit neuen Kategorien und Produkten](~/ef6/media/screen1.png)

-   Drücken Sie die **Schaltfläche Speichern,** um die Daten in der Datenbank zu speichern

Nach dem Aufruf von **DbContexts SaveChanges()** werden die IDs mit den von der Datenbank generierten Werten aufgefüllt. Da wir **Refresh()** nach **SaveChanges()** aufgerufen haben, werden die **DataGrid-Steuerelemente** auch mit den neuen Werten aktualisiert.

![Hauptfenster mit bestückten IDs](~/ef6/media/screen2.png)

## <a name="additional-resources"></a>Weitere Ressourcen

Weitere Informationen zur Datenbindung an Sammlungen mit WPF finden Sie in [diesem Thema](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-overview#binding-to-collections) in der WPF-Dokumentation.  
