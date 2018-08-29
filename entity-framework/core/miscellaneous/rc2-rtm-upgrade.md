---
title: Aktualisieren von EF Core 1.0 RC2 auf RTM – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c3c1940b-136d-45d8-aa4f-cb5040f8980a
uid: core/miscellaneous/rc2-rtm-upgrade
ms.openlocfilehash: 1b95b2ab1943dfb541b3a7c873cff3cb4c16d9c1
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998318"
---
# <a name="upgrading-from-ef-core-10-rc2-to-rtm"></a><span data-ttu-id="f60b0-102">Aktualisieren von EF Core 1.0 RC2 auf RTM-Version</span><span class="sxs-lookup"><span data-stu-id="f60b0-102">Upgrading from EF Core 1.0 RC2 to RTM</span></span>

<span data-ttu-id="f60b0-103">Dieser Artikel enthält Anleitungen zum Verschieben von einer Anwendung erstellt, mit dem RC2-Paketen auf 1.0.0 RTM-Version.</span><span class="sxs-lookup"><span data-stu-id="f60b0-103">This article provides guidance for moving an application built with the RC2 packages to 1.0.0 RTM.</span></span>

## <a name="package-versions"></a><span data-ttu-id="f60b0-104">Paketversionen</span><span class="sxs-lookup"><span data-stu-id="f60b0-104">Package Versions</span></span>

<span data-ttu-id="f60b0-105">Die Namen der Pakete auf oberster Ebene, die Sie in der Regel in eine Anwendung installieren würden wurde zwischen RC2 und RTM-Version nicht geändert werden.</span><span class="sxs-lookup"><span data-stu-id="f60b0-105">The names of the top level packages that you would typically install into an application did not change between RC2 and RTM.</span></span>

<span data-ttu-id="f60b0-106">**Sie müssen die installierten Pakete auf die RTM-Versionen zu aktualisieren:**</span><span class="sxs-lookup"><span data-stu-id="f60b0-106">**You need to upgrade the installed packages to the RTM versions:**</span></span>

* <span data-ttu-id="f60b0-107">Runtime-Pakete (z. B. `Microsoft.EntityFrameworkCore.SqlServer`) von geändert `1.0.0-rc2-final` zu `1.0.0`.</span><span class="sxs-lookup"><span data-stu-id="f60b0-107">Runtime packages (for example, `Microsoft.EntityFrameworkCore.SqlServer`) changed from `1.0.0-rc2-final` to `1.0.0`.</span></span>

* <span data-ttu-id="f60b0-108">Die `Microsoft.EntityFrameworkCore.Tools` Paket geändert `1.0.0-preview1-final` zu `1.0.0-preview2-final`.</span><span class="sxs-lookup"><span data-stu-id="f60b0-108">The `Microsoft.EntityFrameworkCore.Tools` package changed from `1.0.0-preview1-final` to `1.0.0-preview2-final`.</span></span> <span data-ttu-id="f60b0-109">Beachten Sie, dass Tools immer noch eine Vorabversion.</span><span class="sxs-lookup"><span data-stu-id="f60b0-109">Note that tooling is still pre-release.</span></span>

## <a name="existing-migrations-may-need-maxlength-added"></a><span data-ttu-id="f60b0-110">Vorhandene Migrationen möglicherweise MaxLength hinzugefügt</span><span class="sxs-lookup"><span data-stu-id="f60b0-110">Existing migrations may need maxLength added</span></span>

<span data-ttu-id="f60b0-111">In RC2, sah die Spaltendefinition in einer Migration `table.Column<string>(nullable: true)` und die Länge der Spalte in einige Metadaten wir in den Code hinter der Migration speichern gesucht wurde.</span><span class="sxs-lookup"><span data-stu-id="f60b0-111">In RC2, the column definition in a migration looked like `table.Column<string>(nullable: true)` and the length of the column was looked up in some metadata we store in the code behind the migration.</span></span> <span data-ttu-id="f60b0-112">In der RTM-Version ist jetzt die Länge in dem eingerüsteten Code enthalten `table.Column<string>(maxLength: 450, nullable: true)`.</span><span class="sxs-lookup"><span data-stu-id="f60b0-112">In RTM, the length is now included in the scaffolded code `table.Column<string>(maxLength: 450, nullable: true)`.</span></span>

<span data-ttu-id="f60b0-113">Alle vorhandenen Migrationen, bei denen Gerüst erstellt wurden, vor der Verwendung von RTM-Version müssen nicht die `maxLength` Argument angegeben.</span><span class="sxs-lookup"><span data-stu-id="f60b0-113">Any existing migrations that were scaffolded prior to using RTM will not have the `maxLength` argument specified.</span></span> <span data-ttu-id="f60b0-114">Dies bedeutet, dass die maximale Länge, die von der Datenbank unterstützt werden kann (`nvarchar(max)` auf SQL Server).</span><span class="sxs-lookup"><span data-stu-id="f60b0-114">This means the maximum length supported by the database will be used (`nvarchar(max)` on SQL Server).</span></span> <span data-ttu-id="f60b0-115">Dies ist möglicherweise für einige Spalten, aber die Spalten, die Teil eines Schlüssels, foreign key- oder Index müssen aktualisiert werden, um eine maximale Länge enthalten.</span><span class="sxs-lookup"><span data-stu-id="f60b0-115">This may be fine for some columns, but columns that are part of a key, foreign key, or index need to be updated to include a maximum length.</span></span> <span data-ttu-id="f60b0-116">Gemäß der Konvention ist 450, die maximale Länge für Schlüssel, Fremdschlüssel, verwendet und die indizierten Spalten.</span><span class="sxs-lookup"><span data-stu-id="f60b0-116">By convention, 450 is the maximum length used for keys, foreign keys, and indexed columns.</span></span> <span data-ttu-id="f60b0-117">Wenn Sie eine Länge explizit in das Modell konfiguriert haben, sollten Sie stattdessen diese Länge verwenden.</span><span class="sxs-lookup"><span data-stu-id="f60b0-117">If you have explicitly configured a length in the model, then you should use that length instead.</span></span>

<span data-ttu-id="f60b0-118">**ASP.NET Identity**</span><span class="sxs-lookup"><span data-stu-id="f60b0-118">**ASP.NET Identity**</span></span>

<span data-ttu-id="f60b0-119">Diese Änderung wirkt sich auf die Projekte, die aus einer vorab erstellten und Verwenden von ASP.NET Identity-RTM-Projektvorlage.</span><span class="sxs-lookup"><span data-stu-id="f60b0-119">This change impacts projects that use ASP.NET Identity and were created from a pre-RTM project template.</span></span> <span data-ttu-id="f60b0-120">Die Projektvorlage enthält eine Migration verwendet, um die Datenbank zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="f60b0-120">The project template includes a migration used to create the database.</span></span> <span data-ttu-id="f60b0-121">Diese Migration muss bearbeitet werden, zum Angeben einer maximalen Länge von `256` für die folgenden Spalten.</span><span class="sxs-lookup"><span data-stu-id="f60b0-121">This migration must be edited to specify a maximum length of `256` for the following columns.</span></span>

*  <span data-ttu-id="f60b0-122">**AspNetRoles**</span><span class="sxs-lookup"><span data-stu-id="f60b0-122">**AspNetRoles**</span></span>

    * <span data-ttu-id="f60b0-123">name</span><span class="sxs-lookup"><span data-stu-id="f60b0-123">Name</span></span>

    * <span data-ttu-id="f60b0-124">NormalizedName</span><span class="sxs-lookup"><span data-stu-id="f60b0-124">NormalizedName</span></span>

*  <span data-ttu-id="f60b0-125">**AspNetUsers**</span><span class="sxs-lookup"><span data-stu-id="f60b0-125">**AspNetUsers**</span></span>

   * <span data-ttu-id="f60b0-126">E-Mail</span><span class="sxs-lookup"><span data-stu-id="f60b0-126">Email</span></span>

   * <span data-ttu-id="f60b0-127">NormalizedEmail</span><span class="sxs-lookup"><span data-stu-id="f60b0-127">NormalizedEmail</span></span>

   * <span data-ttu-id="f60b0-128">NormalizedUserName</span><span class="sxs-lookup"><span data-stu-id="f60b0-128">NormalizedUserName</span></span>

   * <span data-ttu-id="f60b0-129">UserName</span><span class="sxs-lookup"><span data-stu-id="f60b0-129">UserName</span></span>

<span data-ttu-id="f60b0-130">Fehler für diese Änderung führt in der folgenden Ausnahme, wenn die ursprüngliche Migration auf eine Datenbank angewendet wird.</span><span class="sxs-lookup"><span data-stu-id="f60b0-130">Failure to make this change will result in the following exception when the initial migration is applied to a database.</span></span>

    System.Data.SqlClient.SqlException (0x80131904): Column 'NormalizedName' in table 'AspNetRoles' is of a type that is invalid for use as a key column in an index.

## <a name="net-core-remove-imports-in-projectjson"></a><span data-ttu-id="f60b0-131">.NET Core: Entfernen von "Imports" in "Project.JSON"</span><span class="sxs-lookup"><span data-stu-id="f60b0-131">.NET Core: Remove "imports" in project.json</span></span>

<span data-ttu-id="f60b0-132">Wenn Sie .NET Core mit RC2 verwenden wurden, mussten Sie fügen `imports` Datei "Project.JSON" als vorübergehende problemumgehung für einige der EF Core Abhängigkeiten, die nicht unterstützen .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="f60b0-132">If you were targeting .NET Core with RC2, you needed to add `imports` to project.json as a temporary workaround for some of EF Core's dependencies not supporting .NET Standard.</span></span> <span data-ttu-id="f60b0-133">Diese können jetzt entfernt werden.</span><span class="sxs-lookup"><span data-stu-id="f60b0-133">These can now be removed.</span></span>

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

> [!NOTE]  
> <span data-ttu-id="f60b0-134">Ab Version 1.0 RTM-Version der [.NET Core SDK](https://www.microsoft.com/net/download/core) unterstützt nicht mehr `project.json` oder .NET Core-Anwendungen mit Visual Studio 2015 entwickeln.</span><span class="sxs-lookup"><span data-stu-id="f60b0-134">As of version 1.0 RTM, the [.NET Core SDK](https://www.microsoft.com/net/download/core) no longer supports `project.json` or developing .NET Core applications using Visual Studio 2015.</span></span> <span data-ttu-id="f60b0-135">Es wird empfohlen, [eine Migration von „project.json“ zu „csproj“ durchzuführen](https://docs.microsoft.com/dotnet/articles/core/migration/).</span><span class="sxs-lookup"><span data-stu-id="f60b0-135">We recommend you [migrate from project.json to csproj](https://docs.microsoft.com/dotnet/articles/core/migration/).</span></span> <span data-ttu-id="f60b0-136">Wenn Sie Visual Studio verwenden, sollten Sie Sie ein upgrade auf [Visual Studio 2017](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="f60b0-136">If you are using Visual Studio, we recommend you upgrade to [Visual Studio 2017](https://www.visualstudio.com/downloads/).</span></span>

## <a name="uwp-add-binding-redirects"></a><span data-ttu-id="f60b0-137">UWP: Fügen Sie bindungsumleitungen hinzu</span><span class="sxs-lookup"><span data-stu-id="f60b0-137">UWP: Add binding redirects</span></span>

<span data-ttu-id="f60b0-138">Es wird versucht, EF-Befehle auf der universellen Windows-Plattform (UWP) von Teamprojekten führt zu folgendem Fehler ausgeführt:</span><span class="sxs-lookup"><span data-stu-id="f60b0-138">Attempting to run EF commands on Universal Windows Platform (UWP) projects results in the following error:</span></span>

    System.IO.FileLoadException: Could not load file or assembly 'System.IO.FileSystem.Primitives, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' or one of its dependencies. The located assembly's manifest definition does not match the assembly reference.

<span data-ttu-id="f60b0-139">Sie müssen das UWP-Projekt manuell bindungsumleitungen hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="f60b0-139">You need to manually add binding redirects to the UWP project.</span></span> <span data-ttu-id="f60b0-140">Erstellen Sie eine Datei mit dem Namen `App.config` im Projekt Stammordner, und die richtigen Assemblyversionen umleitungen hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="f60b0-140">Create a file named `App.config` in the project root folder and add redirects to the correct assembly versions.</span></span>

``` xml
<configuration>
 <runtime>
   <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">
     <dependentAssembly>
       <assemblyIdentity name="System.IO.FileSystem.Primitives"
                         publicKeyToken="b03f5f7f11d50a3a"
                         culture="neutral" />
       <bindingRedirect oldVersion="4.0.0.0"
                        newVersion="4.0.1.0"/>
     </dependentAssembly>
     <dependentAssembly>
       <assemblyIdentity name="System.Threading.Overlapped"
                         publicKeyToken="b03f5f7f11d50a3a"
                         culture="neutral" />
       <bindingRedirect oldVersion="4.0.0.0"
                        newVersion="4.0.1.0"/>
     </dependentAssembly>
   </assemblyBinding>
 </runtime>
</configuration>
```
