---
title: Entwurfszeit Dienste-EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/26/2017
uid: core/miscellaneous/cli/services
ms.openlocfilehash: 57294ab41e7c251b1dafae9d573aa98676c5d939
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414206"
---
# <a name="design-time-services"></a><span data-ttu-id="1c337-102">Entwurfszeit Dienste</span><span class="sxs-lookup"><span data-stu-id="1c337-102">Design-time services</span></span>

<span data-ttu-id="1c337-103">Einige Dienste, die von den-Tools verwendet werden, werden nur zur Entwurfszeit verwendet.</span><span class="sxs-lookup"><span data-stu-id="1c337-103">Some services used by the tools are only used at design time.</span></span> <span data-ttu-id="1c337-104">Diese Dienste werden getrennt von den Lauf Zeit Diensten EF Core verwaltet, um zu verhindern, dass Sie mit Ihrer APP bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="1c337-104">These services are managed separately from EF Core's runtime services to prevent them from being deployed with your app.</span></span> <span data-ttu-id="1c337-105">Um einen dieser Dienste (z. b. den-Dienst zum Generieren von Migrations Dateien) zu überschreiben, fügen Sie dem Startprojekt eine Implementierung von `IDesignTimeServices` hinzu.</span><span class="sxs-lookup"><span data-stu-id="1c337-105">To override one of these services (for example the service to generate migration files), add an implementation of `IDesignTimeServices` to your startup project.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/CommandLine/DesignTimeServices.cs)]
