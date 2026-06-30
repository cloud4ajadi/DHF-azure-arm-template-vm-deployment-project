# DHF-azure-arm-template-vm-deployment-project
Azure Virtual Machine Deployment Using Arm Template

````md
# 🚀 Azure ARM Virtual Machine Deployment Project

## 📌 Project Overview

This project demonstrates the implementation of **Infrastructure as Code (IaC)** using **Azure Resource Manager (ARM) templates** to deploy a fully functional Linux Virtual Machine (VM) on Microsoft Azure.

Instead of manually creating resources through the Azure Portal, this project automates the entire infrastructure provisioning process using declarative JSON templates and Azure CLI commands.

The deployment includes:
- Virtual Network (VNet)
- Subnet
- Network Interface (NIC)
- Network Security Group (NSG)
- Public IP Address
- Ubuntu Linux Virtual Machine

This project reflects real-world DevOps practices used for scalable, repeatable, and automated cloud infrastructure deployment.

---

## 🎯 Project Objectives

- Understand Infrastructure as Code (IaC) using ARM templates
- Deploy Azure resources using declarative JSON templates
- Learn Azure CLI-based deployment workflow
- Configure networking components for secure VM access
- Apply cloud security principles using NSGs and SSH
- Understand resource dependencies in cloud infrastructure
- Handle deployment errors and troubleshoot Azure limitations

---

## 🏗️ Architecture Overview

The deployed architecture consists of:

1. **Resource Group**
   - Logical container for all resources

2. **Virtual Network (VNet)**
   - Provides isolated network environment

3. **Subnet**
   - Segments the virtual network

4. **Network Interface (NIC)**
   - Connects VM to network

5. **Public IP Address**
   - Enables external SSH access

6. **Network Security Group (NSG)**
   - Controls inbound traffic (SSH port 22)

7. **Virtual Machine (Ubuntu Server)**
   - Compute instance running Linux OS

---

## 🌍 Azure Region Selection

**Selected Region:** `Canada Central (canadacentral)`

### 📌 Reason for Selection:
- Lower latency for local access
- Better availability of free or low cost VM-size e.g. Standard B2pts v2 (2 vcpus, 1 GiB memory), after testing multiple regions
- Improved success rate for VM deployment compared to US/EU regions tested earlier

---

## ⚙️ ARM Template Structure

The ARM template consists of four main components:

### 1. Parameters
Defines user inputs such as:
- VM name
- Admin username
- Admin password
- Location

### 2. Variables
Stores reusable values such as:
- VNet name
- Subnet name
- NIC name
- Public IP name
- NSG name

### 3. Resources
Defines Azure infrastructure:
- Microsoft.Network/virtualNetworks
- Microsoft.Network/networkInterfaces
- Microsoft.Network/publicIPAddresses
- Microsoft.Network/networkSecurityGroups
- Microsoft.Compute/virtualMachines

### 4. Outputs
Returns deployment results such as:
- Public IP address of the VM

---

## 🚀 Deployment Steps (Azure CLI)

### 1. Create Resource Group
```bash
az group create \
    --name arm-project-rg \
    --location canadacentral
````

---

### 2. Validate ARM Template

Ensures JSON syntax and structure are correct before deployment.

```bash
az deployment group validate \
    --resource-group arm-project-rg \
    --template-file azuredeploytemplate.json \
    --parameters @azuredeployparameters.json
```

---

### 3. Deploy Infrastructure

```bash
az deployment group create \
    --resource-group arm-project-rg \
    --template-file azuredeploytemplate.json \
    --parameters @azuredeployparameters.json
```

---

### 4. Retrieve Public IP Address

```bash
az vm list-ip-addresses \
    --resource-group arm-project-rg \
    --name arm-vm \
    --output table
```

---

### 5. SSH into Virtual Machine

```bash
ssh azureuser@<PUBLIC_IP>
```

---

## 🔐 Security Implementation

This project follows Azure security best practices:

### ✔ Network Security Group (NSG)

* Allows inbound SSH traffic on port 22 only
* Blocks all other unnecessary inbound traffic

### ✔ Identity & Access

* Uses admin username/password authentication
* Encourages secure password policy enforcement

### ✔ Shared Responsibility Model

| Layer                   | Responsibility                         |
| ----------------------- | -------------------------------------- |
| Physical Infrastructure | Microsoft Azure                        |
| Network Security        | Microsoft Azure + User (configuration) |
| Operating System        | User                                   |
| Application Security    | User                                   |
| Access Credentials      | User                                   |

---

## 💰 Cost Management Strategy

To avoid unnecessary charges:

* Azure Free Tier subscription was used
* VM size selected within free-tier-compatible limits
* Resources are deleted after testing using:

```bash
az group delete --name arm-project-rg --yes --no-wait
```

---

## 🧪 Deployment Challenges & Troubleshooting

### ❌ Issue 1: VM SKU Not Available

* Several VM sizes (B-series, A-series) were unavailable in selected regions
* **Solution:** Switched to `Standard B2pts v2 (2 vcpus, 1 GiB memory)` and not available in multiple regions

---

### ❌ Issue 2: Public IP SKU Restriction

* Basic SKU Public IP was not allowed in subscription
* **Solution:** Upgraded to Standard SKU Public IP

---

### ❌ Issue 3: SSH Authentication Failure

* Caused by incorrect or mismatched credentials
* **Solution:** Reset VM password via Azure CLI / Portal

---

## 📸 Verification Evidence (Included in Repository)

The following screenshots are included:

* Azure Resource Group creation
* Successful ARM deployment
* Virtual Machine running status
* Public IP address assignment
* Cost Management dashboard
* NSG configuration rules
* SSH connection proof

---

## 📊 Outputs Example

After deployment, the system returns:

* Public IP Address
* Resource IDs
* Deployment correlation ID
* VM provisioning status

---

## 🧠 Key Learnings

* Infrastructure as Code enables repeatable cloud deployments
* Azure ARM templates require strict JSON structure
* Cloud regions have capacity limitations affecting deployments
* Networking configuration is critical for VM accessibility
* Troubleshooting is a core DevOps skill

---

## 🏁 Conclusion

This project successfully demonstrates the deployment of a fully functional cloud-based virtual machine using Azure ARM templates and CLI automation.

It highlights real-world DevOps practices including:

* Infrastructure automation
* Cloud networking configuration
* Security implementation
* Cost optimization
* Deployment troubleshooting

---

## 📁 Repository Structure

```
├── screenshots/
├── templates/
│   ├── azuredeployparameters.json
│   ├── azuredeploytemplate.json
├── README.md
```


## 🚀 Future Improvements / Enhancements

This project successfully demonstrates Infrastructure as Code (IaC) using ARM templates. However, several enhancements can be implemented to further improve security, scalability, automation, and operational efficiency in a real-world DevOps environment.

---

### 🔐 1. Security Enhancements

- Implement SSH key-based authentication instead of password authentication
- Disable password login completely for Linux VM access
- Store sensitive data (e.g., SSH keys, credentials) in Azure Key Vault
- Apply role-based access control (RBAC) to restrict resource permissions

---

### ⚙️ 2. Infrastructure & Deployment Improvements

- Replace ARM templates with **Bicep files** for improved readability and maintainability
- Parameterize more configuration values for better reusability across environments (dev, staging, production)
- Introduce modular templates for networking and compute resources
- Validate templates using automated pre-deployment checks in CI pipelines

---

### 🔄 3. CI/CD Automation

- Integrate GitHub Actions or Azure DevOps pipelines for automated deployment
- Automate validation and deployment of ARM templates
- Enable rollback strategies in case of deployment failure
- Store templates in version control for better change tracking

---

### 📊 4. Monitoring & Observability

- Enable Azure Monitor for real-time VM performance tracking
- Configure Log Analytics Workspace for centralized logging
- Set up alerts for CPU usage, disk utilization, and network activity
- Monitor security events using Microsoft Defender for Cloud

---

### 💰 5. Cost Optimization Improvements

- Implement Azure Budgets with automated alerts
- Use auto-shutdown schedules for virtual machines to reduce costs
- Continuously monitor unused resources and clean up idle infrastructure
- Explore cost-efficient VM SKUs based on workload requirements

---

### 🧠 6. Architectural Improvements

- Deploy resources across multiple availability zones for high availability
- Introduce load balancers for scalable traffic distribution
- Design infrastructure for multi-region disaster recovery
- Use Infrastructure as Code modules for enterprise-scale deployments

---

### 🧪 7. Testing & Validation Improvements

- Add automated template validation before deployment
- Simulate failure scenarios to test system resilience
- Use Azure Policy to enforce compliance rules
- Perform security baseline checks before production deployment

---

## 📌 Summary

These enhancements would transition this project from a basic deployment exercise into a production-ready DevOps infrastructure pipeline, aligned with industry best practices in cloud engineering, automation, and security.

---

## 🏗️ ARM Template Improvement

To improve security and align with best practices, the ARM template should be updated to support secure authentication methods.

---

### 🔐 Current Limitation

The current deployment uses password-based authentication:

```json id="pwlim1"
"adminPassword": "StrongPassword123!@#"
````

This approach is less secure and not recommended for production environments.

---

### ✅ Recommended Improvement: SSH Key Authentication

The ARM template should be updated to use SSH keys instead of passwords.

### Updated configuration example:

```json id="sshfix1"
"osProfile": {
  "computerName": "devopsVM01",
  "adminUsername": "azureuser",
  "linuxConfiguration": {
    "disablePasswordAuthentication": true,
    "ssh": {
      "publicKeys": [
        {
          "path": "/home/azureuser/.ssh/authorized_keys",
          "keyData": "<YOUR_PUBLIC_SSH_KEY>"
        }
      ]
    }
  }
}
```

---

### 🎯 Benefit of this improvement:

- Stronger authentication security  
- Eliminates password-based vulnerabilities  
- Aligns with DevOps and cloud security best practices  
- Suitable for automation and production environments  

---

## 👤 Author

Cloud Computing Learner Project – Azure ARM Deployment Lab By Jamiu Ajadi
