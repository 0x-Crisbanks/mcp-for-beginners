<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "b2b9e15e78b9d9a2b3ff3e8fd7d1f434",
  "translation_date": "2025-10-11T11:59:20+00:00",
  "source_file": "02-Security/mcp-best-practices.md",
  "language_code": "et"
}
-->
# MCP Turvalisuse Parimad Tavad 2025

See põhjalik juhend kirjeldab olulisi turvalisuse parimaid tavasid Model Context Protocol (MCP) süsteemide rakendamiseks, tuginedes uusimale **MCP Spetsifikatsioonile 2025-06-18** ja praegustele tööstusstandarditele. Need tavad käsitlevad nii traditsioonilisi turvalisuse probleeme kui ka MCP juurutustele omaseid AI-spetsiifilisi ohte.

## Olulised Turvanõuded

### Kohustuslikud Turvakontrollid (MUST Nõuded)

1. **Tokenite Valideerimine**: MCP serverid **EI TOHI** aktsepteerida ühtegi tokenit, mis pole MCP serveri jaoks spetsiaalselt välja antud.
2. **Autoriseerimise Kontroll**: MCP serverid, mis rakendavad autoriseerimist, **PEAVAD** kontrollima KÕIKI sissetulevaid päringuid ja **EI TOHI** kasutada sessioone autentimiseks.
3. **Kasutaja Nõusolek**: MCP proxy-serverid, mis kasutavad staatilisi kliendi ID-sid, **PEAVAD** saama kasutajalt selgesõnalise nõusoleku iga dünaamiliselt registreeritud kliendi jaoks.
4. **Turvalised Sessiooni ID-d**: MCP serverid **PEAVAD** kasutama krüptograafiliselt turvalisi, mitte-deterministlikke sessiooni ID-sid, mis on genereeritud turvaliste juhuslike arvugeneraatoritega.

## Põhilised Turvalisuse Tavad

### 1. Sisendi Valideerimine ja Puhastamine
- **Põhjalik Sisendi Valideerimine**: Valideeri ja puhasta kõik sisendid, et vältida injektsioonirünnakuid, segadusse ajamise probleeme ja prompt-injektsiooni haavatavusi.
- **Parameetrite Skeemi Jõustamine**: Rakenda ranget JSON-skeemi valideerimist kõigi tööriistade parameetrite ja API sisendite jaoks.
- **Sisu Filtreerimine**: Kasuta Microsoft Prompt Shields ja Azure Content Safety't pahatahtliku sisu filtreerimiseks promptides ja vastustes.
- **Väljundi Puhastamine**: Valideeri ja puhasta kõik mudeli väljundid enne nende esitamist kasutajatele või allavoolu süsteemidele.

### 2. Autentimise ja Autoriseerimise Täiustamine
- **Välised Identiteedipakkujad**: Delegeeri autentimine tuntud identiteedipakkujatele (Microsoft Entra ID, OAuth 2.1 pakkujad) selle asemel, et rakendada kohandatud autentimist.
- **Peenhäälestatud Õigused**: Rakenda tööriistapõhiseid õigusi, järgides minimaalse privileegi põhimõtet.
- **Tokenite Elutsükli Haldamine**: Kasuta lühiajalisi juurdepääsutokene koos turvalise rotatsiooni ja õige sihtrühma valideerimisega.
- **Mitmefaktoriline Autentimine**: Nõua MFA-d kõigi administratiivsete juurdepääsude ja tundlike toimingute jaoks.

### 3. Turvalised Kommunikatsiooniprotokollid
- **Transport Layer Security**: Kasuta HTTPS/TLS 1.3 kõigi MCP kommunikatsioonide jaoks koos korrektse sertifikaadi valideerimisega.
- **Lõpuni Krüpteerimine**: Rakenda täiendavaid krüpteerimiskihte väga tundlike andmete edastamiseks ja salvestamiseks.
- **Sertifikaadi Haldamine**: Säilita korrektne sertifikaadi elutsükli haldamine koos automatiseeritud uuendamisprotsessidega.
- **Protokolli Versiooni Jõustamine**: Kasuta praegust MCP protokolli versiooni (2025-06-18) koos korrektse versiooni läbirääkimisega.

### 4. Täiustatud Kiiruse Piiramine ja Ressursside Kaitse
- **Mitmekihiline Kiiruse Piiramine**: Rakenda kiiruse piiramine kasutaja, sessiooni, tööriista ja ressursi tasemel, et vältida kuritarvitamist.
- **Adaptiivne Kiiruse Piiramine**: Kasuta masinõppel põhinevat kiiruse piiramist, mis kohandub kasutusmustrite ja ohunäitajatega.
- **Ressursikvootide Haldamine**: Sea sobivad piirangud arvutusressurssidele, mälukasutusele ja täitmisaegadele.
- **DDoS Kaitse**: Rakenda põhjalikku DDoS kaitset ja liikluse analüüsi süsteeme.

### 5. Põhjalik Logimine ja Jälgimine
- **Struktureeritud Auditilogimine**: Rakenda üksikasjalikke, otsitavaid logisid kõigi MCP toimingute, tööriistade täitmiste ja turvaintsidentide jaoks.
- **Reaalajas Turvamonitooring**: Kasuta SIEM süsteeme koos AI-põhise anomaaliatuvastusega MCP töökoormuste jaoks.
- **Privaatsust Austav Logimine**: Logi turvaintsidente, järgides samal ajal andmete privaatsusnõudeid ja regulatsioone.
- **Intsidentide Reageerimise Integratsioon**: Ühenda logimissüsteemid automatiseeritud intsidentide reageerimise töövoogudega.

### 6. Täiustatud Turvalise Salvestamise Tavad
- **Riistvaralised Turvamoodulid**: Kasuta HSM-toega võtmesalvestust (Azure Key Vault, AWS CloudHSM) kriitiliste krüptograafiliste toimingute jaoks.
- **Krüpteerimisvõtmete Haldamine**: Rakenda korrektset võtmete rotatsiooni, eraldamist ja juurdepääsukontrolli.
- **Saladuste Haldamine**: Hoia kõiki API võtmeid, tokeneid ja mandaate spetsiaalsetes saladuste haldamise süsteemides.
- **Andmete Klassifitseerimine**: Klassifitseeri andmed tundlikkuse tasemete järgi ja rakenda sobivaid kaitsemeetmeid.

### 7. Täiustatud Tokenite Haldamine
- **Tokenite Edastamise Keeld**: Keela selgesõnaliselt tokenite edastamise mustrid, mis mööduvad turvakontrollidest.
- **Sihtrühma Valideerimine**: Kontrolli alati, et tokeni sihtrühma väited vastaksid MCP serveri identiteedile.
- **Väidetepõhine Autoriseerimine**: Rakenda peenhäälestatud autoriseerimist, mis põhineb tokeni väidetel ja kasutaja atribuutidel.
- **Tokenite Sidumine**: Seo tokenid konkreetsete sessioonide, kasutajate või seadmetega, kui see on asjakohane.

### 8. Turvaline Sessioonihaldus
- **Krüptograafilised Sessiooni ID-d**: Genereeri sessiooni ID-d, kasutades krüptograafiliselt turvalisi juhuslike arvugeneraatoreid (mitte ennustatavaid järjestusi).
- **Kasutajaspetsiifiline Sidumine**: Seo sessiooni ID-d kasutajaspetsiifilise teabega, kasutades turvalisi formaate nagu `<user_id>:<session_id>`.
- **Sessiooni Elutsükli Kontrollid**: Rakenda korrektset sessiooni aegumist, rotatsiooni ja tühistamise mehhanisme.
- **Sessiooni Turvapealkirjad**: Kasuta sobivaid HTTP turvapealkirju sessiooni kaitseks.

### 9. AI-spetsiifilised Turvakontrollid
- **Prompt-injektsiooni Kaitse**: Kasuta Microsoft Prompt Shields'i koos esiletõstmise, piirajate ja andmemärgistamise tehnikatega.
- **Tööriista Mürgitamise Ennetamine**: Valideeri tööriista metaandmed, jälgi dünaamilisi muutusi ja kontrolli tööriista terviklikkust.
- **Mudeli Väljundi Valideerimine**: Skaneeri mudeli väljundeid võimaliku andmelekkimise, kahjuliku sisu või turvapoliitika rikkumiste osas.
- **Konteksti Akna Kaitse**: Rakenda kontrollid, et vältida konteksti akna mürgitamist ja manipuleerimisrünnakuid.

### 10. Tööriista Täitmise Turvalisus
- **Täitmise Sandboxing**: Käivita tööriista täitmised konteinerites, isoleeritud keskkondades koos ressursipiirangutega.
- **Privileegide Eraldamine**: Käivita tööriistad minimaalsete vajalike privileegidega ja eraldi teenusekontodega.
- **Võrgu Isoleerimine**: Rakenda tööriista täitmiskeskkondade võrgu segmentatsiooni.
- **Täitmise Jälgimine**: Jälgi tööriista täitmist anomaalse käitumise, ressursikasutuse ja turvarikkumiste osas.

### 11. Pidev Turvalisuse Valideerimine
- **Automatiseeritud Turvatestimine**: Integreeri turvatestimine CI/CD torustikesse, kasutades tööriistu nagu GitHub Advanced Security.
- **Haavatavuste Haldamine**: Skaneeri regulaarselt kõiki sõltuvusi, sealhulgas AI mudeleid ja väliseid teenuseid.
- **Penetratsioonitestimine**: Teosta regulaarselt turvaauditeid, mis on suunatud MCP rakendustele.
- **Turvakoodi Ülevaatused**: Rakenda kohustuslikke turvaülevaatusi kõigi MCP-ga seotud koodimuudatuste jaoks.

### 12. AI Tarneahela Turvalisus
- **Komponentide Kontrollimine**: Kontrolli kõigi AI komponentide (mudelid, embeddingud, API-d) päritolu, terviklikkust ja turvalisust.
- **Sõltuvuste Haldamine**: Säilita ajakohased inventuurid kõigi tarkvara ja AI sõltuvuste kohta koos haavatavuste jälgimisega.
- **Usaldusväärsed Repositooriumid**: Kasuta kontrollitud ja usaldusväärseid allikaid kõigi AI mudelite, teekide ja tööriistade jaoks.
- **Tarneahela Jälgimine**: Jälgi pidevalt AI teenusepakkujate ja mudelirepositooriumide kompromisside osas.

## Täiustatud Turvamustrid

### Nullusalduse Arhitektuur MCP jaoks
- **Ära Usalda, Kontrolli Alati**: Rakenda pidevat kontrollimist kõigi MCP osalejate jaoks.
- **Mikrosegmentatsioon**: Isoleeri MCP komponendid granuleeritud võrgu- ja identiteedikontrollidega.
- **Tingimuslik Juurdepääs**: Rakenda riskipõhiseid juurdepääsukontrolle, mis kohanduvad konteksti ja käitumisega.
- **Pidev Riskihindamine**: Hinda dünaamiliselt turvaseisundit, tuginedes praegustele ohunäitajatele.

### Privaatsust Säilitav AI Rakendamine
- **Andmete Minimeerimine**: Paljasta iga MCP toimingu jaoks ainult minimaalne vajalik andmehulk.
- **Diferentsiaalne Privaatsus**: Rakenda privaatsust säilitavaid tehnikaid tundlike andmete töötlemiseks.
- **Homomorfne Krüpteerimine**: Kasuta täiustatud krüpteerimistehnikaid turvaliseks arvutamiseks krüpteeritud andmetel.
- **Federatiivne Õppimine**: Rakenda hajutatud õppemeetodeid, mis säilitavad andmete lokaalsuse ja privaatsuse.

### Intsidentidele Reageerimine AI Süsteemides
- **AI-spetsiifilised Intsidentide Protseduurid**: Arenda intsidentidele reageerimise protseduure, mis on kohandatud AI ja MCP-spetsiifilistele ohtudele.
- **Automatiseeritud Reageerimine**: Rakenda automatiseeritud ohjeldamist ja parandamist tavaliste AI turvaintsidentide jaoks.
- **Kohtuekspertiisi Võimekus**: Säilita kohtuekspertiisi valmisolek AI süsteemide kompromisside ja andmelekkimiste jaoks.
- **Taastamisprotseduurid**: Loo protseduurid AI mudeli mürgitamise, prompt-injektsiooni rünnakute ja teenuse kompromisside taastamiseks.

## Rakendamise Ressursid ja Standardid

### Ametlik MCP Dokumentatsioon
- [MCP Spetsifikatsioon 2025-06-18](https://spec.modelcontextprotocol.io/specification/2025-06-18/) - Praegune MCP protokolli spetsifikatsioon
- [MCP Turvalisuse Parimad Tavad](https://modelcontextprotocol.io/specification/2025-06-18/basic/security_best_practices) - Ametlik turvajuhend
- [MCP Autoriseerimise Spetsifikatsioon](https://modelcontextprotocol.io/specification/2025-06-18/basic/authorization) - Autentimise ja autoriseerimise mustrid
- [MCP Transpordi Turvalisus](https://modelcontextprotocol.io/specification/2025-06-18/transports/) - Transpordikihi turvanõuded

### Microsofti Turvalahendused
- [Microsoft Prompt Shields](https://learn.microsoft.com/azure/ai-services/content-safety/concepts/jailbreak-detection) - Täiustatud prompt-injektsiooni kaitse
- [Azure Content Safety](https://learn.microsoft.com/azure/ai-services/content-safety/) - Põhjalik AI sisu filtreerimine
- [Microsoft Entra ID](https://learn.microsoft.com/entra/identity-platform/v2-oauth2-auth-code-flow) - Ettevõtte identiteedi ja juurdepääsu haldamine
- [Azure Key Vault](https://learn.microsoft.com/azure/key-vault/general/basic-concepts) - Turvaline saladuste ja mandaadi haldamine
- [GitHub Advanced Security](https://github.com/security/advanced-security) - Tarneahela ja koodi turvalisuse skaneerimine

### Turvastandardid ja Raamistikud
- [OAuth 2.1 Turvalisuse Parimad Tavad](https://datatracker.ietf.org/doc/html/draft-ietf-oauth-security-topics) - Praegune OAuth turvajuhend
- [OWASP Top 10](https://owasp.org/www-project-top-ten/) - Veebirakenduste turvariskid
- [OWASP Top 10 LLM-dele](https://genai.owasp.org/download/43299/?tmstv=1731900559) - AI-spetsiifilised turvariskid
- [NIST AI Riskihaldamise Raamistik](https://www.nist.gov/itl/ai-risk-management-framework) - Põhjalik AI riskihaldamine
- [ISO 27001:2022](https://www.iso.org/standard/27001) - Infoturbe haldussüsteemid

### Rakendamise Juhendid ja Õpetused
- [Azure API Halduse MCP Autentimise Väravana](https://techcommunity.microsoft.com/blog/integrationsonazureblog/azure-api-management-your-auth-gateway-for-mcp-servers/4402690) - Ettevõtte autentimise mustrid
- [Microsoft Entra ID MCP Serveritega](https://den.dev/blog/mcp-server-auth-entra-id-session/) - Identiteedipakkuja integreerimine
- [Turvalise Tokenite Salvestamise Rakendamine](https://youtu.be/uRdX37EcCwg?si=6fSChs1G4glwXRy2) - Tokenite haldamise parimad tavad
- [Lõpuni Krüpteerimine AI jaoks](https://learn.microsoft.com/azure/architecture/example-scenario/confidential/end-to-end-encryption) - Täiustatud krüpteerimismustrid

### Täiustatud Turvalisuse Ressursid
- [Microsofti Turvalise Arenduse Elutsükkel](https://www.microsoft.com/sdl) - Turvalise arenduse tavad
- [AI Punase Meeskonna Juhend](https://learn.microsoft.com/security/ai-red-team/) - AI-spetsiifiline turvatestimine
- [Ohumudelite Loomine AI Süsteemidele](https://learn.microsoft.com/security/adoption/approach/threats-ai) - AI ohumudelite metoodika
- [Privaatsustehnika AI jaoks](https://www.microsoft.com/security/blog/2021/07/13/microsofts-pet-project-privacy-enhancing-technologies-in-action/) - Privaatsust säilitavad AI tehnikad

### Vastavus ja Juhtimine
- [GDPR Vastavus AI jaoks](https://learn.microsoft.com/compliance/regulatory/gdpr-data-protection-impact-assessments) - Privaatsusnõuete järgimine AI süsteemides
- [AI Juhtimise Raamistik](https://learn.microsoft.com/azure/architecture/guide/responsible-ai/responsible-ai-overview) - Vastutustundliku AI rakendamine
- [SOC 2 AI Teenustele](https://learn.microsoft.com/compliance/regulatory/offering-soc) - Turvakontrollid AI teenusepakkujatele
- [HIPAA Vastavus AI jaoks](https://learn.microsoft.com/compliance/regulatory/offering-hipaa-hitech) - Tervishoiu AI vastavusnõuded

### DevSecOps ja Automatiseerimine
- [DevSecOps Torustik AI jaoks](https://learn.microsoft.com/azure/devops
- **Tööriistade arendamine**: Arendada ja jagada turvatööriistu ja -teeke MCP ökosüsteemi jaoks

---

*See dokument kajastab MCP turvalisuse parimaid tavasid seisuga 18. august 2025, tuginedes MCP spetsifikatsioonile 2025-06-18. Turvatavasid tuleks regulaarselt üle vaadata ja uuendada, kuna protokoll ja ohumaastik arenevad.*

---

**Lahtiütlus**:  
See dokument on tõlgitud AI tõlketeenuse [Co-op Translator](https://github.com/Azure/co-op-translator) abil. Kuigi püüame tagada täpsust, palume arvestada, et automaatsed tõlked võivad sisaldada vigu või ebatäpsusi. Algne dokument selle algses keeles tuleks pidada autoriteetseks allikaks. Olulise teabe puhul soovitame kasutada professionaalset inimtõlget. Me ei vastuta selle tõlke kasutamisest tulenevate arusaamatuste või valesti tõlgenduste eest.