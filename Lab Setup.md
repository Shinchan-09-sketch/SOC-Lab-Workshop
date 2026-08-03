# Lab Setup

Before starting the SOC labs, we need to create and configure the required virtual machines. The lab environment consists of one Splunk server, one Windows server generating IIS logs, and one Ubuntu machine running Snort IDS.

---
## Step 1: Create the Virtual Machines

Create the following virtual machines using VMware Workstation.

### Windows Server 2019

Allocate the following resources.

- 2 CPU Cores
- 4 GB RAM
- 60 GB Disk
- NAT Network Adapter

This machine will be used to install Splunk Enterprise.

---

### Windows Server 2025

Allocate the following resources.

- 2 CPU Cores
- 4 GB RAM
- 60 GB Disk
- NAT Network Adapter

This machine will be used to install IIS Web Server and Splunk Universal Forwarder.

---

### Ubuntu

Allocate the following resources.

- 2 CPU Cores
- 2 GB RAM
- 30 GB Disk
- NAT Network Adapter

This machine will be used to install Snort IDS and Splunk Universal Forwarder.

---

## Step 2: Configure Network

Connect all virtual machines to the same VMware NAT network so they can communicate with each other.

Assign the following IP addresses.

| Machine | IP Address |
|---------|------------|
| Windows Server 2019 | 192.168.184.134 |
| Windows Server 2025 | 192.168.184.129 |
| Ubuntu | 192.168.184.130 |

After assigning the IP addresses, verify connectivity by pinging each machine from the others.

Windows

```cmd
ping 192.168.184.129
ping 192.168.184.130
```

Ubuntu

```bash
ping 192.168.184.134
ping 192.168.184.129
```

---

## Step 3: Install Splunk Enterprise

Install Splunk Enterprise on **Windows Server 2019**.

After installation,

1. Start Splunk Enterprise.
2. Create the administrator account.
3. Open the Splunk Web Interface.

```
http://192.168.184.134:8000
```

---

## Step 4: Enable Receiving Port

Open Command Prompt as Administrator.

Navigate to the Splunk installation directory.

```cmd
cd "C:\Program Files\Splunk\bin"
```

Enable port **9997**.

```cmd
splunk enable listen 9997
```

Verify that the port is listening.

```cmd
netstat -ano | findstr 9997
```

---

## Step 5: Install Splunk Universal Forwarder

Install Splunk Universal Forwarder on

- Windows Server 2025
- Ubuntu

During installation, specify the Splunk Enterprise server.

```
192.168.184.134:9997
```

---

## Step 6: Verify the Setup

Verify that

- Splunk Web opens successfully.
- All virtual machines can communicate with each other.
- Port **9997** is listening.
- Universal Forwarder is installed successfully on Windows Server 2025 and Ubuntu.

The environment is now ready to perform the SOC workshop labs.
