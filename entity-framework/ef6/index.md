---
title: 'Entity Framework 6: Kurze Übersicht'
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 8ae74d63-6bad-4686-b325-bbf9d68f3743
caps.latest.revision: 5
uid: ef6/index
ms.openlocfilehash: df661f19afdeef53257c86bdd32b1444737c9b0a
ms.sourcegitcommit: 45494121254ad4fdcec613d1dd22d850068d6f39
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2018
ms.locfileid: "37913498"
---
# <a name="entity-framework-6-quick-overview"></a>Entity Framework 6: Kurze Übersicht

Entity Framework 6 (EF6) ist ein bewährtes Framework für objektrelationale Abbildung (Object-Relational Mapping, ORM) für .NET, dessen Features im Laufe der Zeit immer weiter optimiert und stabilisiert wurden.

EF6 implementiert viele beliebte ORM-Features:
- Mapping von Entitätsklassen, die die Persistenz ignorieren (sogenannte POCOs, Plain Old CLR Objects) und von keinen EF-Typen abhängen
- Automatische Änderungsnachverfolgung
- Identitätsauflösung und Arbeitseinheit
- Eager und Lazy Loading sowie explizites Laden
- Übersetzung stark typisierter Abfragen mit LINQ (Language Integrated Query) 
- Umfangreiche Mappingfunktionen, u.a. Unterstützung für:
  - Vererbung (Tabelle pro Hierarchie, Tabelle pro Typ und Tabelle pro konkreter Klasse)
  - Komplexe Typen
  - Gespeicherte Prozeduren
- Ein visueller Designer zum Erstellen von Entitätsmodellen.
- Ein Code First-Prozess, der das Erstellen von Entitätsmodellen durch das Schreiben von Code unterstützt.
- Modelle können entweder anhand vorhandener Datenbanken erstellt und dann manuell bearbeitet werden, oder Sie können sie komplett neu erstellen und dann zum Erstellen neuer Datenbanken verwenden.
- Integration in .NET Framework-Anwendungsmodelle, ASP.NET eingeschlossen, und anhand von Datenbindung mit WPF und WinForms.
- Datenbankkonnektivität basierend auf ADO.NET und zahlreichen anderen Anbietern, die eine Verbindung mit SQL Server, Oracle, MySQL, SQLite, PostgreSQL, DB2 usw. herstellen.

Als ORM-Framework senkt EF6 die Inkongruenz zwischen dem relationalen und dem objektorientierten Bereich. Dadurch können Entwickler Anwendungen schreiben, die mit Daten interagieren, die in relationalen Datenbanken mit stark typisierten .NET-Objekten gespeichert wurden, die die Anwendungsdomäne darstellen. Außerdem ist es durch EF6 nicht mehr erforderlich, dass ein großer Teil der Daten auf Grundlagencode zugreifen muss, den sie normalerweise schreiben müssen.

## <a name="should-i-use-ef6-or-ef-core"></a>Sollte ich EF6 oder EF Core verwenden?

EF Core ist eine modernere, einfache und erweiterbare Version von Entity Framework mit ähnlichen Funktionen wie EF6 und einigen Vorteilen.
EF Core wurde vollständig neu geschrieben und enthält viele neue Features, die es in EF6 nicht gibt. Andererseits fehlen die komplexesten Mappingfeatures, die in EF6 vorhanden sind.
Es wird empfohlen, EF Core in neuen Anwendungen zu verwenden, wenn die enthaltenen Features Ihren Anforderungen entsprechen.
Unter [Vergleichen von EF Core und EF 6](xref:efcore-and-ef6/index) wird darauf ausführlicher eingegangen.

## <a name="get-startedef6get-startedmd"></a>[Erste Schritte](~/ef6/get-started.md)

Fügen Sie Ihrem Projekt das EntityFramework-NuGet-Paket hinzu, oder installieren Sie Entity Framework Tools für Visual Studio. Sehen Sie sich anschließend Videos an, lesen Sie Tutorials und weiterführende Dokumentationsartikel, sodass Sie die Funktionen von Entity Framework 6 voll ausschöpfen können.

## <a name="past-entity-framework-versions"></a>Frühere Versionen von Entity Framework

Dies ist die Dokumentation für die aktuelle Version von Entity Framework 6. Viele der Punkte gelten jedoch auch für ältere Releases.
Unter [Neues](~/ef6/what-is-new/index.md) und [Frühere Releases](~/ef6/what-is-new/past-releases.md) finden Sie eine vollständige Liste der EF-Releases mit dazugehörigen neuen Features.
