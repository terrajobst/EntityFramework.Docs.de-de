---
title: Behandlung von Fehlern für den Commit Transaction - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 5b1f7a7d-1b24-4645-95ec-5608a31ef577
ms.openlocfilehash: a22a651851bc46e2bf1fe652b3b9a921ec22b70b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996836"
---
# <a name="handling-transaction-commit-failures"></a>Behandeln Fehler bei commit
> [!NOTE]
> **EF6.1 oder höher, nur** -APIs, die Funktionen erläutert, die auf dieser Seite usw. in Entity Framework 6.1 eingeführt wurden. Wenn Sie eine frühere Version verwenden, gelten manche Informationen nicht.  

Als Teil des 6.1 werden wir eine neue verbindungsstabilitätsfeature für EF eingeführt: die Möglichkeit zum Erkennen und automatisch wiederhergestellt, vorübergehenden verbindungsausfällen führen in der Bestätigung der Transaktion ein Commit ausgeführt haben. Die vollständigen Details des Szenarios sind am besten im Blogbeitrag beschrieben [SQL-Datenbankverbindungen und das Problem Idempotenz](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx).  Zusammenfassend lässt sich sagen ist das Szenario an, dass wenn eine Ausnahme, während ein Transaktionscommit ausgelöst wird es zwei mögliche Ursachen gibt:  

1. Fehler bei das Commit der Transaktion auf dem server
2. Das Commit der Transaktion, die auf dem Server war erfolgreich, aber ein Verbindungsproblem verhindert die Benachrichtigung über den Client erreicht  

Wenn die erste Situation der Anwendung oder den Benutzer geschieht, können Sie den Vorgang wiederholen. Wenn jedoch die zweite Situation tritt auf, Wiederholungen sollten vermieden werden und die Anwendung kann automatisch wiederhergestellt. Die Herausforderung besteht, die ohne die Fähigkeit zum erkennen, was die tatsächliche Ursache, die während des Commits, eine Ausnahme gemeldet wurde, die Anwendung nicht die richtige Vorgehensweise wählen kann. Das neue Feature in EF 6.1 ermöglicht EF mit der Datenbank zu überprüfen, ob die Transaktion erfolgreich war, und führen Sie die richtige Vorgehensweise transparent.  

## <a name="using-the-feature"></a>Mithilfe der Funktion  

Damit können das Feature müssen Sie einen Aufruf von einschließen [SetTransactionHandler](https://msdn.microsoft.com/library/system.data.entity.dbconfiguration.setdefaulttransactionhandler.aspx) im Konstruktor der Ihre **"dbconfiguration"**. Wenn Sie keine Erfahrung mit sind **"dbconfiguration"**, finden Sie unter [Code-basierte Konfiguration](~/ef6/fundamentals/configuring/code-based.md). Dieses Feature kann in Kombination mit der automatischen Wiederholungsversuchen, die wir in EF6 eingeführt verwendet werden, die in der Situation dabei helfen, in dem die Transaktion tatsächlich beim commit auf dem Server aufgrund eines vorübergehenden Fehlers Fehler:  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.SqlServer;

public class MyConfiguration : DbConfiguration  
{
  public MyConfiguration()  
  {  
    SetTransactionHandler(SqlProviderServices.ProviderInvariantName, () => new CommitFailureHandler());  
    SetExecutionStrategy(SqlProviderServices.ProviderInvariantName, () => new SqlAzureExecutionStrategy());  
  }  
}
```  

## <a name="how-transactions-are-tracked"></a>Wie werden Transaktionen nachverfolgt.  

Wenn das Feature aktiviert ist, fügen EF eine neue Tabelle für die Datenbank namens **__Transactions**. Eine neue Zeile wird in dieser Tabelle eingefügt, jedes Mal, wenn eine Transaktion von EF erstellt und diese Zeile wird auf Vorhandensein geprüft, ob es sich bei einem Transaktionsfehler während des Commits.  

Obwohl EF ist bemüht sich, löscht Zeilen aus der Tabelle aus, wenn sie nicht mehr benötigt werden, kann die Tabelle vergrößert, wenn die Anwendung beendet wird, vorzeitig und aus diesem Grund müssen Sie möglicherweise die Tabelle manuell in einigen Fällen zu löschen.  

## <a name="how-to-handle-commit-failures-with-previous-versions"></a>Gewusst wie: Behandeln von commitfehlern mit früheren Versionen

Vor dem EF 6.1 war nicht Mechanismus zum Commit-Fehler in der EF-Produkt zu behandeln. Es gibt mehrere Möglichkeiten, die in dieser Situation, die mit früheren Versionen von EF6 angewendet werden kann:  

### <a name="option-1---do-nothing"></a>Option 1: keine Aktion durchführen  

Die Wahrscheinlichkeit für einen Verbindungsfehler während des Transaktionscommits ist gering, sodass es für die Anwendung nur fehlschlägt, wenn diese Bedingung, tatsächlich eintritt gültig sein kann.  

## <a name="option-2---use-the-database-to-reset-state"></a>Option 2: Verwenden Sie die Datenbank, um Status zurückzusetzen  

1. Verwerfen der aktuellen "DbContext"  
2. Erstellen Sie einen neuen "DbContext" und Wiederherstellen Sie des Zustands Ihrer Anwendung aus der Datenbank  
3. Informieren Sie den Benutzer, dass der letzte Vorgang nicht erfolgreich abgeschlossen wurde haben kann  

## <a name="option-3---manually-track-the-transaction"></a>Option 3: die Transaktion manuell nachverfolgen  

1. Fügen Sie eine Tabelle nicht nachverfolgt, mit der Datenbank verwendet, um den Status der Transaktionen nachzuverfolgen.  
2. Eine Zeile in der Tabelle am Anfang jeder Transaktion eingefügt.  
3. Wenn die Verbindung während des Commits fehlschlägt, überprüfen Sie das Vorhandensein der entsprechenden Zeile in der Datenbank.  
    - Wenn die Zeile vorhanden ist, weiterhin normal ausgeführt, da die Transaktion erfolgreich durchgeführt wurde  
    - Wenn die Zeile nicht vorhanden ist, verwenden Sie eine Ausführungsstrategie, um den aktuellen Vorgang wiederholen.  
4. Wenn der Commit erfolgreich ausgeführt wird, löschen Sie die entsprechende Zeile aus, um die Vergrößerung der Tabelle zu vermeiden.  

[In diesem Blogbeitrag](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) enthält Beispielcode, um dies zu erreichen unter SQL Azure.  
