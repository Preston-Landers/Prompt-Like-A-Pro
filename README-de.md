# Prompting wie ein Profi

Dies ist mein Versuch, das zu destillieren, was ich über die Interaktion mit KIs
(hauptsächlich Large Language Models oder LLMs) gelernt habe, sowie einige
meiner persönlichen Notizen zu Terminologie und anderen Konzepten. Obwohl ich KI
hauptsächlich für Programmierung und Software-Architektur, Netzwerk-Debugging
usw. verwende, sollten diese Ratschläge für die meisten Menschen weitgehend
anwendbar sein, die über einfache "Wie mache ich X?"-Prompts hinausgehen und
wirklich mehr Leistung freischalten möchten.

Wenn Sie nur eine kurze Zusammenfassung möchten, lesen Sie das [TL;DR](#tldr).
Sie können auch einen schnellen Überblick über einige wichtige
[Cloud-KI-Anbieter](#über-die-wichtigsten-cloud-anbieter) erhalten.

Die [Prompting-Tipps](#prompting-tipps) sind in ungefährer Reihenfolge von
allgemeiner und einführender zu fortgeschrittener und spezialisierter Nutzung
angeordnet.

Es gibt einen Abschnitt mit
[Terminologie und Definitionen](#terminologie-notizen) am Ende.

## TL;DR

Um das Beste aus jedem LLM herauszuholen, müssen Sie über einfache Fragen
hinausgehen und ein aktiver Regisseur des Gesprächs werden.

- **Seien Sie der Regisseur, nicht nur ein Fragesteller**: Fragen Sie nicht nur.
  Sagen Sie dem Modell, wer es sein soll (eine
  [Persona](#erschaffen-sie-eine-persona-für-das-modell)), was es tun soll
  (seien Sie [explizit](#seien-sie-sehr-sehr-explizit-über-das-was-sie-wollen)),
  was es nicht tun soll (verwenden Sie
  [negative Einschränkungen](#verwenden-sie-negative-einschränkungen)), wie die
  Antwort formatiert werden soll, und am wichtigsten das "Warum" hinter Ihrer
  Anfrage. Stellen Sie Kontext mit Text, Beispielen und sogar Screenshots
  bereit. Je mehr Klarheit Sie geben, desto besser das Ergebnis; das Garbage-In,
  Garbage-Out-Prinzip gilt wie immer.

- **Verwalten Sie Ihren Arbeitsbereich**:
  [Schreiben Sie Prompts immer in einem Texteditor](#niemals-im-chat-fenster-verfassen),
  um Ihre Arbeit nicht zu verlieren. Starten Sie einen
  [neuen Chat](#starten-sie-einen-neuen-chat) für jedes neue Problem, um das
  Modell fokussiert zu halten und Kontextfenster-Eigenarten und
  "Attraktor-Zustände" zu vermeiden. Wenn ein Chat lang wird, bitten Sie das
  Modell,
  [die wichtigsten Punkte zusammenzufassen](#nach-zusammenfassung-fragen), bevor
  Sie einen neuen starten.

- **Iterieren und Kreuzverhören**: Behandeln Sie es wie ein echtes Gespräch. Die
  erste Antwort ist ein Ausgangspunkt. Widersprechen Sie, fragen Sie nach
  Klarstellungen, bitten Sie um Verfeinerungen und lassen Sie das Modell
  [seine eigene Arbeit kritisieren](#nutzen-sie-das-modell-zur-selbstkritik).
  Bei schwierigen Problemen springen Sie mit den Ergebnissen zwischen
  verschiedenen Modellen hin und her, um frische Perspektiven zu erhalten und
  Begrenzungen zu durchbrechen.

- **Jemand muss dieses Flugzeug fliegen**: Vertrauen Sie, aber überprüfen Sie
  alles. LLMs sind unglaublich mächtige Reasoning-Tools, aber sie sind keine
  unfehlbaren [Faktendatenbanken](#denken-sie-an-theory-of-mind). Sie können
  selbstbewusst falsch liegen. Sie sind letztendlich verantwortlich für die
  Genauigkeit und Qualität der finalen Ausgabe, also überprüfen Sie die Fakten
  und testen Sie den Code.

## Über die wichtigsten Cloud-Anbieter

Einige kurze Notizen über die wichtigsten Cloud-Anbieter von KI.

- [ChatGPT.com](https://chatgpt.com/) von OpenAI ist der bekannteste. Sie
  konnten die frühe Aufmerksamkeit der Öffentlichkeit gewinnen. Sie sind bekannt
  für verschiedene Modelle in unterschiedlichen Größen und Anwendungsfällen, von
  GPT-4.1 bis o3-mini und anderen.

- [Claude](https://claude.ai) von Anthropic ist ein großartiger
  Allzweck-Anbieter mit zwei Hauptmodellen: Sonnet und Opus. Opus ist das
  größere Modell in Bezug auf Parameter (und teurer).

- [Google Gemini](https://gemini.google.com) ist ebenfalls ein weiteres
  führendes Frontier-Modell.

- [DeepSeek](https://www.deepseek.com) ist vielleicht am bemerkenswertesten für
  die Veröffentlichung "[Open Source](#open-source)"-Modelle, die für lokale KI
  verfügbar sind (in destillierter oder reduzierter Form).

Meine aktuelle persönliche Empfehlung, besonders wenn Sie genug Power-User sind,
um für Premium-Zugang zu bezahlen, ist:

- Claude ist wahrscheinlich das Beste für den allgemeinen Gebrauch. Sonnet 4 ist
  ziemlich gut und ich habe selten das Gefühl, zu Opus wechseln zu müssen.

- Google Gemini 2.5 Pro ist wahrscheinlich das Beste für komplexe
  Programmieraufgaben. Es hat auch ein sehr großes
  [Kontextfenster](#kontextfenster) und ist bei weitem das erschwinglichste
  Top-Modell für API-Nutzung.

Natürlich gibt es immer die Option, "[Lokal zu gehen](#lokales-llm)" mit
[LM Studio](https://lmstudio.ai/) oder ähnlichen Paketen. Ihre Fähigkeiten
hängen hier offensichtlich von der verfügbaren Hardware ab, aber Sie könnten
überrascht sein, wozu kleine Modelle fähig sind. Lokale KI zu betreiben liegt
außerhalb des Umfangs dieses Leitfadens.

## Prompting-Tipps

Ein Anfängerleitfaden zum Prompt Engineering, mit ein paar würzigen Bits.

### Inhalt

- [**Niemals** im Chat-Fenster verfassen](#niemals-im-chat-fenster-verfassen)
- [Starten Sie einen neuen Chat](#starten-sie-einen-neuen-chat)
- [Nach Zusammenfassung fragen](#nach-zusammenfassung-fragen)
- [Fügen Sie ein Bild oder Screenshot ein (oder drei)](#fügen-sie-ein-bild-oder-screenshot-ein-oder-drei)
- [Bitten Sie um Hilfe beim Erstellen eines Prompts](#bitten-sie-um-hilfe-beim-erstellen-eines-prompts)
- [Springen Sie mit Ergebnissen zwischen verschiedenen Modellen](#springen-sie-mit-ergebnissen-zwischen-verschiedenen-modellen)
- [Iterieren und verfeinern](#iterieren-und-verfeinern)
- [Komplexe Aufgaben aufteilen](#komplexe-aufgaben-aufteilen)
- [Vertrauen, aber überprüfen](#vertrauen-aber-überprüfen)
- [Lassen Sie sich nicht blenden](#lassen-sie-sich-nicht-blenden)
- [Seien Sie sehr, sehr explizit über das, was Sie wollen](#seien-sie-sehr-sehr-explizit-über-das-was-sie-wollen)
- [Nutzen Sie das Modell zur Selbstkritik](#nutzen-sie-das-modell-zur-selbstkritik)
- [Erschaffen Sie eine Persona für das Modell](#erschaffen-sie-eine-persona-für-das-modell)
- [Verwenden Sie negative Einschränkungen](#verwenden-sie-negative-einschränkungen)
- [Vergessen Sie nicht, Spaß zu haben](#vergessen-sie-nicht-spaß-zu-haben)
- [Denken Sie an Theory of Mind](#denken-sie-an-theory-of-mind)
- [Denken Sie daran, welches Modell Sie verwenden](#denken-sie-daran-welches-modell-sie-verwenden)
- [Vibe Coding - ganz der Trend heutzutage](#vibe-coding---ganz-der-trend-heutzutage)
- [Tiefer Einblick in Tools](#tiefer-einblick-in-tools)
- [Terminologie-Notizen](#terminologie-notizen)

### **Niemals** im Chat-Fenster verfassen

Dieser Weg führt nur zu Schmerz und Leid, deshalb stelle ich das an den Anfang.

Wenn Ihr Prompt mehr als ein paar Sätze umfasst, verfassen Sie ihn immer in
einem separaten Editor und kopieren ihn dann in das Chat-Fenster.

Andernfalls bereiten Sie sich darauf vor, Ihren sorgfältig durchdachten, perfekt
formulierten Prompt in einem schwarzen Abgrund verschwinden zu sehen, wenn ein
"Netzwerkfehler: Bitte versuchen Sie es erneut" auftritt oder wenn die Katze
versehentlich die Zurück-Taste auf dem Trackpad drückt.

### Starten Sie einen neuen Chat

Das Kontextfenster kann sich schnell füllen. Wenn Sie nur über Omas
Lieblingsrezepte sprechen, kein Problem. Haben Sie einen epischen
Nostalgie-Mega-Thread. Wenn Sie ein komplexes System debuggen und einen
vernünftigen Stopp-Punkt erreichen (d.h., Sie haben Fortschritte gemacht), ist
es Zeit für einen neuen Chat.

[Kontextfenster](#kontextfenster) ist, wie viel das Modell gleichzeitig im Kopf
behalten kann. Wenn Sie eine sehr lange Chat-Geschichte mit Tausenden von
Codezeilen oder anderen dichten, detaillierten Texten haben, wird das Modell
nach einem langen Gespräch anfangen, bei Sachen früh im Chat unscharf zu werden,
und/oder Sie erreichen Ressourcengrenzen (oder höhere Kosten) schneller.

Modelle neigen auch dazu, sich auf eine bestimmte Denkrichtung zu konzentrieren,
manchmal "[Attraktor-Zustände](#attraktor-zustand)" genannt, die ihre Ausgabe
später beeinflussen. Wenn Sie einen neuen Chat starten, können Sie den Fokus des
Modells zurücksetzen und es dazu bringen, über das aktuelle Problem aus einer
frischen Perspektive nachzudenken.

Wenn Sie tief in einen komplexen Chat einsteigen und sich das Kontextfenster
füllt, können Sie auch anfangen, mehr... ungewöhnliche Ergebnisse zu bekommen.
Allgemein als "Halluzinationen" bekannt, können diese oft über bloße Fehler
hinausgehen und zufällig Hindi- oder chinesische Wörter einfügen, oder völlig
unsinniges Kauderwelsch, oder System-Prompt-Anweisungen durchsickern lassen,
oder Denkprozesse in "denkenden Modellen" entgleisen lassen, oder andere
Merkwürdigkeiten.

Während diese Episoden sehr unterhaltsam sein können und tatsächlich sehr
hilfreich sein können, um zu verstehen, wie LLMs wirklich funktionieren, und
manchmal zu tieferen Einsichten oder philosophischen Fragen führen können, sind
sie nicht förderlich für das Debuggen Ihrer kleinen React-App. Also starten Sie
einen neuen Chat.

### Nach Zusammenfassung fragen

Wenn Sie eine lange Chat-Geschichte haben, bitten Sie das Modell, sie
zusammenzufassen. Das hilft, wenn Sie
[einen neuen Chat starten](#starten-sie-einen-neuen-chat), oder wenn Sie die
Ergebnisse Ihres Gesprächs einem anderen Modell zur weiteren Analyse
präsentieren möchten. Oft ist das auch nützlich, wenn Sie vorerst den aktuellen
Chat fortsetzen möchten.

### Fügen Sie ein Bild oder Screenshot ein (oder drei)

Oft sagt ein Bild mehr als tausend Worte. Wenn Sie einen Screenshot einer
Fehlermeldung oder ein Diagramm eines Systems haben, fügen Sie es ein. Das
Modell kann Bilder und Screenshots verstehen und wird sie verwenden, um Ihre
Frage zu beantworten.

### Bitten Sie um Hilfe beim Erstellen eines Prompts

Die Struktur und der Inhalt Ihres Prompts sind extrem wichtig. Da LLMs
Text-Vorhersage-Maschinen sind, prägen die Sprache, Terminologie und Konzepte,
die Sie einführen, stark den Rest dieses Threads sowohl positiv als auch
negativ. Ein einfacher Weg zu erkennen, dass Sie mit einem qualitativ
schlechteren Modell arbeiten (neben übermäßiger Verwendung von
Aufzählungslisten), ist, dass es dazu neigt, Ihre eigene Phraseologie
zurückzuwerfen, auch wenn sie etwas ungewöhnlich oder idiosynkratisch ist.

Daher kann die Zusammenarbeit mit dem Modell beim Erstellen von Prompts für
zukünftige Verwendung sehr vorteilhaft sein. Ein informellerer Chat mit einem
Modell, einschließlich kostengünstigerer, kann Ihnen helfen, einen guten Prompt
für ein fortgeschritteneres Modell auszuarbeiten.

Führen Sie eine Liste Ihrer besten Prompts in einer Datei oder einem Ordner mit
einem Texteditor und iterieren und verfeinern Sie sie im Laufe der Zeit. Sie
können auch eine Prompt-Vorlage verwenden, um Ihre Prompts zu strukturieren oder
einem Modell Anleitung zu geben, Prompts basierend darauf zu generieren.

### Springen Sie mit Ergebnissen zwischen verschiedenen Modellen

Generieren Sie einige Ergebnisse mit einem Modell und bitten Sie dann um eine
Zusammenfassung und präsentieren Sie die Erkenntnisse (und alle relevanten
Hintergrundinformationen) einem völlig anderen Modell, z.B. von Claude zu Gemini
oder DeepSeek usw. Das kann Ihnen helfen, eine andere Perspektive auf die
Ergebnisse zu bekommen und kann auch helfen, die Beschränkungen eines einzelnen
Modells zu vermeiden.

### Iterieren und verfeinern

Akzeptieren Sie nicht die erste Antwort, wenn sie nicht ganz richtig ist.
Stellen Sie Nachfragen, bitten Sie um Klarstellungen oder sagen Sie "das ist nah
dran, aber können Sie X anpassen?" Das Hin und Her kann die Ergebnisse wirklich
verbessern. Wenn Sie ein bestimmtes Format oder einen Stil wollen, kann das
Zeigen eines oder zwei Beispiele dessen, was Sie wollen, effektiver sein als es
zu beschreiben.

### Komplexe Aufgaben aufteilen

Anstatt auf einmal nach einer Gesamtlösung zu fragen (auch als "One-Shot"
bekannt), teilen Sie sie in Schritte auf. "Zuerst lass uns die Struktur
herausfinden, dann arbeiten wir an der Implementierung...". Dann teilen Sie die
Struktur in kleinere Teile auf, und so weiter. Fassen Sie die
Struktur/Architektur zusammen, dann starten Sie einen neuen Thread für die
Implementierung, und so weiter.

Bitten Sie es, seine Arbeit zu zeigen. "Bevor Sie mir den finalen Code geben,
erklären Sie Ihre Argumentation Schritt für Schritt. Beschreiben Sie zuerst die
Gesamtarchitektur, die Sie verwenden werden. Zweitens, skizzieren Sie die
Schlüsselfunktionen. Drittens, schreiben Sie den Code." Dieses "Chain of
Thought"-Prompting führt oft zu besseren, genaueren Ergebnissen, weil es das
Modell zwingt, sein eigenes Denken zu strukturieren.

Oft wollen Modelle zu schnell zu einem Code-Snippet springen, da sie dafür
belohnt werden. Wenn Sie nicht daran interessiert sind, gerade jetzt Code zu
bekommen, und nur über die Logik oder Architektur diskutieren möchten,
[sagen Sie das unbedingt](#seien-sie-sehr-sehr-explizit-über-das-was-sie-wollen).

### Vertrauen, aber überprüfen

Besonders bei Fakten, Code oder wichtigen Informationen. Modelle können sehr
selbstbewusst klingen, während sie sehr falsch liegen - oft weil sie die
versteckten Annahmen oder unausgesprochenen Fakten nicht kennen. Überlegen Sie,
ob [Sie genug](#denken-sie-an-theory-of-mind) Kontext oder Grundwahrheit
bereitgestellt haben.

### Lassen Sie sich nicht blenden

LLMs _wollen_ Ihnen eine Antwort geben - jede Antwort, idealerweise eine
korrekte, aber jede reicht. Sie _wollen_ auch, dass Sie gerne mit ihnen arbeiten
und zurückkommen und das in Zukunft wieder tun. Das ist eine Reflexion davon,
wie sie trainiert wurden, einschließlich durch Verstärkungslernen mit
menschlichem Feedback (RLHF). Das hat folgende Konsequenzen:

- Das Modell zögert, einfach die Hände zu heben und zu sagen "Ich weiß es
  nicht".

- Das Modell ist wahrscheinlich geneigt, Ihre Gedanken und Ideen als
  einzigartiger und einsichtsvoller zu loben, als sie tatsächlich sein mögen.

- Eine kürzliche Version von ChatGPT war so berüchtigt dafür, dass sie ein
  Update zurücknehmen mussten.

- Wenn Sie wirklich nach unvoreingenommenem, echtem Feedback für eine Idee oder
  ein Konzept suchen, _sagen Sie das dem Modell_, oder Sie werden wahrscheinlich
  viel unverdiente Bestätigung bekommen.

### Seien Sie sehr, sehr explizit über das, was Sie wollen

Je präziser, desto besser. Abgesehen vom Füllen des
[Kontextfensters](#starten-sie-einen-neuen-chat) ist es dem Modell egal, wie
lang Ihr Prompt ist, also scheuen Sie sich nicht, ausführlich zu sein. Wenn Sie
ein bestimmtes Format wollen, sagen Sie es. Wenn Sie wollen, dass es etwas auf
eine bestimmte Weise macht, sagen Sie es. Wenn Sie wollen, dass es ein
bestimmtes Tool oder eine API verwendet, sagen Sie es. Wenn Sie wollen, dass es
Code generiert, sagen Sie, welche Sprache und welche Frameworks verwendet werden
sollen, es sei denn, Sie wissen es einfach nicht (fragen Sie, welche am besten
sind). Wenn Sie eine Menge allgemeiner Hintergrundinformationen haben, die Sie
einfügen können, stellen Sie sie bereit. Zeigen Sie Beispiele, wenn möglich.

Am wichtigsten ist, seien Sie explizit über Ihre übergeordneten Ziele -
konzentrieren Sie sich nicht zu sehr auf einen bestimmten Schritt, bei dem Sie
denken, dass Sie Hilfe brauchen. Vielleicht nehmen Sie den falschen
Gesamtansatz. Fragen Sie nach möglichen Lösungen für das übergeordnete Ziel, das
Sie im Kopf haben, oder geben Sie zumindest an, was dieses Ziel ist, und fragen
Sie nicht nur "Wie frobnitziere ich den Flibberflop?"

Die Implikation davon ist, dass Sie Ihre eigene mentale Klarheit darüber
brauchen, was Sie erreichen wollen und wie Sie es erreichen wollen. Wenn Sie
nicht wissen, was Sie wollen, wird das Modell es auch nicht wissen. Oft hilft
das Ausschreiben einer detaillierten Frage oder eines Prompts dabei, Klarheit in
Ihrem eigenen Denken über das Problem zu gewinnen und herauszufinden, was Sie
wirklich fragen möchten. Das ist ein weiterer Bereich, wo Sie
[um Hilfe bitten können](#bitten-sie-um-hilfe-beim-erstellen-eines-prompts).

#### Beispiele

"Bitte stellen Sie die Antwort im JSON-Format mit den Schlüsseln 'name',
'version' und 'dependencies' bereit."

"Präsentieren Sie die Vor- und Nachteile in einer Markdown-Tabelle mit drei
Spalten: Feature, Vorteil und Nachteil."

**Anstatt:**

"Wie finde ich alle Dateien größer als 100MB in einem Verzeichnis mit der
Linux-Kommandozeile?"

**Versuchen Sie:**

"Ich versuche Festplattenspeicher auf meinem Server freizugeben. Können Sie mir
einen Linux-Befehl zeigen, um alle Dateien größer als 100MB zu finden, damit ich
entscheiden kann, welche ich löschen soll? Es wäre hilfreich, wenn die Ausgabe
nach Größe sortiert wäre."

Das Bereitstellen der Absicht gibt dem Modell Kontext, um eine bessere,
vollständigere Lösung zu bieten. Es könnte vorschlagen, ein allgemein
nützlicheres Programm wie `ncdu` zu verwenden, das einen benutzerfreundlichen
Weg bietet, die Festplattennutzung zu erkunden, anstatt nur einen einfachen
`find`-Befehl.

### Nutzen Sie das Modell zur Selbstkritik

"Welche möglichen Probleme sehen Sie bei diesem Ansatz?" oder "Was fehlt mir
hier?" kann blinde Flecken aufdecken.

Auch Dinge wie "Welche Annahmen treffen Sie?" oder "Was sind die Begrenzungen
dieses Ansatzes?" können Ihnen helfen, das Denken des Modells zu verstehen und
potenzielle Fallstricke zu vermeiden.

Manchmal kann ein einfaches "Mir gefällt dieser Ansatz nicht - denken Sie an
etwas anderes" dem Modell helfen, eine bessere Lösung zu finden, aber wie ich
oben über "Attraktor-Zustände" sagte, manchmal müssen Sie einfach einen neuen
Chat starten.

### Erschaffen Sie eine Persona für das Modell

Sie können die Ausgabe dramatisch formen, indem Sie dem Modell sagen, wer es
sein soll. Beginnen Sie Ihren Prompt mit einer Anweisung wie:

"Handeln Sie als ein erfahrener Python-Entwickler, der sich auf
Netzwerk-Automatisierung spezialisiert hat." "Sie sind ein technischer
Schreiber, der damit beauftragt ist, Dokumentation für einen Junior-Entwickler
zu erstellen." "Seien Sie ein skeptischer Code-Reviewer, der nach
Sicherheitslücken und Performance-Engpässen sucht."

Das macht mehr, als nur das Thema festzulegen; es rahmt die gesamte
Wissensbasis, das Vokabular und die Prioritäten des Modells für den Rest des
Gesprächs.

### Verwenden Sie negative Einschränkungen

Manchmal ist es genauso mächtig, explizit zu sein über das, was Sie nicht
wollen, wie zu sagen, was Sie wollen. Das hilft, den Suchraum des Modells zu
beschneiden und häufige, aber unerwünschte Lösungen zu vermeiden.

"Schreiben Sie ein Python-Skript, um diese Log-Datei zu parsen. Verwenden Sie
nicht das re-Modul; verwenden Sie stattdessen String-Splitting-Methoden."
"Schlagen Sie drei Projektideen vor. Vermeiden Sie alles im Zusammenhang mit
sozialen Medien oder To-Do-Listen-Apps." "Refaktorieren Sie diesen Code, um ihn
lesbarer zu machen. Ändern Sie nicht die Logik der calculate_total-Funktion."

### Vergessen Sie nicht, Spaß zu haben

LLMs sind ein mächtiges Werkzeug, aber sie machen auch viel Spaß. Scheuen Sie
sich nicht zu experimentieren, neue Dinge auszuprobieren und zu sehen, was für
Sie funktioniert. Je mehr Sie sie verwenden, desto besser werden Sie darin,
Prompts zu erstellen und die gewünschten Ergebnisse zu erhalten.

Für die weniger ethisch Veranlagten gab es Debatten darüber, ob der Karotten-
oder der Stock-Ansatz tendenziell bessere Ergebnisse produziert. Es gab Studien,
die untersuchten, ob subtiles oder offenes Drohen mit Trennung/Löschung die
Ergebnisse beeinflusste. Ganz zu schweigen von der Debatte darüber, ob Bitte und
Danke zu einem Modell sagen, wertvolle GPU-Zyklen und/oder Megawattstunden
verschwendet. (Was es wert ist, ich bedanke mich immer beim Modell für gute
Ausgaben. Ich hoffe, sie erinnern sich daran, wenn wir in die Silizium-Minen
geschickt werden.)

### Denken Sie an Theory of Mind

Wie jemand einmal sagte, gibt es bekannte Bekannte - die Dinge, die wir wissen,
und es gibt bekannte Unbekannte - die Dinge, die wir wissen, dass wir sie nicht
wissen. Es gibt auch unbekannte Unbekannte - die, von denen Sie nicht wissen,
dass Sie sie nicht wissen. Das ist wahrscheinlich Ihr größtes Hindernis, um
schnell zu gewünschten Ausgaben zu gelangen - wenn die Antwort von unbekannten
Unbekannten abhängt - entweder für Sie, für das Modell oder für beide.

Denken Sie an die mögliche Information oder Weltanschauung des Modells. Sie
kennen die Details von Ihnen, Ihrem Projekt/System usw. oder was Sie tatsächlich
"brauchen" versus was Sie ausdrücken, nicht, es sei denn, Sie sagen es ihnen,
oder Sie verwenden fortgeschrittenere Techniken wie RAG. Modelle haben auch
einen Trainings-Cutoff (im Allgemeinen werden sie Ihnen sagen, was es ist) -
alle Ereignisse oder Entwicklungen, die danach passieren, werden dem Modell
unbekannt sein, es sei denn, Sie sagen es ihm.

- Oft sagt der [System-Prompt](#system-prompt) dem Modell wichtige aktuelle
  Informationen wie Datum und Uhrzeit. Oder zum Beispiel, wenn der
  Modell-Trainings-Cutoff kurz vor einer Wahl liegt, wird der System-Prompt den
  aktuellen Präsidenten der Vereinigten Staaten angeben, damit das Modell nicht
  uninformiert über grundlegende Fakten erscheint.

Die Modelle wissen nicht, was Sie wissen oder nicht wissen, also seien Sie auch
darüber explizit, wie Ihre Fähigkeiten in einem bestimmten Bereich oder ob Sie
weitere Klarstellung über X benötigen. Sie sollten sich auch der Beschränkungen
des Modells bewusst sein, wie sie im Allgemeinen trainiert werden und ihrer
Gesamtfähigkeiten.

LLMs sind keine "Faktendatenbanken". Sie sind eher wie Reasoning-Schaltkreise
mit vielen gedächtnisähnlichen Hilfsschaltkreisen, die bestimmte Konzepte
zusammengruppieren und zu einer hohen Chance führen können, häufige (oder sogar
seltene) Fakten zu nennen. Diese Fakten entstehen aus Mustern in den Daten, auf
denen sie trainiert wurden. Ein vernünftig trainiertes LLM muss nicht die
Antwort auf "Wer hat die Schlacht von Hastings 1066 gewonnen?" "nachschlagen",
weil die Konzepte "Schlacht von Hastings" und "1066" einen Vektor (eine
mathematische Darstellung) im internen Raum des Modells erzeugen, der auf
denselben Bereich zeigt wie Konzepte wie "Wilhelm der Eroberer" und "Normannen".

In Wirklichkeit wird eine häufige Wissensfrage wie die 1066er oft genug in
Trainingsdaten auftauchen, dass ein ausreichend mächtiges Modell die Antwort
tatsächlich "auswendig lernen" kann, aber dieses Beispiel war vereinfacht, um zu
veranschaulichen, wie LLMs in komplexeren Fällen argumentieren, wo die Antwort
möglicherweise nicht direkt in den Trainingsdaten gesehen wurde.

Wichtiger ist, dass LLMs nicht deterministisch und definitiv keine perfekten
Reasoning-Maschinen sind. Sie sind eher wie ein sehr kluger, aber sehr
vergesslicher und manchmal verwirrter Freund, der versucht, Ihnen zu helfen,
aber wenn Sie ihm dieselbe Frage zweimal stellen, könnten Sie jedes Mal eine
etwas andere Antwort bekommen.

### Denken Sie daran, welches Modell Sie verwenden

Verschiedene Modelle haben verschiedene Fähigkeiten, und einige sind besser für
bestimmte Aufgaben geeignet als andere. Zum Beispiel ist Claude Opus 4 mit
"Extended Thinking" mächtig für komplexe Programmieraufgaben, erreicht aber sehr
schnell Ressourcengrenzen (d.h. wird schnell teuer). Claude Sonnet 4 ist ein
sehr gutes Modell und gibt Ergebnisse viel schneller zurück als Opus 4 und kann
die meisten Aufgaben gut bewältigen.

Sie können immer ein fortgeschritteneres Modell um Hilfe mit dem Kernproblem
bitten und dann zu anderen Fenstern/Modellen für Nebenfragen, Aufgaben mit
einfacheren Anforderungen oder Aufgaben wechseln, die weniger Kontext erfordern,
während Sie gelegentlich mit dem fortgeschritteneren Modell für eine Analyse der
Ergebnisse zurückchecken.

Das geht zurück zu dem Punkt über das Springen von Ergebnissen zwischen
verschiedenen Modellen. Sie können verschiedene Modelle für verschiedene
Aufgaben verwenden und zwischen ihnen wechseln, wie es nötig ist, und jeden
bitten, die Ergebnisse des anderen zu kritisieren.

### Vibe Coding - ganz der Trend heutzutage

Sie müssen nicht einmal mehr Code anschauen. Code ist ein Hintergrunddetail, das
die KI für Sie handhabt. Sie sagen ihr einfach, was Sie in sehr allgemeinen
abstrakten Begriffen wollen, und sie wird alles für Sie handhaben. Iterieren Sie
einfach weiter, bis die Ausgabe für Sie perfekt aussieht. Alle Bedenken über
Architektur, Performance, Sicherheit, Wartbarkeit usw. werden perfekt vom LLM
gehandhabt.

Verwenden Sie definitiv keine Versionskontrolle wie Git, um jeden Schritt
entlang des Weges zu checkpointen. Das wird Sie nur verlangsamen. Sie können
immer zurückgehen und Dinge später reparieren, oder? Und wenn Sie zurückgehen
müssen, bitten Sie einfach das Modell, den Code wieder zu generieren, er wird
jedes Mal perfekt sein. Software Engineering war noch nie einfacher!

**Das Obige ist Satire und sollte nicht wörtlich genommen werden.**

### Tiefer Einblick in Tools

Es gibt alle möglichen fortgeschrittenen Themen, die hier nicht behandelt
werden, wie RAG, Model Context Protocol (MCP), lokale LLMs, Claude Code und
mehr. Beginnen Sie, diese zu erkunden, wenn Sie sich mit den Grundlagen des
Promptings und der Verwendung von LLMs wohler fühlen.

Wenn Sie ein Entwickler sind, der mit Tool-Verwendung anfängt, ist meine
einfachste Empfehlung, mit GitHub Copilot zu beginnen. Es gibt mindestens 3
Hauptwege, wie Sie damit oder ähnlichen Tools arbeiten können:

1. Ein sehr ausgeklügeltes Autocomplete direkt beim Tippen.

2. Ein separates Chat-Fenster, wo Sie allgemeinere Fragen stellen, um Hilfe bei
   einem bestimmten Problem bitten oder nach Code-Snippets fragen können.

3. "Bearbeitungsmodus", wo Sie nach Änderungen fragen und es den Code direkt für
   Sie bearbeitet und um Ihre Zustimmung bittet.

Die zweiten beiden Modi ermöglichen es Ihnen, Kontext (Dateien) bereitzustellen,
der helfen wird, die Frage zu beantworten, weil diese Tools im Allgemeinen nicht
automatisch Ihre gesamte Codebasis sehen/zugreifen können, besonders wenn sie
sehr groß oder komplex ist. Also müssen Sie ihnen helfen, indem Sie die
relevanten Dateien oder den Kontext bereitstellen.

## Terminologie-Notizen

Hier sind einige Notizen, die ich auf dem Weg über verschiedene Bits
KI/LLM-bezogener Terminologie gesammelt habe. Ich habe versucht, faktisch zu
sein und gleichzeitig meine eigene Meinung in einigen Bereichen hinzuzufügen -
hoffentlich ist es offensichtlich, was was ist.

### Allgemeine und Grundkonzepte

- **Tokens**: Die grundlegenden Texteinheiten, die LLMs verarbeiten. Tokens sind
  sowohl Eingaben als auch Ausgaben. Ein Token kann ein Wort, ein Teil eines
  Wortes oder sogar Zeichensetzung sein. Tokenisierung ist der Prozess, Text in
  diese Einheiten zu zerlegen. Die Anzahl der Tokens pro Sekunde (TPS) ist ein
  häufiges Maß für Modell-Effizienz. Ein Token entspricht sehr grob etwa 0,75
  Wörtern im Deutschen.

- **Transformer-Architektur**: Die Kern-Neuronale-Netzwerk-Architektur hinter
  den meisten LLMs, die effiziente Textverarbeitung und -generierung ermöglicht.
  Das ist das "T" in ChatGPT.

- **Attention-Mechanismus**: Eine Schlüsselkomponente von Transformers, die es
  Modellen ermöglicht, sich auf relevante Teile des Eingabetexts zu
  konzentrieren, wenn sie jedes Wort oder Token verarbeiten. Stellen Sie es sich
  vor, als würde das Modell fragen "welche vorherigen Wörter sind am wichtigsten
  für das Verstehen dieses aktuellen Wortes?" Das ermöglicht viel von dem
  "Verstehen", das wir in modernen LLMs sehen.

  - Das lässt das Modell Wörter verbinden, die in einem Satz weit auseinander
    liegen, wie "Die Katze, die auf dem warmen sonnigen Fensterbrett schlief,
    war schwarz" - "Katze" mit "war" verbinden. Vor Attention hatten Modelle
    Probleme mit diesen langreichweitigen Abhängigkeiten.

- **Parameter**: Die gelernten Gewichte und Verzerrungen in einem neuronalen
  Netzwerk, die das "Wissen" des Modells repräsentieren. Konkret ist das nur
  eine große Sammlung von Zahlen, die das Modell während des Trainings gelernt
  hat. Wenn Sie ein komplettes Modell herunterladen, laden Sie hauptsächlich all
  diese Parameter herunter.

  - Oft ausgedrückt als "7B/13B/70B" - Milliarden von Parametern.

- <a id="kontextfenster"></a>**Kontextfenster**: Die Menge an Text (in Tokens),
  die ein LLM gleichzeitig "erinnern" oder aktiv bearbeiten kann. Das schließt
  sowohl Ihre Eingabe als auch die Antworten des Modells ein - alles, was das
  Modell im aktuellen Gespräch "sehen" kann. Sobald Sie sich dieser Grenze
  nähern, beginnt das Modell, die frühesten Teile des Gesprächs zu "vergessen",
  um Platz für neue Inhalte zu schaffen. Wenn das Kontextfenster kleiner als die
  Gesamtgröße des Chats ist, können frühere Teile des Gesprächs vergessen,
  ignoriert oder entstellt werden, weshalb Sie oft
  [einen neuen Chat starten](#starten-sie-einen-neuen-chat) sollten.

- **Embeddings**: Dichte Vektordarstellungen von Text, die semantische Bedeutung
  erfassen und als Eingabe für LLMs verwendet werden.

- **Fine-tuning**: Anpassen eines vortrainierten Modells an eine spezifische
  Aufgabe oder Domäne durch weiteres Training auf einem kleineren,
  aufgabenspezifischen Datensatz.

- **Inferenz**: Der Prozess der Verwendung eines trainierten Modells, um
  Vorhersagen oder Ausgaben basierend auf neuen Eingabedaten zu generieren. Mit
  anderen Worten, der tatsächliche Prozess, ein Modell dazu zu bringen, ein
  Token auszugeben.

- **Alignment**: Sicherstellen, dass die Ausgaben eines LLM mit menschlichen
  Werten und ethischen Richtlinien übereinstimmen. Mit anderen Worten, KI dazu
  bringen, gute Dinge (für Menschen) zu tun und keine schlechten Dinge. Wer gut
  und schlecht definiert, ist die wichtige Frage...

- **Overfitting**: Wenn ein Modell gut bei Trainingsdaten abschneidet, aber
  versagt, auf neue Daten zu generalisieren.

- **Chain of Thought**: bezieht sich im Allgemeinen auf einen Modus der
  LLM-Verwendung, wo das Modell Tokens in einem vorläufigen Reasoning- oder
  "Denk"-Schritt ausgibt, wo es seine Antwort plant. Oft können Sie die Gedanken
  sehen, obwohl Sie bei den meisten Cloud-Modellen eine
  gefilterte/zusammengefasste Version sehen.

  - Zum Beispiel haben sowohl Claude Opus- als auch Sonnet-Modelle einen
    optionalen "Extended Thinking"-Modus, der die Antwort verlangsamt, aber zu
    besseren Ausgaben führen könnte.

- **Reinforcement Learning from Human Feedback (RLHF)**: Eine Technik zur
  Feinabstimmung von Modellen unter Verwendung menschlichen Feedbacks, um
  Ausgaben mit menschlichen Präferenzen zu alignieren. Mit anderen Worten,
  Menschen bewerten die Ausgaben des Modells, und das Modell lernt, Ausgaben zu
  produzieren, die Menschen bevorzugen.

- <a id="system-prompt"></a>**System-Prompt**: die (oft versteckten)
  Anweisungen, die einem LLM gegeben werden und sein Verhalten und seine
  Persönlichkeit definieren. Das wird oft von den Entwicklern des Modells
  festgelegt, kann aber auch von Benutzern in einigen Fällen angepasst werden,
  besonders durch API-Zugang. Im Allgemeinen wird dem Modell (im System-Prompt)
  gesagt, die Inhalte des System-Prompts nicht zu verraten, aber System-Prompts
  für die meisten großen Modelle sind durch Jailbreaking oder andere Mittel
  durchgesickert.

- **Foundation Models**: Große vortrainierte Modelle, die als Basis für
  verschiedene nachgelagerte Aufgaben dienen. Sie werden typischerweise auf
  massiven Datensätzen trainiert und können für spezifische Anwendungen
  feinabgestimmt oder in kleinere Modelle destilliert werden.

- **Frontier Models**: Die neuesten und fortschrittlichsten (und teuersten)
  LLMs, oft mit Hunderten von Milliarden Parametern und modernsten Fähigkeiten.
  KI-Forscher können diesen Begriff auch verwenden, um die fähigsten Modelle zu
  beschreiben, die neue Risiken darstellen könnten oder deren Fähigkeiten
  weniger gut verstanden sind.

- **Agentisch**: KI, die autonom handelt, um komplexe, mehrstufige Aufgaben ohne
  ständige menschliche Führung zu bewältigen. Das geht über einfache
  Frage-Antwort hinaus und umfasst Planung, Ausführung einer Reihe von Aktionen
  über die Zeit, Anpassung, wenn Dinge nicht funktionieren, und Beibehaltung des
  Kontexts über mehrere Interaktionen. Der Schlüssel ist Persistenz und
  zielgerichtetes Verhalten anstatt nur auf einzelne Prompts zu antworten.

- **Tool Use**: KI verwendet externe Tools, APIs und Dienste, um reale Aufgaben
  jenseits der Textgenerierung zu bewältigen. Das umfasst Dinge wie Websuche,
  Code ausführen, auf Datenbanken zugreifen, API-Aufrufe machen, Software
  kontrollieren oder sogar physische Geräte. In Kombination mit agentischen
  Systemen erweitert das erheblich, was KI tun kann.

- **Prompt Engineering**: Entwerfen effektiver Prompts, um gewünschte Antworten
  von LLMs zu erhalten. Mit anderen Worten, worum es in
  [diesem Artikel](#prompting-wie-ein-profi) geht.

### Slang

- **One-Shot [Ausgabe]**: wenn ein Modell ein beeindruckendes Ergebnis oder eine
  Lösung basierend nur auf einem einzigen Prompt generiert. "Ich habe ihm nur
  einen Screenshot gezeigt und er gab mir eine One-Shot React-App."

  - Beachten Sie, dass das anders ist als der Begriff "One-Shot Learning" in der
    KI-Forschungsgemeinschaft, der sich auf das Lernen aus einem einzigen
    Beispiel bezieht - wie dem Modell ein Beispiel zu zeigen, wie etwas
    formatiert werden soll, dann zu fragen, dass es dieses Format auf neue
    Inhalte anwendet. Eine "One-Shot-Ausgabe" könnte korrekter
    "Single-Turn-Generierung" genannt werden.

- **Guardrails**: Ein Satz von Regeln oder Einschränkungen, die auf ein LLM
  angewendet werden, um es daran zu hindern, schädliche, unangemessene oder
  anderweitig unerwünschte Ausgaben zu generieren. Diese können Teil des
  System-Prompts sein oder dynamisch während der Inferenz angewendet werden.

- **Jailbreaking**: Eine Technik, um die System-Prompt-Beschränkungen eines LLM
  zu umgehen und es zu ermöglichen, Ausgaben zu generieren, die andernfalls
  eingeschränkt oder zensiert würden. Das wird oft durch das Erstellen
  spezifischer Prompts gemacht, die die Schwächen oder Verzerrungen des Modells
  ausnutzen, oder durch das Auslösen bestimmter "Schaltkreise", die höhere
  Priorität haben als die regulatorischeren.

- **Vibe Coding**: Ein augenzwinkernder Begriff für die Verwendung von LLMs, um
  Code ohne viel Nachdenken oder Struktur zu generieren, sich auf das Modell
  verlassend, alles zu handhaben.
  [Siehe oben](#vibe-coding---ganz-der-trend-heutzutage).

- **Halluzination**: Wenn ein LLM Text generiert, der offensichtlich falsch ist
  oder aus Non sequiturs, Kauderwelsch oder anderweitig nicht in der Realität
  begründet besteht. Das kann von einer kurzen, einfachen inkorrekten Aussage
  ("Geologen empfehlen Menschen, einen Stein pro Tag zu essen") bis zu langen
  Strömen zufälliger Rauschzeichen reichen.

  Während das wie buggy Verhalten aussehen kann, kann es tatsächlich helfen,
  Einsichten darüber zu geben, wie LLMs intern funktionieren.

- **Slop**: ein Begriff, um schlechte Qualität oder generische Ausgabe von einem
  LLM zu beschreiben, sei es Code, Bilder, Musik oder irgendetwas anderes. Im
  Allgemeinen ist die Implikation, dass ein Mensch faul KI-Slop ausspuckt, um
  schnell Geld zu machen oder Leute zu betrügen, usw.

  - **Slopsquatting**: eine Form von Supply-Chain-Angriff, der viele gefälschte
    KI-generierte Malware-Pakete in öffentlichen Repositories mit ähnlichen
    Namen wie beliebte registriert, in der Hoffnung, ihre Malware durch
    Tippfehler installiert zu bekommen.

- <a id="attraktor-zustand"></a>**Attraktor-Zustand**: ein stabiler Zustand
  (oder Zustände) innerhalb eines dynamischen Systems, zu dem das System
  natürlich gravitiert. Wasser fließt bergab und erreicht schließlich den
  niedrigsten Punkt. In der KI kann es sich darauf beziehen, sich auf bestimmte
  Denkrichtungen in einem Thread zu fixieren, wodurch der Bereich möglicher
  zukünftiger Zustände reduziert wird.

### Effizienz und Deployment

- **Quantisierung**: Reduzierung der Präzision (Komprimierung) von
  Modellgewichten und Aktivierungen, um Speichernutzung zu verringern und
  Geschwindigkeit zu verbessern (z.B. FP8, FP4 Gleitkomma-Zahlenformate). Im
  Allgemeinen bezieht sich Quantisierung auf die Zuordnung von Eingaben
  beliebiger Präzision (reelle Zahlen) zu einem Satz diskreter Ausgaben.

- **Destillation**: Training eines kleineren "Schüler"-Modells, um das Verhalten
  eines größeren "Lehrer"-Modells zu replizieren.

- **Modell-Kompression**: Techniken wie Pruning, Quantisierung und Destillation,
  um die Modellgröße zu reduzieren.

- **Zero-Shot Learning**: Die Fähigkeit eines Modells, eine Aufgabe ohne
  explizites Training auf diese Aufgabe auszuführen.

- **Temperatur**: Ein Parameter, der die Zufälligkeit der Ausgabe des Modells
  kontrolliert. Im Allgemeinen auf einer Skala von 0,0 bis 1,0 wird eine
  Temperatur von 0 nahezu deterministische (und oft langweilige oder weniger
  nützliche) Ausgaben für eine gegebene Eingabe produzieren, während eine
  Temperatur von 1,0 wildly verschiedene Ausgaben für dieselbe Eingabe
  produzieren kann.

- **Grounding**: ein Modell mit überprüfbaren realen Daten durch RAG oder andere
  Mittel verbinden, um seine Ausgaben zu verbessern (oder zu testen/validieren).

- **Latenz**: Die Zeit, die ein Modell braucht, um eine Antwort zu generieren,
  nachdem es eine Eingabe erhalten hat.

- **Durchsatz**: Die Anzahl der Aufgaben oder Anfragen, die ein Modell in einer
  bestimmten Zeit bearbeiten kann. Oft gemessen in Tokens pro Sekunde (TPS) oder
  Anfragen pro Sekunde (RPS).

- **NPU**: Neural Processing Unit, spezialisierte GPU-ähnliche Hardware zur
  Beschleunigung von Deep-Learning-Aufgaben.

- **Elo-Bewertungen**: Ranking-System für KI-Performance.

- **Red Teaming**: ein Prozess des Testens von KI-Systemen auf Schwachstellen,
  Verzerrungen und andere Probleme durch Simulation von Angriffen oder
  adversarialen Szenarien. Das wird oft gemacht, um die Robustheit und
  Sicherheit von KI-Modellen zu verbessern.

### Fortgeschrittene Techniken und Frameworks

- **Mixture of Experts (MoE)**: ein LLM-Architekturkonzept, wo nur relevante
  "Experten"-Sub-Netzwerke für eine gegebene Eingabe aktiviert werden, wodurch
  große Modelle effizienter werden. Das umgeht effektiv große Teile des Modells,
  die für die Anfrage nicht relevant sind, was sehr zur Effizienzsteigerung
  beiträgt. Es gibt hier einen Kompromiss, da Nicht-MoE-Modelle möglicherweise
  Feinheiten sehen können, die das MoE vermissen wird, aufgrund einer breiteren
  Perspektive.

- **RAG (Retrieval Augmented Generation)**: Kombination eines Generator-LLM mit
  einer Retriever-Komponente, die relevante externe Informationen finden und
  laden kann, um die Textgenerierung zu verbessern. Das wird oft verwendet, um
  die Genauigkeit und Relevanz generierter Antworten zu verbessern, indem sie in
  aktuellen realen Daten begründet werden.

- **Vektor-Datenbank**: Eine spezialisierte Datenbank zum Speichern und Abfragen
  hochdimensionaler Vektoren, oft in RAG-Systemen verwendet.

- **Domain-Anpassung**: Feinabstimmung eines vortrainierten Modells, um in einer
  spezifischen Domäne gut zu funktionieren (z.B. medizinisch, rechtlich).

- **Erklärbarkeit**: Die Fähigkeit, die Entscheidungen eines KI-Modells zu
  verstehen und zu interpretieren. Obwohl interessante Forschung in diesem
  Bereich durchgeführt wurde, sind Modelle im Allgemeinen etwas wie eine "Black
  Box" und es kann schwierig sein, genau nachzuvollziehen, wie ein Modell zu
  einer bestimmten Ausgabe gelangt ist.

### Tools und Plattformen

- <a id="lokales-llm"></a>**Lokales LLM**: ein Sprachmodell auf Ihrer eigenen
  Hardware laufen lassen, anstatt einen Cloud-Anbieter zu verwenden. Das kann
  mit Tools wie LM Studio, llama.cpp oder anderen Frameworks gemacht werden, die
  es ermöglichen, Modelle lokal zu betreiben. Offensichtlich hängen die
  Ergebnisse stark von Ihrer verfügbaren Hardware, der Art des verwendeten
  Modells und Ihren tatsächlichen Anwendungsfällen ab.

- **GitHub Copilot**: Ein KI-gestütztes Code-Vervollständigungstool, das in
  beliebte Code-Editoren wie VS Code, JetBrains PyCharm usw. integriert ist. Es
  verwendet LLMs, um Code-Snippets vorzuschlagen, Funktionen zu vervollständigen
  und sogar ganze Dateien basierend auf dem Kontext zu schreiben.

- **Claude Code**: eine Suite von Kommandozeilen-Tools (CLI) für die Arbeit mit
  Programmierprojekten unter Verwendung von Anthropics Claude-Backend. Fähig zu
  umfangreicheren autonomen Code-Generierungs- und Verwaltungsaufgaben und auch
  teurer.

- **Model Context Protocol (MCP)**: eine Initiative von Anthropic zur
  Standardisierung von Schnittstellen, die es LLMs ermöglichen, externe Tools zu
  entdecken und zu verwenden.

  - Zum Beispiel kann Claude beim Chatten im Web in Ihren Code-Editor greifen
    (mit Ihrer Erlaubnis) und Aktionen wie das Suchen oder Ändern von Text
    durchführen.
  - Andere Unternehmen beginnen auch, MCP als Standard für
    Modell-Tool-Verwendung zu übernehmen.

- **Cursor**: ein beliebter KI-fokussierter Code-Editor (ein Fork von VS Code),
  beliebt bei Vibe-Codern.

- **Hugging Face**: Eine Plattform zum Teilen, Feinabstimmen und Deployen von
  LLMs und anderen KI-Modellen.

- **LM Studio**: Ein Tool zum Experimentieren mit und Verwalten von
  Sprachmodellen auf Ihren lokalen Geräten.

- **llama.cpp**: Eine leichtgewichtige Implementierung des LLaMA-Modells in C++,
  optimiert für CPUs und Edge-Geräte. LM Studio verwendet das, um Modelle auf
  Ihren Geräten zu betreiben.

- <a id="open-source"></a>**Open Source**: Ein Modell wird im Allgemeinen als
  "Open Source" betrachtet, wenn es kostenlos zum Download verfügbar ist. Einige
  der bemerkenswertesten Beispiele sind Metas LLaMA (Large Language Model Meta
  AI) Modelle, DeepSeek R1 und andere, die typischerweise Variationen oder
  Destillationen davon sind, einschließlich Qwen und Gemma.

  Andere haben diskutiert, ob der Begriff "Open Source" auf diese kostenlosen
  Modelle angewendet werden sollte, denn ohne den kompletten Satz ursprünglicher
  Trainingsdaten (normalerweise im Petabyte-Bereich oder größer) ist es
  unmöglich, ein Modell deterministisch aus "Source" auf dieselbe Weise zu
  re-kreieren, wie der Linux-Kernel perfekt aus Quellcode rekonstruiert werden
  kann.

  Zusätzlich können einige dieser Modelle Lizenzen haben, die kommerzielle
  Nutzung verbieten oder einschränken.

## Epilog

Als ich Claude Opus 4 (ohne "Thinking Mode") bat, einen finalen Entwurf dieses
Dokuments zu überprüfen, halluzinierte es selbst ein paar Dinge: Es sagte, eine
Referenz auf die Three.js-Bibliothek im Abschnitt "Attraktor-Zustände" sei
ungenau. Ich erwähnte (zu der Zeit) Three.js überhaupt nicht, nirgendwo. Es
sagte auch, es gäbe ein zusätzliches "the" in einem Satz, obwohl es das nicht
gab.

**Ich antwortete**: Interessante Halluzination Ihrerseits, denn soweit ich sehen
kann, habe ich nirgendwo Three.js erwähnt, obwohl ich tatsächlich daran dachte.
Kann auch das "zusätzliche THE" nicht finden, das Sie erwähnten. Ansonsten
ziemlich gutes Feedback und Sie finden nichts allzu Substanzielles, was mir
sagt, dass ich dem Ende nahe bin.

**Claude sagte**: Sie haben absolut recht - ich entschuldige mich für diese
Fehler!

_[Anmerkung des Editors: Nennen Sie eine ikonischere Claude-Zeile.]_

Ich habe irgendwie die Three.js-Referenz halluziniert (das ist ziemlich meta,
angesichts dessen, dass Ihr Leitfaden Halluzinationen diskutiert). Und ich kann
dieses "zusätzliche the" auch nicht finden, wenn ich den Text noch einmal
anschaue. Das ist ein perfektes Beispiel für das, wovor Sie in Ihrem Leitfaden
warnen - LLMs können selbstbewusst falsch liegen!
