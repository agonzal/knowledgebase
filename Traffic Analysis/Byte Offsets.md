bom## Byte offsets

This document is meant to serve as a quick reference for points
of interest in IP, TCP, UDP and ICMP headers. 


## IP byte offsets


```bpf
ip[0] & 0x0f		- protocol version
ip[0] & 0xf0		- protocol options
ip[0] & 0xff00		- internet header length
ip[1]			- TOS
ip[2:2]			- Total length
ip[4:2]			- IP identification
ip[6] & 0xa		- IP flags
ip[6:2] & 0x1fff 	- fragment offset area
ip[8]			- TTL
ip[9]			- protocol field
ip[10:2]		- header checksum
ip[12:4]		- src IP address
ip[16:4]		- dst IP address
ip[20:3]		- options
ip[24]			- padding

Src IP = Dest IP (land attack)
(ip[12:4] = ip[16:4])

IP versions !=4
(ip[0] & 0xf0 != 0x40)

IP with options set:
(ip[0:1] & 0x0f > 5)

Broadcasts to x.x.x.255:
(ip[19] = 0xff)

Broadcasts to x.x.x.0
(ip[19] = 0x00)
```

## TCP byte offsets


```
tcp[0:2]		- src port
tcp[2:2]		- dst port
tcp[4:4]		- seq number
tcp[8:4]		- ack number
tcp[12] & 0x00ff	- data offset
tcp[12] & 0xff00	- reserved
tcp[13]			- tcp flags

tcp[13] & 0x3f = 0	- no flags set (null packet)
tcp[13] & 0x11 = 1	- FIN set and ACK not set
tcp[13] & 0x03 = 3	- SYN set and FIN set
tcp[13] & 0x05 = 5	- RST set and FIN set
tcp[13] & 0x06 = 6	- SYN set and RST set
tcp[13] & 0x18 = 8	- PSH set and ACK not set
tcp[13] & 0x30 = 0x20	- URG set and ACK not set
tcp[13] & 0xc0 != 0	- >= one of the reserved bits of tcp[13] is set

tcp[14:2]		- window
tcp[16:2]		- checksum
tcp[18:2]		- urgent pointer
tcp[20:3]		- options
tcp[23]			- padding
tcp[24]			- data
```

## UDP byte offsets

```
udp[0:2]		- src port
udp[2:2]		- dst port
udp[4:2]		- length
udp[6:2]		- checksum
udp[8:4]		- first 4 octets of data
```



## ICMP

```
icmp[0]			- type
icmp[1]			- code
icmp[3:2]		- checksum

Destination Unreachable:
icmp[0] = 0x3 (3) 

icmp[4:4]		- unused (per RFC]
icmp[8:4]		- internet header + 64 bits original data
icmp[1]			- 0 = net unreachable;
			- 1 = host unreachable;
			- 2 = protocol unreachable;
			- 3 = port unreachable;
			- 4 = fragmentation needed and DF set;
			- 5 = source route failed.`


Time Exceeded:
icmp[0] = 0xB (11)	

icmp[4:4]		- unused (per RFC]
icmp[8:4]		- internet header + 64 bits original data
icmp[1]			- 0 = TTL exceeded intransit
			- 1 = fragment reassembly time exceeded

Parameter Problem:
icmp[0] = 0xC (12)	

icmp[1]			- 0 = pointer indicates error
icmp[4]			- pointer 
icmp[5:3]		- unused, per RFC
icmp[8:4]		- internet header + 64 bits original data
`

Source Quench:
icmp[0] = 0x4 (4)

icmp[1]			- 0 = may be received by gateway or host
icmp[4:4]		- unused, per RFC
icmp[8:4]		- internet header + 64 bits original data`

Redirect Message:
icmp[0] = 0x5 (5)

icmp[1]			- 0 = redirect for network
			- 1 = redirect for host
			- 2 = redirect for TOS & network
			- 3 = redirect for TOS & host
icmp[4:4]		- gateway internet address
icmp[8:4]		- internet header + 64 bits original data

Echo/Echo Reply:
icmp[0]	= 0x0 (0) (echo reply)
icmp[0]	= 0x8 (8) (echo request)

icmp[4:2]		- identifier
icmp[6:2]		- sequence number
icmp[8]			- data begins
		
Timestamp/Timestamp Reply:
icmp[0] = 0xD (13) (timestamp request)
icmp[0] = 0xE (14) (timestamp reply)

icmp[1]			- 0
icmp[4:2]		- identifier
icmp[6:2]		- sequence number
icmp[8:4]		- originate timestamp
icmp[12:4]		- receive timestamp
icmp[16:4]		- transmit timestamp 

Information Request/Reply:
`icmp[0] = 0xF (15) (info request)
icmp[0] = 0x10  (16) (info reply)

icmp[1]			- 0
icmp[4:2]		- identifier
icmp[6:2]		- sequence number

Address Mask Request/Reply:
icmp[0] = 0x11 (11) (address mask request)
icmp[0] = 0x12 (12) (address mask reply)
```

Sources:

RFC768, "User Datagram Protocol Specification"
RFC791, "Internet Protocol Specification"
RFC792, "Internet Control Message Protocol Specification"
RFC793, "Transmission Control Protocol"
filter files from SHADOW-1.8 source distribution
man pages for tcpdump
"TCP/IP and tcpdump Pocket Reference Guide", SANS