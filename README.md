# Azure-Infrastructure-and-Access-Management Lab

## Scenario

A school IT department needs a secure way to manage its cloud systems, control who has access, and keep track of costs. This project creates an Azure environment where IT staff can safely manage a school server, protect sensitive login information, and follow rules for how cloud resources are used. Budget monitoring is also added so the school can avoid unexpected cloud expenses.

## Skills & Tools

| Category | Tools/Skills |
|---|---|
| Cloud Administration | Microsoft Azure, Resource Groups, Virtual Machines |
| Identity & Access | Microsoft Entra ID, Security Groups, Azure RBAC |
| Credential Protection | Azure Key Vault, Secrets |
| Policy & Standards | Azure Policy, Initiative Definitions, Parameters |
| Cost Management | Azure Budgets, Spending Alerts |
| Server Administration | Windows Server 2022, RDP |
| Verification | Access checks, Policy compliance, Resource review |

## Environment

- **Subscription:** Azure for Students
- **Resource Group:** `RG-SchoolIT-Lab`
- **Virtual Machine:** `test-vm`
- **Operating System:** Windows Server 2022 Datacenter: Azure Edition
- **Region:** East US 2
- **Key Vault:** `SchoolIT-KeyVault`
- **Entra Security Group:** `IT-KeyVaultAdmins`
- **Policy Initiative:** `AssetTaggingInitiative`
- **Budget:** `LabBudget`

## Implementation

### 1. Created a Resource Group for the School IT Lab

I created a dedicated resource group called `RG-SchoolIT-Lab` to keep all of the lab resources together in one place. This makes the environment easier to organize, review, and clean up later.

<img src="screenshots/01-resource-group-overview.png" width="900"><br><br>

### 2. Created a Microsoft Entra Security Group

I created a security group called `IT-KeyVaultAdmins` in Microsoft Entra ID. Instead of giving permissions to each person one at a time, the group gives IT staff a central place to manage access.

<img src="screenshots/02-entra-security-group.png" width="900"><br><br>

### 3. Created an Azure Key Vault

I created `SchoolIT-KeyVault` to safely store sensitive information used by the lab. Key Vault keeps passwords and other secrets separate from normal notes or files.

<img src="screenshots/03-key-vault-creation-review.png" width="700"><br><br>

<img src="screenshots/04-key-vault-overview.png" width="900"><br><br>

### 4. Gave the IT Group Access to Key Vault

I assigned the `Key Vault Administrator` role to the `IT-KeyVaultAdmins` group. This allows approved IT staff in that group to manage the vault without giving the same permissions to everyone in the Azure subscription.

<img src="screenshots/05-key-vault-administrator-role-assignment.png" width="900"><br><br>

### 5. Stored Important Secrets in Key Vault

I added a secret for a sample school helpdesk service and another secret for the test virtual machine administrator password. This demonstrates how an IT team can keep important credentials in one protected location instead of writing them down in plaintext.

<img src="screenshots/06-helpdesk-app-service-secret.png" width="900"><br><br>

<img src="screenshots/07-test-vm-admin-password-secret.png" width="900"><br><br>

### 6. Created a Windows Server Virtual Machine

I deployed a Windows Server 2022 virtual machine called `test-vm`. The VM acts as a school IT server that can be used for administrative testing and support tasks.

<img src="screenshots/08-test-vm-overview.png" width="900"><br><br>

### 7. Assigned VM Management Permissions

I assigned the `Virtual Machine Contributor` role at the VM level. This gives the selected IT group or user permission to manage the virtual machine without automatically giving control over every resource in the subscription.

<img src="screenshots/09-vm-contributor-role-assignment.png" width="900"><br><br>

### 8. Verified the Server Was Working

I connected to the Windows Server virtual machine and confirmed that it was running correctly. This verified that the server deployment and access setup were successful.

<img src="screenshots/10-vm-rdp-login-success.png" width="900"><br><br>

### 9. Created an Azure Policy Initiative

I created an initiative called `AssetTaggingInitiative`. An initiative groups several Azure rules together so they can be managed and assigned as one set.

For this lab, the initiative was designed to help the school keep resources organized and limit what types of resources can be created.

<img src="screenshots/11-asset-tagging-initiative-overview.png" width="900"><br><br>

### 10. Created a Reusable Policy Parameter

I created a parameter called `AllowedTagValue`. This allows the same tag setting to be reused across more than one policy instead of entering the same value separately each time.

<img src="screenshots/12-initiative-parameter-definition.png" width="900"><br><br>

### 11. Connected the Policy Settings

I connected the tag policies to the shared initiative parameter and set the allowed resource type to Azure virtual machines. This means the initiative can automatically check whether resources follow the rules I selected.

<img src="screenshots/13-initiative-policy-parameters.png" width="900"><br><br>

### 12. Assigned the Initiative

I assigned the initiative to the Azure subscription and selected the tag value that should be used. Once assigned, Azure can begin checking resources against those rules.

<img src="screenshots/14-asset-tagging-initiative-assignment.png" width="900"><br><br>

### 13. Checked Policy Compliance

I opened the policy section for the test VM to see whether Azure had started evaluating it. This shows how an IT administrator can check whether cloud resources are following the organization's rules.

<img src="screenshots/15-test-vm-policy-compliance.png" width="900"><br><br>

### 14. Created a Monthly Azure Budget

I created a monthly budget called `LabBudget` with a limit of **$20**. The purpose was to keep track of lab spending and make it easier to notice unexpected costs.

<img src="screenshots/16-lab-budget-overview.png" width="900"><br><br>

### 15. Added a Budget Alert

I configured an email alert at **50% of the budget**, or **$10**. This gives the IT team an early warning before the full monthly budget is reached.

<img src="screenshots/17-lab-budget-alert-details.png" width="900"><br><br>

## Decisions & Why

- **Used groups and scoped roles to manage access.** This makes permissions easier to manage and avoids giving users more control than they need.

- **Stored sensitive credentials in Azure Key Vault.** This keeps passwords and service secrets in a protected location instead of normal notes, files, or scripts.

- **Used Azure Policy and budget alerts to keep the environment under control.** Policy helps keep resources organized and consistent, while budget alerts give an early warning if cloud costs start increasing.

## What This Demonstrates

- **Managing Azure users, groups, and permissions:** Microsoft Entra ID and Azure RBAC were used to control who could manage the Key Vault and virtual machine.

- **Protecting and managing cloud resources:** Azure Key Vault was used for sensitive information, while Azure Policy helped enforce rules for resources in the environment.

- **Running and monitoring a school cloud environment:** A Windows Server VM was deployed and tested, and Azure Cost Management was used to track spending and create budget alerts.

## Why This Project Matters

School IT teams often manage many systems with limited staff, so it is important to keep access simple, credentials protected, resources organized, and costs visible. This project shows a practical way to use Azure for those everyday administrative needs while keeping the environment easier to manage as it grows.
