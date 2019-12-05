---
title: Foreign Key-Einschränkungen-EF Core
author: AndriySvyryd
ms.date: 11/21/2019
uid: core/modeling/relational/fk-constraints
ms.openlocfilehash: 2855137adf2ba3c9edaabd15a05f7a209f00f685
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824592"
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
