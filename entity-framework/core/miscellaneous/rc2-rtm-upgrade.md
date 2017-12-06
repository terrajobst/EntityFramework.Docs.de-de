---
title: Aktualisieren von EF Core 1.0 RTM - RC2 EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c3c1940b-136d-45d8-aa4f-cb5040f8980a
ms.technology: entity-framework-core
uid: core/miscellaneous/rc2-rtm-upgrade
ms.openlocfilehash: 7a1d85949a5f9e1ad7efdbf585a608d815e8ce63
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
---
# <a name="upgrading-from-ef-core-10-rc2-to-rtm"></a><span data-ttu-id="5585e-102">Aktualisieren von EF Core 1.0 RC2 auf RTM-Version</span><span class="sxs-lookup"><span data-stu-id="5585e-102">Upgrading from EF Core 1.0 RC2 to RTM</span></span>

<span data-ttu-id="5585e-103">Dieser Artikel enthält Hinweise zum Verschieben einer Anwendung erstellt, die mit den Paketen 1.0.0 RC2 RTM-Version.</span><span class="sxs-lookup"><span data-stu-id="5585e-103">This article provides guidance for moving an application built with the RC2 packages to 1.0.0 RTM.</span></span>

## <a name="package-versions"></a><span data-ttu-id="5585e-104">Paketversionen</span><span class="sxs-lookup"><span data-stu-id="5585e-104">Package Versions</span></span>

<span data-ttu-id="5585e-105">Die Namen der obersten Ebene Pakete, die Sie in der Regel in einer Anwendung installieren möchten zwischen RC2 und RTM nicht ändern.</span><span class="sxs-lookup"><span data-stu-id="5585e-105">The names of the top level packages that you would typically install into an application did not change between RC2 and RTM.</span></span>

<span data-ttu-id="5585e-106">**Sie müssen die installierten Pakete auf die RTM-Versionen aktualisieren:**</span><span class="sxs-lookup"><span data-stu-id="5585e-106">**You need to upgrade the installed packages to the RTM versions:**</span></span>

* <span data-ttu-id="5585e-107">Laufzeit-Pakete (z. B. `Microsoft.EntityFrameworkCore.SqlServer`) angebotskennzeichen `1.0.0-rc2-final` auf `1.0.0`.</span><span class="sxs-lookup"><span data-stu-id="5585e-107">Runtime packages (e.g. `Microsoft.EntityFrameworkCore.SqlServer`) changed from `1.0.0-rc2-final` to `1.0.0`.</span></span>

* <span data-ttu-id="5585e-108">Die `Microsoft.EntityFrameworkCore.Tools` Paket angebotskennzeichen `1.0.0-preview1-final` auf `1.0.0-preview2-final`.</span><span class="sxs-lookup"><span data-stu-id="5585e-108">The `Microsoft.EntityFrameworkCore.Tools` package changed from `1.0.0-preview1-final` to `1.0.0-preview2-final`.</span></span> <span data-ttu-id="5585e-109">Beachten Sie, dass die Tools sind immer noch eine Vorabversion ist.</span><span class="sxs-lookup"><span data-stu-id="5585e-109">Note that tooling is still pre-release.</span></span>

## <a name="existing-migrations-may-need-maxlength-added"></a><span data-ttu-id="5585e-110">Vorhandene Migrationen möglicherweise MaxLength hinzugefügt</span><span class="sxs-lookup"><span data-stu-id="5585e-110">Existing migrations may need maxLength added</span></span>

<span data-ttu-id="5585e-111">In RC2 die Spaltendefinition in einer Migration aussah `table.Column<string>(nullable: true)` und die Länge der Spalte in einige Metadaten wir in den Code für die Migration speichern nachgeschlagen wurde.</span><span class="sxs-lookup"><span data-stu-id="5585e-111">In RC2, the column definition in a migration looked like `table.Column<string>(nullable: true)` and the length of the column was looked up in some metadata we store in the code behind the migration.</span></span> <span data-ttu-id="5585e-112">In der RTM-Version, die Länge jetzt im scaffolded Code enthalten `table.Column<string>(maxLength: 450, nullable: true)`.</span><span class="sxs-lookup"><span data-stu-id="5585e-112">In RTM, the length is now included in the scaffolded code `table.Column<string>(maxLength: 450, nullable: true)`.</span></span>

<span data-ttu-id="5585e-113">Alle vorhandenen Migrationen, bei denen vor der Verwendung von RTM Gerüstbau wurden keine der `maxLength` Argument wurde angegeben.</span><span class="sxs-lookup"><span data-stu-id="5585e-113">Any existing migrations that were scaffolded prior to using RTM will not have the `maxLength` argument specified.</span></span> <span data-ttu-id="5585e-114">Dies bedeutet, dass die maximale Länge, die von der Datenbank unterstützt werden kann (`nvarchar(max)` auf SQL Server).</span><span class="sxs-lookup"><span data-stu-id="5585e-114">This means the maximum length supported by the database will be used (`nvarchar(max)` on SQL Server).</span></span> <span data-ttu-id="5585e-115">Dies ist möglicherweise für einige Spalten, jedoch die Spalten, die Teil eines Schlüssels, Fremdschlüssel, sind in Ordnung, oder Index muss aktualisiert werden, um eine maximale Länge enthalten.</span><span class="sxs-lookup"><span data-stu-id="5585e-115">This may be fine for some columns, but columns that are part of a key, foreign key, or index need to be updated to include a maximum length.</span></span> <span data-ttu-id="5585e-116">Gemäß der Konvention ist 450 die maximale Länge für Schlüssel, Fremdschlüssel, verwendet und volltextindizierte Spalten.</span><span class="sxs-lookup"><span data-stu-id="5585e-116">By convention, 450 is the maximum length used for keys, foreign keys, and indexed columns.</span></span> <span data-ttu-id="5585e-117">Wenn Sie explizit eine Länge in das Modell konfiguriert haben, sollten Sie stattdessen diese Länge verwenden.</span><span class="sxs-lookup"><span data-stu-id="5585e-117">If you have explicitly configured a length in the model, then you should use that length instead.</span></span>

<span data-ttu-id="5585e-118">**ASP.NET Identity**</span><span class="sxs-lookup"><span data-stu-id="5585e-118">**ASP.NET Identity**</span></span>

<span data-ttu-id="5585e-119">Diese Änderung wirkt sich auf Projekte, die ASP.NET Identity verwendet und erstellt wurden, aus einer vor-RTM-Projektvorlage.</span><span class="sxs-lookup"><span data-stu-id="5585e-119">This change impacts projects that use ASP.NET Identity and were created from a pre-RTM project template.</span></span> <span data-ttu-id="5585e-120">Die Projektvorlage enthält eine Migration zum Erstellen der Datenbank verwendet.</span><span class="sxs-lookup"><span data-stu-id="5585e-120">The project template includes a migration used to create the database.</span></span> <span data-ttu-id="5585e-121">Diese Migration bearbeitet werden muss, zum Angeben einer maximalen Länge von `256` für die folgenden Spalten.</span><span class="sxs-lookup"><span data-stu-id="5585e-121">This migration must be edited to specify a maximum length of `256` for the following columns.</span></span>

*  <span data-ttu-id="5585e-122">**AspNetRoles**</span><span class="sxs-lookup"><span data-stu-id="5585e-122">**AspNetRoles**</span></span>

    * <span data-ttu-id="5585e-123">Name</span><span class="sxs-lookup"><span data-stu-id="5585e-123">Name</span></span>

    * <span data-ttu-id="5585e-124">NormalizedName</span><span class="sxs-lookup"><span data-stu-id="5585e-124">NormalizedName</span></span>

*  <span data-ttu-id="5585e-125">**AspNetUsers**</span><span class="sxs-lookup"><span data-stu-id="5585e-125">**AspNetUsers**</span></span>

   * <span data-ttu-id="5585e-126">E-Mail</span><span class="sxs-lookup"><span data-stu-id="5585e-126">Email</span></span>

   * <span data-ttu-id="5585e-127">NormalizedEmail</span><span class="sxs-lookup"><span data-stu-id="5585e-127">NormalizedEmail</span></span>

   * <span data-ttu-id="5585e-128">NormalizedUserName</span><span class="sxs-lookup"><span data-stu-id="5585e-128">NormalizedUserName</span></span>

   * <span data-ttu-id="5585e-129">UserName</span><span class="sxs-lookup"><span data-stu-id="5585e-129">UserName</span></span>

<span data-ttu-id="5585e-130">Diese Änderung wird in der folgenden Ausnahme ansonsten bei die anfängliche Migration auf eine Datenbank angewendet wird.</span><span class="sxs-lookup"><span data-stu-id="5585e-130">Failure to make this change will result in the following exception when the initial migration is applied to a database.</span></span>

    System.Data.SqlClient.SqlException (0x80131904): Column 'NormalizedName' in table 'AspNetRoles' is of a type that is invalid for use as a key column in an index.

## <a name="net-core-remove-imports-in-projectjson"></a><span data-ttu-id="5585e-131">.NET Core: Entfernen von "Imports" in "Project.JSON"</span><span class="sxs-lookup"><span data-stu-id="5585e-131">.NET Core: Remove "imports" in project.json</span></span>

<span data-ttu-id="5585e-132">Bei der Ausrichtung auf .NET Core mit RC2 wurden benötigt hinzuzufügende `imports` auf "Project.JSON" als vorübergehende problemumgehung für einige der EF kernabhängigkeiten .NET Standard, nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="5585e-132">If you were targeting .NET Core with RC2, you needed to add `imports` to project.json as a temporary workaround for some of EF Core's dependencies not supporting .NET Standard.</span></span> <span data-ttu-id="5585e-133">Diese können jetzt entfernt werden.</span><span class="sxs-lookup"><span data-stu-id="5585e-133">These can now be removed.</span></span>

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

## <a name="uwp-add-binding-redirects"></a><span data-ttu-id="5585e-134">Universelle Windows-Plattform: Fügen Sie bindungsumleitungen hinzu</span><span class="sxs-lookup"><span data-stu-id="5585e-134">UWP: Add binding redirects</span></span>

<span data-ttu-id="5585e-135">Es wird versucht, EF-Befehle auf die universelle Windows-Plattform (UWP) Teamprojekten führt zu folgendem Fehler ausgeführt:</span><span class="sxs-lookup"><span data-stu-id="5585e-135">Attempting to run EF commands on Universal Windows Platform (UWP) projects results in the following error:</span></span>

    System.IO.FileLoadException: Could not load file or assembly 'System.IO.FileSystem.Primitives, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' or one of its dependencies. The located assembly's manifest definition does not match the assembly reference.

<span data-ttu-id="5585e-136">In diesem Fall müssen Sie eine uwp-Projekt manuell bindungsumleitungen hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="5585e-136">You need to manually add binding redirects to the UWP project.</span></span> <span data-ttu-id="5585e-137">Erstellen Sie eine Datei mit dem Namen `App.config` im Projekt root-Ordner und die richtigen Assemblyversionen leitet hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="5585e-137">Create a file named `App.config` in the project root folder and add redirects to the correct assembly versions.</span></span>

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
