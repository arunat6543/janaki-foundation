# Janaki Foundation JupyterHub Azure Setup - Complete Documentation

**Date Created**: August 20, 2025  
**Project**: Janaki Foundation JupyterHub Infrastructure  
**Final Status**: ✅ COMPLETE - Production Ready with SSL Certificate  

---

## 📋 Overview

This document provides a complete record of setting up a multi-user JupyterHub environment on Microsoft Azure, from initial Azure CLI installation through final SSL certificate configuration with Let's Encrypt.

---

## 🎯 Final Result

**✅ Production-Ready JupyterHub**
- **URL**: https://135-13-14-211.sslip.io
- **Admin User**: azureuser / JanakiFoundation2025!
- **SSL Certificate**: Let's Encrypt (Trusted, No Browser Warnings)
- **Location**: Azure South India Region
- **VM Type**: Standard_B2ts_v2 (2 vCPUs, 1 GB RAM)
- **OS**: Ubuntu 22.04.5 LTS

---

## 🛠️ Step-by-Step Implementation Log

### Phase 1: Azure CLI Installation and Authentication

#### 1.1 Azure CLI Installation
```powershell
# Attempted MSI installation first (didn't work due to PATH issues)
Invoke-WebRequest -Uri https://aka.ms/installazurecliwindows -OutFile .\AzureCLI.msi
Start-Process msiexec.exe -Wait -ArgumentList '/I AzureCLI.msi /quiet'

# Successfully installed using winget
winget install Microsoft.AzureCLI
```

**Result**: Azure CLI 2.76.0 installed successfully  
**Location**: `C:\Program Files\Microsoft SDKs\Azure\CLI2\wbin\`

#### 1.2 User Authentication
- User manually logged in to Azure CLI using `az login`
- Verified subscription: "Azure subscription 1" (ID: 2f7d2494-8fbb-4d20-b7dc-b1e8a41380ab)
- Confirmed free tier subscription status

---

### Phase 2: Azure Resource Creation

#### 2.1 Initial Attempt (West India Region)
**Failed due to Standard_B1s unavailability**

```bash
# Created initial resource group
az group create --name "JanakiFoundationRG" --location "westindia"

# Created networking components
az network nsg create --resource-group "JanakiFoundationRG" --name "JanakiJupyterHub-nsg"
az network nsg rule create --resource-group "JanakiFoundationRG" --nsg-name "JanakiJupyterHub-nsg" --name "SSH" --protocol tcp --priority 1000 --destination-port-range 22 --access allow
az network nsg rule create --resource-group "JanakiFoundationRG" --nsg-name "JanakiJupyterHub-nsg" --name "HTTP" --protocol tcp --priority 1001 --destination-port-range 80 --access allow
az network nsg rule create --resource-group "JanakiFoundationRG" --nsg-name "JanakiJupyterHub-nsg" --name "HTTPS" --protocol tcp --priority 1002 --destination-port-range 443 --access allow
az network vnet create --resource-group "JanakiFoundationRG" --name "JanakiJupyterHub-vnet" --address-prefix 10.0.0.0/16 --subnet-name "JanakiJupyterHub-subnet" --subnet-prefix 10.0.1.0/24

# VM creation failed - Standard_B1s not available in West India
```

**Issue**: Standard_B1s had subscription restrictions in West India region

#### 2.2 Successful Implementation (South India Region)

```bash
# Created new resource group in South India
az group create --name "JanakiFoundationRG-SouthIndia" --location "southindia"

# Created VM with Standard_B2ts_v2 (successful)
az vm create \
  --resource-group "JanakiFoundationRG-SouthIndia" \
  --name "JanakiJupyterHub" \
  --image "Canonical:0001-com-ubuntu-server-focal:20_04-lts-gen2:latest" \
  --size "Standard_B2ts_v2" \
  --admin-username "azureuser" \
  --admin-password "JanakiFoundation2025!" \
  --nsg-rule SSH \
  --public-ip-address-allocation "static" \
  --location "southindia"

# Opened additional ports
az vm open-port --resource-group "JanakiFoundationRG-SouthIndia" --name "JanakiJupyterHub" --port 80 --priority 1001
az vm open-port --resource-group "JanakiFoundationRG-SouthIndia" --name "JanakiJupyterHub" --port 443 --priority 1002
```

**Result**: VM created successfully with IP 135.13.14.211

#### 2.3 Resource Cleanup
```bash
# Cleaned up unused West India resources
az group delete --name "JanakiFoundationRG" --yes --no-wait
```

**Final Azure Resources**:
- **Resource Group**: JanakiFoundationRG-SouthIndia
- **Virtual Machine**: JanakiJupyterHub (Standard_B2ts_v2)
- **Public IP**: 135.13.14.211 (Static)
- **Network Security Group**: JanakiJupyterHubNSG
- **Virtual Network**: JanakiJupyterHubVNET
- **Storage**: OS Disk (Premium SSD)

---

### Phase 3: JupyterHub Installation

#### 3.1 Initial Attempt (Ubuntu 20.04)
**Failed - TLJH requires Ubuntu 22.04+**

```bash
# Updated system
sudo apt update && sudo apt upgrade -y
sudo apt install python3 python3-dev git curl -y

# TLJH installation failed
curl -L https://tljh.jupyter.org/bootstrap.py | sudo -E python3 - --admin azureuser
# Error: "The Littlest JupyterHub requires Ubuntu 22.04 or higher"
```

#### 3.2 VM Recreation with Ubuntu 22.04

```bash
# Deleted old VM
az vm delete --resource-group "JanakiFoundationRG-SouthIndia" --name "JanakiJupyterHub" --yes

# Created new VM with Ubuntu 22.04
az vm create \
  --resource-group "JanakiFoundationRG-SouthIndia" \
  --name "JanakiJupyterHub" \
  --image "Canonical:0001-com-ubuntu-server-jammy:22_04-lts-gen2:latest" \
  --size "Standard_B2ts_v2" \
  --admin-username "azureuser" \
  --admin-password "JanakiFoundation2025!" \
  --nsg-rule SSH \
  --public-ip-address "JanakiJupyterHubPublicIP" \
  --vnet-name "JanakiJupyterHubVNET" \
  --subnet "JanakiJupyterHubSubnet" \
  --nsg "JanakiJupyterHubNSG" \
  --location "southindia"
```

**Result**: Ubuntu 22.04.5 LTS VM created (same IP: 135.13.14.211)

#### 3.3 Successful TLJH Installation

```bash
# System preparation
sudo apt update && sudo apt upgrade -y
sudo apt install python3 python3-dev git curl -y

# TLJH installation successful
curl -L https://tljh.jupyter.org/bootstrap.py | sudo -E python3 - --admin azureuser
```

**Installation Results**:
- ✅ JupyterHub service active and running
- ✅ Traefik proxy service active and running  
- ✅ Admin user 'azureuser' configured
- ✅ Conda environment (Miniforge) installed
- ✅ Auto-start services enabled

**Service Status Verification**:
```bash
sudo systemctl status jupyterhub --no-pager
sudo systemctl status traefik --no-pager
```

---

### Phase 4: SSL Certificate Configuration

#### 4.1 Initial HTTPS Setup (Self-Signed Certificate)

```bash
# Enabled HTTPS in TLJH
sudo tljh-config set https.enabled true
sudo tljh-config set https.tls.cert /opt/tljh/state/ssl/cert.pem
sudo tljh-config set https.tls.key /opt/tljh/state/ssl/privkey.pem

# Created SSL directory and self-signed certificate
sudo mkdir -p /opt/tljh/state/ssl
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
  -keyout /opt/tljh/state/ssl/privkey.pem \
  -out /opt/tljh/state/ssl/cert.pem \
  -subj '/C=IN/ST=KA/L=Mumbai/O=JanakiFoundation/OU=IT/CN=135.13.14.211'

# Set proper permissions
sudo chown root:root /opt/tljh/state/ssl/*
sudo chmod 600 /opt/tljh/state/ssl/privkey.pem
sudo chmod 644 /opt/tljh/state/ssl/cert.pem

# Reloaded configuration
sudo tljh-config reload proxy
```

**Result**: HTTPS working but with browser certificate warnings

#### 4.2 Let's Encrypt Certificate (Final Solution)

**Domain Setup**:
- Used sslip.io free DNS service
- Domain: `135-13-14-211.sslip.io` automatically resolves to 135.13.14.211

```bash
# Verified domain resolution
nslookup 135-13-14-211.sslip.io
# Result: 135.13.14.211 ✅

# Configured Let's Encrypt
sudo tljh-config set https.letsencrypt.email arun@janakifoundation.org
sudo tljh-config add-item https.letsencrypt.domains 135-13-14-211.sslip.io

# Removed self-signed certificate configuration
sudo tljh-config unset https.tls.cert
sudo tljh-config unset https.tls.key

# Applied Let's Encrypt configuration
sudo tljh-config reload proxy
```

**Certificate Verification**:
```bash
openssl s_client -connect 135-13-14-211.sslip.io:443 -servername 135-13-14-211.sslip.io -showcerts 2>/dev/null | openssl x509 -noout -subject -issuer -dates

# Results:
# subject=CN = 135-13-14-211.sslip.io
# issuer=C = US, O = Let's Encrypt, CN = R10
# notBefore=Aug 20 10:10:30 2025 GMT
# notAfter=Nov 18 10:10:29 2025 GMT
```

**Final SSL Status**: ✅ Trusted Let's Encrypt certificate, no browser warnings

---

## 🔧 Technical Configuration Details

### Network Configuration
```
Public IP: 135.13.14.211 (Static)
Virtual Network: JanakiJupyterHubVNET (10.0.0.0/24)
Subnet: JanakiJupyterHubSubnet (10.0.0.0/24)

Network Security Group Rules:
- SSH (22): Allow from anywhere
- HTTP (80): Allow from anywhere (redirects to HTTPS)
- HTTPS (443): Allow from anywhere
```

### Service Ports
```
Port 22: SSH access
Port 80: HTTP (redirects to HTTPS)
Port 443: HTTPS (JupyterHub via Traefik)
Port 15001: JupyterHub internal API
Port 8099: Traefik admin interface (internal)
```

### TLJH Configuration
```yaml
users:
  admin:
    - azureuser
https:
  enabled: true
  letsencrypt:
    email: arun@janakifoundation.org
    domains:
      - 135-13-14-211.sslip.io
```

### Installed Components
- **JupyterHub**: 5.3.0
- **Traefik**: 3.1.4 (reverse proxy)
- **Miniforge**: 24.7.1-2 (conda package manager)
- **Python**: 3.10.12
- **Ubuntu**: 22.04.5 LTS
- **Let's Encrypt**: R10 certificate authority

---

## 🚀 Access Information

### Primary Access URL
**https://135-13-14-211.sslip.io**

### Administrator Credentials
- **Username**: azureuser
- **Password**: JanakiFoundation2025!

### SSH Access
```bash
ssh azureuser@135.13.14.211
# Password: JanakiFoundation2025!
```

### Azure VM Management
```bash
# VM operations
az vm start --resource-group "JanakiFoundationRG-SouthIndia" --name "JanakiJupyterHub"
az vm stop --resource-group "JanakiFoundationRG-SouthIndia" --name "JanakiJupyterHub"
az vm restart --resource-group "JanakiFoundationRG-SouthIndia" --name "JanakiJupyterHub"

# VM details
az vm show --resource-group "JanakiFoundationRG-SouthIndia" --name "JanakiJupyterHub" --show-details
```

---

## 🔧 Maintenance Commands

### TLJH Management
```bash
# Show configuration
sudo tljh-config show

# Reload services
sudo tljh-config reload proxy
sudo tljh-config reload hub

# Service management
sudo systemctl status jupyterhub
sudo systemctl status traefik
sudo systemctl restart jupyterhub
sudo systemctl restart traefik
```

### User Management
```bash
# Add new admin user
sudo tljh-config add-item users.admin <username>

# Add regular user (JupyterHub web interface preferred)
# Users can be added through the admin panel at:
# https://135-13-14-211.sslip.io/hub/admin
```

### Certificate Management
```bash
# Check certificate details
openssl s_client -connect 135-13-14-211.sslip.io:443 -servername 135-13-14-211.sslip.io -showcerts 2>/dev/null | openssl x509 -noout -dates

# Let's Encrypt certificates auto-renew every 3 months
# Manual renewal (if needed):
sudo tljh-config reload proxy
```

---

## 🐛 Troubleshooting Guide

### Common Issues and Solutions

#### 1. JupyterHub Not Accessible
```bash
# Check service status
sudo systemctl status jupyterhub
sudo systemctl status traefik

# Check listening ports
sudo ss -tlnp | grep ':443\|:80'

# Restart services
sudo systemctl restart traefik
sudo systemctl restart jupyterhub
```

#### 2. SSL Certificate Issues
```bash
# Verify domain resolution
nslookup 135-13-14-211.sslip.io

# Check certificate
openssl s_client -connect 135-13-14-211.sslip.io:443 -servername 135-13-14-211.sslip.io

# Force certificate renewal
sudo tljh-config reload proxy
```

#### 3. Can't Login to JupyterHub
- Verify admin user exists: `sudo tljh-config show`
- Reset password if needed (requires SSH access)
- Check JupyterHub logs: `sudo journalctl -u jupyterhub -f`

#### 4. Network Connectivity Issues
```bash
# Check Azure Network Security Group rules
az network nsg rule list --resource-group "JanakiFoundationRG-SouthIndia" --nsg-name "JanakiJupyterHubNSG" --output table

# Verify VM is running
az vm get-instance-view --resource-group "JanakiFoundationRG-SouthIndia" --name "JanakiJupyterHub" --query instanceView.statuses
```

---

## 💰 Cost Optimization

### Current Resource Costs (Estimated)
- **VM (Standard_B2ts_v2)**: ~$30-40/month
- **Storage (Premium SSD)**: ~$5-10/month
- **Public IP (Static)**: ~$3-5/month
- **Network**: Minimal cost
- **Total**: ~$40-55/month

### Cost Optimization Options
1. **Stop VM when not in use**: `az vm stop` (keeps storage, releases compute)
2. **Use Standard HDD**: Lower storage costs
3. **Monitor usage**: Azure Cost Management
4. **Free tier benefits**: 750 hours/month of B1s (if available)

---

## 📚 References and Resources

### Documentation Links
- [The Littlest JupyterHub](https://tljh.jupyter.org/)
- [Azure CLI Reference](https://docs.microsoft.com/en-us/cli/azure/)
- [Let's Encrypt](https://letsencrypt.org/)
- [sslip.io DNS Service](https://sslip.io/)
- [Traefik Proxy](https://traefik.io/)

### Azure Resource Links
```
Subscription: Azure subscription 1 (2f7d2494-8fbb-4d20-b7dc-b1e8a41380ab)
Resource Group: JanakiFoundationRG-SouthIndia
Region: South India
```

---

## ✅ Implementation Verification Checklist

- [x] Azure CLI installed and authenticated
- [x] Azure subscription verified (free tier)
- [x] VM created in South India region
- [x] Network security groups configured
- [x] Ubuntu 22.04 LTS installed
- [x] TLJH successfully installed
- [x] JupyterHub service running
- [x] Traefik proxy service running
- [x] HTTPS enabled with Let's Encrypt
- [x] SSL certificate trusted (no browser warnings)
- [x] Domain name configured (135-13-14-211.sslip.io)
- [x] Admin user access verified
- [x] Multi-user capability confirmed
- [x] Auto-renewal setup for certificates
- [x] All services auto-start on boot

---

## 🎯 Next Steps / Future Enhancements

### Immediate Tasks
1. **User Onboarding**: Create accounts for Janaki Foundation team
2. **Content Setup**: Install required Python packages for data science
3. **Backup Strategy**: Implement regular backups of user data
4. **Monitoring**: Set up basic monitoring and alerts

### Future Enhancements
1. **Custom Domain**: Purchase janakifoundation.org domain for branded access
2. **Authentication Integration**: LDAP/Active Directory integration
3. **Resource Limits**: Configure user resource quotas
4. **Advanced Monitoring**: Prometheus/Grafana setup
5. **High Availability**: Multiple VM setup with load balancer
6. **Data Storage**: Azure Blob Storage integration
7. **CI/CD**: Automated deployment pipeline

### Security Enhancements
1. **Network Isolation**: VPN-only access option
2. **Multi-Factor Authentication**: Azure AD integration
3. **Audit Logging**: Comprehensive access logging
4. **Data Encryption**: At-rest encryption for user data

---

## 📞 Support Information

### Emergency Contacts
- **Azure Portal**: https://portal.azure.com
- **VM Direct Access**: ssh azureuser@135.13.14.211
- **JupyterHub URL**: https://135-13-14-211.sslip.io

### Key Commands for Quick Recovery
```bash
# Restart all services
sudo systemctl restart traefik jupyterhub

# Check system status
sudo systemctl status traefik jupyterhub

# Full VM restart (from Azure CLI)
az vm restart --resource-group "JanakiFoundationRG-SouthIndia" --name "JanakiJupyterHub"
```

---

**Document Status**: ✅ COMPLETE  
**Last Updated**: August 20, 2025  
**Next Review**: November 2025 (SSL certificate renewal time)  

---

*This document serves as the complete implementation record for the Janaki Foundation JupyterHub infrastructure on Microsoft Azure. All steps have been tested and verified as working in production.*
