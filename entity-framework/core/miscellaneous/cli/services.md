---
title: Während der Entwurfszeit Services – EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/26/2017
ms.openlocfilehash: e1cacdd4f40f9c395d8c88a91df4a92ef27001a8
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997530"
---
<a name="design-time-services"></a><span data-ttu-id="3c6e7-102">Entwurfszeitdienste</span><span class="sxs-lookup"><span data-stu-id="3c6e7-102">Design-time services</span></span>
====================
<span data-ttu-id="3c6e7-103">Einige Dienste, die von den Tools verwendet werden, werden nur zur Entwurfszeit verwendet.</span><span class="sxs-lookup"><span data-stu-id="3c6e7-103">Some services used by the tools are only used at design time.</span></span> <span data-ttu-id="3c6e7-104">Diese Dienste werden in EF Core Runtime-Services, um zu verhindern, dass Sie mit der app bereitgestellt werden separat verwaltet.</span><span class="sxs-lookup"><span data-stu-id="3c6e7-104">These services are managed separately from EF Core's runtime services to prevent them from being deployed with your app.</span></span> <span data-ttu-id="3c6e7-105">Um einen dieser Dienste (z. B. der Dienst zum Generieren der Migrations-Dateien) zu überschreiben, fügen Sie eine Implementierung von `IDesignTimeServices` an Ihr Startprojekt.</span><span class="sxs-lookup"><span data-stu-id="3c6e7-105">To override one of these services (for example the service to generate migration files), add an implementation of `IDesignTimeServices` to your startup project.</span></span>

``` csharp
class MyDesignTimeServices : IDesignTimeServices
{
    public void ConfigureDesignTimeServices(IServiceCollection services)
        => services.AddSingleton<IMigrationsCodeGenerator, MyMigrationsCodeGenerator>()
}
```
