---
title: Auswählen der Version von Entity Framework-Laufzeit für EF-Designer-Modelle – EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 7ace90a6-46f8-4f55-a88c-7cad9620085c
caps.latest.revision: 3
ms.openlocfilehash: 75f7b4ed81528683801893c31de490ce15be6733
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2018
ms.locfileid: "39121363"
---
# <a name="selecting-entity-framework-runtime-version-for-ef-designer-models"></a>Auswählen der Version von Entity Framework-Common Language Runtime für Entity Framework-Modellen-Designer
> [!NOTE]
> **Nur EF6 und höher:** Die Features, APIs usw., die auf dieser Seite erläutert werden, wurden in Entity Framework 6 eingeführt. Wenn Sie eine frühere Version verwenden, gelten manche Informationen nicht.

Ab EF6 wurde der folgende Bildschirm hinzugefügt, um dem EF Designer es Ihnen ermöglichen, wählen Sie die Version der Runtime, die Sie auswählen, wenn Sie ein Modell erstellen möchten. Der Bildschirm wird angezeigt, wenn die neueste Version von Entity Framework nicht bereits im Projekt installiert ist. Wenn die aktuelle Version bereits installiert ist er wird nur in der Standardeinstellung verwendet.

![Bildschirm](~/ef6/media/screen.png)


## <a name="targeting-ef6x"></a>Für EF 6.x

EF6 können Sie auf dem Bildschirm "Wählen Sie Ihre Version", die EF6-Laufzeit zu Ihrem Projekt hinzufügen. Nachdem Sie EF 6 hinzugefügt haben, müssen Sie beenden, sehen diesen Bildschirm im aktuellen Projekt.

EF6 wird deaktiviert werden, wenn Sie bereits über eine ältere Version von Entity Framework installiert wird (da Sie mehrere Versionen der Laufzeit aus demselben Projekt können nicht als Ziel) verfügen. Gehen folgendermaßen Sie vor, um Ihr Projekt auf EF6 zu aktualisieren, wenn der EF6-Option hier nicht aktiviert ist, haben:

1.  Mit der rechten Maustaste auf das Projekt im Projektmappen-Explorer, und wählen Sie **NuGet-Pakete verwalten...**
2.  Wählen Sie **Updates**
3.  Wählen Sie **EntityFramework** (Stellen Sie sicher, dass geht es auf die Version aktualisiert werden sollen)
4.  Klicken Sie auf **aktualisieren**

 

## <a name="targeting-ef5x"></a>Für die Zielgruppenadressierung EF5.x

EF5 können Sie vom Bildschirm "Wählen Sie Ihre Version" auf die Laufzeit-EF5 zu Ihrem Projekt hinzugefügt. Nachdem Sie EF5 hinzugefügt haben, sehen Sie weiterhin den Bildschirm mit der EF6-Option deaktiviert.

Wenn Sie eine EF4.x-Version der Laufzeit bereits installiert haben sehen Sie diese Version von EF in den Bildschirm anstelle von EF5 aufgeführt dann. In diesem Fall können Sie auf EF5 aktualisieren verwenden Sie die folgenden Schritte aus:

1.  Wählen Sie **Tools –&gt; Bibliothekspaket-Manager -&gt; -Paket-Manager-Konsole**
2.  Führen Sie **Install-Package EntityFramework-Version 5.0.0**

 

## <a name="targeting-ef4x"></a>Für die Zielgruppenadressierung EF4.x

Sie können die Laufzeit EF4.x zu Ihrem Projekt mit den folgenden Schritten installieren:

1.  Wählen Sie **Tools –&gt; Bibliothekspaket-Manager -&gt; -Paket-Manager-Konsole**
2.  Führen Sie **Install-Package EntityFramework-Version 4.3.0**
