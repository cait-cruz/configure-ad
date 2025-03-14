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

![image](https://github.com/user-attachments/assets/93cc2566-a5b9-416b-9a76-4ee392d204d9)

![image](https://github.com/user-attachments/assets/eb567bee-5910-4a77-a501-d6a09aa2f425)

![image](https://github.com/user-attachments/assets/0f3197d0-2f88-49f1-9ba8-f5ba989fb8a5)

![image](https://github.com/user-attachments/assets/d1a513cf-2986-4071-9a04-32c16e52285f)

![image](https://github.com/user-attachments/assets/b4e3c750-e29f-4871-9172-5ebe76d77e90)

![image](https://github.com/user-attachments/assets/4ce8c5b5-1662-447d-b015-5809e37ddb81)

![image](https://github.com/user-attachments/assets/6d60f02c-5cb3-4a00-8922-cb94bf75df17)

![image](https://github.com/user-attachments/assets/5d481ecb-468a-4da0-aff7-847a3acf46e7)

![image](https://github.com/user-attachments/assets/f820d694-f49e-4915-8f95-bac0d06b6c09)

![image](https://github.com/user-attachments/assets/f9136e63-2b9e-47e3-a361-2d4a636913e2)

![image](https://github.com/user-attachments/assets/d527a961-342b-43d2-b24e-d28f205e8434)

![image](https://github.com/user-attachments/assets/367b8424-27e7-4bf8-bdec-f2dab2f80ad9)

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

![image](https://github.com/user-attachments/assets/fa19312e-add4-457e-af38-17f62710ce27)

![image](https://github.com/user-attachments/assets/ea8638e5-dfdb-4714-beec-6a7ccaf956ce)

![image](https://github.com/user-attachments/assets/04141940-6302-4723-8588-f426ab38b6bf)

![image](https://github.com/user-attachments/assets/78a8413d-b363-4f91-9fdd-7192100359e8)

![image](https://github.com/user-attachments/assets/81a8a448-d3fb-44d7-9346-8c4043c6eaa2)

![image](https://github.com/user-attachments/assets/d6f22109-2645-427f-a1be-2a3e78fba827)

![image](https://github.com/user-attachments/assets/b9f586b9-4689-42f7-82b3-e23f43730855)

![image](https://github.com/user-attachments/assets/7ddaef2f-5e48-4456-914d-9325f2c150c8)

![image](https://github.com/user-attachments/assets/d1db6914-d444-4be3-9a32-3f184c26a207)



---

### **Part 2: Installing and Configuring Active Directory**  

#### **Step 1: Install Active Directory on DC-1**  
1. Log into **DC-1** and install **Active Directory Domain Services (AD DS)**.  
2. Promote **DC-1** as a **Domain Controller**, creating a new **forest**:  
   - **Domain Name:** `mydomain.com` *(or any domain name of your choice)*  
3. Restart **DC-1** and log in as:  
mydomain.com\labuser

![image](https://github.com/user-attachments/assets/c1b4ca60-0bfd-4f5f-91dd-9a5926d20fe6)

![image](https://github.com/user-attachments/assets/c7c891ec-3d88-467a-b381-c98cff8cad31)

![image](https://github.com/user-attachments/assets/4408907c-c955-43bc-9af8-fb29441756e5)

![image](https://github.com/user-attachments/assets/fa99d26e-54b8-49c5-84f8-d3bdd7223278)

![image](https://github.com/user-attachments/assets/8f1b77ab-a812-4f8c-bfd2-881d119abe9a)

![image](https://github.com/user-attachments/assets/86d7edb7-8688-489e-9a32-e17762685bee)

![image](https://github.com/user-attachments/assets/9dc22556-7b43-4ef5-80f5-a70ad6e1690c)



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

![image](https://github.com/user-attachments/assets/2df64a27-b0d9-4d7a-9048-38825f7936b6)

![image](https://github.com/user-attachments/assets/94f1d142-20b3-4837-a7f3-f0548f7469ec)

![image](https://github.com/user-attachments/assets/52ff97eb-2c20-40bf-ac79-664fbcf851b3)

![image](https://github.com/user-attachments/assets/3e0d7638-058d-4abe-aea4-823d9d9db064)

![image](https://github.com/user-attachments/assets/96b7e726-1c1b-421b-8681-9622e40e803a)

![image](https://github.com/user-attachments/assets/e2bb63d0-b6f3-483d-afa2-e66679b775ad)

![image](https://github.com/user-attachments/assets/641f3d7c-d8a6-419f-bda6-dd3103f4adea)

![image](https://github.com/user-attachments/assets/804095ee-c4c4-404a-9ce6-64b51e3cac48)





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
