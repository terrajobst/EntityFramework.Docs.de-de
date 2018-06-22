---
title: Sichern Felder - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a628795e-64df-4f24-a5e8-76bc261e7ed8
ms.technology: entity-framework-core
uid: core/modeling/backing-field
ms.openlocfilehash: e95417b3368d09a64f9ec02ae40c38dc770c2e6b
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2017
ms.locfileid: "26053460"
---
# <a name="backing-fields"></a><span data-ttu-id="ed5ee-102">Dahinter liegenden Felder</span><span class="sxs-lookup"><span data-stu-id="ed5ee-102">Backing Fields</span></span>

> [!NOTE]  
> <span data-ttu-id="ed5ee-103">Dieses Feature ist neu in EF Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="ed5ee-103">This feature is new in EF Core 1.1.</span></span>

<span data-ttu-id="ed5ee-104">Dahinter liegenden Felder können EF zum Lesen und/oder Schreiben in ein Feld, sondern eine Eigenschaft an.</span><span class="sxs-lookup"><span data-stu-id="ed5ee-104">Backing fields allow EF to read and/or write to a field rather than a property.</span></span> <span data-ttu-id="ed5ee-105">Dies kann nützlich sein, wenn für die Kapselung in der Klasse zum Einschränken der Verwendung von und/oder verbessern die Semantik für den Zugriff auf die Daten vom Anwendungscode verwendet wird, aber der Wert sollte Lese- oder in die Datenbank geschrieben werden, ohne diese Einschränkungen werden / Verbesserungen.</span><span class="sxs-lookup"><span data-stu-id="ed5ee-105">This can be useful when encapsulation in the class is being used to restrict the use of and/or enhance the semantics around access to the data by application code, but the value should be read from and/or written to the database without using those restrictions/enhancements.</span></span>

## <a name="conventions"></a><span data-ttu-id="ed5ee-106">Konventionen</span><span class="sxs-lookup"><span data-stu-id="ed5ee-106">Conventions</span></span>

<span data-ttu-id="ed5ee-107">Gemäß der Konvention werden die folgenden Felder als Felder für eine bestimmte Eigenschaft (in ihrer Rangfolge aufgeführt) sichern ermittelt.</span><span class="sxs-lookup"><span data-stu-id="ed5ee-107">By convention, the following fields will be discovered as backing fields for a given property (listed in precedence order).</span></span> <span data-ttu-id="ed5ee-108">Felder werden nur für Eigenschaften ermittelt, die im Modell enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="ed5ee-108">Fields are only discovered for properties that are included in the model.</span></span> <span data-ttu-id="ed5ee-109">Weitere Informationen, die für die Eigenschaften im Modell enthalten sind, finden Sie unter [einschließlich & Eigenschaften außer](included-properties.md).</span><span class="sxs-lookup"><span data-stu-id="ed5ee-109">For more information on which properties are included in the model, see [Including & Excluding Properties](included-properties.md).</span></span>

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/BackingField.cs#Sample)]

<span data-ttu-id="ed5ee-110">Wenn ein dahinter liegendes Feld konfiguriert ist, schreibt EF direkt auf dieses Feld beim Materialisieren von Instanzen der Entität aus der Datenbank (anstatt den Eigenschaftensetter).</span><span class="sxs-lookup"><span data-stu-id="ed5ee-110">When a backing field is configured, EF will write directly to that field when materializing entity instances from the database (rather than using the property setter).</span></span> <span data-ttu-id="ed5ee-111">Wenn EF muss zum Lesen oder schreiben den Wert zu anderen Zeiten auf, wird die Eigenschaft nach Möglichkeit verwendet.</span><span class="sxs-lookup"><span data-stu-id="ed5ee-111">If EF needs to read or write the value at other times, it will use the property if possible.</span></span> <span data-ttu-id="ed5ee-112">Z. B. EF muss den Wert für eine Eigenschaft aktualisieren, wird es der Setter für eine Eigenschaft verwendet, sofern eine definiert ist.</span><span class="sxs-lookup"><span data-stu-id="ed5ee-112">For example, if EF needs to update the value for a property, it will use the property setter if one is defined.</span></span> <span data-ttu-id="ed5ee-113">Wenn die Eigenschaft schreibgeschützt ist, wird es in das Feld geschrieben.</span><span class="sxs-lookup"><span data-stu-id="ed5ee-113">If the property is read-only, then it will write to the field.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="ed5ee-114">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="ed5ee-114">Data Annotations</span></span>

<span data-ttu-id="ed5ee-115">Sichern die Felder kann nicht mit datenanmerkungen konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="ed5ee-115">Backing fields cannot be configured with data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="ed5ee-116">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="ed5ee-116">Fluent API</span></span>

<span data-ttu-id="ed5ee-117">Sie können die Fluent-API verwenden, so konfigurieren Sie eine dahinter liegende Feld für eine Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="ed5ee-117">You can use the Fluent API to configure a backing field for a property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingField.cs#Sample)]

### <a name="controlling-when-the-field-is-used"></a><span data-ttu-id="ed5ee-118">Steuern, wenn das Feld verwendet wird</span><span class="sxs-lookup"><span data-stu-id="ed5ee-118">Controlling when the field is used</span></span>

<span data-ttu-id="ed5ee-119">Sie können konfigurieren, wenn EF Felds oder der Eigenschaft verwendet.</span><span class="sxs-lookup"><span data-stu-id="ed5ee-119">You can configure when EF uses the field or property.</span></span> <span data-ttu-id="ed5ee-120">Finden Sie unter der [PropertyAccessMode Enum](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) Informationen zu den unterstützten Optionen.</span><span class="sxs-lookup"><span data-stu-id="ed5ee-120">See the [PropertyAccessMode enum](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) for the supported options.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldAccessMode.cs#Sample)]

### <a name="fields-without-a-property"></a><span data-ttu-id="ed5ee-121">Felder ohne eine Eigenschaft</span><span class="sxs-lookup"><span data-stu-id="ed5ee-121">Fields without a property</span></span>

<span data-ttu-id="ed5ee-122">Sie können auch eine konzeptuelle Eigenschaft im Modell erstellen, die verfügt nicht über einen entsprechenden CLR-Eigenschaft in die Entitätsklasse, sondern ein Feld zum Speichern der Daten in der Entität verwendet.</span><span class="sxs-lookup"><span data-stu-id="ed5ee-122">You can also create a conceptual property in your model that does not have a corresponding CLR property in the entity class, but instead uses a field to store the data in the entity.</span></span> <span data-ttu-id="ed5ee-123">Dies unterscheidet sich von [Shadowing von Eigenschaften](shadow-properties.md), wo die Daten in das System zur änderungsnachverfolgung gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="ed5ee-123">This is different from [Shadow Properties](shadow-properties.md), where the data is stored in the change tracker.</span></span> <span data-ttu-id="ed5ee-124">Dies wird normalerweise verwendet, wenn die Entitätsklasse Methoden verwendet, um Get/Set-Werte.</span><span class="sxs-lookup"><span data-stu-id="ed5ee-124">This would typically be used if the entity class uses methods to get/set values.</span></span>

<span data-ttu-id="ed5ee-125">Der Name des Felds in EF geben die `Property(...)` API.</span><span class="sxs-lookup"><span data-stu-id="ed5ee-125">You can give EF the name of the field in the `Property(...)` API.</span></span> <span data-ttu-id="ed5ee-126">Wenn keine Eigenschaft mit dem angegebenen Namen vorhanden ist, sieht dann EF für ein Feld.</span><span class="sxs-lookup"><span data-stu-id="ed5ee-126">If there is no property with the given name, then EF will look for a field.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldNoProperty.cs#Sample)]

<span data-ttu-id="ed5ee-127">Sie könne auch der Eigenschaft geben Sie einen anderen Namen, als der Name des Felds.</span><span class="sxs-lookup"><span data-stu-id="ed5ee-127">You can also choose to give the property a name, other than the field name.</span></span> <span data-ttu-id="ed5ee-128">Dieser Name wird beim Erstellen des Modells verwendet, sondern wird vor allem Kontrollvorgänge verwendet werden, für den Namen der Spalte, der in der Datenbank zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="ed5ee-128">This name is then used when creating the model, most notably it will be used for the column name that is mapped to in the database.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldConceptualProperty.cs#Sample)]

<span data-ttu-id="ed5ee-129">Wenn keine Eigenschaft in der Entitätsklasse vorhanden ist, können Sie die `EF.Property(...)` Methode in einer LINQ-Abfrage zum Verweisen auf die Eigenschaft, die Teil des Modells prinzipiell sind.</span><span class="sxs-lookup"><span data-stu-id="ed5ee-129">When there is no property in the entity class, you can use the `EF.Property(...)` method in a LINQ query to refer to the property that is conceptually part of the model.</span></span>

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "Url"));
```
