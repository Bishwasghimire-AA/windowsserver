# Group Policies

Group policies are policies targeted at resources in the active directory of the Windows domain, which are followed by computers in the domain. Group policies are stored centrally on a domain server that has the AD DS role installed. In our case, this control server is WindowsServer_DC. As the name suggests, the server controls the activity of computers and users in our domain as determined by group policies and the active directory structure.

Group policies can only be applied to Active Directory containers like Sites, Domains, or Organizational units, not directly to individual resources within them. Which means the users or organizational units need to be configured in our Active Directory and the group policy is linked to the organizational units or users from the Group Policy Management Console. So, when a user logs in to a domain computer, the computer retrieves the configuration for this user's organizational unit from the domain control server. 

If the control server has multiple organizational unit configurations that apply to both the computer and the user, these policies are combined into one entity. The computer used by the user in the domain operates within this unified set of rules. Consequently, different users and different computers can have several different configuration combinations in the domain within which the computer works depending on the situation.

Group policies follow the same type of hierarchy as our Active Directory tree structure. Figuring out this hierarchy is of primary importance. Group practices are grouped by domains, and further within domains by organizational units. The hierarchy of group policies starts in the Forest and finally ends up in the organizational unit of the domain.

Group policies are stored as independent objects under the Group Policy Objects structure. A group policy object or GPO (Group Policy Object) contains the actual definition of the group policy. Group policy objects are linked to the active directory container in the Group Policy Management view. When a new Group Policy is defined for an Active Directory organizational unit, this definition is just a link to the Group Policy object itself. The links are stored in connection with each organizational unit, if the link is deleted, the group policy object itself is not lost, but only the link referring to it is removed from the list of group policies of the organizational unit.

In this task we will be configuring group policy for the main organizational units Accounts and Resources in your active directory, as well as the sub-organizational units that belong to them (HeadQuarters, Production, Marketing, Laptops, Desktops, Servers) that we have created in earlier task. This task is done in three steps,

*	In the first task, we will set the access rights of the Person network share we created earlier to the Management group. 

*	For the second task we create Group Policy Objects (GPOs) and link them to our organizational units in our Active Directory.

*	In the third task we define the content of the group policies created in the second task. In practice, we implement the rules described in the task for each organizational unit of your active directory. The code defines how different actors can operate on computers in your domain. In a company, these actors could be, for example, employees of different departments, and computers of different departments.

## Access control and rights for network shares

We set the access rights of the user groups of our file server WindowsServer_FileServer network disk shares. Network shares appear and are only available to users who have access to them. We implement access control, where the user group we added, Management, and the Domain Admins group have access and management rights only to the contents of the main folder. Others have no rights to this share.

First, we start your Control server WindowsServer_DC and our File server WindowsServer_FileServer at Hyper V Manager in our Azure lab environment. Next, in our WindowsServer_DC’s server manager window we select our file server  computer management. This opens the computer management windows here we go to the Shared foldersSharesPerson. In the person folder we go to the properties and change the permissions. These configurations are needed so that the group Management gets rights to the content of the Person network share, i.e., can create and add data to it. In the person properties window select SecurityAdvancedDisable inheritance. Here we remove the existing Users groups and then Add desired rights for the Management group.

After we add the Management group, we define the rights of this group to the network share Person. We specify the permission as required by the task (List folder/read data and Create folders/append data).

![Permissions](https://github.com/bishwasghimire22/windowsserver/assets/144313610/23996bf7-804b-463b-ad44-ff74211ae84f)
 
Now our Management group have special rights to This folder only.

![Final_permissions](https://github.com/bishwasghimire22/windowsserver/assets/144313610/2ed70f63-6a88-4574-b771-4567a2156b9a)

Next, we configure the share permission, this is needed so that the Management group can access the Person network share via the network. This is done in the Person propertiesShare permissionsAddAdvancedFind Now and selecting our Management group from the list. We only give the Read permission for our Management group from the permission for the management section.
 
![Share_permission](https://github.com/bishwasghimire22/windowsserver/assets/144313610/2903f5db-61d6-4421-8797-5221db75c458)


## Adding group policies

Group policies are created from the domain control server and implemented to the organizational unit or sub-organizational units as required.  According to [Microsoft](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831791(v=ws.11)), there are more than 3000 group policies available withing the group policy management, which can be configured and implemented as needed. In order to apply the policy to an organizational unit, these groups need to be created beforehand from the Active Directory Administrative Center in the domain control server.

For this task we are going to create group policy objects and link these policies to their own organizational units we have created in the previous task.  For this task we will be creating following group policy objects and linking to their Active directory containers,

*	AccountsGP  Ghimire.lan  Accounts
*	ProductionGP  Ghimire.lan  Accounts  Production
*	ResourcesGP  Ghimire.lan  Resources
*	DesktopsGP  Ghimire.lan  Resources  Desktops
*	LogonPolicyGP Ghimire.lan  Resources  Servers

First, login to our  Hyper-V virtual machine WindowsServer_DC, then open Tools pulldown menu in Server Manager window and choose Group Policy Management. This opens a window for group policy management.

 ![Group_policy_management](https://github.com/bishwasghimire22/windowsserver/assets/144313610/880da85f-5292-4f91-bcd7-073a2a4fd1d8)

 In the window right click group policy objects New, this opens a new GPO create window, where we fill the name as mentioned above. After creating all the GPOs our Group policy management window look like follow,

![Adding_GPOs](https://github.com/bishwasghimire22/windowsserver/assets/144313610/ba60d526-11ea-44a1-836f-4046edba8d14)

After this, we will link these GPO to the containers in our Active directory according to the task mentioned above. In the Group Policy Management window we select a target organizational unit, in this case Resources, right click  Link an Existing GPO, this option opens a window with all the list of GPOs we have created, from the list select required GPO, in this case ResourcesGP. Now our GPO is linked to our OU. We now follow the same method and link all the GPOs to their respective OU containers as instructed in the task. The final result looks as follow,

![Linking_Gpos](https://github.com/bishwasghimire22/windowsserver/assets/144313610/a51e2472-88e5-4986-8ce8-b5cdbd90a006)

## Editing Group Policy

In this task, we implement the actual rules of the group practices we created in the previous task. We can create and edit a rule either direct from the branch of the target GPO object Group Policy Objects or by selecting the link that indirectly refers to the GPO object in the tree view of the Group Policy Management window. For this task we will make following changes to our GPO as required by the task,

*	Default Domain Policy  Internet Settings/Password Policy/Turn on Script Execution (optional for this task)/ Interactive logon: Do not require CTRL+ALT+DEL (Enabled)
*	LogonPolicyGP  Logon policy
*	DesktopsGP  Interactive logon: Don't display last signed-in (Enabled)
*	ResourcesGP  Link-Layer Topology Discovery

First, we make some changes to our Default Group policy, In the Group policy management window right click Default Group policy  Edit, this will open the Group policy management editor window, within the window we select, Computer Configuration  Policies  Windows Settings  Security Settings  Account Policies  Password Policy  Password must meet complexity requirements (Enabled)

 
![GPO_editor](https://github.com/bishwasghimire22/windowsserver/assets/144313610/9f9119e1-9fa9-4cb2-89e8-131d562620bd)

Next for the Internet explorer warning, Default Domain Policy  User Configuration  Preferences  Control Panel Settings  Internet Settings Right click → New → Internet Explorer 10, in the window that still opens, select New Internet Explorer 10 Properties tab Security.


We continue and update the policies in the same way as requested by the task.





