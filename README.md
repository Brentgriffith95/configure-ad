# config<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Video Demonstration</h2>

- ### [YouTube: How to Deploy on-premises Active Directory within Azure Compute](https://www.youtube.com)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Setup Azure Resources
- Make sure client and Domain Controller are connected
- Install Active Directory
- Create Admin and Normal User accout in Active Directory
- Join client 1 to Domain Controller
- Setup non administrative users on Client 1
- Create additional users and attempt to log into client 1
- Finish by deleting resources


<h2>Deployment and Configuration Steps</h2>

 ![zRe0cCf - Imgur](https://github.com/Brentgriffith95/configure-ad/assets/150200843/bc07a02d-5d9b-445b-9d13-0a610eb07029)
 
First, we will be creating the resources needed for this tutorial. The name for the resource group will vbe "AD-Rg" and the virtual machine will be named "DC-1". Next we will select Windows Server 2022 Data Center for the image*. Make sure the size* is set 2 VCPUs for optimal performance and then create VM.


 
![cnvB4x2 - Imgur](https://github.com/Brentgriffith95/configure-ad/assets/150200843/3dd1b5bd-cd7f-42af-b64c-3f40a18135c0)

Now we will create a 2nd Windows 10 VM named client 1. Make sure that it is made with same resource group, region, and size* VCPUs as the first VM. The Vnet must also be the same as "DC-1"



![vHsALnm - Imgur](https://github.com/Brentgriffith95/configure-ad/assets/150200843/84a41ee7-919c-473d-a63a-6d58962c2628)
Now head to DC-1 and go to network settings tab. Click on the NIC (Network Interface Card)


![yapmyOs - Imgur](https://github.com/Brentgriffith95/configure-ad/assets/150200843/af17726c-3fc8-426f-9e8d-9d4bfb4c3826)
Then click the "IP Configurations" tab and click "ipconfig1"




![ICLgPcW - Imgur](https://github.com/Brentgriffith95/configure-ad/assets/150200843/235b2ad3-02b3-4319-9f1d-6d54a0c665c7)
Set the IP to static then hit save


![IsPnO3R - Imgur](https://github.com/Brentgriffith95/configure-ad/assets/150200843/de9d9c35-2aaa-4bf5-904b-1fc9e57853a8)

Login into "Client-1" with RDC (Remote Desktop Connection). Then open Command Prompt and enter "ping-t" along with DC-1 private IP address. The requests will time out. 


![qHS3MUg - Imgur](https://github.com/Brentgriffith95/configure-ad/assets/150200843/30d128c4-e5c9-4526-b2f8-327e7a4784e9)
After, now log into "DC-1". In the search bar type windows defender firewall. Click Windows Defender Firewall with advanced securirty. 


![EP56DyQ - Imgur](https://github.com/Brentgriffith95/configure-ad/assets/150200843/edb19960-7857-4654-b703-c985b8f676e1)
Go to the inbound rules tab, sort it by protocol and highlight all ICMPv4 protocol rules. Then click enable, this will enable all ICMPv4  traffic between the VMs.


![HjE3v3N - Imgur](https://github.com/Brentgriffith95/configure-ad/assets/150200843/29ee363a-b85a-4027-8bb5-1cf5b6fd108d)
Head back to "Client 1" and ping "DC-1" private IP. We should now see the replies coming through. To stop the ping press Ctrl+C


![tOT1DNF - Imgur](https://github.com/Brentgriffith95/configure-ad/assets/150200843/b4678239-4698-4c25-9fa5-fc37a2f09c87)
We will now install Active Directory inside of DC-1. Login into DC-1 and open Server Manager. First click "Add roles and featuers", click next until you open "Server Roles", tick the box that says "Active Directory Domain Services, click "Add Features" and then continue clicking until you hit confirmation and click install. 


![WLEyLGe - Imgur](https://github.com/Brentgriffith95/configure-ad/assets/150200843/317ae7b4-2763-47b2-ba86-959e4fe28622)
Now we must promote DC-1 as the domain controller. Click on the flag with the yellow caution sign in the top right of Server Manager. Then click "Promote this user to domain controller". 


![MIWJoO0 - Imgur](https://github.com/Brentgriffith95/configure-ad/assets/150200843/8e592f0e-7d77-4641-a971-0b9e47804618)
Next, check "Add new forrest" and name it "mydomain.com" and hit next. You can choose any name you want. 


