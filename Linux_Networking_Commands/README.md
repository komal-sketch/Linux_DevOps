                                                # Linux Networking Commands for DevOps Engineers

A practical, copy‑paste‑friendly README for day‑to‑day diagnostics, incident response, and environment validation. Wherever a command is legacy, a modern alternative is noted.

---

## Table of Contents

* [Quick Start / Cheatsheet](#quick-start--cheatsheet)
* [Install the Tooling (Ubuntu/Debian)](#install-the-tooling-ubuntudebian)
* [Connectivity & Path Diagnostics](#connectivity--path-diagnostics)
* [Name Resolution & DNS](#name-resolution--dns)
* [Interfaces, IPs & Links](#interfaces-ips--links)
* [Ports, Sockets & Service Reachability](#ports-sockets--service-reachability)
* [Routing Tables](#routing-tables)
* [HTTP, APIs & Downloads](#http-apis--downloads)
* [Firewall Basics](#firewall-basics)
* [History & Reproducibility](#history--reproducibility)
* [Safety Notes](#safety-notes)
* [Frequently Used One‑Liners](#frequently-used-one-liners)

---

## Quick Start / Cheatsheet

> ✅ Use **modern `ip` and `ss`** in new scripts; keep `ifconfig`/`netstat` only for legacy hosts.

```bash
# Reachability
ping -c 4 8.8.8.8
traceroute example.com
tracepath example.com
mtr -rwzbc 100 example.com

# DNS
dig example.com +short
nslookup example.com
whois example.com

# Interfaces & IPs
ip addr show
ifconfig
arp -n

# Ports & Sockets
ss -tulpn
nc -vz example.com 80
nmap -v example.com

# Routing
ip route show

# HTTP / APIs
curl -I https://example.com
curl -s https://api/foo | jq
wget https://host/file -O file
```

---

## Install the Tooling (Ubuntu/Debian)

```bash
sudo apt update
sudo apt install -y net-tools mtr dnsutils whois wireless-tools nmap iptables jq
```

---

## Connectivity & Path Diagnostics

### `ping`

**What:** ICMP echo to test reachability and latency.

```bash
ping -c 4 trainwithshubham
ping -c 5 3.33.152.147
```

### `traceroute` & `tracepath`

**What:** Show hop‑by‑hop path to a destination.

```bash
traceroute youtube.com
tracepath youtube.com
```

### `mtr`

**What:** Combines ping + traceroute with live stats.

```bash
mtr youtube.com
mtr -rwzbc 100 mun.ca
```

### `watch`

**What:** Re-run a command at intervals.

```bash
watch -n 5 mtr google.com
```

---

## Name Resolution & DNS

### `nslookup`

```bash
nslookup google.com
```

### `dig`

```bash
dig google.com +short
dig @8.8.8.8 google.com A +noall +answer
```

### `whois`

```bash
whois google.com
```

### `/etc/hosts` & `hostname`

```bash
cat /etc/hosts
hostname
```

---

## Interfaces, IPs & Links

### `ip` & `ifconfig`

```bash
ip address show
ip -br a
ifconfig
```

### `iwconfig`

```bash
iwconfig
```

### `arp`

```bash
arp -n
```

### `ifplugstatus`

```bash
ifplugstatus
```

---

## Ports, Sockets & Service Reachability

### `ss` & `netstat`

```bash
ss -tulpn
netstat -tulpn
```

### `telnet`

```bash
telnet google.com 80
```

### `nc` (netcat)

```bash
nc -vz google.com 80
```

### `nmap`

```bash
nmap -v google.com
nmap -Pn -p 80,443 google.com
```

---

## Routing Tables

```bash
route -n
ip route show
```

---

## HTTP, APIs & Downloads

### `curl`

```bash
curl -X GET https://dummy-json.mock.beeceptor.com/todos
curl -s https://fake-json-api.mock.beeceptor.com/companies | jq
```

### `wget`

```bash
wget https://example.com/file.tar.gz -O file.tar.gz
```

### `jq`

```bash
curl -s https://api/foo | jq '.items[] | {id, name, status}'
```

---

## Firewall Basics

```bash
sudo iptables -L -n -v
sudo iptables -S
```

---

## History & Reproducibility

```bash
history | tail -n 20
```

---

## Safety Notes

* Only run `nmap`, `nc`, and `telnet` where authorized.
* Use caution with `iptables` modifications.
* Be mindful with `watch` and continuous probes.

---

## Frequently Used One‑Liners

```bash
# Find process listening on port 3000
ss -tulpn | grep :3000

# Show default route and interface
ip route get 8.8.8.8

# Reverse DNS lookup
dig -x 3.33.152.147 +short
```

---

## Legacy Commands Mentioned

* `netstate` → `netstat`
* `iptable` → `iptables`
* `ifpludstatus` → `ifplugstatus`

---

![Linux](https://img.shields.io/badge/Linux-Networking-informational) ![DevOps](https://img.shields.io/badge/DevOps-Tooling-blue) ![License](https://img.shields.io/badge/License-MIT-green)
