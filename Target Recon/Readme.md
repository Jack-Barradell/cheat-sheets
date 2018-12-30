# Target Recon

When setting up to target a machine, you need to carry out some recon, to find it on the network and calculate the attack surface 

## Host Discovery 

Before you can attack a target, you need to know where it is on the network. This is called host discovery.

### Nmap

#### No Port Scan

Detect active hosts on a network either testing a specific IP or a given subnet

    nmap -sn [IP][Subnet]

##### Params 

    [IP]: IP address to test, can scan a range by providing a Subnet
    [Subnet] (optional): Subnet range to scan, commonly /24 or /16

##### Examples

    nmap -sn 192.168.212.25

    nmap -sn 192.168.56.0/24

    nmap -sn 192.168.0.0/16

### Bash

#### One-Liner Using Ping (/24 subnet)

Scan a /24 subnet for hosts which respond to ping, indicating they are up

    for i in $(seq 1 255); do ping -c 1 [Part of IP].$i; done | grep "bytes from"

##### Params

    [Part of IP]: The first 3 octets of an IP address e.g. 192.168.56

##### Examples

    for i in $(seq 1 255); do ping -c 1 192.168.56.$i; done | grep "bytes from"

## Port Discovery

Once you know where the host is, you will want to know what's running on it as this is your attack scope, a method of doing this is port discovery

### Nmap

#### TCP SYN Scan + Version Fingerprinting

Scan a given host or subnet using a SYN scan, attempting to work out what service is running on the port

    nmap -sSV [IP][Subnet]

##### Params

    [IP]: IP address to test, can scan a range by providing a Subnet
    [Subnet] (optional): Subnet range to scan, commonly /24 or /16

##### Examples

    nmap -sSV 192.168.56.102

    nmap -sSV 192.168.56.0/24

#### TCP SYN Scan (All Ports) + Version Fingerprinting

Scan a given host or subnet using a SYN scan across all ports, attempting to work out what service is running on the port

    nmap -sSV -p- [IP][Subnet]

##### Params

    [IP]: IP address to test, can scan a range by providing a Subnet
    [Subnet] (optional): Subnet range to scan, commonly /24 or /16

##### Examples

    nmap -sSV -p- 192.168.56.102

    nmap -sSV -p- 192.168.56.0/24

#### TCP SYN Scan + Version Fingerprinting + Default Scripts

Scan a given host or subnet using a SYN scan, attempting to work out what service is running on the port, running default scripts against the services, these can aid in early automated enumeration such as detecting robots.txt files on web servers

    nmap -sSVC [IP][Subnet]

##### Params

    [IP]: IP address to test, can scan a range by providing a Subnet
    [Subnet] (optional): Subnet range to scan, commonly /24 or /16

##### Examples

    nmap -sSVC 192.168.56.102

    nmap -sSVC 192.168.56.0/24

#### UDP Scan + Version Fingerprinting

Scan a given host or subnet using UDP, attempting to work out what service is running on the port

    nmap -sUV [IP][Subnet]

##### Params

    [IP]: IP address to test, can scan a range by providing a Subnet
    [Subnet] (optional): Subnet range to scan, commonly /24 or /16

##### Examples

    nmap -sUV 192.168.56.102

    nmap -sUV 192.168.56.0/24

### Bash

#### One-Liner Using Netcat

Using netcat cycle over each port detecting if it is open 

    for i in $(seq 1 65535); do nc -nvz -w 1 [IP] $i 2>&1; done | grep -v "Connection refused"

##### Params

    [IP]: IP address to test

##### Examples

    for i in $(seq 1 65535); do nc -nvz -w 1 192.168.56.102 $i 2>&1; done | grep -v "Connection refused"