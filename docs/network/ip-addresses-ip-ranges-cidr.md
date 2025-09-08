# IP Addresses, IP Ranges, CIDR

Networking involves understanding IP addresses and how they are grouped into networks. This guide explains the basics of
IP addresses, ranges, and CIDR notation.

## IP Addresses

An **IP address** is a unique identifier for a device on a network. IPv4 addresses consist of **32 bits** and are
usually written in dotted decimal format, like:

10.10.10.1

- Each part separated by dots is called an **octet** (8 bits).
- Each octet can have a value from 0 to 255.

**Example:**

10.10.10.0 → Network address  
10.10.10.1 → First usable host  
10.10.10.255 → Broadcast address

![](<images/ip_address_bits.png>)

---

## IP Ranges

An **IP range** is a set of IP addresses within the same network. Networks are defined by a **subnet mask**, which
separates the network portion from the host portion of the address.

- A subnet mask can be written in traditional form (e.g., 255.255.255.0) or in **CIDR notation** (e.g., /24).
- The subnet mask determines how many IP addresses are in the network.

**Calculator Tool: [MX Toolbox](https://mxtoolbox.com/subnetcalculator.aspx)**

![](<images/ip_range.png>)

---

## CIDR Notation

**CIDR (Classless Inter-Domain Routing)** is a compact way to represent networks and ranges of IP addresses.

It uses the format:

NetworkAddress/PrefixLength

- **NetworkAddress**: Starting IP of the network
- **PrefixLength**: Number of bits used for the network

**Example: 10.10.10.0/24**

- Network: 10.10.10.0
- Prefix length: 24 bits
- Host bits: 32 - 24 = 8 bits

**Number of addresses:**

- Total addresses = 2^(host bits) = 2^8 = 256
- Usable addresses = Total - 2 (network + broadcast) = 254

So, 10.10.10.0/24 contains addresses from 10.10.10.0 to 10.10.10.255.

![](<images/cidr_ip_block.png>)

---

## Calculating CIDR Ranges

To calculate a CIDR range:

1. Determine the number of host bits: 32 - prefix length
2. Calculate total addresses: 2^(host bits)
3. Identify network address (first address)
4. Identify broadcast address (last address)
5. Usable IP addresses are all addresses between network and broadcast

**Example:**

192.168.1.0/26

- Host bits = 32 - 26 = 6
- Total addresses = 2^6 = 64
- Network: 192.168.1.0
- Broadcast: 192.168.1.63
- Usable: 192.168.1.1 to 192.168.1.62

---

## Summary

- **IP addresses** identify devices on a network
- **IP ranges** are groups of addresses defined by a subnet
- **CIDR notation** simplifies network representation
- **Calculating ranges** helps plan networks and allocate IP addresses efficiently

This guide provides a foundation for understanding IP addressing and network planning.
