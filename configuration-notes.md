Configuration & Technical Notes
1. VLAN Configuration

The network uses VLANs to logically segment the enterprise network.

VLAN	Purpose	Network	Gateway
10	Management	192.168.10.0/24	192.168.10.1
20	HR	192.168.20.0/24	192.168.20.1
30	Branch Network	192.168.30.0/24	192.168.30.1
40	Branch Network	192.168.40.0/24	192.168.40.1
50	Branch Network	192.168.50.0/24	192.168.50.1
60	Branch Network	192.168.60.0/24	192.168.60.1

Basic VLAN configuration uses vlan <number> followed by name <name>.

Verification command:

show vlan brief

2. Access Ports

PC-facing interfaces were configured as access ports and assigned to their respective VLANs.

The basic configuration uses:

switchport mode access

switchport access vlan <vlan-id>

Access ports carry traffic for a single VLAN.

3. 802.1Q Trunking

Trunk links were configured between infrastructure devices to carry multiple VLANs across a single physical link.

Key configuration:

switchport trunk encapsulation dot1q

switchport mode trunk

Verification:

show interfaces trunk

Key concept

Access Port → One VLAN

Trunk Port → Multiple VLANs

4. EtherChannel

Three EtherChannel methods were implemented to understand different link aggregation mechanisms.

PAgP

PAgP was used for negotiated EtherChannel formation.

Modes:

Desirable
Auto
LACP

LACP was used for standards-based EtherChannel negotiation.

Modes:

Active
Passive
Static EtherChannel

Static EtherChannel was configured using channel-group <number> mode on.

The resulting Port-Channel was configured as a trunk where required.

Verification:

show etherchannel summary

A successful bundled interface appears with the P flag.

5. Router-on-a-Stick

Router-on-a-Stick was implemented to provide inter-VLAN routing using router subinterfaces.

The physical interface was enabled with no shutdown.

Each VLAN was assigned a corresponding router subinterface.

Examples:

G0/0.10 → VLAN 10 → 192.168.10.1
G0/0.20 → VLAN 20 → 192.168.20.1
G0/0.30 → VLAN 30 → 192.168.30.1
G0/0.40 → VLAN 40 → 192.168.40.1
G0/0.50 → VLAN 50 → 192.168.50.1
G0/0.60 → VLAN 60 → 192.168.60.1

The subinterfaces use encapsulation dot1q <vlan-id>.

Memory Rule

Subinterface number = VLAN number

For example:

G0/0.10 → VLAN 10

G0/0.20 → VLAN 20

Verification:

show ip interface brief

6. Inter-Router Connectivity

Point-to-point /30 networks were used between the routers.

HQ ↔ B-1

Network: 10.10.10.0/30

HQ: 10.10.10.1
B-1: 10.10.10.2
B-1 ↔ B-2

Network: 10.10.20.0/30

B-1: 10.10.20.1
B-2: 10.10.20.2

A /30 network provides two usable host addresses.

Memory Rule

Network → .0

First usable → .1

Second usable → .2

Broadcast → .3

7. CDP

Cisco Discovery Protocol was used to identify directly connected network devices and verify physical connections.

Verification command:

show cdp neighbors

CDP helped identify:

Connected routers
Connected switches
Local interfaces
Neighbor interfaces
Memory Rule

CDP = Who is directly connected to me?

8. Static Routing

Static routes were configured to provide connectivity between remote VLAN networks.

Command structure:

ip route <destination-network> <subnet-mask> <next-hop>

The routing logic is:

Destination Network → Next-Hop Router

Verification:

show ip route

Static routes are identified by the S code.

9. DHCP

DHCP was configured on the routers for the VLAN networks.

The first 20 IP addresses of each subnet were excluded from DHCP allocation.

Example:

192.168.10.1–192.168.10.20 → Reserved

192.168.10.21 onward → DHCP clients

The DHCP pool provides:

Network address
Default gateway
DNS server

Configured DNS:

8.8.8.8

Verification:

show ip dhcp binding

DHCP was successfully tested with a client receiving a dynamically assigned address.

10. Telnet

Telnet was configured for remote management of the routers.

The VTY lines were configured with authentication and Telnet transport.

Configuration included:

line vty 0 4

password cisco

login

transport input telnet

Telnet connectivity was successfully tested against:

HQ
B-1
B-2
11. Port Security

Port Security was configured on PC-facing access ports.

The security configuration uses:

Maximum MAC addresses: 1
Sticky MAC learning
Violation mode: Shutdown
Configuration logic

Port Security

Enables Layer 2 port security.

Maximum 1

Allows only one MAC address on the port.

Sticky MAC

Learns the connected device MAC address and adds it as a secure MAC address.

Shutdown

Places the port into a shutdown or error-disabled state if an unauthorized MAC address causes a violation.

Verification

show port-security

show port-security interface <interface>

show port-security address

Successful verification showed:

Maximum secure MAC addresses: 1
Current secure MAC addresses: 1
Security violations: 0
12. Important Verification Commands
Purpose	Command
VLANs	show vlan brief
Trunks	show interfaces trunk
EtherChannel	show etherchannel summary
Neighbors	show cdp neighbors
Interfaces	show ip interface brief
Routing	show ip route
DHCP	show ip dhcp binding
Port Security	show port-security
Port Security Details	show port-security interface <interface>
Secure MACs	show port-security address
13. Troubleshooting Lessons
Trunk Encapsulation Error

If switchport mode trunk is rejected because the encapsulation is set to Auto, configure 802.1Q encapsulation first.

switchport trunk encapsulation dot1q

switchport mode trunk

Router Interface Down

If the physical router interface is administratively down:

interface g0/0

no shutdown

Router subinterfaces depend on the physical interface being operational.

Static Routing

Always identify:

Destination network → Next-hop router

The next hop should be the neighboring router through which the destination network can be reached.

EtherChannel

PAgP and LACP determine how physical links are negotiated and bundled.

Trunking determines what VLAN traffic the resulting link carries.

EtherChannel ≠ Trunking

They are separate concepts that can be used together.

Port Security

Port Security should be applied to PC-facing access ports.

It should not be applied blindly to infrastructure trunks or EtherChannel links.

14. Final Verification

The completed network was tested using:

VLAN verification
Trunk verification
EtherChannel verification
CDP neighbor verification
Interface status verification
Static route verification
DHCP binding verification
Port Security verification
ICMP connectivity testing
Telnet connectivity testing

The network successfully demonstrated communication between VLANs and across the three simulated locations.

15. Final Result

The project successfully implemented a multi-site enterprise network containing:

VLAN segmentation
Access ports
802.1Q trunking
PAgP EtherChannel
LACP EtherChannel
Static EtherChannel
Router-on-a-Stick
Inter-VLAN routing
Point-to-point router connectivity
Static routing
DHCP
DNS configuration
Telnet remote management
Layer 2 Port Security

The project was independently designed and tested in Cisco Packet Tracer as a hands-on networking and cybersecurity exercise.
