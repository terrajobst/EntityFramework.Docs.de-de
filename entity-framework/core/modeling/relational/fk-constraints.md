---
title: Foreign Key-Einschränkungen-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: dbaf4bac-1fd5-46c0-ac57-64d7153bc574
uid: core/modeling/relational/fk-constraints
ms.openlocfilehash: df739f01a799ec8edad4cf44d8cf50edf292992f
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655998"
---
# <a name="foreign-key-constraints"></a>Fremdschlüsseleinschränkungen

> [!NOTE]  
> Die Konfiguration in diesem Abschnitt gilt allgemein für relationale Datenbanken. Die hier gezeigten Erweiterungsmethoden werden verfügbar, wenn Sie einen relationalen Datenbankanbieter installieren (aufgrund des gemeinsam genutzten Pakets *Microsoft.EntityFrameworkCore.Relational*).

Eine FOREIGN KEY-Einschränkung wird für jede Beziehung im Modell eingeführt.

## <a name="conventions"></a>Konventionen

Gemäß der Konvention werden Foreign Key-Einschränkungen `FK_<dependent type name>_<principal type name>_<foreign key property name>`benannt. Bei zusammengesetzten Fremdschlüsseln `<foreign key property name>` zu einer durch Trennzeichen getrennten Liste von Fremdschlüssel Eigenschaftsnamen.

## <a name="data-annotations"></a>Datenanmerkungen

Namen von Foreign Key-Einschränkungen können nicht mithilfe von Daten Anmerkungen konfiguriert werden.

## <a name="fluent-api"></a>Fluent-API

Sie können die fließende API verwenden, um den Namen der FOREIGN KEY-Einschränkung für eine Beziehung zu konfigurieren.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/RelationshipConstraintName.cs?name=Constraint&highlight=12)]
