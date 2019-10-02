---
title: SQLite-Datenbankanbieter – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
uid: core/providers/sqlite/index
ms.openlocfilehash: 0f8b0044e11ba665b610ac583bf3a79ed7174630
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813477"
---
# <a name="sqlite-ef-core-database-provider"></a>SQLite-EF Core-Datenbankanbieter

Dieser Datenbankanbieter ermöglicht die Verwendung von Entity Framework Core mit SQLite. Dieser Anbieter wird nicht im Rahmen des [Entity Framework Core-Projekts](https://github.com/aspnet/EntityFrameworkCore) verwaltet.

## <a name="install"></a>Installieren

Installieren Sie das [NuGet-Paket „Microsoft.EntityFrameworkCore.Sqlite“](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).

# <a name="net-core-clitabdotnet-core-cli"></a>[.NET Core-CLI](#tab/dotnet-core-cli)

``` console
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
```

# <a name="visual-studiotabvs"></a>[Visual Studio](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

***

## <a name="supported-database-engines"></a>Unterstützte Datenbank-Engines

* SQLite (3.7 oder höher)

## <a name="limitations"></a>Einschränkungen

Einige wichtige Einschränkungen zum SQLite-Anbieter finden Sie unter [Einschränkungen zu SQLite](limitations.md).
