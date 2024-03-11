# Active Directory

Active Directory (AD) is a centralized database and directory service for the Windows domain, which contains information about the domain's user, computer, and network resources. The active directory is located on the Windows domain controller from where it can be remotely controlled. 

In this task we will configure an Active Directory (AD) in our domain where we will create users, groups, and assign it to Organizational Unit (OU). An organizational unit (OU) is a logical entity in the active directory, a container that contains objects (resources) with the same type of usage needs. First, we log in to our Hyper V virtual machine WindowsServer_DC, which is our domain's control server, and open the Server Manager window.

We will now prepare the structure of the active directory as required by the task; we will be implementing a logical structure describing the company's structure in your active directory. We will add users in the AD of our control server, these added users can be centrally managed from our domain’s control server. Next, we will classify the added user to their own logical department group in our AD.

Within our server manager window, using the pull-down menu, we select Tools  Active Directory Administrative Center. Choose Users container and create a new user in the container by right-clicking and selecting New → User. We will get a ‘Create user’ window. Here we enter the details of the user as given in the task.
We have six users at this time.

![Creating_multi_User](https://github.com/bishwasghimire22/windowsserver/assets/144313610/050c4a51-505b-4889-a1b2-4bbb0ab56469)

New users in our domain control are, Head Quarter, Headnon Quarter, Prod Uction, Prodnon Uction, Mark Eting, and Marknon Eting. 

Next, we will group the users created in our active directory according to the company's hierarchy.  This is done by selecting from the container, Users  New  Group. This takes us to a group-creation window where we create two different groups: managers and employees. In the Create Group window, look for Members button and click it, a new window will open. Use the Advanced... button and then the Find Now button to select users from the list. For the non-management group, we add Headnon Quarter, Prodnon Uction, Marknon Eting and for the management group we add Head Quarter, Prod Uction, Mark Eting. After adding members and finishing the group creation our groups should be visible in the Active Directory Administrative Center  ghimire (local)  Users
 
![create_group](https://github.com/bishwasghimire22/windowsserver/assets/144313610/d27ca2e3-8080-4859-8c28-151b31395a28)

## Organizational Units (OU)

Now we group the users of the company we created above into the organizational units. In addition to the company's departments, we create our own organizational units for the company's desktops, laptops and servers. An organizational unit (OU) is a logical entity in the active directory, a container that contains objects (resources) with the same type of usage needs. 

For this task we will create two OU Accounts and Resources. This OU is created in the root of our domain by right-clicking, ghimire (local)   New  Organizational Unit. Next, within the OU we create three more sub-organizational units. For Accounts: Headquarters, Production and Marketing, for Resources: Desktops, Laptops and Servers. Into these sub-OU we add the users we created earlier as assigned in the task. This is done by selecting the users and moving it to the designated sub-organization unit. Also, the WindowsServer_FileServer is also added to the sub-organizational unit called server.
 
![Final_AD_view](https://github.com/bishwasghimire22/windowsserver/assets/144313610/5eb841bc-bd75-4206-a90a-6de934814a53)

 
![OU_creation](https://github.com/bishwasghimire22/windowsserver/assets/144313610/bf3193ba-4099-4e43-b44f-480c8d11a815)


Next, we create a database query for our active directory. The result should give us a summary of the users, computers, groups, and organizational units we created in our active directory. This is done in the following ways.

In our server manager window choose Tools  ADSI Edit. This takes us to the ADSI Edit window, choose at ADSI Edit windows by right clicking ADSI Edit and then Connect to... and ok as default. Then in the ADSI Edit window click the option Default naming context   New → Query.

In the new query window, we enter details as requested in the task and copy the given query phrase from the task in the query string box and save it in the root of query window by selecting OK. Now the ADSI edit windows shows the path leading to our query.
 
![ADSI_edit](https://github.com/bishwasghimire22/windowsserver/assets/144313610/c7ef2d27-5408-426f-8db2-671ab8c84945)


