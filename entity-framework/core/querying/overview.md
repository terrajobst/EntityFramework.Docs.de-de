---
title: Funktionsweise von Abfragen – EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: de2e34cd-659b-4cab-b5ed-7a979c6bf120
ms.technology: entity-framework-core
uid: core/querying/overview
ms.openlocfilehash: 1d28d215302625cf2b6788359527a93a77b7e9fd
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2018
ms.locfileid: "42447812"
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
> **Benutzereingaben immer überprüfen:** EF bietet zwar Schutz vor Angriffen durch Einschleusung von SQL-Befehlen, jedoch keine allgemeine Überprüfung der Eingabe. Stammen Werte, die z.B. an APIs übergeben, in LINQ-Abfragen verwendet oder Entitätseigenschaften zugewiesen werden, aus einer nicht vertrauenswürdigen Quelle, sollte gemäß den Anwendungsanforderungen eine angemessene Prüfung vorgenommen werden. Dies schließt alle Benutzereingaben ein, mit denen Abfragen dynamisch erstellt werden. Wenn Sie Benutzereingaben zum Erstellen von Ausdrücken zulassen, müssen Sie selbst bei LINQ sicherstellen, dass nur beabsichtigte Ausdrücke erstellt werden können.
