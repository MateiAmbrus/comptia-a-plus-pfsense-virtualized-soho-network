# Virtual SOHO network

## Overview

This document presents a step-by-step configuration of a SOHO network in a virtual environment. It does not cover the installation of the machines or software; installation guides are linked in the "Installations" section. A video tutorial covering the same content is also available at the end of this overview.

### Project Objectives

1.  Configure the pfSense router and its necessary services (DNS, DHCP, Firewall).
2.  Connect the virtual machines to the virtual network.
3.  Verify connectivity between the machines.
4.  Isolate traffic between the virtual machines using aliases and firewall rules.

### Resources / Technologies

- VMware Workstation Pro
- pfSense router (ISO)
- Windows 11 VM (ISO)
- Ubuntu 24.04.3 VM (ISO)
- Bash (for scripting)
- PowerShell (for Windows configuration)

Full video of this project: [Watch on YouTube](https://youtu.be/IiSkOGPZcAg?si=_76tftqQx_HEKrUu)

The topology of the network is as follows:  
![NetworkDiagram2.png](../_resources/NetworkDiagram2.png)

## 1\. Configure the pfSense router and its necessary services (DNS, DHCP, Firewall).

### Connect the adapters to the pfSense router

We first have to make sure that all of the network adapters are connected to the pfSense router

![da92a12282fd9dd71d6e3437bf3f52ef.png](../_resources/da92a12282fd9dd71d6e3437bf3f52ef.png)

Add → Network adapter → Custom → VMnet(2 or 3) → OK

We power on the pfSense router and wait until it gets to the following menu

![8f1765844cb9a557eb93f07bc9fa2a95.png](../_resources/8f1765844cb9a557eb93f07bc9fa2a95.png)

### Enter the GUI of the pfSense router and add the adapters

Once here we enter the LAN IP address in our browser in order to access the pfSense GUI. Once inside the pfsense GUI we enter the username admin and the password pfsense

![d2a2db6940ede139d78f943763c125ba.png](../_resources/d2a2db6940ede139d78f943763c125ba.png)

Click on Menu → Interfaces → Assignments

Once in assignments we click "+ Add" two times to add the interfaces of our two virtual machines.

![a2d1180d89b2eb7f1e12990f56ea3bac.png](../_resources/a2d1180d89b2eb7f1e12990f56ea3bac.png)

### Configure the two interfaces

We configure the OPT1 and OPT2 interfaces to LINLAN and WINLAN respectively. Both interfaces will be enabled and LINLAN will have a DNS and DHCP address of 192.168.20.1/24 while WINLAN will have a DNS and DHCP address of 192.168.30.1/24

![ab5fe9f5daf706e41d45f4af011ac0f0.png](../_resources/ab5fe9f5daf706e41d45f4af011ac0f0.png)

![c06eeba78576a976a32cbe21d02c86a8.png](../_resources/c06eeba78576a976a32cbe21d02c86a8.png)

![f56edf8cb8d6049db92abdf2c73b07ed.png](../_resources/f56edf8cb8d6049db92abdf2c73b07ed.png)

![b4d8f4e4e983cbdb832e19eca45b7351.png](../_resources/b4d8f4e4e983cbdb832e19eca45b7351.png)

Save and apply

### Enable the DNS resolver for both LANs

Menu → Services → DNS Resolver → Enable DNS resolver → Enable DNSSEC Support → Check "Register DHCP leases in the DNS Resolver"

![f4caddac5f7a0d7cbed333cb7c007c6e.png](../_resources/f4caddac5f7a0d7cbed333cb7c007c6e.png)

![3ca75d69a3dc5cbc51f2989f10158e64.png](../_resources/3ca75d69a3dc5cbc51f2989f10158e64.png)

![9cba2d302346815325d7fe4f3cf48272.png](../_resources/9cba2d302346815325d7fe4f3cf48272.png)

Save and Apply

### Enable DHCP for both LAN's

Menu → Services → DHCP Server

Inside this page we will do the same process for both LINLAN and WINLAN, the only difference being that the address pool for LINLAN will be 192.168.20.100 - 192.168.20.200 while for WINLAN the address pool will be 192.168.30.100 - 192.168.30.200

![0ac4e8822292d2eb45a5c629e788061e.png](../_resources/0ac4e8822292d2eb45a5c629e788061e.png)  
![e28c81668b77d91aa567d315bb53967b.png](../_resources/e28c81668b77d91aa567d315bb53967b.png)

![00ccc3f02475d6cd2a8b237618d59566.png](../_resources/00ccc3f02475d6cd2a8b237618d59566.png)  
![a638a310c355fde0f803b9dde3c402d9.png](../_resources/a638a310c355fde0f803b9dde3c402d9.png)

Save and Apply

### Create Initial Firewall rules

Menu → Firewall → Rules

Inside this page we'll go to both LINLAN and WINLAN and create a rule that allows all traffic.

![d43f6d8bfee1ad3a75520aea50deba12.png](../_resources/d43f6d8bfee1ad3a75520aea50deba12.png)

![25c06e9c6e850df0908f51364c4aacf3.png](../_resources/25c06e9c6e850df0908f51364c4aacf3.png)

Save and Apply

### Check the configuration

Restart the pfSense virtual machine. When it's running again it will show the following information.

![f50ecf6b9cb309a5276e373a57c65336.png](../_resources/f50ecf6b9cb309a5276e373a57c65336.png)

## 2\. Connect the virtual machines to the virtual network.

Firstly we make sure that the two machines have the correct network adapter. For the Ubuntu 24.04.3 (Linux) machine it will be VMnet2 adapter while for the Windows 11 machine it will be the VMnet3 adapter.

![963d56bfa82cd4a9d073b77c6cb1e3bb.png](../_resources/963d56bfa82cd4a9d073b77c6cb1e3bb.png)

![69abba733fc778f942ff5ac7c6a34ecb.png](../_resources/69abba733fc778f942ff5ac7c6a34ecb.png)

Start both machines

## 3\. Verify connectivity between the machines.

After opening the machines we co into the network settings menu to see the IP address, DNS address and DHCP address.

Network settings of the Ubuntu 24.04.03 machine  
![e53bdf1ac6dd918c6bc4fa4aaa98f8dc.png](../_resources/e53bdf1ac6dd918c6bc4fa4aaa98f8dc.png)

Network settings of the Windows 11 machine  
![f3f5474e57688fc416f6bdb9e82971a5.png](../_resources/f3f5474e57688fc416f6bdb9e82971a5.png)

We can also use the terminal to see the same information

PowerShell: `ipconfig /all`  
![1e39fbfd46b31510fbdbdf63500a858f.png](../_resources/1e39fbfd46b31510fbdbdf63500a858f.png)

Bash: `ip addr show`  
![5e9ae92f4684c8bc9f9b32bb164301b7.png](../_resources/5e9ae92f4684c8bc9f9b32bb164301b7.png)

To test the connection between the machines and the internet we run the command `ping 8.8.8.8` on the Windwos 11 machine and the command `ping -c4 8.8.8.8` on the Ubuntu machine.

Bash: `ping -c4 8.8.8.8`  
![62ea4f676872c2c108ce329aa2475532.png](../_resources/62ea4f676872c2c108ce329aa2475532.png)

PowerShell: `ping 8.8.8.8`  
![57c0e08e5ba769dbfbdef72e5d3bd37a.png](../_resources/57c0e08e5ba769dbfbdef72e5d3bd37a.png)

We can also test the connection between the DNS/DHCP server and the machines by pinging them

PowerShell: `ping 192.168.30.1`  
![085de5db820945f35fabd42b0e0df8f4.png](../_resources/085de5db820945f35fabd42b0e0df8f4.png)

Bash: `ping -c4 192.168.20.1`  
![cfc8c15edae0f823b3bc4b5c72c6503b.png](../_resources/cfc8c15edae0f823b3bc4b5c72c6503b.png)

We can see that the the machines can communicate with each other locally (on the LAN network) by pinging the other machines DNS or IP address.

Bash: `ping -c4 192.168.30.1`  
![995a773c251b8ddd1d40634e7e75f2a9.png](../_resources/995a773c251b8ddd1d40634e7e75f2a9.png)

PowerShell: `ping 192.168.20.101`  
![bccf0273561886b6481e5f688f61b5e3.png](../_resources/bccf0273561886b6481e5f688f61b5e3.png)

## 4\. Isolate traffic between the virtual machines using aliases and firewall rules.

### Create the alias

Inside of pfSense GUI

Menu → Firewall → Aliases → IP → Click on "+ Add" → Create the alias

The alias will target all of the private IP addresses that exist.

![84317b0d4e32d1f5690c0d98f09e76a3.png](../_resources/84317b0d4e32d1f5690c0d98f09e76a3.png)

### Create the firewall rules

Menu → Firewall → Rules

Inside this page we'll go to LINLAN and then WINLAN. Both will have the same two rules however the rules will be configured such that it targets their lan.

First rule allows internet traffic:

Protocol = IPv4 TCP/UDP  
Source = LINLAN subnets / WINLAN subnets  
Port = \*  
Destination = LINLAN address / WINLAN address  
Port = \*  
Gateway = \*  
Queue = none

The second rule disallows all local traffic between the machines

Protocol = IPv4 \*  
Source = LINLAN subnets / WINLAN subnets  
Port = \*  
Destination = !PrivateIPs  
Port = \*  
Gateway = \*  
Queue = none

The exclamation mark at the beginning of the "PrivateIPs" alias (the value for the destination of the second rule), represents that it is an inverted match meaning, instead of allowing it disallows.

![e720d4edeb1a130497a9347b15b949a1.png](../_resources/e720d4edeb1a130497a9347b15b949a1.png)

Save and Apply

After we have set up the firewall rules we test them by running the same ping commands we have run earlier. For the pings inside the LAN the response we should get is 100% packet lose. For the pings towards the google DNS (8.8.8.8) the response should be the same.

Bash: `ping -c4 192.168.30.1`  
![d3a1db4ab1b595c2d30457d533d552df.png](../_resources/d3a1db4ab1b595c2d30457d533d552df.png)

PowerShell: `ping 192.168.20.101`  
![d69ca4f70a3131e59de48f0248eb4e00.png](../_resources/d69ca4f70a3131e59de48f0248eb4e00.png)

Bash: `ping -c4 192.168.30.1`  
![995a773c251b8ddd1d40634e7e75f2a9.png](../_resources/995a773c251b8ddd1d40634e7e75f2a9.png)

PowerShell: `ping 192.168.20.101`  
![bccf0273561886b6481e5f688f61b5e3.png](../_resources/bccf0273561886b6481e5f688f61b5e3.png)