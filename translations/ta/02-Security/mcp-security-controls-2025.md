<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "0c243c6189393ed7468e470ef2090049",
  "translation_date": "2025-10-11T12:06:39+00:00",
  "source_file": "02-Security/mcp-security-controls-2025.md",
  "language_code": "ta"
}
-->
# MCP பாதுகாப்பு கட்டுப்பாடுகள் - ஆகஸ்ட் 2025 புதுப்பிப்பு

> **தற்போதைய தரநிலை**: இந்த ஆவணம் [MCP Specification 2025-06-18](https://spec.modelcontextprotocol.io/specification/2025-06-18/) பாதுகாப்பு தேவைகள் மற்றும் அதிகாரப்பூர்வ [MCP Security Best Practices](https://modelcontextprotocol.io/specification/2025-06-18/basic/security_best_practices) ஆகியவற்றை பிரதிபலிக்கிறது.

மாடல் கான்டெக்ஸ்ட் புரோட்டோகால் (MCP) பாரம்பரிய மென்பொருள் பாதுகாப்பு மற்றும் AI-சார்ந்த அச்சுறுத்தல்களைத் தீர்க்க மேம்பட்ட பாதுகாப்பு கட்டுப்பாடுகளுடன் குறிப்பிடத்தக்க முறையில் வளர்ந்துள்ளது. ஆகஸ்ட் 2025 நிலவரப்படி பாதுகாப்பான MCP செயல்பாடுகளுக்கான விரிவான பாதுகாப்பு கட்டுப்பாடுகளை இந்த ஆவணம் வழங்குகிறது.

## **கட்டாயமான பாதுகாப்பு தேவைகள்**

### **MCP Specification-இல் முக்கியமான தடைச்செயல்கள்:**

> **தடைசெய்யப்பட்டது**: MCP சேவைகள் **MUST NOT** MCP சேவைக்கு வெளிப்படையாக வழங்கப்படாத எந்த டோக்கன்களையும் ஏற்க **கூடாது**  
>
> **தடைசெய்யப்பட்டது**: MCP சேவைகள் **MUST NOT** அங்கீகாரத்திற்காக அமர்வுகளைப் பயன்படுத்த **கூடாது**  
>
> **தேவை**: MCP சேவைகள் அனுமதி செயல்படுத்தும்போது **அனைத்து** உள்ளீட்டு கோரிக்கைகளைச் சரிபார்க்க **வேண்டும்**  
>
> **கட்டாயம்**: நிலையான கிளையன்ட் ஐடிகளைப் பயன்படுத்தும் MCP ப்ராக்ஸி சேவைகள் **ஒவ்வொரு** மாறும் பதிவுசெய்யப்பட்ட கிளையன்டிற்கும் பயனர் ஒப்புதலைப் பெற **வேண்டும்**  

---

## 1. **அங்கீகாரம் மற்றும் அனுமதி கட்டுப்பாடுகள்**

### **வெளியக அடையாள வழங்குநர் ஒருங்கிணைப்பு**

**தற்போதைய MCP தரநிலை (2025-06-18)** MCP சேவைகள் வெளியக அடையாள வழங்குநர்களுக்கு அங்கீகாரத்தை ஒப்படைக்க அனுமதிக்கிறது, இது ஒரு முக்கியமான பாதுகாப்பு மேம்பாட்டை பிரதிபலிக்கிறது:

**பாதுகாப்பு நன்மைகள்:**
1. **தனிப்பயன் அங்கீகார ஆபத்துகளை நீக்குகிறது**: தனிப்பயன் அங்கீகார செயல்பாடுகளைத் தவிர்ப்பதன் மூலம் பாதிப்பு மேற்பரப்பை குறைக்கிறது  
2. **நிறுவன தரநிலை பாதுகாப்பு**: Microsoft Entra ID போன்ற நிறுவனம்-தரநிலை அடையாள வழங்குநர்களின் மேம்பட்ட பாதுகாப்பு அம்சங்களைப் பயன்படுத்துகிறது  
3. **மையகப்படுத்தப்பட்ட அடையாள மேலாண்மை**: பயனர் வாழ்க்கைச்சுழற்சி மேலாண்மை, அணுகல் கட்டுப்பாடு மற்றும் ஒழுங்குமுறை ஆடிட்டிங்கை எளிதாக்குகிறது  
4. **பல காரணி அங்கீகாரம்**: நிறுவனம்-தரநிலை அடையாள வழங்குநர்களின் MFA திறன்களைப் பெறுகிறது  
5. **நிபந்தனை அணுகல் கொள்கைகள்**: ஆபத்து அடிப்படையிலான அணுகல் கட்டுப்பாடுகள் மற்றும் தழுவல் அங்கீகாரத்தின் நன்மைகளைப் பெறுகிறது  

**செயல்படுத்தல் தேவைகள்:**
- **டோக்கன் பார்வையாளரின் சரிபார்ப்பு**: அனைத்து டோக்கன்களும் MCP சேவைக்கு வெளிப்படையாக வழங்கப்பட்டவை என்பதை உறுதிப்படுத்தவும்  
- **வெளியீட்டாளர் சரிபார்ப்பு**: டோக்கன் வெளியீட்டாளர் எதிர்பார்க்கப்படும் அடையாள வழங்குநருடன் பொருந்துவதை உறுதிப்படுத்தவும்  
- **கையொப்ப சரிபார்ப்பு**: டோக்கனின் முழுமையை குறியீட்டு முறையில் சரிபார்க்கவும்  
- **காலாவதி அமலாக்கம்**: டோக்கன் வாழ்க்கை வரம்புகளை கடுமையாக அமலாக்கவும்  
- **வாய்ப்பு சரிபார்ப்பு**: கோரப்பட்ட செயல்பாடுகளுக்கு உரிய அனுமதிகளை டோக்கன்கள் கொண்டிருப்பதை உறுதிப்படுத்தவும்  

### **அனுமதி தர்க்க பாதுகாப்பு**

**முக்கிய கட்டுப்பாடுகள்:**
- **விரிவான அனுமதி ஆடிட்டுகள்**: அனைத்து அனுமதி முடிவு புள்ளிகளின் முறைமையான பாதுகாப்பு மதிப்பீடுகள்  
- **தவறாத இயல்புகள்**: அனுமதி தர்க்கம் தெளிவான முடிவை எடுக்க முடியாதபோது அணுகலை மறுக்கவும்  
- **அனுமதி எல்லைகள்**: வெவ்வேறு சலுகை நிலைகள் மற்றும் வள அணுகலுக்கு இடையிலான தெளிவான பிரிவுகள்  
- **ஆடிட் பதிவு**: பாதுகாப்பு கண்காணிப்புக்கான அனைத்து அனுமதி முடிவுகளின் முழுமையான பதிவு  
- **முறைமையான அணுகல் மதிப்பீடுகள்**: பயனர் அனுமதிகள் மற்றும் சலுகை ஒதுக்கீடுகளின் காலாண்டு சரிபார்ப்பு  

## 2. **டோக்கன் பாதுகாப்பு & பாஸ்த்ரூ கட்டுப்பாடுகள்**

### **டோக்கன் பாஸ்த்ரூ தடுப்பு**

**MCP Authorization Specification**-இல் டோக்கன் பாஸ்த்ரூ **தடையிடப்பட்டுள்ளது** முக்கியமான பாதுகாப்பு ஆபத்துகளால்:

**பாதுகாப்பு ஆபத்துகள் தீர்க்கப்பட்டவை:**
- **கட்டுப்பாட்டு தவிர்ப்பு**: வீத வரம்பு, கோரிக்கை சரிபார்ப்பு மற்றும் போக்குவரத்து கண்காணிப்பு போன்ற முக்கியமான பாதுகாப்பு கட்டுப்பாடுகளை புறக்கணிக்கிறது  
- **கணக்கெடுப்பு சீர்கேடு**: கிளையன்ட் அடையாளத்தை சாத்தியமற்றதாக ஆக்குகிறது, ஆடிட் தடங்கள் மற்றும் சம்பவ விசாரணையை கெடுப்பது  
- **ப்ராக்ஸி அடிப்படையிலான வெளியேற்றம்**: அனுமதியில்லாத தரவினை அணுகுவதற்கான சேவைகளை ப்ராக்ஸியாக பயன்படுத்த தீவிரமான நடிகர்களுக்கு அனுமதிக்கிறது  
- **நம்பகத்தன்மை எல்லை மீறல்கள்**: டோக்கன் மூலங்களைப் பற்றிய கீழ்நிலை சேவை நம்பகத்தன்மை கருதுகோள்களை உடைக்கிறது  
- **பக்கவாட்டு இயக்கம்**: பல சேவைகளில் கெட்டுப்போன டோக்கன்கள் பரந்த தாக்குதல்களை விரிவாக்க அனுமதிக்கிறது  

**செயல்படுத்தல் கட்டுப்பாடுகள்:**
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

### **பாதுகாப்பான டோக்கன் மேலாண்மை முறைமைகள்**

**சிறந்த நடைமுறைகள்:**
- **குறுகிய கால டோக்கன்கள்**: அடிக்கடி டோக்கன் சுழற்சியுடன் வெளிப்பாட்டு சாளரத்தை குறைக்கவும்  
- **தேவையான நேரத்தில் வழங்கல்**: குறிப்பிட்ட செயல்பாடுகளுக்கு மட்டுமே டோக்கன்களை வழங்கவும்  
- **பாதுகாப்பான சேமிப்பு**: ஹார்ட்வேர பாதுகாப்பு தொகுதிகள் (HSMs) அல்லது பாதுகாப்பான விசை களஞ்சியங்களைப் பயன்படுத்தவும்  
- **டோக்கன் பிணைப்பு**: டோக்கன்களை குறிப்பிட்ட கிளையன்ட்கள், அமர்வுகள் அல்லது செயல்பாடுகளுடன் பிணைக்கவும்  
- **கண்காணிப்பு & எச்சரிக்கை**: டோக்கன் தவறான பயன்பாடு அல்லது அனுமதியில்லாத அணுகல் முறைமைகளை நேரடி கண்டறிதல்  

## 3. **அமர்வு பாதுகாப்பு கட்டுப்பாடுகள்**

### **அமர்வு கடத்தல் தடுப்பு**

**தாக்குதல் வழிகள் தீர்க்கப்பட்டவை:**
- **அமர்வு கடத்தல் தூண்டல் செலுத்தல்**: பகிரப்பட்ட அமர்வு நிலைக்கு தீவிரமான நிகழ்வுகளை செலுத்துதல்  
- **அமர்வு போலி அடையாளம்**: திருடப்பட்ட அமர்வு ஐடிகளை அனுமதி தவிர்க்க அனுமதியில்லாத பயன்பாடு  
- **மீண்டும் தொடங்கக்கூடிய ஸ்ட்ரீம் தாக்குதல்கள்**: தீவிரமான உள்ளடக்க செலுத்தலுக்கான சேவையக-அனுப்பிய நிகழ்வு மீளத்தொடர்ச்சியின் சுரண்டல்  

**கட்டாயமான அமர்வு கட்டுப்பாடுகள்:**
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

**போக்குவரத்து பாதுகாப்பு:**
- **HTTPS அமலாக்கம்**: அனைத்து அமர்வு தொடர்புகளும் TLS 1.3 மூலம்  
- **பாதுகாப்பான குக்கி பண்புகள்**: HttpOnly, Secure, SameSite=Strict  
- **சான்றிதழ் பினிங்**: MITM தாக்குதல்களைத் தடுப்பதற்கான முக்கியமான இணைப்புகளுக்கு  

### **நிலையான vs நிலையற்ற கருத்துக்கள்**

**நிலையான செயல்பாடுகளுக்கு:**
- பகிரப்பட்ட அமர்வு நிலை செலுத்தல் தாக்குதல்களுக்கு கூடுதல் பாதுகாப்பு தேவை  
- வரிசை அடிப்படையிலான அமர்வு மேலாண்மை முழுமை சரிபார்ப்பு தேவை  
- பல சேவையக நிகழ்வுகள் பாதுகாப்பான அமர்வு நிலை ஒத்திசைவை தேவை  

**நிலையற்ற செயல்பாடுகளுக்கு:**
- JWT அல்லது இதற்கு இணையான டோக்கன் அடிப்படையிலான அமர்வு மேலாண்மை  
- அமர்வு நிலை முழுமையின் குறியீட்டு சரிபார்ப்பு  
- குறைந்த தாக்குதல் மேற்பரப்பு ஆனால் வலுவான டோக்கன் சரிபார்ப்பு தேவை  

## 4. **AI-சார்ந்த பாதுகாப்பு கட்டுப்பாடுகள்**

### **தூண்டல் செலுத்தல் பாதுகாப்பு**

**Microsoft Prompt Shields ஒருங்கிணைப்பு:**
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

**செயல்படுத்தல் கட்டுப்பாடுகள்:**
- **உள்ளீட்டு சுத்திகரிப்பு**: அனைத்து பயனர் உள்ளீடுகளின் விரிவான சரிபார்ப்பு மற்றும் வடிகட்டி  
- **உள்ளடக்க எல்லை வரையறை**: அமைப்பு வழிகாட்டுதல்கள் மற்றும் பயனர் உள்ளடக்கத்திற்கிடையிலான தெளிவான பிரிவு  
- **வழிகாட்டுதல் மாறுபாடு**: முரண்பட்ட வழிகாட்டுதல்களுக்கு சரியான முன்னுரிமை விதிகள்  
- **வெளியீட்டு கண்காணிப்பு**: தீவிரமான அல்லது மாற்றப்பட்ட வெளியீடுகளை கண்டறிதல்  

### **கருவி விஷம் தடுப்பு**

**கருவி பாதுகாப்பு கட்டமைப்பு:**
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

**மாறும் கருவி மேலாண்மை:**
- **ஒப்புதல் பணிகள்**: கருவி மாற்றங்களுக்கு வெளிப்படையான பயனர் ஒப்புதல்  
- **மீளமைப்பு திறன்கள்**: முந்தைய கருவி பதிப்புகளுக்கு திரும்பும் திறன்  
- **மாற்ற ஆடிட்டிங்**: கருவி வரையறை மாற்றங்களின் முழுமையான வரலாறு  
- **ஆபத்து மதிப்பீடு**: கருவி பாதுகாப்பு நிலையை தானியங்கி மதிப்பீடு  

## 5. **குழப்பமான துணை தாக்குதல் தடுப்பு**

### **OAuth ப்ராக்ஸி பாதுகாப்பு**

**தாக்குதல் தடுப்பு கட்டுப்பாடுகள்:**
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

**செயல்படுத்தல் தேவைகள்:**
- **பயனர் ஒப்புதல் சரிபார்ப்பு**: மாறும் கிளையன்ட் பதிவு ஒப்புதல் திரைகள் எப்போதும் தவிர்க்க **கூடாது**  
- **மறுவழிமாற்ற URI சரிபார்ப்பு**: மறுவழிமாற்ற இடங்களின் கடுமையான வெள்ளைப்பட்டியல் அடிப்படையிலான சரிபார்ப்பு  
- **அங்கீகார குறியீட்டு பாதுகாப்பு**: குறுகிய கால குறியீடுகள் மற்றும் ஒற்றை பயன்பாட்டு அமலாக்கம்  
- **கிளையன்ட் அடையாள சரிபார்ப்பு**: கிளையன்ட் சான்றுகள் மற்றும் மெட்டாடேட்டாவின் வலுவான சரிபார்ப்பு  

## 6. **கருவி செயல்படுத்தல் பாதுகாப்பு**

### **சேமிப்பிடுதல் & தனிமைப்படுத்தல்**

**கண்டெய்னர் அடிப்படையிலான தனிமைப்படுத்தல்:**
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

**செயல்முறை தனிமைப்படுத்தல்:**
- **தனித்த செயல்முறை சூழல்கள்**: ஒவ்வொரு கருவி செயல்பாடும் தனிமைப்படுத்தப்பட்ட செயல்முறை இடத்தில்  
- **செயல்முறை இடையிலான தொடர்பு**: சரிபார்ப்புடன் பாதுகாப்பான IPC முறைகள்  
- **செயல்முறை கண்காணிப்பு**: இயக்க நேரத்தில் நடத்தை பகுப்பாய்வு மற்றும் சீர்கேடு கண்டறிதல்  
- **வள அமலாக்கம்**: CPU, நினைவகம் மற்றும் I/O செயல்பாடுகளின் கடினமான வரம்புகள்  

### **குறைந்த சலுகை செயல்படுத்தல்**

**அனுமதி மேலாண்மை:**
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

## 7. **விநியோக சங்கிலி பாதுகாப்பு கட்டுப்பாடுகள்**

### **இணைப்பு சரிபார்ப்பு**

**விரிவான கூறு பாதுகாப்பு:**
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

### **தொடர்ச்சியான கண்காணிப்பு**

**விநியோக சங்கிலி அச்சுறுத்தல் கண்டறிதல்:**
- **இணைப்பு ஆரோக்கிய கண்காணிப்பு**: அனைத்து இணைப்புகளின் பாதுகாப்பு பிரச்சினைகளுக்கான தொடர்ச்சியான மதிப்பீடு  
- **அச்சுறுத்தல் நுண்ணறிவு ஒருங்கிணைப்பு**: உருவாகும் விநியோக சங்கிலி அச்சுறுத்தல்களுக்கான நேரடி புதுப்பிப்புகள்  
- **நடத்தை பகுப்பாய்வு**: வெளிப்புற கூறுகளில் சீரற்ற நடத்தை கண்டறிதல்  
- **தானியங்கி பதில்**: பாதிக்கப்பட்ட கூறுகளின் உடனடி அடக்குதல்  

## 8. **கண்காணிப்பு & கண்டறிதல் கட்டுப்பாடுகள்**

### **பாதுகாப்பு தகவல் மற்றும் நிகழ்வு மேலாண்மை (SIEM)**

**விரிவான பதிவு உத்தி:**
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

### **நேரடி அச்சுறுத்தல் கண்டறிதல்**

**நடத்தை பகுப்பாய்வு:**
- **பயனர் நடத்தை பகுப்பாய்வு (UBA)**: சீரற்ற பயனர் அணுகல் முறைமைகளை கண்டறிதல்  
- **அமைப்பு நடத்தை பகுப்பாய்வு (EBA)**: MCP சேவைகள் மற்றும் கருவி நடத்தை கண்காணிப்பு  
- **இயந்திர கற்றல் சீர்கேடு கண்டறிதல்**: AI மூலம் இயக்கப்படும் பாதுகாப்பு அச்சுறுத்தல்களின் அடையாளம்  
- **அச்சுறுத்தல் நுண்ணறிவு தொடர்பு**: காணப்பட்ட செயல்பாடுகளை அறியப்பட்ட தாக்குதல் முறைமைகளுடன் பொருத்துதல்  

## 9. **சம்பவ பதில் & மீட்பு**

### **தானியங்கி பதில் திறன்கள்**

**உடனடி பதில் நடவடிக்கைகள்:**
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

### **நீதியியல் திறன்கள்**

**விசாரணை ஆதரவு:**
- **ஆடிட்டிங் தடம் பாதுகாப்பு**: குறியீட்டு முழுமையுடன் மாற்றமில்லாத பதிவு  
- **ஆதார சேகரிப்பு**: தொடர்புடைய பாதுகாப்பு பொருட்களை தானியங்கி சேகரித்தல்  
- **காலவரிசை மீளமைப்பு**: பாதுகாப்பு சம்பவங்களுக்கு வழிவகுக்கும் நிகழ்வுகளின் விரிவான வரிசை  
- **தாக்கம் மதிப்பீடு**: உடைமையின் அளவு மற்றும் தரவின் வெளிப்பாட்டை மதிப்பீடு  

## **முக்கிய பாதுகாப்பு கட்டமைப்பு கொள்கைகள்**

### **ஆழமான பாதுகாப்பு**
- **பல பாதுகாப்பு அடுக்குகள்**: பாதுகாப்பு கட்டமைப்பில் எந்த ஒரு புள்ளியும் தோல்வியடையாதது  
- **மீண்டும் மீண்டும் கட்டுப்பாடுகள்**: முக்கிய செயல்பாடுகளுக்கான ஒட்டுமொத்த பாதுகாப்பு நடவடிக்கைகள்  
- **தவறாத இயல்புகள்**: அமைப்புகள் பிழைகள் அல்லது தாக்குதல்களை சந்திக்கும் போது பாதுகாப்பான இயல்புகள்  

### **பூஜ்ஜிய நம்பகத்தன்மை செயல்படுத்தல்**
- **எப்போதும் நம்பாதீர்கள், எப்போதும் சரிபார்க்கவும்**: அனைத்து அமைப்புகள் மற்றும் கோரிக்கைகளின் தொடர்ச்சியான சரிபார்ப்பு  
- **குறைந்த சலுகை கொள்கை**: அனைத்து கூறுகளுக்கும் குறைந்த அணுகல் உரிமைகள்  
- **மைக்ரோ-பிரிவாக்கம்**: நுணுக்கமான நெட்வொர்க் மற்றும் அணுகல் கட்டுப்பாடுகள்  

### **தொடர்ச்சியான பாதுகாப்பு மேம்பாடு**
- **அச்சுறுத்தல் நிலப்பரப்பு தழுவல்**: உருவாகும் அச்சுறுத்தல்களைத் தீர்க்க முறைமையான புதுப்பிப்புகள்  
- **பாதுகாப்பு கட்டுப்பாடுகளின் செயல்திறன்**: கட்டுப்பாடுகளின் தொடர்ச்சியான மதிப்பீடு மற்றும் மேம்பாடு  
- **தரநிலை இணக்கம்**: உருவாகும் MCP பாதுகாப்பு தரநிலைகளுடன் ஒத்திசைவு  

---

## **செயல்படுத்தல் வளங்கள்**

### **அதிகாரப்பூர்வ MCP ஆவணங்கள்**
- [MCP Specification (2025-06-18)](https://spec.modelcontextprotocol.io/specification/2025-06-18/)  
- [MCP Security Best Practices](https://modelcontextprotocol.io/specification/2025-06-18/basic/security_best_practices)  
- [MCP Authorization Specification](https://modelcontextprotocol.io/specification/2025-06-18/basic/authorization)  

### **Microsoft பாதுகாப்பு

---

**குறிப்பு**:  
இந்த ஆவணம் [Co-op Translator](https://github.com/Azure/co-op-translator) என்ற AI மொழிபெயர்ப்பு சேவையை பயன்படுத்தி மொழிபெயர்க்கப்பட்டுள்ளது. எங்கள் தரச்சிறப்பிற்காக முயற்சி செய்கிறோம், ஆனால் தானியக்க மொழிபெயர்ப்புகளில் பிழைகள் அல்லது தவறுகள் இருக்கக்கூடும் என்பதை தயவுசெய்து கவனத்தில் கொள்ளவும். அதன் தாய்மொழியில் உள்ள மூல ஆவணம் அதிகாரப்பூர்வ ஆதாரமாக கருதப்பட வேண்டும். முக்கியமான தகவல்களுக்கு, தொழில்முறை மனித மொழிபெயர்ப்பு பரிந்துரைக்கப்படுகிறது. இந்த மொழிபெயர்ப்பைப் பயன்படுத்துவதால் ஏற்படும் எந்த தவறான புரிதல்கள் அல்லது தவறான விளக்கங்களுக்கு நாங்கள் பொறுப்பல்ல.