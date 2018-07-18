---
title: Self-Tracking Entities Exemplarische Vorgehensweise – EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: b21207c9-1d95-4aa3-ae05-bc5fe300dab0
caps.latest.revision: 3
ms.openlocfilehash: d07ae727ffba60a9296b34b9261acd99f7e8e3b6
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2018
ms.locfileid: "39121171"
---
# <a name="self-tracking-entities-walkthrough"></a><span data-ttu-id="ebd45-102">Self-Tracking Entities Exemplarische Vorgehensweise</span><span class="sxs-lookup"><span data-stu-id="ebd45-102">Self-Tracking Entities Walkthrough</span></span>
> [!IMPORTANT]
> <span data-ttu-id="ebd45-103">Es wird nicht mehr empfohlen, die Vorlage für Entitäten mit Selbstnachverfolgung zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="ebd45-103">We no longer recommend using the self-tracking-entities template.</span></span> <span data-ttu-id="ebd45-104">Die Vorlage ist nur für die Unterstützung vorhandener Anwendungen weiterhin verfügbar.</span><span class="sxs-lookup"><span data-stu-id="ebd45-104">It will only continue to be available to support existing applications.</span></span> <span data-ttu-id="ebd45-105">Wenn für Ihre Anwendung die Arbeit mit unverbundenen Diagrammen von Entitäten erforderlich ist, sollten Sie daher Alternativen erwägen, wie z.B. [nachverfolgbare Entitäten](http://trackableentities.github.io/). Diese Technologie ähnelt den Entitäten mit Selbstnachverfolgung und wird von der Community aktiver entwickelt. Alternativ dazu können Sie auch benutzerdefinierten Code mithilfe von APIs auf niedriger Ebene zur Änderungsnachverfolgung schreiben.</span><span class="sxs-lookup"><span data-stu-id="ebd45-105">If your application requires working with disconnected graphs of entities, consider other alternatives such as [Trackable Entities](http://trackableentities.github.io/), which is a technology similar to Self-Tracking-Entities that is more actively developed by the community, or writing custom code using the low-level change tracking APIs.</span></span>

<span data-ttu-id="ebd45-106">Diese exemplarische Vorgehensweise veranschaulicht das Szenario, in dem ein Windows Communication Foundation (WCF)-Dienst einen Vorgang verfügbar macht, der ein Diagramm für die Entität zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="ebd45-106">This walkthrough demonstrates the scenario in which a Windows Communication Foundation (WCF) service exposes an operation that returns an entity graph.</span></span> <span data-ttu-id="ebd45-107">Als Nächstes eine Clientanwendung das Diagramm bearbeitet und übermittelt die Änderungen auf einen Dienstvorgang, der überprüft und die Updates in einer Datenbank mithilfe von Entity Framework speichert.</span><span class="sxs-lookup"><span data-stu-id="ebd45-107">Next, a client application manipulates that graph and submits the modifications to a service operation that validates and saves the updates to a database using Entity Framework.</span></span>

<span data-ttu-id="ebd45-108">Vor dem Durchführen dieser exemplarischen Vorgehensweise stellen Sie sicher, dass Sie lesen die [Self-Tracking Entities](index.md) Seite.</span><span class="sxs-lookup"><span data-stu-id="ebd45-108">Before completing this walkthrough make sure you read the [Self-Tracking Entities](index.md) page.</span></span>

<span data-ttu-id="ebd45-109">Diese exemplarische Vorgehensweise umfasst die folgenden Aktionen:</span><span class="sxs-lookup"><span data-stu-id="ebd45-109">This walkthrough completes the following actions:</span></span>

-   <span data-ttu-id="ebd45-110">Erstellt eine Datenbank auf.</span><span class="sxs-lookup"><span data-stu-id="ebd45-110">Creates a database to access.</span></span>
-   <span data-ttu-id="ebd45-111">Erstellt eine Klassenbibliothek mit dem Modell an.</span><span class="sxs-lookup"><span data-stu-id="ebd45-111">Creates a class library that contains the model.</span></span>
-   <span data-ttu-id="ebd45-112">Austausch von Bereitstellungen der Self-Tracking Entity-Generator-Vorlage.</span><span class="sxs-lookup"><span data-stu-id="ebd45-112">Swaps to the Self-Tracking Entity Generator template.</span></span>
-   <span data-ttu-id="ebd45-113">Wird die Entitätsklassen in ein separates Projekt verschoben.</span><span class="sxs-lookup"><span data-stu-id="ebd45-113">Moves the entity classes to a separate project.</span></span>
-   <span data-ttu-id="ebd45-114">Erstellt einen WCF-Dienst, der Vorgänge zum Abfragen und Speichern von Entitäten bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="ebd45-114">Creates a WCF service that exposes operations to query and save entities.</span></span>
-   <span data-ttu-id="ebd45-115">Erstellt Anwendungen (Konsole und WPF), die den Dienst zu nutzen.</span><span class="sxs-lookup"><span data-stu-id="ebd45-115">Creates client applications (Console and WPF) that consume the service.</span></span>

<span data-ttu-id="ebd45-116">Database First in dieser exemplarischen Vorgehensweise verwendet, aber die gleichen Techniken gelten auch für die Model First.</span><span class="sxs-lookup"><span data-stu-id="ebd45-116">We'll use Database First in this walkthrough but the same techniques apply equally to Model First.</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="ebd45-117">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="ebd45-117">Pre-Requisites</span></span>

<span data-ttu-id="ebd45-118">Zum Durchführen dieser exemplarischen Vorgehensweise benötigen Sie eine aktuelle Version von Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ebd45-118">To complete this walkthrough you will need a recent version of Visual Studio.</span></span>

## <a name="create-a-database"></a><span data-ttu-id="ebd45-119">Erstellen Sie eine Datenbank</span><span class="sxs-lookup"><span data-stu-id="ebd45-119">Create a Database</span></span>

<span data-ttu-id="ebd45-120">Der Datenbankserver, der mit Visual Studio installiert ist, unterscheidet sich abhängig von der Version von Visual Studio, die Sie installiert haben:</span><span class="sxs-lookup"><span data-stu-id="ebd45-120">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="ebd45-121">Wenn Sie Visual Studio 2012 verwenden, wird Sie eine LocalDB-Datenbank erstellen.</span><span class="sxs-lookup"><span data-stu-id="ebd45-121">If you are using Visual Studio 2012 then you'll be creating a LocalDB database.</span></span>
-   <span data-ttu-id="ebd45-122">Wenn Sie Visual Studio 2010 verwenden erstellen Sie eine SQL Express-Datenbank.</span><span class="sxs-lookup"><span data-stu-id="ebd45-122">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>

<span data-ttu-id="ebd45-123">Wir jetzt, und Erstellen der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="ebd45-123">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="ebd45-124">Öffnen Sie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ebd45-124">Open Visual Studio</span></span>
-   <span data-ttu-id="ebd45-125">**Ansicht – Profiler -&gt; Server-Explorer**</span><span class="sxs-lookup"><span data-stu-id="ebd45-125">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="ebd45-126">Klicken Sie mit der rechten Maustaste auf **Datenverbindungen -&gt; Verbindung hinzufügen...**</span><span class="sxs-lookup"><span data-stu-id="ebd45-126">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="ebd45-127">Wenn Sie vor dem müssen auswählen, im Server-Explorer mit einer Datenbank verbunden haben **Microsoft SQL Server** als Datenquelle</span><span class="sxs-lookup"><span data-stu-id="ebd45-127">If you haven’t connected to a database from Server Explorer before you’ll need to select **Microsoft SQL Server** as the data source</span></span>
-   <span data-ttu-id="ebd45-128">Eine Verbindung mit LocalDB oder SQL Express, je nachdem, welches Sie installiert haben</span><span class="sxs-lookup"><span data-stu-id="ebd45-128">Connect to either LocalDB or SQL Express, depending on which one you have installed</span></span>
-   <span data-ttu-id="ebd45-129">Geben Sie **STESample** als Datenbankname</span><span class="sxs-lookup"><span data-stu-id="ebd45-129">Enter **STESample** as the database name</span></span>
-   <span data-ttu-id="ebd45-130">Wählen Sie **OK** und Sie werden gefragt, ob Sie eine neue Datenbank, die auf erstellen möchten **Ja**</span><span class="sxs-lookup"><span data-stu-id="ebd45-130">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>
-   <span data-ttu-id="ebd45-131">Die neue Datenbank wird jetzt im Server-Explorer angezeigt.</span><span class="sxs-lookup"><span data-stu-id="ebd45-131">The new database will now appear in Server Explorer</span></span>
-   <span data-ttu-id="ebd45-132">Bei Verwendung von Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="ebd45-132">If you are using Visual Studio 2012</span></span>
    -   <span data-ttu-id="ebd45-133">Mit der rechten Maustaste auf die Datenbank im Server-Explorer, und wählen Sie **neue Abfrage**</span><span class="sxs-lookup"><span data-stu-id="ebd45-133">Right-click on the database in Server Explorer and select **New Query**</span></span>
    -   <span data-ttu-id="ebd45-134">Kopieren Sie die folgende SQL-Anweisung in die neue Abfrage, und klicken Sie dann mit der rechten Maustaste auf die Abfrage, und wählen **ausführen**</span><span class="sxs-lookup"><span data-stu-id="ebd45-134">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>
-   <span data-ttu-id="ebd45-135">Wenn Sie Visual Studio 2010 verwenden</span><span class="sxs-lookup"><span data-stu-id="ebd45-135">If you are using Visual Studio 2010</span></span>
    -   <span data-ttu-id="ebd45-136">Wählen Sie **Daten per Push –&gt; Transact-SQL-Editor –&gt; neue Abfrageverbindung...**</span><span class="sxs-lookup"><span data-stu-id="ebd45-136">Select **Data -&gt; Transact SQL Editor -&gt; New Query Connection...**</span></span>
    -   <span data-ttu-id="ebd45-137">Geben Sie **.\\ SQLEXPRESS** als Server ein, und klicken Sie auf **OK**</span><span class="sxs-lookup"><span data-stu-id="ebd45-137">Enter **.\\SQLEXPRESS** as the server name and click **OK**</span></span>
    -   <span data-ttu-id="ebd45-138">Wählen Sie die **STESample** Datenbank aus der Dropdownliste unten am oberen Rand des Abfrage-Editors</span><span class="sxs-lookup"><span data-stu-id="ebd45-138">Select the **STESample** database from the drop down at the top of the query editor</span></span>
    -   <span data-ttu-id="ebd45-139">Kopieren Sie die folgende SQL-Anweisung in die neue Abfrage, und klicken Sie dann mit der rechten Maustaste auf die Abfrage, und wählen **SQL ausführen**</span><span class="sxs-lookup"><span data-stu-id="ebd45-139">Copy the following SQL into the new query, then right-click on the query and select **Execute SQL**</span></span>

``` SQL
    CREATE TABLE [dbo].[Blogs] (
        [BlogId] INT IDENTITY (1, 1) NOT NULL,
        [Name] NVARCHAR (200) NULL,
        [Url]  NVARCHAR (200) NULL,
        CONSTRAINT [PK_dbo.Blogs] PRIMARY KEY CLUSTERED ([BlogId] ASC)
    );

    CREATE TABLE [dbo].[Posts] (
        [PostId] INT IDENTITY (1, 1) NOT NULL,
        [Title] NVARCHAR (200) NULL,
        [Content] NTEXT NULL,
        [BlogId] INT NOT NULL,
        CONSTRAINT [PK_dbo.Posts] PRIMARY KEY CLUSTERED ([PostId] ASC),
        CONSTRAINT [FK_dbo.Posts_dbo.Blogs_BlogId] FOREIGN KEY ([BlogId]) REFERENCES [dbo].[Blogs] ([BlogId]) ON DELETE CASCADE
    );

    SET IDENTITY_INSERT [dbo].[Blogs] ON
    INSERT INTO [dbo].[Blogs] ([BlogId], [Name], [Url]) VALUES (1, N'ADO.NET Blog', N'blogs.msdn.com/adonet')
    SET IDENTITY_INSERT [dbo].[Blogs] OFF
    INSERT INTO [dbo].[Posts] ([Title], [Content], [BlogId]) VALUES (N'Intro to EF', N'Interesting stuff...', 1)
    INSERT INTO [dbo].[Posts] ([Title], [Content], [BlogId]) VALUES (N'What is New', N'More interesting stuff...', 1)
```

## <a name="create-the-model"></a><span data-ttu-id="ebd45-140">Erstellen des Modells</span><span class="sxs-lookup"><span data-stu-id="ebd45-140">Create the Model</span></span>

<span data-ttu-id="ebd45-141">Eingerichtet ist, benötigen wir zunächst ein Projekt für das Modell.</span><span class="sxs-lookup"><span data-stu-id="ebd45-141">First up, we need a project to put the model in.</span></span>

-   <span data-ttu-id="ebd45-142">**Datei -&gt; neu –&gt; Projekt...**</span><span class="sxs-lookup"><span data-stu-id="ebd45-142">**File -&gt; New -&gt; Project...**</span></span>
-   <span data-ttu-id="ebd45-143">Wählen Sie **Visual C\#**  im linken Bereich und dann **-Klassenbibliothek**</span><span class="sxs-lookup"><span data-stu-id="ebd45-143">Select **Visual C\#** from the left pane and then **Class Library**</span></span>
-   <span data-ttu-id="ebd45-144">Geben Sie **STESample** als ein, und klicken Sie auf **OK**</span><span class="sxs-lookup"><span data-stu-id="ebd45-144">Enter **STESample** as the name and click **OK**</span></span>

<span data-ttu-id="ebd45-145">Jetzt erstellen wir ein einfaches Modell im EF Designer den Zugriff auf unsere Datenbank:</span><span class="sxs-lookup"><span data-stu-id="ebd45-145">Now we'll create a simple model in the EF Designer to access our database:</span></span>

-   <span data-ttu-id="ebd45-146">**Projekt -&gt; neues Element hinzufügen...**</span><span class="sxs-lookup"><span data-stu-id="ebd45-146">**Project -&gt; Add New Item...**</span></span>
-   <span data-ttu-id="ebd45-147">Wählen Sie **Daten** im linken Bereich und dann **ADO.NET Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="ebd45-147">Select **Data** from the left pane and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="ebd45-148">Geben Sie **BloggingModel** als ein, und klicken Sie auf **OK**</span><span class="sxs-lookup"><span data-stu-id="ebd45-148">Enter **BloggingModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="ebd45-149">Wählen Sie **aus Datenbank generieren** , und klicken Sie auf **weiter**</span><span class="sxs-lookup"><span data-stu-id="ebd45-149">Select **Generate from database** and click **Next**</span></span>
-   <span data-ttu-id="ebd45-150">Geben Sie die Verbindungsinformationen für die Datenbank, die Sie im vorherigen Abschnitt erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="ebd45-150">Enter the connection information for the database that you created in the previous section</span></span>
-   <span data-ttu-id="ebd45-151">Geben Sie **BloggingContext** als Namen für die Verbindungszeichenfolge und klicken Sie auf **weiter**</span><span class="sxs-lookup"><span data-stu-id="ebd45-151">Enter **BloggingContext** as the name for the connection string and click **Next**</span></span>
-   <span data-ttu-id="ebd45-152">Aktivieren Sie das Kontrollkästchen neben **Tabellen** , und klicken Sie auf **Fertig stellen**</span><span class="sxs-lookup"><span data-stu-id="ebd45-152">Check the box next to **Tables** and click **Finish**</span></span>

## <a name="swap-to-ste-code-generation"></a><span data-ttu-id="ebd45-153">Wechseln Sie in die STE-Codegenerierung</span><span class="sxs-lookup"><span data-stu-id="ebd45-153">Swap to STE Code Generation</span></span>

<span data-ttu-id="ebd45-154">Nun müssen wir die standardmäßige codegenerierung und wechseln zu Self-Tracking Entities deaktivieren.</span><span class="sxs-lookup"><span data-stu-id="ebd45-154">Now we need to disable the default code generation and swap to Self-Tracking Entities.</span></span>

### <a name="if-you-are-using-visual-studio-2012"></a><span data-ttu-id="ebd45-155">Bei Verwendung von Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="ebd45-155">If you are using Visual Studio 2012</span></span>

-   <span data-ttu-id="ebd45-156">Erweitern Sie **BloggingModel.edmx** in **Projektmappen-Explorer** und löschen Sie die **BloggingModel.tt** und **BloggingModel.Context.tt** 
     *Dadurch wird die standardmäßige codegenerierung deaktiviert.*</span><span class="sxs-lookup"><span data-stu-id="ebd45-156">Expand **BloggingModel.edmx** in **Solution Explorer** and delete the **BloggingModel.tt** and **BloggingModel.Context.tt**
*This will disable the default code generation*</span></span>
-   <span data-ttu-id="ebd45-157">Mit der rechten Maustaste auf die EF-Designer, Entwurfsoberfläche, und wählen eines leeren Bereichs **Codegenerierungselement hinzufügen...**</span><span class="sxs-lookup"><span data-stu-id="ebd45-157">Right-click an empty area on the EF Designer surface and select **Add Code Generation Item...**</span></span>
-   <span data-ttu-id="ebd45-158">Wählen Sie **Online** aus dem linken Bereich und suchen Sie nach **AL-Generator**</span><span class="sxs-lookup"><span data-stu-id="ebd45-158">Select **Online** from the left pane and search for **STE Generator**</span></span>
-   <span data-ttu-id="ebd45-159">Wählen Sie die **AL-Generator für C\#**  Vorlage, geben Sie **STETemplate** als ein, und klicken Sie auf **hinzufügen**</span><span class="sxs-lookup"><span data-stu-id="ebd45-159">Select the **STE Generator for C\#** template, enter **STETemplate** as the name and click **Add**</span></span>
-   <span data-ttu-id="ebd45-160">Die **STETemplate.tt** und **STETemplate.Context.tt** Dateien werden unter der Datei BloggingModel.edmx geschachtelte hinzugefügt</span><span class="sxs-lookup"><span data-stu-id="ebd45-160">The **STETemplate.tt** and **STETemplate.Context.tt** files are added nested under the BloggingModel.edmx file</span></span>

### <a name="if-you-are-using-visual-studio-2010"></a><span data-ttu-id="ebd45-161">Wenn Sie Visual Studio 2010 verwenden</span><span class="sxs-lookup"><span data-stu-id="ebd45-161">If you are using Visual Studio 2010</span></span>

-   <span data-ttu-id="ebd45-162">Mit der rechten Maustaste auf die EF-Designer, Entwurfsoberfläche, und wählen eines leeren Bereichs **Codegenerierungselement hinzufügen...**</span><span class="sxs-lookup"><span data-stu-id="ebd45-162">Right-click an empty area on the EF Designer surface and select **Add Code Generation Item...**</span></span>
-   <span data-ttu-id="ebd45-163">Wählen Sie **Code** im linken Bereich und dann **ADO.NET-Entitäts-Generator**</span><span class="sxs-lookup"><span data-stu-id="ebd45-163">Select **Code** from the left pane and then **ADO.NET Self-Tracking Entity Generator**</span></span>
-   <span data-ttu-id="ebd45-164">Geben Sie **STETemplate** als ein, und klicken Sie auf **hinzufügen**</span><span class="sxs-lookup"><span data-stu-id="ebd45-164">Enter **STETemplate** as the name and click **Add**</span></span>
-   <span data-ttu-id="ebd45-165">Die **STETemplate.tt** und **STETemplate.Context.tt** Dateien werden direkt in Ihrem Projekt hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="ebd45-165">The **STETemplate.tt** and **STETemplate.Context.tt** files are added directly to your project</span></span>

## <a name="move-entity-types-into-separate-project"></a><span data-ttu-id="ebd45-166">Entitätstypen in separaten Projekt verschieben</span><span class="sxs-lookup"><span data-stu-id="ebd45-166">Move Entity Types into Separate Project</span></span>

<span data-ttu-id="ebd45-167">Zur Verwendung von Self-Tracking Entities benötigt die Clientanwendung den Zugriff auf die Entitätsklassen aus unserem Modell generiert.</span><span class="sxs-lookup"><span data-stu-id="ebd45-167">To use Self-Tracking Entities our client application needs access to the entity classes generated from our model.</span></span> <span data-ttu-id="ebd45-168">Da wir nicht, um das gesamte Modell an die Clientanwendung verfügbar zu machen möchten werden wir die Entitätsklassen in ein separates Projekt verschoben.</span><span class="sxs-lookup"><span data-stu-id="ebd45-168">Because we don't want to expose the whole model to the client application we're going to move the entity classes into a separate project.</span></span>

<span data-ttu-id="ebd45-169">Der erste Schritt ist zum Generieren von Entitätsklassen in das vorhandene Projekt zu beenden:</span><span class="sxs-lookup"><span data-stu-id="ebd45-169">The first step is to stop generating entity classes in the existing project:</span></span>

-   <span data-ttu-id="ebd45-170">Mit der rechten Maustaste auf **STETemplate.tt** in **Projektmappen-Explorer** , und wählen Sie **Eigenschaften**</span><span class="sxs-lookup"><span data-stu-id="ebd45-170">Right-click on **STETemplate.tt** in **Solution Explorer** and select **Properties**</span></span>
-   <span data-ttu-id="ebd45-171">In der **Eigenschaften** Fenster-clear **TextTemplatingFileGenerator** aus der **CustomTool** Eigenschaft</span><span class="sxs-lookup"><span data-stu-id="ebd45-171">In the **Properties** window clear **TextTemplatingFileGenerator** from the **CustomTool** property</span></span>
-   <span data-ttu-id="ebd45-172">Erweitern Sie **STETemplate.tt** in **Projektmappen-Explorer** und löschen Sie alle Dateien, die geschachtelt sind</span><span class="sxs-lookup"><span data-stu-id="ebd45-172">Expand **STETemplate.tt** in **Solution Explorer** and delete all files nested under it</span></span>

<span data-ttu-id="ebd45-173">Als Nächstes werden wir ein neues Projekt hinzufügen und Generieren von Entitätsklassen darin</span><span class="sxs-lookup"><span data-stu-id="ebd45-173">Next, we are going to add a new project and generate the entity classes in it</span></span>

-   <span data-ttu-id="ebd45-174">**Datei -&gt; hinzufügen –&gt; Projekt...**</span><span class="sxs-lookup"><span data-stu-id="ebd45-174">**File -&gt; Add -&gt; Project...**</span></span>
-   <span data-ttu-id="ebd45-175">Wählen Sie **Visual C\#**  im linken Bereich und dann **-Klassenbibliothek**</span><span class="sxs-lookup"><span data-stu-id="ebd45-175">Select **Visual C\#** from the left pane and then **Class Library**</span></span>
-   <span data-ttu-id="ebd45-176">Geben Sie **STESample.Entities** als ein, und klicken Sie auf **OK**</span><span class="sxs-lookup"><span data-stu-id="ebd45-176">Enter **STESample.Entities** as the name and click **OK**</span></span>
-   <span data-ttu-id="ebd45-177">**Projekt -&gt; vorhandenes Element hinzufügen...**</span><span class="sxs-lookup"><span data-stu-id="ebd45-177">**Project -&gt; Add Existing Item...**</span></span>
-   <span data-ttu-id="ebd45-178">Navigieren Sie zu der **STESample** Projektordner</span><span class="sxs-lookup"><span data-stu-id="ebd45-178">Navigate to the **STESample** project folder</span></span>
-   <span data-ttu-id="ebd45-179">Wählen Sie zum Anzeigen von **alle Dateien (\*.\*)**</span><span class="sxs-lookup"><span data-stu-id="ebd45-179">Select to view **All Files (\*.\*)**</span></span>
-   <span data-ttu-id="ebd45-180">Wählen Sie die **STETemplate.tt** Datei</span><span class="sxs-lookup"><span data-stu-id="ebd45-180">Select the **STETemplate.tt** file</span></span>
-   <span data-ttu-id="ebd45-181">Klicken Sie auf den Dropdownpfeil neben der **hinzufügen** Schaltfläche, und wählen **als Link hinzufügen**</span><span class="sxs-lookup"><span data-stu-id="ebd45-181">Click on the drop down arrow next to the **Add** button and select **Add As Link**</span></span>

    ![AddLinkedTemplate](~/ef6/media/addlinkedtemplate.png)

<span data-ttu-id="ebd45-183">Wir werden auch sicherstellen, dass die Entitätsklassen im selben Namespace wie der Kontext generiert.</span><span class="sxs-lookup"><span data-stu-id="ebd45-183">We're also going to make sure the entity classes get generated in the same namespace as the context.</span></span> <span data-ttu-id="ebd45-184">Dies reduziert lediglich die Anzahl der using-Anweisungen, die wir in unserer Anwendung hinzufügen möchten.</span><span class="sxs-lookup"><span data-stu-id="ebd45-184">This just reduces the number of using statements we need to add throughout our application.</span></span>

-   <span data-ttu-id="ebd45-185">Mit der rechten Maustaste auf die verknüpften **STETemplate.tt** in **Projektmappen-Explorer** , und wählen Sie **Eigenschaften**</span><span class="sxs-lookup"><span data-stu-id="ebd45-185">Right-click on the linked **STETemplate.tt** in **Solution Explorer** and select **Properties**</span></span>
-   <span data-ttu-id="ebd45-186">In der **Eigenschaften** legen **Custom Tool Namespace** zu **STESample**</span><span class="sxs-lookup"><span data-stu-id="ebd45-186">In the **Properties** window set **Custom Tool Namespace** to **STESample**</span></span>

<span data-ttu-id="ebd45-187">Der Code, der von der AL-Vorlage generiert wird, benötigen einen Verweis auf **System.Runtime.Serialization** zum Kompilieren.</span><span class="sxs-lookup"><span data-stu-id="ebd45-187">The code generated by the STE template will need a reference to **System.Runtime.Serialization** in order to compile.</span></span> <span data-ttu-id="ebd45-188">Diese Bibliothek ist erforderlich, für die WCF **DataContract** und **DataMember** Attribute, die für die serialisierbaren Entitätstypen verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="ebd45-188">This library is needed for the WCF **DataContract** and **DataMember** attributes that are used on the serializable entity types.</span></span>

-   <span data-ttu-id="ebd45-189">Klicken Sie mit der rechten Maustaste auf die **STESample.Entities** Projekt **Projektmappen-Explorer** , und wählen Sie **Verweis hinzufügen...**</span><span class="sxs-lookup"><span data-stu-id="ebd45-189">Right click on the **STESample.Entities** project in **Solution Explorer** and select **Add Reference...**</span></span>
    -   <span data-ttu-id="ebd45-190">In Visual Studio 2012 – aktivieren Sie das Kontrollkästchen neben **System.Runtime.Serialization** , und klicken Sie auf **OK**</span><span class="sxs-lookup"><span data-stu-id="ebd45-190">In Visual Studio 2012 - check the box next to **System.Runtime.Serialization** and click **OK**</span></span>
    -   <span data-ttu-id="ebd45-191">Wählen Sie in Visual Studio 2010 - **System.Runtime.Serialization** , und klicken Sie auf **OK**</span><span class="sxs-lookup"><span data-stu-id="ebd45-191">In Visual Studio 2010 - select **System.Runtime.Serialization** and click **OK**</span></span>

<span data-ttu-id="ebd45-192">Schließlich wird das Projekt mit unserem Kontext darin einen Verweis auf den Entitätstypen benötigen.</span><span class="sxs-lookup"><span data-stu-id="ebd45-192">Finally, the project with our context in it will need a reference to the entity types.</span></span>

-   <span data-ttu-id="ebd45-193">Klicken Sie mit der rechten Maustaste auf die **STESample** Projekt **Projektmappen-Explorer** , und wählen Sie **Verweis hinzufügen...**</span><span class="sxs-lookup"><span data-stu-id="ebd45-193">Right click on the **STESample** project in **Solution Explorer** and select **Add Reference...**</span></span>
    -   <span data-ttu-id="ebd45-194">Wählen Sie in Visual Studio 2012 - **Lösung** aktivieren Sie im linken Bereich das Kontrollkästchen neben **STESample.Entities** , und klicken Sie auf **OK**</span><span class="sxs-lookup"><span data-stu-id="ebd45-194">In Visual Studio 2012 - select **Solution** from the left pane, check the box next to **STESample.Entities** and click **OK**</span></span>
    -   <span data-ttu-id="ebd45-195">Wählen Sie in Visual Studio 2010 – die **Projekte** Registerkarte **STESample.Entities** , und klicken Sie auf **OK**</span><span class="sxs-lookup"><span data-stu-id="ebd45-195">In Visual Studio 2010 - select the **Projects** tab, select **STESample.Entities** and click **OK**</span></span>

>[!NOTE]
> <span data-ttu-id="ebd45-196">Eine weitere Möglichkeit zum Verschieben von Entitätstypen in einem separaten Projekt ist, verschieben Sie die Vorlagendatei, anstatt es aus seinem Standardspeicherort zu verknüpfen.</span><span class="sxs-lookup"><span data-stu-id="ebd45-196">Another option for moving the entity types to a separate project is to move the template file, rather than linking it from its default location.</span></span> <span data-ttu-id="ebd45-197">Wenn Sie dies tun, müssen Sie zum Aktualisieren der **InputFile** Variable in der Vorlage, geben Sie den relativen Pfad der Edmx-Datei (in diesem Beispiel, die **... \\BloggingModel.edmx**).</span><span class="sxs-lookup"><span data-stu-id="ebd45-197">If you do this, you will need to update the **inputFile** variable in the template to provide the relative path to the edmx file (in this example that would be **..\\BloggingModel.edmx**).</span></span>

## <a name="create-a-wcf-service"></a><span data-ttu-id="ebd45-198">Erstellen Sie einen WCF-Dienst</span><span class="sxs-lookup"><span data-stu-id="ebd45-198">Create a WCF Service</span></span>

<span data-ttu-id="ebd45-199">Nun ist es Zeit zum Hinzufügen eines WCF-Diensts, um unsere Daten verfügbar zu machen, beginnen wir, indem Sie das Projekt erstellen.</span><span class="sxs-lookup"><span data-stu-id="ebd45-199">Now it's time to add a WCF Service to expose our data, we'll start by creating the project.</span></span>

-   <span data-ttu-id="ebd45-200">**Datei -&gt; hinzufügen –&gt; Projekt...**</span><span class="sxs-lookup"><span data-stu-id="ebd45-200">**File -&gt; Add -&gt; Project...**</span></span>
-   <span data-ttu-id="ebd45-201">Wählen Sie **Visual C\#**  im linken Bereich und dann **WCF-Dienstanwendung**</span><span class="sxs-lookup"><span data-stu-id="ebd45-201">Select **Visual C\#** from the left pane and then **WCF Service Application**</span></span>
-   <span data-ttu-id="ebd45-202">Geben Sie **STESample.Service** als ein, und klicken Sie auf **OK**</span><span class="sxs-lookup"><span data-stu-id="ebd45-202">Enter **STESample.Service** as the name and click **OK**</span></span>
-   <span data-ttu-id="ebd45-203">Hinzufügen eines Verweises auf die **System.Data.Entity** Assembly</span><span class="sxs-lookup"><span data-stu-id="ebd45-203">Add a reference to the **System.Data.Entity** assembly</span></span>
-   <span data-ttu-id="ebd45-204">Hinzufügen eines Verweises auf die **STESample** und **STESample.Entities** Projekte</span><span class="sxs-lookup"><span data-stu-id="ebd45-204">Add a reference to the **STESample** and **STESample.Entities** projects</span></span>

<span data-ttu-id="ebd45-205">Wir müssen die EF-Verbindungszeichenfolge auf dieses Projekt zu kopieren, damit sie zur Laufzeit gefunden wird.</span><span class="sxs-lookup"><span data-stu-id="ebd45-205">We need to copy the EF connection string to this project so that it is found at runtime.</span></span>

-   <span data-ttu-id="ebd45-206">Öffnen der **"App.config"** -Datei für die ** STESample **-Projekt und kopiert die **ConnectionStrings** Element</span><span class="sxs-lookup"><span data-stu-id="ebd45-206">Open the **App.Config** file for the **STESample **project and copy the **connectionStrings** element</span></span>
-   <span data-ttu-id="ebd45-207">Fügen Sie der **ConnectionStrings** -Element als untergeordnetes Element des der **Konfiguration** Element der **"Web.config"** Datei die **STESample.Service** Projekt</span><span class="sxs-lookup"><span data-stu-id="ebd45-207">Paste the **connectionStrings** element as a child element of the **configuration** element of the **Web.Config** file in the **STESample.Service** project</span></span>

<span data-ttu-id="ebd45-208">Jetzt ist es Zeit, um den Dienst zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="ebd45-208">Now it's time to implement the actual service.</span></span>

-   <span data-ttu-id="ebd45-209">Open **IService1.cs** und Ersetzen Sie den Inhalt durch den folgenden Code</span><span class="sxs-lookup"><span data-stu-id="ebd45-209">Open **IService1.cs** and replace the contents with the following code</span></span>

``` csharp
    using System.Collections.Generic;
    using System.ServiceModel;

    namespace STESample.Service
    {
        [ServiceContract]
        public interface IService1
        {
            [OperationContract]
            List<Blog> GetBlogs();

            [OperationContract]
            void UpdateBlog(Blog blog);
        }
    }
```

-   <span data-ttu-id="ebd45-210">Open **Service1.svc** und Ersetzen Sie den Inhalt durch den folgenden Code</span><span class="sxs-lookup"><span data-stu-id="ebd45-210">Open **Service1.svc** and replace the contents with the following code</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Data;
    using System.Linq;

    namespace STESample.Service
    {
        public class Service1 : IService1
        {
            /// <summary>
            /// Gets all the Blogs and related Posts.
            /// </summary>
            public List<Blog> GetBlogs()
            {
                using (BloggingContext context = new BloggingContext())
                {
                    return context.Blogs.Include("Posts").ToList();
                }
            }

            /// <summary>
            /// Updates Blog and its related Posts.
            /// </summary>
            public void UpdateBlog(Blog blog)
            {
                using (BloggingContext context = new BloggingContext())
                {
                    try
                    {
                        // TODO: Perform validation on the updated order before applying the changes.

                        // The ApplyChanges method examines the change tracking information
                        // contained in the graph of self-tracking entities to infer the set of operations
                        // that need to be performed to reflect the changes in the database.
                        context.Blogs.ApplyChanges(blog);
                        context.SaveChanges();

                    }
                    catch (UpdateException)
                    {
                        // To avoid propagating exception messages that contain sensitive data to the client tier
                        // calls to ApplyChanges and SaveChanges should be wrapped in exception handling code.
                        throw new InvalidOperationException("Failed to update. Try your request again.");
                    }
                }
            }        
        }
    }
```

## <a name="consume-the-service-from-a-console-application"></a><span data-ttu-id="ebd45-211">Nutzen Sie den Dienst aus einer Konsolenanwendung</span><span class="sxs-lookup"><span data-stu-id="ebd45-211">Consume the Service from a Console Application</span></span>

<span data-ttu-id="ebd45-212">Wir erstellen eine Konsolenanwendung, die unseren Dienst verwendet.</span><span class="sxs-lookup"><span data-stu-id="ebd45-212">Let's create a console application that uses our service.</span></span>

-   <span data-ttu-id="ebd45-213">**Datei -&gt; neu –&gt; Projekt...**</span><span class="sxs-lookup"><span data-stu-id="ebd45-213">**File -&gt; New -&gt; Project...**</span></span>
-   <span data-ttu-id="ebd45-214">Wählen Sie **Visual C\#**  im linken Bereich und dann **-Konsolenanwendung**</span><span class="sxs-lookup"><span data-stu-id="ebd45-214">Select **Visual C\#** from the left pane and then **Console Application**</span></span>
-   <span data-ttu-id="ebd45-215">Geben Sie **STESample.ConsoleTest** als ein, und klicken Sie auf **OK**</span><span class="sxs-lookup"><span data-stu-id="ebd45-215">Enter **STESample.ConsoleTest** as the name and click **OK**</span></span>
-   <span data-ttu-id="ebd45-216">Hinzufügen eines Verweises auf die **STESample.Entities** Projekt</span><span class="sxs-lookup"><span data-stu-id="ebd45-216">Add a reference to the **STESample.Entities** project</span></span>

<span data-ttu-id="ebd45-217">Wir benötigen einen Dienstverweis an den WCF-Dienst</span><span class="sxs-lookup"><span data-stu-id="ebd45-217">We need a service reference to our WCF service</span></span>

-   <span data-ttu-id="ebd45-218">Mit der rechten Maustaste die **STESample.ConsoleTest** Projekt **Projektmappen-Explorer** , und wählen Sie **Dienstverweis hinzufügen...**</span><span class="sxs-lookup"><span data-stu-id="ebd45-218">Right-click the **STESample.ConsoleTest** project in **Solution Explorer** and select **Add Service Reference...**</span></span>
-   <span data-ttu-id="ebd45-219">Klicken Sie auf **ermitteln**</span><span class="sxs-lookup"><span data-stu-id="ebd45-219">Click **Discover**</span></span>
-   <span data-ttu-id="ebd45-220">Geben Sie **BloggingService** als Namespace, und klicken Sie auf **OK**</span><span class="sxs-lookup"><span data-stu-id="ebd45-220">Enter **BloggingService** as the namespace and click **OK**</span></span>

<span data-ttu-id="ebd45-221">Jetzt schreiben wir Code, um den Dienst zu nutzen.</span><span class="sxs-lookup"><span data-stu-id="ebd45-221">Now we can write some code to consume the service.</span></span>

-   <span data-ttu-id="ebd45-222">Open **"Program.cs"** und Ersetzen Sie den Inhalt durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="ebd45-222">Open **Program.cs** and replace the contents with the following code.</span></span>

``` csharp
    using STESample.ConsoleTest.BloggingService;
    using System;
    using System.Linq;

    namespace STESample.ConsoleTest
    {
        class Program
        {
            static void Main(string[] args)
            {
                // Print out the data before we change anything
                Console.WriteLine("Initial Data:");
                DisplayBlogsAndPosts();

                // Add a new Blog and some Posts
                AddBlogAndPost();
                Console.WriteLine("After Adding:");
                DisplayBlogsAndPosts();

                // Modify the Blog and one of its Posts
                UpdateBlogAndPost();
                Console.WriteLine("After Update:");
                DisplayBlogsAndPosts();

                // Delete the Blog and its Posts
                DeleteBlogAndPost();
                Console.WriteLine("After Delete:");
                DisplayBlogsAndPosts();

                Console.WriteLine("Press any key to exit...");
                Console.ReadKey();
            }

            static void DisplayBlogsAndPosts()
            {
                using (var service = new Service1Client())
                {
                    // Get all Blogs (and Posts) from the service
                    // and print them to the console
                    var blogs = service.GetBlogs();
                    foreach (var blog in blogs)
                    {
                        Console.WriteLine(blog.Name);
                        foreach (var post in blog.Posts)
                        {
                            Console.WriteLine(" - {0}", post.Title);
                        }
                    }
                }

                Console.WriteLine();
                Console.WriteLine();
            }

            static void AddBlogAndPost()
            {
                using (var service = new Service1Client())
                {
                    // Create a new Blog with a couple of Posts
                    var newBlog = new Blog
                    {
                        Name = "The New Blog",
                        Posts =
                        {
                            new Post { Title = "Welcome to the new blog"},
                            new Post { Title = "What's new on the new blog"}
                        }
                    };

                    // Save the changes using the service
                    service.UpdateBlog(newBlog);
                }
            }

            static void UpdateBlogAndPost()
            {
                using (var service = new Service1Client())
                {
                    // Get all the Blogs
                    var blogs = service.GetBlogs();

                    // Use LINQ to Objects to find The New Blog
                    var blog = blogs.First(b => b.Name == "The New Blog");

                    // Update the Blogs name
                    blog.Name = "The Not-So-New Blog";

                    // Update one of the related posts
                    blog.Posts.First().Content = "Some interesting content...";

                    // Save the changes using the service
                    service.UpdateBlog(blog);
                }
            }

            static void DeleteBlogAndPost()
            {
                using (var service = new Service1Client())
                {
                    // Get all the Blogs
                    var blogs = service.GetBlogs();

                    // Use LINQ to Objects to find The Not-So-New Blog
                    var blog = blogs.First(b => b.Name == "The Not-So-New Blog");

                    // Mark all related Posts for deletion
                    // We need to call ToList because each Post will be removed from the
                    // Posts collection when we call MarkAsDeleted
                    foreach (var post in blog.Posts.ToList())
                    {
                        post.MarkAsDeleted();
                    }

                    // Mark the Blog for deletion
                    blog.MarkAsDeleted();

                    // Save the changes using the service
                    service.UpdateBlog(blog);
                }
            }
        }
    }
```

<span data-ttu-id="ebd45-223">Sie können die Anwendung jetzt ausführen, um sie in Aktion zu sehen.</span><span class="sxs-lookup"><span data-stu-id="ebd45-223">You can now run the application to see it in action.</span></span>

-   <span data-ttu-id="ebd45-224">Mit der rechten Maustaste die **STESample.ConsoleTest** Projekt **Projektmappen-Explorer** , und wählen Sie **Debuggen -&gt; neue Instanz starten**</span><span class="sxs-lookup"><span data-stu-id="ebd45-224">Right-click the **STESample.ConsoleTest** project in **Solution Explorer** and select **Debug -&gt; Start new instance**</span></span>

<span data-ttu-id="ebd45-225">Sie sehen die folgende Ausgabe, wenn die Anwendung ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="ebd45-225">You'll see the following output when the application executes.</span></span>

```
Initial Data:
ADO.NET Blog
- Intro to EF
- What is New

After Adding:
ADO.NET Blog
- Intro to EF
- What is New
The New Blog
- Welcome to the new blog
- What's new on the new blog

After Update:
ADO.NET Blog
- Intro to EF
- What is New
The Not-So-New Blog
- Welcome to the new blog
- What's new on the new blog

After Delete:
ADO.NET Blog
- Intro to EF
- What is New

Press any key to exit...
```

## <a name="consume-the-service-from-a-wpf-application"></a><span data-ttu-id="ebd45-226">Nutzen Sie den Dienst aus einer WPF-Anwendung</span><span class="sxs-lookup"><span data-stu-id="ebd45-226">Consume the Service from a WPF Application</span></span>

<span data-ttu-id="ebd45-227">Erstellen wir eine WPF-Anwendung, die unseren Dienst verwendet.</span><span class="sxs-lookup"><span data-stu-id="ebd45-227">Let's create a WPF application that uses our service.</span></span>

-   <span data-ttu-id="ebd45-228">**Datei -&gt; neu –&gt; Projekt...**</span><span class="sxs-lookup"><span data-stu-id="ebd45-228">**File -&gt; New -&gt; Project...**</span></span>
-   <span data-ttu-id="ebd45-229">Wählen Sie **Visual C\#**  im linken Bereich und dann **WPF-Anwendung**</span><span class="sxs-lookup"><span data-stu-id="ebd45-229">Select **Visual C\#** from the left pane and then **WPF Application**</span></span>
-   <span data-ttu-id="ebd45-230">Geben Sie **STESample.WPFTest** als ein, und klicken Sie auf **OK**</span><span class="sxs-lookup"><span data-stu-id="ebd45-230">Enter **STESample.WPFTest** as the name and click **OK**</span></span>
-   <span data-ttu-id="ebd45-231">Hinzufügen eines Verweises auf die **STESample.Entities** Projekt</span><span class="sxs-lookup"><span data-stu-id="ebd45-231">Add a reference to the **STESample.Entities** project</span></span>

<span data-ttu-id="ebd45-232">Wir benötigen einen Dienstverweis an den WCF-Dienst</span><span class="sxs-lookup"><span data-stu-id="ebd45-232">We need a service reference to our WCF service</span></span>

-   <span data-ttu-id="ebd45-233">Mit der rechten Maustaste die **STESample.WPFTest** Projekt **Projektmappen-Explorer** , und wählen Sie **Dienstverweis hinzufügen...**</span><span class="sxs-lookup"><span data-stu-id="ebd45-233">Right-click the **STESample.WPFTest** project in **Solution Explorer** and select **Add Service Reference...**</span></span>
-   <span data-ttu-id="ebd45-234">Klicken Sie auf **ermitteln**</span><span class="sxs-lookup"><span data-stu-id="ebd45-234">Click **Discover**</span></span>
-   <span data-ttu-id="ebd45-235">Geben Sie **BloggingService** als Namespace, und klicken Sie auf **OK**</span><span class="sxs-lookup"><span data-stu-id="ebd45-235">Enter **BloggingService** as the namespace and click **OK**</span></span>

<span data-ttu-id="ebd45-236">Jetzt schreiben wir Code, um den Dienst zu nutzen.</span><span class="sxs-lookup"><span data-stu-id="ebd45-236">Now we can write some code to consume the service.</span></span>

-   <span data-ttu-id="ebd45-237">Open **"MainWindow.xaml"** und Ersetzen Sie den Inhalt durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="ebd45-237">Open **MainWindow.xaml** and replace the contents with the following code.</span></span>

``` xaml
    <Window
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:STESample="clr-namespace:STESample;assembly=STESample.Entities"
        mc:Ignorable="d" x:Class="STESample.WPFTest.MainWindow"
        Title="MainWindow" Height="350" Width="525" Loaded="Window_Loaded">

        <Window.Resources>
            <CollectionViewSource
                x:Key="blogViewSource"
                d:DesignSource="{d:DesignInstance {x:Type STESample:Blog}, CreateList=True}"/>
            <CollectionViewSource
                x:Key="blogPostsViewSource"
                Source="{Binding Posts, Source={StaticResource blogViewSource}}"/>
        </Window.Resources>

        <Grid DataContext="{StaticResource blogViewSource}">
            <DataGrid AutoGenerateColumns="False" EnableRowVirtualization="True"
                      ItemsSource="{Binding}" Margin="10,10,10,179">
                <DataGrid.Columns>
                    <DataGridTextColumn Binding="{Binding BlogId}" Header="Id" Width="Auto" IsReadOnly="True" />
                    <DataGridTextColumn Binding="{Binding Name}" Header="Name" Width="Auto"/>
                    <DataGridTextColumn Binding="{Binding Url}" Header="Url" Width="Auto"/>
                </DataGrid.Columns>
            </DataGrid>
            <DataGrid AutoGenerateColumns="False" EnableRowVirtualization="True"
                      ItemsSource="{Binding Source={StaticResource blogPostsViewSource}}" Margin="10,145,10,38">
                <DataGrid.Columns>
                    <DataGridTextColumn Binding="{Binding PostId}" Header="Id" Width="Auto"  IsReadOnly="True"/>
                    <DataGridTextColumn Binding="{Binding Title}" Header="Title" Width="Auto"/>
                    <DataGridTextColumn Binding="{Binding Content}" Header="Content" Width="Auto"/>
                </DataGrid.Columns>
            </DataGrid>
            <Button Width="68" Height="23" HorizontalAlignment="Right" VerticalAlignment="Bottom"
                    Margin="0,0,10,10" Click="buttonSave_Click">Save</Button>
        </Grid>
    </Window>
```

-   <span data-ttu-id="ebd45-238">Öffnen den Code-behind für MainWindow-Element (**"MainWindow.Xaml.cs"**), und Ersetzen Sie den Inhalt durch den folgenden Code</span><span class="sxs-lookup"><span data-stu-id="ebd45-238">Open the code behind for MainWindow (**MainWindow.xaml.cs**) and replace the contents with the following code</span></span>

``` csharp
    using STESample.WPFTest.BloggingService;
    using System.Collections.Generic;
    using System.Linq;
    using System.Windows;
    using System.Windows.Data;

    namespace STESample.WPFTest
    {
        public partial class MainWindow : Window
        {
            public MainWindow()
            {
                InitializeComponent();
            }

            private void Window_Loaded(object sender, RoutedEventArgs e)
            {
                using (var service = new Service1Client())
                {
                    // Find the view source for Blogs and populate it with all Blogs (and related Posts)
                    // from the Service. The default editing functionality of WPF will allow the objects
                    // to be manipulated on the screen.
                    var blogsViewSource = (CollectionViewSource)this.FindResource("blogViewSource");
                    blogsViewSource.Source = service.GetBlogs().ToList();
                }
            }

            private void buttonSave_Click(object sender, RoutedEventArgs e)
            {
                using (var service = new Service1Client())
                {
                    // Get the blogs that are bound to the screen
                    var blogsViewSource = (CollectionViewSource)this.FindResource("blogViewSource");
                    var blogs = (List<Blog>)blogsViewSource.Source;

                    // Save all Blogs and related Posts
                    foreach (var blog in blogs)
                    {
                        service.UpdateBlog(blog);
                    }

                    // Re-query for data to get database-generated keys etc.
                    blogsViewSource.Source = service.GetBlogs().ToList();
                }
            }
        }
    }
```

<span data-ttu-id="ebd45-239">Sie können die Anwendung jetzt ausführen, um sie in Aktion zu sehen.</span><span class="sxs-lookup"><span data-stu-id="ebd45-239">You can now run the application to see it in action.</span></span>

-   <span data-ttu-id="ebd45-240">Mit der rechten Maustaste die **STESample.WPFTest** Projekt **Projektmappen-Explorer** , und wählen Sie **Debuggen -&gt; neue Instanz starten**</span><span class="sxs-lookup"><span data-stu-id="ebd45-240">Right-click the **STESample.WPFTest** project in **Solution Explorer** and select **Debug -&gt; Start new instance**</span></span>
-   <span data-ttu-id="ebd45-241">Können Sie die Daten, die mit dem Bildschirm bearbeiten und speichern Sie es über den Dienst mithilfe der **speichern** Schaltfläche</span><span class="sxs-lookup"><span data-stu-id="ebd45-241">You can manipulate the data using the screen and save it via the service using the **Save** button</span></span>

![WPF](~/ef6/media/wpf.png)
