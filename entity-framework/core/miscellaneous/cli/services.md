---
title: Entwurfszeit Dienste-EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/26/2017
uid: core/miscellaneous/cli/services
ms.openlocfilehash: ff0243a588d5e957aed89fcf1ce7462b5b9a747a
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72811938"
---
# <a name="design-time-services"></a>Entwurfszeit Dienste

Einige Dienste, die von den-Tools verwendet werden, werden nur zur Entwurfszeit verwendet. Diese Dienste werden getrennt von den Lauf Zeit Diensten EF Core verwaltet, um zu verhindern, dass Sie mit Ihrer APP bereitgestellt werden. Um einen dieser Dienste (z. b. den-Dienst zum Generieren von Migrations Dateien) zu überschreiben, fügen Sie dem Startprojekt eine Implementierung von `IDesignTimeServices` hinzu.

``` csharp
class MyDesignTimeServices : IDesignTimeServices
{
    public void ConfigureDesignTimeServices(IServiceCollection services)
        => services.AddSingleton<IMigrationsCodeGenerator, MyMigrationsCodeGenerator>()
}
```
