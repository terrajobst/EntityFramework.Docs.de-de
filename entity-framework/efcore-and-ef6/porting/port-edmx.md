---
title: Portieren von EF6 auf EF Core – Portieren eines EDMX-basierten Modells
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: 2c3336ac675a830566001a0ddb3777839f52db18
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997410"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a><span data-ttu-id="d2bb6-102">Portieren eines EDMX-Datei-basierten Modells mit EF6 zu EF Core</span><span class="sxs-lookup"><span data-stu-id="d2bb6-102">Porting an EF6 EDMX-Based Model to EF Core</span></span>

<span data-ttu-id="d2bb6-103">EF Core unterstützt nicht das EDMX-Dateiformat für Modelle.</span><span class="sxs-lookup"><span data-stu-id="d2bb6-103">EF Core does not support the EDMX file format for models.</span></span> <span data-ttu-id="d2bb6-104">So portieren Sie diese Modelle, die beste Option ist ein neues, codebasierte Modell aus der Datenbank für Ihre Anwendung zu generieren.</span><span class="sxs-lookup"><span data-stu-id="d2bb6-104">The best option to port these models, is to generate a new code-based model from the database for your application.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="d2bb6-105">Installieren von EF Core NuGet-Paketen</span><span class="sxs-lookup"><span data-stu-id="d2bb6-105">Install EF Core NuGet packages</span></span>

<span data-ttu-id="d2bb6-106">Installieren Sie die `Microsoft.EntityFrameworkCore.Tools` NuGet-Paket.</span><span class="sxs-lookup"><span data-stu-id="d2bb6-106">Install the `Microsoft.EntityFrameworkCore.Tools` NuGet package.</span></span>

## <a name="regenerate-the-model"></a><span data-ttu-id="d2bb6-107">Das Modell erneut generieren</span><span class="sxs-lookup"><span data-stu-id="d2bb6-107">Regenerate the model</span></span>

<span data-ttu-id="d2bb6-108">Sie können jetzt die reverse Engineering-Funktionen verwenden, zum Erstellen eines Modells, basierend auf Ihrer vorhandenen Datenbank.</span><span class="sxs-lookup"><span data-stu-id="d2bb6-108">You can now use the reverse engineer functionality to create a model based on your existing database.</span></span>

<span data-ttu-id="d2bb6-109">Führen den folgenden Befehl in der Paket-Manager-Konsole (Tools Memberauswahloperator NuGet-Paket-Manager-Paket-Manager-Konsole Memberauswahloperator).</span><span class="sxs-lookup"><span data-stu-id="d2bb6-109">Run the following command in Package Manager Console (Tools –> NuGet Package Manager –> Package Manager Console).</span></span> <span data-ttu-id="d2bb6-110">Finden Sie unter [-Paket-Manager-Konsole (Visual Studio)](../../core/miscellaneous/cli/powershell.md) für Befehlsoptionen, um eine Teilmenge der Tabellen usw. zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="d2bb6-110">See [Package Manager Console (Visual Studio)](../../core/miscellaneous/cli/powershell.md) for command options to scaffold a subset of tables etc.</span></span>

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

<span data-ttu-id="d2bb6-111">Hier ist z. B. der Befehl zum Erstellen des Gerüsts für ein Modell aus der Blogging-Datenbank auf der SQL Server-LocalDB-Instanz.</span><span class="sxs-lookup"><span data-stu-id="d2bb6-111">For example, here is the command to scaffold a model from the Blogging database on your SQL Server LocalDB instance.</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a><span data-ttu-id="d2bb6-112">Entfernen von EF6-Modell</span><span class="sxs-lookup"><span data-stu-id="d2bb6-112">Remove EF6 model</span></span>

<span data-ttu-id="d2bb6-113">Nun möchten, das EF6-Modell von der Anwendung entfernen.</span><span class="sxs-lookup"><span data-stu-id="d2bb6-113">You would now remove the EF6 model from your application.</span></span>

<span data-ttu-id="d2bb6-114">Es ist in Ordnung, das EF6-NuGet-Paket ("EntityFramework"), die installiert ist, zu verlassen, da EF Core und EF6 verwendete Seite-an-Seite in der gleichen Anwendung sein kann.</span><span class="sxs-lookup"><span data-stu-id="d2bb6-114">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="d2bb6-115">Jedoch wenn Sie nicht sind EF6 in alle Bereiche der Anwendung verwenden, können klicken Sie dann das Paket deinstallieren Kompilierungsfehler auf Teile des Codes, die Ihre Aufmerksamkeit erfordern.</span><span class="sxs-lookup"><span data-stu-id="d2bb6-115">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="update-your-code"></a><span data-ttu-id="d2bb6-116">Aktualisieren Sie Ihren code</span><span class="sxs-lookup"><span data-stu-id="d2bb6-116">Update your code</span></span>

<span data-ttu-id="d2bb6-117">An diesem Punkt ist es eine Frage der Adressierung Kompilierungsfehler und Überprüfung von Code zu überprüfen, ob die verhaltensänderungen zwischen EF6 und EF Core Auswirkungen informiert.</span><span class="sxs-lookup"><span data-stu-id="d2bb6-117">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes between EF6 and EF Core will impact you.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="d2bb6-118">Testen Sie den port</span><span class="sxs-lookup"><span data-stu-id="d2bb6-118">Test the port</span></span>

<span data-ttu-id="d2bb6-119">Nur weil die Anwendung kompiliert wird, bedeutet nicht, dass es erfolgreich in EF Core portiert werden.</span><span class="sxs-lookup"><span data-stu-id="d2bb6-119">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="d2bb6-120">Sie benötigen, um alle Bereiche der Anwendung, um sicherzustellen, dass keine der Änderungen des Verhaltens Ihrer Anwendung beeinträchtigt haben zu testen.</span><span class="sxs-lookup"><span data-stu-id="d2bb6-120">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>

> [!TIP]
> <span data-ttu-id="d2bb6-121">Finden Sie unter [erste Schritte mit EF Core in ASP.NET Core mit einer vorhandenen Datenbank](xref:core/get-started/aspnetcore/existing-db) für einen zusätzlichen Verweis auf das Arbeiten mit einer vorhandenen Datenbank</span><span class="sxs-lookup"><span data-stu-id="d2bb6-121">See [Getting Started with EF Core on ASP.NET Core with an Existing Database](xref:core/get-started/aspnetcore/existing-db) for an additional reference on how to work with an existing database,</span></span> 
