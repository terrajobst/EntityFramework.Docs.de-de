---
title: Wie fragt Arbeit - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: de2e34cd-659b-4cab-b5ed-7a979c6bf120
ms.technology: entity-framework-core
uid: core/querying/overview
ms.openlocfilehash: 7fd2940d559f82016d7a8fc3fdcf3af0d5b8bc8f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
---
# <a name="how-queries-work"></a>Funktionsweise von Abfragen

Entity Framework Core verwendet Language integrieren Query (LINQ) zum Abfragen von Daten aus der Datenbank. LINQ ermöglicht Ihnen die Verwendung von c# (oder Ihre Sprache .NET) um stark typisierte Abfragen basierend auf Ihren abgeleiteten Klassen von Kontext und die Entität zu schreiben.

## <a name="the-life-of-a-query"></a>Die Lebensdauer einer Abfrage

Der folgende Code ist eine grobe Übersicht des Prozesses, der bei jeder Abfrage durchläuft.

1. Die LINQ-Abfrage wird vom Entity Framework Core eine Darstellung zu erstellen, die von dem Datenbankanbieter verarbeitet werden kann
   1. Das Ergebnis wird diese Verarbeitung muss nicht ausgeführt werden, jedes Mal, wenn die Abfrage ausgeführt wird, zwischengespeichert.
2. Das Ergebnis wird an den Datenbankanbieter übergeben.
   1. Der Datenbankanbieter identifiziert, welche Teile der Abfrage ausgewertet werden können, in der Datenbank
   2. Diese Teile der Abfrage werden in bestimmten Abfragesprache für Datenbank (z. B. SQL für eine relationale Datenbank) übersetzt.
   3. Eine oder mehrere Abfragen werden an die Datenbank und das zurückgegebene Resultset gesendet (Ergebnisse sind Werte aus der Datenbank, die keine Instanzen der Entität)
3. Für jedes Element im Resultset
   1. Ist dies einer Überwachungsabfrage, EF überprüft, wenn die Daten bereits in der änderungsnachverfolgung für die Kontextinstanz eine Entität darstellt
      * Wenn dies der Fall ist, wird die vorhandene Entität zurückgegeben.
      * Wenn dies nicht der Fall ist, wird eine neue Entität erstellt, änderungsnachverfolgung eingerichtet ist und die neue Entität wird zurückgegeben.
   2. Ist dies ein ohne-Überwachungsabfrage überprüft EF, wenn die Daten bereits in das Resultset für diese Abfrage eine Entität darstellt
      * Wenn also die vorhandene Entität zurückgegeben wird <sup>(1)</sup>
      * Wenn nicht, eine neue Entität erstellt und zurückgegeben wird

<sup>(1) </sup> Keine Überwachungsabfragen verwenden schwache Verweise zum Nachverfolgen Entitäten, die bereits zurückgegeben wurden. Wenn ein vorheriges Ergebnis mit derselben Identität den Gültigkeitsbereich verlässt, und die Garbagecollection ausführt, können Sie eine neue Entitätsinstanz abrufen.

## <a name="when-queries-are-executed"></a>Wenn Abfragen ausgeführt werden

Wenn Sie LINQ-Operatoren aufrufen, erstellen Sie einfach eine in-Memory-Darstellung der Abfrage. Die Abfrage wird nur an die Datenbank gesendet, wenn die Ergebnisse verarbeitet werden.

Die allgemeinsten Vorgänge, die in der Abfrage wird an die Datenbank gesendet werden:
* Durchlaufen die Ergebnisse in einem `for` Schleife
* Mit einem anderen Operator wie z. B. `ToList`, `ToArray`, `Single`,`Count`
* DataBinding die Ergebnisse einer Abfrage an eine Benutzeroberfläche

> [!WARNING]  
> **Überprüfen Sie immer alle Benutzereingaben:** während EF bietet Schutz vor SQL Injection-Angriffen, aber keine allgemeinen Überprüfung der Eingabe. Aus diesem Grund Werte, die APIs, die in LINQ-Abfragen zugewiesen Entitätseigenschaften usw., verwendet übergeben werden entsprechende Überprüfung pro den Anforderungen Ihrer Anwendung aus einer nicht vertrauenswürdigen Quelle stammt muss ausgeführt werden. Dies schließt alle Benutzereingaben verwendet, um Abfragen dynamisch zu erstellen. Selbst wenn LINQ verwendet, wenn Sie zulassen, dass Benutzereingaben, zum Erstellen von Ausdrücken als nur beabsichtigte Ausdrücke sicherzustellen, müssen Sie erstellt werden kann.
