# Metasploitable 
Metasploitable is the easiest way to set up a vulnerable machine as our metasploit target. 

# Installing Metasploitable

Download the installation here: [Source Force Installing Metasploitable](https://sourceforge.net/projects/metasploitable/files/Metasploitable2/)

Setup the machine on your VirtualBox or VMWare. Make sure to configure the network adapter to **Brigde Adpater**. This way our attacker can scan our target.
Make sure our Kali machine has the same setting.

![ss](/img/msf7.PNG)


Run the virtual hard disk file on your Virtual Box or VMWare. You should see this welcome screen: 

![ss](/img/msf4.PNG)

The default username is `msfadmin` and the password is also `msfadmin`

# Firing up our target

Run ```ifconfig``` to get the target's inet address. We'll need this when we launch on exploit. 

See Metasploit.md to run our exploit


