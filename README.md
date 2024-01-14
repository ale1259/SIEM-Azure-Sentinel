<img src="https://i.imgur.com/RKFcAdV.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>


# SIEM-Azure-Sentinel

In this project I setup Azure Sentinel, a SIEM(Security Information and Event Manager) and connect it to a VM(Virtual Machine) acting as a honeypot. We will observe RDP(Remote Desktop Protocol) Brute Force attacks from all around the world. Using a PowerShell Script we will also look up for the attackers Geolocation information and plot it on Azure Sentinel.

<h2>Environments and Technologies Used</h2>

- Remote Desktop Protocol(RDP)

- Azure Sentinel 

- PowerShell

- Geolocation.io API Key

<h2>Operating Systems Used</h2>

- Windows

<h2>High-Level Deployment</h2> 


 - Creating Resources in Azure:

   -First we need to create our Honeypot VM(Virtual Machine) to do this we go to Virtual Machines->Create

     <img src="https://i.imgur.com/RdpNRVe.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

     <img src="https://i.imgur.com/L4Pw83z.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>

   -We have to create a Resource Group as well, so click on Create New on the Resource Group blank
   
     <img src="https://i.imgur.com/KgRdOQe.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>

   -You will need a Username and a Password to log in to the VM, write it down on a Note or a .txt

   -Now we go to Networking and in the "NIC network security group" click Advanced and under that we need to create a new Network Security Group

   <img src="https://i.imgur.com/THTAXms.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

   -We want the Firewall of the VM to be open to the Internet so when we click on create new on this new windows remove the default inbound rule that was created 
 
   <img src="https://i.imgur.com/vmgjTjK.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

  -Then +Add an inbound rule and on the Destination port range type a *(for any port), Priority 100 and the name could be anything. This setup will allow any traffic from the Internet into our VM making it very discoverable. Click OK->Review and create->Create
  
   <img src="https://i.imgur.com/Sf7YXNQ.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>

   -The creation of the VM will take a while so let's create our Log Analytics Workspace. This is to ingest logs from the Windows Events logs and we are going to create our custom log that contains geographic information to discover where the attackers are coming from. Search for Log Analytics Workspace in Azure Portal and create one

<img src="https://i.imgur.com/U9GmRsm.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

   -Select the previous Resource Group and a name for your Log Analytics Workspace, I chose LAW-Honeypot1. Review and create->Create

   -Next we need to go to Microsoft Defender for Cloud in the Azure Portal to enable the ability to gather logs from the VM into the Log Analytics Workspace. Once you are on Microsoft Defender for Cloud go to Environment settings on the left column and select your Subscription and the Log Analytics Workspace

<img src="https://i.imgur.com/CgZEgl2.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

  -On the defender plans turn on the "Servers" and turn off the "SQL Servers" on machines 
<img src="https://i.imgur.com/Clwo6fH.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

  -Go to the next section "Data Collection" and select "All Events"

  <img src="https://i.imgur.com/9Uae2Ky.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  
  -After that since we need to connect our VM with the Log Analytics Workspace go to the Log Analytics Workspace(LAW-Honeypot1 my case) and on the left column go to Virtual Machines(deprecated) and select your VM

<img src="https://i.imgur.com/C7IifSE.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>

 -Click on connect. As you can see my is already connected
 
<img src="https://i.imgur.com/X4LJyvH.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
 
 -Now we need to create Sentinel. On your Azure Portal go and type sentinel on the search bar and choose Microsoft Sentinel. 

<img src="https://i.imgur.com/cgzPAIl.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>

-Select "+Create"

<img src="https://i.imgur.com/sjJmTw9.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>

-Click on your Log Analytics Workspace and click "Add"

<img src="https://i.imgur.com/TTxKLOs.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

-After successfully added go to your VM and we need the Public IP Address so we can connect to it by Remote Desktop

<img src="https://i.imgur.com/CoqjPiS.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>

-Copy the IP address and go to your search(your actual computer) and search for remote desktop. I will encourage you to fail the first attempt to login so we can see the logs once into teh VM you could type the wrong password 

<img src="https://i.imgur.com/gg2vCXK.png" height="30%" width="30%" alt="Disk Sanitization Steps"/>

-First will tell that the identify computer cannot be verified and if you want to connect anyway select "Yes" 

<img src="https://i.imgur.com/dsrHeUi.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>

and second (once into the VM) will and you about chooosing your privacy setting select "No" for everything and Accept


<img src="https://i.imgur.com/45xt66X.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

-Next into the VM go to "Event Viewer"

<img src="https://i.imgur.com/SSFmS5x.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>

-Once into the Event Viewer click on Windows Logs and Security. This are all the security logs and we will look for the one with the Event ID 4265

<img src="https://i.imgur.com/t7rBwLa.png" height="90%" width="90%" alt="Disk Sanitization Steps"/>

-Once you locate the log click on it to see general information like why it failed, the account that try lo login and even some network information like the IP address of the computer if you scroll down 

<img src="https://i.imgur.com/d31Gr8C.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>

 -Next step configure firewall. On the search bar type wf.msc and select the Miscrosof Common Console Document

<img src="https://i.imgur.com/dKe08TH.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>

 -On the new window go to "Windows Defender Firewall Properties"

<img src="https://i.imgur.com/EX07Dfp.png" height="90%" width="90%" alt="Disk Sanitization Steps"/>

 -Now we need to turn off the Firewall state on the Domain Profile, Private Profile and Public Profile sections. Click Apply and OK

 <img src="https://i.imgur.com/jqNfzym.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>

 -Open Powershell ISE  

<img src="https://i.imgur.com/crEIuv1.png" height="30%" width="30%" alt="Disk Sanitization Steps"/>

 -Click on New Script 

 <img src="https://i.imgur.com/x7nvUjR.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>

 
 -And copy the code from this link https://github.com/ale1259/SIEM-Azure Sentinel/blob/main/Custom%20Security%20Log%20Exporter.ps1

<img src="https://i.imgur.com/eTERvbN.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>


<img src="https://i.imgur.com/x7nvUjR.png" height="90%" width="90%" alt="Disk Sanitization Steps"/>


<img src="https://i.imgur.com/EX07Dfp.png" height="90%" width="90%" alt="Disk Sanitization Steps"/>


<img src="https://i.imgur.com/EX07Dfp.png" height="90%" width="90%" alt="Disk Sanitization Steps"/>


<img src="https://i.imgur.com/EX07Dfp.png" height="90%" width="90%" alt="Disk Sanitization Steps"/>


<img src="https://i.imgur.com/EX07Dfp.png" height="90%" width="90%" alt="Disk Sanitization Steps"/>

<img src="https://i.imgur.com/EX07Dfp.png" height="90%" width="90%" alt="Disk Sanitization Steps"/>

