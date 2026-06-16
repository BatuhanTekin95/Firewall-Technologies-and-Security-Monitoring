# Firewall Fundamentals

## Introduction

After completing my SIEM Investigation Case Studies project, I wanted to expand my knowledge into another essential area of cybersecurity: firewall technologies and network security.

Firewalls play a critical role in protecting modern networks by monitoring and controlling incoming and outgoing traffic based on predefined security rules. Acting as a security barrier between trusted internal networks and untrusted external sources, they help prevent unauthorized access, block malicious activity, and enforce organizational security policies.

For SOC analysts, understanding how firewalls operate is fundamental. Firewall logs often provide valuable evidence during security investigations, helping analysts identify suspicious connections, detect scanning activity, investigate potential attacks, and understand network communication patterns.

In this module, I will explore the fundamental concepts of firewall technology, different firewall types, firewall rules, and the role firewalls play in network defense. I will also examine how firewall data can support security monitoring and incident investigations in real-world environments.

By building a strong foundation in firewall technologies, security analysts can better understand network activity and improve their ability to detect and respond to potential threats.

## Types of Firewalls

### Stateless Firewall

Stateless firewalls operate primarily at OSI layers 3 and 4 and evaluate each packet independently against predefined rules. They do not maintain information about previous connections, making them fast and efficient but less capable of applying context-aware security decisions.

### Stateful Firewall

Stateful firewalls monitor active connections and maintain a state table containing information about ongoing sessions. By understanding the context of network traffic, they can make more intelligent filtering decisions and provide stronger security than stateless firewalls.

### Proxy Firewall

Proxy firewalls act as intermediaries between internal users and external resources. Operating at the application layer, they inspect packet contents, hide internal IP addresses, and provide advanced content filtering capabilities.

### Next-Generation Firewall (NGFW)

NGFWs combine traditional firewall functionality with advanced security features such as deep packet inspection, intrusion prevention, application awareness, SSL/TLS inspection, and threat intelligence integration. They are widely used in modern enterprise environments.

| Firewall Type | Key Characteristics |
|--------------|--------------------|
| Stateless | Fast packet filtering, no connection tracking |
| Stateful | Tracks active sessions and connection states |
| Proxy | Inspects application-layer traffic and content |
| NGFW | Deep packet inspection, IPS, application awareness |

## Why Firewalls Matter for SOC Analysts

Firewalls are one of the most important sources of network visibility in modern environments. During security investigations, firewall logs can help analysts identify suspicious connections, detect scanning activity, investigate command-and-control communication, and monitor potential data exfiltration attempts.

Understanding how firewalls operate and how they generate logs enables SOC analysts to perform more effective investigations and respond to security incidents with greater confidence.

## Firewall Rule Components

Every firewall rule is built using several key elements that determine how traffic is handled.

- **Source Address** – The IP address originating the traffic.
- **Destination Address** – The target IP address receiving the traffic.
- **Port** – The network port used for communication.
- **Protocol** – The communication protocol, such as TCP or UDP.
- **Action** – The decision applied to the traffic (Allow, Deny, or Forward).
- **Direction** – Whether the rule applies to inbound or outbound traffic.

  ## Directionality of Rules

Firewall rules can be categorized based on the direction of the network traffic they control. Understanding traffic direction is important when creating security policies and investigating network activity.

### Inbound Rules

Inbound rules apply to traffic entering a device or network. These rules are commonly used to allow or restrict access to services such as web servers, file servers, or remote administration interfaces.

### Outbound Rules

Outbound rules apply to traffic leaving a device or network. Organizations often use outbound filtering to prevent unauthorized communications, restrict applications, and reduce the risk of data exfiltration.

### Forward Rules

Forward rules are used to redirect traffic to another host or network segment. This functionality is commonly used when publishing internal services or forwarding external requests to internal systems.

## Types of Firewall Actions

### Allow

The Allow action permits traffic that matches a firewall rule and enables legitimate communication between systems.

### Deny

The Deny action blocks traffic that matches a firewall rule and prevents the connection from being established.

### Forward

The Forward action redirects traffic to another host or network segment based on the configured firewall policy.

## Windows Defender Firewall

Windows Defender Firewall is the built-in host-based firewall solution included with Microsoft Windows. It allows administrators to create inbound and outbound rules, monitor network traffic, and control access to system resources.

In this section, I explored the Windows Defender Firewall interface and reviewed how firewall rules can be configured to manage network communications.

The Windows Defender Firewall dashboard provides an overview of active network profiles and firewall settings. From this interface, administrators can manage inbound and outbound traffic, adjust security policies, and access advanced firewall configuration options.


<img width="1121" height="586" alt="Ekran görüntüsü 2026-06-16 215816" src="https://github.com/user-attachments/assets/bd90a197-a697-466e-b300-3fa41cbe9b31" />

> Windows Defender Firewall dashboard displaying available network profiles and firewall management options.

## Creating a Custom Firewall Rule

To better understand how firewall policies affect network communications, I created a custom outbound rule designed to block HTTP and HTTPS traffic. This exercise demonstrates how firewall rules can be used to restrict specific services and control network access.

The rule was configured through the Windows Defender Firewall with Advanced Security console, which provides detailed control over inbound and outbound traffic policies.



























