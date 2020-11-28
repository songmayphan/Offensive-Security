# Cross-Site Scripting

Cross-Site Scripting (XSS) attacks are a type of injection, in which malicious scripts are injected into otherwise benign and trusted websites. XSS attacks occur when an attacker uses a web application to send malicious code, generally in the form of a browser side script, to a different end user. Flaws that allow these attacks to succeed are quite widespread and occur anywhere a web application uses input from a user within the output it generates without validating or encoding it.

## How to preven XSS attacks

[OWASP Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html)

### Rule 1: Never Insert Untrusted Data Except in Allowed Locations

Directly in a script:
`<script>...NEVER PUT UNTRUSTED DATA HERE...</script>`

Inside an HTML comment:
`<!--...NEVER PUT UNTRUSTED DATA HERE...-->`

In an attribute name:
`<div ...NEVER PUT UNTRUSTED DATA HERE...=test />`

In a tag name:
`<NEVER PUT UNTRUSTED DATA HERE... href="/test" />`

Directly in CSS:
`<style>
...NEVER PUT UNTRUSTED DATA HERE...
</style>
`
>never accept actual JavaScript code from an untrusted source and then run it

### Rule 2: HTML Encode Before Inserting Untrusted Data into HTML Element Content

```<body>
...ENCODE UNTRUSTED DATA BEFORE PUTTING HERE...
</body>
<div>
...ENCODE UNTRUSTED DATA BEFORE PUTTING HERE...
</div>
```

### Rule 3: Attribute Encode Before Inserting Untrusted Data into HTML Common Attributes

Inside UNquoted attribute:
`<div attr=...ENCODE UNTRUSTED DATA BEFORE PUTTING HERE...>content`
  
Inside single quoted attribute:
`<div attr='...ENCODE UNTRUSTED DATA BEFORE PUTTING HERE...'>content`

Inside double quoted attribute :
`<div attr="...ENCODE UNTRUSTED DATA BEFORE PUTTING HERE...">content`

### Rule 4: JavaScript Encode Before Inserting Untrusted Data into JavaScript Data Values

Inside a quoted string:
`<script>alert('...ENCODE UNTRUSTED DATA BEFORE PUTTING HERE...')</script>`

One side of a quoted expression:
`<script>x='...ENCODE UNTRUSTED DATA BEFORE PUTTING HERE...'</script>`

Inside quoted event handler:
`<div onmouseover="x='...ENCODE UNTRUSTED DATA BEFORE PUTTING HERE...'"</div>`
