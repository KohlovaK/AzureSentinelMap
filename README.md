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

<p align="center">
Creating Virtual Machine (Exposed): <br/>
<img src="https://snipboard.io/ARryaO.jpg" height="80%" width="80%" alt="VM"/>
<img src="https://snipboard.io/Q3fNRw.jpg" height="80%" width="80%" alt="ports"/>
<br />
<br />
Creating Log Analytics Workspace:  <br/>
<br />
<br />
