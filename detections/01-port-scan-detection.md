Title: TCP Port Scan Detection

Objective:
Detect TCP port scanning activity using a custom Suricata IDS rule.

Traffic Source:
Local Nmap scan against 127.0.0.1.

PCAP:
pcaps/portscan.pcap

Traffic Generation:
sudo nmap -sS -p 1-1000 127.0.0.1

Zeek Analysis:
Zeek processed the captured traffic and generated approximately 1000 connection records in conn.log.

Custom Suricata Rule:
alert tcp any any -> any any (msg:"Possible TCP Port Scan"; flags:S; threshold:type both, track by_src, count 20, seconds 10; sid:1000001; rev:1;)

Detection Result:
Suricata successfully generated an alert for the scan activity.

Rule ID:
1000001

Description:
Possible TCP Port Scan

Protocol:
TCP

Analysis:
The scan generated a high number of TCP SYN packets across multiple destination ports.

The custom rule triggered when the configured threshold was reached.

This demonstrates how IDS rules can detect reconnaissance behavior based on connection frequency.

MITRE ATT&CK:
T1046
Network Service Scanning

Potential Impact:
Port scanning can be used during reconnaissance to identify exposed services and potential attack surfaces.

Remediation:
Monitor repeated connection attempts across multiple ports.
Restrict unnecessary exposed services.
Use firewall rules to limit access.
Investigate unexpected reconnaissance activity.
Segment sensitive network services.

Evidence:
screenshots/portscan-alert.png