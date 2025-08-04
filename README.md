# UniFi WAP VLAN Setup

This project documents the final phase of a professional homelab build focused on enterprise-grade wireless segmentation using the Ubiquiti UniFi U6+ Wireless Access Point. It showcases the setup, VLAN assignment, isolation validation, and security configuration of a multi-SSID access point in a VLAN-aware network.

The wireless access layer was deployed to support logically segmented VLANs for administrative tools, lab environments, smart home devices, guest access, and a DMZ. The UniFi controller is self-hosted on a Raspberry Pi using Docker for clean and portable management.

This project is part of a broader homelab architecture that includes an OPNsense firewall, managed switch, and a fully segmented VLAN layout. The wireless VLAN configuration shown here is intended to demonstrate production-grade skills in network design, device provisioning, isolation enforcement, and system hardening.

---

## Repository Scope

This repository focuses exclusively on:

- Hosting the UniFi Controller via Docker on Raspberry Pi
- Configuring the U6+ WAP to support multiple VLAN-tagged SSIDs
- Restricting WAP management to a single secure VLAN
- Testing DHCP, routing, and access control per VLAN
- Validating wireless isolation and segmentation

For switch trunking, firewall rules, and wired VLAN configuration, see the corresponding repositories:
- [opnsense-firewall-build](https://github.com/charlesX0101/opnsense-firewall-build)
- [tp-link-switch-setup](https://github.com/charlesX0101/tp-link-switch-setup)

---

## VLAN Assignments

| SSID       | VLAN ID | Purpose                          |
|------------|---------|----------------------------------|
| lab        | 20      | Internal lab work and hacking    |
| home       | 30      | Trusted home devices             |
| iot        | 40      | Smart TVs, cameras, consoles     |
| guest      | 50      | Internet-only guest access       |
| dmz        | 60      | DMZ testing zone                 |

All SSIDs are assigned to tagged VLANs and routed through Switch Port 2 (tagged trunk). DHCP leases are handled by OPNsense per segment.

---

## Physical Port Assignments

**WAP:** 
- Connected to TP-Link TL-SG2210MP **Port 2** 
- Tagged trunk supporting VLANs 20–60

**Switch Notes:** 
- Port 1 reserved for TP-Link GUI access 
- Port 8 acts as trunk uplink to firewall

Full network topology and VLAN plan is available in the main homelab repository.

---

## Documentation

All configuration steps and testing procedures are included in the `docs/` directory:

- `01_controller_setup.md`: Hosting the UniFi controller using Docker on Raspberry Pi 
- `02_wap_vlan_configuration.md`: WAP setup and SSID-to-VLAN assignments 
- `03_initial_testing.md`: DHCP, ping, and access validation for all VLANs 
- `04_network_validation_notes.md`: (Proposed) Lessons learned and post-deployment checklist 

---

## Why This Matters

Enterprise environments rely heavily on wireless segmentation for security, compliance, and traffic management. This project reflects a production-style deployment using consumer-grade equipment configured to meet enterprise principles. It is intended to demonstrate the ability to:

- Design and implement tagged VLAN wireless networks
- Secure WAP management to trusted subnets only
- Validate segmentation through controlled testing
- Build modular, reproducible network components



