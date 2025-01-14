# Cisco Packet Tracer Project Documentation

## Author
Juraj Rekeň, II.PDT

## Overview
This project involves configuring a network topology in Cisco Packet Tracer. The goal is to set up VLANs, manage inter-VLAN communication, and implement access control using access lists.

## Steps and Configuration Details

### Step 1: Topology Creation
1. Design the network topology in Cisco Packet Tracer as follows:
   - Add switches, routers, and end devices.
   - Connect devices appropriately.

![image](https://github.com/user-attachments/assets/7bcc989e-4fb4-47cf-9738-cb56934e02f5)


### Step 2: VLAN Configuration
1. Open `Switch0` CLI and configure VLANs 10, 20, and 30 for:
   - Management
   - Workers
   - Cameras
2. Assign VLANs to the appropriate ports for the devices.
3. Repeat the process for `Switch1`.
4. Set trunking between `Switch0` and `Router` and between switches for VLAN communication.

```bash
Switch(config)#vlan 10
Switch(config-vlan)#name management
Switch(config)#vlan 20
Switch(config-vlan)#name workers
Switch(config)#vlan 30
Switch(config-vlan)#name cameras

Switch(config)#int fa0/7
Switch(config-if)#switchport mode trunk
```

### Step 3: Router Subinterface Configuration
1. Configure subinterfaces on the router for each VLAN.
2. Set encapsulation and IP addresses for the default gateways.

```bash
Router(config)#int gig0/0.10
Router(config-subif)#encapsulation dot1Q 10
Router(config-subif)#ip address 10.0.0.1 255.255.255.0
Router(config-subif)#no shutdown

Router(config)#int gig0/0.20
Router(config-subif)#encapsulation dot1Q 20
Router(config-subif)#ip address 20.0.0.1 255.255.255.0
Router(config-subif)#no shutdown

Router(config)#int gig0/0.30
Router(config-subif)#encapsulation dot1Q 30
Router(config-subif)#ip address 30.0.0.1 255.255.255.0
Router(config-subif)#no shutdown
```

### Step 4: Access Control Lists (ACLs)
1. Create ACLs to control inter-VLAN communication.
2. Allow only management VLAN to communicate with others.

```bash
Router(config)#access-list 20 permit 10.0.0.0 0.255.255.255
Router(config)#access-list 30 deny any
Router(config)#interface gig0/0.20
Router(config-if)#ip access-group 20 out
Router(config)#interface gig0/0.30
Router(config-if)#ip access-group 30 out
```

## How It Works
- When sending a message to another device within the same VLAN, the switch routes it internally.
- If the destination is outside the VLAN, the router (default gateway) forwards it to the correct VLAN, checking permissions with the ACLs.

## Physical and Logical Configurations
### Physical Configuration
- Devices and interfaces have been configured with the following:

| Device | Interface | IP Address   | Subnet Mask    | Default Gateway |
|--------|-----------|--------------|----------------|-----------------|
| PC1    | FastEthernet0 | 10.0.0.2 | 255.255.255.0 | 10.0.0.1       |
| PC2    | FastEthernet0 | 20.0.0.2 | 255.255.255.0 | 20.0.0.1       |
| PC3    | FastEthernet0 | 30.0.0.2 | 255.255.255.0 | 30.0.0.1       |

### Logical Configuration
- VLANs:
  - VLAN 10: Management
  - VLAN 20: Workers
  - VLAN 30: Cameras
