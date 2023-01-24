<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDP, DNS, ICMP)
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
</p>
<br />
 Once installed, open the program. 
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
Next, run a perpetual ping on the linux VM using "ping -t". While the VM is pinging the other, minimize the windows VM and enter the NetworkTraffic resource group. Then select your linux VM -> Networking -> Add inbound port rule. Select the ICMP protocol. Then select Deny and set the priority to 200. Name the port rule "DenyInboundICMP". Allow the security rule to be created and return to the windows VM. This security rule will prevent any inbound ICMP from reaching the Linux VM. 
</p>
<br />
<p>
<img src="https://i.imgur.com/RPqjsyb.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />
<p>
Observe that the perpetual ping we sent out is now requesting time out. Allow the ping to continue. 
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
 <img src="https://i.imgur.com/cYbyvEb.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
 </p>
<br />
<p>
 <img src="https://i.imgur.com/LAUMTRz.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  </p>
<br />
<p>
 Next we will observe SSH traffic. in the filter bar, type in "ssh" and open a Command Prompt. In the Command Prompt connect to the Linux VM using SSH. To do this type in: ssh username@PrivateIP. For reference, mine was "ssh VM2@10.0.0.5". Next input your password and hit enter. Type a few Linux commands and oberve the traffic. (one commmand you can use is dig) To exit SSH, simply type "exit".
 </p>
<br />
<p>
 <img src="https://i.imgur.com/Pg5uNPk.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
 </p>
<br />
<p>
 Next, we will observe DHCP traffic. In the filter bar type "dhcp" and open Command Prompt.
 </p>
<br />
<p>
 <img src="https://i.imgur.com/PGrE6a0.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
 </p>
<br />
<p>
 In Command Prompt type "ipconfig /renew". This will send DHCP traffic by renewing your IP address.
 </p>
<br />
<p>
 <img src="https://i.imgur.com/cZt89kn.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
 </p>
<br />
<p>
 Next we will observe DNS traffic. In the filter bar type in "dns" and open Command Prompt.
 </p>
<br />
<p>
 <img src="https://i.imgur.com/qk2NQHm.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
 </p>
<br />
<p>
 In Command Prompt, type in "nslookup google.com". This will send DNS traffic by converting google.com to an IP address.
 </p>
<br />
<p>
 <img src="https://i.imgur.com/pvvhsTp.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
 </p>
<br />
<p>
 Lastly, we will observe RDP traffic. In the filter bar, type "tcp.port == 3389". Notice traffic appears instantly. Since we are logged into the VM using Remote Desktop, RDP traffic is continuosly being sent.
 </p>
<br />
<p>
 <img src="https://i.imgur.com/n6RJGKh.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
 </p>
<br />
<p>
 Congrats! You've made it to the end of the lab. You now know how to install and observe different protocols using Wireshark!
 </p>
<br />
<p>
