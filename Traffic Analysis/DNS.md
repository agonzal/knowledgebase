Building a bpf to look for authoritative answers from a dns server.

We know that a udp packet is 8 bytes long, so the payload exists at offset 8.

The DNS header looks like this:

1`6 bit identifier
1 bit identifier stating whether the packet is a question or answer
4 bit query field that states the type of message
1 bit signifying whether the query is authoritative or not
`
This is all we need to know to facilitate the bpf.

we have a minimum of 8 bits to look at

00000000
10000100 <-- the ones represent what we want to look at

10000100 == 0x84
`udp[10:1] & 0x84 == 0x84 `

easy enough, now lets only look at authoritative nxdomain's 

now we need to know
`16 bit id
1 bit qa
4 bit type
1 bit auth
1 bit trunc
1 bit recurs`

`1 bit recurs avail
3 bit reserved
4 bit rcode`

rcode (response codes) values:
`0 No error condition.
1 Unable to interpret query due to format error.
2 Unable to process due to server failure.
3 Name in query does not exist.
4 Type of query not supported.
5 Query refused.
`
`udp[10:1] & 0x84 == 0x84 && udp[11:1] & 0xf == 3`

This created 14 bytecode instructions. Can we optimize more?

sure we can look at 2 bytes instead of one 

`udp[10:2] & 0x840f == 0x8403`

essentially we want to only look at the bits that we are interested in
`1000 0100 0000 0011`

but we have to AND along the last 4 bits all set 
`1000 0100 0000 1111`

then look for the value of 
`1000 0100 0000 0011 = 0x8403`

This results in 11 instructions, 3 less than previous

A lot of the time our bpf compiler will do special operations 
to skip over any type of IP options that could be present, lets 
try to do our filter above to skip over these instructions. 

Yeah, we would get funked data if there were ip options present, 
but in most cases these won't even exist.  

`ether[23:1] == 0x11 && ether[44:2] & 0x840f == 0x8403`

this results in 6 instructions! 8 less than what we started with. We sacrifice
edge cases for performance 