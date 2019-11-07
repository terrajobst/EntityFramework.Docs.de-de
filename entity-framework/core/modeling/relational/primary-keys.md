---
title: Primärschlüssel-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c78f8f42-564a-45a4-aca7-3ede9f7ed2bc
uid: core/modeling/relational/primary-keys
ms.openlocfilehash: c195cc37859ea0689b5c6fbb84051564cf96c83a
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656047"
---
# <a name="primary-keys"></a>Primärschlüssel

> [!NOTE]  
> Die Konfiguration in diesem Abschnitt gilt allgemein für relationale Datenbanken. Die hier gezeigten Erweiterungsmethoden werden verfügbar, wenn Sie einen relationalen Datenbankanbieter installieren (aufgrund des gemeinsam genutzten Pakets *Microsoft.EntityFrameworkCore.Relational*).

Eine PRIMARY KEY-Einschränkung wird für den Schlüssel jedes Entitäts Typs eingeführt.

## <a name="conventions"></a>Konventionen

Gemäß der Konvention wird der Primärschlüssel in der Datenbank `PK_<type name>`benannt.

## <a name="data-annotations"></a>Datenanmerkungen

Keine relationalen datenbankspezifischen Aspekte eines Primärschlüssels können mithilfe von Daten Anmerkungen konfiguriert werden.

## <a name="fluent-api"></a>Fluent-API

Sie können die fließende API verwenden, um den Namen der PRIMARY KEY-Einschränkung in der Datenbank zu konfigurieren.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/KeyName.cs?name=KeyName&highlight=9)]
