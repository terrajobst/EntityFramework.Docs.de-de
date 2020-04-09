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
> <span data-ttu-id="5e3d4-102">**Dieses Dokument ist nur für WPF auf .NET Framework gültig.**</span><span class="sxs-lookup"><span data-stu-id="5e3d4-102">**This document is valid for WPF on the .NET Framework only**</span></span>
>
> <span data-ttu-id="5e3d4-103">In diesem Dokument wird die Datenbindung für WPF in .NET Framework beschrieben.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-103">This document describes databinding for WPF on the .NET Framework.</span></span> <span data-ttu-id="5e3d4-104">Für neue .NET Core-Projekte wird empfohlen, [EF Core](/ef/core) anstelle von Entity Framework 6 zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-104">For new .NET Core projects, we recommend you use [EF Core](/ef/core) instead of Entity Framework 6.</span></span> <span data-ttu-id="5e3d4-105">Die Dokumentation zur Datenbindung in EF Core wird unter [Ausgabe #778](https://github.com/dotnet/EntityFramework.Docs/issues/778)nachverfolgt.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-105">The documentation for databinding in EF Core is tracked in [Issue #778](https://github.com/dotnet/EntityFramework.Docs/issues/778).</span></span>

# <a name="databinding-with-wpf"></a><span data-ttu-id="5e3d4-106">Datenbindung mit WPF</span><span class="sxs-lookup"><span data-stu-id="5e3d4-106">Databinding with WPF</span></span>
<span data-ttu-id="5e3d4-107">In dieser schrittweisen exemplarischen Vorgehensweise wird gezeigt, wie POCO-Typen in einem "Master-Detail"-Formular an WPF-Steuerelemente gebunden werden.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-107">This step-by-step walkthrough shows how to bind POCO types to WPF controls in a “master-detail" form.</span></span> <span data-ttu-id="5e3d4-108">Die Anwendung verwendet die Entity Framework-APIs, um Objekte mit Daten aus der Datenbank aufzufüllen, Änderungen nachzuverfolgen und Daten in der Datenbank beizubehalten.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-108">The application uses the Entity Framework APIs to populate objects with data from the database, track changes, and persist data to the database.</span></span>

<span data-ttu-id="5e3d4-109">Das Modell definiert zwei Typen, die an einer 1:n-Beziehung beteiligt\\sind: **Kategorie** (Prinzipalmaster)\\und **Produkt** (abhängiges Detail).</span><span class="sxs-lookup"><span data-stu-id="5e3d4-109">The model defines two types that participate in one-to-many relationship: **Category** (principal\\master) and **Product** (dependent\\detail).</span></span> <span data-ttu-id="5e3d4-110">Anschließend werden die Visual Studio-Tools verwendet, um die im Modell definierten Typen an die WPF-Steuerelemente zu binden.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-110">Then, the Visual Studio tools are used to bind the types defined in the model to the WPF controls.</span></span> <span data-ttu-id="5e3d4-111">Das WPF-Datenbindungsframework ermöglicht die Navigation zwischen verwandten Objekten: Durch das Auswählen von Zeilen in der Masteransicht wird die Detailansicht mit den entsprechenden untergeordneten Daten aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-111">The WPF data-binding framework enables navigation between related objects: selecting rows in the master view causes the detail view to update with the corresponding child data.</span></span>

<span data-ttu-id="5e3d4-112">Die Screenshots und Codeauflistungen in dieser exemplarischen Vorgehensweise stammen aus Visual Studio 2013, Sie können diese exemplarische Vorgehensweise jedoch mit Visual Studio 2012 oder Visual Studio 2010 abschließen.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-112">The screen shots and code listings in this walkthrough are taken from Visual Studio 2013 but you can complete this walkthrough with Visual Studio 2012 or Visual Studio 2010.</span></span>

## <a name="use-the-object-option-for-creating-wpf-data-sources"></a><span data-ttu-id="5e3d4-113">Verwenden der Option 'Objekt' zum Erstellen von WPF-Datenquellen</span><span class="sxs-lookup"><span data-stu-id="5e3d4-113">Use the 'Object' Option for Creating WPF Data Sources</span></span>

<span data-ttu-id="5e3d4-114">Mit der vorherigen Version von Entity Framework haben wir empfohlen, die **Option Datenbank** zu verwenden, wenn eine neue Datenquelle basierend auf einem Modell erstellt wird, das mit dem EF-Designer erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-114">With previous version of Entity Framework we used to recommend using the **Database** option when creating a new Data Source based on a model created with the EF Designer.</span></span> <span data-ttu-id="5e3d4-115">Dies liegt daran, dass der Designer einen Kontext generiert, der von ObjectContext und Entitätsklassen abgeleitet wurde, die von EntityObject abgeleitet wurden.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-115">This was because the designer would generate a context that derived from ObjectContext and entity classes that derived from EntityObject.</span></span> <span data-ttu-id="5e3d4-116">Wenn Sie die Option Datenbank verwenden, können Sie den besten Code für die Interaktion mit dieser API-Oberfläche schreiben.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-116">Using the Database option would help you write the best code for interacting with this API surface.</span></span>

<span data-ttu-id="5e3d4-117">Die EF-Designer für Visual Studio 2012 und Visual Studio 2013 generieren einen Kontext, der von DbContext zusammen mit einfachen POCO-Entitätsklassen abstammt.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-117">The EF Designers for Visual Studio 2012 and Visual Studio 2013 generate a context that derives from DbContext together with simple POCO entity classes.</span></span> <span data-ttu-id="5e3d4-118">Mit Visual Studio 2010 wird empfohlen, eine Codegenerierungsvorlage auszutauschen, die DbContext verwendet, wie weiter unten in dieser exemplarischen Vorgehensweise beschrieben.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-118">With Visual Studio 2010 we recommend swapping to a code generation template that uses DbContext as described later in this walkthrough.</span></span>

<span data-ttu-id="5e3d4-119">Wenn Sie die DbContext-API-Oberfläche verwenden, sollten Sie beim Erstellen einer neuen Datenquelle die Option **Objekt** verwenden, wie in dieser exemplarischen Vorgehensweise gezeigt.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-119">When using the DbContext API surface you should use the **Object** option when creating a new Data Source, as shown in this walkthrough.</span></span>

<span data-ttu-id="5e3d4-120">Bei Bedarf können Sie für Modelle, die mit dem EF-Designer erstellt wurden, [auf ObjectContext-basierte Codegenerierung zurückgreifen.](~/ef6/modeling/designer/codegen/legacy-objectcontext.md)</span><span class="sxs-lookup"><span data-stu-id="5e3d4-120">If needed, you can [revert to ObjectContext based code generation](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) for models created with the EF Designer.</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="5e3d4-121">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="5e3d4-121">Pre-Requisites</span></span>

<span data-ttu-id="5e3d4-122">Sie müssen Visual Studio 2013, Visual Studio 2012 oder Visual Studio 2010 installiert haben, um diese exemplarische Vorgehensweise abzuschließen.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-122">You need to have Visual Studio 2013, Visual Studio 2012 or Visual Studio 2010 installed to complete this walkthrough.</span></span>

<span data-ttu-id="5e3d4-123">Wenn Sie Visual Studio 2010 verwenden, müssen Sie auch NuGet installieren.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-123">If you are using Visual Studio 2010, you also have to install NuGet.</span></span> <span data-ttu-id="5e3d4-124">Weitere Informationen finden Sie unter [Installieren von NuGet](https://docs.microsoft.com/nuget/install-nuget-client-tools).</span><span class="sxs-lookup"><span data-stu-id="5e3d4-124">For more information, see [Installing NuGet](https://docs.microsoft.com/nuget/install-nuget-client-tools).</span></span>  

## <a name="create-the-application"></a><span data-ttu-id="5e3d4-125">Erstellen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="5e3d4-125">Create the Application</span></span>

-   <span data-ttu-id="5e3d4-126">Öffnen Sie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-126">Open Visual Studio</span></span>
-   <span data-ttu-id="5e3d4-127">**Datei&gt; -&gt; Neu - Projekt....**</span><span class="sxs-lookup"><span data-stu-id="5e3d4-127">**File -&gt; New -&gt; Project….**</span></span>
-   <span data-ttu-id="5e3d4-128">Wählen Sie **Windows** im linken Bereich und **WPFApplication** im rechten Bereich aus.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-128">Select **Windows** in the left pane and **WPFApplication** in the right pane</span></span>
-   <span data-ttu-id="5e3d4-129">Geben Sie **WPFwithEFSample** als Namen ein</span><span class="sxs-lookup"><span data-stu-id="5e3d4-129">Enter **WPFwithEFSample** as the name</span></span>
-   <span data-ttu-id="5e3d4-130">Wählen Sie **OK**</span><span class="sxs-lookup"><span data-stu-id="5e3d4-130">Select **OK**</span></span>

## <a name="install-the-entity-framework-nuget-package"></a><span data-ttu-id="5e3d4-131">Installieren des Entity Framework NuGet-Pakets</span><span class="sxs-lookup"><span data-stu-id="5e3d4-131">Install the Entity Framework NuGet package</span></span>

-   <span data-ttu-id="5e3d4-132">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das **WinFormswithEFSample-Projekt**</span><span class="sxs-lookup"><span data-stu-id="5e3d4-132">In Solution Explorer, right-click on the **WinFormswithEFSample** project</span></span>
-   <span data-ttu-id="5e3d4-133">**NuGet-Pakete verwalten...**</span><span class="sxs-lookup"><span data-stu-id="5e3d4-133">Select **Manage NuGet Packages…**</span></span>
-   <span data-ttu-id="5e3d4-134">Wählen Sie im Dialogfeld NuGet-Pakete verwalten die Registerkarte **Online** aus, und wählen Sie das **EntityFramework-Paket** aus.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-134">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package</span></span>
-   <span data-ttu-id="5e3d4-135">Klicken Sie auf **Installieren**</span><span class="sxs-lookup"><span data-stu-id="5e3d4-135">Click **Install**</span></span>  
    >[!NOTE]
    > <span data-ttu-id="5e3d4-136">Zusätzlich zur EntityFramework-Assembly wird auch ein Verweis auf System.ComponentModel.DataAnnotations hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-136">In addition to the EntityFramework assembly a reference to System.ComponentModel.DataAnnotations is also added.</span></span> <span data-ttu-id="5e3d4-137">Wenn das Projekt einen Verweis auf System.Data.Entity enthält, wird es entfernt, wenn das EntityFramework-Paket installiert wird.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-137">If the project has a reference to System.Data.Entity, then it will be removed when the EntityFramework package is installed.</span></span> <span data-ttu-id="5e3d4-138">Die System.Data.Entity-Assembly wird nicht mehr für Entity Framework 6-Anwendungen verwendet.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-138">The System.Data.Entity assembly is no longer used for Entity Framework 6 applications.</span></span>

## <a name="define-a-model"></a><span data-ttu-id="5e3d4-139">Definieren eines Modells</span><span class="sxs-lookup"><span data-stu-id="5e3d4-139">Define a Model</span></span>

<span data-ttu-id="5e3d4-140">In dieser exemplarischen Vorgehensweise können Sie ein Modell mit Code First oder ef Designer implementieren.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-140">In this walkthrough you can chose to implement a model using Code First or the EF Designer.</span></span> <span data-ttu-id="5e3d4-141">Führen Sie einen der beiden folgenden Abschnitte aus.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-141">Complete one of the two following sections.</span></span>

### <a name="option-1-define-a-model-using-code-first"></a><span data-ttu-id="5e3d4-142">Option 1: Definieren eines Modells mit Code First</span><span class="sxs-lookup"><span data-stu-id="5e3d4-142">Option 1: Define a Model using Code First</span></span>

<span data-ttu-id="5e3d4-143">In diesem Abschnitt wird gezeigt, wie Sie ein Modell und die zugehörige Datenbank mit Code First erstellen.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-143">This section shows how to create a model and its associated database using Code First.</span></span> <span data-ttu-id="5e3d4-144">Fahren Sie mit dem nächsten Abschnitt fort **(Option 2: Definieren eines Modells mit Database First),** wenn Sie lieber Database First verwenden, um Ihr Modell mithilfe des EF-Designers aus einer Datenbank zurückzuentwickeln.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-144">Skip to the next section (**Option 2: Define a model using Database First)** if you would rather use Database First to reverse engineer your model from a database using the EF designer</span></span>

<span data-ttu-id="5e3d4-145">Wenn Sie Code First-Entwicklung verwenden, beginnen Sie in der Regel mit dem Schreiben von .NET Framework-Klassen, die Ihr konzeptionelles (Domänen-)Modell definieren.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-145">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span>

-   <span data-ttu-id="5e3d4-146">Fügen Sie dem **WPFwithEFSample** eine neue Klasse hinzu:</span><span class="sxs-lookup"><span data-stu-id="5e3d4-146">Add a new class to the **WPFwithEFSample:**</span></span>
    -   <span data-ttu-id="5e3d4-147">Rechtsklick auf den Projektnamen</span><span class="sxs-lookup"><span data-stu-id="5e3d4-147">Right-click on the project name</span></span>
    -   <span data-ttu-id="5e3d4-148">Wählen **Sie Hinzufügen**, dann Neues **Element**</span><span class="sxs-lookup"><span data-stu-id="5e3d4-148">Select **Add**, then **New Item**</span></span>
    -   <span data-ttu-id="5e3d4-149">Klasse **Class** auswählen und **Produkt** für den Klassennamen eingeben</span><span class="sxs-lookup"><span data-stu-id="5e3d4-149">Select **Class** and enter **Product** for the class name</span></span>
-   <span data-ttu-id="5e3d4-150">Ersetzen Sie die **Produktklassendefinition** durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="5e3d4-150">Replace the **Product** class definition with the following code:</span></span>

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

<span data-ttu-id="5e3d4-151">Die **Products-Eigenschaft** für die **Kategorieklasse** und **die Kategorie-Eigenschaft** in der **Produktklasse** sind Navigationseigenschaften.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-151">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="5e3d4-152">In Entity Framework bieten Navigationseigenschaften eine Möglichkeit, eine Beziehung zwischen zwei Entitätstypen zu navigieren.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-152">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="5e3d4-153">Zusätzlich zum Definieren von Entitäten müssen Sie eine Klasse definieren, die&lt;von&gt; DbContext abstammt und DbSet TEntity-Eigenschaften verfügbar macht.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-153">In addition to defining entities, you need to define a class that derives from DbContext and exposes DbSet&lt;TEntity&gt; properties.</span></span> <span data-ttu-id="5e3d4-154">Die DbSet&lt;&gt; TEntity-Eigenschaften informieren den Kontext darüber, welche Typen sie in das Modell aufnehmen möchten.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-154">The DbSet&lt;TEntity&gt; properties let the context know which types you want to include in the model.</span></span>

<span data-ttu-id="5e3d4-155">Eine Instanz des abgeleiteten DbContext-Typs verwaltet die Entitätsobjekte während der Laufzeit, die das Auffüllen von Objekten mit Daten aus einer Datenbank, die Änderungsnachverfolgung und das Beibehalten von Daten in der Datenbank umfasst.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-155">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

-   <span data-ttu-id="5e3d4-156">Fügen Sie dem Projekt eine neue **ProductContext-Klasse** mit der folgenden Definition hinzu:</span><span class="sxs-lookup"><span data-stu-id="5e3d4-156">Add a new **ProductContext** class to the project with the following definition:</span></span>

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

<span data-ttu-id="5e3d4-157">Kompilieren Sie das Projekt.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-157">Compile the project.</span></span>

### <a name="option-2-define-a-model-using-database-first"></a><span data-ttu-id="5e3d4-158">Option 2: Definieren eines Modells mithilfe von Database First</span><span class="sxs-lookup"><span data-stu-id="5e3d4-158">Option 2: Define a model using Database First</span></span>

<span data-ttu-id="5e3d4-159">In diesem Abschnitt wird gezeigt, wie Sie Database First verwenden, um Ihr Modell mithilfe des EF-Designers aus einer Datenbank zurückzuentwickeln.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-159">This section shows how to use Database First to reverse engineer your model from a database using the EF designer.</span></span> <span data-ttu-id="5e3d4-160">Wenn Sie den vorherigen Abschnitt abgeschlossen haben **(Option 1: Definieren eines Modells mit Code First),** überspringen Sie diesen Abschnitt, und gehen Sie direkt zum Abschnitt **Lazy Loading.**</span><span class="sxs-lookup"><span data-stu-id="5e3d4-160">If you completed the previous section (**Option 1: Define a model using Code First)**, then skip this section and go straight to the **Lazy Loading** section.</span></span>

#### <a name="create-an-existing-database"></a><span data-ttu-id="5e3d4-161">Erstellen einer vorhandenen Datenbank</span><span class="sxs-lookup"><span data-stu-id="5e3d4-161">Create an Existing Database</span></span>

<span data-ttu-id="5e3d4-162">Wenn Sie auf eine vorhandene Datenbank abzielen, wird sie in der Regel bereits erstellt, aber für diese exemplarische Vorgehensweise müssen wir eine Datenbank für den Zugriff erstellen.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-162">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="5e3d4-163">Der Datenbankserver, der mit Visual Studio installiert wird, unterscheidet sich je nach installierter Version von Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="5e3d4-163">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="5e3d4-164">Wenn Sie Visual Studio 2010 verwenden, erstellen Sie eine SQL Express-Datenbank.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-164">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="5e3d4-165">Wenn Sie Visual Studio 2012 verwenden, erstellen Sie eine [LocalDB-Datenbank.](https://msdn.microsoft.com/library/hh510202.aspx)</span><span class="sxs-lookup"><span data-stu-id="5e3d4-165">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) database.</span></span>

<span data-ttu-id="5e3d4-166">Lassen Sie uns fortfahren und die Datenbank generieren.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-166">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="5e3d4-167">**Ansicht&gt; - Server-Explorer**</span><span class="sxs-lookup"><span data-stu-id="5e3d4-167">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="5e3d4-168">Rechtsklick auf **Datenverbindungen -&gt; Verbindung hinzufügen...**</span><span class="sxs-lookup"><span data-stu-id="5e3d4-168">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="5e3d4-169">Wenn Sie keine Verbindung mit einer Datenbank aus Server Explorer hergestellt haben, müssen Sie Microsoft SQL Server als Datenquelle auswählen.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-169">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![Datenquelle ändern](~/ef6/media/changedatasource.png)

-   <span data-ttu-id="5e3d4-171">Stellen Sie eine Verbindung mit LocalDB oder SQL Express her, je nachdem, welche installiert wurde, und geben Sie **Produkte** als Datenbanknamen ein.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-171">Connect to either LocalDB or SQL Express, depending on which one you have installed, and enter **Products** as the database name</span></span>

    ![Hinzufügen von Connection LocalDB](~/ef6/media/addconnectionlocaldb.png)

    ![Hinzufügen von Connection Express](~/ef6/media/addconnectionexpress.png)

-   <span data-ttu-id="5e3d4-174">Wählen Sie **OK** und Sie werden gefragt, ob Sie eine neue Datenbank erstellen möchten, wählen Sie **Ja**</span><span class="sxs-lookup"><span data-stu-id="5e3d4-174">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![Erstellen einer Datenbank](~/ef6/media/createdatabase.png)

-   <span data-ttu-id="5e3d4-176">Die neue Datenbank wird nun im Server Explorer angezeigt, mit der rechten Maustaste darauf geklickt und **Neue Abfrage** ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-176">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="5e3d4-177">Kopieren Sie die folgende SQL-Abfrage in die neue Abfrage, klicken Sie dann mit der rechten Maustaste auf die Abfrage, und wählen Sie **Ausführen aus.**</span><span class="sxs-lookup"><span data-stu-id="5e3d4-177">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

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

#### <a name="reverse-engineer-model"></a><span data-ttu-id="5e3d4-178">Reverse Engineering-Modell</span><span class="sxs-lookup"><span data-stu-id="5e3d4-178">Reverse Engineer Model</span></span>

<span data-ttu-id="5e3d4-179">Wir verwenden Entity Framework Designer, der als Teil von Visual Studio enthalten ist, um unser Modell zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-179">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="5e3d4-180">**Projekt&gt; - Neues Element hinzufügen...**</span><span class="sxs-lookup"><span data-stu-id="5e3d4-180">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="5e3d4-181">Wählen Sie **Daten** aus dem linken Menü aus, und **ADO.NET Entitätsdatenmodell**</span><span class="sxs-lookup"><span data-stu-id="5e3d4-181">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="5e3d4-182">Geben Sie **ProductModel** als Namen ein und klicken Sie auf **OK**</span><span class="sxs-lookup"><span data-stu-id="5e3d4-182">Enter **ProductModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="5e3d4-183">Dadurch wird der **Entitätsdatenmodell-Assistent** gestartet.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-183">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="5e3d4-184">Wählen **Sie Aus Datenbank generieren** aus, und klicken Sie auf **Weiter**</span><span class="sxs-lookup"><span data-stu-id="5e3d4-184">Select **Generate from Database** and click **Next**</span></span>

    ![Auswählen von Modellinhalten](~/ef6/media/choosemodelcontents.png)

-   <span data-ttu-id="5e3d4-186">Wählen Sie die Verbindung zur Datenbank aus, die Sie im ersten Abschnitt erstellt haben, geben Sie **ProductContext** als Namen der Verbindungszeichenfolge ein, und klicken Sie auf **Weiter.**</span><span class="sxs-lookup"><span data-stu-id="5e3d4-186">Select the connection to the database you created in the first section, enter **ProductContext** as the name of the connection string and click **Next**</span></span>

    ![Wählen Sie Ihre Verbindung](~/ef6/media/chooseyourconnection.png)

-   <span data-ttu-id="5e3d4-188">Klicken Sie auf das Kontrollkästchen neben "Tabellen", um alle Tabellen zu importieren, und klicken Sie auf "Fertig stellen".</span><span class="sxs-lookup"><span data-stu-id="5e3d4-188">Click the checkbox next to ‘Tables’ to import all tables and click ‘Finish’</span></span>

    ![Wählen Sie Ihre Objekte](~/ef6/media/chooseyourobjects.png)

<span data-ttu-id="5e3d4-190">Sobald der Reverse Engineering-Prozess abgeschlossen ist, wird das neue Modell zu Ihrem Projekt hinzugefügt und für die Anzeige im Entity Framework Designer geöffnet.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-190">Once the reverse engineer process completes the new model is added to your project and opened up for you to view in the Entity Framework Designer.</span></span> <span data-ttu-id="5e3d4-191">Außerdem wurde Ihrem Projekt eine App.config-Datei mit den Verbindungsdetails für die Datenbank hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-191">An App.config file has also been added to your project with the connection details for the database.</span></span>

#### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="5e3d4-192">Weitere Schritte in Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="5e3d4-192">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="5e3d4-193">Wenn Sie in Visual Studio 2010 arbeiten, müssen Sie den EF-Designer aktualisieren, um die EF6-Codegenerierung zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-193">If you are working in Visual Studio 2010 then you will need to update the EF designer to use EF6 code generation.</span></span>

-   <span data-ttu-id="5e3d4-194">Klicken Sie mit der rechten Maustaste auf eine leere Stelle Ihres Modells im EF-Designer und wählen Sie **Codegenerierungselement hinzufügen...**</span><span class="sxs-lookup"><span data-stu-id="5e3d4-194">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="5e3d4-195">Wählen Sie **Online-Vorlagen** aus dem linken Menü und suchen Sie nach **DbContext**</span><span class="sxs-lookup"><span data-stu-id="5e3d4-195">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="5e3d4-196">Wählen Sie den **EF 6.x\#DbContext Generator für C** , geben Sie **ProductsModel** als Namen ein und klicken Sie auf Hinzufügen</span><span class="sxs-lookup"><span data-stu-id="5e3d4-196">Select the **EF 6.x DbContext Generator for C\#,** enter **ProductsModel** as the name and click Add</span></span>

#### <a name="updating-code-generation-for-data-binding"></a><span data-ttu-id="5e3d4-197">Aktualisieren der Codegenerierung für die Datenbindung</span><span class="sxs-lookup"><span data-stu-id="5e3d4-197">Updating code generation for data binding</span></span>

<span data-ttu-id="5e3d4-198">EF generiert Code aus Ihrem Modell mithilfe von T4-Vorlagen.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-198">EF generates code from your model using T4 templates.</span></span> <span data-ttu-id="5e3d4-199">Die Vorlagen, die mit Visual Studio ausgeliefert oder aus dem Visual Studio-Katalog heruntergeladen wurden, sind für den allgemeinen Gebrauch vorgesehen.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-199">The templates shipped with Visual Studio or downloaded from the Visual Studio gallery are intended for general purpose use.</span></span> <span data-ttu-id="5e3d4-200">Dies bedeutet, dass die aus diesen&lt;Vorlagen&gt; generierten Entitäten über einfache ICollection T-Eigenschaften verfügen.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-200">This means that the entities generated from these templates have simple ICollection&lt;T&gt; properties.</span></span> <span data-ttu-id="5e3d4-201">Beim Festlegen der Datenbindung mit WPF ist es jedoch wünschenswert, **ObservableCollection** für Auflistungseigenschaften zu verwenden, damit WPF die an den Auflistungen vorgenommenen Änderungen nachverfolgen kann.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-201">However, when doing data binding using WPF it is desirable to use **ObservableCollection** for collection properties so that WPF can keep track of changes made to the collections.</span></span> <span data-ttu-id="5e3d4-202">Zu diesem Zweck werden wir die Vorlagen so ändern, dass sie ObservableCollection verwenden.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-202">To this end we will to modify the templates to use ObservableCollection.</span></span>

-   <span data-ttu-id="5e3d4-203">Öffnen Sie den **Projektmappen-Explorer,** und suchen Sie die **Datei ProductModel.edmx**</span><span class="sxs-lookup"><span data-stu-id="5e3d4-203">Open the **Solution Explorer** and find **ProductModel.edmx** file</span></span>
-   <span data-ttu-id="5e3d4-204">Suchen Sie die **ProductModel.tt** Datei, die unter der Datei ProductModel.edmx verschachtelt wird.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-204">Find the **ProductModel.tt** file which will be nested under the ProductModel.edmx file</span></span>

    ![WPF-Produktmodellvorlage](~/ef6/media/wpfproductmodeltemplate.png)

-   <span data-ttu-id="5e3d4-206">Doppelklicken Sie auf die ProductModel.tt Datei, um sie im Visual Studio-Editor zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-206">Double-click on the ProductModel.tt file to open it in the Visual Studio editor</span></span>
-   <span data-ttu-id="5e3d4-207">Suchen und ersetzen Sie die beiden Vorkommen von "**ICollection**" durch "**ObservableCollection**".</span><span class="sxs-lookup"><span data-stu-id="5e3d4-207">Find and replace the two occurrences of “**ICollection**” with “**ObservableCollection**”.</span></span> <span data-ttu-id="5e3d4-208">Diese befinden sich etwa an den Linien 296 und 484.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-208">These are located approximately at lines 296 and 484.</span></span>
-   <span data-ttu-id="5e3d4-209">Suchen und ersetzen Sie das erste Vorkommen von "**HashSet**" durch "**ObservableCollection**".</span><span class="sxs-lookup"><span data-stu-id="5e3d4-209">Find and replace the first occurrence of “**HashSet**” with “**ObservableCollection**”.</span></span> <span data-ttu-id="5e3d4-210">Dieses Vorkommen befindet sich ungefähr in Zeile 50.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-210">This occurrence is located approximately at line 50.</span></span> <span data-ttu-id="5e3d4-211">**Ersetzen Sie nicht** das zweite Vorkommen von HashSet, das später im Code gefunden wurde.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-211">**Do not** replace the second occurrence of HashSet found later in the code.</span></span>
-   <span data-ttu-id="5e3d4-212">Suchen und ersetzen Sie das einzige Vorkommen von "**System.Collections.Generic**" mit "**System.Collections.ObjectModel**".</span><span class="sxs-lookup"><span data-stu-id="5e3d4-212">Find and replace the only occurrence of “**System.Collections.Generic**” with “**System.Collections.ObjectModel**”.</span></span> <span data-ttu-id="5e3d4-213">Dieser befindet sich ungefähr an der Linie 424.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-213">This is located approximately at line 424.</span></span>
-   <span data-ttu-id="5e3d4-214">Speichern Sie die ProductModel.tt Datei.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-214">Save the ProductModel.tt file.</span></span> <span data-ttu-id="5e3d4-215">Dies sollte dazu führen, dass der Code für Entitäten neu generiert wird.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-215">This should cause the code for entities to be regenerated.</span></span> <span data-ttu-id="5e3d4-216">Wenn sich der Code nicht automatisch regeneriert, klicken Sie mit der rechten Maustaste auf ProductModel.tt und wählen Sie "Custom Tool ausführen".</span><span class="sxs-lookup"><span data-stu-id="5e3d4-216">If the code does not regenerate automatically, then right click on ProductModel.tt and choose “Run Custom Tool”.</span></span>

<span data-ttu-id="5e3d4-217">Wenn Sie nun die Category.cs Datei öffnen (die unter ProductModel.tt verschachtelt ist), sollten Sie sehen, dass die Produkte-Auflistung den Typ **ObservableCollection&lt;&gt;Product**hat.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-217">If you now open the Category.cs file (which is nested under ProductModel.tt) then you should see that the Products collection has the type **ObservableCollection&lt;Product&gt;**.</span></span>

<span data-ttu-id="5e3d4-218">Kompilieren Sie das Projekt.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-218">Compile the project.</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="5e3d4-219">Lazy Loading</span><span class="sxs-lookup"><span data-stu-id="5e3d4-219">Lazy Loading</span></span>

<span data-ttu-id="5e3d4-220">Die **Products-Eigenschaft** für die **Kategorieklasse** und **die Kategorie-Eigenschaft** in der **Produktklasse** sind Navigationseigenschaften.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-220">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="5e3d4-221">In Entity Framework bieten Navigationseigenschaften eine Möglichkeit, eine Beziehung zwischen zwei Entitätstypen zu navigieren.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-221">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="5e3d4-222">EF bietet Ihnen die Möglichkeit, verwandte Entitäten automatisch aus der Datenbank zu laden, wenn Sie zum ersten Mal auf die Navigationseigenschaft zugreifen.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-222">EF gives you an option of loading related entities from the database automatically the first time you access the navigation property.</span></span> <span data-ttu-id="5e3d4-223">Beachten Sie bei dieser Art des Ladens (als "Lazy Loading" bezeichnet), dass beim ersten Zugriff auf jede Navigationseigenschaft eine separate Abfrage für die Datenbank ausgeführt wird, wenn sich der Inhalt nicht bereits im Kontext befindet.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-223">With this type of loading (called lazy loading), be aware that the first time you access each navigation property a separate query will be executed against the database if the contents are not already in the context.</span></span>

<span data-ttu-id="5e3d4-224">Bei der Verwendung von POCO-Entitätstypen erreicht EF das verzögerte Laden, indem Instanzen abgeleiteter Proxytypen während der Laufzeit erstellt und dann virtuelle Eigenschaften in Ihren Klassen überschreiben, um den Lade-Hook hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-224">When using POCO entity types, EF achieves lazy loading by creating instances of derived proxy types during runtime and then overriding virtual properties in your classes to add the loading hook.</span></span> <span data-ttu-id="5e3d4-225">Um das verzögerte Laden verwandter Objekte zu erhalten, müssen Sie Navigationseigenschaften-Getter als **öffentlich** und **virtuell** deklarieren **(überschreibbar** in Visual Basic), und Ihre Klasse darf nicht **versiegelt** werden **(NotOverridable** in Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="5e3d4-225">To get lazy loading of related objects, you must declare navigation property getters as **public** and **virtual** (**Overridable** in Visual Basic), and you class must not be **sealed** (**NotOverridable** in Visual Basic).</span></span> <span data-ttu-id="5e3d4-226">Bei Verwendung von Database First werden Navigationseigenschaften automatisch virtuell gemacht, um das verzögerte Laden zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-226">When using Database First navigation properties are automatically made virtual to enable lazy loading.</span></span> <span data-ttu-id="5e3d4-227">Im Abschnitt Code First haben wir uns entschieden, die Navigationseigenschaften aus dem gleichen Grund virtuell zu machen</span><span class="sxs-lookup"><span data-stu-id="5e3d4-227">In the Code First section we chose to make the navigation properties virtual for the same reason</span></span>

## <a name="bind-object-to-controls"></a><span data-ttu-id="5e3d4-228">Binden von Objekt an Steuerelemente</span><span class="sxs-lookup"><span data-stu-id="5e3d4-228">Bind Object to Controls</span></span>

<span data-ttu-id="5e3d4-229">Fügen Sie die Klassen hinzu, die im Modell als Datenquellen für diese WPF-Anwendung definiert sind.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-229">Add the classes that are defined in the model as data sources for this WPF application.</span></span>

-   <span data-ttu-id="5e3d4-230">Doppelklicken Sie im Projektmappen-Explorer auf **MainWindow.xaml,** um das Hauptformular zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-230">Double-click **MainWindow.xaml** in Solution Explorer to open the main form</span></span>
-   <span data-ttu-id="5e3d4-231">Wählen Sie im Hauptmenü **Projekt -&gt; Neue Datenquelle hinzufügen ...**</span><span class="sxs-lookup"><span data-stu-id="5e3d4-231">From the main menu, select **Project -&gt; Add New Data Source …**</span></span>
    <span data-ttu-id="5e3d4-232">(In Visual Studio 2010 müssen Sie Daten auswählen **-&gt; Neue Datenquelle hinzufügen...**)</span><span class="sxs-lookup"><span data-stu-id="5e3d4-232">(in Visual Studio 2010, you need to select **Data -&gt; Add New Data Source…**)</span></span>
-   <span data-ttu-id="5e3d4-233">Wählen Sie im Fenster Datenquellentyp auswählen **Objekt** aus, und klicken Sie auf **Weiter**</span><span class="sxs-lookup"><span data-stu-id="5e3d4-233">In the Choose a Data Source Typewindow, select **Object** and click **Next**</span></span>
-   <span data-ttu-id="5e3d4-234">Im Dialog "Datenobjekte auswählen" das **WPFwithEFSample** zweimal aufundschließen und **Kategorie**</span><span class="sxs-lookup"><span data-stu-id="5e3d4-234">In the Select the Data Objects dialog, unfold the **WPFwithEFSample** two times and select **Category**</span></span>  
    <span data-ttu-id="5e3d4-235">*Es ist nicht erforderlich, die **Produktdatenquelle** auszuwählen, da wir über die Eigenschaft des **Produkts**in der **Kategorie-Datenquelle** darauf gelangen.*</span><span class="sxs-lookup"><span data-stu-id="5e3d4-235">*There is no need to select the **Product** data source, because we will get to it through the **Product**’s property on the **Category** data source*</span></span>  

    ![Datenobjekte auswählen](~/ef6/media/selectdataobjects.png)

-   <span data-ttu-id="5e3d4-237">Klicken Sie auf **Fertig stellen**.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-237">Click **Finish.**</span></span>
-   <span data-ttu-id="5e3d4-238">Das Fenster Datenquellen wird neben dem Fenster MainWindow.xaml geöffnet \*Wenn das Fenster Datenquellen nicht angezeigt wird, wählen Sie \*\*Ansicht -&gt; Andere Windows-Datenquellen&gt; \*\* \* aus.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-238">The Data Sources window is opened next to the MainWindow.xaml window *If the Data Sources window is not showing up, select **View -&gt; Other Windows-&gt; Data Sources***</span></span>
-   <span data-ttu-id="5e3d4-239">Drücken Sie das Pin-Symbol, damit das Fenster Datenquellen nicht automatisch ausgeblendet wird.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-239">Press the pin icon, so the Data Sources window does not auto hide.</span></span> <span data-ttu-id="5e3d4-240">Möglicherweise müssen Sie auf die Schaltfläche "Aktualisieren" klicken, wenn das Fenster bereits sichtbar war.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-240">You may need to hit the refresh button if the window was already visible.</span></span>

    ![Projektmappen-Explorer](~/ef6/media/datasources.png)

-   <span data-ttu-id="5e3d4-242">Wählen Sie die **Kategorie-Datenquelle** aus, und ziehen Sie sie auf das Formular.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-242">Select the **Category** data source and drag it on the form.</span></span>

<span data-ttu-id="5e3d4-243">Das Folgende ist passiert, als wir diese Quelle gezogen haben:</span><span class="sxs-lookup"><span data-stu-id="5e3d4-243">The following happened when we dragged this source:</span></span>

-   <span data-ttu-id="5e3d4-244">Die **resourceViewSource-Ressource** und das **categoryDataGrid-Steuerelement** wurden XAML hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-244">The **categoryViewSource** resource and the **categoryDataGrid** control were added to XAML</span></span> 
-   <span data-ttu-id="5e3d4-245">Die DataContext-Eigenschaft für das übergeordnete Grid-Element wurde auf "-StaticResource **categoryViewSource"** festgelegt.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-245">The DataContext property on the parent Grid element was set to "{StaticResource **categoryViewSource** }".</span></span><span data-ttu-id="5e3d4-246">Die **categoryViewSource-Ressource** dient als Bindungsquelle für das äußere\\übergeordnete Grid-Element.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-246"> The **categoryViewSource** resource serves as a binding source for the outer\\parent Grid element.</span></span> <span data-ttu-id="5e3d4-247">Die inneren Grid-Elemente erben dann den DataContext-Wert vom übergeordneten Grid (die ItemsSource-Eigenschaft von categoryDataGrid ist auf "-Bindung" festgelegt).</span><span class="sxs-lookup"><span data-stu-id="5e3d4-247">The inner Grid elements then inherit the DataContext value from the parent Grid (the categoryDataGrid’s ItemsSource property is set to "{Binding}")</span></span>

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

## <a name="adding-a-details-grid"></a><span data-ttu-id="5e3d4-248">Hinzufügen eines Details-Rasters</span><span class="sxs-lookup"><span data-stu-id="5e3d4-248">Adding a Details Grid</span></span>

<span data-ttu-id="5e3d4-249">Nun, da wir ein Raster zum Anzeigen von Kategorien haben, fügen wir ein Detailraster hinzu, um die zugehörigen Produkte anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-249">Now that we have a grid to display Categories let's add a details grid to display the associated Products.</span></span>

-   <span data-ttu-id="5e3d4-250">Wählen Sie die **Products-Eigenschaft** unter der **Kategorie-Datenquelle** aus, und ziehen Sie sie auf das Formular.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-250">Select the **Products** property from under the **Category** data source and drag it on the form.</span></span>
    -   <span data-ttu-id="5e3d4-251">Die **KategorieProductsViewSource-Ressource** und **das ProductDataGrid-Raster** werden XAML hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-251">The **categoryProductsViewSource** resource and **productDataGrid** grid are added to XAML</span></span>
    -   <span data-ttu-id="5e3d4-252">Der Bindungspfad für diese Ressource ist auf Produkte festgelegt.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-252">The binding path for this resource is set to Products</span></span>
    -   <span data-ttu-id="5e3d4-253">Das WPF-Datenbindungsframework stellt sicher, dass nur Produkte, die sich auf die ausgewählte Kategorie beziehen, in **productDataGrid** angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-253">WPF data-binding framework ensures that only Products related to the selected Category show up in **productDataGrid**</span></span>
-   <span data-ttu-id="5e3d4-254">Ziehen Sie in der Toolbox die **Schaltfläche** auf das Formular.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-254">From the Toolbox, drag **Button** on to the form.</span></span> <span data-ttu-id="5e3d4-255">Legen Sie die **Name-Eigenschaft** auf **buttonSave** und die **Content-Eigenschaft** auf **Speichern**fest.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-255">Set the **Name** property to **buttonSave** and the **Content** property to **Save**.</span></span>

<span data-ttu-id="5e3d4-256">Das Formular sollte wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="5e3d4-256">The form should look similar to this:</span></span>

![Designer](~/ef6/media/designer.png) 

## <a name="add-code-that-handles-data-interaction"></a><span data-ttu-id="5e3d4-258">Code hinzufügen, der die Dateninteraktion behandelt</span><span class="sxs-lookup"><span data-stu-id="5e3d4-258">Add Code that Handles Data Interaction</span></span>

<span data-ttu-id="5e3d4-259">Es ist an der Zeit, dem Hauptfenster einige Ereignishandler hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-259">It's time to add some event handlers to the main window.</span></span>

-   <span data-ttu-id="5e3d4-260">Klicken Sie im XAML-Fenster auf das \*\* &lt;Fensterelement,\*\* das das Hauptfenster</span><span class="sxs-lookup"><span data-stu-id="5e3d4-260">In the XAML window, click on the **&lt;Window** element, this selects the main window</span></span>
-   <span data-ttu-id="5e3d4-261">Wählen Sie im **Eigenschaftenfenster** **Ereignisse** oben rechts aus, und doppelklicken Sie dann auf das Textfeld rechts neben der **Beschriftung geladen**</span><span class="sxs-lookup"><span data-stu-id="5e3d4-261">In the **Properties** window choose **Events** at the top right, then double-click the text box to right of the **Loaded** label</span></span>

    ![Eigenschaften des Hauptfensters](~/ef6/media/mainwindowproperties.png)

-   <span data-ttu-id="5e3d4-263">Fügen Sie auch das **Click-Ereignis** für die **Schaltfläche Speichern** hinzu, indem Sie im Designer auf die Schaltfläche Speichern doppelklicken.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-263">Also add the **Click** event for the **Save** button by double-clicking the Save button in the designer.</span></span> 

<span data-ttu-id="5e3d4-264">Dadurch gelangen Sie zum Code dahinter für das Formular, wir bearbeiten nun den Code, um den ProductContext zum Ausführen des Datenzugriffs zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-264">This brings you to the code behind for the form, we'll now edit the code to use the ProductContext to perform data access.</span></span> <span data-ttu-id="5e3d4-265">Aktualisieren Sie den Code für das MainWindow wie unten gezeigt.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-265">Update the code for the MainWindow as shown below.</span></span>

<span data-ttu-id="5e3d4-266">Der Code deklariert eine lang andauernde Instanz von **ProductContext**.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-266">The code declares a long-running instance of **ProductContext**.</span></span> <span data-ttu-id="5e3d4-267">Das **ProductContext-Objekt** wird zum Abfragen und Speichern von Daten in der Datenbank verwendet.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-267">The **ProductContext** object is used to query and save data to the database.</span></span> <span data-ttu-id="5e3d4-268">Die **Dispose()** auf der **ProductContext-Instanz** wird dann von der überschriebenen **OnClosing-Methode** aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-268">The **Dispose()** on the **ProductContext** instance is then called from the overridden **OnClosing** method.</span></span><span data-ttu-id="5e3d4-269">Die Codekommentare enthalten Details dazu, was der Code tut.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-269"> The code comments provide details about what the code does.</span></span>

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

## <a name="test-the-wpf-application"></a><span data-ttu-id="5e3d4-270">Testen der WPF-Anwendung</span><span class="sxs-lookup"><span data-stu-id="5e3d4-270">Test the WPF Application</span></span>

-   <span data-ttu-id="5e3d4-271">Kompilieren Sie die Anwendung, und führen Sie sie aus.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-271">Compile and run the application.</span></span> <span data-ttu-id="5e3d4-272">Wenn Sie Code First verwendet haben, werden Sie feststellen, dass eine **WPFwithEFSample.ProductContext-Datenbank** für Sie erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-272">If you used Code First, then you will see that a **WPFwithEFSample.ProductContext** database is created for you.</span></span>
-   <span data-ttu-id="5e3d4-273">Geben Sie einen Kategorienamen im oberen Raster und Die Produktnamen im unteren Raster *ein, geben Sie nichts in ID-Spalten ein, da der Primärschlüssel von der Datenbank generiert wird.*</span><span class="sxs-lookup"><span data-stu-id="5e3d4-273">Enter a category name in the top grid and product names in the bottom grid *Do not enter anything in ID columns, because the primary key is generated by the database*</span></span>

    ![Hauptfenster mit neuen Kategorien und Produkten](~/ef6/media/screen1.png)

-   <span data-ttu-id="5e3d4-275">Drücken Sie die **Schaltfläche Speichern,** um die Daten in der Datenbank zu speichern</span><span class="sxs-lookup"><span data-stu-id="5e3d4-275">Press the **Save** button to save the data to the database</span></span>

<span data-ttu-id="5e3d4-276">Nach dem Aufruf von **DbContexts SaveChanges()** werden die IDs mit den von der Datenbank generierten Werten aufgefüllt.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-276">After the call to DbContext’s **SaveChanges()**, the IDs are populated with the database generated values.</span></span> <span data-ttu-id="5e3d4-277">Da wir **Refresh()** nach **SaveChanges()** aufgerufen haben, werden die **DataGrid-Steuerelemente** auch mit den neuen Werten aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-277">Because we called **Refresh()** after **SaveChanges()** the **DataGrid** controls are updated with the new values as well.</span></span>

![Hauptfenster mit bestückten IDs](~/ef6/media/screen2.png)

## <a name="additional-resources"></a><span data-ttu-id="5e3d4-279">Weitere Ressourcen</span><span class="sxs-lookup"><span data-stu-id="5e3d4-279">Additional Resources</span></span>

<span data-ttu-id="5e3d4-280">Weitere Informationen zur Datenbindung an Sammlungen mit WPF finden Sie in [diesem Thema](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-overview#binding-to-collections) in der WPF-Dokumentation.</span><span class="sxs-lookup"><span data-stu-id="5e3d4-280">To learn more about data binding to collections using WPF, see [this topic](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-overview#binding-to-collections) in the WPF documentation.</span></span>  
