---
title: "Speichern von Daten – EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ef044629-feca-4fd1-a48f-d208daedaf92
ms.technology: entity-framework-core
uid: core/saving/index
ms.openlocfilehash: 9280b9c34b41c0319f918488cd7d28eeceef12e7
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
---
# <a name="saving-data"></a>Speichern von Daten

Jede Kontextinstanz verfügt über ein `ChangeTracker`-Objekt, das für die Nachverfolgung von in die Datenbank zu schreibenden Änderungen zuständig ist. Wenn Sie Änderungen an Instanzen Ihrer Entitätsklassen vornehmen, werden diese Änderungen in `ChangeTracker` erfasst und dann beim Aufrufen von `SaveChanges` in die Datenbank geschrieben. Der Datenbankanbieter ist für die Übersetzung der Änderungen in datenbankspezifische Vorgänge zuständig (z.B. die Befehle `INSERT`, `UPDATE` und `DELETE` für eine relationale Datenbank).
