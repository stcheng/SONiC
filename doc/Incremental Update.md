# SONiC Incremental Update
# High Level Design Document
### Rev 0.1

# Table of Contents

##### Revision
| Rev |  Date   |       Author       | Change Description |
|:---:|:--------|:------------------:|--------------------|
| 0.1 | 2018-09 |   Shuotian Cheng   | Initial Version    |

# About
This document provides the general information about basic SONiC incremental configuration support including IP addresses configuration changes, and port channel configuration changes.

# Definitions/Abbreviations

# 1. Requirements Overview
## 1.1 Functional Requirements
#### Phase #0
- Should be able to boot directly into working state given a working minigraph
1. All IPs are assigned correctly to each router interfaces
2. All port channel interfaces are created with correct members enslaved
3. All VLAN interfaces are created with correct members enslaved
4. All configured ports are set to admin status UP
5. All configured ports are set to desired MTU
#### Phase #1
- Should be able to use command line to execute incremental updates including:
1. Bring up/down all ports/port channels/VLANs
2. Assign/remove IPs towards non-LAG-member/non-VLAN-member front panel ports, and port channels
3. Create/remove port channels
4. Add/remove members of port channels
- Should be able to restart docker swss and the system recovers to the state before the restart
*Note:*
1. *Conflicting configurations that cannot be directly resolved are **NOT** supported in this phrase, including: moving a port with IP into a port channel; assign an IP to a port channel member; adding/removing/manipulating non-existing ports, etc.*
2. *Port channel and port channel members' admin status are controlled separately, indicating that a port channel's admin status DOWN will NOT affect its members' admin status to be brought down as well.*
3. *Admin status and MTU are must have attributes for ports and port channels, and the default values are UP and 9100.*
4. *MTU will be changed to the port channel's MTU once a port is enslaved into the port channel. However, the value will be automatically reset to its original one after the port is removed from the port channel.*
#### Phase #2
- Should be able to restart docker teamd and all port channel configurations are reapplied
*Note:*
*The reason of moving this request into phase 2 is due to unrelated issues encountered while removing and recreating router interfaces, including IPv6 removal and potential SAI implementation issues.*

#### Future Work
## 1.2 Orchagent Requirements
The gap that orchagent daemon needs to fill is mostly related to MTU:
- Should be able to change router interface MTU
- Should be able to change LAG MTU

## 1.3 \*-syncd Requirements
- `portsyncd`: Should not be listening to netlink message to get port admin status and MTU

## 1.4 \*-mgrd Requirements
- `portmgrd`: Should be responsible for 
- `intfsmgrd`:
- `teammgrd`:

## 1.4 Utility Requirements

# 2. Database Design
## 2.1 CONF_DB

## 2.2 APPL_DB

# 3. Daemon Design
## 3.1 orchagent
## 3.2 portsyncd
## 3.3 intfsyncd
## 3.4 teamsyncd

# 4. Flows
## 4.1 IP Assignment on System Start
## 4.2 Port Channel Creation on System Start
## 4.3 CLI on IP Assignment
## 4.4 CLI on Port Channel Manipulation
## 4.5 Docker swss Restart
## 4.6 Docker teamd Restart
