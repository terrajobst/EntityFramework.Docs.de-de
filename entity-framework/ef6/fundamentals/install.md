---
title: Get Entity Framework-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 122c38a2-f9e8-4ecc-9c72-a83bc9af7814
ms.openlocfilehash: 2bdec6a9be228fbe934d0f46aa1bfafdfb2c971c
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181738"
---
# <a name="get-entity-framework"></a><span data-ttu-id="0d4f2-102">Abrufen von Entity Framework</span><span class="sxs-lookup"><span data-stu-id="0d4f2-102">Get Entity Framework</span></span>
<span data-ttu-id="0d4f2-103">Entity Framework besteht aus den EF-Tools für Visual Studio und der EF-Laufzeit.</span><span class="sxs-lookup"><span data-stu-id="0d4f2-103">Entity Framework is made up of the EF Tools for Visual Studio and the EF Runtime.</span></span>

## <a name="ef-tools-for-visual-studio"></a><span data-ttu-id="0d4f2-104">EF-Tools für Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0d4f2-104">EF Tools for Visual Studio</span></span>

<span data-ttu-id="0d4f2-105">Die Entity Framework Tools für Visual Studio umfassen den EF-Designer und den EF-Modell-Assistenten und sind für die Daten Bank First-und Model First-Workflows erforderlich.</span><span class="sxs-lookup"><span data-stu-id="0d4f2-105">The Entity Framework Tools for Visual Studio include the EF Designer and the EF Model Wizard and are required for the database first and model first workflows.</span></span> <span data-ttu-id="0d4f2-106">EF-Tools sind in allen neueren Versionen von Visual Studio enthalten.</span><span class="sxs-lookup"><span data-stu-id="0d4f2-106">EF Tools are included in all recent versions of Visual Studio.</span></span> <span data-ttu-id="0d4f2-107">Wenn Sie eine benutzerdefinierte Installation von Visual Studio durchführen, müssen Sie sicherstellen, dass das Element "Entity Framework 6 Tools" ausgewählt ist. Wählen Sie hierzu entweder eine Arbeitsauslastung aus, die es enthält, oder wählen Sie es als einzelne Komponente aus.</span><span class="sxs-lookup"><span data-stu-id="0d4f2-107">If you perform a custom install of Visual Studio you will need to ensure that the item "Entity Framework 6 Tools" is selected by either choosing a workload that includes it or by selecting it as an individual component.</span></span>

<span data-ttu-id="0d4f2-108">Für einige frühere Versionen von Visual Studio stehen aktualisierte EF-Tools als Download zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="0d4f2-108">For some past versions of Visual Studio, updated EF Tools are available as a download.</span></span> <span data-ttu-id="0d4f2-109">Unter [Visual Studio-Versionen](~/ef6/what-is-new/visual-studio.md) finden Sie Anleitungen zum Bereitstellen der neuesten Version von EF-Tools für Ihre Version von Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0d4f2-109">See [Visual Studio Versions](~/ef6/what-is-new/visual-studio.md) for guidance on how to get the latest version of EF Tools available for your version of Visual Studio.</span></span>

## <a name="ef-runtime"></a><span data-ttu-id="0d4f2-110">EF-Laufzeit</span><span class="sxs-lookup"><span data-stu-id="0d4f2-110">EF Runtime</span></span>

<span data-ttu-id="0d4f2-111">Die neueste Version von Entity Framework ist als das [nuget-Paket "EntityFramework](https://nuget.org/packages/EntityFramework/)" verfügbar.</span><span class="sxs-lookup"><span data-stu-id="0d4f2-111">The latest version of Entity Framework is available as the [EntityFramework NuGet package](https://nuget.org/packages/EntityFramework/).</span></span> <span data-ttu-id="0d4f2-112">Wenn Sie mit dem nuget-Paket-Manager nicht vertraut sind, empfehlen wir Ihnen, die [Übersicht über nuget](https://docs.microsoft.com/nuget/consume-packages/overview-and-workflow)zu lesen.</span><span class="sxs-lookup"><span data-stu-id="0d4f2-112">If you are not familiar with the NuGet Package Manager, we encourage you to read the [NuGet Overview](https://docs.microsoft.com/nuget/consume-packages/overview-and-workflow).</span></span>

### <a name="installing-the-ef-nuget-package"></a><span data-ttu-id="0d4f2-113">Installieren des EF-nuget-Pakets</span><span class="sxs-lookup"><span data-stu-id="0d4f2-113">Installing the EF NuGet Package</span></span>

<span data-ttu-id="0d4f2-114">Sie können das Paket "EntityFramework" installieren, indem Sie mit der rechten Maustaste auf den Ordner " **Verweise** " des Projekts klicken und **nuget-Pakete verwalten auswählen.**</span><span class="sxs-lookup"><span data-stu-id="0d4f2-114">You can install the EntityFramework package by right-clicking on the **References** folder of your project and selecting **Manage NuGet Packages…**</span></span>

![Verwalten von nuget-Paketen](~/ef6/media/managenugetpackages.png)

### <a name="installing-from-package-manager-console"></a><span data-ttu-id="0d4f2-116">Installieren über die Paket-Manager-Konsole</span><span class="sxs-lookup"><span data-stu-id="0d4f2-116">Installing from Package Manager Console</span></span>

<span data-ttu-id="0d4f2-117">Alternativ können Sie "EntityFramework" installieren, indem Sie den folgenden Befehl in der [Paket-Manager-Konsole](https://docs.nuget.org/docs/start-here/using-the-package-manager-console)ausführen.</span><span class="sxs-lookup"><span data-stu-id="0d4f2-117">Alternatively, you can install EntityFramework by running the following command in the [Package Manager Console](https://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span></span>

``` powershell
Install-Package EntityFramework
```

## <a name="installing-a-specific-version-of-ef"></a><span data-ttu-id="0d4f2-118">Installieren einer bestimmten Version von EF</span><span class="sxs-lookup"><span data-stu-id="0d4f2-118">Installing a specific version of EF</span></span>

<span data-ttu-id="0d4f2-119">Ab EF 4,1 wurden neue Versionen der EF-Laufzeit als das [nuget-Paket "EntityFramework](https://www.nuget.org/packages/EntityFramework/)" veröffentlicht.</span><span class="sxs-lookup"><span data-stu-id="0d4f2-119">From EF 4.1 onwards, new versions of the EF runtime have been released as the [EntityFramework NuGet Package](https://www.nuget.org/packages/EntityFramework/).</span></span> <span data-ttu-id="0d4f2-120">Diese Versionen können einem .NET Framework basierten Projekt hinzugefügt werden, indem Sie den folgenden Befehl in der Paket-Manager- [Konsole](https://docs.nuget.org/docs/start-here/using-the-package-manager-console)von Visual Studio ausführen:</span><span class="sxs-lookup"><span data-stu-id="0d4f2-120">Any of those versions can be added to a .NET Framework-based project by running the following command in Visual Studio's [Package Manager Console](https://docs.nuget.org/docs/start-here/using-the-package-manager-console):</span></span>

``` powershell
Install-Package EntityFramework -Version <number>
```

<span data-ttu-id="0d4f2-121">Beachten Sie, dass `<number>` die spezifische Version von EF darstellt, die installiert werden soll.</span><span class="sxs-lookup"><span data-stu-id="0d4f2-121">Note that `<number>` represents the specific version of EF to install.</span></span> <span data-ttu-id="0d4f2-122">Beispielsweise ist 6.2.0 die Version der Zahl für EF 6,2.</span><span class="sxs-lookup"><span data-stu-id="0d4f2-122">For example, 6.2.0 is the version of number for EF 6.2.</span></span>   

<span data-ttu-id="0d4f2-123">EF-Laufzeiten vor 4,1 waren Teil .NET Framework und können nicht separat installiert werden.</span><span class="sxs-lookup"><span data-stu-id="0d4f2-123">EF runtimes before 4.1 were part of .NET Framework and cannot be installed separately.</span></span>

### <a name="installing-the-latest-preview"></a><span data-ttu-id="0d4f2-124">Installieren der aktuellen Vorschauversion</span><span class="sxs-lookup"><span data-stu-id="0d4f2-124">Installing the Latest Preview</span></span>

<span data-ttu-id="0d4f2-125">Die oben genannten Methoden geben Ihnen die neueste vollständig unterstützte Version von Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="0d4f2-125">The above methods will give you the latest fully supported release of Entity Framework.</span></span> <span data-ttu-id="0d4f2-126">Häufig sind vorab Versionen von Entity Framework verfügbar, die wir gerne ausprobieren und uns Feedback geben würden.</span><span class="sxs-lookup"><span data-stu-id="0d4f2-126">There are often prerelease versions of Entity Framework available that we would love you to try out and give us feedback on.</span></span>

<span data-ttu-id="0d4f2-127">Um die neueste Vorschau von EntityFramework zu installieren, können Sie im Fenster nuget-Pakete verwalten die Option **Vorabversion einschließen** auswählen.</span><span class="sxs-lookup"><span data-stu-id="0d4f2-127">To install the latest preview of EntityFramework you can select **Include Prerelease** in the Manage NuGet Packages window.</span></span> <span data-ttu-id="0d4f2-128">Wenn keine vorab Versionen verfügbar sind, erhalten Sie automatisch die neueste vollständig unterstützte Version von Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="0d4f2-128">If no prerelease versions are available you will automatically get the latest fully supported version of Entity Framework.</span></span>

![Vorabversion einschließen](~/ef6/media/includeprerelease.png)

<span data-ttu-id="0d4f2-130">Alternativ können Sie den folgenden Befehl in der Paket- [Manager-Konsole](https://docs.nuget.org/docs/start-here/using-the-package-manager-console)ausführen.</span><span class="sxs-lookup"><span data-stu-id="0d4f2-130">Alternatively, you can run the following command in the [Package Manager Console](https://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span></span>

``` powershell
Install-Package EntityFramework -Pre
```
