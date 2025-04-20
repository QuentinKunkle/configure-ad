<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Set up a Domain Controller in Azure
- Setup Client-1 in Azure
- Install Active Directory
- Create a Domain Admin user within the domain
- Join Client-1 to your domain (mydomain.com)
- Set up Remote Desktop for non-administrative users on Client-1
- Create a bunch of additional users and attempt to log into client-1 with one of the users
- Dealing with Account Lockouts
- Enabling and Disabling Accounts
- Observing Logs

<h2>Set up a Domain Controller in Azure</h2>

<p>
<img src="https://github.com/user-attachments/assets/81577a79-dcac-484d-8cc9-f1a199cf35ae" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
First, we will need to log into Azure and create a Resource Group named AD-Lab. 
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/530a1e5d-8494-44ba-85e4-b73867d9c68a" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next, we will need to create a virtual network and name it AD-lab-network. Assign it to the resource group AD-lab and put it in the same region you chose for your resource group in step one. 
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/892993ca-2997-4e8a-9120-d61dca40a20b" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Now that we have our virtual network set up, we can create our Domain Controller Virtual Machine on a Windows Server 2022 and name it DC-1. You will assign this to the AD-Lab resource group and on the AD-lab-network. 
  
- Username: labuser
- Password: Cyberlab123!
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/6c072edf-3ae3-4f46-9b5c-048614f666d9" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
After VM is created, set the Domain Controller’s NIC Private IP address to be static
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/e67af8f3-7fa6-4024-b8bd-f1b1d4c32920" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  
</p>
<p>
To test the network connectivity we will need to log into the VM and disable the Windows Firewall. To do this run remote desktop and log into the private IP address of DC-1 you created earlier. 
</p>
<p>
<img src="https://github.com/user-attachments/assets/61295057-ac31-41db-9592-bf4521ef01fa" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Once logged in, run wf.msc
</p>
<p>
<img src="https://github.com/user-attachments/assets/113431b3-2bc3-471d-82b0-2ba52323266d" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://github.com/user-attachments/assets/c15e54a9-d312-4ac6-b2e0-31c0704bbd34" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Then, turn off all firewalls from within the properties menu

</p>
<br />

<h2>Setup Client-1 in Azure</h2>

<p>
<img src="https://github.com/user-attachments/assets/774a13d8-0d8a-4331-bbf2-1659fa4fc21f" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Now its time to create the client virtual machine. This will run on windows 10. We will have it assigned to AD-Lab resource groub and on the AD-lab-network. Once again this will need to be placed in the same region as your RG and VN.  
  
- Username: labuser
- Password: Cyberlab123!
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/c012255a-57bf-4564-b45e-178a331f9b3b" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Now we need to set Client-1 DNS settings to DC-1's private IP address. You can find the DC-1 private IP address from the Virtual Machines tab in Azure. Restart Client-1 once completed. 
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/fbc2ec73-ff21-4109-926b-2b8b0345d135" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next, we will want to test our network settings by logging into Client-1 in Remote Desktop and attempting to ping DC-1 through PowerShell.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/66f7d70d-8bf3-4636-b2e6-ece0ae164ca2" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
We will also want to test ipconfig/all to ensure the DNS setting is showing DC-1's private IP address while we are in PowerShell.
</p>
<br />

<h2>Install Active Directory</h2>

<p>
<img src="https://github.com/user-attachments/assets/31ee478a-2030-46d9-8726-4e86bb326f9f" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Login to DC-1 and install Active Directory Domain Services
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/d6c6b21b-92b9-434a-a313-dc43f80ba4ca" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Promote as a DC: Set up a new forest as mydomain.com
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/d3d73d17-e0ef-4160-b0ab-ac0cc6c28d2f" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Once the install is complete, the remote desktop session will restart, and then you will log back into DC-1 as user mydomain.com\labuser
</p>
<br />

<h2>Create a Domain Admin user within the domain</h2>

<p>
<img src="https://github.com/user-attachments/assets/52318c34-22ce-4f90-b3fe-65ef8cce54aa" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES”
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/340d5a88-85f4-4643-8110-b938b43af3f6" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Create a new OU named “_ADMINS”
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/6589ced3-c121-48e3-9c45-1ac682e8a0e3" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Create a new employee named “Jane Doe” with the username of “jane_admin” and password of Cyberlab123!
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/27b3ffe2-4930-40e8-b429-b352c2255797" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Add jane_admin to the “Domain Admins” Security Group
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/94349990-9dfc-4ff2-89f5-5cd081a704bf" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Log out / close the connection to DC-1 and log back in as “mydomain.com\jane_admin”. User jane_admin as your admin account from now on
</p>
<br />

<h2>Join Client-1 to your domain (mydomain.com)</h2>

<p>
<img src="https://github.com/user-attachments/assets/7e1f39d9-75f1-48f8-9cc2-e2adb4482fab" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Log in to Client-1 as the original local admin (labuser) and join it to the domain (computer will restart)
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/4502f607-97e8-4191-889c-c9608300d8c9" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>Log inn to the Domain Controller and verify Client-1 shows up in ADUC
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/89eae75b-0773-4f10-b1f5-941473b6986e" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Create a new OU named “_CLIENTS” and drag Client-1 into there
</p>
<br />

<h2>Set up Remote Desktop for non-administrative users on Client-1</h2>

<p>
<img src="https://github.com/user-attachments/assets/8ea931d4-af9b-48b0-8450-f6f7c978b37c" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Log into Client-1 as mydomain.com\jane_admin
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/f68418d1-878c-4015-a008-77f40b9f8a26" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Open system properties
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/e247b46b-83a0-4a0e-9740-dd1d1e3cc558" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Click “Remote Desktop”
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/f6dd0c29-78bd-4775-b93a-52293c90faef" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Allow “domain users” access to remote desktop, You can now log into Client-1 as a normal, non-administrative user.
</p>
<br />

<h2>Create a bunch of additional users and attempt to log into client-1 with one of the users</h2>

<p>
<img src="https://github.com/user-attachments/assets/533b4d68-81a3-4dcf-8c71-400a5c1853b1" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Login to DC-1 as jane_admin and open PowerShell_ise as an administrator
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/dda6c354-a575-4aef-95d8-62e047f89c24" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Create a new File and paste the contents of this <a href="https://github.com/QuentinKunkle/Generate-Names-Create-Users">script</a></h1> into it
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Run the script and observe the accounts being created
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
When finished, open ADUC and observe the accounts in the appropriate OU　(_EMPLOYEES)
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
attempt to log into Client-1 with one of the accounts (take note of the password in the script)
</p>
<br />
