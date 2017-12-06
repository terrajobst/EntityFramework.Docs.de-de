---
title: Entwurfszeitdienste - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/26/2017
ms.technology: entity-framework-core
ms.openlocfilehash: f9c8208a59bfcefeaab01ea69e65fe809a0b3d89
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
<a name="design-time-services"></a><span data-ttu-id="d6bff-102">Entwurfszeitdienste</span><span class="sxs-lookup"><span data-stu-id="d6bff-102">Design-time services</span></span>
====================
<span data-ttu-id="d6bff-103">Einige Dienste, die von den Tools verwendet werden, werden nur zur Entwurfszeit verwendet.</span><span class="sxs-lookup"><span data-stu-id="d6bff-103">Some services used by the tools are only used at design time.</span></span> <span data-ttu-id="d6bff-104">Diese Dienste werden von EF-Core-Laufzeitdienste, um zu verhindern, dass sie mit der app bereitgestellt wird getrennt verwaltet.</span><span class="sxs-lookup"><span data-stu-id="d6bff-104">These services are managed separately from EF Core's runtime services to prevent them from being deployed with your app.</span></span> <span data-ttu-id="d6bff-105">Um einem dieser Dienste (z. B. der Dienst zum Generieren von Migrationsdateien) zu überschreiben, Hinzufügen einer Implementierung der `IDesignTimeServices` an Ihr Startprojekt.</span><span class="sxs-lookup"><span data-stu-id="d6bff-105">To override one of these services (for example the service to generate migration files), add an implementation of `IDesignTimeServices` to your startup project.</span></span>

``` csharp
class MyDesignTimeServices : IDesignTimeServices
{
    public void ConfigureDesignTimeServices(IServiceCollection services)
        => services.AddSingleton<IMigrationsCodeGenerator, MyMigrationsCodeGenerator>()
}
```
