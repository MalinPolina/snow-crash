```
level09@SnowCrash:~$ ls -la
total 24
dr-x------ 1 level09 level09  140 Mar  5  2016 .
d--x--x--x 1 root    users    340 Aug 30  2015 ..
-r-x------ 1 level09 level09  220 Apr  3  2012 .bash_logout
-r-x------ 1 level09 level09 3518 Aug 30  2015 .bashrc
-rwsr-sr-x 1 flag09  level09 7640 Mar  5  2016 level09
-r-x------ 1 level09 level09  675 Apr  3  2012 .profile
----r--r-- 1 flag09  level09   26 Mar  5  2016 token

level09@SnowCrash:~$ cat token
f4kmm6p|= p n  DB Du{  

level09@SnowCrash:~$ ./level09
You need to provied only one arg.

level09@SnowCrash:~$ ./level09 token
tpmhr

level09@SnowCrash:~$ strings ./level09
You should not reverse this
```
Ok, not to reverse it. Got it <br />
Well then, let's look at hex for token:
```
level09@SnowCrash:~$ od -c -b token
0000000   f   4   k   m   m   6   p   |   = 202 177   p 202   n 203 202
        146 064 153 155 155 066 160 174 075 202 177 160 202 156 203 202
0000020   D   B 203   D   u   { 177 214 211  \n
        104 102 203 104 165 173 177 214 211 012
0000032

level09@SnowCrash:~$ hexdump token
0000000 3466 6d6b 366d 7c70 823d 707f 6e82 8283
0000010 4244 4483 7b75 8c7f 0a89
000001a

level09@SnowCrash:~$ hexdump -C token
00000000  66 34 6b 6d 6d 36 70 7c  3d 82 7f 70 82 6e 83 82  |f4kmm6p|=..p.n..|
00000010  44 42 83 44 75 7b 7f 8c  89 0a                    |DB.Du{....|
0000001a
```
But what does level09 do?
```
level09@SnowCrash:~$ ./level09 flag
fmcj
level09@SnowCrash:~$ ./level09 a
a
level09@SnowCrash:~$ ./level09 b
b
level09@SnowCrash:~$ ./level09 c
c
level09@SnowCrash:~$ ./level09 abc
ace
level09@SnowCrash:~$ ./level09 aaa
abc
```
`level09` encodes string by adding index to each char. Trying to input encoded token doesn't work so we should decode token instead <br />
Simple program to decode token:
```
#include <stdio.h>

int main(int argc, char** argv) {
        char* str = argv[1];
        int i = 0;
        while (str[i]) {
                printf("%c", (str[i] - i));
                i++;
        }
        return 0;
}
```
Let's run:
```
> gcc decode09.c; ./a.out `cat token`
f3iji1ju5yuevaus41q1afiuq
```
Now time to get out flag:
```
level09@SnowCrash:~$ su flag09
Password:
Don't forget to launch getflag !
flag09@SnowCrash:~$ getflag
Check flag.Here is your token : s5cAJpM8ev6XHw998pRWG728z
```
