---
title: Unterstützungs Felder-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a628795e-64df-4f24-a5e8-76bc261e7ed8
uid: core/modeling/backing-field
ms.openlocfilehash: c3ca8bb97992c192672e8c2f2040b0de029df68d
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197480"
---
# <a name="backing-fields"></a><span data-ttu-id="e3a34-102">Unterstützungsfelder</span><span class="sxs-lookup"><span data-stu-id="e3a34-102">Backing Fields</span></span>

> [!NOTE]  
> <span data-ttu-id="e3a34-103">Diese Funktion ist neu in EF Core 1,1.</span><span class="sxs-lookup"><span data-stu-id="e3a34-103">This feature is new in EF Core 1.1.</span></span>

<span data-ttu-id="e3a34-104">Mit Unterstützungs Feldern kann EF anstelle einer Eigenschaft ein Feld lesen und/oder in ein Feld schreiben.</span><span class="sxs-lookup"><span data-stu-id="e3a34-104">Backing fields allow EF to read and/or write to a field rather than a property.</span></span> <span data-ttu-id="e3a34-105">Dies kann hilfreich sein, wenn die Kapselung in der-Klasse verwendet wird, um die Verwendung von zu beschränken und/oder die Semantik für den Zugriff auf die Daten durch den Anwendungscode zu verbessern. der Wert sollte jedoch aus der Datenbank gelesen und/oder in diese geschrieben werden, ohne dass diese Einschränkungen verwendet werden. Steigerung.</span><span class="sxs-lookup"><span data-stu-id="e3a34-105">This can be useful when encapsulation in the class is being used to restrict the use of and/or enhance the semantics around access to the data by application code, but the value should be read from and/or written to the database without using those restrictions/enhancements.</span></span>

## <a name="conventions"></a><span data-ttu-id="e3a34-106">Konventionen</span><span class="sxs-lookup"><span data-stu-id="e3a34-106">Conventions</span></span>

<span data-ttu-id="e3a34-107">Gemäß der Konvention werden die folgenden Felder als Sicherungs Felder für eine bestimmte Eigenschaft (aufgelistet in der Rangfolge) erkannt.</span><span class="sxs-lookup"><span data-stu-id="e3a34-107">By convention, the following fields will be discovered as backing fields for a given property (listed in precedence order).</span></span> <span data-ttu-id="e3a34-108">Felder werden nur für Eigenschaften erkannt, die im Modell enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="e3a34-108">Fields are only discovered for properties that are included in the model.</span></span> <span data-ttu-id="e3a34-109">Weitere Informationen zu den Eigenschaften, die im Modell enthalten sind, finden Sie unter [einschließen & Ausschließen von Eigenschaften](included-properties.md).</span><span class="sxs-lookup"><span data-stu-id="e3a34-109">For more information on which properties are included in the model, see [Including & Excluding Properties](included-properties.md).</span></span>

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/BackingField.cs#Sample)]

<span data-ttu-id="e3a34-110">Wenn ein dahinter liegendes Feld konfiguriert ist, schreibt EF direkt in dieses Feld, wenn Entitäts Instanzen aus der Datenbank materialisiert werden (anstatt den Eigenschaften Setter zu verwenden).</span><span class="sxs-lookup"><span data-stu-id="e3a34-110">When a backing field is configured, EF will write directly to that field when materializing entity instances from the database (rather than using the property setter).</span></span> <span data-ttu-id="e3a34-111">Wenn EF den Wert zu einem anderen Zeitpunkt lesen oder schreiben muss, wird die-Eigenschaft, sofern möglich, verwendet.</span><span class="sxs-lookup"><span data-stu-id="e3a34-111">If EF needs to read or write the value at other times, it will use the property if possible.</span></span> <span data-ttu-id="e3a34-112">Wenn EF z. b. den Wert für eine Eigenschaft aktualisieren muss, wird der Eigenschaften Setter verwendet, wenn ein solcher definiert ist.</span><span class="sxs-lookup"><span data-stu-id="e3a34-112">For example, if EF needs to update the value for a property, it will use the property setter if one is defined.</span></span> <span data-ttu-id="e3a34-113">Wenn die Eigenschaft schreibgeschützt ist, wird in das Feld geschrieben.</span><span class="sxs-lookup"><span data-stu-id="e3a34-113">If the property is read-only, then it will write to the field.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="e3a34-114">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="e3a34-114">Data Annotations</span></span>

<span data-ttu-id="e3a34-115">Unterstützungs Felder können nicht mit Daten Anmerkungen konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="e3a34-115">Backing fields cannot be configured with data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="e3a34-116">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="e3a34-116">Fluent API</span></span>

<span data-ttu-id="e3a34-117">Sie können die fließende API verwenden, um ein Unterstützungs Feld für eine Eigenschaft zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="e3a34-117">You can use the Fluent API to configure a backing field for a property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingField.cs#Sample)]

### <a name="controlling-when-the-field-is-used"></a><span data-ttu-id="e3a34-118">Steuern, wann das Feld verwendet wird</span><span class="sxs-lookup"><span data-stu-id="e3a34-118">Controlling when the field is used</span></span>

<span data-ttu-id="e3a34-119">Sie können konfigurieren, wann EF das Feld oder die Eigenschaft verwendet.</span><span class="sxs-lookup"><span data-stu-id="e3a34-119">You can configure when EF uses the field or property.</span></span> <span data-ttu-id="e3a34-120">Die unterstützten Optionen finden Sie in der [propertyaccessmode-Enumeration](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) .</span><span class="sxs-lookup"><span data-stu-id="e3a34-120">See the [PropertyAccessMode enum](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) for the supported options.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldAccessMode.cs#Sample)]

### <a name="fields-without-a-property"></a><span data-ttu-id="e3a34-121">Felder ohne Eigenschaft</span><span class="sxs-lookup"><span data-stu-id="e3a34-121">Fields without a property</span></span>

<span data-ttu-id="e3a34-122">Sie können auch eine konzeptionelle Eigenschaft in Ihrem Modell erstellen, die nicht über eine entsprechende CLR-Eigenschaft in der Entitäts Klasse verfügt, sondern stattdessen ein Feld zum Speichern der Daten in der Entität verwendet.</span><span class="sxs-lookup"><span data-stu-id="e3a34-122">You can also create a conceptual property in your model that does not have a corresponding CLR property in the entity class, but instead uses a field to store the data in the entity.</span></span> <span data-ttu-id="e3a34-123">Dies unterscheidet sich von den [Schatten Eigenschaften](shadow-properties.md), bei denen die Daten in der Änderungs Nachverfolgung gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="e3a34-123">This is different from [Shadow Properties](shadow-properties.md), where the data is stored in the change tracker.</span></span> <span data-ttu-id="e3a34-124">Dies wird normalerweise verwendet, wenn die Entitäts Klasse Methoden verwendet, um Werte zu erhalten bzw. festzulegen.</span><span class="sxs-lookup"><span data-stu-id="e3a34-124">This would typically be used if the entity class uses methods to get/set values.</span></span>

<span data-ttu-id="e3a34-125">Sie können EF den Namen des Felds in der `Property(...)` API übergeben.</span><span class="sxs-lookup"><span data-stu-id="e3a34-125">You can give EF the name of the field in the `Property(...)` API.</span></span> <span data-ttu-id="e3a34-126">Wenn keine Eigenschaft mit dem angegebenen Namen vorhanden ist, sucht EF nach einem Feld.</span><span class="sxs-lookup"><span data-stu-id="e3a34-126">If there is no property with the given name, then EF will look for a field.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldNoProperty.cs#Sample)]

<span data-ttu-id="e3a34-127">Sie können auch festlegen, dass der Eigenschaft ein anderer Name als der Feldname zugewiesen wird.</span><span class="sxs-lookup"><span data-stu-id="e3a34-127">You can also choose to give the property a name, other than the field name.</span></span> <span data-ttu-id="e3a34-128">Dieser Name wird dann verwendet, wenn das Modell erstellt wird. insbesondere wird er für den Spaltennamen verwendet, der in der Datenbank zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="e3a34-128">This name is then used when creating the model, most notably it will be used for the column name that is mapped to in the database.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldConceptualProperty.cs#Sample)]

<span data-ttu-id="e3a34-129">Wenn in der Entitäts Klasse keine Eigenschaft vorhanden ist, können Sie die `EF.Property(...)` -Methode in einer LINQ-Abfrage verwenden, um auf die Eigenschaft zu verweisen, die konzeptionell Teil des Modells ist.</span><span class="sxs-lookup"><span data-stu-id="e3a34-129">When there is no property in the entity class, you can use the `EF.Property(...)` method in a LINQ query to refer to the property that is conceptually part of the model.</span></span>

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "Url"));
```
