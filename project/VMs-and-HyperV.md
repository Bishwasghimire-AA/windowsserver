# Hyper-V Installation

Although I forgot to record installing a Hyper-V Manager, here I am showing and explaining how I configured a Hyper-V Manager for me to get ready for my own Lab environment. This exercise is based on Microsoft Azure Lab Services. As a student I have all the access to free services.
The first step for creating your own virtual network environment is creating a virtual switch manager, whose main task is to control network traffic between virtual(client) machines to run inside our virtual environment. After creating a Virtual network switch a virtual network card has been installed in the Lab.  

 ![VirtualVVswitch_connection](https://github.com/bishwasghimire22/windowsserver/assets/144313610/b66860c0-6695-4348-ae9d-26d4162dcecb)

![Power_Shell_Get-NetAdapter](https://github.com/bishwasghimire22/windowsserver/assets/144313610/686fb2b7-c731-4c4d-815e-92c2a3d18a14)

After successfully installing a Virtual switch, we need to define an IP address to our Virtual machine(host), although the devices within the same LAN can communicate with each other without the IP address setting, it is done to communicate with the outside network later. We can always check information on our Network card by running this command.
The purpose of IPv4 address is to help communication between the devices within the network quickly and effectively. Every IP address need to unique, meaning they shouldn’t be same.


![IPv4_conig_adress](https://github.com/bishwasghimire22/windowsserver/assets/144313610/8d375355-317c-4a54-8b70-55c7003e1af2)

![IPv4_config](https://github.com/bishwasghimire22/windowsserver/assets/144313610/f3187d42-853c-4934-b458-632e81a920ae)

As assigned for the Lab environment the IP address is configured as mentioned in figure above. After successfully configuring an IP address for our virtual network the next step is to configure a virtual Network Address Translation (NAT) for our environment. This configuration is done only after the above process of creating virtual switch and assigning a virtual IP address. The purpose of NAT is to run the connection smoothly in all the devices or machines or clients we add in our Lab environment later. NAT configuration is implemented from the PowerShell command. The NAT configuration enables effective traffic between our incoming and outgoing packages/messages.
 
![NAT_config_Success](https://github.com/bishwasghimire22/windowsserver/assets/144313610/ec7be819-65b6-4a16-93eb-bb10c52fd344)

During the configuration I got an error message which was caused by the wrong command as highlighted above. This is where this week ends, with configuring our own virtual LAN using Microsoft Azure Lab Services.

Things I learned from this chapter on Hyper V and VMs
•	Installing a Hyper-V manager in my virtual machine
•	Setting up a virtual switch for my virtual Local Area Network(vLAN)
•	Assigning an IPv4 address and subnet masks for my vLAN.
•	Configuring NAT, for smooth connection of my machines, and to connect to inbound and outbound network traffic.
