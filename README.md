<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

# Deploying Active Directory in Azure Virtual Machines  

This tutorial outlines the **implementation of on-premises Active Directory** within **Azure Virtual Machines**. It covers the setup of a Domain Controller, joining a client machine to the domain, configuring remote desktop access, and automating user creation with PowerShell.  

---

## Environments & Technologies Used  
- **Microsoft Azure** – Cloud platform for deploying VMs  
- **Windows Server 2022** – Used as the **Domain Controller (DC-1)**  
- **Windows 10** – Used as the **Client Machine (Client-1)**  
- **Active Directory Domain Services (AD DS)** – Manages users, groups, and authentication  
- **PowerShell** – Automates user creation  
- **Remote Desktop Protocol (RDP)** – Enables remote access  

---

## Operating Systems Used  
- **Windows Server 2022** (Domain Controller)  
- **Windows 10** (Client Machine)  

---

## High-Level Objectives  
1. Set up a Domain Controller (DC-1) and a Client Machine (Client-1) in Azure.  
2. Deploy and configure Active Directory Domain Services (AD DS).  
3. Join the client machine to the domain.  
4. Enable Remote Desktop access for domain users.  
5. Automate user creation using PowerShell.  

---

## Deployment and Configuration Steps  

### **Part 1: Setting Up the Virtual Machines in Azure**  

#### **Step 1: Create the Domain Controller (DC-1)**  
1. Create a **Resource Group** in Azure.  
2. Create a **Virtual Network** and **Subnet**.  
3. Deploy a **Windows Server 2022 VM** named **DC-1**:  
   - **Username:** `labuser`  
   - **Password:** `Cyberlab123!`  
4. After the VM is created, set **DC-1’s NIC Private IP address** to **static**.  
5. Log into **DC-1** and disable the **Windows Firewall** *(for testing connectivity)*.  

![image](https://github.com/user-attachments/assets/ad505344-0015-4e82-ab9e-f68526535c99)

![image](https://github.com/user-attachments/assets/4537d845-352c-474b-8452-f77cad78e4e2)

![image](https://github.com/user-attachments/assets/77d00803-c40b-4ea5-ac29-842284208812)

![image](https://github.com/user-attachments/assets/ac0eff28-9e3e-4a88-ab5f-fc4e68bd6957)

![image](https://github.com/user-attachments/assets/7e689263-03de-435e-ab68-85655f36b1d7)

![image](https://github.com/user-attachments/assets/62ae9f2e-7e5c-4332-9ba1-3925888b56ae)

![image](https://github.com/user-attachments/assets/4780a456-f928-4f5e-9a96-bf4653e9acf2)

![image](https://github.com/user-attachments/assets/b93f556b-e8b1-44ee-acc7-08f306848134)

#### **Step 2: Create the Client Machine (Client-1)**  
1. Deploy a **Windows 10 VM** named **Client-1**:  
   - **Username:** `labuser`  
   - **Password:** `Cyberlab123!`  
2. Attach **Client-1** to the same **Virtual Network** as **DC-1**.  
3. Set **Client-1’s DNS settings** to **DC-1’s Private IP Address**.  
4. Restart **Client-1** from the Azure Portal.  
5. Log into **Client-1** and verify connectivity:  
   - Open **Command Prompt** and run:  
     ```powershell
     ping <DC-1 Private IP>
     ```  
   - Ensure the ping succeeds.  
   - Run `ipconfig /all` in PowerShell to confirm **DC-1’s Private IP is set as the DNS server**.  

![image](https://github.com/user-attachments/assets/43669683-be49-4306-a45a-cb1746b9dde0)

![image](https://github.com/user-attachments/assets/2009d40c-57de-446d-b477-45363ac5531f)

![image](https://github.com/user-attachments/assets/f8b79c17-4215-4499-a61c-950d22e537d9)

![image](https://github.com/user-attachments/assets/b42c1fab-0a98-478f-8ac5-ac5bc4adf2d5)

---

### **Part 2: Installing and Configuring Active Directory**  

#### **Step 1: Install Active Directory on DC-1**  
1. Log into **DC-1** and install **Active Directory Domain Services (AD DS)**.  
2. Promote **DC-1** as a **Domain Controller**, creating a new **forest**:  
   - **Domain Name:** `mydomain.com` *(or any domain name of your choice)*  
3. Restart **DC-1** and log in as:  
mydomain.com\labuser

#### **Step 2: Create Administrative Users in Active Directory**  
1. Open **Active Directory Users and Computers (ADUC)**.  
2. Create an **Organizational Unit (OU)** named **`_EMPLOYEES`**.  
3. Create another **OU** named **`_ADMINS`**.  
4. Create a **new domain user**:  
- **Name:** Jane Doe  
- **Username:** `jane_admin`  
- **Password:** `Cyberlab123!`  
5. Add `jane_admin` to the **Domain Admins** Security Group.  
6. Log out of **DC-1** and log back in as **jane_admin**:  
mydomain.com\jane_admin
7. Use `jane_admin` as the **admin account** for all further steps.  

#### **Step 3: Join Client-1 to the Domain**  
1. Ensure **Client-1’s DNS settings** point to **DC-1’s Private IP**.  
2. Restart **Client-1**.  
3. Log into **Client-1** as the local admin (`labuser`).  
4. Join **Client-1** to the **domain (`mydomain.com`)**.  
5. Restart **Client-1**.  
6. Verify **Client-1 appears in ADUC** on **DC-1**.  
7. Create an **OU** named **`_CLIENTS`** and move **Client-1** into it.  

---

### **Part 3: Configuring Remote Desktop & Automating User Creation**  

#### **Step 1: Enable Remote Desktop for Domain Users**  
1. Log into **Client-1** as **jane_admin**.  
2. Open **System Properties** and select **Remote Desktop**.  
3. Allow **domain users** to access **Remote Desktop**.  
4. Test logging into **Client-1** with a non-administrative domain user.  

#### **Step 2: Automate Bulk User Creation with PowerShell**  
1. Log into **DC-1** as **jane_admin**.  
2. Open **PowerShell_ise** as Administrator.  
3. Create a new script file and paste the contents of this [**script**](https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1).  
4. Run the script and observe **user accounts being generated**.  
5. Open **ADUC** and verify users appear in `_EMPLOYEES`.  
6. Test logging into **Client-1** using one of the newly created accounts.  

---

## **Finishing Up & Best Practices**  
- **Do NOT delete the VMs in Azure**; they will be used for future labs.  
- **To save costs**, turn off the VMs in the **Azure Portal** when not in use.  
- **Explore Group Policy (GPO)** for automating system-wide settings.  
- **Practice this lab multiple times** to build familiarity with Active Directory.  

---

### 🎉 **Congratulations! You’ve successfully deployed Active Directory in Azure!** 🎉
