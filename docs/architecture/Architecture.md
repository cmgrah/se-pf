                           Internet
                               |
                        [ Home Router ]
                        10.223.79.132/8
                               |
                     ┌────────────────────┐
                     │   pfSense 7070     │
                     │ lab-fw-7070        │
                     │ Firewall / IDS / VPN│
                     └─────────┬──────────┘
                               |
        ┌───────────────┬───────────────┬────────────────┬────────────────┐
        │               │               │                │                │
 [ Management ]   [ Internal ]       [ DMZ ]        [ Clients ]       [ Home LAN ]
  VLAN 10           VLAN 20          VLAN 30         VLAN 40          (Unsegmented)
  10.10.0.0/24      10.20.0.0/24     10.30.0.0/24    10.40.0.0/24     Your normal
                                                                       household devices
        │               │               │                │
        │               │               │                │
 ┌──────────────┐  ┌────────────────┐  ┌──────────────┐  ┌────────────────────┐
 │ Managed Switch│  │ ODROID H2      │  │ Web Apps     │  │ Windows Client VM   │
 │ 10.10.0.2     │  │ Pi-hole / NTP  │  │ DVWA, etc.   │  │ Kali Linux VM       │
 └──────┬────────┘  │ 10.20.0.5      │  │ 10.30.0.10   │  │ 10.40.0.10 / .20    │
        │            └────────────────┘  └──────────────┘  └────────────────────┘
        │
        │
 ┌──────────────────────────────┐
 │ ProLiant ML310e (Unraid)     │
 │ lab-hypervisor-proliant      |
 | 74:46:A0:FE:C8:B8            │
 │ 10.10.0.10                   │
 │                              │
 │  • lab-proliant-dc-01        │
 │    Domain Controller         │
 │    10.20.0.10                │
 │                              │
 │  • lab-proliant-linux-01     │
 │    Utility Server            │
 │    10.20.0.20                │
 │                              │
 │  • lab-proliant-log-01       │
 │    Syslog Server             │
 │    10.20.0.30                │
 │                              │
 │  • lab-proliant-web-01       │
 │    DMZ Web Server            │
 │    10.30.0.10                │
 └──────────────────────────────┘


 ┌──────────────────────────────────────────────┐
 │ Desktop (Windows OS + VMware/Hyper-V)        │
 │ lab-hypervisor-desktop                       │
 │ 10.10.0.20                                    │
 │                                              │
 │  • lab-desktop-siem-01                       │
 │    SIEM (Wazuh/Splunk)                       │
 │    10.20.0.50                                 │
 │                                              │
 │  • lab-desktop-vuln-01                       │
 │    Vulnerability Scanner (Nessus/OpenVAS)    │
 │    10.20.0.60                                 │
 │                                              │
 │  • lab-desktop-win-01                        │
 │    Windows Client VM                         │
 │    10.40.0.10                                 │
 │                                              │
 │  • lab-desktop-kali-01                       │
 │    Kali Linux VM                             │
 │    10.40.0.20                                 │
 └──────────────────────────────────────────────┘

- **Desktop (Ryzen 3700X, 32 GB RAM, 1+2 TB storage)**  
  Primary hypervisor and analyst workstation for SIEM, scanners, Kali, Windows clients, cloud tooling, and heavy workloads.

- **HP ProLiant ML310e Gen8 (Xeon E3‑1220, 10 GB RAM, 4 TB storage)**  
  Infrastructure server hosting AD, Linux servers, logging nodes, vulnerable apps, and long‑running services.

- **Dell OptiPlex 7070 Micro (i5‑9500T, 16 GB RAM)**  
  Dedicated pfSense firewall with Suricata IDS/IPS, VLAN segmentation, and VPN access.

- **Dell Precision 3530 (i5‑8300H, 16 GB RAM)**  
  Mobile analyst/attack platform for Kali, Wireshark, Burp Suite, scripting, and remote administration.

- **ODROID H2**  
  Auxiliary services node running Pi‑hole DNS, NTP, lightweight honeypots, and monitoring utilities.
