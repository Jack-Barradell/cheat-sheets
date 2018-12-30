# Linux Priv Esc

Once you have a low priv shell, the next step is to priv esc, this involves enumerating the system to look for potential exploitation avenues

## Kernel Version

Checking the kernel version can reveal if the kernel is out of date, and potentially vulnerable to known exploits, this happens surprisingly often where an update has not been applied

    uname -a

## Finding SUID Files

Files with the SUID bit are interesting, as if we can exploit them it becomes possible to run things as another user

    find / -perm -u=s 2>/dev/null

## Finding SGUID Files

Same as SUID bit files, but this time looking for files with the SGUID bit

    find / -perm -g=s 2>/dev/null


## Finding World Writeable Files

Finding files to which anyone can write can be useful, as if they are used in some way, or are executable they can be modified for exploitation

    find / -perm -2 -type f 2>/dev/null

## Finding Running Processes 

Searching for running processes can be useful, as it may be possible to cause the process to carry out unintended actions as its running user

    ps -aux | grep [User]

### Params

    [User]: The user whose processes you want to see

### Examples

    ps -aux | grep root

## Curent Abilities 

If you can, always check to see what the user you are running as can already do

    sudo -l

## Reading Shadow File

It's generally worth checking to see if the shadow file has been badly configured allowing access as if you can read it then you can attempt to crack the password hashes 

    cat /etc/shadow

## Cron 

Looking for automated scripts which run can be useful, as it is likely they will run as another user, so if you can influence the execution then you can run things as the user

    ls -la /etc/cron*

    cat /etc/crontab