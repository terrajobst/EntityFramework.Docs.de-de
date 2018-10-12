---
title: Funktionsweise von Abfragen – EF Core
author: rowanmiller
ms.date: 09/26/2018
ms.assetid: de2e34cd-659b-4cab-b5ed-7a979c6bf120
uid: core/querying/overview
ms.openlocfilehash: 23d26f9c0ac17fc0df744f5339946947ea366911
ms.sourcegitcommit: 15022dd06d919c29b1189c82611ea32f9fdc6617
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2018
ms.locfileid: "47415730"
---
# <a name="how-queries-work"></a>Funktionsweise von Abfragen

Entity Framework Core verwendet Language Integrated Query (LINQ), um Daten von der Datenbank abzufragen. LINQ ermöglicht Ihnen, mit C# (oder Ihrer bevorzugten .NET-Sprache) stark typisierte Abfragen basierend auf Ihrem abgeleiteten Kontext und Entitätsklassen zu schreiben.

## <a name="the-life-of-a-query"></a>Der Abfragezyklus

Nachstehend finden Sie eine grobe Übersicht über die verschiedenen Phasen des Abfragevorgangs.

1. Die LINQ-Abfrage wird von Entity Framework Core verarbeitet. Dabei wird eine Darstellung erstellt, die wiederum vom Datenbankanbieter verarbeitet wird.
   1. Das Ergebnis wird zwischengespeichert, damit dieser Vorgang nicht jedes Mal ausgeführt werden muss, wenn die Abfrage ausgeführt wird.
2. Das Ergebnis wird an den Datenbankanbieter übergeben.
   1. Der Datenbankanbieter ermittelt, welche Teile der Abfrage in der Datenbank ausgewertet werden können.
   2. Diese Teile der Abfrage werden in die datenbankspezifische Abfragesprache übersetzt, z.B. SQL für relationale Datenbanken.
   3. Mindestens eine Abfrage wird an die Datenbank gesendet, und das Resultset wird zurückgegeben (Ergebnisse sind Datenbankwerte, keine Entitätsinstanzen).
3. Für jedes Element im Resultset wird Folgendes ausgeführt:
   1. Wenn es sich um eine Überwachungsabfrage handelt, überprüft EF, ob die Daten eine Entität darstellen, die bereits in der Änderungsnachverfolgung für die Kontextinstanz vorhanden ist.
      * Wenn dies der Fall ist, wird die vorhandene Entität zurückgegeben.
      * Wenn dies nicht der Fall ist, wird eine neue Entität erstellt, die Änderungsnachverfolgung wird eingerichtet, und die neue Entität wird zurückgegeben.
   2. Wenn dies keine Überwachungsabfrage ist, überprüft EF, ob die Daten eine Entität darstellen, die bereits im Resultset der Abfrage vorhanden ist.
      * Wenn dies der Fall ist, wird die vorhandene Entität zurückgegeben.<sup>(1)</sup>
      * Wenn dies nicht der Fall ist, wird eine neue Entität erstellt und zurückgegeben.

<sup>(1)</sup> Abfragen, die keine Überwachungsabfragen sind, verwenden schwache Verweise, um Entitäten nachzuverfolgen, die bereits zurückgegeben wurden. Wenn ein vorheriges Ergebnis mit derselben Identität den gültigen Bereich verlässt und die Garbage Collection ausgeführt wird, können Sie eine neue Entitätsinstanz abrufen.

## <a name="when-queries-are-executed"></a>Ausführen von Abfragen

Wenn Sie LINQ-Operatoren aufrufen, erstellen Sie einfach eine speicherinterne Darstellung der Abfrage. Die Abfrage wird nur an die Datenbank gesendet, wenn die Ergebnisse verarbeitet werden.

Dies sind die üblichsten Vorgänge, aufgrund derer die Abfrage wird an die Datenbank gesendet wird:
* Durchlaufen die Ergebnisse in einer `for`-Schleife
* Verwenden eines Operators wie `ToList`, `ToArray`, `Single` oder `Count`
* Datenbindung der Ergebnisse einer Abfrage an eine Benutzeroberfläche

> [!WARNING]  
> **Benutzereingaben immer überprüfen:** Zwar schützt EF Core vor SQL-Einschleusungsangriffen durch Verwendung von Parametern und das Escapen von Literalen in Abfragen, es überprüft jedoch keine Eingaben. Geeignete Validierung gemäß den Anforderungen der Anwendung sollte erfolgen, bevor Werte aus nicht vertrauenswürdigen Quellen in LINQ-Abfragen verwendet, Entitätseigenschaften zugewiesen oder an andere EF Core-APIs übergeben werden. Dies schließt alle Benutzereingaben ein, mit denen Abfragen dynamisch erstellt werden. Wenn Sie Benutzereingaben zum Erstellen von Ausdrücken zulassen, müssen Sie selbst bei LINQ sicherstellen, dass nur beabsichtigte Ausdrücke erstellt werden können.
