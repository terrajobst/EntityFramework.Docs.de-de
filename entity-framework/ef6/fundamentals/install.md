---
title: Abrufen von Entitätsframework – EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 122c38a2-f9e8-4ecc-9c72-a83bc9af7814
ms.openlocfilehash: 78ef1e7b20bd879c972870552c8f692e153b7abb
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996564"
---
# <a name="get-entity-framework"></a><span data-ttu-id="78b6b-102">Beziehen von Entitätsframework</span><span class="sxs-lookup"><span data-stu-id="78b6b-102">Get Entity Framework</span></span>
<span data-ttu-id="78b6b-103">Entitätsframework die EF-Tools für Visual Studio und die EF-Laufzeit besteht.</span><span class="sxs-lookup"><span data-stu-id="78b6b-103">Entity Framework is made up of the EF Tools for Visual Studio and the EF Runtime.</span></span>

## <a name="ef-tools-for-visual-studio"></a><span data-ttu-id="78b6b-104">EF-Tools für Visual Studio</span><span class="sxs-lookup"><span data-stu-id="78b6b-104">EF Tools for Visual Studio</span></span>

<span data-ttu-id="78b6b-105">Die Entity Framework-Tools für Visual Studio umfassen die EF-Designers und des EF-Model-Assistenten und zuerst für die Datenbank erforderlich sind und ersten Workflows zu modellieren.</span><span class="sxs-lookup"><span data-stu-id="78b6b-105">The Entity Framework Tools for Visual Studio include the EF Designer and the EF Model Wizard and are required for the database first and model first workflows.</span></span> <span data-ttu-id="78b6b-106">EF-Tools sind in allen jüngeren Versionen von Visual Studio enthalten.</span><span class="sxs-lookup"><span data-stu-id="78b6b-106">EF Tools are included in all recent versions of Visual Studio.</span></span> <span data-ttu-id="78b6b-107">Wenn Sie eine benutzerdefinierte Installation von Visual Studio ausführen, die Sie sicherstellen, dass das Element müssen, ist "Entity Framework 6-Tools" ausgewählt, entweder eine arbeitsauslastung, die es enthält oder indem Sie sie als einzelne Komponente auswählen.</span><span class="sxs-lookup"><span data-stu-id="78b6b-107">If you perform a custom install of Visual Studio you will need to ensure that the item "Entity Framework 6 Tools" is selected by either choosing a workload that includes it or by selecting it as an individual component.</span></span>

<span data-ttu-id="78b6b-108">Für einige frühere Versionen von Visual Studio sind aktualisierte EF-Tools als Download verfügbar.</span><span class="sxs-lookup"><span data-stu-id="78b6b-108">For some past versions of Visual Studio, updated EF Tools are available as a download.</span></span> <span data-ttu-id="78b6b-109">Finden Sie unter [Visual Studio-Versionen](~/ef6/what-is-new/visual-studio.md) Anleitungen dazu, wie Sie die neueste Version von EF-Tools für Ihre Version von Visual Studio zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="78b6b-109">See [Visual Studio Versions](~/ef6/what-is-new/visual-studio.md) for guidance on how to get the latest version of EF Tools available for your version of Visual Studio.</span></span>

## <a name="ef-runtime"></a><span data-ttu-id="78b6b-110">EF-Laufzeit</span><span class="sxs-lookup"><span data-stu-id="78b6b-110">EF Runtime</span></span>

<span data-ttu-id="78b6b-111">Die neueste Version von Entity Framework steht als die [EntityFramework NuGet-Paket](http://nuget.org/packages/EntityFramework/).</span><span class="sxs-lookup"><span data-stu-id="78b6b-111">The latest version of Entity Framework is available as the [EntityFramework NuGet package](http://nuget.org/packages/EntityFramework/).</span></span> <span data-ttu-id="78b6b-112">Wenn Sie nicht mit dem NuGet-Paket-Manager vertraut sind, empfehlen wir Ihnen das Lesen der [NuGet-Übersicht](https://docs.microsoft.com/nuget/consume-packages/overview-and-workflow).</span><span class="sxs-lookup"><span data-stu-id="78b6b-112">If you are not familiar with the NuGet Package Manager, we encourage you to read the [NuGet Overview](https://docs.microsoft.com/nuget/consume-packages/overview-and-workflow).</span></span>

### <a name="installing-the-ef-nuget-package"></a><span data-ttu-id="78b6b-113">Das EF-NuGet-Paket installieren</span><span class="sxs-lookup"><span data-stu-id="78b6b-113">Installing the EF NuGet Package</span></span>

<span data-ttu-id="78b6b-114">Sie können das EntityFramework-Paket installieren, indem Sie mit der rechten Maustaste auf die **Verweise** Ordner des Projekts, und wählen **NuGet-Pakete verwalten...**</span><span class="sxs-lookup"><span data-stu-id="78b6b-114">You can install the EntityFramework package by right-clicking on the **References** folder of your project and selecting **Manage NuGet Packages…**</span></span>

![ManageNuGetPackages](~/ef6/media/managenugetpackages.png)

### <a name="installing-from-package-manager-console"></a><span data-ttu-id="78b6b-116">Installieren von Paket-Manager-Konsole</span><span class="sxs-lookup"><span data-stu-id="78b6b-116">Installing from Package Manager Console</span></span>

<span data-ttu-id="78b6b-117">Alternativ können Sie mithilfe des folgenden Befehls EntityFramework installieren die [-Paket-Manager-Konsole](http://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="78b6b-117">Alternatively, you can install EntityFramework by running the following command in the [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span></span>

``` powershell
Install-Package EntityFramework
```

## <a name="installing-a-specific-version-of-ef"></a><span data-ttu-id="78b6b-118">Installieren eine bestimmte Version von Entity Framework</span><span class="sxs-lookup"><span data-stu-id="78b6b-118">Installing a specific version of EF</span></span>

<span data-ttu-id="78b6b-119">In EF 4.1 oder höher, neue Versionen der EF-Laufzeit veröffentlicht wurden als die [EntityFramework NuGet-Paket](https://www.nuget.org/packages/EntityFramework/).</span><span class="sxs-lookup"><span data-stu-id="78b6b-119">From EF 4.1 onwards, new versions of the EF runtime have been released as the [EntityFramework NuGet Pacakge](https://www.nuget.org/packages/EntityFramework/).</span></span> <span data-ttu-id="78b6b-120">Eine dieser Versionen können hinzugefügt werden ein Projekt .NET Framework-basierten mithilfe des folgenden Befehls im Visual Studio [-Paket-Manager-Konsole](http://docs.nuget.org/docs/start-here/using-the-package-manager-console):</span><span class="sxs-lookup"><span data-stu-id="78b6b-120">Any of those versions can be added to a .NET Framework-based project by running the following command in Visual Studio's [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console):</span></span>

``` powershell
Install-Package EntityFramework -Version <number>
```

<span data-ttu-id="78b6b-121">Beachten Sie, dass `<number>` stellt dar, die bestimmte Version von EF zu installieren.</span><span class="sxs-lookup"><span data-stu-id="78b6b-121">Note that `<number>` represents the specific version of EF to install.</span></span> <span data-ttu-id="78b6b-122">So ist beispielsweise 6.2.0 fest, der Versionsnummer für EF 6.2.</span><span class="sxs-lookup"><span data-stu-id="78b6b-122">For example, 6.2.0 is the version of number for EF 6.2.</span></span>   

<span data-ttu-id="78b6b-123">EF-Laufzeiten vor 4.1 waren Teil von .NET Framework und können nicht separat installiert werden.</span><span class="sxs-lookup"><span data-stu-id="78b6b-123">EF runtimes before 4.1 were part of .NET Framework and cannot be installed separately.</span></span>

### <a name="installing-the-latest-preview"></a><span data-ttu-id="78b6b-124">Installieren der aktuellsten Preview</span><span class="sxs-lookup"><span data-stu-id="78b6b-124">Installing the Latest Preview</span></span>

<span data-ttu-id="78b6b-125">Die oben genannten Methoden erhalten Sie die neueste Version von Entity Framework vollständig unterstützt.</span><span class="sxs-lookup"><span data-stu-id="78b6b-125">The above methods will give you the latest fully supported release of Entity Framework.</span></span> <span data-ttu-id="78b6b-126">Es gibt häufig Vorabversionen von Entity Framework verfügbar, die wir sicherlich gern nutzen würden Sie testen, und geben Sie uns Feedback zu.</span><span class="sxs-lookup"><span data-stu-id="78b6b-126">There are often prerelease versions of Entity Framework available that we would love you to try out and give us feedback on.</span></span>

<span data-ttu-id="78b6b-127">Um die aktuelle Vorschau von EntityFramework zu installieren, Sie auswählen können, **Include Prerelease** im Fenster NuGet-Pakete verwalten.</span><span class="sxs-lookup"><span data-stu-id="78b6b-127">To install the latest preview of EntityFramework you can select **Include Prerelease** in the Manage NuGet Packages window.</span></span> <span data-ttu-id="78b6b-128">Wenn keine Vorabversionen verfügbar sind, automatisch erhalten Sie die neueste Version vollständig unterstützte Version von Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="78b6b-128">If no prerelease versions are available you will automatically get the latest fully supported version of Entity Framework.</span></span>

![IncludePreRelease](~/ef6/media/includeprerelease.png)

<span data-ttu-id="78b6b-130">Alternativ können Sie den folgenden Befehl ausführen, der [-Paket-Manager-Konsole](http://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="78b6b-130">Alternatively, you can run the following command in the [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span></span>

``` powershell
Install-Package EntityFramework -Pre
```
