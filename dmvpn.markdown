

# DMVPN Configuration #

### Minimal Configuration ###

Note: Used [RFC5737](http://www.rfc-base.org/txt/rfc-5737.txt) addresses in this document.

		TEST-NET-1 (192.0.2.0/24) as the DMVPN addresses.
		TEST-NET-3 (203.0.113.0/24) as the public addresses.
		TEST-NET-2 (198.51.100.0/24) is not used.

	I recommend everyone using these addresses, when possible. :)


Hub:

	interface Tunnel0
		ip address 192.0.2.10 255.255.255.0
		ip nhrp authentication secret
		ip nhrp map multicast
		ip nhrp network-id 1
		tunnel source Ethernet0/0
		tunnel mode gre multipoint
		tunnel key 1
	!
	interface Ethernet0/0
		ip address 203.0.113.1 255.255.255.0
		no shutdown
	!

Spoke:

	interface Tunnel0
		ip address 192.0.2.101 255.255.255.0
		ip nhrp authentication secret
		ip nhrp map nhs 192.0.2.10
		ip nhrp map multicast 192.0.2.10 203.0.113.1
		tunnel source Ethernet0/0
		tunnel key 1
	!
	interface Ethernet0/0
		ip address 203.0.113.101 255.255.255.0
		no shutdown



