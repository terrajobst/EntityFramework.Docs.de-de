---
title: Datenbindung mit Windows Forms - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 80fc5062-2f1c-4dbd-ab6e-b99496784b36
ms.openlocfilehash: 48e6d997875a25a5954484f854953df69a267d05
ms.sourcegitcommit: 8d04a2ad98036f32ca70c77ce3040c5edb1cdf82
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2018
ms.locfileid: "44384851"
---
# <a name="databinding-with-winforms"></a>Datenbindung mit Windows Forms
Diese exemplarische Vorgehensweise veranschaulicht POCO-Typen an Windows Forms (WinForms) Steuerelemente in einem "Master / Detail-Formular zu binden. Die Anwendung verwendet Entity Framework-Objekte mit Daten aus der Datenbank zu füllen, Nachverfolgen von Änderungen und Beibehalten von Daten in der Datenbank.

Das Modell definiert zwei Typen, die Teilnahme an 1: n Beziehung: Kategorie (principal\\master) und das Produkt (abhängige\\Detail). Anschließend werden Visual Studio-Tools verwendet, um in das Modell, das die Windows Forms-Steuerelemente definierten Typen zu binden. Das Framework der Windows Forms-Datenbindung ermöglicht die Navigation zwischen verknüpften Objekten: Auswählen von Zeilen in der die Masteransicht bewirkt, dass die Detailansicht, um mit den entsprechenden untergeordneten Daten zu aktualisieren.

Die Screenshots und Codebeispiele in dieser exemplarischen Vorgehensweise stammen aus Visual Studio 2013, aber Sie können in dieser exemplarischen Vorgehensweise mit Visual Studio 2012 oder Visual Studio 2010 ausführen.

## <a name="pre-requisites"></a>Voraussetzungen

Sie müssen Visual Studio 2013, Visual Studio 2012 oder Visual Studio 2010 installiert werden, um diese exemplarische Vorgehensweise abgeschlossen haben.

Wenn Sie Visual Studio 2010 verwenden, müssen Sie auch NuGet zu installieren. Weitere Informationen finden Sie unter [Installing NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).

## <a name="create-the-application"></a>Erstellen der Anwendung

-   Öffnen Sie Visual Studio.
-   **Datei -&gt; neu –&gt; Projekt...**
-   Wählen Sie **Windows** im linken Bereich und **Windows FormsApplication** im rechten Bereich
-   Geben Sie **WinFormswithEFSample** als Name
-   Wählen Sie **OK** aus.

## <a name="install-the-entity-framework-nuget-package"></a>Installieren Sie das Entity Framework-NuGet-Paket

-   Im Projektmappen-Explorer mit der Maustaste auf die **WinFormswithEFSample** Projekt
-   Wählen Sie **NuGet-Pakete verwalten...**
-   Wählen Sie in das Dialogfeld "NuGet-Pakete verwalten" die **Online** Registerkarte, und wählen Sie die **EntityFramework** Paket
-   Klicken Sie auf **installieren**  
    > [!NOTE]
    > Zusätzlich zu den EntityFramework-Assembly wird ein Verweis auf System.ComponentModel.DataAnnotations ebenfalls hinzugefügt. Wenn das Projekt einen Verweis auf System.Data.Entity verfügt, wird dann es entfernt, wenn das EntityFramework-Paket installiert wird. Die Assembly System.Data.Entity wird nicht mehr für Entity Framework 6-Anwendungen verwendet werden.

## <a name="implementing-ilistsource-for-collections"></a>Implementieren der IListSource für Sammlungen

-Auflistung – Eigenschaften müssen die zum Aktivieren der bidirektionale Datenbindung, bei der Sortierung, bei Verwendung von Windows Forms IListSource-Schnittstelle implementieren. Zu diesem Zweck werden wir zum Erweitern der ObservableCollection IListSource-Funktionen hinzufügen.

-   Hinzufügen einer **ObservableListSource** Klasse, um das Projekt:
    -   Mit der rechten Maustaste auf den Namen des Projekts
    -   Wählen Sie **hinzufügen –&gt; neues Element**
    -   Wählen Sie **Klasse** , und geben Sie **ObservableListSource** für den Namen der Klasse
-   Ersetzen Sie den Code, der standardmäßig mit dem folgenden Code generiert:

*Diese Klasse ermöglicht die bidirektionale Daten, die Bindung sowie das Sortieren. Die Klasse ist von ObservableCollection&lt;T&gt; und fügt eine explizite Implementierung der IListSource. Die GetList()-Methode der IListSource wird implementiert, um eine Implementierung IBindingList zurückzugeben, die mit der ObservableCollection synchron bleibt. Die IBindingList-Implementierung von ToBindingList generiert das Sortieren unterstützt. Die ToBindingList Erweiterung-Methode ist in der EntityFramework-Assembly definiert.*

``` csharp
    using System.Collections;
    using System.Collections.Generic;
    using System.Collections.ObjectModel;
    using System.ComponentModel;
    using System.Diagnostics.CodeAnalysis;
    using System.Data.Entity;

    namespace WinFormswithEFSample
    {
        public class ObservableListSource<T> : ObservableCollection<T>, IListSource
            where T : class
        {
            private IBindingList _bindingList;

            bool IListSource.ContainsListCollection { get { return false; } }

            IList IListSource.GetList()
            {
                return _bindingList ?? (_bindingList = this.ToBindingList());
            }
        }
    }
```

## <a name="define-a-model"></a>Definieren eines Modells

In dieser exemplarischen Vorgehensweise können Sie ein Modell mithilfe von Code First oder dem EF Designer implementieren möchten. Führen Sie eine der beiden folgenden Abschnitte.

### <a name="option-1-define-a-model-using-code-first"></a>Option 1: Definieren eines Modells mit Code First

In diesem Abschnitt zeigt, wie Sie ein Modell und der zugehörigen Datenbank mithilfe von Code First zu erstellen. Fahren Sie mit dem nächsten Abschnitt (**Option 2: Definieren eines Modells mit Database First)** verwenden, wenn Sie stattdessen Database First Reverse Engineering für das Modell aus einer Datenbank mit dem EF-Designer

Bei Verwendung von Code First-Entwicklung beginnen Sie in der Regel durch das Schreiben von .NET Framework-Klassen, die das konzeptionelle (Domäne)-Modell zu definieren.

-   Fügen Sie einen neuen **Produkt** Projekt
-   Ersetzen Sie den Code, der standardmäßig mit dem folgenden Code generiert:

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;

    namespace WinFormswithEFSample
    {
        public class Product
        {
            public int ProductId { get; set; }
            public string Name { get; set; }

            public int CategoryId { get; set; }
            public virtual Category Category { get; set; }
        }
    }
```

-   Hinzufügen einer **Kategorie** Klasse, um das Projekt.
-   Ersetzen Sie den Code, der standardmäßig mit dem folgenden Code generiert:

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;

    namespace WinFormswithEFSample
    {
        public class Category
        {
            private readonly ObservableListSource<Product> _products =
                    new ObservableListSource<Product>();

            public int CategoryId { get; set; }
            public string Name { get; set; }
            public virtual ObservableListSource<Product> Products { get { return _products; } }
        }
    }
```

Zusätzlich zum Definieren von Entitäten, müssen Sie eine Klasse definieren, die von abgeleitet **"DbContext"** und macht **"DbSet"&lt;TEntity&gt;**  Eigenschaften. Die **"DbSet"** Eigenschaften können Sie den Kontext, die wissen, welche Typen im Modell enthalten sein sollen. Die **"DbContext"** und **"DbSet"** Typen werden in der EntityFramework-Assembly definiert.

Eine Instanz des Typs "DbContext" abgeleitet verwaltet die Entitätsobjekte während der Laufzeit, einschließlich ausfüllenden Objekten mit Daten aus einer Datenbank, verfolgen und Speichern von Daten in der Datenbank zu ändern.

-   Fügen Sie einen neuen **ProductContext** Klasse, um das Projekt.
-   Ersetzen Sie den Code, der standardmäßig mit dem folgenden Code generiert:

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Data.Entity;
    using System.Linq;
    using System.Text;

    namespace WinFormswithEFSample
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

    ![Datenquelle wechseln](~/ef6/media/changedatasource.png)

-   Eine Verbindung mit LocalDB oder SQL Express, je nachdem, welches Sie installiert haben, und geben Sie **Produkte** als Datenbankname

    ![Hinzufügen der Verbindung LocalDB](~/ef6/media/addconnectionlocaldb.png)

    ![Verbindung Express hinzufügen](~/ef6/media/addconnectionexpress.png)

-   Wählen Sie **OK** und Sie werden gefragt, ob Sie eine neue Datenbank, die auf erstellen möchten **Ja**

    ![Datenbank erstellen](~/ef6/media/createdatabase.png)

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

    ![Wählen Sie die Verbindung](~/ef6/media/chooseyourconnection.png)

-   Klicken Sie auf das Kontrollkästchen neben "Tabellen", um alle Tabellen importiert werden, und klicken Sie auf "Fertig stellen"

    ![Wählen Sie Ihre Objekte aus.](~/ef6/media/chooseyourobjects.png)

Nach Abschluss der reverse-Engineering-Prozess ist das neue Modell dem Projekt hinzugefügt und eröffnet Ihnen im Entity Framework Designer angezeigt. Zu Ihrem Projekt mit den Verbindungsdetails für die Datenbank wurde auch eine Datei "App.config" hinzugefügt.

#### <a name="additional-steps-in-visual-studio-2010"></a>Zusätzliche Schritte in Visual Studio 2010

Wenn Sie in Visual Studio 2010 arbeiten, müssen Sie den EF Designer zur Verwendung von EF6-codegenerierung zu aktualisieren.

-   Mit der rechten Maustaste auf eine leere Stelle des Modells im EF Designer, und wählen Sie **Codegenerierungselement hinzufügen...**
-   Wählen Sie **Onlinevorlagen** aus dem linken Menü und die Suche nach **"DbContext"**
-   Wählen Sie die **EF 6.x DbContext Generator für C\#,** geben **ProductsModel** als den Namen, und klicken Sie auf Hinzufügen

#### <a name="updating-code-generation-for-data-binding"></a>Aktualisieren die Generierung von Code für die Datenbindung

EF generiert Code aus dem Modell mithilfe von T4-Vorlagen. Die Vorlagen in Visual Studio bereitgestellt oder von der Visual Studio Gallery heruntergeladen werden für die allgemeine Verwendung vorgesehen. Dies bedeutet, dass die Entitäten, die mit diesen Vorlagen generierten einfach ICollection&lt;T&gt; Eigenschaften. Allerdings ist bei der Datenbindung es wünschenswert,-Auflistung – Eigenschaften verfügen, die IListSource zu implementieren. Aus diesem Grund wir die oben gezeigte ObservableListSource-Klasse erstellt, und wir werden nun die Vorlagen zu ändern ist dieser Klasse verwenden.

-   Öffnen der **Projektmappen-Explorer** und **ProductModel.edmx** Datei
-   Suchen der **ProductModel.tt** Datei, die unter der Datei ProductModel.edmx geschachtelt werden

    ![Produkt-Manager-Vorlage](~/ef6/media/productmodeltemplate.png)

-   Doppelklicken Sie auf die Datei ProductModel.tt, um sie in Visual Studio-Editor öffnen
-   Suchen und Ersetzen Sie die beiden Instanzen der "**ICollection**"mit"**ObservableListSource**". Diese befinden sich in etwa die Zeilen 296 und 484.
-   Suchen und Ersetzen Sie das erste Vorkommen von "**HashSet**"mit"**ObservableListSource**". Dieses Ereignis befindet sich ungefähr bei Zeile 50. **Nicht** beim zweite Vorkommen der HashSet finden Sie weiter unten in den Code zu ersetzen.
-   Speichern Sie die ProductModel.tt-Datei. Dadurch sollte den Code für Entitäten neu generiert werden. Wenn der Code nicht automatisch neu generiert wird, klicken Sie mit der rechten Maustaste auf ProductModel.tt, und wählen Sie "Benutzerdefiniertes Tool ausführen".

Wenn Sie jetzt die Category.cs-Datei (mit unter ProductModel.tt geschachtelt ist) öffnen, und klicken Sie dann Sie sehen, dass die produktsammlung den Typ **ObservableListSource&lt;Produkt&gt;**.

Kompilieren Sie das Projekt.

## <a name="lazy-loading"></a>Lazy Loading

Die **Produkte** Eigenschaft für die **Kategorie** Klasse und **Kategorie** Eigenschaft für die **Produkt** -Klasse sind Navigationseigenschaften. Entity Framework bieten Ihnen eine Möglichkeit, eine Beziehung zwischen zwei Entitätstypen zu navigieren Navigationseigenschaften.

EF bietet Ihnen eine Option beim Laden von verknüpften Entitäten aus der Datenbank automatisch den ersten Zugriff auf die Navigationseigenschaft. Bedenken Sie mit dieser Art von Loading (verzögertes Laden bezeichnet), dass beim ersten Zugriff auf jede Navigationseigenschaft eine separate Abfrage für die Datenbank ausgeführt wird, wenn der Inhalt nicht bereits im Kontext ist.

Wenn Sie POCO-Entität-Typen zu verwenden, erreicht EF Lazy Load durch Erstellen von Instanzen der abgeleiteten Proxytypen während der Laufzeit, und klicken Sie dann überschreiben virtuelle Eigenschaften in den Klassen den Laden Hook hinzufügen. Um lazy Loading von verknüpften Objekten zu erhalten, müssen Sie deklarieren, Navigation Eigenschaftengetter als **öffentliche** und **virtuellen** (**Overridable** in Visual Basic), und Sie Klasse darf nicht sein. **versiegelten** (**NotOverridable** in Visual Basic). Wenn die Datenbank mithilfe von werden erste Navigationseigenschaften automatisch virtuelle zum Aktivieren von lazy Loading vorgenommen. Im Abschnitt mit Code First haben wir die Navigationseigenschaften aus demselben Grund virtuellen stellen

## <a name="bind-object-to-controls"></a>Binden Sie Objekt wird an Steuerelemente.

Fügen Sie die Klassen, die im Modell, als Datenquellen für diese Windows Forms-Anwendung definiert sind hinzu.

-   Wählen Sie im Hauptmenü **-Projekt –&gt; neue Datenquelle hinzufügen...**
    (in Visual Studio 2010 müssen Sie auswählen **Daten per Push –&gt; neue Datenquelle hinzufügen...** )
-   Wählen Sie in der ein Fenster Datenquellentyp auswählen, **Objekt** , und klicken Sie auf **weiter**
-   Erweitern Sie das Dialogfeld "Data-Objekte" Wählen Sie im der **WinFormswithEFSample** zwei Mal, und wählen **Kategorie** besteht nicht erforderlich, wählen Sie die Produkt-Datenquelle, da wir, über des Produktanforderungen erhalten die Eigenschaft für die Kategorie-Datenquelle.

    ![Datenquelle](~/ef6/media/datasource.png)

-   Klicken Sie auf **Fertig stellen.** 
     *, Wenn das Fenster "Datenquellen" nicht angezeigt wird, wählen Sie *** Ansicht – Profiler -&gt; andere Windows -&gt; -Datenquellen**
-   Drücken Sie das Symbol zum anheften, damit das Fenster "Datenquellen" nicht automatisch ausgeblendet. Sie müssen möglicherweise die Schaltfläche "Aktualisieren" erreicht, wenn das Fenster bereits sichtbar war.

    ![Datenquelle 2](~/ef6/media/datasource2.png)

-   Doppelklicken Sie im Projektmappen-Explorer auf die **"Form1.cs"** Datei, die im Formular im Designer zu öffnen.
-   Wählen Sie die **Kategorie** Data source, und ziehen Sie es auf dem Formular. Standardmäßig wird ein neues DataGridView-Steuerelement (**CategoryDataGridView**) und Navigation-Symbolleisten-Steuerelemente werden in den Designer hinzugefügt. Diese Steuerelemente sind an die Bindungsquelle gebunden (**CategoryBindingSource**) und Bindung Navigator (**CategoryBindingNavigator**) Komponenten, die ebenfalls erstellt werden.
-   Die Spalten bearbeiten, auf die **CategoryDataGridView**. Wir möchten, legen Sie die **CategoryId** Spalte in den schreibgeschützten Modus. Der Wert für die **CategoryId** -Eigenschaft von der Datenbank generiert wird, nachdem wir die Daten zu speichern.
    -   Mit der rechten Maustaste in des DataGridView-Steuerelements, und wählen Sie Spalten bearbeiten...
    -   Wählen Sie die Kategorie-ID-Spalte und ReadOnly auf True festgelegt.
    -   Klicken Sie auf OK
-   Wählen Sie die Produkte aus der Datenquelle für die Kategorie aus, und ziehen Sie es auf dem Formular. Die ProductDataGridView und ProductBindingSource werden dem Formular hinzugefügt.
-   Bearbeiten Sie die Spalten der ProductDataGridView. Wir möchten die CategoryId und Kategorie Spalten ausblenden, und legen "ProductID" in den schreibgeschützten Modus. Der Wert für die Eigenschaft "ProductID" wird von der Datenbank generiert, nachdem wir die Daten zu speichern.
    -   Mit der rechten Maustaste in des DataGridView-Steuerelements, und wählen Sie **Spalten bearbeiten...** .
    -   Wählen Sie die **"ProductID"** Spalte mit **ReadOnly** zu **"true"**.
    -   Wählen Sie die **CategoryId** Spalte, und drücken Sie die **entfernen** Schaltfläche. Nicht nur die **Kategorie** Spalte.
    -   Drücken Sie **OK**.

    Bisher Serverkomponenten zugeordnet sind wir unsere DataGridView-Steuerelementen BindingSource im Designer. Im nächsten Abschnitt werden wir die CodeBehind-Code hinzufügen, categoryBindingSource.DataSource auf die Auflistung von Entitäten festgelegt wird, die zurzeit von "DbContext" nachverfolgt werden. Wenn wir Produkte gezogen und abgelegt, unter der Kategorie, der WinForms sorgte für die Eigenschaft productsBindingSource.DataSource CategoryBindingSource und productsBindingSource.DataMember-Eigenschaft auf Produkte einrichten. Aufgrund dieser Bindung werden nur die Produkte, die der derzeit ausgewählten Kategorie gehören in der ProductDataGridView angezeigt.
-   Aktivieren der **speichern** auf der Symbolleiste Navigation, indem Sie auf der rechten Maustaste und auswählen **aktiviert**.

    ![1-Formular-Designer](~/ef6/media/form1-designer.png)

-   Hinzufügen den Ereignishandler für das Speichern der Schaltfläche durch Doppelklicken auf die Schaltfläche. Den Ereignishandler hinzufügen wird und erhalten Sie eine der CodeBehind für das Formular. Der Code für die **CategoryBindingNavigatorSaveItem\_klicken Sie auf** -Ereignishandler wird im nächsten Abschnitt hinzugefügt werden.

## <a name="add-the-code-that-handles-data-interaction"></a>Fügen Sie den Code, der Dateninteraktion behandelt.

Wir fügen nun den Code, um die ProductContext zu verwenden, um die Daten zugreift. Aktualisieren Sie den Code für das Hauptformular-Fenster aus, wie unten dargestellt.

Der Code deklariert eine lang andauernde-Instanz der ProductContext. Das ProductContext-Objekt wird zum Abfragen und Speichern von Daten in der Datenbank verwendet. Die Dispose()-Methode in die ProductContext-Instanz wird dann von der überschriebenen OnClosing-Methode aufgerufen. Die Codekommentare enthalten Details darüber, was der Code bewirkt.

``` csharp
    using System;
    using System.Collections.Generic;
    using System.ComponentModel;
    using System.Data;
    using System.Drawing;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using System.Windows.Forms;
    using System.Data.Entity;

    namespace WinFormswithEFSample
    {
        public partial class Form1 : Form
        {
            ProductContext _context;
            public Form1()
            {
                InitializeComponent();
            }

            protected override void OnLoad(EventArgs e)
            {
                base.OnLoad(e);
                _context = new ProductContext();

                // Call the Load method to get the data for the given DbSet
                // from the database.
                // The data is materialized as entities. The entities are managed by
                // the DbContext instance.
                _context.Categories.Load();

                // Bind the categoryBindingSource.DataSource to
                // all the Unchanged, Modified and Added Category objects that
                // are currently tracked by the DbContext.
                // Note that we need to call ToBindingList() on the
                // ObservableCollection<TEntity> returned by
                // the DbSet.Local property to get the BindingList<T>
                // in order to facilitate two-way binding in WinForms.
                this.categoryBindingSource.DataSource =
                    _context.Categories.Local.ToBindingList();
            }

            private void categoryBindingNavigatorSaveItem_Click(object sender, EventArgs e)
            {
                this.Validate();

                // Currently, the Entity Framework doesn’t mark the entities
                // that are removed from a navigation property (in our example the Products)
                // as deleted in the context.
                // The following code uses LINQ to Objects against the Local collection
                // to find all products and marks any that do not have
                // a Category reference as deleted.
                // The ToList call is required because otherwise
                // the collection will be modified
                // by the Remove call while it is being enumerated.
                // In most other situations you can do LINQ to Objects directly
                // against the Local property without using ToList first.
                foreach (var product in _context.Products.Local.ToList())
                {
                    if (product.Category == null)
                    {
                        _context.Products.Remove(product);
                    }
                }

                // Save the changes to the database.
                this._context.SaveChanges();

                // Refresh the controls to show the values         
                // that were generated by the database.
                this.categoryDataGridView.Refresh();
                this.productsDataGridView.Refresh();
            }

            protected override void OnClosing(CancelEventArgs e)
            {
                base.OnClosing(e);
                this._context.Dispose();
            }
        }
    }
```

## <a name="test-the-windows-forms-application"></a>Testen Sie die Windows Forms-Anwendung

-   Kompilieren und ausführen, die die Funktionalität der Anwendung und testen können.

    ![1 bilden vor dem Speichern](~/ef6/media/form1beforesave.png)

-   Nach dem Speichern werden die speichern generierter Schlüssel auf dem Bildschirm angezeigt.

    ![Bilden Sie 1 nach dem Speichern](~/ef6/media/form1aftersave.png)

-   Wenn Sie Code First verwendet, es wird auch angezeigt, eine **WinFormswithEFSample.ProductContext** Datenbank wird für Sie erstellt.

    ![Server-Objekt-Explorer](~/ef6/media/serverobjexplorer.png)
