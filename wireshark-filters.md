# Wireshark Filters

## IP Filtering
* Show all traffic with specific IP (src or dst): `ip.addr == 10.0.0.1`
* Show all traffic in a subnet: `ip.addr == 10.0.0.0/24`
* Show traffic from one IP to another: `ip.src == 10.0.0.1 && ip.dst == 10.0.0.2`
* Exclude traffic to/from an IP: `!(ip.addr == 10.0.0.1)`

## Protocol Filtering
* Show TCP or UDP traffic: `tcp or udp`
* Show HTTP or DNS traffic: `http or dns`
* Show ICMP destination unreachable packets: `icmp.type == 3`

## Port Filtering
* Show TCP traffic on specific port: `tcp.port == 80`
* Show TCP traffic with source port below 1000: `tcp.srcport < 1000`

## TCP Flags
* Show packets with SYN flag set: `tcp.flags.syn == 1`
* Show packets with SYN and ACK flags set: `tcp.flags == 0x012`
* Show all retransmitted TCP packets: `tcp.analysis.retransmission`

## HTTP Filtering
* Show HTTP GET requests: `http.request.method == "GET"`
* Show HTTP 404 responses: `http.response.code == 404`
* Show HTTP traffic matching a host: `http.host == "www.abc.com"`

## TLS Filtering
* Show only TLS handshake packets: `tls.handshake`
* Show client Hello packet: `tls.handshake.type == 1`

## DHCP Filtering
* Show DHCP traffic for a subnet: `dhcp and ip.addr == 10.0.0.0/24`
* Show DHCP packets for a specific MAC: `dhcp.hw.mac_addr == 00:11:22:33:44:55`

## DNS Filtering
* Show DNS responses for a domain: `dns.resp.name == cnn.com`

## Frame Filtering
* Show packets containing a keyword: `frame contains keyword`
* Show packets larger than 1000 bytes: `frame.len > 1000`

## Ethernet & MAC Filtering
* Show traffic to/from a MAC address: `eth.addr == 00:11:22:33:44:55`
* Match Ethernet frames at byte offset: `eth[0x47:2] == 01:80`

## Misc
* Filter out ARP, ICMP, and STP background traffic: `!(arp or icmp or stp)`
* Show packets with specific VLAN ID: `vlan.id == 100`
