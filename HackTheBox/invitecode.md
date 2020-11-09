# Invite code

when I first joined hackthebox, I was greeted with the log in page, where it said "Hack this page to get an invite code" 
Here's how I did that

# Begin

The hint said to look at the console, so I right clicked, inspect element
I saw a link that said something related to invite code, so I went to that

`https://www.hackthebox.eu/js/inviteapi.min.js`
 
Saw this

`//This javascript code looks strange...is it obfuscated???
eval(function(p,a,c,k,e,r){e=function(c){return c.toString(a)};if(!''.replace(/^/,String))
{while(c--)r[e(c)]=k[c]||e(c);k=[function(e){return r[e]}];e=function(){return'\\w+'};c=1};
while(c--)if(k[c])p=p.replace(new RegExp('\\b'+e(c)+'\\b','g'),k[c]);return p}('0 3(){$.4({5:"6",7:"8",9:\'/b/c/d/e/f\',g:0(a){1.2(a)},h:0(a){1.2(a)}})}',18,18,
'function|console|log|**makeInviteCode**|ajax|type|POST|dataType|json|url||api|invite|how|to|generate|success|error'.split('|'),0,{}))`

Aha! another invite code related item, and it's a function. So I went back to the console and call the function
`makeInviteCode()`

The function contains:

`data{some random string}
 type{BASE64}`

There is a hint..

`Data is encrypted … We should probably check the encryption type in order to decrypt it`

# Decrypt

I search BASIC64 decoder on google and pasted in the code,
the code said to send a POST request to /api/invite/generate

# CMD
I openedmy console and typed in 
`curl -XPOST https://www.hackthebox.eu/api/invite/generate`

AND SUCCESS!!
it gave me this: 

`{"success":1,"data":{"code":"TkFMTlAtUkVSSFAtTkhYUlMtQkRHTEstRkRWQ0Y=","format":"encoded"},"0":200}`

The code is still in encoded format, so I have to decrypt it once more with BASE64 decoder

And that's it I got my code!





#
