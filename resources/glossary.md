# Glossary

[← Back to README](../README.md)

| Term | Definition |
|---|---|
| **ARP (Address Resolution Protocol)** | Maps IP addresses to MAC addresses on a local network segment; has no built-in authentication. |
| **ARP Poisoning / Spoofing** | Sending forged ARP replies to redirect traffic through an attacker's machine. |
| **Active Sniffing** | Sniffing that requires generating traffic (e.g., fake ARP requests) to intercept data on switched networks. |
| **Bettercap** | Modern, actively maintained MITM framework; successor to Ettercap and MITMf. |
| **CAM Table** | The MAC address table a switch uses to decide which port to forward a packet to. |
| **DHCP Spoofing** | Standing up a rogue DHCP server to hand victims a malicious gateway/DNS. |
| **Ettercap** | GUI+CLI tool for active MITM attacks: ARP poisoning, DNS spoofing, SSL stripping. |
| **HSTS (HTTP Strict Transport Security)** | A response header instructing browsers to never load a site over plain HTTP again. |
| **HTTPS Stripping / SSL Stripping** | Downgrading a victim's HTTPS session to HTTP mid-connection to sniff credentials. |
| **IDS/IPS** | Intrusion Detection/Prevention System — monitors traffic for malicious patterns. |
| **LSTM (Long Short-Term Memory)** | A recurrent neural network architecture well-suited to detecting time-based anomalies in sequential data, like ARP traffic over time. |
| **MAC Flooding** | Overwhelming a switch's CAM table with fake addresses to force fail-open (hub-like) broadcast behavior. |
| **MITM (Man-in-the-Middle)** | An attack where the adversary intercepts, monitors, and/or modifies communication between two parties without their knowledge. |
| **Passive Sniffing** | Listening to network traffic without generating any traffic of your own; works on hubs/open Wi-Fi. |
| **Promiscuous Mode** | A network card mode that captures all packets on a segment, not just those addressed to it. |
| **SHAP / LIME** | Explainable AI (XAI) techniques that show which input features caused a model's prediction — increasingly required for trustworthy AI-based intrusion detection. |
| **Tcpdump** | CLI packet-capture tool, fast and scriptable, commonly used on headless servers. |
| **Telnet** | A legacy remote-login protocol that transmits everything — including the login itself — in plain text. |
| **Wireshark** | The most widely used GUI-based network protocol analyzer for live capture and deep packet inspection. |
| **XArp** | A dedicated ARP-spoofing detection tool for end users and small networks. |

---

[← Back to README](../README.md)
