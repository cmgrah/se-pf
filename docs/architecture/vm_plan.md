# VM Assignments
## VMs on Unraid (HP ProLiant ML310e)
- Purpose: Stable, always‑on infrastructure + DMZ workloads
- Why: Unraid is ideal for long‑running services, storage‑heavy workloads, and infrastructure VMs.
- Summary: Unraid hosts all infrastructure, DMZ, and persistent services.

| VM Name                 | Role / Purpose                     | VLAN | IP            | Notes                         |
|-------------------------|-------------------------------------|------|---------------|-------------------------------|
| lab-proliant-dc-01      | Domain Controller (AD DS + DNS)     | 20   | 10.20.0.10    | Core identity service         |
| lab-proliant-linux-01   | Linux server (utility)              | 20   | 10.20.0.20    | Syslog, scripts, automation   |
| lab-proliant-log-01     | Syslog / log aggregation            | 20   | 10.20.0.30    | Forwarder target for pfSense  |
| lab-proliant-web-01     | Web server (DVWA/Juice Shop)        | 30   | 10.30.0.10    | DMZ attack surface            |
| lab-proliant-misc-01    | Misc services / test VM             | 20   | 10.20.0.x     | Optional                      |


## VMs on Desktop (Windows + VMware/Hyper‑V)
- Purpose: Analyst workstation, security tooling, heavy compute tasks
- Why: Desktop has strong CPU/RAM and you need to keep the OS intact.
- Summary: Desktop hosts SIEM, scanners, Kali, and client VMs — all the heavy, interactive workloads.

| VM Name                 | Role / Purpose                     | VLAN | IP            | Notes                                 |
|-------------------------|-------------------------------------|------|---------------|---------------------------------------|
| lab-desktop-siem-01     | SIEM (Wazuh or Splunk)             | 20   | 10.20.0.50    | Heavy CPU/RAM usage fits desktop      |
| lab-desktop-vuln-01     | Vulnerability Scanner (Nessus)      | 20   | 10.20.0.60    | Scanning load better on desktop       |
| lab-desktop-kali-01     | Kali Linux                          | 40   | 10.40.0.20    | Offensive testing                     |
| lab-desktop-win-01      | Windows client                      | 40   | 10.40.0.10    | Analyst workstation VM                |
| lab-desktop-ids-01      | Optional Suricata/Snort VM          | 20   | 10.20.0.x     | Only if not using pfSense IDS         |

## Services on ODROID
| Service                 | IP          | VLAN | Notes                          |
|-------------------------|-------------|------|--------------------------------|
| Pi-hole DNS             | 10.20.0.5   | 20   | Internal DNS for lab           |
| NTP Server              | 10.20.0.5   | 20   | Time sync for AD + SIEM        |
| Honeypot (optional)     | 10.20.0.x   | 20   | Cowrie or similar              |


## Consolidated table
| Host Machine            | VM Name                 | Role / Purpose                     | VLAN | IP            |
|-------------------------|--------------------------|-------------------------------------|------|---------------|
| Unraid (ProLiant)       | lab-proliant-dc-01       | Domain Controller                   | 20   | 10.20.0.10    |
|                         | lab-proliant-linux-01    | Linux utility server                | 20   | 10.20.0.20    |
|                         | lab-proliant-log-01      | Syslog server                       | 20   | 10.20.0.30    |
|                         | lab-proliant-web-01      | DMZ web server                      | 30   | 10.30.0.10    |
|                         | lab-proliant-misc-01     | Misc services                       | 20   | 10.20.0.x     |
| Desktop (VMware/Hyper-V)| lab-desktop-siem-01      | SIEM                                | 20   | 10.20.0.50    |
|                         | lab-desktop-vuln-01      | Vulnerability scanner               | 20   | 10.20.0.60    |
|                         | lab-desktop-kali-01      | Kali Linux                          | 40   | 10.40.0.20    |
|                         | lab-desktop-win-01       | Windows client                      | 40   | 10.40.0.10    |
|                         | lab-desktop-ids-01       | Optional IDS VM                     | 20   | 10.20.0.x     |
| ODROID H2               | Pi-hole / NTP / Honeypot | Infra services                      | 20   | 10.20.0.5     |


# Resource Allocation Plan

## Host Capacity

| Host                  | CPU                     | RAM Total | Notes                          |
|-----------------------|-------------------------|-----------|--------------------------------|
| Unraid (ProLiant)     | Xeon E3-1220 (4c/4t)    | 10 GB     | Infra + DMZ, keep some for host|
| Desktop (Windows)     | Ryzen 7 3700X (8c/16t)  | 32 GB     | SIEM, scanners, clients, Kali  |

---

## Unraid (ProLiant) – VM Resources

_Assume ~2 GB reserved for Unraid OS → ~8 GB usable for VMs._

| VM Name               | vCPU | RAM   | Storage (approx) | Notes                          |
|-----------------------|------|-------|------------------|--------------------------------|
| lab-proliant-dc-01    | 2    | 3 GB  | 80–100 GB        | AD DS + DNS                    |
| lab-proliant-linux-01 | 1    | 1.5 GB| 40–60 GB         | Utility / scripts              |
| lab-proliant-log-01   | 1    | 1.5 GB| 100–200 GB       | Logs (depends on retention)    |
| lab-proliant-web-01   | 1    | 1 GB  | 40–60 GB         | DVWA/Juice Shop                |
| lab-proliant-misc-01  | 1    | 1 GB  | 40–60 GB         | Optional / lab tasks           |

**Unraid VM total (target):**  
- vCPU: 6  
- RAM: ~8 GB  

---

## Desktop – VM Resources

_Leave ~8–10 GB for host OS + tools → ~22–24 GB usable for VMs._

| VM Name               | vCPU | RAM    | Storage (approx) | Notes                          |
|-----------------------|------|--------|------------------|--------------------------------|
| lab-desktop-siem-01   | 4    | 10–12 GB| 200–300 GB      | Wazuh/Splunk (heavy)           |
| lab-desktop-vuln-01   | 2    | 4 GB   | 80–120 GB        | Nessus/OpenVAS                 |
| lab-desktop-win-01    | 2    | 4 GB   | 80–120 GB        | Windows client                 |
| lab-desktop-kali-01   | 2    | 4 GB   | 60–100 GB        | Kali                           |
| lab-desktop-ids-01*   | 2    | 2–4 GB | 40–80 GB         | Optional IDS VM                |

\*Only if you’re not relying solely on pfSense IDS.

**Desktop VM total (target, without IDS):**  
- vCPU: 10  
- RAM: ~22–24 GB  

---

## Quick Rules of Thumb

- **Unraid:** keep at least **2 GB** and **1 core** free for the host.  
- **Desktop:** keep at least **8–10 GB** and **2–4 cores** free for the OS + tools.  
- Start lower on RAM for each VM and scale up if you see pressure.