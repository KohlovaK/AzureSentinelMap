<h1>Azure Sentinel Map with Live Attacks(SIEM)</h1>

<h2>Description</h2>
I set up cloud based SIEM (Azure Sentinel) and connected it to a live virtual machine acting as a honey pot. Then, I was monitoring live attacks (Remote Desktop Protocol Brute Force) from all the world through logs (which collect information such are IP adresses, countries, credentials which attackers tried to use for log in etc.). I used a custom PowerShell script to see geolocation information of the attackers, and plotted it on the Azure Sentinel Map. As the data is displayed on a map, it is easy to see where these attacks are coming from. 
<br />
In this project I did not only learned how to set this up, but it is also a strong reminder, that anything published on the Internet can be vulnerable and the attackers will try to take an advantage of it, no matter who you are, you will always be a target. The other reminder is to use strong credentials, strong passwords and obviously not to use "admin" as a username as the most of attackers tried to use this one. It is also crucial to setup firewalls properly, so they are not opened to everything coming from the Internet.
<br />

<h2>Tutorial from:</h2>
For this project I followed the SIEM tutorial from Josh Madakor, the whole tutorial is available on his Youtube channel: 
<a href="https://www.youtube.com/watch?v=RoZeVbbZ0o0&ab_channel=JoshMadakor-Tech%2CEducation%2CCareer">SIEM Tutorial</a>

<h2>Languages and Utilities Used</h2>

- <b>Miscrosoft Azure (Virtual Machines, Log Analytics, Sentinel)</b>
- <b>PowerShell</b> 
- <b>KQL</b> 

<h2>Environments Used </h2>

- <b>Windows 10</b>

<h2>Project walk-through:</h2>

<b>1. Creating Virtual Machine (Exposed):</b> <br/>
First, I created an exposed virtual machine (VM), which is supossed to be discovered easily on the Internet, so people from the other countries can try to attack it. Virtual machine is named "Honeypot1".

<img src="https://snipboard.io/ARryaO.jpg" height="80%" width="80%" alt="VM"/>

Then, instead of using a default firewall, I created the new one, which is opened to everything coming from the Internet to my virtual machine.

<img src="https://snipboard.io/Q3fNRw.jpg" height="80%" width="80%" alt="firewall"/>

<b>2. Creating Log Analytics Workspace:</b>  <br/>
I created and connected logs with my virtual machine. The goal of these logs is to monitor from which countries are the attackers coming. Later, I will connect the logs with Sentinel to display locations of the attacks. Log Analytics Workspace is named "lawHoneypot1".

<img src="https://snipboard.io/szok1j.jpg" height="80%" width="80%" alt="log"/>

Enabling to gather logs from my Virtual Machine into Logs Analytics Workspace:

<img src="https://snipboard.io/Qme3nM.jpg" height="80%" width="80%" alt="gather_logs"/>

In Data Collection I chose the option to collect all events:

<img src="https://snipboard.io/CgT2yA.jpg" height="80%" width="80%" alt="collect_all"/>

Connecting Log Analytics Workspace with Virtual Machine:

<img src="https://snipboard.io/24rEdb.jpg" height="80%" width="80%" alt="connection"/>

<b>3. Azure Sentinel Setup:</b>  <br/>
I created Azure Sentinel and added it to my Log Analytics Workspace - "lawHoneypot1".

<img src="https://snipboard.io/fodaKW.jpg" height="80%" width="80%" alt="sentinel"/>

<b>4. Logging into VM with Remote Desktop:</b>  <br/>
I copied Public IP address of VM which I will use to log in via Remote Desktop.

<img src="https://snipboard.io/MZouJe.jpg" height="80%" width="80%" alt="copy_IP"/>

Then I pasted this IP address into Remote Desktop Connection and logged into VM.

<img src="https://snipboard.io/TzvR4S.jpg" height="80%" width="80%" alt="remote"/>

On Event Viewer on VM we can already see some attacks (Audit Failure with Event ID 4625) happening after some time.

<img src="https://snipboard.io/apPlLE.jpg" height="80%" width="80%" alt="event_log"/>

Example of Log Failure:

<img src="https://snipboard.io/o0pBiG.jpg" height="80%" width="80%" alt="event_log_detail"/>

Here we can see which Account the attacker was trying to use, why log in process failed and also the address from the attacker. However, it is not visible here, from which country the attacker is, and the other information.

<b>5. Using ipgeolocation.io:</b>  <br/>
For finding out more specific information about country, latitude, longitude etc. of the attacker I used ipgeolocation.io. I just pasted the IP address of the attacker and could see this:

<img src="https://snipboard.io/qC237l.jpg" height="80%" width="80%" alt="ipgeolocation_show"/>

For the next steps, it is necessary to get geolocation API key (to get geo data of the attackers) - after signing up into ipgeolocation.io, I got my API key.

<b>6. Turnning off Windows Firewall on VM:</b>  <br/>
To make VM more discoverable on the Internet, I turned off Windows firewall - on all Domain, Private and Public profile.

<img src="https://snipboard.io/1yUhjT.jpg" height="80%" width="80%" alt="turningOff_firewall"/>

<b>7. Downloading Powershell Script:</b>  <br/>
I copied the Powershell script from <a href="https://github.com/joshmadakor1/Sentinel-Lab/blob/main/Custom_Security_Log_Exporter.ps1">Github link</a> from Josh Madakor (the creator of the tutorial) and pasted it into Powershell ISE on my VM. I pasted my API key (which I mentioned above) into the script and then run it. The script looks through Event Viewer, grabs all the events which falied to log in (Event ID 4625), takes their IP addresses, sends them to ipgeolocation.io, gets geo data and creates the new log file, named: "failed_rdp.log", where we can see the data. When script runs we can see the "purple" outputs, which represent failed events.

<img src="https://snipboard.io/PYBRkF.jpg" height="80%" width="80%" alt="powershell"/>

<b>8. Creating Custom Log in Log Analytics Workspace</b>  <br/>
In "lawHoneypot1" (Log Analytics Workspace) I went to "Tables" and created a new custom log. To create it, I added "failed_rdp.log" log file. That will be used for training Log Analytics, how to get geo data from the file.

<img src="https://snipboard.io/zdOCMr.jpg" height="80%" width="80%" alt="custom_log"/>

Then we can see the logs in "Log" section, after I run the query with the name of custom log.

<img src="https://snipboard.io/MXjeRE.jpg" height="80%" width="80%" alt="run_query"/>

The "RawData" column contains the needed information. Therefore, I extracted information like country, state etc. from it. As the tutorial from Josh was created 2 years ago, and Micsrosoft Azure no loger supports custom fields feature, I came up with the following solution all by myself: I wrote the query in KQL (Kusto Query Language) to separate data included in "RawData" column and created more separated columns, which will be used for a visualization.

<img src="https://snipboard.io/YQRupo.jpg" height="80%" width="80%" alt="query"/>

After running this query we can see the new columns:

<img src="https://snipboard.io/yXgNTu.jpg" height="80%" width="80%" alt="custom_log_table"/>

<b>9. Setting up map in Sentinel</b>  <br/>
I headed to Miscrosoft Sentinel. I went to "Workbook" and created the new one. I removed the default ones and clicked on "Add query". I pasted my script, I created and checked in Log Analytics Workspace before. Then I added another script, selecting only the columns I want to see on the map - these are Sourcehost, Latitude, Longitude, Country, Label and Destinationhost. I did not include destionationhost which are "samplehost", as they are just samples and empty sourcehosts. Event_count counts how many times this event happened.

<img src="https://snipboard.io/5fvqlc.jpg" height="80%" width="80%" alt="all_query"/>

Then as a "Visualization" I chose map. For plotting the map I used "Latitude/Longitude" as it shows specific location of IP address. For logical visualization I set "event_count" as "Size by" - so a country with more attacks will have a bigger bubble. Colors are also based on "event_count". I also set "Metric Label" by "Label" and "Metric Value" by "event_count" so we can see numbers of attacks, connected with countries, under the map.
In the end, I saved the map so I can refresh it later when more attacks will occur. However, we can already see various attacks from all the world, the most of them are currently coming from Russia.

<img src="https://snipboard.io/anCbm7.jpg" height="80%" width="80%" alt="map"/>
<br />
<br />
