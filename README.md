<h1>Azure Sentinel Map with Live Attacks(SIEM)</h1>

<h2>Description</h2>
I setup cloud based SIEM (Azure Sentinel) and connect it to a live virtual machine acting as a honey pot. Then, I was monitoring live attacks (RDP Brute Force) from all the world through logs (IP adresses, countries, credentials which attackers tried to use for log in). I used a custom PowerShell script to look up the attackers Geolocation information and plotted it on the Azure Sentinel Map. This data is displayed on a map, so it is easy to see where these attacks are coming from.
<br />


<h2>Languages and Utilities Used</h2>

- <b>Miscrosoft Azure (Virtual Machines, Log Analytics, Sentinel)</b>
- <b>PowerShell</b> 

<h2>Environments Used </h2>

- <b>Windows 10</b>

<h2>Project walk-through:</h2>

<b>Creating Virtual Machine (Exposed):</b> <br/>
First, I created exposed virtual machine, which is supossed to be discovered easily on the Internet, so people from other countries can try to attack it. Virtual machine is named "Honeypot1".

<img src="https://snipboard.io/ARryaO.jpg" height="80%" width="80%" alt="VM"/>

Then, instead of using default firewall, I created the new one, which is opened to everything coming from the Internet to my virtual machine.
<br/>

<img src="https://snipboard.io/Q3fNRw.jpg" height="80%" width="80%" alt="firewall"/>
<br />

<b>Creating Log Analytics Workspace:</b>  <br/>
I created and connected logs with my virtual machine. The goal of these logs is to monitor from which countries are the attackers coming. Later I will connect the logs with Sentinel to display locations of the attacks. Log Analytics Workspace is named "lawHoneypot1".

<img src="https://snipboard.io/szok1j.jpg" height="80%" width="80%" alt="log"/>

Enabling ability to gather logs from my Virtual Machine into Logs Analytics Workspace.
<img src="https://snipboard.io/Qme3nM.jpg" height="80%" width="80%" alt="gather_logs"/>
<br />
In Data Collection I chose the option to collect all events.
<img src="https://snipboard.io/CgT2yA.jpg" height="80%" width="80%" alt="collect_all"/>

Connecting Log Analytics Workspace with Virtual Machine.
<img src="https://snipboard.io/24rEdb.jpg" height="80%" width="80%" alt="connection"/>

<b>Azure Sentinel Setup:</b>  <br/>

I created Azure Sentinel and added it to my Log Analytics Workspace - "lawHoneypot1".
<img src="https://snipboard.io/fodaKW.jpg" height="80%" width="80%" alt="sentinel"/>

<b>Logging into VM with Remote Desktop:</b>  <br/>

I coppied Public IP address of VM which I will use for log in via Remote Desktop.
<img src="https://snipboard.io/MZouJe.jpg" height="80%" width="80%" alt="copy_IP"/>

Then I paste this IP address into Remote Desktop Connection and logged into VM.
<img src="https://snipboard.io/TzvR4S.jpg" height="80%" width="80%" alt="remote"/>
<br />
<br />
