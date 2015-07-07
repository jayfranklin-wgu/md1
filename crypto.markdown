

### IPSec with Crypto Maps ###

Overview
* Legacy method
* Dynamic routing is not supported (no interface in routing table)
* Used to form on-demand IPSec tunnels

How it works
* Crypto maps is a data plane feature
* Matching traffic causes ISAKMP to start
* Traffic matched via ACL
  + define proxy ID's for IPSec Phase 2
* Allows for granual control over VPN traffic 
  + only send telnet over tunnel, etcetera.

Applying Crypto Maps
* only one per interface
* always outbound direction
* can apply one map to multiple interfaces
* entries processed in top-down approach
* tunnel source applies to interface address
1a. changed using `crypto-map local-address` command

Order of Operations
* Encryption applies after routing (may need static routing)
* Encryption applies after NAT (exemption may be required)



Configuration Steps
* Define Phase 1 ISAKMP Policy
* Define Phase 2 IPSec Policy
* Apply to interface
* Generate interesting traffic

Phase 1 ISAKMP Policy
* Authentication
* Encryption
* Hashing
* DH Group
* Lifetime

Phase 2 IPSec Policy
* Peer address
* Proxy ACL
* Transform Set


## Configuration ##

[R1] --- [Rx] -- [Ry] --- [R2]


    ! R1 Configuration
    !
    ! Define Phase 1 ISAKMP
    !
    crypto isakmp policy 10
     authentication pre-share
     encryption aes 256
     hash sha256
     group 5
    !
    crypto isakmp key SECRETKEY address 192.0.2.2
    !
    crypto map MAP1 10 ipsec-isakmp 
     match address CRYPTO_ACL
     set transform-set TRANSFORM1
     set peer 192.0.2.2
     set transform-set TRANSFORM1  
    !
    crypto-map MAP1 local-address Loopback0
    !
    ip access-list ext CRYPTO_ACL 
     permit ip 192.0.2.2 0.0.0.0 192.0.2.3 0.0.0.0.0
    !
    crypto ipsec transform-set TRANSFORM1 esp-aes 192 esp-sha-hmac
     mode tunnel
 

