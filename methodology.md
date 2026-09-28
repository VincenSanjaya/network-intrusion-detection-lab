Network Intrusion Detection Methodology
Objective
The objective of this project was to build a basic network monitoring and intrusion detection workflow using packet capture, Zeek analysis, Suricata IDS rules, and security investigation.
Environment
Operating System:
macOS
Tools:
tcpdump
Zeek
Suricata
Nmap
curl
nslookup
Python HTTP Server
1. Traffic Generation
Controlled network activities were generated locally.
The activities included:
TCP port scanning
Suspicious HTTP requests
Encoded-looking DNS queries
2. Packet Capture
tcpdump was used to capture network traffic into PCAP files.
Each scenario used a separate capture.
Port Scan:
pcaps/portscan.pcap
HTTP:
pcaps/suspicious-http.pcap
DNS:
pcaps/suspicious-dns.pcap
3. Zeek Analysis
Zeek was used to analyze the PCAP files.
Example:
zeek -C -r capture.pcap
Zeek generated protocol-specific logs such as:
conn.log
dns.log
weird.log
These logs were used to understand network behavior before creating IDS detections.
4. Suricata Analysis
Suricata was used to inspect captured network traffic.
Custom IDS rules were stored in:
rules/local.rules
Rules were validated using:
suricata -T -S rules/local.rules
PCAP files were then processed using the custom rules.
5. Port Scan Detection
An Nmap SYN scan targeted ports 1 through 1000.
Zeek recorded approximately 1000 connections.
A Suricata threshold rule detected repeated TCP SYN packets from the same source.
6. HTTP Detection
A Python HTTP server was started locally.
Suspicious requests were generated containing:
cmd=
script
Suricata inspected the HTTP URI and triggered custom rules based on those patterns.
7. DNS Detection
DNS queries containing long encoded-looking subdomains were generated.
Zeek recorded the DNS activity in dns.log.
Suricata detected long DNS query labels using a regular expression rule.
8. Alert Validation
Generated alerts were reviewed using Suricata fast.log.
Each alert was validated against the original activity and PCAP.
9. MITRE ATT&CK Mapping
Detected activities were mapped to relevant MITRE ATT&CK techniques.
Port Scanning:
T1046
Suspicious HTTP:
T1059
T1190
Suspicious DNS:
T1071.004
T1048
10. Documentation
Each detection was documented with:
Objective
Traffic generation
PCAP source
Detection rule
Alert result
Analysis
MITRE mapping
Remediation
Evidence
Limitations
The project uses simulated traffic.
The rules focus on educational detection logic.
Production IDS rules require further tuning, baselining, and false-positive analysis.