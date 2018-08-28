---
title: Auswählen der Version von Entity Framework-Laufzeit für EF-Designer-Modelle – EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 7ace90a6-46f8-4f55-a88c-7cad9620085c
ms.openlocfilehash: 8864bb8166a7c16455d5c3bebe91e2ce8d142685
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998241"
---
# <a name="selecting-entity-framework-runtime-version-for-ef-designer-models"></a><span data-ttu-id="e8d24-102">Auswählen der Version von Entity Framework-Common Language Runtime für Entity Framework-Modellen-Designer</span><span class="sxs-lookup"><span data-stu-id="e8d24-102">Selecting Entity Framework Runtime Version for EF Designer Models</span></span>
> [!NOTE]
> <span data-ttu-id="e8d24-103">**Nur EF6 und höher:** Die Features, APIs usw., die auf dieser Seite erläutert werden, wurden in Entity Framework 6 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="e8d24-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="e8d24-104">Wenn Sie eine frühere Version verwenden, gelten manche Informationen nicht.</span><span class="sxs-lookup"><span data-stu-id="e8d24-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="e8d24-105">Ab EF6 wurde der folgende Bildschirm hinzugefügt, um dem EF Designer es Ihnen ermöglichen, wählen Sie die Version der Runtime, die Sie auswählen, wenn Sie ein Modell erstellen möchten.</span><span class="sxs-lookup"><span data-stu-id="e8d24-105">Starting with EF6 the following screen was added to the EF Designer to allow you to select the version of the runtime you wish to target when creating a model.</span></span> <span data-ttu-id="e8d24-106">Der Bildschirm wird angezeigt, wenn die neueste Version von Entity Framework nicht bereits im Projekt installiert ist.</span><span class="sxs-lookup"><span data-stu-id="e8d24-106">The screen will appear when the latest version of Entity Framework is not already installed in the project.</span></span> <span data-ttu-id="e8d24-107">Wenn die aktuelle Version bereits installiert ist er wird nur in der Standardeinstellung verwendet.</span><span class="sxs-lookup"><span data-stu-id="e8d24-107">If the latest version is already installed it will just be used by default.</span></span>

![Bildschirm](~/ef6/media/screen.png)


## <a name="targeting-ef6x"></a><span data-ttu-id="e8d24-109">Für EF 6.x</span><span class="sxs-lookup"><span data-stu-id="e8d24-109">Targeting EF6.x</span></span>

<span data-ttu-id="e8d24-110">EF6 können Sie auf dem Bildschirm "Wählen Sie Ihre Version", die EF6-Laufzeit zu Ihrem Projekt hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="e8d24-110">You can choose EF6 from the 'Choose Your Version' screen to add the EF6 runtime to your project.</span></span> <span data-ttu-id="e8d24-111">Nachdem Sie EF 6 hinzugefügt haben, müssen Sie beenden, sehen diesen Bildschirm im aktuellen Projekt.</span><span class="sxs-lookup"><span data-stu-id="e8d24-111">Once you've added EF6, you’ll stop seeing this screen in the current project.</span></span>

<span data-ttu-id="e8d24-112">EF6 wird deaktiviert werden, wenn Sie bereits über eine ältere Version von Entity Framework installiert wird (da Sie mehrere Versionen der Laufzeit aus demselben Projekt können nicht als Ziel) verfügen.</span><span class="sxs-lookup"><span data-stu-id="e8d24-112">EF6 will be disabled if you already have an older version of EF installed (since you can't target multiple versions of the runtime from the same project).</span></span> <span data-ttu-id="e8d24-113">Gehen folgendermaßen Sie vor, um Ihr Projekt auf EF6 zu aktualisieren, wenn der EF6-Option hier nicht aktiviert ist, haben:</span><span class="sxs-lookup"><span data-stu-id="e8d24-113">If EF6 option is not enabled here, follow these steps to upgrade your project to EF6:</span></span>

1.  <span data-ttu-id="e8d24-114">Mit der rechten Maustaste auf das Projekt im Projektmappen-Explorer, und wählen Sie **NuGet-Pakete verwalten...**</span><span class="sxs-lookup"><span data-stu-id="e8d24-114">Right-click on your project in Solution Explorer and select **Manage NuGet Packages...**</span></span>
2.  <span data-ttu-id="e8d24-115">Wählen Sie **Updates**</span><span class="sxs-lookup"><span data-stu-id="e8d24-115">Select **Updates**</span></span>
3.  <span data-ttu-id="e8d24-116">Wählen Sie **EntityFramework** (Stellen Sie sicher, dass geht es auf die Version aktualisiert werden sollen)</span><span class="sxs-lookup"><span data-stu-id="e8d24-116">Select **EntityFramework** (make sure it is going to update it to the version you want)</span></span>
4.  <span data-ttu-id="e8d24-117">Klicken Sie auf **aktualisieren**</span><span class="sxs-lookup"><span data-stu-id="e8d24-117">Click **Update**</span></span>

 

## <a name="targeting-ef5x"></a><span data-ttu-id="e8d24-118">Für die Zielgruppenadressierung EF5.x</span><span class="sxs-lookup"><span data-stu-id="e8d24-118">Targeting EF5.x</span></span>

<span data-ttu-id="e8d24-119">EF5 können Sie vom Bildschirm "Wählen Sie Ihre Version" auf die Laufzeit-EF5 zu Ihrem Projekt hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="e8d24-119">You can choose EF5 from the 'Choose Your Version' screen to add the EF5 runtime to your project.</span></span> <span data-ttu-id="e8d24-120">Nachdem Sie EF5 hinzugefügt haben, sehen Sie weiterhin den Bildschirm mit der EF6-Option deaktiviert.</span><span class="sxs-lookup"><span data-stu-id="e8d24-120">Once you've added EF5, you’ll still see the screen with the EF6 option disabled.</span></span>

<span data-ttu-id="e8d24-121">Wenn Sie eine EF4.x-Version der Laufzeit bereits installiert haben sehen Sie diese Version von EF in den Bildschirm anstelle von EF5 aufgeführt dann.</span><span class="sxs-lookup"><span data-stu-id="e8d24-121">If you have an EF4.x version of the runtime already installed then you will see that version of EF listed in the screen rather than EF5.</span></span> <span data-ttu-id="e8d24-122">In diesem Fall können Sie auf EF5 aktualisieren verwenden Sie die folgenden Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="e8d24-122">In this situation you can upgrade to EF5 using the following steps:</span></span>

1.  <span data-ttu-id="e8d24-123">Wählen Sie **Tools –&gt; Bibliothekspaket-Manager -&gt; -Paket-Manager-Konsole**</span><span class="sxs-lookup"><span data-stu-id="e8d24-123">Select **Tools -&gt; Library Package Manager -&gt; Package Manager Console**</span></span>
2.  <span data-ttu-id="e8d24-124">Führen Sie **Install-Package EntityFramework-Version 5.0.0**</span><span class="sxs-lookup"><span data-stu-id="e8d24-124">Run **Install-Package EntityFramework -version 5.0.0**</span></span>

 

## <a name="targeting-ef4x"></a><span data-ttu-id="e8d24-125">Für die Zielgruppenadressierung EF4.x</span><span class="sxs-lookup"><span data-stu-id="e8d24-125">Targeting EF4.x</span></span>

<span data-ttu-id="e8d24-126">Sie können die Laufzeit EF4.x zu Ihrem Projekt mit den folgenden Schritten installieren:</span><span class="sxs-lookup"><span data-stu-id="e8d24-126">You can install the EF4.x runtime to your project using the following steps:</span></span>

1.  <span data-ttu-id="e8d24-127">Wählen Sie **Tools –&gt; Bibliothekspaket-Manager -&gt; -Paket-Manager-Konsole**</span><span class="sxs-lookup"><span data-stu-id="e8d24-127">Select **Tools -&gt; Library Package Manager -&gt; Package Manager Console**</span></span>
2.  <span data-ttu-id="e8d24-128">Führen Sie **Install-Package EntityFramework-Version 4.3.0**</span><span class="sxs-lookup"><span data-stu-id="e8d24-128">Run **Install-Package EntityFramework -version 4.3.0**</span></span>
