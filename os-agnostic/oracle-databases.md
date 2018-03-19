# Oracle Databases

Most of the examples here will be utilizing [ODAT ](https://github.com/quentinhardy/odat), the Oracle Database Attacking Tool.  This is because it has a number of exploits and scanners built in, is generally easier to set up and more stable than metasploit, and can use a lot of metasploits wordlists and data.  Follow the guide in the ODAT readme to ensure that your Kali has all the required tools.

In general I'd recommend this over trying to get Metasploit up and running.  However, if you are still keen then follow [this guide](https://github.com/rapid7/metasploit-framework/wiki/How-to-get-Oracle-Support-working-with-Kali-Linux).  For an alternative, see [Andy Gill's guide](https://blog.zsec.uk/msforacle/).

## SID Enumeration

The SID for Oracle identifies the database instance running.  We can use ODAT to enumerate these SID's and find instances to attack.

## TNS Poisoning

## CVE-2012-3137

[Alternative Script \(untested\)](https://github.com/r1-/cve-2012-3137)

## Further Reading

[Blackhat USA 2009 - Attacking Oracle with the Metasploit Framework](http://www.blackhat.com/presentations/bh-usa-09/GATES/BHUSA09-Gates-OracleMetasploit-SLIDES.pdf) & [Paper](http://www.blackhat.com/presentations/bh-usa-09/GATES/BHUSA09-Gates-OracleMetasploit-PAPER.pdf)  
[Oracle DB Vulnerabilities: The Missing Pentester Handbook](https://hackmag.com/uncategorized/looking-into-methods-to-penetrate-oracle-db/)

