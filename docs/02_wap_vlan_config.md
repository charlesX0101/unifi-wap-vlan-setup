# 02 – WAP VLAN Configuration (Ubiquiti U6+)

This document outlines the configuration of the Ubiquiti UniFi U6+ Wireless Access Point to support multiple VLAN-tagged SSIDs. Each SSID corresponds to a unique VLAN used in the segmented homelab environment. The goal is to enforce logical separation between device types and usage zones via wireless access.

# Physical Network Configuration

- Device: Ubiquiti UniFi U6+ Access Point
- Connection: TP-Link TL-SG2210MP Port 2
- Port Mode: Tagged trunk
- VLANs Passed: 20 (LAB), 30 (HOME), 40 (IOT), 50 (GUEST), 60 (DMZ)

# SSID to VLAN Mapping

- SSID: lab        = VLAN 20
- SSID: home       = VLAN 30
- SSID: iot        = VLAN 40
- SSID: guest      = VLAN 50
- SSID: dmz        = VLAN 60

Each SSID is isolated to its respective VLAN, with DHCP, routing, and ACL policies handled by OPNsense.

# UniFi Controller Configuration Steps

- Navigate to the UniFi Controller at https://10.0.20.102:8443
- Go to WiFi section and create a new wireless network
- For each SSID:
  - Set the SSID name (e.g., lab, home, iot)
  - Enable "Use VLAN" option and enter the corresponding VLAN ID
  - Leave WPA2/WPA3 settings as appropriate
  - Assign network to the corresponding VLAN segment (defined in controller settings)

Repeat for each VLAN-backed SSID. Ensure VLAN IDs match firewall and switch configurations.

# Management Access Restriction

- Management interface for the WAP is limited to VLAN 20 (LAB)
- Only VLAN 20 devices are allowed to access the controller and adopt/manage the AP
- Controller port 8443 is restricted via OPNsense firewall rules to VLAN 20 only

# Verification

- All VLANs assigned via SSID were confirmed to pass tagged traffic through Switch Port 2
- DHCP confirmed functional for each VLAN via wireless connection
- Ping and routing tests validated segment isolation
- Controller remains unreachable from any non-LAB VLAN or external networks

# Outcome

- The Ubiquiti U6+ WAP is now broadcasting five SSIDs mapped to five distinct VLANs
- Wireless clients are segmented by use-case and access level
- Management is isolated and secured through firewall-level access control
- This configuration emulates real-world enterprise wireless segmentation using prosumer gear


