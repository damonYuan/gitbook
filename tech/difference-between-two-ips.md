---
description: 191001 - The difference between 127.0.0.1 and 0.0.0.0
---
# The Difference between 127.0.0.1 and 0.0.0.0

When using tomcat, the tomcat will be bound to the address of 127.0.0.1 by default. While in node.js the bound address is 0.0.0.0. So what is the difference between these two IP addresses?

## IP Address Classification

Before explaining the difference between the two addresses, let's review the basics of IP addresses.

### IP Address Representation

An IP address consists of two parts, net-id and host-id, which are the network number and host number. Net-id specifies the network number where the IP address is located. Host-id indicates the host number on the network where the IP address is located.

which is:
 
 `IP-address ::= { <Network-number>, <Host-number> }`
 
### IP Address Classification

The IP addresses are classified into five categories from A to E. The classification is based on the byte length occupied by the net-id and the first few bits of the network number.

 - Class A: The network number occupies 1 byte. The first digit of the network number is fixed at `0`.
 - Class B: The network number occupies 2 bytes. The first two digits of the network number are fixed at `10`.
 - Class C: The network number occupies 3 bytes. The first three fixed bits of the network number are `110`.
 - Class D: The first four digits are 1110, which is used for multicast, that is, one-to-many communication.
 - Class E: The first four digits are `1111` and are reserved for future use. Among them, ABC three types of addresses are unicast addresses, which are used for one-to-one communication and are most commonly used.

### Special IP Address

Special IP addresses are used to do something special. The following special IP addresses are defined in RFC1700.

 - `{0,0}`: Both the Network-number and the Host-number are 0, which means "this host on this network" and it can only be used as the source address.
 - `{0, host-id}`: A host on this network. Can only be used as a source address.
 - `{-1,-1}`: indicates that all bits of the network number and host number are 1. It is used for broadcast on this network and can only be used as the destination address. Packets sent to this address cannot be forwarded to the networks other than the source network.
 - `{net-id,-1}`: Broadcast directly to the specified network. Can only be used as the destination address.
 - `{net-id, subnet-id, -1}`: Broadcast directly to the specified subnet of the specified network. Can only be used as the destination address.
 - `{net-id,-1,-1}`: Broadcast directly to all subnets of the specified network. Can only be used as the destination address.
 - `{127,}`: Any IP address with a network number of 127. All the IP addresses of this type are internal host loopback, that can never appear on a network outside the host.

## Questions and answers

Next, let's look at the question that was asked before: What is the difference between the `127.0.0.1` and `0.0.0.0` addresses? Let's first look at the commonalities:

 - All belong to a special address.
 - Both belong to the class A address: `01111111` and `00000000`.
 - Both are IPV4 addresses.

Next we look at the two addresses separately.

### 0.0.0.0

In IPV4, the `0.0.0.0` address is used to indicate an invalid, unknown or unavailable target.

 - In the server, `0.0.0.0` refers to all IPV4 addresses on the local machine. If a host has two IP addresses, 192.168.1.1 and 10.1.2.1, and the address monitored by a service on the host is 0.0.0.0, Then the service can be accessed through both the IP addresses.

 - In the route, `0.0.0.0` indicates the default route, that is, the route used when an exact match cannot be found in the routing table.

Summary of usage,

 - When a host has not been assigned an IP address, it is used to indicate the host itself. For example, before an IP address is assigned by DHCP.
 - Used as a default route, meaning "any IPV4 host", which indicates that the target machine is not available.
 - Used as a server to represent any IPV4 address on this machine.

### 127.0.0.1

`127.0.0.1` belongs to the `{127,}` collection, and all addresses with a network number of 127 are called loopback addresses. 

 - Loopback address: All packets destined for this type of address should be loop back.

Summary of usage,

 - Loopback test, by using `ping 127.0.0.1` to test the network device on a machine, the operating system or TCP/IP implementation is working properly.
 - DDos attack defense: After receiving the DDos attack, change the domain A record to `127.0.0.1`, which force the attackers to attack themselves.
 - Most web containers bind themselves to it when working in testing or development environment

### localhost

Compared to `127.0.0.1`, more meaning has been assigned to `localhost`. `localhost` is a domain name, not an IP address. The reason why we often think that `localhost` and `127.0.0.1` is the same because most of the articles tell that the `localhost` points to the address of `127.0.0.1`. In the ubuntu system, the /ets/hosts file will have the following content:

``` 
127.0.0.1 localhost 
127.0.1.1 damon-mac 

# The following lines are desirable for IPv6 capable hosts 
::1 IP6-localhost IP6-loopback 
fe00::0 IP6-localnet 
ff00::0 IP6-mcastprefix 
ff02::1 IP6-allnodes 
ff02::2 IP6-allrouters
```
 
The first line above is the default configuration that will be available on almost every computer. But the meaning of localhost is not limited to `127.0.0.1`.

`localhost` is a domain name and it is used to refer to **this computer** or **this host**, and can be used to obtain network services running on this machine. In most systems, `localhost` is pointed to IPV4's `127.0.0.1` and IPV6's `::1`.

``` 
127.0.0.1 localhost 
::1 localhost
```

Therefore, pay attention to confirm the protocol is IPV4 or IPV6 when using the `localhost`.

## Summary

`127.0.0.1` is a loopback address, it does not mean "this machine". `0.0.0.0` is the real expression of "this machine in this network." Normally we choose to bind to `0.0.0.0` when the server wants to bind the port. In this way the service can be accessed by multiple IP addresses.

For example, I have setup a server with an external network address A and an internal network address B. If the port I bind to is specified as `0.0.0.0`, my application can be accessed through both the internal network address and the external network address. But if I only bind it to the intranet address, I can't access it through the external network address. While it also means that some security risks will be introduced by binding `0.0.0.0`. For services that only require intranet access, you can only bind them with the intranet address.