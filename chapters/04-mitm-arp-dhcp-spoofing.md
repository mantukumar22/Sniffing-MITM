# Chapter 4 — MITM Attacks: ARP Poisoning & DHCP Spoofing

[← Previous: Vulnerable Protocols](03-vulnerable-protocols.md) | [Back to README](../README.md) | [Next: Sniffing Tools →](05-sniffing-tools.md)

---

## 4.1 What Is a MITM Attack?

A **Man-in-the-Middle (MITM)** attack is when a hacker places themselves between you and the server you're communicating with — intercepting everything, without you knowing.

**Simple example:** Imagine you're sending a letter to a friend. A third person intercepts it, reads it, maybe edits it, and then sends it along. You never notice anything unusual — but they now know everything.

> **MITM = Intercept + Monitor + Modify**

### Visualizing the attacker's position

```
Normal:   [Your PC] <───── original connection ─────> [Internet/Server]

MITM:     [Your PC] <──┐                        ┌──> [Internet/Server]
                        └──> [Attacker/Phisher] ──┘
                          "new connection" — attacker
                          relays both sides, seeing everything
```

In a router-based network, this looks like:

```
        [Attacker] <--- Requests/Responses ---> [Gateway/Router] <---> [Internet]
                                  ▲
                                  │
        [User]     <--- Requests/Responses --------┘
```

The attacker inserts themselves logically between the victim and the gateway, relaying every request and response while reading (and potentially modifying) the traffic in transit.

## 4.2 ARP Poisoning — The Hacker's Shortcut

**ARP = Address Resolution Protocol.** It maps IP addresses to MAC addresses inside a local network — essentially answering the question "who is 192.168.1.5, and what's their hardware address?"

**ARP Poisoning (a.k.a. ARP Spoofing)** = the attacker sends *forged* ARP replies to both the victim and the router:

- To the router: *"Hey router, I'm the victim!"*
- To the victim: *"Hey victim, I'm the router!"*

Because ARP has **no built-in authentication**, both sides simply believe the fake replies and update their ARP tables accordingly. From that point on, all traffic between the victim and the router flows *through the attacker's machine* first.

> "ARP table ka confusion hacker ke liye golden opportunity hai!" — The trust-everything nature of ARP is a golden opportunity for an attacker.

**Tools commonly used:** Ettercap, Cain & Abel, Bettercap

### Worked example with real addresses

```
IP: 10.1.5.6         Hacker
MAC: 06:01:a2:52:ff
        │
        │ "10.1.5.11 is at 06:01:a2:52:ff" (lie sent to Router)
        ▼
IP: 10.1.5.3          Router
MAC: 04:14:6b:21:c4  ───────────► Internet
        │
        │ "10.1.5.3 is at 06:01:a2:52:ff" (lie sent to Victim)
        ▼
IP: 10.1.5.11         Victim
MAC: 02:11:ab:55:ee
```

- The **Hacker tells the Router**: "I am the Victim" (poisons the router's ARP cache)
- The **Hacker tells the Victim**: "I am the Router" (poisons the victim's ARP cache)
- Now every packet either side sends is routed *through the attacker first*

### Real-life impact of ARP poisoning

- Students sniffing each other's traffic over campus Wi-Fi
- Attackers in cafés stealing Facebook/Instagram session cookies
- Internal employees snooping on corporate networks they're not authorized to access
- **2019:** ARP spoofing on hotel Wi-Fi led to multiple online banking sessions being intercepted in a real, documented incident

> "Victim thinks they're safe on HTTPS… but the attacker is already sitting in on the handshake."

## 4.3 DHCP Spoofing — The Fake Gateway Trap

**DHCP** normally assigns IP addresses automatically when you connect to a network.

**DHCP Spoofing** = the attacker stands up a **rogue DHCP server** and races the legitimate one to respond to the victim's DHCP request first. If the attacker wins, the victim is handed:

- A **fake IP address**
- A **fake gateway** (pointing to the attacker)
- A **fake DNS server** (also controlled by the attacker)

From that point, *everything* the victim sends — web requests, DNS lookups, logins — routes through the attacker by design, without any ARP trickery needed.

> **Analogy:** "It's like checking into a hotel and the receptionist gives you room keys and CCTV access… but she's not even hotel staff."

**Tools commonly used:** Yersinia, Bettercap, DHCPig

## 4.4 The Full Flow of a MITM Attack

```
1. Attacker joins the same network
2. Attacker scans for victims
3. Attacker spoofs ARP and/or DHCP
4. Victim starts sending traffic → attacker intercepts it
5. Attacker captures credentials, cookies, sessions
```

From a single successful MITM session, an attacker can:

- Read usernames and passwords
- Download files transferred over HTTP, FTP, or Telnet
- Hijack active sessions via stolen cookies
- Modify data *in transit* — injecting JavaScript, altering page content, redirecting downloads

> "Tumko lagta hai bank ka site open hai… Hacker ko lagta hai paisa nikalne ka time aa gaya hai." — You think you're on your bank's website; the attacker thinks it's payday.

## 4.5 Why MITM Is So Dangerous

Unlike brute-force attacks or DDoS, **MITM is silent**. There's no error message, no lag, no visible clue that anything is wrong. It's:

- A digital pickpocket
- A fake Wi-Fi network with very real financial and privacy consequences

> "MITM doesn't knock… it enters, loots, and leaves before you know it happened."

## 4.6 Chapter Recap

- **MITM** = an attacker inserting themselves between victim and server to intercept, monitor, and modify traffic
- **ARP poisoning** exploits ARP's lack of authentication to trick both victim and gateway into routing through the attacker
- **DHCP spoofing** achieves the same result by racing to hand the victim a malicious gateway/DNS at connection time
- The full attack chain — join network → scan → spoof → intercept → capture — is what turns passive listening into active theft
- The next chapter covers the tools (Wireshark, Tcpdump, Ettercap) used to actually execute and analyze these attacks — followed by a hands-on lab

---

[← Previous: Vulnerable Protocols](03-vulnerable-protocols.md) | [Back to README](../README.md) | [Next: Sniffing Tools →](05-sniffing-tools.md)
