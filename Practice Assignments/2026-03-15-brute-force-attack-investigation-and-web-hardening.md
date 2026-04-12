# Brute Force Attack Investigation and Web Application Hardening

## Overview
This report documents the investigation of a security incident involving unauthorized access to a web hosting account followed by malicious code injection. A public-facing website was compromised, resulting in malware delivery to end users through browser redirects and forced downloads.

The incident was analyzed through network traffic inspection, controlled sandbox testing, and source code review, with the objective of identifying the attack method, documenting impact, and recommending defensive controls to prevent recurrence.

Completed in March 2026 (exact day not recorded).

## Environment
- Network Analysis Tool: tcpdump
- Protocols Analyzed: DNS, HTTP
- Target Website: yummyrecipesforme.com
- Malicious Redirect Domain: greatrecipesforme.com

## Incident Summary
A former employee successfully gained unauthorized access to the administrative interface of the target website by exploiting weak authentication controls. After obtaining access, the attacker injected malicious JavaScript into the site’s source code, forcing visitors to download and execute a malicious file that redirected them to a fraudulent website hosting malware.

The attack resulted in user system compromise, service disruption, and loss of administrative control by the website owner.

## Network Protocol Analysis

### DNS Activity
DNS was observed at two critical stages of the attack:
- Resolving the legitimate domain (`yummyrecipesforme.com`) during initial user access
- Resolving the attacker-controlled domain (`greatrecipesforme.com`) after the malicious redirect

### HTTP Activity
HTTP traffic revealed:
- Normal page requests to the legitimate website
- An unexpected executable file download triggered by injected JavaScript
- Subsequent HTTP requests to the malicious domain hosting malware

### Observed Traffic Sequence

| Step | Protocol | Description |
|------|----------|-------------|
| 1 | DNS | Resolve `yummyrecipesforme.com` |
| 2 | DNS | DNS server returns legitimate IP |
| 3 | HTTP | Browser requests website content |
| 4 | HTTP | Malicious executable download initiated |
| 5 | DNS | Resolve `greatrecipesforme.com` |
| 6 | DNS | DNS server returns attacker IP |
| 7 | HTTP | Malware delivery from fake website |

## Attack Analysis

### Attack Type
- Brute Force Attack (Credential Guessing)
- Follow-on Malware Injection

### Attack Execution
The attacker repeatedly attempted login combinations against the administrative account using default credentials. Due to the absence of account lockout mechanisms and password complexity requirements, the correct password was eventually guessed.

Once authenticated, the attacker:
1. Injected malicious JavaScript into the website source code
2. Changed the administrative password, preventing legitimate access

### Impact on Users
Affected users reported:
- Forced file download prompts
- Browser redirection to an unknown domain
- Noticeable system performance degradation consistent with malware infection

## Detection and Investigation
The incident was detected when the website owner was unable to access the administrative panel. A cybersecurity response team reproduced the behavior in a sandbox environment and captured network traffic using tcpdump.

A senior analyst confirmed the compromise through source code inspection, identifying the injected JavaScript responsible for the malicious download and redirect behavior.

## Remediation and Hardening Recommendations

### Enforce Strong Authentication Controls
Default credentials significantly reduce the effort required for a successful brute force attack.

Recommended actions:
- Immediately change all default passwords
- Enforce password length and complexity requirements
- Prevent password reuse

### Implement Multi-Factor Authentication (MFA)
MFA would have blocked access even after credential compromise by requiring an additional verification factor beyond the password.

### Additional Defensive Measures
- Implement account lockout or rate limiting after repeated failed login attempts
- Monitor administrative login activity for anomalous behavior
- Conduct regular code integrity reviews of web applications

## Security Insight
Brute force attacks often succeed not because of advanced techniques, but due to weak operational security practices such as default credentials and missing authentication controls. Once administrative access is obtained, attackers can pivot quickly from account compromise to malware distribution, turning a single weak password into a large-scale user impact event.

This incident highlights the importance of layered defenses: strong passwords alone are insufficient without rate limiting, MFA, and continuous monitoring. Preventing initial access is significantly more effective than responding after malicious code has already been deployed.

*This project is part of my cybersecurity portfolio, completed as a hands-on exercise in network traffic analysis and DoS attack identification.*
``
