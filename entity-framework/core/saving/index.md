---
title: Speichern von Daten – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ef044629-feca-4fd1-a48f-d208daedaf92
uid: core/saving/index
ms.openlocfilehash: c610ea2a9138482f93d2d54c9085ef827af276c8
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/07/2020
ms.locfileid: "78413095"
---
# <a name="saving-data"></a>Speichern von Daten

Jede Kontextinstanz verfügt über ein `ChangeTracker`-Objekt, das für die Nachverfolgung von in die Datenbank zu schreibenden Änderungen zuständig ist. Wenn Sie Änderungen an Instanzen Ihrer Entitätsklassen vornehmen, werden diese Änderungen in `ChangeTracker` erfasst und dann beim Aufrufen von `SaveChanges` in die Datenbank geschrieben. Der Datenbankanbieter ist für die Übersetzung der Änderungen in datenbankspezifische Vorgänge zuständig (z.B. die Befehle `INSERT`, `UPDATE` und `DELETE` für eine relationale Datenbank).
