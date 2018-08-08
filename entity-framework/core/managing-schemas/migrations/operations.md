---
title: Benutzerdefinierte Migrationsoperationen – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/7/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 510d585534b4809179c905ee5b77cab4209a2b8f
ms.sourcegitcommit: 902257be9c63c427dc793750a2b827d6feb8e38c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/07/2018
ms.locfileid: "39614284"
---
<a name="custom-migrations-operations"></a><span data-ttu-id="aefbb-102">Benutzerdefinierte Migrationsoperationen</span><span class="sxs-lookup"><span data-stu-id="aefbb-102">Custom Migrations Operations</span></span>
============================
<span data-ttu-id="aefbb-103">Die MigrationBuilder-API können Sie viele verschiedene Arten von Vorgängen bei der Migration ausführen, aber er ist längst nicht vollständig.</span><span class="sxs-lookup"><span data-stu-id="aefbb-103">The MigrationBuilder API allows you to perform many different kinds of operations during a migration, but it's far from exhaustive.</span></span> <span data-ttu-id="aefbb-104">Die API ist jedoch auch erweiterbar und ermöglichen Ihnen, Ihre eigenen Vorgänge zu definieren.</span><span class="sxs-lookup"><span data-stu-id="aefbb-104">However, the API is also extensible allowing you to define your own operations.</span></span> <span data-ttu-id="aefbb-105">Es gibt zwei Möglichkeiten, um die API zu erweitern: Verwenden der `Sql()` -Methode, oder durch Definieren von benutzerdefinierten `MigrationOperation` Objekte.</span><span class="sxs-lookup"><span data-stu-id="aefbb-105">There are two ways to extend the API: Using the `Sql()` method, or by defining custom `MigrationOperation` objects.</span></span>

<span data-ttu-id="aefbb-106">Um zu veranschaulichen, betrachten wir einen Vorgang, der erstellt einen Datenbankbenutzer, der mit jeder Ansatz, zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="aefbb-106">To illustrate, let's look at implementing an operation that creates a database user using each approach.</span></span> <span data-ttu-id="aefbb-107">In unserem Migrationen möchten wir aktivieren Sie den folgenden Code zu schreiben:</span><span class="sxs-lookup"><span data-stu-id="aefbb-107">In our migrations, we want to enable writing the following code:</span></span>

``` csharp
migrationBuilder.CreateUser("SQLUser1", "Password");
```

<a name="using-migrationbuildersql"></a><span data-ttu-id="aefbb-108">Verwenden von MigrationBuilder.Sql()</span><span class="sxs-lookup"><span data-stu-id="aefbb-108">Using MigrationBuilder.Sql()</span></span>
----------------------------
<span data-ttu-id="aefbb-109">Die einfachste Möglichkeit zum Implementieren eines benutzerdefinierten Vorgangs ist eine Erweiterungsmethode zu definieren, die aufruft `MigrationBuilder.Sql()`.</span><span class="sxs-lookup"><span data-stu-id="aefbb-109">The easiest way to implement a custom operation is to define an extension method that calls `MigrationBuilder.Sql()`.</span></span>
<span data-ttu-id="aefbb-110">Hier ist ein Beispiel, das die entsprechenden Transact-SQL generiert.</span><span class="sxs-lookup"><span data-stu-id="aefbb-110">Here is an example that generates the appropriate Transact-SQL.</span></span>

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
    => migrationBuilder.Sql($"CREATE USER {name} WITH PASSWORD '{password}';");
```

<span data-ttu-id="aefbb-111">Wenn Sie Ihre Migrationen mehrere Datenbankanbieter unterstützen müssen, können Sie mithilfe der `MigrationBuilder.ActiveProvider` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="aefbb-111">If your migrations need to support multiple database providers, you can use the `MigrationBuilder.ActiveProvider` property.</span></span> <span data-ttu-id="aefbb-112">Hier ist ein Beispiel für Unterstützung von Microsoft SQL Server und PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="aefbb-112">Here's an example supporting both Microsoft SQL Server and PostgreSQL.</span></span>

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
{
    switch (migrationBuilder.ActiveProvider)
    {
        case "Npgsql.EntityFrameworkCore.PostgreSQL":
            return migrationBuilder
                .Sql($"CREATE USER {name} WITH PASSWORD '{password}';");

        case "Microsoft.EntityFrameworkCore.SqlServer":
            return migrationBuilder
                .Sql($"CREATE USER {name} WITH PASSWORD = '{password}';");
    }

    return migrationBuilder;
}
```

<span data-ttu-id="aefbb-113">Dieser Ansatz funktioniert nur, wenn Sie wissen, dass jeder Anbieter, wo der benutzerdefinierte Vorgang angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="aefbb-113">This approach only works if you know every provider where your custom operation will be applied.</span></span>

<a name="using-a-migrationoperation"></a><span data-ttu-id="aefbb-114">Verwenden eine MigrationOperation</span><span class="sxs-lookup"><span data-stu-id="aefbb-114">Using a MigrationOperation</span></span>
---------------------------
<span data-ttu-id="aefbb-115">Um der benutzerdefinierte Vorgang aus dem SQL zu entkoppeln, Sie können Ihren eigenen definieren `MigrationOperation` für die Darstellung.</span><span class="sxs-lookup"><span data-stu-id="aefbb-115">To decouple the custom operation from the SQL, you can define your own `MigrationOperation` to represent it.</span></span> <span data-ttu-id="aefbb-116">Der Vorgang wird dann an den Anbieter übergeben, damit sie die entsprechende SQL generieren bestimmen kann.</span><span class="sxs-lookup"><span data-stu-id="aefbb-116">The operation is then passed to the provider so it can determine the appropriate SQL to generate.</span></span>

``` csharp
class CreateUserOperation : MigrationOperation
{
    public string Name { get; set; }
    public string Password { get; set; }
}
```

<span data-ttu-id="aefbb-117">Bei diesem Ansatz die Erweiterungsmethode muss lediglich eine von diesen Vorgängen hinzufügen `MigrationBuilder.Operations`.</span><span class="sxs-lookup"><span data-stu-id="aefbb-117">With this approach, the extension method just needs to add one of these operations to `MigrationBuilder.Operations`.</span></span>

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
{
    migrationBuilder.Operations.Add(
        new CreateUserOperation
        {
            Name = name,
            Password = password
        });

    return migrationBuilder;
}
```

<span data-ttu-id="aefbb-118">Dieser Ansatz erfordert jeder Anbieter wissen, wie Sie für diesen Vorgang im SQL generieren die `IMigrationsSqlGenerator` Service.</span><span class="sxs-lookup"><span data-stu-id="aefbb-118">This approach requires each provider to know how to generate SQL for this operation in their `IMigrationsSqlGenerator` service.</span></span> <span data-ttu-id="aefbb-119">Hier ist ein Beispiel, das Überschreiben der SQL Server Generator aus, um den neuen Vorgang zu verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="aefbb-119">Here is an example overriding the SQL Server's generator to handle the new operation.</span></span>

``` csharp
class MyMigrationsSqlGenerator : SqlServerMigrationsSqlGenerator
{
    public MyMigrationsSqlGenerator(
        MigrationsSqlGeneratorDependencies dependencies,
        IMigrationsAnnotationProvider migrationsAnnotations)
        : base(dependencies, migrationsAnnotations)
    {
    }

    protected override void Generate(
        MigrationOperation operation,
        IModel model,
        MigrationCommandListBuilder builder)
    {
        if (operation is CreateUserOperation createUserOperation)
        {
            Generate(createUserOperation, builder);
        }
        else
        {
            base.Generate(operation, model, builder);
        }
    }

    private void Generate(
        CreateUserOperation operation,
        MigrationCommandListBuilder builder)
    {
        var sqlHelper = Dependencies.SqlGenerationHelper;
        var stringMapping = Dependencies.TypeMapper.GetMapping(typeof(string));

        builder
            .Append("CREATE USER ")
            .Append(sqlHelper.DelimitIdentifier(name))
            .Append(" WITH PASSWORD = ")
            .Append(stringMapping.GenerateSqlLiteral(password))
            .AppendLine(sqlHelper.StatementTerminator)
            .EndCommand();
    }
}
```

<span data-ttu-id="aefbb-120">Ersetzen Sie den Standarddienst Migrationen Sql-Generator, mit dem aktualisierten.</span><span class="sxs-lookup"><span data-stu-id="aefbb-120">Replace the default migrations sql generator service with the updated one.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IMigrationsSqlGenerator, MyMigrationsSqlGenerator>();
```
