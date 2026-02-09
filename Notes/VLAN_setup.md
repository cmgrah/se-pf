# Safe LAN Setup Steps for pfSense on a Single NIC with VLANs
## Overview
This guide explains how to safely migrate your pfSense LAN to a VLAN when running pfSense on a single NIC (e.g., Dell 7070), while ensuring you never lose access during the process.

1. Keep the Existing LAN Interface Active
Before making any changes:

Leave the pfSense LAN assigned to the physical interface (e.g., igb0).
Keep your management PC connected to a LAN port on the switch.
Verify you can access pfSense on its current LAN IP.

This ensures you have a stable connection before moving LAN to a VLAN.

2. Prepare VLANs on the Switch (Netgear GS108E)

Log in to the GS108E switch.
Create VLAN 10 (WAN) and 20 (LAN).
Tag the port going to pfSense for both VLANs.
For end‑device ports, assign:

Untagged VLAN 20, PVID 20


Save the configuration.

Do not change pfSense LAN yet.

3. Configure VLANs on OpenWRT Router

Create VLAN 10 for WAN.
Assign the WAN interface to ethX.10.
Create VLAN 20 and tag the port going to the switch.
Do not assign an IP interface to VLAN 20 — this is only a passthrough to pfSense.


4. Add VLAN Interfaces in pfSense (But Don’t Switch LAN Yet)
In pfSense:

Go to Interfaces → Assignments → VLANs.
Add:

igb0.10 for WAN
igb0.20 for LAN


Assign both as OPT interfaces temporarily.

Do not switch LAN to VLAN 20 yet.

5. Test VLAN Connectivity Safely

Edit OPT interface for VLAN 20.
Assign a temporary IP, e.g., 192.168.99.1/24.
Enable DHCP for this OPT interface.
On the switch, set one port to Untagged VLAN 20.
Connect your PC to that port.
Verify:

Your PC receives a 192.168.99.x address
You can reach pfSense at http://192.168.99.1



If this works, the VLAN path is confirmed.

6. Safely Migrate LAN to VLAN 20
Once VLAN 20 is confirmed working:

Go to pfSense Interface Assignments.
Change LAN from igb0 to igb0.20.
Set LAN’s final IP (e.g., 192.168.1.1).
Reconfigure DHCP on the new LAN.
Apply.

Your PC should now obtain an address in 192.168.1.x on VLAN 20.

7. Clean Up

Remove temporary OPT interface.
Remove temporary subnet and DHCP server.
Ensure switch ports are properly configured.


8. Safety & Recovery Tips

pfSense gives a 90‑second rollback timer — if you lose access, it auto‑reverts.
Keep a backup of your current pfSense config.
Have a laptop ready with a static IP matching the original LAN.


Summary

Prepare VLANs everywhere before switching pfSense LAN.
Test VLAN 20 using a temporary interface.
Switch LAN only after successful testing.
Confirm connectivity through VLAN‑tagged paths.

This ensures a zero‑risk LAN migration.