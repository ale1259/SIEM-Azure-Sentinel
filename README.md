<img src="https://i.imgur.com/RKFcAdV.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>


# SIEM-Azure-Sentinel

In this project I setup Azure Sentinel, a SIEM(Security Information and Event Manager) and connect it to a VM(Virtual Machine) acting as a honeypot. We will observe RDP(Remote Desktop Protocol) Brute Force attacks from all around the world. Using a PowerShell Script we will also look up for the attackers Geolocation information and plot it on Azure Sentinel.

<h2>Environments and Technologies Used</h2>

- Remote Desktop Protocol(RDP)

- Azure Sentinel 

- PowerShell ISE

- Geolocation.io API Key

<h2>Operating Systems Used</h2>

- Windows 10

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

<img src="https://i.imgur.com/C7IifSE.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

 -Click on connect to connect both
 
<img src="https://i.imgur.com/g9DAWp0.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
 
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

 
 -And copy the code from this link in the blank https://github.com/ale1259/SIEM-Azure-Sentinel/blob/main/Custom%20Security%20Log%20Exporter.ps1

<img src="https://i.imgur.com/eTERvbN.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>

 -To continue we need to obtain a new API KEY number that is unique 

 <img src="https://i.imgur.com/3OidBve.png" height="90%" width="90%" alt="Disk Sanitization Steps"/>

 
 -To do this go to https://ipgeolocation.io/. This is a free app that we will use for our project. Click on the three bars, sign up, enter all the information: username, password and email address necessary  

<img src="https://i.imgur.com/ITkgPyW.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>

 -You will get your unique API KEY on the Dashboard section after you sign up

<img src="https://i.imgur.com/LYbTdb3.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>

 -Once you have your new key replace it and run the command

<img src="https://i.imgur.com/GTRWs7Z.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>

 -I suggest that you save this script to do that click Save script so you can use anytime 

<img src="https://i.imgur.com/xR9eJMX.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>

 -After you run the command this will create a text file on C:\ProgramData\failed_rdp.log that will gather all the logs that failed, pull out the IP address of the attacker and put it on the map with Free IP Geolocation API Lookup Database 
 
 -As a test try to login into the VM again and type a wrong password. The log will show up on Powershell ISE and any attempt to login into the VM from anyhwere in the world

<img src="https://i.imgur.com/w4SVavy.png" height="90%" width="90%" alt="Disk Sanitization Steps"/>

 -Now we need to create a custom log in our Log Analytics Workspace that will allows us to bring all those custom logs with the geodate into our Log Analytics Workspace, so go to Log Analytics Workspace, select the one you created and on the left column go to Tables -> Create -> New custom log(MMA based). This step is to train Log Analytics Workspace what to look for in the log file

<img src="https://i.imgur.com/HRWCjOu.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>

 -The file we need is the file from our VM that is collecting the logs that is C:\ProgramData\failed_rdp.log. When creating the custom log you need to copy the failed_rdp.log. file into your actual computer and select that file, then click Next skipping the Recorder delimiter section and on Collecting paths select type Windows and C:\ProgramData\failed_rdp.log. is the path on the VM, on Details I name the custom log FAILED_RDP_WITH_GEO you can name it whatever you like, click next and Review and create. 

 -This will take a while for the Log Analytics Workspace and th VM to sync up and bring the logs into Log Analytics Workspace. To see this got to your Log Analytics Workspace and on the left column select Logs. Type your custom log name, FAILED_RDP_WITH_GEO is my case.Probably will say "No results found from the last 24 hours' as in the picture, but you can see later the custom logs entries there. 
 

<img src="https://i.imgur.com/HaELH4N.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

 

 -Let's try it with the Securityevents. As you could see the logs showed up and those are the logs collected by the Event Viewer on the VM. In case nothing show up don't panic it may take a while to sync up as I said

<img src="https://i.imgur.com/M5x0lhl.png" height="90%" width="90%" alt="Disk Sanitization Steps"/>

 -You can try for example on the query type Securityevent | where EventID == 4625 and will show you this will return all the failed rdp logs. 

<img src="https://i.imgur.com/0xCNt90.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>

 -If you expand the logs you will see information like the account failed to logon, the IP address, the username that try to login and so forth. So may I suggest that you wait 10-15 minutes to try again with the failed rdp logs

<img src="https://i.imgur.com/z2PDtRg.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>

 -Now that the logs are visible there is a column on the entries that is called RawData we need to extract certain fields from it and have separeted into differents fields like latitude, longitude,counrty, etc. To do this copy the content this script https://github.com/ale1259/SIEM-Azure-Sentinel/blob/main/query_log and paste it on the query. You should be able to see the columns divided like this. 

<img src="https://i.imgur.com/5vQwNcV.png" height="90%" width="90%" alt="Disk Sanitization Steps"/>

 -Now that we know the logs are coming we need just need to wait for the bad people on the internet to discover the VM, but in the meantime we can finish setting up our map in sentinel. To do that I suggest that you open a new tab on the browser and go to the Azure Portal. Go to the search bar and type sentinel. Click on your previous created Log Analytics Workspace

<img src="https://i.imgur.com/eQGKewX.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

 -Unchecked New Overview

<img src="https://i.imgur.com/5Rozz1S.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>

 -It will change to this window that is like a general dashboard of Microsoft Sentinel. 

 <img src="https://i.imgur.com/NnJz71m.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>

  -For the next step click on "Workbooks" on the left column

<img src="https://i.imgur.com/DTh53T6.png" height="30%" width="30%" alt="Disk Sanitization Steps"/>

 -And we will Add a New Workbook 
 
<img src="https://i.imgur.com/IPh7paf.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>

 -On the new window click on Edit 
 
<img src="https://i.imgur.com/QZ0e1rB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

  -We are going to remove those two widgets that come with the workbook
 
<img src="https://i.imgur.com/Twb35Ej.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>

<img src="https://i.imgur.com/Wdop1fj.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>

  -And it should look like this and we need to Add a query

<img src="https://i.imgur.com/tz9yjz3.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>

  -On the query blank copy the same query that we use to see the logs 

<img src="https://i.imgur.com/ztqrtgb.png" height="90%" width="90%" alt="Disk Sanitization Steps"/>

 -But on the visualization setting we'll select by Map

<img src="https://i.imgur.com/HL6kZPE.png" height="90%" width="90%" alt="Disk Sanitization Steps"/>

 -Now the map will show up and mine already have my failed attempts to login located on it 

<img src="https://i.imgur.com/mPgNJod.png" height="90%" width="90%" alt="Disk Sanitization Steps"/>

 -You can also see on the right settings you can filter for example on "Metric Settings" you can select "Metric Label" adn choose "label", click on Apply 

<img src="https://i.imgur.com/Uy8haH1.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>

 -And you will see on the bottom left corner of the map is showing up the country and the IP address

<img src="https://i.imgur.com/jw6wIZy.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>

 -Or by setting the Metric Value you can see that we have 6 failed logon from the United States from that IP address

<img src="https://i.imgur.com/OD5aC7B.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>

 -Next "Save and Close" the map settings

<img src="https://i.imgur.com/XO2oR8Z.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>

 -And save the map. You can choose a name like "Failed RDP World Map", your Resource Group and same region of your VM and Save it adn Done Editing. 
<img src="https://i.imgur.com/y6P27Zt.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>

 -Something important to have in mind the VM with the Powershell script running still need to be running to allow the map to collect the data if not will only be gather by Windows Events logs and not to our custom log which is what we need for the extraction of the geodata.

 -The green spot were my failed attempts to login into the VM before. Now we just need to wait for the attackers and the Map to locate the attacks. Leave the map open and wait for it, you can set an Auto refresh or do it manually
 
<img src="https://i.imgur.com/4oar0ub.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

 -Someone from Canada already tried to login into my VM as you can see let's wait to see if it shows up on the map this may take a couple hours so don't desperate this is for educational purposes

 <img src="https://i.imgur.com/XK8SLmQ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

  -A couple hours later we can see the attacks from Canada on the map as well as the IP address. A total of 89 failed attempts to login into the VM and two extra form my computer to text connectivity 

<img src="https://i.imgur.com/THxuw8s.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  
 



