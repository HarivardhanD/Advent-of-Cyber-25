==============================================================================================================================================================================================================================================================================================================
# Advent of Cyber Prep Track
==============================================================================================================================================================================================================================================================================================================


# Task 1, explains the importance of strong password and ask us to write a strong password including caps , numbers , symbols to create a strong password


- ls => list files
- cat filename.txt => cat stands for concatenation and it is used to read files
- cd dir_name => cd stands for change directory and is used to move from one dir to other
- to find the hidden file use ` ls -la ` 


==============================================================================================================================================================================================================================================================================================================
#                       Linux CLI - Shells Bells

==============================================================================================================================================================================================================================================================================================================

- ` echo ` is used to echo / print back the text on the terminal :
    - ex : ` echo hello` will print `hello` back to terminal

- `pwd` gives u the current directory you are on !

-  `grep` a command to look for a specific text inside a file
    - ex: `grep "Failed password" auth.log` this greps/gets us failed password from auth.log file

- `find`  is command that searches for files with specific parameters, such as -name 
    - ex: `find /home/socmas -name *egg*` to search for "eggs" in the socmas home directory.

- Pipe symbol `|`	Send the output from the first command to the second	
    - ex: `cat unordered-list.txt | sort | uniq` so what happens here is , first unordered list file is opened and then it is sorted and then the sorted one is sent to uniq command to find the unique elements


- Output redirect `>/>>` => Use > to overwrite a file, and >> to append to the end
    - ex: `some-long-command > /home/mcskidy/output.txt` send some-long-cmd to output.txt file


- Double ampersand `&&`	=> Run the second command if the first was successful	
    - ex: `grep "secret" message.txt && echo "Secret found!"` so here what happens is , first it greps secret from msg.txt and only if it was successfull , is the echo executed


-  `uptime` command is used  to see how much time your system is running

- `ip addr` or ` ip a s ` to check your IP address

- ` ps aux ` to list all processes. 

-  You can switch the user to root with `sudo su`and return back with ` exit` command

- `whoami` check ur current user level

- `history` command, to get list of all commands u had executed up to now




======================================================================================================================================================
======================================================================================================================================================
#                                           PHISHING 
============================================================================================================================================================================================================================================================================================================

- Social engineering refers to manipulating a user to make a mistake. Examples of such mistakes include sharing a password, opening a malicious file, and approving a payment.

- It has been very difficult to spot this nowadays and we also have a subset in social engineering called "PHISHING", Where we send messages via email, telegram or any social media and when target opens it or click it or send reply to the message his/her system is compromised .

- We also have social engineering through voice calls (VISHING) and through QRcode (QUISHING) .

- Now how can we identify a social engineering message or calls , so for this we have a neumonic as ` S T O P ` Which means:
    - Suspicious?
    - Telling me to click something?
    - Offering me an amazing deal?
    - Pushing me to do something now?

- Another neumonic for the same , ie STOP is :
    - Slow down. Scammers run on your adrenaline.
    - Type the address yourself. Don’t use the message’s link.
    - Open nothing unexpected. Verify first.
    - Prove the sender. Check the real From address/number, not just the display name.

- so for social engieering , lets take a scenario, u want a username and pass of an employee, now for this first u need to prepare a email, which looks legit and it can be done using the follwoing [https://github.com/trustedsec/social-engineer-toolkit] , here u will be given options to crate a email or do phishing/ this tool has all kind of social engierring toolkit , in the email , u have to paste a link to fake login page, created by u , whch gets u the username and password , as soon as u type it in the fake login page,u shoul have the credentials



==============================================================================================================================================================================================================================================================================================================
#                                                   Splunk Basics - Did you SIEM?
==============================================================================================================================================================================================================================================================================================================

- In this room , we will learn basic of slunk and how to use it and why.
- Splunk is , used to anakyze logs and it has variet tools for the same.Collecting, storing, and analysing machine data. It provides various tools for analysing data, including search, correlation, and visualisation.

- First i opened the splunk and went to -> `Search and Reporting ` section .

- Then in search bar  i searched ` index=main ` ,this is basic  , this tells splunk to look for logs in main, its like telling splunk, dont search evry folder, rather search only where required and after this set time from 24 hours to `all time`

- Then in selected field we will have `source type` , where we will have types of logs which are captured and in our case it shows :
    - web_traffic: This data source contains events related to web connections to and from the web server.
    - firewall_logs: This data source contains the firewall logs, showing the traffic allowed or blocked

- Now use this query in search field ` index=main sourcetype=web_traffic ` , we get logs from only web traffic

- Now we get few information and most imp one to take a eye on is :
    - client IP => The IP of user who made req to visit or view our site
    - path => The page or file the user tried to access.
    - status => Whether the request succeeded or failed.
    - user_agent => What kind of browser/device/app the user is using.

- Next query ` index=main sourcetype=web_traffic | timechart span=1d count ` 
    - Read it as:
    - Search for events in index=main with sourcetype=web_traffic
    - AND THEN
    - Make a timechart that counts how many events happened each day.

- Query : `index=main sourcetype=web_traffic | timechart span=1d count | sort by count | reverse`
    - reverse will show the count in descendiing order

- clicking on `user_agent` will give us all user and the browser they used to access the site

- `client_ip` contains the IP addresses of the clients accessing the web server.

- Now we know that , the hackers wont use nornal browser , so lets find user using a different browser :
    - `index=main sourcetype=web_traffic user_agent!=*Mozilla* user_agent!=*Chrome* user_agent!=*Safari* user_agent!=*Firefox*`

- Now lets narrow down suspicious ip and query is as ` sourcetype=web_traffic user_agent!=*Mozilla* user_agent!=*Chrome* user_agent!=*Safari* user_agent!=*Firefox* | stats count by client_ip | sort -count | head 5 ` which means:
    - searhc in main and ignore normal browser.
    - ` stats count by client_ip` this mean, give ip addresses of client with more no. of request
    - `  sort -count `  and then after getting the count, sort it wiht higest request first.
    - ` head 5` means , give only top5 ip with more number of request

 - Now use this query, for reconisassiacne ` sourcetype=web_traffic client_ip="<REDACTED>" AND path IN ("/.env", "/*phpinfo*", "/.git*") | table _time, path, user_agent, status ` which means:
    - First, it filter out specific ip the request come from
    - `path IN ("/.env", "/*phpinfo*", "/.git*")`Show me if this IP is trying to access files that hackers commonly look for.
    - next, gives us the respoonse in format.

- `sourcetype=web_traffic client_ip="<REDACTED>" AND path="*..*" OR path="*redirect*"` this checks if user has used ` .. ` or ` redirect` path

- ` sourcetype=web_traffic client_ip="<REDACTED>" AND user_agent IN ("*sqlmap*", "*Havij*") | table _time, path, status` => this is like , check if user_agent is `Havij` and `sqlmap`


- ` sourcetype=web_traffic client_ip="<REDACTED>" AND path IN ("*backup.zip*", "*logs.tar.gz*") | table _time path, user_agent` check if url path has backup.zip or logs.tar.gz ? hackers searhc these files in url section 

- `sourcetype=web_traffic client_ip="<REDACTED>" AND path IN ("*bunnylock.bin*", "*shell.php?cmd=*") | table _time, path, user_agent, status` => Show me all requests from this IP that tried to access dangerous files (bunnylock.bin or shell.php?cmd=*), and show when it happened, what they tried, which tool they used, and what the server did.


- query => `  sourcetype=firewall_logs src_ip="10.10.1.5" AND dest_ip="<REDACTED>" AND action="ALLOWED" | table _time, action, protocol, src_ip, dest_ip, dest_port, reason `


- query => `sourcetype=firewall_logs src_ip="10.10.1.5" AND dest_ip="<REDACTED>" AND action="ALLOWED" | stats sum(bytes_transferred) by src_ip ` => this shows how much bytes of data is transferred by src_ip , ie our device ip to the target or attacker 


==============================================================================================================================================================================================================================================================================================================
#                                               AI IN SECURITY
==============================================================================================================================================================================================================================================================================================================

- Yes this room explains and tells the use of AI in cyber security to automate the task.

# Defensve security

- in the defense field , AI can be used to analyze logs and summarize them .
- Also, it can be used to detect latest malware and notify.

# OFFENISVIE SECURITY

- There are some task, like using tools or brute forcing which involves repeatedly using the same the tool or repeating the task again and again , so instead of doing this ourself we can ask AI to do such task , reducing manual labour


# CONCLUSION

- These benefits doesnt mean , AI is perfect , we still need humans for resolving few things as AI can have only trained knowledge .

-  We cannot assume the output from AI is 100% correct. Efforts must be made to verify the information it provides. Additionally, managing challenges such as keeping data private, securing AI models, and informing users properly requires careful consideration.





==============================================================================================================================================================================================================================================================================================================
#                                         IDOR - Santa’s Little IDOR
==============================================================================================================================================================================================================================================================================================================



- IDOR stands for Insecure Direct Object Reference and is a type of access control vulnerability

- Lets understand the basic IDOR with the example given below:
    - lets consider : `https://awesome.website.thm/TrackPackage?packageID=1001`
    - now , as u can see , in the above URL we have packageID=1001 , so pkdid is given in url.
    - Now , when we do , packageID=1002 , whaat might happen is , u might get access to objectID 1002.
    - This itself, ie getting access to other objectID , which doesnt belong to you is called IDOR

- So ,when user re writes the pkgid as 1002, the web application must ask, if the user is authentic and authorized and only then it must give access to object 1002, but in case of IDOR,the web doesnt check the same and directly gives access to 1002.


-  The real issue is that the system doesn’t check whether the person making the request is allowed to access it.

- A lot of people try to “fix” IDORs by hiding or encoding IDs. For example, changing /user/1 to /user/ea21f09b2. That might make it look harder to guess, but if the server still isn’t checking permissions, it’s just as insecure. The vulnerability isn’t about how the object is referenced, it’s about missing authorization checks.

- To understand the root cause of IDOR, it is important to understand the basic principles of authentication and authorization:
    - Authentication: The process by which you verify who you are. For example, supplying your username and password.

    - Authorization: The process by which the web application verifies your permissions. For example, are you allowed to visit the admin page of a web application, or are you allowed to make a payment using a specific account?


- Authorization cannot happen before authentication. ie: If the application doesn't know who you are, it cannot verify what permissions your user has


- Now lets see what are types of privilage escalation and under what type does IDOR come to :

    - Vertical privilege escalation: This refers to privilege escalation where you gain access to more features. For example, you may be a normal user on the application, but can perform actions that should be restricted for an administrator.

    - Horizontal privilege escalation: This refers to privilege escalation where you use a feature you are authorized to use, but gain access to data that you are not allowed to access. For example, you should only be able to see your accounts, not someone else's accounts.


- I hands-on did it as follows : 
    - Logged in as legit user and then went to inspect and network tab
    - there , in the GET section, i saw that userid=10 , was beign mentioned
    - so i went went to storage tab and then , clicked local storage and then in the https://..../userid=10 , i changed the user id from 10 to 11, refreshed the page and booyah i could get access to the other objec without any authentication , hence IDOR was detected.


- Don't rely on tricks like Base64 or hashing the IDs; those can still be guessed or decoded. Instead, keep all the real permission checks on the server. Whenever a request comes in, check: "Does this user own or have permission to view this item?"


==============================================================================================================================================================================================================================================================================================================
#                                   Malware Analysis - Egg-xecutable     
==============================================================================================================================================================================================================================================================================================================


- Malware analysis is the process of examining a malicious file to understand its functionality, operation, and methods for defence against it.
    - ex: For example, could the malicious file communicate with an attacker's server? We can block that server.

- So , malware analysis is always done in a controlled environment, and there are 2 types of analysis done :
    - STATIC ANALYSIS : The process of analyzing malware without executing it, but in a controlled environment.

    - DYNAMIC ANALYSIS : Executing the malware and analysing it , in a controlled environment.

- All the testing for malware, shall be always done in sandboxes, because directly testing it on our system can have potential harm for our files , but testing in sandboxes prevents it from affecting our system.

- The use of sandboxes is part of the golden rule in malware analysis: never run dangerous applications on devices you care about.



# Interactive: Static Analysis

- As we alluded to previously in this room, we use static analysis to gather information about a sample without executing it and digging deep.  

- while doing the static analysis, we should take a note for all of these :

    - checksum -> Every malware has checksum or it is like identity to itself.

    - Strings ->  for example, IP addresses, URLs, commands, or even passwords!

    - Imports ->  rather than building everything from scratch, applications will use operating system functions and libraries to interact with the OS.

    - Resources -> "Resources" contain data such as the icon that is displayed to the user. Malware can hide itself in the form of pdf or word file , so the user cant recognize it


- This can be done with tool called : ` peStudio `

- Go to -> PeStudio -> File -> new -> malware.exe



# Interactive: Dynamic Analysis


-  Dynamic analysis involves executing the malicious sample to identify its behaviours and how it interacts with the operating system.

- For this , we can use tool ` RegSHot` and its like , i take snapshot of registry before runniing the malware and then, take a snapshot with malware running and after than we compare both snapshots and check , which registry have been infected

- Then, i compare bothb the snapshots and look where the malware.exe is being executed in the registry.

- Another tool that can be used is ProcMon, where i first monitor the system without executing the malware and the i execute the malware and take note/capture the logs

- Then we can filter based on protocol / malware.exe to see where it is being executed and it also captures each and every corner of system , so it also captures if the malware is hiding