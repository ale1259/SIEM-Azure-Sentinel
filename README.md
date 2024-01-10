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

   -You will need  a Username and a Password to login write it down on a Note or a .txt
 
   



