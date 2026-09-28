Suspicious HTTP Request Detection
Objective:
Detect suspicious HTTP request patterns using custom Suricata IDS rules.
Traffic Source:
Local Python HTTP server running on:
127.0.0.1:8000
PCAP:
pcaps/suspicious-http.pcap
Traffic Generation:
Command Injection Pattern:
curl "http://127.0.0.1:8000/?cmd=cat%20/etc/passwd"
XSS Pattern:
curl "http://127.0.0.1:8000/?q=%3Cscript%3Ealert(1)%3C/script%3E"
Custom Suricata Rules:
Command Injection Detection:
alert http any any -> any any (msg:"Suspicious Command Injection Pattern"; http.uri; content:"cmd="; nocase; sid:1000002; rev:1;)
XSS Detection:
alert http any any -> any any (msg:"Suspicious XSS Pattern"; http.uri; content:"script"; nocase; sid:1000003; rev:1;)
Detection Results:
Rule ID:
1000002
Description:
Suspicious Command Injection Pattern
Protocol:
HTTP over TCP
Destination:
127.0.0.1:8000
Rule ID:
1000003
Description:
Suspicious XSS Pattern
Protocol:
HTTP over TCP
Destination:
127.0.0.1:8000
Analysis:
Suricata successfully inspected the HTTP URI and identified suspicious input patterns.
The first rule detected the presence of the cmd parameter.
The second rule detected the word script inside the HTTP URI.
This demonstrates how network IDS rules can inspect application-layer traffic and identify potentially malicious requests.
Potential Security Impact:
Suspicious HTTP input may indicate attempts to:
Execute operating system commands
Inject malicious scripts
Exploit vulnerable web applications
Perform reconnaissance against application endpoints
MITRE ATT&CK:
For command injection style activity:
T1059
Command and Scripting Interpreter
For web exploitation activity:
T1190
Exploit Public-Facing Application
Remediation:
Validate and sanitize user input.
Apply server-side input validation.
Use parameterized APIs where appropriate.
Apply output encoding for web content.
Deploy a Web Application Firewall where appropriate.
Monitor suspicious HTTP parameters.
Review repeated requests from the same source.
Evidence:
screenshots/suspicious-http-alert.png
Sekarang project ke-4 sudah punya dua detection yang bagus:
Port Scan Detection
Suspicious HTTP Detection