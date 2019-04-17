---
title: Codebasierte Konfiguration – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 13886d24-2c74-4a00-89eb-aa0dee328d83
ms.openlocfilehash: 079a4ab30af74eac8b1f51ece5801ff40a867a29
ms.sourcegitcommit: 5280dcac4423acad8b440143433459b18886115b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2019
ms.locfileid: "59619284"
---
# <a name="code-based-configuration"></a><span data-ttu-id="fef98-102">Codebasierte Konfiguration</span><span class="sxs-lookup"><span data-stu-id="fef98-102">Code-based configuration</span></span>
> [!NOTE]
> <span data-ttu-id="fef98-103">**Nur EF6 und höher:** Die Features, APIs usw., die auf dieser Seite erläutert werden, wurden in Entity Framework 6 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="fef98-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="fef98-104">Wenn Sie eine frühere Version verwenden, gelten manche Informationen nicht.</span><span class="sxs-lookup"><span data-stu-id="fef98-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="fef98-105">Konfiguration für eine Entity Framework-Anwendung kann in einer Konfigurationsdatei (app.config/web.config) oder durch Code angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="fef98-105">Configuration for an Entity Framework application can be specified in a config file (app.config/web.config) or through code.</span></span> <span data-ttu-id="fef98-106">Letzteres wird als codebasierte Konfiguration bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="fef98-106">The latter is known as code-based configuration.</span></span>  

<span data-ttu-id="fef98-107">Konfiguration in einer Konfigurationsdatei finden Sie auf eine [separaten Artikel](config-file.md).</span><span class="sxs-lookup"><span data-stu-id="fef98-107">Configuration in a config file is described in a [separate article](config-file.md).</span></span> <span data-ttu-id="fef98-108">Codebasierte Konfiguration hat Vorrang vor die Config-Datei.</span><span class="sxs-lookup"><span data-stu-id="fef98-108">The config file takes precedence over code-based configuration.</span></span> <span data-ttu-id="fef98-109">Das heißt, wenn eine Konfigurationsoption in sowohl Code und in der Konfigurationsdatei festgelegt ist, wird die Einstellung in der Config-Datei verwendet.</span><span class="sxs-lookup"><span data-stu-id="fef98-109">In other words, if a configuration option is set in both code and in the config file, then the setting in the config file is used.</span></span>  

## <a name="using-dbconfiguration"></a><span data-ttu-id="fef98-110">Mithilfe von "dbconfiguration"</span><span class="sxs-lookup"><span data-stu-id="fef98-110">Using DbConfiguration</span></span>  

<span data-ttu-id="fef98-111">Codebasierte Konfiguration in EF 6 und höher wird erreicht, indem Sie eine Unterklasse von System.Data.Entity.Config.DbConfiguration erstellen.</span><span class="sxs-lookup"><span data-stu-id="fef98-111">Code-based configuration in EF6 and above is achieved by creating a subclass of System.Data.Entity.Config.DbConfiguration.</span></span> <span data-ttu-id="fef98-112">Die folgenden Richtlinien sollten eingehalten werden, wenn Unterklassen aus den "dbconfiguration":</span><span class="sxs-lookup"><span data-stu-id="fef98-112">The following guidelines should be followed when subclassing DbConfiguration:</span></span>  

- <span data-ttu-id="fef98-113">Erstellen Sie für Ihre Anwendung nur eine "dbconfiguration"-Klasse.</span><span class="sxs-lookup"><span data-stu-id="fef98-113">Create only one DbConfiguration class for your application.</span></span> <span data-ttu-id="fef98-114">Diese Klasse gibt die Anwendungsdomäne websiteweite Einstellungen.</span><span class="sxs-lookup"><span data-stu-id="fef98-114">This class specifies app-domain wide settings.</span></span>  
- <span data-ttu-id="fef98-115">Platzieren Sie die DbConfiguration-Klasse, in der gleichen Assembly wie die Klasse "DbContext".</span><span class="sxs-lookup"><span data-stu-id="fef98-115">Place your DbConfiguration class in the same assembly as your DbContext class.</span></span> <span data-ttu-id="fef98-116">(Finden Sie unter den *verschieben "dbconfiguration"* Abschnitt, wenn Sie dies ändern möchten.)</span><span class="sxs-lookup"><span data-stu-id="fef98-116">(See the *Moving DbConfiguration* section if you want to change this.)</span></span>  
- <span data-ttu-id="fef98-117">Geben Sie der DbConfiguration-Klasse einen öffentlichen parameterlosen Konstruktor.</span><span class="sxs-lookup"><span data-stu-id="fef98-117">Give your DbConfiguration class a public parameterless constructor.</span></span>  
- <span data-ttu-id="fef98-118">Optionen für die Konfiguration durch Aufrufen von geschützten "dbconfiguration"-Methoden in diesem Konstruktor festgelegt.</span><span class="sxs-lookup"><span data-stu-id="fef98-118">Set configuration options by calling protected DbConfiguration methods from within this constructor.</span></span>  

<span data-ttu-id="fef98-119">Befolgen diese Richtlinien können EF, um zu ermitteln und verwenden die Konfiguration automatisch durch beide Tools, die das Modell zugreifen muss und wenn Ihre Anwendung ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="fef98-119">Following these guidelines allows EF to discover and use your configuration automatically by both tooling that needs to access your model and when your application is run.</span></span>  

## <a name="example"></a><span data-ttu-id="fef98-120">Beispiel</span><span class="sxs-lookup"><span data-stu-id="fef98-120">Example</span></span>  

<span data-ttu-id="fef98-121">Eine von "dbconfiguration" abgeleitete Klasse könnte wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="fef98-121">A class derived from DbConfiguration might look like this:</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.SqlServer;

namespace MyNamespace
{
    public class MyConfiguration : DbConfiguration
    {
        public MyConfiguration()
        {
            SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
            SetDefaultConnectionFactory(new LocalDbConnectionFactory("mssqllocaldb"));
        }
    }
}
```  

<span data-ttu-id="fef98-122">Diese Klasse stellt EF so verwenden die Ausführungsstrategie für SQL Azure - Fehler bei Datenbankvorgängen - automatisch wiederholt und Local DB für Datenbanken verwenden, die gemäß der Konvention von Code First erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="fef98-122">This class sets up EF to use the SQL Azure execution strategy - to automatically retry failed database operations - and to use Local DB for databases that are created by convention from Code First.</span></span>  

## <a name="moving-dbconfiguration"></a><span data-ttu-id="fef98-123">"Dbconfiguration" verschieben</span><span class="sxs-lookup"><span data-stu-id="fef98-123">Moving DbConfiguration</span></span>  

<span data-ttu-id="fef98-124">Es gibt Fälle, in denen es nicht möglich, die DbConfiguration-Klasse in der gleichen Assembly wie die DbContext-Klasse platziert ist.</span><span class="sxs-lookup"><span data-stu-id="fef98-124">There are cases where it is not possible to place your DbConfiguration class in the same assembly as your DbContext class.</span></span> <span data-ttu-id="fef98-125">Angenommen, Sie jedes zwei "DbContext"-Klassen in verschiedenen Assemblys möglicherweise.</span><span class="sxs-lookup"><span data-stu-id="fef98-125">For example, you may have two DbContext classes each in different assemblies.</span></span> <span data-ttu-id="fef98-126">Es gibt zwei Möglichkeiten zum Durchführen dieser.</span><span class="sxs-lookup"><span data-stu-id="fef98-126">There are two options for handling this.</span></span>  

<span data-ttu-id="fef98-127">Die erste Option ist die Verwendung die Config-Datei, die zu verwendende "dbconfiguration"-Instanz an.</span><span class="sxs-lookup"><span data-stu-id="fef98-127">The first option is to use the config file to specify the DbConfiguration instance to use.</span></span> <span data-ttu-id="fef98-128">Legen Sie zu diesem Zweck das CodeConfigurationType-Attribut für den Abschnitt "EntityFramework" ein.</span><span class="sxs-lookup"><span data-stu-id="fef98-128">To do this, set the codeConfigurationType attribute of the entityFramework section.</span></span> <span data-ttu-id="fef98-129">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="fef98-129">For example:</span></span>  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyDbConfiguration, MyAssembly">
    ...Your EF config...
</entityFramework>
```  

<span data-ttu-id="fef98-130">Der Wert des CodeConfigurationType muss die Assembly und einen Namespace qualifizierten Namen der Klasse "dbconfiguration" sein.</span><span class="sxs-lookup"><span data-stu-id="fef98-130">The value of codeConfigurationType must be the assembly and namespace qualified name of your DbConfiguration class.</span></span>  

<span data-ttu-id="fef98-131">Die zweite Option besteht, DbConfigurationTypeAttribute auf Ihr Context-Klasse zu platzieren.</span><span class="sxs-lookup"><span data-stu-id="fef98-131">The second option is to place DbConfigurationTypeAttribute on your context class.</span></span> <span data-ttu-id="fef98-132">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="fef98-132">For example:</span></span>  

``` csharp  
[DbConfigurationType(typeof(MyDbConfiguration))]
public class MyContextContext : DbContext
{
}
```  

<span data-ttu-id="fef98-133">Der übergebene Wert für das Attribut kann entweder der Typ "dbconfiguration" – Siehe oben - oder die Assembly und den Namespace-qualifizierten Typnamenzeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="fef98-133">The value passed to the attribute can either be your DbConfiguration type - as shown above - or the assembly and namespace qualified type name string.</span></span> <span data-ttu-id="fef98-134">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="fef98-134">For example:</span></span>  

``` csharp
[DbConfigurationType("MyNamespace.MyDbConfiguration, MyAssembly")]
public class MyContextContext : DbContext
{
}
```  

## <a name="setting-dbconfiguration-explicitly"></a><span data-ttu-id="fef98-135">"Dbconfiguration" festlegen explizit</span><span class="sxs-lookup"><span data-stu-id="fef98-135">Setting DbConfiguration explicitly</span></span>  

<span data-ttu-id="fef98-136">Es gibt einige Situationen, in denen Konfiguration erforderlich sind, möglicherweise um einen beliebigen Typ von "DbContext" verwendet wurde.</span><span class="sxs-lookup"><span data-stu-id="fef98-136">There are some situations where configuration may be needed before any DbContext type has been used.</span></span> <span data-ttu-id="fef98-137">Beispiele hierfür sind:</span><span class="sxs-lookup"><span data-stu-id="fef98-137">Examples of this include:</span></span>  

- <span data-ttu-id="fef98-138">Verwenden zum Erstellen eines Modells ohne Kontext DbModelBuilder</span><span class="sxs-lookup"><span data-stu-id="fef98-138">Using DbModelBuilder to build a model without a context</span></span>  
- <span data-ttu-id="fef98-139">Mithilfe von anderen Framework-Hilfsprogramm Code, der einen "DbContext" verwendet, in diesem Kontext verwendet wird, bevor Kontext Ihrer Anwendung verwendet wird</span><span class="sxs-lookup"><span data-stu-id="fef98-139">Using some other framework/utility code that utilizes a DbContext where that context is used before your application context is used</span></span>  

<span data-ttu-id="fef98-140">In solchen Situationen EF nicht die Konfiguration automatisch zu erkennen ist, und stattdessen müssen Sie eine der folgenden:</span><span class="sxs-lookup"><span data-stu-id="fef98-140">In such situations EF is unable to discover the configuration automatically and you must instead do one of the following:</span></span>  

- <span data-ttu-id="fef98-141">Legen Sie den "dbconfiguration"-Typ in der Config-Datei an, wie beschrieben in der *verschieben "dbconfiguration"* weiter oben</span><span class="sxs-lookup"><span data-stu-id="fef98-141">Set the DbConfiguration type in the config file, as described in the *Moving DbConfiguration* section above</span></span>
- <span data-ttu-id="fef98-142">Rufen Sie die statische DbConfiguration.SetConfiguration-Methode während des Anwendungsstarts</span><span class="sxs-lookup"><span data-stu-id="fef98-142">Call the static DbConfiguration.SetConfiguration method during application startup</span></span>  

## <a name="overriding-dbconfiguration"></a><span data-ttu-id="fef98-143">Überschreiben von "dbconfiguration"</span><span class="sxs-lookup"><span data-stu-id="fef98-143">Overriding DbConfiguration</span></span>  

<span data-ttu-id="fef98-144">Es gibt einige Situationen, in dem Sie die Konfiguration, legen Sie in den "dbconfiguration" überschreiben müssen.</span><span class="sxs-lookup"><span data-stu-id="fef98-144">There are some situations where you need to override the configuration set in the DbConfiguration.</span></span> <span data-ttu-id="fef98-145">Dies erfolgt nicht in der Regel von Anwendungsentwicklern, sondern durch Drittanbieter-Anbieter und -Plug-ins, die eine abgeleitete Klasse von "dbconfiguration" nicht verwenden können.</span><span class="sxs-lookup"><span data-stu-id="fef98-145">This is not typically done by application developers but rather by third party providers and plug-ins that cannot use a derived DbConfiguration class.</span></span>  

<span data-ttu-id="fef98-146">Hierzu kann EntityFramework einen Ereignishandler registriert werden, der vorhandene Konfiguration ändern können, kurz bevor es gesperrt wird.</span><span class="sxs-lookup"><span data-stu-id="fef98-146">For this, EntityFramework allows an event handler to be registered that can modify existing configuration just before it is locked down.</span></span>  <span data-ttu-id="fef98-147">Darüber hinaus eine Sugar-Methode speziell für ersetzen alle Dienste, die von der EF-Service-Locator zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="fef98-147">It also provides a sugar method specifically for replacing any service returned by the EF service locator.</span></span> <span data-ttu-id="fef98-148">Dies ist, wie sie verwendet werden soll:</span><span class="sxs-lookup"><span data-stu-id="fef98-148">This is how it is intended to be used:</span></span>  

- <span data-ttu-id="fef98-149">Beim Starten der app (bevor EF verwendet wird) das plug-in oder der Anbieter sollte die Ereignishandlermethode für dieses Ereignis zu registrieren.</span><span class="sxs-lookup"><span data-stu-id="fef98-149">At app startup (before EF is used) the plug-in or provider should register the event handler method for this event.</span></span> <span data-ttu-id="fef98-150">(Beachten Sie, dass dies geschehen muss, bevor die Anwendung auf EF verwendet.)</span><span class="sxs-lookup"><span data-stu-id="fef98-150">(Note that this must happen before the application uses EF.)</span></span>  
- <span data-ttu-id="fef98-151">Der Ereignishandler ruft ReplaceService für jeden Dienst, der ersetzt werden muss.</span><span class="sxs-lookup"><span data-stu-id="fef98-151">The event handler makes a call to ReplaceService for every service that needs to be replaced.</span></span>  

<span data-ttu-id="fef98-152">Um IDbConnectionFactory und DbProviderService ersetzen würden Sie beispielsweise einen Handler könnte folgendermaßen aussehen registrieren:</span><span class="sxs-lookup"><span data-stu-id="fef98-152">For example, to replace IDbConnectionFactory and DbProviderService you would register a handler something like this:</span></span>  

``` csharp
DbConfiguration.Loaded += (_, a) =>
   {
       a.ReplaceService<DbProviderServices>((s, k) => new MyProviderServices(s));
       a.ReplaceService<IDbConnectionFactory>((s, k) => new MyConnectionFactory(s));
   };
```  

<span data-ttu-id="fef98-153">Stellen Sie im Code über MyProviderServices und MyConnectionFactory Ihre Implementierungen des Diensts dar.</span><span class="sxs-lookup"><span data-stu-id="fef98-153">In the code above MyProviderServices and MyConnectionFactory represent your implementations of the service.</span></span>  

<span data-ttu-id="fef98-154">Sie können auch zusätzliche Abhängigkeit-Ereignishandler, um dasselbe Ergebnis erreichen Sie hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="fef98-154">You can also add additional dependency handlers to get the same effect.</span></span>  

<span data-ttu-id="fef98-155">Beachten Sie, dass Sie konnte "DbProviderFactory" auch auf diese Weise umschließen, aber dies daher sich nur Entity Framework und der "DbProviderFactory" außerhalb von EF nicht verwendet wirkt.</span><span class="sxs-lookup"><span data-stu-id="fef98-155">Note that you could also wrap DbProviderFactory in this way, but doing so will only affect EF and not uses of the DbProviderFactory outside of EF.</span></span> <span data-ttu-id="fef98-156">Aus diesem Grund sollten Sie wahrscheinlich "DbProviderFactory" umschließen, wie Sie vor dem fortfahren zu können.</span><span class="sxs-lookup"><span data-stu-id="fef98-156">For this reason you’ll probably want to continue to wrap DbProviderFactory as you have before.</span></span>  

<span data-ttu-id="fef98-157">Sie sollten auch Beachten Sie die Dienste behalten, die Sie extern zu Ihrer Anwendung – z. B. beim Ausführen von Migrationen über die Paket-Manager-Konsole ausführen.</span><span class="sxs-lookup"><span data-stu-id="fef98-157">You should also keep in mind the services that you run externally to your application - for example, when running migrations from the Package Manager Console.</span></span> <span data-ttu-id="fef98-158">Migrieren Sie beim Ausführen über die Konsole mit dem versucht wird, Ihre "dbconfiguration" suchen.</span><span class="sxs-lookup"><span data-stu-id="fef98-158">When you run migrate from the console it will attempt to find your DbConfiguration.</span></span> <span data-ttu-id="fef98-159">Hängt jedoch davon, ob den Wrapper-Dienst abgerufen werden sollen wo der Ereignishandler, die sie registriert.</span><span class="sxs-lookup"><span data-stu-id="fef98-159">However, whether or not it will get the wrapped service depends on where the event handler it registered.</span></span> <span data-ttu-id="fef98-160">Wenn es im Rahmen der Erstellung des Ihre "dbconfiguration" registriert ist sollte der Code ausgeführt werden, und der Dienst sollte umschlossen zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="fef98-160">If it is registered as part of the construction of your DbConfiguration then the code should execute and the service should get wrapped.</span></span> <span data-ttu-id="fef98-161">In der Regel wird dies nicht der Fall, und dies bedeutet, dass die Tools den umschlossenen Dienst erhalten.</span><span class="sxs-lookup"><span data-stu-id="fef98-161">Usually this won’t be the case and this means that tooling won’t get the wrapped service.</span></span>  
