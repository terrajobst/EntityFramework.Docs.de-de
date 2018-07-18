---
title: Tests mit Ihren eigenen Testdoubles - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 16a8b7c0-2d23-47f4-9cc0-e2eb2e738ca3
caps.latest.revision: 4
ms.openlocfilehash: 4e2511f92f9bb034ab468dd030ef238e325ce7c0
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2018
ms.locfileid: "39121336"
---
# <a name="testing-with-your-own-test-doubles"></a>Tests mit Ihren eigenen Testdoubles
> [!NOTE]
> **Nur EF6 und höher:** Die Features, APIs usw., die auf dieser Seite erläutert werden, wurden in Entity Framework 6 eingeführt. Wenn Sie eine frühere Version verwenden, gelten manche Informationen nicht.  

Beim Schreiben von Tests für Ihre Anwendung ist es oft wünschenswert, überschreiten die Datenbank zu vermeiden.  Entitätsframework ermöglicht Ihnen, dies zu erreichen, indem ein Kontext erstellt werden – von den Tests – definierten Verhalten, dass die Daten im Arbeitsspeicher verwendet.  

## <a name="options-for-creating-test-doubles"></a>Optionen zum Erstellen von Testdoubles  

Es gibt zwei Ansätze, die verwendet werden können, um eine in-Memory-Version Ihres Kontexts erstellen.  

- **Erstellen Sie eigene Testdoubles** – bei diesem Ansatz muss Ihre eigene in-Memory-Implementierung, der den Kontext und die "dbsets" schreiben. Dies bietet Ihnen viele steuern, wie die Klassen Verhalten sich jedoch betreffen können, schreiben und eine angemessene Menge an Code besitzt.  
- **Verwenden Sie zum Erstellen von Testdoubles ein Mockframework** – Verwendung eines Simulationsframeworks (wie etwa Moq) können Sie die in-Memory-Implementierungen von Ihnen Kontext und legt fest, die dynamisch zur Laufzeit für Sie erstellt haben.  

Erstellen eigene Tests double wird in diesem Artikel behandeln. Weitere Informationen zur Verwendung eines Simulationsframeworks finden Sie unter [Testen mit einem Mocking-Framework](mocking.md).  

## <a name="testing-with-pre-ef6-versions"></a>Vor EF6 Versionen testen  

Der Code in diesem Artikel ist kompatibel mit EF6. Testen mit EF5 und frühere Versionen finden Sie unter [Testen mit einem Fake-Kontext](http://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).  

## <a name="limitations-of-ef-in-memory-test-doubles"></a>Einschränkungen von EF-in-Memory-Test-doubles  

In-Memory-Test-Doubles möglich, dass eine gute Möglichkeit zum Bereitstellen von Komponententests für Servicelevel-Abdeckung der Bits der Anwendung, die EF verwenden. Jedoch dabei verwenden Sie LINQ to Objects, um Abfragen für Daten im Arbeitsspeicher auszuführen. Dadurch können zu unterschiedlichem Verhalten als die Verwendung von EF LINQ-Anbieter (LINQ to Entities), um Abfragen in SQL zu übersetzen, die für Ihre Datenbank ausgeführt wird.  

Ein Beispiel für einen solchen Unterschied lädt verwandte Daten. Wenn Sie eine Reihe von Blogs erstellen, die jeweils haben Beiträgen und dann bei Verwendung von in-Memory-Daten Beiträge werden immer geladen werden für jeden Blog. Allerdings werden bei der Ausführung für eine Datenbank die Daten nur geladen werden, wenn Sie die Include-Methode verwenden.  

Aus diesem Grund wird empfohlen, immer gewisse End-to-End-Tests (zusätzlich zu den Komponententests), um sicherzustellen, dass Ihre Anwendung funktioniert korrekt für eine Datenbank enthalten.  

## <a name="following-along-with-this-article"></a>Verbindung mit diesem Artikel befolgen  

Dieser Artikel bietet vollständige codeauflistungen, die Sie, in Visual Studio kopieren können nachvollziehen können, wenn Sie möchten. Am einfachsten erstellen Sie eine **Komponententestprojekt** , und Sie müssen Ziel **.NET Framework 4.5** in den Abschnitten ausführen, die asynchrone verwenden.  

## <a name="creating-a-context-interface"></a>Erstellen eine Kontextschnittstelle  

Wir werden sehen Sie sich das Testen eines Diensts, die von einem EF nutzt Modell. Um unser EF-Kontext mit einer in-Memory-Version zum Testen ersetzen können, definieren wir eine Schnittstelle, dass unser EF-Kontext (und deren in-Memory-Double-Wert) Imeplement werden.  

Der Dienst zum Testen der hier verwendeten Abfragen und Ändern von Daten, die mit den Eigenschaften "DbSet" von unserem Kontext und auch aufrufen, "SaveChanges", um die Änderungen in der Datenbank zu übertragen. Wir haben also diese Member der Schnittstelle einschließlich.  

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

## <a name="the-ef-model"></a>Das EF-Modell  

Der Dienst, die wir testen wollen nutzt ein EF Modell setzt sich aus der BloggingContext und den Blog und Post-Klassen. Dieser Code wurde möglicherweise vom EF-Designer generiert oder ein Code First-Modell.  

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

### <a name="implementing-the-context-interface-with-the-ef-designer"></a>Implementieren der Kontextschnittstelle, mit dem EF Designer  

Beachten Sie, dass es sich bei unserem Kontext die IBloggingContext-Schnittstelle implementiert.  

Wenn Sie Code First verwenden, können Sie den Kontext direkt zum Implementieren der Schnittstelle bearbeiten. Wenn Sie dem EF Designer verwenden, müssen Sie die T4-Vorlage zu bearbeiten, die den Kontext generiert. Öffnen Sie die \<Model_name\>. Context.tt-Datei, die unter der Sie Edmx-Datei, geschachtelt ist finden das folgende Fragment des Codes, und fügen in der Schnittstelle, wie gezeigt.  

``` csharp  
<#=Accessibility.ForType(container)#> partial class <#=code.Escape(container)#> : DbContext, IBloggingContext
```  

## <a name="service-to-be-tested"></a>Dienst getestet werden  

Um zu veranschaulichen, mit in-Memory-Test-Doubles testen werden wir eine Reihe von Tests für eine BlogService schreiben. Der Dienst ist, erstellen neue Blogs (AddBlog) kann ein, und anhand des Namens (GetAllBlogs) sortiert alle Blogs zurückgegeben. Neben GetAllBlogs haben wir auch eine Methode bereitgestellt, die asynchron alle Blogs, sortiert nach Name (GetAllBlogsAsync) erhält.  

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

<a name="creating-the-in-memory-test-doubles"/> ## Verdoppelt erstellen die in-Memory-Tests  

Nun, wir haben das echte EF-Modell und dem Dienst, der sie verwenden können, ist es Zeit, des in-Memory-Tests double zu erstellen, dass es für Tests verwenden können. Wir haben einen TestContext-Test für unsere Kontext doppelte erstellt. Test-Doubles, die wir erhalten, um das Verhalten auswählen, um die Tests unterstützen sollten, werden wir ausgeführt. In diesem Beispiel haben wir nur erfasst die Anzahl der Häufigkeit, mit die SaveChanges aufgerufen wird, aber Sie können aufnehmen, automatisierungsszenario zugeschnittene Logik erforderlich ist, um das Szenario zu überprüfen, das Sie testen.  

Wir haben auch eine TestDbSet erstellt, die eine in-Memory-Implementierung von "DbSet" bereitstellt. Wir haben eine vollständige gesammelte für alle Methoden bereitgestellt, auf "DbSet" (mit Ausnahme von Suchen), jedoch müssen Sie nur die Member zu implementieren, die Ihr Testszenario verwendet werden.  

TestDbSet nutzt einige andere Infrastruktur-Klassen, die wir eingefügt haben, um sicherzustellen, dass die Async-Abfragen verarbeitet werden können.  

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

### <a name="implementing-find"></a>Implementieren von Suchen  

Die Find-Methode ist schwierig, die auf generische Weise implementieren. Wenn Sie testen möchten den Code, mit der, verwenden, der Find-Methode, die am einfachsten erstellen Sie einen Test aus, suchen "DbSet" für jeden der Entitätstypen, die unterstützt werden müssen. Anschließend können Sie die Logik zum Suchen dieses bestimmten Typs Entität schreiben, wie unten dargestellt.  

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

## <a name="writing-some-tests"></a>Schreiben einiger tests  

Das ist alles, was, die wir tun, um die Tests starten müssen. Der folgende Test erstellt einen TestContext und dann ein Dienst für diesen Kontext basiert. Der Dienst wird dann verwendet, um einen neuen Blog – mithilfe der AddBlog-Methode zu erstellen. Abschließend vergewissert sich, dass der Dienst einen neuen Blog in der Kontexteigenschaft Blogs hinzugefügt und "SaveChanges" benannt im Kontext des Tests.  

Dies ist nur ein Beispiel der Typen von Dinge, die Sie mit einer in-Memory-Testdouble testen können, und Sie können die Logik des Test-Doubles und die Überprüfung nach Ihren Anforderungen anpassen.  

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

Hier ist ein weiteres Beispiel für einen Test - diesmal eine, die eine Abfrage ausführt. Der Test wird durch das Erstellen einer Testkontext mit einigen Daten in der Blog-Eigenschaft – Beachten Sie, dass die Daten nicht in alphabetischer Reihenfolge. Wir können eine BlogService basierend auf unseren Testkontext erstellen und stellen Sie sicher, dass die Daten, die wir aus GetAllBlogs erhalten nach Namen sortiert werden.  

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

Schließlich schreiben wir eine weitere Tests, die die Async-Methode verwendet, um sicherzustellen, dass wir in enthalten die Async-Infrastruktur [TestDbSet](#creating-the-in-memory-test-doubles) arbeitet.  

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
