Suspicious DNS Query Detection
Objective:
Detect unusually long DNS subdomains that may indicate encoded data or possible DNS exfiltration activity.
PCAP:
pcaps/suspicious-dns.pcap
Traffic Generation:
Example DNS queries were generated using long encoded-looking subdomains.
Example:
aGVsbG93b3JsZA.example.com
c2VjcmV0ZGF0YTEyMzQ1Njc4OTA.example.com
VGhpc0xvb2tzTGlrZUVuY29kZWREYXRh.example.com
Zeek Analysis:
Zeek successfully parsed the DNS traffic and recorded the queries in dns.log.
The traffic included long subdomain labels that resembled encoded data.
Custom Suricata Rule:
alert dns any any -> any any (msg:"Suspicious Long DNS Query"; dns.query; pcre:"/^[A-Za-z0-9+/]{20,}./i"; sid:1000004; rev:1;)
Detection Result:
Rule ID:
1000004
Description:
Suspicious Long DNS Query
Protocol:
DNS
Analysis:
The rule detected DNS queries containing unusually long subdomain labels with characters commonly associated with encoded data.
Long encoded-looking subdomains can be used to transfer data through DNS queries.
This detection does not prove data exfiltration by itself, but it can identify traffic that should be investigated further.
Potential Security Impact:
DNS tunneling
DNS exfiltration
Command and control communication
Covert data transfer
MITRE ATT&CK:
T1048
Exfiltration Over Alternative Protocol
T1071.004
Application Layer Protocol: DNS
Investigation Steps:
Review the queried domain.
Inspect the length and structure of subdomains.
Check whether queries repeat frequently.
Look for Base64-like or hexadecimal patterns.
Review the source host.
Compare the traffic with normal DNS behavior.
Check whether the destination domain is trusted.
Remediation:
Monitor abnormal DNS query lengths.
Restrict DNS traffic to approved resolvers.
Block known malicious domains.
Use DNS filtering.
Investigate hosts producing repeated encoded-looking queries.
Monitor DNS logs for high-frequency unusual subdomains.
Evidence:
screenshots/suspicious-dns-alert.png
Sekarang project ke-4 kamu sudah punya tiga skenario yang cukup kuat:
Port Scan Detection
Suspicious HTTP Detection
Suspicious DNS Detection