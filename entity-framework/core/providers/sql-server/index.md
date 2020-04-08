---
title: Microsoft SQL Server-Datenbankanbieter – EF Core
description: Dokumentation für den Datenbankanbieter, mit der Entity Framework Core mit Microsoft SQL Server verwendet werden kann
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/sql-server/index
ms.openlocfilehash: baae668a7ec255e35ab0e23e5c5ddfa47bda917e
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/07/2020
ms.locfileid: "78413145"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a>Microsoft SQL Server-EF Core-Datenbankanbieter

Dieser Datenbankanbieter ermöglicht die Verwendung von Entity Framework Core mit Microsoft SQL Server (einschließlich Azure SQL-Datenbank). Dieser Anbieter wird nicht im Rahmen des [Entity Framework Core-Projekts](https://github.com/aspnet/EntityFrameworkCore) verwaltet.

## <a name="install"></a>Installieren

Installieren Sie das [NuGet-Paket „Microsoft.EntityFrameworkCore.SqlServer“](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).

### <a name="net-core-cli"></a>[.NET Core-CLI](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

### <a name="visual-studio"></a>[Visual Studio](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

***

> [!NOTE]
> Seit Version 3.0.0 verweist der Anbieter auf Microsoft.Data.SqlClient (vorherige Versionen waren von System.Data.SqlClient abhängig). Wenn Ihr Projekt direkt von SqlClient abhängt, stellen Sie sicher, dass es auf das Microsoft.Data.SqlClient-Paket verweist.

## <a name="supported-database-engines"></a>Unterstützte Datenbank-Engines

* Microsoft SQL Server (2012 oder höher)
