# SOC Detection Project: Brute-Force Attack (Active Directory + Splunk)

## Project Overview
This project demonstrates detection of brute-force login attempts in an Active Directory environment using Splunk SIEM.

The lab simulates real-world SOC monitoring by collecting Windows Security logs and identifying suspicious authentication activity.

## Attack Scenario
An attacker attempts multiple login attempts using invalid credentials on a domain user account.

The system logs these attempts as Event ID 4625 in Windows Security logs. These logs are forwarded to Splunk, where repeated failures are analyzed to detect brute-force behavior.

---

## Technologies Used
- Windows Server 2022 (Active Directory)
- Splunk Enterprise
- Splunk Universal Forwarder
- Windows Event Logs

---

## Lab Setup

### Active Directory Users
![AD Setup](images/1-ad-setup.png)

- Created domain: company.local
- Created users: Nisha, Priya, Vijay
- Organized under Sales → Employees OU

---

### Audit Policy Configuration
![Audit Policy](images/2-audit-policy.png)

Enabled:
- Audit logon events → Success, Failure
- Audit account logon events → Success, Failure

---

### Simulated Brute-Force Attack
![Failed Login](images/3-failed-login.png)

- Attempted multiple failed logins using user Priya
- Incorrect password used repeatedly

---

### Event Viewer Logs (Event ID 4625)
![Event Viewer](images/4-event-viewer.png)

- Event ID: 4625
- Description: Failed login attempt
- Failure reason: Bad username or password

---

### Splunk Data Ingestion
![Splunk Input](images/5-splunk-input.png)

- Added Windows Event Logs (Security)
- Logs successfully ingested into Splunk

---

### Splunk Log Analysis
![Splunk Logs](images/6-splunk-logs.png)

- Verified failed login events in Splunk
- Source: Windows Security Logs

---

### Detection Query
![Detection](images/7-attack-detection.png)
index=* EventCode=4625
| stats count by Account_Name, src_ip
| where count > 5
| sort -count

## Alerting
A Splunk alert can be configured to trigger when failed login attempts exceed a defined threshold (e.g., more than 5 attempts).

This allows real-time detection and response to brute-force attacks.

---

## Detection Logic
- Monitors failed login attempts (Event ID 4625)
- Flags accounts with multiple failures
- Helps identify brute-force attacks
- Can be extended to detect password spray attacks by analyzing multiple accounts from a single source IP

---

## Key Learnings
- Active Directory log management
- Windows Security Event analysis
- Splunk data ingestion and querying
- Brute-force attack detection

---

## Future Improvements
- Create real-time alerts in Splunk
- Add dashboard visualization
- Integrate with SOAR tools

## Why This Project Matters
Brute-force attacks are one of the most common techniques used by attackers to gain unauthorized access to systems.

This project demonstrates how a SOC analyst can detect such attacks early using log analysis and SIEM tools like Splunk, helping prevent potential security breaches.

