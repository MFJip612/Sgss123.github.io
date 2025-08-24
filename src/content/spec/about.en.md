---
lang: en
---

# About & Privacy Policy

Last Updated: 2025-08-09  

## Overview
This site is a static website built with the [Fuwari](https://github.com/saicaca/fuwari) framework.  
::github{repo="saicaca/fuwari"}

## 1. Site Nature
- Pre-rendered static content, no dynamic account system.  
- The server does not actively write or store users’ personal profiles.  
- Technical data (if processed) is mainly for: content delivery, performance optimization, security protection, abuse / attack detection, compliance auditing.  

## 2. Third-Party Services

### 2.1 Cloudflare
Purpose: CDN / reverse proxy, security protection (e.g. DDoS mitigation, bot management), caching, edge acceleration.  
Cloudflare may temporarily (or per its own policies) process / cache the following at its global edge nodes:  
- Access logs: IP address, port, timestamp, request URL, HTTP method, status code, bytes transferred, referrer, User-Agent.  
- Network & security events: threat score, challenge / verification results (e.g. Turnstile token status), firewall rule IDs triggered, TLS protocol & cipher info, handshake failure reasons.  
- Performance & diagnostics: request latency, cache hit / miss status, regional routing info.  
- Cookies (if required for security or cache differentiation) and temporary identifiers set by Cloudflare.  

Cloudflare Turnstile (if integrated) may additionally process: browser fingerprint signals, runtime environment characteristics, challenge interaction results. The site only receives the verification token result, not raw identifiable fingerprint data.

### 2.2 EdgeOne
Purpose: content delivery acceleration, smart routing, caching, baseline security enhancement.  
Data categories are similar to Cloudflare (request metadata, IP, User-Agent, timestamp, regional info, cache status, basic security event records).  
Uses: routing optimization, performance statistics, abuse detection, service quality maintenance.  

### 2.3 Logging & Retention
- The site itself (if server access logs are enabled) keeps only minimal general access records for troubleshooting and security auditing: time, IP, request path, status code, size, User-Agent.  
- Retention periods of third parties follow their own privacy / compliance policies (refer to Cloudflare & EdgeOne official documentation).  
- The site is not responsible for independent processing by third parties, but strives to minimize exposure of personal-related dimensions.  

## 3. Cookies & Local Storage
- The site does not currently set custom tracking cookies for cross-site behavioral profiling.  
- Still possible:  
  - Temporary tokens required for third-party security / verification (e.g. Turnstile).  
  - Technical cookies automatically set by caching or protection layers (session distinction, protection flags, preference persistence; not for marketing profiling).  

## 4. Purposes of Data Use
- Security: mitigate malicious traffic, abuse, spam submissions, automated attacks.  
- Performance: load balancing, cache optimization, network path optimization.  
- Reliability: incident diagnostics, availability monitoring.  
- Compliance & auditing: legal, operational, or abuse investigation needs (if applicable).  

## 5. What We Do Not Do
We do NOT:  
- Build user behavioral profiles.  
- Sell, rent, or trade access logs.  
- Collect sensitive personally identifiable information (PII) via hidden scripts.  

## 6. Visitor Rights
Subject to applicable legal frameworks (e.g. GDPR or local data protection laws), you may have:  
- Right of Access: know whether data relating to you (if any) is processed (the site itself stores no persistent personal profile).  
- Right to Rectification: correct inaccuracies (limited if we do not hold such data).  
- Right to Erasure: request deletion where legally / technically feasible (third-party held data must be requested from them).  
- Right to Restrict Processing: in specific circumstances.  
- Right to Object: object to processing based on legitimate interests.  
- Data Portability: structured, commonly used format (not applicable if no such persistent dataset exists).  
- Withdrawal of Consent: for optional future consent-based features (if any).  
- Right to Lodge a Complaint: with a supervisory authority.  

If you wish to exercise rights, use any public contact channel (if provided). For data held independently by Cloudflare or EdgeOne, refer to their official privacy channels. If we store no identifiable data, we may be unable to fulfill certain access or correction requests.

## 7. Information Security
We apply data minimization; rely on edge network security features (WAF, DDoS mitigation, verification challenges); restrict log access. Absolute security over the Internet cannot be guaranteed; improvements are ongoing.

## 8. Policy Updates
This policy may change due to service adjustments or legal requirements. Significant changes are reflected by the update date. Continued use constitutes awareness and acceptance of revisions.

## 9. Contact
If you have questions regarding this policy or data handling, use the contact channels provided (e.g. repository issues, message channels). If no direct method is listed, you may open an issue in the public repository.
