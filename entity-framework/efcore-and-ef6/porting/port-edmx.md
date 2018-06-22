---
title: Portieren von EF6 auf EF-Core - Portieren eine EDMX-basiertes Modell
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: c999d2114c76ee3a7615ae897b42ee3376cff14e
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/26/2018
ms.locfileid: "31812689"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a><span data-ttu-id="d033a-102">Portieren einer EF6 EDMX-basiertes Modell EF-Kern</span><span class="sxs-lookup"><span data-stu-id="d033a-102">Porting an EF6 EDMX-Based Model to EF Core</span></span>

<span data-ttu-id="d033a-103">EF Core unterstützt nicht das Format der EDMX-Datei für Modelle.</span><span class="sxs-lookup"><span data-stu-id="d033a-103">EF Core does not support the EDMX file format for models.</span></span> <span data-ttu-id="d033a-104">Die beste Option So portieren Sie diese Modelle werden aus der Datenbank für Ihre Anwendung ein neues Code basierende Modell generieren.</span><span class="sxs-lookup"><span data-stu-id="d033a-104">The best option to port these models, is to generate a new code-based model from the database for your application.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="d033a-105">Installieren von EF Core NuGet-Pakete</span><span class="sxs-lookup"><span data-stu-id="d033a-105">Install EF Core NuGet packages</span></span>

<span data-ttu-id="d033a-106">Installieren der `Microsoft.EntityFrameworkCore.Tools` NuGet-Paket.</span><span class="sxs-lookup"><span data-stu-id="d033a-106">Install the `Microsoft.EntityFrameworkCore.Tools` NuGet package.</span></span>

## <a name="regenerate-the-model"></a><span data-ttu-id="d033a-107">Das Modell erneut generieren</span><span class="sxs-lookup"><span data-stu-id="d033a-107">Regenerate the model</span></span>

<span data-ttu-id="d033a-108">Sie können jetzt die reverse Engineering-Funktionen verwenden, zum Erstellen eines Modells, basierend auf der vorhandenen Datenbank.</span><span class="sxs-lookup"><span data-stu-id="d033a-108">You can now use the reverse engineer functionality to create a model based on your existing database.</span></span>

<span data-ttu-id="d033a-109">Führen den folgenden Befehl in der Paket-Manager-Konsole (Tools Klassenschlüssel NuGet Package Manager Klassenschlüssel Paket-Manager-Konsole).</span><span class="sxs-lookup"><span data-stu-id="d033a-109">Run the following command in Package Manager Console (Tools –> NuGet Package Manager –> Package Manager Console).</span></span> <span data-ttu-id="d033a-110">Finden Sie unter [Package Manager Console (Visual Studio)](../../core/miscellaneous/cli/powershell.md) für Befehlsoptionen, um das Gerüst für eine Teilmenge der Tabellen usw. erstellen.</span><span class="sxs-lookup"><span data-stu-id="d033a-110">See [Package Manager Console (Visual Studio)](../../core/miscellaneous/cli/powershell.md) for command options to scaffold a subset of tables etc.</span></span>

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

<span data-ttu-id="d033a-111">Hier ist z. B. den Befehl aus, um ein Modell aus der Blogging-Datenbank auf der SQL Server-LocalDB-Instanz zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="d033a-111">For example, here is the command to scaffold a model from the Blogging database on your SQL Server LocalDB instance.</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a><span data-ttu-id="d033a-112">Entfernen von EF6-Modell</span><span class="sxs-lookup"><span data-stu-id="d033a-112">Remove EF6 model</span></span>

<span data-ttu-id="d033a-113">Das Modell EF6 würden jetzt von der Anwendung entfernt werden.</span><span class="sxs-lookup"><span data-stu-id="d033a-113">You would now remove the EF6 model from your application.</span></span>

<span data-ttu-id="d033a-114">Es ist in Ordnung, um das EF6 NuGet-Paket (EntityFramework) installiert ist, zu lassen, als EF Core und EF6 verwendete Seite-an-Seite in derselben Anwendung sein können.</span><span class="sxs-lookup"><span data-stu-id="d033a-114">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="d033a-115">Jedoch wenn Sie nicht sind EF6 in alle Bereiche der Anwendung zu verwenden, können klicken Sie dann das Paket deinstallieren Kompilierungsfehler auf Codeabschnitte, die Aufmerksamkeit erfordern.</span><span class="sxs-lookup"><span data-stu-id="d033a-115">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="update-your-code"></a><span data-ttu-id="d033a-116">Aktualisieren Sie den code</span><span class="sxs-lookup"><span data-stu-id="d033a-116">Update your code</span></span>

<span data-ttu-id="d033a-117">An diesem Punkt ist es nur wenige der Adressierung Kompilierungsfehler und Überprüfung von Code zu überprüfen, ob die verhaltensänderungen zwischen EF6 und EF Core auswirkt.</span><span class="sxs-lookup"><span data-stu-id="d033a-117">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes between EF6 and EF Core will impact you.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="d033a-118">Testen Sie den port</span><span class="sxs-lookup"><span data-stu-id="d033a-118">Test the port</span></span>

<span data-ttu-id="d033a-119">Einfach, da die Anwendung kompiliert wird, bedeutet nicht, dass es erfolgreich zu EF Core portiert werden.</span><span class="sxs-lookup"><span data-stu-id="d033a-119">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="d033a-120">Sie müssen zum Testen der Anwendung, um sicherzustellen, dass keines der verhaltensänderungen Ihrer Anwendung negativ beeinträchtigt, haben alle Bereiche.</span><span class="sxs-lookup"><span data-stu-id="d033a-120">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>

> [!TIP]
> <span data-ttu-id="d033a-121">Finden Sie unter [erste Schritte mit EF Core unter ASP.NET Core mit einer vorhandenen Datenbank](xref:core/get-started/aspnetcore/existing-db) für ein zusätzlicher Verweis zum Arbeiten mit einer vorhandenen Datenbank</span><span class="sxs-lookup"><span data-stu-id="d033a-121">See [Getting Started with EF Core on ASP.NET Core with an Existing Database](xref:core/get-started/aspnetcore/existing-db) for an additional reference on how to work with an existing database,</span></span> 
