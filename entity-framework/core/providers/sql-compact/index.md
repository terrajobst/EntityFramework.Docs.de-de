---
title: "Microsoft SQL Server Compact Edition-Datenbankanbieter – EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 073f0004-3eb5-4618-ab93-0674910e1819
ms.technology: entity-framework-core
uid: core/providers/sql-compact/index
ms.openlocfilehash: d8b73621bdd363efec5bb7728886e0a0f6bdcf76
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="microsoft-sql-server-compact-edition-ef-core-database-provider"></a>Microsoft SQL Server Compact Edition-EF Core-Datenbankanbieter

Dieser Datenbankanbieter ermöglicht die Verwendung von Entity Framework Core mit SQL Server Compact Edition. Der Anbieter wird im Rahmen des [GitHub-Projekts „ErikEJ/EntityFramework.SqlServerCompact“](https://github.com/ErikEJ/EntityFramework.SqlServerCompact) verwaltet.

> [!NOTE]  
> Dieser Anbieter wird nicht im Rahmen des Entity Framework Core-Projekts verwaltet. Wenn Sie einen Drittanbieter in Betracht ziehen, sollten Sie Aspekte wie Qualität, Lizenzierung und Support auswerten, um sicherzustellen, dass dieser Ihren Anforderungen entspricht.

## <a name="install"></a>Installieren

Installieren Sie zum Arbeiten mit SQL Server Compact Edition 4.0 das [NuGet-Paket „EntityFrameworkCore.SqlServerCompact40“](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact40).

``` powershell
Install-Package EntityFrameworkCore.SqlServerCompact40
```

Installieren Sie zum Arbeiten mit SQL Server Compact Edition 3.5 das Paket [EntityFrameworkCore.SqlServerCompact35](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact35).

``` powershell
Install-Package EntityFrameworkCore.SqlServerCompact35
```

## <a name="get-started"></a>Erste Schritte

Weitere Informationen finden Sie in der [Dokumentation zu den ersten Schritten auf der Projektwebsite](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications).

## <a name="supported-database-engines"></a>Unterstützte Datenbank-Engines

* SQL Server Compact Edition 3.5

* SQL Server Compact Edition 4.0

## <a name="supported-platforms"></a>Unterstützte Plattformen

* .NET Framework (4.5.1 oder höher)
