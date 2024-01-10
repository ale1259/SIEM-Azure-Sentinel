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


 - Craeting Resources in Azure:

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

  -Then +Add an inbound rule and on the Destination port range type a *(for any port), Priority 100 and the name could be anything
   <img src="https://i.imgur.com/Sf7YXNQ.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>




<img src="https://i.imgur.com/Sf7YXNQ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
