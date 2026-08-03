# Lab-01: Windows Event Log Collection

## Aim

Configure Splunk Universal Forwarder on Windows Server 2025 to collect Windows Event Logs and forward them to Splunk Enterprise running on Windows Server 2019.

---

## Step 1: Verify Splunk Enterprise

Before configuring the Universal Forwarder, ensure that Splunk Enterprise is running on Windows Server 2019.

Open a web browser and navigate to

```
http://192.168.184.134:8000
```

Log in using the administrator credentials created during the installation.

---

## Step 2: Verify Receiving Port

Open Command Prompt as Administrator.

Navigate to the Splunk installation directory.

```cmd
cd "C:\Program Files\Splunk\bin"
```

Verify that Splunk is listening on port **9997**.

```cmd
netstat -ano | findstr 9997
```

If port **9997** is displayed in the LISTENING state, the Splunk server is ready to receive logs.

---

## Step 3: Install Splunk Universal Forwarder

On **Windows Server 2025**, install Splunk Universal Forwarder.

During the installation,

- Enter the Splunk Enterprise server IP address.
- Specify the receiving port **9997**.

```
192.168.184.134:9997
```

Complete the installation.

---

## Step 4: Configure outputs.conf

Navigate to

```
C:\Program Files\SplunkUniversalForwarder\etc\system\local
```

If **outputs.conf** does not exist, create it.

Add the following configuration.

```conf
[tcpout]
defaultGroup = default-autolb-group

[tcpout:default-autolb-group]
server = 192.168.184.134:9997
```

Save the file.

---

## Step 5: Configure inputs.conf

In the same directory, open or create **inputs.conf**.

Add the following configuration.

```conf
[WinEventLog://Application]
disabled = 0

[WinEventLog://System]
disabled = 0

[WinEventLog://Security]
disabled = 0
```

Save the file.

---

## Step 6: Restart the Universal Forwarder

Open Command Prompt as Administrator.

Restart the Splunk Universal Forwarder service.

```cmd
net stop SplunkForwarder
net start SplunkForwarder
```

---

## Step 7: Verify Log Collection

Open Splunk Enterprise.

Navigate to **Search & Reporting**.

Search for Windows Event Logs.

```spl
source="WinEventLog:System"
```

Similarly, verify the remaining log sources.

```spl
source="WinEventLog:Application"
```

```spl
source="WinEventLog:Security"
```

If events are displayed, the Universal Forwarder is successfully forwarding Windows Event Logs to Splunk Enterprise.

---

## Result

Successfully configured Splunk Universal Forwarder to collect Windows Event Logs from Windows Server 2025 and forward them to Splunk Enterprise running on Windows Server 2019.
