The level01 folder is empty but, thankfully, filename `john` from previous level looks like a clue <br />
*John The Ripper* is a free password cracker which can be installed with a simple command:
```
sudo apt install john
```
On Unix systems passwords are kept encrypted in `/etc/password` file, so let's see what's in it:  
```
level01@SnowCrash:~$ cat /etc/passwd
...
flag01:42hDRfypTqqnw:3001:3001::/home/flag/flag01:/bin/bash
...
```
After copying passwd file out of VM run the following command:
```
> john --show passwd
flag01:abcdefg:3001:3001::/home/flag/flag01:/bin/bash

1 password hash cracked, 0 left
```
Input password `abcdefg` and get flag:
```
level01@SnowCrash:~$ su flag01
Password:
Don't forget to launch getflag !
flag01@SnowCrash:~$ getflag
Check flag.Here is your token : f2av5il02puano7naaf6adaaf
```
