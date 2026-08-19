# Chapter 5 — Tools for Sniffing (Wireshark, Tcpdump, Ettercap & More)

[← Previous: MITM — ARP & DHCP Spoofing](04-mitm-arp-dhcp-spoofing.md) | [Back to README](../README.md) | [Next: SSL Stripping →](06-ssl-stripping.md)

---

## 5.1 Why Tools Matter

You can know the theory of sniffing all day — but without the right tool, you're just a listener, not an analyst.

- **GUI tools** = great for beginners and visual investigation
- **CLI tools** = fast and scriptable, built for professionals and automation
- **MITM tools** = used for *active* sniffing and full attack simulation

## 5.2 Wireshark — The King of Packet Sniffing

**What it is:**
- Free, open-source network protocol analyzer
- GUI-based → excellent for live captures and deep packet inspection

**Core features:**
- Capture live packets from any network interface
- Filter by IP, protocol, port, MAC address
- Follow TCP streams to rebuild entire conversations (e.g., a full HTTP login POST)
- Save capture logs for offline analysis

**Common filter examples:**
```
ip.addr == 192.168.26.134 && http
http.request.method == "POST"
tcp.port == 80
```

> "Wireshark is like X-ray vision for your internet traffic — hackers can literally watch a website skeleton being built packet by packet."

## 5.3 Tcpdump — The Hacker's Terminal Weapon

**What it is:**
- CLI-based packet capture tool
- Super fast, minimal, and ideal for scripting

**Command examples:**
```bash
tcpdump -i eth0 tcp port 80
tcpdump -nn -v -X
```

**Real-world use:**
- Used on headless servers (no GUI available)
- Output can be piped directly into Wireshark for deeper analysis
- Preferred in stealth operations or digital forensics work, where a lightweight footprint matters

> "Tcpdump is Wireshark's older, command-line cousin. Looks boring — behaves like a snake in the grass."

## 5.4 Ettercap — The MITM Beast

**What it is:**
- An *active* MITM attack tool (not just passive capture)
- Available in both GUI and CLI modes
- Supports ARP poisoning, DNS spoofing, packet filtering, and SSL stripping

**Why it's popular:**
- Plug-and-play ARP poisoning
- Fake certificate injection
- Real-time password sniffing from unencrypted protocols like FTP and Telnet

> "Ettercap doesn't knock… it walks in, says 'hi' to the gateway, and takes all the data."

See the [full hands-on lab](../labs/lab-ettercap-mitm.md) for step-by-step Ettercap commands.

## 5.5 Tool Comparison Table

| Tool | Type | Interface | Best For |
|---|---|---|---|
| **Wireshark** | Passive | GUI | Deep packet analysis |
| **Tcpdump** | Passive | CLI | Fast capture, scripting |
| **Ettercap** | Active | GUI + CLI | MITM attacks / active sniffing |

> **Pro tip:** Use Tcpdump to *collect* traffic, then Wireshark to *analyze* it. Use Ettercap to actively test vulnerabilities in an authorized lab.

## 5.6 Other Noteworthy Tools (Quick Shoutout)

| Tool | Notes |
|---|---|
| **Bettercap** | Modern alternative to Ettercap; supports HTTPS stripping, MITM6 (IPv6-based attacks) |
| **Tshark** | The CLI version of Wireshark — same engine, scriptable output |
| **Cain & Abel** | Windows-only; ARP poisoning + password recovery (largely legacy today) |
| **MITMf** *(deprecated)* | Python-based MITM framework, superseded by Bettercap |

> **Reminder:** Tools don't make you a hacker — understanding their power (and the damage they can do) does.

## 5.7 Chapter Recap

- Wireshark = deep, GUI-based passive analysis
- Tcpdump = fast, scriptable, CLI-based passive capture
- Ettercap = active MITM attack execution (ARP poisoning, SSL stripping, credential capture)
- The right workflow often combines tools: capture with Tcpdump, analyze in Wireshark, attack (in an authorized lab) with Ettercap or Bettercap

**Next up:** the full Ettercap-based lab, followed by a deep dive into **SSL Stripping** — the technique that shows why "https://" alone isn't a magic shield.

---

[← Previous: MITM — ARP & DHCP Spoofing](04-mitm-arp-dhcp-spoofing.md) | [Back to README](../README.md) | [Next: SSL Stripping →](06-ssl-stripping.md)
