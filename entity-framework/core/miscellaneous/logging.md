---
title: Protokollierung - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f6e35c6d-45b7-4258-be1d-87c1bb67438d
ms.technology: entity-framework-core
uid: core/miscellaneous/logging
ms.openlocfilehash: 807560e563eddfb72d4286353b1403a0d2e2a441
ms.sourcegitcommit: 5367516f063cb42804ec92c31cdf76322554f2b5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2017
---
# <a name="logging"></a>Protokollierung

> [!TIP]  
> Sie können anzeigen, dass dieser Artikel [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) auf GitHub.

## <a name="aspnet-core-applications"></a>ASP.NET Core-Anwendungen

EF Core wird automatisch in die Protokollierung Mechanims von ASP.NET Core integriert, wenn `AddDbContext` oder `AddDbContextPool` verwendet wird. Daher bei der Verwendung von ASP.NET Core Protokollierung sollten konfiguriert werden wie beschrieben in der [Dokumentation zu ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).

## <a name="other-applications"></a>Andere Anwendungen

EF Core derzeit Protokollierung erfordert ein iloggerfactory-Standardobjekt zur die selbst mit einem oder mehreren ILoggerProvider konfiguriert ist. Allgemeine Anbieter sind in den folgenden Paketen geliefert:

* [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): eine einfache konsolenprotokollierung.
* [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): unterstützt das Azure-App-Dienste "Diagnoseprotokolle" und "Log Stream" Funktionen.
* [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): die Protokolle, um einen Debugger Monitor System.Diagnostics.Debug.WriteLine() verwenden.
* [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Windows-Ereignisprotokoll protokolliert.
* [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): EventSource/EventListener unterstützt.
* [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): die Protokolle, um einen Ablaufverfolgungslistener System.Diagnostics.TraceSource.TraceEvent() verwenden.

Nach der Installation die entsprechenden Pakete, sollte die Anwendung eine Singleton/globale Instanz von einem LoggerFactory erstellen. Verwenden z. B. die konsolenprotokollierung:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

Diese Singleton/globale Instanz sollte dann mit EF-Kern-registriert werden, auf die `DbContextOptionsBuilder`. Zum Beispiel:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> Es ist sehr wichtig, dass Anwendungen eine neue Instanz der iloggerfactory-Standardobjekt zur für jede Kontextinstanz keine erstellen. Auf diese Weise führt zu einem Speicherverlust und eine schlechte Leistung.

## <a name="filtering-what-is-logged"></a>Filtern von protokolliert werden soll

Die einfachste Möglichkeit zum Filtern der protokolliert werden soll, wird beim Registrieren der ILoggerProvider zu konfigurieren. Zum Beispiel:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

In diesem Beispiel wird das Protokoll gefiltert, um nur Nachrichten zurückgeben:
 * in der Kategorie "Microsoft.EntityFrameworkCore.Database.Command"
 * auf der Ebene "Information"

Für EF-Kern, Protokollierung Kategorien definiert sind, der `DbLoggerCategory` Klasse, um die Kategorien, doch diese suchen zu erleichtern, die sich in einfache Zeichenfolgen auflösen lassen.

Weitere Informationen zu den zugrunde liegenden Infrastruktur für die Protokollierung finden Sie der [Dokumentation zu ASP.NET Core Protokollierung](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).