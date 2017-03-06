---
layout: post
title: "TCP/IP Basics"
excerpt: ""
categories: knownotes
tags: [computer-network]
comments: true
share: true
author: chungyu
---

> * [Udemy Course: TCP/IP Training Video A Definitive & Easy To Follow Course](https://www.udemy.com/tcp-ip-training/learn/v4/overview)

# History
* TCP/IP becomes predominant protocol in early 80s
* Not till late 1990s, do we have the Internet today

# OSI Model
![confintervalexp1]({{site.url}}/images/network/osimodel.png)
* Open Systems Interconnection
* 7 layers model
* Created in the 1970s as an architecture for distributed database system.
* Adopted by the ISO as a standard communication architecture.
* One layer speaks to another adjacent layer
  * When sending to upper layer, each layer adds headers (**encapsulation**)
  * When receiving from upper layer, each layer is responsible for peeling that layer off from the other end

### 1) Physical layer
* Handles all physical definitions -- connection types (coax, 4-pair, fiber, etc)
* Handles error correction and detection on the physical medium.
  * collision: two electrical signal collides in the physical medium

### 2) Data link layers
* Handles translation from digital data to physical signals
  * Framing the data into forms that could be transmitted through the underlying media (coax/4-pair/fiber/etc...)
* Physical addressing (MAC, Media Access Control)
* Flow & access control
  * Ex: solving the collision through CSMA/CD on the ethernet
    * retransmission through a random holdup /back-off timer

### 3) Network layer
* Handles getting data from one network to another (routing)
  * Compare: Data link layer focus on the data within the same network
* Since it's handling data from different network
  * it required logical addressing to ensure reachability from one network to another
    * IP/ICMP/IPX are in layer 3!
  * it does repackaging if two networks using different physical connection

### 4) Transport layer
* Handles end-to-end, reliable data service
* In the case of TCP/IP, handles ports to differentiate traffic from different applications
* That's how we handle different data streams in the same wire

### 5) Session layer
* Controls specific dialogs between two systems (session management)
  * The management of communications between two systems, it's actually through setting up a session as systems exchanging data back and forth.
* A bit more abstract than lower layers
* Sometimes disagreement over which protocols land here (SSL/TLS, SSH, etc)
  * Sometimes there are overlapped functionality on different protocols

### 6) Presentation layer
* Manages how data is presented

### 7) Application layer
* Closest to the user

# TCP/IP Model
![confintervalexp1]({{site.url}}/images/network/tcpipmodel.png)
* [IETF](https://www.ietf.org/) manages architecture and documentation
* 4 layers model

### 1) Network Access layer
* Physical + data link layer in the OSI model
* Handles physical connection, error correction and control on the physical medium
* Specifies how messages are translated onto the physical medium (electrical or optical) and what that looks like. (What framing should look like)

### 2) Internet layer
* Network layer in the OSI model
* IP, ICMP intended for this layer
* Processing rules to ensure datagrams end up when they are supposed to, although different datagrams may require reassembly because they could end up to be fragmented

### 3) Transport layer
* TCP UDP are transport layer protocols
* Handle end-to-end delivery
  * UDP doesn't care whether datagrams arrive or notes
  * TCP handles connection-oriented communication

### 4) Application layer
* Session, presentation, and application in OSI model
* All application functionality
* Session Management
* Presentation
* APIs


# UNICAST/BROADCAST package
* UNICAST: the package will only give to one machine
* BROADCAST: any device on the network system would get the package

# Protocols
* Rules/Guideline for communication: how you engage somebody else
  * We learned it over time
  * the rule need to be written down
* Helps everyone stay on same page

# One of Layer 2 Address: MAC
* MAC: Media Access Control
  * Physical address that bounds to the physical network interface card
* structure example: `f8:1e:df:e2:4d:bd`
  * 48 bytes
  * First 3 8 byte (ex: `f8:1e:df`) will map to a vender (Organization Unique Identifiers)
  * Last 3 bytes (ex: `e2:4d:bd`) are the unique address that is related to the interface card
* Why not just use IP address?
  * We need an address that is abstract from IP address so that we can use different network protocol on top of the ethernet
  * This setup protect layer 3 protocol from implementing all the layer 2 pieces
* Bluetooth, fiber, ATM also use MAC address

# Layer 2 Protocol: PPP
* PPP: Point to point protocol
* Successor of SLIP (serial line IP)
* A protocol that can set up connection between two devices quickly and easily also can encapsulated higher layer protocol information
* Mainly use for the authentication between devices
* PPPoE: PPP over Ethernet in order to facilitate DSL working\

# WAN: Wide Area Network Protocol (Layer 2 )
* Any network providers use this really large connection pipes/tubes in order to transmit data
* For these large network, it surely needs its own protocol to get data back and forth
