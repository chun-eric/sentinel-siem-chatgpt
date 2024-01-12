
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

<h3>Step 1 - Deploying Sentinel to Azure</h3>
To speed development we will use Sentinel All in One.
Sentinel All in One is basically AI
<br/>
It also configures Content Hub solutions
<br/>
If you want to have a look at setting up SIME from beginning to end go to  Pavs other course which we bought
<br/>
If you look at some of the data connectors some are billed and some are free
but don’t worry we wont need to spend anything 
<br/>
When we first deploy Sentinel we get 10gb ingestion per day for the first month
That’s plenty of time to go through all the necessary tasks
Then you will delete Sentinel
<br/>
Then whenever you want to spin up a fresh new Sentinel just Deploy to Azure the Sentinel All in One.
<br/>
<br/>
<br/>
[https://www.microsoft.com/en-us/software-download/windows10
](https://github.com/Azure/Azure-Sentinel/tree/master/Tools/Sentinel-All-In-One#readme
)
<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="1" border="0" /></a>
<br/>


<br></br>
<h3>Step 2 - Deploying Sentinel to Azure</h3>
First download VirtualBox
<br ![image](https://github.com/chun-eric/sentinel-siem-chatgpt/assets/102393871/06b34c7d-7e7f-406c-b6f4-7bb473b7d772)
/>
To speed development we will use Sentinel All in One.
Sentinel All in One is basically AI

It also configures Content Hub solutions

If you want to have a look at setting up SIME from beginning to end go to  Pavs other course which we bought

If you look at some of the data connectors some are billed and some are free
but don’t worry we wont need to spend anything 

When we first deploy Sentinel we get 10gb ingestion per day for the first month
That’s plenty of time to go through all the necessary tasks
Then you will delete Sentinel

Then whenever you want to spin up a fresh new Sentinel just Deploy to Azure the Sentinel All in One.
<br/>
<br/>
After downloading the program, go through the installation process.
<br/>
<br/>
Add your VM name, add folder and ISO image.
<br/>
<br/>
Check --> Unattended Guest OS Install Setup.
<br/>
Complete Username and Password.
<br/>
<br/>

<a href="https://ibb.co/KwrZLwr"><img src="https://i.ibb.co/vYj5QYj/1.png" alt="1" border="0" /></a>
<br/>

<p>Click --> Next --> Choose your hardware, base memory, processors.
</p>
<p>Click --> Next --> Choose your virtual hard disk.</p>
Click --> Next --> Install the computer.
<br/>
<br/>
Voila! The installation is complete.
<br/>
<br/>
Add guest additions because we can change screen resolution for our vm.
<br/>
<br/>
In VirtualBox:
<br/>
Devices > Upgrade Guest Additions
<br/>
<br/>
<a href="https://ibb.co/M75HdHK"><img src="https://i.ibb.co/6FJSQSG/2.png" alt="2" border="0" /></a>
<br />
<br />
Now that everything is installed, we are ready to start the project.
<br/>
<a href="https://ibb.co/J7bN0M4"><img src="https://i.ibb.co/gTXGHBp/3.png" alt="3" border="0" /></a>
<br />
<br />




<h3>Step 3 - Configure Initial Windows VM Setup</h3>
<p>Before we begin scanning with Nessus, we actually need to make it more vulnerable!</p>
<p>Say what?!</p>
<br />
<p>We want to actively seek to remediate these vulnerabilities.</p>
<br />
<p>A few things we must do are:</p>
<p>1) Turn off security updates</p>
<p>2) Turn off the firewall</p>
<br />
<p>This is very important to do as Nessus can accurately identify any vulnerabilities or
weaknesses so we can improve our VM security.
</p>
<br />
Go to:
<p>Start > Settings > Update & Security > Pause Update for 7 days > Advance Options > Change to later date</p>
<a href="https://ibb.co/qDsKNYN"><img src="https://i.ibb.co/rQ6Yx2x/4.png" alt="4" border="0" /></a>
<br />
<br />
<p>Go back to:</p>
<p>Update & Security > View update History > Uninstall updates</p>
<a href="https://ibb.co/r3BVjwT"><img src="https://i.ibb.co/HCcjSH1/5.png" alt="5" border="0" /></a>
<br />
<p>Uninstall updates</p>
<a href="https://ibb.co/RyXkbMS"><img src="https://i.ibb.co/rsBR0Lt/6.png" alt="6" border="0" /></a>
<br/>
<p>You can uninstall updates this way to:</p>
<p>Control panel > Programs > Programs and Features > Installed Updates</p>
<a href="https://ibb.co/6YNvgzp"><img src="https://i.ibb.co/5T2YBmz/7.png" alt="7" border="0" /></a>
<br/>
<p>Delete all Installed updates and restart the system.</p>
<a href="https://ibb.co/kKRRf4H"><img src="https://i.ibb.co/7vww5nY/8.png" alt="8" border="0" /></a>
<br/>
<br/>
Type: wf.msc
<br/>
<p>Next we need to turn off all Windows Firewall settings. This is because Nessus won't be able to reach its destination.</p>
Do the following:
<p>Go to Start > Windows Defender Firewall > Advanced settings > Windows Defender Firewall Properties  > Domain Profile > Firewall state > Off > Apply</p>
<p>Go to Start > Windows Defender Firewall > Advanced settings > Windows Defender Firewall Properties  > Private Profile > Firewall state > Off > Apply</p>
<p>Go to Start > Windows Defender Firewall > Advanced settings > Windows Defender Firewall Properties  > Public Profile > Firewall state > Off > Apply</p>
<p>Restart System.</p>
<a href="https://ibb.co/FJ8YHJ9"><img src="https://i.ibb.co/3WSFRWw/9.png" alt="9" border="0" /></a>
<br/>
<a href="https://ibb.co/0CNm4FG"><img src="https://i.ibb.co/6Hhr91W/10.png" alt="10" border="0" /></a>
<br/>
<a href="https://ibb.co/ryj3SCN"><img src="https://i.ibb.co/cTBwHGV/11.png" alt="11" border="0" /></a>
<br/>
<p>Last step we have to ensure that the VM is on the same network as the Nessus machine that will perform the scan.</p>
<p>In VirtualBox click Devices > Network > Network Settings > Bridged Adapter </p>
<p>Why a bridged adapter?</p>
<p>With bridged networking, the virtual machine has direct access to an external Ethernet network.</p>
<p>The virtual machine must have its own IP address on the external network.</p>
<p>If your host system is on a network and you have a separate IP address for your virtual machine (or can get an IP address from a DHCP server), select this setting. </p>
<p>Other computers on the network can communicate directly with the virtual machine.
</p>
<br/>
<a href="https://imgbb.com/"><img src="https://i.ibb.co/5YQ7p2T/12.png" alt="12" border="0" /></a>
<br/>
<p>To confirm we are on the same network, we can try a simple ping test.</p>
<p>Open a command prompt application or type cmd in the search box and run the command prompt as administrator.</p>
<p>Type ipconfig --> IP address --> 192.168.0.105</p>
<p>This will present you with a configuration of your network adapter, such as IP addresses of our VM, subnet mask, default gateway.</p>
<p>What we are going to do now is ping our VM IP address from our local machine to see that we can connect to our VM from our local machine.</p>
<p>Using Windows Powershell as Administrator:</p>
<p>Type ping 192.168.0.105 (or your VM IP address)</p>
<p>If we get a reply back that means the connection is good.</p>
<br/>
<a href="https://ibb.co/348nntQ"><img src="https://i.ibb.co/gmXxxsh/13.png" alt="13" border="0" /></a>


<br></br>
<h3>Step 4 - Install Tenable Nessus </h3>
<p>Now that our Windows machine is setup, we are ready to download and install Nessus a powerful network vulnerability scanner.</p>
<p>Go to this website link to download the program. www.tenable.com/products/nessus/nessus-essentials</p>
<br/>
Fill out the registration form and enter your email address. Click the get started button.
<br/>
<p>Get your activation code in your email. </p>
<p>Choose the appropriate version and plaftform, whether it is Mac, Linux or Windows. For me, I have chosen Windows 64 bit.</p>
<p>Click > Download and then accept the license agreement.</p>
<br/>
<a href="https://ibb.co/ZTJHTrJ"><img src="https://i.ibb.co/bd3HdV3/14.png" alt="14" border="0" /></a>
<br />
<br />
<p>Accept license agreement.</p>
<p>Once downloaded execute and open executable file. The Nessus file will be downloaded onto your local host.</p>
<p>Confirm OK > Click Next.</p>
<p>Set your default destination.</p>
<p>Once downloaded execute and open executable file. The Nessus file will be downloaded onto your local host.</p>
Confirm OK > Click Next.
<br/>
<p>Set your default destination.</p>
<a href="https://imgbb.com/"><img src="https://i.ibb.co/Kyxq4mj/15.png" alt="15" border="0" /></a>
<br />
<br/>
<p>Click > Next </p>
<a href="https://imgbb.com/"><img src="https://i.ibb.co/mNQP15G/16.png" alt="16" border="0" /></a>
<br/>
<br/>
<p>Click > Next </p>
<a href="https://imgbb.com/"><img src="https://i.ibb.co/kcLFJ9w/17.png" alt="17" border="0" /></a>
<br/>
<br/>
<p>Click > Finish </p>
<a href="https://imgbb.com/"><img src="https://i.ibb.co/0V0HtNd/18.png" alt="18" border="0" /></a>
<br />
<br/>
<p>Shortly after the initial installation, a new page in your private browser will open on localhost or port 8834 where you will finish installation process.</p>
<p>Click > Connect via SSL</p>
<a href="https://imgbb.com/"><img src="https://i.ibb.co/SdCLZ6q/19.png" alt="19" border="0" /></a>
<br />
<br/>
<p>You might be a warning on the browser stating:</p>
<p>Warning: Potential Security Risk Ahead.</p>
<p>Select Advanced > Accept the Risk and Continue.</p>
<a href="https://ibb.co/LhPrL9K"><img src="https://i.ibb.co/zH5FyPd/20.png" alt="20" border="0" /></a>
<br/>
<br/>
<p>A welcome to Nessus screen will appear.</p>
<p>Select > Continue</p>
<a href="https://imgbb.com/"><img src="https://i.ibb.co/k2HrbMQ/21.png" alt="21" border="0" /></a>
<br/>
<br/>
<p>Choose how you want to deploy Nessus. Select an option to get started. Select Register for Nessus Essentials.</p>
<a href="https://imgbb.com/"><img src="https://i.ibb.co/mGFWHDL/22.png" alt="22" border="0" /></a>
<br/>
<br/>
<p>We can skip the registration process since we already have the activation code.</p>
<p>Click > Skip</p>
<a href="https://imgbb.com/"><img src="https://i.ibb.co/R6hMvwJ/23.png" alt="23" border="0" /></a>
<br />
<br />
<p>Paste the activation code you received in your email.</p>
<p>Click > Continue.</p>
<br />
<p>Next we need to create a Nessus administrator user account.Create a username and password.</p>
<p>Click > Submit</p>
<a href="https://imgbb.com/"><img src="https://i.ibb.co/dPPn1qJ/25.png" alt="25" border="0" /></a>
<br />
<br />
<p>We might have to wait for a few minutes.</p>
<a href="https://imgbb.com/"><img src="https://i.ibb.co/6RBHC8R/26.png" alt="26" border="0" /></a>
<br />
<br/>
<p>Once installation is complete we will configure and initialize our first scan.</p>
<a href="https://ibb.co/MSHNh5P"><img src="https://i.ibb.co/PjyCYz1/27.png" alt="27" border="0" /></a>
<br />
<br />


<br></br>
<h3>Step 5 - Configure Nessus for Scanning </h3>
<br/>
Now that its installed its time to perform our first scan
<br />
As soon as you land on the Nessus page you will be greeted by a welcome message
<br/>
<p>It also presents a target box. However let's have a look at all the scanning options.</p>
<p>Click > My Scans > Create a new scan</p>
<a href="https://ibb.co/6B6ftf0"><img src="https://i.ibb.co/L542g2d/28.png" alt="28" border="0" /></a>
<br />
A basic network scan is a good starting point for the overall security posture of your network.
<p>A basic network scan scans your network to identify open ports, running services and potential vulnerabilities on the hosts within the network.</p>
<p>We also have the ability to perform a malware scan.
</p>
<br/>
<p>A malware scan is designed to help you identify malware that may be present on your system.
</p>
<p>It scans for no malware signature as well as potentially unwanted programs and other suspicious activity.
</p>
<p>Running malware scan on a regular basis can help you detect and remove malware from your network.
</p>
<p>
Wannacry ransomware scan is specifically designed to help you detect potential indicators of WannaCry ransomware on our network.
Wannacray first appeared in May 2017 and spreads through a vulnerability in Microsoft Windows Systems.
</p>
<p>It encrypts users files and demands payment in exchange for the encryption key.
Running this scan can help you take action to prevent its spread and protect your network from this type of attack.</p>
<p>
Shell scans can help you detect potential indicators of the log4 Shell vulnerability on your network.
Log4 Shell was discovered in December 2021 and it affected the widely used Apache log4j logging library used by many applications and services.
Attackers could use this vulnerability to execute arbitrary code remotely which could lead to complete compromise of affected systems.
Running this scan can help you take action to protect your system from this critical vulnerability.
</p>
<a href="https://ibb.co/xLP1btR"><img src="https://i.ibb.co/RbZ3KkV/29.png" alt="29" border="0" /></a>
<br />
<br />
Okay let's start with a basic network scan.
<br/>
<p>Click > Basic Network Scan</p>
<br />
<a href="https://ibb.co/wCnPLQ8"><img src="https://i.ibb.co/ft3ySNZ/30.png" alt="30" border="0" /></a>
<br />
<br />
In the Settings tab we need to fill out two required fields: Name & Targets.
<br/>
Name = the name of your scan.
<br />
Targets = IP Address or subnet range you want to target (this is our VM IP address) 192.168.0.105
<br/>
Get creative with your scan name.
<br/>
Let's name our Name: Windows Basic Scan.
<br/>
Targets: VM IP Address. We can get that from Command Prompt by typing ipconfig.
<br/>
<a href="https://ibb.co/3SbhHd2"><img src="https://i.ibb.co/F817t32/31.png" alt="31" border="0" /></a>
<br />
<br />
In the Settings Tab under BASIC > Schedule > We can schedule how often we get this scan.
<br/>
In the Settings Tab under BASIC > Notification > If you want to be notified that scan is done.
<br />
<a href="https://ibb.co/KmGZC4T"><img src="https://i.ibb.co/4JfyBvh/32.png" alt="32" border="0" /></a>
<br />
<br />
<p>
In Settings Tab under DISCOVER, we can change Port scan types to common ports, all ports or Custom.
For now lets stick with the common ports.</p>
<p>Port scan common ports use TCP, ARP, ICMP and uses SYN scanner if necessary.
The most common ports are FTP, SSH, Telnet, SMTP.</p>
<p>For now lets choose the Port Scan common.</p>
<br/>
<br />
<p>
In Settings Tab under ASSESSMENT allows you to configure how the scanner performs the scan and what types of vulnerabilities it looks for.
There are two categories: General and Web Applications.
</p>
<br/>
<br/>
<p>Under general settings avoiding potential false alarms reduces the number alarms however it does miss legitimate vulnerabilities.
In general settings you can also disable common gateway interface CGI scripts which could also miss legit vulnerabilities.</p>
<p>CGI scripts are like little programs that help your web browser communicate with a web server.
CGI scripts often used to process data like forms you fill out online, process search query.</p>
<br/>
<p>Under Web applications you can also disable web application scanning altogether
Web applications can be exploited and targeted.</p>
<p>For now lets keep our Assessment setting to Default.
</p>
<br/>
<a href="https://ibb.co/gyFW8kB"><img src="https://i.ibb.co/NxnNqcH/33.png" alt="33" border="0" /></a>
<br />
<br />
<p>
In Settings tab under REPORT, we can control how the scan results are processed and presented to user.
We can adjust the level of detail to our specific needs and requirements.
Lets just keep this as it is, for now.
</p>
<br/>
<a href="https://ibb.co/3fqhyLV"><img src="https://i.ibb.co/ZHPT6kv/34.png" alt="34" border="0" /></a>
<br />
<br />
<p>Lastly in Settings tab under ADVANCED in the performance options we can control the number of hosts that will 
be scanned at the same time and the network read timeout.</p>
<p>Lets leave it as it is.</p>
<br/>
<br />
<a href="https://ibb.co/CK94qHg"><img src="https://i.ibb.co/SwmH23b/35.png" alt="35" border="0" /></a>
<br />
<br />
<p>
Under the Credentials tab we can provide credentials and perform a scan with those credentials.
Performing a credential scan with Nessus can provide several advantages over non credential scan.
</p>
<p>This can be information about installed software patch levels and system configurations that might be critical for detecting vulnerabilities and risk
We will explore the difference scans with and without credentials later on.</p>
<br/>
<br />
<a href="https://ibb.co/QdM9WVX"><img src="https://i.ibb.co/PQgx2yt/36.png" alt="36" border="0" /></a>
<br />
<br />
<p>In the Plugins tab we have a lot of plugins or tools we can add different plugins for different types of vulnerabilities and risk.
There is a huge library of plugins.
</p>
<p>Plugins are organized by families based on their function or type of vulnerability they target.
</p>
<br/>
<br />
<p>We want to focus on detecting web application vulnerabilities, we can use the web application family of plugins.
We want to focus on database vulnerabilties, we can use the database family of plugins.</p>
<br/>
<a href="https://ibb.co/KWwrH1D"><img src="https://i.ibb.co/njCLSWB/37.png" alt="37" border="0" /></a>
<br />
<br />
<p>
Now that we have a basic understanding, we can go back and save our scan.
We have saved our first basic scan. Woohoo!
All we have to do is click the Launch play button
</p>
<br/>
<br />
<a href="https://ibb.co/bHKJSxN"><img src="https://i.ibb.co/LQZ5LM9/38.png" alt="38" border="0" /></a>
<br />
<br />


<br></br>
<h3>Step 6 - Our First Nessus Initial Scan (without Credentials) </h3>
<br/>
<p>Okay you can press the launch button.</p>
<p>Then click on the name of the scan you gave previously. For us its the Windows basic scan.</p>
<p>Then you will be redirected to another window.</p>
<p>You will see a Hosts tab with the scan running.</p>
<p>The scan details will be shown on the right-hand side.</p>
<p>As Vulnerabilities details will be shown below.</p>
<br />
<p>There will be 5 categories under vulnerabilities.</p>
Critical
<br/>
High
<br/>
Medium
<br/>
Low
<br/>
Info
<br/>
<p>Click on the bar in the middle of the screen to access more detailed information.</p>
<p>There you will see the names of the vulnerabilities.</p>
<a href="https://ibb.co/0tjHYHm"><img src="https://i.ibb.co/YZ7HfHP/39.png" alt="39" border="0" /></a>
<br />
<br />
<p>If you want to take it further you can keep clicking on each vulnerability.</p>
<p>Lets take a closer look at one of them:</p>
<p>Windows NetBIOS / SMB Remote Host Information Disclosure.</p>
<br/>
<p>Here you will receive very detailed information about the vulnerability.</p>
<p>Description</p>
<p>Output</p>
<br />
<p>Most cybercriminals often exploit vulnerabilities on Port 445 to spread wannacry.</p>
<p>Wait for the scan to completely finish.</p>
<br/>
<a href="https://ibb.co/FqXKSX4"><img src="https://i.ibb.co/dJtrdt4/40.png" alt="40" border="0" /></a>
<br />
<br />
<p>As you can see there is medium SMB Signing not required.</p>
<p>Ours can be different as we are in 2023.</p>
<p>Read the Description, Solution, See Also and Output.</p>
<p>Again, its up to you remediate the steps.</p>
<p>You can also view your previous scans in the History tab.</p>
<p>We haven't found enough vulnerabilities, so let's change some settings to increase the vulnerabilities.</p>
<br />
<a href="https://ibb.co/hHPqB1y"><img src="https://i.ibb.co/Y8zrcT3/41.png" alt="41" border="0" /></a>
<br />
<br />



<br></br>
<h3>Step 7 - Configuration of Credential Scan </h3>
<br/>
<p>We are going to add credentials to our saved window scan.</p>
<p>First navigate to My Scans.</p>
<p>Select your previous saved scan.</p>
<p>Okay now in the top right corner there is configure button. Click it.</p>
<br />
<a href="https://ibb.co/cNt6xwk"><img src="https://i.ibb.co/Jz2vCqm/42.png" alt="42" border="0" /></a>
<br />
<br />
<p>Then go to the Credentials Tab.</p>
<p>Select Windows as our VM is Windows based.</p>
<p>A new box to the right will appear titled Windows.</p>
<p>There are several authentication method we can choose from.</p>
<p>The password method requires the admin or user password to be provided for authentication.</p>
<p>Kerberos is used when the target system is joined to a domain.</p>
<p>For now lets pick the Password option.</p>
<p>Fill in your Username, Password, Domain.</p>
<br/>
<br/>
<p>If you cant remember your Username, Password, Domain etc. We don’t have a domain name.</p>
<p>Go to Command Prompt and type:</p>
<p>whoami or echo %username%</p>
<p>Once you have filled out the credentials click save.</p>
<br />
<a href="https://ibb.co/rpLjN5B"><img src="https://i.ibb.co/5jz0wsQ/43.png" alt="43" border="0" /></a>
<br />
<br />
<p>However you will receive a notification that the account user doesn’t have the necessary privileges if you try to run the scan.</p>
<p>This is because the non-default administrator account isnt automatically added to the local group administrators and doesn’t have the appropriate rights to access all the system files and settings.</p>
<p>To address this issue we need to perform a few additional steps.</p>
<p>Its not complicated but it takes a bit of time. =(</p>
<br/>
<p>Go back to our Windows VM.</p>
<p>Click Windows > search for "services".</p>
<br/>
<p>Find remote registry and double click.</p>
<br />
<a href="https://ibb.co/3Fb2zrv"><img src="https://i.ibb.co/Lgym5kr/44.png" alt="44" border="0" /></a>
<br />
<br/>
<p>In General Tab go to Startup type:  and then change to Automatic > Apply > OK</p>
<p>Make sure you click Start in Service Status to enable > Apply </p>
<p>This will allow you to remotely connect to the system registry database and perform different operations such as viewing registry keys and values.</p>
<p></p>
<br />
<a href="https://imgbb.com/"><img src="https://i.ibb.co/Bccnbyz/45.png" alt="45" border="0" /></a>
<br />
<br />
<p>Search bar  'change User Account Control settings' and select.</p>
<p>Under Choose when to be notified about changes to your computer >  slide down to Never notify > Click OK.</p>
<p>This will prevent user account control prompts from interrupting our scan.</p>
<br />
<a href="https://ibb.co/rZFh8b6"><img src="https://i.ibb.co/wKBqGJW/46.png" alt="46" border="0" /></a>
<br />
<br />
<p>Type 'Registry Editor' in search bar.</p>
<p>We need to create a new Dword Value in the Registry Editor.</p>
<br/>
<p>Click on HKEY_LOCAL_MACHINE > SOFTWARE > MICROSOFT > WINDOWS > CurrentVersion > Policies > System</p>
<br />
<a href="https://ibb.co/vzxx0NL"><img src="https://i.ibb.co/LpzzVT9/47.png" alt="47" border="0" /></a>
<br />
<br />
<p>In the System folder with your mouse hovering on the right screen, right click , select New > DWORD 32 bit Value.</p>
<p>This will create a new registry value that you can name exactly as below.</p>
<p>LocalAccountTokenFilterPolicy</p>
<br />
<a href="https://ibb.co/z4W5nRb"><img src="https://i.ibb.co/S3Dxsn0/48.png" alt="48" border="0" /></a>
<br />
<a href="https://ibb.co/CP08jBT"><img src="https://i.ibb.co/dmKMNBz/49.png" alt="49" border="0" /></a>
<br />
<br />
<p>Double click on this newly created file.</p>
<p>Change Value data to:</p>
<p>1 Hexadecimal</p>
<p>This will enable local account token filter policy which allows non-admin accounts to access admin resources on the system when using remote procedure call.</p>
<br />
<p>Restart computer to ensure changes take effect.</p>
<br/>
<p>Now Nessus will be able to use credential scanning to perform more comprehensive security scans of your system.</p>
<p>Now we will be able to find more vulnerabilities.</p>
<br/>
<a href="https://ibb.co/xsLQ5f7"><img src="https://i.ibb.co/JdtPkys/50.png" alt="50" border="0" /></a>
<br />
<br />



<br></br>
<h3>Step 8 - Nessus Credential Scan </h3>
<br/>
<p>Click on the launch button again on the same scanner.</p>
<br />
<p>After you wait for the scan to finish check your findings.</p>
<p>As you can see there is a lot more vulnerabilities.</p>
<br/>
<p>Look at the high vulnerabilities.</p>
<p>Click on each one to see more detailed information about the vulnerability.</p>
<br />
<a href="https://ibb.co/HqhVRzJ"><img src="https://i.ibb.co/rHdmq6W/51.png" alt="51" border="0" /></a>
<br />
<a href="https://ibb.co/7jT3K1R"><img src="https://i.ibb.co/n6VXjLn/52.png" alt="52" border="0" /></a>
<br />
<br />
<p>In the video, teacher clicked on Security Updates for Microsoft .NET Framework December 2022 vulnerability.</p>
<p>There are 2 vulnerabilities as shown by the CVE number.</p>
<br />
<p>If you want to know more about a particular CV.</p>
<p>copy and paste CVE number into the browser.</p>
<br />
<p>Okay, great, let's start to install outdated software. </p>
<br/>
<a href="https://ibb.co/LR3MgW2"><img src="https://i.ibb.co/fFsbN5P/53.png" alt="53" border="0" /></a>
<br />
<br />


<br></br>
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
