---
title: Portieren von EF6 auf EF-Core - Portieren eine Code-basiertes Modell
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: a0fa4f9a7028f56666fb993185cb03eddb9a2cd1
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052950"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a><span data-ttu-id="2e23b-102">Portieren von EF6 Code basierende Modell EF-Kern</span><span class="sxs-lookup"><span data-stu-id="2e23b-102">Porting an EF6 Code-Based Model to EF Core</span></span>

<span data-ttu-id="2e23b-103">Wenn Sie alle Einschränkungen gelesen haben und Sie bereit, Port sind, sind hier einige Richtlinien, die Ihnen beim Einstieg helfen.</span><span class="sxs-lookup"><span data-stu-id="2e23b-103">If you've read all the caveats and you are ready to port, then here are some guidelines to help you get started.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="2e23b-104">Installieren von EF Core NuGet-Pakete</span><span class="sxs-lookup"><span data-stu-id="2e23b-104">Install EF Core NuGet packages</span></span>

<span data-ttu-id="2e23b-105">Wenn EF Core verwenden möchten, installieren Sie das NuGet-Paket für den Datenbankanbieter, die, den Sie verwenden möchten.</span><span class="sxs-lookup"><span data-stu-id="2e23b-105">To use EF Core, you install the NuGet package for the database provider you want to use.</span></span> <span data-ttu-id="2e23b-106">Beispielsweise, wenn SQL Server verwenden möchten, Sie würden installieren `Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="2e23b-106">For example, when targeting SQL Server, you would install `Microsoft.EntityFrameworkCore.SqlServer`.</span></span> <span data-ttu-id="2e23b-107">Finden Sie unter [Datenbankanbieter](../../core/providers/index.md) Details.</span><span class="sxs-lookup"><span data-stu-id="2e23b-107">See [Database Providers](../../core/providers/index.md) for details.</span></span>

<span data-ttu-id="2e23b-108">Wenn Sie planen die Migrationen zu verwenden, installieren Sie auch die `Microsoft.EntityFrameworkCore.Tools` Paket.</span><span class="sxs-lookup"><span data-stu-id="2e23b-108">If you are planning to use migrations, then you should also install the `Microsoft.EntityFrameworkCore.Tools` package.</span></span>

<span data-ttu-id="2e23b-109">Es ist in Ordnung, um das EF6 NuGet-Paket (EntityFramework) installiert ist, zu lassen, als EF Core und EF6 verwendete Seite-an-Seite in derselben Anwendung sein können.</span><span class="sxs-lookup"><span data-stu-id="2e23b-109">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="2e23b-110">Jedoch wenn Sie nicht sind EF6 in alle Bereiche der Anwendung zu verwenden, können klicken Sie dann das Paket deinstallieren Kompilierungsfehler auf Codeabschnitte, die Aufmerksamkeit erfordern.</span><span class="sxs-lookup"><span data-stu-id="2e23b-110">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="swap-namespaces"></a><span data-ttu-id="2e23b-111">Swap-namespaces</span><span class="sxs-lookup"><span data-stu-id="2e23b-111">Swap namespaces</span></span>

<span data-ttu-id="2e23b-112">Die meisten APIs, die in EF6 verwendet werden, der `System.Data.Entity` Namespace (und in verwandten Sub-Namespaces).</span><span class="sxs-lookup"><span data-stu-id="2e23b-112">Most APIs that you use in EF6 are in the `System.Data.Entity` namespace (and related sub-namespaces).</span></span> <span data-ttu-id="2e23b-113">Die erste Änderung des Codes wird zum Austauschen der `Microsoft.EntityFrameworkCore` Namespace.</span><span class="sxs-lookup"><span data-stu-id="2e23b-113">The first code change is to swap to the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="2e23b-114">Sie würden in der Regel beginnen Sie mit der abgeleiteten Kontext Codedatei und dann ausarbeiten dort Adressierung Kompilierungsfehler, sobald sie auftreten.</span><span class="sxs-lookup"><span data-stu-id="2e23b-114">You would typically start with your derived context code file and then work out from there, addressing compilation errors as they occur.</span></span>

## <a name="context-configuration-connection-etc"></a><span data-ttu-id="2e23b-115">Konfiguration des Sicherheitskontexts (Verbindung usw.).</span><span class="sxs-lookup"><span data-stu-id="2e23b-115">Context configuration (connection etc.)</span></span>

<span data-ttu-id="2e23b-116">Wie in beschrieben [sicherzustellen EF wird Hauptarbeit, das für die Anwendung](ensure-requirements.md), EF Core hat weniger Magic, um das Erkennen von der Datenbank für die Verbindung.</span><span class="sxs-lookup"><span data-stu-id="2e23b-116">As described in [Ensure EF Core Will Work for Your Application](ensure-requirements.md), EF Core has less magic around detecting the database to connect to.</span></span> <span data-ttu-id="2e23b-117">Müssen Sie überschreiben die `OnConfiguring` Methode auf dem abgeleiteten Kontext und verwenden Sie die Datenbank-API-Anbieter spezifische beim Einrichten der Verbindung mit der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="2e23b-117">You will need to override the `OnConfiguring` method on your derived context, and use the database provider specific API to setup the connection to the database.</span></span>

<span data-ttu-id="2e23b-118">Die meisten EF6 Anwendungen die Verbindungszeichenfolge speichern, die in der Anwendung `App/Web.config` Datei.</span><span class="sxs-lookup"><span data-stu-id="2e23b-118">Most EF6 applications store the connection string in the applications `App/Web.config` file.</span></span> <span data-ttu-id="2e23b-119">In der EF-Kern, Sie Lesen dieser Verbindungszeichenfolge mithilfe der `ConfigurationManager` API.</span><span class="sxs-lookup"><span data-stu-id="2e23b-119">In EF Core, you read this connection string using the `ConfigurationManager` API.</span></span> <span data-ttu-id="2e23b-120">Sie müssen möglicherweise einen Verweis auf Hinzufügen der `System.Configuration` Framework-Assembly, um diese API verwenden zu können.</span><span class="sxs-lookup"><span data-stu-id="2e23b-120">You may need to add a reference to the `System.Configuration` framework assembly to be able to use this API.</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
      optionsBuilder.UseSqlServer(ConfigurationManager.ConnectionStrings["BloggingDatabase"].ConnectionString);
    }
}
```

## <a name="update-your-code"></a><span data-ttu-id="2e23b-121">Aktualisieren Sie den code</span><span class="sxs-lookup"><span data-stu-id="2e23b-121">Update your code</span></span>

<span data-ttu-id="2e23b-122">An diesem Punkt ist es nur wenige Adressierung Kompilierungsfehler enthält, und Überprüfen von Code, um festzustellen, ob die verändertes Programmverhalten auswirkt.</span><span class="sxs-lookup"><span data-stu-id="2e23b-122">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes will impact you.</span></span>

## <a name="existing-migrations"></a><span data-ttu-id="2e23b-123">Vorhandene Migrationen</span><span class="sxs-lookup"><span data-stu-id="2e23b-123">Existing migrations</span></span>

<span data-ttu-id="2e23b-124">Es ist eigentlich möglich ermöglichen die vorhandene EF6-Migrationen zu EF Core.</span><span class="sxs-lookup"><span data-stu-id="2e23b-124">There isn't really a feasible way to port existing EF6 migrations to EF Core.</span></span>

<span data-ttu-id="2e23b-125">Wenn möglich, empfiehlt es sich, wird davon ausgegangen, dass alle vorherigen Migrationen von EF6 an die Datenbank angewendet wurden, und zeigen Sie dann das Schema aus, die Migration starten mit EF Core.</span><span class="sxs-lookup"><span data-stu-id="2e23b-125">If possible, it is best to assume that all previous migrations from EF6 have been applied to the database and then start migrating the schema from that point using EF Core.</span></span> <span data-ttu-id="2e23b-126">Zu diesem Zweck verwenden Sie die `Add-Migration` Befehl aus, um eine Migration hinzufügen, nachdem das Modell zu EF Core portiert werden.</span><span class="sxs-lookup"><span data-stu-id="2e23b-126">To do this, you would use the `Add-Migration` command to add a migration once the model is ported to EF Core.</span></span> <span data-ttu-id="2e23b-127">Sie würden dann entfernen Sie sämtlichen Code aus der `Up` und `Down` Methoden der scaffolded Migration.</span><span class="sxs-lookup"><span data-stu-id="2e23b-127">You would then remove all code from the `Up` and `Down` methods of the scaffolded migration.</span></span> <span data-ttu-id="2e23b-128">Nachfolgende Migrationen werden für das Modell verglichen, wenn diese anfänglichen Migration Gerüstbau wurde.</span><span class="sxs-lookup"><span data-stu-id="2e23b-128">Subsequent migrations will compare to the model when that initial migration was scaffolded.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="2e23b-129">Testen Sie den port</span><span class="sxs-lookup"><span data-stu-id="2e23b-129">Test the port</span></span>

<span data-ttu-id="2e23b-130">Einfach, da die Anwendung kompiliert wird, bedeutet nicht, dass es erfolgreich zu EF Core portiert werden.</span><span class="sxs-lookup"><span data-stu-id="2e23b-130">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="2e23b-131">Sie müssen zum Testen der Anwendung, um sicherzustellen, dass keines der verhaltensänderungen Ihrer Anwendung negativ beeinträchtigt, haben alle Bereiche.</span><span class="sxs-lookup"><span data-stu-id="2e23b-131">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>
