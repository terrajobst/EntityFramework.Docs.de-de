---
title: "MySQL-Datenbankanbieter – EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 4900b882-79c5-40d2-a44a-ccb0292f6ed9
ms.technology: entity-framework-core
uid: core/providers/mysql/index
ms.openlocfilehash: 1500d017cb463c3f394131a79b9063ff90cce5e2
ms.sourcegitcommit: 6ed04bb05a3d05c367f0f55616807af2bf4037ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="mysql-ef-core-database-provider"></a><span data-ttu-id="d50ec-102">MySQL-EF Core-Datenbankanbieter</span><span class="sxs-lookup"><span data-stu-id="d50ec-102">MySQL EF Core Database Provider</span></span>

<span data-ttu-id="d50ec-103">Dieser Datenbankanbieter ermöglicht die Verwendung von Entity Framework Core mit MySQL.</span><span class="sxs-lookup"><span data-stu-id="d50ec-103">This database provider allows Entity Framework Core to be used with MySQL.</span></span> <span data-ttu-id="d50ec-104">Der Anbieter wird im Rahmen des [MySQL-Projekts](http://dev.mysql.com) verwaltet.</span><span class="sxs-lookup"><span data-stu-id="d50ec-104">The provider is maintained as part of the [MySQL project](http://dev.mysql.com).</span></span>

> [!WARNING]  
> <span data-ttu-id="d50ec-105">Bei diesem Anbieter handelt es sich um ein Vorabrelease.</span><span class="sxs-lookup"><span data-stu-id="d50ec-105">This provider is pre-release.</span></span>

> [!NOTE]  
> <span data-ttu-id="d50ec-106">Dieser Anbieter wird nicht im Rahmen des Entity Framework Core-Projekts verwaltet.</span><span class="sxs-lookup"><span data-stu-id="d50ec-106">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="d50ec-107">Wenn Sie einen Drittanbieter in Betracht ziehen, sollten Sie Aspekte wie Qualität, Lizenzierung und Support auswerten, um sicherzustellen, dass dieser Ihren Anforderungen entspricht.</span><span class="sxs-lookup"><span data-stu-id="d50ec-107">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="d50ec-108">Installieren</span><span class="sxs-lookup"><span data-stu-id="d50ec-108">Install</span></span>

<span data-ttu-id="d50ec-109">Installieren Sie das [NuGet-Paket „MySql.Data.EntityFrameworkCore“](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="d50ec-109">Install the [MySql.Data.EntityFrameworkCore NuGet package](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore).</span></span>

``` powershell
Install-Package MySql.Data.EntityFrameworkCore -Pre
```

## <a name="get-started"></a><span data-ttu-id="d50ec-110">Erste Schritte</span><span class="sxs-lookup"><span data-stu-id="d50ec-110">Get Started</span></span>

<span data-ttu-id="d50ec-111">Weitere Informationen finden Sie unter [Starting with MySQL EF Core provider and Connector/Net 7.0.4](http://insidemysql.com/howto-starting-with-mysql-ef-core-provider-and-connectornet-7-0-4/) (Erste Schritte mit dem MySQL EF Core-Anbieter und Connector/Net 7.0.4).</span><span class="sxs-lookup"><span data-stu-id="d50ec-111">See [Starting with MySQL EF Core provider and Connector/Net 7.0.4](http://insidemysql.com/howto-starting-with-mysql-ef-core-provider-and-connectornet-7-0-4/).</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="d50ec-112">Unterstützte Datenbank-Engines</span><span class="sxs-lookup"><span data-stu-id="d50ec-112">Supported Database Engines</span></span>

* <span data-ttu-id="d50ec-113">MySQL</span><span class="sxs-lookup"><span data-stu-id="d50ec-113">MySQL</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="d50ec-114">Unterstützte Plattformen</span><span class="sxs-lookup"><span data-stu-id="d50ec-114">Supported Platforms</span></span>

* <span data-ttu-id="d50ec-115">.NET Framework (4.5.1 oder höher)</span><span class="sxs-lookup"><span data-stu-id="d50ec-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="d50ec-116">.NET Core</span><span class="sxs-lookup"><span data-stu-id="d50ec-116">.NET Core</span></span>

<span data-ttu-id="d50ec-117">Informieren Sie sich in der MySQL-Dokumentation [hier](https://dev.mysql.com/doc/connector-net/en/connector-net-versions.html) und [hier](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework-core.html) über Informationen zur Versionskompatibilität.</span><span class="sxs-lookup"><span data-stu-id="d50ec-117">Be sure to review the MySQL documentation for version compatibility information [here](https://dev.mysql.com/doc/connector-net/en/connector-net-versions.html) and [here](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework-core.html)</span></span>
