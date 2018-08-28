---
title: Migrationen in Teamumgebungen – EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.openlocfilehash: e8ff7f468d5ab6dbd6285f1abf9199e413288d10
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997694"
---
<a name="migrations-in-team-environments"></a>Migrationen in Teamumgebungen
===============================
Bei der Arbeit mit Migrationen in teamumgebungen Achten Sie zusätzliche auf der Snapshot-Modelldatei. Diese Datei können Sie erkennen, wenn Ihre Kollegin der Migration ordnungsgemäß mit dem eigenen zusammengeführt oder Sie einen Konflikt zu beheben, erstellen Sie Ihre Migration, bevor er freigegeben wird müssen.

<a name="merging"></a>Zusammenführen
-------
Beim Zusammenführen von Migrationen von Ihren Teamkollegen erhalten Sie möglicherweise Konflikte in Ihrem Modell Datenbankmomentaufnahme-Datei. Wenn beide Änderungen unabhängig sind, die Zusammenführung ist einfach, und zwei livemigrationen können gleichzeitig vorhanden sein. Beispielsweise erhalten Sie möglicherweise ein Zusammenführungskonflikt in der typenkonfiguration der Customer-Entität, die folgendermaßen aussieht:

    <<<<<<< Mine
    b.Property<bool>("Deactivated");
    =======
    b.Property<int>("LoyaltyPoints");
    >>>>>>> Theirs

Da diese beiden Eigenschaften im endgültigen Modell vorhanden sind müssen, führen Sie die Zusammenführung durch beide Eigenschaften hinzufügen. In vielen Fällen kann Ihr Versionskontrollsystem solche Änderungen automatisch für Sie zusammenführen.

``` csharp
b.Property<bool>("Deactivated");
b.Property<int>("LoyaltyPoints");
```

In diesen Fällen sind die Migration und Ihre Kollegin der Migration unabhängig voneinander. Da es sich bei einem der beiden zuerst angewendet werden kann, müssen Sie keine weiteren Änderungen vornehmen, um die Migration, bevor er mit Ihrem Team freigegeben wird.

<a name="resolving-conflicts"></a>Lösen von Konflikten
-------------------
Sie treten gelegentlich einen Konflikt mit "true", beim mergen der Modell-Momentaufnahme-Modell. Angenommen, Sie und Ihre Kollegin möglicherweise jeweils die gleiche Eigenschaft umbenannt.

    <<<<<<< Mine
    b.Property<string>("Username");
    =======
    b.Property<string>("Alias");
    >>>>>>> Theirs

Wenn Sie diese Art von Konflikt auftritt, müssen beheben Sie ihn, indem der Migration neu zu erstellen. Führen Sie folgende Schritte aus:

1. Abbrechen der zusammenführen und ein Rollback in Ihr Arbeitsverzeichnis vor der Zusammenführung
2. Entfernen Sie die Migration (aber behalten Sie Ihr Modell ändert)
3. Ihre Kollegin der Änderungen in Ihrem Arbeitsverzeichnis zusammenführen
4. Fügen Sie Ihre Migration erneut hinzu.

Nachdem Sie auf diese Weise können die zwei Migrationen in der richtigen Reihenfolge angewendet werden. Ihre Migration wird zuerst angewendet, die Spalte umbenennen *Alias*, danach Ihre Migration benennt es um *Benutzername*.

Die Migration kann mit dem Rest des Teams sicher freigegeben werden.
