---
title: Plan für Entity Framework Core 5.0
author: ajcvickers
ms.date: 01/14/2020
uid: core/what-is-new/ef-core-5.0/plan.md
ms.openlocfilehash: c5b7300c61c2f668b6f9393ae51bf9ebddf330a7
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78413822"
---
# <a name="plan-for-entity-framework-core-50"></a><span data-ttu-id="dbd18-102">Plan für Entity Framework Core 5.0</span><span class="sxs-lookup"><span data-stu-id="dbd18-102">Plan for Entity Framework Core 5.0</span></span>

<span data-ttu-id="dbd18-103">Wie im [Artikel zum Planungsprozess](../release-planning.md) beschrieben, haben wir aus dem Feedback der Stakeholder diesen vorläufigen Plan für EF Core 5.0 erstellt.</span><span class="sxs-lookup"><span data-stu-id="dbd18-103">As described in the [planning process](../release-planning.md), we have gathered input from stakeholders into a tentative plan for the EF Core 5.0 release.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="dbd18-104">Er befindet sich aktuell in der Entwicklung.</span><span class="sxs-lookup"><span data-stu-id="dbd18-104">This plan is still a work-in-progress.</span></span> <span data-ttu-id="dbd18-105">Sein Inhalt ist nicht verbindlich.</span><span class="sxs-lookup"><span data-stu-id="dbd18-105">Nothing here is a commitment.</span></span> <span data-ttu-id="dbd18-106">Er dient als Ausgangspunkt und wird durch neues Feedback weiterentwickelt.</span><span class="sxs-lookup"><span data-stu-id="dbd18-106">This plan is a starting point that will evolve as we learn more.</span></span> <span data-ttu-id="dbd18-107">Es kann also sein, dass die Version 5.0 Funktionen enthalten wird, die bisher nicht geplant waren.</span><span class="sxs-lookup"><span data-stu-id="dbd18-107">Some things not currently planned for 5.0 may get pulled in.</span></span> <span data-ttu-id="dbd18-108">Im Gegenzug kann es jedoch auch passieren, dass Funktionen gestrichen werden, die bisher für die Version 5.0 geplant waren.</span><span class="sxs-lookup"><span data-stu-id="dbd18-108">Some things currently planned for 5.0 may get punted out.</span></span>

### <a name="version-number-and-release-date"></a><span data-ttu-id="dbd18-109">Versionsnummer und Veröffentlichungsdatum</span><span class="sxs-lookup"><span data-stu-id="dbd18-109">Version number and release date.</span></span>

<span data-ttu-id="dbd18-110">EF Core 5.0 soll [zusammen mit .NET 5.0](https://devblogs.microsoft.com/dotnet/introducing-net-5/) veröffentlicht werden.</span><span class="sxs-lookup"><span data-stu-id="dbd18-110">EF Core 5.0 is currently scheduled for release at [the same time as .NET 5.0](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span></span> <span data-ttu-id="dbd18-111">Deshalb wurde auch „5.0“ als Versionsnummer gewählt.</span><span class="sxs-lookup"><span data-stu-id="dbd18-111">The version "5.0" was chosen to align with .NET 5.0.</span></span>

### <a name="supported-platforms"></a><span data-ttu-id="dbd18-112">Unterstützte Plattformen</span><span class="sxs-lookup"><span data-stu-id="dbd18-112">Supported platforms</span></span>

<span data-ttu-id="dbd18-113">Dank der [Zusammenführung verschiedener Plattformen in .NET Core](https://devblogs.microsoft.com/dotnet/introducing-net-5/) wird EF Core 5.0 auf allen NET 5.0-Plattformen ausführbar sein.</span><span class="sxs-lookup"><span data-stu-id="dbd18-113">EF Core 5.0 is planned to run on any .NET 5.0 platform based on the [convergence of these platforms to .NET Core](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span></span> <span data-ttu-id="dbd18-114">Was das hinsichtlich .NET Standard und des zu verwendenden TFM bedeutet, wurde noch nicht festgelegt.</span><span class="sxs-lookup"><span data-stu-id="dbd18-114">What this means in terms of .NET Standard and the actual TFM used is still TBD.</span></span>

<span data-ttu-id="dbd18-115">EF Core 5.0 kann nicht auf .NET Framework ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="dbd18-115">EF Core 5.0 will not run on .NET Framework.</span></span>

### <a name="breaking-changes"></a><span data-ttu-id="dbd18-116">Breaking Changes</span><span class="sxs-lookup"><span data-stu-id="dbd18-116">Breaking changes</span></span>

<span data-ttu-id="dbd18-117">EF Core 5.0 wird einige Breaking Changes enthalten. Diese werden jedoch weniger gravierende Auswirkungen haben als bei EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="dbd18-117">EF Core 5.0 will contain some breaking changes, but these will be much less severe than was the case for EF Core 3.0.</span></span> <span data-ttu-id="dbd18-118">Unser Ziel ist es, die Mehrzahl der Anwendungen zu aktualisieren, ohne dass Breaking Changes auftreten.</span><span class="sxs-lookup"><span data-stu-id="dbd18-118">Our goal is to allow the vast majority of applications to update without breaking.</span></span>

<span data-ttu-id="dbd18-119">Dennoch gehen wir davon, dass Datenbankanbieter auf einige Breaking Changes stoßen werden, besonders im Bereich TPT-Unterstützung.</span><span class="sxs-lookup"><span data-stu-id="dbd18-119">It is expected that there will be some breaking changes for database providers, especially around TPT support.</span></span> <span data-ttu-id="dbd18-120">Wir gehen jedoch davon aus, dass der Arbeitsaufwand für das Aktualisieren eines Providers auf 5.0 geringer ausfällt als bei 3.0.</span><span class="sxs-lookup"><span data-stu-id="dbd18-120">However, we expect the work to update a provider for 5.0 will be less than was required to update for 3.0.</span></span>

## <a name="themes"></a><span data-ttu-id="dbd18-121">Designs</span><span class="sxs-lookup"><span data-stu-id="dbd18-121">Themes</span></span>

<span data-ttu-id="dbd18-122">Wir haben einige wichtige Bereiche/Themen bestimmt, die die Grundlage für die großen Änderungen in EF Core 5.0 bilden.</span><span class="sxs-lookup"><span data-stu-id="dbd18-122">We have extracted a few major areas or themes which will form the basis for the large investments in EF Core 5.0.</span></span>

## <a name="many-to-many-navigation-properties-aka-skip-navigations"></a><span data-ttu-id="dbd18-123">M:n-Navigationseigenschaften (auch bekannt als „Navigationseigenschaften überspringen“)</span><span class="sxs-lookup"><span data-stu-id="dbd18-123">Many-to-many navigation properties (a.k.a "skip navigations")</span></span>

<span data-ttu-id="dbd18-124">Leitende Entwickler: @smitpatel und @AndriySvyryd</span><span class="sxs-lookup"><span data-stu-id="dbd18-124">Lead developers: @smitpatel and @AndriySvyryd</span></span>

<span data-ttu-id="dbd18-125">Nachverfolgbar über [#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003)</span><span class="sxs-lookup"><span data-stu-id="dbd18-125">Tracked by [#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003)</span></span>

<span data-ttu-id="dbd18-126">T-Shirt-Größe: L</span><span class="sxs-lookup"><span data-stu-id="dbd18-126">T-shirt size: L</span></span>

<span data-ttu-id="dbd18-127">Status: In Bearbeitung</span><span class="sxs-lookup"><span data-stu-id="dbd18-127">Status: In-progress</span></span>

<span data-ttu-id="dbd18-128">„m:n“ ist die am [meisten geforderte Funktion](https://github.com/aspnet/EntityFrameworkCore/issues/1368) (~407 Stimmen) im GitHub-Backlog.</span><span class="sxs-lookup"><span data-stu-id="dbd18-128">Many-to-many is the [most requested feature](https://github.com/aspnet/EntityFrameworkCore/issues/1368) (~407 votes) on the GitHub backlog.</span></span>

<span data-ttu-id="dbd18-129">Die Unterstützung für m:n-Beziehungen in ihrer Gesamtheit wird als [#10508](https://github.com/aspnet/EntityFrameworkCore/issues/10508) nachverfolgt.</span><span class="sxs-lookup"><span data-stu-id="dbd18-129">Support for many-to-many relationships in their entirety is tracked as [#10508](https://github.com/aspnet/EntityFrameworkCore/issues/10508).</span></span> <span data-ttu-id="dbd18-130">Dies kann in drei Hauptbereiche aufgeteilt werden:</span><span class="sxs-lookup"><span data-stu-id="dbd18-130">This can be broken down into three major areas:</span></span>

* <span data-ttu-id="dbd18-131">Navigationseigenschaften überspringen.</span><span class="sxs-lookup"><span data-stu-id="dbd18-131">Skip navigation properties.</span></span> <span data-ttu-id="dbd18-132">Dank dieser Eigenschaften kann ein Modell für Abfragen usw. verwendet werden, ohne dass ein Verweis auf die zugrunde liegende Entität „Jointabelle“ benötigt wird.</span><span class="sxs-lookup"><span data-stu-id="dbd18-132">These allow the model to be used for queries, etc. without reference to the underlying join table entity.</span></span> <span data-ttu-id="dbd18-133">([#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003))</span><span class="sxs-lookup"><span data-stu-id="dbd18-133">([#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003))</span></span>
* <span data-ttu-id="dbd18-134">Entitätstypen für Eigenschaftenbehälter.</span><span class="sxs-lookup"><span data-stu-id="dbd18-134">Property-bag entity types.</span></span> <span data-ttu-id="dbd18-135">Dank dieser Entitätstypen kann für Entitätsinstanzen ein Standard-CLR-Typ wie `Dictionary` verwendet werden. So ist für einen Entitätstyp kein expliziter CLR-Typ mehr notwendig.</span><span class="sxs-lookup"><span data-stu-id="dbd18-135">These allow a standard CLR type (e.g. `Dictionary`) to be used for entity instances such that an explicit CLR type is not needed for each entity type.</span></span> <span data-ttu-id="dbd18-136">(Stretch für 5.0: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914).)</span><span class="sxs-lookup"><span data-stu-id="dbd18-136">(Stretch for 5.0: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914).)</span></span>
* <span data-ttu-id="dbd18-137">Sugar für die einfache Konfiguration von m:n-Beziehungen.</span><span class="sxs-lookup"><span data-stu-id="dbd18-137">Sugar for easy configuration of many-to-many relationships.</span></span> <span data-ttu-id="dbd18-138">(Stretch für 5.0.)</span><span class="sxs-lookup"><span data-stu-id="dbd18-138">(Stretch for 5.0.)</span></span>

<span data-ttu-id="dbd18-139">Unserer Meinung nach liegt der Wunsch nach m:n-Unterstützung hauptsächlich daran, dass bei Geschäftslogik wie Queries bisher keine Möglichkeit besteht, die „natürlichen“ Beziehungen zu verwenden, ohne auf die Jointabelle verweisen zu müssen.</span><span class="sxs-lookup"><span data-stu-id="dbd18-139">We believe that the most significant blocker for those wanting many-to-many support is not being able to use the "natural" relationships, without referring to the join table, in business logic such as queries.</span></span> <span data-ttu-id="dbd18-140">Der Entitätstyp „Jointabelle“ existiert zwar noch, sollte in Geschäftslogik aber nicht mehr verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="dbd18-140">The join table entity type may still exist, but it should not get in the way of business logic.</span></span> <span data-ttu-id="dbd18-141">Deshalb haben wir uns entschieden, in der Version 5.0 das Thema „Navigationseigenschaften überspringen“ anzugehen.</span><span class="sxs-lookup"><span data-stu-id="dbd18-141">This is why we have chosen to tackle skip navigation properties for 5.0.</span></span>

<span data-ttu-id="dbd18-142">Darüber hinausgehende m:n-Funktionen sind für EF Core 5.0 als „Stretch Goal“ definiert.</span><span class="sxs-lookup"><span data-stu-id="dbd18-142">At this time the other parts of many-to-many are being pursued as a stretch goal for EF Core 5.0.</span></span> <span data-ttu-id="dbd18-143">Das bedeutet, sie sind aktuell nicht für die Version 5.0 vorgesehen, sollen aber bei gutem Projektfortschritt noch integriert werden.</span><span class="sxs-lookup"><span data-stu-id="dbd18-143">This means they are not currently in the plan for 5.0, but if things go well we hope to pull them in.</span></span>

## <a name="table-per-type-tpt-inheritance-mapping"></a><span data-ttu-id="dbd18-144">Tabelle-pro-Typ-Vererbungszuordnung (TPT)</span><span class="sxs-lookup"><span data-stu-id="dbd18-144">Table-per-type (TPT) inheritance mapping</span></span>

<span data-ttu-id="dbd18-145">Leitender Entwickler: @AndriySvyryd</span><span class="sxs-lookup"><span data-stu-id="dbd18-145">Lead developer: @AndriySvyryd</span></span>

<span data-ttu-id="dbd18-146">Nachverfolgbar über [#2266](https://github.com/aspnet/EntityFrameworkCore/issues/2266)</span><span class="sxs-lookup"><span data-stu-id="dbd18-146">Tracked by [#2266](https://github.com/aspnet/EntityFrameworkCore/issues/2266)</span></span>

<span data-ttu-id="dbd18-147">T-Shirt-Größe: XL</span><span class="sxs-lookup"><span data-stu-id="dbd18-147">T-shirt size: XL</span></span>

<span data-ttu-id="dbd18-148">Status: Nicht begonnen</span><span class="sxs-lookup"><span data-stu-id="dbd18-148">Status: Not started</span></span>

<span data-ttu-id="dbd18-149">Wir arbeiten an der TPT-Unterstützung, weil diese Funktion sehr stark nachgefragt wurde (~254 Stimmen, 3. Platz insgesamt) und weil dafür nur wenige spezifische Änderungen erforderlich sind, die den allgemeinen Grundlagen des .NET 5-Plans entsprechen.</span><span class="sxs-lookup"><span data-stu-id="dbd18-149">We're doing TPT because it is both a highly requested feature (~254 votes; 3rd overall) and because it requires some low-level changes that we feel are appropriate for the foundational nature of the overall .NET 5 plan.</span></span> <span data-ttu-id="dbd18-150">Datenbankanbieter werden deshalb höchstwahrscheinlich Breaking Changes bemerken. Diese erfordern jedoch viel weniger Arbeit als bei der Umstellung auf Version 3.0.</span><span class="sxs-lookup"><span data-stu-id="dbd18-150">We expect this to result in breaking changes for database providers, although these should be much less severe than the changes required for 3.0.</span></span>

## <a name="filtered-include"></a><span data-ttu-id="dbd18-151">Gefilterte Include-Funktion</span><span class="sxs-lookup"><span data-stu-id="dbd18-151">Filtered Include</span></span>

<span data-ttu-id="dbd18-152">Leitender Entwickler: @maumar</span><span class="sxs-lookup"><span data-stu-id="dbd18-152">Lead developer: @maumar</span></span>

<span data-ttu-id="dbd18-153">Nachverfolgbar über [#1833](https://github.com/aspnet/EntityFrameworkCore/issues/1833)</span><span class="sxs-lookup"><span data-stu-id="dbd18-153">Tracked by [#1833](https://github.com/aspnet/EntityFrameworkCore/issues/1833)</span></span>

<span data-ttu-id="dbd18-154">T-Shirt-Größe: M</span><span class="sxs-lookup"><span data-stu-id="dbd18-154">T-shirt size: M</span></span>

<span data-ttu-id="dbd18-155">Status: Nicht begonnen</span><span class="sxs-lookup"><span data-stu-id="dbd18-155">Status: Not started</span></span>

<span data-ttu-id="dbd18-156">Die gefilterte Include-Funktion wurde sehr oft gewünscht (~317 Stimmen, 2. Platz insgesamt). Sie bedarf nicht viel Arbeit und ermöglicht bzw. vereinfacht viele Szenarios, für die bisher Filter auf Modellebene oder komplexere Anforderungen nötig sind.</span><span class="sxs-lookup"><span data-stu-id="dbd18-156">Filtered Include is a highly-requested feature (~317 votes; 2nd overall) that isn't a huge amount of work, and that we believe will unblock or make easier many scenarios that currently require model-level filters or more complex queries.</span></span>

## <a name="rationalize-totable-toquery-toview-fromsql-etc"></a><span data-ttu-id="dbd18-157">Rationalisieren von „ToTable“, „ToQuery“, „ToView“, „FromSQL“, usw.</span><span class="sxs-lookup"><span data-stu-id="dbd18-157">Rationalize ToTable, ToQuery, ToView, FromSql, etc.</span></span>

<span data-ttu-id="dbd18-158">Leitende Entwickler: @maumar und @smitpatel</span><span class="sxs-lookup"><span data-stu-id="dbd18-158">Lead developers: @maumar and @smitpatel</span></span>

<span data-ttu-id="dbd18-159">Nachverfolgbar über [#17270](https://github.com/aspnet/EntityFrameworkCore/issues/17270)</span><span class="sxs-lookup"><span data-stu-id="dbd18-159">Tracked by [#17270](https://github.com/aspnet/EntityFrameworkCore/issues/17270)</span></span>

<span data-ttu-id="dbd18-160">T-Shirt-Größe: L</span><span class="sxs-lookup"><span data-stu-id="dbd18-160">T-shirt size: L</span></span>

<span data-ttu-id="dbd18-161">Status: Nicht begonnen</span><span class="sxs-lookup"><span data-stu-id="dbd18-161">Status: Not started</span></span>

<span data-ttu-id="dbd18-162">Bereits in früheren Versionen wurden Schritte zur Unterstützung von unformatierten SQL-Abfragen, schlüssellosen Typen sowie damit zusammenhängenden Dingen unternommen.</span><span class="sxs-lookup"><span data-stu-id="dbd18-162">We have made progress in previous releases towards supporting raw SQL, keyless types, and related areas.</span></span> <span data-ttu-id="dbd18-163">Beim Zusammenspiel des Ganzen bestehen jedoch noch Inkonsistenzen und Verbesserungsbedarf.</span><span class="sxs-lookup"><span data-stu-id="dbd18-163">However, there are both gaps and inconsistencies in the way everything works together as a whole.</span></span> <span data-ttu-id="dbd18-164">Ziel in Version 5.0 ist es, daran zu arbeiten und den Benutzern das Definieren, Migrieren und Verwenden verschiedener Entitätstypen sowie der dazugehörigen Abfragen und Datenbankartefakte zu erleichtern.</span><span class="sxs-lookup"><span data-stu-id="dbd18-164">The goal for 5.0 is to fix these and create a good experience for defining, migrating, and using different types of entities and their associated queries and database artifacts.</span></span> <span data-ttu-id="dbd18-165">Dafür sind ggf. auch Aktualisierungen der kompilierten Abfrage-API erforderlich.</span><span class="sxs-lookup"><span data-stu-id="dbd18-165">This may also involve updates to the compiled query API.</span></span>

<span data-ttu-id="dbd18-166">Dadurch kann es jedoch zu Breaking Changes auf Anwendungsebene kommen, da manche der aktuell verfügbaren Funktionen den Benutzern zu viel ermöglichen und dadurch Fehler provoziert werden.</span><span class="sxs-lookup"><span data-stu-id="dbd18-166">Note that this item may result in some application-level breaking changes since some of the functionality we currently have is too permissive such that it can quickly lead people into pits of failure.</span></span> <span data-ttu-id="dbd18-167">Deshalb werden später wahrscheinliche einige dieser Funktionen blockiert sein. Sie erhalten dann stattdessen eine Anleitung mit einer alternativen Lösung.</span><span class="sxs-lookup"><span data-stu-id="dbd18-167">We will likely end up blocking some of this functionality together with guidance on what to do instead.</span></span>

## <a name="general-query-enhancements"></a><span data-ttu-id="dbd18-168">Allgemeine Abfrageverbesserungen</span><span class="sxs-lookup"><span data-stu-id="dbd18-168">General query enhancements</span></span>

<span data-ttu-id="dbd18-169">Leitende Entwickler: @smitpatel und @maumar</span><span class="sxs-lookup"><span data-stu-id="dbd18-169">Lead developers: @smitpatel and @maumar</span></span>

<span data-ttu-id="dbd18-170">Nachverfolgbar über [Liste der Probleme im 5.0-Meilenstein mit der Bezeichnung `area-query`](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-query+milestone%3A5.0.0+)</span><span class="sxs-lookup"><span data-stu-id="dbd18-170">Tracked by [issues labeled with `area-query` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-query+milestone%3A5.0.0+)</span></span>

<span data-ttu-id="dbd18-171">T-Shirt-Größe: XL</span><span class="sxs-lookup"><span data-stu-id="dbd18-171">T-shirt size: XL</span></span>

<span data-ttu-id="dbd18-172">Status: In Bearbeitung</span><span class="sxs-lookup"><span data-stu-id="dbd18-172">Status: In-progress</span></span>

<span data-ttu-id="dbd18-173">Für EF Core 3.0 wurde der Code für die Abfrageübersetzung umfassend umgeschrieben.</span><span class="sxs-lookup"><span data-stu-id="dbd18-173">The query translation code was extensively rewritten for EF Core 3.0.</span></span> <span data-ttu-id="dbd18-174">Deshalb ist er allgemein viel robuster.</span><span class="sxs-lookup"><span data-stu-id="dbd18-174">The query code is generally in a much more robust state because of this.</span></span> <span data-ttu-id="dbd18-175">Bei Version 5.0 sind neben den für die TPT-Unterstützung und das Überspringen von Navigationseigenschaften erforderlichen Änderungen keine großen Anpassungen an der Abfrageübersetzung vorgesehen.</span><span class="sxs-lookup"><span data-stu-id="dbd18-175">For 5.0 we aren't planning on making major query changes, outside those needed to support TPT and skip navigation properties.</span></span> <span data-ttu-id="dbd18-176">Es bestehen jedoch noch einige technische Altlasten aus dem Wechsel zu Version 3.0, für die noch viel Arbeit erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="dbd18-176">However, there is still significant work needed to fix some technical debt left over from the 3.0 overhaul.</span></span> <span data-ttu-id="dbd18-177">Zusätzlich werden viele Fehler behoben und kleine Verbesserungen eingeführt, wodurch die Benutzer Abfragen noch besser ausführen können.</span><span class="sxs-lookup"><span data-stu-id="dbd18-177">We also plan to fix many bugs and implement small enhancements to further improve the overall query experience.</span></span>

## <a name="migrations-and-deployment-experience"></a><span data-ttu-id="dbd18-178">Migration und Bereitstellung</span><span class="sxs-lookup"><span data-stu-id="dbd18-178">Migrations and deployment experience</span></span>

<span data-ttu-id="dbd18-179">Leitender Entwickler: @bricelam</span><span class="sxs-lookup"><span data-stu-id="dbd18-179">Lead developers: @bricelam</span></span>

<span data-ttu-id="dbd18-180">Nachverfolgbar über [#19587](https://github.com/dotnet/efcore/issues/19587)</span><span class="sxs-lookup"><span data-stu-id="dbd18-180">Tracked by [#19587](https://github.com/dotnet/efcore/issues/19587)</span></span>

<span data-ttu-id="dbd18-181">T-Shirt-Größe: L</span><span class="sxs-lookup"><span data-stu-id="dbd18-181">T-shirt size: L</span></span>

<span data-ttu-id="dbd18-182">Status: In Bearbeitung</span><span class="sxs-lookup"><span data-stu-id="dbd18-182">Status: In-progress</span></span>

<span data-ttu-id="dbd18-183">Derzeit migrieren viele Entwickler ihre Datenbanken beim Start der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="dbd18-183">Currently, many developers migrate their databases at application startup time.</span></span> <span data-ttu-id="dbd18-184">Das ist einfach, empfiehlt sich jedoch aus folgenden Gründen nicht:</span><span class="sxs-lookup"><span data-stu-id="dbd18-184">This is easy but is not recommended because:</span></span>

* <span data-ttu-id="dbd18-185">Mehrere Threads/Prozesse/Server versuchen möglicherweise gleichzeitig, die Datenbank zu migrieren.</span><span class="sxs-lookup"><span data-stu-id="dbd18-185">Multiple threads/processes/servers may attempt to migrate the database concurrently</span></span>
* <span data-ttu-id="dbd18-186">Anwendungen versuchen möglicherweise, den Zustand „inkonsistent“ zu erreichen, während dies geschieht.</span><span class="sxs-lookup"><span data-stu-id="dbd18-186">Applications may try to access inconsistent state while this is happening</span></span>
* <span data-ttu-id="dbd18-187">Normalerweise sollte die Berechtigung, das Schema zu modifizieren, nicht bei Ausführung der Anwendung erteilt werden.</span><span class="sxs-lookup"><span data-stu-id="dbd18-187">Usually the database permissions to modify the schema should not be granted for application execution</span></span>
* <span data-ttu-id="dbd18-188">Wenn etwas schiefgeht, ist es schwer, wieder einen fehlerfreien Zustand zu erreichen.</span><span class="sxs-lookup"><span data-stu-id="dbd18-188">It's hard to revert back to a clean state if something goes wrong</span></span>

<span data-ttu-id="dbd18-189">Wir möchten dafür sorgen, dass es einfach ist, die Datenbank zum Zeitpunkt der Bereitstellung zu migrieren.</span><span class="sxs-lookup"><span data-stu-id="dbd18-189">We want to deliver a better experience here that allows an easy way to migrate the database at deployment time.</span></span> <span data-ttu-id="dbd18-190">Dafür haben wir uns folgende Ziele gesetzt:</span><span class="sxs-lookup"><span data-stu-id="dbd18-190">This should:</span></span>

* <span data-ttu-id="dbd18-191">Gemeinsame Lösung für Linux, macOS und Windows</span><span class="sxs-lookup"><span data-stu-id="dbd18-191">Work on Linux, Mac, and Windows</span></span>
* <span data-ttu-id="dbd18-192">Unkompliziert über die Befehlszeile ausführbar</span><span class="sxs-lookup"><span data-stu-id="dbd18-192">Be a good experience on the command line</span></span>
* <span data-ttu-id="dbd18-193">Unterstützung von Szenarios mit Containern</span><span class="sxs-lookup"><span data-stu-id="dbd18-193">Support scenarios with containers</span></span>
* <span data-ttu-id="dbd18-194">Kombinierbar mit häufig verwendeten Bereitstellungstools-/-flows</span><span class="sxs-lookup"><span data-stu-id="dbd18-194">Work with commonly used real-world deployment tools/flows</span></span>
* <span data-ttu-id="dbd18-195">Integration in Visual Studio (mindestens)</span><span class="sxs-lookup"><span data-stu-id="dbd18-195">Integrate into at least Visual Studio</span></span>

<span data-ttu-id="dbd18-196">Daraus werden sich wahrscheinlich viele kleine Verbesserungen bei EF Core ergeben, z. B. bessere Migrationen bei SQLite. Kombiniert mit Leitfäden und einer langfristigen Zusammenarbeit mit anderen Teams entstehen ganzheitliche Verbesserungen, nicht nur bei EF.</span><span class="sxs-lookup"><span data-stu-id="dbd18-196">The result is likely to be many small improvements in EF Core (for example, better Migrations on SQLite), together with guidance and longer-term collaborations with other teams to improve end-to-end experiences that go beyond just EF.</span></span>

## <a name="ef-core-platforms-experience"></a><span data-ttu-id="dbd18-197">EF Core-Plattformen</span><span class="sxs-lookup"><span data-stu-id="dbd18-197">EF Core platforms experience</span></span> 

<span data-ttu-id="dbd18-198">Leitende Entwickler: @roji und @bricelam</span><span class="sxs-lookup"><span data-stu-id="dbd18-198">Lead developers: @roji and @bricelam</span></span>

<span data-ttu-id="dbd18-199">Nachverfolgbar über [#19588](https://github.com/dotnet/efcore/issues/19588)</span><span class="sxs-lookup"><span data-stu-id="dbd18-199">Tracked by [#19588](https://github.com/dotnet/efcore/issues/19588)</span></span>

<span data-ttu-id="dbd18-200">T-Shirt-Größe: L</span><span class="sxs-lookup"><span data-stu-id="dbd18-200">T-shirt size: L</span></span>

<span data-ttu-id="dbd18-201">Status: Nicht begonnen</span><span class="sxs-lookup"><span data-stu-id="dbd18-201">Status: Not started</span></span>

<span data-ttu-id="dbd18-202">Wir haben gute Leitfäden für die Verwendung von EF Core in traditionellen, MVC ähnelnden Webanwendungen.</span><span class="sxs-lookup"><span data-stu-id="dbd18-202">We have good guidance for using EF Core in traditional MVC-like web applications.</span></span> <span data-ttu-id="dbd18-203">Die Leitfäden für andere Plattformen und Anwendungsmodelle sind entweder veraltet oder fehlen ganz.</span><span class="sxs-lookup"><span data-stu-id="dbd18-203">Guidance for other platforms and application models is either missing or out-of-date.</span></span> <span data-ttu-id="dbd18-204">Für EF Core 5.0 werden wir untersuchen, wie gut sich EF-Core mit den folgenden Dingen kombinieren lässt. Die Ergebnisse werden dokumentiert, und wir versuchen, weitere Verbesserungen zu erreichen.</span><span class="sxs-lookup"><span data-stu-id="dbd18-204">For EF Core 5.0, we plan to investigate, improve, and document the experience of using EF Core with:</span></span>

* <span data-ttu-id="dbd18-205">Blazor</span><span class="sxs-lookup"><span data-stu-id="dbd18-205">Blazor</span></span>
* <span data-ttu-id="dbd18-206">Xamarin, einschließlich der Verwendung von AOT/Linker-Story</span><span class="sxs-lookup"><span data-stu-id="dbd18-206">Xamarin, including using the AOT/linker story</span></span>
* <span data-ttu-id="dbd18-207">WinForms/WPF/WinUI und möglicherweise andere UI</span><span class="sxs-lookup"><span data-stu-id="dbd18-207">WinForms/WPF/WinUI and possibly other U.I.</span></span> <span data-ttu-id="dbd18-208">frameworks</span><span class="sxs-lookup"><span data-stu-id="dbd18-208">frameworks</span></span>

<span data-ttu-id="dbd18-209">Daraus werden sich wahrscheinlich viele kleine Verbesserungen bei EF Core ergeben. Kombiniert mit Leitfäden und einer langfristigen Zusammenarbeit mit anderen Teams entstehen ganzheitliche Verbesserungen, nicht nur bei EF.</span><span class="sxs-lookup"><span data-stu-id="dbd18-209">This is likely to be many small improvements in EF Core, together with guidance and longer-term collaborations with other teams to improve end-to-end experiences that go beyond just EF.</span></span>

<span data-ttu-id="dbd18-210">Bestimmte Bereiche, die wir uns ansehen möchten, sind:</span><span class="sxs-lookup"><span data-stu-id="dbd18-210">Specific areas we plan to look at are:</span></span>

* <span data-ttu-id="dbd18-211">Bereitstellung, einschließlich der Verwendung von EF-Tools, z. B. für Migrationen</span><span class="sxs-lookup"><span data-stu-id="dbd18-211">Deployment, including the experience for using EF tooling such as for Migrations</span></span>
* <span data-ttu-id="dbd18-212">Anwendungsmodelle, einschließlich Xamarin, Blazer und gegebenenfalls weitere</span><span class="sxs-lookup"><span data-stu-id="dbd18-212">Application models, including Xamarin and Blazor, and probably others</span></span>
* <span data-ttu-id="dbd18-213">SQLite-Funktionen, einschließlich räumlicher Daten und Neuerstellung von Tabellen</span><span class="sxs-lookup"><span data-stu-id="dbd18-213">SQLite experiences, including the spatial experience and table rebuilds</span></span>
* <span data-ttu-id="dbd18-214">AOT und Verknüpfungen</span><span class="sxs-lookup"><span data-stu-id="dbd18-214">AOT and linking experiences</span></span>
* <span data-ttu-id="dbd18-215">Integration von Diagnosefunktionen, einschließlich Leistungsindikatoren</span><span class="sxs-lookup"><span data-stu-id="dbd18-215">Diagnostics integration, including perf counters</span></span>

## <a name="performance"></a><span data-ttu-id="dbd18-216">Leistung</span><span class="sxs-lookup"><span data-stu-id="dbd18-216">Performance</span></span>

<span data-ttu-id="dbd18-217">Leitender Entwickler: @roji</span><span class="sxs-lookup"><span data-stu-id="dbd18-217">Lead developer: @roji</span></span>

<span data-ttu-id="dbd18-218">Nachverfolgbar über [Liste der Probleme im 5.0-Meilenstein mit der Bezeichnung `area-perf`](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-perf+milestone%3A5.0.0+)</span><span class="sxs-lookup"><span data-stu-id="dbd18-218">Tracked by [issues labeled with `area-perf` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-perf+milestone%3A5.0.0+)</span></span>

<span data-ttu-id="dbd18-219">T-Shirt-Größe: L</span><span class="sxs-lookup"><span data-stu-id="dbd18-219">T-shirt size: L</span></span>

<span data-ttu-id="dbd18-220">Status: In Bearbeitung</span><span class="sxs-lookup"><span data-stu-id="dbd18-220">Status: In-progress</span></span>

<span data-ttu-id="dbd18-221">In EF Core möchten wir die vorhandenen Leistungsbenchmarks verbessern und gezielt Leistungsverbesserungen an der Runtime vornehmen.</span><span class="sxs-lookup"><span data-stu-id="dbd18-221">For EF Core, we plan to improve our suite of performance benchmarks and make directed performance improvements to the runtime.</span></span> <span data-ttu-id="dbd18-222">Außerdem ist geplant, die Arbeit an der neuen ADO.NET-API für Batchverarbeitung abzuschließen, die während des Veröffentlichungszyklus der Version 3.0 als Prototyp vorgestellt worden war.</span><span class="sxs-lookup"><span data-stu-id="dbd18-222">In addition, we plan to complete the new ADO.NET batching API which was prototyped during the 3.0 release cycle.</span></span> <span data-ttu-id="dbd18-223">Zusätzlich möchten wir auf der ADO.NET-Ebene zusätzliche Leistungsverbesserungen bei Npgsql vornehmen.</span><span class="sxs-lookup"><span data-stu-id="dbd18-223">Also at the ADO.NET layer, we plan additional performance improvements to the Npgsql provider.</span></span>

<span data-ttu-id="dbd18-224">Des Weiteren möchten wir bei Bedarf ADO.NET/EF Core-Leistungsindikatoren und andere Diagnosefunktionen hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="dbd18-224">As part of this work we also plan to add ADO.NET/EF Core performance counters and other diagnostics as appropriate.</span></span>

## <a name="architecturalcontributor-documentation"></a><span data-ttu-id="dbd18-225">Dokumentation zu Architektur und Mitwirkenden</span><span class="sxs-lookup"><span data-stu-id="dbd18-225">Architectural/contributor documentation</span></span>

<span data-ttu-id="dbd18-226">Für die Dokumentation verantwortliche Person: @ajcvickers</span><span class="sxs-lookup"><span data-stu-id="dbd18-226">Lead documenter: @ajcvickers</span></span>

<span data-ttu-id="dbd18-227">Nachverfolgbar über [#1920](https://github.com/dotnet/EntityFramework.Docs/issues/1920)</span><span class="sxs-lookup"><span data-stu-id="dbd18-227">Tracked by [#1920](https://github.com/dotnet/EntityFramework.Docs/issues/1920)</span></span>

<span data-ttu-id="dbd18-228">T-Shirt-Größe: L</span><span class="sxs-lookup"><span data-stu-id="dbd18-228">T-shirt size: L</span></span>

<span data-ttu-id="dbd18-229">Status: Nicht begonnen</span><span class="sxs-lookup"><span data-stu-id="dbd18-229">Status: Not started</span></span>

<span data-ttu-id="dbd18-230">Ziel ist es, verständlicher zu machen, was in EF Core passiert.</span><span class="sxs-lookup"><span data-stu-id="dbd18-230">The idea here is to make it easier to understand what is going on in the internals of EF Core.</span></span> <span data-ttu-id="dbd18-231">Dies kann für alle Personen nützlich sein, die EF Core verwenden. Das Hauptziel ist jedoch, externen Personen Folgendes zu erleichtern:</span><span class="sxs-lookup"><span data-stu-id="dbd18-231">This can be useful to anyone using EF Core, but the primary motivation is to make it easier for external people to:</span></span>

* <span data-ttu-id="dbd18-232">Mitarbeit am EFF Core-Code</span><span class="sxs-lookup"><span data-stu-id="dbd18-232">Contribute to the EF Core code</span></span>
* <span data-ttu-id="dbd18-233">Erstellen von Datenbankanbietern</span><span class="sxs-lookup"><span data-stu-id="dbd18-233">Create database providers</span></span>
* <span data-ttu-id="dbd18-234">Erstellen anderer Erweiterungen</span><span class="sxs-lookup"><span data-stu-id="dbd18-234">Build other extensions</span></span>

## <a name="microsoftdatasqlite-documentation"></a><span data-ttu-id="dbd18-235">Dokumentation zu Microsoft.Data.Sqlite</span><span class="sxs-lookup"><span data-stu-id="dbd18-235">Microsoft.Data.Sqlite documentation</span></span>

<span data-ttu-id="dbd18-236">Für die Dokumentation verantwortliche Person: @bricelam</span><span class="sxs-lookup"><span data-stu-id="dbd18-236">Lead documenter: @bricelam</span></span>

<span data-ttu-id="dbd18-237">Nachverfolgbar über [#1675](https://github.com/dotnet/EntityFramework.Docs/issues/1675)</span><span class="sxs-lookup"><span data-stu-id="dbd18-237">Tracked by [#1675](https://github.com/dotnet/EntityFramework.Docs/issues/1675)</span></span>

<span data-ttu-id="dbd18-238">T-Shirt-Größe: M</span><span class="sxs-lookup"><span data-stu-id="dbd18-238">T-shirt size: M</span></span>

<span data-ttu-id="dbd18-239">Status: Abgeschlossen.</span><span class="sxs-lookup"><span data-stu-id="dbd18-239">Status: Completed.</span></span> <span data-ttu-id="dbd18-240">Die neue Dokumentation finden Sie [hier](https://docs.microsoft.com/dotnet/standard/data/sqlite/?tabs=netcore-cli).</span><span class="sxs-lookup"><span data-stu-id="dbd18-240">The new documentation is [live on the Microsoft docs site](https://docs.microsoft.com/dotnet/standard/data/sqlite/?tabs=netcore-cli).</span></span>

<span data-ttu-id="dbd18-241">Das EF-Team ist auch Eigentümer des Microsoft.Data.Sqlite ADO.NET-Anbieters.</span><span class="sxs-lookup"><span data-stu-id="dbd18-241">The EF Team also owns the Microsoft.Data.Sqlite ADO.NET provider.</span></span> <span data-ttu-id="dbd18-242">Wir planen, als Teil der Veröffentlichungen für EF Core 5.0, eine vollständige Dokumentation zu diesem Anbieter bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="dbd18-242">We plan to fully document this provider as part of the 5.0 release.</span></span>

## <a name="general-documentation"></a><span data-ttu-id="dbd18-243">Allgemeine Dokumentation</span><span class="sxs-lookup"><span data-stu-id="dbd18-243">General documentation</span></span>

<span data-ttu-id="dbd18-244">Für die Dokumentation verantwortliche Person: @ajcvickers</span><span class="sxs-lookup"><span data-stu-id="dbd18-244">Lead documenter: @ajcvickers</span></span>

<span data-ttu-id="dbd18-245">Nachverfolgbar über [Docs-Repository im 5.0-Meilenstein](https://github.com/dotnet/EntityFramework.Docs/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+)</span><span class="sxs-lookup"><span data-stu-id="dbd18-245">Tracked by [issues in the docs repo in the 5.0 milestone](https://github.com/dotnet/EntityFramework.Docs/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+)</span></span>

<span data-ttu-id="dbd18-246">T-Shirt-Größe: L</span><span class="sxs-lookup"><span data-stu-id="dbd18-246">T-shirt size: L</span></span>

<span data-ttu-id="dbd18-247">Status: In Bearbeitung</span><span class="sxs-lookup"><span data-stu-id="dbd18-247">Status: In-progress</span></span>

<span data-ttu-id="dbd18-248">Die Aktualisierung der bestehenden Dokumentation für die Releases 3.0 und 3.1 läuft bereits.</span><span class="sxs-lookup"><span data-stu-id="dbd18-248">We are already in the process of updating documentation for the 3.0 and 3.1 releases.</span></span> <span data-ttu-id="dbd18-249">Zusätzlich arbeiten wir an Folgendem:</span><span class="sxs-lookup"><span data-stu-id="dbd18-249">We are also working on:</span></span>
  * <span data-ttu-id="dbd18-250">Überarbeitung der Einsteigerdokumentation, um sie ansprechender und leichter verständlich zu machen</span><span class="sxs-lookup"><span data-stu-id="dbd18-250">An overhaul of the getting started docs to make them more approachable/easier to follow</span></span>
  * <span data-ttu-id="dbd18-251">Neuanordnung von Dokumenten, um sie leichter auffindbar zu machen und um Querverweise hinzufügen zu können</span><span class="sxs-lookup"><span data-stu-id="dbd18-251">Reorganization of docs to make things easier to find and to add cross-references</span></span>
  * <span data-ttu-id="dbd18-252">Hinzufügen von ausführlicheren Details und Erläuterungen zu vorhandenen Dokumenten</span><span class="sxs-lookup"><span data-stu-id="dbd18-252">Adding more details and clarifications to existing docs</span></span>
  * <span data-ttu-id="dbd18-253">Aktualisieren bestehender Beispiele und Hinzufügen zusätzlicher Beispiele</span><span class="sxs-lookup"><span data-stu-id="dbd18-253">Updating the samples and adding more examples</span></span>

## <a name="fixing-bugs"></a><span data-ttu-id="dbd18-254">Beheben von Fehlern</span><span class="sxs-lookup"><span data-stu-id="dbd18-254">Fixing bugs</span></span>

<span data-ttu-id="dbd18-255">Nachverfolgbar über [Liste der Probleme im 5.0-Meilenstein mit der Bezeichnung `type-bug`](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-bug+)</span><span class="sxs-lookup"><span data-stu-id="dbd18-255">Tracked by [issues labeled with `type-bug` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-bug+)</span></span>

<span data-ttu-id="dbd18-256">Entwickler: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd @ajcvickers</span><span class="sxs-lookup"><span data-stu-id="dbd18-256">Developers: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span></span>

<span data-ttu-id="dbd18-257">T-Shirt-Größe: L</span><span class="sxs-lookup"><span data-stu-id="dbd18-257">T-shirt size: L</span></span>

<span data-ttu-id="dbd18-258">Status: In Bearbeitung</span><span class="sxs-lookup"><span data-stu-id="dbd18-258">Status: In-progress</span></span>

<span data-ttu-id="dbd18-259">Zum Zeitpunkt der Entstehung dieses Artikels haben wir 135 Fehler kategorisiert, die in Version 5.0 behoben werden sollen. Davon sind 62 bereits behoben. Hier besteht jedoch eine starke Überschneidung mit den weiter oben beschriebenen _allgemeinen Abfrageverbesserungen_.</span><span class="sxs-lookup"><span data-stu-id="dbd18-259">At the time of writing, we have 135 bugs triaged to be fixed in the 5.0 release (with 62 already fixed), but there is significant overlap with the _General query enhancements_ section above.</span></span>

<span data-ttu-id="dbd18-260">Im Laufe des Releases 3.0 wurden pro Monat etwa 23 Probleme in Meilensteine aufgenommen.</span><span class="sxs-lookup"><span data-stu-id="dbd18-260">The incoming rate (issues that end up as work in a milestone) was about 23 issues per month over the course of the 3.0 release.</span></span> <span data-ttu-id="dbd18-261">Davon müssen jedoch nicht alle in Version 5.0 behoben werden.</span><span class="sxs-lookup"><span data-stu-id="dbd18-261">Not all of these will need to be fixed in 5.0.</span></span> <span data-ttu-id="dbd18-262">Grob geschätzt sollen im Zeitraum von Version 5.0 zusätzlich 150 Probleme behoben werden.</span><span class="sxs-lookup"><span data-stu-id="dbd18-262">As a rough estimate we plan to fix an additional 150 issues in the 5.0 time frame.</span></span>

## <a name="small-enhancements"></a><span data-ttu-id="dbd18-263">Kleinere Verbesserungen</span><span class="sxs-lookup"><span data-stu-id="dbd18-263">Small enhancements</span></span>

<span data-ttu-id="dbd18-264">Nachverfolgbar über [Liste der Probleme im 5.0-Meilenstein mit der Bezeichnung `type-enhancement`](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-enhancement+)</span><span class="sxs-lookup"><span data-stu-id="dbd18-264">Tracked by [issues labeled with `type-enhancement` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-enhancement+)</span></span>

<span data-ttu-id="dbd18-265">Entwickler: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd @ajcvickers</span><span class="sxs-lookup"><span data-stu-id="dbd18-265">Developers: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span></span>

<span data-ttu-id="dbd18-266">T-Shirt-Größe: L</span><span class="sxs-lookup"><span data-stu-id="dbd18-266">T-shirt size: L</span></span>

<span data-ttu-id="dbd18-267">Status: In Bearbeitung</span><span class="sxs-lookup"><span data-stu-id="dbd18-267">Status: In-progress</span></span>

<span data-ttu-id="dbd18-268">Zusätzlich zu den oben beschriebenen größeren Funktionen sind in Version 5.0 auch viele kleinere Verbesserungen geplant, mit denen Probleme behoben werden, die als unkritisch gelten.</span><span class="sxs-lookup"><span data-stu-id="dbd18-268">In addition to the bigger features outlined above, we also have many smaller improvements scheduled for 5.0 to fix "paper-cuts".</span></span> <span data-ttu-id="dbd18-269">Viele dieser Verbesserungen sind auch durch die oben beschriebenen, allgemeineren Themen abgedeckt.</span><span class="sxs-lookup"><span data-stu-id="dbd18-269">Note that many of these enhancements are also covered by the more general themes outlined above.</span></span>

## <a name="below-the-line"></a><span data-ttu-id="dbd18-270">Verschoben auf nächste Version</span><span class="sxs-lookup"><span data-stu-id="dbd18-270">Below-the-line</span></span>

<span data-ttu-id="dbd18-271">Nachverfolgbar über [Liste der Probleme im 5.0-Meilenstein mit der Bezeichnung `consider-for-next-release`](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aconsider-for-next-release)</span><span class="sxs-lookup"><span data-stu-id="dbd18-271">Tracked by [issues labeled with `consider-for-next-release`](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aconsider-for-next-release)</span></span>

<span data-ttu-id="dbd18-272">Dabei handelt es sich um Fehlerbehebungen und Verbesserungen, die aktuell **nicht** für das Release 5.0 vorgesehen sind, aber je nach Fortschritt bei den oben beschriebenen Zielen zusätzlich angegangen werden.</span><span class="sxs-lookup"><span data-stu-id="dbd18-272">These are bug fixes and enhancements that are **not** currently scheduled for the 5.0 release, but we will look at as stretch goals depending on the progress made on the work above.</span></span>

<span data-ttu-id="dbd18-273">Zusätzlich werden bei der Planung auch immer die [Probleme mit den meisten Stimmen](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc) berücksichtigt.</span><span class="sxs-lookup"><span data-stu-id="dbd18-273">In addition, we always consider the [most voted issues](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc) when planning.</span></span> <span data-ttu-id="dbd18-274">Bekannte Probleme erst in einem späteren Release zu beheben ist nicht schön, aber nötig, um die Auslastung der vorhandenen Ressourcen auf ein realistisches Maß zu beschränken.</span><span class="sxs-lookup"><span data-stu-id="dbd18-274">Cutting any of these issues from a release is always painful, but we do need a realistic plan for the resources we have.</span></span>

## <a name="feedback"></a><span data-ttu-id="dbd18-275">Feedback</span><span class="sxs-lookup"><span data-stu-id="dbd18-275">Feedback</span></span>

<span data-ttu-id="dbd18-276">Ihr Feedback zur Planung ist wichtig.</span><span class="sxs-lookup"><span data-stu-id="dbd18-276">Your feedback on planning is important.</span></span> <span data-ttu-id="dbd18-277">Sie können für ein Problem auf GitHub abstimmen (Daumen hoch) und so angeben, dass dieses Problem wichtig ist.</span><span class="sxs-lookup"><span data-stu-id="dbd18-277">The best way to indicate the importance of an issue is to vote (thumbs-up) for that issue on GitHub.</span></span> <span data-ttu-id="dbd18-278">Diese Daten werden dann in den [Planungsprozess](../release-planning.md) für das nächste Release aufgenommen.</span><span class="sxs-lookup"><span data-stu-id="dbd18-278">This data will then feed into the [planning process](../release-planning.md) for the next release.</span></span>
