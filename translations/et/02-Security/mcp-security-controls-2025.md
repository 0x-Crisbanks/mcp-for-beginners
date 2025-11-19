<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "0c243c6189393ed7468e470ef2090049",
  "translation_date": "2025-10-11T12:07:16+00:00",
  "source_file": "02-Security/mcp-security-controls-2025.md",
  "language_code": "et"
}
-->
# MCP Turvakontrollid - August 2025 Uuendus

> **Kehtiv standard**: See dokument kajastab [MCP Spetsifikatsioon 2025-06-18](https://spec.modelcontextprotocol.io/specification/2025-06-18/) turvanõudeid ja ametlikke [MCP Turvalisuse Parimaid Tavasid](https://modelcontextprotocol.io/specification/2025-06-18/basic/security_best_practices).

Model Context Protocol (MCP) on oluliselt arenenud, pakkudes täiustatud turvakontrolle, mis käsitlevad nii traditsioonilisi tarkvara turvaprobleeme kui ka tehisintellekti spetsiifilisi ohte. See dokument sisaldab põhjalikke turvakontrolle MCP turvaliseks rakendamiseks seisuga august 2025.

## **KOHUSTUSLIKUD Turvanõuded**

### **Olulised keelud MCP spetsifikatsioonist:**

> **KEELATUD**: MCP serverid **EI TOHI** aktsepteerida ühtegi tokenit, mida MCP serverile pole selgesõnaliselt väljastatud  
>
> **KEELATUD**: MCP serverid **EI TOHI** kasutada seansse autentimiseks  
>
> **NÕUTUD**: Autoriseerimist rakendavad MCP serverid **PEAVAD** kontrollima KÕIKI sissetulevaid päringuid  
>
> **KOHUSTUSLIK**: MCP puhverserverid, mis kasutavad staatilisi kliendi ID-sid, **PEAVAD** saama kasutaja nõusoleku iga dünaamiliselt registreeritud kliendi jaoks  

---

## 1. **Autentimise ja Autoriseerimise Kontrollid**

### **Välise identiteedipakkuja integreerimine**

**Kehtiv MCP standard (2025-06-18)** lubab MCP serveritel delegeerida autentimise välistele identiteedipakkujatele, mis on oluline turvalisuse täiustus:

**Turvalisuse eelised:**
1. **Välistab kohandatud autentimise riskid**: Vähendab haavatavuste pinda, vältides kohandatud autentimislahendusi  
2. **Ettevõtte tasemel turvalisus**: Kasutab selliseid väljakujunenud identiteedipakkujaid nagu Microsoft Entra ID, millel on täiustatud turvafunktsioonid  
3. **Keskne identiteedihaldus**: Lihtsustab kasutajate elutsükli haldamist, juurdepääsukontrolli ja vastavusauditit  
4. **Mitmefaktoriline autentimine**: Pärib MFA võimalused ettevõtte identiteedipakkujatelt  
5. **Tingimuslikud juurdepääsueeskirjad**: Kasu riskipõhisest juurdepääsukontrollist ja kohanduvast autentimisest  

**Rakendamise nõuded:**
- **Tokeni sihtrühma valideerimine**: Kontrollige, et kõik tokenid oleksid selgesõnaliselt MCP serverile väljastatud  
- **Väljastaja valideerimine**: Kontrollige, et tokeni väljastaja vastab oodatud identiteedipakkujale  
- **Allkirja valideerimine**: Tokeni terviklikkuse krüptograafiline kontroll  
- **Aegumise jõustamine**: Tokeni kehtivusaja rangelt järgimine  
- **Õiguste valideerimine**: Veenduge, et tokenid sisaldavad sobivaid õigusi taotletud toimingute jaoks  

### **Autoriseerimisloogika turvalisus**

**Olulised kontrollid:**
- **Põhjalikud autoriseerimisauditid**: Kõigi autoriseerimisotsuste punktide regulaarne turvakontroll  
- **Ohutud vaikeseaded**: Juurdepääsu keelamine, kui autoriseerimisloogika ei suuda teha kindlat otsust  
- **Õiguste piirid**: Selge eristus erinevate privileegitasemete ja ressursside juurdepääsu vahel  
- **Auditilogid**: Kõigi autoriseerimisotsuste täielik logimine turvaseireks  
- **Regulaarsed juurdepääsukontrollid**: Kasutajaõiguste ja privileegide määramiste perioodiline valideerimine  

## 2. **Tokenite Turvalisus ja Läbivoolu Tõkestamine**

### **Tokenite läbivoolu ennetamine**

**Tokenite läbivool on MCP autoriseerimise spetsifikatsioonis selgesõnaliselt keelatud** kriitiliste turvariskide tõttu:

**Käsitletavad turvariskid:**
- **Kontrolli vältimine**: Väldib olulisi turvakontrolle, nagu päringute valideerimine ja liikluse jälgimine  
- **Vastutuse kadumine**: Muudab kliendi tuvastamise võimatuks, kahjustades auditijälgi ja intsidentide uurimist  
- **Puhverserveri kaudu andmete lekkimine**: Võimaldab pahatahtlikel osapooltel kasutada servereid volitamata andmetele juurdepääsuks  
- **Usalduspiiride rikkumine**: Rikub alluvate teenuste usaldusväärsuse eeldusi tokenite päritolu kohta  
- **Külgmine liikumine**: Kompromiteeritud tokenid mitmes teenuses võimaldavad laiemat rünnakute ulatust  

**Rakendamise kontrollid:**
```yaml
Token Validation Requirements:
  audience_validation: MANDATORY
  issuer_verification: MANDATORY  
  signature_check: MANDATORY
  expiration_enforcement: MANDATORY
  scope_validation: MANDATORY
  
Token Lifecycle Management:
  rotation_frequency: "Short-lived tokens preferred"
  secure_storage: "Azure Key Vault or equivalent"
  transmission_security: "TLS 1.3 minimum"
  replay_protection: "Implemented via nonce/timestamp"
```

### **Turvalised tokenite haldamise mustrid**

**Parimad tavad:**
- **Lühiajalised tokenid**: Minimeerige kokkupuuteaega sagedase tokenite uuendamisega  
- **Õigeaegne väljastamine**: Väljastage tokenid ainult siis, kui neid on konkreetsete toimingute jaoks vaja  
- **Turvaline salvestamine**: Kasutage riistvaralisi turvamooduleid (HSM) või turvalisi võtmehoidlaid  
- **Tokenite sidumine**: Siduge tokenid konkreetsete klientide, seansside või toimingutega, kui võimalik  
- **Jälgimine ja hoiatused**: Reaalajas tokenite väärkasutuse või volitamata juurdepääsumustrite tuvastamine  

## 3. **Seansi Turvakontrollid**

### **Seansi kaaperdamise ennetamine**

**Käsitletavad ründevektorid:**
- **Seansi kaaperdamise süstimine**: Pahatahtlike sündmuste süstimine jagatud seansi olekusse  
- **Seansi identiteedi vargus**: Varastatud seansi ID-de volitamata kasutamine autentimise vältimiseks  
- **Jätkatavad vooguründed**: Serveri saadetud sündmuste jätkamise ärakasutamine pahatahtliku sisu süstimiseks  

**Kohustuslikud seansikontrollid:**
```yaml
Session ID Generation:
  randomness_source: "Cryptographically secure RNG"
  entropy_bits: 128 # Minimum recommended
  format: "Base64url encoded"
  predictability: "MUST be non-deterministic"

Session Binding:
  user_binding: "REQUIRED - <user_id>:<session_id>"
  additional_identifiers: "Device fingerprint, IP validation"
  context_binding: "Request origin, user agent validation"
  
Session Lifecycle:
  expiration: "Configurable timeout policies"
  rotation: "After privilege escalation events"
  invalidation: "Immediate on security events"
  cleanup: "Automated expired session removal"
```

**Transpordi turvalisus:**
- **HTTPS-i jõustamine**: Kõik seanssidega seotud suhtlus TLS 1.3 kaudu  
- **Turvalised küpsise atribuudid**: HttpOnly, Secure, SameSite=Strict  
- **Sertifikaatide kinnitamine**: Kriitiliste ühenduste jaoks, et vältida MITM-rünnakuid  

### **Seisundipõhised vs seisundivabad kaalutlused**

**Seisundipõhiste rakenduste puhul:**
- Jagatud seansi olek vajab täiendavat kaitset süstimisrünnakute vastu  
- Järjekorrapõhine seansihaldus vajab terviklikkuse kontrolli  
- Mitme serveri eksemplari korral on vajalik turvaline seansi oleku sünkroniseerimine  

**Seisundivabade rakenduste puhul:**
- JWT või sarnane tokenipõhine seansihaldus  
- Seansi oleku terviklikkuse krüptograafiline kontroll  
- Vähendatud ründevektor, kuid nõuab tugevat tokenite valideerimist  

## 4. **Tehisintellekti-spetsiifilised Turvakontrollid**

### **Prompt Injection'i kaitse**

**Microsoft Prompt Shields integratsioon:**
```yaml
Detection Mechanisms:
  - "Advanced ML-based instruction detection"
  - "Contextual analysis of external content"
  - "Real-time threat pattern recognition"
  
Protection Techniques:
  - "Spotlighting trusted vs untrusted content"
  - "Delimiter systems for content boundaries"  
  - "Data marking for content source identification"
  
Integration Points:
  - "Azure Content Safety service"
  - "Real-time content filtering"
  - "Threat intelligence updates"
```

**Rakendamise kontrollid:**
- **Sisendi sanitiseerimine**: Kõigi kasutajasisendite põhjalik valideerimine ja filtreerimine  
- **Sisu piiride määratlemine**: Selge eristus süsteemi juhiste ja kasutaja sisu vahel  
- **Juhiste hierarhia**: Õiged prioriteedireeglid vastuoluliste juhiste jaoks  
- **Väljundi jälgimine**: Potentsiaalselt kahjulike või manipuleeritud väljundite tuvastamine  

### **Tööriistade mürgitamise ennetamine**

**Tööriistade turvaraamistik:**
```yaml
Tool Definition Protection:
  validation:
    - "Schema validation against expected formats"
    - "Content analysis for malicious instructions" 
    - "Parameter injection detection"
    - "Hidden instruction identification"
  
  integrity_verification:
    - "Cryptographic hashing of tool definitions"
    - "Digital signatures for tool packages"
    - "Version control with change auditing"
    - "Tamper detection mechanisms"
  
  monitoring:
    - "Real-time change detection"
    - "Behavioral analysis of tool usage"
    - "Anomaly detection for execution patterns"
    - "Automated alerting for suspicious modifications"
```

**Dünaamiline tööriistade haldamine:**
- **Kinnitamise töövood**: Kasutaja selgesõnaline nõusolek tööriistade muudatuste jaoks  
- **Tagasipööramise võimalused**: Võimalus naasta eelmiste tööriistade versioonide juurde  
- **Muutuste audit**: Tööriistade määratluste muudatuste täielik ajalugu  
- **Riskihindamine**: Tööriistade turvaseisundi automaatne hindamine  

## 5. **Segadusse aetud asendaja rünnakute ennetamine**

### **OAuth Puhverserveri Turvalisus**

**Rünnakute ennetamise kontrollid:**
```yaml
Client Registration:
  static_client_protection:
    - "Explicit user consent for dynamic registration"
    - "Consent bypass prevention mechanisms"  
    - "Cookie-based consent validation"
    - "Redirect URI strict validation"
    
  authorization_flow:
    - "PKCE implementation (OAuth 2.1)"
    - "State parameter validation"
    - "Authorization code binding"
    - "Nonce verification for ID tokens"
```

**Rakendamise nõuded:**
- **Kasutaja nõusoleku kontrollimine**: Ärge kunagi jätke nõusoleku ekraane vahele dünaamilise kliendi registreerimise jaoks  
- **Ümbersuunamise URI valideerimine**: Ümbersuunamise sihtkohtade range lubatud nimekirja põhine valideerimine  
- **Autoriseerimiskoodi kaitse**: Lühiajalised koodid ühekordse kasutamise jõustamisega  
- **Kliendi identiteedi kontrollimine**: Kliendi mandaadi ja metaandmete tugev valideerimine  

## 6. **Tööriistade Käivitamise Turvalisus**

### **Liivakastimine ja Isolatsioon**

**Konteineripõhine isolatsioon:**
```yaml
Execution Environment:
  containerization: "Docker/Podman with security profiles"
  resource_limits:
    cpu: "Configurable CPU quotas"
    memory: "Memory usage restrictions"
    disk: "Storage access limitations"
    network: "Network policy enforcement"
  
  privilege_restrictions:
    user_context: "Non-root execution mandatory"
    capability_dropping: "Remove unnecessary Linux capabilities"
    syscall_filtering: "Seccomp profiles for syscall restriction"
    filesystem: "Read-only root with minimal writable areas"
```

**Protsessi isolatsioon:**
- **Eraldi protsessikontekstid**: Iga tööriista käivitamine isoleeritud protsessiruumis  
- **Protsessidevaheline suhtlus**: Turvalised IPC-mehhanismid valideerimisega  
- **Protsesside jälgimine**: Käitumise analüüs ja anomaaliate tuvastamine reaalajas  
- **Ressursside piiramine**: Rangelt määratletud piirangud CPU, mälu ja I/O operatsioonidele  

### **Minimaalse privileegi rakendamine**

**Õiguste haldamine:**
```yaml
Access Control:
  file_system:
    - "Minimal required directory access"
    - "Read-only access where possible"
    - "Temporary file cleanup automation"
    
  network_access:
    - "Explicit allowlist for external connections"
    - "DNS resolution restrictions" 
    - "Port access limitations"
    - "SSL/TLS certificate validation"
  
  system_resources:
    - "No administrative privilege elevation"
    - "Limited system call access"
    - "No hardware device access"
    - "Restricted environment variable access"
```

## 7. **Tarneahela Turvakontrollid**

### **Sõltuvuste kontrollimine**

**Komponentide põhjalik turvalisus:**
```yaml
Software Dependencies:
  scanning: 
    - "Automated vulnerability scanning (GitHub Advanced Security)"
    - "License compliance verification"
    - "Known vulnerability database checks"
    - "Malware detection and analysis"
  
  verification:
    - "Package signature verification"
    - "Checksum validation"
    - "Provenance attestation"
    - "Software Bill of Materials (SBOM)"

AI Components:
  model_verification:
    - "Model provenance validation"
    - "Training data source verification" 
    - "Model behavior testing"
    - "Adversarial robustness assessment"
  
  service_validation:
    - "Third-party API security assessment"
    - "Service level agreement review"
    - "Data handling compliance verification"
    - "Incident response capability evaluation"
```

### **Pidev jälgimine**

**Tarneahela ohtude tuvastamine:**
- **Sõltuvuste tervise jälgimine**: Kõigi sõltuvuste pidev hindamine turvaprobleemide osas  
- **Ohuluure integreerimine**: Reaalajas uuendused tekkivate tarneahela ohtude kohta  
- **Käitumise analüüs**: Väliste komponentide ebatavalise käitumise tuvastamine  
- **Automaatne reageerimine**: Kompromiteeritud komponentide viivitamatu isoleerimine  

## 8. **Jälgimis- ja Tuvastamiskontrollid**

### **Turvateabe ja sündmuste haldamine (SIEM)**

**Põhjalik logimisstrateegia:**
```yaml
Authentication Events:
  - "All authentication attempts (success/failure)"
  - "Token issuance and validation events"
  - "Session creation, modification, termination"
  - "Authorization decisions and policy evaluations"

Tool Execution:
  - "Tool invocation details and parameters"
  - "Execution duration and resource usage"
  - "Output generation and content analysis"
  - "Error conditions and exception handling"

Security Events:
  - "Potential prompt injection attempts"
  - "Tool poisoning detection events"
  - "Session hijacking indicators"
  - "Unusual access patterns and anomalies"
```

### **Reaalajas ohtude tuvastamine**

**Käitumisanalüüs:**
- **Kasutaja käitumise analüüs (UBA)**: Ebatavaliste kasutaja juurdepääsumustrite tuvastamine  
- **Entiteedi käitumise analüüs (EBA)**: MCP serveri ja tööriistade käitumise jälgimine  
- **Masinõppe anomaaliate tuvastamine**: Tehisintellektil põhinev turvaohtude tuvastamine  
- **Ohuluure korrelatsioon**: Täheldatud tegevuste vastavusse viimine teadaolevate rünnakumustritega  

## 9. **Intsidentidele Reageerimine ja Taastumine**

### **Automatiseeritud Reageerimisvõimalused**

**Kohesed reageerimistoimingud:**
```yaml
Threat Containment:
  session_management:
    - "Immediate session termination"
    - "Account lockout procedures"
    - "Access privilege revocation"
  
  system_isolation:
    - "Network segmentation activation"
    - "Service isolation protocols"
    - "Communication channel restriction"

Recovery Procedures:
  credential_rotation:
    - "Automated token refresh"
    - "API key regeneration"
    - "Certificate renewal"
  
  system_restoration:
    - "Clean state restoration"
    - "Configuration rollback"
    - "Service restart procedures"
```

### **Kohtuekspertiisi võimalused**

**Uurimise tugi:**
- **Auditijälgede säilitamine**: Muutumatud logid krüptograafilise terviklikkusega  
- **Tõendite kogumine**: Asjakohaste turvaelementide automaatne kogumine  
- **Ajajoone rekonstrueerimine**: Turvaintsidentidele eelnenud sündmuste üksikasjalik järjestus  
- **Mõjude hindamine**: Kompromissi ulatuse ja andmete lekkimise hindamine  

## **Peamised Turvaarhitektuuri Põhimõtted**

### **Sügavuskaitse**
- **Mitmekihiline turvalisus**: Turvaarhitektuuris ei tohi olla ühtegi nõrka lüli  
- **Redundantsed kontrollid**: Kriitiliste funktsioonide kattuvad turvameetmed  
- **Ohutud mehhanismid**: Turvalised vaikeseaded süsteemivigade või rünnakute korral  

### **Nullusalduse rakendamine**
- **Ära kunagi usalda, kontrolli alati**: Kõigi üksuste ja päringute pidev valideerimine  
- **Minimaalse privileegi põhimõte**: Kõigile komponentidele minimaalne juurdepääsuõigus  
- **Mikrosegmenteerimine**: Granulaarne võrgu- ja juurdepääsukontroll  

### **Pidev turvalisuse areng**
- **Ohuolukorra kohandamine**: Regulaarne uuendamine tekkivate ohtude käsitlemiseks  
- **Turvakontrollide tõhusus**: Kontrollide pidev hindamine ja täiustamine  
- **Spetsifikatsiooni järgimine**: Vastavus arenevatele MCP turvastandarditele  

---

## **Rakendamise Ressursid**

### **Ametlik MCP dokumentatsioon**
- [MCP Spetsifikatsioon (2025-06-18)](https://spec.modelcontextprotocol.io/specification/2025-06-18/)  
- [MCP Turvalisuse Parimad Tavad](https://modelcontextprotocol.io/specification/2025-06-18/basic/security_best_practices)  
- [MCP Autoriseerimise Spetsifikatsioon](https://modelcontextprotocol.io/specification/2025-06-18/basic/authorization)  

### **Microsofti Turvalahendused**
- [Microsoft Prompt Shields](https://learn.microsoft.com/azure/ai-services/content-safety/concepts/jailbreak-detection)  
- [Azure Content Safety](https://learn.microsoft.com/azure/ai-services/content-safety/)  
- [GitHub Advanced Security](https://github.com/security/advanced-security)  
- [Azure Key Vault](https://learn.microsoft.com/azure/key-vault/)  

### **Turvastandardid**
- [OAuth 2.0 Turvalisuse Parimad Tavad (RFC 9700)](https://datatracker.ietf.org/doc/html/rfc9700)  
- [OWASP Top 10 Suurte Keeltemudelite jaoks](https://genai.owasp.org/)  
- [NIST Küberjulgeoleku Raamistik](https://www.nist.gov/cyberframework)  

---

> **Oluline**: Need turvakontrollid kajastavad kehtivat MCP spetsifikatsiooni (2025-06-18). Kontrollige alati [ametliku dokumentatsiooni](https://spec.modelcontextprotocol.io/) vastu, kuna standardid arenevad kiiresti.

---

**Lahtiütlus**:  
See dokument on tõlgitud AI tõlketeenuse [Co-op Translator](https://github.com/Azure/co-op-translator) abil. Kuigi püüame tagada täpsust, palume arvestada, et automaatsed tõlked võivad sisaldada vigu või ebatäpsusi. Algne dokument selle algses keeles tuleks pidada autoriteetseks allikaks. Olulise teabe puhul soovitame kasutada professionaalset inimtõlget. Me ei vastuta selle tõlke kasutamisest tulenevate arusaamatuste või valesti tõlgenduste eest.