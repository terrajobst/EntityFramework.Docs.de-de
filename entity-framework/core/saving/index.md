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
# <a name="saving-data"></a><span data-ttu-id="ffeb9-102">Speichern von Daten</span><span class="sxs-lookup"><span data-stu-id="ffeb9-102">Saving Data</span></span>

<span data-ttu-id="ffeb9-103">Jede Kontextinstanz verfügt über ein `ChangeTracker`-Objekt, das für die Nachverfolgung von in die Datenbank zu schreibenden Änderungen zuständig ist.</span><span class="sxs-lookup"><span data-stu-id="ffeb9-103">Each context instance has a `ChangeTracker` that is responsible for keeping track of changes that need to be written to the database.</span></span> <span data-ttu-id="ffeb9-104">Wenn Sie Änderungen an Instanzen Ihrer Entitätsklassen vornehmen, werden diese Änderungen in `ChangeTracker` erfasst und dann beim Aufrufen von `SaveChanges` in die Datenbank geschrieben.</span><span class="sxs-lookup"><span data-stu-id="ffeb9-104">As you make changes to instances of your entity classes, these changes are recorded in the `ChangeTracker` and then written to the database when you call `SaveChanges`.</span></span> <span data-ttu-id="ffeb9-105">Der Datenbankanbieter ist für die Übersetzung der Änderungen in datenbankspezifische Vorgänge zuständig (z.B. die Befehle `INSERT`, `UPDATE` und `DELETE` für eine relationale Datenbank).</span><span class="sxs-lookup"><span data-stu-id="ffeb9-105">The database provider is responsible for translating the changes into database-specific operations (for example, `INSERT`, `UPDATE`, and `DELETE` commands for a relational database).</span></span>
