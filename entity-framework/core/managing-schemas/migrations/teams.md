---
title: Migrationen in Team Umgebungen - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 40cbc1c1bb0273bf733fadb884bffadcceeb162b
ms.sourcegitcommit: b467368cc350e6059fdc0949e042a41cb11e61d9
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
<a name="migrations-in-team-environments"></a>Migrationen in Team Umgebungen
===============================
Beim Arbeiten im Team-Umgebungen mit Migrationen auch Sie besondere Sorgfalt walten, um die Snapshot-Modelldatei. Diese Datei können Sie feststellen, ob Ihres Kollegen Migration ordnungsgemäß mit Ihren Instanzen-der zusammengeführt werden soll, wenn Sie einen Konflikt zu beheben, durch die Migration vor der Freigabe erneut erstellen müssen.

<a name="merging"></a>Zusammenführen
-------
Beim Zusammenführen von Migrationen von Ihre Teamkollegen darüber erhalten Sie möglicherweise Konflikte in Ihrem Modell Datenbankmomentaufnahme-Datei. Wenn beide Änderungen nicht verbunden sind, die Zusammenführung ist trivial, und zwei Migrationen können parallel ausgeführt werden. Beispielsweise erhalten Sie möglicherweise einen Merge-Konflikt in der typenkonfiguration der Customer-Entität, die wie folgt aussieht:

    <<<<<<< Mine
    b.Property<bool>("Deactivated");
    =======
    b.Property<int>("LoyaltyPoints");
    >>>>>>> Theirs

Da beide Eigenschaften im endgültigen Modell vorhanden sind müssen, führen Sie die Zusammenführung durch beide Eigenschaften hinzufügen. In vielen Fällen kann das Versionskontrollsystem solche Änderungen automatisch für Sie zusammengeführt.

``` csharp
b.Property<bool>("Deactivated");
b.Property<int>("LoyaltyPoints");
```

In diesen Fällen sind die Migration und Ihres Kollegen Migration unabhängig voneinander. Da diese zuerst angewendet werden konnten, müssen Sie weitere Änderungen vorzunehmen, um die Migration, bevor er mit Ihrem Team freigegeben wird.

<a name="resolving-conflicts"></a>Lösen von Konflikten
-------------------
In einigen Fällen auftreten ein Konflikts "true", beim Zusammenführen der Modell-Momentaufnahme-Modell. Angenommen, Sie und Ihre Kollegin möglicherweise jeweils die gleiche Eigenschaft umbenannt.

    <<<<<<< Mine
    b.Property<string>("Username");
    =======
    b.Property<string>("Alias");
    >>>>>>> Theirs

Wenn Sie diese Art von Konflikt auftritt, lösen Sie ihn nach der Migration neu zu erstellen. Führen Sie folgende Schritte aus:

1. Die Merge- und Ausführen eines Rollbacks zu Ihrem Arbeitsverzeichnis vor der Zusammenführung Abbrechen
2. Entfernen der Migrations (behalten Sie jedoch Ihre modelländerungen)
3. Zusammenführen von Änderungen Ihres Kollegen in Ihrem Arbeitsverzeichnis
4. Fügen Sie die Migration erneut hinzu.

Nachdem Sie auf diese Weise können die zwei Migrationen in der richtigen Reihenfolge angewendet werden. Ihre Migration wird zuerst angewendet, die Spalte umbenennen *Alias*, danach Ihre Migration benennt es um *Benutzername*.

Die Migration kann problemlos mit dem Rest des Teams freigegeben werden.
