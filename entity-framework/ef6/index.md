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
# <a name="entity-framework-6-quick-overview"></a><span data-ttu-id="01eb5-102">Entity Framework 6: Kurze Übersicht</span><span class="sxs-lookup"><span data-stu-id="01eb5-102">Entity Framework 6 Quick Overview</span></span>

<span data-ttu-id="01eb5-103">Entity Framework 6 (EF6) ist ein bewährtes Framework für objektrelationale Abbildung (Object-Relational Mapping, ORM) für .NET, dessen Features im Laufe der Zeit immer weiter optimiert und stabilisiert wurden.</span><span class="sxs-lookup"><span data-stu-id="01eb5-103">Entity Framework 6 (EF6) is a tried and tested object-relational mapper (O/RM) for .NET with many years of feature development and stabilization.</span></span>

<span data-ttu-id="01eb5-104">EF6 implementiert viele beliebte ORM-Features:</span><span class="sxs-lookup"><span data-stu-id="01eb5-104">EF6 implements many popular O/RM features:</span></span>
- <span data-ttu-id="01eb5-105">Mapping von Entitätsklassen, die die Persistenz ignorieren (sogenannte POCOs, Plain Old CLR Objects) und von keinen EF-Typen abhängen</span><span class="sxs-lookup"><span data-stu-id="01eb5-105">Mapping of "persistence ignorant" (also known as "POCO", for Plan-Old CLR Object) entity classes which do not depend on any EF types</span></span>
- <span data-ttu-id="01eb5-106">Automatische Änderungsnachverfolgung</span><span class="sxs-lookup"><span data-stu-id="01eb5-106">Automatic change tracking</span></span>
- <span data-ttu-id="01eb5-107">Identitätsauflösung und Arbeitseinheit</span><span class="sxs-lookup"><span data-stu-id="01eb5-107">Identity resolution and Unit of Work</span></span>
- <span data-ttu-id="01eb5-108">Eager und Lazy Loading sowie explizites Laden</span><span class="sxs-lookup"><span data-stu-id="01eb5-108">Eager, lazy and explicit loading</span></span>
- <span data-ttu-id="01eb5-109">Übersetzung stark typisierter Abfragen mit LINQ (Language Integrated Query)</span><span class="sxs-lookup"><span data-stu-id="01eb5-109">Translation of strongly-typed queries using LINQ (Language INtegrated Query)</span></span> 
- <span data-ttu-id="01eb5-110">Umfangreiche Mappingfunktionen, u.a. Unterstützung für:</span><span class="sxs-lookup"><span data-stu-id="01eb5-110">Rich mapping capabilities, including support for:</span></span>
  - <span data-ttu-id="01eb5-111">Vererbung (Tabelle pro Hierarchie, Tabelle pro Typ und Tabelle pro konkreter Klasse)</span><span class="sxs-lookup"><span data-stu-id="01eb5-111">Inheritance (table per hierarchy, table per type and table per concrete class)</span></span>
  - <span data-ttu-id="01eb5-112">Komplexe Typen</span><span class="sxs-lookup"><span data-stu-id="01eb5-112">Complex types</span></span>
  - <span data-ttu-id="01eb5-113">Gespeicherte Prozeduren</span><span class="sxs-lookup"><span data-stu-id="01eb5-113">Stored procedures</span></span>
- <span data-ttu-id="01eb5-114">Ein visueller Designer zum Erstellen von Entitätsmodellen.</span><span class="sxs-lookup"><span data-stu-id="01eb5-114">A visual designer to create entity models.</span></span>
- <span data-ttu-id="01eb5-115">Ein Code First-Prozess, der das Erstellen von Entitätsmodellen durch das Schreiben von Code unterstützt.</span><span class="sxs-lookup"><span data-stu-id="01eb5-115">A "Code First" experience that supports creating entity models by writing code.</span></span>
- <span data-ttu-id="01eb5-116">Modelle können entweder anhand vorhandener Datenbanken erstellt und dann manuell bearbeitet werden, oder Sie können sie komplett neu erstellen und dann zum Erstellen neuer Datenbanken verwenden.</span><span class="sxs-lookup"><span data-stu-id="01eb5-116">Models can either be generated form existing databases and then hand-edited, or they can be created from scratch and then used to generate new databases.</span></span>
- <span data-ttu-id="01eb5-117">Integration in .NET Framework-Anwendungsmodelle, ASP.NET eingeschlossen, und anhand von Datenbindung mit WPF und WinForms.</span><span class="sxs-lookup"><span data-stu-id="01eb5-117">Integration with .NET Framework application models, including ASP.NET, and through databinding, with WPF and WinForms.</span></span>
- <span data-ttu-id="01eb5-118">Datenbankkonnektivität basierend auf ADO.NET und zahlreichen anderen Anbietern, die eine Verbindung mit SQL Server, Oracle, MySQL, SQLite, PostgreSQL, DB2 usw. herstellen.</span><span class="sxs-lookup"><span data-stu-id="01eb5-118">Database connectivity based on ADO.NET and numerous providers available to connect to SQL Server, Oracle, MySQL, SQLite, PostgreSQL, DB2, etc.</span></span>

<span data-ttu-id="01eb5-119">Als ORM-Framework senkt EF6 die Inkongruenz zwischen dem relationalen und dem objektorientierten Bereich. Dadurch können Entwickler Anwendungen schreiben, die mit Daten interagieren, die in relationalen Datenbanken mit stark typisierten .NET-Objekten gespeichert wurden, die die Anwendungsdomäne darstellen. Außerdem ist es durch EF6 nicht mehr erforderlich, dass ein großer Teil der Daten auf Grundlagencode zugreifen muss, den sie normalerweise schreiben müssen.</span><span class="sxs-lookup"><span data-stu-id="01eb5-119">As an O/RM, EF6 reduces the impedance mismatch between the relational and object-oriented worlds, enabling developers to write applications that interact with data stored in relational databases using strongly-typed .NET objects that represent the application's domain, and eliminating the need for a large portion of the data access "plumbing" code that they usually need to write.</span></span>

## <a name="should-i-use-ef6-or-ef-core"></a><span data-ttu-id="01eb5-120">Sollte ich EF6 oder EF Core verwenden?</span><span class="sxs-lookup"><span data-stu-id="01eb5-120">Should I use EF6 or EF Core?</span></span>

<span data-ttu-id="01eb5-121">EF Core ist eine modernere, einfache und erweiterbare Version von Entity Framework mit ähnlichen Funktionen wie EF6 und einigen Vorteilen.</span><span class="sxs-lookup"><span data-stu-id="01eb5-121">EF Core is a more modern, lightweight and extensible version of Entity Framework that has very similar capabilities and benefits to EF6.</span></span>
<span data-ttu-id="01eb5-122">EF Core wurde vollständig neu geschrieben und enthält viele neue Features, die es in EF6 nicht gibt. Andererseits fehlen die komplexesten Mappingfeatures, die in EF6 vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="01eb5-122">EF Core is a complete rewrite and contains many new features not available in EF6, although it also still lacks some of the most advanced mapping capabilities of EF6.</span></span>
<span data-ttu-id="01eb5-123">Es wird empfohlen, EF Core in neuen Anwendungen zu verwenden, wenn die enthaltenen Features Ihren Anforderungen entsprechen.</span><span class="sxs-lookup"><span data-stu-id="01eb5-123">We recommend using EF Core in new applications as long as the feature set matches your requirements.</span></span>
<span data-ttu-id="01eb5-124">Unter [Vergleichen von EF Core und EF 6](xref:efcore-and-ef6/index) wird darauf ausführlicher eingegangen.</span><span class="sxs-lookup"><span data-stu-id="01eb5-124">[Compare EF Core & EF6](xref:efcore-and-ef6/index) examines this choice in greater detail.</span></span>

## <a name="get-startedef6get-startedmd"></a>[<span data-ttu-id="01eb5-125">Erste Schritte</span><span class="sxs-lookup"><span data-stu-id="01eb5-125">Get Started</span></span>](~/ef6/get-started.md)

<span data-ttu-id="01eb5-126">Fügen Sie Ihrem Projekt das EntityFramework-NuGet-Paket hinzu, oder installieren Sie Entity Framework Tools für Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="01eb5-126">Add the EntityFramework NuGet package to your project or install the Entity Framework Tools for Visual Studio.</span></span> <span data-ttu-id="01eb5-127">Sehen Sie sich anschließend Videos an, lesen Sie Tutorials und weiterführende Dokumentationsartikel, sodass Sie die Funktionen von Entity Framework 6 voll ausschöpfen können.</span><span class="sxs-lookup"><span data-stu-id="01eb5-127">Then watch videos, read tutorials, and advanced documentation to help you make the most of Entity Framework 6.</span></span>

## <a name="past-entity-framework-versions"></a><span data-ttu-id="01eb5-128">Frühere Versionen von Entity Framework</span><span class="sxs-lookup"><span data-stu-id="01eb5-128">Past Entity Framework Versions</span></span>

<span data-ttu-id="01eb5-129">Dies ist die Dokumentation für die aktuelle Version von Entity Framework 6. Viele der Punkte gelten jedoch auch für ältere Releases.</span><span class="sxs-lookup"><span data-stu-id="01eb5-129">This is the documentation for the latest version of Entity Framework 6, although much of it also applies to past releases.</span></span>
<span data-ttu-id="01eb5-130">Unter [Neues](~/ef6/what-is-new/index.md) und [Frühere Releases](~/ef6/what-is-new/past-releases.md) finden Sie eine vollständige Liste der EF-Releases mit dazugehörigen neuen Features.</span><span class="sxs-lookup"><span data-stu-id="01eb5-130">Check out [What's New](~/ef6/what-is-new/index.md) and [Past Releases](~/ef6/what-is-new/past-releases.md) for a complete list of EF releases and the features they introduced.</span></span>
