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