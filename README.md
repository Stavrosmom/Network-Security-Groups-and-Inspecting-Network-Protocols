<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04



<h2>Actions and Observations</h2>

</p>
<br />
<p>
<img src="https://i.imgur.com/KyCaxYV.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<p>
<img src="https://i.imgur.com/zM7TQv8.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
First, create a resource group in Azure named "NetworkTraffic". Then create a Windows 10 VM and a Linux Ubuntu VM. Ensure that both VM's are made with the same virtual network. You can verify this with Network Watcher Topology.
</p>
<br />
<p>
<img src="https://i.imgur.com/eLXwDmy.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />
<p>
<img src="https://i.imgur.com/I57tunP.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
After, use remote desktop to connect to your Windows 10 VM. Once connected, enter Internet Explorer and search "Google.com". Next, search "Wireshark", select download, then click the windows installer. Go through the default settings and install Wireshark.
</p>
<br />

<p>
<img src="https://i.imgur.com/bmDmMa2.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
After, 
</p>
<br />
 After installed, open the program. 
</p>
<br />
<p>
<img src="https://i.imgur.com/n2wKIEh.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />
<p>
First we will observe ICMP traffic. At the display filter, type "icmp" and open command prompt. Once Command Prompt is open, ping the private IP address of the linux VM. Notice the ICMP traffic appearing on Wireshark. 
</p>
<br />
<p>
<img src="https://i.imgur.com/p592qIO.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />
<p>
Then, ping google.com. Observe the traffic. 
</p>
<br />
<p>
<img src="https://i.imgur.com/03HB8zQ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />
<p>
<img src="https://i.imgur.com/V873QDw.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />
<p>
Next, run a perpetual ping on the linux VM using "ping -t". While the VM is pinging the other, minimize the windows VM and enter the NetworkTraffic resource group. Then select your linux VM -> Networking -> Add inbound port rule. Select the ICMP protocol. Then select Deny and set the priority to 200. Name the port rule "DenyInboundICMP". Allow the security rule to be created and return to the windows VM. What this security rule will do is prevent any inbound ICMP to reach the Linux VM. 
</p>
<br />
<p>
<img src="https://i.imgur.com/RPqjsyb.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />
<p>
Observe the perpetual ping we sent out is now requesting time out. Allow the ping to continue. 
</p>
<br />
<p>
<img src="https://i.imgur.com/cPhrGod.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />
<p>
Return to Azure and delete the security rule we just created. Next, Open back up the Windows VM and observe the ping is now succesfull. Stop the ping with ctrl + c.
</p>
<br />
<p>
