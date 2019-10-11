---
title: Anbieter Unterstützung für räumliche Typen EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 1097cb00-15f5-453d-90ed-bff9403d23e3
ms.openlocfilehash: 863f1b4551bd62160915eba90fee7ba6c49c169c
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181593"
---
# <a name="provider-support-for-spatial-types"></a><span data-ttu-id="556cb-102">Anbieter Unterstützung für räumliche Typen</span><span class="sxs-lookup"><span data-stu-id="556cb-102">Provider Support for Spatial Types</span></span>
<span data-ttu-id="556cb-103">Entity Framework unterstützt das Arbeiten mit räumlichen Daten durch die dbgeography-Klasse oder die dbgeometry-Klasse.</span><span class="sxs-lookup"><span data-stu-id="556cb-103">Entity Framework supports working with spatial data through the DbGeography or DbGeometry classes.</span></span> <span data-ttu-id="556cb-104">Diese Klassen basieren auf datenbankspezifischen Funktionen, die der Entity Framework Anbieter bietet.</span><span class="sxs-lookup"><span data-stu-id="556cb-104">These classes rely on database-specific functionality offered by the Entity Framework provider.</span></span> <span data-ttu-id="556cb-105">Nicht alle Anbieter unterstützen räumliche Daten, und solche, die möglicherweise zusätzliche Voraussetzungen aufweisen, wie z. b. die Installation von Assemblys für räumliche</span><span class="sxs-lookup"><span data-stu-id="556cb-105">Not all providers support spatial data and those that do may have additional prerequisites such as the installation of spatial type assemblies.</span></span> <span data-ttu-id="556cb-106">Weitere Informationen zur Anbieter Unterstützung für räumliche Typen finden Sie weiter unten.</span><span class="sxs-lookup"><span data-stu-id="556cb-106">More information about provider support for spatial types is provided below.</span></span>  

<span data-ttu-id="556cb-107">Weitere Informationen zur Verwendung räumlicher Typen in einer Anwendung finden Sie in zwei exemplarischen Vorgehensweisen, einer für Code First, der andere für Database First oder Model First:</span><span class="sxs-lookup"><span data-stu-id="556cb-107">Additional information on how to use spatial types in an application can be found in two walkthroughs, one for Code First, the other for Database First or Model First:</span></span>  

- [<span data-ttu-id="556cb-108">Räumliche Datentypen in Code First</span><span class="sxs-lookup"><span data-stu-id="556cb-108">Spatial Data Types in Code First</span></span>](~/ef6/modeling/code-first/data-types/spatial.md)  
- [<span data-ttu-id="556cb-109">Räumliche Datentypen im EF-Designer</span><span class="sxs-lookup"><span data-stu-id="556cb-109">Spatial Data Types in EF Designer</span></span>](~/ef6/modeling/designer/data-types/spatial.md)  

## <a name="ef-releases-that-support-spatial-types"></a><span data-ttu-id="556cb-110">EF-Releases, die räumliche Typen unterstützen</span><span class="sxs-lookup"><span data-stu-id="556cb-110">EF releases that support spatial types</span></span>  

<span data-ttu-id="556cb-111">Unterstützung für räumliche Typen wurde in EF5 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="556cb-111">Support for spatial types was introduced in EF5.</span></span> <span data-ttu-id="556cb-112">In EF5 werden räumliche Typen jedoch nur unterstützt, wenn die Anwendung auf .NET 4,5 abzielt und ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="556cb-112">However, in EF5 spatial types are only supported when the application targets and runs on .NET 4.5.</span></span>  

<span data-ttu-id="556cb-113">Beginnend mit EF6 räumliche Typen werden für Anwendungen unterstützt, die auf .NET 4 und .NET 4,5 abzielen.</span><span class="sxs-lookup"><span data-stu-id="556cb-113">Starting with EF6 spatial types are supported for applications targeting both .NET 4 and .NET 4.5.</span></span>  

## <a name="ef-providers-that-support-spatial-types"></a><span data-ttu-id="556cb-114">EF-Anbieter, die räumliche Typen unterstützen</span><span class="sxs-lookup"><span data-stu-id="556cb-114">EF providers that support spatial types</span></span>  

### <a name="ef5"></a><span data-ttu-id="556cb-115">EF5</span><span class="sxs-lookup"><span data-stu-id="556cb-115">EF5</span></span>  

<span data-ttu-id="556cb-116">Die Entity Framework Anbieter für EF5, die wir uns bewusst sind und räumliche Typen unterstützen, sind:</span><span class="sxs-lookup"><span data-stu-id="556cb-116">The Entity Framework providers for EF5 that we are aware of that support spatial types are:</span></span>  

- <span data-ttu-id="556cb-117">Microsoft SQL Server Anbieter</span><span class="sxs-lookup"><span data-stu-id="556cb-117">Microsoft SQL Server provider</span></span>  
    - <span data-ttu-id="556cb-118">Dieser Anbieter wird als Teil von EF5 ausgeliefert.</span><span class="sxs-lookup"><span data-stu-id="556cb-118">This provider is shipped as part of EF5.</span></span>  
    - <span data-ttu-id="556cb-119">Dieser Anbieter hängt von einigen zusätzlichen, auf Low-Level-Bibliotheken ab, die möglicherweise installiert werden müssen – weitere Informationen finden Sie weiter unten.</span><span class="sxs-lookup"><span data-stu-id="556cb-119">This provider depends on some additional low-level libraries that may need to be installed—see below for details.</span></span>  
- [<span data-ttu-id="556cb-120">Devart dotConnect für Oracle</span><span class="sxs-lookup"><span data-stu-id="556cb-120">Devart dotConnect for Oracle</span></span>](https://www.devart.com/dotconnect/oracle/)  
    - <span data-ttu-id="556cb-121">Dabei handelt es sich um einen Drittanbieter von Devart.</span><span class="sxs-lookup"><span data-stu-id="556cb-121">This is a third-party provider from Devart.</span></span>  

<span data-ttu-id="556cb-122">Wenn Sie von einem EF5-Anbieter wissen, der räumliche Typen unterstützt, wenden Sie sich bitte an den Kontakt, und wir freuen uns, ihn dieser Liste hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="556cb-122">If you know of an EF5 provider that supports spatial types then please get in contact and we will be happy to add it to this list.</span></span>  

### <a name="ef6"></a><span data-ttu-id="556cb-123">EF6</span><span class="sxs-lookup"><span data-stu-id="556cb-123">EF6</span></span>  

<span data-ttu-id="556cb-124">Die Entity Framework Anbieter für EF6, die wir uns bewusst sind und räumliche Typen unterstützen, sind:</span><span class="sxs-lookup"><span data-stu-id="556cb-124">The Entity Framework providers for EF6 that we are aware of that support spatial types are:</span></span>  

- <span data-ttu-id="556cb-125">Microsoft SQL Server Anbieter</span><span class="sxs-lookup"><span data-stu-id="556cb-125">Microsoft SQL Server provider</span></span>  
    - <span data-ttu-id="556cb-126">Dieser Anbieter wird als Teil von EF6 ausgeliefert.</span><span class="sxs-lookup"><span data-stu-id="556cb-126">This provider is shipped as part of EF6.</span></span>  
    - <span data-ttu-id="556cb-127">Dieser Anbieter hängt von einigen zusätzlichen, auf Low-Level-Bibliotheken ab, die möglicherweise installiert werden müssen – weitere Informationen finden Sie weiter unten.</span><span class="sxs-lookup"><span data-stu-id="556cb-127">This provider depends on some additional low-level libraries that may need to be installed—see below for details.</span></span>  
- [<span data-ttu-id="556cb-128">Devart dotConnect für Oracle</span><span class="sxs-lookup"><span data-stu-id="556cb-128">Devart dotConnect for Oracle</span></span>](https://www.devart.com/dotconnect/oracle/)  
    - <span data-ttu-id="556cb-129">Dabei handelt es sich um einen Drittanbieter von Devart.</span><span class="sxs-lookup"><span data-stu-id="556cb-129">This is a third-party provider from Devart.</span></span>  

<span data-ttu-id="556cb-130">Wenn Sie von einem EF6-Anbieter wissen, der räumliche Typen unterstützt, wenden Sie sich bitte an den Kontakt, und wir freuen uns, ihn dieser Liste hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="556cb-130">If you know of an EF6 provider that supports spatial types then please get in contact and we will be happy to add it to this list.</span></span>  

## <a name="prerequisites-for-spatial-types-with-microsoft-sql-server"></a><span data-ttu-id="556cb-131">Voraussetzungen für räumliche Typen mit Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="556cb-131">Prerequisites for spatial types with Microsoft SQL Server</span></span>  

<span data-ttu-id="556cb-132">SQL Server räumliche Unterstützung hängt von den SQL Server spezifischen Typen "SQLGeography" und "SQLGeometry" auf niedriger Ebene ab.</span><span class="sxs-lookup"><span data-stu-id="556cb-132">SQL Server spatial support depends on the low-level, SQL Server-specific types SqlGeography and SqlGeometry.</span></span> <span data-ttu-id="556cb-133">Diese Typen befinden sich in der Microsoft. SqlServer. types. dll-Assembly, und diese Assembly wird nicht als Teil von EF oder als Teil der .NET Framework ausgeliefert.</span><span class="sxs-lookup"><span data-stu-id="556cb-133">These types live in Microsoft.SqlServer.Types.dll assembly, and this assembly is not shipped as part of EF or as part of the .NET Framework.</span></span>  

<span data-ttu-id="556cb-134">Wenn Visual Studio installiert ist, wird häufig auch eine Version von SQL Server installiert. dazu gehört auch die Installation von Microsoft. SqlServer. types. dll.</span><span class="sxs-lookup"><span data-stu-id="556cb-134">When Visual Studio is installed it will often also install a version of SQL Server, and this will include installation of the Microsoft.SqlServer.Types.dll.</span></span>  

<span data-ttu-id="556cb-135">Wenn SQL Server nicht auf dem Computer installiert ist, auf dem Sie räumliche Typen verwenden möchten, oder wenn räumliche Typen aus der SQL Server Installation ausgeschlossen wurden, müssen Sie diese manuell installieren.</span><span class="sxs-lookup"><span data-stu-id="556cb-135">If SQL Server is not installed on the machine where you want to use spatial types, or if spatial types were excluded from the SQL Server installation, then you will need to install them manually.</span></span> <span data-ttu-id="556cb-136">Die Typen können mit `SQLSysClrTypes.msi` installiert werden, das Teil von Microsoft SQL Server Feature Pack ist.</span><span class="sxs-lookup"><span data-stu-id="556cb-136">The types can be installed using `SQLSysClrTypes.msi`, which is part of Microsoft SQL Server Feature Pack.</span></span> <span data-ttu-id="556cb-137">Räumliche Typen sind SQL Server Versions spezifisch. Daher wird empfohlen, im Microsoft Download Center [nach "SQL Server Feature Pack" zu suchen](https://www.microsoft.com/search/result.aspx?q=sql+server+feature+pack) und dann die Option auszuwählen, die der Version von SQL Server entspricht, die Sie verwenden möchten.</span><span class="sxs-lookup"><span data-stu-id="556cb-137">Spatial types are SQL Server version-specific, so we recommend [search for "SQL Server Feature Pack"](https://www.microsoft.com/search/result.aspx?q=sql+server+feature+pack) in the Microsoft Download Center, then select and download the option that corresponds to the version of SQL Server you will use.</span></span>
