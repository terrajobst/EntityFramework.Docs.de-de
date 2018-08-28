---
title: Alternativschlüssel (Unique-Einschränkungen) – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3d419dcf-2b5d-467c-b408-ea03d830721a
uid: core/modeling/relational/unique-constraints
ms.openlocfilehash: 7ec58ee31aac79e15329dc8542f37fd117772fbe
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994190"
---
# <a name="alternate-keys-unique-constraints"></a>Alternativschlüssel (Unique-Einschränkungen)

> [!NOTE]  
> Die Konfiguration in diesem Abschnitt gilt allgemein für relationale Datenbanken. Die hier gezeigten Erweiterungsmethoden werden verfügbar, wenn Sie einen relationalen Datenbankanbieter installieren (aufgrund des gemeinsam genutzten Pakets *Microsoft.EntityFrameworkCore.Relational*).

Eine unique-Einschränkung wird für jeden alternativen Schlüssel im Modell eingeführt.

## <a name="conventions"></a>Konventionen

Gemäß der Konvention, den Index und die Einschränkung, die für einen alternativen Schlüssel eingeführt werden Namen `AK_<type name>_<property name>`. Für zusammengesetzte alternativen Schlüssel `<property name>` wird eine Unterstrich getrennt Liste von Eigenschaftsnamen.

## <a name="data-annotations"></a>Datenanmerkungen

Unique-Einschränkungen können nicht mithilfe von Datenanmerkungen konfiguriert werden.

## <a name="fluent-api"></a>Fluent-API

Sie können die Fluent-API verwenden, so konfigurieren Sie die Index- und einschränkungsobjekten Namen für einen alternativen Schlüssel.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/AlternateKeyName.cs?name=Model&highlight=9)]
