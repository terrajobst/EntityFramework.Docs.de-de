---
title: Protokollierung und Abfangen von Datenbankvorgängen - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: b5ee7eb1-88cc-456e-b53c-c67e24c3f8ca
ms.openlocfilehash: 3f06e073f3ab6e46883663620219e302d5db1d60
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490080"
---
# <a name="logging-and-intercepting-database-operations"></a>Protokollierung und Abfangen von Datenbankvorgängen
> [!NOTE]
> **Nur EF6 und höher:** Die Features, APIs usw., die auf dieser Seite erläutert werden, wurden in Entity Framework 6 eingeführt. Wenn Sie eine frühere Version verwenden, gelten manche Informationen nicht.  

Ab Entity Framework 6, jedes Mal, wenn Entity Framework einem Befehl an der Datenbank mit diesem Befehl sendet, kann vom Anwendungscode abgefangen werden. Dies wird am häufigsten für die Protokollierung von SQL verwendet, aber es kann auch verwendet werden, um zu ändern, oder brechen Sie den Befehl.  

Sie enthält insbesondere EF:  

- Eine Log-Eigenschaft für den Kontext, der ähnlich wie DataContext.Log in LINQ to SQL  
- Ein Mechanismus zum Anpassen der Inhalt und die Formatierung der Ausgabe in das Anwendungsprotokoll gesendet  
- Low-Level-Bausteine für die Abfangfunktion sind so flexibler-Steuerelement  

## <a name="context-log-property"></a>Kontext-Log-Eigenschaft  

In einen Delegaten für eine beliebige Methode, die eine Zeichenfolge akzeptiert, kann die DbContext.Database.Log-Eigenschaft festgelegt werden. Es ist am häufigsten mit jeder TextWriter verwendet, auf die "Write"-Methode von diesem TextWriter festgelegt wird. Alle nach dem aktuellen Kontext generiert SQL werden in der sich der Writer protokolliert. Beispielsweise wird der folgende Code SQL an der Konsole protokolliert:  

``` csharp
using (var context = new BlogContext())
{
    context.Database.Log = Console.Write;

    // Your code here...
}
```  

Beachten Sie, dass dieses Kontexts. Console.Write ist "Database.log" festgelegt. Dies ist alles, die was erforderlich ist, um SQL an der Konsole protokolliert.  

Fügen Sie einige einfachen Code für die Abfrage/einfügen/aktualisieren, damit wir eine Ausgabe angezeigt werden:  

``` csharp
using (var context = new BlogContext())
{
    context.Database.Log = Console.Write;

    var blog = context.Blogs.First(b => b.Title == "One Unicorn");

    blog.Posts.First().Title = "Green Eggs and Ham";

    blog.Posts.Add(new Post { Title = "I do not like them!" });

    context.SaveChangesAsync().Wait();
}
```  

Dadurch wird die folgende Ausgabe generiert:  

``` SQL
SELECT TOP (1)
    [Extent1].[Id] AS [Id],
    [Extent1].[Title] AS [Title]
    FROM [dbo].[Blogs] AS [Extent1]
    WHERE (N'One Unicorn' = [Extent1].[Title]) AND ([Extent1].[Title] IS NOT NULL)
-- Executing at 10/8/2013 10:55:41 AM -07:00
-- Completed in 4 ms with result: SqlDataReader

SELECT
    [Extent1].[Id] AS [Id],
    [Extent1].[Title] AS [Title],
    [Extent1].[BlogId] AS [BlogId]
    FROM [dbo].[Posts] AS [Extent1]
    WHERE [Extent1].[BlogId] = @EntityKeyValue1
-- EntityKeyValue1: '1' (Type = Int32)
-- Executing at 10/8/2013 10:55:41 AM -07:00
-- Completed in 2 ms with result: SqlDataReader

UPDATE [dbo].[Posts]
SET [Title] = @0
WHERE ([Id] = @1)
-- @0: 'Green Eggs and Ham' (Type = String, Size = -1)
-- @1: '1' (Type = Int32)
-- Executing asynchronously at 10/8/2013 10:55:41 AM -07:00
-- Completed in 12 ms with result: 1

INSERT [dbo].[Posts]([Title], [BlogId])
VALUES (@0, @1)
SELECT [Id]
FROM [dbo].[Posts]
WHERE @@ROWCOUNT > 0 AND [Id] = scope_identity()
-- @0: 'I do not like them!' (Type = String, Size = -1)
-- @1: '1' (Type = Int32)
-- Executing asynchronously at 10/8/2013 10:55:41 AM -07:00
-- Completed in 2 ms with result: SqlDataReader
```  

(Beachten Sie, dass dies ist die Ausgabe, vorausgesetzt, dass es sich bei jeder Initialisierung der Datenbank bereits geschehen ist. Wenn Datenbankinitialisierung nicht bereits geschehen hatte, und klicken Sie dann wäre hat noch viel mehr Ausgabe, die mit der Arbeit Migrationen im Hintergrund zu überprüfen oder eine neue Datenbank erstellen.)  

## <a name="what-gets-logged"></a>Ruft protokolliert?  

Wenn der Log-Eigenschaft alle folgenden festgelegt ist, wird die werden protokolliert:  

- "SQL" für alle anderen Arten von Befehlen. Zum Beispiel:  
    - Abfragen, einschließlich der normalen LINQ-Abfragen, eSQL-Abfragen und unformatierte Abfragen von Methoden wie z. B. SqlQuery  
    - Einfügungen, Updates und löschungen, die als Teil von "SaveChanges" generiert  
    - Laden von Abfragen wie die von lazy Loading generierten Beziehung  
- Parameter  
- Unabhängig davon, ob der Befehl asynchron ausgeführt wird  
- Ein Zeitstempel, der angibt, wenn der Befehl gestartet ausführen  
- Unabhängig davon, ob der Befehl erfolgreich abgeschlossen, Fehler durch Auslösen einer Ausnahme oder, für die asynchrone, wurde abgebrochen.  
- Ein Hinweis auf den Ergebniswert  
- Die ungefähre Dauer der Zeit zum Ausführen des Befehls dauert. Beachten Sie, dass dies die Zeit vom Senden des Befehls an das Ergebnisobjekt abrufen. Es umfasst nicht die Zeit, die Ergebnisse zu lesen.  

Betrachten die Beispielausgabe oben aus, die vier Befehle, die protokolliert werden:  

- Die Abfrage, die durch den Aufruf von Kontext. Blogs.First  
    - Beachten Sie, dass die ToString-Methode dar, die SQL-Anweisung nicht für diese Abfrage seit gearbeitet haben, würde "First" kein "IQueryable" auf dem ToString aufgerufen werden kann  
- Die Abfrage aus der Lazy-Blog. Beiträge  
    - Beachten Sie, dass die Details zu den Parametern für den Schlüsselwert für die Lazy Load geschieht.  
    - Nur Eigenschaften des Parameters, die auf nicht standardmäßige Werte festgelegt sind, werden protokolliert. Beispielsweise wird die Size-Eigenschaft wird nur angezeigt, wenn es ungleich NULL ist.  
- Zwei Befehle, die sich aus SaveChangesAsync ergeben; eine für die Aktualisierung so ändern Sie einen Post-Titel, die andere für eine Einfügung, einen neuen Beitrag hinzufügen  
    - Beachten Sie, dass die Details zu den Parametern für den Fremdschlüssel "und" Title "  
    - Beachten Sie, dass diese Befehle asynchron ausgeführt werden  

## <a name="logging-to-different-places"></a>Protokollierung in verschiedenen Speicherorten  

Wie oben Protokollierung wird die Konsole sehr leicht. Es ist auch einfach, melden Sie sich mit verschiedenen Arten, Arbeitsspeicher, Datei usw. der [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx).  

Wenn Sie mit LINQ to SQL vertraut sind, die Ihnen, die in LINQ to SQL auffallen, die Log-Eigenschaft auf die tatsächliche TextWriter-Objekt (z. B. Console.Out) beim in EF festgelegt ist, die an eine Methode, die eine Zeichenfolge (z. B. akzeptiert, die Log-Eigenschaft festgelegt ist Console.Write "oder" Console.Out.Write). Der Grund dafür ist, entkoppeln EF über TextWriter durch das Akzeptieren von jeder Delegat, der als Senke für Zeichenfolgen verwendet werden kann. Beispiel: Angenommen, dass Sie bereits einige protokollierungsframework und eine Protokollierungsmethode definiert wie folgt:  

``` csharp
public class MyLogger
{
    public void Log(string component, string message)
    {
        Console.WriteLine("Component: {0} Message: {1} ", component, message);
    }
}
```  

Dies kann auf die EF-Log-Eigenschaft wie folgt verknüpft werden:  

``` csharp
var logger = new MyLogger();
context.Database.Log = s => logger.Log("EFApp", s);
```  

## <a name="result-logging"></a>Führen Sie die Protokollierung  

Die Protokollierung protokolliert Befehlstext (SQL), Parameter und die Zeile "Wird ausgeführt" mit einem Zeitstempel, bevor der Befehl in die Datenbank gesendet wird. "Abgeschlossene" Linie, die verstrichene Zeit ist, protokolliert der folgenden Ausführung des Befehls.  

Beachten Sie, dass für die asynchrone Befehle die "Abgeschlossene" Zeile nicht angemeldet ist, bis die asynchrone Aufgabe tatsächlich abgeschlossen ist, fehlschlägt oder abgebrochen wird.  

Die Zeile "Abgeschlossene" enthält verschiedene Informationen je nach Art des-Befehls sowie davon, ob die Ausführung erfolgreich war.  

### <a name="successful-execution"></a>Die erfolgreiche Ausführung  

Ist für Befehle, die die Ausgabe erfolgreich abgeschlossen "abgeschlossen in x-ms mit Ergebnis:" gefolgt von einigen Überblick darüber, wie das Ergebnis lautet. Für Befehle, die einen Datenreader das Ergebnis zurückzugeben, ist der Typ des [DbDataReader](https://msdn.microsoft.com/library/system.data.common.dbdatareader.aspx) zurückgegeben. Für Befehle, die einen Ganzzahlwert wie das Update zurückgeben ist Befehl das Ergebnis sehen Sie oben, ganze Zahl.  

### <a name="failed-execution"></a>Fehler bei der Ausführung  

Für Befehle, die durch Auslösen einer Ausnahme fehlschlägt, enthält die Ausgabe die Nachricht aus der Ausnahme. Beispielsweise wird Abfrage für eine Tabelle, die vorhanden ist, SqlQuery mit Ergebnis im Anwendungsprotokoll etwa wie folgt ausgegeben:  

``` SQL
SELECT * from ThisTableIsMissing
-- Executing at 5/13/2013 10:19:05 AM
-- Failed in 1 ms with error: Invalid object name 'ThisTableIsMissing'.
```  

### <a name="canceled-execution"></a>Abgebrochene Ausführung  

Für asynchrone Befehle, in dem die Aufgabe abgebrochen wird, das Ergebnis möglicherweise Fehler mit einer Ausnahme, da es sich handelt, was der zugrunde liegenden ADO.NET-Anbieter selten der Fall bei dem Versuch zum Abbrechen. Wenn dies nicht möglich, und die Aufgabe, ordnungsgemäß abgebrochen wird wird die Ausgabe etwa wie folgt aussehen:  

```  
update Blogs set Title = 'No' where Id = -1
-- Executing asynchronously at 5/13/2013 10:21:10 AM
-- Canceled in 1 ms
```  

## <a name="changing-log-content-and-formatting"></a>Ändern des Inhalts und Formatierung  

Hinter den Kulissen die Eigenschaft "Database.log" Verwenden eines DatabaseLogFormatter-Objekts. Dieses Objekt bindet effektiv eine IDbCommandInterceptor-Implementierung (siehe unten) an einen Delegaten, der Zeichenfolgen und einen "DbContext" akzeptiert. Dies bedeutet, dass Methoden auf DatabaseLogFormatter vor und nach der Ausführung von Befehlen von EF aufgerufen werden. Diese Methoden DatabaseLogFormatter erfassen und formatieren die Protokollausgabe und an den Delegaten zu senden.  

### <a name="customizing-databaselogformatter"></a>Anpassen von DatabaseLogFormatter  

Protokolliert werden soll, und die Art der Formatierung ändern kann durch Erstellen einer neuen Klasse, die DatabaseLogFormatter abgeleitet und überschreibt die Methoden entsprechend erreicht werden. Am häufigsten verwendeten Methoden überschrieben werden:  

- LogCommand – überschreiben Sie diese Option, um die ändern, wie Befehle protokolliert werden, bevor sie ausgeführt werden. Standardmäßig ruft LogCommand LogParameter für jeden Parameter. Sie können auswählen, in der Außerkraftsetzung oder Parameter stattdessen unterschiedlich behandeln.  
- LogResult – überschreiben Sie diese Option, um Sie ändern möchten, wie das Ergebnis aus der Ausführung eines Befehls protokolliert wird.  
- LogParameter – überschreiben Sie diese Option, um die Formatierung und den Inhalt der Parameter-Protokollierung zu ändern.  

Nehmen wir beispielsweise an, dass wir nur eine einzelne Zeile anmelden, bevor jeder Befehl in die Datenbank gesendet wird. Dies kann mit zwei Außerkraftsetzungen erfolgen:  

- Überschreiben Sie LogCommand zum Formatieren und die einzelne SQL-Codezeile zu schreiben  
- Überschreiben Sie LogResult, um den Vorgang.  

Der Code würde in etwa wie folgt aussehen:

``` csharp
public class OneLineFormatter : DatabaseLogFormatter
{
    public OneLineFormatter(DbContext context, Action<string> writeAction)
        : base(context, writeAction)
    {
    }

    public override void LogCommand<TResult>(
        DbCommand command, DbCommandInterceptionContext<TResult> interceptionContext)
    {
        Write(string.Format(
            "Context '{0}' is executing command '{1}'{2}",
            Context.GetType().Name,
            command.CommandText.Replace(Environment.NewLine, ""),
            Environment.NewLine));
    }

    public override void LogResult<TResult>(
        DbCommand command, DbCommandInterceptionContext<TResult> interceptionContext)
    {
    }
}
```  

Zum Protokollieren Ausgabe rufen einfach die Write-Methode, die Ausgabe an den Delegaten konfigurierten schreiben gesendet wird.  

(Beachten Sie, dass dieser Code einfach zum Entfernen von Zeilenumbrüchen nur als Beispiel bewirkt. Sie wahrscheinlich funktioniert nicht gut für komplexe SQL anzeigen.)  

### <a name="setting-the-databaselogformatter"></a>Festlegen der DatabaseLogFormatter  

Sobald eine neue DatabaseLogFormatter-Klasse, es erstellt wurde muss mit EF registriert werden. Dies erfolgt mithilfe von codebasierte Konfiguration. Kurz gesagt bedeutet dies eine neue Klasse erstellen, die "dbconfiguration" in der gleichen Assembly wie die Klasse "DbContext" abgeleitet und dem anschließenden Aufrufen der SetDatabaseLogFormatter im Konstruktor der diese neue Klasse. Zum Beispiel:  

``` csharp
public class MyDbConfiguration : DbConfiguration
{
    public MyDbConfiguration()
    {
        SetDatabaseLogFormatter(
            (context, writeAction) => new OneLineFormatter(context, writeAction));
    }
}
```  

### <a name="using-the-new-databaselogformatter"></a>Verwenden die neue DatabaseLogFormatter  

Diese neue DatabaseLogFormatter wird jetzt verwendet werden, jedes Mal, wenn "Database.log" festgelegt ist. Daher führt das Ausführen des Codes aus Teil 1 jetzt in der folgenden Ausgabe:  

```  
Context 'BlogContext' is executing command 'SELECT TOP (1) [Extent1].[Id] AS [Id], [Extent1].[Title] AS [Title]FROM [dbo].[Blogs] AS [Extent1]WHERE (N'One Unicorn' = [Extent1].[Title]) AND ([Extent1].[Title] IS NOT NULL)'
Context 'BlogContext' is executing command 'SELECT [Extent1].[Id] AS [Id], [Extent1].[Title] AS [Title], [Extent1].[BlogId] AS [BlogId]FROM [dbo].[Posts] AS [Extent1]WHERE [Extent1].[BlogId] = @EntityKeyValue1'
Context 'BlogContext' is executing command 'update [dbo].[Posts]set [Title] = @0where ([Id] = @1)'
Context 'BlogContext' is executing command 'insert [dbo].[Posts]([Title], [BlogId])values (@0, @1)select [Id]from [dbo].[Posts]where @@rowcount > 0 and [Id] = scope_identity()'
```  

## <a name="interception-building-blocks"></a>Bausteine für die Abfangfunktion  

Bisher haben wir an, wie DbContext.Database.Log zu verwenden, um das Protokollieren von EF generierten SQL erläutert. Doch dieser Code ist tatsächlich eine relativ schlanke Fassade, über einige spezielle Bausteine für allgemeinere abfangen.  

### <a name="interception-interfaces"></a>Die Abfangfunktion-Schnittstellen  

Der Abfangfunktion-Code wird das Konzept der Abfangfunktion Schnittstellen erstellt. Diese Schnittstellen erben von IDbInterceptor und definieren Sie Methoden, die aufgerufen werden, wenn EF eine Aktion ausführt. Die Absicht besteht darin, eine Schnittstelle pro Typ des abgefangenen Objekts verfügen. Beispielsweise definiert die Schnittstelle IDbCommandInterceptor Methoden, die aufgerufen werden, bevor EF ExecuteNonQuery, "ExecuteScalar", "ExecuteReader" und zugehörigen Methoden aufruft. Ebenso definiert die Schnittstelle Methoden, die aufgerufen werden, wenn es sich bei diesen Vorgängen abgeschlossen ist. Die DatabaseLogFormatter-Klasse, der wir uns weiter oben angesehen haben implementiert diese Schnittstelle, um Befehle zu protokollieren.  

### <a name="the-interception-context"></a>Der Kontext für die Abfangfunktion  

Betrachten die Methoden, die für jede der Schnittstellen Interceptor definiert es ist offensichtlich, dass für jeden Aufruf wie z. B. DbCommandInterceptionContext angegebenen ein Objekt des Typs DbInterceptionContext oder eine Art von dieser abgeleitet ist\<\>. Dieses Objekt enthält Kontextinformationen über die Aktion, die EF ausgeführt wird. Z. B. wenn die Aktion für einen "DbContext" aufgenommen wird, ist dann "DbContext" in der DbInterceptionContext enthalten. Auf ähnliche Weise ist für Befehle, die asynchron ausgeführt werden, der IsAsync-Flag auf DbCommandInterceptionContext festgelegt.  

### <a name="result-handling"></a>Führen Sie die Verarbeitung  

Die DbCommandInterceptionContext\< \> Klasse enthält Eigenschaften, die aufgerufen wird, Ergebnis, OriginalResult, Ausnahme und OriginalException. Diese Eigenschaften werden festgelegt auf Null/NULL für Aufrufe an den Interceptor-Methoden, die aufgerufen werden, bevor der Vorgang ausgeführt wird, d. h. für die... Ausführen von Methoden. Wenn der Vorgang ausgeführt wird und erfolgreich ist, werden Ergebnis und OriginalResult auf das Ergebnis des Vorgangs festgelegt. Diese Werte können dann beobachtet werden, in der Interceptor-Methoden, die aufgerufen werden, nachdem der Vorgang ausgeführt wurde, d. h. auf die... Ausgeführte Methoden. Ebenso, wenn der Vorgang ausgelöst wird, werden klicken Sie dann die Ausnahme und OriginalException Eigenschaften festgelegt werden.  

#### <a name="suppressing-execution"></a>Unterdrücken der Ausführung  

Ein Interceptor die Result-Eigenschaft festgelegt, bevor der Befehl ausgeführt hat (eines der... Ausführen von Methoden) klicken Sie dann EF versucht nicht, um den Befehl tatsächlich auszuführen, aber das Resultset wird stattdessen nur verwenden. Das heißt, kann der Interceptor unterdrücken Ausführung des Befehls, aber das vorhandenem EF fortgesetzt, als wäre, wenn der Befehl ausgeführt wurde, hatte.  

Verdeutlicht, wie diese verwendet werden kann, ist, dass der Befehl, Batchverarbeitung, die normalerweise mit einem Wrapper-Anbieter erfolgt ist. Der Interceptor als Batch den Befehl für die spätere Ausführung speichern, aber es würde "nehmen" mit EF, der der Befehl wie gewohnt ausgeführt worden. Beachten Sie, dass sie mehr zum Implementieren der Batchverarbeitung erfordert, aber dies ist ein Beispiel, wie die Änderung des Ergebnis der Abfangfunktion verwendet werden kann.  

Ausführung kann auch durch Festlegen der Exception-Eigenschaft in einem der unterdrückt werden die... Ausführen von Methoden. Dies bewirkt, dass EF fortgesetzt, als wäre die Ausführung des Vorgangs Fehler hatte, durch die angegebene Ausnahme auslösen. Kann dies natürlich, dazu führen, dass die Anwendung zum Absturz bringen, aber es kann auch sein, eine vorübergehende Ausnahme oder sonstige Ausnahmen, die von EF behandelt wird. Beispielsweise könnte dies in testumgebungen verwendet werden, testen das Verhalten einer Anwendung aus, wenn es sich bei Ausführung des Befehls ein Fehler auftritt.  

#### <a name="changing-the-result-after-execution"></a>Ändern das Ergebnis nach der Ausführung  

Wenn ein Interceptor die Result-Eigenschaft festlegt, nachdem der Befehl ausgeführt hat (eines der... Ausgeführt von Methoden) und dann EF das geänderte Ergebnis das Ergebnis verwendet wird, die tatsächlich vom Vorgang zurückgegeben wurde. Auf ähnliche Weise, wenn ein Interceptor die Exception-Eigenschaft festlegt, nachdem der Befehl ausgeführt wurde, EF die Set-Ausnahme löst, als ob der Vorgang die Ausnahme ausgelöst haben.  

Ein Interceptor kann auch die Exception-Eigenschaft festlegen, auf null, um anzugeben, dass keine Ausnahme ausgelöst werden soll. Dies kann nützlich sein, wenn Fehler bei der Ausführung des Vorgangs, aber der Interceptor wünscht, dass EF fortgesetzt, als wäre der Vorgang erfolgreich war. Dies umfasst in der Regel auch das Ergebnis festlegen, sodass EF einige Ergebniswert hat mit arbeiten, da er weiterhin.  

#### <a name="originalresult-and-originalexception"></a>OriginalResult und OriginalException  

Nachdem EF einen Vorgang ausgeführt wurde wird es entweder das Ergebnis und OriginalResult-Eigenschaften, wenn die Ausführung nicht fehlgeschlagen ist oder die Ausnahme und OriginalException Eigenschaften festgelegt, wenn Fehler bei der Ausführung mit einer Ausnahme.  

Die OriginalResult und OriginalException-Eigenschaften sind schreibgeschützt und nur von EF nach dem tatsächlich Ausführen eines Vorgangs festgelegt. Diese Eigenschaften können nicht von Interceptors festgelegt werden. Dies bedeutet, dass alle Interceptor unterscheiden kann eine Ausnahme oder ein Ergebnis, das festgelegt wurde, indem einige andere Interceptor im Gegensatz zu der echte Ausnahme oder ein Ergebnis, die aufgetreten sind, wenn der Vorgang ausgeführt wurde.  

### <a name="registering-interceptors"></a>Registrieren des interceptors  

Nach der Erstellung einer Klasse, die eine oder mehrere der Abfangfunktion Schnittstellen implementiert, können sie mit EF unter Verwendung der DbInterception-Klasse registriert werden. Zum Beispiel:  

``` csharp
DbInterception.Add(new NLogCommandInterceptor());
```  

Interceptors können auch auf app-Domäne mithilfe der Mechanismen zur "dbconfiguration" codebasierte Konfiguration registriert werden.  

### <a name="example-logging-to-nlog"></a>Beispiel: Protokollierung NLog  

Können wir all dies zusammen in einem Beispiel, dass die Verwendung IDbCommandInterceptor und [NLog](http://nlog-project.org/) auf:  

- Melden Sie sich eine Warnung für solche Befehle, die nicht asynchron ausgeführt wird  
- Melden Sie Fehler für jeden Befehl, der auslöst, wenn ausgeführt  

Dies ist die Klasse, die die Protokollierung, die registriert werden soll, wie oben gezeigt:  

``` csharp
public class NLogCommandInterceptor : IDbCommandInterceptor
{
    private static readonly Logger Logger = LogManager.GetCurrentClassLogger();

    public void NonQueryExecuting(
        DbCommand command, DbCommandInterceptionContext<int> interceptionContext)
    {
        LogIfNonAsync(command, interceptionContext);
    }

    public void NonQueryExecuted(
        DbCommand command, DbCommandInterceptionContext<int> interceptionContext)
    {
        LogIfError(command, interceptionContext);
    }

    public void ReaderExecuting(
        DbCommand command, DbCommandInterceptionContext<DbDataReader> interceptionContext)
    {
        LogIfNonAsync(command, interceptionContext);
    }

    public void ReaderExecuted(
        DbCommand command, DbCommandInterceptionContext<DbDataReader> interceptionContext)
    {
        LogIfError(command, interceptionContext);
    }

    public void ScalarExecuting(
        DbCommand command, DbCommandInterceptionContext<object> interceptionContext)
    {
        LogIfNonAsync(command, interceptionContext);
    }

    public void ScalarExecuted(
        DbCommand command, DbCommandInterceptionContext<object> interceptionContext)
    {
        LogIfError(command, interceptionContext);
    }

    private void LogIfNonAsync<TResult>(
        DbCommand command, DbCommandInterceptionContext<TResult> interceptionContext)
    {
        if (!interceptionContext.IsAsync)
        {
            Logger.Warn("Non-async command used: {0}", command.CommandText);
        }
    }

    private void LogIfError<TResult>(
        DbCommand command, DbCommandInterceptionContext<TResult> interceptionContext)
    {
        if (interceptionContext.Exception != null)
        {
            Logger.Error("Command {0} failed with exception {1}",
                command.CommandText, interceptionContext.Exception);
        }
    }
}
```  

Beachten Sie, wie dieser Code abfangen Kontext ermitteln, wenn ein Befehl nicht asynchron ausgeführt wird und erkannt wird, wenn Fehler bei der Ausführung eines Befehls verwendet.  
