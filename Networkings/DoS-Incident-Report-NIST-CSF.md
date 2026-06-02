# Incident Report Analysis

**Summary**  
A Denial of Service (DoS) attack targeted the company’s internal network for approximately two hours. The attack involved a flood of ICMP packets that overwhelmed the network due to an unconfigured firewall, making internal network resources unavailable. The incident was resolved by blocking malicious traffic and implementing new security controls.

| Category   | Description |
|------------|-------------|
| **Identify** | The organization has identified a critical vulnerability within our internal network infrastructure: the primary firewall was left unconfigured. This security gap directly exposes the digital servers and network resources required to deliver our core business services (web design, graphic design, and social media marketing), leaving the company highly susceptible to operational downtime from Denial of Service (DoS) attacks. |
| **Protect** | To prevent future incidents, the organization implemented:<br>• New firewall rules to limit the rate of incoming ICMP packets<br>• Source IP address verification to check for spoofed IP addresses<br>• Network monitoring software to detect abnormal traffic patterns<br>• An IDS/IPS system to filter suspicious ICMP traffic |
| **Detect** | The attack was detected when:<br>• Network services suddenly became unresponsive<br>• Monitoring systems identified abnormal spikes in ICMP traffic<br>• Alerts indicated unusual traffic patterns affecting availability |
| **Respond** | The incident response team took the following actions:<br>• Blocked incoming ICMP packets at the firewall<br>• Took all non-critical network services offline to reduce load<br>• Restored critical network services to maintain business continuity<br>• Investigated the root cause (misconfigured firewall) |
| **Recover** | After containment:<br>• Affected systems were restored to normal operation<br>• Firewall configurations were updated and hardened<br>• Additional monitoring and detection tools (IDS/IPS) were deployed<br>• A post-incident review was conducted to improve future incident response and prevention strategies |

**Reflections/Notes:**  
This incident highlights the importance of proper firewall configuration and the need for layered security controls (defense-in-depth). Implementing rate limiting, anti-spoofing measures, and continuous monitoring significantly improves resilience against DoS attacks. Regular audits of firewall rules and network configurations should be part of ongoing security practices to prevent similar vulnerabilities.
