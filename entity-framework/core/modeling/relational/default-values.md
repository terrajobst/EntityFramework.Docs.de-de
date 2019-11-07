---
title: Standardwerte-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e541366a-130f-47dd-9997-1b110a11febe
uid: core/modeling/relational/default-values
ms.openlocfilehash: b6ac283d551e2c6ee119f7de6933363b5d8793a1
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655906"
---
# <a name="default-values"></a>Standardwerte

> [!NOTE]  
> Die Konfiguration in diesem Abschnitt gilt allgemein für relationale Datenbanken. Die hier gezeigten Erweiterungsmethoden werden verfügbar, wenn Sie einen relationalen Datenbankanbieter installieren (aufgrund des gemeinsam genutzten Pakets *Microsoft.EntityFrameworkCore.Relational*).

Der Standardwert einer Spalte ist der Wert, der eingefügt wird, wenn eine neue Zeile eingefügt wird, aber für die Spalte kein Wert angegeben wird.

## <a name="conventions"></a>Konventionen

Gemäß der Konvention ist kein Standardwert konfiguriert.

## <a name="data-annotations"></a>Datenanmerkungen

Mithilfe von Daten Anmerkungen können Sie keinen Standardwert festlegen.

## <a name="fluent-api"></a>Fluent-API

Sie können die fließende API verwenden, um den Standardwert für eine Eigenschaft anzugeben.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/DefaultValue.cs?name=DefaultValue&highlight=9)]

Sie können auch ein SQL-Fragment angeben, das verwendet wird, um den Standardwert zu berechnen.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/DefaultValueSql.cs?name=DefaultValueSql&highlight=9)]
