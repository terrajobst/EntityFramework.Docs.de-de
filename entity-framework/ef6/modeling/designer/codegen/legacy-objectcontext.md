---
title: Wiederherstellen der ObjectContext im Entity Framework Designer - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 36550569-a1de-47cb-ba6d-544794ffd500
caps.latest.revision: 3
ms.openlocfilehash: 17fa6538cf5e92f59ab72376af96ad65c640a085
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2018
ms.locfileid: "39121165"
---
# <a name="reverting-to-objectcontext-in-entity-framework-designer"></a>Wiederherstellen der ObjectContext im Entity Framework-Designer
Mit früheren Version von Entity Framework ein Modell erstellt, mit dem EF Designer erzeugt ein, die von ObjectContext abgeleiteten Kontext und Entitätsklassen, die von "EntityObject" abgeleitet.

Ab ef4. 1 empfiehlt es sich auf eine Vorlage für die codegenerierung, die einen Kontext an, der von "DbContext" und POCO-Entitätsklassen generiert austauschen.

In Visual Studio 2012 erhalten Sie die "DbContext"-Code generiert standardmäßig für alle neuen Modelle, die mit dem EF Designer erstellt. Vorhandene Modelle weiterhin zum Generieren von ObjectContext-basierten Code, es sei denn, Sie an den "DbContext"-basierten Code-Generator wechseln möchten.

## <a name="reverting-back-to-objectcontext-code-generation"></a>Das Zurückkehren zur Generierung von ObjectContext-Code

### <a name="1-disable-dbcontext-code-generation"></a>1. Deaktivieren von "DbContext"-Codegenerierung

Generierung von den abgeleiteten "DbContext" und POCO-Klassen von zwei TT-Dateien in Ihrem Projekt behandelt werden, wenn Sie die EDMX-Datei im Projektmappen-Explorer erweitern, sehen Sie diese Dateien. Löschen Sie beide Dateien aus Ihrem Projekt.

![CodeGenFiles](~/ef6/media/codegenfiles.png)

Bei Verwendung von VB.NET müssen Sie auf die **alle Dateien anzeigen** Schaltfläche, um die abhängigen Dateien anzuzeigen.

![ShowAllFiles](~/ef6/media/showallfiles.png)

### <a name="2-re-enable-objectcontext-code-generation"></a>2. ObjectContext-Codegenerierung wieder aktivieren

Öffnen Sie im EF Designer, Modell mit der rechten Maustaste auf einen leeren Bereich der Entwurfsoberfläche, und wählen **Eigenschaften**.

Die Änderung der Eigenschaften im Fenster der **Code Generation Strategy** aus **keine** zu **Standard**.

![CodeGenStrategy](~/ef6/media/codegenstrategy.png)
