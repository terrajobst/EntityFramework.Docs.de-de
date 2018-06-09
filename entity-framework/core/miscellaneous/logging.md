---
title: Protokollierung - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f6e35c6d-45b7-4258-be1d-87c1bb67438d
ms.technology: entity-framework-core
uid: core/miscellaneous/logging
ms.openlocfilehash: 60d76bf3360eb47cdd9836494c1f135d1005a215
ms.sourcegitcommit: 3adf1267be92effc3c9daa893906a7f36834204f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35232135"
---
# <a name="logging"></a><span data-ttu-id="d4439-102">Protokollierung</span><span class="sxs-lookup"><span data-stu-id="d4439-102">Logging</span></span>

> [!TIP]  
> <span data-ttu-id="d4439-103">Das in diesem Artikel verwendete [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) finden Sie auf GitHub.</span><span class="sxs-lookup"><span data-stu-id="d4439-103">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) on GitHub.</span></span>

## <a name="aspnet-core-applications"></a><span data-ttu-id="d4439-104">ASP.NET Core-Anwendungen</span><span class="sxs-lookup"><span data-stu-id="d4439-104">ASP.NET Core applications</span></span>

<span data-ttu-id="d4439-105">EF Core Protokollierungsmechanismen integriert, automatisch die von ASP.NET Core immer `AddDbContext` oder `AddDbContextPool` verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="d4439-105">EF Core integrates automatically with the logging mechanisms of ASP.NET Core whenever `AddDbContext` or `AddDbContextPool` is used.</span></span> <span data-ttu-id="d4439-106">Daher bei der Verwendung von ASP.NET Core Protokollierung sollten konfiguriert werden wie beschrieben in der [Dokumentation zu ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span><span class="sxs-lookup"><span data-stu-id="d4439-106">Therefore, when using ASP.NET Core, logging should be configured as described in the [ASP.NET Core documentation](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>

## <a name="other-applications"></a><span data-ttu-id="d4439-107">Andere Anwendungen</span><span class="sxs-lookup"><span data-stu-id="d4439-107">Other applications</span></span>

<span data-ttu-id="d4439-108">EF Core derzeit Protokollierung erfordert ein iloggerfactory-Standardobjekt zur die selbst mit einem oder mehreren ILoggerProvider konfiguriert ist.</span><span class="sxs-lookup"><span data-stu-id="d4439-108">EF Core logging currently requires an ILoggerFactory which is itself configured with one or more ILoggerProvider.</span></span> <span data-ttu-id="d4439-109">Allgemeine Anbieter sind in den folgenden Paketen geliefert:</span><span class="sxs-lookup"><span data-stu-id="d4439-109">Common providers are shipped in the following packages:</span></span>

* <span data-ttu-id="d4439-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): eine einfache konsolenprotokollierung.</span><span class="sxs-lookup"><span data-stu-id="d4439-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): A simple console logger.</span></span>
* <span data-ttu-id="d4439-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): unterstützt das Azure-App-Dienste "Diagnoseprotokolle" und "Log Stream" Funktionen.</span><span class="sxs-lookup"><span data-stu-id="d4439-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Supports Azure App Services 'Diagnostics logs' and 'Log stream' features.</span></span>
* <span data-ttu-id="d4439-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): die Protokolle, um einen Debugger Monitor System.Diagnostics.Debug.WriteLine() verwenden.</span><span class="sxs-lookup"><span data-stu-id="d4439-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): Logs to a debugger monitor using System.Diagnostics.Debug.WriteLine().</span></span>
* <span data-ttu-id="d4439-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Windows-Ereignisprotokoll protokolliert.</span><span class="sxs-lookup"><span data-stu-id="d4439-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Logs to Windows Event Log.</span></span>
* <span data-ttu-id="d4439-114">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): EventSource/EventListener unterstützt.</span><span class="sxs-lookup"><span data-stu-id="d4439-114">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): Supports EventSource/EventListener.</span></span>
* <span data-ttu-id="d4439-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): die Protokolle, um einen Ablaufverfolgungslistener System.Diagnostics.TraceSource.TraceEvent() verwenden.</span><span class="sxs-lookup"><span data-stu-id="d4439-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Logs to a trace listener using System.Diagnostics.TraceSource.TraceEvent().</span></span>

<span data-ttu-id="d4439-116">Nach der Installation die entsprechenden Pakete, sollte die Anwendung eine Singleton/globale Instanz von einem LoggerFactory erstellen.</span><span class="sxs-lookup"><span data-stu-id="d4439-116">After installing the appropriate package(s), the application should create a singleton/global instance of a LoggerFactory.</span></span> <span data-ttu-id="d4439-117">Verwenden z. B. die konsolenprotokollierung:</span><span class="sxs-lookup"><span data-stu-id="d4439-117">For example, using the console logger:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

<span data-ttu-id="d4439-118">Diese Singleton/globale Instanz sollte dann mit EF-Kern-registriert werden, auf die `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="d4439-118">This singleton/global instance should then be registered with EF Core on the `DbContextOptionsBuilder`.</span></span> <span data-ttu-id="d4439-119">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="d4439-119">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> <span data-ttu-id="d4439-120">Es ist sehr wichtig, dass Anwendungen eine neue Instanz der iloggerfactory-Standardobjekt zur für jede Kontextinstanz keine erstellen.</span><span class="sxs-lookup"><span data-stu-id="d4439-120">It is very important that applications do not create a new ILoggerFactory instance for each context instance.</span></span> <span data-ttu-id="d4439-121">Auf diese Weise führt zu einem Speicherverlust und eine schlechte Leistung.</span><span class="sxs-lookup"><span data-stu-id="d4439-121">Doing so will result in a memory leak and poor performance.</span></span>

## <a name="filtering-what-is-logged"></a><span data-ttu-id="d4439-122">Filtern von protokolliert werden soll</span><span class="sxs-lookup"><span data-stu-id="d4439-122">Filtering what is logged</span></span>

<span data-ttu-id="d4439-123">Die einfachste Möglichkeit zum Filtern der protokolliert werden soll, wird beim Registrieren der ILoggerProvider zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="d4439-123">The easiest way to filter what is logged is to configure it when registering the ILoggerProvider.</span></span> <span data-ttu-id="d4439-124">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="d4439-124">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

<span data-ttu-id="d4439-125">In diesem Beispiel wird das Protokoll gefiltert, um nur Nachrichten zurückgeben:</span><span class="sxs-lookup"><span data-stu-id="d4439-125">In this example, the log is filtered to return only messages:</span></span>
 * <span data-ttu-id="d4439-126">in der Kategorie "Microsoft.EntityFrameworkCore.Database.Command"</span><span class="sxs-lookup"><span data-stu-id="d4439-126">in the 'Microsoft.EntityFrameworkCore.Database.Command' category</span></span>
 * <span data-ttu-id="d4439-127">auf der Ebene "Information"</span><span class="sxs-lookup"><span data-stu-id="d4439-127">at the 'Information' level</span></span>

<span data-ttu-id="d4439-128">Für EF-Kern, Protokollierung Kategorien definiert sind, der `DbLoggerCategory` Klasse, um die Kategorien, doch diese suchen zu erleichtern, die sich in einfache Zeichenfolgen auflösen lassen.</span><span class="sxs-lookup"><span data-stu-id="d4439-128">For EF Core, logger categories are defined in the `DbLoggerCategory` class to make it easy to find categories, but these resolve to simple strings.</span></span>

<span data-ttu-id="d4439-129">Weitere Informationen zu den zugrunde liegenden Infrastruktur für die Protokollierung finden Sie der [Dokumentation zu ASP.NET Core Protokollierung](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span><span class="sxs-lookup"><span data-stu-id="d4439-129">More details on the underlying logging infrastructure can be found in the [ASP.NET Core logging documentation](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>
