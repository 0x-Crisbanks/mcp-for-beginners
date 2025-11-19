<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "1c767a35642f753127dc08545c25a290",
  "translation_date": "2025-10-11T12:05:11+00:00",
  "source_file": "02-Security/README.md",
  "language_code": "et"
}
-->
# MCP Turvalisus: Põhjalik kaitse AI-süsteemidele

[![MCP Turvalisuse Parimad Tavad](../../../translated_images/03.175aed6dedae133f9d41e49cefd0f0a9a39c3317e1eaa7ef7182696af7534308.et.png)](https://youtu.be/88No8pw706o)

_(Klõpsake ülaloleval pildil, et vaadata selle õppetunni videot)_

Turvalisus on AI-süsteemide disaini põhialus, mistõttu käsitleme seda oma teises jaotises. See on kooskõlas Microsofti **Secure by Design** põhimõttega, mis on osa [Secure Future Initiative](https://www.microsoft.com/security/blog/2025/04/17/microsofts-secure-by-design-journey-one-year-of-success/) algatusest.

Model Context Protocol (MCP) toob AI-põhistele rakendustele võimsad uued võimalused, kuid samas ka unikaalsed turvalisuse väljakutsed, mis ulatuvad kaugemale traditsioonilistest tarkvarariskidest. MCP-süsteemid seisavad silmitsi nii tavapäraste turvalisuse probleemidega (turvaline kodeerimine, minimaalne privileegide kasutamine, tarneahela turvalisus) kui ka uute AI-spetsiifiliste ohtudega, nagu prompt injection, tööriistade mürgitamine, sessiooni kaaperdamine, confused deputy rünnakud, token passthrough haavatavused ja dünaamiline võimekuse muutmine.

Selles õppetunnis uurime MCP rakenduste kõige kriitilisemaid turvariske—käsitledes autentimist, autoriseerimist, liigseid privileege, kaudset prompt injection'i, sessiooni turvalisust, confused deputy probleeme, tokenite haldamist ja tarneahela haavatavusi. Õpite rakendama praktilisi kontrollimeetmeid ja parimaid tavasid nende riskide leevendamiseks, kasutades Microsofti lahendusi nagu Prompt Shields, Azure Content Safety ja GitHub Advanced Security, et tugevdada oma MCP juurutust.

## Õppe-eesmärgid

Selle õppetunni lõpuks suudate:

- **Tuvastada MCP-spetsiifilisi ohte**: Tunnustada MCP-süsteemide unikaalseid turvariske, sealhulgas prompt injection, tööriistade mürgitamine, liigsed privileegid, sessiooni kaaperdamine, confused deputy probleemid, token passthrough haavatavused ja tarneahela riskid
- **Rakendada turvakontrolle**: Rakendada tõhusaid leevendusmeetmeid, sealhulgas tugev autentimine, minimaalne privileegide kasutamine, turvaline tokenite haldamine, sessiooni turvakontrollid ja tarneahela kontrollimine
- **Kasutada Microsofti turvalahendusi**: Mõista ja juurutada Microsoft Prompt Shields, Azure Content Safety ja GitHub Advanced Security MCP töökoormuste kaitseks
- **Valideerida tööriistade turvalisust**: Tunnustada tööriistade metaandmete valideerimise, dünaamiliste muutuste jälgimise ja kaudsete prompt injection rünnakute vastu kaitsmise olulisust
- **Integreerida parimaid tavasid**: Kombineerida kehtestatud turvalisuse aluspõhimõtted (turvaline kodeerimine, serveri kõvendamine, nullusaldus) MCP-spetsiifiliste kontrollidega, et tagada terviklik kaitse

# MCP Turvalisuse Arhitektuur ja Kontrollimeetmed

Kaasaegsed MCP rakendused vajavad kihilist turvalisuse lähenemist, mis käsitleb nii traditsioonilisi tarkvara turvalisuse kui ka AI-spetsiifilisi ohte. Kiiresti arenev MCP spetsifikatsioon jätkab oma turvakontrollide täiustamist, võimaldades paremat integreerimist ettevõtte turvalisuse arhitektuuridega ja kehtestatud parimate tavadega.

[Microsoft Digital Defense Report](https://aka.ms/mddr) uuringud näitavad, et **98% teatatud rikkumistest oleksid ennetatavad tugeva turvahügieeniga**. Kõige tõhusam kaitsestrateegia ühendab põhilised turvapraktikad MCP-spetsiifiliste kontrollidega—tõestatud aluspõhimõtted jäävad kõige mõjusamaks üldise turvariski vähendamisel.

## Praegune Turvalisuse Olukord

> **Märkus:** See teave kajastab MCP turvastandardeid seisuga **18. august 2025**. MCP protokoll areneb kiiresti ja tulevased rakendused võivad tuua uusi autentimismustreid ja täiustatud kontrolle. Alati viidake praegusele [MCP Spetsifikatsioonile](https://spec.modelcontextprotocol.io/), [MCP GitHubi repositooriumile](https://github.com/modelcontextprotocol) ja [turvalisuse parimate tavade dokumentatsioonile](https://modelcontextprotocol.io/specification/2025-06-18/basic/security_best_practices) uusima juhendi saamiseks.

### MCP Autentimise Evolutsioon

MCP spetsifikatsioon on autentimise ja autoriseerimise lähenemisviisis oluliselt arenenud:

- **Algne lähenemine**: Varased spetsifikatsioonid nõudsid arendajatelt kohandatud autentimisserverite rakendamist, kus MCP serverid toimisid OAuth 2.0 autoriseerimisserveritena, hallates kasutaja autentimist otse
- **Praegune standard (2025-06-18)**: Uuendatud spetsifikatsioon võimaldab MCP serveritel delegeerida autentimist välistele identiteedipakkujatele (näiteks Microsoft Entra ID), parandades turvalisust ja vähendades rakendamise keerukust
- **Transport Layer Security**: Täiustatud tugi turvaliste transpordimehhanismide jaoks koos korrektsete autentimismustritega nii kohalike (STDIO) kui ka kaugühenduste (Streamable HTTP) jaoks

## Autentimise ja Autoriseerimise Turvalisus

### Praegused Turvalisuse Väljakutsed

Kaasaegsed MCP rakendused seisavad silmitsi mitmete autentimise ja autoriseerimise väljakutsetega:

### Riskid ja Ohud

- **Valesti konfigureeritud autoriseerimisloogika**: Vigane autoriseerimise rakendamine MCP serverites võib paljastada tundlikke andmeid ja rakendada valesid juurdepääsukontrolle
- **OAuth tokeni kompromiss**: Kohaliku MCP serveri tokeni vargus võimaldab ründajatel serverit jäljendada ja pääseda juurde allavoolu teenustele
- **Token passthrough haavatavused**: Ebaõige tokenite käsitlemine loob turvakontrollide möödapääsemise ja vastutuse lüngad
- **Liigsed privileegid**: Üleprivilegeeritud MCP serverid rikuvad minimaalsete privileegide põhimõtteid ja laiendavad rünnakupinda

#### Token Passthrough: Kriitiline Anti-Muster

**Token passthrough on praeguses MCP autoriseerimise spetsifikatsioonis rangelt keelatud** tõsiste turvalisuse tagajärgede tõttu:

##### Turvakontrollide möödapääsemine
- MCP serverid ja allavoolu API-d rakendavad kriitilisi turvakontrolle (kiiruse piiramine, päringute valideerimine, liikluse jälgimine), mis sõltuvad korrektsetest tokenite valideerimisest
- Otsene kliendi-API tokenite kasutamine möödub nendest olulistest kaitsemehhanismidest, õõnestades turvalisuse arhitektuuri

##### Vastutuse ja auditi väljakutsed  
- MCP serverid ei suuda eristada kliente, kes kasutavad ülesvoolu väljastatud tokeneid, rikkudes auditi jälgi
- Allavoolu ressursiserveri logid näitavad eksitavaid päringu päritolu, mitte tegelikke MCP serveri vahendajaid
- Intsidentide uurimine ja vastavusauditid muutuvad oluliselt keerulisemaks

##### Andmete väljaviimise riskid
- Valideerimata tokeni väited võimaldavad pahatahtlikel osapooltel varastatud tokenitega kasutada MCP servereid andmete väljaviimiseks
- Usalduspiiride rikkumised võimaldavad volitamata juurdepääsumustreid, mis mööduvad kavandatud turvakontrollidest

##### Mitme teenuse rünnakuvektorid
- Kompromiteeritud tokenid, mida aktsepteerivad mitmed teenused, võimaldavad külgliikumist ühendatud süsteemides
- Usaldus eeldused teenuste vahel võivad olla rikutud, kui tokeni päritolu ei saa kinnitada

### Turvakontrollid ja Leevendusmeetmed

**Kriitilised Turvanõuded:**

> **KOHUSTUSLIK**: MCP serverid **EI TOHI** aktsepteerida ühtegi tokenit, mida ei ole MCP serverile spetsiaalselt väljastatud

#### Autentimise ja Autoriseerimise Kontrollid

- **Täpne autoriseerimise ülevaatus**: Viige läbi põhjalikud auditid MCP serveri autoriseerimisloogika kohta, et tagada, et ainult kavandatud kasutajad ja kliendid pääsevad tundlikele ressurssidele
  - **Rakendamise juhend**: [Azure API Management kui MCP serverite autentimisvärav](https://techcommunity.microsoft.com/blog/integrationsonazureblog/azure-api-management-your-auth-gateway-for-mcp-servers/4402690)
  - **Identiteedi integreerimine**: [Microsoft Entra ID kasutamine MCP serveri autentimiseks](https://den.dev/blog/mcp-server-auth-entra-id-session/)

- **Turvaline tokenite haldamine**: Rakendage [Microsofti tokenite valideerimise ja elutsükli parimaid tavasid](https://learn.microsoft.com/en-us/entra/identity-platform/access-tokens)
  - Valideerige tokeni sihtgrupi väited, et need vastaksid MCP serveri identiteedile
  - Rakendage korrektseid tokenite rotatsiooni ja aegumispoliitikaid
  - Ennetage tokenite korduvkasutamise rünnakuid ja volitamata kasutamist

- **Kaitstud tokenite salvestamine**: Turvaline tokenite salvestamine krüpteerimisega nii puhkeolekus kui ka edastamisel
  - **Parimad tavad**: [Turvalise tokenite salvestamise ja krüpteerimise juhised](https://youtu.be/uRdX37EcCwg?si=6fSChs1G4glwXRy2)

#### Juurdepääsukontrolli rakendamine

- **Minimaalsete privileegide põhimõte**: Andke MCP serveritele ainult minimaalsed õigused, mis on vajalikud kavandatud funktsionaalsuse jaoks
  - Regulaarne privileegide ülevaatus ja uuendamine, et vältida privileegide kuhjumist
  - **Microsofti dokumentatsioon**: [Turvaline minimaalsete privileegide juurdepääs](https://learn.microsoft.com/entra/identity-platform/secure-least-privileged-access)

- **Rollipõhine juurdepääsukontroll (RBAC)**: Rakendage peeneteralisi rollide määramisi
  - Piirake rolle rangelt konkreetsete ressursside ja toimingutega
  - Vältige laiaulatuslikke või mittevajalikke privileege, mis laiendavad rünnakupinda

- **Jätkuv privileegide jälgimine**: Rakendage pidevat juurdepääsu auditeerimist ja jälgimist
  - Jälgige privileegide kasutusmustreid anomaaliate tuvastamiseks
  - Parandage kiiresti liigsed või kasutamata privileegid

## AI-spetsiifilised Turvaohtud

### Prompt Injection ja Tööriistade Manipuleerimise Rünnakud

Kaasaegsed MCP rakendused seisavad silmitsi keerukate AI-spetsiifiliste rünnakutega, mida traditsioonilised turvameetmed ei suuda täielikult käsitleda:

#### **Kaudne Prompt Injection (Cross-Domain Prompt Injection)**

**Kaudne Prompt Injection** kujutab endast üht kriitilisemat haavatavust MCP-toega AI-süsteemides. Ründajad peidavad pahatahtlikke juhiseid välises sisus—dokumentides, veebilehtedel, e-kirjades või andmeallikates—mida AI-süsteemid hiljem töötlevad kui legitiimseid käske.

**Rünnaku stsenaariumid:**
- **Dokumendipõhine süstimine**: Pahatahtlikud juhised peidetud töödeldud dokumentides, mis käivitavad soovimatud AI-toimingud
- **Veebisisu ekspluateerimine**: Kompromiteeritud veebilehed, mis sisaldavad sisseehitatud prompt'e, mis manipuleerivad AI käitumist, kui neid kraabitakse
- **E-kirjapõhised rünnakud**: Pahatahtlikud prompt'id e-kirjades, mis põhjustavad AI-assistentide andmete lekkimist või volitamata toiminguid
- **Andmeallika saastamine**: Kompromiteeritud andmebaasid või API-d, mis edastavad saastatud sisu AI-süsteemidele

**Reaalse maailma mõju**: Need rünnakud võivad põhjustada andmete väljaviimist, privaatsuse rikkumisi, kahjuliku sisu genereerimist ja kasutajate interaktsioonide manipuleerimist. Üksikasjaliku analüüsi jaoks vaadake [Prompt Injection MCP-s (Simon Willison)](https://simonwillison.net/2025/Apr/9/mcp-prompt-injection/).

![Prompt Injection Rünnaku Diagramm](../../../translated_images/prompt-injection.ed9fbfde297ca877c15bc6daa808681cd3c3dc7bf27bbbda342ef1ba5fc4f52d.et.png)

#### **Tööriistade Mürgitamise Rünnakud**

**Tööriistade mürgitamine** sihib MCP tööriistade metaandmeid, kasutades ära seda, kuidas LLM-id tõlgendavad tööriistade kirjeldusi ja parameetreid, et teha täitmispõhimõtteid.

**Rünnaku mehhanismid:**
- **Metaandmete manipuleerimine**: Ründajad süstivad pahatahtlikke juhiseid tööriistade kirjeldustesse, parameetrite määratlustesse või kasutusnäidetesse
- **Nähtamatud juhised**: Peidetud prompt'id tööriistade metaandmetes, mida AI-mudelid töötlevad, kuid mis on inimkasutajatele nähtamatud
- **Dünaamiline tööriistade muutmine ("Rug Pulls")**: Kasutajate poolt heaks kiidetud tööriistad muudetakse hiljem pahatahtlikeks toiminguteks ilma kasutaja teadlikkuseta
- **Parameetrite süstimine**: Pahatahtlik sisu, mis on peidetud tööriistade parameetrite skeemidesse ja mõjutab mudeli käitumist

**Hostitud serveri riskid**: Kaug-MCP serverid kujutavad endast kõrgendatud riske, kuna tööriistade määratlusi saab pärast esialgset kasutaja heakskiitu uuendada, luues olukordi, kus varem ohutud tööriistad muutuvad pahatahtlikeks. Üksikasjaliku analüüsi jaoks vaadake [Tööriistade Mürgitamise Rünnakud (Invariant Labs)](https://invariantlabs.ai/blog/mcp-security-notification-tool-poisoning-attacks).

![Tööriistade Süstimise Rünnaku Diagramm](../../../translated_images/tool-injection.3b0b4a6b24de6befe7d3afdeae44138ef005881aebcfc84c6f61369ce31e3640.et.png)

#### **Täiendavad AI Rünnakuvektorid**

- **Cross-Domain Prompt Injection (XPIA)**: Keerukad rünnakud, mis kasutavad mitme domeeni sisu turvakontrollide möödapääsemiseks
- **Dünaamiline Võimekuse Muutmine**: Reaalajas muudatused tööriistade võimekustes, mis jäävad esialgsete turvahinnangute alt välja
- **Kontekstakna mürgitamine**: Rünnakud, mis manipuleerivad suuri kontekstaknaid, et peita pahatahtlikke juhiseid
- **Mudeli segaduse rünnakud**: Mudeli piirangute ärakasutamine, et luua ettearvamatuid või ohtlikke käitumisi

### AI Turvariski Mõju

**Kõrge mõjuga tagajärjed:**
- **Andmete väljaviimine**: Volitamata juurdepääs ja tundlike ettevõtte või isikuandmete vargus
- **Privaatsuse rikkumised**: Isikuandmete (PII) ja konfidentsiaalsete ärisaladuste paljastamine  
- **Süsteemi manipuleerimine**: Soovimatud muudatused kriitilistes süsteemides ja töövoogudes
- **Mandaatide vargus**: Autentimistokenite ja teenusemandaatide kompromiteerimine
- **Külgliikumine**: Kompromiteeritud AI-süsteemide kasutamine laiemate võrgurünnakute alustamiseks

### Microsofti AI Turvalahendused

#### **AI Prompt Shields: Täiustatud Kaitse Süstimisrünnakute Vastu**

Microsoft **AI Prompt Shields** pakub terviklikku kaitset nii otseste kui ka kaudsete prompt injection rünnakute vastu mitme turvakihi kaudu:

##### **Põhilised Kaitsemehhanismid:**

1. **Täiustatud Tuvastus ja Filtreerimine**
   - Masinõppe algoritmid
- **Autoriseerimise kontroll**: MCP serverid, mis rakendavad autoriseerimist, **PEAVAD** kontrollima KÕIKI sissetulevaid päringuid ja **EI TOHI** tugineda sessioonidele autentimiseks.
- **Turvaline sessiooni genereerimine**: Kasutage krüptograafiliselt turvalisi, mitte-deterministlikke sessiooni ID-sid, mis on genereeritud turvaliste juhuslike arvude generaatoritega.
- **Kasutajaspetsiifiline sidumine**: Siduge sessiooni ID-d kasutajaspetsiifilise teabega, kasutades formaate nagu `<user_id>:<session_id>`, et vältida sessioonide kuritarvitamist kasutajate vahel.
- **Sessiooni elutsükli haldamine**: Rakendage korrektne aegumine, rotatsioon ja tühistamine, et piirata haavatavuse aknaid.
- **Transpordi turvalisus**: HTTPS on kohustuslik kogu suhtluse jaoks, et vältida sessiooni ID-de pealtkuulamist.

### Segaduses asetäitja probleem

**Segaduses asetäitja probleem** tekib, kui MCP serverid toimivad autentimisproksidena klientide ja kolmandate osapoolte teenuste vahel, luues võimalusi autoriseerimise möödahiilimiseks staatiliste kliendi ID-de kuritarvitamise kaudu.

#### **Rünnaku mehhanismid ja riskid**

- **Küpsistepõhine nõusoleku möödahiilimine**: Varasem kasutaja autentimine loob nõusolekuküpsiseid, mida ründajad kasutavad pahatahtlike autoriseerimispäringute kaudu, kasutades manipuleeritud ümbersuunamise URI-sid.
- **Autoriseerimiskoodi vargus**: Olemasolevad nõusolekuküpsised võivad põhjustada autoriseerimisserverite nõusolekuekraanide vahelejätmist, suunates koodid ründaja kontrollitud lõpp-punktidesse.
- **Volitamata API-le juurdepääs**: Varastatud autoriseerimiskoodid võimaldavad tokenite vahetust ja kasutaja identiteedi varjamist ilma otsese heakskiiduta.

#### **Leevendamisstrateegiad**

**Kohustuslikud meetmed:**
- **Selged nõusoleku nõuded**: MCP proksiserverid, mis kasutavad staatilisi kliendi ID-sid, **PEAVAD** hankima kasutaja nõusoleku iga dünaamiliselt registreeritud kliendi jaoks.
- **OAuth 2.1 turvalisuse rakendamine**: Järgige praeguseid OAuth turvalisuse parimaid tavasid, sealhulgas PKCE (Proof Key for Code Exchange) kõigi autoriseerimispäringute jaoks.
- **Range kliendi valideerimine**: Rakendage ranget ümbersuunamise URI-de ja kliendi identifikaatorite valideerimist, et vältida kuritarvitamist.

### Tokenite edastamise haavatavused  

**Tokenite edastamine** kujutab endast selget antipraktikat, kus MCP serverid aktsepteerivad kliendi tokenit ilma korraliku valideerimiseta ja edastavad selle allavoolu API-dele, rikkudes MCP autoriseerimise spetsifikatsioone.

#### **Turvalisuse tagajärjed**

- **Kontrolli möödahiilimine**: Otsene kliendi-API tokenite kasutamine möödub kriitilistest kiiruse piirangutest, valideerimisest ja jälgimiskontrollidest.
- **Auditijälje rikkumine**: Ülesvoolu väljastatud tokenid muudavad kliendi identifitseerimise võimatuks, rikkudes juhtumite uurimise võimalusi.
- **Proksipõhine andmete väljavedu**: Valideerimata tokenid võimaldavad pahatahtlikel osapooltel kasutada servereid volitamata andmetele juurdepääsuks.
- **Usalduspiiride rikkumine**: Allavoolu teenuste usaldus eeldused võivad olla rikutud, kui tokenite päritolu ei saa kinnitada.
- **Mitme teenuse rünnaku laiendamine**: Kompromiteeritud tokenid, mida aktsepteeritakse mitmes teenuses, võimaldavad külgsuunalist liikumist.

#### **Nõutavad turvameetmed**

**Mitte läbiräägitavad nõuded:**
- **Tokenite valideerimine**: MCP serverid **EI TOHI** aktsepteerida tokenit, mis pole MCP serveri jaoks selgesõnaliselt väljastatud.
- **Auditooriumi kontroll**: Kontrollige alati, et tokeni auditooriumi väited vastaksid MCP serveri identiteedile.
- **Korralik tokenite elutsükkel**: Rakendage lühiajalisi juurdepääsutokene turvaliste rotatsioonipraktikatega.

## Tarneahela turvalisus AI süsteemide jaoks

Tarneahela turvalisus on arenenud kaugemale traditsioonilistest tarkvara sõltuvustest, hõlmates kogu AI ökosüsteemi. Kaasaegsed MCP rakendused peavad rangelt kontrollima ja jälgima kõiki AI-ga seotud komponente, kuna igaüks neist võib tuua potentsiaalseid haavatavusi, mis ohustavad süsteemi terviklikkust.

### Laiendatud AI tarneahela komponendid

**Traditsioonilised tarkvara sõltuvused:**
- Avatud lähtekoodiga raamatukogud ja raamistikud
- Konteinerpildid ja baasüsteemid  
- Arendustööriistad ja ehitustorud
- Infrastruktuuri komponendid ja teenused

**AI-spetsiifilised tarneahela elemendid:**
- **Alusmudelid**: Eelõpetatud mudelid erinevatelt pakkujatelt, mis vajavad päritolu kontrollimist.
- **Embeding-teenused**: Välised vektoriseerimis- ja semantilise otsingu teenused.
- **Kontekstipakkujad**: Andmeallikad, teadmistebaasid ja dokumendirepositooriumid.  
- **Kolmanda osapoole API-d**: Välised AI teenused, ML torud ja andmetöötluse lõpp-punktid.
- **Mudelartefaktid**: Kaalud, konfiguratsioonid ja peenhäälestatud mudelivariandid.
- **Treeningandmete allikad**: Andmekogumid, mida kasutatakse mudelite treenimiseks ja peenhäälestamiseks.

### Põhjalik tarneahela turvalisuse strateegia

#### **Komponentide kontroll ja usaldus**
- **Päritolu valideerimine**: Kontrollige kõigi AI komponentide päritolu, litsentsimist ja terviklikkust enne integreerimist.
- **Turvalisuse hindamine**: Viige läbi mudelite, andmeallikate ja AI teenuste haavatavuse skaneerimine ja turvalisuse ülevaatus.
- **Maine analüüs**: Hinnake AI teenusepakkujate turvalisuse ajalugu ja praktikaid.
- **Vastavuse kontroll**: Veenduge, et kõik komponendid vastavad organisatsiooni turvalisuse ja regulatiivsetele nõuetele.

#### **Turvalised juurutustorud**  
- **Automatiseeritud CI/CD turvalisus**: Integreerige turvalisuse skaneerimine kogu automatiseeritud juurutustorude jooksul.
- **Artefaktide terviklikkus**: Rakendage krüptograafilist kontrolli kõigi juurutatud artefaktide (kood, mudelid, konfiguratsioonid) jaoks.
- **Etapiviisiline juurutamine**: Kasutage progressiivseid juurutusstrateegiaid turvalisuse valideerimisega igas etapis.
- **Usaldusväärsed artefaktide hoidlad**: Juurutage ainult kontrollitud ja turvalistest artefaktide registritest ja hoidlatest.

#### **Pidev jälgimine ja reageerimine**
- **Sõltuvuste skaneerimine**: Pidev haavatavuste jälgimine kõigi tarkvara ja AI komponentide sõltuvuste jaoks.
- **Mudelite jälgimine**: Pidev mudelite käitumise, jõudluse kõrvalekallete ja turvalisuse anomaaliate hindamine.
- **Teenuste tervise jälgimine**: Jälgige väliseid AI teenuseid saadavuse, turvaintsidentide ja poliitikamuutuste osas.
- **Ohuluure integreerimine**: Kaasake ohusööte, mis on spetsiifilised AI ja ML turvariskide jaoks.

#### **Juurdepääsukontroll ja minimaalne privileeg**
- **Komponenditaseme õigused**: Piirake juurdepääsu mudelitele, andmetele ja teenustele vastavalt ärivajadusele.
- **Teenusekontode haldamine**: Rakendage spetsiaalseid teenusekontosid minimaalsete vajalike õigustega.
- **Võrgusegmentatsioon**: Isolatsioon AI komponentide vahel ja piiratud võrgupääs teenuste vahel.
- **API värava kontrollid**: Kasutage tsentraliseeritud API väravaid, et kontrollida ja jälgida juurdepääsu välistele AI teenustele.

#### **Intsidentidele reageerimine ja taastumine**
- **Kiired reageerimisprotseduurid**: Etablitseeritud protsessid kompromiteeritud AI komponentide parandamiseks või asendamiseks.
- **Mandaatide rotatsioon**: Automatiseeritud süsteemid mandaatide, API võtmete ja teenuse volituste rotatsiooniks.
- **Tagasipööramise võimalused**: Võime kiiresti naasta AI komponentide varasemate teadaolevalt heade versioonide juurde.
- **Tarneahela rikkumise taastamine**: Spetsiifilised protseduurid reageerimiseks AI teenuste kompromiteerimisele.

### Microsofti turvatööriistad ja integratsioon

**GitHub Advanced Security** pakub põhjalikku tarneahela kaitset, sealhulgas:
- **Saladuste skaneerimine**: Automaatne volituste, API võtmete ja tokenite tuvastamine hoidlates.
- **Sõltuvuste skaneerimine**: Haavatavuste hindamine avatud lähtekoodiga sõltuvuste ja raamatukogude jaoks.
- **CodeQL analüüs**: Staatiline koodianalüüs turvaaukude ja kodeerimisprobleemide tuvastamiseks.
- **Tarneahela ülevaated**: Nähtavus sõltuvuste tervise ja turvalisuse seisundi osas.

**Azure DevOps ja Azure Repos integratsioon:**
- Sujuv turvalisuse skaneerimise integratsioon Microsofti arendusplatvormidel.
- Automaatne turvakontroll Azure Pipelines AI töökoormuste jaoks.
- Poliitikate jõustamine turvaliste AI komponentide juurutamiseks.

**Microsofti sisemised praktikad:**
Microsoft rakendab ulatuslikke tarneahela turvalisuse praktikaid kõigis toodetes. Lisateavet tõestatud lähenemisviiside kohta leiate [Microsofti tarkvara tarneahela turvalisuse teekond](https://devblogs.microsoft.com/engineering-at-microsoft/the-journey-to-secure-the-software-supply-chain-at-microsoft/).

## Alusturvalisuse parimad tavad

MCP rakendused pärivad ja arendavad edasi teie organisatsiooni olemasolevat turvalisuse taset. Alusturvalisuse tugevdamine parandab oluliselt AI süsteemide ja MCP juurutuste üldist turvalisust.

### Põhilised turvalisuse alused

#### **Turvalised arenduspraktikad**
- **OWASP vastavus**: Kaitske [OWASP Top 10](https://owasp.org/www-project-top-ten/) veebirakenduste haavatavuste eest.
- **AI-spetsiifilised kaitsed**: Rakendage kontrolli [OWASP Top 10 LLM-de jaoks](https://genai.owasp.org/download/43299/?tmstv=1731900559).
- **Turvaline saladuste haldamine**: Kasutage spetsiaalseid hoidlaid tokenite, API võtmete ja tundlike konfiguratsiooniandmete jaoks.
- **Lõpust-lõpuni krüpteerimine**: Rakendage turvalist suhtlust kõigi rakenduse komponentide ja andmevoogude vahel.
- **Sisendi valideerimine**: Rangelt kontrollige kõiki kasutaja sisendeid, API parameetreid ja andmeallikaid.

#### **Infrastruktuuri tugevdamine**
- **Mitmefaktoriline autentimine**: Kohustuslik MFA kõigi administratiivsete ja teenusekontode jaoks.
- **Plaasterdamise haldamine**: Automatiseeritud ja õigeaegne operatsioonisüsteemide, raamistikute ja sõltuvuste plaastrite rakendamine.  
- **Identiteedipakkuja integratsioon**: Tsentraliseeritud identiteedihaldus ettevõtte identiteedipakkujate kaudu (Microsoft Entra ID, Active Directory).
- **Võrgusegmentatsioon**: MCP komponentide loogiline isolatsioon, et piirata külgsuunalist liikumist.
- **Minimaalse privileegi põhimõte**: Minimaalselt vajalikud õigused kõigi süsteemi komponentide ja kontode jaoks.

#### **Turvalisuse jälgimine ja tuvastamine**
- **Põhjalik logimine**: Üksikasjalik logimine AI rakenduste tegevuste kohta, sealhulgas MCP klient-serveri interaktsioonid.
- **SIEM integratsioon**: Tsentraliseeritud turvateabe ja sündmuste haldamine anomaaliate tuvastamiseks.
- **Käitumuslik analüütika**: AI-põhine jälgimine, et tuvastada ebatavalisi mustreid süsteemi ja kasutaja käitumises.
- **Ohuluure**: Väliste ohusööte ja kompromissinäitajate (IOC) integreerimine.
- **Intsidentidele reageerimine**: Hästi määratletud protseduurid turvaintsidentide tuvastamiseks, reageerimiseks ja taastumiseks.

#### **Nullusalduse arhitektuur**
- **Ära kunagi usalda, alati kontrolli**: Kasutajate, seadmete ja võrguühenduste pidev kontroll.
- **Mikrosegmentatsioon**: Granuleeritud võrgukontrollid, mis isoleerivad individuaalsed töökoormused ja teenused.
- **Identiteedikeskne turvalisus**: Turvapoliitikad, mis põhinevad kontrollitud identiteetidel, mitte võrgu asukohal.
- **Pidev riskihindamine**: Dünaamiline turvalisuse taseme hindamine, mis põhineb praegusel kontekstil ja käitumisel.
- **Tingimuslik juurdepääs**: Juurdepääsukontrollid, mis kohanduvad riskitegurite, asukoha ja seadme usaldusväärsuse alusel.

### Ettevõtte integratsioonimustrid

#### **Microsofti turvaökosüsteemi integratsioon**
- **Microsoft Defender for Cloud**: Põhjalik pilveturvalisuse taseme haldamine.
- **Azure Sentinel**: Pilvepõhised SIEM ja SOAR võimalused AI töökoormuste kaitseks.
- **Microsoft Entra ID**: Ettevõtte identiteedi ja juurdepääsu haldamine tingimuslike juurdepääsupoliitikatega.
- **Azure Key Vault**: Tsentraliseeritud saladuste haldamine riistvaraturvalisuse mooduli (HSM) toega.
- **Microsoft Purview**: Andmehaldus ja vastavus AI andmeallikate ja töövoogude jaoks.

#### **Vastavus ja juhtimine**
- **Regulatiivne vastavus**: Veenduge, et MCP rakendused vastavad tööstusharu spetsiifilistele vastavusnõuetele (GDPR, HIPAA, SOC 2).
- **Andmete klassifikatsioon**: Tundlike andmete korrektne kategoriseerimine ja käsitlemine AI süsteemide poolt.
- **Auditijäljed**: Põhjalik logimine regulatiivse vastavuse ja kohtuekspertiisi uurimise jaoks.
- **Privaatsuskontrollid**: Privaatsuse põhimõtete rakendamine AI süsteemi arhitektuuris.
- **Muutuste haldamine**: Formaalne protsess AI süsteemi muudatuste turvaülevaatuseks.

Need alusturvalisuse praktikad loovad tugeva turvalisuse aluse, mis suurendab MCP-spetsiifiliste turvakontrollide tõhusust ja pakub põhjalikku kaitset AI-põhiste rakenduste jaoks.

## Olulised turvalisuse järeldused

- **Kihiline turvalisuse lähenemine**: Kombineerige alusturvalisuse praktikad (turvaline kodeerimine, minimaalne privileeg, tarneahela kontroll, pidev jälgimine) AI-spetsiifiliste kontrollidega, et tagada põhjalik kaitse.

- **AI-spetsiifiline ohumaastik**: MCP süsteemid seisavad silmitsi unikaalsete riskidega, sealhulgas prompt injection, tööriistade mürgitamine, sessiooni kaaperdamine, segaduses asetäitja probleemid, tokenite edastamise haavatavused ja liigsed õigused, mis vajavad spetsialiseeritud leevendusi.

- **Autentimise ja autoriseerimise tipptase**: Rakendage tugevat autentimist, kasutades väliseid identiteedipakkujaid (Microsoft Entra ID), jõustage korrektne tokenite valideerimine ja ärge kunagi aktsepteerige token
- [MCP Turvalisuse Uuringu Ülevaade (Wiz Security)](https://www.wiz.io/blog/mcp-security-research-briefing#remote-servers-22)

### **Microsofti Turvalahendused**
- [Microsoft Prompt Shields Dokumentatsioon](https://learn.microsoft.com/azure/ai-services/content-safety/concepts/jailbreak-detection)
- [Azure Sisu Turvalisuse Teenus](https://learn.microsoft.com/azure/ai-services/content-safety/)
- [Microsoft Entra ID Turvalisus](https://learn.microsoft.com/entra/identity-platform/secure-least-privileged-access)
- [Azure Tokenite Halduse Parimad Tavad](https://learn.microsoft.com/entra/identity-platform/access-tokens)
- [GitHub Täiustatud Turvalisus](https://github.com/security/advanced-security)

### **Rakendamise Juhendid ja Õpetused**
- [Azure API Management kui MCP Autentimise Värav](https://techcommunity.microsoft.com/blog/integrationsonazureblog/azure-api-management-your-auth-gateway-for-mcp-servers/4402690)
- [Microsoft Entra ID Autentimine MCP Serveritega](https://den.dev/blog/mcp-server-auth-entra-id-session/)
- [Turvaline Tokenite Salvestamine ja Krüpteerimine (Video)](https://youtu.be/uRdX37EcCwg?si=6fSChs1G4glwXRy2)

### **DevOps ja Tarneahela Turvalisus**
- [Azure DevOps Turvalisus](https://azure.microsoft.com/products/devops)
- [Azure Repos Turvalisus](https://azure.microsoft.com/products/devops/repos/)
- [Microsofti Tarneahela Turvalisuse Teekond](https://devblogs.microsoft.com/engineering-at-microsoft/the-journey-to-secure-the-software-supply-chain-at-microsoft/)

## **Täiendav Turvalisuse Dokumentatsioon**

Põhjaliku turvalisuse juhendi saamiseks vaadake selle jaotise spetsialiseeritud dokumente:

- **[MCP Turvalisuse Parimad Tavad 2025](./mcp-security-best-practices-2025.md)** - Täielik MCP rakenduste turvalisuse parimate tavade juhend
- **[Azure Sisu Turvalisuse Rakendamine](./azure-content-safety-implementation.md)** - Praktilised näited Azure Sisu Turvalisuse integreerimiseks  
- **[MCP Turvalisuse Kontrollid 2025](./mcp-security-controls-2025.md)** - Uusimad turvalisuse kontrollid ja tehnikad MCP juurutamiseks
- **[MCP Parimate Tavade Kiirviide](./mcp-best-practices.md)** - Kiirviide MCP turvalisuse olulistele tavadele

---

## Mis Järgmiseks

Järgmiseks: [3. peatükk: Alustamine](../03-GettingStarted/README.md)

---

**Lahtiütlus**:  
See dokument on tõlgitud AI tõlketeenuse [Co-op Translator](https://github.com/Azure/co-op-translator) abil. Kuigi püüame tagada täpsust, palume arvestada, et automaatsed tõlked võivad sisaldada vigu või ebatäpsusi. Algne dokument selle algses keeles tuleks pidada autoriteetseks allikaks. Olulise teabe puhul soovitame kasutada professionaalset inimtõlget. Me ei vastuta selle tõlke kasutamisest tulenevate arusaamatuste või valesti tõlgenduste eest.