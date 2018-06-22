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
ms.locfileid: "34163141"
---
<a name="custom-migrations-operations"></a>Benutzerdefinierte Migrationen-Vorgänge
============================
Die MigrationBuilder-API ermöglicht Ihnen, viele verschiedene Arten von Vorgängen während einer Migration durchzuführen, aber zurückgelegt vollständig ist. Allerdings ist die API erweiterbar gestattet, eigene Vorgänge zu definieren. Es gibt zwei Methoden zum Erweitern von der API: Verwenden der `Sql()` -Methode, oder definieren benutzerdefinierte `MigrationOperation` Objekte.

Um zu veranschaulichen, sehen wir uns das Implementieren eines Vorgangs, das einen Datenbankbenutzer, der mit jeder Ansatz erstellt. In unserem Migrationen möchten wir ermöglichen, schreiben den folgenden Code:

``` csharp
migrationBuilder.CreateUser("SQLUser1", "Password");
```

<a name="using-migrationbuildersql"></a>Verwenden von MigrationBuilder.Sql()
----------------------------
Die einfachste Möglichkeit, einen Vorgang implementiert wird, um eine Erweiterungsmethode zu definieren, die aufruft `MigrationBuilder.Sql()`.
Hier ist ein Beispiel, das die entsprechende Transact-SQL generiert.

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
    => migrationBuilder.Sql($"CREATE USER {name} WITH PASSWORD '{password}';");
```

Wenn Ihre Migrationen mehrere Datenbankanbieter unterstützen müssen, können Sie mithilfe der `MigrationBuilder.ActiveProvider` Eigenschaft. Hier ist ein Beispiel für Microsoft SQL Server und PostgreSQL unterstützen.

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

Dieser Ansatz funktioniert nur, wenn Sie wissen, dass alle Anbieter, in denen Ihre benutzerdefinierte Vorgänge angewendet werden.

<a name="using-a-migrationoperation"></a>Verwenden eine MigrationOperation
---------------------------
Um den benutzerdefinierten Vorgang aus der SQL zu entkoppeln, können Sie definieren eine eigene `MigrationOperation` für die Darstellung. Der Vorgang wird dann an den Anbieter übergeben, sodass er die entsprechende SQL generieren ermitteln kann.

``` csharp
class CreateUserOperation : MigrationOperation
{
    public string Name { get; set; }
    public string Password { get; set; }
}
```

Bei diesem Ansatz die Erweiterungsmethode benötigt nur einen dieser Vorgänge zum Hinzufügen `MigrationBuilder.Operations`.

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

Dieser Ansatz erfordert jeder Anbieter wissen, wie zum Generieren von SQL für diesen Vorgang in ihre `IMigrationsSqlGenerator` Dienst. Hier ist ein Beispiel für die SQL Server-Generator zum Verarbeiten der neuen Vorgangs überschreiben.

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

Ersetzen Sie den Standarddienst Migrationen Sql-Generator, mit dem aktualisierten.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IMigrationsSqlGenerator, MyMigrationsSqlGenerator>();
```
