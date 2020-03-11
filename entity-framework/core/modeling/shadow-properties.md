---
title: Schatten Eigenschaften-EF Core
author: AndriySvyryd
ms.date: 01/03/2020
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
uid: core/modeling/shadow-properties
ms.openlocfilehash: 229cfd83f75b01dff9ac9ad30ee55c7cc727c19e
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414710"
---
# <a name="shadow-properties"></a>Schatteneigenschaften

Schatten Eigenschaften sind Eigenschaften, die nicht in der .net-Entitäts Klasse definiert sind, aber für diesen Entitätstyp im EF Core Modell definiert sind. Der Wert und der Status dieser Eigenschaften werden ausschließlich in der Änderungs Nachverfolgung beibehalten. Schatten Eigenschaften sind nützlich, wenn Daten in der Datenbank vorhanden sind, die für die zugeordneten Entitäts Typen nicht verfügbar gemacht werden sollen.

## <a name="foreign-key-shadow-properties"></a>Fremdschlüssel Schatten-Eigenschaften

Schatten Eigenschaften werden am häufigsten für Fremdschlüssel Eigenschaften verwendet, bei denen die Beziehung zwischen zwei Entitäten durch einen Fremdschlüssel Wert in der Datenbank dargestellt wird, die Beziehung jedoch für die Entitäts Typen mithilfe von Navigations Eigenschaften zwischen der Entität verwaltet wird. solche. Gemäß der Konvention führt EF eine Schatten Eigenschaft ein, wenn eine Beziehung entdeckt wird, aber in der abhängigen Entitäts Klasse keine Fremdschlüssel Eigenschaft gefunden wird.

Die-Eigenschaft wird `<navigation property name><principal key property name>` benannt (die Navigation auf der abhängigen Entität, die auf die Prinzipal Entität verweist, wird für die Benennung verwendet). Wenn der Name der Prinzipal Schlüsseleigenschaft den Namen der Navigations Eigenschaft enthält, wird der Name nur `<principal key property name>`. Wenn für die abhängige Entität keine Navigations Eigenschaft vorhanden ist, wird der Prinzipal Typname an seiner Stelle verwendet.

Beispielsweise führt das folgende Codebeispiel dazu, dass eine `BlogId` Shadow-Eigenschaft in die `Post` Entität eingefügt wird:

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/ShadowForeignKey.cs?name=Conventions&highlight=21-23)]

## <a name="configuring-shadow-properties"></a>Konfigurieren von Schatten Eigenschaften

Sie können die fließende API verwenden, um Schatten Eigenschaften zu konfigurieren. Nachdem Sie die Zeichen folgen Überladung `Property`aufgerufen haben, können Sie alle Konfigurations Aufrufe verketten, die für andere Eigenschaften gelten. Im folgenden Beispiel wird eine Schatten Eigenschaft erstellt, da `Blog` keine CLR-Eigenschaft mit dem Namen `LastUpdated`hat:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ShadowProperty.cs?name=ShadowProperty&highlight=8)]

Wenn der für die `Property` Methode angegebene Name mit dem Namen einer vorhandenen Eigenschaft übereinstimmt (eine Schatten Eigenschaft oder eine, die für die Entitäts Klasse definiert ist), konfiguriert der Code diese vorhandene Eigenschaft, anstatt eine neue Schatten Eigenschaft einzuführen.

## <a name="accessing-shadow-properties"></a>Zugreifen auf Schatten Eigenschaften

Schatten Eigenschaftswerte können über die `ChangeTracker`-API abgerufen und geändert werden:

``` csharp
context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

Auf Schatten Eigenschaften kann in LINQ-Abfragen über die `EF.Property` statische Methode verwiesen werden:

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```
