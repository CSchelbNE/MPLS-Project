# MPLS-Project
This is a sandbox project made in GNS3. All the Cisco routers use the 7200 iOS image and are run in the GNS3 VM using VMWare workstation. 
I am currently using GNS3 version 2.2.20.

This is a multi-tennant mpls network. There are two customers (CEs) with independent vrfs configured to allow for routing between remote sites over the provider's mpls backbone. The vrfs contain a default route to the provider's internet gateway (IGW) to simulate global internet traffic. The default route is shared to the CEs via bgp. Prefix lists are used to control the bgp prefix advertisements to the CE sites as well as between the IGW and Tier1isp routers.

The Tier1ISP's loopback is used to simulate globally routed traffic. It is not advertised in the bgp peering with the IGW. It is only reachable via the default route propogated inside the MPLS backbone. The loopbacks on the Customersite routers are used to simulate local end user traffic so any ping/traceroute commands should use loopbacks as the source in order to check for reachability. 



CE Sites:
Both customers use OSPF as their internal routing protocol. A prefix-list is used to control the ospf redistribution of the CERemoteOffice's Loopbacks into bgp. This is in order
to allow globally routed internet traffic a path back to the customer site via the MPLS backbone.

A virtual tunnel interface (VTI) is configured between the local and edge customer sites. This tunnel is encrypted using an ipsec profile configured with esp-aes 256 aes-sha-hmac. 

Even though MPLS creates a L3 VPN via segmenteation some regulatory requirements might dictate that CE-CE traffic must also be encrypted in transit. 
The default route propogated from the CELocal site through ospf requires that all globally routed traffic from the remote sites must be passed over the VTI and through the LocalCE's connection to the ISP's mpls backbone.This, in theory, to allow for the LocalCE to filter all globally routed traffic coming from the RemoteCE through a firewall.



Provider MPLS Backbone:

The Provider Edge(PE) routers are configured with prefix lists to control what information is distributed to the CE routers. This is due to the fact many customer stub network routers are not able to manage a full BGP prefix table. 

The IGW is configured with a prefix list to control what customer prefixes are distributed to the ISP peer. 

The IGW is configured with an access list to only allow bgp tcp connections with the IP of the trusted ISP peer. This is intended as a mechanism to protect the BGP peering from 
being hijacked.

There are three BGP confederations configured inside of AS222. This is in order to reduce the total number of iBGP pairings necessary within AS222. 

MPLS is configured to allow label switching for the default route to the IGW. 

OSPF is used for reachability within the MPLS core. iBGP is used to exchange vpnv4 prefixes between the PEs and the IGW. 




