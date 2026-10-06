# Secure Hybrid Cloud Network on AWS

## Project Overview
## Objectives
## Architecture
## AWS Network Configuration
## Technologies Used
## Security Configuration
## Testing
## Skills Demonstrated
## Security Notice

# Secure Hybrid Cloud Network on AWS

## Project Overview

This project demonstrates the design and implementation of a secure hybrid cloud network using AWS and WireGuard VPN.

A Windows client securely connects to an AWS VPC through a WireGuard VPN server located in a public subnet. A private EC2 server is hosted in a private subnet without a public IP address and can only be accessed through the VPN connection.

## Objectives

- Design an AWS VPC with public and private subnets
- Deploy a VPN gateway in the public subnet
- Configure WireGuard VPN connectivity
- Deploy a private EC2 server without a public IP address
- Restrict access using AWS Security Groups
- Access the private server through the VPN
- Host a private web service
- Verify that the private service cannot be accessed when the VPN is disabled

## Architecture

Windows Client
10.20.0.2
        |
        | WireGuard VPN
        |
VPN Server
Public Subnet
WireGuard IP: 10.20.0.1
AWS Private IP: 10.0.13.114
        |
        |
Private Server
Private Subnet
10.0.129.118
        |
        |
Private Web Server
TCP Port 80

## AWS Network Configuration

- VPC CIDR: 10.0.0.0/16
- Public Subnet: 10.0.0.0/20
- Private Subnet: 10.0.128.0/20
- WireGuard VPN Network: 10.20.0.0/24

## Technologies Used

- Amazon Web Services (AWS)
- Amazon VPC
- Amazon EC2
- AWS Security Groups
- Ubuntu Linux
- WireGuard VPN
- SSH
- TCP/IP
- Routing
- NAT
- Python HTTP Server
- Windows PowerShell

## Security Configuration

### VPN Server

Inbound access:

- SSH TCP/22 from administrator public IP
- WireGuard UDP/51820
- Source/Destination Check disabled for VPN routing

### Private Server

The private EC2 instance has no public IPv4 address.

Inbound access is restricted through the VPN gateway.

Allowed services:

- SSH TCP/22
- HTTP TCP/80
- ICMP

## Testing

### VPN Connectivity

The Windows client successfully connected to the AWS VPN server using WireGuard.

Client VPN address:

10.20.0.2

VPN gateway:

10.20.0.1

### Private Network Connectivity

The private EC2 instance was successfully reached through the VPN:

10.0.129.118

### SSH Test

SSH access to the private EC2 instance was successfully established through the WireGuard VPN.

### Private Web Server Test

A web server was hosted on the private EC2 instance on TCP port 80.

The website was accessible from the Windows client while the WireGuard VPN was active.

### Security Test

VPN Enabled:
Private website accessible.

VPN Disabled:
Private website inaccessible.

VPN Enabled Again:
Private website accessible again.

This confirms that the private server is protected from direct public access.

## Skills Demonstrated

- AWS VPC design
- Public and private subnet configuration
- EC2 deployment
- Linux administration
- VPN configuration
- WireGuard
- Network routing
- IP addressing and subnetting
- Security Groups
- SSH
- Firewall configuration
- Private cloud resource access
- Network troubleshooting

## Security Notice

No private keys, AWS credentials, PEM files, passwords, or sensitive configuration values are stored in this repository.
