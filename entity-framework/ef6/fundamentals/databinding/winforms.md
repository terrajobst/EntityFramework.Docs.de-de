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
# <a name="databinding-with-winforms"></a><span data-ttu-id="a212c-102">Datenbindung mit Windows Forms</span><span class="sxs-lookup"><span data-stu-id="a212c-102">Databinding with WinForms</span></span>
<span data-ttu-id="a212c-103">Diese exemplarische Vorgehensweise veranschaulicht POCO-Typen an Windows Forms (WinForms) Steuerelemente in einem "Master / Detail-Formular zu binden.</span><span class="sxs-lookup"><span data-stu-id="a212c-103">This step-by-step walkthrough shows how to bind POCO types to Window Forms (WinForms) controls in a “master-detail" form.</span></span> <span data-ttu-id="a212c-104">Die Anwendung verwendet Entity Framework-Objekte mit Daten aus der Datenbank zu füllen, Nachverfolgen von Änderungen und Beibehalten von Daten in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="a212c-104">The application uses Entity Framework to populate objects with data from the database, track changes, and persist data to the database.</span></span>

<span data-ttu-id="a212c-105">Das Modell definiert zwei Typen, die Teilnahme an 1: n Beziehung: Kategorie (principal\\master) und das Produkt (abhängige\\Detail).</span><span class="sxs-lookup"><span data-stu-id="a212c-105">The model defines two types that participate in one-to-many relationship: Category (principal\\master) and Product (dependent\\detail).</span></span> <span data-ttu-id="a212c-106">Anschließend werden Visual Studio-Tools verwendet, um in das Modell, das die Windows Forms-Steuerelemente definierten Typen zu binden.</span><span class="sxs-lookup"><span data-stu-id="a212c-106">Then, the Visual Studio tools are used to bind the types defined in the model to the WinForms controls.</span></span> <span data-ttu-id="a212c-107">Das Framework der Windows Forms-Datenbindung ermöglicht die Navigation zwischen verknüpften Objekten: Auswählen von Zeilen in der die Masteransicht bewirkt, dass die Detailansicht, um mit den entsprechenden untergeordneten Daten zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="a212c-107">The WinForms data-binding framework enables navigation between related objects: selecting rows in the master view causes the detail view to update with the corresponding child data.</span></span>

<span data-ttu-id="a212c-108">Die Screenshots und Codebeispiele in dieser exemplarischen Vorgehensweise stammen aus Visual Studio 2013, aber Sie können in dieser exemplarischen Vorgehensweise mit Visual Studio 2012 oder Visual Studio 2010 ausführen.</span><span class="sxs-lookup"><span data-stu-id="a212c-108">The screen shots and code listings in this walkthrough are taken from Visual Studio 2013 but you can complete this walkthrough with Visual Studio 2012 or Visual Studio 2010.</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="a212c-109">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="a212c-109">Pre-Requisites</span></span>

<span data-ttu-id="a212c-110">Sie müssen Visual Studio 2013, Visual Studio 2012 oder Visual Studio 2010 installiert werden, um diese exemplarische Vorgehensweise abgeschlossen haben.</span><span class="sxs-lookup"><span data-stu-id="a212c-110">You need to have Visual Studio 2013, Visual Studio 2012 or Visual Studio 2010 installed to complete this walkthrough.</span></span>

<span data-ttu-id="a212c-111">Wenn Sie Visual Studio 2010 verwenden, müssen Sie auch NuGet zu installieren.</span><span class="sxs-lookup"><span data-stu-id="a212c-111">If you are using Visual Studio 2010, you also have to install NuGet.</span></span> <span data-ttu-id="a212c-112">Weitere Informationen finden Sie unter [Installing NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).</span><span class="sxs-lookup"><span data-stu-id="a212c-112">For more information, see [Installing NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).</span></span>

## <a name="create-the-application"></a><span data-ttu-id="a212c-113">Erstellen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="a212c-113">Create the Application</span></span>

-   <span data-ttu-id="a212c-114">Öffnen Sie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a212c-114">Open Visual Studio</span></span>
-   <span data-ttu-id="a212c-115">**Datei -&gt; neu –&gt; Projekt...**</span><span class="sxs-lookup"><span data-stu-id="a212c-115">**File -&gt; New -&gt; Project….**</span></span>
-   <span data-ttu-id="a212c-116">Wählen Sie **Windows** im linken Bereich und **Windows FormsApplication** im rechten Bereich</span><span class="sxs-lookup"><span data-stu-id="a212c-116">Select **Windows** in the left pane and **Windows FormsApplication** in the right pane</span></span>
-   <span data-ttu-id="a212c-117">Geben Sie **WinFormswithEFSample** als Name</span><span class="sxs-lookup"><span data-stu-id="a212c-117">Enter **WinFormswithEFSample** as the name</span></span>
-   <span data-ttu-id="a212c-118">Wählen Sie **OK** aus.</span><span class="sxs-lookup"><span data-stu-id="a212c-118">Select **OK**</span></span>

## <a name="install-the-entity-framework-nuget-package"></a><span data-ttu-id="a212c-119">Installieren Sie das Entity Framework-NuGet-Paket</span><span class="sxs-lookup"><span data-stu-id="a212c-119">Install the Entity Framework NuGet package</span></span>

-   <span data-ttu-id="a212c-120">Im Projektmappen-Explorer mit der Maustaste auf die **WinFormswithEFSample** Projekt</span><span class="sxs-lookup"><span data-stu-id="a212c-120">In Solution Explorer, right-click on the **WinFormswithEFSample** project</span></span>
-   <span data-ttu-id="a212c-121">Wählen Sie **NuGet-Pakete verwalten...**</span><span class="sxs-lookup"><span data-stu-id="a212c-121">Select **Manage NuGet Packages…**</span></span>
-   <span data-ttu-id="a212c-122">Wählen Sie in das Dialogfeld "NuGet-Pakete verwalten" die **Online** Registerkarte, und wählen Sie die **EntityFramework** Paket</span><span class="sxs-lookup"><span data-stu-id="a212c-122">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package</span></span>
-   <span data-ttu-id="a212c-123">Klicken Sie auf **installieren**</span><span class="sxs-lookup"><span data-stu-id="a212c-123">Click **Install**</span></span>  
    > [!NOTE]
    > <span data-ttu-id="a212c-124">Zusätzlich zu den EntityFramework-Assembly wird ein Verweis auf System.ComponentModel.DataAnnotations ebenfalls hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="a212c-124">In addition to the EntityFramework assembly a reference to System.ComponentModel.DataAnnotations is also added.</span></span> <span data-ttu-id="a212c-125">Wenn das Projekt einen Verweis auf System.Data.Entity verfügt, wird dann es entfernt, wenn das EntityFramework-Paket installiert wird.</span><span class="sxs-lookup"><span data-stu-id="a212c-125">If the project has a reference to System.Data.Entity, then it will be removed when the EntityFramework package is installed.</span></span> <span data-ttu-id="a212c-126">Die Assembly System.Data.Entity wird nicht mehr für Entity Framework 6-Anwendungen verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="a212c-126">The System.Data.Entity assembly is no longer used for Entity Framework 6 applications.</span></span>

## <a name="implementing-ilistsource-for-collections"></a><span data-ttu-id="a212c-127">Implementieren der IListSource für Sammlungen</span><span class="sxs-lookup"><span data-stu-id="a212c-127">Implementing IListSource for Collections</span></span>

<span data-ttu-id="a212c-128">-Auflistung – Eigenschaften müssen die zum Aktivieren der bidirektionale Datenbindung, bei der Sortierung, bei Verwendung von Windows Forms IListSource-Schnittstelle implementieren.</span><span class="sxs-lookup"><span data-stu-id="a212c-128">Collection properties must implement the IListSource interface to enable two-way data binding with sorting when using Windows Forms.</span></span> <span data-ttu-id="a212c-129">Zu diesem Zweck werden wir zum Erweitern der ObservableCollection IListSource-Funktionen hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="a212c-129">To do this we are going to extend ObservableCollection to add IListSource functionality.</span></span>

-   <span data-ttu-id="a212c-130">Hinzufügen einer **ObservableListSource** Klasse, um das Projekt:</span><span class="sxs-lookup"><span data-stu-id="a212c-130">Add an **ObservableListSource** class to the project:</span></span>
    -   <span data-ttu-id="a212c-131">Mit der rechten Maustaste auf den Namen des Projekts</span><span class="sxs-lookup"><span data-stu-id="a212c-131">Right-click on the project name</span></span>
    -   <span data-ttu-id="a212c-132">Wählen Sie **hinzufügen –&gt; neues Element**</span><span class="sxs-lookup"><span data-stu-id="a212c-132">Select **Add -&gt; New Item**</span></span>
    -   <span data-ttu-id="a212c-133">Wählen Sie **Klasse** , und geben Sie **ObservableListSource** für den Namen der Klasse</span><span class="sxs-lookup"><span data-stu-id="a212c-133">Select **Class** and enter **ObservableListSource** for the class name</span></span>
-   <span data-ttu-id="a212c-134">Ersetzen Sie den Code, der standardmäßig mit dem folgenden Code generiert:</span><span class="sxs-lookup"><span data-stu-id="a212c-134">Replace the code generated by default with the following code:</span></span>

<span data-ttu-id="a212c-135">*Diese Klasse ermöglicht die bidirektionale Daten, die Bindung sowie das Sortieren. Die Klasse ist von ObservableCollection&lt;T&gt; und fügt eine explizite Implementierung der IListSource. Die GetList()-Methode der IListSource wird implementiert, um eine Implementierung IBindingList zurückzugeben, die mit der ObservableCollection synchron bleibt. Die IBindingList-Implementierung von ToBindingList generiert das Sortieren unterstützt. Die ToBindingList Erweiterung-Methode ist in der EntityFramework-Assembly definiert.*</span><span class="sxs-lookup"><span data-stu-id="a212c-135">*This class enables two-way data binding as well as sorting. The class derives from ObservableCollection&lt;T&gt; and adds an explicit implementation of IListSource. The GetList() method of IListSource is implemented to return an IBindingList implementation that stays in sync with the ObservableCollection. The IBindingList implementation generated by ToBindingList supports sorting. The ToBindingList extention method is defined in the EntityFramework assembly.*</span></span>

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

## <a name="define-a-model"></a><span data-ttu-id="a212c-136">Definieren eines Modells</span><span class="sxs-lookup"><span data-stu-id="a212c-136">Define a Model</span></span>

<span data-ttu-id="a212c-137">In dieser exemplarischen Vorgehensweise können Sie ein Modell mithilfe von Code First oder dem EF Designer implementieren möchten.</span><span class="sxs-lookup"><span data-stu-id="a212c-137">In this walkthrough you can chose to implement a model using Code First or the EF Designer.</span></span> <span data-ttu-id="a212c-138">Führen Sie eine der beiden folgenden Abschnitte.</span><span class="sxs-lookup"><span data-stu-id="a212c-138">Complete one of the two following sections.</span></span>

### <a name="option-1-define-a-model-using-code-first"></a><span data-ttu-id="a212c-139">Option 1: Definieren eines Modells mit Code First</span><span class="sxs-lookup"><span data-stu-id="a212c-139">Option 1: Define a Model using Code First</span></span>

<span data-ttu-id="a212c-140">In diesem Abschnitt zeigt, wie Sie ein Modell und der zugehörigen Datenbank mithilfe von Code First zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="a212c-140">This section shows how to create a model and its associated database using Code First.</span></span> <span data-ttu-id="a212c-141">Fahren Sie mit dem nächsten Abschnitt (**Option 2: Definieren eines Modells mit Database First)** verwenden, wenn Sie stattdessen Database First Reverse Engineering für das Modell aus einer Datenbank mit dem EF-Designer</span><span class="sxs-lookup"><span data-stu-id="a212c-141">Skip to the next section (**Option 2: Define a model using Database First)** if you would rather use Database First to reverse engineer your model from a database using the EF designer</span></span>

<span data-ttu-id="a212c-142">Bei Verwendung von Code First-Entwicklung beginnen Sie in der Regel durch das Schreiben von .NET Framework-Klassen, die das konzeptionelle (Domäne)-Modell zu definieren.</span><span class="sxs-lookup"><span data-stu-id="a212c-142">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span>

-   <span data-ttu-id="a212c-143">Fügen Sie einen neuen **Produkt** Projekt</span><span class="sxs-lookup"><span data-stu-id="a212c-143">Add a new **Product** class to project</span></span>
-   <span data-ttu-id="a212c-144">Ersetzen Sie den Code, der standardmäßig mit dem folgenden Code generiert:</span><span class="sxs-lookup"><span data-stu-id="a212c-144">Replace the code generated by default with the following code:</span></span>

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

-   <span data-ttu-id="a212c-145">Hinzufügen einer **Kategorie** Klasse, um das Projekt.</span><span class="sxs-lookup"><span data-stu-id="a212c-145">Add a **Category** class to the project.</span></span>
-   <span data-ttu-id="a212c-146">Ersetzen Sie den Code, der standardmäßig mit dem folgenden Code generiert:</span><span class="sxs-lookup"><span data-stu-id="a212c-146">Replace the code generated by default with the following code:</span></span>

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

<span data-ttu-id="a212c-147">Zusätzlich zum Definieren von Entitäten, müssen Sie eine Klasse definieren, die von abgeleitet **"DbContext"** und macht **"DbSet"&lt;TEntity&gt;**  Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="a212c-147">In addition to defining entities, you need to define a class that derives from **DbContext** and exposes **DbSet&lt;TEntity&gt;** properties.</span></span> <span data-ttu-id="a212c-148">Die **"DbSet"** Eigenschaften können Sie den Kontext, die wissen, welche Typen im Modell enthalten sein sollen.</span><span class="sxs-lookup"><span data-stu-id="a212c-148">The **DbSet** properties let the context know which types you want to include in the model.</span></span> <span data-ttu-id="a212c-149">Die **"DbContext"** und **"DbSet"** Typen werden in der EntityFramework-Assembly definiert.</span><span class="sxs-lookup"><span data-stu-id="a212c-149">The **DbContext** and **DbSet** types are defined in the EntityFramework assembly.</span></span>

<span data-ttu-id="a212c-150">Eine Instanz des Typs "DbContext" abgeleitet verwaltet die Entitätsobjekte während der Laufzeit, einschließlich ausfüllenden Objekten mit Daten aus einer Datenbank, verfolgen und Speichern von Daten in der Datenbank zu ändern.</span><span class="sxs-lookup"><span data-stu-id="a212c-150">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

-   <span data-ttu-id="a212c-151">Fügen Sie einen neuen **ProductContext** Klasse, um das Projekt.</span><span class="sxs-lookup"><span data-stu-id="a212c-151">Add a new **ProductContext** class to the project.</span></span>
-   <span data-ttu-id="a212c-152">Ersetzen Sie den Code, der standardmäßig mit dem folgenden Code generiert:</span><span class="sxs-lookup"><span data-stu-id="a212c-152">Replace the code generated by default with the following code:</span></span>

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

<span data-ttu-id="a212c-153">Kompilieren Sie das Projekt.</span><span class="sxs-lookup"><span data-stu-id="a212c-153">Compile the project.</span></span>

### <a name="option-2-define-a-model-using-database-first"></a><span data-ttu-id="a212c-154">Option 2: Definieren eines Modells mit Database First</span><span class="sxs-lookup"><span data-stu-id="a212c-154">Option 2: Define a model using Database First</span></span>

<span data-ttu-id="a212c-155">In diesem Abschnitt zeigt, wie mit Database First zum zurückentwickeln das Modell aus einer Datenbank mit dem EF Designer.</span><span class="sxs-lookup"><span data-stu-id="a212c-155">This section shows how to use Database First to reverse engineer your model from a database using the EF designer.</span></span> <span data-ttu-id="a212c-156">Wenn Sie den vorherigen Abschnitt abgeschlossen (**Option 1: Definieren eines Modells mithilfe von Code First)**, klicken Sie dann diesen Abschnitt überspringen und sofort die **Lazy Loading** Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="a212c-156">If you completed the previous section (**Option 1: Define a model using Code First)**, then skip this section and go straight to the **Lazy Loading** section.</span></span>

#### <a name="create-an-existing-database"></a><span data-ttu-id="a212c-157">Erstellen Sie eine vorhandene Datenbank</span><span class="sxs-lookup"><span data-stu-id="a212c-157">Create an Existing Database</span></span>

<span data-ttu-id="a212c-158">In der Regel müssen, wenn Sie eine vorhandene Datenbank verwenden möchten, die sie bereits erstellt wird, aber in dieser exemplarischen Vorgehensweise wir zum Erstellen einer Datenbank auf.</span><span class="sxs-lookup"><span data-stu-id="a212c-158">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="a212c-159">Der Datenbankserver, der mit Visual Studio installiert ist, unterscheidet sich abhängig von der Version von Visual Studio, die Sie installiert haben:</span><span class="sxs-lookup"><span data-stu-id="a212c-159">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="a212c-160">Wenn Sie Visual Studio 2010 verwenden erstellen Sie eine SQL Express-Datenbank.</span><span class="sxs-lookup"><span data-stu-id="a212c-160">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="a212c-161">Wenn Sie Visual Studio 2012 verwenden, und klicken Sie dann Sie erstellen eine [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) Datenbank.</span><span class="sxs-lookup"><span data-stu-id="a212c-161">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) database.</span></span>

<span data-ttu-id="a212c-162">Wir jetzt, und Erstellen der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="a212c-162">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="a212c-163">**Ansicht – Profiler -&gt; Server-Explorer**</span><span class="sxs-lookup"><span data-stu-id="a212c-163">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="a212c-164">Klicken Sie mit der rechten Maustaste auf **Datenverbindungen -&gt; Verbindung hinzufügen...**</span><span class="sxs-lookup"><span data-stu-id="a212c-164">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="a212c-165">Wenn Sie vor dem müssen Sie Microsoft SQL Server als Datenquelle wählen im Server-Explorer mit einer Datenbank verbunden haben</span><span class="sxs-lookup"><span data-stu-id="a212c-165">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![Datenquelle wechseln](~/ef6/media/changedatasource.png)

-   <span data-ttu-id="a212c-167">Eine Verbindung mit LocalDB oder SQL Express, je nachdem, welches Sie installiert haben, und geben Sie **Produkte** als Datenbankname</span><span class="sxs-lookup"><span data-stu-id="a212c-167">Connect to either LocalDB or SQL Express, depending on which one you have installed, and enter **Products** as the database name</span></span>

    ![Hinzufügen der Verbindung LocalDB](~/ef6/media/addconnectionlocaldb.png)

    ![Verbindung Express hinzufügen](~/ef6/media/addconnectionexpress.png)

-   <span data-ttu-id="a212c-170">Wählen Sie **OK** und Sie werden gefragt, ob Sie eine neue Datenbank, die auf erstellen möchten **Ja**</span><span class="sxs-lookup"><span data-stu-id="a212c-170">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![Datenbank erstellen](~/ef6/media/createdatabase.png)

-   <span data-ttu-id="a212c-172">Die neue Datenbank wird jetzt im Server-Explorer angezeigt. mit der rechten Maustaste darauf und wählen Sie **neue Abfrage**</span><span class="sxs-lookup"><span data-stu-id="a212c-172">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="a212c-173">Kopieren Sie die folgende SQL-Anweisung in die neue Abfrage, und klicken Sie dann mit der rechten Maustaste auf die Abfrage, und wählen **ausführen**</span><span class="sxs-lookup"><span data-stu-id="a212c-173">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

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

#### <a name="reverse-engineer-model"></a><span data-ttu-id="a212c-174">Reverse Engineering-Modells</span><span class="sxs-lookup"><span data-stu-id="a212c-174">Reverse Engineer Model</span></span>

<span data-ttu-id="a212c-175">Wir werden die Verwendung von Entity Framework Designer, die als Teil von Visual Studio enthalten ist, um unser Modell zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="a212c-175">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="a212c-176">**Projekt -&gt; neues Element hinzufügen...**</span><span class="sxs-lookup"><span data-stu-id="a212c-176">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="a212c-177">Wählen Sie **Daten** im linken Menü und dann **ADO.NET Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="a212c-177">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="a212c-178">Geben Sie **ProductModel** als ein, und klicken Sie auf **OK**</span><span class="sxs-lookup"><span data-stu-id="a212c-178">Enter **ProductModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="a212c-179">Dadurch wird die **Entity Data Model-Assistenten**</span><span class="sxs-lookup"><span data-stu-id="a212c-179">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="a212c-180">Wählen Sie **aus Datenbank generieren** , und klicken Sie auf **weiter**</span><span class="sxs-lookup"><span data-stu-id="a212c-180">Select **Generate from Database** and click **Next**</span></span>

    ![ChooseModelContents](~/ef6/media/choosemodelcontents.png)

-   <span data-ttu-id="a212c-182">Wählen Sie die Verbindung mit der Datenbank, die Sie im ersten Abschnitt erstellt haben, geben Sie **ProductContext** als Namen für die Verbindungszeichenfolge und klicken Sie auf **weiter**</span><span class="sxs-lookup"><span data-stu-id="a212c-182">Select the connection to the database you created in the first section, enter **ProductContext** as the name of the connection string and click **Next**</span></span>

    ![Wählen Sie die Verbindung](~/ef6/media/chooseyourconnection.png)

-   <span data-ttu-id="a212c-184">Klicken Sie auf das Kontrollkästchen neben "Tabellen", um alle Tabellen importiert werden, und klicken Sie auf "Fertig stellen"</span><span class="sxs-lookup"><span data-stu-id="a212c-184">Click the checkbox next to ‘Tables’ to import all tables and click ‘Finish’</span></span>

    ![Wählen Sie Ihre Objekte aus.](~/ef6/media/chooseyourobjects.png)

<span data-ttu-id="a212c-186">Nach Abschluss der reverse-Engineering-Prozess ist das neue Modell dem Projekt hinzugefügt und eröffnet Ihnen im Entity Framework Designer angezeigt.</span><span class="sxs-lookup"><span data-stu-id="a212c-186">Once the reverse engineer process completes the new model is added to your project and opened up for you to view in the Entity Framework Designer.</span></span> <span data-ttu-id="a212c-187">Zu Ihrem Projekt mit den Verbindungsdetails für die Datenbank wurde auch eine Datei "App.config" hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="a212c-187">An App.config file has also been added to your project with the connection details for the database.</span></span>

#### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="a212c-188">Zusätzliche Schritte in Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="a212c-188">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="a212c-189">Wenn Sie in Visual Studio 2010 arbeiten, müssen Sie den EF Designer zur Verwendung von EF6-codegenerierung zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="a212c-189">If you are working in Visual Studio 2010 then you will need to update the EF designer to use EF6 code generation.</span></span>

-   <span data-ttu-id="a212c-190">Mit der rechten Maustaste auf eine leere Stelle des Modells im EF Designer, und wählen Sie **Codegenerierungselement hinzufügen...**</span><span class="sxs-lookup"><span data-stu-id="a212c-190">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="a212c-191">Wählen Sie **Onlinevorlagen** aus dem linken Menü und die Suche nach **"DbContext"**</span><span class="sxs-lookup"><span data-stu-id="a212c-191">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="a212c-192">Wählen Sie die **EF 6.x DbContext Generator für C\#,** geben **ProductsModel** als den Namen, und klicken Sie auf Hinzufügen</span><span class="sxs-lookup"><span data-stu-id="a212c-192">Select the **EF 6.x DbContext Generator for C\#,** enter **ProductsModel** as the name and click Add</span></span>

#### <a name="updating-code-generation-for-data-binding"></a><span data-ttu-id="a212c-193">Aktualisieren die Generierung von Code für die Datenbindung</span><span class="sxs-lookup"><span data-stu-id="a212c-193">Updating code generation for data binding</span></span>

<span data-ttu-id="a212c-194">EF generiert Code aus dem Modell mithilfe von T4-Vorlagen.</span><span class="sxs-lookup"><span data-stu-id="a212c-194">EF generates code from your model using T4 templates.</span></span> <span data-ttu-id="a212c-195">Die Vorlagen in Visual Studio bereitgestellt oder von der Visual Studio Gallery heruntergeladen werden für die allgemeine Verwendung vorgesehen.</span><span class="sxs-lookup"><span data-stu-id="a212c-195">The templates shipped with Visual Studio or downloaded from the Visual Studio gallery are intended for general purpose use.</span></span> <span data-ttu-id="a212c-196">Dies bedeutet, dass die Entitäten, die mit diesen Vorlagen generierten einfach ICollection&lt;T&gt; Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="a212c-196">This means that the entities generated from these templates have simple ICollection&lt;T&gt; properties.</span></span> <span data-ttu-id="a212c-197">Allerdings ist bei der Datenbindung es wünschenswert,-Auflistung – Eigenschaften verfügen, die IListSource zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="a212c-197">However, when doing data binding it is desirable to have collection properties that implement IListSource.</span></span> <span data-ttu-id="a212c-198">Aus diesem Grund wir die oben gezeigte ObservableListSource-Klasse erstellt, und wir werden nun die Vorlagen zu ändern ist dieser Klasse verwenden.</span><span class="sxs-lookup"><span data-stu-id="a212c-198">This is why we created the ObservableListSource class above and we are now going to modify the templates to make use of this class.</span></span>

-   <span data-ttu-id="a212c-199">Öffnen der **Projektmappen-Explorer** und **ProductModel.edmx** Datei</span><span class="sxs-lookup"><span data-stu-id="a212c-199">Open the **Solution Explorer** and find **ProductModel.edmx** file</span></span>
-   <span data-ttu-id="a212c-200">Suchen der **ProductModel.tt** Datei, die unter der Datei ProductModel.edmx geschachtelt werden</span><span class="sxs-lookup"><span data-stu-id="a212c-200">Find the **ProductModel.tt** file which will be nested under the ProductModel.edmx file</span></span>

    ![Produkt-Manager-Vorlage](~/ef6/media/productmodeltemplate.png)

-   <span data-ttu-id="a212c-202">Doppelklicken Sie auf die Datei ProductModel.tt, um sie in Visual Studio-Editor öffnen</span><span class="sxs-lookup"><span data-stu-id="a212c-202">Double-click on the ProductModel.tt file to open it in the Visual Studio editor</span></span>
-   <span data-ttu-id="a212c-203">Suchen und Ersetzen Sie die beiden Instanzen der "**ICollection**"mit"**ObservableListSource**".</span><span class="sxs-lookup"><span data-stu-id="a212c-203">Find and replace the two occurrences of “**ICollection**” with “**ObservableListSource**”.</span></span> <span data-ttu-id="a212c-204">Diese befinden sich in etwa die Zeilen 296 und 484.</span><span class="sxs-lookup"><span data-stu-id="a212c-204">These are located at approximately lines 296 and 484.</span></span>
-   <span data-ttu-id="a212c-205">Suchen und Ersetzen Sie das erste Vorkommen von "**HashSet**"mit"**ObservableListSource**".</span><span class="sxs-lookup"><span data-stu-id="a212c-205">Find and replace the first occurrence of “**HashSet**” with “**ObservableListSource**”.</span></span> <span data-ttu-id="a212c-206">Dieses Ereignis befindet sich ungefähr bei Zeile 50.</span><span class="sxs-lookup"><span data-stu-id="a212c-206">This occurrence is located at approximately line 50.</span></span> <span data-ttu-id="a212c-207">**Nicht** beim zweite Vorkommen der HashSet finden Sie weiter unten in den Code zu ersetzen.</span><span class="sxs-lookup"><span data-stu-id="a212c-207">**Do not** replace the second occurrence of HashSet found later in the code.</span></span>
-   <span data-ttu-id="a212c-208">Speichern Sie die ProductModel.tt-Datei.</span><span class="sxs-lookup"><span data-stu-id="a212c-208">Save the ProductModel.tt file.</span></span> <span data-ttu-id="a212c-209">Dadurch sollte den Code für Entitäten neu generiert werden.</span><span class="sxs-lookup"><span data-stu-id="a212c-209">This should cause the code for entities to be regenerated.</span></span> <span data-ttu-id="a212c-210">Wenn der Code nicht automatisch neu generiert wird, klicken Sie mit der rechten Maustaste auf ProductModel.tt, und wählen Sie "Benutzerdefiniertes Tool ausführen".</span><span class="sxs-lookup"><span data-stu-id="a212c-210">If the code does not regenerate automatically, then right click on ProductModel.tt and choose “Run Custom Tool”.</span></span>

<span data-ttu-id="a212c-211">Wenn Sie jetzt die Category.cs-Datei (mit unter ProductModel.tt geschachtelt ist) öffnen, und klicken Sie dann Sie sehen, dass die produktsammlung den Typ **ObservableListSource&lt;Produkt&gt;**.</span><span class="sxs-lookup"><span data-stu-id="a212c-211">If you now open the Category.cs file (which is nested under ProductModel.tt) then you should see that the Products collection has the type **ObservableListSource&lt;Product&gt;**.</span></span>

<span data-ttu-id="a212c-212">Kompilieren Sie das Projekt.</span><span class="sxs-lookup"><span data-stu-id="a212c-212">Compile the project.</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="a212c-213">Lazy Loading</span><span class="sxs-lookup"><span data-stu-id="a212c-213">Lazy Loading</span></span>

<span data-ttu-id="a212c-214">Die **Produkte** Eigenschaft für die **Kategorie** Klasse und **Kategorie** Eigenschaft für die **Produkt** -Klasse sind Navigationseigenschaften.</span><span class="sxs-lookup"><span data-stu-id="a212c-214">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="a212c-215">Entity Framework bieten Ihnen eine Möglichkeit, eine Beziehung zwischen zwei Entitätstypen zu navigieren Navigationseigenschaften.</span><span class="sxs-lookup"><span data-stu-id="a212c-215">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="a212c-216">EF bietet Ihnen eine Option beim Laden von verknüpften Entitäten aus der Datenbank automatisch den ersten Zugriff auf die Navigationseigenschaft.</span><span class="sxs-lookup"><span data-stu-id="a212c-216">EF gives you an option of loading related entities from the database automatically the first time you access the navigation property.</span></span> <span data-ttu-id="a212c-217">Bedenken Sie mit dieser Art von Loading (verzögertes Laden bezeichnet), dass beim ersten Zugriff auf jede Navigationseigenschaft eine separate Abfrage für die Datenbank ausgeführt wird, wenn der Inhalt nicht bereits im Kontext ist.</span><span class="sxs-lookup"><span data-stu-id="a212c-217">With this type of loading (called lazy loading), be aware that the first time you access each navigation property a separate query will be executed against the database if the contents are not already in the context.</span></span>

<span data-ttu-id="a212c-218">Wenn Sie POCO-Entität-Typen zu verwenden, erreicht EF Lazy Load durch Erstellen von Instanzen der abgeleiteten Proxytypen während der Laufzeit, und klicken Sie dann überschreiben virtuelle Eigenschaften in den Klassen den Laden Hook hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="a212c-218">When using POCO entity types, EF achieves lazy loading by creating instances of derived proxy types during runtime and then overriding virtual properties in your classes to add the loading hook.</span></span> <span data-ttu-id="a212c-219">Um lazy Loading von verknüpften Objekten zu erhalten, müssen Sie deklarieren, Navigation Eigenschaftengetter als **öffentliche** und **virtuellen** (**Overridable** in Visual Basic), und Sie Klasse darf nicht sein. **versiegelten** (**NotOverridable** in Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="a212c-219">To get lazy loading of related objects, you must declare navigation property getters as **public** and **virtual** (**Overridable** in Visual Basic), and you class must not be **sealed** (**NotOverridable** in Visual Basic).</span></span> <span data-ttu-id="a212c-220">Wenn die Datenbank mithilfe von werden erste Navigationseigenschaften automatisch virtuelle zum Aktivieren von lazy Loading vorgenommen.</span><span class="sxs-lookup"><span data-stu-id="a212c-220">When using Database First navigation properties are automatically made virtual to enable lazy loading.</span></span> <span data-ttu-id="a212c-221">Im Abschnitt mit Code First haben wir die Navigationseigenschaften aus demselben Grund virtuellen stellen</span><span class="sxs-lookup"><span data-stu-id="a212c-221">In the Code First section we chose to make the navigation properties virtual for the same reason</span></span>

## <a name="bind-object-to-controls"></a><span data-ttu-id="a212c-222">Binden Sie Objekt wird an Steuerelemente.</span><span class="sxs-lookup"><span data-stu-id="a212c-222">Bind Object to Controls</span></span>

<span data-ttu-id="a212c-223">Fügen Sie die Klassen, die im Modell, als Datenquellen für diese Windows Forms-Anwendung definiert sind hinzu.</span><span class="sxs-lookup"><span data-stu-id="a212c-223">Add the classes that are defined in the model as data sources for this WinForms application.</span></span>

-   <span data-ttu-id="a212c-224">Wählen Sie im Hauptmenü **-Projekt –&gt; neue Datenquelle hinzufügen...**</span><span class="sxs-lookup"><span data-stu-id="a212c-224">From the main menu, select **Project -&gt; Add New Data Source …**</span></span>
    <span data-ttu-id="a212c-225">(in Visual Studio 2010 müssen Sie auswählen **Daten per Push –&gt; neue Datenquelle hinzufügen...** )</span><span class="sxs-lookup"><span data-stu-id="a212c-225">(in Visual Studio 2010, you need to select **Data -&gt; Add New Data Source…**)</span></span>
-   <span data-ttu-id="a212c-226">Wählen Sie in der ein Fenster Datenquellentyp auswählen, **Objekt** , und klicken Sie auf **weiter**</span><span class="sxs-lookup"><span data-stu-id="a212c-226">In the Choose a Data Source Type window, select **Object** and click **Next**</span></span>
-   <span data-ttu-id="a212c-227">Erweitern Sie das Dialogfeld "Data-Objekte" Wählen Sie im der **WinFormswithEFSample** zwei Mal, und wählen **Kategorie** besteht nicht erforderlich, wählen Sie die Produkt-Datenquelle, da wir, über des Produktanforderungen erhalten die Eigenschaft für die Kategorie-Datenquelle.</span><span class="sxs-lookup"><span data-stu-id="a212c-227">In the Select the Data Objects dialog, unfold the **WinFormswithEFSample** two times and select **Category** There is no need to select the Product data source, because we will get to it through the Product’s property on the Category data source.</span></span>

    ![Datenquelle](~/ef6/media/datasource.png)

-   <span data-ttu-id="a212c-229">Klicken Sie auf **Fertig stellen.** 
     *, Wenn das Fenster "Datenquellen" nicht angezeigt wird, wählen Sie \*\*\* Ansicht – Profiler -&gt; andere Windows -&gt; -Datenquellen*\*</span><span class="sxs-lookup"><span data-stu-id="a212c-229">Click **Finish.**
*If the Data Sources window is not showing up, select\*\*\*View -&gt; Other Windows-&gt; Data Sources*\*</span></span>
-   <span data-ttu-id="a212c-230">Drücken Sie das Symbol zum anheften, damit das Fenster "Datenquellen" nicht automatisch ausgeblendet.</span><span class="sxs-lookup"><span data-stu-id="a212c-230">Press the pin icon, so the Data Sources window does not auto hide.</span></span> <span data-ttu-id="a212c-231">Sie müssen möglicherweise die Schaltfläche "Aktualisieren" erreicht, wenn das Fenster bereits sichtbar war.</span><span class="sxs-lookup"><span data-stu-id="a212c-231">You may need to hit the refresh button if the window was already visible.</span></span>

    ![Datenquelle 2](~/ef6/media/datasource2.png)

-   <span data-ttu-id="a212c-233">Doppelklicken Sie im Projektmappen-Explorer auf die **"Form1.cs"** Datei, die im Formular im Designer zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="a212c-233">In Solution Explorer, double-click the **Form1.cs** file to open the main form in designer.</span></span>
-   <span data-ttu-id="a212c-234">Wählen Sie die **Kategorie** Data source, und ziehen Sie es auf dem Formular.</span><span class="sxs-lookup"><span data-stu-id="a212c-234">Select the **Category** data source and drag it on the form.</span></span> <span data-ttu-id="a212c-235">Standardmäßig wird ein neues DataGridView-Steuerelement (**CategoryDataGridView**) und Navigation-Symbolleisten-Steuerelemente werden in den Designer hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="a212c-235">By default, a new DataGridView (**categoryDataGridView**) and Navigation toolbar controls are added to the designer.</span></span> <span data-ttu-id="a212c-236">Diese Steuerelemente sind an die Bindungsquelle gebunden (**CategoryBindingSource**) und Bindung Navigator (**CategoryBindingNavigator**) Komponenten, die ebenfalls erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="a212c-236">These controls are bound to the BindingSource (**categoryBindingSource**) and Binding Navigator (**categoryBindingNavigator**) components that are created as well.</span></span>
-   <span data-ttu-id="a212c-237">Die Spalten bearbeiten, auf die **CategoryDataGridView**.</span><span class="sxs-lookup"><span data-stu-id="a212c-237">Edit the columns on the **categoryDataGridView**.</span></span> <span data-ttu-id="a212c-238">Wir möchten, legen Sie die **CategoryId** Spalte in den schreibgeschützten Modus.</span><span class="sxs-lookup"><span data-stu-id="a212c-238">We want to set the **CategoryId** column to read-only.</span></span> <span data-ttu-id="a212c-239">Der Wert für die **CategoryId** -Eigenschaft von der Datenbank generiert wird, nachdem wir die Daten zu speichern.</span><span class="sxs-lookup"><span data-stu-id="a212c-239">The value for the **CategoryId** property is generated by the database after we save the data.</span></span>
    -   <span data-ttu-id="a212c-240">Mit der rechten Maustaste in des DataGridView-Steuerelements, und wählen Sie Spalten bearbeiten...</span><span class="sxs-lookup"><span data-stu-id="a212c-240">Right-click the DataGridView control and select Edit Columns…</span></span>
    -   <span data-ttu-id="a212c-241">Wählen Sie die Kategorie-ID-Spalte und ReadOnly auf True festgelegt.</span><span class="sxs-lookup"><span data-stu-id="a212c-241">Select the CategoryId column and set ReadOnly to True</span></span>
    -   <span data-ttu-id="a212c-242">Klicken Sie auf OK</span><span class="sxs-lookup"><span data-stu-id="a212c-242">Press OK</span></span>
-   <span data-ttu-id="a212c-243">Wählen Sie die Produkte aus der Datenquelle für die Kategorie aus, und ziehen Sie es auf dem Formular.</span><span class="sxs-lookup"><span data-stu-id="a212c-243">Select Products from under the Category data source and drag it on the form.</span></span> <span data-ttu-id="a212c-244">Die ProductDataGridView und ProductBindingSource werden dem Formular hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="a212c-244">The productDataGridView and productBindingSource are added to the form.</span></span>
-   <span data-ttu-id="a212c-245">Bearbeiten Sie die Spalten der ProductDataGridView.</span><span class="sxs-lookup"><span data-stu-id="a212c-245">Edit the columns on the productDataGridView.</span></span> <span data-ttu-id="a212c-246">Wir möchten die CategoryId und Kategorie Spalten ausblenden, und legen "ProductID" in den schreibgeschützten Modus.</span><span class="sxs-lookup"><span data-stu-id="a212c-246">We want to hide the CategoryId and Category columns and set ProductId to read-only.</span></span> <span data-ttu-id="a212c-247">Der Wert für die Eigenschaft "ProductID" wird von der Datenbank generiert, nachdem wir die Daten zu speichern.</span><span class="sxs-lookup"><span data-stu-id="a212c-247">The value for the ProductId property is generated by the database after we save the data.</span></span>
    -   <span data-ttu-id="a212c-248">Mit der rechten Maustaste in des DataGridView-Steuerelements, und wählen Sie **Spalten bearbeiten...** .</span><span class="sxs-lookup"><span data-stu-id="a212c-248">Right-click the DataGridView control and select **Edit Columns...**.</span></span>
    -   <span data-ttu-id="a212c-249">Wählen Sie die **"ProductID"** Spalte mit **ReadOnly** zu **"true"**.</span><span class="sxs-lookup"><span data-stu-id="a212c-249">Select the **ProductId** column and set **ReadOnly** to **True**.</span></span>
    -   <span data-ttu-id="a212c-250">Wählen Sie die **CategoryId** Spalte, und drücken Sie die **entfernen** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="a212c-250">Select the **CategoryId** column and press the **Remove** button.</span></span> <span data-ttu-id="a212c-251">Nicht nur die **Kategorie** Spalte.</span><span class="sxs-lookup"><span data-stu-id="a212c-251">Do the same with the **Category** column.</span></span>
    -   <span data-ttu-id="a212c-252">Drücken Sie **OK**.</span><span class="sxs-lookup"><span data-stu-id="a212c-252">Press **OK**.</span></span>

    <span data-ttu-id="a212c-253">Bisher Serverkomponenten zugeordnet sind wir unsere DataGridView-Steuerelementen BindingSource im Designer.</span><span class="sxs-lookup"><span data-stu-id="a212c-253">So far, we associated our DataGridView controls with BindingSource components in the designer.</span></span> <span data-ttu-id="a212c-254">Im nächsten Abschnitt werden wir die CodeBehind-Code hinzufügen, categoryBindingSource.DataSource auf die Auflistung von Entitäten festgelegt wird, die zurzeit von "DbContext" nachverfolgt werden.</span><span class="sxs-lookup"><span data-stu-id="a212c-254">In the next section we will add code to the code behind to set categoryBindingSource.DataSource to the collection of entities that are currently tracked by DbContext.</span></span> <span data-ttu-id="a212c-255">Wenn wir Produkte gezogen und abgelegt, unter der Kategorie, der WinForms sorgte für die Eigenschaft productsBindingSource.DataSource CategoryBindingSource und productsBindingSource.DataMember-Eigenschaft auf Produkte einrichten.</span><span class="sxs-lookup"><span data-stu-id="a212c-255">When we dragged-and-dropped Products from under the Category, the WinForms took care of setting up the productsBindingSource.DataSource property to categoryBindingSource and productsBindingSource.DataMember property to Products.</span></span> <span data-ttu-id="a212c-256">Aufgrund dieser Bindung werden nur die Produkte, die der derzeit ausgewählten Kategorie gehören in der ProductDataGridView angezeigt.</span><span class="sxs-lookup"><span data-stu-id="a212c-256">Because of this binding, only the products that belong to the currently selected Category will be displayed in the productDataGridView.</span></span>
-   <span data-ttu-id="a212c-257">Aktivieren der **speichern** auf der Symbolleiste Navigation, indem Sie auf der rechten Maustaste und auswählen **aktiviert**.</span><span class="sxs-lookup"><span data-stu-id="a212c-257">Enable the **Save** button on the Navigation toolbar by clicking the right mouse button and selecting **Enabled**.</span></span>

    ![1-Formular-Designer](~/ef6/media/form1-designer.png)

-   <span data-ttu-id="a212c-259">Hinzufügen den Ereignishandler für das Speichern der Schaltfläche durch Doppelklicken auf die Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="a212c-259">Add the event handler for the save button by double-clicking on the button.</span></span> <span data-ttu-id="a212c-260">Den Ereignishandler hinzufügen wird und erhalten Sie eine der CodeBehind für das Formular.</span><span class="sxs-lookup"><span data-stu-id="a212c-260">This will add the event handler and bring you to the code behind for the form.</span></span> <span data-ttu-id="a212c-261">Der Code für die **CategoryBindingNavigatorSaveItem\_klicken Sie auf** -Ereignishandler wird im nächsten Abschnitt hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="a212c-261">The code for the **categoryBindingNavigatorSaveItem\_Click** event handler will be added in the next section.</span></span>

## <a name="add-the-code-that-handles-data-interaction"></a><span data-ttu-id="a212c-262">Fügen Sie den Code, der Dateninteraktion behandelt.</span><span class="sxs-lookup"><span data-stu-id="a212c-262">Add the Code that Handles Data Interaction</span></span>

<span data-ttu-id="a212c-263">Wir fügen nun den Code, um die ProductContext zu verwenden, um die Daten zugreift.</span><span class="sxs-lookup"><span data-stu-id="a212c-263">We'll now add the code to use the ProductContext to perform data access.</span></span> <span data-ttu-id="a212c-264">Aktualisieren Sie den Code für das Hauptformular-Fenster aus, wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="a212c-264">Update the code for the main form window as shown below.</span></span>

<span data-ttu-id="a212c-265">Der Code deklariert eine lang andauernde-Instanz der ProductContext.</span><span class="sxs-lookup"><span data-stu-id="a212c-265">The code declares a long-running instance of ProductContext.</span></span> <span data-ttu-id="a212c-266">Das ProductContext-Objekt wird zum Abfragen und Speichern von Daten in der Datenbank verwendet.</span><span class="sxs-lookup"><span data-stu-id="a212c-266">The ProductContext object is used to query and save data to the database.</span></span> <span data-ttu-id="a212c-267">Die Dispose()-Methode in die ProductContext-Instanz wird dann von der überschriebenen OnClosing-Methode aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="a212c-267">The Dispose() method on the ProductContext instance is then called from the overridden OnClosing method.</span></span> <span data-ttu-id="a212c-268">Die Codekommentare enthalten Details darüber, was der Code bewirkt.</span><span class="sxs-lookup"><span data-stu-id="a212c-268">The code comments provide details about what the code does.</span></span>

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

## <a name="test-the-windows-forms-application"></a><span data-ttu-id="a212c-269">Testen Sie die Windows Forms-Anwendung</span><span class="sxs-lookup"><span data-stu-id="a212c-269">Test the Windows Forms Application</span></span>

-   <span data-ttu-id="a212c-270">Kompilieren und ausführen, die die Funktionalität der Anwendung und testen können.</span><span class="sxs-lookup"><span data-stu-id="a212c-270">Compile and run the application and you can test out the functionality.</span></span>

    ![1 bilden vor dem Speichern](~/ef6/media/form1beforesave.png)

-   <span data-ttu-id="a212c-272">Nach dem Speichern werden die speichern generierter Schlüssel auf dem Bildschirm angezeigt.</span><span class="sxs-lookup"><span data-stu-id="a212c-272">After saving the store generated keys are shown on the screen.</span></span>

    ![Bilden Sie 1 nach dem Speichern](~/ef6/media/form1aftersave.png)

-   <span data-ttu-id="a212c-274">Wenn Sie Code First verwendet, es wird auch angezeigt, eine **WinFormswithEFSample.ProductContext** Datenbank wird für Sie erstellt.</span><span class="sxs-lookup"><span data-stu-id="a212c-274">If you used Code First, then you will also see that a **WinFormswithEFSample.ProductContext** database is created for you.</span></span>

    ![Server-Objekt-Explorer](~/ef6/media/serverobjexplorer.png)
