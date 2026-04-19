# "Hands up!" - vulnerability of the school authenification & payment system with static keys
**By:** @Vulndisclosure 

[Читать на русском языке 🇷🇺](README_RU.md)

**Problem:**
1. Static keys in FFFFFFFFFFFF format
2. User identification and authentication in payment and access control systems relies solely on UID
3. Lack of key diversification
4. Use of an insecure standard with no encryption in critical infrastructure

**Corresponding CWE:**
- CWE-798: Use of Hard-coded Credentials
- CWE-1391: Use of Weak Credentials
- CWE-287: Improper Authentication
- CWE-290: Authentication Bypass by Spoofing

**What is vulnerable:**
All palm cards and bracelets

**Explanation:**
From the card dump, it is clear that each instance differs only by its UID and uses the static key "FFFFFFFFFFFF". This allows an attacker when reading the card to:
1. Pass through turnstiles, impersonate a student
2. Use a stolen UID to emulate transactions

Essentially, the physical access system lacks any authentication — only identification.

The problem is not that "the keys are bad", but that the financial transaction trusts an unprotected identifier:

1. **Identification:** Card UID (public information).
2. **Authentication:** Missing (default keys, sector 0 accessible to everyone).
3. **Authorization:** Happens on the server based solely on UID.

**PoC:**
![Access Conditions 1](https://github.com/Vulndisclosure/Hands_UP-ladoshki-system-card-vuln/blob/main/Pics/Screenshot_20260419_114546_MIFARE%20Classic%20Tool.jpg)

![Access Conditions 2](https://github.com/Vulndisclosure/Hands_UP-ladoshki-system-card-vuln/blob/main/Pics/Screenshot_20260419_114012_MIFARE%20Classic%20Tool.jpg)

In the screenshot of Access Conditions, it is clear that all sectors are managed with the default key. The Read/Write columns confirm the ability to manipulate block data without using proprietary encryption algorithms, making the system defenseless against any NFC-compatible device.

**We see:**
- UID - main identifier
- Key A - FFFFFFFFFFFF 
- Key B - FFFFFFFFFFFF 
- And access conditions

This is a fundamental error not only in business logic but also in the access control system, as millions of people trust this system and use it every day.
