# SOC-Home-Lab
Creating a virtualize SOC home lab using VMware, SecurityOnion, pfSense, Windows AD, and Kali Linux. Based on the lab created by Cyberwox Academy. This lab will be used to get hands on experience with different cybersecurity topics and tools.

First I downloaded and ran through the install wizard for VMware and collected all the ISO files required.

# pfSense Setup
pfSense is a firewall solution based on FreeBSD that will be managed via a GUI on our Kali linux machine that will be configured later in the lab.

Open VMware and click "Create a New Virtual Machine."
Click "Browse" and find the folder where the PfSense ISO is stored.

<img src="https://i.imgur.com/tl9FPaq.png" height=300px width=300px>

Click Next
Here you can rename the virtual machine I chose to name this instance "pfSense"
Click Next

Here you'll be able to allocate the amount of disk space you would like the VM to have. 20GB is enough for this instance.

<img src="https://i.imgur.com/PtkXfQN.png" height=300px width=300px>

Be sure to choose the "Split virtual disk into multiple files" option.

Click Next

Now click on the "Customize Hardware" option. Here you'll increase the memory to at least 2GB. Also add five network adapters, choosing "Custom" for each of them and labling them VMnet2 through VMnet6.

<img src="https://i.imgur.com/fiuq181.png" height=300px width=300px>

After this, the pfSense machine will start up and you will be greeted with a setup prompt. Accept all defaults and pfSense will reboot

<img src="https://i.imgur.com/RvkUbzn.png" height=300px width=300px>
<img src="https://i.imgur.com/xVqBeOm.png" height=300px width=300px>
<img src="https://i.imgur.com/cA8grBk.png" height=300px width=300px>
<img src="https://i.imgur.com/X4Q3QlT.png" height=300px width=300px>
<img src="https://i.imgur.com/2eANBEB.png" height=300px width=300px>

Once you proceed through the installation pfSense will reboot and you'll be greeted with a screen like this

<img src="https://i.imgur.com/eJ5cTRv.png" height=300px width=300px>

We'll choose option 1 to begin assinging interfaces.

"Should VLANS be set up now? [y:n]?: n"
Enter em0, em1, em2, em3, em4, em5 for each of the following questions
"Do you want to proceed? [y:n]: y"

<img src="https://i.imgur.com/LxrshQm.png" height=300px width=300px>

From here enter option 2 to set IP addresses

<img src="https://i.imgur.com/qZsRcBO.png" height=300px width=300px>

Start by configuring the LAN interface, number 2
Set the IP address as 192.168.1.1. This will be used to access the pfSense webGUI via the Kali machine that will be configured later.

Use the configuration betlow for the LAN interface

<img src="https://i.imgur.com/FTZD9fM.png" height=300px width=300px>

Use the configuration below for interface OPT1

<img src="https://i.imgur.com/9bqkEzw.png" height=300px width=300px>

Use the configuration below for interface OPT2

<img src="https://i.imgur.com/FHzwtvh.png" height=300px width=300px>

Leave the OPT3 interface without an IP address. This interface is going to have the span port traffic that Security Onion will be monitoring

Use the configuration below for interface OPT4

<img src="https://i.imgur.com/8yaxTgl.png" height=300px width=300px>

This is the end of the pfSense configuration for now. We'll continue via the webGUI after the Kali machine is set up

# Kali Machine Setup
Kali Linux will be what we use to simulate attacks against our network.
For this setup we'll use a premade Kali VM file. All you need to do is click on the .vmx file from the Kali folder after unzipping it. This will automatically load a Kali VM into VMware.

Before using the Kali machine we do need to change the network adapter to Vmnet2 and increase the memory to 4GB.

<img src="https://i.imgur.com/TOUvWcW.png" height=300px width=300px>

From here power on the VM and log in with the default credentials of "Kali" for both the username and password

Once logged in open the terminal and use the "passwd" command to change the password to something more secure

<img src="https://i.imgur.com/baxy8pU.png" height=300px width=450px>

From here open a web browser on the Kali machine and navigate to the pfSense WebConfigurator located at 192.168.1.1

You'll get a warning when trying to access but just click on the "Advanced" option and accept the risk and continue

<img src="https://i.imgur.com/JkvBpCI.png" height=300px width=550px>

Sign in using the default credentials of "admin" and "pfsense"

<img src="https://i.imgur.com/IzqViGa.png" height=300px width=400px>

You'll load into the "Wizard/pfSense Setup/" page.
Click Next until you get to step 2 of 9
Add 8.8.8.8 as your primary DNS server and 4.4.4.4 as your secondary DNS server

<img src="https://i.imgur.com/7JAfEPy.png" height=300px width=300px>

Click Next

At step 3 of 9 choose your timezone
Click Next
At step 4 of 9 uncheck the last two options

<img src="https://i.imgur.com/AIJtFxY.png" height=350px width=600px>

At step 5 of 9 click Next
At step 6 of 9 set a new Admin password

<img src="https://i.imgur.com/69Ir5l6.png" height=300px width=500px>

At step 7 of 9 click Reload
Then click Finish
At this point the pfSense wizard is complete and now changes will be made to the interfaces

Click on Interfaces

<img src="https://i.imgur.com/Nrge7QD.png" height=300px width=350px>

Select LAN
For "Description" change LAN to Kali since this is the Kali interface
Scroll all the way to the bottom and click Save
Make sure to select "Apply" at the top to make sure the changes are actually applied

<img src="https://i.imgur.com/8XVTWoC.png" height=200px width=400px>

Go through the remaining interfaces and change the descriptions of each until they all look like this

<img src="https://i.imgur.com/gD2uGco.png" height=300px width=300px>

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

This is the bulk of the pfSense configuration done.


# Security Onion Setup
Security Onion is an all-in-one IDS, Security Monitoring, and Log Management tool.

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

Now we're going to configure a quick Ubuntu Desktop as our Analyst machine. To set this up just create a new VM like usual, select Ubuntu Desktop ISO, and use the default configurations, nothing special needed. 

After quickly spinning up the Ubuntu desktop open the command prompt and use the command "ifconfig" to find out the IP address.

Head back to the Security Onion VM and run the command "sudo so-allow"
It will ask for your password
Choose option "a" and wait for the process to complete

<img src="https://i.imgur.com/I5ZdUDg.png" height=500px width=500px>

Type in the IP address for the Ubuntu desktop. This will create a firewall rule on Securit Onion that will allow web access from the Ubuntu desktop

On the Ubuntu desktop open a web browser and navigate to the Security Onion web interface IP that was confgiured earlier. 

<img src="https://i.imgur.com/vOuCznb.png" height=500px width=1500px>







# Windows AD Setup
aaaaaa
# Windows User Setup
aaaaa
