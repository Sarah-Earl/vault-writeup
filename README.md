# Vault Deployment Overview

Vault is a management tool that securely stores certificate keys and passwords, among other functionalities. Due to its strong security features, it was implemented as a credentials manager for a home lab **Proxmox Virtual Environment (PVE)** to enhance security and facilitate user access to the Proxmox environment.

## Operating System Compatibility

For Vault to run within the homelab environment, a compatible operating system must be used. Supported Linux distributions include:

- Ubuntu  
- Debian  

The decision to use a Linux distribution was made due to licensing reasons. Ubuntu was chosen over Debian as it is more widely used, Version 24 was used over 25 as it is a LTS release.

## Deployment Steps

1. **Add Ubuntu 24.04 to Proxmox**
   - Ubuntu 24.04 was added to Proxmox using a script obtained from [Proxmox VE Helper-Scripts](https://community-scripts.github.io/ProxmoxVE/scripts?id=ubuntu2404-vm).

2. **Network Configuration**
   - Initially, the virtual machine was limited to LAN access, as confirmed by ping tests.
   - After adding a firewall rule to allow WAN connectivity, the virtual machine was able to update its packages and download the Vault package.
   - The rule was created very openendedly to facilitate future projects on the InternalManagement network.
  
![Vault diagram](/assets/images/firewallrule1.png)

3. **Vault Installation**
   Vault was installed in the VM through the following steps:

   - Add HashiCorp Repo
```
sudo apt update
sudo apt install -y gnupg software-properties-common wget
```
```
wget -O- https://apt.releases.hashicorp.com/gpg | \
gpg --dearmor | \
sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg
```
```
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] \
https://apt.releases.hashicorp.com $(lsb_release -cs) main" | \
sudo tee /etc/apt/sources.list.d/hashicorp.list
```
   - Install Vault
```
sudo apt update
sudo apt install vault
```
```
vault --version
```
   - Configured Vault (Basic File Backend)
```
sudo nano /etc/vault.d/vault.hcl
```
```
storage "file" {
  path = "/opt/vault/data"
}

listener "tcp" {
  address     = "0.0.0.0:8200"
  tls_disable = 1
}

ui = true
```
```
sudo mkdir -p /opt/vault/data
sudo chown -R vault:vault /opt/vault
```
   - Start and Enable Vault Service
```
sudo systemctl enable vault
sudo systemctl start vault
```
```
sudo systemctl status vault
```
   - Initialise Vault
```
export VAULT_ADDR=http://127.0.0.1:8200
```
```
vault operator init
```
6. **Web UI**
   - The Vault web UI was made accessible.
  
![Vault diagram](/assets/images/vaultwebui.png)

## Future Work

   - Currently accessing vault over HTTP, need to create and install a certificate to enable HTTPS, unable to do this at the moment as there is no certificate authority server.
   - Configure LDAPS to allow users from the domain to sign in to Vault, unable to do this as there is currently no domain controller.
   - Configure RBAC to control access of credentials to users.

