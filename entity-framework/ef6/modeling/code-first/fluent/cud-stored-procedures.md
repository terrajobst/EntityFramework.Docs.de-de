---
title: Code zuerst einfügen, aktualisieren und Löschen von gespeicherten Prozeduren – EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 9a7ae7f9-4072-4843-877d-506dd7eef576
ms.openlocfilehash: a0448fb44dabb2e03b2358aa7b4f69d92cffa15a
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994572"
---
# <a name="code-first-insert-update-and-delete-stored-procedures"></a><span data-ttu-id="b5a95-102">Code zuerst einfügen, aktualisieren und Löschen von gespeicherten Prozeduren</span><span class="sxs-lookup"><span data-stu-id="b5a95-102">Code First Insert, Update, and Delete Stored Procedures</span></span>
> [!NOTE]
> <span data-ttu-id="b5a95-103">**Nur EF6 und höher:** Die Features, APIs usw., die auf dieser Seite erläutert werden, wurden in Entity Framework 6 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="b5a95-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="b5a95-104">Wenn Sie eine frühere Version verwenden, gelten manche Informationen nicht.</span><span class="sxs-lookup"><span data-stu-id="b5a95-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="b5a95-105">Standardmäßig wird Code First konfiguriert alle Entitäten für Insert ausführen, aktualisieren und löschen die Befehle mithilfe der direkten Tabellenzugriff.</span><span class="sxs-lookup"><span data-stu-id="b5a95-105">By default, Code First will configure all entities to perform insert, update and delete commands using direct table access.</span></span> <span data-ttu-id="b5a95-106">Ab EF6 Sie können Ihre Code First-Modell zur Verwendung gespeicherter Prozeduren für einige oder alle Entitäten in Ihrem Modell konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="b5a95-106">Starting in EF6 you can configure your Code First model to use stored procedures for some or all entities in your model.</span></span>  

## <a name="basic-entity-mapping"></a><span data-ttu-id="b5a95-107">Grundlegende Entitätszuordnung</span><span class="sxs-lookup"><span data-stu-id="b5a95-107">Basic Entity Mapping</span></span>  

<span data-ttu-id="b5a95-108">Sie können festlegen, verwenden von gespeicherten Prozeduren für Einfüge-, aktualisieren und löschen Sie mithilfe der Fluent-API.</span><span class="sxs-lookup"><span data-stu-id="b5a95-108">You can opt into using stored procedures for insert, update and delete using the Fluent API.</span></span>  

``` csharp
modelBuilder
  .Entity<Blog>()
  .MapToStoredProcedures();
```  

<span data-ttu-id="b5a95-109">Dies bewirkt Code First einige Konventionen zu verwenden, um die erwartete Form der gespeicherten Prozeduren in der Datenbank zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="b5a95-109">Doing this will cause Code First to use some conventions to build the expected shape of the stored procedures in the database.</span></span>  

- <span data-ttu-id="b5a95-110">Drei gespeicherte Prozeduren, die mit dem Namen  **\<Type_name\>e_infügen**,  **\<Type_name\>_Update** und  **\<Type_ Namen\>_temporäre** (z. B. Blog_Insert, Blog_Update und Blog_Delete).</span><span class="sxs-lookup"><span data-stu-id="b5a95-110">Three stored procedures named **\<type_name\>_Insert**, **\<type_name\>_Update** and **\<type_name\>_Delete** (for example, Blog_Insert, Blog_Update and Blog_Delete).</span></span>  
- <span data-ttu-id="b5a95-111">Parameternamen entsprechen den Eigenschaftennamen.</span><span class="sxs-lookup"><span data-stu-id="b5a95-111">Parameter names correspond to the property names.</span></span>  
  > [!NOTE]
  > <span data-ttu-id="b5a95-112">Wenn Sie zum Umbenennen der Spalte für eine bestimmte Eigenschaft HasColumnName() oder Column-Attribut verwenden, wird dieser Name für Parameter anstelle des Namens der Eigenschaft verwendet.</span><span class="sxs-lookup"><span data-stu-id="b5a95-112">If you use HasColumnName() or the Column attribute to rename the column for a given property then this name is used for parameters instead of the property name.</span></span>  
- <span data-ttu-id="b5a95-113">**Die Insert-Prozedur** weist einen Parameter für jede Eigenschaft, mit Ausnahme der als speichern generierter gekennzeichnet (Identität oder berechnet).</span><span class="sxs-lookup"><span data-stu-id="b5a95-113">**The insert stored procedure** will have a parameter for every property, except for those marked as store generated (identity or computed).</span></span> <span data-ttu-id="b5a95-114">Die gespeicherte Prozedur sollte es sich um ein Resultset mit einer Spalte für jede Eigenschaft speichern generierter zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="b5a95-114">The stored procedure should return a result set with a column for each store generated property.</span></span>  
- <span data-ttu-id="b5a95-115">**Das Update gespeicherte Prozedur** weist einen Parameter für jede Eigenschaft, außer denen mit einem generierten speichern-Muster von "Berechnet" gekennzeichnet.</span><span class="sxs-lookup"><span data-stu-id="b5a95-115">**The update stored procedure** will have a parameter for every property, except for those marked with a store generated pattern of 'Computed'.</span></span> <span data-ttu-id="b5a95-116">Einige parallelitätstoken einen Parameter für den ursprünglichen Wert erfordern, finden Sie unter den *Parallelitätstoken* Informationen weiter unten im Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="b5a95-116">Some concurrency tokens require a parameter for the original value, see the *Concurrency Tokens* section below for details.</span></span> <span data-ttu-id="b5a95-117">Die gespeicherte Prozedur sollte es sich um ein Resultset mit einer Spalte für jede berechnete Eigenschaft zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="b5a95-117">The stored procedure should return a result set with a column for each computed property.</span></span>  
- <span data-ttu-id="b5a95-118">**Der Löschvorgang gespeicherte Prozedur** müssen einen Parameter für den Schlüsselwert, der die Entität (oder mehrere Parameter, wenn die Entität einen zusammengesetzten Schlüssel verfügt).</span><span class="sxs-lookup"><span data-stu-id="b5a95-118">**The delete stored procedure** should have a parameter for the key value of the entity (or multiple parameters if the entity has a composite key).</span></span> <span data-ttu-id="b5a95-119">Darüber hinaus muss die Prozedur Delete auch verfügen über Parameter für alle Fremdschlüssel unabhängige Zuordnung für die Zieltabelle (Beziehungen, die keine entsprechende Fremdschlüsseleigenschaften, die in der Entität deklariert haben).</span><span class="sxs-lookup"><span data-stu-id="b5a95-119">Additionally, the delete procedure should also have parameters for any independent association foreign keys on the target table (relationships that do not have corresponding foreign key properties declared in the entity).</span></span> <span data-ttu-id="b5a95-120">Einige parallelitätstoken einen Parameter für den ursprünglichen Wert erfordern, finden Sie unter den *Parallelitätstoken* Informationen weiter unten im Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="b5a95-120">Some concurrency tokens require a parameter for the original value, see the *Concurrency Tokens* section below for details.</span></span>  

<span data-ttu-id="b5a95-121">Verwenden Sie die folgende Klasse als Beispiel an:</span><span class="sxs-lookup"><span data-stu-id="b5a95-121">Using the following class as an example:</span></span>  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  public string Url { get; set; }  
}
```  

<span data-ttu-id="b5a95-122">Die standardmäßige gespeicherte Prozeduren würde folgendermaßen lauten:</span><span class="sxs-lookup"><span data-stu-id="b5a95-122">The default stored procedures would be:</span></span>  

``` SQL
CREATE PROCEDURE [dbo].[Blog_Insert]  
  @Name nvarchar(max),  
  @Url nvarchar(max)  
AS  
BEGIN
  INSERT INTO [dbo].[Blogs] ([Name], [Url])
  VALUES (@Name, @Url)

  SELECT SCOPE_IDENTITY() AS BlogId
END
CREATE PROCEDURE [dbo].[Blog_Update]  
  @BlogId int,  
  @Name nvarchar(max),  
  @Url nvarchar(max)  
AS  
  UPDATE [dbo].[Blogs]
  SET [Name] = @Name, [Url] = @Url     
  WHERE BlogId = @BlogId;
CREATE PROCEDURE [dbo].[Blog_Delete]  
  @BlogId int  
AS  
  DELETE FROM [dbo].[Blogs]
  WHERE BlogId = @BlogId
```  

### <a name="overriding-the-defaults"></a><span data-ttu-id="b5a95-123">Die Standardwerte außer Kraft setzen</span><span class="sxs-lookup"><span data-stu-id="b5a95-123">Overriding the Defaults</span></span>  

<span data-ttu-id="b5a95-124">Sie können überschreiben, teilweise oder ganz wie standardmäßig konfiguriert wurde.</span><span class="sxs-lookup"><span data-stu-id="b5a95-124">You can override part or all of what was configured by default.</span></span>  

<span data-ttu-id="b5a95-125">Sie können den Namen des einen oder mehrere gespeicherte Prozeduren ändern.</span><span class="sxs-lookup"><span data-stu-id="b5a95-125">You can change the name of one or more stored procedures.</span></span> <span data-ttu-id="b5a95-126">In diesem Beispiel wird die gespeicherte Updateprozedur nur.</span><span class="sxs-lookup"><span data-stu-id="b5a95-126">This example renames the update stored procedure only.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog")));
```  

<span data-ttu-id="b5a95-127">In diesem Beispiel wird alle drei gespeicherte Prozeduren.</span><span class="sxs-lookup"><span data-stu-id="b5a95-127">This example renames all three stored procedures.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog"))  
     .Delete(d => d.HasName("delete_blog"))  
     .Insert(i => i.HasName("insert_blog")));
```  

<span data-ttu-id="b5a95-128">In diesen Beispielen die Aufrufe verkettet werden, aber Sie können auch die Syntax von Lambda-Block.</span><span class="sxs-lookup"><span data-stu-id="b5a95-128">In these examples the calls are chained together, but you can also use lambda block syntax.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    {  
      s.Update(u => u.HasName("modify_blog"));  
      s.Delete(d => d.HasName("delete_blog"));  
      s.Insert(i => i.HasName("insert_blog"));  
    });
```  

<span data-ttu-id="b5a95-129">In diesem Beispiel wird den Parameter für die BlogId-Eigenschaft für die gespeicherte Updateprozedur.</span><span class="sxs-lookup"><span data-stu-id="b5a95-129">This example renames the parameter for the BlogId property on the update stored procedure.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.BlogId, "blog_id")));
```  

<span data-ttu-id="b5a95-130">Diese Aufrufe sind verkettbare und zusammensetzbar.</span><span class="sxs-lookup"><span data-stu-id="b5a95-130">These calls are all chainable and composable.</span></span> <span data-ttu-id="b5a95-131">Hier ist ein Beispiel, das alle drei gespeicherten Prozeduren und deren Parameter benennt.</span><span class="sxs-lookup"><span data-stu-id="b5a95-131">Here is an example that renames all three stored procedures and their parameters.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog")  
                   .Parameter(b => b.BlogId, "blog_id")  
                   .Parameter(b => b.Name, "blog_name")  
                   .Parameter(b => b.Url, "blog_url"))  
     .Delete(d => d.HasName("delete_blog")  
                   .Parameter(b => b.BlogId, "blog_id"))  
     .Insert(i => i.HasName("insert_blog")  
                   .Parameter(b => b.Name, "blog_name")  
                   .Parameter(b => b.Url, "blog_url")));
```  

<span data-ttu-id="b5a95-132">Sie können auch den Namen der Spalten im Resultset ändern, die generiert Datenbankwerte enthält.</span><span class="sxs-lookup"><span data-stu-id="b5a95-132">You can also change the name of the columns in the result set that contains database generated values.</span></span>  

``` csharp
modelBuilder
  .Entity<Blog>()
  .MapToStoredProcedures(s =>
    s.Insert(i => i.Result(b => b.BlogId, "generated_blog_identity")));
```

``` SQL
CREATE PROCEDURE [dbo].[Blog_Insert]  
  @Name nvarchar(max),  
  @Url nvarchar(max)  
AS  
BEGIN
  INSERT INTO [dbo].[Blogs] ([Name], [Url])
  VALUES (@Name, @Url)

  SELECT SCOPE_IDENTITY() AS generated_blog_id
END
```  

## <a name="relationships-without-a-foreign-key-in-the-class-independent-associations"></a><span data-ttu-id="b5a95-133">Beziehungen ohne ein Fremdschlüssel in der Klasse (unabhängigen Zuordnungen)</span><span class="sxs-lookup"><span data-stu-id="b5a95-133">Relationships Without a Foreign Key in the Class (Independent Associations)</span></span>  

<span data-ttu-id="b5a95-134">Wenn eine foreign Key-Eigenschaft in der Klassendefinition enthalten ist, kann der entsprechende Parameter in die gleiche Weise wie jede andere Eigenschaft umbenannt werden.</span><span class="sxs-lookup"><span data-stu-id="b5a95-134">When a foreign key property is included in the class definition, the corresponding parameter can be renamed in the same way as any other property.</span></span> <span data-ttu-id="b5a95-135">Wenn eine Beziehung, ohne eine Fremdschlüsseleigenschaft in der Klasse vorhanden ist, wird der Standardname für den Parameter  **\<Navigation_property_name\>_\<Primary_key_name\>**.</span><span class="sxs-lookup"><span data-stu-id="b5a95-135">When a relationship exists without a foreign key property in the class, the default parameter name is **\<navigation_property_name\>_\<primary_key_name\>**.</span></span>  

<span data-ttu-id="b5a95-136">Beispielsweise führt die folgenden Klassendefinitionen in einem Blog_BlogId-Parameter, die in den gespeicherten Prozeduren zum Einfügen und aktualisieren die Beiträge erwartet wird.</span><span class="sxs-lookup"><span data-stu-id="b5a95-136">For example, the following class definitions would result in a Blog_BlogId parameter being expected in the stored procedures to insert and update Posts.</span></span>  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  public string Url { get; set; }

  public List<Post> Posts { get; set; }  
}  

public class Post  
{  
  public int PostId { get; set; }  
  public string Title { get; set; }  
  public string Content { get; set; }  

  public Blog Blog { get; set; }  
}
```  

### <a name="overriding-the-defaults"></a><span data-ttu-id="b5a95-137">Die Standardwerte außer Kraft setzen</span><span class="sxs-lookup"><span data-stu-id="b5a95-137">Overriding the Defaults</span></span>  

<span data-ttu-id="b5a95-138">Sie können Parameter für die Fremdschlüssel ändern, die nicht in der Klasse enthalten sind, durch die Angabe des Pfads zur Eigenschaft primären Schlüssels für die Parameter-Methode.</span><span class="sxs-lookup"><span data-stu-id="b5a95-138">You can change parameters for foreign keys that are not included in the class by supplying the path to the primary key property to the Parameter method.</span></span>  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Parameter(p => p.Blog.BlogId, "blog_id")));
```  

<span data-ttu-id="b5a95-139">Wenn Sie eine Navigationseigenschaft für die abhängige Entität (d.h. haben</span><span class="sxs-lookup"><span data-stu-id="b5a95-139">If you don’t have a navigation property on the dependent entity (i.e</span></span> <span data-ttu-id="b5a95-140">keine Post.Blog-Eigenschaft) können Sie die Zuordnungsmethode verwenden, um das andere Ende der Beziehung zu identifizieren und konfigurieren Sie dann die Parameter, die jeweils die Schlüsseleigenschaften entsprechen.</span><span class="sxs-lookup"><span data-stu-id="b5a95-140">no Post.Blog property) then you can use the Association method to identify the other end of the relationship and then configure the parameters that correspond to each of the key property(s).</span></span>  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Navigation<Blog>(  
      b => b.Posts,  
      c => c.Parameter(b => b.BlogId, "blog_id"))));
```  

## <a name="concurrency-tokens"></a><span data-ttu-id="b5a95-141">Parallelitätstoken</span><span class="sxs-lookup"><span data-stu-id="b5a95-141">Concurrency Tokens</span></span>  

<span data-ttu-id="b5a95-142">Aktualisieren und löschen, die gespeicherten Prozeduren müssen möglicherweise auch für den Umgang mit Parallelität:</span><span class="sxs-lookup"><span data-stu-id="b5a95-142">Update and delete stored procedures may also need to deal with concurrency:</span></span>  

- <span data-ttu-id="b5a95-143">Wenn die Entität parallelitätstoken enthält, kann die gespeicherte Prozedur optional einen Output-Parameter aufweisen, der die Anzahl der Zeilen aktualisiert/gelöscht (Zeilen betroffen) zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="b5a95-143">If the entity contains concurrency tokens, the stored procedure can optionally have an output parameter that returns the number of rows updated/deleted (rows affected).</span></span> <span data-ttu-id="b5a95-144">Solcher Parameter muss mit der Methode RowsAffectedParameter konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="b5a95-144">Such a parameter must be configured using the RowsAffectedParameter method.</span></span>  
<span data-ttu-id="b5a95-145">EF verwendet standardmäßig den Rückgabewert von ExecuteNonQuery, um zu bestimmen, wie viele Zeilen betroffen sind.</span><span class="sxs-lookup"><span data-stu-id="b5a95-145">By default EF uses the return value from ExecuteNonQuery to determine how many rows were affected.</span></span> <span data-ttu-id="b5a95-146">Angeben ein Output-Parameter für betroffene Zeilen ist nützlich, wenn Sie keinerlei Logik in Ihrem Sproc ausführen, die den Rückgabewert der ExecuteNonQuery wird falsch (aus Sicht des EF) verursachen würde am Ende der Ausführung.</span><span class="sxs-lookup"><span data-stu-id="b5a95-146">Specifying a rows affected output parameter is useful if you perform any logic in your sproc that would result in the return value of ExecuteNonQuery being incorrect (from EF's perspective) at the end of execution.</span></span>  
- <span data-ttu-id="b5a95-147">Für die einzelnen Parallelität werden Token gibt es einen Parameter namens  **\<Property_name\>_Original** (z. B. Timestamp_Original).</span><span class="sxs-lookup"><span data-stu-id="b5a95-147">For each concurrency token there will be a parameter named **\<property_name\>_Original** (for example, Timestamp_Original ).</span></span> <span data-ttu-id="b5a95-148">Dadurch wird den ursprünglichen Wert dieser Eigenschaft: der Wert, wenn die Abfrage aus der Datenbank übergeben werden.</span><span class="sxs-lookup"><span data-stu-id="b5a95-148">This will be passed the original value of this property – the value when queried from the database.</span></span>  
    - <span data-ttu-id="b5a95-149">Parallelitätstoken, die von der Datenbank – z. B. Zeitstempel – berechnet werden, haben nur einen ursprünglichen-Parameter.</span><span class="sxs-lookup"><span data-stu-id="b5a95-149">Concurrency tokens that are computed by the database – such as timestamps – will only have an original value parameter.</span></span>  
    - <span data-ttu-id="b5a95-150">Nicht berechneten Eigenschaften, die als parallelitätstoken festgelegt werden müssen auch einen Parameter für den neuen Wert in der Updateprozedur.</span><span class="sxs-lookup"><span data-stu-id="b5a95-150">Non-computed properties that are set as concurrency tokens will also have a parameter for the new value in the update procedure.</span></span> <span data-ttu-id="b5a95-151">Hierbei werden die Benennungskonventionen, die bereits erläuterten für neue Werte verwendet.</span><span class="sxs-lookup"><span data-stu-id="b5a95-151">This uses the naming conventions already discussed for new values.</span></span> <span data-ttu-id="b5a95-152">Ein derartiges Token Beispiel würde die URL eines Blogs als ein parallelitätstoken verwenden, der neue Wert ist erforderlich, da dies kann durch Ihren Code (im Gegensatz zu einer Timestamp-Token die nur von der Datenbank aktualisiert wird) einen neuen Wert aktualisiert werden.</span><span class="sxs-lookup"><span data-stu-id="b5a95-152">An example of such a token would be using a Blog's URL as a concurrency token, the new value is required because this can be updated to a new value by your code (unlike a Timestamp token which is only updated by the database).</span></span>  

<span data-ttu-id="b5a95-153">Dies ist ein Beispiel-Klasse und die gespeicherte Prozedur mit einem Zeitstempel Concurrency Token zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="b5a95-153">This is an example class and update stored procedure with a timestamp concurrency token.</span></span>  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  public string Url { get; set; }  
  [Timestamp]
  public byte[] Timestamp { get; set; }
}
```

``` SQL
CREATE PROCEDURE [dbo].[Blog_Update]  
  @BlogId int,  
  @Name nvarchar(max),  
  @Url nvarchar(max),
  @Timestamp_Original rowversion  
AS  
  UPDATE [dbo].[Blogs]
  SET [Name] = @Name, [Url] = @Url     
  WHERE BlogId = @BlogId AND [Timestamp] = @Timestamp_Original
```  

<span data-ttu-id="b5a95-154">Hier ist ein Beispiel für Klasse, und aktualisieren Sie die gespeicherte Prozedur mit Token für nicht berechnete Parallelität.</span><span class="sxs-lookup"><span data-stu-id="b5a95-154">Here is an example class and update stored procedure with non-computed concurrency token.</span></span>  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  [ConcurrencyCheck]
  public string Url { get; set; }  
}
```

``` SQL
CREATE PROCEDURE [dbo].[Blog_Update]  
  @BlogId int,  
  @Name nvarchar(max),  
  @Url nvarchar(max),
  @Url_Original nvarchar(max),
AS  
  UPDATE [dbo].[Blogs]
  SET [Name] = @Name, [Url] = @Url     
  WHERE BlogId = @BlogId AND [Url] = @Url_Original
```  

### <a name="overriding-the-defaults"></a><span data-ttu-id="b5a95-155">Die Standardwerte außer Kraft setzen</span><span class="sxs-lookup"><span data-stu-id="b5a95-155">Overriding the Defaults</span></span>  

<span data-ttu-id="b5a95-156">Sie können optional einen Parameter für betroffene Zeilen einführen.</span><span class="sxs-lookup"><span data-stu-id="b5a95-156">You can optionally introduce a rows affected parameter.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.RowsAffectedParameter("rows_affected")));
```  

<span data-ttu-id="b5a95-157">Für die Datenbank berechnet parallelitätstoken –, bei dem nur der ursprüngliche Wert übergeben wird – können nur die Standardparameter umbenennen Mechanismus Sie so benennen Sie den Parameter für den ursprünglichen Wert um.</span><span class="sxs-lookup"><span data-stu-id="b5a95-157">For database computed concurrency tokens – where only the original value is passed – you can just use the standard parameter renaming mechanism to rename the parameter for the original value.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.Timestamp, "blog_timestamp")));
```  

<span data-ttu-id="b5a95-158">Für nicht berechnete parallelitätstoken –, bei dem sowohl die ursprünglichen und neuen Wert übergeben werden – können Sie eine Überladung des Parameters, die Ihnen ermöglicht, einen Namen für jeden Parameter angeben.</span><span class="sxs-lookup"><span data-stu-id="b5a95-158">For non-computed concurrency tokens – where both the original and new value are passed – you can use an overload of Parameter that allows you to supply a name for each parameter.</span></span>  

``` csharp
modelBuilder
 .Entity<Blog>()
 .MapToStoredProcedures(s => s.Update(u => u.Parameter(b => b.Url, "blog_url", "blog_original_url")));
```  

## <a name="many-to-many-relationships"></a><span data-ttu-id="b5a95-159">Viele-zu-viele-Beziehungen</span><span class="sxs-lookup"><span data-stu-id="b5a95-159">Many to Many Relationships</span></span>  

<span data-ttu-id="b5a95-160">In diesem Abschnitt als Beispiel verwenden wir die folgenden Klassen.</span><span class="sxs-lookup"><span data-stu-id="b5a95-160">We’ll use the following classes as an example in this section.</span></span>  

``` csharp
public class Post  
{  
  public int PostId { get; set; }  
  public string Title { get; set; }  
  public string Content { get; set; }  

  public List<Tag> Tags { get; set; }  
}  

public class Tag  
{  
  public int TagId { get; set; }  
  public string TagName { get; set; }  

  public List<Post> Posts { get; set; }  
}
```  

<span data-ttu-id="b5a95-161">Gespeicherte Prozeduren mit der folgenden Syntax können viele zu viele Beziehungen zugeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="b5a95-161">Many to many relationships can be mapped to stored procedures with the following syntax.</span></span>  

``` csharp
modelBuilder  
  .Entity<Post>()  
  .HasMany(p => p.Tags)  
  .WithMany(t => t.Posts)  
  .MapToStoredProcedures();
```  

<span data-ttu-id="b5a95-162">Wenn keine andere Konfiguration bereitgestellt wird, ist die folgende gespeicherte Prozedur Form standardmäßig verwendet.</span><span class="sxs-lookup"><span data-stu-id="b5a95-162">If no other configuration is supplied then the following stored procedure shape is used by default.</span></span>  

- <span data-ttu-id="b5a95-163">Zwei gespeicherte Prozeduren, die mit dem Namen  **\<Type_one\>\<Type_two\>e_infügen** und  **\<Type_one\>\<Type_two \>_Temporäre** (z. B. PostTag_Insert und PostTag_Delete).</span><span class="sxs-lookup"><span data-stu-id="b5a95-163">Two stored procedures named **\<type_one\>\<type_two\>_Insert** and **\<type_one\>\<type_two\>_Delete** (for example, PostTag_Insert and PostTag_Delete).</span></span>  
- <span data-ttu-id="b5a95-164">Die Parameter werden die wichtigsten Werte für jeden Typ.</span><span class="sxs-lookup"><span data-stu-id="b5a95-164">The parameters will be the key value(s) for each type.</span></span> <span data-ttu-id="b5a95-165">Der Name der einzelnen Parameter wird **\<Type_name\>_\<Property_name\>** (z. B. Post_PostId und Tag_TagId).</span><span class="sxs-lookup"><span data-stu-id="b5a95-165">The name of each parameter being **\<type_name\>_\<property_name\>** (for example, Post_PostId and Tag_TagId).</span></span>

<span data-ttu-id="b5a95-166">Hier einige Beispiele für einfügen und Aktualisieren von gespeicherten Prozeduren.</span><span class="sxs-lookup"><span data-stu-id="b5a95-166">Here are example insert and update stored procedures.</span></span>  

``` SQL
CREATE PROCEDURE [dbo].[PostTag_Insert]  
  @Post_PostId int,  
  @Tag_TagId int  
AS  
  INSERT INTO [dbo].[Post_Tags] (Post_PostId, Tag_TagId)   
  VALUES (@Post_PostId, @Tag_TagId)
CREATE PROCEDURE [dbo].[PostTag_Delete]  
  @Post_PostId int,  
  @Tag_TagId int  
AS  
  DELETE FROM [dbo].[Post_Tags]    
  WHERE Post_PostId = @Post_PostId AND Tag_TagId = @Tag_TagId
```  

### <a name="overriding-the-defaults"></a><span data-ttu-id="b5a95-167">Die Standardwerte außer Kraft setzen</span><span class="sxs-lookup"><span data-stu-id="b5a95-167">Overriding the Defaults</span></span>  

<span data-ttu-id="b5a95-168">Die Namen der Prozedur und Parameter können auf ähnliche Weise Entität, die gespeicherten Prozeduren konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="b5a95-168">The procedure and parameter names can be configured in a similar way to entity stored procedures.</span></span>  

``` csharp
modelBuilder  
  .Entity<Post>()  
  .HasMany(p => p.Tags)  
  .WithMany(t => t.Posts)  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.HasName("add_post_tag")  
                   .LeftKeyParameter(p => p.PostId, "post_id")  
                   .RightKeyParameter(t => t.TagId, "tag_id"))  
     .Delete(d => d.HasName("remove_post_tag")  
                   .LeftKeyParameter(p => p.PostId, "post_id")  
                   .RightKeyParameter(t => t.TagId, "tag_id")));
```  
