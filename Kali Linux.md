# Metasploit
Metasploit is a framework used in pen-testing that simplified hacking.

## Vocabulary
- Payload = **exploit module**. They are simple scripts used to interact with the target. A payload can transfer data to the victim's system.
- Exploit = **a hacking activity* *. An exploit is either  **active** : exploit a specific host, run until completion, and then exit *or* **passive**: wait for incoming hosts and exploit them as they connect.

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
msf > help
```
To show the list of available exploits we can use

````
msf > show exploits
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

