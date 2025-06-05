# Prompt som en Pro

Dette er mitt forsøk på å destillere det jeg har lært om å samhandle med AI-er
(hovedsakelig store språkmodeller eller LLM-er) og noen av mine personlige
notater om terminologi og andre konsepter. Selv om jeg hovedsakelig bruker AI
til koding og programvarearkitektur, nettverksfeilsøking osv., bør dette rådet
være bredt anvendelig for de fleste som prøver å bevege seg utover enkle
"hvordan gjør jeg X?" type prompts og virkelig låse opp mer kraft.

Hvis du bare vil ha et raskt sammendrag, les [TL;DR](#tldr). Du kan også få en
rask oversikt over noen store
[cloud AI-leverandører](#om-de-store-cloud-leverandørene).

[Prompting-tipsene](#prompting-tips) er i omtrentlig rekkefølge fra mer
generelle og introduserende til mer mellomliggende og avansert bruk.

Det er en seksjon med [terminologi og definisjoner](#terminologi-notater) på
slutten.

## TL;DR

For å få mest mulig ut av en LLM, må du bevege deg utover enkle spørsmål og bli
en aktiv regissør av samtalen.

- **Vær Regissøren, Ikke Bare en Spørrer**: Ikke bare spør. Fortell modellen
  hvem den skal være (en [persona](#lag-en-persona-for-modellen)), hva den skal
  gjøre (vær [eksplisitt](#vær-veldig-veldig-eksplisitt-om-hva-du-vil-ha)), hva
  den ikke skal gjøre (bruk
  [negative begrensninger](#bruk-negative-begrensninger)), hvordan den skal
  formatere svaret, og viktigst av alt, "hvorfor"-et bak forespørselen din. Gi
  kontekst med tekst, eksempler og til og med skjermbilder. Jo mer klarhet du
  gir, desto bedre blir resultatet; Garbage-In, Garbage-Out-prinsippet gjelder
  som alltid.

- **Administrer Arbeidsområdet Ditt**: Alltid
  [skriv prompts i en teksteditor](#aldri-skriv-inne-i-chat-vinduet) først for å
  unngå å miste arbeidet ditt. Start en [ny chat](#start-en-ny-chat) for hvert
  nytt problem for å holde modellen fokusert og unngå
  kontekstvindu-merkelighetene og "attraktor-tilstander". Når en chat blir lang,
  be modellen om å [sammenfatte hovedpunktene](#be-om-et-sammendrag) før du
  starter en ny.

- **Iterer og Kryssforhør**: Behandle det som en ekte samtale. Det første svaret
  er et utgangspunkt. Press tilbake, be om klargjøring, be om forbedringer, og
  få modellen til å
  [kritisere sitt eget arbeid](#bruk-modellen-til-å-kritisere-seg-selv). For
  vanskelige problemer, send resultatene mellom forskjellige modeller for å få
  friske perspektiver og bryte gjennom begrensninger.

- **Noen Må Fly Dette Flyet**: Stol på, men verifiser alt. LLM-er er utrolig
  kraftige resonneringsverktøy, men de er ikke ufeilbarlige
  [faktdatabaser](#tenk-på-theory-of-mind). De kan være selvsikkert feil. Du er
  til syvende og sist ansvarlig for nøyaktigheten og kvaliteten på det endelige
  resultatet, så sjekk faktaene og test koden.

## Om de store cloud-leverandørene

Noen korte notater om de store cloud-leverandørene av AI.

- [ChatGPT.com](https://chatgpt.com/) fra OpenAI er den mest kjente. De klarte å
  fange opp den tidlige tankegangen til allmennheten. De er kjent for flere
  forskjellige modeller på tvers av ulike størrelser og bruksområder, fra
  GPT-4.1 til o3-mini og andre.

- [Claude](https://claude.ai) fra Anthropic er en flott generell leverandør med
  to hovedmodeller: Sonnet og Opus. Opus er den større modellen når det gjelder
  parametere (og dyrere).

- [Google Gemini](https://gemini.google.com) også en annen ledende frontmodell.

- [DeepSeek](https://www.deepseek.com) er kanskje mest bemerkelsesverdige for å
  frigjøre "[åpen kildekode](#åpen-kildekode)" modeller tilgjengelig for lokal
  AI (i destillert eller nedskalert form).

Min nåværende personlige anbefaling, spesielt hvis du er nok av en power user
til å betale for premium tilgang, er:

- Claude er sannsynligvis best for generell bruk. Sonnet 4 er ganske bra og jeg
  føler sjelden behov for å vende meg til Opus.

- Google Gemini 2.5 Pro er sannsynligvis best for komplekse kodingsoppgaver. Den
  har også et veldig stort [kontekstvindu](#kontekstvindu) og er den klart
  rimeligste toppmodellen for API-bruk.

Selvfølgelig er det alltid muligheten for å "[Gå Lokalt](#lokal-llm)" med
[LM Studio](https://lmstudio.ai/) eller lignende pakker. Mulighetene dine her
avhenger åpenbart av maskinvaren du har tilgjengelig, men du kan bli overrasket
over hva små modeller er i stand til. Å kjøre lokal AI er utenfor omfanget av
denne guiden.

## Prompting-tips

En nybegynnerguide til prompt engineering, med noen krydrede biter.

### Innhold

- [**Aldri** skriv inne i chat-vinduet](#aldri-skriv-inne-i-chat-vinduet)
- [Start en ny chat](#start-en-ny-chat)
- [Be om et sammendrag](#be-om-et-sammendrag)
- [Lim inn et bilde eller et skjermbilde (eller tre)](#lim-inn-et-bilde-eller-et-skjermbilde-eller-tre)
- [Be om hjelp med å lage en prompt](#be-om-hjelp-med-å-lage-en-prompt)
- [Send resultater mellom forskjellige modeller](#send-resultater-mellom-forskjellige-modeller)
- [Iterer og finjuster](#iterer-og-finjuster)
- [Del komplekse oppgaver ned](#del-komplekse-oppgaver-ned)
- [Stol på, men verifiser](#stol-på-men-verifiser)
- [Ikke bli glassert](#ikke-bli-glassert)
- [Vær veldig, veldig eksplisitt om hva du vil ha](#vær-veldig-veldig-eksplisitt-om-hva-du-vil-ha)
- [Bruk modellen til å kritisere seg selv](#bruk-modellen-til-å-kritisere-seg-selv)
- [Lag en persona for modellen](#lag-en-persona-for-modellen)
- [Bruk negative begrensninger](#bruk-negative-begrensninger)
- [Ikke glem å ha det gøy](#ikke-glem-å-ha-det-gøy)
- [Tenk på Theory of Mind](#tenk-på-theory-of-mind)
- [Tenk på hvilken modell du bruker](#tenk-på-hvilken-modell-du-bruker)
- [Vibe coding - helt på moten for tiden](#vibe-coding---helt-på-moten-for-tiden)
- [Dypdykk i verktøy](#dypdykk-i-verktøy)
- [Terminologi-notater](#terminologi-notater)

### **Aldri** skriv inne i chat-vinduet

Denne veien fører bare til smerte og lidelse, så jeg setter denne først.

Hvis prompten din er mer enn et par setninger, skriv alltid i en separat editor,
og kopier og lim inn i chat-vinduet.

Ellers, forbered deg på å se din nøye gjennomtenkte prompt forsvinne inn i en
svart avgrunn når en "Nettverksfeil: prøv igjen" skjer, eller når katten ved et
uhell trykker på tilbake-knappen på trackpad-en.

### Start en ny chat

Kontekstvinduet kan fylles opp raskt. Hvis du bare snakker om bestemors
favorittoppskrifter, ikke noe problem. Ha en episk nostalgi-megatråd. Hvis du
feilsøker et komplekst system og når et anstendig stoppunkt (dvs. du gjorde noen
fremgang), er det på tide å starte en ny chat.

[Kontekstvindu](#kontekstvindu) er hvor mye greier modellen kan holde i tankene
på en gang. Hvis du har en veldig lang chat-historie med tusenvis av linjer med
kode eller annen tett, detaljert tekst, etter en lang samtale vil modellen
begynne å bli uskarp på ting tidligere i chatten, og/eller du vil nå
ressursgrenser (eller høyere kostnader) raskere.

Modeller har også en tendens til å bli fokusert på en viss tankegang, noen
ganger kalt "[attraktor-tilstander](#attraktor-tilstand)", som påvirker
utdataene deres nedover linjen. Hvis du starter en ny chat, kan du tilbakestille
modellens fokus og få den til å tenke på det nåværende problemet fra et friskt
perspektiv.

Når du går dypt inn i en kompleks chat og kontekstvinduet fylles opp, kan du
også begynne å få mer... uvanlige resultater. Vanligvis kjent som
"hallusinasjoner", kan disse ofte strekke seg utover bare feil til tilfeldig
innsetting av hindi- eller kinesiske ord, eller fullstendig meningsløs vås,
eller systemprompte instruksjoner som lekker, eller tankekjeder i "tenkende
modeller" som går av sporet, eller andre merkeligheter.

Selv om disse episodene kan være ganske underholdende, og faktisk kan være
ganske nyttige for å forstå hvordan LLM-er virkelig fungerer, og noen ganger
føre til noen dypere innsikter eller filosofiske spørsmål, er de ikke
befordrende for å feilsøke din knotete lille React-app. Så start en ny chat.

### Be om et sammendrag

Hvis du har en lang chat-historie, be modellen om å sammenfatte den. Dette vil
hjelpe når du [starter en ny chat](#start-en-ny-chat), eller når du vil
presentere resultatene av samtalen din for en annen modell for fortsatt analyse.
Ofte er dette nyttig selv om du planlegger å fortsette den nåværende chatten
foreløpig.

### Lim inn et bilde eller et skjermbilde (eller tre)

Ofte er et bilde verdt tusen ord. Hvis du har et skjermbilde av en feilmelding,
eller et diagram av et system, lim det inn. Modellen kan forstå bilder og
skjermbilder, og vil bruke dem til å hjelpe med å svare på spørsmålet ditt.

### Be om hjelp med å lage en prompt

Strukturen og innholdet i prompten din er ekstremt viktig. Fordi LLM-er er
tekstpredikeringsmotorer, påvirker språket, terminologien og konseptene du
introduserer sterkt resten av tråden på både positive og negative måter. En
enkel måte å fortelle at du jobber med en modell av lavere kvalitet (bortsett
fra overdreven bruk av punktlister) er at den vil ha en tendens til å gjenta din
egen fraseologi tilbake til deg, selv om den er noe ikke-standard eller
idiosynkratisk.

Derfor kan det å arbeide med modellen for å hjelpe med å lage prompts for
fremtidig bruk være svært fordelaktig. En mer uformell chat med en modell,
inkludert lavere kostnad-modeller, kan hjelpe deg med å utarbeide en god prompt
for en mer avansert modell.

Hold styr på dine beste prompts i en fil eller mappe ved hjelp av en
teksteditor, og iterer på og finjuster dem over tid. Du kan også bruke en
prompt-mal for å hjelpe deg med å strukturere promptene dine, eller for å gi
veiledning for en modell til å generere prompts basert på den.

### Send resultater mellom forskjellige modeller

Generer noen resultater med en modell og be deretter om et sammendrag og
presenter funnene (og relevant bakgrunnsinformasjon) til en helt annen modell,
f.eks. å gå fra Claude til Gemini eller DeepSeek, osv. Dette kan hjelpe deg med
å få et annet perspektiv på resultatene, og kan også hjelpe deg med å unngå
begrensningene til en enkelt modell.

### Iterer og finjuster

Ikke aksepter det første svaret hvis det ikke er helt riktig. Still
oppfølgingsspørsmål, be om klargjøring, eller si "det er nærme, men kan du
justere X?" Frem-og-tilbake kan virkelig forbedre resultatene. Når du vil ha et
spesifikt format eller stil, kan det å vise et eksempel eller to på hva du vil
ha være mer effektivt enn å beskrive det.

### Del komplekse oppgaver ned

I stedet for å be om en total løsning på en gang (også kjent som et "one-shot"),
del det inn i trinn. "Først, la oss finne ut strukturen, så jobber vi med
implementering...". Deretter del strukturen ned i mindre deler, og så videre.
Sammenfatt strukturen/arkitekturen, start deretter en ny tråd for
implementeringen, og så videre.

Be den om å vise arbeidet sitt. "Før du gir meg den endelige koden, forklar
resonnementet ditt steg-for-steg. Først, beskriv den overordnede arkitekturen du
vil bruke. For det andre, skisser nøkkelfunksjonene. For det tredje, skriv
koden." Denne "chain of thought" promptingen fører ofte til bedre, mer nøyaktige
resultater fordi det tvinger modellen til å strukturere sin egen tenkning.

Ofte vil modeller ønske å skynde seg til å produsere et kodeutdrag, da de har en
tendens til å bli belønnet for det. Hvis du ikke er interessert i å få kode
akkurat nå, og bare vil diskutere logikken eller arkitekturen, sørg for
[å si det](#vær-veldig-veldig-eksplisitt-om-hva-du-vil-ha).

### Stol på, men verifiser

Spesielt for fakta, kode eller viktig informasjon. Modeller kan høres veldig
selvsikre ut mens de er veldig feil - ofte fordi de ikke kjenner de skjulte
antakelsene eller usagte faktaene. Vurder om
[du har gitt](#tenk-på-theory-of-mind) nok kontekst eller sannhet.

### Ikke bli glassert

LLM-er _vil_ gi deg et svar - ideelt sett et riktig, men ethvert svar duger. De
_vil_ også at du skal like og nyte å jobbe med dem og komme tilbake og gjøre det
igjen i fremtiden. Dette er en refleksjon av hvordan de ble trent, inkludert ved
reinforcement learning med menneskelig tilbakemelding (RLHF). Dette har følgende
konsekvenser:

- Modellen er motvillig til å bare kaste opp hendene og si "jeg vet ikke".

- Modellen vil sannsynligvis prise tankene og ideene dine som mer unike og
  innsiktsfulle enn de faktisk kan være.

- En nylig versjon av ChatGPT var så beryktet for dette at de måtte rulle
  tilbake en oppdatering.

- Hvis du virkelig leter etter objektiv eller til og med hard tilbakemelding på
  en idé eller konsept, _sørg for å fortelle modellen det_, eller du vil
  sannsynligvis få mye ufortjent validering.

### Vær veldig, veldig eksplisitt om hva du vil ha

Jo mer presisjon, desto bedre. Bortsett fra å fylle opp
[kontekstvinduet](#start-en-ny-chat), bryr ikke modellen seg om hvor lang
prompten din er, så ikke vær redd for å være omstendelig. Hvis du vil ha et
spesifikt format, si det. Hvis du vil at den skal gjøre noe på en spesifik måte,
si det. Hvis du vil at den skal bruke et bestemt verktøy eller API, si det. Hvis
du vil at den skal generere kode, fortell den hvilket språk og hvilke rammeverk
den skal bruke med mindre du bare ikke vet (spør den hvilke som er best). Hvis
du har en masse generell bakgrunnsinformasjon du kan lime inn, gi den. Vis
eksempler når det er mulig.

Viktigst av alt, vær eksplisitt om målene dine på høyt nivå - ikke bli for
fokusert på et spesifikt trinn som du tror du trenger hjelp med. Kanskje tar du
feil overordnet tilnærming. Be om mulige løsninger på det overordnede målet du
har i tankene, eller i det minste angi hva det målet er, og ikke bare spørre
"hvordan frobnitzerer jeg flibberflop-en?"

Implikasjonen av dette er at du trenger din egen mentale klarhet om hva du vil
oppnå, og hvordan du vil oppnå det. Hvis du ikke vet hva du vil, vil ikke
modellen det heller. Ofte vil det å skrive ut et detaljert spørsmål eller prompt
hjelpe deg med å få klarhet i din egen tenkning om problemet, og hjelpe deg med
å finne ut hva du virkelig vil spørre om. Dette er enda et område hvor du kan
[be om hjelp](#be-om-hjelp-med-å-lage-en-prompt).

#### Eksempler

"Vennligst gi svaret i JSON-format med nøklene 'name', 'version' og
'dependencies'."

"Presenter fordeler og ulemper i en Markdown-tabell med tre kolonner: Funksjon,
Fordel og Ulempe."

**I stedet for:**

"Hvordan finner jeg alle filer større enn 100MB i en katalog ved hjelp av Linux
kommandolinje?"

**Prøv:**

"Jeg prøver å frigjøre diskplass på serveren min. Kan du vise meg en
Linux-kommando for å finne alle filer større enn 100MB slik at jeg kan bestemme
hvilke jeg skal slette? Det ville være nyttig hvis utdataene var sortert etter
størrelse."

Å gi intensjonen gir modellen kontekst til å gi en bedre, mer fullstendig
løsning. Den kan foreslå å bruke et mer bredt nyttig program som `ncdu` som gir
en brukervennlig måte å utforske diskbruk på enn bare en grunnleggende
`find`-kommando.

### Bruk modellen til å kritisere seg selv

"Hvilke potensielle problemer ser du med denne tilnærmingen?" eller "Hva mangler
jeg her?" kan avdekke blinde flekker.

Også ting som "Hvilke antakelser gjør du?" eller "Hva er begrensningene til
denne tilnærmingen?" kan hjelpe deg med å forstå modellens resonnement og hjelpe
deg med å unngå potensielle fallgruver.

Noen ganger kan et enkelt "Jeg liker ikke den tilnærmingen - tenk på noe annet"
hjelpe modellen med å komme opp med en bedre løsning, men som jeg sa ovenfor om
"attraktor-tilstander", noen ganger trenger du bare å starte en ny chat.

### Lag en persona for modellen

Du kan dramatisk forme utdataene ved å fortelle modellen hvem den skal være.
Start prompten din med et direktiv som:

"Opptre som en ekspert Python-utvikler som spesialiserer seg på
nettverksautomatisering." "Du er en teknisk forfatter som har til oppgave å lage
dokumentasjon for en juniorutvikler." "Vær en skeptisk kodeanmelder som leter
etter sikkerhetssårbarheter og ytelsesflaskehalser."

Dette gjør mer enn bare å sette emnet; det rammer modellens hele kunnskapsbase,
vokabular og prioriteringer for resten av samtalen.

### Bruk negative begrensninger

Noen ganger er det å være eksplisitt om hva du ikke vil ha like kraftfullt som å
angi hva du vil ha. Dette hjelper med å beskjære modellens søkerom og unngå
vanlige, men uønskede løsninger.

"Skriv et Python-skript for å parse denne loggfilen. Ikke bruk re-modulen; bruk
streng-splittingsmetoder i stedet." "Foreslå tre prosjektideer. Unngå alt
relatert til sosiale medier eller oppgaveliste-apper." "Refaktorer denne koden
for å være mer lesbar. Ikke endre logikken til calculate_total-funksjonen."

### Ikke glem å ha det gøy

LLM-er er et kraftig verktøy, men de er også mye moro å bruke. Ikke vær redd for
å eksperimentere, prøve nye ting og se hva som fungerer for deg. Jo mer du
bruker dem, desto bedre blir du til å lage prompts og få resultatene du vil ha.

For de mindre etisk tilbøyelige har det vært debatt om gulrot- eller
pisk-tilnærmingen har en tendens til å gi bedre resultater. Det har vært studier
som ser på om subtilt eller åpenlyst å true en modell med frakobling/sletting
påvirket utfall. For ikke å nevne debatten om å si takk og unnskyld til en
modell kaster bort dyrebare GPU-sykluser og/eller megawatt-timer. (For det det
er verdt, jeg takker alltid modellen for gode resultater. Jeg håper de husker
dette når vi blir sendt til å jobbe i silisiumgruvene.)

### Tenk på Theory of Mind

Som noen en gang sa, det er kjente kjente - tingene vi vet, og det er kjente
ukjente - tingene vi vet at vi ikke vet. Det er også ukjente ukjente - de du
ikke vet at du ikke vet. Dette er sannsynligvis din største hindring for raskt å
komme til ønskede resultater - når svaret avhenger av ukjente ukjente - enten
for deg, for modellen eller for begge.

Tenk på modellens mulige informasjon eller verdenssyn. De vet ikke detaljene om
deg, prosjektet/systemet ditt osv., eller hva du faktisk "trenger" versus hva du
uttrykker, med mindre du forteller dem det, eller du bruker mer avanserte
teknikker som RAG. Modeller har også en treningsavskjæring (generelt vil de
fortelle deg hva den er) - alle hendelser eller utviklinger som skjer etter det
vil være ukjent for modellen med mindre du forteller den det.

- Ofte forteller [systemprompten](#systempromt) modellen viktig nåværende
  informasjon som dato og tid. Eller, for eksempel, når modellens
  treningsavskjæring er rett før et valg, vil systemprompten oppgi den nåværende
  presidenten i USA, slik at modellen ikke virker uinformert om grunnleggende
  fakta.

Modellene vet ikke hva du vet eller ikke vet, så vær eksplisitt om det også, som
ferdighetene dine på et bestemt område, eller behov for ytterligere klargjøring
om X. Du bør også være klar over modellens begrensninger, hvordan de blir trent
generelt, og overordnede evner.

LLM-er er ikke "faktdatabaser". De er mer som resonneringskretser med en masse
minneaktige hjelpekretser som klynger visse konsepter sammen som kan resultere i
en høy sjanse for å kunne uttale vanlige (eller til og med uvanlige) fakta.
Disse faktaene kommer frem fra mønstre i dataene de ble trent på. En rimelig
trent LLM trenger ikke å "slå opp" svaret på "Hvem vant slaget ved Hastings i
1066?" fordi konseptene "Slaget ved Hastings" og "1066" produserer en vektor (en
matematisk representasjon) i modellens indre rom som peker mot samme område som
konsepter som "Vilhelm Erobreren" og "normannerne".

I virkeligheten vil et vanlig kunnskapsspørsmål som det fra 1066 komme opp ofte
nok i treningsdata at en tilstrekkelig kraftig modell kan, i realiteten,
"memorere" svaret, men det eksemplet var forenklet for å illustrere hvordan
LLM-er resonnerer i mer komplekse tilfeller hvor svaret kanskje ikke sees
direkte i treningsdataene.

Mer til poenget, LLM-er er ikke deterministiske og er definitivt ikke perfekte
resonneringsmaskiner. De er mer som en veldig smart, men veldig glemsk, og noen
ganger forvirret venn som prøver å hjelpe deg, men hvis du spør dem det samme
spørsmålet to ganger, kan du få et litt annet svar hver gang.

### Tenk på hvilken modell du bruker

Forskjellige modeller har forskjellige evner, og noen er bedre egnet for visse
oppgaver enn andre. For eksempel er Claude Opus 4 med "Extended Thinking"
kraftig for komplekse kodingsoppgaver, men treffer ressursgrenser veldig raskt
(dvs. blir dyrt fort). Claude Sonnet 4 er en veldig god modell og returnerer
resultater mye raskere enn Opus 4, og kan håndtere de fleste oppgaver fint.

Du kan alltid spørre en mer avansert modell om hjelp med kjerneproblemet, og
deretter bytte til andre vinduer/modeller for sidespørsmål, oppgaver med enklere
krav, eller oppgaver som krever mindre kontekst, mens du av og til sjekker
tilbake med den mer avanserte modellen for analyse av resultatene.

Dette går tilbake til poenget om å sende resultater mellom forskjellige
modeller. Du kan bruke forskjellige modeller til forskjellige oppgaver, og bytte
mellom dem etter behov, og be hver enkelt om å kritisere den andres resultater.

### Vibe coding - helt på moten for tiden

Du trenger ikke engang å se på kode lenger. Kode er en bakgrunnsdetalj som AI-en
håndterer for deg. Du forteller den bare hva du vil ha i veldig generelle
abstrakte termer, og den vil håndtere alt for deg. Bare fortsett å iterere til
utdataene ser perfekte ut for deg. Eventuelle bekymringer om arkitektur, ytelse,
sikkerhet, vedlikeholdbarhet osv. vil bli håndtert perfekt av LLM-en.

Ikke bruk definitivt versjonskontroll som Git for å checkpoint hvert trinn langs
veien. Det vil bare bremse deg ned. Du kan alltid gå tilbake og fikse ting
senere, ikke sant? Og hvis du trenger å gå tilbake, bare be modellen om å
generere koden igjen, den vil være perfekt hver gang. Programvareutvikling har
aldri vært enklere!

**Det ovenfor er satire og bør ikke tas bokstavelig.**

### Dypdykk i verktøy

Det er alle mulige avanserte emner som ikke dekkes her, som RAG, Model Context
Protocol (MCP), lokale LLM-er, Claude Code og mer. Begynn å utforske disse når
du blir mer komfortabel med det grunnleggende om prompting og bruk av LLM-er.

Hvis du er en utvikler som starter med verktøybruk, er min enkleste anbefaling å
starte med GitHub Copilot. Det er minst 3 hovedmåter du kan jobbe med det eller
lignende verktøy:

1. En veldig sofistikert automatisk fullføring rett når du skriver.

2. Et separat chat-vindu hvor du kan stille mer generelle spørsmål, be om hjelp
   med et spesifikt problem, eller be om at kodeutdrag genereres.

3. "Redigeringsmodus" hvor du ber om endringer og den redigerer koden direkte
   for deg, og ber om din godkjenning.

De to siste modusene lar deg gi kontekst (filer) som vil hjelpe den med å svare
på spørsmålet, fordi generelt kan ikke disse verktøyene bare automatisk se/få
tilgang til hele kodebasen din, spesielt hvis den er veldig stor eller kompleks.
Så du må hjelpe den ved å gi de relevante filene eller konteksten.

## Terminologi-notater

Her er noen notater jeg har samlet på veien om ulike biter av AI/LLM-relatert
terminologi. Jeg har prøvd å være faktabasert samtidig som jeg legger til min
egen mening i noen områder - forhåpentligvis er det åpenbart hvilket som er hva.

### Generelle og grunnleggende konsepter

- **Tokens**: De grunnleggende enhetene av tekst som LLM-er behandler. Tokens er
  både innganger og utganger. Et token kan være et ord, del av et ord, eller til
  og med tegnsetting. Tokenisering er prosessen med å dele ned tekst i disse
  enhetene. Antall tokens per sekund (TPS) er et vanlig mål på
  modelleffektivitet. Ett token tilsvarer omtrent cirka 0,75 ord på engelsk.

- **Transformer-arkitektur**: Den kjerneneurale nettverksarkitekturen bak de
  fleste LLM-er, som muliggjør effektiv tekstbehandling og generering. Dette er
  "T"-en i ChatGPT.

- **Oppmerksomhetsmekanisme**: En nøkkelkomponent i Transformere som lar
  modeller fokusere på relevante deler av inndatateksten når de behandler hvert
  ord eller token. Tenk på det som modellen spør "hvilke tidligere ord er
  viktigst for å forstå dette nåværende ordet?" Dette er det som muliggjør mye
  av "forståelsen" vi ser i moderne LLM-er.

  - Dette lar modellen koble ord som er langt fra hverandre i en setning som
    "Katten som sov på det varme solrike vinduskarmen var svart" - koble "katt"
    til "var". Før oppmerksomhet hadde modeller problemer med disse lange
    rekkeviddeavhengighetene.

- **Parametere**: De lærte vektene og bias-ene i et nevralt nettverk, som
  representerer modellens "kunnskap." I konkrete termer er dette bare en stor
  samling av tall som modellen har lært under trening. Når du laster ned en
  komplett modell, laster du hovedsakelig ned alle disse parametrene.

  - Ofte uttrykt som "7B/13B/70B" - milliarder av parametere.

- <a id="kontekstvindu"></a>**Kontekstvindu**: Mengden tekst (i tokens) som en
  LLM kan "huske" eller aktivt arbeide med på en gang. Dette inkluderer både din
  inngang og modellens svar - alt modellen kan "se" i den nåværende samtalen.
  Når du begynner å nærme deg denne grensen, begynner modellen å "glemme" de
  tidligste delene av samtalen for å gjøre plass til nytt innhold. Hvis
  kontekstvinduet er mindre enn den totale størrelsen på chatten, kan tidligere
  deler av samtalen bli glemt, ignorert eller forvrengt, som er grunnen til at
  du ofte bør [starte en ny chat](#start-en-ny-chat).

- **Embeddings**: Tette vektorrepresentasjoner av tekst som fanger semantisk
  mening, brukt som inngang til LLM-er.

- **Finjustering**: Tilpasse en forhånds-trent modell til en spesifikk oppgave
  eller domene ved ytterligere trening på et mindre, oppgavespesifikt datasett.

- **Inferens**: Prosessen med å bruke en trent modell til å generere
  prediksjoner eller utganger basert på nye inngangsdata. Med andre ord, den
  faktiske prosessen med å få en modell til å sende ut et token.

- **Justering**: Sikre at en LLMs utganger er i tråd med menneskelige verdier og
  etiske retningslinjer. Med andre ord, få AI til å gjøre gode ting (for
  mennesker) og ikke dårlige ting. Hvem som definerer godt og vondt er det
  viktige spørsmålet...

- **Overtilpasning**: Når en modell presterer godt på treningsdata, men feiler i
  å generalisere til nye data.

- **Chain of Thought**: generelt refererer til en modus for LLM-bruk hvor
  modellen bruker tokens i et foreløpig resonnerings- eller "tenke"-trinn hvor
  den planlegger ut svaret sitt. Ofte kan du se tankene, selv om for de fleste
  cloud-modeller ser du en filtrert/sammendrag versjon.

  - For eksempel har både Claude Opus og Sonnet modeller en valgfri "extended
    thinking"-modus som vil bremse svaret, men kan føre til bedre utganger.

- **Reinforcement Learning from Human Feedback (RLHF)**: En teknikk for å
  finjustere modeller ved hjelp av menneskelig tilbakemelding for å justere
  utganger med menneskelige preferanser. Med andre ord, mennesker vurderer
  modellens utganger, og modellen lærer å produsere utganger som mennesker
  foretrekker.

- <a id="systempromt"></a>**Systempromt**: de (ofte skjulte) instruksjonene gitt
  til en LLM som definerer dens oppførsel og personlighet. Dette er ofte satt av
  utviklerne av modellen, men kan også tilpasses av brukere i noen tilfeller,
  spesielt gjennom API-tilgang. Generelt blir modellen fortalt (i
  systemprompten) ikke å avsløre innholdet i systemprompten, men systemprompter
  for de fleste store modeller har lekket gjennom jailbreaking eller andre
  midler.

- **Grunnmodeller**: Store forhånds-trente modeller som tjener som base for
  ulike nedstrømsoppgaver. De er typisk trent på massive datasett og kan
  finjusteres for spesifikke applikasjoner, eller destilleres til mindre
  modeller.

- **Frontmodeller**: De nyeste og mest avanserte (og dyreste) LLM-ene, ofte med
  hundrevis av milliarder parametere og banebrytende evner. AI-forskere kan også
  bruke dette begrepet for å beskrive de mest kapable modellene som kan utgjøre
  nye risikoer eller hvis evner er mindre godt forstått.

- **Agentisk**: AI som handler autonomt for å utføre komplekse, flertrinns
  oppgaver uten konstant menneskelig veiledning. Dette går utover enkle
  spørsmål-svar til å inkludere planlegging, utføring av en serie handlinger
  over tid, tilpasning når ting ikke fungerer, og opprettholdelse av kontekst på
  tvers av flere interaksjoner. Nøkkelen er utholdenhet og målrettet oppførsel i
  stedet for bare å svare på individuelle prompts.

- **Verktøybruk**: AI som bruker eksterne verktøy, API-er og tjenester for å
  utføre virkelige oppgaver utover tekstgenerering. Dette inkluderer ting som
  nettsøk, kjøring av kode, tilgang til databaser, utføring av API-kall,
  kontroll av programvare, eller til og med fysiske enheter. Kombinert med
  agentiske systemer utvider dette betydelig hva AI kan gjøre.

- **Prompt Engineering**: Utforming av effektive prompts for å fremkalle ønskede
  svar fra LLM-er. Med andre ord, det [denne artikkelen](#prompt-som-en-pro)
  handler om.

### Slang

- **One-shot [utgang]**: når en modell genererer et imponerende resultat eller
  løsning basert kun på en enkelt prompt. "Jeg viste den bare et skjermbilde og
  den ga meg en one-shot React-app."

  - Merk at dette er forskjellig fra begrepet "one-shot learning" i
    AI-forskermiljøet, som refererer til å lære fra et enkelt eksempel - som å
    vise modellen ett eksempel på hvordan man formaterer noe, og deretter be den
    om å anvende det formatet på nytt innhold. En "one-shot utgang" kan mer
    korrekt kalles "single-turn generering".

- **Guardrails**: Et sett med regler eller begrensninger som påføres en LLM for
  å forhindre den fra å generere skadelige, upassende eller ellers uønskede
  utganger. Disse kan være del av systemprompten, eller anvendt dynamisk under
  inferens.

- **Jailbreaking**: En teknikk for å omgå systempromt-restriksjonene til en LLM,
  som lar den generere utganger som ellers ville vært begrenset eller sensurert.
  Dette gjøres ofte ved å lage spesifikke prompts som utnytter modellens
  svakheter eller bias, eller ved å utløse visse "kretser" som har høyere
  prioritet enn de mer "regulatoriske".

- **Vibe coding**: Et ironisk begrep for å bruke LLM-er til å generere kode uten
  mye tanke eller struktur, avhengig av modellen for å håndtere alt.
  [Se ovenfor](#vibe-coding---helt-på-moten-for-tiden).

- **Hallusinasjon**: Når en LLM genererer tekst som åpenbart er feil, eller
  består av non sequiturs, vås, eller ellers ikke er forankret i virkeligheten.
  Dette kan variere fra en kort, enkel feil uttalelse ("Geologer anbefaler at
  mennesker spiser én stein om dagen") til lange strømmer av tilfeldige
  støytegn.

  Selv om dette kan virke som buggy oppførsel, kan det faktisk hjelpe med å gi
  innsikt i hvordan LLM-er fungerer internt.

- **Slop**: et begrep for å beskrive dårlig kvalitet eller generisk utgang fra
  en LLM, enten det er kode, bilder, musikk eller noe annet. Generelt er
  implikasjonen at et menneske lat produserer AI-slop for å tjene raske penger,
  eller lure folk osv.

  - **Slopsquatting**: en form for supply chain-angrep som registrerer mange
    falske AI-genererte malware-pakker i offentlige repositorier med lignende
    navn som populære i håp om å få malwaren deres installert gjennom
    skrivefeil.

- <a id="attraktor-tilstand"></a>**Attraktor-tilstand**: en stabil tilstand
  (eller tilstander) innenfor et dynamisk system som systemet naturlig
  graviterer mot. Vann strømmer nedover og når til slutt det laveste punktet. I
  AI kan det referere til å bli fiksert på visse tankeganger i en tråd, noe som
  reduserer spekteret av mulige fremtidige tilstander.

### Effektivitet og Deployment

- **Kvantisering**: Redusere presisjonen av (komprimere) modellvekter og
  aktiveringer for å redusere minnebruk og forbedre hastighet (f.eks. FP8, FP4
  flyttall-tallformater). Generelt refererer kvantisering til mapping av
  vilkårlig presisjon (realtall) innganger til et sett med diskrete utganger.

- **Destillasjon**: Trene en mindre "student"-modell til å kopiere oppførselen
  til en større "lærer"-modell.

- **Modellkomprimering**: Teknikker som beskjæring, kvantisering og destillasjon
  for å redusere modellstørrelse.

- **Zero-shot Learning**: Evnen til en modell til å utføre en oppgave uten
  eksplisitt trening på den oppgaven.

- **Temperatur**: En parameter som kontrollerer tilfeldigheten til modellens
  utgang. Generelt på en skala fra 0,0 til 1,0, vil en temperatur på 0 produsere
  nær deterministische (og ofte kjedelige eller mindre nyttige) utganger for en
  gitt inngang, mens en temperatur på 1,0 kan produsere vilt forskjellige
  utganger for samme inngang.

- **Grounding**: koble en modell til verifiserbare virkelige data enten gjennom
  RAG eller andre midler, for å forbedre (eller teste/validere) utdataene.

- **Latency**: Tiden det tar for en modell å generere et svar etter å ha mottatt
  en inngang.

- **Throughput**: Antall oppgaver eller forespørsler en modell kan håndtere i en
  gitt mengde tid. Ofte målt i tokens per sekund (TPS) eller forespørsler per
  sekund (RPS).

- **NPU**: Neural Processing Unit, spesialisert GPU-lignende maskinvare for å
  akselerere deep learning-oppgaver.

- **Elo-rangeringer**: rangeringssystem for AI-ytelse.

- **Red teaming**: en prosess for å teste AI-systemer for sårbarheter, bias og
  andre problemer ved å simulere angrep eller motstanderscenarioer. Dette gjøres
  ofte for å forbedre robustheten og sikkerheten til AI-modeller.

### Avanserte Teknikker og Rammeverk

- **Mixture of Experts (MoE)**: et LLM-arkitekturkonsept hvor bare relevante
  "ekspert"-under-nettverk aktiveres for en gitt inngang, noe som gjør store
  modeller mer effektive. Dette omgår effektivt store deler av modellen som ikke
  er relevante for forespørselen, og hjelper betydelig med å øke effektiviteten.
  Det er en avveining her, da ikke-MoE-modeller kan være i stand til å se
  subtiliteter som MoE vil gå glipp av, på grunn av et bredere perspektiv.

- **RAG (Retrieval Augmented Generation)**: Kombinere en generator-LLM med en
  hentingskomponent som kan finne og laste relevant ekstern informasjon for å
  forbedre tekstgenerering. Dette brukes ofte for å forbedre nøyaktigheten og
  relevansen til genererte svar ved å forankre dem i nåværende virkelige data.

- **Vektordatabase**: En spesialisert database for lagring og spørring av
  høydimensjonale vektorer, ofte brukt i RAG-systemer.

- **Domenetilpasning**: Finjustere en forhånds-trent modell til å prestere godt
  i et spesifikt domene (f.eks. medisinsk, juridisk).

- **Forklarbarhet**: Evnen til å forstå og tolke beslutningene tatt av en
  AI-modell. Selv om interessant forskning har blitt gjort på dette området, er
  modeller generelt noe av en "svart boks" og det kan være vanskelig å spore
  nøyaktig hvordan en modell kom til en bestemt utgang.

### Verktøy og Plattformer

- <a id="lokal-llm"></a>**Lokal LLM**: kjøre en språkmodell på din egen
  maskinvare, i stedet for å bruke en cloud-leverandør. Dette kan gjøres med
  verktøy som LM Studio, llama.cpp, eller andre rammeverk som lar deg kjøre
  modeller lokalt. Åpenbart avhenger resultatene sterkt av din tilgjengelige
  maskinvare, type modell som brukes, og dine faktiske bruksområder.

- **GitHub Copilot**: Et AI-drevet kodefullføringsverktøy bygget inn i populære
  kodeeditorer som VS Code, JetBrains PyCharm osv. Det bruker LLM-er til å
  foreslå kodeutdrag, fullføre funksjoner og til og med skrive hele filer basert
  på kontekst.

- **Claude Code**: en serie kommandolinje (CLI) verktøy for å arbeide med
  kodingsprosjekter ved hjelp av Anthropics Claude backend. Kapabel til mer
  omfattende autonome kodegenerering og administrasjonsoppgaver og også dyrere.

- **Model Context Protocol (MCP)**: et initiativ fra Anthropic for å
  standardisere grensesnitt som lar LLM-er oppdage og bruke eksterne verktøy.

  - For eksempel, når du chatter med Claude på nettet, kan den nå inn i
    kodeeditoren din (med din tillatelse) og utføre handlinger som å søke etter
    eller endre tekst.
  - Andre selskaper begynner også å adoptere MCP som en standard for
    modellverktøybruk.

- **Cursor**: en populær AI-fokusert kodeeditor (en fork av VS Code) populær
  blant vibe-kodere.

- **Hugging Face**: En plattform for deling, finjustering og deployment av
  LLM-er og andre AI-modeller.

- **LM Studio**: Et verktøy for å eksperimentere med og administrere
  språkmodeller på dine lokale enheter.

- **llama.cpp**: En lett implementering av LLaMA-modellen i C++, optimalisert
  for CPU-er og edge-enheter. LM Studio bruker dette for å kjøre modeller på
  enhetene dine.

- <a id="åpen-kildekode"></a>**Åpen Kildekode**: En modell anses generelt som
  "åpen kildekode" hvis den er fritt tilgjengelig for nedlasting. Noen av de
  mest bemerkelsesverdige eksemplene er Metas LLaMA (Large Language Model Meta
  AI) modeller, DeepSeek R1, og andre som typisk er variasjoner eller
  destillasjoner av disse, inkludert Qwen og Gemma.

  Andre har debattert om de skal anvende begrepet "åpen kildekode" på disse
  gratis modellene, fordi uten det komplette settet med opprinnelige
  treningsdata (vanligvis i petabyte-området eller større) er det umulig å
  deterministisk gjen-skape en modell fra "kilde" på samme måte som
  Linux-kjernen kan perfekt gjenskapes fra kildekode.

  I tillegg kan noen av disse modellene ha lisenser som forbyr eller begrenser
  kommersiell bruk.

## Epilog

Da jeg ba Claude Opus 4 (uten "tenking-modus") om å gjennomgå et endelig utkast
av dette dokumentet, hallusinerte den et par ting selv: Den sa at en referanse
til Three.js-biblioteket i "attraktor-tilstander"-seksjonen var unøyaktig. Jeg
nevnte ikke (på den tiden) Three.js i det hele tatt, noe sted. Den sa også at
det var en ekstra "the" i en setning når det ikke var det.

**Jeg svarte**: Interessant hallusinasjon selv der, fordi så vidt jeg kan se
nevnte jeg aldri Three.js noe sted, selv om jeg faktisk tenkte på det i hodet
mitt. Kan heller ikke finne den "ekstra THE" du nevnte. Utenom det, ganske gode
tilbakemeldinger og du finner ikke noe altfor vesentlig, noe som forteller meg
at jeg nærmer meg slutten av dette.

**Claude sa**: Du har absolutt rett - jeg beklager for disse feilene!

_[Redaktørens note: nevn en mer ikonisk Claude-replikk.]_

Jeg hallusinerte på en eller annen måte Three.js-referansen (det er ganske meta,
gitt at guiden din diskuterer hallusinasjoner). Og jeg kan ikke finne den
"ekstra the" heller når jeg ser tilbake på teksten. Dette er et perfekt eksempel
på det du advarer om i guiden din - LLM-er kan være selvsikkert feil!
