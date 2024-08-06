<p align="center">
<img src="https://i.imgur.com/Clzj7Xs.png" alt="osTicket logo"/>
</p>

<h1>osTicket - Prerequisites and Installation</h1>
This tutorial outlines the prerequisites and installation of the open-source help desk ticketing system osTicket through Azure.<br />




<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Internet Information Services (IIS)

<h2>Operating Systems Used </h2>

- Windows 10</b> (22H2)

<h2>List of Prerequisites</h2>

- Virtual Machine
- Internet Information Services
- PHP Manager
- VC redist
- MySQL
- Heidi SQL

<h2>Installation Steps</h2>

![image](https://github.com/user-attachments/assets/b17d078d-dd61-4589-a2ac-7183577bd9a7)


</p>
<p>
After logging into my Azure account, I created a Resource Group and a Windows 10 Virtual Machine with 4 Virtual CPUs in that resource group. After creating the virtual machine, I used a remote desktop to connect to the virtual machine.
</p>
<br />

![image](https://github.com/user-attachments/assets/d0cc6aa4-f6d7-4419-977c-8dd8e546f413)


</p>
<p>
After connecting to the virtual machine, I first accessed the control panel. From there, I clicked the Programs tab. Next, I clicked the Turn the Features On or Off tab and proceeded to check off the boxes viewed in the picture above (Right Side). To confirm that IIS was activated, I typed 127.0.0.1 in the web browser as seen in the picture above(Left Side).
</p>
<br />

![Installation Files](https://github.com/BenW618/osticket-prereqs/assets/140227052/aba984c9-74b8-47f9-b717-4a01cfc936ac)

</p>
<p>
The next step involves many files being installed. A list of files used to download will be presented below.
 
- PHPManagerForIIS_V1.5.0.msi
- rewrite_amd64_en-US.msi
[Before installing the rest of the files, create the directory C:\PHP]
- php-7.3.8-nts-Win32-VC15-x86.zip
- VC_redist.x86.exe
- mysql-5.5.62-win32.msi

  
</p>
<br />


![Register PHP](https://github.com/BenW618/osticket-prereqs/assets/140227052/c6b0055b-d677-4ea4-81b8-79f15a2a8256)

</p>
<p>
After installing the files, I opened the IIS Manager as an administrator. Next, I clicked the PHP Manager tab and the Register new PHP version. I then typed C:\PHP\php-cgi.exe.
</p>
<br />


![Install osTicket](https://github.com/BenW618/osticket-prereqs/assets/140227052/b4c36fb3-f67a-4390-864e-fc0e478915e1)

</p>
<p>
After registering PHP, I installed the osTicket file. Once the file was installed, I extracted and copied the “upload” folder to c:\inetpub\wwwroot. Within c:\inetpub\wwwroot, I renamed “upload” to “osTicket”. After this, I restarted IIS Manager and clicked the sites tab.
</p>
<br />


![Screenshot (7)](https://github.com/BenW618/osticket-prereqs/assets/140227052/5e5dcc25-8dfb-4286-9314-f658c1ab4b54)

</p>
<p>
Before I could continue, I had to fix some of the extensions. First, I went back to IIS and clicked the “PHP Manager” tab under osTicket. From there, I enabled three extensions; php_imap.dll, php_intl.dll, php_opcache.dll. Next, I refreshed the osTicket page to observe a few of the errors disappearing.
</p>
<br />


![Screenshot (9)](https://github.com/BenW618/osticket-prereqs/assets/140227052/bdf586d0-dc26-4cd2-aefa-e3e6c9a0e349)

</p>
<p>
I pressed continue on the osTicket page. I proceeded to fill out the needed information until I reached the “Database Setting” category. I then downloaded HeidiSQL to create a new session. I put the sessions information in the “database setting “category and then proceeded to hit the "Install Now" tab.
</p>
<br />


![Screenshot (10)](https://github.com/BenW618/osticket-prereqs/assets/140227052/6096f8ec-9d04-4a52-88d2-61993341c00c)

</p>
<p>
Next, I browsed to the help desk login page: http://localhost/osTicket/scp/login.php
</p>
<br />


![Screenshot (11)](https://github.com/BenW618/osticket-prereqs/assets/140227052/05af93eb-f4a1-4da1-acf2-b921cbfddf16)

</p>
<p>
</p>
<br />
