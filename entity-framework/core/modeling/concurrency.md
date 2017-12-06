---
title: "Parallelitätstoken - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
ms.technology: entity-framework-core
uid: core/modeling/concurrency
ms.openlocfilehash: 6574a9098d38c4aa525ffb4896adb01082420b5f
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2017
---
# <a name="concurrency-tokens"></a><span data-ttu-id="16792-102">Parallelitätstoken</span><span class="sxs-lookup"><span data-stu-id="16792-102">Concurrency Tokens</span></span>

<span data-ttu-id="16792-103">Wenn eine Eigenschaft als ein parallelitätstoken konfiguriert ist prüft EF dann, dass kein anderer Benutzer beim Speichern der Änderungen an diesen Datensatz der Wert in der Datenbank nicht geändert wurde.</span><span class="sxs-lookup"><span data-stu-id="16792-103">If a property is configured as a concurrency token then EF will check that no other user has modified that value in the database when saving changes to that record.</span></span> <span data-ttu-id="16792-104">EF ein Muster für die vollständige Parallelität verwendet, d. h. sie wird wird davon ausgegangen, dass der Wert nicht geändert hat und versuchen Sie es, speichern Sie die Daten auszulösen, wenn es feststellt, dass der Wert geändert wurde.</span><span class="sxs-lookup"><span data-stu-id="16792-104">EF uses an optimistic concurrency pattern, meaning it will assume the value has not changed and try to save the data, but throw if it finds the value has been changed.</span></span>

<span data-ttu-id="16792-105">Wir möchten z. B. konfigurieren `LastName` auf `Person` ein parallelitätstoken ist.</span><span class="sxs-lookup"><span data-stu-id="16792-105">For example we may want to configure `LastName` on `Person` to be a concurrency token.</span></span> <span data-ttu-id="16792-106">Dies bedeutet, dass, wenn ein Benutzer versucht, speichern einige Änderungen an einer `Person`, jedoch einem anderen Benutzer geändert wurde die `LastName` und dann eine Ausnahme ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="16792-106">This means that if one user tries to save some changes to a `Person`, but another user has changed the `LastName` then an exception will be thrown.</span></span> <span data-ttu-id="16792-107">Dies kann wünschenswert sein, damit Ihre Anwendung kann der Benutzer aufgefordert, stellen Sie sicher, dass dieser Datensatz weiterhin dieselbe tatsächlichen Person darstellt, bevor Sie ihre Änderungen speichern.</span><span class="sxs-lookup"><span data-stu-id="16792-107">This may be desirable so that your application can prompt the user to ensure this record still represents the same actual person before saving their changes.</span></span>

> [!NOTE]
> <span data-ttu-id="16792-108">Auf dieser Seite dokumentiert, wie Sie parallelitätstoken zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="16792-108">This page documents how to configure concurrency tokens.</span></span> <span data-ttu-id="16792-109">Finden Sie unter [Behandeln von Parallelität](../saving/concurrency.md) für Beispiele für vollständige Parallelität in Ihrer Anwendung verwenden.</span><span class="sxs-lookup"><span data-stu-id="16792-109">See [Handling Concurrency](../saving/concurrency.md) for examples of how to use optimistic concurrency in your application.</span></span>

## <a name="how-concurrency-tokens-work-in-ef"></a><span data-ttu-id="16792-110">Funktionsweise von parallelitätstoken in EF</span><span class="sxs-lookup"><span data-stu-id="16792-110">How concurrency tokens work in EF</span></span>

<span data-ttu-id="16792-111">Datenspeicher können parallelitätstoken erzwingen, indem Sie überprüfen, dass ein Datensatz, aktualisieren oder löschen noch den gleichen Wert für die parallelitätstoken, der zugewiesen wurde verfügt, wenn der Kontext ursprünglich die Daten aus der Datenbank geladen.</span><span class="sxs-lookup"><span data-stu-id="16792-111">Data stores can enforce concurrency tokens by checking that any record being updated or deleted still has the same value for the concurrency token that was assigned when the context originally loaded the data from the database.</span></span>

<span data-ttu-id="16792-112">Z. B. relationale Datenbanken erreichen, indem z. B. im parallelitätstoken in der `WHERE` Klausel beliebiger `UPDATE` oder `DELETE` Befehle und überprüfen die Anzahl der Zeilen, die betroffen sind.</span><span class="sxs-lookup"><span data-stu-id="16792-112">For example, relational databases achieve this by including the concurrency token in the `WHERE` clause of any `UPDATE` or `DELETE` commands and checking the number of rows that were affected.</span></span> <span data-ttu-id="16792-113">Wenn immer noch im parallelitätstoken übereinstimmt, wird eine Zeile aktualisiert werden.</span><span class="sxs-lookup"><span data-stu-id="16792-113">If the concurrency token still matches then one row will be updated.</span></span> <span data-ttu-id="16792-114">Wenn der Wert in der Datenbank geändert wurde, werden keine Zeilen aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="16792-114">If the value in the database has changed, then no rows are updated.</span></span>

```sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="conventions"></a><span data-ttu-id="16792-115">Konventionen</span><span class="sxs-lookup"><span data-stu-id="16792-115">Conventions</span></span>

<span data-ttu-id="16792-116">Gemäß der Konvention werden die Eigenschaften nie als parallelitätstoken konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="16792-116">By convention, properties are never configured as concurrency tokens.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="16792-117">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="16792-117">Data Annotations</span></span>

<span data-ttu-id="16792-118">Sie können die Datenanmerkungen so konfigurieren Sie eine Eigenschaft als ein parallelitätstoken verwenden.</span><span class="sxs-lookup"><span data-stu-id="16792-118">You can use the Data Annotations to configure a property as a concurrency token.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Concurrency.cs#ConfigureConcurrencyAnnotations)]

## <a name="fluent-api"></a><span data-ttu-id="16792-119">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="16792-119">Fluent API</span></span>

<span data-ttu-id="16792-120">Sie können die Fluent-API verwenden, so konfigurieren Sie eine Eigenschaft als ein parallelitätstoken.</span><span class="sxs-lookup"><span data-stu-id="16792-120">You can use the Fluent API to configure a property as a concurrency token.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Concurrency.cs#ConfigureConcurrencyFluent)]

## <a name="timestamprow-version"></a><span data-ttu-id="16792-121">Timestamp/Zeilenversion</span><span class="sxs-lookup"><span data-stu-id="16792-121">Timestamp/row version</span></span>

<span data-ttu-id="16792-122">Ein Zeitstempel ist eine Eigenschaft, wird ein neuer Wert von der Datenbank generiert jedes Mal, wenn eine Zeile eingefügt oder aktualisiert wird.</span><span class="sxs-lookup"><span data-stu-id="16792-122">A timestamp is a property where a new value is generated by the database every time a row is inserted or updated.</span></span> <span data-ttu-id="16792-123">Die Eigenschaft wird auch als ein parallelitätstoken behandelt.</span><span class="sxs-lookup"><span data-stu-id="16792-123">The property is also treated as a concurrency token.</span></span> <span data-ttu-id="16792-124">Dadurch wird sichergestellt, dass eine Ausnahme erhalten Sie, wenn andere Benutzer eine Zeile geändert hat, die aktualisiert werden, weil Sie für die Daten abgefragt werden soll.</span><span class="sxs-lookup"><span data-stu-id="16792-124">This ensures you will get an exception if anyone else has modified a row that you are trying to update since you queried for the data.</span></span>

<span data-ttu-id="16792-125">Wird dies erreicht ist, bis zu der Datenbankanbieter verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="16792-125">How this is achieved is up to the database provider being used.</span></span> <span data-ttu-id="16792-126">Für SQL Server wird in der Regel Zeitstempel auf verwendet eine *Byte []* setup-Eigenschaft, die als eine *ROWVERSION* Spalte in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="16792-126">For SQL Server, timestamp is usually used on a *byte[]* property, which will be setup as a *ROWVERSION* column in the database.</span></span>

### <a name="conventions"></a><span data-ttu-id="16792-127">Konventionen</span><span class="sxs-lookup"><span data-stu-id="16792-127">Conventions</span></span>

<span data-ttu-id="16792-128">Gemäß der Konvention werden die Eigenschaften nie als Zeitstempel konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="16792-128">By convention, properties are never configured as timestamps.</span></span>

### <a name="data-annotations"></a><span data-ttu-id="16792-129">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="16792-129">Data Annotations</span></span>

<span data-ttu-id="16792-130">Datenanmerkungen können Sie eine Eigenschaft als Zeitstempel konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="16792-130">You can use Data Annotations to configure a property as a timestamp.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Timestamp.cs#ConfigureTimestampAnnotations)]

### <a name="fluent-api"></a><span data-ttu-id="16792-131">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="16792-131">Fluent API</span></span>

<span data-ttu-id="16792-132">Sie können die Fluent-API verwenden, so konfigurieren Sie eine Eigenschaft als einen Zeitstempel.</span><span class="sxs-lookup"><span data-stu-id="16792-132">You can use the Fluent API to configure a property as a timestamp.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Timestamp.cs#ConfigureTimestampFluent)]
