# Installing DC in Hyper V

A domain controller is a network server that is responsible for allowing the host access to domain resources. It authenticates users, stores user account information, and enforces security policy for a domain.[3] It is most commonly implemented in Microsoft Windows environments, where it is the centerpiece of the Windows Active Directory service.

In this task we created a virtual machine WindoesServer_DC that will be promoted to the role of our control server. The first process is to create a virtual machine, same process as we created a server template in our previous task. But this time we will be using the existing virtual hard disk we created earlier in our template.

 ![Installing_new_machine](https://github.com/bishwasghimire22/windowsserver/assets/144313610/a8b4a18c-4e6b-45f8-810f-fb6d90aa7528)


Next, we connect our virtual machine in our hyper V manager, that we named WindowsServer_DC.  Although I had previously initialized the sysprep tool, the system didn’t ask for any further instruction on setting language etc. So, we proceed with normal login. After logging in we configure our network settings, it is done manually from the option local server  Ethernet. The process was same when we configure our HypervvSwitch to connect to our host network. Only difference is now we connect our virtual machine to the HypervvSwitch using the network setting. After this we will be able to have internet connection in our virtual machine thru the HypervvSwitch.
 
![Network_config](https://github.com/bishwasghimire22/windowsserver/assets/144313610/bae20e23-d538-4b51-937c-d28f0d9e334f)


As seen on picture above the IP for our virtual machine is 10.208.0.10 and its default gateway is 10.208.0.1, which is the IP address of our HyperVVswitch.

After configuring our network, we need to change our firewall settings. By default, our network is public. But, since we are working in our lab environment, which is private and trusted, we change it to private. This is done in the “Local group policy editor” by changing the Network list manager policies to private from public. Alternatively, this can be done by running the command “gpedit.msc” in our terminal. Group Policy is a set of rules for Windows machines, and the Group Policy Editor is a program for editing this set of rules.
 
![Firewall_to_private](https://github.com/bishwasghimire22/windowsserver/assets/144313610/10035aa9-8b05-454d-955d-64589970bd1e)


Finally, we confirm to check if the network connection is working or not. We use “nslookup” command in our terminal to check the network connection.
 
![Network_connection_sucessfull](https://github.com/bishwasghimire22/windowsserver/assets/144313610/86fcd65b-93cb-47b9-8f3b-39b0a20df811)

After successfully testing our connection, our local server network setting looks as follow,

![Final_network_config](https://github.com/bishwasghimire22/windowsserver/assets/144313610/1a8148fb-0240-4021-81e8-2baaac7fc3fa)


After this we personalize our server by providing an appropriate name, in this case we chose my surname as the name of the computer. After this we restart our Virtual machine for all the settings to take effect.

## Windows Operating System Activation

Next, we activate the Windows operating system of our Virtual machine “WindowsSwever_DC”. This is being done in order to ensure the machines’ functionality for a long period of time. The tool slmgr.vbs is run in our terminal to start the activation process. For the Windows activation to be successful, the slmbr.vbs tool must be used to tell where the KMS activation server is located. This is done with a command slmgr /skms kms.core.windows.net.  After this we get a message from Windows Script Host notifying us the task is successful. Finally, we run the command slmgr /ato. After successful execution we get the notification saying, “Product activated successfully”.
 

![activatiing_slmgr](https://github.com/bishwasghimire22/windowsserver/assets/144313610/1056d578-1be7-427f-a1cd-0a836f1087cf)

## Installing the Control Servers software

After successfully completing all the steps described above with the Hyper V virtual machine Windows Server DC, we can now proceed to assign the role of our actual domain's control server to it. It will control the operation of all our other virtual machines in the course during later exercises. The main concepts are.

* Domain is a managed network area formed by different computers.
* Domain Controller is A server that centrally controls the operation of other computers in the domain.
* Active Directory is a centralized storage and configuration location for the specifications and operational instructions concerning domain users and “machines” on the control server. The computers in the network retrieve their own configurations from the active directory of the Domain Controller. 
* Domain Name Service (DNS) is Domain intranet name resolution. Can contain, for example, IP address information for the internal network, network names ghimire.lan and web.example.lan. The domain name server can also resolve public/external network addresses for domain machines if the network administrator allows it.

The first step is to add the role to our WindowsServer_DC server. It is done from the server manager window, under mange tab  add roles and features. In the installation wizard, we chose the following roles in the options.

* Role-based installation
* Next select this machine from the list
* In the next step, there are many roles to chose from, but we will be choosing “Active Directory Domain Services” and again choosing the “Include management tools” from the popup windows.
  
After applying the roles we have the next option of adding features, in this section we chose the default selection and continue next. In the pop-up view of the features, make sure to check the Group Policy Management features and Remote Server Administration Tools features for our Active directory domain services role.

Next windows in the installation wizard shows the general information about the AD DS in the domain. Nothing to do in this page click next.

Before installing the AD DS role, a summary is displayed. We check the option “Restart the destination server automatically if required”. This is done to restart our machine after the role of controller is added. By pressing install and accepting to restart if required we install the role. The process of installation took about 15 minutes.
 
![DC_installation final](https://github.com/bishwasghimire22/windowsserver/assets/144313610/b27d7356-dfce-4c4e-93bb-2cbf7247ff92)

## Installing Domain Controller and DNS

After the successful installation we can see a Domain service (AD DS) installed in our virtual machine’s server manager. Next step is to promote our virtual machine WindowsServer_DC to the control server role in the Hyper V network 10.208.0.0/24 area by following the steps. At the same time, the DNS service is automatically installed on the control server.

On our server manager view we click on the AD DS and open it and select the more option from the warning bar and select the option to “Promote this server to a domain controller”. This takes us to the deployment configuration wizard. We chose the option to add new forest option and enter the domain name as lastname.lan, in my case it is ghimire.lan. 

Next step we define a name server by selecting “Domain Name System (DNS) Server” and setting the Directory Services Restore Mode (DSRM) password, the password is same as the administrator password for our Hyper V machine, “Qwerty789” in this case. This is done to cause less confusion for us but in real life we won’t be having the same password because the password can only be known to the system administrator to prevent unauthorize access to our domain controller. We get a warning saying, “that no parent zone can be found for the domain being created”. The warning is unwarranted because we are creating our own main business area from scratch. Our area is therefore the specific authoritative parent zone, the absence of which is warned. We are now creating a new DNS service for this area from scratch. So, continue by clicking Next.

In the next step the NetBIOS domain name is automatically set to the first part of our root domain, in our case my surname, “GHIMIRE” all in capital letter. Next, we specify the location of the AD DS database, logfile and SYSVOL data on our control server, we choose the default setting and click next.

A summary of our configuration is shown and again we go to the next step by clicking next. After this the next step is prerequisites check, after confirming everything looks good, we can begin the installation. The computer will restart after the installation.
 
![prequisite_check](https://github.com/bishwasghimire22/windowsserver/assets/144313610/0f727571-c994-4d78-8222-83dc1c18cc08)

After the installation we got a notification saying that our Hyper V virtual machine has been set to the role of control server for your domain. The server will restart. Our Hyper V virtual machine is now "officially" a control server.
 
![DC_install_succesful](https://github.com/bishwasghimire22/windowsserver/assets/144313610/bf255c47-60c8-4497-b709-cf11ab800b90)

After restarting the machine, we can now login to our domain as a domain user. A domain user is not the same as a local computer user. The login view of every domain user connected to the domain has the format DOMAIN\User, in our case GHIMIRE\Administrator. Alternatively, we can also login by typing user@domain or Adminstrator@ghimire.lan in our case and providing the password we set up earlier. In the server Manager we can see our new control server showing the services installed on the server machine and their operating status. We have now successfully added all the essential domain control server services to your WindowsServer_DC virtual machine.
 
![DC_Dashboard](https://github.com/bishwasghimire22/windowsserver/assets/144313610/5eb4eb48-1728-4eac-bf6e-1f7eef72d384)








