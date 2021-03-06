---
title: Testen von Komponenten mit EF Core
description: Unterschiedliche Ansätze zum Testen von Anwendungen, die EF Core verwenden
author: ajcvickers
ms.date: 03/23/2020
uid: core/miscellaneous/testing/index
ms.openlocfilehash: b1ab37ebb0a3aae09d5d5b225f746cf83dfba170
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/07/2020
ms.locfileid: "80634250"
---
# <a name="testing-code-that-uses-ef-core"></a>Testen von Code, der EF Core verwendet

Das Testen von Code, der auf eine Datenbank zugreift, erfordert Folgendes:
* Das Ausführen von Abfragen und Aktualisierungen in dem Datenbanksystem, das auch in der Produktion verwendet wird, oder
* Das Ausführen von Abfragen und Aktualisierungen in einem andere einfacher zu verwaltenden Datenbanksystem oder
* Das Verwenden von Testdoubles oder eines anderen Mechanismus, um die Verwendung einer Datenbank grundsätzlich zu vermeiden

In diesem Dokument werden die Abwägungen dargelegt, die mit jeder dieser Optionen verbunden sind, und es wird gezeigt, wie EF Core bei jedem Ansatz eingesetzt werden kann.  

## <a name="all-database-providers-are-not-equal"></a>Alle Datenbankanbieter sind nicht gleich

Es ist sehr wichtig zu verstehen, dass EF Core nicht darauf ausgelegt ist, jeden Aspekt des zugrunde liegenden Datenbanksystems zu abstrahieren.
Stattdessen handelt es sich bei EF Core um einen einheitlichen Satz von Mustern und Konzepten, der bei jedem Datenbanksystem verwendet werden kann.
EF Core-Datenbankanbieter schichten dann datenbankspezifisches Verhalten und Funktionalität über diesem allgemeinen Framework.
Auf diese Weise kann jedes Datenbanksystem das tun, was es am besten kann, während gleichzeitig die Gemeinsamkeiten mit anderen Datenbanksystemen, falls angebracht, gewahrt bleiben. 

Grundsätzlich bedeutet dies, dass ein Austauschen des Datenbankanbieters das Verhalten von EF Core verändert, und es kann nicht erwartet werden, dass die Anwendung ordnungsgemäß funktioniert, wenn sie nicht explizit alle Unterschiede im Verhalten berücksichtigt.
Dennoch wird dies in vielen Fällen funktionieren, weil es ein hohes Maß an Gemeinsamkeiten unter relationalen Datenbanken gibt.
Das ist gut und schlecht zugleich.
Gut, da das Wechseln von Datenbanken relativ einfach erfolgen kann.
Schlecht, weil es ein falsches Gefühl der Sicherheit vermitteln kann, wenn die Anwendung nicht vollständig mit dem neuen Datenbanksystem getestet wird.  

## <a name="approach-1-production-database-system"></a>Ansatz 1: Produktions-Datenbanksystem

Wie im vorigen Abschnitt beschrieben, ist die einzige Möglichkeit, sicher zu sein, dass das getestet wird, was in der Produktion ausgeführt wird, die Verwendung desselben Datenbanksystems.
Wenn die bereitgestellte Anwendung beispielsweise SQL Azure verwendet, müssen die Tests auch mit SQL Azure erfolgen.

Allerdings wäre es sowohl langsam als auch teuer, wenn jeder Entwickler Tests mit SQL Azure durchführen würde, während er aktiv am Code arbeitet.
Dies veranschaulicht den wesentlichen Haken bei diesen Ansätzen: Wann ist es angebracht, vom Produktions-Datenbanksystem abzuweichen, um die Testeffizienz zu verbessern?

Glücklicherweise ist die Antwort in diesem Fall recht einfach, nämlich das Verwenden einer lokal SQL Server-Instanz für Entwicklertests.
SQL Azure und SQL Server sind sehr ähnlich, sodass das Testen mit SQL Server in der Regel ein angemessener Kompromiss ist.
Dennoch ist es ratsam, vor dem Aufnehmen der Produktion Tests mit SQL Azure selbst durchzuführen.
 
### <a name="localdb"></a>LocalDb 

Alle wichtigen Datenbanksysteme verfügen über eine Form von „Developer Edition“ für lokales Testen.
Bei SQL Server heißt dieses Feature [LocalDb](/sql/database-engine/configure-windows/sql-server-express-localdb?view=sql-server-ver15).
Der Hauptvorteil von LocalDb besteht darin, dass die Datenbankinstanz bei Bedarf hochgefahren wird.
Dadurch wird vermieden, dass ein Datenbankdienst auf Ihrem Computer läuft, auch wenn gerade keine Tests ausgeführt werden.

LocalDb ist nicht ohne Probleme:
* Sie unterstützt nicht alles, was von [SQL Server Developer Edition](/sql/sql-server/editions-and-components-of-sql-server-2016?view=sql-server-ver15) unterstützt wird.
* Sie ist unter Linux nicht verfügbar.
* Sie kann beim ersten Testlauf Verzögerungen verursachen, wenn der Dienst hochgefahren wird.

Ich persönlich habe es nie als Problem empfunden, dass ein Datenbankdienst auf meinem Entwicklungscomputer läuft, und würde generell empfehlen, stattdessen die Developer Edition zu verwenden.
Allerdings kann dies für manche Entwickler geeignet sein, insbesondere auf weniger leistungsfähigen Entwicklungscomputern.  

## <a name="approach-2-sqlite"></a>Ansatz 2: SQLite

EF Core testet den SQL Server-Anbieter hauptsächlich durch Ausführen in einer lokalen SQL Server-Instanz.
Bei diesen Tests werden in ein paar Minuten Zehntausende von Abfragen auf einem schnellen Computer ausgeführt.
Dies veranschaulicht, dass die Verwendung des realen Datenbanksystems eine leistungsstarke Lösung sein kann.
Es ist ein Mythos, dass die Verwendung einer schlankeren Datenbank die einzige Möglichkeit ist, Tests schnell durchzuführen.

Davon abgesehen, was ist, wenn Sie aus welchem Grund auch immer keine Tests mit etwas durchführen können, das Ihrem Produktions-Datenbanksystem nahe kommt?
Die nächstbeste Wahl ist die Verwendung von etwas mit ähnlicher Funktionalität.
Dies bedeutet in der Regel eine andere relationale Datenbank, wofür [SQLite](https://sqlite.org/index.html) die offensichtliche Wahl ist.

SQLite ist aus folgenden Gründen eine gute Wahl:
* Sie wird In-Process mit Ihrer Anwendung ausgeführt und hat daher einen geringen Verarbeitungsaufwand.
* Sie verwendet einfache, automatisch erstellte Dateien für Datenbanken und erfordert daher keine Datenbankverwaltung.
* Sie hat einen InMemory-Modus, der sogar die Dateierstellung vermeidet.

Beachten Sie jedoch Folgendes:
* SQLite unterstützt nicht zwangsläufig alles, was Ihr Produktions-Datenbanksystem leistet.
* SQLite verhält sich bei einigen Abfragen anders als Ihr Produktions-Datenbanksystem.

Wenn Sie also SQLite für einige Tests verwenden, stellen Sie sicher, dass Sie die Test auch mit Ihrem tatsächlichen Datenbanksystem durchführen.

Unter [Testen mit SQLite](xref:core/miscellaneous/testing/sqlite) finden Sie eine EF Core-spezifische Anleitung. 

## <a name="approach-3-the-ef-core-in-memory-database"></a>Ansatz 3: Die InMemory-Datenbank von EF Core

EF Core bietet eine InMemory-Datenbank, die wir für interne Tests von EF Core selbst verwenden.
Diese Datenbank ist im Allgemeinen **nicht als Ersatz für das Testen von Anwendungen geeignet, die EF Core verwenden**. Insbesondere gilt:
* Es handelt sich nicht um eine relationale Datenbank.
* Transaktionen werden nicht unterstützt.
* Sie ist nicht für Leistung optimiert.

Nichts davon ist beim Testen interner EF Core-Aspekte sehr wichtig, weil wir es speziell dort einsetzen, wo die Datenbank für den Test irrelevant ist.
Auf der anderen Seite sind diese Dinge beim Testen einer Anwendung, die EF Core verwendet, in der Regel sehr wichtig.

## <a name="unit-testing"></a>Komponententest

Erwägen Sie das Testen eines Teils der Geschäftslogik, der möglicherweise einige Daten aus einer Datenbank verwenden muss, aber nicht inhärent die Datenbankinteraktionen testet.
Eine Möglichkeit ist die Verwendung eines [Testdoubles](https://en.wikipedia.org/wiki/Test_double), z. B. eines simulierten oder nachgemachten Elements.

Wir verwenden Testdoubles für interne Tests von EF Core.
Wir versuchen jedoch nie, DbContext oder IQueryable zu simulieren.
Dies ist schwierig, umständlich und instabil.
**Machen Sie es nicht.**

Stattdessen verwenden wir die InMemory-Datenbank, wenn wir Komponententests mit etwas durchführen, das DbContext verwendet.
In diesem Fall ist die Verwendung der InMemory-Datenbank angemessen, da der Test nicht vom Datenbankverhalten abhängig ist.
Tun Sie dies bloß nicht, um tatsächliche Datenbankabfragen oder -aktualisierungen zu testen.   

Eine EF Core-spezifische Anleitung zur Verwendung der InMemory-Datenbank für Komponententests finden Sie unter [Testen mit dem InMemory-Anbieter](xref:core/miscellaneous/testing/in-memory).
