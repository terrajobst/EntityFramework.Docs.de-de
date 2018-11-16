---
title: Reverse-Engineering - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/13/2018
ms.assetid: 6263EF7D-4989-42E6-BDEE-45DA770342FB
uid: core/managing-schemas/scaffolding
ms.openlocfilehash: ef729c0c26d5a1f57099f339eb51cda7e83289df
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688679"
---
# <a name="reverse-engineering"></a><span data-ttu-id="9c08e-102">Reverse-Engineering</span><span class="sxs-lookup"><span data-stu-id="9c08e-102">Reverse Engineering</span></span>

<span data-ttu-id="9c08e-103">Reverse Engineering ist der Prozess der Gerüstbau, Entity-Typ-Klassen und eine DbContext-Klasse, die basierend auf einem Datenbankschema.</span><span class="sxs-lookup"><span data-stu-id="9c08e-103">Reverse engineering is the process of scaffolding entity type classes and a DbContext class based on a database schema.</span></span> <span data-ttu-id="9c08e-104">Sie können mithilfe von ausgeführt werden die `Scaffold-DbContext` Befehl der Paket-Manager-Konsole (PMC) von EF Core-Tools oder `dotnet ef dbcontext scaffold` -Befehl von .NET Command-Line Interface (CLI)-Tools.</span><span class="sxs-lookup"><span data-stu-id="9c08e-104">It can be performed using the `Scaffold-DbContext` command of the EF Core Package Manager Console (PMC) tools or the `dotnet ef dbcontext scaffold` command of the .NET Command-line Interface (CLI) tools.</span></span>

## <a name="installing"></a><span data-ttu-id="9c08e-105">Installation</span><span class="sxs-lookup"><span data-stu-id="9c08e-105">Installing</span></span>

<span data-ttu-id="9c08e-106">Vor reverse Engineering, müssen Sie zum Installieren der [PMC-Tools](xref:core/miscellaneous/cli/powershell) (nur Visual Studio) oder die [CLI-Tools](xref:core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="9c08e-106">Before reverse engineering, you'll need to install either the [PMC tools](xref:core/miscellaneous/cli/powershell) (Visual Studio only) or the [CLI tools](xref:core/miscellaneous/cli/dotnet).</span></span> <span data-ttu-id="9c08e-107">Finden Sie Links Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="9c08e-107">See links for details.</span></span>

<span data-ttu-id="9c08e-108">Sie müssen auch eine entsprechende installieren [Datenbankanbieter](xref:core/providers/index) für das Datenbankschema für das reverse Engineering werden sollen.</span><span class="sxs-lookup"><span data-stu-id="9c08e-108">You'll also need to install an appropriate [database provider](xref:core/providers/index) for the database schema you want to reverse engineer.</span></span>

## <a name="connection-string"></a><span data-ttu-id="9c08e-109">Verbindungszeichenfolge</span><span class="sxs-lookup"><span data-stu-id="9c08e-109">Connection string</span></span>

<span data-ttu-id="9c08e-110">Das erste Argument für den Befehl wird eine Verbindungszeichenfolge für die Datenbank.</span><span class="sxs-lookup"><span data-stu-id="9c08e-110">The first argument to the command is a connection string to the database.</span></span> <span data-ttu-id="9c08e-111">Die Tools werden diese Verbindungszeichenfolge verwenden, um das Datenbankschema zu lesen.</span><span class="sxs-lookup"><span data-stu-id="9c08e-111">The tools will use this connection string to read the database schema.</span></span>

<span data-ttu-id="9c08e-112">Wie Sie zitieren und Escapezeichen die Verbindungszeichenfolge hängt ab, in der Shell Sie zum Ausführen des Befehls verwenden.</span><span class="sxs-lookup"><span data-stu-id="9c08e-112">How you quote and escape the connection string depends on which shell you are using to execute the command.</span></span> <span data-ttu-id="9c08e-113">Finden Sie Einzelheiten in Ihrer Shell.</span><span class="sxs-lookup"><span data-stu-id="9c08e-113">Refer to your shell's documentation for specifics.</span></span> <span data-ttu-id="9c08e-114">Z. B. PowerShell müssen Sie mit Escapezeichen versehen die `$` Zeichen, aber nicht `\`.</span><span class="sxs-lookup"><span data-stu-id="9c08e-114">For example, PowerShell requires you to escape the `$` character, but not `\`.</span></span>

``` powershell
Scaffold-DbContext 'Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook' Microsoft.EntityFrameworkCore.SqlServer
```

``` Console
dotnet ef dbcontext scaffold "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook" Microsoft.EntityFrameworkCore.SqlServer
```

### <a name="configuration-and-user-secrets"></a><span data-ttu-id="9c08e-115">Konfiguration und vertrauliche Informationen eines Benutzers</span><span class="sxs-lookup"><span data-stu-id="9c08e-115">Configuration and User Secrets</span></span>

<span data-ttu-id="9c08e-116">Wenn Sie eine ASP.NET Core-Projekt verfügen, können Sie die `Name=<connection-string>` Syntax, um die Verbindungszeichenfolge aus der Konfiguration gelesen.</span><span class="sxs-lookup"><span data-stu-id="9c08e-116">If you have an ASP.NET Core project, you can use the `Name=<connection-string>` syntax to read the connection string from configuration.</span></span>

<span data-ttu-id="9c08e-117">Dies funktioniert gut mit den [Secret Manager-Tool](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager) um Ihr Datenbankkennwort von Ihrer Codebasis zu trennen.</span><span class="sxs-lookup"><span data-stu-id="9c08e-117">This works well with the [Secret Manager tool](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager) to keep your database password separate from your codebase.</span></span>

``` Console
dotnet user-secrets set ConnectionStrings.Chinook "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook"
dotnet ef dbcontext scaffold Name=Chinook Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="provider-name"></a><span data-ttu-id="9c08e-118">Name des Anbieters</span><span class="sxs-lookup"><span data-stu-id="9c08e-118">Provider name</span></span>

<span data-ttu-id="9c08e-119">Das zweite Argument ist der Name des Anbieters.</span><span class="sxs-lookup"><span data-stu-id="9c08e-119">The second argument is the provider name.</span></span> <span data-ttu-id="9c08e-120">Der Name des Anbieters entspricht in der Regel der Name des Anbieters NuGet-Paket.</span><span class="sxs-lookup"><span data-stu-id="9c08e-120">The provider name is typically the same as the provider's NuGet package name.</span></span>

## <a name="specifying-tables"></a><span data-ttu-id="9c08e-121">Festlegen von Tabellen</span><span class="sxs-lookup"><span data-stu-id="9c08e-121">Specifying tables</span></span>

<span data-ttu-id="9c08e-122">Alle Tabellen im Datenbankschema sind reverse Engineering in Entitätstypen in der Standardeinstellung.</span><span class="sxs-lookup"><span data-stu-id="9c08e-122">All tables in the database schema are reverse engineered into entity types by default.</span></span> <span data-ttu-id="9c08e-123">Sie können einschränken, welche Tabellen reverse sind so konzipiert, indem Sie Schemas und Tabellen angeben.</span><span class="sxs-lookup"><span data-stu-id="9c08e-123">You can limit which tables are reverse engineered by specifying schemas and tables.</span></span>

<span data-ttu-id="9c08e-124">Die `-Schemas` Parameter in PMC und `--schema` Option in der CLI kann verwendet werden, um jede Tabelle in einem Schema enthalten.</span><span class="sxs-lookup"><span data-stu-id="9c08e-124">The `-Schemas` parameter in PMC and the `--schema` option in the CLI can be used to include every table within a schema.</span></span>

<span data-ttu-id="9c08e-125">`-Tables` (PMC) und `--table` (CLI) kann verwendet werden, können Sie bestimmte Tabellen einbeziehen.</span><span class="sxs-lookup"><span data-stu-id="9c08e-125">`-Tables` (PMC) and `--table` (CLI) can be used to include specific tables.</span></span>

<span data-ttu-id="9c08e-126">Um mehrere Tabellen in der PMC einzuschließen, verwenden Sie ein Array.</span><span class="sxs-lookup"><span data-stu-id="9c08e-126">To include multiple tables in PMC, use an array.</span></span>

``` powershell
Scaffold-DbContext ... -Tables Artist, Album
```

<span data-ttu-id="9c08e-127">Um mehrere Tabellen in der CLI einzuschließen, geben Sie die Option mehrfach.</span><span class="sxs-lookup"><span data-stu-id="9c08e-127">To include multiple tables in the CLI, specify the option multiple times.</span></span>

``` Console
dotnet ef dbcontext scaffold ... --table Artist --table Album
```

## <a name="preserving-names"></a><span data-ttu-id="9c08e-128">Beibehalten von Namen</span><span class="sxs-lookup"><span data-stu-id="9c08e-128">Preserving names</span></span>

<span data-ttu-id="9c08e-129">Tabellen- und Spaltennamen werden repariert, die .NET-Benennungskonventionen für Typen und Eigenschaften besser zu entsprechen standardmäßig.</span><span class="sxs-lookup"><span data-stu-id="9c08e-129">Table and column names are fixed up to better match the .NET naming conventions for types and properties by default.</span></span> <span data-ttu-id="9c08e-130">Angeben der `-UseDatabaseNames` in PMC wechseln oder die `--use-database-names` Option in der CLI wird dieses Verhalten, das die ursprünglichen Datenbanknamen so weit wie möglich beibehalten deaktiviert.</span><span class="sxs-lookup"><span data-stu-id="9c08e-130">Specifying the `-UseDatabaseNames` switch in PMC or the `--use-database-names` option in the CLI will disable this behavior preserving the original database names as much as possible.</span></span> <span data-ttu-id="9c08e-131">Ungültige .NET Bezeichner werden immer noch behoben werden, und synthetische Namen wie z. B. Navigationseigenschaften werden weiterhin Namenskonventionen von .NET entsprechen.</span><span class="sxs-lookup"><span data-stu-id="9c08e-131">Invalid .NET identifiers will still be fixed and synthesized names like navigation properties will still conform to .NET naming conventions.</span></span>

## <a name="fluent-api-or-data-annotations"></a><span data-ttu-id="9c08e-132">Fluent-API oder Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="9c08e-132">Fluent API or Data Annotations</span></span>

<span data-ttu-id="9c08e-133">Entitätstypen mithilfe der Fluent-API wird standardmäßig konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="9c08e-133">Entity types are configured using the Fluent API by default.</span></span> <span data-ttu-id="9c08e-134">Geben Sie `-DataAnnotations` (PMC) oder `--data-annotations` (CLI) zu verwenden. von datenanmerkungen, sofern möglich.</span><span class="sxs-lookup"><span data-stu-id="9c08e-134">Specify `-DataAnnotations` (PMC) or `--data-annotations` (CLI) to instead use data annotations when possible.</span></span>

<span data-ttu-id="9c08e-135">Beispielsweise wird mit der Fluent-API dieses Gerüst erstellen.</span><span class="sxs-lookup"><span data-stu-id="9c08e-135">For example, using the Fluent API will scaffold the this.</span></span>

``` csharp
entity.Property(e => e.Title)
    .IsRequired()
    .HasMaxLength(160);
```

<span data-ttu-id="9c08e-136">Bei der Verwendung von Datenanmerkungen wird dies Erstellen des Gerüsts.</span><span class="sxs-lookup"><span data-stu-id="9c08e-136">While using Data Annotations will scaffold this.</span></span>

``` csharp
[Required]
[StringLength(160)]
public string Title { get; set; }
```

## <a name="dbcontext-name"></a><span data-ttu-id="9c08e-137">"DbContext"-name</span><span class="sxs-lookup"><span data-stu-id="9c08e-137">DbContext name</span></span>

<span data-ttu-id="9c08e-138">Die erstellte "DbContext"-Klasse wird als Name der Name der Datenbank mit dem Suffix *Kontext* standardmäßig.</span><span class="sxs-lookup"><span data-stu-id="9c08e-138">The scaffolded DbContext class name will be the name of the database suffixed with *Context* by default.</span></span> <span data-ttu-id="9c08e-139">Um einen anderen Wert anzugeben, verwenden `-Context` in PMC und `--context` in der CLI.</span><span class="sxs-lookup"><span data-stu-id="9c08e-139">To specify a different one, use `-Context` in PMC and `--context` in the CLI.</span></span>

## <a name="directories-and-namespaces"></a><span data-ttu-id="9c08e-140">Verzeichnisse und namespaces</span><span class="sxs-lookup"><span data-stu-id="9c08e-140">Directories and namespaces</span></span>

<span data-ttu-id="9c08e-141">Die Entitätsklassen und eine DbContext-Klasse sind, die in das Stammverzeichnis des Projekts erstellt haben, und Verwenden von Standardnamespace des Projekts aus.</span><span class="sxs-lookup"><span data-stu-id="9c08e-141">The entity classes and a DbContext class are scaffolded into the project's root directory and use the project's default namespace.</span></span> <span data-ttu-id="9c08e-142">Sie können das Verzeichnis angeben, das Gerüst für Klassen erstellt werden, mithilfe von `-OutputDir` (PMC) oder `--output-dir` (CLI).</span><span class="sxs-lookup"><span data-stu-id="9c08e-142">You can specify the directory where classes are scaffolded using `-OutputDir` (PMC) or `--output-dir` (CLI).</span></span> <span data-ttu-id="9c08e-143">Der Namespace werden den Stamm-Namespace sowie die Namen der keine Unterverzeichnisse unter dem Stammverzeichnis des Projekts.</span><span class="sxs-lookup"><span data-stu-id="9c08e-143">The namespace will be the root namespace plus the names of any subdirectories under the project's root directory.</span></span>

<span data-ttu-id="9c08e-144">Sie können auch `-ContextDir` (PMC) und `--context-dir` (CLI) zum Erstellen des Gerüsts für der DbContext-Klasse in einem separaten Verzeichnis über die entitätstypklassen.</span><span class="sxs-lookup"><span data-stu-id="9c08e-144">You can also use `-ContextDir` (PMC) and `--context-dir` (CLI) to scaffold the DbContext class into a separate directory from the entity type classes.</span></span>

``` powershell
Scaffold-DbContext ... -ContextDir Data -OutputDir Models
```

``` Console
dotnet ef dbcontext scaffold ... --context-dir Data --output-dir Models
```

## <a name="how-it-works"></a><span data-ttu-id="9c08e-145">So funktioniert es</span><span class="sxs-lookup"><span data-stu-id="9c08e-145">How it works</span></span>

<span data-ttu-id="9c08e-146">Reverse Engineering beginnt mit der das Schema der Datenbank lesen.</span><span class="sxs-lookup"><span data-stu-id="9c08e-146">Reverse engineering starts by reading the database schema.</span></span> <span data-ttu-id="9c08e-147">Sie liest die Informationen zu Tabellen, Spalten, Einschränkungen und Indizes.</span><span class="sxs-lookup"><span data-stu-id="9c08e-147">It reads information about tables, columns, constraints, and indexes.</span></span>

<span data-ttu-id="9c08e-148">Als Nächstes verwendet er die Schemainformationen zum Erstellen eines EF Core-Modells.</span><span class="sxs-lookup"><span data-stu-id="9c08e-148">Next, it uses the schema information to create an EF Core model.</span></span> <span data-ttu-id="9c08e-149">Tabellen werden verwendet, um Entitätstypen zu erstellen. Spalten werden verwendet, um Eigenschaften zu erstellen. und Fremdschlüssel werden verwendet, um Beziehungen zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="9c08e-149">Tables are used to create entity types; columns are used to create properties; and foreign keys are used to create relationships.</span></span>

<span data-ttu-id="9c08e-150">Schließlich wird das Modell verwendet, um Code zu generieren.</span><span class="sxs-lookup"><span data-stu-id="9c08e-150">Finally, the model is used to generate code.</span></span> <span data-ttu-id="9c08e-151">Die entsprechenden Entität Klassen, Fluent-API und Daten typanmerkungen sind erstellt haben, um das gleiche Modell aus Ihrer app neu zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="9c08e-151">The corresponding entity type classes, Fluent API, and data annotations are scaffolded in order to re-create the same model from your app.</span></span>

## <a name="what-doesnt-work"></a><span data-ttu-id="9c08e-152">Was nicht funktioniert</span><span class="sxs-lookup"><span data-stu-id="9c08e-152">What doesn't work</span></span>

<span data-ttu-id="9c08e-153">Nicht alles, was zu einem Modell kann mit einem Datenbankschema dargestellt werden.</span><span class="sxs-lookup"><span data-stu-id="9c08e-153">Not everything about a model can be represented using a database schema.</span></span> <span data-ttu-id="9c08e-154">Z. B. Informationen zu **Vererbungshierarchien**, **eigene Typen**, und **Tabelle aufteilen** nicht in das Schema der Datenbank vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="9c08e-154">For example, information about **inheritance hierarchies**, **owned types**, and **table splitting** are not present in the database schema.</span></span> <span data-ttu-id="9c08e-155">Aus diesem Grund wird diese Konstrukte nie zurückentwickelt werden.</span><span class="sxs-lookup"><span data-stu-id="9c08e-155">Because of this, these constructs will never be reverse engineered.</span></span>

<span data-ttu-id="9c08e-156">Darüber hinaus **Einige Spaltentypen** möglicherweise nicht vom Entity Framework Core-Anbieter unterstützt werden.</span><span class="sxs-lookup"><span data-stu-id="9c08e-156">In addition, **some column types** may not be supported by the EF Core provider.</span></span> <span data-ttu-id="9c08e-157">Diese Spalten nicht im Modell berücksichtigt.</span><span class="sxs-lookup"><span data-stu-id="9c08e-157">These columns won't be included in the model.</span></span>

<span data-ttu-id="9c08e-158">EF Core erfordert jeder Entitätstyp, der einen Schlüssel verfügen.</span><span class="sxs-lookup"><span data-stu-id="9c08e-158">EF Core requires every entity type to have a key.</span></span> <span data-ttu-id="9c08e-159">Tabellen werden nicht allerdings erforderlich, um einen primären Schlüssel anzugeben.</span><span class="sxs-lookup"><span data-stu-id="9c08e-159">Tables, however, aren't required to specify a primary key.</span></span> <span data-ttu-id="9c08e-160">**Tabellen ohne Primärschlüssel** sind derzeit nicht mit Reverse Engineering.</span><span class="sxs-lookup"><span data-stu-id="9c08e-160">**Tables without a primary key** are currently not reverse engineered.</span></span>

<span data-ttu-id="9c08e-161">Sie können definieren, **parallelitätstoken** in ein EF Core-Modell, um zu verhindern, dass zwei Benutzer die gleiche Entität zur selben Zeit aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="9c08e-161">You can define **concurrency tokens** in an EF Core model to prevent two users from updating the same entity at the same time.</span></span> <span data-ttu-id="9c08e-162">Einige Datenbanken haben eine besondere Art dieser Art von Spalte (z. B. "Rowversion" in SQL Server) darstellen, in diesem Fall kehren wir können diese Informationen zu entwickeln. Allerdings wird andere parallelitätstoken nicht zurückentwickelt werden.</span><span class="sxs-lookup"><span data-stu-id="9c08e-162">Some databases have a special type to represent this type of column (for example, rowversion in SQL Server) in which case we can reverse engineer this information; however, other concurrency tokens will not be reverse engineered.</span></span>

## <a name="customizing-the-model"></a><span data-ttu-id="9c08e-163">Anpassen des Modells</span><span class="sxs-lookup"><span data-stu-id="9c08e-163">Customizing the model</span></span>

<span data-ttu-id="9c08e-164">Der von EF Core generierte Code ist der Code.</span><span class="sxs-lookup"><span data-stu-id="9c08e-164">The code generated by EF Core is your code.</span></span> <span data-ttu-id="9c08e-165">Sie ändern können.</span><span class="sxs-lookup"><span data-stu-id="9c08e-165">Feel free to change it.</span></span> <span data-ttu-id="9c08e-166">Es wird nur neu generiert werden, wenn Sie Engineering desselben Modells erneut Reverse.</span><span class="sxs-lookup"><span data-stu-id="9c08e-166">It will only be regenerated if you reverse engineer the same model again.</span></span> <span data-ttu-id="9c08e-167">Der eingerüstete Code stellt *eine* Modell, das verwendet werden kann, auf die Datenbank, aber es ist sicherlich nicht der *nur* Modell, das verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="9c08e-167">The scaffolded code represents *one* model that can be used to access the database, but it's certainly not the *only* model that can be used.</span></span>

<span data-ttu-id="9c08e-168">Passen Sie die entitätstypklassen und DbContext-Klasse, die Ihren Bedürfnissen an.</span><span class="sxs-lookup"><span data-stu-id="9c08e-168">Customize the entity type classes and DbContext class to fit your needs.</span></span> <span data-ttu-id="9c08e-169">Sie könnten z. B. das Umbenennen von Typen und Eigenschaften, Vererbungshierarchien einführen oder Teilen einer Tabelle in mehreren Entitäten.</span><span class="sxs-lookup"><span data-stu-id="9c08e-169">For example, you may choose to rename types and properties, introduce inheritance hierarchies, or split a table into to multiple entities.</span></span> <span data-ttu-id="9c08e-170">Sie können auch nicht eindeutige Indizes, nicht verwendete Sequenzen und Navigationseigenschaften, optionale skalare Eigenschaften und Einschränkungsnamen, die aus dem Modell entfernen.</span><span class="sxs-lookup"><span data-stu-id="9c08e-170">You can also remove non-unique indexes, unused sequences and navigation properties, optional scalar properties, and constraint names from the model.</span></span>

<span data-ttu-id="9c08e-171">Sie können auch zusätzliche Konstruktoren, Methoden, Eigenschaften usw., hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="9c08e-171">You can also add additional constructors, methods, properties, etc.</span></span> <span data-ttu-id="9c08e-172">verwenden eine andere partielle Klasse in einer separaten Datei an.</span><span class="sxs-lookup"><span data-stu-id="9c08e-172">using another partial class in a separate file.</span></span> <span data-ttu-id="9c08e-173">Dieser Ansatz funktioniert auch, wenn Sie beabsichtigen, reverse-Engineering-das Modell erneut aus.</span><span class="sxs-lookup"><span data-stu-id="9c08e-173">This approach works even when you intend to reverse engineer the model again.</span></span>

## <a name="updating-the-model"></a><span data-ttu-id="9c08e-174">Aktualisieren des Modells</span><span class="sxs-lookup"><span data-stu-id="9c08e-174">Updating the model</span></span>

<span data-ttu-id="9c08e-175">Nach dem vornehmen von Änderungen in der Datenbank, müssen Sie das EF Core-Modell entsprechend aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="9c08e-175">After making changes to the database, you may need to update your EF Core model to reflect those changes.</span></span> <span data-ttu-id="9c08e-176">Wenn die Änderungen in der Datenbank auf einfach festgelegt sind, kann es am einfachsten, nur um die Änderungen an Ihrem EF Core-Modell manuell zu vornehmen sein.</span><span class="sxs-lookup"><span data-stu-id="9c08e-176">If the database changes are simple, it may be easiest just to manually make the changes to your EF Core model.</span></span> <span data-ttu-id="9c08e-177">Beispielsweise werden durch das Umbenennen einer Tabelle oder Spalte, eine Spalte zu entfernen oder Aktualisieren einer Spalte des Typs banale Änderungen im Code vornehmen.</span><span class="sxs-lookup"><span data-stu-id="9c08e-177">For example, renaming a table or column, removing a column, or updating a column's type are trivial changes to make in code.</span></span>

<span data-ttu-id="9c08e-178">Bei wichtigeren Änderungen sind jedoch nicht einfach stellen manuell.</span><span class="sxs-lookup"><span data-stu-id="9c08e-178">More significant changes, however, are not as easy make manually.</span></span> <span data-ttu-id="9c08e-179">Ein gängiger Workflow ist Reverse Engineering für das Modell aus der Datenbank erneut mit `-Force` (PMC) oder `--force` (CLI), um das vorhandene Modell durch eine aktualisierte zu überschreiben.</span><span class="sxs-lookup"><span data-stu-id="9c08e-179">One common workflow is to reverse engineer the model from the database again using `-Force` (PMC) or `--force` (CLI) to overwrite the existing model with an updated one.</span></span>

<span data-ttu-id="9c08e-180">Ein weiteres häufig angeforderte Feature ist die Möglichkeit, das Modell aus der Datenbank zu aktualisieren, Beibehaltung von Anpassungen wie umbenennungen, Typhierarchien usw. an. Verwenden von Problem [#831](https://github.com/aspnet/EntityFrameworkCore/issues/831) zum Nachverfolgen des Status der dieses Feature.</span><span class="sxs-lookup"><span data-stu-id="9c08e-180">Another commonly requested feature is the ability to update the model from the database while preserving customization like renames, type hierarchies, etc. Use issue [#831](https://github.com/aspnet/EntityFrameworkCore/issues/831) to track the progress of this feature.</span></span>

> [!WARNING]
> <span data-ttu-id="9c08e-181">Wenn reverse das Modell aus der Datenbank erneut Engineering verloren zu den Dateien vorgenommenen Änderungen.</span><span class="sxs-lookup"><span data-stu-id="9c08e-181">If you reverse engineer the model from the database again, any changes you've made to the files will be lost.</span></span>
