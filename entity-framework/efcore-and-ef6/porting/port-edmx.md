---
title: Portieren von EF6 auf EF Core portieren eines edmx-basierten Modells
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: a1c978e141f47a39fff59eb5fc417a63afd37b04
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149000"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a><span data-ttu-id="715c6-102">Portieren eines EF6 edmx-basierten Modells auf EF Core</span><span class="sxs-lookup"><span data-stu-id="715c6-102">Porting an EF6 EDMX-Based Model to EF Core</span></span>

<span data-ttu-id="715c6-103">EF Core unterstützt das edmx-Dateiformat für Modelle nicht.</span><span class="sxs-lookup"><span data-stu-id="715c6-103">EF Core does not support the EDMX file format for models.</span></span> <span data-ttu-id="715c6-104">Die beste Option zum Portieren dieser Modelle besteht darin, ein neues Code basiertes Modell aus der Datenbank für Ihre Anwendung zu generieren.</span><span class="sxs-lookup"><span data-stu-id="715c6-104">The best option to port these models, is to generate a new code-based model from the database for your application.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="715c6-105">Installieren von EF Core nuget-Paketen</span><span class="sxs-lookup"><span data-stu-id="715c6-105">Install EF Core NuGet packages</span></span>

<span data-ttu-id="715c6-106">Installieren Sie das NuGet-Paket `Microsoft.EntityFrameworkCore.Tools`.</span><span class="sxs-lookup"><span data-stu-id="715c6-106">Install the `Microsoft.EntityFrameworkCore.Tools` NuGet package.</span></span>

## <a name="regenerate-the-model"></a><span data-ttu-id="715c6-107">Erneutes Generieren des Modells</span><span class="sxs-lookup"><span data-stu-id="715c6-107">Regenerate the model</span></span>

<span data-ttu-id="715c6-108">Sie können jetzt die Reverse-Engineering-Funktion verwenden, um ein Modell auf der Grundlage der vorhandenen Datenbank zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="715c6-108">You can now use the reverse engineer functionality to create a model based on your existing database.</span></span>

<span data-ttu-id="715c6-109">Führen Sie den folgenden Befehl in der Paket-Manager-Konsole aus (Tools – > nuget-Paket-Manager – > Paket-Manager-Konsole).</span><span class="sxs-lookup"><span data-stu-id="715c6-109">Run the following command in Package Manager Console (Tools –> NuGet Package Manager –> Package Manager Console).</span></span> <span data-ttu-id="715c6-110">In der [Paket-Manager-Konsole (Visual Studio)](../../core/miscellaneous/cli/powershell.md) finden Sie Befehlsoptionen zum Gerüstbau einer Teilmenge von Tabellen usw.</span><span class="sxs-lookup"><span data-stu-id="715c6-110">See [Package Manager Console (Visual Studio)](../../core/miscellaneous/cli/powershell.md) for command options to scaffold a subset of tables etc.</span></span>

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

<span data-ttu-id="715c6-111">Hier sehen Sie z. b. den Befehl, um ein Modell aus der BLOB-Datenbank auf Ihrer SQL Server localdb-Instanz zu Gerüstbau.</span><span class="sxs-lookup"><span data-stu-id="715c6-111">For example, here is the command to scaffold a model from the Blogging database on your SQL Server LocalDB instance.</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a><span data-ttu-id="715c6-112">Entfernen des EF6-Modells</span><span class="sxs-lookup"><span data-stu-id="715c6-112">Remove EF6 model</span></span>

<span data-ttu-id="715c6-113">Nun entfernen Sie das EF6-Modell aus der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="715c6-113">You would now remove the EF6 model from your application.</span></span>

<span data-ttu-id="715c6-114">Es ist in Ordnung, das EF6 nuget-Paket (EntityFramework) installiert zu lassen, da EF Core und EF6 parallel in der gleichen Anwendung verwendet werden können.</span><span class="sxs-lookup"><span data-stu-id="715c6-114">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="715c6-115">Wenn Sie jedoch nicht beabsichtigen, EF6 in einem Bereich Ihrer Anwendung zu verwenden, kann das Deinstallieren des Pakets dabei helfen, Kompilier Fehler für Code Elemente zu erhalten, die eine Aufmerksamkeit erfordern.</span><span class="sxs-lookup"><span data-stu-id="715c6-115">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="update-your-code"></a><span data-ttu-id="715c6-116">Aktualisieren Ihres Codes</span><span class="sxs-lookup"><span data-stu-id="715c6-116">Update your code</span></span>

<span data-ttu-id="715c6-117">An dieser Stelle ist es wichtig, Kompilierungsfehler zu beheben und Code zu überprüfen, um festzustellen, ob sich die Verhaltensänderungen zwischen EF6 und EF Core auf Sie auswirken.</span><span class="sxs-lookup"><span data-stu-id="715c6-117">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes between EF6 and EF Core will impact you.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="715c6-118">Testen des Ports</span><span class="sxs-lookup"><span data-stu-id="715c6-118">Test the port</span></span>

<span data-ttu-id="715c6-119">Nur weil Ihre Anwendung kompiliert, bedeutet nicht, dass Sie erfolgreich in EF Core portiert wurde.</span><span class="sxs-lookup"><span data-stu-id="715c6-119">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="715c6-120">Sie müssen alle Bereiche Ihrer Anwendung testen, um sicherzustellen, dass keine der Verhaltensänderungen Ihre Anwendung beeinträchtigt hat.</span><span class="sxs-lookup"><span data-stu-id="715c6-120">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>
