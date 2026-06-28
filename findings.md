# Findings — AI-Driven Phishing Simulation Lab

## Executive Summary

This document presents the key findings from a controlled phishing simulation lab. The project successfully demonstrated how AI-assisted phishing attacks are executed, how credentials are harvested, and how a SOC analyst detects suspicious activity using SIEM tools.


## Red Team Findings

### Finding 1: AI Enhances Phishing Effectiveness

Traditional phishing emails are easy to detect due to poor grammar and generic content. AI-assisted phishing emails are:
- Professionally worded with proper grammar
- Personalized to the target context (IT department impersonation)
- Designed to create urgency (password expiry alert)
- Significantly harder to distinguish from legitimate communications

**Impact:** High — AI-crafted emails dramatically increase the likelihood of victim interaction.


### Finding 2: Credential Harvesting is Trivially Easy

Using GoPhish and a simple HTML form, a fully functional credential harvesting page was deployed in under 10 minutes. The fake IT portal:
- Captured username and password in plaintext
- Stored credentials in a local database
- Redirected victims to a legitimate page after submission (reducing suspicion)

**Impact:** Critical — Victims had no indication their credentials were captured.


### Finding 3: Campaign Metrics Reveal Full Attack Lifecycle

GoPhish provided real-time tracking of the entire phishing lifecycle:

| Metric | Result |
|---|---|
| Email Sent | 1 |
| Email Opened | 1 |
| Clicked Link | 1 |
| Submitted Data | 1 |
| Email Reported | 0 |

**Observation:** The victim (test user) completed the full attack chain without reporting the email — a common real-world outcome demonstrating the effectiveness of social engineering.


### Finding 4: Unique RID Parameters Enable Precise Tracking

Each phishing link contained a unique RID (Result ID) parameter:
```
http://127.0.0.1/?rid=rwzbmAG
```

This allowed GoPhish to:
- Track individual victim interactions
- Record exact timestamps of each action
- Map the complete victim journey from email to credential submission

**SOC Implication:** Defenders can use URL parameter analysis to identify phishing infrastructure and track campaign scope.


## Blue Team Findings

### Finding 5: SIEM Detected Authentication Failures

Splunk logs revealed multiple ERROR level events indicating authentication issues:

```
ERROR ScsTokenHandler - Token not found
ERROR ScsConfig - Vault script path is empty
```

**SOC Interpretation:** Token authentication failures are a key indicator of:
- Unauthorized access attempts
- Misconfigured security controls
- Potential credential stuffing attacks

**Recommended Alert Rule:** Trigger SOC alert when ERROR level authentication events exceed 3 occurrences within 60 seconds.


### Finding 6: Access Logs Revealed Attacker Fingerprint

Splunk access logs captured detailed information about system access:

```
IP Address  : 49.43.234.124
User        : sc_admin
Action      : GET request
Timestamp   : 26/Jun/2026 17:24:45
User Agent  : Mozilla/5.0 (X11; Linux x86_64; Firefox/140.0)
```

**SOC Interpretation:** This log entry provides a complete attacker fingerprint including IP address, access time, and browser/OS information — critical for incident response and threat hunting.


### Finding 7: Burst Events Indicate Automated Activity

Multiple log events were recorded within the same second, differentiated only by milliseconds:

```
17:24:45.907 → GET request logged
17:24:45.910 → Token error logged
17:24:45.911 → Request ID generated
```

**SOC Interpretation:** Millisecond-level burst events are strong indicators of:
- Automated attack tools
- Bot-driven credential stuffing
- Scripted reconnaissance activity

Human attackers cannot operate at millisecond speed — this pattern is a reliable automated attack indicator.


### Finding 8: MongoDB Connection Errors Indicate Infrastructure Stress

Splunk logs showed unexpected MongoDB connection terminations:

```
msg: Connection ended
msg: Error receiving request from client. Ending connection from remote
```

**SOC Interpretation:** Unexpected database connection drops during an active session can indicate:
- Resource exhaustion from attack traffic
- Forced connection termination by security controls
- Potential data exfiltration attempts


## Attack Timeline Reconstruction

Based on log analysis, the following attack timeline was reconstructed:

```
Phase 1 — Reconnaissance
└─ Attacker sets up GoPhish infrastructure

Phase 2 — Weaponization  
└─ AI-crafted phishing email template created
└─ Fake credential harvesting page deployed

Phase 3 — Delivery
└─ Phishing email sent to target
└─ Email opened by victim

Phase 4 — Exploitation
└─ Victim clicks phishing link (RID tracked)
└─ Victim lands on fake IT portal

Phase 5 — Credential Capture
└─ Victim submits username and password
└─ Credentials stored in GoPhish database

Phase 6 — Detection (Blue Team)
└─ SIEM logs reveal authentication errors
└─ Access patterns mapped by IP and timestamp
└─ Burst events identified as automated activity indicators
```


## Recommendations

1. **Deploy email filtering** — Flag emails with urgent password reset requests from external domains
2. **Enable MFA** — Multi-factor authentication prevents credential-only attacks
3. **Configure SIEM alerts** — Set thresholds for authentication failures and burst events
4. **User awareness training** — Regular phishing simulation training reduces click rates
5. **Monitor access logs** — Flag admin access from unknown IP addresses in real time
6. **URL inspection** — Block URLs containing suspicious RID parameters or unknown domains


## Conclusion

This lab successfully demonstrated the full lifecycle of an AI-assisted phishing attack and the corresponding SOC detection workflow. The project highlights that:

- AI dramatically lowers the barrier for sophisticated phishing attacks
- Open-source tools like GoPhish make credential harvesting accessible
- SIEM platforms like Splunk are effective at detecting attack indicators
- SOC analysts must correlate multiple log sources to build a complete attack picture

The gap between attack sophistication and defender detection capability is narrowing — making continuous SOC training and SIEM tuning critical for modern organizations.
