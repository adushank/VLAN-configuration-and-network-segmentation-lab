# VLAN Configuration & Network Segmentation Lab

An academic Cisco Packet Tracer lab demonstrating how to create VLANs on a Cisco 2960 switch, assign access ports, and observe how VLANs separate Layer 2 traffic. This repository documents the intended configuration and test procedure; it does not claim that the lab or connectivity tests have been performed.

## Objectives

- Create VLAN 10 (Sales) and VLAN 20 (HR) on a Cisco 2960 switch.
- Assign four switch access ports to the correct VLANs.
- Configure IP addresses for four PCs in their respective subnets.
- Test expected connectivity within a VLAN and between VLANs.
- Understand broadcast-domain separation and the role of Layer 3 routing.

## Technologies Used

- Cisco Packet Tracer
- Cisco 2960 Layer 2 switch
- Ethernet switching, VLANs, and IPv4 addressing
- Cisco IOS CLI

## Network Topology

The planned topology contains one Cisco 2960 switch and four PCs. Connect each PC to its assigned switch port using an Ethernet cable. PC0 and PC1 belong to Sales (VLAN 10); PC2 and PC3 belong to HR (VLAN 20). No router or Layer 3 switch is included, so inter-VLAN routing is not available in this lab.

```text
PC0 ── Fa0/1 ┐
PC1 ── Fa0/2 ├── Cisco 2960 switch
PC2 ── Fa0/3 ┤
PC3 ── Fa0/4 ┘
```

## IP Addressing Plan

Configure each address under the PC's **Desktop > IP Configuration** settings. No default gateways are specified because this topology has no router or Layer 3 interface configured.

| PC | IPv4 address | Subnet mask | Switch port | VLAN | Default gateway |
|---|---|---|---|---:|---|
| PC0 | 192.168.10.2 | 255.255.255.0 | Fa0/1 | 10 | Not configured |
| PC1 | 192.168.10.3 | 255.255.255.0 | Fa0/2 | 10 | Not configured |
| PC2 | 192.168.20.2 | 255.255.255.0 | Fa0/3 | 20 | Not configured |
| PC3 | 192.168.20.3 | 255.255.255.0 | Fa0/4 | 20 | Not configured |

## VLAN and Port Plan

| VLAN ID | VLAN name | Access ports | Assigned devices |
|---:|---|---|---|
| 10 | Sales | Fa0/1, Fa0/2 | PC0, PC1 |
| 20 | HR | Fa0/3, Fa0/4 | PC2, PC3 |

## Implementation Procedure

1. In Cisco Packet Tracer, place one Cisco 2960 switch and four PCs.
2. Connect PC0, PC1, PC2, and PC3 to Fa0/1, Fa0/2, Fa0/3, and Fa0/4 respectively.
3. Configure the PC addresses and subnet masks from the IP addressing table. Leave the default gateway blank for this topology.
4. Open the switch CLI and enter the commands in [`vlan_commands.txt`](./vlan_commands.txt).
5. Verify the VLAN and port assignments with `show vlan brief`.
6. Perform the connectivity checks in the procedure below, and record only the results you observe in your actual Packet Tracer lab.

## Cisco IOS Commands

The complete command sequence with explanations is in [`vlan_commands.txt`](./vlan_commands.txt). Its executable commands are:

```text
enable
configure terminal

vlan 10
name Sales
exit

vlan 20
name HR
exit

interface fastEthernet 0/1
switchport mode access
switchport access vlan 10
exit

interface fastEthernet 0/2
switchport mode access
switchport access vlan 10
exit

interface fastEthernet 0/3
switchport mode access
switchport access vlan 20
exit

interface fastEthernet 0/4
switchport mode access
switchport access vlan 20
exit

end
show vlan brief
```

`show vlan brief` should list VLAN 10 as Sales and VLAN 20 as HR, with the access ports shown in the VLAN plan, after the configuration has been entered successfully.

## Connectivity Testing

From each PC, open **Desktop > Command Prompt** and issue the appropriate `ping` command:

| Source | Destination | Command |
|---|---|---|
| PC0 | PC1 | `ping 192.168.10.3` |
| PC2 | PC3 | `ping 192.168.20.3` |
| PC0 | PC2 | `ping 192.168.20.2` |

Repeat the cross-VLAN check in the reverse direction if desired. Capture the actual CLI output for your evidence. This README describes expected behavior; it does not claim these tests have been run.

## Expected Results

- PC0 and PC1 are in the same VLAN and IP subnet; they should be able to ping each other when the cabling, addresses, masks, and switch configuration are correct.
- PC2 and PC3 are in the same VLAN and IP subnet; they should be able to ping each other when correctly configured.
- PC0 should not be able to ping PC2 in this topology without inter-VLAN routing.
- A failed ping alone does not prove VLAN isolation; check the configuration and test both within-VLAN cases as well.

## Network Segmentation and Broadcast Domains

A VLAN is a logical Layer 2 segment. The switch places the Sales ports in VLAN 10 and the HR ports in VLAN 20, separating their Layer 2 broadcast domains even though they use the same physical switch. A Layer 2 broadcast from one VLAN is not forwarded as a broadcast into the other VLAN.



## Project Directory Structure

```text
VLAN-Configuration-Network-Lab/
├── README.md
├── vlan_commands.txt
├── .gitignore
├── Screenshots/
│   └── README.md
└── Documentation/
    └── README.md
```

## Key Learning Outcomes

- Creating and naming VLANs on a Cisco switch.
- Configuring switch ports as access ports and assigning VLAN membership.
- Relating IPv4 subnets and switch VLANs in a small topology.
- Testing same-VLAN connectivity and recognizing the need for Layer 3 routing between VLANs.
- Recording configuration and test evidence accurately.


## Author

**Aditya Shashank**  
B.Tech Information Technology  
Netaji Subhas University of Technology (NSUT)

