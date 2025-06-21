# 🚀 Suricata – Cloud Threat Detection on Raspberry Pi 5

In this lab, I deployed **Suricata**, a powerful open-source network threat detection engine, on my **Raspberry Pi 5 running Kali Linux**. My main goal was to detect suspicious traffic—specifically **ICMP ping packets**—by configuring and running Suricata with a custom rule.

I used a lightweight setup to simulate how cloud security tools can monitor packet behavior in a low-resource environment. After configuring Suricata to watch the traffic on `eth0`, I created a rule that would trigger an alert whenever ping (ICMP) packets were detected. I then tested this rule by pinging from and to the Raspberry Pi, and confirmed that Suricata logged the alert to the `fast.log` file.

🔍 This lab showed me how real-time traffic analysis works and gave me a clearer understanding of rule syntax, interface monitoring, and the importance of log verification.

---

## 🧠 Lab Overview

- **Platform Used**: Raspberry Pi 5 (Kali Linux ARM64)
- **Interface Monitored**: `eth0` (connected to VLAN 20 / Attack Lab)
- **Objective**: Detect ICMP echo requests (pings) using a custom Suricata rule
- **Tools**: Suricata 7.0.10, custom `local.rules`, CLI logging via `fast.log`

---

## 🛠️ Step-by-Step Implementation

### 1. 🔧 Installation & Key Fixes

- Encountered issues with Kali repo signatures (`gpg: dearmor failed`).
- Resolved by cleaning APT lists and importing the correct Kali archive key using `sudo`.
- Successfully installed Suricata using `apt install suricata`.

### 2. 🌐 Interface Configuration

- Verified active interface:  
  `ip a` showed `eth0` with IP `192.xxx.xx.xxx`.

- Ran Suricata with:
  ```bash
  sudo suricata -c /etc/suricata/suricata.yaml -i eth0
  ```

### 3. 📜 Custom Rule Creation

- Edited `/etc/suricata/rules/local.rules` with:
  ```
  alert icmp any any -> any any (msg:"ICMP Ping Detected"; sid:1000001; rev:1;)
  ```

- Made sure Suricata was configured to read `local.rules`:
  - Verified in `/etc/suricata/suricata.yaml` under `rule-files`.

### 4. 🧪 Detection Test

- Sent ICMP pings to and from the Pi:
  ```bash
  ping 192.xxx.xx.xxx
  ping google.com
  ```

- Monitored alerts in real-time:
  ```bash
  sudo tail -f /var/log/suricata/fast.log
  ```

- ✅ Confirmed detection:
  ```
  [**] [1:1000001:1] ICMP Ping Detected [**]
  [Classification: (null)] [Priority: 3] {ICMP} 192.xxx.xx.xxx -> 142.xxx.xx.xxx
  ```

---

## 💡 Key Takeaways

- Suricata operates as a passive network intrusion detection engine that logs suspicious traffic in real time.
- Rules are highly customizable—easy to tailor for cloud, IoT, or enterprise environments.
- Log files like `fast.log` allow analysts to track events post-incident or live.

---

## 📝 TL;DR Executive Summary (for Hiring Managers)

> I deployed Suricata on a Raspberry Pi 5 to simulate cloud-based packet detection in a constrained environment. I created a custom rule to detect ICMP traffic and confirmed real-time alerting using log files. This lab strengthened my hands-on skills in configuring open-source detection engines, troubleshooting Linux repos, and writing actionable detection logic for network monitoring.
