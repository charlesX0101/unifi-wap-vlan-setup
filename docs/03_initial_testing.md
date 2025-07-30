# 03 – Initial Testing and Validation

This document outlines the testing procedures used to verify the correct operation of the UniFi U6+ WAP VLAN configuration. The focus was to confirm DHCP functionality, routing behavior, isolation between VLANs, and enforcement of access control policies for both wireless and management traffic.

# Test Environment

- Test Device: Laptop with wireless adapter supporting 802.1q-tagged SSIDs
- Switch Port 2: Tagged trunk to WAP (TP-Link TL-SG2210MP)
- Controller Host: Raspberry Pi (LAB VLAN, static DHCP reservation)
- Firewall: OPNsense (VLAN interface configuration and DHCP services)
- DHCP Scope: Unique IP ranges per VLAN

# Test Procedure

- Connect to each SSID sequentially (lab, home, iot, guest, honeypot)
- Confirm successful IP address assignment from correct subnet
- Ping local gateway and external addresses to validate routing
- Attempt cross-VLAN communication (e.g., ping device on another VLAN)
- Attempt access to UniFi Controller GUI from non-LAB SSID
- Monitor firewall logs in OPNsense for rule hits and unexpected traffic
- Disconnect and reconnect to validate lease renewals and stability

# Expected Results Per SSID

- SSID: lab (VLAN 20)
  - Receives IP in 10.0.20.0/24 range
  - Full access to controller and internal lab services
  - No access to HOME, IOT, GUEST, or DMZ VLANs

- SSID: home (VLAN 30)
  - Receives IP in 10.0.30.0/24 range
  - Full internet access, no access to internal lab or controller
  - Access to shared storage and printers allowed by rule

- SSID: iot (VLAN 40)
  - Receives IP in 10.0.40.0/24 range
  - Access to internet only; strict egress rules applied
  - No lateral movement or access to any other VLAN

- SSID: guest (VLAN 50)
  - Receives IP in 10.0.50.0/24 range
  - Internet-only access enforced with default deny rules
  - Blocked from accessing LAN, LAB, HOME, or controller

- SSID: honeypot (VLAN 60)
  - Receives IP in 10.0.60.0/24 range
  - Routes only to isolated DMZ interface
  - No outbound or cross-VLAN traffic permitted

# Sample Test Commands

- Check IP assignment:

```
ip a
```

- Ping default gateway: 
```
ping -c 3 10.0.10.1
```

- Test DNS resolution:
````
dig google.com

ping google.com
```

- Attempt controller access (expected to fail except from LAB):
```
curl -k https://10.0.20.102:8443
```


# Observations

- All SSIDs received proper IP assignments
- No VLANs leaked traffic or responded to pings from unauthorized segments
- Controller access was strictly limited to LAB VLAN as intended
- Firewall logs confirmed rule matches and drops where expected
- Network isolation was successful across wireless segments

# Outcome

- Wireless VLAN segmentation was validated using end-to-end DHCP and routing tests
- Firewall policies enforced proper isolation per SSID
- Management plane access was locked down to trusted administrative subnet
- The UniFi WAP is now fully integrated and operating securely within the segmented network



























