# Methodology — AI-Driven Phishing Simulation Lab

## Overview

This document outlines the step-by-step methodology used to design, execute, and detect a simulated AI-assisted phishing campaign in a controlled lab environment.


## Red Team Methodology

### Step 1: Lab Environment Setup

- Installed VirtualBox on Windows host machine
- Downloaded and imported Kali Linux 2026.1 as the attacker VM
- Configured VirtualBox Extension Pack for enhanced VM capabilities
- Verified network connectivity inside Kali Linux

### Step 2: GoPhish Installation

- Downloaded GoPhish v0.12.1 for Linux 64-bit
- Extracted the binary and configured execution permissions
- Launched GoPhish with sudo privileges
- Accessed the admin dashboard at https://127.0.0.1:3333
- Reset default credentials and secured the admin account

### Step 3: Phishing Infrastructure Setup

**Email Template Creation:**
- Navigated to Email Templates in GoPhish dashboard
- Created a spear phishing template impersonating an IT department
- Used AI-assisted language to craft a convincing urgent password reset message
- Configured envelope sender as it-support@company.com
- Added GoPhish tracking variable {{.URL}} for click tracking

**Landing Page Creation:**
- Created a fake IT Security Portal login page
- Built custom HTML form with username and password fields
- Enabled Capture Submitted Data and Capture Passwords options
- Configured redirect to legitimate site after credential submission

**Target Configuration:**
- Created a test target group with a disposable test email
- Configured sending profile with fake IT department identity

### Step 4: Campaign Execution

- Launched phishing campaign targeting the test group
- Retrieved unique phishing URL with RID parameter from GoPhish database using SQLite3
- Simulated victim interaction by clicking phishing link and submitting credentials
- Verified credential capture in GoPhish campaign results

### Step 5: Evidence Collection

- Recorded campaign metrics (Email Sent, Opened, Clicked, Submitted)
- Exported captured credentials from GoPhish database
- Documented attack timeline and campaign results


## Blue Team Methodology

### Step 1: SIEM Platform Setup

- Provisioned Splunk Cloud free trial instance
- Configured Splunk for log ingestion and analysis
- Accessed Search and Reporting interface

### Step 2: Log Analysis

Ran the following SPL queries to simulate SOC analyst detection workflow:

**Query 1 — General log overview:**
```
index=* | head 10
```

**Query 2 — Internal Splunk logs:**
```
index=_internal | head 10
```

**Query 3 — Error event filtering:**
```
index=_internal level=ERROR | head 20
```

### Step 3: Alert Identification

Analyzed log events to identify:
- Authentication failures (Token not found)
- Misconfigured security settings (Vault script path errors)
- Administrative access patterns (IP, timestamp, user agent)
- Database connection anomalies (MongoDB errors)

### Step 4: Timeline Correlation

- Mapped event timestamps to build chronological attack timeline
- Identified burst events (multiple logs within same second) as indicators of automated activity
- Correlated access logs with source IP addresses


## 360° Workflow Summary

```
Red Team                          Blue Team
─────────                         ─────────
GoPhish Setup          →          Splunk SIEM Setup
Email Template         →          Log Ingestion
Fake Landing Page      →          SPL Query Analysis
Campaign Launch        →          Error Detection
Credential Capture     →          Access Pattern Mapping
Campaign Metrics       →          Timeline Correlation
```


## Ethical Considerations

- All testing conducted on self-owned virtual machines
- Only disposable test email accounts used as targets
- No real credentials or personal data collected
- Lab environment fully isolated from production networks
- Project conducted strictly for educational purposes
