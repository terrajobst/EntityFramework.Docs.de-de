---
title: Datenbindung mit WPF – EF6
author: divega
ms.date: 2016-10-23
ms.assetid: e90d48e6-bea5-47ef-b756-7b89cce4daf0
ms.openlocfilehash: 0b1f4d5ea204cd80acf42caa499732610daa0e31
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994822"
---
# <a name="databinding-with-wpf"></a><span data-ttu-id="c1467-102">Datenbindung mit WPF</span><span class="sxs-lookup"><span data-stu-id="c1467-102">Databinding with WPF</span></span>
<span data-ttu-id="c1467-103">Schritt für Schritt dieser exemplarischen Vorgehensweise zeigt, wie POCO-Typen an WPF-Steuerelemente in einem "Master / Detail-Formular gebunden wird.</span><span class="sxs-lookup"><span data-stu-id="c1467-103">This step-by-step walkthrough shows how to bind POCO types to WPF controls in a “master-detail" form.</span></span> <span data-ttu-id="c1467-104">Die Anwendung verwendet Entity Framework-APIs, um Objekte mit Daten aus der Datenbank zu füllen, Nachverfolgen von Änderungen und Speichern von Daten in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="c1467-104">The application uses the Entity Framework APIs to populate objects with data from the database, track changes, and persist data to the database.</span></span>

<span data-ttu-id="c1467-105">Das Modell definiert zwei Typen, die Teilnahme an 1: n Beziehung: **Kategorie** (principal\\master) und **Produkt** (abhängige\\Detail).</span><span class="sxs-lookup"><span data-stu-id="c1467-105">The model defines two types that participate in one-to-many relationship: **Category** (principal\\master) and **Product** (dependent\\detail).</span></span> <span data-ttu-id="c1467-106">Anschließend werden Visual Studio-Tools verwendet, um in das Modell, das WPF-Steuerelemente definierten Typen zu binden.</span><span class="sxs-lookup"><span data-stu-id="c1467-106">Then, the Visual Studio tools are used to bind the types defined in the model to the WPF controls.</span></span> <span data-ttu-id="c1467-107">Das WPF-Datenbindung-Framework ermöglicht die Navigation zwischen verknüpften Objekten: Auswählen von Zeilen in der die Masteransicht bewirkt, dass die Detailansicht, um mit den entsprechenden untergeordneten Daten zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="c1467-107">The WPF data-binding framework enables navigation between related objects: selecting rows in the master view causes the detail view to update with the corresponding child data.</span></span>

<span data-ttu-id="c1467-108">Die Screenshots und Codebeispiele in dieser exemplarischen Vorgehensweise stammen aus Visual Studio 2013, aber Sie können in dieser exemplarischen Vorgehensweise mit Visual Studio 2012 oder Visual Studio 2010 ausführen.</span><span class="sxs-lookup"><span data-stu-id="c1467-108">The screen shots and code listings in this walkthrough are taken from Visual Studio 2013 but you can complete this walkthrough with Visual Studio 2012 or Visual Studio 2010.</span></span>

## <a name="use-the-object-option-for-creating-wpf-data-sources"></a><span data-ttu-id="c1467-109">Verwenden Sie die Option 'Object' zum Erstellen von WPF-Datenquellen</span><span class="sxs-lookup"><span data-stu-id="c1467-109">Use the 'Object' Option for Creating WPF Data Sources</span></span>

<span data-ttu-id="c1467-110">Mit früheren Version von Entity Framework wir verwendet, um die Verwendung wird empfohlen, die **Datenbank** option erstellen eine neue Datenquelle basierend auf einem Modell mit dem EF Designer erstellt.</span><span class="sxs-lookup"><span data-stu-id="c1467-110">With previous version of Entity Framework we used to recommend using the **Database** option when creating a new Data Source based on a model created with the EF Designer.</span></span> <span data-ttu-id="c1467-111">Dies war, da der Designer generieren würden, ein, die von ObjectContext abgeleiteten Kontext und Entitätsklassen, die von "EntityObject" abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="c1467-111">This was because the designer would generate a context that derived from ObjectContext and entity classes that derived from EntityObject.</span></span> <span data-ttu-id="c1467-112">Mit der Option können Sie den besten Code für die Interaktion mit dieser API-Oberfläche zu schreiben.</span><span class="sxs-lookup"><span data-stu-id="c1467-112">Using the Database option would help you write the best code for interacting with this API surface.</span></span>

<span data-ttu-id="c1467-113">Die EF-Designer für Visual Studio 2012 und Visual Studio 2013 generieren einen Kontext an, der von "DbContext" abgeleitet, sowie einfache POCO-Entitätsklassen wird.</span><span class="sxs-lookup"><span data-stu-id="c1467-113">The EF Designers for Visual Studio 2012 and Visual Studio 2013 generate a context that derives from DbContext together with simple POCO entity classes.</span></span> <span data-ttu-id="c1467-114">Es wird empfohlen, mit Visual Studio 2010 austauschen, um eine Vorlage für die codegenerierung, die "DbContext" verwendet wird, wie weiter unten in dieser exemplarischen Vorgehensweise beschrieben.</span><span class="sxs-lookup"><span data-stu-id="c1467-114">With Visual Studio 2010 we recommend swapping to a code generation template that uses DbContext as described later in this walkthrough.</span></span>

<span data-ttu-id="c1467-115">Bei Verwendung die DbContext-API-Oberfläche verwenden Sie die **Objekt** option beim Erstellen einer neuen Datenquelle, wie in dieser exemplarischen Vorgehensweise gezeigt.</span><span class="sxs-lookup"><span data-stu-id="c1467-115">When using the DbContext API surface you should use the **Object** option when creating a new Data Source, as shown in this walkthrough.</span></span>

<span data-ttu-id="c1467-116">Bei Bedarf können Sie [codegenerierung ObjectContext basierend wiederherstellen](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) für Modelle, die mit dem EF Designer erstellt.</span><span class="sxs-lookup"><span data-stu-id="c1467-116">If needed, you can [revert to ObjectContext based code generation](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) for models created with the EF Designer.</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="c1467-117">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="c1467-117">Pre-Requisites</span></span>

<span data-ttu-id="c1467-118">Sie müssen Visual Studio 2013, Visual Studio 2012 oder Visual Studio 2010 installiert werden, um diese exemplarische Vorgehensweise abgeschlossen haben.</span><span class="sxs-lookup"><span data-stu-id="c1467-118">You need to have Visual Studio 2013, Visual Studio 2012 or Visual Studio 2010 installed to complete this walkthrough.</span></span>

<span data-ttu-id="c1467-119">Wenn Sie Visual Studio 2010 verwenden, müssen Sie auch NuGet zu installieren.</span><span class="sxs-lookup"><span data-stu-id="c1467-119">If you are using Visual Studio 2010, you also have to install NuGet.</span></span> <span data-ttu-id="c1467-120">Weitere Informationen finden Sie unter [Installing NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).</span><span class="sxs-lookup"><span data-stu-id="c1467-120">For more information, see [Installing NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).</span></span>  

## <a name="create-the-application"></a><span data-ttu-id="c1467-121">Erstellen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="c1467-121">Create the Application</span></span>

-   <span data-ttu-id="c1467-122">Öffnen Sie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c1467-122">Open Visual Studio</span></span>
-   <span data-ttu-id="c1467-123">**Datei -&gt; neu –&gt; Projekt...**</span><span class="sxs-lookup"><span data-stu-id="c1467-123">**File -&gt; New -&gt; Project….**</span></span>
-   <span data-ttu-id="c1467-124">Wählen Sie **Windows** im linken Bereich und **WPFApplication** im rechten Bereich</span><span class="sxs-lookup"><span data-stu-id="c1467-124">Select **Windows** in the left pane and **WPFApplication** in the right pane</span></span>
-   <span data-ttu-id="c1467-125">Geben Sie **WPFwithEFSample** als Name</span><span class="sxs-lookup"><span data-stu-id="c1467-125">Enter **WPFwithEFSample** as the name</span></span>
-   <span data-ttu-id="c1467-126">Wählen Sie **OK** aus.</span><span class="sxs-lookup"><span data-stu-id="c1467-126">Select **OK**</span></span>

## <a name="install-the-entity-framework-nuget-package"></a><span data-ttu-id="c1467-127">Installieren Sie das Entity Framework-NuGet-Paket</span><span class="sxs-lookup"><span data-stu-id="c1467-127">Install the Entity Framework NuGet package</span></span>

-   <span data-ttu-id="c1467-128">Im Projektmappen-Explorer mit der Maustaste auf die **WinFormswithEFSample** Projekt</span><span class="sxs-lookup"><span data-stu-id="c1467-128">In Solution Explorer, right-click on the **WinFormswithEFSample** project</span></span>
-   <span data-ttu-id="c1467-129">Wählen Sie **NuGet-Pakete verwalten...**</span><span class="sxs-lookup"><span data-stu-id="c1467-129">Select **Manage NuGet Packages…**</span></span>
-   <span data-ttu-id="c1467-130">Wählen Sie in das Dialogfeld "NuGet-Pakete verwalten" die **Online** Registerkarte, und wählen Sie die **EntityFramework** Paket</span><span class="sxs-lookup"><span data-stu-id="c1467-130">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package</span></span>
-   <span data-ttu-id="c1467-131">Klicken Sie auf **installieren**</span><span class="sxs-lookup"><span data-stu-id="c1467-131">Click **Install**</span></span>  
    >[!NOTE]
    > <span data-ttu-id="c1467-132">Zusätzlich zu den EntityFramework-Assembly wird ein Verweis auf System.ComponentModel.DataAnnotations ebenfalls hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="c1467-132">In addition to the EntityFramework assembly a reference to System.ComponentModel.DataAnnotations is also added.</span></span> <span data-ttu-id="c1467-133">Wenn das Projekt einen Verweis auf System.Data.Entity verfügt, wird dann es entfernt, wenn das EntityFramework-Paket installiert wird.</span><span class="sxs-lookup"><span data-stu-id="c1467-133">If the project has a reference to System.Data.Entity, then it will be removed when the EntityFramework package is installed.</span></span> <span data-ttu-id="c1467-134">Die Assembly System.Data.Entity wird nicht mehr für Entity Framework 6-Anwendungen verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="c1467-134">The System.Data.Entity assembly is no longer used for Entity Framework 6 applications.</span></span>

## <a name="define-a-model"></a><span data-ttu-id="c1467-135">Definieren eines Modells</span><span class="sxs-lookup"><span data-stu-id="c1467-135">Define a Model</span></span>

<span data-ttu-id="c1467-136">In dieser exemplarischen Vorgehensweise können Sie ein Modell mithilfe von Code First oder dem EF Designer implementieren möchten.</span><span class="sxs-lookup"><span data-stu-id="c1467-136">In this walkthrough you can chose to implement a model using Code First or the EF Designer.</span></span> <span data-ttu-id="c1467-137">Führen Sie eine der beiden folgenden Abschnitte.</span><span class="sxs-lookup"><span data-stu-id="c1467-137">Complete one of the two following sections.</span></span>

### <a name="option-1-define-a-model-using-code-first"></a><span data-ttu-id="c1467-138">Option 1: Definieren eines Modells mit Code First</span><span class="sxs-lookup"><span data-stu-id="c1467-138">Option 1: Define a Model using Code First</span></span>

<span data-ttu-id="c1467-139">In diesem Abschnitt zeigt, wie Sie ein Modell und der zugehörigen Datenbank mithilfe von Code First zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="c1467-139">This section shows how to create a model and its associated database using Code First.</span></span> <span data-ttu-id="c1467-140">Fahren Sie mit dem nächsten Abschnitt (**Option 2: Definieren eines Modells mit Database First)** verwenden, wenn Sie stattdessen Database First Reverse Engineering für das Modell aus einer Datenbank mit dem EF-Designer</span><span class="sxs-lookup"><span data-stu-id="c1467-140">Skip to the next section (**Option 2: Define a model using Database First)** if you would rather use Database First to reverse engineer your model from a database using the EF designer</span></span>

<span data-ttu-id="c1467-141">Bei Verwendung von Code First-Entwicklung beginnen Sie in der Regel durch das Schreiben von .NET Framework-Klassen, die das konzeptionelle (Domäne)-Modell zu definieren.</span><span class="sxs-lookup"><span data-stu-id="c1467-141">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span>

-   <span data-ttu-id="c1467-142">Fügen Sie eine neue Klasse, die **WPFwithEFSample:**</span><span class="sxs-lookup"><span data-stu-id="c1467-142">Add a new class to the **WPFwithEFSample:**</span></span>
    -   <span data-ttu-id="c1467-143">Mit der rechten Maustaste auf den Namen des Projekts</span><span class="sxs-lookup"><span data-stu-id="c1467-143">Right-click on the project name</span></span>
    -   <span data-ttu-id="c1467-144">Wählen Sie **hinzufügen**, klicken Sie dann **neues Element**</span><span class="sxs-lookup"><span data-stu-id="c1467-144">Select **Add**, then **New Item**</span></span>
    -   <span data-ttu-id="c1467-145">Wählen Sie **Klasse** , und geben Sie **Produkt** für den Namen der Klasse</span><span class="sxs-lookup"><span data-stu-id="c1467-145">Select **Class** and enter **Product** for the class name</span></span>
-   <span data-ttu-id="c1467-146">Ersetzen Sie die **Produkt** -Klassendefinition durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="c1467-146">Replace the **Product** class definition with the following code:</span></span>

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

<span data-ttu-id="c1467-147">Die **Produkte** Eigenschaft für die **Kategorie** Klasse und **Kategorie** Eigenschaft für die **Produkt** -Klasse sind Navigationseigenschaften.</span><span class="sxs-lookup"><span data-stu-id="c1467-147">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="c1467-148">Entity Framework bieten Ihnen eine Möglichkeit, eine Beziehung zwischen zwei Entitätstypen zu navigieren Navigationseigenschaften.</span><span class="sxs-lookup"><span data-stu-id="c1467-148">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="c1467-149">Zusätzlich zum Definieren von Entitäten, müssen Sie eine Klasse definieren, die von "DbContext" abgeleitet und stellt "DbSet"&lt;TEntity&gt; Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="c1467-149">In addition to defining entities, you need to define a class that derives from DbContext and exposes DbSet&lt;TEntity&gt; properties.</span></span> <span data-ttu-id="c1467-150">"DbSet"&lt;TEntity&gt; Eigenschaften können Sie den Kontext, die wissen, welche Typen im Modell enthalten sein sollen.</span><span class="sxs-lookup"><span data-stu-id="c1467-150">The DbSet&lt;TEntity&gt; properties let the context know which types you want to include in the model.</span></span>

<span data-ttu-id="c1467-151">Eine Instanz des Typs "DbContext" abgeleitet verwaltet die Entitätsobjekte während der Laufzeit, einschließlich ausfüllenden Objekten mit Daten aus einer Datenbank, verfolgen und Speichern von Daten in der Datenbank zu ändern.</span><span class="sxs-lookup"><span data-stu-id="c1467-151">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

-   <span data-ttu-id="c1467-152">Fügen Sie einen neuen **ProductContext** Klasse, um das Projekt mit der folgenden Definition:</span><span class="sxs-lookup"><span data-stu-id="c1467-152">Add a new **ProductContext** class to the project with the following definition:</span></span>

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

<span data-ttu-id="c1467-153">Kompilieren Sie das Projekt.</span><span class="sxs-lookup"><span data-stu-id="c1467-153">Compile the project.</span></span>

### <a name="option-2-define-a-model-using-database-first"></a><span data-ttu-id="c1467-154">Option 2: Definieren eines Modells mit Database First</span><span class="sxs-lookup"><span data-stu-id="c1467-154">Option 2: Define a model using Database First</span></span>

<span data-ttu-id="c1467-155">In diesem Abschnitt zeigt, wie mit Database First zum zurückentwickeln das Modell aus einer Datenbank mit dem EF Designer.</span><span class="sxs-lookup"><span data-stu-id="c1467-155">This section shows how to use Database First to reverse engineer your model from a database using the EF designer.</span></span> <span data-ttu-id="c1467-156">Wenn Sie den vorherigen Abschnitt abgeschlossen (**Option 1: Definieren eines Modells mithilfe von Code First)**, klicken Sie dann diesen Abschnitt überspringen und sofort die **Lazy Loading** Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="c1467-156">If you completed the previous section (**Option 1: Define a model using Code First)**, then skip this section and go straight to the **Lazy Loading** section.</span></span>

#### <a name="create-an-existing-database"></a><span data-ttu-id="c1467-157">Erstellen Sie eine vorhandene Datenbank</span><span class="sxs-lookup"><span data-stu-id="c1467-157">Create an Existing Database</span></span>

<span data-ttu-id="c1467-158">In der Regel müssen, wenn Sie eine vorhandene Datenbank verwenden möchten, die sie bereits erstellt wird, aber in dieser exemplarischen Vorgehensweise wir zum Erstellen einer Datenbank auf.</span><span class="sxs-lookup"><span data-stu-id="c1467-158">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="c1467-159">Der Datenbankserver, der mit Visual Studio installiert ist, unterscheidet sich abhängig von der Version von Visual Studio, die Sie installiert haben:</span><span class="sxs-lookup"><span data-stu-id="c1467-159">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="c1467-160">Wenn Sie Visual Studio 2010 verwenden erstellen Sie eine SQL Express-Datenbank.</span><span class="sxs-lookup"><span data-stu-id="c1467-160">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="c1467-161">Wenn Sie Visual Studio 2012 verwenden, und klicken Sie dann Sie erstellen eine [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) Datenbank.</span><span class="sxs-lookup"><span data-stu-id="c1467-161">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) database.</span></span>

<span data-ttu-id="c1467-162">Wir jetzt, und Erstellen der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="c1467-162">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="c1467-163">**Ansicht – Profiler -&gt; Server-Explorer**</span><span class="sxs-lookup"><span data-stu-id="c1467-163">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="c1467-164">Klicken Sie mit der rechten Maustaste auf **Datenverbindungen -&gt; Verbindung hinzufügen...**</span><span class="sxs-lookup"><span data-stu-id="c1467-164">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="c1467-165">Wenn Sie vor dem müssen Sie Microsoft SQL Server als Datenquelle wählen im Server-Explorer mit einer Datenbank verbunden haben</span><span class="sxs-lookup"><span data-stu-id="c1467-165">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![ChangeDataSource](~/ef6/media/changedatasource.png)

-   <span data-ttu-id="c1467-167">Eine Verbindung mit LocalDB oder SQL Express, je nachdem, welches Sie installiert haben, und geben Sie **Produkte** als Datenbankname</span><span class="sxs-lookup"><span data-stu-id="c1467-167">Connect to either LocalDB or SQL Express, depending on which one you have installed, and enter **Products** as the database name</span></span>

    ![AddConnectionLocalDB](~/ef6/media/addconnectionlocaldb.png)

    ![AddConnectionExpress](~/ef6/media/addconnectionexpress.png)

-   <span data-ttu-id="c1467-170">Wählen Sie **OK** und Sie werden gefragt, ob Sie eine neue Datenbank, die auf erstellen möchten **Ja**</span><span class="sxs-lookup"><span data-stu-id="c1467-170">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![CreateDatabase](~/ef6/media/createdatabase.png)

-   <span data-ttu-id="c1467-172">Die neue Datenbank wird jetzt im Server-Explorer angezeigt. mit der rechten Maustaste darauf und wählen Sie **neue Abfrage**</span><span class="sxs-lookup"><span data-stu-id="c1467-172">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="c1467-173">Kopieren Sie die folgende SQL-Anweisung in die neue Abfrage, und klicken Sie dann mit der rechten Maustaste auf die Abfrage, und wählen **ausführen**</span><span class="sxs-lookup"><span data-stu-id="c1467-173">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

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

#### <a name="reverse-engineer-model"></a><span data-ttu-id="c1467-174">Reverse Engineering-Modells</span><span class="sxs-lookup"><span data-stu-id="c1467-174">Reverse Engineer Model</span></span>

<span data-ttu-id="c1467-175">Wir werden die Verwendung von Entity Framework Designer, die als Teil von Visual Studio enthalten ist, um unser Modell zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="c1467-175">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="c1467-176">**Projekt -&gt; neues Element hinzufügen...**</span><span class="sxs-lookup"><span data-stu-id="c1467-176">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="c1467-177">Wählen Sie **Daten** im linken Menü und dann **ADO.NET Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="c1467-177">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="c1467-178">Geben Sie **ProductModel** als ein, und klicken Sie auf **OK**</span><span class="sxs-lookup"><span data-stu-id="c1467-178">Enter **ProductModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="c1467-179">Dadurch wird die **Entity Data Model-Assistenten**</span><span class="sxs-lookup"><span data-stu-id="c1467-179">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="c1467-180">Wählen Sie **aus Datenbank generieren** , und klicken Sie auf **weiter**</span><span class="sxs-lookup"><span data-stu-id="c1467-180">Select **Generate from Database** and click **Next**</span></span>

    ![ChooseModelContents](~/ef6/media/choosemodelcontents.png)

-   <span data-ttu-id="c1467-182">Wählen Sie die Verbindung mit der Datenbank, die Sie im ersten Abschnitt erstellt haben, geben Sie **ProductContext** als Namen für die Verbindungszeichenfolge und klicken Sie auf **weiter**</span><span class="sxs-lookup"><span data-stu-id="c1467-182">Select the connection to the database you created in the first section, enter **ProductContext** as the name of the connection string and click **Next**</span></span>

    ![ChooseYourConnection](~/ef6/media/chooseyourconnection.png)

-   <span data-ttu-id="c1467-184">Klicken Sie auf das Kontrollkästchen neben "Tabellen", um alle Tabellen importiert werden, und klicken Sie auf "Fertig stellen"</span><span class="sxs-lookup"><span data-stu-id="c1467-184">Click the checkbox next to ‘Tables’ to import all tables and click ‘Finish’</span></span>

    ![ChooseYourObjects](~/ef6/media/chooseyourobjects.png)

<span data-ttu-id="c1467-186">Nach Abschluss der reverse-Engineering-Prozess ist das neue Modell dem Projekt hinzugefügt und eröffnet Ihnen im Entity Framework Designer angezeigt.</span><span class="sxs-lookup"><span data-stu-id="c1467-186">Once the reverse engineer process completes the new model is added to your project and opened up for you to view in the Entity Framework Designer.</span></span> <span data-ttu-id="c1467-187">Zu Ihrem Projekt mit den Verbindungsdetails für die Datenbank wurde auch eine Datei "App.config" hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="c1467-187">An App.config file has also been added to your project with the connection details for the database.</span></span>

#### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="c1467-188">Zusätzliche Schritte in Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="c1467-188">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="c1467-189">Wenn Sie in Visual Studio 2010 arbeiten, müssen Sie den EF Designer zur Verwendung von EF6-codegenerierung zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="c1467-189">If you are working in Visual Studio 2010 then you will need to update the EF designer to use EF6 code generation.</span></span>

-   <span data-ttu-id="c1467-190">Mit der rechten Maustaste auf eine leere Stelle des Modells im EF Designer, und wählen Sie **Codegenerierungselement hinzufügen...**</span><span class="sxs-lookup"><span data-stu-id="c1467-190">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="c1467-191">Wählen Sie **Onlinevorlagen** aus dem linken Menü und die Suche nach **"DbContext"**</span><span class="sxs-lookup"><span data-stu-id="c1467-191">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="c1467-192">Wählen Sie die **EF 6.x DbContext Generator für C\#,** geben **ProductsModel** als den Namen, und klicken Sie auf Hinzufügen</span><span class="sxs-lookup"><span data-stu-id="c1467-192">Select the **EF 6.x DbContext Generator for C\#,** enter **ProductsModel** as the name and click Add</span></span>

#### <a name="updating-code-generation-for-data-binding"></a><span data-ttu-id="c1467-193">Aktualisieren die Generierung von Code für die Datenbindung</span><span class="sxs-lookup"><span data-stu-id="c1467-193">Updating code generation for data binding</span></span>

<span data-ttu-id="c1467-194">EF generiert Code aus dem Modell mithilfe von T4-Vorlagen.</span><span class="sxs-lookup"><span data-stu-id="c1467-194">EF generates code from your model using T4 templates.</span></span> <span data-ttu-id="c1467-195">Die Vorlagen in Visual Studio bereitgestellt oder von der Visual Studio Gallery heruntergeladen werden für die allgemeine Verwendung vorgesehen.</span><span class="sxs-lookup"><span data-stu-id="c1467-195">The templates shipped with Visual Studio or downloaded from the Visual Studio gallery are intended for general purpose use.</span></span> <span data-ttu-id="c1467-196">Dies bedeutet, dass die Entitäten, die mit diesen Vorlagen generierten einfach ICollection&lt;T&gt; Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="c1467-196">This means that the entities generated from these templates have simple ICollection&lt;T&gt; properties.</span></span> <span data-ttu-id="c1467-197">Bei der Daten für das Binden mit WPF es ist jedoch wünschenswert, verwenden Sie **ObservableCollection** für die Auflistungseigenschaften damit, die von WPF an die Sammlungen vorgenommenen Änderungen mitverfolgen kann.</span><span class="sxs-lookup"><span data-stu-id="c1467-197">However, when doing data binding using WPF it is desirable to use **ObservableCollection** for collection properties so that WPF can keep track of changes made to the collections.</span></span> <span data-ttu-id="c1467-198">Aus diesem Grund werden wir so ändern Sie die Vorlagen, um ObservableCollection verwenden.</span><span class="sxs-lookup"><span data-stu-id="c1467-198">To this end we will to modify the templates to use ObservableCollection.</span></span>

-   <span data-ttu-id="c1467-199">Öffnen der **Projektmappen-Explorer** und **ProductModel.edmx** Datei</span><span class="sxs-lookup"><span data-stu-id="c1467-199">Open the **Solution Explorer** and find **ProductModel.edmx** file</span></span>
-   <span data-ttu-id="c1467-200">Suchen der **ProductModel.tt** Datei, die unter der Datei ProductModel.edmx geschachtelt werden</span><span class="sxs-lookup"><span data-stu-id="c1467-200">Find the **ProductModel.tt** file which will be nested under the ProductModel.edmx file</span></span>

    ![WpfProductModelTemplate](~/ef6/media/wpfproductmodeltemplate.png)

-   <span data-ttu-id="c1467-202">Doppelklicken Sie auf die Datei ProductModel.tt, um sie in Visual Studio-Editor öffnen</span><span class="sxs-lookup"><span data-stu-id="c1467-202">Double-click on the ProductModel.tt file to open it in the Visual Studio editor</span></span>
-   <span data-ttu-id="c1467-203">Suchen und Ersetzen Sie die beiden Instanzen der "**ICollection**"mit"**ObservableCollection**".</span><span class="sxs-lookup"><span data-stu-id="c1467-203">Find and replace the two occurrences of “**ICollection**” with “**ObservableCollection**”.</span></span> <span data-ttu-id="c1467-204">Diese befinden sich ungefähr in Zeile 296 und 484.</span><span class="sxs-lookup"><span data-stu-id="c1467-204">These are located approximately at lines 296 and 484.</span></span>
-   <span data-ttu-id="c1467-205">Suchen und Ersetzen Sie das erste Vorkommen von "**HashSet**"mit"**ObservableCollection**".</span><span class="sxs-lookup"><span data-stu-id="c1467-205">Find and replace the first occurrence of “**HashSet**” with “**ObservableCollection**”.</span></span> <span data-ttu-id="c1467-206">Dieses Ereignis befindet sich ungefähr in Zeile 50.</span><span class="sxs-lookup"><span data-stu-id="c1467-206">This occurrence is located approximately at line 50.</span></span> <span data-ttu-id="c1467-207">**Nicht** beim zweite Vorkommen der HashSet finden Sie weiter unten in den Code zu ersetzen.</span><span class="sxs-lookup"><span data-stu-id="c1467-207">**Do not** replace the second occurrence of HashSet found later in the code.</span></span>
-   <span data-ttu-id="c1467-208">Suchen und Ersetzen Sie das einzige Vorkommen von "**System.Collections.Generic**"mit"**System.Collections.ObjectModel**".</span><span class="sxs-lookup"><span data-stu-id="c1467-208">Find and replace the only occurrence of “**System.Collections.Generic**” with “**System.Collections.ObjectModel**”.</span></span> <span data-ttu-id="c1467-209">Dies ist ungefähr in Zeile 424 befindet.</span><span class="sxs-lookup"><span data-stu-id="c1467-209">This is located approximately at line 424.</span></span>
-   <span data-ttu-id="c1467-210">Speichern Sie die ProductModel.tt-Datei.</span><span class="sxs-lookup"><span data-stu-id="c1467-210">Save the ProductModel.tt file.</span></span> <span data-ttu-id="c1467-211">Dadurch sollte den Code für Entitäten neu generiert werden.</span><span class="sxs-lookup"><span data-stu-id="c1467-211">This should cause the code for entities to be regenerated.</span></span> <span data-ttu-id="c1467-212">Wenn der Code nicht automatisch neu generiert wird, klicken Sie mit der rechten Maustaste auf ProductModel.tt, und wählen Sie "Benutzerdefiniertes Tool ausführen".</span><span class="sxs-lookup"><span data-stu-id="c1467-212">If the code does not regenerate automatically, then right click on ProductModel.tt and choose “Run Custom Tool”.</span></span>

<span data-ttu-id="c1467-213">Wenn Sie jetzt die Category.cs-Datei (mit unter ProductModel.tt geschachtelt ist) öffnen, und klicken Sie dann Sie sehen, dass die produktsammlung den Typ **ObservableCollection&lt;Produkt&gt;**.</span><span class="sxs-lookup"><span data-stu-id="c1467-213">If you now open the Category.cs file (which is nested under ProductModel.tt) then you should see that the Products collection has the type **ObservableCollection&lt;Product&gt;**.</span></span>

<span data-ttu-id="c1467-214">Kompilieren Sie das Projekt.</span><span class="sxs-lookup"><span data-stu-id="c1467-214">Compile the project.</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="c1467-215">Lazy Loading</span><span class="sxs-lookup"><span data-stu-id="c1467-215">Lazy Loading</span></span>

<span data-ttu-id="c1467-216">Die **Produkte** Eigenschaft für die **Kategorie** Klasse und **Kategorie** Eigenschaft für die **Produkt** -Klasse sind Navigationseigenschaften.</span><span class="sxs-lookup"><span data-stu-id="c1467-216">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="c1467-217">Entity Framework bieten Ihnen eine Möglichkeit, eine Beziehung zwischen zwei Entitätstypen zu navigieren Navigationseigenschaften.</span><span class="sxs-lookup"><span data-stu-id="c1467-217">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="c1467-218">EF bietet Ihnen eine Option beim Laden von verknüpften Entitäten aus der Datenbank automatisch den ersten Zugriff auf die Navigationseigenschaft.</span><span class="sxs-lookup"><span data-stu-id="c1467-218">EF gives you an option of loading related entities from the database automatically the first time you access the navigation property.</span></span> <span data-ttu-id="c1467-219">Bedenken Sie mit dieser Art von Loading (verzögertes Laden bezeichnet), dass beim ersten Zugriff auf jede Navigationseigenschaft eine separate Abfrage für die Datenbank ausgeführt wird, wenn der Inhalt nicht bereits im Kontext ist.</span><span class="sxs-lookup"><span data-stu-id="c1467-219">With this type of loading (called lazy loading), be aware that the first time you access each navigation property a separate query will be executed against the database if the contents are not already in the context.</span></span>

<span data-ttu-id="c1467-220">Wenn Sie POCO-Entität-Typen zu verwenden, erreicht EF Lazy Load durch Erstellen von Instanzen der abgeleiteten Proxytypen während der Laufzeit, und klicken Sie dann überschreiben virtuelle Eigenschaften in den Klassen den Laden Hook hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="c1467-220">When using POCO entity types, EF achieves lazy loading by creating instances of derived proxy types during runtime and then overriding virtual properties in your classes to add the loading hook.</span></span> <span data-ttu-id="c1467-221">Um lazy Loading von verknüpften Objekten zu erhalten, müssen Sie deklarieren, Navigation Eigenschaftengetter als **öffentliche** und **virtuellen** (**Overridable** in Visual Basic), und Sie Klasse darf nicht sein. **versiegelten** (**NotOverridable** in Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="c1467-221">To get lazy loading of related objects, you must declare navigation property getters as **public** and **virtual** (**Overridable** in Visual Basic), and you class must not be **sealed** (**NotOverridable** in Visual Basic).</span></span> <span data-ttu-id="c1467-222">Wenn die Datenbank mithilfe von werden erste Navigationseigenschaften automatisch virtuelle zum Aktivieren von lazy Loading vorgenommen.</span><span class="sxs-lookup"><span data-stu-id="c1467-222">When using Database First navigation properties are automatically made virtual to enable lazy loading.</span></span> <span data-ttu-id="c1467-223">Im Abschnitt mit Code First haben wir die Navigationseigenschaften aus demselben Grund virtuellen stellen</span><span class="sxs-lookup"><span data-stu-id="c1467-223">In the Code First section we chose to make the navigation properties virtual for the same reason</span></span>

## <a name="bind-object-to-controls"></a><span data-ttu-id="c1467-224">Binden Sie Objekt wird an Steuerelemente.</span><span class="sxs-lookup"><span data-stu-id="c1467-224">Bind Object to Controls</span></span>

<span data-ttu-id="c1467-225">Fügen Sie die Klassen, die im Modell, als Datenquellen für dieses WPF-Anwendung definiert sind hinzu.</span><span class="sxs-lookup"><span data-stu-id="c1467-225">Add the classes that are defined in the model as data sources for this WPF application.</span></span>

-   <span data-ttu-id="c1467-226">Doppelklicken Sie auf **"MainWindow.xaml"** im Projektmappen-Explorer, um das Hauptformular zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="c1467-226">Double-click **MainWindow.xaml** in Solution Explorer to open the main form</span></span>
-   <span data-ttu-id="c1467-227">Wählen Sie im Hauptmenü **-Projekt –&gt; neue Datenquelle hinzufügen...**</span><span class="sxs-lookup"><span data-stu-id="c1467-227">From the main menu, select **Project -&gt; Add New Data Source …**</span></span>
    <span data-ttu-id="c1467-228">(in Visual Studio 2010 müssen Sie auswählen **Daten per Push –&gt; neue Datenquelle hinzufügen...** )</span><span class="sxs-lookup"><span data-stu-id="c1467-228">(in Visual Studio 2010, you need to select **Data -&gt; Add New Data Source…**)</span></span>
-   <span data-ttu-id="c1467-229">In der wählen Sie eine Data Source Typewindow **Objekt** , und klicken Sie auf **weiter**</span><span class="sxs-lookup"><span data-stu-id="c1467-229">In the Choose a Data Source Typewindow, select **Object** and click **Next**</span></span>
-   <span data-ttu-id="c1467-230">Das Dialogfeld "Data-Objekte" Wählen Sie im Erweitern der **WPFwithEFSample** zwei Mal, und wählen **Kategorie**</span><span class="sxs-lookup"><span data-stu-id="c1467-230">In the Select the Data Objects dialog, unfold the **WPFwithEFSample** two times and select **Category**</span></span>  
    <span data-ttu-id="c1467-231">*Besteht keine Notwendigkeit, wählen Sie die **Produkt** die Datenquelle, da wir mit erhält die **Produkt**des Eigenschaft für die **Kategorie** -Datenquelle*</span><span class="sxs-lookup"><span data-stu-id="c1467-231">*There is no need to select the **Product** data source, because we will get to it through the **Product**’s property on the **Category** data source*</span></span>  

    ![SelectDataObjects](~/ef6/media/selectdataobjects.png)

-   <span data-ttu-id="c1467-233">Klicken Sie auf **Fertig stellen.**</span><span class="sxs-lookup"><span data-stu-id="c1467-233">Click **Finish.**</span></span>
-   <span data-ttu-id="c1467-234">Das Fenster "Datenquellen" wird geöffnet, neben dem Fenster "MainWindow.xaml" *, wenn das Fenster "Datenquellen" nicht angezeigt wird, wählen Sie **Ansicht – Profiler -&gt; andere Windows -&gt; -Datenquellen***</span><span class="sxs-lookup"><span data-stu-id="c1467-234">The Data Sources window is opened next to the MainWindow.xaml window *If the Data Sources window is not showing up, select **View -&gt; Other Windows-&gt; Data Sources***</span></span>
-   <span data-ttu-id="c1467-235">Drücken Sie das Symbol zum anheften, damit das Fenster "Datenquellen" nicht automatisch ausgeblendet.</span><span class="sxs-lookup"><span data-stu-id="c1467-235">Press the pin icon, so the Data Sources window does not auto hide.</span></span> <span data-ttu-id="c1467-236">Sie müssen möglicherweise die Schaltfläche "Aktualisieren" erreicht, wenn das Fenster bereits sichtbar war.</span><span class="sxs-lookup"><span data-stu-id="c1467-236">You may need to hit the refresh button if the window was already visible.</span></span>

    ![DataSources](~/ef6/media/datasources.png)

-   <span data-ttu-id="c1467-238">Wählen Sie die ** Kategorie ** Data source, und ziehen Sie es auf dem Formular.</span><span class="sxs-lookup"><span data-stu-id="c1467-238">Select the **Category **data source and drag it on the form.</span></span>

<span data-ttu-id="c1467-239">Folgendes passiert, wenn wir diese Quelle gezogen:</span><span class="sxs-lookup"><span data-stu-id="c1467-239">The following happened when we dragged this source:</span></span>

-   <span data-ttu-id="c1467-240">Die **CategoryViewSource** Ressource und die ** CategoryDataGrid ** Steuerelement XAML hinzugefügt wurden.</span><span class="sxs-lookup"><span data-stu-id="c1467-240">The **categoryViewSource** resource and the** categoryDataGrid** control were added to XAML.</span></span> <span data-ttu-id="c1467-241">Weitere Informationen zu DataViewSources, finden Sie unter http://bea.stollnitz.com/blog/?p=387.</span><span class="sxs-lookup"><span data-stu-id="c1467-241">For more information about DataViewSources, see http://bea.stollnitz.com/blog/?p=387.</span></span>
-   <span data-ttu-id="c1467-242">Die DataContext-Eigenschaft des übergeordneten Grid-Elements wurde festgelegt auf "{StaticResource **CategoryViewSource** }".</span><span class="sxs-lookup"><span data-stu-id="c1467-242">The DataContext property on the parent Grid element was set to "{StaticResource **categoryViewSource** }".</span></span>  <span data-ttu-id="c1467-243">Die **CategoryViewSource** Ressourcen dient als Bindungsquelle für das äußere\\übergeordneten Grid-Elements.</span><span class="sxs-lookup"><span data-stu-id="c1467-243">The **categoryViewSource** resource serves as a binding source for the outer\\parent Grid element.</span></span> <span data-ttu-id="c1467-244">Das innere Rasterelemente erben klicken Sie dann den DataContext-Wert aus dem übergeordneten Raster (das CategoryDataGrid die ItemsSource-Eigenschaft ist auf "{Binding}" festgelegt).</span><span class="sxs-lookup"><span data-stu-id="c1467-244">The inner Grid elements then inherit the DataContext value from the parent Grid (the categoryDataGrid’s ItemsSource property is set to "{Binding}").</span></span> 

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

## <a name="adding-a-details-grid"></a><span data-ttu-id="c1467-245">Hinzufügen eines Rasters Details</span><span class="sxs-lookup"><span data-stu-id="c1467-245">Adding a Details Grid</span></span>

<span data-ttu-id="c1467-246">Nun, wir haben ein Raster aus, um Kategorien anzuzeigen, lassen Sie uns ein Raster für die zugehörigen Produkte hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="c1467-246">Now that we have a grid to display Categories let's add a details grid to display the associated Products.</span></span>

-   <span data-ttu-id="c1467-247">Wählen Sie die ** Produkte **-Eigenschaft aus, unter der ** Kategorie ** Data source, und ziehen Sie es auf dem Formular.</span><span class="sxs-lookup"><span data-stu-id="c1467-247">Select the **Products **property from under the **Category **data source and drag it on the form.</span></span>
    -   <span data-ttu-id="c1467-248">Die **CategoryProductsViewSource** Ressource und **ProductDataGrid** XAML Raster hinzugefügt werden</span><span class="sxs-lookup"><span data-stu-id="c1467-248">The **categoryProductsViewSource** resource and **productDataGrid** grid are added to XAML</span></span>
    -   <span data-ttu-id="c1467-249">Der Bindungspfad für diese Ressource ist auf Produkte festgelegt.</span><span class="sxs-lookup"><span data-stu-id="c1467-249">The binding path for this resource is set to Products</span></span>
    -   <span data-ttu-id="c1467-250">WPF-Datenbindung Framework wird sichergestellt, dass nur Produkte, die im Zusammenhang mit der ausgewählten Kategorie angezeigt **ProductDataGrid**</span><span class="sxs-lookup"><span data-stu-id="c1467-250">WPF data-binding framework ensures that only Products related to the selected Category show up in **productDataGrid**</span></span>
-   <span data-ttu-id="c1467-251">Ziehen Sie aus der Toolbox **Schaltfläche** an das Formular.</span><span class="sxs-lookup"><span data-stu-id="c1467-251">From the Toolbox, drag **Button** on to the form.</span></span> <span data-ttu-id="c1467-252">Festlegen der **Name** Eigenschaft **ButtonSave** und **Inhalt** Eigenschaft, um **speichern**.</span><span class="sxs-lookup"><span data-stu-id="c1467-252">Set the **Name** property to **buttonSave** and the **Content** property to **Save**.</span></span>

<span data-ttu-id="c1467-253">Das Formular sollte etwa wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="c1467-253">The form should look similar to this:</span></span>

![Designer](~/ef6/media/designer.png) 

## <a name="add-code-that-handles-data-interaction"></a><span data-ttu-id="c1467-255">Fügen Sie Code, die Dateninteraktion behandelt.</span><span class="sxs-lookup"><span data-stu-id="c1467-255">Add Code that Handles Data Interaction</span></span>

<span data-ttu-id="c1467-256">Es ist Zeit, einige Ereignishandler an das Hauptfenster hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="c1467-256">It's time to add some event handlers to the main window.</span></span>

-   <span data-ttu-id="c1467-257">Klicken Sie im XAML-Fenster auf die  **&lt;Fenster** -Element, das Hauptfenster zur Auswahl</span><span class="sxs-lookup"><span data-stu-id="c1467-257">In the XAML window, click on the **&lt;Window** element, this selects the main window</span></span>
-   <span data-ttu-id="c1467-258">In der **Eigenschaften** wählen **Ereignisse** oben rechts, doppelklicken Sie auf das Textfeld rechts neben der **Loaded** Bezeichnung</span><span class="sxs-lookup"><span data-stu-id="c1467-258">In the **Properties** window choose **Events** at the top right, then double-click the text box to right of the **Loaded** label</span></span>

    ![MainWindowProperties](~/ef6/media/mainwindowproperties.png)

-   <span data-ttu-id="c1467-260">Steigern der **klicken Sie auf** -Ereignis für die **speichern** Schaltfläche durch Doppelklicken auf die Schaltfläche "Speichern" im Designer.</span><span class="sxs-lookup"><span data-stu-id="c1467-260">Also add the **Click** event for the **Save** button by double-clicking the Save button in the designer.</span></span> 

<span data-ttu-id="c1467-261">Dadurch wird des Codes-behind für das Formular, jetzt bearbeiten wir den Code, um die ProductContext zu verwenden, um die Daten zugreift.</span><span class="sxs-lookup"><span data-stu-id="c1467-261">This brings you to the code behind for the form, we'll now edit the code to use the ProductContext to perform data access.</span></span> <span data-ttu-id="c1467-262">Aktualisieren Sie den Code für das MainWindow-Element, wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="c1467-262">Update the code for the MainWindow as shown below.</span></span>

<span data-ttu-id="c1467-263">Der Code deklariert eine lang ausgeführte Instanz der **ProductContext**.</span><span class="sxs-lookup"><span data-stu-id="c1467-263">The code declares a long-running instance of **ProductContext**.</span></span> <span data-ttu-id="c1467-264">Die **ProductContext** Objekt wird zum Abfragen und Speichern von Daten in der Datenbank verwendet.</span><span class="sxs-lookup"><span data-stu-id="c1467-264">The **ProductContext** object is used to query and save data to the database.</span></span> <span data-ttu-id="c1467-265">Die **Dispose**() für die **ProductContext** Instanz wird dann aufgerufen, von der überschriebenen **OnClosing** Methode.</span><span class="sxs-lookup"><span data-stu-id="c1467-265">The **Dispose**() on the **ProductContext** instance is then called from the overridden **OnClosing** method.</span></span> <span data-ttu-id="c1467-266">Die Codekommentare enthalten Details darüber, was der Code bewirkt.</span><span class="sxs-lookup"><span data-stu-id="c1467-266">The code comments provide details about what the code does.</span></span>

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

## <a name="test-the-wpf-application"></a><span data-ttu-id="c1467-267">Testen Sie die WPF-Anwendung</span><span class="sxs-lookup"><span data-stu-id="c1467-267">Test the WPF Application</span></span>

-   <span data-ttu-id="c1467-268">Kompilieren Sie die Anwendung, und führen Sie sie aus.</span><span class="sxs-lookup"><span data-stu-id="c1467-268">Compile and run the application.</span></span> <span data-ttu-id="c1467-269">Wenn Sie Code First verwendet haben, sehen Sie, dass eine **WPFwithEFSample.ProductContext** Datenbank wird für Sie erstellt.</span><span class="sxs-lookup"><span data-stu-id="c1467-269">If you used Code First, then you will see that a **WPFwithEFSample.ProductContext** database is created for you.</span></span>
-   <span data-ttu-id="c1467-270">Geben Sie einen Kategorienamen in den oberen Raster und Produkt im unteren Raster *Geben Sie nicht alles in ID-Spalten, da der Primärschlüssel von der Datenbank generiert wurde*</span><span class="sxs-lookup"><span data-stu-id="c1467-270">Enter a category name in the top grid and product names in the bottom grid *Do not enter anything in ID columns, because the primary key is generated by the database*</span></span>

    ![Bildschirm1](~/ef6/media/screen1.png)

-   <span data-ttu-id="c1467-272">Drücken Sie die **speichern** Schaltfläche, um die Daten in der Datenbank speichern</span><span class="sxs-lookup"><span data-stu-id="c1467-272">Press the **Save** button to save the data to the database</span></span>

<span data-ttu-id="c1467-273">Nach dem Aufruf von "DbContext" des **"SaveChanges"**(-), die IDs werden mit der Datenbank generierte Werte aufgefüllt.</span><span class="sxs-lookup"><span data-stu-id="c1467-273">After the call to DbContext’s **SaveChanges**(), the IDs are populated with the database generated values.</span></span> <span data-ttu-id="c1467-274">Da wir aufgerufen **aktualisieren**() nach **"SaveChanges"**() die **DataGrid** Steuerelemente mit als auch den neuen Werten aktualisiert werden.</span><span class="sxs-lookup"><span data-stu-id="c1467-274">Because we called **Refresh**() after **SaveChanges**() the **DataGrid** controls are updated with the new values as well.</span></span>

![Screen2](~/ef6/media/screen2.png)
