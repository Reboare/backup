# Mysql Databases

MySQL is by far the most common database system you'll find on systems, maybe excluding Sqlite.  Consequently, it's important to understand how to exfiltrate data from them and exploit them.

They're quite simple so there aren't many tools out there to assist with exploitation, and consequently this chapter will be quite short.

## Connecting to the Database

You can connect to a MySQL database via the following command:

```
mysql -u username -ppassword -h ip
```

With the above we'll be returned a prompt, but we can also send a chain of commands directly via the command line tool:

```
mysql -u username -ppassword -e "use users; select * from accounts"
```

It may seem superfluous to mention this, but many times when operating a reverse shell on a Windows machine, running mysql.exe will not return you a prompt over the connection.  It's incredibly annoying to have the entire session hang when running mysql for the first time.  Consequently, the above is invaluable.

## Database Dump

Again if faced with a situation in which you can't spawn a mysql shell you can use mysqldump to dump the database.

## User Defined Functions

If the mysql database is running as an elevated user, such as root or NT/SYSTEM, then it becomes trivial to escalate your privileges on a machine thanks to User Defined Functions.  The following links walk through the exploitation process for the various operating systems very well:

**Linux**: [https://infamoussyn.com/2014/07/11/gaining-a-root-shell-using-mysql-user-defined-functions-and-setuid-binaries/](https://infamoussyn.com/2014/07/11/gaining-a-root-shell-using-mysql-user-defined-functions-and-setuid-binaries/)    
**Windows**: [https://osandamalith.com/2018/02/11/mysql-udf-exploitation/](https://osandamalith.com/2018/02/11/mysql-udf-exploitation/)



## Further Reading

[http://www.blackhat.com/presentations/bh-usa-09/DZULFAKAR/BHUSA09-Dzulfakar-MySQLExploit-SLIDES.pdf](http://www.blackhat.com/presentations/bh-usa-09/DZULFAKAR/BHUSA09-Dzulfakar-MySQLExploit-SLIDES.pdf)

