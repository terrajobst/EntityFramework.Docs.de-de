---
title: Datenbindung mit WPF – EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: e90d48e6-bea5-47ef-b756-7b89cce4daf0
caps.latest.revision: 3
ms.openlocfilehash: 1756ec14fe83d80199b6040bd345dc2fe6294281
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2018
ms.locfileid: "39121138"
---
# <a name="databinding-with-wpf"></a>Datenbindung mit WPF
Schritt für Schritt dieser exemplarischen Vorgehensweise zeigt, wie POCO-Typen an WPF-Steuerelemente in einem "Master / Detail-Formular gebunden wird. Die Anwendung verwendet Entity Framework-APIs, um Objekte mit Daten aus der Datenbank zu füllen, Nachverfolgen von Änderungen und Speichern von Daten in der Datenbank.

Das Modell definiert zwei Typen, die Teilnahme an 1: n Beziehung: **Kategorie** (principal\\master) und **Produkt** (abhängige\\Detail). Anschließend werden Visual Studio-Tools verwendet, um in das Modell, das WPF-Steuerelemente definierten Typen zu binden. Das WPF-Datenbindung-Framework ermöglicht die Navigation zwischen verknüpften Objekten: Auswählen von Zeilen in der die Masteransicht bewirkt, dass die Detailansicht, um mit den entsprechenden untergeordneten Daten zu aktualisieren.

Die Screenshots und Codebeispiele in dieser exemplarischen Vorgehensweise stammen aus Visual Studio 2013, aber Sie können in dieser exemplarischen Vorgehensweise mit Visual Studio 2012 oder Visual Studio 2010 ausführen.

## <a name="use-the-object-option-for-creating-wpf-data-sources"></a>Verwenden Sie die Option 'Object' zum Erstellen von WPF-Datenquellen

Mit früheren Version von Entity Framework wir verwendet, um die Verwendung wird empfohlen, die **Datenbank** option erstellen eine neue Datenquelle basierend auf einem Modell mit dem EF Designer erstellt. Dies war, da der Designer generieren würden, ein, die von ObjectContext abgeleiteten Kontext und Entitätsklassen, die von "EntityObject" abgeleitet. Mit der Option können Sie den besten Code für die Interaktion mit dieser API-Oberfläche zu schreiben.

Die EF-Designer für Visual Studio 2012 und Visual Studio 2013 generieren einen Kontext an, der von "DbContext" abgeleitet, sowie einfache POCO-Entitätsklassen wird. Es wird empfohlen, mit Visual Studio 2010 austauschen, um eine Vorlage für die codegenerierung, die "DbContext" verwendet wird, wie weiter unten in dieser exemplarischen Vorgehensweise beschrieben.

Bei Verwendung die DbContext-API-Oberfläche verwenden Sie die **Objekt** option beim Erstellen einer neuen Datenquelle, wie in dieser exemplarischen Vorgehensweise gezeigt.

Bei Bedarf können Sie [codegenerierung ObjectContext basierend wiederherstellen](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) für Modelle, die mit dem EF Designer erstellt.

## <a name="pre-requisites"></a>Voraussetzungen

Sie müssen Visual Studio 2013, Visual Studio 2012 oder Visual Studio 2010 installiert werden, um diese exemplarische Vorgehensweise abgeschlossen haben.

Wenn Sie Visual Studio 2010 verwenden, müssen Sie auch NuGet zu installieren. Weitere Informationen finden Sie unter [Installing NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).  

## <a name="create-the-application"></a>Erstellen der Anwendung

-   Öffnen Sie Visual Studio.
-   **Datei -&gt; neu –&gt; Projekt...**
-   Wählen Sie **Windows** im linken Bereich und **WPFApplication** im rechten Bereich
-   Geben Sie **WPFwithEFSample** als Name
-   Wählen Sie **OK** aus.

## <a name="install-the-entity-framework-nuget-package"></a>Installieren Sie das Entity Framework-NuGet-Paket

-   Im Projektmappen-Explorer mit der Maustaste auf die **WinFormswithEFSample** Projekt
-   Wählen Sie **NuGet-Pakete verwalten...**
-   Wählen Sie in das Dialogfeld "NuGet-Pakete verwalten" die **Online** Registerkarte, und wählen Sie die **EntityFramework** Paket
-   Klicken Sie auf **installieren**  
    >[!NOTE]
    > Zusätzlich zu den EntityFramework-Assembly wird ein Verweis auf System.ComponentModel.DataAnnotations ebenfalls hinzugefügt. Wenn das Projekt einen Verweis auf System.Data.Entity verfügt, wird dann es entfernt, wenn das EntityFramework-Paket installiert wird. Die Assembly System.Data.Entity wird nicht mehr für Entity Framework 6-Anwendungen verwendet werden.

## <a name="define-a-model"></a>Definieren eines Modells

In dieser exemplarischen Vorgehensweise können Sie ein Modell mithilfe von Code First oder dem EF Designer implementieren möchten. Führen Sie eine der beiden folgenden Abschnitte.

### <a name="option-1-define-a-model-using-code-first"></a>Option 1: Definieren eines Modells mit Code First

In diesem Abschnitt zeigt, wie Sie ein Modell und der zugehörigen Datenbank mithilfe von Code First zu erstellen. Fahren Sie mit dem nächsten Abschnitt (**Option 2: Definieren eines Modells mit Database First)** verwenden, wenn Sie stattdessen Database First Reverse Engineering für das Modell aus einer Datenbank mit dem EF-Designer

Bei Verwendung von Code First-Entwicklung beginnen Sie in der Regel durch das Schreiben von .NET Framework-Klassen, die das konzeptionelle (Domäne)-Modell zu definieren.

-   Fügen Sie eine neue Klasse, die **WPFwithEFSample:**
    -   Mit der rechten Maustaste auf den Namen des Projekts
    -   Wählen Sie **hinzufügen**, klicken Sie dann **neues Element**
    -   Wählen Sie **Klasse** , und geben Sie **Produkt** für den Namen der Klasse
-   Ersetzen Sie die **Produkt** -Klassendefinition durch den folgenden Code:

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

Die **Produkte** Eigenschaft für die **Kategorie** Klasse und **Kategorie** Eigenschaft für die **Produkt** -Klasse sind Navigationseigenschaften. Entity Framework bieten Ihnen eine Möglichkeit, eine Beziehung zwischen zwei Entitätstypen zu navigieren Navigationseigenschaften.

Zusätzlich zum Definieren von Entitäten, müssen Sie eine Klasse definieren, die von "DbContext" abgeleitet und stellt "DbSet"&lt;TEntity&gt; Eigenschaften. "DbSet"&lt;TEntity&gt; Eigenschaften können Sie den Kontext, die wissen, welche Typen im Modell enthalten sein sollen.

Eine Instanz des Typs "DbContext" abgeleitet verwaltet die Entitätsobjekte während der Laufzeit, einschließlich ausfüllenden Objekten mit Daten aus einer Datenbank, verfolgen und Speichern von Daten in der Datenbank zu ändern.

-   Fügen Sie einen neuen **ProductContext** Klasse, um das Projekt mit der folgenden Definition:

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

### <a name="option-2-define-a-model-using-database-first"></a>Option 2: Definieren eines Modells mit Database First

In diesem Abschnitt zeigt, wie mit Database First zum zurückentwickeln das Modell aus einer Datenbank mit dem EF Designer. Wenn Sie den vorherigen Abschnitt abgeschlossen (**Option 1: Definieren eines Modells mithilfe von Code First)**, klicken Sie dann diesen Abschnitt überspringen und sofort die **Lazy Loading** Abschnitt.

#### <a name="create-an-existing-database"></a>Erstellen Sie eine vorhandene Datenbank

In der Regel müssen, wenn Sie eine vorhandene Datenbank verwenden möchten, die sie bereits erstellt wird, aber in dieser exemplarischen Vorgehensweise wir zum Erstellen einer Datenbank auf.

Der Datenbankserver, der mit Visual Studio installiert ist, unterscheidet sich abhängig von der Version von Visual Studio, die Sie installiert haben:

-   Wenn Sie Visual Studio 2010 verwenden erstellen Sie eine SQL Express-Datenbank.
-   Wenn Sie Visual Studio 2012 verwenden, und klicken Sie dann Sie erstellen eine [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) Datenbank.

Wir jetzt, und Erstellen der Datenbank.

-   **Ansicht – Profiler -&gt; Server-Explorer**
-   Klicken Sie mit der rechten Maustaste auf **Datenverbindungen -&gt; Verbindung hinzufügen...**
-   Wenn Sie vor dem müssen Sie Microsoft SQL Server als Datenquelle wählen im Server-Explorer mit einer Datenbank verbunden haben

    ![ChangeDataSource](~/ef6/media/changedatasource.png)

-   Eine Verbindung mit LocalDB oder SQL Express, je nachdem, welches Sie installiert haben, und geben Sie **Produkte** als Datenbankname

    ![AddConnectionLocalDB](~/ef6/media/addconnectionlocaldb.png)

    ![AddConnectionExpress](~/ef6/media/addconnectionexpress.png)

-   Wählen Sie **OK** und Sie werden gefragt, ob Sie eine neue Datenbank, die auf erstellen möchten **Ja**

    ![CreateDatabase](~/ef6/media/createdatabase.png)

-   Die neue Datenbank wird jetzt im Server-Explorer angezeigt. mit der rechten Maustaste darauf und wählen Sie **neue Abfrage**
-   Kopieren Sie die folgende SQL-Anweisung in die neue Abfrage, und klicken Sie dann mit der rechten Maustaste auf die Abfrage, und wählen **ausführen**

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

#### <a name="reverse-engineer-model"></a>Reverse Engineering-Modells

Wir werden die Verwendung von Entity Framework Designer, die als Teil von Visual Studio enthalten ist, um unser Modell zu erstellen.

-   **Projekt -&gt; neues Element hinzufügen...**
-   Wählen Sie **Daten** im linken Menü und dann **ADO.NET Entity Data Model**
-   Geben Sie **ProductModel** als ein, und klicken Sie auf **OK**
-   Dadurch wird die **Entity Data Model-Assistenten**
-   Wählen Sie **aus Datenbank generieren** , und klicken Sie auf **weiter**

    ![ChooseModelContents](~/ef6/media/choosemodelcontents.png)

-   Wählen Sie die Verbindung mit der Datenbank, die Sie im ersten Abschnitt erstellt haben, geben Sie **ProductContext** als Namen für die Verbindungszeichenfolge und klicken Sie auf **weiter**

    ![ChooseYourConnection](~/ef6/media/chooseyourconnection.png)

-   Klicken Sie auf das Kontrollkästchen neben "Tabellen", um alle Tabellen importiert werden, und klicken Sie auf "Fertig stellen"

    ![ChooseYourObjects](~/ef6/media/chooseyourobjects.png)

Nach Abschluss der reverse-Engineering-Prozess ist das neue Modell dem Projekt hinzugefügt und eröffnet Ihnen im Entity Framework Designer angezeigt. Zu Ihrem Projekt mit den Verbindungsdetails für die Datenbank wurde auch eine Datei "App.config" hinzugefügt.

#### <a name="additional-steps-in-visual-studio-2010"></a>Zusätzliche Schritte in Visual Studio 2010

Wenn Sie in Visual Studio 2010 arbeiten, müssen Sie den EF Designer zur Verwendung von EF6-codegenerierung zu aktualisieren.

-   Mit der rechten Maustaste auf eine leere Stelle des Modells im EF Designer, und wählen Sie **Codegenerierungselement hinzufügen...**
-   Wählen Sie **Onlinevorlagen** aus dem linken Menü und die Suche nach **"DbContext"**
-   Wählen Sie die **EF 6.x DbContext Generator für C\#,** geben **ProductsModel** als den Namen, und klicken Sie auf Hinzufügen

#### <a name="updating-code-generation-for-data-binding"></a>Aktualisieren die Generierung von Code für die Datenbindung

EF generiert Code aus dem Modell mithilfe von T4-Vorlagen. Die Vorlagen in Visual Studio bereitgestellt oder von der Visual Studio Gallery heruntergeladen werden für die allgemeine Verwendung vorgesehen. Dies bedeutet, dass die Entitäten, die mit diesen Vorlagen generierten einfach ICollection&lt;T&gt; Eigenschaften. Bei der Daten für das Binden mit WPF es ist jedoch wünschenswert, verwenden Sie **ObservableCollection** für die Auflistungseigenschaften damit, die von WPF an die Sammlungen vorgenommenen Änderungen mitverfolgen kann. Aus diesem Grund werden wir so ändern Sie die Vorlagen, um ObservableCollection verwenden.

-   Öffnen der **Projektmappen-Explorer** und **ProductModel.edmx** Datei
-   Suchen der **ProductModel.tt** Datei, die unter der Datei ProductModel.edmx geschachtelt werden

    ![WpfProductModelTemplate](~/ef6/media/wpfproductmodeltemplate.png)

-   Doppelklicken Sie auf die Datei ProductModel.tt, um sie in Visual Studio-Editor öffnen
-   Suchen und Ersetzen Sie die beiden Instanzen der "**ICollection**"mit"**ObservableCollection**". Diese befinden sich ungefähr in Zeile 296 und 484.
-   Suchen und Ersetzen Sie das erste Vorkommen von "**HashSet**"mit"**ObservableCollection**". Dieses Ereignis befindet sich ungefähr in Zeile 50. **Nicht** beim zweite Vorkommen der HashSet finden Sie weiter unten in den Code zu ersetzen.
-   Suchen und Ersetzen Sie das einzige Vorkommen von "**System.Collections.Generic**"mit"**System.Collections.ObjectModel**". Dies ist ungefähr in Zeile 424 befindet.
-   Speichern Sie die ProductModel.tt-Datei. Dadurch sollte den Code für Entitäten neu generiert werden. Wenn der Code nicht automatisch neu generiert wird, klicken Sie mit der rechten Maustaste auf ProductModel.tt, und wählen Sie "Benutzerdefiniertes Tool ausführen".

Wenn Sie jetzt die Category.cs-Datei (mit unter ProductModel.tt geschachtelt ist) öffnen, und klicken Sie dann Sie sehen, dass die produktsammlung den Typ **ObservableCollection&lt;Produkt&gt;**.

Kompilieren Sie das Projekt.

## <a name="lazy-loading"></a>Lazy Loading

Die **Produkte** Eigenschaft für die **Kategorie** Klasse und **Kategorie** Eigenschaft für die **Produkt** -Klasse sind Navigationseigenschaften. Entity Framework bieten Ihnen eine Möglichkeit, eine Beziehung zwischen zwei Entitätstypen zu navigieren Navigationseigenschaften.

EF bietet Ihnen eine Option beim Laden von verknüpften Entitäten aus der Datenbank automatisch den ersten Zugriff auf die Navigationseigenschaft. Bedenken Sie mit dieser Art von Loading (verzögertes Laden bezeichnet), dass beim ersten Zugriff auf jede Navigationseigenschaft eine separate Abfrage für die Datenbank ausgeführt wird, wenn der Inhalt nicht bereits im Kontext ist.

Wenn Sie POCO-Entität-Typen zu verwenden, erreicht EF Lazy Load durch Erstellen von Instanzen der abgeleiteten Proxytypen während der Laufzeit, und klicken Sie dann überschreiben virtuelle Eigenschaften in den Klassen den Laden Hook hinzufügen. Um lazy Loading von verknüpften Objekten zu erhalten, müssen Sie deklarieren, Navigation Eigenschaftengetter als **öffentliche** und **virtuellen** (**Overridable** in Visual Basic), und Sie Klasse darf nicht sein. **versiegelten** (**NotOverridable** in Visual Basic). Wenn die Datenbank mithilfe von werden erste Navigationseigenschaften automatisch virtuelle zum Aktivieren von lazy Loading vorgenommen. Im Abschnitt mit Code First haben wir die Navigationseigenschaften aus demselben Grund virtuellen stellen

## <a name="bind-object-to-controls"></a>Binden Sie Objekt wird an Steuerelemente.

Fügen Sie die Klassen, die im Modell, als Datenquellen für dieses WPF-Anwendung definiert sind hinzu.

-   Doppelklicken Sie auf **"MainWindow.xaml"** im Projektmappen-Explorer, um das Hauptformular zu öffnen.
-   Wählen Sie im Hauptmenü **-Projekt –&gt; neue Datenquelle hinzufügen...**
    (in Visual Studio 2010 müssen Sie auswählen **Daten per Push –&gt; neue Datenquelle hinzufügen...** )
-   In der wählen Sie eine Data Source Typewindow **Objekt** , und klicken Sie auf **weiter**
-   Das Dialogfeld "Data-Objekte" Wählen Sie im Erweitern der **WPFwithEFSample** zwei Mal, und wählen **Kategorie**  
    *Besteht keine Notwendigkeit, wählen Sie die **Produkt** die Datenquelle, da wir mit erhält die **Produkt**des Eigenschaft für die **Kategorie** -Datenquelle*  

    ![SelectDataObjects](~/ef6/media/selectdataobjects.png)

-   Klicken Sie auf **Fertig stellen.**
-   Das Fenster "Datenquellen" wird geöffnet, neben dem Fenster "MainWindow.xaml" *, wenn das Fenster "Datenquellen" nicht angezeigt wird, wählen Sie **Ansicht – Profiler -&gt; andere Windows -&gt; -Datenquellen***
-   Drücken Sie das Symbol zum anheften, damit das Fenster "Datenquellen" nicht automatisch ausgeblendet. Sie müssen möglicherweise die Schaltfläche "Aktualisieren" erreicht, wenn das Fenster bereits sichtbar war.

    ![DataSources](~/ef6/media/datasources.png)

-   Wählen Sie die ** Kategorie ** Data source, und ziehen Sie es auf dem Formular.

Folgendes passiert, wenn wir diese Quelle gezogen:

-   Die **CategoryViewSource** Ressource und die ** CategoryDataGrid ** Steuerelement XAML hinzugefügt wurden. Weitere Informationen zu DataViewSources, finden Sie unter http://bea.stollnitz.com/blog/?p=387.
-   Die DataContext-Eigenschaft des übergeordneten Grid-Elements wurde festgelegt auf "{StaticResource **CategoryViewSource** }".  Die **CategoryViewSource** Ressourcen dient als Bindungsquelle für das äußere\\übergeordneten Grid-Elements. Das innere Rasterelemente erben klicken Sie dann den DataContext-Wert aus dem übergeordneten Raster (das CategoryDataGrid die ItemsSource-Eigenschaft ist auf "{Binding}" festgelegt). 

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

## <a name="adding-a-details-grid"></a>Hinzufügen eines Rasters Details

Nun, wir haben ein Raster aus, um Kategorien anzuzeigen, lassen Sie uns ein Raster für die zugehörigen Produkte hinzufügen.

-   Wählen Sie die ** Produkte **-Eigenschaft aus, unter der ** Kategorie ** Data source, und ziehen Sie es auf dem Formular.
    -   Die **CategoryProductsViewSource** Ressource und **ProductDataGrid** XAML Raster hinzugefügt werden
    -   Der Bindungspfad für diese Ressource ist auf Produkte festgelegt.
    -   WPF-Datenbindung Framework wird sichergestellt, dass nur Produkte, die im Zusammenhang mit der ausgewählten Kategorie angezeigt **ProductDataGrid**
-   Ziehen Sie aus der Toolbox **Schaltfläche** an das Formular. Festlegen der **Name** Eigenschaft **ButtonSave** und **Inhalt** Eigenschaft, um **speichern**.

Das Formular sollte etwa wie folgt aussehen:

![Designer](~/ef6/media/designer.png) 

## <a name="add-code-that-handles-data-interaction"></a>Fügen Sie Code, die Dateninteraktion behandelt.

Es ist Zeit, einige Ereignishandler an das Hauptfenster hinzuzufügen.

-   Klicken Sie im XAML-Fenster auf die  **&lt;Fenster** -Element, das Hauptfenster zur Auswahl
-   In der **Eigenschaften** wählen **Ereignisse** oben rechts, doppelklicken Sie auf das Textfeld rechts neben der **Loaded** Bezeichnung

    ![MainWindowProperties](~/ef6/media/mainwindowproperties.png)

-   Steigern der **klicken Sie auf** -Ereignis für die **speichern** Schaltfläche durch Doppelklicken auf die Schaltfläche "Speichern" im Designer. 

Dadurch wird des Codes-behind für das Formular, jetzt bearbeiten wir den Code, um die ProductContext zu verwenden, um die Daten zugreift. Aktualisieren Sie den Code für das MainWindow-Element, wie unten dargestellt.

Der Code deklariert eine lang ausgeführte Instanz der **ProductContext**. Die **ProductContext** Objekt wird zum Abfragen und Speichern von Daten in der Datenbank verwendet. Die **Dispose**() für die **ProductContext** Instanz wird dann aufgerufen, von der überschriebenen **OnClosing** Methode. Die Codekommentare enthalten Details darüber, was der Code bewirkt.

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

## <a name="test-the-wpf-application"></a>Testen Sie die WPF-Anwendung

-   Kompilieren Sie die Anwendung, und führen Sie sie aus. Wenn Sie Code First verwendet haben, sehen Sie, dass eine **WPFwithEFSample.ProductContext** Datenbank wird für Sie erstellt.
-   Geben Sie einen Kategorienamen in den oberen Raster und Produkt im unteren Raster *Geben Sie nicht alles in ID-Spalten, da der Primärschlüssel von der Datenbank generiert wurde*

    ![Bildschirm1](~/ef6/media/screen1.png)

-   Drücken Sie die **speichern** Schaltfläche, um die Daten in der Datenbank speichern

Nach dem Aufruf von "DbContext" des **"SaveChanges"**(-), die IDs werden mit der Datenbank generierte Werte aufgefüllt. Da wir aufgerufen **aktualisieren**() nach **"SaveChanges"**() die **DataGrid** Steuerelemente mit als auch den neuen Werten aktualisiert werden.

![Screen2](~/ef6/media/screen2.png)
