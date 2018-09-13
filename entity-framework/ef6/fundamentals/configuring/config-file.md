---
title: Dienstkonfigurations-Dateieinstellungen - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 000044c6-1d32-4cf7-ae1f-ea21d86ebf8f
ms.openlocfilehash: 949ad4f030205753c5fbf9b95f4d183d8c0d1fe7
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490871"
---
# <a name="configuration-file-settings"></a><span data-ttu-id="2ffee-102">Dienstkonfigurations-Dateieinstellungen</span><span class="sxs-lookup"><span data-stu-id="2ffee-102">Configuration File Settings</span></span>
<span data-ttu-id="2ffee-103">Entitätsframework ermöglicht eine Reihe von Einstellungen aus der Konfigurationsdatei angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="2ffee-103">Entity Framework allows a number of settings to be specified from the configuration file.</span></span> <span data-ttu-id="2ffee-104">EF befolgt in der Regel ein "Konvention geht vor Konfiguration"-Prinzip: alle Einstellungen, die in diesem Post erörterten über ein Standardverhalten verfügen, müssen Sie nur Gedanken um die Einstellung ändern, wenn der Standardwert nicht mehr auf Ihre Anforderungen erfüllt.</span><span class="sxs-lookup"><span data-stu-id="2ffee-104">In general EF follows a ‘convention over configuration’ principle: all the settings discussed in this post have a default behavior, you only need to worry about changing the setting when the default no longer satisfies your requirements.</span></span>  

## <a name="a-code-based-alternative"></a><span data-ttu-id="2ffee-105">Eine Alternative codebasierte</span><span class="sxs-lookup"><span data-stu-id="2ffee-105">A Code-Based Alternative</span></span>  

<span data-ttu-id="2ffee-106">Alle diese Einstellungen können auch mithilfe von Code angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="2ffee-106">All of these settings can also be applied using code.</span></span> <span data-ttu-id="2ffee-107">Ab EF6 eingeführt [codebasierte Konfiguration](code-based.md), die eine zentrale Methode zum Anwenden der Konfiguration von Code bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="2ffee-107">Starting in EF6 we introduced [code-based configuration](code-based.md), which provides a central way of applying configuration from code.</span></span> <span data-ttu-id="2ffee-108">Vor EF6 kann Konfiguration über Code angewendet werden, jedoch müssen Sie verschiedene APIs verwenden, um verschiedene Bereiche zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="2ffee-108">Prior to EF6, configuration can still be applied from code but you need to use various APIs to configure different areas.</span></span> <span data-ttu-id="2ffee-109">Die Konfigurationsdatei-Option kann diese Einstellungen während der Bereitstellung einfach geändert werden, ohne Aktualisierung des Codes.</span><span class="sxs-lookup"><span data-stu-id="2ffee-109">The configuration file option allows these settings to be easily changed during deployment without updating your code.</span></span>

## <a name="the-entity-framework-configuration-section"></a><span data-ttu-id="2ffee-110">Der Konfigurationsabschnitt für Entity Framework</span><span class="sxs-lookup"><span data-stu-id="2ffee-110">The Entity Framework Configuration Section</span></span>  

<span data-ttu-id="2ffee-111">Beginnend mit ef4. 1 konnte legen Sie den Datenbankinitialisierer für einen Kontext mit den **"appSettings"** Abschnitt der Konfigurationsdatei.</span><span class="sxs-lookup"><span data-stu-id="2ffee-111">Starting with EF4.1 you could set the database initializer for a context using the **appSettings** section of the configuration file.</span></span> <span data-ttu-id="2ffee-112">In EF 4.3 eingeführte wir das benutzerdefinierte **EntityFramework** Abschnitt aus, um die neuen Einstellungen zu behandeln.</span><span class="sxs-lookup"><span data-stu-id="2ffee-112">In EF 4.3 we introduced the custom **entityFramework** section to handle the new settings.</span></span> <span data-ttu-id="2ffee-113">Entitätsframework erkennt weiterhin Datenbankinitialisierer, die über das alte Format festgelegt, aber wir empfehlen den Umstieg auf das neue Format, wenn möglich.</span><span class="sxs-lookup"><span data-stu-id="2ffee-113">Entity Framework will still recognize database initializers set using the old format, but we recommend moving to the new format where possible.</span></span>

<span data-ttu-id="2ffee-114">Die **EntityFramework** Abschnitt wurde in die Konfigurationsdatei des Projekts automatisch hinzugefügt, wenn Sie das EntityFramework NuGet-Paket installiert.</span><span class="sxs-lookup"><span data-stu-id="2ffee-114">The **entityFramework** section was automatically added to the configuration file of your project when you installed the EntityFramework NuGet package.</span></span>  

``` xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <configSections>
    <!-- For more information on Entity Framework configuration, visit http://go.microsoft.com/fwlink/?LinkID=237468 -->
    <section name="entityFramework"
       type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, EntityFramework, Version=4.3.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />
  </configSections>
</configuration>
```  

## <a name="connection-strings"></a><span data-ttu-id="2ffee-115">Verbindungszeichenfolgen</span><span class="sxs-lookup"><span data-stu-id="2ffee-115">Connection Strings</span></span>  

<span data-ttu-id="2ffee-116">[Auf dieser Seite](~/ef6/fundamentals/configuring/connection-strings.md) finden Sie weitere Details darüber, wie Entity Framework die Datenbank verwendet werden, einschließlich der Verbindungszeichenfolgen in der Konfigurationsdatei.</span><span class="sxs-lookup"><span data-stu-id="2ffee-116">[This page](~/ef6/fundamentals/configuring/connection-strings.md) provides more details on how Entity Framework determines the database to be used, including connection strings in the configuration file.</span></span>  

<span data-ttu-id="2ffee-117">Verbindungszeichenfolgen wechseln Sie in der standardmäßigen **ConnectionStrings** Element und erfordern keine der **EntityFramework** Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="2ffee-117">Connection strings go in the standard **connectionStrings** element and do not require the **entityFramework** section.</span></span>  

<span data-ttu-id="2ffee-118">Zuerst Codemodelle verwenden normalen ADO.NET-Verbindungszeichenfolgen.</span><span class="sxs-lookup"><span data-stu-id="2ffee-118">Code First based models use normal ADO.NET connection strings.</span></span> <span data-ttu-id="2ffee-119">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="2ffee-119">For example:</span></span>  

``` xml
<connectionStrings>
  <add name="BlogContext"  
        providerName="System.Data.SqlClient"  
        connectionString="Server=.\SQLEXPRESS;Database=Blogging;Integrated Security=True;"/>
</connectionStrings>
```  

<span data-ttu-id="2ffee-120">EF Designer basierte Modelle verwenden spezielle EF-Verbindungszeichenfolgen.</span><span class="sxs-lookup"><span data-stu-id="2ffee-120">EF Designer based models use special EF connection strings.</span></span> <span data-ttu-id="2ffee-121">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="2ffee-121">For example:</span></span>  

``` xml  
<connectionStrings>
  <add name="BlogContext"  
    connectionString=
      "metadata=
        res://*/BloggingModel.csdl|
        res://*/BloggingModel.ssdl|
        res://*/BloggingModel.msl;
      provider=System.Data.SqlClient
      provider connection string=
        &quot;data source=(localdb)\mssqllocaldb;
        initial catalog=Blogging;
        integrated security=True;
        multipleactiveresultsets=True;&quot;"
     providerName="System.Data.EntityClient" />
</connectionStrings>
```

## <a name="code-based-configuration-type-ef6-onwards"></a><span data-ttu-id="2ffee-122">Typ der codebasierte Konfiguration (EF6 oder höher)</span><span class="sxs-lookup"><span data-stu-id="2ffee-122">Code-Based Configuration Type (EF6 Onwards)</span></span>  

<span data-ttu-id="2ffee-123">Ab mit EF6 können Sie angeben, die "dbconfiguration" für Entity Framework für die Verwendung [codebasierte Konfiguration](code-based.md) in Ihrer Anwendung.</span><span class="sxs-lookup"><span data-stu-id="2ffee-123">Starting with EF6, you can specify the DbConfiguration for EF to use for [code-based configuration](code-based.md) in your application.</span></span> <span data-ttu-id="2ffee-124">In den meisten Fällen müssen Sie diese Einstellung angeben, wie EF automatisch Ihre "dbconfiguration" ermittelt.</span><span class="sxs-lookup"><span data-stu-id="2ffee-124">In most cases you don't need to specify this setting as EF will automatically discover your DbConfiguration.</span></span> <span data-ttu-id="2ffee-125">Details der beim müssen Sie möglicherweise "dbconfiguration" in der Config-Datei angeben, finden Sie unter den **verschieben "dbconfiguration"** Abschnitt [codebasierte Konfiguration](code-based.md).</span><span class="sxs-lookup"><span data-stu-id="2ffee-125">For details of when you may need to specify DbConfiguration in your config file see the **Moving DbConfiguration** section of [Code-Based Configuration](code-based.md).</span></span>  

<span data-ttu-id="2ffee-126">Um einen Typ "dbconfiguration" festzulegen, geben Sie die Assembly qualifizierte Typname in der **CodeConfigurationType** Element.</span><span class="sxs-lookup"><span data-stu-id="2ffee-126">To set a DbConfiguration type, you specify the assembly qualified type name in the **codeConfigurationType** element.</span></span>  

> [!NOTE]
> <span data-ttu-id="2ffee-127">Ein qualifizierter Name der Assembly ist, einen Namespace gekennzeichneten Namen, gefolgt von einem Komma, klicken Sie dann auf die Assembly, die der Typ befindet.</span><span class="sxs-lookup"><span data-stu-id="2ffee-127">An assembly qualified name is the namespace qualified name, followed by a comma, then the assembly that the type resides in.</span></span> <span data-ttu-id="2ffee-128">Sie können optional auch angeben, die Assemblyversion, Kultur und Token des öffentlichen Schlüssels.</span><span class="sxs-lookup"><span data-stu-id="2ffee-128">You can optionally also specify the assembly version, culture and public key token.</span></span>  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyConfiguration, MyAssembly">
</entityFramework>
```  

## <a name="ef-database-providers-ef6-onwards"></a><span data-ttu-id="2ffee-129">EF-Datenbankanbieter (EF6 oder höher)</span><span class="sxs-lookup"><span data-stu-id="2ffee-129">EF Database Providers (EF6 Onwards)</span></span>  

<span data-ttu-id="2ffee-130">Vor EF6 Entity Framework-spezifische Teile eines Datenbankanbieters als Bestandteil des ADO.NET-Anbieters Core werden musste.</span><span class="sxs-lookup"><span data-stu-id="2ffee-130">Prior to EF6, Entity Framework-specific parts of a database provider had to be included as part of the core ADO.NET provider.</span></span> <span data-ttu-id="2ffee-131">EF6 ab, sind separat die bestimmte Teile von EF jetzt verwaltet und registriert.</span><span class="sxs-lookup"><span data-stu-id="2ffee-131">Starting with EF6, the EF specific parts are now managed and registered separately.</span></span>  

<span data-ttu-id="2ffee-132">Sie müssen normalerweise nicht Anbieter sich zu registrieren.</span><span class="sxs-lookup"><span data-stu-id="2ffee-132">Normally you won't need to register providers yourself.</span></span> <span data-ttu-id="2ffee-133">Dies wird in der Regel vom Anbieter ausgeführt werden, bei der Installation.</span><span class="sxs-lookup"><span data-stu-id="2ffee-133">This will typically be done by the provider when you install it.</span></span>  

<span data-ttu-id="2ffee-134">Anbieter werden registriert, dazu einen **Anbieter** Element unter der **Anbieter** Unterabschnitt von der **EntityFramework** Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="2ffee-134">Providers are registered by including a **provider** element under the **providers** child section of the **entityFramework** section.</span></span> <span data-ttu-id="2ffee-135">Es gibt zwei obligatorische Attribute für einen Anbietereintrag:</span><span class="sxs-lookup"><span data-stu-id="2ffee-135">There are two required attributes for a provider entry:</span></span>  

- <span data-ttu-id="2ffee-136">**InvariantName** identifiziert den ADO.NET-Anbieter Core, das dieser Ziele der EF-Anbieter</span><span class="sxs-lookup"><span data-stu-id="2ffee-136">**invariantName** identifies the core ADO.NET provider that this EF provider targets</span></span>  
- <span data-ttu-id="2ffee-137">**Typ** ist der vollqualifizierte Assemblyname der EF-Anbieter-Implementierung</span><span class="sxs-lookup"><span data-stu-id="2ffee-137">**type** is the assembly qualified type name of the EF provider implementation</span></span>  

> [!NOTE]
> <span data-ttu-id="2ffee-138">Ein qualifizierter Name der Assembly ist, einen Namespace gekennzeichneten Namen, gefolgt von einem Komma, klicken Sie dann auf die Assembly, die der Typ befindet.</span><span class="sxs-lookup"><span data-stu-id="2ffee-138">An assembly qualified name is the namespace qualified name, followed by a comma, then the assembly that the type resides in.</span></span> <span data-ttu-id="2ffee-139">Sie können optional auch angeben, die Assemblyversion, Kultur und Token des öffentlichen Schlüssels.</span><span class="sxs-lookup"><span data-stu-id="2ffee-139">You can optionally also specify the assembly version, culture and public key token.</span></span>  

<span data-ttu-id="2ffee-140">Als Beispiel hier ist der Eintrag erstellt, um die standardmäßige SQL Server-Ressourcenanbieter registrieren, bei der Installation von Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="2ffee-140">As an example here is the entry created to register the default SQL Server provider when you install Entity Framework.</span></span>  

``` xml  
<providers>
  <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
</providers>
```  

## <a name="interceptors-ef61-onwards"></a><span data-ttu-id="2ffee-141">Interceptors (EF6.1 oder höher)</span><span class="sxs-lookup"><span data-stu-id="2ffee-141">Interceptors (EF6.1 Onwards)</span></span>  

<span data-ttu-id="2ffee-142">Starten Sie mit EF6.1 kann Interceptors in der Konfigurationsdatei registrieren.</span><span class="sxs-lookup"><span data-stu-id="2ffee-142">Starting with EF6.1 you can register interceptors in the configuration file.</span></span> <span data-ttu-id="2ffee-143">Interceptors können Sie zusätzlichen Logik ausgeführt werden, wenn EF leistet großartiges und bestimmte Vorgänge wie das Ausführen von Datenbankabfragen, das Sie öffnen, Verbindungen usw.</span><span class="sxs-lookup"><span data-stu-id="2ffee-143">Interceptors allow you to run additional logic when EF performs certain operations, such as executing database queries, opening connections, etc.</span></span>  

<span data-ttu-id="2ffee-144">Interceptors werden registriert, indem einschließlich ein **Interceptor** Element unter den **Interceptors** Unterabschnitt von der **EntityFramework** Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="2ffee-144">Interceptors are registered by including an **interceptor** element under the **interceptors** child section of the **entityFramework** section.</span></span> <span data-ttu-id="2ffee-145">Die folgende Konfiguration registriert z. B. die integrierte **DatabaseLogger** Interceptor, den alle Datenbankvorgänge an der Konsole protokolliert werden.</span><span class="sxs-lookup"><span data-stu-id="2ffee-145">For example, the following configuration registers the built-in **DatabaseLogger** interceptor that will log all database operations to the Console.</span></span>  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework"/>
</interceptors>
```  

### <a name="logging-database-operations-to-a-file-ef61-onwards"></a><span data-ttu-id="2ffee-146">Protokollierung von Datenbankvorgängen in einer Datei (EF6.1 oder höher)</span><span class="sxs-lookup"><span data-stu-id="2ffee-146">Logging Database Operations to a File (EF6.1 Onwards)</span></span>  

<span data-ttu-id="2ffee-147">Interceptors über die Config-Datei registrieren ist besonders nützlich, wenn Sie die Protokollierung hinzuzufügen, zu einer vorhandenen Anwendung ein Problem debuggen möchten.</span><span class="sxs-lookup"><span data-stu-id="2ffee-147">Registering interceptors via the config file is especially useful when you want to add logging to an existing application to help debug an issue.</span></span> <span data-ttu-id="2ffee-148">**DatabaseLogger** unterstützt die Protokollierung in eine Datei durch Angabe der Dateiname als Konstruktorparameter.</span><span class="sxs-lookup"><span data-stu-id="2ffee-148">**DatabaseLogger** supports logging to a file by supplying the file name as a constructor parameter.</span></span>  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
    <parameters>
      <parameter value="C:\Temp\LogOutput.txt"/>
    </parameters>
  </interceptor>
</interceptors>
```  

<span data-ttu-id="2ffee-149">Standardmäßig wird dadurch die Protokolldatei mit einer neuen Datei jedes Mal überschrieben werden, die app gestartet wird.</span><span class="sxs-lookup"><span data-stu-id="2ffee-149">By default this will cause the log file to be overwritten with a new file each time the app starts.</span></span> <span data-ttu-id="2ffee-150">Wenn Sie stattdessen in das Protokoll anfügen Datei bereits vorhanden ist verwenden etwa:</span><span class="sxs-lookup"><span data-stu-id="2ffee-150">To instead append to the log file if it already exists use something like:</span></span>  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
    <parameters>
      <parameter value="C:\Temp\LogOutput.txt"/>
      <parameter value="true" type="System.Boolean"/>
    </parameters>
  </interceptor>
</interceptors>
```  

<span data-ttu-id="2ffee-151">Weitere Informationen zu **DatabaseLogger** und Registrieren von Interceptors finden Sie im Blogbeitrag [EF 6.1: das Aktivieren der Protokollierung ohne erneute Kompilierung](https://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/).</span><span class="sxs-lookup"><span data-stu-id="2ffee-151">For additional information on **DatabaseLogger** and registering interceptors, see the blog post [EF 6.1: Turning on logging without recompiling](https://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/).</span></span>  

## <a name="code-first-default-connection-factory"></a><span data-ttu-id="2ffee-152">Erste Standardverbindungsfactory Code</span><span class="sxs-lookup"><span data-stu-id="2ffee-152">Code First Default Connection Factory</span></span>  

<span data-ttu-id="2ffee-153">Der Konfigurationsabschnitt können Sie eine standardverbindungsfactory angeben, die Code First verwenden sollten, finden Sie eine Datenbank, die für einen Kontext verwendet.</span><span class="sxs-lookup"><span data-stu-id="2ffee-153">The configuration section allows you to specify a default connection factory that Code First should use to locate a database to use for a context.</span></span> <span data-ttu-id="2ffee-154">Die standardverbindungsfactory wird nur verwendet, wenn keine Verbindungszeichenfolge in die Konfigurationsdatei für einen Kontext hinzugefügt wurde.</span><span class="sxs-lookup"><span data-stu-id="2ffee-154">The default connection factory is only used when no connection string has been added to the configuration file for a context.</span></span>  

<span data-ttu-id="2ffee-155">Bei der Installation von EF-NuGet-Paket wurde eine standardverbindungsfactory registriert, der auf SQL Express oder LocalDB, je nachdem, welche Schlüssel Sie installiert haben verweist.</span><span class="sxs-lookup"><span data-stu-id="2ffee-155">When you installed the EF NuGet package a default connection factory was registered that points to either SQL Express or LocalDB, depending on which one you have installed.</span></span>  

<span data-ttu-id="2ffee-156">Um ein verbindungsfactory festzulegen, geben Sie die Assembly qualifizierte Typname in der **DeafultConnectionFactory** Element.</span><span class="sxs-lookup"><span data-stu-id="2ffee-156">To set a connection factory, you specify the assembly qualified type name in the **deafultConnectionFactory** element.</span></span>  

> [!NOTE]
> <span data-ttu-id="2ffee-157">Ein qualifizierter Name der Assembly ist, einen Namespace gekennzeichneten Namen, gefolgt von einem Komma, klicken Sie dann auf die Assembly, die der Typ befindet.</span><span class="sxs-lookup"><span data-stu-id="2ffee-157">An assembly qualified name is the namespace qualified name, followed by a comma, then the assembly that the type resides in.</span></span> <span data-ttu-id="2ffee-158">Sie können optional auch angeben, die Assemblyversion, Kultur und Token des öffentlichen Schlüssels.</span><span class="sxs-lookup"><span data-stu-id="2ffee-158">You can optionally also specify the assembly version, culture and public key token.</span></span>  

<span data-ttu-id="2ffee-159">Hier ist ein Beispiel für Ihre eigenen standardverbindungsfactory festlegen:</span><span class="sxs-lookup"><span data-stu-id="2ffee-159">Here is an example of setting your own default connection factory:</span></span>  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="MyNamespace.MyCustomFactory, MyAssembly"/>
</entityFramework>
```  

<span data-ttu-id="2ffee-160">Das obige Beispiel erfordert die benutzerdefinierte Factory, einen parameterlosen Konstruktor verfügt.</span><span class="sxs-lookup"><span data-stu-id="2ffee-160">The above example requires the custom factory to have a parameterless constructor.</span></span> <span data-ttu-id="2ffee-161">Bei Bedarf können Sie angeben, dass Konstruktorparameter, die mit der **Parameter** Element.</span><span class="sxs-lookup"><span data-stu-id="2ffee-161">If needed, you can specify constructor parameters using the **parameters** element.</span></span>  

<span data-ttu-id="2ffee-162">Beispielsweise erfordert die SqlCeConnectionFactory, die in Entity Framework enthalten ist, aufgefordert, einen invarianten Anbieternamen an den Konstruktor.</span><span class="sxs-lookup"><span data-stu-id="2ffee-162">For example, the SqlCeConnectionFactory, that is included in Entity Framework, requires you to supply a provider invariant name to the constructor.</span></span> <span data-ttu-id="2ffee-163">Der invariante Anbietername identifiziert die Version von SQL Compact, die Sie verwenden möchten.</span><span class="sxs-lookup"><span data-stu-id="2ffee-163">The provider invariant name identifies the version of SQL Compact you want to use.</span></span> <span data-ttu-id="2ffee-164">Die folgende Konfiguration führt dazu, dass Kontexte SQL Compact Version 4.0 wird standardmäßig verwendet.</span><span class="sxs-lookup"><span data-stu-id="2ffee-164">The following configuration will cause contexts to use SQL Compact version 4.0 by default.</span></span>  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlCeConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="System.Data.SqlServerCe.4.0" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

<span data-ttu-id="2ffee-165">Wenn Sie nicht, dass eine standardmäßige verbindungsfactory festlegen, Code First verwendet die SqlConnectionFactory, die auf `.\SQLEXPRESS`.</span><span class="sxs-lookup"><span data-stu-id="2ffee-165">If you don’t set a default connection factory, Code First uses the SqlConnectionFactory, pointing to `.\SQLEXPRESS`.</span></span> <span data-ttu-id="2ffee-166">SqlConnectionFactory verfügt auch über einen Konstruktor, der Ihnen ermöglicht, die Teile der Verbindungszeichenfolge zu überschreiben.</span><span class="sxs-lookup"><span data-stu-id="2ffee-166">SqlConnectionFactory also has a constructor that allows you to override parts of the connection string.</span></span> <span data-ttu-id="2ffee-167">Wenn Sie eine SQL Server-Instanz nicht verwenden möchten `.\SQLEXPRESS` verwenden Sie diesen Konstruktor, um den Server festzulegen.</span><span class="sxs-lookup"><span data-stu-id="2ffee-167">If you want to use a SQL Server instance other than `.\SQLEXPRESS` you can use this constructor to set the server.</span></span>  

<span data-ttu-id="2ffee-168">Die folgende Konfiguration führt dazu, dass Code First mit **Datenbankserver** für Kontexte, die eine explizite Verbindungszeichenfolge festgelegt haben.</span><span class="sxs-lookup"><span data-stu-id="2ffee-168">The following configuration will cause Code First to use **MyDatabaseServer** for contexts that don’t have an explicit connection string set.</span></span>  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="Data Source=MyDatabaseServer; Integrated Security=True; MultipleActiveResultSets=True" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

<span data-ttu-id="2ffee-169">Standardmäßig wird davon ausgegangen, dass der Konstruktorargumente vom Typzeichenfolge sind.</span><span class="sxs-lookup"><span data-stu-id="2ffee-169">By default, it’s assumed that constructor arguments are of type string.</span></span> <span data-ttu-id="2ffee-170">Sie können das Type-Attribut verwenden, um dies zu ändern.</span><span class="sxs-lookup"><span data-stu-id="2ffee-170">You can use the type attribute to change this.</span></span>  

``` xml
<parameter value="2" type="System.Int32" />
```  

## <a name="database-initializers"></a><span data-ttu-id="2ffee-171">Datenbankinitialisierer</span><span class="sxs-lookup"><span data-stu-id="2ffee-171">Database Initializers</span></span>  

<span data-ttu-id="2ffee-172">Datenbankinitialisierer werden auf einer Basis pro Kontext konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="2ffee-172">Database initializers are configured on a per-context basis.</span></span> <span data-ttu-id="2ffee-173">Sie können festgelegt werden, in der Konfiguration mithilfe der **Kontext** Element.</span><span class="sxs-lookup"><span data-stu-id="2ffee-173">They can be set in the configuration file using the **context** element.</span></span> <span data-ttu-id="2ffee-174">Dieses Element verwendet die Assembly qualifizierter Name zum Identifizieren des Kontexts, der konfiguriert wird.</span><span class="sxs-lookup"><span data-stu-id="2ffee-174">This element uses the assembly qualified name to identify the context being configured.</span></span>  

<span data-ttu-id="2ffee-175">Standardmäßig sind Code First-Kontexten zur Verwendung von "createDatabaseIfNotExists" Initialisierer konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="2ffee-175">By default, Code First contexts are configured to use the CreateDatabaseIfNotExists initializer.</span></span> <span data-ttu-id="2ffee-176">Gibt es eine **DisableDatabaseInitialization** -Attribut für die **Kontext** -Element, das zum Deaktivieren der Initialisierung der Datenbank verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="2ffee-176">There is a **disableDatabaseInitialization** attribute on the **context** element that can be used to disable database initialization.</span></span>  

<span data-ttu-id="2ffee-177">Die folgende Konfiguration deaktiviert z. B. die Initialisierung der Datenbank für den Blogging.BlogContext-Kontext, der in "MeineAssembly.dll" definiert.</span><span class="sxs-lookup"><span data-stu-id="2ffee-177">For example, the following configuration disables database initialization for the Blogging.BlogContext context defined in MyAssembly.dll.</span></span>  

``` xml  
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly" disableDatabaseInitialization="true" />
</contexts>
```  

<span data-ttu-id="2ffee-178">Sie können die **DatabaseInitializer** Element, das einen benutzerdefinierten Initialisierer festgelegt.</span><span class="sxs-lookup"><span data-stu-id="2ffee-178">You can use the **databaseInitializer** element to set a custom initializer.</span></span>  

``` xml
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="Blogging.MyCustomBlogInitializer, MyAssembly" />
  </context>
</contexts>
```  

<span data-ttu-id="2ffee-179">Konstruktorparameter verwenden dieselbe Syntax wie standardmäßige Verbindung Factorys.</span><span class="sxs-lookup"><span data-stu-id="2ffee-179">Constructor parameters use the same syntax as default connection factories.</span></span>  

``` xml  
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="Blogging.MyCustomBlogInitializer, MyAssembly">
      <parameters>
        <parameter value="MyConstructorParameter" />
      </parameters>
    </databaseInitializer>
  </context>
</contexts>
```  

<span data-ttu-id="2ffee-180">Sie können eines der allgemeinen Datenbank-Initialisierer konfigurieren, die in Entity Framework enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="2ffee-180">You can configure one of the generic database initializers that are included in Entity Framework.</span></span> <span data-ttu-id="2ffee-181">Die **Typ** Attribut verwendet das .NET Framework-Format für generische Typen.</span><span class="sxs-lookup"><span data-stu-id="2ffee-181">The **type** attribute uses the .NET Framework format for generic types.</span></span>  

<span data-ttu-id="2ffee-182">Z. B. Wenn Sie Code First-Migrationen verwenden, können Sie die Datenbank konfigurieren, die migriert werden automatisch mit der `MigrateDatabaseToLatestVersion<TContext, TMigrationsConfiguration>` Initialisierer.</span><span class="sxs-lookup"><span data-stu-id="2ffee-182">For example, if you are using Code First Migrations, you can configure the database to be migrated automatically using the `MigrateDatabaseToLatestVersion<TContext, TMigrationsConfiguration>` initializer.</span></span>  

``` xml
<contexts>
  <context type="Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="System.Data.Entity.MigrateDatabaseToLatestVersion`2[[Blogging.BlogContext, MyAssembly], [Blogging.Migrations.Configuration, MyAssembly]], EntityFramework" />
  </context>
</contexts>
```
