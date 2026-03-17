# 🛡️ Cybersecurity Incident Report: Brute Force Attack & OS Hardening

**Course:** Google Cybersecurity Certificate  
**Type:** Security Incident Report — Brute Force Attack  
**Tool Used:** tcpdump  
**Protocols Analyzed:** DNS, HTTP  

---

## 📋 Scenario

A former employee launched a brute force attack against the web hosting account of `yummyrecipesforme.com`, a website that sells recipes and cookbooks. After gaining unauthorized access to the admin panel, the attacker injected malicious JavaScript into the website's source code, redirecting visitors to a fake website containing malware. As a cybersecurity analyst, I was tasked with investigating the incident, documenting the attack, and recommending security measures to prevent future occurrences.

---

## 🧰 Tools & Methods

- **tcpdump** — Network protocol analyzer used to capture and inspect DNS and HTTP traffic
- **Sandbox environment** — Used to safely observe the malicious website behavior without risking production systems
- **Source code analysis** — Senior analyst reviewed the website's source code to identify the injected JavaScript

---

## 📄 Section 1: Network Protocols Involved in the Incident

The incident involved two network protocols: **DNS** and **HTTP**.

**DNS (Domain Name System)** was used at two key points during the attack:
- First, to resolve the IP address of `yummyrecipesforme.com` when the user visited the legitimate website
- Second, to resolve the IP address of `greatrecipesforme.com` after the malware redirected the user's browser to the fake site

**HTTP (Hypertext Transfer Protocol)** was used to:
- Request and load the `yummyrecipesforme.com` webpage, which triggered the malicious file download
- Request and load the `greatrecipesforme.com` webpage after the redirect, delivering the malware to the user's machine

The following sequence was captured in the tcpdump log:

| Step | Protocol | Description |
|------|----------|-------------|
| 1 | DNS | Browser requests IP address of `yummyrecipesforme.com` |
| 2 | DNS | DNS server replies with the correct IP address |
| 3 | HTTP | Browser requests the `yummyrecipesforme.com` webpage |
| 4 | HTTP | Browser initiates download of the malicious executable file |
| 5 | DNS | Browser requests IP address of `greatrecipesforme.com` |
| 6 | DNS | DNS server replies with the IP address of the fake site |
| 7 | HTTP | Browser loads `greatrecipesforme.com`, delivering the malware |

---

## 📄 Section 2: Incident Documentation

**Attack type:** Brute Force Attack followed by Malware Injection

**How the attack unfolded:**

The attacker — a disgruntled former employee — initiated a brute force attack against the administrative account of `yummyrecipesforme.com`. By repeatedly entering known default passwords, the attacker was eventually able to guess the correct credentials, as the admin password had never been changed from its default value and no controls were in place to limit login attempts.

Once inside the admin panel, the attacker made two critical changes:
1. Injected a JavaScript function into the website's source code that prompted all visitors to download and run an executable file
2. Changed the admin account password, locking out the legitimate website owner

**Impact on users:**

Several hours after the attack, multiple customers contacted the helpdesk reporting that the website had prompted them to download a file in order to access free recipes. After running the file, users reported that their browser was redirected to a different website address and that their personal computers began running significantly more slowly — consistent with malware infection.

**How the incident was detected:**

The website owner attempted to log in to the admin panel and was unable to, prompting them to contact the hosting provider. A cybersecurity team was assembled to investigate. Analysts created a sandbox environment, ran tcpdump to capture network traffic, and visited the website to reproduce the behavior. The browser immediately prompted a file download, and after running it, the browser was redirected to `greatrecipesforme.com`.

A senior analyst confirmed the attack by reviewing the website's source code and identifying the injected JavaScript responsible for the malicious download prompt and redirect.

---

## 📄 Section 3: Remediation Recommendations for Brute Force Attacks

### 1. 🔑 Enforce Strong Password Policies

The most immediate vulnerability exploited in this attack was the use of a **default admin password**. Default passwords are publicly known and are among the first credentials attempted in any brute force attack.

**Recommended actions:**
- Immediately change all default passwords upon account creation
- Enforce a strong password policy requiring a minimum length, a mix of uppercase/lowercase letters, numbers, and special characters
- Prohibit the reuse of previously used passwords

### 2. 🔐 Implement Multi-Factor Authentication (MFA)

Even if an attacker successfully obtains a valid password through brute force, **MFA adds a second layer of verification** that they cannot bypass without access to the user's second factor (e.g., a one-time code sent to a phone or authentication app).

Implementing MFA on the admin panel would have prevented this attack entirely — the attacker had the correct password but would have been blocked at the second authentication step.


---


*This project is part of my cybersecurity portfolio, completed as a hands-on exercise in security incident documentation and OS hardening techniques.*
