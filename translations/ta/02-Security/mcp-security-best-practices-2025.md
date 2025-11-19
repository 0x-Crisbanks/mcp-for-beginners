<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "057dd5cc6bea6434fdb788e6c93f3f3d",
  "translation_date": "2025-10-11T11:59:59+00:00",
  "source_file": "02-Security/mcp-security-best-practices-2025.md",
  "language_code": "ta"
}
-->
# MCP பாதுகாப்பு சிறந்த நடைமுறைகள் - ஆகஸ்ட் 2025 புதுப்பிப்பு

> **முக்கியம்**: இந்த ஆவணம் சமீபத்திய [MCP Specification 2025-06-18](https://spec.modelcontextprotocol.io/specification/2025-06-18/) பாதுகாப்பு தேவைகள் மற்றும் அதிகாரப்பூர்வ [MCP Security Best Practices](https://modelcontextprotocol.io/specification/2025-06-18/basic/security_best_practices) ஆகியவற்றை பிரதிபலிக்கிறது. எப்போதும் தற்போதைய விவரக்குறிப்பை அணுகி புதுப்பிக்கப்பட்ட வழிகாட்டுதல்களைப் பெறுங்கள்.

## MCP செயல்பாடுகளுக்கான அடிப்படை பாதுகாப்பு நடைமுறைகள்

Model Context Protocol பாரம்பரிய மென்பொருள் பாதுகாப்பை விட அதிகமான சவால்களை உருவாக்குகிறது. இந்த நடைமுறைகள் அடிப்படை பாதுகாப்பு தேவைகள் மற்றும் MCP-க்கு தனித்துவமான அச்சுறுத்தல்களை, உதாரணமாக prompt injection, tool poisoning, session hijacking, confused deputy பிரச்சினைகள் மற்றும் token passthrough பாதிப்புகளை கையாளுகின்றன.

### **கட்டாயமான பாதுகாப்பு தேவைகள்**

**MCP Specification-இன் முக்கிய தேவைகள்:**

> **MUST NOT**: MCP சேவைகள் **MUST NOT** MCP சேவைக்கு வெளியில் வழங்கப்பட்ட tokens-ஐ ஏற்கக்கூடாது  
> 
> **MUST**: Authorization செயல்படுத்தும் MCP சேவைகள் **MUST** அனைத்து உள்ளீட்டு கோரிக்கைகளையும் சரிபார்க்க வேண்டும்  
>  
> **MUST NOT**: MCP சேவைகள் **MUST NOT** authentication-க்கு sessions-ஐ பயன்படுத்தக்கூடாது  
>
> **MUST**: Static client IDs-ஐ பயன்படுத்தும் MCP proxy சேவைகள் **MUST** ஒவ்வொரு dynamic client-க்கும் பயனர் ஒப்புதலைப் பெற வேண்டும்  

---

## 1. **Token பாதுகாப்பு & Authentication**

**Authentication & Authorization கட்டுப்பாடுகள்:**
   - **கடுமையான Authorization மதிப்பீடு**: MCP சேவையின் authorization logic-ஐ முழுமையாக audit செய்து, resources-ஐ அணுகுவதற்கு உரிய பயனர்கள் மற்றும் clients மட்டுமே அனுமதிக்கப்படுவதை உறுதிசெய்யவும்  
   - **வெளியக அடையாள வழங்குநர் ஒருங்கிணைப்பு**: Microsoft Entra ID போன்ற நிறுவப்பட்ட identity providers-ஐ பயன்படுத்தவும்; custom authentication-ஐ உருவாக்க வேண்டாம்  
   - **Token Audience Validation**: MCP சேவைக்கு வழங்கப்பட்ட tokens-ஐ மட்டுமே validate செய்யவும்; upstream tokens-ஐ ஏற்க வேண்டாம்  
   - **சரியான Token Lifecycle**: பாதுகாப்பான token rotation, expiration policies-ஐ செயல்படுத்தவும்; token replay attacks-ஐ தடுக்கவும்  

**Token பாதுகாப்பான சேமிப்பு:**
   - Azure Key Vault அல்லது இதற்கு இணையான பாதுகாப்பான credential stores-ஐ பயன்படுத்தவும்  
   - Tokens-ஐ rest மற்றும் transit-இல் encryption செய்யவும்  
   - Unauthorized access-ஐ கண்டறிய credential rotation மற்றும் monitoring-ஐ சீராக செயல்படுத்தவும்  

## 2. **Session மேலாண்மை & Transport பாதுகாப்பு**

**Session பாதுகாப்பு நடைமுறைகள்:**
   - **Cryptographically Secure Session IDs**: பாதுகாப்பான random number generators மூலம் உருவாக்கப்பட்ட non-deterministic session IDs-ஐ பயன்படுத்தவும்  
   - **User-Specific Binding**: `<user_id>:<session_id>` போன்ற formats-ஐ session IDs-ஐ user identities-க்கு bind செய்ய cross-user session abuse-ஐ தடுக்கவும்  
   - **Session Lifecycle Management**: Expiration, rotation, invalidation ஆகியவற்றை சரியாக செயல்படுத்த vulnerability windows-ஐ குறைக்கவும்  
   - **HTTPS/TLS Enforcement**: Session ID interception-ஐ தடுக்க அனைத்து தொடர்புகளுக்கும் HTTPS கட்டாயமாக செயல்படுத்தவும்  

**Transport Layer Security:**
   - TLS 1.3-ஐ சரியான certificate management-இன் மூலம் configure செய்யவும்  
   - முக்கியமான தொடர்புகளுக்கு certificate pinning-ஐ செயல்படுத்தவும்  
   - Certificates-ஐ சீராக rotate செய்து validity-ஐ சரிபார்க்கவும்  

## 3. **AI-க்கு தனித்துவமான அச்சுறுத்தல்களைத் தடுக்க 🤖**

**Prompt Injection பாதுகாப்பு:**
   - **Microsoft Prompt Shields**: Malicious instructions-ஐ advanced detection மற்றும் filtering செய்ய AI Prompt Shields-ஐ deploy செய்யவும்  
   - **Input Sanitization**: Injection attacks மற்றும் confused deputy பிரச்சினைகளைத் தடுக்க inputs-ஐ validate செய்து sanitize செய்யவும்  
   - **Content Boundaries**: Trusted instructions மற்றும் வெளிப்புற உள்ளடக்கத்தை வேறுபடுத்த delimiter மற்றும் datamarking systems-ஐ பயன்படுத்தவும்  

**Tool Poisoning தடுப்பு:**
   - **Tool Metadata Validation**: Tool definitions integrity checks-ஐ செயல்படுத்த unexpected changes-ஐ கண்காணிக்கவும்  
   - **Dynamic Tool Monitoring**: Runtime behavior-ஐ கண்காணித்து unexpected execution patterns-க்கு alerting அமைக்கவும்  
   - **Approval Workflows**: Tool modifications மற்றும் capability changes-க்கு explicit user approval-ஐ தேவைப்படுத்தவும்  

## 4. **Access Control & Permissions**

**Principle of Least Privilege:**
   - MCP சேவைகளுக்கு தேவையான குறைந்த permissions மட்டுமே வழங்கவும்  
   - Fine-grained permissions-உடன் role-based access control (RBAC)-ஐ செயல்படுத்தவும்  
   - Privilege escalation-ஐ தடுக்க permissions-ஐ சீராக மதிப்பீடு செய்து கண்காணிக்கவும்  

**Runtime Permission Controls:**
   - Resource exhaustion attacks-ஐ தடுக்க resource limits-ஐ செயல்படுத்தவும்  
   - Tool execution environments-க்கு container isolation-ஐ பயன்படுத்தவும்  
   - Administrative functions-க்கு just-in-time access-ஐ செயல்படுத்தவும்  

## 5. **Content Safety & Monitoring**

**Content Safety செயல்படுத்தல்:**
   - **Azure Content Safety Integration**: Hateful content, jailbreak முயற்சிகள் மற்றும் policy மீறல்களை கண்டறிய Azure Content Safety-ஐ பயன்படுத்தவும்  
   - **Behavioral Analysis**: MCP சேவை மற்றும் tool execution-இல் உள்ள runtime anomalies-ஐ கண்டறிய runtime behavioral monitoring-ஐ செயல்படுத்தவும்  
   - **Comprehensive Logging**: Authentication முயற்சிகள், tool invocations மற்றும் security நிகழ்வுகளை tamper-proof storage-இல் பதிவு செய்யவும்  

**தொடர்ச்சியான கண்காணிப்பு:**
   - Unauthorized access முயற்சிகள் மற்றும் சந்தேகமான patterns-க்கு real-time alerting  
   - SIEM systems-உடன் ஒருங்கிணைத்து centralized security event management  
   - MCP செயல்பாடுகளின் security audits மற்றும் penetration testing-ஐ சீராக செயல்படுத்தவும்  

## 6. **Supply Chain Security**

**Component Verification:**
   - **Dependency Scanning**: Vulnerability scanning-ஐ software dependencies மற்றும் AI components-க்கு automated முறையில் செயல்படுத்தவும்  
   - **Provenance Validation**: Models, data sources மற்றும் வெளிப்புற சேவைகளின் origin, licensing மற்றும் integrity-ஐ சரிபார்க்கவும்  
   - **Signed Packages**: Cryptographically signed packages-ஐ deploy செய்யும் முன் signatures-ஐ validate செய்யவும்  

**Secure Development Pipeline:**
   - **GitHub Advanced Security**: Secret scanning, dependency analysis மற்றும் CodeQL static analysis-ஐ செயல்படுத்தவும்  
   - **CI/CD Security**: Automated deployment pipelines-இல் security validation-ஐ ஒருங்கிணைக்கவும்  
   - **Artifact Integrity**: Cryptographic verification-ஐ deployed artifacts மற்றும் configurations-க்கு செயல்படுத்தவும்  

## 7. **OAuth Security & Confused Deputy Prevention**

**OAuth 2.1 செயல்படுத்தல்:**
   - **PKCE Implementation**: Authorization requests-க்கு Proof Key for Code Exchange (PKCE)-ஐ பயன்படுத்தவும்  
   - **Explicit Consent**: Confused deputy attacks-ஐ தடுக்க ஒவ்வொரு dynamic client-க்கும் பயனர் ஒப்புதலைப் பெறவும்  
   - **Redirect URI Validation**: Redirect URIs மற்றும் client identifiers-ஐ கடுமையாக validate செய்யவும்  

**Proxy Security:**
   - Static client ID exploitation மூலம் authorization bypass-ஐ தடுக்கவும்  
   - Third-party API access-க்கு சரியான consent workflows-ஐ செயல்படுத்தவும்  
   - Authorization code theft மற்றும் unauthorized API access-ஐ கண்காணிக்கவும்  

## 8. **Incident Response & Recovery**

**Rapid Response திறன்கள்:**
   - **Automated Response**: Credential rotation மற்றும் threat containment-க்கு automated systems-ஐ செயல்படுத்தவும்  
   - **Rollback Procedures**: Known-good configurations மற்றும் components-க்கு விரைவாக revert செய்யும் திறனை உருவாக்கவும்  
   - **Forensic Capabilities**: Incident investigation-க்கு விரிவான audit trails மற்றும் logging-ஐ செயல்படுத்தவும்  

**Communications & Coordination:**
   - Security incidents-க்கு தெளிவான escalation செயல்முறைகள்  
   - அமைப்பின் incident response குழுக்களுடன் ஒருங்கிணைப்பு  
   - Security incident simulations மற்றும் tabletop exercises-ஐ சீராக செயல்படுத்தவும்  

## 9. **Compliance & Governance**

**Regulatory Compliance:**
   - MCP செயல்பாடுகள் GDPR, HIPAA, SOC 2 போன்ற தொழில்துறை-specific தேவைகளை பூர்த்தி செய்ய வேண்டும்  
   - AI data processing-க்கு data classification மற்றும் privacy controls-ஐ செயல்படுத்தவும்  
   - Compliance auditing-க்கு விரிவான ஆவணங்களை பராமரிக்கவும்  

**Change Management:**
   - MCP system மாற்றங்களுக்கு formal security review செயல்முறைகள்  
   - Configuration changes-க்கு version control மற்றும் approval workflows  
   - Compliance assessments மற்றும் gap analysis-ஐ சீராக செயல்படுத்தவும்  

## 10. **Advanced Security Controls**

**Zero Trust Architecture:**
   - **Never Trust, Always Verify**: Users, devices மற்றும் connections-ஐ தொடர்ச்சியாக validate செய்யவும்  
   - **Micro-segmentation**: MCP components-ஐ தனித்துவமாக isolate செய்ய network controls  
   - **Conditional Access**: Risk-based access controls-ஐ current context மற்றும் behavior-க்கு ஏற்ப செயல்படுத்தவும்  

**Runtime Application Protection:**
   - **Runtime Application Self-Protection (RASP)**: Real-time threat detection-க்கு RASP techniques-ஐ deploy செய்யவும்  
   - **Application Performance Monitoring**: Attacks-ஐ குறிக்க performance anomalies-ஐ monitor செய்யவும்  
   - **Dynamic Security Policies**: Current threat landscape-க்கு ஏற்ப security policies-ஐ adapt செய்யவும்  

## 11. **Microsoft Security Ecosystem Integration**

**Comprehensive Microsoft Security:**
   - **Microsoft Defender for Cloud**: MCP workloads-க்கு cloud security posture management  
   - **Azure Sentinel**: Advanced threat detection-க்கு cloud-native SIEM மற்றும் SOAR திறன்கள்  
   - **Microsoft Purview**: AI workflows மற்றும் data sources-க்கு data governance மற்றும் compliance  

**Identity & Access Management:**
   - **Microsoft Entra ID**: Conditional access policies-உடன் enterprise identity management  
   - **Privileged Identity Management (PIM)**: Administrative functions-க்கு just-in-time access மற்றும் approval workflows  
   - **Identity Protection**: Risk-based conditional access மற்றும் automated threat response  

## 12. **Continuous Security Evolution**

**தற்போதைய நிலையை பராமரித்தல்:**
   - **Specification Monitoring**: MCP specification updates மற்றும் security guidance changes-ஐ சீராக மதிப்பீடு செய்யவும்  
   - **Threat Intelligence**: AI-specific threat feeds மற்றும் compromise-indicators-ஐ ஒருங்கிணைக்கவும்  
   - **Security Community Engagement**: MCP security community மற்றும் vulnerability disclosure programs-இல் செயல்படவும்  

**Adaptive Security:**
   - **Machine Learning Security**: Novel attack patterns-ஐ கண்டறிய ML-based anomaly detection-ஐ பயன்படுத்தவும்  
   - **Predictive Security Analytics**: Proactive threat identification-க்கு predictive models-ஐ செயல்படுத்தவும்  
   - **Security Automation**: Threat intelligence மற்றும் specification changes-ஐ அடிப்படையாகக் கொண்டு automated security policy updates-ஐ செயல்படுத்தவும்  

---

## **முக்கிய பாதுகாப்பு வளங்கள்**

### **அதிகாரப்பூர்வ MCP ஆவணங்கள்**
- [MCP Specification (2025-06-18)](https://spec.modelcontextprotocol.io/specification/2025-06-18/)
- [MCP Security Best Practices](https://modelcontextprotocol.io/specification/2025-06-18/basic/security_best_practices)
- [MCP Authorization Specification](https://modelcontextprotocol.io/specification/2025-06-18/basic/authorization)

### **Microsoft Security Solutions**
- [Microsoft Prompt Shields](https://learn.microsoft.com/azure/ai-services/content-safety/concepts/jailbreak-detection)
- [Azure Content Safety](https://learn.microsoft.com/azure/ai-services/content-safety/)
- [Microsoft Entra ID Security](https://learn.microsoft.com/entra/identity-platform/secure-least-privileged-access)
- [GitHub Advanced Security](https://github.com/security/advanced-security)

### **Security Standards**
- [OAuth 2.0 Security Best Practices (RFC 9700)](https://datatracker.ietf.org/doc/html/rfc9700)
- [OWASP Top 10 for Large Language Models](https://genai.owasp.org/)
- [NIST AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework)

### **Implementation Guides**
- [Azure API Management MCP Authentication Gateway](https://techcommunity.microsoft.com/blog/integrationsonazureblog/azure-api-management-your-auth-gateway-for-mcp-servers/4402690)
- [Microsoft Entra ID with MCP Servers](https://den.dev/blog/mcp-server-auth-entra-id-session/)

---

> **Security Notice**: MCP பாதுகாப்பு நடைமுறைகள் வேகமாக மாறுகின்றன. MCP செயல்படுத்துவதற்கு முன் தற்போதைய [MCP specification](https://spec.modelcontextprotocol.io/) மற்றும் [அதிகாரப்பூர்வ பாதுகாப்பு ஆவணங்கள்](https://modelcontextprotocol.io/specification/2025-06-18/basic/security_best_practices)-ஐ சரிபார்க்கவும்.

---

**குறிப்பு**:  
இந்த ஆவணம் [Co-op Translator](https://github.com/Azure/co-op-translator) என்ற AI மொழிபெயர்ப்பு சேவையைப் பயன்படுத்தி மொழிபெயர்க்கப்பட்டுள்ளது. நாங்கள் துல்லியத்திற்காக முயற்சிக்கின்றோம், ஆனால் தானியங்கி மொழிபெயர்ப்புகளில் பிழைகள் அல்லது தவறான தகவல்கள் இருக்கக்கூடும் என்பதை கவனத்தில் கொள்ளவும். அதன் தாய்மொழியில் உள்ள மூல ஆவணம் அதிகாரப்பூர்வ ஆதாரமாகக் கருதப்பட வேண்டும். முக்கியமான தகவல்களுக்கு, தொழில்முறை மனித மொழிபெயர்ப்பு பரிந்துரைக்கப்படுகிறது. இந்த மொழிபெயர்ப்பைப் பயன்படுத்துவதால் ஏற்படும் எந்தவொரு தவறான புரிதல்கள் அல்லது தவறான விளக்கங்களுக்கு நாங்கள் பொறுப்பல்ல.