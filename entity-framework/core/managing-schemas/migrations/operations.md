---
title: Benutzerdefinierte Migrationsoperationen – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2017
uid: core/managing-schemas/migrations/operations
ms.openlocfilehash: 93de6ee1b2eda1875188ace6eda299260fbcc1fe
ms.sourcegitcommit: 082946dcaa1ee5174e692dbfe53adeed40609c6a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2018
ms.locfileid: "51028082"
---
<a name="custom-migrations-operations"></a>Benutzerdefinierte Migrationsoperationen
============================
Die MigrationBuilder-API können Sie viele verschiedene Arten von Vorgängen bei der Migration ausführen, aber er ist längst nicht vollständig. Die API ist jedoch auch erweiterbar und ermöglichen Ihnen, Ihre eigenen Vorgänge zu definieren. Es gibt zwei Möglichkeiten, um die API zu erweitern: Verwenden der `Sql()` -Methode, oder durch Definieren von benutzerdefinierten `MigrationOperation` Objekte.

Um zu veranschaulichen, betrachten wir einen Vorgang, der erstellt einen Datenbankbenutzer, der mit jeder Ansatz, zu implementieren. In unserem Migrationen möchten wir aktivieren Sie den folgenden Code zu schreiben:

``` csharp
migrationBuilder.CreateUser("SQLUser1", "Password");
```

<a name="using-migrationbuildersql"></a>Verwenden von MigrationBuilder.Sql()
----------------------------
Die einfachste Möglichkeit zum Implementieren eines benutzerdefinierten Vorgangs ist eine Erweiterungsmethode zu definieren, die aufruft `MigrationBuilder.Sql()`.
Hier ist ein Beispiel, das die entsprechenden Transact-SQL generiert.

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
    => migrationBuilder.Sql($"CREATE USER {name} WITH PASSWORD '{password}';");
```

Wenn Sie Ihre Migrationen mehrere Datenbankanbieter unterstützen müssen, können Sie mithilfe der `MigrationBuilder.ActiveProvider` Eigenschaft. Hier ist ein Beispiel für Unterstützung von Microsoft SQL Server und PostgreSQL.

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

Dieser Ansatz funktioniert nur, wenn Sie wissen, dass jeder Anbieter, wo der benutzerdefinierte Vorgang angewendet werden.

<a name="using-a-migrationoperation"></a>Verwenden eine MigrationOperation
---------------------------
Um der benutzerdefinierte Vorgang aus dem SQL zu entkoppeln, Sie können Ihren eigenen definieren `MigrationOperation` für die Darstellung. Der Vorgang wird dann an den Anbieter übergeben, damit sie die entsprechende SQL generieren bestimmen kann.

``` csharp
class CreateUserOperation : MigrationOperation
{
    public string Name { get; set; }
    public string Password { get; set; }
}
```

Bei diesem Ansatz die Erweiterungsmethode muss lediglich eine von diesen Vorgängen hinzufügen `MigrationBuilder.Operations`.

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

Dieser Ansatz erfordert jeder Anbieter wissen, wie Sie für diesen Vorgang im SQL generieren die `IMigrationsSqlGenerator` Service. Hier ist ein Beispiel, das Überschreiben der SQL Server Generator aus, um den neuen Vorgang zu verarbeiten.

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

Ersetzen Sie den Standarddienst Migrationen Sql-Generator, mit dem aktualisierten.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IMigrationsSqlGenerator, MyMigrationsSqlGenerator>();
```
