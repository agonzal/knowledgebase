`udp[10] >> 3 & 0xf = 0` dns query.

`udp[11] & 0xf = 3` nxdomain response

`udp[11] & 0xf = 5` refused response. 

## TCP



### SSH
`tcp[(tcp[12]>>2):4] = 0x5353482D && (tcp[((tcp[12]>>2)+4):2] = 0x312E || \
 tcp[((tcp[12]>>2)+4):2] = 0x322E)`
 

### Rlogin
`(tcp[(ip[2:2]-((ip[0]&0x0f)<<2))-1]=0) && \
 ((ip[2:2]-((ip[0]&0x0f)<<2) - (tcp[12]>>2)) != 0) && \
 ((ip[2:2]-((ip[0]&0x0f)<<2) - (tcp[12]>>2)) <= 128)`

### HTTP Data 
`tcp port 80 and (((ip[2:2] - ((ip[0]&0xf)<<2)) - ((tcp[12]&0xf0)>>2)) != 0)`

### Filter TCP
```tcp[(tcp[12]>>2):4] = 0x3232302d || tcp[(tcp[12]>>2):4] = 0x32323020```

### URG set and ACK not set
```tcp[13] & 0x30 = 0x20 ```

### MAP service exploit 
```tcp && (tcp[13] & 2 != 0) && (dst port 143)```

### Filter root backdoor
`tcp[(tcp[12]>>2):2] = 0x2320 && \
 (ip[2:2] - ((ip[0]&0x0f)<<2) - (tcp[12]>>2)) == 2`

### RST set and FIN set
```tcp[13] & 0x05 = 5 ```

### filter out napster

`((ip[2:2] - ((ip[0]&0x0f)<<2) - (tcp[12]>>2)) = 4 && \
 tcp[(tcp[12]>>2):4] = 0x53454e44) || \
 ((ip[2:2] - ((ip[0]&0x0f)<<2) - (tcp[12]>>2)) = 3 && \
 tcp[(tcp[12]>>2):2] = 0x4745 && tcp[(tcp[12]>>2)+2]=0x54)`


### Telnet
```(tcp[(tcp[12]>>2):2] > 0xfffa) && (tcp[(tcp[12]>>2):2] < 0xffff)```

### FTP connection
`dst net 82.48.9.1/22 && dst port 21 \
 && (tcp[13] & 0x3f = 2) && !(dst host ftp.bla.org)`

### attempts to include data on the initial SYN.
`tcp[13] & 0xff = 2 && \
 (ip[2:2] - ((ip[0] & 0x0f) * 4) - ((tcp[12] & 0xf0) / 4)) != 0`

### active open (syn set without ack)
```(tcp[13] & 0x12 < 16)```



### destination port less than 1024
`tcp[2:2] < 1024`

### SYN set and FIN set
`tcp[13] & 0x03 = 3`

### one of the reserved bits of tcp[13] is set
`tcp[13] & 0xc0 != 0 `

### DNS zone transfer
`tcp && dst port 53`

### active open connection, syn is set, ack is not
`tcp[13] & 0x12 = 2`

### X11 ports
`(tcp[2:2] >= 6000) && (tcp[2:2] < 7000)`

### IRC
`(tcp[13] & 0x10 = 1) && (tcp[0:2]=6667 || tcp[2:2]=6667) \
 && (not ip[32:4] = 1346981447 || not ip[32:4] = 1347374663 \
 || not ip[32:4] = 1246710094 || not ip[32:4] = 1364543828)`

### except ack push
`(tcp[13] & 0xe7) != 0`

### all packets with the PUSH flag set
`tcp[13] & 8 != 0`

### all packets with the RST flag set
`tcp[13] & 4 != 0`

### filter out gnutella
`tcp(tcp[12]>>2):4] = 0x474e5554 && \
 tcp[(4+(tcp[12]>>2)):4] = 0x454c4c41 && tcp[8+(tcp[12]>>2)] = 0x20`

### catch default hping 2 pings
`tcp [3] = 0 && tcp[13] = 0 `

### FIN set and ACK not set
`tcp[13] & 0x11 = 1       `

### null scan filter with no flags set
`tcp[13] = 0`
### could also be written as
`tcp[13] & 0xff = 0`

### no flags set, null packet
`tcp[13] & 0x3f = 0`

### syn-fyn 
`tcp[13] = 3`

### syn-fin flags set
`(tcp[13] & 0x03) = 3`

### only syn..
`tcp[13] & 0x02) != 0
`
### reserved bits set
`tcp[14] >= 64`

### incomming http requests 
`(tcp[13:1]&18 = 2) && (port 80) && (ip dst 192.168.1.40)`

### broadcasts x.x.x.255
`ip[19] = 0xff
`
### broadcasts x.x.x.0
`ip[19] = 0x00
`
### Incomming SYN packets
`tcp && (tcp[13] & 0x02 != 0) && \
 (tcp[13] & 0x10 = 0) && (not dst port 53) && \
 (not dst port 80) && (not dst port 25) && (not dst port 21)`

### SMB
`dst port 139 && tcp[13:1] & 18 = 2`

### ACK flag set, ack value is ZERO. Not normal for three-way handshake. Possible capture of NMAP(1) os fingerprinting.
`tcp[13] & 0xff = 0x10 && tcp[8:4] = 0`

### high-order reserved bits should be ZERO. NMAP(1) sometimes sets the  bit that is in the 64 position for os fingerprinting.
`tcp[13] >= 64`

### SYN set and RST set
`tcp[13] & 0x06 = 6 `

### PSH set and ACK not set
`tcp[13] & 0x18 = 8 
`
### Some filters combined for a general [catch [[bad]] events filter]
`(tcp && (tcp[13] & 3 != 0) && ((dst port 143) || \
 (dst port 111) || (tcp[13] & 3 != 0 && tcp[13] & 0x10 = 0 && \
 dst net 172.16 && dst port 1080) || \
 (dst port 512 || dst port 513 || dst port 514) || \
 ((ip[19] = 0xff) && not (net 172.16/16 || net 192.168/16)) || \
 (ip[12:4] = ip[16:4]))) || (not tcp && igrp && not dst port 520 && \
 ((dst port 111) || (udp port 2049) || ((ip[19] = 0xff) && \
 not (net 172.16/16 || net 192.168/16)) || (ip[12:4] = ip[16:4])))`



### in/out going fragmentation attack
`tcp && ip[6:2]&16383 != 0`

### Top hosts by packets
```tcpdump -nnn -t -c 200 | cut -f 1,2,3,4 -d '.' | sort | uniq -c | sort -nr | head -n 20```

## IP


### all packets with more than 20 bytes of payload
`(ip[2:2] - ((ip[0]&0x0f)<<2) - (tcp[12]>>2)) <= 20`

### ping of death attack
`((ip[6] & 0x20 = 0) && (ip[6:2] & 0x1fff != 0)) && \
 ((65535 < (ip[2:2] + 8 * (ip[6:2] & 0x1fff))`

### more fragments bit is not set [but] the fragment offset is not zero
`((ip[6:1] & 0x20 = 0) && (ip[6:2] & 0x1fff != 0))
`
### any packet with a header more than 20 bytes.
`ip[0] & 0x0f  > 5`

### any packet with more fragments set
`ip[6] & 0x20 !=0
`
### packets with TTL's less than 5
`ip[8] < 5`

### source ip equal to destination ip [classic land attack]
`ip[12:4] = ip[16:4]
`
### another, land attack
`(tcp[0:2] = tcp[2:2]) && (ip[12:4] = ip[16:4])`

### IP options
`(ip[0] & 0x0f) != 5
`
### broadcasts to xxx.xxx.xxx.255 || xxx.xxx.xxx.0
`(ip[19]=0xff) || (ip[19]=0x00)`

### fragmented packet with zero offset 
`ip[6:2] & 0x1fff = 0`

### and more fragments [terminal]
`(ip[6] & 0x20 = 0) && (ip[6:2] & 0x1fff != 0)`

### and even more fragments [intervening]
`(ip[6] & 0x20 != 0) && (ip[6:2] & 0x1fff != 0)`

### my head was fragmented [initially]
`(ip[6] & 0x20 != 0) && (ip[6:2] & 0x1fff = 0)`

### fragmented packets with more coming
`ip[6:1] & 0x20 != 0 `

### more fragments bit is not set, [but] the fragment offset is not zero
`(ip[6:1] & 0x20 = 0) && (ip[6:2] & 0x1fff != 0))`

### unroutable addresses
`not ((ip[12] < 3) || net 5 || net 10 || net 127 || net 172.16 \
 || net 192.168 || (ip[12] > 239)) 
`
### IP options
`ip[0:1] & 0x0f > 5`

### loose source routing, [(ip[0:1] & 0x0f > 5)] ip[20] opts: 
### 7,0x44,0x83,0x89 

### source routing 
`ip[20:1] & 0xff = 131`

### other IP versions than ipv4
`ip && (ip[0] & 0xf0 != 0x40)
`


## ICMP

### fragmentation needed but DF flag set
`(icmp[0] = 3) && (icmp[1] = 4)`

### fragmented ICMP
`icmp && (ip[6:1] & 0x20 != 0)`

### in/out going smurf attack
`icmp && (ip[19:1] = 255)`

### in/out going fragmentation attack
`icmp && ip[6:2] & 16383 != 0`

### Loki Filter
`((icmp[0] = 0) || (icmp[0] = 8)) && ((icmp[6:2] = 0xf001) || (icmp[6:2] = 0x01f0)`

### ICMP address mask requests
`icmp[0] = 17
`
### Frag required but DF set*
`((icmp[0] = 3) && (icmp[1] = 4)) 
`
### source route failed 
`(icmp[0] = 3) && (icmp[1] = 5)`

### all ICMP except ping
`icmp && icmp[0] != 8 && icmp[0] != 0`

### source quench      				`: icmp[0] = 4  `
### redirect           				`: icmp[0] = 5  `
### router advertisement 			`: icmp[0] = 9 ` 
### router solicitation  			`: icmp[0] = 10 `
### parameter problem   			`: icmp[0] = 12 `
### timestamp request    			`: icmp[0] = 13 `
### timestamp reply     			`: icmp[0] = 14 `
### information request 			`: icmp[0] = 15 `
### information reply    			`: icmp[0] = 16 `
### address mask request 		    `: icmp[0] = 17 `
### address mask reply   			`: icmp[0] = 18 `


## UDP

### DNS 
`tshark -2 -R "dns && (dns.flags.response == 0) && ! dns.response_in"`

### teardrop attack
`udp && (ip[6:1] & 0x20 != 0)`

### catch anything udp to port 500 udp 
`-n -vv udp && dst port 500`

### catch udp packets with impossible udp lengths
`(udp[4:2] < 0) || (udp[4:2] > 1500)`

### back Orifice
`-n -vv udp && dst port 31337
`
### UNIX traceroute destports between 33000 and 33999 
`(udp[2:2] >= 33000) && (udp[2:2] <= 33999)
`
### or alternatively..
`udp[2:2] >= 33000 && udp[2:2] < 34000 && ip[8] = 1`

### UDP port scan
`udp && src port = dst port`
