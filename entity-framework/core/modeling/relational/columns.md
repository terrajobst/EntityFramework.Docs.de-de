---
title: Spalten Zuordnung-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 05a47de9-1078-488e-a823-b516a4208f33
uid: core/modeling/relational/columns
ms.openlocfilehash: eaffc0cc1642f64edabeeef974f5f6de7a23b656
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197210"
---
# <a name="column-mapping"></a>Spaltenzuordnung

> [!NOTE]  
> Die Konfiguration in diesem Abschnitt gilt allgemein für relationale Datenbanken. Die hier gezeigten Erweiterungsmethoden werden verfügbar, wenn Sie einen relationalen Datenbankanbieter installieren (aufgrund des gemeinsam genutzten Pakets *Microsoft.EntityFrameworkCore.Relational*).

Die Spalten Zuordnung identifiziert, welche Spaltendaten aus der Datenbank abgefragt und in dieser gespeichert werden sollen.

## <a name="conventions"></a>Konventionen

Gemäß der Konvention wird jede Eigenschaft so eingerichtet, dass Sie einer Spalte mit dem gleichen Namen wie die Eigenschaft zugeordnet wird.

## <a name="data-annotations"></a>Datenanmerkungen

Sie können Daten Anmerkungen verwenden, um die Spalte zu konfigurieren, der eine Eigenschaft zugeordnet ist.

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Relational/Column.cs?highlight=13)]

## <a name="fluent-api"></a>Fluent-API

Sie können die fließende API verwenden, um die Spalte zu konfigurieren, der eine Eigenschaft zugeordnet ist.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/Column.cs?highlight=11-13)]
