<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Inspecting Network Traffic Between Azure Virtual Machines</h1>
In this lab, I explored and analyzed various types of network traffic between Azure virtual machines using Wireshark, a protocol analyzer, and experimented with network security groups to understand their impact on traffic flow.



</p>
<br />

<h2>Technologies and Tools Used:</h2>

- Cloud Environment: Microsoft Azure (Virtual Machines/Compute)
- Remote Access: Remote Desktop Connection
- Command-Line Utilities: PowerShell, SSH, nslookup, ipconfig
- Network Protocols: ICMP, SSH, DNS, HTTP/S, DHCP, RDP
- Protocol Analysis: Wireshark



</p>
<br />

<h2>Operating Systems Used </h2>

- Windows 10 Pro (21H2)
- Ubuntu Server 20.04



</p>
<br />

<h2>Setup</h2>

Two virtual machines (VMs) were created within the same virtual network on Azure—one running Windows 10 Pro and the other Ubuntu Server 20.04. The VMs were configured to communicate with each other, with the Windows VM connecting to the Ubuntu VM via the command line and PowerShell.



</p>
<br />

<h2>Procedure and Observations:</h2>
Using Remote Desktop Connection, I accessed the Windows Virtual Machine (VM) via its public IP address. I then installed Wireshark to begin traffic inspection.
In Wireshark, I applied a filter for ICMP (Internet Control Message Protocol) traffic and utilized PowerShell to execute a ping command. The ping command, which leverages ICMP, is used by network devices to communicate issues during data transmission. I conducted ping tests to verify connectivity between the Windows VM and the Ubuntu VM using its private IP address, as well as to google.com. Additionally, I ran a continuous ping to the Ubuntu VM using the command ping -t <IP address> to monitor how network security groups affect traffic.
<br />

![image](https://github.com/user-attachments/assets/36bf40b2-8bf8-4475-ab24-a661fa4b715e)

</p>
</p>
</p>
<br />

In the Azure portal, I accessed the networking settings of the Ubuntu VM and created an inbound security rule to block ICMP traffic. I ensured that this rule had a higher priority (lower number) than the SSH rule (priority 300) to guarantee its precedence. 
![image](https://github.com/user-attachments/assets/deb89e6e-7146-4fbf-90b8-0f476159da07)
</p>
</p>
</p>
<br />
Returning to the Windows VM, I observed that ICMP traffic was effectively blocked by the new security rule. After modifying the rule to permit ICMP traffic, the continuous ping resumed without any timeouts.

![image](https://github.com/user-attachments/assets/e0142868-e769-406c-9f31-0423f4a9b97c)
</p>
</p>
<br />
Next, I analyzed SSH traffic by logging into the Ubuntu server via PowerShell using the ssh command. In Wireshark, I filtered traffic using tcp.port == 22. Each command executed during the SSH session was captured and logged in Wireshark.
</p>
Following the SSH traffic analysis, I filtered for DHCP traffic. To generate relevant data, I attempted to renew the VM's IP address using the command ipconfig /renew. This temporarily disconnected the session for a few seconds, and the resulting DHCP traffic was captured in Wireshark upon reconnection.

![image](https://github.com/user-attachments/assets/d80f2534-80c0-47aa-bd4f-d5a7c76ef248)
![image](https://github.com/user-attachments/assets/8b3b030d-90e8-49b7-bc82-17eda120a044)

</p>
</p>
<br />
To observe DNS traffic, I used the filter udp.port == 53 and the command nslookup. I wanted to see the results that are from looking up google.com and disney.com, two very popular sites. 

![image](https://github.com/user-attachments/assets/111fcc2f-8c07-40ff-ab0c-326cab3ae73a)

</p>
</p>
<br />

Finally, I examined RDP (Remote Desktop Protocol) traffic by applying a Wireshark filter for tcp.port == 3389. Continuous traffic was observed as RDP constantly streams a live session from one computer to another—in this case, from my local machine to the Azure-hosted VM.
![image](https://github.com/user-attachments/assets/457af29a-a89e-442b-a630-a67950a695fc)



</p>
<br />

<h2>Key Learnings</h2>

This lab provided valuable insights into how various network protocols and ports are used for communication between devices in a networked environment. Although the lab's primary focus was on observing traffic rather than troubleshooting, it emphasized the importance of understanding network traffic flows and the use of tools like Wireshark and command-line utilities for effective network analysis. Familiarity with these tools and a curious approach to exploring network behaviors are crucial for successful network management and troubleshooting.
