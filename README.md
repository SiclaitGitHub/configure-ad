<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Video Demonstration</h2>

- ### [YouTube: How to Deploy on-premises Active Directory within Azure Compute](https://www.youtube.com)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machine/Computer)
- Domain Controller (Virtual Machine with AD Installed)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Step 1
- Step 2
- Step 3
- Step 4

<h2>Deployment and Configuration Steps</h2>

<img width="908" alt="Screen Shot 2023-07-10 at 1 30 29 PM" src="https://github.com/SiclaitGitHub/osticket-prereqs/assets/139138443/4f97a83e-1c37-4210-9dc1-8530cb7f91bf">


1. Set up your Azure Virtual Machine (VM1)

Azure is a cloud computing platform and service offered by Microsoft. It provides a wide range of cloud services that enable organizations to build, deploy, and manage applications and services through Microsoft-managed data centers.

A virtual machine (VM) on Microsoft Azure is a computing resource that uses software instead of a physical computer to run programs and deploy apps. Each VM instance can run its own operating system (OS), which means multiple VMs can run different operating systems on the same physical machine.

Create a new Azure resource group, virtual network, subnet and virtual machine running Windows 10. Choose a VM size according to your needs. Once the VM is set up, you will need to connect to it using Remote Desktop. For this, you'll need the public IP address of the VM and the credentials you provided during the VM setup.

<p><img width="918" alt="Screen Shot 2023-07-11 at 9 51 36 PM" src="https://github.com/SiclaitGitHub/configure-ad/assets/139138443/0cfdf26d-c770-4180-8dd1-d05351d4e8db">

</p>
<p>

2. Create Domain Controller VM (DC1) in Azure

- For DC1's Operating System, select Windows Server 2022 Datacenter
- Make sure DC1 is in the same network and region as VM1
</p>
<br />


<p><img width="600" alt="Screen Shot 2023-07-11 at 10 08 32 PM" src="https://github.com/SiclaitGitHub/configure-ad/assets/139138443/c1a74bef-1c85-4530-9698-a5e0baf36a76">


</p>
<p>  
3. Connect VM1 and DC1 to the remote deskotp. 

- Copy the Public IP Addresses of Both VM1 and DC1
- Click "Add PC" on the remote desktop and past them in to the log in page.
- Use the log in cridentials created in Azure when setting up VM1 and DC1


</p><img width="1024" alt="Screen Shot 2023-07-11 at 10 20 13 PM" src="https://github.com/SiclaitGitHub/configure-ad/assets/139138443/631f3e56-cac2-4c73-bc88-9ba14605fbf9">
<br />

4. Ping (Perpectual Ping) DC1 from VM1 using DC1's Private IP Addrress
   
- Open the "Command Prompt" application on VM1 remote desktop.
- Type "ping -t 10.0.0.6" (DC1's Private IP Address) the press "Enter". This will start the perpetual ping from VM1 to DC1
- Notice the the perpetual ping timesout due to the blockage of ICMP traffic due to the Network Security Group or "Firewall" on DC1


<img width="1439" alt="Screen Shot 2023-07-12 at 10 15 27 AM" src="https://github.com/SiclaitGitHub/configure-ad/assets/139138443/2ab18f4b-5726-4338-a7a5-c069d69bcf0c">

5. Open up ICMP traffic on DC1's firewall using wf.msc application on DC1

- Open Windows Defender Firewall application by typing "wf.msc" in the Start menu search bar
- Select "Inbound Rules" in the top left corner of the "wf.msc" applicantion on DC1
- Click on the "Protocol" column to sort data
- Locate the "ICMP V4" proptocol. This is the protocol that "ping" uses
- Enable both "Core Networking Diagnostics - ICMP Echo Request (ICMPv4-in)". Make sure to enable both rules that share this title.



<img width="1105" alt="Screen Shot 2023-07-12 at 10 17 49 AM" src="https://github.com/SiclaitGitHub/configure-ad/assets/139138443/3d1fc3ec-ddcc-4084-aad7-778ea6afc2bc">


6. Confirm ICMP traffic between VM1 and DC1

- Go back to the VM1 remote desktop. You will notice that the perpetual ping that was created on VM1 is now recieving reply traffic from DC1


<img width="926" alt="Screen Shot 2023-07-12 at 10 25 09 AM" src="https://github.com/SiclaitGitHub/configure-ad/assets/139138443/a08a15d6-b74d-463c-b1c1-051461cc8f7c">

7. Install Active Directly Application (AD)

- Open "Server Manager" application on DC1.
- Select "New Roles & Features"
- Select "Active Directory Domain Services" and Click "Add Features"
- Click "Install" to being installation of Active Directory


<img width="1417" alt="Screen Shot 2023-07-12 at 10 29 12 AM" src="https://github.com/SiclaitGitHub/configure-ad/assets/139138443/f302162e-90df-43c4-be5a-3b14fd4c25fa">


8. Promote Server Into Domain Controller

- Select the yeller "!" icon in the the top lect window if the Server Managmer on DC1
- Select "Promote This Server into A Domain Controller"


<img width="941" alt="Screen Shot 2023-07-12 at 10 39 02 AM" src="https://github.com/SiclaitGitHub/configure-ad/assets/139138443/782b78c8-f7b1-4f1c-896b-17dcc67fa33a">

9. Configure Active Directory Domain Services

- Select "Add New Forest" in the "Deployment Configuration" window found through the Server Manager application on DC1
- Title the new forest "mydomain.com" and click "Next" 
- Set log in creidentials 
- Click "Next" for all prompts until "Insatall" buttom becomes avilable. Then click "Install"
- Keep in mind you may be disconnected from DC1's remote connection after installation is complete. 
- Reestablish remote connection with DC1 (using DC1's Public IP Address) as "mydomain.com\User305". These are the log in cridentials we created when "New Forest" was created.


<img width="893" alt="Screen Shot 2023-07-12 at 12 31 29 PM" src="https://github.com/SiclaitGitHub/configure-ad/assets/139138443/878a4f38-b282-47f7-a1e1-14603305bffa">


10. Create Admin User and Organizational User in AD

- Select "Tools" the top right corner of the AD window and select "Active Directory Users and Computuers"
- This will open the stadard AD user interface.
- Right click the "mydoinain.com" file then "New" then "Organizational User" in the top left corner of the AD interface window
- Or click "Action" after selecting "mydomain.com" and then "select "New" then "Organizational User"
- Create a Organizational User titled "_EMPLOYEES"
- Repeat this process and create an Organizational User ttiled "_ADKINS"


<img width="816" alt="Screen Shot 2023-07-12 at 12 41 05 PM" src="https://github.com/SiclaitGitHub/configure-ad/assets/139138443/6e3bcd97-a879-4728-8f3e-5319b956efca">

11. Create new "User" under "_ADMINS" tab in AD (Users and Computers)

- Right clikc "_ADMINS" select "New" then click on "User"
- Creae an admin account name "Jane Doe"
- Create log in cridnetials - Username "Jane_Doe@mydomian.com" Password "Password347!"
- When creating the password uncheck the "User must change password at next log in" box before clicking "Next"


<img width="1028" alt="Screen Shot 2023-07-12 at 12 48 49 PM" src="https://github.com/SiclaitGitHub/configure-ad/assets/139138443/9cf57e4c-b46b-428b-ac0a-1efb637d7d47">

12. Establish "Jane Doe" as an Administrative User

- In the AD interface window select "Jane Doe" and click on "Actions" then "Properties".
- When in the "Properties" window of the "Jane Doe" AD user window select the "Memebr Of" tab.
- Type the work "Domain" in the search back then click on "Search Names"
- A list of search results will appear. Select "Domain Admins" then click "OK"
- Lo out of DC1
- Log back in as "Jane Doe" using "mydomain.com\jane_admin" as the user name.

  



<img width="1436" alt="Screen Shot 2023-07-12 at 2 29 00 PM" src="https://github.com/SiclaitGitHub/configure-ad/assets/139138443/a629673e-b5ea-411e-8fde-a124a80ee51d">

13. Change DNS Server settings for VM1 in AZure

- Go to VM1 in Azure.
- Click on the "Networking" tab on the VM1 window in Azure
- Click on the "network Interface" hyberlink (Ex: vm1460)
- Click on "DNS Servers" on the "vm1460" the (Network Interface window).
- Select "Custom"
- Enert DC1's Private IP Address (10.0.0.6) in the "Domain Server" Field the click "Save"
- Select "Restart" on the VM1 page of Azure. This will flush the domina cache. 


<img width="1408" alt="Screen Shot 2023-07-12 at 2 20 21 PM" src="https://github.com/SiclaitGitHub/configure-ad/assets/139138443/13609164-8ccb-4424-9d70-36ae0a39f69e">

14. Join VM1 to Domain Server

- Log back in to VM1 using "User305" cridentials.
- Right click the "Start" botton on VM1 and select "System"
- Click on "Rename this PC (Advanced)" in the coloum on the right os the "System Settings" window
- Click on "Change" under the "Computer Name" tab of the "System properties" window.
- Click on "Domain"
- Type "mydomain.com" and click "OK"
- Enter admin log in cridentials "mydomain.com\jane_admin"
- VM1 will restart. We can now use the domain admin account credientials to log into VM1
  

<img width="1360" alt="Screen Shot 2023-07-12 at 2 51 22 PM" src="https://github.com/SiclaitGitHub/configure-ad/assets/139138443/f7e6c061-d897-468e-bef5-448670fb8bec">


15. Enable "Domain Users' with remote access to VM1

 - In VM1 right click the "Start" and select "System"
 - Click on "Remote Desktop" in the column on the right of the "System Settings" window
 - Under the "Account Users" tab click on the "Select users that can remotley access this PC" hyperlink
 - Select "Add"
 - Typer "Domain users" into the search bar and click "Check Names"
 - Add the underlined resulst of the search.

<img width="814" alt="Screen Shot 2023-07-12 at 2 53 50 PM" src="https://github.com/SiclaitGitHub/configure-ad/assets/139138443/14e28834-2f9d-4138-8d0f-847489469edf">

16. Locate Domain Users in AD on DC1

- Open AD (Users and Computers) in DC1
- Expand "mydomain.com" then expand "Users" and "domian Users" will be visible






