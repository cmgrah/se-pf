# Home Lab Build Checklist

## 1. Planning & Baseline
- [x] Consolidate naming scheme, VLANs, IP ranges, device roles into a single reference file.
- [ ] Update BIOS/firmware on:
  - [ ] Desktop
  - [ ] ProLiant ML310e
  - [ ] OptiPlex 7070
  - [ ] ODROID H2
- [ ] Enable virtualization (VT-x/AMD-V, VT-d/IOMMU) on desktop and ProLiant.

---

## 2. Physical Network Layout
- [ ] Connect pfSense (OptiPlex 7070) ethernet (trunk) → home router.
    - [ ] Port number
- [ ] Connect router (trunk) → lab switch.
    - [ ] Port number
- [ ] Connect Desktop → lab switch.
    - [ ] Port number
- [ ] Connect ProLiant → lab switch.
    - [ ] Port number
- [ ] Connect ODROID → lab switch.
    - [ ] Port number
- [ ] Label cables and switch ports.

---

## 3. Install & Configure pfSense (OptiPlex 7070)
- [ ] Install pfSense from USB.
- [ ] Assign WAN and LAN interfaces.
- [ ] Set LAN IP to `10.10.0.1/24`.
- [ ] Create admin account and disable default admin.
- [ ] Create VLANs:
  - [ ] VLAN 10 – Management
  - [ ] VLAN 20 – Internal
  - [ ] VLAN 30 – DMZ
  - [ ] VLAN 40 – Clients
  - [ ] VLAN 50 – Lab WiFi (optional)
- [ ] Configure DHCP scopes (optional).
- [ ] Add initial firewall rules:
  - [ ] Allow VLANs → WAN.
  - [ ] Allow inter-VLAN traffic temporarily for setup.

---

## 4. Install Hypervisors
- [ ] Install Proxmox on ProLiant:
  - [ ] Hostname: `pve-proliant`
  - [ ] IP: `10.10.0.10`
- [ ] Install Proxmox on Desktop:
  - [ ] Hostname: `pve-desktop`
  - [ ] IP: `10.10.0.20`
- [ ] Configure switch ports as VLAN trunks for hypervisors.

---

## 5. Build Core Infrastructure VMs (ProLiant)
- [ ] Create `lab-proliant-dc-01` (Domain Controller):
  - [ ] Windows Server installed
  - [ ] IP: `10.20.0.10`
  - [ ] Promote to AD DS + DNS
  - [ ] Create `lab.local` domain
- [ ] Create `lab-proliant-linux-01`:
  - [ ] Ubuntu/Rocky installed
  - [ ] IP: `10.20.0.20`
- [ ] (Optional) Integrate pfSense with AD for LDAP/RADIUS.

---

## 6. Configure ODROID H2 Services
- [ ] Install Pi-hole:
  - [ ] IP: `10.20.0.5`
  - [ ] Set as DNS for Internal/Clients VLANs
- [ ] (Optional) Install:
  - [ ] NTP server
  - [ ] Honeypot (Cowrie)
  - [ ] Monitoring agent

---

## 7. Deploy Security Tooling (Desktop Hypervisor)
- [ ] Create `lab-desktop-siem-01`:
  - [ ] Install Wazuh or Splunk
  - [ ] IP: `10.20.0.50`
- [ ] Create `lab-desktop-vuln-01`:
  - [ ] Install Nessus/OpenVAS
  - [ ] IP: `10.20.0.60`
- [ ] Create `lab-desktop-kali-01`:
  - [ ] Kali installed
  - [ ] IP: `10.40.0.20`
- [ ] Create `lab-desktop-win-01`:
  - [ ] Windows 10/11 installed
  - [ ] IP: `10.40.0.10`
  - [ ] Join to `lab.local`

---

## 8. DMZ & Web Targets
- [ ] Create `lab-proliant-web-01`:
  - [ ] Linux web server installed
  - [ ] IP: `10.30.0.10`
  - [ ] Deploy DVWA/Juice Shop
- [ ] Update firewall rules:
  - [ ] Allow Internal/Clients → DMZ (HTTP/HTTPS)
  - [ ] Restrict DMZ → Internal (DNS only, if needed)

---

## 9. Logging & Monitoring Integration
- [ ] Configure syslog server (`lab-proliant-log-01` or reuse linux-01).
- [ ] Forward logs:
  - [ ] pfSense → SIEM
  - [ ] Windows (Sysmon + Winlogbeat/agent) → SIEM
  - [ ] Linux → SIEM
- [ ] Build dashboards:
  - [ ] Authentication failures
  - [ ] Firewall blocks
  - [ ] IDS alerts
  - [ ] System health

---

## 10. IDS/IPS Configuration
- [ ] Enable Suricata on pfSense.
- [ ] Monitor DMZ and Internal VLANs.
- [ ] Apply balanced ruleset.
- [ ] Generate benign traffic for testing.
- [ ] Tune noisy rules.

---

## 11. Hardening & Baselines
- [ ] Harden Linux servers:
  - [ ] SSH hardening
  - [ ] Firewall rules
  - [ ] auditd
  - [ ] Logging
- [ ] Harden Windows:
  - [ ] Apply GPO baselines
  - [ ] Deploy Sysmon
- [ ] Document baselines in GitHub.

---

## 12. Vulnerability Assessment & Automation
- [ ] Run scans from `lab-desktop-vuln-01`.
- [ ] Export and triage results.
- [ ] Build automation script:
  - [ ] Log parser
  - [ ] IAM checker
  - [ ] Firewall rule analyzer
- [ ] Document tool in GitHub.

---

## 13. Lockdown & Realism Pass
- [ ] Tighten firewall rules (least privilege).
- [ ] Disable unnecessary services.
- [ ] Create snapshots of stable VM states.
- [ ] Configure Proxmox backups.

---

## 14. Documentation & Portfolio
- [ ] Update GitHub repo with:
  - [ ] Diagrams
  - [ ] Sanitised configs
  - [ ] READMEs
  - [ ] Case study
- [ ] Write ASD-style case study.
- [ ] Capture screenshots of:
  - [ ] SIEM dashboards
  - [ ] pfSense
  - [ ] Suricata
  - [ ] AD
  - [ ] Vulnerability scans