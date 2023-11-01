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

- Create 2 Virtual Machines and log into RDP
- Create a hole in the firewall in DC-1
- Step 3
- Step 4

<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/6SkQzM0.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
First thing I do is login the Azure Portal and create two virtual machines. Name the first one DC-1(Domain Controller) and the second one Client-1. DC-1 will be the Windows Server, the 2022 version and Client-1 will be Windows 10. After both VMs are created, I go into the settings of the DC-1 and change the IP Address from dynamic to static. This means the IP address never changes. Then I proceed to login into the Client-1 VM by using the IP address and inputing it in the Remote Desktop Protocol. The RDP allows me to access the computer remotely without being present. Once the computer is pulled up, I start to ping DC-1 private IP Address as shown in the picture above but it is failing because the firewall. Now I have to create a hole in the firewall to allow ICMP traffic.
</p>
<br />

<p>
<img src="https://i.imgur.com/Sr70LQR.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
To create a hole in the firewall, I must login into DC-1 via RDP. So, I create another RDP and log in. Once I'm in, I bring up the Server Manager and WF.MSC(Windows Firewall). I go to Inbound Rules then click on protocol. Plenty of options will show up, scroll down and click CORE NETWORKING-DESTINATION UNREACHABLE ECHO REQUEST and enable rule. Note: "I make sure it is IPv4, Internet Protocol version 4. Now switch the Client-1 RDP and start the ping over. It should work because a hole was created in the firewall to allow ICMP traffic. Once its confirmed, I start the process to download Active Directory.
</p>
<br />

<p>
<img src="https://i.imgur.com/YXtkSuT.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
I must install Active Directory on the DC-1 VM. Once I'm inside DC-1, I pull up the server manager and then go to roles and features and begin the installation of Active Directory Domain Services. After the install is complete, I proceed to promote this server to a domain server, which direct me to a menu to give the server a domain name. I name it Jumpman23domain.com(Huge NBA Fan lol) The NetBios name will be Jumpman23domain. 
</p>
<br />

<p>
<img src="https://i.imgur.com/VNaoNUY.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Now after the Active Directory is installed and has a domain name. Client-1 DNS(Domain Name Service) must have be the same as DC-1 IP address because when Client-1 attempts to join the domain it will search the internet for the domain name but when the DNS has the same IP as DC-1, it will already be on the network. In order to change the DNS, I must go into the Azure Portal and copy DC-1 NIC Private IP then go into Client-1 DNS Servers Setting and add a custom address and input it. Restart the Client-1 VM.Then I log back into Client-1 RDP, and go to system settings and allow "Domain Users" to access this VM.
</p>
<br />

<p>
<img src="https://i.imgur.com/UKOkSjR.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  <img src="https://i.imgur.com/0zxv4od.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
After all the steps above are done right, I log back onto DC-1 and bring up Active Directory Users and Computers. I begin to make Organizational Units titled, _EMPLOYEES and _ADMINS. I make a user in the _ADMINS unit, the user name is JUMP MAN and the login is Jump.man@jumpman23domain. I also make users in the _EMPLOYEES folder, and Admins and Employee will have different access clearance. Employees and Admins can log onto the Client-1 RDP as shown above.
</p>
<br />
