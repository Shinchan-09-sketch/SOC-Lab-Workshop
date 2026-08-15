# Module 03: Log Management
- Lab-1: Local Logging: Configuring, Monitoring, and Analyzing Windows Logs
- Lab-2: Local Logging: Configuring, Monitoring, and Analyzing IIS Logs
- Lab-3: Local Logging: Configuring, Monitoring, and Analyzing Snort IDS Logs
- Lab-4: Centralized Logging: Collecting Logs from Different Devices into Centralized Location Using Splunk


# Lab Environment

| Machine | Operating System | Purpose |
|---------|------------------|---------|
| Host | Windows | Splunk Enterprise + IIS|
| Ubuntu VM | Ubuntu | Snort IDS + Universal Forwarder |



# Lab-01: Windows Event Log Collection

Learn how to configure, monitor, and analyze Windows Security Event Logs using Windows Event Viewer and Splunk Enterprise.

➡️ **Open:** [Lab-1--Windows Event Log](./Lab-1--Windows%20Logs)

---

# Lab-02: IIS Log Monitoring

Learn how to monitor IIS Web Server logs, generate web activity, and analyze HTTP requests and responses using Splunk Enterprise.

➡️ **Open:** [Lab-2--IIS Logs](./Lab-2--IIS%20Logs)

---

# Lab-03: Snort IDS Log Monitoring

Configure Snort IDS on Ubuntu, generate controlled network traffic, detect activity using a custom rule, and analyze the resulting alerts.

➡️ **Open:** [Lab-3--Snort IDS](./Lab-3--Snort%20IDS)

---

# Lab-04: Centralized Logging

Configure the Splunk Universal Forwarder on Ubuntu to collect Snort alerts and forward them to Splunk Enterprise for centralized monitoring and analysis.

➡️ **Open:** [Lab-4--Centralised Logging](./Lab-4--Centralised%20Logging)


# Technologies Used
- Splunk Enterprise
- Splunk Universal Forwarder
- Windows Event Viewer
- IIS Web Server
- Ubuntu
- Snort IDS
- VMware Workstation Pro
- Splunk Search Processing Language (SPL)


# Learning Outcomes
After completing this workshop, students will be able to:

- Configure and use Splunk Enterprise
- Monitor and analyze Windows Security Logs
- Understand Windows Event ID 4624 and common logon types
- Monitor and analyze IIS Web Server logs
- Configure and test Snort IDS on Ubuntu
- Create custom Snort detection rules
- Generate controlled network traffic and analyze Snort alerts
- Configure the Splunk Universal Forwarder on Ubuntu
- Forward Snort logs to Splunk Enterprise
- Perform centralized security log analysis using Splunk
- Use Splunk Search Processing Language (SPL)
- Distinguish between security alerts and confirmed attacks
