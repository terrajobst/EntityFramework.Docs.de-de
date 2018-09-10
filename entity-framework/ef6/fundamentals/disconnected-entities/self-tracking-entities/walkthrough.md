---
title: Self-Tracking Entities Exemplarische Vorgehensweise – EF6
author: divega
ms.date: 2016-10-23
ms.assetid: b21207c9-1d95-4aa3-ae05-bc5fe300dab0
ms.openlocfilehash: 1c450bbb20c246d9b9d58707ac03eb48eadfa970
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/09/2018
ms.locfileid: "44251283"
---
# <a name="self-tracking-entities-walkthrough"></a>Self-Tracking Entities Exemplarische Vorgehensweise
> [!IMPORTANT]
> Es wird nicht mehr empfohlen, die Vorlage für Entitäten mit Selbstnachverfolgung zu verwenden. Die Vorlage ist nur für die Unterstützung vorhandener Anwendungen weiterhin verfügbar. Wenn für Ihre Anwendung die Arbeit mit unverbundenen Diagrammen von Entitäten erforderlich ist, sollten Sie daher Alternativen erwägen, wie z.B. [nachverfolgbare Entitäten](http://trackableentities.github.io/). Diese Technologie ähnelt den Entitäten mit Selbstnachverfolgung und wird von der Community aktiver entwickelt. Alternativ dazu können Sie auch benutzerdefinierten Code mithilfe von APIs auf niedriger Ebene zur Änderungsnachverfolgung schreiben.

Diese exemplarische Vorgehensweise veranschaulicht das Szenario, in dem ein Windows Communication Foundation (WCF)-Dienst einen Vorgang verfügbar macht, der ein Diagramm für die Entität zurückgibt. Als Nächstes eine Clientanwendung das Diagramm bearbeitet und übermittelt die Änderungen auf einen Dienstvorgang, der überprüft und die Updates in einer Datenbank mithilfe von Entity Framework speichert.

Vor dem Durchführen dieser exemplarischen Vorgehensweise stellen Sie sicher, dass Sie lesen die [Self-Tracking Entities](index.md) Seite.

Diese exemplarische Vorgehensweise umfasst die folgenden Aktionen:

-   Erstellt eine Datenbank auf.
-   Erstellt eine Klassenbibliothek mit dem Modell an.
-   Austausch von Bereitstellungen der Self-Tracking Entity-Generator-Vorlage.
-   Wird die Entitätsklassen in ein separates Projekt verschoben.
-   Erstellt einen WCF-Dienst, der Vorgänge zum Abfragen und Speichern von Entitäten bereitstellt.
-   Erstellt Anwendungen (Konsole und WPF), die den Dienst zu nutzen.

Database First in dieser exemplarischen Vorgehensweise verwendet, aber die gleichen Techniken gelten auch für die Model First.

## <a name="pre-requisites"></a>Voraussetzungen

Zum Durchführen dieser exemplarischen Vorgehensweise benötigen Sie eine aktuelle Version von Visual Studio.

## <a name="create-a-database"></a>Erstellen Sie eine Datenbank

Der Datenbankserver, der mit Visual Studio installiert ist, unterscheidet sich abhängig von der Version von Visual Studio, die Sie installiert haben:

-   Wenn Sie Visual Studio 2012 verwenden, wird Sie eine LocalDB-Datenbank erstellen.
-   Wenn Sie Visual Studio 2010 verwenden erstellen Sie eine SQL Express-Datenbank.

Wir jetzt, und Erstellen der Datenbank.

-   Öffnen Sie Visual Studio.
-   **Ansicht – Profiler -&gt; Server-Explorer**
-   Klicken Sie mit der rechten Maustaste auf **Datenverbindungen -&gt; Verbindung hinzufügen...**
-   Wenn Sie vor dem müssen auswählen, im Server-Explorer mit einer Datenbank verbunden haben **Microsoft SQL Server** als Datenquelle
-   Eine Verbindung mit LocalDB oder SQL Express, je nachdem, welches Sie installiert haben
-   Geben Sie **STESample** als Datenbankname
-   Wählen Sie **OK** und Sie werden gefragt, ob Sie eine neue Datenbank, die auf erstellen möchten **Ja**
-   Die neue Datenbank wird jetzt im Server-Explorer angezeigt.
-   Bei Verwendung von Visual Studio 2012
    -   Mit der rechten Maustaste auf die Datenbank im Server-Explorer, und wählen Sie **neue Abfrage**
    -   Kopieren Sie die folgende SQL-Anweisung in die neue Abfrage, und klicken Sie dann mit der rechten Maustaste auf die Abfrage, und wählen **ausführen**
-   Wenn Sie Visual Studio 2010 verwenden
    -   Wählen Sie **Daten per Push –&gt; Transact-SQL-Editor –&gt; neue Abfrageverbindung...**
    -   Geben Sie **.\\ SQLEXPRESS** als Server ein, und klicken Sie auf **OK**
    -   Wählen Sie die **STESample** Datenbank aus der Dropdownliste unten am oberen Rand des Abfrage-Editors
    -   Kopieren Sie die folgende SQL-Anweisung in die neue Abfrage, und klicken Sie dann mit der rechten Maustaste auf die Abfrage, und wählen **SQL ausführen**

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

## <a name="create-the-model"></a>Erstellen des Modells

Eingerichtet ist, benötigen wir zunächst ein Projekt für das Modell.

-   **Datei -&gt; neu –&gt; Projekt...**
-   Wählen Sie **Visual C\#**  im linken Bereich und dann **-Klassenbibliothek**
-   Geben Sie **STESample** als ein, und klicken Sie auf **OK**

Jetzt erstellen wir ein einfaches Modell im EF Designer den Zugriff auf unsere Datenbank:

-   **Projekt -&gt; neues Element hinzufügen...**
-   Wählen Sie **Daten** im linken Bereich und dann **ADO.NET Entity Data Model**
-   Geben Sie **BloggingModel** als ein, und klicken Sie auf **OK**
-   Wählen Sie **aus Datenbank generieren** , und klicken Sie auf **weiter**
-   Geben Sie die Verbindungsinformationen für die Datenbank, die Sie im vorherigen Abschnitt erstellt haben.
-   Geben Sie **BloggingContext** als Namen für die Verbindungszeichenfolge und klicken Sie auf **weiter**
-   Aktivieren Sie das Kontrollkästchen neben **Tabellen** , und klicken Sie auf **Fertig stellen**

## <a name="swap-to-ste-code-generation"></a>Wechseln Sie in die STE-Codegenerierung

Nun müssen wir die standardmäßige codegenerierung und wechseln zu Self-Tracking Entities deaktivieren.

### <a name="if-you-are-using-visual-studio-2012"></a>Bei Verwendung von Visual Studio 2012

-   Erweitern Sie **BloggingModel.edmx** in **Projektmappen-Explorer** und löschen Sie die **BloggingModel.tt** und **BloggingModel.Context.tt** 
     *Dadurch wird die standardmäßige codegenerierung deaktiviert.*
-   Mit der rechten Maustaste auf die EF-Designer, Entwurfsoberfläche, und wählen eines leeren Bereichs **Codegenerierungselement hinzufügen...**
-   Wählen Sie **Online** aus dem linken Bereich und suchen Sie nach **AL-Generator**
-   Wählen Sie die **AL-Generator für C\#**  Vorlage, geben Sie **STETemplate** als ein, und klicken Sie auf **hinzufügen**
-   Die **STETemplate.tt** und **STETemplate.Context.tt** Dateien werden unter der Datei BloggingModel.edmx geschachtelte hinzugefügt

### <a name="if-you-are-using-visual-studio-2010"></a>Wenn Sie Visual Studio 2010 verwenden

-   Mit der rechten Maustaste auf die EF-Designer, Entwurfsoberfläche, und wählen eines leeren Bereichs **Codegenerierungselement hinzufügen...**
-   Wählen Sie **Code** im linken Bereich und dann **ADO.NET-Entitäts-Generator**
-   Geben Sie **STETemplate** als ein, und klicken Sie auf **hinzufügen**
-   Die **STETemplate.tt** und **STETemplate.Context.tt** Dateien werden direkt in Ihrem Projekt hinzugefügt.

## <a name="move-entity-types-into-separate-project"></a>Entitätstypen in separaten Projekt verschieben

Zur Verwendung von Self-Tracking Entities benötigt die Clientanwendung den Zugriff auf die Entitätsklassen aus unserem Modell generiert. Da wir nicht, um das gesamte Modell an die Clientanwendung verfügbar zu machen möchten werden wir die Entitätsklassen in ein separates Projekt verschoben.

Der erste Schritt ist zum Generieren von Entitätsklassen in das vorhandene Projekt zu beenden:

-   Mit der rechten Maustaste auf **STETemplate.tt** in **Projektmappen-Explorer** , und wählen Sie **Eigenschaften**
-   In der **Eigenschaften** Fenster-clear **TextTemplatingFileGenerator** aus der **CustomTool** Eigenschaft
-   Erweitern Sie **STETemplate.tt** in **Projektmappen-Explorer** und löschen Sie alle Dateien, die geschachtelt sind

Als Nächstes werden wir ein neues Projekt hinzufügen und Generieren von Entitätsklassen darin

-   **Datei -&gt; hinzufügen –&gt; Projekt...**
-   Wählen Sie **Visual C\#**  im linken Bereich und dann **-Klassenbibliothek**
-   Geben Sie **STESample.Entities** als ein, und klicken Sie auf **OK**
-   **Projekt -&gt; vorhandenes Element hinzufügen...**
-   Navigieren Sie zu der **STESample** Projektordner
-   Wählen Sie zum Anzeigen von **alle Dateien (\*.\*)**
-   Wählen Sie die **STETemplate.tt** Datei
-   Klicken Sie auf den Dropdownpfeil neben der **hinzufügen** Schaltfläche, und wählen **als Link hinzufügen**

    ![Fügen Sie verknüpfte Vorlage hinzu.](~/ef6/media/addlinkedtemplate.png)

Wir werden auch sicherstellen, dass die Entitätsklassen im selben Namespace wie der Kontext generiert. Dies reduziert lediglich die Anzahl der using-Anweisungen, die wir in unserer Anwendung hinzufügen möchten.

-   Mit der rechten Maustaste auf die verknüpften **STETemplate.tt** in **Projektmappen-Explorer** , und wählen Sie **Eigenschaften**
-   In der **Eigenschaften** legen **Custom Tool Namespace** zu **STESample**

Der Code, der von der AL-Vorlage generiert wird, benötigen einen Verweis auf **System.Runtime.Serialization** zum Kompilieren. Diese Bibliothek ist erforderlich, für die WCF **DataContract** und **DataMember** Attribute, die für die serialisierbaren Entitätstypen verwendet werden.

-   Klicken Sie mit der rechten Maustaste auf die **STESample.Entities** Projekt **Projektmappen-Explorer** , und wählen Sie **Verweis hinzufügen...**
    -   In Visual Studio 2012 – aktivieren Sie das Kontrollkästchen neben **System.Runtime.Serialization** , und klicken Sie auf **OK**
    -   Wählen Sie in Visual Studio 2010 - **System.Runtime.Serialization** , und klicken Sie auf **OK**

Schließlich wird das Projekt mit unserem Kontext darin einen Verweis auf den Entitätstypen benötigen.

-   Klicken Sie mit der rechten Maustaste auf die **STESample** Projekt **Projektmappen-Explorer** , und wählen Sie **Verweis hinzufügen...**
    -   Wählen Sie in Visual Studio 2012 - **Lösung** aktivieren Sie im linken Bereich das Kontrollkästchen neben **STESample.Entities** , und klicken Sie auf **OK**
    -   Wählen Sie in Visual Studio 2010 – die **Projekte** Registerkarte **STESample.Entities** , und klicken Sie auf **OK**

>[!NOTE]
> Eine weitere Möglichkeit zum Verschieben von Entitätstypen in einem separaten Projekt ist, verschieben Sie die Vorlagendatei, anstatt es aus seinem Standardspeicherort zu verknüpfen. Wenn Sie dies tun, müssen Sie zum Aktualisieren der **InputFile** Variable in der Vorlage, geben Sie den relativen Pfad der Edmx-Datei (in diesem Beispiel, die **... \\BloggingModel.edmx**).

## <a name="create-a-wcf-service"></a>Erstellen Sie einen WCF-Dienst

Nun ist es Zeit zum Hinzufügen eines WCF-Diensts, um unsere Daten verfügbar zu machen, beginnen wir, indem Sie das Projekt erstellen.

-   **Datei -&gt; hinzufügen –&gt; Projekt...**
-   Wählen Sie **Visual C\#**  im linken Bereich und dann **WCF-Dienstanwendung**
-   Geben Sie **STESample.Service** als ein, und klicken Sie auf **OK**
-   Hinzufügen eines Verweises auf die **System.Data.Entity** Assembly
-   Hinzufügen eines Verweises auf die **STESample** und **STESample.Entities** Projekte

Wir müssen die EF-Verbindungszeichenfolge auf dieses Projekt zu kopieren, damit sie zur Laufzeit gefunden wird.

-   Öffnen der **"App.config"** -Datei für die ** STESample **-Projekt und kopiert die **ConnectionStrings** Element
-   Fügen Sie der **ConnectionStrings** -Element als untergeordnetes Element des der **Konfiguration** Element der **"Web.config"** Datei die **STESample.Service** Projekt

Jetzt ist es Zeit, um den Dienst zu implementieren.

-   Open **IService1.cs** und Ersetzen Sie den Inhalt durch den folgenden Code

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

-   Open **Service1.svc** und Ersetzen Sie den Inhalt durch den folgenden Code

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

## <a name="consume-the-service-from-a-console-application"></a>Nutzen Sie den Dienst aus einer Konsolenanwendung

Wir erstellen eine Konsolenanwendung, die unseren Dienst verwendet.

-   **Datei -&gt; neu –&gt; Projekt...**
-   Wählen Sie **Visual C\#**  im linken Bereich und dann **-Konsolenanwendung**
-   Geben Sie **STESample.ConsoleTest** als ein, und klicken Sie auf **OK**
-   Hinzufügen eines Verweises auf die **STESample.Entities** Projekt

Wir benötigen einen Dienstverweis an den WCF-Dienst

-   Mit der rechten Maustaste die **STESample.ConsoleTest** Projekt **Projektmappen-Explorer** , und wählen Sie **Dienstverweis hinzufügen...**
-   Klicken Sie auf **ermitteln**
-   Geben Sie **BloggingService** als Namespace, und klicken Sie auf **OK**

Jetzt schreiben wir Code, um den Dienst zu nutzen.

-   Open **"Program.cs"** und Ersetzen Sie den Inhalt durch den folgenden Code.

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

Sie können die Anwendung jetzt ausführen, um sie in Aktion zu sehen.

-   Mit der rechten Maustaste die **STESample.ConsoleTest** Projekt **Projektmappen-Explorer** , und wählen Sie **Debuggen -&gt; neue Instanz starten**

Sie sehen die folgende Ausgabe, wenn die Anwendung ausgeführt wird.

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

## <a name="consume-the-service-from-a-wpf-application"></a>Nutzen Sie den Dienst aus einer WPF-Anwendung

Erstellen wir eine WPF-Anwendung, die unseren Dienst verwendet.

-   **Datei -&gt; neu –&gt; Projekt...**
-   Wählen Sie **Visual C\#**  im linken Bereich und dann **WPF-Anwendung**
-   Geben Sie **STESample.WPFTest** als ein, und klicken Sie auf **OK**
-   Hinzufügen eines Verweises auf die **STESample.Entities** Projekt

Wir benötigen einen Dienstverweis an den WCF-Dienst

-   Mit der rechten Maustaste die **STESample.WPFTest** Projekt **Projektmappen-Explorer** , und wählen Sie **Dienstverweis hinzufügen...**
-   Klicken Sie auf **ermitteln**
-   Geben Sie **BloggingService** als Namespace, und klicken Sie auf **OK**

Jetzt schreiben wir Code, um den Dienst zu nutzen.

-   Open **"MainWindow.xaml"** und Ersetzen Sie den Inhalt durch den folgenden Code.

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

-   Öffnen den Code-behind für MainWindow-Element (**"MainWindow.Xaml.cs"**), und Ersetzen Sie den Inhalt durch den folgenden Code

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

Sie können die Anwendung jetzt ausführen, um sie in Aktion zu sehen.

-   Mit der rechten Maustaste die **STESample.WPFTest** Projekt **Projektmappen-Explorer** , und wählen Sie **Debuggen -&gt; neue Instanz starten**
-   Können Sie die Daten, die mit dem Bildschirm bearbeiten und speichern Sie es über den Dienst mithilfe der **speichern** Schaltfläche

![WPF-Hauptfenster](~/ef6/media/wpf.png)
