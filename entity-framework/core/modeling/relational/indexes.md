---
title: Indizes - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
ms.technology: entity-framework-core
uid: core/modeling/relational/indexes
ms.openlocfilehash: f577fccfefc6908edf2ac47ae630323d7a9f5f2b
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="indexes"></a>Indizes

> [!NOTE]  
> Die Konfiguration in diesem Abschnitt gilt allgemein für relationale Datenbanken. Die hier gezeigten Erweiterungsmethoden werden verfügbar, wenn Sie einen relationalen Datenbankanbieter installieren (aufgrund des gemeinsam genutzten Pakets *Microsoft.EntityFrameworkCore.Relational*).

Ein Index in einer relationalen Datenbank ordnet dasselbe Konzept wie ein Index im Kern des Entity Framework.

## <a name="conventions"></a>Konventionen

Indizes werden gemäß der Konvention benannt `IX_<type name>_<property name>`. Bei zusammengesetzten Indizes `<property name>` wird eine Unterstrich getrennte Liste von Eigenschaftsnamen.

## <a name="data-annotations"></a>Datenanmerkungen

Indizes können nicht mithilfe von Datenanmerkungen konfiguriert werden.

## <a name="fluent-api"></a>Fluent-API

So konfigurieren Sie den Namen eines Indexes können Sie die Fluent-API verwenden.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexName.cs?name=Model&highlight=9)]

Sie können auch einen Filter angeben.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexFilter.cs?name=Model&highlight=9)]

Fügt mithilfe des SQL Server-Anbieters EF filtern Sie bei einem "IS NOT NULL" für alle Spalten, die Teil eines eindeutigen Indexes sind. Diese Konvention überschreiben, Sie angeben können, ein `null` Wert.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexNoFilter.cs?name=Model&highlight=10)]
