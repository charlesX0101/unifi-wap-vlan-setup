# 01 – UniFi Controller Setup (Docker on Raspberry Pi)

This document outlines the process of hosting the Ubiquiti UniFi Controller (Network Application) on a Raspberry Pi using Docker. This approach ensures portability, separation from physical WAPs, and centralized control of VLAN-aware wireless settings.

# Host System

- Hardware: Raspberry Pi 4 Model B (4GB RAM)
- OS: Ubuntu Server 22.04 LTS (64-bit)
- Static IP: Assigned via DHCP reservation from OPNsense
- Purpose: Run UniFi Controller in a Docker container for persistent and remote WAP configuration

# Step 1 – System Preparation

- Update and install required packages:

```
sudo apt update && sudo apt upgrade -y
sudo apt install docker.io docker-compose -y
sudo systemctl enable docker
sudo usermod -aG docker $USER
```
 - Log out and back in to apply docker group permissions.


#Step 2 – Project Directory and Docker Compose File
- Create a working directory for the controller setup:

```
mkdir -p ~/unifi-controller && cd ~/unifi-controller
```

- Create a docker-compose.yml file:
```
version: '3'

services:
unifi:
image: jacobalberty/unifi
container_name: unifi
restart: unless-stopped
network_mode: host
volumes:
- ./unifi:/unifi
environment:
- TZ=America/Los_Angeles
```
- Note: The host network mode allows the controller to broadcast discovery packets to UniFi devices.

# Step 3 – Start the Container
- Start the container:
```
docker-compose up -d
```
- Confirm it’s running:
```
docker ps
```

- You should see `jacobalberty/unifi` listening on ports such as 8443, 8080, 3478/UDP, and others.

# Step 4 – Access the UniFi Controller

- From any client within the same subnet (LAB VLAN):

https://10.0.20.102:8443


- Complete the UniFi setup wizard:
  - Disable remote access/cloud sync
  - Assign local device name and password
  - Set appropriate timezone and hostname
  - Adopt your UniFi U6+ device

# Step 5 – Secure the Controller

- Restrict access to port 8443 to LAB VLAN only via firewall rules in OPNsense:
  - Allow inbound TCP 8443 from VLAN 20
  - Block all other source networks

# Outcome

- You now have a fully functioning UniFi Controller hosted in a containerized, headless environment.
- This setup allows centralized configuration of all UniFi AP settings.
- The controller will auto-restart with the system and persist data in `~/unifi-controller/unifi`.




