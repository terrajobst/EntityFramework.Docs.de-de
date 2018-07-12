---
title: Abfragen von Daten – EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 7c65ec3e-46c8-48f8-8232-9e31f96c277b
ms.technology: entity-framework-core
uid: core/querying/index
ms.openlocfilehash: 447f48b780bc48b7a79153d17dcc1b8ef0cc508c
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949274"
---
# <a name="querying-data"></a>Abfrage von Daten

Entity Framework Core verwendet Language Integrated Query (LINQ), um Daten von der Datenbank abzufragen. LINQ ermöglicht Ihnen, mit C# (oder Ihrer bevorzugten .NET-Sprache) stark typisierte Abfragen basierend auf Ihrem abgeleiteten Kontext und Entitätsklassen zu schreiben. Eine Darstellung der LINQ-Abfrage wird an den Datenbankanbieter übergeben und dort in eine datenbankspezifische Abfragesprache (z.B. SQL für eine relationale Datenbank) übersetzt. Ausführlichere Informationen zur Verarbeitung einer Abfrage finden Sie unter [Funktionsweise von Abfragen](overview.md).
