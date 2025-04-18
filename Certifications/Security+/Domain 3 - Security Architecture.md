# Cybersecurity Notes

## Virtualization and Cloud Security

### VM Escape Attack
- Attackers attempt to break out of virtual machines (VMs) to access the host system or other VMs.

### VM Sprawl
- Unused and abandoned servers on the network that may not be properly maintained.

### Virtual Desktop Infrastructure (VDI)
- Provides network-based access to a desktop computing environment.

### Application Virtualization
- Allows access to applications running in different environments from your systems.

### Cloud Storage Options
- **Block Storage**: Allocates large chunks of storage partitioned into volumes; more expensive as you pay for all storage on the drive.
- **Object Storage**: Stores files as individual objects, managed by cloud providers; less expensive as you pay only for the storage you use.

### Key Implementations of Cloud Storage Security
- Set permissions properly.
- Use encryption for sensitive data.
- Replicate data to multiple data centers for backup.

## Cloud Networking and Services

### Virtual Private Cloud (VPC)
- Different clouds for different classes of servers.

### VPC Endpoints
- Secure connections that allow resources inside your cloud network (VPC) to connect to other services (e.g., AWS S3, DynamoDB) without using the public internet.

### Software-Defined Networking (SDN)
- Separates network control from hardware, allowing administrators to manage the entire network through software.

### Software-Defined Visibility (SDV)
- A method of monitoring cloud network traffic using cloud provider APIs, enabling visibility into data flow without physical access to hardware.

### Cloud Orchestration
- Automated management of cloud resources like servers, storage, and applications.

### Infrastructure as Code
- Write code for managing resources instead of using command lines.

## Cloud Database Models

### Cloud Database Options
- **Build Databases on Virtualized Servers**: Customer is responsible for managing the database.
- **Use Managed Database Services**: Cloud provider manages the database for the customer.
- **Cloud-Native Database Platforms**: Full management of databases by the cloud provider.

### Cloud Service Models
- **XaaS (Anything as a Service)**: Broad category including various cloud services.
- **SaaS (Software as a Service)**: Customer purchases an entire application.
- **IaaS (Infrastructure as a Service)**: Customer purchases servers and storage.
- **PaaS (Platform as a Service)**: Customer purchases an app platform to run their own code/applications.
- **FaaS (Function as a Service)**: Users design functions to run on the provider's platform.

### Cloud Deployment Models
- **Private Cloud**: The organization owns its infrastructure.
- **Public Cloud**: Resources are shared among users to manage cloud services.
- **Hybrid Cloud**: A combination of public and private clouds.

## Cloud Security Frameworks and Compliance

### Cloud Controls Matrix (CCM)
- A security framework created by the Cloud Security Alliance, outlining detailed security controls and compliance requirements for cloud service providers.

### ISO Cloud Reference Architecture
- A framework from the International Organization for Standardization (ISO) that outlines roles, responsibilities, and activities for cloud services.

### Governance in Cloud Operations
- Ensures effective oversight of cloud operations.

### Audibility in Cloud Computing
- Allows customers to audit cloud providers' security operations.

### Regulatory Oversight in Cloud Computing
- Ensures compliance with regulations.

### Data Sovereignty
- Refers to local laws governing data within specific jurisdictions.

## Cloud Security Technologies

### Network-based CASB (Cloud Access Security Broker)
- Sits between users and cloud services to monitor security.

### API-based CASB
- Queries cloud services via an API to monitor for security issues.

### Cloud-native Controls
- Security controls offered by cloud providers for their services.

### Resource Policies in Cloud Security
- Rules that define what users or roles can do within a cloud environment.

## Networking Concepts

### Virtualization in Networking
- Runs hypervisors on physical hardware.

### Type 1 Hypervisors
- Run directly on hardware (e.g., VMware, Hyper-V).

### Type 2 Hypervisors
- Run on an existing operating system (e.g., VirtualBox).

### Access Control Lists (ACLs) in Networking
- Used for filtering traffic on routers.
  - **Standard ACLs**: Filter traffic by IP address.
  - **Extended ACLs**: Filter based on source/destination, ports, and protocols.

### Transmission Control Protocol (TCP) and Internet Protocol (IP)
- TCP (Transport Layer) and IP (Network Layer) are fundamental components of the internet protocol suite.

### IP Addressing
- Comprised of a network address (identifies the network) and a host address (identifies the host).

### Network Address Translation (NAT)
- Translates private IP addresses to public addresses.

### Subnetting
- Divides an IP address into network and host portions.

### DNS (Domain Name System)
- Resolves domain names to IP addresses (Port 53).

### DNSSEC (DNS Security Extensions)
- Adds digital signatures to DNS records to ensure authenticity.

### Port Ranges
- **0-1023**: Well-known ports.
- **1024-49151**: Application vendor ports.
- **49152-65535**: Dynamic ports.

### Common Ports
- **FTP**: Port 21
- **SSH**: Port 22
- **RDP**: Port 3389
- **NetBIOS**: Ports 137-139
- **SMTP**: Port 25
- **POP**: Port 110
- **IMAP**: Port 143

## Firewall Types

### Stateless Firewall
- Filters traffic based on individual packets.

### Stateful Firewall
- Monitors the state of active connections and filters traffic based on the context of the traffic, determining if an incoming packet is part of an existing, legitimate connection or potentially harmful.

# Cybersecurity Notes

## Next-Generation Firewall (NGFW)
- An advanced firewall that analyzes traffic at deeper levels, such as application data and user identity.
- Detects malware and blocks more complex attacks compared to traditional firewalls.

## Inline IPS Deployment
- An Intrusion Prevention System (IPS) device placed directly in the network path.
- Actively monitors and prevents malicious traffic in real-time by blocking or alerting to threats.

## Out-of-band IPS Deployment
- An IPS device that monitors network traffic by receiving copies of the traffic without being directly in the network path.
- Cannot block threats in real-time but can alert administrators for further action.

## Virtual Private Networks (VPN)
- **Site-to-Site VPN**: Connects remote offices to the headquarters.
- **Remote Access VPN**: Provides access to corporate networks for mobile users.

## IPsec
- Internet Protocol Security: A suite of protocols used to secure Internet Protocol (IP) communications.
- Authenticates and encrypts each IP packet in a communication session.
- Commonly used to create secure VPN connections.

## SSL/TLS VPN
- A type of VPN that uses SSL or TLS protocols to secure data transmitted over the internet.

## Load Balancing
- **Round-Robin Load Balancing**: A distribution method that equally distributes requests to servers.
- **Active-Active Load Balancing**: Multiple load balancers handle traffic simultaneously.
- **Active-Passive Load Balancing**: One load balancer handles all traffic, and another takes over if it fails.

## Managed Security Service Provider (MSSP)
- Third-party companies that manage and monitor security systems for businesses.

## Network Access Control (NAC)
- A security solution that enforces policies on devices attempting to connect to a network.
- Ensures devices meet predefined security requirements before granting access.
- Typically uses **802.1x** for authentication and authorization.

## Port Security
- A security feature on switches that restricts which MAC addresses can connect to each port.

## RAID (Redundant Array of Inexpensive Disks)
- Fault tolerance technique, not a backup strategy.
- Various RAID configurations:
  - **RAID 0 (Striping)**: Splits data across multiple drives to improve performance. Data is lost if a drive fails.
  - **RAID 1 (Mirroring)**: Identical data written to two drives. One drive failure doesn't result in data loss but reduces storage capacity.
  - **RAID 5 (Striping with Parity)**: Uses striping and stores parity information. Requires at least 3 drives. Can withstand one drive failure.
  - **RAID 6 (Striping with Double Parity)**: Similar to RAID 5 but with double parity. Can withstand two drive failures. Requires at least 4 drives.
  - **RAID 10 (Mirrored Striping)**: Combines RAID 1 and RAID 0 for high performance and fault tolerance. Requires at least 4 drives.

## Site Recovery and Backup
- **Hot Site**: A fully operational data center.
- **Cold Site**: An empty data center without servers or data.
- **Warm Site**: A partial setup with hardware and software but not fully operational.

### Backup Types
- **Full Backup**: A complete backup of all data.
- **Differential Backup**: Backs up data that has changed since the last full backup.
- **Incremental Backup**: Backs up data that has changed since the last backup of any type (full or incremental).

## Business Continuity and Disaster Recovery
- **Business Impact Assessment (BIA)**: Identifies and prioritizes risks to business operations.
- **Fault Tolerance**: Ensures systems remain resilient to failures.
- **Recovery Time Objective (RTO)**: The maximum allowable downtime after a disaster.
- **Recovery Point Objective (RPO)**: The maximum amount of data loss after a disaster.

## Other Key Concepts
- **Embedded Systems**: Technology components embedded inside larger systems (e.g., printers, self-driving cars).
- **Penetration Testing**: Simulating cyberattacks to identify vulnerabilities.

## Authentication and Authorization
- **TACACS+**: A centralized AAA protocol for managing network gear access. It encrypts full packets for enhanced security.
  - **Port**: TCP 49
- **RADIUS**: A centralized AAA protocol for managing user access to network resources.
  - **Ports**: UDP 1812 & 1813
  - **Security Comparison**: TACACS+ is more secure than RADIUS as it encrypts the entire packet, while RADIUS only encrypts the password.

## RAID 0 (Striping)
- Splits data evenly across two or more drives to improve performance. If one drive fails, all data is lost.

## RAID 1 (Mirroring)
- Writes identical data to two drives simultaneously. One drive can fail without data loss, but storage capacity is halved.

## RAID 5 (Striping with Parity)
- Uses striping and stores parity information (error-checking data) across all drives. Requires at least 3 drives. If one drive fails, data can be rebuilt using parity.

## RAID 6 (Striping with Double Parity)
- Similar to RAID 5 but with double parity. Requires at least 4 drives and can withstand the failure of two drives.

## RAID 10 (Mirrored Striping)
- Combines RAID 1 and RAID 0 for high performance and fault tolerance. Requires at least 4 drives. Each pair of drives contains a full copy of the striped data.
