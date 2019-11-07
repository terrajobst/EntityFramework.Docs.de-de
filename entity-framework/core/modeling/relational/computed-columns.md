---
title: Berechnete Spalten-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9d81f06-805d-45c9-97c2-3546df654829
uid: core/modeling/relational/computed-columns
ms.openlocfilehash: 4e92ed6d785f3b7bdf54015101bdcddb9e4e0e1c
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655919"
---
# <a name="computed-columns"></a>Berechnete Spalten

> [!NOTE]  
> Die Konfiguration in diesem Abschnitt gilt allgemein für relationale Datenbanken. Die hier gezeigten Erweiterungsmethoden werden verfügbar, wenn Sie einen relationalen Datenbankanbieter installieren (aufgrund des gemeinsam genutzten Pakets *Microsoft.EntityFrameworkCore.Relational*).

Eine berechnete Spalte ist eine Spalte, deren Wert in der Datenbank berechnet wird. Für eine berechnete Spalte können andere Spalten in der Tabelle verwendet werden, um ihren Wert zu berechnen.

## <a name="conventions"></a>Konventionen

Gemäß der Konvention werden berechnete Spalten nicht im Modell erstellt.

## <a name="data-annotations"></a>Datenanmerkungen

Berechnete Spalten können nicht mit Daten Anmerkungen konfiguriert werden.

## <a name="fluent-api"></a>Fluent-API

Sie können die fließende API verwenden, um anzugeben, dass eine Eigenschaft einer berechneten Spalte zugeordnet werden soll.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/ComputedColumn.cs?name=ComputedColumn&highlight=9)]
