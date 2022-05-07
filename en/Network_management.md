---
title: Network_management
description: 
published: true
date: 2022-05-07T02:30:24.293Z
tags: 
editor: markdown
dateCreated: 2022-04-21T03:56:23.168Z
---

## Summary

This entry introducce briefly the network management in Linux, including modifying network configuration files and commands for managing network.

## Network configuration files

Here are some commonly used configuration files for network:

- Host address configuration: /etc/hosts
- Information of network services: /etc/services
- List of allowed and rejected addresses: /etc/hosts.allow  and /etc/hosts.deny
- Configuration of network interfaces: /etc/network/interfaces
- Rules for looking up hostnames: /etc/host.conf
- Rules for domain name service: /etc/resolv.conf
- Network card configurations: etc/network/interfaces

###/etc/hosts

File /etc/hosts is used for quick name resolving in Linux. It is a ASCII file, contains maps from hostnames (and host alias) to IPs.

In the condition that no domain name server is present, all network program use this file for resolving IP addresses of hostnames; otherwise, DNS service is used. Users can add frequently used domains and their IPs to /etc/hosts to reduce the time of name resolving.

For example, there is a record in /etc/hosts:

        192.168.1.100 linumu100 test100

Supposing "192.168.1.100" is backed by a web server. Visiting http://linumu100 in the browser will give us the web page at 192.168.1.100.

Generally, the first line in the file is the local IP and the local hostname:

         127.0.0.1 localhost.localdomain localhost 

followed by IPs and hostnames of other machines:

        192.168.1.1 debian debian
        192.168.0.2 t02 t02.tiger
        192.168.0.4 t04 t04.tiger

where the three fields of each line correspond to the IP address, the hostname and the host alias, respectively. Lines beginning with "#" are seen as comments.

There are some different between the hostname and the domain. The former are often used in LANs, and are resolved to IPs by checking in /etc/hosts. The later are used on the Internet, and are resolved through DNS service. If we do not want to use DNS service on the Internet, we can change the hosts file and add a hostname to the target IP, so it can be visited by our customized hostname.

#### Tools for modifying hostname

Apart from modifying /etc/hostname, you can also use command "hostname" to change the hostname. For example, to show the current hostname:

    $ hostname
    linmu100

Set a temporary hostname:

    # hostname test100
    test100

This hostname will expire after the user logs out. To make it a permanent one, you must modify the file /etc/hostname.

To show the IP address of the host:

        $ hostname -i
        192.168.1.100
        
Note that the IP is read from file /etc/hosts.


#### Common problems

##### Slow connection to remote Linux hosts

When a client wants to log in to a Linux host via SSH, it has to wait for a moment before it can enter the shell. This is because that Linux resolves the IP of the client. If we add the hostname and the IP of that client to /etc/hosts on the Linux host, it would take less time during login.

Other types of logins, like remote login of MySQL or querying of shared files, can be tuned in the same way.

##### Make a connection between two computers

When two computers are connected directly, both need to set their own IPs, and both need to have the IP and the hostname of the other computer.

###/etc/services

It records the names of all network services, the ports and the protocols they use. It looks like:

        # Each line describes one service, and is of the form:#
        # service-name port/protocol [aliases ...] [# comment]
        tcpmux 1/tcp #TCP port service multiplexer
        tcpmux 1/udp # TCP port service multiplexer
        rje 5/tcp # Remote Job Entry
        rje 5/udp # Remote Job Entry
        echo 7/tcp
        echo 7/udp
        .....

The port number used in Linux ranges from 0 to 65,535, with different subranges assigned for different usage.

        0    Not used
        1 - 1023    Reserverd for system and available only to root
        1024 - 4999    Assigned by client programs
        5000 - 65535    Assigned by other programs

###/etc/hosts.allow and /etc/hosts.deny

These are configuration files of TCPd server who controls the access of external IP to local host. The format of each record in these files are:

    # Name of service process : Host list : Actions to do when rules matched
    server_name:hosts-list[:command]

/etc/hosts.allow holds the IP addresses that are allowed to access the host, while /etc/hosts.deny holds the IP addresses that are forbidden. If there is any conflict, Linux will adopt the later.

Here is an example of /etc/hosts.allow:

        ALL:127.0.0.1 ## Allow access to all services from this machine
        smbd:192.168.0.0/255.255.255.0 ## Allow access to smbd service of clients from network segment of 192.168.0

The keyword "All" matches all cases, "EXCEPT" excludes some cases, and "PARANOID" matches the cases where an IP address is not mapped to the presumed domain.

### /etc/sysconfig/network

File /etc/network/interfaces sets details for each network device. It look like:

    # Automatically obtaining IP
    auto  eth0
    iface eth0 inet dhcp

    # Using static IP
    auto  lo    # Enable "lo" during startup
    iface eth0 inet static    # "eth0" will use a static IP
    address    192.168.1.2    # The IP address assgined to "eth0"
    netmask    255.255.255.0  # Subnetwork mask for "eth0"
    gateway    192.168.1.1    # IP address of the gateway

Note:

1. To manually assign a MAC addresss to a device, append one line after the "iface" line:

    pre-up ifconfig eth0 hw ether xx:xx:xx:xx:xx:xx

where "xx:xx:xx:xx:xx:xx" is the "cloned" MAC address.

2. All changes will take effect only when the device has been restarted:

    sudo /etc/init.d/networking restart

or
    sudo systemctl restart networking.service

### /etc/host.conf

When both DNS service and /etc/hosts are present in system, the order of name resolving is determined by file /etc/host.conf. It look like:

    order hosts,bind    # Order of services to use for resolving
    multi on    # Allow the host to have multiple IPs
    nospoof on    # Forbid IP address spoofing

The keyword "order" defines the order of name resolving: first using the hosts file, then using the bind (DNS) server.

### /etc/resolv.conf

This file determine the priority of all DNS servers. Each line is beginned with a keyword, followed by a parameter indicating the server IP or host name. There are four kinds of keyword:

* nameserver: IP address of a DNS server

* domain: A local domain

* search: A list for domain searching

* sortlist: A list for sorting obtained domains

The keyword "nameserver" is mandatary, as no DNS server can be found without it. Other keywords are optional.

Here is an example of /etc/resolv.conf:

        nameserver 127.0.1.1
        domain corp.linuxdeepin.com
        search corp.linuxdeepin.com
        nameserver 10.0.0.1

### /etc/sysconfig/network-scrips/ifcfg-ethx

This file defines the parameters for a specific network interface. It does not belong to deepin, but is quite common in Red Hat Linux and its derivatives.

Here is an example of this file:

        DEVICE=ethX
        BOOTPROTO=none
        BROADCAST=x.x.x.x 
        HWADDR=00:50:BA:88:72:D4 
        IPADDR=x.x.x.x
        NETMASK=255.255.255.0 
        NETWORK=x.x.x.x 
        ONBOOT=yes
        TYPE=Ethernet
        GATEWAY=x.x.x.x 

## Commands for network management

### ifconfig

"ifconfig" is used to view and change the address of a network interface, as well as its subnet mask and broadcast address. Only super users can change parameters.

Syntax:

    ifconfig interface [options] address
        
Options:

    interface    The interface specified
    address    Assign the given address to the interface
    up    Activate the interface
    down    Disactive the interface
    -broadcast address    Set the broadcast address
    -pointopoint address    Set the P2P address
    -netmask address    Set the subnet mask

Examples:

To configure and activate device "eth0":

         ifconfig eth0 192.168.4.1 netmask 255.255.255.0 up

To configure the IP for device "eth0:1" which is a alias of "eth0", then add a router for it:

        ifconfig eth0:1 192.168.4.2
        route add –host 192.168.4.2 dev eth0:1

Active or disactive "eth0:1":

         ifconfig eth0:1 up
         ifconfig eth0:1 down

See configurations of the specified (or all) device:

         ifconfig eth0
         ifconfig

### ip

ip is a powerful tool for network configuration provided by package "iproute2". It replaces tranditional tools like ifconfig and route, and most of the Linux distributions support it. Only super users can use it to change configurations.

Syntax:

    ip [Options] OBJECT [Command [Arguments]]
        
Options:

    -s, -stats, -statistics    Print more details when executed
    -f, -family    Specify the protocol family type, such ash inet, inet6 or link. inet is implied if not type is specified
    -4    Using inet family
    -6    Using inet6 family
    -0    Using link family
    -o, -oneline    Ouput in a single line for each record. Useful when output need to be parsed by programs like wc or grep
    -r, -resolve    Query the DNS service, and use the hostname obtained for the IP given

Commands:

Commands defines the action to be done, depending the type of the object. Commonly used commands are: add, delete, show and list. The default command is "list", or "help" if the object specified cannot be listed. Users can use

    ip help
    
to get a list of all suppported commands.

Arguments:

Arguments are passed to the command. There are two kinds of arguments: the flag and the parameter. flag consists of a keyword, while the parameter consists of a keyword and a value. Each command has its own default argument, for example

    ip link ls eth0

equals to

    ip link ls dev eth0
    
Examples:

To assign the address 192.168.2.2/24 to interface eth0:

         ip addr add 192.168.1.1/24 dev eth0

To discard all datagram whose sources address is 192.168.2.0/24:

         ip rule add from 192.168.2.0/24 prio 32777 reject

### ping

"ping" is used to check the status of network interfaces. All users can perform the check.

Syntax:

    ping [-dfnqrRv][-c][-i][-I][-l][-p][-s][-t] Address
        
Options:

        -d    Set the SO_DEBUG option on the socket being used. 
        -c    Set the total count of response
        -f    Flood ping with very short interval
        -i    Wait  interval seconds between sending each packet.
        -I    Specify the network interface to use
        -l    Sends preload packets of the specified number while not waiting for reply
        -n    Numeric output only.
        -p    Specify the pattern to fill out the packet
        -q    Nothing is displayed except the summary lines at startup time and when finished.
        -r    Bypass  the normal routing tables and send directly to a host on an attached interface.
        -R    Record the routing process in the packet.
        -s    Specifies the number of data bytes to be sent.
        -t    Set the IP Time to Live.
        -v    Verbose output.

ping is the mostly used network command. It checks the connectivity of networks using UDP or ICMP. Sometimes we may find that a website can be visited through a browser, but cannot be reached by ping command. This is due to the firewall installed for the security concerns. We may also do a test on our own PCs:

    echo 1 > /proc/sys/net/ipv4/icmp_echo_ignore_all
    ping 127.0.0.1

Examples:

ping the public website on the Internet:

         ping -c 5 www.google.com.hk/

ping the gateway:

 ping -b 192.168.120.1

ping for a certain number of times:

         ping -c 10 192.168.120.206
 
ping for a certain number of times with given intervals:

         ping -c 10 -i 0.5 192.168.120.206

### netstat

Use "netstat" to check the status of Linux networks.

Syntax:

    netstat [-acCeFghilMnNoprstuvVwx][-A][--ip]

Options:

    -a, --all    Show both listening and non-listening sockets
    -A    Specifies the address families
    -c, --continuous    print the selected information every second continuously
    -C, --cache    Print routing information from the route cache.
    -e, --extend    Display additional information.
    -F, --fib    Print routing information from the FIB.
    -g, --groups    Display multicast group membership information for IPv4 and IPv6.
    -h, --help    Show help.
    -i, --interfaces    Display a table of all network interfaces, or the specified iface.
    -l, --listening    Show only listening sockets.
    -M, --masquerade    Display a list of masqueraded connections.
    -n, --numeric    Show numerical addresses instead of trying to determine symbolic  host, port or user names.
    -N, --netlink--symbolic    Show symbolic names of peripheral network devices.
    -o, --timers    Include information related to networking timers.
    -p, --programs    Show the PID and name of the program to which each socket belongs.
    -r, --route    Display  the kernel routing tables.
    -s, --statistice    Display summary statistics for each protocol.
    -t, --tcp    Show status of TCP connections.
    -u, --udp    Show status of UDP connections.
    -v, --verbose    Tell the user what is going on by being verbose.
    -w, --raw    Show status of RAW connections.
    -x, --unix    Similar to "-A unix".
    --ip, --inet    Similar to "-A inet"


Example:

netstat can be used to view local network status, including opening ports, active users and service status. Besides, it provides the information about system route tables and interfaces status. By default, netstat show only the ports with established connections. To show all listened port, use "-a" option:

        # netstat -a
        Active Internet connections (only servers)
        Proto Recv-Q Send-Q Local Address Foreign Address State
        tcp 0 0 *:32768 *:* LISTEN
        tcp 0 0 *:32769 *:* LISTEN
        tcp 0 0 *:nfs *:* LISTEN
        tcp 0 0 *:32770 *:* LISTEN
        tcp 0 0 *:868 *:* LISTEN
        tcp 0 0 *:617 *:* LISTEN
        tcp 0 0 *:mysql *:* LISTEN
        tcp 0 0 *:netbios-ssn *:* LISTEN
        tcp 0 0 *:sunrpc *:* LISTEN
        tcp 0 0 *:10000 *:* LISTEN
        tcp 0 0 *:http *:* LISTEN
        ......

The output above indicates that the host provides services like HTTP, FTP, NFS and MySQL

### telnet

"telnet" is used to provide a terminal on the connection with a remote host. It also refers to the protocol used in remote login.

Syntax:

    telnet [-8acdEfFKLrx][-b][-e][-k][-l][-n][-S][-X][Hostname or IP <Port>]
        
Options:

        -8    Specify an 8-bit data path and try to negotiate the TELNET BINARY option on both input and output.
        -a    Attempt automatic login.
        -b    Specify the alias for the remote host.
        -c    Disable the reading of the user's .telnetrc file.
        -d    Start in debug mode.
        -e    Set escape characters for telnet connections.
        -E    Stop any character from being recognized as an escape character.
        -f    Same as "-F"
        -F    Forward a copy of the local credentials to the remote system when using Kerberos V5 authentication
        -k    When using Kerberos V5 authentication, force telnet to obtain tickets for the remote host in the specified realm, instead of the remote host's realm
        -K    Specify no automatic login to the remote system.
        -l    Specify the user name for login.
        -L    Specify  an  8-bit  data path on output.
        -n    Open tracefile for recording trace information.
        -r    Specify a user interface similar to rlogin.
        -S    Set the IP type-of-service (TOS) option for the telnet connection to the specified value
        -x    Turn on encryption of the data stream if supported.
        -X    Disable the atype type of authentication.

Users can login to a remote host and communicate with them through telnet. The login process is just like that on the local machine. To "telnet" to a remote host, users must know the legal user name and password. Although some system may provide remote login for guests, the permission of guests is stricted for security concerns, thus with few functions allowed to use.

telnet simulates the traditional terminal, rather graphical environments like X-Window. When a remote user has logged in, the system will put him / her in a restricted shell, so that no damage can be caused intendedly or unintendedly. Users can also "telnet" from remote hosts to their own computers, then check emails, edit text and run programs, just like the local login.

### ftp

"ftp" is a program used for remote file transfer. FTP is also the standard protocol for file transfer for ARPANet, the predecessor of the Internet.

Syntax:

    ftp [-dignv][Hostname or IP]
        
Options:

        -d    Enables debugging.
        -i    Turns off interactive prompting during multiple file transfers.
        -g    Disables file name globbing.
        -n    Disable auto-login.
        -v    Forces ftp to show all responses from the remote server, as well as report on data transfer statistics.

ftp command is the standard inteface of file transfer protocol. It allows easy transfer of ASCII files and binary files between computers on a TCP/IP network. Users need to know a legal user name and a password for login to remote hosts before they can use ftp to exchange files. The user name / password pair is then used to verify the session and determine the access permission.

Users can use a ftp client to connect to a ftp server, where they can list directories, download and upload files. There are 72 internal ftp commands, and some commonly used commands are listed below:

    ls    List the content of the curreent directory
    cd    Change the working directory on the remote host
    lcd    Change the working directory on the local machine
    close    Close current FTP session
    hash    Show a hashtag every time the data buffer is cleared
    get(mget)    Download files to the local machine
    put(mput)    Upload files to the remote host
    quit    Disconnect from the remote host and quit ftp program
    
### route

"route" is used to generate, manipulate and view route tables.

Syntax:

        route [-add][-net|-host] TargetAddress [netmask Mask] [[Device]Interface]
        route [-delete][-net|-host] TargetAddress [gw Gateway] [netmask Mask]  [[Device]Interface]
        
Options:

        -add    Add a router
        -delete    Delete a router
        -net    Specify a network (instead of a host) as the target of a router
        -host    Specify a host as the target of a router
        netmask Mask    Specify the subnet mask for a router
        gw Gateway    Specify the gateway for a router
        [[Device]Interface]    Specify the inteface for a router

Users can use route command to set proper routers for Linux so that the system can communicate with other devices in the network. In order to establish a connection between devices in different subnets, either a router connecting two subnets or a gateway within two subnets is required.

In some cases, users need to manually set routers for Linux, so that: a machine in a local area network where a gateway accessible to the Internet is present can access the Internet through this gateway. The gateway here serves as a router for the machine in the LAN.


Use the following command to create a default router:

         route add 0.0.0.0 192.168.1.1

### rlogin

"rlogin" is another program for remote login.

Syntax:

    rlogin [ -8EKLdx ] [ -e Char ] [-k Realm ] [ - l Username ] Host
        
Options:

    -8 allows an eight-bit input data path at all times; otherwise parity bits are  stripped  except when the remote side's stop and start characters are other than ^S/^Q.
    
    -E    Stop escaping any character. When used with "-8", rlogin provides a totally explict connection
    -K    Close all authentication. Only useful when connected to hosts that use Kerberos authentication
    -L    Allows the rlogin session to be run in litout mode.
    -d    Turn  on socket debugging on the TCP sockets used for communication with the remote host.
    -e    Specify the  escape  character. There is no space separating this option flag and the new escape character.
    -k    Request rlogin to obtain tickets for the remote host in the specified realm instead  of  the  remote  host's  realm  as determined by krb_realmofhost.
    -x    Turn on DES encryption for data passed via the  rlogin  session. This significantly reduces response time and significantly increases CPU utilization.

If a user has several accounts in different systems, then he / she need first register in these systems, and remote logins to the accouints. rlogin provides exactly this function for remote logins.

### rcp

"rcp" is used for remote copy of files. All users can use it.

Syntax:

    rcp [-px] [-k Realm] File1 File2
    rcp [-px] [-r] [-k Realm] File
        
Options:
    -r    Copy recursively all files in source directories to the target directory
    -p    Attempt to preserve the modification times and modes of the source files in the copies, ignoring the umask.
    -k    Obtain tickets for the remote host in specified realm instead of the remote host's realm as determined by krb_realmofhost
    -x    Encrypt all information transferring between hosts.

### finger

"finger" is used to query the account information in a host, including the user name, the home direcotry, the stay time, the login time and the login shell. All user can use it.

Syntax:

    finger [Options] [User]
    finger [Options] [User@Host]
        
Options:

    -s    Show information including login name, real name, terminal name, writing status, stay time and login time.
    -l    Show extra information like home directory, login shell, mail status and the file content of .plan, .project , and .forward in the user's home directory
    -p    Similar to "-l", except that the content of .plan and .project is not shown

Example:

To view the information of all logged in account:

        $ finger
        Login Name Tty Idle Login Time Office Office Phone
        root root tty1 2 Dec 15 11
        root root pts/0 1 Dec 15 11
        root root *pts/1 Dec 15 11

### mail

mail is used to send emails for all users. Mail is also a mail program.

Syntax:

    mail [-s Subject] [-c Address] [-b Address]
    mail -f [Mailbox] Mail [-u User]
        
Options:

    -b Address    List of addresses to send blind carbon copies to.
    -c Address    List of addresses to send carbon copies to.
    -f [Mailbox]  Read in the contents of the user's mailbox (or the specified file) for processing.
    -s Subject    Specify the subject for the mail.
    -u User       Reads the mailbox of the given user name.

###nslookup

"nslookup" is used to query the IP of a machine by its domain. All user can use it.
Generally, it requires a domain name server to provide DNS service. 

Syntax:

    nslookup [IP or Domain]

Example:

To run nslookup in the interactive mode:

    $ nslookup
    Default Server: name.cao.com.cn
    Address: 192.168.1.9
    >

You can type the IP address or the domain to query, then press Enter. To quit the program, type "exit" and then press Enter.

You may also use this command to check if named service is running normally. 

## Common network management operations

### Modify the host name

First modify the file /etc/hostname. Execute in terminal:

         sudo gedit /etc/hostname

Delete the existing content and type the host name you preferred, save your changes and exit.

Then modify /etc/hosts.

         sudo gedit /etc/hosts 

Replace the host name after the IP "127.0.0.1" with the host name that you choose in the previous step, save your changes and exit.

Now reboot your computer for changes to take effect.

### Enable IPv6

The Internet Protocol version 6 (IPv6) is the next generation protocol replacing IPv4. See [wikipedia](https://en.wikipedia.org/wiki/IPv6) for more details about IPv6.

#### Test for IPV6

Two ways to test for the support of IPv6 of the current network environment:

* Execute in terminal: ```ifconfig```, and see if any inet6 address is present

* Visit [www.kame.net](http://www.kame.net) in a browser. If you see a walking turtle, then the local network has already supported the IPv6; otherwise, you will need to configure manually.

#### Configure the IPv6 network

Currently, most of the higher education institutions have network connections with IPv6 support. If you find the IPv6 netework is not working, try the following method.

Install miredo:

         sudo apt-get install miredo

and modify the hosts file:

         sudo gedit /etc/hosts

Now reboot the networking service:

         sudo /etc/init.d/networking restart

Some IPv6 sites that are commonly used:

IPv6 China: http://www.ipv6.net.cn/

IPv6 Google: http://ipv6.google.com/

CERNET2: http://www.cernet2.edu.cn/

Sixy.ch: http://sixy.ch/

### Accelerate DNS resolving

dnsmasq provides DNS cache and DHCP services.

## Frequently asked questions

###  Gibberish of Chinese characters in mud over Telnet

For example, when login to mud server through telnet

    telnet 112.126.83.105 5555

There may be gibberish for Chinese characters. Use this

    luit -encoding gbk telnet 112.126.83.105 5555

to login instead.

luit is a tool for conversion between different encodings. Usage:

    luit -encoding SourceEncoding Command


## References

[Debian学习笔记](http://man.chinaunix.net/linux/debian/debian_learning/index.html)

[Linux操作系统下/etc/hosts文件配置方法](http://www.51cto.com/art/200803/68170.htm)

[linux网络管理常用命令](http://bbs.linuxtone.org/thread-815-1-1.html)