---
title: Tests mit Ihren eigenen Testdoubles - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 16a8b7c0-2d23-47f4-9cc0-e2eb2e738ca3
ms.openlocfilehash: fa63c483e0a55b6cbd43382f68ab9592f3c1768b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997216"
---
# <a name="testing-with-your-own-test-doubles"></a><span data-ttu-id="eb06a-102">Tests mit Ihren eigenen Testdoubles</span><span class="sxs-lookup"><span data-stu-id="eb06a-102">Testing with your own test doubles</span></span>
> [!NOTE]
> <span data-ttu-id="eb06a-103">**Nur EF6 und höher:** Die Features, APIs usw., die auf dieser Seite erläutert werden, wurden in Entity Framework 6 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="eb06a-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="eb06a-104">Wenn Sie eine frühere Version verwenden, gelten manche Informationen nicht.</span><span class="sxs-lookup"><span data-stu-id="eb06a-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="eb06a-105">Beim Schreiben von Tests für Ihre Anwendung ist es oft wünschenswert, überschreiten die Datenbank zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="eb06a-105">When writing tests for your application it is often desirable to avoid hitting the database.</span></span>  <span data-ttu-id="eb06a-106">Entitätsframework ermöglicht Ihnen, dies zu erreichen, indem ein Kontext erstellt werden – von den Tests – definierten Verhalten, dass die Daten im Arbeitsspeicher verwendet.</span><span class="sxs-lookup"><span data-stu-id="eb06a-106">Entity Framework allows you to achieve this by creating a context – with behavior defined by your tests – that makes use of in-memory data.</span></span>  

## <a name="options-for-creating-test-doubles"></a><span data-ttu-id="eb06a-107">Optionen zum Erstellen von Testdoubles</span><span class="sxs-lookup"><span data-stu-id="eb06a-107">Options for creating test doubles</span></span>  

<span data-ttu-id="eb06a-108">Es gibt zwei Ansätze, die verwendet werden können, um eine in-Memory-Version Ihres Kontexts erstellen.</span><span class="sxs-lookup"><span data-stu-id="eb06a-108">There are two different approaches that can be used to create an in-memory version of your context.</span></span>  

- <span data-ttu-id="eb06a-109">**Erstellen Sie eigene Testdoubles** – bei diesem Ansatz muss Ihre eigene in-Memory-Implementierung, der den Kontext und die "dbsets" schreiben.</span><span class="sxs-lookup"><span data-stu-id="eb06a-109">**Create your own test doubles** – This approach involves writing your own in-memory implementation of your context and DbSets.</span></span> <span data-ttu-id="eb06a-110">Dies bietet Ihnen viele steuern, wie die Klassen Verhalten sich jedoch betreffen können, schreiben und eine angemessene Menge an Code besitzt.</span><span class="sxs-lookup"><span data-stu-id="eb06a-110">This gives you a lot of control over how the classes behave but can involve writing and owning a reasonable amount of code.</span></span>  
- <span data-ttu-id="eb06a-111">**Verwenden Sie zum Erstellen von Testdoubles ein Mockframework** – Verwendung eines Simulationsframeworks (wie etwa Moq) können Sie die in-Memory-Implementierungen von Ihnen Kontext und legt fest, die dynamisch zur Laufzeit für Sie erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="eb06a-111">**Use a mocking framework to create test doubles** – Using a mocking framework (such as Moq) you can have the in-memory implementations of you context and sets created dynamically at runtime for you.</span></span>  

<span data-ttu-id="eb06a-112">Erstellen eigene Tests double wird in diesem Artikel behandeln.</span><span class="sxs-lookup"><span data-stu-id="eb06a-112">This article will deal with creating your own test double.</span></span> <span data-ttu-id="eb06a-113">Weitere Informationen zur Verwendung eines Simulationsframeworks finden Sie unter [Testen mit einem Mocking-Framework](mocking.md).</span><span class="sxs-lookup"><span data-stu-id="eb06a-113">For information on using a mocking framework see [Testing with a Mocking Framework](mocking.md).</span></span>  

## <a name="testing-with-pre-ef6-versions"></a><span data-ttu-id="eb06a-114">Vor EF6 Versionen testen</span><span class="sxs-lookup"><span data-stu-id="eb06a-114">Testing with pre-EF6 versions</span></span>  

<span data-ttu-id="eb06a-115">Der Code in diesem Artikel ist kompatibel mit EF6.</span><span class="sxs-lookup"><span data-stu-id="eb06a-115">The code shown in this article is compatible with EF6.</span></span> <span data-ttu-id="eb06a-116">Testen mit EF5 und frühere Versionen finden Sie unter [Testen mit einem Fake-Kontext](http://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).</span><span class="sxs-lookup"><span data-stu-id="eb06a-116">For testing with EF5 and earlier version see [Testing with a Fake Context](http://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).</span></span>  

## <a name="limitations-of-ef-in-memory-test-doubles"></a><span data-ttu-id="eb06a-117">Einschränkungen von EF-in-Memory-Test-doubles</span><span class="sxs-lookup"><span data-stu-id="eb06a-117">Limitations of EF in-memory test doubles</span></span>  

<span data-ttu-id="eb06a-118">In-Memory-Test-Doubles möglich, dass eine gute Möglichkeit zum Bereitstellen von Komponententests für Servicelevel-Abdeckung der Bits der Anwendung, die EF verwenden.</span><span class="sxs-lookup"><span data-stu-id="eb06a-118">In-memory test doubles can be a good way to provide unit test level coverage of bits of your application that use EF.</span></span> <span data-ttu-id="eb06a-119">Jedoch dabei verwenden Sie LINQ to Objects, um Abfragen für Daten im Arbeitsspeicher auszuführen.</span><span class="sxs-lookup"><span data-stu-id="eb06a-119">However, when doing this you are using LINQ to Objects to execute queries against in-memory data.</span></span> <span data-ttu-id="eb06a-120">Dadurch können zu unterschiedlichem Verhalten als die Verwendung von EF LINQ-Anbieter (LINQ to Entities), um Abfragen in SQL zu übersetzen, die für Ihre Datenbank ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="eb06a-120">This can result in different behavior than using EF’s LINQ provider (LINQ to Entities) to translate queries into SQL that is run against your database.</span></span>  

<span data-ttu-id="eb06a-121">Ein Beispiel für einen solchen Unterschied lädt verwandte Daten.</span><span class="sxs-lookup"><span data-stu-id="eb06a-121">One example of such a difference is loading related data.</span></span> <span data-ttu-id="eb06a-122">Wenn Sie eine Reihe von Blogs erstellen, die jeweils haben Beiträgen und dann bei Verwendung von in-Memory-Daten Beiträge werden immer geladen werden für jeden Blog.</span><span class="sxs-lookup"><span data-stu-id="eb06a-122">If you create a series of Blogs that each have related Posts, then when using in-memory data the related Posts will always be loaded for each Blog.</span></span> <span data-ttu-id="eb06a-123">Allerdings werden bei der Ausführung für eine Datenbank die Daten nur geladen werden, wenn Sie die Include-Methode verwenden.</span><span class="sxs-lookup"><span data-stu-id="eb06a-123">However, when running against a database the data will only be loaded if you use the Include method.</span></span>  

<span data-ttu-id="eb06a-124">Aus diesem Grund wird empfohlen, immer gewisse End-to-End-Tests (zusätzlich zu den Komponententests), um sicherzustellen, dass Ihre Anwendung funktioniert korrekt für eine Datenbank enthalten.</span><span class="sxs-lookup"><span data-stu-id="eb06a-124">For this reason, it is recommended to always include some level of end-to-end testing (in addition to your unit tests) to ensure your application works correctly against a database.</span></span>  

## <a name="following-along-with-this-article"></a><span data-ttu-id="eb06a-125">Verbindung mit diesem Artikel befolgen</span><span class="sxs-lookup"><span data-stu-id="eb06a-125">Following along with this article</span></span>  

<span data-ttu-id="eb06a-126">Dieser Artikel bietet vollständige codeauflistungen, die Sie, in Visual Studio kopieren können nachvollziehen können, wenn Sie möchten.</span><span class="sxs-lookup"><span data-stu-id="eb06a-126">This article gives complete code listings that you can copy into Visual Studio to follow along if you wish.</span></span> <span data-ttu-id="eb06a-127">Am einfachsten erstellen Sie eine **Komponententestprojekt** , und Sie müssen Ziel **.NET Framework 4.5** in den Abschnitten ausführen, die asynchrone verwenden.</span><span class="sxs-lookup"><span data-stu-id="eb06a-127">It's easiest to create a **Unit Test Project** and you will need to target **.NET Framework 4.5** to complete the sections that use async.</span></span>  

## <a name="creating-a-context-interface"></a><span data-ttu-id="eb06a-128">Erstellen eine Kontextschnittstelle</span><span class="sxs-lookup"><span data-stu-id="eb06a-128">Creating a context interface</span></span>  

<span data-ttu-id="eb06a-129">Wir werden sehen Sie sich das Testen eines Diensts, die von einem EF nutzt Modell.</span><span class="sxs-lookup"><span data-stu-id="eb06a-129">We're going to look at testing a service that makes use of an EF model.</span></span> <span data-ttu-id="eb06a-130">Um unser EF-Kontext mit einer in-Memory-Version zum Testen ersetzen können, definieren wir eine Schnittstelle, dass unser EF-Kontext (und deren in-Memory-Double-Wert) Imeplement werden.</span><span class="sxs-lookup"><span data-stu-id="eb06a-130">In order to be able to replace our EF context with an in-memory version for testing, we'll define an interface that our EF context (and it's in-memory double) will imeplement.</span></span>  

<span data-ttu-id="eb06a-131">Der Dienst zum Testen der hier verwendeten Abfragen und Ändern von Daten, die mit den Eigenschaften "DbSet" von unserem Kontext und auch aufrufen, "SaveChanges", um die Änderungen in der Datenbank zu übertragen.</span><span class="sxs-lookup"><span data-stu-id="eb06a-131">The service we are going to test will query and modify data using the DbSet properties of our context and also call SaveChanges to push changes to the database.</span></span> <span data-ttu-id="eb06a-132">Wir haben also diese Member der Schnittstelle einschließlich.</span><span class="sxs-lookup"><span data-stu-id="eb06a-132">So we're including these members on the interface.</span></span>  

``` csharp
using System.Data.Entity;

namespace TestingDemo
{
    public interface IBloggingContext
    {
        DbSet<Blog> Blogs { get; }
        DbSet<Post> Posts { get; }
        int SaveChanges();
    }
}
```  

## <a name="the-ef-model"></a><span data-ttu-id="eb06a-133">Das EF-Modell</span><span class="sxs-lookup"><span data-stu-id="eb06a-133">The EF model</span></span>  

<span data-ttu-id="eb06a-134">Der Dienst, die wir testen wollen nutzt ein EF Modell setzt sich aus der BloggingContext und den Blog und Post-Klassen.</span><span class="sxs-lookup"><span data-stu-id="eb06a-134">The service we're going to test makes use of an EF model made up of the BloggingContext and the Blog and Post classes.</span></span> <span data-ttu-id="eb06a-135">Dieser Code wurde möglicherweise vom EF-Designer generiert oder ein Code First-Modell.</span><span class="sxs-lookup"><span data-stu-id="eb06a-135">This code may have been generated by the EF Designer or be a Code First model.</span></span>  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;

namespace TestingDemo
{
    public class BloggingContext : DbContext, IBloggingContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }
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

### <a name="implementing-the-context-interface-with-the-ef-designer"></a><span data-ttu-id="eb06a-136">Implementieren der Kontextschnittstelle, mit dem EF Designer</span><span class="sxs-lookup"><span data-stu-id="eb06a-136">Implementing the context interface with the EF Designer</span></span>  

<span data-ttu-id="eb06a-137">Beachten Sie, dass es sich bei unserem Kontext die IBloggingContext-Schnittstelle implementiert.</span><span class="sxs-lookup"><span data-stu-id="eb06a-137">Note that our context implements the IBloggingContext interface.</span></span>  

<span data-ttu-id="eb06a-138">Wenn Sie Code First verwenden, können Sie den Kontext direkt zum Implementieren der Schnittstelle bearbeiten.</span><span class="sxs-lookup"><span data-stu-id="eb06a-138">If you are using Code First then you can edit your context directly to implement the interface.</span></span> <span data-ttu-id="eb06a-139">Wenn Sie dem EF Designer verwenden, müssen Sie die T4-Vorlage zu bearbeiten, die den Kontext generiert.</span><span class="sxs-lookup"><span data-stu-id="eb06a-139">If you are using the EF Designer then you’ll need to edit the T4 template that generates your context.</span></span> <span data-ttu-id="eb06a-140">Öffnen Sie die \<Model_name\>. Context.tt-Datei, die unter der Sie Edmx-Datei, geschachtelt ist finden das folgende Fragment des Codes, und fügen in der Schnittstelle, wie gezeigt.</span><span class="sxs-lookup"><span data-stu-id="eb06a-140">Open up the \<model_name\>.Context.tt file that is nested under you edmx file, find the following fragment of code and add in the interface as shown.</span></span>  

``` csharp  
<#=Accessibility.ForType(container)#> partial class <#=code.Escape(container)#> : DbContext, IBloggingContext
```  

## <a name="service-to-be-tested"></a><span data-ttu-id="eb06a-141">Dienst getestet werden</span><span class="sxs-lookup"><span data-stu-id="eb06a-141">Service to be tested</span></span>  

<span data-ttu-id="eb06a-142">Um zu veranschaulichen, mit in-Memory-Test-Doubles testen werden wir eine Reihe von Tests für eine BlogService schreiben.</span><span class="sxs-lookup"><span data-stu-id="eb06a-142">To demonstrate testing with in-memory test doubles we are going to be writing a couple of tests for a BlogService.</span></span> <span data-ttu-id="eb06a-143">Der Dienst ist, erstellen neue Blogs (AddBlog) kann ein, und anhand des Namens (GetAllBlogs) sortiert alle Blogs zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="eb06a-143">The service is capable of creating new blogs (AddBlog) and returning all Blogs ordered by name (GetAllBlogs).</span></span> <span data-ttu-id="eb06a-144">Neben GetAllBlogs haben wir auch eine Methode bereitgestellt, die asynchron alle Blogs, sortiert nach Name (GetAllBlogsAsync) erhält.</span><span class="sxs-lookup"><span data-stu-id="eb06a-144">In addition to GetAllBlogs, we’ve also provided a method that will asynchronously get all blogs ordered by name (GetAllBlogsAsync).</span></span>  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;
using System.Linq;
using System.Threading.Tasks;

namespace TestingDemo
{
    public class BlogService
    {
        private IBloggingContext _context;

        public BlogService(IBloggingContext context)
        {
            _context = context;
        }

        public Blog AddBlog(string name, string url)
        {
            var blog = new Blog { Name = name, Url = url };
            _context.Blogs.Add(blog);
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

<span data-ttu-id="eb06a-145"><a name="creating-the-in-memory-test-doubles"/> ## Verdoppelt erstellen die in-Memory-Tests</span><span class="sxs-lookup"><span data-stu-id="eb06a-145"><a name="creating-the-in-memory-test-doubles"/> ## Creating the in-memory test doubles</span></span>  

<span data-ttu-id="eb06a-146">Nun, wir haben das echte EF-Modell und dem Dienst, der sie verwenden können, ist es Zeit, des in-Memory-Tests double zu erstellen, dass es für Tests verwenden können.</span><span class="sxs-lookup"><span data-stu-id="eb06a-146">Now that we have the real EF model and the service that can use it, it's time to create the in-memory test double that we can use for testing.</span></span> <span data-ttu-id="eb06a-147">Wir haben einen TestContext-Test für unsere Kontext doppelte erstellt.</span><span class="sxs-lookup"><span data-stu-id="eb06a-147">We've created a TestContext test double for our context.</span></span> <span data-ttu-id="eb06a-148">Test-Doubles, die wir erhalten, um das Verhalten auswählen, um die Tests unterstützen sollten, werden wir ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="eb06a-148">In test doubles we get to choose the behavior we want in order to support the tests we are going to run.</span></span> <span data-ttu-id="eb06a-149">In diesem Beispiel haben wir nur erfasst die Anzahl der Häufigkeit, mit die SaveChanges aufgerufen wird, aber Sie können aufnehmen, automatisierungsszenario zugeschnittene Logik erforderlich ist, um das Szenario zu überprüfen, das Sie testen.</span><span class="sxs-lookup"><span data-stu-id="eb06a-149">In this example we're just capturing the number of times SaveChanges is called, but you can include whatever logic is needed to verify the scenario you are testing.</span></span>  

<span data-ttu-id="eb06a-150">Wir haben auch eine TestDbSet erstellt, die eine in-Memory-Implementierung von "DbSet" bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="eb06a-150">We've also created a TestDbSet that provides an in-memory implementation of DbSet.</span></span> <span data-ttu-id="eb06a-151">Wir haben eine vollständige gesammelte für alle Methoden bereitgestellt, auf "DbSet" (mit Ausnahme von Suchen), jedoch müssen Sie nur die Member zu implementieren, die Ihr Testszenario verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="eb06a-151">We've provided a complete implemention for all the methods on DbSet (except for Find), but you only need to implement the members that your test scenario will use.</span></span>  

<span data-ttu-id="eb06a-152">TestDbSet nutzt einige andere Infrastruktur-Klassen, die wir eingefügt haben, um sicherzustellen, dass die Async-Abfragen verarbeitet werden können.</span><span class="sxs-lookup"><span data-stu-id="eb06a-152">TestDbSet makes use of some other infrastructure classes that we've included to ensure that async queries can be processed.</span></span>  

``` csharp
using System;
using System.Collections.Generic;
using System.Collections.ObjectModel;
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Linq;
using System.Linq.Expressions;
using System.Threading;
using System.Threading.Tasks;

namespace TestingDemo
{
    public class TestContext : IBloggingContext
    {
        public TestContext()
        {
            this.Blogs = new TestDbSet<Blog>();
            this.Posts = new TestDbSet<Post>();
        }

        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }
        public int SaveChangesCount { get; private set; }
        public int SaveChanges()
        {
            this.SaveChangesCount++;
            return 1;
        }
    }

    public class TestDbSet<TEntity> : DbSet<TEntity>, IQueryable, IEnumerable<TEntity>, IDbAsyncEnumerable<TEntity>
        where TEntity : class
    {
        ObservableCollection<TEntity> _data;
        IQueryable _query;

        public TestDbSet()
        {
            _data = new ObservableCollection<TEntity>();
            _query = _data.AsQueryable();
        }

        public override TEntity Add(TEntity item)
        {
            _data.Add(item);
            return item;
        }

        public override TEntity Remove(TEntity item)
        {
            _data.Remove(item);
            return item;
        }

        public override TEntity Attach(TEntity item)
        {
            _data.Add(item);
            return item;
        }

        public override TEntity Create()
        {
            return Activator.CreateInstance<TEntity>();
        }

        public override TDerivedEntity Create<TDerivedEntity>()
        {
            return Activator.CreateInstance<TDerivedEntity>();
        }

        public override ObservableCollection<TEntity> Local
        {
            get { return _data; }
        }

        Type IQueryable.ElementType
        {
            get { return _query.ElementType; }
        }

        Expression IQueryable.Expression
        {
            get { return _query.Expression; }
        }

        IQueryProvider IQueryable.Provider
        {
            get { return new TestDbAsyncQueryProvider<TEntity>(_query.Provider); }
        }

        System.Collections.IEnumerator System.Collections.IEnumerable.GetEnumerator()
        {
            return _data.GetEnumerator();
        }

        IEnumerator<TEntity> IEnumerable<TEntity>.GetEnumerator()
        {
            return _data.GetEnumerator();
        }

        IDbAsyncEnumerator<TEntity> IDbAsyncEnumerable<TEntity>.GetAsyncEnumerator()
        {
            return new TestDbAsyncEnumerator<TEntity>(_data.GetEnumerator());
        }
    }

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

### <a name="implementing-find"></a><span data-ttu-id="eb06a-153">Implementieren von Suchen</span><span class="sxs-lookup"><span data-stu-id="eb06a-153">Implementing Find</span></span>  

<span data-ttu-id="eb06a-154">Die Find-Methode ist schwierig, die auf generische Weise implementieren.</span><span class="sxs-lookup"><span data-stu-id="eb06a-154">The Find method is difficult to implement in a generic fashion.</span></span> <span data-ttu-id="eb06a-155">Wenn Sie testen möchten den Code, mit der, verwenden, der Find-Methode, die am einfachsten erstellen Sie einen Test aus, suchen "DbSet" für jeden der Entitätstypen, die unterstützt werden müssen.</span><span class="sxs-lookup"><span data-stu-id="eb06a-155">If you need to test code that makes use of the Find method it is easiest to create a test DbSet for each of the entity types that need to support find.</span></span> <span data-ttu-id="eb06a-156">Anschließend können Sie die Logik zum Suchen dieses bestimmten Typs Entität schreiben, wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="eb06a-156">You can then write logic to find that particular type of entity, as shown below.</span></span>  

``` csharp
using System.Linq;

namespace TestingDemo
{
    class TestBlogDbSet : TestDbSet<Blog>
    {
        public override Blog Find(params object[] keyValues)
        {
            var id = (int)keyValues.Single();
            return this.SingleOrDefault(b => b.BlogId == id);
        }
    }
}
```  

## <a name="writing-some-tests"></a><span data-ttu-id="eb06a-157">Schreiben einiger tests</span><span class="sxs-lookup"><span data-stu-id="eb06a-157">Writing some tests</span></span>  

<span data-ttu-id="eb06a-158">Das ist alles, was, die wir tun, um die Tests starten müssen.</span><span class="sxs-lookup"><span data-stu-id="eb06a-158">That’s all we need to do to start testing.</span></span> <span data-ttu-id="eb06a-159">Der folgende Test erstellt einen TestContext und dann ein Dienst für diesen Kontext basiert.</span><span class="sxs-lookup"><span data-stu-id="eb06a-159">The following test creates a TestContext and then a service based on this context.</span></span> <span data-ttu-id="eb06a-160">Der Dienst wird dann verwendet, um einen neuen Blog – mithilfe der AddBlog-Methode zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="eb06a-160">The service is then used to create a new blog – using the AddBlog method.</span></span> <span data-ttu-id="eb06a-161">Abschließend vergewissert sich, dass der Dienst einen neuen Blog in der Kontexteigenschaft Blogs hinzugefügt und "SaveChanges" benannt im Kontext des Tests.</span><span class="sxs-lookup"><span data-stu-id="eb06a-161">Finally, the test verifies that the service added a new Blog to the context's Blogs property and called SaveChanges on the context.</span></span>  

<span data-ttu-id="eb06a-162">Dies ist nur ein Beispiel der Typen von Dinge, die Sie mit einer in-Memory-Testdouble testen können, und Sie können die Logik des Test-Doubles und die Überprüfung nach Ihren Anforderungen anpassen.</span><span class="sxs-lookup"><span data-stu-id="eb06a-162">This is just an example of the types of things you can test with an in-memory test double and you can adjust the logic of the test doubles and the verification to meet your requirements.</span></span>  

``` csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using System.Linq;

namespace TestingDemo
{
    [TestClass]
    public class NonQueryTests
    {
        [TestMethod]
        public void CreateBlog_saves_a_blog_via_context()
        {
            var context = new TestContext();

            var service = new BlogService(context);
            service.AddBlog("ADO.NET Blog", "http://blogs.msdn.com/adonet");

            Assert.AreEqual(1, context.Blogs.Count());
            Assert.AreEqual("ADO.NET Blog", context.Blogs.Single().Name);
            Assert.AreEqual("http://blogs.msdn.com/adonet", context.Blogs.Single().Url);
            Assert.AreEqual(1, context.SaveChangesCount);
        }
    }
}
```  

<span data-ttu-id="eb06a-163">Hier ist ein weiteres Beispiel für einen Test - diesmal eine, die eine Abfrage ausführt.</span><span class="sxs-lookup"><span data-stu-id="eb06a-163">Here is another example of a test - this time one that performs a query.</span></span> <span data-ttu-id="eb06a-164">Der Test wird durch das Erstellen einer Testkontext mit einigen Daten in der Blog-Eigenschaft – Beachten Sie, dass die Daten nicht in alphabetischer Reihenfolge.</span><span class="sxs-lookup"><span data-stu-id="eb06a-164">The test starts by creating a test context with some data in its Blog property - note that the data is not in alphabetical order.</span></span> <span data-ttu-id="eb06a-165">Wir können eine BlogService basierend auf unseren Testkontext erstellen und stellen Sie sicher, dass die Daten, die wir aus GetAllBlogs erhalten nach Namen sortiert werden.</span><span class="sxs-lookup"><span data-stu-id="eb06a-165">We can then create a BlogService based on our test context and ensure that the data we get back from GetAllBlogs is ordered by name.</span></span>  

``` csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;

namespace TestingDemo
{
    [TestClass]
    public class QueryTests
    {
        [TestMethod]
        public void GetAllBlogs_orders_by_name()
        {
            var context = new TestContext();
            context.Blogs.Add(new Blog { Name = "BBB" });
            context.Blogs.Add(new Blog { Name = "ZZZ" });
            context.Blogs.Add(new Blog { Name = "AAA" });

            var service = new BlogService(context);
            var blogs = service.GetAllBlogs();

            Assert.AreEqual(3, blogs.Count);
            Assert.AreEqual("AAA", blogs[0].Name);
            Assert.AreEqual("BBB", blogs[1].Name);
            Assert.AreEqual("ZZZ", blogs[2].Name);
        }
    }
}
```  

<span data-ttu-id="eb06a-166">Schließlich schreiben wir eine weitere Tests, die die Async-Methode verwendet, um sicherzustellen, dass wir in enthalten die Async-Infrastruktur [TestDbSet](#creating-the-in-memory-test-doubles) arbeitet.</span><span class="sxs-lookup"><span data-stu-id="eb06a-166">Finally, we'll write one more test that uses our async method to ensure that the async infrastructure we included in [TestDbSet](#creating-the-in-memory-test-doubles) is working.</span></span>  

``` csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using System.Collections.Generic;
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
            var context = new TestContext();
            context.Blogs.Add(new Blog { Name = "BBB" });
            context.Blogs.Add(new Blog { Name = "ZZZ" });
            context.Blogs.Add(new Blog { Name = "AAA" });

            var service = new BlogService(context);
            var blogs = await service.GetAllBlogsAsync();

            Assert.AreEqual(3, blogs.Count);
            Assert.AreEqual("AAA", blogs[0].Name);
            Assert.AreEqual("BBB", blogs[1].Name);
            Assert.AreEqual("ZZZ", blogs[2].Name);
        }
    }
}
```  
