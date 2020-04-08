---
title: Erste Schritte – EF Core
author: rick-anderson
ms.date: 09/17/2019
ms.assetid: 3c88427c-20c6-42ec-a736-22d3eccd5071
uid: core/get-started/index
ms.openlocfilehash: 0e7a1ee159cdf5b72448fe6d73c972975b1ab95b
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/07/2020
ms.locfileid: "78412865"
---
# <a name="getting-started-with-ef-core"></a><span data-ttu-id="355c0-102">Erste Schritte mit EF Core</span><span class="sxs-lookup"><span data-stu-id="355c0-102">Getting Started with EF Core</span></span>

<span data-ttu-id="355c0-103">In diesem Tutorial erstellen Sie eine .NET Core-Konsolen-App, die Datenzugriff auf eine SQLite-Datenbank mithilfe von Entity Framework Core ausführt.</span><span class="sxs-lookup"><span data-stu-id="355c0-103">In this tutorial, you create a .NET Core console app that performs data access against a SQLite database using Entity Framework Core.</span></span>

<span data-ttu-id="355c0-104">Sie können das Tutorial mit Visual Studio unter Windows oder mithilfe von .NET Core-CLI unter Windows, MacOS oder Linux nachvollziehen.</span><span class="sxs-lookup"><span data-stu-id="355c0-104">You can follow the tutorial by using Visual Studio on Windows, or by using the .NET Core CLI on Windows, macOS, or Linux.</span></span>

<span data-ttu-id="355c0-105">[Sehen Sie sich das Beispiel aus diesem Artikel auf GitHub an](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/GetStarted).</span><span class="sxs-lookup"><span data-stu-id="355c0-105">[View this article's sample on GitHub](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/GetStarted).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="355c0-106">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="355c0-106">Prerequisites</span></span>

<span data-ttu-id="355c0-107">Installieren Sie die folgenden Softwarekomponenten:</span><span class="sxs-lookup"><span data-stu-id="355c0-107">Install the following software:</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="355c0-108">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="355c0-108">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="355c0-109">[.NET Core SDK](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="355c0-109">[.NET Core SDK](https://www.microsoft.com/net/download/core).</span></span>

### <a name="visual-studio"></a>[<span data-ttu-id="355c0-110">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="355c0-110">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="355c0-111">[Visual Studio 2019 Version 16.3 oder höher ](https://www.visualstudio.com/downloads/) mit dieser Workload:</span><span class="sxs-lookup"><span data-stu-id="355c0-111">[Visual Studio 2019 version 16.3 or later](https://www.visualstudio.com/downloads/) with this  workload:</span></span>
  * <span data-ttu-id="355c0-112">**Plattformübergreifende .NET Core-Entwicklung** (unter **Andere Toolsets**)</span><span class="sxs-lookup"><span data-stu-id="355c0-112">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>

---

## <a name="create-a-new-project"></a><span data-ttu-id="355c0-113">Erstellt ein neues Projekt</span><span class="sxs-lookup"><span data-stu-id="355c0-113">Create a new project</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="355c0-114">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="355c0-114">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new console -o EFGetStarted
cd EFGetStarted
```

### <a name="visual-studio"></a>[<span data-ttu-id="355c0-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="355c0-115">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="355c0-116">Öffnen Sie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="355c0-116">Open Visual Studio</span></span>
* <span data-ttu-id="355c0-117">Klicken Sie auf **Neues Projekt erstellen**.</span><span class="sxs-lookup"><span data-stu-id="355c0-117">Click **Create a new project**</span></span>
* <span data-ttu-id="355c0-118">Wählen Sie **Konsolen-App (.NET Core)** mit dem Tag **C#** aus, und klicken Sie auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="355c0-118">Select **Console App (.NET Core)** with the **C#** tag and click **Next**</span></span>
* <span data-ttu-id="355c0-119">Geben Sie als Namen **EFGetStarted** ein, und klicken Sie dann auf **Erstellen**.</span><span class="sxs-lookup"><span data-stu-id="355c0-119">Enter **EFGetStarted** for the name and click **Create**</span></span>

---

## <a name="install-entity-framework-core"></a><span data-ttu-id="355c0-120">Installieren von Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="355c0-120">Install Entity Framework Core</span></span>

<span data-ttu-id="355c0-121">Installieren Sie das Paket für den (oder die) gewünschten EF Core-Datenbankanbieter, um EF Core zu installieren.</span><span class="sxs-lookup"><span data-stu-id="355c0-121">To install EF Core, you install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="355c0-122">In diesem Tutorial wird SQLite verwendet, da es auf allen Plattformen ausgeführt werden kann, die .NET Core unterstützt.</span><span class="sxs-lookup"><span data-stu-id="355c0-122">This tutorial uses SQLite because it runs on all platforms that .NET Core supports.</span></span> <span data-ttu-id="355c0-123">Eine Liste der verfügbaren Anbieter finden Sie unter [Datenbankanbieter](../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="355c0-123">For a list of available providers, see [Database Providers](../providers/index.md).</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="355c0-124">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="355c0-124">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
```

### <a name="visual-studio"></a>[<span data-ttu-id="355c0-125">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="355c0-125">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="355c0-126">Wählen Sie **Tools > NuGet-Paket-Manager > Paket-Manager-Konsole** aus.</span><span class="sxs-lookup"><span data-stu-id="355c0-126">**Tools > NuGet Package Manager > Package Manager Console**</span></span>
* <span data-ttu-id="355c0-127">Führen Sie die folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="355c0-127">Run the following commands:</span></span>

  ``` PowerShell
  Install-Package Microsoft.EntityFrameworkCore.Sqlite
  ```

<span data-ttu-id="355c0-128">Tipp: Sie können Pakete auch installieren, indem Sie mit der rechten Maustaste auf das Projekt klicken und dann **NuGet-Pakete verwalten** auswählen.</span><span class="sxs-lookup"><span data-stu-id="355c0-128">Tip: You can also install packages by right-clicking on the project and selecting **Manage NuGet Packages**</span></span>

---

## <a name="create-the-model"></a><span data-ttu-id="355c0-129">Erstellen des Modells</span><span class="sxs-lookup"><span data-stu-id="355c0-129">Create the model</span></span>

<span data-ttu-id="355c0-130">Definieren Sie eine Kontextklasse und Entitätsklassen für das Modell.</span><span class="sxs-lookup"><span data-stu-id="355c0-130">Define a context class and entity classes that make up the model.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="355c0-131">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="355c0-131">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="355c0-132">Erstellen Sie im Projektverzeichnis die Datei **Model.cs** mit dem folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="355c0-132">In the project directory, create **Model.cs** with the following code</span></span>

### <a name="visual-studio"></a>[<span data-ttu-id="355c0-133">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="355c0-133">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="355c0-134">Klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie **Hinzufügen > Klasse** aus.</span><span class="sxs-lookup"><span data-stu-id="355c0-134">Right-click on the project and select **Add > Class**</span></span>
* <span data-ttu-id="355c0-135">Geben Sie als Namen **Model.cs** ein, und klicken Sie dann auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="355c0-135">Enter **Model.cs** as the name and click **Add**</span></span>
* <span data-ttu-id="355c0-136">Ersetzen Sie den Inhalt der Datei durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="355c0-136">Replace the contents of the file with the following code</span></span>

---

[!code-csharp[Main](../../../samples/core/GetStarted/Model.cs)]

<span data-ttu-id="355c0-137">EF Core kann auch [Reverse Engineering](../managing-schemas/scaffolding.md) eines Modells aus einer vorhandenen Datenbank ausführen.</span><span class="sxs-lookup"><span data-stu-id="355c0-137">EF Core can also [reverse engineer](../managing-schemas/scaffolding.md) a model from an existing database.</span></span>

<span data-ttu-id="355c0-138">Tipp: In einer echten App platzieren Sie jede Anwendung in einer separaten Datei und fügen die [Verbindungszeichenfolge](../miscellaneous/connection-strings.md) in eine Konfigurationsdatei oder Umgebungsvariable ein.</span><span class="sxs-lookup"><span data-stu-id="355c0-138">Tip: In a real app, you put each class in a separate file and put the [connection string](../miscellaneous/connection-strings.md) in a configuration file or environment variable.</span></span> <span data-ttu-id="355c0-139">Der Einfachheit halber sind in diesem Tutorial alle Elemente in einer Datei enthalten.</span><span class="sxs-lookup"><span data-stu-id="355c0-139">To keep the tutorial simple, everything is contained in one file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="355c0-140">Erstellen der Datenbank</span><span class="sxs-lookup"><span data-stu-id="355c0-140">Create the database</span></span>

<span data-ttu-id="355c0-141">Die folgenden Schritte verwenden [Migrationen](xref:core/managing-schemas/migrations/index), um eine Datenbank zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="355c0-141">The following steps use [migrations](xref:core/managing-schemas/migrations/index) to create a database.</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="355c0-142">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="355c0-142">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="355c0-143">Führen Sie die folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="355c0-143">Run the following commands:</span></span>

  ```dotnetcli
  dotnet tool install --global dotnet-ef
  dotnet add package Microsoft.EntityFrameworkCore.Design
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

  <span data-ttu-id="355c0-144">Hierdurch werden [dotnet ef](../miscellaneous/cli/dotnet.md) und das Entwurfspaket installiert, das zum Ausführen des Befehls für ein Projekt erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="355c0-144">This installs [dotnet ef](../miscellaneous/cli/dotnet.md) and the design package which is required to run the command on a project.</span></span> <span data-ttu-id="355c0-145">Der Befehl `migrations` richtet per Gerüstbau eine Migration ein und erstellt den anfänglichen Tabellensatz für das Modell.</span><span class="sxs-lookup"><span data-stu-id="355c0-145">The `migrations` command scaffolds a migration to create the initial set of tables for the model.</span></span> <span data-ttu-id="355c0-146">Der Befehl `database update` erstellt die Datenbank und wendet die neue Migration auf sie an.</span><span class="sxs-lookup"><span data-stu-id="355c0-146">The `database update` command creates the database and applies the new migration to it.</span></span>

### <a name="visual-studio"></a>[<span data-ttu-id="355c0-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="355c0-147">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="355c0-148">Führen Sie die folgenden Befehle in der **Paket-Manager-Konsole** aus.</span><span class="sxs-lookup"><span data-stu-id="355c0-148">Run the following commands in **Package Manager Console**</span></span>

  ``` PowerShell
  Install-Package Microsoft.EntityFrameworkCore.Tools
  Add-Migration InitialCreate
  Update-Database
  ```

  <span data-ttu-id="355c0-149">Dadurch werden die [PMC-Tools für EF Core](../miscellaneous/cli/powershell.md) installiert.</span><span class="sxs-lookup"><span data-stu-id="355c0-149">This installs the [PMC tools for EF Core](../miscellaneous/cli/powershell.md).</span></span> <span data-ttu-id="355c0-150">Der Befehl `Add-Migration` richtet per Gerüstbau eine Migration ein und erstellt den anfänglichen Tabellensatz für das Modell.</span><span class="sxs-lookup"><span data-stu-id="355c0-150">The `Add-Migration` command scaffolds a migration to create the initial set of tables for the model.</span></span> <span data-ttu-id="355c0-151">Der Befehl `Update-Database` erstellt die Datenbank und wendet die neue Migration auf sie an.</span><span class="sxs-lookup"><span data-stu-id="355c0-151">The `Update-Database` command creates the database and applies the new migration to it.</span></span>

---

## <a name="create-read-update--delete"></a><span data-ttu-id="355c0-152">Erstellen, Lesen, Aktualisieren und Löschen</span><span class="sxs-lookup"><span data-stu-id="355c0-152">Create, read, update & delete</span></span>

* <span data-ttu-id="355c0-153">Öffnen Sie die Datei *Program.cs*, und ersetzen Sie den Inhalt durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="355c0-153">Open *Program.cs* and replace the contents with the following code:</span></span>

  [!code-csharp[Main](../../../samples/core/GetStarted/Program.cs)]

## <a name="run-the-app"></a><span data-ttu-id="355c0-154">Ausführen der App</span><span class="sxs-lookup"><span data-stu-id="355c0-154">Run the app</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="355c0-155">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="355c0-155">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet run
```

### <a name="visual-studio"></a>[<span data-ttu-id="355c0-156">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="355c0-156">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="355c0-157">Visual Studio verwendet beim Ausführen von .NET Core-Konsolen-Apps ein inkonsistentes Arbeitsverzeichnis.</span><span class="sxs-lookup"><span data-stu-id="355c0-157">Visual Studio uses an inconsistent working directory when running .NET Core console apps.</span></span> <span data-ttu-id="355c0-158">(Siehe [dotnet/project-system#3619](https://github.com/dotnet/project-system/issues/3619).) Dies führt dazu, dass eine Ausnahme ausgelöst wird: *no such table: Blogs* (Tabelle nicht vorhanden: Blogs).</span><span class="sxs-lookup"><span data-stu-id="355c0-158">(see [dotnet/project-system#3619](https://github.com/dotnet/project-system/issues/3619)) This results in an exception being thrown: *no such table: Blogs*.</span></span> <span data-ttu-id="355c0-159">So aktualisieren Sie das Arbeitsverzeichnis:</span><span class="sxs-lookup"><span data-stu-id="355c0-159">To update the working directory:</span></span>

* <span data-ttu-id="355c0-160">Klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie **Projektdatei bearbeiten** aus.</span><span class="sxs-lookup"><span data-stu-id="355c0-160">Right-click on the project and select **Edit Project File**</span></span>
* <span data-ttu-id="355c0-161">Fügen Sie direkt unterhalb der Eigenschaft *TargetFramework* Folgendes hinzu:</span><span class="sxs-lookup"><span data-stu-id="355c0-161">Just below the *TargetFramework* property, add the following:</span></span>

  ``` XML
  <StartWorkingDirectory>$(MSBuildProjectDirectory)</StartWorkingDirectory>
  ```

* <span data-ttu-id="355c0-162">Speichern Sie die Datei.</span><span class="sxs-lookup"><span data-stu-id="355c0-162">Save the file</span></span>

<span data-ttu-id="355c0-163">Nun können Sie die App ausführen:</span><span class="sxs-lookup"><span data-stu-id="355c0-163">Now you can run the app:</span></span>

* <span data-ttu-id="355c0-164">Klicken Sie auf **Debuggen > Ohne Debuggen starten**.</span><span class="sxs-lookup"><span data-stu-id="355c0-164">**Debug > Start Without Debugging**</span></span>

---

## <a name="next-steps"></a><span data-ttu-id="355c0-165">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="355c0-165">Next steps</span></span>

* <span data-ttu-id="355c0-166">Befolgen Sie das [ASP.NET Core-Tutorial](/aspnet/core/data/ef-rp/intro), um EF Core in einer Web-App zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="355c0-166">Follow the [ASP.NET Core Tutorial](/aspnet/core/data/ef-rp/intro) to use EF Core in a web app</span></span>
* <span data-ttu-id="355c0-167">Weitere Informationen zu [LINQ-Abfrageausdrücken](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations)</span><span class="sxs-lookup"><span data-stu-id="355c0-167">Learn more about [LINQ query expressions](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations)</span></span>
* <span data-ttu-id="355c0-168">[Konfigurieren Sie das Modell](xref:core/modeling/index), um Aspekte wie [required](xref:core/modeling/entity-properties#required-and-optional-properties) (erforderlich) und [maximum length](xref:core/modeling/entity-properties#maximum-length) (maximale Länge) anzugeben.</span><span class="sxs-lookup"><span data-stu-id="355c0-168">[Configure your model](xref:core/modeling/index) to specify things like [required](xref:core/modeling/entity-properties#required-and-optional-properties) and [maximum length](xref:core/modeling/entity-properties#maximum-length)</span></span>
* <span data-ttu-id="355c0-169">Verwenden Sie [Migrationen](xref:core/managing-schemas/migrations/index) zum Aktualisieren des Datenbankschemas nach dem Ändern des Modells.</span><span class="sxs-lookup"><span data-stu-id="355c0-169">Use [Migrations](xref:core/managing-schemas/migrations/index) to update the database schema after changing your model</span></span>
