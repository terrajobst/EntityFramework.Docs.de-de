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
# <a name="logging"></a>Protokollierung

> [!TIP]  
> Das in diesem Artikel verwendete [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) finden Sie auf GitHub.

## <a name="aspnet-core-applications"></a>ASP.NET Core-Anwendungen

EF Core wird automatisch mit den Protokollierungsmechanismen von ASP.NET Core integriert. wenn `AddDbContext` oder `AddDbContextPool` verwendet wird. Aus diesem Grund bei der Verwendung von ASP.NET Core Protokollierung sollte konfiguriert werden wie beschrieben in der [ASP.NET Core-Dokumentation](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).

## <a name="other-applications"></a>Andere Anwendungen

EF Core-Protokollierung derzeit muss ein "iloggerfactory" die selbst mit einem oder mehreren ILoggerProvider konfiguriert ist. Allgemeine Anbieter sind in den folgenden Paketen geliefert:

* [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): eine einfache Konsolenanwendung-Protokollierung.
* [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): unterstützt die Azure App Services "Diagnoseprotokolle" und "Log Stream" Funktionen.
* [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): Protokolle, um einen Debugger Monitor System.Diagnostics.Debug.WriteLine() verwenden.
* [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Protokolle in Windows-Ereignisprotokoll.
* [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): EventSource/EventListener unterstützt.
* [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Protokolle, um einen Ablaufverfolgungslistener System.Diagnostics.TraceSource.TraceEvent() verwenden.

Nach der Installation die entsprechenden Pakete, sollte die Anwendung eine Singleton oder globale Instanz von einem "loggerfactory" erstellen. Verwenden Sie beispielsweise die konsolenprotokollierung:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

Diese Singleton oder globale Instanz sollte klicken Sie dann mit EF Core registriert werden, auf die `DbContextOptionsBuilder`. Zum Beispiel:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> Es ist sehr wichtig, dass Anwendungen keine neue Instanz der "iloggerfactory" für jede Context-Instanz erstellen. Auf diese Weise führt zu einem Speicherverlust und schlechter Leistung.

## <a name="filtering-what-is-logged"></a>Filtern von protokolliert werden soll

Die einfachste Möglichkeit zum Filtern der protokolliert werden soll, ist es zu konfigurieren, wenn die ILoggerProvider zu registrieren. Zum Beispiel:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

In diesem Beispiel wird das Protokoll gefiltert, um nur Nachrichten zurückzugeben:
 * in der Kategorie "Microsoft.EntityFrameworkCore.Database.Command"
 * auf der Ebene "Information"

Für EF Core die Protokollierungskategorien werden in definiert die `DbLoggerCategory` Klasse, um sie Kategorien, aber diese Suche nach zu vereinfachen, die in einfache Zeichenfolgen aufgelöst werden.

Weitere Informationen zu den zugrunde liegenden Infrastruktur für die Protokollierung befinden sich die [Dokumentation zur ASP.NET Core-Protokollierung](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).
