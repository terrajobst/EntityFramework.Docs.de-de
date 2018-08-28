---
title: Portieren von EF6 auf EF Core – Portieren eines codebasierten Modells
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: 2484b681d71ae8711b1b3a59bc274a0b2e403294
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997048"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a><span data-ttu-id="97f74-102">Portieren einer codebasierten Modells mit EF6 zu EF Core</span><span class="sxs-lookup"><span data-stu-id="97f74-102">Porting an EF6 Code-Based Model to EF Core</span></span>

<span data-ttu-id="97f74-103">Wenn Sie alle die Einschränkungen gelesen haben und portieren möchten, klicken Sie dann sind hier einige Richtlinien, die Ihnen beim Einstieg helfen.</span><span class="sxs-lookup"><span data-stu-id="97f74-103">If you've read all the caveats and you are ready to port, then here are some guidelines to help you get started.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="97f74-104">Installieren von EF Core NuGet-Paketen</span><span class="sxs-lookup"><span data-stu-id="97f74-104">Install EF Core NuGet packages</span></span>

<span data-ttu-id="97f74-105">Um EF Core zu verwenden, installieren Sie das NuGet-Paket für den Anbieter, die, den Sie verwenden möchten.</span><span class="sxs-lookup"><span data-stu-id="97f74-105">To use EF Core, you install the NuGet package for the database provider you want to use.</span></span> <span data-ttu-id="97f74-106">Beispielsweise, wenn SQL Server angezielt wurde, Sie würden installieren `Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="97f74-106">For example, when targeting SQL Server, you would install `Microsoft.EntityFrameworkCore.SqlServer`.</span></span> <span data-ttu-id="97f74-107">Finden Sie unter [Datenbankanbieter](../../core/providers/index.md) Details.</span><span class="sxs-lookup"><span data-stu-id="97f74-107">See [Database Providers](../../core/providers/index.md) for details.</span></span>

<span data-ttu-id="97f74-108">Wenn Sie Migrationen verwenden möchten, sollten Sie auch installieren die `Microsoft.EntityFrameworkCore.Tools` Paket.</span><span class="sxs-lookup"><span data-stu-id="97f74-108">If you are planning to use migrations, then you should also install the `Microsoft.EntityFrameworkCore.Tools` package.</span></span>

<span data-ttu-id="97f74-109">Es ist in Ordnung, das EF6-NuGet-Paket ("EntityFramework"), die installiert ist, zu verlassen, da EF Core und EF6 verwendete Seite-an-Seite in der gleichen Anwendung sein kann.</span><span class="sxs-lookup"><span data-stu-id="97f74-109">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="97f74-110">Jedoch wenn Sie nicht sind EF6 in alle Bereiche der Anwendung verwenden, können klicken Sie dann das Paket deinstallieren Kompilierungsfehler auf Teile des Codes, die Ihre Aufmerksamkeit erfordern.</span><span class="sxs-lookup"><span data-stu-id="97f74-110">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="swap-namespaces"></a><span data-ttu-id="97f74-111">Swap-namespaces</span><span class="sxs-lookup"><span data-stu-id="97f74-111">Swap namespaces</span></span>

<span data-ttu-id="97f74-112">Die meisten APIs, die in EF6 verwendet werden, der `System.Data.Entity` Namespace (und verwandte untergeordneten Namespaces).</span><span class="sxs-lookup"><span data-stu-id="97f74-112">Most APIs that you use in EF6 are in the `System.Data.Entity` namespace (and related sub-namespaces).</span></span> <span data-ttu-id="97f74-113">Die erste Änderung des Codes wird zum Austauschen der `Microsoft.EntityFrameworkCore` Namespace.</span><span class="sxs-lookup"><span data-stu-id="97f74-113">The first code change is to swap to the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="97f74-114">Sie würden normalerweise beginnen Sie mit Ihrem abgeleiteten Kontext Codedatei und dann ausarbeiten dort Adressierung Kompilierungsfehler, sobald sie auftreten.</span><span class="sxs-lookup"><span data-stu-id="97f74-114">You would typically start with your derived context code file and then work out from there, addressing compilation errors as they occur.</span></span>

## <a name="context-configuration-connection-etc"></a><span data-ttu-id="97f74-115">Kontextbezogene Konfigurationen der (Verbindung usw.).</span><span class="sxs-lookup"><span data-stu-id="97f74-115">Context configuration (connection etc.)</span></span>

<span data-ttu-id="97f74-116">Siehe [stellen Sie sicher EF Core wird Arbeit für die Anwendung](ensure-requirements.md), EF Core verfügt über weniger Magic-Befehl in der Datenbank für die Verbindung zu erkennen.</span><span class="sxs-lookup"><span data-stu-id="97f74-116">As described in [Ensure EF Core Will Work for Your Application](ensure-requirements.md), EF Core has less magic around detecting the database to connect to.</span></span> <span data-ttu-id="97f74-117">Sie benötigen zum Überschreiben der `OnConfiguring` Methode auf Ihrem abgeleiteten Kontext ein, und verwenden Sie die jeweilige Datenbank-Anbieter-API, um das Einrichten der Verbindung mit der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="97f74-117">You will need to override the `OnConfiguring` method on your derived context, and use the database provider specific API to setup the connection to the database.</span></span>

<span data-ttu-id="97f74-118">Die meisten EF6-Anwendungen speichern die Verbindungszeichenfolge in den Anwendungen `App/Web.config` Datei.</span><span class="sxs-lookup"><span data-stu-id="97f74-118">Most EF6 applications store the connection string in the applications `App/Web.config` file.</span></span> <span data-ttu-id="97f74-119">In EF Core Sie lesen diese Verbindungszeichenfolge mithilfe der `ConfigurationManager` API.</span><span class="sxs-lookup"><span data-stu-id="97f74-119">In EF Core, you read this connection string using the `ConfigurationManager` API.</span></span> <span data-ttu-id="97f74-120">Sie müssen möglicherweise einen Verweis auf Hinzufügen der `System.Configuration` Framework-Assembly, um diese API verwenden zu können.</span><span class="sxs-lookup"><span data-stu-id="97f74-120">You may need to add a reference to the `System.Configuration` framework assembly to be able to use this API.</span></span>

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

## <a name="update-your-code"></a><span data-ttu-id="97f74-121">Aktualisieren Sie Ihren code</span><span class="sxs-lookup"><span data-stu-id="97f74-121">Update your code</span></span>

<span data-ttu-id="97f74-122">An diesem Punkt ist es lediglich die Adressierung von Kompilierungsfehlern und Überprüfen von Code, um festzustellen, ob die verhaltensänderungen Auswirkungen informiert.</span><span class="sxs-lookup"><span data-stu-id="97f74-122">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes will impact you.</span></span>

## <a name="existing-migrations"></a><span data-ttu-id="97f74-123">Vorhandener Migrationen</span><span class="sxs-lookup"><span data-stu-id="97f74-123">Existing migrations</span></span>

<span data-ttu-id="97f74-124">Derzeit nicht wirklich möglich, vorhandene EF6-Migrationen zu EF Core zu portieren.</span><span class="sxs-lookup"><span data-stu-id="97f74-124">There isn't really a feasible way to port existing EF6 migrations to EF Core.</span></span>

<span data-ttu-id="97f74-125">Wenn möglich, empfiehlt es sich davon ausgehen, dass alle vorherige Migrationen von EF 6 auf die Datenbank angewendet wurden, und zeigen Sie dann das Schema aus, die Migration starten mit EF Core.</span><span class="sxs-lookup"><span data-stu-id="97f74-125">If possible, it is best to assume that all previous migrations from EF6 have been applied to the database and then start migrating the schema from that point using EF Core.</span></span> <span data-ttu-id="97f74-126">Zu diesem Zweck verwenden Sie die `Add-Migration` Befehl aus, um eine Migration hinzufügen, nachdem das Modell zu EF Core portiert wird.</span><span class="sxs-lookup"><span data-stu-id="97f74-126">To do this, you would use the `Add-Migration` command to add a migration once the model is ported to EF Core.</span></span> <span data-ttu-id="97f74-127">Sie würden dann entfernen Sie sämtlichen Code aus der `Up` und `Down` Methoden der erstellten Migration.</span><span class="sxs-lookup"><span data-stu-id="97f74-127">You would then remove all code from the `Up` and `Down` methods of the scaffolded migration.</span></span> <span data-ttu-id="97f74-128">Bei der anfänglichen Migration Gerüst erstellt wurde, werden die nachfolgende Migrationen für das Modell verglichen.</span><span class="sxs-lookup"><span data-stu-id="97f74-128">Subsequent migrations will compare to the model when that initial migration was scaffolded.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="97f74-129">Testen Sie den port</span><span class="sxs-lookup"><span data-stu-id="97f74-129">Test the port</span></span>

<span data-ttu-id="97f74-130">Nur weil die Anwendung kompiliert wird, bedeutet nicht, dass es erfolgreich in EF Core portiert werden.</span><span class="sxs-lookup"><span data-stu-id="97f74-130">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="97f74-131">Sie benötigen, um alle Bereiche der Anwendung, um sicherzustellen, dass keine der Änderungen des Verhaltens Ihrer Anwendung beeinträchtigt haben zu testen.</span><span class="sxs-lookup"><span data-stu-id="97f74-131">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>
