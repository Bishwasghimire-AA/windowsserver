# Virtual template and sysprep

During this task we create a template server using sysprep tool and copy of a virtual hard disk for future windows server role needed later using the template we create in this task. All this task is done in Hyper V. First, we need to create a server-role Vm template. 

Following the instructions in Moodle material, we create our first virtual machine as a template. We configure it manually by providing a location and naming the server, then we allocate a virtual memory for our virtual machine, the memory in question is allocated dynamically. The purpose of allocating a dynamic location is so that the machine will not use all the memory allocated but will only use the memory required to perform the task.  Next, we connect our machine to the Hypervvswitch we created in the earlier task for our future internet connection needed. Next, we created a virtual hard disk for our machine, for this task we allocated 30GB of storage, it can be changed later. The virtual hard disk is saved in either VHD or VHDX file format. Finally, we choose the .iso file of windows server 2019 we had pre-installed in our Lab Vm, and we install the virtual machine.

![Summary_WIndows_server_installation](https://github.com/bishwasghimire22/windowsserver/assets/144313610/9317438c-36d3-45bb-b3ae-cc6b8a6ad9e8)
 

Now our virtual machine is visible in Hyper V manager. Next, we configure the processor, it is done by going in the setting and choosing the processor tab and changing the “Number of processors” option. For this machine we allocated 4 virtual processors.
 

![Proccessror](https://github.com/bishwasghimire22/windowsserver/assets/144313610/50b50768-3c23-4df0-972d-5543cce59013)

Next, we install our windows server operating system on the Hyper V virtual machine. First, we go to our virtual machine we just created in our Hyper V manager, and we connect to it. The window of the virtual machine we just created appears, next we change the languages setting, keyboard setting, and other features as required. Next the window takes to installation wizard where we do follow task,
•	Accepting terms and conditions,
•	Choose custom option “Install windows only.” 
•	Choose the hard disk space we created earlier,
•	And finally, Install.
The installation took 15 min in the campus network to finish. Ater the installation is finished the server restarts, then we set up our administrator password and accept by finish button.

 
  Figure 3. Installing windows server
	
As a rule, a Windows server that is running should never be forced to shut down. The Server Manager tool is used to install and configure new server programs on the server computer and to monitor status and log information. After logging into our Virtual machine, we have our server manager dashboard, where different features are present. Some notable features of the server manager dashboard are.
•	A summary of the states and functions of the running server programs is displayed in the section: ROLES AND SERVER GROUPS.
•	The PROPERTIES window shows the general configuration of this server.
•	From the Tools menu, you can open the server's various role and tool management programs and other useful administrator tools.
•	From the Manage menu, roles can be added and removed from the server and server management can be implemented.
 
Figure 4. Server manager dashboard

Windows sysprep tool
For this task we create a server template which can be used for several virtual machines we create later.
The system preparation tool or sysprep is a built-in Windows tool that, as the name suggests, prepares the system for use in a new environment. The tool removes previously made configurations from the target machine, such as the machine's internal identification information (Machine SID) and password information, i.e., makes the system a neutral platform. With sysprep, the computer's internal ID information is deleted. This is essential when creating several server machines from the same model. If two or more computers on the network have the same internal ID, the machines cause conflict situations on the network and do not work correctly. This task is done using the command prompt,
•	First, we run the command prompt as Administrator,
•	The following command is input, C:\Windows\System32\Sysprep\sysprep.exe /generalize /shutdown. 
•	Alternatively, this can be done in graphical interface as well by locating the above file path and double clicking the syspres.exe and tick generalize option + shutdown option.
By accepting our selection by OK button, it takes a while for the virtual machine to shut down. Upon restarting, we see "language settings" and the option to set a password. After providing the information as needed we have created a template for your future servers.
 
Figure 5. Initiating sysprep for creating a template for future machines

 Using the model machine
Next, we use the template we created and make two copies of its virtual hard disk for our future virtual machines. First, we need to turn off our template and return to Hyper V manager window and go through following steps.
•	First, we select our virtual machine in Hyper v manager window and select the option export, this is done to save the snapshot or to save the mold machine as template.
•	Export in the new folder we create; in this case we create a new folder in desktop of our Azure lab machine.
•	Next, we find the folder we just created and find the file WindowsServerTemplate.vhdx and we copy it into two new files in the same folder we created.
 
Figure 6. Copy of virtual hard disk from the template
The copy of virtual hard disks is saved in the folder we created in or desktop of Azure lab machine in the location, C:\Users\Admin123456\Desktop\HyperV\. The saved files are named as WindowsServer_DC.vhdx  and  WindowsServer_FileServer.vhdx which will be used as storage for our future virtual machines we create in the future class.
