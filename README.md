# HTB-Campfire-1



<h2>Description</h2>
The HackTheBox Sherlock (Campfire-1) machine simulates a real-world investigation scenario where users must analyze logs and events to uncover malicious activity within a compromised environment. Solving it using Splunk provided a streamlined and efficient approach to threat hunting, leveraging powerful search queries, field extractions, and visualizations to quickly correlate events, identify suspicious behavior, and trace the attacker’s actions across the network. Compared to manual log review or basic command-line tools, Splunk significantly reduced investigation time and improved accuracy through its centralized, intuitive interface and advanced filtering capabilities.

<br />


<h2>Languages and Utilities Used</h2>

- <b>PowerShell</b> 
- <b>Splunk</b>

<h2>Environments Used </h2>

- <b>Windows 10</b> (21H2)


<h2>Program walk-through:</h2>
<br/>
<br/>

<p align="center">
<h3>Part One (Install & setting up VMs:</h3>
Installing Ubuntu: <br/>
We need to take a few steps to set our static IP address for this VM.<br/>
<br />
<img src="https://i.imgur.com/nl6DEQ6.png"/>
<br />
<br />
Finding out the gateway IP of your VMware workstation NAT network:  <br/>
VMware workstation > click Edit menu on top > Click "Virtual Network Editor" > Select Type : "NAT" network > and click "NAT Settings" > make sure to take down the Gateway address IP and Subnet mask <br/>
<br />
<img src="https://i.imgur.com/BwFP3Er.png"/>
<br />
<br />
Now back to Ubuntu Installer we are going to change the interface from DHCPv4 to Manual or static: <br/>
Now got back to Network Connections > drop down to "ens33 eth" and select "edit IPv4" > after that select "Manual" > now a window has appeared and just plug in the required IP from our previous steps <br/>
<br />
<img src="https://i.imgur.com/9JN0v2l.png"/>
<br />
<br />
Continue to Install Ubuntu:  <br/>
Once Static IP has been set > continue to next installer > Make sure to set memorable username/password > Next step "Install OpenSSH server" > then continue isntalling OS until "Install Complete" and hit "reboot now" <br/>
<br />
<img src="https://i.imgur.com/EtZPCjM.png"/>
<br />
<br />
Installation and Reboot complete:  <br/>
After reboot now we make sure DNS and outbound pings are working > make sure to login in with the your credentials > type in "ping -c 2 google.com" > looks like we pinged right now we are all set for our ubuntu server <br/>
<br />
<img src="https://i.imgur.com/0pTReUk.png"/>
