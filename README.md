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

4. Ping DC1 from VM1 ussing DC1's Private IP Addrress
   
- Open the "Command Prompt" application on VM1 remote desktop.
- Type "ping -t 10.0.0.6" (DC1's Private IP Address) the press "Enter". This will startthe perpetual ping from VM1 to DC1
- Notice the the perpetual ping timesout due to the blockage of ICMP traffic due to the Network Security Group or "Firewall" on DC1


<img width="1145" alt="Screen Shot 2023-07-11 at 10 26 50 PM" src="https://github.com/SiclaitGitHub/configure-ad/assets/139138443/8a25e508-2b66-4a2e-b4fa-78a21f8cb622">


5. Open up ICMP traffic on DC1's firewall using wf.msc application

- Open Windows Defender Firewall application by typing "wf.msc" in the Start menu search bar
- 




