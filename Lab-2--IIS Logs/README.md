# Lab-02: Configuring, Monitoring and Analyzing IIS Logs

## Aim

Configure, monitor, and analyze IIS Web Server logs using Splunk Enterprise running on the Windows host.

In this lab, students will generate web activity, locate IIS log files, add the IIS log directory to Splunk Enterprise, and analyze HTTP requests and responses using Splunk.


---

## Step 1: Verify IIS Installation

Before configuring IIS log collection, verify that IIS is installed and running.

Press:

```text
Win + R
```

Enter:

```text
inetmgr
```

Press **Enter**.

The **IIS Manager** window should open.

Alternatively, open a web browser and navigate to:

```text
http://localhost
```

If IIS is running successfully, the default IIS welcome page should be displayed.

---

## Step 2: Verify the IIS Log Folder

Open **Command Prompt** and verify that the IIS log directory exists.

Run:

```cmd
dir C:\inetpub\logs\LogFiles
```

The command should display one or more log folders.

For the default IIS website, the log directory used in this lab is:

```text
C:\inetpub\logs\LogFiles\W3SVC1
```

---

## Step 3: Verify IIS Log Files

Navigate to the IIS log directory:

```cmd
dir C:\inetpub\logs\LogFiles\W3SVC1
```

The directory should contain IIS log files.

Example files may look like:

```text
u_ex260808.log
u_ex260809.log
u_ex260812.log
```

The exact filenames depend on the date on which the logs were generated.

To view a log file, use:

```cmd
type C:\inetpub\logs\LogFiles\W3SVC1\<log-file>
```

For example:

```cmd
type C:\inetpub\logs\LogFiles\W3SVC1\u_ex260812.log
```

If IIS has processed requests, the log file should contain HTTP request information.

> **Note:** A newly created IIS log file may initially be `0` bytes. Generate web activity and check the file again.

---

## Step 4: Generate IIS Web Activity

Open a web browser and generate requests to the IIS server.

Start with:

```text
http://localhost
```

Then generate additional requests:

```text
http://localhost/test
```

```text
http://localhost/admin
```

```text
http://localhost/login
```

These requests create entries in the IIS log.

You can also refresh the pages several times to generate additional events.

After generating activity, check the IIS log directory again:

```cmd
dir C:\inetpub\logs\LogFiles\W3SVC1
```

Then view the latest log file:

```cmd
type C:\inetpub\logs\LogFiles\W3SVC1\<latest-log-file>
```

---

## Step 5: Configure IIS Log Collection in Splunk Enterprise

Open Splunk Enterprise:

```text
http://localhost:8000
```

Alternatively, use the host IP:

```text
http://192.168.184.1:8000
```

Log in using the Splunk Enterprise administrator credentials.

Navigate to:

**Settings → Add Data**

Select:

**Files & Directories**

Select the option to monitor files and directories on the local Windows host.

Enter the IIS log directory:

```text
C:\inetpub\logs\LogFiles\W3SVC1
```

Select:

**Continuously monitor**

For the destination index, select:

```text
main
```

Configure the sourcetype as appropriate for the IIS data.

If you configure the sourcetype as:

```text
iis
```

it can later be searched using:

```text
index=main sourcetype=iis
```

> **Note:** If Splunk assigns a different sourcetype, use the actual sourcetype shown in the indexed events.

Save the configuration.

---

## Step 6: Generate Additional IIS Logs

Open a web browser and navigate to:

```text
http://localhost
```

Then generate additional requests:

```text
http://localhost/test
```

```text
http://localhost/admin
```

```text
http://localhost/login
```

Refresh the pages several times.

Each request should generate an entry in the IIS log.

Verify that the log file is receiving data:

```cmd
type C:\inetpub\logs\LogFiles\W3SVC1\<latest-log-file>
```

---

## Step 7: Understand IIS Log Fields

IIS logs contain information about HTTP requests and responses.

Important fields include:

| Field | Description |
|---|---|
| `c-ip` | Client IP address |
| `cs-method` | HTTP request method |
| `cs-uri-stem` | Requested resource or URL path |
| `sc-status` | HTTP response status code |

For example:

```text
c-ip
cs-method
cs-uri-stem
sc-status
```

These fields can help a SOC analyst understand who made a request, what resource was requested, and how the server responded.

---

## Step 8: Verify IIS Logs in Splunk

Open Splunk Enterprise.

Navigate to:

**Search & Reporting**

If the sourcetype was configured as `iis`, search:

```text
index=main sourcetype=iis
```

If a different sourcetype was assigned, search using the actual sourcetype shown in Splunk.

You can also search the main index:

```text
index=main
```

Then inspect the returned events and identify the IIS log entries.

---

## Step 9: Analyze HTTP Requests

Search for IIS events:

```text
index=main sourcetype=iis
```

Review the events and identify:

- Client IP address
- HTTP method
- Requested URI
- HTTP status code
- Request time
- Server response

For example, requests generated during this lab may include:

```text
/
 /test
 /admin
 /login
```

The SOC analyst can use these fields to understand normal web activity and identify requests that may require further investigation.

---

## Step 10: Investigate HTTP Status Codes

The `sc-status` field represents the HTTP response status code.

Common examples include:

| Status Code | Meaning |
|---|---|
| `200` | Successful request |
| `301` | Permanent redirect |
| `302` | Temporary redirect |
| `400` | Bad request |
| `401` | Unauthorized |
| `403` | Forbidden |
| `404` | Resource not found |
| `500` | Internal server error |

A status code alone does not confirm malicious activity.

The analyst should examine the requested resource, source IP, frequency of requests, and surrounding activity.

---

## Step 11: Investigate Requested Resources

Search for requests to a specific URI.

For example:

```text
index=main sourcetype=iis cs-uri-stem="/admin"
```

You can also search for:

```text
index=main sourcetype=iis cs-uri-stem="/login"
```

These searches help identify requests directed toward specific application resources.

Investigate whether the activity is:

- Expected
- Repeated
- From an unusual source
- Associated with unusual response codes
- Part of a larger pattern

---

## Step 12: Perform a Basic SOC Investigation

Use the IIS logs to answer:

1. What IP address generated the request?
2. What HTTP method was used?
3. What resource was requested?
4. What HTTP status code was returned?
5. Was the request expected?
6. Was the same resource requested repeatedly?
7. Does the activity require further investigation?

The investigation process is:

```text
Web Activity
     ↓
IIS Log
     ↓
Splunk
     ↓
Analysis
     ↓
Investigation
     ↓
Decision
```

A suspicious-looking request should be investigated using additional evidence rather than being immediately classified as an attack.

---

## Result

Successfully configured Splunk Enterprise to monitor the IIS log directory on the Windows host and analyzed IIS web server activity using Splunk.

The lab demonstrates how IIS logs can provide valuable evidence about web requests, client addresses, requested resources, and server responses during a SOC investigation.

---

## Key Takeaways

- IIS records HTTP activity in log files.
- IIS logs are stored under:

```text
C:\inetpub\logs\LogFiles\W3SVC1
```

- Web requests can be generated using a browser.
- Splunk Enterprise can directly monitor the IIS log directory.
- `c-ip` identifies the client IP address.
- `cs-method` identifies the HTTP method.
- `cs-uri-stem` identifies the requested resource.
- `sc-status` identifies the HTTP response status.
- A single unusual request does not automatically indicate an attack.
- SOC analysts should correlate web activity with other available evidence before making a decision.
