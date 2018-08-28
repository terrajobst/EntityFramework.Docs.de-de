---
title: Abfragen von Daten – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 7c65ec3e-46c8-48f8-8232-9e31f96c277b
uid: core/querying/index
ms.openlocfilehash: 51aaa5de11d3fe38b4fba82db8dcb5658088cc27
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993533"
---
# <a name="querying-data"></a><span data-ttu-id="32f28-102">Abfrage von Daten</span><span class="sxs-lookup"><span data-stu-id="32f28-102">Querying Data</span></span>

<span data-ttu-id="32f28-103">Entity Framework Core verwendet Language Integrated Query (LINQ), um Daten von der Datenbank abzufragen.</span><span class="sxs-lookup"><span data-stu-id="32f28-103">Entity Framework Core uses Language Integrated Query (LINQ) to query data from the database.</span></span> <span data-ttu-id="32f28-104">LINQ ermöglicht Ihnen, mit C# (oder Ihrer bevorzugten .NET-Sprache) stark typisierte Abfragen basierend auf Ihrem abgeleiteten Kontext und Entitätsklassen zu schreiben.</span><span class="sxs-lookup"><span data-stu-id="32f28-104">LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries based on your derived context and entity classes.</span></span> <span data-ttu-id="32f28-105">Eine Darstellung der LINQ-Abfrage wird an den Datenbankanbieter übergeben und dort in eine datenbankspezifische Abfragesprache (z.B. SQL für eine relationale Datenbank) übersetzt.</span><span class="sxs-lookup"><span data-stu-id="32f28-105">A representation of the LINQ query is passed to the database provider, to be translated in database-specific query language (for example, SQL for a relational database).</span></span> <span data-ttu-id="32f28-106">Ausführlichere Informationen zur Verarbeitung einer Abfrage finden Sie unter [Funktionsweise von Abfragen](overview.md).</span><span class="sxs-lookup"><span data-stu-id="32f28-106">For more detailed information on how a query is processed, see [How Query Works](overview.md).</span></span>
