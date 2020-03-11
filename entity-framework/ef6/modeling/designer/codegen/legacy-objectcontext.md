---
title: Zurücksetzen von ObjectContext in Entity Framework Designer-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 36550569-a1de-47cb-ba6d-544794ffd500
ms.openlocfilehash: 3e436f0d9cf94720be9c424b327816438d571ae8
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415430"
---
# <a name="reverting-to-objectcontext-in-entity-framework-designer"></a>Zurücksetzen auf ObjectContext in Entity Framework Designer
Mit einer früheren Version von Entity Framework ein mit dem EF-Designer erstelltes Modell einen Kontext generieren, der von ObjectContext und Entitäts Klassen abgeleitet wurde, die von EntityObject abgeleitet wurden.

Ab EF 4.1 empfehlen wir das austauschen zu einer Vorlage für die Codegenerierung, die einen Kontext generiert, der von dbcontext-und poco-Entitäts Klassen abgeleitet wird.

In Visual Studio 2012 erhalten Sie dbcontext-Code, der standardmäßig für alle neuen Modelle generiert wird, die mit dem EF-Designer erstellt wurden. Vorhandene Modelle generieren weiterhin ObjectContext-basierten Code, es sei denn, Sie entscheiden sich für den Wechsel zum dbcontext-basierten Code-Generator.

## <a name="reverting-back-to-objectcontext-code-generation"></a>Zurücksetzen auf die ObjectContext-Code Generierung

### <a name="1-disable-dbcontext-code-generation"></a>1. Deaktivieren der dbcontext-Code Generierung

Die Generierung der abgeleiteten dbcontext-und poco-Klassen wird von zwei TT-Dateien in Ihrem Projekt behandelt. Wenn Sie die EDMX-Datei im Projektmappen-Explorer erweitern, werden diese Dateien angezeigt. Löschen Sie beide Dateien aus dem Projekt.

![Code-gen-Dateien](~/ef6/media/codegenfiles.png)

Wenn Sie VB.NET verwenden, müssen Sie die Schaltfläche **alle Dateien anzeigen** auswählen, um die schaltenden Dateien anzuzeigen.

![Alle Dateien anzeigen](~/ef6/media/showallfiles.png)

### <a name="2-re-enable-objectcontext-code-generation"></a>2. Erneutes Aktivieren der ObjectContext-Code Generierung

Öffnen Sie das Modell im EF-Designer, klicken Sie mit der rechten Maustaste auf einen leeren Bereich der Entwurfs Oberfläche, und wählen Sie **Eigenschaften**aus.

Ändern Sie im Eigenschaftenfenster die **Code Generierungs Strategie** von **keine** in **Standard**.

![Code-gen-Strategie](~/ef6/media/codegenstrategy.png)
