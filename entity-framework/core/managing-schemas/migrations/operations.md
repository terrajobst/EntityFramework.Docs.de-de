---
title: Benutzerdefinierte Migrationen Operations - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/7/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 84d80175e719c950844b13688e1a4992614f25d8
ms.sourcegitcommit: 038acd91ce2f5a28d76dcd2eab72eeba225e366d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/14/2018
---
<a name="custom-migrations-operations"></a><span data-ttu-id="26c31-102">Benutzerdefinierte Migrationen-Vorgänge</span><span class="sxs-lookup"><span data-stu-id="26c31-102">Custom Migrations Operations</span></span>
============================
<span data-ttu-id="26c31-103">Die MigrationBuilder-API ermöglicht Ihnen, viele verschiedene Arten von Vorgängen während einer Migration durchzuführen, aber zurückgelegt vollständig ist.</span><span class="sxs-lookup"><span data-stu-id="26c31-103">The MigrationBuilder API allows you to perform many different kinds of operations during a migration, but it's far from exhaustive.</span></span> <span data-ttu-id="26c31-104">Allerdings ist die API erweiterbar gestattet, eigene Vorgänge zu definieren.</span><span class="sxs-lookup"><span data-stu-id="26c31-104">However, the API is also extensible allowing you to define your own operations.</span></span> <span data-ttu-id="26c31-105">Es gibt zwei Methoden zum Erweitern von der API: Verwenden der `Sql()` -Methode, oder definieren benutzerdefinierte `MigrationOperation` Objekte.</span><span class="sxs-lookup"><span data-stu-id="26c31-105">There are two ways to extend the API: Using the `Sql()` method, or by defining custom `MigrationOperation` objects.</span></span>

<span data-ttu-id="26c31-106">Um zu veranschaulichen, sehen wir uns das Implementieren eines Vorgangs, das einen Datenbankbenutzer, der mit jeder Ansatz erstellt.</span><span class="sxs-lookup"><span data-stu-id="26c31-106">To illustrate, let's look at implementing an operation that creates a database user using each approach.</span></span> <span data-ttu-id="26c31-107">In unserem Migrationen möchten wir ermöglichen, schreiben den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="26c31-107">In our migrations, we want to enable writing the following code:</span></span>

``` csharp
migrationBuilder.CreateUser("SQLUser1", "Password");
```

<a name="using-migrationbuildersql"></a><span data-ttu-id="26c31-108">Verwenden von MigrationBuilder.Sql()</span><span class="sxs-lookup"><span data-stu-id="26c31-108">Using MigrationBuilder.Sql()</span></span>
----------------------------
<span data-ttu-id="26c31-109">Die einfachste Möglichkeit, einen Vorgang implementiert wird, um eine Erweiterungsmethode zu definieren, die aufruft `MigrationBuilder.Sql()`.</span><span class="sxs-lookup"><span data-stu-id="26c31-109">The easiest way to implement a custom operation is to define an extension method that calls `MigrationBuilder.Sql()`.</span></span>
<span data-ttu-id="26c31-110">Hier ist ein Beispiel, das die entsprechende Transact-SQL generiert.</span><span class="sxs-lookup"><span data-stu-id="26c31-110">Here is an example that generates the appropriate Transact-SQL.</span></span>

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
    => migrationBuilder.Sql($"CREATE USER {name} WITH PASSWORD '{password}';");
```

<span data-ttu-id="26c31-111">Wenn Ihre Migrationen mehrere Datenbankanbieter unterstützen müssen, können Sie mithilfe der `MigrationBuilder.ActiveProvider` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="26c31-111">If your migrations need to support multiple database providers, you can use the `MigrationBuilder.ActiveProvider` property.</span></span> <span data-ttu-id="26c31-112">Hier ist ein Beispiel für Microsoft SQL Server und PostgreSQL unterstützen.</span><span class="sxs-lookup"><span data-stu-id="26c31-112">Here's an example supporting both Microsoft SQL Server and PostgreSQL.</span></span>

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
}
```

<span data-ttu-id="26c31-113">Dieser Ansatz funktioniert nur, wenn Sie wissen, dass alle Anbieter, in denen Ihre benutzerdefinierte Vorgänge angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="26c31-113">This approach only works if you know every provider where your custom operation will be applied.</span></span>

<a name="using-a-migrationoperation"></a><span data-ttu-id="26c31-114">Verwenden eine MigrationOperation</span><span class="sxs-lookup"><span data-stu-id="26c31-114">Using a MigrationOperation</span></span>
---------------------------
<span data-ttu-id="26c31-115">Um den benutzerdefinierten Vorgang aus der SQL zu entkoppeln, können Sie definieren eine eigene `MigrationOperation` für die Darstellung.</span><span class="sxs-lookup"><span data-stu-id="26c31-115">To decouple the custom operation from the SQL, you can define your own `MigrationOperation` to represent it.</span></span> <span data-ttu-id="26c31-116">Der Vorgang wird dann an den Anbieter übergeben, sodass er die entsprechende SQL generieren ermitteln kann.</span><span class="sxs-lookup"><span data-stu-id="26c31-116">The operation is then passed to the provider so it can determine the appropriate SQL to generate.</span></span>

``` csharp
class CreateUserOperation : MigrationOperation
{
    public string Name { get; set; }
    public string Password { get; set; }
}
```

<span data-ttu-id="26c31-117">Bei diesem Ansatz die Erweiterungsmethode benötigt nur einen dieser Vorgänge zum Hinzufügen `MigrationBuilder.Operations`.</span><span class="sxs-lookup"><span data-stu-id="26c31-117">With this approach, the extension method just needs to add one of these operations to `MigrationBuilder.Operations`.</span></span>

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

<span data-ttu-id="26c31-118">Dieser Ansatz erfordert jeder Anbieter wissen, wie zum Generieren von SQL für diesen Vorgang in ihre `IMigrationsSqlGenerator` Dienst.</span><span class="sxs-lookup"><span data-stu-id="26c31-118">This approach requires each provider to know how to generate SQL for this operation in their `IMigrationsSqlGenerator` service.</span></span> <span data-ttu-id="26c31-119">Hier ist ein Beispiel für die SQL Server-Generator zum Verarbeiten der neuen Vorgangs überschreiben.</span><span class="sxs-lookup"><span data-stu-id="26c31-119">Here is an example overriding the SQL Server's generator to handle the new operation.</span></span>

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

<span data-ttu-id="26c31-120">Ersetzen Sie den Standarddienst Migrationen Sql-Generator, mit dem aktualisierten.</span><span class="sxs-lookup"><span data-stu-id="26c31-120">Replace the default migrations sql generator service with the updated one.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IMigrationsSqlGenerator, MyMigrationsSqlGenerator>();
```
