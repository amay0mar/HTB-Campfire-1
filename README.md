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
<h3>Q1: Analyzing Domain Controller Security Logs, can you confirm the date & time when the kerberoasting activity occurred?
<br/>
</h1>Based on the question we know it has something to do with Kerberos. Now let us see if there is any event code associated with Kerberos. <br/>
Upon looking we found that event code 4769 pertains to Kerberos service ticket was requested, we can use that now on our index.<br/>
Now we can go and add "EventCode=4769" by clicking "event code" and then "4769" located on the left hand side of the result window. <br/>
Inspecting the first on the list we can see the highlighted on the message section saying "kerberos ticket was requested.<br/>
We can add a new query pertaining to Service name as shown in the screenshot. Once outpur has been returned we can inspect the first result. <br/>
Inspecting the event, I can see that the Ticket encryption type is "0x17" which is a weak encryption. We can safely say that this is a potential for a kerberos ticket attack and we can try and proceed to answer the question using the time found in this event. </br>
<br />
<img src="https://i.imgur.com/l2eAMZy.png"/>
<br />
<img src="https://i.imgur.com/Hf8m1JB.png"/>
<br />
<br />
<h3>Q2: What is the Service Name that was targeted?</h3>  <br/>
</h1>For this one we can just go back to the event and look through the "service name" <br/>
<br />
<img src="https://i.imgur.com/jNCUwk9.png"/>
<br />
<br />
<h3>Q3:It is really important to identify the Workstation from which this activity occurred. What is the IP Address of the workstation?</h3> <br/>
</h1>Now this one is a quick one too. We go back inspect the event output again and look for the "Client Address" section. <br/>
<br />
<img src="https://i.imgur.com/Zu5Vwyj.png"/>
<br />
<br />
<h3>Q4:What is the name of the file used to Enumerate Active directory objects and possibly find Kerberoastable accounts in the network? <br/>
</h1>Now that we have identified the workstation, a triage including PowerShell logs and Prefetch files are provided to you for some deeper insights so we can understand how this activity occurred on the endpoint. For this one we have to step back and go back to query some data. Since this question points to the Powershell logs and prefetch files, we can go back to our original query and delete "event code" and "source name". Now on the left hand side panel we can click "source type" and click through the one that says "powershel-operational.evtx" I think the next best step is to filter for any execution commands. When you look into powershell execution event code "4104" shows up. So we can query that also. After querying the event code we can go through the top event. Upon inspecting we can see the Path section mentioning about "powerview.ps1" upon investigating PowerView is a powershell to gain network situational awareness on windows domain. So this might be the file we are hunting for the answer. <br/>
<br />
<img src="https://i.imgur.com/A8OtAKP.png"/>
<br />
<br />
<img src="https://i.imgur.com/z0VDwf5.png"/>
<br />
<br />
<h3>Q5:When was this script executed?  <br/>
</h3>For this question we can add "PowerView.ps1" on our query search. For when it was first executed we go the the very last event on our list and enter the time and date. <br/>
<br />
<br />
<h3>Q6:What is the full path of the tool used to perform the actual kerberoasting attack?
</h1>Here we can look into the prefetch files. We can use a prefetch parser called "PECmd.exe" which I downloaded from Eric Zimmerman's tools. Once downloaded we extract the file and then open powershell as administrator and run ".\PECmd.exe" and after that this command ".\PECmd.exe -d C:\Users\ZORO\Downloads\campfire-1\Triage\Workstation\2024-05-21T033012_triage_asset\C\Windows\prefetch --csv . --csvf campfire1-prefetch.csv -q". Once you run the those  commands, as shown in  in the screenshot two new .CSV files are created. Go to these files and open them, I chose to use the Timeline Explorer again from Mr. Eric Zimmerman's tools. When we go back to our splunk query we saw that the script was executed at "2024-05-21 03:16:32" so now we go check our CSV file and look for that timeline, upon inspecting we see that at 2024-05-21 03:18:08 a suspicous file "Rubeus.exe" was run. Upon investigating this is a malicous file. So our answer is the full path as shown in the screenshot. 
<br />
<br />
<img src="https://i.imgur.com/fuTCGkk.png"/>
<br />
<img src="https://i.imgur.com/sW0V9hR.png"/>
<br />
<img src="https://i.imgur.com/QninSSd.png"/>
<br />
<br />
<h3>Q7:When was the tool executed to dump credentials?
</h1>h1>looking again on our screenshot it the time when we saw "rubeus.exe" was executed. 
<br />
<br />
<img src="https://i.imgur.com/FK6omCk.png"/>
<br />
<br /> 
<h3> Thanks for going through my documentation! Please let me know of any improvements.  </h3>
<h1/> 
<br />
<br />

