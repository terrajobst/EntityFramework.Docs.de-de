---
title: Benutzerdefinierte Migrations Vorgänge-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2017
uid: core/managing-schemas/migrations/operations
ms.openlocfilehash: bd2bfdc24977a47eaf7a6756a88b758b563d818a
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414326"
---
# <a name="custom-migrations-operations"></a><span data-ttu-id="2f148-102">Benutzerdefinierte Migrations Vorgänge</span><span class="sxs-lookup"><span data-stu-id="2f148-102">Custom Migrations Operations</span></span>

<span data-ttu-id="2f148-103">Die migrationbuilder-API ermöglicht es Ihnen, viele verschiedene Arten von Vorgängen während einer Migration auszuführen, aber es ist weit von der Vollständigkeit her.</span><span class="sxs-lookup"><span data-stu-id="2f148-103">The MigrationBuilder API allows you to perform many different kinds of operations during a migration, but it's far from exhaustive.</span></span> <span data-ttu-id="2f148-104">Die API ist jedoch auch erweiterbar, sodass Sie eigene Vorgänge definieren können.</span><span class="sxs-lookup"><span data-stu-id="2f148-104">However, the API is also extensible allowing you to define your own operations.</span></span> <span data-ttu-id="2f148-105">Es gibt zwei Möglichkeiten, die API zu erweitern: mithilfe der `Sql()`-Methode oder durch Definieren benutzerdefinierter `MigrationOperation` Objekte.</span><span class="sxs-lookup"><span data-stu-id="2f148-105">There are two ways to extend the API: Using the `Sql()` method, or by defining custom `MigrationOperation` objects.</span></span>

<span data-ttu-id="2f148-106">Um dies zu veranschaulichen, betrachten wir die Implementierung eines Vorgangs, der einen Datenbankbenutzer mit jedem Ansatz erstellt.</span><span class="sxs-lookup"><span data-stu-id="2f148-106">To illustrate, let's look at implementing an operation that creates a database user using each approach.</span></span> <span data-ttu-id="2f148-107">In unseren Migrationen möchten wir das Schreiben des folgenden Codes aktivieren:</span><span class="sxs-lookup"><span data-stu-id="2f148-107">In our migrations, we want to enable writing the following code:</span></span>

``` csharp
migrationBuilder.CreateUser("SQLUser1", "Password");
```

## <a name="using-migrationbuildersql"></a><span data-ttu-id="2f148-108">Verwenden von migrationbuilder. SQL ()</span><span class="sxs-lookup"><span data-stu-id="2f148-108">Using MigrationBuilder.Sql()</span></span>

<span data-ttu-id="2f148-109">Die einfachste Möglichkeit zum Implementieren eines benutzerdefinierten Vorgangs besteht darin, eine Erweiterungsmethode zu definieren, die `MigrationBuilder.Sql()`aufruft.</span><span class="sxs-lookup"><span data-stu-id="2f148-109">The easiest way to implement a custom operation is to define an extension method that calls `MigrationBuilder.Sql()`.</span></span> <span data-ttu-id="2f148-110">Es folgt ein Beispiel, das die entsprechende Transact-SQL-Anwendung generiert.</span><span class="sxs-lookup"><span data-stu-id="2f148-110">Here is an example that generates the appropriate Transact-SQL.</span></span>

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
    => migrationBuilder.Sql($"CREATE USER {name} WITH PASSWORD '{password}';");
```

<span data-ttu-id="2f148-111">Wenn Ihre Migrationen mehrere Datenbankanbieter unterstützen müssen, können Sie die `MigrationBuilder.ActiveProvider`-Eigenschaft verwenden.</span><span class="sxs-lookup"><span data-stu-id="2f148-111">If your migrations need to support multiple database providers, you can use the `MigrationBuilder.ActiveProvider` property.</span></span> <span data-ttu-id="2f148-112">Hier sehen Sie ein Beispiel, das sowohl Microsoft SQL Server als auch PostgreSQL unterstützt.</span><span class="sxs-lookup"><span data-stu-id="2f148-112">Here's an example supporting both Microsoft SQL Server and PostgreSQL.</span></span>

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

<span data-ttu-id="2f148-113">Dieser Ansatz funktioniert nur, wenn Sie jeden Anbieter kennen, auf den der benutzerdefinierte Vorgang angewendet wird.</span><span class="sxs-lookup"><span data-stu-id="2f148-113">This approach only works if you know every provider where your custom operation will be applied.</span></span>

## <a name="using-a-migrationoperation"></a><span data-ttu-id="2f148-114">Verwenden von migrationoperation</span><span class="sxs-lookup"><span data-stu-id="2f148-114">Using a MigrationOperation</span></span>

<span data-ttu-id="2f148-115">Wenn Sie den benutzerdefinierten Vorgang von SQL entkoppeln möchten, können Sie eine eigene `MigrationOperation` definieren, um Sie darzustellen.</span><span class="sxs-lookup"><span data-stu-id="2f148-115">To decouple the custom operation from the SQL, you can define your own `MigrationOperation` to represent it.</span></span> <span data-ttu-id="2f148-116">Der Vorgang wird dann an den Anbieter weitergegeben, damit er die zu generierende SQL-Datei bestimmen kann.</span><span class="sxs-lookup"><span data-stu-id="2f148-116">The operation is then passed to the provider so it can determine the appropriate SQL to generate.</span></span>

``` csharp
class CreateUserOperation : MigrationOperation
{
    public string Name { get; set; }
    public string Password { get; set; }
}
```

<span data-ttu-id="2f148-117">Bei diesem Ansatz muss die Erweiterungsmethode nur einen dieser Vorgänge `MigrationBuilder.Operations`hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="2f148-117">With this approach, the extension method just needs to add one of these operations to `MigrationBuilder.Operations`.</span></span>

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

<span data-ttu-id="2f148-118">Diese Vorgehensweise erfordert, dass jeder Anbieter weiß, wie SQL für diesen Vorgang in seinem `IMigrationsSqlGenerator` Dienst generiert wird.</span><span class="sxs-lookup"><span data-stu-id="2f148-118">This approach requires each provider to know how to generate SQL for this operation in their `IMigrationsSqlGenerator` service.</span></span> <span data-ttu-id="2f148-119">Hier ist ein Beispiel, das den Generator der SQL Server überschreibt, um den neuen Vorgang zu verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="2f148-119">Here is an example overriding the SQL Server's generator to handle the new operation.</span></span>

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
        var stringMapping = Dependencies.TypeMappingSource.FindMapping(typeof(string));

        builder
            .Append("CREATE USER ")
            .Append(sqlHelper.DelimitIdentifier(operation.Name))
            .Append(" WITH PASSWORD = ")
            .Append(stringMapping.GenerateSqlLiteral(operation.Password))
            .AppendLine(sqlHelper.StatementTerminator)
            .EndCommand();
    }
}
```

<span data-ttu-id="2f148-120">Ersetzen Sie den SQL-Generator-Standard Migrationsdienst durch den aktualisierten Dienst.</span><span class="sxs-lookup"><span data-stu-id="2f148-120">Replace the default migrations sql generator service with the updated one.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IMigrationsSqlGenerator, MyMigrationsSqlGenerator>();
```
