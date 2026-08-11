# Opgavebeskrivelse til De Fire Arkitekter 
## Forberedelse af arkitekturworkshop om OS2fri og OS2base

## 1. Baggrund 
OS2fri er OS2’s rammeprogram for tre selvstændige initiativer: OS2skole, OS2borgerPC og 
OS2admPC. 

De tre initiativer har hver sin målgruppe, økonomi, organisering og governance. De skal fortsat 
kunne drives som selvstændige projekter. OS2fri skal sikre, at det, der med fordel kan være fælles, 
bliver udviklet, beskrevet og forvaltet fælles. 

Som fælles teknisk og organisatorisk bund etableres OS2base. OS2base skal ikke udvikle alt for 
alle. OS2base skal afklare og samle de elementer, hvor der er et reelt fælles behov, og hvor en 
fælles løsning skaber større værdi end parallel udvikling i hvert projekt. 

Der gennemføres en arkitekturworkshop den 8. september 2026. Workshoppen skal skabe 
grundlag for konkrete beslutninger om den fælles arkitektur, OS2base og de væsentligste fælles 
valg på tværs af OS2skole, OS2borgerPC og OS2admPC. 

Forud for workshoppen skal De Fire Arkitekter udarbejde et fælles fagligt beslutningsoplæg. 
Oplægget skal bygge på eksisterende materiale, især fra OS2skole og OS2borgerPC, og skal gøre de 
vigtigste arkitekturvalg tydelige. 

## 2. Formål med opgaven 
De Fire Arkitekter skal udarbejde ét samlet beslutningsoplæg til arkitekturworkshoppen.

Oplægget skal gøre det muligt at træffe oplyste beslutninger om, hvad der skal være fælles i 
OS2fri, hvad der skal ligge i OS2base, og hvad der fortsat skal ligge i de enkelte projekter. 

Opgaven er ikke at gentænke OS2skole, OS2borgerPC eller OS2admPC fra bunden. Opgaven er at 
identificere de fælles arkitektoniske valg, der skal træffes nu, så projekterne kan udvikle sig i 
samme retning uden at miste deres egne målgrupper, leverancer og styringsrum. 

Arkitekterne skal derfor både kvalificere fælles tekniske valg og beskrive de steder, hvor der er 
reelle alternativer eller faglig uenighed. 

## 3. Udgangspunkt for arbejdet 
Arkitekterne skal sætte sig grundigt ind i det eksisterende materiale fra OS2skole og OS2borgerPC. 
Det materiale udgør et centralt grundlag for opgaven. 

Materialet skal bruges til at forstå de allerede beskrevne behov, målgrupper, brugerrejser, 
funktionelle krav, tekniske valg, driftsbehov og arkitekturovervejelser. Arkitekterne skal ikke starte 
med et blankt ark, men bygge videre på det arbejde, der allerede er lavet. 

Hvis der findes relevant materiale fra OS2admPC, skal det også indgå. Hvis OS2admPC er mindre 
modent eller mindre beskrevet end de øvrige initiativer, skal arkitekterne markere det som en 
forudsætning eller mangel i beslutningsoplægget. 

Funktionalitet og brugeroplevelse skal behandles som kendte hensyn. Det betyder, at arkitekterne 
ikke skal definere den samlede funktionalitet eller brugeroplevelse på ny. De skal i stedet vurdere, 
hvordan den fælles arkitektur, OS2base og eventuelle fælles komponenter bedst understøtter de 
behov og brugeroplevelser, der allerede er beskrevet i projekterne. 

## 4. Opgavens hovedspørgsmål 
De Fire Arkitekter skal besvare følgende hovedspørgsmål: 

Hvilken fælles arkitektur, hvilke fælles komponenter og hvilke fælles principper bør OS2fri etablere 
gennem OS2base, så OS2skole, OS2borgerPC og OS2admPC kan bevare deres egne målgrupper og 
leverancer, men samtidig opnå størst mulig genbrug, sammenhæng, sikkerhed, driftbarhed og 
leverandøruafhængighed? 

## 5. Metode 
Arbejdet gennemføres som en beslutningsrettet arkitekturvurdering. 

Metoden skal sikre, at arkitekterne ikke alene beskriver teknik, men leverer et grundlag, der kan 
bruges til at træffe valg. For hvert væsentligt arkitekturvalg skal det fremgå, hvilket behov valget 
adresserer, hvilke alternativer der findes, og hvilke konsekvenser valget har. 

Arbejdet foreslås gennemført i fem trin. 

Først gennemgår arkitekterne eksisterende materiale fra OS2skole og OS2borgerPC. Formålet er at 
få et fælles billede af projekternes status, behov, funktionalitet, brugeroplevelse, tekniske valg og 
kendte afhængigheder. Gennemgangen skal også afdække, hvilke elementer der allerede kan 
genbruges, generaliseres eller indgå i OS2base. 

Dernæst udarbejder arkitekterne et fælles kapabilitetskort. Kortet skal vise de funktionelle og 
tekniske kapabiliteter, der er relevante på tværs af projekterne. Det skal skelne mellem 
kapabiliteter, der bør ligge i OS2base, kapabiliteter der bør håndteres som fælles principper, og 
kapabiliteter der bør blive i de enkelte projekter. 

Herefter formulerer arkitekterne forslag til fælles arkitekturprincipper. Principperne skal være 
konkrete nok til at kunne bruges i valg om teknologi, komponenter, integration, sikkerhed, 
dokumentation, drift og vedligehold. De skal samtidig være rummelige nok til, at de tre projekter 
kan bevare deres egen fremdrift. 

Som fjerde trin vurderer arkitekterne mulige fælles applikationer, komponenter og 
integrationsmønstre. Her skal der blandt andet tages stilling til, om kommunernes IT
medarbejdere kan få én fælles applikation eller brugergrænseflade til udrulning og administration 
på tværs af skoleområdet, borgerrettede pc’er og administrative pc’er. 

Til sidst samler arkitekterne de væsentligste valg i en beslutningslog. Hvis arkitekterne ikke kan nå 
til enighed, skal uenigheden beskrives åbent. Det skal fremgå, hvilke positioner der findes, hvilke 
argumenter der taler for hver position, og hvilke konsekvenser de forskellige valg har. 

## 6. Faglige temaer 
Beslutningsoplægget skal særligt belyse fem temaer. 

### 6.1 Funktionalitet som arkitekturhensyn 
Funktionaliteten i OS2skole og OS2borgerPC er allerede beskrevet i det eksisterende materiale. 
Arkitekterne skal derfor ikke definere funktionaliteten på ny. 

Arkitekterne skal i stedet vurdere, hvilke arkitektoniske hensyn den kendte funktionalitet giver 
anledning til. Det gælder især krav til fælles komponenter, adgangsstyring, administration, 
udrulning, integrationer, dokumentation, drift og support. 

Det skal tydeligt fremgå, hvor kendt funktionalitet peger på fælles løsninger, og hvor 
funktionaliteten er så projektspecifik, at den bør blive i det enkelte projekt. 

### 6.2 Brugeroplevelse som arkitekturhensyn 
Brugeroplevelsen for de forskellige målgrupper skal også behandles som et kendt hensyn. 
OS2skole retter sig mod skoleelever, lærere og skoleområdet. OS2borgerPC retter sig mod 
borgere, der bruger offentlige computere. OS2admPC retter sig mod medarbejdere og 
administrative arbejdspladser. 

Arkitekterne skal vurdere, hvordan en fælles arkitektur kan understøtte tryghed, genkendelighed 
og sammenhæng, uden at brugeroplevelsen ensrettes unødigt. Det skal være tydeligt, hvor fælles 
principper kan skabe værdi, og hvor forskellige målgrupper kræver forskellige løsninger. 

### 6.3 IT-medarbejdernes arbejdsgang 
Et centralt spørgsmål er, om kommunernes IT-medarbejdere kan få én fælles applikation eller 
brugergrænseflade til udrulning og administration af løsninger på tværs af OS2skole, OS2borgerPC 
og OS2admPC. 

Arkitekterne skal vurdere, hvilke opgaver der går igen på tværs, hvilke forskelle der skal håndteres, 
og hvilke forudsætninger der skal være opfyldt for at etablere en fælles administrationsløsning. 
Vurderingen skal omfatte både teknik, drift, support, dokumentation, sikkerhed og kommunernes 
praktiske implementering. 

### 6.4 Fælles arkitektur 
Arkitekterne skal pege på de arkitekturprincipper, komponenter og integrationsmønstre, der bør 
være fælles i OS2fri. 

Det skal blandt andet vurderes, hvordan OS2fri kan sikre modularitet, udskiftelighed, åbne 
standarder, open source, leverandøruafhængighed og robust drift.

Arkitekterne skal også beskrive, hvilke valg der bør træffes tidligt, fordi de får betydning for 
projekternes videre udvikling. 

### 6.5 OS2base 
Arkitekterne skal beskrive, hvad der bør indgå i OS2base fra starten, og hvad der ikke bør indgå. 
OS2base kan omfatte fælles arkitekturprincipper, komponenter, dokumentationsstandarder, 
sikkerhedsmodeller, governance, integrationsmønstre, administrationsværktøjer eller andre 
byggesten, som flere projekter har gavn af. 

Afgrænsningen er vigtig. OS2base skal fokusere på de dele, hvor der er et reelt fælles behov, og 
hvor fælles udvikling skaber større værdi end parallel udvikling i hvert enkelt projekt. 

### 7. Leverance 
De Fire Arkitekter skal levere ét samlet beslutningsoplæg til arkitekturworkshoppen. 
Leverancen skal bestå af et kort hovedoplæg, et beslutningsbilag og et visuelt arkitekturkort. 
Derudover skal arkitekterne kunne præsentere hovedpointerne på workshoppen. 

Hovedoplægget bør være på maksimalt 10 sider. Det skal kunne læses af både bestyrelse, 
projektfolk og tekniske deltagere. Oplægget skal beskrive den anbefalede fælles arkitektoniske 
retning, forslag til indhold i OS2base, væsentlige fælles komponenter eller applikationer, vigtige 
afgrænsninger og de beslutninger, der skal træffes på workshoppen. 

Beslutningsbilaget skal samle de væsentligste valg i en enkel form. For hvert valg skal det fremgå, 
hvad der skal besluttes, hvilke alternativer der findes, hvad arkitekterne anbefaler, hvilke 
konsekvenser anbefalingen har, og om der er enighed eller uenighed. 

Arkitekturkortet skal give et samlet visuelt billede af OS2fri, de tre projekter, OS2base, fælles 
komponenter, projektspecifikke komponenter, centrale integrationer og kommunernes IT
administration. 

Leverancen skal ikke være en tung teknisk rapport. Den skal være et beslutningsgrundlag. 

## 8. Håndtering af uenighed 
Arkitekterne skal forsøge at nå frem til fælles anbefalinger. Det er samtidig en vigtig del af 
opgaven, at uenighed ikke skjules. 

Hvis arkitekterne ikke kan blive enige om et væsentligt valg, skal uenigheden beskrives tydeligt. 
Det skal fremgå, hvad uenigheden handler om, hvilke faglige positioner der findes, hvilke 
argumenter der taler for hver position, og hvilke konsekvenser de forskellige valg har for genbrug, 
drift, sikkerhed, dokumentation og leverandøruafhængighed. 

Uenighed skal beskrives på en måde, så workshoppen og OS2’s bestyrelse kan træffe beslutning på 
et oplyst grundlag. 

## 9. Afgrænsning 
Arkitekterne skal ikke træffe beslutninger på vegne af de tre initiativer, OS2fri, OS2base eller OS2’s 
bestyrelse. 

Arkitekterne skal heller ikke udarbejde fuldt løsningsdesign for alle komponenter, definere 
projekternes samlede funktionalitet, redesigne brugeroplevelsen eller engagere sig i 
projektledelsen i OS2skole, OS2borgerPC eller OS2admPC. 

Arkitekternes opgave er at gøre de vigtigste fælles arkitekturvalg beslutningsklare. 

## 10. Succeskriterier 
Opgaven er løst tilfredsstillende, når leverancen giver et klart grundlag for workshoppen den 8. 
september 2026. 

Leverancen skal gøre det tydeligt, hvad der bør være fælles i OS2fri, hvad der bør ligge i OS2base, 
og hvad der fortsat bør ligge i de enkelte projekter. Den skal vise, hvordan eksisterende materiale 
fra især OS2skole og OS2borgerPC er indarbejdet. Den skal respektere de kendte funktionelle 
behov og brugeroplevelser, men omsætte dem til konkrete arkitekturhensyn. 

Leverancen skal samtidig gøre de væsentligste valg, konsekvenser og eventuelle uenigheder 
tydelige, så workshoppen og efterfølgende OS2’s bestyrelse kan træffe beslutninger på et oplyst 
grundlag.

## 11. Foreløbig proces 
Processen indledes med et møde mellem De Fire Arkitekter og programledelsen. På mødet 
afklares opgaven, metode, materiale, leverance, tidsplan og forventninger. 

Herefter arbejder arkitekterne med materialegennemgang, kapabilitetskort, arkitekturprincipper, 
vurdering af fælles komponenter og udarbejdelse af beslutningsoplæg. Der bør undervejs være 
kort dialog med OS2skole og OS2borgerPC, så antagelser kan afklares, og eksisterende materiale 
forstås korrekt. OS2admPC inddrages i det omfang, der findes tilstrækkeligt materiale eller 
relevante projektpersoner. 

Det færdige oplæg anvendes som grundlag for arkitekturworkshoppen den 8. september 2026. 

## 12. Forventning til arbejdsform 
De Fire Arkitekter forventes at arbejde åbent, pragmatisk og beslutningsorienteret. 

Arbejdet skal bygge på faglig vurdering og eksisterende materiale. Samtidig skal det være praktisk 
anvendeligt for et OS2-program, hvor governance, økonomi, implementering og drift er centrale 
forhold. 

Målet er ikke at beskrive den perfekte arkitektur i alle detaljer. Målet er at etablere det rigtige 
skelet fra starten, så OS2fri kan skabe fælles retning, genbrug og digital handlefrihed på tværs af 
OS2skole, OS2borgerPC og OS2admPC.