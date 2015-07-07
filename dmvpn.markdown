

# DMVPN Configuration #

Phase 1
* mGRE on hub and p2p on spokes
 + NHRP required for spoke registration to hub
 + No spoke to spoke tunnels
* Routing
 + Summarization and default routing is allowed at hub
 + Next-hop on spokes is always changed by hub

Phase 2 (obsolete)
* mGRE on hub and spokes
 + NHRP required for spoke registration to hub
 + NHRP required for spoke to spoke resolution
 + Spoke to spoke tunnel triggered by spoke
* Routing
 + Summarization and default routing is __NOT__ allowed
 + Next-hop on spokes is always preserved by hub
 + Multi-level hierarchy requires hub daisy-chaining

Phase 3
 * mGRE on hub and spokes
  + NHRP required for spoke registration to hub
  + NHRP required for spoke to spoke resolution
 * When hub receives and forwards packet out same interface
  + Send NHRP redirect message back to packet source
  + Forward original packet down to spoke via RIB
 * Routing
  + Summarization and default routing is allowd at hub
   * Results in NHRP type routes for spoke to spoke tunnel
   * With no-summary, Next Hop O? is performed for spoke to spoke
    1. Next-hop is changed from hub IP to spoke IP
 * Next-Hop on spokes is always changed by hub
  + This means NHRP resolution is triggered by hub
 * Multi-level hierarchy works without daisy changing


### Minimal Configuration ###

Note: Used [RFC5737](http://www.rfc-base.org/txt/rfc-5737.txt) and [RFC3849](http://www.rfc-base.org/txt/rfc-3849.txt) addresses in this document.

		TEST-NET-1 (192.0.2.0/24) as the DMVPN addresses.
		TEST-NET-3 (203.0.113.0/24) as the public addresses.
		TEST-NET-2 (198.51.100.0/24) is not used.
		
		2001:DB8::/32 is not used (currently)

	I recommend everyone using these addresses, when possible. :)


Hub:

	interface Tunnel0
		ip address 192.0.2.10 255.255.255.0
		ip nhrp authentication secret
		ip nhrp map multicast dynamic
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
		ip nhrp network-id 1
		ip nhrp map nhs 192.0.2.10
		ip nhrp map multicast 192.0.2.10 203.0.113.1
		tunnel mode gre
		tunnel destination 203.0.113.1
		tunnel source Ethernet0/0
		tunnel key 1
	!
	interface Ethernet0/0
		ip address 203.0.113.101 255.255.255.0
		no shutdown




