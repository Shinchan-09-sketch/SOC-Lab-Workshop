# Module 03: Log Management
- Lab-1: Local Logging: Configuring, Monitoring, and Analyzing Windows Logs
- Lab-2: Local Logging: Configuring, Monitoring, and Analyzing IIS Logs
- Lab-3: Local Logging: Configuring, Monitoring, and Analyzing Snort IDS Logs
- Lab-4: Centralized Logging: Collecting Logs from Different Devices into Centralized Location Using Splunk

## Lab Setup

[Click here to view the Lab Setup Guide](./Lab%20Setup.md)

# Lab Environment

| Machine | Operating System | Purpose |
|---------|------------------|---------|
| Windows Server 2019 | Windows Server 2019 | Splunk Enterprise |
| Windows Server 2025 | Windows Server 2025 | IIS + Universal Forwarder |
| Ubuntu | Ubuntu | Snort IDS + Universal Forwarder |



# Lab-01: Windows Event Log Collection

Collect Windows Event Logs using Splunk Universal Forwarder and analyze them in Splunk Enterprise.

➡️ **Open:** [Lab-1--Windows Event Log](./Lab-1--Windows%20Logs)

---

# Lab-02: IIS Log Monitoring

Monitor IIS Web Server logs and forward them to Splunk Enterprise.

➡️ **Open:** [Lab-2--IIS Logs](./Lab-2--IIS%20Logs)

---

# Lab-03: Snort IDS Log Monitoring

Collect Snort IDS alerts from Ubuntu and forward them to Splunk Enterprise.

➡️ **Open:** [Lab-3--Snort IDS](./Lab-3--Snort%20IDS)

---

# Lab-04: Centralized Logging

Collect logs from multiple systems and analyze them using Splunk Enterprise.

➡️ **Open:** [Lab-4--Centralised Logging](./Lab-4--Centralised%20Logging)


# Technologies Used:
- Splunk Enterprise
- Splunk Universal Forwarder
- Windows Server 2019
- Windows Server 2025
- Ubuntu
- IIS Web Server
- Snort IDS
- VMware Workstation Pro

# Learning Outcomes:
- Configure Splunk Enterprise
- Configure Splunk Universal Forwarder
- Collect Windows Event Logs
- Monitor IIS Logs
- Monitor Snort IDS Alerts
- Perform centralized log analysis
- Use Splunk Search Processing Language (SPL)
