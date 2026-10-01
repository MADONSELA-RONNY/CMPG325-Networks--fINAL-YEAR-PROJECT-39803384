Functional Requirements
●	Design and simulate a computer network for Kgosi Brick & Block Works using Cisco Packet Tracer.
●	Use the assigned addressing block 172.30.14.0/23 as the basis of the IP addressing plan.
●	Provide appropriate connectivity and network services for the manufacturing scenario (office/admin, production floor, sales/yard operations).
●	Configure and demonstrate the assigned networking challenge: Router-on-a-Stick (sub-interface inter-VLAN routing).
●	Accommodate Change Request CR7: four CCTV cameras are added to the network and their traffic must be segmented from other traffic.
●	Produce a working, testable Packet Tracer (.pkt) implementation with evidence of successful connectivity.
1.2 Non-Functional Requirement (Design Constraint)
●	Load-shedding resilience: core network devices (router and switch) must be protected by an Uninterruptible Power Supply (UPS).
●	Minimal device count is preferred, to reduce cost, complexity, and the number of devices that require UPS backup.
1.3 Interpretation
Because the client is a small manufacturing business, the network has been kept deliberately lean: a single router performing Router-on-a-Stick inter-VLAN routing, and a single access/trunking switch, are used to satisfy both the minimal-device-count preference and the UPS/load-shedding constraint (only two devices need battery backup). Four traffic groups were identified from the client context and the CR7 change request: Admin/Office, Production Floor, Sales/Yard, and Security/CCTV, each placed on its own VLAN and subnet for logical segmentation.
2. Physical Topology
The physical topology shows the actual devices and cabling to be implemented in Cisco Packet Tracer. Router0 and Switch0 form the network core and are the only devices connected to the UPS, satisfying the load-shedding resilience constraint while keeping the overall device count to a minimum.
 
●	Router0 (ISR 2911) connects to Switch0 via a single 802.1Q trunk link (Gi0/0 <-> Fa0/24) carrying all VLANs - this trunk is what enables the Router-on-a-Stick configuration.
●	Switch0 provides access ports for end devices, each assigned to a specific VLAN.
●	PC-Admin1/2 (VLAN 10), PC-Prod1/2 (VLAN 20), PC-Sales1 (VLAN 30), and CCTV1-4 (VLAN 40, added per CR7) connect to their respective access ports.
●	Router0 and Switch0 are the only devices on UPS backup power, minimising the cost and footprint of the load-shedding resilience measure.
3. Logical Topology
The logical topology illustrates the VLAN structure, IP subnets, and default gateways used for inter-VLAN routing. Router0 uses one physical interface (Gi0/0) divided into logical sub-interfaces, one per VLAN, each configured with 802.1Q encapsulation and acting as the default gateway for that VLAN - this is the Router-on-a-Stick design required by the assigned technical challenge.
 
●	VLAN 10 (Admin/Office), VLAN 20 (Production), VLAN 30 (Sales/Yard), and VLAN 40 (Security/CCTV - CR7) are each mapped to a dedicated /26 subnet within 172.30.14.0/23.
●	VLAN 99 is reserved as an unused native VLAN on the trunk link, reducing the risk of VLAN hopping attacks.
●	The second half of the assigned block, 172.30.15.0/24, is left unallocated for future growth.
●	Segmenting CCTV traffic onto VLAN 40 directly satisfies CR7, and allows an access control list (ACL) to be applied at the router sub-interface if traffic between CCTV and other VLANs needs to be restricted further.
4. IP Addressing Plan
The table below details the subnetting of the assigned block 172.30.14.0/23 into four /26 subnets (62 usable hosts each), which comfortably accommodates the current device counts per department with room to grow.
VLAN	Purpose	Subnet (CIDR)	Usable Range	Gateway (Router sub-int)	Broadcast
10	Admin / Office	172.30.14.0/26	172.30.14.1 - .62	172.30.14.1 (Gi0/0.10)	172.30.14.63
20	Production Floor	172.30.14.64/26	172.30.14.65 - .126	172.30.14.65 (Gi0/0.20)	172.30.14.127
30	Sales / Yard	172.30.14.128/26	172.30.14.129 - .190	172.30.14.129 (Gi0/0.30)	172.30.14.191
40	Security / CCTV (CR7)	172.30.14.192/26	172.30.14.193 - .254	172.30.14.193 (Gi0/0.40)	172.30.14.255
99	Native (unused, trunk only)	n/a	n/a	n/a	n/a

Reserved for future use:
Reserved	Future expansion (Wi-Fi, new departments)	172.30.15.0/24	172.30.15.1 - .254	Not yet allocated	172.30.15.255

4.1 Subnetting Justification
●	172.30.14.0/23 provides 512 total addresses. Splitting the first /24 (172.30.14.0/24) into four /26 subnets gives each department up to 62 usable host addresses - sufficient for current staff/device counts and realistic growth.
●	The second /24 (172.30.15.0/24) is deliberately left unallocated, giving the client room to add further VLANs (e.g. guest Wi-Fi) without re-addressing the existing network.
●	Each VLAN's gateway address is the router sub-interface configured with the first usable host address in that subnet (e.g. 172.30.14.1 for VLAN 10).
