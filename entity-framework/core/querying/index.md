---
title: Abfragen von Daten – EF Core
author: smitpatel
ms.date: 10/03/2019
ms.assetid: 7c65ec3e-46c8-48f8-8232-9e31f96c277b
uid: core/querying/index
ms.openlocfilehash: 0e1e50d1a3f647d65301552d0a447f9fcae81438
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/07/2020
ms.locfileid: "78413115"
---
# <a name="querying-data"></a>Abfrage von Daten

Entity Framework Core verwendet Language Integrated Query (LINQ), um Daten von der Datenbank abzufragen. LINQ ermöglicht Ihnen, mit C# (oder Ihrer bevorzugten .NET-Sprache) stark typisierte Abfragen zu schreiben. Dabei werden der abgeleitete Kontext und Entitätsklassen verwendet, um auf Datenbankobjekte zu verweisen. EF Core übergibt eine Darstellung der LINQ-Abfrage an den Datenbankanbieter. Die Datenbankanbieter übersetzen diese dann in die datenbankspezifische Abfragesprache, z. B. SQL für relationale Datenbanken.

> [!TIP]
> Das in diesem Artikel verwendete [Beispiel](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) finden Sie auf GitHub.

Auf den folgenden Ausschnitten werden einige Beispiele für das Ausführen gängiger Aufgaben mit EF Core dargestellt.

## <a name="loading-all-data"></a>Laden aller Daten

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#LoadingAllData)]

## <a name="loading-a-single-entity"></a>Laden einer einzelnen Entität

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#LoadingSingleEntity)]

## <a name="filtering"></a>Filterung

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#Filtering)]

## <a name="further-readings"></a>Weiterführende Themen

- Weitere Informationen zu [LINQ-Abfrageausdrücken](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations)
- Ausführlichere Informationen zur Verarbeitung einer Abfrage finden Sie unter [Funktionsweise von Abfragen](xref:core/querying/how-query-works).
