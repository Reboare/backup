# My Awesome Book

This file file serves as your book's preface, a great place to describe your book's content and ideas.



```py
def listen():
    s = socket.socket(socket.AF_INET,socket.SOCK_RAW,socket.IPPROTO_ICMP)
    s.setsockopt(socket.SOL_IP, socket.IP_HDRINCL, 1)
    with open('icmpoutput.txt','wb') as catch:   
        while 1:
            data, addr = s.recvfrom(1508)
            print "Packet from %r: %r" % (addr,data)
            if '^BOF' in data:
                continue
            if '^EOF' in data:
                catch.write(data[-1472:-4])
            catch.write(data[-1472:])

listen()
```



