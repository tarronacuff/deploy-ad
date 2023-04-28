<p><img alt="Microsoft Active Directory Logo" src="https://i.imgur.com/pU5A58S.png" /></p>

<h1>How to Install and Deploy Active Directory</h1>

<p><span style="color:#2980b9">Active Directory is a directory service created by Microsoft that allows network administrators to manage and organize a company&#39;s resources, such as computers, users, and security policies. It provides a centralized database of information about all the resources on a network and enables administrators to manage those resources from a single location.</span></p>

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>Configuration Task</h2>

- Task 1: Create resources
- Task 2: Establish connectivity to between Domain Controller and Client
- Task 3: Install Active Directory
- Task 4: Create an administrator and regular account in Active Directory
- Task 5: Join Client to the Domain Controller 
- Task 6: Setup Remote Desktop for non-administrative users to Client
- Task 7: Create users in Active Directory using Powershell script

<h3><strong>Task #1: Create Resources</strong></h3>

<p>The 1st task is to create to two Azure virtual machines. The 1st virtual machine is the domain controller (DC-1) which will have Windows Server 2022.&nbsp; The second virtual machine is the client (Client-1) which will have Windows 10. The client is just any computer connected to the domain controller. -Note: Ensure that Client-1 is on the same virutal network as DC-1. You can confirm this under the &quot;Networking&quot; tab &gt; topology from within your VM.</p>

<p><a href="https://imgur.com/HyGecxa"><img src="https://i.imgur.com/HyGecxa.png" title="source: imgur.com" /></a></p>

<p><a href="https://imgur.com/miLS17P"><img src="https://i.imgur.com/miLS17P.png" title="source: imgur.com" /></a></p>

- Next is to ensure DC-1&#39;s private IP address is changed from dynamic to static. 
- DC-1&#39;s private IP address needs to be static so it does not change during the course of this exercise. 
- To do this, go to DC-1&#39;s &quot;Networking&quot; section and click on the virtual Network Interface Card (NIC).

<p><a href="https://imgur.com/aQEuOOM"><img src="https://i.imgur.com/aQEuOOM.png" title="source: imgur.com" /></a></p>

<p><span style="color:#2980b9">- Click on IP configurations.</span></p>

<p><a href="https://imgur.com/29OMhnc"><img src="https://i.imgur.com/29OMhnc.png" title="source: imgur.com" /></a></p>

- Click on the private IP address (10.0.0.4)(Your private IP maybe different). 

<p><a href="https://imgur.com/jUh0D1i"><img src="https://i.imgur.com/jUh0D1i.png" title="source: imgur.com" /></a></p>

- Click the &quot;Assignment&quot; button to switch it from dynamic to static. 

<p><a href="https://imgur.com/iXtZFN9"><img src="https://i.imgur.com/iXtZFN9.png" title="source: imgur.com" /></a></p>

<h3><strong>Task 2: Establish Connectivity To Between Domain Controller and Client</strong></h3>

- The 2nd task is to establish connectivity between Client-1 and DC-1. Use Microsoft Remote Desktop to connect to both virtual machines.
- Open Client-1 and open the Command line. 
- Type in the command &quot;ping -t&quot; + the private IP address of DC-1. In this case, it is 10.0.0.4.

<p><a href="https://imgur.com/lQk1hZw"><img src="https://i.imgur.com/lQk1hZw.png" title="source: imgur.com" /></a></p>

- Due to the firewall blocking incoming Internet Control Message Protocol (ICMP) traffic, the request is timing out. 
- To fix this, login into DC-1 and open the application Windows Defender Firewall with Advanced Security from the Start menu.

<p><<a href="https://imgur.com/ZbHXCAE"><img src="https://i.imgur.com/ZbHXCAE.png" title="source: imgur.com" /></a></p>

<p><a href="https://imgur.com/tYKGcq2"><img src="https://i.imgur.com/tYKGcq2.png" title="source: imgur.com" /></a></p>

- From here, sort the list by protocol so it is easier to see. 
- Scroll down to ICMPv4 and enable these two inbound rules.

<p><a href="https://imgur.com/uliuVhW"><img src="https://i.imgur.com/uliuVhW.png" title="source: imgur.com" /></a></p>

- Switch back to Client-1 and the Command line will start to ping DC-1 successfully.

<p><a href="https://imgur.com/pOEu32a"><img src="https://i.imgur.com/pOEu32a.png" title="source: imgur.com" /></a></p>

<h3><strong>Task 3: Install Active Directory</strong></h3>

- The 3rd task is to install Active Directory Domain Services.
- Go back into DC-1 Open the Server Manager in DC-1.
- This should normally open whenever DC-1 is logged into for the first. It can also be found in the Start menu.

<p><a href="https://imgur.com/pmODHgx"><img src="https://i.imgur.com/pmODHgx.png" title="source: imgur.com" /></a></p>

- In Server Manager, click &quot;Add roles and features.&quot; Follow the prompts. At &quot;Server Roles&quot;, check &quot;Active Directory Domain Services.&quot; Then click &quot;Add Features.&quot;

<p><a href="https://imgur.com/epQTsNH"><img src="https://i.imgur.com/epQTsNH.png" title="source: imgur.com" /></a></p>

<p><a href="https://imgur.com/UjKZxkp"><img src="https://i.imgur.com/UjKZxkp.png" title="source: imgur.com" /></a></p>

- Continue to follow all the installation prompts until it is done.

<p><a href="https://imgur.com/FH9alix"><img src="https://i.imgur.com/FH9alix.png" title="source: imgur.com" /></a></p>

- Once the installation is done, back at the Server Manager Dashboard, click the flag with the yellow hazard sign underneath.
- Then click &quot;Promote this server to a domain controller.&quot;

<p><a href="https://imgur.com/SMHOmvN"><img src="https://i.imgur.com/SMHOmvN.png" title="source: imgur.com" /></a></p>

<p><a href="https://imgur.com/YhVGGiC"><img src="https://i.imgur.com/YhVGGiC.png" title="source: imgur.com" /></a></p>

- Click &quot;Add a new forest&quot; and type in the root domain. In this example, it was mydomain.com.

<p><a href="https://imgur.com/GVRjrOM"><img src="https://i.imgur.com/GVRjrOM.png" title="source: imgur.com" /></a></p>

- Follow all the prompts to finish the installation.
- DC-1 will restart to add all the updates.

<p><a href="https://imgur.com/tipDxLI"><img src="https://i.imgur.com/tipDxLI.png" title="source: imgur.com" /></a></p>

<h3><strong>Task #4: Create An Administrator and Regular Account in Active Directory</strong></h3>

- The 4th task is to create an administrator account and a regular account in Active Directory.
- Once DC-1 has restarted, open up Server Manager, click &quot;Tools&quot; in the upper right corner, and select &quot;Active Directory Users and Computers.&quot;

<p><a href="https://imgur.com/Xp11haR"><img src="https://i.imgur.com/Xp11haR.png" title="source: imgur.com" /></a></p>

- In Active Directory Users and Computers, right click the domain (mydomain.com), go to &quot;New&quot; and &quot;Organizational Unit.&quot;
- Create two organizational units for administrators (_ ADMINS) and employees (_ EMPLOYEES).
- Once done, right click mydomain.com and click referesh to sort the new organizational units to the top.

<p><a href="https://imgur.com/xekMn8E"><img src="https://i.imgur.com/xekMn8E.png" title="source: imgur.com" /></a></p>

<p><a href="https://imgur.com/GBG6Xut"><img src="https://i.imgur.com/GBG6Xut.png" title="source: imgur.com" /></a></p>

<p><a href="https://imgur.com/coNj1mc"><img src="https://i.imgur.com/coNj1mc.png" title="source: imgur.com" /></a></p>

<p><a href="https://imgur.com/Z21Dnme"><img src="https://i.imgur.com/Z21Dnme.png" title="source: imgur.com" /></a></p>

- Click on the _ ADMINS organizational unit and right click into the right window pane. Go to &quot;New&quot; and click &quot;User.&quot;
- Fill in the boxes to create a new user. In this example, &quot;Jane Doe&quot; was used and the login name is &quot;jane_admin.&quot;


<p><a href="https://imgur.com/h8Wm70Z"><img src="https://i.imgur.com/h8Wm70Z.png" title="source: imgur.com" /></a></p>

<p><a href="https://imgur.com/tHQy5ep"><img src="https://i.imgur.com/tHQy5ep.png" title="source: imgur.com" /></a></p>

<p><a href="https://imgur.com/iqk5mDY"><img src="https://i.imgur.com/iqk5mDY.png" title="source: imgur.com" /></a></p>

- Once the account has been created, right click it and go to &quot;Properties.&quot;
- Click the &quot;Member Of&quot; tab, click the &quot;Add...&quot; button, type in domain and click the &quot;Check Names&quot; button.
- Click &quot;Domain Admins&quot; and press &quot;OK&quot; and &quot;Apply&quot; to exit out.

<p><a href="https://imgur.com/dwriDz7"><img src="https://i.imgur.com/dwriDz7.png" title="source: imgur.com" /></a></p>

<p><a href="https://imgur.com/SWXQ7rc"><img src="https://i.imgur.com/SWXQ7rc.png" title="source: imgur.com" /></a></p>

- Next, Log out of DC-1 as &quot;labuser&quot; and log back in as the administrator account that was created (jane_admin).

<h3><strong>Task #5: Join Client to the Domain Controller</strong></h3>

- The 5th task is to join Client-1 to DC-1.<br />
T-o do this, go back to the Azure Portal.
Go to the Client-1 virtual machine and click on the &quot;Networking&quot; section in underneath &quot;Settings&quot; on the left hand side.

<p><a href="https://imgur.com/ShfTooL"><img src="https://i.imgur.com/ShfTooL.png" title="source: imgur.com" /></a></p>

- Click on &quot;DNS Servers.&quot;
- Then click, &quot;Custom.&quot;
- Type in DC-1&#39;s private IP address (10.0.0.4).

<p><a href="https://imgur.com/WjSTlit"><img src="https://i.imgur.com/WjSTlit.png" title="source: imgur.com" /></a></p>

<p><a href="https://imgur.com/jEtth6T"><img src="https://i.imgur.com/jEtth6T.png" title="source: imgur.com" /></a></p>

- Restart Client-1 then log back in.
- After you have logged back in, right click the Start menu and click on &quot;System.&quot;

<p><a href="https://imgur.com/EjtOZEO"><img src="https://i.imgur.com/EjtOZEO.png" title="source: imgur.com" /></a></p>

- On the right hand side, click &quot;Rename this PC (advanced).&quot; - Then click the &quot;Change...&quot; button and type in the domain name (mydomain.com).

<p><a href="https://imgur.com/QuQCAMH"><img src="https://i.imgur.com/QuQCAMH.png" title="source: imgur.com" /></a></p>

- In the Command prompt (windows key, type cmd), and type the command &quot;ipconfig /displaydns.&quot;
- This will show all the Fully Qualified Domain Names associated to VM Client-1. It will show that the DNS Servers are associated to DC-1&#39;s private IP address.

<p><a href="https://imgur.com/xyRTgLt"><img src="https://i.imgur.com/xyRTgLt.png" title="source: imgur.com" /></a></p>

<h3><strong>Task #6: Setup Remote Desktop for Non-Administrative users To Client</strong></h3>

<p>The 6th task is to setup Remote Desktop for non-administrative users to Client-1.</p>

- Log into Client-1 using the domain name and the admin account (mydomain.com\jane_admin).
- Once you have logged in, right click the Start menu and click &quot;System.&quot;
- Click &quot;Remote desktop&quot; on the right side.
- Next, click &quot;Add...&quot; and type in &quot;Domain Users.&quot; Click &quot;Check Names&quot; and click &quot;OK&quot; to exit out.

<p><a href="https://imgur.com/RuRK4AW"><img src="https://i.imgur.com/RuRK4AW.png" title="source: imgur.com" /></a></p>

<p><a href="https://imgur.com/ppNICOk"><img src="https://i.imgur.com/ppNICOk.png" title="source: imgur.com" /></a></p>

<h3><strong>Task #7: Create Users in Active Directory Using Powershell Script</strong></h3>

Seventh and final step is to use Powershell to create users. 

<p></p>

The script below creates 10 users with the password "Password1." You may edit the number of users you would like to create as well.

<p></p>


```
# ----- Edit these Variables for your own Use Case ----- #
$PASSWORD_FOR_USERS   = "Password1"
$USER_FIRST_LAST_LIST = Get-Content .\names.txt
# ------------------------------------------------------ #

$password = ConvertTo-SecureString $PASSWORD_FOR_USERS -AsPlainText -Force
New-ADOrganizationalUnit -Name _USERS -ProtectedFromAccidentalDeletion $false

foreach ($n in $USER_FIRST_LAST_LIST) {
    $first = $n.Split(" ")[0].ToLower()
    $last = $n.Split(" ")[1].ToLower()
    $username = "$($first.Substring(0,1))$($last)".ToLower()
    Write-Host "Creating user: $($username)" -BackgroundColor Black -ForegroundColor Cyan
    
    New-AdUser -AccountPassword $password `
               -GivenName $first `
               -Surname $last `
               -DisplayName $username `
               -Name $username `
               -EmployeeID $username `
               -PasswordNeverExpires $true `
               -Path "ou=_USERS,$(([ADSI]`"").distinguishedName)" `
               -Enabled $true
}
```

Go back into DC-1 and open Windows Powershell from the start menu. Right click it and "Run as administrator."


Copy and paste the script into a new Powershell console.

<p><a href="https://imgur.com/csYOeKy"><img src="https://i.imgur.com/csYOeKy.png" title="source: imgur.com" /></a></p>

<p>-Once the users have been created, go back to Active Directory Users and Computers. -Users that were created are placed into the _ EMPLOYEES organizational unit. -Choose one user (fihapi.nile) and log into Client-1 with the user. -Write down the password</p>

<p><a href="https://imgur.com/8j6Z3JH"><img src="https://i.imgur.com/8j6Z3JH.png" title="source: imgur.com" /></a></p>

<p>That's it!</p>


