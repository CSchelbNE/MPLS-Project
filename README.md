# MPLS-Project
This is a sandbox project made in GNS3. All the Cisco routers use the 7200 iOS image and are run in the GNS3 VM using VMWare workstation. 
I am currently using GNS3 version 2.2.20.

This is a multi-tennant mpls network. There are two customers with independent vrfs configured to allow for routing between remote sites over the provider's mpls backbone. The vrfs contain a default route to the provider's internet gateway (IGW) to simulate global internet traffic. The default route is shared to the customer sites via bgp. Prefix lists are used to control the bgp prefix advertisements to the CE sites as well as between the IGW and Tier1isp routers. 

CE Sites:
Both customers use OSPF as the internal routing protocol. 
A virtual tunnel interface (VTI) is configured between the local and edge customer sites. This tunnel is encrypted using an ipsec profile using a sha-

The customer sites are segregated by vrfs and each site has a ipsec VTI configured to encrypt traffic between the CEs.
