---
title: 'Protokollierung: EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f6e35c6d-45b7-4258-be1d-87c1bb67438d
uid: core/miscellaneous/logging
ms.openlocfilehash: efc78fbada3c59bf9cf2c4cb694835bb5ad60e76
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997004"
---
# <a name="logging"></a><span data-ttu-id="3a66c-102">Protokollierung</span><span class="sxs-lookup"><span data-stu-id="3a66c-102">Logging</span></span>

> [!TIP]  
> <span data-ttu-id="3a66c-103">Das in diesem Artikel verwendete [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) finden Sie auf GitHub.</span><span class="sxs-lookup"><span data-stu-id="3a66c-103">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) on GitHub.</span></span>

## <a name="aspnet-core-applications"></a><span data-ttu-id="3a66c-104">ASP.NET Core-Anwendungen</span><span class="sxs-lookup"><span data-stu-id="3a66c-104">ASP.NET Core applications</span></span>

<span data-ttu-id="3a66c-105">EF Core wird automatisch mit den Protokollierungsmechanismen von ASP.NET Core integriert. wenn `AddDbContext` oder `AddDbContextPool` verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="3a66c-105">EF Core integrates automatically with the logging mechanisms of ASP.NET Core whenever `AddDbContext` or `AddDbContextPool` is used.</span></span> <span data-ttu-id="3a66c-106">Aus diesem Grund bei der Verwendung von ASP.NET Core Protokollierung sollte konfiguriert werden wie beschrieben in der [ASP.NET Core-Dokumentation](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span><span class="sxs-lookup"><span data-stu-id="3a66c-106">Therefore, when using ASP.NET Core, logging should be configured as described in the [ASP.NET Core documentation](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>

## <a name="other-applications"></a><span data-ttu-id="3a66c-107">Andere Anwendungen</span><span class="sxs-lookup"><span data-stu-id="3a66c-107">Other applications</span></span>

<span data-ttu-id="3a66c-108">EF Core-Protokollierung derzeit muss ein "iloggerfactory" die selbst mit einem oder mehreren ILoggerProvider konfiguriert ist.</span><span class="sxs-lookup"><span data-stu-id="3a66c-108">EF Core logging currently requires an ILoggerFactory which is itself configured with one or more ILoggerProvider.</span></span> <span data-ttu-id="3a66c-109">Allgemeine Anbieter sind in den folgenden Paketen geliefert:</span><span class="sxs-lookup"><span data-stu-id="3a66c-109">Common providers are shipped in the following packages:</span></span>

* <span data-ttu-id="3a66c-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): eine einfache Konsolenanwendung-Protokollierung.</span><span class="sxs-lookup"><span data-stu-id="3a66c-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): A simple console logger.</span></span>
* <span data-ttu-id="3a66c-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): unterstützt die Azure App Services "Diagnoseprotokolle" und "Log Stream" Funktionen.</span><span class="sxs-lookup"><span data-stu-id="3a66c-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Supports Azure App Services 'Diagnostics logs' and 'Log stream' features.</span></span>
* <span data-ttu-id="3a66c-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): Protokolle, um einen Debugger Monitor System.Diagnostics.Debug.WriteLine() verwenden.</span><span class="sxs-lookup"><span data-stu-id="3a66c-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): Logs to a debugger monitor using System.Diagnostics.Debug.WriteLine().</span></span>
* <span data-ttu-id="3a66c-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Protokolle in Windows-Ereignisprotokoll.</span><span class="sxs-lookup"><span data-stu-id="3a66c-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Logs to Windows Event Log.</span></span>
* <span data-ttu-id="3a66c-114">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): EventSource/EventListener unterstützt.</span><span class="sxs-lookup"><span data-stu-id="3a66c-114">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): Supports EventSource/EventListener.</span></span>
* <span data-ttu-id="3a66c-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Protokolle, um einen Ablaufverfolgungslistener System.Diagnostics.TraceSource.TraceEvent() verwenden.</span><span class="sxs-lookup"><span data-stu-id="3a66c-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Logs to a trace listener using System.Diagnostics.TraceSource.TraceEvent().</span></span>

<span data-ttu-id="3a66c-116">Nach der Installation die entsprechenden Pakete, sollte die Anwendung eine Singleton oder globale Instanz von einem "loggerfactory" erstellen.</span><span class="sxs-lookup"><span data-stu-id="3a66c-116">After installing the appropriate package(s), the application should create a singleton/global instance of a LoggerFactory.</span></span> <span data-ttu-id="3a66c-117">Verwenden Sie beispielsweise die konsolenprotokollierung:</span><span class="sxs-lookup"><span data-stu-id="3a66c-117">For example, using the console logger:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

<span data-ttu-id="3a66c-118">Diese Singleton oder globale Instanz sollte klicken Sie dann mit EF Core registriert werden, auf die `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="3a66c-118">This singleton/global instance should then be registered with EF Core on the `DbContextOptionsBuilder`.</span></span> <span data-ttu-id="3a66c-119">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="3a66c-119">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> <span data-ttu-id="3a66c-120">Es ist sehr wichtig, dass Anwendungen keine neue Instanz der "iloggerfactory" für jede Context-Instanz erstellen.</span><span class="sxs-lookup"><span data-stu-id="3a66c-120">It is very important that applications do not create a new ILoggerFactory instance for each context instance.</span></span> <span data-ttu-id="3a66c-121">Auf diese Weise führt zu einem Speicherverlust und schlechter Leistung.</span><span class="sxs-lookup"><span data-stu-id="3a66c-121">Doing so will result in a memory leak and poor performance.</span></span>

## <a name="filtering-what-is-logged"></a><span data-ttu-id="3a66c-122">Filtern von protokolliert werden soll</span><span class="sxs-lookup"><span data-stu-id="3a66c-122">Filtering what is logged</span></span>

<span data-ttu-id="3a66c-123">Die einfachste Möglichkeit zum Filtern der protokolliert werden soll, ist es zu konfigurieren, wenn die ILoggerProvider zu registrieren.</span><span class="sxs-lookup"><span data-stu-id="3a66c-123">The easiest way to filter what is logged is to configure it when registering the ILoggerProvider.</span></span> <span data-ttu-id="3a66c-124">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="3a66c-124">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

<span data-ttu-id="3a66c-125">In diesem Beispiel wird das Protokoll gefiltert, um nur Nachrichten zurückzugeben:</span><span class="sxs-lookup"><span data-stu-id="3a66c-125">In this example, the log is filtered to return only messages:</span></span>
 * <span data-ttu-id="3a66c-126">in der Kategorie "Microsoft.EntityFrameworkCore.Database.Command"</span><span class="sxs-lookup"><span data-stu-id="3a66c-126">in the 'Microsoft.EntityFrameworkCore.Database.Command' category</span></span>
 * <span data-ttu-id="3a66c-127">auf der Ebene "Information"</span><span class="sxs-lookup"><span data-stu-id="3a66c-127">at the 'Information' level</span></span>

<span data-ttu-id="3a66c-128">Für EF Core die Protokollierungskategorien werden in definiert die `DbLoggerCategory` Klasse, um sie Kategorien, aber diese Suche nach zu vereinfachen, die in einfache Zeichenfolgen aufgelöst werden.</span><span class="sxs-lookup"><span data-stu-id="3a66c-128">For EF Core, logger categories are defined in the `DbLoggerCategory` class to make it easy to find categories, but these resolve to simple strings.</span></span>

<span data-ttu-id="3a66c-129">Weitere Informationen zu den zugrunde liegenden Infrastruktur für die Protokollierung befinden sich die [Dokumentation zur ASP.NET Core-Protokollierung](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span><span class="sxs-lookup"><span data-stu-id="3a66c-129">More details on the underlying logging infrastructure can be found in the [ASP.NET Core logging documentation](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>
