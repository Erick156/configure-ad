![image](https://github.com/jameswsm/configure-ad/assets/170709350/532c9016-9486-4f51-a45e-fde696c9324f)
</p>

<h1>Active Directory Configuration Tutorial</h1>
Configuration of Active Directory with Azure VMs.<br />
<br />
Part 1: Setup Resources in Azure <br />
Part 2: Ensure Connectivity between the client and Domain Controller <br />
Part 3: Install Active Directory Domain Services + promote as DC <br />
Part 4: Create an Admin User Account in Active Directory <br />
Part 5: Join a client to the domain <br />
Part 6: Allow multiple users on the domain to log into Client1<br />
Part 7: Test case for Part 6 <br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Internet Information Services (IIS)
- Active Directory Users and Computers
- Server Manager
- Powershell

<h2>Operating Systems Used </h2>

- Windows 10</b> (21H2)

<h2>List of Prerequisites</h2>

- Microsoft Azure Active Subscription (Creation of Reasearch Group, VMs, Virtual Networks, Subnets)
  
<h2>Part 1: (Setup Resources in Azure) Steps: 1 - 18</h2>

![image](https://github.com/user-attachments/assets/653655d3-c396-405c-b545-87a686da619e)

<p>
Step 1: Open Virtual Machines services
</p>
<br />

![image](https://github.com/user-attachments/assets/5cd78f3c-6fb6-475d-9026-9eb3644567f1)

<p>
Step 2: Click Create Azure virtual machine
</p>
<br />

![image](https://github.com/user-attachments/assets/cc15826a-e638-4195-8d42-ee489d556445)

<p>
Step 3: Create new Resource Group and choose a name. (ex: activeDirectory)
</p>
<br />

![image](https://github.com/jameswsm/configure-ad/assets/170709350/ff915c89-1138-4c56-b9ac-8e3ca095049e)
<p>
Step 4: Enter information, ie: Virtual machine name that we will use for our Domain Controller (ex DC1) image: Windows Server 2022 Datacenter: Azure Edition.
</p>
<br />

![image](https://github.com/jameswsm/configure-ad/assets/170709350/223d075a-4202-4aac-8dea-4c2bca30b825)
<p>
Step 5: Choose at least 2 vcpus. Enter a Username/password (ex: labuser). Check in confirmation box below. Click Next.
</p>
<br />

![image](https://github.com/jameswsm/configure-ad/assets/170709350/710441f6-cbed-4a08-8575-48a0401e1269)
<p>
Step 7: Click Next: Networking 
</p>
<br />

![image](https://github.com/jameswsm/configure-ad/assets/170709350/9b7f2e9f-99a1-4efd-874b-bed49500e690)
<p>
Step 8: Click Review + Create -> Click Create
</p>
<br />

![image](https://github.com/jameswsm/configure-ad/assets/170709350/296f7e92-e69c-48a3-a1df-960b246465df)
<p>
Step 9: Navigate back to Virtual Machines -> Create -> Azure virtual machine.  We are going to create our client computer that will join the domain
</p>
<br />

![image](https://github.com/jameswsm/configure-ad/assets/170709350/1b5053c6-1a75-4274-9004-ed77a21a47e6)
![image](https://github.com/jameswsm/configure-ad/assets/170709350/c178f7a2-522c-438d-a437-23e5e855064b)
<p>
Step 10: Fill out info ex: Choose RG we created (ex: activeDirectory). Enter name (ex: Client1). Click Review + Create -> Next -> Next: Networking
</p>
<br />

![image](https://github.com/jameswsm/configure-ad/assets/170709350/afa02bbb-fe8c-4464-9ad3-4380ef8d7c72)
<p>
Step 11: Virtual Network: Choose the same virtual network as the Domain Controller. (should have been created when we created the DC). Click Review + create -> Create
</p>
<br />

![image](https://github.com/jameswsm/configure-ad/assets/170709350/1a14975e-f1d7-4fda-87cd-5282f489a462)
<p>
Step 12: Navigate to Virtual machines -> Click Domain Controller (ex: DC1)
</p>
<br />

![image](https://github.com/jameswsm/configure-ad/assets/170709350/af0e36e5-2e4a-449d-a844-18d296d09f1a)
<p>
Step 13: Navigate on the left to Networking -> Network Settings
</p>
<br />

![image](https://github.com/jameswsm/configure-ad/assets/170709350/af0e36e5-2e4a-449d-a844-18d296d09f1a)
<p>
Step 14: When we have a server (ex: DC1) that will offer services to another computer, (ex: we have a domain controller offering active directory services), in these cases we DONT want the ip address to change aka do not get it from DHCP, so we are gonna change domain controller virtual nic ip address from dynamic to static. Click on Network interface (ex: dc1190)
</p>
<br />

![image](https://github.com/jameswsm/configure-ad/assets/170709350/58d5d1ca-c4e4-4165-b1ed-e4d35c5f20e3)
<p>
Step 15: Click IP configurations.
</p>
<br />

![image](https://github.com/jameswsm/configure-ad/assets/170709350/70c37640-dd4e-4d63-87cb-58f351f1cce1)
<p>
Step 16: Here we see the Private IP address is set to dynamic. Lets change this to Static. Click the Name (ex: ipconfig1)
</p>
<br />

![image](https://github.com/jameswsm/configure-ad/assets/170709350/6074648e-5d26-4587-829a-31e98b5bb124)
<p>
Step 17: Change the Private IP address from dynamic to static (ex: 10.0.0.4). Click Save.
</p>
<br />

![image](https://github.com/user-attachments/assets/9beee212-f014-4c36-9443-f03f59216355)


<p>
Step 18: Navigate to Virtual Machines. Ensure Both Client1 and DC1 are in the same Virtual network/subnet (ex: DC1-vnet/default)
</p>
<br />
<br />
<br />

<h2>Part 2: (Ensure Connectivity between the client and Domain Controller) Steps: 1 - 5</h2>

![image](https://github.com/user-attachments/assets/7f5d5499-7863-4a30-8d6c-cbb5f64f01df)

<p>
Step 1: If we log in to Client1 and ping DC1, it will fail. We need to log in to our Domain Controller(DC1) and enable ICMPv4 on the local windows Firewall. Copy IP address from DC1 (ex:74.235.208.187) -> Use this to RDC into DC1 -> Enter credentials (ex:labuser/password)
</p>

![image](https://github.com/jameswsm/configure-ad/assets/170709350/614bcade-c164-4423-81a0-5c86f18ba0d8)
<p>
Step 2: Open Windows Defender Firewall with Advanced Security
</p>
<br />

![image](https://github.com/user-attachments/assets/3eec621b-0f68-4e42-8c64-67f7196a6fe8)

<p>
Step 3: Click Inbound Rules -> Enable both ICMP Echo Requests on protocol ICMPv4
</p>
<br />

![image](https://github.com/user-attachments/assets/6bef2241-2975-4f52-9a05-1c7c8942515c)

<p>
Step 4: Minimize DC1 -> RDC into Client1
</p>
<br />

![image](https://github.com/jameswsm/configure-ad/assets/170709350/89591070-caf0-4942-bc42-3c8380cc4f9c)
<p>
Step 5: Open Command Line -> echo ping the Domain Controller's private IP address (ex: 10.0.0.4) -> Confirm connectivity is working -> close command lin -> minimize Client 1
</p>
<br />
<br />
<br />

<h2>Part 3: (Install Active Directory Domain Services + promote as DC) Steps: 1 - 5</h2>

![image](https://github.com/jameswsm/configure-ad/assets/170709350/12e714cd-0f4f-4634-a016-9a28f6f58f8d)
<p>
Step 1: RDC into DC1. In Server Manager click Add roles and features -> next -> next -> next
</p>
<br />


![image](https://github.com/jameswsm/configure-ad/assets/170709350/6847ada0-1561-471b-a452-b09df577e884)
<p>
Step 2: Check the box for Active Directory Domain Services -> Add Features -> next through all dependencies -> install
</p>
<br />

![image](https://github.com/jameswsm/configure-ad/assets/170709350/4fbbfb24-1a6d-4502-9663-31c824b73299)
<p>
Step 3: Click exclamation in top right corder of Server Manager -> Click "Promote this server to a domain controller"
</p>
<br />

![image](https://github.com/jameswsm/configure-ad/assets/170709350/998811d9-27c3-4f2a-8a74-c014a5ff5774)
<p>
Step 4: Choose Add a new forest -> Add a root domain name (ex: mydomain.com) -> Next
</p>
<br />

![image](https://github.com/jameswsm/configure-ad/assets/170709350/db0fbf00-bc0c-4be9-8b30-5078104dd5aa)
<p>
Step 5: Enter a password -> Next through all checks -> Install -> Computer will restart when finished -> RDC back into DC1 after restart -> May have to enter username with domain: (username: mydomain.com\labuser)
</p>
<br />

![image](https://github.com/jameswsm/configure-ad/assets/170709350/4d0a9ca5-3a60-45a0-8018-637146554c12)
<p>
Domain Controller is online with domain set up.
</p>
<br />
<br />
<br />


<h2>Part 4: (Create an Admin User Account in Active Directory) Steps: 1 - 7</h2>

![image](https://github.com/jameswsm/configure-ad/assets/170709350/771c9cc6-97d5-4c9a-9671-7ff58ed369bf)
<p>
Step 1: Navigate to Active Directory Users and Computers
</p>
<br />

![image](https://github.com/jameswsm/configure-ad/assets/170709350/351a3013-5f31-48ae-b501-15a69c11972e)
<p>
Step 2: Create a new Organizational Unit (ex: _EMPLOYEES). note: underscore in name makes it easier to filter. Create another OU (ex: _ADMINS)
</p>
<br />

![image](https://github.com/jameswsm/configure-ad/assets/170709350/8903a485-8493-47e3-8b3b-0b1bceb57a24)
<p>
Step 3: We can continue using labuser, however ambiguous account names are not best practices. We are going to create another ADMIN account that is tied to an identity. Right click _ADMINS to create a new user.
</p>
<br />

![image](https://github.com/jameswsm/configure-ad/assets/170709350/b513ed32-1f5c-4c04-bf2f-0c61ef9244e9)
<p>
Step 4: Fill out information for the new user. (ex: james_admin) -> Next -> Set Password
</p>
<br />

![image](https://github.com/jameswsm/configure-ad/assets/170709350/972c0c2c-77e8-47aa-a851-aa131e733907)
<p>
Step 5: Go to properties of user we want to give admin permissions
</p>
<br />

![image](https://github.com/jameswsm/configure-ad/assets/170709350/af399f80-cdb8-4c8e-a7c5-06f41c0365c1)
<p>
Step 6: Click Member of -> enter domain and click Check Names -> Domain Admins -> Ok -> Ok -> Apply
</p>
<br />

![image](https://github.com/jameswsm/configure-ad/assets/170709350/4839e78f-a39e-4f82-af60-820e287d5a0e)
![image](https://github.com/jameswsm/configure-ad/assets/170709350/2bbb2385-5f51-492b-95e6-7dbbeec7e858)
<p>
Step 7: Log out of Domain Controller(currently is labuser), log back in as our new user (ex:james_admin)
</p>
<br />
<br />
<br />


<h2>Part 5: (Join a client to the domain) Steps: 1 - 8</h2>

![image](https://github.com/user-attachments/assets/83e043db-3e87-4ef8-bc77-cf2f36534592)

<p>
Step 1: Navigate to Domain Controller(ex: DC1) Virtual Machine -> Network settings -> copy private IP ADDRESS  (ex: 10.0.0.6)
</p>
<p>
note: We are going to join client computer to the domain, and we want to control DNS settings on this computer. When we create a VM inside an Azure virtual network, the ip addressing is set up automatically and the DNS is set to use the VNET DNS Server. We dont want our client computer to use the VNET DNS serever, we want our client to use the IP ADDRESS of the Domain Controller as the DNS server, because when you install active directory on a server, and turn that server into a domain controller, a dns service is installed as well. In order for the client to be able to join the domain, it needs to use the domain controller as its DNS server. The domain controller knows what our domain is (ex: mydomain.com), if our client uses the VNET DNS server, the VNET DNS server is going to look through the internet for mydomain.com and fail.
</p>
<br />

![image](https://github.com/user-attachments/assets/2ede340e-69dc-464a-a274-9b4bccdc72e6)

<p>
Step 2: Navigate to the Client1 Virtual Machine -> Network settings -> click Network interface (ex: dc1305)
</p>
<br />

![image](https://github.com/jameswsm/configure-ad/assets/170709350/cf5b5ea7-2834-4e97-9b74-7be86c30c88c)
<p>
Step 3: Settings -> DNS servers -> Check in Custom -> Enter Domain Controller (ex:DC1) private IP ADDRESS -> Save
</p>
<br />

![image](https://github.com/jameswsm/configure-ad/assets/170709350/a32e18d4-2738-421b-a5d4-6f63a3bff6a9)
<p>
Step 4: Navigate to Client1 Virtual Machine -> Restart -> RDC into Client1 using labuser bc its not part of the domain yet.
</p>
<br />

![image](https://github.com/jameswsm/configure-ad/assets/170709350/97ccd184-e93b-4e71-93e7-56eb07cdc18c)
<p>
Step 5: Open command line -> ipconfig /all -> DNS Server should be 10.0.0.6 (Domain Controller private IP ADDRESS)
</p>
<br />

![image](https://github.com/jameswsm/configure-ad/assets/170709350/f9c0f445-33bc-4a9e-8186-359c8c6202bd)
<p>
Step 6: Start -> System -> Rename this PC(adavanced) -> Change -> Check in Domain -> enter domain (ex:mydomain.com) -> Ok -> Enter the info of the Domain Administrator that we created (ex: mydomain.com\james_admin) -> Computer will restart
</p>
<br />

![image](https://github.com/jameswsm/configure-ad/assets/170709350/1250c527-c984-4bd5-90cd-44b537513595)
<p>
Step 7: Now that Client1 is part of the domain, we can log into Client1 with our domain admin account.
</p>
<br />

![image](https://github.com/jameswsm/configure-ad/assets/170709350/21e76e2c-1b11-40e6-aec5-05b14284a6a2)
<p>
Step 8: To check, log into Client1 using admin username/password. 
</p>
<br />
<br />
<br />


<h2>Part 6: (Allow multiple "domain users" to log into client) Step: 1 </h2>

![image](https://github.com/jameswsm/configure-ad/assets/170709350/d661b2b2-19e6-4636-ab33-d6093a38d83c)
<p>
Step 1: Client1 Environment -> System -> Remote Desktop -> Select users that can remotely access this PC -> Add -> domain users -> Check Names -> Ok
</p>
<p>
note: This will simulate an environment on Client1 where any user on the domain can log into Client1 using their own username/password. Group Policy allows us to do the above to many different Clients at the same time.
</p>
<br />

<h2>Part 7: (Test case for Part 6 ) Step: 1 - 6</h2>

![image](https://github.com/user-attachments/assets/3e4d8717-f16b-4ef9-a5e7-908b13cd832c)

<p>
Step 1: Log into Domain Controller(ex: DC1) as admin (ex: james_admin)
</p>
<br />

![image](https://github.com/jameswsm/configure-ad/assets/170709350/5afec38d-4149-48fc-9088-f621c58d5fd7)
<p>
Step 2: Open Powershell_ise as administrator 
</p>
<br />

![image](https://github.com/jameswsm/configure-ad/assets/170709350/01e89f96-6d1f-4f5b-b3a6-c6bc1b0dbc1c)
<p>
Step 3: Create a new script -> paste this code into file https://github.com/jameswsm/ad-usergenerate -> Run script
</p>
<p>
note: This will create 10,000 different accounts with the password: Password1
</p>
<br />

![image](https://github.com/jameswsm/configure-ad/assets/170709350/934c8d5b-2cef-49cf-9e96-740baeb82e8d)
<p>
Step 4: Navigate to Active Directory Users and Computers -> _EMPLOYEES -> refresh -> View employee list being populated
</p>
<br />
<p>
note: Because all 10,000 users are in the domain, they can all log in to Client1 with their own username/password.
</p>

![image](https://github.com/jameswsm/configure-ad/assets/170709350/b5f48168-7442-4595-aecd-835371594423)
<p>
Step 5: Copy a name from _EMPLOYEES (ex: bes.jom) -> Minimize DC1
</p>
<br />

![image](https://github.com/user-attachments/assets/46b2ad2e-2a25-481c-b1d0-266544a97284)

<p>
Step 6: Log into Client1 with the employee name we have copied (ex: user:bes.jom password:Password1 )
</p>
<br />

![image](https://github.com/jameswsm/configure-ad/assets/170709350/1b6d461c-63a6-4177-8417-c36d2b839d47)
<p>
Because bes.jom is in the domain and Client1 is joined to that domain, Client1 can use the domain to validate bes.jom and allow bes.jom to log into Client1. All 10,000 users can now log into Client1!
</p>
<br />
















