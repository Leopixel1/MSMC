# How to Only Check Your Own Accounts

This guide explains how to safely use MSMC to check only accounts that you personally own.

---

## ⚠️ Important Legal Notice

**ONLY use this tool to check accounts you personally own and have authorized access to.**

Testing credentials you don't own is:
- ❌ **ILLEGAL** - Violates Computer Fraud and Abuse Act (CFAA)
- ❌ **Against Microsoft Terms of Service**
- ❌ **Against Mojang/Microsoft EULA**
- ❌ **Punishable by law** - Can result in criminal charges

---

## ✅ Safe Usage: Step-by-Step Guide

### Step 1: Prepare Your Credential File

Create a text file containing **ONLY your own account credentials** in this format:

```
your_email@example.com:your_password
your_other_email@example.com:your_other_password
```

**Important:**
- ✅ Only include accounts you personally own
- ✅ One account per line
- ✅ Format: `email:password`
- ❌ Never use combo lists from the internet
- ❌ Never use credentials that aren't yours
- ❌ Never test "leaked" or "cracked" accounts

**Example of a safe combo file:**
```
myemail@gmail.com:MySecurePassword123
mywork@outlook.com:AnotherPassword456
```

---

### Step 2: Update Dependencies (REQUIRED)

Before running, fix the security vulnerability:

```bash
pip install --upgrade urllib3>=2.6.0
```

---

### Step 3: Configure Webhooks (Optional)

If you want Discord notifications:

1. Open `config.ini` (created on first run)
2. Add YOUR OWN Discord webhook URL:
   ```ini
   [Settings]
   Webhook = https://discord.com/api/webhooks/YOUR_WEBHOOK_HERE
   ```

⚠️ **Security Warning:**
- Only use webhooks YOU control
- Never share your config.ini file
- Credentials will be sent to this webhook

---

### Step 4: Run MSMC

```bash
python MSMC.py
```

When prompted:
1. **Select threads** - Start with 5-10 for proxyless
2. **Select proxy type** - Choose [4] None if checking your own accounts
3. **Select screen** - Choose [1] CUI or [2] Log
4. **Select combo file** - Choose YOUR credential file (with only your accounts)

---

### Step 5: Review Results

Results are saved in `results/[filename]/`:
- `Hits.txt` - Successfully validated accounts
- `Capture.txt` - Detailed account information
- Other files for specific account types

⚠️ **Security Best Practices:**
- Delete result files immediately after viewing
- Never commit results to git (already blocked by .gitignore)
- Keep your system secure - files contain plaintext credentials

---

## 🛡️ Safety Checklist

Before running MSMC, ensure:

- [ ] I created the combo file myself with ONLY my accounts
- [ ] I did NOT download a combo list from the internet
- [ ] I did NOT use any "leaked" or "cracked" accounts
- [ ] Every account in my file belongs to me personally
- [ ] I have updated urllib3 to >=2.6.0
- [ ] I reviewed the webhook URL (if configured)
- [ ] I understand results are stored in plaintext
- [ ] I will delete results after viewing
- [ ] I accept full legal responsibility

**If you cannot check ALL boxes above, DO NOT run this tool.**

---

## ❌ What NOT to Do

### Never Do These Things:

1. **Don't use combo lists from the internet**
   - ❌ "100k email:pass combos.txt"
   - ❌ "Leaked accounts 2024.txt"
   - ❌ "Minecraft accounts.txt" from forums/Discord

2. **Don't test credentials you don't own**
   - ❌ Friend's accounts (even with permission)
   - ❌ Family member's accounts
   - ❌ Any account not registered to your email

3. **Don't use for credential stuffing**
   - ❌ Testing if passwords work across services
   - ❌ Checking if "leaked" accounts are valid
   - ❌ Verifying stolen credentials

4. **Don't share results**
   - ❌ Posting results online
   - ❌ Sharing working accounts
   - ❌ Committing results to git

---

## ✅ Legitimate Use Cases

**Acceptable uses:**
- ✅ Checking if your own old accounts still work
- ✅ Verifying which of your accounts have Minecraft
- ✅ Checking your accounts for game pass subscriptions
- ✅ Reviewing your own account statistics
- ✅ Testing account security (2FA status)

**All of these require the accounts to be:**
- Registered to YOUR email address
- Created by YOU
- Owned by YOU

---

## 🔒 Additional Security Tips

### 1. Use on Isolated System
- Run on a virtual machine if possible
- Don't run on shared computers
- Keep your system updated and secure

### 2. Minimize Credential Exposure
- Delete combo files after use
- Clear result files regularly
- Don't store credentials long-term

### 3. Use Secure Proxies (Optional)
- If using proxies, use trusted sources
- Free proxies may log your traffic
- Consider using your own VPN instead

### 4. Monitor Your Accounts
- Enable 2FA on your accounts
- Check for unauthorized access
- Change passwords if concerned

---

## 📞 Need Help?

### Questions About Safe Usage:
- Read [SECURITY_WARNING.txt](SECURITY_WARNING.txt) for warnings
- Read [SECURITY_ANALYSIS.md](SECURITY_ANALYSIS.md) for technical details
- Check the GitHub issues for common questions

### If You're Unsure:
**When in doubt, DON'T run the tool.**

If you're not 100% certain that all accounts in your combo file are yours, do not proceed.

---

## 📋 Quick Reference

### Valid Combo File (YOUR ACCOUNTS):
```
✅ myemail@gmail.com:MyPassword123
✅ myother@outlook.com:SecurePass456
```

### Invalid Combo File (NOT YOUR ACCOUNTS):
```
❌ random@email.com:password123  (from internet list)
❌ leaked@gmail.com:abc123       (from leaked database)
❌ friend@yahoo.com:pass         (not your account)
```

### Summary:
- **Safe:** Only your accounts, created by you, registered to your email
- **Unsafe:** Any account from the internet, leaked lists, or not owned by you
- **Legal:** Personal account verification
- **Illegal:** Testing credentials you don't own

---

## ⚖️ Legal Disclaimer

By using MSMC, you acknowledge and agree that:
- You will ONLY use it on accounts you personally own
- You understand testing others' credentials is illegal
- You accept full legal responsibility for your actions
- You understand this may violate Microsoft/Mojang Terms of Service
- The developers are not liable for your misuse of this tool

**Using this tool on accounts you don't own is a criminal offense in most jurisdictions.**

---

## 🎯 Bottom Line

**To only check your own accounts:**
1. Create a combo file with ONLY accounts you registered yourself
2. Update urllib3 to >=2.6.0
3. Run MSMC and select your file
4. Delete results after viewing

**That's it. If an account isn't yours, don't include it.**

---

Last Updated: 2024-12-14
