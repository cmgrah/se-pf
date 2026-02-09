                Internet
                    |
             [Home Router]
                    |
             ┌───────────────┐
             │ pfSense 7070   │  ← Firewall, IDS, VPN
             └──────┬────────┘
                    │
        ┌───────────┼────────────┬──────────────┐
        │            │            │              │
   [Management]  [Internal]    [DMZ]        [Home LAN]
        │            │            │              │
   ProLiant ML310   Desktop     Web apps     Your normal
   (Infra VMs)      (SIEM,      test VMs     home devices
                    scanners,
                    Kali, etc)

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
