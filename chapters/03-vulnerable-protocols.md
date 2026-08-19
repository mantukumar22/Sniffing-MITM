# Chapter 3 — Protocols Vulnerable to Sniffing

[← Previous: Types of Sniffing](02-types-of-sniffing.md) | [Back to README](../README.md) | [Next: MITM — ARP & DHCP Spoofing →](04-mitm-arp-dhcp-spoofing.md)

---

## 3.1 Why Protocols Matter

Every time you open a website, upload a file, or log in somewhere, you're using a network protocol to do it. Some protocols are secure (like HTTPS). Some are wide open, like a broken lock.

> "Tum samajh rahe ho tum secure ho… par tumhare protocol ne already surrender kar diya." — You think you're safe, but the protocol you're using already gave up the fight before it started.

## 3.2 HTTP — The Unsecured Web

**HTTP = Hypertext Transfer Protocol** (without the "S" for Secure)

- No encryption — all data is sent in plain text
- Passwords, form data, and search queries are fully visible to anyone sniffing the connection
- Many old websites, IoT admin panels, and internal enterprise tools still run HTTP

**Real example:** A 2021 bug bounty hunter sniffed the credentials of an IoT smart-bulb admin panel simply because it used HTTP during initial setup — no exploit needed, just a listener on the network.

## 3.3 FTP — File Transfer Protocol

FTP is used to upload/download files between computers.

- No encryption at all
- Usernames and passwords are sent in clear text, exactly like HTTP
- Can expose full file structures and file contents to attackers

> "FTP ke case mein hacker kuch nahi karta… password khud chal ke uske system mein aa jaata hai!" — With FTP, the attacker doesn't even have to work for it; the password practically walks itself into their system.

## 3.4 Telnet — The Dinosaur Still in Use

Telnet allows remote login to a device — but sends *everything*, including the login itself, in clear text.

- No encryption whatsoever
- Still commonly found on routers, printers, and network switches
- Still enabled by default on some older enterprise devices

**Real story:** A university once left Telnet open on a server. Within hours, students had sniffed the admin login and defaced the homepage to announce that they'd sniffed it — a live demonstration of exactly why Telnet should have been retired decades ago.

## 3.5 POP3 & IMAP — Email Login Danger

These are the classic protocols used to *fetch* mail from a server.

- **POP3** runs on port 110, **IMAP** on port 143 — by default, unencrypted
- If not wrapped in SSL/TLS, they leak your email password in plain text on every login
- Many people configure email clients without realizing whether the connection is actually secured

> **Tip for viewers:** If your email client's settings don't explicitly say "SSL" or use port 993/995 (the secure variants), assume sniffers are already reading your inbox.

## 3.6 The OSI Layer View of Sniffing

Sniffing threats exist at every layer of the OSI model — the higher the layer, the more "human-readable" the leaked data tends to be:

| Layer | Name | What Gets Sniffed |
|---|---|---|
| 7 | Application | User ID / password sniffing |
| 6 | Presentation | SSL/TLS session sniffing |
| 5 | Session | Telnet and FTP session sniffing |
| 4 | Transport | TCP session / UDP sniffing |
| 3 | Network | IP and port sniffing |
| 2 | Data Link | MAC/ARP sniffing |
| 1 | Physical | Surveillance sniffing (physical tap/wiretap) |

This matters because different tools and defenses operate at different layers — a firewall rule at Layer 3/4 won't help if the leak is happening at Layer 7 through plaintext credentials.

## 3.7 Recap: Protocols Ranked by Risk

| Protocol | Risk Level | Safe Alternative |
|---|---|---|
| HTTP | Very High | **HTTPS** |
| FTP | Very High | **SFTP / FTPS** |
| Telnet | Extreme | **SSH** |
| POP3 / IMAP | High | **POP3S / IMAPS** |

> "Secure connection ≠ green padlock only… It means your data isn't being served on a plate to attackers." A padlock icon signals *transport* encryption exists — it doesn't automatically mean your application logic, cookies, or session handling are safe. But without that padlock, you have essentially zero protection against a sniffer.

## 3.8 Chapter Recap

- HTTP, FTP, and Telnet transmit data (including credentials) in **plain text**
- POP3/IMAP leak email logins unless explicitly wrapped in SSL/TLS
- Sniffing threats map onto every layer of the OSI model, from physical wiretaps to application-layer password capture
- The universal fix is the same everywhere: **replace the insecure protocol with its encrypted equivalent** (HTTPS, SSH, SFTP, IMAPS/POP3S)

Even encrypted protocols aren't automatically bulletproof, though — [Chapter 6](06-ssl-stripping.md) covers **SSL Stripping**, the attack that downgrades HTTPS back to HTTP mid-session.

---

[← Previous: Types of Sniffing](02-types-of-sniffing.md) | [Back to README](../README.md) | [Next: MITM — ARP & DHCP Spoofing →](04-mitm-arp-dhcp-spoofing.md)
