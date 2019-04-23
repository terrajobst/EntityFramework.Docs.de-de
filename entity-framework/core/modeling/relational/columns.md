---
title: Spaltenzuordnung – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 05a47de9-1078-488e-a823-b516a4208f33
uid: core/modeling/relational/columns
ms.openlocfilehash: 22fcafbfcf9daf765c94e6ca9c42d7770d3e7d07
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929861"
---
# <a name="column-mapping"></a>Spaltenzuordnung

> [!NOTE]  
> Die Konfiguration in diesem Abschnitt gilt allgemein für relationale Datenbanken. Die hier gezeigten Erweiterungsmethoden werden verfügbar, wenn Sie einen relationalen Datenbankanbieter installieren (aufgrund des gemeinsam genutzten Pakets *Microsoft.EntityFrameworkCore.Relational*).

Die spaltenzuordnung gibt die Spaltendaten aus abgefragt und in der Datenbank gespeichert werden sollten.

## <a name="conventions"></a>Konventionen

Gemäß der Konvention wird jede Eigenschaft einer Spalte mit dem gleichen Namen wie die Eigenschaft zuordnen eingerichtet werden.

## <a name="data-annotations"></a>Datenanmerkungen

Sie können Datenanmerkungen verwenden, um die Spalte zu konfigurieren, die eine Eigenschaft zugeordnet ist.

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Samples/Relational/Column.cs?highlight=13)]

## <a name="fluent-api"></a>Fluent-API

Sie können die Fluent-API verwenden, um die Spalte zu konfigurieren, die eine Eigenschaft zugeordnet ist.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/Column.cs?highlight=11-13)]
