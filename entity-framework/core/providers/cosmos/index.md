---
title: 'Azure Cosmos DB-Anbieter: EF Core'
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 09/12/2019
ms.assetid: 28264681-4486-4891-888c-be5e4ade24f1
uid: core/providers/cosmos/index
ms.openlocfilehash: 683436aa485d2fef9aa8bf6c6ff02b00dfeb28cf
ms.sourcegitcommit: 2caec1e63f2ce1d9439ef6193df5a77da2fedd0f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/26/2019
ms.locfileid: "71317559"
---
# <a name="ef-core-azure-cosmos-db-provider"></a><span data-ttu-id="1f616-102">EF Core Azure Cosmos DB-Anbieter</span><span class="sxs-lookup"><span data-stu-id="1f616-102">EF Core Azure Cosmos DB Provider</span></span>

>[!NOTE]
> <span data-ttu-id="1f616-103">Dieser Anbieter ist neu in EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="1f616-103">This provider is new in EF Core 3.0.</span></span>

<span data-ttu-id="1f616-104">Dieser Datenbankanbieter ermöglicht die Verwendung von Entity Framework Core mit Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="1f616-104">This database provider allows Entity Framework Core to be used with Azure Cosmos DB.</span></span> <span data-ttu-id="1f616-105">Dieser Anbieter wird nicht im Rahmen des [Entity Framework Core-Projekts](https://github.com/aspnet/EntityFrameworkCore) verwaltet.</span><span class="sxs-lookup"><span data-stu-id="1f616-105">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

<span data-ttu-id="1f616-106">Es wird dringend empfohlen, sich vor dem Lesen dieses Abschnitts mit der [Azure Cosmos DB-Dokumentation](https://docs.microsoft.com/en-us/azure/cosmos-db/introduction) vertraut zu machen.</span><span class="sxs-lookup"><span data-stu-id="1f616-106">It is strongly recommended to familiarize yourself with the [Azure Cosmos DB documentation](https://docs.microsoft.com/en-us/azure/cosmos-db/introduction) before reading this section.</span></span>

## <a name="install"></a><span data-ttu-id="1f616-107">Installieren</span><span class="sxs-lookup"><span data-stu-id="1f616-107">Install</span></span>

<span data-ttu-id="1f616-108">Installieren Sie das [NuGet-Paket „Microsoft.EntityFrameworkCore.Cosmos“](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos/).</span><span class="sxs-lookup"><span data-stu-id="1f616-108">Install the [Microsoft.EntityFrameworkCore.Cosmos NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Cosmos
```

## <a name="get-started"></a><span data-ttu-id="1f616-109">Erste Schritte</span><span class="sxs-lookup"><span data-stu-id="1f616-109">Get Started</span></span>

> [!TIP]  
> <span data-ttu-id="1f616-110">Das in diesem Artikel verwendete [Beispiel finden Sie auf GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Cosmos).</span><span class="sxs-lookup"><span data-stu-id="1f616-110">You can view this article's [sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Cosmos).</span></span>

<span data-ttu-id="1f616-111">Wie bei anderen Anbietern besteht der erste Schritt darin, `UseCosmos` aufzurufen:</span><span class="sxs-lookup"><span data-stu-id="1f616-111">Like for other providers the first step is to call `UseCosmos`:</span></span>

[!code-csharp[Configuration](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Configuration)]

> [!WARNING]
> <span data-ttu-id="1f616-112">Der Endpunkt und der Schlüssel sind aus Gründen der Einfachheit hier hartcodiert, aber in einer Produktions-App sollten sie [sicher gespeichert werden](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager).</span><span class="sxs-lookup"><span data-stu-id="1f616-112">The endpoint and key are hardcoded here for simplicity, but in a production app these should be [stored securily](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager)</span></span>

<span data-ttu-id="1f616-113">In diesem Beispiel ist `Order` eine einfache Entität mit einem Verweis auf den [nicht eigenständigen](../../modeling/owned-entities.md) Typ `StreetAddress`.</span><span class="sxs-lookup"><span data-stu-id="1f616-113">In this example `Order` is a simple entity with a reference to the [owned type](../../modeling/owned-entities.md) `StreetAddress`.</span></span>

[!code-csharp[Order](../../../../samples/core/Cosmos/ModelBuilding/Order.cs?name=Order)]

[!code-csharp[StreetAddress](../../../../samples/core/Cosmos/ModelBuilding/StreetAddress.cs?name=StreetAddress)]

<span data-ttu-id="1f616-114">Das Speichern und Abfragen von Daten folgt dem normalen EF-Muster:</span><span class="sxs-lookup"><span data-stu-id="1f616-114">Saving and quering data follows the normal EF pattern:</span></span>

[!code-csharp[HelloCosmos](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=HelloCosmos)]

> [!IMPORTANT]
> <span data-ttu-id="1f616-115">Der Aufruf von `EnsureCreated` ist erforderlich, um die erforderlichen Sammlungen zu erstellen und die [Seeddaten](../../modeling/data-seeding.md) einzufügen, wenn Sie im Modell vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="1f616-115">Calling `EnsureCreated` is necessary to create the required collections and insert the [seed data](../../modeling/data-seeding.md) if present in the model.</span></span> <span data-ttu-id="1f616-116">`EnsureCreated` sollte jedoch nur während der Bereitstellung und nicht im normalen Betrieb aufgerufen werden, da dies zu Leistungsproblemen führen kann.</span><span class="sxs-lookup"><span data-stu-id="1f616-116">However `EnsureCreated` should only be called during deployment, not normal operation, as it may cause performance issues.</span></span>

## <a name="cosmos-specific-model-customization"></a><span data-ttu-id="1f616-117">Cosmos-spezifische Modellanpassung</span><span class="sxs-lookup"><span data-stu-id="1f616-117">Cosmos-specific Model Customization</span></span>

<span data-ttu-id="1f616-118">Standardmäßig werden alle Entitätstypen demselben Container zugeordnet, der nach dem abgeleiteten Kontext benannt wird (in diesem Fall `"OrderContext"`).</span><span class="sxs-lookup"><span data-stu-id="1f616-118">By default all entity types are mapped to the same container, named after the derived context (`"OrderContext"` in this case).</span></span> <span data-ttu-id="1f616-119">Verwenden Sie zum Ändern des Standardcontainernamens `HasDefaultContainer`:</span><span class="sxs-lookup"><span data-stu-id="1f616-119">To change the default container name use `HasDefaultContainer`:</span></span>

[!code-csharp[DefaultContainer](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=DefaultContainer)]

<span data-ttu-id="1f616-120">Um einen Entitätstyp einem anderen Container zuzuordnen, verwenden Sie `ToContainer`:</span><span class="sxs-lookup"><span data-stu-id="1f616-120">To map an entity type to a different container use `ToContainer`:</span></span>

[!code-csharp[Container](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Container)]

<span data-ttu-id="1f616-121">Um den Entitätstyp zu identifizieren, den ein bestimmtes Element darstellt, fügt EF Core einen Diskriminatorwert auch dann hinzu, wenn keine abgeleiteten Entitätstypen vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="1f616-121">To identify the entity type that a given item represent EF Core adds a discriminator value even if there are no derived entity types.</span></span> <span data-ttu-id="1f616-122">Der Name und der Wert des Diskriminators [können geändert werden](../../modeling/inheritance.md).</span><span class="sxs-lookup"><span data-stu-id="1f616-122">The name and value of the discriminator [can be changed](../../modeling/inheritance.md).</span></span>

## <a name="embedded-entities"></a><span data-ttu-id="1f616-123">Eingebettete Entitäten</span><span class="sxs-lookup"><span data-stu-id="1f616-123">Embedded Entities</span></span>

<span data-ttu-id="1f616-124">Nicht eigenständige Cosmos-Entitäten sind in das gleiche Element wie der Besitzer eingebettet.</span><span class="sxs-lookup"><span data-stu-id="1f616-124">For Cosmos owned entities are embedded in the same item as the owner.</span></span> <span data-ttu-id="1f616-125">Verwenden Sie zum Ändern eines Eigenschaftennamens `ToJsonProperty`:</span><span class="sxs-lookup"><span data-stu-id="1f616-125">To change a property name use `ToJsonProperty`:</span></span>

[!code-csharp[PropertyNames](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=PropertyNames)]

<span data-ttu-id="1f616-126">Mit dieser Konfiguration wird die Reihenfolge des obigen Beispiels wie folgt gespeichert:</span><span class="sxs-lookup"><span data-stu-id="1f616-126">With this configuration the order from the example above is stored like this:</span></span>

``` json
{
    "Id": 1,
    "Discriminator": "Order",
    "TrackingNumber": null,
    "id": "Order|1",
    "Address": {
        "ShipsToCity": "London",
        "Discriminator": "StreetAddress",
        "ShipsToStreet": "221 B Baker St"
    },
    "_rid": "6QEKAM+BOOABAAAAAAAAAA==",
    "_self": "dbs/6QEKAA==/colls/6QEKAM+BOOA=/docs/6QEKAM+BOOABAAAAAAAAAA==/",
    "_etag": "\"00000000-0000-0000-683c-692e763901d5\"",
    "_attachments": "attachments/",
    "_ts": 1568163674
}
```

<span data-ttu-id="1f616-127">Sammlungen von nicht eigenständigen Entitäten werden ebenfalls eingebettet.</span><span class="sxs-lookup"><span data-stu-id="1f616-127">Collections of owned entities are embedded as well.</span></span> <span data-ttu-id="1f616-128">Im nächsten Beispiel verwenden wir die `Distributor`-Klasse mit einer Sammlung von `StreetAddress`:</span><span class="sxs-lookup"><span data-stu-id="1f616-128">For the next example we'll use the `Distributor` class with a collection of `StreetAddress`:</span></span>

[!code-csharp[Distributor](../../../../samples/core/Cosmos/ModelBuilding/Distributor.cs?name=Distributor)]

<span data-ttu-id="1f616-129">Die nicht eigenständigen Entitäten müssen keine expliziten Schlüsselwerte angeben, die gespeichert werden sollen:</span><span class="sxs-lookup"><span data-stu-id="1f616-129">The owned entities don't need to provide explicit key values to be stored:</span></span>

[!code-csharp[OwnedCollection](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=OwnedCollection)]

<span data-ttu-id="1f616-130">Sie werden auf diese Weise persistent gespeichert:</span><span class="sxs-lookup"><span data-stu-id="1f616-130">They will be persisted in this way:</span></span>

``` json
{
    "Id": 1,
    "Discriminator": "Distributor",
    "id": "Distributor|1",
    "ShippingCenters": [
        {
            "City": "Phoenix",
            "Discriminator": "StreetAddress",
            "Street": "500 S 48th Street"
        },
        {
            "City": "Anaheim",
            "Discriminator": "StreetAddress",
            "Street": "5650 Dolly Ave"
        }
    ],
    "_rid": "6QEKANzISj0BAAAAAAAAAA==",
    "_self": "dbs/6QEKAA==/colls/6QEKANzISj0=/docs/6QEKANzISj0BAAAAAAAAAA==/",
    "_etag": "\"00000000-0000-0000-683c-7b2b439701d5\"",
    "_attachments": "attachments/",
    "_ts": 1568163705
}
```

<span data-ttu-id="1f616-131">Intern muss EF Core für alle nachverfolgten Entitäten immer eindeutige Schlüsselwerte aufweisen.</span><span class="sxs-lookup"><span data-stu-id="1f616-131">Internally EF Core always needs to have unique key values for all tracked entities.</span></span> <span data-ttu-id="1f616-132">Der Primärschlüssel, der standardmäßig für Sammlungen nicht eigenständiger Typen erstellt wird, besteht aus den Fremdschlüsseleigenschaften, die auf den Besitzer verweisen, und einer `int`-Eigenschaft, die dem Index im JSON-Array entspricht.</span><span class="sxs-lookup"><span data-stu-id="1f616-132">The primary key created by default for collections of owned types consists of the foreign key properties pointing to the owner and an `int` property corresponding to the index in the JSON array.</span></span> <span data-ttu-id="1f616-133">Zum Abrufen dieser Werte kann die Eingabe-API verwendet werden:</span><span class="sxs-lookup"><span data-stu-id="1f616-133">To retrieve these values entry API could be used:</span></span>

[!code-csharp[ImpliedProperties](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=ImpliedProperties)]

> [!TIP]
> <span data-ttu-id="1f616-134">Bei Bedarf kann der standardmäßige Primärschlüssel für die nicht eigenständigen Entitätstypen geändert werden. Dann sollten jedoch Schlüsselwerte explizit angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="1f616-134">When necessary the default primary key for the owned entity types can be changed, but then key values should be provided explicitly.</span></span>

## <a name="working-with-disconnected-entities"></a><span data-ttu-id="1f616-135">Arbeiten mit getrennten Entitäten</span><span class="sxs-lookup"><span data-stu-id="1f616-135">Working with Disconnected Entities</span></span>

<span data-ttu-id="1f616-136">Jedes Element muss über einen `id`-Wert verfügen, der für den angegebenen Partitionsschlüssel eindeutig ist.</span><span class="sxs-lookup"><span data-stu-id="1f616-136">Every item needs to have an `id` value that is unique for the given partition key.</span></span> <span data-ttu-id="1f616-137">Standardmäßig generiert EF Core den Wert durch Verkettung der Diskriminatorwerte und der Primärschlüsselwerte, wobei „|“ als Trennzeichen verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="1f616-137">By default EF Core generates the value by concatenating the discriminator and the primary key values, using '|' as a delimiter.</span></span> <span data-ttu-id="1f616-138">Die Schlüsselwerte werden nur generiert, wenn eine Entität in den Zustand `Added` wechselt.</span><span class="sxs-lookup"><span data-stu-id="1f616-138">The key values are only generated when an entity enters the `Added` state.</span></span> <span data-ttu-id="1f616-139">Dies kann ein Problem darstellen, wenn Sie [Entitäten anfügen](../../saving/disconnected-entities.md), wenn diese nicht über eine `id`-Eigenschaft für den .NET-Typ verfügen, um den Wert zu speichern.</span><span class="sxs-lookup"><span data-stu-id="1f616-139">This might pose a problem when [attaching entities](../../saving/disconnected-entities.md) if they don't have an `id` property on the .NET type to store the value.</span></span>

<span data-ttu-id="1f616-140">Um diese Einschränkung zu umgehen, können Sie den `id`-Wert manuell erstellen und festlegen oder die Entität zuerst als hinzugefügt markieren und dann in den gewünschten Zustand ändern:</span><span class="sxs-lookup"><span data-stu-id="1f616-140">To work around this limitation one could create and set the `id` value manually or mark the entity as added first, then changing it to the desired state:</span></span>

[!code-csharp[Attach](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?highlight=4&name=Attach)]

<span data-ttu-id="1f616-141">Dieser JSON-Code ergibt sich:</span><span class="sxs-lookup"><span data-stu-id="1f616-141">This is the resulting JSON:</span></span>

``` json
{
    "Id": 1,
    "Discriminator": "Order",
    "TrackingNumber": null,
    "id": "Order|1",
    "Address": {
        "ShipsToCity": "London",
        "Discriminator": "StreetAddress",
        "ShipsToStreet": "3 Abbey Road"
    },
    "_rid": "6QEKAM+BOOABAAAAAAAAAA==",
    "_self": "dbs/6QEKAA==/colls/6QEKAM+BOOA=/docs/6QEKAM+BOOABAAAAAAAAAA==/",
    "_etag": "\"00000000-0000-0000-683c-8f7ac48f01d5\"",
    "_attachments": "attachments/",
    "_ts": 1568163739
}
```