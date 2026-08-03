# Lab-04: Centralized Logging and Log Analysis

## Aim

Verify that logs from Windows Server 2025 and Ubuntu are successfully collected and analyzed using Splunk Enterprise running on Windows Server 2019.

---

## Step 1: Verify Splunk Enterprise

Open a web browser and navigate to

```
http://192.168.184.134:8000
```

Log in using the Splunk administrator credentials.

---

## Step 2: Verify Connected Forwarders

Open **Search & Reporting** and verify that data is being received from all configured machines.

Execute the following search.

```spl
| stats count by host
```

The output should display the host names of the connected machines along with the number of events received.

---

## Step 3: Verify Available Log Sources

To verify the different types of logs received by Splunk Enterprise, execute the following search.

```spl
| stats count by sourcetype
```

The output should display all configured source types such as:

- WinEventLog
- IIS
- Snort

---

## Step 4: Verify Windows Event Logs

Search for Windows Event Logs.

```spl
source="WinEventLog:System"
```

or

```spl
index=main sourcetype=WinEventLog:System
```

Verify that Windows Event Logs from Windows Server 2025 are displayed.

---

## Step 5: Verify IIS Logs

Search for IIS Web Server logs.

```spl
sourcetype=iis
```

Verify that IIS access logs are successfully indexed.

---

## Step 6: Verify Snort IDS Logs

Search for Snort IDS alerts.

```spl
sourcetype=snort
```

or

```spl
source="/var/log/snort/snort.alert.fast"
```

Verify that Snort alerts generated on Ubuntu are displayed.

---

## Step 7: Analyze the Collected Logs

Use different SPL (Search Processing Language) queries to analyze the collected events.

Example:

Display the latest events.

```spl
index=main
```

Display the total number of events received from each host.

```spl
| stats count by host
```

Display the total number of events for each source type.

```spl
| stats count by sourcetype
```

---

## Result

Successfully verified centralized log collection from Windows Server 2025 and Ubuntu using Splunk Enterprise running on Windows Server 2019. All logs were indexed successfully and could be searched and analyzed using Splunk Search Processing Language (SPL).
