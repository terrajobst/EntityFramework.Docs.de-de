---
title: Alternativschlüssel (Unique-Einschränkungen) – EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 3d419dcf-2b5d-467c-b408-ea03d830721a
ms.technology: entity-framework-core
uid: core/modeling/relational/unique-constraints
ms.openlocfilehash: 1b7e2bef6ede95f8c27211ba00dcc6b97cccde9b
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052790"
---
# <a name="alternate-keys-unique-constraints"></a>Alternativschlüssel (Unique-Einschränkungen)

> [!NOTE]  
> Die Konfiguration in diesem Abschnitt ist im Allgemeinen gilt für relationale Datenbanken. Die Erweiterungsmethoden, die hier gezeigten werden verfügbar, wenn Sie einen relationale Datenbank-Anbieter installieren (aufgrund der freigegebenen *Microsoft.EntityFrameworkCore.Relational* Paket).

Eine unique-Einschränkung wird für jede Alternativschlüssel im Modell eingeführt.

## <a name="conventions"></a>Konventionen

Gemäß der Konvention werden die Index- und einschränkungsobjekten, die für einen alternativen Schlüssel eingeführt werden Namen `AK_<type name>_<property name>`. Für zusammengesetzte alternativen Schlüssel `<property name>` wird eine Unterstrich getrennte Liste von Eigenschaftsnamen.

## <a name="data-annotations"></a>Datenanmerkungen

Unique-Einschränkungen können nicht mithilfe von Datenanmerkungen konfiguriert werden.

## <a name="fluent-api"></a>Fluent-API

Sie können die Fluent-API verwenden, so konfigurieren Sie den Index und Namen für einen alternativen Schlüssel.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/AlternateKeyName.cs?name=Model&highlight=9)]
