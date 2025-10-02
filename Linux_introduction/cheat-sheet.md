# Linux for DevSecOps — Comprehensive Cheat Sheet

## Table of Contents

- [0) TL;DR Runbook: Daily Ops Shortlist](#0-tldr-runbook-daily-ops-shortlist)
- [1) Files, Directories, and Permissions](#1-files-directories-and-permissions)
- [2) Users, Groups, and sudo](#2-users-groups-and-sudo)
- [3) Package Management & Integrity](#3-package-management--integrity)
- [4) Processes, Services, and Systemd](#4-processes-services-and-systemd)
- [5) Logs & Auditing](#5-logs--auditing)
- [6) Networking, Firewalls, and TLS](#6-networking-firewalls-and-tls)
- [7) Disks, Filesystems, LVM](#7-disks-filesystems-lvm)
- [8) Performance & Troubleshooting](#8-performance--troubleshooting)
- [9) Text Processing & Shell Ninja](#9-text-processing--shell-ninja-grepfindsedawkxargs)
- [10) SSH, SCP, and rsync](#10-ssh-scp-and-rsync)
- [11) Compression, Archives, and Checksums](#11-compression-archives-and-checksums)
- [12) Cron & systemd Timers](#12-cron--systemd-timers)
- [13) Security Hardening Highlights](#13-security-hardening-highlights)
- [14) SELinux / AppArmor](#14-selinux--apparmor)
- [15) File Integrity (AIDE)](#15-file-integrity-aide)
- [16) Malware & Vulnerability Scanning](#16-malware--vulnerability-scanning-host--container)
- [17) Containers on Linux (Security Essentials)](#17-containers-on-linux-security-essentials)
- [18) Git, Signing, and Tags (Supply Chain)](#18-git-signing-and-tags-supply-chain)
- [19) Incident Response Quick Ops](#19-incident-response-quick-ops)
- [20) Handy One-Liners](#20-handy-one-liners)
- [Interview Questions — with Solutions](#interview-questions--with-solutions)
  - [A) Fundamentals](#a-fundamentals)
  - [B) Networking & Services](#b-networking--services)
  - [C) Security & Hardening](#c-security--hardening)
  - [D) Packaging & Supply Chain](#d-packaging--supply-chain)
  - [E) Performance & Troubleshooting](#e-performance--troubleshooting)
  - [F) Containers & Orchestration](#f-containers--orchestration-linux-view)
  - [G) Observability & IR](#g-observability--ir)
  - [H) Scenario-Based](#h-scenario-based)
  - [I) Practical Exercises (Sample Solutions)](#i-practical-exercises-sample-solutions)
- [Appendix: Quick Install Matrix](#appendix-quick-install-matrix)

---

## 0) TL;DR Runbook: Daily Ops Shortlist

    uptime
    free -h
    df -hT
    top
    journalctl -p err -n 100 --no-pager
    ss -tulpen
    systemctl --failed
    sudo ufw status
    sudo last -n 10
    sudo tail -n 100 /var/log/auth.log
    getenforce
    sudo lynis audit system
    sudo aide --check
    sudo du -xhd1 / | sort -h | tail
    sudo lsof +L1

---

## 1) Files, Directories, and Permissions

    ls -lah --time-style=long-iso
    stat <FILE>
    tree -L 2
    chown <USER>:<GROUP> <PATH>
    chmod 640 <FILE>
    chmod -R o-rwx <DIR>
    umask 027
    getfacl <FILE>
    setfacl -m u:<USER>:r <FILE>
    setfacl -x u:<USER> <FILE>
    sudo find / -xdev -perm -4000 -o -perm -2000 -type f 2>/dev/null
    getcap /usr/bin/ping
    sudo setcap cap_net_raw+ep /usr/local/bin/mybin
    sudo setcap -r /usr/local/bin/mybin

---

## 2) Users, Groups, and sudo

    id <USER>
    getent passwd <USER>
    getent group <GROUP>
    sudo useradd -m -s /bin/bash <USER>
    sudo passwd <USER>
    sudo usermod -aG sudo <USER>
    sudo deluser <USER> --remove-home
    sudo visudo
    # %deploy  ALL=(root) NOPASSWD: /usr/bin/systemctl restart mysvc

---

## 3) Package Management & Integrity

    apt-cache policy <PKG>
    apt show <PKG>
    sudo apt update && sudo apt install -y <PKG>
    apt-mark hold <PKG>
    apt-mark unhold <PKG>
    sha256sum <FILE>
    gpg --keyserver keyserver.ubuntu.com --recv-keys <KEYID>
    gpg --verify <FILE>.asc <FILE>

---

## 4) Processes, Services, and Systemd

    ps -ef | grep <PROC>
    pstree -ap
    top
    systemctl status <UNIT>
    sudo systemctl start|stop|restart <UNIT>
    sudo systemctl enable|disable <UNIT>
    sudo systemctl list-unit-files --state=enabled
    journalctl -u <UNIT> -f
    systemd-analyze blame
    systemd-cgls

Create a service:

    sudo tee /etc/systemd/system/mysvc.service <<'EOF'
    [Unit]
    Description=My App
    After=network-online.target
    Wants=network-online.target
    [Service]
    User=app
    Group=app
    ExecStart=/usr/local/bin/myapp --config /etc/myapp.yaml
    Restart=on-failure
    CapabilityBoundingSet=CAP_NET_BIND_SERVICE
    AmbientCapabilities=CAP_NET_BIND_SERVICE
    NoNewPrivileges=true
    [Install]
    WantedBy=multi-user.target
    EOF
    sudo systemctl daemon-reload
    sudo systemctl enable --now mysvc

---

## 5) Logs & Auditing

    journalctl -xe
    journalctl -p warning -S today
    journalctl -u ssh -g "Failed password"
    sudo tail -f /var/log/auth.log /var/log/syslog
    sudo apt install -y auditd audispd-plugins
    sudo auditctl -l
    sudo auditctl -w /etc/sudoers -p wa -k sudoers_change
    sudo ausearch -k sudoers_change
    sudo aureport -au -i

---

## 6) Networking, Firewalls, and TLS

    hostname -I
    ip a
    ip r
    ss -tulpen
    dig A example.com +short
    traceroute example.com
    curl -I https://example.com
    nc -zv <HOST> <PORT>

ufw:

    sudo ufw enable
    sudo ufw allow 22/tcp
    sudo ufw allow from <CIDR> to any port 443 proto tcp
    sudo ufw status verbose

iptables / nft:

    sudo iptables -S
    sudo nft list ruleset

TLS:

    openssl s_client -connect <HOST>:443 -servername <HOST> -showcerts
    openssl x509 -in cert.pem -noout -text

---

## 7) Disks, Filesystems, LVM

    lsblk -f
    df -hT
    sudo fdisk -l
    sudo mount /dev/<DEV> /mnt
    # /etc/fstab for persistence
    sudo pvdisplay
    sudo vgdisplay
    sudo lvdisplay
    sudo lvextend -r -L +10G /dev/<VG>/<LV>
    sudo du -xhd1 /var | sort -h | tail
    sudo lsof +L1

---

## 8) Performance & Troubleshooting

    vmstat 1 5
    iostat -xz 1
    mpstat -P ALL 1 5
    pidstat -d -r -u -p <PID> 1
    sar -n DEV 1 10
    cat /proc/pressure/{cpu,io,memory}
    sudo perf top

---

## 9) Text Processing & Shell Ninja (grep/find/sed/awk/xargs)

    find /var/log -type f -name "*.log" -mtime +7 -print
    sudo find / -xdev -type f -size +100M -print
    grep -Rni --color=auto "error" /var/log -C2
    sed -i.bak 's/PermitRootLogin yes/PermitRootLogin no/' /etc/ssh/sshd_config
    ss -tulpn | awk 'NR>1 {print $5 "\t" $1}'
    printf "%s\0" *.log | xargs -0 -n1 -P4 gzip -9

---

## 10) SSH, SCP, and rsync

    ssh -i ~/.ssh/id_ed25519 <USER>@<HOST>
    scp -i ~/.ssh/id_ed25519 file.txt <USER>@<HOST>:/tmp/
    rsync -avzP --delete ./dist/ <USER>@<HOST>:/var/www/dist/
    chmod 700 ~/.ssh
    chmod 600 ~/.ssh/id_ed25519
    ssh-copy-id <USER>@<HOST>

---

## 11) Compression, Archives, and Checksums

    tar czf logs.tar.gz /var/log/myapp/
    tar xzf logs.tar.gz -C /tmp
    gzip -9 <FILE> ; gunzip <FILE>.gz
    zstd -19 <FILE> ; unzstd <FILE>.zst
    md5sum <FILE>
    sha256sum <FILE>

---

## 12) Cron & systemd Timers

    crontab -e
    crontab -l
    sudo ls /etc/cron.{hourly,daily,weekly,monthly}
    sudo systemctl list-timers

---

## 13) Security Hardening Highlights

- SSH key-only; disable root/password logins
- Default-deny firewall; least privilege
- Auto security updates where allowed
- Review setuid/setgid; prefer capabilities
- SELinux/AppArmor enforcing
- auditd + centralized logs
- AIDE baseline & checks
- Secrets via Vault/Secrets Manager
- Containers: drop caps, read-only FS, seccomp, rootless

sysctl example:

    sudo tee /etc/sysctl.d/99-hardening.conf <<'EOF'
    net.ipv4.conf.all.rp_filter = 1
    net.ipv4.conf.default.rp_filter = 1
    net.ipv4.icmp_echo_ignore_broadcasts = 1
    net.ipv4.tcp_syncookies = 1
    kernel.kptr_restrict = 2
    kernel.dmesg_restrict = 1
    fs.protected_hardlinks = 1
    fs.protected_symlinks = 1
    EOF
    sudo sysctl --system

---

## 14) SELinux / AppArmor

SELinux:

    getenforce
    sudo setenforce 1
    sudo ausearch -m avc -ts recent | audit2why
    sudo ausearch -m avc -ts recent | audit2allow -M myallow
    sudo semodule -i myallow.pp

AppArmor:

    sudo aa-status
    sudo aa-enforce /etc/apparmor.d/usr.sbin.nginx
    sudo aa-complain /etc/apparmor.d/usr.sbin.nginx
    sudo journalctl -k | grep DENIED

---

## 15) File Integrity (AIDE)

    sudo apt install -y aide
    sudo aideinit
    sudo mv /var/lib/aide/aide.db.new /var/lib/aide/aide.db
    sudo aide --check

---

## 16) Malware & Vulnerability Scanning (Host & Container)

    sudo apt install -y clamav
    sudo freshclam
    sudo clamscan -r / --bell -i
    sudo apt install -y lynis
    sudo lynis audit system
    trivy image nginx:alpine
    trivy fs .
    trivy config .

---

## 17) Containers on Linux (Security Essentials)

    cat /proc/self/cgroup
    lsns
    docker ps -a
    docker images
    docker run --rm -it --read-only --cap-drop all alpine:3.20 sh
    docker exec -it <CID> sh
    docker logs -f <CID>
    docker inspect <CID_OR_IMG> | jq .
    docker run --read-only --cap-drop all --pids-limit 200 --memory 256m --cpus 0.5 --security-opt no-new-privileges -d myimg:tag
    podman run --rm -it --userns=keep-id alpine id

---

## 18) Git, Signing, and Tags (Supply Chain)

    git config --global user.name "DevSecOps"
    git config --global user.email you@example.com
    git config --global commit.gpgsign true
    gpg --full-generate-key
    gpg --list-secret-keys --keyid-format=long
    git config --global user.signingkey <KEYID>
    git tag -a v1.2.3 -m "Release v1.2.3"
    git push origin v1.2.3

---

## 19) Incident Response Quick Ops

    who
    w
    last -n 20
    sudo tail -n 200 /var/log/auth.log
    sudo journalctl -S "1 hour ago" -p err
    sudo lsof -i
    sudo ss -tulpen
    sudo iptables -S
    sudo nft list ruleset
    sudo ps auxf --sort=-%mem | head
    sudo top -H -p <PID>
    sudo tcpdump -i <IFACE> -nn -w capture.pcap
    sudo strace -p <PID> -f -tt -s 128

---

## 20) Handy One-Liners

    journalctl -u nginx --since today | awk '/client:/ {print $NF}' | sort | uniq -c | sort -nr | head
    sudo find / -xdev -type f -perm -0002 -print
    sudo ss -tulpen | grep :<PORT>
    (cd /path && python3 -m http.server 8080)

---

# Interview Questions — with Solutions

## A) Fundamentals

**1) Explain Linux file permissions and how ACLs differ from standard modes.**  
**Solution:** Standard modes (rwx for user/group/other) are coarse. ACLs add per-user/per-group rules independent of the file’s owning group, enabling fine-grained access. Use `getfacl/setfacl`. Prefer ACLs when multiple teams need distinct rights without changing primary ownership.

**2) Difference between a process, thread, and cgroup?**  
**Solution:** A process is an execution context with its own memory space; threads share a process’s memory. cgroups are kernel mechanisms to *limit/account/isolate* resources used by processes/threads (CPU, memory, I/O, PIDs).

**3) How does systemd differ from init? Targets and units?**  
**Solution:** systemd parallelizes startup, tracks dependencies, and manages services via *units* (service, socket, timer, target, mount, etc.). *Targets* group units (e.g., `multi-user.target`) similar to runlevels.

**4) High-level Linux boot process.**  
**Solution:** Firmware → bootloader → kernel → initramfs → PID 1 (`systemd`) → units (services/mounts/targets) → login shell.

**5) Role of `/proc` and `/sys`?**  
**Solution:** `/proc` exposes *process* and *kernel* info (virtual FS), useful for metrics/tuning. `/sys` (sysfs) exposes *device/driver/kernel* objects for attributes and hotplug; used by udev and tuning tools.

---

## B) Networking & Services

**1) How to check what’s listening on a port?**  
**Solution:** `ss -tulpen` (modern) or `lsof -i :<PORT>`. If `netstat` missing, use `ss`.

**2) Differences among iptables, nftables, and ufw.**  
**Solution:** nftables is the modern kernel framework replacing iptables; ufw is a high-level front-end (Ubuntu) generating iptables/nftables rules. Prefer nftables for new setups; ufw for simplicity.

**3) “Service reachable locally but not externally.” Steps?**  
**Solution:** Verify service binds to correct interface (0.0.0.0), check host firewall (ufw/nft), cloud SG/NACL, routing, and reverse proxy/VIP. Use `ss -tulpen`, `curl -v`, `tcpdump` to confirm traffic path.

**4) Validate HTTPS cert chain via CLI.**  
**Solution:** `openssl s_client -connect host:443 -servername host -showcerts` then inspect `openssl x509 -noout -text`. Confirm CN/SAN, validity, chain completeness, and trust.

---

## C) Security & Hardening

**1) Compare SELinux and AppArmor.**  
**Solution:** Both are MAC systems. SELinux uses labels and contexts with more granular policy; AppArmor is path-based and simpler. Enforce when policies are settled; use permissive/complain to learn.

**2) Linux capabilities vs setuid.**  
**Solution:** Capabilities split root privileges into discrete bits (e.g., `CAP_NET_BIND_SERVICE`), allowing least-privilege binaries instead of full root via setuid.

**3) Baseline and detect drift.**  
**Solution:** Establish golden images + AIDE for file integrity; auditd rules on sensitive files; config management (Ansible/Chef) idempotence; central logs and alerts for deviations.

**4) Secure SSH on public hosts.**  
**Solution:** Key-only auth, `PermitRootLogin no`, `PasswordAuthentication no`, fail2ban, change default port only as deterrence, MFA where feasible, strict firewall, `AllowUsers` list.

**5) Useful sysctl tunings and why.**  
**Solution:** rp_filter (anti-spoof), tcp_syncookies (SYN flood), kptr/dmesg_restrict (leak reduction), protected_{hardlinks,symlinks} (link attacks). Apply via `/etc/sysctl.d`.

---

## D) Packaging & Supply Chain

**1) Verify authenticity of binary/source.**  
**Solution:** Compare `sha256sum` with vendor checksum and verify GPG signature against trusted public keys. Only install from signed repos.

**2) Git tags vs Docker image tags for traceability.**  
**Solution:** Git tags mark immutable source snapshots; Docker tags tie built artifacts to versions/SHAs. Use both to ensure reproducible deploys and rollbacks.

**3) Enforce signed commits/images in pipeline.**  
**Solution:** Server-side Git hooks or platform rules (e.g., “require signed commits”); verify signatures in CI. For containers, use cosign/policy-controller (admission) to require signed images.

---

## E) Performance & Troubleshooting

**1) System is slow: first 5 commands and why.**  
**Solution:** `uptime` (load), `top` (CPU/mem hot spots), `vmstat 1 5` (runq, swap), `iostat -xz 1` (disk latency), `ss -s` or `sar -n` (network). Then dive deeper (`pidstat`, `ps`, `journalctl`).

**2) Explain vmstat/iostat/pidstat/sar.**  
**Solution:** `vmstat` = CPU/memory/swap/runq; `iostat` = block I/O latency/throughput; `pidstat` = per-process CPU/mem/I/O; `sar` = historical system activity by kind.

**3) High I/O localization.**  
**Solution:** `iostat -xz 1`, `pidstat -d 1`, `lsof +D /path`, `iotop`/`bpftrace` to identify files/processes and patterns.

**4) Detect memory pressure/swapping.**  
**Solution:** `vmstat` (si/so), `/proc/meminfo` (Swap*, MemAvailable), `sar -B`, PSI `/proc/pressure/memory`. Tune overcommit, add swap, fix leaks.

---

## F) Containers & Orchestration (Linux View)

**1) Linux namespaces and cgroups in containers.**  
**Solution:** Namespaces isolate views (PID, mount, net, user, uts, ipc), cgroups control/measure resources. Together they form the foundation of container isolation.

**2) Runtime flags to improve container security.**  
**Solution:** `--read-only`, `--cap-drop all` (+add back minimal caps), `--security-opt no-new-privileges`, resource limits, non-root user, seccomp profile, read-only mounts.

**3) Read-only root FS but allow writes.**  
**Solution:** Use `--read-only` plus tmpfs/volume mounts for necessary write paths (`/tmp`, app data dir). Configure app to write only to mounted paths.

**4) Approach to container vuln scanning.**  
**Solution:** Shift-left scans in CI (Trivy/Grype), SBOM generation, fail on criticals, base image pinning, automated rebuilds on CVE updates, admission policies in cluster.

---

## G) Observability & IR

**1) Aggregate logs on Linux hosts.**  
**Solution:** journald → forward via rsyslog/journal-remote/Vector/Fluent Bit to ELK/Loki/CloudWatch. Standardize log format; include trace IDs.

**2) First checks in auth logs for suspected compromise.**  
**Solution:** `auth.log` failed logins, new user creation, sudo escalations, key additions; correlate with IP geolocation/time anomalies.

**3) Capture pcap safely and analyze offline.**  
**Solution:** Narrow scope: `tcpdump -i <IFACE> host <IP> and port <PORT> -w file.pcap`. Limit duration and rotate files. Analyze in Wireshark/Moloch offline.

**4) Prove attacker deleted logs but process holds file handles.**  
**Solution:** `lsof +L1` to show deleted-but-open files; the space remains used until process restart. This is common for log-wiping attempts.

---

## H) Scenario-Based

**1) Service fails only after reboot. What to check?**  
**Solution:** `systemctl status`, `journalctl -u <svc> -b`, `systemd-analyze blame`, missing dependencies in unit file (`After=`, `Wants=`), secrets/volumes not ready at boot, environment files.

**2) SSH keys stop working post-patch.**  
**Solution:** Check `/etc/ssh/sshd_config` (`PubkeyAuthentication yes`), permissions of `~/.ssh` (700) and `authorized_keys` (600), SELinux/AppArmor denials, `sshd` logs, key format (ED25519 vs legacy).

**3) CPU spikes at 2AM daily.**  
**Solution:** Correlate with `systemctl list-timers`, `crontab -l`, `/etc/cron.*`; enable accounting (`sa`), check scripts/backup jobs, examine `pidstat` history if collected.

**4) Docker host out of disk — likely culprits & fixes.**  
**Solution:** Orphaned images/containers (`docker system df`, `prune`), logs in `/var/lib/docker/containers/*-json.log`, big layers, volumes. Rotate logs, use `max-size`/`max-file`, prune regularly.

**5) New kernel causes network instability — safe rollback.**  
**Solution:** Keep previous kernel, select via bootloader or `grub-reboot`. If remote, ensure OOB mgmt. Pin kernel, analyze regressions, test in staging.

---

## I) Practical Exercises (Sample Solutions)

**1) 10 largest files under `/var` (single filesystem).**  
**Solution:**

    sudo du -xah /var | sort -h | tail -n 10

**2) Pipeline-safe string replace with backup.**  
**Solution:**

    sed -i.bak 's/old_value/new_value/g' /etc/myapp.conf

**3) systemd unit with minimal privileges + CAP_NET_BIND_SERVICE.**  
**Solution:** (see service template in section 4; ensure `User=app`, `NoNewPrivileges=true`, `CapabilityBoundingSet=CAP_NET_BIND_SERVICE`)

**4) Read-only container, no extra caps, with healthcheck.**  
**Solution:**

    docker run --read-only --cap-drop all --health-cmd='curl -f http://localhost:8080/health || exit 1' -p 8080:8080 -d myimg:tag

---

## Appendix: Quick Install Matrix

    sudo apt update && sudo apt install -y \
      curl wget gnupg lsb-release ca-certificates \
      git jq dnsutils iproute2 net-tools traceroute \
      tcpdump nmap ncat iptables nftables ufw \
      auditd apparmor-utils aide \
      clamav lynis \
      lvm2 sysstat dstat iotop iftop bpfcc-tools \
      zstd zip unzip p7zip-full \
      rsync openssl python3 python3-pip

Trivy:

    curl -fsSL https://aquasecurity.github.io/trivy-repo/deb/public.key | sudo gpg --dearmor -o /usr/share/keyrings/trivy.gpg
    echo "deb [signed-by=/usr/share/keyrings/trivy.gpg] https://aquasecurity.github.io/trivy-repo/deb stable main" | sudo tee /etc/apt/sources.list.d/trivy.list
    sudo apt update && sudo apt install -y trivy

---
