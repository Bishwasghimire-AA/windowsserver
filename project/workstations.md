# Workstations
A workstation is a computer designed for technical or scientific applications. Intended primarily to be used by a single user, they are commonly connected to a local area network and run multi-user operating systems. 

In this task we will create a new virtual machine WindowsClient in our Hyper V, this machine will be our virtual workstation. We will connect this machine to our domain which we have created earlier and test its functionality as part of this domain. 

## Installing virtual machine in Hyper V

Installing and configuring the virtual machine is the same process as our previous installation of our domain control and file server. The only change will be the installation of the operating system for the workstation. We will be installing the Windows Pro iso file located at the desktop of our hyper v lab. So, we start in
      
    Hyper V manager  New  Virtual machine….
 
![Summary](https://github.com/user-attachments/assets/5754b909-46a0-4896-847c-6e1fc910bf36)

Now our virtual machine WindowsClient is shown in the Hyper V manager. We press connect to start running the installation. In the windows installer wizard we assign language, region, keyboard etc. as mentioned in the task. Finally, we custom install windows only by agreeing to the terms. After the installation is completed, we also change local group policy by enabling the Do not display the lock screen feature.

Next, we will rename and connect the workstation to our domain. Renaming is done as our previous task when renaming the domain control and file server. Right click on top of This PC   Properties  Change settings. In the System Properties windows button press Change, here we provide the computer name and domain name by providing our domain administrator information.
 
![changes](https://github.com/user-attachments/assets/53d1901a-c747-4d54-bbd2-50fd5219cef2)

Upon successfully joining the workstation to the domain, the system asks to restart the PC, but we need to activate windows using slmbr.vbs tool. Also, in our WndowsServer_DC server manager we need to move our workstations to correct active directory using the Active Directory Administrative Center tool. The workstation is located in the container Computers by default, we move it to the Resource  Desktops. This is done because when the workstation is moved to the correct container in active directory before it was restarted, all correct group policies will take effect immediately after the restart. Now we restart our workstation.

## Testing functionality of Workstation

For the test we will be using the domain user we have created on our previous task

 ![List of users we have created](https://github.com/user-attachments/assets/514ce870-3fb9-4bc0-90f7-7f5cc5de3926)

First, we sign in as a domain user Prod using the above credentials and check group policies affecting the logged in domain user Prod using the WindowsClient tool gpresult on the workstation. Next, we will test the Network shares as user Prod by logging in to your WindowsClient workstation as the domain user Prod. Open the File Explorer window and run the command whoami in the command prompt.
 
![Prod_test](https://github.com/user-attachments/assets/62557a15-8285-46cc-a95e-94e39fa1c229)

Next, we test the domain user Marknon, who belongs to the non-Management group. After logging in we run the same command as we ran for the domain user Prod in above picture.
 
![marknon](https://github.com/user-attachments/assets/18e1c23b-8ccf-4d96-a01c-9c0577ab0cea)

Next we will test our test websites TEST and 5004 that we have implemented in our file server. On our client computer, open the WindowsClient web browser Edge or Internet Explorer. In our browser we go to the address: http://intra.ghimire.lan/ . We get the following view.
 
![website_test on workstation](https://github.com/user-attachments/assets/8fc13f4b-2fb5-4313-b021-05acda7662ba)

Next we test the website 5004 by entering the address http://web.ghimire.lan:5004/ in the web browser of our workstation. But since we have redirected the HTTP request for this address to be redirect to the page  https://www.haaga-helia.fi in our previous task we get the following view
 
![Testing website 5004](https://github.com/user-attachments/assets/014346b2-93bf-4c20-b55e-52b023254b1a)

Finally, we test the functionality of our FTP site. First, we add a folder with our student number and a test file in the FTP share directory. Then from our workstation web browser we open the FTP page by the entering the address ftp://intra.ghimire.lan/ . The result is as below.
 
![FTP site functionality tested](https://github.com/user-attachments/assets/f43b286d-1223-47d6-b775-25a24eb8128a)
