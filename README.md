# SOC-Home-Lab
Creating a virtualize SOC home lab using VMware, SecurityOnion, pfSense, Windows AD, and Kali Linux. Based on the lab created by Cyberwox Academy. This lab will be used to get hands on experience with different cybersecurity topics and tools.

First I downloaded and ran through the install wizard for VMware and collected all the ISO files required


# Downloads
[VMware Player](https://www.vmware.com/products/workstation-player/workstation-player-evaluation.html)

[pfSense ISO](https://www.pfsense.org/download/)

[Kali ISO](https://www.kali.org/get-kali/#kali-virtual-machines)

[Security Onion](https://github.com/Security-Onion-Solutions/securityonion/blob/master/VERIFY_ISO.md)

[Ubuntu Desktop ISO](https://ubuntu.com/download/desktop)

[Ubuntu Server ISO](https://ubuntu.com/download/server)

[Windows Server 2019 ISO](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2019)

[Windows 10 ISO](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-10-enterprise)

[Splunk Trial and Universal Forwarder](https://www.splunk.com/en_us/download/splunk-cloud.html)

You don't need to download Splunk separately you can download it from the Ubuntu server as seen later in the lab


# pfSense Setup
pfSense is a firewall solution based on FreeBSD that will be managed via a GUI on our Kali linux machine that will be configured later in the lab

Open VMware and click "Create a New Virtual Machine."

Click "Browse" and find the folder where the PfSense ISO is stored

<img src="https://i.imgur.com/tl9FPaq.png" height=500px width=500px>

Click Next

Here you can rename the virtual machine I chose to name this instance "pfSense"

Click Next

Here you'll be able to allocate the amount of disk space you would like the VM to have. 20GB is enough for this instance.

<img src="https://i.imgur.com/PtkXfQN.png" height=500px width=500px>

Be sure to choose the "Split virtual disk into multiple files" option.

Click Next

Now click on the "Customize Hardware" option. Here you'll increase the memory to at least 2GB. Also add five network adapters, choosing "Custom" for each of them and labling them VMnet2 through VMnet6

<img src="https://i.imgur.com/fiuq181.png" height=500px width=500px>

After this, the pfSense machine will start up and you will be greeted with a setup prompt. Accept all defaults and pfSense will reboot

<img src="https://i.imgur.com/RvkUbzn.png" height=500px width=500px>

<img src="https://i.imgur.com/xVqBeOm.png" height=500px width=500px>

<img src="https://i.imgur.com/cA8grBk.png" height=500px width=500px>

<img src="https://i.imgur.com/X4Q3QlT.png" height=500px width=500px>

<img src="https://i.imgur.com/2eANBEB.png" height=500px width=500px>

Once you proceed through the installation pfSense will reboot and you'll be greeted with a screen like this

<img src="https://i.imgur.com/eJ5cTRv.png" height=500px width=500px>

We'll choose option 1 to begin assinging interfaces

"Should VLANS be set up now? [y:n]?: n"

Enter em0, em1, em2, em3, em4, em5 for each of the following questions

"Do you want to proceed? [y:n]: y"

<img src="https://i.imgur.com/LxrshQm.png" height=500px width=500px>

From here enter option 2 to set IP addresses

<img src="https://i.imgur.com/qZsRcBO.png" height=500px width=500px>

Start by configuring the LAN interface, number 2

Set the IP address as 192.168.1.1. This will be used to access the pfSense webGUI via the Kali machine that will be configured later

Use the configuration betlow for the LAN interface

<img src="https://i.imgur.com/FTZD9fM.png" height=500px width=500px>

Use the configuration below for interface OPT1

<img src="https://i.imgur.com/9bqkEzw.png" height=500px width=500px>

Use the configuration below for interface OPT2

<img src="https://i.imgur.com/FHzwtvh.png" height=500px width=500px>

Leave the OPT3 interface without an IP address. This interface is going to have the span port traffic that Security Onion will be monitoring

Use the configuration below for interface OPT4

<img src="https://i.imgur.com/8yaxTgl.png" height=500px width=500px>

This is the end of the pfSense configuration for now. We'll continue via the webGUI after the Kali machine is set up


# Kali Machine Setup
Kali Linux will be what we use to simulate attacks against our network and access the WebUI for pfSense

For this setup we'll use a premade Kali VM file. All you need to do is click on the .vmx file from the Kali folder after unzipping it. This will automatically load a Kali VM into VMware.

Before using the Kali machine we do need to change the network adapter to Vmnet2 and increase the memory to 4GB.

<img src="https://i.imgur.com/TOUvWcW.png" height=500px width=500px>

From here power on the VM and log in with the default credentials of "Kali" for both the username and password

Once logged in open the terminal and use the `passwd` command to change the password to something more secure

<img src="https://i.imgur.com/baxy8pU.png" height=500px width=650px>

From here open a web browser on the Kali machine and navigate to the pfSense WebConfigurator located at 192.168.1.1

You'll get a warning when trying to access but just click on the "Advanced" option and accept the risk and continue

<img src="https://i.imgur.com/JkvBpCI.png" height=300px width=550px>

Sign in using the default credentials of "admin" and "pfsense"

<img src="https://i.imgur.com/IzqViGa.png" height=300px width=400px>

You'll load into the "Wizard/pfSense Setup/" page

Click Next until you get to step 2 of 9

Add 8.8.8.8 as your primary DNS server and 4.4.4.4 as your secondary DNS server

<img src="https://i.imgur.com/7JAfEPy.png" height=500px width=500px>

Click Next

At step 3 of 9 choose your timezone

Click Next

At step 4 of 9 uncheck the last two options

<img src="https://i.imgur.com/AIJtFxY.png" height=350px width=800px>

At step 5 of 9 click Next

At step 6 of 9 set a new Admin password

<img src="https://i.imgur.com/69Ir5l6.png" height=300px width=800px>

At step 7 of 9 click Reload

Then click Finish

At this point the pfSense wizard is complete and now changes will be made to the interfaces

Click on Interfaces

<img src="https://i.imgur.com/Nrge7QD.png" height=300px width=350px>

Select LAN

For "Description" change LAN to Kali since this is the Kali interface

Scroll all the way to the bottom and click Save

Make sure to select "Apply" at the top to make sure the changes are actually applied

<img src="https://i.imgur.com/8XVTWoC.png" height=200px width=800px>

Go through the remaining interfaces and change the descriptions of each until they all look like this

<img src="https://i.imgur.com/gD2uGco.png" height=300px width=500px>

OPT3 will need the "Enable Interface" option checked as shown below

<img src="https://i.imgur.com/RZkHvf9.png" height=300px width=500px>

Now go back to the Interfaces Assignment tab and click on Bridges

Click Add

<img src="https://i.imgur.com/44qA1G7.png" height=300px width=600px>

Select VICTIMNETWORK as the member interface

<img src="https://i.imgur.com/GolIpI2.png" height=300px width=600px>

Then select Displace Advanced

Under Advanced Configuration for Span Port, select SPANPORT

<img src="https://i.imgur.com/ASlVM3b.png" height=300px width=600px>

Scroll to the end and click Save and then click Apply at the top of the page

Now click on Firewall and click on the Rules option

Click on the Add button with the arrow pointing downwards

<img src="https://i.imgur.com/QAjYguH.png" height=300px width=600px>

Under the "edit Firewall Rule" for Protocol select ANY

Scroll to the bottom to click Save and then back to the top to click Apply

<img src="https://i.imgur.com/Q5sDAEp.png" height=250px width=1500px>

Add another rule under the VICTIMNETWORK by clicking the arrow pointing down

Again set Protocol, Source, and Destination all to ANY

Click Save and Apply

This rule will allow our Domain Controller to access the internet later on

This is the bulk of the pfSense configuration done


# Security Onion Setup
Security Onion is an all-in-one IDS, Security Monitoring, and Log Management tool

Create a new VM

Select the Security Onion ISO and click Next

<img src="https://i.imgur.com/UYmDEwK.png" height=300px width=300px>

Choose Linux for guest OS and CentOS 8 64-bit for for the Version

<img src="https://i.imgur.com/iCJwHOf.png" height=300px width=300px>

Click Next

Specify the disk size(minimum 200GB) and for this VM choose the store as a single file option then click Next

Now click "Customize Hardware" and change the memory to between 4 and 32 GB and add two Network Adapters and assign the Vmnet 4 and Vmnet5

<img src="https://i.imgur.com/vAgwMdd.png" height=400px width=400px>

CLick Finish

Power on the VM and click Enter when prompted

<img src="https://i.imgur.com/mauj4Ee.png" height=500px width=500px>

Type "yes"

<img src="https://i.imgur.com/ufTk91r.png" height=500px width=500px>

You'll be prompted to set a username and password and then Security Onion will reboot

Once rebooted, enter your username and password and select "Yes"

<img src="https://i.imgur.com/58WK0l9.png" height=500px width=500px>

Click Enter to install

<img src="https://i.imgur.com/0dayAnz.png" height=500px width=500px>

Select the EVAL option

<img src="https://i.imgur.com/s1YJep7.png" height=500px width=500px>

Type "AGREE"

<img src="https://i.imgur.com/admFatk.png" height=500px width=500px>

Select "Standard"

<img src="https://i.imgur.com/PugqUe1.png" height=500px width=500px>

Click the spacebar and select the first option as the management interface, ens160 in my case

<img src="https://i.imgur.com/AlpLtXw.png" height=500px width=500px>

Set the address to DHCP

<img src="https://i.imgur.com/iuxTySN.png" height=500px width=500px>

Select "Yes"

Select "OK"

Select "Direct" for connecting to the internet

<img src="https://i.imgur.com/t2yM88E.png" height=500px width=500px>

Select the second interface as the Monitor interface, ens224 in my case

<img src="https://i.imgur.com/Btl09l1.png" height=500px width=500px>

Select "Automatic"

Accept the default home network IP

Accept all the defaults

Enter email and password for the admin account

Select IP for "How would you like to access the web interface"

<img src="https://i.imgur.com/nnoins9.png" height=500px width=500px>

Select "Yes" for the NTP server adn accecpt the defualts

Finally you'll see a review screen of what settings were chosen, be sure to write down the web access IP

<img src="https://i.imgur.com/jjumLgx.png" height=500px width=500px>

Select "Yes"

Now we're going to configure a quick Ubuntu Desktop as our Analyst machine. To set this up just create a new VM like usual, select Ubuntu Desktop ISO, and use the default configurations, nothing special needed

After quickly spinning up the Ubuntu desktop open the command prompt and use the command `ifconfig` to find out the IP address

Head back to the Security Onion VM and run the command `sudo so-allow`

It will ask for your password

Choose option "a" and wait for the process to complete

<img src="https://i.imgur.com/I5ZdUDg.png" height=500px width=500px>

Type in the IP address for the Ubuntu desktop. This will create a firewall rule on Securit Onion that will allow web access from the Ubuntu desktop

On the Ubuntu desktop open a web browser and navigate to the Security Onion web interface IP that was confgiured earlier

<img src="https://i.imgur.com/vOuCznb.png" height=500px width=1500px>


# Windows AD Setup
Using a copy of Windows Server 2019 we'll create an Active Directory(AD) domain with the server as the Domain Controller and two Windows 10 machines as users

For all Windows machines in this lab we'll not be using a product key so you can simply press Next when prompted for one

For all Windows machines make sure to go to "Edit virtual machine settings: and remove the Floppy drive after they have been installed

Add the end of the installation change the Network Adapter to Vmnet3

Again, start by creating a new virtual machine using the Windows Server 2019 ISO and keep all the default configurations except selecting Vmnet3 as the Network Adapter

Once the machine spins up we'll be greeted with the installation wizard

<img src="https://i.imgur.com/IFzswku.png" height=500px width=500px>

Select the "Windows Server 2019 Evaluation (Desktop Experience) from the list of OS available

<img src="https://i.imgur.com/Dc8BPGW.png" height=500px width=500px>

Accept the License Agreement and click Next

Select Custom Install

Click New

Click Apply

Click Ok

Click Next

Now when the installation is finished you should be prompted to set a password for the Administrator account

<img src="https://i.imgur.com/WnIsoJq.png" height=500px width=500px>

Once done you will be at the Server Manager homescreen

<img src="https://i.imgur.com/SRTPdPU.png" height=500px width=500px>

Now to add Active Directory Services we need to navigate to Manage > Add Roles and Features

<img src="https://i.imgur.com/aZij7yG.png" height=500px width=500px>

Go to Server Roles and click the "Active Directory Domain Services" option and click Add Features

<img src="https://i.imgur.com/s00jdgC.png" height=500px width=500px>

When you get to the Confirmation tab click Install

<img src="https://i.imgur.com/gSh9LOl.png" height=500px width=500px>

Click on the yellow caution flag on the homepage

<img src="https://i.imgur.com/ZYHWNz0.png" height=500px width=500px>

Select "Promote this server to a domain controller"

Select Add a new forest

Specify a domain name

Click Next

Set a Password

<img src="https://i.imgur.com/Mtrn9H1.png" height=500px width=500px>

Click Next until you get to the Prerequisites Check Menu

Click Install

Wait for the machine to Reboot

After the reboot, log back in

Select Manage > Add Roles and Features again

Click Next until you get to Server Roles

Select Active Directory Certificate Services

Select Add Features

<img src="https://i.imgur.com/6cKS4Ux.png" height=500px width=500px>

Click Next until you get to the Confirmation menu

Check "Restart the destination server automatically if required"

Click Yes

Click Install

After the installation click Close

Again, click the yellow caution flag

<img src="https://i.imgur.com/pTdD1pL.png" height=500px width=500px>

Select "Configure Active Directory Certificate Services on the destination server"

Click Next on Credentials

On the Role Services menu, check Certification Authority

<img src="https://i.imgur.com/HjjRDQq.png" height=500px width=500px>

Click Next until the Validity Period menu and change it to 99 year

<img src="https://i.imgur.com/WwX2WWv.png" height=500px width=500px>

Click Next until the Confirmation menu

Then select Configure and manually resart the server for the settings to take effect

Now we'll add some users

Go to the Tools tab and select Active Directory Users and Computers

Select your Domain Name > Users, right click and select New > User

<img src="https://i.imgur.com/lfHOrwr.png" height=500px width=500px>

<img src="https://i.imgur.com/jIHeecy.png" height=500px width=500px>

Enter a First, Last, and Username for the user

<img src="https://i.imgur.com/vs6IoGu.png" height=500px width=500px>

Set a password that never expires and click Finish

Right click on the previously created user, select Copy, and create another user and follow the same steps and you're done with user creation

Now we're going to turn off Windows Defender Firewall

Search for "Windows Defender Firewall" > Turn Windows Defender Firewall on or off

Turn it off for all Networks

<img src="https://i.imgur.com/0l2ce7A.png" height=500px width=500px>

<img src="https://i.imgur.com/sw1npdg.png" height=500px width=500px>

Now we're going to configure pfSense as the default gateway for the Domain Controller

Navigate to the Control Panel > Network and Internet > Network Connections

Enter the following configurations for IPv4

<img src="https://i.imgur.com/kuMDMvX.png" height=500px width=500px>

This is the end of the Domain Controller and AD configuration, next we'll configure the user PC's for the users we created earlier


# Windows User Setup
Before setting up our user accounts we must make sure our DHCP Server for our AD is set up. To do this load into the Kali machine and go to the pfSense webGUI

Go to Services > DHCP Server > VICTIMSNETWORK > DNS Server and set it to the IP address of your domain controller, 192.168.2.4 for me

Go to Services > DHCP Server > VICTIMSNETWORK > Other Options > Domain Name and set it to the name of your domain controller, SOC.lab for me

Go to the bottom of the page to save and then hit Apply at the top of the page

<img src="https://i.imgur.com/2VqY0HW.png" height=500px width=500px>

Now to start configuring the user machines, create a new VM, use the Windows 10 Enterprise ISO, enter the name of the user account, and then leave all the defaults the same except make sure the Network Adapter is set to Vmnet3

<img src="https://i.imgur.com/z68ewNF.png" height=500px width=500px>

Uncheck all the privacy settings for the device

<img src="https://i.imgur.com/McXVcrr.png" height=500px width=500px>

Once the VM is booted up you can go to the search bar and type in PC name which will bring you to the "About" section where you can change the name of the PC. I changed the first user PC name to "Cameron" so that it will be easier to track in the AD

After that's all done go back to the search bar and type in "Domain." This will bring up the "Acess Work and School" option.

Click on "Connect" > "Join this Device to Local Active Directory Domain"

Type in the name of your AD, SOC.lab in my case

<img src="https://i.imgur.com/Qsn1RPH.png" height=500px width=500px>

You'll then be prompted for a username and password use "Administrator" and the admin password you use to get into the AD

<img src="https://i.imgur.com/iIoUyyh.png" height=500px width=500px>

Click "Skip" and Restart the PC

Once rebooted you can see that the computer is part of the AD

<img src="https://i.imgur.com/TtTMi7I.png" height=500px width=500px>

You can see this from the AD as well by going to "Active Directory Users and Computers"

<img src="https://i.imgur.com/caAN04v.png" height=500px width=500px>

Now just repeat this process for the other User you created when you set up the Active Directory machine


# Splunk Install

For the Splunk install we're going to use a copy of Ubuntu Server. Go through the standard installation, 4GB memory, 100GB HDD

<img src="https://i.imgur.com/TOkBrjr.png" height=500px width=500px>

Go through the setup wizard using all the defaults and create a user and name the server, in my case I named it "splunk"

<img src="https://i.imgur.com/ht8oH7V.png" height=500px width=500px>

Once the install is complete it will ask you to remove the ISO disk and reboot. When you do you'll sign in and be greeted by a screen like this

<img src="https://i.imgur.com/6klmMw4.png" height=500px width=500px>

From here we'll install a GUI to make our life easier

First we'll have to install tasksel by running the command

  `sudo apt install tasksel`

Now enter the command

`sudo tasksel`

This will bring up the following menu and check the Ubuntu Desktop option

<img src="https://i.imgur.com/K0x08v1.png" height=500px width=500px>

Hit "Ok" and it will begin installing the packages needed

Once installed you'll have to reboot the machine using the command

`reboot`

Now we'll have a GUI like this and we can proceed with setting up Splunk

<img src="https://i.imgur.com/DUjfL46.png" height=500px width=500px>

On the server open a web browser and navigate to www.splunk.com 

Click on "Free Splunk" in the top right corner and create an account and login

Go to the downloads and select the Linux .tgz download

Open the terminal and navigate to the Downloads directory

<img src="https://i.imgur.com/jSMUiw7.png" height=500px width=500px>

Untar the file using the following command

`tar xvzf`

<img src="https://i.imgur.com/0OdaqSj.png" height=150px width=2500px>

Navigate to the ~/splunk/bin directory and issue the command

`./splunk start`

<img src="https://i.imgur.com/Fi7Bh6N.png" height=500px width=500px>

This will prompt you for a username and password then show you the IP address Splunk can be reached at on the server

Navigate to IP:8000 in the browser and login with the username and password you just set. Once you do you'll be at the homescreen of the Splunk instance

Splunk uses Universal Forwarders to receive data from endpoints so we'll be setting one up for our Windows Domain Controller

We'll have to start by setting up a receiving port for Splunk to listen on

To do this we navigate to Settings > Forwarding and Receiving > New Receiving Port. We're going to set this as the default port of 9997

<img src="https://i.imgur.com/V0Wo3za.png" height=500px width=500px>

<img src="https://i.imgur.com/RPScmxq.png" height=500px width=500px>

Now we're going to configure a new index. We do this by navigating to Settings > Indexes > New Index

<img src="https://i.imgur.com/CJe07Mv.png" height=500px width=500px>

<img src="https://i.imgur.com/YRjMmuz.png" height=500px width=500px>

We'll call this index "wineventlog"

Now head over to the Windows Server and download the Universal Forwarder from the Splunk website

Once downloaded run the file and follow the installation Wizard, create a username and password

<img src="https://i.imgur.com/1khC6ur.png" height=500px width=500px>

We'll need the IP of our Ubuntu server to configure the Deployment Server so head back there, open a terminal and enter the command

`ifconfig`

<img src="https://i.imgur.com/9M9yTJZ.png" height=500px width=650px>

Back to the forwarder setup, copy the IP of the Ubuntu server and use the default port of 8089

<img src="https://i.imgur.com/V8pNmkH.png" height=500px width=500px>

Next we'll do the same thing for the Receiving Indexer but using the default port of 9997

<img src="https://i.imgur.com/FdxtlGH.png" height=500px width=500px>

Now that the Universal Forwarder is configured on the Windows Server head back to the Splunk instance

Navigate to Settings > Add Data > Forward

<img src="https://i.imgur.com/2beZblv.png" height=500px width=650px>

The Domain Controller should automatically populate in the "Select Forwarders" tab, select it, and name the server class

<img src="https://i.imgur.com/Bt3DDJO.png" height=500px width=650px>

Next we'll add the event logs we'd like to import. Usually you'd be more selective in a production environment but for this lab we'll just select all of them

<img src="https://i.imgur.com/eFHlmgU.png" height=500px width=650px>

For Input Settings we'll select the "wineventlog" index we created earlier

<img src="https://i.imgur.com/0eL6Tqj.png" height=500px width=650px>

At the Review tab we'll see everything we've configured

<img src="https://i.imgur.com/yZfIyus.png" height=500px width=650px>

And finally we can click the "Start New Search" button and we'll see logs being ingested into Splunk

<img src="https://i.imgur.com/G5mjB9N.png" height=500px width=650px>

# Conclusion
This is the end of lab creation, from here we can start using the Kali machine to lauch attacks against the Windows enviornment and get some red team experience or craft rules for Splunk and Security Onion to log and monitor and harden our Windows environment against attacks.
