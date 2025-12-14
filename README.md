# MSMC

⚠️ **SECURITY WARNING** ⚠️
**READ [SECURITY_WARNING.txt](SECURITY_WARNING.txt) AND [SECURITY_ANALYSIS.md](SECURITY_ANALYSIS.md) BEFORE USING**

**CRITICAL:** This tool stores credentials in plaintext, sends data to external services, and may violate Terms of Service. Only use on accounts you own. See security documentation for full details.

📖 **[HOW TO ONLY CHECK YOUR OWN ACCOUNTS →](HOW_TO_USE_SAFELY.md)** - Step-by-step guide for safe usage

---

## About:
msmc is a minecraft account checker that checks through microsoft xbox login instead of the older mojang login.
it supports http(s), socks4, socks5 proxies but they must be pretty decent because microsofts authentication is very protective. it also uses tor proxies. it auto installs tor for you if selected.

## Proxy Format:
`user:pass@ip:port` and `ip:port`

### [github discord](https://discord.com/invite/g9tb4S3BJk) | boosting gives access to beta msmc

## Captures:
- xbox game pass/xbox game pass ultimate accounts
- minecraft capes
- optifine cape
- email access
- last name change
- hypixel rank, level, first/last login, bedwars stars, skyblock coins, ban status

## Installing:
MSMC ONLY SUPPORTS WINDOWS IT WILL NOT WORK ON LINUX OR MACOS
### [LINUX VERSION](https://github.com/8h3-coder/MSMC_Linux) (thanks to 8h3)

Watch the tutorial [here](https://youtu.be/8j8JQBe06Nw)

You do not need to install tor. Tor is automatically installed when selected for proxies.

install [python](https://www.python.org/downloads/) and [git](https://git-scm.com/download/win)
```
git clone https://github.com/MachineKillin/MSMC
cd MSMC
pip install -r requirements.txt
python MSMC.py
```

## Addons
[Inboxer](https://github.com/PgerTools/MSMC-Inbox) (No longer working)

## Pictures:
![LOG](https://i.imgur.com/oBd2Pbj.png)

## Usage:
You are not allowed to sell msmc or any modified versions. If you use any of my code please give me credit.

---

## ⚠️ Security & Legal Notice

### Security Risks
This tool has been analyzed and contains several security concerns:
- **Credentials stored in plaintext** - All results are saved without encryption
- **External data transmission** - Credentials sent to Discord webhooks and third-party APIs
- **Vulnerable dependencies** - urllib3 2.2.2 has known CVEs (update to >=2.6.0 required)
- **Disabled SSL verification** - Vulnerable to man-in-the-middle attacks
- **Untrusted proxy usage** - Auto-scraped proxies may log your traffic

### Legal Warnings
⚖️ Using this tool may violate:
- Microsoft Terms of Service
- Mojang/Microsoft EULA
- Computer Fraud and Abuse Act (CFAA)
- Local anti-hacking laws

**ONLY use this tool on accounts you personally own.**

### Required Reading
Before using this software, you MUST read:
1. **[HOW_TO_USE_SAFELY.md](HOW_TO_USE_SAFELY.md)** - Step-by-step guide to check only your own accounts
2. [SECURITY_WARNING.txt](SECURITY_WARNING.txt) - Critical security and legal warnings
3. [SECURITY_ANALYSIS.md](SECURITY_ANALYSIS.md) - Comprehensive security analysis

### Recommendations
- ✅ Only check your own accounts
- ✅ Run on isolated/virtual machine
- ✅ Update urllib3: `pip install --upgrade urllib3>=2.6.0`
- ✅ Review webhook URL in config.ini before running
- ✅ Delete result files immediately after use
- ✅ Never commit results to git
- ❌ Never test credentials you don't own
- ❌ Never share your config.ini file

**By using this software, you accept full legal responsibility and acknowledge all security risks.**

