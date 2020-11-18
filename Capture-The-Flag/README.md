# GuidePoint Security CTF 2020

[GuidePoint CTF 2020](https://www.youtube.com/redirect?event=video_description&v=yJF0YPd8lDw&q=https%3A%2F%2Fgo.guidepointsecurity.com%2F2020_11_17_Corp_CaptureTheFlag_Digital_01-Registration-LP.html&redir_token=QUFFLUhqbVVlSTdCOGFwVDF1Z0FTVEU4MGswdWZ5OHNJZ3xBQ3Jtc0ttcjdhR0dld3lkUFJuNHhQQ3lwMEFEYkkycVdfbnlfT0N0ZndzWGFBVGttaExKazJjZm56Z3ktanFGSWdwaExZQ0dya0U4MmFHQ1R6M0pUWlY2alEyVDBTbEg5VG1BOS1uZXdRaGdmZURDNHF3RjB5dw%3D%3D)

## Timeline

Launch: Tuesday, November 17th - 8:00 AM EST
Ends: Monday, November 23rd - 5PM EST

I accidentally did the harder one and didn't get the flag, will try again tomorrow!


# Belle 1
> May Phan November 17, 2020 6pm

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

