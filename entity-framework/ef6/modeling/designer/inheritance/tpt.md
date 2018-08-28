---
title: Designer TPT-Vererbung – EF6
author: divega
ms.date: 2016-10-23
ms.assetid: efc78c31-b4ea-4ea3-a0cd-c69eb507020e
ms.openlocfilehash: 68980fa89446940b8b7f5f73c519d38e727a9039
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996347"
---
# <a name="designer-tpt-inheritance"></a><span data-ttu-id="84fc9-102">Designer TPT-Vererbung</span><span class="sxs-lookup"><span data-stu-id="84fc9-102">Designer TPT Inheritance</span></span>
<span data-ttu-id="84fc9-103">Schritt für Schritt dieser exemplarischen Vorgehensweise veranschaulicht implementieren Sie eine Tabelle pro Typ (TPT)-Vererbung in Ihrem Modell, mit dem Entity Framework Designer (EF-Designer).</span><span class="sxs-lookup"><span data-stu-id="84fc9-103">This step-by-step walkthrough shows how to implement table-per-type (TPT) inheritance in your model using the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="84fc9-104">Bei der "Tabelle pro Typ"-Vererbung wird eine separate Tabelle in der Datenbank verwendet, um die Daten für nicht geerbte Eigenschaften und Schlüsseleigenschaften für jeden Typ in der Vererbungshierarchie zu verwalten.</span><span class="sxs-lookup"><span data-stu-id="84fc9-104">Table-per-type inheritance uses a separate table in the database to maintain data for non-inherited properties and key properties for each type in the inheritance hierarchy.</span></span>

<span data-ttu-id="84fc9-105">In dieser exemplarischen Vorgehensweise ordnen wir den **Kurs** (Basistyp), **OnlineCourse** (abgeleitet aus dem Kurs wird), und **"OnsiteCourse"** (leitet sich von **Kurs**) Entitäten, die Tabellen mit dem gleichen Namen.</span><span class="sxs-lookup"><span data-stu-id="84fc9-105">In this walkthrough we will map the **Course** (base type), **OnlineCourse** (derives from Course), and **OnsiteCourse** (derives from **Course**) entities to tables with the same names.</span></span> <span data-ttu-id="84fc9-106">Wir erstellen ein Modell aus der Datenbank und ändern Sie dann das Modell, um die TPT-Vererbung zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="84fc9-106">We'll create a model from the database and then alter the model to implement the TPT inheritance.</span></span>

<span data-ttu-id="84fc9-107">Sie können auch mit den Model First beginnen und generieren Sie dann die Datenbank aus dem Modell.</span><span class="sxs-lookup"><span data-stu-id="84fc9-107">You can also start with the Model First and then generate the database from the model.</span></span> <span data-ttu-id="84fc9-108">Dem EF Designer verwendet die TPT-Strategie in der Standardeinstellung, und daher Vererbung in das Modell wird separaten Tabellen zugeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="84fc9-108">The EF Designer uses the TPT strategy by default and so any inheritance in the model will be mapped to separate tables.</span></span>

## <a name="other-inheritance-options"></a><span data-ttu-id="84fc9-109">Andere Vererbungsoptionen</span><span class="sxs-lookup"><span data-stu-id="84fc9-109">Other Inheritance Options</span></span>

<span data-ttu-id="84fc9-110">Tabelle pro Hierarchie (TPH) ist eine andere Art von Vererbung in der eine Tabelle verwendet wird, um die Daten aller Entitätstypen in einer Vererbungshierarchie zu verwalten.</span><span class="sxs-lookup"><span data-stu-id="84fc9-110">Table-per-Hierarchy (TPH) is another type of inheritance in which one database table is used to maintain data for all of the entity types in an inheritance hierarchy.</span></span>  <span data-ttu-id="84fc9-111">Informationen dazu, wie Sie eine Tabelle pro Hierarchie-Vererbung mit dem Entity Designer zuordnen, finden Sie unter [EF Designer TPH-Vererbung](~/ef6/modeling/designer/inheritance/tph.md).</span><span class="sxs-lookup"><span data-stu-id="84fc9-111">For information about how to map Table-per-Hierarchy inheritance with the Entity Designer, see [EF Designer TPH Inheritance](~/ef6/modeling/designer/inheritance/tph.md).</span></span> 

<span data-ttu-id="84fc9-112">Beachten Sie, dass die Tabelle pro konkreter geben, Vererbung (TPC) und gemischte Vererbung Modelle werden von der Entity Framework-Laufzeit unterstützt jedoch nicht von dem EF Designer unterstützt werden.</span><span class="sxs-lookup"><span data-stu-id="84fc9-112">Note that, the Table-per-Concrete Type Inheritance (TPC) and mixed inheritance models are supported by the Entity Framework runtime but are not supported by the EF Designer.</span></span> <span data-ttu-id="84fc9-113">Wenn Sie die TPC oder gemischte Vererbung verwenden möchten, haben Sie zwei Optionen: Code First verwenden, oder die EDMX-Datei manuell bearbeiten.</span><span class="sxs-lookup"><span data-stu-id="84fc9-113">If you want to use TPC or mixed inheritance, you have two options: use Code First, or manually edit the EDMX file.</span></span> <span data-ttu-id="84fc9-114">Wenn Sie mit der EDMX-Datei arbeiten möchten, Detailfenster Zuordnung wird in "abgesicherten Modus" versetzt werden, und Sie werden nicht in der Lage, den Designer zu verwenden, um die Zuordnungen ändern.</span><span class="sxs-lookup"><span data-stu-id="84fc9-114">If you choose to work with the EDMX file, the Mapping Details Window will be put into “safe mode” and you will not be able to use the designer to change the mappings.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="84fc9-115">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="84fc9-115">Prerequisites</span></span>

<span data-ttu-id="84fc9-116">Um die exemplarische Vorgehensweise nachzuvollziehen, benötigen Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="84fc9-116">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="84fc9-117">Eine aktuelle Version von Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="84fc9-117">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="84fc9-118">Die [Beispieldatenbank ' School '](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="84fc9-118">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="84fc9-119">Einrichten des Projekts</span><span class="sxs-lookup"><span data-stu-id="84fc9-119">Set up the Project</span></span>

-   <span data-ttu-id="84fc9-120">Öffnen Sie Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="84fc9-120">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="84fc9-121">Wählen Sie **Dateien&gt; neu –&gt; Projekt**</span><span class="sxs-lookup"><span data-stu-id="84fc9-121">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="84fc9-122">Klicken Sie im linken Bereich auf **Visual C\#**, und wählen Sie dann die **Konsole** Vorlage.</span><span class="sxs-lookup"><span data-stu-id="84fc9-122">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="84fc9-123">Geben Sie **TPTDBFirstSample** als Namen.</span><span class="sxs-lookup"><span data-stu-id="84fc9-123">Enter **TPTDBFirstSample** as the name.</span></span>
-   <span data-ttu-id="84fc9-124">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="84fc9-124">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="84fc9-125">Erstellen eines Modells</span><span class="sxs-lookup"><span data-stu-id="84fc9-125">Create a Model</span></span>

-   <span data-ttu-id="84fc9-126">Mit der rechten Maustaste in des Projekts im Projektmappen-Explorer, und wählen **hinzufügen –&gt; neues Element**.</span><span class="sxs-lookup"><span data-stu-id="84fc9-126">Right-click the project in Solution Explorer, and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="84fc9-127">Wählen Sie **Daten** aus dem linken Menü und wählen Sie dann **ADO.NET Entity Data Model** im Bereich Vorlagen.</span><span class="sxs-lookup"><span data-stu-id="84fc9-127">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="84fc9-128">Geben Sie **TPTModel.edmx** für den Dateinamen ein, und klicken Sie dann auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="84fc9-128">Enter **TPTModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="84fc9-129">Im Dialogfeld "Modellinhalte auswählen" das Feld, wählen Sie ** generieren, aus der Datenbank **, und klicken Sie dann auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="84fc9-129">In the Choose Model Contents dialog box, select** Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="84fc9-130">Klicken Sie auf **neue Verbindung**.</span><span class="sxs-lookup"><span data-stu-id="84fc9-130">Click **New Connection**.</span></span>
    <span data-ttu-id="84fc9-131">Geben Sie den Servernamen, klicken Sie im Dialogfeld "Verbindungseigenschaften" (z. B. **(Localdb)\\Mssqllocaldb**), wählen die Authentifizierungsmethode, Typ **School** für den Datenbanknamen, und klicken Sie dann Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="84fc9-131">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="84fc9-132">Das Dialogfeld "Wählen Sie Ihre Datenverbindung" wird mit Ihrem datenbankverbindungseinstellung aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="84fc9-132">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="84fc9-133">Wählen Sie in das Dialogfeld Datenbankobjekte auswählen, unter dem Knoten Tabellen der **Abteilung**, **Kurs OnlineCourse und "OnsiteCourse"** Tabellen.</span><span class="sxs-lookup"><span data-stu-id="84fc9-133">In the Choose Your Database Objects dialog box, under the Tables node, select the **Department**, **Course, OnlineCourse, and OnsiteCourse** tables.</span></span>
-   <span data-ttu-id="84fc9-134">Klicken Sie auf **Fertig stellen**.</span><span class="sxs-lookup"><span data-stu-id="84fc9-134">Click **Finish**.</span></span>

<span data-ttu-id="84fc9-135">Im Entity Designer, der bietet eine Entwurfsoberfläche zum Bearbeiten des Modells, wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="84fc9-135">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="84fc9-136">Alle Objekte, die Sie im Dialogfeld "Datenbankobjekte auswählen" ausgewählt werden mit dem Modell hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="84fc9-136">All the objects that you selected in the Choose Your Database Objects dialog box are added to the model.</span></span>

## <a name="implement-table-per-type-inheritance"></a><span data-ttu-id="84fc9-137">Implementieren von Vererbung von Tabelle pro Typ</span><span class="sxs-lookup"><span data-stu-id="84fc9-137">Implement Table-per-Type Inheritance</span></span>

-   <span data-ttu-id="84fc9-138">Klicken Sie auf der Entwurfsoberfläche mit der Maustaste der **OnlineCourse** Entitätstyp, und wählen Sie **Eigenschaften**.</span><span class="sxs-lookup"><span data-stu-id="84fc9-138">On the design surface, right-click the **OnlineCourse** entity type and select **Properties**.</span></span>
-   <span data-ttu-id="84fc9-139">In der **Eigenschaften** legen die Base-Type-Eigenschaft auf **Kurs**.</span><span class="sxs-lookup"><span data-stu-id="84fc9-139">In the **Properties** window, set the Base Type property to **Course**.</span></span>
-   <span data-ttu-id="84fc9-140">Mit der rechten Maustaste die **"OnsiteCourse"** Entitätstyp, und wählen Sie **Eigenschaften**.</span><span class="sxs-lookup"><span data-stu-id="84fc9-140">Right-click the **OnsiteCourse** entity type and select **Properties**.</span></span>
-   <span data-ttu-id="84fc9-141">In der **Eigenschaften** legen die Base-Type-Eigenschaft auf **Kurs**.</span><span class="sxs-lookup"><span data-stu-id="84fc9-141">In the **Properties** window, set the Base Type property to **Course**.</span></span>
-   <span data-ttu-id="84fc9-142">Mit der rechten Maustaste der Zuordnung (die Linie) zwischen den **OnlineCourse** und **Kurs** Entitätstypen.</span><span class="sxs-lookup"><span data-stu-id="84fc9-142">Right-click the association (the line) between the **OnlineCourse** and **Course** entity types.</span></span>
    <span data-ttu-id="84fc9-143">Wählen Sie **aus Modell löschen**.</span><span class="sxs-lookup"><span data-stu-id="84fc9-143">Select **Delete from Model**.</span></span>
-   <span data-ttu-id="84fc9-144">Mit der rechten Maustaste in der Zuordnung zwischen der **"OnsiteCourse"** und **Kurs** Entitätstypen.</span><span class="sxs-lookup"><span data-stu-id="84fc9-144">Right-click the association between the **OnsiteCourse** and **Course** entity types.</span></span>
    <span data-ttu-id="84fc9-145">Wählen Sie **aus Modell löschen**.</span><span class="sxs-lookup"><span data-stu-id="84fc9-145">Select **Delete from Model**.</span></span>

<span data-ttu-id="84fc9-146">Wir werden nun gelöscht. die **CourseID** Eigenschaft **OnlineCourse** und **"OnsiteCourse"** , da diese Klassen erben **CourseID** aus die **Kurs** Basistyp.</span><span class="sxs-lookup"><span data-stu-id="84fc9-146">We will now delete the **CourseID** property from **OnlineCourse** and **OnsiteCourse** because these classes inherit **CourseID** from the **Course** base type.</span></span>

-   <span data-ttu-id="84fc9-147">Mit der rechten Maustaste die **CourseID** Eigenschaft der **OnlineCourse** Entitätstyps, und wählen Sie dann **aus Modell löschen**.</span><span class="sxs-lookup"><span data-stu-id="84fc9-147">Right-click the **CourseID** property of the **OnlineCourse** entity type, and then select **Delete from Model**.</span></span>
-   <span data-ttu-id="84fc9-148">Mit der rechten Maustaste die **CourseID** Eigenschaft der **"OnsiteCourse"** Entitätstyps, und wählen Sie dann **aus Modell löschen**</span><span class="sxs-lookup"><span data-stu-id="84fc9-148">Right-click the **CourseID** property of the **OnsiteCourse** entity type, and then select **Delete from Model**</span></span>
-   <span data-ttu-id="84fc9-149">Die "Tabelle pro Typ"-Vererbung ist damit implementiert.</span><span class="sxs-lookup"><span data-stu-id="84fc9-149">Table-per-type inheritance is now implemented.</span></span>

![TPT](~/ef6/media/tpt.png)

## <a name="use-the-model"></a><span data-ttu-id="84fc9-151">Verwenden Sie das Modell</span><span class="sxs-lookup"><span data-stu-id="84fc9-151">Use the Model</span></span>

<span data-ttu-id="84fc9-152">Öffnen der **"Program.cs"** -Datei, in der **Main** Methode definiert ist.</span><span class="sxs-lookup"><span data-stu-id="84fc9-152">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="84fc9-153">Fügen Sie den folgenden Code in die **Main** Funktion.</span><span class="sxs-lookup"><span data-stu-id="84fc9-153">Paste the following code into the **Main** function.</span></span> <span data-ttu-id="84fc9-154">Der Code ausgeführt wird, drei Abfragen.</span><span class="sxs-lookup"><span data-stu-id="84fc9-154">The code executes three queries.</span></span> <span data-ttu-id="84fc9-155">Die erste Abfrage gibt wieder alle **Kurse** im Zusammenhang mit der angegebenen Abteilung.</span><span class="sxs-lookup"><span data-stu-id="84fc9-155">The first query brings back all **Courses** related to the specified department.</span></span> <span data-ttu-id="84fc9-156">Die zweite Abfrage verwendet die **OfType** -Methode zur Rückgabe **OnlineCourses** im Zusammenhang mit der angegebenen Abteilung.</span><span class="sxs-lookup"><span data-stu-id="84fc9-156">The second query uses the **OfType** method to return **OnlineCourses** related to the specified department.</span></span> <span data-ttu-id="84fc9-157">Gibt zurück, die dritte Abfrage **OnsiteCourses**.</span><span class="sxs-lookup"><span data-stu-id="84fc9-157">The third query returns **OnsiteCourses**.</span></span>

``` csharp
    using (var context = new SchoolEntities())
    {
        foreach (var department in context.Departments)
        {
            Console.WriteLine("The {0} department has the following courses:",
                               department.Name);

            Console.WriteLine("   All courses");
            foreach (var course in department.Courses )
            {
                Console.WriteLine("     {0}", course.Title);
            }

            foreach (var course in department.Courses.
                OfType<OnlineCourse>())
            {
                Console.WriteLine("   Online - {0}", course.Title);
            }

            foreach (var course in department.Courses.
                OfType<OnsiteCourse>())
            {
                Console.WriteLine("   Onsite - {0}", course.Title);
            }
        }
    }
```
