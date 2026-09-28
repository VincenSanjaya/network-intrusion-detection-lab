Network Detection Remediation Guide
Purpose
This document describes investigation and remediation recommendations for the network activity detected during the project.
1. TCP Port Scanning
Potential Risk
Port scanning may indicate reconnaissance before an attack.
Attackers can use scanning to discover:
Open ports
Running services
Attack surfaces
Potential vulnerable systems
Investigation
Identify the source IP.
Determine how many destination ports were contacted.
Review the scan timeframe.
Check whether the source is authorized.
Review other activity from the same source.
Remediation
Close unnecessary ports.
Restrict access using firewall rules.
Segment sensitive network services.
Monitor repeated connection attempts.
Investigate unexpected reconnaissance activity.
Use IDS or IPS monitoring on sensitive networks.
2. Suspicious Command Injection Requests
Potential Risk
Command injection attempts may allow an attacker to execute operating system commands through a vulnerable application.
Investigation
Review the HTTP request.
Identify suspicious parameters.
Review the source IP.
Check server and application logs.
Determine whether the command was executed.
Search for related requests.
Remediation
Validate input on the server.
Avoid executing user-controlled input through system commands.
Apply allowlists where possible.
Use secure APIs instead of shell commands.
Restrict application privileges.
Monitor repeated suspicious parameters.
3. Suspicious XSS Requests
Potential Risk
Cross-Site Scripting may allow attacker-controlled JavaScript to execute in a user's browser.
Investigation
Review the HTTP payload.
Identify affected parameters.
Determine whether input was stored or reflected.
Check whether JavaScript execution occurred.
Remediation
Apply contextual output encoding.
Sanitize untrusted HTML.
Use safe DOM APIs.
Implement Content Security Policy.
Validate user-controlled input.
4. Suspicious DNS Queries
Potential Risk
Long or encoded DNS subdomains may indicate:
DNS tunneling
Data exfiltration
Command and control
Covert communication
Investigation
Identify the originating host.
Review queried domains.
Analyze query length.
Look for Base64 or hexadecimal patterns.
Check query frequency.
Compare with normal DNS behavior.
Investigate repeated queries to unusual domains.
Remediation
Restrict DNS traffic to approved resolvers.
Use DNS filtering.
Block malicious domains.
Monitor abnormal DNS query lengths.
Detect unusually high DNS request volumes.
Investigate endpoints generating encoded-looking DNS queries.
5. False Positive Handling
Custom IDS rules may generate false positives.
Each alert should be validated before escalation.
Review:
Source
Destination
Protocol
Payload
Frequency
Business context
6. Detection Tuning
Production IDS rules should be continuously tuned.
Recommended improvements:
Adjust thresholds.
Add trusted network exceptions.
Improve protocol-specific matching.
Add flow conditions.
Add application context.
Review false positives.
Monitor rule performance.
7. Incident Response Workflow
Recommended process:
Detection
Validation
Investigation
Classification
Containment
Remediation
Recovery
Retesting
Documentation
incident-reports/final-network-investigation-report.md
Network Intrusion Detection Lab
Final Investigation Report
Project Overview
This project simulated common suspicious network activities and analyzed them using Zeek and Suricata.
Three detection scenarios were successfully implemented.
Scenario 1
TCP Port Scan
Activity:
Nmap SYN scan against:
127.0.0.1
Ports:
1 to 1000
Packet Capture:
pcaps/portscan.pcap
Zeek Result:
Approximately 1000 network connection records were generated.
Suricata Detection:
Rule ID:
1000001
Alert:
Possible TCP Port Scan
MITRE ATT&CK:
T1046 Network Service Scanning
Analysis:
The large number of TCP SYN packets sent to multiple destination ports matched reconnaissance behavior.
The Suricata threshold rule successfully detected the scan.
Scenario 2
Suspicious HTTP Activity
Target:
127.0.0.1:8000
Traffic included a command injection-style request:
?cmd=cat /etc/passwd
and an XSS-style request:
<script>alert(1)</script>

Suricata Results
Rule ID:
1000002
Alert:
Suspicious Command Injection Pattern
Rule ID:
1000003
Alert:
Suspicious XSS Pattern
Analysis:
Suricata successfully inspected application-layer HTTP traffic and detected suspicious URI patterns.
This demonstrated that network IDS rules can identify potential web exploitation attempts.
Scenario 3
Suspicious DNS Activity
Encoded-looking DNS queries were generated using long subdomains.
Zeek Result:
The queries were successfully recorded in dns.log.
Suricata Result:
Rule ID:
1000004
Alert:
Suspicious Long DNS Query
Analysis:
Long encoded-looking DNS labels may indicate covert data transfer.
The alert does not confirm exfiltration by itself, but identifies traffic requiring further investigation.
Incident Investigation Workflow
The investigation process followed this workflow:
Identify Alert
Review Rule
Inspect Source and Destination
Review Protocol
Analyze PCAP
Review Zeek Logs
Compare Related Events
Map to MITRE ATT&CK
Determine Security Impact
Recommend Remediation
Detection Summary
Port Scan
Rule:
1000001
MITRE:
T1046
Status:
Detected
Suspicious Command Injection
Rule:
1000002
MITRE:
T1059
Status:
Detected
Suspicious XSS
Rule:
1000003
MITRE:
T1190
Status:
Detected
Suspicious DNS
Rule:
1000004
MITRE:
T1071.004 / T1048
Status:
Detected
Key Findings
Custom IDS rules successfully detected all three simulated network attack patterns.
Zeek provided network visibility and protocol metadata.
Suricata provided rule-based intrusion detection.
Using both tools provided complementary information during investigation.
Skills Demonstrated
Packet capture
PCAP investigation
Zeek analysis
Suricata IDS
Custom IDS rules
Network reconnaissance detection
HTTP traffic inspection
DNS investigation
MITRE ATT&CK mapping
Alert validation
Incident investigation
Conclusion
The Network Intrusion Detection Lab successfully demonstrated a complete basic network security monitoring workflow.
The project covered:
Traffic generation
Packet capture
Network analysis
Custom IDS detection
Alert validation
Incident investigation
MITRE ATT&CK mapping
Remediation recommendations
The project also demonstrated how Zeek and Suricata can be combined to provide both network visibility and automated threat detection.
