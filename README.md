Network Intrusion Detection Lab
Project Overview
This project demonstrates network traffic analysis and intrusion detection using Zeek and Suricata.
The objective was to capture network traffic, analyze PCAP files, create custom IDS rules, detect suspicious network activity, and document the investigation process.
Environment
Platform:
macOS
Traffic Capture:
tcpdump
Network Analysis:
Zeek 9.0.0
IDS:
Suricata 8.0.7
Additional Tools:
Nmap
curl
nslookup
Python HTTP Server
Git
Detection Scenarios
1. TCP Port Scan Detection
A SYN scan was performed against localhost using Nmap.
Command:
sudo nmap -sS -p 1-1000 127.0.0.1
The traffic was captured and analyzed with Zeek.
Zeek generated approximately 1000 connection records in conn.log.
A custom Suricata rule was created:
alert tcp any any -> any any (msg:"Possible TCP Port Scan"; flags:S; threshold:type both, track by_src, count 20, seconds 10; sid:1000001; rev:1;)
Detection Result:
Rule ID:
1000001
Alert:
Possible TCP Port Scan
MITRE ATT&CK:
T1046 Network Service Scanning
2. Suspicious HTTP Request Detection
A local Python HTTP server was used as the target:
127.0.0.1:8000
Suspicious HTTP requests were generated containing command injection and XSS-like patterns.
Example:
http://127.0.0.1:8000/?cmd=cat%20/etc/passwd
http://127.0.0.1:8000/?q=%3Cscript%3Ealert(1)%3C/script%3E
Custom Suricata Rules:
Rule 1000002:
Suspicious Command Injection Pattern
Rule 1000003:
Suspicious XSS Pattern
Both requests successfully generated Suricata alerts.
MITRE ATT&CK:
T1059 Command and Scripting Interpreter
T1190 Exploit Public-Facing Application
3. Suspicious DNS Detection
DNS queries containing long encoded-looking subdomains were generated.
Example:
c2VjcmV0ZGF0YTEyMzQ1Njc4OTA.example.com
Zeek successfully recorded the DNS queries in dns.log.
A custom Suricata rule was created to identify long Base64-like DNS labels.
Rule ID:
1000004
Alert:
Suspicious Long DNS Query
MITRE ATT&CK:
T1071.004 Application Layer Protocol: DNS
T1048 Exfiltration Over Alternative Protocol
Detection Workflow
Traffic Generation
Packet Capture
PCAP Analysis
Zeek Log Generation
Suricata Inspection
Custom Rule Matching
Alert Generation
Investigation
Remediation Recommendation
Skills Demonstrated
Network traffic analysis
PCAP analysis
Intrusion Detection Systems
Suricata
Zeek
Custom IDS rule creation
Network reconnaissance detection
HTTP traffic analysis
DNS traffic analysis
Nmap
tcpdump
MITRE ATT&CK mapping
Security investigation
Project Structure
network-intrusion-detection-lab/
README.md
methodology.md
remediation.md
detections/
01-port-scan-detection.md
02-suspicious-http-detection.md
03-suspicious-dns-detection.md
pcaps/
portscan.pcap
suspicious-http.pcap
suspicious-dns.pcap
rules/
local.rules
reports/
zeek-portscan/
zeek-dns/
suricata-portscan-custom/
suricata-http/
suricata-dns/
screenshots/
portscan-alert.png
suspicious-http-alert.png
suspicious-dns-alert.png
incident-reports/
final-network-investigation-report.md
Key Observation
Zeek and Suricata serve different purposes during network investigation.
Zeek provides detailed network metadata and protocol logs.
Suricata focuses on detecting suspicious traffic based on IDS rules.
Using both tools provides better visibility than relying on one tool alone.
Limitations
All activities were performed inside a controlled local laboratory.
The generated traffic was intentionally created for detection testing.
The custom IDS rules are simplified examples and would require tuning before production use.
Disclaimer
All network traffic and security testing were generated in a controlled environment for educational and portfolio purposes.