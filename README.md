# AI-Driven Phishing Simulation Lab

A controlled red team vs blue team cybersecurity project simulating AI-assisted phishing campaigns and SOC detection workflows.



## Project Overview

This project demonstrates a full 360° phishing simulation — from attack execution (Red Team) to detection and analysis (Blue Team). It was built to understand how AI enhances modern phishing attacks and how SOC analysts detect and respond to them.



## Tools Used

|Tool|Purpose|
|-|-|
|Kali Linux|Attacker operating system|
|GoPhish|Phishing campaign framework|
|Splunk Cloud|SIEM log analysis and alert detection|
|VirtualBox|Lab virtualization|
|Firefox|Browser-based attack simulation|
|SQLite3|GoPhish database inspection|



## Project Structure


phishing-simulation-lab/
├── red-team/
│   ├── email-templates/
│   ├── screenshots/
│   └── fake-login-page/
├── blue-team/
│   ├── siem-alerts/
│   └── sandbox-reports/
├── docs/
│   ├── methodology.md
│   └── findings.md
└── README.md




## Red Team — Attack Simulation

### 

### Phase 1: Environment Setup

* Deployed Kali Linux inside VirtualBox as the attacker machine
* Installed and configured GoPhish phishing framework
* Set up a local phishing server on port 80



### Phase 2: Campaign Creation

* Created an AI-assisted phishing email template simulating an IT department password reset alert
* Built a fake credential harvesting login page mimicking an internal IT portal
* Configured target group and sending profile
* Launched phishing campaign via GoPhish



### Phase 3: Results

* Successfully simulated email delivery, link click, and credential submission
* Captured submitted credentials (username + password) via GoPhish database
* Campaign metrics recorded:

  * Email Sent: 1
  * Email Opened: 1
  * Clicked Link: 1
  * Submitted Data: 1



## 

## Blue Team — Detection \& Analysis



### Phase 1: SIEM Setup

* Configured Splunk Cloud free trial as the SIEM platform
* Ingested internal system logs and access logs



### Phase 2: Log Analysis

* Ran SPL (Search Processing Language) queries to detect suspicious activity
* Identified authentication failures (Token not found errors)
* Detected admin access logs with IP, timestamp, and browser fingerprint
* Identified unexpected MongoDB connection terminations



### Phase 3: Key Detections

* ERROR level events filtered and analyzed
* Access patterns mapped by IP address and user agent
* Event timestamps correlated to build attack timeline





## Key Findings

1. AI-crafted phishing emails are significantly more convincing than traditional templates
2. Credential harvesting pages can be built and deployed in minutes using open-source tools
3. SIEM tools like Splunk can detect authentication failures and suspicious access patterns in real time
4. Millisecond-level timestamp differences in logs indicate automated/batch attack activity
5. Token authentication errors and vault misconfigurations are common indicators of unauthorized access attempts



## Skills Demonstrated

* Red team attack simulation using GoPhish
* Phishing campaign management and tracking
* SIEM log analysis using Splunk SPL queries
* SOC alert identification and interpretation
* Linux terminal operations (Kali Linux, SQLite3)
* Virtualization lab setup (VirtualBox)
* Security documentation and reporting





## 

