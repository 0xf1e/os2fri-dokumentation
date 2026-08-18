---
marp: true
theme: default
paginate: true
---
<script type="module">
  import mermaid from 'https://cdn.jsdelivr.net/npm/mermaid@11.16.1/+esm';
  mermaid.initialize({ startOnLoad: true });
</script>

# Vigtige designovervejelser

Hvordan får vi en maskine i en bestemt tilstand?

### Mønster A: Agent som administratorens "forlængede arm"

<!-- Mønster A: En agent på maskinen fungerer som administratorens "forlængede arm" og kører kode, der er fastlagt af et centralt system under administratorens kontrol -->

<div class="mermaid">
flowchart LR
    subgraph Machine ["Maskine"]
        Agent[agent]
    end
    Admin(("admin")) -->|indtaster kommandoer i| ControlSystem[styringssystem]
    ControlSystem -->|sender kommandoer til| Agent
    Agent -->|udfører kommandoer på| Machine
</div>

---

### Mønster B: Standardiserede deklarationer

<!--Mønster B: En ønsket maskintilstand for en hel flåde af maskiner defineres. Maskinen konfigureres til at følge den rette maskintilstandskanal og holder sig derefter opdateret via kanalen.
-->

<div class="mermaid">
flowchart LR
    subgraph Machine ["Maskine"]
        Updater[opdateringsprogram]
    end
    Updater -->|erstatter systemet på| Machine
    Updater -->|henter den seneste deklaration fra| ImageStream[image stream]
    Admin(("admin")) -->|opdaterer deklarationen i| ImageStream
</div>

-> Vi anbefaler Mønster B.

**Hvorfor?** Pålidelighed, Sikkerhed, Effektiv drift, Lovgivningsmæssig efterlevelse, Driftskontinuitet

<!--
Vi fravælger Mønster A. I stedet udvider vi Mønster B, så det løser alle de problemer, der er relevante for os2fri, og som kræver interaktion med individuelle maskiner.

**Hvorfor?**
- sikrer, at der er en godkendt systemtilstand, som er beskrevet ét sted (single source of truth) -> leverer på Pålidelighed, Sikkerhed, Effektiv drift, Lovgivningsmæssig efterlevelse, Driftskontinuitet
vi vil undgå:
- maskiner begynder at afvige lidt fra hinanden på måder, som kun den person, der udførte ændringen, forstår
- udokumenterede ændringer på nogle maskiner forårsager kompatibilitetsproblemer ved opdatering
- uautoriserede aktører kompromitterer den aktør, der har fuld kontrol, og foretager farlige ændringer i systemet
- viden om, hvordan systemer fungerer, går tabt, når kommuner skifter leverandør, eller nøglemedarbejdere skifter job
vi vil have:
- enhver maskine kan bringes tilbage til sin ønskede tilstand
- det er nemt at indsamle dokumentation for, hvilken software der kører på maskinerne i flåden, og hvordan den er konfigureret

Vi vil nu beskrive, hvordan vi løser individuelle problemer inden for rammerne af Mønster B.
-->

---

# Forbehold om de beskrevne problemer

Et softwaresystem bør designes til at løse specifikke problemer.
Under designprocessen for os2fri deltog en Product Owner fra et relateret projekt i teamet, men ellers havde vi ikke mulighed for at tale med systemets faktiske aktører.
Derfor må vi _gætte på_, hvilke problemer der er vigtige nok til at behandle her.

Under selve udviklingen skal brugerrepræsentanter inddrages, så de reelle problemer kan undersøges og prioriteres.

<!-- Mens vi tænkte over, hvordan os2base skulle implementeres, fremstod følgende problemer som vigtige nok til at håndtere tidligt: -->

---

# Problem: Begrænsede systemer vs. slutbrugerbehov

<!--
👸 Slutbruger
🧑‍💻 Admin
-->

🧑‍💻 "Jeg kan ikke skrive en systemdeklaration for hver enkelt medarbejder på rådhuset, så jeg skriver én deklaration, der skal fungere for alle."
👸 "Jeg er nysgerrig på at afprøve et vektorredigeringsprogram i mit arbejde. Forhåbentlig kan jeg bare installere og afprøve et sådant program, uden at det bliver en stor bureaukratisk proces."

---

## Løsningsdesign

<!--Vi definerer grupper af maskiner med lignende konfigurationer på følgende måde: Systemdeklarationer kan arve fra andre systemdeklarationer, og der kan defineres undersystemer af eksisterende systemer. -->

Eksempel: Indskolings-PC hos Aarhus Kommune:

<div class="mermaid">
flowchart LR
    Machine["Maskine"]
    subgraph ownerOS2Base ["OS2Base"]
        OS2BaseDecl["OS2Base\n(deklaration)"]
    end
    subgraph ownerOS2Skole ["OS2Skole"]
        OS2SkolePcDecl["OS2SkolePC\n(deklaration)"]
    end
    subgraph ownerAarhus ["Aarhus Kommune"]
        direction TB
        SkoleAtAarhusDecl["Skole@Aarhus\n(deklaration)"]
        IndskolingAtAarhusDecl["Indskoling@Aarhus\n(deklaration)"]
    end
    Machine -->|følger image stream fra| IndskolingAtAarhusDecl
    IndskolingAtAarhusDecl -->|arver fra| SkoleAtAarhusDecl
    SkoleAtAarhusDecl -->|arver fra| OS2SkolePcDecl
    OS2SkolePcDecl -->|arver fra| OS2BaseDecl
</div>

---

For at undgå at ende med én deklaration pr. maskine:

### Tilføjelsesmønster

<div class="mermaid">
flowchart LR
    subgraph Machine ["Maskine"]
        direction TB
        guaranteed["Konfigurationsafstemt tilstand"]
        compartment["Brugertilføjelser"]
    end
</div>

<!-- Administratoren kan tillade slutbrugeren at _udvide_ det eksisterende system, for eksempel ved at tilføje softwareinstallationer, men ikke at _fjerne_ dele af den konfigurationsafstemte tilstand.

Eksempel: Bruger "Jeg vil gerne kunne installere software, der er relevant for mit brugsscenarie"
Løsning: Stil en liste over apps, der må installeres på systemet, til rådighed. Disse apps må ikke ændre nogen af de parametre, som administratoren er afhængig af (der findes allerede sandbox-løsninger til dette)
-->

### Referencemønster

<div class="mermaid">
flowchart LR
    subgraph Machine ["Maskine"]
        guaranteed["Konfigurationsafstemt tilstand"]
    end
    modifiableSystem["Modificerbart system\n(indeholder brugervalg)"]
    guaranteed -->|omdirigerer til| modifiableSystem
</div>

<!-- Et andet eksempel: Bruger "Jeg vil gerne kunne bruge den rigtige printer på mit kontor"
Løsning: Gør systemet uafhængigt af, hvilken specifik printer en bruger anvender. En maskine kan få adgang til en lang liste over printere, og eventuelle adgangsrettigheder håndteres på printer- eller netværksniveau. -->

---

# Problem: Hvordan ved vi, hvilken tilstand flåden er i?

<!--
👸 Slutbruger
🧑‍💻 Admin
👩‍💼 CISO
-->

👸 "Av, computeren på biblioteket er lige gået ned! Forhåbentlig bliver nogen hurtigt informeret om det."
👩‍💼 "Nogen har opsat et falsk wi-fi-netværk på vores kontor! Heldigvis kan jeg slå op, hvilke netværk maskinerne på kontoret har været forbundet til."
🧑‍💻 "Efter vores seneste opdatering har nogle klaget over problemer med printerforbindelsen. Men for at finde ud af, hvad der er galt, har jeg virkelig brug for at se, hvilke fejl folk får på deres computere."

---

## Løsningsdesign

<!--
- hver maskine stiller standardiserede observationsdata til rådighed
- der findes et centralt observationssystem, der indsamler alle disse data
- observationsdata konverteres til et standardiseret format

Informationsflowet bliver således:
-->

<div class="mermaid">
flowchart LR
    Machine -->|sender standardiserede brugsdata til| ObservabilitySystem[Observationssystem]
    Machine -->|henter image stream-patches fra| ImageRegistry[Image Registry]
</div>

Påkrævede maskinkomponenter:

- Maskinedataindsamler <!-- en komponent, der indsamler maskinadfærd, konverterer denne adfærd til det standardiserede brugsdataformat og sender det til observationssystemet -->
- Opdateringstjekker <!-- en komponent, der regelmæssigt tjekker, om der findes nye image-patches, downloader dem og anvender dem på maskinen -->

---

# Problem: Problemfri konfigurationsoplevelse

<!--
🧑‍💻 Admin
👩‍💼 CISO
-->

Forventede problemer:

🧑‍💻 "Når jeg vil ændre flådens konfiguration, ved jeg, at jeg kan finde den rette indstilling i konfigurationsgrænsefladen. Det er min one-stop-shop for enhver form for konfiguration."
🧑‍💻 "De bruger sikkert alverdens komplicerede teknologi til at få systemet til at fungere, men heldigvis behøver jeg ikke at lære om noget af det for at ændre konfigurationen."
👩‍💼 "For nylig blev der installeret software på maskinerne i flåden, og jeg vidste ikke rigtig, hvorfor den var der. Heldigvis kunne jeg nemt tjekke, hvem der tilføjede denne konfiguration, hvem der godkendte den, og hvornår ændringen blev foretaget."
👩‍💼 "Jeg vil være sikker på, at ingen af mine medarbejdere kan ændre flådens konfiguration uden at få en anden til at gennemgå ændringen."

---

## Løsningsdesign

<!--
- konfigurationsstyring er baseret på en eksisterende VCS-løsning som Git. **Hvorfor?** Denne løsning giver os sporbar historik, tilbagerulning, gennemgang og godkendelse af ændringer gratis
- som en del af os2fri oprettes et tyndt lag oven på Git, der gør konfiguration lettere for lokale administratorer
- os2base leverer UI-logikken til konfigurationsgrænsefladen og undersøger det rette visuelle/oplevelsesmæssige designsprog
- UI-logikken konverterer et standardiseret mellemformat til en brugergrænseflade
- de enkelte os2fri-produkter eksponerer konfigurationselementer i det standardiserede mellemformat
-->

<div class="mermaid">
flowchart LR
    admin["Admin"]
    configurationUI["Konfigurations-UI"]
    subgraph gitForge["Git Forge"]
        localConfiguration["Lokal konfiguration (Git Repo)"]
    end
    admin -->|bruger| configurationUI
    configurationUI -->|tilføjer til| localConfiguration
</div>

Sådan sikres et problemfrit konfigurations-UI på tværs af projektgrænser:

<div class="mermaid">
flowchart LR
    subgraph OS2Base["OS2Base"]
        uiDeclarationsBase["UI-paneldeklarationer"]
    end
    subgraph OS2BorgerPC["OS2BorgerPC"]
        uiDeclarationsBorgerPC["UI-paneldeklarationer"]
    end
    configurationUi["Konfigurations-UI"]
    configurationUi -->|indlæser UI-konfiguration fra| uiDeclarationsBase
    configurationUi -->|indlæser UI-konfiguration fra| uiDeclarationsBorgerPC
</div>

---

# Problem: Fjernstyret ændring af systemkonfiguration

<!-- 🧑‍💻 Admin -->

Forventede problemer:

🧑‍💻 "Maskinerne med numrene 100 til 110 følger 'børnebibliotek'-kanalen, men de skal nu alle følge 'voksenbibliotek'-kanalen. Jeg kan udføre denne ændring uden at skulle tage ud til stedet."
🧑‍💻 "Når skoleåret er slut, kan jeg nemt nulstille alle elevmaskiner til standardtilstand, så de er klar til nye elever næste år." ("Powerwash")

---

## Løsningsdesign

**Vigtigt:** Hvis kravet om "fjernstyring" ikke er lige så strengt, er andre designs bedre end dette!

<div class="mermaid">
flowchart LR
subgraph machine["Maskine 01 (kører system-a)"]
    systemA["system-a (kørende)"]
end
subgraph systemAStream["system-a – seneste deklaration"]
    direction TB
    conf["System A-konfiguration"]
    reset["Logik, der får Maskine 01 til at nulstille til system-b-imaget"]
end
systemA -->|henter opdatering fra| systemAStream
</div>

<!--
De konkrete ændringsmekanismer skal undersøges på forhånd, hvorefter den nødvendige logik tilføjes systemet.

Fordi et bestemt system kun henter den konfiguration, som andre systemer også henter, kan problemerne løses således:

- de enkelte ændringsmekanismer er forudkonfigureret i imaget
- ved hver image-opdatering downloader alle maskiner en liste, og denne liste indeholder ID'erne på de maskiner, som ændringen skal anvendes på
- dette betyder, at hver maskine har en form for unik og uforanderlig identifikation

**Vigtigt:** Maskinspecifikke ændringer flytter kun maskinen til den tilstand, der er defineret af en kendt, godkendt image stream.
-->
