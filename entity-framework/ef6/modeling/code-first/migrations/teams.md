---
title: Code First-Migrationen in Teamumgebungen - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 4c2d9a95-de6f-4e97-9738-c1f8043eff69
caps.latest.revision: 3
ms.openlocfilehash: 6a29fe3ab3aa6b9a51bea54706490f143e860a48
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2018
ms.locfileid: "39121492"
---
# <a name="code-first-migrations-in-team-environments"></a>Code First-Migrationen in Teamumgebungen
> [!NOTE]
> In diesem Artikel wird davon ausgegangen, dass Sie wissen, wie Code First-Migrationen in grundlegenden Szenarien verwenden. Wenn Sie dies nicht tun, müssen Sie lesen [Code First-Migrationen](~/ef6/modeling/code-first/migrations/index.md) vor dem fortfahren.

## <a name="grab-a-coffee-you-need-to-read-this-whole-article"></a>Einen Kaffee, müssen Sie diesen gesamten Artikel lesen

Die Probleme in teamumgebungen sind hauptsächlich, um das Zusammenführen von Migrationen, bei der zwei Entwickler Migrationen in ihrer lokalen Codebasis generiert haben. Während Sie die Schritte mit der diese ziemlich einfach sind, erforderlich sind Sie solide Kenntnisse über die Funktionsweise von Migrationen. Sie nicht nur fahren Sie mit der End-finden Sie im gesamten Artikel, um sicherzustellen, dass Sie erfolgreich sind die Zeit in Anspruch nehmen.

## <a name="some-general-guidelines"></a>Einige allgemeinen Richtlinien

Bevor wir näher so verwalten Sie das Zusammenführen von Migrationen, die von mehreren Entwicklern generiert untersuchen, sind hier einige allgemeinen Richtlinien, die Sie festlegen, Erfolge.

### <a name="each-team-member-should-have-a-local-development-database"></a>Jedes Teammitglied sollte eine lokale Entwicklungs-Datenbank verfügen.

Migrationen verwendet die  **\_ \_MigrationsHistory** Tabelle zum Speichern, welche Migrationen auf die Datenbank angewendet wurden. Wenn mehrere generieren Weitere Migrationen beim Versuch Entwickler, die an dieselbe Datenbank gerichtet (und somit eine  **\_ \_MigrationsHistory** Tabelle) Migrationen sehr verwirrt werden soll.

Wenn Sie Teammitglieder, die Migrationen zu generieren haben, werden nicht, besteht natürlich kein Problem, müssen sie eine zentrale Development-Datenbank gemeinsam nutzen.

### <a name="avoid-automatic-migrations"></a>Vermeiden Sie automatische Migrationen

Das Fazit ist, dass automatische Migrationen anfänglich in teamumgebungen gut aussehen, aber in Wirklichkeit nur funktionieren nicht. Wenn Sie wissen, warum, lesen – beibehalten möchten, dann können Sie mit dem nächsten Abschnitt fortfahren.

Automatische Migrationen können Sie das Datenbankschema aktualisiert, um das aktuelle Modell ohne die Notwendigkeit zum Generieren von Codedateien (Code-basierte Migrationen) übereinstimmen. Automatische Migrationen funktioniert sehr gut in einer teamumgebung, wenn Sie nur einmal verwendet und nie Code-basierte Migrationen generiert. Das Problem besteht darin, dass automatische Migrationen sind beschränkt und nicht behandeln eine Reihe von Operationen Eigenschaftenspalte/benennt, Verschieben von Daten in einer anderen Tabelle usw. Für solche Szenarien entworfen, erhalten Sie letztlich, Generieren von Code-basierte Migrationen (und Bearbeiten des eingerüsteten Codes), die zwischen den Änderungen gemischt werden, die durch automatische Migrationen behandelt werden. Dadurch wird es in der Nähe auf unmöglich, Änderungen zu mergen, wenn zwei Entwickler bei Migrationen aktivieren.

## <a name="screencasts"></a>Screencasts

Wenn Sie stattdessen sehen Sie sich einen Screencast als diesen Artikel lesen würde, behandelt die beiden folgenden Videos den gleichen Inhalt wie in diesem Artikel.

### <a name="video-one-migrations---under-the-hood"></a>Video 1: "Migrations - im Hintergrund"

[Dieser Screencast](http://channel9.msdn.com/blogs/ef/migrations-under-the-hood) befasst sich mit Migrationen wie nachverfolgt und Informationen über das Modell zum Erkennen von Änderungen des Datenmodells verwendet.

### <a name="video-two-migrations---team-environments"></a>Video 2: "Migrations - Teamumgebungen"

Erstellen auf den Konzepten von der früheren Video behandelt, [diesem Screencast](http://channel9.msdn.com/blogs/ef/migrations-team-environments) behandelt die Probleme, die in einer teamumgebung und zu deren Lösung auftreten.

## <a name="understanding-how-migrations-works"></a>Grundlegendes zur Funktionsweise von Migrationen

Der Schlüssel beim Verwenden von Migrationen erfolgreich in einer teamumgebung stellt ein grundlegendes Verständnis, wie Migrationen nachverfolgt und Informationen über das Modell zum Erkennen von Änderungen des Datenmodells verwendet.

### <a name="the-first-migration"></a>Die erste migration

Wenn Sie die erste Migration zu Ihrem Projekt hinzufügen, führen Sie etwa **Add-Migration erste** in Paket-Manager-Konsole. Die allgemeinen Schritte, die mit diesem Befehl wird, sind unten dargestellt.

![FirstMigration](~/ef6/media/firstmigration.png)

Das aktuelle Modell wird im Code (1) berechnet. Die erforderlichen Datenbankobjekte werden dann berechnet, indem das Modell unterscheiden (2) – da dies die erste Migration des Modells ist, unterscheiden sich lediglich verwendet ein leeres Modell für den Vergleich. Die erforderlichen Änderungen werden übergeben, um den Code-Generator, um den Code für eine Migration erforderlich (3) erstellen, die Visual Studio-Projektmappe (4) klicken Sie dann hinzugefügt wird.

Migrationen zusätzlich zu die eigentliche Migration-Code, der in der Haupt-Codedatei gespeichert wird, auch einige zusätzliche Code-Behind-Dateien generiert. Diese Dateien sind Metadaten, die von Migrationen verwendet wird, und es sind nicht etwas, die Sie bearbeiten sollte. Eine dieser Dateien ist eine Ressourcendatei (.resx), die eine Momentaufnahme des Modells zum Zeitpunkt enthält, die die Migration wurde generiert. Sie werden sehen, wie dies im nächsten Schritt verwendet wird.

An dieser Stelle würden Sie wahrscheinlich ausführen **Update-Database** gelten die Änderungen für die Datenbank, und wechseln dann zu anderen Bereichen Ihrer Anwendung implementieren.

### <a name="subsequent-migrations"></a>Nachfolgende Migrationen

Später zurückkehren, und einige Änderungen an Ihrem Modell vornehmen – in unserem Beispiel fügen wir eine **Url** Eigenschaft **Blog**. Klicken Sie dann geben Sie einen Befehl wie z. B. **Add-Migration AddUrl** Änderungen per Gerüstbau eine Migration zum Anwenden der entsprechenden Datenbank einrichten. Die allgemeinen Schritte, die mit diesem Befehl wird, sind unten dargestellt.

![SecondMigration](~/ef6/media/secondmigration.png)

Nur wie beim letzten Mal wird das aktuelle Modell von Code (1) berechnet. Aber dieses Mal sind vorhandene Migrationen, sodass das vorherige Modell aus der letzten Migration (2) abgerufen werden. Diese beiden Modelle sind mit die erforderlichen datenbankänderungen (3) finden, und klicken Sie dann den Vorgang abschließt wie zuvor.

Dieser Prozess wird für alle weiteren Migrationen verwendet, die Sie dem Projekt hinzu.

### <a name="why-bother-with-the-model-snapshot"></a>Wozu benötigen Sie die Momentaufnahme des Modells?

Sie vielleicht, warum draußen von EF mit der Momentaufnahme des Modells – Warum suchen Sie in der Datenbank nicht einfach. Wenn dies der Fall, lesen Sie weiter. Wenn Sie nicht interessiert sind, können Sie diesen Abschnitt überspringen.

Es gibt zahlreiche Gründe, aus denen EF die Momentaufnahme des Modells um behält:

-   Sie können Ihre Datenbank aus dem EF-Modell abweichen. Diese Änderungen können direkt in der Datenbank vorgenommen werden, oder Sie können den eingerüsteten Code ändern, in Ihrer Migrationen, um die Änderungen vorzunehmen. Hier sind einige Beispiele der in der Praxis:
    -   Ein eingefügter und aktualisierte Spalte an eine oder mehrere Tabellen hinzufügen möchten, aber keine dieser Spalten in der EF-Datenmodell enthalten sein sollen. Wenn die Datenbank Migrationen, die es ständig versuchen würde betrachtet, diese Spalten zu löschen, jedes Mal, wenn Sie eine Migration erstellt haben. Verwenden die Momentaufnahme des Modells, erkennt EF nur legitime Änderungen am Modell.
    -   Ändern Sie den Text einer gespeicherten Prozedur, die für Updates verwendet, um einige Protokollierung einschließen möchten. Wenn diese gespeicherte Prozedur aus der Datenbank Migrationen betrachtet kontinuierlich, wieder zurück auf die Definition, die von EF erwartet zurückgesetzt. Verwenden Sie die Momentaufnahme des Modells, Gerüst die EF wird immer nur Code, um die gespeicherte Prozedur zu ändern, wenn Sie die Form der Prozedur in der EF-Modell zu ändern.
    -   Diese gleichen Prinzipien gelten für Hinzufügen von zusätzlichen Indizes, einschließlich zusätzliche Tabellen in der Datenbank, zum Zuordnen von EF zu einer Datenbanksicht, die über eine Tabelle usw. befindet.
-   Das EF-Modell enthält mehr als nur die Form der Datenbank. Müssen das gesamte Modell kann die Migrationen zu Informationen über die Eigenschaften und Klassen in Ihrem Modell und wie sie die Spalten und Tabellen zugeordnet. Diese Informationen kann Migrationen intelligentere im Code sein, die sie erstellt das Gerüst für. Wenn Sie den Namen der Spalte ändern, die eine Eigenschaft zu Migrationen zuordnet können z. B. das Umbenennen von erkennen, sehen, dass es sich um die gleiche Eigenschaft – etwas, die erfolgen kann ist, wenn Sie nur das Schema der Datenbank verfügen. 

## <a name="what-causes-issues-in-team-environments"></a>Was bewirkt, dass Probleme in teamumgebungen

Der Workflow, die in den vorherigen Abschnitt funktioniert hervorragend behandelt werden, wenn Sie einen einzelnen Entwickler, die eine Anwendung sind. Dies funktioniert auch gut in einer teamumgebung, wenn Sie die einzige Person, die Änderungen an das Modell sind. In diesem Szenario können modelländerungen vornehmen, Migrationen zu generieren und diese an die quellcodeverwaltung zu senden. Andere Entwickler können Ihre Änderungen zu synchronisieren und ausführen **Update-Database** die schemaänderungen angewendet haben.

Probleme beginnen auftreten, wenn mehrere Entwickler Änderungen an das EF-Modell, und übermitteln zur quellcodeverwaltung, zur gleichen Zeit. Was verfügt nicht über die EF ist eine erstklassige Möglichkeit, Ihre lokalen Migrationen mit Migrationen zusammenzuführen, die ein anderer Entwickler in die quellcodeverwaltung gesendet hat, seit Sie zuletzt synchronisiert.

## <a name="an-example-of-a-merge-conflict"></a>Ein Beispiel für ein Zusammenführungskonflikt

Zuerst sehen wir uns diese eines Mergekonflikts ein konkretes Beispiel. Wir werden auf das Beispiel weiterhin, die, denen uns zuvor betrachtete. Als Ausgangspunkt zeigen wir davon aus, die Änderungen aus dem vorherigen Abschnitt wurden vom ursprünglichen Entwickler eingecheckt. Wir werden zwei Entwickler verfolgen, wie sie Änderungen an Code Codebasis vornehmen zu müssen.

Das EF-Modell und die Migrationen über eine Reihe von Änderungen nachverfolgt. Für einen Ausgangspunkt haben sowohl Entwickler als auch mit den Quellcodeverwaltungs-Repository, synchronisiert, wie in der folgenden Abbildung dargestellt ist.

!["Startingpoint"](~/ef6/media/startingpoint.png)

Entwickler \#1 und Entwickler \#2 jetzt nimmt einige Änderungen an das EF-Modell in ihrem lokalen Code Basis. Entwickler \#1 fügt eine **Bewertung** Eigenschaft **Blog** – und generiert eine **AddRating** Migration, um die Änderungen auf die Datenbank anzuwenden. Entwickler \#2 Fügt eine **Leser** Eigenschaft **Blog** – und generiert den entsprechenden **AddReaders** Migration. Führen Sie sowohl Entwickler als auch **Update-Database**, zum Anwenden der Änderungen auf ihren lokalen Datenbanken, und fahren die Entwicklung der Anwendung.

> [!NOTE]
> Migrationen werden mit dem Präfix mit einem Zeitstempel, damit, die die Grafik darstellt der AddReaders-Migration von Developer \#2 wird nach der Migration AddRating Entwickler \#1. Ob Entwickler \#1 oder \#2 generiert die erste Migration macht keinerlei Auswirkung auf die Lösung von Problemen in einem Team oder der Prozess zum Zusammenführen, die wir im nächsten Abschnitt betrachten werde.

![LocalChanges](~/ef6/media/localchanges.png)

Es ist ein Glück Tag für Entwickler \#1 wiedergibt, um ihre Änderungen zuerst zu übermitteln. Da es sich bei keinem anderen Benutzer eingecheckt wurde, da sie ihrem Repository synchronisiert, können sie einfach ihre Änderungen senden, ohne jede zusammenführen.

![Senden](~/ef6/media/submit.png)

Nun ist es Zeit für Entwickler \#2 zu übermitteln. Sie sind nicht so viel Glück. Da eine andere Person Änderungen übermittelt hat, da sie synchronisiert, müssen sie die Änderungen und Merge per pull abgerufen. Das Quellcodeverwaltungssystem wird wahrscheinlich in der Lage, die Änderungen auf der Codeebene automatisch zusammenzuführen, da sie sehr einfach sind. Der Status des Entwicklers \#2 des lokalen Repository nach der Synchronisierung in der folgenden Abbildung dargestellt ist. 

![Pull](~/ef6/media/pull.png)

Auf dieser Stufe Developer \#2 ausführen können **Update-Database** erkennt die neuen **AddRating** Migration (die wurde nicht angewendet. Entwickler \#2 Datenbank) und diese anzuwenden. Jetzt die **Bewertung** Spalte wird hinzugefügt, um die **Blogs** Tabelle und die Datenbank ist mit dem Modell.

Es gibt jedoch einige Probleme:

1.  Obwohl **Update-Database** gelten die **AddRating** Migration wird es auch eine Warnung ausgelöst: *kann nicht zum Aktualisieren der Datenbank, um das aktuelle Modell übereinstimmen, da es ausstehende Änderungen gibt und Automatische Migration wird deaktiviert...*
    Das Problem besteht darin, dass die Momentaufnahme des Modells in der letzten Migration gespeichert (**AddReader**) fehlt die **Bewertung** Eigenschaft **Blog** (da er Teil des Modells wurde nicht beim der Migration generiert wurde). Code erkennt zuerst an, dass das Modell in der letzten Migration nicht das aktuelle Modell entspricht und die Warnung auslöst.
2.  Ausführen der Anwendung führt zu einer InvalidOperationException, die besagt, dass "*des Modells, das den Kontext 'BloggingContext' unterstützt wurde geändert, da die Datenbank erstellt wurde. Erwägen Sie die Verwendung von Code First-Migrationen zum Aktualisieren der Datenbank..."*
    In diesem Fall wird das Problem der modellmomentaufnahme in der letzten Migration gespeichert, das aktuelle Modell entspricht.
3.  Schließlich erwarten wir, Ausführung **Add-Migration** jetzt generiert eine leere Migration (da es keine Änderungen auf die Datenbank anwenden sind). Aber da Migrationen vergleicht das aktuelle Modell auf den von der letzten Migration (die ist nicht vorhanden der **Bewertung** Eigenschaft) Es ist tatsächlich Erstellen des Gerüsts für eine andere **AddColumn** Aufruf zum Hinzufügen in der **Bewertung** Spalte. Natürlich würde diese Migration während fehlschlagen **Update-Database** da die **Bewertung** Spalte ist bereits vorhanden.

## <a name="resolving-the-merge-conflict"></a>Den Merge-Konflikt auflösen

Die gute Nachricht ist, dass es nicht zu schwierig, die den Merge manuell – Behandlung bereitgestellt haben Sie einen Überblick über die Funktionsweise von Migrationen. Wenn Sie direkt zu diesem Abschnitt übersprungen haben... Leider müssen Sie zurückkehren und den Rest dieses Artikels zunächst lesen!

Es gibt zwei Möglichkeiten, eine leere Migration zu generieren, das richtige aktuelle Modell als eine Momentaufnahme der einfachste Weg ist. Die zweite Option ist beim Aktualisieren der Momentaufnahme in die letzte Migration, damit die richtige Momentaufnahme des Modells. Die zweite Option ist etwas komplizierter und kann nicht in jedem Szenario verwendet werden, aber es ist auch übersichtlicher, da es nicht erforderlich ist, Hinzufügen einer zusätzlichen Migrations.

### <a name="option-1-add-a-blank-merge-migration"></a>Option 1: Hinzufügen einer leeren 'Merge'-Migrations

Bei dieser Option generiert eine leere Migration ausschließlich zum Zweck der sicherstellen, dass die neueste Migration verfügt über die Momentaufnahme des richtigen Modells darin gespeichert.

Diese Option kann unabhängig davon verwendet werden, die letzte Migration generiert hat. Im Beispiel wir haben Entwickler nach wurde \#2 zuständig ist die Zusammenführung aus, und sie aufgetreten sind, um die letzte Migration zu generieren. Aber die gleichen Schritte können verwendet werden, wenn Entwickler \#1 generiert, die letzte Migration. Die Schritte gelten auch, wenn mehrere livemigrationen beteiligt sind – wir nur zwei gesucht wurde, haben um einfach zu halten.

Für diesen Ansatz, beginnend ab dem Zeitpunkt, der Sie feststellen, dass Sie Änderungen vornehmen möchten, die aus der quellcodeverwaltung synchronisiert werden müssen, kann der folgende Prozess verwendet werden.

1.  Stellen Sie sicher, dass alle ausstehenden modelländerungen in Ihrer lokalen Codebasis für eine Migration geschrieben wurden. Dadurch wird sichergestellt, dass keine legitimen Änderungen nicht entgehen, wenn es Zeit, die zum Generieren der Migrations leer ist.
2.  Synchronisieren Sie mit der quellcodeverwaltung.
3.  Führen Sie **Update-Database** , alle neuen Migrationen anzuwenden, die andere Entwickler eingecheckt haben.
    **
    *Hinweis: *** Wenn Sie keine Warnungen, von der Update-Database-Befehl erhalten dann gab es keine neuen Migrationen von anderen Entwicklern und besteht keine Notwendigkeit zum Ausführen der weiter zusammenführen.*
4.  Führen Sie **Add-Migration &lt;auswählen\_eine\_Namen&gt; – IgnoreChanges** (z. B. **Merge Add-Migration – IgnoreChanges**). Dies erzeugt eine Migration mit dem alle Metadaten (einschließlich einer Momentaufnahme des aktuellen Modells), jedoch ignoriert, dass alle Änderungen, die erkennt, wenn das aktuelle Modell mit Momentaufnahme in den letzten Migrationen zu vergleichen (d. h., Sie erhalten ein leeres **einrichten** und **Unten** Methode).
5.  Weiterentwickeln, oder Senden an die quellcodeverwaltung (nachdem Ihre Komponententests ausführen von tests).

Hier ist der Status des Entwicklers \#2 des lokalen Codebasis nach der Verwendung dieses Ansatzes.

![MergeMigration](~/ef6/media/mergemigration.png)

### <a name="option-2-update-the-model-snapshot-in-the-last-migration"></a>Option 2: Aktualisieren der modellmomentaufnahme in der letzten migration

Diese Option ist sehr ähnlich, Option 1, aber die zusätzliche leere Migration – entfernt, da seien wir ehrlich, zusätzlichen Code-Dateien in ihre Projektmappe möchte.

**Dieser Ansatz ist nur möglich, wenn die neueste Migration ist nur in Ihrer lokalen Codebasis vorhanden und noch nicht, Datenquellen-Steuerelement übermittelt wurde (z. B., wenn die letzte Migration durch den Benutzer, die die Zusammenführung ausführen generiert wurde)**. Bearbeiten Sie die Metadaten für Migrationen, die andere Entwickler möglicherweise bereits in ihrer Entwicklungsdatenbank – oder sogar angewendet haben kann auf eine Produktionsdatenbank – noch schlimmer ist angewendet unerwartete Nebeneffekte führen. Während des Prozesses stellen wir zum Rollback für der letzten Migrations in der lokalen Datenbank, und wenden Sie es erneut mit der aktualisierten Metadaten.

Während die letzte Migration einfach muss sein, in der lokalen Codebasis bestehen keine Einschränkungen der Anzahl oder Reihenfolge der Migrationen, bei die sie fortfahren. Es können mehrere livemigrationen von mehreren anderen Entwicklern vorhanden sein und die gleichen Schritte gelten – wir haben nur zwei gesucht wurde, um einfach zu halten.

Für diesen Ansatz, beginnend ab dem Zeitpunkt, der Sie feststellen, dass Sie Änderungen vornehmen möchten, die aus der quellcodeverwaltung synchronisiert werden müssen, kann der folgende Prozess verwendet werden.

1.  Stellen Sie sicher, dass alle ausstehenden modelländerungen in Ihrer lokalen Codebasis für eine Migration geschrieben wurden. Dadurch wird sichergestellt, dass keine legitimen Änderungen nicht entgehen, wenn es Zeit, die zum Generieren der Migrations leer ist.
2.  Synchronisieren Sie mit dem Datenquellen-Steuerelement.
3.  Führen Sie **Update-Database** , alle neuen Migrationen anzuwenden, die andere Entwickler eingecheckt haben.
    **
    *Hinweis: *** Wenn Sie keine Warnungen, von der Update-Database-Befehl erhalten dann gab es keine neuen Migrationen von anderen Entwicklern und besteht keine Notwendigkeit zum Ausführen der weiter zusammenführen.*
4.  Führen Sie **Update-Database – TargetMigration &lt;zweite\_letzten\_Migration&gt;**  (im Beispiel wir haben nach wurde dies wäre **Update-Database: TargetMigration AddRating**). Diese Rollen, die die Datenbank wieder in den Zustand der zweiten der letzten Migration – effektiv "nicht anwenden" der letzten Migrations aus der Datenbank.
    **
    *Hinweis: *** Dieser Schritt ist erforderlich, um es sicher so bearbeiten Sie die Metadaten der Migration, da die Metadaten auch in gespeichert ist die \_ \_MigrationsHistoryTable der Datenbank. Daher ist diese Option nur verwenden sollten, ist die letzte Migration nur in der lokalen Codebasis. Wenn andere Datenbanken die letzte Migration angewendet haben, müssten Sie auch zum Rollback für sie und wenden Sie die letzte Migration zum Aktualisieren der Metadaten.* 
5.  Führen Sie **Add-Migration &lt;vollständige\_Namen\_einschließlich\_Zeitstempel\_von\_letzten\_Migration** &gt; (im Beispiel Wir haben folgende wurde dies wäre etwa **Add-Migration 201311062215252\_AddReaders**).
    **
    *Hinweis: *** müssen Sie die Zeitstempel enthalten, sodass Migrationen weiß, dass der Gerüstbau eine neue, anstatt die vorhandene Migration bearbeitet werden soll.*
    Dadurch werden die Metadaten für die letzte Migration das aktuelle Modell entsprechend aktualisiert. Sie erhalten die folgende Warnung, wenn der Befehl abgeschlossen ist, aber das ist genau das, erwünscht. "*Nur der Designer-Code für die Migration" 201311062215252\_AddReaders wurde erneut erstellten. Um die gesamte Migration erneut ein Gerüst erstellen, verwenden Sie den - Force-Parameter. "*
6.  Führen Sie **Update-Database** die neueste Migration mit den aktualisierten Metadaten erneut anwenden.
7.  Weiterentwickeln, oder Senden an die quellcodeverwaltung (nachdem Ihre Komponententests ausführen von tests).

Hier ist der Status des Entwicklers \#2 des lokalen Codebasis nach der Verwendung dieses Ansatzes.

![UpdatedMetadata](~/ef6/media/updatedmetadata.png)

## <a name="summary"></a>Zusammenfassung

Es sind einige Probleme auf, wenn Sie Code First-Migrationen in einer teamumgebung zu verwenden. Jedoch ein grundlegendes Verständnis der Funktionsweise von Migrationen und einige einfache Methoden zum Auflösen von Zusammenführungskonflikten erleichtern Ihnen diese Herausforderungen zu bewältigen.

Das grundlegende Problem ist nicht korrekt in die neueste Migration gespeicherten Metadaten. Dies bewirkt, dass Code First fälschlicherweise anzeigt, dass das aktuelle Modell und das Datenbankschema nicht übereinstimmen und falschen Code in der nächsten Migration erstellen. Diese Situation kann durch eine leere Migration mit dem richtigen Modell generieren oder Aktualisieren der Metadaten in der letzten Migration überwunden werden.
