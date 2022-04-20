[[zh:DHCP服务]]


## Summary

Dynamic Host Configuration Protocol (DHCP) is a LAN protocol using UDP. It is created for two main purposes:

* Automatically providing  IP addresses to subscribers for ISP
* Providing management of all computers in LAN for network administrator

## Applicability

DHCP introduces a server or a group of servers for managing allocation of network parameters, which is a solution with high fault tolerance. DHCP is useful even in a network with few machines, as it allows addition of a machine to the network without introducing  extra cost.

Even for those servers whose IP addresses rarely change, it is still recommended to use DHCP to assign addresses, because the next time when they need to have new addresses, fewer works need to be done. For other devices like routers or firewalls, DHCP is not recommended.

For the convenience in management, it is possible to set up TFTP service or SSH service on the same machine where DHCP service is provided.

DHCP can also be used to directly provide addresses for servers and desktops through PPP agent; or provide addresses for dial-ups, broadband host and home NAT gateway / router. However, it may not be used on boundless routers or DNS servers.

## Histroy

DHCP becomes a standard protocol of network in October 1993. It evolves from the BOOTP protocal. The current definition of DHCP can be found in RFC 2131, while the suggested standard of DHCP based on IPv6 (DHCPv6) can be found in RFC3315.

## Principle

Each device connected to IP network is assigned with a unique IP address. DHCP helps network administrator monitor and assign IP addresses from a centre node. When a device change its location in the physical network, DHCP can automatically send it a new IP address.

DHCP uses the concept of lease time, which is a fixed time that a device can be connected to the network, This is useful for users from education sector or other industry. By setting a short lease time, DHCP makes it possible to mainitain a stable network where the number of potential devices is larger than the number of available IP addresses. DHCP also supports assignment of static addresses for devices like Web servers.

DHCP and its predecessor, BOOTP, are both commonly used, while DHCP is more advanced. Some operating systems, like Windows NT/2000, support DHCP by default. The client of DHCP or BOOTP, on the other hand, is usually a program installed in most of the computer to acquire a IP address according to the configuration.

## Protocal structure

<table class="block1">

    <tbody>
        <tr>
            <td>8 bits</td>
           <td>16 bits</td>
           <td>24 bits</td>
            <td>32 bits</td>
        </tr>
        <tr>
            <td>Op</td>
           <td>Htype</td>
           <td>Hlen</td>
            <td>Hops</td>
        </tr>
        <tr>
            <td colspan="4">Xid</td>
        </tr>
        <tr>
            <td colspan="2">Secs</td>
           <td colspan="2">Flags</td>
        </tr>
        <tr>
            <td colspan="4">Ciaddr</td>
        </tr>
        <tr>
            <td colspan="4">Yiaddr</td>
        </tr>
        <tr>
            <td colspan="4">Siaddr</td>
        </tr>
        <tr>
            <td colspan="4" >Giaddr</td>
        </tr>
        <tr>
            <td colspan="4">Chaddr (16 bytes)</td>
        </tr>
        <tr>
            <td colspan="4">Sname (64 bytes)</td>
        </tr>
        <tr>
            <td colspan="4">File (128 bytes)</td>
        </tr>
           <tr>
            <td colspan="4">Option (variable)</td>
        </tr>
    </tbody>

 </table>

- Op – Message operation code; it can be BOOTREQUEST or BOOTREPLY
- Htype – Hardware address type
- Hlen – Hardware address length
- Xid – Transaction ID
- Secs – Time elapsed from the obtaining of IP or the begining of lease
- Flags – Field containing flags; currently, it is used to indicate whether it is a unicast or multicast
- Ciaddr – Client IP address
- Yiaddr – Your (client) IP address
- Siaddr – IP address of next server (used in bootstrap)
- Giaddr – IP address of DHCP relay
- Chaddr –MAC address of client
- Sname – Arbitary host name of server, terminated with NULL character
- File – Boot file name, null terminator, property name in DHCP discovery protocol, or restricted path name in DHCP offer protocal
- Options – Optional field. Refer to the selection file in the definition selection list

## Technical details

DHCP uses two ports assigned by IANA for BOOTP: 67/udp on servers and 68/udp on clients.

There are four basic procedure during running of DHCP: lease request, lease offering, lease selection and lease confirmation. After clients have obtained their IPs, they can send an ARP request to avoid IP conflicts due to the overlay of the address pool on DHCP server.

### DHCP Discover

Clients broadcast on the physical subnet to find the available server. A UDP pcakge heading to target 255.255.255.255 (or other broadcast address) is then generated. Network administrator can configure a local router to forward DHCP packages to a DHCP server on another subnet.

A client may also request the last IP address used by itself. If this IP address is still available in the network, DHCP server will grants the request. Otherwise, it will refuse the request and assign a new IP address  if the DHCP server is a authorized one; or just ignore the request if it is a unauthorized one,  causing the client to give up and request for a new IP address.

### DHCP Offer

When a DHCP server receives a IP lease request from a client, it will offer a lease, reserver a IP address for it, and broadcast a DHCPOFFER message to the client. This message contains the MAC address of that client, the IP address provided by the server, a subnet mask, a lease time and an IP of the DHCP server.

The value in Chaddr field is used for checking the configuration.

### DHCP Request

When a client receives a IP lease, it must tells all other DHCP servers about that. Thus, it sends a DHCPREQUEST message that contains the IP of the DHCP server that provided its IP. When other DHCP servers receive this message, they can revoke the lease provided to that client (if any), then make the old address available in the address pool, so it can be assigned to another client in the next time. A client may receive several IP requests at a same time, but should accept only one.

###DHCP Acknowledge

When DHCP receives the DHCPREQUEST message from the client, it does the last work of IP configuration: sending a DHCPACK message to the client, which contains the lease time and any other configurations that may required by the client. At that time, the whole configuration procedure is finished.

### DHCP Release

A client sends a request to DHCP server to revoke its IP address and release the DCHP resource it takes. As the server does not know when clients are detached from the network, this protocol does not host sending of DHCP release message.

### Client configurations

DHCP server provides optional settings for DHCP client. These settings are mentioned in RFC 2132. You may also refer to document written by Internet Assigned Numbers Authority (IANA) - DHCP and BOOTP PARAMETERS.

### Option60 for home LAN construction

DHCP Option60 can be used by DHCP client to identify the compatibility of the provider and DHCP clients. You may refer to Section 9.13 in RFC 2132. Options for default router and provider ID are also mentioned, which can be used to provide specific choices for set up box (STB) on customer-premises equipment (CPE). This is convenient as no port number of bridge or route need to be specified. Also, the bridge is based on the MAC from Option60, thus the switcher can be connected to STB, providing same interfaces for PCs and STBs.

### Installation and configuration

Execute in terminal:

         sudo apt-get install 3- dhcp3-server

Back up the original configuration of the network card and create a new configuration for it:

          sudo cp /etc/default/isc-dhcp-server /etc/default/isc-dhcp-server.bk
         sudo gedit /etc/default/isc-dhcp-server

Modify the network card that needs to be listened. Here we take "eth0" as example. You may determine it by executing `ifconfig -a`.

          INTERFACES="eht0"

Then modify the main configuration file of dhcp:

          sudo cp /etc/dhcp/dhcpd.conf /etc/dhcp/dhcpd.conf.bk
         sudo gedit /etc/dhcp/dhcpd.conf

Add following lines:

         option domain-name “XXX.com”;
         option routers 192.168.4.1;
         subnet 192.168.4.0 netmask 255.255.255.0 {
         range 192.168.4.10 192.168.4.100;

          # IP address pool
         option domain-name-servers 192.168.4.1;

          # Default gateway
         option broadcast-address 192.168.4.255;
         # Broadcast address
         }

and modify default lease time while commenting default domain and DNS name:

         default-lease-time 6000;
         max-lease-time 72000;
         #option domain-name “example.org”;
         #option domain-name-servers ns1.example.org, ns2.example.org;

Restart DHCP service for changes to take effect:

          sudo /etc/init.d/isc-dhcp-server restart

## References

[维基百科:DHCP](http://zh.wikipedia.org/wiki/DHCP)

[Ubuntu 12.04下的DHCP服务器的配置](http://www.slightme.info/archives/59)