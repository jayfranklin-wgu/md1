# DMVPN Configuration #

### Minimal Configuration ###

Hub:
	interface Tunnel0
		ip address 192.0.2.10 255.255.255.0
		tunnel source Ethernet0/0
	!
	interface Ethernet0/0
		ip address 203.0.113.1 255.255.255.0
		no shutdown
	!

Spoke:
	interface Tunnel0
		ip address 192.0.2.101 255.255.255.0
		tunnel source Ethernet0/0
	!
	interface Ethernet0/0
		ip address 198.51.100.101 255.255.255.0
		no shutdown
