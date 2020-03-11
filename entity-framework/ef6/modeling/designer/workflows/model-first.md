---
title: Model First-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: e1b9c319-bb8a-4417-ac94-7890f257e7f6
ms.openlocfilehash: 1b37805beb3d33f0b6dad2577a8abb3ea8f7b1e4
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414956"
---
# <a name="model-first"></a><span data-ttu-id="a7c19-102">Model First</span><span class="sxs-lookup"><span data-stu-id="a7c19-102">Model First</span></span>
<span data-ttu-id="a7c19-103">Dieses Video und die schrittweise exemplarische Vorgehensweise bieten eine Einführung in die Model First Entwicklung mit Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="a7c19-103">This video and step-by-step walkthrough provide an introduction to Model First development using Entity Framework.</span></span> <span data-ttu-id="a7c19-104">Mit Model First können Sie ein neues Modell mithilfe des Entity Framework Designer erstellen und dann ein Datenbankschema aus dem Modell generieren.</span><span class="sxs-lookup"><span data-stu-id="a7c19-104">Model First allows you to create a new model using the Entity Framework Designer and then generate a database schema from the model.</span></span> <span data-ttu-id="a7c19-105">Das Modell wird in einer EDMX-Datei (edmx-Erweiterung) gespeichert und kann in der Entity Framework Designer angezeigt und bearbeitet werden.</span><span class="sxs-lookup"><span data-stu-id="a7c19-105">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="a7c19-106">Die Klassen, mit denen Sie in Ihrer Anwendung interagieren, werden automatisch aus der EDMX-Datei generiert.</span><span class="sxs-lookup"><span data-stu-id="a7c19-106">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="a7c19-107">Video ansehen</span><span class="sxs-lookup"><span data-stu-id="a7c19-107">Watch the video</span></span>
<span data-ttu-id="a7c19-108">Dieses Video und die schrittweise exemplarische Vorgehensweise bieten eine Einführung in die Model First Entwicklung mit Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="a7c19-108">This video and step-by-step walkthrough provide an introduction to Model First development using Entity Framework.</span></span> <span data-ttu-id="a7c19-109">Mit Model First können Sie ein neues Modell mithilfe des Entity Framework Designer erstellen und dann ein Datenbankschema aus dem Modell generieren.</span><span class="sxs-lookup"><span data-stu-id="a7c19-109">Model First allows you to create a new model using the Entity Framework Designer and then generate a database schema from the model.</span></span> <span data-ttu-id="a7c19-110">Das Modell wird in einer EDMX-Datei (edmx-Erweiterung) gespeichert und kann in der Entity Framework Designer angezeigt und bearbeitet werden.</span><span class="sxs-lookup"><span data-stu-id="a7c19-110">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="a7c19-111">Die Klassen, mit denen Sie in Ihrer Anwendung interagieren, werden automatisch aus der EDMX-Datei generiert.</span><span class="sxs-lookup"><span data-stu-id="a7c19-111">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

<span data-ttu-id="a7c19-112">**Präsentation:** [Rowan Miller](https://romiller.com/)</span><span class="sxs-lookup"><span data-stu-id="a7c19-112">**Presented By**: [Rowan Miller](https://romiller.com/)</span></span>

<span data-ttu-id="a7c19-113">**Video**: [WMV](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.wmv) | [MP4](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-mp4video-modelfirst.m4v) | [WMV (zip)](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.zip)</span><span class="sxs-lookup"><span data-stu-id="a7c19-113">**Video**: [WMV](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.wmv) | [MP4](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-mp4video-modelfirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="a7c19-114">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="a7c19-114">Pre-Requisites</span></span>

<span data-ttu-id="a7c19-115">Sie müssen Visual Studio 2010 oder Visual Studio 2012 installiert haben, um diese exemplarische Vorgehensweise abzuschließen.</span><span class="sxs-lookup"><span data-stu-id="a7c19-115">You will need to have Visual Studio 2010 or Visual Studio 2012 installed to complete this walkthrough.</span></span>

<span data-ttu-id="a7c19-116">Wenn Sie Visual Studio 2010 verwenden, müssen Sie auch [nuget](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installieren.</span><span class="sxs-lookup"><span data-stu-id="a7c19-116">If you are using Visual Studio 2010, you will also need to have [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installed.</span></span>

## <a name="1-create-the-application"></a><span data-ttu-id="a7c19-117">1. Erstellen Sie die Anwendung.</span><span class="sxs-lookup"><span data-stu-id="a7c19-117">1. Create the Application</span></span>

<span data-ttu-id="a7c19-118">Um dies zu gewährleisten, erstellen wir eine einfache Konsolenanwendung, die die Model First für den Datenzugriff verwendet:</span><span class="sxs-lookup"><span data-stu-id="a7c19-118">To keep things simple we’re going to build a basic console application that uses the Model First to perform data access:</span></span>

-   <span data-ttu-id="a7c19-119">Öffnen Sie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a7c19-119">Open Visual Studio</span></span>
-   <span data-ttu-id="a7c19-120">**Datei&gt; Projekt für neue&gt;...**</span><span class="sxs-lookup"><span data-stu-id="a7c19-120">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="a7c19-121">Wählen Sie im Menü auf der linken Seite und **Konsolenanwendung** **Windows** aus.</span><span class="sxs-lookup"><span data-stu-id="a7c19-121">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="a7c19-122">Geben Sie **modelfirstsample** als Name ein.</span><span class="sxs-lookup"><span data-stu-id="a7c19-122">Enter **ModelFirstSample** as the name</span></span>
-   <span data-ttu-id="a7c19-123">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="a7c19-123">Select **OK**</span></span>

## <a name="2-create-model"></a><span data-ttu-id="a7c19-124">2. Modell erstellen</span><span class="sxs-lookup"><span data-stu-id="a7c19-124">2. Create Model</span></span>

<span data-ttu-id="a7c19-125">Wir verwenden Entity Framework Designer, die als Teil von Visual Studio enthalten ist, um das Modell zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="a7c19-125">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="a7c19-126">**Projekt&gt; neues Element hinzufügen...**</span><span class="sxs-lookup"><span data-stu-id="a7c19-126">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="a7c19-127">Wählen Sie im linken Menü **Daten** aus, und klicken Sie dann auf **ADO.NET Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="a7c19-127">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="a7c19-128">Geben Sie **bloggingmodel** als Name ein, und klicken Sie auf **OK**. Dadurch wird der Entity Data Model-Assistent gestartet.</span><span class="sxs-lookup"><span data-stu-id="a7c19-128">Enter **BloggingModel** as the name and click **OK**, this launches the Entity Data Model Wizard</span></span>
-   <span data-ttu-id="a7c19-129">**Leeres Modell** auswählen und auf **Fertig** stellen klicken</span><span class="sxs-lookup"><span data-stu-id="a7c19-129">Select **Empty Model** and click **Finish**</span></span>

    ![Leeres Modell erstellen](~/ef6/media/createemptymodel.png)

<span data-ttu-id="a7c19-131">Die Entity Framework Designer wird mit einem leeren Modell geöffnet.</span><span class="sxs-lookup"><span data-stu-id="a7c19-131">The Entity Framework Designer is opened with a blank model.</span></span> <span data-ttu-id="a7c19-132">Nun können Sie dem Modell Entitäten, Eigenschaften und Zuordnungen hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="a7c19-132">Now we can start adding entities, properties and associations to the model.</span></span>

-   <span data-ttu-id="a7c19-133">Klicken Sie mit der rechten Maustaste auf die Entwurfs Oberfläche, und wählen Sie **Eigenschaften**</span><span class="sxs-lookup"><span data-stu-id="a7c19-133">Right-click on the design surface and select **Properties**</span></span>
-   <span data-ttu-id="a7c19-134">In der Eigenschaftenfenster den **Namen des Entitäts Containers** in **bloggingcontext** ändern
    *dies der Name des abgeleiteten Kontexts, der für Sie generiert wird. der Kontext stellt eine Sitzung mit der Datenbank dar und ermöglicht es uns, Daten abzufragen und zu speichern* .</span><span class="sxs-lookup"><span data-stu-id="a7c19-134">In the Properties window change the **Entity Container Name** to **BloggingContext**
*This is the name of the derived context that will be generated for you, the context represents a session with the database, allowing us to query and save data*</span></span>
-   <span data-ttu-id="a7c19-135">Klicken Sie mit der rechten Maustaste auf die Entwurfs Oberfläche, und wählen Sie **Add New-&gt; Entity...**</span><span class="sxs-lookup"><span data-stu-id="a7c19-135">Right-click on the design surface and select **Add New -&gt; Entity…**</span></span>
-   <span data-ttu-id="a7c19-136">Geben **Sie** den Namen der Entität und die **BlogId** als Schlüssel Name ein, und klicken Sie auf **OK** .</span><span class="sxs-lookup"><span data-stu-id="a7c19-136">Enter **Blog** as the entity name and **BlogId** as the key name and click **OK**</span></span>

    ![Blog Entität hinzufügen](~/ef6/media/addblogentity.png)

-   <span data-ttu-id="a7c19-138">Klicken Sie in der Entwurfs Oberfläche mit der rechten Maustaste auf die neue Entität, und wählen Sie **neue&gt; skalare Eigenschaft hinzufügen**aus, und geben Sie **Name** als Namen für die Eigenschaft ein.</span><span class="sxs-lookup"><span data-stu-id="a7c19-138">Right-click on the new entity on the design surface and select **Add New -&gt; Scalar Property**, enter **Name** as the name of the property.</span></span>
-   <span data-ttu-id="a7c19-139">Wiederholen Sie diesen Vorgang, um eine **URL** -Eigenschaft hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="a7c19-139">Repeat this process to add a **Url** property.</span></span>
-   <span data-ttu-id="a7c19-140">Klicken Sie mit der rechten Maustaste auf die **URL** -Eigenschaft auf der Entwurfs Oberfläche, und wählen Sie **Eigenschaften**aus. **in der Eigenschaftenfenster** ändern Sie die Einstellung auf "true", die auf " **true** " festgelegt ist
    *dadurch können wir einen Blog in der Datenbank speichern, ohne*</span><span class="sxs-lookup"><span data-stu-id="a7c19-140">Right-click on the **Url** property on the design surface and select **Properties**, in the Properties window change the **Nullable** setting to **True**
*This allows us to save a Blog to the database without assigning it a Url*</span></span>
-   <span data-ttu-id="a7c19-141">Fügen Sie mit den Techniken, die Sie soeben gelernt haben, eine **Post** -Entität mit der Eigenschaft " **POSID** Key</span><span class="sxs-lookup"><span data-stu-id="a7c19-141">Using the techniques you just learnt, add a **Post** entity with a **PostId** key property</span></span>
-   <span data-ttu-id="a7c19-142">Hinzufügen von **Titel** -und **inhaltsskalaren** Eigenschaften zur **Post** -Entität</span><span class="sxs-lookup"><span data-stu-id="a7c19-142">Add **Title** and **Content** scalar properties to the **Post** entity</span></span>

<span data-ttu-id="a7c19-143">Nachdem wir nun über mehrere Entitäten verfügen, ist es an der Zeit, eine Zuordnung (oder Beziehung) zwischen Ihnen hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="a7c19-143">Now that we have a couple of entities, it’s time to add an association (or relationship) between them.</span></span>

-   <span data-ttu-id="a7c19-144">Klicken Sie mit der rechten Maustaste auf die Entwurfs Oberfläche, und wählen Sie **Hinzufügen New-&gt; Association...**</span><span class="sxs-lookup"><span data-stu-id="a7c19-144">Right-click on the design surface and select **Add New -&gt; Association…**</span></span>
-   <span data-ttu-id="a7c19-145">Erstellen Sie ein Ende der **Beziehung mit einer** Multiplizität von **einem** und dem anderen Endpunkt, um ihn mit einer Multiplizität von **vielen**
    zu **veröffentlichen** . *Dies bedeutet, dass ein Blog viele Beiträge enthält und ein Beitrag zu einem Blog gehört* .</span><span class="sxs-lookup"><span data-stu-id="a7c19-145">Make one end of the relationship point to **Blog** with a multiplicity of **One** and the other end point to **Post** with a multiplicity of **Many**
*This means that a Blog has many Posts and a Post belongs to one Blog*</span></span>
-   <span data-ttu-id="a7c19-146">Stellen Sie sicher, dass das Feld **Fremdschlüssel Eigenschaften zu ' Post ' hinzufügen** aktiviert ist, und klicken Sie auf **OK**</span><span class="sxs-lookup"><span data-stu-id="a7c19-146">Ensure the **Add foreign key properties to 'Post' Entity** box is checked and click **OK**</span></span>

    ![Association-MF hinzufügen](~/ef6/media/addassociationmf.png)

<span data-ttu-id="a7c19-148">Wir verfügen jetzt über ein einfaches Modell, aus dem eine Datenbank generiert und zum Lesen und Schreiben von Daten verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="a7c19-148">We now have a simple model that we can generate a database from and use to read and write data.</span></span>

![Modell anfänglich](~/ef6/media/modelinitial.png)

### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="a7c19-150">Weitere Schritte in Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="a7c19-150">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="a7c19-151">Wenn Sie in Visual Studio 2010 arbeiten, müssen Sie einige zusätzliche Schritte ausführen, um ein Upgrade auf die neueste Version von Entity Framework durchzuführen.</span><span class="sxs-lookup"><span data-stu-id="a7c19-151">If you are working in Visual Studio 2010 there are some additional steps you need to follow to upgrade to the latest version of Entity Framework.</span></span> <span data-ttu-id="a7c19-152">Das Upgrade ist wichtig, da Sie Ihnen Zugriff auf eine verbesserte API-Oberfläche, die viel einfacher zu verwenden ist, sowie auf die neuesten Fehlerbehebungen bietet.</span><span class="sxs-lookup"><span data-stu-id="a7c19-152">Upgrading is important because it gives you access to an improved API surface, that is much easier to use, as well as the latest bug fixes.</span></span>

<span data-ttu-id="a7c19-153">Zuerst müssen wir die neueste Version von Entity Framework von nuget erhalten.</span><span class="sxs-lookup"><span data-stu-id="a7c19-153">First up, we need to get the latest version of Entity Framework from NuGet.</span></span>

-   <span data-ttu-id="a7c19-154">**Project –&gt; nuget-Pakete verwalten...** 
    *Wenn Sie nicht über die Option " **nuget-Pakete verwalten..** ." verfügen, sollten Sie die [neueste Version von nuget](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installieren.*</span><span class="sxs-lookup"><span data-stu-id="a7c19-154">**Project –&gt; Manage NuGet Packages…**
*If you don’t have the **Manage NuGet Packages…** option you should install the [latest version of NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*</span></span>
-   <span data-ttu-id="a7c19-155">Auswählen der Registerkarte " **Online** "</span><span class="sxs-lookup"><span data-stu-id="a7c19-155">Select the **Online** tab</span></span>
-   <span data-ttu-id="a7c19-156">Auswählen des " **EntityFramework** "-Pakets</span><span class="sxs-lookup"><span data-stu-id="a7c19-156">Select the **EntityFramework** package</span></span>
-   <span data-ttu-id="a7c19-157">Klicken Sie auf **Installieren**</span><span class="sxs-lookup"><span data-stu-id="a7c19-157">Click **Install**</span></span>

<span data-ttu-id="a7c19-158">Als nächstes müssen wir das Modell austauschen, um Code zu generieren, der die dbcontext-API nutzt, die in späteren Versionen von Entity Framework eingeführt wurde.</span><span class="sxs-lookup"><span data-stu-id="a7c19-158">Next, we need to swap our model to generate code that makes use of the DbContext API, which was introduced in later versions of Entity Framework.</span></span>

-   <span data-ttu-id="a7c19-159">Klicken Sie mit der rechten Maustaste auf eine leere Stelle des Modells im EF-Designer, und wählen Sie **Code Generierungs Element hinzufügen... aus.**</span><span class="sxs-lookup"><span data-stu-id="a7c19-159">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="a7c19-160">Wählen Sie im linken Menü **Online Vorlagen** aus, und suchen Sie nach **dbcontext** .</span><span class="sxs-lookup"><span data-stu-id="a7c19-160">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="a7c19-161">Wählen Sie den EF **5. x dbcontext Generator für C\#aus** , geben Sie **bloggingmodel** als Name ein, und klicken Sie auf **Hinzufügen** .</span><span class="sxs-lookup"><span data-stu-id="a7c19-161">Select the EF **5.x DbContext Generator for C\#**, enter **BloggingModel** as the name and click **Add**</span></span>

    ![Dbcontext-Vorlage](~/ef6/media/dbcontexttemplate.png)

## <a name="3-generating-the-database"></a><span data-ttu-id="a7c19-163">3. Erstellen der Datenbank</span><span class="sxs-lookup"><span data-stu-id="a7c19-163">3. Generating the Database</span></span>

<span data-ttu-id="a7c19-164">Mit unserem Modell können Entity Framework ein Datenbankschema berechnen, das es uns ermöglicht, Daten mit dem Modell zu speichern und abzurufen.</span><span class="sxs-lookup"><span data-stu-id="a7c19-164">Given our model, Entity Framework can calculate a database schema that will allow us to store and retrieve data using the model.</span></span>

<span data-ttu-id="a7c19-165">Der Datenbankserver, der mit Visual Studio installiert wird, unterscheidet sich abhängig von der installierten Version von Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="a7c19-165">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="a7c19-166">Wenn Sie Visual Studio 2010 verwenden, erstellen Sie eine SQL Express-Datenbank.</span><span class="sxs-lookup"><span data-stu-id="a7c19-166">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="a7c19-167">Wenn Sie Visual Studio 2012 verwenden, erstellen Sie eine [localdb](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) -Datenbank.</span><span class="sxs-lookup"><span data-stu-id="a7c19-167">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) database.</span></span>

<span data-ttu-id="a7c19-168">Nun generieren wir die Datenbank.</span><span class="sxs-lookup"><span data-stu-id="a7c19-168">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="a7c19-169">Klicken Sie mit der rechten Maustaste auf die Entwurfs Oberfläche, und wählen Sie **Datenbank aus Modell generieren... aus.**</span><span class="sxs-lookup"><span data-stu-id="a7c19-169">Right-click on the design surface and select **Generate Database from Model…**</span></span>
-   <span data-ttu-id="a7c19-170">Klicken Sie auf **neue Verbindung...**</span><span class="sxs-lookup"><span data-stu-id="a7c19-170">Click **New Connection…**</span></span> <span data-ttu-id="a7c19-171">und geben Sie entweder localdb oder SQL Express an, je nachdem, welche Version von Visual Studio Sie verwenden, geben Sie **modelfirst. blogals** Datenbanknamen ein.</span><span class="sxs-lookup"><span data-stu-id="a7c19-171">and specify either LocalDB or SQL Express, depending on which version of Visual Studio you are using, enter **ModelFirst.Blogging** as the database name.</span></span>

    ![Localdb-Verbindungs-MF](~/ef6/media/localdbconnectionmf.png)

    ![SQL Express-Verbindung (MF)](~/ef6/media/sqlexpressconnectionmf.png)

-   <span data-ttu-id="a7c19-174">Wählen Sie **OK** aus, und Sie werden gefragt, ob Sie eine neue Datenbank erstellen möchten, und wählen Sie **Ja** aus.</span><span class="sxs-lookup"><span data-stu-id="a7c19-174">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>
-   <span data-ttu-id="a7c19-175">Wählen Sie **weiter** aus, und der Entity Framework Designer berechnet ein Skript, um das Datenbankschema zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="a7c19-175">Select **Next** and the Entity Framework Designer will calculate a script to create the database schema</span></span>
-   <span data-ttu-id="a7c19-176">Nachdem das Skript angezeigt wird, klicken Sie auf **Fertig** stellen, und das Skript wird dem Projekt hinzugefügt und geöffnet.</span><span class="sxs-lookup"><span data-stu-id="a7c19-176">Once the script is displayed, click **Finish** and the script will be added to your project and opened</span></span>
-   <span data-ttu-id="a7c19-177">Klicken Sie mit der rechten Maustaste auf das Skript, und wählen Sie **Ausführen**aus. Sie werden aufgefordert, die Datenbank anzugeben, mit der eine Verbindung hergestellt werden soll, und localdb oder SQL Server Express anzugeben, abhängig von der verwendeten Version von Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a7c19-177">Right-click on the script and select **Execute**, you will be prompted to specify the database to connect to, specify LocalDB or SQL Server Express, depending on which version of Visual Studio you are using</span></span>

## <a name="4-reading--writing-data"></a><span data-ttu-id="a7c19-178">4. Lesen & Schreiben von Daten</span><span class="sxs-lookup"><span data-stu-id="a7c19-178">4. Reading & Writing Data</span></span>

<span data-ttu-id="a7c19-179">Nachdem wir nun über ein Modell verfügen, ist es an der Zeit, es für den Zugriff auf einige Daten zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="a7c19-179">Now that we have a model it’s time to use it to access some data.</span></span> <span data-ttu-id="a7c19-180">Die Klassen, die für den Zugriff auf Daten verwendet werden, werden automatisch für Sie basierend auf der EDMX-Datei generiert.</span><span class="sxs-lookup"><span data-stu-id="a7c19-180">The classes we are going to use to access data are being automatically generated for you based on the EDMX file.</span></span>

<span data-ttu-id="a7c19-181">*Dieser Screenshot ist von Visual Studio 2012. Wenn Sie Visual Studio 2010 verwenden, befinden sich die Dateien BloggingModel.TT und BloggingModel.Context.tt direkt unter Ihrem Projekt, anstatt Sie in der EDMX-Datei zu untersuchen.*</span><span class="sxs-lookup"><span data-stu-id="a7c19-181">*This screen shot is from Visual Studio 2012, if you are using Visual Studio 2010 the BloggingModel.tt and BloggingModel.Context.tt files will be directly under your project rather than nested under the EDMX file.*</span></span>

![Generierte Klassen](~/ef6/media/generatedclasses.png)

<span data-ttu-id="a7c19-183">Implementieren Sie die Main-Methode in Program.cs, wie unten gezeigt.</span><span class="sxs-lookup"><span data-stu-id="a7c19-183">Implement the Main method in Program.cs as shown below.</span></span> <span data-ttu-id="a7c19-184">Mit diesem Code wird eine neue Instanz des Kontexts erstellt und anschließend verwendet, um einen neuen Blog einzufügen.</span><span class="sxs-lookup"><span data-stu-id="a7c19-184">This code creates a new instance of our context and then uses it to insert a new Blog.</span></span> <span data-ttu-id="a7c19-185">Anschließend wird eine LINQ-Abfrage verwendet, um alle Blogs aus der Datenbank alphabetisch nach Titel abzurufen.</span><span class="sxs-lookup"><span data-stu-id="a7c19-185">Then it uses a LINQ query to retrieve all Blogs from the database ordered alphabetically by Title.</span></span>

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

<span data-ttu-id="a7c19-186">Sie können die Anwendung jetzt ausführen und testen.</span><span class="sxs-lookup"><span data-stu-id="a7c19-186">You can now run the application and test it out.</span></span>

```console
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```

## <a name="5-dealing-with-model-changes"></a><span data-ttu-id="a7c19-187">5. Umgang mit Modelländerungen</span><span class="sxs-lookup"><span data-stu-id="a7c19-187">5. Dealing with Model Changes</span></span>

<span data-ttu-id="a7c19-188">Nun ist es an der Zeit, einige Änderungen an unserem Modell vorzunehmen, wenn wir diese Änderungen vornehmen, müssen wir auch das Datenbankschema aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="a7c19-188">Now it’s time to make some changes to our model, when we make these changes we also need to update the database schema.</span></span>

<span data-ttu-id="a7c19-189">Wir beginnen mit dem Hinzufügen einer neuen Benutzer Entität zum Modell.</span><span class="sxs-lookup"><span data-stu-id="a7c19-189">We’ll start by adding a new User entity to our model.</span></span>

-   <span data-ttu-id="a7c19-190">Fügen Sie einen neuen **Benutzer** Entitäts Namen mit **Benutzername** als Schlüssel Name und **Zeichenfolge** als Eigenschaftentyp für den Schlüssel hinzu.</span><span class="sxs-lookup"><span data-stu-id="a7c19-190">Add a new **User** entity name with **Username** as the key name and **String** as the property type for the key</span></span>

    ![Benutzer Entität hinzufügen](~/ef6/media/adduserentity.png)

-   <span data-ttu-id="a7c19-192">Klicken Sie mit der rechten Maustaste auf die **username** -Eigenschaft auf der Entwurfs Oberfläche, und wählen Sie **Eigenschaften**aus. in der Eigenschaftenfenster ändern Sie die **MaxLength** -Einstellung in **50**
    *damit werden die Daten, die in username gespeichert werden können, auf 50 Zeichen beschränkt*</span><span class="sxs-lookup"><span data-stu-id="a7c19-192">Right-click on the **Username** property on the design surface and select **Properties**, In the Properties window change the **MaxLength** setting to **50**
*This restricts the data that can be stored in username to 50 characters*</span></span>
-   <span data-ttu-id="a7c19-193">Hinzufügen einer **DisplayName** -Skalareigenschaft zur **Benutzer** Entität</span><span class="sxs-lookup"><span data-stu-id="a7c19-193">Add a **DisplayName** scalar property to the **User** entity</span></span>

<span data-ttu-id="a7c19-194">Wir haben jetzt ein aktualisiertes Modell, und wir können die Datenbank aktualisieren, um den neuen Benutzer Entitätstyp aufzunehmen.</span><span class="sxs-lookup"><span data-stu-id="a7c19-194">We now have an updated model and we are ready to update the database to accommodate our new User entity type.</span></span>

-   <span data-ttu-id="a7c19-195">Klicken Sie mit der rechten Maustaste auf die Entwurfs Oberfläche, und wählen Sie **Datenbank aus Modell generieren... aus**, Entity Framework ein Skript berechnen, um ein Schema basierend auf dem aktualisierten Modell neu zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="a7c19-195">Right-click on the design surface and select **Generate Database from Model…**, Entity Framework will calculate a script to recreate a schema based on the updated model.</span></span>
-   <span data-ttu-id="a7c19-196">Klicken Sie auf **Fertig stellen**.</span><span class="sxs-lookup"><span data-stu-id="a7c19-196">Click **Finish**</span></span>
-   <span data-ttu-id="a7c19-197">Sie erhalten möglicherweise Warnungen zum Überschreiben des vorhandenen DDL-Skripts und die Zuordnung und Speicher Teile des Modells. Klicken Sie für beide Warnungen auf " **Ja** ".</span><span class="sxs-lookup"><span data-stu-id="a7c19-197">You may receive warnings about overwriting the existing DDL script and the mapping and storage parts of the model, click **Yes** for both these warnings</span></span>
-   <span data-ttu-id="a7c19-198">Das aktualisierte SQL-Skript zum Erstellen der Datenbank wird für Sie geöffnet.</span><span class="sxs-lookup"><span data-stu-id="a7c19-198">The updated SQL script to create the database is opened for you</span></span>  
    <span data-ttu-id="a7c19-199">*Das Skript, das generiert wird, löscht alle vorhandenen Tabellen und erstellt das Schema neu. Dies funktioniert möglicherweise für die lokale Entwicklung, ist jedoch nicht für das Übertragen von Änderungen an eine Datenbank geeignet, die bereits bereitgestellt wurde. Wenn Sie Änderungen an einer Datenbank veröffentlichen müssen, die bereits bereitgestellt wurde, müssen Sie das Skript bearbeiten oder ein Schema Vergleichstool verwenden, um ein Migrations Skript zu berechnen.*</span><span class="sxs-lookup"><span data-stu-id="a7c19-199">*The script that is generated will drop all existing tables and then recreate the schema from scratch. This may work for local development but is not a viable for pushing changes to a database that has already been deployed. If you need to publish changes to a database that has already been deployed, you will need to edit the script or use a schema compare tool to calculate a migration script.*</span></span>
-   <span data-ttu-id="a7c19-200">Klicken Sie mit der rechten Maustaste auf das Skript, und wählen Sie **Ausführen**aus. Sie werden aufgefordert, die Datenbank anzugeben, mit der eine Verbindung hergestellt werden soll, und localdb oder SQL Server Express anzugeben, abhängig von der verwendeten Version von Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a7c19-200">Right-click on the script and select **Execute**, you will be prompted to specify the database to connect to, specify LocalDB or SQL Server Express, depending on which version of Visual Studio you are using</span></span>

## <a name="summary"></a><span data-ttu-id="a7c19-201">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="a7c19-201">Summary</span></span>

<span data-ttu-id="a7c19-202">In dieser exemplarischen Vorgehensweise haben wir uns mit Model First Entwicklung beschäftigt, mit der wir ein Modell im EF-Designer erstellen und dann eine Datenbank aus diesem Modell generieren konnten.</span><span class="sxs-lookup"><span data-stu-id="a7c19-202">In this walkthrough we looked at Model First development, which allowed us to create a model in the EF Designer and then generate a database from that model.</span></span> <span data-ttu-id="a7c19-203">Anschließend haben wir das Modell verwendet, um Daten aus der Datenbank zu lesen und zu schreiben.</span><span class="sxs-lookup"><span data-stu-id="a7c19-203">We then used the model to read and write some data from the database.</span></span> <span data-ttu-id="a7c19-204">Schließlich haben wir das Modell aktualisiert und dann das Datenbankschema neu erstellt, um es mit dem Modell zu vergleichen.</span><span class="sxs-lookup"><span data-stu-id="a7c19-204">Finally, we updated the model and then recreated the database schema to match the model.</span></span>
