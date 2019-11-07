---
title: Standardwerte-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e541366a-130f-47dd-9997-1b110a11febe
uid: core/modeling/relational/default-values
ms.openlocfilehash: b6ac283d551e2c6ee119f7de6933363b5d8793a1
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655906"
---
# <a name="default-values"></a><span data-ttu-id="dc8f3-102">Standardwerte</span><span class="sxs-lookup"><span data-stu-id="dc8f3-102">Default Values</span></span>

> [!NOTE]  
> <span data-ttu-id="dc8f3-103">Die Konfiguration in diesem Abschnitt gilt allgemein für relationale Datenbanken.</span><span class="sxs-lookup"><span data-stu-id="dc8f3-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="dc8f3-104">Die hier gezeigten Erweiterungsmethoden werden verfügbar, wenn Sie einen relationalen Datenbankanbieter installieren (aufgrund des gemeinsam genutzten Pakets *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="dc8f3-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="dc8f3-105">Der Standardwert einer Spalte ist der Wert, der eingefügt wird, wenn eine neue Zeile eingefügt wird, aber für die Spalte kein Wert angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="dc8f3-105">The default value of a column is the value that will be inserted if a new row is inserted but no value is specified for the column.</span></span>

## <a name="conventions"></a><span data-ttu-id="dc8f3-106">Konventionen</span><span class="sxs-lookup"><span data-stu-id="dc8f3-106">Conventions</span></span>

<span data-ttu-id="dc8f3-107">Gemäß der Konvention ist kein Standardwert konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="dc8f3-107">By convention, a default value is not configured.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="dc8f3-108">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="dc8f3-108">Data Annotations</span></span>

<span data-ttu-id="dc8f3-109">Mithilfe von Daten Anmerkungen können Sie keinen Standardwert festlegen.</span><span class="sxs-lookup"><span data-stu-id="dc8f3-109">You can not set a default value using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="dc8f3-110">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="dc8f3-110">Fluent API</span></span>

<span data-ttu-id="dc8f3-111">Sie können die fließende API verwenden, um den Standardwert für eine Eigenschaft anzugeben.</span><span class="sxs-lookup"><span data-stu-id="dc8f3-111">You can use the Fluent API to specify the default value for a property.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/DefaultValue.cs?name=DefaultValue&highlight=9)]

<span data-ttu-id="dc8f3-112">Sie können auch ein SQL-Fragment angeben, das verwendet wird, um den Standardwert zu berechnen.</span><span class="sxs-lookup"><span data-stu-id="dc8f3-112">You can also specify a SQL fragment that is used to calculate the default value.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/DefaultValueSql.cs?name=DefaultValueSql&highlight=9)]
