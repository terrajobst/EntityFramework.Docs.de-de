---
title: "Datenbankanbieter – EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
ms.technology: entity-framework-core
uid: core/providers/index
ms.openlocfilehash: 19c275b7e89c62e79c8bded977e39b2cfb2b439a
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
---
# <a name="database-providers"></a>Datenbankanbieter

Entity Framework Core verwendet ein Anbietermodell, um dem EF den Zugriff auf viele verschiedene Datenbanken zu ermöglichen. Einige Konzepte gelten für die meisten Datenbanken und sind im Leistungsumfang der primären EF Core-Komponenten inbegriffen. Zu diesen Konzepten gehören das Ausdrücken von Abfragen in LINQ, Transaktionen und das Nachverfolgen von Änderungen an Objekten, nachdem sie aus der Datenbank geladen wurden. Einige Konzepte gelten speziell für einen bestimmten Anbieter. Der SQL Server-Anbieter ermöglicht es Ihnen beispielsweise, speicheroptimierte Tabellen (ein für SQL Server spezifisches Feature) zu konfigurieren. Andere Konzepte gelten speziell für eine Klasse von Anbietern. Beispielsweise bauen EF Core-Anbieter für relationale Datenbanken auf der gemeinsamen `Microsoft.EntityFrameworkCore.Relational`-Bibliothek auf, die u.a. APIs für die Konfiguration von Tabellen- und Spaltenzuordnungen und Fremdschlüsseleinschränkungen bereitstellt.

EF Core-Anbieter werden durch eine Vielzahl von Quellen erstellt. Nicht alle Anbieter werden im Rahmen des Entity Framework Core-Projekts verwaltet. Wenn Sie einen Drittanbieter in Betracht ziehen, sollten Sie Aspekte wie Qualität, Lizenzierung und Support auswerten, um sicherzustellen, dass dieser Ihren Anforderungen entspricht.
