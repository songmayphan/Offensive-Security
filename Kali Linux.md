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
