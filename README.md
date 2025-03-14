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

![image](https://github.com/user-attachments/assets/93cc2566-a5b9-416b-9a76-4ee392d204d9)

2. Create a **Virtual Network** and **Subnet**.  

![image](https://github.com/user-attachments/assets/eb567bee-5910-4a77-a501-d6a09aa2f425)

3. Deploy a **Windows Server 2022 VM** named **DC-1**:  
   - **Username:** `labuser`  
   - **Password:** `Cyberlab123!`  

![image](https://github.com/user-attachments/assets/0f3197d0-2f88-49f1-9ba8-f5ba989fb8a5)

![image](https://github.com/user-attachments/assets/d1a513cf-2986-4071-9a04-32c16e52285f)

![image](https://github.com/user-attachments/assets/b4e3c750-e29f-4871-9172-5ebe76d77e90)

![image](https://github.com/user-attachments/assets/4ce8c5b5-1662-447d-b015-5809e37ddb81)

4. After the VM is created, set **DC-1’s NIC Private IP address** to **static**.  

![image](https://github.com/user-attachments/assets/3f4336c9-6653-4d8b-8267-2c9db46cade5)

![image](https://github.com/user-attachments/assets/6b8fd86e-c8e1-4c61-b0ed-48ae315b21bb)

![image](https://github.com/user-attachments/assets/67912e7a-8f4b-419a-b341-1f259206fd56)


5. Log into **DC-1** and disable the **Windows Firewall** *(for testing connectivity)*.  

![image](https://github.com/user-attachments/assets/8be6a1ed-ae3b-45ee-968f-f559665e980b)

![image](https://github.com/user-attachments/assets/7c50e3a0-d53b-4f48-b67f-65ccdb75bd37)

![image](https://github.com/user-attachments/assets/39139ad8-d1a9-43ea-8adb-3e9a96c42adf)





#### **Step 2: Create the Client Machine (Client-1)**  
1. Deploy a **Windows 10 VM** named **Client-1**:  
   - **Username:** `labuser`  
   - **Password:** `Cyberlab123!`  
2. Attach **Client-1** to the same **Virtual Network** as **DC-1**.  

![image](https://github.com/user-attachments/assets/fa19312e-add4-457e-af38-17f62710ce27)

![image](https://github.com/user-attachments/assets/ea8638e5-dfdb-4714-beec-6a7ccaf956ce)

![image](https://github.com/user-attachments/assets/04141940-6302-4723-8588-f426ab38b6bf)

![image](https://github.com/user-attachments/assets/78a8413d-b363-4f91-9fdd-7192100359e8)

3. Set **Client-1’s DNS settings** to **DC-1’s Private IP Address**.  

![image](https://github.com/user-attachments/assets/ceabc60e-3327-4668-a4bf-748b7e9b1289)

![image](https://github.com/user-attachments/assets/c42aca92-878f-4ba2-9490-eedd787afae5)


4. Restart **Client-1** from the Azure Portal.  

![image](https://github.com/user-attachments/assets/5b9463d1-f5c7-4fc7-a3c3-8516cdcc397f)



5. Log into **Client-1** and verify connectivity:  
   - Open **Command Prompt** and run:  
     ```powershell
     ping <DC-1 Private IP>
     ```  
   - Ensure the ping succeeds.  
   - Run `ipconfig /all` in PowerShell to confirm **DC-1’s Private IP is set as the DNS server**.  

![image](https://github.com/user-attachments/assets/7ddaef2f-5e48-4456-914d-9325f2c150c8)

![image](https://github.com/user-attachments/assets/d1db6914-d444-4be3-9a32-3f184c26a207)



---

### **Part 2: Installing and Configuring Active Directory**  

#### **Step 1: Install Active Directory on DC-1**  
1. Log into **DC-1** and install **Active Directory Domain Services (AD DS)**.  

![image](https://github.com/user-attachments/assets/91edfdfa-f84a-47f3-b7cb-5d510479aba6)

![image](https://github.com/user-attachments/assets/c9ae8085-bfcb-4422-8073-53f09ce5e35d)

2. Promote **DC-1** as a **Domain Controller**, creating a new **forest**:  
   - **Domain Name:** `mydomain.com` *(or any domain name of your choice)*  

![image](https://github.com/user-attachments/assets/a13a3005-383d-4923-a4a6-c87acafa2ece)

![image](https://github.com/user-attachments/assets/b8c2630a-9e75-42b0-9114-0d7c6077070b)

![image](https://github.com/user-attachments/assets/76446469-586b-4144-a2ad-5e746e582d67)


3. Restart **DC-1** and log in as:  
mydomain.com\labuser

![image](https://github.com/user-attachments/assets/9dc22556-7b43-4ef5-80f5-a70ad6e1690c)



#### **Step 2: Create Administrative Users in Active Directory**  
1. Open **Active Directory Users and Computers (ADUC)**. 

![image](https://github.com/user-attachments/assets/c91060b2-976c-48b4-b58c-5abca30b1f4d)


2. Create an **Organizational Unit (OU)** named **`_EMPLOYEES`**.  
3. Create another **OU** named **`_ADMINS`**.

   
![image](https://github.com/user-attachments/assets/94f1d142-20b3-4837-a7f3-f0548f7469ec)

![image](https://github.com/user-attachments/assets/15d53e49-3f62-45e8-adc8-31c2f0493a77)


4. Create a **new domain user**:  
- **Name:** Jane Doe  
- **Username:** `jane_admin`  
- **Password:** `Cyberlab123!`  

![image](https://github.com/user-attachments/assets/c701f592-fa64-4b1b-9a39-427fee87589c)

![image](https://github.com/user-attachments/assets/96b7e726-1c1b-421b-8681-9622e40e803a)

5. Add `jane_admin` to the **Domain Admins** Security Group.  

![image](https://github.com/user-attachments/assets/39820469-7043-4186-b389-bbf961fe1ef0)

![image](https://github.com/user-attachments/assets/b04606a9-c45c-45a1-b9b7-ff5b25ebf042)

![image](https://github.com/user-attachments/assets/bc111418-8487-45f0-9a8b-6438693b37f0)


6. Log out of **DC-1** and log back in as **jane_admin**:  
mydomain.com\jane_admin
7. Use `jane_admin` as the **admin account** for all further steps.  






#### **Step 3: Join Client-1 to the Domain**  
1. Ensure **Client-1’s DNS settings** point to **DC-1’s Private IP**.  
2. Restart **Client-1**.  
3. Log into **Client-1** as the local admin (`labuser`).  
4. Join **Client-1** to the **domain (`mydomain.com`)**.  

![image](https://github.com/user-attachments/assets/a54d23f1-458a-4bd6-af3c-80258527f00b)

![image](https://github.com/user-attachments/assets/05af1a24-b23f-45e7-ae3a-1ccd8c6ac2a5)

![image](https://github.com/user-attachments/assets/10a56f0f-e1c1-4ad7-87b4-8e52e2e50bf0)

![image](https://github.com/user-attachments/assets/57ed1da0-fb1d-46a0-ac72-d9e0d64bcac9)

![image](https://github.com/user-attachments/assets/b9e2200c-b6d0-4759-9dde-edc978817025)

5. Restart **Client-1**.  
6. Verify **Client-1 appears in ADUC** on **DC-1**.  

![image](https://github.com/user-attachments/assets/e8060f46-ff9d-4452-a623-2bfd370aa6a4)

7. Create an **OU** named **`_CLIENTS`** and move **Client-1** into it.  


---

### **Part 3: Configuring Remote Desktop & Automating User Creation**  

#### **Step 1: Enable Remote Desktop for Domain Users**  
1. Log into **Client-1** as **jane_admin**.  
2. Open **System Properties** and select **Remote Desktop**.  

![image](https://github.com/user-attachments/assets/fb691c42-c0d6-4687-ad96-460e2c1a10e4)

![image](https://github.com/user-attachments/assets/ddddcc7a-99d2-484b-91fb-9c1b6f9242c9)

3. Allow **domain users** to access **Remote Desktop**.  

![image](https://github.com/user-attachments/assets/62f9153f-fce4-4e42-b6b5-0d40c445a91f)

![image](https://github.com/user-attachments/assets/cb043d71-eb87-4b7e-8abc-e115ebb5b27d)

![image](https://github.com/user-attachments/assets/9c6c593c-4b7b-4f17-8283-ad6f20027283)

4. Test logging into **Client-1** with a non-administrative domain user.  



#### **Step 2: Automate Bulk User Creation with PowerShell**  
1. Log into **DC-1** as **jane_admin**.  
2. Open **PowerShell_ise** as Administrator.  
3. Create a new script file and paste the contents of this [**script**](https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1).  

![image](https://github.com/user-attachments/assets/880d516f-09ec-4774-a575-29fa44842cf5)

![image](https://github.com/user-attachments/assets/18ec9979-86ec-4e94-9b56-e71853cfd227)

4. Run the script and observe **user accounts being generated**.  

![image](https://github.com/user-attachments/assets/c9030b50-9c7c-499f-a6cf-d2172b776514)


5. Open **ADUC** and verify users appear in `_EMPLOYEES`.  

![image](https://github.com/user-attachments/assets/f6f6715e-6f5f-47e3-a786-708a58aaa66e)



6. Test logging into **Client-1** using one of the newly created accounts.  


---

## **Finishing Up & Best Practices**  
- **Do NOT delete the VMs in Azure**; they will be used for future labs.  
- **To save costs**, turn off the VMs in the **Azure Portal** when not in use.  
- **Explore Group Policy (GPO)** for automating system-wide settings.  
- **Practice this lab multiple times** to build familiarity with Active Directory.  

---

### 🎉 **Congratulations! You’ve successfully deployed Active Directory in Azure!** 🎉
