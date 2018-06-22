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
ms.locfileid: "26053690"
---
<a name="design-time-services"></a>Entwurfszeitdienste
====================
Einige Dienste, die von den Tools verwendet werden, werden nur zur Entwurfszeit verwendet. Diese Dienste werden von EF-Core-Laufzeitdienste, um zu verhindern, dass sie mit der app bereitgestellt wird getrennt verwaltet. Um einem dieser Dienste (z. B. der Dienst zum Generieren von Migrationsdateien) zu überschreiben, Hinzufügen einer Implementierung der `IDesignTimeServices` an Ihr Startprojekt.

``` csharp
class MyDesignTimeServices : IDesignTimeServices
{
    public void ConfigureDesignTimeServices(IServiceCollection services)
        => services.AddSingleton<IMigrationsCodeGenerator, MyMigrationsCodeGenerator>()
}
```
