---
title: 'Portieren von EF6 auf EF Core: Portieren eines EDMX-basierten Modells'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: f0bb06dc687aaa774981d97daadc55f00fbd527e
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78413528"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a><span data-ttu-id="cc1dd-102">Portieren eines EDMX-basierten EF6-Modells auf EF Core</span><span class="sxs-lookup"><span data-stu-id="cc1dd-102">Porting an EF6 EDMX-Based Model to EF Core</span></span>

<span data-ttu-id="cc1dd-103">EF Core unterstützt das EDMX-Dateiformat nicht für Modelle.</span><span class="sxs-lookup"><span data-stu-id="cc1dd-103">EF Core does not support the EDMX file format for models.</span></span> <span data-ttu-id="cc1dd-104">Der beste Ansatz zum Portieren dieser Modelle besteht darin, ein neues codebasiertes Modell aus der Datenbank für Ihre Anwendung zu generieren.</span><span class="sxs-lookup"><span data-stu-id="cc1dd-104">The best option to port these models, is to generate a new code-based model from the database for your application.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="cc1dd-105">Installieren von EF Core-NuGet-Paketen</span><span class="sxs-lookup"><span data-stu-id="cc1dd-105">Install EF Core NuGet packages</span></span>

<span data-ttu-id="cc1dd-106">Installieren Sie das NuGet-Paket `Microsoft.EntityFrameworkCore.Tools`.</span><span class="sxs-lookup"><span data-stu-id="cc1dd-106">Install the `Microsoft.EntityFrameworkCore.Tools` NuGet package.</span></span>

## <a name="regenerate-the-model"></a><span data-ttu-id="cc1dd-107">Erneutes Generieren des Modells</span><span class="sxs-lookup"><span data-stu-id="cc1dd-107">Regenerate the model</span></span>

<span data-ttu-id="cc1dd-108">Sie können jetzt die Reverse-Engineering-Funktion verwenden, um ein Modell auf Grundlage der vorhandenen Datenbank zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="cc1dd-108">You can now use the reverse engineer functionality to create a model based on your existing database.</span></span>

<span data-ttu-id="cc1dd-109">Führen Sie in der Paket-Manager-Konsole (Extras > NuGet-Paket-Manager > Paket-Manager-Konsole) den folgenden Befehl aus.</span><span class="sxs-lookup"><span data-stu-id="cc1dd-109">Run the following command in Package Manager Console (Tools –> NuGet Package Manager –> Package Manager Console).</span></span> <span data-ttu-id="cc1dd-110">Im Artikel zur [Paket-Manager-Konsole in Visual Studio](../../core/miscellaneous/cli/powershell.md) finden Sie Befehlsoptionen für das Erstellen eines Gerüsts für eine Teilmenge von Tabellen oder anderen Komponenten.</span><span class="sxs-lookup"><span data-stu-id="cc1dd-110">See [Package Manager Console (Visual Studio)](../../core/miscellaneous/cli/powershell.md) for command options to scaffold a subset of tables etc.</span></span>

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

<span data-ttu-id="cc1dd-111">Hier sehen Sie z. B. den Befehl, um das Gerüst eines Modell aus der Blogging-Datenbank auf Ihrer SQL Server-LocalDB-Instanz zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="cc1dd-111">For example, here is the command to scaffold a model from the Blogging database on your SQL Server LocalDB instance.</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a><span data-ttu-id="cc1dd-112">Entfernen des EF6-Modells</span><span class="sxs-lookup"><span data-stu-id="cc1dd-112">Remove EF6 model</span></span>

<span data-ttu-id="cc1dd-113">An dieser Stelle entfernen Sie das EF6-Modell aus der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="cc1dd-113">You would now remove the EF6 model from your application.</span></span>

<span data-ttu-id="cc1dd-114">Sie müssen das EF6-NuGet-Paket (EntityFramework) nicht deinstallieren, da EF Core und EF6 parallel in derselben Anwendung verwendet werden können.</span><span class="sxs-lookup"><span data-stu-id="cc1dd-114">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="cc1dd-115">Wenn Sie jedoch nicht beabsichtigen, EF6 in Ihrer Anwendung zu verwenden, kann das Deinstallieren des Pakets dazu beitragen, dass nur zu den Codebestandteilen Kompilierfehler ausgegeben werden, die wirklich Ihre Aufmerksamkeit erfordern.</span><span class="sxs-lookup"><span data-stu-id="cc1dd-115">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="update-your-code"></a><span data-ttu-id="cc1dd-116">Aktualisieren Ihres Codes</span><span class="sxs-lookup"><span data-stu-id="cc1dd-116">Update your code</span></span>

<span data-ttu-id="cc1dd-117">An dieser Stelle ist es wichtig, Kompilierfehler zu beheben und ein Code Review durchzuführen, um festzustellen, ob die Verhaltensänderungen zwischen EF6 und EF Core eine Auswirkung zeigen.</span><span class="sxs-lookup"><span data-stu-id="cc1dd-117">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes between EF6 and EF Core will impact you.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="cc1dd-118">Testen des Ports</span><span class="sxs-lookup"><span data-stu-id="cc1dd-118">Test the port</span></span>

<span data-ttu-id="cc1dd-119">Nur weil Ihre Anwendung erfolgreich kompiliert wird, bedeutet das nicht, dass sie auch erfolgreich auf EF Core portiert wurde.</span><span class="sxs-lookup"><span data-stu-id="cc1dd-119">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="cc1dd-120">Sie müssen alle Bereiche Ihrer Anwendung testen, um sicherzustellen, dass keine der Verhaltensänderungen Ihre Anwendung beeinträchtigt hat.</span><span class="sxs-lookup"><span data-stu-id="cc1dd-120">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>
