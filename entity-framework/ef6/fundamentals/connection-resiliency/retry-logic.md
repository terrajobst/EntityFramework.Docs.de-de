---
title: Datenbankverbindungsresilienz und Wiederholungslogik Verbindungslogik - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 47d68ac1-927e-4842-ab8c-ed8c8698dff2
ms.openlocfilehash: 47181292873009c7bce2047787503258ffa35d9d
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997484"
---
# <a name="connection-resiliency-and-retry-logic"></a>Verbindungslogik datenbankverbindungsresilienz und Wiederholungslogik
> [!NOTE]
> **Nur EF6 und höher:** Die Features, APIs usw., die auf dieser Seite erläutert werden, wurden in Entity Framework 6 eingeführt. Wenn Sie eine frühere Version verwenden, gelten manche Informationen nicht.  

Anwendungen, die mit einem Datenbankserver verbinden wurden immer anfällig für Unterbrechungen der Verbindung aufgrund von Back-End-Fehlern und netzwerkinstabilität. Allerdings sind diese Fehler in einer LAN-basierte Umgebung arbeiten mit dedizierten Datenbankserver selten genug, dass zusätzliche Logik zur Behandlung dieser Fehler häufig nicht erforderlich ist. Mit dem Aufstieg des Cloud-basierter Datenbankserver, z. B. Windows Azure SQL-Datenbank und Verbindungen über weniger zuverlässige Netzwerke ist es nun häufiger bei Unterbrechungen der Verbindung auftreten. Dies kann aufgrund von Verteidigungsmaßnahmen sein, die cloud-Datenbanken verwenden, um sicherzustellen, dass die Ausgewogenheit des Diensts, z. B. verbindungsdrosselung oder Instabilität im Netzwerk verursachen vorübergehende Timeouts und andere vorübergehende Fehler.  

Verbindungsresilienz bezieht sich auf die Möglichkeit, dass EF automatisch alle Befehle wiederholt, die fehl, weil diese Verbindung unterbrochen.  

## <a name="execution-strategies"></a>Ausführungsstrategien  

Herstellen einer erneuten Verbindung wird durch eine Implementierung der Schnittstelle IDbExecutionStrategy übernommen. Implementierungen der IDbExecutionStrategy werden verantwortlich für das Akzeptieren eines Vorgangs und, wenn eine Ausnahme auftritt, bestimmen, ob eine Wiederholung geeignet ist, und wiederholen, wenn es sich handelt. Es gibt vier Ausführungsstrategien aus dem Lieferumfang von EF:  

1. **DefaultExecutionStrategy**: Diese Ausführungsstrategie wird nicht erneut versucht, alle Vorgänge ist dies die Standardeinstellung für die Datenbanken als SqlServer.  
2. **DefaultSqlExecutionStrategy**: Dies ist eine interne Ausführung-Strategie, die standardmäßig verwendet wird. Diese Strategie wird nicht erneut versucht, alle, bricht jedoch alle Ausnahmen, die möglicherweise vorübergehende, um Benutzer zu informieren, die diese resilienz von Verbindungen aktivieren möchten.  
3. **DbExecutionStrategy**: Diese Klasse als Basisklasse für andere Ausführungsstrategien, einschließlich Ihrer eigenen benutzerdefinierten geeignet ist. Sie implementiert eine exponentielle wiederholungsrichtlinie, in dem der erste Wiederholungsversuch mit 0 (null) Verzögerung und die Verzögerung steigt exponentiell erfolgt bis die maximale Anzahl von Wiederholungsversuchen erreicht ist. Diese Klasse verfügt über eine abstrakte ShouldRetryOn-Methode, die implementiert werden kann, in der abgeleiteten Ausführungsstrategien steuern, welche Ausnahmen wiederholt werden soll.  
4. **"Sqlazureexecutionstrategy"**: Diese Ausführungsstrategie von DbExecutionStrategy erbt, und wiederholt auf Ausnahmen, die bekannt ist, dass beim Arbeiten mit Azure SQL-Datenbank möglicherweise vorübergehend sein.

> [!NOTE]
> Ausführungsstrategien 2 und 4 in der Sql Server-Anbieter, der Lieferumfang von EF, handelt es sich in der Assembly EntityFramework.SqlServer enthalten sind, und dienen zum Arbeiten mit SQL Server.  

## <a name="enabling-an-execution-strategy"></a>Aktivieren eine Ausführungsstrategie  

Die einfachste Möglichkeit zum Teilen von EF verwenden, eine Ausführungsstrategie für die wird mit der SetExecutionStrategy-Methode, der die ["dbconfiguration"](~/ef6/fundamentals/configuring/code-based.md) Klasse:  

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
    }
}
```  

Dieser Code weist EF die "sqlazureexecutionstrategy" beim Verbinden mit SQL Server zu verwenden.  

## <a name="configuring-the-execution-strategy"></a>Konfigurieren die Ausführungsstrategie  

Der Konstruktor der "sqlazureexecutionstrategy" kann zwei Parameter: "maxretrycount" und MaxDelay akzeptieren. MaxRetry-Anzahl ist die maximale Anzahl von Wiederholungen, die die Strategie wiederholt wird. Die MaxDelay ist ein TimeSpan-Objekt, das die maximale Verzögerung zwischen Wiederholungen, die die Ausführungsstrategie verwenden darstellt.  

Um die maximale Anzahl von Wiederholungen auf 1 und die maximale Verzögerung beträgt 30 Sekunden festlegen sollen Execue Folgendes:  

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetExecutionStrategy(
            "System.Data.SqlClient",
            () => new SqlAzureExecutionStrategy(1, TimeSpan.FromSeconds(30)));
    }
}
```  

Die "sqlazureexecutionstrategy" Vorgang wird wiederholt, sofort beim ersten ein vorübergehender Fehler auftritt, aber mehr zwischen den einzelnen Wiederholungsversuchen, bis die maximale Anzahl Wiederholungslimit verzögert überschritten wird, oder die Gesamtzeit, die maximale Verzögerung erreicht.  

Die Ausführungsstrategien werden nur eine begrenzte Anzahl von Ausnahmen, die in der Regel Tansient wiederholen, müssen Sie weiterhin, behandeln andere Fehler sowie das Abfangen der Ausnahme RetryLimitExceeded für den Fall, in denen ein Fehler ist nicht vorübergehend oder dauert zu lange, Auflösen sich selbst.  

## <a name="limitations"></a>Einschränkungen  

Es gibt einige bekannte Einschränkungen auf, wenn eine Wiederholung Ausführungsstrategie verwenden:  

### <a name="streaming-queries-are-not-supported"></a>Streamingabfragen werden nicht unterstützt.  

Standardmäßig werden EF 6 und höher Abfrageergebnisse, anstatt sie streaming Puffern. Wenn Sie möchten Ergebnisse gestreamt, Sie können die AsStreaming-Methode um eine LINQ to Entities-Abfrage, um streaming zu ändern.  

``` csharp
using (var db = new BloggingContext())
{
    var query = (from b in db.Blogs
                orderby b.Url
                select b).AsStreaming();
    }
}
```  

Streaming wird nicht unterstützt, wenn eine Wiederholung Ausführungsstrategie registriert wird. Diese Einschränkung ist vorhanden, da die Verbindung Teil Weg durch die zurückgegebenen Ergebnisse löschen. In diesem Fall EF benötigt die gesamte Abfrage erneut ausführen, aber keine zuverlässige Möglichkeit, zu wissen, welche Ergebnisse zurückgegeben wurden (die Daten möglicherweise wurden geändert, da die ursprüngliche Abfrage gesendet wurde, Ergebnisse in einer anderen Reihenfolge zurückkehren können, Ergebnisse möglicherweise keinen eindeutigen Bezeichner usw..).  

### <a name="user-initiated-transactions-not-supported"></a>Vom Benutzer initiierte Transaktionen nicht unterstützt.  

Wenn Sie eine Ausführungsstrategie, die zu Wiederholungen konfiguriert haben, gibt es einige Einschränkungen für die Verwendung von Transaktionen.  

#### <a name="whats-supported-efs-default-transaction-behavior"></a>Was unterstützt wird: Transaktion-Standardverhalten für die von EF  

Standardmäßig wird EF datenbankaktualisierungen innerhalb einer Transaktion ausgeführt. Sie müssen nichts tun, um dies zu ermöglichen, EF immer führt dies automatisch aus.  

Beispielsweise wird im folgenden Code "SaveChanges" automatisch innerhalb einer Transaktion ausgeführt. Wenn "SaveChanges" schlagen fehl, nachdem eines der neuen Website einfügen, und klicken Sie dann die Transaktion ein Rollback ausgeführt werden sollen und keine Änderungen auf die Datenbank angewendet wurden. Der Kontext bleibt auch in einem Zustand, die "SaveChanges", um wiederholen, Anwenden der Änderungen erneut aufgerufen werden können.  

``` csharp
using (var db = new BloggingContext())
{
    db.Blogs.Add(new Site { Url = "http://msdn.com/data/ef" });
    db.Blogs.Add(new Site { Url = "http://blogs.msdn.com/adonet" });
    db.SaveChanges();
}
```  

#### <a name="whats-not-supported-user-initiated-transactions"></a>Was nicht unterstützt wird: vom Benutzer initiierte Transaktionen  

Wenn Sie keine Wiederholung Ausführungsstrategie verwenden, können Sie mehrere Vorgänge in einer einzelnen Transaktion umschließen. Beispielsweise umschließt der folgende Code zwei "SaveChanges" aufrufen, in einer einzelnen Transaktion. Wenn keine der Änderungen klicken Sie dann einen beliebigen Teil entweder Vorgang fehlschlägt, werden angewendet.  

``` csharp
using (var db = new BloggingContext())
{
    using (var trn = db.Database.BeginTransaction())
    {
        db.Blogs.Add(new Site { Url = "http://msdn.com/data/ef" });
        db.Blogs.Add(new Site { Url = "http://blogs.msdn.com/adonet" });
        db.SaveChanges();

        db.Blogs.Add(new Site { Url = "http://twitter.com/efmagicunicorns" });
        db.SaveChanges();

        trn.Commit();
    }
}
```  

Dies wird nicht unterstützt, wenn eine Wiederholung Ausführungsstrategie verwenden, da EF über aller vorherigen Vorgänge und die Vorgehensweise beim Wiederholen Sie diese nicht. Beispielsweise, wenn die zweite "SaveChanges" EF nicht mehr danach nicht hat die erforderliche Informationen zum Wiederholen des ersten Aufrufs von "SaveChanges".  

#### <a name="possible-workarounds"></a>Mögliche problemumgehungen  

##### <a name="suspend-execution-strategy"></a>Ausführungsstrategie anhalten  

Eine mögliche Lösung besteht, die wiederholt Ausführungsstrategie für den Codeabschnitt anzuhalten, die ein Benutzer verwenden, muss die Transaktion initiiert. Die einfachste Möglichkeit hierzu ist ein SuspendExecutionStrategy-Flag, um Ihren Code basierte Configuration-Klasse und ändern die Ausführung Strategie Lambda, um die Standardeinstellung (nicht retying) Ausführungsstrategie zurückzugeben, wenn das Flag festgelegt ist.  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.SqlServer;
using System.Runtime.Remoting.Messaging;

namespace Demo
{
    public class MyConfiguration : DbConfiguration
    {
        public MyConfiguration()
        {
            this.SetExecutionStrategy("System.Data.SqlClient", () => SuspendExecutionStrategy
              ? (IDbExecutionStrategy)new DefaultExecutionStrategy()
              : new SqlAzureExecutionStrategy());
        }

        public static bool SuspendExecutionStrategy
        {
            get
            {
                return (bool?)CallContext.LogicalGetData("SuspendExecutionStrategy")  false;
            }
            set
            {
                CallContext.LogicalSetData("SuspendExecutionStrategy", value);
            }
        }
    }
}
```  

Beachten Sie, dass wir CallContext verwenden, um den Flagwert zu speichern. Dies bietet ähnliche Funktionen wie der threadlokale Speicher ist jedoch problemlos mit asynchronem Code – einschließlich Async-Abfrage verwenden, und speichern Sie mit Entity Framework.  

Wir können jetzt die Ausführungsstrategie für den Codeabschnitt anhalten, die eine vom Benutzer initiierten Transaktion verwendet.  

``` csharp
using (var db = new BloggingContext())
{
    MyConfiguration.SuspendExecutionStrategy = true;

    using (var trn = db.Database.BeginTransaction())
    {
        db.Blogs.Add(new Blog { Url = "http://msdn.com/data/ef" });
        db.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
        db.SaveChanges();

        db.Blogs.Add(new Blog { Url = "http://twitter.com/efmagicunicorns" });
        db.SaveChanges();

        trn.Commit();
    }

    MyConfiguration.SuspendExecutionStrategy = false;
}
```  

##### <a name="manually-call-execution-strategy"></a>Rufen Sie die Ausführungsstrategie manuell  

Eine weitere Möglichkeit ist die Ausführungsstrategie verwenden manuell, und geben sie den gesamten Satz von Logik ausgeführt werden, damit sie alles, was wiederholt werden kann, wenn ein Vorgang fehlschlägt. Wir benötigen die Ausführungsstrategie - mithilfe der Technik anhalten oberhalb - angezeigt wird, sodass alle Kontexte, in der wiederholbare Codeblock verwendet nicht versuchen, wiederholen Sie dann.  

Beachten Sie, dass Kontexte erstellt werden soll, in den Codeblock wiederholt werden. Dadurch wird sichergestellt, dass wir mit einem sauberen Status für jeden Wiederholungsversuch gestartet werden.  

``` csharp
var executionStrategy = new SqlAzureExecutionStrategy();

MyConfiguration.SuspendExecutionStrategy = true;

executionStrategy.Execute(
    () =>
    {
        using (var db = new BloggingContext())
        {
            using (var trn = db.Database.BeginTransaction())
            {
                db.Blogs.Add(new Blog { Url = "http://msdn.com/data/ef" });
                db.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
                db.SaveChanges();

                db.Blogs.Add(new Blog { Url = "http://twitter.com/efmagicunicorns" });
                db.SaveChanges();

                trn.Commit();
            }
        }
    });

MyConfiguration.SuspendExecutionStrategy = false;
```  
