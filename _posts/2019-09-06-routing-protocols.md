---
layout: post
title: Routing protocols
date:   2019-09-06 01:26
description: basic knowledge about routing protocols
comments: true
tags:
- networking,routing,OSPF,BGP
---
>[source](https://www.routerfreak.com/understanding-network-routing-protocols/)

The purpose of routing protocols is to learn of available routes that
exist on the enterprise network, build routing tables and
make routing decisions. 

Most common routing protocols include:  
* `RIP`  
* `IGRP`  
* `EIGRP`  
* [`OSPF`](#ospf) 
* `IS-IS`  
* [`BGP`](#bgp)  

There are two primary routing protocol types although many different
routing protocols defined with those two types.
[`Link state`](#link-state-protocols) and
[`distance vector`](#distance-vector-protocols) protocols comprise the primary types.

#### Distance vector protocols
> advertise their routing table to all directly
connected neighbors at regular frequent intervals using a lot
of bandwidth and are slow to converge. When a route becomes unavailable,
all router tables must be updated with that new information.
The problem is with each router having to advertise that new information
to its neighbors, it takes a long time for all routers to have a current
accurate view of the network. Distance vector protocols use fixed
length subnet masks which aren’t scalable.

#### Link state protocols
> advertise routing updates only when they occur which uses bandwidth
more effectively. Routers don’t advertise the routing table which makes
convergence faster. The routing protocol will flood the network with 
link state advertisements to all neighbor routers per area in an attempt
to converge the network with new route information. The incremental 
change is all that is advertised to all routers as a multicast
LSA update. They use variable length subnet masks, which are scalable
and use addressing more efficiently.

### OSPF:
> Open Shortest Path First is a true link state protocol developed as 
an open standard for routing IP across large multi-vendor networks. 
A link state protocol will send link state advertisements to all 
connected neighbours of the same area to communicate route information. 
Each OSPF enabled router, when started, will send hello packets to all 
directly connected OSPF routers.

> The hello packets contain information such as router timers, 
router ID and subnet mask. If the routers agree on the information 
they become OSPF neighbours. Once routers become neighbours 
they establish adjacencies by exchanging link state databases. 
Routers on point-to-point and point-to-multipoint links 
(as specified with the OSPF interface type setting) automatically 
establish adjacencies. Routers with OSPF interfaces configured as 
broadcast (Ethernet) and NBMA (Frame Relay) will use a designated 
router that establishes those adjacencies.
 
> #### Convergence
> Fast convergence is accomplished with the SPF (Dijkstra) algorithm 
which determines a shortest path from source to destination. 
The routing table is built from running SPF which determines all routes 
from neighbor routers. Since each OSPF router has a copy of the topology 
database and routing table for its particular area, any route changes 
are detected faster than with distance vector protocols and alternate 
routes are determined.

> #### Designated Router
> Broadcast networks such as Ethernet and Non-Broadcast Multi Access 
networks such as Frame Relay have a designated router (DR) and a 
backup designated router (BDR) that are elected. Designated routers 
establish adjacencies with all routers on that network segment. This is 
to reduce broadcasts from all routers sending regular hello packets to 
its neighbors. The DR sends multicast packets to all routers that it 
has established adjacencies with. If the DR fails, it is the BDR that 
sends multicasts to specific routers. Each router is assigned 
a router ID, which is the highest assigned IP address on a working 
interface. OSPF uses the router ID (RID) for all routing processes.


### BGP:
> The Border Gateway Protocol (BGP) is an inter-Autonomous System
routing protocol. BGP works over TCP on port 179 

> The primary function of a BGP speaking system is to exchange network
reachability information with other BGP systems.  This network
reachability information includes information on the list of
Autonomous Systems (ASes) that reachability information traverses.
This information is sufficient for constructing a graph of AS
connectivity for this reachability, from which routing loops may be
pruned and, at the AS level, some policy decisions may be enforced.

> Border Gateway Protocol is an exterior gateway protocol,
Exterior gateway protocols such as BGP route between 
autonomous systems, which are assigned a particular AS number. 
AS numbers can be assigned to an office with one or several BGP routers.
The BGP routing table is comprised of destination IP addresses, 
an associated AS-Path to reach that destination and a next hop router 
address. The AS-Path is a collection of AS numbers that represent each 
office involved with routing packets.
BGP is utilized a lot by Internet Service Providers (ISP) 
and large enterprise companies that have dual homed internet connections 
with single or dual routers homed to the same or different Internet 
Service Providers. BGP will route packets across an ISP network, which 
is a separate routing domain that is managed by them.

> The ISP has its own assigned AS number, which is assigned by InterNIC. 
New customers can either request an AS assignment for their office from 
the ISP or InterNIC. A unique AS number assignment is required for 
customers when they connect using BGP. There are 10 defined attributes 
that have a particular order or sequence, which BGP utilizes as metrics
to determine the best path to a destination.

> Companies with only one circuit connection to an ISP will implement a 
default route at their router, which forwards any packets that are 
destined for an external network. BGP routers will redistribute routing 
information (peering) with all IGP routers on the network (EIGRP, RIP, 
OSPF etc) which involve exchange of full routing tables. Once that is 
finished, incremental updates are sent with topology changes. Each BGP 
router can be configured to filter routing broadcasts with route maps 
instead of sending/receiving the entire internet routing table.

#### BGP Routing Table Components:
* Destination IP Address / Subnet Mask
* AS-Path
* Next Hop IP Address
