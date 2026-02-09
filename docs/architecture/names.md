1. Naming Convention Standard
Format:
[location]-[function]-[role]-[n]
Where:
- location = lab (your home lab)
- function = hypervisor, server, client, fw, infra, ws
- role = dc, linux, win, siem, vuln, web, log, ids, kali, misc
- n = instance number (01, 02, etc.)

Network Device
| Device              | IP            | VLAN       | 
|---------------------|---------------|------------|
| GS108Ev3.L2.switch  | 10.10.0.2     | Management |

Hardware Hostnames
| Device                     | Purpose                                           | Hostname                 |
|----------------------------|---------------------------------------------------|---------------------------|
| Desktop (Ryzen 3700X)      | Primary hypervisor + analyst workstation          | lab-hypervisor-desktop   |
| HP ProLiant ML310e Gen8    | Infrastructure hypervisor                         | lab-hypervisor-proliant  |
| Dell OptiPlex 7070 Micro   | pfSense firewall + IDS/IPS                        | lab-fw-7070              |
| Dell Precision 3530        | Mobile analyst workstation                        | lab-ws-3530              |
| ODROID H2                  | DNS, NTP, honeypot, auxiliary services            | lab-infra-odroid         |

Proxmox Node Names
| Node     | Hostname        |
|----------|------------------|
| Desktop  | pve-desktop      |
| ProLiant | pve-proliant     |

VM Naming Scheme (including examples)
| VM Role              | Example Name             | Description                          |
|----------------------|--------------------------|--------------------------------------|
| Domain Controller    | lab-proliant-dc-01       | AD DS, DNS                           |
| Windows Client       | lab-desktop-win-01       | Analyst workstation VM               |
| Linux Server         | lab-proliant-linux-01    | General-purpose Linux server         |
| SIEM                 | lab-desktop-siem-01      | Wazuh/Splunk                         |
| Vulnerability Scan   | lab-desktop-vuln-01      | Nessus/OpenVAS                       |
| Kali                 | lab-desktop-kali-01      | Offensive testing                    |
| Web Server           | lab-proliant-web-01      | DVWA/Juice Shop                      |
| Syslog Server        | lab-proliant-log-01      | Log aggregation                      |
| IDS (virtualised)    | lab-desktop-ids-01       | Suricata/Snort                       |
| Misc Utility         | lab-proliant-misc-01     | Any other service                    |

VLAN Naming Scheme
| VLAN ID | Name          | Purpose                          |
|---------|---------------|----------------------------------|
| 10      | 10-management | Hypervisors, pfSense, admin      |
| 20      | 20-internal   | AD, servers, internal services   |
| 30      | 30-dmz        | Web apps, exposed services       |
| 40      | 40-clients    | Windows clients, Kali            |
| 50      | 50-labwifi    | Optional lab WiFi network        |
| 99      | 99-native     | Native/untagged VLAN             |

IP Addressing Plan
| VLAN       | Subnet         | Purpose                          |
|------------|-----------------|----------------------------------|
| Management | 10.10.0.0/24    | Hypervisors, pfSense, admin      |
| Internal   | 10.20.0.0/24    | AD, Linux servers, SIEM          |
| DMZ        | 10.30.0.0/24    | Web servers, test apps           |
| Clients    | 10.40.0.0/24    | Windows clients, Kali            |
| Lab WiFi   | 10.50.0.0/24    | Optional wireless                |
| Native     | 10.99.0.0/24    | Untagged                         |

Example IP Assignments
| Device             | IP            | VLAN       |
|--------------------|---------------|------------|
| pfSense LAN        | 10.10.0.1     | Management |
| GS108Ev3.L2.switch | 10.10.0.2     | Management |
| ProLiant Host      | 10.10.0.10    | Management |
| Desktop Host       | 10.10.0.20    | Management |
| ODROID H2          | 10.20.0.5     | Internal   |
| Domain Controller  | 10.20.0.10    | Internal   |
| SIEM               | 10.20.0.50    | Internal   |
| Web Server         | 10.30.0.10    | DMZ        |
| Windows Client     | 10.40.0.10    | Clients    |
| Kali               | 10.40.0.20    | Clients    |