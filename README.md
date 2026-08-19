# 🕵️ Sniffing & Man-in-the-Middle Attacks — A Practical Cybersecurity Guide

> **Module 8 companion repo:** Packet Sniffing, MITM Attacks & Countermeasures — with a dedicated chapter on how AI has changed both attack and defense since 2024.

This repository is a structured, chapter-wise knowledge base on **network sniffing and Man-in-the-Middle (MITM) attacks** — built for students, aspiring penetration testers, SOC analysts, and anyone studying for OSCP/CEH/Security+ style certifications. It's written as a companion to a lab-based ethical hacking course, reorganized into clean reference material with real lab commands, real-world case studies, and a forward-looking look at how AI is reshaping this space.

⚠️ **Ethical use only.** Every technique in this repo is for **authorized labs, CTFs, and pentests with written permission**. Sniffing traffic you don't own or don't have consent to test is illegal in most jurisdictions.

---

## 📚 Table of Contents

| # | Chapter | What You'll Learn |
|---|---------|--------------------|
| 01 | [Introduction to Sniffing](chapters/01-introduction-to-sniffing.md) | What sniffing is, promiscuous mode, why it matters, legality |
| 02 | [Types of Sniffing](chapters/02-types-of-sniffing.md) | Passive vs. active sniffing, MAC flooding, where sniffing happens most |
| 03 | [Protocols Vulnerable to Sniffing](chapters/03-vulnerable-protocols.md) | HTTP, FTP, Telnet, POP3/IMAP — risk levels & safe alternatives |
| 04 | [MITM Attacks — ARP & DHCP Spoofing](chapters/04-mitm-arp-dhcp-spoofing.md) | How ARP poisoning and DHCP spoofing hijack traffic, attack flow diagrams |
| 05 | [Sniffing Tools](chapters/05-sniffing-tools.md) | Wireshark, Tcpdump, Ettercap, Bettercap, Tshark, Cain & Abel compared |
| 06 | [SSL Stripping](chapters/06-ssl-stripping.md) | How HTTPS gets downgraded to HTTP mid-session, sslstrip walkthrough |
| 07 | [Detection & Countermeasures](chapters/07-detection-and-countermeasures.md) | Spotting sniffers, defense tactics, encryption as the golden rule |
| 08 | [Sniffing & MITM in the AI Era](chapters/08-ai-era-sniffing-and-mitm.md) | ML-based intrusion detection, AI-assisted attacks, encrypted traffic analysis, XAI in SOCs |

**Labs:**
- [Lab: Ettercap ARP-Poisoning MITM Walkthrough](labs/lab-ettercap-mitm.md) — a full attacker/victim/gateway lab with real commands and output

**Extras:**
- [resources/glossary.md](resources/glossary.md) — quick-reference glossary of every term used
- [resources/cheat-sheet.md](resources/cheat-sheet.md) — one-page command & defense cheat sheet
- [resources/further-reading.md](resources/further-reading.md) — cited sources, papers, and practice rooms

---

## 🎯 Learning Path

```
Start here
   │
   ▼
01 Introduction ──▶ 02 Types of Sniffing ──▶ 03 Vulnerable Protocols
   │                                                  │
   ▼                                                  ▼
Lab: Ettercap MITM ◀── 04 MITM (ARP/DHCP) ◀───────────┘
   │
   ▼
05 Tools (Wireshark/Tcpdump/Ettercap) ──▶ 06 SSL Stripping
   │
   ▼
07 Detection & Countermeasures ──▶ 08 Sniffing in the AI Era
   │
   ▼
resources/cheat-sheet.md  +  practice labs (TryHackMe)
```

## 🧪 Lab Environment Used Throughout

| Role | OS | IP (example) |
|------|----|----|
| Attacker | Kali Linux | `192.168.26.130` |
| Victim | Windows 10 | `192.168.26.134` |
| Gateway/Router | — | `192.168.26.2` |

All commands in this repo assume a **local, isolated VMware/VirtualBox lab network** — never run these against a network you don't own or lack written authorization to test.

## 🧑‍🏫 Who This Is For

- Students working through ethical hacking / penetration testing modules
- SOC analysts who want the attacker's-eye view of what they're defending against
- Anyone prepping for CEH, Security+, OSCP, or similar exams
- Developers who want to understand *why* "always use HTTPS" actually matters

## ⚖️ Legal & Ethical Notice

Sniffing and MITM techniques are **dual-use**: the same tools that let a pentester find a vulnerability let an attacker steal a password. Per most computer-crime laws (e.g., the US Computer Fraud and Abuse Act, India's IT Act Section 43/66, UK Computer Misuse Act), intercepting data you're not authorized to access is a criminal offense — regardless of your intent. Use this material only:

- In your own lab / VM network
- In a CTF or training environment designed for this purpose
- With **explicit, documented, written authorization** (a signed pentest scope)

## 📄 License

Educational content — free to use, adapt, and share for learning purposes. Attribute if you redistribute. No warranty; use at your own risk and legal responsibility.

---

*Built from course notes on "Practical Ethical Hacking – Module 8: Sniffing," expanded with independent research and current (2025–2026) sources on AI-driven network security.*
