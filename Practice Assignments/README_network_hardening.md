# 🔒 Security Risk Assessment: Network Hardening Recommendations

**Course:** Google Cybersecurity Certificate  
**Type:** Security Risk Assessment — Network Hardening  
**Context:** Post-Data Breach Analysis  

---

## 📋 Scenario

A social media organization recently experienced a major data breach that compromised customers' personal information, including names and addresses. As a security analyst, I was tasked with inspecting the organization's network, identifying existing vulnerabilities, and recommending network hardening practices to prevent future attacks and breaches.

---

## 🔍 Identified Vulnerabilities

During the network inspection, four critical vulnerabilities were discovered:

| # | Vulnerability | Risk Level |
|---|---------------|------------|
| 1 | Employees share passwords across accounts |  High |
| 2 | Admin password for the database is set to default |  High |
| 3 | Firewalls have no rules to filter incoming or outgoing traffic |  High |
| 4 | Multi-Factor Authentication (MFA) is not in use |  High |

---

## 📄 Part 1: Selected Hardening Tools and Methods

After reviewing the four vulnerabilities, the following three hardening methods are recommended for immediate implementation:

**1. Multi-Factor Authentication (MFA)**  
MFA adds a second layer of verification to the login process, making it significantly harder for unauthorized users to access accounts, servers, and network resources — even if a password has been compromised or shared.

**2. Firewall Rule Configuration and Maintenance**  
Implementing and regularly maintaining firewall rules allows the organization to control and filter both incoming and outgoing network traffic, blocking suspicious or unauthorized connections before they can cause damage.

**3. Password Policies**  
Enforcing a strong password policy addresses multiple vulnerabilities simultaneously — eliminating the use of default passwords, preventing password sharing among employees, and ensuring all credentials meet minimum security requirements.

---

## 📄 Part 2: Explanation of Recommendations

### 🔐 1. Multi-Factor Authentication (MFA)

MFA requires users to verify their identity using more than one factor before gaining access to an account or system. The three common factors are:

- **Something you know** — a password or PIN
- **Something you have** — a physical ID card, security token, or one-time code sent to a phone
- **Something you are** — a biometric identifier such as a fingerprint or facial recognition

This directly addresses **vulnerability #4** (MFA not in use) and also mitigates the risk created by **vulnerability #1** (employees sharing passwords). Even if an employee shares their password or it is guessed by an attacker, MFA ensures that the attacker cannot complete the login without access to the second authentication factor. This significantly reduces the risk of unauthorized access to the organization's systems and customer data.

---

### 🛡️ 2. Firewall Rule Configuration and Maintenance

A firewall acts as a barrier between the internal network and external traffic. However, a firewall without properly defined rules provides little to no protection. This directly addresses **vulnerability #3** — the organization currently has no rules in place to filter traffic coming in and out of the network.

Recommended actions:
- Define rules that explicitly allow only trusted traffic and block everything else by default
- Implement separate rules for inbound and outbound traffic
- Regularly review and update firewall rules to stay ahead of new and evolving threats
- Monitor firewall logs for unusual or suspicious traffic patterns

Proper firewall configuration creates a critical first line of defense, preventing malicious actors from accessing the network and reducing the attack surface significantly.

---

### 🔑 3. Password Policies

A strong password policy establishes organization-wide standards for how passwords are created, managed, and protected. This directly addresses both **vulnerability #1** (employees sharing passwords) and **vulnerability #2** (default admin password on the database).

Recommended policy requirements:
- Immediately change all default passwords upon account creation — default passwords are publicly known and are among the first credentials an attacker will attempt
- Set a minimum password length and complexity requirement (uppercase, lowercase, numbers, and special characters)
- Prohibit password sharing between employees under any circumstance
- Require passwords to be changed on a regular schedule
- Implement account lockout after a defined number of failed login attempts to prevent brute force attacks

Password policies make it significantly harder for attackers to guess or brute force their way into accounts, and eliminate the risk of a single compromised password granting access to multiple accounts due to sharing.

---

*This project is part of my cybersecurity portfolio, completed as a hands-on exercise in security risk assessment and network hardening techniques.*
