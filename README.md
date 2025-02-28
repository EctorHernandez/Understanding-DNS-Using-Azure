# Understanding-DNS-Using-Azure
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
<p>First thing we will do is search for "System Security" using command "ping System Security" in Command Prompt. We then observe that that it will give us an error message that it could not locate it. Now will try using the command "Nslookup System Security" to try and locate the ip address. Again it will fail and display the message "Ping request could not find host system. Please check the name and try again."</p>
<p>The following images display what we did in these steps:</p>
<p>
<img src="https://i.imgur.com/irWQdSx.png" height="70%" width="70%" alt="DNS Steps"/>
<img src="https://i.imgur.com/eST1R6g.png" height="70%" width="70%" alt="DNS Steps"/>
<img src="https://i.imgur.com/96xUgLF.png" height="80%" width="80%" alt="DNS Steps"/>
</p>
<p>
While logged in to the client as an admin, open the command prompt. When we ping "mainframe" it will fail. Nslookup "mainframe" provides similar results and shows that there is no DNS record for it. A DNS A-record will have to be created for mainframe on the domain controller. On the domain controller, open the DNS Manager and go to the domain you created within the Forward Lookup Zones tab. In my case, it is ernestotest.com. Right click on the page and create a New Host. For name, input mainframe and the IP address should be the same IP as the domain controller so that ping can resolve. Refresh the DNS server so that the new record can be updated. Upon returning to the client, ping mainframe once again to see that it will now resolve.
</p>
<br />

<p>
<img src="https://i.imgur.com/nLlOGKl.png" height="80%" width="80%" alt="DNS Steps"/>
<img src="https://i.imgur.com/p0VcajB.png" height="80%" width="80%" alt="DNS Steps"/>
<img src="https://i.imgur.com/ds0SCKW.png" height="80%" width="80%" alt="DNS Steps"/>
<img src="https://i.imgur.com/d4cXTCS.png" height="80%" width="80%" alt="DNS Steps"/>
</p>
<p>
This next exercise will showcase the DNS cache. On the domain controller, I changed mainframe's record address to 8.8.8.8 (Google) and refreshed the DNS server. When pinging mainframe on the client, it will still ping the old IP address. When the command ipconfig /displaydns is run, it will reveal that the DNS cache still has the old IP. In order to update the cache, we need to clear it. The command ipconfig /flushdns will empty the cache so that when we ping mainframe again, the IP address will be updated to the new one on the client side. When pinging mainframe, the new IP address of the record will show.
</p>
<br />

<p>
<img src="https://i.imgur.com/rI2NYU7.png" height="80%" width="80%" alt="DNS Steps"/>
<img src="https://i.imgur.com/FmVM0IU.png" height="80%" width="80%" alt="DNS Steps"/>
<img src="https://i.imgur.com/dyWduSW.png" height="80%" width="80%" alt="DNS Steps"/>
</p>
<p>
A CNAME record will now be made on the DNS server that will point "search" to Google. On the Forward Lookup Zones tab in the DNS Manager, open the tab that has the domain. Create a new CNAME record called search and point it to Google. Refresh the server to save the changes. On the client, pinging search and using nslookup will return the results of the CNAME record.
</p>
<br />

<h2>Lessons Learned </h2>

This lab made me understand how DNS directly works on a computer and the network. DNS records can change and sometimes this can cause connectivity issues. The DNS cache may have the old records and needs to be cleared out to update to the new records. The ipconfig /flushdns command is a common troubleshooting tool that has been referenced a lot in IT programs and I did not get a complete understanding how and why this works until I have done this lab. In the context of pinging "mainframe" at the start of the lab, the DNS cache gets checked first. If there is no result, the host file will be checked. The DNS server will be checked if there are no results in the host file. I made a record on the DNS server so a ping to mainframe can resolve. A CNAME record maps an alias name to another domain name. In this case, "search" was another name for Google.
