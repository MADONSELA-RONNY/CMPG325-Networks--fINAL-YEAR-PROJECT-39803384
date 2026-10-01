# CMPG325-Networks--fINAL-YEAR-PROJECT-39803384
A Cisco Packet Tracer network design and implementation project for CMPG 325 (Computer Networks) at NWU. The assigned technical challenge is Router-on-a-Stick (sub-interface inter-VLAN routing), built around addressing block 172.30.14.0/23 and Change Request CR7 (CCTV traffic segmentation).
```text
# CMPG 325 - Computer Networks
**Student Name:** Ronny Madonsela  
**Project ID:** CMPG325-2026-033  
**Client:** Kgosi Brick & Block Works (Taung)  
**Technical Challenge:** Router-on-a-Stick (sub-interface inter-VLAN routing)  

---

## 12. GitHub Portfolio of Evidence

### Portfolio Directory Layout
This repository is organized into the following evidence folders:
*   `01-client-requirements/` – Contains the project brief summary and functional requirement definitions.
*   `02-topology/` – Houses physical and logical network diagram images (`physical-topology.png` and `logical-topology.png`).
*   `03-ip-addressing/` – Contains the complete IP addressing plan table mapped to the 172.30.14.0/23 block.
*   `04-packet-tracer/` – Stores incremental versions of the working Cisco Packet Tracer `.pkt` file.
*   `05-configuration/` – Holds exported device configuration text files for Router0 and Switch0.
*   `06-testing/` – Stores connectivity test evidence screenshots (named `test01-...png` through `test10-...png`).
*   `07-troubleshooting/` – Includes engineering logs, notes on errors encountered, and detailed resolutions.
*   `08-reflection/` – Contains the written project reflection for the final submission.

### Required Project Evidence Breakdown
The portfolio explicitly highlights the implementation of the following design elements:
*   **Client Requirements:** Focused on a lean network that satisfies the load-shedding resilience constraint (core devices on UPS) and minimal device counts.
*   **Network Design:** A single router doing inter-VLAN routing and a single access/trunking switch to optimize hardware footprint.
*   **IP Addressing:** Subnetting the assigned block (`172.30.14.0/23`) into four balanced `/26` subnets supporting 62 usable hosts each.
*   **Topology Diagrams:** Clear visual proof of physical cabling interfaces and logical broadcast boundaries.
*   **Packet Tracer File:** A fully testable `.pkt` file replicating the precise business configuration layout.
*   **Configuration Files:** Verified CLI logs showing running 802.1Q sub-interface assignments (`Gi0/0.10` through `Gi0/0.40`).
*   **Testing Evidence:** Systematic verification logs for intra-VLAN local switching, inter-VLAN path routing, and default gateway pings.
*   **Troubleshooting Logs:** Logged records tracking port isolation debugging, interface tracking, and operational fixes.
*   **Reflection:** Evaluation summary detailing engineering choices and insights gained during deployment.

### Change Request CR7 Checklist
Accommodated security variance configurations to handle added surveillance equipment:
*   **VLAN Separation:** Isolating four CCTV cameras entirely inside **VLAN 40 (Security/CCTV)** to isolate their local broadcast domain.
*   **ACL Application:** Applying an extended Access Control List (`CCTV-SEGMENTATION`) inbound on the **Gi0/0.40** router sub-interface.
*   **Traffic Restraints:** Enforcing explicit rule statements blocking CCTV hardware from initiating connections to Production (`VLAN 20`) or Sales/Yard (`VLAN 30`).
*   **Monitoring Access:** Keeping rule architectures open to explicitly allow the Admin/Office network (`VLAN 10`) to safely reach and watch active video streams.
*
