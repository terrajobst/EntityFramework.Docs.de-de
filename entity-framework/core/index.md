---
title: Überblick über Entity Framework Core – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
uid: core/index
ms.openlocfilehash: e6127f775d6bbbdf81debf5519388fe252fe079d
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655626"
---
# <a name="entity-framework-core"></a>Entity Framework Core

Entity Framework Core (EF Core) ist eine einfache, erweiterbare und plattformübergreifende [Open Source](https://github.com/aspnet/EntityFrameworkCore)-Version der beliebten Entity Framework-Datenzugriffstechnologie.

EF Core kann als objektrelationaler Mapper (O/RM) eingesetzt werden und bietet .NET-Entwicklern so die Möglichkeit, unter Verwendung von .NET-Objekten mit einer Datenbank zu arbeiten. Auf diese Weise entfällt ein Großteil des Datenzugriffscodes, der üblicherweise geschrieben werden muss.

Einzelheiten zu den von EF Core unterstützten Datenbank-Engines finden Sie unter [Datenbankanbieter](providers/index.md).

## <a name="the-model"></a>Das Modell

Bei EF Core erfolgt der Datenzugriff über ein Modell. Ein Modell setzt sich aus Entitätsklassen und einem Kontextobjekt zusammen, das eine Sitzung mit der Datenbank darstellt und Ihnen das Abfragen und Speichern von Daten ermöglicht. Weitere Informationen finden Sie unter [Erstellen eines Modells](modeling/index.md).

Sie können ein Modell aus einer vorhandenen Datenbank generieren, ein Modell manuell entsprechend Ihrer Datenbank codieren oder mithilfe von [EF-Migrationen](managing-schemas/migrations/index.md) eine Datenbank anhand Ihres Modells erstellen und sie im Laufe der Zeit gemäß Ihres Modells weiterentwickeln.

[!code-csharp[Main](../../samples/core/Intro/Model.cs)]

## <a name="querying"></a>Abfragen

Instanzen Ihrer Entitätsklassen werden mit Language Integrated Query (LINQ) von der Datenbank abgerufen. Weitere Informationen finden Sie unter [Abfragen von Daten](querying/index.md).

[!code-csharp[Main](../../samples/core/Intro/Program.cs#Querying)]

## <a name="saving-data"></a>Speichern von Daten

Daten werden in der Datenbank mithilfe von Instanzen Ihrer Entitätsklassen erstellt, gelöscht und geändert. Weitere Informationen finden Sie unter [Speichern von Daten](saving/index.md).

[!code-csharp[Main](../../samples/core/Intro/Program.cs#SavingData)]

## <a name="next-steps"></a>Nächste Schritte

Einführungstutorials finden Sie unter [Erste Schritte mit Entity Framework Core](get-started/index.md).
