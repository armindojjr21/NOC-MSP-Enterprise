# NOC-MSP-Enterprise

Overview

This project simulates a small enterprise/MSP infrastructure using Cisco Packet Tracer.

The environment includes:

Multi-site WAN
VLAN segmentation
Inter-VLAN routing
OSPF dynamic routing
NAT/PAT
Dual ISP architecture
SNMP monitoring
DHCP services
Enterprise switching

```
#VLAN Architecture
 VLAN	Name	       Purpose	               Network
 10	  USERS	       End-user devices	       10.10.10.0/24
 20	  SERVERS	     Infrastructure servers	 10.10.20.0/24
 30	  VOICE	       Voice devices	         10.10.30.0/24
 99	  MANAGEMENT	 NOC/Management	         10.10.99.0/24  

 VLAN Creation
  vlan 10
  name USERS

  vlan 20
  name SERVERS

  vlan 30
  name VOICE

  vlan 99
  name MANAGEMENT
 
#SVI
 interface vlan 10
 ip address 10.10.10.1 255.255.255.0
 no shutdown

 interface vlan 20
 ip address 10.10.20.1 255.255.255.0
 no shutdown

 interface vlan 30
 ip address 10.10.30.1 255.255.255.0
 no shutdown

 interface vlan 99
 ip address 10.10.99.1 255.255.255.0
 no shutdown

#Routed Port to EDGE1
 interface g1/0/1
 no switchport
 ip address 10.255.100.2 255.255.255.252
 no shutdown

#Trunk Ports
 interface g1/0/2
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30,99


##CORE-SW2 Management SVI
 interface vlan 99
 ip address 10.10.99.2 255.255.255.0
 no shutdown
 
 Default Gateway
 ip default-gateway 10.10.99.1
 
 Trunk to CORE-SW1
 interface g0/1
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30,99

##OSPF Configuration

CORE-SW1
router ospf 1
 network 10.10.0.0 0.0.255.255 area 0
 network 10.255.100.0 0.0.0.3 area 0

EDGE1
router ospf 1
 network 10.255.0.0 0.0.255.255 area 0
 default-information originate

Branch Routers
router ospf 1
 network 10.0.0.0 0.255.255.255 area 0

##NAT Configuration
 EDGE1
  NAT ACL
   access-list 1 permit 10.0.0.0 0.255.255.255

 Inside Interface
 interface g0/1
  ip nat inside

 Outside Interface
 interface g0/0
   ip nat outside
   
 PAT/NAT Overload
   ip nat inside source list 1 interface g0/0 overload

 Default Route
   ip route 0.0.0.0 0.0.0.0 200.1.1.1

 CORE-SW1 Default Route
   ip route 0.0.0.0 0.0.0.0 10.255.100.1

##DHCP Architecture
  DHCP was implemented locally in each branch/LAN.

  Each router/server provides IP addressing for its local subnet.

##SNMP Configuration
  snmp-server community NOC-RO RO

##Monitoring Server
Service	  IP
SNMP/NMS	10.10.20.12
SYSLOG	  10.10.20.11
DHCP	    10.10.20.10

##Management Network
 VLAN99
 Used for:
  NOC
  SSH management
  monitoring
  infrastructure access

```
##Enterprise Concepts Implemented

This lab demonstrates:

  Layer 2 switching
  Layer 3 switching
  Inter-VLAN routing
  OSPF routing
  WAN routing
  NAT/PAT
  VLAN segmentation
  SNMP monitoring
  DHCP services
  Trunking
  Routed ports
  Enterprise core design
  Dual-ISP architecture

##Verification Commands
VLANs
  show vlan brief
Trunks
  show interfaces trunk
Routing
  show ip route
OSPF Neighbors
  show ip ospf neighbor
NAT
  show ip nat translations
  show ip nat statistics
SNMP
  show snmp community


##Possible future upgrades:
  HSRP
  GRE VPN tunnels
  IP SLA failover
  ACL security policies
  SSH hardening
  Syslog centralization
  NTP
  SNMPv3
  Zabbix/PRTG integration
  NetFlow monitoring

##Skills Demonstrated
  Cisco networking
  Enterprise switching
  Enterprise routing
  NOC operations
  MSP infrastructure
  WAN technologies
  Network monitoring
  Troubleshooting
  Infrastructure design
  
  

