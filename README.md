#VMware Active Directory Environment

I spun up some VMs in VMware Workstation-Server 2019 and Win10. I figured documenting it could help someone who is new and looking for a way to practice hardening, scanning, testing, exploiting, etc. 

Step by step: 

VMware Active Directory 
LAB	
How to set up a virtual Active Directory environment using VMware Workstation 
By Gabe Silva	
 
Purpose
The focus of this lab is to provide an overview of how to set up a basic virtual active directory environment using VMware Workstation Pro and evaluation editions of Server 2019 and Windows 10. This will require a basic understanding of virtual computing, networking, and active directory. I aim to provide easily digestible definitions and instructions applicable to this lab and the skill level needed to utilize a virtual active directory environment. In information technology and information security, research is your best friend. Your ability to problem solve and think critically is essential to your success. 

“Research means that you don’t know, but are willing to find out”- Charles F. Kettering
“No research without action, no action without research”-Kurt Lewin

Benefits of a Test Environment
If you are new to IT, networking, and/or cybersecurity, setting up a lab will help you gain hands on experience and provide an opportunity to share your knowledge. The importance of networking cannot be overstated. Being able to take your community with you on your journey will help potential employers get a look into your experience. Showcasing your ability to work in an active directory environment, configure networks, and perform security operations can significantly improve your chances of landing the opportunity you have been searching for. After completing this lab, you can use it to start new labs!
Need some ideas? 
•	Perform reconnaissance using network mapper (Nmap) a network scanning tool to identify IP addresses, ports, services, and systems.
•	Capture and analyze network traffic using Wireshark to learn about networking protocols and threat hunting.
•	Practice scripting and automation.
•	Set up a Firewall, Security Information and Event Management (SIEM) solution, or Intrusion Detection/Prevention System (IDS or IPS) to detect, analyze, and respond to security threats.
•	Perform a vulnerability scan using OpenVAS, Nessus, Qualys, etc.
•	Practice exploits- LLMNR poisoning, SMB or NTLM relay, Kerberoasting, etc. 
•	Analyze the network with Bloodhound.
There are many use cases for this environment. Be sure to take a snapshot within VMware Workstation Pro to preserve your evaluation edition operating systems before first booting so you can reset quickly after the trial expires. These are 90 day evaluation versions. 
 	
Lab Overview
1.	We will download hypervisor software (VMware) and create:
•	2 virtual machines (VMs)
•	1 LAN Segment for the VMs to communicate (You can use bridged or NAT connections but that is not part of this lab environment)
2.	We will download 2 Microsoft evaluation edition operating system ISO files:
•	Server 2019 (You can use Server 2022)
•	Windows 10 (You can use Windows 11)
3.	We will create a new forest and LAB.local domain
4.	We will promote the Server 2019 VM to a Domain Controller, DHCP and DNS server. 
5.	We will rename and join the Windows 10 VM to the LAB domain.
6.	We will create an Active Directory User
Virtualization
Virtualization is the concept of running a resource like a windows machine or a server without needing an actual physical machine or server. These virtual machines are run on a piece of software, a hypervisor, VMware Workstation Pro. This is the paid version, but this lab can also be completed using the free version. You download and install VMware Workstation Player for FREE here: https://www.VMware.com/go/downloadplayer The “PRO” version used in this lab is a paid version and has more capabilities. 
Microsoft Evaluation ISO Files
Microsoft offers evaluation operating systems. You will need these to deploy your virtual machines. You can get the newest versions here: Microsoft Evaluation Center. Once you have these files downloaded it’s time to create the Virtual Machines (VMs). 

Create Virtual Machines (VMs)
1.	Open your VMware software and click on create machine.  
2.	Click on next and browse to where you stored your ISO files: 
3.	You may be prompted to enter a key; you don’t need a key. Click “next” and continue to the next two screens where you will name your VMs, and configure the storage space: 
4.	In the next screen below, we can configure the RAM space and create a LAN Segment by clicking on “Customize Hardware” (I recommend 8 GB RAM and at least 32GB storage): 
5.	Click on “Network Adapter” on the left box and set up your LAN Segment by adding and naming it. I chose to name mine “LAN Group” and you will make sure both VMs are added to this group:
 
Boot up and Set Up: Once you have the VMs configured boot up your Server 2019. Walk through the set up process where you will be prompted to enter a name and password. Make sure to choose the “desktop experience.”
 
CONGRATULATIONS! 
You have created your VMs and are ready to configure networking, roles, and services. 

You are ready to create your forest and local domain in the process of promoting your Server 2019 VM to a domain controller, DHCP and DNS server. Then, we will add the first user to active directory. 
But first, let’s talk about network typology. This will give you an idea of what we are building. 

Typology
The following network diagram shows the lab environment consisting of:
•	Physical host computer on its own private network
•	The hypervisor software (VMware Workstation Pro) configured with a LAN Segment for the 2 VMs
•	Virtual Machine: Windows Server 2019 - the Domain Controller
•	Virtual Machine: Windows 10 - joined to the LAB domain (LAB.local)

Set Static IP on Server
The first thing I did was name my Server and set a static IP address. You will need to navigate to control panel>system and security> System> click on “change settings” on the right to rename your Server. To set a static IP address configure your Server’s network adapter IPv4 properties. For this lab, the server IP address is 192.168.0.1/24 which means the subnet mask is 255.255.255.0. The DNS server is the DC’s IP address-192.168.0.1. 
 
 
Promote Server to Domain Controller
From the server manager: 
1.	Add Roles and Features
2.	Select Role Based or Feature Based Installation
3.	Select your Server. 
4.	Select Active Directory Domain Services and select “Next.”
5.	Select “Add Features” confirm and install. It will begin installing the selected features. You will see a progress bar. 
6.	You will notice in the top right of the server manager there is a notification flag for post-deployment configuration tasks. Click on the flag and select “promote this server to a domain controller.”
7.	The Deployment Configuration will start and you will create a new forest and domain (I named mine LAB.local). 
8.	Next in Domain Controller Options -create your password.
9.	You will have a prerequisite check to make sure everything is ok. 


If you have not set a password for the local Administrator account you will have an error and will need to set this password before you can continue. To do that, from the server manager>tools>computer management>local users and groups>users

Take care of the DNS task as well. This will require you to enter the Administrator credentials. 

Now we are ready to join the Windows 10 machine to the domain. 

Join Windows 10 VM to Domain
Similar to changing the Server’s name, but this time you will rename your pc and join it to a domain by clicking “change.”

You will be prompted for the domain administrator password and it will require a computer restart. 	
 
Add Active Directory User
Now that you have configured your domain controller, dns and dhcp servers, and joined your Windows 10 VM to the domain. You will need a domain user to sign in. 
1.	On the Domain Controller select windows administrators and tools> active directory users and computers and find the users folder.  
2.	Select create user icon.
3.	Create a user and logon to the WIN10 VM


Lab Complete
Your active directory environment is ready to use. Keep in mind active directory is still prevalent but with cloud adoption it may be beneficial to explore Azure AD Connect and Azure AD Sync to learn about hybrid environments. Document and take your community on your journey!

Thank You for taking the time to read the document and/or complete the lab. I hope this helps you and I wish you the best in pursuit of success!
