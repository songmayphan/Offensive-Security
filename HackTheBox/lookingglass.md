# Looking Glass room with Mentor

## Vuln

This machine has the Command Injection vulnerability. We were able to pass cmd into the text box of the website

## Discovery 

passing `whoami` into the arg returns `www`. We know we should have more access.

passing `ls` returns `index.php`, but passing the file in the machine's IP address returns the same page

let's crawl up the directory to see what else we have. passing `cd ../; pwd; ls -alhF`. 

`ls -alhF` gives us more details about the content of the directory. 

found a file called flag.txt, `cat flag.txt` shows us the flag


