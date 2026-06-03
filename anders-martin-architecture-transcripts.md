---
type: Note
related_to:
  - "[[Decisions]]"
  - "[[3D Track Questions]]"
  - "[[Report Track Questions]]"
---

# Anders/Martin Architecture Transcripts

Collects two Anders/Martin 1-on-1 sessions pasted on 2026-06-02. Session 1 covers Core/Capture separation, device support, online/offline, and 3D editing. Session 2 covers the French Shape refactor, RGBD vs Roomplan, the Bocheck operational flow, manual-handling pricing, and report renewals. Fellow's topic anchor links are normalized to plain timestamps.

## Session 1 — Core/Capture, devices, offline, 3D editing

Meeting: Anders/Martin | AI Notes by Fellow

### AI Notes (Fellow)

#### Summary

Anders og Martin diskuterede teknisk arkitektur og produktstrategi for en capture/survey-app med fokus på at opdele systemet i Capture og Core-lag. Appen skal være let og kun håndtere dataindsamling og verificering, mens Core ejer beregninger og validering. Teamet besluttede minimumskrav for surveys (adresse + RGBD-scan) og enedes om at både iPhone og iPad skal understøttes til forskellige brugsscenarier. Der blev lagt vægt på offline-funktionalitet med mulighed for at starte og gennemføre surveys uden internet, mens synkronisering og rapportgenerering kræver online-forbindelse.

Derudover diskuterede de workflow-konfiguration med faste kernekomponenter (materialer, roomplan, RGBD) og fleksible elementer, hvor Field Deployment Engineers (FDE) kan iterere hurtigt. Målet er at eliminere kundevendt 3D-geometriredigering gennem bedre automatisering og potentielt via et service-lag. Systemarkitekturen skal have klar ansvarsfordeling hvor Core leverer struktureret data fra fysisk realitet, mens FDE ejer fortolkningslaget og rapportpræsentation.

#### Action items

- Anders Valentin: Undersøg det faktiske behov for area verification og udforsk forskellige løsningsmuligheder på rumniveau, herunder hvordan brugere kan verificere deres arbejde og material overrides. (1:30:20)

#### Decisions

- Appen skal ikke have kendskab til CM-relaterede ting - den skal kun kunne logge ind, vise sager, og definere komponenter til dataindsamling, mens Core ejer verificeringen. (10:20)
- Core skal omfatte den komplette definition af konflikttlaget (både input og output) samt beregning af minimum derived data, som strækker sig fra JSON til færdig 3D-model. (11:01)
- Minimumskrav for en survey er en adresse og en RGBD-scan - plansapp er ikke en tilfældig survey-app, men har definerede minimumskrav. (27:45)
- Besigtigelsesworkflowet skal have klart definerede retningslinjer frem for fuld frihed, da det påvirker materialevalidering og er for kompliceret at håndtere uden struktur. (34:27)
- Både iPhone og iPad skal understøttes i appen, da forskellige brugergrupper vil foretrække forskellige enheder til forskellige opgaver. (43:50)
- Teamet vil stræbe efter at eliminere 3D-geometriredigering som kundeansvar, så kunder ikke skal forholde sig til geometrien mere end nødvendigt. (48:02)
- 3D-editing skal kunne fungere på iPhone eller alternativt være plansapps ansvar at håndtere - kunder kan ikke forventes at have adgang til store skærme til dette formål. (53:38)
- Offline opstart af rapporter tillades, med synkronisering af ny konfiguration når brugeren går online for at uploade - staleness på op til 3 måneder accepteres. (59:05)
- On-site verifikation skal fungere offline, så brugere kan verificere materialer og attributter uden serveradgang, selvom dette ikke er fuld 3D-rekonstruktion. (1:01:19)

#### Topics

**App-arkitektur og Core/Capture-separation**

- Diskussion om, hvor meget appen skal vide om Cases-rapport versus at være en 'dum' capture-app. Trade-offet er mellem en simpel envejsapp til dataindsamling og en intelligent app, der styrer hele processen. (00:34)
- Verificering er en kernefunktion - appen behøver ikke at forstå hele rapporten, men skal kunne samle alle data og verificere, at brugeren har det nødvendige før afgang. (02:01)
- Anders understreger, at produktmæssigt er de nødt til at hjælpe brugerne med at verificere, om de har samlet det krævede data - det er det største painpoint for compliance-agtige rapporter. (02:32)
- Enighed om, at appen har minimumsansvar for capture og verificering, men beregningskorrekthed skal ikke ligge i appen - det er bare capture. (03:47)
- Appen skal være meget let - den definerer komponenter og capturer data, men bliver linket til Core for verificering. Core ejer verifikationen, ikke appen selv. (10:20)

**System-ansvarsfordeling og Core-funktionalitet**

- Diskussionen handler om hvorvidt FDE skal kontrollere interpretation og surface layer, specifikt omkring Report Derivation, Binding, Jurisdiction, Calc, og Core Canvas. (1:30:59)
- Arealberegninger (hvordan man går fra 80'er til 90'er til measured areas) skal laves i core, ikke i service. FDE skal kunne trække data, men ikke eje beregningslogikken. (1:31:22)
- Hvordan rapporten ser ud (f.eks. hvordan Kelvin ser ud) skal FDE eje, mens core leverer de underliggende beregninger. (1:31:51)
- FDE skal eje materialfortolkning og tillægsberegning (f.eks. thermal linear loss). FDE skal kunne vælge hvilke tabeller der skal anvendes for junctions mellem væg og slab. (1:32:34)
- Core skal tage fysisk realitet og lave det om til struktureret data. FDE skal derefter applicere fortolkningslaget baseret på kunde, regulering eller andre krav oven på det strukturerede dataset. (1:38:19)

**Rolledefinition og deployment-engineering**

- Opdeling i Capture og Core skaber tydelig afgrænsning af ansvar for forskellige teammedlemmer og opgaver, hvilket simplificerer fokus og constrainer, hvad deployment-ingeniører kan ændre. (12:06)
- Formålet er at forhindre deployment-ingeniører i at ændre i Core bare fordi det er nemmere, da det fører til kostbare løsninger - kun deliberate ændringer skal ske i Core. (12:30)
- Core/Capture kræver langsomme, deliberate løsninger (de betyder rigtig meget), mens deployment-engineering-laget kan iterere hurtigt og ændre dagligt uden konsekvenser. (13:13)
- Enighed om vigtigheden af stabilitet, når noget er i flow. Det handler om, hvem der ejer det - FDE har fleksibiliteten og skal selv bestemme, med tillid til deres kompetence. (18:42)
- Specificering skal separeres fra kode - der skal være to lag med forskellige iterationshastigheder, hvor Core skal være robust, mens specifikationslaget kan iterere hurtigt. (22:02)

**Workflow-konfiguration og produktfleksibilitet**

- Sondring mellem faste kerneområder (materialer, roomplan, RGBD) der ligger fixed i appen, og fleksible områder (Forms, Notes, Fields, Tasks) hvor der kan være fleksibilitet i flowrækkefølge. (22:13)
- Der vil være en rolle (ikke nødvendigvis en person) der sidder med kunden og itererer på domæneområdet og får det til at passe til kernemoduler - det er måden at deploye fremover. (23:44)
- FDE skal kunne kontrollere, hvad der spørges om, hvornår og i hvilken rækkefølge, samt om komponenter er fixed eller ej - med mulighed for at styre rækkefølgen på non-core elementer. (24:40)
- Lifecycle-points med tasks foretrækkes frem for fuld flow-engine - faste punkter som scan (ikke konfigurerbar) og fleksible steder som annotation flow hvor opgaver kan tilføjes. (26:08)
- Workflow-komponenter diskuteret: Polycam, Roomplan, attributter og annotations kan struktureres på forskellige måder, men rækkefølgen skal defineres klart for at undgå forvirring. (34:06)
- Hele huset skal scannes i ét RGBD-flow for at opnå bedst resultat med kontinuitet mellem billeder. Workflow-komponenter skal være konfigurerbare med faste kernekomponenter hvor Capture definerer hvilke komponenter der bruges, og rækkefølge/datakrav verificeres før næste trin. (35:48)

**Online og offline funktionalitet**

- Offline fungering er kritisk - brugere skal kunne starte og gennemføre capture uden internetforbindelse, med sync/derivation/reports krævet online. (56:36)
- Risiko for forældet JSON-konfiguration hvis offline for længe - løsning er 3-måneders cache af gamle states med backwards-compatible backend. (57:33)
- Upload-trin fungerer som sync-event hvor ny konfiguration downloades - acceptabelt at køre én ekstra rapport på gammel EPC-version da det ikke kan styres 100%. (59:05)
- Backend skal være backwards compatible i mindst en måned for at håndtere forældede reports, med mulig staleness-advarsel efter 3 måneder. (1:00:21)
- Målsætning er at kunne logge ind og lave en sag offline samt verificere arbejde uden server-adgang til materialer og B-faktorer. (1:01:24)
- RGBD vil spille en stor rolle i offline data collection, hvor al nødvendig data skal indsamles på device før arbejdet kan færdiggøres. Dette skaber udfordringer med at vide præcis hvornår LLM-processing kan ske. (1:08:05)

**3D-scanning, rekonstruktion og editing**

- Diskussion om, hvorvidt en vokset repræsentation af huset (uden fokus på huller i taget) ville være en bedre måde at håndtere materialeplacering end præcis 3D-rekonstruktion. (05:16)
- Alternative tilgange til materialespecifikation overvejes - at tage noter på point cloud-punkter og inferere hele overfladen, når den parametriske model kommer, ligesom at klikke på 3D-modellen. (06:09)
- Tagmaterialer er sjældent forskellige på samme tag, og kvist kan detekteres fra rumgeometri eller satellitbilleder, hvilket gør materiale- og U-værdi-verificering til en overordnet opgave. (07:06)
- Det endelige artefakt skal komme efter en roundtrip til serveren - især når de går til RGB-D, skal brugerrejsen være tilpasset til at have haft internet før færdiggørelse. (08:13)
- RGBD foretrækkes som minimumskrav frem for floorplan og roomplan, da en point cloud kan sendes til roomformer-algoritmer for at generere roomplan, hvilket giver større værdi og derivationspotentiale. (28:05)
- Ideal model er at 3D-editing sker on-device i stedet for web, og at opgaver der ikke kan laves på mobile enheder håndteres af plansapp. (47:29)

**3D-editing udfordringer og automatisering**

- Fjernelse af 3D-redigering fra kunder er meget lettere at sælge end usikkerheden ved at de skal hyre nogen der kan finde ud af det. (48:02)
- 3D-editing interface med manuel polygon-tegning er ikke ideelt hverken på iPad eller desktop - bedre interface kræver smartere løsninger som Tinder-lignende flows. (49:51)
- Filipinere kan ikke håndtere 3D-redigering professionelt, så fokus skal være på at reducere deres arbejdsbyrde og omkostninger gennem bedre automatisering. (52:05)
- SLA er mindst lige så vigtigt som pris - kunder i Frankrig forventer rapport samme aften da huse skal sælges næste dag, hvilket kræver færre mennesker i loopet. (52:44)
- Med offline-tilgangen uden on-device reconstruction opererer man ikke på det endelige objekt, hvilket skaber behov for 'staged changes' der skal reconciles ind i den endelige model senere. (1:10:46)
- Kerneproblemet er om 3D-model reconstruction skal kunne være færdig on-device uden internet. Dette er ikke et mål i sig selv, men påvirker kompleksiteten af in-field interface. (1:13:28)

**Materialeverificering og UX**

- On-site verifikation skal fungere offline uden at sende til server - brugere skal kunne tjekke om materialer og B-faktorer er placeret korrekt. (1:01:38)
- USDZ-visualisering skal bruges til verifikation, ikke pixel-perfect rekonstruktion - ligegyldigt om 13 eller 15 m², vigtigt er om korrekt materiale er tildelt. (1:02:24)
- Forskellige modeller (USDZ vs. fuld rekonstruktion) vil kræve forskellige logikker for at bestemme hvad der er dormer på web versus device. (1:03:52)
- Materiale-assignment skal muligvis flyttes til annotation-based struktur i stedet for USDZ-segment-valg, potentielt med 3D polygon annotations. (1:05:37)
- USDZ er fantastisk til at filtrere om arbejdet er lavet rigtigt (vinduer, døre, vægge fanget), men ikke til at verificere om væg er 2m eller 2.1m - visualisering til grov validering, ikke præcis måling. (1:06:57)
- Det er enormt svært at verificere ud fra noter alene. Brugere har brug for at kunne se deres material assignments visualiseret, ikke bare som tekstnotater. (1:20:11)

**Verifikationsudfordringer**

- Farvelægning efter materialer er det mest brugbare feature på web i lang tid, fordi det giver overblik over material assignments. (1:20:31)
- Brugere har svært ved at huske hvilke materialer der er hvor når de har været væk fra rummet, og arbejdshukommelsen er ikke stor nok til at stole på manuelt overblik. (1:25:23)
- I en fremtidig RGBD-only løsning vil der ikke være objekter brugere kan klikke på og se skifte farve, hvilket reducerer fideliteten i at verificere om overrides er sat korrekt. (1:26:00)
- Area verification skal være feasible on device, så brugere kan tjekke om deres arbejde er korrekt og om appen har registreret alle annotations korrekt før de forlader stedet. (1:29:00)
- Materialevalidering påvirkes direkte af workflow-rækkefølgen - usikkerhed om hvorvidt materialer skal sættes upfront i en shortlist eller efterfølgende på 3D-modellen. (34:38)

**iPhone og iPad understøttelse**

- Begge enheder skal understøttes - nogle brugergrupper vil foretrække iPhone mens andre foretrækker iPad, så det er nødvendigt at have begge muligheder. (43:50)
- Web bliver mere central da capture-appen reduceres til 'stupid capture' - færdiggørelse lægges et andet sted, muligvis som webview på iPad for større skærm. (44:24)
- iPad er foretrukken for professionelt arbejde med tastatur til tekst-editing og mulighed for at vise klienten arbejdet, mens iPhone er bedre til hurtig RGBD/Roomplan scanning. (49:09)
- Vitali rapporterede at han kører alt på iPad fordi det kører hurtigere og bedre end iPhone - muligvis grundet stærkere processor, selvom færre kameraer. (54:42)
- CoreRGBD leverer 1920x1080 opløsning - forskelle mellem iPad og iPhone kan ligge i fokuslinse kvalitet og lysindtagelse frem for resolution. (55:14)

**Andre emner og overvejelser**

- Enighed om at definere minimum survey-krav (adresse + indoor scanning) og styre rækkefølgen - men der er tvivl om, hvorvidt polycam skal tvinges først, og om roomplans skal ske rum-for-rum eller rum-rapport-rum-rapport. (33:00)
- RGBD-frames i bunden giver værdi som fallback hvis plansapp 3D-produktion fejler, og særligt vigtig tryghed for at alt er blevet samlet op korrekt. (37:07)
- Workflow skal være konfigurerbart med faste komponenter - Capture definerer hvilke komponenter der bruges, og rækkefølge/datakrav skal verificeres før næste trin. (42:25)
- Hvis escape hatch med filippinerne skal fungere, skal systemet kunne håndtere at 3D-modellen går ud og ind, og brugeren må potentielt vente på redigering før arbejdet kan fortsættes. (1:14:24)
- Der skal være mulighed for at notere data (f.eks. B-faktorer på 0,7 vs 0,15) sammen med billeder eller polygoner, så dataen er tilgængelig selvom online reconstruction endnu ikke er færdig. (1:09:28)

### Transcript

[0:00:00] Anders Valentin, Martin Collignon: Anders Valentin, Martin Collignon joined
[0:00:00] Anders Valentin: Anders Valentin started sharing your screen
[0:00:00] Martin Collignon (2): Så
[0:00:00] Martin Collignon (1): kører vi. Øhm... Yes. Øhm... Altså, der var...
[0:00:11] Anders Valentin: English?
[0:00:13] Martin Collignon (1): Oh, sure. I think Fellow is actually okay now in Danish, but...
[0:00:16] Anders Valentin: Okay. Så vi kan også gå til den på dansk. Det mere summarized skal jo sikkert gives til Marina også.
[0:00:21] Anders Valentin: Det er jo derfor, jeg gør det.
[0:00:23] Martin Collignon (1): Ja, det kan vi sagtens. Jeg tror dog, at den fungerer faktisk okay nu på dansk. Fra det jeg har set.
[0:00:28] Martin Collignon (1): Man skal bare huske lige at sige, at den skal være på dansk. Altså, når den er delt efterfølgende.
[0:00:34] Martin Collignon (1): Så, hvor meget skal appen vide omkring Cases rapport versus bare være en dum app i virkeligheden?
[0:00:44] Martin Collignon (1): Og trade-offet er i virkeligheden, at du har, altså man kunne sige, at den ene er, at appen er vildt bare en capture-ting, at den går kun den ene vej, og den får ikke noget den anden vej.
[0:00:54] Martin Collignon (1): Så du laver en case, den spytter, og så sker det, at du har nok en, som du bruger.
[0:01:00] Martin Collignon (1): Det er så dumt, som det overhovedet kan være Og sådan en anden vej er, at det er appen, der styrer det hele I virkeligheden At du har ikke noget web, den fungerer independently Du kan lave alle rapporten der Du kan, at den forstår, hvor langt den er i rapporten og så videre Det var for, så vidt jeg forstår, et ret stort problem for backend Hvor meget skal state forstås?
[0:01:25] Martin Collignon (1): Altså completeness af noget
[0:01:30] Martin Collignon (2): Og
[0:01:30] Martin Collignon (1): det er særligt svært med vores eksterne setup, fordi der er så meget fleksibilitet i hvad du sender og hvad du ikke sender.
[0:01:40] Martin Collignon (1): Altså du kan skippe det hele i virkeligheden. Selv orienteringen kan du skippe, så vidt jeg ved.
[0:01:45] Martin Collignon (1): Så det er ret svært nu som ting er at vide, har du alt det du skal lave til at lave en rapport eller ej.
[0:01:58] Martin Collignon (1): Så hvad er ansvaret for appen?
[0:02:01] Martin Collignon (1): Og for eksempel verification er jo en form for også at tjekke om den har det hele.
[0:02:09] Martin Collignon (1): Jeg er enig med Clode at det er midt imellem. På den måde er det en Capio Instrument.
[0:02:21] Martin Collignon (1): Den har en purpose som er at samle alt data.
[0:02:24] Martin Collignon (1): Og den skal kunne samle alle data, men den behøver ikke at forstå alt rapport.
[0:02:28] Martin Collignon (1): Altså den behøver ikke gå hele vejen til at færdiggøre rapporten.
[0:02:32] Anders Valentin: Jeg synes for mig, at det her er ekstremt meget et engineering spørgsmål.
[0:02:38] Anders Valentin: Der er ikke nogen tvivl om, at produktmæssigt, UX-mæssigt for mig, så er vi nødt til at hjælpe dem med at verify, om de har lavet de opgaver, de skulle.
[0:02:46] Anders Valentin: Jeg kan ikke se, at vi kan lykkes uden at give dem hjælp til, har jeg samlet det data, jeg skulle før jeg går.
[0:02:54] Anders Valentin: Fordi det er ligesom, for de her compliance-agtige rapporter, der er det painpointet, faktisk.
[0:03:03] Anders Valentin: Det er faktisk det største painpoint, der er.
[0:03:06] Anders Valentin: Og jeg tror ikke, vores wedge lykkes, hvis vi ikke hjælper dem med det.
[0:03:10] Anders Valentin: Jeg tror også, det er derfor, alt er sådan forms i alle andre apps.
[0:03:14] Anders Valentin: Fordi du har en meget tydelig state om, du har samlet det, du skulle, i virkeligheden.
[0:03:20] Anders Valentin: Og så er jeg helt enig, det er den smalleste version af det, Jeg vil hellere have et interface, der hedder web, hvor du løser alle de der lolleproblemer og skriver tekster og sådan noget.
[0:03:34] Anders Valentin: Det synes jeg egentlig som udgangspunkt skal ligge i web eller iPad, men det kan vi komme tilbage til.
[0:03:41] Anders Valentin: Men jeg ser ikke, at appen har mere ansvar end det, som er minimumsansvaret.
[0:03:47] Anders Valentin: Dermed vil jeg heller ikke sige, at beregningskorrekthed skal heller ikke ligge på appen.
[0:03:54] Anders Valentin: Det er bare Capture.
[0:03:56] Martin Collignon (2): Og så
[0:03:57] Martin Collignon (1): er der, hvis du deler op i Capture og Core, hvor Core er for eksempel geometrien, for mig var der noget, altså jeg synes, at du havde ret, at du tog to forskellige ting.
[0:04:07] Martin Collignon (1): Men på en måde, du kan godt have en beregningskerne til at forsøge geometri, og Arda
[0:04:11] Martin Collignon (2): Core,
[0:04:12] Martin Collignon (1): som er på on-device,
[0:04:14] Martin Collignon (1): eller som bliver vist på device faktisk, så er det en lille asterisk her, som er at du, Jeg er i princippet ligeglad.
[0:04:22] Martin Collignon (1): Jeg forstår, at det er trade-offs.
[0:04:24] Martin Collignon (1): Men om vi siger, at du sender din geometri til serveren, og så tilbage igen, og så visualiserer du så tilbage i appen.
[0:04:30] Martin Collignon (1): Eller at du har alt optagelse, som fungerer rigtig meget lokalt, og så alt derefter, for eksempel WebView, når du har samlet alt.
[0:04:40] Martin Collignon (1): Det er for mig ikke vigtigt.
[0:04:42] Martin Collignon (1): Det er, hvis det er et spørgsmål, som du siger, om det er engineering, om det er hastighed, om det er et eller andet.
[0:04:47] Martin Collignon (1): Vi kan selvfølgelig tænke over,
[0:04:50] Martin Collignon (2): hvad
[0:04:50] Martin Collignon (1): det betyder i engineering.
[0:04:55] Martin Collignon (1): Vi kan godt deltage i en omkostning i spørgsmål, men det er mere, hvis det åbner døren for nogle nye ting, så er det okay for mig, at vi deler det op på den måde.
[0:05:03] Martin Collignon (1): Et andet idé, jeg havde i går, fordi jeg forsøgte at constrain mig, hvad kunne man gøre?
[0:05:09] Martin Collignon (1): Og kvist er det altid et eksempel, vi bruger ikke, for at sætte marshal for, vi
[0:05:12] Martin Collignon (2): vil se,
[0:05:13] Martin Collignon (1): at det arbejder.
[0:05:16] Martin Collignon (1): Hvad hvis Hvis du havde en vokset repræsentation af huset, hvor du satte materialer på, så du ikke fokuserede på, om der var huller i taget, ville det være distraktivt.
[0:05:38] Martin Collignon (1): Så er det en bedre repræsentation, eller skal materiale hænge sammen med en præcis 3D reconstruction?
[0:05:47] Martin Collignon (1): Og det er ikke noget, jeg har et svar på, men jeg var...
[0:05:50] Anders Valentin: Jeg har faktisk gjort mig noget af de samme tanker. I form af, at... Altså...
[0:05:59] Anders Valentin: Hvis man tager den her konklusion til enden, og man gerne vil supportere iPhone til EPC... Så...
[0:06:09] Anders Valentin: Så kunne du lige så godt tage en note og sige, det her materiale er det her, eller...
[0:06:16] Anders Valentin: på et eller andet en point cloud punkt og så inforere hele overfladen af det, når den parametriske model kommer.
[0:06:25] Anders Valentin: Det vil man lige så godt kunne gøre, som du vil kunne sætte klik på 3D modellen og sige, at den her er det her.
[0:06:32] Anders Valentin: Det kan sagtens være en fremgangsmåde. Jeg tror, at de tænker meget i overflader.
[0:06:40] Anders Valentin: Jeg tror, I ser problemvarene her, fordi Den måde de tænker det på er arealer, og hvis det ikke er arealer, de har svært med de der åbne noter der, ikke?
[0:06:51] Anders Valentin: De vil gerne have masser til objektet.
[0:06:56] Martin Collignon (2): Jeg
[0:06:56] Martin Collignon (1): er
[0:06:56] Martin Collignon (2): enig,
[0:06:57] Martin Collignon (1): men det er sådan, hvis du, altså der er tænkt på det i forhold til også de der sealing reconstruction jeg har lavet, altså det er ikke fordi materialet er meget anderledes.
[0:07:06] Martin Collignon (1): Altså det er meget, meget sjældent, hvor du har forskellige tagmaterialer.
[0:07:10] Anders Valentin: Ja, de har samme isolering alt sammen. Men kvistmaterialerne er typisk tyndere jo.
[0:07:16] Martin Collignon (2): Jo,
[0:07:17] Martin Collignon (1): men kvist er så lidt... Enten er der kvist eller er det ikke kvist.
[0:07:20] Martin Collignon (1): Altså sagt på den måde, at hvis du dikterer kvisten, og det tror jeg faktisk godt vi kan alene fra rumgeometrien faktisk.
[0:07:27] Anders Valentin: Ja, det tror jeg også.
[0:07:28] Martin Collignon (1): Uden at vide det globale ting, så vil du godt kunne, eller faktisk på forhånd med satellitbilleder, så vil du godt kunne spørge om kvistmaterialet på rumniveau, baseret på det.
[0:07:42] Martin Collignon (1): På samme måde som vi spørger dem til Windows i
[0:07:46] Martin Collignon (2): virkeligheden.
[0:07:48] Martin Collignon (1): Og i så fald er denne verificeringsopgave en materiale og b-faktor overordnet opgave, hvor du kan sagtens lege med en USDZ, som er meget rough, og som du gør meget tydeligt for kunden, ikke er den endelige model, men en forsimplet repræsentation af det, for eksempel.
[0:08:10] Martin Collignon (1): Perfekt repræsentation, er det noget, jeg først skal sige?
[0:08:13] Anders Valentin: Som udgangspunkt rent arkitekturmæssigt, så er vi interesseret i, at det endelige artefakt kommer efter en roundtrip.
[0:08:23] Anders Valentin: Det er vi interesseret i, fordi når vi går over i RGB D-land, så skal vi have et interface, der er en brugerrejse, som generelt set er tilvand til, at du har haft internet, før du færdiggør tingene.
[0:08:36] Martin Collignon (1): Yes. Ja,
[0:08:37] Martin Collignon (2): og jeg
[0:08:38] Martin Collignon (1): mener faktisk med AGBD, hvis vi skal gøre det for træningsrobotters skyld, så mener jeg nok, at vi skal have delayet nogle ting til de får wifi.
[0:08:47] Martin Collignon (1): Så du ikke får svar på AGBD med det samme.
[0:08:54] Anders Valentin: Det er bare en non-solution, og derfor er det dumt at constrain sig til deviceet. Det er en fejl.
[0:09:02] Anders Valentin: Så interfacet skal være på en måde, hvor det er tilvandet.
[0:09:06] Anders Valentin: Og nu ved jeg godt, at vi springer lidt i det, men jeg har også tænkt...
[0:09:12] Anders Valentin: Måske handler det også om, hvad du anbefaler, hvornår til hvem, i form af device.
[0:09:20] Anders Valentin: Fordi jeg kan godt se...
[0:09:22] Anders Valentin: Grunden til, at jeg har været meget restrained med at sige, at vi skal gå til iPad, for eksempel.
[0:09:29] Anders Valentin: Det har været, at jeg kan forestille mig, at det kan blive lidt svært i Frankrig, måske.
[0:09:35] Anders Valentin: Prismæssigt, fordi den er lige det dyre.
[0:09:39] Anders Valentin: Men det andet er, at jeg havde svært ved at se det for ejendomsmalere og for alle mulige andre, som ikke er sådan professionelle professionelle.
[0:09:49] Anders Valentin: Men jeg ser faktisk ikke en særlig stor udfordring for fx Butchek, at alle har iPad der.
[0:09:54] Anders Valentin: Det tror jeg, at de vil have det super fint med. Og heller ikke er økonomisk et problem.
[0:10:01] Martin Collignon (1): Nej, og det kan vi vende tilbage til iPhone og iPad, for det tror jeg, det er...
[0:10:05] Anders Valentin: Den er væsentlig i forhold til... Det hele hænger sammen, ikke?
[0:10:10] Anders Valentin: Altså, den er væsentlig i forhold til formfaktor for, hvad for en interface kan du realistisk bygge, ikke?
[0:10:15] Martin Collignon (2): Ja.
[0:10:17] Martin Collignon (2): Men
[0:10:18] Martin Collignon (1): for mig var det sådan, så jeg er enig i forhold til appen.
[0:10:20] Martin Collignon (1): Appen skal ikke vide noget om det CM-mæssigt, så den skal kunne logge ind, og den skulle vise sager til dig, men den skal ikke
[0:10:27] Martin Collignon (2): tænke
[0:10:27] Martin Collignon (1): for mig, om den har... Altså, den skal ikke... Det er ikke i appen, det skal defineres.
[0:10:32] Martin Collignon (1): Altså de fleste ting skal defineres enten i Core eller af Board Deployed Engineer.
[0:10:37] Martin Collignon (1): Det vil sige, appen skal vide meget, meget let.
[0:10:39] Martin Collignon (1): Den skal definere nogle komponenter og capture ting rigtig fint.
[0:10:44] Martin Collignon (1): Og så skal den blive linket til Core i forhold til at verificere nogle ting.
[0:10:50] Martin Collignon (1): Fordi at verificere, at man har alle data, er vigtigt i virkeligheden.
[0:10:53] Martin Collignon (2): Men
[0:10:54] Martin Collignon (1): det er
[0:10:54] Martin Collignon (2): Core,
[0:10:54] Martin Collignon (1): der ejer den verificering. Når du siger
[0:10:58] Martin Collignon (2): core, er det så kalor? Ja,
[0:11:01] Martin Collignon (1): det kan du sige. Men det er kalor minus nogle ting, som der i dag er i kalor.
[0:11:06] Martin Collignon (1): Core i fremtiden bør være i virkeligheden en hel definition af konfliktlaget både input og output plus beregning af det vi ser som en minimums derived data.
[0:11:23] Martin Collignon (1): Og det kan være at tage den fra JSON til en færdig 3D-model.
[0:11:28] Martin Collignon (1): Det kan være en RGBD til en splat, for eksempel.
[0:11:33] Martin Collignon (1): Altså det er de ting, som vi anser som det minimum leverance, vi har i API'en, i bund og grund.
[0:11:39] Martin Collignon (1): Og der er rigeligt, hvor jeg siger det sådan.
[0:11:43] Martin Collignon (1): Og det er ikke ens betydende med, at det ikke har noget at gøre med... Altså...
[0:11:50] Martin Collignon (1): Ja, vi kan vende tilbage til, hvad det ikke er så, også i fremtiden.
[0:11:53] Anders Valentin: Kan du udtale dig, hvorfor du har brug for den sondring der?
[0:11:56] Anders Valentin: Altså, hvad er det, der ligger til baggrund for det?
[0:12:00] Anders Valentin: Ikke fordi jeg nødvendigvis er uenig, jeg skal bare forstå, hvor det kommer fra.
[0:12:02] Martin Collignon (2): Ja,
[0:12:03] Martin Collignon (1): det er helt cool. Her er det meget vigtigt at være eksplicit.
[0:12:06] Martin Collignon (1): Det er, at du adskiller, så du gater i virkeligheden, hvad ansvaret er for forskellige teammedlemmer, eller for forskellige opgaver i virkeligheden.
[0:12:17] Martin Collignon (2): Således
[0:12:18] Martin Collignon (1): at man fokuserer, simplificerer, hvad opgaven er, og ting har en kasse, også for os i virkeligheden.
[0:12:27] Martin Collignon (1): Og har en måde at snakke om tingene på, om noget er et sted eller ej.
[0:12:30] Martin Collignon (1): Og det er også for at constrain, i min optik, for deployeringeniører en rolle, eller et eller andet, til at sige, hvad kan du, hvad kan du ikke, hvornår skal du tage fat i en anden person, hvornår kræver det en større diskussion.
[0:12:42] Martin Collignon (1): Således at du ikke kan forwarde en deployeringeniør, der ændrer ved ting i core, bare fordi det er nemmere, fordi så ender du i rigtig mange kostnomsløsninger i virkeligheden.
[0:12:53] Martin Collignon (2): Så
[0:12:53] Martin Collignon (1): det er også at sige, hvad er vigtigt og hvad er ikke vigtigt. Hvad skal vi have styr på?
[0:13:00] Martin Collignon (1): Hvad skal der være rigtig godt styr på? Og ting skal bare virke hele tiden. Hvad skal ikke?
[0:13:05] Martin Collignon (1): Hvad er der kontraktfuldt forpligtende?
[0:13:06] Martin Collignon (1): Og jeg synes, at totalt på en god måde, det er Core Capture har det der, hvor vi skal være langsomme, deliberate løsninger, for det betyder rigtig meget.
[0:13:21] Martin Collignon (1): Fordeploy Engineer er der, hvor du virkelig kan fuck rundt.
[0:13:25] Martin Collignon (1): Altså du kan wipe code, du kan ændre tingene på daglig basis, der sker ikke noget ved det.
[0:13:28] Martin Collignon (1): Altså det er mænd på den måde. Og hvis
[0:13:31] Martin Collignon (2): du
[0:13:31] Martin Collignon (1): har noget i Fordeploy Engineer, lad os sige, at du kan sagtens have en RGBD til Splat, der kører til at starte med IFDE, fordi det er bare en prototype til en kunde, som så graduerer og bliver til en del af Core Offeringen i fremtiden.
[0:13:50] Anders Valentin: Ja, altså jeg kan godt se, hvad du mener. Jeg tror, en ting er ligesom vibecoded-hed.
[0:14:00] Anders Valentin: Jeg synes, vi skal stoppe med at bruge det udtryk.
[0:14:03] Anders Valentin: Men noget, der er mere prototypeorienteret eller bygget med måske mere et faste forsøg.
[0:14:16] Anders Valentin: Den iterationshastighed skal der jo ligesom kunne være på begge dele.
[0:14:20] Anders Valentin: Jeg tror i praksis, altså i den praksis vi går ind i, så stopper det der iterationslag med at være hurtigt, ret hurtigt.
[0:14:29] Anders Valentin: Fordi du har 80 servere, som bliver trænet i at bruge tingene på en bestemt måde, som skal trænes igen, som skal vide hvor filterne er på den ene side, og på den anden side har du nogle downstream dependencies, som begynder at forvente at splatted er der.
[0:14:44] Anders Valentin: Begynder at forvente, at der er case. Det skal virke reliably.
[0:14:47] Anders Valentin: Altså jeg tror, at den der drøm om Greenfield hele vejen rundt.
[0:14:52] Anders Valentin: Jeg tror, at den er meget midlertidig. Kun når du laver nye ting. Når du får testet dem af.
[0:15:01] Anders Valentin: Når du får svaret om det virker eller ej. Så dør iterationshastigheden ret hurtigt.
[0:15:07] Anders Valentin: Og reliability og predictability og alle de her ting bliver ligesom.
[0:15:12] Anders Valentin: Meget hurtigt en del af gamet, fordi det er operationelt.
[0:15:15] Martin Collignon (2): To
[0:15:15] Martin Collignon (1): sekunder, jeg skal lige...
[0:15:17] Martin Collignon: Martin Collignon left
[0:15:17] Martin Collignon: Martin Collignon stopped sharing their screen
[0:16:52] Martin Collignon: Martin Collignon joined
[0:16:52] Martin Collignon (2): Jeg har jo lagt min skærm.
[0:16:55] Anders Valentin: Ej, vil jeg trykke på den, da du skræder den op?
[0:17:15] Martin Collignon (2): Nå, men jeg hørte, hvad du sagde.
[0:17:18] Martin Collignon (2): Du var helt sort. Jeg vil bare gerne se dig. Så ved jeg ikke, hvad der skete med mit...
[0:17:22] Anders Valentin: Jeg ser også fin ud med mit lille halsklod.
[0:17:24] Anders Valentin: Jeg ved ikke, hvilken del af det her, der betyder noget for dig. Det betyder meget for mig.
[0:17:33] Anders Valentin: Især, at vi hurtigt kan iterere, når vi skal lave de her...
[0:17:39] Anders Valentin: Dels på den ene side, fordi vi skal opdage, hvad der er vigtigt. Hvad der sælger på downstream-siden.
[0:17:46] Anders Valentin: Og på FDE-siden i forhold til Ops, der er der bare rigtig meget, vi ligesom skal lære af serveresne om, hvordan de gerne vil have tingene.
[0:17:53] Anders Valentin: Og det iterationslag bliver nødt til at være hurtigt.
[0:17:56] Anders Valentin: Jeg er ikke i tvivl om, om det så betyder, at det er speccing og prototype, eller det betyder, at vores arkitektur skal være sådan.
[0:18:08] Anders Valentin: Ej, jeg ved det ikke. Jeg ved det honestly ikke.
[0:18:10] Anders Valentin: Men jeg kan bare ikke se, Jeg sidder også med problemer her med det her internal ops, hvor vi har ikke nogen API til ting.
[0:18:17] Anders Valentin: Masser af ting, som jeg gerne vil have en API til, og det gør det bare mega besværligt at bygge en masse ting, som det ikke burde være.
[0:18:24] Anders Valentin: Så jeg forstår problemstillingen. Jeg er i tvivl om, hvor langt det er.
[0:18:29] Martin Collignon (2): Vi er enige om vigtigheden af stabilitet, når noget er en flow.
[0:18:42] Martin Collignon (2): For mig handler det mere om, hvem ejer det.
[0:18:47] Martin Collignon (2): Det er en FDI, der har den fleksibilitet til at gøre det, og det er FDI'en, der selv skal bestemme.
[0:18:57] Martin Collignon (2): Jeg synes, det er en form for latent bekymring om en FDE, og det er måske, fordi det er tegnet til mig eller dig, at vi ikke kan finde ud af at gøre tingene ordentligt.
[0:19:08] Martin Collignon (2): Altså, at vi kommer basically til at fuck rundt og ændre på en JSON eller en YAML-fil hver dag.
[0:19:15] Martin Collignon (2): Altså, jeg tror, vi skal antage, at personen er dygtig, og personen ved, hvad de gør, og personen forstår de constraints, de opererer med, og ikke ændrer på det.
[0:19:22] Martin Collignon (2): Altså, at deres kunde som de er ansat til at servicere, ikke har lyst til at de ændrer ved ting, de trækker om dagen.
[0:19:31] Martin Collignon (2): Altså, jeg synes, det er kernantagelsen. Jeg synes, at der er et eller andet...
[0:19:35] Martin Collignon (2): Det er ikke, fordi noget er nemt at ændre, at den ændres hele tiden. Det er to forskellige ting.
[0:19:46] Anders Valentin: I kommunikation omkring det her. Jeg ser det på samme måde, som du gør. Altså, vi er meget enige her.
[0:19:51] Anders Valentin: Jeg tror bare, der er noget i kommunikationen omkring, hvad det er, du søger.
[0:19:55] Anders Valentin: Det er sådan lidt irriterende, men jeg tror, de frygter, at nu bliver det vibecodeland, og Martin tager styringen, ikke?
[0:20:02] Anders Valentin: Det er det, der sidder i deres hoveder, hvilket er uretfærdigt, men det er det, der sidder i deres hoveder.
[0:20:06] Martin Collignon (2): Ja, ja, og jeg synes faktisk, det er omvendt.
[0:20:08] Martin Collignon (2): Det at sige i stedet for, jeg kan PR alt, hvis jeg skulle have den rolle, så er det, jeg kan PR en Jason, som jeg kan ændre markefølge på.
[0:20:18] Martin Collignon (2): Og jeg kan ikke lave nye komponenter i appen. Jeg kan ikke lave nye API'er i backend.
[0:20:25] Martin Collignon (2): Jeg er constrained til at vælge mellem de ting, som de giver mig.
[0:20:28] Martin Collignon (2): Og de kan selv styre den validering, der er brug for.
[0:20:31] Martin Collignon (2): Jeg synes bare, at det er forkert at sige, jamen det skal ikke den JSON og den...
[0:20:45] Martin Collignon (2): Martin er ikke i stand til at definere, hvad energimærket er.
[0:20:50] Martin Collignon (2): FD er ikke i stand til at definere, hvad EPC'et er.
[0:20:54] Martin Collignon (2): Hvis jeg sidder med det ansvar, at jeg skal definere, hvad EPC'et er til Frankrig, fordi det meget skal til Frankrig-agtigt, så sidder jeg med ansvar for at gøre det, og ansvar for, at det virker, og jeg bliver målt på, om det virker, om kunden køber det i sidste ende.
[0:21:08] Martin Collignon (2): Hvis det ikke gør det, så er det ikke, fordi jeg ikke er god til mit job. Og så er det en modpoint.
[0:21:14] Martin Collignon (2): Vi skal ikke have folk, der er dårlige til deres job. Så det er lidt det, jeg tænker.
[0:21:28] Martin Collignon (2): Fordi lige nu, vi snakker overflade over det hele. Alt er muligt. Alt.
[0:21:33] Martin Collignon (2): Når vi snakker om budget, alt kan lade sig gøre.
[0:21:36] Martin Collignon (2): Og det betyder også, at vores udviklere skal tænke fra fra input til product management til hele vejen igennem på tværs af alle platformer samtidig.
[0:21:46] Martin Collignon (2): Så har vi pludselig fem mennesker i et møde der skal drøfte det.
[0:21:49] Martin Collignon (2): Hvor problemet skal deles op i flere ting i min butik.
[0:21:56] Martin Collignon (2): Og jeg synes det skal være tydeligere end det det er i det dokument, men det er lidt det der formål i hvert fald.
[0:22:02] Anders Valentin: Jeg tænker en god sondring er så, at vi vil gerne have, at det er nogle moduler, som kan tilgås fra det her specificeringslag.
[0:22:13] Anders Valentin: At det at specificere, hvilken rækkefølge flere ting kommer i, og hvad der er i dem, det bliver nødt til at blive separeret fra koden, så der er to lag, man kan iterere på.
[0:22:26] Anders Valentin: Fordi kårlader skal være robust.
[0:22:28] Anders Valentin: Og hvis vi konflaterer de to, så får vi to forskellige iterationshastigheder og sværhedsgrader at redigere.
[0:22:39] Anders Valentin: Jeg synes også, vi bliver nødt til at sige tydeligt, hvis vi gerne vil have, at det er den måde, vi ruller ting ud på.
[0:22:48] Anders Valentin: At det er efter ære. Der er en person. Det kunne være Niels. Det kunne være Mark. Det kunne være mig.
[0:22:55] Anders Valentin: Det kunne være dig. Det kunne være en ny ansættelse, vi har.
[0:22:58] Anders Valentin: Der er en, der sidder med kunden og itererer på domæneområdet, som får det til at passe til vores kernemodul.
[0:23:05] Anders Valentin: Det er deres rolle, og den måde vil vi gerne deploye det på fremover.
[0:23:15] Martin Collignon (2): Men jeg vil sige, det kan også være en rolle. Det behøver ikke være en person.
[0:23:19] Martin Collignon (2): Det kan være en rolle, som en person tager.
[0:23:27] Anders Valentin: Men den her rolle kommer til at eksistere for vores klienter og marknader.
[0:23:32] Martin Collignon (2): Også for at gøre det tydeligt, det betyder også for mig, fordi vi skal have fallbacks og vi skal lade folk gøre deres arbejde, som de har lyst til.
[0:23:44] Martin Collignon (2): Fallbacks er også, at nogle gange skal vi lave fejl.
[0:23:50] Martin Collignon (2): Hvad appen skal tjekke for, om det er blevet optaget eller ej, skal defineres et andet sted.
[0:23:55] Martin Collignon (2): Det vil sige, at det ikke er appen, der skal definere, om en rapport er blevet færdig eller ej i appen.
[0:24:01] Martin Collignon (2): Nej, det er konfigurationen, der skal definere det. Lige præcis.
[0:24:08] Anders Valentin: Og det er jo også den logistik, vi har lagt for appen, at det er opgaver.
[0:24:14] Anders Valentin: Og lige nu supporterer vi ikke, at vores state-håndtering i appen er ikke eksisterende.
[0:24:24] Martin Collignon (2): Og så kan FD kontrollere, hvad der bliver spurgt, hvornår i hvilken rækkefølge, fixed components or not.
[0:24:40] Anders Valentin: Jeg tror ikke, der er nogen vej rundt om, at man også er nødt til at kunne styre rækkefølgen på nogle af tingene.
[0:24:46] Anders Valentin: Jeg ville lave en sondring mellem nogle bestemte kerneområder, som er teknisk svære.
[0:24:54] Anders Valentin: Så det er sådan noget som materialer, hvornår de bliver sat, roomplan, rgbd.
[0:24:59] Anders Valentin: Alle de her ting ligger utroligt fixed i appen. De er ikke noget, der rykker rundt.
[0:25:05] Anders Valentin: Og så har du sådan noget som Forms, Notes, Fields, Tasks, hvor der skal fyldes noget data ind i et langt ark på en eller anden måde.
[0:25:14] Anders Valentin: Og der kan godt være fleksibilitet for, hvor de komponenter ligger henne flowmæssigt.
[0:25:18] Anders Valentin: Det er også sådan noget, som forestiller dig en annotation mode. Er det elder, der kommer først?
[0:25:23] Anders Valentin: Er det tilstand, der kommer først?
[0:25:26] Anders Valentin: De der ting, det tror jeg, at en bliver nødt til at kunne styre rækkefilm på, fordi super meget af Den medspecifisering, der sker ude i marken, er rækkefølgen.
[0:25:37] Anders Valentin: Jeg tror ikke, det kan undgås helt.
[0:25:41] Martin Collignon (2): Bare så jeg forstår, hvad du mener her, det er, at når du siger, det skal defineres, ligger det fast, eller ligger det noget, som FDE'en styrer?
[0:25:50] Anders Valentin: FDE'en styrer rækkefølgen på ting, der er non-core sagt på en anden måde.
[0:25:55] Anders Valentin: Så der bliver nødt til at være begge dele, lad mig sige det sådan.
[0:25:59] Anders Valentin: Jeg tror ikke på fuld fleksibilitet.
[0:26:00] Anders Valentin: Det er enormt kompliceret at lave en fuld flow, fuldstændig konfigurerbar flow engine.
[0:26:08] Anders Valentin: Det jeg tror mere på, det er at du har sådan nogle life cycle points, for eksempel.
[0:26:14] Anders Valentin: Og så er der tasks, der ligger på det lag, som har en rækkefølge.
[0:26:19] Anders Valentin: Men der vil være, for eksempel scan.
[0:26:22] Anders Valentin: Du skal ikke rykke rundt på, om det er scanning per rum, eller scanning per attache, eller scanning per attache.
[0:26:29] Anders Valentin: eller per bygning. Det skal være fuldstændig fikst. Det ligger på det samme sted.
[0:26:34] Anders Valentin: Og det betyder også, at scanreviewet kommer lige der. Og det kan du heller ikke ændre på.
[0:26:39] Anders Valentin: Du kan måske ændre på, hvilke notetyper der kan laves, men du kan ikke ændre på, at der er et scanreview for eksempel.
[0:26:47] Anders Valentin: Og så har du omvendt sådan noget som et annotation flow, hvor du har nogle ting, du skal annotere, eller nogle opgaver, der skal følges noget data ud på det her rum, eller på den her etage, eller på den her bygning, og det ligger ligesom i sin egen kategori, hvor der er fleksibilitet.
[0:27:07] Anders Valentin: Så du har sådan nogle faste punkter, som ikke kan rykkes ved, og så har du nogle steder, hvor du så kan tilføre en opgave.
[0:27:34] Martin Collignon (2): Det jeg skrev, var også en form for et flaw. Det vil sige, at vi er ikke en random survey app.
[0:27:45] Martin Collignon (2): Det vil sige, at vi har en minimum krav til, hvad en survey skal være.
[0:27:50] Martin Collignon (2): Og det flaw skal vi have defineret, og det vil sige, skal der være en 3D-scan, skal vi sige at det faktisk snarest muligt er RGBD, der er mindst kravet.
[0:28:05] Martin Collignon (2): Så hvis du skal bruge plan, så skal du altid bruge en adresse og en RGBD-scan. Det er det minimum.
[0:28:11] Martin Collignon (2): Du kan ikke lave en plansurvey uden det. Indtid har det været floorplan og roomplan.
[0:28:21] Martin Collignon (2): Hvad er det bedste, jeg synes mere og mere om RGBD, hvis vi kan få det til at hænge sammen?
[0:28:25] Martin Collignon (2): Igen, der er en del research, men fordi du kan i teorien altid en roomplan ovenpå RGBD.
[0:28:36] Martin Collignon (2): Hvis du har en point cloud, så kan du sende den point cloud videre til roomformer eller de her algoritmer, og så kommer de til at lave en roomplan for dig.
[0:28:46] Martin Collignon (2): Jeg siger ikke, at det er der, hvor vi er, men det er nemmere at gøre det på den måde, end at have en roomplan og forsøge at lave en point cloud
[0:28:53] Anders Valentin: tilbage. Ja, du har lidt tab af
[0:28:56] Martin Collignon (2): datakvalitet. Det er i hvert fald teorien.
[0:29:16] Martin Collignon (2): Du har større værdi og større derivation potentiale, hvis du har en RGPD, end med Roomplan.
[0:29:24] Martin Collignon (2): Noget, vi også kan stille os selv. I forhold til floor.
[0:29:34] Martin Collignon (2): Altså i dag, den tykkelse af vægge er normdefineret af nogle materiale properties, så nogle attributes på nogle vægge, eller på elementer.
[0:29:44] Martin Collignon (2): Og de kan være rigtige eller forkerte, fordi de er menneskelige drevet, vi har ikke nogen krav til at scanne ud fra, som vil være måden at tjekke det på, hvis vi anser, at R-case er god nok til at måle op.
[0:29:58] Martin Collignon (2): Altså, er ydervægts tykkelse en del af flår i virkeligheden?
[0:30:10] Martin Collignon (2): Skal elektrikere måle tykkelsebevægninger?
[0:30:14] Anders Valentin: Nej, det har de jo i reelt ikke noget brug for.
[0:30:16] Martin Collignon (2): Og hvad betyder det for os? Altså, er vi okay med det?
[0:30:28] Anders Valentin: Hvorfor er det her en udfordring?
[0:30:33] Martin Collignon (2): Det kan det være, hvis du tror, at det med i forhold til vores isolering er vigtigt.
[0:30:46] Anders Valentin: Nå, altså du tænker jo som minimums-artefakt har vi brug for tykkelsen af væggen til at lave, yes.
[0:30:53] Martin Collignon (2): Det er det jeg mener med flaw i virkeligheden. Hvad skal en plansurvey være?
[0:30:57] Anders Valentin: Okay, så det er ikke hvidt og lidt etage eller gulv, det er... Minimums-arbejdsurvey, ja.
[0:31:03] Martin Collignon (2): ja. Okay. Jeg synes ikke, vi skal måle tykkelsen.
[0:31:10] Anders Valentin: Den måde, vi bliver nødt til at tænke det her på, det er, at det bliver iterativt beriget.
[0:31:14] Anders Valentin: Og det bliver beriget kvalificeret, så kvalificeret som muligt.
[0:31:19] Anders Valentin: Så hvis der er brug for at kigge på isoleringstykkelse, så er der nødt til at være en fagperson, der kan vurdere isolering ude og se, om der er isolering på.
[0:31:30] Anders Valentin: Jeg tror ikke, den der tykkelse, selv hvis vi havde tykkelsen, så aner du ikke, hvor tyk murstenen er.
[0:31:35] Anders Valentin: Du aner ikke, hvor tyk... Det får du ikke noget ud af.
[0:31:42] Martin Collignon (2): Det handler også om, at han konstruerer huset i virkeligheden. Ikke kun renovering.
[0:31:48] Anders Valentin: Det forstår jeg.
[0:31:50] Anders Valentin: Men det er jo en balance, fordi vi skal også passe på, at vi ikke bliver for indadvendte ud til, at vi også husker, at der er nogle mennesker, som skal lave det her, og det skal give mening i deres hverdag.
[0:32:04] Anders Valentin: De skal få værdi ud af det. Jeg kan se en tilstandsrapport, der vil du gerne scanne væggen udefra.
[0:32:11] Anders Valentin: Og det kan også godt være, at vi kan sige på et tidspunkt, at der er så meget værdi i randomsmail og downstream-tingene, at du også kan få elinstallationsfolkene til at scanne udefra.
[0:32:23] Anders Valentin: Men der er ikke nogen operationel værdi i det for el.
[0:32:26] Anders Valentin: Der er helt klart operationel værdi i det for tilstand.
[0:32:29] Anders Valentin: Du kan være 100% sikker på, at tilstand har en kæmpe interesse i at scanne. Hele lortet.
[0:32:35] Anders Valentin: Det er der hele problemet ligger. Hvis vi bliver meget rapportkonkrete.
[0:32:43] Martin Collignon (2): Jeg synes faktisk, at det løser sig selv med lejligheder.
[0:32:47] Anders Valentin: Ja, der bliver det lidt konkreteret, ja.
[0:32:51] Martin Collignon (2): Jeg skulle definere, at vi skal have en adresse, og vi skal have en scanning indenfor.
[0:33:00] Martin Collignon (2): Det er for mig det minimum.
[0:33:02] Martin Collignon (2): Og så vil jeg også sige at jeg er enig med dig i at vi skal styre rækkefølgen.
[0:33:11] Martin Collignon (2): Skanner vi så rum på rum, skanner vi helt flå over, skanner vi sådan de her ting.
[0:33:27] Martin Collignon (2): Så jeg er enig med dig, at vi skal have defineret ting, og jeg synes, at vi skal have gjort det hurtigst muligt.
[0:33:33] Martin Collignon (2): Så en af de store ting, jeg er i tvivl om, er nemlig det her, hvornår skal tingene scannes.
[0:33:38] Martin Collignon (2): Så det skal vi have defineret for teamet, og det tror jeg skal prøves af med nogle forskellige UX, så vi får lidt mere tillid til det selv.
[0:33:44] Martin Collignon (2): Altså hvis vi skal tvinge folk til at lave polycam først, er det okay? Og room plans skal så ske rum for rum før de råber rapporterne, eller skal det ske rum, rapport, rum, rapport, rum, rapport, efterfølgende.
[0:34:01] Anders Valentin: Uddyb det lige igen, sorry.
[0:34:06] Martin Collignon (2): Jamen, du kunne lave Polycam helt ejendom, Roomplan helt ejendom, og så derefter starter du først med at lave attributter og annotations.
[0:34:14] Martin Collignon (2): Du kan også lave, i teorien, Polycam, Roomplan, rapport, annotation på rum, næste rum osv.
[0:34:27] Martin Collignon (2): Jeg er enig med dig, at det skal nok fastsættes ret klart, hvordan vi synes, at et besigtigt skal ske.
[0:34:34] Martin Collignon (2): Jeg synes ikke, at der skal være fuld frihed, fordi det er kompliceret.
[0:34:38] Martin Collignon (2): For det påvirker også i sidste ende, hvordan vi skal tjekke materialer.
[0:34:46] Martin Collignon (2): Skal materialet ske noget som upfront, som du lavede i din shortlist, Og så sætter du, hvad du har i shortlisten på undtagelser undervejs.
[0:34:57] Martin Collignon (2): Eller skal du i virkeligheden have en shortlist, og det er først, når du har scanned det hele, at du så sætter materiale efterfølgende.
[0:35:02] Martin Collignon (2): Altså på en 3D-model for eksempel. Ikke in the room. Det er jeg sådan set ikke sikker på nu.
[0:35:33] Anders Valentin: Jeg så helst, at man scannede det hele i én omgang RGBD. Altså at du scannede hele huset i et flow.
[0:35:58] Anders Valentin: Fordi der er alle de der transitionsområder, trapper, mellemrum, tykkelsen på væggene.
[0:36:06] Anders Valentin: Alt bliver ligesom givet af, at du scanner det hele på én gang.
[0:36:09] Anders Valentin: I hvert fald indersiden på én gang, og så ydersiden måske separat.
[0:36:14] Anders Valentin: Det tror jeg giver det bedste resultat. Også fordi kontinuiteten mellem billederne bliver større.
[0:36:25] Martin Collignon (2): Og du holder et arkæt i mindre tid på det der er vigtigste.
[0:36:30] Anders Valentin: Og det jeg så er i tvivl om, når det er sagt, det er om man er nødt til at have en værdiskæbende handling, mens du scanner.
[0:36:42] Anders Valentin: Altså, er det ligesom nok? Er det nok i sig selv for dem?
[0:36:48] Anders Valentin: Jeg er ikke i tvivl om Eindhavnsmæler, fordi det er det, de gerne vil have.
[0:36:53] Anders Valentin: Og ind- og udflytning er også det, de gerne vil have.
[0:36:56] Anders Valentin: Men dette artefakt er ikke det, de gerne vil ende op med.
[0:37:01] Anders Valentin: I tilstand og i L og i PC, for den sags skyld.
[0:37:07] Martin Collignon (2): Jeg synes det er værd at teste, fordi det er så vigtigt.
[0:37:09] Martin Collignon (2): Når jeg ser hvad Casper har lavet her over weekenden med de der frames i bunden, så ser du værdien meget hurtigt faktisk.
[0:37:14] Martin Collignon (2): Du får også en tryghed, at det hele er samlet op.
[0:37:24] Martin Collignon (2): Du får den der, når jeg har en fallback, hvis nu plans fuck op i deres 3D-producering.
[0:37:33] Martin Collignon (2): Jeg synes også, vi skal huske, at de fleste... Altså når vi når til Frankrig, så bliver den lejlighed der meget mindre end det, vi ser i Danmark generelt.
[0:37:48] Martin Collignon (2): Og det samme gælder i Danmark øvrigt. Du har ikke de her trapper i de fleste huse.
[0:37:57] Martin Collignon (2): Vi skal teste det, fordi det er ret vigtigt.
[0:38:04] Martin Collignon (2): Jeg er enig i alle dine close for at gøre det på den måde.
[0:38:09] Anders Valentin: Hvis vi skal capture RGBD, så skal vi helst gøre det i én session.
[0:38:13] Anders Valentin: Det andet der er her i det, det er at gøre det upfront.
[0:38:20] Anders Valentin: Det giver dig også en mulighed for at lade noget af netværkstingene køre i baggrunden, mens du laver resten.
[0:38:33] Anders Valentin: Man skal huske alle de der VGT-modeller og sådan noget. VGG bruger jo 512.
[0:38:42] Anders Valentin: Vi vil nok gerne have højere opløsning end det, for at være future-proof, to be clear.
[0:38:46] Anders Valentin: Men når du ligesom stoffer det ned i en lavere opløsning anyway, så kan det godt være, at du vil gøre en eller anden nedskalering plus upload i baggrunden, og så lave den store upload, når du er færdig.
[0:39:07] Martin Collignon (2): Jeg ved det ikke endnu, om der er værdi i at lave en billigere version hurtigere.
[0:39:21] Martin Collignon (2): Det jeg mener mere, det er at hvis du har en god capture og du ved den har captured noget ordentligt, altså du kan vise en ply i appen, hvor det ikke koster noget at rekonstruere.
[0:39:34] Martin Collignon (2): Altså der kan være, så her værdien verificerer jeg har datan korrekt i virkeligheden.
[0:39:40] Martin Collignon (2): At have en 3D-rekonstruktion on-device, altså Gaussian-splat, image-space-wise, er ikke så udfuldt, ikke?
[0:39:50] Anders Valentin: Nej, men det kommer også an på, hvilken arbejdsbyrde vi placerer i forbindelse med 3D-scanner.
[0:40:01] Anders Valentin: Hvis det skal bruges til noget operationelt, jeg skal lave en dormer, eller what do I know, så må vi også forvente, at de skal kunne bruge dagshagen på en eller anden måde.
[0:40:12] Anders Valentin: Og den er ikke brugbar i Ply.
[0:40:19] Martin Collignon (2): Men for Dormer vil jeg mene specifikt, at det så er Runeplan, der skal levere om, at det ikke ser ud som Dormer. Det giver materialerne, men det giver ikke arealerne.
[0:40:45] Martin Collignon (2): Vi ved også, at Roomplan kører også på en mindre version. Den kører ikke på de fulde opløsninger.
[0:40:54] Martin Collignon (2): Så hvis vi skal have genskabt arealerne for at kunne verificere dem og bringe tilbage, kan der være, at vi kan have vores egen roomplan biljavation, som oplådes således, at du får det, når du er færdig med skaden.
[0:41:05] Anders Valentin: Det giver os en fleksibilitet for at sige, at her er rekonstruktionen.
[0:41:10] Anders Valentin: Hvis vi begynder at lave noget som helst rekonstruering baseret på plyen, og det er vores måde at løse ceilings på, for eksempel.
[0:41:16] Anders Valentin: Det kunne det meget vel ende op med at blive, fordi hvis vi kan lave roomplan overflader, så kan vi også lave towerflader.
[0:41:30] Anders Valentin: Og vi snakker en lille optimering her, men det er mere, så er det rartere, at den har pipet et eller andet i baggrunden, så du ikke står og venter 20 minutter, når du er færdig.
[0:41:53] Martin Collignon (2): Så der er i virkeligheden en flaw, som er hvad for nogle data skal vi minimalt forvente, så er der noget der kommer ud fra det, som er skal vi, hvis vi skal definere nogle lifecycles, i de lifecycles skal der sagtens være nogle krav til hvad rækkefølgen skal være, du skal have lavet din point cloud inden du går videre fx.
[0:42:25] Martin Collignon (2): For det er meget nemmere at holde styr på, kan du nu lave dine dormer, fordi du allerede har scannet punk-hop?
[0:42:33] Martin Collignon (2): Vi er enige i forhold til, at det hele skal være konfigurerbart over floor.
[0:42:40] Martin Collignon (2): Så rækkefølgen, hvad bliver spurgt? Hvornår bliver det spurgt? Hvilken måde bliver det spurgt?
[0:42:45] Martin Collignon (2): Med nogle faste komponenter til gengæld.
[0:42:47] Martin Collignon (2): Det vil sige, det er Capture, der definerer, hvilken komponent det skal være.
[0:42:51] Martin Collignon (2): Og det skal være irriterende for FDE at be om en ekstra kompetent.
[0:43:00] Anders Valentin: Vi siger, du kan lave de her fem opgavetyper på de her ti koordinater. Find a way.
[0:43:10] Martin Collignon (2): Så er vi egentlig omkring det, og så har vi en punkt, som er, at vi skal have verificeret, hvad den her flaw er.
[0:43:15] Martin Collignon (2): Ikke så meget nødvendigvis i forhold til, hvad for en datatype, men mere i forhold til rækkefølgen.
[0:43:35] Martin Collignon (2): Jamen så håber jeg at den gode fellow forstår hvad jeg siger.
[0:43:41] Martin Collignon (2): I forhold til iPhone, iPad eller Bose, fortæl mig hvad du tænker her.
[0:43:46] Anders Valentin: Jeg kan ikke se nogen... Det kommer an på, på hvilket niveau vi taler.
[0:43:50] Anders Valentin: Understøttelsesmæssigt, jeg kan ikke se nogen verden, hvor vi ikke både skal have iPhone og iPad.
[0:43:56] Anders Valentin: Jeg er sad to arrive at that conclusion, fordi jeg ville egentlig gerne have fokuset, men jeg kan bare se, der er nogle brugergrupper, som vil foretrække iPhone, og der er nogle, der vil foretrække iPad.
[0:44:07] Anders Valentin: Og jeg tror, det der lidt lugter i på tværs af hele den her snak her, det lyder lidt som om, at web faktisk bliver bibeholdt ret meget, fordi vi prøver at reducere capture-appen til stupid capture.
[0:44:24] Anders Valentin: Og færdiggørelse vil vi gerne lægge et andet sted, som ikke er i appen i princippet.
[0:44:31] Anders Valentin: Og der tror jeg, at der er en nemmere sondring, der hedder, at vi bibeholder web og bygger det interface om, så det virker godt på iPad som et webview.
[0:44:39] Anders Valentin: Og at vi så siger, om du så vælger at tage en iPad med ud og har brug for at du kan sende det tilbage til serveren, altså du er reliant på internet, men at den så kører et webview inde i en iPad, og du så kan redigere det færdigt, that's up to you.
[0:45:11] Anders Valentin: Når vi begyndte at gøre det, hvis vi skifter fra den verden, der hedder, at alt ligger i appen, så bliver web... Dem får en større arbejdsbyrde end den havde oprindeligt.
[0:45:51] Anders Valentin: Du skaber en situation, hvor du har brug for en stor skærm.
[0:45:56] Anders Valentin: Og lige så snart du har brug for en stor skærm, så skal du også understøtte iPad, men du skal også understøtte laptop, og derfor bliver det web, der bliver bærende.
[0:46:03] Martin Collignon (2): Ja, jeg forstår det. Jeg overvejer, hvor meget der kan flyttes på web på telefonen.
[0:46:20] Anders Valentin: Det er ikke alting, der bliver godt der.
[0:46:24] Anders Valentin: Men det er jo også Marinas foretrukne løsning, vil jeg lige sige. Det er jo webviews.
[0:46:34] Martin Collignon (2): Så du har nogle ting der er native, og så på et tidspunkt hvor du ikke skal scanne mere, så er du tilbage.
[0:46:46] Martin Collignon (2): Så et konkret eksempel kunne være tekster fra ABC.
[0:46:56] Anders Valentin: Her er jeg engineering-mæssigt i tvivl om, hvor godt det bliver.
[0:47:00] Anders Valentin: Man kan sige, at webunderstøttelsen har været i kraftig forbedring i forhold til, hvad det var tidligere.
[0:47:18] Anders Valentin: Hvad er baren for de interfaces, vi bygger her? Jeg ved ikke, om den er særlig stor nødvendigvis.
[0:47:25] Martin Collignon (2): Jeg har lidt en forhåbning om, at 3D editing skal ske som on-device, at det ikke er relevant for web.
[0:47:39] Martin Collignon (2): Kan vi lave så meget, at de ting, der ikke kan laves på mobilen eller på en mobile device, skal laves af os?
[0:47:55] Martin Collignon (2): Men jeg har lidt en idé om, kunne man svinge sig selv til at gøre det på den måde?
[0:48:02] Anders Valentin: Jeg vil være stor fan af den model, fordi jeg tror, det er meget nemmere at sælge.
[0:48:06] Anders Valentin: Det er en meget nemmere skæring, at de ikke skal forholde sig til geometrien.
[0:48:19] Anders Valentin: Det er et meget nemmere promise, end den her usikkerhed, der er i, at de skal hyre nogen, der kan finde ud af det.
[0:48:24] Martin Collignon (2): Det er lidt min idé, og i forhold til iPhone og iPad, jeg er sådan set lidt...
[0:48:37] Martin Collignon (2): Så har du stadigvæk lige CLI-10, og du skal stadig have en Windows. Eller en stor skærm.
[0:48:44] Anders Valentin: Ja, men et 10 kører jo fint på en iPad. Det er ikke et problem.
[0:48:50] Martin Collignon (2): Jeg tror for deres arbejdsgang skal de have en stor skærm.
[0:49:02] Martin Collignon (2): Jeg er enig med dig, at jeg vil være mere i fremtiden opsat på at få ting til at virke på iPad.
[0:49:09] Martin Collignon (2): Det er lækrere, du kan vise det kunden. Jeg kan også sagtens se det, men det er mere professionelt.
[0:49:27] Martin Collignon (2): Jeg har ikke noget imod, at vi siger vi er iPad first faktisk, og så iPhone supporterer.
[0:49:33] Martin Collignon (2): Hvor iPhone er måske foretrukken af nogle iOS malere.
[0:49:38] Martin Collignon (2): Hvor det kun er RGBD og Roomplane der skal bruges. Og der er ikke brug for noget andet.
[0:49:44] Martin Collignon (2): Men lige så snart du har noget tekst-editing. Så vil du gerne have en tastatur. Altså simple as that.
[0:49:51] Martin Collignon (2): Jeg vil dog sige, at jeg synes 3D-editing skal fungere på en iPhone.
[0:49:56] Martin Collignon (2): Altså jeg synes også 3D-editing er forkert. Det er sådan geometri. At færdiggøre geometrien.
[0:50:03] Martin Collignon (2): Skal kunne gøres på iPhone. Eller så skal den ikke kunne gøres.
[0:50:08] Martin Collignon (2): Hvis du skal have en stor skærm, så er det noget, som vi har ansvar for.
[0:50:19] Anders Valentin: Det er jo også strømmescenariet, men det kommer jo allesammen til at være feasibility bundet det her.
[0:50:25] Anders Valentin: Hvis du kan løse det på en iPhone, så har du også løst problemet, kan man sige.
[0:50:38] Anders Valentin: Så for mig er jeg aspirationally helt enig, fordi det fjerner 3D-redigering.
[0:50:49] Anders Valentin: Selv på iPad er det svært at sidde og lave punkter.
[0:50:53] Martin Collignon (2): Det er enormt svært, så jeg synes ikke, det er det rigtige interface generelt.
[0:50:59] Martin Collignon (2): Det er heller ikke det rigtige interface på desktop.
[0:51:05] Anders Valentin: Nej, det er i hvert fald ikke ideelt, men det er også meget sværere.
[0:51:08] Anders Valentin: Det interface, som vi gerne vil bygge, er jo bare et lag længere nede i Solution Depth.
[0:51:26] Anders Valentin: Så det er klart et bedre interface, men det er også sværere at bygge end det, vi har gjort.
[0:51:37] Martin Collignon (2): Jeg tror tilbage til, om det er en business requirement. Er der en verden, hvor vi kan leve uden det?
[0:51:44] Martin Collignon (2): Det tror jeg ikke, der er.
[0:51:47] Anders Valentin: Jeg tror, jeg er enig med det.
[0:51:54] Anders Valentin: At der i hvert fald ikke er en verden, hvor vi ikke både har filipiner og en...
[0:51:59] Anders Valentin: Altså, vi kan ikke leve, hvis vi skal betale for gildet med filipiner.
[0:52:05] Anders Valentin: De kan ikke få 3D-redigeringer på. Altså, det kan de ikke finde ud af.
[0:52:09] Anders Valentin: Og derfor er det et spørgsmål om at reducere omkostningerne af filipinerne i virkeligheden.
[0:52:16] Anders Valentin: Det, der bliver endestaten, bliver, hvordan kan vi reducere deres arbejdespris så meget som muligt, givet de kvalitetsmål, der er.
[0:52:23] Martin Collignon (2): Men det er ikke kun pris, ikke? SLA, synes jeg, er næsten vigtigere efterhånden.
[0:52:28] Anders Valentin: Ja, jeg tror SLA-delen er helt sikkert vigtig.
[0:52:36] Martin Collignon (2): Altså, hvad jeg hørte i Frankrig, var også noget med SLA.
[0:52:41] Martin Collignon (2): Så du har nogle folk, der bare siger, hey, jeg skal have rapporten i aften, please.
[0:52:44] Martin Collignon (2): Mit hus skal ud og blive solgt i morgen, ikke?
[0:52:53] Martin Collignon (2): Hvor det er SLA'en, der er et problem, og hvor SLA'en bliver et problem af, at du har flere in the loop.
[0:53:06] Martin Collignon (2): Du vil gerne minimere, hvor ofte Filippinen er i spil, fordi det påvirker både prisen, og det påvirker SLA'en.
[0:53:23] Anders Valentin: Det tror jeg jeg er enig i.
[0:53:27] Martin Collignon (2): Men jeg ville være okay her i forhold til iPad og iPhone, så jeg synes det er enig med at det skal være begge.
[0:53:32] Martin Collignon (2): Jeg synes vi skal have 3D-editing, altså vi skal ikke have 3D-editing der kun virker på en stor skærm.
[0:53:45] Martin Collignon (2): Så vi skal forvente, at vores kunder beder os om at kunne løse 3D på en iPhone.
[0:53:53] Martin Collignon (2): Og hvis det ikke er det, så er det os, der sørger for det.
[0:53:56] Martin Collignon (2): Men jeg er enig i, at vi ville helst have, at folk faktisk bruger iPad.
[0:54:25] Martin Collignon (2): Så hvis nogle nye i morgen ringer til mig og siger hvad skal jeg have, så vil jeg nok sige iPad med en god 5G connection.
[0:54:32] Martin Collignon (2): Jeg har en lille ukendt, at jeg ved at processoren er stærkere på iPad, jeg ved ikke om det faktisk påvirker positivt eller negativt room plan accuracy.
[0:54:45] Anders Valentin: Vitali han sagde, og det er igen, hvor meget kan man ståle på ham.
[0:54:50] Anders Valentin: Han sagde, han kørte alt på iPad, fordi det kører hurtigere og bedre.
[0:54:58] Martin Collignon (2): Jeg er lidt overrasket, for jeg tror, at der er færre kameraer.
[0:55:02] Martin Collignon (2): Men det kan godt være, at den, fordi at os på iPhone, den bruger dårligere kameraer og indstillinger til room plan.
[0:55:10] Martin Collignon (2): Men det kan sagtens til gengæld være på AGPD, at det betyder noget.
[0:55:14] Martin Collignon (2): Men CoreGBD, jeg får 1920x1080.
[0:55:21] Martin Collignon (2): Jeg kan godt bede om de der ekstra store størrelser.
[0:55:32] Martin Collignon (2): Jeg kan bare ikke tro at du har dårligere end 1920x1080 på den iPad. Det har du helt sikkert ikke.
[0:55:38] Martin Collignon (2): Så i princippet det eneste der ville betyde noget var mere eller værre din fokus linse og hvor god den er til at tage lys ind og sådan noget ikke?
[0:55:49] Martin Collignon (2): Men det er måske noget vi skulle kigge ind i svar på.
[0:56:08] Martin Collignon (2): Okay, godt. Så er der online vs. offline, hvad jeg synes, der har været meget binært.
[0:56:17] Martin Collignon (2): Jeg synes faktisk, den var god til at finde den rigtige snit her, Claude.
[0:56:31] Martin Collignon (2): Men at du har... Det skal fungere offline.
[0:56:36] Martin Collignon (2): Og så er der nogle specifikke ting, Hvor du, altså Sync, Derivation Reports, skal du have en online snit.
[0:56:45] Martin Collignon (2): Det vil sige, hvis du gerne vil videre fra efter din Capture step, og efter din Verificering step, så har du et step, der hedder Online.
[0:57:01] Martin Collignon (2): Og det kan også være et step, hvor du kunne have en knap, hvor det hedder Go to Big Screen.
[0:57:16] Martin Collignon (2): Altså noget som den ikke havde svar på, og som vi skal have svar på, for det er ret betydningshuld.
[0:57:23] Martin Collignon (2): Hvis du ikke behøver online for at logge ind, kan du ende med en rapport der er for gammel.
[0:57:35] Anders Valentin: En rapport der er for gammel, hvordan?
[0:57:36] Martin Collignon (2): Hvis min JSON-konfiguration har ændret sig?
[0:57:38] Anders Valentin: Ja, det er et kæmpe problem, det der.
[0:57:41] Anders Valentin: Det havde jeg også med min app, da jeg lavede den konfigurabare.
[0:57:48] Anders Valentin: Den måde, jeg løste på, det var, at du har en eller anden 3-måneders cache af gamle states, som du skal holde backwards-compatible.
[0:58:00] Anders Valentin: Og backenden bygger alligevel, altså den er nødt til at være backwards-compatible ret lang tid.
[0:58:12] Anders Valentin: Det er jo igen den der separation af core og non-core.
[0:58:15] Anders Valentin: De der kerneobjekter, der har du brug for, at de lever selvstændigt fra konfigurationen.
[0:58:23] Anders Valentin: Fordi kerneobjekterne, der må ikke være noget koncept om staleness i dem.
[0:58:39] Martin Collignon (2): Det jeg umiddelbart tænker, hvis vi ikke tænker, at vi ikke skal være så mange engineers her.
[0:58:46] Martin Collignon (2): Vores audience er folk, der bruger det flere gange om dagen. I teorien.
[0:58:54] Martin Collignon (2): Vi er interesseret i folk, der laver rigtig mange besigtigelser.
[0:58:58] Martin Collignon (2): Du kan godt lave din rapport hver offline, hver start, hvis du er et langt bortestand og skal komme i gang.
[0:59:05] Martin Collignon (2): Men fordi du skal, for at kunne færdiggøre en rapport, gå online, så har du alligevel et synkevent der, hvor du kan både upload, men også download den nye konfiguration.
[0:59:14] Martin Collignon (2): Så vi kan basic acceptere, at den rapport, du ikke har, det er optional, så hvis du ikke kan logge ind, fordi der ikke er online, det kan du sagtens.
[0:59:29] Martin Collignon (2): Og så når du når til upload, så er det der, hvor vi kan både sige til dig, hey, det er en gammel version, det skal du bare være opmærksom på.
[0:59:44] Martin Collignon (2): Altså sagt på den måde, jeg er ikke super bekymret om Jakob Reimer kører en ekstra rapport på en gammel EPC-version.
[0:59:51] Martin Collignon (2): Vi kan alligevel ikke styre, om de er opdateret af den.
[0:59:56] Anders Valentin: Nej, og det er jo også derfor, det ikke skal være afhængigt af det.
[1:00:02] Anders Valentin: Men backenden, altså jeg har jo snakket med Henrik om det her problem.
[1:00:07] Anders Valentin: Altså backenden er nødt til at være ret backwards compatible anyway.
[1:00:21] Anders Valentin: Men du er bare nødt til at have en eller anden måned eller et eller andet, hvor du holder backwards compatible reports.
[1:00:29] Anders Valentin: Og det der med at sige, betyder det noget for dig, det er rigtig svært.
[1:00:53] Martin Collignon (2): Jeg er enig, at det skal være en form for staleness, hvis du ikke har dukket op i tre måneder, og vi måske tvinger dig så til at gøre det.
[1:00:57] Martin Collignon (2): Det ved jeg ikke, om der er en anden måde, man kunne tjekke det på, at man laver en hash i versionen.
[1:01:11] Martin Collignon (2): For mig er det mere, at vi gør det noget, at vi tillader offline, off til at starte med.
[1:01:19] Martin Collignon (2): Og jeg synes, at det er den rigtige prioritering, at man tilladet kan logge ind og lave en sag, uden at være online.
[1:01:28] Martin Collignon (2): Og så er der også, jeg synes, at hvis on-site verify, det vil sige, at du skal kunne verificere dit arbejde, uden nødvendigvis, den skal kunne være online.
[1:01:41] Martin Collignon (2): Jeg synes, at vi skal gå efter en model, hvor du ikke behøver at sende til serveren for at verificere, om du har sat materiale og b-faktorer på de rigtige steder.
[1:01:53] Martin Collignon (2): Det betyder dog ikke nødvendigvis, at det er en fuld 3D reconstruction ting, som er noget andet.
[1:02:02] Anders Valentin: Du kan jo heller ikke sige, om de har sat b-faktorer på de rigtige steder.
[1:02:10] Anders Valentin: Men du kan sige, at her er der en dommer, og du har ikke sat noget dommermateriale.
[1:02:17] Martin Collignon (2): Jeg tror godt, at vi kan sige, at det kunne være på en USDZ i virkeligheden.
[1:02:24] Martin Collignon (2): At du ikke blander en pixel perfect reconstruction med en verificeret arbejde.
[1:02:35] Martin Collignon (2): Det er ligegyldigt om det er 13 eller 15 kvadratmeter. Det er mere vigtigt, har du den materiel på dig?
[1:02:42] Anders Valentin: Men jeg tror, at USD-17, den trade-off, der i hvert fald er ved det der, det er, når du ikke rykker den der reconstruction on device, så vil du have forskellige modeller og forskellige logikker og forskellige regler på web og på devices.
[1:03:04] Anders Valentin: Det der siger ES Stormer og Not, det vil være to forskellige logikker, fordi modellerne er forskellige.
[1:03:14] Martin Collignon (2): Ja, jeg er enig, og jeg synes ikke at USSN for mig var en visualisering, men det var objektet.
[1:03:21] Martin Collignon (2): Du skal kunne placere, du skal kunne verificere dit arbejde.
[1:03:29] Martin Collignon (2): Og jeg er enig i at du har den der lidt Du selekter en element, som du ikke ved findes i virkeligheden, men i fremtiden, hvordan gør du det?
[1:03:39] Martin Collignon (2): Så vi skal nok have tænkt over her, i forhold til materialer B-faktorer, hvad kan vi abstrahere?
[1:03:44] Martin Collignon (2): At det element, du selekter, er måske mere en polygon, mere end det er...
[1:03:55] Martin Collignon (2): Ja, altså i virkeligheden, at det er næsten en annotation, som så dækker et område.
[1:04:02] Anders Valentin: Ja, og det forstår jeg godt. Det er den måde, jeg havde forestillet mig, at vi endte med at gøre det på, når du gerne vil sætte materialer på etage- eller bygningsniveau.
[1:04:22] Anders Valentin: Og når du så laver en skæring, for eksempel.
[1:04:29] Anders Valentin: så er det faktisk skæringerne, du gemmer, og ikke polygonet.
[1:04:40] Anders Valentin: Lad os sige, hypothetically, du laver den der annotering der, og siger, det her er quiz material X.
[1:04:52] Anders Valentin: Så laver den rekonstruktion, og så laver den fem polygoner. Hvordan ved du, hvilken en, eller om det er alle sammen, der hører under den annotering?
[1:05:04] Martin Collignon (2): Det synes jeg godt kunne være en del af at komme efter snillet på... Det er efterverificeringsarbejde i min optik.
[1:05:17] Martin Collignon (2): Så der er forskel med at capture data rigtigt og at... Du skal samle alt det du skal kunne lave for at færdiggøre rapporten.
[1:05:25] Martin Collignon (2): Det er ikke ens betydning med at du skal færdiggøre rapporten.
[1:05:30] Martin Collignon (2): Så det er okay for mig, at du har et ting, der hedder, hey har vi sat det hele rigtigt efterfølgende?
[1:05:37] Martin Collignon (2): Det kan sagtens være, at vi skal faktisk fjerne det der USDZ-valg lige nu, at det skal gå over til at være mere annotation-based.
[1:05:57] Martin Collignon (2): Og når jeg siger annotation, det kan også være polygon annotation, så det bliver virkelig en 3D polygon, som faktisk også kan hjælpe dig med rekonstruktion i princippet.
[1:06:12] Martin Collignon (2): Jeg synes, at UECC er absolut fantastisk til at prøve at filtrere, om du har lavet dit arbejde rigtigt.
[1:06:23] Martin Collignon (2): Har du fanget vinduer, har du fanget døre, har du fanget en væg.
[1:06:29] Martin Collignon (2): Det er ikke det, der skal bruges til at verificere, om din væg er 2 meter, 2 meter 10.
[1:06:42] Martin Collignon (2): Fordi JSON er også for præcist. Og så skal du begynde at skære i alle mulige ting og his og pist.
[1:06:57] Martin Collignon (2): Så jeg synes, at vi skal være visualiseringsformål og skal være tydigt, og forklare til personen at det er en grov tegning for at se om det hele er roughly directly correct.
[1:07:09] Martin Collignon (2): Godt, jeg tror i forhold til online og offline tror vi har nået en form for enighed.
[1:07:21] Anders Valentin: Jeg vil lige, kan vi lige to sekunder.
[1:07:28] Anders Valentin: Hvad så med det her med at se, hvilke materialer du har assignet, hvor?
[1:07:33] Anders Valentin: Altså i dag bruger de meget den der 3D-model som en form for, har jeg gjort mit arbejde?
[1:07:44] Martin Collignon (2): Jeg synes, at det skal ske efter online. Det kan sagtens ske on device.
[1:07:58] Martin Collignon (2): Vi skal have til, at den rekonstruktion skal kunne ske online.
[1:08:05] Martin Collignon (2): Det vigtigste for mig offline er, at du har samlet op alle data, der skal til, for at du kan lave dit arbejde færdigt. RGBD spiller en stor rolle her.
[1:08:43] Anders Valentin: Ja, der kommer til at være nogle udfordringer UX-mæssigt med det, men dem kan vi løse.
[1:08:54] Anders Valentin: Jeg sidder og tænker på sådan noget som, lad os sige du skærer en væg lodret, fordi der er en anden b-faktor til højre end til venstre.
[1:09:06] Anders Valentin: Så skal du lave skæringen, og så skal du fortælle, at arealet her til højre B-faktor 0,7 Og til venstre er det 0,15.
[1:09:28] Martin Collignon (2): Jeg synes den er fair, jeg synes bare at du skal kunne notere at her er det 0,7 Så du har dataen plus billedet, eller en polygon, jo et plus billedet.
[1:09:47] Martin Collignon (2): Jamen det sker bare efter du har samlet datien op.
[1:09:51] Martin Collignon (2): Det er for gate, hvornår kan vi, altså gate hvad før og efter, og du kan ikke have det halvvejs, altså du kan ikke have LLM hele vejen, fordi så skal du være online hele tiden.
[1:10:09] Martin Collignon (2): Altså, jeg vil ønske, vi kan have en 3D reconstruction, og alt kan ske lokalt.
[1:10:20] Martin Collignon (2): Det er mere, hvis vi altid er mellem to stole, kan det, kan det ikke, så ender vi altid med at være, altså, det er at reducere.
[1:10:35] Anders Valentin: Den problemstilling, jeg tror, jeg pointerer, det er, der er udfordringer med, at du laver, med den der offline tankegang, hvor du ikke har rekonstruktionen on device, at det ikke er the final object i et PC-land.
[1:11:02] Anders Valentin: Man kan se dem som staged changes. De skal reconciles ind i den endelige model på en god måde.
[1:11:16] Anders Valentin: Og der er sådan et eller andet med 3D, at der sker bare nogle virkelig weird edge cases.
[1:11:33] Anders Valentin: Og det eneste der er, det er, at informationen er god nok til, at en menneske kan lave redigeringerne efterfølgende.
[1:11:41] Anders Valentin: Men hvis de ligesom skal materialisere sig ind i objektet, appliere automatisk, så er det der, hvor der åbner vi i hvert fald en sårbarhedsflanke, som er, at de der reconciliations er svære at lave, altså stabile, ikke?
[1:12:02] Martin Collignon (2): Jo, men det synes jeg nu, at UX også, det vil sige, skal vi, når du har lavet en note eller polygon eller whatever, der handler om for eksempel b-faktorer, at det er sådan noget, du skal reviewe efter gaten, der har været online?
[1:12:14] Anders Valentin: Ja, men det er bare, det skal du de facto, det vil du skulle.
[1:12:39] Anders Valentin: Så den flanke vi åbner, det er, at vi skal kunne håndtere det der problem, fordi vi kommer ikke videre end information og så kommer vi til at skabe en masse arbejde for dem.
[1:12:57] Martin Collignon (2): Det er forskel mellem online offline og on-device vs. off-device.
[1:13:05] Martin Collignon (2): For mig skal du kunne gøre det, du nævner på on-device. In-field.
[1:13:28] Martin Collignon (2): Det vi snakker om her reelt er mere om, skal 3D-modellen Reconstruction kunne være færdig på on-device uden internet?
[1:13:40] Anders Valentin: Og det er ikke fordi det er et mål i sig selv. Det synes jeg ikke, det er.
[1:13:45] Anders Valentin: Det jeg anfekter det er, om det gør det sværere for os at lave det her on-device in-field interface.
[1:13:54] Anders Valentin: Fordi at Du har en ændring af modellen fra de ændringer, du stager, til den model, du applierer dem på. Det bliver to forskellige modeller.
[1:14:07] Anders Valentin: Det kan være dyrere at gøre, end at lave rekonstruktionen på device.
[1:14:17] Martin Collignon (2): Den udfordring, jeg tror også, den skaber, er, hvis vi skal fjerne det problem, så betyder det også, at vi skal fjerne muligheden for en escape hatch af filipinerne.
[1:14:29] Martin Collignon (2): Fordi 3D-modellen skal ud og ind. Du har en online-cut, som filipinerne nu skal redigere.
[1:14:42] Martin Collignon (2): Og hvis du har brug for at vente på, at den kommer tilbage, før du kan fortsætte dit arbejde, sætte materiale rigtigt osv.
[1:14:52] Martin Collignon (2): Så skal den enten være helt hurtig, eller så kan du ikke færdiggøre dit arbejde.
[1:15:00] Anders Valentin: Ja, men hvad har det med det her at gøre?
[1:15:02] Martin Collignon (2): Jeg forstår problemet, at du har to modeller. Lad os sige, at du har en USDZ og en JSON.
[1:15:34] Martin Collignon (2): Lad os sige, som vi siger, at vi altid skal have det rigtige svar, som er en JSON-model. Du ender med en rigtig, rigtig svær bygning, som du ikke kan konstruere på device.
[1:15:49] Martin Collignon (2): Hvis du ikke har en perfekt model, hvordan skal du så sætte en dormer?
[1:16:00] Martin Collignon (2): Det er, at du skaber en alternativ realitet, som er at tage en annotation, som er en form for fiktiv, ustruktureret information, som du helst skal bruge til at kunne lave det senere.
[1:16:30] Martin Collignon (2): Men du løser ikke problemet af, at hvis du ikke kan løse 3D rekonstruktionen selv, så er du nødt til at få en 3D-part med.
[1:16:43] Anders Valentin: Øh, enig. Men det er nok... Jeg ser det som to sidestillede problemer for mig.
[1:16:57] Anders Valentin: Du er nødt til at kunne håndtere, at der er lavet en ændring on-site, uanset om filipinerne bliver involveret i billedet eller ej.
[1:17:19] Anders Valentin: Hvis der er lavet det der split, vi for eksempel taler om tidligere, men det er lavet på den samme model, så kan filipinerne stadig godt se den information.
[1:17:39] Anders Valentin: Min godt feeling er, at det kan være et ret hairy problem, det der i sig selv.
[1:17:46] Anders Valentin: Og når jeg siger on device, så er det ikke fordi, det ikke skal være den samme model, som filipinerne ikke arbejder på. Det skal være én.
[1:18:00] Anders Valentin: Så er det enten information, og så applierer man editsene.
[1:18:05] Anders Valentin: Der kan være en kæmpe omkostning i at reconcile alle de der forskellige ting bagefter.
[1:18:14] Martin Collignon (2): Nå, det er jeg enig i, og det er faktisk derfor jeg sagde måske at den der USDZ segmentvalg er et forkert ting.
[1:18:22] Anders Valentin: Ja, men spørgsmålet er om UX-mæssigt, om de kan leve uden?
[1:18:29] Martin Collignon (2): Ja, ellers så er det at vi konverterer dataen til at være uafhængig, men det er noget der lever AR-kit frem for det er noget der lever USDZ'en.
[1:18:44] Anders Valentin: Jo, så de peger på vinduet.
[1:18:48] Martin Collignon (2): Eller at du i virkeligheden laver... Vælg at der ender det som en annotation i vores system.
[1:18:57] Martin Collignon (2): Når du raycaster mod et vindue, så er det det, du skal appliere til det vindue, frem for at sige, at det er det samme vindue, der er i JSON-modellen og i WSD-setten.
[1:19:27] Anders Valentin: Altså, vi får det samme problem, hvis vi går over til fuld RGBD.
[1:19:34] Anders Valentin: Så vil du skulle have sådan et annotation base, fordi der er ikke nogen objekter endnu før at den har været forbi.
[1:19:42] Martin Collignon (2): Så du er nødt til spørgsmålet mere, hvad du skal reconcile, hvornår du skal reconcile det i virkeligheden.
[1:19:53] Martin Collignon (2): Ellers så skal vi tilbage til det spørgsmål, som en chiller havde, som var det perfekte 3D-scan indenfra, ikke?
[1:20:11] Anders Valentin: Men der er bare noget visualiseringsudfordring her, fordi det vil være svært for dem at se på en masse noter og vide, hvor har jeg sat de rigtige materialer og hvor.
[1:20:27] Martin Collignon (2): Der er en grund til, at vi har den der farvelæg efter materialer.
[1:20:31] Anders Valentin: og det er nok det mest brugbare, der er lavet på web i lang tid.
[1:20:37] Martin Collignon (2): Jeg tror, vi snakker om to forskellige ting.
[1:20:41] Martin Collignon (2): Jeg synes, det er en af visualiseringen af notation, som vi har det i dag, og den anden er, hvordan det leverer som data i backenden i virkeligheden.
[1:20:49] Martin Collignon (2): Du kan i teorien igen have en USDZ, hvor du visualiserer det, som det er i dag.
[1:21:05] Martin Collignon (2): Lad os sige, at du har en forkert dør, bl.a. som et vindue eller omvendt.
[1:21:25] Martin Collignon (2): Du kan præge på USSL-segmentet, som er en polygon, der lever i AR World. Og så ændre dens attribut.
[1:22:01] Martin Collignon (2): Jeg er enig i, at det er trade-offs, og der er ikke nogen rigtig god måde. Altså, det er 3D, det er diff and a lort.
[1:22:24] Martin Collignon (2): Og jeg er enig også, at Wasted 7 er sådan ret... Den er farlig, fordi den nogle gange viser en virkelighed, hvor bagvedlæggende data, altså raw JSON, viser faktisk noget andet.
[1:22:41] Martin Collignon (2): Jeg ser mere en USDZ som en måde at kommunikere grovhed, hvor du ikke skal forholde dig til detaljerne.
[1:22:55] Anders Valentin: Men det kan man skabe på andre måder. Det kan også ligge i visualiseringslaget, at du bare laver noget, der er grovere.
[1:23:06] Anders Valentin: Det er jeg sgu i tvivl om. Der skal vi tage trade-offs igennem med ingeniørerne.
[1:23:15] Martin Collignon (2): Det er også meget feasibility-relateret. For mig skal det være lokalt rigtigt versus globalt rigtigt.
[1:23:43] Martin Collignon (2): Grunden til, at jeg er mindre bekymret, er fordi, at det er sjældent, at du har Rigtig små arealer.
[1:24:07] Anders Valentin: Jeg er mest bekymret for at servere UX'et i det her. Det er den, der er mest concerning for mig.
[1:24:20] Anders Valentin: Det der med at skulle tage et billede af noget og så sætte et materiel, det er ikke så fedt.
[1:24:32] Martin Collignon (2): Jeg tænker på andre UX's, og det kan vi spørge. Du har så et AR-view, hvor du har pan, og så i stedet for at du kan skære frit, så har du virkelig mere en slider-agtigt, ikke?
[1:25:02] Martin Collignon (2): Correct me if I'm wrong, men her er det mere et spørgsmål. Har jeg registreret det? Har appen registreret det jeg sagde?
[1:25:09] Anders Valentin: Jeg tror ikke kun det er det. Det er også har jeg registreret det jeg skulle. Som er en anden problemstilling.
[1:25:23] Anders Valentin: Altså de har svært ved at have overblik over har jeg sat alle de overrides som jeg sagde og som jeg egentlig så.
[1:25:46] Anders Valentin: Jeg tror, at flere af dem, jeg har spurgt om det her, de havde svært ved at huske, hvilke materialer der var hvor, når de havde været væk fra rummet.
[1:26:00] Anders Valentin: I en fremtid hvor det er RGBD only, så kan du ikke gøre det på andre måder, fordi du har ikke nogen rekonstruktion af overfladerne.
[1:26:16] Anders Valentin: Problemet er, at der ikke er et objekt, de kan se, og de kan klikke på, og de kan sætte materialet, og det ændrer farve.
[1:26:23] Anders Valentin: Det er den der fidelitet i, at jeg har sat en override her. Den, som jeg er bange for at stable.
[1:26:31] Martin Collignon (2): Men der kunne være nogle andre måder. Måske kunne det være tilsagt med et billede.
[1:26:36] Anders Valentin: Ja, men jeg tror, det bliver ret frustrerende for dem at skulle tage billeder af alle steder, hvor der er material overrides.
[1:26:58] Anders Valentin: Men det er meget langsommere end at klikke på modellen, og du ved den andre farve.
[1:27:01] Martin Collignon (2): Men hvis jeg har frames, så kan dit valg af materiale uafhængigt af billedet, der bliver vist.
[1:27:12] Martin Collignon (2): Hvis vi kan re-caste billedet til det sted, du har valgt på OSS-sætten, hvis vi gør det hurtigt, fordi vi har den frame, så bliver det mere den måde, du husker det på, er mere en reminder af, hvor vi var henne frem for den præcise kvadratmetervalg.
[1:27:33] Anders Valentin: Den præcise kvadratmetervalg er ikke så vigtigt. Det handler om, at du kan se en 3D-model, og den har skiftet farve.
[1:27:41] Martin Collignon (2): Ja, og jeg tror, der er forskellige måder at gøre det på.
[1:27:46] Martin Collignon (2): Det kunne være visualisering på en 3D-model. Det kan være faktisk en 2D-model. Maybe.
[1:28:01] Martin Collignon (2): Jeg forstår behovet for lokalt at forstå, om du har den rette til brugt niveau.
[1:28:06] Martin Collignon (2): Så når du får lavet et flot rum, så kan du faktisk tjekke, har jeg ikke nok husket rigtigt, men også har planen husket rigtigt.
[1:28:20] Martin Collignon (2): Jeg ville have et spørgsmål, som er, jeg har ikke rigtigt kommet af den til at gøre det rigtigt i rekonstruktionen efterfølgende.
[1:28:31] Anders Valentin: Ja, men det er et transient problem. Altså det er et tillidsproblem, som der er i en opstartsperiode.
[1:28:41] Anders Valentin: Det er et træningsproblem. Kærneproblemet er, at de bare ikke har noget overblik.
[1:28:50] Martin Collignon (2): Godt. Jeg forsøgte lidt at sige det. Area verification is feasible on device.
[1:29:03] Martin Collignon (2): Det er for mig vigtigt, at du skal have en 3D rekonstruktion på mobilen, men givet, hvad vi har skrevet før, er det okay, at den skal efter en webadgang eller en onlineadgang.
[1:29:21] Martin Collignon (2): Jeg synes godt, at vi kan sige, at der skal være en mulighed for at tjekke, om det arbejde, du har lavet, er vigtigt.
[1:29:35] Martin Collignon (2): Der er noget vigtigt her, det er ikke for sjovt. Det skal kunne fungere, at du skal få lavet stedet, velvidende at du har opsamlet alt.
[1:30:00] Anders Valentin: Det, jeg tænker, er, at det her er et område, hvor de har brug for specificitet, ingeniørerne.
[1:30:06] Anders Valentin: Så vi bliver nok nødt til at komme med et bedre, et mere klart svar end det her.
[1:30:14] Anders Valentin: Det, de leder efter, det er, hvad er løsningen?
[1:30:23] Martin Collignon (2): Er det sådan noget, som du vil tage på dig for at forstå, hvad det faktiske behov er?
[1:30:29] Martin Collignon (2): Og så måske kigge på forskellige muligheder på rumniveau, korrekt?
[1:30:34] Anders Valentin: Ja, korrekt. Det kan jeg godt tage på mig.
[1:30:41] Martin Collignon (2): Cool. Den tror jeg ikke nødvendigvis bliver særlig kompliceret.
[1:30:51] Martin Collignon (2): Should FGEs control the interpretation and or surface layer? Problemet op i flere forskellige ting.
[1:30:59] Martin Collignon (2): Den ene er Report Derivation and Preparation, Binding, Jurisdiction, Calc, Core Canvas and Juristics i virkeligheden.
[1:31:22] Martin Collignon (2): Jeg synes at de skal ikke eje Arealer, så arealberegninger, hvordan du går fra 80'er til 90'er til measured areas, er noget, der skal laves i core.
[1:31:51] Martin Collignon (2): Hvordan rapporten ser ud, så hvordan Kelvin ser ud i dag, synes jeg, at FD skal eje.
[1:31:58] Martin Collignon (2): Der hvor det er murky er det FDE'en der skal forstå materialer. Hvor jeg er stærkt proponent for at det er noget som FDE'en skal eje.
[1:32:34] Martin Collignon (2): Og så er det op til snittet til at forstå hvordan skal en tilæg, hvordan skal det modelleres i vores system eller thermal linear loss hvordan skal det forstås i systemet når det afhænger af flere forskellige ting.
[1:33:02] Martin Collignon (2): Det vil sige, at FD'en skal have lov til at vælge, at hvis man skal give den relation, der er mellem den junction mellem væg og slab, så er det det her tabelt, du skal hente i virkeligheden.
[1:33:23] Martin Collignon (2): Det er ikke ens betydende, at det hele skal lave i frontend. Du kan have en backend til FD'en.
[1:33:49] Martin Collignon (2): Der vil jeg bare have skrevet det endelige uværdi, og så vil jeg bare have skrevet i teksten, at der er et tillæg på 0,01 eller en note.
[1:34:02] Anders Valentin: Jeg ser det meget ens, men måske er der lidt forskel.
[1:34:12] Anders Valentin: Det, der er vigtigt for mig at klargøre, det er, at jeg tror, det er core at sige, at her er der et linjetab.
[1:34:22] Anders Valentin: Og det er også core at sige, Modellen for det her linjetab kan være de her fem forskellige afhængige af, om det er Frankrig eller Tyskland.
[1:34:42] Anders Valentin: Og det kan godt være en ret bred datamodel for det.
[1:34:52] Anders Valentin: Men du har en model for hvert segment, eller krydset mellem hvert segment, som beskriver, hvilke måder det her specialelement kan fortolkes på.
[1:35:06] Anders Valentin: Men fortolkningen er FDE'en, der så siger, givet at du har klassificeret det her som en krydsflunke, så skal du sætte uværdi 3,4 på.
[1:35:23] Martin Collignon (2): Men det er vigtigt, at I kan love, at det ikke er landbestemt faktisk.
[1:35:30] Anders Valentin: Det var bare for at eksemplificere. I kan love siger, at der er de her fem fortolkninger af det her linjetabsobjekt.
[1:35:44] Anders Valentin: Og så er det FDE1, der siger, at i Tyskland bruger man fortolkning 2.
[1:35:55] Anders Valentin: Der bliver noget murkiness i at lige præcis linjetab er faktisk et godt eksempel, fordi den er bundet til andre objekter.
[1:36:17] Anders Valentin: Den måde vi har lavet skillevægtsfundamenter er ret forkert i den konstellation, fordi du vil hellere ligge det på lignetapesobjektet.
[1:36:48] Martin Collignon (2): Men lige med den er det faktisk relativt nemt, fordi det er et materialets uværdi i virkeligheden.
[1:37:21] Martin Collignon (2): Altså der er et uklart ejerskab af den logik.
[1:37:26] Anders Valentin: Hører den til, fordi der er et fundament og en skillevæg? Eller hører den til, fordi skillevæggen vil satte tal i materiale?
[1:37:46] Martin Collignon (2): Den er defineret som materiale frem for den fysiske realitet, som vil være at tegne rundt om skildevæggen.
[1:37:57] Martin Collignon (2): For kommersielle bygninger kan du ikke lave den simplificering.
[1:38:05] Martin Collignon (2): Her er det for mig et specifikt bevis på, at når det ikke handler om den kan love for mig skal tage en fysisk realitet og lave den om til struktureret data.
[1:38:19] Martin Collignon (2): Og den strukturerede data skal FDI'en tage fat i og applie den i virkeligheden den fortolkningslag som enten kunden, som regulering har oven på den fysisk realitet.
[1:38:45] Martin Collignon (2): Hvis der mangler et lag, hvis du ikke kan genbygge den fysiske virkelighed endnu, fordi der mangler noget fra KALOR, så skal FD'en spørge den gode KALOR udvikler, nu har jeg brug for NatGrowthArea, der går halvvejs.
[1:39:08] Martin Collignon (2): Og så er det KALORs opgave at udstille sig halvvejs gennem muren, det er sådan det er.
[1:39:13] Martin Collignon (2): Og der hvor der er igen en undtagelse, er det der med faktisk materiel tykkelse. Materiel tykkelse og materielet selv i virkeligheden.
[1:39:33] Martin Collignon (2): Fordi at Kalor også skal forstås igen af en fysisk virkelighed, hvor short text i FD1 det er en 34 cm tykke væg med isolering i men den fortolkning skal leve i det er næsten FD'en der skal binde sammen.
[1:40:17] Martin Collignon (2): Men den skal, i min optik er den nød til hvad der skal laves.
[1:40:21] Martin Collignon (2): Og så er det FD'ens arbejde at binde whatever, altså en infinite katalog af materialer med nogle UIDs på den anden side.
[1:40:44] Martin Collignon (2): Det er for mig, at du har en FD, der laver core-contracts. Der er en mellemlag, der handler om forståelse af HBEMO osv.
[1:40:57] Martin Collignon (2): Niels, hvis han er FD'en, han er ansvarlig for at fortolke den fysiske realitet rigtigt til at lave en fransk rapport.
[1:41:08] Martin Collignon (2): Du har en Calore, du har en fortolkningslag fra Frankrig, og så er du derefter en visualiseringslag, en Kelvin, som kan ændres afhængig af, hvilken kunde det er.
[1:41:21] Anders Valentin: Kan vi tage pause fra nu til kl. 12.30, og så fortsætte dialogen?
[1:41:28] Martin Collignon (2): Vi har en management-møde om 15 minutter, så du får 15 minutter.
[1:41:34] Anders Valentin: Jeg har brug for at skrive en e-mail til det der gruppe der.
[1:41:48] Martin Collignon (2): Jeg synes, vi skal holde det der management-møde, fordi det er det, vi er blevet bedt om.
[1:41:56] Martin Collignon (2): Og så kan vi sagtens fortsætte vores møde meget senere hen på eftermiddag.
[1:42:02] Anders Valentin: Anders Valentin left

## Session 2 — French Shape, RGBD, Bocheck flow, pricing, renewals

Meeting: Anders/Martin | AI Notes by Fellow

### AI Notes (Fellow)

#### Summary

Martin og Anders diskuterede arkitektur og implementeringsstrategi for 3D-scanning i Plans-platformen med fokus på at adskille core-kode fra landspecifik logik. De besluttede at elektrikere skal lave RGBD-scanning men ikke Roomplan for at forenkle implementeringen, og at RGBD altid skal udføres ved EPC-besøg uanset tidligere scanning. Teamet blev enige om at V1-løsningen skal holdes simpel uden reconciliation mellem forskellige scan-typer, selvom dette skaber nogle operationelle udfordringer for BoCheck's workflow.

De drøftede også input-metoder på tværs af platforme, hvor visit time judgments skal ske natively mens andet kan være web-baseret. For håndtering af manuelle edge cases foreslog de en prisstruktur på 300 kr for 1 dags behandling eller 50 kr for 2 dage, estimeret til at dække ca. 2-5% af sager. Juridiske spørgsmål om rapportfornyelser kræver afklaring med ledelsen omkring hvad der udgør en fornyelse versus en ny tilstandsrapport.

#### Action items

- Anders Valentin, Martin Collignon: Udarbejd en detaljeret matrix med specificitet for Q5 omkring opgaver fordelt på platforme (native offline, native online, web online). (12:20)
- Anders Valentin, Martin Collignon: Præsenter spørgsmålet for Thomas om hvorvidt el-rapporter skal indgå i 3D-rapportvejen. (28:01)
- Anders Valentin, Martin Collignon: Tegn de forskellige muligheder op for håndtering af el-besøg og 3D-scanning, så teamet ved hvad de vælger imellem. (30:59)
- Martin Collignon: Undersøg og få klarhed over hvor ofte der laves elinstallation uden tilstandsrapport. (42:10)
- Martin Collignon: Udarbejd svar på managementspørgsmål baseret på mødetranscriptet og send til Anders til review. (1:06:15)
- Martin Collignon: Indkald Anders til møde senere på ugen om Engineering Decision Router dokumentet. (1:06:55)

#### Decisions

- Core-koden skal ikke indeholde dansk-specifik regulering - landspecifik logik skal holdes adskilt fra kerneplatformen. (10:08)
- Elektrikere skal lave RGBD-scanning og ikke Roomplan - denne tilgang forenkler implementeringen betydeligt. (19:19)
- RGBD-scanning skal altid udføres ved EPC-besøg uanset om der tidligere er scannet, for at sikre korrekt tilstandsdokumentation. (22:08)
- V1-løsningen skal ikke inkludere reconciliation mellem forskellige scan-typer og skal holdes simpel frem for smart integration. (46:47)
- RGBD-scanning er en del af tilstandsrapportens deliverable, ikke som en separat eller valgfri komponent. (57:09)
- Plans vil ikke understøtte revidering af tilstandsrapporter der blev lavet uden spatial data - det kræver ny scanning. (1:00:28)

#### Topics

**Kodearkitektur og rolleadskillelse**

- Diskussion om hvorvidt kode skal refaktoreres til et 'French Shape' pattern, hvor enums og domains kommer fra FD-systemet i stedet for at være flettet ind som i det danske system. (06:03)
- Teamet skal adskille diskussionen om kode-lag fra personroller - fokus skal være på om core-platformen skal forstå dansk regulering (nej) fremfor at diskutere FDE- vs core-engineer roller. (08:02)
- Ved opgavedefinition skal der tagges om opgaven er 'Capture Core' eller 'Surface' for at tydeliggøre hvilket lag den tilhører, så personer kan have opgaver på tværs af lag. (09:18)
- Core-koden skal ikke indeholde dansk-specifik logik, men det er ikke en hastesag at fjerne eksisterende dansk kode - fokus er på ikke at tilføje mere. (10:20)

**Input-metoder og platforme**

- Spørgsmål om hvor meget input skal ske via web versus app - svaret er begge dele, med en split mellem visit time judgments (native) og alt andet (kan være web). (10:49)
- Anders foreslår at skabe en specificitetstabel med tre kolonner: skal ske natively på app offline, skal ske i app online, skal ske kun på web online. (11:43)
- EPC-redigering og 3D-redigering er de største udfordringer for input-metode beslutninger - svært at få Kelvin og Lumens håndtering ud af hovedet ved planlægning. (12:47)
- 3D-redigering på web skal ikke være et tvunget krav som i Kelvin i dag, men kan være tilladt for undtagelsestilfælde (3% cases) hvor der er fejl. (13:54)

**3D-scanning og Roomplan workflow**

- Room-by-room modellen skaber udfordringer for elektrik-, scanner- og EPC-flow, da det kræver two-way synkronisering mellem device og server hvis elinstallatør og EPC-konsulent er forskellige personer. (15:39)
- Diskussion om hvorvidt byggesagkyndig kan rette i allerede processeret scan - Martin foreslår at gå tilbage til det specifikke rum, re-scanne, og re-merge med resten af huset. (17:17)
- Elektrikere skal lave RGBD men ikke Roomplan som udgangspunkt - dette forenkler implementeringen betydeligt og undgår kompleks merging af modeller. (19:19)
- Elektriker kan i teorien bruge AR-kit til annotations uden at scanne - næste person relocaliserer i samme AR-world og laver Roomplan efterfølgende. (20:01)
- RGBD skal altid tages ved EPC-besøg uanset tidligere scanning, da man ikke kan vide om tidligere scan var korrekt eller om noget har ændret sig - omkostningen er på virksomheden, ikke individet. (22:08)

**RGBD integrationsstrategi**

- Tre mulige tilgange blev diskuteret: automatisk model-merge via AI-lokalisering, manuel 3D-plan reconciliation, eller merge baseret på rumnavne. Ingen løsninger er helt 'clean' pga. manglende 100% træfsikkerhed. (33:37)
- RGBD og RoomPlan forsvares overfor el-branchen ved at muliggøre proaktiv indsættelse og AI-lokalisering - funktioner der kræver topologi og ikke kan laves uden den. (34:07)
- Enighed om ikke at implementere multiplayer funktionalitet samtidig med RGBD-integration pga. bekymringer om tidslinjen og kompleksitet. (36:09)
- Anders foreslår at elmænd enten laver fuld scanning eller ingen scanning, mens Martin mener reconciliation ikke haster i første omgang. (38:38)
- Diskussion om hvorvidt tilstand og energimærke altid skal lave RGBD over RoomPlan - Anders udtrykker bekymring for at elmænd ikke vil acceptere 15 minutters ekstra scanningstid oven i nuværende 40 minutters gennemsnitstid. (42:42)
- Løsningsforslag om at tvinge brugere til at tage billede ved indgang af hvert rum for at muliggøre image correspondence mellem RGBD og el-installationer, med forventet rumpræcision frem for centimeter-præcision. (43:01)

**Teknisk arkitektur valg**

- Med den tilgængelige tidslinje lugter det mere af at lave 'dum' version 1 frem for smart integration mellem forskellige scanner-typer. (36:18)
- Martin udtrykker bekymring for at blive for blandet ind i kundernes interne CM-systemer og data - ønsker at undgå afhængighed af deres operations. (39:09)
- Forslag om at hver service skal være uafhængig af hinanden så meget som muligt, med eventuel relokalisering som efterfølgende 'magic' frem for at personer skal tænke over hvem der kommer før/efter. (41:18)
- Anders foreslår at udskyde reconciliation af el-installationer da det operationelt ville blive for komplekst, og 30% længere tid ville være urealistisk for elmænd at acceptere. (44:33)
- Enighed om at hvis der implementeres tekstautomatisering med speciale-beskrivelser, kan elmanden ikke vente på at EPC-rapporten er færdig for at få RoomPlan og automatiske beskrivelser. (45:40)

**BoCheck operationel flow**

- BoCheck har samme person til både NGI og 27 mange gange, så de skal kunne flytte elbesøget senere hvis det skaber problemer operationelt. (22:34)
- BoCheck bestræber sig på at sælger kun skal være hjemme én dag - typisk er der 5 elbesøg og 2 af de andre typer, hvilket skaber koordineringsudfordringer. (23:37)
- Tilstand har højeste dækningsgrad sammenlignet med el og EPC, da den også gælder for sommerhuse i nogle tilfælde - el er mere et tilkøb. (24:35)
- To separate problemer identificeret: reconciliation af data (drevet af Thomas' ønske om 3D) versus kommercielle operationelle problemer med at tvinge bestemte arbejdsgange på BoCheck. (25:30)
- Forskellige løsninger diskuteret: tvinge el til RGBD/Roomplan, få elmand til relocalization i AR World, eller acceptere forskellige arbejdsgange for el versus tilstand/EPC med senere reconciliation. (31:56)

**Manuel arbejde og prissætning**

- Diskussion om hvor meget manuelt arbejde der er acceptabelt - forslag om at målsætte 5% af sagerne som acceptabel andel af manuel behandling. (47:43)
- Prisstruktur foreslået: 300 kr for 1 dags behandlingstid eller 50 kr for 2 dage som en form for forsikring kunder kan købe til de komplekse sager. (48:38)
- Ved 2% af 20.000 årlige sager (≈400 sager) ville det kræve ca. 1,5 medarbejder i Filippinerne til manuel behandling - langt billigere end at ansætte i Danmark til 100.000 kr/måned. (50:27)
- De resterende 5% af sager er også de mest komplekse og tidskrævende - proportionelt med kompleksitet, så prissætning skal baseres på de værste procenter. (49:36)
- SLA-aftale skal være fleksibel og commit til et halvt år for derefter at analysere og justere, da det bliver et moving target med selvselektering (travle perioder øger sandsynlighed for betalt behandling). (51:06)

**Rapportfornyelser og juridiske spørgsmål**

- Tilstandsrapporter holder kun et halvt år, og der er forskel i hvordan fornyelser håndteres internt - nogle kører drive-by, andre scanner igen. (58:00)
- Udfordring med at tilføje nye noter kræver interface - forslag om 3D-placering af noter (selvom UX er svært), mens sletning af noter er nemmere. (59:41)
- Hvis der ikke laves nyt scan kan de ikke dokumentere hvordan ejendommen så ud den specifikke dag - nødvendigt for juridisk forsvar. (1:01:03)
- Behov for at konsultere Lukas eller Thomas om hvad der juridisk og praktisk udgør en fornyelse - usikkerhed om genbesøg efter 6 måneder stadig tæller som fornyelse eller er ny tilstandsrapport. (1:04:09)
- Interface-forslag med 'verify, verify, verify' flow blev diskuteret, men forskellen mellem lovlige og praktiske krav er uklar og skal afklares med ledelsen. (1:03:03)

### Transcript

[0:00:00] Martin Collignon, Anders Valentin: Martin Collignon, Anders Valentin joined
[0:00:40] Martin Collignon (1): Hallo!
[0:00:40] Martin Collignon (1): Jeg fik en kold klud i hovedet af ham der Andersbo Pedersen, som vi tidligere fær'er, meter, dude.
[0:00:54] Martin Collignon (1): Hvad er det for noget? Hvad er det for noget?
[0:01:39] Anders Valentin: Og hvad skete der med ham?
[0:01:42] Martin Collignon (1): Ja, han var bare ikke super nice for at gøre det enkelt Altså jeg forsøgte at forklare ham hvad vi havde og jeg havde lidt bræftet oplysninger Han var sådan, du skal ikke gøre mere hjemmearbejde, for jeg er relevant, faktisk.
[0:02:00] Martin Collignon (1): Jeg kan simpelthen ikke finde svar på mine spørgsmål, så derfor ringer jeg. Men han mente, at der var rigtig mange dage til allerede at sige engelsk. Så jeg har sendt demoindkaldelser til alle potentielle og relevante labeler.
[0:02:21] Martin Collignon (1): Så vi ikke bare har et dobbelt down på noget, som ikke hænger sammen.
[0:02:26] Martin Collignon (1): Men jeg har mere og mere fornemmelse i den industri, at det er lidt en cirkuityrk, hvor der ikke rigtig er nogen der gør noget.
[0:02:42] Anders Valentin: Der er sådan 100 mennesker, som er i industrien, ikke?
[0:02:47] Martin Collignon (1): Nej, ja, men omvendt tror jeg, at der er meget meget meget supply og ingen demand, altså alle synes, at idéer om robotter er gode nok, men ingen indkøber robotter.
[0:03:01] Martin Collignon (1): Så jeg tror der er lidt en VC Circle Jerk i forhold til det, og it's gonna work, it's gonna work.
[0:03:07] Martin Collignon (1): På samme måde som der er måske noget i forhold til selvkørende biler for 10 år siden, ikke?
[0:03:12] Martin Collignon (1): Men, well seen. Nå. Vi er kommet godt igennem allerede, synes jeg.
[0:03:25] Anders Valentin: Vi skal også lige køre det igennem den liste, de faktisk sendte, så vi er sikre på, at vi har svar på de spørgsmål også.
[0:03:31] Martin Collignon (1): Jeg har tænkt mig at køre, hvad hedder det? Få codecheck, map det tilbage.
[0:03:40] Anders Valentin: Ja, vi kan jo lige se om der er nogen vi føler vi kan svare på. Før vi laver den mapping.
[0:03:46] Martin Collignon (1): Jeg har også tænkt mig, at nu har jeg lavet keyboard transcribe og min hardware er helt fucked.
[0:04:02] Martin Collignon (1): At de steder, hvor vi har været måske lidt for opinioneret i forhold til løsninger osv. Sige, gør det klart, at det ikke sådan er.
[0:04:11] Martin Collignon (1): Det er, hvor vores forståelse er lige nu. Men det er sådan mere input, end det er tænkt med.
[0:04:20] Martin Collignon (1): Men der er ikke en super stærk... Altså, det er mangel på omkostning og forståbiner.
[0:04:29] Anders Valentin: Ja, hvad er du bange for?
[0:04:32] Anders Valentin: Er du bange for, at de føler sig kommanderede, eller er du bange for, at de kommer tilbage, og så prøver de at lave et spæk på præcis det, vi har, og så tænker de ikke over omkostningerne?
[0:04:40] Martin Collignon (1): Begge dele, ikke? At vi kommer overhovedet og siger, no, fuck, det er her, hvad I skal bygge. Som jeg ikke synes, er nødvendigvis det rigtige.
[0:04:49] Martin Collignon (1): Altså, at svare på de ordene og spørgsmål, tror jeg, er det rigtige.
[0:04:53] Martin Collignon (1): om vi skal have en cash på tre måneder til vores surveys det tænker jeg de kommer og svarer på allerede.
[0:05:02] Anders Valentin: Jeg tror det er okay at vi siger at det er et fint trade-off fordi lige nu der kan de jo tænke sådan noget som infinite backwards compatibility og sådan noget hvilket de ikke behøver.
[0:05:35] Martin Collignon (1): Yes, men jeg bare... Kig på, hvad vi har af constraints, og vær ikke... Bare for at sige, jeg kommer ikke til at dumpe, hvad vi har snakket om direkte.
[0:05:45] Martin Collignon: Martin Collignon started sharing their screen
[0:05:52] Martin Collignon (1): Godt. Yes. Jeg tror, at vi har nået hertil.
[0:05:56] Anders Valentin: Ja, det tror jeg også.
[0:05:58] Martin Collignon (1): Vagen til mad. Godt. Shooter og korengineers, har du forstået Dangerous Nation?
[0:06:03] Martin Collignon (1): Her var mit klare svar. Nej. Men. Problemet er, at det er blandet. Også om noget skal hives ud nu. Det synes jeg ikke er det, der haster.
[0:06:20] Anders Valentin: Og hvad er Retrofit til French Shape?
[0:06:25] Martin Collignon (1): Ja, det er fordi at der ikke er... Altså du har en sæs... Nogle enums og doms.
[0:06:30] Martin Collignon (1): Og derfor passer det mere til en egentlig... Altså de enums kunne være kommet fra en FD. Det franske system.
[0:06:44] Martin Collignon (1): Ja, men det gjorde de. Og problemet var i det danske system har du alt muligt flettet ind. Det er det, der er forskellen.
[0:06:52] Martin Collignon (1): Hvor det kommer mere fra, jamen det er fordi, Frankrig kunne vi ikke have gjort anderledes, fordi det blev givet til os.
[0:07:06] Anders Valentin: Men jeg tror, det er vigtigt, at vi skal passe på med... Jeg tror, vi skal forklare det her med rollerne rigtig godt.
[0:07:17] Anders Valentin: Fordi lige nu, Der tror jeg, at det her vil blive forstået, som de engineers, der er der i dag, præcis Niels, er core engineers.
[0:07:28] Anders Valentin: Og resten er, altså så er du og jeg og Niels, vi er, og Mark kan du sige effektivt også, vi er FDE'er.
[0:07:43] Anders Valentin: Det har vi ikke rigtig kapacitet til i virkeligheden.
[0:07:47] Anders Valentin: Altså vi kan ikke, eller det kan vi godt snakke om, om vi skal ansætte forward-deployed engineers.
[0:08:02] Martin Collignon (1): Det er en rolle, og det er en midlertidig rolle, og ikke en fin titel.
[0:08:08] Anders Valentin: Jeg tror i vores transcript, og i den måde vi taler om det, så er der ligesom, lad os snakke om koden, og ikke om personerne.
[0:08:20] Anders Valentin: Så det er nok bedre at adskille. Should the core platform have to understand Danish regulation? No.
[0:08:31] Anders Valentin: Og det er ligesom det, der kommer udefra. Så vi skal ligesom tegne den der proces op.
[0:08:36] Anders Valentin: Så der ligesom er core engineering, og så er der operational layer, og så er der downstream layer, så man ligesom forstår, at FDE-rollen er en rolle, der spækker de to lag og core-rollen er, at du arbejder på det der lag.
[0:09:05] Martin Collignon (1): Ja, yes.
[0:09:08] Anders Valentin: Fordi det her hører man som roller, der tænker folk deres eget job, og så stopper de med at tænke over, hvad der står.
[0:09:14] Martin Collignon (1): Ja, jeg er enig.
[0:09:18] Martin Collignon (1): Når vi definerer en opgave eller prioritet, at vi faktisk også kan sige om det er en Capture Core Surface.
[0:09:48] Martin Collignon (1): Og at en person kan have... Når er lagt på en opgave der ligger her som en core opgave, eller som en surface opgave, ikke?
[0:09:56] Martin Collignon (1): Men så en anden måde at omfunde et spørgsmål på, det er mere nej. En core koden skal ikke kende til dansk.
[0:10:08] Martin Collignon (1): Det snakkede vi om før, og hvis det er det, så skal den komme ud af det.
[0:10:20] Martin Collignon (1): Om det er en hastesag den gør det, nej. Men at vi skal tilføje mere dansk kode i vores korte ting, nej.
[0:10:29] Martin Collignon (1): Og min næste risik, det hedder jeg, jeg aner ikke Hvor svært eller umuligt det er, at skille ting nedad.
[0:10:43] Anders Valentin: Ja, det er ikke conventionelt. Yes.
[0:10:49] Martin Collignon (1): Godt. How much input on web versus app? Is the app the only way to input data?
[0:10:58] Martin Collignon (1): Jeg tror her, at det handler om, at kun app kan gøre det. eller app. Kun web kan gøre det.
[0:11:07] Martin Collignon (1): Og jeg tror, at svaret er begge dele. Der er en split, som er visit time judgments og alt andet, som i princippet kan være web.
[0:11:22] Martin Collignon (1): Det er måske mere native versus web. Jeg ved ikke, hvad den rigtige ord er.
[0:11:33] Anders Valentin: For mig er et enormt godt eksempel, det er proposals eller texts. Eller tjekke om b-faktorerne er sat rigtigt.
[0:11:43] Anders Valentin: Jeg tror at her, lige præcis den her Q5, gavner enormt meget specificitet.
[0:11:50] Anders Valentin: Og der er ligesom tre kolonner. Skal kunne ske natively på appen, offline. Skal kunne ske i appen, online.
[0:12:00] Anders Valentin: skal kunne ske på begge platforme online, skal kunne ske kun på web online, ikke?
[0:12:22] Anders Valentin: Altså jeg tror, vi skal gøre det så specifikt, det kan vi eventuelt gøre, du og jeg, eller en af os kan tage et stab, og så kan vi reviewe det sammen.
[0:12:29] Anders Valentin: Men jeg tror, det er den specificitet, de har brug for, fordi de simpelthen ikke kan forstå det.
[0:12:47] Martin Collignon (1): Den der bliver ved med at drille os af er EPC'et i virkeligheden og 3D-redigering.
[0:12:58] Martin Collignon (1): Det vil sige, man kan ikke komme væk fra Kelvin og sige, hvad vil Kelvin? Hvor er det hen? Eller Lumens?
[0:13:06] Martin Collignon (1): Og hvis du kigger på alle andre rapporter, der er det i virkeligheden måske meget mere simpelt. Du har en output, den er blevet processed af Core.
[0:13:31] Martin Collignon (1): Og en præsentation til en kunde kan være tekst, kan være et godt eksempel. Et forslag kan være et godt eksempel.
[0:13:54] Martin Collignon (1): Og det der for mig, hvor det er det, men skal du have parity nødvendigvis mellem 3D editing på mobilen og på web, det synes jeg ikke nødvendigvis, der skal være.
[0:14:03] Martin Collignon (1): Altså jeg tror, vi kan godt lave en antagelse, at du godt kan, det er en one way street, men hvis det er forkert opsamlet, så kan du ikke nødvendigvis rette i det.
[0:14:26] Anders Valentin: Nej, det er ikke nødvendigvis skide, men jeg tror, at vi i praksis, så bliver det svært at undgå det. Altså, hvis der sker nogen fejl.
[0:14:38] Martin Collignon (1): Men det skal ikke være et krav. Det skal ikke være en tvungestep, som Kelvin er i dag.
[0:14:45] Anders Valentin: Nej, det er jeg sindssygt enig med dig i.
[0:14:48] Martin Collignon (1): Det vil sige, at vi kan godt have en mode, hvor vi siger, at lad os gøre det, fordi vi kan se, at de skal udstille 3% cases, men det skal blive en exception frem for hovedmålet.
[0:15:01] Martin Collignon (1): Yes. Fuck hudderne.
[0:15:03] Anders Valentin: Enig. Enig.
[0:15:05] Anders Valentin: Og så vil jeg sige, at det andet problem er, at hvis du laver den der room-by-room-model, og det er den, man editerer på, Så skaber det i hvert fald en klar problemstilling i forhold til elektrik og scanner, EPC, fyr og retter.
[0:15:39] Anders Valentin: Fordi så har du ligesom et eller andet flow imellem dem, hvor den skal hentes tilbage til device. Så bliver det i hvert fald ikke one way. Så bliver det two way.
[0:16:00] Martin Collignon (1): Du får RGBD. Altså elektrikeren laver RGBD. Han laver hans...
[0:16:06] Anders Valentin: Ja, hvis de også laver roomplan. Roomplan er det skærende for mig.
[0:16:10] Anders Valentin: Hvis det ikke er den person, der laver epicedet, som laver roomplan, så er du nødt til at sende modellen tilbage igen, fordi så skal de gå ret i den, ikke?
[0:16:21] Martin Collignon (1): Hvis jeg hører, hvad du siger her, så lad os anske to energimærker for at gøre det enkelt.
[0:16:33] Anders Valentin: Nej, ikke et nyt energimærke. Elfyr kommer først på onsite. Han laver også roomplan, så ikke RGBD. Men han skal ikke lave energimærket.
[0:16:45] Anders Valentin: Så kommer byggesavkyndig bagefter, og det er den person, der skal lave energimærket.
[0:16:52] Anders Valentin: Den person vil unægteligt have rettet sig. Det er en basecase, at det kommer til at ske.
[0:17:00] Anders Valentin: Hvordan gør den person det? Kan de åbne det scan efter det er processeret? Eller åbner de det rå scan og retter i det one way?
[0:17:17] Martin Collignon (1): Jeg synes, at et visualiseringslag, som er at vise det færdige model Hvis de gerne vil ændre det, så går du tilbage i rummet, der skal ændres. Og så har du scanner.
[0:17:40] Martin Collignon (1): Og så vil du re-merge og re... Og så skal du køre det hele forfra ved VM1.
[0:17:51] Anders Valentin: Så hvis der skal laves én ændring, så skal du scanne hele huset igen?
[0:17:55] Martin Collignon (1): Ja, ikke scanne nødvendigvis. Du kan jo scanne det i en rum igen. Stadig send Airworld og genbruge alle de andre rum.
[0:18:05] Martin Collignon (1): Men problemet med at hvis du havde ændringer efter Merged, så kan du ikke bruge det.
[0:18:15] Anders Valentin: Ja, eller... Jeg ved ikke om man kan restore den ikke-mergede model.
[0:18:22] Martin Collignon (1): Det kan du ikke. Det kan du godt sende, hvis du relocaliserer.
[0:18:33] Anders Valentin: Det er et teknisk spørgsmål. Men der skal vi træffe en beslutning, der også gør livet nemt for os selv.
[0:18:46] Anders Valentin: Jeg tror, det er en stor interface i forhold til en model, der er scannet af en anden, og så skulle lave alle ændringer.
[0:18:57] Anders Valentin: Der er det nemmere at co-locate de to ting. Men jeg ved ikke hvilken omkostning det så har for flowet.
[0:19:08] Martin Collignon (1): Altså min intuition er, at det lige nu er så få, altså Jeg synes, at elektrikerne kommer til at lave en RGBD og kommer ikke til at lave en roomplan.
[0:19:22] Anders Valentin: Ja, og hvis vi laver det, så tror jeg, at alt bliver meget nemmere.
[0:19:25] Martin Collignon (1): Ja, det tror jeg, at det er den vej, hvor jeg synes, at I giver mening på alle måder.
[0:19:31] Martin Collignon (1): Og så er det lidt mere et spørgsmål, hvordan vi laver en RGBD om til et rum. Men så kan det bare være, at han namer det selv, rummet selv i virkeligheden.
[0:19:43] Anders Valentin: Ja, så skal vi stadig kunne lokalt lave rum, ikke?
[0:19:47] Martin Collignon (1): Ja, vi skulle kunne binde RGBD med Roomplan på en eller anden måde. Men det er faktisk ikke, nej det behøver vi faktisk ikke.
[0:19:54] Martin Collignon (1): Fordi spatial, altså AR, spatial data kommer til at være uafhængig af RGBD. Det kommer bare til at ligge i 3D, Airworld.
[0:20:04] Anders Valentin: Ja, men du vil gerne have placeringerne?
[0:20:10] Martin Collignon (1): Jo, men det er Raycaster, du har ikke brug for det. I teorien, du behøver ikke scanne som elektriker.
[0:20:22] Martin Collignon (1): Du kan rende rundt, åbne AR-kit, tage en annotation, lukke appen, videre i dit liv.
[0:20:28] Martin Collignon (1): Den næste person skal så relocalise i den samme AR-world, og så lave roomplanen efterfølgende.
[0:20:37] Anders Valentin: Er der ikke nogle downstream-ting, vi er afhængige af? Altså rumnavne, etager, spatial graph?
[0:20:47] Martin Collignon (1): Jo, du mener til el?
[0:20:53] Anders Valentin: Altså der er du jo meget dependent på rum og rumtype.
[0:21:01] Martin Collignon (1): Ja, til at være proaktiv i hvert fald, ikke?
[0:21:09] Anders Valentin: Hvis vi skal nå baseline, der er i dag for WhoSweb, så opretter de rumnavne med typer, som affører de spørgsmål, der skal stilles. Det er en baseline-feature?
[0:21:20] Martin Collignon (1): Ja, det kunne vi sagtens, men så er det bare ikke en proactive feature. Så er det bare reactive, men sådan er det. Jeg tror ikke, du kan få begge dele.
[0:21:29] Anders Valentin: Nej, i hvert fald ikke til at starte med. Men vil du så lave det sådan, så det altid er den første person, der er on-site, der laver RGBD?
[0:21:46] Martin Collignon (1): Jeg synes altid, at vi skal have en RGBD, og jeg synes i min optik, at det giver allermest mening, at vi også har en RGBD, når EBC-manden kommer, fordi vi kan ikke vide, om det blev scannet rigtigt.
[0:22:08] Martin Collignon (1): Og det er bare sådan det er, omkostningen er jo på virksomheden, ikke på individet, hvis du står med en af.
[0:22:16] Anders Valentin: Ja, men den er også på individet, sådan som brugeroplevelse, ikke? Men ja, så er det jo mere tilstand, der skal scannen, end el.
[0:22:27] Martin Collignon (1): Jeg synes at forbudtjek specifikt, fordi det er dem specifikt vi snakker om her. Altså de er både NGI og 27 har jo den samme person mange gange.
[0:22:40] Martin Collignon (1): Så er det, at de skal flytte elbesøget senere, hvis det virkelig er der problemer for dem.
[0:22:44] Anders Valentin: Det kan de jo ikke gøre. Det kan de ikke operationelt lade sig gøre, fordi de har 5 elbesøg og 2 af de andre.
[0:22:57] Martin Collignon (1): Nej, det ved jeg godt. Men hvis der er en, der laver tre energimærker og tilstand, og to dagen før, så kan du godt i princippet have de fem elbesøg dagen efter.
[0:23:19] Anders Valentin: Det tror jeg ikke kommer til operationelt at give mening for dem. De skal sørge for kun at hjemme to dage i træk.
[0:23:27] Martin Collignon (1): Og i dag er det sådan, at de gerne vil have den samme person, der gør tingene den samme dag?
[0:23:30] Anders Valentin: Ja, de bestræber sig selvfølgelig på, at sælgeren kun skal være hjemme én dag for at lukke folk.
[0:23:43] Martin Collignon (1): Og hvor mange flere elsager har du over tilstand?
[0:23:59] Anders Valentin: Nej, der er meget færre, der laver el.
[0:24:01] Martin Collignon (1): Men er der færre mennesker, eller er der færre sager?
[0:24:06] Anders Valentin: Der er færre mennesker, altså der er sammenlignet, der er typisk alle tre herhjemme, men elpersonen dækker to konsulenter.
[0:24:16] Martin Collignon (1): Men er der nogle tidspunkter, hvor elbemanden har et hus, hvor der ikke er tilstander?
[0:24:24] Anders Valentin: Det kan der godt være, men det tror jeg er mere sjældent. Det tror jeg er corner case. Jeg tror, at L er jo mere tilkøb, end tilstand er.
[0:24:48] Anders Valentin: Men tilstand er nok den, der har højeste dækningsgrad, fordi den også gælder sommerhuset i nogle tilfælde.
[0:24:54] Martin Collignon (1): Jeg synes, Anders, at der er to ting. For mig er der to problemer. Den ene er, hvordan rationerer du en bolig?
[0:25:12] Martin Collignon (1): Den anden er, at her er der to veje. Det er skal du relocalize, eller skal du reconcile lidt senere hen?
[0:25:17] Martin Collignon (1): Den anden problem er mere det operationelle problem, som du nævner, som er mere, hvad kan vi byde vores, altså her BoCheck specifikt på?
[0:25:35] Martin Collignon (1): Og så er jeg lidt bekymret for, at vi overindekser på en meget teknisk kompliceret, og jeg ved ikke hvor løsbart det er.
[0:25:51] Martin Collignon (1): Så er det mere om det er en kormekage, som vi skal have fundet en løsning på til Butik, eller om det er en virksomhedsting, vi skal have tænkt over produktmæssigt?
[0:26:01] Anders Valentin: Jeg er også i tvivl. Det er ikke fordi, jeg lyder meget mere stolthet på det område, end jeg er.
[0:26:16] Anders Valentin: Men de har sagt, at det er vigtigt for dem, at det er EPC-personen, der laver 3D-scanningen af RoomCane.
[0:26:23] Anders Valentin: Og de har sagt, at på tilstand vil de gerne have 3D-scanning. Jeg ved ikke, om de vil det på ELLE.
[0:26:33] Martin Collignon (1): Godt. Så det eneste spørgsmål, vi har, lad os antage, at vi kunne bare være en virkelig dum og sige, ELLE, det kommer ikke med en 3D-verden. Det kommer bare som en hoodsweb, som det er i dag.
[0:26:48] Martin Collignon (1): Altså, er BoTech okay med, at der ikke er proaktiv rum-input?
[0:26:57] Anders Valentin: Det tror jeg. Jeg tror ikke, det er en særlig stor tidsbesparelse, hvis du laver UX'et godt for at komme frem og tilbage med en rum.
[0:27:14] Anders Valentin: En udfordring, som er, at rummene hedder noget forskelligt på tværl.
[0:27:25] Martin Collignon (1): Der kan du bare tage, hvad den rigtige person er.
[0:27:29] Anders Valentin: Ja, men de har forskellige regler. Jeg tror, at det er okay ikke at have et proaktivt rum.
[0:27:47] Anders Valentin: Fordi der er ikke meget tidsforskel i at oprette rummene.
[0:28:01] Martin Collignon (1): Jeg synes, vi skal vise til Thomas. Jeg synes, Thomas skal tage stilling til, om L skal også indgå i en 3D-rapportvej.
[0:28:09] Anders Valentin: Det tror jeg gerne, han vil have, det gør. Men det er for mig, at der kan du stadigvæk reconcile det senere?
[0:28:19] Martin Collignon (1): Ja, men det du kan sige er, at hvis du skal have et rumnavn, så kan vi sætte det i vores krav, hvis det kræver sig.
[0:28:30] Martin Collignon (1): Og jeg ved godt, at på marken bliver de ked af, at du skal have Runeplan og det andet.
[0:28:39] Martin Collignon (1): Men jeg synes, at reconciliation er to problemer. Vi kan tvinge 11 folk til at lave på Runeplan og AGBD i teorien.
[0:28:50] Martin Collignon (1): Og så bliver vi enige om med, At reconciliation sker på, enten ved lokalisation eller noget, vi merger de to AR-worlds, og det er muligt at gøre det manuelt.
[0:29:13] Anders Valentin: Men det er, altså det kommer til at være et produktproblem for os, et forretningsniveaus problem på et tidspunkt. Dermed ikke sagt, at det skal løses nu.
[0:29:28] Martin Collignon (1): Jeg er ikke uenig, det er bare gør meget meget klart og tydeligt, og jeg ved ikke, hvad det koster og hvor svært det er med de tegningsting.
[0:29:45] Martin Collignon (1): problemer vi snakker om her i virkeligheden, og jeg tror måske Erik kommer til at fortælle os hvad for nogle forskellige problemer det er, men vi skal adskille dem.
[0:29:51] Martin Collignon (1): At der er nogle kommersielle problemer med at tvinge til noget, der er nogle reconciliation problemer, som her er mest drevet af en kortsigtet ønske fra Thomas om at have et 3D-ting.
[0:30:11] Martin Collignon (1): Og vi skal finde ud af, og det er en sequencing problem, hvad vi har brug for at løse nu.
[0:30:28] Martin Collignon (1): Det jeg har været bekymret for, har vi noget, hvor planens minimum-survey ikke er dækket, hvis vi ikke har AGPD? Og det tror vi er blevet enige om, der skal være det.
[0:30:44] Martin Collignon (1): I er stadig bekymrede for en AR World fuck-up, ikke?
[0:30:49] Anders Valentin: Jo. Jeg tror, jeg er mere bekymret for den der kommercielle modskyld der, eller driftstilen.
[0:30:59] Anders Valentin: Men der er i hvert fald en opgave i at tegne de her muligheder op. Altså så vi ved hvad vi vælger imellem.
[0:31:09] Martin Collignon (1): Ja. Så bare så vi igen tager det. Så du har de forskellige muligheder. Du har en elmand.
[0:31:22] Martin Collignon (1): Og han skal i dag, så bruger han et eller andet virkelig dumt ting, hvor han laver ligesom på 27, hvor han skaber rummene.
[0:31:28] Martin Collignon (1): Og ud fra de typer, så bliver der skabt nogle steppers med risiko og så videre. Det kan i princippet være som det er i dag.
[0:31:56] Anders Valentin: Man kan sige i forhold til hvad produktet vil levere, at du har en udfordring i at Hvis du har en person, der laver el-tilsend og energimærker, så får de en underlig oplevelse, hvis der er to forskellige måder at lave el-rapporter på.
[0:32:26] Martin Collignon (1): Du kan have en kommersiel udfordring, at du ikke har det samme på samme 3D-plan.
[0:32:39] Martin Collignon (1): De synes, at der skal være en form for, at både tilsendt el og energimærke skal være det samme sted, og hvor rumnavnet ikke er tilstrækkeligt.
[0:32:52] Anders Valentin: Så har du en udfordring i, at det skal reconciles på et tidspunkt, hvis man skal have downstream-artefakter, hvor de lever det samme sted?
[0:33:02] Martin Collignon (1): Og så har du løsninger, du kan tvinge el til at lave RGBD, du kan tvinge el til at lave Roomplan, du kan få elmanden til at tage signalisation i en AR World.
[0:33:21] Martin Collignon (1): Og så har du på den anden side i forhold til energimærker og tilstand, der kan du få vedkommende til at relocalize ind i den samme AR world for at kunne fortsætte sit arbejde.
[0:33:40] Martin Collignon (1): Og så er der i sidste ende også mulighed for manuel reconcile modellerne. Det vil sige at en 3D-plan sørger for at de er lige rigtigt i stedet.
[0:33:53] Martin Collignon (1): Hvor ingen af dem er, synes jeg, clean, fordi du har et problem med der ikke er 100% træfsikkerhed på noget af det i virkeligheden.
[0:34:02] Anders Valentin: Ja, og så er det hvilken type træfsikkerhed, hvor vil du helst tage usikkerheden henne?
[0:34:07] Martin Collignon (1): Ja, hvis vi skal forsvare RGBD og RunePlan også for el-dugten, det er vel at du får ud over det proaktive ting, så du kan insarter tingene, så er det også at du kan lave det der AI-localization ting, ikke?
[0:34:29] Martin Collignon (1): Det kan du ikke gøre, hvis du ikke har typologi.
[0:34:38] Martin Collignon (1): Men okay, der er noget, der skal tænkes her, og noget, der skal diskuteres. Der er helt sikkert noget i forhold til omkostning, træfsikkerhed, og kondition.
[0:34:50] Anders Valentin: Hvis vi vælger, at der er noget som helst, der skal arves imellem, så har du også sådan noget med, hvad for en er vindende, hvad for en redigering tæller, hvordan deles de mellem serveresne. Det kræver en internetforvendelse.
[0:35:33] Martin Collignon (1): Ja, det er vi enige i. Så er der noget, der skal tænkes her. Vi skal kigge i scenariet. Hvor ofte sker de?
[0:35:45] Martin Collignon (1): Jeg synes, det er værd at tænke over, hvor meget det er en botjek-problem versus andre problemer.
[0:35:49] Martin Collignon (1): Så basic er der en helt dum løsning til botjek, som ikke skal skalere, men som bare virker, hvor vi har sat af hus-web.
[0:36:09] Martin Collignon (1): Det måske vi kunne blive enige om her, Anders, rent teknisk, så vi ikke skræmmer folk, det er, at jeg tror ikke, at vi skal forsøge at lave multiplayer samtidig.
[0:36:18] Anders Valentin: Nej, jeg er også meget bekymret for tidslinjen i det her. Med den tidscene vi har, der lugter det mere af at lave dum husweb version 1.
[0:36:32] Martin Collignon (1): Ja, eller dum begge scanner. Det er også dumt.
[0:36:37] Anders Valentin: Den tror jeg er mindre på.
[0:36:42] Martin Collignon (1): Men det er en kommerciel overvejelse mere end en teknisk overvejelse, korrekt?
[0:36:46] Anders Valentin: Det hele er ikke så meget teknisk. Jeg tror ikke på den value proposition mæssigt.
[0:36:53] Anders Valentin: Jeg tror ikke, du kan bruge L til at bruge dobbelt så lang tid, som det gør i dag for very small gains.
[0:37:04] Martin Collignon (1): Nej, det modtager jeg.
[0:37:15] Martin Collignon (1): Den løsning der også er, det er RGBD per rum. Men så kan vi så godt lave en rumplan per rum.
[0:37:22] Anders Valentin: Ja, det tror jeg ikke, du vinder meget ved. Så er der måske et eller andet, hvor når du tager annoteringer af hvad du adskiller, så laver den et eller andet scan, og det kan vi bruge til at reconcile det senere mod RGBD'en.
[0:37:39] Anders Valentin: Altså RKBD'en, du kan jo lave det der image correspondence i virkeligheden. Jeg tror de er ret gode de der modeller efterhånden.
[0:37:52] Martin Collignon (1): Jamen det er ikke så meget, det er mere... Du skal forsøge at matche en åben dåse med en dåse der ikke er åbnet.
[0:38:01] Anders Valentin: Jamen det bliver jo ikke dåsen der bliver afgørende. Altså det bliver ikke dåsens datapunkter der er afgørende.
[0:38:09] Martin Collignon (1): Jeg vil sige, at det jeg ikke synes haster, det er Reconciliation, nødvendigvis.
[0:38:17] Anders Valentin: Ja. Det er jeg enig med dig i.
[0:38:22] Martin Collignon (1): For mig er det mere spørgsmålet, hvor meget. Hvis vi skal have den hurtige tekniske ting, vil jeg skyde på, at enten fuldscan... Det er nok fuldscan eller ingen scan for elvirkeligheden.
[0:38:38] Anders Valentin: Ja. Ja, men hvad så i tilfældet af, hvor det er den anden person, der kommer først? Vil du så have full scan?
[0:38:52] Anders Valentin: Er det et hierarki, eller er det et tidshierarki? Altså er det, hvis der er L, så er det L. Hvis der er tilstand, så er det tilstand. Hvis der ikke er nogen af de to, så er det EPC. Eller er det, hvem kommer først og laver scan?
[0:39:09] Martin Collignon (1): Altså min idé, det jeg tænker her er problemet, det er at vi bliver rigtig meget blandet ind i deres egen cm og data. Og det kan jeg ikke lide.
[0:39:25] Anders Valentin: Jeg tror det er svært at undgå.
[0:39:28] Martin Collignon (1): Der er forskel med at have det og så relye på det.
[0:39:32] Anders Valentin: Ja, men jeg tror det er svært at undgå, at vi skal forholde os til produkterne.
[0:39:44] Anders Valentin: Hvis du gerne vil have Roomplan, så skal du vide, om der kommer en EPC senere, ikke?
[0:39:54] Martin Collignon (1): Jamen, jeg synes ikke, Roomplan er nødvendigt. RGBD synes jeg er bedre end Roomplan, hvis jeg skal vælge mellem to ting.
[0:39:59] Anders Valentin: Men vi skal have Roomplan for at lave EPC.
[0:40:02] Martin Collignon (1): Jamen så kan vi bare lave RunePlan over RGBD på APC og tilstand.
[0:40:10] Anders Valentin: Ja, men du skal jo vide om den APC person skal lave RGBD eller ej.
[0:40:16] Martin Collignon (1): Men jeg tror altid at tilstand og Energimax skal lave RGBD over RunePlan.
[0:40:26] Anders Valentin: Det tror jeg ikke de kommer til at acceptere. Altså det der medscan, det kommer til at tage et kvarter.
[0:40:36] Anders Valentin: Altså så skal du tænke på medieringstiden lige nu er 40 minutter, ikke?
[0:40:40] Martin Collignon (1): Jamen i så fald, så tror jeg ikke de skal, så synes jeg det er alle der ikke skal gøre noget.
[0:40:45] Martin Collignon (1): Vi skal enten sige, der hvor LGBT er vigtigste, det er for tilstand. Så det er enten, at L laver ikke noget, og så må vi stole på, at ARKit virker.
[0:41:10] Anders Valentin: Men hvis der kun laves L på en ejendom, vil du så ikke stadig gerne have RKBD?
[0:41:18] Martin Collignon (1): Jo jo, men jeg ville tro at det er nemmere ikke at forholde sig til deres interne CMSystem, fordi det tror jeg simpelthen ikke vi kan stå på.
[0:41:30] Martin Collignon (1): Det er principielt om, hvor meget vi skal være enige i deres operations. Faktisk mere end det handler om det.
[0:41:45] Anders Valentin: Vi er totalt alene på, at vi skal undgå det så meget som muligt. Det er jeg helt enig med dig i.
[0:41:53] Anders Valentin: Jeg tror mere på, hvis du laver RGBD, og du lægger den på tilstanden.
[0:42:05] Anders Valentin: Vi skal finde ud af, hvor mange steder, der bliver lavet elinstallation, hvor der ikke laves tilstand.
[0:42:14] Martin Collignon (1): Ja, det kan jeg bare skrive til din navne og spørge om.
[0:42:18] Anders Valentin: Hvis det tal er lavt, og det tror jeg det er, så vil det være nemmest at bare lægge den på tilstandsrapporten, og Roomplanen har EPC rapporten, og så EL, de har ikke noget smart, og hvis vi kan matche det bagefter på billeder, så vil vi få en cirka correspondence, og det giver et rum.
[0:42:46] Martin Collignon (1): Jamen, jeg tror, så længe vi har rumnavn, så kan vi matche det.
[0:42:50] Anders Valentin: Jamen, jeg tror ikke, at rumnavn bliver ekstabilt.
[0:42:53] Martin Collignon (1): Nej, ikke på det rinke. Hvis du har AR-kit... Noget vi kunne gøre det på, det er, at vi tvinger dem til at tage et billede ved indgang af, altså rumhejtsbilledet.
[0:43:12] Anders Valentin: Men det, der er lagt op til for dem, det er jo også, at de skal tage billedet langt nok fra, så du kan se, hvad det er.
[0:43:25] Anders Valentin: Det kan jeg vibecode rimelig hurtigt på. Præcisionen af den der matching er jo ikke nede på 20 centimeter.
[0:43:41] Anders Valentin: EL har forskellen, at det er en helt specifik installation, så du kan se, hvor den er henne.
[0:43:51] Anders Valentin: Og til den der virtuelle rapport, der skal de jo ikke tage et nærbillede. De skal tage et fjernbillede.
[0:44:11] Anders Valentin: Og jeg ved godt, at der vil være tilfælde, hvor den ikke kan matches på billeder. Men hvis du har taget en RGBD-scanning, så har du i hvert fald 10 frames per second.
[0:44:29] Anders Valentin: Så tror jeg altså også, at de der correspondence vil være i hvert fald rumpræcise. Og det er meget nemmere at starte uden.
[0:44:41] Anders Valentin: Udskyde Reconciliation af el. Fordi det andet, det bliver lidt inden eller, tror jeg. Operationelt.
[0:44:52] Anders Valentin: Altså jeg tror ikke, der er nogen verden, hvor de lader det tage dobbelt så lang tid. Og 30% længere tid.
[0:44:53] Martin Collignon (1): Altså det er urealistisk.
[0:44:55] Anders Valentin: Og så må vi tabe de der el-sager på gulvet. Hvis det kun er Bochek, og det kun er Danmark, hvor vi taber el-sager. Who cares? Det er 3% af sagerne.
[0:45:12] Martin Collignon (1): Især hvis det ikke er muligt. Altså hvis det ikke kan lade sig gøre, at de scanner. Så er det sådan, det er jo.
[0:45:18] Anders Valentin: Det må vi tjekke senere, men jeg tror bare, det er et problem, vi kan løse senere i hvert fald.
[0:45:30] Anders Valentin: Og så vil jeg samtidig sige, at du får selvfølgelig ikke special beskrivelse, men du skal også tænke på, hvis vi har noget automatisering af teksterne med et eller andet special.
[0:45:40] Anders Valentin: Altså så dur det jo heller ikke, at elmanden skal vente på, at EPC rapporten er færdig, for at de kan få roomplan, for at de kan få specialmodeller, for at de kan få automatiske beskrivelser.
[0:45:57] Martin Collignon (1): Jeg synes, at hvert service skal være uafhængig af hinanden så meget som muligt, og hvis der skal være nogen magic, så kan det enten være en relocalisation.
[0:46:23] Martin Collignon (1): Det er irriterende, hvis du ikke kan planlægge, om du skal scanne i 3D eller ej.
[0:46:33] Anders Valentin: Men det kræver online, at du ved, om der er en model. Uanset hvad, så skal du downloade den.
[0:46:40] Martin Collignon (1): Ja, din award. Derfor har jeg det bedst med, at det er dumt, og reconciliation sker efterfølgende. Og så vi finder en måde at gøre det på.
[0:46:50] Anders Valentin: Det er et fint V1-oplæg, at vi siger, at vi ikke reconcilerer dem her til start med, at vi ikke laver noget smart.
[0:46:59] Anders Valentin: Vi kan godt lave noget med billedbeskrivelse på Gemini Flash, men jeg tror, det giver den der flavorbeskrivelse, men det giver ikke placeringen.
[0:47:10] Anders Valentin: Det, der var vigtigst for dem, var, at den skrev det over radiatoren ved siden af Loll i stuen.
[0:47:24] Martin Collignon (1): Og jeg synes, vi skal vise de to muligheder. Enten skal I scanne, eller så skal I ikke scanne. Og så er det det, det koster.
[0:47:43] Martin Collignon (1): Q6. How much do we accept to do manually her?
[0:47:50] Anders Valentin: Jeg synes vi skal accepte 100%. Så den kunne jeg godt tænke mig, at du uddøbelte.
[0:48:13] Martin Collignon (1): Vi skal have et tal på, hvad der er acceptabelt af at være manuelt vs. ikke manuelt.
[0:48:19] Anders Valentin: Det skal også være pinerneagtigt.
[0:48:21] Martin Collignon (1): Ja, præcis. Og jeg har lidt en idé om, at vi skal acceptere 5% af sagerne, som det er lige nu.
[0:48:32] Martin Collignon (1): Og så har vi et mål, og så er det mere en kommersiel spørgsmål, hvordan vi prissætter det.
[0:48:38] Martin Collignon (1): Jeg har lidt en idé om, at vi skal sælge det 5%. Og så kan du enten bruge vores advance ting til at rette det manuelt, eller så kan du bruge penge, og så er det to dage, og så koster det lidt ekstra.
[0:48:57] Anders Valentin: Ja. Det er jeg enig i. Jeg tror ikke, der er nogen andre måder at gøre det godt på, end at det bliver betalt.
[0:49:12] Martin Collignon (1): Og det er okay, at det koster en 50'er eller et eller andet.
[0:49:18] Anders Valentin: Men det vi skal være opmærksomme på herinde, det er i forhold til prissætningen, det er at de sager der er tilbage, er også de sager der tager lang tid.
[0:49:36] Anders Valentin: Så når vi prissætter det, så skal vi tage de fem værste procent og prissætte dem.
[0:49:46] Martin Collignon (1): Jeg tror det er derfor at sige, at to dages ventetid gør meget mere smertefulde end 50 kroner.
[0:49:54] Anders Valentin: En dag er også okay, hvis det er en periode. Nul dage er umuligt. Jeg tror ikke, at en dag gør så meget.
[0:50:09] Martin Collignon (1): Derfor jeg er enig med dig, er at Der er faktisk rigtig høj plads. Det vil sige at sige 300 kroner for en dag og 50 kroner for to dage er en god forretning for begge parter.
[0:50:22] Martin Collignon (1): Hvis det er 2% af sagerne, så er det hvor mange sager om året? Det er 400 for dem. Så det er to om dagen. Så vi skal have én person ansat til det i Filippinerne. Så halvanden. Det er ikke dyrt.
[0:50:47] Martin Collignon (1): Og det er meget dyrt for dem, at have en mest forhåbentlig ansat i Danmark. Det er 100.000 om måneden, ikke?
[0:51:00] Martin Collignon (1): Så jeg synes, det kunne godt være noget, vi præsenterer til Thomas og siger, at det er det, vi er, og vi tager risikoen her.
[0:51:06] Anders Valentin: Ja, men jeg tror, at den forhandling der, der skal vi sige, Vi bliver nødt til at være fleksible på de der SLA, og det her er et bud vi kan commit til et halvt år.
[0:51:15] Anders Valentin: Og så må vi analysere det derefter. Fordi det bliver enormt meget et moving target det der.
[0:51:29] Anders Valentin: Altså der kommer sådan en selvselektion i det.
[0:51:34] Martin Collignon (1): Når de har travlt, vil de mere sandsynligt gøre det for at spare tid.
[0:51:42] Martin Collignon (1): Ja, ja. Og så kan det være, jeg tror 300 kroner, og så er det spørgsmålet, hvem betaler?
[0:51:53] Anders Valentin: Men vi har jo ikke noget problem med det, når det er 5% af sagerne.
[0:51:57] Martin Collignon (1): Nej, men det vil sige, det er også et risiko, vi tager. Så jeg skal lige igen lukke min computer, for den har igen gået i sort. Jeg skal til Odense i morgen.
[0:52:08] Martin Collignon: Martin Collignon left
[0:52:08] Martin Collignon: Martin Collignon stopped sharing their screen
[0:53:04] Martin Collignon: Martin Collignon joined
[0:53:06] Anders Valentin: To sekunder. To sekunder.
[0:53:17] Martin Collignon (2): er det vel tid til en backup.
[0:53:22] Martin Collignon (2): Det er okay, når det sker, så kan man altid bruge en større skærm og fikse det.
[0:54:08] Anders Valentin: Jeg laver lige et chart, for jeg vil gerne se, hvor meget af tiden, der bliver brugt i hvilke buckets, Hvis man inddeler det i procentiler, hvad er summen af altid i det sværeste procentil?
[0:54:28] Anders Valentin: Fordi jeg tror, at den er ret fordelt over mod de svære sager.
[0:54:34] Martin Collignon (2): Det ved jeg 100%. Det var de sager, jeg ikke kunne løse algoritmisk. Så det er de sager, hvor der er tre Geber rules osv.
[0:54:48] Martin Collignon (2): Det er en til en fuck-fuck. Så du får de mest irriterende sager.
[0:54:57] Martin Collignon (2): Det er basically, hvor lang tid bruger I på de her usindssyge sager?
[0:55:01] Anders Valentin: Det er mere, hvis vi automatiserer de 80% af sagerne, som står for 20% af tiden, så er det ligegyldigt, at det kun er 20% af sagerne, vi skal lave behandling på, fordi 80% af vores tid ligger stadig på de 20% af sagerne.
[0:55:17] Martin Collignon (2): Men jeg tror ikke det... Altså, mit bud er, at det ikke er sådan, det bliver.
[0:55:26] Martin Collignon (2): Det er ikke fordi, at automatisering på de andre sager ikke også påvirker de dårlige sager.
[0:55:33] Martin Collignon (2): Altså, det er mere områdesspecifikt. Der er et rum, der er fucked. Eller et roof, der er fucked. Men de andre ting, vi har stadig lukket af.
[0:55:51] Martin Collignon (2): Godt, jeg har så ikke HTML'en mere, så det er perfekt. It's back!
[0:56:13] Martin Collignon (2): Jeg tror vi er for sikre hos Flexbuys, så let's see how it goes. Jeg hopper lige tilbage.
[0:56:30] Martin Collignon: Martin Collignon left
[0:56:30] Martin Collignon: Martin Collignon joined
[0:56:35] Anders Valentin: Altså, den her har vi også talt meget om. Det næste spørgsmål. Det er det vi er meget enige om.
[0:56:44] Martin Collignon (3): det tror jeg også vi er, jeg skal bare lige have noget tab window. Mere soft, fordi det skal jo være udtrykkeligt at sige højt til AI.
[0:56:51] Martin Collignon: Martin Collignon started sharing their screen
[0:57:02] Martin Collignon (3): det er den vi lige har drøftet i virkeligheden
[0:57:09] Anders Valentin: Det betyder også, at vi er enige om, at det faktisk er en del af tilstandsrapportens deliverable at lave et RGBD-scan?
[0:57:18] Martin Collignon (3): Ja, altså jeg vil mene, at det er også et tilfælde for alle, men det er så der, hvor jeg tror, at jeg er villig til at sige, at hvis du samler alle de trade-offs, der skal laves, så er det bare nemmere at lave den enten fuld smart eller fuld dum, og mellemvejen er ikke så god.
[0:57:35] Martin Collignon (3): Og fuldsmart er rigtig dyrt, fordi der er rigtig mange ting, der skal hænge sammen. Og mange bevægelige dele, som vi ikke helt ved, om fungerer 100%.
[0:57:47] Martin Collignon (3): Og fulddom kan eksekveres, og det er egentlig ikke der, hvor de har brug for mest smartness.
[0:57:54] Anders Valentin: Ja, og så er der også en anden sag, som vi ikke lige havde talt om, som er de her fornyelser. Som er en ret stor del af rapportmængden.
[0:58:13] Anders Valentin: De holder jo kun et halvt år.
[0:58:17] Martin Collignon (3): Ja, og der hører jeg selv internt, der er nogen, der kører drive-by, der er nogen, der ikke gør.
[0:58:20] Anders Valentin: Jo, altså der er helt sikkert forskel i, hvordan det bliver gjort det der.
[0:58:31] Anders Valentin: Men du får en udfordring i, at hvis du skal tilføje en note, så skal vi lave et interface til det. Det er nemmere at slette, det er okay. Men hvis du skal tilføre en, hvad så?
[0:58:49] Martin Collignon (3): Så må du lave et ikke speciale ting. En anden måde at gøre det på kunne være, at vi laver en 3D-placering af en note.
[0:59:07] Anders Valentin: Det er svært at lave. Altså svært UX-mæssigt. At placere en note.
[0:59:16] Martin Collignon (3): Jeg siger ikke, at det er nemt. Men jeg forstår, at det er svært at sætte noget i en åben luft.
[0:59:25] Martin Collignon (3): Men så må man vel lave i to dele. Du laver opfra til at starte noget. Så må du justere op og ned i højde.
[0:59:41] Martin Collignon (3): Altså relocalization kan jo sagtens gøre os efterfølgende. Fordi det er også at slætte noter, ikke?
[0:59:48] Martin Collignon (3): Slætte noter er fint.
[0:59:49] Anders Valentin: Altså det er jo bare at slætte dem.
[0:59:51] Martin Collignon (3): Og selvom de laver drivebys, så i teorien skal du også... Jeg tænkte faktisk, at du var mere på et sted som... De ville ikke kunne bevise, at de havde været forbi.
[1:00:06] Anders Valentin: Det tænker jeg ikke så meget på.
[1:00:10] Martin Collignon (3): Ja, men det er fucking weird. Du kan sagtens bruge en 3-model. Det kan du så ikke for din revision.
[1:00:19] Anders Valentin: Altså, det er klart, at vi kan ikke understøtte, at de har lavet en selskabsrapport uden spatial, og så laver et revisit. Det er bare en sunkost. Det skal vi ikke understøtte.
[1:00:42] Martin Collignon (3): Når du laver en ny tilstand, så får du et nyt forsættningstilbud i princippet?
[1:00:49] Anders Valentin: Ja.
[1:00:52] Martin Collignon (3): De kan ikke bruge det gamle model som bevis til det nye? Vi kan ikke være garant for det i hvert fald?
[1:01:03] Anders Valentin: Nej, fordi hvis de ikke har lavet et scan, så kan de ikke dokumentere, at sådan her så det ud, den dag jeg var der.
[1:01:12] Martin Collignon (3): Jamen, det kan vi ikke komme ud om. Vi skal bare lige gøre det.
[1:01:17] Martin Collignon (3): Jeg tror ikke altid, at de tænker så langt, at de tænker, hey, giv det drive-by, der før tog et kvarter. Giv enten tre kvarter, eller så har de ikke mere.
[1:01:32] Martin Collignon (3): Altså vi er nødt til at præcisere hvornår tingene kom fra hvornår. Og du kan selvfølgelig genbekræfte at noget ser sådan ud.
[1:01:46] Anders Valentin: Vi lavede noget... Nu skal jeg lige åbne Figma, det tager lige 14 år.
[1:01:57] Anders Valentin: Anders Valentin started sharing your screen
[1:01:57] Anders Valentin: Anders Valentin stopped sharing your screen
[1:03:03] Anders Valentin: var det sådan her, det var tænkt. Og det er ligesom bare yes, verify, verify, verify.
[1:03:22] Anders Valentin: Jeg ved ikke hvad forskellen er her på det lovlige og det praktiske faktisk.
[1:03:25] Martin Collignon (3): Det var det største spørgsmål for mig.
[1:03:30] Martin Collignon (1): Ja, det ved jeg heller ikke.
[1:03:33] Anders Valentin: Anders Valentin stopped sharing your screen
[1:03:41] Martin Collignon (3): Jeg føler, at det ikke er helt godt i mit hoved, at vi så tydeligt muliggør en proces, hvor de skal starte forfra. Det er nok lovmæssigt.
[1:04:09] Martin Collignon (3): Jeg tror, det er faktisk nogle spørgsmål, man skal stille til Thomas. Eller til Lukas, faktisk.
[1:04:14] Anders Valentin: Ja. Det tror jeg, du har ret i.
[1:04:18] Martin Collignon (3): Vi står og tænker over det her, hvad jeres tanker er.
[1:04:23] Martin Collignon (3): Fordi der er noget kommersielt i forhold til, at de altid skal kunne vise sin 3D-model. Hvis Porsche altid gerne vil have en 3D-model til at lave deres priser, så er de nødt til at have det nyeste model.
[1:04:35] Anders Valentin: Det kan også godt være, at vi kan gøre det på den gamle. Det forsvarer dem bare juridisk. Men det har mere kommercielt hensyn.
[1:04:47] Anders Valentin: Det den primært gør overfor Forsja, det er jo noget med at beskytte budget. Det er jo så de kan sige, du kan ikke klage over den her fejl til os.
[1:04:58] Martin Collignon (3): Jeg tror vi skal snakke med Lukas, fordi der er også noget. Hvad er en genbesøg? Jeg tror ikke det er noget, der officielt findes. Inden for seks måneder, ja. Måske.
[1:05:09] Martin Collignon (3): Men efter seks måneder, fordi hun er stadig til salg, det tror jeg ikke er noget, der findes. Så er det bare et nyt tilsætningsrapport.
[1:05:17] Anders Valentin: Ja, efter seks måneder tror jeg også. Men inden for seks måneder, det... Men jeg ved det ikke.
[1:05:27] Martin Collignon (3): det lyder underligt, at du kan...
[1:05:28] Martin Collignon (1): Det lyder usandsynligt, at de får lov
[1:05:30] Anders Valentin: at gøre det, hvis det er ulovligt.
[1:05:34] Martin Collignon (3): Ja ja, og man kan sige det er deres ansvar, det er jo konsulentens, altså bygsafkøbningens ansvar.
[1:05:44] Martin Collignon (3): Jeg kunne godt se en Gerd Lomholdt sidde i bilen og trykke ja mange gange. Mens han spiser en smørbrød.
[1:06:05] Martin Collignon (3): Okay, godt. Jeg synes, vi er kommet igennem i hvert fald de spørgsmål, som jeg havde fundet, vi skulle svare på.
[1:06:12] Anders Valentin: Skal vi ikke igennem dem, som management stillede til os?
[1:06:17] Martin Collignon (3): Jamen skal jeg ikke, i stedet for at vi igennemgår det derfra, skal jeg ikke bare på vores transcript give et svar på dem, og så give dem til review til dig?
[1:06:30] Anders Valentin: Jo, det er også fint. Lad os gøre det.
[1:06:32] Martin Collignon (3): Yes, og så har jeg sådan nogle af de andre dokumenter jeg delte var mere i forhold til hvad vi synes vi skal diskutere, hvornår vi skal diskutere osv.
[1:06:38] Martin Collignon (3): Og jeg synes jeg kunne godt lide især den der hed Engineering Decision Router.
[1:06:49] Anders Valentin: Hvad indgælder du til et møde omkring den? Så skal jeg nok have læst den.
[1:06:54] Martin Collignon (3): Ja, det er mere The Matrix, den her var ret god. Og vi skal ikke snakke om det nu, men jeg indkæder dig til et møde senere på ugen omkring det.
[1:07:08] Martin Collignon (3): Allright, jamen god bedring, velsign.
[1:07:13] Anders Valentin: Ja, jeg slapper lidt af. Vi ses nu. Vi ses, hej.
[1:07:20] Anders Valentin: Anders Valentin left
