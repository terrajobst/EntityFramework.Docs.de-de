---
title: Entity Framework Core Tools-Referenz – EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/19/2018
uid: core/miscellaneous/cli/index
ms.openlocfilehash: 13e80f740bc5ce3404e8dba40b65ec872c5e3f90
ms.sourcegitcommit: ea1cdec0b982b922a59b9d9301d3ed2b94baca0f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/31/2019
ms.locfileid: "66452255"
---
# <a name="entity-framework-core-tools-reference"></a><span data-ttu-id="3ea01-102">Entity Framework Core Tools-Referenz</span><span class="sxs-lookup"><span data-stu-id="3ea01-102">Entity Framework Core tools reference</span></span>

<span data-ttu-id="3ea01-103">Die Entity Framework Core Tools helfen zur Entwurfszeit bei Entwicklungsaufgaben.</span><span class="sxs-lookup"><span data-stu-id="3ea01-103">The Entity Framework Core tools help with design-time development tasks.</span></span> <span data-ttu-id="3ea01-104">Diese werden hauptsächlich verwendet, um Migrationen zu verwalten und ein Gerüst für `DbContext` und Entitätstypen durch Reverse Engineering eines Datenbankschemas zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="3ea01-104">They're primarily used to manage Migrations and to scaffold a `DbContext` and entity types by reverse engineering the schema of a database.</span></span>

* <span data-ttu-id="3ea01-105">Die [EF Core Paket-Manager-Konsolentools](powershell.md) werden in der [Paket-Manager-Konsole](https://docs.microsoft.com/nuget/tools/package-manager-console) in Visual Studio ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="3ea01-105">The [EF Core Package Manager Console tools](powershell.md) run in the [Package Manager Console](https://docs.microsoft.com/nuget/tools/package-manager-console) in Visual Studio.</span></span> <span data-ttu-id="3ea01-106">Diese Tools funktionieren sowohl mit .NET Framework- als auch mit .NET Core-Projekten.</span><span class="sxs-lookup"><span data-stu-id="3ea01-106">These tools work with both .NET Framework and .NET Core projects.</span></span>

* <span data-ttu-id="3ea01-107">Die [EF Core .NET-Befehlszeilentools (CLI-Tools)](dotnet.md) stellen eine Erweiterung der plattformübergreifenden [.NET Core CLI-Tools](https://docs.microsoft.com/dotnet/core/tools/) dar.</span><span class="sxs-lookup"><span data-stu-id="3ea01-107">The [EF Core .NET command-line interface (CLI) tools](dotnet.md) are an extension to the cross-platform [.NET Core CLI tools](https://docs.microsoft.com/dotnet/core/tools/).</span></span> <span data-ttu-id="3ea01-108">Für diese Tools ist ein .NET Core-SDK-Projekt (mit `Sdk="Microsoft.NET.Sdk"` oder einem ähnlichen SDK in der Projektdatei) erforderlich.</span><span class="sxs-lookup"><span data-stu-id="3ea01-108">These tools require a .NET Core SDK project (one with `Sdk="Microsoft.NET.Sdk"` or similar in the project file).</span></span>

<span data-ttu-id="3ea01-109">Beide Tools bieten dieselbe Funktionalität.</span><span class="sxs-lookup"><span data-stu-id="3ea01-109">Both tools expose the same functionality.</span></span> <span data-ttu-id="3ea01-110">Für die Entwicklung in Visual Studio empfehlen wir die Verwendung von **Paket-Manager-Konsolentools**, da diese ein besseres ganzheitliches Erlebnis bieten.</span><span class="sxs-lookup"><span data-stu-id="3ea01-110">If you're developing in Visual Studio, we recommend using the **Package Manager Console** tools since they provide a more integrated experience.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3ea01-111">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="3ea01-111">Next steps</span></span>

* [<span data-ttu-id="3ea01-112">EF Core-Paket-Manager-Konsolentools – Referenz</span><span class="sxs-lookup"><span data-stu-id="3ea01-112">EF Core Package Manager Console tools reference</span></span>](powershell.md)
* [<span data-ttu-id="3ea01-113">EF Core .NET CLI-Tools – Referenz</span><span class="sxs-lookup"><span data-stu-id="3ea01-113">EF Core .NET CLI tools reference</span></span>](dotnet.md)
