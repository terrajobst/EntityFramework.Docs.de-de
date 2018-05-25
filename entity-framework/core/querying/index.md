---
title: Abfragen von Daten – EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 7c65ec3e-46c8-48f8-8232-9e31f96c277b
ms.technology: entity-framework-core
uid: core/querying/index
ms.openlocfilehash: a2dd830b25c64b007a881c105a87b5c631b00266
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
---
# <a name="querying-data"></a><span data-ttu-id="0fea9-102">Abfrage von Daten</span><span class="sxs-lookup"><span data-stu-id="0fea9-102">Querying Data</span></span>

<span data-ttu-id="0fea9-103">Entity Framework Core verwendet Language Integrate Query (LINQ), um Daten von der Datenbank abzufragen.</span><span class="sxs-lookup"><span data-stu-id="0fea9-103">Entity Framework Core uses Language Integrate Query (LINQ) to query data from the database.</span></span> <span data-ttu-id="0fea9-104">LINQ ermöglicht Ihnen, mit C# (oder Ihrer bevorzugten .NET-Sprache) stark typisierte Abfragen basierend auf Ihrem abgeleiteten Kontext und Entitätsklassen zu schreiben.</span><span class="sxs-lookup"><span data-stu-id="0fea9-104">LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries based on your derived context and entity classes.</span></span> <span data-ttu-id="0fea9-105">Eine Darstellung der LINQ-Abfrage wird an den Datenbankanbieter übergeben und in eine datenbankspezifische Abfragesprache (z.B. SQL für eine relationale Datenbank) übersetzt.</span><span class="sxs-lookup"><span data-stu-id="0fea9-105">A representation of the LINQ query is passed to the database provider, to be translated in database-specific query language (e.g. SQL for a relational database).</span></span> <span data-ttu-id="0fea9-106">Ausführlichere Informationen zur Verarbeitung einer Abfrage finden Sie unter [Funktionsweise von Abfragen](overview.md).</span><span class="sxs-lookup"><span data-stu-id="0fea9-106">For more detailed information on how a query is processed, see [How Query Works](overview.md).</span></span>
