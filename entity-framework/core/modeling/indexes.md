---
title: Indizes-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
uid: core/modeling/indexes
ms.openlocfilehash: d1b5cd6853cd24f7e1aa3aace2f0a3455c657cc1
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655697"
---
# <a name="indexes"></a>Indizes

Indizes sind ein gängiges Konzept in vielen Daten speichern. Während Ihre Implementierung im Datenspeicher variieren kann, werden Sie verwendet, um Suchvorgänge auf Grundlage einer Spalte (oder einer Gruppe von Spalten) effizienter zu gestalten.

## <a name="conventions"></a>Konventionen

Gemäß der Konvention wird ein Index in jeder Eigenschaft (oder einem Satz von Eigenschaften) erstellt, die als Fremdschlüssel verwendet wird.

## <a name="data-annotations"></a>Datenanmerkungen

Indizes können nicht mithilfe von Daten Anmerkungen erstellt werden.

## <a name="fluent-api"></a>Fluent-API

Sie können die fließende API verwenden, um einen Index für eine einzelne Eigenschaft anzugeben. Standardmäßig sind Indizes nicht eindeutig.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Index.cs?name=Index&highlight=7,8)]

Sie können auch angeben, dass ein Index eindeutig sein soll. Dies bedeutet, dass keine zwei Entitäten für die angegebene Eigenschaft (n) die gleichen Werte aufweisen dürfen.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexUnique.cs?name=ModelBuilder&highlight=3)]

Sie können einen Index auch über mehr als eine Spalte angeben.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexComposite.cs?name=Composite&highlight=7,8)]

> [!TIP]  
> Pro eindeutigem Satz von Eigenschaften gibt es nur einen Index. Wenn Sie die fließende API verwenden, um einen Index für eine Gruppe von Eigenschaften zu konfigurieren, für die bereits ein Index definiert wurde (entweder durch Konvention oder vorherige Konfiguration), ändern Sie die Definition des Indexes. Dies ist hilfreich, wenn Sie einen von der Konvention erstellten Index weiter konfigurieren möchten.
