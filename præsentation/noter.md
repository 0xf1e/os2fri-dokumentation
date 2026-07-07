## Fælles Brugeroplevelse / Admin-oplevelse

- Personaer
  - Bruger og Admin — men hvilke andre 'Personaer' definerer vi?
- Brugeroplevelsen: 'Win10-lignende'
  - Vi forventer, at selve OS-grænsefladen giver anledning til få problemer; brugerfeedback bør rettes mod de mere udfordrende komponenter.
- Admin-oplevelsen: Skræddersyet platform, tilgængelighed
  - Hvorfor? Fordi administratorer ikke vil føle sig hjemme i en Git-grænseflade.
  - Vi skal spørge ind til, hvad administratorernes ideelle admin-UI ville se ud, uafhængigt af, hvordan vores backend ser ud.
  - Inspiration kan hentes fra Google-/Chromebook-grænsefladen, som administratorer er meget tilfredse med.
- Hardware-complianceliste
  - Der er efterspørgsel på både x86- og arm64-kompatibilitet. (Kiosk-sammenhæng uden brugerinteraktion, hvor brugerne kører RPI4/5.)
  - Kan vi levere en delmængde af værdien til ikke-kompatible enheder? Hvordan håndterer vi BYOD-scenariet? -> Web- og cloudbaserede løsninger fortjener særligt fokus.
- Tilgængelighed
  - Overvej EU-regler og danske designstandarder (dette kan påvirke vores valg af skrivebordsmiljø).
- Hvor er brugerinput mest vigtigt? (prioritering)
  - Vi bør spørge ind til brugerbehov frem for at forsøge at kopiere al funktionaliteten fra en Windows-løsning. Men nogle beslutninger må vi træffe selv af effektivitetshensyn.
- Hvem snakker vi om? Hvilken type bruger bygger vi til?
  - For at optimere bruger- og admin-oplevelsen: definer målbrugeren, og byg specifikt målrettet disse brugere.

## Fælles Standarder & Compliance

- NIS2 / GDPR parat
  - 'Compliance by design'. Løsningen er designet, så NIS2/GDPR-compliance bliver et billigt biprodukt frem for en ekstern komponent, der skal håndteres separat.
- Forsyningskædesikkerhed
- Sikkerhed
- Digital suverænitet
- Compliance – NIS2 / GDPR / CRA, reviderbarhed
- Uafviselighed (non-repudiation)
  - Alle handlinger i kode og system skal kunne henføres til en bestemt aktør.
- Facilitering af compliance-processen
  - Reguleringer kræver specifikke processer og handlinger. Vores løsning bør designes, så disse processer og handlinger bliver nemme.
- Hvordan automatiserer vi compliance?
  - Dette er en opgave for en senere version.
- Politikker
  - Hvordan kan vi lade folk definere compliance-politikker?
- Eksponering af data-endpoints
  - Vores system skal levere alle de endepunkter, der skal kunne revideres. Selve revisionsværktøjet er out of scope.

## Fælles Funktionalitet

- Standard infrastruktur
  - En betragtelig del af udviklingsarbejdet mangler stadig.
  - IAM / Git Forge / Build Pipeline
- Standard-OS + software
  - Vi forventer at blive enige om et fælles OS-deklarationssystem.
  - UX og browser bliver en del af OS2base.
- Undgå 'configuration drift'
- Hvilke organisationer ejer hvilke opgaver? Hvordan håndterer vi data/opgave-overdragelse?
  - Især fordi nogle projekter kan være bedre finansieret end andre og/eller have forskellige udviklingsstrategier.

### Standard infrastruktur

- GitOps-udvikling vs. GitOps-konfigurationsstyring
  - I forbindelse med et GitOps-mandat kan vi måske pålægge det ene, men ikke det andet.
- Forge
  - Dette er en runtime-afhængighed; suverænitet er afgørende.
- Base-OS / Opdateringsflow / Beslutninger
  - Opdateringer i core-OS-laget risikerer stort båndbreddeforbrug og ustabilitet. Hvor meget kontrol ønsker vi over udrulningen af core-OS-programmer?
- IDENTITETSSTYRING / SIGNEREDE COMMITS

### Standard-OS + software

- Brugerisolation vs. Administration
  - Sikkerhedsudfordring: hvordan lader vi administratorer udføre deres arbejde uden samtidig at åbne døren for angreb?
- OS-deklaration er fælles anliggende – hvorfor?
  - Vi har besluttet, at valget af OS-deklarationsteknologi bliver en del af OS2base. Det betyder, at der skal tages stilling til bootc vs. nix.
  - Projekter kan overskrive vores OS-deklaration – helt ned til at bruge en anden basisteknologi – men det kræver en indsats fra deres side.
- Skal UI være fælles eller ej?
  - Ja, de fleste løsninger har brug for et skrivebordsmiljø og har lignende behov.
- App Store
  - Leverer løsningsskabeloner. Skabelonerne hører under child-projekterne, men strukturen leveres af OS2base. Installationsmetode for apps: åbent spørgsmål.

## Fælles Proces & Mennesker

- Dokumentation
  - Ved at levere relevant dokumentation gør vi det muligt for leverandører at yde support og service.
- Skabeloner
  - Vi hjælper administratorer og leverandører med at opretholde vores designmønstre ved at levere brugervenlige skabeloner.
- Ændringsstyring
- Proces / Way of Work / SDLC
- GitOps-mandat eller ej?
- Change Management: Hvordan kan vi få ændringerne til at lykkes?
  - Identificer smertepunkter og lyt til brugerne.
- Hvordan gør vi det nemt at arbejde på den måde, vi anbefaler?
- Brugersupport – Hvor er snitfladerne for OS2base-projektet?
  - Dokumentation. Men hvor handlingsorienteret skal denne dokumentation være? Skal der for eksempel være flowdiagrammer over administrative handlinger?
- Hvilke principper fører til fleksibilitet? (uden at det hele brækker)
  - Omkostningerne ligger på domænelaget (child-projekter).
  - Lagvis overskrivning eller fork (afhængigt af implementeringen).
  - Vi forventer, at child-projekter vil ønske at foretage justeringer, men de må selv vurdere, om det giver mening for dem at vedligeholde deres ændringer.
  - Vi tager ikke tunge arkitektoniske forholdsregler for at gøre skræddersyede ændringer nemme; i stedet støtter vi os til open source og dokumentation.
- Forsyningskædesikkerhed og daglig compliance
  - Hvordan forhindrer vi administratorer i at foretage usikre ændringer?

## Fælles Driftsmodel

- Mål: Undgå afvigelser mellem leverandørernes opsætninger.
- Mål: Undgå leverandør-lock-in.
- Nogle kommuner vil (måske) dele leverandører, andre ikke.
- Hvilke administrationsopgaver ejes af administratorerne vs. leverandørerne?
  - Administratorer bør ikke skulle bede leverandører om hjælp hele tiden.
