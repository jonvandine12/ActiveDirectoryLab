<h1>Using Virtual Machines, Active Directory, Domain Controller, Users and Computers</h1>

 ### [YouTube Demonstration]

<h2>Description</h2>
Lab consists of a walkthrough of how to use a virtual machine to run Server 2019, install Active Directory to a computer, Assign a Domain Controller, Navigate Users and Computers to Add another User, add many users (1000) with power shell, and set up their account with user name and password. 

<h2>Tools Used</h2>

- <b>Virtual Box</b> 
- <b>Active Directory</b>
- <b>Users and Computers</b>

<h2>Environments Used </h2>

- <b>Server 2019</b>
- <b>Windows 10</b>

<h2>Program walk-through:</h2>

Overview: 
We want the client to be able to connect to the internet through the DC through a created NIC that will allow flow through the internet NIC. To do this, I have created AD DS (active directory domain server) equipped with RAS NAT so that the client can connect.  We also have to set up a DHCP server with scope information to allow windows 10 clients to get an IP address to browse internet through private internal network (setup like at school or work). Lastly, we create a client computer and connect it to the domain. And in the end, we have a working network with 1000 users that can log in and use the private NIC gateway to connect to the DC's internal internet. 

<br />
High Level overview Illustration of what we are doing to create the above described network

<p align="center">
Overview of Project: <br/>
<img src="https://i.imgur.com/760MxjA.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />

Step 1: 
downloaded Virtual Box, Windows 10 ISO, and server 2019 from free websites 

Step 2: 
Used Virtual Box to install Server 2019
Password: Password1 

Step 3: 
Set up Network Interface Controller for internal setup

<img src="https://i.imgur.com/JWrNkDf.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

Step 4: 
Set up Internal NIC by changing name to “Internal” , and assigning IP address 
Network>change adapter options>renamed unassigned network
R click on Internal NIC>properties>IPv4>properties>set IP as 172.16.0.1 and subnet mask 255.255.255.0 and DSN the same as IP 

<img src="https://i.imgur.com/xWtwPuX.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>


Step 5: 
Renamed computer as Domain Controller 
Settings>about this pc>rename PC>restart PC

Step 6: 
Create active directory domain services: 
FQDN: mydomain.com
Server Manager>add roles and features> Active Directory Domain Services> Install >Promote this server to a Domain Controller >add new forest>mydomain.com Password1> NETBIOS domain name: mydomain> installed Active Directory>restart system 
<img src="https://i.imgur.com/SmMqRLn.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

Step 7: Use Active Directory Users and Computers to create an organizational unit to put admin account in, then create a new user for admin, assign user as an admin, logout, and log back in as the admin
 
Windows Administrative tools> Active Directory Users and Computers> Right click mydomain.com>new>Organizational unit> “Admins” > Right click admins>New>User > a-jvandine Password1> R click the new user> member properties >add> “domain admins”>apply >logout>log back in using other user and use the new account name and password just created above to sign back in



<img src="https://i.imgur.com/wszgDLk.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>


Step 8: Install RAS/NAT on domain controller so that other users can access internet through the domain controller

Server Manager>add roles and features> remote access> install routing and direct access and VPN> install 
Server Manager> tools> routing and remote access > R click DC > configure and enable> NAT allow internal clients to connect to the internet using one internal IP address> Select the public network interface that has internet that can be sourced to internal users> finish 

<img src="https://i.imgur.com/g9zpYNd.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>


Step 9: Set up DHCP server with scope information to allow windows 10 clients to get an IP address to browse internet through private internal network (setup like at school or work) 

Server Manager>add roles and features>DHCP>Install
Server Manager>tools>DHCP> R click IPv4> new scope> 172.16.0.100-200> add IP address from domain controller to be used by clients as 172.16.0.1 >finish

Under DHCP, refresh the domain control server DHCP with R click Authorize and then R click and refresh > the IPv4 and 6 servers are now online and display green. 
DNS is now set up

<img src="https://i.imgur.com/v8rB4Ef.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>


Step 10: Create 1000 users using power shell to show the power of understanding scripts 

Powershell ISE ran as administrator > open>find script taken from online course to create users> 

type set-executionpolicy unrestricted> cd C:\users\a-jvandine\Desktop\AD_PS-master>ls> push play> run once > power shell creates the 1000 users 

<img src="https://i.imgur.com/w2A9wop.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

<img src="https://i.imgur.com/t8aTuva.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>




Step 11: create client computer and connect it to the domain 

Load windows 10 ISO into virtual box>settings>adapter 1 set to internal network instead of NAT>install windows 10 on Virtual Box> command ipconfig /all >verify internet connection through NAT direct controller by pinging google and seeing if a response happens. 

rename pc and check in the DC if a new lease has been created for user1

Windows 10 VM: system>rename this pc>name client 1 and join mydomain.com

navigate back to VM with the DC: under DHCP click on IPv4>address leases> see that there is one lease with the new name you have given to the windows 10 client. Success. 


<img src="https://i.imgur.com/4lV2OsJ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

<img src="https://i.imgur.com/FmkXI5W.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

This process has successfully achieved the goal of allowing the client to sign in using an account, and to be able to connect to the internet through the DC thanks to a created NIC that allows flow through the DC internet NIC. 
Lab complete. 

