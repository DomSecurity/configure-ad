<p align="center">
<img src="https://i.imgur.com/hOUBSUI.png" height="80%" width="80%" alt="Microsoft Active Directory Logo"/>
</p>


<h2>Deployment and Configuration Steps</h2>

<p>
</p>
<p>
In this lab we will create two VMs in the same VNET. One will be a Domain Controller(DC), the other will be a Client machine(Client 1). We will change the DC Private IP Address from Dynamic to a static IP Address because its offering Active Directory services to the client machine and we don't want it to change. Must have every new computer we install be able to connect to the DC at anytime. Client machine will be joined to the domain. We will configure the DNS settings on the client machine, the client machine will use the DC as its DNS server. 
</p>
<br />

<p>
<img src="https://i.imgur.com/d22FHIm.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>
<p>

<p>
In the DC, we'll change the DC Private IP Adress from Dynamic to a static IP Address
</p>
<br />

<p>
<img src="https://i.imgur.com/rutGwlg.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

<p>
In Client one, we'll change Client DNS sever to DC Private static IP Address (that does not change). In client 1 -> Network -> Network Settings -> Network Interface -> Settings -> DNS Server -> Under DNS Server, change to custom -> Add DC Private IP Address -> then Save
</p>
<br />

<p>
<img src="https://i.imgur.com/ASONaJa.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
<p>
Now we'll go into Client 1 VM. Type in search bar at bottom left corner "Powershell" to ping DC Private IP Address. Type "ping 10.0.0.4 (DC Private IP Address)" and observe it replying. Next we'll type "ipconfig /all" and observe more detailed information on the network interface, mainly to discover that the 'DNS Servers' is the same as DC Private IP Address. Which what we set it to be in the previous image.
</p>
<br />

<p>
<img src="https://i.imgur.com/1pWitRK.jpeg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

<h1> Deploying Active Directory </h1>
<br />

<p>
We'll log into DC-1, to install Active Directory Services
</p>
<br />

<p>
<img src="https://i.imgur.com/TvYsMer.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

<p>
<img src="https://i.imgur.com/vzyBkEn.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

<p>
<img src="https://i.imgur.com/cK858Nb.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

<p>
<img src="https://i.imgur.com/20BDrsd.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

<p>
Next we'll continue configuring Active Directory into a new forest as 'mydomain.com' (can be anything you want)
</p>
<br />

<p>
<img src="https://i.imgur.com/lvEwa3Q.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

<p>
<img src="https://i.imgur.com/s7JS9tr.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

<p>
<img src="https://i.imgur.com/aqRjHc7.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

<p>
Since it logged us out, we will have to now either log in as a local user or domain user. Only because now our DC is considered to be an actual Domain. So we'll now login as a user to continue.
</p>
<br />

<p>
<img src="https://i.imgur.com/nEHeYAF.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>


<p>
Now we'll create a Domain Admin user, within Active Directory Users and Computers (ADUC) create an Organizational Unit (OU) and name it '_EMPLOYEES'. Then we'll create a new OU and name it '_ADMINS'. Next create a new employee named Dom Phillips. Username will be 'dom_admin' & password 'Cyberlab123!'.  And next time we log in, we don't have to use the default login of CyberDom. Just once we add 'dom_admin' to the Domain Admins security group.
</p>
<br />

<p>
<img src="https://i.imgur.com/6ANuYtb.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

<p>
<img src="https://i.imgur.com/dPd3dTt.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

<p>
Here we are adding Dom Phillips as a Domain Admin in security groups and we'll login as 'mydomain.com\dom_admin'
</p>
<br />

<p>
<img src="https://i.imgur.com/4MS8Ofz.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>


Now lets login to Client-1 and we'll join into the domain. In System -> About -> Rename this PC (advanced) -> System Properties: Computer Name Tab -> Change -> Members of Domain: mydomain.com then login as 'mydomain.com\dom_admin' with the password you've set. Client-1 is able to locate DC only because we set Client-1 DNS to DC private IP address.
</p>
<br />

<p>
<img src="https://i.imgur.com/CRIrcQn.jpeg" height="60%" width="60%" alt="Disk Sanitization Steps"/>


Now we'll log back onto the Domain Controller as 'mydomain.com\dom_admin' and verify that Client-1 shows up in Active Directory User Computer. Which we can see here it does. 
</p>
<img src="https://i.imgur.com/bSYAwMM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<p>
Now we will create a new OU and name it '_CLIENT' just for organization purposes. Then drag Client-1 into that OU.
</p>
<br />
<img src="https://i.imgur.com/gkwGhgD.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

<h1>Setup Remote Desktop for non-administrative users on Client-1</h1>

<p>
Now we will set up remote desktop for non-administrative users on Client-1. We have to log into Client-1 as an admin and open system properties. Click on "Remote Desktop", allow "domain users" access to remote desktop. After completing those steps you should be able to log into Client-1 as a normal user.
</p>
<br />

<p>
<img src="https://i.imgur.com/WOWmI2X.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/gK9gysh.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<h1>Creating Users with PowerShell</h1>

<p>
Lastly to verify that noraml users can RDP into Client-1 we will use a script to generate thousands of users into the domain. We will input the script in powershell, after the users are created we will select one and RDP into Client-1.
</p>
<br />
<img src="https://i.imgur.com/EzWG8ug.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<p>
<p>
  <p>
<img src="https://i.imgur.com/Gkpe68K.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<img src="https://i.imgur.com/n3gMwQV.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
<p>
As you can see the Powershell script created a user with the username "bab.hubo" We were able to login to Client-1 with his credentials as a normal user. 
</p>
