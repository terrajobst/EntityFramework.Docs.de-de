---
title: Database First – EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: cc6ffdb3-388d-4e79-a201-01ec2577c949
caps.latest.revision: 3
ms.openlocfilehash: 17bba5fe9883a1bee0f8b9624dfa35da889e6005
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2018
ms.locfileid: "39120853"
---
# <a name="database-first"></a><span data-ttu-id="d9ea5-102">Zunächst-Datenbank</span><span class="sxs-lookup"><span data-stu-id="d9ea5-102">Database First</span></span>
<span data-ttu-id="d9ea5-103">Dieses video und schrittweise exemplarische Vorgehensweise bieten eine Einführung in die Verwendung von Entity Framework Database First-Entwicklung.</span><span class="sxs-lookup"><span data-stu-id="d9ea5-103">This video and step-by-step walkthrough provide an introduction to Database First development using Entity Framework.</span></span> <span data-ttu-id="d9ea5-104">Datenbank können Sie für das reverse Engineering zuerst ein Modell aus einer vorhandenen Datenbank.</span><span class="sxs-lookup"><span data-stu-id="d9ea5-104">Database First allows you to reverse engineer a model from an existing database.</span></span> <span data-ttu-id="d9ea5-105">Das Modell kann im Entity Framework Designer wird in einer EDMX-Datei (EDMX-Erweiterung) gespeichert und angezeigt und bearbeitet werden.</span><span class="sxs-lookup"><span data-stu-id="d9ea5-105">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="d9ea5-106">Die Klassen, die Sie in Ihrer Anwendung interagieren, werden automatisch aus der EDMX-Datei generiert.</span><span class="sxs-lookup"><span data-stu-id="d9ea5-106">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="d9ea5-107">Video ansehen</span><span class="sxs-lookup"><span data-stu-id="d9ea5-107">Watch the video</span></span>
<span data-ttu-id="d9ea5-108">Dieses Video bietet eine Einführung in die Verwendung von Entity Framework Database First-Entwicklung.</span><span class="sxs-lookup"><span data-stu-id="d9ea5-108">This video provides an introduction to Database First development using Entity Framework.</span></span> <span data-ttu-id="d9ea5-109">Datenbank können Sie für das reverse Engineering zuerst ein Modell aus einer vorhandenen Datenbank.</span><span class="sxs-lookup"><span data-stu-id="d9ea5-109">Database First allows you to reverse engineer a model from an existing database.</span></span> <span data-ttu-id="d9ea5-110">Das Modell kann im Entity Framework Designer wird in einer EDMX-Datei (EDMX-Erweiterung) gespeichert und angezeigt und bearbeitet werden.</span><span class="sxs-lookup"><span data-stu-id="d9ea5-110">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="d9ea5-111">Die Klassen, die Sie in Ihrer Anwendung interagieren, werden automatisch aus der EDMX-Datei generiert.</span><span class="sxs-lookup"><span data-stu-id="d9ea5-111">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

<span data-ttu-id="d9ea5-112">**Präsentation:** [Rowan Miller](http://romiller.com/)</span><span class="sxs-lookup"><span data-stu-id="d9ea5-112">**Presented By**: [Rowan Miller](http://romiller.com/)</span></span>

<span data-ttu-id="d9ea5-113">**Video**: [WMV](http://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.wmv) | [MP4](http://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-mp4video-databasefirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.zip)</span><span class="sxs-lookup"><span data-stu-id="d9ea5-113">**Video**: [WMV](http://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.wmv) | [MP4](http://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-mp4video-databasefirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="d9ea5-114">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="d9ea5-114">Pre-Requisites</span></span>

<span data-ttu-id="d9ea5-115">Sie benötigen mindestens Visual Studio 2010 oder Visual Studio 2012 installiert werden, um diese exemplarische Vorgehensweise abgeschlossen haben.</span><span class="sxs-lookup"><span data-stu-id="d9ea5-115">You will need to have at least Visual Studio 2010 or Visual Studio 2012 installed to complete this walkthrough.</span></span>

<span data-ttu-id="d9ea5-116">Wenn Sie Visual Studio 2010 verwenden, müssen Sie auch haben [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installiert.</span><span class="sxs-lookup"><span data-stu-id="d9ea5-116">If you are using Visual Studio 2010, you will also need to have [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installed.</span></span>

 

## <a name="1-create-an-existing-database"></a><span data-ttu-id="d9ea5-117">1. Erstellen Sie eine vorhandene Datenbank</span><span class="sxs-lookup"><span data-stu-id="d9ea5-117">1. Create an Existing Database</span></span>

<span data-ttu-id="d9ea5-118">In der Regel müssen, wenn Sie eine vorhandene Datenbank verwenden möchten, die sie bereits erstellt wird, aber in dieser exemplarischen Vorgehensweise wir zum Erstellen einer Datenbank auf.</span><span class="sxs-lookup"><span data-stu-id="d9ea5-118">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="d9ea5-119">Der Datenbankserver, der mit Visual Studio installiert ist, unterscheidet sich abhängig von der Version von Visual Studio, die Sie installiert haben:</span><span class="sxs-lookup"><span data-stu-id="d9ea5-119">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="d9ea5-120">Wenn Sie Visual Studio 2010 verwenden erstellen Sie eine SQL Express-Datenbank.</span><span class="sxs-lookup"><span data-stu-id="d9ea5-120">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="d9ea5-121">Wenn Sie Visual Studio 2012 verwenden, und klicken Sie dann Sie erstellen eine [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) Datenbank.</span><span class="sxs-lookup"><span data-stu-id="d9ea5-121">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) database.</span></span>

 

<span data-ttu-id="d9ea5-122">Wir jetzt, und Erstellen der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="d9ea5-122">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="d9ea5-123">Öffnen Sie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d9ea5-123">Open Visual Studio</span></span>
-   <span data-ttu-id="d9ea5-124">**Ansicht – Profiler -&gt; Server-Explorer**</span><span class="sxs-lookup"><span data-stu-id="d9ea5-124">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="d9ea5-125">Klicken Sie mit der rechten Maustaste auf **Datenverbindungen -&gt; Verbindung hinzufügen...**</span><span class="sxs-lookup"><span data-stu-id="d9ea5-125">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="d9ea5-126">Wenn Sie vor dem müssen Sie Microsoft SQL Server als Datenquelle wählen im Server-Explorer mit einer Datenbank verbunden haben</span><span class="sxs-lookup"><span data-stu-id="d9ea5-126">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![SelectDataSource](~/ef6/media/selectdatasource.png)

-   <span data-ttu-id="d9ea5-128">Eine Verbindung mit LocalDB oder SQL Express, je nachdem, welches Sie installiert haben, und geben Sie **DatabaseFirst.Blogging** als Datenbankname</span><span class="sxs-lookup"><span data-stu-id="d9ea5-128">Connect to either LocalDB or SQL Express, depending on which one you have installed, and enter **DatabaseFirst.Blogging** as the database name</span></span>

    ![SqlExpressConnectionDF](~/ef6/media/sqlexpressconnectiondf.png)

    ![LocalDBConnectionDF](~/ef6/media/localdbconnectiondf.png)

-   <span data-ttu-id="d9ea5-131">Wählen Sie **OK** und Sie werden gefragt, ob Sie eine neue Datenbank, die auf erstellen möchten **Ja**</span><span class="sxs-lookup"><span data-stu-id="d9ea5-131">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![CreateDatabaseDialog](~/ef6/media/createdatabasedialog.png)

-   <span data-ttu-id="d9ea5-133">Die neue Datenbank wird jetzt im Server-Explorer angezeigt. mit der rechten Maustaste darauf und wählen Sie **neue Abfrage**</span><span class="sxs-lookup"><span data-stu-id="d9ea5-133">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="d9ea5-134">Kopieren Sie die folgende SQL-Anweisung in die neue Abfrage, und klicken Sie dann mit der rechten Maustaste auf die Abfrage, und wählen **ausführen**</span><span class="sxs-lookup"><span data-stu-id="d9ea5-134">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

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
```

## <a name="2-create-the-application"></a><span data-ttu-id="d9ea5-135">2. Erstellen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="d9ea5-135">2. Create the Application</span></span>

<span data-ttu-id="d9ea5-136">Einfachheit halber werden wir eine einfache Konsolenanwendung zu erstellen, die das Database First verwendet, auf Daten zugreift:</span><span class="sxs-lookup"><span data-stu-id="d9ea5-136">To keep things simple we’re going to build a basic console application that uses the Database First to perform data access:</span></span>

-   <span data-ttu-id="d9ea5-137">Öffnen Sie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d9ea5-137">Open Visual Studio</span></span>
-   <span data-ttu-id="d9ea5-138">**Datei -&gt; neu –&gt; Projekt...**</span><span class="sxs-lookup"><span data-stu-id="d9ea5-138">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="d9ea5-139">Wählen Sie **Windows** im linken Menü und **-Konsolenanwendung**</span><span class="sxs-lookup"><span data-stu-id="d9ea5-139">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="d9ea5-140">Geben Sie **DatabaseFirstSample** als Name</span><span class="sxs-lookup"><span data-stu-id="d9ea5-140">Enter **DatabaseFirstSample** as the name</span></span>
-   <span data-ttu-id="d9ea5-141">Wählen Sie **OK** aus.</span><span class="sxs-lookup"><span data-stu-id="d9ea5-141">Select **OK**</span></span>

 

## <a name="3-reverse-engineer-model"></a><span data-ttu-id="d9ea5-142">3. Reverse Engineering-Modells</span><span class="sxs-lookup"><span data-stu-id="d9ea5-142">3. Reverse Engineer Model</span></span>

<span data-ttu-id="d9ea5-143">Wir werden die Verwendung von Entity Framework Designer, die als Teil von Visual Studio enthalten ist, um unser Modell zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="d9ea5-143">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="d9ea5-144">**Projekt -&gt; neues Element hinzufügen...**</span><span class="sxs-lookup"><span data-stu-id="d9ea5-144">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="d9ea5-145">Wählen Sie **Daten** im linken Menü und dann **ADO.NET Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="d9ea5-145">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="d9ea5-146">Geben Sie **BloggingModel** als ein, und klicken Sie auf **OK**</span><span class="sxs-lookup"><span data-stu-id="d9ea5-146">Enter **BloggingModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="d9ea5-147">Dadurch wird die **Entity Data Model-Assistenten**</span><span class="sxs-lookup"><span data-stu-id="d9ea5-147">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="d9ea5-148">Wählen Sie **aus Datenbank generieren** , und klicken Sie auf **weiter**</span><span class="sxs-lookup"><span data-stu-id="d9ea5-148">Select **Generate from Database** and click **Next**</span></span>

    ![WizardStep1](~/ef6/media/wizardstep1.png)

-   <span data-ttu-id="d9ea5-150">Wählen Sie die Verbindung mit der Datenbank, die Sie im ersten Abschnitt erstellt haben, geben Sie **BloggingContext** als Namen für die Verbindungszeichenfolge und klicken Sie auf **weiter**</span><span class="sxs-lookup"><span data-stu-id="d9ea5-150">Select the connection to the database you created in the first section, enter **BloggingContext** as the name of the connection string and click **Next**</span></span>

    ![WizardStep2](~/ef6/media/wizardstep2.png)

-   <span data-ttu-id="d9ea5-152">Klicken Sie auf das Kontrollkästchen neben "Tabellen", um alle Tabellen importiert werden, und klicken Sie auf "Fertig stellen"</span><span class="sxs-lookup"><span data-stu-id="d9ea5-152">Click the checkbox next to ‘Tables’ to import all tables and click ‘Finish’</span></span>

    ![WizardStep3](~/ef6/media/wizardstep3.png)

 

<span data-ttu-id="d9ea5-154">Nach Abschluss der reverse-Engineering-Prozess ist das neue Modell dem Projekt hinzugefügt und eröffnet Ihnen im Entity Framework Designer angezeigt.</span><span class="sxs-lookup"><span data-stu-id="d9ea5-154">Once the reverse engineer process completes the new model is added to your project and opened up for you to view in the Entity Framework Designer.</span></span> <span data-ttu-id="d9ea5-155">Zu Ihrem Projekt mit den Verbindungsdetails für die Datenbank wurde auch eine Datei "App.config" hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="d9ea5-155">An App.config file has also been added to your project with the connection details for the database.</span></span>

![ModelInitial](~/ef6/media/modelinitial.png)

### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="d9ea5-157">Zusätzliche Schritte in Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="d9ea5-157">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="d9ea5-158">Wenn Sie in Visual Studio 2010 arbeiten gibt es jedoch einige zusätzliche Schritte, die Sie befolgen, um auf die neueste Version von Entity Framework zu aktualisieren müssen.</span><span class="sxs-lookup"><span data-stu-id="d9ea5-158">If you are working in Visual Studio 2010 there are some additional steps you need to follow to upgrade to the latest version of Entity Framework.</span></span> <span data-ttu-id="d9ea5-159">Es ist wichtig, ein Upgrade, da sie Zugriff auf eine verbesserte API-Oberfläche, die viel einfacher erhalten zu verwenden ist, sowie die neuesten Updates.</span><span class="sxs-lookup"><span data-stu-id="d9ea5-159">Upgrading is important because it gives you access to an improved API surface, that is much easier to use, as well as the latest bug fixes.</span></span>

<span data-ttu-id="d9ea5-160">Zuerst muss, um die neueste Version von Entity Framework von NuGet zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="d9ea5-160">First up, we need to get the latest version of Entity Framework from NuGet.</span></span>

-   <span data-ttu-id="d9ea5-161">**Projekt:&gt; NuGet-Pakete verwalten... ** 
     *Wenn Ihnen keine der **NuGet-Pakete verwalten... ** Option Sie installieren die [neueste Version von NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*</span><span class="sxs-lookup"><span data-stu-id="d9ea5-161">**Project –&gt; Manage NuGet Packages…**
*If you don’t have the **Manage NuGet Packages…** option you should install the [latest version of NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*</span></span>
-   <span data-ttu-id="d9ea5-162">Wählen Sie die **Online** Registerkarte</span><span class="sxs-lookup"><span data-stu-id="d9ea5-162">Select the **Online** tab</span></span>
-   <span data-ttu-id="d9ea5-163">Wählen Sie die **EntityFramework** Paket</span><span class="sxs-lookup"><span data-stu-id="d9ea5-163">Select the **EntityFramework** package</span></span>
-   <span data-ttu-id="d9ea5-164">Klicken Sie auf **installieren**</span><span class="sxs-lookup"><span data-stu-id="d9ea5-164">Click **Install**</span></span>

<span data-ttu-id="d9ea5-165">Als Nächstes müssen wir unser Modell zum Generieren von Code, mit der die DbContext-API verwenden, die in höheren Versionen von Entity Framework eingeführte, austauschen.</span><span class="sxs-lookup"><span data-stu-id="d9ea5-165">Next, we need to swap our model to generate code that makes use of the DbContext API, which was introduced in later versions of Entity Framework.</span></span>

-   <span data-ttu-id="d9ea5-166">Mit der rechten Maustaste auf eine leere Stelle des Modells im EF Designer, und wählen Sie **Codegenerierungselement hinzufügen...**</span><span class="sxs-lookup"><span data-stu-id="d9ea5-166">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="d9ea5-167">Wählen Sie **Onlinevorlagen** aus dem linken Menü und die Suche nach **"DbContext"**</span><span class="sxs-lookup"><span data-stu-id="d9ea5-167">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="d9ea5-168">Wählen Sie die EF **5.x DbContext Generator für C\#**, geben Sie **BloggingModel** als ein, und klicken Sie auf **hinzufügen**</span><span class="sxs-lookup"><span data-stu-id="d9ea5-168">Select the EF **5.x DbContext Generator for C\#**, enter **BloggingModel** as the name and click **Add**</span></span>

    ![DbContextTemplate](~/ef6/media/dbcontexttemplate.png)

 

## <a name="4-reading--writing-data"></a><span data-ttu-id="d9ea5-170">4. Lesen und Schreiben von Daten</span><span class="sxs-lookup"><span data-stu-id="d9ea5-170">4. Reading & Writing Data</span></span>

<span data-ttu-id="d9ea5-171">Nun, da es sich um ein Modell verfügen, ist es Zeit, die sie verwenden, um einige Daten zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="d9ea5-171">Now that we have a model it’s time to use it to access some data.</span></span> <span data-ttu-id="d9ea5-172">Die Klassen werden wir Sie verwenden, um den Zugriff auf Daten werden wird automatisch generiert basierend auf die EDMX-Datei.</span><span class="sxs-lookup"><span data-stu-id="d9ea5-172">The classes we are going to use to access data are being automatically generated for you based on the EDMX file.</span></span>

<span data-ttu-id="d9ea5-173">*Dieser Screenshot stammt aus Visual Studio 2012, bei Verwendung von Visual Studio 2010 die BloggingModel.tt und BloggingModel.Context.tt Dateien werden direkt unterhalb des Projekts und nicht als geschachtelte unter der EDMX-Datei.*</span><span class="sxs-lookup"><span data-stu-id="d9ea5-173">*This screen shot is from Visual Studio 2012, if you are using Visual Studio 2010 the BloggingModel.tt and BloggingModel.Context.tt files will be directly under your project rather than nested under the EDMX file.*</span></span>

![GeneratedClassesDF](~/ef6/media/generatedclassesdf.png)

 

<span data-ttu-id="d9ea5-175">Implementieren Sie die Main-Methode in "Program.cs" ein, wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="d9ea5-175">Implement the Main method in Program.cs as shown below.</span></span> <span data-ttu-id="d9ea5-176">Dieser Code erstellt eine neue Instanz unseres Kontexts und anschließend verwendet, um einen neuen Blog einfügen.</span><span class="sxs-lookup"><span data-stu-id="d9ea5-176">This code creates a new instance of our context and then uses it to insert a new Blog.</span></span> <span data-ttu-id="d9ea5-177">Anschließend wird eine LINQ-Abfrage zum Abrufen aller Blogs aus der Datenbank nach Titel alphabetisch angeordnet sind.</span><span class="sxs-lookup"><span data-stu-id="d9ea5-177">Then it uses a LINQ query to retrieve all Blogs from the database ordered alphabetically by Title.</span></span>

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

<span data-ttu-id="d9ea5-178">Sie können jetzt die Anwendung auszuführen, und probieren Sie es aus.</span><span class="sxs-lookup"><span data-stu-id="d9ea5-178">You can now run the application and test it out.</span></span>

```
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```
 

## <a name="5-dealing-with-database-changes"></a><span data-ttu-id="d9ea5-179">5. Umgang mit Änderungen in der Datenbank</span><span class="sxs-lookup"><span data-stu-id="d9ea5-179">5. Dealing with Database Changes</span></span>

<span data-ttu-id="d9ea5-180">Jetzt ist es Zeit für unsere Datenbankschema, einige Änderungen vornehmen, wenn wir diese Änderungen vornehmen, die wir auch unser Modell entsprechend aktualisieren müssen.</span><span class="sxs-lookup"><span data-stu-id="d9ea5-180">Now it’s time to make some changes to our database schema, when we make these changes we also need to update our model to reflect those changes.</span></span>

<span data-ttu-id="d9ea5-181">Der erste Schritt ist, einige Änderungen am Datenbankschema vornehmen.</span><span class="sxs-lookup"><span data-stu-id="d9ea5-181">The first step is to make some changes to the database schema.</span></span> <span data-ttu-id="d9ea5-182">Wir werden das Schema einer Benutzertabelle hinzu.</span><span class="sxs-lookup"><span data-stu-id="d9ea5-182">We’re going to add a Users table to the schema.</span></span>

-   <span data-ttu-id="d9ea5-183">Mit der rechten Maustaste auf die **DatabaseFirst.Blogging** Datenbank im Server-Explorer, und wählen Sie **neue Abfrage**</span><span class="sxs-lookup"><span data-stu-id="d9ea5-183">Right-click on the **DatabaseFirst.Blogging** database in Server Explorer and select **New Query**</span></span>
-   <span data-ttu-id="d9ea5-184">Kopieren Sie die folgende SQL-Anweisung in die neue Abfrage, und klicken Sie dann mit der rechten Maustaste auf die Abfrage, und wählen **ausführen**</span><span class="sxs-lookup"><span data-stu-id="d9ea5-184">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

``` SQL
CREATE TABLE [dbo].[Users]
(
    [Username] NVARCHAR(50) NOT NULL PRIMARY KEY,  
    [DisplayName] NVARCHAR(MAX) NULL
)
```

<span data-ttu-id="d9ea5-185">Nachdem das Schema aktualisiert wird, ist es Zeit, um das Modell mit diesen Änderungen aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="d9ea5-185">Now that the schema is updated, it’s time to update the model with those changes.</span></span>

-   <span data-ttu-id="d9ea5-186">Mit der rechten Maustaste auf eine leere Stelle Ihres Modells im EF Designer und wählen "Updatemodell aus der Datenbank...", dadurch wird die Update-Assistenten</span><span class="sxs-lookup"><span data-stu-id="d9ea5-186">Right-click on an empty spot of your model in the EF Designer and select ‘Update Model from Database…’, this will launch the Update Wizard</span></span>
-   <span data-ttu-id="d9ea5-187">Auf der Registerkarte für das Hinzufügen der Update-Assistenten aktivieren Sie das Kontrollkästchen neben den Tabellen gibt dies an, dass alle neuen Tabellen aus dem Schema hinzugefügt werden soll.</span><span class="sxs-lookup"><span data-stu-id="d9ea5-187">On the Add tab of the Update Wizard check the box next to Tables, this indicates that we want to add any new tables from the schema.</span></span>
    <span data-ttu-id="d9ea5-188">*Die Registerkarte "Aktualisieren" zeigt alle bestehenden Tabellen im Modell, das während des Updates auf Änderungen überprüft werden. Die Delete-Registerkarten zeigen alle Tabellen, die aus dem Schema entfernt wurden, und werden ebenfalls aus dem Modell im Rahmen des Updates entfernt werden. Die Informationen zu diesen beiden Registerkarten wird automatisch erkannt und dient nur zu Informationszwecken können nicht Sie keine Einstellungen geändert.*</span><span class="sxs-lookup"><span data-stu-id="d9ea5-188">*The Refresh tab shows any existing tables in the model that will be checked for changes during the update. The Delete tabs show any tables that have been removed from the schema and will also be removed from the model as part of the update. The information on these two tabs is automatically detected and is provided for informational purposes only, you cannot change any settings.*</span></span>

    ![RefreshWizard](~/ef6/media/refreshwizard.png)

-   <span data-ttu-id="d9ea5-190">Klicken Sie auf "Fertig stellen", die Update-Assistenten</span><span class="sxs-lookup"><span data-stu-id="d9ea5-190">Click Finish on the Update Wizard</span></span>

 

<span data-ttu-id="d9ea5-191">Das Modell wurde aktualisiert, um eine neue Benutzerentität enthalten, die der Tabelle zugeordnet, die wir in der Datenbank hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="d9ea5-191">The model is now updated to include a new User entity that maps to the Users table we added to the database.</span></span>

![ModelUpdated](~/ef6/media/modelupdated.png)

## <a name="summary"></a><span data-ttu-id="d9ea5-193">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="d9ea5-193">Summary</span></span>

<span data-ttu-id="d9ea5-194">In dieser exemplarischen Vorgehensweise, die wir uns Database First-Entwicklung angesehen haben, hatten die wir ein Modell im EF Designer basierend auf einer vorhandenen Datenbank zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="d9ea5-194">In this walkthrough we looked at Database First development, which allowed us to create a model in the EF Designer based on an existing database.</span></span> <span data-ttu-id="d9ea5-195">Klicken Sie dann verwendet das Modell lesen und Schreiben von Daten aus der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="d9ea5-195">We then used that model to read and write some data from the database.</span></span> <span data-ttu-id="d9ea5-196">Abschließend aktualisiert wir das Modell, um die Änderungen, die wir am Datenbankschema vorgenommen.</span><span class="sxs-lookup"><span data-stu-id="d9ea5-196">Finally, we updated the model to reflect changes we made to the database schema.</span></span>
