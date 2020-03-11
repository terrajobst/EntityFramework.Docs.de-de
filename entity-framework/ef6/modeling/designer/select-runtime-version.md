---
title: Auswählen Entity Framework Lauf Zeit Version für EF-Designer-Modelle EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 7ace90a6-46f8-4f55-a88c-7cad9620085c
ms.openlocfilehash: 40ad05c1f015e6a51150d04eee8d9aa581d72c33
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414992"
---
# <a name="selecting-entity-framework-runtime-version-for-ef-designer-models"></a><span data-ttu-id="9f733-102">Auswählen Entity Framework Lauf Zeit Version für EF-Designer-Modelle</span><span class="sxs-lookup"><span data-stu-id="9f733-102">Selecting Entity Framework Runtime Version for EF Designer Models</span></span>
> [!NOTE]
> <span data-ttu-id="9f733-103">**Nur EF6 und höher:** Die Features, APIs usw., die auf dieser Seite erläutert werden, wurden in Entity Framework 6 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="9f733-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="9f733-104">Wenn Sie eine frühere Version verwenden, gelten manche Informationen nicht.</span><span class="sxs-lookup"><span data-stu-id="9f733-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="9f733-105">Beginnend mit EF6 wurde dem EF-Designer der folgende Bildschirm hinzugefügt, damit Sie die Version der Laufzeit auswählen können, die Sie beim Erstellen eines Modells als Ziel auswählen möchten.</span><span class="sxs-lookup"><span data-stu-id="9f733-105">Starting with EF6 the following screen was added to the EF Designer to allow you to select the version of the runtime you wish to target when creating a model.</span></span> <span data-ttu-id="9f733-106">Der Bildschirm wird angezeigt, wenn die neueste Version von Entity Framework nicht bereits im Projekt installiert ist.</span><span class="sxs-lookup"><span data-stu-id="9f733-106">The screen will appear when the latest version of Entity Framework is not already installed in the project.</span></span> <span data-ttu-id="9f733-107">Wenn die neueste Version bereits installiert ist, wird Sie standardmäßig verwendet.</span><span class="sxs-lookup"><span data-stu-id="9f733-107">If the latest version is already installed it will just be used by default.</span></span>

![Bildschirm](~/ef6/media/screen.png)


## <a name="targeting-ef6x"></a><span data-ttu-id="9f733-109">Ziel EF6. x</span><span class="sxs-lookup"><span data-stu-id="9f733-109">Targeting EF6.x</span></span>

<span data-ttu-id="9f733-110">Sie können EF6 auf dem Bildschirm "wählen Sie Ihre Version" auswählen, um dem Projekt die EF6-Laufzeit hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="9f733-110">You can choose EF6 from the 'Choose Your Version' screen to add the EF6 runtime to your project.</span></span> <span data-ttu-id="9f733-111">Nachdem Sie EF6 hinzugefügt haben, wird dieser Bildschirm im aktuellen Projekt nicht mehr angezeigt.</span><span class="sxs-lookup"><span data-stu-id="9f733-111">Once you've added EF6, you’ll stop seeing this screen in the current project.</span></span>

<span data-ttu-id="9f733-112">EF6 wird deaktiviert, wenn Sie bereits eine ältere Version von EF installiert haben (da Sie nicht mehrere Versionen der Laufzeit aus demselben Projekt ausrichten können).</span><span class="sxs-lookup"><span data-stu-id="9f733-112">EF6 will be disabled if you already have an older version of EF installed (since you can't target multiple versions of the runtime from the same project).</span></span> <span data-ttu-id="9f733-113">Wenn die EF6-Option hier nicht aktiviert ist, führen Sie die folgenden Schritte aus, um Ihr Projekt auf EF6 zu aktualisieren:</span><span class="sxs-lookup"><span data-stu-id="9f733-113">If EF6 option is not enabled here, follow these steps to upgrade your project to EF6:</span></span>

1.  <span data-ttu-id="9f733-114">Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und wählen Sie **nuget-Pakete verwalten aus.**</span><span class="sxs-lookup"><span data-stu-id="9f733-114">Right-click on your project in Solution Explorer and select **Manage NuGet Packages...**</span></span>
2.  <span data-ttu-id="9f733-115">**Updates** auswählen</span><span class="sxs-lookup"><span data-stu-id="9f733-115">Select **Updates**</span></span>
3.  <span data-ttu-id="9f733-116">Wählen Sie **EntityFramework** aus (stellen Sie sicher, dass es auf die gewünschte Version aktualisiert wird).</span><span class="sxs-lookup"><span data-stu-id="9f733-116">Select **EntityFramework** (make sure it is going to update it to the version you want)</span></span>
4.  <span data-ttu-id="9f733-117">Klicken Sie auf **Aktualisieren**.</span><span class="sxs-lookup"><span data-stu-id="9f733-117">Click **Update**</span></span>

 

## <a name="targeting-ef5x"></a><span data-ttu-id="9f733-118">Ziel EF5. x</span><span class="sxs-lookup"><span data-stu-id="9f733-118">Targeting EF5.x</span></span>

<span data-ttu-id="9f733-119">Sie können EF5 auf dem Bildschirm "wählen Sie Ihre Version" auswählen, um dem Projekt die EF5-Laufzeit hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="9f733-119">You can choose EF5 from the 'Choose Your Version' screen to add the EF5 runtime to your project.</span></span> <span data-ttu-id="9f733-120">Nachdem Sie EF5 hinzugefügt haben, wird der Bildschirm mit deaktivierter EF6-Option angezeigt.</span><span class="sxs-lookup"><span data-stu-id="9f733-120">Once you've added EF5, you’ll still see the screen with the EF6 option disabled.</span></span>

<span data-ttu-id="9f733-121">Wenn Sie eine EF4. x-Version der Laufzeit bereits installiert haben, sehen Sie, dass die EF-Version auf dem Bildschirm anstelle von EF5 aufgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="9f733-121">If you have an EF4.x version of the runtime already installed then you will see that version of EF listed in the screen rather than EF5.</span></span> <span data-ttu-id="9f733-122">In dieser Situation können Sie ein Upgrade auf EF5 ausführen, indem Sie die folgenden Schritte ausführen:</span><span class="sxs-lookup"><span data-stu-id="9f733-122">In this situation you can upgrade to EF5 using the following steps:</span></span>

1.  <span data-ttu-id="9f733-123">Wählen Sie **Tools-&gt; Bibliothekspaket-Manager-&gt; Paket-Manager-Konsole**</span><span class="sxs-lookup"><span data-stu-id="9f733-123">Select **Tools -&gt; Library Package Manager -&gt; Package Manager Console**</span></span>
2.  <span data-ttu-id="9f733-124">Ausführen von " **install-Package" EntityFramework-Version 5.0.0**</span><span class="sxs-lookup"><span data-stu-id="9f733-124">Run **Install-Package EntityFramework -version 5.0.0**</span></span>

 

## <a name="targeting-ef4x"></a><span data-ttu-id="9f733-125">Ziel EF4. x</span><span class="sxs-lookup"><span data-stu-id="9f733-125">Targeting EF4.x</span></span>

<span data-ttu-id="9f733-126">Mithilfe der folgenden Schritte können Sie die EF4. x-Laufzeit für Ihr Projekt installieren:</span><span class="sxs-lookup"><span data-stu-id="9f733-126">You can install the EF4.x runtime to your project using the following steps:</span></span>

1.  <span data-ttu-id="9f733-127">Wählen Sie **Tools-&gt; Bibliothekspaket-Manager-&gt; Paket-Manager-Konsole**</span><span class="sxs-lookup"><span data-stu-id="9f733-127">Select **Tools -&gt; Library Package Manager -&gt; Package Manager Console**</span></span>
2.  <span data-ttu-id="9f733-128">Ausführen von " **install-Package" EntityFramework-Version 4.3.0**</span><span class="sxs-lookup"><span data-stu-id="9f733-128">Run **Install-Package EntityFramework -version 4.3.0**</span></span>
