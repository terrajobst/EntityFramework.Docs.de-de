---
title: Datenbindung mit WinForms-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 80fc5062-2f1c-4dbd-ab6e-b99496784b36
ms.openlocfilehash: ad55ef4d496bbfe30eafcab9811c92989066519f
ms.sourcegitcommit: c9c3e00c2d445b784423469838adc071a946e7c9
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/18/2019
ms.locfileid: "68306562"
---
# <a name="databinding-with-winforms"></a><span data-ttu-id="9de13-102">Datenbindung mit WinForms</span><span class="sxs-lookup"><span data-stu-id="9de13-102">Databinding with WinForms</span></span>
<span data-ttu-id="9de13-103">In dieser schrittweisen exemplarischen Vorgehensweise wird veranschaulicht, wie poco-Typen an Windows Forms-Steuerelemente (WinForms) in einem Master-Detail-Formular gebunden werden.</span><span class="sxs-lookup"><span data-stu-id="9de13-103">This step-by-step walkthrough shows how to bind POCO types to Window Forms (WinForms) controls in a “master-detail" form.</span></span> <span data-ttu-id="9de13-104">Die Anwendung verwendet Entity Framework zum Auffüllen von Objekten mit Daten aus der Datenbank, zum Nachverfolgen von Änderungen und zum persistenten Speichern von Daten in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="9de13-104">The application uses Entity Framework to populate objects with data from the database, track changes, and persist data to the database.</span></span>

<span data-ttu-id="9de13-105">Das Modell definiert zwei Typen, die an einer 1: n-Beziehung beteiligt sind: Kategorie (Prinzipal\\Master) und Produkt (\\abhängige Details).</span><span class="sxs-lookup"><span data-stu-id="9de13-105">The model defines two types that participate in one-to-many relationship: Category (principal\\master) and Product (dependent\\detail).</span></span> <span data-ttu-id="9de13-106">Anschließend werden die Visual Studio-Tools verwendet, um die im Modell definierten Typen an die WinForms-Steuerelemente zu binden.</span><span class="sxs-lookup"><span data-stu-id="9de13-106">Then, the Visual Studio tools are used to bind the types defined in the model to the WinForms controls.</span></span> <span data-ttu-id="9de13-107">Das Daten Bindungs Framework von WinForms ermöglicht die Navigation zwischen verknüpften Objekten: Wenn Sie Zeilen in der Master Ansicht auswählen, wird die Detailansicht mit den entsprechenden untergeordneten Daten aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="9de13-107">The WinForms data-binding framework enables navigation between related objects: selecting rows in the master view causes the detail view to update with the corresponding child data.</span></span>

<span data-ttu-id="9de13-108">Die Screenshots und Code Auflistungen in dieser exemplarischen Vorgehensweise werden aus Visual Studio 2013 erstellt, aber Sie können diese exemplarische Vorgehensweise mit Visual Studio 2012 oder Visual Studio 2010 durcharbeiten.</span><span class="sxs-lookup"><span data-stu-id="9de13-108">The screen shots and code listings in this walkthrough are taken from Visual Studio 2013 but you can complete this walkthrough with Visual Studio 2012 or Visual Studio 2010.</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="9de13-109">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="9de13-109">Pre-Requisites</span></span>

<span data-ttu-id="9de13-110">Zum Durchführen dieser exemplarischen Vorgehensweise muss Visual Studio 2013, Visual Studio 2012 oder Visual Studio 2010 installiert sein.</span><span class="sxs-lookup"><span data-stu-id="9de13-110">You need to have Visual Studio 2013, Visual Studio 2012 or Visual Studio 2010 installed to complete this walkthrough.</span></span>

<span data-ttu-id="9de13-111">Wenn Sie Visual Studio 2010 verwenden, müssen Sie auch nuget installieren.</span><span class="sxs-lookup"><span data-stu-id="9de13-111">If you are using Visual Studio 2010, you also have to install NuGet.</span></span> <span data-ttu-id="9de13-112">Weitere Informationen finden Sie unter [Installieren von nuget](http://docs.nuget.org/docs/start-here/installing-nuget).</span><span class="sxs-lookup"><span data-stu-id="9de13-112">For more information, see [Installing NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).</span></span>

## <a name="create-the-application"></a><span data-ttu-id="9de13-113">Erstellen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="9de13-113">Create the Application</span></span>

-   <span data-ttu-id="9de13-114">Öffnen Sie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9de13-114">Open Visual Studio</span></span>
-   <span data-ttu-id="9de13-115">**Datei-&gt; New-&gt; Project...**</span><span class="sxs-lookup"><span data-stu-id="9de13-115">**File -&gt; New -&gt; Project….**</span></span>
-   <span data-ttu-id="9de13-116">Wählen Sie im linken Bereich **Fenster** und im rechten Bereich **Windows formsapplication** aus.</span><span class="sxs-lookup"><span data-stu-id="9de13-116">Select **Windows** in the left pane and **Windows FormsApplication** in the right pane</span></span>
-   <span data-ttu-id="9de13-117">Geben Sie **winformswitef Sample** als Name ein.</span><span class="sxs-lookup"><span data-stu-id="9de13-117">Enter **WinFormswithEFSample** as the name</span></span>
-   <span data-ttu-id="9de13-118">Wählen Sie **OK** aus.</span><span class="sxs-lookup"><span data-stu-id="9de13-118">Select **OK**</span></span>

## <a name="install-the-entity-framework-nuget-package"></a><span data-ttu-id="9de13-119">Installieren des Entity Framework nuget-Pakets</span><span class="sxs-lookup"><span data-stu-id="9de13-119">Install the Entity Framework NuGet package</span></span>

-   <span data-ttu-id="9de13-120">Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf das **winformswideratsample** -Projekt.</span><span class="sxs-lookup"><span data-stu-id="9de13-120">In Solution Explorer, right-click on the **WinFormswithEFSample** project</span></span>
-   <span data-ttu-id="9de13-121">Wählen Sie **nuget-Pakete verwalten... aus.**</span><span class="sxs-lookup"><span data-stu-id="9de13-121">Select **Manage NuGet Packages…**</span></span>
-   <span data-ttu-id="9de13-122">Wählen Sie im Dialogfeld nuget-Pakete verwalten die Registerkarte **Online** aus, und wählen Sie das Paket **EntityFramework** aus.</span><span class="sxs-lookup"><span data-stu-id="9de13-122">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package</span></span>
-   <span data-ttu-id="9de13-123">Klicken Sie auf **Installieren**</span><span class="sxs-lookup"><span data-stu-id="9de13-123">Click **Install**</span></span>  
    > [!NOTE]
    > <span data-ttu-id="9de13-124">Zusätzlich zur EntityFramework-Assembly wird auch ein Verweis auf "System. ComponentModel. DataAnnotations" hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="9de13-124">In addition to the EntityFramework assembly a reference to System.ComponentModel.DataAnnotations is also added.</span></span> <span data-ttu-id="9de13-125">Wenn das Projekt einen Verweis auf System. Data. Entity enthält, wird es entfernt, wenn das EntityFramework-Paket installiert wird.</span><span class="sxs-lookup"><span data-stu-id="9de13-125">If the project has a reference to System.Data.Entity, then it will be removed when the EntityFramework package is installed.</span></span> <span data-ttu-id="9de13-126">Die System. Data. Entity-Assembly wird für Entity Framework 6-Anwendungen nicht mehr verwendet.</span><span class="sxs-lookup"><span data-stu-id="9de13-126">The System.Data.Entity assembly is no longer used for Entity Framework 6 applications.</span></span>

## <a name="implementing-ilistsource-for-collections"></a><span data-ttu-id="9de13-127">Implementieren von IListSource für Sammlungen</span><span class="sxs-lookup"><span data-stu-id="9de13-127">Implementing IListSource for Collections</span></span>

<span data-ttu-id="9de13-128">Sammlungs Eigenschaften müssen die IListSource-Schnittstelle implementieren, um bei Verwendung von Windows Forms die bidirektionale Datenbindung mit Sortierung zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="9de13-128">Collection properties must implement the IListSource interface to enable two-way data binding with sorting when using Windows Forms.</span></span> <span data-ttu-id="9de13-129">Zu diesem Zweck wird ObservableCollection erweitert, um die IListSource-Funktionalität hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="9de13-129">To do this we are going to extend ObservableCollection to add IListSource functionality.</span></span>

-   <span data-ttu-id="9de13-130">Fügen Sie dem Projekt eine **observablelistsource** -Klasse hinzu:</span><span class="sxs-lookup"><span data-stu-id="9de13-130">Add an **ObservableListSource** class to the project:</span></span>
    -   <span data-ttu-id="9de13-131">Klicken Sie mit der rechten Maustaste auf den Projektnamen.</span><span class="sxs-lookup"><span data-stu-id="9de13-131">Right-click on the project name</span></span>
    -   <span data-ttu-id="9de13-132">Wählen Sie **Add&gt; -New Item** aus.</span><span class="sxs-lookup"><span data-stu-id="9de13-132">Select **Add -&gt; New Item**</span></span>
    -   <span data-ttu-id="9de13-133">Wählen Sie **Klasse** aus, und geben Sie **observablelistsource** als Klassennamen ein.</span><span class="sxs-lookup"><span data-stu-id="9de13-133">Select **Class** and enter **ObservableListSource** for the class name</span></span>
-   <span data-ttu-id="9de13-134">Ersetzen Sie den standardmäßig generierten Code durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="9de13-134">Replace the code generated by default with the following code:</span></span>

<span data-ttu-id="9de13-135">*Diese Klasse ermöglicht die bidirektionale Datenbindung und Sortierung. Die-Klasse wird von&lt;ObservableCollection&gt; T abgeleitet und fügt eine explizite Implementierung von IListSource hinzu. Die GetList ()-Methode von IListSource wird implementiert, um eine IBindingList-Implementierung zurückzugeben, die mit der ObservableCollection synchron bleibt. Die von "debindinglist" generierte IBindingList-Implementierung unterstützt die Sortierung. Die "tybindinglist"-Erweiterungsmethode wird in der EntityFramework-Assembly definiert.*</span><span class="sxs-lookup"><span data-stu-id="9de13-135">*This class enables two-way data binding as well as sorting. The class derives from ObservableCollection&lt;T&gt; and adds an explicit implementation of IListSource. The GetList() method of IListSource is implemented to return an IBindingList implementation that stays in sync with the ObservableCollection. The IBindingList implementation generated by ToBindingList supports sorting. The ToBindingList extension method is defined in the EntityFramework assembly.*</span></span>

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

## <a name="define-a-model"></a><span data-ttu-id="9de13-136">Definieren eines Modells</span><span class="sxs-lookup"><span data-stu-id="9de13-136">Define a Model</span></span>

<span data-ttu-id="9de13-137">In dieser exemplarischen Vorgehensweise können Sie ein Modell mithilfe von Code First oder dem EF-Designer implementieren.</span><span class="sxs-lookup"><span data-stu-id="9de13-137">In this walkthrough you can chose to implement a model using Code First or the EF Designer.</span></span> <span data-ttu-id="9de13-138">Führen Sie einen der beiden folgenden Abschnitte aus.</span><span class="sxs-lookup"><span data-stu-id="9de13-138">Complete one of the two following sections.</span></span>

### <a name="option-1-define-a-model-using-code-first"></a><span data-ttu-id="9de13-139">Option 1: Definieren eines Modells mit Code First</span><span class="sxs-lookup"><span data-stu-id="9de13-139">Option 1: Define a Model using Code First</span></span>

<span data-ttu-id="9de13-140">In diesem Abschnitt wird gezeigt, wie ein Modell und die zugehörige Datenbank mithilfe Code First erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="9de13-140">This section shows how to create a model and its associated database using Code First.</span></span> <span data-ttu-id="9de13-141">Fahren Sie mit dem nächsten Abschnitt**fort (Option 2: Definieren eines Modells mit Database First)** Wenn Sie stattdessen Database First verwenden möchten, um das Modell aus einer Datenbank mit dem EF-Designer umzukehren</span><span class="sxs-lookup"><span data-stu-id="9de13-141">Skip to the next section (**Option 2: Define a model using Database First)** if you would rather use Database First to reverse engineer your model from a database using the EF designer</span></span>

<span data-ttu-id="9de13-142">Wenn Sie Code First Entwicklung verwenden, schreiben Sie in der Regel .NET Framework Klassen, die ihr konzeptionelles (Domänen-) Modell definieren.</span><span class="sxs-lookup"><span data-stu-id="9de13-142">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span>

-   <span data-ttu-id="9de13-143">Neue **Produkt** Klasse zu Projekt hinzufügen</span><span class="sxs-lookup"><span data-stu-id="9de13-143">Add a new **Product** class to project</span></span>
-   <span data-ttu-id="9de13-144">Ersetzen Sie den standardmäßig generierten Code durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="9de13-144">Replace the code generated by default with the following code:</span></span>

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

-   <span data-ttu-id="9de13-145">Fügen Sie dem Projekt eine **Category** -Klasse hinzu.</span><span class="sxs-lookup"><span data-stu-id="9de13-145">Add a **Category** class to the project.</span></span>
-   <span data-ttu-id="9de13-146">Ersetzen Sie den standardmäßig generierten Code durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="9de13-146">Replace the code generated by default with the following code:</span></span>

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

<span data-ttu-id="9de13-147">Zusätzlich zum Definieren von Entitäten müssen Sie eine Klasse definieren, die von **dbcontext** abgeleitet ist und **dbset&lt;TEntity&gt;**  -Eigenschaften verfügbar macht.</span><span class="sxs-lookup"><span data-stu-id="9de13-147">In addition to defining entities, you need to define a class that derives from **DbContext** and exposes **DbSet&lt;TEntity&gt;** properties.</span></span> <span data-ttu-id="9de13-148">Die **dbset** -Eigenschaften lassen den Kontext erkennen, welche Typen Sie in das Modell einschließen möchten.</span><span class="sxs-lookup"><span data-stu-id="9de13-148">The **DbSet** properties let the context know which types you want to include in the model.</span></span> <span data-ttu-id="9de13-149">Die Typen " **dbcontext** " und " **dbset** " werden in der EntityFramework-Assembly definiert.</span><span class="sxs-lookup"><span data-stu-id="9de13-149">The **DbContext** and **DbSet** types are defined in the EntityFramework assembly.</span></span>

<span data-ttu-id="9de13-150">Eine Instanz des abgeleiteten dbcontext-Typs verwaltet die Entitäts Objekte zur Laufzeit. dazu gehören das Auffüllen von Objekten mit Daten aus einer Datenbank, die Änderungs Nachverfolgung und das Beibehalten von Daten in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="9de13-150">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

-   <span data-ttu-id="9de13-151">Fügen Sie dem Projekt eine neue **productcontext** -Klasse hinzu.</span><span class="sxs-lookup"><span data-stu-id="9de13-151">Add a new **ProductContext** class to the project.</span></span>
-   <span data-ttu-id="9de13-152">Ersetzen Sie den standardmäßig generierten Code durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="9de13-152">Replace the code generated by default with the following code:</span></span>

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

<span data-ttu-id="9de13-153">Kompilieren Sie das Projekt.</span><span class="sxs-lookup"><span data-stu-id="9de13-153">Compile the project.</span></span>

### <a name="option-2-define-a-model-using-database-first"></a><span data-ttu-id="9de13-154">Option 2: Definieren eines Modells mit Database First</span><span class="sxs-lookup"><span data-stu-id="9de13-154">Option 2: Define a model using Database First</span></span>

<span data-ttu-id="9de13-155">In diesem Abschnitt wird gezeigt, wie Sie mit Database First das Modell aus einer Datenbank mit dem EF-Designer rückgängig machen.</span><span class="sxs-lookup"><span data-stu-id="9de13-155">This section shows how to use Database First to reverse engineer your model from a database using the EF designer.</span></span> <span data-ttu-id="9de13-156">Wenn Sie den vorherigen Abschnitt abgeschlossen haben **(Option 1: Definieren Sie ein Modell mit Code First**), überspringen Sie diesen Abschnitt, und wechseln Sie direkt zum Abschnitt **Lazy Load** .</span><span class="sxs-lookup"><span data-stu-id="9de13-156">If you completed the previous section (**Option 1: Define a model using Code First)**, then skip this section and go straight to the **Lazy Loading** section.</span></span>

#### <a name="create-an-existing-database"></a><span data-ttu-id="9de13-157">Erstellen einer vorhandenen Datenbank</span><span class="sxs-lookup"><span data-stu-id="9de13-157">Create an Existing Database</span></span>

<span data-ttu-id="9de13-158">Wenn Sie eine vorhandene Datenbank als Ziel haben, wird Sie in der Regel bereits erstellt, aber in dieser exemplarischen Vorgehensweise müssen wir eine Datenbank erstellen, auf die zugegriffen werden kann.</span><span class="sxs-lookup"><span data-stu-id="9de13-158">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="9de13-159">Der Datenbankserver, der mit Visual Studio installiert wird, unterscheidet sich abhängig von der installierten Version von Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="9de13-159">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="9de13-160">Wenn Sie Visual Studio 2010 verwenden, erstellen Sie eine SQL Express-Datenbank.</span><span class="sxs-lookup"><span data-stu-id="9de13-160">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="9de13-161">Wenn Sie Visual Studio 2012 verwenden, erstellen Sie eine [localdb](https://msdn.microsoft.com/library/hh510202.aspx) -Datenbank.</span><span class="sxs-lookup"><span data-stu-id="9de13-161">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) database.</span></span>

<span data-ttu-id="9de13-162">Nun generieren wir die Datenbank.</span><span class="sxs-lookup"><span data-stu-id="9de13-162">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="9de13-163">**Ansicht-&gt; Server-Explorer**</span><span class="sxs-lookup"><span data-stu-id="9de13-163">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="9de13-164">Klicken Sie mit der rechten Maustaste auf **Datenverbindungen-&gt; Verbindung hinzufügen...**</span><span class="sxs-lookup"><span data-stu-id="9de13-164">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="9de13-165">Wenn Sie über Server-Explorer keine Verbindung mit einer Datenbank hergestellt haben, müssen Sie Microsoft SQL Server als Datenquelle auswählen.</span><span class="sxs-lookup"><span data-stu-id="9de13-165">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![Datenquelle ändern](~/ef6/media/changedatasource.png)

-   <span data-ttu-id="9de13-167">Stellen Sie abhängig von der installierten Verbindung entweder localdb oder SQL Express her, und geben Sie **Produkte** als Datenbanknamen ein.</span><span class="sxs-lookup"><span data-stu-id="9de13-167">Connect to either LocalDB or SQL Express, depending on which one you have installed, and enter **Products** as the database name</span></span>

    ![Verbindung localdb hinzufügen](~/ef6/media/addconnectionlocaldb.png)

    ![Verbindung hinzufügen Express](~/ef6/media/addconnectionexpress.png)

-   <span data-ttu-id="9de13-170">Wählen Sie **OK** aus, und Sie werden gefragt, ob Sie eine neue Datenbank erstellen möchten, und wählen Sie **Ja** aus.</span><span class="sxs-lookup"><span data-stu-id="9de13-170">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![Datenbank erstellen](~/ef6/media/createdatabase.png)

-   <span data-ttu-id="9de13-172">Die neue Datenbank wird nun in Server-Explorer angezeigt, klicken Sie mit der rechten Maustaste darauf, und wählen Sie **neue Abfrage** aus.</span><span class="sxs-lookup"><span data-stu-id="9de13-172">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="9de13-173">Kopieren Sie den folgenden SQL-Befehl in die neue Abfrage, klicken Sie mit der rechten Maustaste auf die Abfrage, und wählen Sie **Ausführen** .</span><span class="sxs-lookup"><span data-stu-id="9de13-173">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

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

#### <a name="reverse-engineer-model"></a><span data-ttu-id="9de13-174">Reverse Engineering-Modell</span><span class="sxs-lookup"><span data-stu-id="9de13-174">Reverse Engineer Model</span></span>

<span data-ttu-id="9de13-175">Wir verwenden Entity Framework Designer, die als Teil von Visual Studio enthalten ist, um das Modell zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="9de13-175">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="9de13-176">**Project-&gt; neues Element hinzufügen...**</span><span class="sxs-lookup"><span data-stu-id="9de13-176">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="9de13-177">Wählen Sie im linken Menü **Daten** aus, und klicken Sie dann auf **ADO.NET Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="9de13-177">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="9de13-178">Geben Sie **ProductModel** als Name ein, und klicken Sie auf **OK** .</span><span class="sxs-lookup"><span data-stu-id="9de13-178">Enter **ProductModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="9de13-179">Dadurch wird der **Entity Data Model-Assistent** gestartet.</span><span class="sxs-lookup"><span data-stu-id="9de13-179">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="9de13-180">Wählen Sie **aus Datenbank generieren aus** , und klicken Sie auf **weiter**</span><span class="sxs-lookup"><span data-stu-id="9de13-180">Select **Generate from Database** and click **Next**</span></span>

    ![Auswahl modelcontent](~/ef6/media/choosemodelcontents.png)

-   <span data-ttu-id="9de13-182">Wählen Sie die Verbindung mit der Datenbank, die Sie im ersten Abschnitt erstellt haben, geben Sie **productcontext** als Namen der Verbindungs Zeichenfolge ein, und klicken Sie auf **weiter** .</span><span class="sxs-lookup"><span data-stu-id="9de13-182">Select the connection to the database you created in the first section, enter **ProductContext** as the name of the connection string and click **Next**</span></span>

    ![Verbindung auswählen](~/ef6/media/chooseyourconnection.png)

-   <span data-ttu-id="9de13-184">Aktivieren Sie das Kontrollkästchen neben "Tables", um alle Tabellen zu importieren, und klicken Sie auf "Fertigstellen"</span><span class="sxs-lookup"><span data-stu-id="9de13-184">Click the checkbox next to ‘Tables’ to import all tables and click ‘Finish’</span></span>

    ![Objekte auswählen](~/ef6/media/chooseyourobjects.png)

<span data-ttu-id="9de13-186">Sobald der Reverse-Engineering-Prozess abgeschlossen ist, wird das neue Modell dem Projekt hinzugefügt und geöffnet, damit Sie es im Entity Framework Designer anzeigen können.</span><span class="sxs-lookup"><span data-stu-id="9de13-186">Once the reverse engineer process completes the new model is added to your project and opened up for you to view in the Entity Framework Designer.</span></span> <span data-ttu-id="9de13-187">Eine APP. config-Datei wurde dem Projekt auch mit den Verbindungsdetails für die Datenbank hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="9de13-187">An App.config file has also been added to your project with the connection details for the database.</span></span>

#### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="9de13-188">Weitere Schritte in Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="9de13-188">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="9de13-189">Wenn Sie in Visual Studio 2010 arbeiten, müssen Sie den EF-Designer aktualisieren, damit die EF6-Codegenerierung verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="9de13-189">If you are working in Visual Studio 2010 then you will need to update the EF designer to use EF6 code generation.</span></span>

-   <span data-ttu-id="9de13-190">Klicken Sie mit der rechten Maustaste auf eine leere Stelle des Modells im EF-Designer, und wählen Sie **Code Generierungs Element hinzufügen... aus.**</span><span class="sxs-lookup"><span data-stu-id="9de13-190">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="9de13-191">Wählen Sie im linken Menü **Online Vorlagen** aus, und suchen Sie nach **dbcontext** .</span><span class="sxs-lookup"><span data-stu-id="9de13-191">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="9de13-192">Wählen Sie den **EF 6. x dbcontext Generator für\#C aus,** geben Sie **ProductModel** als Name ein, und klicken Sie auf hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="9de13-192">Select the **EF 6.x DbContext Generator for C\#,** enter **ProductsModel** as the name and click Add</span></span>

#### <a name="updating-code-generation-for-data-binding"></a><span data-ttu-id="9de13-193">Aktualisieren der Codegenerierung für die Datenbindung</span><span class="sxs-lookup"><span data-stu-id="9de13-193">Updating code generation for data binding</span></span>

<span data-ttu-id="9de13-194">EF generiert Code aus dem Modell mithilfe von T4-Vorlagen.</span><span class="sxs-lookup"><span data-stu-id="9de13-194">EF generates code from your model using T4 templates.</span></span> <span data-ttu-id="9de13-195">Die Vorlagen, die in Visual Studio enthalten sind oder aus der Visual Studio Gallery heruntergeladen wurden, sind für die allgemeine Verwendung vorgesehen.</span><span class="sxs-lookup"><span data-stu-id="9de13-195">The templates shipped with Visual Studio or downloaded from the Visual Studio gallery are intended for general purpose use.</span></span> <span data-ttu-id="9de13-196">Dies bedeutet, dass die von diesen Vorlagen generierten Entitäten einfache&lt;ICollection&gt; T-Eigenschaften haben.</span><span class="sxs-lookup"><span data-stu-id="9de13-196">This means that the entities generated from these templates have simple ICollection&lt;T&gt; properties.</span></span> <span data-ttu-id="9de13-197">Bei der Datenbindung ist es jedoch wünschenswert, dass Sammlungs Eigenschaften vorhanden sind, die IListSource implementieren.</span><span class="sxs-lookup"><span data-stu-id="9de13-197">However, when doing data binding it is desirable to have collection properties that implement IListSource.</span></span> <span data-ttu-id="9de13-198">Aus diesem Grund haben wir die oben genannte observablelistsource-Klasse erstellt und nun die Vorlagen so ändern, dass diese Klasse verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="9de13-198">This is why we created the ObservableListSource class above and we are now going to modify the templates to make use of this class.</span></span>

-   <span data-ttu-id="9de13-199">Öffnen Sie die **Projektmappen-Explorer** , und suchen Sie nach der Datei **ProductModel. edmx.**</span><span class="sxs-lookup"><span data-stu-id="9de13-199">Open the **Solution Explorer** and find **ProductModel.edmx** file</span></span>
-   <span data-ttu-id="9de13-200">Suchen Sie die **ProductModel.tt** -Datei, die unter der Datei "ProductModel. edmx" eingefügt wird.</span><span class="sxs-lookup"><span data-stu-id="9de13-200">Find the **ProductModel.tt** file which will be nested under the ProductModel.edmx file</span></span>

    ![Produktmodell Vorlage](~/ef6/media/productmodeltemplate.png)

-   <span data-ttu-id="9de13-202">Doppelklicken Sie auf die Datei ProductModel.tt, um Sie im Visual Studio-Editor zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="9de13-202">Double-click on the ProductModel.tt file to open it in the Visual Studio editor</span></span>
-   <span data-ttu-id="9de13-203">Suchen und ersetzen Sie die beiden Vorkommen von "**ICollection**" durch "**observablelistsource**".</span><span class="sxs-lookup"><span data-stu-id="9de13-203">Find and replace the two occurrences of “**ICollection**” with “**ObservableListSource**”.</span></span> <span data-ttu-id="9de13-204">Diese befinden sich in ungefähr den Zeilen 296 und 484.</span><span class="sxs-lookup"><span data-stu-id="9de13-204">These are located at approximately lines 296 and 484.</span></span>
-   <span data-ttu-id="9de13-205">Suchen und ersetzen Sie das erste Vorkommen von "**HashSet**" durch "**observablelistsource**".</span><span class="sxs-lookup"><span data-stu-id="9de13-205">Find and replace the first occurrence of “**HashSet**” with “**ObservableListSource**”.</span></span> <span data-ttu-id="9de13-206">Dieser Vorgang befindet sich in ungefähr Zeile 50.</span><span class="sxs-lookup"><span data-stu-id="9de13-206">This occurrence is located at approximately line 50.</span></span> <span data-ttu-id="9de13-207">Ersetzen **Sie nicht** das zweite Vorkommen des Hashsets, das später im Code gefunden wurde.</span><span class="sxs-lookup"><span data-stu-id="9de13-207">**Do not** replace the second occurrence of HashSet found later in the code.</span></span>
-   <span data-ttu-id="9de13-208">Speichern Sie die Datei ProductModel.tt.</span><span class="sxs-lookup"><span data-stu-id="9de13-208">Save the ProductModel.tt file.</span></span> <span data-ttu-id="9de13-209">Dies sollte bewirken, dass der Code für Entitäten erneut generiert wird.</span><span class="sxs-lookup"><span data-stu-id="9de13-209">This should cause the code for entities to be regenerated.</span></span> <span data-ttu-id="9de13-210">Wenn der Code nicht automatisch neu generiert wird, klicken Sie mit der rechten Maustaste auf ProductModel.tt, und wählen Sie "benutzerdefiniertes Tool ausführen".</span><span class="sxs-lookup"><span data-stu-id="9de13-210">If the code does not regenerate automatically, then right click on ProductModel.tt and choose “Run Custom Tool”.</span></span>

<span data-ttu-id="9de13-211">Wenn Sie nun die Category.cs-Datei öffnen (die unter ProductModel.tt angezeigt wird), sollten Sie sehen, dass die Products-Sammlung den Typ **&lt;observablelistsource&gt;** aufweist.</span><span class="sxs-lookup"><span data-stu-id="9de13-211">If you now open the Category.cs file (which is nested under ProductModel.tt) then you should see that the Products collection has the type **ObservableListSource&lt;Product&gt;**.</span></span>

<span data-ttu-id="9de13-212">Kompilieren Sie das Projekt.</span><span class="sxs-lookup"><span data-stu-id="9de13-212">Compile the project.</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="9de13-213">Lazy Loading</span><span class="sxs-lookup"><span data-stu-id="9de13-213">Lazy Loading</span></span>

<span data-ttu-id="9de13-214">Die **Products** -Eigenschaft der **Category** -Klasse und der **Category** -Eigenschaft in der **Product** -Klasse sind Navigations Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="9de13-214">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="9de13-215">In Entity Framework bieten Navigations Eigenschaften eine Möglichkeit, eine Beziehung zwischen zwei Entitäts Typen zu navigieren.</span><span class="sxs-lookup"><span data-stu-id="9de13-215">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="9de13-216">EF bietet Ihnen die Möglichkeit, Verwandte Entitäten aus der Datenbank automatisch zu laden, wenn Sie zum ersten Mal auf die Navigations Eigenschaft zugreifen.</span><span class="sxs-lookup"><span data-stu-id="9de13-216">EF gives you an option of loading related entities from the database automatically the first time you access the navigation property.</span></span> <span data-ttu-id="9de13-217">Beachten Sie bei dieser Art von laden (Lazy Loading), dass beim ersten Zugriff auf jede Navigations Eigenschaft eine separate Abfrage für die Datenbank ausgeführt wird, wenn der Inhalt nicht bereits im Kontext vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="9de13-217">With this type of loading (called lazy loading), be aware that the first time you access each navigation property a separate query will be executed against the database if the contents are not already in the context.</span></span>

<span data-ttu-id="9de13-218">Wenn poco-Entitäts Typen verwendet werden, erreicht EF Lazy Loading durch Erstellen von Instanzen abgeleiteter Proxy Typen während der Laufzeit und das anschließende Überschreiben virtueller Eigenschaften in den Klassen, um den Lade Hook hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="9de13-218">When using POCO entity types, EF achieves lazy loading by creating instances of derived proxy types during runtime and then overriding virtual properties in your classes to add the loading hook.</span></span> <span data-ttu-id="9de13-219">Wenn Sie Lazy Loading verknüpfter Objekte abrufen möchten, müssen Sie die Navigations Eigenschaften Getter als **Public** und **Virtual** (**Overridable** in Visual Basic) deklarieren, und die Klasse darf nicht **versiegelt** sein (**nodeverridable** in Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="9de13-219">To get lazy loading of related objects, you must declare navigation property getters as **public** and **virtual** (**Overridable** in Visual Basic), and you class must not be **sealed** (**NotOverridable** in Visual Basic).</span></span> <span data-ttu-id="9de13-220">Bei Verwendung Database First Navigations Eigenschaften automatisch als virtuell fest, um Lazy Loading zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="9de13-220">When using Database First navigation properties are automatically made virtual to enable lazy loading.</span></span> <span data-ttu-id="9de13-221">Im Code First Abschnitt haben wir entschieden, die Navigations Eigenschaften aus demselben Grund virtuell zu machen.</span><span class="sxs-lookup"><span data-stu-id="9de13-221">In the Code First section we chose to make the navigation properties virtual for the same reason</span></span>

## <a name="bind-object-to-controls"></a><span data-ttu-id="9de13-222">Binden von Objekten an Steuerelemente</span><span class="sxs-lookup"><span data-stu-id="9de13-222">Bind Object to Controls</span></span>

<span data-ttu-id="9de13-223">Fügen Sie die Klassen, die im Modell definiert sind, als Datenquellen für diese WinForms-Anwendung hinzu.</span><span class="sxs-lookup"><span data-stu-id="9de13-223">Add the classes that are defined in the model as data sources for this WinForms application.</span></span>

-   <span data-ttu-id="9de13-224">Wählen Sie im Hauptmenü **Projekt-&gt; neue Datenquelle hinzufügen... aus.**</span><span class="sxs-lookup"><span data-stu-id="9de13-224">From the main menu, select **Project -&gt; Add New Data Source …**</span></span>
    <span data-ttu-id="9de13-225">(in Visual Studio 2010 müssen Sie **Daten&gt; -neue Datenquelle hinzufügen...** )</span><span class="sxs-lookup"><span data-stu-id="9de13-225">(in Visual Studio 2010, you need to select **Data -&gt; Add New Data Source…**)</span></span>
-   <span data-ttu-id="9de13-226">Wählen Sie im Fenster Daten Quellentyp auswählen die Option **Objekt** aus, und klicken Sie auf **weiter** .</span><span class="sxs-lookup"><span data-stu-id="9de13-226">In the Choose a Data Source Type window, select **Object** and click **Next**</span></span>
-   <span data-ttu-id="9de13-227">Erweitern Sie im Dialogfeld Wählen Sie die Datenobjekte aus das **winformswiderfsample** -Objekt zweimal, und wählen Sie die **Kategorie** aus. es ist nicht erforderlich, die Produktdaten Quelle auszuwählen, da wir Sie über die-Eigenschaft des Produkts in der Kategorie Datenquelle erhalten.</span><span class="sxs-lookup"><span data-stu-id="9de13-227">In the Select the Data Objects dialog, unfold the **WinFormswithEFSample** two times and select **Category** There is no need to select the Product data source, because we will get to it through the Product’s property on the Category data source.</span></span>

    ![Datenquelle](~/ef6/media/datasource.png)

-   <span data-ttu-id="9de13-229">Klicken Sie auf **Finish.** Wenn das *Fenster Datenquellen nicht angezeigt wird, wählen Sie \* \* \* Ansicht-&gt; weitere Windows-&gt; Datenquellen* aus. 
    \*</span><span class="sxs-lookup"><span data-stu-id="9de13-229">Click **Finish.**
*If the Data Sources window is not showing up, select\*\*\*View -&gt; Other Windows-&gt; Data Sources*\*</span></span>
-   <span data-ttu-id="9de13-230">Klicken Sie auf das anheft Symbol, damit das Fenster Datenquellen nicht automatisch ausgeblendet wird.</span><span class="sxs-lookup"><span data-stu-id="9de13-230">Press the pin icon, so the Data Sources window does not auto hide.</span></span> <span data-ttu-id="9de13-231">Sie müssen möglicherweise auf die Schaltfläche "Aktualisieren" klicken, wenn das Fenster bereits sichtbar war.</span><span class="sxs-lookup"><span data-stu-id="9de13-231">You may need to hit the refresh button if the window was already visible.</span></span>

    ![Datenquelle 2](~/ef6/media/datasource2.png)

-   <span data-ttu-id="9de13-233">Doppelklicken Sie in Projektmappen-Explorer auf die Datei **Form1.cs** , um das Hauptformular im-Designer zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="9de13-233">In Solution Explorer, double-click the **Form1.cs** file to open the main form in designer.</span></span>
-   <span data-ttu-id="9de13-234">Wählen Sie die **kategoriedatenquelle** aus, und ziehen Sie Sie auf das Formular.</span><span class="sxs-lookup"><span data-stu-id="9de13-234">Select the **Category** data source and drag it on the form.</span></span> <span data-ttu-id="9de13-235">Standardmäßig werden dem Designer neue DataGridView-Steuerelemente (**categorydatagridview**) und Navigations Symbolleisten-Steuerelemente hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="9de13-235">By default, a new DataGridView (**categoryDataGridView**) and Navigation toolbar controls are added to the designer.</span></span> <span data-ttu-id="9de13-236">Diese Steuerelemente sind an die Komponenten BindingSource (**categorybindingsource**) und Binding Navigator (**categorybindingnavigator**) gebunden, die ebenfalls erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="9de13-236">These controls are bound to the BindingSource (**categoryBindingSource**) and Binding Navigator (**categoryBindingNavigator**) components that are created as well.</span></span>
-   <span data-ttu-id="9de13-237">Bearbeiten Sie die Spalten in der **categorydatagridview**.</span><span class="sxs-lookup"><span data-stu-id="9de13-237">Edit the columns on the **categoryDataGridView**.</span></span> <span data-ttu-id="9de13-238">Die **CategoryID** -Spalte soll als schreibgeschützt festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="9de13-238">We want to set the **CategoryId** column to read-only.</span></span> <span data-ttu-id="9de13-239">Der Wert für die **CategoryID** -Eigenschaft wird von der Datenbank generiert, nachdem die Daten gespeichert wurden.</span><span class="sxs-lookup"><span data-stu-id="9de13-239">The value for the **CategoryId** property is generated by the database after we save the data.</span></span>
    -   <span data-ttu-id="9de13-240">Klicken Sie mit der rechten Maustaste auf das DataGridView-Steuerelement, und wählen Sie Spalten bearbeiten aus.</span><span class="sxs-lookup"><span data-stu-id="9de13-240">Right-click the DataGridView control and select Edit Columns…</span></span>
    -   <span data-ttu-id="9de13-241">Wählen Sie die Spalte CategoryID aus, und legen Sie schreibgeschützt auf true fest.</span><span class="sxs-lookup"><span data-stu-id="9de13-241">Select the CategoryId column and set ReadOnly to True</span></span>
    -   <span data-ttu-id="9de13-242">OK drücken</span><span class="sxs-lookup"><span data-stu-id="9de13-242">Press OK</span></span>
-   <span data-ttu-id="9de13-243">Wählen Sie unter der Kategorie Datenquelle die Option Produkte aus, und ziehen Sie Sie auf das Formular.</span><span class="sxs-lookup"><span data-stu-id="9de13-243">Select Products from under the Category data source and drag it on the form.</span></span> <span data-ttu-id="9de13-244">Productdatagridview und ProductBindingSource werden dem Formular hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="9de13-244">The productDataGridView and productBindingSource are added to the form.</span></span>
-   <span data-ttu-id="9de13-245">Bearbeiten Sie die Spalten in productdatagridview.</span><span class="sxs-lookup"><span data-stu-id="9de13-245">Edit the columns on the productDataGridView.</span></span> <span data-ttu-id="9de13-246">Wir möchten die Spalten "CategoryID" und "Category" ausblenden und "ProductID" auf "schreibgeschützt" festlegen.</span><span class="sxs-lookup"><span data-stu-id="9de13-246">We want to hide the CategoryId and Category columns and set ProductId to read-only.</span></span> <span data-ttu-id="9de13-247">Der Wert für die ProductID-Eigenschaft wird von der Datenbank generiert, nachdem die Daten gespeichert wurden.</span><span class="sxs-lookup"><span data-stu-id="9de13-247">The value for the ProductId property is generated by the database after we save the data.</span></span>
    -   <span data-ttu-id="9de13-248">Klicken Sie mit der rechten Maustaste auf das DataGridView-Steuerelement, und wählen Sie **Spalten bearbeiten**aus.</span><span class="sxs-lookup"><span data-stu-id="9de13-248">Right-click the DataGridView control and select **Edit Columns...**.</span></span>
    -   <span data-ttu-id="9de13-249">Wählen Sie die Spalte **ProductID** aus **, und** legen Sie schreibgeschützt auf **true**fest.</span><span class="sxs-lookup"><span data-stu-id="9de13-249">Select the **ProductId** column and set **ReadOnly** to **True**.</span></span>
    -   <span data-ttu-id="9de13-250">Wählen Sie die Spalte **CategoryID** aus, und klicken Sie auf die Schaltfläche **Entfernen** .</span><span class="sxs-lookup"><span data-stu-id="9de13-250">Select the **CategoryId** column and press the **Remove** button.</span></span> <span data-ttu-id="9de13-251">Gehen Sie mit der Spalte **Category** gleich vor.</span><span class="sxs-lookup"><span data-stu-id="9de13-251">Do the same with the **Category** column.</span></span>
    -   <span data-ttu-id="9de13-252">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="9de13-252">Press **OK**.</span></span>

    <span data-ttu-id="9de13-253">Bisher haben wir unsere DataGridView-Steuerelemente mit BindingSource-Komponenten im Designer verknüpft.</span><span class="sxs-lookup"><span data-stu-id="9de13-253">So far, we associated our DataGridView controls with BindingSource components in the designer.</span></span> <span data-ttu-id="9de13-254">Im nächsten Abschnitt wird dem Code Behind Code hinzugefügt, um categorybindingsource. DataSource auf die Auflistung von Entitäten festzulegen, die derzeit von dbcontext nachverfolgt werden.</span><span class="sxs-lookup"><span data-stu-id="9de13-254">In the next section we will add code to the code behind to set categoryBindingSource.DataSource to the collection of entities that are currently tracked by DbContext.</span></span> <span data-ttu-id="9de13-255">Wenn wir die Produkte aus der Kategorie gezogen und abgelegt haben, haben die WinForms das Einrichten der ProductBindingSource. DataSource-Eigenschaft in der categorybindingsource-Eigenschaft und der ProductBindingSource. DataMember-Eigenschaft in Produkte erledigt.</span><span class="sxs-lookup"><span data-stu-id="9de13-255">When we dragged-and-dropped Products from under the Category, the WinForms took care of setting up the productsBindingSource.DataSource property to categoryBindingSource and productsBindingSource.DataMember property to Products.</span></span> <span data-ttu-id="9de13-256">Aufgrund dieser Bindung werden nur die Produkte, die der aktuell ausgewählten Kategorie angehören, in der productdatagridview angezeigt.</span><span class="sxs-lookup"><span data-stu-id="9de13-256">Because of this binding, only the products that belong to the currently selected Category will be displayed in the productDataGridView.</span></span>
-   <span data-ttu-id="9de13-257">Aktivieren Sie die Schaltfläche **Speichern** auf der Navigations Symbolleiste, indem Sie auf die Rechte Maustaste klicken und **aktivieren**auswählen.</span><span class="sxs-lookup"><span data-stu-id="9de13-257">Enable the **Save** button on the Navigation toolbar by clicking the right mouse button and selecting **Enabled**.</span></span>

    ![Designer von Formular 1](~/ef6/media/form1-designer.png)

-   <span data-ttu-id="9de13-259">Fügen Sie den Ereignishandler für die Schaltfläche Speichern hinzu, indem Sie auf die Schaltfläche doppelklicken.</span><span class="sxs-lookup"><span data-stu-id="9de13-259">Add the event handler for the save button by double-clicking on the button.</span></span> <span data-ttu-id="9de13-260">Dadurch wird der Ereignishandler hinzugefügt, und Sie gelangen zum Code Behind für das Formular.</span><span class="sxs-lookup"><span data-stu-id="9de13-260">This will add the event handler and bring you to the code behind for the form.</span></span> <span data-ttu-id="9de13-261">Der Code für den " **categorybindingnavigatorsaveitem\_** "-Click-Ereignishandler wird im nächsten Abschnitt hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="9de13-261">The code for the **categoryBindingNavigatorSaveItem\_Click** event handler will be added in the next section.</span></span>

## <a name="add-the-code-that-handles-data-interaction"></a><span data-ttu-id="9de13-262">Fügen Sie den Code hinzu, der die Daten Interaktion behandelt.</span><span class="sxs-lookup"><span data-stu-id="9de13-262">Add the Code that Handles Data Interaction</span></span>

<span data-ttu-id="9de13-263">Nun fügen wir den Code hinzu, um den productcontext zum Ausführen von Datenzugriff zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="9de13-263">We'll now add the code to use the ProductContext to perform data access.</span></span> <span data-ttu-id="9de13-264">Aktualisieren Sie den Code für das Hauptformular Fenster, wie unten gezeigt.</span><span class="sxs-lookup"><span data-stu-id="9de13-264">Update the code for the main form window as shown below.</span></span>

<span data-ttu-id="9de13-265">Der Code deklariert eine Instanz von productcontext mit langer Laufzeit.</span><span class="sxs-lookup"><span data-stu-id="9de13-265">The code declares a long-running instance of ProductContext.</span></span> <span data-ttu-id="9de13-266">Das productcontext-Objekt wird verwendet, um Daten abzufragen und in der Datenbank zu speichern.</span><span class="sxs-lookup"><span data-stu-id="9de13-266">The ProductContext object is used to query and save data to the database.</span></span> <span data-ttu-id="9de13-267">Die verwerfen ()-Methode der productcontext-Instanz wird dann von der überschriebenen OnClosing-Methode aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="9de13-267">The Dispose() method on the ProductContext instance is then called from the overridden OnClosing method.</span></span> <span data-ttu-id="9de13-268">Die Code Kommentare enthalten Details zum Funktionsumfang des Codes.</span><span class="sxs-lookup"><span data-stu-id="9de13-268">The code comments provide details about what the code does.</span></span>

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

## <a name="test-the-windows-forms-application"></a><span data-ttu-id="9de13-269">Testen der Windows Forms Anwendung</span><span class="sxs-lookup"><span data-stu-id="9de13-269">Test the Windows Forms Application</span></span>

-   <span data-ttu-id="9de13-270">Kompilieren Sie die Anwendung, und führen Sie Sie aus, und Sie können die Funktionalität testen.</span><span class="sxs-lookup"><span data-stu-id="9de13-270">Compile and run the application and you can test out the functionality.</span></span>

    ![Formular 1 vor dem Speichern](~/ef6/media/form1beforesave.png)

-   <span data-ttu-id="9de13-272">Nach dem Speichern der Speicher generierten Schlüssel werden auf dem Bildschirm angezeigt.</span><span class="sxs-lookup"><span data-stu-id="9de13-272">After saving the store generated keys are shown on the screen.</span></span>

    ![Formular 1 nach dem Speichern](~/ef6/media/form1aftersave.png)

-   <span data-ttu-id="9de13-274">Wenn Sie Code First verwendet haben, sehen Sie auch, dass eine **winformswitef Sample. productcontext** -Datenbank für Sie erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="9de13-274">If you used Code First, then you will also see that a **WinFormswithEFSample.ProductContext** database is created for you.</span></span>

    ![Server Objekt-Explorer](~/ef6/media/serverobjexplorer.png)
