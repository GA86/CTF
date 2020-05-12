# RattataTACACS

Forensics challenge on a network communications. From the server we retrieve the file `Chall.pcapng`. These files are readable with Wireshark or tshark.

## Protocols

Using tshark we first have a look at all protocols in file.

```console
$ tshark -r Chall.pcapng -T fields -e frame.protocols | sort | uniq
eth:ethertype:arp
eth:ethertype:ip:icmp:ip:udp
eth:ethertype:ip:tcp
eth:ethertype:ip:tcp:tacplus
eth:ethertype:ip:udp:tftp
eth:ethertype:ip:udp:tftp:data
eth:ethertype:loop:data
eth:llc:cdp
```
