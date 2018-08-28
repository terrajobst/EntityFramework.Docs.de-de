---
title: Unterstützte .NET-Implementierungen – EF Core
author: rowanmiller
ms.date: 08/30/2017
uid: core/platforms/index
ms.openlocfilehash: 347965818f0eab9a86411f66eaaf10cb3aa8d652
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996437"
---
# <a name="net-implementations-supported-by-ef-core"></a>Von EF Core unterstützte .NET-Implementierungen

Wir möchten, dass EF Core in allen Anwendungen verfügbar ist, in denen Sie .NET-Code schreiben können, und wir arbeiten nach wie vor kontinuierlich an der Erreichung dieses Ziels. Obwohl die EF Core-Unterstützung für .NET Core und .NET Framework durch automatisierte Tests abgedeckt wird und viele Clientanwendungen diese erwiesenermaßen erfolgreich verwendet haben, treten bei Mono, Xamarin und der UWP einige Probleme auf.

## <a name="overview"></a>Übersicht

Die folgende Tabelle enthält Anweisungen zu den einzelnen .NET-Implementierungen:

| .NET-Implementierung                                                                                                  | Status                                                             | EF Core 1.x-Anforderungen                                                                                | EF Core 2.x-Anforderungen <sup>(1)</sup>                                                                 |
|:---------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------|
| **.NET Core** ([ASP.NET Core](../get-started/aspnetcore/index.md), [Konsole](../get-started/netcore/index.md) etc.) | Vollständig unterstützt und empfohlen                                    | [.NET Core SDK 1.x](https://www.microsoft.com/net/core/)                                                | [.NET Core SDK 2.x](https://www.microsoft.com/net/core/)                                                |
| **.NET Framework** (Windows Forms, WPF, ASP.NET, [Konsole](../get-started/full-dotnet/index.md) etc.)                    | Vollständig unterstützt und empfohlen. EF 6 ebenfalls verfügbar <sup>(2)</sup> | .NET Framework 4.5.1                                                                                    | .NET Framework 4.6.1                                                                                    |
| **Mono und Xamarin**                                                                                                   | In Bearbeitung <sup>(3)</sup>                                         | Mono 4.6 <br/> Xamarin.iOS 10 <br/> Xamarin.Mac 3 <br/> Xamarin.Android 7                               | Mono 5.4 <br/> Xamarin.iOS 10.14 <br/> Xamarin.Mac 3.8 <br/> Xamarin.Android 7.5                        |
| [**Universelle Windows-Plattform**](../get-started/uwp/index.md)                                                        | EF Core 2.0.1 empfohlen <sup>(4)</sup>                           | [.NET Core UWP 5.x-Paket](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/) | [.NET Core UWP 6.x-Paket](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform/) |

<sup>(1)</sup> Für EF Core 2.0 wird die Zielversion für [.NET Standard 2.0](https://docs.microsoft.com/dotnet/standard/net-standard) festgelegt, und es erfordert daher .NET-Implementierungen, die Unterstützung hierfür bieten.

<sup>(2)</sup> Weitere Informationen zum Auswählen der richtigen Technologie finden Sie unter [Vergleichen von EF Core und EF 6](../../efcore-and-ef6/index.md).

<sup>(3)</sup> Bei Xamarin wurden Probleme und bekannte Einschränkungen verzeichnet, die die Funktionsweise einiger mit EF Core 2.0 entwickelten Anwendungen beeinträchtigen können. Überprüfen Sie die Liste der [aktiven Probleme](https://github.com/aspnet/entityframeworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-xamarin) zur Problemumgehung.

<sup>(4)</sup> Informationen hierzu finden Sie in diesem Artikel im Abschnitt [Universelle Windows-Plattform](#universal-windows-platform).

## <a name="universal-windows-platform"></a>Universelle Windows-Plattform

Bei älteren Versionen der EF Core- und .NET-UWP sind zahlreiche Kompatibilitätsprobleme aufgetreten. Dies galt besonders für Anwendungen, die mit der .NET Native-Toolkette kompiliert wurden. Die neue .NET-UWP-Version bietet Unterstützung für .NET Standard 2.0 und .NET Native 2.0, die einen Großteil der zuvor erwähnten Kompatibilitätsprobleme behebt. EF Core 2.0.1 wurde mit der UWP gründlicher getestet, die Tests sind jedoch nicht automatisiert.

Bei Verwendung von EF Core unter UWP:

* Um die Abfrageleistung zu optimieren, vermeiden Sie anonyme Typen in LINQ-Abfragen. Das Bereitstellen einer UWP-Anwendung im App Store erfordert die Kompilierung einer Anwendung mit .NET Native. Abfragen mit anonymen Typen zeigen in .NET Native eine unzureichende Leistung.

* Zur Optimierung der `SaveChanges()`-Leistung verwenden Sie [ChangeTrackingStrategy.ChangingAndChangedNotifications](/dotnet/api/microsoft.entityframeworkcore.changetrackingstrategy), und implementieren Sie [INotifyPropertyChanged](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanged.aspx), [INotifyPropertyChanging](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanging.aspx) und [INotifyCollectionChanged](https://msdn.microsoft.com/en-us/library/system.collections.specialized.inotifycollectionchanged.aspx) in Ihren Entitätstypen.

## <a name="report-issues"></a>Melden von Problemen

Für alle Kombinationen, die nicht wie erwartet funktioniert, empfehlen wir, neue Probleme in der [EF Core-Problemverfolgung](https://github.com/aspnet/entityframeworkcore/issues/new) zu erstellen. Verwenden Sie bei Xamarin-bezogenen Problemen [Xamarin.Android](https://github.com/xamarin/xamarin-android/issues/new) oder [Xamarin.iOS](https://github.com/xamarin/xamarin-macios/issues/new).
