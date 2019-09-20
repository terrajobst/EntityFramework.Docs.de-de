---
title: Datentypen-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9d2e647f-29e4-483b-af00-74269eb06e8f
uid: core/modeling/relational/data-types
ms.openlocfilehash: d667cbcb821e321faed36d097b531c7c55b81248
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149157"
---
# <a name="data-types"></a><span data-ttu-id="47a60-102">Datentypen</span><span class="sxs-lookup"><span data-stu-id="47a60-102">Data Types</span></span>

> [!NOTE]  
> <span data-ttu-id="47a60-103">Die Konfiguration in diesem Abschnitt gilt allgemein für relationale Datenbanken.</span><span class="sxs-lookup"><span data-stu-id="47a60-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="47a60-104">Die hier gezeigten Erweiterungsmethoden werden verfügbar, wenn Sie einen relationalen Datenbankanbieter installieren (aufgrund des gemeinsam genutzten Pakets *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="47a60-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="47a60-105">Datentyp bezieht sich auf den datenbankspezifischen Typ der Spalte, der eine Eigenschaft zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="47a60-105">Data type refers to the database specific type of the column to which a property is mapped.</span></span>

## <a name="conventions"></a><span data-ttu-id="47a60-106">Konventionen</span><span class="sxs-lookup"><span data-stu-id="47a60-106">Conventions</span></span>

<span data-ttu-id="47a60-107">Gemäß der Konvention wählt der Datenbankanbieter einen Datentyp aus, der auf dem .NET-Typ der Eigenschaft basiert.</span><span class="sxs-lookup"><span data-stu-id="47a60-107">By convention, the database provider selects a data type based on the .NET type of the property.</span></span> <span data-ttu-id="47a60-108">Es berücksichtigt auch andere Metadaten, z. b. die konfigurierte [Maximale Länge](../max-length.md), ob die Eigenschaft Teil eines Primärschlüssels ist usw.</span><span class="sxs-lookup"><span data-stu-id="47a60-108">It also takes into account other metadata, such as the configured [Maximum Length](../max-length.md), whether the property is part of a primary key, etc.</span></span>

<span data-ttu-id="47a60-109">`datetime2(7)` SQL Server z. b. für `DateTime` Eigenschaften und `nvarchar(max)` für `string` Eigenschaften (oder `nvarchar(450)` für `string` Eigenschaften, die als Schlüssel verwendet werden).</span><span class="sxs-lookup"><span data-stu-id="47a60-109">For example, SQL Server uses `datetime2(7)` for `DateTime` properties, and `nvarchar(max)` for `string` properties (or `nvarchar(450)` for `string` properties that are used as a key).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="47a60-110">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="47a60-110">Data Annotations</span></span>

<span data-ttu-id="47a60-111">Mithilfe von Daten Anmerkungen können Sie einen exakten Datentyp für eine Spalte angeben.</span><span class="sxs-lookup"><span data-stu-id="47a60-111">You can use Data Annotations to specify an exact data type for a column.</span></span>

<span data-ttu-id="47a60-112">`Url` Der folgende Code konfiguriert z. b. als nicht-Unicode-Zeichenfolge mit einer maximalen Länge von `200` und `Rating` als Dezimal `5` Länge mit einer `2`Genauigkeit von und einer Skala von.</span><span class="sxs-lookup"><span data-stu-id="47a60-112">For example the following code configures `Url` as a non-unicode string with maximum length of `200` and `Rating` as decimal with precision of `5` and scale of `2`.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Samples/Relational/DataType.cs?name=Entities&highlight=4,6)]

## <a name="fluent-api"></a><span data-ttu-id="47a60-113">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="47a60-113">Fluent API</span></span>

<span data-ttu-id="47a60-114">Sie können auch die fließende API verwenden, um dieselben Datentypen für die Spalten anzugeben.</span><span class="sxs-lookup"><span data-stu-id="47a60-114">You can also use the Fluent API to specify the same data types for the columns.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/DataType.cs?name=Model&highlight=9-10)]
