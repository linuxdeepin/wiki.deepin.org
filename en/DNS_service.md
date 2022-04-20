[[zh:DNS服务]]


## Summary

Domain Name System (DNS) is a service for the Internet. It servers as the database of mappings between domains and IP addresses, making access to Internet more convenient. DNS service uses TCP and UDP on port 53. The max length of the name of each domain level it can deal with is 63, and the whole length of the domain cannot be longer than 253.

Before year of 2008, the legal characters in a domain is confined in a subset of ASCII. After that, ICANN adopted a resolution, making characters from other languages (mainly non-latin languages) in the domain name possible. DNS now uses IDNA system based on Punycode, which maps Unicode to legal DNS character set. Thus, domains like "XXX.中国" can be visited directly through the browser without installing extra plugins. However, due to the predominant use of English, characaters from other languages still have difficulties when being used in domains as they are not easy to input and promote in other countries.

## History

DNS was first invented by Paul Mockapetris in 1983. The original specification is published in RFC 882. The RFC 1034 and RFC 1035 made some correction to it, abandoning the RFC 882 and RFC 883. After that, no changes has been made to DNS specifications. 

In the past, domains must end with a dot ("."), ilike "www.wikipedia.org.". DNS servers in nowadays have been able to automatically complete the dot so that users do not need to type it in the address bar anymore.

## Record types

In a DNS system, the commonly used resource types are:

* Host record (A record): an important record for domain resolve defined in RFC 1035. It maps a specific host name to the related IP address.

* Alias record (CNAME record): defined in RFC 1035, used for point an alias to an A record, so that no extra A record needs to be created when assigning a new host name

* IPv6 host record (AAAA record): defined in RFC 3596, similar to A record. It maps a specific host name to the related IPv6 address.

* Server record (SRV record): defined in RFC 2782, used to provide the location information (such as host name and port) of a specific server

* NAPTRP record: defined in RFC 3403, provided a regular expression to map a domain. It is generally used for ENUM query.

## Implementation

### Outline

DNS realizes a layered namespace by allowing a name server delegating part of its name service (i.e. zones) to its sub-server. Besides, DNS provides extra information including system alias, contact information and the name of the host that servers as the mail hub of system group or system domain.

Any computer network using IP protocol can utilize DNS to establish its own type of name system. However, when mentioned in the public Internet DNS system, the term "domain" is much more preferred.

The public DNS service is based on 13 root servers all over the world. 10 of them are located in the USA. Any other DNS servers are delegated to provide the specific part of the Internet DNS namespace.

### Software

DNS system is powered by different kinds of DNS software, including:

- BIND (Berkeley Internet Name Domain): the most used DNS software
- DJBDNS (Dan J Bernstein's DNS implementation)
- MaraDNS
- NSD (Name Server Daemon)
- PowerDNS

### Domain character extension

Punycode is a coding system made according to RFC 3492. It is used for converting domains using Unicode characters of local language to those recognized by DNS system. The conversion is based on domain name table (designed by IANA). Punycode prevents IDN spoofing as well.

### Domain resolve

Take server "en.wikipedia.org" as example. It has a related IP 198.35.26.96. DNS is then used as a telephone directory, and we can use "en.wikipedia.org", instead of the IP, to call that server. DNS will do the conversion for us after we have typed the host name in the address bar.

There are two kinds of DNS query: recursive query and iterative query. Most DNS clients use recursive query for getting final DNS result, while iterative queries are often used among DNS servers.

For example, if a client queries for the A record of "en.wikipedia.org":

* The client first send a message "query en.wikipedia.org" to a DNS server;

* The DNS receives the message and checks in its cache. If the record exists, it returns the result directly.

* If the result is too old or it does not exist, then the DNS server will send query messages  to upper level domain servers. First it queries the root domain server for "en.wikipdia.org", and gets authorized name server of ".org" domain; then it queries the ".org" domain server for "en.wikipedia.org", and gets authorized name server of "wikipedia.org" server. Finally, it queries the "wikipedia.org" domain server for "en.wikipedia.org", gets the final A record of "en.wikipedia.org".

* The DNS server caches this result, then return it to the client.

### WHOIS (Queries for domain databases)

The owner of a domain can be found by querying the database of WHOIS. For most root domains, basic WHOIS information is maintained by ICANN, while the detail of WHOIS information is provided by the domain register.

For 240 country code top-level domains (ccTLDs), the WHOIS information is generally provided by the authorized register of that domain. For example, China Internet Network Information Center (CNNIC) is responsible to .CN domain; Hong Kong Internet Registration Corporation Limited is responsible for .HK domain, Taiwan Network Information Center is responsible for .TW domain.

## Controversy of DNS

The current control mode of DNS system receives many controversy. The most attacked point is the abuse of DNS by monopoly businesses or 
quasi-monopoly businesses, and the assignment of top-level domains by companies like VeriSign.

Some people claims that many DNS server do not work well on environment of dynamic IP allocation, though this is usually due to the failure of certain implementations instead of the protocol itself.

### Security issues

Some attackers falsify DNS servers to lead users to spoof websites, then gather their personal information. There are currently about 68,000 servers of this kind.

## Installation and configuration of DNS service

The DNS server aims to provide service for conversion from domains to IP addresses. When setting up mail servers, local debugging is sometimes needed, thus a DNS server is required to resolve MX record.

### DNS record type

A record

     www	IN	A	192.168.1.101

CNAME record

     www	IN	A	192.168.1.101
    mail	IN	CNAME	www

NS record

     @	IN	NS	wangyan.org.
     @	IN	A	192.168.1.101

### Install bind9

Execute in terminal:

    sudo apt-get -y install bind9 bind9utils

Modify DNS server address:

     echo "nameserver 192.168.1.101" >> /etc/resolv.conf
    /etc/init.d/networking restart # restart network

### DNS server configuration

First, create forward zone file:

    sudo gedit /etc/bind/named.conf.local

add the following content to the file

    zone "wangyan.org"{
     type master;
     file "db.wangyan.org";
    };

The "master" refers to the main configuration file.

Then configure forwarding. This is to forward DNS queries to a public DNS server when no appropriate DNS resolver can be found.

    sudo gedit  /etc/bind/named.conf.options
    forwarders {
     8.8.8.8;
    };

Add DNS record:

     cp /etc/bind/db.local /var/cache/bind/db.wangyan.org
    vim /var/cache/bind/db.wangyan.org
     :%s/localhost/wangyan\.org/g　# Replace "localhost" with your own domain

### DNS test

See if DNS has been specified

     cat /etc/resolv.conf

See if MX record takes effect:

     nslookup -qt=mx extmail.org (windows)
    host -t mx example.com (linux)

Test with ping utility:

     ping wangyan.org

Test with ping dig:

    dig wangyan.org (linux)

Test with named-checkzone utility:

    named-checkzone wangyan.org /var/cache/bind/db.wangyan.org

##相关链接

[维基百科:域名系统](http://zh.wikipedia.org/wiki/%E5%9F%9F%E5%90%8D%E7%B3%BB%E7%BB%9F)

[Debian/Ubuntu 快速架构DNS服务器](http://wangyan.org/blog/debian-setup-dns.html)