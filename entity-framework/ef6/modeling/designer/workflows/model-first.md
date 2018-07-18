---
title: Model First – EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: e1b9c319-bb8a-4417-ac94-7890f257e7f6
caps.latest.revision: 3
ms.openlocfilehash: e7876776ed0dee764d5ced97b863a3580e02e20b
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2018
ms.locfileid: "39121111"
---
# <a name="model-first"></a><span data-ttu-id="e3201-102">Model First</span><span class="sxs-lookup"><span data-stu-id="e3201-102">Model First</span></span>
<span data-ttu-id="e3201-103">Dieses video und schrittweise exemplarische Vorgehensweise bieten eine Einführung in die Verwendung von Entity Framework Model First-Entwicklung.</span><span class="sxs-lookup"><span data-stu-id="e3201-103">This video and step-by-step walkthrough provide an introduction to Model First development using Entity Framework.</span></span> <span data-ttu-id="e3201-104">Modell zuerst können Sie ein neues Modell mit dem Entity Framework Designer erstellen und anschließend ein Datenbankschema aus dem Modell generieren.</span><span class="sxs-lookup"><span data-stu-id="e3201-104">Model First allows you to create a new model using the Entity Framework Designer and then generate a database schema from the model.</span></span> <span data-ttu-id="e3201-105">Das Modell kann im Entity Framework Designer wird in einer EDMX-Datei (EDMX-Erweiterung) gespeichert und angezeigt und bearbeitet werden.</span><span class="sxs-lookup"><span data-stu-id="e3201-105">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="e3201-106">Die Klassen, die Sie in Ihrer Anwendung interagieren, werden automatisch aus der EDMX-Datei generiert.</span><span class="sxs-lookup"><span data-stu-id="e3201-106">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="e3201-107">Video ansehen</span><span class="sxs-lookup"><span data-stu-id="e3201-107">Watch the video</span></span>
<span data-ttu-id="e3201-108">Dieses video und schrittweise exemplarische Vorgehensweise bieten eine Einführung in die Verwendung von Entity Framework Model First-Entwicklung.</span><span class="sxs-lookup"><span data-stu-id="e3201-108">This video and step-by-step walkthrough provide an introduction to Model First development using Entity Framework.</span></span> <span data-ttu-id="e3201-109">Modell zuerst können Sie ein neues Modell mit dem Entity Framework Designer erstellen und anschließend ein Datenbankschema aus dem Modell generieren.</span><span class="sxs-lookup"><span data-stu-id="e3201-109">Model First allows you to create a new model using the Entity Framework Designer and then generate a database schema from the model.</span></span> <span data-ttu-id="e3201-110">Das Modell kann im Entity Framework Designer wird in einer EDMX-Datei (EDMX-Erweiterung) gespeichert und angezeigt und bearbeitet werden.</span><span class="sxs-lookup"><span data-stu-id="e3201-110">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="e3201-111">Die Klassen, die Sie in Ihrer Anwendung interagieren, werden automatisch aus der EDMX-Datei generiert.</span><span class="sxs-lookup"><span data-stu-id="e3201-111">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

<span data-ttu-id="e3201-112">**Präsentation:** [Rowan Miller](http://romiller.com/)</span><span class="sxs-lookup"><span data-stu-id="e3201-112">**Presented By**: [Rowan Miller](http://romiller.com/)</span></span>

<span data-ttu-id="e3201-113">**Video**: [WMV](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.wmv) | [MP4](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-mp4video-modelfirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.zip)</span><span class="sxs-lookup"><span data-stu-id="e3201-113">**Video**: [WMV](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.wmv) | [MP4](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-mp4video-modelfirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="e3201-114">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="e3201-114">Pre-Requisites</span></span>

<span data-ttu-id="e3201-115">Sie benötigen Visual Studio 2010 oder Visual Studio 2012 installiert werden, um diese exemplarische Vorgehensweise abgeschlossen haben.</span><span class="sxs-lookup"><span data-stu-id="e3201-115">You will need to have Visual Studio 2010 or Visual Studio 2012 installed to complete this walkthrough.</span></span>

<span data-ttu-id="e3201-116">Wenn Sie Visual Studio 2010 verwenden, müssen Sie auch haben [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installiert.</span><span class="sxs-lookup"><span data-stu-id="e3201-116">If you are using Visual Studio 2010, you will also need to have [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installed.</span></span>

## <a name="1-create-the-application"></a><span data-ttu-id="e3201-117">1. Erstellen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="e3201-117">1. Create the Application</span></span>

<span data-ttu-id="e3201-118">Einfachheit halber werden wir eine einfache Konsolenanwendung zu erstellen, die den Model First verwendet, auf Daten zugreift:</span><span class="sxs-lookup"><span data-stu-id="e3201-118">To keep things simple we’re going to build a basic console application that uses the Model First to perform data access:</span></span>

-   <span data-ttu-id="e3201-119">Öffnen Sie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e3201-119">Open Visual Studio</span></span>
-   <span data-ttu-id="e3201-120">**Datei -&gt; neu –&gt; Projekt...**</span><span class="sxs-lookup"><span data-stu-id="e3201-120">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="e3201-121">Wählen Sie **Windows** im linken Menü und **-Konsolenanwendung**</span><span class="sxs-lookup"><span data-stu-id="e3201-121">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="e3201-122">Geben Sie **ModelFirstSample** als Name</span><span class="sxs-lookup"><span data-stu-id="e3201-122">Enter **ModelFirstSample** as the name</span></span>
-   <span data-ttu-id="e3201-123">Wählen Sie **OK** aus.</span><span class="sxs-lookup"><span data-stu-id="e3201-123">Select **OK**</span></span>

## <a name="2-create-model"></a><span data-ttu-id="e3201-124">2. Modell erstellen</span><span class="sxs-lookup"><span data-stu-id="e3201-124">2. Create Model</span></span>

<span data-ttu-id="e3201-125">Wir werden die Verwendung von Entity Framework Designer, die als Teil von Visual Studio enthalten ist, um unser Modell zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="e3201-125">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="e3201-126">**Projekt -&gt; neues Element hinzufügen...**</span><span class="sxs-lookup"><span data-stu-id="e3201-126">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="e3201-127">Wählen Sie **Daten** im linken Menü und dann **ADO.NET Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="e3201-127">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="e3201-128">Geben Sie **BloggingModel** als ein, und klicken Sie auf **OK**, dadurch wird der Assistent für Entity Data Model</span><span class="sxs-lookup"><span data-stu-id="e3201-128">Enter **BloggingModel** as the name and click **OK**, this launches the Entity Data Model Wizard</span></span>
-   <span data-ttu-id="e3201-129">Wählen Sie **leeres Modell** , und klicken Sie auf **Fertig stellen**</span><span class="sxs-lookup"><span data-stu-id="e3201-129">Select **Empty Model** and click **Finish**</span></span>

    ![CreateEmptyModel](~/ef6/media/createemptymodel.png)

<span data-ttu-id="e3201-131">Entity Framework Designer wird mit einem leeren Modell geöffnet.</span><span class="sxs-lookup"><span data-stu-id="e3201-131">The Entity Framework Designer is opened with a blank model.</span></span> <span data-ttu-id="e3201-132">Jetzt können wir beginnen, dem Modell Entitäten, Eigenschaften und Zuordnungen hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="e3201-132">Now we can start adding entities, properties and associations to the model.</span></span>

-   <span data-ttu-id="e3201-133">Mit der rechten Maustaste auf die Entwurfsoberfläche, und wählen **Eigenschaften**</span><span class="sxs-lookup"><span data-stu-id="e3201-133">Right-click on the design surface and select **Properties**</span></span>
-   <span data-ttu-id="e3201-134">Die Änderung der Eigenschaften im Fenster der **Entitätencontainername** zu **BloggingContext**
    *Dies ist der Name des abgeleiteten Kontexts, die für Sie, den Kontext generiert werden repräsentiert eine Sitzung mit der Datenbank, die es uns ermöglicht, Abfragen und Speichern von Daten*</span><span class="sxs-lookup"><span data-stu-id="e3201-134">In the Properties window change the **Entity Container Name** to **BloggingContext**
*This is the name of the derived context that will be generated for you, the context represents a session with the database, allowing us to query and save data*</span></span>
-   <span data-ttu-id="e3201-135">Mit der rechten Maustaste auf die Entwurfsoberfläche, und wählen **Add New -&gt; Entität...**</span><span class="sxs-lookup"><span data-stu-id="e3201-135">Right-click on the design surface and select **Add New -&gt; Entity…**</span></span>
-   <span data-ttu-id="e3201-136">Geben Sie **Blog** als den Namen der Entität und **BlogId** als Schlüssel ein, und klicken Sie auf **OK**</span><span class="sxs-lookup"><span data-stu-id="e3201-136">Enter **Blog** as the entity name and **BlogId** as the key name and click **OK**</span></span>

    ![AddBlogEntity](~/ef6/media/addblogentity.png)

-   <span data-ttu-id="e3201-138">Mit der rechten Maustaste auf die neue Entität, auf die Entwurfsoberfläche, und wählen **Add New -&gt; Skalareigenschaft**, geben Sie **Namen** als Name der Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="e3201-138">Right-click on the new entity on the design surface and select **Add New -&gt; Scalar Property**, enter **Name** as the name of the property.</span></span>
-   <span data-ttu-id="e3201-139">Wiederholen Sie diesen Vorgang zum Hinzufügen einer **Url** Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="e3201-139">Repeat this process to add a **Url** property.</span></span>
-   <span data-ttu-id="e3201-140">Mit der rechten Maustaste auf die **Url** Eigenschaft auf die Entwurfsoberfläche, und wählen **Eigenschaften**, in das Ändern der Eigenschaften im Fenster der **Nullable** auf **"true"** 
     *Dadurch können wir einen Blog in der Datenbank zu speichern, ohne dass sie eine Url zugewiesen*</span><span class="sxs-lookup"><span data-stu-id="e3201-140">Right-click on the **Url** property on the design surface and select **Properties**, in the Properties window change the **Nullable** setting to **True**
*This allows us to save a Blog to the database without assigning it a Url*</span></span>
-   <span data-ttu-id="e3201-141">Mit den Verfahren, die Sie gerade gelernt haben, fügen einen **Post** Entität mit einem **Beitrags** Schlüsseleigenschaft</span><span class="sxs-lookup"><span data-stu-id="e3201-141">Using the techniques you just learnt, add a **Post** entity with a **PostId** key property</span></span>
-   <span data-ttu-id="e3201-142">Hinzufügen **Titel** und **Content** skalare Eigenschaften der **Post** Entität</span><span class="sxs-lookup"><span data-stu-id="e3201-142">Add **Title** and **Content** scalar properties to the **Post** entity</span></span>

<span data-ttu-id="e3201-143">Nun, da wir eine Reihe von Entitäten haben, ist es Zeit zum Hinzufügen einer Zuordnung (oder einer Beziehung) zwischen ihnen.</span><span class="sxs-lookup"><span data-stu-id="e3201-143">Now that we have a couple of entities, it’s time to add an association (or relationship) between them.</span></span>

-   <span data-ttu-id="e3201-144">Mit der rechten Maustaste auf die Entwurfsoberfläche, und wählen **Add New -&gt; Zuordnung...**</span><span class="sxs-lookup"><span data-stu-id="e3201-144">Right-click on the design surface and select **Add New -&gt; Association…**</span></span>
-   <span data-ttu-id="e3201-145">Stellen Sie ein Ende der Beziehung auf verweisen **Blog** mit einer Multiplizität von **eine** und der andere Endpunkt zu **Post** mit einer Multiplizität von **viele** 
     *Dies bedeutet, dass ein Blog viele Beiträge hat und ein Blog ein Posting gehört*</span><span class="sxs-lookup"><span data-stu-id="e3201-145">Make one end of the relationship point to **Blog** with a multiplicity of **One** and the other end point to **Post** with a multiplicity of **Many**
*This means that a Blog has many Posts and a Post belongs to one Blog*</span></span>
-   <span data-ttu-id="e3201-146">Stellen Sie sicher die **Fremdschlüsseleigenschaften auf "Post" Entität hinzufügen** wird überprüft, und klicken Sie auf **OK**</span><span class="sxs-lookup"><span data-stu-id="e3201-146">Ensure the **Add foreign key properties to 'Post' Entity** box is checked and click **OK**</span></span>

    ![AddAssociationMF](~/ef6/media/addassociationmf.png)

<span data-ttu-id="e3201-148">Wir haben nun ein einfaches Modell, das wir Generieren einer Datenbank aus und zum Lesen und Schreiben von Daten verwenden können.</span><span class="sxs-lookup"><span data-stu-id="e3201-148">We now have a simple model that we can generate a database from and use to read and write data.</span></span>

![ModelInitial](~/ef6/media/modelinitial.png)

### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="e3201-150">Zusätzliche Schritte in Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="e3201-150">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="e3201-151">Wenn Sie in Visual Studio 2010 arbeiten gibt es jedoch einige zusätzliche Schritte, die Sie befolgen, um auf die neueste Version von Entity Framework zu aktualisieren müssen.</span><span class="sxs-lookup"><span data-stu-id="e3201-151">If you are working in Visual Studio 2010 there are some additional steps you need to follow to upgrade to the latest version of Entity Framework.</span></span> <span data-ttu-id="e3201-152">Es ist wichtig, ein Upgrade, da sie Zugriff auf eine verbesserte API-Oberfläche, die viel einfacher erhalten zu verwenden ist, sowie die neuesten Updates.</span><span class="sxs-lookup"><span data-stu-id="e3201-152">Upgrading is important because it gives you access to an improved API surface, that is much easier to use, as well as the latest bug fixes.</span></span>

<span data-ttu-id="e3201-153">Zuerst muss, um die neueste Version von Entity Framework von NuGet zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="e3201-153">First up, we need to get the latest version of Entity Framework from NuGet.</span></span>

-   <span data-ttu-id="e3201-154">**Projekt:&gt; NuGet-Pakete verwalten... ** 
     *Wenn Ihnen keine der **NuGet-Pakete verwalten... ** Option Sie installieren die [neueste Version von NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*</span><span class="sxs-lookup"><span data-stu-id="e3201-154">**Project –&gt; Manage NuGet Packages…**
*If you don’t have the **Manage NuGet Packages…** option you should install the [latest version of NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*</span></span>
-   <span data-ttu-id="e3201-155">Wählen Sie die **Online** Registerkarte</span><span class="sxs-lookup"><span data-stu-id="e3201-155">Select the **Online** tab</span></span>
-   <span data-ttu-id="e3201-156">Wählen Sie die **EntityFramework** Paket</span><span class="sxs-lookup"><span data-stu-id="e3201-156">Select the **EntityFramework** package</span></span>
-   <span data-ttu-id="e3201-157">Klicken Sie auf **installieren**</span><span class="sxs-lookup"><span data-stu-id="e3201-157">Click **Install**</span></span>

<span data-ttu-id="e3201-158">Als Nächstes müssen wir unser Modell zum Generieren von Code, mit der die DbContext-API verwenden, die in höheren Versionen von Entity Framework eingeführte, austauschen.</span><span class="sxs-lookup"><span data-stu-id="e3201-158">Next, we need to swap our model to generate code that makes use of the DbContext API, which was introduced in later versions of Entity Framework.</span></span>

-   <span data-ttu-id="e3201-159">Mit der rechten Maustaste auf eine leere Stelle des Modells im EF Designer, und wählen Sie **Codegenerierungselement hinzufügen...**</span><span class="sxs-lookup"><span data-stu-id="e3201-159">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="e3201-160">Wählen Sie **Onlinevorlagen** aus dem linken Menü und die Suche nach **"DbContext"**</span><span class="sxs-lookup"><span data-stu-id="e3201-160">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="e3201-161">Wählen Sie die EF **5.x DbContext Generator für C\#**, geben Sie **BloggingModel** als ein, und klicken Sie auf **hinzufügen**</span><span class="sxs-lookup"><span data-stu-id="e3201-161">Select the EF **5.x DbContext Generator for C\#**, enter **BloggingModel** as the name and click **Add**</span></span>

    ![DbContextTemplate](~/ef6/media/dbcontexttemplate.png)

## <a name="3-generating-the-database"></a><span data-ttu-id="e3201-163">3. Der datenbankgenerierung</span><span class="sxs-lookup"><span data-stu-id="e3201-163">3. Generating the Database</span></span>

<span data-ttu-id="e3201-164">Wenn unser Modell ist, können Entity Framework ein Datenbankschema berechnen, die es ermöglichen, speichern und Abrufen von Daten mithilfe des Datenmodells.</span><span class="sxs-lookup"><span data-stu-id="e3201-164">Given our model, Entity Framework can calculate a database schema that will allow us to store and retrieve data using the model.</span></span>

<span data-ttu-id="e3201-165">Der Datenbankserver, der mit Visual Studio installiert ist, unterscheidet sich abhängig von der Version von Visual Studio, die Sie installiert haben:</span><span class="sxs-lookup"><span data-stu-id="e3201-165">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="e3201-166">Wenn Sie Visual Studio 2010 verwenden erstellen Sie eine SQL Express-Datenbank.</span><span class="sxs-lookup"><span data-stu-id="e3201-166">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="e3201-167">Wenn Sie Visual Studio 2012 verwenden, und klicken Sie dann Sie erstellen eine [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) Datenbank.</span><span class="sxs-lookup"><span data-stu-id="e3201-167">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) database.</span></span>

<span data-ttu-id="e3201-168">Wir jetzt, und Erstellen der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="e3201-168">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="e3201-169">Mit der rechten Maustaste auf die Entwurfsoberfläche, und wählen **Datenbank aus Modell generieren...**</span><span class="sxs-lookup"><span data-stu-id="e3201-169">Right-click on the design surface and select **Generate Database from Model…**</span></span>
-   <span data-ttu-id="e3201-170">Klicken Sie auf **neue Verbindung...**</span><span class="sxs-lookup"><span data-stu-id="e3201-170">Click **New Connection…**</span></span> <span data-ttu-id="e3201-171">und geben Sie entweder LocalDB oder SQL Express, je nachdem, welche Version von Visual Studio Sie verwenden, geben Sie **ModelFirst.Blogging** als Datenbankname verwendet.</span><span class="sxs-lookup"><span data-stu-id="e3201-171">and specify either LocalDB or SQL Express, depending on which version of Visual Studio you are using, enter **ModelFirst.Blogging** as the database name.</span></span>

    ![LocalDBConnectionMF](~/ef6/media/localdbconnectionmf.png)

    ![SqlExpressConnectionMF](~/ef6/media/sqlexpressconnectionmf.png)

-   <span data-ttu-id="e3201-174">Wählen Sie **OK** und Sie werden gefragt, ob Sie eine neue Datenbank, die auf erstellen möchten **Ja**</span><span class="sxs-lookup"><span data-stu-id="e3201-174">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>
-   <span data-ttu-id="e3201-175">Wählen Sie **Weiter** und Entity Framework Designer berechnet ein Skript, um das Datenbankschema zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="e3201-175">Select **Next** and the Entity Framework Designer will calculate a script to create the database schema</span></span>
-   <span data-ttu-id="e3201-176">Nachdem das Skript angezeigt wird, klicken Sie auf **Fertig stellen** und das Skript zu Ihrem Projekt hinzugefügt und geöffnet werden</span><span class="sxs-lookup"><span data-stu-id="e3201-176">Once the script is displayed, click **Finish** and the script will be added to your project and opened</span></span>
-   <span data-ttu-id="e3201-177">Mit der rechten Maustaste auf das Skript, und wählen Sie **Execute**, werden Sie aufgefordert, geben Sie die Datenbank eine Verbindung herstellen, geben Sie LocalDB oder SQLServer Express, abhängig von der Version von Visual Studio Sie verwenden</span><span class="sxs-lookup"><span data-stu-id="e3201-177">Right-click on the script and select **Execute**, you will be prompted to specify the database to connect to, specify LocalDB or SQL Server Express, depending on which version of Visual Studio you are using</span></span>

## <a name="4-reading--writing-data"></a><span data-ttu-id="e3201-178">4. Lesen und Schreiben von Daten</span><span class="sxs-lookup"><span data-stu-id="e3201-178">4. Reading & Writing Data</span></span>

<span data-ttu-id="e3201-179">Nun, da es sich um ein Modell verfügen, ist es Zeit, die sie verwenden, um einige Daten zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="e3201-179">Now that we have a model it’s time to use it to access some data.</span></span> <span data-ttu-id="e3201-180">Die Klassen werden wir Sie verwenden, um den Zugriff auf Daten werden wird automatisch generiert basierend auf die EDMX-Datei.</span><span class="sxs-lookup"><span data-stu-id="e3201-180">The classes we are going to use to access data are being automatically generated for you based on the EDMX file.</span></span>

<span data-ttu-id="e3201-181">*Dieser Screenshot stammt aus Visual Studio 2012, bei Verwendung von Visual Studio 2010 die BloggingModel.tt und BloggingModel.Context.tt Dateien werden direkt unterhalb des Projekts und nicht als geschachtelte unter der EDMX-Datei.*</span><span class="sxs-lookup"><span data-stu-id="e3201-181">*This screen shot is from Visual Studio 2012, if you are using Visual Studio 2010 the BloggingModel.tt and BloggingModel.Context.tt files will be directly under your project rather than nested under the EDMX file.*</span></span>

![Assistenten GeneratedClasses](~/ef6/media/generatedclasses.png)

<span data-ttu-id="e3201-183">Implementieren Sie die Main-Methode in "Program.cs" ein, wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="e3201-183">Implement the Main method in Program.cs as shown below.</span></span> <span data-ttu-id="e3201-184">Dieser Code erstellt eine neue Instanz unseres Kontexts und anschließend verwendet, um einen neuen Blog einfügen.</span><span class="sxs-lookup"><span data-stu-id="e3201-184">This code creates a new instance of our context and then uses it to insert a new Blog.</span></span> <span data-ttu-id="e3201-185">Anschließend wird eine LINQ-Abfrage zum Abrufen aller Blogs aus der Datenbank nach Titel alphabetisch angeordnet sind.</span><span class="sxs-lookup"><span data-stu-id="e3201-185">Then it uses a LINQ query to retrieve all Blogs from the database ordered alphabetically by Title.</span></span>

``` csharp
class Program
{
    static void Main(string[] args)
    {
        using (var db = new BloggingContext())
        {
            // Create and save a new Blog
            Console.Write("Enter a name for a new Blog: ");
            var name = Console.ReadLine();

            var blog = new Blog { Name = name };
            db.Blogs.Add(blog);
            db.SaveChanges();

            // Display all Blogs from the database
            var query = from b in db.Blogs
                        orderby b.Name
                        select b;

            Console.WriteLine("All blogs in the database:");
            foreach (var item in query)
            {
                Console.WriteLine(item.Name);
            }

            Console.WriteLine("Press any key to exit...");
            Console.ReadKey();
        }
    }
}
```

<span data-ttu-id="e3201-186">Sie können jetzt die Anwendung auszuführen, und probieren Sie es aus.</span><span class="sxs-lookup"><span data-stu-id="e3201-186">You can now run the application and test it out.</span></span>

```
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```

## <a name="5-dealing-with-model-changes"></a><span data-ttu-id="e3201-187">5. Umgang mit Änderungen des Datenmodells</span><span class="sxs-lookup"><span data-stu-id="e3201-187">5. Dealing with Model Changes</span></span>

<span data-ttu-id="e3201-188">Jetzt ist es Zeit, einige Änderungen auf unserem Modell vornehmen, wenn wir diese Änderungen vornehmen, die wir auch das Datenbankschema zu aktualisieren müssen.</span><span class="sxs-lookup"><span data-stu-id="e3201-188">Now it’s time to make some changes to our model, when we make these changes we also need to update the database schema.</span></span>

<span data-ttu-id="e3201-189">Wir beginnen, durch das Hinzufügen einer neuen Benutzerentität auf unserem Modell.</span><span class="sxs-lookup"><span data-stu-id="e3201-189">We’ll start by adding a new User entity to our model.</span></span>

-   <span data-ttu-id="e3201-190">Fügen Sie einen neuen **Benutzer** Entitätsname mit **Benutzername** des Schlüsselnamens und **Zeichenfolge** weil der Eigenschaftentyp, für den Schlüssel</span><span class="sxs-lookup"><span data-stu-id="e3201-190">Add a new **User** entity name with **Username** as the key name and **String** as the property type for the key</span></span>

    ![AddUserEntity](~/ef6/media/adduserentity.png)

-   <span data-ttu-id="e3201-192">Mit der rechten Maustaste auf die **Benutzername** Eigenschaft auf die Entwurfsoberfläche, und wählen **Eigenschaften**, Eigenschaften In der Fenster Ändern der **MaxLength** auf **50 ** 
     *Dies schränkt die Daten, die im Benutzernamen auf 50 Zeichen gespeichert werden können*</span><span class="sxs-lookup"><span data-stu-id="e3201-192">Right-click on the **Username** property on the design surface and select **Properties**, In the Properties window change the **MaxLength** setting to **50**
*This restricts the data that can be stored in username to 50 characters*</span></span>
-   <span data-ttu-id="e3201-193">Hinzufügen einer **"DisplayName"** skalare Eigenschaft, um die **Benutzer** Entität</span><span class="sxs-lookup"><span data-stu-id="e3201-193">Add a **DisplayName** scalar property to the **User** entity</span></span>

<span data-ttu-id="e3201-194">Wir verfügen jetzt über einen aktualisierten Modell, und wir können zum Aktualisieren der Datenbank, um unsere neuen Entitätstyp für den Benutzer zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="e3201-194">We now have an updated model and we are ready to update the database to accommodate our new User entity type.</span></span>

-   <span data-ttu-id="e3201-195">Mit der rechten Maustaste auf die Entwurfsoberfläche, und wählen **Datenbank aus Modell generieren...** , Entity Framework ein Skript zum Erstellen Sie ein Schema basierend auf dem aktualisierten Modell neu berechnet.</span><span class="sxs-lookup"><span data-stu-id="e3201-195">Right-click on the design surface and select **Generate Database from Model…**, Entity Framework will calculate a script to recreate a schema based on the updated model.</span></span>
-   <span data-ttu-id="e3201-196">Klicken Sie auf **Fertig stellen**</span><span class="sxs-lookup"><span data-stu-id="e3201-196">Click **Finish**</span></span>
-   <span data-ttu-id="e3201-197">Sie können Warnungen zum Überschreiben der vorhandenen DDL-Skript und die Zuordnung und Speicher Teile des Modells angezeigt, klicken Sie auf **Ja** für beide diese Warnungen</span><span class="sxs-lookup"><span data-stu-id="e3201-197">You may receive warnings about overwriting the existing DDL script and the mapping and storage parts of the model, click **Yes** for both these warnings</span></span>
-   <span data-ttu-id="e3201-198">Das aktualisierte SQL-Skript zum Erstellen der Datenbank ist für Sie geöffnet.</span><span class="sxs-lookup"><span data-stu-id="e3201-198">The updated SQL script to create the database is opened for you</span></span>  
    <span data-ttu-id="e3201-199">*Das Skript, das generiert wird, werden alle vorhandene Tabellen löschen und erstellen Sie das Schema von Grund auf neu. Dies funktioniert für die lokale Entwicklung ist jedoch kein gültiges pushübermittlung von Änderungen in einer Datenbank, die bereits bereitgestellt wurde. Wenn Sie müssen Änderungen in einer Datenbank zu veröffentlichen, die bereits bereitgestellt wurde, müssen Sie bearbeiten Sie das Skript aus, oder verwenden ein Schema vergleichen-Tool, um ein Migrationsskript zu berechnen.*</span><span class="sxs-lookup"><span data-stu-id="e3201-199">*The script that is generated will drop all existing tables and then recreate the schema from scratch. This may work for local development but is not a viable for pushing changes to a database that has already been deployed. If you need to publish changes to a database that has already been deployed, you will need to edit the script or use a schema compare tool to calculate a migration script.*</span></span>
-   <span data-ttu-id="e3201-200">Mit der rechten Maustaste auf das Skript, und wählen Sie **Execute**, werden Sie aufgefordert, geben Sie die Datenbank eine Verbindung herstellen, geben Sie LocalDB oder SQLServer Express, abhängig von der Version von Visual Studio Sie verwenden</span><span class="sxs-lookup"><span data-stu-id="e3201-200">Right-click on the script and select **Execute**, you will be prompted to specify the database to connect to, specify LocalDB or SQL Server Express, depending on which version of Visual Studio you are using</span></span>

## <a name="summary"></a><span data-ttu-id="e3201-201">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="e3201-201">Summary</span></span>

<span data-ttu-id="e3201-202">In dieser exemplarischen Vorgehensweise, die wir uns Model First-Entwicklung angesehen haben, hatten die wir ein Modell im EF Designer erstellen und anschließend eine Datenbank von diesem Modell generieren.</span><span class="sxs-lookup"><span data-stu-id="e3201-202">In this walkthrough we looked at Model First development, which allowed us to create a model in the EF Designer and then generate a database from that model.</span></span> <span data-ttu-id="e3201-203">Klicken Sie dann verwendet das Modell lesen und Schreiben von Daten aus der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="e3201-203">We then used the model to read and write some data from the database.</span></span> <span data-ttu-id="e3201-204">Abschließend das Modell aktualisiert und dann neu erstellt das Datenbankschema entsprechend das Modell.</span><span class="sxs-lookup"><span data-stu-id="e3201-204">Finally, we updated the model and then recreated the database schema to match the model.</span></span>
