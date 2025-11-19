<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "beaeca2ae0ec007783e6310a3b63291f",
  "translation_date": "2025-10-11T11:18:45+00:00",
  "source_file": "changelog.md",
  "language_code": "et"
}
-->
# Muudatuste logi: MCP algajate õppekava

See dokument on aruanne kõigist olulistest muudatustest, mis on tehtud Model Context Protocol (MCP) algajate õppekavas. Muudatused on dokumenteeritud pööratud kronoloogilises järjekorras (uusimad muudatused esimesena).

## 6. oktoober 2025

### Algajate sektsiooni laiendamine – Täiustatud serveri kasutamine ja lihtne autentimine

#### Täiustatud serveri kasutamine (03-GettingStarted/10-advanced)
- **Lisatud uus peatükk**: Toodud põhjalik juhend MCP serveri täiustatud kasutamise kohta, hõlmates nii tavalisi kui ka madala taseme serveri arhitektuure.
  - **Tavaline vs. madala taseme server**: Üksikasjalik võrdlus ja koodinäited Pythonis ja TypeScriptis mõlema lähenemisviisi kohta.
  - **Handler-põhine disain**: Selgitus handler-põhise tööriista/ressursi/küsimuste haldamise kohta skaleeritavate ja paindlike serveri rakenduste jaoks.
  - **Praktilised mustrid**: Reaalsed stsenaariumid, kus madala taseme serveri mustrid on kasulikud täiustatud funktsioonide ja arhitektuuri jaoks.

#### Lihtne autentimine (03-GettingStarted/11-simple-auth)
- **Lisatud uus peatükk**: Samm-sammuline juhend lihtsa autentimise rakendamiseks MCP serverites.
  - **Autentimise kontseptsioonid**: Selge selgitus autentimise ja autoriseerimise ning mandaadi haldamise kohta.
  - **Lihtsa autentimise rakendamine**: Middleware-põhised autentimismustrid Pythonis (Starlette) ja TypeScriptis (Express), koos koodinäidetega.
  - **Edasiminek täiustatud turvalisuseni**: Juhised lihtsa autentimise alustamiseks ja edasiliikumiseks OAuth 2.1 ja RBAC-i juurde, viidates täiustatud turvamoodulitele.

Need lisandused pakuvad praktilisi ja käed-külge juhiseid MCP serverite ehitamiseks, mis on vastupidavamad, turvalisemad ja paindlikumad, ühendades põhimõisted täiustatud tootmismustritega.

## 29. september 2025

### MCP serveri andmebaasi integreerimise laborid – põhjalik praktiline õppeprogramm

#### 11-MCPServerHandsOnLabs – uus täielik andmebaasi integreerimise õppekava
- **Täielik 13-labori õppeprogramm**: Lisatud põhjalik praktiline õppekava tootmisvalmis MCP serverite ehitamiseks PostgreSQL andmebaasi integreerimisega.
  - **Reaalse maailma rakendus**: Zava Retaili analüütika kasutusjuhtum, mis demonstreerib ettevõtte tasemel mustreid.
  - **Struktureeritud õppeprogressioon**:
    - **Laborid 00-03: Alused** - Sissejuhatus, põhistruktuur, turvalisus ja mitme rentniku tugi, keskkonna seadistamine.
    - **Laborid 04-06: MCP serveri ehitamine** - Andmebaasi disain ja skeem, MCP serveri rakendamine, tööriistade arendamine.
    - **Laborid 07-09: Täiustatud funktsioonid** - Semantiline otsing, testimine ja silumine, VS Code integratsioon.
    - **Laborid 10-12: Tootmine ja parimad praktikad** - Juurutamisstrateegiad, jälgimine ja nähtavus, parimad praktikad ja optimeerimine.
  - **Ettevõtte tehnoloogiad**: FastMCP raamistik, PostgreSQL koos pgvectoriga, Azure OpenAI embeddings, Azure Container Apps, Application Insights.
  - **Täiustatud funktsioonid**: Rida taseme turvalisus (RLS), semantiline otsing, mitme rentniku andmete juurdepääs, vektorite embeddings, reaalajas jälgimine.

#### Terminoloogia standardiseerimine – moodulite muutmine laboriteks
- **Põhjalik dokumentatsiooni uuendus**: Süsteemselt uuendatud kõik README failid 11-MCPServerHandsOnLabs kaustas, et kasutada "Labori" terminoloogiat "Mooduli" asemel.
  - **Sektsiooni pealkirjad**: Uuendatud "Mida see moodul hõlmab" "Mida see labor hõlmab" vastu kõigis 13 laboris.
  - **Sisu kirjeldus**: Muudetud "See moodul pakub..." "See labor pakub..." vastu kogu dokumentatsioonis.
  - **Õpieesmärgid**: Uuendatud "Selle mooduli lõpuks..." "Selle labori lõpuks..." vastu.
  - **Navigeerimislingid**: Kõik "Moodul XX:" viited muudetud "Labor XX:" vastu ristviidetes ja navigeerimises.
  - **Lõpetamise jälgimine**: Uuendatud "Pärast selle mooduli lõpetamist..." "Pärast selle labori lõpetamist..." vastu.
  - **Tehnilised viited säilitatud**: Säilitatud Python mooduli viited konfiguratsioonifailides (nt `"module": "mcp_server.main"`).

#### Õppejuhendi täiustamine (study_guide.md)
- **Visuaalne õppekava kaart**: Lisatud uus "11. Andmebaasi integreerimise laborid" sektsioon koos põhjaliku laboristruktuuri visualiseerimisega.
- **Repo struktuur**: Uuendatud kümnest üheteistkümneks põhiosaks koos üksikasjaliku 11-MCPServerHandsOnLabs kirjeldusega.
- **Õppeprogrammi juhised**: Täiustatud navigeerimisjuhised, et hõlmata sektsioone 00-11.
- **Tehnoloogia katvus**: Lisatud FastMCP, PostgreSQL, Azure teenuste integreerimise üksikasjad.
- **Õpitulemused**: Rõhutatud tootmisvalmis serverite arendamist, andmebaasi integreerimise mustreid ja ettevõtte turvalisust.

#### Peamise README struktuuri täiustamine
- **Laboripõhine terminoloogia**: Uuendatud peamine README.md 11-MCPServerHandsOnLabs kaustas, et järjekindlalt kasutada "Labori" struktuuri.
- **Õppeprogrammi organisatsioon**: Selge progressioon aluskontseptsioonidest täiustatud rakendamiseni ja tootmise juurutamiseni.
- **Reaalse maailma fookus**: Rõhk praktilisel, käed-külge õppimisel ettevõtte tasemel mustrite ja tehnoloogiatega.

### Dokumentatsiooni kvaliteedi ja järjepidevuse parandamine
- **Praktilise õppimise rõhutamine**: Tugevdatud praktilist, laboripõhist lähenemist kogu dokumentatsioonis.
- **Ettevõtte mustrite fookus**: Tõstetud esile tootmisvalmis rakendused ja ettevõtte turvalisuse kaalutlused.
- **Tehnoloogia integreerimine**: Kaasaegsete Azure teenuste ja AI integreerimise mustrite põhjalik katvus.
- **Õppeprogressioon**: Selge, struktureeritud tee aluskontseptsioonidest tootmise juurutamiseni.

## 26. september 2025

### Juhtumiuuringute täiustamine – GitHub MCP registri integreerimine

#### Juhtumiuuringud (09-CaseStudy/) – Fookus ökosüsteemi arendamisel
- **README.md**: Suur laiendus koos põhjaliku GitHub MCP registri juhtumiuuringuga.
  - **GitHub MCP registri juhtumiuuring**: Uus põhjalik juhtumiuuring, mis analüüsib GitHubi MCP registri käivitamist 2025. aasta septembris.
    - **Probleemi analüüs**: Üksikasjalik analüüs killustatud MCP serveri avastamise ja juurutamise väljakutsetest.
    - **Lahenduse arhitektuur**: GitHubi tsentraliseeritud registri lähenemine ühe klõpsuga VS Code installatsiooniga.
    - **Ärimõju**: Mõõdetavad parandused arendajate sisseelamises ja produktiivsuses.
    - **Strateegiline väärtus**: Fookus modulaarsete agentide juurutamisel ja tööriistadevahelisel koostöövõimel.
    - **Ökosüsteemi arendamine**: Positsioneerimine agentide integreerimise platvormi alusena.
  - **Täiustatud juhtumiuuringu struktuur**: Uuendatud kõik seitse juhtumiuuringut järjekindla vormingu ja põhjalike kirjeldustega.
    - Azure AI reisibürood: Mitme agendi orkestreerimise rõhutamine.
    - Azure DevOps integratsioon: Töövoo automatiseerimise fookus.
    - Reaalajas dokumentatsiooni hankimine: Python konsooli kliendi rakendamine.
    - Interaktiivne õppeplaani generaator: Chainlit vestlusveebi rakendus.
    - Dokumentatsioon redaktoris: VS Code ja GitHub Copilot integratsioon.
    - Azure API haldus: Ettevõtte API integreerimise mustrid.
    - GitHub MCP register: Ökosüsteemi arendamine ja kogukonna platvorm.
  - **Põhjalik kokkuvõte**: Uuesti kirjutatud kokkuvõtte sektsioon, mis toob esile seitse juhtumiuuringut, mis hõlmavad mitmeid MCP rakendamise mõõtmeid.
    - Ettevõtte integratsioon, mitme agendi orkestreerimine, arendaja produktiivsus.
    - Ökosüsteemi arendamine, hariduslikud rakendused kategooriatena.
    - Täiustatud ülevaated arhitektuurimustrite, rakendamisstrateegiate ja parimate praktikate kohta.
    - Rõhk MCP-le kui küpsele, tootmisvalmis protokollile.

#### Õppejuhendi uuendused (study_guide.md)
- **Visuaalne õppekava kaart**: Uuendatud mõttekaart, et lisada GitHub MCP register juhtumiuuringute sektsiooni.
- **Juhtumiuuringute kirjeldus**: Täiustatud üldistest kirjeldustest põhjalikuks jaotuseks seitsme juhtumiuuringu kohta.
- **Repo struktuur**: Uuendatud sektsioon 10, et kajastada põhjalikku juhtumiuuringute katvust koos konkreetsete rakenduse üksikasjadega.
- **Muudatuste logi integreerimine**: Lisatud 26. september 2025 sissekanne, mis dokumenteerib GitHub MCP registri lisamist ja juhtumiuuringute täiustusi.
- **Kuupäeva uuendused**: Uuendatud jaluse ajatemplit, et kajastada viimast redaktsiooni (26. september 2025).

### Dokumentatsiooni kvaliteedi parandamine
- **Järjepidevuse täiustamine**: Standardiseeritud juhtumiuuringute vorming ja struktuur kõigis seitsmes näites.
- **Põhjalik katvus**: Juhtumiuuringud hõlmavad nüüd ettevõtte, arendaja produktiivsuse ja ökosüsteemi arendamise stsenaariume.
- **Strateegiline positsioneerimine**: Täiustatud fookus MCP-le kui agentide süsteemide juurutamise platvormile.
- **Ressursside integreerimine**: Uuendatud täiendavad ressursid, et lisada GitHub MCP registri link.

## 15. september 2025

### Täiustatud teemade laiendamine – Kohandatud transpordid ja konteksti inseneriteadus

#### MCP kohandatud transpordid (05-AdvancedTopics/mcp-transport/) – uus täiustatud rakendamise juhend
- **README.md**: Täielik rakendamise juhend MCP kohandatud transpordimehhanismide jaoks.
  - **Azure Event Grid transport**: Põhjalik serverivaba sündmuspõhine transpordi rakendamine.
    - C#, TypeScript ja Python näited koos Azure Functions integratsiooniga.
    - Sündmuspõhised arhitektuurimustrid skaleeritavate MCP lahenduste jaoks.
    - Veebikonksu vastuvõtjad ja push-põhine sõnumite käsitlemine.
  - **Azure Event Hubs transport**: Suure läbilaskevõimega voogedastuse transpordi rakendamine.
    - Reaalajas voogedastuse võimalused madala latentsusega stsenaariumide jaoks.
    - Partitsioneerimisstrateegiad ja kontrollpunktide haldamine.
    - Sõnumite rühmitamine ja jõudluse optimeerimine.
  - **Ettevõtte integratsioonimustrid**: Tootmisvalmis arhitektuurinäited.
    - Hajutatud MCP töötlemine mitme Azure Functions vahel.
    - Hübriidtranspordi arhitektuurid, mis ühendavad mitut transporditüüpi.
    - Sõnumite vastupidavus, usaldusväärsus ja vigade käsitlemise strateegiad.
  - **Turvalisus ja jälgimine**: Azure Key Vault integratsioon ja jälgitavuse mustrid.
    - Hallatud identiteedi autentimine ja minimaalne privileegide juurdepääs.
    - Application Insights telemeetria ja jõudluse jälgimine.
    - Circuit breakers ja tõrketaluvuse mustrid.
  - **Testimise raamistikud**: Põhjalikud testimisstrateegiad kohandatud transpordimehhanismide jaoks.
    - Üksustestimine testdubleerijate ja mokkeraamistikega.
    - Integreerimistestimine Azure Test Containersiga.
    - Jõudluse ja koormustestimise kaalutlused.

#### Konteksti inseneriteadus (05-AdvancedTopics/mcp-contextengineering/) – Tekkiv AI valdkond
- **README.md**: Põhjalik uurimus konteksti inseneriteaduse kui tekkiva valdkonna kohta.
  - **Põhiprintsiibid**: Täielik konteksti jagamine, tegevuse otsuste teadlikkus ja konteksti akna haldamine.
  - **MCP protokolli joondamine**: Kuidas MCP disain lahendab konteksti inseneriteaduse väljakutseid.
    - Konteksti akna piirangud ja progressiivse laadimise strateegiad.
    - Asjakohasuse määramine ja dünaamilise konteksti hankimise strateegiad.
    - Mitmeliigilise konteksti käsitlemine ja turvalisuse kaalutlused.
  - **Rakendamise lähenemised**: Ühe lõimega vs. mitme agendi arhitektuurid.
    - Konteksti tükeldamise ja prioriteetide määramise tehnikad.
    - Progressiivse konteksti laadimise ja tihendamise strateegiad.
    - Kihiline konteksti lähenemine ja hankimise optimeerimine.
  - **Mõõtmise raamistik**: Tekkivad mõõdikud konteksti tõhususe hindamiseks.
    - Sisendi efektiivsus, jõudlus, kvaliteet ja kasutajakogemuse kaalutlused.
    - Eksperimentaalsed lähenemised konteksti optimeerimiseks.
    - Tõrkeanalüüs ja täiustamise metoodikad.

#### Õppekava navigeerimise uuendused (README.md)
- **Täiustatud moodulistruktuur**: Uuendatud õppekava tabel, et lisada uusi täiustatud teemasid.
  - Lisatud Konteksti inseneriteadus (5.14) ja Kohandatud transport (5.15) kirjed.
  - Järjekindel vorming ja navigeerimislingid kõigis moodulites.
  - Uuendatud kirjeldused, et kajastada praegust sisukava.

### Kataloogistruktuuri täiustused
- **Nime standardiseerimine**: "mcp transport" ümber nimetatud "mcp-transport" järjepidevuse tagamiseks teiste täiustatud teemade kaustadega.
- **Sisu organiseerimine**: Kõik 05-AdvancedTopics kaustad järgivad nüüd järjekindlat nime mustrit (mcp-[teema]).

### Dokumentatsiooni kvaliteedi täiustused
- **MCP spetsifikatsiooni joondamine**: Kogu uus sisu viitab praegusele MCP spetsifikatsioonile 2025-06-18.
- **Mitmekeelne näidete valik**: Põhjalikud koodinäited C#, TypeScriptis ja Pythonis.
- **Ettevõtte fookus**: Tootmisvalmis mustrid ja Azure pilve integreerimine kogu ulatuses.
- **Visuaalne dokumentatsioon**: Mermaid diagrammid arhitektuuri ja voogude visualiseerimiseks.

## 18. august 2025

### Dokumentatsiooni põhjalik uuendus – MCP 2025-06-18 standardid

#### MCP turvalisuse parimad praktikad (02-Security/) – täielik moderniseerimine
- **MCP-SECURITY-BEST-PRACTICES-2025.md**: Täielik ümberkirjutus, mis on kooskõlas MCP spetsifikatsiooniga 2025-06-18.
  - **Kohustuslikud nõuded**: Lisatud selged MUST/MUST NOT nõuded ametlikust spetsifikatsioonist koos visua
#### Täiustatud teemad Turvalisus (05-AdvancedTopics/mcp-security/) - Tootmiskõlblik rakendus
- **README.md**: Täielik ümberkirjutus ettevõtte turvalisuse rakendamiseks
  - **Praegune spetsifikatsiooni vastavus**: Uuendatud MCP spetsifikatsioonile 2025-06-18 koos kohustuslike turvanõuetega
  - **Tõhustatud autentimine**: Microsoft Entra ID integreerimine koos põhjalike .NET ja Java Spring Security näidetega
  - **AI turvalisuse integreerimine**: Microsoft Prompt Shields ja Azure Content Safety rakendamine koos detailsete Python näidetega
  - **Täiustatud ohu leevendamine**: Põhjalikud rakendusnäited
    - Segadusse ajava asendaja rünnakute ennetamine PKCE ja kasutaja nõusoleku valideerimisega
    - Tokeni edastamise ennetamine auditooriumi valideerimise ja turvalise tokeni haldamisega
    - Sessiooni kaaperdamise ennetamine krüptograafilise sidumise ja käitumusliku analüüsiga
  - **Ettevõtte turvalisuse integreerimine**: Azure Application Insights jälgimine, ohu tuvastamise torud ja tarneahela turvalisus
  - **Rakendamise kontrollnimekiri**: Selged kohustuslikud vs soovitatavad turvakontrollid koos Microsofti turvaökosüsteemi eelistega

### Dokumentatsiooni kvaliteet ja standardite vastavus
- **Spetsifikatsiooni viited**: Kõik viited uuendatud praegusele MCP spetsifikatsioonile 2025-06-18
- **Microsofti turvaökosüsteem**: Täiustatud integreerimisjuhised kogu turvadokumentatsioonis
- **Praktiline rakendamine**: Lisatud detailseid koodinäiteid .NET, Java ja Pythonis koos ettevõtte mustritega
- **Ressursside organiseerimine**: Ametlike dokumentide, turvastandardite ja rakendusjuhendite põhjalik kategoriseerimine
- **Visuaalsed indikaatorid**: Kohustuslike nõuete ja soovitatavate praktikate selge märgistamine

#### Põhimõisted (01-CoreConcepts/) - Täielik moderniseerimine
- **Protokolli versiooni uuendus**: Uuendatud viited praegusele MCP spetsifikatsioonile 2025-06-18 koos kuupäevapõhise versioonimisega (YYYY-MM-DD formaat)
- **Arhitektuuri täpsustamine**: Täiustatud Hostide, Klientide ja Serverite kirjeldused, et kajastada praeguseid MCP arhitektuurimustreid
  - Hostid nüüd selgelt määratletud kui AI rakendused, mis koordineerivad mitut MCP kliendiühendust
  - Kliendid kirjeldatud kui protokolli ühendajad, mis säilitavad üks-ühele serveri suhteid
  - Serverid täpsustatud kohalike vs kaugjuurutamise stsenaariumidega
- **Primitiivide ümberstruktureerimine**: Serveri ja kliendi primitiivide täielik ümberkorraldamine
  - Serveri primitiivid: Ressursid (andmeallikad), Mallid (šabloonid), Tööriistad (käivitatavad funktsioonid) koos detailsete selgituste ja näidetega
  - Kliendi primitiivid: Proovivõtmine (LLM täitmised), Küsitlus (kasutaja sisend), Logimine (silumine/jälgimine)
  - Uuendatud praeguste avastamise (`*/list`), hankimise (`*/get`) ja käivitamise (`*/call`) meetodimustritega
- **Protokolli arhitektuur**: Tutvustatud kahetasandiline arhitektuurimudel
  - Andmekiht: JSON-RPC 2.0 alus koos elutsükli haldamise ja primitiividega
  - Transpordikiht: STDIO (kohalik) ja Streamable HTTP koos SSE-ga (kaug) transpordimehhanismid
- **Turvalisuse raamistik**: Põhjalikud turvapõhimõtted, sealhulgas kasutaja selgesõnaline nõusolek, andmete privaatsuse kaitse, tööriistade käivitamise ohutus ja transpordikihi turvalisus
- **Kommunikatsioonimustrid**: Uuendatud protokollisõnumid, et näidata initsialiseerimist, avastamist, käivitamist ja teavitamisvooge
- **Koodinäited**: Uuendatud mitmekeelsed näited (.NET, Java, Python, JavaScript), et kajastada praeguseid MCP SDK mustreid

#### Turvalisus (02-Security/) - Põhjalik turvalisuse ümberkorraldamine  
- **Standardite vastavus**: Täielik vastavus MCP spetsifikatsiooni 2025-06-18 turvanõuetele
- **Autentimise areng**: Dokumenteeritud areng kohandatud OAuth serveritest väliste identiteedipakkujate delegatsioonini (Microsoft Entra ID)
- **AI-spetsiifiline ohuanalüüs**: Täiustatud katvus kaasaegsete AI rünnakute vektorite osas
  - Detailne prompt injection rünnakustsenaariumide kirjeldus koos reaalse maailma näidetega
  - Tööriistade mürgitamise mehhanismid ja "vaiba alt tõmbamise" rünnakumustrid
  - Kontekstiakna mürgitamine ja mudeli segadusse ajamise rünnakud
- **Microsofti AI turvalahendused**: Põhjalik katvus Microsofti turvaökosüsteemi osas
  - AI Prompt Shields koos täiustatud tuvastamise, esiletõstmise ja eraldaja tehnikatega
  - Azure Content Safety integreerimismustrid
  - GitHub Advanced Security tarneahela kaitseks
- **Täiustatud ohu leevendamine**: Detailne turvakontroll
  - Sessiooni kaaperdamine MCP-spetsiifiliste rünnakustsenaariumidega ja krüptograafiliste sessiooni ID nõuetega
  - Segadusse ajava asendaja probleemid MCP proxy stsenaariumides koos selgesõnaliste nõusolekunõuetega
  - Tokeni edastamise haavatavused koos kohustuslike valideerimiskontrollidega
- **Tarneahela turvalisus**: Laiendatud AI tarneahela katvus, sealhulgas baasmudelid, sisseehitatud teenused, konteksti pakkujad ja kolmanda osapoole API-d
- **Alusturvalisus**: Täiustatud integreerimine ettevõtte turvamustritega, sealhulgas nullusalduse arhitektuur ja Microsofti turvaökosüsteem
- **Ressursside organiseerimine**: Kategoriseeritud põhjalikud ressursilingid tüübi järgi (Ametlikud dokumendid, Standardid, Uuringud, Microsofti lahendused, Rakendusjuhendid)

### Dokumentatsiooni kvaliteedi parandused
- **Struktureeritud õpieesmärgid**: Täiustatud õpieesmärgid konkreetsete, rakendatavate tulemustega
- **Ristviited**: Lisatud lingid seotud turvalisuse ja põhimõistete teemade vahel
- **Praegune teave**: Kõik kuupäevaviited ja spetsifikatsioonilingid uuendatud vastavalt praegustele standarditele
- **Rakendusjuhised**: Lisatud konkreetsed, rakendatavad juhised kogu mõlemas jaotises

## 16. juuli 2025

### README ja navigeerimise täiustused
- Täielikult ümber kujundatud õppekava navigeerimine README.md-s
- Asendatud `<details>` sildid ligipääsetavama tabelipõhise formaadiga
- Loodud alternatiivsed paigutusvalikud uues "alternative_layouts" kaustas
- Lisatud kaardipõhised, vahekaartidega ja akordionstiilis navigeerimise näited
- Uuendatud repositooriumi struktuuri jaotis, et hõlmata kõiki viimaseid faile
- Täiustatud "Kuidas seda õppekava kasutada" jaotist selgete soovitustega
- Uuendatud MCP spetsifikatsiooni lingid, et viidata õigele URL-ile
- Lisatud konteksti inseneriteaduse jaotis (5.14) õppekava struktuuri

### Õpijuhendi uuendused
- Täielikult ümber töötatud õpijuhend, et vastata praegusele repositooriumi struktuurile
- Lisatud uued jaotised MCP klientide ja tööriistade ning populaarsete MCP serverite kohta
- Uuendatud visuaalne õppekava kaart, et täpselt kajastada kõiki teemasid
- Täiustatud täpsustused täiustatud teemade kohta, et hõlmata kõiki spetsialiseeritud valdkondi
- Uuendatud juhtumiuuringute jaotis, et kajastada tegelikke näiteid
- Lisatud see põhjalik muudatuste logi

### Kogukonna panused (06-CommunityContributions/)
- Lisatud detailne teave MCP serverite kohta pildigeneratsiooniks
- Lisatud põhjalik jaotis Claude'i kasutamise kohta VSCode'is
- Lisatud Cline terminalikliendi seadistamise ja kasutamise juhised
- Uuendatud MCP kliendi jaotis, et hõlmata kõiki populaarseid kliendivalikuid
- Täiustatud panusenäited täpsemate koodinäidete abil

### Täiustatud teemad (05-AdvancedTopics/)
- Korraldatud kõik spetsialiseeritud teemade kaustad ühtlase nimetamisega
- Lisatud konteksti inseneriteaduse materjalid ja näited
- Lisatud Foundry agendi integreerimise dokumentatsioon
- Täiustatud Entra ID turvalisuse integreerimise dokumentatsioon

## 11. juuni 2025

### Esialgne loomine
- Välja antud MCP algajatele mõeldud õppekava esimene versioon
- Loodud kõigi 10 põhijaotise põhistruktuur
- Rakendatud visuaalne õppekava kaart navigeerimiseks
- Lisatud esialgsed näidisprojektid mitmes programmeerimiskeeles

### Alustamine (03-GettingStarted/)
- Loodud esimesed serveri rakendamise näited
- Lisatud kliendi arendamise juhised
- Kaasatud LLM kliendi integreerimise juhised
- Lisatud VS Code integreerimise dokumentatsioon
- Rakendatud Server-Sent Events (SSE) serveri näited

### Põhimõisted (01-CoreConcepts/)
- Lisatud detailne selgitus kliendi-serveri arhitektuuri kohta
- Loodud dokumentatsioon protokolli põhikomponentide kohta
- Dokumenteeritud sõnumimustrid MCP-s

## 23. mai 2025

### Repositooriumi struktuur
- Algatatud repositoorium põhilise kaustastruktuuriga
- Loodud README failid iga suurema jaotise jaoks
- Seadistatud tõlke infrastruktuur
- Lisatud pildivarad ja diagrammid

### Dokumentatsioon
- Loodud esialgne README.md õppekava ülevaatega
- Lisatud CODE_OF_CONDUCT.md ja SECURITY.md
- Seadistatud SUPPORT.md juhistega abi saamiseks
- Loodud esialgne õpijuhendi struktuur

## 15. aprill 2025

### Planeerimine ja raamistik
- MCP algajatele mõeldud õppekava esialgne planeerimine
- Määratletud õpieesmärgid ja sihtrühm
- Kavandatud õppekava 10-jaotise struktuur
- Välja töötatud kontseptuaalne raamistik näidete ja juhtumiuuringute jaoks
- Loodud esialgsed prototüüpnäited põhikontseptsioonide jaoks

---

**Lahtiütlus**:  
See dokument on tõlgitud AI tõlketeenuse [Co-op Translator](https://github.com/Azure/co-op-translator) abil. Kuigi püüame tagada täpsust, palume arvestada, et automaatsed tõlked võivad sisaldada vigu või ebatäpsusi. Algne dokument selle algses keeles tuleks pidada autoriteetseks allikaks. Olulise teabe puhul soovitame kasutada professionaalset inimtõlget. Me ei vastuta selle tõlke kasutamisest tulenevate arusaamatuste või valesti tõlgenduste eest.