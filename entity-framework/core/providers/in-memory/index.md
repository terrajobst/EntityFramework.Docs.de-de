---
title: "In-Memory Database-Anbieter – EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
ms.technology: entity-framework-core
uid: core/providers/in-memory/index
ms.openlocfilehash: a8e05f50837f3da554b338475d24215706dfa2ec
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="ef-core-in-memory-database-provider"></a>EF Core-In-Memory Database-Anbieter

Dieser Datenbankanbieter ermöglicht, dass Entity Framework Core mit einer In-Memory Database verwendet wird. Dies ist beim Testen von Code nützlich, der Entity Framework Core verwendet. Der Anbieter wird im Rahmen des [GitHub-Projekts „EntityFramework“](https://github.com/aspnet/EntityFramework) verwaltet.

## <a name="install"></a>Installieren

Installieren Sie das [NuGet-Paket „Microsoft.EntityFrameworkCore.InMemory“](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

## <a name="get-started"></a>Erste Schritte

Die folgenden Ressourcen unterstützen Sie bei den ersten Schritten mit diesem Anbieter:
* [Testen mit InMemory](../../miscellaneous/testing/in-memory.md)

* [Tests der UnicornStore-Beispielanwendung](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a>Unterstützte Datenbank-Engines

* Integrierte In-Memory Database (nur für Testzwecke konzipiert)

## <a name="supported-platforms"></a>Unterstützte Plattformen

* .NET Framework (4.5.1 oder höher)

* .NET Core

* Mono (4.2.0 oder höher)

* Universelle Windows-Plattform
