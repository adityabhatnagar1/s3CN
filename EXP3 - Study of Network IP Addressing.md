# Experiment - 3

# Experiment 3: Study of Network IP Addressing

## Aim

To study the classification of IP addresses, subnetting, and supernetting, including how to calculate subnet and supernet masks and identify subnet and supernet addresses.

## Concepts

- **IP class:** the first octet (8-bit group) of an IPv4 address decides its class (A-E), its default mask, and its host capacity.
- **Subnet mask:** a 32-bit value marking which bits are network and which are host. ANDing an IP with the mask gives the network address.
- **Subnetting:** splitting one network into smaller networks by borrowing host bits. It reduces broadcast traffic and wasted addresses.
- **Supernetting:** merging contiguous networks into one larger block by lending network bits back to the host part. It shrinks routing tables.
- **Usable hosts:** 2^h − 2, where h is the number of host bits. One address is the network ID and one is the broadcast.

## Topology & Addressing

Not applicable (Apparatus: NA). This is a paper-based study with worked examples.

| Device | Interface | IP | Mask | Gateway |
| --- | --- | --- | --- | --- |
| NA | NA | NA | NA | NA |

## Procedure

1. Classify an address by its first octet using the table below.
Why: the first octet decides the default mask and host capacity.
    
    
    | Class | Address Range (as given) | Supports |
    | --- | --- | --- |
    | A | 1.0.0.1 to 126.255.255.254 | 16 million hosts on each of 127 networks |
    | B | 128.1.0.1 to 191.255.255.254 | 65,000 hosts on each of 16,000 networks |
    | C | 192.0.1.1 to 223.255.254.254 | 254 hosts on each of 2 million networks |
    | D | 224.0.0.0 to 239.255.255.255 | Reserved for multicast groups |
    | E | 240.0.0.0 to 254.255.255.254 | Reserved |
    
    ⚠️ Note: The ranges above are approximate. Standard ranges are A 0.0.0.0-127.255.255.255 (126 usable networks; 127.x.x.x is loopback), B 128.0.0.0-191.255.255.255 (16,384 networks), C 192.0.0.0-223.255.255.255 (2,097,152 networks), E 240.0.0.0-255.255.255.255. Classful addressing is obsolete; current networks use CIDR (prefix-length addressing, e.g. /26). It is still studied for exams.
    
2. Match the leading bits of the first octet to find the class. Default masks: A 255.0.0.0, B 255.255.0.0, C 255.255.255.0.
Why: the leading bits are the fixed class identifier.

```
! Class A: first bit  0     -> 1-126
! Class B: first bits 10    -> 128-191
! Class C: first bits 110   -> 192-223
! Class D: first bits 1110  -> 224-239
! Class E: first bits 1111  -> 240-255
! Example: 200.1.1.1 -> 200 = 11001000 -> starts 110 -> Class C
```

1. Subnetting: split 192.168.1.0/24 into 4 subnets. Find n (bits to borrow) where 2^n ≥ 4.
Why: subnetting is used to create smaller networks, cutting broadcast traffic and address waste. Here n = 2.

```
! 2^2 = 4 subnets -> borrow 2 host bits
```

1. Calculate the subnet mask: add the borrowed bits to the default mask.
Why: borrowed bits become network bits (set to 1).

```
! /24 + 2 = /26
! 11111111.11111111.11111111.11000000 = 255.255.255.192
! Block size = 256 - 192 = 64
! Subnets: 192.168.1.0, .64, .128, .192
! Hosts per subnet = 2^6 - 2 = 62
```

1. Identify the subnet address of a host by ANDing its IP with the mask. Example host: 192.168.1.130.
Why: AND keeps network bits and zeroes host bits.

```
! 130 = 10000010
! 192 = 11000000
! AND = 10000000 = 128
! Subnet address   : 192.168.1.128
! Usable range     : 192.168.1.129 - 192.168.1.190
! Broadcast address: 192.168.1.191
```

1. Supernetting: combine 192.168.0.0/24, 192.168.1.0/24, 192.168.2.0/24, 192.168.3.0/24 into one block.
Why: supernetting is used to summarise many routes into one, shrinking routing tables.
    
    ⚠️ Note: The modern name is CIDR aggregation or route summarisation. Classful supernetting is not used in current networks.
    
2. Check the three conditions before combining.
Why: an invalid block cannot be represented by a single mask.

```
! 1. Networks are contiguous      -> 0,1,2,3 (yes)
! 2. Count is a power of 2         -> 4 = 2^2 (yes)
! 3. First network is on the block boundary -> 3rd octet 0 is divisible by 4 (yes)
```

1. Calculate the supernet mask: remove the lent bits from the default mask.
Why: lent bits become host bits (set to 0).

```
! 4 networks = 2^2 -> lend 2 bits
! /24 - 2 = /22
! 11111111.11111111.11111100.00000000 = 255.255.252.0
```

1. Identify the supernet address: the first network address of the block, or AND any member with the supernet mask.
Why: all member networks share the same /22 prefix.

```
! 3rd octet: 0 = 00000000, 3 = 00000011 -> common first 6 bits
! Supernet address: 192.168.0.0/22
! Covers: 192.168.0.0 - 192.168.3.255
```

## Verification

| Command/Test | Expected Result |
| --- | --- |
| Class of 10.5.5.5 / 172.16.0.1 / 200.1.1.1 | A / B / C |
| 192.168.1.130 AND 255.255.255.192 | 192.168.1.128 |
| Usable hosts per /26 subnet | 62 |
| Broadcast of subnet 192.168.1.128/26 | 192.168.1.191 |
| Supernet mask for four contiguous /24 networks | 255.255.252.0 (/22) |
| Supernet address for 192.168.0.0-192.168.3.0 | 192.168.0.0/22 |

## Troubleshooting

| Problem | Cause | Fix |
| --- | --- | --- |
| Wrong host count | Forgot to subtract 2 | Use 2^h − 2 |
| Wrong mask value | Bits converted to decimal incorrectly | Write the mask in binary first, then convert each octet |
| Wrong subnet address | Added instead of ANDing | AND the IP with the mask, bit by bit |
| Supernet mask rejected | Networks not contiguous or not on boundary | Recheck the three conditions in step 7 |
| 127.x.x.x treated as Class A host | Loopback range is reserved | Use 1-126 for Class A hosts |

## Viva Q&A

1. **Q:** How is an IP address classified? **A:** By the leading bits (first octet) into classes A-E.
2. **Q:** Why is subnetting developed? **A:** To split one network into smaller ones, reducing broadcasts and wasted addresses.
3. **Q:** How is the subnet mask calculated? **A:** Borrow n bits where 2^n ≥ subnets, then add n to the default prefix length.
4. **Q:** How is the subnet address identified? **A:** AND the host IP with the subnet mask.
5. **Q:** Why is supernetting developed? **A:** To combine contiguous networks into one route, shrinking routing tables.
6. **Q:** How is the supernet mask calculated? **A:** Lend n bits where 2^n = number of networks, then subtract n from the prefix length.

## Result

IP address classes, subnet masks and subnet addresses, and supernet masks and supernet addresses were studied and calculated.

## Submission Checklist

- [ ]  Class table written with ranges and host capacity
- [ ]  Binary class identification shown for sample addresses
- [ ]  Subnet mask, block size, and subnet list calculated
- [ ]  Subnet address found using AND
- [ ]  Supernet conditions, mask, and address calculated
- [ ]  Viva answers prepared