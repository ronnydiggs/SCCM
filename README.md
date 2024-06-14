<p align="center">
<img src="https://github.com/ronnydiggs/SCCM/assets/64152064/6842b8d8-eb69-421e-959f-a8fa5d20b3ea" width="500"/>
</p>

<h1>Microsoft Endpoint Configuration Manager (MECM/SCCM) Prerequisites and Installation</h1>
This lab outlines the prerequisites and installation of Microsoft Endpoint Configuration Manager and the deployment of software on to a domain computer from a distribution point. The goal of this lab is to get hands on experience onboarding new users and setting up baseline application configuration for all computers on a domain.<br />

<h2>Lab Environment</h2>
<p>The lab environment provides you with an automatically provisioned virtual lab environment, including domain-joined desktop clients, a domain controller, an Internet gateway, and a fully configured Configuration Manager instance. The following products are included:</p>

- Windows 11 Enterprise, Version 22H2
- Microsoft Configuration Manager, Version 2211
- Windows Assessment and Deployment Kit for Windows 11
- Windows Server 2022 

<h2>Video Tutorial</h2>
<p>
Work in Progress
<!-- <img src="https://github.com/ronnydiggs/SCCM/assets/64152064/6842b8d8-eb69-421e-959f-a8fa5d20b3ea" width="500"/> -->
</p>
<p>
  
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

<h2>Create Package from Definition</h2>
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
<h2>Deploy Package</h2>
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



</p>
<br />
