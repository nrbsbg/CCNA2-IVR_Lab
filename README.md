Inter-VLAN Routing Lab

Overview

This project demonstrates two different Inter-VLAN Routing methods using Cisco Packet Tracer.

1. Legacy Inter-VLAN Routing

Router interfaces are configured with separate IP addresses for each VLAN.

Each VLAN uses a dedicated physical router interface.

VLAN 10 and VLAN 20 are configured on the switch.

2. Router-on-a-Stick

A single physical router interface is used for multiple VLANs.

Subinterfaces are configured using 802.1Q encapsulation.

Trunk links connect the switches.

VLAN 110 and VLAN 140 are configured.

Technologies Used

Cisco Packet Tracer

Cisco 1941 Router

Cisco 2960 Switch

VLAN

Inter-VLAN Routing

802.1Q Encapsulation

Trunk Configuration

IPv4 Addressing

Testing

Connectivity between different VLANs is tested using ICMP (Ping).

Project File

Inter-VLAN-Routing.pkt


## Configurations
//ESKİ TİP VLAN'LER ARASI YÖNLENDİRME
// SWITCH’de VLAN Oluşturma 
Switch> enable 
Switch# conf t 
Switch(config)# hostname SW2
SW2(config)# vlan 10 
SW2(config-vlan)# name MISAFIR 
SW2(config-vlan)# vlan 20 
SW2(config-vlan)# name CALISANLAR 
SW2(config-vlan)# exit

// Portu VLAN’e Atama (Access Port)
SW2(config)# interface fastEthernet 0/1
SW2(config-if)# switchport mode access 
SW2(config-if)# switchport access vlan 10 
SW2(config)# interface fastEthernet 0/2 
SW2(config-if)# switchport mode access 
SW2(config-if)# switchport access vlan 10 
SW2(config-if)# interface fastEthernet 0/6 
SW2(config-if)# switchport mode access 
SW2(config-if)# switchport access vlan 20 
SW2(config-if)# end 
SW2(config)# interface fastEthernet 0/11 
SW2(config-if)# switchport mode access 
SW2(config-if)# switchport access vlan 20 
SW2# show vlan brief 

// ROUTER Yapılandırması
Router> enable 
Router # conf t 
Router(config)# hostname R2
R2(config)#interface gig 0/0 
R2(config-if)#ip address 199.200.10.1 255.255.255.0 
R2(config-if)#description MISAFIR CIKIS NOKTASI 
R2(config-if)#no shutdown
R2(config-if)# interface gig 0/1 
R2(config-if)# ip address 199.200.20.1 255.255.255.0 
R2(config-if)# description CALISANLAR CIKIS NOKTASI 
R2(config-if)# no shutdown
R2(config-if)# Ctrl+Z
R2# write 
R2(config)#show ip route


// ÇUBUK YÖNLENDİRİCİ İLE VLAN'LER ARASI YÖNLENDİRME

// SWITCH’lerde VLAN Oluşturma (Her 3 Switch için bu komutlar tekrar etmektedir.)
Switch> enable 
Switch# conf t 
Switch(config)# hostname ANAHTAR-X 
//(Her 3 Switch için bu komutlar tekrar etmektedir.)
(config)# vlan 110 
(config-vlan)# name GRUP1 
(config-vlan)# vlan 120 
(config-vlan)# name GRUP2 
(config-vlan)# vlan 130 
(config-vlan)# name GRUP3 
(config-vlan)#vlan 140
(config-vlan)#name GRUP4
(config-vlan)# Ctrl+Z.
# write //Konfigürasyon kaydedildi.
// Portu VLAN’e Atama (Access Port)
ANAHTAR-3(config)#int fa0/1
ANAHTAR-3(config-if)#switchport mode access 
ANAHTAR-3(config-if)#switchport access vlan 110 
// Portu TRUNK Yapma
ANAHTAR-3(config)#int fa0/24
ANAHTAR-3(config-if)#switchport mode trunk
ANAHTAR-3 (config-if)# Ctrl+Z 
ANAHTAR-3# write 
// Diğer Switch’ler’de de benzer işlem yapılır.
ANAHTAR-2(config)#int fa0/1 
ANAHTAR-2(config-if)#switchport mode access 
ANAHTAR-2(config-if)#switchport access vlan 140 
ANAHTAR-2(config-if)#int fa0/23 
ANAHTAR-2(config-if)#switchport mode trunk 
ANAHTAR-2(config-if)#int fa0/24 
ANAHTAR-2(config-if)#switchport mode trunk 
ANAHTAR-2 (config-if)# Ctrl+Z
ANAHTAR-2# write 

ANAHTAR-1(config)#int fa0/1 
ANAHTAR-1(config-if)#switchport mode access 
ANAHTAR-1(config-if)#switchport access vlan 110 

ANAHTAR-1(config-if)#int fa0/23 
ANAHTAR-1(config-if)#switchport mode trunk 
ANAHTAR-1(config-if)#int Gi 0/1 
ANAHTAR-1(config-if)#switchport mode trunk 
ANAHTAR-1 (config-if)# Ctrl+Z 
ANAHTAR-1# write 

// ROUTER Yapılandırması
Router> enable 
Router # conf t
Router(config)# hostname R1
// ROUTER’da Sanal Interface Oluşturma
R1 (config)#interface gig 0/0 
R1 (config-if)#no shutdown
R1(config-if)# interface gig0/0.110
R1(config-subif)# encapsulation dot1q 110 
Sanal interface ile VLAN paket numarası ilişkilendirildi.
R1(config-subif)# description 110
R1(config-subif)# ip address 172.16.110.1 255.255.255.0 
R1(config-subif)# no shutdown
R1(config-if)# interface gig0/0.120 
R1(config-subif)# encapsulation dot1q 120 
R1(config-subif)# description 120 
R1(config-subif)# ip address 172.16.120.1 255.255.255.0 
R1(config-subif)# no shutdown
R1(config-if)# interface gig0/0.130 
R1(config-subif)# encapsulation dot1q 130 
R1(config-subif)# description 130 
R1(config-subif)# ip address 172.16.130.1 255.255.255.0 
R1(config-subif)# no shutdown
R1(config-if)# interface gig0/0.140
R1(config-subif)# encapsulation dot1q 140 
R1(config-subif)# description 140 
R1(config-subif)# ip address 172.16.140.1 255.255.255.0 
R1(config-subif)# no shutdown
R1(config)# end 
R1# copy running-config startup-config
