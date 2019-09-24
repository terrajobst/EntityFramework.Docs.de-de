---
title: Maximale Länge-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c39c5d43-018d-48b8-94f2-b8bc7c686c69
uid: core/modeling/max-length
ms.openlocfilehash: b6f0594fed0c491b4f79dcda5273cdebe9ecf35f
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197224"
---
# <a name="maximum-length"></a><span data-ttu-id="8e274-102">Maximale Länge</span><span class="sxs-lookup"><span data-stu-id="8e274-102">Maximum Length</span></span>

<span data-ttu-id="8e274-103">Das Konfigurieren einer maximalen Länge stellt einen Hinweis für den Datenspeicher für den entsprechenden Datentyp bereit, der für eine bestimmte Eigenschaft verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="8e274-103">Configuring a maximum length provides a hint to the data store about the appropriate data type to use for a given property.</span></span> <span data-ttu-id="8e274-104">Die maximale Länge gilt nur für Array Datentypen, `string` z `byte[]`. b. und.</span><span class="sxs-lookup"><span data-stu-id="8e274-104">Maximum length only applies to array data types, such as `string` and `byte[]`.</span></span>

> [!NOTE]  
> <span data-ttu-id="8e274-105">Entity Framework führt keine Überprüfung der maximalen Länge durch, bevor Daten an den Anbieter übergeben werden.</span><span class="sxs-lookup"><span data-stu-id="8e274-105">Entity Framework does not do any validation of maximum length before passing data to the provider.</span></span> <span data-ttu-id="8e274-106">Der Anbieter oder Datenspeicher muss ggf. überprüft werden.</span><span class="sxs-lookup"><span data-stu-id="8e274-106">It is up to the provider or data store to validate if appropriate.</span></span> <span data-ttu-id="8e274-107">Wenn Sie z. b. auf SQL Server abzielen, führt das Überschreiten der maximalen Länge zu einer Ausnahme, da der Datentyp der zugrunde liegenden Spalte nicht zulässt, dass überschüssige Daten gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="8e274-107">For example, when targeting SQL Server, exceeding the maximum length will result in an exception as the data type of the underlying column will not allow excess data to be stored.</span></span>

## <a name="conventions"></a><span data-ttu-id="8e274-108">Konventionen</span><span class="sxs-lookup"><span data-stu-id="8e274-108">Conventions</span></span>

<span data-ttu-id="8e274-109">Gemäß der Konvention wird es dem Datenbankanbieter überlassen, einen geeigneten Datentyp für Eigenschaften auszuwählen.</span><span class="sxs-lookup"><span data-stu-id="8e274-109">By convention, it is left up to the database provider to choose an appropriate data type for properties.</span></span> <span data-ttu-id="8e274-110">Bei Eigenschaften mit einer Länge wählt der Datenbankanbieter im Allgemeinen einen Datentyp aus, der die längste Daten Länge zulässt.</span><span class="sxs-lookup"><span data-stu-id="8e274-110">For properties that have a length, the database provider will generally choose a data type that allows for the longest length of data.</span></span> <span data-ttu-id="8e274-111">Microsoft SQL Server werden z. b. `nvarchar(max)` für `string` Eigenschaften verwenden ( `nvarchar(450)` oder, wenn die Spalte als Schlüssel verwendet wird).</span><span class="sxs-lookup"><span data-stu-id="8e274-111">For example, Microsoft SQL Server will use `nvarchar(max)` for `string` properties (or `nvarchar(450)` if the column is used as a key).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="8e274-112">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="8e274-112">Data Annotations</span></span>

<span data-ttu-id="8e274-113">Mit den Daten Anmerkungen können Sie eine maximale Länge für eine Eigenschaft konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="8e274-113">You can use the Data Annotations to configure a maximum length for a property.</span></span> <span data-ttu-id="8e274-114">In diesem Beispiel führt die Ziel SQL Server dies dazu, dass `nvarchar(500)` der Datentyp verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="8e274-114">In this example, targeting SQL Server this would result in the `nvarchar(500)` data type being used.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/MaxLength.cs?highlight=14)]

## <a name="fluent-api"></a><span data-ttu-id="8e274-115">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="8e274-115">Fluent API</span></span>

<span data-ttu-id="8e274-116">Sie können die fließende API verwenden, um eine maximale Länge für eine Eigenschaft zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="8e274-116">You can use the Fluent API to configure a maximum length for a property.</span></span> <span data-ttu-id="8e274-117">In diesem Beispiel führt die Ziel SQL Server dies dazu, dass `nvarchar(500)` der Datentyp verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="8e274-117">In this example, targeting SQL Server this would result in the `nvarchar(500)` data type being used.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/MaxLength.cs?highlight=11-13)]
