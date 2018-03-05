---
title: "In-Memory Database-Anbieter – EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
ms.technology: entity-framework-core
uid: core/providers/in-memory/index
ms.openlocfilehash: 356af9390a8aafa5afe35f333cd1e6ac1988390d
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="ef-core-in-memory-database-provider"></a>EF Core-In-Memory Database-Anbieter

Dieser Datenbankanbieter ermöglicht, dass Entity Framework Core mit einer In-Memory Database verwendet wird. Dies kann für Tests nützlich sein, obwohl der SQLite-Anbieter im In-Memory-Modus für Testersatz für relationale Datenbanken besser geeignet sein kann. Dieser Anbieter wird nicht im Rahmen des [Entity Framework Core-Projekts](https://github.com/aspnet/EntityFrameworkCore) verwaltet.

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
