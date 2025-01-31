##  Lab: Building Intuition For DNS

![image](https://github.com/user-attachments/assets/c0cc2cca-ec54-47df-bb80-0dc27ca297cc)


### Overview
This lab will help you understand DNS (Domain Name System) by experimenting with A Records, DNS Cache, and CNAME Records. You will create DNS records, observe their behavior, and flush the cache when changes are made.

### Lab Environment
-  Microsoft Azure Virtual Machines
-  DC-1 (Domain Controller, DNS Server) same we used in our pravious lab
-  Client-1 (Joined to the Domain)
-  Remote Desktop Connection (RDP)
-  Windows 11 (22Hz)

###  Prerequisites
-  Active Directory (AD) Virtual Machine (DC-1)
-  Client Machine (Client-1) joined to the domain
-  Administrator access to DC-1 and Client-1

### **Lab Steps**
Step 1: Start the Virtual Machines
-  Log into Azure Portal.
-  Navigate to Virtual Machines and start:
-  DC-1 (Domain Controller)
-  Client-1 (Workstation)
RDP into DC-1 as a Domain Admin (mydomain.com\kane_admin).
RDP into Client-1 as an Admin (mydomain.com\adminuser).

###  Exercise 1: Understanding A Records
-  Check DNS Resolution Before Creating an A Record
-  On Client-1, open Command Prompt (cmd).
-  Try to ping mainframe by running:
-  ping mainframe
❌ It should fail because mainframe does not exist in DNS.
![image](https://github.com/user-attachments/assets/ef1d427d-fe04-4aef-8251-3eaaf3d3c978)


### Create A Record for "mainframe"
-  On DC-1, open Server Manager.
-  Click Tools → Select DNS.
-  Expand DC-1 → Expand Forward Lookup Zones → Click your domain (mydomain.com).
-  Right-click in the right panel → Select New Host (A or AAAA).
-  Enter:
-  Name: mainframe
-  IP Address: (Use DC-1’s private IP, e.g., 10.0.1.4)
-  Click Add Host → OK → Done.
![](https://i.imgur.com/IShgOTG.png)
![](https://i.imgur.com/akjlp1g.png)

### Test DNS Resolution On Client-1
On Client-1, open Command Prompt (cmd).
-  ping mainframe
It should now reply from 10.0.1.4.

### Exercise 2: Observing Local DNS Cache
-  Change the A Record of "mainframe"
-  On DC-1, go back to the DNS Manager.
-  Find "mainframe" in Forward Lookup Zones → Right-click Properties  → Edit.
-  Change its IP Address to 8.8.8.8.
-  Click OK → Close DNS Manager.
 -  check the DNS Cache on Client-1
-  On Client-1, open Command Prompt.
Run:
ping mainframe
❌ It still pings the old IP (10.0.1.4) instead of 8.8.8.8
![image](https://github.com/user-attachments/assets/1c85dfff-a1f0-4146-b690-a4528535ff65)
![](https://i.imgur.com/DmD03ts.png)
-  Check the DNS cache:
  -  ipconfig /displaydns
Observe that mainframe is still cached.
-  Flush the DNS Cache
-  On Client-1, run:
-  ipconfig /flushdns
  the is clears the cache.
![image](https://github.com/user-attachments/assets/d847d49b-7c7b-4f09-a684-20f55c3eeb63)

Try pinging again:
-  ping mainframe
  It should now resolve to 8.8.8.8.
![image](https://github.com/user-attachments/assets/fd43c4c1-556a-4528-8219-86fee80a5ec9)


###  Exercise 3: Configuring a CNAME Record
-  Create a CNAME Record for "search"
-  On DC-1, open DNS Manager.
-  Expand Forward Lookup Zones → Click your domain (mydomain.com).
-  Right-click in the right panel → New Alias (CNAME).
-  Enter:
-  Alias Name: search
-  Fully Qualified Domain Name (FQDN): www.google.com
-  Click OK → Done.

![](https://i.imgur.com/lGxsu3Z.png)
![](https://i.imgur.com/YxoWOYw.png)

Test the CNAME Record on Client-1
On Client-1, open Command Prompt.
-  Try to ping search:
-  ping search
  -  it may not work, because www.google.com blocks ICMP (ping).
Use nslookup search
![image](https://github.com/user-attachments/assets/2d27b96e-79c7-482d-811f-3110dc92d8fe)
![image](https://github.com/user-attachments/assets/26371e75-3dcb-4e8c-9ab8-39af67c98576)

### Conclusion
In this lab, we successfully: ✅ Created an A Record (mainframe) and observed its resolution.
✅ Examined the DNS cache, changed a record, and flushed the cache.
✅ Created a CNAME Record (search → www.google.com) and tested it.

