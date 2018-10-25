---
title: Anbieterunterstützung für räumliche Typen - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 1097cb00-15f5-453d-90ed-bff9403d23e3
ms.openlocfilehash: 9c00e82c663daec219fe649a8d889afcc81564f7
ms.sourcegitcommit: 5e11125c9b838ce356d673ef5504aec477321724
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50022274"
---
# <a name="provider-support-for-spatial-types"></a><span data-ttu-id="da80c-102">Unterstützung für räumliche Typen</span><span class="sxs-lookup"><span data-stu-id="da80c-102">Provider Support for Spatial Types</span></span>
<span data-ttu-id="da80c-103">Entitätsframework unterstützt die Arbeit mit räumlichen Daten über die DbGeography oder DbGeometry-Klassen.</span><span class="sxs-lookup"><span data-stu-id="da80c-103">Entity Framework supports working with spatial data through the DbGeography or DbGeometry classes.</span></span> <span data-ttu-id="da80c-104">Diese Klassen basieren auf Datenbank-spezifische Funktionen, die die Entity Framework-Dienstanbieter angeboten werden.</span><span class="sxs-lookup"><span data-stu-id="da80c-104">These classes rely on database-specific functionality offered by the Entity Framework provider.</span></span> <span data-ttu-id="da80c-105">Nicht alle Anbieter werden räumliche Daten unterstützt, und diejenigen, die möglicherweise zusätzlicher Voraussetzungen, z. B. die Installation der räumliche Typ Assemblys.</span><span class="sxs-lookup"><span data-stu-id="da80c-105">Not all providers support spatial data and those that do may have additional prerequisites such as the installation of spatial type assemblies.</span></span> <span data-ttu-id="da80c-106">Weitere Informationen über anbieterunterstützung für räumliche Typen finden Sie weiter unten.</span><span class="sxs-lookup"><span data-stu-id="da80c-106">More information about provider support for spatial types is provided below.</span></span>  

<span data-ttu-id="da80c-107">Weitere Informationen zur Verwendung von räumlichen Typen in einer Anwendung finden Sie in beiden exemplarischen Vorgehensweisen, eine für Code First, die andere für Database First oder Model First:</span><span class="sxs-lookup"><span data-stu-id="da80c-107">Additional information on how to use spatial types in an application can be found in two walkthroughs, one for Code First, the other for Database First or Model First:</span></span>  

- [<span data-ttu-id="da80c-108">Räumliche Datentypen im Code zuerst</span><span class="sxs-lookup"><span data-stu-id="da80c-108">Spatial Data Types in Code First</span></span>](~/ef6/modeling/code-first/data-types/spatial.md)  
- [<span data-ttu-id="da80c-109">Typen von räumlichen Daten im EF Designer</span><span class="sxs-lookup"><span data-stu-id="da80c-109">Spatial Data Types in EF Designer</span></span>](~/ef6/modeling/designer/data-types/spatial.md)  

## <a name="ef-releases-that-support-spatial-types"></a><span data-ttu-id="da80c-110">EF-Versionen, die räumliche Typen unterstützen.</span><span class="sxs-lookup"><span data-stu-id="da80c-110">EF releases that support spatial types</span></span>  

<span data-ttu-id="da80c-111">Unterstützung für räumliche Datentypen wurde in EF5 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="da80c-111">Support for spatial types was introduced in EF5.</span></span> <span data-ttu-id="da80c-112">Jedoch werden räumliche Typen in EF5 nur unterstützt, wenn die Anwendung ausgerichtet ist und auf .NET 4.5 ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="da80c-112">However, in EF5 spatial types are only supported when the application targets and runs on .NET 4.5.</span></span>  

<span data-ttu-id="da80c-113">Beginnend mit EF6, die räumliche Typen für Anwendungen, die auf .NET 4 und .NET 4.5 unterstützt werden.</span><span class="sxs-lookup"><span data-stu-id="da80c-113">Starting with EF6 spatial types are supported for applications targeting both .NET 4 and .NET 4.5.</span></span>  

## <a name="ef-providers-that-support-spatial-types"></a><span data-ttu-id="da80c-114">EF-Anbieter, die räumliche Typen zu unterstützen</span><span class="sxs-lookup"><span data-stu-id="da80c-114">EF providers that support spatial types</span></span>  

### <a name="ef5"></a><span data-ttu-id="da80c-115">EF5</span><span class="sxs-lookup"><span data-stu-id="da80c-115">EF5</span></span>  

<span data-ttu-id="da80c-116">Die Entity Framework-Anbieter für EF5, die wir beachten, dass Unterstützung für räumliche Typen sind:</span><span class="sxs-lookup"><span data-stu-id="da80c-116">The Entity Framework providers for EF5 that we are aware of that support spatial types are:</span></span>  

- <span data-ttu-id="da80c-117">Microsoft SQL Server-Anbieter</span><span class="sxs-lookup"><span data-stu-id="da80c-117">Microsoft SQL Server provider</span></span>  
    - <span data-ttu-id="da80c-118">Dieser Anbieter wird als Teil des EF5 ausgeliefert.</span><span class="sxs-lookup"><span data-stu-id="da80c-118">This provider is shipped as part of EF5.</span></span>  
    - <span data-ttu-id="da80c-119">Dieser Anbieter abhängig ist, auf einige zusätzlichen Low-Level-Bibliotheken, die installiert werden müssen, finden Sie weiter unten Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="da80c-119">This provider depends on some additional low-level libraries that may need to be installed—see below for details.</span></span>  
- [<span data-ttu-id="da80c-120">Devart DotConnect für Oracle</span><span class="sxs-lookup"><span data-stu-id="da80c-120">Devart dotConnect for Oracle</span></span>](http://www.devart.com/dotconnect/oracle/)  
    - <span data-ttu-id="da80c-121">Dies ist eine Drittanbieter-Anbieter von Devart.</span><span class="sxs-lookup"><span data-stu-id="da80c-121">This is a third-party provider from Devart.</span></span>  

<span data-ttu-id="da80c-122">Wenn Sie einen Anbieter EF5 kennen, die Sie räumliche Typen unterstützt erhalten, wenden Sie sich an, und wir werden darüber freuen, dass es zu dieser Liste hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="da80c-122">If you know of an EF5 provider that supports spatial types then please get in contact and we will be happy to add it to this list.</span></span>  

### <a name="ef6"></a><span data-ttu-id="da80c-123">EF6</span><span class="sxs-lookup"><span data-stu-id="da80c-123">EF6</span></span>  

<span data-ttu-id="da80c-124">Die Entity Framework-Anbieter für EF6, die wir beachten, dass Unterstützung für räumliche Typen sind:</span><span class="sxs-lookup"><span data-stu-id="da80c-124">The Entity Framework providers for EF6 that we are aware of that support spatial types are:</span></span>  

- <span data-ttu-id="da80c-125">Microsoft SQL Server-Anbieter</span><span class="sxs-lookup"><span data-stu-id="da80c-125">Microsoft SQL Server provider</span></span>  
    - <span data-ttu-id="da80c-126">Dieser Anbieter wird als Teil von EF6 ausgeliefert.</span><span class="sxs-lookup"><span data-stu-id="da80c-126">This provider is shipped as part of EF6.</span></span>  
    - <span data-ttu-id="da80c-127">Dieser Anbieter abhängig ist, auf einige zusätzlichen Low-Level-Bibliotheken, die installiert werden müssen, finden Sie weiter unten Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="da80c-127">This provider depends on some additional low-level libraries that may need to be installed—see below for details.</span></span>  
- [<span data-ttu-id="da80c-128">Devart DotConnect für Oracle</span><span class="sxs-lookup"><span data-stu-id="da80c-128">Devart dotConnect for Oracle</span></span>](http://www.devart.com/dotconnect/oracle/)  
    - <span data-ttu-id="da80c-129">Dies ist eine Drittanbieter-Anbieter von Devart.</span><span class="sxs-lookup"><span data-stu-id="da80c-129">This is a third-party provider from Devart.</span></span>  

<span data-ttu-id="da80c-130">Wenn Sie einen Anbieter für EF 6 kennen, die Sie räumliche Typen unterstützt erhalten, wenden Sie sich an, und wir werden darüber freuen, dass es zu dieser Liste hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="da80c-130">If you know of an EF6 provider that supports spatial types then please get in contact and we will be happy to add it to this list.</span></span>  

## <a name="prerequisites-for-spatial-types-with-microsoft-sql-server"></a><span data-ttu-id="da80c-131">Voraussetzungen für räumliche Typen, die mit Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="da80c-131">Prerequisites for spatial types with Microsoft SQL Server</span></span>  

<span data-ttu-id="da80c-132">Unterstützung von räumlichen Daten SQL Server hängt die Low-Level, SQL Server-spezifische Typen "sqlgeography" und "sqlgeometry" aus.</span><span class="sxs-lookup"><span data-stu-id="da80c-132">SQL Server spatial support depends on the low-level, SQL Server-specific types SqlGeography and SqlGeometry.</span></span> <span data-ttu-id="da80c-133">Diese Typen befinden sich in "Microsoft.SqlServer.Types.dll"-Assembly, und diese Assembly ist nicht als Bestandteil von EF oder als Teil von .NET Framework geliefert.</span><span class="sxs-lookup"><span data-stu-id="da80c-133">These types live in Microsoft.SqlServer.Types.dll assembly, and this assembly is not shipped as part of EF or as part of the .NET Framework.</span></span>  

<span data-ttu-id="da80c-134">Bei der Installation von Visual Studio wird häufig auch eine Version von SQL Server installiert, und dies schließt die Installation von "Microsoft.SqlServer.Types.dll".</span><span class="sxs-lookup"><span data-stu-id="da80c-134">When Visual Studio is installed it will often also install a version of SQL Server, and this will include installation of the Microsoft.SqlServer.Types.dll.</span></span>  

<span data-ttu-id="da80c-135">Wenn SQL Server nicht auf dem Computer installiert ist, in dem Sie räumliche Typen verwenden möchten, oder wenn räumliche Typen aus der SQL Server-Installation ausgeschlossen wurden, müssen Sie sie manuell installieren.</span><span class="sxs-lookup"><span data-stu-id="da80c-135">If SQL Server is not installed on the machine where you want to use spatial types, or if spatial types were excluded from the SQL Server installation, then you will need to install them manually.</span></span> <span data-ttu-id="da80c-136">Die Typen können installiert werden, mithilfe von `SQLSysClrTypes.msi`, diese ist Teil von Microsoft SQL Server Feature Pack.</span><span class="sxs-lookup"><span data-stu-id="da80c-136">The types can be installed using `SQLSysClrTypes.msi`, which is part of Microsoft SQL Server Feature Pack.</span></span> <span data-ttu-id="da80c-137">Räumliche Typen sind SQL Server-Version-spezifisch ist, daher wir empfehlen [suchen Sie nach "SQL Server Feature Pack"](https://www.microsoft.com/search/result.aspx?q=sql+server+feature+pack) im Microsoft Download Center, wählen Sie dann und Laden Sie auf die Version von SQL Server, die Sie verwenden die Option, die entspricht.</span><span class="sxs-lookup"><span data-stu-id="da80c-137">Spatial types are SQL Server version-specific, so we recommend [search for "SQL Server Feature Pack"](https://www.microsoft.com/search/result.aspx?q=sql+server+feature+pack) in the Microsoft Download Center, then select and download the option that corresponds to the version of SQL Server you will use.</span></span>
