# Linux Fundamentals Part 2

## Description
Further enhance your Linux skills by understanding operators. Get hands-on and remotely access your own Linux machine to put your knowledge into use!

Also available on [YouTube](https://youtu.be/iCHdGW5Oz2E)

## Task 1 - Intro
In this activity, we will exploring various Linux concepts, including Linux Operators and Advanced File Operators.
Unlike the first room, we will need to SSH into the machine as there is no broswer attached machine, unless you are on a paid TryHackMe subscription. This can be circumvented by downloading a free OpenVPN configuration file to enter the environment. Once this has been completed, it is possible to SSH into this box. 

*This task does not require any answers.*

## Task 2 - SSH Intro.
SSH, or Secure Shell is a network communication protocol allowing remote access to a machine. Through SSH it is possible to run commands interactively on the remote machine. A popular SSH client on Windows is PuTTY, while on Mac and Linux environments, it is possible to SSH into a remote machine directly through the Terminal console. 

*This task does not require any answers.*

## Task 3 - Putty and SSH
SSH on Linux and Mac:
The process is as simple as opening up the local Terminal console and SSH'ing in. The syntax to do so is ssh <user>@<host>. In the associated screenshot, we can see that the command used is `ssh shiba2@10.10.233.213`. This is followed by a prompt to enter the password - we enter the passphrase "pinguftw" here.

**SSH on Linux and Mac**

The process is as simple as opening up the local Terminal console and SSH'ing in. The syntax to do so is ssh "user@host". In the associated screenshot, we can see that the command used is `ssh shiba2@10.10.233.213`. This is followed by a prompt to enter the password - we enter the passphrase "pinguftw" here.

![SSH_on_linux](Images/SSH_on_linux.png)

**SSH on Windows**

Putty can be downloaded from the following [link](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)

![putty_install](Images/putty_install.png)

Once downloaded, you will be presented with the following screenshot. We are mainly interested in the Host Name Field. Here we enter the same command we used on the Linux instance - `ssh shiba2@10.10.233.213`

*This task does not require any answers.*

## Task 4 - "&&"
Just as the "&" sign is used to denote "and", the "&&" command is used to execute a second command after the first command has executed successfully. 

*This task does not require any answers.*

## Task 5 - "&"
Traditionally, if you were to run a command that took 10 seconds to run, you wouldn't be able to run other commands during this period. However, including "&", a background operator, allows you run other commands.

*This task does not require any answers.*

## Task 6 - "$"
The "$" command denotes environment variables to affect different processes and how they work. Editing these variables affrect how different processes work. 
Environment variables can be set, following a specific syntax - "export varname=value"

**Question** - How would you set nootnoot equal to 1111?

**Answer** - Following the above syntax to set Environment variables, in this case, the answer would be "export nootnoot=1111"

**Question** - What is the value of the home environment variable?

**Answer** - This can be found out by using the `pwd` command, also known as Print Working Directory. This directory tells you what directory you are currently on. Navigating to the root directory and using the `pwd` command gives us the answer "/home/shiba2"

## Task 7 - "|"
So far we have learned that the >> operator, for example, allows you to store the oputput of a command. The pipe, also denoted by "|" allows you to link multiple commands - you take the output of the first command and use it as input for the second. It is important to note that while the pipe is a very common operator, not all commands support it

*This task does not require any answers.*

## Task 8 - ";"
The ; operator works in a similar manner to &&, except, it does not require the first command to execute successfully.

*This task does not require any answers.*

## Task 9 - ">"
This is the operator used for output redirection. For example, running echo hello > hello.txt, saves the output (in this case, "hello") to a file called hello.txt, instead of outputting the text to the terminal console. 
It is important to realise that using this operator on a file that already exits, it would completely overwrite all the contents of this file and replace it with the ouput of your command. As a result, it is optimal when writing to a new file.

**Question** - How would you output twenty to a file called test

**Answer** - echo twenty > test. This command prints the string "twenty" into a file called test.

## Task 10 - ">>"
The main difference between this operator and ">" is that the ">>" operator appends, or adds the output of a command to an existing file instead of erasing it.

*This task does not require any answers.*

## Task 11 - Binary - shiba2
This task requires you to check if the environment variable "test1234" exists and if it is equal to the current $USER environment variable.

In step 6, we established that the syntax to set environment variables is "export "varname="value". This can be manipulated, resulting in the following command - `export test1234=$USER`

Following this, we run the user binary, resulting in the password, as seen in the attached screenshot.

![shiba2](Images/shiba2.png)

## Task 12 - Advanced File Operations

*This task does not require any answers.*

## Task 13 - Advanced File Operations - A Background

*This task does not require any answers.*

## Task 14 - chown
The Chown command allows us to change the user and group for any file. The syntax for this command is `chown user:group file`

It is important to note that the chown command can only be used if the current user is above, or has greater permissions than the required user - indicating that it is best to perform the chown command as a root (administrator) user

**Question** - How would you change the owner of file to paradox

**Answer** - Following the above mentioned syntax, we can use the following command - "chown paradox file"

**Question** - What about the owner and the group of file to paradox

**Answer** - We use the same syntax, except this time, we include a clause for the group - "chown paradox:paradox file"

**Question** - What flag allows you to operate on every file in the directory at once?

**Answer** - The answer, according to the man page for the `chown` command is "-R". This flag initiates a recursive operation - list operates on files, directories and subdirectories recursively

## Task 15 - chmod
The `chmod` command allows you to set different permissions for a file and control who can read it. 

Chmod syntax - `chmod <permission> <file>`


| Digit | Meaning                                        |
|-------|------------------------------------------------|
| 1     | That file can be executed                      |
| 2     | That file can be written to                    |
| 3     | That file can be executed and written to       |
| 4     | That file can be read                          |
| 5     | That file can be read and executed             |
| 6     | That file can be written to and read           |
| 7     | That file can be read, written to and executed |

File Permissions are divided to three sections - user, group and everyone else.

Permissions are in sequential order - first three characters control permissions for the user, second set of characters control the permissions for the group and the final three characters control permissions for everyone else.

**Question** - What permissions mean the user can read the file, the group can read and write to the file, and no one else can read, write or execute the file?

**Answer** - According to the notation table, the correct answer would be 460
| Digit | Meaning                                                                 |
|-------|-------------------------------------------------------------------------|
| 4     | Users can read                                                          |
| 6     | Group can read and write to the file                                    |
| 0     | It is possible to give someone no permissions by putting 0 as the digit |

**Question** - What permissions mean the user can read, write, and execute the file, the group can read, write, and execute the file, and everyone else can read, write, and execute the file.

**Answer** - The correct answer would be 777 as all parties have access to read, write and execute the file.

## Task 16 - rm
The `rm` command means to remove and has the potential to wreck mass havoc, if used carelessly!

**Question** - What flag deletes every file in a directory

**Answer** - According to the man pages for the `rm` command, the answer would be the `-r` flag

**Question** - How do you suppress all warning prompts

**Answer** - The `-f` flag removes the specified files without prompting for confirmation, regardless of the file's permissions

## Task 17 - mv
The `mv` command allows you to move files from one place to another. It is equivalent to a cut and paste in other Operating Systems.
`mv` syntax - `mv <file> <destination>`

It is possible to move a file to the home directory by `mv file ~`
The `mv` command can also be used to rename files. Syntax - `mv file ~/abcd` renames "file" to "abcd"

**Question** - How would you move file to /tmp

**Answer** - `mv file /tmp`

## Task 18 - Linux Fundamentals 3
*This task does not require any answers.*
