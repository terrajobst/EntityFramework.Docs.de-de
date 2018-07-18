---
title: Code zuerst einfügen, aktualisieren und Löschen von gespeicherten Prozeduren – EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 9a7ae7f9-4072-4843-877d-506dd7eef576
caps.latest.revision: 3
ms.openlocfilehash: 1f100ed888abd98df83c80d0de2086cfb1ba7b4f
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2018
ms.locfileid: "39121465"
---
# <a name="code-first-insert-update-and-delete-stored-procedures"></a>Code zuerst einfügen, aktualisieren und Löschen von gespeicherten Prozeduren
> [!NOTE]
> **Nur EF6 und höher:** Die Features, APIs usw., die auf dieser Seite erläutert werden, wurden in Entity Framework 6 eingeführt. Wenn Sie eine frühere Version verwenden, gelten manche Informationen nicht.  

Standardmäßig wird Code First konfiguriert alle Entitäten für Insert ausführen, aktualisieren und löschen die Befehle mithilfe der direkten Tabellenzugriff. Ab EF6 Sie können Ihre Code First-Modell zur Verwendung gespeicherter Prozeduren für einige oder alle Entitäten in Ihrem Modell konfigurieren.  

## <a name="basic-entity-mapping"></a>Grundlegende Entitätszuordnung  

Sie können festlegen, verwenden von gespeicherten Prozeduren für Einfüge-, aktualisieren und löschen Sie mithilfe der Fluent-API.  

``` csharp
modelBuilder
  .Entity<Blog>()
  .MapToStoredProcedures();
```  

Dies bewirkt Code First einige Konventionen zu verwenden, um die erwartete Form der gespeicherten Prozeduren in der Datenbank zu erstellen.  

- Drei gespeicherte Prozeduren, die mit dem Namen  **\<Type_name\>e_infügen**,  **\<Type_name\>_Update** und  **\<Type_ Namen\>_temporäre** (z. B. Blog_Insert, Blog_Update und Blog_Delete).  
- Parameternamen entsprechen den Eigenschaftennamen.  
  > [!NOTE]
  > Wenn Sie zum Umbenennen der Spalte für eine bestimmte Eigenschaft HasColumnName() oder Column-Attribut verwenden, wird dieser Name für Parameter anstelle des Namens der Eigenschaft verwendet.  
- **Die Insert-Prozedur** weist einen Parameter für jede Eigenschaft, mit Ausnahme der als speichern generierter gekennzeichnet (Identität oder berechnet). Die gespeicherte Prozedur sollte es sich um ein Resultset mit einer Spalte für jede Eigenschaft speichern generierter zurückgeben.  
- **Das Update gespeicherte Prozedur** weist einen Parameter für jede Eigenschaft, außer denen mit einem generierten speichern-Muster von "Berechnet" gekennzeichnet. Einige parallelitätstoken einen Parameter für den ursprünglichen Wert erfordern, finden Sie unter den *Parallelitätstoken* Informationen weiter unten im Abschnitt. Die gespeicherte Prozedur sollte es sich um ein Resultset mit einer Spalte für jede berechnete Eigenschaft zurückgeben.  
- **Der Löschvorgang gespeicherte Prozedur** müssen einen Parameter für den Schlüsselwert, der die Entität (oder mehrere Parameter, wenn die Entität einen zusammengesetzten Schlüssel verfügt). Darüber hinaus muss die Prozedur Delete auch verfügen über Parameter für alle Fremdschlüssel unabhängige Zuordnung für die Zieltabelle (Beziehungen, die keine entsprechende Fremdschlüsseleigenschaften, die in der Entität deklariert haben). Einige parallelitätstoken einen Parameter für den ursprünglichen Wert erfordern, finden Sie unter den *Parallelitätstoken* Informationen weiter unten im Abschnitt.  

Verwenden Sie die folgende Klasse als Beispiel an:  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  public string Url { get; set; }  
}
```  

Die standardmäßige gespeicherte Prozeduren würde folgendermaßen lauten:  

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

### <a name="overriding-the-defaults"></a>Die Standardwerte außer Kraft setzen  

Sie können überschreiben, teilweise oder ganz wie standardmäßig konfiguriert wurde.  

Sie können den Namen des einen oder mehrere gespeicherte Prozeduren ändern. In diesem Beispiel wird die gespeicherte Updateprozedur nur.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog")));
```  

In diesem Beispiel wird alle drei gespeicherte Prozeduren.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog"))  
     .Delete(d => d.HasName("delete_blog"))  
     .Insert(i => i.HasName("insert_blog")));
```  

In diesen Beispielen die Aufrufe verkettet werden, aber Sie können auch die Syntax von Lambda-Block.  

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

In diesem Beispiel wird den Parameter für die BlogId-Eigenschaft für die gespeicherte Updateprozedur.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.BlogId, "blog_id")));
```  

Diese Aufrufe sind verkettbare und zusammensetzbar. Hier ist ein Beispiel, das alle drei gespeicherten Prozeduren und deren Parameter benennt.  

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

Sie können auch den Namen der Spalten im Resultset ändern, die generiert Datenbankwerte enthält.  

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

## <a name="relationships-without-a-foreign-key-in-the-class-independent-associations"></a>Beziehungen ohne ein Fremdschlüssel in der Klasse (unabhängigen Zuordnungen)  

Wenn eine foreign Key-Eigenschaft in der Klassendefinition enthalten ist, kann der entsprechende Parameter in die gleiche Weise wie jede andere Eigenschaft umbenannt werden. Wenn eine Beziehung, ohne eine Fremdschlüsseleigenschaft in der Klasse vorhanden ist, wird der Standardname für den Parameter  **\<Navigation_property_name\>_\<Primary_key_name\>**.  

Beispielsweise führt die folgenden Klassendefinitionen in einem Blog_BlogId-Parameter, die in den gespeicherten Prozeduren zum Einfügen und aktualisieren die Beiträge erwartet wird.  

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

### <a name="overriding-the-defaults"></a>Die Standardwerte außer Kraft setzen  

Sie können Parameter für die Fremdschlüssel ändern, die nicht in der Klasse enthalten sind, durch die Angabe des Pfads zur Eigenschaft primären Schlüssels für die Parameter-Methode.  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Parameter(p => p.Blog.BlogId, "blog_id")));
```  

Wenn Sie eine Navigationseigenschaft für die abhängige Entität (d.h. haben keine Post.Blog-Eigenschaft) können Sie die Zuordnungsmethode verwenden, um das andere Ende der Beziehung zu identifizieren und konfigurieren Sie dann die Parameter, die jeweils die Schlüsseleigenschaften entsprechen.  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Navigation<Blog>(  
      b => b.Posts,  
      c => c.Parameter(b => b.BlogId, "blog_id"))));
```  

## <a name="concurrency-tokens"></a>Parallelitätstoken  

Aktualisieren und löschen, die gespeicherten Prozeduren müssen möglicherweise auch für den Umgang mit Parallelität:  

- Wenn die Entität parallelitätstoken enthält, kann die gespeicherte Prozedur optional einen Output-Parameter aufweisen, der die Anzahl der Zeilen aktualisiert/gelöscht (Zeilen betroffen) zurückgibt. Solcher Parameter muss mit der Methode RowsAffectedParameter konfiguriert werden.  
EF verwendet standardmäßig den Rückgabewert von ExecuteNonQuery, um zu bestimmen, wie viele Zeilen betroffen sind. Angeben ein Output-Parameter für betroffene Zeilen ist nützlich, wenn Sie keinerlei Logik in Ihrem Sproc ausführen, die den Rückgabewert der ExecuteNonQuery wird falsch (aus Sicht des EF) verursachen würde am Ende der Ausführung.  
- Für die einzelnen Parallelität werden Token gibt es einen Parameter namens  **\<Property_name\>_Original** (z. B. Timestamp_Original). Dadurch wird den ursprünglichen Wert dieser Eigenschaft: der Wert, wenn die Abfrage aus der Datenbank übergeben werden.  
    - Parallelitätstoken, die von der Datenbank – z. B. Zeitstempel – berechnet werden, haben nur einen ursprünglichen-Parameter.  
    - Nicht berechneten Eigenschaften, die als parallelitätstoken festgelegt werden müssen auch einen Parameter für den neuen Wert in der Updateprozedur. Hierbei werden die Benennungskonventionen, die bereits erläuterten für neue Werte verwendet. Ein derartiges Token Beispiel würde die URL eines Blogs als ein parallelitätstoken verwenden, der neue Wert ist erforderlich, da dies kann durch Ihren Code (im Gegensatz zu einer Timestamp-Token die nur von der Datenbank aktualisiert wird) einen neuen Wert aktualisiert werden.  

Dies ist ein Beispiel-Klasse und die gespeicherte Prozedur mit einem Zeitstempel Concurrency Token zu aktualisieren.  

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

Hier ist ein Beispiel für Klasse, und aktualisieren Sie die gespeicherte Prozedur mit Token für nicht berechnete Parallelität.  

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

### <a name="overriding-the-defaults"></a>Die Standardwerte außer Kraft setzen  

Sie können optional einen Parameter für betroffene Zeilen einführen.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.RowsAffectedParameter("rows_affected")));
```  

Für die Datenbank berechnet parallelitätstoken –, bei dem nur der ursprüngliche Wert übergeben wird – können nur die Standardparameter umbenennen Mechanismus Sie so benennen Sie den Parameter für den ursprünglichen Wert um.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.Timestamp, "blog_timestamp")));
```  

Für nicht berechnete parallelitätstoken –, bei dem sowohl die ursprünglichen und neuen Wert übergeben werden – können Sie eine Überladung des Parameters, die Ihnen ermöglicht, einen Namen für jeden Parameter angeben.  

``` csharp
modelBuilder
 .Entity<Blog>()
 .MapToStoredProcedures(s => s.Update(u => u.Parameter(b => b.Url, "blog_url", "blog_original_url")));
```  

## <a name="many-to-many-relationships"></a>Viele-zu-viele-Beziehungen  

In diesem Abschnitt als Beispiel verwenden wir die folgenden Klassen.  

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

Gespeicherte Prozeduren mit der folgenden Syntax können viele zu viele Beziehungen zugeordnet werden.  

``` csharp
modelBuilder  
  .Entity<Post>()  
  .HasMany(p => p.Tags)  
  .WithMany(t => t.Posts)  
  .MapToStoredProcedures();
```  

Wenn keine andere Konfiguration bereitgestellt wird, ist die folgende gespeicherte Prozedur Form standardmäßig verwendet.  

- Zwei gespeicherte Prozeduren, die mit dem Namen  **\<Type_one\>\<Type_two\>e_infügen** und  **\<Type_one\>\<Type_two \>_Temporäre** (z. B. PostTag_Insert und PostTag_Delete).  
- Die Parameter werden die wichtigsten Werte für jeden Typ. Der Name der einzelnen Parameter wird **\<Type_name\>_\<Property_name\>** (z. B. Post_PostId und Tag_TagId).

Hier einige Beispiele für einfügen und Aktualisieren von gespeicherten Prozeduren.  

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

### <a name="overriding-the-defaults"></a>Die Standardwerte außer Kraft setzen  

Die Namen der Prozedur und Parameter können auf ähnliche Weise Entität, die gespeicherten Prozeduren konfiguriert werden.  

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
