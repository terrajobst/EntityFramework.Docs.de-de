---
title: Testen mit der ein Mockframework - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: bd66a638-d245-44d4-8e71-b9c6cb335cc7
caps.latest.revision: 3
ms.openlocfilehash: 7529929a3ed3906e1201c0899f2fb8b959ec9ed2
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2018
ms.locfileid: "39121342"
---
# <a name="testing-with-a-mocking-framework"></a>Testen mit der ein Mockframework
> [!NOTE]
> **Nur EF6 und höher:** Die Features, APIs usw., die auf dieser Seite erläutert werden, wurden in Entity Framework 6 eingeführt. Wenn Sie eine frühere Version verwenden, gelten manche Informationen nicht.  

Beim Schreiben von Tests für Ihre Anwendung ist es oft wünschenswert, überschreiten die Datenbank zu vermeiden.  Entitätsframework ermöglicht Ihnen, dies zu erreichen, indem ein Kontext erstellt werden – von den Tests – definierten Verhalten, dass die Daten im Arbeitsspeicher verwendet.  

## <a name="options-for-creating-test-doubles"></a>Optionen zum Erstellen von Testdoubles  

Es gibt zwei Ansätze, die verwendet werden können, um eine in-Memory-Version Ihres Kontexts erstellen.  

- **Erstellen Sie eigene Testdoubles** – bei diesem Ansatz muss Ihre eigene in-Memory-Implementierung, der den Kontext und die "dbsets" schreiben. Dies bietet Ihnen viele steuern, wie die Klassen Verhalten sich jedoch betreffen können, schreiben und eine angemessene Menge an Code besitzt.  
- **Verwenden Sie zum Erstellen von Testdoubles ein Mockframework** – Verwendung eines Simulationsframeworks (wie etwa Moq) können Sie die in-Memory-Implementierungen von Ihnen Kontext und legt fest, die dynamisch zur Laufzeit für Sie erstellt haben.  

In diesem Artikel wird mit der Verwendung eines Simulationsframeworks bearbeiten. Erstellen eigene Testdoubles finden Sie unter [Tests mit Ihrer eigenen Test-Doubles](writing-test-doubles.md).  

Veranschaulicht die Verwendung von EF mit ein Mockframework sind wir Moq verwenden. Die einfachste Möglichkeit zum Abrufen von Moq ist zum Installieren der [Moq-Paket von NuGet](http://nuget.org/packages/Moq/).  

## <a name="testing-with-pre-ef6-versions"></a>Vor EF6 Versionen testen  

In diesem Artikel dargestellte ist abhängig von der wir "DbSet" in EF6 vorgenommenen Änderungen. Testen mit EF5 und frühere Versionen finden Sie unter [Testen mit einem Fake-Kontext](http://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).  

## <a name="limitations-of-ef-in-memory-test-doubles"></a>Einschränkungen von EF-in-Memory-Test-doubles  

In-Memory-Test-Doubles möglich, dass eine gute Möglichkeit zum Bereitstellen von Komponententests für Servicelevel-Abdeckung der Bits der Anwendung, die EF verwenden. Jedoch dabei verwenden Sie LINQ to Objects, um Abfragen für Daten im Arbeitsspeicher auszuführen. Dadurch können zu unterschiedlichem Verhalten als die Verwendung von EF LINQ-Anbieter (LINQ to Entities), um Abfragen in SQL zu übersetzen, die für Ihre Datenbank ausgeführt wird.  

Ein Beispiel für einen solchen Unterschied lädt verwandte Daten. Wenn Sie eine Reihe von Blogs erstellen, die jeweils haben Beiträgen und dann bei Verwendung von in-Memory-Daten Beiträge werden immer geladen werden für jeden Blog. Allerdings werden bei der Ausführung für eine Datenbank die Daten nur geladen werden, wenn Sie die Include-Methode verwenden.  

Aus diesem Grund wird empfohlen, immer gewisse End-to-End-Tests (zusätzlich zu den Komponententests), um sicherzustellen, dass Ihre Anwendung funktioniert korrekt für eine Datenbank enthalten.  

## <a name="following-along-with-this-article"></a>Verbindung mit diesem Artikel befolgen  

Dieser Artikel bietet vollständige codeauflistungen, die Sie, in Visual Studio kopieren können nachvollziehen können, wenn Sie möchten. Am einfachsten erstellen Sie eine **Komponententestprojekt** , und Sie müssen Ziel **.NET Framework 4.5** in den Abschnitten ausführen, die asynchrone verwenden.  

## <a name="the-ef-model"></a>Das EF-Modell  

Der Dienst, die wir testen wollen nutzt ein EF Modell setzt sich aus der BloggingContext und den Blog und Post-Klassen. Dieser Code wurde möglicherweise vom EF-Designer generiert oder ein Code First-Modell.  

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

### <a name="virtual-dbset-properties-with-ef-designer"></a>Eigenschaften des virtuellen "DbSet" mit dem EF Designer  

Beachten Sie, dass die Eigenschaften "DbSet" für den Kontext als virtuell markiert sind. Dadurch wird die pseudoframework, das von unserem Kontext und überschreiben diese Eigenschaften durch eine simulierte Implementierung abgeleitet werden.  

Wenn Sie Code First verwenden können Sie Ihre Klassen direkt bearbeiten. Wenn Sie dem EF Designer verwenden, müssen Sie die T4-Vorlage zu bearbeiten, die den Kontext generiert. Öffnen Sie die \<Model_name\>. Context.tt-Datei, die unter der Sie Edmx-Datei, geschachtelt ist finden das folgende Fragment des Codes, und fügen in der virtual-Schlüsselwort, wie dargestellt.  

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

## <a name="testing-non-query-scenarios"></a>Testen von nicht-Query-Szenarien  

Das ist alles, was, die wir tun, um die Tests nicht Abfragemethoden starten müssen. Der folgende Test verwendet Moq, um einen Kontext erstellen. Er erstellt dann einen "DbSet"\<Blog\> und verbindet es aus der Kontexteigenschaft Blogs zurückgegeben werden. Als Nächstes wird der Kontext verwendet, eine neue BlogService zu erstellen, die dann zum Erstellen eines neuen Blogs – mithilfe der AddBlog-Methode verwendet wird. Abschließend vergewissert sich, dass der Dienst einen neuen Blog hinzugefügt und dem Namen "SaveChanges" im Kontext des Tests.  

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

## <a name="testing-query-scenarios"></a>Testen von Abfrageszenarien  

Um Abfragen für unser "DbSet" Testdouble ausführen zu können, müssen wir eine Implementierung von "IQueryable" einrichten. Der erste Schritt ist, um einige Daten im Arbeitsspeicher zu erstellen – wir verwenden eine Liste\<Blog\>. Als Nächstes erstellen wir einen Kontext und den "DbSet"\<Blog\> verknüpfen Sie anschließend die IQueryable-Implementierung für "DbSet" – sie sind nur delegieren, an den Anbieter LINQ to Objects, die mit der Liste funktioniert\<T\>.  

Wir können eine BlogService basierend auf unseren Test-Doubles erstellen und stellen Sie sicher, dass die Daten, die wir aus GetAllBlogs erhalten nach Namen sortiert werden.  

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

### <a name="testing-with-async-queries"></a>Testen mit Async-Abfragen

Entitätsframework 6 eingeführt, einen Satz von Erweiterungsmethoden, die verwendet werden kann, um eine Abfrage asynchron ausgeführt werden soll. Beispiele für diese Methoden werden "tolistasync", "firstasync", ForEachAsync usw. umfassen.  

Da Entity Framework-Abfragen von LINQ zu verwenden vornehmen, werden die Erweiterungsmethoden für "IQueryable" und "IEnumerable" definiert. Jedoch, da sie nur so ausgelegt sind, die mit Entity Framework verwendet werden den folgenden Fehler erhalten Sie, wenn Sie versuchen, diese in einer LINQ-Abfrage verwenden, die nicht von Entity Framework-Abfrage:

> Die Quelle "IQueryable" implementiert keine IDbAsyncEnumerable{0}. Nur Quellen, die IDbAsyncEnumerable implementieren können für asynchrone Vorgänge von Entity Framework verwendet werden. Weitere Informationen finden Sie [ http://go.microsoft.com/fwlink/?LinkId=287068 ](http://go.microsoft.com/fwlink/?LinkId=287068).  

Während die asynchronen Methoden nur bei der Ausführung für ein EF-Abfragen unterstützt werden, sollten Sie beim Ausführen mit einer in-Memory testen, Double, der ein "DbSet" im Komponententest verwenden.  

Um die Async-Methoden verwenden, müssen wir eine in-Memory-DbAsyncQueryProvider zum Verarbeiten der Async-Abfrage zu erstellen. Dabei wird der Anbieter für eine Abfrage mit Moq einrichten werden würde, ist es viel einfacher, eine testimplementierung double in Code zu erstellen. Der Code für diese Implementierung ist wie folgt aus:  

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

Nun, da wir eine Async-Abfrageanbieter haben können, Schreiben wir einen Komponententest für unsere neue GetAllBlogsAsync-Methode.  

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
