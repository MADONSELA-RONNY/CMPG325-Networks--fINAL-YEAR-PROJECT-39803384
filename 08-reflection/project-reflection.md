# 08. Written Project Reflection

**Module:** CMPG 325 - Computer Networks  
**Project Scope:** Network Architecture and Implementation for Kgosi Brick & Block Works (Taung)  
**Technical Focus:** Router-on-a-Stick (802.1Q Inter-VLAN Routing) & Change Request CR7 Security Filtering  

---

## 1. Technical Evaluation & Architecture Validation
The architectural design chosen for this project focused on balancing explicit technical challenges against a strict hardware constraint. The client required load-shedding resilience while preferring a minimal device footprint to limit the cost and physical space required for Uninterruptible Power Supply (UPS) battery backup. 

Implementing the **Router-on-a-Stick (RoaS)** topology successfully satisfied both parameters. By configuring logical sub-interfaces (`Gi0/0.10` through `Gi0/0.40`) on a single physical link running 802.1Q encapsulation, the entire network core was consolidated into a single router and a single Layer-2 access switch. This choice avoided the higher cost and complexity of introducing an independent Layer-3 switch or multi-interface routing hardware, meaning only two core assets required critical UPS line integration.

---

## 2. Security Mitigation & Change Request (CR7) Reflection
The sudden inclusion of Change Request CR7 (four surveillance CCTV cameras) introduced a crucial traffic segregation challenge. Surveillance systems generate consistent, high-volume traffic that can degrade business network paths if left unmanaged. Additionally, security systems represent a common vector for external intrusion if they can touch sensitive business zones freely.

Isolating the camera systems entirely inside a dedicated broadcast domain (**VLAN 40**) effectively eliminated cross-department broadcast pollution. However, logical VLAN separation alone does not stop inter-VLAN routing paths. To enforce true structural isolation, a rule system was established via an extended Access Control List (`CCTV-SEGMENTATION`). 

By applying the filtering system inbound on the `Gi0/0.40` sub-interface, traffic sourced from the surveillance hardware was systematically screened at the closest routing boundary. This design choice successfully restricted the cameras from initiating connections into the sensitive Production (`VLAN 20`) or Sales/Yard (`VLAN 30`) networks, while keeping the path open for the Admin/Office space (`VLAN 10`) to poll and display streaming footage.

---

## 3. Engineering Insights & Operational Growth
Developing this project from the initial Milestone 1 planning stage through to final implementation provided valuable hands-on experience with modern networking principles:

*   **Subnetting Precision:** Working with a tight `/23` block distribution forced absolute precision when allocating `/26` variable-length subnet masks (VLSM) to ensure maximum host capacity without overlaps.
*   **Logical vs. Physical Topologies:** Troubleshooting simple interface drop conditions reinforced the lesson that a network configuration is only as reliable as its baseline layer infrastructure.
*   **Incremental Portfolio Practices:** Maintaining an organized, multi-folder GitHub layout using clear commit messages emphasized how essential version control and transparent change logs are within professional enterprise engineering teams.
*
