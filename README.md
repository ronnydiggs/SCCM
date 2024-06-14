# SCCM

<p align="center">
<img src="https://github.com/ronnydiggs/SCCM/assets/64152064/6842b8d8-eb69-421e-959f-a8fa5d20b3ea" width="500"/>
</p>

<h1>Microsoft Endpoint Configuration Manager (MECM/SCCM) - Prerequisites and Installation</h1>
This tutorial outlines the prerequisites and installation of Microsoft Endpoint Configuration Manager. The goal of this lab is to learn data migration and software configuration.<br />


<h2>Lab Environment</h2>
<p>The lab environment provides you with an automatically provisioned virtual lab environment, including domain-joined desktop clients, a domain controller, an Internet gateway, and a fully configured Configuration Manager instance. The following products are included:</p>

- Windows 11 Enterprise, Version 22H2
- Microsoft Configuration Manager, Version 2211
- Windows Assessment and Deployment Kit for Windows 11
- Windows Server 2022 

<h2>Installation Steps</h2>

<p>
1. Enable Hyper-V from Windows Features. Make sure all boxes are checked within the Hyper-V tab. Restart the computer.
</p>
<p>
2. Download the lab environment from here: https://www.microsoft.com/en-us/evalcenter/download-mem-evaluation-lab-kit 
</p>
<p>
3. Unzip into desired folder and directory and run the setup.exe</p>
<p>
4. Once the setup is complete, open Hyper-V Manager and connect to these virtual machines:

- HYD-CM1
- HYD-DC1
- HYD-CLIENT1
</p>
<p>
5. From HYD-CM1, open Microsoft Configuration Manager.  
</p>
<br />

<p>
<img src="https://github.com/ronnydiggs/configure-ad/assets/64152064/56d83058-1c1d-4776-9aa5-bce8f669680f" width="500"/>
</p>
<p>
Use the same resource group AD-Lab and select the image as Windows 10 Pro. 
</p>
<br />

<p>
<img src="https://github.com/ronnydiggs/configure-ad/assets/64152064/647f8e94-ede6-4869-bdeb-542fd60e549f" width="500"/>
</p>
<p>
Go to the networking tab and change the virtual network to DC-1 vnet, so that Client 1 and Domain Controller are on the same virtual network. 
Create the Virtual Machine for Client 1.
</p>
<br />

<p>
<img src="https://github.com/ronnydiggs/configure-ad/assets/64152064/62583ba5-ad7f-4355-b49e-880420af9d32" width="500"/>
</p>
<p>
Go to the networking tab and change the virtual network to DomainController vnet, so that Client 1 and Domain Controller are on the same virtual network. 
Create the Virtual Machine for Client 1.
</p>
<br />

<p>
<img src="https://github.com/ronnydiggs/configure-ad/assets/64152064/62583ba5-ad7f-4355-b49e-880420af9d32" width="250"/>
</p>
<p>
Go to network settings of DomainController VM. Click the network interface domaincontroller180_z1. 
Go to ipconfig1 and change the private IP address to static. This is important because we want to keep the IP address from changing.
</p>
<br />

<h2>Test the connection between VMs</h2>

<p>
<img src="https://github.com/ronnydiggs/configure-ad/assets/64152064/18a7a67a-aa28-47d6-b5bb-636989bdc664" width="500"/>
</p>
<p>
As currently constructed, the two VMs cannot ping one another. We want Client 1 to be able to ping the domain controller. To do this we need to change a firewall rule to allow ICMPv4 traffic in the DomainController VM.
</p>
<br />

<p>
<img src="https://github.com/ronnydiggs/configure-ad/assets/64152064/af0ca4f9-5970-4b0a-a658-505413d2ddaa" width="500"/>
</p>
<p>
Enable firewall rule to allow ICMPv4 traffic (echo request and reply) for IP address 10.0.0.4, the Client 1 IP.
</p>
<br />

<p>
<img src="https://github.com/ronnydiggs/configure-ad/assets/64152064/3bec9090-441d-4a26-93b7-3963a1583465" width="400"/>
</p>
<p>
Test the connection between Client 1 and Domain Controller. Now the two VMs can communicate.
</p>
<br />

<p>
<img src="https://github.com/ronnydiggs/configure-ad/assets/64152064/3bec9090-441d-4a26-93b7-3963a1583465" width="400"/>
</p>
<p>
Test the connection between Client 1 and Domain Controller. Now the two VMs can communicate.
</p>
<br />

<h2>Install Active Directory on Domain Controller</h2>

<p>
<img src="https://github.com/ronnydiggs/configure-ad/assets/64152064/598e65fd-6676-4334-8bf8-ffeb3153be41" width="400"/>
</p>
<p>
Install Active Directory with Domain Services checked.
</p>
<br />

<p>
<img src="https://github.com/ronnydiggs/configure-ad/assets/64152064/c879a299-5665-4254-a9df-816e4d04fc5d" width="500"/>
</p>
<p>
At the top right, click the flag and select promote a domain controller. Set add a new forest and name the domain: mydomain.com and install.
</p>
<br />

<p>
<img src="https://github.com/ronnydiggs/configure-ad/assets/64152064/c24ca1bd-086d-4553-9892-37e07243b4a4" width="400"/>
</p>
<p>
RDP into DomainController as a user.
</p>
<br />

<h2>Create Admin and User Accounts in Active Directory</h2>

<p>
<img src="https://github.com/ronnydiggs/configure-ad/assets/64152064/f270ab53-7f0e-4874-be7b-53be41bf98ca" width="400"/>
</p>
<p>
In Active Directory Users and Computers, create an organization unit called _EMPLOYEES and _ADMINS.
</p>
<br />

<p>
<img src="https://github.com/ronnydiggs/configure-ad/assets/64152064/88e11b22-20bb-4365-b014-2ff5c624d425" width="400"/>
</p>
<p>
Create an admin account.
</p>
<br />

<p>
<img src="https://github.com/ronnydiggs/configure-ad/assets/64152064/8112c34b-569a-41c6-b78d-d86e24d5c608" width="400"/>
</p>
<p>
Add the admin user to domain admins security group. Restart and Log back in (RDP) as mydomain.com\ronny
</p>
<br />

<h2>Join Client 1 to your Domain</h2>

<p>
<img src="https://github.com/ronnydiggs/configure-ad/assets/64152064/9a868978-978a-4853-9da1-f35f5305bcb1" width="500"/>
</p>
<p>
Go to network settings of DomainController VM. Click the network interface client1464_z1. 
Go to DNS Servers and add the private IP address: 10.0.0.5. This is important because essentially we are making the Domain controller the DNS server for Client 1.
Restart Client 1 in Azure portal.
Login (RDP) to Client 1 as admin user DClab. 
</p>
<br />

<p>
<img src="https://github.com/ronnydiggs/configure-ad/assets/64152064/382f3fe3-b060-4d9f-b779-6621fc981c21" width="400"/>
</p>
<p>
Check the DNS Server in command prompt.
</p>
<br />

<p>
<img src="https://github.com/ronnydiggs/configure-ad/assets/64152064/0aa0a0ad-da35-4c46-b4b9-90d8b5d9c713" width="400"/>
<img src="https://github.com/ronnydiggs/configure-ad/assets/64152064/c501d643-fc30-47b9-8afb-6065c594da20" width="400" />  
</p>
<p>
Right click the windows logo. Go to System. Rename this PC (advanced). Rename the domain to mydomain.com, restart and check the change with ipconfig /all.
</p>
<p>
<img src="https://github.com/ronnydiggs/configure-ad/assets/64152064/9f997afe-dc4e-40e6-b7b7-0ead9eae0bdb" width="400" />  
</p>
<p>
Verify Client1 is in the Computers container of Active Directory Users and Computers.  
</p>
<br />

<h2>Setup Remote Desktop for Non-Admin Users on Client 1</h2>

<p>
<img src="https://github.com/ronnydiggs/configure-ad/assets/64152064/988d61e2-b17b-4ff1-9ae0-46c881e8aa7c" width="500" />
</p>
<p>
Right click the windows logo. Go to System. Select Users that can remotely access this PC. Enter Domain Users under domain. Now non-admin users can RDP.
</p>
<br />

<p>
<img src="https://github.com/ronnydiggs/configure-ad/assets/64152064/4639a211-be40-480d-b4d2-00d2f70887d8" width="500" />
<img src="https://github.com/ronnydiggs/configure-ad/assets/64152064/f5c91ef1-d4c6-4c94-8379-eedb1d70c1bc" width="500" />
</p>
<p>
On the Domain Controller, run Powershell ISE as adminstrator and add a script to create non-admin user accounts. Log in with a random user on Client 1.
</p>
<br />
