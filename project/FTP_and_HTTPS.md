# FTP and HTTPS

FTP (File Transfer Protocol) is a method used to communicate and transfer files between computers on a TCP/IP (Transmission Control Protocol/Internet Protocol) network, aka the internet. Users who have been granted access can receive and transfer files in the File Transfer Protocol server (also known as FTP host/site).

A standard FTP connection consists of a control connection using the Telnet protocol, i.e. it is a file transfer connection in binary or ASCII format. The goal of FTP is, for example: Makes transferring and sharing files easier. Here is how FTP works,

* A client establishes a connection to the FTP server by requesting the server’s IP address and port number. The server responds with a message indicating that the connection has been established.
*	The client authenticates with the server by providing a username and password. If the authentication is successful, the server grants access to the client. Sometimes, anonymous access is also possible.
*	The client can then issue FTP commands, such as upload, download, delete, or list files, to the server.
*	For file transfers, the client opens a separate data connection to the server. The data connection transfers the actual file contents between the client and the server.
*	When the file transfer is complete, the data connection is closed. The client then can issue further FTP commands or disconnect from the server.
  
Hypertext transfer protocol secure (HTTPS) is the secure version of HTTP, which is the primary protocol used to send data between a web browser and a website. HTTPS is encrypted in order to increase security of data transfer. This is particularly important when users transmit sensitive data, such as by logging into a bank account, email service, or health insurance provider.

HTTPS uses an encryption protocol to encrypt communications. The protocol is called Transport Layer Security (TLS), although formerly it was known as Secure Sockets Layer (SSL). This protocol secures communications by using what’s known as an asymmetric public key infrastructure. This type of security system uses two different keys to encrypt communications between two parties:

*	The private key: this key is controlled by the owner of a website, and it’s kept private. This key lives on a web server and is used to decrypt information encrypted by the public key.
*	The public key: this key is available to everyone who wants to interact with the server in a way that’s secure. Information that’s encrypted by the public key can only be decrypted by the private key.

## FTP and HTTP configuration.

In this task we will be configuring and installing roles of HTTP/web service and FTP service in our WindowsServer_FileServer. In In working life, the web service can be used either to maintain the company's intra websites, or at discretion also to maintain the company's public websites locally, i.e., an On-Premises solution. 

The tasks are done comprehensively on our file server, but the control server of your domain must also be on so that normal domain functions, such as domain user login, work.

For the first task we will install the IIS server software required by the HTTP service and the FTP service.

Internet Information Services (IIS) is a flexible, general-purpose web server from Microsoft that runs on Windows systems to serve requested HTML pages or files. An IIS web server accepts requests from remote client computers and returns the appropriate response. This basic functionality allows web servers to share and deliver information across local area networks (LAN), such as corporate intranets, and wide area networks (WAN), such as the Internet.

The installation of the IIS service follows the same pattern as the installation of server software/roles done in the previous exercises. First, we login to our WindowsServer_FileServer as a domain administrator, in the server manager  Manage → Add Roles and Features. In the set-up window Continuing to Server Roles in at the Server Roles choose option: Web Server (IIS) and include Management tools options.

After making all thew correct selections as requested by the task as seen in the picture below we confirm the installation of the role.
 
![IIS Intallation](https://github.com/user-attachments/assets/e94c177d-2c49-4a6a-aaf7-ce14ed31351a)

Now the option IIS appear in the Server Manager window of our file server:
 
![IIS installation succesfull](https://github.com/user-attachments/assets/635a1189-ed6b-4205-8c6a-98d6b1f18979)

For the second task we will make a test webpage by modifying the IIS default web page, and its functionality is tested locally on our file server. The purpose is for our copied page to appear locally on our file server. At the same time, we will also familiarize ourselves with the IIS Manager view. We will test the operation of the test page on a workstation connected to your domain later.

On our file server, open Server Manager → Tools → Internet Information Services (IIS) Manager. During the installation phase of the IIS service, the content structure of the Default Web Site page was created in the file server path C:\inetpub. Copy this folder with its contents to the new path C:\inettesti This folder will be the basis of your own test website with its content. The IIS service will use it. 

In the IIS Manager window. Create your own page in IIS Manager by selecting Add Website by clicking Right mouse at Sites. In the setup window we fill in the information as requested by the task and press ok.
Next we create a content for our webpage using notepad and writing some basic HTML code snippet.
 
 ![TEST page](https://github.com/user-attachments/assets/7fb89ec4-175b-4e40-a5b7-0dd52858bde4)

For the third task we will create another test webpage which uses a different port number (5004) than the HTTP protocol. At the same time, the firewall settings of our file server are configured for this port, and the site is redirected. At the beginning, a page like the previous task was implemented, but now we do for it a redirection to an external web address. Applying the instructions from the previous task, we created a new page that now uses the specifications as requested by the task.

In the path C:\inet5004, use the same base idea as in the previous task, copy the folder C:\inetpub. Edit the file C:\inet5004\wwwroot\iistart.htm content. As in the previous task, add HTML content to it as done in previous task.
 
![TEST page 5004](https://github.com/user-attachments/assets/fba1d921-ad04-4daa-b4bc-2eb8db11c9cf)

Page 5004 should already be working, and we can access it locally from our file server, but the problem is that we can't access the page from any other computer in the domain. This is because our file server's firewall blocks inbound traffic to its TCP port 5004. 

Port 5004 is not a standard port, so there is no default configuration for it at all. The file server must allow inbound traffic to TCP port 5004 so that other machines in your domain can also access the web page 5004. So now we will implement a firewall configuration to allow connection to this port to be accessible.

In our file server WindowsServer_FileServer, in the search bar search for “Windows Defender Firewall with Advanced Security”. In the window select Inbound rules  New rule (Action tab), withing the setup wizard select,

    Port  TCP & Specific local ports: 5004Allow the connection  Domain & Private  Name (Inbound TCP 5004)  Finish.
    
The firewall rule has appeared in the firewall's Inbound rule list. In addition, it is already connected.

Now our file server allows incoming traffic to IIS web service page 5004 via TCP port 5004 from computers in domain and private network space 10.208.0.0/24. Let’s test our test page 5004 functionality from our file server web browser. Since the port number used by this page is 5004. It can be referenced locally either by address http://127.0.0.1:5004/ or http://localhost:5004/ The content should look like this:
 
![page 5004 from DC](https://github.com/user-attachments/assets/c475b940-2bbb-4ebd-9c5c-5a6a95dc66a2)

Let’s setup redirection, after making the page and successfully testing it, the purpose is to determine that the IIS service redirects the requests made to the page to https://haaga-helia.fi.

Open settings 5004 at IIS Manager in our file server. Double-click on the selection HTTP Redirect. In the HTTP redirect dialog box, we implement the redirect by using the following value,

*	🗹 Redirect requests to this destination: https://haaga-helia.fi
*	Status code: Permanent Redirect (308) Apply.

A status code is the value used by the HTTP protocol that controls the operation of HTTP pages, with which web server software implements the desired functionality on websites. The status code used in the task tells the IIS server software that it should permanently redirect all (client) requests considering 5004 to https://haaga-helia.fi.

Now the request made for address http://localhost:5004/ or http://127.0.0.1:5004/ should redirect to the page https://haaga-helia.fi.

![Redirect](https://github.com/user-attachments/assets/9ed6230c-bb62-4407-9e17-7a0e6daa123f)

For the final task we will implement an FTP page and test its functionality. First open the IIS manager in our file server then select  the  Sites option from the IIS Manager tree structure, and press Add FTP Site by right clicking the mouse. Provide the following details in the window,

    *	FTP site name: ftp
    *	Physical path: C:\inettesti\ftproot (this is the folder we created in earlier steps)
  
In the next page we add,

    •	Port:  21 (FTP service´s default port)
    •	 IP Address: All Unassigned (Means practically all web addresses)
    •	🗹 Start FTP site automatically 
    •	SSL:  ◉ No SSL

NOTE! The use of SSL is generally a strong recommendation to improve data security. We do not use SSL in the course assignment, but in working life the company may require its use.

Secure Sockets Layer (SSL) now known as Transport Layer Security (TLS) is an encryption protocol that can be used to protect the communication of Internet applications over IP networks. It is currently one of the most common ways to protect data traffic. The most common way of using TLS is to protect the transmission of WWW pages with the HTTPS protocol.

In the next page authentication and authorization, we enter following details,

    •	Authentication: Anonymous 
    •	Authorization – Allow access to: All users.
    •	Permissions:  🗹 Read   🗹 Write

Continue by pressing finish.

Making sure the FTP page we created is turned on. Then add a new file to the folder: C:\inettesti\ftproot\ In the example of the instructions, a file test_image.png has been placed in the folder.
Next we test that the file also appears on our FTP page in one of the following addresses ftp://intra.ghimire.lan/ or ftp://web.ghimire.lan/,

 ![FTP page ](https://github.com/user-attachments/assets/f0e14e8f-0e2d-4e44-ae23-dc881982b6a0)

Also, in the firewall setting we can see that TCP port 21 for incoming traffic used by the FTP service was automatically opened when the service was enabled.

After completing above task our IIS window manager site view looks as follow:
 
![Final_result](https://github.com/user-attachments/assets/18562e3a-5c12-4ac5-b391-c47ec549f77c)

