<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "057dd5cc6bea6434fdb788e6c93f3f3d",
  "translation_date": "2025-10-11T12:00:31+00:00",
  "source_file": "02-Security/mcp-security-best-practices-2025.md",
  "language_code": "et"
}
-->
# MCP Turvalisuse Parimad Tavad - August 2025 Uuendus

> **Oluline**: See dokument kajastab uusimaid [MCP Spetsifikatsioon 2025-06-18](https://spec.modelcontextprotocol.io/specification/2025-06-18/) turvanõudeid ja ametlikke [MCP Turvalisuse Parimaid Tavasid](https://modelcontextprotocol.io/specification/2025-06-18/basic/security_best_practices). Järgige alati kehtivat spetsifikatsiooni, et saada kõige ajakohasemat juhendit.

## Olulised Turvalisuse Tavad MCP Rakenduste jaoks

Model Context Protocol toob kaasa unikaalseid turvalisuse väljakutseid, mis ulatuvad kaugemale traditsioonilisest tarkvara turvalisusest. Need tavad käsitlevad nii põhilisi turvanõudeid kui ka MCP-spetsiifilisi ohte, sealhulgas prompt injection, tööriistade mürgitamine, sessiooni kaaperdamine, segadusse aetud asendaja probleemid ja token passthrough haavatavused.

### **KOHUSTUSLIKUD Turvanõuded**

**Olulised nõuded MCP Spetsifikatsioonist:**

> **EI TOHI**: MCP serverid **EI TOHI** aktsepteerida ühtegi tokenit, mis ei ole MCP serveri jaoks selgesõnaliselt välja antud  
> 
> **PEAB**: MCP serverid, mis rakendavad autoriseerimist, **PEAVAD** kontrollima KÕIKI sissetulevaid päringuid  
>  
> **EI TOHI**: MCP serverid **EI TOHI** kasutada autentimiseks sessioone  
>
> **PEAB**: MCP proxy serverid, mis kasutavad staatilisi kliendi ID-sid, **PEAVAD** saama kasutaja nõusoleku iga dünaamiliselt registreeritud kliendi jaoks  

---

## 1. **Tokenite Turvalisus ja Autentimine**

**Autentimise ja Autoriseerimise Kontrollid:**
   - **Põhjalik Autoriseerimise Ülevaatus**: Viige läbi ulatuslikud auditid MCP serveri autoriseerimisloogika kohta, et tagada, et ainult ettenähtud kasutajad ja kliendid pääsevad ressurssidele ligi  
   - **Välise Identiteedipakkuja Integreerimine**: Kasutage tuntud identiteedipakkujaid nagu Microsoft Entra ID, mitte ärge rakendage kohandatud autentimist  
   - **Tokeni Sihtgrupi Kontroll**: Kontrollige alati, et tokenid oleksid selgesõnaliselt välja antud teie MCP serveri jaoks - ärge kunagi aktsepteerige ülesvoolu tokenit  
   - **Õige Tokeni Elutsükkel**: Rakendage turvalist tokenite rotatsiooni, aegumispoliitikaid ja vältige tokenite korduskasutamise rünnakuid  

**Tokenite Kaitstud Salvestamine:**
   - Kasutage Azure Key Vaulti või sarnaseid turvalisi volikirjade hoidlaid kõigi saladuste jaoks  
   - Rakendage tokenite krüpteerimist nii puhkeolekus kui ka edastamisel  
   - Korrapärane volikirjade rotatsioon ja volitamata juurdepääsu jälgimine  

## 2. **Sessioonihaldus ja Transpordi Turvalisus**

**Turvalised Sessioonitavad:**
   - **Krüptograafiliselt Turvalised Sessiooni ID-d**: Kasutage turvalisi, mitte-deterministlikke sessiooni ID-sid, mis on genereeritud turvaliste juhuslike numbrite generaatoritega  
   - **Kasutajaspetsiifiline Sidumine**: Siduge sessiooni ID-d kasutaja identiteetidega, kasutades formaate nagu `<user_id>:<session_id>`, et vältida sessioonide kuritarvitamist kasutajate vahel  
   - **Sessiooni Elutsükli Haldus**: Rakendage õiget aegumist, rotatsiooni ja tühistamist, et piirata haavatavuse aknaid  
   - **HTTPS/TLS Kohustuslikkus**: Kohustuslik HTTPS kogu suhtluse jaoks, et vältida sessiooni ID-de pealtkuulamist  

**Transpordikihi Turvalisus:**
   - Konfigureerige TLS 1.3 võimaluse korral koos korraliku sertifikaadihaldusega  
   - Rakendage sertifikaadi kinnitamist kriitiliste ühenduste jaoks  
   - Korrapärane sertifikaatide rotatsioon ja kehtivuse kontroll  

## 3. **AI-Spetsiifiliste Ohtude Kaitse** 🤖

**Prompt Injection Kaitse:**
   - **Microsoft Prompt Shields**: Kasutage AI Prompt Shieldi, et tuvastada ja filtreerida pahatahtlikke juhiseid  
   - **Sisendi Sanitiseerimine**: Kontrollige ja puhastage kõik sisendid, et vältida injektsioonirünnakuid ja segadusse aetud asendaja probleeme  
   - **Sisu Piirid**: Kasutage eraldus- ja andemärgistussüsteeme, et eristada usaldusväärseid juhiseid välisest sisust  

**Tööriistade Mürgitamise Ennetamine:**
   - **Tööriista Metaandmete Kontroll**: Rakendage tööriistade määratluste terviklikkuse kontrolli ja jälgige ootamatuid muudatusi  
   - **Dünaamiline Tööriistade Jälgimine**: Jälgige käitusaja käitumist ja seadistage hoiatused ootamatute täitmismustrite jaoks  
   - **Kinnitamise Töövood**: Nõudke kasutaja selgesõnalist kinnitust tööriistade muudatuste ja võimekuse muutuste jaoks  

## 4. **Juurdepääsukontroll ja Õigused**

**Vähima Õiguse Printsiip:**
   - Andke MCP serveritele ainult minimaalsed õigused, mis on vajalikud ettenähtud funktsionaalsuse jaoks  
   - Rakendage rollipõhist juurdepääsukontrolli (RBAC) koos peeneteraliste õigustega  
   - Korrapärased õiguste ülevaatused ja pidev jälgimine privileegide eskaleerimise suhtes  

**Käitusaja Õiguste Kontrollid:**
   - Rakendage ressursipiiranguid, et vältida ressursi ammendumise rünnakuid  
   - Kasutage konteineri isolatsiooni tööriistade täitmise keskkondade jaoks  
   - Rakendage just-in-time juurdepääsu administratiivsete funktsioonide jaoks  

## 5. **Sisu Turvalisus ja Jälgimine**

**Sisu Turvalisuse Rakendamine:**
   - **Azure Content Safety Integreerimine**: Kasutage Azure Content Safetyt, et tuvastada kahjulikku sisu, jailbreak-katseid ja poliitika rikkumisi  
   - **Käitumuslik Analüüs**: Rakendage käitusaja käitumise jälgimist, et tuvastada anomaaliaid MCP serveri ja tööriistade täitmises  
   - **Ulatuslik Logimine**: Logige kõik autentimiskatsed, tööriistade käivitamised ja turvaintsidendid turvalise, manipuleerimiskindla salvestusega  

**Pidev Jälgimine:**
   - Reaalajas hoiatused kahtlaste mustrite ja volitamata juurdepääsukatsete kohta  
   - Integreerimine SIEM-süsteemidega tsentraliseeritud turvaintsidentide haldamiseks  
   - Korrapärased turvaauditid ja MCP rakenduste läbitungimistestid  

## 6. **Tarneahela Turvalisus**

**Komponentide Kontroll:**
   - **Sõltuvuste Skaneerimine**: Kasutage automatiseeritud haavatavuste skaneerimist kõigi tarkvara sõltuvuste ja AI komponentide jaoks  
   - **Päritolu Kontroll**: Kontrollige mudelite, andmeallikate ja väliste teenuste päritolu, litsentsimist ja terviklikkust  
   - **Allkirjastatud Paketid**: Kasutage krüptograafiliselt allkirjastatud pakette ja kontrollige allkirju enne kasutuselevõttu  

**Turvaline Arendustoru:**
   - **GitHub Advanced Security**: Rakendage saladuste skaneerimist, sõltuvuste analüüsi ja CodeQL staatilist analüüsi  
   - **CI/CD Turvalisus**: Integreerige turvakontrollid kogu automatiseeritud juurutustorusse  
   - **Artefaktide Terviklikkus**: Rakendage krüptograafilist kontrolli juurutatud artefaktide ja konfiguratsioonide jaoks  

## 7. **OAuth Turvalisus ja Segadusse Aetud Asendaja Ennetamine**

**OAuth 2.1 Rakendamine:**
   - **PKCE Rakendamine**: Kasutage Proof Key for Code Exchange (PKCE) kõigi autoriseerimispäringute jaoks  
   - **Selgesõnaline Nõusolek**: Saage kasutaja nõusolek iga dünaamiliselt registreeritud kliendi jaoks, et vältida segadusse aetud asendaja rünnakuid  
   - **Ümbersuunamise URI Kontroll**: Rakendage ranget ümbersuunamise URI-de ja kliendi identifikaatorite kontrolli  

**Proxy Turvalisus:**
   - Ennetage autoriseerimise möödumist staatiliste kliendi ID-de ärakasutamise kaudu  
   - Rakendage korralikke nõusoleku töövooge kolmanda osapoole API-dele juurdepääsuks  
   - Jälgige autoriseerimiskoodi vargust ja volitamata API-dele juurdepääsu  

## 8. **Intsidentidele Reageerimine ja Taastumine**

**Kiired Reageerimisvõimalused:**
   - **Automatiseeritud Reageerimine**: Rakendage automatiseeritud süsteeme volikirjade rotatsiooniks ja ohtude tõkestamiseks  
   - **Tagasipöördumisprotseduurid**: Võime kiiresti naasta teadaolevalt heade konfiguratsioonide ja komponentide juurde  
   - **Kohtuekspertiisi Võimalused**: Üksikasjalikud auditijäljed ja logimine intsidentide uurimiseks  

**Suhtlus ja Koordineerimine:**
   - Selged eskaleerimisprotseduurid turvaintsidentide jaoks  
   - Integreerimine organisatsiooni intsidentidele reageerimise meeskondadega  
   - Korrapärased turvaintsidentide simulatsioonid ja lauaharjutused  

## 9. **Vastavus ja Juhtimine**

**Regulatiivne Vastavus:**
   - Tagage, et MCP rakendused vastavad tööstusharu spetsiifilistele nõuetele (GDPR, HIPAA, SOC 2)  
   - Rakendage andmete klassifitseerimise ja privaatsuse kontrollid AI andmetöötluse jaoks  
   - Säilitage ulatuslik dokumentatsioon vastavusauditite jaoks  

**Muutuste Haldus:**
   - Ametlikud turvaülevaatusprotsessid kõigi MCP süsteemi muudatuste jaoks  
   - Versioonikontroll ja kinnitamise töövood konfiguratsioonimuudatuste jaoks  
   - Korrapärased vastavushindamised ja puudujääkide analüüs  

## 10. **Täiustatud Turvakontrollid**

**Zero Trust Arhitektuur:**
   - **Ära Usalda, Kontrolli Alati**: Kasutajate, seadmete ja ühenduste pidev kontrollimine  
   - **Mikrosegmenteerimine**: Granuleeritud võrgu kontrollid, mis isoleerivad individuaalseid MCP komponente  
   - **Tingimuslik Juurdepääs**: Riskipõhised juurdepääsukontrollid, mis kohanduvad praeguse konteksti ja käitumisega  

**Rakenduse Käitusaja Kaitse:**
   - **Runtime Application Self-Protection (RASP)**: Rakendage RASP tehnikaid reaalajas ohtude tuvastamiseks  
   - **Rakenduse Jõudluse Jälgimine**: Jälgige jõudluse anomaaliaid, mis võivad viidata rünnakutele  
   - **Dünaamilised Turvapoliitikad**: Rakendage turvapoliitikaid, mis kohanduvad vastavalt praegusele ohumaastikule  

## 11. **Microsofti Turvaökosüsteemi Integreerimine**

**Ulatuslik Microsofti Turvalisus:**
   - **Microsoft Defender for Cloud**: Pilveturvalisuse seisundi haldamine MCP töökoormuste jaoks  
   - **Azure Sentinel**: Pilvepõhised SIEM ja SOAR võimalused täiustatud ohtude tuvastamiseks  
   - **Microsoft Purview**: Andmehaldus ja vastavus AI töövoogude ja andmeallikate jaoks  

**Identiteedi ja Juurdepääsu Haldus:**
   - **Microsoft Entra ID**: Ettevõtte identiteedihaldus tingimuslike juurdepääsupoliitikatega  
   - **Privileged Identity Management (PIM)**: Just-in-time juurdepääs ja kinnitamise töövood administratiivsete funktsioonide jaoks  
   - **Identiteedi Kaitse**: Riskipõhine tingimuslik juurdepääs ja automatiseeritud ohtude reageerimine  

## 12. **Pidev Turvalisuse Areng**

**Ajakohasena Püsimine:**
   - **Spetsifikatsiooni Jälgimine**: MCP spetsifikatsiooni uuenduste ja turvajuhiste muudatuste regulaarne ülevaatus  
   - **Ohuluure**: AI-spetsiifiliste ohusöötade ja kompromissi indikaatorite integreerimine  
   - **Turvakogukonna Kaasamine**: Aktiivne osalemine MCP turvakogukonnas ja haavatavuste avalikustamise programmides  

**Kohanduv Turvalisus:**
   - **Masinõppe Turvalisus**: Kasutage ML-põhist anomaaliate tuvastamist, et tuvastada uusi rünnakumustreid  
   - **Ennustav Turvaanalüütika**: Rakendage ennustavaid mudeleid proaktiivseks ohtude tuvastamiseks  
   - **Turvapoliitika Automatiseerimine**: Automatiseeritud turvapoliitika uuendused, mis põhinevad ohuluurel ja spetsifikatsiooni muudatustel  

---

## **Olulised Turvaressursid**

### **Ametlik MCP Dokumentatsioon**
- [MCP Spetsifikatsioon (2025-06-18)](https://spec.modelcontextprotocol.io/specification/2025-06-18/)
- [MCP Turvalisuse Parimad Tavad](https://modelcontextprotocol.io/specification/2025-06-18/basic/security_best_practices)
- [MCP Autoriseerimise Spetsifikatsioon](https://modelcontextprotocol.io/specification/2025-06-18/basic/authorization)

### **Microsofti Turvalahendused**
- [Microsoft Prompt Shields](https://learn.microsoft.com/azure/ai-services/content-safety/concepts/jailbreak-detection)
- [Azure Content Safety](https://learn.microsoft.com/azure/ai-services/content-safety/)
- [Microsoft Entra ID Turvalisus](https://learn.microsoft.com/entra/identity-platform/secure-least-privileged-access)
- [GitHub Advanced Security](https://github.com/security/advanced-security)

### **Turvastandardid**
- [OAuth 2.0 Turvalisuse Parimad Tavad (RFC 9700)](https://datatracker.ietf.org/doc/html/rfc9700)
- [OWASP Top 10 Suurte Keelemudelite jaoks](https://genai.owasp.org/)
- [NIST AI Riskijuhtimise Raamistik](https://www.nist.gov/itl/ai-risk-management-framework)

### **Rakendamise Juhendid**
- [Azure API Management MCP Autentimise Gateway](https://techcommunity.microsoft.com/blog/integrationsonazureblog/azure-api-management-your-auth-gateway-for-mcp-servers/4402690)
- [Microsoft Entra ID MCP Serveritega](https://den.dev/blog/mcp-server-auth-entra-id-session/)

---

> **Turvateade**: MCP turvatavad arenevad kiiresti. Kontrollige alati kehtiva [MCP spetsifikatsiooni](https://spec.modelcontextprotocol.io/) ja [ametliku turvadokumentatsiooni](https://modelcontextprotocol.io/specification/2025-06-18/basic/security_best_practices) vastu enne rakendamist.

---

**Lahtiütlus**:  
See dokument on tõlgitud AI tõlketeenuse [Co-op Translator](https://github.com/Azure/co-op-translator) abil. Kuigi püüame tagada täpsust, palume arvestada, et automaatsed tõlked võivad sisaldada vigu või ebatäpsusi. Algne dokument selle algses keeles tuleks pidada autoriteetseks allikaks. Olulise teabe puhul soovitame kasutada professionaalset inimtõlget. Me ei vastuta selle tõlke kasutamisest tulenevate arusaamatuste või valesti tõlgenduste eest.