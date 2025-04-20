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
-Set up Remote Desktop for non-administrative users on Client-1
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
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Login to DC-1 and install Active Directory Domain Services
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Promote as a DC: Setup a new forest as mydomain.com
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Restart and then log back into DC-1 as user: mydomain.com\labuser
</p>
<br />

