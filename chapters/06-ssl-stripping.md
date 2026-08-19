# Chapter 6 — SSL Stripping

[← Previous: Sniffing Tools](05-sniffing-tools.md) | [Back to README](../README.md) | [Next: Detection & Countermeasures →](07-detection-and-countermeasures.md)

---

## 6.1 What Is SSL Stripping?

**SSL Stripping** is a technique attackers use to downgrade a secure HTTPS connection to plain HTTP, making it trivial to sniff credentials that were *supposed* to be encrypted.

**What it does:**
1. Intercepts the victim's secure request
2. Forwards it to the victim as HTTP instead of HTTPS
3. The victim never realizes their session isn't actually encrypted
4. The attacker captures everything — username, password, cookies — in plain text

> "You clicked on 'https://login.com'… but the hacker showed you 'http://login.com'. Tumhe laga secure ho… hacker ne toh chhota 's' nikaal diya." — You thought you were secure, but the attacker simply stripped out the "s."

## 6.2 How It Actually Works, Step by Step

1. Victim connects to public Wi-Fi
2. Attacker performs ARP poisoning (MITM) — see [Chapter 4](04-mitm-arp-dhcp-spoofing.md)
3. Victim visits a website (e.g., `facebook.com`)
4. Under normal conditions, the site would redirect the browser from `http://` → `https://`
5. **The attacker intercepts this redirect** before it reaches the browser
6. Victim ends up viewing the site's content over **plain HTTP** instead
7. Any credentials submitted go to the attacker in plain text

**Tool used historically:** `sslstrip`, created by security researcher Moxie Marlinspike — the tool that first demonstrated this class of attack publicly and popularized the technique.

## 6.3 Real-World Example

A penetration tester used SSL Stripping during an authorized red-team assessment at a café. In 30 minutes, they captured Facebook logins from 5 different people — all because those users typed in their credentials *before* noticing the HTTPS padlock icon hadn't appeared.

**Moral:** If a site doesn't strictly enforce HTTPS, users can be trapped this way. This is precisely why sites today implement **HSTS (HTTP Strict Transport Security)** — a response header that tells the browser "never load me over plain HTTP again, even if someone tries to downgrade the connection."

## 6.4 Tools Used for SSL Stripping

| Tool | Use |
|---|---|
| **sslstrip** | The original SSL-stripping tool (Python) |
| **Bettercap** | Modern MITM tool with a built-in `sslstrip`/HTTPS-proxy module |
| **Ettercap** | Supports filter-based HTTP injection |
| **mitmproxy** | Can inspect and modify HTTPS requests *if* the victim accepts a fake certificate |

Most modern browsers now actively warn users when they're on plain HTTP or when a certificate looks suspicious — but attackers still rely on humans ignoring small details, like a missing padlock icon or a certificate warning they click through without reading.

## 6.5 How to Stay Safe (Viewer's Perspective)

**Tips to avoid being a victim:**

- ✅ Always check for `https://` and the padlock icon before entering anything sensitive
- ✅ Avoid logging in over public Wi-Fi without a VPN
- ✅ Use browser protections like HTTPS-Everywhere-style enforcement (or a privacy-hardened browser)
- ✅ Never enter credentials unless the site is fully secured (no certificate warnings, valid padlock)
- ✅ Turn off auto-connect to open/unknown Wi-Fi networks

> "Jitni dikkat tum photo mein filter lagane mein karte ho… utni kar lo URL padhne mein, toh hacker fail ho jayega." — Put as much effort into checking the URL bar as you put into picking an Instagram filter, and most SSL-stripping attempts will fail.

## 6.6 Chapter Recap

- SSL Stripping downgrades HTTPS to HTTP *in the middle of the connection*, silently, after a successful ARP-poisoning MITM
- It works because HTTP→HTTPS redirects historically had no cryptographic guarantee — the attacker can simply intercept and rewrite them
- **HSTS** is the modern, server-side fix that closes this gap for compliant sites
- On the user side, vigilance about the padlock icon and avoiding sensitive logins on open Wi-Fi remain the best defenses

**Next up:** how do you actually *detect* that any of this — sniffing, ARP poisoning, or SSL stripping — is happening on your network, and what concrete steps stop it?

---

[← Previous: Sniffing Tools](05-sniffing-tools.md) | [Back to README](../README.md) | [Next: Detection & Countermeasures →](07-detection-and-countermeasures.md)
