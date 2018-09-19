---
title: Unterstützung von Enumerationen – Code First – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 77a42501-27c9-4f4b-96df-26c128021467
ms.openlocfilehash: 1cecbf7065367deb3d202977fe39187bd907d824
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283719"
---
# <a name="enum-support---code-first"></a><span data-ttu-id="c5060-102">Unterstützung von Enumerationen – Code First</span><span class="sxs-lookup"><span data-stu-id="c5060-102">Enum Support - Code First</span></span>
> [!NOTE]
> <span data-ttu-id="c5060-103">**EF5 oder höher, nur** -APIs, die Funktionen erläutert, die auf dieser Seite usw. in Entity Framework 5 eingeführt wurden.</span><span class="sxs-lookup"><span data-stu-id="c5060-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="c5060-104">Wenn Sie eine frühere Version verwenden, gelten manche Informationen nicht.</span><span class="sxs-lookup"><span data-stu-id="c5060-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="c5060-105">Dieses video und schrittweise exemplarische Vorgehensweise zeigt die Verwendung von Enumerationstypen mit Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="c5060-105">This video and step-by-step walkthrough shows how to use enum types with Entity Framework Code First.</span></span> <span data-ttu-id="c5060-106">Es wird veranschaulicht, wie Enumerationen in einer LINQ-Abfrage verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="c5060-106">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="c5060-107">In dieser exemplarischen Vorgehensweise wird Code First verwenden, um eine neue Datenbank zu erstellen, aber Sie können auch [Code First mit einer vorhandenen Datenbank zuordnen](~/ef6/modeling/code-first/workflows/existing-database.md).</span><span class="sxs-lookup"><span data-stu-id="c5060-107">This walkthrough will use Code First to create a new database, but you can also use [Code First to map to an existing database](~/ef6/modeling/code-first/workflows/existing-database.md).</span></span>

<span data-ttu-id="c5060-108">Enum-Unterstützung wurde in Entity Framework 5 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="c5060-108">Enum support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="c5060-109">Um die neuen Features wie Enumerationen, räumliche Datentypen und Tabellenwertfunktionen zu verwenden, müssen Sie .NET Framework 4.5 ausrichten.</span><span class="sxs-lookup"><span data-stu-id="c5060-109">To use the new features like enums, spatial data types, and table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="c5060-110">Visual Studio 2012 ist standardmäßig die Zielversion .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="c5060-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="c5060-111">Im Entity Framework kann eine Enumeration der folgenden zugrunde liegende Typen aufweisen: **Byte**, **Int16**, **Int32**, **Int64** , oder **SByte**.</span><span class="sxs-lookup"><span data-stu-id="c5060-111">In Entity Framework, an enumeration can have the following underlying types: **Byte**, **Int16**, **Int32**, **Int64** , or **SByte**.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="c5060-112">Video ansehen</span><span class="sxs-lookup"><span data-stu-id="c5060-112">Watch the video</span></span>
<span data-ttu-id="c5060-113">Dieses Video zeigt, wie Sie Enum-Typen mit Entity Framework Code First zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="c5060-113">This video shows how to use enum types with Entity Framework Code First.</span></span> <span data-ttu-id="c5060-114">Es wird veranschaulicht, wie Enumerationen in einer LINQ-Abfrage verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="c5060-114">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="c5060-115">**Präsentiert von**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="c5060-115">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="c5060-116">**Video**: [WMV](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.wmv) | [MP4](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-mp4video-enumwithcodefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.zip)</span><span class="sxs-lookup"><span data-stu-id="c5060-116">**Video**: [WMV](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.wmv) | [MP4](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-mp4video-enumwithcodefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="c5060-117">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="c5060-117">Pre-Requisites</span></span>

<span data-ttu-id="c5060-118">Sie müssen Visual Studio 2012, Ultimate, Premium, Professional oder Web Express Edition installiert, um diese exemplarische Vorgehensweise abgeschlossen haben.</span><span class="sxs-lookup"><span data-stu-id="c5060-118">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

 

## <a name="set-up-the-project"></a><span data-ttu-id="c5060-119">Einrichten des Projekts</span><span class="sxs-lookup"><span data-stu-id="c5060-119">Set up the Project</span></span>

1.  <span data-ttu-id="c5060-120">Visual Studio 2012 öffnen</span><span class="sxs-lookup"><span data-stu-id="c5060-120">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="c5060-121">Auf der **Datei** Startmenü **neu**, und klicken Sie dann auf **Projekt**</span><span class="sxs-lookup"><span data-stu-id="c5060-121">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="c5060-122">Klicken Sie im linken Bereich auf **Visual C\#**, und wählen Sie dann die **Konsole** Vorlage</span><span class="sxs-lookup"><span data-stu-id="c5060-122">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="c5060-123">Geben Sie **EnumCodeFirst** als Namen für das Projekt und auf **OK**</span><span class="sxs-lookup"><span data-stu-id="c5060-123">Enter **EnumCodeFirst** as the name of the project and click **OK**</span></span>

## <a name="define-a-new-model-using-code-first"></a><span data-ttu-id="c5060-124">Definieren Sie ein neues Modell mit Code First</span><span class="sxs-lookup"><span data-stu-id="c5060-124">Define a New Model using Code First</span></span>

<span data-ttu-id="c5060-125">Bei Verwendung von Code First-Entwicklung beginnen Sie in der Regel durch das Schreiben von .NET Framework-Klassen, die das konzeptionelle (Domäne)-Modell zu definieren.</span><span class="sxs-lookup"><span data-stu-id="c5060-125">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span> <span data-ttu-id="c5060-126">Der folgende Code definiert die Department-Klasse.</span><span class="sxs-lookup"><span data-stu-id="c5060-126">The code below defines the Department class.</span></span>

<span data-ttu-id="c5060-127">Der Code definiert außerdem die DepartmentNames-Enumeration.</span><span class="sxs-lookup"><span data-stu-id="c5060-127">The code also defines the DepartmentNames enumeration.</span></span> <span data-ttu-id="c5060-128">Die Enumeration wird standardmäßig der **Int** Typ.</span><span class="sxs-lookup"><span data-stu-id="c5060-128">By default, the enumeration is of **int** type.</span></span> <span data-ttu-id="c5060-129">Die Name-Eigenschaft für die Abteilung-Klasse ist die DepartmentNames-Typs.</span><span class="sxs-lookup"><span data-stu-id="c5060-129">The Name property on the Department class is of the DepartmentNames type.</span></span>

<span data-ttu-id="c5060-130">Öffnen Sie die Datei "Program.cs" ein, und fügen Sie die folgenden Klassendefinitionen.</span><span class="sxs-lookup"><span data-stu-id="c5060-130">Open the Program.cs file and paste the following class definitions.</span></span>

``` csharp
public enum DepartmentNames
{
    English,
    Math,
    Economics
}     

public partial class Department
{
    public int DepartmentID { get; set; }
    public DepartmentNames Name { get; set; }
    public decimal Budget { get; set; }
}
```
 

## <a name="define-the-dbcontext-derived-type"></a><span data-ttu-id="c5060-131">Definieren Sie die "DbContext" abgeleiteten Typ</span><span class="sxs-lookup"><span data-stu-id="c5060-131">Define the DbContext Derived Type</span></span>

<span data-ttu-id="c5060-132">Zusätzlich zum Definieren von Entitäten, müssen Sie eine Klasse definieren, die von "DbContext" abgeleitet und stellt "DbSet"&lt;TEntity&gt; Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="c5060-132">In addition to defining entities, you need to define a class that derives from DbContext and exposes DbSet&lt;TEntity&gt; properties.</span></span> <span data-ttu-id="c5060-133">"DbSet"&lt;TEntity&gt; Eigenschaften können Sie den Kontext, die wissen, welche Typen im Modell enthalten sein sollen.</span><span class="sxs-lookup"><span data-stu-id="c5060-133">The DbSet&lt;TEntity&gt; properties let the context know which types you want to include in the model.</span></span>

<span data-ttu-id="c5060-134">Eine Instanz des Typs "DbContext" abgeleitet verwaltet die Entitätsobjekte während der Laufzeit, einschließlich ausfüllenden Objekten mit Daten aus einer Datenbank, verfolgen und Speichern von Daten in der Datenbank zu ändern.</span><span class="sxs-lookup"><span data-stu-id="c5060-134">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

<span data-ttu-id="c5060-135">Die "DbContext" und "DbSet"-Typen werden in der EntityFramework-Assembly definiert.</span><span class="sxs-lookup"><span data-stu-id="c5060-135">The DbContext and DbSet types are defined in the EntityFramework assembly.</span></span> <span data-ttu-id="c5060-136">Es wird einen Verweis auf diese DLL-Datei mit EntityFramework NuGet-Paket hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="c5060-136">We will add a reference to this DLL by using the EntityFramework NuGet package.</span></span>

1.  <span data-ttu-id="c5060-137">Im Projektmappen-Explorer mit der rechten Maustaste auf den Projektnamen.</span><span class="sxs-lookup"><span data-stu-id="c5060-137">In Solution Explorer, right-click on the project name.</span></span>
2.  <span data-ttu-id="c5060-138">Wählen Sie **NuGet-Pakete verwalten...**</span><span class="sxs-lookup"><span data-stu-id="c5060-138">Select **Manage NuGet Packages…**</span></span>
3.  <span data-ttu-id="c5060-139">Wählen Sie in das Dialogfeld "NuGet-Pakete verwalten" die **Online** Registerkarte, und wählen Sie die **EntityFramework** Paket.</span><span class="sxs-lookup"><span data-stu-id="c5060-139">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package.</span></span>
4.  <span data-ttu-id="c5060-140">Klicken Sie auf **installieren**</span><span class="sxs-lookup"><span data-stu-id="c5060-140">Click **Install**</span></span>

<span data-ttu-id="c5060-141">Beachten Sie, dass zusätzlich zu den EntityFramework-Assembly, sowie Verweise auf Assemblys System.ComponentModel.DataAnnotations und System.Data.Entity hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="c5060-141">Note, that in addition to the EntityFramework  assembly, references to System.ComponentModel.DataAnnotations and System.Data.Entity assemblies are added as well.</span></span>

<span data-ttu-id="c5060-142">Fügen Sie am Anfang der Datei "Program.cs" die folgenden using-Anweisung:</span><span class="sxs-lookup"><span data-stu-id="c5060-142">At the top of the Program.cs file, add the following using statement:</span></span>

``` csharp
using System.Data.Entity;
```

<span data-ttu-id="c5060-143">Fügen Sie die Kontextdefinition in "Program.cs" hinzu.</span><span class="sxs-lookup"><span data-stu-id="c5060-143">In the Program.cs add the context definition.</span></span> 

``` csharp
public partial class EnumTestContext : DbContext
{
    public DbSet<Department> Departments { get; set; }
}
```
 

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="c5060-144">Speichern und Abrufen von Daten</span><span class="sxs-lookup"><span data-stu-id="c5060-144">Persist and Retrieve Data</span></span>

<span data-ttu-id="c5060-145">Öffnen Sie die Datei "Program.cs", die, in die Main-Methode definiert ist.</span><span class="sxs-lookup"><span data-stu-id="c5060-145">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="c5060-146">Fügen Sie der Main-Funktion mit den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="c5060-146">Add the following code into the Main function.</span></span> <span data-ttu-id="c5060-147">Der Code Fügt ein neues Objekt für die Abteilung, in den Kontext.</span><span class="sxs-lookup"><span data-stu-id="c5060-147">The code adds a new Department object to the context.</span></span> <span data-ttu-id="c5060-148">Klicken Sie dann die Daten gespeichert.</span><span class="sxs-lookup"><span data-stu-id="c5060-148">It then saves the data.</span></span> <span data-ttu-id="c5060-149">Der Code führt auch eine LINQ-Abfrage, die eine Abteilung zurückgibt, wobei der Name DepartmentNames.English ist.</span><span class="sxs-lookup"><span data-stu-id="c5060-149">The code also executes a LINQ query that returns a Department where the name is DepartmentNames.English.</span></span>

``` csharp
using (var context = new EnumTestContext())
{
    context.Departments.Add(new Department { Name = DepartmentNames.English });

    context.SaveChanges();

    var department = (from d in context.Departments
                        where d.Name == DepartmentNames.English
                        select d).FirstOrDefault();

    Console.WriteLine(
        "DepartmentID: {0} Name: {1}",
        department.DepartmentID,  
        department.Name);
}
```

<span data-ttu-id="c5060-150">Kompilieren Sie die Anwendung, und führen Sie sie aus.</span><span class="sxs-lookup"><span data-stu-id="c5060-150">Compile and run the application.</span></span> <span data-ttu-id="c5060-151">Das Programm erzeugt die folgende Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="c5060-151">The program produces the following output:</span></span>

``` csharp
DepartmentID: 1 Name: English
```
 

## <a name="view-the-generated-database"></a><span data-ttu-id="c5060-152">Anzeigen der generierten Datenbank.</span><span class="sxs-lookup"><span data-stu-id="c5060-152">View the Generated Database</span></span>

<span data-ttu-id="c5060-153">Wenn Sie die Anwendung zum ersten Mal ausführen, erstellt Entity Framework eine Datenbank für Sie.</span><span class="sxs-lookup"><span data-stu-id="c5060-153">When you run the application the first time, the Entity Framework creates a database for you.</span></span> <span data-ttu-id="c5060-154">Da wir Visual Studio 2012 installiert haben, wird die Datenbank für die LocalDB-Instanz erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="c5060-154">Because we have Visual Studio 2012 installed, the database will be created on the LocalDB instance.</span></span> <span data-ttu-id="c5060-155">In der Standardeinstellung benennt Entity Framework die Datenbank nach der vollqualifizierte Name des abgeleiteten Kontexts (in diesem Beispiel, das **EnumCodeFirst.EnumTestContext**).</span><span class="sxs-lookup"><span data-stu-id="c5060-155">By default, the Entity Framework names the database after the fully qualified name of the derived context (for this example that is **EnumCodeFirst.EnumTestContext**).</span></span> <span data-ttu-id="c5060-156">Die nachfolgende, wie oft die vorhandene Datenbank verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="c5060-156">The subsequent times the existing database will be used.</span></span>  

<span data-ttu-id="c5060-157">Beachten Sie, dass wenn Sie Änderungen an Ihrem Modell vornehmen, nachdem die Datenbank erstellt wurde, sollten Sie Code First-Migrationen verwenden, um das Datenbankschema aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="c5060-157">Note, that if you make any changes to your model after the database has been created, you should use Code First Migrations to update the database schema.</span></span> <span data-ttu-id="c5060-158">Finden Sie unter [Code First in eine neue Datenbank](~/ef6/modeling/code-first/workflows/new-database.md) ein Beispiel für die Verwendung von Migrationen.</span><span class="sxs-lookup"><span data-stu-id="c5060-158">See [Code First to a New Database](~/ef6/modeling/code-first/workflows/new-database.md) for an example of using Migrations.</span></span>

<span data-ttu-id="c5060-159">Um die Datenbank und die Daten anzuzeigen, führen Sie folgende Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="c5060-159">To view the database and data, do the following:</span></span>

1.  <span data-ttu-id="c5060-160">Wählen Sie im Hauptmenü von Visual Studio 2012 **Ansicht**  - &gt; **Objekt-Explorer von SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="c5060-160">In the Visual Studio 2012 main menu, select **View** -&gt; **SQL Server Object Explorer**.</span></span>
2.  <span data-ttu-id="c5060-161">Wenn LocalDB nicht in der Liste der Server ist, klicken Sie auf die rechten Maustaste **SQL Server** , und wählen Sie **SQL Server hinzufügen** verwenden Sie die Standardeinstellung **Windows-Authentifizierung** zum Herstellen einer Verbindung mit der LocalDB-Instanz</span><span class="sxs-lookup"><span data-stu-id="c5060-161">If LocalDB is not in the list of servers, click the right mouse button on **SQL Server** and select **Add SQL Server** Use the default **Windows Authentication** to connect to the LocalDB instance</span></span>
3.  <span data-ttu-id="c5060-162">Erweitern Sie den Knoten für LocalDB</span><span class="sxs-lookup"><span data-stu-id="c5060-162">Expand the LocalDB node</span></span>
4.  <span data-ttu-id="c5060-163">Erweitern der **Datenbanken** Ordner finden Sie unter der neuen Datenbank aus, und navigieren Sie zu der **Abteilung** Tabelle Beachten Sie, dass, die Code First keine Tabelle erstellt wird, der den Enumerationstyp zugeordnet</span><span class="sxs-lookup"><span data-stu-id="c5060-163">Unfold the **Databases** folder to see the new database and browse to the **Department** table Note, that Code First does not create a table that maps to the enumeration type</span></span>
5.  <span data-ttu-id="c5060-164">Daten anzeigen, mit der rechten Maustaste auf die Tabelle aus, und wählen Sie **Anzeigedaten**</span><span class="sxs-lookup"><span data-stu-id="c5060-164">To view data, right-click on the table and select **View Data**</span></span>

## <a name="summary"></a><span data-ttu-id="c5060-165">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="c5060-165">Summary</span></span>

<span data-ttu-id="c5060-166">In dieser exemplarischen Vorgehensweise erläutert, wie Sie Enum-Typen mit Entity Framework Code First zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="c5060-166">In this walkthrough we looked at how to use enum types with Entity Framework Code First.</span></span> 
