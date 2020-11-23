# Sanitize retired room

this room has SQL Injection Vulnerability. passing ` OR 1=1; --` returns the flag

` OR 1=1` will always returns true, so the SQL code is always executed. 
`--` is to comment out the rest of the command 

