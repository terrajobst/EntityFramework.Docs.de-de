---
title: Fallstudien für Entitätsframework – EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: cd5d3ae3-717d-4095-a2ef-0e8fd72b1a2f
caps.latest.revision: 3
ms.openlocfilehash: 27c911799f957dd81a1866a3fd49e7f6cfa0059b
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2018
ms.locfileid: "39121372"
---
# <a name="microsoft-case-studies-for-entity-framework"></a>Microsoft Fallstudien für Entitätsframework
Fallstudien auf dieser Seite markieren Sie ein paar echten produktionsumgebung-Projekte, die Entity Framework eingesetzt wurden.
> [!NOTE]
> Die ausführlichen Versionen diese Fallstudien sind nicht mehr verfügbar ist, klicken Sie auf der Microsoft-Website. Aus diesem Grund wurden die Links entfernt.

## <a name="epicor"></a>Epicor
Epicor ist eine große globale Softwareunternehmen (mit mehr als 400 Entwickler), das entwickelt (Enterprise Resource Planning, ERP)-Lösungen für Unternehmen, die in mehr als 150 Ländern.
Hauptprodukt, Epicor 9, basiert auf einer dienstorientierten Architektur (SOA) mithilfe von .NET Framework.
Konfrontiert sind, mit zahlreichen Kundenanfragen zur Unterstützung von Language Integrated Query (LINQ), und möchten auch die Belastung ihrer Back-End-SQL-Server zu reduzieren, möchte, dass das Team auf Visual Studio 2010 und .NET Framework 4.0 zu aktualisieren.
Verwenden die Entity Framework 4.0, konnten sie diese Ziele erreichen und erheblich vereinfachen Sie Entwicklung und Wartung.
Insbesondere darf Entity Framework Unterstützung für umfangreiche T4 sie vollständige Kontrolle über ihre generierten Code und Leistung sparenden Features wie vorkompilierte Abfragen und Zwischenspeichern automatisch erstellen.

> "Wir haben einige Leistungstests vor kurzem mit vorhandenem Code, und wir konnten die Anforderungen an SQL Server 90 Prozent verringern.
Das ist aufgrund der ADO.NET Entity Framework 4." – Erik Johnson, Vice President, Produktforschung  

## <a name="veracity-solutions"></a>Lösungen für Richtigkeit
Erworben hat, haben eines veranstaltungsplanung Softwaresystems, das schwierig zu verwalten und über die langfristige erweitert würde, verwendet Richtigkeit Lösungen Visual Studio 2010, sie neu schreiben, wie ein leistungsfähiges und leicht zu bedienende Rich Internet Application über Silverlight 4 erstellt.
Verwenden .NET RIA-Dienste, konnten sie schnell eine Dienstschicht über Entity Framework zu erstellen, die Duplizierung von Code vermieden, und für allgemeine Validierung und die Authentifizierungslogik auf Ebenen zulässig.  

> "Wir verkauft wurden auf Entity Framework bei der es zuerst eingeführt wurde, und Entity Framework 4 erwiesenermaßen das noch besser werden.
Tools wird verbessert, und es ist einfacher, bearbeiten die EDMX-Dateien, die die konzeptionelle Modell, Speichermodell und Mapping zwischen diesen Modellen definieren... Mit dem Entity Framework, erhalte ich, Datenzugriffsebene, die Arbeit an einem Tag, und erstellen Sie ihn wieder zusammen.
Das Entity Framework ist unsere de-facto Datenzugriffsebene. Ich weiß nicht warum jeder Benutzer es verwenden, wäre nicht." – Joe McBride, Senior Developer

## <a name="nec-display-solutions-of-america"></a>NEC-Anzeige-Lösungen von Amerika
NEC wollte geben den Markt für digitale Ort basierende Werbung mit einer Lösung zu profitieren, Werbetreibende und Netzwerk-Besitzer und Ihre eigene Umsätze steigern.
Zu diesem Zweck ein Paar von Webanwendungen, die die manuelle Prozesse, die erforderlich sind, in einer herkömmlichen Ad Kampagne automatisieren gestartet.
Die Standorte wurden mithilfe von ASP.NET, Silverlight 3, AJAX und WCF, zusammen mit Entity Framework in der Datenzugriffsebene Kommunikation mit SQL Server 2008 erstellt.

> "Mit SQL Server, wir sind der Ansicht wir konnten den Durchsatz, mussten Sie dienen Werbetreibende und Netzwerke mit den Informationen in Echtzeit und die Zuverlässigkeit, um sicherzustellen, dass die Informationen in unseren unternehmenskritische Anwendungen immer verfügbar sein würde, abrufen" – Mike Corcoran, Director of IT

## <a name="darwin-dimensions"></a>Darwin Dimensionen
Verwenden eine Vielzahl von Microsoft-Technologien, legen das Team bei Darwin Evolver - ein Avatar von online-Portal erstellen, die Kunden zum Erstellen von beeindruckenden, immer immersivere Avatare für die Verwendung in Spielen, Animationen und soziale Netzwerke Seiten verwenden können.
Mit die Produktivitätsvorteile von Entity Framework und Komponenten wie Windows Workflow Foundation (WF) und Windows Server AppFabric (eine hochgradig skalierbare von in-Memory-Anwendungscache) einbezieht konnte das Team eine erstaunliche Produkt in 35 % bereitstellen Zeitpunkt der Entwicklung.
Trotz Teilen Teammitglieder in mehreren Ländern kann das Team einen Prozess beweglicher Entwicklung mit wöchentlichen Veröffentlichungen befolgen.

 > "Wir versuchen nicht, um Technologie um der Technologie willen zu erstellen. Als Startup ist es entscheidend, dass wir Technologie nutzen, die Zeit und Geld spart.
 .NET wurde die Auswahl für die schnelle, kostengünstige Entwicklung." – Zachary Olsen, Architekt  

## <a name="silverware"></a>Besteck
Legen mit mehr als 15 Jahren Erfahrung in der Entwicklung von POS-(POS)-Lösungen für kleine und mittelgroße Restaurant-Gruppen das Entwicklungsteam auf Besteck ihres Produkts durch zusätzliche Funktionen, auf Unternehmensebene zu verbessern, um größere gewinnen Restaurantketten.
Verwenden die neueste Version von Entwicklungstools von Microsoft, konnten sie die neue Projektmappe, die vier Mal schneller als je zuvor zu erstellen.
Wichtige neue Features wie LINQ und Entity Framework erleichtert Übergang von Crystal Reports zu SQL Server 2008 und SQL Server Reporting Services (SSRS) für die datenspeicherung und berichterstellungsanforderungen.

> "Effektive datenverwaltung ist der Schlüssel für den Erfolg des Besteck – und daher entschieden wir, SQL Reporting zu übernehmen." -Nicholas Romanidis, Director of IT / Software Engineering
