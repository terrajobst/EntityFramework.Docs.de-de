---
title: Automatische Code First-Migrationen – EF 6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 0eb86787-2161-4cb4-9cb8-67c5d6e95650
caps.latest.revision: 3
ms.openlocfilehash: 1f6ed728271e38d8e65fcf33fd45c74f085d9178
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2018
ms.locfileid: "39121114"
---
# <a name="automatic-code-first-migrations"></a><span data-ttu-id="d157e-102">Automatische Code First-Migrationen</span><span class="sxs-lookup"><span data-stu-id="d157e-102">Automatic Code First Migrations</span></span>
<span data-ttu-id="d157e-103">Automatische Migrationen können Sie Code First-Migrationen verwenden, ohne dass eine Codedatei im Projekt für jede Änderung, die Sie vornehmen.</span><span class="sxs-lookup"><span data-stu-id="d157e-103">Automatic Migrations allows you to use Code First Migrations without having a code file in your project for each change you make.</span></span> <span data-ttu-id="d157e-104">Nicht alle Änderungen automatisch angewendet werden können – z. B. Umbenennen der Spalte ist die Verwendung einer codebasierten Migration erforderlich.</span><span class="sxs-lookup"><span data-stu-id="d157e-104">Not all changes can be applied automatically - for example column renames require the use of a code-based migration.</span></span>

> [!NOTE]
> <span data-ttu-id="d157e-105">In diesem Artikel wird davon ausgegangen, dass Sie wissen, wie Code First-Migrationen in grundlegenden Szenarien verwenden.</span><span class="sxs-lookup"><span data-stu-id="d157e-105">This article assumes you know how to use Code First Migrations in basic scenarios.</span></span> <span data-ttu-id="d157e-106">Wenn Sie dies nicht tun, müssen Sie lesen [Code First-Migrationen](~/ef6/modeling/code-first/migrations/index.md) vor dem fortfahren.</span><span class="sxs-lookup"><span data-stu-id="d157e-106">If you don’t, then you’ll need to read [Code First Migrations](~/ef6/modeling/code-first/migrations/index.md) before continuing.</span></span>

## <a name="recommendation-for-team-environments"></a><span data-ttu-id="d157e-107">Empfehlung für Teamumgebungen</span><span class="sxs-lookup"><span data-stu-id="d157e-107">Recommendation for Team Environments</span></span>

<span data-ttu-id="d157e-108">Sie können automatische als auch Code basierende Migrationen hier und da, aber dies wird nicht empfohlen, im Team-Entwicklungsszenarios.</span><span class="sxs-lookup"><span data-stu-id="d157e-108">You can intersperse automatic and code-based migrations but this is not recommended in team development scenarios.</span></span> <span data-ttu-id="d157e-109">Wenn Sie ein Team von Entwicklern gehören, die quellcodeverwaltung verwenden, sollten Sie entweder ausschließlich automatische Migrationen oder ausschließlich codebasierten Migrationen verwenden.</span><span class="sxs-lookup"><span data-stu-id="d157e-109">If you are part of a team of developers that use source control you should either use purely automatic migrations or purely code-based migrations.</span></span> <span data-ttu-id="d157e-110">Aufgrund der Einschränkungen der automatische Migrationen wird die Verwendung von Code-basierte Migrationen in teamumgebungen empfohlen.</span><span class="sxs-lookup"><span data-stu-id="d157e-110">Given the limitations of automatic migrations we recommend using code-based migrations in team environments.</span></span>

## <a name="building-an-initial-model--database"></a><span data-ttu-id="d157e-111">Erstellen eines Ausgangsmodells und einer Ausgangsdatenbank</span><span class="sxs-lookup"><span data-stu-id="d157e-111">Building an Initial Model & Database</span></span>

<span data-ttu-id="d157e-112">Bevor Sie Migrationen verwenden können, benötigen Sie ein Projekt und ein Code First-Modell.</span><span class="sxs-lookup"><span data-stu-id="d157e-112">Before we start using migrations we need a project and a Code First model to work with.</span></span> <span data-ttu-id="d157e-113">In dieser exemplarischen Vorgehensweise werden die kanonischen Modelle **Blog** und **Post** verwendet.</span><span class="sxs-lookup"><span data-stu-id="d157e-113">For this walkthrough we are going to use the canonical **Blog** and **Post** model.</span></span>

-   <span data-ttu-id="d157e-114">Erstellen Sie ein neues **MigrationsAutomaticDemo** Konsolenanwendung</span><span class="sxs-lookup"><span data-stu-id="d157e-114">Create a new **MigrationsAutomaticDemo** Console application</span></span>
-   <span data-ttu-id="d157e-115">Fügen Sie dem Projekt die neueste Version des NuGet-Pakets **EntityFramework** hinzu.</span><span class="sxs-lookup"><span data-stu-id="d157e-115">Add the latest version of the **EntityFramework** NuGet package to the project</span></span>
    -   <span data-ttu-id="d157e-116">Wählen Sie **Tools > Bibliotheks-Paket-Manger > Paket-Manager-Konsole** aus.</span><span class="sxs-lookup"><span data-stu-id="d157e-116">**Tools –&gt; Library Package Manager –&gt; Package Manager Console**</span></span>
    -   <span data-ttu-id="d157e-117">Führen Sie den Befehl **Install-Package EntityFramework** aus.</span><span class="sxs-lookup"><span data-stu-id="d157e-117">Run the **Install-Package EntityFramework** command</span></span>
-   <span data-ttu-id="d157e-118">Fügen Sie eine **Model.cs**-Datei mit dem unten aufgeführten Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="d157e-118">Add a **Model.cs** file with the code shown below.</span></span> <span data-ttu-id="d157e-119">Mit diesem Code wird eine einzelne **Blog**-Klasse, die das Domänenmodell bildet, und eine **BlogContext**-Klasse, die den EF Code First-Kontext darstellt, definiert.</span><span class="sxs-lookup"><span data-stu-id="d157e-119">This code defines a single **Blog** class that makes up our domain model and a **BlogContext** class that is our EF Code First context</span></span>

  ``` csharp
      using System.Data.Entity;
      using System.Collections.Generic;
      using System.ComponentModel.DataAnnotations;
      using System.Data.Entity.Infrastructure;

      namespace MigrationsAutomaticDemo
      {
          public class BlogContext : DbContext
          {
              public DbSet<Blog> Blogs { get; set; }
          }

          public class Blog
          {
              public int BlogId { get; set; }
              public string Name { get; set; }
          }
      }
  ```

-   <span data-ttu-id="d157e-120">Mit dem Modell können Sie nun auf die Daten zugreifen.</span><span class="sxs-lookup"><span data-stu-id="d157e-120">Now that we have a model it’s time to use it to perform data access.</span></span> <span data-ttu-id="d157e-121">Aktualisieren Sie die **Program.cs**-Datei mit dem folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="d157e-121">Update the **Program.cs** file with the code shown below.</span></span>

  ``` csharp
      using System;
      using System.Collections.Generic;
      using System.Linq;
      using System.Text;

      namespace MigrationsAutomaticDemo
      {
          class Program
          {
              static void Main(string[] args)
              {
                  using (var db = new BlogContext())
                  {
                      db.Blogs.Add(new Blog { Name = "Another Blog " });
                      db.SaveChanges();

                      foreach (var blog in db.Blogs)
                      {
                          Console.WriteLine(blog.Name);
                      }
                  }

                  Console.WriteLine("Press any key to exit...");
                  Console.ReadKey();
              }
          }
      }
  ```

-   <span data-ttu-id="d157e-122">Führen Sie die Anwendung, und Sie sehen, dass eine **MigrationsAutomaticCodeDemo.BlogContext** Datenbank wird für Sie erstellt.</span><span class="sxs-lookup"><span data-stu-id="d157e-122">Run your application and you will see that a **MigrationsAutomaticCodeDemo.BlogContext** database is created for you.</span></span>

    ![DatabaseLocalDB](~/ef6/media/databaselocaldb.png)

## <a name="enabling-migrations"></a><span data-ttu-id="d157e-124">Aktivieren von Migrationen</span><span class="sxs-lookup"><span data-stu-id="d157e-124">Enabling Migrations</span></span>

<span data-ttu-id="d157e-125">Im Folgenden erfahren Sie, wie Sie weitere Änderungen am Modell vornehmen.</span><span class="sxs-lookup"><span data-stu-id="d157e-125">It’s time to make some more changes to our model.</span></span>

-   <span data-ttu-id="d157e-126">Fügen Sie der Blog-Klasse eine URL-Eigenschaft hinzu.</span><span class="sxs-lookup"><span data-stu-id="d157e-126">Let’s introduce a Url property to the Blog class.</span></span>

``` csharp
    public string Url { get; set; }
```

<span data-ttu-id="d157e-127">Wenn Sie zu diesem Zeitpunkt die Anwendung erneut ausführen würden, würden Sie die Ausnahme „InvalidOperationException“ mit folgendem Text erhalten: *The model backing the 'BlogContext' context has changed since the database was created. Consider using Code First Migrations to update the database (* [*http://go.microsoft.com/fwlink/?LinkId=238269*](http://go.microsoft.com/fwlink/?LinkId=238269)*) (Das Modell, das den Kontext „BlogContext“ unterstützt, wurde seit der Erstellung der Datenbank verändert. Verwenden Sie Code First-Migrationen, um die Datenbank zu aktualisieren).*</span><span class="sxs-lookup"><span data-stu-id="d157e-127">If you were to run the application again you would get an InvalidOperationException stating *The model backing the 'BlogContext' context has changed since the database was created. Consider using Code First Migrations to update the database (* [*http://go.microsoft.com/fwlink/?LinkId=238269*](http://go.microsoft.com/fwlink/?LinkId=238269)*).*</span></span>

<span data-ttu-id="d157e-128">Beginnen Sie daher mit der Verwendung von Code First-Migrationen.</span><span class="sxs-lookup"><span data-stu-id="d157e-128">As the exception suggests, it’s time to start using Code First Migrations.</span></span> <span data-ttu-id="d157e-129">Da wir automatische Migrationen verwenden möchten wir werden an die **– EnableAutomaticMigrations** wechseln.</span><span class="sxs-lookup"><span data-stu-id="d157e-129">Because we want to use automatic migrations we’re going to specify the **–EnableAutomaticMigrations** switch.</span></span>

-   <span data-ttu-id="d157e-130">Führen Sie die **Enable-Migrations-EnableAutomaticMigrations** Befehl in der Paket-Manager-Konsole für diesen Befehl wurde hinzugefügt. eine **Migrationen** Ordner, um das Projekt.</span><span class="sxs-lookup"><span data-stu-id="d157e-130">Run the **Enable-Migrations –EnableAutomaticMigrations** command in Package Manager Console This command has added a **Migrations** folder to our project.</span></span> <span data-ttu-id="d157e-131">Diesen neue Ordner enthält eine Datei:</span><span class="sxs-lookup"><span data-stu-id="d157e-131">This new folder contains one file:</span></span>

-   <span data-ttu-id="d157e-132">**Die Dateikonfigurationsklasse:**</span><span class="sxs-lookup"><span data-stu-id="d157e-132">**The Configuration class.**</span></span> <span data-ttu-id="d157e-133">Mit dieser Klasse können Sie das Verhalten von Migrationen für den Kontext konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="d157e-133">This class allows you to configure how Migrations behaves for your context.</span></span> <span data-ttu-id="d157e-134">Verwenden Sie für diese exemplarische Vorgehensweise die Standardkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="d157e-134">For this walkthrough we will just use the default configuration.</span></span>
    <span data-ttu-id="d157e-135">*Da in Ihrem Projekt nur ein einzelner Code First-Kontext vorhanden ist, wurde der Kontexttyp, der für diese Konfiguration gilt, automatisch von „Enable-Migrations“ ausgefüllt.*</span><span class="sxs-lookup"><span data-stu-id="d157e-135">*Because there is just a single Code First context in your project, Enable-Migrations has automatically filled in the context type this configuration applies to.*</span></span>

 

## <a name="your-first-automatic-migration"></a><span data-ttu-id="d157e-136">Ihre erste automatische Migration</span><span class="sxs-lookup"><span data-stu-id="d157e-136">Your First Automatic Migration</span></span>

<span data-ttu-id="d157e-137">Code First-Migrationen verfügt über zwei primäre Befehle, die Sie im Folgenden kennenlernen werden:</span><span class="sxs-lookup"><span data-stu-id="d157e-137">Code First Migrations has two primary commands that you are going to become familiar with.</span></span>

-   <span data-ttu-id="d157e-138">**Add-Migration** erstellt den Entwurf für die nächste Migration basierend auf Änderungen, die Sie seit dem Erstellen der letzten Migration an Ihrem Modell vorgenommen haben.</span><span class="sxs-lookup"><span data-stu-id="d157e-138">**Add-Migration** will scaffold the next migration based on changes you have made to your model since the last migration was created</span></span>
-   <span data-ttu-id="d157e-139">**Update-Database** übernimmt alle ausstehenden Migrationen für die Datenbank.</span><span class="sxs-lookup"><span data-stu-id="d157e-139">**Update-Database** will apply any pending migrations to the database</span></span>

<span data-ttu-id="d157e-140">Wir wollen vermeiden Sie mithilfe der Add-Migration (es sei denn, es unbedingt erforderlich) und den Fokus auf, sodass Code First-Migrationen automatisch berechnen und die Änderungen anzuwenden.</span><span class="sxs-lookup"><span data-stu-id="d157e-140">We are going to avoid using Add-Migration (unless we really need to) and focus on letting Code First Migrations automatically calculate and apply the changes.</span></span> <span data-ttu-id="d157e-141">Verwenden wir **Update-Database** zum Abrufen von Code First-Migrationen unseres Modells die Änderungen übertragen (die neue **Blog.Ur**l-Eigenschaft) in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="d157e-141">Let’s use **Update-Database** to get Code First Migrations to push the changes to our model (the new **Blog.Ur**l property) to the database.</span></span>

-   <span data-ttu-id="d157e-142">Führen Sie die **Update-Database** im Paket-Manager-Konsole den Befehl.</span><span class="sxs-lookup"><span data-stu-id="d157e-142">Run the **Update-Database** command in Package Manager Console.</span></span>

<span data-ttu-id="d157e-143">Die **MigrationsAutomaticDemo.BlogContext** Datenbank wird jetzt aktualisiert die **Url** -Spalte in der **Blogs** Tabelle.</span><span class="sxs-lookup"><span data-stu-id="d157e-143">The **MigrationsAutomaticDemo.BlogContext** database is now updated to include the **Url** column in the **Blogs** table.</span></span>

 

## <a name="your-second-automatic-migration"></a><span data-ttu-id="d157e-144">Der zweiten automatische Migration</span><span class="sxs-lookup"><span data-stu-id="d157e-144">Your Second Automatic Migration</span></span>

<span data-ttu-id="d157e-145">Lassen Sie uns eine andere ändern, sodass Code First-Migrationen automatisch die Änderungen in der Datenbank übernommen, für uns darüber.</span><span class="sxs-lookup"><span data-stu-id="d157e-145">Let’s make another change and let Code First Migrations automatically push the changes to the database for us.</span></span>

-   <span data-ttu-id="d157e-146">Fügen Sie zudem eine neue **Post**-Klasse hinzu.</span><span class="sxs-lookup"><span data-stu-id="d157e-146">Let's also add a new **Post** class</span></span>

``` csharp
    public class Post
    {
        public int PostId { get; set; }
        [MaxLength(200)]
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public Blog Blog { get; set; }
    }
```

-   <span data-ttu-id="d157e-147">Fügen Sie anschließend der **Blog**-Klasse eine **Posts**-Sammlung hinzu, die das andere Ende der Beziehung zwischen **Blog** und **Post** bilden soll.</span><span class="sxs-lookup"><span data-stu-id="d157e-147">We'll also add a **Posts** collection to the **Blog** class to form the other end of the relationship between **Blog** and **Post**</span></span>

``` csharp
    public virtual List<Post> Posts { get; set; }
```

<span data-ttu-id="d157e-148">Verwenden Sie jetzt **Update-Database** auf die Datenbank auf dem neuesten Stand zu bringen.</span><span class="sxs-lookup"><span data-stu-id="d157e-148">Now use **Update-Database** to bring the database up-to-date.</span></span> <span data-ttu-id="d157e-149">Geben Sie dieses Mal das Flag **–Verbose** an, damit das SQL angezeigt wird, mit dem Code First-Migrationen ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="d157e-149">This time let’s specify the **–Verbose** flag so that you can see the SQL that Code First Migrations is running.</span></span>

-   <span data-ttu-id="d157e-150">Führen Sie in der Paket-Manager-Konsole den Befehl **Update-Database –Verbose** aus.</span><span class="sxs-lookup"><span data-stu-id="d157e-150">Run the **Update-Database –Verbose** command in Package Manager Console.</span></span>

## <a name="adding-a-code-based-migration"></a><span data-ttu-id="d157e-151">Hinzufügen eines Codes Sammlungsbasierten Migration</span><span class="sxs-lookup"><span data-stu-id="d157e-151">Adding a Code Based Migration</span></span>

<span data-ttu-id="d157e-152">Jetzt sehen wir uns etwas möglicherweise eine Migration codebasierte für verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="d157e-152">Now let’s look at something we might want to use a code-based migration for.</span></span>

-   <span data-ttu-id="d157e-153">Fügen Sie eine **Bewertung** Eigenschaft, um die **Blog** Klasse</span><span class="sxs-lookup"><span data-stu-id="d157e-153">Let’s add a **Rating** property to the **Blog** class</span></span>

``` csharp
    public int Rating { get; set; }
```

<span data-ttu-id="d157e-154">Wir können einfach ausführen **Update-Database** diese Änderungen in der Datenbank zu übertragen.</span><span class="sxs-lookup"><span data-stu-id="d157e-154">We could just run **Update-Database** to push these changes to the database.</span></span> <span data-ttu-id="d157e-155">Allerdings fügen wir einen NULL- **Blogs.Rating** Spalte, wenn alle vorhandenen Daten in der Tabelle vorhanden ist. es zugewiesen den CLR-Standardwert des Datentyps für die neue Spalte (Bewertung ganze Zahl ist, also **0**).</span><span class="sxs-lookup"><span data-stu-id="d157e-155">However, we're adding a non-nullable **Blogs.Rating** column, if there is any existing data in the table it will get assigned the CLR default of the data type for new column (Rating is integer, so that would be **0**).</span></span> <span data-ttu-id="d157e-156">Damit die vorhandenen Spalten der **Blogs**-Tabelle mit einem vernünftigen Rating-Wert beginnen, geben Sie den Standardwert **3** an.</span><span class="sxs-lookup"><span data-stu-id="d157e-156">But we want to specify a default value of **3** so that existing rows in the **Blogs** table will start with a decent rating.</span></span>
<span data-ttu-id="d157e-157">Wir verwenden Sie den Befehl Add-Migration, um diese Änderung sich für eine Migration codebasierte zu schreiben, damit wir diese bearbeiten können.</span><span class="sxs-lookup"><span data-stu-id="d157e-157">Let’s use the Add-Migration command to write this change out to a code-based migration so that we can edit it.</span></span> <span data-ttu-id="d157e-158">Die **Add-Migration** Befehl ermöglicht es uns, benennen Sie diese Migrationen, wir nennen unsere **AddBlogRating**.</span><span class="sxs-lookup"><span data-stu-id="d157e-158">The **Add-Migration** command allows us to give these migrations a name, let’s just call ours **AddBlogRating**.</span></span>

-   <span data-ttu-id="d157e-159">Führen Sie die **Add-Migration AddBlogRating** im Paket-Manager-Konsole den Befehl.</span><span class="sxs-lookup"><span data-stu-id="d157e-159">Run the **Add-Migration AddBlogRating** command in Package Manager Console.</span></span>
-   <span data-ttu-id="d157e-160">In der **Migrationen** Ordner "" Wir verfügen jetzt über ein neues **AddBlogRating** Migration.</span><span class="sxs-lookup"><span data-stu-id="d157e-160">In the **Migrations** folder we now have a new **AddBlogRating** migration.</span></span> <span data-ttu-id="d157e-161">Der Dateiname für die Migration ist bereits mit einem Zeitstempel fest, um mit der Sortierung zu.</span><span class="sxs-lookup"><span data-stu-id="d157e-161">The migration filename is pre-fixed with a timestamp to help with ordering.</span></span> <span data-ttu-id="d157e-162">Ermöglicht das Bearbeiten des generierten Codes zum Angeben von eines Standardwert von 3 für Blog.Rating (Zeile 10 in den folgenden Code)</span><span class="sxs-lookup"><span data-stu-id="d157e-162">Let’s edit the generated code to specify a default value of 3 for Blog.Rating (Line 10 in the code below)</span></span>

<span data-ttu-id="d157e-163">*Die Migration besteht auch eine CodeBehind-Datei, die einige Metadaten erfasst werden. Diese Metadaten können auf den Code First-Migrationen die automatische Migrationen zu replizieren, die wir vor diesem codebasierte Migration durchgeführt. Dies ist wichtig, wenn ein anderer Entwickler unsere Migrationen ausgeführt werden soll, oder wenn es Zeit für die Bereitstellung der Anwendung ist.*</span><span class="sxs-lookup"><span data-stu-id="d157e-163">*The migration also has a code-behind file that captures some metadata. This metadata will allow Code First Migrations to replicate the automatic migrations we performed before this code-based migration. This is important if another developer wants to run our migrations or when it’s time to deploy our application.*</span></span>

``` csharp
    namespace MigrationsAutomaticDemo.Migrations
    {
        using System;
        using System.Data.Entity.Migrations;

        public partial class AddBlogRating : DbMigration
        {
            public override void Up()
            {
                AddColumn("Blogs", "Rating", c => c.Int(nullable: false, defaultValue: 3));
            }

            public override void Down()
            {
                DropColumn("Blogs", "Rating");
            }
        }
    }
```

<span data-ttu-id="d157e-164">Die Migration ist nun fertig bearbeitet. Aktualisieren Sie daher die Datenbank mit **Update-Database**.</span><span class="sxs-lookup"><span data-stu-id="d157e-164">Our edited migration is looking good, so let’s use **Update-Database** to bring the database up-to-date.</span></span>

-   <span data-ttu-id="d157e-165">Führen Sie die **Update-Database** im Paket-Manager-Konsole den Befehl.</span><span class="sxs-lookup"><span data-stu-id="d157e-165">Run the **Update-Database** command in Package Manager Console.</span></span>

## <a name="back-to-automatic-migrations"></a><span data-ttu-id="d157e-166">Automatische Migrationen an</span><span class="sxs-lookup"><span data-stu-id="d157e-166">Back to Automatic Migrations</span></span>

<span data-ttu-id="d157e-167">Wir können jetzt wechseln Sie zurück zur automatische Migrationen für unsere Änderungen einfacheren.</span><span class="sxs-lookup"><span data-stu-id="d157e-167">We are now free to switch back to automatic migrations for our simpler changes.</span></span> <span data-ttu-id="d157e-168">Code First-Migrationen gelten für die automatischen und Code-basierte Migrationen durchführen, in der richtigen Reihenfolge basierend auf den Metadaten, die, den Sie in der CodeBehind-Datei für jede Migration codebasierte gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="d157e-168">Code First Migrations will take care of performing the automatic and code-based migrations in the correct order based on the metadata it is storing in the code-behind file for each code-based migration.</span></span>

-   <span data-ttu-id="d157e-169">Fügen Sie eine Eigenschaft Post.Abstract unseres Modells</span><span class="sxs-lookup"><span data-stu-id="d157e-169">Let’s add a Post.Abstract property to our model</span></span>

``` csharp
    public string Abstract { get; set; }
```

<span data-ttu-id="d157e-170">Nun können wir **Update-Database** zum Abrufen von Code First-Migrationen diese Änderung für eine automatische Migration mithilfe der Datenbank mithilfe von Push übertragen.</span><span class="sxs-lookup"><span data-stu-id="d157e-170">Now we can use **Update-Database** to get Code First Migrations to push this change to the database using an automatic migration.</span></span>

-   <span data-ttu-id="d157e-171">Führen Sie die **Update-Database** im Paket-Manager-Konsole den Befehl.</span><span class="sxs-lookup"><span data-stu-id="d157e-171">Run the **Update-Database** command in Package Manager Console.</span></span>

## <a name="summary"></a><span data-ttu-id="d157e-172">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="d157e-172">Summary</span></span>

<span data-ttu-id="d157e-173">Modell wird in dieser exemplarischen Vorgehensweise, die Sie haben gesehen, wie Sie mit, dass automatische Migrationen mithilfe von Push übertragen an die Datenbank geändert.</span><span class="sxs-lookup"><span data-stu-id="d157e-173">In this walkthrough you saw how to use automatic migrations to push model changes to the database.</span></span> <span data-ttu-id="d157e-174">Sie haben auch das Gerüst erstellen und Code-basierte Migrationen zwischen automatische Migrationen ausgeführt werden, wenn Sie mehr Kontrolle benötigen.</span><span class="sxs-lookup"><span data-stu-id="d157e-174">You also saw how to scaffold and run code-based migrations in between automatic migrations when you need more control.</span></span>
