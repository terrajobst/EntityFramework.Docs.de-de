---
title: Microsoft SQL Server-Datenbankanbieter – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/index
ms.openlocfilehash: a524794a61a9f5078998aea04b45c31c19357f2b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995668"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a><span data-ttu-id="15b59-102">Microsoft SQL Server-EF Core-Datenbankanbieter</span><span class="sxs-lookup"><span data-stu-id="15b59-102">Microsoft SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="15b59-103">Dieser Datenbankanbieter ermöglicht die Verwendung von Entity Framework Core mit Microsoft SQL Server (einschließlich SQL Azure).</span><span class="sxs-lookup"><span data-stu-id="15b59-103">This database provider allows Entity Framework Core to be used with Microsoft SQL Server (including SQL Azure).</span></span> <span data-ttu-id="15b59-104">Dieser Anbieter wird nicht im Rahmen des [Entity Framework Core-Projekts](https://github.com/aspnet/EntityFrameworkCore) verwaltet.</span><span class="sxs-lookup"><span data-stu-id="15b59-104">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="15b59-105">Installieren</span><span class="sxs-lookup"><span data-stu-id="15b59-105">Install</span></span>

<span data-ttu-id="15b59-106">Installieren Sie das [NuGet-Paket „Microsoft.EntityFrameworkCore.SqlServer“](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="15b59-106">Install the [Microsoft.EntityFrameworkCore.SqlServer NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="get-started"></a><span data-ttu-id="15b59-107">Erste Schritte</span><span class="sxs-lookup"><span data-stu-id="15b59-107">Get Started</span></span>

<span data-ttu-id="15b59-108">Die folgenden Ressourcen unterstützen Sie bei den ersten Schritten mit diesem Anbieter:</span><span class="sxs-lookup"><span data-stu-id="15b59-108">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="15b59-109">Erste Schritte mit dem .NET Framework (Konsole, Windows Forms, WPF etc.)</span><span class="sxs-lookup"><span data-stu-id="15b59-109">Getting Started on .NET Framework (Console, WinForms, WPF, etc.)</span></span>](../../get-started/full-dotnet/index.md)

* [<span data-ttu-id="15b59-110">Erste Schritte in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="15b59-110">Getting Started on ASP.NET Core</span></span>](../../get-started/aspnetcore/index.md)

* [<span data-ttu-id="15b59-111">UnicornStore-Beispielanwendung</span><span class="sxs-lookup"><span data-stu-id="15b59-111">UnicornStore Sample Application</span></span>](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornStore)

## <a name="supported-database-engines"></a><span data-ttu-id="15b59-112">Unterstützte Datenbank-Engines</span><span class="sxs-lookup"><span data-stu-id="15b59-112">Supported Database Engines</span></span>

* <span data-ttu-id="15b59-113">Microsoft SQL Server (2008 oder höher)</span><span class="sxs-lookup"><span data-stu-id="15b59-113">Microsoft SQL Server (2008 onwards)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="15b59-114">Unterstützte Plattformen</span><span class="sxs-lookup"><span data-stu-id="15b59-114">Supported Platforms</span></span>

* <span data-ttu-id="15b59-115">.NET Framework (4.5.1 oder höher)</span><span class="sxs-lookup"><span data-stu-id="15b59-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="15b59-116">.NET Core</span><span class="sxs-lookup"><span data-stu-id="15b59-116">.NET Core</span></span>

* <span data-ttu-id="15b59-117">Mono (4.2.0 oder höher)</span><span class="sxs-lookup"><span data-stu-id="15b59-117">Mono (4.2.0 onwards)</span></span>

      Caution: Using this provider on Mono will make use of the Mono SQL Client implementation, which has a number of known issues. For example, it does not support secure connections (SSL).
