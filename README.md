![Vault diagram](/assets/images/selfportrait.jpg)

# Vault Deployment Overview

Vault is a management tool that securely stores certificate keys and passwords, among other functionalities. Due to its strong security features, it was implemented as a credentials manager for a home lab **Proxmox Virtual Environment (PVE)** to enhance security and facilitate user access to the Proxmox environment.

## Operating System Compatibility

For Vault to run within the homelab environment, a compatible operating system must be used. Supported Linux distributions include:

- Ubuntu  
- Debian  

The decision to use a Linux distribution was made due to licensing reasons. Ubuntu was chosen over Debian as it is more widely used, Version 24 was used over 25 as it is a LTS release.

## Deployment Steps

1. **Add Ubuntu 24.04 to Proxmox**
   - Ubuntu 24.04 was added to Proxmox using a script obtained from [Proxmox VE Helper-Scripts]([https://community-scripts.github.io/ProxmoxVE/scripts?id=ubuntu2404-vm).

2. **Network Configuration**
   - Initially, the virtual machine was limited to LAN access, as confirmed by ping tests.
   - After adding a firewall rule, the virtual machine obtained WAN connectivity.

3. **Vault Installation**
   - Vault was installed in the VM using a community download link from the HashiCorp Vault website.

4. **Firewall Configuration**
   - Port `8200` was opened to allow access to Vault (firewall configuration was performed at some point).

5. **Web UI**
   - The Vault web UI was made accessible

## Future Work

- Vault requires further configuration and setup via the web UI, which is accessible on the LAN.

