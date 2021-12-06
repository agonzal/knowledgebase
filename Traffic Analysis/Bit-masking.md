# TCPDUMP BIT-MASKING (WITH STICKY NOTES)


Of all the topics I have taught to new analysts coming into the SOC, bit-masking was continually the most difficult one for me to articulate. For the longest time I tried to teach bit-masks to students by explaining it logically using math and XOR. Re framing bit-masks as a collection of tiny sticky notes provided a much needed bridge to help students mentally tie this concept to something tangible.

---

### TCP FLAG PRIMER

While tcpdump bit-masking can be used on any byte/nibble, it is often used to isolate combinations of TCP flags so that is the example that we will use here. Let’s take a quick detour through the TCP protocol to give some context to the bits we will be masking. Feel free to hop over this section if you have a firm understanding of TCP and TCP flags.

The TCP protocol sits at the transport layer and is responsible to establishing and maintaining a connection between two sockets or ports. The combination of the flags in this protocol are a way for a host to express intent and indicate that a connection is being requested, accepted, ended, among other things.

![These flags live at the 13th byte offset within the TCP header which can be referred to by tcp[13].](https://images.squarespace-cdn.com/content/v1/5e5df4d5caa7fb722c15ef50/1591066886781-HBWHPRHFYUMBOCVR90PR/tcpflags.png?format=750w)

These flags live at the 13th byte offset within the TCP header which can be referred to by tcp[13].

![They are better understood when tcp[13] is represented in bits rather than as a byte. The following flags correspond to the bits in inside tcp[13]: CWR, ECE, URG, ACK, PUSH, RESET, SYN, FIN](https://images.squarespace-cdn.com/content/v1/5e5df4d5caa7fb722c15ef50/1591674645280-K8ETQKID29EB3VOFI0VY/TCPFlags.png?format=750w)

They are better understood when tcp[13] is represented in bits rather than as a byte. The following flags correspond to the bits in inside tcp[13]: **C**WR, **E**CE, **U**RG, **A**CK, **P**USH, **R**ESET, **S**YN, **F**IN

This isn’t the post to go into the functions of the various flags but here are two pieces of context that will be important for our example:

-   Congestion Flags - The CWR and ECE flags are used for congestion control and are independent from the functional flags (Highlighted in yellow)
    
-   Functional Flags - URG, ACK, PUSH, RESET, SYN, and FIN flags are used to control the connection status and are independent from the congestion flags
    
-   When a Server wants to accept a connection request it responds with a TCP packet with ONLY the **SYN** and **ACK** flags enabled (from the functional flags)
    

---

### SITUATION 1: CONNECTION ESTABLISHED

**Build a filter to capture packets that indicate that a server had accepted a connection (SYN + ACK).**

#### WHY USE A BIT-MASK?

Using a standard BPF (tcp[13] = 0x12) would be a start but an issue arises if any of the congestion flags are enabled. While these flags do not have any impact on the TCP handshake process, they still change the value of the byte tcp[13] as you can see in the example.

In order to focus on the bits that matter, the user needs to ignore or **mask** the insignificant bits.

![Sit1Pack.png](https://images.squarespace-cdn.com/content/v1/5e5df4d5caa7fb722c15ef50/1591743670182-2KUCIJ79I07G5JNVON9Q/Sit1Pack.png?format=750w)

#### BUILDING A TCPDUMP BIT-MASK (POSITIONING THE STICKY NOTES)

When building a bit-mask, the key lies in recognizing the bits you want to ignore vs the ones that you want to preserve. Once you understand this, you can set them accordingly:  
**0 - Ignore this bit/flag value**  
**1 - Preserve this bit/flag value**

![Sit1Mask.png](https://images.squarespace-cdn.com/content/v1/5e5df4d5caa7fb722c15ef50/1591743686507-FNYKN5OGDXU05E7LX33I/Sit1Mask.png?format=500w)

In this situation, we want to **ignore the congestion flags** (CWE/ECE) so we set them to 0 in the mask.  
All the other flags are important for our filter so we set them to 1 in the mask.  
The mask for this situation is:

> 0x3F

Think of the 0’s as taking the form of blue sticky notes that cover the value of the flag and force it to be zero.

#### APPLYING THE TCPDUMP BIT-MASK (TRANSFERRING THE STICKY NOTES)

When applying the bit-mask, imagine that you move those sticky notes to the same position on the bits in the packets. Note that it does not matter what the values of those fields were before because they have all been covered by the mask and changed to zeros.  
The syntax for applying a tcpdump bit-mask is:

> <proto>[offset] & <bit-mask> <operator> <value>

The completed tcp filter with the bit-mask for this situation is:

> tcp[13] & 0x3F = 0x12

Takeaways:

-   Once the congestion bits are masked, the filter then matches packets where the SYN and ACK flags are the only one enabled.
    
-   The filter completely ignores if the congestion bytes were enabled before the masking occurred.
    
-   Notice that Packet D has another functional flag enabled (RESET) that keeps it from matching the filter even after the mask.
    

![Sit1MaskApply.png](https://images.squarespace-cdn.com/content/v1/5e5df4d5caa7fb722c15ef50/1591743703364-6Z0RUPZO4EOUKWGIVR01/Sit1MaskApply.png?format=750w)

---

### SITUATION 2: SYN DETECTED

**Build a filter to capture packets that have the SYN flag enabled.**

#### WHY USE A BIT-MASK?

The only requirement for this situation is that the SYN flag is enabled, it doesn’t matter what combination of flags are enabled with it. Using a standard BPF (tcp[13] = 0x02) would return packets where ONLY the SYN flag was enabled but would not capture packets with other flags flipped.

In order to focus on the bits that matter, the user needs to ignore or **mask** the insignificant bits.

![Sit2Pack.png](https://images.squarespace-cdn.com/content/v1/5e5df4d5caa7fb722c15ef50/1591743722095-6FPP9PIHDSN8O8UV51PT/Sit2Pack.png?format=500w)

####  BUILDING A TCPDUMP BIT-MASK (POSITIONING THE STICKY NOTES)

In this situation, we want to **ignore ALL the flags other than SYN** so we set them to 0 in the mask. The SYN flag is important for our filter because it has to be enabled, so we set it to 1 in the mask.

The mask for this situation is:

> 0x02

Think of the 0’s as taking the form of blue sticky notes that cover the value of the flag and force it to be zero.

![Sit2Mask.png](https://images.squarespace-cdn.com/content/v1/5e5df4d5caa7fb722c15ef50/1591743735982-Z0UOIJR4BA8GB38NWCTW/Sit2Mask.png?format=500w)

#### APPLYING THE TCPDUMP BIT-MASK (TRANSFERRING THE STICKY NOTES)

We once again, slide those sticky notes from the mask into the same potions on our packets. Remember, the syntax for applying a tcpdump bit-mask is:

> <proto>[offset] & <bit-mask> <operator> <value>

The completed tcp filter with the bit-mask for this situation is:

> tcp[13] & 0x02 = 0x02

Takeaways:

-   The filter completely ignores if any flags other than SYN were enabled before the masking occurred.
    
-   Notice that Packet C is the only packet that does not have SYN enabled and therefore was not captured with this filter.
    
-   The mask sets ALL bits other than SYN to 0, leaving only two possible flag combinations 0x00 and 0x02 (depending on if SYN was enabled or not)
    
-   This bit-mask can be reused to find packets that have the SYN flag disabled by changing the value in the filter: tcp[13] & 0x02 = **0x00**
    

![Sit2MaskApply.png](https://images.squarespace-cdn.com/content/v1/5e5df4d5caa7fb722c15ef50/1591743770163-TGV6MHFTJB3OXC3I32J0/Sit2MaskApply.png?format=500w)
