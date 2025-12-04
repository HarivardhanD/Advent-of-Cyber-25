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




