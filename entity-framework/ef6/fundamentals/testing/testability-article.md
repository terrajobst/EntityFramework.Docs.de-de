---
title: Prüfbarkeit und Entitätsframework 4.0
author: divega
ms.date: 10/23/2016
ms.assetid: 9430e2ab-261c-4e8e-8545-2ebc52d7a247
ms.openlocfilehash: aec177438004fd255bef85a5e5047cf6b5a6f782
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2018
ms.locfileid: "46284043"
---
# <a name="testability-and-entity-framework-40"></a>Prüfbarkeit und Entitätsframework 4.0
Scott Allen

Veröffentlicht: Mai 2010

## <a name="introduction"></a>Einführung

Dieses Whitepaper beschreibt und veranschaulicht das Schreiben von testbarem Code mit dem ADO.NET Entity Framework 4.0 und Visual Studio 2010. In diesem Artikel versucht nicht, sich auf eine bestimmte Testverfahren vor, wie z. B. Test-driven Design (TDD) oder verhaltensgesteuerter Entwurf (BDD) konzentrieren. Stattdessen in diesem Artikel liegt der Schwerpunkt auf Code schreiben, das ADO.NET Entity Framework verwendet, die noch bleibt, einfach zu isolieren und auf automatisierte Weise zu testen. Sehen wir uns allgemeine Entwurfsmuster, die zu erleichtern, Testszenarien in Daten zugreifen, und erfahren Sie, wie diese Muster bei der Verwendung des Frameworks zu übernehmen. Außerdem betrachten wir bestimmte Features des Frameworks zu sehen, wie diese Features in testfähiger Codeabschnitt ordnungsgemäß funktionieren können.

## <a name="what-is-testable-code"></a>Was ist testbarem Code?

Die Möglichkeit, um zu überprüfen, ob einen Teil der Software, die automatisierte Komponententests bietet viele Vorteile, die wünschenswert. Jeder weiß, dass gute Tests die Anzahl der von Softwarefehlern in einer Anwendung und die Erhöhung der Anwendung Codequalität – aber wenn Komponententests vorhanden geht weit über schon das Auffinden von Fehlern verringern werden.

Eine gute Komponententest-Suite ermöglicht ein Entwicklungsteam, Zeit zu sparen, und behalten die Kontrolle der Software, die sie erstellen. Ein Team kann Änderungen vornehmen, vorhandenen Code, Umgestalten, Umgestaltung und Umstrukturierung von Software an neue Anforderungen zu erfüllen, und neue Komponenten in einer Anwendung gleichzeitig zu wissen, die Testsammlung hinzufügen, kann das Verhalten der Anwendung überprüfen. Komponententests sind Teil eines Zyklus dann schnell rückmeldungen leicht geändert und die Verwaltbarkeit von Software als vergrößert sich die Komplexität zu erhalten.

Komponententests ist jedoch mit einem Preis. Ein Team muss die Zeit zum Erstellen und Verwalten von Komponententests zu investieren. Die erforderlichen Aufwand zum Erstellen dieser Tests steht in direkter Beziehung zu den **Testability** der zugrunde liegenden Software. Wie einfach die Software, um zu testen ist? Softwareentwicklung mit Prüfbarkeit Teams werden effektive Tests schneller als das Team arbeiten mit nicht testbare Software erstellen.

Microsoft hat das ADO.NET Entity Framework 4.0 (EF4) mit Prüfbarkeit entwickelt. Dies bedeutet nicht, dass Entwickler Komponententests für Frameworkcode selbst geschrieben werden. Stattdessen erleichtern die Testability-Ziele für EF4 testbarem Code zu erstellen, die basierend auf dem Framework erstellt. Bevor wir die spezifische Beispiele betrachten, lohnt es sich um Qualitäten testfähiger Codeabschnitt zu verstehen.

### <a name="the-qualities-of-testable-code"></a>Die Qualitäten von testbarem Code

Code, der einfach zu testen ist zeigt immer mindestens zwei "traits". Erstens ist testfähig Code ist einfach, **beobachten**. Wenn eine Gruppe von Eingaben, sollte es einfach, um die Ausgabe des Codes zu beobachten sein. Testen die folgende Methode ist beispielsweise einfach, da die Methode direkt das Ergebnis einer Berechnung zurückgibt.

``` csharp
    public int Add(int x, int y) {
        return x + y;
    }
```

Eine Methode testen ist schwierig, wenn die Methode den berechneten Wert in ein Netzwerksocket, einer Datenbanktabelle oder eine Datei, wie der folgende Code schreibt. Der Test muss sich um zusätzliche Vorgänge zum Abrufen des Werts auszuführen.

``` csharp
    public void AddAndSaveToFile(int x, int y) {
         var results = string.Format("The answer is {0}", x + y);
         File.WriteAllText("results.txt", results);
    }
```

Zweitens testbarem Code ist einfach, **isolieren**. Verwenden Sie im folgenden Pseudocode wir als Beispiel für fehlerhafte testbarem Code.

``` csharp
    public int ComputePolicyValue(InsurancePolicy policy) {
        using (var connection = new SqlConnection("dbConnection"))
        using (var command = new SqlCommand(query, connection)) {

            // business calculations omitted ...               

            if (totalValue > notificationThreshold) {
                var message = new MailMessage();
                message.Subject = "Warning!";
                var client = new SmtpClient();
                client.Send(message);
            }
        }
        return totalValue;
    }
```

Die Methode ist leicht zu sehen – wir können in einer Versicherungspolice zu übergeben und stellen Sie sicher, dass der Rückgabewert ein erwartetes Ergebnis übereinstimmt. Allerdings zum Testen der Methode müssen wir eine Datenbank mit dem korrekten Schema installiert haben, und konfigurieren den SMTP-Server aus, für den Fall, dass die Methode versucht, eine e-Mail zu senden.

Der Komponententest wünscht, dass nur um zu überprüfen, ob die Berechnungslogik innerhalb der Methode, aber der Test schlägt möglicherweise fehl, weil es sich bei der e-Mail-Server offline ist, oder der Datenbankserver befindet. Beide dieser Fehler sind nicht mit dem Verhalten, das möchte, dass der Test überprüfen. Das Verhalten ist schwierig, zu isolieren.

Softwareentwickler, die sich bemühen, Schreiben von testbarem Code häufig sich bemühen, warten eine Trennung der Zuständigkeiten im Code, dass sie schreiben. Die oben dargestellte Methode sollte konzentrieren, die Geschäfts-Berechnungen und delegieren die Implementierungsdetails für Datenbank "und"-e-Mail an andere Komponenten. Martin ruft dies auf dem Prinzip der einzigen Verantwortung. Ein Objekt muss eine einzelne, schmale Verantwortung, z. B. Berechnen des Werts von einer Richtlinie kapseln. Alle anderen Datenbank und Benachrichtigung arbeiten, sollten die Verantwortung für ein anderes Objekt sein. Auf diese Weise erstellter Code ist einfacher zu isolieren, da sie auf eine einzelne Aufgabe fokussiert ist.

.NET enthält die Abstraktionen, die wir müssen dem Prinzip der einzigen Verantwortung folgen und erreichen die Isolierung. Schnittstellendefinitionen verwenden Sie können und den Code so-schnittstellenabstraktion, die anstelle eines konkreten Typs zu erzwingen. Weiter unten in diesem Artikel werden wir sehen, wie eine Methode, wie Sie mit der ungültige oben angeführte Beispiel arbeiten kann, die Schnittstellen *suchen* , wie sie mit der Datenbank kommunizieren werden. Während der Tests können wir jedoch eine dummy-Implementierung ersetzen, die nicht mit der Datenbank kommunizieren, sondern Daten im Arbeitsspeicher enthält. Dieser dummy-Implementierung wird der Code nicht verknüpfte Probleme beheben, die in der Datenbankkonfiguration oder Code für den Datenzugriff isolieren.

Es gibt weitere Vorteile bei der Isolation. Die verwendete Berechnung in der letzten Methode dauert nur wenige Millisekunden zum Ausführen, aber der Test selbst möglicherweise mehrere Sekunden lang wie der Code-Hops für das Netzwerk und kommuniziert mit verschiedenen Servern ausgeführt. Komponententests sollten schnell um kleine Änderungen zu ermöglichen. Komponententests sollten auch wiederholbar sein und nicht fehl, da eine Komponente, die nicht mit den Test, ein Problem auftritt. Schreiben, dass Code, der einfach, das überwacht werden und zum Isolieren von bedeutet, dass Entwickler zum Schreiben von Tests für den Code weniger aufwendig, verbringen Sie weniger Zeit mit dem Warten auf auszuführende Tests und mehr wichtig ist, dass weniger Zeit für das Aufspüren von Fehlern, die nicht vorhanden sind.

Hoffentlich können, die testbarem Code weist Qualitäten Sie freuen uns über die Vorteile von Tests und verstehen. Wir sind dabei zu beschrieben, wie Sie Code schreiben, die mit EF4 zum Speichern von Daten in eine Datenbank während des Bestehens Observable- und einfach, zu isolieren, aber wir zuerst schränken wir konzentrieren uns um die testbarer Entwürfe für den Datenzugriff zu erörtern.

## <a name="design-patterns-for-data-persistence"></a>Entwurfsmuster für Dauerhaftigkeit von Daten

Beide fehlerhaften zuvor präsentierten Beispiele haben zu viele Aufgaben. Im erste Beispiel für die fehlerhafte Ausführen einer Berechnung mussten *und* in eine Datei schreiben. Im zweite Beispiel für die fehlerhafte musste zum Lesen von Daten aus einer Datenbank *und* führt eine Berechnung durch Business *und* -e-Mail zu senden. Mit dem Entwerfen von kleineren Methoden, die Trennen von Concerns, und diese Aufgabe an andere Komponenten delegieren, stellen Sie große Fortschritte für das Schreiben von testbarem Code. Das Ziel ist, um Funktionalität zu erstellen, durch das Erstellen von Aktionen über Abstraktionen für klein und konzentriert.

Wenn es darum, Datenpersistenz den kleinen geht und fokussierte Abstraktionen, die wir suchen so gängig, haben sie als Entwurfsmuster dokumentiert. Fowlers Buch Patterns of Enterprise Application Architecture war das erste Arbeitselement zum Beschreiben dieser Muster gedruckt. Wir gebe eine kurze Beschreibung dieser Muster in den folgenden Abschnitten, bevor wir zeigen, wie diese ADO.NET Entity Framework implementiert und kann mit dieser Muster.

### <a name="the-repository-pattern"></a>Das Repositorymuster

Fowler sagt, ein Repository "zwischen der Domäne und die Daten mithilfe einer sammlungsähnlichen Schnittstelle für den Zugriff auf die Domänenobjekte zuordnungsebenen vermittelt". Das Ziel des Repositorymusters zum Isolieren von Code über eine umfassende Datenzugriff ist und wie wir früher Isolation gesehen haben, ist eine erforderliche Eigenschaft für testbarkeit.

Die Taste, um die Isolation ist wie das Repository auf Objekte, die mithilfe einer sammlungsähnlichen Schnittstelle verfügbar macht. Die Logik, die Sie schreiben, verwenden Sie das Repository weiß nicht, wie das Repository werden die Objekte materialisieren, die Sie anfordern. Das Repository mit einer Datenbank kommunizieren kann, oder es kann nur Objekte zurückgeben, aus einer in-Memory-Sammlung. Alles, was Ihr Code muss wissen, ist, dass das Repository wird angezeigt, um die Auflistung zu verwalten, können Sie abrufen, hinzufügen und Löschen von Objekten aus der Auflistung.

In vorhandenen .NET-Anwendungen erbt eine konkrete Repository häufig von einer generischen Schnittstelle wie folgt:

``` csharp
    public interface IRepository<T> {       
        IEnumerable<T> FindAll();
        IEnumerable<T> FindBy(Expression<Func\<T, bool>> predicate);
        T FindById(int id);
        void Add(T newEntity);
        void Remove(T entity);
    }
```

Wir erstellen ein paar Änderungen zur Schnittstellendefinition Wir stellen eine Implementierung für EF4, aber das grundlegende Konzept bleibt unverändert. Code kann eine konkrete Repository, die Implementierung dieser Schnittstelle verwenden, um eine Entität von der primären Schlüsselwert, zum Abrufen einer Auflistung von Entitäten, die basierend auf der Auswertung eines Prädikats, abzurufen, oder rufen Sie einfach alle verfügbare Entitäten. Der Code kann auch hinzufügen und Entfernen von Entitäten über die Repositoryschnittstelle.

Wenn eine IRepository der Employee-Objekten, kann Code die folgenden Vorgänge ausführen.

``` csharp
    var employeesNamedScott =
        repository
            .FindBy(e => e.Name == "Scott")
            .OrderBy(e => e.HireDate);
    var firstEmployee = repository.FindById(1);
    var newEmployee = new Employee() {/*... */};
    repository.Add(newEmployee);
```

Da der Code eine Schnittstelle (IRepository der Mitarbeiter) verwendet wird, können wir den Code mit verschiedenen Implementierungen der Schnittstelle bereitstellen. Eine Implementierung ist möglicherweise eine Implementierung von EF4 und Beibehalten von Objekten in einer Microsoft SQL Server-Datenbank gesichert. Eine andere Implementierung (eine, die während der Tests verwendet werden) kann von einer Liste von Employee-Objekte im Arbeitsspeicher unterstützt werden. Die Schnittstelle trägt zur Isolierung im Code zu erzielen.

Beachten Sie, dass die IRepository&lt;T&gt; Schnittstelle macht ein Speichervorgang nicht verfügbar. Wie aktualisieren wir vorhandene Objekte? Gelangen Sie möglicherweise über IRepository-Definitionen, die den Speichervorgang enthalten sind, und Implementierungen der folgenden Repositorys sofort ein Objekt in der Datenbank beibehalten werden müssen. In vielen Anwendungen sollten sich jedoch nicht die Objekte einzeln beibehalten werden. Stattdessen möchten wir setzen Sie Objekte, vielleicht, diese Objekte als Teil einer Geschäftsaktivität aus anderen Repositorys, ändern und dann persistent gespeichert werden alle Objekte als Teil einer einzelnen atomaren Vorgang. Es ist ein Muster für diese Art von Verhalten zu ermöglichen.

### <a name="the-unit-of-work-pattern"></a>Das Arbeitseinheitsmuster

Fowler sagt eine Arbeitseinheit "verwaltet eine Liste von Objekten einer Geschäftstransaktion betroffen und koordiniert das Schreiben aus Änderungen und die Auflösung von Parallelitätsproblemen". Es ist die Verantwortung für die Arbeitseinheit zum Nachverfolgen von Änderungen an den Objekten, wir setzen aus einem Repository, und speichern alle Änderungen, die wir an den Objekten vorgenommen haben, wenn wir sagen, dass die Arbeitseinheit um die Änderungen zu übernehmen. Es ist auch die Verantwortung für die Arbeitseinheit die neuen Objekte werden, die wir alle Repositorys hinzugefügt haben, und fügen Sie die Objekte in die Datenbank, und auch verwalten löschen.

Wenn Sie bisher keine Arbeit mit ADO.NET DataSets durchgeführt haben wird bereits mit der arbeitseinheitsmuster vertraut sein. ADO.NET DataSets haben die Möglichkeit zur nachverfolgung von den Updates, löschungen und Einfügen von DataRow-Objekten und kann (mithilfe eines TableAdapter) abstimmen alle unsere Änderungen in einer Datenbank. DataSet-Objekte Modell jedoch einen getrennten Teil der zugrunde liegenden Datenbank. Das arbeitseinheitsmuster weist dasselbe Verhalten, aber mit Geschäftsobjekten und Domänenobjekte, die vom Datenzugriffscode isoliert und der Datenbank nicht bewusst sind.

Eine Abstraktion für die Arbeitseinheit in .NET-Code Modell könnte folgendermaßen aussehen:

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<Order> Orders { get; }
        IRepository<Customer> Customers { get; }
        void Commit();
    }
```

Durch Verfügbarmachen von Repository-Verweise, aus die Arbeitseinheit, die eine einzelne Arbeitseinheit sichergestellt hat Objekt die Möglichkeit, alle Entitäten, die während einer Geschäftstransaktion materialisiert nachverfolgen. Die Implementierung der Commit-Methode für eine echte Arbeitseinheit ist, in dem alle der Zauberstab Abstimmen von Änderungen im Speicher mit der Datenbank. 

Wenn einen IUnitOfWork-Verweis, Code nehmen Sie Änderungen an Geschäftsobjekte aus ein oder mehrere Repositorys abgerufen und speichert alle Änderungen, die mithilfe des atomic-Commit-Vorgangs.

``` csharp
    var firstEmployee = unitofWork.Employees.FindById(1);
    var firstCustomer = unitofWork.Customers.FindById(1);
    firstEmployee.Name = "Alex";
    firstCustomer.Name = "Christopher";
    unitofWork.Commit();
```

### <a name="the-lazy-load-pattern"></a>Das Lazy Loading-Muster

Fowler verwendet den Name-lazy-Load "ein Objekt, das alle Daten enthalten keine Sie benötigen, sondern weiß, wie Sie es herunterladen". Lazy Load transparent ist ein wichtiges Feature damit beim Schreiben von testfähigen Business-Code von und Arbeiten mit einer relationalen Datenbank. Betrachten Sie beispielsweise den folgenden Code ein.

``` csharp
    var employee = repository.FindById(id);
    // ... and later ...
    foreach(var timeCard in employee.TimeCards) {
        // .. manipulate the timeCard
    }
```

Wie wird die Rückkehr Auflistung aufgefüllt? Es gibt zwei mögliche Antworten. Eine Antwort ist, dass der Mitarbeiter-Repository, wenn Sie gefragt werden, um Mitarbeiter abzurufen, eine Abfrage zum Abrufen sowohl des Mitarbeiters sowie des Mitarbeiters zugeordneten Zeit Karteninformationen ausgibt. In relationale Datenbanken, die dies in der Regel erfordert eine Abfrage mit einer JOIN-Klausel und kann zu mehr Informationen als eine Anwendung abrufen müssen. Was geschieht, wenn die Anwendung nie muss die Eigenschaft für die Rückkehr zu berühren?

Eine zweite Antwort ist die Rückkehr-Eigenschaft "bei Nachfrage" geladen. Diese Lazy Load ist implizit und transparent für die Geschäftslogik, da es sich bei spezielle APIs zum Abrufen von Karteninformationen Zeit nicht in der Code aufgerufen wird. Der Code wird davon ausgegangen, dass der Zeit Informationen vorhanden, wenn erforderlich ist. Stellt Magie lazy Loading, die Common Language Runtime Abfangen der Methodenaufrufe in der Regel umfasst. Der abfangender Code ist für die Kommunikation mit der Datenbank und Abrufen von Informationen einmal-Karte, bleiben die Geschäftslogik können Geschäftslogik verantwortlich. Dieser lazy Loading-Magic-Befehl können den Code selbst Datenabrufvorgängen und führt weitere testbarem Code zu isolieren.

Der Nachteil ein lazy Load ist, die, wenn eine Anwendung *ist* benötigen die der Zeit Karteninformationen, die eine weitere Abfrage wird der Code ausgeführt. Dies ist ein Problem für viele Anwendungen, sondern für leistungsabhängige Anwendungen oder Anwendungen durchläuft eine Anzahl von Objekten für Mitarbeiter und Ausführen einer Abfrage zum Abrufen von Karten der Zeit während jeder Iteration der Schleife (ein Problem häufig als N + 1 bezeichnet nicht. abfrageproblem), lazy Loading steht ein Ziehvorgang. In diesen Szenarios möchten eine Anwendung laden Zeit Karteninformationen auf möglichst effiziente Weise möglich.

Zum Glück, erfahren Sie, wie EF4 unterstützt sowohl implizite verzögerten Laden und effiziente Eager Load geladen. wie wir in den nächsten Abschnitt verschieben und diese Muster zu implementieren.

## <a name="implementing-patterns-with-the-entity-framework"></a>Implementieren von Mustern, die mit dem Entitätsframework

Die gute Nachricht ist, dass alle Entwurfsmuster, die wir im letzten Abschnitt beschriebenen einfach mit EF4 zu implementieren sind. Zur Veranschaulichung werden wir eine einfache ASP.NET MVC-Anwendung verwenden, bearbeiten und Mitarbeiter und ihre zugeordneten Zeit Karte-Informationen angezeigt werden. Wir beginnen mit dem folgenden "plain old CLR Objects" (POCOs). 

``` csharp
    public class Employee {
        public int Id { get; set; }
        public string Name { get; set; }
        public DateTime HireDate { get; set; }
        public ICollection<TimeCard> TimeCards { get; set; }
    }

    public class TimeCard {
        public int Id { get; set; }
        public int Hours { get; set; }
        public DateTime EffectiveDate { get; set; }
    }
```

Diese Definitionen der Klasse ändert sich etwas untersucht, Ansätze und Funktionen von EF4, aber die Absicht ist, um diese Klassen als Persistenzignoranz (PI) wie möglich zu halten. Ein Objekt PI nicht weiß, *wie*, oder sogar *Wenn*, der Zustand enthält befindet sich in einer Datenbank. PI und POCOs gehen hand in hand mit testbare Software. Objekte, die mit einem POCO-Ansatz sind weniger eingeschränkte, flexibler und einfacher zu testen, da sie ohne vorhanden Datenbank ausgeführt werden können.

Wir können ein Entity Data Model (EDM) in Visual Studio erstellen, mit der POCOs eingerichtet (siehe Abbildung 1). Wir werden nicht das EDM verwenden, um Code für die Entitäten zu generieren. Stattdessen möchten wir die Entitäten verwenden, die es Entwicklern auch liebevoll manuell erstellen. Wir verwenden nur des EDM auf unsere Datenbankschema zu generieren, und geben Sie die Metadaten, die ef4 benötigt, um Objekte in der Datenbank zuzuordnen.

![EF test_01](~/ef6/media/eftest-01.jpg)

**Abbildung 1**

Hinweis: sollten Sie zunächst das EDM-Modell zu entwickeln, ist es möglich, bereinigen, POCO-Code aus dem EDM zu generieren. Dies ist mit der Erweiterung Visual Studio 2010, die vom Data Programmability-Team bereitgestellten möglich. Um die Erweiterung heruntergeladen haben, starten Sie den Erweiterungs-Manager in Visual Studio im Menü Extras, und suchen Sie im Onlinekatalog von Vorlagen für "POCO" (siehe Abbildung 2). Es sind mehrere POCO-Vorlagen für EF verfügbar. Weitere Informationen zur Verwendung der Vorlage finden Sie unter " [Exemplarische Vorgehensweise: POCO-Vorlage für das Entity Framework](https://blogs.msdn.com/adonet/pages/walkthrough-poco-template-for-the-entity-framework.aspx)".

![EF test_02](~/ef6/media/eftest-02.png)

**Abbildung 2**

Von diesem Ausgangspunkt POCO erforschen wir zwei verschiedene Ansätze zum testbarem Code. Der erste Ansatz rufe ich die EF-Ansatz, da datenabstraktionen in Einheiten von Arbeit und Repositorys implementiert die Entity Framework-API nutzt. Bei der zweiten Methode wir unserer eigenen benutzerdefinierten repositoryabstraktionen zu erstellen, und klicken Sie dann die vor- und Nachteile jeder Vorgehensweise. Wir beginnen, mithilfe des EF-Ansatzes.  

### <a name="an-ef-centric-implementation"></a>Eine zentrierte EF-Implementierung

Erwägen Sie die folgende Controlleraktion aus einem ASP.NET MVC-Projekt aus. Die Aktion ruft ein Employee-Objekt ab und gibt ein Ergebnis, um eine detaillierte Ansicht des Mitarbeiters anzuzeigen.

``` csharp
    public ViewResult Details(int id) {
        var employee = _unitOfWork.Employees
                                  .Single(e => e.Id == id);
        return View(employee);
    }
```

Ist der Code getestet werden? Es sind mindestens zwei Tests, die wir müssten, um das Verhalten der Aktion zu überprüfen. Zuerst möchten wir überprüfen, ob die Aktion der richtigen Ansicht – einen einfachen Test zurückgibt. Wir möchten auch einen Test aus, um zu überprüfen, ob die Aktion ruft die richtigen Mitarbeiter ab, und wir möchten dies tun, ohne Ausführung von Code zum Abfragen der Datenbank schreiben. Denken Sie daran, dass den getestete Code isoliert werden soll. Isolation sorgt, dass der Test nicht aufgrund eines Fehlers in der Datenbankkonfiguration oder den Datenzugriffscode fehlschlägt. Wenn der Test fehlschlägt, werden wir wissen, dass wir einen Fehler in der Controllerlogik und nicht in eine niedrigere Ebene Systemkomponente verfügen.

Um Isolierung zu erzielen, wir benötigen einige Abstraktionen, wie die Schnittstellen, die wir präsentiert weiter oben für Repositorys und Arbeitseinheiten. Denken Sie daran, dass das Repositorymuster für die Vermittlung zwischen Domänenobjekten und der Datenschicht für die Zuordnung ausgelegt ist. In diesem Szenario EF4 *ist* Zuordnung layer und stellt bereits eine Repository-ähnliche Abstraktion, die mit dem Namen IObjectSet&lt;T&gt; (aus dem Namespace System.Data.Objects). Die Schnittstellendefinition sieht folgendermaßen aus.

``` csharp
    public interface IObjectSet<TEntity> :
                     IQueryable<TEntity>,
                     IEnumerable<TEntity>,
                     IQueryable,
                     IEnumerable
                     where TEntity : class
    {
        void AddObject(TEntity entity);
        void Attach(TEntity entity);
        void DeleteObject(TEntity entity);
        void Detach(TEntity entity);
    }
```

IObjectSet&lt;T&gt; erfüllt die Anforderungen für ein Repository aus, da es sich um eine Auflistung von Objekten ähnelt (über "IEnumerable"&lt;T&gt;) und stellt Methoden zum Hinzufügen und Entfernen von Objekten aus der simulierten Auflistung bereit. Die Anfügen und trennen-Methoden verfügbar machen, den Funktionsumfang der EF4-API. Verwenden von IObjectSet&lt;T&gt; als Schnittstelle für Repositorys, die wir benötigen einer Einheit der Arbeit Abstraktion Repositorys miteinander verbunden.

``` csharp
    public interface IUnitOfWork {
        IObjectSet<Employee> Employees { get; }
        IObjectSet<TimeCard> TimeCards { get; }
        void Commit();
    }
```

Eine konkrete Implementierung dieser Schnittstelle wird wenden Sie sich an SQL Server und ist einfach, mit der ObjectContext-Klasse von EF4 zu erstellen. Die ObjectContext-Klasse ist die echten Arbeitseinheit in der EF4-API.

``` csharp
    public class SqlUnitOfWork : IUnitOfWork {
        public SqlUnitOfWork() {
            var connectionString =
                ConfigurationManager
                    .ConnectionStrings[ConnectionStringName]
                    .ConnectionString;
            _context = new ObjectContext(connectionString);
        }

        public IObjectSet<Employee> Employees {
            get { return _context.CreateObjectSet<Employee>(); }
        }

        public IObjectSet<TimeCard> TimeCards {
            get { return _context.CreateObjectSet<TimeCard>(); }
        }

        public void Commit() {
            _context.SaveChanges();
        }

        readonly ObjectContext _context;
        const string ConnectionStringName = "EmployeeDataModelContainer";
    }
```

Schalten ein IObjectSet&lt;T&gt; zum Leben ist so einfach wie das Aufrufen der CreateObjectSet-Methode von ObjectContext-Objekt. Hinter den Kulissen verwendet das Framework die Metadaten wir bereitgestellt haben, im EDM erstellen Sie eine konkrete "ObjectSet"&lt;T&gt;. Wir bei der Rückgabe der IObjectSet bleibe&lt;T&gt; Schnittstelle, da verhindern Testability im Clientcode beibehalten.

Dieser konkreten Implementierung eignet sich in der Produktion, aber wir müssen sich auf das konzentrieren, wie wir unsere IUnitOfWork-Abstraktion zum Vereinfachen der Tests verwenden.

### <a name="the-test-doubles"></a>Der Test-Doubles

Um die Controlleraktion zu isolieren benötigen wir die Möglichkeit zum Wechseln zwischen der echten Arbeitseinheit (unterstützt durch einen ObjectContext) und einen Test doppelte oder "gefälschtes" Arbeitseinheit (in-Memory-Vorgänge ausführen). Die übliche Methode zum Führen Sie diese Art von wechseln besteht darin, dass nicht die MVC-Controller eine Arbeitseinheit, sondern übergeben die Arbeitseinheit in den Controller als Konstruktorparameter instanziieren.

``` csharp
    class EmployeeController : Controller {
      publicEmployeeController(IUnitOfWork unitOfWork)  {
          _unitOfWork = unitOfWork;
      }
      ...
    }
```

Der obige Code ist ein Beispiel der Abhängigkeitsinjektion. Wir zulassen nicht, dass den Controller, erstellen sie die Abhängigkeit (die Arbeitseinheit), aber die Abhängigkeit in den Controller injizieren. In einem MVC-Projekt ist es üblich, eine benutzerdefinierten Controllerfactory in Kombination mit Inversion of Control-Container (IoC) zu verwenden, um die Abhängigkeitsinjektion zu automatisieren. Diese Themen können Sie über den Rahmen dieses Artikels hinaus, aber Sie können mehr durch nach den verweisen am Ende dieses Artikels lesen.

Eine gefälschte Einheit arbeiten-Implementierung, die wir für Tests verwenden können, kann wie folgt aussehen.

``` csharp
    public class InMemoryUnitOfWork : IUnitOfWork {
        public InMemoryUnitOfWork() {
            Committed = false;
        }
        public IObjectSet<Employee> Employees {
            get;
            set;
        }

        public IObjectSet<TimeCard> TimeCards {
            get;
            set;
        }

        public bool Committed { get; set; }
        public void Commit() {
            Committed = true;
        }
    }
```

Beachten Sie, dass die gefälschte Arbeitseinheit eine Commit-Eigenschaft verfügbar macht. Manchmal ist es nützlich, um das Hinzufügen von Funktionen zu einer gefälschten-Klasse, die Tests zu erleichtern. In diesem Fall ist es leicht zu sehen, wenn der Code eine Arbeitseinheit committet durch Überprüfen der-Eigenschaft ein Commit ausgeführt.

Wir benötigen auch eine gefälschte IObjectSet&lt;T&gt; , Employee "und" Zeitkartenverwaltung Objekte im Arbeitsspeicher. Wir bieten eine einzelne Implementierung verwenden von Generika.

``` csharp
    public class InMemoryObjectSet<T> : IObjectSet<T> where T : class
        public InMemoryObjectSet()
            : this(Enumerable.Empty<T>()) {
        }
        public InMemoryObjectSet(IEnumerable<T> entities) {
            _set = new HashSet<T>();
            foreach (var entity in entities) {
                _set.Add(entity);
            }
            _queryableSet = _set.AsQueryable();
        }
        public void AddObject(T entity) {
            _set.Add(entity);
        }
        public void Attach(T entity) {
            _set.Add(entity);
        }
        public void DeleteObject(T entity) {
            _set.Remove(entity);
        }
        public void Detach(T entity) {
            _set.Remove(entity);
        }
        public Type ElementType {
            get { return _queryableSet.ElementType; }
        }
        public Expression Expression {
            get { return _queryableSet.Expression; }
        }
        public IQueryProvider Provider {
            get { return _queryableSet.Provider; }
        }
        public IEnumerator<T> GetEnumerator() {
            return _set.GetEnumerator();
        }
        IEnumerator IEnumerable.GetEnumerator() {
            return GetEnumerator();
        }

        readonly HashSet<T> _set;
        readonly IQueryable<T> _queryableSet;
    }
```

Dieses Testdouble delegiert die meisten seiner Arbeit an einer zugrunde liegenden HashSet&lt;T&gt; Objekt. Beachten Sie, IObjectSet&lt;T&gt; erfordert eine generische Einschränkung T als eine Klasse (Referenztyp) erzwingen und erzwingt auch die "IQueryable" implementieren wir&lt;T&gt;. Es ist einfach eine in-Memory-Auflistung, die als ein "IQueryable" angezeigt werden,&lt;T&gt; mit dem standardmäßigen LINQ-Operator AsQueryable.

### <a name="the-tests"></a>Die Tests

Herkömmlicher Komponententests werden eine einzelnen Testklasse für alle Tests für alle Aktionen in einem einzelnen MVC-Controller verwenden. Können schreiben wir diese Tests oder jede Art von Komponententests mit den in-Memory-fakes erstellt haben. Allerdings für diesen Artikel, die wir zu vermeiden, wird der monolithischen Anwendung Klasse Ansatz zu testen und gruppieren stattdessen unsere Tests auf einen bestimmten Teil der Funktionalität konzentrieren.  Gebilligt z. B. "erstellen" möglicherweise die Funktionalität, die getestet werden soll, damit wir eine einzelnen Testklasse verwenden wird, um zu überprüfen, ob die einzelnen Controlleraktion, die verantwortlich für das Erstellen eines neuen Mitarbeiters.

Es gibt einige allgemeine Setupcode, die für alle diese differenzierte Testklassen erforderlich. Beispielsweise müssen wir immer unsere in-Memory-Repositorys und gefälschte Unit of Work, zu erstellen. Wir benötigen auch eine Instanz des Controllers Mitarbeiter die gefälschte Arbeitseinheit eingefügt. Wir werden diese allgemeine Setupcode für Testklassen freigeben, mithilfe von einer Basisklasse.

``` csharp
    public class EmployeeControllerTestBase {
        public EmployeeControllerTestBase() {
            _employeeData = EmployeeObjectMother.CreateEmployees()
                                                .ToList();
            _repository = new InMemoryObjectSet<Employee>(_employeeData);
            _unitOfWork = new InMemoryUnitOfWork();
            _unitOfWork.Employees = _repository;
            _controller = new EmployeeController(_unitOfWork);
        }

        protected IList<Employee> _employeeData;
        protected EmployeeController _controller;
        protected InMemoryObjectSet<Employee> _repository;
        protected InMemoryUnitOfWork _unitOfWork;
    }
```

Wir in der Basisklasse verwenden "-Objekt Mutter" ist ein häufiges Muster für das Erstellen von Testdaten. Ein Objekt Mutter enthält Factorymethoden zum testentitäten für die Verwendung in mehreren prüfvorrichtungen zu instanziieren.

``` csharp
    public static class EmployeeObjectMother {
        public static IEnumerable<Employee> CreateEmployees() {
            yield return new Employee() {
                Id = 1, Name = "Scott", HireDate=new DateTime(2002, 1, 1)
            };
            yield return new Employee() {
                Id = 2, Name = "Poonam", HireDate=new DateTime(2001, 1, 1)
            };
            yield return new Employee() {
                Id = 3, Name = "Simon", HireDate=new DateTime(2008, 1, 1)
            };
        }
        // ... more fake data for different scenarios
    }
```

Wir können die EmployeeControllerTestBase als Basisklasse für eine Reihe von prüfvorrichtungen verwenden (siehe Abbildung 3). Jeder Testfixture testet eine bestimmte Controlleraktion. Z. B. eine Testfixture konzentriert sich auf den Test der erstellen-Aktion, die während einer HTTP GET-Anforderung (für die Ansicht für die Erstellung eines Mitarbeiters Anzeige) verwendet und eine andere Fixierung konzentriert sich auf die Create-Aktion, die in einer HTTP POST-Anforderung verwendet (von übermittelt Informationen zu den Benutzer, der einen Mitarbeiter zu erstellen). Jede abgeleitete Klasse ist nur für das Setup erforderlich sind, in der spezifischen Kontext und geben Sie die Assertionen, die erforderlich sind, um zu überprüfen, ob die Ergebnisse für die Kontext-spezifischer Test verantwortlich.

![EF test_03](~/ef6/media/eftest-03.png)

**Abbildung 3**

Die Aufrufkonvention und der Test Benennungsstil hier vorgestellten nicht für testbarem Code erforderlich ist – es nur einen Ansatz ist. Abbildung 4 zeigt, dass die Tests ausgeführt werden, in der Jet-Gehirne Resharper Runner-Plug-In für Visual Studio 2010 testen.

![EF test_04](~/ef6/media/eftest-04.png)

**Abbildung 4**

Sind mit einer Basisklasse, die freigegebene Setupcode zu behandeln die Komponententests für jede Controlleraktion, klein und einfach zu schreiben. Die Tests schnell (da wir in-Memory-Vorgängen durchgeführt wird) ausgeführt werden, und sollte nicht aufgrund von-Infrastruktur oder umweltbeeinträchtigungen bei fehlschlagen, (da wir die Komponenten unter testbedingungen isoliert haben).

``` csharp
    [TestClass]
    public class EmployeeControllerCreateActionPostTests
               : EmployeeControllerTestBase {
        [TestMethod]
        public void ShouldAddNewEmployeeToRepository() {
            _controller.Create(_newEmployee);
            Assert.IsTrue(_repository.Contains(_newEmployee));
        }
        [TestMethod]
        public void ShouldCommitUnitOfWork() {
            _controller.Create(_newEmployee);
            Assert.IsTrue(_unitOfWork.Committed);
        }
        // ... more tests

        Employee _newEmployee = new Employee() {
            Name = "NEW EMPLOYEE",
            HireDate = new System.DateTime(2010, 1, 1)
        };
    }
```

Bei diesen Tests wird die Basisklasse die meiste Arbeit Setup. Denken Sie daran, dass der Konstruktor der Basisklasse der in-Memory-Repository, eine gefälschte Arbeitseinheit und eine Instanz der Klasse EmployeeController erstellt. Die Testklasse wird von dieser Basisklasse abgeleitet und konzentriert sich auf die Einzelheiten die Create-Methode zu testen. In diesem Fall erzwingungsebenen die Besonderheiten, die "anordnen, agieren und Assertion"-Schritte, die Sie in alle Verfahren für die Komponententests sehen:

-   Erstellen Sie ein NeuerMitarbeiter-Objekt, um eingehende Daten zu simulieren.
-   Rufen Sie die Create-Aktion, der die EmployeeController, und die NeuerMitarbeiter übergeben.
-   Stellen Sie sicher, dass die Aktion "Create" die erwarteten Ergebnisse erzeugt werden (der Mitarbeiter, die in das Repository angezeigt wird).

Was wir erstellt haben, können wir die EmployeeController Aktionen zu testen. Wenn wir Schreiben von Tests für die Index-Aktion des Controllers Mitarbeiter können wir beispielsweise von der Basisklasse "Test" zu, um die gleiche grundlegende Einrichtung für unsere Tests herzustellen erben. Die Basisklasse wird erneut das Repository im Arbeitsspeicher, die falsche Arbeitseinheit und einer Instanz von der EmployeeController erstellt. Die Tests für die Index-Aktion nur konzentrieren, die Index-Aktion aufrufen müssen, und gibt die Qualität des Modells die Aktion zu testen.

``` csharp
    [TestClass]
    public class EmployeeControllerIndexActionTests
               : EmployeeControllerTestBase {
        [TestMethod]
        public void ShouldBuildModelWithAllEmployees() {
            var result = _controller.Index();
            var model = result.ViewData.Model
                          as IEnumerable<Employee>;
            Assert.IsTrue(model.Count() == _employeeData.Count);
        }
        [TestMethod]
        public void ShouldOrderModelByHiredateAscending() {
            var result = _controller.Index();
            var model = result.ViewData.Model
                         as IEnumerable<Employee>;
            Assert.IsTrue(model.SequenceEqual(
                           _employeeData.OrderBy(e => e.HireDate)));
        }
        // ...
    }
```

Die Tests, die erstellt wird, wird mithilfe von Fakes für im Arbeitsspeicher sind auf die Tests ausgerichtet der *Zustand* der Software. Beispielsweise enthalten beim Testen der Aktion "Create", untersuchen Sie den Status des Repositorys ein, nachdem die Aktion "Create" ausgeführt wird – sollten, im Repository des neuen Mitarbeiters?

``` csharp
    [TestMethod]
    public void ShouldAddNewEmployeeToRepository() {
        _controller.Create(_newEmployee);
        Assert.IsTrue(_repository.Contains(_newEmployee));
    }
```

Später werden wir Interaktion basierten testen betrachten. Testen der Interaktion basierend fragt, ob es sich bei der getesteten Code die entsprechenden Methoden für unsere Objekte aufgerufen und die richtigen Parameter übergeben. Jetzt werden wir die Abdeckung einen anderen-Entwurfsmuster – lazy Loading verschieben.

## <a name="eager-loading-and-lazy-loading"></a>Eager Load und Lazy Loading

An einem bestimmten Punkt in der ASP.NET MVC-Web zugeordnete Anwendung, die wir möglicherweise eines Mitarbeiters-Informationen anzeigen und des Mitarbeiters einschließen möchten Zeit Karten an. Wir möglicherweise z. B. eine Zeitpunkt-Karte angezeigt werden sollen, die den Namen des Mitarbeiters und die Gesamtzahl der Zeit Karten im System angezeigt. Es gibt verschiedene Ansätze, die wir ausführen können, um dieses Feature zu implementieren.

### <a name="projection"></a>Projection

Ein einfacher Ansatz zum Erstellen der Zusammenfassung ist zum Erstellen eines Modells, die speziell für die Informationen, die wir in der Ansicht angezeigt werden soll. In diesem Szenario kann das Modell wie folgt aussehen.

``` csharp
    public class EmployeeSummaryViewModel {
        public string Name { get; set; }
        public int TotalTimeCards { get; set; }
    }
```

Beachten Sie, dass die EmployeeSummaryViewModel keine Entität ist – anders ausgedrückt: Es ist nicht etwas, das wir in der Datenbank beibehalten möchten. Wir werden nur diese Klasse verwenden, um Daten in die Ansicht in einem strikter Typzuordnung zu mischen. Das Ansichtsmodell ist wie eine Daten übertragen (Object, DTO), da er kein Verhalten (keine Methoden) – nur Eigenschaften enthält. Die Eigenschaften werden die Daten aufzunehmen, die wir verschieben müssen. Es ist einfach, dieses Modell anzeigen, die mithilfe LINQs standard projektionsoperator – der Select-Operator zu instanziieren.

``` csharp
    public ViewResult Summary(int id) {
        var model = _unitOfWork.Employees
                               .Where(e => e.Id == id)
                               .Select(e => new EmployeeSummaryViewModel
                                  {
                                    Name = e.Name,
                                    TotalTimeCards = e.TimeCards.Count()
                                  })
                               .Single();
        return View(model);
    }
```

Es gibt zwei wichtige Features, dem obigen Code. Zuerst – ist der Code einfach zu testen, da es immer noch leicht ist zu überwachen und zu isolieren. Der Select-Operator funktioniert ebenso gut für unsere in-Memory-Fakes wie für die echten Arbeitseinheit.

``` csharp
    [TestClass]
    public class EmployeeControllerSummaryActionTests
               : EmployeeControllerTestBase {
        [TestMethod]
        public void ShouldBuildModelWithCorrectEmployeeSummary() {
            var id = 1;
            var result = _controller.Summary(id);
            var model = result.ViewData.Model as EmployeeSummaryViewModel;
            Assert.IsTrue(model.TotalTimeCards == 3);
        }
        // ...
    }
```

Die zweite wichtige Funktion ist wie der Code EF4, generieren Sie eine einmalige und effiziente-Abfrage, um Informationen für Mitarbeiter und Zeit-Karte gemeinsam zusammenstellen kann. Wir haben Informationen zu Mitarbeitern und Zeit-Karteninformationen in das gleiche Objekt geladen, ohne spezielle APIs verwenden. Der Code ausgedrückt lediglich die Informationen, die es erfordert die Verwendung der standardmäßiger LINQ-Standardabfrageoperatoren, die in-Memory-Datenquellen sowie Remotedatenquellen entgegenwirken. EF4 wurde die Ausdrucksbaumstrukturen, die von der LINQ-Abfrage und C generiert übersetzen\# Compiler in eine einmalige und effiziente T-SQL-Abfrage.

``` SQL
    SELECT
    [Limit1].[Id] AS [Id],
    [Limit1].[Name] AS [Name],
    [Limit1].[C1] AS [C1]
    FROM (SELECT TOP (2)
      [Project1].[Id] AS [Id],
      [Project1].[Name] AS [Name],
      [Project1].[C1] AS [C1]
      FROM (SELECT
        [Extent1].[Id] AS [Id],
        [Extent1].[Name] AS [Name],
        (SELECT COUNT(1) AS [A1]
         FROM [dbo].[TimeCards] AS [Extent2]
         WHERE [Extent1].[Id] =
               [Extent2].[EmployeeTimeCard_TimeCard_Id]) AS [C1]
              FROM [dbo].[Employees] AS [Extent1]
               WHERE [Extent1].[Id] = @p__linq__0
         )  AS [Project1]
    )  AS [Limit1]
```

Es gibt in anderen Fällen, wenn es nicht mit einem Ansichtsmodell oder DTO-Objekt, jedoch mit echten Entitäten verwenden möchten. Wenn wir wissen, dass wir einen Mitarbeiter benötigen *und* des Mitarbeiters Zeitkarten, wir können die verwandten Daten sorgfältig auf unaufdringliche und effiziente Weise laden.

### <a name="explicit-eager-loading"></a>Explizite Eager Loading

Wenn wir die verknüpfte Entität-Informationen, die wir benötigen einen Mechanismus, für die Geschäftslogik (oder in diesem Szenario wird die Aktion der Controllerlogik) laden möchten, ihren Wunsch an das Repository auszudrücken. Die EF4 ObjectQuery&lt;T&gt; -Klasse definiert eine Include-Methode, um anzugeben, die verknüpften Objekte während der Abfrage abgerufen. Denken Sie daran, EF4 ObjectContext verfügbar macht, Entitäten über die konkreten "ObjectSet"&lt;T&gt; die von ObjectQuery erbt&lt;T&gt;.  Wenn wir mit "ObjectSet" verwendeten&lt;T&gt; Verweise in unserer Controlleraktion schreiben wir den folgenden Code hinzu, geben ein unverzüglichem Laden von Time-Karteninformationen für jeden Mitarbeiter.

``` csharp
    _employees.Include("TimeCards")
              .Where(e => e.HireDate.Year > 2009);
```

Aber da wir unseren Code testbar möchten wir nicht offen "ObjectSet"&lt;T&gt; von außerhalb der echten Maßeinheit Arbeit-Klasse. Stattdessen wir abhängig von der IObjectSet&lt;T&gt; -Schnittstelle, um falsche vereinfacht wird, aber IObjectSet&lt;T&gt; definiert eine "Include"-Methode nicht. Das Schöne an LINQ ist, dass wir unseren eigenen Include-Operator erstellen können.

``` csharp
    public static class QueryableExtensions {
        public static IQueryable<T> Include<T>
                (this IQueryable<T> sequence, string path) {
            var objectQuery = sequence as ObjectQuery<T>;
            if(objectQuery != null)
            {
                return objectQuery.Include(path);
            }
            return sequence;
        }
    }
```

Beachten Sie, dass dieser Include-Operator wird als eine Erweiterungsmethode definiert, für die "IQueryable"&lt;T&gt; IObjectSet&lt;T&gt;. Dies ergibt die Möglichkeit, verwenden die Methode mit einer größeren Auswahl der möglichen Typen, einschließlich "IQueryable"&lt;T&gt;, IObjectSet&lt;T&gt;, ObjectQuery&lt;T&gt;, und "ObjectSet"&lt;T&gt;. Falls die zugrunde liegenden Sequenz keine echte EF4 ObjectQuery ist&lt;T&gt;, gibt es keinen Schaden durchgeführt und der Include-Operator ist keine Aktion. Wenn die zugrunde liegende sequenzieren *ist* ObjectQuery&lt;T&gt; (oder ObjectQuery abgeleitet&lt;T&gt;), EF4 finden Sie unter der Anforderung für zusätzliche Daten und formulieren die richtige SQL wird Abfrage.

Mit dieser neuen Operator direktes können wir ein unverzüglichem Laden von Karteninformationen Zeit explizit aus dem Repository anfordern.

``` csharp
    public ViewResult Index() {
        var model = _unitOfWork.Employees
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

Wenn für eine echte ObjectContext ausführen, erstellt der Code die folgende Abfrage für die einzelne. Die Abfrage sammelt genug Informationen aus der Datenbank in einen Trip zur materialisieren der Employee-Objekten und vollständig auffüllen ihrer Rückkehr-Eigenschaft.

``` SQL
    SELECT
    [Project1].[Id] AS [Id],
    [Project1].[Name] AS [Name],
    [Project1].[HireDate] AS [HireDate],
    [Project1].[C1] AS [C1],
    [Project1].[Id1] AS [Id1],
    [Project1].[Hours] AS [Hours],
    [Project1].[EffectiveDate] AS [EffectiveDate],
    [Project1].[EmployeeTimeCard_TimeCard_Id] AS [EmployeeTimeCard_TimeCard_Id]
    FROM ( SELECT
         [Extent1].[Id] AS [Id],
         [Extent1].[Name] AS [Name],
         [Extent1].[HireDate] AS [HireDate],
         [Extent2].[Id] AS [Id1],
         [Extent2].[Hours] AS [Hours],
         [Extent2].[EffectiveDate] AS [EffectiveDate],
         [Extent2].[EmployeeTimeCard_TimeCard_Id] AS
                    [EmployeeTimeCard_TimeCard_Id],
         CASE WHEN ([Extent2].[Id] IS NULL) THEN CAST(NULL AS int)
         ELSE 1 END AS [C1]
         FROM  [dbo].[Employees] AS [Extent1]
         LEFT OUTER JOIN [dbo].[TimeCards] AS [Extent2] ON [Extent1].[Id] = [Extent2].[EmployeeTimeCard_TimeCard_Id]
    )  AS [Project1]
    ORDER BY [Project1].[HireDate] ASC,
             [Project1].[Id] ASC, [Project1].[C1] ASC
```

Die gute Nachricht ist, dass der Code innerhalb der Aktionsmethode bleibt, die vollständig getestet werden. Wir müssen keine zusätzlichen Funktionen für unsere Fakes zur Unterstützung des Include-Operators angeben. Die schlechte Nachricht ist, dass den Include-Operator in der Code verwendet, wollten wir auch Persistenz ignorierende, werden musste. Dies ist ein hervorragendes Beispiel für den Typ der Kompromisse, die Sie beim Entwickeln testfähigen Codeabschnitt ordnungsgemäß ausgewertet werden müssen. Es gibt Situationen, Sie Persistenz Bedenken Speicherverlusten außerhalb der Abstraktion Repository für die Leistungsziele zu ermöglichen müssen.

Die Alternative zur Eager Load ist lazy Loading. Lazy loading-bedeutet, dass wir tun *nicht* benötigen unsere Business-Code, um die Anforderung für die zugehörigen Daten explizit bekanntgeben zu können. Stattdessen verwenden wir die Entitäten in der Anwendung und zusätzliche Daten benötigt Entity Framework lädt die Daten nach Bedarf.

### <a name="lazy-loading"></a>Lazy Loading

Es ist einfach, stellen Sie sich ein Szenario vor, in denen wir nicht wissen, welche Daten ein Teil der Geschäftslogik müssen. Wir unter Umständen kennen Sie die Logik benötigt ein Employee-Objekt, aber wir Verzweigung in unterschiedliche Ausführungspfade, in denen einige dieser Pfade erfordern Zeit Informationen aus der Mitarbeiter und andere nicht. Szenarios wie diese sind ideal für implizite verzögerten Laden, da Daten wie von Zauberhand auf einen Bedarf angezeigt wird.

Lazy Loading, auch bekannt als verzögert geladen, einige Anforderungen für unsere Entitätsobjekte platzieren. POCOs mit "true" Ignorieren der Persistenz würde keine Anforderungen von der persistenzschicht zur Verfügung stehen, aber "true" Ignorieren der Persistenz ist praktisch unmöglich, zu erreichen.  Stattdessen messen wir die Persistenz in relativen Grad. Es wäre unglücklich, wenn wir brauchten eine dienstorientierte Persistenz-Basisklasse erben, oder verwenden eine spezielle Auflistung lazy loadings in POCOs erreichen. Glücklicherweise verfügt EF4 eine Lösung mit weniger intrusive.

### <a name="virtually-undetectable"></a>Praktisch nicht aufzuspüren

Wenn Sie POCO-Objekte zu verwenden, können EF4 dynamisch Laufzeit Proxys für Entitäten generiert werden. Diese Proxys unsichtbar umschließen die materialisierte POCOs angeben, dass zusätzliche Dienste durch Abfangen jeder Eigenschaft erhalten und set-Vorgang, um zusätzliche Vorgänge auszuführen. Ein solcher Dienst ist die lazy Loading-Funktion, die wir suchen. Ein anderer Dienst ist, einen effizienten änderungsnachverfolgungsmechanismus die aufzeichnen können, wenn das Programm die Eigenschaftswerte einer Entität geändert wird. Die Liste der Änderungen wird vom ObjectContext während SaveChanges-Methode verwendet, alle geänderten Entitäten, die mithilfe der UPDATE-Befehle beibehalten werden.

Für diese Proxys zur Arbeit fahren allerdings sie benötigen eine Möglichkeit, Sie hook Eigenschaft Get und Set-Vorgänge für eine Entität und die Proxys dieses Ziel erreichen, indem virtuelle Member überschreiben. Daher sollten wir implizite lazy Loading und effizienten änderungsnachverfolgung haben müssen wir wechseln zurück zu unserem POCO-Klassendefinitionen und Eigenschaften als virtuell markiert.

``` csharp
    public class Employee {
        public virtual int Id { get; set; }
        public virtual string Name { get; set; }
        public virtual DateTime HireDate { get; set; }
        public virtual ICollection<TimeCard> TimeCards { get; set; }
    }
```

Wir können noch sagen, dass die Employee-Entität vor allem die keine Persistenz ist. Die einzige Anforderung ist die Verwendung von virtuellen Member und dies hat keine Auswirkungen auf die testmöglichkeiten von Code. Wir müssen keine spezielle Basisklasse abgeleitet werden soll, oder verwenden Sie auch eine spezielle Auflistung, die speziell für verzögertes Laden. Wie der Code, zeigt jede Klasse, die ICollection implementieren&lt;T&gt; ist verfügbar, um verknüpfte Entitäten zu speichern.

Es gibt auch einen kleine Änderung, die wir in unserer Unit of Work, machen müssen. Mit Lazy Loading steht *aus* standardmäßig bei der Arbeit direkt mit einem ObjectContext-Objekt. Es ist eine Eigenschaft, die wir für die ContextOptions-Eigenschaft festlegen können, aktivieren Sie verzögertes Laden, und wir können diese Eigenschaft in unsere tatsächlichen Unit of Work, festlegen, wenn Sie möchten, aktivieren Sie das verzögerte Laden überall.

``` csharp
    public class SqlUnitOfWork : IUnitOfWork {
         public SqlUnitOfWork() {
             // ...
             _context = new ObjectContext(connectionString);
             _context.ContextOptions.LazyLoadingEnabled = true;
         }    
         // ...
     }
```

Implizite lazy Loading aktiviert, kann Anwendungscode einen Mitarbeiter und des Mitarbeiters Zeit Karten während des Bestehens der Arbeitsaufwand für EF zum Laden der zusätzlichen Daten nicht bekannt.

``` csharp
    var employee = _unitOfWork.Employees
                              .Single(e => e.Id == id);
    foreach (var card in employee.TimeCards) {
        // ...
    }
```

Lazy Loading vereinfacht den Anwendungscode schreiben, und mit dem Proxy-Magic bleibt des Codes vollständig getestet werden. In-Memory-Fakes, die Arbeitseinheit können einfach vorab laden, gefälschte Entitäten mit zugeordneten Daten bei Bedarf während eines Tests.

An diesem Punkt werden wir unsere Aufmerksamkeit von der Erstellung des Repositorys mithilfe des IObjectSet aktivieren&lt;T&gt; und sehen Sie sich Abstraktionen, um alle Anzeichen für das persistenzframework auszublenden.

## <a name="custom-repositories"></a>Benutzerdefinierte Repositorys

Wenn wir zunächst die Einheit der Entwurfsmuster für die Arbeit in diesem Artikel vorgestellt bereitgestellt wurde Beispielcode, für die Arbeitseinheit könnte folgendermaßen aussehen. Erneut stellen wir diese ursprünglichen Idee, die mit dem Mitarbeiter und Mitarbeiter Zeit Karte-Szenario, das wir gearbeitet haben.

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<TimeCard> TimeCards { get;  }
        void Commit();
    }
```

Der Hauptunterschied zwischen dieser Arbeitseinheit und jene Arbeitseinheit, die wir im letzten Abschnitt erstellt wird, wie dieser Arbeitseinheit aller Abstraktionen von EF4 Framework nicht verwendet wird (es ist nicht IObjectSet&lt;T&gt;). IObjectSet&lt;T&gt; funktioniert gut als einer Repository-Schnittstelle, aber die API macht möglicherweise nicht perfekt im Einklang mit der Anwendung Bedürfnissen. Bei diesem Ansatz bevorstehende-Repositorys mithilfe einer benutzerdefinierten IRepository dargestellt werden&lt;T&gt; Abstraktion.

Viele Entwickler, die Test-driven Design, verhaltensgesteuerter Entwurf und Entwurf von Domain-driven Methoden führen Sie es vorziehen, die IRepository&lt;T&gt; Ansatz, verschiedene Ursachen haben. Zuerst die IRepository&lt;T&gt; Schnittstelle stellt eine "antibeschädigungsebene" dar. Wie in seinem Domain-Driven Design-Buch von Eric Evans beschrieben behält eine Anti-Corruption-Ebene Ihren domänencode enthalten von Infrastruktur-APIs, wie ein Persistenz-API. Zweitens können Entwickler Methoden erstellen, in das Repository, die eine Anwendung die genauen Anforderungen erfüllen (wie beim Schreiben von Tests ermittelt). Beispielsweise können wir häufig müssen eine einzelne Entität, die mit einem ID-Wert, damit die Repositoryschnittstelle eine FindById-Methode hinzugefügt werden können.  Unsere IRepository&lt;T&gt; Definition wird wie folgt aussehen.

``` csharp
    public interface IRepository<T>
                    where T : class, IEntity {
        IQueryable<T> FindAll();
        IQueryable<T> FindWhere(Expression<Func\<T, bool>> predicate);
        T FindById(int id);       
        void Add(T newEntity);
        void Remove(T entity);
    }
```

Beachten Sie, dass legen wir zurück zur Verwendung einer "IQueryable"&lt;T&gt; entitätsauflistungen verfügbar gemacht werden. "IQueryable"&lt;T&gt; ermöglicht LINQ-Ausdrucksbaumstrukturen flow in den EF4-Anbieter und des Anbieters eine ganzheitliche Sicht der Abfrage. Die zweite Möglichkeit wäre die "IEnumerable" zurückgeben&lt;T&gt;, was bedeutet, dass den EF4 LINQ-Anbieter wird nur angezeigt, die Ausdrücke, die in das Repository erstellt. Gruppierung, Sortierung und Projektion ausgeführt wird, außerhalb des Repositorys werden nicht erstellt werden, in der SQL-Befehl gesendet, um die Datenbank, die Leistung beeinträchtigt werden kann. Andererseits, ein Repository nur "IEnumerable" zurückgeben&lt;T&gt; Ergebnisse überraschen Sie nie mit einem neuen SQL‑Befehl. Beide Ansätze funktioniert, und beide Ansätze testfähig bleiben.

Es ist einfach, geben Sie eine einzelne Implementierung der IRepository&lt;T&gt; Schnittstelle mit Generika und der ObjectContext-API von EF4.

``` csharp
    public class SqlRepository<T> : IRepository<T>
                                    where T : class, IEntity {
        public SqlRepository(ObjectContext context) {
            _objectSet = context.CreateObjectSet<T>();
        }
        public IQueryable<T> FindAll() {
            return _objectSet;
        }
        public IQueryable<T> FindWhere(
                               Expression<Func\<T, bool>> predicate) {
            return _objectSet.Where(predicate);
        }
        public T FindById(int id) {
            return _objectSet.Single(o => o.Id == id);
        }
        public void Add(T newEntity) {
            _objectSet.AddObject(newEntity);
        }
        public void Remove(T entity) {
            _objectSet.DeleteObject(entity);
        }
        protected ObjectSet<T> _objectSet;
    }
```

Der IRepository&lt;T&gt; Ansatz haben wir einige zusätzliche Kontrolle darüber unserer Abfragen da sich ein Client zum Aufrufen einer Methode, um eine Entität zu erhalten. Innerhalb der Methode können wir zusätzliche Prüfungen und LINQ-Operatoren zum Erzwingen von anwendungseinschränkungen bereitstellen. Beachten Sie, dass die Schnittstelle verfügt über zwei Einschränkungen in den generischen Typparameter. Die erste Einschränkung ist die Klasse Nachteile Qualitätsmerkmale "ObjectSet" erforderlichen&lt;T&gt;, und die zweite Einschränkung erzwingt, dass die Entitäten IEntity – eine Abstraktion für die Anwendung erstellt zu implementieren. IEntity-Schnittstelle erzwingt, dass Entitäten, die eine lesbare Id-Eigenschaft aufweisen, und wir können Sie diese Eigenschaft in der FindById-Methode. IEntity ist mit den folgenden Code definiert.

``` csharp
    public interface IEntity {
        int Id { get; }
    }
```

IEntity kann die eine kleine Verletzung ignorieren der Persistenz angesehen werden, da die Entitäten erforderlich sind, diese Schnittstelle implementieren. Denken Sie daran ignorieren der Persistenz geht es um Kompromisse und für viele die FindById-Funktionalität überwiegen die Einschränkung seitens der Schnittstelle. Die Schnittstelle hat keine Auswirkungen auf die testbarkeit.

Instanziieren eine live IRepository&lt;T&gt; erfordert einen ObjectContext EF4 eine konkrete Einheit arbeiten-Implementierung die Instanziierung verwalten soll.

``` csharp
    public class SqlUnitOfWork : IUnitOfWork {
        public SqlUnitOfWork() {
            var connectionString =
                ConfigurationManager
                    .ConnectionStrings[ConnectionStringName]
                    .ConnectionString;

            _context = new ObjectContext(connectionString);
            _context.ContextOptions.LazyLoadingEnabled = true;
        }

        public IRepository<Employee> Employees {
            get {
                if (_employees == null) {
                    _employees = new SqlRepository<Employee>(_context);
                }
                return _employees;
            }
        }

        public IRepository<TimeCard> TimeCards {
            get {
                if (_timeCards == null) {
                    _timeCards = new SqlRepository<TimeCard>(_context);
                }
                return _timeCards;
            }
        }

        public void Commit() {
            _context.SaveChanges();
        }

        SqlRepository<Employee> _employees = null;
        SqlRepository<TimeCard> _timeCards = null;
        readonly ObjectContext _context;
        const string ConnectionStringName = "EmployeeDataModelContainer";
    }
```

### <a name="using-the-custom-repository"></a>Verwenden das benutzerdefinierte Repository

Mit unseren benutzerdefinierten Repositorys ist nicht grundlegend von mithilfe des Repositorys, die basierend auf IObjectSet&lt;T&gt;. Anstelle von LINQ-Operatoren direkt auf eine Eigenschaft anwenden, wir müssen zunächst eine des Repositorys-Methoden zum Abrufen einer "IQueryable" aufrufen&lt;T&gt; Verweis.

``` csharp
    public ViewResult Index() {
        var model = _repository.FindAll()
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

Beachten Sie, dass die benutzerdefinierte Include-Operator, den wir zuvor implementiert ohne Änderung funktionieren. FindById-Methode des Repositorys entfernt doppelte Logik aus Aktionen, die versuchen, eine einzelne Entität abrufen.

``` csharp
    public ViewResult Details(int id) {
        var model = _repository.FindById(id);
        return View(model);
    }
```

Es ist kein wesentlicher Unterschied zwischen der testbarkeit der beiden Methoden, die wir untersucht haben. Bieten wir falsche Implementierungen IRepository&lt;T&gt; erstellen konkrete Klassen, die durch HashSet gesichert&lt;Mitarbeiter&gt; – wie was wir im letzten Abschnitt hat. Einige Entwickler bevorzugen es jedoch verwenden Sie Mockobjekte und Modellieren von Objekt-Frameworks, anstatt zu Fakes zu erstellen. Betrachten wir mit Mocks testen unsere Implementierung und die Unterschiede zwischen Mocks und Fakes im nächsten Abschnitt erläutert.

### <a name="testing-with-mocks"></a>Testen mit Mocks

Es gibt verschiedene Ansätze zum Erstellen von welche Aufrufe von Martin Fowler ein "Testdouble". Ein Testdouble (z. B. einen Film Testdouble) ist ein Objekt, das Sie erstellen, um "für real stehen" Produktions-Objekte während der Tests. Die in-Memory-Repositorys, die wir erstellt haben, sind Testdoubles für die Repositorys, die mit SQL Server kommunizieren. Wir haben gesehen, wie Sie verwenden diese Testdoubles während der Komponententests, Code zu isolieren, und halten die Tests, die schnell ausgeführt.

Der Test-Doubles an, die wir erstellt haben über real, funktionsfähiges Implementierungen verfügen. Hinter den Kulissen jeweils eine konkrete Auflistung von Objekten speichert, und sie hinzufügen und entfernen Sie Objekte aus dieser Auflistung, wie wir das Repository während eines Tests bearbeiten. Einige Entwickler möchten ihre Test-Doubles auf diese Weise – mit echten Code und arbeiten Implementierungen erstellen.  Diese Testdoubles werden nennen wir *fakes*. Weisen sie Arbeit Implementierungen, aber sie sind nicht ausreichend "realitätsnah" für die Produktion. Das fake-Repository nicht tatsächlich in die Datenbank geschrieben. Die gefälschte SMTP-Server nicht tatsächlich eine e-Mail-Nachricht über das Netzwerk gesendet werden.

### <a name="mocks-versus-fakes"></a>Mocks und Fakes

Es ist eine andere Art des Tests, die als double bekannt. eine *simulieren*. Während der Fakes arbeiten Implementierungen verfügen, enthalten Mocks keine Implementierung. Mithilfe eines pseudoobjektframeworks, wenn wir diese simulierte Objekte zur Laufzeit erstellen und als Test-Doubles verwenden können. In diesem Abschnitt wird die open Source simulationsframework Moq verwendet werden. Hier ist ein einfaches Beispiel mit Moq dynamisch einen Test für ein Repository für die Mitarbeiter doppelte erstellen.

``` csharp
    Mock<IRepository<Employee>> mock =
        new Mock<IRepository<Employee>>();
    IRepository<Employee> repository = mock.Object;
    repository.Add(new Employee());
    var employee = repository.FindById(1);
```

Wir bitten Moq IRepository&lt;Mitarbeiter&gt; eine Implementierung und dynamisch erstellt. Wir können auf das Objekt, das Implementieren von IRepository&lt;Mitarbeiter&gt; durch den Zugriff auf die Objekteigenschaft des Mock&lt;T&gt; Objekt. Es ist dieses interne Objekt, die wir in unserem Controller übergeben können, und sie wissen nicht, ist dies ein Testdouble oder die echten. Wir können die Methoden für das Objekt aufrufen, wie wir Aufrufen von Methoden auf ein Objekt mit einer echten Implementierung würde.

Sie müssen wissen, welche Aktionen das pseudorepository ausgeführt werden, wenn wir die Add-Methode aufrufen. Da keine Implementierung hinter das mock-Objekt ist, führt keine Aktion hinzufügen aus. Es gibt keine konkreten Sammlung hinter den Kulissen wie musste mit den Fakes, die wir geschrieben haben, die damit der Mitarbeiter verworfen wird. Was ist der Rückgabewert von FindById? In diesem Fall ist das mock-Objekt dem einzigen, was, das Sie tun kann, das zurückgegeben wird einen Standardwert. Da wir einen Verweistyp handelt (ein Mitarbeiter) zurückgeben, ist der Rückgabewert ein null-Wert.

Mocks mag nutzlos; Es gibt jedoch zwei weitere Funktionen Mocks, die wir gesprochen noch nicht. Das Framework Moq zeichnet zuerst alle Aufrufe für das mock-Objekt. Später im Code können wir Moq bitten, wenn jemand die Add-Methode aufgerufen, oder wenn jemand die FindById-Methode aufgerufen. Wir sehen die erläutert, wie wir dieses aufzeichnungsfeature "Blackbox" in Tests verwenden können.

Die zweite großartiges Feature ist, wie wir Moq verwenden können, ein mock-Objekt mit Programmieren *Erwartungen*. Eine Erwartung weist dem mock-Objekt zum Reagieren auf bestimmte eingreifen. Beispielsweise können wir Programmieren in unserem Mock eine Erwartung und anweisen, ein Employee-Objekt zurück, wenn jemand FindById aufruft. Das Moq-Framework verwendet ein Setup-API und Lambda-Ausdrücke, diese Erwartungen zu programmieren.

``` csharp
    [TestMethod]
    public void MockSample() {
        Mock<IRepository<Employee>> mock =
            new Mock<IRepository<Employee>>();
        mock.Setup(m => m.FindById(5))
            .Returns(new Employee {Id = 5});
        IRepository<Employee> repository = mock.Object;
        var employee = repository.FindById(5);
        Assert.IsTrue(employee.Id == 5);
    }
```

In diesem Beispiel bitten wir Moq, um dynamisch ein Repository zu erstellen, und klicken Sie dann das Repository mit der Erwartung programmieren. Der Annahme weist das mock-Objekt, ein neues Employee-Objekt mit Id-Wert 5 zurück, wenn jemand die übergeben des Werts 5 FindById-Methode aufruft. Dieser Test erfolgreich verläuft, und wir mussten erstellen Sie eine vollständige Implementierung, um gefälschte IRepository&lt;T&gt;.

Wir rufen Sie erneut auf die Tests, die wir zuvor geschrieben hat, und überarbeiten, um anstelle von Fakes Mocks zu verwenden. Genau wie verwenden zuvor eine Basisklasse wir die allgemeinen Teile der Infrastruktur einrichten, die für alle des Controllers Tests erforderlich.

``` csharp
    public class EmployeeControllerTestBase {
        public EmployeeControllerTestBase() {
            _employeeData = EmployeeObjectMother.CreateEmployees()
                                                .AsQueryable();
            _repository = new Mock<IRepository<Employee>>();
            _unitOfWork = new Mock<IUnitOfWork>();
            _unitOfWork.Setup(u => u.Employees)
                       .Returns(_repository.Object);
            _controller = new EmployeeController(_unitOfWork.Object);
        }

        protected IQueryable<Employee> _employeeData;
        protected Mock<IUnitOfWork> _unitOfWork;
        protected EmployeeController _controller;
        protected Mock<IRepository<Employee>> _repository;
    }
```

Durch den Setupcode bleibt größtenteils gleich. Anstatt Sie Fakes verwenden, verwenden wir Moq mock-Objekte erstellen. Die Basisklasse ordnet für das mock Arbeitseinheit, die ein pseudorepository zurückgegeben werden soll, wenn der Code wird aufgerufen, die Mitarbeiter-Eigenschaft an. Die restlichen das mock-Setup wird in der prüfvorrichtungen, die speziell für jeden bestimmten Szenario stattfinden. Die Testfixture für die Index-Aktion wird z. B. das pseudorepository, um eine Liste der Mitarbeiter zurückzugeben, wenn die Aktion die FindAll-Methode, der das pseudorepository aufruft eingerichtet.

``` csharp
    [TestClass]
    public class EmployeeControllerIndexActionTests
               : EmployeeControllerTestBase {
        public EmployeeControllerIndexActionTests() {
            _repository.Setup(r => r.FindAll())
                        .Returns(_employeeData);
        }
        // .. tests
        [TestMethod]
        public void ShouldBuildModelWithAllEmployees() {
            var result = _controller.Index();
            var model = result.ViewData.Model
                          as IEnumerable<Employee>;
            Assert.IsTrue(model.Count() == _employeeData.Count());
        }
        // .. and more tests
    }
```

Außer den Erwartungen ähneln sich unsere Tests für die Tests, die, denen wir vorher hatten. Allerdings können die Aufzeichnung Möglichkeit ein mock Framework wir Herangehensweisen zum Testen aus einem anderen Winkel. Wir werden im nächsten Abschnitt dieses neue Perspektive betrachten.

### <a name="state-versus-interaction-testing"></a>Status im Vergleich zur Interaktion testen

Es gibt verschiedene Techniken, die Sie zum Testen von Software mit Pseudoobjekten verwenden können. Ein Ansatz basiert auf Zustands testen, d.h., was wir in diesem Artikel bisher getan haben. Zustandsbasierte Test macht Assertionen über den Status der Software. In den letzten Test haben wir eine Aktionsmethode im Controller aufgerufen und versucht eine Assertion zum Modell, dass er erstellen soll. Hier sind einige weitere Beispiele für Testzustands:

-   Stellen Sie sicher, dass das Repository das neue Mitarbeiterobjekt enthält, nach dem Erstellen ausgeführt wird.
-   Stellen Sie sicher, dass das Modell enthält eine Liste aller Mitarbeiter nach Index-Anweisung ausführt.
-   Stellen Sie sicher, dass das Repository keinen Mitarbeiter enthält, nach dem Löschen wird ausgeführt.

Eine weitere Möglichkeit, die Ihnen mit Pseudoobjekten angezeigt wird, um zu überprüfen *Interaktionen*. Während der Zustandsbasierte testen macht Assertionen zu den Zustand von Objekten basierend Interaktion Test macht Assertionen über die Interaktion von Objekten an. Zum Beispiel:

-   Stellen Sie sicher, dass der Controller des Repository-Add-Methode aufruft, bei der Ausführung erstellen.
-   Stellen Sie sicher, dass der Controller des Repositorys FindAll-Methode ruft, wenn der Index-Anweisung ausführt.
-   Stellen Sie sicher, dass der Controller Ruft die Einheit der Arbeit des Commit-Methode, um Änderungen zu speichern, wenn Bearbeiten ausgeführt wird.

Testen der Interaktion erfordert häufig weniger Testdaten, da wir nicht innerhalb von Sammlungen Herumstochern und überprüfen die Anzahl. Z. B. wenn wir wissen, dass die Aktion für die Details des Repositorys eine FindById-Methode mit dem richtigen Wert - ruft ist dann die Aktion wahrscheinlich ordnungsgemäß verhält. Wir können dies überprüfen, ohne alle Testdaten aus FindById zurückgegeben einrichten zu müssen.

``` csharp
    [TestClass]
    public class EmployeeControllerDetailsActionTests
               : EmployeeControllerTestBase {
         // ...
        [TestMethod]
        public void ShouldInvokeRepositoryToFindEmployee() {
            var result = _controller.Details(_detailsId);
            _repository.Verify(r => r.FindById(_detailsId));
        }
        int _detailsId = 1;
    }
```

Das einzige Setup in der oben genannten Testfixture ist das Setup von der Basisklasse bereitgestellt. Wenn wir die Controlleraktion aufrufen, wird die Moq die Interaktionen mit das pseudorepository aufgezeichnet. Verwenden den überprüfen-API von Moq, können wir Moq bitten, wenn der Controller FindById mit der richtigen ID-Wert aufgerufen. Wenn der Controller nicht die Methode aufgerufen oder die Methode mit einem Wert unerwarteter Parameter aufgerufen, die Verify-Methode löst eine Ausnahme aus, und der Test schlägt fehl.

Hier ist ein weiteres Beispiel, um sicherzustellen, dass die Aktion "Create" Commit für die aktuelle Arbeitseinheit aufruft.

``` csharp
    [TestMethod]
    public void ShouldCommitUnitOfWork() {
        _controller.Create(_newEmployee);
        _unitOfWork.Verify(u => u.Commit());
    }
```

Eine Gefahr besteht, mit dem Testen der Interaktion ist die Tendenz, über die Interaktionen bestimmen. Die Fähigkeit zum Aufzeichnen und überprüfen, ob es sich bei jeder Interaktion mit dem Mockobjekt bedeutet, dass den Test nicht das mock-Objekt sollten versuchen, um zu überprüfen, ob jede Interaktion. Einige Aktivitäten sind Implementierungsdetails und sollten Sie nur überprüfen Sie, ob die Interaktionen *erforderlichen* um den aktuellen Test zu erfüllen.

Die Wahl zwischen Mocks oder Fakes hängt zum größten Teil des Systems, die Sie testen und Ihren persönlichen (oder Team) Einstellungen. Pseudoobjekten können die Menge des Codes, die Sie Test-Doubles implementieren müssen, erheblich senken, aber nicht jeder Programmierung Erwartungen, und überprüfen die Interaktionen vertraut ist.

## <a name="conclusions"></a>Zusammenfassung

In diesem Artikel haben wir verschiedene Ansätze zum Erstellen von testbarem Code bei der Verwendung von ADO.NET Entity Framework für Dauerhaftigkeit von Daten veranschaulicht. Wir können die integrierte Abstraktionen wie IObjectSet nutzen&lt;T&gt;, oder erstellen Sie eigene Abstraktionen wie IRepository&lt;T&gt;.  In beiden Fällen kann die POCO-Unterstützung in ADO.NET Entity Framework 4.0 der Consumer diese Abstraktionen persistent bleiben, ignorieren und leicht zu testendes. Zusätzliche EF4-Features wie Lazy Load implizite Geschäfts- und -Dienst ermöglicht es code arbeiten, ohne sich Gedanken über die Details eines relationalen Datenspeichers machen. Abschließend die von uns erstellte Abstraktionen sind einfach zu simulieren oder imitieren in Komponententests, und wir können diese Testdoubles schnell ausgeführt wird, hoch isoliert, erreichen und zuverlässige Tests verwenden.

### <a name="additional-resources"></a>Zusätzliche Ressourcen

-   Martin, " [Prinzip der einzigen Verantwortung](http://www.objectmentor.com/resources/articles/srp.pdf)"
-   Martin Fowler [Musterkatalog](http://www.martinfowler.com/eaaCatalog/index.html) aus *Muster der Unternehmensanwendungsarchitektur*
-   Griffin Caprio, " [Abhängigkeitsinjektion](https://msdn.microsoft.com/magazine/cc163739.aspx)"
-   Data Programmability-Blog " [Exemplarische Vorgehensweise: testgesteuerte Entwicklung mit Entitätsframework 4.0](https://blogs.msdn.com/adonet/pages/walkthrough-test-driven-development-with-the-entity-framework-4-0.aspx)".
-   Data Programmability-Blog " [mithilfe von Repository und Arbeitseinheit Muster mit Entity Framework 4.0](https://blogs.msdn.com/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)"
-   Dave Astels, " [BDD Einführung](http://blog.daveastels.com/files/BDD_Intro.pdf)"
-   Aaron Jensen, " [Einführung in die Spezifikationen des Computers](http://codebetter.com/blogs/aaron.jensen/archive/2008/05/08/introducing-machine-specifications-or-mspec-for-short.aspx)"
-   Eric Lee, " [BDD mit MSTest](https://blogs.msdn.com/elee/archive/2009/01/20/bdd-with-mstest.aspx)"
-   Eric Evans " [Domain-Driven Design](http://books.google.com/books?id=7dlaMs0SECsC&printsec=frontcover&dq=evans%20domain%20driven%20design&hl=en&ei=cHztS6C8KIaglAfA_dS1CA&sa=X&oi=book_result&ct=result&resnum=1&ved=0CCoQ6AEwAA)"
-   Martin Fowler " [Pseudoobjekte sind keine Stubs](http://martinfowler.com/articles/mocksArentStubs.html)"
-   Martin Fowler " [testen Double](http://martinfowler.com/bliki/TestDouble.html)"
-   Jeremy Miller " [Status im Vergleich zur Interaktion testen](http://codebetter.com/blogs/jeremy.miller/articles/129544.aspx)"
-   [Moq](http://code.google.com/p/moq/)

### <a name="biography"></a>Biografie

Scott Allen ist ein Mitglied des technischen Personals bei Pluralsight und Gründer von OdeToCode.com. In den 15 Jahren der Entwicklung von kommerziellen Software arbeitete Scott an Lösungen für alle Elemente aus 8-Bit-embedded-Geräte in hochgradig skalierbaren ASP.NET-Webanwendungen. Sie können Scott auf Twitter unter oder über seinen Blog unter OdeToCode erreichen [ http://twitter.com/OdeToCode ](http://twitter.com/OdeToCode).
