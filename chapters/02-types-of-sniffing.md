# Chapter 2 — Types of Sniffing

[← Previous: Introduction](01-introduction-to-sniffing.md) | [Back to README](../README.md) | [Next: Vulnerable Protocols →](03-vulnerable-protocols.md)

---

Sniffing isn't one single technique — there are two major approaches, and the difference between them determines whether a network even notices you're there.

## 2.1 Passive Sniffing

> **Passive sniffing** — you quietly listen. No network disturbance. Ideal for hub-based networks or open Wi-Fi.

In a hub-based network (or an open wireless network broadcasting to everyone), every device already receives every packet by design. The attacker simply sets their network card to promiscuous mode and listens — no packets are sent, no traffic is generated, nothing changes on the wire.

- **Detection difficulty:** Very high — there's no network footprint
- **Where it works:** Hubs (rare today), open/unencrypted Wi-Fi, wireless networks in monitor mode
- **Hinglish shorthand from the course:** *"Passive = chhupke sunna"* (passively listening in silence)

## 2.2 Active Sniffing

> **Active sniffing** — you create traffic (like fake ARP requests) to force packets your way. Mostly used on switch-based networks.

Modern networks almost universally use **switches**, not hubs. A switch is smart — it maintains a MAC address table and forwards each packet only to the port of the intended recipient. That means passive sniffing on a switched network gets you... nothing but your own traffic.

To sniff on a switch, an attacker has to actively manipulate the network — poisoning ARP tables, flooding MAC tables, or spoofing DHCP — to trick the switch into sending them traffic that isn't meant for them.

- **Detection difficulty:** Lower — active sniffing generates anomalous traffic that can be spotted (duplicate IPs, unusual ARP broadcasts)
- **Where it works:** Switch-based LANs (i.e., basically every modern office/home network)
- **Hinglish shorthand from the course:** *"Active = jab sirf sunna kaafi nahi hota, thoda jhatka dena padta hai"* (when just listening isn't enough, you have to give the network a little "shock")

## 2.3 MAC Flooding — Forcing a Switch to Act Like a Hub

One classic active technique is **MAC flooding**:

- Modern switches forward packets only to the intended device based on their MAC address table (CAM table)
- An attacker floods the switch with a massive number of fake MAC addresses
- The switch's CAM table fills up and it can no longer make forwarding decisions correctly
- **Fail-open behavior:** many switches, once overwhelmed, fall back to broadcasting every packet to every port — behaving like an old-fashioned hub
- The attacker, sitting in promiscuous mode, now receives everyone's traffic

**What happens, step by step:**
1. Switch fails to decide where to send traffic
2. It begins broadcasting to all ports, like a hub
3. The attacker (in promiscuous mode) gets all the data 🔥

> *"MAC flooding ke baad switch ne multitasking chhod di... sabko sab kuch bhejne laga!"* — after MAC flooding, the switch gave up trying to be selective and just broadcasts everything to everyone.

## 2.4 A Sneak Peek: DNS Poisoning / Redirection

Active sniffing is often paired with **DNS spoofing** to redirect traffic outright:

1. Attacker sends a **fake DNS response**
2. Victim's browser resolves a domain to the attacker's IP instead of the real server
3. Victim lands on an attacker-controlled site that looks identical to the real one
4. Attacker captures all credentials entered, via sniffing

This is a preview of the full **Man-in-the-Middle** playbook covered in [Chapter 4](04-mitm-arp-dhcp-spoofing.md) — sniffing isn't always just passive listening; it can escalate into full-on deception.

## 2.5 Tools Preview

You'll go deep on tools in [Chapter 5](05-sniffing-tools.md), but here's the quick map:

| Tool | Category |
|---|---|
| **Wireshark** | GUI, passive packet capture & analysis |
| **Tcpdump** | CLI, passive packet capture |
| **Ettercap** | GUI + CLI, *active* MITM-based sniffing |
| **Tshark, Bettercap, Cain & Abel** | Mixed passive/active, specialty use cases |

> "Tools toh sabke paas hote hain… seekhna ye hai ke unka use kitna samajhdari se karte ho." — Everyone has access to the same tools; what separates a professional from a script kiddie is *how responsibly and skillfully* those tools are used.

## 2.6 Chapter Recap

- **Passive sniffing** = silent listening, works on hubs/open Wi-Fi, nearly undetectable
- **Active sniffing** = generating traffic (ARP spoofing, MAC flooding, DHCP spoofing) to force a switched network to leak data
- **MAC flooding** exploits a switch's fail-open behavior under CAM table overflow
- Active sniffing sets the stage for full MITM attacks — the subject of the next chapter

---

[← Previous: Introduction](01-introduction-to-sniffing.md) | [Back to README](../README.md) | [Next: Vulnerable Protocols →](03-vulnerable-protocols.md)
