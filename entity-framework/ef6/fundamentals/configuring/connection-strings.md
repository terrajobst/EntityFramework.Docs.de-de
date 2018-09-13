---
title: Verbindungszeichenfolgen und -Modelle – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 294bb138-978f-4fe2-8491-fdf3cd3c60c4
ms.openlocfilehash: 2c9f084107e4de7f5439bf0082b46a3b538496e0
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490744"
---
# <a name="connection-strings-and-models"></a><span data-ttu-id="142b8-102">Verbindungszeichenfolgen und Modelle</span><span class="sxs-lookup"><span data-stu-id="142b8-102">Connection strings and models</span></span>
<span data-ttu-id="142b8-103">Dieses Thema behandelt wie Entity Framework die, die zu verwendende datenbankverbindung ermittelt und wie Sie sie ändern können.</span><span class="sxs-lookup"><span data-stu-id="142b8-103">This topic covers how Entity Framework discovers which database connection to use, and how you can change it.</span></span> <span data-ttu-id="142b8-104">Mit Code First und dem EF Designer erstellte Modelle, die beide in diesem Thema behandelt werden.</span><span class="sxs-lookup"><span data-stu-id="142b8-104">Models created with Code First and the EF Designer are both covered in this topic.</span></span>  

<span data-ttu-id="142b8-105">Eine Entity Framework-Anwendung verwendet in der Regel eine von DbContext abgeleitete Klasse.</span><span class="sxs-lookup"><span data-stu-id="142b8-105">Typically an Entity Framework application uses a class derived from DbContext.</span></span> <span data-ttu-id="142b8-106">Diese abgeleitete Klasse ruft einer der Konstruktoren für die DbContext-Basisklasse, um zu steuern:</span><span class="sxs-lookup"><span data-stu-id="142b8-106">This derived class will call one of the constructors on the base DbContext class to control:</span></span>  

- <span data-ttu-id="142b8-107">Wie der Kontext mit einer Datenbank hergestellt werden – gefunden "/" verwendet, also wie eine Verbindungszeichenfolge ist</span><span class="sxs-lookup"><span data-stu-id="142b8-107">How the context will connect to a database — that is, how a connection string is found/used</span></span>  
- <span data-ttu-id="142b8-108">Unabhängig davon, ob der Kontext wird Berechnen eines Modells mit Code First oder ein Modell erstellt, mit dem EF Designer laden</span><span class="sxs-lookup"><span data-stu-id="142b8-108">Whether the context will use calculate a model using Code First or load a model created with the EF Designer</span></span>  
- <span data-ttu-id="142b8-109">Zusätzliche erweiterte Optionen</span><span class="sxs-lookup"><span data-stu-id="142b8-109">Additional advanced options</span></span>  

<span data-ttu-id="142b8-110">Die folgenden Fragmente zeigen, dass einige der Möglichkeiten der "DbContext"-Konstruktor verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="142b8-110">The following fragments show some of the ways the DbContext constructors can be used.</span></span>  

## <a name="use-code-first-with-connection-by-convention"></a><span data-ttu-id="142b8-111">Code First mit Verbindung gemäß der Konvention verwenden</span><span class="sxs-lookup"><span data-stu-id="142b8-111">Use Code First with connection by convention</span></span>  

<span data-ttu-id="142b8-112">Wenn Sie eine beliebige andere Konfiguration in Ihrer Anwendung noch nicht getan haben, verursacht die rufen Sie dann des parameterlosen Konstruktors, klicken Sie auf "DbContext", dass "DbContext" mit einer Verbindung mit Datenbank gemäß der Konvention erstellt im Code-First-Modus ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="142b8-112">If you have not done any other configuration in your application, then calling the parameterless constructor on DbContext will cause DbContext to run in Code First mode with a database connection created by convention.</span></span> <span data-ttu-id="142b8-113">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="142b8-113">For example:</span></span>  

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

<span data-ttu-id="142b8-114">In diesem Beispiel "DbContext" verwendet einen Namespace gekennzeichneten Namen von Ihrem abgeleiteten Kontext class—Demo.EF.BloggingContext—as der Name der Datenbank und erstellt eine Verbindungszeichenfolge für diese Datenbank mit SQL Express oder LocalDB.</span><span class="sxs-lookup"><span data-stu-id="142b8-114">In this example DbContext uses the namespace qualified name of your derived context class—Demo.EF.BloggingContext—as the database name and creates a connection string for this database using either SQL Express or LocalDB.</span></span> <span data-ttu-id="142b8-115">Wenn beide installiert werden, wird SQL Express verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="142b8-115">If both are installed, SQL Express will be used.</span></span>  

<span data-ttu-id="142b8-116">Visual Studio 2010 enthält SQL Express, indem Sie Standard- und Visual Studio 2012 und höher umfasst LocalDB.</span><span class="sxs-lookup"><span data-stu-id="142b8-116">Visual Studio 2010 includes SQL Express by default and Visual Studio 2012 and later includes LocalDB.</span></span> <span data-ttu-id="142b8-117">Während der Installation überprüft EntityFramework NuGet-Paket an, welche Datenbankserver verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="142b8-117">During installation, the EntityFramework NuGet package checks which database server is available.</span></span> <span data-ttu-id="142b8-118">Das NuGet-Paket aktualisiert dann die Konfigurationsdatei durch Festlegen der Standarddatenbankserver, die Code First verwendet, wenn Sie eine Verbindung gemäß der Konvention zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="142b8-118">The NuGet package will then update the configuration file by setting the default database server that Code First uses when creating a connection by convention.</span></span> <span data-ttu-id="142b8-119">Wenn Sie SQL Express ausgeführt wird, wird er verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="142b8-119">If SQL Express is running, it will be used.</span></span> <span data-ttu-id="142b8-120">Wenn Sie SQL Express nicht verfügbar ist wird LocalDB stattdessen als Standard registriert.</span><span class="sxs-lookup"><span data-stu-id="142b8-120">If SQL Express is not available then LocalDB will be registered as the default instead.</span></span> <span data-ttu-id="142b8-121">In der Konfigurationsdatei werden keine Änderungen vorgenommen, wenn sie bereits über eine Einstellung für die standardverbindungsfactory enthält.</span><span class="sxs-lookup"><span data-stu-id="142b8-121">No changes are made to the configuration file if it already contains a setting for the default connection factory.</span></span>  

## <a name="use-code-first-with-connection-by-convention-and-specified-database-name"></a><span data-ttu-id="142b8-122">Verwenden Sie Code First mit einer Verbindung von der Aufrufkonvention und der angegebene Datenbankname</span><span class="sxs-lookup"><span data-stu-id="142b8-122">Use Code First with connection by convention and specified database name</span></span>  

<span data-ttu-id="142b8-123">Wenn Sie eine beliebige andere Konfiguration in Ihrer Anwendung noch nicht getan haben, bewirkt rufen Sie dann die String-Konstruktors, auf "DbContext", mit dem Datenbanknamen, die, den Sie verwenden möchten "DbContext" im Code-First-Modus ausgeführt, mit einer Verbindung mit Datenbank erstellt, die gemäß der Konvention, mit der Datenbank des, dass Dieser Name.</span><span class="sxs-lookup"><span data-stu-id="142b8-123">If you have not done any other configuration in your application, then calling the string constructor on DbContext with the database name you want to use will cause DbContext to run in Code First mode with a database connection created by convention to the database of that name.</span></span> <span data-ttu-id="142b8-124">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="142b8-124">For example:</span></span>  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingDatabase")
    {
    }
}
```  

<span data-ttu-id="142b8-125">In diesem Beispiel "DbContext" verwendet "BloggingDatabase" als Namen der Datenbank und erstellt eine Verbindungszeichenfolge für diese Datenbank mit SQL Express (installiert mit Visual Studio 2010) oder LocalDB (installiert mit Visual Studio 2012).</span><span class="sxs-lookup"><span data-stu-id="142b8-125">In this example DbContext uses “BloggingDatabase” as the database name and creates a connection string for this database using either SQL Express (installed with Visual Studio 2010) or LocalDB (installed with Visual Studio 2012).</span></span> <span data-ttu-id="142b8-126">Wenn beide installiert werden, wird SQL Express verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="142b8-126">If both are installed, SQL Express will be used.</span></span>  

## <a name="use-code-first-with-connection-string-in-appconfigwebconfig-file"></a><span data-ttu-id="142b8-127">Verwenden Sie Code First, mit der Verbindungszeichenfolge in app.config/web.config-Datei</span><span class="sxs-lookup"><span data-stu-id="142b8-127">Use Code First with connection string in app.config/web.config file</span></span>  

<span data-ttu-id="142b8-128">Sie können auswählen, um eine Verbindungszeichenfolge in der Datei "App.config" oder "Web.config" einfügen.</span><span class="sxs-lookup"><span data-stu-id="142b8-128">You may choose to put a connection string in your app.config or web.config file.</span></span> <span data-ttu-id="142b8-129">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="142b8-129">For example:</span></span>  

``` xml  
<configuration>
  <connectionStrings>
    <add name="BloggingCompactDatabase"
         providerName="System.Data.SqlServerCe.4.0"
         connectionString="Data Source=Blogging.sdf"/>
  </connectionStrings>
</configuration>
```  

<span data-ttu-id="142b8-130">Dies ist eine einfache Möglichkeit zum Teilen von "DbContext" einen Datenbankserver als SQL Express oder LocalDB verwenden – das obige Beispiel gibt eine SQL Server Compact Edition-Datenbank an.</span><span class="sxs-lookup"><span data-stu-id="142b8-130">This is an easy way to tell DbContext to use a database server other than SQL Express or LocalDB — the example above specifies a SQL Server Compact Edition database.</span></span>  

<span data-ttu-id="142b8-131">Wenn der Name der Verbindungszeichenfolge den Namen Ihres Kontexts (mit oder ohne namespacenennung) übereinstimmt wird dann es von "DbContext" gefunden bei der parameterlose Konstruktor verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="142b8-131">If the name of the connection string matches the name of your context (either with or without namespace qualification) then it will be found by DbContext when the parameterless constructor is used.</span></span> <span data-ttu-id="142b8-132">Wenn der Name der Verbindungszeichenfolge den Namen Ihres Kontexts unterscheidet, können Sie "DbContext", um diese Verbindung in Code First-Modus zu verwenden, indem der Name der Verbindungszeichenfolge an den "DbContext"-Konstruktor übergeben feststellen.</span><span class="sxs-lookup"><span data-stu-id="142b8-132">If the connection string name is different from the name of your context then you can tell DbContext to use this connection in Code First mode by passing the connection string name to the DbContext constructor.</span></span> <span data-ttu-id="142b8-133">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="142b8-133">For example:</span></span>  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingCompactDatabase")
    {
    }
}
```  

<span data-ttu-id="142b8-134">Alternativ können Sie die Form "Name =\<Verbindungszeichenfolgenname\>" für die Zeichenfolge, die an den "DbContext"-Konstruktor übergeben.</span><span class="sxs-lookup"><span data-stu-id="142b8-134">Alternatively, you can use the form “name=\<connection string name\>” for the string passed to the DbContext constructor.</span></span> <span data-ttu-id="142b8-135">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="142b8-135">For example:</span></span>  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("name=BloggingCompactDatabase")
    {
    }
}
```  

<span data-ttu-id="142b8-136">Dieses Formular vereinfacht die explizite Sie erwarten, dass die Verbindungszeichenfolge in der Config-Datei gefunden werden.</span><span class="sxs-lookup"><span data-stu-id="142b8-136">This form makes it explicit that you expect the connection string to be found in your config file.</span></span> <span data-ttu-id="142b8-137">Wenn eine Verbindungszeichenfolge mit dem angegebenen Namen nicht gefunden wird, wird eine Ausnahme ausgelöst werden.</span><span class="sxs-lookup"><span data-stu-id="142b8-137">An exception will be thrown if a connection string with the given name is not found.</span></span>  

## <a name="databasemodel-first-with-connection-string-in-appconfigwebconfig-file"></a><span data-ttu-id="142b8-138">Datenbankmodell/erste Verbindungszeichenfolge in app.config/web.config-Datei</span><span class="sxs-lookup"><span data-stu-id="142b8-138">Database/Model First with connection string in app.config/web.config file</span></span>  

<span data-ttu-id="142b8-139">Mit dem EF Designer erstellte Modelle unterscheiden sich von Code First, darin, dass Ihr Modell bereits vorhanden und nicht aus Code generiert wird, wenn die Anwendung ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="142b8-139">Models created with the EF Designer are different from Code First in that your model already exists and is not generated from code when the application runs.</span></span> <span data-ttu-id="142b8-140">Das Modell ist in der Regel als eine EDMX-Datei in Ihrem Projekt vorhanden.</span><span class="sxs-lookup"><span data-stu-id="142b8-140">The model typically exists as an EDMX file in your project.</span></span>  

<span data-ttu-id="142b8-141">Der Designer Fügt eine EF-Verbindungszeichenfolge zu Ihrer Datei "App.config" oder "Web.config".</span><span class="sxs-lookup"><span data-stu-id="142b8-141">The designer will add an EF connection string to your app.config or web.config file.</span></span> <span data-ttu-id="142b8-142">Diese Verbindungszeichenfolge Sonderregeln darin, dass es sich um Informationen zur Verwendung finden Sie die Informationen in der EDMX-Datei enthält.</span><span class="sxs-lookup"><span data-stu-id="142b8-142">This connection string is special in that it contains information about how to find the information in your EDMX file.</span></span> <span data-ttu-id="142b8-143">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="142b8-143">For example:</span></span>  

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

<span data-ttu-id="142b8-144">Die EF-Designer generiert außerdem Code, der angibt, "DbContext", um diese Verbindung verwendet werden, indem der Name der Verbindungszeichenfolge an die "DbContext"-Konstruktor übergeben.</span><span class="sxs-lookup"><span data-stu-id="142b8-144">The EF Designer will also generate code that tells DbContext to use this connection by passing the connection string name to the DbContext constructor.</span></span> <span data-ttu-id="142b8-145">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="142b8-145">For example:</span></span>  

``` csharp  
public class NorthwindContext : DbContext
{
    public NorthwindContext()
        : base("name=Northwind_Entities")
    {
    }
}
```  

<span data-ttu-id="142b8-146">DbContext weiß, um das vorhandene Modell (statt in Code berechnet mithilfe von Code First) zu laden, da die Verbindungszeichenfolge einer EF-Verbindungszeichenfolge, enthält die Details des Modells verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="142b8-146">DbContext knows to load the existing model (rather than using Code First to calculate it from code) because the connection string is an EF connection string containing details of the model to use.</span></span>  

## <a name="other-dbcontext-constructor-options"></a><span data-ttu-id="142b8-147">Andere Optionen für "DbContext"-Konstruktor</span><span class="sxs-lookup"><span data-stu-id="142b8-147">Other DbContext constructor options</span></span>  

<span data-ttu-id="142b8-148">Die DbContext-Klasse enthält andere Konstruktoren und die Verwendungsmuster, die einige erweiterte Szenarios zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="142b8-148">The DbContext class contains other constructors and usage patterns that enable some more advanced scenarios.</span></span> <span data-ttu-id="142b8-149">Einige davon sind:</span><span class="sxs-lookup"><span data-stu-id="142b8-149">Some of these are:</span></span>  

- <span data-ttu-id="142b8-150">Sie können die DbModelBuilder-Klasse verwenden, ein Code First-Modell zu erstellen, ohne einen "DbContext"-Instanz instanziieren.</span><span class="sxs-lookup"><span data-stu-id="142b8-150">You can use the DbModelBuilder class to build a Code First model without instantiating a DbContext instance.</span></span> <span data-ttu-id="142b8-151">Das Ergebnis davon ist ein DbModel-Objekt.</span><span class="sxs-lookup"><span data-stu-id="142b8-151">The result of this is a DbModel object.</span></span> <span data-ttu-id="142b8-152">Sie können dieses DbModel-Objekt klicken Sie dann auf eine der "DbContext"-Konstruktoren übergeben, wenn Sie bereit sind, Ihr "DbContext"-Instanz zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="142b8-152">You can then pass this DbModel object to one of the DbContext constructors when you are ready to create your DbContext instance.</span></span>  
- <span data-ttu-id="142b8-153">Sie können eine vollständige Verbindungszeichenfolge auf "DbContext" statt nur für die Verwendung der Datenbank oder die Verbindung Zeichenfolgenname übergeben.</span><span class="sxs-lookup"><span data-stu-id="142b8-153">You can pass a full connection string to DbContext instead of just the database or connection string name.</span></span> <span data-ttu-id="142b8-154">Standardmäßig wird diese Verbindungszeichenfolge mit den Anbieter "System.Data.SqlClient" verwendet. Dies kann geändert werden, indem Sie eine andere Implementierung der IConnectionFactory auf Kontext festlegen. Database.DefaultConnectionFactory.</span><span class="sxs-lookup"><span data-stu-id="142b8-154">By default this connection string is used with the System.Data.SqlClient provider; this can be changed by setting a different implementation of IConnectionFactory onto context.Database.DefaultConnectionFactory.</span></span>  
- <span data-ttu-id="142b8-155">Sie können ein vorhandenes DbConnection-Objekt durch Übergabe an einen "DbContext"-Konstruktor verwenden.</span><span class="sxs-lookup"><span data-stu-id="142b8-155">You can use an existing DbConnection object by passing it to a DbContext constructor.</span></span> <span data-ttu-id="142b8-156">Wenn das Verbindungsobjekt eine Instanz des EntityConnection-Objekt, und klicken Sie dann das Modell in der Verbindung angegebene werden verwendet, anstatt die Berechnung eines Modells mit Code First.</span><span class="sxs-lookup"><span data-stu-id="142b8-156">If the connection object is an instance of EntityConnection, then the model specified in the connection will be used rather than calculating a model using Code First.</span></span> <span data-ttu-id="142b8-157">Wenn das Objekt eine Instanz eines anderen Typs ist – z. B. SqlConnection, der Kontext für Code First-Modus verwendet.</span><span class="sxs-lookup"><span data-stu-id="142b8-157">If the object is an instance of some other type—for example, SqlConnection—then the context will use it for Code First mode.</span></span>  
- <span data-ttu-id="142b8-158">Sie können einen vorhandenen ObjectContext an einen "DbContext"-Konstruktor zum Erstellen eines "DbContext" umschließen die vorhandene Kontextfilter übergeben.</span><span class="sxs-lookup"><span data-stu-id="142b8-158">You can pass an existing ObjectContext to a DbContext constructor to create a DbContext wrapping the existing context.</span></span> <span data-ttu-id="142b8-159">Dies kann verwendet werden für vorhandene Anwendungen, die ObjectContext verwenden, aber die "DbContext" in einige Teile der Anwendung nutzen möchten.</span><span class="sxs-lookup"><span data-stu-id="142b8-159">This can be used for existing applications that use ObjectContext but which want to take advantage of DbContext in some parts of the application.</span></span>  
