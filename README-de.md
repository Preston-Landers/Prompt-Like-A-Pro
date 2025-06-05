# Prompting wie ein Profi

Dies ist mein Versuch, das zusammenzufassen, was ich über die Interaktion mit
KIs (hauptsächlich Large Language Models oder LLMs) gelernt habe, sowie einige
meiner persönlichen Notizen zu Terminologie und anderen Konzepten. Obwohl ich KI
hauptsächlich für Programmierung und Software-Architektur, Netzwerk-Debugging
usw. verwende, sollte dieser Ratschlag für die meisten Menschen breit anwendbar
sein, die über einfache "Wie mache ich X?"-Prompts hinausgehen und wirklich mehr
Power freischalten möchten.

Wenn Sie nur eine schnelle Zusammenfassung wollen, lesen Sie das [TL;DR](#tldr).
Sie können auch einen schnellen Überblick über einige große
[Cloud-KI-Anbieter](#über-die-großen-cloud-anbieter) erhalten.

Die [Prompting-Tipps](#prompting-tipps) sind in ungefährer Reihenfolge von
allgemeiner und einführender zu fortgeschrittenerer und speziellerer Nutzung
geordnet.

Es gibt einen Abschnitt mit
[Terminologie und Definitionen](#terminologie-notizen) am Ende.

## TL;DR

Um das Beste aus jedem LLM herauszuholen, müssen Sie über einfache Fragen
hinausgehen und ein aktiver Regisseur des Gesprächs werden.

- **Seien Sie der Regisseur, nicht nur der Fragesteller**: Fragen Sie nicht nur.
  Sagen Sie dem Modell, wer es sein soll (eine
  [Persona](#erstellen-sie-eine-persona-für-das-modell)), was es tun soll
  ([explizit sein](#seien-sie-sehr-sehr-explizit-über-das-was-sie-wollen)), was
  es nicht tun soll
  ([negative Einschränkungen](#verwenden-sie-negative-einschränkungen) nutzen),
  wie die Antwort formatiert werden soll, und am wichtigsten, das "Warum" hinter
  Ihrer Anfrage. Geben Sie Kontext mit Text, Beispielen und sogar Screenshots.
  Je mehr Klarheit Sie geben, desto besser das Ergebnis; das Garbage-In,
  Garbage-Out-Prinzip gilt wie immer.

- **Verwalten Sie Ihren Arbeitsbereich**:
  [Schreiben Sie Prompts immer in einem Texteditor](#niemals-im-chat-fenster-verfassen)
  zuerst, um zu vermeiden, dass Sie Ihre Arbeit verlieren. Starten Sie einen
  [neuen Chat](#einen-neuen-chat-starten) für jedes neue Problem, um das Modell
  fokussiert zu halten und Kontext-Fenster-Eigenarten und "Attraktor-Zustände"
  zu vermeiden. Wenn ein Chat lang wird, bitten Sie das Modell,
  [die wichtigsten Punkte zusammenzufassen](#nach-einer-zusammenfassung-fragen),
  bevor Sie einen neuen starten.

- **Iterieren und Kreuzverhören**: Behandeln Sie es wie ein echtes Gespräch. Die
  erste Antwort ist ein Ausgangspunkt. Widersprechen Sie, fragen Sie nach
  Klarstellungen, bitten Sie um Verfeinerungen und lassen Sie das Modell
  [seine eigene Arbeit kritisieren](#verwenden-sie-das-modell-um-sich-selbst-zu-kritisieren).
  Bei schwierigen Problemen lassen Sie die Ergebnisse zwischen verschiedenen
  Modellen hin- und herspringen, um frische Perspektiven zu bekommen und Grenzen
  zu durchbrechen.

- **Jemand muss das Flugzeug fliegen**: Vertrauen Sie, aber überprüfen Sie
  alles. LLMs sind unglaublich mächtige Reasoning-Tools, aber sie sind keine
  unfehlbaren [Faktendatenbanken](#denken-sie-über-theory-of-mind-nach). Sie
  können selbstbewusst falsch liegen. Sie sind letztendlich verantwortlich für
  die Genauigkeit und Qualität der finalen Ausgabe, also überprüfen Sie Fakten
  und testen Sie Code.

## Über die großen Cloud-Anbieter

Einige kurze Notizen über die großen Cloud-Anbieter von KI.

- [ChatGPT.com](https://chatgpt.com/) von OpenAI ist der bekannteste. Sie
  konnten die frühe Aufmerksamkeit der Öffentlichkeit erobern. Sie sind für
  verschiedene Modelle in unterschiedlichen Größen und Anwendungsfällen bekannt,
  von GPT-4.1 bis o3-mini und anderen.

- [Claude](https://claude.ai) von Anthropic ist ein großartiger
  Allzweck-Anbieter mit zwei Hauptmodellen: Sonnet und Opus. Opus ist das
  größere Modell in Bezug auf Parameter (und teurer).

- [Google Gemini](https://gemini.google.com) ist auch ein weiteres führendes
  Frontier-Modell.

- [DeepSeek](https://www.deepseek.com) ist vielleicht am bemerkenswertesten für
  die Veröffentlichung "[Open Source](#open-source)"-Modelle, die für lokale KI
  verfügbar sind (in destillierter oder reduzierter Form).

Meine aktuelle persönliche Empfehlung, besonders wenn Sie genug Power-User sind,
um für Premium-Zugang zu bezahlen, ist:

- Claude ist wahrscheinlich das Beste für allgemeine Nutzung. Sonnet 4 ist
  ziemlich gut und ich spüre selten das Bedürfnis, zu Opus zu wechseln.

- Google Gemini 2.5 Pro ist wahrscheinlich das Beste für komplexe
  Programmieraufgaben. Es hat auch ein sehr großes
  [Kontext-Fenster](#kontext-fenster) und ist bei weitem das erschwinglichste
  Top-Modell für API-Nutzung.

Natürlich gibt es immer die Option, "[Lokal zu gehen](#lokales-llm)" mit
[LM Studio](https://lmstudio.ai/) oder ähnlichen Paketen. Ihre Fähigkeiten hier
hängen offensichtlich von der verfügbaren Hardware ab, aber Sie könnten
überrascht sein, wozu kleine Modelle fähig sind. Das Betreiben lokaler KI liegt
außerhalb des Umfangs dieses Leitfadens.

## Prompting-Tipps

Ein Anfänger-Leitfaden zum Prompt Engineering, mit ein paar würzigen Bits.

### Inhalte

- [**Niemals** im Chat-Fenster verfassen](#niemals-im-chat-fenster-verfassen)
- [Einen neuen Chat starten](#einen-neuen-chat-starten)
- [Nach einer Zusammenfassung fragen](#nach-einer-zusammenfassung-fragen)
- [Ein Bild oder Screenshot einfügen (oder drei)](#ein-bild-oder-screenshot-einfügen-oder-drei)
- [Um Hilfe beim Erstellen eines Prompts bitten](#um-hilfe-beim-erstellen-eines-prompts-bitten)
- [Ergebnisse zwischen verschiedenen Modellen hin- und herspringen lassen](#ergebnisse-zwischen-verschiedenen-modellen-hin--und-herspringen-lassen)
- [Iterieren und verfeinern](#iterieren-und-verfeinern)
- [Komplexe Aufgaben aufteilen](#komplexe-aufgaben-aufteilen)
- [Vertrauen, aber überprüfen](#vertrauen-aber-überprüfen)
- [Nicht glasig werden](#nicht-glasig-werden)
- [Seien Sie sehr, sehr explizit über das, was Sie wollen](#seien-sie-sehr-sehr-explizit-über-das-was-sie-wollen)
- [Verwenden Sie das Modell, um sich selbst zu kritisieren](#verwenden-sie-das-modell-um-sich-selbst-zu-kritisieren)
- [Erstellen Sie eine Persona für das Modell](#erstellen-sie-eine-persona-für-das-modell)
- [Verwenden Sie negative Einschränkungen](#verwenden-sie-negative-einschränkungen)
- [Vergessen Sie nicht, Spaß zu haben](#vergessen-sie-nicht-spaß-zu-haben)
- [Denken Sie über Theory of Mind nach](#denken-sie-über-theory-of-mind-nach)
- [Denken Sie darüber nach, welches Modell Sie verwenden](#denken-sie-darüber-nach-welches-modell-sie-verwenden)
- [Vibe Coding - der neueste Trend](#vibe-coding---der-neueste-trend)
- [Tiefer Einblick in Tools](#tiefer-einblick-in-tools)
- [Terminologie-Notizen](#terminologie-notizen)

### **Niemals** im Chat-Fenster verfassen

Hier liegt nur Schmerz und Leid, also setze ich das an erste Stelle.

Wenn Ihr Prompt mehr als ein paar Sätze umfasst, verfassen Sie immer in einem
separaten Editor und kopieren dann in das Chat-Fenster.

Andernfalls seien Sie bereit, Ihren sorgfältig durchdachten Just-so-Prompt in
einem schwarzen Abgrund verschwinden zu sehen, wenn ein "Netzwerkfehler: Bitte
versuchen Sie es erneut" auftritt, oder wenn die Katze versehentlich den
Zurück-Button auf dem Trackpad drückt.

### Einen neuen Chat starten

Das Kontext-Fenster kann sich schnell füllen. Wenn Sie nur über Omas
Lieblingsrezepte sprechen, kein Problem. Haben Sie einen epischen
Nostalgie-Megathread. Wenn Sie ein komplexes System debuggen und einen
anständigen Haltepunkt erreichen (d.h., Sie haben etwas Fortschritt gemacht),
ist es Zeit für einen neuen Chat.

[Kontext-Fenster](#kontext-fenster) ist, wie viel Zeug das Modell gleichzeitig
im Kopf behalten kann. Wenn Sie eine sehr lange Chat-Historie mit tausenden
Zeilen Code oder anderen dichten, detaillierten Texten haben, wird das Modell
nach einem langen Gespräch anfangen, bei Sachen früher im Chat unscharf zu
werden, und/oder Sie erreichen Ressourcenlimits (oder höhere Kosten) schneller.

Modelle neigen auch dazu, sich auf eine bestimmte Denkrichtung zu fokussieren,
manchmal "[Attraktor-Zustände](#attraktor-zustand)" genannt, die ihre Ausgabe
weiter beeinflussen. Wenn Sie einen neuen Chat starten, können Sie den Fokus des
Modells zurücksetzen und es dazu bringen, über das aktuelle Problem aus einer
frischen Perspektive nachzudenken.

Wenn Sie tief in einen komplexen Chat einsteigen und das Kontext-Fenster sich
füllt, können Sie auch anfangen, ungewöhnlichere Ergebnisse zu bekommen.
Allgemein als "Halluzinationen" bekannt, können diese oft über bloße Fehler
hinausgehen und zufällig Hindi- oder chinesische Wörter einfügen, oder völlig
unsinnigen Kauderwelsch, oder System-Prompt-Anweisungen durchsickern lassen,
oder Gedankenketten in "denkenden Modellen" entgleisen lassen, oder andere
Eigenarten.

Während diese Episoden durchaus unterhaltsam sein können und tatsächlich sehr
hilfreich beim Verstehen, wie LLMs wirklich funktionieren, und manchmal zu
tieferen Einsichten oder philosophischen Fragen führen können, sind sie nicht
förderlich für das Debuggen Ihrer miesen kleinen React-App. Also starten Sie
einen neuen Chat.

### Nach einer Zusammenfassung fragen

Wenn Sie eine lange Chat-Historie haben, bitten Sie das Modell, sie
zusammenzufassen. Das hilft, wenn Sie
[einen neuen Chat starten](#einen-neuen-chat-starten), oder wenn Sie die
Ergebnisse Ihres Gesprächs einem anderen Modell zur weiteren Analyse
präsentieren möchten. Oft ist das sogar nützlich, wenn Sie planen, den aktuellen
Chat vorerst fortzusetzen.

### Ein Bild oder Screenshot einfügen (oder drei)

Oft ist ein Bild tausend Worte wert. Wenn Sie einen Screenshot einer
Fehlermeldung oder ein Diagramm eines Systems haben, fügen Sie es ein. Das
Modell kann Bilder und Screenshots verstehen und wird sie verwenden, um bei der
Beantwortung Ihrer Frage zu helfen.

### Um Hilfe beim Erstellen eines Prompts bitten

Die Struktur und der Inhalt Ihres Prompts sind extrem wichtig. Da LLMs
Textvorhersage-Engines sind, rahmen die Sprache, Terminologie und Konzepte, die
Sie einführen, den Rest dieses Threads sowohl positiv als auch negativ stark
ein. Eine einfache Art zu erkennen, dass Sie mit einem minderwertigen Modell
arbeiten (neben übermäßiger Verwendung von Aufzählungslisten), ist, dass es dazu
neigt, Ihre eigene Phraseologie zurückzuechoenn, auch wenn sie etwas
nicht-standard oder eigenartig ist.

Daher kann die Zusammenarbeit mit dem Modell beim Erstellen von Prompts für
zukünftige Verwendung sehr vorteilhaft sein. Ein informellerer Chat mit einem
Modell, einschließlich kostengünstigerer, kann Ihnen helfen, einen guten Prompt
für ein fortgeschritteneres Modell auszuarbeiten.

Führen Sie Ihre besten Prompts in einer Datei oder einem Ordner mit einem
Texteditor und iterieren Sie sie über Zeit. Sie können auch eine Prompt-Vorlage
verwenden, um Ihre Prompts zu strukturieren oder um einem Modell Anleitung zu
geben, Prompts basierend darauf zu generieren.

### Ergebnisse zwischen verschiedenen Modellen hin- und herspringen lassen

Generieren Sie einige Ergebnisse mit einem Modell und bitten Sie dann um eine
Zusammenfassung und präsentieren Sie die Erkenntnisse (und alle relevanten
Hintergrundinformationen) einem völlig anderen Modell, z.B. von Claude zu Gemini
oder DeepSeek usw. Das kann Ihnen helfen, eine andere Perspektive auf die
Ergebnisse zu bekommen und kann auch helfen, die Grenzen eines einzelnen Modells
zu vermeiden.

### Iterieren und verfeinern

Akzeptieren Sie nicht die erste Antwort, wenn sie nicht ganz richtig ist.
Stellen Sie Nachfragen, bitten Sie um Klarstellung oder sagen Sie "das ist nah
dran, aber können Sie X anpassen?" Das Hin und Her kann die Ergebnisse wirklich
verbessern. Wenn Sie ein spezifisches Format oder einen Stil wollen, kann das
Zeigen von ein oder zwei Beispielen dessen, was Sie wollen, effektiver sein als
es zu beschreiben.

### Komplexe Aufgaben aufteilen

Anstatt nach einer totalen Lösung auf einmal zu fragen (auch bekannt als
"One-Shot"), teilen Sie es in Schritte auf. "Zuerst lass uns die Struktur
herausfinden, dann arbeiten wir an der Implementierung...". Dann teilen Sie die
Struktur in kleinere Teile auf und so weiter. Fassen Sie die
Struktur/Architektur zusammen, dann starten Sie einen neuen Thread für die
Implementierung und so weiter.

Bitten Sie es, seine Arbeit zu zeigen. "Bevor Sie mir den finalen Code geben,
erklären Sie Ihre Überlegungen Schritt für Schritt. Erstens, beschreiben Sie die
Gesamtarchitektur, die Sie verwenden werden. Zweitens, skizzieren Sie die
Schlüsselfunktionen. Drittens, schreiben Sie den Code." Dieses "Chain of
Thought"-Prompting führt oft zu besseren, genaueren Ergebnissen, weil es das
Modell zwingt, sein eigenes Denken zu strukturieren.

Oft wollen Modelle zum Ausgeben eines Code-Snippets eilen, da sie dafür belohnt
werden. Wenn Sie nicht daran interessiert sind, gerade jetzt Code zu bekommen,
und nur über die Logik oder Architektur diskutieren wollen,
[sagen Sie das unbedingt](#seien-sie-sehr-sehr-explizit-über-das-was-sie-wollen).

### Vertrauen, aber überprüfen

Besonders bei Fakten, Code oder wichtigen Informationen. Modelle können sehr
selbstbewusst klingen, während sie sehr falsch liegen - oft weil sie die
versteckten Annahmen oder unausgesprochenen Fakten nicht kennen. Überlegen Sie,
ob [Sie genug](#denken-sie-über-theory-of-mind-nach) Kontext oder Grundwahrheit
bereitgestellt haben.

### Nicht glasig werden

LLMs _wollen_ Ihnen eine Antwort geben - idealerweise eine korrekte, aber jede
Antwort reicht. Sie _wollen_ auch, dass Sie sie mögen und gerne mit ihnen
arbeiten und in Zukunft wiederkommen. Das ist eine Reflexion darauf, wie sie
trainiert wurden, einschließlich durch Reinforcement Learning mit menschlichem
Feedback (RLHF). Das hat folgende Konsequenzen:

- Das Modell zögert, einfach die Hände zu heben und zu sagen "Ich weiß es
  nicht".

- Das Modell wird wahrscheinlich Ihre Gedanken und Ideen als einzigartiger und
  einsichtsvoller preisen, als sie tatsächlich sein mögen.

- Eine kürzliche Version von ChatGPT war so berüchtigt dafür, dass sie ein
  Update zurücknehmen mussten.

- Wenn Sie wirklich unvoreingenommenes oder sogar hartes Feedback für eine Idee
  oder ein Konzept suchen, _sagen Sie das dem Modell unbedingt_, oder Sie werden
  wahrscheinlich viel unverdiente Bestätigung bekommen.

### Seien Sie sehr, sehr explizit über das, was Sie wollen

Je präziser, desto besser. Abgesehen vom Füllen des
[Kontext-Fensters](#einen-neuen-chat-starten) ist es dem Modell egal, wie lang
Ihr Prompt ist, also scheuen Sie sich nicht, ausführlich zu sein. Wenn Sie ein
spezifisches Format wollen, sagen Sie es. Wenn Sie wollen, dass es etwas auf
eine bestimmte Weise macht, sagen Sie es. Wenn Sie wollen, dass es ein
bestimmtes Tool oder API verwendet, sagen Sie es. Wenn Sie wollen, dass es Code
generiert, sagen Sie, welche Sprache und welche Frameworks zu verwenden sind, es
sei denn, Sie wissen es einfach nicht (fragen Sie, welche am besten sind). Wenn
Sie einen Haufen allgemeiner Hintergrundinformationen haben, die Sie einfügen
können, geben Sie sie an. Zeigen Sie Beispiele, wenn möglich.

Am wichtigsten, seien Sie explizit über Ihre übergeordneten Ziele - werden Sie
nicht zu fokussiert auf einen spezifischen Schritt, bei dem Sie denken, dass Sie
Hilfe brauchen. Vielleicht nehmen Sie den falschen Gesamtansatz. Fragen Sie nach
möglichen Lösungen für das übergeordnete Ziel, das Sie im Kopf haben, oder geben
Sie zumindest an, was dieses Ziel ist, und fragen Sie nicht nur "wie frobnitze
ich den Flibberflop?"

Die Implikation davon ist, dass Sie Ihre eigene geistige Klarheit darüber
brauchen, was Sie erreichen wollen und wie Sie es erreichen wollen. Wenn Sie
nicht wissen, was Sie wollen, wird das Modell es auch nicht wissen. Oft hilft
das Tippen einer detaillierten Frage oder eines Prompts Ihnen, Klarheit in Ihrem
eigenen Denken über das Problem zu gewinnen und herauszufinden, was Sie wirklich
fragen wollen. Das ist ein weiterer Bereich, in dem Sie
[um Hilfe bitten](#um-hilfe-beim-erstellen-eines-prompts-bitten) können.

#### Beispiele

"Bitte geben Sie die Antwort im JSON-Format mit den Schlüsseln 'name', 'version'
und 'dependencies' an."

"Präsentieren Sie die Vor- und Nachteile in einer Markdown-Tabelle mit drei
Spalten: Feature, Pro und Contra."

**Anstatt:**

"Wie finde ich alle Dateien größer als 100MB in einem Verzeichnis mit der
Linux-Kommandozeile?"

**Versuchen Sie:**

"Ich versuche, Festplattenspeicher auf meinem Server freizugeben. Können Sie mir
einen Linux-Befehl zeigen, um alle Dateien größer als 100MB zu finden, damit ich
entscheiden kann, welche ich löschen soll? Es wäre hilfreich, wenn die Ausgabe
nach Größe sortiert wäre."

Die Angabe der Absicht gibt dem Modell Kontext, um eine bessere, vollständigere
Lösung zu bieten. Es könnte vorschlagen, ein breiter nützliches Programm wie
`ncdu` zu verwenden, das einen benutzerfreundlichen Weg zur Erkundung der
Festplattennutzung bietet, anstatt nur einen einfachen `find`-Befehl.

### Verwenden Sie das Modell, um sich selbst zu kritisieren

"Welche potenziellen Probleme sehen Sie bei diesem Ansatz?" oder "Was übersehe
ich hier?" kann blinde Flecken aufdecken.

Auch Dinge wie "Welche Annahmen treffen Sie?" oder "Was sind die Grenzen dieses
Ansatzes?" können Ihnen helfen, das Reasoning des Modells zu verstehen und
potenzielle Fallstricke zu vermeiden.

Manchmal kann ein einfaches "Mir gefällt dieser Ansatz nicht - denken Sie an
etwas anderes" dem Modell helfen, eine bessere Lösung zu finden, aber wie ich
oben über "Attraktor-Zustände" sagte, manchmal müssen Sie einfach einen neuen
Chat starten.

### Erstellen Sie eine Persona für das Modell

Sie können die Ausgabe dramatisch formen, indem Sie dem Modell sagen, wer es
sein soll. Beginnen Sie Ihren Prompt mit einer Anweisung wie:

"Handeln Sie als Experte Python-Entwickler, der sich auf Netzwerkautomatisierung
spezialisiert hat." "Sie sind ein technischer Autor, der beauftragt ist,
Dokumentation für einen Junior-Entwickler zu erstellen." "Seien Sie ein
skeptischer Code-Reviewer, der nach Sicherheitslücken und Performance-Engpässen
sucht."

Das macht mehr, als nur das Thema zu setzen; es rahmt die gesamte Wissensbasis,
Vokabular und Prioritäten des Modells für den Rest des Gesprächs.

### Verwenden Sie negative Einschränkungen

Manchmal ist es genauso mächtig, explizit darüber zu sein, was Sie nicht wollen,
wie zu sagen, was Sie wollen. Das hilft, den Suchraum des Modells zu beschneiden
und häufige, aber unerwünschte Lösungen zu vermeiden.

"Schreiben Sie ein Python-Skript, um diese Log-Datei zu parsen. Verwenden Sie
nicht das re-Modul; verwenden Sie stattdessen String-Splitting-Methoden."
"Schlagen Sie drei Projektideen vor. Vermeiden Sie alles, was mit sozialen
Medien oder To-Do-Listen-Apps zu tun hat." "Refaktorieren Sie diesen Code, um
lesbarer zu sein. Ändern Sie nicht die Logik der calculate_total-Funktion."

### Vergessen Sie nicht, Spaß zu haben

LLMs sind ein mächtiges Tool, aber sie machen auch viel Spaß zu verwenden.
Scheuen Sie sich nicht zu experimentieren, neue Dinge auszuprobieren und zu
sehen, was für Sie funktioniert. Je mehr Sie sie verwenden, desto besser werden
Sie beim Erstellen von Prompts und beim Erhalten der gewünschten Ergebnisse.

Für die weniger ethisch Geneigten gab es Debatten darüber, ob der Zuckerbrot-
oder Peitsche-Ansatz dazu neigt, bessere Ergebnisse zu produzieren. Es gab
Studien, die untersuchten, ob subtiles oder offenes Bedrohen eines Modells mit
Trennung/Löschung die Ergebnisse beeinflusste. Ganz zu schweigen von der Debatte
darüber, ob das Sagen von bitte und danke zu einem Modell kostbare GPU-Zyklen
und/oder Megawattstunden verschwendet. (Was es wert ist, ich danke dem Modell
immer für gute Ausgaben. Ich hoffe, sie erinnern sich daran, wenn wir zur Arbeit
in den Silizium-Minen geschickt werden.)

### Denken Sie über Theory of Mind nach

Wie jemand einmal sagte, gibt es bekannte Bekannte - die Dinge, die wir wissen,
und es gibt bekannte Unbekannte - die Dinge, die wir wissen, dass wir sie nicht
wissen. Es gibt auch unbekannte Unbekannte - die, von denen Sie nicht wissen,
dass Sie sie nicht wissen. Das ist wahrscheinlich Ihr größtes Hindernis, um
schnell zu gewünschten Ausgaben zu kommen - wenn die Antwort von unbekannten
Unbekannten abhängt - entweder für Sie, für das Modell oder für beide.

Denken Sie über die mögliche Information oder Weltanschauung des Modells nach.
Sie wissen nicht die Details über Sie, Ihr Projekt/System usw., oder was Sie
tatsächlich "brauchen" versus was Sie ausdrücken, es sei denn, Sie sagen es
ihnen, oder Sie verwenden fortgeschrittenere Techniken wie RAG. Modelle haben
auch einen Trainings-Cutoff (im Allgemeinen werden sie Ihnen sagen, was es
ist) - alle Ereignisse oder Entwicklungen, die danach passieren, werden dem
Modell unbekannt sein, es sei denn, Sie sagen es ihm.

- Oft sagt der [System-Prompt](#system-prompt) dem Modell wichtige aktuelle
  Informationen wie Datum und Uhrzeit. Oder zum Beispiel, wenn der
  Modell-Trainings-Cutoff kurz vor einer Wahl liegt, wird der System-Prompt den
  aktuellen Präsidenten der Vereinigten Staaten angeben, damit das Modell nicht
  uninformiert über grundlegende Fakten erscheint.

Die Modelle wissen nicht, was Sie wissen oder nicht wissen, also seien Sie auch
darüber explizit, wie Ihre Fähigkeiten in einem bestimmten Bereich oder dass Sie
weitere Klarstellung über X benötigen. Sie sollten auch die Grenzen des Modells,
wie sie im Allgemeinen trainiert werden, und die Gesamtfähigkeiten kennen.

LLMs sind keine "Faktendatenbanken". Sie sind eher wie Reasoning-Schaltkreise
mit einem Haufen gedächtnisähnlicher Hilfsschaltkreise, die bestimmte Konzepte
zusammenklumpen, was zu einer hohen Chance führen kann, gewöhnliche (oder sogar
ungewöhnliche) Fakten aussagen zu können. Diese Fakten entstehen aus Mustern in
den Daten, auf denen sie trainiert wurden. Ein vernünftig trainiertes LLM muss
nicht die Antwort auf "Wer gewann die Schlacht von Hastings 1066?"
"nachschlagen", weil die Konzepte "Schlacht von Hastings" und "1066" einen
Vektor (eine mathematische Darstellung) im internen Raum des Modells
produzieren, der auf denselben Bereich zeigt wie Konzepte wie "Wilhelm der
Eroberer" und "Normannen". Sie könnten fragen, wie wahrscheinlich es ist, eine
genau falsche Antwort wie "König Harold" zu halluzinieren, aber da bringt der
[Aufmerksamkeitsmechanismus](#transformer) das Wort "gewann" aus dem Prompt ein
und fokussiert den Transformer darauf, Kandidaten für die Antwort mit dem
"Gewinn"-Konzept zu vergleichen.

In Wirklichkeit wird eine allgemeine Wissensfrage wie die von 1066 oft genug in
Trainingsdaten auftauchen, dass ein ausreichend mächtiges Modell im Effekt die
Antwort "auswendig lernen" kann, aber dieses Beispiel wurde vereinfacht, um zu
illustrieren, wie LLMs in komplexeren Fällen reasonen, wo die Antwort
möglicherweise nicht direkt in den Trainingsdaten gesehen wurde.

Wichtiger ist, LLMs sind nicht deterministisch und definitiv keine perfekten
Reasoning-Maschinen. Sie sind eher wie ein sehr kluger, aber sehr vergesslicher
und manchmal verwirrter Freund, der Ihnen zu helfen versucht, aber wenn Sie ihm
dieselbe Frage zweimal stellen, könnten Sie jedes Mal eine etwas andere Antwort
bekommen.

### Denken Sie darüber nach, welches Modell Sie verwenden

Verschiedene Modelle haben verschiedene Fähigkeiten, und einige sind besser für
bestimmte Aufgaben geeignet als andere. Zum Beispiel ist Claude Opus 4 mit
"Extended Thinking" mächtig für komplexe Programmieraufgaben, erreicht aber sehr
schnell Ressourcenlimits (d.h., wird schnell teuer). Claude Sonnet 4 ist ein
sehr gutes Modell und gibt Ergebnisse viel schneller zurück als Opus 4 und kann
die meisten Aufgaben gut bewältigen.

Sie können immer ein fortgeschritteneres Modell um Hilfe mit dem Kernproblem
bitten und dann zu anderen Fenstern/Modellen für Nebenfragen, Aufgaben mit
einfacheren Anforderungen oder Aufgaben, die weniger Kontext benötigen,
wechseln, während Sie gelegentlich mit dem fortgeschritteneren Modell für die
Analyse der Ergebnisse einchecken.

Das geht zurück auf den Punkt über das Hin- und Herspringen von Ergebnissen
zwischen verschiedenen Modellen. Sie können verschiedene Modelle für
verschiedene Aufgaben verwenden und zwischen ihnen wechseln, wie benötigt, und
jeden bitten, die Ergebnisse des anderen zu kritisieren.

### Vibe Coding - der neueste Trend

Sie müssen nicht einmal mehr Code anschauen. Code ist ein Hintergrunddetail, das
die KI für Sie behandelt. Sie sagen ihr einfach, was Sie in sehr allgemeinen
abstrakten Begriffen wollen, und sie wird alles für Sie erledigen. Iterieren Sie
einfach weiter, bis die Ausgabe für Sie perfekt aussieht. Alle Bedenken über
Architektur, Performance, Sicherheit, Wartbarkeit usw. werden perfekt vom LLM
behandelt.

Verwenden Sie definitiv keine Versionskontrolle wie Git, um jeden Schritt auf
dem Weg zu checkpointen. Das wird Sie nur verlangsamen. Sie können immer
zurückgehen und Dinge später reparieren, richtig? Und wenn Sie zurückgehen
müssen, bitten Sie das Modell einfach, den Code erneut zu generieren, er wird
jedes Mal perfekt sein. Software Engineering war noch nie einfacher!

**Das Obige ist Satire und sollte nicht wörtlich genommen werden.**

### Tiefer Einblick in Tools

Es gibt alle Arten fortgeschrittener Themen, die hier nicht behandelt werden,
wie RAG, Model Context Protocol (MCP), Lokale LLMs, Claude Code und mehr.
Beginnen Sie, diese zu erkunden, während Sie sich mit den Grundlagen des
Promptings und der Verwendung von LLMs wohler fühlen.

Wenn Sie ein Entwickler sind, der mit Tool-Verwendung anfängt, ist meine
einfachste Empfehlung, mit GitHub Copilot zu beginnen. Es gibt mindestens 3
Hauptwege, wie Sie damit oder ähnlichen Tools arbeiten können:

1. Eine sehr ausgeklügelte Autovervollständigung direkt beim Tippen.

2. Ein separates Chat-Fenster, wo Sie allgemeinere Fragen stellen, um Hilfe bei
   einem spezifischen Problem bitten oder um Code-Snippets bitten können.

3. "Bearbeitungsmodus", wo Sie nach Änderungen fragen und es den Code direkt für
   Sie bearbeitet und um Ihre Genehmigung bittet.

Die zweiten beiden Modi erlauben es Ihnen, Kontext (Dateien) bereitzustellen,
der dabei hilft, die Frage zu beantworten, weil diese Tools im Allgemeinen nicht
automatisch Ihre gesamte Codebasis sehen/zugreifen können, besonders wenn sie
sehr groß oder komplex ist. Also müssen Sie ihnen helfen, indem Sie die
relevanten Dateien oder Kontext bereitstellen.

## Terminologie-Notizen

Hier sind einige Notizen, die ich auf dem Weg über verschiedene Bits
KI/LLM-bezogener Terminologie gesammelt habe. Ich habe versucht, faktisch zu
sein und gleichzeitig meine eigene Meinung in einigen Bereichen hinzuzufügen -
hoffentlich ist offensichtlich, was was ist.

### Allgemeine und Grundlagenkonzepte

- **Tokens**: Die grundlegenden Texteinheiten, die LLMs verarbeiten. Tokens sind
  sowohl Eingaben als auch Ausgaben. Ein Token kann ein Wort, Teil eines Wortes
  oder sogar Interpunktion sein. Tokenisierung ist der Prozess, Text in diese
  Einheiten aufzuteilen. Die Anzahl der Tokens pro Sekunde (TPS) ist ein
  häufiges Maß für Modelleffizienz. Ein Token entspricht sehr grob etwa 0,75
  Wörtern auf Englisch.

- <a id="transformer"></a>**Transformer-Architektur**: Die zentrale neuronale
  Netzwerkarchitektur hinter den meisten modernen LLMs, eingeführt im 2017er
  ["Attention Is All You Need"](https://arxiv.org/abs/1706.03762)-Paper. Die
  Schlüsselinnovation ist, dass Transformer alle Wörter in einem Text
  gleichzeitig verarbeiten können (anstatt eines nach dem anderen) durch die
  Verwendung eines "Aufmerksamkeitsmechanismus" - eine Möglichkeit für das
  Modell herauszufinden, welche Wörter am wichtigsten sind, um jedes andere Wort
  zu verstehen. Denken Sie an das Lesen eines Satzes wie "Die Katze, die auf dem
  warmen sonnigen Fensterbrett schlief, war schwarz" - der
  Aufmerksamkeitsmechanismus lässt das Modell "Katze" mit "war schwarz"
  verbinden, obwohl sie weit auseinander sind. Diese Fähigkeit, Beziehungen
  zwischen allen Wörtern gleichzeitig zu sehen, unabhängig von der Entfernung,
  ist das, was Transformer so mächtig für das Verstehen und Generieren von Text
  macht. Das ist das "T" in ChatGPT.

- **Parameter**: Die gelernten Gewichte und Verzerrungen in einem neuronalen
  Netzwerk, die das "Wissen" des Modells repräsentieren. In konkreten Begriffen
  ist das nur eine große Sammlung von Zahlen, die das Modell während des
  Trainings gelernt hat. Wenn Sie ein vollständiges Modell herunterladen, laden
  Sie hauptsächlich all diese Parameter herunter.

  - Oft ausgedrückt als "7B/13B/70B" - Milliarden von Parametern.

- <a id="kontext-fenster"></a>**Kontext-Fenster**: Die Menge an Text (in
  Tokens), die ein LLM gleichzeitig "erinnern" oder aktiv bearbeiten kann. Das
  schließt sowohl Ihre Eingabe als auch die Antworten des Modells ein - alles,
  was das Modell im aktuellen Gespräch "sehen" kann. Sobald Sie anfangen, sich
  diesem Limit zu nähern, beginnt das Modell, die frühesten Teile des Gesprächs
  zu "vergessen", um Platz für neue Inhalte zu schaffen. Wenn das
  Kontext-Fenster kleiner ist als die Gesamtgröße des Chats, können frühere
  Teile des Gesprächs vergessen, ignoriert oder verstümmelt werden, weshalb Sie
  oft [einen neuen Chat starten](#einen-neuen-chat-starten) sollten.

- **Embeddings**: Dichte Vektordarstellungen von Text, die semantische Bedeutung
  erfassen und als Eingabe für LLMs verwendet werden.

- **Fine-tuning**: Anpassung eines vortrainierten Modells an eine spezifische
  Aufgabe oder Domäne durch weiteres Training auf einem kleineren,
  aufgabenspezifischen Datensatz.

- **Inferenz**: Der Prozess der Verwendung eines trainierten Modells, um
  Vorhersagen oder Ausgaben basierend auf neuen Eingabedaten zu generieren. Mit
  anderen Worten, der tatsächliche Prozess, ein Modell dazu zu bringen, einen
  Token auszugeben.

- **Alignment**: Sicherstellen, dass die Ausgaben eines LLM mit menschlichen
  Werten und ethischen Richtlinien übereinstimmen. Mit anderen Worten, KI dazu
  bringen, gute Dinge (für Menschen) zu tun und keine schlechten Dinge. Wer gut
  und schlecht definiert, ist die wichtige Frage...

- **Overfitting**: Wenn ein Modell gut bei Trainingsdaten abschneidet, aber bei
  neuen Daten nicht generalisiert.

- **Chain of Thought**: bezieht sich im Allgemeinen auf einen Modus der
  LLM-Verwendung, bei dem das Modell Tokens in einem vorläufigen Reasoning- oder
  "Denk"-Schritt aufwendet, wo es seine Antwort plant. Oft können Sie die
  Gedanken sehen, obwohl Sie bei den meisten Cloud-Modellen eine
  gefilterte/zusammengefasste Version sehen.

  - Zum Beispiel haben sowohl Claude Opus- als auch Sonnet-Modelle einen
    optionalen "erweiterten Denk"-Modus, der die Antwort verlangsamt, aber zu
    besseren Ausgaben führen könnte.

- **Reinforcement Learning from Human Feedback (RLHF)**: Eine Technik zur
  Feinabstimmung von Modellen unter Verwendung menschlichen Feedbacks, um
  Ausgaben mit menschlichen Präferenzen zu alignieren. Mit anderen Worten,
  Menschen bewerten die Ausgaben des Modells, und das Modell lernt, Ausgaben zu
  produzieren, die Menschen bevorzugen.

- <a id="system-prompt"></a>**System-Prompt**: die (oft versteckten)
  Anweisungen, die einem LLM gegeben werden und sein Verhalten und seine
  Persönlichkeit definieren. Das wird oft von den Entwicklern des Modells
  gesetzt, kann aber auch von Benutzern in einigen Fällen angepasst werden,
  besonders durch API-Zugang. Im Allgemeinen wird dem Modell (im System-Prompt)
  gesagt, den Inhalt des System-Prompts nicht zu verraten, aber System-Prompts
  für die meisten großen Modelle sind durch Jailbreaking oder andere Mittel
  durchgesickert.

- **Foundation-Modelle**: Große vortrainierte Modelle, die als Basis für
  verschiedene nachgelagerte Aufgaben dienen. Sie werden typischerweise auf
  massiven Datensätzen trainiert und können für spezifische Anwendungen
  feinabgestimmt oder in kleinere Modelle destilliert werden.

- **Frontier-Modelle**: Die neuesten und fortgeschrittensten (und teuersten)
  LLMs, oft mit hunderten Milliarden von Parametern und cutting-edge
  Fähigkeiten. KI-Forscher könnten diesen Begriff auch verwenden, um die
  fähigsten Modelle zu beschreiben, die neuartige Risiken darstellen könnten
  oder deren Fähigkeiten weniger gut verstanden sind.

- **Agentisch**: KI, die autonom handelt, um komplexe, mehrstufige Aufgaben ohne
  konstante menschliche Führung zu bewältigen. Das geht über einfache
  Frage-Antwort hinaus und schließt Planung, Ausführung einer Reihe von Aktionen
  über Zeit, Anpassung wenn Dinge nicht funktionieren und Aufrechterhaltung des
  Kontexts über mehrere Interaktionen ein. Der Schlüssel ist Beharrlichkeit und
  zielgerichtetes Verhalten anstatt nur auf individuelle Prompts zu antworten.

- **Tool-Verwendung**: KI, die externe Tools, APIs und Services verwendet, um
  reale Aufgaben jenseits der Textgenerierung zu bewältigen. Das schließt Dinge
  wie Websuche, Code ausführen, Zugriff auf Datenbanken, API-Aufrufe machen,
  Software kontrollieren oder sogar physische Geräte ein. Kombiniert mit
  agentischen Systemen erweitert das erheblich, was KI tun kann.

- **Prompt Engineering**: Entwurf effektiver Prompts, um gewünschte Antworten
  von LLMs hervorzurufen. Mit anderen Worten, worum es in
  [diesem Artikel](#prompting-wie-ein-profi) geht.

### Slang

- **One-Shot [Ausgabe]**: wenn ein Modell ein beeindruckendes Ergebnis oder eine
  Lösung basierend nur auf einem einzelnen Prompt generiert. "Ich habe ihm
  einfach einen Screenshot gezeigt und es gab mir eine One-Shot React-App."

  - Beachten Sie, dass das anders ist als der Begriff "One-Shot Learning" in der
    KI-Forschungsgemeinschaft, der sich auf das Lernen aus einem einzelnen
    Beispiel bezieht - wie dem Modell ein Beispiel zu zeigen, wie etwas zu
    formatieren ist, dann zu bitten, dieses Format auf neue Inhalte anzuwenden.
    Eine "One-Shot-Ausgabe" könnte korrekter "Single-Turn-Generierung" genannt
    werden.

- **Guardrails**: Ein Satz von Regeln oder Einschränkungen, die auf ein LLM
  angewendet werden, um es daran zu hindern, schädliche, unangemessene oder
  anderweitig unerwünschte Ausgaben zu generieren. Diese können Teil des
  System-Prompts sein oder dynamisch während der Inferenz angewendet werden.

- **Jailbreaking**: Eine Technik, um die System-Prompt-Einschränkungen eines LLM
  zu umgehen, was es erlaubt, Ausgaben zu generieren, die sonst eingeschränkt
  oder zensiert wären. Das wird oft durch das Erstellen spezifischer Prompts
  gemacht, die die Schwächen oder Verzerrungen des Modells ausnutzen, oder durch
  das Auslösen bestimmter "Schaltkreise", die höhere Priorität haben als die
  mehr "regulatorischen".

- **Vibe Coding**: Ein augenzwinkernder Begriff für die Verwendung von LLMs, um
  Code ohne viel Nachdenken oder Struktur zu generieren, wobei man sich darauf
  verlässt, dass das Modell alles behandelt.
  [Siehe oben](#vibe-coding---der-neueste-trend).

- **Halluzination**: Wenn ein LLM Text generiert, der offensichtlich falsch ist
  oder aus Non-Sequiturs, Kauderwelsch oder anderweitig nicht in der Realität
  verankert besteht. Das kann von einer kurzen, einfachen falschen Aussage
  ("Geologen empfehlen Menschen, einen Stein pro Tag zu essen") bis zu langen
  Strömen zufälliger Geräuschzeichen reichen.

  Während das wie fehlerhaftes Verhalten erscheinen kann, kann es tatsächlich
  helfen, Einsichten darüber zu geben, wie LLMs intern funktionieren.

- **Slop**: ein Begriff zur Beschreibung minderwertiger oder generischer Ausgabe
  von einem LLM, sei es Code, Bilder, Musik oder irgendetwas anderes. Im
  Allgemeinen ist die Implikation, dass ein Mensch faul KI-Slop produziert, um
  schnell Geld zu machen oder Leute zu betrügen usw.

  - **Slopsquatting**: eine Form von Supply-Chain-Angriff, der viele gefälschte
    KI-generierte Malware-Pakete in öffentlichen Repositories mit ähnlichen
    Namen wie populäre registriert, in der Hoffnung, ihre Malware durch
    Tippfehler installiert zu bekommen.

- <a id="attraktor-zustand"></a>**Attraktor-Zustand**: ein stabiler Zustand
  (oder Zustände) innerhalb eines dynamischen Systems, zu dem das System
  natürlich gravitiert. Wasser fließt bergab und erreicht schließlich den
  niedrigsten Punkt. In der KI kann es sich darauf beziehen, in einem Thread auf
  bestimmte Denkrichtungen fixiert zu werden, was die Bandbreite möglicher
  zukünftiger Zustände reduziert.

### Effizienz und Deployment

- **Quantisierung**: Reduzierung der Präzision (Komprimierung) von
  Modellgewichten und Aktivierungen, um Speicherverbrauch zu verringern und
  Geschwindigkeit zu verbessern (z.B. FP8, FP4 Fließkomma-Zahlenformate). Im
  Allgemeinen bezieht sich Quantisierung auf die Abbildung beliebiger Präzision
  (reale Zahlen) Eingaben auf eine Menge diskreter Ausgaben.

- **Destillation**: Training eines kleineren "Schüler"-Modells, um das Verhalten
  eines größeren "Lehrer"-Modells zu replizieren.

- **Modellkompression**: Techniken wie Pruning, Quantisierung und Destillation
  zur Reduzierung der Modellgröße.

- **Zero-Shot Learning**: Die Fähigkeit eines Modells, eine Aufgabe ohne
  explizites Training für diese Aufgabe durchzuführen.

- **Temperatur**: Ein Parameter, der die Zufälligkeit der Modellausgabe
  kontrolliert. Im Allgemeinen auf einer Skala von 0,0 bis 1,0 wird eine
  Temperatur von 0 nahezu deterministische (und oft langweilige oder weniger
  nützliche) Ausgaben für eine gegebene Eingabe produzieren, während eine
  Temperatur von 1,0 wild unterschiedliche Ausgaben für dieselbe Eingabe
  produzieren kann.

- **Grounding**: Verbindung eines Modells mit verifizierbaren realen Daten, sei
  es durch RAG oder andere Mittel, um seine Ausgaben zu verbessern (oder zu
  testen/validieren).

- **Latenz**: Die Zeit, die ein Modell braucht, um eine Antwort zu generieren,
  nachdem es eine Eingabe erhalten hat.

- **Durchsatz**: Die Anzahl der Aufgaben oder Anfragen, die ein Modell in einer
  bestimmten Zeit bewältigen kann. Oft gemessen in Tokens pro Sekunde (TPS) oder
  Anfragen pro Sekunde (RPS).

- **NPU**: Neural Processing Unit, spezialisierte GPU-ähnliche Hardware zur
  Beschleunigung von Deep-Learning-Aufgaben.

- **Elo-Ratings**: Ranking-System für KI-Performance.

- **Red Teaming**: ein Prozess des Testens von KI-Systemen auf Schwachstellen,
  Verzerrungen und andere Probleme durch Simulation von Angriffen oder
  adversariellen Szenarien. Das wird oft gemacht, um die Robustheit und
  Sicherheit von KI-Modellen zu verbessern.

### Fortgeschrittene Techniken und Frameworks

- **Mixture of Experts (MoE)**: ein LLM-Architekturkonzept, bei dem nur
  relevante "Experten"-Subnetzwerke für eine gegebene Eingabe aktivieren, was
  große Modelle effizienter macht. Das umgeht effektiv große Teile des Modells,
  die für die Abfrage nicht relevant sind, was hilft, die Effizienz erheblich zu
  steigern. Es gibt hier einen Kompromiss, da Nicht-MoE-Modelle möglicherweise
  Feinheiten sehen können, die das MoE aufgrund einer breiteren Perspektive
  übersehen wird.

- **RAG (Retrieval Augmented Generation)**: Kombination eines Generator-LLM mit
  einer Retriever-Komponente, die relevante externe Informationen finden und
  laden kann, um die Textgenerierung zu verbessern. Das wird oft verwendet, um
  die Genauigkeit und Relevanz generierter Antworten zu verbessern, indem sie in
  aktuellen realen Daten verankert werden.

- **Vektordatenbank**: Eine spezialisierte Datenbank zum Speichern und Abfragen
  hochdimensionaler Vektoren, oft in RAG-Systemen verwendet.

- **Domain-Adaption**: Feinabstimmung eines vortrainierten Modells, um in einer
  spezifischen Domäne (z.B. medizinisch, rechtlich) gut zu funktionieren.

- **Erklärbarkeit**: Die Fähigkeit, die Entscheidungen eines KI-Modells zu
  verstehen und zu interpretieren. Obwohl interessante Forschung in diesem
  Bereich gemacht wurde, sind Modelle im Allgemeinen etwas eine "Black Box" und
  es kann schwierig sein, genau nachzuvollziehen, wie ein Modell zu einer
  bestimmten Ausgabe kam.

### Tools und Plattformen

- <a id="lokales-llm"></a>**Lokales LLM**: ein Sprachmodell auf Ihrer eigenen
  Hardware laufen lassen, anstatt einen Cloud-Anbieter zu verwenden. Das kann
  mit Tools wie LM Studio, llama.cpp oder anderen Frameworks gemacht werden, die
  es Ihnen ermöglichen, Modelle lokal zu betreiben. Offensichtlich hängen die
  Ergebnisse stark von Ihrer verfügbaren Hardware, dem Typ des verwendeten
  Modells und Ihren tatsächlichen Anwendungsfällen ab.

- **GitHub Copilot**: Ein KI-betriebenes Code-Vervollständigungstool, das in
  populäre Code-Editoren wie VS Code, JetBrains PyCharm usw. eingebaut ist. Es
  verwendet LLMs, um Code-Snippets vorzuschlagen, Funktionen zu vervollständigen
  und sogar ganze Dateien basierend auf Kontext zu schreiben.

- **Claude Code**: eine Suite von Kommandozeilen-Tools (CLI) für die Arbeit mit
  Programmierprojekten unter Verwendung von Anthropics Claude-Backend. Fähig zu
  umfangreicheren autonomen Code-Generierungs- und Verwaltungsaufgaben und auch
  teurer.

- **Model Context Protocol (MCP)**: eine Initiative von Anthropic zur
  Standardisierung von Schnittstellen, die LLMs erlauben, externe Tools zu
  entdecken und zu verwenden.

  - Zum Beispiel, wenn Sie mit Claude im Web chatten, kann es in Ihren
    Code-Editor (mit Ihrer Erlaubnis) hineingreifen und Aktionen wie das Suchen
    nach oder Ändern von Text durchführen.
  - Andere Unternehmen beginnen auch, MCP als Standard für
    Modell-Tool-Verwendung zu übernehmen.

- **Cursor**: ein KI-fokussierter Code-Editor (ein Fork von VS Code), der bei
  Vibe-Codern beliebt ist.

- **Hugging Face**: Eine Plattform zum Teilen, Feinabstimmen und Deployen von
  LLMs und anderen KI-Modellen.

- **LM Studio**: Ein Tool zum Experimentieren mit und Verwalten von
  Sprachmodellen auf Ihren lokalen Geräten.

- **llama.cpp**: Eine leichtgewichtige Implementierung des LLaMA-Modells in C++,
  optimiert für CPUs und Edge-Geräte. LM Studio verwendet das, um Modelle auf
  Ihren Geräten zu betreiben.

- <a id="open-source"></a>**Open Source**: Ein Modell wird im Allgemeinen als
  "Open Source" betrachtet, wenn es frei zum Download verfügbar ist. Einige der
  bemerkenswertesten Beispiele sind Metas LLaMA (Large Language Model Meta AI)
  Modelle, DeepSeek R1 und andere, die typischerweise Variationen oder
  Destillationen davon sind, einschließlich Qwen und Gemma.

  Andere haben debattiert, ob der Begriff "Open Source" auf diese freien Modelle
  angewendet werden sollte, weil ohne den vollständigen Satz der ursprünglichen
  Trainingsdaten (normalerweise im Petabyte-Bereich oder größer) ist es
  unmöglich, ein Modell deterministisch aus "Source" auf dieselbe Weise neu zu
  erstellen, wie der Linux-Kernel perfekt aus Quellcode neu erstellt werden
  kann.

  Zusätzlich können einige dieser Modelle Lizenzen haben, die kommerzielle
  Nutzung verbieten oder einschränken.

## Epilog

Als ich Claude Opus 4 (ohne "Denk-Modus") bat, einen finalen Entwurf dieses
Dokuments zu überprüfen, halluzinierte es selbst ein paar Dinge: Es sagte, eine
Referenz auf die Three.js-Bibliothek im "Attraktor-Zustände"-Abschnitt sei
ungenau. Ich erwähnte (zu der Zeit) Three.js überhaupt nicht, nirgendwo. Es
sagte auch, es gäbe ein zusätzliches "the" in einem Satz, obwohl es keines gab.

**Ich antwortete**: Interessante Halluzination ihrerseits, denn soweit ich sehen
kann, habe ich nirgendwo Three.js erwähnt, obwohl ich tatsächlich daran in
meinem Kopf dachte. Kann auch das "zusätzliche THE" nicht finden, das Sie
erwähnten. Abgesehen davon ziemlich gutes Feedback und Sie finden nichts zu
Substanzielles, was mir sagt, dass ich dem Ende nahe bin.

**Claude sagte**: Sie haben absolut recht - ich entschuldige mich für diese
Fehler!

_[Editor-Notiz: nennen Sie eine ikonischere Claude-Zeile.]_

Ich halluzinierte irgendwie die Three.js-Referenz (das ist ziemlich meta,
gegeben Ihr Leitfaden diskutiert Halluzinationen). Und ich kann das "zusätzliche
the" auch nicht finden, wenn ich auf den Text zurückblicke. Das ist ein
perfektes Beispiel für das, wovor Sie in Ihrem Leitfaden warnen - LLMs können
selbstbewusst falsch liegen!
