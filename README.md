<p align="center">
<img src="https://github.com/ronnydiggs/SCCM/assets/64152064/6842b8d8-eb69-421e-959f-a8fa5d20b3ea" width="500"/>
</p>

<h1>Microsoft Endpoint Configuration Manager (MECM/SCCM) - Prerequisites and Installation</h1>
This lab outlines the prerequisites and installation of Microsoft Endpoint Configuration Manager and the deployment of software on to a domain computer from a distribution point. The goal of this lab is to get hands on experience onboarding new users and setting up baseline application configuration for all computers on a domain.<br />


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
5. From HYD-CM1, locate the Client folder in the Microsoft Configuration Manager folder in the C drive and copy it into HYD-CLIENT1.  
</p>
<p>6. Copy the client folder into the C drive using \\CLIENT1\c$ in file explorer and install the ccmsetup.exe to get access to Software Center.</p>

<br />

<p>
7. We want a package from definition because want to download a specific package software. In this lab, we will be using msi files. From HYD-CM1, inside MECM go to software library -> applications management -> packages -> create packages from definition.</p>
<p>
8. Download any msi file you need for your company. Store the downloads into a package folder for organization. Add permissions for Domain Users and Domain Computers to read, execute in the packages folder. This allows for all computers and users on the domain to be able to access and install the programs from the software center.
</p>
<p>
9. Click create a package from definition -> browse for desired file, next -> Select always obtain source files from source folder, next ->  Copy the network path of the msi file and use it as the package source folder. \\CM1\Packages$\putty
</p>
<p>
10. Click next, confirm the configurations and close the wizard. The msi file should be visible in the packages window.
</p>

<p>
11. From the packages window, right-click the desired package and click deploy. 
</p>
<p>
  
- In the General tab, under software select per system unattended and under collection select all desktop and server clients.
- In the Contents tab, add CM1.corp.contoso.com as the distribution point.
- In the Deployment Settings tab, choose a desired off-hour schedule for installs and updates to not affect business production. For the purposes of this lab, leave scheduling and user experience default.
- In the Distribution Points tab, select "Download content from distribution point and run locally" and "Do not run program."
- Confirm the settings and deploy. 
</p>
<p>
12. In the HYD-CLIENT1, go to Control Panel, configuration manager, click the actions tab. Run all the cycles in the action tab. This speeds up the client cycle so that the package will show up in the software center quicker.</p>
<p>
13. Go to Software Center, select the application you want to install. Once installed, the application will be available for any user on the computer. 
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
