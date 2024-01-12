
<h1>The Blue Team Red Team Microsoft Sentinel SIEM lab. From Deployment, Configuration to Compromise, Threat Detection and Remediation.</h1>


<h2>Description</h2>
The project began by spinning up a SIEM in Microsoft Sentinel, to initial configuration then setting up playbook rules with KQL as a blue teamer. We then created a user within Azure and acted as if the account was compromised by the user causing a security incident. (Red teamer). To end the project, we went back to blue team and remediated the security incident. This project was not easy to complete. There were a lot of errors and frustrations along the way but we were very happy to complete it. We learnt a lot of valuable skills like setting up playbooks, querying through Kusto Query Language(KQL), setting role permissions, analysing logs, and finally remediating the security incident. Overall, it was fantastic learning and skill leveling up experience.
<br />


<h2>Languages and Utilities Used</h2>
- <b>KQL (Kluster Query Language</b>

<h2>Environments Used </h2>
- <b>Microsoft Azure</b> 
- <b>Microsoft Sentinel</b>

<br/>
<h2>Project walk-through:</h2>

<h3>Step 1 - Introduction to Sentinel SIEM Deployment</h3>
To speed development we will use Sentinel All in One.
Sentinel All in One is basically AI
<br/>
<br/>
It also configures Content Hub solutions
<br/>
<br/>
If you want to have a look at setting up SIME from beginning to end go to  Pavs other course which we bought
<br/>
If you look at some of the data connectors some are billed and some are free
but don’t worry we wont need to spend anything 
<br/>
<br/>
When we first deploy Sentinel we get 10gb ingestion per day for the first month
That’s plenty of time to go through all the necessary tasks
Then you will delete Sentinel
<br/>
<br/>
Then whenever you want to spin up a fresh new Sentinel just Deploy to Azure the Sentinel All in One.
<br/>
<br/>
<br/>
(https://github.com/Azure/Azure-Sentinel/tree/master/Tools/Sentinel-All-In-One#readme
)
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="1" border="0" /></a>
<br/>


<br></br>
<h3>Step 2 - Deploying Sentinel to Azure</h3>
<br/>
Click > Deploy to Azure
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="2" border="0" /></a>
<br/>
Now that you in the custom deployment 
<br/>
Basic Tab
<br/>
Choose: 
<br/>
Subscription --> Azure 1
<br/>
Location --> East US  (this is important for costs purposes East US is the cheapest. However for this project, pick the closest one to where we are. Our 10gb daily limit will be more than enough)
<br/>
Resource Group Name:  (pick a name to reflect what the solution is about) --> Security-Monitoring-rg
<br/>
Workspace Name --> same as resource group name
<br/>
Daily ingestion limit in GBs. --> 10
<br/>
Number of days of retention --> 90  (this means its free for the first 90 days. After that we have to pay for every Gigabyte in data!)
<br/>
Select pricing tier for Log Analytics --> Pay-as-you-go (just leave it as is)
<br/>
Select pricing tier for Sentinel --> Pay-as-you-go (just leave it as is)
<br/>
<br/>
<a href="https://ibb.co/M75HdHK"><img src="https://i.ibb.co/6FJSQSG/2.png" alt="3" border="0" /></a>
<br />
<br />
Settings tab
<br/>
Enable Sentinel Health Diagnostics --> Select
<br/>
We will Enable User Entity Behavior Analytics (UEBA)  later together
<br/>
<a href="https://ibb.co/M75HdHK"><img src="https://i.ibb.co/6FJSQSG/2.png" alt="4" border="0" /></a>
<br/>
<br/>
Content Hub Solutions Tab
<br/>
There are three content categories for solutions
<br/>
<br/>
Select Microsoft Content Hub Solutions --> Select All
<br/>
Select Essential Content Hub Solutions --> Select All
<br/>
Select Training and Tutorials Content Hub Solutions --> Select All
<br/>
<a href="https://ibb.co/M75HdHK"><img src="https://i.ibb.co/6FJSQSG/2.png" alt="5" border="0" /></a>
<br/>
<br/>
Data Connectors Tab
<br/>
Select data connectors to onboard --> select all
<br/>
If we don’t have permissions for some data connectors it will simply fail so don’t worry.
<br/>
Select Azure Active Directory log types to enable --> Select all
<br/>
<br/>
<a href="https://ibb.co/M75HdHK"><img src="https://i.ibb.co/6FJSQSG/2.png" alt="6" border="0" /></a>
<br/>
<br/>
Analytics Rules Tab
<br/>
Enable Scheduled alert rules for selected Content Hub solutions and Data Connectors --> Select
<br/>
we don’t want to enable hundreds of scheduled alert from the content hub manually
<br/>
Select the severity of the rules to enable --> Select all
<br/>
<a href="https://ibb.co/M75HdHK"><img src="https://i.ibb.co/6FJSQSG/2.png" alt="7" border="0" /></a>
<br/>
<br/>
Review + create Tab
<br/>
Check all the details .
<br/>
Click > Create
<br/>
This will take 10-15 minutes to deploy
<br/>
During this process you may encounter some failures because you wont have the license for some.
<br/>
<a href="https://ibb.co/M75HdHK"><img src="https://i.ibb.co/6FJSQSG/2.png" alt="8" border="0" /></a>
<br/>
<br/>
I am getting an authentication error.
<br/>
<a href="https://ibb.co/M75HdHK"><img src="https://i.ibb.co/6FJSQSG/2.png" alt="9" border="0" /></a>
<br/>
<br/>
I think I need to change user write permissions to enable this deployment.
<br/>
In the Azure All in One readme in Github it states.
<br/>
Azure user account with enough permissions to enable the desired connectors. See table at the end of this page for additional permissions. 
Write permissions to the workspace are always needed.
<br/>
I created the resource group siem-training and gave owner privileges.
<br/>
Click Role Assignment >  Add > Add role assignment.
<br/>
<a href="https://ibb.co/M75HdHK"><img src="https://i.ibb.co/6FJSQSG/2.png" alt="10" border="0" /></a>
<br/>
<br/>
Role Tab
Select role by clicking on View. We chose Privileged Admin role.
<br/>
<a href="https://ibb.co/M75HdHK"><img src="https://i.ibb.co/6FJSQSG/2.png" alt="11" border="0" /></a>
<br/>
<br/>
Select members.
<a href="https://ibb.co/M75HdHK"><img src="https://i.ibb.co/6FJSQSG/2.png" alt="12" border="0" /></a>
<br/>
<br/>
Select role assignments and members.
<a href="https://ibb.co/M75HdHK"><img src="https://i.ibb.co/6FJSQSG/2.png" alt="13" border="0" /></a>
<br/>
<br/>
Role assignment I put it as constrained to owner.
<br/>
<a href="https://ibb.co/M75HdHK"><img src="https://i.ibb.co/6FJSQSG/2.png" alt="14" border="0" /></a>
<br/>
<a href="https://ibb.co/M75HdHK"><img src="https://i.ibb.co/6FJSQSG/2.png" alt="15" border="0" /></a>
<br/>
<a href="https://ibb.co/M75HdHK"><img src="https://i.ibb.co/6FJSQSG/2.png" alt="16" border="0" /></a>
<br/>
Good seems like it all worked now.
<a href="https://ibb.co/M75HdHK"><img src="https://i.ibb.co/6FJSQSG/2.png" alt="17" border="0" /></a>
<br/>
<br/>




<h3>Step 3 - Exploring Artifacts Installed</h3>
<br />
There will be some errors for enableDataConnectors. For Status Message look for InvalidLicense.
We can ignore it for now.
<br/>
Now lets go to the resource group associated with this Sentinel. Type in search bar siem-training2.
<br/>
Click on Overview.
<br/>
There should be over 390 records but I only have 9 records. Is this right?
So I made a new deployment called siem-training3 and will compare
<br/>
I created siem-training3 and it is exactly the same as siem-training2.
Don’t worry about the 9 records. 
<br />
<br/>
<a href="https://ibb.co/348nntQ"><img src="https://i.ibb.co/gmXxxsh/13.png" alt="18" border="0" /></a>
<br/>
<br/>
This is where all the data is stored
However most of them are just templates
<br/>
For instance if you want to deploy a package from the  Content Hub in Sentinel, for example includes 50 analytics rules, it will create a separate template for each.
<br/>
Lets choose one to see whats inside (ignore this section - its not applicable in the recent deployment)
<br/>
In a template you can see Display Name and Description on the right side. So that’s basically how the solution looks in the background
<br/>
Microsoft Sentinel is built on top of the log analytics workspace where all the data is stored.
We want to monitor Sentinel as well as the log analytics workspace.
Why?
<br/>
Because we want to be aware of situations where our queries don’t perform as they should, or maybe it takes too long and ends up in error
so lets go back to our resource group and find the log analytics workspace for us it should be:
<br/>
siem-training2 (type Log Analytics workspace) > Click
<br/>
<a href="https://ibb.co/348nntQ"><img src="https://i.ibb.co/gmXxxsh/13.png" alt="19" border="0" /></a>
<br/>
<br/>
On the Monitoring blade --> Diagnostic settings --> Add diagnostic setting
<br/>
<a href="https://ibb.co/348nntQ"><img src="https://i.ibb.co/gmXxxsh/13.png" alt="20" border="0" /></a>
<br/>
<br/>
What is Diagnostic settings?
<br/>
Diagnostic settings are used to configure streaming export of platform logs and metrics for a resource to the destination of your choice. 
You may create up to five different diagnostic settings to send different logs and metrics to independent destinations.
<br/>
Diagnostic setting name -->  Sentinel
<br/>
Logs --> Category groups --> allLogs (select)
<br/>
Metrics --> AllMetrics (select)
<br/>
Destination details --> Send to Log Analytics workspace --> Subscription (choose) --> Log Analytics workspace --> security-monitoring (japanwest)
<br/>
Click Save
<br/>
<a href="https://ibb.co/348nntQ"><img src="https://i.ibb.co/gmXxxsh/13.png" alt="21" border="0" /></a>
<br/>
Now we are all setup! We can move on to Sentinel itself.


<br>
</br>
<h3>Step 4 - Exploring Sentinel </h3>
<br/>
Search for Sentinel in the search bar --> choose your resource group > siem-training2
<br/>
<br/>
<a href="https://ibb.co/ZTJHTrJ"><img src="https://i.ibb.co/bd3HdV3/14.png" alt="22" border="0" /></a>
<br/>
<br/>
Click on Microsoft Sentinel (MS)
<br/>
Overview gives you a dashboard overview
<br/>
On the left side MS is divided into 4 sections
<br/>
General
<br/>
Threat Management
<br/>
Content Management
<br/>
Configuration
<br/>
<a href="https://ibb.co/ZTJHTrJ"><img src="https://i.ibb.co/bd3HdV3/14.png" alt="23" border="0" /></a>
<br/>
<br/>
Lets focus on some of the most interesting tabs in Sentinel.
<br/>
General --> Logs
<br/>
Here we can search for you data using Kusto Query Language or KQL
You can see all the tables by click on the all the triangle icons.
<br/>
e.g   LogManagement, Microsoft Sentinel, Custom Logs
<br/>
One of the tables we need to use are signin log, which is currently missing.
<br/>
For now lets check AzureActivity.
This table provides information actions inside your portal and provides information about who performed the action and other properties important for investigation.
<br/>
Lets also check AADNonInteractiveUserSignInLogs (we don’t have that now siem-training2 which is v2 but in siem-training3 v1 it has)
this provides information about authentication requirements, client usage and location details
location details will be particulary useful for us.
<br/>
<br/>
<a href="https://ibb.co/ZTJHTrJ"><img src="https://i.ibb.co/bd3HdV3/14.png" alt="24" border="0" /></a>
<br/>
<br/>
Lets have a look at the Configuration Section.
<br/>
Configuration --> Data Connectors.
<br/>
We should have around 9-10 Connected.
<br/>
We can filter by Status: Connected.
<br/>
This lets us see the type of data being collected, number of logs received and tables that are being populated.
<br/>
<br/>
<a href="https://ibb.co/ZTJHTrJ"><img src="https://i.ibb.co/bd3HdV3/14.png" alt="25" border="0" /></a>
<br/>
<br/>
Configuration --> Analytics
<br/>
Do you remember the hundreds of templates inside your resource group?
<br/>
Most of them were for analytics rules that were automatically deployed and enabled for you
There are already roughly around a staggering detection rules already built and provided by Microsoft, this is great to get started with monitoring threats.
<br/>
Lets have a look at some  high severity just to see what rules have been broken?
<br/>
<br/>
<a href="https://ibb.co/ZTJHTrJ"><img src="https://i.ibb.co/bd3HdV3/14.png" alt="26" border="0" /></a>
<br/>
<br/>
Lets also have a look at Anomalies > Click.
<br/>
There are anomalies templates developed to be robust by using thousands of data sources and millions of events
<br/>
Microsoft allows you to change the thresholds for them in case they generate a lot of false positives.
Right now I don’t even know what a false positive looks like! 
<br/>
Note that the  Anomalies template works with the User and Entity Behavior Analytics which is currently not working
<br/>
Next video, our very first task in Microsoft Sentinel will be to fix this issue!
<br/>
<a href="https://ibb.co/ZTJHTrJ"><img src="https://i.ibb.co/bd3HdV3/14.png" alt="27" border="0" /></a>
<br/>
<br/>




<br>
</br>
<h3>Step 5 - Enabling User Entity Behavior Analytics and Playbooks  </h3>
<br/>
The user and entity behavior analytics (UEBA) is an amazing feature that uses AI to detect and alert you to any unusual behavior happening within your system.
<br/>
To turn this feature we have to go to:
<br/>
Configuration --> Settings --> Settings --> Set UEBA 
<br/>
<a href="https://ibb.co/ZTJHTrJ"><img src="https://i.ibb.co/bd3HdV3/14.png" alt="28" border="0" /></a>
<br/>
You will come to a new screen called Entity behavior Configuration.
<br/>
1. Turn on the UEBA feature --> On (click)
<br/>
2. Sync Microsoft Sentinel with at least one of the following directory services --> Microsoft Entra ID (select) --> Apply
<br/>
3. Select existing data sources you want to enable for entity behavior analytics --> Select Audit Logs, Azure Activity , Signin Logs --> Apply
<br/>
Now we have enabled AI Machine Learning in Microsoft Sentinel.
<br/>
<a href="https://ibb.co/ZTJHTrJ"><img src="https://i.ibb.co/bd3HdV3/14.png" alt="29" border="0" /></a>
<br/>
<br/>
As we are already in Sentinel  configuring lets use automation playbooks.
<br/>
To do this we need to give MS permissions.
<br/>
Configuration --> Settings --> Settings --> Playbook permissions --> Configure permissions
<br/>
<a href="https://ibb.co/ZTJHTrJ"><img src="https://i.ibb.co/bd3HdV3/14.png" alt="30" border="0" /></a>
<br/>
Manage permissions panel will show up to the right.
<br/>
Select the correct resource group siem-training2 > Apply
<br/>
Now we are all set up and ready to create some amazing artifacts within Microsoft Sentinel.
<br/>
Next video we will be diving into watchlists!
<br/>
We will learn to create our own watchlist and how to leverage powerful capabilities to enhance security operations.
<br/>
<a href="https://ibb.co/ZTJHTrJ"><img src="https://i.ibb.co/bd3HdV3/14.png" alt="30" border="0" /></a>
<br/>
<br/>



<br>
</br>
<h3>Step 6 - Creating Watchlists to Detect Threats</h3>
<br/>
This will be the first step in monitoring TOR exit nodes using Microsoft Sentinel.
How?
<br/>
We will create a watchlist that checks all the TOR exit nodes IP addresses and use it with analytics rules.
<br/>
Go to the watchlist
<br/>
Configuration --> Watchlist --> Add new
<br/>
<a href="https://ibb.co/hHPqB1y"><img src="https://i.ibb.co/Y8zrcT3/41.png" alt="32" border="0" /></a>
<br />
<br />
New window pops up Watchlist Wizard
<br />
There are three tabs, General, Source, Review and Create
<br />
In the General tab:
<br />
Name --> Tor-IP-Addresses
<br />
Description --> A watchlist that checks all the Tor Exit nodes IP Addresses
<br />
Alias --> Tor-IP-Addresses
<br/>
<a href="https://ibb.co/hHPqB1y"><img src="https://i.ibb.co/Y8zrcT3/41.png" alt="33" border="0" /></a>
<br />
<br />
We can create watchlists from a local file or from Azure storage
<br />
Source tab (is it from a local file or from Azure storage)
<br />
Fill in the below fields:
<br />
Source type --> Local file
<br />
File type --> CSV file with a header
<br />
Number of lines before row with headings --> 0
<br />
Upload file --> Tor+Exit+Nodes.csv
<br />
File preview on right side to check for your validation
<br />
SearchKey --> IpAddress
<br />
Review and Create  tab --> Create
<br />
<br />
<a href="https://ibb.co/hHPqB1y"><img src="https://i.ibb.co/Y8zrcT3/41.png" alt="34" border="0" /></a>
<br />
Our newly created watchlist.
<br />
<a href="https://ibb.co/hHPqB1y"><img src="https://i.ibb.co/Y8zrcT3/41.png" alt="35" border="0" /></a>
<br />
<br />
Select the watchlist
<br />
On the right side panel --> click View in Logs
<br />
This is what the watchlist looks when you call it with KQL and Sentinel will present you with the results.
<br />
<a href="https://ibb.co/hHPqB1y"><img src="https://i.ibb.co/Y8zrcT3/41.png" alt="36" border="0" /></a>
<br />
<br />
This is what it would look like in the Logs. You can see it has KQL queries.
<br />
A the top under the Run button you can see the KQL syntax, which will come in handy in our next step of creating an analytics rule to detect malicious login from Tor exit nodes.
<br />
<a href="https://ibb.co/hHPqB1y"><img src="https://i.ibb.co/Y8zrcT3/41.png" alt="37" border="0" /></a>
<br />
<br />
Lets go back to our watchlist
<br />
One more really cool thing about watchlists is that they are easily modifiable
<br />
click --> Update watchlist --> Edit watchlist items
<br />
<a href="https://ibb.co/hHPqB1y"><img src="https://i.ibb.co/Y8zrcT3/41.png" alt="38" border="0" /></a>
<br />
<br />
This is particularly useful if you have multiple analytics rules that use the same information
Its easier to upodate the watch list in one place rather than going into each analytics rule and making changes there.
<br />
<br />
<a href="https://ibb.co/hHPqB1y"><img src="https://i.ibb.co/Y8zrcT3/41.png" alt="39" border="0" /></a>
<br />
In the next step let's create our very first analytics rule to detect threats from Tor network.
<br />
<br />





<br>
</br>
<h3>Step 7 - Creating a Detection Rule for our Threat</h3>
<br/>
In MS side bar
Configuration --> Analytics --> Create --> Scheduled Query rule
<br/>
<a href="https://ibb.co/xsLQ5f7"><img src="https://i.ibb.co/JdtPkys/50.png" alt="40" border="0" /></a>
<br />
<br />
General Tab
<br />
Analytics rule details
<br />
Name --> Successful Sign-ins from Tor Network
<br />
Description.
<br />
This rule detects successful sign-ins from the Tor Network, which is a popular tool used by threat actors to anonymize their activity. 
The rule triggers when a successful sign-in event occurs on an account that has a Tor Network IP Address. This could indicate a potential security threat, as legitimate users typically do not use the Tor Network to sign in to an organization's resources.
<br />
Tactics and Techniques
<br />
Initial Access --> T1133 External Remote Access
<br />
Severity  --> High
<br />
<a href="https://ibb.co/xsLQ5f7"><img src="https://i.ibb.co/JdtPkys/50.png" alt="41" border="0" /></a>
<br />
<br />
Set Logic Rule Tab
<br />
Rule query
<br />
let TorNodes = (_GetWatchlist('Tor-IP-Addresses') | project TorIP = IpAddress);
SigninLogs
| where IPAddress in (TorNodes)
| where ResultType !=  50126
| project 
   TimeGenerated,
   Location,
IPAddress,
UserDisplayName,
UserPrincipalName,
UserId,
LocationDetails,
RiskState,
RiskLevelDuringSignIn,
AuthenticationRequirement,
ClientAppUsed, 
ConditionalAccessPolicies
<br />
Click > View query results
<br />
Clic > Test with current data (top right)
<br />
<br />
<a href="https://ibb.co/xsLQ5f7"><img src="https://i.ibb.co/JdtPkys/50.png" alt="42" border="0" /></a>
<br />
<br />
Alert enrichment
Entity in MS are objects that represent important information about the environment such as host, users, IP addresses and many more
Lets add two different entities.
<br />
You can select an entity then you add key value pairs to it.
<br />
Entity Mapping --> Add new entity
<br />
Account (1st Entity)
<br />
Sid  --> UserId
<br />
click Add identifier
<br />
DisplayName -->  UserDisplayName
<br />
IP (2nd Entity)
<br />
Address --> Ipaddress
<br />
Custom details (we can surface any particular event parameters) we have to add key value pairs.
<br />
IPAddress --> IPAddress
<br />
User --> UserDisplayName
<br />
<a href="https://ibb.co/xsLQ5f7"><img src="https://i.ibb.co/JdtPkys/50.png" alt="43" border="0" /></a>
<br />
<br />
Alert Details  (we can make adjustments to our alert detail).
<br />
From the value pairs in our Custom details we can add that in our Alert Name Format and Alert Description Format
<br />
Alert Name Format --> Successful Sign-Ins from Tor Network IP {{IPAddress}}
<br />
Alert Description Format.
<br />
This rule detects successful sign-ins from the Tor Network, which is a popular tool used by threat actors to anonymize their activity. 
The rule triggers when a successful sign-in event occurs on an account {{UserDisplayName}}  that has a Tor Network IP Address. This could indicate a potential security threat, as legitimate users typically do not use the Tor Network to sign in to an organization's resources.
<br />
Alert Property
<br />
ConfidenceScore --> RiskState
<br />
<br />
<a href="https://ibb.co/xsLQ5f7"><img src="https://i.ibb.co/JdtPkys/50.png" alt="44" border="0" /></a>
<br />
<br />
Query Scheduling
Run query every
5 minutes
<br />
Lookup data from the last 5 minutes.
<br />
<a href="https://ibb.co/xsLQ5f7"><img src="https://i.ibb.co/JdtPkys/50.png" alt="45" border="0" /></a>
<br />
<br />
Leave everything else as is.
<br />
<a href="https://ibb.co/xsLQ5f7"><img src="https://i.ibb.co/JdtPkys/50.png" alt="46" border="0" /></a>
<br />
<br />
Incident Tab > Incident settings > Enabled
<br />
Alert grouping
Enabled
<br />
Limit the group to alerts created within the selected time frame leave as --> 5 hours
<br />
Group alerts triggered by this analytics rule into a single incident by leave as (recommended)
<br />
<a href="https://ibb.co/xsLQ5f7"><img src="https://i.ibb.co/JdtPkys/50.png" alt="47" border="0" /></a>
<br />
<br />
Automated ResponseTab.
<br />
None here so we will move on.
<br />
Check out the advanced Sentinel Course we got from Pavel.
<br />
<a href="https://ibb.co/xsLQ5f7"><img src="https://i.ibb.co/JdtPkys/50.png" alt="48" border="0" /></a>
<br />
<br />
Review and Create Tab
<br />
Validation passed --> Save
<br />
<a href="https://ibb.co/xsLQ5f7"><img src="https://i.ibb.co/JdtPkys/50.png" alt="49" border="0" /></a>
<br />
<br />
Our Analytics rule will be created.
<br />
To see our new analytics rule go to:
<br />
Microsoft Sentinel > Analytics > Successful Sign-Ins from Tor Network
<br />
Now what we need to do is impersonate as an attacker. 
Act like a hacker that signed into to an Azure account from an Anonymous IP address.
<br />
This is the really hard part and will test your skills. It was very challenging.
<br />
<a href="https://ibb.co/xsLQ5f7"><img src="https://i.ibb.co/JdtPkys/50.png" alt="50" border="0" /></a>
<br />
<br />


<br>
</br>
<h3>Step 8 - Create a Compromised User Account in Azure for Incident Investigation </h3>
<br/>
We are going to prepare an environment and create a new account that will perform some unusual activity.
We need to turn off security defaults in Azure AD to avoid any potential interruption.
<br/>
Microsoft turns on security as default as it provides crucial baseline level of protection for all users and accounts in organizations.
<br/>
This includes all MFA for all administrators blocking legacy authentications such as imap or smtp which helps reduce risk of attacks like password spray and brute force attacks.
<br/>
Security defaults also require strong passwords. To turn this feature off we will have to move to Entra ID
<br/>
In searchbar > Entra ID
<br/>
Default Directory look at side bar:
<br/>
Manage > Properties
<br/>
At the very bottom click > Manage security defaults
<br/>
<a href="https://ibb.co/LR3MgW2"><img src="https://i.ibb.co/fFsbN5P/53.png" alt="51" border="0" /></a>
<br />
<br />
Change security defaults --> Disabled
Give a reason
other --> testing --> Save
<br />
<br />
<a href="https://ibb.co/LR3MgW2"><img src="https://i.ibb.co/fFsbN5P/53.png" alt="52" border="0" /></a>
<br />
Now we can create a new account.
<br />
Manage --> Users --> New user --> Create New User
<a href="https://ibb.co/LR3MgW2"><img src="https://i.ibb.co/fFsbN5P/53.png" alt="53" border="0" /></a>
<br />
Create new User.
<br />
Basics Tab.
<br />
User principal name --> keysersoze@acloudcallednimbushotmail.onmicrosoft.com
keysersoze@acloudcallednimbushotmail.onmicrosoft.com
Mail nickname --> keysersoze
Display name -->  keysersoze
Password --> [add here]
Account Enabled --> select
<a href="https://ibb.co/LR3MgW2"><img src="https://i.ibb.co/fFsbN5P/53.png" alt="54" border="0" /></a>
<br />
Properties Tab
Identity
First name --> Keyser
Last name --> Soze
User type --> 
<br />
Job Information
Job title --> CISO
Company name --> Kobayashi
Department --> C-Suite
Employee ID --> 1010
Employee type --> Executive
Employee hire date --> Feb 1st 2020
Office location --> Turkey
Manager 
<br />
Contact Information
Street Address
City --> Instanbul
<br />
<a href="https://ibb.co/LR3MgW2"><img src="https://i.ibb.co/fFsbN5P/53.png" alt="55" border="0" /></a>
<br />
Assignments Tab
No need to add a role here
Do it later
<br />
<a href="https://ibb.co/LR3MgW2"><img src="https://i.ibb.co/fFsbN5P/53.png" alt="56" border="0" /></a>
<br />
Review + Create Tab
remember to copy the User principal name and password!
click > Create
<br />
keysersoze@acloudcallednimbushotmail.onmicrosoft.com
<br />
<a href="https://ibb.co/LR3MgW2"><img src="https://i.ibb.co/fFsbN5P/53.png" alt="57" border="0" /></a>
<br />
After the account is created we now need to assign some roles to keyser. Maybe need to refresh.
<br />
So go one step back
Home > Default Directory  | Users
click on keyser soze user
<br />
<a href="https://ibb.co/LR3MgW2"><img src="https://i.ibb.co/fFsbN5P/53.png" alt="58" border="0" /></a>
<br />
Now keyser soze details will show up
Home > Default Directory  | Users > Users >
<a href="https://ibb.co/LR3MgW2"><img src="https://i.ibb.co/fFsbN5P/53.png" alt="59" border="0" /></a>
<br />
on left side bar go to
Manage --> Assigned roles --> Add assignments
<br />
<a href="https://ibb.co/LR3MgW2"><img src="https://i.ibb.co/fFsbN5P/53.png" alt="60" border="0" /></a>
<br />
Add assignments window
Membership tab
Select role --> Security reader
Scope type --> Directory (automatic)
<br />
<a href="https://ibb.co/LR3MgW2"><img src="https://i.ibb.co/fFsbN5P/53.png" alt="61" border="0" /></a>
<br />
Setting tab
Assignment type --> Eligible
Permanently eligible --> selected
Click Assign
<br />
The above didn’t show up in the new Azure Assigned Role
I think Microsoft changed their layouts
<br />
So go to the next section
<br />
in searchbar go to the resource group attached to this project which is either siem-training2 or siem-training3
in the left sidebar
Overview --> Access control (IAM) --> Add --> Add role assignment 
<br />
<a href="https://ibb.co/LR3MgW2"><img src="https://i.ibb.co/fFsbN5P/53.png" alt="62" border="0" /></a>
Assignment type  --> privileged administrator roles
Role --> Contributor --> Next
<br />
<a href="https://ibb.co/LR3MgW2"><img src="https://i.ibb.co/fFsbN5P/53.png" alt="63" border="0" /></a>









<br>
</br>
<h3>Step 9 - Installation of Vulnerable Software </h3>
<br/>
<br />
<p>Older Chrome browser versions Link
![image](https://github.com/chun-eric/nessus-scanner/assets/102393871/6af7e9d6-1e0a-4c86-86e3-3d59e58edc8a)
</p>
<p>https://www.slimjet.com/chrome/google-chrome-old-version.php
![image](https://github.com/chun-eric/nessus-scanner/assets/102393871/7f7a6766-3269-45e2-8d27-893944a58285)
</p>
<br/>
<p>We will download a version from 2020.</p>
<p>Lets add an outdated google chrome.</p>
<p>Why?</p>
<p>Because people keep passwords in google chrome and this could be a huge vulnerability.</p>
<p>An attacker might gain access to our password.</p>
<br/>
<p>Go to the above link I added.</p>
<br/>
<p>Lets download version:</p>
<p>61.0.3163.79</p>
<br />
<a href="https://imgbb.com/"><img src="https://i.ibb.co/Jx60P2j/54.png" alt="54" border="0" /></a>
<br />
<br />
<p>In order to get vulnerable software in the VM we need to change adapter setting again and put it back later.</p>
<p>Its annoying but necessary.</p>
<p>In the VM go to Devices > Network > Network Settings > Bridged Adapter.</p>
<p>Bridged Adapter will give you internet connectivity.</p>
<br />
<p>Open installation wizard and install software.</p>
<p>We don’t want Chrome to automatically update we need to do a few things.</p>
<p>In Searchbar search for System Configuration.</p>
<p>In the  Services Tab disable Google Update Service (gupdate and gupadatem).</p>
<br/>
<p>Click OK > Restart Computer.</p>
<p>Now Chrome updates will be disabled.</p>
<br />
<a href="https://imgbb.com/"><img src="https://i.ibb.co/5KGrsXm/55.png" alt="55" border="0" /></a>
<br />
<br />
<p>Lets also make advanced sharing settings in Network and Sharing Center  in control panel to :</p>
<p>Network Discovery > Turn on</p>
<p>File and printer sharing > Turn on</p>
<br />
<a href="https://ibb.co/HdXC9m3"><img src="https://i.ibb.co/vYDhM2R/56.png" alt="56" border="0" /></a>
<br />
<br />
<p>If you check Chrome updates it wont be able to perform any updates.</p>
<br />
<p>To make our windows even more vulnerable.</p>
<p>We will download an older version of minecraft with the log4j vulnerability.</p>
<p>What was log4j?</p>
<p>Individuals learned that games chat was being logged using log4j and if they entered malicious code into the chat, it led to remote code execution to be set.</p>
<p>Above 50% of orgs use the Log4j library ---> HOLY SH*****T!</p>
<br />
<p>Older Minecraft Server.</p>
<p>https://mcversions.net/download/1.18.1</p>
<p>It was a huge vulnerability that shook the entire cybersecurity.</p>
<br/>
<p>So what we will do is download an older version of Minecraft server affected by this vulnerability.</p>
<br/>
<p>Check out the link for the minecraft net to download older versions of minecraft.</p>
<p>https://mcversions.net</p>
<br/>
<p>Filter by year 2021.</p>
<p>Choose Minecraft 1.18.1 Server Hosting.</p>
<p>Server Jar > Download Server Jar.</p>
<br />
<a href="https://ibb.co/jZDNXb5"><img src="https://i.ibb.co/7NKfmjz/57.png" alt="57" border="0" /></a>
<br />
<br />
<p>Because we don’t have a program to extract we will download an older zip extract vulnerable software.</p>
<p>Its an older version of 7zip.</p>
<br />
<p>Link to older 7zip below.</p>
<p>https://7-zip.en.uptodown.com/windows/versions</p>
<p>lets get a 2019 version.</p>
<br />
<p>Choose version 19.00.</p>
<p>Download and Install.</p>
<br/>
<br />
<a href="https://imgbb.com/"><img src="https://i.ibb.co/k5XMqv6/58.png" alt="58" border="0" /></a>
<br />
<br />
<p>Now extract Minecraft server file with 7zip.</p>
<p>Very IMPORTANT!</p>
<p>We must change our network adapter again back to Host-only Adapter!</p>
<p>This is because we don’t want the VM having access to the outside Internet. Hmm maybe we don’t have to change it.</p>
<p>Now we can find a lot of vulnerabilities if we scan again</p>
<br />
<br />
<a href="https://imgbb.com/"><img src="https://i.ibb.co/txTNWK5/59.png" alt="59" border="0" /></a>
<br />
<br />


<br></br>
<h3>Step 10 - Advanced Nessus Scanning for Vulnerable Software </h3>
<br />
<p>Ok now lets run another scan.</p>
<p>After the scan you can see a tonne of vulnerabilities!</p>
<br />
<a href="https://ibb.co/sj8VQnP"><img src="https://i.ibb.co/SNF30bx/60.png" alt="60" border="0" /></a>
<br />
<br />
<p>We have Google critical vulnerabilities.</p>
<br />
<a href="https://ibb.co/ChBsMnC"><img src="https://i.ibb.co/t8BPxbj/61.png" alt="61" border="0" /></a>
<br />
<br />
<p>One of the vulnerabilities is Apache Log4j.</p>
<p>We can see that this vulnerability could be a huge issue.</p>
<p>The Apache log4j if you click on it one of the vulnerability scores a CVSS score of 10.0!</p>
<br />
<a href="https://ibb.co/vZPnV3T"><img src="https://i.ibb.co/Ch7GmbY/62.png" alt="62" border="0" /></a>
<br />
<br />
<p>if you click on the link we can see more in depth description , solutions and output.</p>
<p>Lets take a look at another example.</p>
<p>If you click on the Google Chrome Vulnerability you can see there are many critical vulnerabilities.</p>
<p>Take a note of the CVE number.</p>
<br />
<a href="https://ibb.co/Lr3dnLy"><img src="https://i.ibb.co/68k0g3S/63.png" alt="63" border="0" /></a>
<br />
<br />
<p>Now its up to you to dig in and use the solution guide from Nessus to fix all the vulnerabilities.</p>
<p>Okay another good thing about Nessus is that it can generate.</p>
<p>REPORTS!</p>
<p>Creating a report is very easy to do!</p>
<p>Click Report button on top right.</p>
<p>We can then share this report to our dev team for instance.</p>
<br />
<a href="https://ibb.co/ZKKjrRr"><img src="https://i.ibb.co/1KKgNYN/64.png" alt="64" border="0" /></a>
<br />
<br />
<p>Now will update our system and enable automatic updates for applications. Also any malicious files that could potentially cause issues in the future will be removed.</p>




<br></br>
<h3>Step 11 - Remediation of Vulnerabilities </h3>
<br />
<p>Remediation is actually quite boring but its an absolutely necessary in cybersecurity.</p>
<p>Make sure you are on bridge network adapter or NAT to connect to the internet.</p>
<p>Lets tackle each vulnerability from highest to lowest.</p>
<p>First we need to ensure that our system is receiving the latest security patches.</p>
<p>Go to Settings > Windows Update > Resume Updates.</p>
<br />
<a href="https://ibb.co/Wnj4vR6"><img src="https://i.ibb.co/F6CL4dK/65.png" alt="65" border="0" /></a>
<br />
<br />
<p>Next turn on the updates for Google Chrome.</p>
<p>System Configuration >  Services > Turn on both Google Update Service > Apply > OK</p>
<br />
<a href="https://imgbb.com/"><img src="https://i.ibb.co/sb7c76M/66.png" alt="66" border="0" /></a>
<br />
<br />
<p>Check that the firewall is on. We are also receiving firewall message saying to turn it on.</p>
<p>Go to wf.msc</p>
<p>turn on firewall properties for Domain, Private, Public.
</p>
<p>*NOTE* -- Best to do this process at the end as it will affect the scanner.</p>
<br />
<a href="https://imgbb.com/"><img src="https://i.ibb.co/DR4qC83/67.png" alt="67" border="0" /></a>
<br />
<br />
<p>Next remove any threats.</p>
<p>In our case remove Minecraft server file and old outdate zip files from the Downloads folder.</p>
<br />
<a href="https://ibb.co/Sy6ZLnc"><img src="https://i.ibb.co/MSgJXVf/68.png" alt="68" border="0" /></a>
<br />
<br />
<p>Look at the Remediation tab. This will give you a high level overview of what to do.</p>
<br />
<a href="https://ibb.co/HqbnZRj"><img src="https://i.ibb.co/VTz26bs/69.png" alt="69" border="0" /></a>
<br />
<br />
<p>Confirm all updates are completed.</p>
<p>Settings > Windows Update > Retry.</p>
<p>Windows Update Patches should be downloading.</p>
<br />
<a href="https://ibb.co/BgW72YN"><img src="https://i.ibb.co/THjnMNL/70.png" alt="70" border="0" /></a>
<br />
<br />
<p>Reboot the system.</p>
<p>Run one more scan to see if vulnerabilities are fixed.</p>
<p>Delete 7-zip or update it to the latest version.</p>
<br />
<br />
<a href="https://ibb.co/NVjxGKT"><img src="https://i.ibb.co/tmsB1DL/71.png" alt="71" border="0" /></a>
<br />
<br />




<br></br>
<h3>Step 12 - Conclusion and Final Scan Check </h3>
<br />
<p>Run the scan again to check for any vulnerabilities.</p>
<p>If no vulnerabilities, great!</p>
<p>Turn on firewall and everything should be remediated.
![image](https://github.com/chun-eric/nessus-scanner/assets/102393871/816fc07b-d40a-425c-939b-d522ccc83d6b)
</p>
<br />
<a href="https://ibb.co/hXSFNwM"><img src="https://i.ibb.co/TKsMGdT/72.png" alt="72" border="0" /></a>
<br />
<br />

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
