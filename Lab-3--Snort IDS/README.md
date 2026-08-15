# Lab-03: Configuring, Monitoring and Analyzing Snort IDS Logs

## Aim

Configure, monitor, and analyze Snort IDS alerts on Ubuntu and forward the generated Snort logs to Splunk Enterprise running on the Windows host.

In this lab, students will create a custom Snort detection rule, generate controlled network traffic, verify the resulting alert, and use the Splunk Universal Forwarder to send Snort alerts to Splunk Enterprise.


---

## Step 1: Verify Snort Installation

Before starting the lab, ensure that Snort is installed on the Ubuntu VM.

Open the terminal and run:

```bash
snort -V
```

The installed Snort version should be displayed.

Also verify the Snort executable:

```bash
which snort
```

Expected output:

```text
/usr/local/bin/snort
```

---

## Step 2: Identify the Network Interface

Use the following command to identify the active network interface:

```bash
ip addr
```

In the current Ubuntu lab environment, the interface is:

```text
ens33
```

> **Note:** The interface name may be different on another system. Students should use the interface shown on their own Ubuntu VM.

---

## Step 3: Verify the Snort Configuration

Validate the Snort configuration using:

```bash
sudo snort -c /usr/local/etc/snort/snort.lua -T
```

If the configuration is valid, Snort should report that the configuration test was successful.

---

## Step 4: Create the Local Snort Rule

Create the Snort rules directory:

```bash
sudo mkdir -p /usr/local/etc/snort/rules
```

Create the local rules file:

```bash
sudo nano /usr/local/etc/snort/rules/local.rules
```

Add the following rule:

```text
alert icmp any any -> any any (msg:"LOCAL ICMP TEST ALERT"; sid:1000001; rev:1;)
```

Save the file and exit.

### Rule Explanation

| Field | Description |
|---|---|
| `alert` | Generate an alert when the rule matches |
| `icmp` | Detect ICMP traffic |
| `any any` | Any source IP and source port |
| `->` | Traffic direction |
| `any any` | Any destination IP and destination port |
| `msg` | Alert message |
| `sid` | Unique Snort rule identifier |
| `rev` | Rule revision |

The rule detects ICMP traffic such as `ping`.

---

## Step 5: Create the Snort Log Directory

Create the directory where Snort alerts will be stored:

```bash
sudo mkdir -p /var/log/snort
```

The Snort alert file used in this lab is:

```text
/var/log/snort/alert_fast.txt
```

---

## Step 6: Validate the Local Rule

Test the Snort configuration together with the local rule:

```bash
sudo snort -c /usr/local/etc/snort/snort.lua -R /usr/local/etc/snort/rules/local.rules -T
```

Make sure the configuration test completes successfully before running Snort.

---

## Step 7: Run Snort IDS

Start Snort using the Ubuntu network interface:

```bash
sudo snort -c /usr/local/etc/snort/snort.lua -R /usr/local/etc/snort/rules/local.rules -i ens33 -A alert_fast -l /var/log/snort
```

Keep this terminal open while Snort is running.

> **Note:** If the Ubuntu network interface is not `ens33`, replace `ens33` with the interface identified in Step 2.

---

## Step 8: Generate Controlled Network Traffic

Open another terminal on the Ubuntu VM.

Generate ICMP traffic toward the Windows host:

```bash
ping -c 5 192.168.184.1
```

The current Windows host IP is:

```text
192.168.184.1
```

> **Note:** Students should verify the current host IP before running the test.

The ping traffic should trigger the local Snort ICMP detection rule.

---

## Step 9: Verify the Snort Alert

Check the Snort alert file:

```bash
sudo cat /var/log/snort/alert_fast.txt
```

You can also display the latest alerts using:

```bash
sudo tail -n 10 /var/log/snort/alert_fast.txt
```

The alert should contain:

```text
LOCAL ICMP TEST ALERT
```

This confirms that Snort successfully detected the ICMP traffic.

---

## Step 10: Configure the Splunk Universal Forwarder

The Splunk Universal Forwarder is installed on the Ubuntu VM.

Navigate to the Universal Forwarder directory:

```bash
cd /opt/splunkforwarder/bin
```

Check the Universal Forwarder status:

```bash
sudo ./splunk status
```

If the forwarder is not running, start it:

```bash
sudo ./splunk start
```

---

## Step 11: Configure Snort Log Monitoring and Forwarding

Add the Snort alert file as a monitored input:

```bash
sudo ./splunk add monitor /var/log/snort/alert_fast.txt -index main -sourcetype snort
```

Configure the Splunk Enterprise server as the forwarding destination.

For the current lab environment, the Windows host is:

```text
192.168.184.1
```

Add the forwarding server:

```bash
sudo ./splunk add forward-server 192.168.184.1:9997
```

> **Note:** If the host IP is different in another lab environment, replace `192.168.184.1` with the actual Splunk Enterprise host IP.

---

## Step 12: Restart and Verify the Universal Forwarder

Restart the Universal Forwarder:

```bash
sudo ./splunk restart
```

Verify the configured forwarding server:

```bash
sudo ./splunk list forward-server
```

The output should show:

```text
192.168.184.1:9997
```

Verify that the Snort alert file is being monitored:

```bash
sudo ./splunk list monitor
```

The monitored file should include:

```text
/var/log/snort/alert_fast.txt
```

---

## Step 13: Test the Connection to Splunk Enterprise

From the Ubuntu VM, test connectivity to the Splunk Enterprise receiving port:

```bash
nc -zv 192.168.184.1 9997
```

A successful connection should display a message similar to:

```text
Connection to 192.168.184.1 9997 port [tcp/*] succeeded!
```

This confirms that the Ubuntu VM can communicate with the Splunk Enterprise receiving port.

---

## Step 14: Generate Another Snort Alert

Generate ICMP traffic again:

```bash
ping -c 5 192.168.184.1
```

Check the latest Snort alerts:

```bash
sudo tail -n 10 /var/log/snort/alert_fast.txt
```

Verify that the alert appears:

```text
LOCAL ICMP TEST ALERT
```

---

## Step 15: Verify Snort Logs in Splunk

Open Splunk Enterprise on the Windows host:

```text
http://192.168.184.1:8000
```

Navigate to:

**Search & Reporting**

Search for:

```text
index=main sourcetype=snort
```

The Snort alert should appear in the search results.

Look for:

```text
LOCAL ICMP TEST ALERT
```

---

## Step 16: Analyze the Snort Alert

Review the alert and identify:

- Alert message
- Source IP
- Destination IP
- Protocol
- Time of detection
- Rule SID

The custom rule used in this lab is:

```text
sid:1000001
```

The alert was generated because ICMP traffic matched the local detection rule.

---

## Step 17: Perform a Basic SOC Investigation

Use the Snort alert and surrounding information to answer:

1. What traffic triggered the alert?
2. What was the source IP address?
3. What was the destination IP address?
4. Which rule generated the alert?
5. Was the traffic expected?
6. Was the activity generated intentionally?
7. Does the event require further investigation?

The investigation process is:

```text
Network Activity
       ↓
Snort Detection
       ↓
Alert
       ↓
Log Collection
       ↓
Splunk
       ↓
Investigation
       ↓
Decision
```

---

## Important SOC Observation

The Snort alert in this lab is a **benign detection**.

The ICMP traffic was generated intentionally using:

```bash
ping -c 5 192.168.184.1
```

Therefore, the presence of the alert does **not** mean that an attack occurred.

This demonstrates an important SOC principle:

> Detection does not automatically mean confirmed attack.

The analyst must investigate the context of an alert before deciding whether the activity is malicious.

---

## Result

Successfully configured Snort IDS on Ubuntu to detect ICMP traffic using a custom detection rule.

The generated Snort alerts were written to:

```text
/var/log/snort/alert_fast.txt
```

The Splunk Universal Forwarder on Ubuntu was configured to monitor the Snort alert file and forward the events to Splunk Enterprise running on the Windows host through TCP port `9997`.

The Snort alert was successfully verified in Splunk using:

```text
index=main sourcetype=snort
```

---

## Key Takeaways

- Snort IDS can detect network activity using detection rules.
- Custom Snort rules can be created for controlled security testing.
- ICMP traffic can be detected using an ICMP alert rule.
- Snort writes alerts to the configured log directory.
- The Splunk Universal Forwarder can monitor Snort log files.
- The Ubuntu Universal Forwarder forwards Snort alerts to Splunk Enterprise.
- TCP port `9997` is used for Splunk log forwarding.
- Splunk can centralize and search Snort alerts.
- A detection is not automatically a confirmed attack.
- SOC analysts must investigate the context before making a security decision.
