# Secure Hybrid Cloud Network on AWS

## Project Overview

This project demonstrates the design and implementation of a **secure hybrid cloud network on AWS** using Amazon VPC, EC2, Ubuntu Linux, Security Groups, and WireGuard VPN.

A Windows client securely connects to an AWS network through an encrypted WireGuard VPN tunnel. The VPN server is deployed in a public subnet, while the application server is deployed in a private subnet without a public IP address.

The private EC2 server can be accessed through SSH and HTTP only through the VPN path.

---

## Project Objectives

- Design an AWS VPC with public and private subnets
- Deploy EC2 instances in separate network segments
- Configure a WireGuard VPN server on Ubuntu
- Establish an encrypted VPN tunnel from a Windows client
- Configure IP forwarding and secure routing
- Protect cloud resources using AWS Security Groups
- Deploy an EC2 instance without a public IP address
- Access the private server through SSH over the VPN
- Host a private web service
- Verify that the private service becomes inaccessible when the VPN is disabled

---

## Architecture

![Secure Hybrid Cloud Network Architecture](architecture/architecture-diagram.png)

The editable Draw.io source file is also included:

`architecture/secure-hybrid-cloud-network-aws.drawio`

---

## Network Architecture

```text
Windows Laptop
WireGuard Client
10.20.0.2/24
       |
       | Encrypted WireGuard VPN
       | UDP 51820
       |
       v
Internet
       |
       v
AWS VPC - 10.0.0.0/16
       |
       +---------------- Public Subnet
       |                 10.0.0.0/20
       |                       |
       |                  VPN Server
       |                  Ubuntu EC2
       |                  10.0.13.114
       |                  WG: 10.20.0.1
       |                       |
       |                       | Routed Private Traffic
       |                       |
       +---------------- Private Subnet
                         10.0.128.0/20
                               |
                         Private Server
                         Ubuntu EC2
                         10.0.129.118
                         No Public IP
                               |
                         HTTP :80 / SSH :22
```

---

## AWS Network Configuration

| Component | Configuration |
|---|---|
| VPC | `10.0.0.0/16` |
| Public Subnet | `10.0.0.0/20` |
| Private Subnet | `10.0.128.0/20` |
| WireGuard VPN Network | `10.20.0.0/24` |
| VPN Server Private IP | `10.0.13.114` |
| VPN Server WireGuard IP | `10.20.0.1` |
| Windows VPN Client | `10.20.0.2` |
| Private Server | `10.0.129.118` |

---

## Technologies Used

- Amazon Web Services (AWS)
- Amazon VPC
- Amazon EC2
- AWS Security Groups
- Ubuntu Linux
- WireGuard VPN
- SSH
- TCP/IP
- IP Routing
- NAT / IP Masquerading
- Linux iptables
- Windows PowerShell
- Python HTTP Server
- Draw.io

---

## Security Design

### VPN Server

The VPN server is located inside the public subnet and provides the secure entry point into the AWS private network.

Configured controls include:

- WireGuard UDP port `51820`
- SSH TCP port `22` restricted to the administrator IP
- Linux IP forwarding enabled
- EC2 Source/Destination Check disabled for routing
- WireGuard encrypted tunnel
- Routing between VPN clients and private AWS resources

### Private Server

The private server is deployed inside the private subnet.

Security characteristics:

- No public IPv4 address
- No direct public Internet access
- SSH access restricted through the VPN gateway
- HTTP access restricted through the VPN gateway
- ICMP access restricted through the VPN gateway

---

## WireGuard VPN

The Windows computer acts as the WireGuard client.

VPN client address:

```text
10.20.0.2/24
```

VPN server address:

```text
10.20.0.1/24
```

The tunnel provides secure access from the local Windows machine to resources inside the AWS private network.

---

## Project Screenshots

### 1. AWS VPC Overview

![AWS VPC Overview](screenshots/01a-vpc-overview.png)

### 2. AWS Subnet Details

![AWS Subnet Details](screenshots/01b-vpc-subnet-details.png)

The AWS environment contains separate public and private subnets to isolate Internet-facing resources from protected internal resources.

---

### 3. EC2 Instances Overview

![EC2 Instances Overview](screenshots/02a-ec2-instances-overview.png)

### 4. EC2 Instance Details

![EC2 Instance Details](screenshots/02b-ec2-instance-details.png)

Two EC2 instances were deployed:

- VPN Server in the public subnet
- Private Server in the private subnet

The private server does not have a public IPv4 address.

---

### 5. WireGuard VPN Connection

![WireGuard VPN Active](screenshots/03-wireguard-active.png)

The Windows WireGuard client successfully establishes an encrypted tunnel to the AWS VPN server.

---

### 6. Private Server Connectivity Test

![Private Server Ping](screenshots/04-private-server-ping.png)

The private EC2 instance was successfully reached from the Windows computer through the WireGuard VPN.

```text
Windows Client
      ↓
WireGuard VPN
      ↓
VPN Server
      ↓
Private EC2
```

---

### 7. SSH Access to Private EC2

![Private Server SSH](screenshots/05-private-server-ssh.png)

SSH access to the private EC2 instance was successfully established using its private IP address.

This demonstrates secure administrative access without exposing SSH directly to the public Internet.

---

### 8. Private Web Server

![Private Web Server](screenshots/06-private-web-page.png)

A private HTTP service was hosted on the private EC2 instance using TCP port `80`.

The service was accessible from the Windows client while the WireGuard VPN tunnel was active.

---

## Security Validation

The project was tested by enabling and disabling the WireGuard tunnel.

### VPN Enabled

```text
Windows Client
      ↓
Encrypted VPN
      ↓
AWS VPN Server
      ↓
Private Server

Result: ACCESS SUCCESSFUL
```

The private server responded to:

```text
Ping
SSH
HTTP
```

### VPN Disabled

When the WireGuard VPN was disabled, the private web server could no longer be reached.

```text
VPN OFF
   ↓
Private Server
   ↓
ACCESS FAILED
```

### VPN Enabled Again

After reactivating WireGuard, access to the private server and web application was restored.

This demonstrates that the private workload is protected from direct public access.

---

## Testing Performed

| Test | Result |
|---|---|
| WireGuard VPN handshake | ✅ Successful |
| VPN server connectivity | ✅ Successful |
| Private EC2 ping through VPN | ✅ Successful |
| SSH to private EC2 through VPN | ✅ Successful |
| Private HTTP service through VPN | ✅ Successful |
| HTTP access with VPN disabled | ❌ Blocked as expected |
| HTTP access after VPN reactivation | ✅ Successful |

---

## Skills Demonstrated

This project demonstrates practical knowledge of:

- AWS VPC architecture
- Public and private subnet design
- Amazon EC2
- AWS Security Groups
- Cloud network segmentation
- Linux administration
- WireGuard VPN configuration
- Secure remote access
- TCP/IP networking
- CIDR and subnetting
- IP forwarding
- NAT and masquerading
- Linux firewall configuration
- SSH administration
- Private cloud resource access
- Network troubleshooting
- Security testing

---

## Key Learning Outcomes

Through this project, I gained practical experience designing and troubleshooting a secure cloud network.

I learned how VPN tunnels, routing, Security Groups, public/private subnet separation, Linux networking, and AWS EC2 work together to provide secure access to private cloud resources.

I also gained experience troubleshooting connectivity issues caused by routing, firewall rules, VPN configuration, and changes in client network connections.

---

## Repository Structure

```text
secure-hybrid-cloud-network-aws/
│
├── README.md
├── .gitignore
│
├── architecture/
│   ├── architecture-diagram.png
│   └── secure-hybrid-cloud-network-aws.drawio
│
├── screenshots/
│   ├── 01a-vpc-overview.png
│   ├── 01b-vpc-subnet-details.png
│   ├── 02a-ec2-instances-overview.png
│   ├── 02b-ec2-instance-details.png
│   ├── 03-wireguard-active.png
│   ├── 04-private-server-ping.png
│   ├── 05-private-server-ssh.png
│   └── 06-private-web-page.png
│
└── documentation/
    └── README.md
```

---

## Security Notice

Sensitive information is intentionally excluded from this repository.

The following files and information must never be committed:

```text
AWS access keys
AWS secret keys
SSH private keys
.pem files
WireGuard private keys
Passwords
Environment secrets
Credential files
```

Only public keys, architecture information, sanitized configurations, and project evidence should be stored in this repository.

---

## Future Improvements

Possible future improvements include:

- Infrastructure as Code using Terraform
- AWS CloudWatch monitoring
- VPC Flow Logs
- Automated VPN deployment
- DNS configuration
- HTTPS/TLS for internal services
- Multi-AZ architecture
- AWS managed Site-to-Site VPN
- Network monitoring and alerting
- Centralized security logging

---

## Project Status

**Completed ✅**

The secure hybrid cloud network was successfully deployed and tested with private server access available through an encrypted WireGuard VPN connection.
