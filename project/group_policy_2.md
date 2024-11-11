# Group policy modelling.

When a user logs in, Windows combines all of the different Group Policies that apply to the user account with those that apply to the computer that the user is logging in from. This is very helpful for the administrator, but each level of the Group Policy hierarchy contains many of the same settings. That means there is a potential for the administrative staff to implement contradictory Group Policy settings. In smaller companies, administrators might be able to avoid Group Policy contradictions by using a single GPO, but this usually isn't practical in larger organizations. The group policy settings that apply to the computer can sometimes also contradict with group policy settings applied to a user.

This is where the Group Policy Object Modelling is used to troubleshoot our settings quickly and easily. We can perform GPO modeling from within the Group Policy Management Console (GPMC). The Group Policy Modeling tool works as a tool for planning, implementing, and testing group policies. Even if everything appears to be running smoothly it is a good idea to periodically use group policy modeling just to make sure that group policies are being applied in the way that you think that they are. Group policy modeling is also extremely useful in situations in which you are reorganizing the Active Directory or creating new group policy objects.

With the Group Policy Modeling tool, all group policy definitions for the selected object are read and a report is produced as a result, from which the administrator can see the result, i.e., the group policies to be executed and the definitions they contain. The result is called the Resultant Set of Policy (RSoP), which is the name of the administrative right that must be delegated in Active Directory to run this tool.

In this task we will be modelling the final RSoP group policy (Resultant Set of Policy) using selected organizational units and saving the result. We are looking for an answer to following question,

    "If the user “Prod Uction” or “Prodnon Uction” logs on to a desktop that belongs to the Desktops organizational unit, what policies apply to her/him?"
    
On our previous task of configuring Group policies, we have applied 2 group policies, DesktopGP and LogonPolicyGP to our OU Desktops, which is a sub-OU of our Resources OU which also has a ResourcesGP applied to it.

In our Hyper V manager login to WindowsServer_DC, Unfortunately I get an option to change password, this must have been because of the group policy we applied in previous task. I try to change the password to the same one given in our task, but it gave an error,
 
![Password error](https://github.com/user-attachments/assets/08711cca-3f1e-47e3-a112-38275f583722)

This must be because we need to have some special character in our password as specified in the Default group policy. So, I changed the password as needed. The new password for my WindowsServer_DC is Qwerty@789. After successfully changing the password, we log in to our domain controller. 

More about this error and changed password to be discussed with the teacher.

In the server manager Tools  Group Policy Management, this opens a policy management window, in the policy management window select Group Policy Modeling  Group Policy Modeling Wizard. 

In the modelling wizard window, we select our domain and select user information and computer information as asked by our task,

    User  Container: OU=Production OU=Accounts, DC=ghimire, DC=lan
    
    Computer  Container: OU=Desktops OU=Resources DC= ghimire,DC=lan

We cannot select an individual user or computer. Therefore, we select the container in our active directory (in our case the organizational unit) where the target resource is. We want to model users Prod Uction and Prodnon Uction on a desktop computer, which is imaginary in the Desktops organizational unit.
 
![User_info](https://github.com/user-attachments/assets/efde85a9-f41a-4bff-87a6-66207700d2f8)

We continue to the User and Computer security page where we assign the modelling to Everyone from the list. For the WMI filter we choose “All linked filters” for both users and computers. WMI filters can be used to specify finely, for example, what characteristics a computer should have (operating system version, amount of RAM, etc.) or for group policy to be applied to it.

In the next page we get all the Modelling summary new configured, at this stage we can still make changes to the selection we have made. If everything looks good, we continue by clicking the Finish button.
 

![Summary](https://github.com/user-attachments/assets/36639842-66db-4e93-8f3d-44df15916a9d)

## Returning the modelling result
After we finish configuring the modelling we can choose to print or save the report, benefits of saving the produced reports are,
•	It provides a record of the simulated configuration, which can be useful for documentation purposes, compliance audits, or future reference.

•	It allows administrators to analyze the impact of the simulated Group Policy settings in detail, including their effects on specific users, computers, or organizational units.

•	Saved reports can be compared with actual Group Policy results or other simulated scenarios to identify discrepancies, troubleshoot issues, or verify compliance with organizational policies.

•	Reports can be shared with stakeholders or other team members for review, approval, or discussion of proposed Group Policy changes.

We will be saving the report in the HTML format on our Desktop of our WindowsServer_DC in the following way,
In the Group policy Management window select the model we created earlier  save Report, save it as HTML file type. The result look as follow:
 
Figure 4. Modelling report saved as HTML file.

The results show directly which Group Policy Objects (GPOs) and the configurations defined in them affect Production users on Desktops.
We can always make changes to the group policies in the Group Policy Management window and run our modelling again instead of having to reproduce the modelling in the wizard again. After the changes, we can run the modelling again by selecting Rerun Query from the modelling with the right mouse button,

Extract from the current structure of our active directory.
Next, we create a LDAP (Lightweight Directory Access Protocol) database query for our active directory. The result should give us a summary of the users, computers, groups, and organizational units we created in our active directory. This is done in the following ways. 
In our server manager window choose Tools → ADSI Edit. This takes us to the ADSI Edit window, choose at ADSI Edit windows by right clicking ADSI Edit and then Connect to... and ok as default. Then in the ADSI Edit window click the option Default naming context → New → Query.
In the new query window, we enter details as requested in the task and copy the given query phrase from the task in the query string box and save it in the root of query window by selecting OK. Now the ADSI edit windows shows the path leading to our query.
 
Figure 5. LDAP query of our Active Directory using ADSI Edit tool.

In the query we didn’t see the Non-Management group because of the special character “–“used in the naming of the group.

