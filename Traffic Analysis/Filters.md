
**IP Filters**

`ip[0] & 0x0f` low nibble: header length in 4octet words. should be 5

`ip[1]` type of service/QoS/DiffServ

`ip[2:2]` total length of datagram in octets 

`ip[4:2]` IP ID number

`ip[6] & 0x80` reserved bit (possibly used for ECN)

`ip[6] & 0x40` DF bit

`ip[6] & 0x20` MF bit

`ip[6:2] & 0x1fff` fragment offset (number of 8octet blocks)

`ip[8]` ttl

`ip[9]` protocol

`ip[10:2]` header checksum

`ip[12:4]` source IP

`ip[16:4]` destination IP

**Samples**

`(ip[12:4] = ip[16:4])` Src IP = Dest IP (land attack) 

`ip[0] & 0xf0` high nibble: IP version. almost always 4

`(ip[0] & 0xf0 != 0x40)` IP versions !=4

`(ip[0:1] & 0x0f > 5)` IP with options set

`(ip[19] = 0xff)` Broadcasts to x.x.x.255

`(ip[19] = 0x00)` Broadcasts to x.x.x.0

`(ip and ip[1] & 0xfc == 0xb8)` search for EF in DSCP

`(ip and ip[1] & 0xfc == 0x28)` search for AF11 in DSCP

`(ip and ip[1] & 0xfc != 0x00)` search for DCSP Packets != 0

`(ip[6] & 0x20 != 0) && (ip[6:2] & 0x1fff = 0)` initial fragments

`(ip[6] & 0x20 != 0) && (ip[6:2] & 0x1fff != 0)` intervening fragments

`(ip[6] & 0x20 = 0) && (ip[6:2] & 0x1fff != 0)` terminal fragments

`(ip[0] & 0x0f) != 5` has ip options (or is truncated, or is just some sort of freak...)

`ip[8] < 5` short TTL value

`ip[6] = 32`

MF set

`iip[2:2] > 999`

IP Packet greater then 999

  

**ICMP Filters**

`icmp[0]`

type

`icmp[1]`

code

`icmp[2:2]`

checksum

**Samples**

`icmp[0]=0x#`

all Packets with ICMP Type

`icmp[0]=0x# and icmp[1]=0x#`

all Packets with ICMP Type X and Code = Y
`icmp[0]=8`

ICMP Request Messages

`icmp[8]=0`

ICMP Request Replay

`icmp[0]=0x11`

ICMP Address Mask Request

`icmp[0]=0x12`

ICMP Address Mask Replay

`icmp[0]=11 and icmp[1]=0`

ICMP Time Exceeded

`icmp[0]=3 and icmp[1]=4`

ICMP Time Exceeded

`icmp[0]=8 and ip[2:2] > 64`

Large ICMP Packets

  

**TCP Filters**

`tcp[0:2]`

source port

`tcp[2:2]`

destination port

`tcp[4:4]`

sequence number

`tcp[8:4]`

ack number

`tcp[12]`

header length

`tcp[13]`

tcp flags

```
---- --S-       0000 0010 = 0x02   normal syn

---A --S-       0001 0010 = 0x12   normal syn-ack

---A ----       0001 0000 = 0x10   normal ack

--UA P---       0011 1000 = 0x38   psh-urg-ack. interactive stuff like ssh

---A -R--       0001 0100 = 0x14   rst-ack. it happens.

---- --SF       0000 0011 = 0x03   syn-fin scan

--U- P--F       0010 1001 = 0x29   urg-psh-fin. nmap fingerprint packet

-Y-- ----       0100 0000 = 0x40   anything >= 0x40 has a reserved bit set

XY-- ----       1100 0000 = 0xC0   both reserved bits set

XYUA PRSF       1111 1111 = 0xFF   FULL_XMAS scan
```


`tcp[14:2]`

window size

`tcp[16:2]`

checksumt

`tcp[18:2]`

urgent pointer

**Samples**

`tcp[13] = 0x02`

is SYN. nothing else.

`(tcp[13] & 0x02) != 0`

contains SYN. we don't care what else...

`(tcp[13] & 0x03) = 3`

is some kind of SYN-FIN. realy Bad


`tcp[20:4] = 0x47455420`

GET in request

  

**UDP Filters**

`udp[0:2]`

source port

`udp[2:2]`

destination port

`udp[4:2]`

datagram length

`udp[6:2]`

UDP checksum

  

**protocols**

`ip[9] == 8`

EGP

`ip[9] == 9`

IGP

`ip[9] == 88`

EIRGP

`ip[9] == 50`

ESP

`ip[9] == 51`

AH

`ip[9] == 89`

OSPF

`ip[9] == 124`

ISIS

**other, see /etc/protocols**

  

**Routing Protocols**

`(udp and port 520) or (host 224.0.0.9)`

RIP 1 + 2

`tcp and port 179`

BGP

`ip[9] == 8`

EGP

`ip[9] == 9`

IGP

`ip[9] == 88`

EIRGP

ip[9] == 89

OSPF

`ip[9] == 124`

ISIS

  

**ether Filters**

`ether[20:2] == 0x2000`

CDP pakets

`ether[12:2] == 0x0806`

ARP pakets

  

**IPv6**

`ip6`

filters native IPv6 traffic (including ICMPv6)

`icmp6`

filters native ICMPv6 traffic

`proto ipv6`

filters tunneled IPv6-in-IPv4 traffic

**TCP**

`ip6 and (ip6[6] == 0x06)`

IPv6 TCP

`ip6 and (ip6[6] == 0x06) and (ip6[53] == 0x02)`

IPv6 TCP Syn

`ip6 and (ip6[6] == 0x06) and (ip6[53] == 0x10)`

IPv6 TCP ACK

`ip6 and (ip6[6] == 0x06) and (ip6[53] == 0x12)`

`IPv6 TCP Syn/ACK
`
**UDP**

`ip6 and (ip6[6] == 0x11)`

IPv6 TCP

**ICMP**

`(ip6[6] == 0x3a)`

ICMP v6

`(ip6[6] == 0x3a) and (ip6[40] == 0x01)`

ipv6 and type 1 Dest Unreachable

`(ip6[6] == 0x3a) and (ip6[40] == 0x02)`

ipv6 and type 2 Packet too big

`(ip6[6] == 0x3a) and (ip6[40] == 0x03)`

ipv6 and type 3 Time Exeedet

`(ip6[6] == 0x3a) and (ip6[40] == 0x04)`

ipv6 and type 4 Parameter Problem

`(ip6[6] == 0x3a) and (ip6[40] == 0x80)`

ipv6 and type 128 Echo Request

`(ip6[6] == 0x3a) and (ip6[40] == 0x81)`

ipv6 and type 129 Echo Reply

`(ip6[6] == 0x3a) and (ip6[40] == 0x86)`

ipv6 and type 133 Router Solicitation

`(ip6[6] == 0x3a) and (ip6[40] == 0x87)`

ipv6 and type 134 Router Advertisement

`(ip6[6] == 0x3a) and (ip6[40] == 0x88)`

ipv6 and type 135 Neighbor Solicitation

`(ip6[6] == 0x3a) and (ip6[40] == 0x89)`

ipv6 and type 136 Neighbor Advertisement

  

**MY Filters**

`tcpdump 'ether[0] & 1 = 0 and ip[16] >= 224'`

IP broadcast or multicast packets that were not sent via ethernet broadcast or multicast:

  

**More Infos**

 [byte_offsets.txt](https://www.packetlevel.ch/html/txt/byte_offsets.txt)  
 [tcpdump.filters](https://www.packetlevel.ch/html/txt/tcpdump.filters)

  
