---
title: "Datentypen – EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9d2e647f-29e4-483b-af00-74269eb06e8f
ms.technology: entity-framework-core
uid: core/modeling/relational/data-types
ms.openlocfilehash: fd4668a3f9554eb9d3b1161d5dddce2fcdcac712
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2017
---
# <a name="data-types"></a><span data-ttu-id="14254-102">Datentypen</span><span class="sxs-lookup"><span data-stu-id="14254-102">Data Types</span></span>

> [!NOTE]  
> <span data-ttu-id="14254-103">Die Konfiguration in diesem Abschnitt ist im Allgemeinen gilt für relationale Datenbanken.</span><span class="sxs-lookup"><span data-stu-id="14254-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="14254-104">Die Erweiterungsmethoden, die hier gezeigten werden verfügbar, wenn Sie einen relationale Datenbank-Anbieter installieren (aufgrund der freigegebenen *Microsoft.EntityFrameworkCore.Relational* Paket).</span><span class="sxs-lookup"><span data-stu-id="14254-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="14254-105">Datentyp bezieht sich auf den bestimmten Datenbanktyp der Spalte, die eine Eigenschaft zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="14254-105">Data type refers to the database specific type of the column to which a property is mapped.</span></span>

## <a name="conventions"></a><span data-ttu-id="14254-106">Konventionen</span><span class="sxs-lookup"><span data-stu-id="14254-106">Conventions</span></span>

<span data-ttu-id="14254-107">Gemäß der Konvention wählt der Datenbankanbieter einen Datentyp, der basierend auf den CLR-Typ der Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="14254-107">By convention, the database provider selects a data type based on the CLR type of the property.</span></span> <span data-ttu-id="14254-108">Außerdem akzeptiert es berücksichtigt andere Metadaten, wie z. B. die konfigurierte [Maximallänge](../max-length.md), ob die Eigenschaft Teil einer primary key-, usw. ist.</span><span class="sxs-lookup"><span data-stu-id="14254-108">It also takes into account other metadata, such as the configured [Maximum Length](../max-length.md), whether the property is part of a primary key, etc.</span></span>

<span data-ttu-id="14254-109">SQL Server verwendet z. B. `datetime2(7)` für `DateTime` Eigenschaften und `nvarchar(max)` für `string` Eigenschaften (oder `nvarchar(450)` für `string` Eigenschaften, die als Schlüssel verwendet werden).</span><span class="sxs-lookup"><span data-stu-id="14254-109">For example, SQL Server uses `datetime2(7)` for `DateTime` properties, and `nvarchar(max)` for `string` properties (or `nvarchar(450)` for `string` properties that are used as a key).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="14254-110">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="14254-110">Data Annotations</span></span>

<span data-ttu-id="14254-111">Sie können Datenanmerkungen verwenden, einen genauer Datentyp für eine Spalte angeben.</span><span class="sxs-lookup"><span data-stu-id="14254-111">You can use Data Annotations to specify an exact data type for a column.</span></span>

<span data-ttu-id="14254-112">Der folgende Code z. B. konfiguriert `Url` als nicht-Unicode-Zeichenfolge mit maximal `200` und `Rating` als Dezimalzahl mit einer Genauigkeit von `5` und Skalieren von `2`.</span><span class="sxs-lookup"><span data-stu-id="14254-112">For example the following code configures `Url` as a non-unicode string with maximum length of `200` and `Rating` as decimal with precision of `5` and scale of `2`.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Samples/Relational/DataType.cs?name=Entities&highlight=4,6)]

## <a name="fluent-api"></a><span data-ttu-id="14254-113">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="14254-113">Fluent API</span></span>

<span data-ttu-id="14254-114">Die Fluent-API können auch die gleichen Datentypen für die Spalten angeben.</span><span class="sxs-lookup"><span data-stu-id="14254-114">You can also use the Fluent API to specify the same data types for the columns.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/DataType.cs?name=Model&highlight=9-10)]
