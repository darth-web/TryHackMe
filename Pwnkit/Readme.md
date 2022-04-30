# ![Pwnkit Logo](Images/logo.png)  Pwnkit: CVE-2021-4034

> Room Link - https://tryhackme.com/room/pwnkit

## Introduction 

CVE-2021-4034, also known as `Pwnkit` is a Local Privilege Escalation (LEP) vulnerability, located in the `Pwnkit` package, that is installed by default in most major Linux distributions. 

## Overview
CVE-2021-4034 [(Pwnkit)](https://www.qualys.com/2022/01/25/cve-2021-4034/pwnkit.txt) was announced in January 2022. Surprisingly, this vulnerability has existed since Pokit (Policy Toolkit) was first released in 2009 and it allows an unprivileged attacker to escalated permissions on any machine with the Polkit package. Despite being a widespread vulnerability due to its tendency to be installed by default. Fortunately, it is not exploitable remotely, making it a Local Privilege Escalation (LPE) vulnerability. 

## What is Polkit?
Polkit is part of the Linux authorization system. When legitimate users attempt to perform actions that require increased levels of authentication, Polkit determines whether the user has adequate permissions. It is integrated with `systemd` and is much more configurable than traditional sudo authentication as it provides increased granularity of assigned permissions to users.
When interacting with Polkit, we can use the `pkexec` utility.

## The Pwnkit Vulnerability Demonstrated
The Pwnkit vulnerability is localized towards the `pkexec` front-end of the Polkit system. Vulnerable versions of `pkexec` don't handle command-line arguments safely which leads to an "out of bounds write" vulnerability that allows attackers to manipulate the environment when pkexec is run. `pkexec` attempts to parse command-line arguments using a for-loop, starting at an index of 1 to offset the named of the program and obtain the first real argument.

```
Example - entering "pkexec bash" since "pkexec" is the name of the program, it would be argument 0 - since it starts at 1, it is ignored since the name is irrelvant to argument parsing
```

If no arguments are present, the index is set permanently to 1. If number of arguments is 0, then "n" is never less than the number of arguments and n stays equal to 1 and the loop is bypassed. 
When `psexec` attempts to write to the argument at index `n`, since there are no command-line arguments, there is no argument at index `n` - instead the program writes to the next thing in memory - the first value in the list of environment variables when the program is called, using a C function called `execve()`. 
Hence, by passing pkexec a null list of arguments, we force it to overwrite an environment variable instead.

```
for(n=1; n < number_of_arguments; n++){
  //Do Stuff
}
```

## Exploitation

This is a relatively straightforward vulnerability to exploit. The C exploit by [`arthepsy`](https://github.com/arthepsy/CVE-2021-4034/blob/main/cve-2021-4034-poc.c) utilizes the `GCONV_PATH` variable to include a malicious shared object that invokes a `/bin/bash` shell with escalated permissions. 

```
tryhackme@pwnkit:~/pwnkit$ gcc cve-2021-4034-poc.c -o exploit
tryhackme@pwnkit:~/pwnkit$ chmod +x exploit
tryhackme@pwnkit:~/pwnkit$ ./exploit 
# whoami
root
# id
uid=0(root) gid=0(root) groups=0(root),1000(tryhackme)
# pwd
# cat flag.txt  
```

## Remediations

The Polkit patch can be installed with an apt upgrade - `sudo apt update && sudo apt upgrade`. In situations where patches have not been released, the recommended hotfix is to remove the SUID bit from the `pkexec` binary. 

This is done with the following command
```
sudo chmod 0755 `which pkexec`
```

