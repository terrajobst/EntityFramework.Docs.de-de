---
title: Microsoft SQL Server-Datenbankanbieter – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/index
ms.openlocfilehash: f0aa290e8c5166c278f8c9782c4304de5e91f26b
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813504"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a>Microsoft SQL Server-EF Core-Datenbankanbieter

Dieser Datenbankanbieter ermöglicht die Verwendung von Entity Framework Core mit Microsoft SQL Server (einschließlich SQL Azure). Dieser Anbieter wird nicht im Rahmen des [Entity Framework Core-Projekts](https://github.com/aspnet/EntityFrameworkCore) verwaltet.

## <a name="install"></a>Installieren

Installieren Sie das [NuGet-Paket „Microsoft.EntityFrameworkCore.SqlServer“](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).

# <a name="net-core-clitabdotnet-core-cli"></a>[.NET Core-CLI](#tab/dotnet-core-cli)

``` console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

# <a name="visual-studiotabvs"></a>[Visual Studio](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

***

## <a name="supported-database-engines"></a>Unterstützte Datenbank-Engines

* Microsoft SQL Server (2012 oder höher)
