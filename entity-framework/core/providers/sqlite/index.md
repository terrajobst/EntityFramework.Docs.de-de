---
title: "SQLite-Datenbankanbieter – EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
ms.technology: entity-framework-core
uid: core/providers/sqlite/index
ms.openlocfilehash: 0f3905a491fb5f7a657ce9037d166771e1f326d8
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="sqlite-ef-core-database-provider"></a>SQLite-EF Core-Datenbankanbieter

Dieser Datenbankanbieter ermöglicht die Verwendung von Entity Framework Core mit SQLite. Der Anbieter wird im Rahmen des [GitHub-Projekts „EntityFramework“](https://github.com/aspnet/EntityFramework) verwaltet.

## <a name="install"></a>Installieren

Installieren Sie das [NuGet-Paket „Microsoft.EntityFrameworkCore.Sqlite“](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

## <a name="get-started"></a>Erste Schritte

Die folgenden Ressourcen unterstützen Sie bei den ersten Schritten mit diesem Anbieter:
* [Lokaler SQLite-Anbieter in UWP](../../get-started/uwp/getting-started.md)

* [.NET Core-Anwendung für die neue SQLite-Datenbank](../../get-started/netcore/new-db-sqlite.md)

* [Unicorn Clicker-Beispielanwendung](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornClicker/UWP)

* [Unicorn Packer-Beispielanwendung](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornPacker)

## <a name="supported-database-engines"></a>Unterstützte Datenbank-Engines

* SQLite (3.7 oder höher)

## <a name="supported-platforms"></a>Unterstützte Plattformen

* .NET Framework (4.5.1 oder höher)

* .NET Core

* Mono (4.2.0 oder höher)

* Universelle Windows-Plattform

## <a name="limitations"></a>Einschränkungen

Einige wichtige Einschränkungen zum SQLite-Anbieter finden Sie unter [Einschränkungen zu SQLite](limitations.md).
