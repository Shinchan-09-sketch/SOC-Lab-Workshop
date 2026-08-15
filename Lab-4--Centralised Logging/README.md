# Lab-04: Centralized Logging and Log Analysis

## Aim

Collect, centralize, and analyze security logs from Windows, IIS, and Snort using Splunk Enterprise.

In this lab, students will verify that Windows Security Logs and IIS Logs are available in Splunk Enterprise and that Snort alerts generated on Ubuntu are forwarded to Splunk Enterprise through the Splunk Universal Forwarder.

The objective is to understand how a SOC analyst can bring different security data sources into a central platform for investigation and analysis.


---

## Step 1: Verify Splunk Enterprise

Open a web browser on the Windows host and navigate to:

```text
http://localhost:8000
```

Alternatively, use the host IP:

```text
http://192.168.184.1:8000
```

Log in using the Splunk Enterprise administrator credentials.

---

## Step 2: Verify the Splunk Receiving Port

Splunk Enterprise receives forwarded data from the Ubuntu Universal Forwarder through TCP port `9997`.

On the Windows host, open **Command Prompt as Administrator**.

Run:

```cmd
netstat -ano | findstr :9997
```

Verify that port `9997` is in the `LISTENING` state.

The receiving port should be:

```text
9997
```

This allows the Ubuntu Splunk Universal Forwarder to send Snort logs to Splunk Enterprise.

---

## Step 3: Verify the Ubuntu Universal Forwarder

On the Ubuntu VM, navigate to the Splunk Universal Forwarder directory:

```bash
cd /opt/splunkforwarder/bin
```

Check the Universal Forwarder status:

```bash
sudo ./splunk status
```

If the Universal Forwarder is not running, start it:

```bash
sudo ./splunk start
```

---

## Step 4: Verify the Forwarding Server

On Ubuntu, run:

```bash
sudo ./splunk list forward-server
```

The configured Splunk Enterprise destination should be:

```text
192.168.184.1:9997
```

> **Note:** `192.168.184.1` is the current Windows host IP. If the host IP changes, use the current Splunk Enterprise host IP.

---

## Step 5: Test Connectivity Between Ubuntu and Splunk Enterprise

From the Ubuntu VM, test the Splunk receiving port:

```bash
nc -zv 192.168.184.1 9997
```

A successful connection should display a message similar to:

```text
Connection to 192.168.184.1 9997 port [tcp/*] succeeded!
```

This confirms that the Ubuntu VM can communicate with Splunk Enterprise through TCP port `9997`.

---

## Step 6: Verify the Snort Log Monitor

The Snort alert file is:

```text
/var/log/snort/alert_fast.txt
```

On Ubuntu, run:

```bash
sudo ./splunk list monitor
```

Verify that the following file is being monitored:

```text
/var/log/snort/alert_fast.txt
```

If the file has not been added yet, configure it using:

```bash
sudo ./splunk add monitor /var/log/snort/alert_fast.txt -index main -sourcetype snort
```

Restart the Universal Forwarder:

```bash
sudo ./splunk restart
```

Then verify again:

```bash
sudo ./splunk list monitor
```

---

## Step 7: Verify Windows Security Logs

Open Splunk Enterprise.

Navigate to:

**Search & Reporting**

Search for Windows Security Events:

```text
index=main sourcetype="WinEventLog:Security"
```

To search specifically for successful logons:

```text
index=main sourcetype="WinEventLog:Security" EventCode=4624
```

Review the returned events.

Important fields may include:

- Account Name
- Logon Type
- Source Network Address
- Authentication Package
- Process Information
- Event Code

---

## Step 8: Verify IIS Logs

Search for IIS events in Splunk.

If the IIS sourcetype was configured as `iis`, use:

```text
index=main sourcetype=iis
```

If a different sourcetype was assigned, use the actual sourcetype shown in Splunk.

The IIS logs are collected from:

```text
C:\inetpub\logs\LogFiles\W3SVC1
```

Review the IIS events and identify:

- Client IP
- HTTP method
- Requested URI
- HTTP status code
- Request time

---

## Step 9: Verify Snort IDS Logs

Search for Snort alerts:

```text
index=main sourcetype=snort
```

The expected Snort alert from the previous lab is:

```text
LOCAL ICMP TEST ALERT
```

You can also search the Snort log path:

```text
source="/var/log/snort/alert_fast.txt"
```

If the Snort alert appears, the Ubuntu Universal Forwarder is successfully forwarding the alert to Splunk Enterprise.

---

## Step 10: Generate New Snort Activity

On Ubuntu, ensure Snort is running with the local ICMP detection rule.

Generate controlled traffic toward the Windows host:

```bash
ping -c 5 192.168.184.1
```

Check the Snort alert file:

```bash
sudo tail -n 10 /var/log/snort/alert_fast.txt
```

Verify that the alert appears:

```text
LOCAL ICMP TEST ALERT
```

---

## Step 11: Verify the New Alert in Splunk

Return to Splunk Enterprise and run:

```text
index=main sourcetype=snort
```

Verify that the newly generated Snort alert appears.

Expected alert:

```text
LOCAL ICMP TEST ALERT
```

This confirms the complete forwarding path:

```text
Network Traffic
      ↓
Snort IDS
      ↓
alert_fast.txt
      ↓
Splunk Universal Forwarder
      ↓
TCP 9997
      ↓
Splunk Enterprise
```

---

## Step 12: Analyze the Collected Logs

Use Splunk Search Processing Language (SPL) to analyze the collected events.

### Display recent events

```text
index=main
```

### Count events by host

```text
index=main | stats count by host
```

### Count events by source type

```text
index=main | stats count by sourcetype
```

### Search Windows Security Events

```text
index=main sourcetype="WinEventLog:Security"
```

### Search successful Windows logons

```text
index=main sourcetype="WinEventLog:Security" EventCode=4624
```

### Search IIS Logs

```text
index=main sourcetype=iis
```

### Search Snort Alerts

```text
index=main sourcetype=snort
```

These searches allow a SOC analyst to identify the different sources of security data and analyze the events collected by Splunk.

---

## Step 13: Compare the Different Log Sources

The workshop now contains three different types of security data.

### Windows Security Logs

Provide information about:

- User logons
- Authentication activity
- Logon types
- Security events

### IIS Logs

Provide information about:

- Client IP addresses
- HTTP methods
- Requested resources
- HTTP response codes
- Web activity

### Snort Alerts

Provide information about:

- Detected network traffic
- Detection rules
- Source and destination information
- Network security alerts

The SOC analyst can use these different sources together to understand what happened in the environment.

---

## Step 14: Perform a Centralized SOC Investigation

Use the available evidence to answer:

1. What activity occurred?
2. Which system generated the event?
3. Which log source contains the evidence?
4. Was the activity expected?
5. Are there related events from another source?
6. Does the activity require further investigation?
7. Can the activity be classified as legitimate or suspicious?

The investigation process is:

```text
Activity
   ↓
Logs / Alerts
   ↓
Collection
   ↓
Forwarding
   ↓
Splunk
   ↓
Investigation
   ↓
Decision
```

---

## Step 15: Correlate Security Evidence

A SOC analyst should not rely on a single event.

For example:

```text
Windows Security Event
        +
IIS Web Activity
        +
Snort Network Alert
        ↓
   SOC Investigation
```

Each log source provides a different view of activity in the environment.

Combining the evidence provides better context for making a security decision.

---

## Step 16: Understand Detection vs. Attack

The Snort ICMP alert generated during this lab was intentional.

The traffic was generated using:

```bash
ping -c 5 192.168.184.1
```

Therefore:

```text
Snort Alert ≠ Confirmed Attack
```

The alert is a **benign detection** because the traffic was intentionally generated for testing.

This demonstrates an important SOC principle:

> A SOC analyst does not immediately classify every alert as an attack. The analyst investigates the evidence, understands the context, and then makes a decision.

---

## Result

Successfully verified centralized security log collection and analysis using Splunk Enterprise.

Windows Security Logs and IIS Logs were analyzed on the Windows host, while Snort IDS alerts generated on Ubuntu were collected by the Splunk Universal Forwarder and forwarded to Splunk Enterprise through TCP port `9997`.

The collected data was searched and analyzed using Splunk Search Processing Language (SPL).

---

## Key Takeaways

- Splunk Enterprise acts as the central SOC analysis platform.
- Windows Security Logs provide authentication and security information.
- IIS Logs provide web server activity.
- Snort IDS detects network activity using configured rules.
- Snort alerts are stored in:

```text
/var/log/snort/alert_fast.txt
```

- The Splunk Universal Forwarder on Ubuntu monitors the Snort alert file.
- The Universal Forwarder sends Snort logs to Splunk Enterprise using TCP port `9997`.
- Splunk Search Processing Language can be used to analyze events from different sources.
- Multiple log sources provide better context during a SOC investigation.
- A detection does not automatically mean that an attack has occurred.
- SOC analysts must collect evidence, investigate context, correlate events, and then make a decision.

   ↓
Decision
```
