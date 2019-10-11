---
title: 'Portieren von EF6 auf EF Core portieren eines Code basierten Modells: EF'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: 0a99eac2091c07d8bcf7d4e5e4bdc2afcaeee810
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181214"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a><span data-ttu-id="26d33-102">Portieren eines EF6-Code basierten Modells auf EF Core</span><span class="sxs-lookup"><span data-stu-id="26d33-102">Porting an EF6 Code-Based Model to EF Core</span></span>

<span data-ttu-id="26d33-103">Wenn Sie alle Vorsichtsmaßnahmen gelesen haben und Sie zum Portieren bereit sind, finden Sie hier einige Richtlinien, die Ihnen den Einstieg erleichtern.</span><span class="sxs-lookup"><span data-stu-id="26d33-103">If you've read all the caveats and you are ready to port, then here are some guidelines to help you get started.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="26d33-104">Installieren von EF Core nuget-Paketen</span><span class="sxs-lookup"><span data-stu-id="26d33-104">Install EF Core NuGet packages</span></span>

<span data-ttu-id="26d33-105">Um EF Core zu verwenden, installieren Sie das nuget-Paket für den Datenbankanbieter, den Sie verwenden möchten.</span><span class="sxs-lookup"><span data-stu-id="26d33-105">To use EF Core, you install the NuGet package for the database provider you want to use.</span></span> <span data-ttu-id="26d33-106">Wenn Sie z. b. auf SQL Server abzielen, würden Sie `Microsoft.EntityFrameworkCore.SqlServer` installieren.</span><span class="sxs-lookup"><span data-stu-id="26d33-106">For example, when targeting SQL Server, you would install `Microsoft.EntityFrameworkCore.SqlServer`.</span></span> <span data-ttu-id="26d33-107">Weitere Informationen finden Sie unter [Datenbankanbieter](../../core/providers/index.md) .</span><span class="sxs-lookup"><span data-stu-id="26d33-107">See [Database Providers](../../core/providers/index.md) for details.</span></span>

<span data-ttu-id="26d33-108">Wenn Sie beabsichtigen, Migrationen zu verwenden, sollten Sie auch das `Microsoft.EntityFrameworkCore.Tools`-Paket installieren.</span><span class="sxs-lookup"><span data-stu-id="26d33-108">If you are planning to use migrations, then you should also install the `Microsoft.EntityFrameworkCore.Tools` package.</span></span>

<span data-ttu-id="26d33-109">Es ist in Ordnung, das EF6 nuget-Paket (EntityFramework) installiert zu lassen, da EF Core und EF6 parallel in der gleichen Anwendung verwendet werden können.</span><span class="sxs-lookup"><span data-stu-id="26d33-109">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="26d33-110">Wenn Sie jedoch nicht beabsichtigen, EF6 in einem Bereich Ihrer Anwendung zu verwenden, kann das Deinstallieren des Pakets dabei helfen, Kompilier Fehler für Code Elemente zu erhalten, die eine Aufmerksamkeit erfordern.</span><span class="sxs-lookup"><span data-stu-id="26d33-110">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="swap-namespaces"></a><span data-ttu-id="26d33-111">Namespaces austauschen</span><span class="sxs-lookup"><span data-stu-id="26d33-111">Swap namespaces</span></span>

<span data-ttu-id="26d33-112">Die meisten APIs, die Sie in EF6 verwenden, sind im `System.Data.Entity`-Namespace (und den zugehörigen Subnamespaces) enthalten.</span><span class="sxs-lookup"><span data-stu-id="26d33-112">Most APIs that you use in EF6 are in the `System.Data.Entity` namespace (and related sub-namespaces).</span></span> <span data-ttu-id="26d33-113">Die erste Codeänderung besteht darin, den `Microsoft.EntityFrameworkCore`-Namespace auszutauschen.</span><span class="sxs-lookup"><span data-stu-id="26d33-113">The first code change is to swap to the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="26d33-114">In der Regel beginnen Sie mit der abgeleiteten Kontext Codedatei und können dann von dort aus arbeiten, um Kompilierungsfehler zu beheben, sobald sie auftreten.</span><span class="sxs-lookup"><span data-stu-id="26d33-114">You would typically start with your derived context code file and then work out from there, addressing compilation errors as they occur.</span></span>

## <a name="context-configuration-connection-etc"></a><span data-ttu-id="26d33-115">Kontext Konfiguration (Verbindung usw.)</span><span class="sxs-lookup"><span data-stu-id="26d33-115">Context configuration (connection etc.)</span></span>

<span data-ttu-id="26d33-116">Wie unter [sicherstellen, dass EF Core für Ihre Anwendung funktioniert](ensure-requirements.md), hat EF Core weniger Magie im Hinblick auf das Erkennen der Datenbank, mit der eine Verbindung hergestellt werden soll.</span><span class="sxs-lookup"><span data-stu-id="26d33-116">As described in [Ensure EF Core Will Work for Your Application](ensure-requirements.md), EF Core has less magic around detecting the database to connect to.</span></span> <span data-ttu-id="26d33-117">Sie müssen die `OnConfiguring`-Methode in Ihrem abgeleiteten Kontext überschreiben und die Datenbankanbieter spezifische API verwenden, um die Verbindung mit der Datenbank einzurichten.</span><span class="sxs-lookup"><span data-stu-id="26d33-117">You will need to override the `OnConfiguring` method on your derived context, and use the database provider specific API to setup the connection to the database.</span></span>

<span data-ttu-id="26d33-118">In den meisten EF6-Anwendungen wird die Verbindungs Zeichenfolge in der Anwendung `App/Web.config` gespeichert.</span><span class="sxs-lookup"><span data-stu-id="26d33-118">Most EF6 applications store the connection string in the applications `App/Web.config` file.</span></span> <span data-ttu-id="26d33-119">In EF Core lesen Sie diese Verbindungs Zeichenfolge mit der `ConfigurationManager`-API.</span><span class="sxs-lookup"><span data-stu-id="26d33-119">In EF Core, you read this connection string using the `ConfigurationManager` API.</span></span> <span data-ttu-id="26d33-120">Sie müssen möglicherweise auf die Framework-Assembly `System.Configuration` verweisen, um diese API verwenden zu können.</span><span class="sxs-lookup"><span data-stu-id="26d33-120">You may need to add a reference to the `System.Configuration` framework assembly to be able to use this API.</span></span>

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

## <a name="update-your-code"></a><span data-ttu-id="26d33-121">Aktualisieren Ihres Codes</span><span class="sxs-lookup"><span data-stu-id="26d33-121">Update your code</span></span>

<span data-ttu-id="26d33-122">An dieser Stelle ist es wichtig, Kompilierungsfehler zu beheben und Code zu überprüfen, um festzustellen, ob sich die Verhaltensänderungen auf Sie auswirken.</span><span class="sxs-lookup"><span data-stu-id="26d33-122">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes will impact you.</span></span>

## <a name="existing-migrations"></a><span data-ttu-id="26d33-123">Vorhandene Migrationen</span><span class="sxs-lookup"><span data-stu-id="26d33-123">Existing migrations</span></span>

<span data-ttu-id="26d33-124">Es ist nicht wirklich möglich, vorhandene EF6-Migrationen auf EF Core zu portieren.</span><span class="sxs-lookup"><span data-stu-id="26d33-124">There isn't really a feasible way to port existing EF6 migrations to EF Core.</span></span>

<span data-ttu-id="26d33-125">Wenn möglich, sollten Sie davon ausgehen, dass alle vorherigen Migrationen von EF6 auf die Datenbank angewendet wurden und dann EF Core mit der Migration des Schemas von diesem Punkt aus beginnen.</span><span class="sxs-lookup"><span data-stu-id="26d33-125">If possible, it is best to assume that all previous migrations from EF6 have been applied to the database and then start migrating the schema from that point using EF Core.</span></span> <span data-ttu-id="26d33-126">Dazu verwenden Sie den Befehl "`Add-Migration`", um eine Migration hinzuzufügen, sobald das Modell in EF Core portiert wurde.</span><span class="sxs-lookup"><span data-stu-id="26d33-126">To do this, you would use the `Add-Migration` command to add a migration once the model is ported to EF Core.</span></span> <span data-ttu-id="26d33-127">Anschließend würden Sie den gesamten Code aus den Methoden `Up` und `Down` der Gerüst Migration entfernen.</span><span class="sxs-lookup"><span data-stu-id="26d33-127">You would then remove all code from the `Up` and `Down` methods of the scaffolded migration.</span></span> <span data-ttu-id="26d33-128">Nachfolgende Migrationen werden mit dem Modell verglichen, wenn für die anfängliche Migration ein Gerüst erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="26d33-128">Subsequent migrations will compare to the model when that initial migration was scaffolded.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="26d33-129">Testen des Ports</span><span class="sxs-lookup"><span data-stu-id="26d33-129">Test the port</span></span>

<span data-ttu-id="26d33-130">Nur weil Ihre Anwendung kompiliert, bedeutet nicht, dass Sie erfolgreich in EF Core portiert wurde.</span><span class="sxs-lookup"><span data-stu-id="26d33-130">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="26d33-131">Sie müssen alle Bereiche Ihrer Anwendung testen, um sicherzustellen, dass keine der Verhaltensänderungen Ihre Anwendung beeinträchtigt hat.</span><span class="sxs-lookup"><span data-stu-id="26d33-131">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>
