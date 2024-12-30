# Vlan_on_MultiLayer_Switches

# Network Configuration Instructions

This document provides step-by-step instructions for configuring the network topology shown in the provided Cisco Packet Tracer file. The configuration includes hostname setup, VLAN configuration, trunking, inter-VLAN routing, and static routes.

---

## 1. Hostname and Banner Configuration

### Switch Configuration:
1. Enter privileged mode:
   ```
   Switch>enable
   ```
2. Enter global configuration mode:
   ```
   Switch#configure terminal
   ```
3. Set the hostname:
   ```
   Switch(config)#hostname MLS1
   ```
4. Configure a Message of the Day (MOTD) banner:
   ```
   MLS1(config)#banner motd #Cyber Quince#
   ```

---

## 2. Access Interfaces and VLAN Assignment

### Switch Interface Configuration:
1. Configure interface FastEthernet 0/1 on S1:
   ```
   S1(config)#interface FastEthernet 0/1
   S1(config-if)#switchport mode access
   S1(config-if)#switchport access vlan 10
   ```
2. Configure interfaces on S2:
   - Interface FastEthernet 0/1:
     ```
     S2(config)#interface FastEthernet 0/1
     S2(config-if)#switchport mode access
     S2(config-if)#switchport access vlan 10
     ```
   - Interface FastEthernet 0/2:
     ```
     S2(config)#interface FastEthernet 0/2
     S2(config-if)#switchport mode access
     S2(config-if)#switchport access vlan 20
     ```

---

## 3. Configuring Trunks

### Switch Trunk Ports:
1. Configure trunk on GigabitEthernet 0/1 of S1:
   ```
   S1(config)#interface GigabitEthernet 0/1
   S1(config-if)#switchport mode trunk
   ```
2. Configure trunk on MLS1:
   ```
   MLS1(config)#interface GigabitEthernet 0/1
   MLS1(config-if)#switchport trunk encapsulation dot1Q
   MLS1(config-if)#switchport mode trunk
   ```

---

## 4. VLAN Configuration

### VLAN Creation:
1. Create VLANs on MLS1:
   ```
   MLS1(config)#vlan 10
   MLS1(config)#vlan 20
   ```
2. Create VLANs on MLS2:
   ```
   MLS2(config)#vlan 30
   MLS2(config)#vlan 40
   ```
3. Additional VLANs:
   - VLAN 20 on S1:
     ```
     S1(config)#vlan 20
     ```
   - VLAN 40 on S4:
     ```
     S4(config)#vlan 40
     ```

---

## 5. Configuring Gateway for VLANs

### VLAN Interface Configuration:
1. On MLS1:
   - VLAN 10:
     ```
     MLS1(config)#interface VLAN 10
     MLS1(config)#ip address 192.168.10.1 255.255.255.0
     ```
   - VLAN 20:
     ```
     MLS1(config)#interface VLAN 20
     MLS1(config)#ip address 192.168.20.1 255.255.255.0
     ```
2. On MLS2:
   - VLAN 30:
     ```
     MLS2(config)#interface VLAN 30
     MLS2(config)#ip address 192.168.30.1 255.255.255.0
     ```
   - VLAN 40:
     ```
     MLS2(config)#interface VLAN 40
     MLS2(config)#ip address 192.168.40.1 255.255.255.0
     ```

### Enable Routing:
1. On both MLS1 and MLS2:
   ```
   MLS1(config)#ip routing
   MLS2(config)#ip routing
   ```

---

## 6. Interconnecting MLS1 and MLS2

### Link Configuration:
1. On MLS1:
   ```
   MLS1(config)#interface GigabitEthernet 0/2
   MLS1(config-if)#no switchport
   MLS1(config-if)#ip address 192.168.100.1 255.255.255.0
   ```
2. On MLS2:
   ```
   MLS2(config)#interface GigabitEthernet 0/2
   MLS2(config-if)#no switchport
   MLS2(config-if)#ip address 192.168.100.2 255.255.255.0
   ```

---

## 7. Static Routing Configuration

### On MLS1:
1. Add routes to reach VLANs on MLS2:
   ```
   MLS1(config)#ip route 192.168.30.0 255.255.255.0 192.168.100.2
   MLS1(config)#ip route 192.168.40.0 255.255.255.0 192.168.100.2
   ```

### On MLS2:
1. Add routes to reach VLANs on MLS1:
   ```
   MLS2(config)#ip route 192.168.10.0 255.255.255.0 192.168.100.1
   MLS2(config)#ip route 192.168.20.0 255.255.255.0 192.168.100.1
   ```

---

## Notes

- Ensure all devices are correctly cabled as shown in the topology.
- Verify connectivity using `ping` commands between PCs across VLANs to test routing.
- Save the configuration on all devices with the `write memory` command.

--- 

This concludes the network configuration guide.
