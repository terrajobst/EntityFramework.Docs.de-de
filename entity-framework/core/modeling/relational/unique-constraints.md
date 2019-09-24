---
title: Alternative Schlüssel (Unique-Einschränkungen)-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3d419dcf-2b5d-467c-b408-ea03d830721a
uid: core/modeling/relational/unique-constraints
ms.openlocfilehash: 7afcb804aeeccbd5e07c228a8fd9850ca00a2919
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197617"
---
# <a name="alternate-keys-unique-constraints"></a>Alternativschlüssel (Unique-Einschränkungen)

> [!NOTE]  
> Die Konfiguration in diesem Abschnitt gilt allgemein für relationale Datenbanken. Die hier gezeigten Erweiterungsmethoden werden verfügbar, wenn Sie einen relationalen Datenbankanbieter installieren (aufgrund des gemeinsam genutzten Pakets *Microsoft.EntityFrameworkCore.Relational*).

Eine Unique-Einschränkung wird für jeden alternativen Schlüssel im Modell eingeführt.

## <a name="conventions"></a>Konventionen

Gemäß der Konvention werden der Index und die Einschränkung, die für einen alternativen Schlüssel eingeführt werden `AK_<type name>_<property name>`, benannt. Bei zusammengesetzten alternativen `<property name>` Schlüsseln wird eine durch Trennzeichen getrennte Liste mit Eigenschaftsnamen.

## <a name="data-annotations"></a>Datenanmerkungen

Unique-Einschränkungen können nicht mithilfe von Daten Anmerkungen konfiguriert werden.

## <a name="fluent-api"></a>Fluent-API

Sie können die fließende API verwenden, um den Index-und Einschränkungs Namen für einen alternativen Schlüssel zu konfigurieren.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/AlternateKeyName.cs?name=Model&highlight=9)]
