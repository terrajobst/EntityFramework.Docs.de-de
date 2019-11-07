---
title: Schatten Eigenschaften-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
uid: core/modeling/shadow-properties
ms.openlocfilehash: ab57358dd247e32c4ca0f57d07b4cb98f2b85d29
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655954"
---
# <a name="shadow-properties"></a>Schatteneigenschaften

Schatten Eigenschaften sind Eigenschaften, die nicht in der .net-Entitäts Klasse definiert sind, aber für diesen Entitätstyp im EF Core Modell definiert sind. Der Wert und der Status dieser Eigenschaften werden ausschließlich in der Änderungs Nachverfolgung beibehalten.

Schatten Eigenschaften sind nützlich, wenn Daten in der Datenbank vorhanden sind, die für die zugeordneten Entitäts Typen nicht verfügbar gemacht werden sollen. Sie werden am häufigsten für Fremdschlüssel Eigenschaften verwendet, bei denen die Beziehung zwischen zwei Entitäten durch einen Fremdschlüssel Wert in der Datenbank dargestellt wird, die Beziehung jedoch für die Entitäts Typen mithilfe von Navigations Eigenschaften zwischen den Entitäts Typen verwaltet wird.

Schatten Eigenschaftswerte können über die `ChangeTracker`-API abgerufen und geändert werden.

``` csharp
context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

Auf Schatten Eigenschaften kann in LINQ-Abfragen über die `EF.Property` statische Methode verwiesen werden.

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a>Konventionen

Schatten Eigenschaften können nach der Konvention erstellt werden, wenn eine Beziehung gefunden wird, aber keine Fremdschlüssel Eigenschaft in der abhängigen Entitäts Klasse gefunden wurde. In diesem Fall wird eine Schatten Fremdschlüssel-Eigenschaft eingeführt. Die Schatten-Fremdschlüssel Eigenschaft wird `<navigation property name><principal key property name>` benannt (die Navigation auf der abhängigen Entität, die auf die Prinzipal Entität verweist, wird für die Benennung verwendet). Wenn der Name der Prinzipal Schlüsseleigenschaft den Namen der Navigations Eigenschaft enthält, wird der Name nur `<principal key property name>`. Wenn für die abhängige Entität keine Navigations Eigenschaft vorhanden ist, wird der Prinzipal Typname an seiner Stelle verwendet.

Beispielsweise führt das folgende Codebeispiel dazu, dass eine `BlogId` Shadow-Eigenschaft in die `Post` Entität eingefügt wird.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/ShadowForeignKey.cs?name=Conventions)]

## <a name="data-annotations"></a>Datenanmerkungen

Schatten Eigenschaften können nicht mit Daten Anmerkungen erstellt werden.

## <a name="fluent-api"></a>Fluent-API

Sie können die fließende API verwenden, um Schatten Eigenschaften zu konfigurieren. Nachdem Sie die Zeichen folgen Überladung `Property` aufgerufen haben, können Sie alle Konfigurations Aufrufe verketten, die Sie für andere Eigenschaften haben.

Wenn der für die `Property` Methode angegebene Name mit dem Namen einer vorhandenen Eigenschaft übereinstimmt (eine Schatten Eigenschaft oder eine, die für die Entitäts Klasse definiert ist), konfiguriert der Code diese vorhandene Eigenschaft, anstatt eine neue Schatten Eigenschaft einzuführen.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ShadowProperty.cs?name=ShadowProperty&highlight=8)]
