<p align="center">
<img src="https://i.imgur.com/WoBoDqf.jpeg" alt="DNS Photo"/>
</p>

<h1>Understanding DNS</h1>
Domain Name System (DNS), is an important aspect of IT and networking. It essentially functions like a phone book for the internet, translating human-readable domain names (like www.google.com) into machine-readable IP addresses (such as 8.8.8.8) that computers use to identify each other on the network. DNS is essential for ensuring that we can easily navigate the internet without needing to memorize IP addresses for each website we visit. It’s a foundational part of internet infrastructure.
<p>In the steps below we will obsure how DNS is used to look up ip address information from within the network</p>
<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- Command Prompt

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 Pro (21H2)

<h2>DNS Configuration Steps</h2>


<p>
After logging into both the client Virtual Machine (VM) and the Domain Controller (DC) as admins we can begin running some commands in Command Prompt.
</p>
<p>First thing we will do is search for "System-Security" using command "ping System-Security" in Command Prompt. We then observe that that it will give us the error message: "Ping request could not find host system. Please check the name and try again.". Now will try using the command "Nslookup System-Security" to try and locate the ip address. Again it will fail and display an error message</p>
<p>The following images display what we did in these steps:</p>
<p>
<img src="https://i.imgur.com/irWQdSx.png" height="70%" width="70%" alt="DNS Steps"/>
<img src="https://i.imgur.com/eST1R6g.png" height="70%" width="70%" alt="DNS Steps"/>
</p>
<p>
We will create an DNS A-Record for "System-Security" on the DC. On the DC, open the DNS Manager and go to the Forward Lookup Zones tab, and select the domain we are working within (mine is mydomain.com) Right click on the page and create a New Host. The name, I'll make it "System-Security" and the IP address should be the same IP as the DC so that the ping can resolve. Refresh the DNS server, this will allow for the new record to show and be updated. Returning to the client VM use command  "ping system-security" to see that it will now resolve.
</p>
<p>
<img src="https://i.imgur.com/fNAfESH.png" height="70%" width="70%" alt="DNS Steps"/>
</p>
<br />

<p>
<img src="https://i.imgur.com/WqoEpN6.png" height="80%" width="80%" alt="DNS Steps"/>
<img src="https://i.imgur.com/qNkK6WB.png" height="80%" width="80%" alt="DNS Steps"/>
<img src="https://i.imgur.com/YOsrXe0.png" height="80%" width="80%" alt="DNS Steps"/>
</p>
<p>
Next we will look at DNS cache and see how we can influance it. With the DC, I changed "System-Security"'s record address to 8.8.8.8 (same as Google) and refreshed the DNS server. When we use the command "ping system-secrutiy" on the VM, it will still ping the old IP address. When the command "ipconfig /displaydns" is use, we will see that the DNS cache still has the old IP address.
</p>
<img src="https://i.imgur.com/geTpvTo.png" height="80%" width="80%" alt="DNS Steps"/>
<img src="https://i.imgur.com/kZZ8MML.png" height="80%" width="80%" alt="DNS Steps"/>
<p>
To update the cache, we use the command "ip config /flushdns". This willclear the cache. Now when we try to ping "system-security the IP address will be updated to the new one.
</p>
<img src="https://i.imgur.com/Q81wzvq.png" height="80%" width="80%" alt="DNS Steps"/>
<br />


<p>
A CNAME record will now be made on the DNS server that will point "search" to Google. On the Forward Lookup Zones tab in the DNS Manager, open the tab that has the domain. Create a new CNAME record called search and point it to Google. Refresh the server to save the changes. On the client, pinging search and using nslookup will return the results of the CNAME record.
</p>
<br />

<h2>Summary</h2>

Domain Name System (DNS) is a crucial part of the internet and computing infastructure needed for devices to communicate. DNS uses various record types, such as A, AAAA, MX, and CNAME, to provide specific information about domains. It improves efficiency through caching, reduces load on servers, and enhances speed. 
