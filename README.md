**\
\
Mac Firewall and Network Investigation Lab\
File name: Mac_network_investigation_lab**

**Introduction — Mac Firewall and Network Investigation Lab**

In this lab, a real-world firewall and network investigation was performed using a personal MacBook Pro system to generate authentic network activity and security-related datasets. The purpose of this lab was to simulate the type of network visibility and traffic analysis commonly performed by SOC analysts, incident responders, and network security professionals in enterprise environments. Instead of relying on publicly available datasets, live system activity from the local machine was used to create realistic investigation data containing active network connections, listening services, encrypted traffic, local communication, and packet capture information.

Throughout the investigation, multiple datasets were generated from the macOS environment to provide visibility into network interfaces, active TCP and UDP sessions, open ports, local services, encrypted HTTPS communication, and packet-level traffic. These datasets were then organized into structured log and capture files that can later be reused for additional cybersecurity investigations, Splunk SIEM ingestion, packet analysis, threat hunting exercises, and firewall monitoring scenarios. The lab demonstrates how a local machine can be transformed into a practical cybersecurity investigation environment using native system activity and real network traffic.\
\
Main Goals
===========================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================

In this lab I will learn how to:

inspect live network connections,\
identify listening ports,\
monitor active sessions,\
inspect processes using the network,\
analyze firewall/network logs,\
detect suspicious IPs,\
view TCP/UDP activity,\
and perform basic threat hunting from terminal.\
\
Terminal Tools We Will Use
================================================

| **Tool** | **Purpose** |
|----------|-------------|

| ifconfig | View network interfaces and IP addresses |
|----------|------------------------------------------|

| netstat | View network connections and ports |
|---------|------------------------------------|

| lsof | See which process owns a port |
|------|-------------------------------|

| ping | Connectivity testing |
|------|----------------------|

| traceroute | Trace network path |
|------------|--------------------|

| tcpdump | Packet capture |
|---------|----------------|

| grep | Search logs |
|------|-------------|

| awk | Extract fields |
|-----|----------------|

| sort | Organize output |
|------|-----------------|

| uniq | Show unique values |
|------|--------------------|

| wc  | Count results |
|-----|---------------|

| head/tail | View beginning/end of logs |
|-----------|----------------------------|

**\
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
Generate Real Data on Mac (Data Collection & Dataset Creation)\
\
Opening Terminal personal MacBook pro\
\
Step1-12 Live investigation & monitoring\**
We will investigate:

interfaces,\
IP addresses,\
active connections,\
listening ports,\
HTTPS traffic,\
packet captures,\
live monitoring.

**Step 1 — Identify personal MacBook pro Network Interfaces\
Command:** ifconfig**\**
\
This shows:

Wi-Fi interface\
local IP address\
MAC address\
active network adapters\
\
**Step 2 — Show Active Network Connections\
Command: netstat -an\
\**
This displays:

active TCP sessions,\
listening ports,\
remote IP connections,\
UDP activity.

**Step 3 — Show Listening Ports**\
Command: **lsof -i -P -n\**
\
It shows:

process name,\
PID,\
protocol,\
local port,\
remote IP,\
connection state.\
\
Example:

Google 1234 user TCP 192.168.1.193:51234-\>142.250.x.x:443

It means: Chrome/Google process connected to remote HTTPS server on port 443.\
\
**Step 4 — Find Processes Listening on Ports\
\**
Command: **lsof -iTCP -sTCP:LISTEN -n -P\**
This identifies:

services listening on your machine,\
potentially exposed ports,\
applications accepting incoming connections.\
\
Step 5 — Investigate Specific Ports
=============================================

Command: **lsof -i :443**

or

**lsof -i :22**

This checks:

HTTPS traffic,\
SSH activity,\
who owns the port,\
active sessions.\
\
Step 6 — Real-Time Connection Monitoring
========================================

Command: **nettop**

This gives live network activity:

bandwidth usage,

active connections,\
process communication.

**Step 7 — Packet Capture (Very Important SOC Skill)**

Command: **sudo tcpdump -i en0**

This captures:

packets,\
protocols,\
IP addresses,\
traffic flow.

**Step 8 — Capture Only HTTP/HTTPS Traffic**

For HTTPS using command:

**sudo tcpdump -I en0 port 443**

For HTTP using command:

**sudo tcpdump -i en0 port 80**

This is useful for:

suspicious outbound traffic,\
malware traffic investigation,\
encrypted traffic monitoring.\
\
Step 9 — Save Traffic to PCAP File
==================================

Command: **sudo tcpdump -i en0 -w network_capture.pcap**

This creates: **network_capture.pcap\
\
Can be analyzed by:**

Wireshark,\
tcpdump,\
Splunk,\
Zeek,\
Suricata.

**Step 10 — Detect External Connections**

Command: **netstat -an \| grep ESTABLISHED**

This shows active remote connections.

Useful for:

suspicious IP investigation,\
malware beaconing,\
outbound communication analysis.\
\
Step 11 — Count Connections Per IP
==================================

Command: **netstat -an \| grep ESTABLISHED \| awk '{print \$5}' \| cut -d. -f1-4 \| sort \| uniq -c \| sort -nr**

This helps identify:

top communicating IPs,\
suspicious repetitive connections,\
possible scanning or beaconing behavior.\
\
Step 12 — Investigate DNS Activity
=========================================

Command: **sudo tcpdump -i en0 port 53**

Very important for:

malware detection,\
command-and-control domains,\
suspicious DNS traffic.\
monitors DNS queries\
\
\
Step 13 through 17 Dataset generation\
\
Step 13 — Create Investigation Folder
======================================

Command: cd ~/Downloads\
mkdir mac_network_investigation_lab\
cd mac_network_investigation_lab\
pwd\
\
Step 14 — Create ifconfig Dataset
====================================

Commasnd: ifconfig \> mac_ifconfig.log

ls -lh

**Step 15 — Create netstat Dataset**

Command: netstat -an \> mac_netstat.log\
ls -lh\
\
\
\
Step 16 — Create Listening Ports Dataset
========================================

Command: lsof -i -P -n \> listening_ports.log\
**ls -lh**\
\
\
\
**Step 17 — Create Packet Capture Dataset**

Command: sudo tcpdump -i en0 -c 200 -w mac_network_capture.pcap\
\
ls -lh

open browser\
visit websites\
generate traffic\
\
**Files created\**
mac_ifconfig.log\
mac_netstat.log\
listening_ports.log\
mac_network_capture.pcap\
\
**Screenshots step 1 through 17\
\
\**
\**
\**
\
\**

\
\
\
\

\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
\
**SOC Skills Covered**

This lab maps to:

Network Security Monitoring\
Threat Hunting\
Firewall Investigation\
Packet Analysis

Traffic Inspection\
Incident Response\
Tier-1 SOC Analyst Skills\
Basic Tier-2 Investigation\
\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\
\
\
\
===========================================================================================================================================

\
=
