---
title: Microsoft SQL Server-Datenbankanbieter – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/index
ms.openlocfilehash: 1e75bc4bf334b1a60d13a2ec9ef314e3afcf0273
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812100"
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

> [!NOTE]
> Seit Version 3.0.0 verweist der Anbieter auf Microsoft.Data.SqlClient (vorherige Versionen waren von System.Data.SqlClient abhängig). Wenn Ihr Projekt direkt von SqlClient abhängt, stellen Sie sicher, dass es auf das richtige Paket verweist.

## <a name="supported-database-engines"></a>Unterstützte Datenbank-Engines

* Microsoft SQL Server (2012 oder höher)
