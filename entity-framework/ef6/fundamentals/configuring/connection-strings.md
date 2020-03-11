---
title: Verbindungs Zeichenfolgen und Modelle EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 294bb138-978f-4fe2-8491-fdf3cd3c60c4
ms.openlocfilehash: 2c9f084107e4de7f5439bf0082b46a3b538496e0
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415952"
---
# <a name="connection-strings-and-models"></a><span data-ttu-id="d76ff-102">Verbindungs Zeichenfolgen und Modelle</span><span class="sxs-lookup"><span data-stu-id="d76ff-102">Connection strings and models</span></span>
<span data-ttu-id="d76ff-103">In diesem Thema wird erläutert, wie Entity Framework ermittelt, welche Datenbankverbindung verwendet werden soll, und wie Sie Sie ändern können.</span><span class="sxs-lookup"><span data-stu-id="d76ff-103">This topic covers how Entity Framework discovers which database connection to use, and how you can change it.</span></span> <span data-ttu-id="d76ff-104">Die Modelle, die mit Code First und dem EF-Designer erstellt wurden, werden in diesem Thema behandelt.</span><span class="sxs-lookup"><span data-stu-id="d76ff-104">Models created with Code First and the EF Designer are both covered in this topic.</span></span>  

<span data-ttu-id="d76ff-105">In der Regel verwendet eine Entity Framework Anwendung eine von dbcontext abgeleitete Klasse.</span><span class="sxs-lookup"><span data-stu-id="d76ff-105">Typically an Entity Framework application uses a class derived from DbContext.</span></span> <span data-ttu-id="d76ff-106">Diese abgeleitete Klasse ruft einen der Konstruktoren für die Basisklasse "dbcontext" auf, um Folgendes zu steuern:</span><span class="sxs-lookup"><span data-stu-id="d76ff-106">This derived class will call one of the constructors on the base DbContext class to control:</span></span>  

- <span data-ttu-id="d76ff-107">Wie der Kontext eine Verbindung mit einer Datenbank herstellt – d. h., wie eine Verbindungs Zeichenfolge gefunden bzw. verwendet wird</span><span class="sxs-lookup"><span data-stu-id="d76ff-107">How the context will connect to a database — that is, how a connection string is found/used</span></span>  
- <span data-ttu-id="d76ff-108">Ob der Kontext das Berechnen eines Modells mithilfe von Code First oder das Laden eines Modells verwendet, das mit dem EF-Designer erstellt wurde</span><span class="sxs-lookup"><span data-stu-id="d76ff-108">Whether the context will use calculate a model using Code First or load a model created with the EF Designer</span></span>  
- <span data-ttu-id="d76ff-109">Weitere Erweiterte Optionen</span><span class="sxs-lookup"><span data-stu-id="d76ff-109">Additional advanced options</span></span>  

<span data-ttu-id="d76ff-110">Die folgenden Fragmente zeigen einige Möglichkeiten, wie die dbcontext-Konstruktoren verwendet werden können.</span><span class="sxs-lookup"><span data-stu-id="d76ff-110">The following fragments show some of the ways the DbContext constructors can be used.</span></span>  

## <a name="use-code-first-with-connection-by-convention"></a><span data-ttu-id="d76ff-111">Code First mit Verbindung nach Konvention verwenden</span><span class="sxs-lookup"><span data-stu-id="d76ff-111">Use Code First with connection by convention</span></span>  

<span data-ttu-id="d76ff-112">Wenn Sie in der Anwendung keine andere Konfiguration ausgeführt haben, führt das Aufrufen des Parameter losen Konstruktors in dbcontext dazu, dass dbcontext im Code First Modus mit einer Datenbankverbindung ausgeführt wird, die von der Konvention erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="d76ff-112">If you have not done any other configuration in your application, then calling the parameterless constructor on DbContext will cause DbContext to run in Code First mode with a database connection created by convention.</span></span> <span data-ttu-id="d76ff-113">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="d76ff-113">For example:</span></span>  

``` csharp  
namespace Demo.EF
{
    public class BloggingContext : DbContext
    {
        public BloggingContext()
        // C# will call base class parameterless constructor by default
        {
        }
    }
}
```  

<span data-ttu-id="d76ff-114">In diesem Beispiel verwendet dbcontext den qualifizierten Namespace Namen ihrer abgeleiteten Kontext Klasse – Demo. EF. bloggingcontext – als Datenbanknamen und erstellt eine Verbindungs Zeichenfolge für diese Datenbank mithilfe von SQL Express oder localdb.</span><span class="sxs-lookup"><span data-stu-id="d76ff-114">In this example DbContext uses the namespace qualified name of your derived context class—Demo.EF.BloggingContext—as the database name and creates a connection string for this database using either SQL Express or LocalDB.</span></span> <span data-ttu-id="d76ff-115">Wenn beides installiert ist, wird SQL Express verwendet.</span><span class="sxs-lookup"><span data-stu-id="d76ff-115">If both are installed, SQL Express will be used.</span></span>  

<span data-ttu-id="d76ff-116">Visual Studio 2010 enthält standardmäßig SQL Express, und Visual Studio 2012 und höher enthalten localdb.</span><span class="sxs-lookup"><span data-stu-id="d76ff-116">Visual Studio 2010 includes SQL Express by default and Visual Studio 2012 and later includes LocalDB.</span></span> <span data-ttu-id="d76ff-117">Während der Installation überprüft das nuget-Paket "EntityFramework", welcher Datenbankserver verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="d76ff-117">During installation, the EntityFramework NuGet package checks which database server is available.</span></span> <span data-ttu-id="d76ff-118">Das nuget-Paket aktualisiert dann die Konfigurationsdatei, indem der von Code First verwendete Standarddaten Bankserver beim Erstellen einer Verbindung per Konvention festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="d76ff-118">The NuGet package will then update the configuration file by setting the default database server that Code First uses when creating a connection by convention.</span></span> <span data-ttu-id="d76ff-119">Wenn SQL Express ausgeführt wird, wird es verwendet.</span><span class="sxs-lookup"><span data-stu-id="d76ff-119">If SQL Express is running, it will be used.</span></span> <span data-ttu-id="d76ff-120">Wenn SQL Express nicht verfügbar ist, wird localdb stattdessen als Standard registriert.</span><span class="sxs-lookup"><span data-stu-id="d76ff-120">If SQL Express is not available then LocalDB will be registered as the default instead.</span></span> <span data-ttu-id="d76ff-121">An der Konfigurationsdatei werden keine Änderungen vorgenommen, wenn Sie bereits eine Einstellung für die standardverbindungsfactory enthält.</span><span class="sxs-lookup"><span data-stu-id="d76ff-121">No changes are made to the configuration file if it already contains a setting for the default connection factory.</span></span>  

## <a name="use-code-first-with-connection-by-convention-and-specified-database-name"></a><span data-ttu-id="d76ff-122">Code First mit Verbindung gemäß Konvention und angegebenem Datenbanknamen verwenden</span><span class="sxs-lookup"><span data-stu-id="d76ff-122">Use Code First with connection by convention and specified database name</span></span>  

<span data-ttu-id="d76ff-123">Wenn Sie in der Anwendung keine andere Konfiguration ausgeführt haben, führt der Aufruf des zeichenfolgenkonstruktors in dbcontext mit dem Datenbanknamen, den Sie verwenden möchten, dazu, dass dbcontext in Code First Modus mit einer Datenbankverbindung ausgeführt wird, die von der Konvention für die-Datenbank erstellt wurde. Dieser Name.</span><span class="sxs-lookup"><span data-stu-id="d76ff-123">If you have not done any other configuration in your application, then calling the string constructor on DbContext with the database name you want to use will cause DbContext to run in Code First mode with a database connection created by convention to the database of that name.</span></span> <span data-ttu-id="d76ff-124">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="d76ff-124">For example:</span></span>  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingDatabase")
    {
    }
}
```  

<span data-ttu-id="d76ff-125">In diesem Beispiel verwendet dbcontext "bloggingdatabase" als Datenbanknamen und erstellt eine Verbindungs Zeichenfolge für diese Datenbank entweder mithilfe von SQL Express (installiert mit Visual Studio 2010) oder localdb (installiert mit Visual Studio 2012).</span><span class="sxs-lookup"><span data-stu-id="d76ff-125">In this example DbContext uses “BloggingDatabase” as the database name and creates a connection string for this database using either SQL Express (installed with Visual Studio 2010) or LocalDB (installed with Visual Studio 2012).</span></span> <span data-ttu-id="d76ff-126">Wenn beides installiert ist, wird SQL Express verwendet.</span><span class="sxs-lookup"><span data-stu-id="d76ff-126">If both are installed, SQL Express will be used.</span></span>  

## <a name="use-code-first-with-connection-string-in-appconfigwebconfig-file"></a><span data-ttu-id="d76ff-127">Verwenden Sie Code First mit der Verbindungs Zeichenfolge in der Datei app. config/Web. config.</span><span class="sxs-lookup"><span data-stu-id="d76ff-127">Use Code First with connection string in app.config/web.config file</span></span>  

<span data-ttu-id="d76ff-128">Sie können eine Verbindungs Zeichenfolge in der Datei "App. config" oder "Web. config" platzieren.</span><span class="sxs-lookup"><span data-stu-id="d76ff-128">You may choose to put a connection string in your app.config or web.config file.</span></span> <span data-ttu-id="d76ff-129">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="d76ff-129">For example:</span></span>  

``` xml  
<configuration>
  <connectionStrings>
    <add name="BloggingCompactDatabase"
         providerName="System.Data.SqlServerCe.4.0"
         connectionString="Data Source=Blogging.sdf"/>
  </connectionStrings>
</configuration>
```  

<span data-ttu-id="d76ff-130">Dies ist eine einfache Möglichkeit, dbcontext anzuweisen, einen anderen Datenbankserver als SQL Express oder localdb zu verwenden – das obige Beispiel gibt eine SQL Server Compact Edition-Datenbank an.</span><span class="sxs-lookup"><span data-stu-id="d76ff-130">This is an easy way to tell DbContext to use a database server other than SQL Express or LocalDB — the example above specifies a SQL Server Compact Edition database.</span></span>  

<span data-ttu-id="d76ff-131">Wenn der Name der Verbindungs Zeichenfolge mit dem Namen Ihres Kontexts übereinstimmt (entweder mit oder ohne Namespace Qualifizierung), wird er von dbcontext gefunden, wenn der Parameter lose Konstruktor verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="d76ff-131">If the name of the connection string matches the name of your context (either with or without namespace qualification) then it will be found by DbContext when the parameterless constructor is used.</span></span> <span data-ttu-id="d76ff-132">Wenn sich der Name der Verbindungs Zeichenfolge vom Namen Ihres Kontexts unterscheidet, können Sie dbcontext anweisen, diese Verbindung im Code First Modus zu verwenden, indem Sie den Namen der Verbindungs Zeichenfolge an den dbcontext-Konstruktor übergeben.</span><span class="sxs-lookup"><span data-stu-id="d76ff-132">If the connection string name is different from the name of your context then you can tell DbContext to use this connection in Code First mode by passing the connection string name to the DbContext constructor.</span></span> <span data-ttu-id="d76ff-133">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="d76ff-133">For example:</span></span>  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingCompactDatabase")
    {
    }
}
```  

<span data-ttu-id="d76ff-134">Alternativ können Sie das Format "Name =\<Name der Verbindungs Zeichenfolge\>" für die Zeichenfolge verwenden, die an den dbcontext-Konstruktor übergeben wird.</span><span class="sxs-lookup"><span data-stu-id="d76ff-134">Alternatively, you can use the form “name=\<connection string name\>” for the string passed to the DbContext constructor.</span></span> <span data-ttu-id="d76ff-135">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="d76ff-135">For example:</span></span>  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("name=BloggingCompactDatabase")
    {
    }
}
```  

<span data-ttu-id="d76ff-136">Dieses Formular macht es explizit, dass die Verbindungs Zeichenfolge in der Konfigurationsdatei zu finden ist.</span><span class="sxs-lookup"><span data-stu-id="d76ff-136">This form makes it explicit that you expect the connection string to be found in your config file.</span></span> <span data-ttu-id="d76ff-137">Eine Ausnahme wird ausgelöst, wenn keine Verbindungs Zeichenfolge mit dem angegebenen Namen gefunden wird.</span><span class="sxs-lookup"><span data-stu-id="d76ff-137">An exception will be thrown if a connection string with the given name is not found.</span></span>  

## <a name="databasemodel-first-with-connection-string-in-appconfigwebconfig-file"></a><span data-ttu-id="d76ff-138">Datenbank/Model First mit Verbindungs Zeichenfolge in der Datei "App. config/Web. config"</span><span class="sxs-lookup"><span data-stu-id="d76ff-138">Database/Model First with connection string in app.config/web.config file</span></span>  

<span data-ttu-id="d76ff-139">Modelle, die mit dem EF-Designer erstellt wurden, unterscheiden sich von Code First darin, dass das Modell bereits vorhanden ist und nicht aus Code generiert wird, wenn die Anwendung ausgeführt wird</span><span class="sxs-lookup"><span data-stu-id="d76ff-139">Models created with the EF Designer are different from Code First in that your model already exists and is not generated from code when the application runs.</span></span> <span data-ttu-id="d76ff-140">Das Modell ist in der Regel als EDMX-Datei in Ihrem Projekt vorhanden.</span><span class="sxs-lookup"><span data-stu-id="d76ff-140">The model typically exists as an EDMX file in your project.</span></span>  

<span data-ttu-id="d76ff-141">Der Designer fügt eine EF-Verbindungs Zeichenfolge zu Ihrer Datei "App. config" oder "Web. config" hinzu.</span><span class="sxs-lookup"><span data-stu-id="d76ff-141">The designer will add an EF connection string to your app.config or web.config file.</span></span> <span data-ttu-id="d76ff-142">Diese Verbindungs Zeichenfolge ist insofern speziell, als Sie Informationen darüber enthält, wie Sie die Informationen in der EDMX-Datei finden.</span><span class="sxs-lookup"><span data-stu-id="d76ff-142">This connection string is special in that it contains information about how to find the information in your EDMX file.</span></span> <span data-ttu-id="d76ff-143">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="d76ff-143">For example:</span></span>  

``` xml  
<configuration>  
  <connectionStrings>  
    <add name="Northwind_Entities"  
         connectionString="metadata=res://*/Northwind.csdl|  
                                    res://*/Northwind.ssdl|  
                                    res://*/Northwind.msl;  
                           provider=System.Data.SqlClient;  
                           provider connection string=  
                               &quot;Data Source=.\sqlexpress;  
                                     Initial Catalog=Northwind;  
                                     Integrated Security=True;  
                                     MultipleActiveResultSets=True&quot;"  
         providerName="System.Data.EntityClient"/>  
  </connectionStrings>  
</configuration>
```  

<span data-ttu-id="d76ff-144">Der EF-Designer generiert außerdem Code, der dbcontext mitteilt, dass diese Verbindung verwendet werden soll, indem der Name der Verbindungs Zeichenfolge an den dbcontext-Konstruktor übergeben wird.</span><span class="sxs-lookup"><span data-stu-id="d76ff-144">The EF Designer will also generate code that tells DbContext to use this connection by passing the connection string name to the DbContext constructor.</span></span> <span data-ttu-id="d76ff-145">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="d76ff-145">For example:</span></span>  

``` csharp  
public class NorthwindContext : DbContext
{
    public NorthwindContext()
        : base("name=Northwind_Entities")
    {
    }
}
```  

<span data-ttu-id="d76ff-146">Dbcontext weiß, dass das vorhandene Modell geladen werden soll (anstatt Code First zu verwenden, um es aus Code zu berechnen), da die Verbindungs Zeichenfolge eine EF-Verbindungs Zeichenfolge mit Details des zu verwendenden Modells ist.</span><span class="sxs-lookup"><span data-stu-id="d76ff-146">DbContext knows to load the existing model (rather than using Code First to calculate it from code) because the connection string is an EF connection string containing details of the model to use.</span></span>  

## <a name="other-dbcontext-constructor-options"></a><span data-ttu-id="d76ff-147">Weitere dbcontext-konstruktoroptionen</span><span class="sxs-lookup"><span data-stu-id="d76ff-147">Other DbContext constructor options</span></span>  

<span data-ttu-id="d76ff-148">Die dbcontext-Klasse enthält andere Konstruktoren und Verwendungs Muster, die einige erweiterte Szenarien ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="d76ff-148">The DbContext class contains other constructors and usage patterns that enable some more advanced scenarios.</span></span> <span data-ttu-id="d76ff-149">Einige davon sind:</span><span class="sxs-lookup"><span data-stu-id="d76ff-149">Some of these are:</span></span>  

- <span data-ttu-id="d76ff-150">Sie können die dbmodelbuilder-Klasse verwenden, um ein Code First Modell zu erstellen, ohne eine dbcontext-Instanz zu instanziieren.</span><span class="sxs-lookup"><span data-stu-id="d76ff-150">You can use the DbModelBuilder class to build a Code First model without instantiating a DbContext instance.</span></span> <span data-ttu-id="d76ff-151">Das Ergebnis dieses Objekts ist ein dbmodel-Objekt.</span><span class="sxs-lookup"><span data-stu-id="d76ff-151">The result of this is a DbModel object.</span></span> <span data-ttu-id="d76ff-152">Sie können dieses dbmodel-Objekt dann an einen der dbcontext-Konstruktoren übergeben, wenn Sie bereit sind, die dbcontext-Instanz zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="d76ff-152">You can then pass this DbModel object to one of the DbContext constructors when you are ready to create your DbContext instance.</span></span>  
- <span data-ttu-id="d76ff-153">Sie können eine vollständige Verbindungs Zeichenfolge an dbcontext übergeben, anstatt nur den Namen der Datenbank oder der Verbindungs Zeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="d76ff-153">You can pass a full connection string to DbContext instead of just the database or connection string name.</span></span> <span data-ttu-id="d76ff-154">Diese Verbindungs Zeichenfolge wird standardmäßig mit dem System. Data. SqlClient-Anbieter verwendet. Dies kann geändert werden, indem eine andere iconnectionfactory-Implementierung auf den Kontext festgelegt wird. Database. defaultconnectionfactory.</span><span class="sxs-lookup"><span data-stu-id="d76ff-154">By default this connection string is used with the System.Data.SqlClient provider; this can be changed by setting a different implementation of IConnectionFactory onto context.Database.DefaultConnectionFactory.</span></span>  
- <span data-ttu-id="d76ff-155">Sie können ein vorhandenes DbConnection-Objekt verwenden, indem Sie es an einen dbcontext-Konstruktor übergeben.</span><span class="sxs-lookup"><span data-stu-id="d76ff-155">You can use an existing DbConnection object by passing it to a DbContext constructor.</span></span> <span data-ttu-id="d76ff-156">Wenn das Verbindungs Objekt eine Instanz von EntityConnection ist, wird das in der Verbindung angegebene Modell verwendet, anstatt ein Modell mithilfe von Code First zu berechnen.</span><span class="sxs-lookup"><span data-stu-id="d76ff-156">If the connection object is an instance of EntityConnection, then the model specified in the connection will be used rather than calculating a model using Code First.</span></span> <span data-ttu-id="d76ff-157">Wenn das Objekt eine Instanz eines anderen Typs ist – z. b. SqlConnection –, wird der Kontext ihn für Code First Modus verwenden.</span><span class="sxs-lookup"><span data-stu-id="d76ff-157">If the object is an instance of some other type—for example, SqlConnection—then the context will use it for Code First mode.</span></span>  
- <span data-ttu-id="d76ff-158">Sie können einen vorhandenen ObjectContext an einen dbcontext-Konstruktor übergeben, um einen dbcontext zu erstellen, der den vorhandenen Kontext umwickelt.</span><span class="sxs-lookup"><span data-stu-id="d76ff-158">You can pass an existing ObjectContext to a DbContext constructor to create a DbContext wrapping the existing context.</span></span> <span data-ttu-id="d76ff-159">Dies kann für vorhandene Anwendungen verwendet werden, die ObjectContext verwenden, aber in einigen Teilen der Anwendung dbcontext nutzen möchten.</span><span class="sxs-lookup"><span data-stu-id="d76ff-159">This can be used for existing applications that use ObjectContext but which want to take advantage of DbContext in some parts of the application.</span></span>  
