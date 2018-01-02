---
title: "Unterstützte .NET-Implementierungen – EF Core"
author: rowanmiller
ms.author: divega
ms.date: 08/30/2017
ms.technology: entity-framework-core
uid: core/platforms/index
ms.openlocfilehash: 6b6ea9ca833810169d0d850f3bea8776b761c007
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
---
# <a name="net-implementations-supported-by-ef-core"></a>Von EF Core unterstützte .NET-Implementierungen

Wir möchten, dass EF Core in allen Anwendungen verfügbar ist, in denen Sie .NET-Code schreiben können, und wir arbeiten nach wie vor kontinuierlich an der Erreichung dieses Ziels. Die folgende Tabelle enthält Anweisungen zu den einzelnen .NET-Implementierungen, bei denen wir EF Core aktivieren möchten.

Für EF Core 2.0 wird die Zielversion für [.NET Standard 2.0](https://docs.microsoft.com/dotnet/standard/net-standard) festgelegt, und es erfordert daher .NET-Implementierungen, die Unterstützung dafür bieten.

| .NET-Implementierung | Status | Erfordert 1.x | Erfordert 2.x
|-|-|-|-
| **.NET Core** ([ASP.NET Core](../get-started/aspnetcore/index.md), [Konsole](../get-started/netcore/index.md) etc.) | **Vollständig unterstützt und empfohlen:** Wird von automatisierten Tests und vielen Anwendungen abgedeckt, von denen bekannt ist, dass sie erfolgreich eingesetzt werden. | [.NET Core SDK 1.x](https://www.microsoft.com/net/core/) | [.NET Core SDK 2.x](https://www.microsoft.com/net/core/)
| **.NET Framework** (Windows Forms, WPF, ASP.NET, [Konsole](../get-started/full-dotnet/index.md) etc.) | **Vollständig unterstützt und empfohlen:** Wird von automatisierten Tests und vielen Anwendungen abgedeckt, von denen bekannt ist, dass sie erfolgreich eingesetzt werden. EF 6 ist auch in dieser Plattform verfügbar (weitere Informationen zum Auswählen der richtigen Technologie finden Sie unter [Vergleichen von EF Core und EF 6](../../efcore-and-ef6/index.md)). | .NET Framework 4.5.1 | .NET Framework 4.6.1
| **Mono und Xamarin** | **In Bearbeitung – Es können Probleme auftreten:** Einige Tests wurden vom EF Core-Team und den Kunden durchgeführt. Early Adopters haben zwar über Erfolge berichtet, es sind jedoch auch [Probleme aufgetreten](https://github.com/aspnet/entityframework/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-xamarin), und im Zuge der Tests werden wahrscheinlich weitere Probleme zutage treten. Besonders bei der Erweiterung Xamarin.iOS wurden Einschränkungen verzeichnet, die die Funktionsweise einiger mit EF Core 2.0 entwickelten Anwendungen beeinträchtigen können. | Mono 4.6 <br/> Xamarin.iOS 10 <br/> Xamarin.Mac 3 <br/> Xamarin.Android 7 | Mono 5.4 <br/> Xamarin.iOS 10.14 <br/> Xamarin.Mac 3.8 <br/> Xamarin.Android 7.5
| [**Universelle Windows-Plattform**](../get-started/uwp/index.md) | **In Bearbeitung – Es können Probleme auftreten:** Einige Tests wurden vom EF Core-Team und den Kunden durchgeführt. Es wurden [Probleme](https://github.com/aspnet/entityframework/issues?utf8=%E2%9C%93&q=is%3Aopen%20is%3Aissue%20label%3Aarea-uwp%20) bei der Kompilierung mit der nativen .NET-Toolkette gemeldet, die typischerweise während eines Releasebuilds verwendet wird und eine Voraussetzung für die Bereitstellung im Microsoft Store darstellt (wenn Sie .NET Native nicht verwenden oder einfach nur experimentieren möchten, werden Sie von der Vielzahl der Probleme nicht betroffen sein). | [Neuestes .NET UWP 5-Paket](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/5.4.1) | [Neuestes .NET UWP 6-Paket](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/) <sup>(1)</sup>

<sup>(1) </sup> In dieser .NET UWP-Version wird Unterstützung für .NET Standard 2.0 und .NET Native 2.0 hinzugefügt, wodurch die meisten zuvor gemeldeten Kompatibilitätsprobleme behoben werden. Bei den Tests wurden jedoch [einige noch bestehende Probleme](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A2.0.1+label%3Aarea-uwp) mit EF Core 2.0 festgestellt, die in einem bevorstehenden Patchrelease behoben werden sollen.

Für alle Kombinationen, die nicht wie erwartet funktioniert, empfehlen wir, neue Probleme in der [EF Core-Problemverfolgung](https://github.com/aspnet/entityframeworkcore/issues/new) und bei Xamarin-bezogenen Problemen in der [Xamarin-Problemverfolgung](https://bugzilla.xamarin.com/newbug) zu erstellen.
