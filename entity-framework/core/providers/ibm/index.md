---
title: "IBM Data Server-Datenbankanbieter (DB2) – EF Core"
author: rowanmiller
ms.author: divega
ms.date: 02/15/2017
ms.assetid: 825e5332-5aa3-4600-9efb-ab71aaff59ec
ms.technology: entity-framework-core
uid: core/providers/ibm/index
ms.openlocfilehash: a9caa8df63850d4f6b5f2164dad7ac5af7504076
ms.sourcegitcommit: 6ed04bb05a3d05c367f0f55616807af2bf4037ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="ibm-data-server-db2-ef-core-database-providers"></a>IBM Data Server-EF Core-Datenbankanbieter (DB2)

Dieser Datenbankanbieter ermöglicht die Verwendung von Entity Framework Core mit IBM Data Server. Der Anbieter wird von IBM verwaltet.

> [!NOTE]  
> Dieser Anbieter wird nicht im Rahmen des Entity Framework Core-Projekts verwaltet. Wenn Sie einen Drittanbieter in Betracht ziehen, sollten Sie Aspekte wie Qualität, Lizenzierung und Support auswerten, um sicherzustellen, dass dieser Ihren Anforderungen entspricht.

## <a name="install"></a>Installieren

Um mit IBM Data Server unter Windows arbeiten zu können, installieren Sie das [NuGet-Paket „IBM.EntityFrameworkCore“](https://www.nuget.org/packages/IBM.EntityFrameworkCore).

``` powershell
Install-Package IBM.EntityFrameworkCore
```

Um mit IBM Data Server unter Linux arbeiten zu können, installieren Sie das [NuGet-Paket „IBM.EntityFrameworkCore-lnx“](https://www.nuget.org/packages/IBM.EntityFrameworkCore-lnx).

``` powershell
Install-Package IBM.EntityFrameworkCore-lnx
```

## <a name="get-started"></a>Erste Schritte

[Getting started with IBM .NET Provider for .NET Core (Erste Schritte mit dem IBM .NET-Anbieter für .NET Core)](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/DB2DotnetCore?lang=en)

## <a name="supported-database-engines"></a>Unterstützte Datenbank-Engines

* zOS
* LUW, einschließlich IBM dashDB
* IBM I
* Informix

## <a name="supported-platforms"></a>Unterstützte Plattformen

* .NET Framework (4.6 oder höher)
* .NET Core
