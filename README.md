# wireshark-traffic-analysis
Beginner Wireshark project exploring DNS, TCP, TLS, ICMP and Nmap port-scan captures.

## What I wanted to learn 

- How to capture packets in Wireshark.
- How DNS queries and responses work
- How to recognise a TCP three-way handshake
- What information is visible in a TLS Client Hello
- How ICMP ping requests and replies appear in a capture
- How SYN, SYN-ACK and RST packets can help when looking at a port scan

## Tools used
- Windows
- Wireshark
- Npcap for live packet capture
- Windows Command prompt
- Wireshark's sample packet captures, including Nmap examples

## 1. DNS
I used the `dns` display filter to find DNS packets. One of the queries I looked at was for `ota.nvidia.com`. I expanded the response and saw CNAME records and IPv4 addresses.

This helped me understand that DNS traffic can show which domain a computer is looking up and what answers the DNS server returns. A normal DNS lookup on its own isn't suspicious.

![DNS response details showing CNAME records and returned IPv4 addresses](screenshots/02-dns-response-details.png)

## 2. TCP handshake

I used this filter to find initial TCP SYN packets: 
`tcp.flags.syn == 1 && tcp.flags.ack == 0`

1. **SYN** - the client starts the connection
2. **SYN-ACK** - The other side responds
3. **ACK** - the client acknowledges the response

Seeing all three packets showed that this connection's TCP handshake completed. A SYN packet by itself only shows an attempt to connect.

![TCP conversation showing SYN, SYN-ACK and ACK](screenshots/03-tcp-handshake.png)

## 3. TLS
I used the `tls` filter and inspected a Client Hello. The packet showed the SNI hostname ota.nvidia.com and a supported-versions extension. TLS application-data packets were also visible.

This showed me that encrypted connections can still expose some information, such as a hostname in the Client Hello, depending on the protocol and configuration. The supported versions listed by the client are versions it can use; they do not mean every version was chosen. I didn't decrypt the application data.

![TLS Client Hello showing SNI and supported versions](screenshots/04-tls-client-hello.png)




