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
<a name="design-time-services"></a>Entwurfszeitdienste
====================
Einige Dienste, die von den Tools verwendet werden, werden nur zur Entwurfszeit verwendet. Diese Dienste werden in EF Core Runtime-Services, um zu verhindern, dass Sie mit der app bereitgestellt werden separat verwaltet. Um einen dieser Dienste (z. B. der Dienst zum Generieren der Migrations-Dateien) zu überschreiben, fügen Sie eine Implementierung von `IDesignTimeServices` an Ihr Startprojekt.

``` csharp
class MyDesignTimeServices : IDesignTimeServices
{
    public void ConfigureDesignTimeServices(IServiceCollection services)
        => services.AddSingleton<IMigrationsCodeGenerator, MyMigrationsCodeGenerator>()
}
```
