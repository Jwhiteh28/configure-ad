<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This guide covers the implementation of an on-premises Active Directory setup using Azure Virtual Machines.<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>Step 1: Overview</h2>
<b>Configure and install Active Directory services on the designated Domain Controller virtual machine.</b>
<p>
  1. Install "Active Directory Domain Services" in DC-1
  <ul>
    <li>In the Server Manager dashboard</li>
    <li>Click Add roles and features</li>
    <li>Select Active Directory Domain Services and finish the installation</li>
    <img src="https://i.imgur.com/S6IjH92.png" width="600" alt="AD"/>
  </ul>
</p>
<br />

<h2>Step 2: Promote DC-1 to Domain Controller</h2>
<p>
  2. Click on the flag at the top right of the screen with the warning sign
  <ul>
    <li>Promote DC-1 to Domain Controller</li>
    <li>Setup a new forest as "mydomain.com"</li>
    <li>Restart and then log back into DC-1 as user:</li>
    <img src="https://i.imgur.com/CwWB3iN.png" width="600" alt="AD"/>
    <li>Run Active Directory Users & Computers shown below</li>
    <img src="https://i.imgur.com/rFPPfn3.png" width="600" alt="AD"/>
  </ul>
</p>
<br />


<h2>Step 3: Create an Organizational Unit(OU) named _EMPLOYEES and _ADMINS</h2>
<p>
- Right click on mydomain.com and select New and click on Organizational Unit
</p>
<img src="https://i.imgur.com/baIUUZ5.png" width="600" alt="AD"/>
<p>- Create OU_EMPLOYEES and _ADMINS</p>
<img src="https://i.imgur.com/gfar0X4.png" width="600" alt="AD"/>
<br />


<h2>Step 4: Create a Domain Admmin user within the domain</h2>
<ul>
  <li>Right click on the OU _ADMINS and create a new user named Jane Doe</li>
  <li>With the username jane_admin</li>
  <img src="https://i.imgur.com/5ZCBGa8.png" width="600" alt="AD"/>
</ul>
<br />


<h2>Step 5: Add jane_admin to the "Domain Admins" Security Group</h2>
<ul>
  <li>Make Jane Doe into an admin by right clicking her name > properties</li>
  <li>Member of and adding her to the "Domain Admins" Security Group</li>
  <img src="https://i.imgur.com/jOWw7m3.png" width="600" alt="AD"/>
</ul>
<br />


<h2>Step 6: Log out of DC-1 to sign in as Jane Doe</h2>
<ul>
  <li>Log in as "mydomain.com\jane_admin"</li>
  <li>Use jane_admin as your admin account from now on</li>
  <img src="https://i.imgur.com/eDY45PT.png" width="600" alt="AD"/>
</ul>
<br />


<h2>Step 7: Log into Client-1 as the original local admin and join it to the domain (Computer will restart)</h2>
<ul>
  <li>Right click the start menu > click system > click rename this pc advanced</li>
  <li>Under the computer name tab, click on "Change"</li>
  <li>Join it to the domain "mydomain.com"</li>
  <img src="https://i.imgur.com/3VId8pg.png" width="600" alt="AD"/>
  <li>Enter the name and password of an account with permission to join the domain</li>
  <li>Use the account: mydomain.com\jane_admin</li>
  <li>Once Client-1 has been added, the VM will restart</li>
  <img src="https://i.imgur.com/AjSrttS.png" width="600" alt="AD"/>
</ul>
<br />


<h2>Step 8: Log into the Domain Controller and verify Client-1 appears in ADUC</h2>
<p>Expand mydomain.com then go to "computers" to verify</p>
<img src="https://i.imgur.com/q9GGutL.png" width="600" alt="AD"/>
<br />


<h2>Step 9: Create a new OU named "_CLIENTS" and drag Client-1 into it</h2>
<img src="https://i.imgur.com/GWagISK.png" width="600" alt="AD"/>
<br />


<h2>Step 10: Setting up RDP & User Accounts via PowerShell</h2>
<ul>
  <li>Setup RDP for non-administrative users on Client-1</li>
  <li>We have to login to Client-1 as an admin (using mydomain.com\jane_admin) right click the start menu and open system</li>
  <li>Click on "Remote Desktop" on User Accounts and click "Select users that can remotely access this PC"</li>
  <li>Allow "Domain Users" access to remote desktop</li>
  <li>After completing those steps you should be able to log into Client-1 as normal user (any user)</li>
  <img src="https://i.imgur.com/TABonUl.png" width="600" alt="AD"/>
</ul>
<br />


<h2>Step 11: Run Powershell script</h2>
<ul>
  <li>Use a Powershell script to generate a number of users for our Active Directory Domain</li>
  <li>Login to DC-1 as mydomain.com\jane_admin</li>
  <li>Open Powershell_ISE as an administrator and create a new file then save</li>
  <li>https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1</li>
  <li>Paste the script to generate thousands of users into the domain</li>
  <li>Run Script</li>
  <img src="https://i.imgur.com/DfUO0U8.png" width="600" alt="AD"/>
  <li>When finished, open ADUC and observe the accounts in the appropriate OU "_EMPLOYEES"</li>
  <img src="https://i.imgur.com/5f6kAx3.png" width="600" alt="AD"/>
</ul>
<br />


<h2>Step 12: Login as any user</h2>
<ul>
  <li>You can now log in using any of the user accounts created by the script to further confirm that it executed successfully.
</li>
  <li>Please note the script's default password — `Password1` — which is assigned to all user accounts.
</li>
<img src="https://i.imgur.com/BWlWMtp.png" width="600" alt="AD"/>
</ul>
<p>As shown, the PowerShell script successfully created a user with the username **"mog.raki"**. We were then able to log in to **Client-1** using this account’s credentials as a standard user.
</p>
<img src="https://i.imgur.com/zx2OF0y.png" width="600" alt="AD"/>
