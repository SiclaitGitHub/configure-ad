<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud </h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Video Demonstration</h2>

- ### [YouTube: How to Deploy on-premises Active Directory within Azure Computer](https://www.youtube.com/watch?v=MHsI8hJmggI)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machine/Computer = "VM1")
- Domain Controller (Virtual Machine with AD Installed = "DC1")
- Microsoft Remote Desktop
- Active Directory Domain Services (AD)
- PowerShell ISE

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 

<h2>High-Level Deployment and Configuration Steps</h2>

- Create Virtual Machines in Azure (Windows 10 = "VM1")
- Create Virtual Machine in Azure (Virtual Machine with AD Installed = "DC1")
- Connect VM1 and DC1 to Microsoft Remote Desktop
- Ping (Perpetual Ping) DC1 from VM1 using DC1's Private IP Address
- Enable ICMP traffic on DC1's firewall using "wf.msc" application on DC1 
- Confirm ICMP traffic between VM1 and DC1
- Install Active Directory Application (AD)
- Promote Server Into Domain Controller
- Configure Active Directory Domain Services
- Create Admin User and Organizational User in AD
- Create new "User" under "_ADMINS" tab in AD (Users and Computers)
- Establish "Jane Doe" as an Administrative User
- Change DNS Server settings for VM1 in Azure
- Join VM1 to Domain Server
- Enable "Domain Users' with remote access to VM1
- Locate Domain Users in AD on DC1.
- Create users and use a random user to log into DC1

<h2>Deployment and Configuration Steps</h2>

<img width="908" alt="Screen Shot 2023-07-10 at 1 30 29 PM" src="https://github.com/SiclaitGitHub/osticket-prereqs/assets/139138443/4f97a83e-1c37-4210-9dc1-8530cb7f91bf">


1. Create your Virtual Machine in Azure (VM1)

Azure is a cloud computing platform and service offered by Microsoft. It provides a wide range of cloud services that enable organizations to build, deploy, and manage applications and services through Microsoft-managed data centers.

A virtual machine (VM) on Microsoft Azure is a computing resource that uses software instead of a physical computer to run programs and deploy apps. Each VM instance can run its own operating system (OS), which means multiple VMs can run different operating systems on the same physical machine.

- In Azure, Click on the "Virtual Machine" icon.
- Click on "Create".
- Select "Azure Virtual Machine".
- Select a "Resource Group".
- Create a VM name.
- Select the region.
- Select Windows 10 as your OS in the drop down menu titled "Image".
- Select your CPU "Size".
- Create Login Credentials.
- Check the box titled "I confirm I have an eligible Windows 10/11 license with multi-tenant hosting rights.".
- Click "Review + Create".
- Click "Create".

In Azure create a new resource group, virtual network, subnet and virtual machine running Windows 10. Choose a VM size according to your needs. Once the VM is set up, you will need to connect to it using Remote Desktop. For this, you'll need the public IP address of the VM and the credentials you provided during the VM setup.

<p><img width="918" alt="Screen Shot 2023-07-11 at 9 51 36 PM" src="https://github.com/SiclaitGitHub/configure-ad/assets/139138443/0cfdf26d-c770-4180-8dd1-d05351d4e8db">

</p>
<p>

2. Create Domain Controller VM in Azure (DC1)


- In Azure, Click on the "Virtual Machine" Icon.
- Click on "Create".
- Select "Azure Virtual Machine".
- Select a "Resource Group".
- Create a VM name.
- Make sure DC1 is in the same Resource Group and Region as VM1.
- Select "Windows Server 2022 Datacenter" as your OS in the drop down menu titled "Image".
- Select your CPU "Size".
- Create Login Credentials.
- Check the box titled "I confirm I have an eligible Windows 10/11 license with multi-tenant hosting rights.".
- Click "Review + Create".
- Click "Create".
  
</p>
<br />


<p><img width="600" alt="Screen Shot 2023-07-11 at 10 08 32 PM" src="https://github.com/SiclaitGitHub/configure-ad/assets/139138443/c1a74bef-1c85-4530-9698-a5e0baf36a76">


</p>
<p>  
3. Connect VM1 and DC1 to Microsoft Remote Desktop

Microsoft Remote Desktop (often abbreviated as "RDP" for Remote Desktop Protocol) is a technology developed by Microsoft that allows users to access and control a computer remotely over a network. It enables you to connect to a remote computer from another device, such as your personal computer, laptop, or mobile device, as long as both devices are connected to the internet or are part of the same local network.

- Copy the Public IP Addresses of Both VM1 and DC1.
- Click "Add PC" on the remote desktop and paste them into the login page.
- Use the login credentials created in Azure when setting up VM1 and DC1.


</p><img width="1024" alt="Screen Shot 2023-07-11 at 10 20 13 PM" src="https://github.com/SiclaitGitHub/configure-ad/assets/139138443/631f3e56-cac2-4c73-bc88-9ba14605fbf9">
<br />

4. Ping (Perpetual Ping) DC1 from VM1 using DC1's Private IP Address

"Ping" is a basic network utility used to test the reachability of a host (a device or computer) on an Internet Protocol (IP) network, and to measure the round-trip time for data packets to travel from the source device to the destination device and back. The term "ping" comes from the sonar technology, where a sound pulse (ping) is sent to detect objects underwater.

When you "ping" a host, your computer sends a small data packet to the destination device's IP address. If the destination device is reachable and online, it will respond by sending back a reply to the source device. The time taken for this round-trip communication is measured and displayed as the "ping time" or "round-trip time."
   
- Open the "Command Prompt" application on VM1. This application can be found in the “Start”menu.
- Type "ping -t 10.0.0.6" (DC1's Private IP Address) the press "Enter". This will start the perpetual ping from VM1 to DC1.
- Notice the perpetual ping timeout due to the blockage of ICMP traffic due to the Network Security Group or "Firewall" on DC.


<img width="1439" alt="Screen Shot 2023-07-12 at 10 15 27 AM" src="https://github.com/SiclaitGitHub/configure-ad/assets/139138443/2ab18f4b-5726-4338-a7a5-c069d69bcf0c">

5. Open up ICMP traffic on DC1's firewall using wf.msc application on DC1

ICMP (Internet Control Message Protocol) is a network protocol used to send error messages and operational information about IP (Internet Protocol) network communication. It is an integral part of the Internet Protocol Suite and operates at the Network Layer (Layer 3) of the OSI model. ICMP is typically used by networking devices, routers, and hosts to communicate network-related information and perform various network management functions.

- Open Windows Defender Firewall application by typing "wf.msc" in the Start menu search bar.
- Select "Inbound Rules" in the top left corner of the "wf.msc" application on DC1.
- Click on the "Protocol" column to sort data.
- Locate the "ICMP V4" protocol. This is the protocol that "ping" uses.
- Enable both "Core Networking Diagnostics - ICMP Echo Request (ICMPv4-in)". Make sure to enable both rules that share this title.



<img width="1105" alt="Screen Shot 2023-07-12 at 10 17 49 AM" src="https://github.com/SiclaitGitHub/configure-ad/assets/139138443/3d1fc3ec-ddcc-4084-aad7-778ea6afc2bc">


6. Confirm ICMP traffic between VM1 and DC1

- Go back to "Command Prompt" application on VM1. You will notice that the perpetual ping that was created on VM1 is now receiving reply traffic from DC1.


<img width="926" alt="Screen Shot 2023-07-12 at 10 25 09 AM" src="https://github.com/SiclaitGitHub/configure-ad/assets/139138443/a08a15d6-b74d-463c-b1c1-051461cc8f7c">

7. Install Active Directory Application (AD)

- Open "Server Manager" application on DC1.
- Select "New Roles & Features".
- Select "Active Directory Domain Services" and Click "Add Role and Features".
- Click "Next" until the option to install becomes available. 
- Click "Install" to begin installation of Active Directory.
- Restart DC1 once installation is complete.


<img width="1417" alt="Screen Shot 2023-07-12 at 10 29 12 AM" src="https://github.com/SiclaitGitHub/configure-ad/assets/139138443/f302162e-90df-43c4-be5a-3b14fd4c25fa">


8. Promote Server Into Domain Controller

   A Domain Controller (DC) is a server in a Windows-based network that serves as a central authority for security, authentication, and management of network resources. It plays a crucial role in the implementation of Active Directory (AD), which is Microsoft's directory service that provides a hierarchical structure for organizing and managing network resources, such as user accounts, computers, groups, and shared resources like printers and file servers.

- Select the yellow "!" icon in the top left corner of the Server Manager window on DC1.
- Select "Promote This Server into a Domain Controller".


<img width="941" alt="Screen Shot 2023-07-12 at 10 39 02 AM" src="https://github.com/SiclaitGitHub/configure-ad/assets/139138443/782b78c8-f7b1-4f1c-896b-17dcc67fa33a">

9. Configure Active Directory Domain Services

- Select "Add New Forest" in the "Deployment Configuration" window found through the Server Manager application on DC1.
- Title the new forest "mydomain.com" and click "Next”. 
- Create login credentials.
- Click "Next" for all prompts until the "Install" button becomes available. Then click "Install".
- Keep in mind, you may be disconnected from DC1's remote connection after installation is complete. 
- Reestablish remote connection with DC1 (using DC1's Public IP Address) as "mydomain.com\User305".
- log in using the credentials we created when "New Forest" was created.


<img width="893" alt="Screen Shot 2023-07-12 at 12 31 29 PM" src="https://github.com/SiclaitGitHub/configure-ad/assets/139138443/878a4f38-b282-47f7-a1e1-14603305bffa">


10. Create Admin User and Organizational Units in AD

- Select "Tools" in the top right corner of the AD window and select "Active Directory Users and Computers".
- This will open the standard AD user interface.
- Right click the "mydoinain.com" file then "New" then "Organizational Unit" in the top left corner of the AD interface window.
- Or click "Action" after selecting "mydomain.com" and then select "New " then "Organizational Unit".
- Create an Organizational Unit titled "_EMPLOYEES".
- Repeat this process and create an Organizational User titled "_ADMINS".


<img width="816" alt="Screen Shot 2023-07-12 at 12 41 05 PM" src="https://github.com/SiclaitGitHub/configure-ad/assets/139138443/6e3bcd97-a879-4728-8f3e-5319b956efca">

11. Create new "User" under "_ADMINS" tab in AD (Users and Computers)

- Right click "_ADMINS" select "New" then click on "User".
- Create an admin account named "Jane Doe".
- Create User login credentials - Username "Jane_Doe@mydomian.com" Password "Password347!".
- When creating the password, uncheck the "User must change password at next log in" box before clicking "Next".
  This is for the sake convenience during this lab. In a real work application, you want the user to create their own password.
  
<img width="1028" alt="Screen Shot 2023-07-12 at 12 48 49 PM" src="https://github.com/SiclaitGitHub/configure-ad/assets/139138443/9cf57e4c-b46b-428b-ac0a-1efb637d7d47">

12. Establish "Jane Doe" as an Administrative User

- In the AD interface window select "Jane Doe" and click on "Actions" then "Properties".
- When in the "Properties" window of the "Jane Doe" AD user window select the "Member Of" tab.
- Type the word "Domain" in the search back then click on "Search Names".
- A list of search results will appear. Select "Domain Admins" then click "OK".
- Log out of DC1.
- Using Microsoft Remote Desktop, log back in as "Jane Doe" using "mydomain.com\jane_admin" as the user name.

<img width="1436" alt="Screen Shot 2023-07-12 at 2 29 00 PM" src="https://github.com/SiclaitGitHub/configure-ad/assets/139138443/a629673e-b5ea-411e-8fde-a124a80ee51d">

13. Change DNS Server settings for VM1 in Azure

A DNS server, also known as a Domain Name System server, is a critical component of the internet infrastructure that translates human-readable domain names into their corresponding IP addresses. DNS serves as a distributed hierarchical system, allowing users to access websites, services, and resources using easy-to-remember domain names (like www.example.com) rather than numerical IP addresses (like 192.0.2.1).

When you enter a domain name in your web browser or any other network application, your computer first contacts a DNS resolver (usually provided by your internet service provider or a public DNS resolver like Google's 8.8.8.8). The resolver then communicates with one or more DNS servers to resolve the domain name to its corresponding IP address.

In order to join the Domain we have to use the Domain Controller as the DNS server. This is because the Domain Controller knows what "mydomain.com" is. The current server will try to located the domain on the internet, as opposed to within the server with have configured. 

- Go to VM1 in Azure.
- Click on the "Networking" tab on the VM1 window in Azure.
- Click on the "network Interface" hyperlink (Ex: vm1460).
- Click on "DNS Servers" on the "vm1460" the (Network Interface window).
- Select "Custom".
- Enter DC1's Private IP Address (10.0.0.6) in the "Domain Server" Field the click "Save".
- Select "Restart" on the VM1 page of Azure. This will flush the DNS cache. 


<img width="1408" alt="Screen Shot 2023-07-12 at 2 20 21 PM" src="https://github.com/SiclaitGitHub/configure-ad/assets/139138443/13609164-8ccb-4424-9d70-36ae0a39f69e">

14. Join VM1 to the Domain Server

- Log back in to VM1 using "User305" credentials using Microsoft Remote Desktop. 
- Right click the "Start" button on VM1 and select "System".
- Click on "Rename this PC (Advanced)" in the column on the right of the "System Settings" window.
- Click on "Change" under the "Computer Name" tab of the "System properties" window.
- Click on "Domain".
- Type "mydomain.com" and click "OK".
- Enter admin login credentials "mydomain.com\jane_admin".
- VM1 will restart. We can now use the domain admin account credentials ("mydomain.com\jane_admin") to log into VM1.
  

<img width="1360" alt="Screen Shot 2023-07-12 at 2 51 22 PM" src="https://github.com/SiclaitGitHub/configure-ad/assets/139138443/f7e6c061-d897-468e-bef5-448670fb8bec">


15. Enable "Domain Users' with remote access to VM1

 - In VM1 right click the "Start" and select "System".
 - Click on "Remote Desktop" in the column on the right of the "System Settings" window.
 - Under the "Account Users" tab click on the "Select users that can remotely access this PC" hyperlink.
 - Select "Add".
 - Type "Domain users" into the search bar and click "Check Names"
 - Add the underlined results of the search.
 - Click "OK".

<img width="814" alt="Screen Shot 2023-07-12 at 2 53 50 PM" src="https://github.com/SiclaitGitHub/configure-ad/assets/139138443/14e28834-2f9d-4138-8d0f-847489469edf">

16. Locate Domain Users in AD on DC1

- Open AD (Users and Computers) in DC1.
- Expand "mydomain.com" then expand "Users" and "Domain Users" will be visible.

<img width="1386" alt="Screen Shot 2023-07-12 at 3 08 47 PM" src="https://github.com/SiclaitGitHub/configure-ad/assets/139138443/eb318f8c-7aec-4f2c-84fb-ea31bf2fa365">

17. Create users and use a random user to log into DC1
    
- Open PowerShell ISE in DC1 as an admin (jane_admin).
- Select "Run as Administrator" before opening PowerShell ISE.
- Open "PowerShell ISE" in DC1.
- Paste user generation script into PowerShell ISE and run. This script will generate random users and login credentials.
- Click "Enter". Random user generation should begin automatically.
- The user accounts being generated by the script entered in PowerShell ISE should be visible in the "_EMPLYEES" folder in AD (Users & Computers).
- Select a random user (Ex: bal.qog) & Password: "Password1" (specified in the script).
- The successful login will confirm that the VM1 has successfully been joined to the Domina Server.









