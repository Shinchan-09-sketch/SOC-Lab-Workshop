# Lab-02: Configuring, Monitoring and Analyzing IIS Logs

## Aim

Configure Splunk Universal Forwarder to collect IIS Web Server logs from Windows Server 2025 and forward them to Splunk Enterprise running on Windows Server 2019.

---

## Step 1: Verify IIS Installation

Before configuring Splunk Universal Forwarder, verify that IIS Web Server is installed and running.

Open **Run** (`Win + R`) and type

```cmd
inetmgr
```

The IIS Manager window should open.

Alternatively, open a web browser and navigate to

```
http://localhost
```

If IIS is running successfully, the default IIS welcome page will be displayed.

---

## Step 2: Verify IIS Log Folder

Open Command Prompt and verify that the IIS log directory exists.

```cmd
dir C:\inetpub\logs\LogFiles
```

The command should display one or more log folders.

---

## Step 3: Verify IIS Log Files

Navigate to the default IIS log folder.

```cmd
dir C:\inetpub\logs\LogFiles\W3SVC1
```

If IIS has processed requests, one or more log files will be present.

---

## Step 4: Configure inputs.conf

Navigate to

```
C:\Program Files\SplunkUniversalForwarder\etc\system\local
```

Open **inputs.conf**.

If the file does not exist, create it.

Add the following configuration.

```conf
[monitor://C:\inetpub\logs\LogFiles]
disabled = 0
sourcetype = iis
index = main
recursive = true
```

Save the file.

---

## Step 5: Restart Splunk Universal Forwarder

Open Command Prompt as Administrator.

Restart the Splunk Universal Forwarder service.

```cmd
net stop SplunkForwarder
net start SplunkForwarder
```

---

## Step 6: Generate IIS Logs

Open a web browser.

Navigate to

```
http://localhost
```

Refresh the page **5–10 times**.

Each request generates entries in the IIS log files.

---

## Step 7: Verify IIS Logs in Splunk

Open Splunk Enterprise.

Navigate to **Search & Reporting**.

Search for IIS logs.

```spl
sourcetype=iis
```

If events are displayed, IIS logs are being successfully forwarded to Splunk Enterprise.

---

## Result

Successfully configured Splunk Universal Forwarder to monitor IIS Web Server logs and forward them to Splunk Enterprise for centralized monitoring and analysis.
