# Installing File Server in Hyper V

In this task we will be installing a file server named WindowsServer_FileServer which will be the file server of our Windows domain we created in earlier task. Its role in our new network domain is to distribute network folders to other computers in the domain.

The installation and initial configuration follow the same pattern as the creation of the control server. The only difference being the Virtual hard disk assignment in this case we assign the virtual hard disk called WindowsServer_FileServer. vhdx located in our desktop which was created on our very first task. 

The other difference then the control server is the Network setting where we give this file server a Static Ip address 10.208.0.12 and the DNS server address as 10.208.0.10, Which is the Ip address of our Control server. This is done so our file server VM is connected to the domain controller we created in the previous chapter. Also, the password for logging in is different for the file server.
 
![New_VM](https://github.com/bishwasghimire22/windowsserver/assets/144313610/00884115-8748-4bdf-baf8-1d16c465a186)

Although we had run the sysprep task in earlier during the template creation process, the machine booted to administrator login page straight. So, as instructed we are going to run a sysprep tool first. When running the sysprep program the WindowsServer_DC Vm is disconnected. After successfully running the sysprep program by running the command “C:\Windows\System32\Sysprep\sysprep.exe /generalize /shutdown” in our command prompt we come to the following page in our virtual machine
 
![Sysprep_new VM](https://github.com/bishwasghimire22/windowsserver/assets/144313610/1fee7ae8-c114-4321-ab59-81b4d4cdcb45)


Next, we configure the Network setting of our File Server Virtual machine. The process is same as creation of the Domain controller except we assign a static IP address for our Vm and give the DNS server IP as the IP of our Domain controller. And the default gateway here is the IP of our HyperVVswitch. And now we have a connection for our virtual machine.

![File_server_network](https://github.com/bishwasghimire22/windowsserver/assets/144313610/43898933-5f01-4687-8f63-7c6e928c2172)


After this we go through the steps of renaming our Machine, set the control servers lock screen off in the Local Group Policy Editor (gpedit.msc (C:\Windows\System32\gpedit.msc)) and activation our Windows operating system by running the tool slmgr.vbs on our command prompt. This task was done and reported during the installation of our WindowsServer_DC machine.

## Joining our File server to our Domain

After successfully activating Windows operating system in our Virtual machine, we now need to join this machine, WindowsServer_FileServer to our domain WindowsServer_DC. Both our virtual machines should be running in our Hyper V manager before the process can begin. This process is done by changing our WORKGROUP to be the member of our domain ghimire.lan. After selecting our domain as member, we need to put our Administrator Username and password, in our case the username is GHIMIRE\Administrator or Administrator@ghimire.lan and password is Qwerty789. It takes a while until Windows reports that the domain was successfully joined. 

Since we have previously configured our DNS server with the IP address of our control server during the network setting, the DNS server software knows where the control server for the domain ghimire.lan is located. The control server, in turn, checks that the computer sending the connection request has the technical requirements like the IP address, username and passwords to join the domain. When all the conditions are met, the computer can be connected to the domain.
 

![Domain connection](https://github.com/bishwasghimire22/windowsserver/assets/144313610/c8d66968-bd80-42c8-8d66-527ed21e5cf7)

![Domain_connection_sucess](https://github.com/bishwasghimire22/windowsserver/assets/144313610/9771f5aa-ae5b-4cd9-adb7-e664bea3c5c5)


After successfully joining our file server to our domain. The machine restarts and we login as administrator. 

## Installing File Service role

After being able to log in successfully as Administrator, we install the role of file service to our WindowsServer_FileServer machine. This task is done in two stages, in the first stage we add the file server as managed server and in the next stage we the actual role of file server is installed using our control server.

In the first stage the File Server is added to the list of managed servers in our control server. This is done to be able to mange the file server’s configuration remotely, such as installing services on it. We first login to our WindowServer_DC and open Server Manager. Under the manage tab we ADD servers and under the active directory we name our server. This server is found in the Active directory database of our control server.
 
![Adding_file_server_to DC](https://github.com/bishwasghimire22/windowsserver/assets/144313610/100e3d11-545f-4cc8-90ef-3fbb3f6aa284)


At this point our File server is added to the list of domain servers known computers in our control server. Now we can fully manage our file server from our control server.

In the next stage the file service role is added remotely from our control server to our WindowsServer_Fileserver. The main purpose is to add the WindowsServer_FileServer core role to our file server to enable file and network sharing between our Virtual machines that we will connect to the future as our workstations.

We now go to our Server manager dashboard in our WindowsServer_DC and choose All servers and click on the file server we had added on previous stage. From the manage option choose the Add roles and features option. This takes us to the adding windows role installation wizard. During this installation process we select our server from the server pool and install the roles of File server and File server Resource Manager. On the next pop-up window Include management tools and continue by add features. And finally, by checking “Restart the destination server automatically if required”, we install. The server did not restart so we had to restart it manually.
 
![Installing_server_manager_role](https://github.com/bishwasghimire22/windowsserver/assets/144313610/3d6623b8-d491-4aa0-b7d6-c2ba18cbfb80)

After restarting the server from our control server, our file server is now a real file server with the role of file service. This role helps us to share our incoming network shares between the computers in our IP network, 10.208.0.0/24.

## Creating Network shares

In this next task we create a domain-specific network shares for our WindowsServer_FileServer. The purpose of adding a network shares is to create folders that are locally defined in our file server which is used to distribute to other computers in our domain using the same network paths. This Network sharing path can be created both locally from our WindowsServer_Fileserver or remotely from our WindowsServer_DC, although both ways have their own settings to be applied.

First, we do it locally from our file server. We do that by logging in to our file server with our domain administrator credentials. In the server manager window we select following options,

 File and Storage Services  Shares  To create a file share, start the New Share Wizard.
 
Above selection will take us to a new share wizard window where we choose the option Select profile  SMB share – Quick. This takes us to the next window where we need to choose a path for our network share. This can be done in two ways,

* Type a custom path  Enter C:\Files in the field, OR
* Create a new folder and choose a path  Browse to C:\ path and create a folder called ‘File’ and select folder.

I went with the first option and created a custom path. Next, we specify our share name to Data, this action changes our remote path to share to \\FileServer-Ghim\Data, this is the network path from which other computers in the domain will find the network share. Here, the two backslashes indicate the NetBIOS name of the server, and one backslash indicates the folder share name.

On the next page new choose other setting  Allow caching of share from the list of options available. This selection enables the users of the domain to configure the selected files and folders of the network share to be available even when the file server is offline/shut down. Which means domain computer can keep the local copies of files in the network share and synchronize them to the file server when it is back online/accessible again.

Next window is about permissions, which we leave as default for now and move on to next page.

On the next page we get the summary of settings we have configured as seen in the picture below. We continue by clicking the Create button. Now our Network sharing 
has been successfully created locally.

![Local_share_summary](https://github.com/bishwasghimire22/windowsserver/assets/144313610/622066d1-b3d8-4098-87ca-c2f69e87a2a9)
 
Next, we define a new network share remotely from our control server, this network share we define will be further configured in the later stages of courses. In order to start we need to be logged into both our WindowsServer_FileServer and WindowsServer_DC. 

In our Domain controller’s server manager we choose,

All Servers  computer management (File server)

Here we get an error saying, ‘Event viewer can’t connect’.
 
![Error](https://github.com/bishwasghimire22/windowsserver/assets/144313610/b50b764e-e26f-4693-97da-1c5d33ec0e74)

This problem should be fixed from our file server so we log into our WindowsServer_FileServer, here we search for ‘Windows Defender Firewall with Advanced Security’ from our search bar. On the Windows Defender Firewall window, we change the following setting to enable their status to ‘Yes’.

* Remote Event Log Management (NP-In) 
*	Remote Event Log Management (RPC) 
*	Remote Event Log Management (RPC-EPMAP)
 
![Error_fixed](https://github.com/bishwasghimire22/windowsserver/assets/144313610/3b55b279-d1a4-4c78-8407-f0036f436174)


We can close the file server's firewall window and log out of your file server and continue in our WindowsServer_DC from the option in computer management Shared Folders  Shares, here we can see the folder path of the file server you added earlier and connected to it, along with other related information. 
 
![CM](https://github.com/bishwasghimire22/windowsserver/assets/144313610/1f1e11a7-4147-412e-a44e-126457f7ad90)


Here, the $ sign at the end of the division name means the so-called hidden division. A hidden share is not visible when browsing files in the resource manager. You can only reveal hidden partitions with this tool.

Now we start to configure the actual network share remotely from our control server. We name this division person, which is done by creating a new partition, in addition to one we have already created locally called data. This is done by right clicking shares folder and selecting New share.

This takes us to a new window to create a shared folder. Next, we specify a new folder ‘C:\Management’ inside the C$ of our file server and go to the next page, where we define the share name as Person and change the offline setting to ‘No files or programs from the shared folder are available offline’, which corresponds to the allow caching of share setting we made previously in connection with data sharing.

On the next page we define network share access control to set as ‘Administrators have full access; other users have no access’, and press finish. This will give us a summary of our setting,
 
![Remote_NetworkShare_summary](https://github.com/bishwasghimire22/windowsserver/assets/144313610/ea5fbfa9-e26f-4eaa-a085-c002b252814e)

Now our computer management view has a new division ‘Person’ created remotely from our control server. We can check this by logging in to our file server and inside the Server Manager  File and storage services.

 ![Final_Netwroekshare_config](https://github.com/bishwasghimire22/windowsserver/assets/144313610/5658b453-3792-467d-b0b0-c698e96e91ec)
