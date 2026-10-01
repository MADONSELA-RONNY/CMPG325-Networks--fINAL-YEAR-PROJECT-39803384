# 07. Engineering Troubleshooting Logs

This document tracks technical anomalies encountered during the development of the Kgosi Brick & Block Works network implementation, alongside the diagnostic commands and operational workflows applied to resolve them.

---

## Technical Hurdle 1: Access Port Misalignment / Unresponsive Host Pings
*   **Symptom:** Initial testing showed that hosts on the Production Floor (`VLAN 20`) were completely unable to ping adjacent computers on the same physical link, failing baseline intra-VLAN pathing.
*   **Diagnostic Methodology:** 
    *   Executed `show vlan brief` on **Switch0** to cross-examine port assignments.
    *   Discovered that interface `FastEthernet0/3` was left incorrectly mapped under default `VLAN 1`, isolating `PC-Prod1`.
*   **Resolution Protocol:** Forced the interface port directly into the explicit access broadcast group:
    ```text
    Switch0# configure terminal
    Switch0(config)# interface FastEthernet0/3
    Switch0(config-if)# switchport mode access
    Switch0(config-if)# switchport access vlan 20
    ```
    *   **Verification:** Re-ran `show vlan brief` to confirm interface grouping showed active mapping alongside port `Fa0/4`.

---

## Technical Hurdle 2: Sub-Interface Operational Drop / Gateway Timeouts
*   **Symptom:** Host workstations could ping local link peers but experienced total packet loss when executing `ping` commands to their default gateways (e.g., `172.30.14.1`). Inter-VLAN routing was entirely inactive.
*   **Diagnostic Methodology:**
    *   Executed `show ip interface brief` on **Router0**.
    *   Identified that while logical sub-interfaces `Gi0/0.10` through `Gi0/0.40` were defined, the main parent interface link status was flagged as `Administratively Down`.
*   **Resolution Protocol:** Triggered a physical interface power override:
    ```text
    Router0# configure terminal
    Router0(config)# interface GigabitEthernet0/0
    Router0(config-if)# no shutdown
    ```
    *   **Verification:** Verified status logs using `show ip interface brief` until all logical interfaces turned to an active `up/up` operational state.

---

## Technical Hurdle 3: Security Leakage / CCTV Traffic Escaping Constraints
*   **Symptom:** During initial `CR7` validation runs, surveillance devices on `VLAN 40` could successfully ping and reach the Production Floor (`VLAN 20`) and Sales/Yard (`VLAN 30`) spaces, violating separation constraints.
*   **Diagnostic Methodology:**
    *   Executed `show running-config` on **Router0**.
    *   Confirmed the extended access list `CCTV-SEGMENTATION` was properly configured with explicit drop actions, but omitted the critical interface application trigger.
*   **Resolution Protocol:** Bound the filtering matrix group inbound onto the security sub-interface:
    ```text
    Router0# configure terminal
    Router0(config)# interface GigabitEthernet0/0.40
    Router0(config-subif)# ip access-group CCTV-SEGMENTATION in
    ```
    *   **Verification:** Executed `show access-lists` following targeted ping drops from `CCTV1` to confirm matching counter indices incremented continuously.
