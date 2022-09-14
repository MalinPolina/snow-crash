The first clue is in the video for snow-crash:
`FIND` tells us which command to use

Let's find the file owned by flag00
```
find / -user flag00 2>/dev/null
```
Two such files were found: `/rofs/usr/sbin/john` and `/usr/sbin/john`

```
> ls -la /usr/sbin/john
----r--r-- 1 flag00 flag00 15 Mar  5  2016 /usr/sbin/john
```
Inside this file is this string: `cdiiddwpgswtgt`
Using [decoder](https://www.dcode.fr/) we get in ROT(15): `nottoohardhere`

```
> getflag
Check flag.Here is your token : x24ti5gi3x0ol2eh4esiuxias
```