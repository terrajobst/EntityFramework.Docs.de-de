---
title: Anpassen der Verlaufstabelle Migrationen – EF 6
author: divega
ms.date: 2016-10-23
ms.assetid: ed5518f0-a9a6-454e-9e98-a4fa7748c8d0
ms.openlocfilehash: ad07b1fe97b3dafe789c0315bd555f979fc412e7
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996760"
---
# <a name="customizing-the-migrations-history-table"></a><span data-ttu-id="4fb28-102">Anpassen der Verlaufstabelle Migrationen</span><span class="sxs-lookup"><span data-stu-id="4fb28-102">Customizing the migrations history table</span></span>
> [!NOTE]
> <span data-ttu-id="4fb28-103">**Nur EF6 und höher:** Die Features, APIs usw., die auf dieser Seite erläutert werden, wurden in Entity Framework 6 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="4fb28-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="4fb28-104">Wenn Sie eine frühere Version verwenden, gelten manche Informationen nicht.</span><span class="sxs-lookup"><span data-stu-id="4fb28-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

> [!NOTE]
> <span data-ttu-id="4fb28-105">In diesem Artikel wird davon ausgegangen, dass Sie wissen, wie Code First-Migrationen in grundlegenden Szenarien verwenden.</span><span class="sxs-lookup"><span data-stu-id="4fb28-105">This article assumes you know how to use Code First Migrations in basic scenarios.</span></span> <span data-ttu-id="4fb28-106">Wenn Sie dies nicht tun, müssen Sie lesen [Code First-Migrationen](~/ef6/modeling/code-first/migrations/index.md) vor dem fortfahren.</span><span class="sxs-lookup"><span data-stu-id="4fb28-106">If you don’t, then you’ll need to read [Code First Migrations](~/ef6/modeling/code-first/migrations/index.md) before continuing.</span></span>

## <a name="what-is-migrations-history-table"></a><span data-ttu-id="4fb28-107">Was ist Migrations History Table?</span><span class="sxs-lookup"><span data-stu-id="4fb28-107">What is Migrations History Table?</span></span>

<span data-ttu-id="4fb28-108">Migrationen Verlaufstabelle ist eine Tabelle, die durch Code First-Migrationen verwendet werden, um Details zu Migrationen angewendet werden, in der Datenbank speichern.</span><span class="sxs-lookup"><span data-stu-id="4fb28-108">Migrations history table is a table used by Code First Migrations to store details about migrations applied to the database.</span></span> <span data-ttu-id="4fb28-109">Standardmäßig ist der Name der Tabelle in der Datenbank \_ \_MigrationHistory und es wird erstellt, wenn die erste Migration führen Sie die Datenbank anwenden.</span><span class="sxs-lookup"><span data-stu-id="4fb28-109">By default the name of the table in the database is \_\_MigrationHistory and it is created when applying the first migration do the database.</span></span> <span data-ttu-id="4fb28-110">In Entity Framework 5 wurde in dieser Tabelle eine Systemtabelle aus, wenn die Anwendung Microsoft Sql Server-Datenbank verwendet.</span><span class="sxs-lookup"><span data-stu-id="4fb28-110">In Entity Framework 5 this table was a system table if the application used Microsoft Sql Server database.</span></span> <span data-ttu-id="4fb28-111">Dies wurde geändert in Entity Framework 6 jedoch, die Migrationen Verlaufstabelle wird nicht mehr eine Systemtabelle markiert.</span><span class="sxs-lookup"><span data-stu-id="4fb28-111">This has changed in Entity Framework 6 however and the migrations history table is no longer marked a system table.</span></span>

## <a name="why-customize-migrations-history-table"></a><span data-ttu-id="4fb28-112">Warum sollte werden Migrations History Table angepasst?</span><span class="sxs-lookup"><span data-stu-id="4fb28-112">Why customize Migrations History Table?</span></span>

<span data-ttu-id="4fb28-113">Migrationen Verlaufstabelle ausschließlich von Code First-Migrationen verwendet werden soll, und es manuell zu ändern, kann Migrationen unterbrechen.</span><span class="sxs-lookup"><span data-stu-id="4fb28-113">Migrations history table is supposed to be used solely by Code First Migrations and changing it manually can break migrations.</span></span> <span data-ttu-id="4fb28-114">Jedoch manchmal die Standardkonfiguration ist nicht geeignet, und die Tabelle muss, z. B. angepasst werden:</span><span class="sxs-lookup"><span data-stu-id="4fb28-114">However sometimes the default configuration is not suitable and the table needs to be customized, for instance:</span></span>

-   <span data-ttu-id="4fb28-115">So ändern Sie Namen und/oder Facets von Spalten, um eine 3 aktivieren benötigten<sup>rd</sup> Migrationen-Anbieters</span><span class="sxs-lookup"><span data-stu-id="4fb28-115">You need to change names and/or facets of the columns to enable a 3<sup>rd</sup> party Migrations provider</span></span>
-   <span data-ttu-id="4fb28-116">Der Name der Tabelle geändert werden soll</span><span class="sxs-lookup"><span data-stu-id="4fb28-116">You want to change the name of the table</span></span>
-   <span data-ttu-id="4fb28-117">Sie benötigen ein nicht dem Standard-Schema für die \_ \_Tabelle "MigrationHistory"</span><span class="sxs-lookup"><span data-stu-id="4fb28-117">You need to use a non-default schema for the \_\_MigrationHistory table</span></span>
-   <span data-ttu-id="4fb28-118">Sie müssen zusätzliche Daten für eine bestimmte Version des Kontexts zu speichern und aus diesem Grund müssen Sie eine zusätzliche Spalte zur Tabelle hinzufügen</span><span class="sxs-lookup"><span data-stu-id="4fb28-118">You need to store additional data for a given version of the context and therefore you need to add an additional column to the table</span></span>

## <a name="words-of-precaution"></a><span data-ttu-id="4fb28-119">Wörter vorsorglich</span><span class="sxs-lookup"><span data-stu-id="4fb28-119">Words of precaution</span></span>

<span data-ttu-id="4fb28-120">Ändern der migrationsverlaufstabelle ist leistungsstark, aber Sie müssen darauf achten, nicht übertreiben.</span><span class="sxs-lookup"><span data-stu-id="4fb28-120">Changing the migration history table is powerful but you need to be careful to not overdo it.</span></span> <span data-ttu-id="4fb28-121">EF Runtime überprüft, ob die angepasste Migrationen-Verlaufstabelle mit der Laufzeit kompatibel ist derzeit nicht.</span><span class="sxs-lookup"><span data-stu-id="4fb28-121">EF runtime currently does not check whether the customized migrations history table is compatible with the runtime.</span></span> <span data-ttu-id="4fb28-122">Wenn es sich nicht um Ihre Anwendung ist möglicherweise zur Laufzeit unterbrechen oder auf unvorhersehbare Weise Verhalten.</span><span class="sxs-lookup"><span data-stu-id="4fb28-122">If it is not your application may break at runtime or behave in unpredictable ways.</span></span> <span data-ttu-id="4fb28-123">Dies ist sogar noch wichtiger, wenn Sie mehrere Kontexte pro Datenbank verwenden in diesem Fall mehrere Kontexte der gleichen migrationsverlaufstabelle verwenden können zum Speichern von Informationen zu Migrationen.</span><span class="sxs-lookup"><span data-stu-id="4fb28-123">This is even more important if you use multiple contexts per database in which case multiple contexts can use the same migration history table to store information about migrations.</span></span>

## <a name="how-to-customize-migrations-history-table"></a><span data-ttu-id="4fb28-124">Wie Migrations History Table angepasst?</span><span class="sxs-lookup"><span data-stu-id="4fb28-124">How to customize Migrations History Table?</span></span>

<span data-ttu-id="4fb28-125">Bevor Sie beginnen, müssen Sie wissen, dass Sie die Verlaufstabelle Migrationen anpassen können, nur verwendet werden, bevor Sie die erste Migration anwenden.</span><span class="sxs-lookup"><span data-stu-id="4fb28-125">Before you start you need to know that you can customize the migrations history table only before you apply the first migration.</span></span> <span data-ttu-id="4fb28-126">Jetzt, um den Code.</span><span class="sxs-lookup"><span data-stu-id="4fb28-126">Now, to the code.</span></span>

<span data-ttu-id="4fb28-127">Zunächst müssen Sie zum Erstellen einer Klasse, die von System.Data.Entity.Migrations.History.HistoryContext-Klasse abgeleitet.</span><span class="sxs-lookup"><span data-stu-id="4fb28-127">First, you will need to create a class derived from System.Data.Entity.Migrations.History.HistoryContext class.</span></span> <span data-ttu-id="4fb28-128">Die Klasse "historycontext" wird von der Klasse "DbContext" abgeleitet werden, so konfigurieren die Verlaufstabelle Migrationen zum Konfigurieren von EF-Modellen mit fluent-API sehr ähnlich ist.</span><span class="sxs-lookup"><span data-stu-id="4fb28-128">The HistoryContext class is derived from the DbContext class so configuring the migrations history table is very similar to configuring EF models with fluent API.</span></span> <span data-ttu-id="4fb28-129">Sie müssen nur zum Überschreiben der OnModelCreating-Methode aus, und Verwenden von fluent-API so konfigurieren Sie in der Tabelle.</span><span class="sxs-lookup"><span data-stu-id="4fb28-129">You just need to override the OnModelCreating method and use fluent API to configure the table.</span></span>

>[!NOTE]
> <span data-ttu-id="4fb28-130">Beim Konfigurieren der EF-Modellen müssen Sie in der Regel keine Basisklasse aufrufen. OnModelCreating() aus der außer Kraft gesetzte OnModelCreating-Methode, da die DbContext.OnModelCreating() weist einen leeren Text.</span><span class="sxs-lookup"><span data-stu-id="4fb28-130">Typically when you configure EF models you don’t need to call base.OnModelCreating() from the overridden OnModelCreating method since the DbContext.OnModelCreating() has empty body.</span></span> <span data-ttu-id="4fb28-131">Dies ist nicht der Fall, wenn Sie die Verlaufstabelle Migrationen zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="4fb28-131">This is not the case when configuring the migrations history table.</span></span> <span data-ttu-id="4fb28-132">In diesem Fall die erste ist in der Überschreibung OnModelCreating() tatsächlich Basisklasse aufrufen. OnModelCreating().</span><span class="sxs-lookup"><span data-stu-id="4fb28-132">In this case the first thing to do in your OnModelCreating() override is to actually call base.OnModelCreating().</span></span> <span data-ttu-id="4fb28-133">Dadurch wird die Verlaufstabelle Migrationen in die Standardmethode für konfiguriert, die Sie, klicken Sie dann in der überschreibenden Methode anpassen.</span><span class="sxs-lookup"><span data-stu-id="4fb28-133">This will configure the migrations history table in the default way which you then tweak in the overriding method.</span></span>

<span data-ttu-id="4fb28-134">Angenommen, Sie verwenden möchten, benennen Sie die Verlaufstabelle Migrationen und platzieren Sie ihn in ein benutzerdefiniertes Schema namens "Admin".</span><span class="sxs-lookup"><span data-stu-id="4fb28-134">Let’s say you want to rename the migrations history table and put it to a custom schema called “admin”.</span></span> <span data-ttu-id="4fb28-135">Darüber hinaus Ihr Datenbankadministrator möchten Sie die MigrationId-Spalte umbenennen, auf die Migration zu\_-ID.</span><span class="sxs-lookup"><span data-stu-id="4fb28-135">In addition your DBA would like you to rename the MigrationId column to Migration\_ID.</span></span>  <span data-ttu-id="4fb28-136">Sie können dies erreichen, erstellen Sie die folgende Klasse, die von "historycontext" abgeleitet:</span><span class="sxs-lookup"><span data-stu-id="4fb28-136">You could achieve this by creating the following class derived from HistoryContext:</span></span>

``` csharp
    using System.Data.Common;
    using System.Data.Entity;
    using System.Data.Entity.Migrations.History;

    namespace CustomizableMigrationsHistoryTableSample
    {
        public class MyHistoryContext : HistoryContext
        {
            public MyHistoryContext(DbConnection dbConnection, string defaultSchema)
                : base(dbConnection, defaultSchema)
            {
            }

            protected override void OnModelCreating(DbModelBuilder modelBuilder)
            {
                base.OnModelCreating(modelBuilder);
                modelBuilder.Entity<HistoryRow>().ToTable(tableName: "MigrationHistory", schemaName: "admin");
                modelBuilder.Entity<HistoryRow>().Property(p => p.MigrationId).HasColumnName("Migration_ID");
            }
        }
    }
```

<span data-ttu-id="4fb28-137">Sobald Ihr benutzerdefinierte "historycontext" bereit ist, müssen Sie EF davon aufmerksam zu machen, durch die Registrierung über [codebasierte Konfiguration](http://msdn.com/data/jj680699):</span><span class="sxs-lookup"><span data-stu-id="4fb28-137">Once your custom HistoryContext is ready you need to make EF aware of it by registering it via [code-based configuration](http://msdn.com/data/jj680699):</span></span>

``` csharp
    using System.Data.Entity;

    namespace CustomizableMigrationsHistoryTableSample
    {
        public class ModelConfiguration : DbConfiguration
        {
            public ModelConfiguration()
            {
                this.SetHistoryContext("System.Data.SqlClient",
                    (connection, defaultSchema) => new MyHistoryContext(connection, defaultSchema));
            }
        }
    }
```

<span data-ttu-id="4fb28-138">Das ist ziemlich alles.</span><span class="sxs-lookup"><span data-stu-id="4fb28-138">That’s pretty much it.</span></span> <span data-ttu-id="4fb28-139">Nachdem Sie auf der Paket-Manager-Konsole, Enable-Migrations, zugreifen können Add-Migration und schließlich die Datenbank aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="4fb28-139">Now you can go to the Package Manager Console, Enable-Migrations, Add-Migration and finally Update-Database.</span></span> <span data-ttu-id="4fb28-140">Daraufhin sollte in der Datenbank, die eine Verlaufstabelle Migrationen entsprechend den Details der konfiguriert wird, die Sie angegeben haben, in der "historycontext" abgeleitete Klasse hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="4fb28-140">This should result in adding to the database a migrations history table configured according to the details you specified in your HistoryContext derived class.</span></span>

![Datenbank](~/ef6/media/database.png)
