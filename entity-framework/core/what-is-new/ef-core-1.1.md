---
title: Neuigkeiten in EF Core 1.1 – EF Core
author: divega
ms.date: 10/27/2016
ms.assetid: C7FE8C85-445A-4F0C-97EC-CC3F7F1D6F5E
uid: core/what-is-new/ef-core-1.1
ms.openlocfilehash: 9f8f2d46f967c7d8ec4f8ea410e51531dfe3ca7b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995434"
---
# <a name="new-features-in-ef-core-11"></a><span data-ttu-id="a54bb-102">Neue Features in EF Core 1.1</span><span class="sxs-lookup"><span data-stu-id="a54bb-102">New features in EF Core 1.1</span></span>

## <a name="modelling"></a><span data-ttu-id="a54bb-103">Modellierung</span><span class="sxs-lookup"><span data-stu-id="a54bb-103">Modelling</span></span>
### <a name="field-mapping"></a><span data-ttu-id="a54bb-104">Feldzuordnung</span><span class="sxs-lookup"><span data-stu-id="a54bb-104">Field mapping</span></span>
<span data-ttu-id="a54bb-105">Ermöglicht Ihnen das Konfigurieren eines Unterstützungsfelds für eine Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="a54bb-105">Allows you to configure a backing field for a property.</span></span> <span data-ttu-id="a54bb-106">Dies kann für schreibgeschützte Eigenschaften oder Daten nützlich sein, die anstelle einer Eigenschaft Get/Set-Methoden verwenden.</span><span class="sxs-lookup"><span data-stu-id="a54bb-106">This can be useful for read-only properties, or data that has Get/Set methods rather than a property.</span></span>
### <a name="mapping-to-memory-optimized-tables-in-sql-server"></a><span data-ttu-id="a54bb-107">Zuordnung zu speicheroptimierten Tabellen in SQL Server</span><span class="sxs-lookup"><span data-stu-id="a54bb-107">Mapping to Memory-Optimized Tables in SQL Server</span></span>
<span data-ttu-id="a54bb-108">Sie können angeben, dass es sich bei der einer Entität zugeordneten Tabelle um eine speicheroptimierte Tabelle handelt.</span><span class="sxs-lookup"><span data-stu-id="a54bb-108">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="a54bb-109">Beim Einsatz von EF Core zum Erstellen und Verwalten einer auf Ihrem Modell basierenden Datenbank (entweder mit Migrationen oder `Database.EnsureCreated()`) wird eine speicheroptimierte Tabelle für diese Entitäten erstellt.</span><span class="sxs-lookup"><span data-stu-id="a54bb-109">When using EF Core to create and maintain a database based on your model (either with migrations or `Database.EnsureCreated()`), a memory-optimized table will be created for these entities.</span></span>

## <a name="change-tracking"></a><span data-ttu-id="a54bb-110">Änderungsnachverfolgung</span><span class="sxs-lookup"><span data-stu-id="a54bb-110">Change tracking</span></span>
### <a name="additional-change-tracking-apis-from-ef6"></a><span data-ttu-id="a54bb-111">Zusätzliche APIs für die Änderungsnachverfolgung aus EF6</span><span class="sxs-lookup"><span data-stu-id="a54bb-111">Additional change tracking APIs from EF6</span></span>
<span data-ttu-id="a54bb-112">Hierzu gehören beispielsweise `Reload`, `GetModifiedProperties`, `GetDatabaseValues` usw.</span><span class="sxs-lookup"><span data-stu-id="a54bb-112">Such as `Reload`, `GetModifiedProperties`, `GetDatabaseValues` etc.</span></span>

## <a name="query"></a><span data-ttu-id="a54bb-113">Abfrage</span><span class="sxs-lookup"><span data-stu-id="a54bb-113">Query</span></span>
### <a name="explicit-loading"></a><span data-ttu-id="a54bb-114">Explizites Laden</span><span class="sxs-lookup"><span data-stu-id="a54bb-114">Explicit Loading</span></span>
<span data-ttu-id="a54bb-115">Ermöglicht es Ihnen, das Auffüllen einer Navigationseigenschaft oder einer Entität auszulösen, die zuvor aus der Datenbank geladen wurde.</span><span class="sxs-lookup"><span data-stu-id="a54bb-115">Allows you to trigger population of a navigation property on an entity that was previously loaded from the database.</span></span>
### <a name="dbsetfind"></a><span data-ttu-id="a54bb-116">DbSet.Find</span><span class="sxs-lookup"><span data-stu-id="a54bb-116">DbSet.Find</span></span>
<span data-ttu-id="a54bb-117">Bietet eine einfache Möglichkeit zum Abrufen einer Entität basierend auf dem Wert ihres Primärschlüssels.</span><span class="sxs-lookup"><span data-stu-id="a54bb-117">Provides an easy way to fetch an entity based on its primary key value.</span></span>

## <a name="other"></a><span data-ttu-id="a54bb-118">Andere</span><span class="sxs-lookup"><span data-stu-id="a54bb-118">Other</span></span>
### <a name="connection-resiliency"></a><span data-ttu-id="a54bb-119">Verbindungsresilienz</span><span class="sxs-lookup"><span data-stu-id="a54bb-119">Connection resiliency</span></span>
<span data-ttu-id="a54bb-120">Für nicht erfolgreich ausgeführte Datenbankbefehle werden automatisch Neuversuche ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="a54bb-120">Automatically retries failed database commands.</span></span> <span data-ttu-id="a54bb-121">Dies ist insbesondere bei der Verbindungsherstellung mit SQL Azure nützlich, weil hier häufiger vorübergehende Fehler auftreten.</span><span class="sxs-lookup"><span data-stu-id="a54bb-121">This is especially useful when connection to SQL Azure, where transient failures are common.</span></span>
### <a name="simplified-service-replacement"></a><span data-ttu-id="a54bb-122">Vereinfachte Dienstersetzung</span><span class="sxs-lookup"><span data-stu-id="a54bb-122">Simplified service replacement</span></span>
<span data-ttu-id="a54bb-123">Es ist jetzt einfacher, von EF verwendete interne Dienste zu ersetzen.</span><span class="sxs-lookup"><span data-stu-id="a54bb-123">Makes it easier to replace internal services that EF uses.</span></span>
