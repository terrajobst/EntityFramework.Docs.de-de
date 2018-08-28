---
title: Testen mit der ein Mockframework - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: bd66a638-d245-44d4-8e71-b9c6cb335cc7
ms.openlocfilehash: abb915f2df5645b3838a659dafc59a2b499b4ee2
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998212"
---
# <a name="testing-with-a-mocking-framework"></a><span data-ttu-id="1c74d-102">Testen mit der ein Mockframework</span><span class="sxs-lookup"><span data-stu-id="1c74d-102">Testing with a mocking framework</span></span>
> [!NOTE]
> <span data-ttu-id="1c74d-103">**Nur EF6 und höher:** Die Features, APIs usw., die auf dieser Seite erläutert werden, wurden in Entity Framework 6 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="1c74d-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="1c74d-104">Wenn Sie eine frühere Version verwenden, gelten manche Informationen nicht.</span><span class="sxs-lookup"><span data-stu-id="1c74d-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="1c74d-105">Beim Schreiben von Tests für Ihre Anwendung ist es oft wünschenswert, überschreiten die Datenbank zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="1c74d-105">When writing tests for your application it is often desirable to avoid hitting the database.</span></span>  <span data-ttu-id="1c74d-106">Entitätsframework ermöglicht Ihnen, dies zu erreichen, indem ein Kontext erstellt werden – von den Tests – definierten Verhalten, dass die Daten im Arbeitsspeicher verwendet.</span><span class="sxs-lookup"><span data-stu-id="1c74d-106">Entity Framework allows you to achieve this by creating a context – with behavior defined by your tests – that makes use of in-memory data.</span></span>  

## <a name="options-for-creating-test-doubles"></a><span data-ttu-id="1c74d-107">Optionen zum Erstellen von Testdoubles</span><span class="sxs-lookup"><span data-stu-id="1c74d-107">Options for creating test doubles</span></span>  

<span data-ttu-id="1c74d-108">Es gibt zwei Ansätze, die verwendet werden können, um eine in-Memory-Version Ihres Kontexts erstellen.</span><span class="sxs-lookup"><span data-stu-id="1c74d-108">There are two different approaches that can be used to create an in-memory version of your context.</span></span>  

- <span data-ttu-id="1c74d-109">**Erstellen Sie eigene Testdoubles** – bei diesem Ansatz muss Ihre eigene in-Memory-Implementierung, der den Kontext und die "dbsets" schreiben.</span><span class="sxs-lookup"><span data-stu-id="1c74d-109">**Create your own test doubles** – This approach involves writing your own in-memory implementation of your context and DbSets.</span></span> <span data-ttu-id="1c74d-110">Dies bietet Ihnen viele steuern, wie die Klassen Verhalten sich jedoch betreffen können, schreiben und eine angemessene Menge an Code besitzt.</span><span class="sxs-lookup"><span data-stu-id="1c74d-110">This gives you a lot of control over how the classes behave but can involve writing and owning a reasonable amount of code.</span></span>  
- <span data-ttu-id="1c74d-111">**Verwenden Sie zum Erstellen von Testdoubles ein Mockframework** – Verwendung eines Simulationsframeworks (wie etwa Moq) können Sie die in-Memory-Implementierungen von Ihnen Kontext und legt fest, die dynamisch zur Laufzeit für Sie erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="1c74d-111">**Use a mocking framework to create test doubles** – Using a mocking framework (such as Moq) you can have the in-memory implementations of you context and sets created dynamically at runtime for you.</span></span>  

<span data-ttu-id="1c74d-112">In diesem Artikel wird mit der Verwendung eines Simulationsframeworks bearbeiten.</span><span class="sxs-lookup"><span data-stu-id="1c74d-112">This article will deal with using a mocking framework.</span></span> <span data-ttu-id="1c74d-113">Erstellen eigene Testdoubles finden Sie unter [Tests mit Ihrer eigenen Test-Doubles](writing-test-doubles.md).</span><span class="sxs-lookup"><span data-stu-id="1c74d-113">For creating your own test doubles see [Testing with Your Own Test Doubles](writing-test-doubles.md).</span></span>  

<span data-ttu-id="1c74d-114">Veranschaulicht die Verwendung von EF mit ein Mockframework sind wir Moq verwenden.</span><span class="sxs-lookup"><span data-stu-id="1c74d-114">To demonstrate using EF with a mocking framework we are going to use Moq.</span></span> <span data-ttu-id="1c74d-115">Die einfachste Möglichkeit zum Abrufen von Moq ist zum Installieren der [Moq-Paket von NuGet](http://nuget.org/packages/Moq/).</span><span class="sxs-lookup"><span data-stu-id="1c74d-115">The easiest way to get Moq is to install the [Moq package from NuGet](http://nuget.org/packages/Moq/).</span></span>  

## <a name="testing-with-pre-ef6-versions"></a><span data-ttu-id="1c74d-116">Vor EF6 Versionen testen</span><span class="sxs-lookup"><span data-stu-id="1c74d-116">Testing with pre-EF6 versions</span></span>  

<span data-ttu-id="1c74d-117">In diesem Artikel dargestellte ist abhängig von der wir "DbSet" in EF6 vorgenommenen Änderungen.</span><span class="sxs-lookup"><span data-stu-id="1c74d-117">The scenario shown in this article is dependent on some changes we made to DbSet in EF6.</span></span> <span data-ttu-id="1c74d-118">Testen mit EF5 und frühere Versionen finden Sie unter [Testen mit einem Fake-Kontext](http://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).</span><span class="sxs-lookup"><span data-stu-id="1c74d-118">For testing with EF5 and earlier version see [Testing with a Fake Context](http://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).</span></span>  

## <a name="limitations-of-ef-in-memory-test-doubles"></a><span data-ttu-id="1c74d-119">Einschränkungen von EF-in-Memory-Test-doubles</span><span class="sxs-lookup"><span data-stu-id="1c74d-119">Limitations of EF in-memory test doubles</span></span>  

<span data-ttu-id="1c74d-120">In-Memory-Test-Doubles möglich, dass eine gute Möglichkeit zum Bereitstellen von Komponententests für Servicelevel-Abdeckung der Bits der Anwendung, die EF verwenden.</span><span class="sxs-lookup"><span data-stu-id="1c74d-120">In-memory test doubles can be a good way to provide unit test level coverage of bits of your application that use EF.</span></span> <span data-ttu-id="1c74d-121">Jedoch dabei verwenden Sie LINQ to Objects, um Abfragen für Daten im Arbeitsspeicher auszuführen.</span><span class="sxs-lookup"><span data-stu-id="1c74d-121">However, when doing this you are using LINQ to Objects to execute queries against in-memory data.</span></span> <span data-ttu-id="1c74d-122">Dadurch können zu unterschiedlichem Verhalten als die Verwendung von EF LINQ-Anbieter (LINQ to Entities), um Abfragen in SQL zu übersetzen, die für Ihre Datenbank ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="1c74d-122">This can result in different behavior than using EF’s LINQ provider (LINQ to Entities) to translate queries into SQL that is run against your database.</span></span>  

<span data-ttu-id="1c74d-123">Ein Beispiel für einen solchen Unterschied lädt verwandte Daten.</span><span class="sxs-lookup"><span data-stu-id="1c74d-123">One example of such a difference is loading related data.</span></span> <span data-ttu-id="1c74d-124">Wenn Sie eine Reihe von Blogs erstellen, die jeweils haben Beiträgen und dann bei Verwendung von in-Memory-Daten Beiträge werden immer geladen werden für jeden Blog.</span><span class="sxs-lookup"><span data-stu-id="1c74d-124">If you create a series of Blogs that each have related Posts, then when using in-memory data the related Posts will always be loaded for each Blog.</span></span> <span data-ttu-id="1c74d-125">Allerdings werden bei der Ausführung für eine Datenbank die Daten nur geladen werden, wenn Sie die Include-Methode verwenden.</span><span class="sxs-lookup"><span data-stu-id="1c74d-125">However, when running against a database the data will only be loaded if you use the Include method.</span></span>  

<span data-ttu-id="1c74d-126">Aus diesem Grund wird empfohlen, immer gewisse End-to-End-Tests (zusätzlich zu den Komponententests), um sicherzustellen, dass Ihre Anwendung funktioniert korrekt für eine Datenbank enthalten.</span><span class="sxs-lookup"><span data-stu-id="1c74d-126">For this reason, it is recommended to always include some level of end-to-end testing (in addition to your unit tests) to ensure your application works correctly against a database.</span></span>  

## <a name="following-along-with-this-article"></a><span data-ttu-id="1c74d-127">Verbindung mit diesem Artikel befolgen</span><span class="sxs-lookup"><span data-stu-id="1c74d-127">Following along with this article</span></span>  

<span data-ttu-id="1c74d-128">Dieser Artikel bietet vollständige codeauflistungen, die Sie, in Visual Studio kopieren können nachvollziehen können, wenn Sie möchten.</span><span class="sxs-lookup"><span data-stu-id="1c74d-128">This article gives complete code listings that you can copy into Visual Studio to follow along if you wish.</span></span> <span data-ttu-id="1c74d-129">Am einfachsten erstellen Sie eine **Komponententestprojekt** , und Sie müssen Ziel **.NET Framework 4.5** in den Abschnitten ausführen, die asynchrone verwenden.</span><span class="sxs-lookup"><span data-stu-id="1c74d-129">It's easiest to create a **Unit Test Project** and you will need to target **.NET Framework 4.5** to complete the sections that use async.</span></span>  

## <a name="the-ef-model"></a><span data-ttu-id="1c74d-130">Das EF-Modell</span><span class="sxs-lookup"><span data-stu-id="1c74d-130">The EF model</span></span>  

<span data-ttu-id="1c74d-131">Der Dienst, die wir testen wollen nutzt ein EF Modell setzt sich aus der BloggingContext und den Blog und Post-Klassen.</span><span class="sxs-lookup"><span data-stu-id="1c74d-131">The service we're going to test makes use of an EF model made up of the BloggingContext and the Blog and Post classes.</span></span> <span data-ttu-id="1c74d-132">Dieser Code wurde möglicherweise vom EF-Designer generiert oder ein Code First-Modell.</span><span class="sxs-lookup"><span data-stu-id="1c74d-132">This code may have been generated by the EF Designer or be a Code First model.</span></span>  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;

namespace TestingDemo
{
    public class BloggingContext : DbContext
    {
        public virtual DbSet<Blog> Blogs { get; set; }
        public virtual DbSet<Post> Posts { get; set; }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Name { get; set; }
        public string Url { get; set; }

        public virtual List<Post> Posts { get; set; }
    }

    public class Post
    {
        public int PostId { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public virtual Blog Blog { get; set; }
    }
}
```  

### <a name="virtual-dbset-properties-with-ef-designer"></a><span data-ttu-id="1c74d-133">Eigenschaften des virtuellen "DbSet" mit dem EF Designer</span><span class="sxs-lookup"><span data-stu-id="1c74d-133">Virtual DbSet properties with EF Designer</span></span>  

<span data-ttu-id="1c74d-134">Beachten Sie, dass die Eigenschaften "DbSet" für den Kontext als virtuell markiert sind.</span><span class="sxs-lookup"><span data-stu-id="1c74d-134">Note that the DbSet properties on the context are marked as virtual.</span></span> <span data-ttu-id="1c74d-135">Dadurch wird die pseudoframework, das von unserem Kontext und überschreiben diese Eigenschaften durch eine simulierte Implementierung abgeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="1c74d-135">This will allow the mocking framework to derive from our context and overriding these properties with a mocked implementation.</span></span>  

<span data-ttu-id="1c74d-136">Wenn Sie Code First verwenden können Sie Ihre Klassen direkt bearbeiten.</span><span class="sxs-lookup"><span data-stu-id="1c74d-136">If you are using Code First then you can edit your classes directly.</span></span> <span data-ttu-id="1c74d-137">Wenn Sie dem EF Designer verwenden, müssen Sie die T4-Vorlage zu bearbeiten, die den Kontext generiert.</span><span class="sxs-lookup"><span data-stu-id="1c74d-137">If you are using the EF Designer then you’ll need to edit the T4 template that generates your context.</span></span> <span data-ttu-id="1c74d-138">Öffnen Sie die \<Model_name\>. Context.tt-Datei, die unter der Sie Edmx-Datei, geschachtelt ist finden das folgende Fragment des Codes, und fügen in der virtual-Schlüsselwort, wie dargestellt.</span><span class="sxs-lookup"><span data-stu-id="1c74d-138">Open up the \<model_name\>.Context.tt file that is nested under you edmx file, find the following fragment of code and add in the virtual keyword as shown.</span></span>  

``` csharp
public string DbSet(EntitySet entitySet)
{
    return string.Format(
        CultureInfo.InvariantCulture,
        "{0} virtual DbSet\<{1}> {2} {{ get; set; }}",
        Accessibility.ForReadOnlyProperty(entitySet),
        _typeMapper.GetTypeName(entitySet.ElementType),
        _code.Escape(entitySet));
}
```  

## <a name="service-to-be-tested"></a><span data-ttu-id="1c74d-139">Dienst getestet werden</span><span class="sxs-lookup"><span data-stu-id="1c74d-139">Service to be tested</span></span>  

<span data-ttu-id="1c74d-140">Um zu veranschaulichen, mit in-Memory-Test-Doubles testen werden wir eine Reihe von Tests für eine BlogService schreiben.</span><span class="sxs-lookup"><span data-stu-id="1c74d-140">To demonstrate testing with in-memory test doubles we are going to be writing a couple of tests for a BlogService.</span></span> <span data-ttu-id="1c74d-141">Der Dienst ist, erstellen neue Blogs (AddBlog) kann ein, und anhand des Namens (GetAllBlogs) sortiert alle Blogs zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="1c74d-141">The service is capable of creating new blogs (AddBlog) and returning all Blogs ordered by name (GetAllBlogs).</span></span> <span data-ttu-id="1c74d-142">Neben GetAllBlogs haben wir auch eine Methode bereitgestellt, die asynchron alle Blogs, sortiert nach Name (GetAllBlogsAsync) erhält.</span><span class="sxs-lookup"><span data-stu-id="1c74d-142">In addition to GetAllBlogs, we’ve also provided a method that will asynchronously get all blogs ordered by name (GetAllBlogsAsync).</span></span>  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;
using System.Linq;
using System.Threading.Tasks;

namespace TestingDemo
{
    public class BlogService
    {
        private BloggingContext _context;

        public BlogService(BloggingContext context)
        {
            _context = context;
        }

        public Blog AddBlog(string name, string url)
        {
            var blog = _context.Blogs.Add(new Blog { Name = name, Url = url });
            _context.SaveChanges();

            return blog;
        }

        public List<Blog> GetAllBlogs()
        {
            var query = from b in _context.Blogs
                        orderby b.Name
                        select b;

            return query.ToList();
        }

        public async Task<List<Blog>> GetAllBlogsAsync()
        {
            var query = from b in _context.Blogs
                        orderby b.Name
                        select b;

            return await query.ToListAsync();
        }
    }
}
```  

## <a name="testing-non-query-scenarios"></a><span data-ttu-id="1c74d-143">Testen von nicht-Query-Szenarien</span><span class="sxs-lookup"><span data-stu-id="1c74d-143">Testing non-query scenarios</span></span>  

<span data-ttu-id="1c74d-144">Das ist alles, was, die wir tun, um die Tests nicht Abfragemethoden starten müssen.</span><span class="sxs-lookup"><span data-stu-id="1c74d-144">That’s all we need to do to start testing non-query methods.</span></span> <span data-ttu-id="1c74d-145">Der folgende Test verwendet Moq, um einen Kontext erstellen.</span><span class="sxs-lookup"><span data-stu-id="1c74d-145">The following test uses Moq to create a context.</span></span> <span data-ttu-id="1c74d-146">Er erstellt dann einen "DbSet"\<Blog\> und verbindet es aus der Kontexteigenschaft Blogs zurückgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="1c74d-146">It then creates a DbSet\<Blog\> and wires it up to be returned from the context’s Blogs property.</span></span> <span data-ttu-id="1c74d-147">Als Nächstes wird der Kontext verwendet, eine neue BlogService zu erstellen, die dann zum Erstellen eines neuen Blogs – mithilfe der AddBlog-Methode verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="1c74d-147">Next, the context is used to create a new BlogService which is then used to create a new blog – using the AddBlog method.</span></span> <span data-ttu-id="1c74d-148">Abschließend vergewissert sich, dass der Dienst einen neuen Blog hinzugefügt und dem Namen "SaveChanges" im Kontext des Tests.</span><span class="sxs-lookup"><span data-stu-id="1c74d-148">Finally, the test verifies that the service added a new Blog and called SaveChanges on the context.</span></span>  

``` csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using Moq;
using System.Data.Entity;

namespace TestingDemo
{
    [TestClass]
    public class NonQueryTests
    {
        [TestMethod]
        public void CreateBlog_saves_a_blog_via_context()
        {
            var mockSet = new Mock<DbSet<Blog>>();

            var mockContext = new Mock<BloggingContext>();
            mockContext.Setup(m => m.Blogs).Returns(mockSet.Object);

            var service = new BlogService(mockContext.Object);
            service.AddBlog("ADO.NET Blog", "http://blogs.msdn.com/adonet");

            mockSet.Verify(m => m.Add(It.IsAny<Blog>()), Times.Once());
            mockContext.Verify(m => m.SaveChanges(), Times.Once());
        }
    }
}
```  

## <a name="testing-query-scenarios"></a><span data-ttu-id="1c74d-149">Testen von Abfrageszenarien</span><span class="sxs-lookup"><span data-stu-id="1c74d-149">Testing query scenarios</span></span>  

<span data-ttu-id="1c74d-150">Um Abfragen für unser "DbSet" Testdouble ausführen zu können, müssen wir eine Implementierung von "IQueryable" einrichten.</span><span class="sxs-lookup"><span data-stu-id="1c74d-150">In order to be able to execute queries against our DbSet test double we need to setup an implementation of IQueryable.</span></span> <span data-ttu-id="1c74d-151">Der erste Schritt ist, um einige Daten im Arbeitsspeicher zu erstellen – wir verwenden eine Liste\<Blog\>.</span><span class="sxs-lookup"><span data-stu-id="1c74d-151">The first step is to create some in-memory data – we’re using a List\<Blog\>.</span></span> <span data-ttu-id="1c74d-152">Als Nächstes erstellen wir einen Kontext und den "DbSet"\<Blog\> verknüpfen Sie anschließend die IQueryable-Implementierung für "DbSet" – sie sind nur delegieren, an den Anbieter LINQ to Objects, die mit der Liste funktioniert\<T\>.</span><span class="sxs-lookup"><span data-stu-id="1c74d-152">Next, we create a context and DBSet\<Blog\> then wire up the IQueryable implementation for the DbSet – they’re just delegating to the LINQ to Objects provider that works with List\<T\>.</span></span>  

<span data-ttu-id="1c74d-153">Wir können eine BlogService basierend auf unseren Test-Doubles erstellen und stellen Sie sicher, dass die Daten, die wir aus GetAllBlogs erhalten nach Namen sortiert werden.</span><span class="sxs-lookup"><span data-stu-id="1c74d-153">We can then create a BlogService based on our test doubles and ensure that the data we get back from GetAllBlogs is ordered by name.</span></span>  

``` csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using Moq;
using System.Collections.Generic;
using System.Data.Entity;
using System.Linq;

namespace TestingDemo
{
    [TestClass]
    public class QueryTests
    {
        [TestMethod]
        public void GetAllBlogs_orders_by_name()
        {
            var data = new List<Blog>
            {
                new Blog { Name = "BBB" },
                new Blog { Name = "ZZZ" },
                new Blog { Name = "AAA" },
            }.AsQueryable();

            var mockSet = new Mock<DbSet<Blog>>();
            mockSet.As<IQueryable<Blog>>().Setup(m => m.Provider).Returns(data.Provider);
            mockSet.As<IQueryable<Blog>>().Setup(m => m.Expression).Returns(data.Expression);
            mockSet.As<IQueryable<Blog>>().Setup(m => m.ElementType).Returns(data.ElementType);
            mockSet.As<IQueryable<Blog>>().Setup(m => m.GetEnumerator()).Returns(0 => data.GetEnumerator());

            var mockContext = new Mock<BloggingContext>();
            mockContext.Setup(c => c.Blogs).Returns(mockSet.Object);

            var service = new BlogService(mockContext.Object);
            var blogs = service.GetAllBlogs();

            Assert.AreEqual(3, blogs.Count);
            Assert.AreEqual("AAA", blogs[0].Name);
            Assert.AreEqual("BBB", blogs[1].Name);
            Assert.AreEqual("ZZZ", blogs[2].Name);
        }
    }
}
```  

### <a name="testing-with-async-queries"></a><span data-ttu-id="1c74d-154">Testen mit Async-Abfragen</span><span class="sxs-lookup"><span data-stu-id="1c74d-154">Testing with async queries</span></span>

<span data-ttu-id="1c74d-155">Entitätsframework 6 eingeführt, einen Satz von Erweiterungsmethoden, die verwendet werden kann, um eine Abfrage asynchron ausgeführt werden soll.</span><span class="sxs-lookup"><span data-stu-id="1c74d-155">Entity Framework 6 introduced a set of extension methods that can be used to asynchronously execute a query.</span></span> <span data-ttu-id="1c74d-156">Beispiele für diese Methoden werden "tolistasync", "firstasync", ForEachAsync usw. umfassen.</span><span class="sxs-lookup"><span data-stu-id="1c74d-156">Examples of these methods include ToListAsync, FirstAsync, ForEachAsync, etc.</span></span>  

<span data-ttu-id="1c74d-157">Da Entity Framework-Abfragen von LINQ zu verwenden vornehmen, werden die Erweiterungsmethoden für "IQueryable" und "IEnumerable" definiert.</span><span class="sxs-lookup"><span data-stu-id="1c74d-157">Because Entity Framework queries make use of LINQ, the extension methods are defined on IQueryable and IEnumerable.</span></span> <span data-ttu-id="1c74d-158">Jedoch, da sie nur so ausgelegt sind, die mit Entity Framework verwendet werden den folgenden Fehler erhalten Sie, wenn Sie versuchen, diese in einer LINQ-Abfrage verwenden, die nicht von Entity Framework-Abfrage:</span><span class="sxs-lookup"><span data-stu-id="1c74d-158">However, because they are only designed to be used with Entity Framework you may receive the following error if you try to use them on a LINQ query that isn’t an Entity Framework query:</span></span>

> <span data-ttu-id="1c74d-159">Die Quelle "IQueryable" implementiert keine IDbAsyncEnumerable{0}.</span><span class="sxs-lookup"><span data-stu-id="1c74d-159">The source IQueryable doesn't implement IDbAsyncEnumerable{0}.</span></span> <span data-ttu-id="1c74d-160">Nur Quellen, die IDbAsyncEnumerable implementieren können für asynchrone Vorgänge von Entity Framework verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="1c74d-160">Only sources that implement IDbAsyncEnumerable can be used for Entity Framework asynchronous operations.</span></span> <span data-ttu-id="1c74d-161">Weitere Informationen finden Sie [ http://go.microsoft.com/fwlink/?LinkId=287068 ](http://go.microsoft.com/fwlink/?LinkId=287068).</span><span class="sxs-lookup"><span data-stu-id="1c74d-161">For more details see [http://go.microsoft.com/fwlink/?LinkId=287068](http://go.microsoft.com/fwlink/?LinkId=287068).</span></span>  

<span data-ttu-id="1c74d-162">Während die asynchronen Methoden nur bei der Ausführung für ein EF-Abfragen unterstützt werden, sollten Sie beim Ausführen mit einer in-Memory testen, Double, der ein "DbSet" im Komponententest verwenden.</span><span class="sxs-lookup"><span data-stu-id="1c74d-162">Whilst the async methods are only supported when running against an EF query, you may want to use them in your unit test when running against an in-memory test double of a DbSet.</span></span>  

<span data-ttu-id="1c74d-163">Um die Async-Methoden verwenden, müssen wir eine in-Memory-DbAsyncQueryProvider zum Verarbeiten der Async-Abfrage zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="1c74d-163">In order to use the async methods we need to create an in-memory DbAsyncQueryProvider to process the async query.</span></span> <span data-ttu-id="1c74d-164">Dabei wird der Anbieter für eine Abfrage mit Moq einrichten werden würde, ist es viel einfacher, eine testimplementierung double in Code zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="1c74d-164">Whilst it would be possible to setup a query provider using Moq, it is much easier to create a test double implementation in code.</span></span> <span data-ttu-id="1c74d-165">Der Code für diese Implementierung ist wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="1c74d-165">The code for this implementation is as follows:</span></span>  

``` csharp
using System.Collections.Generic;
using System.Data.Entity.Infrastructure;
using System.Linq;
using System.Linq.Expressions;
using System.Threading;
using System.Threading.Tasks;

namespace TestingDemo
{
    internal class TestDbAsyncQueryProvider<TEntity> : IDbAsyncQueryProvider
    {
        private readonly IQueryProvider _inner;

        internal TestDbAsyncQueryProvider(IQueryProvider inner)
        {
            _inner = inner;
        }

        public IQueryable CreateQuery(Expression expression)
        {
            return new TestDbAsyncEnumerable<TEntity>(expression);
        }

        public IQueryable<TElement> CreateQuery<TElement>(Expression expression)
        {
            return new TestDbAsyncEnumerable<TElement>(expression);
        }

        public object Execute(Expression expression)
        {
            return _inner.Execute(expression);
        }

        public TResult Execute<TResult>(Expression expression)
        {
            return _inner.Execute<TResult>(expression);
        }

        public Task<object> ExecuteAsync(Expression expression, CancellationToken cancellationToken)
        {
            return Task.FromResult(Execute(expression));
        }

        public Task<TResult> ExecuteAsync<TResult>(Expression expression, CancellationToken cancellationToken)
        {
            return Task.FromResult(Execute<TResult>(expression));
        }
    }

    internal class TestDbAsyncEnumerable<T> : EnumerableQuery<T>, IDbAsyncEnumerable<T>, IQueryable<T>
    {
        public TestDbAsyncEnumerable(IEnumerable<T> enumerable)
            : base(enumerable)
        { }

        public TestDbAsyncEnumerable(Expression expression)
            : base(expression)
        { }

        public IDbAsyncEnumerator<T> GetAsyncEnumerator()
        {
            return new TestDbAsyncEnumerator<T>(this.AsEnumerable().GetEnumerator());
        }

        IDbAsyncEnumerator IDbAsyncEnumerable.GetAsyncEnumerator()
        {
            return GetAsyncEnumerator();
        }

        IQueryProvider IQueryable.Provider
        {
            get { return new TestDbAsyncQueryProvider<T>(this); }
        }
    }

    internal class TestDbAsyncEnumerator<T> : IDbAsyncEnumerator<T>
    {
        private readonly IEnumerator<T> _inner;

        public TestDbAsyncEnumerator(IEnumerator<T> inner)
        {
            _inner = inner;
        }

        public void Dispose()
        {
            _inner.Dispose();
        }

        public Task<bool> MoveNextAsync(CancellationToken cancellationToken)
        {
            return Task.FromResult(_inner.MoveNext());
        }

        public T Current
        {
            get { return _inner.Current; }
        }

        object IDbAsyncEnumerator.Current
        {
            get { return Current; }
        }
    }
}
```  

<span data-ttu-id="1c74d-166">Nun, da wir eine Async-Abfrageanbieter haben können, Schreiben wir einen Komponententest für unsere neue GetAllBlogsAsync-Methode.</span><span class="sxs-lookup"><span data-stu-id="1c74d-166">Now that we have an async query provider we can write a unit test for our new GetAllBlogsAsync method.</span></span>  

``` csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using Moq;
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Linq;
using System.Threading.Tasks;

namespace TestingDemo
{
    [TestClass]
    public class AsyncQueryTests
    {
        [TestMethod]
        public async Task GetAllBlogsAsync_orders_by_name()
        {

            var data = new List<Blog>
            {
                new Blog { Name = "BBB" },
                new Blog { Name = "ZZZ" },
                new Blog { Name = "AAA" },
            }.AsQueryable();

            var mockSet = new Mock<DbSet<Blog>>();
            mockSet.As<IDbAsyncEnumerable<Blog>>()
                .Setup(m => m.GetAsyncEnumerator())
                .Returns(new TestDbAsyncEnumerator<Blog>(data.GetEnumerator()));

            mockSet.As<IQueryable<Blog>>()
                .Setup(m => m.Provider)
                .Returns(new TestDbAsyncQueryProvider<Blog>(data.Provider));

            mockSet.As<IQueryable<Blog>>().Setup(m => m.Expression).Returns(data.Expression);
            mockSet.As<IQueryable<Blog>>().Setup(m => m.ElementType).Returns(data.ElementType);
            mockSet.As<IQueryable<Blog>>().Setup(m => m.GetEnumerator()).Returns(data.GetEnumerator());

            var mockContext = new Mock<BloggingContext>();
            mockContext.Setup(c => c.Blogs).Returns(mockSet.Object);

            var service = new BlogService(mockContext.Object);
            var blogs = await service.GetAllBlogsAsync();

            Assert.AreEqual(3, blogs.Count);
            Assert.AreEqual("AAA", blogs[0].Name);
            Assert.AreEqual("BBB", blogs[1].Name);
            Assert.AreEqual("ZZZ", blogs[2].Name);
        }
    }
}
```  
