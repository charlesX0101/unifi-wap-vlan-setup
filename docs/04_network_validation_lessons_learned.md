# 04 – Network Validation Notes and Lessons Learned

This document captures key observations, adjustments, and lessons learned during the deployment and validation of VLAN-tagged wireless in the segmented homelab network. It provides a field-ready summary of practical challenges and outcomes that reflect real-world implementation experience.

# Validation Objectives

- Ensure each SSID maps cleanly to the correct VLAN
- Confirm end-to-end DHCP, routing, and DNS functionality
- Restrict management access to trusted subnet only (LAB VLAN)
- Detect and eliminate VLAN leakage or lateral movement
- Log and analyze any unexpected traffic or access attempts
- Test reconnection stability across SSIDs

# Tools Used

- UniFi Controller Web GUI (Docker-hosted)
- OPNsense firewall live logs and interface statistics
- Terminal tools: `ping`, `ip a`, `ip route`, `dig`, `traceroute`
- Network devices: Test laptops, phones, and smart devices
- Manual firewall rule testing using allow/deny toggles

# Key Findings

- DHCP leases correctly assigned from OPNsense for all SSIDs
- Static IP reservation for UniFi controller ensured persistent GUI access
- VLAN tagging from WAP confirmed clean via per-SSID testing
- No inter-VLAN communication detected (as intended by policy)
- NAT, routing, and DNS resolved properly for each VLAN with separate behavior
- Controller access attempts from non-LAB VLANs were correctly blocked

# Lessons Learned

- TP-Link switch port configuration must be verified after firmware reset; native port settings may revert to untagged or access mode
- Trunking from switch port 1 was initially impossible due to GUI access being bound to it; solution was to relocate trunk to port 8 and reserve port 1 for local management only
- Frequent cable/NIC swapping during testing highlighted the need for an automation script to handle IP flushing, reassignment, and default route changes; this script is now in development
- WiFi reconnections after roaming between SSIDs showed occasional delays in DHCP, solved by lowering lease time for test duration
- Isolation is only as effective as the firewall rules; early misconfigurations allowed limited ping replies until stateful filtering was properly tuned

# Configuration Notes

- UniFi U6+ passed tagged VLAN traffic cleanly over trunked port (Switch Port 2)
- Each SSID was created with explicit VLAN ID in the controller
- Controller host was bound to static IP in LAB VLAN for consistency
- Firewall rules were constructed as default deny with explicit allow per function
- Inter-VLAN routing was disabled except where specifically required (e.g., LAB to NAS)

# Final Validation Summary

- SSID-to-VLAN mapping works as expected
- Management and control plane is secured
- Isolation holds across wired and wireless segments
- Wireless clients function reliably with DHCP, routing, and DNS
- All behavior is reproducible, logged, and documented

This document closes out the wireless VLAN phase of the segmented homelab build. The next phase involves further hardening, monitoring, and the integration of intrusion detection and traffic inspection tools.

