# <a name="contributing-to-the-entity-framework-documentation"></a><span data-ttu-id="f6e63-101">Mitwirken an der Entity Framework-Dokumentation</span><span class="sxs-lookup"><span data-stu-id="f6e63-101">Contributing to the Entity Framework documentation</span></span>

<span data-ttu-id="f6e63-102">Das Dokument beschreibt den Prozess bei der Mitwirkung an den Artikeln und Codebeispielen, die auf der Website [Entity Framework-Dokumentation](https://docs.microsoft.com/ef) gehostet werden.</span><span class="sxs-lookup"><span data-stu-id="f6e63-102">The document covers the process for contributing to the articles and code samples that are hosted on the [Entity Framework documentation site](https://docs.microsoft.com/ef).</span></span> <span data-ttu-id="f6e63-103">Die Beiträge können so einfach wie das Korrigieren von Tippfehlern oder so komplex wie das Verfassen neuer Artikel sein.</span><span class="sxs-lookup"><span data-stu-id="f6e63-103">Contributions may be as simple as typo corrections or as complex as new articles.</span></span>

## <a name="how-to-make-a-simple-correction-or-suggestion"></a><span data-ttu-id="f6e63-104">So nehmen Sie eine einfache Korrektur vor oder machen Sie einen einfachen Vorschlag</span><span class="sxs-lookup"><span data-stu-id="f6e63-104">How to make a simple correction or suggestion</span></span>

<span data-ttu-id="f6e63-105">Artikel werden als Markdowndateien im Repository gespeichert.</span><span class="sxs-lookup"><span data-stu-id="f6e63-105">Articles are stored in the repository as Markdown files.</span></span> <span data-ttu-id="f6e63-106">Einfache Änderungen am Inhalt einer Markdowndatei können im Browser vorgenommen werden, indem auf den Link **Bearbeiten** in der oberen rechten Ecke des Browserfensters geklickt wird.</span><span class="sxs-lookup"><span data-stu-id="f6e63-106">Simple changes to the content of a Markdown file can be made in the browser by tapping the **Edit** link in the upper right corner of the browser window.</span></span> <span data-ttu-id="f6e63-107">(Erweitern Sie in einem kleinen Browserfenster ggf. die Leiste **Optionen**, um den Link **Bearbeiten** sehen zu können.) Befolgen Sie die Anweisungen zum Erstellen eines Pull Requests (PR).</span><span class="sxs-lookup"><span data-stu-id="f6e63-107">(In narrow browser windows you might need to expand the **options** bar to see the **Edit** link.) Follow the directions to create a pull request (PR).</span></span> <span data-ttu-id="f6e63-108">Das EF-Team überprüft den PR und akzeptiert diesen oder schlägt Änderungen vor.</span><span class="sxs-lookup"><span data-stu-id="f6e63-108">The EF team will review the PR and accept it or suggest changes.</span></span>

## <a name="how-to-make-a-more-complex-submission"></a><span data-ttu-id="f6e63-109">So reichen Sie einen komplexeren Vorschlag ein</span><span class="sxs-lookup"><span data-stu-id="f6e63-109">How to make a more complex submission</span></span>

<span data-ttu-id="f6e63-110">Sie benötigen Grundkenntnisse zu [Git und GitHub.com](https://guides.github.com/activities/hello-world/).</span><span class="sxs-lookup"><span data-stu-id="f6e63-110">You'll need a basic understanding of [Git and GitHub.com](https://guides.github.com/activities/hello-world/).</span></span>

* <span data-ttu-id="f6e63-111">Öffnen Sie ein [Problem](https://github.com/aspnet/EntityFramework.Docs/issues/new) (Thema), in dem beschrieben ist, was Sie tun möchten, z. B. Ändern eines vorhandenen Artikels oder Erstellen eines neuen Artikels.</span><span class="sxs-lookup"><span data-stu-id="f6e63-111">Open an [issue](https://github.com/aspnet/EntityFramework.Docs/issues/new) describing what you want to do, such as change an existing article or create a new one.</span></span> <span data-ttu-id="f6e63-112">Bevor Sie zu viel Zeit investieren, warten Sie auf die Genehmigung des EF-Teams.</span><span class="sxs-lookup"><span data-stu-id="f6e63-112">Wait for approval from the EF team before you invest much time.</span></span>
* <span data-ttu-id="f6e63-113">Forken Sie das Repository [aspnet/EntityFramework.Docs](https://github.com/aspnet/EntityFramework.Docs/), und erstellen Sie einen Branch für Ihre Änderungen.</span><span class="sxs-lookup"><span data-stu-id="f6e63-113">Fork the [aspnet/EntityFramework.Docs](https://github.com/aspnet/EntityFramework.Docs/) repo and create a branch for your changes.</span></span>
* <span data-ttu-id="f6e63-114">Senden Sie einen Pull Request (PR) mit Ihren Änderungen an den Master.</span><span class="sxs-lookup"><span data-stu-id="f6e63-114">Submit a pull request (PR) to master with your changes.</span></span>
* <span data-ttu-id="f6e63-115">Reagieren Sie auf das PR-Feedback.</span><span class="sxs-lookup"><span data-stu-id="f6e63-115">Respond to PR feedback.</span></span>

## <a name="markdown-syntax"></a><span data-ttu-id="f6e63-116">Markdownsyntax</span><span class="sxs-lookup"><span data-stu-id="f6e63-116">Markdown syntax</span></span>

<span data-ttu-id="f6e63-117">Artikel werden in [DocFx-flavored Markdown](http://dotnet.github.io/docfx/spec/docfx_flavored_markdown.html) – eine Erweiterung von [GitHub-flavored Markdown (GFM)](https://guides.github.com/features/mastering-markdown/) – geschrieben.</span><span class="sxs-lookup"><span data-stu-id="f6e63-117">Articles are written in [DocFx-flavored Markdown](http://dotnet.github.io/docfx/spec/docfx_flavored_markdown.html), which is a superset of [GitHub-flavored Markdown (GFM)](https://guides.github.com/features/mastering-markdown/).</span></span> <span data-ttu-id="f6e63-118">Beispiele zur DFM-Syntax für Features der Benutzeroberfläche, die häufig in der EF-Dokumentation verwendet werden, finden Sie im Styleguide für das .NET Core-Repository unter [Metadaten- und Markdownvorlage](https://github.com/dotnet/docs/blob/master/styleguide/template.md).</span><span class="sxs-lookup"><span data-stu-id="f6e63-118">For examples of DFM syntax for UI features commonly used in the EF documentation, see [Metadata and Markdown Template](https://github.com/dotnet/docs/blob/master/styleguide/template.md) in the .NET Core repo style guide.</span></span> 

## <a name="folder-structure-conventions"></a><span data-ttu-id="f6e63-119">Konventionen für Ordnerstrukturen</span><span class="sxs-lookup"><span data-stu-id="f6e63-119">Folder structure conventions</span></span>

<span data-ttu-id="f6e63-120">Bilder und andere statische Inhalte werden in einem `_static`-Ordner in jedem Bereich/Ordner der Website gespeichert.</span><span class="sxs-lookup"><span data-stu-id="f6e63-120">Images, and other static content, are stored in an `_static` folder within each area/folder of the site.</span></span>

<span data-ttu-id="f6e63-121">Codebeispiele werden im Stammordner `samples` gespeichert.</span><span class="sxs-lookup"><span data-stu-id="f6e63-121">Code samples are stored in the `samples` root folder.</span></span> <span data-ttu-id="f6e63-122">Sie werden in einer Ordnerstruktur angeordnet, in der die Dokumentationsstruktur imitiert ist (zu finden unter dem Stammordner `entity-framework`).</span><span class="sxs-lookup"><span data-stu-id="f6e63-122">They are organized into a folder structure that mimics the documentation structure (found under the `entity-framework` root folder).</span></span>

## <a name="code-snippets"></a><span data-ttu-id="f6e63-123">Codeausschnitte</span><span class="sxs-lookup"><span data-stu-id="f6e63-123">Code snippets</span></span>

<span data-ttu-id="f6e63-124">Artikel enthalten häufig Codeausschnitte, um Argumente zu veranschaulichen.</span><span class="sxs-lookup"><span data-stu-id="f6e63-124">Articles frequently contain code snippets to illustrate points.</span></span> <span data-ttu-id="f6e63-125">In DFM können Sie Code in die Markdowndatei kopieren oder auf eine separate Codedatei verweisen.</span><span class="sxs-lookup"><span data-stu-id="f6e63-125">DFM lets you copy code into the Markdown file or refer to a separate code file.</span></span> <span data-ttu-id="f6e63-126">Wir bevorzugen möglichst die Verwendung separater Codedateien, um das Risiko von Fehlern im Code zu minimieren.</span><span class="sxs-lookup"><span data-stu-id="f6e63-126">We prefer to use separate code files whenever possible, to minimize the chance of errors in the code.</span></span> <span data-ttu-id="f6e63-127">Die Codedateien müssen im Repository mit der Ordnerstruktur gespeichert werden, die für die obigen Beispielprojekte beschrieben ist.</span><span class="sxs-lookup"><span data-stu-id="f6e63-127">The code files should be stored in the repo using the folder structure described above for sample projects.</span></span>

<span data-ttu-id="f6e63-128">Es folgen einige Beispiele für die [Syntax von DFM-Codeausschnitten](http://dotnet.github.io/docfx/spec/docfx_flavored_markdown.html#code-snippet).</span><span class="sxs-lookup"><span data-stu-id="f6e63-128">Here are some examples of [DFM code snippet syntax](http://dotnet.github.io/docfx/spec/docfx_flavored_markdown.html#code-snippet).</span></span>

<span data-ttu-id="f6e63-129">Um eine gesamte Codedatei als Ausschnitt zu rendern, führen Sie Folgendes aus:</span><span class="sxs-lookup"><span data-stu-id="f6e63-129">To render an entire code file as a snippet:</span></span>

``` none
[!code-csharp[Main](../../../samples/core/saving/Program.cs)]
```

<span data-ttu-id="f6e63-130">Um einen Teil einer Datei als Ausschnitt mittels Zeilennummern zu rendern, führen Sie Folgendes aus:</span><span class="sxs-lookup"><span data-stu-id="f6e63-130">To render a portion of a file as a snippet by using line numbers:</span></span>

``` none
[!code-csharp[Main](../../../samples/core/saving/Program.cs?range=1-10]
```

<span data-ttu-id="f6e63-131">Bei C#-Ausschnitten können Sie auf eine [C#-Region](https://msdn.microsoft.com/library/9a1ybwek.aspx) verweisen.</span><span class="sxs-lookup"><span data-stu-id="f6e63-131">For C# snippets, you can reference a [C# region](https://msdn.microsoft.com/library/9a1ybwek.aspx).</span></span> <span data-ttu-id="f6e63-132">Verwenden Sie nach Möglichkeit Regionen anstelle von Zeilennummern, da sich Zeilennummern in einer Codedatei in der Regel ändern und somit nicht mehr mit den Zeilennummernverweisen in der Markdowndatei übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="f6e63-132">Whenever possible, use regions rather than line numbers, because line numbers in a code file tend to change and get out of sync with line number references in Markdown.</span></span> <span data-ttu-id="f6e63-133">C#-Regionen können geschachtelt werden, und wenn Sie auf die äußere Region verweisen, werden die inneren Anweisungen `#region` und `#endregion` nicht in einem Ausschnitt gerendert.</span><span class="sxs-lookup"><span data-stu-id="f6e63-133">C# regions can be nested, and if you reference the outer region, the inner `#region` and `#endregion` directives are not rendered in a snippet.</span></span>

<span data-ttu-id="f6e63-134">Um eine C# Region namens „snippet_Example“ zu rendern, führen Sie Folgendes aus:</span><span class="sxs-lookup"><span data-stu-id="f6e63-134">To render a C# region named "snippet_Example":</span></span>

``` none
[!code-csharp[Main](../../../samples/core/saving/Program.cs?name=snippet_Example)]
```

<span data-ttu-id="f6e63-135">Um ausgewählte Zeilen in einem gerenderten Ausschnitt zu markieren (in der Regel mit gelber Hintergrundfarbe gerendert), führen Sie Folgendes aus:</span><span class="sxs-lookup"><span data-stu-id="f6e63-135">To highlight selected lines in a rendered snippet (usually renders as yellow background color):</span></span>

``` none
[!code-csharp[Main](../../../samples/core/saving/Program.cs?name=snippet_Example&highlight=1-3,10,20-25)]
```

## <a name="test-your-changes-with-docfx"></a><span data-ttu-id="f6e63-136">Testen Sie Ihre Änderungen mit DocFX</span><span class="sxs-lookup"><span data-stu-id="f6e63-136">Test your changes with DocFX</span></span>

<span data-ttu-id="f6e63-137">Testen Sie Ihre Änderungen mit dem [DocFX-Befehlszeilentool](https://dotnet.github.io/docfx/tutorial/docfx_getting_started.html#2-use-docfx-as-a-command-line-tool), wodurch eine lokal gehostete Version der Website erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="f6e63-137">Test your changes with the [DocFX command line tool](https://dotnet.github.io/docfx/tutorial/docfx_getting_started.html#2-use-docfx-as-a-command-line-tool), which creates a locally hosted version of the site.</span></span> <span data-ttu-id="f6e63-138">DocFX rendert keine Format- und Websiteerweiterungen, die für „docs.microsoft.com“ erstellt wurden.</span><span class="sxs-lookup"><span data-stu-id="f6e63-138">DocFX doesn't render style and site extensions created for docs.microsoft.com.</span></span>

<span data-ttu-id="f6e63-139">DocFX erfordert das .NET Framework für Windows oder Mono für Linux oder macOS.</span><span class="sxs-lookup"><span data-stu-id="f6e63-139">DocFX requires the .NET Framework on Windows, or Mono for Linux or macOS.</span></span>

### <a name="windows-instructions"></a><span data-ttu-id="f6e63-140">Anweisungen für Windows</span><span class="sxs-lookup"><span data-stu-id="f6e63-140">Windows instructions</span></span>

* <span data-ttu-id="f6e63-141">Laden Sie *docfx.zip* von der Seite [DocFX-Releases](https://github.com/dotnet/docfx/releases) herunter, und entpacken Sie die Datei.</span><span class="sxs-lookup"><span data-stu-id="f6e63-141">Download and unzip *docfx.zip* from [DocFX releases](https://github.com/dotnet/docfx/releases).</span></span>
* <span data-ttu-id="f6e63-142">Fügen Sie DocFX zu Ihrem PATH hinzu.</span><span class="sxs-lookup"><span data-stu-id="f6e63-142">Add DocFX to your PATH.</span></span>
* <span data-ttu-id="f6e63-143">Navigieren Sie in einem Befehlszeilenfenster zu dem geklonten Repository (das die Datei *docfx.json* enthält), und führen Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="f6e63-143">In a command line window, navigate to the cloned repository (which contains the *docfx.json* file) and run the following command:</span></span>

   ``` console
   docfx -t default --serve
   ```

* <span data-ttu-id="f6e63-144">Navigieren Sie in einem Browser zu `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="f6e63-144">In a browser, navigate to `http://localhost:8080`.</span></span>

### <a name="mono-instructions"></a><span data-ttu-id="f6e63-145">Anweisungen für Mono</span><span class="sxs-lookup"><span data-stu-id="f6e63-145">Mono instructions</span></span>

* <span data-ttu-id="f6e63-146">Installieren Sie Mono über Homebrew: `brew install mono`.</span><span class="sxs-lookup"><span data-stu-id="f6e63-146">Install Mono via Homebrew - `brew install mono`.</span></span>
* <span data-ttu-id="f6e63-147">Laden Sie die [aktuelle Version von DocFX](https://github.com/dotnet/docfx/releases/tag/v2.7.2) herunter.</span><span class="sxs-lookup"><span data-stu-id="f6e63-147">Download the [latest version of DocFX](https://github.com/dotnet/docfx/releases/tag/v2.7.2).</span></span>
* <span data-ttu-id="f6e63-148">Extrahieren Sie es unter `\bin\docfx`.</span><span class="sxs-lookup"><span data-stu-id="f6e63-148">Extract to `\bin\docfx`.</span></span>
* <span data-ttu-id="f6e63-149">Erstellen Sie einen Alias für **docfx**:</span><span class="sxs-lookup"><span data-stu-id="f6e63-149">Create an alias for **docfx**:</span></span>

  ``` console
  function docfx {
    mono $HOME/bin/docfx/docfx.exe
  }

  function docfx-serve {
    mono $HOME/bin/docfx/docfx.exe serve _site
  }
  ```

* <span data-ttu-id="f6e63-150">Führen Sie **docfx** im geklonten Repository aus, um die Website zu erstellen, und **docfx-serve**, um die Website unter `http://localhost:8080` anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="f6e63-150">Run **docfx** in the cloned repository to build the site, and **docfx-serve** to view the site at `http://localhost:8080`.</span></span>

## <a name="voice-and-tone"></a><span data-ttu-id="f6e63-151">Schreibstil</span><span class="sxs-lookup"><span data-stu-id="f6e63-151">Voice and tone</span></span>

<span data-ttu-id="f6e63-152">Unser Ziel ist es, Dokumentation bereitzustellen, die für eine größtmögliche Zielgruppe leicht verständlich ist.</span><span class="sxs-lookup"><span data-stu-id="f6e63-152">Our goal is to write documentation that is easily understandable by the widest possible audience.</span></span> <span data-ttu-id="f6e63-153">Zu diesem Zweck haben wir Richtlinien für den Schreibstil entwickelt, um deren Einhaltung wir unsere Beitragenden bitten.</span><span class="sxs-lookup"><span data-stu-id="f6e63-153">To that end we have established guidelines for writing style that we ask our contributors to follow.</span></span> <span data-ttu-id="f6e63-154">Weitere Informationen finden Sie im .NET Core-Repository unter [Voice and tone guidelines](https://github.com/dotnet/docs/blob/master/styleguide/voice-tone.md) (Richtlinien zum Schreibstil).</span><span class="sxs-lookup"><span data-stu-id="f6e63-154">For more information, see [Voice and tone guidelines](https://github.com/dotnet/docs/blob/master/styleguide/voice-tone.md) in the .NET Core repo.</span></span>
