
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
<br/>
<br/>
It also configures Content Hub solutions.
<br/>
If you look at some of the data connectors some are billed and some are free. 
We don’t need to worry about spending anything. 
<br/>
<br/>
When we first deploy Sentinel, we get 10gb ingestion per day for the first month.
That’s plenty to go through all the necessary tasks. 
All we need to do is make sure to delete Sentinel and the entire resource group,
so we don't rack up a huge bill!
<br/>
<br/>
Whenever we want to spin up a fresh new Sentinel just Deploy to Azure the Sentinel All in One.
<br/>
<br/>
(https://github.com/Azure/Azure-Sentinel/tree/master/Tools/Sentinel-All-In-One#readme
)
<br/>
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="1" border="0" /></a>
<br/>
<br/>



<br></br>
<h3>Step 2 - Deploying Sentinel to Azure</h3>

```
Click > Deploy to Azure
```
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="2" border="0" /></a>
<br/>
Now that we are in the custom deployment. Go to:
<br/>
Basic Tab and choose:
<br/>

```
Subscription --> Azure 1
```
<br/>

```
Location --> East US
```
Usually East US is the cheapest. However for this project, we will choose the closest one to our current location -- Japan.
<br/>

```
Resource Group Name:  (pick a name to reflect what the solution is about) --> Security-Monitoring-rg
```
<br/>

```
Workspace Name --> same as resource group name
```
<br/>

```
Daily ingestion limit in GBs. --> 10
```
<br/>

```
Number of days of retention --> 90
```
This means it's free for the first 90 days. After that we have to pay for every Gigabyte of data! Yikes!
<br/>

```
Select pricing tier for Log Analytics --> Pay-as-you-go (leave it as is)
```
<br/>

```
Select pricing tier for Sentinel --> Pay-as-you-go (leave it as is)
```
<a href="https://ibb.co/M75HdHK"><img src="https://i.ibb.co/6FJSQSG/2.png" alt="3" border="0" /></a>
<br />
<br />
In the settings tab:
<br/>

```
Enable Sentinel Health Diagnostics --> Select
```
<br/>
We will Enable User Entity Behavior Analytics (UEBA) later together.
<a href="https://ibb.co/M75HdHK"><img src="https://i.ibb.co/6FJSQSG/2.png" alt="4" border="0" /></a>
<br/>
<br/>
In the Content Hub Solutions Tab.
<br/>
There are three content categories for solutions.
<br/>
<br/>

```
Select Microsoft Content Hub Solutions --> Select All
```
<br/>

```
Select Essential Content Hub Solutions --> Select All
```
<br/>

```
Select Training and Tutorials Content Hub Solutions --> Select All
```
<a href="https://ibb.co/M75HdHK"><img src="https://i.ibb.co/6FJSQSG/2.png" alt="5" border="0" /></a>
<br/>
<br/>
In the Data Connectors Tab:
<br/>

```
Select data connectors to onboard --> select all
```
<br/>
If we don’t have permissions for some data connectors, it will simply fail, so don’t worry.
<br/>

```
Select Azure Active Directory log types to enable --> Select all
```
<br/>
<a href="https://ibb.co/M75HdHK"><img src="https://i.ibb.co/6FJSQSG/2.png" alt="6" border="0" /></a>
<br/>
<br/>
In the Analytics Rules Tab:
<br/>

```
Enable Scheduled alert rules for selected Content Hub solutions and Data Connectors --> Select
```
<br/>
we don’t want to enable hundreds of scheduled alert from the content hub manually.
<br/>

```
Select the severity of the rules to enable --> Select all
```
<a href="https://ibb.co/M75HdHK"><img src="https://i.ibb.co/6FJSQSG/2.png" alt="7" border="0" /></a>
<br/>
<br/>

```
Review + create Tab
```
<br/>
Check all the details.
<br/>

```
Click > Create
```
<br/>
This will take 10-15 minutes to deploy, so grab a cup of coffee.
<br/>
During this process you may encounter some failures because you wont have the license for some.
<a href="https://ibb.co/M75HdHK"><img src="https://i.ibb.co/6FJSQSG/2.png" alt="8" border="0" /></a>
<br/>
<br/>
We are getting an authentication error.
<br/>
<a href="https://ibb.co/M75HdHK"><img src="https://i.ibb.co/6FJSQSG/2.png" alt="9" border="0" /></a>
<br/>
<br/>
It's stating, we need to change user write permissions to enable this deployment.
<br/>
In the Azure All in One readme file in Github, it states:
<br/>

```
Azure user account with enough permissions to enable the desired connectors. See table at the end of this page for additional permissions.
```

Write permissions to the workspace are always needed.
<br/>
After a while of researching,  I created the resource group siem-training and gave owner privileges.
<br/>

```
Click Role Assignment >  Add > Add role assignment.
```
<a href="https://ibb.co/M75HdHK"><img src="https://i.ibb.co/6FJSQSG/2.png" alt="10" border="0" /></a>
<br/>
<br/>
In the Role Tab:
Select role by clicking on View. We chose the Privileged Admin role.
<br/>
<a href="https://ibb.co/M75HdHK"><img src="https://i.ibb.co/6FJSQSG/2.png" alt="11" border="0" /></a>
<br/>
<br/>

```
Select members.
```
<a href="https://ibb.co/M75HdHK"><img src="https://i.ibb.co/6FJSQSG/2.png" alt="12" border="0" /></a>
<br/>
<br/>

```
Select role assignments and members.
```
<a href="https://ibb.co/M75HdHK"><img src="https://i.ibb.co/6FJSQSG/2.png" alt="13" border="0" /></a>
<br/>
<br/>
The role assignment we set is constrained to the owner.
<br/>
<a href="https://ibb.co/M75HdHK"><img src="https://i.ibb.co/6FJSQSG/2.png" alt="14" border="0" /></a>
<br/>
<a href="https://ibb.co/M75HdHK"><img src="https://i.ibb.co/6FJSQSG/2.png" alt="15" border="0" /></a>
<br/>
<a href="https://ibb.co/M75HdHK"><img src="https://i.ibb.co/6FJSQSG/2.png" alt="16" border="0" /></a>
<br/>
Good seems like it all worked now. Issue solved. Yes!
<a href="https://ibb.co/M75HdHK"><img src="https://i.ibb.co/6FJSQSG/2.png" alt="17" border="0" /></a>
<br/>
<br/>



<br/>
<br/>
<h3>Step 3 - Exploring Artifacts Installed</h3>
There will be some errors for enableDataConnectors. For Status Message look for InvalidLicense.
We can ignore it for now.
<br/>
<br/>
Now lets go to the resource group associated with this Sentinel. Type in search bar siem-training2.
<br/>
<br/>

```
Click on Overview.
```
There should be over 390 records but I only have 9 records. Is this right?
So I made a new deployment called siem-training3 and will compare between the two.
<br/>
<br/>
I created siem-training3 and it is exactly the same as siem-training2.
We can ignore the 9 records. 
<br />
<br/>
<a href="https://ibb.co/348nntQ"><img src="https://i.ibb.co/gmXxxsh/13.png" alt="18" border="0" /></a>
<br/>
<br/>
This is where all the data is stored. However most of them are just templates.
<br/>
<br/>
For instance if you want to deploy a package from the Content Hub in Sentinel, it includes 50 analytics rules and it will create a separate template for each.
<br/>
<br/>
Lets choose one to see whats inside (ignore this section - its not applicable in the recent deployment)
<br/>
<br/>
In a template you can see Display Name and Description on the right side. So that’s basically how the solution looks in the background
<br/>
<br/>
Microsoft Sentinel is built on top of the log analytics workspace where all the data is stored.
We want to monitor Sentinel as well as the log analytics workspace.
But why?
<br/>
<br/>
This is because we want to be aware of situations where our queries don’t perform as they should, or maybe it takes too long and ends up in error.
<br/>
<br/>
Let's go back to our resource group and find the log analytics workspace. For us it should be:
<br/>
<br/>

```
siem-training2 (type Log Analytics workspace) > Click
```
<a href="https://ibb.co/348nntQ"><img src="https://i.ibb.co/gmXxxsh/13.png" alt="19" border="0" /></a>
<br/>
<br/>
On the Monitoring blade:

```
Click Diagnostic settings --> Add diagnostic setting
```
<a href="https://ibb.co/348nntQ"><img src="https://i.ibb.co/gmXxxsh/13.png" alt="20" border="0" /></a>
<br/>
<br/>
What is Diagnostic settings?
<br/>
<br/>
Diagnostic settings are used to configure streaming export of platform logs and metrics for a resource to the destination of your choice. 
<br/>
<br/>
You may create up to five different diagnostic settings to send different logs and metrics to independent destinations.
<br/>

```
Diagnostic setting name -->  Sentinel
```

```
Logs --> Category groups --> allLogs (select)
```


```
Metrics --> AllMetrics (select)
```


```
Destination details --> Send to Log Analytics workspace --> Subscription (choose) --> Log Analytics workspace --> security-monitoring (japanwest)
```


```
Click > Save
```
<a href="https://ibb.co/348nntQ"><img src="https://i.ibb.co/gmXxxsh/13.png" alt="21" border="0" /></a>
<br/>
<br/>
Now we are all setup! We can move on to Sentinel itself.


<br>
</br>
<h3>Step 4 - Exploring Sentinel </h3>
Okay, let's start exploring Microsoft Sentinel.
Search for Sentinel in the search bar. 
<br/>
<br/>
Choose your resource group > siem-training2
<br/>
<br/>
<a href="https://ibb.co/ZTJHTrJ"><img src="https://i.ibb.co/bd3HdV3/14.png" alt="22" border="0" /></a>
<br/>
<br/>

```
Click > Microsoft Sentinel (MS)
```
Overview gives you a dashboard overview. On the left side MS is divided into 4 sections.
<br/>
<br/>
```
General
```

```
Threat Management
```


```
Content Management
```


```
Configuration
```

<a href="https://ibb.co/ZTJHTrJ"><img src="https://i.ibb.co/bd3HdV3/14.png" alt="23" border="0" /></a>
<br/>
<br/>
Lets focus on some of the most interesting tabs in Sentinel.
<br/>

```
Click General > Logs
```
Here we can search for you data using Kusto Query Language or KQL
You can see all the tables by click on the all the triangle icons.
<br/>
e.g   LogManagement, Microsoft Sentinel, Custom Logs
<br/>
<br/>
One of the tables we need to use are signin log, which is currently missing.
<br/>
<br/>
For now lets check AzureActivity.
This table provides information actions inside your portal and provides information about who performed the action and other properties important for investigation.
<br/>
<br/>
Let's also check AADNonInteractiveUserSignInLogs (we don’t have that now siem-training2 which is v2 but in siem-training3 v1 it has) this provides information about authentication requirements, client usage and location details
location details will be particulary useful for us.
<br/>
<br/>
<a href="https://ibb.co/ZTJHTrJ"><img src="https://i.ibb.co/bd3HdV3/14.png" alt="24" border="0" /></a>
<br/>
<br/>
Lets have a look at the Configuration Section.
<br/>
<br/>

```
Configuration --> Data Connectors.
```
We should have around 9-10 Connected. We can filter by Status: Connected.
<br/>
This lets us see the type of data being collected, number of logs received and tables that are being populated.
<br/>
<br/>
<a href="https://ibb.co/ZTJHTrJ"><img src="https://i.ibb.co/bd3HdV3/14.png" alt="25" border="0" /></a>
<br/>
<br/>

```
Configuration --> Analytics
```
Do you remember the hundreds of templates inside your resource group?
<br/>
<br/>
Most of them were for analytics rules that were automatically deployed and enabled for you.
There are already roughly around a staggering detection rules already built and provided by Microsoft, this is great to get started with monitoring threats.
<br/>
<br/>
Lets have a look at some  high severity just to see what rules have been broken?
<br/>
<br/>
<a href="https://ibb.co/ZTJHTrJ"><img src="https://i.ibb.co/bd3HdV3/14.png" alt="26" border="0" /></a>
<br/>
<br/>
Lets also have a look at Anomalies.
<br/>
<br/>
There are anomalies templates developed to be robust by using thousands of data sources and millions of events.
<br/>
<br/>
Microsoft allows you to change the thresholds for them in case they generate a lot of false positives.
Right now I don’t even know what a false positive looks like! 
<br/>
<br/>
Note that the  Anomalies template works with the User and Entity Behavior Analytics which is currently not working
<br/>
<br/>
In the next step, our very first task in Microsoft Sentinel will be to fix this very issue!
<br/>
<br/>
<a href="https://ibb.co/ZTJHTrJ"><img src="https://i.ibb.co/bd3HdV3/14.png" alt="27" border="0" /></a>
<br/>





<br>
</br>
<h3>Step 5 - Enabling User Entity Behavior Analytics and Playbooks  </h3>
The user and entity behavior analytics (UEBA) is an amazing feature that uses AI to detect and alert you to any unusual behavior happening within your system.
<br/>
<br/>
To turn this feature we have to go to:
<br/>

```
Configuration --> Settings --> Settings --> Set UEBA
```
<a href="https://ibb.co/ZTJHTrJ"><img src="https://i.ibb.co/bd3HdV3/14.png" alt="28" border="0" /></a>
<br/>
<br/>
You will come to a new screen called Entity behavior Configuration.
<br/>
<br/>
Turn on the UEBA feature.

```
Click > On
```
<br/>
Sync Microsoft Sentinel with at least one of the following directory services.

```
Microsoft Entra ID (select) > Apply
```
<br/>
Select existing data sources you want to enable for entity behavior analytics. 

```
Select Audit Logs, Azure Activity , Signin Logs > Apply
```
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
<br/>

```
Configuration > Settings > Settings > Playbook permissions > Configure permissions
```
<br/>
<a href="https://ibb.co/ZTJHTrJ"><img src="https://i.ibb.co/bd3HdV3/14.png" alt="30" border="0" /></a>
<br/>
<br/>
Manage permissions panel will show up to the right.
<br/>

```
Select resource group siem-training2 > Apply
```
<br/>
<br/>
Now we are all set up and ready to create some amazing artifacts within Microsoft Sentinel.
<br/>
<br/>
In the next step let's dive into watchlists.
<br/>
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
<br />
Members --> Select members --> Choose keysersoze --> Select --> Review + Assign
<br />
<a href="https://ibb.co/LR3MgW2"><img src="https://i.ibb.co/fFsbN5P/53.png" alt="64" border="0" /></a>
<br />
Click > Review + assign
<br />
<a href="https://ibb.co/LR3MgW2"><img src="https://i.ibb.co/fFsbN5P/53.png" alt="65" border="0" /></a>
<br />
Lets check if it worked, go back to the resource group we are working with.
<br />
Overview --> Access control (IAM) --> Role assignments --> Check Contributor [keysersozai]
<br />
If its all there now we can log into this account
<br />
<a href="https://ibb.co/LR3MgW2"><img src="https://i.ibb.co/fFsbN5P/53.png" alt="66" border="0" /></a>
<br />
lets use a different browser for this use brave. Sign into Microsoft Azure.
<br />

Enter the username and password we created for keyser soze
<br />
keysersoze@acloudcallednimbushotmail.onmicrosoft.com
<br />
Before we login with the correct credentials we need to create a new password as Microsoft will ask you to change it.
Try to use a password that’s been used for brute force attackes like password. You will notice Microsoft trying to prevent us from using commonly used passwords. 
<br />
Next try an alphanumeric combination and login:
<br />
keysersoze@acloudcallednimbushotmail.onmicrosoft.com
<br />
Go to the Azure portal to sign in and voila we are in!
<br />
<a href="https://ibb.co/LR3MgW2"><img src="https://i.ibb.co/fFsbN5P/53.png" alt="67" border="0" /></a>
Go to Microsoft Entra ID with our new account
<br />
Click Overview to see everything is in order.
<br />
<a href="https://ibb.co/LR3MgW2"><img src="https://i.ibb.co/fFsbN5P/53.png" alt="68" border="0" /></a>
<br />
Next check out our resource group. 
<br />
Search resource group in search bar. Click on our resource group to confirm.
<br />
Okay we are in the portal and it looks like everything is in order.
<br />
MFA is disabled for this account,  lets see if MS will catch this. Lets have some fun!
<br />
<a href="https://ibb.co/LR3MgW2"><img src="https://i.ibb.co/fFsbN5P/53.png" alt="69" border="0" /></a>
<br />







<br>
</br>
<h3>Step 9 - Generating an Incident from the Compromised Account </h3>
<br/>
I found this video and the next video to be the most challenging use Brave browser and Tor network right side menu view brave browser in private window with TOR.
<br/>
Time to put on our red hat.
<br />
<a href="https://ibb.co/LR3MgW2"><img src="https://i.ibb.co/fFsbN5P/53.png" alt="70" border="0" /></a>
<br/>
Go to portual.azure.com.
<br/>
Use your credentials of Keyser Soze to login:
<br/>
keysersoze@acloudcallednimbushotmail.onmicrosoft.com
<br/>
We are logged in with the TOR Relay in Brave Browser
<br/>
<a href="https://ibb.co/LR3MgW2"><img src="https://i.ibb.co/fFsbN5P/53.png" alt="71" border="0" /></a>
<br/>
Once we are logged in, the very first thing a hacker would most likely do is establish persistence
<br/>
Change Account Password:
<br/>
Top right click your account details and change your password from Overview.
<br/>
old password -->   
<br/>
new password -->  make the new password really long and complex
<br/>
<a href="https://ibb.co/LR3MgW2"><img src="https://i.ibb.co/fFsbN5P/53.png" alt="72" border="0" /></a>
<br/>
Change MFA settings (however we are not doing that here)
<br/>
Change resource group diagnostic settings. This is another thing a hacker might do. This is because the hacker doesn’t want activities logged.
<br/>
Go to our resource group --> Monitoring --> Diagnostic settings
<br/>
Click on the resource group log analytics workspace name.
<br/>
Click Edit setting.
<br/>
<a href="https://ibb.co/LR3MgW2"><img src="https://i.ibb.co/fFsbN5P/53.png" alt="73" border="0" /></a>
<br/>
Click > Delete
<br/>
<a href="https://ibb.co/LR3MgW2"><img src="https://i.ibb.co/fFsbN5P/53.png" alt="74" border="0" /></a>
<br/>
Confirm there is no diagnostic settings.
<br/>
<a href="https://ibb.co/LR3MgW2"><img src="https://i.ibb.co/fFsbN5P/53.png" alt="75" border="0" /></a>
<br/>
Turn off Diagnostic settings in microsoft sentinel in searchbar type microsoft sentinel since we changed our password we might have to sign in again.
<br/>
In left side bar:
<br/>
Configuration --> Settings --> Settings --> Auditing and health monitoring --> Configure Diagnostic settings --> Edit settings --> Delete
<br/>
<a href="https://ibb.co/LR3MgW2"><img src="https://i.ibb.co/fFsbN5P/53.png" alt="76" border="0" /></a>
<br/>
<a href="https://ibb.co/LR3MgW2"><img src="https://i.ibb.co/fFsbN5P/53.png" alt="77" border="0" /></a>
<br/>
<a href="https://ibb.co/LR3MgW2"><img src="https://i.ibb.co/fFsbN5P/53.png" alt="78" border="0" /></a>
<br/>
Confirm there is no diagnostic settings.
<br/>
<a href="https://ibb.co/LR3MgW2"><img src="https://i.ibb.co/fFsbN5P/53.png" alt="79" border="0" /></a>
<br/>
An attacker might also escalate privileges and gain additional access to the environment by using Azure Portals Web shell interface to run scripts and different tools like Microburst or Azure Hound to search for sensitive information.
<br/>
Attackers might also perform crypto mining they will spin up expensive resources resulting in a large bill
We can follow along with the tutorial or not Lets create a VM within the same resource group. This is very sneaky. 
<br/>
Lets give the VM a very generic name.
<br/>
Dev-Temporary-VM
<br/>
This time I changed Admin account authentication type to Password. This might help me get validation to create a VM. 
<br/>
Select> Password
<br/>
Username > azureuser
<br/>
Password > [Add yours here]
<br/>
Inbound port rules
<br/>
Allow selected ports
<br/>
SSH (22)
<br/>
<a href="https://ibb.co/LR3MgW2"><img src="https://i.ibb.co/fFsbN5P/53.png" alt="80" border="0" /></a>
<br/>
<a href="https://ibb.co/LR3MgW2"><img src="https://i.ibb.co/fFsbN5P/53.png" alt="81a" border="0" /></a>
<br/>
<a href="https://ibb.co/LR3MgW2"><img src="https://i.ibb.co/fFsbN5P/53.png" alt="81b" border="0" /></a>
<br/>
Leave everything as is with Disks.
<br/>
<a href="https://ibb.co/LR3MgW2"><img src="https://i.ibb.co/fFsbN5P/53.png" alt="82" border="0" /></a>
<br/>
Networking credentials.
<br/>
<a href="https://ibb.co/LR3MgW2"><img src="https://i.ibb.co/fFsbN5P/53.png" alt="83" border="0" /></a>
<br/>
Click > Review + Create
<br/>
Click > Create
<br/>
After all that I found out I don’t have authorization to perform this action. 
<br/>
However after I changed to Adminstrator Authentication with username and password when setting up the VM, validation passed and I could create our VM!
<br/>
<a href="https://ibb.co/LR3MgW2"><img src="https://i.ibb.co/fFsbN5P/53.png" alt="84" border="0" /></a>
<br/>
Our VM has been created! Now the VM is created lets try running Cloud Shells as an attacker.
<br/>
<a href="https://ibb.co/LR3MgW2"><img src="https://i.ibb.co/fFsbN5P/53.png" alt="85" border="0" /></a>
<br/>
Click on the Cloud Shell button next to the bell Icon top right, then select Powershell.
<br/>
<a href="https://ibb.co/LR3MgW2"><img src="https://i.ibb.co/fFsbN5P/53.png" alt="86" border="0" /></a>
<br/>
<a href="https://ibb.co/LR3MgW2"><img src="https://i.ibb.co/fFsbN5P/53.png" alt="87" border="0" /></a>
<br/>
Click > Create Storage
<br/>
You might get a message saying: "You have no storage mounted".
<br/>
We need  to have a storage account which is used to store all users files and scripts as well as other data such as session history and environment settings and preferences.
<br/>
<a href="https://ibb.co/LR3MgW2"><img src="https://i.ibb.co/fFsbN5P/53.png" alt="88" border="0" /></a>
<br/>
Because we didn’t run Cloud shell in the past, we will need to create a new storage.
<br/>
If we click Create Storage we will get a failure "Storage creation failed".
<br/>
This is because we don’t have privileges to create a new resource group.
<br/>
However there is another way.
<br/>
Click Show advanced settings.
<br/>
Then we get some options.
<br/>
Resource group --> Use existing.
<br/>
Storage account --> storage1849756184.
<br/>
File share  account --> storage1849756184.
<br/>
Click > Create Storage
<br/>
<a href="https://ibb.co/LR3MgW2"><img src="https://i.ibb.co/fFsbN5P/53.png" alt="89" border="0" /></a>
<br/>
After a minute or so the terminal window will load up. We have succesfully launched a cloud shell!
<br/>
Now we have to wait for MS to detect threats.
<br/>
<a href="https://ibb.co/LR3MgW2"><img src="https://i.ibb.co/fFsbN5P/53.png" alt="90" border="0" /></a>
<br/>
Our detection rule will run every 5 minutes
<br/>
In the next step, we will explore the newly created incidents in Sentinel and how to respond to these threats
<br/>
I have been trying to add a new assigned role to keyser.
<br/>
I went to our storage account and created a new container.
<br/>
Containers > first-container
<br/>
Click > Upload
<br/>
Technically if you were a real hacker you could upload anything. 
<br/>
Like a bad script, jpg, malware I suppose etc..
<br/>
I made two text files: log5j.txt and wannasleep.txt.
<br/>
<a href="https://ibb.co/LR3MgW2"><img src="https://i.ibb.co/fFsbN5P/53.png" alt="91" border="0" /></a>
<br/>
Lets see if sentinel picked up on these actions. We will wait 24 hours. 
<br/>











<br></br>
<h3>Step 10 - Analysing the Security Incident in Sentinel </h3>
<br />
Okay now go back to your original Azure account.
<br />
Go to the Microsoft Sentinel Dashboard. Look at the incidents.
<br />
Okay so how do we get started?
<br />
Go to Threat management > Incidents  > Manage Incidents
<br />
<a href="https://ibb.co/sj8VQnP"><img src="https://i.ibb.co/SNF30bx/60.png" alt="92" border="0" /></a>
<br />
We can see all the incidents (note pics can be either siem-trainin2 or siem-training3).
<br />
One of the SOC Analysts questions you might be asked is:
<br />
How would you prioritize incidents that are currently open and present?
<br />
The answer is complicated…
<br />
Well it depends on the situation but most of the time we would prioritize incidents based on the highest severity
<br />
For us the highest severity would be successful Sign-in from Tor Network. Look at all the IP addresses. We can see it because of the analytics rule we specified. One thing we are noticing is that a certain IP address is showing up twice and within close time proximity.
<br />
This could already indicate a flaw in our analytic rule that we created. Below is from siem-training2.
<br />
<a href="https://ibb.co/sj8VQnP"><img src="https://i.ibb.co/SNF30bx/60.png" alt="93" border="0" /></a>
<br />
Click > View Full Details (bottom right) to get more detailed information.
<br />
<a href="https://ibb.co/sj8VQnP"><img src="https://i.ibb.co/SNF30bx/60.png" alt="94" border="0" /></a>
<br />
If you look at our incident in the high severity, we can already see there is a flaw in our analytics.
The Tor Network IP address is the same on two different days.
<br />
We should probably have to fix that later.
<br />
<a href="https://ibb.co/sj8VQnP"><img src="https://i.ibb.co/SNF30bx/60.png" alt="95" border="0" /></a>
<br />
Lets check the other incidents quickly before jumping into the high severity cases. You can sort incidents by Incident ID or Created time.
<br />
** Below can probably be seen from siem-training3 not siem-training2 **
<br />
Some of incidents are:
<br />
Under low severity there was a title: User account created without expected attributes defined
<br />
Under low severity there was a title: Anomolous Single Factor Signin
<br />
Under medium there was a title: Service Principal Authentication Attempt from New Country
<br />
Under medium there was a title: Insider Risk Risky User Access by Application
<br />
Under medium there was a title: Anomalous Sign-in location by user account and authenticating application
<br />
Under high there was a title: Successful Sign-Ins from Tor Network [ip address]
<br />
** Below is shown in siem-training2 ** 
Another incident is:
<br />
Under low there is a title: Suspicious Resource Deployment.
<br />
<a href="https://ibb.co/sj8VQnP"><img src="https://i.ibb.co/SNF30bx/60.png" alt="96" border="0" /></a>
<br />
So if we looked at all these incidents it could mean all the incidents are related together right? Why do we have so many?
<br />
Why doesn’t Sentinel take them all and create one with all of the information presented?
<br />
Answer is very simple:
<br />
The analytics rules are not correctly created to show you. If you click on any incidents on the right side panel under Entities = 0
<br />
Why is this important?
<br />
If the entities are not correctly setup, it wont be able to perform such correlation.
<br />
It seems like for siem-training2 the entities are correctly setup.
<br />
I am still confused on this section, the most.
<br />
What are entities, and why do we need to correctly set them up?
<br />
<a href="https://ibb.co/sj8VQnP"><img src="https://i.ibb.co/SNF30bx/60.png" alt="97" border="0" /></a>
<br />
Lets move on for the time being.
<br />
Lets say you want to start the investigation. You want to investigate the high severity incidents.
<br />
Click on all high severity incidents.
<br />
At the very top middle --> click Actions.
<br />
<a href="https://ibb.co/sj8VQnP"><img src="https://i.ibb.co/SNF30bx/60.png" alt="98" border="0" /></a>
<br />
Actions side panel to the right will show up. We have three boxes.
<br />
Severity > High
<br />
Owner --> Assign to me
<br />
Status --> Active
<br />
Click > Apply
<br />
<a href="https://ibb.co/sj8VQnP"><img src="https://i.ibb.co/SNF30bx/60.png" alt="99" border="0" /></a>
<br />
Now if you go back to all the incidents, the owner of all the high incidents have been changed to me as well the status of the incident being changed to Active.
<br />
This is very important step especially if you are part of security operations team. All team members are aware of who is handling the incident
<br />
Now the incidents are assigned to you, we can start the investigation process.
<br />
<a href="https://ibb.co/sj8VQnP"><img src="https://i.ibb.co/SNF30bx/60.png" alt="100" border="0" /></a>
<br />
The easiest way is to just click on any of the incidents. See the right side panel pop out.
<br />
Click  >  View full details > This will show you more detailed information about the incident.
<br />
<a href="https://ibb.co/sj8VQnP"><img src="https://i.ibb.co/SNF30bx/60.png" alt="101" border="0" /></a>
<br />









<br></br>
<h3>Step 11 - Further Analysis and Investigation of the Security Incident </h3>
<br />

you will get a full screen panel

The incident window is divided into three parts:
left, middle, right


Left side -- essential details about the incident
like username, Ipaddress, evidence (events), alerts, bookmarks
Click on events --> a new Logs window will pop out
The above doesn’t work there is a new button called Logs on the Top left > Click
<a href="https://ibb.co/sj8VQnP"><img src="https://i.ibb.co/SNF30bx/60.png" alt="102" border="0" /></a>
<br />
In Logs window --> Results tab --> we can get a lot of information
Run the Query  in the Logs
let TorNodes = (_GetWatchlist('Tor-IP-Addresses')
    | project TorIP = IpAddress);
SigninLogs
// | where IPAddress in (TorNodes)
// | where ResultType != 50126
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
<a href="https://ibb.co/sj8VQnP"><img src="https://i.ibb.co/SNF30bx/60.png" alt="103" border="0" /></a>
<br />
Click on any file/line to see for more information
<a href="https://ibb.co/sj8VQnP"><img src="https://i.ibb.co/SNF30bx/60.png" alt="104" border="0" /></a>
<br />
First thing we need to do is grab this IP address and check if its related to any malicious activity
We can check websites like abuseipdb
insert the ip address into this abuseipdb.com
<br />
<a href="https://ibb.co/sj8VQnP"><img src="https://i.ibb.co/SNF30bx/60.png" alt="105" border="0" /></a>
<br />
We checked the IP Address and its shown to be an abused IP
<br />
<a href="https://ibb.co/sj8VQnP"><img src="https://i.ibb.co/SNF30bx/60.png" alt="106" border="0" /></a>
<br />
Go back to the Logs window --> result

There is no indication in our logs that this log was a successful login. Where does it show it?
This is because we forgot to include the result type column name in our query for project argument
<br />
<a href="https://ibb.co/sj8VQnP"><img src="https://i.ibb.co/SNF30bx/60.png" alt="107" border="0" /></a>
<br />
What do we have to do?
in the KQL just remove the project argument and the column names + uncomment the ResultType
Run the below query: 

let TorNodes = (_GetWatchlist('Tor-IP-Addresses')
    | project TorIP = IpAddress);
SigninLogs
 | where IPAddress in (TorNodes)
 | where ResultType != 50126


Now we can see the ResultType = 0
This means the login was successful
We can make it easier by replacing the fieldvalue of 0 to successful

<br />
<a href="https://ibb.co/sj8VQnP"><img src="https://i.ibb.co/SNF30bx/60.png" alt="108" border="0" /></a>
<br />
The next thing is to figure out what was the login pattern for this user?

Are there any other IP addresses that have been used for the past week or month related to keyser?
If you check the user department or country details we can corroborate if the user is logging in from his country or another country
<br />
<a href="https://ibb.co/sj8VQnP"><img src="https://i.ibb.co/SNF30bx/60.png" alt="109" border="0" /></a>
<br />
<a href="https://ibb.co/sj8VQnP"><img src="https://i.ibb.co/SNF30bx/60.png" alt="110" border="0" /></a>
<br />

Lets make the adjustment to our query one more time to check for all logins in the past
First we will find the UserPrincipalName and use it in our next query
If we right click on the UserPrincipalName we can include it in our query from a little drop down menu
You will see this populate in the KQL


<br />
<a href="https://ibb.co/sj8VQnP"><img src="https://i.ibb.co/SNF30bx/60.png" alt="111" border="0" /></a>
<br />
Next remove all query lines except:
SigninLogs and UserPrincipleName
Specify the time range > Last 3 days  > Run

We will provided with a sign in history for the past 3 days
This will be sorted by TimeGenerated

<br />
<a href="https://ibb.co/sj8VQnP"><img src="https://i.ibb.co/SNF30bx/60.png" alt="112" border="0" /></a>
<br />

We can sort the results by time generated, and if we scroll to the right location will be presented with the location column
Notice the different countries that person logged in
Scroll down to confirm that this user has been logging in from different locations
You can see locations from NL, DE, IT
From our user details in Entra ID > Users we know that keysersozai is based in Turkey is TR

This should be a sign that this user is suspicious and you should be concerned
This is enough information to start a remediation process
<br />
<a href="https://ibb.co/sj8VQnP"><img src="https://i.ibb.co/SNF30bx/60.png" alt="113" border="0" /></a>
<br />

But maybe you want more evidence before you disable the account
Then lets dig deeper and investigate further into this IP address and this user


We can stay on the investigation page then  open a new window with MS

Go to main MS page
Then go to:
Threat management --> Entity Behavior
This gives us the option to search and select the user
We can see all of the user activities in one place
This can help us identify and pattern or anomalies that could be relevant to our investigation

Enrichment widgets:
We can also add and customize widgets that will help us gather information from
Virus Total, Recorded Future, Anomali, AbuseIPDB (DO THIS LATER!!)


<br />
<a href="https://ibb.co/sj8VQnP"><img src="https://i.ibb.co/SNF30bx/60.png" alt="114" border="0" /></a>
<br />

If you look closely, you will see the user keyser also has the most alerts. Well d26369b24 has the most alerts. Who da eff is this?
We also see keyser and d26369b24 .
This is because an analytics rule wrongly linked an entity
This is a perfect example of why need to correctly setup the functionality
Im confused here. 


<br />
<a href="https://ibb.co/sj8VQnP"><img src="https://i.ibb.co/SNF30bx/60.png" alt="115" border="0" /></a>
<br />

In the Entity behavior page go to the search box and input keyser email
A new window will pop up displaying a graph with all relevent alerts, anomalies and activities


<br />
<a href="https://ibb.co/sj8VQnP"><img src="https://i.ibb.co/SNF30bx/60.png" alt="116" border="0" /></a>
<br />
In the Alerts, anomalies and activities timeline will give us every single alert generated for this user providing a nice timeline

Now lets go to the middle graph
Click on AzureActivity label in the middle upper graph
A new Logs window will open up > run
If you see the KQL its super complex,,,nanja kore!!!

Somehow for keyser running the KQL didn’t work for Azure Activity Logs because there isnt any. The user deleted it.
but for d26369b24 it worked.
Hmm…I wonder why?
Ah! We have to click on the other keyser 

<br />
<a href="https://ibb.co/sj8VQnP"><img src="https://i.ibb.co/SNF30bx/60.png" alt="117" border="0" /></a>
<br />

Click on the below keysersoze

<br />
<a href="https://ibb.co/sj8VQnP"><img src="https://i.ibb.co/SNF30bx/60.png" alt="118" border="0" /></a>
<br />

Then we will see a lot of Azure Activity logs
Yay!
Click on it
and run the KQL query


<br />
<a href="https://ibb.co/sj8VQnP"><img src="https://i.ibb.co/SNF30bx/60.png" alt="119" border="0" /></a>
<br />

As you can see there is a lot of Azure Activity logs

The query combines different queries together
That is super complicated to create on my own
The result of this query are sorted from oldest to newest


<br />
<a href="https://ibb.co/sj8VQnP"><img src="https://i.ibb.co/SNF30bx/60.png" alt="120" border="0" /></a>
<br />


If you check the next Azure activity log
ActivityStatus = Suceeded
ActivitySubstatus = OK (status code: 200)
which means it succeeded
Lets check the first log from Azure activity
The OperationName field tells us about an attempt to delete resource diagnostic settings
![image](https://github.com/chun-eric/sentinel-siem-chatgpt/assets/102393871/9a99b380-1b21-44e1-a921-e64f6a03bce5)




<br />
<a href="https://ibb.co/sj8VQnP"><img src="https://i.ibb.co/SNF30bx/60.png" alt="121" border="0" /></a>
<br />

Check out the other Azure activity logs
ResourceProvider column
Resource column


<br />
<a href="https://ibb.co/sj8VQnP"><img src="https://i.ibb.co/SNF30bx/60.png" alt="122" border="0" /></a>
<br />
If you scroll further down you will see many Azure activity logs on Information level
If you scroll right you can see the OperationName column and ActivityStatus
this will give you even more detailed information about the activity that took place and whether it was successful or not


<br />
<a href="https://ibb.co/sj8VQnP"><img src="https://i.ibb.co/SNF30bx/60.png" alt="123" border="0" /></a>
<br />

You can see that SSH key pairs were made, including VMs, etc.. All from a single deployment.  I didn’t create any SSH keys. Maybe Pavel did. 
Scroll down further to see all the different activities
CONFUSED NOT SHOWING UP

At the bottom of our logs we can see more sign ins
Was it successful sign ins?  You should see error code 0
CONFUSED NOT SHOWING UP

<br />
<a href="https://ibb.co/sj8VQnP"><img src="https://i.ibb.co/SNF30bx/60.png" alt="124" border="0" /></a>
<br />


In AppDisplayName there was on item called AzurePortal Console App
This is the cloud shell login that was successful


<br />
<a href="https://ibb.co/sj8VQnP"><img src="https://i.ibb.co/SNF30bx/60.png" alt="125" border="0" /></a>
<br />

Now with all this evidence, what do you do next?
Whats the next action?

We could check the department and role and responsibilities of keyser
Is keyser still working at the company or are his daily responsibilities to sales or development?


The correct next step would be to disable the account and stop the attacker from moving laterally through the web shell access they gained
Next lesson lets take the necessary steps to secure our system
<br />
<br />





<br>
</br>
<h3>Step 12 - Conclusion and Final Scan Check </h3>
<br />
We will need to take quick and effective actions to secure your Azure environment from potential threats

In our Logs Window

The first thing to do is
Disable the compromised account
why? To prevent further damage
How do we do this?

Go to Entra ID (put it in the search bar)
Once inside Entra ID
left side panel
Manage --> Users
Select our user keyser --> click
Bottom left Account status --> Edit --> uncheck Account enabled --> Save
Now the Account status --> Disabled


A quick question. Does disabling the user, disable all the resources this user provisioned as well?

<br />
<a href="https://ibb.co/sj8VQnP"><img src="https://i.ibb.co/SNF30bx/60.png" alt="126" border="0" /></a>
<br />
Delete the Virtual Machine associated with keyser
in search box type virtual machines
select the vm associated with keyser
delete the resources
I would delete all resources associated with this vm
check --> Apply force delete --> Delete
<br />
<a href="https://ibb.co/sj8VQnP"><img src="https://i.ibb.co/SNF30bx/60.png" alt="127" border="0" /></a>
<br />


<br />
<a href="https://ibb.co/sj8VQnP"><img src="https://i.ibb.co/SNF30bx/60.png" alt="128" border="0" /></a>
<br />
Turn on Diagnostic settings for Log Analytics workspace. Why? Helps us log all movements in the resource group
type in Log analytics workspace in search bar
click on the correct resource group instance
On left side bar
Monitoring --> Diagnostic settings --> Add diagnostic setting

<br />
<a href="https://ibb.co/sj8VQnP"><img src="https://i.ibb.co/SNF30bx/60.png" alt="129" border="0" /></a>
<br />
In Diagnostic setting window
check Logs --> both audit and allLogs
check Metrics --> AllMetrics
Add Diagnostic setting name --> Microsoft Sentinel
Destination details --> check Send to Log Analytics workspace
Log Analytics workspace --> choose correct resource group
Click > Save

<br />
<a href="https://ibb.co/sj8VQnP"><img src="https://i.ibb.co/SNF30bx/60.png" alt="130" border="0" /></a>
<br />
Turn on Diagnostic settings for Microsoft Sentinel. Why? We want to track all logs in Microsoft Sentinel.
type in ms in search bar
choose the correct resource group
Left side bar
Configuration --> Settings

In Settings window
settings tab --> Auditing and health monitoring --> click enable
click Configure diagnostic settings

<br />
<a href="https://ibb.co/sj8VQnP"><img src="https://i.ibb.co/SNF30bx/60.png" alt="131" border="0" /></a>
<br />
Diagnostic settings have been enabled

<br />
<a href="https://ibb.co/sj8VQnP"><img src="https://i.ibb.co/SNF30bx/60.png" alt="132" border="0" /></a>
<br />
We have now remediated all the issues, we can close all other windows and move back to our incident investigation window 

One more thing throughout investigations, its essential to add comments to your incidents
All evidence and findings should be collected and present in your incident

How you do that??
The instructor didn’t clarify
Maybe I should ask him
How do you add comments to incidents? How do you collect evidence and findings in your incident. Is there a separate incident worksheet that we can add to?
My answer has been given below


Go back to main MS --> Threat management --> Incidents
Select all the high severity incidents --> click Actions --> Status changed to Close
A new drop down menu will pop down
Classification reason --> true positive
Add to comment section --- this is very important
We need to summarize:
what happened
what we found
what was done
Comment example below


what happened
User performed unusual login from Tor exit node. Based on user login history and performed action, account was most likely breached.

what we found
Log files show user keyser used an Ipaddress that is known for malicious activity originating from multiple locations.
User had removed diagnostics settings from Log Analytics Workspace and Microsoft Sentinel and also created a new VM
User had also changed passwords. 

what was done
User was contacted. Account status is currently disabled.
Diagnostic Settings for Sentinel and Log File workspaces were put back to the previous state and the associated VM deleted from Azure.


We have successful identified and remediated high threat inside the cloud environment

<br />
<a href="https://ibb.co/sj8VQnP"><img src="https://i.ibb.co/SNF30bx/60.png" alt="133" border="0" /></a>
<br />

For the other low incident (siem-training2) we do the same thing. 
Click > Actions 

Fill out details

Severity > Low
Status > Closed
Classification reason --> true positive suspicious activity
Add to comment section --- this is very important

We need to summarize:
what happened
The user account was breached as the User IP address was originating from multiple locations. 

what we found
The user then provisioned a VM in the resource group. 

what was done
The user account is currently disabled and the deployed VM has since been deleted. 

<br />
<a href="https://ibb.co/sj8VQnP"><img src="https://i.ibb.co/SNF30bx/60.png" alt="134" border="0" /></a>
<br />
<br />
<a href="https://ibb.co/sj8VQnP"><img src="https://i.ibb.co/SNF30bx/60.png" alt="135" border="0" /></a>
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
