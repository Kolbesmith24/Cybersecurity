### Virtualization & Cloud Security

#### VM Escape Attacks
- **VM escape**: Attackers attempt to break out of virtual machines (VMs) to access the host system or other VMs.
    

#### VM Sprawl
- **VM Sprawl**: Unused and abandoned servers on the network that may not be properly maintained.
    

#### Virtualization Technologies
- **VDI (Virtual Desktop Infrastructure)**: Provides network-based access to a desktop computing environment.
    
- **Application Virtualization**: Access applications running in different environments from your systems.
    
- **Block Storage**: Allocates large chunks of storage partitioned into volumes, more expensive as you pay for all storage on the drive.
    
- **Object Storage**: Stores files as individual objects, managed by cloud providers; less expensive as you pay only for the storage you use.
    

#### Cloud Storage Security

- **Cloud Storage Security Implementations**:
    
    - Set permissions properly.
        
    - Use encryption for sensitive data.
        
    - Replicate data to multiple data centers for backup.
        

#### Cloud Networking & Management

- **VPC (Virtual Private Cloud)**: Different clouds for different classes of servers.
    
- **VPC Endpoints**: Securely connect VPCs to each other and other cloud-based services.
    
- **SDN (Software-Defined Networking)**: Automates cloud networking.
    
- **SDV (Software-Defined Visibility)**: Uses the provider's API to gain visibility into cloud network traffic.
    
- **Cloud Orchestration**: Automates cloud management workflows.
    
- **Infrastructure as Code**: Write code for managing resources instead of using command lines.
    

---

### Cloud Computing Models & Services

#### Cloud Database Options

- **Build Databases on Virtualized Servers**: Burden is on the customer.
    
- **Use Managed Database Services**: Cloud provider manages the database for the customer.
    
- **Cloud-Native Database Platforms**: Full management on the cloud.
    

#### Cloud Service Models

- **XaaS (Anything as a Service)**: Broad category that includes various cloud services.
    
- **SaaS (Software as a Service)**: Customer purchases an entire application.
    
- **IaaS (Infrastructure as a Service)**: Customer purchases servers and storage.
    
- **PaaS (Platform as a Service)**: Customer purchases an app platform to run their own code/applications.
    
- **FaaS (Function as a Service)**: Users design functions that run on the provider’s platform.
    

#### Cloud Security Models

- **Private Cloud**: The organization owns its infrastructure.
    
- **Public Cloud**: Resources are shared to manage cloud services.
    
- **Hybrid Cloud**: Uses both public and private clouds.
    
- **CCM (Cloud Controls Matrix)**: Outlines cloud security controls to meet objectives.
    
- **ISO Cloud Reference Architecture**: Framework defining cloud computing activities and responsibilities.
    
- **Governance in Cloud Operations**: Ensures effective oversight of cloud operations.
    
- **Audibility in Cloud Computing**: Allows customers to audit cloud providers’ security operations.
    
- **Regulatory Oversight in Cloud Computing**: Ensures compliance with regulations.
    
- **Data Sovereignty**: Local laws regarding data.
    

---

### Cloud Networking & Security

#### Cloud Access Security Brokers (CASB)

- **Network-based CASB**: Sits between user and cloud services to monitor security.
    
- **API-based CASB**: Queries cloud service via an API to monitor for security issues.
    

#### Cloud Security Tools

- **Cloud-native Controls**: Security controls offered by cloud providers for their services.
    
- **Resource Policies**: Limit actions taken by users with cloud access.
    

#### Virtualization & Networking

- **Virtualization**: Runs hypervisors on physical hardware.
    
    - **Type 1 Hypervisors**: Run directly on hardware (e.g., VMware, Hyper-V).
        
    - **Type 2 Hypervisors**: Run on an existing OS (e.g., VirtualBox).
        
- **ACLs (Access Control Lists)**: Used for filtering traffic on routers.
    
    - **Standard ACLs**: Filter by IP address.
        
    - **Extended ACLs**: Filter based on source/destination, ports, protocols.
        

---

### IP & Network Security

#### TCP/IP and IP Addressing

- **TCP/IP**: Transmission Control Protocol (Transport Layer) and Internet Protocol (Network Layer).
    
- **IP Address**: Comprised of a network address (identifies network) and host address (identifies host).
    
- **Network Address Translation (NAT)**: Transition of private IP addresses to public addresses.
    
- **Subnetting**: Divides an IP address into network and host portions.
    

#### DNS & Ports

- **DNS**: Resolves domain names to IP addresses (Port 53).
    
- **DNSSEC**: Adds digital signatures to DNS records to ensure authenticity.
    
- **Port Ranges**:
    
    - 0-1023: Well-known ports.
        
    - 1024-49151: Application vendor ports.
        
    - 49152-65535: Dynamic ports.
        

#### Common Ports

- **FTP**: Port 21
    
- **SSH**: Port 22
    
- **RDP**: Port 3389
    
- **NetBIOS**: Ports 137-139
    
- **SMTP**: Port 25
    
- **POP**: Port 110
    
- **IMAP**: Port 143
    

---

### Network Protocols & Security

#### Firewalls & IDS/IPS

- **Firewall Types**:
    
    - **Stateless**: Filters based on individual packets.
        
    - **Stateful**: Tracks established connections.
        
    - **NGFW (Next-Generation Firewall)**: Works at all OSI layers and analyzes more than just packet filtering.
        
- **IPS Deployment**:
    
    - **Inline**: Device sits in the network path.
        
    - **Out-of-band**: Device gets copies of network traffic without intervening.
        

#### VPN & SSL/TLS

- **VPN Types**:
    
    - **Site-to-Site VPN**: Connects remote offices to headquarters.
        
    - **Remote Access VPN**: Provides access to corporate networks for mobile users.
        
    - **IPsec**: Provides secure transport for VPNs.
        
    - **SSL/TLS VPN**: Works over port 443.
        

#### Load Balancers & VPN

- **Load Balancing**:
    
    - **Round-Robin**: Equal distribution of requests to servers.
        
    - **Active-Active**: Multiple load balancers handle traffic simultaneously.
        
    - **Active-Passive**: One load balancer handles all traffic, another takes over if it fails.
        

---

### Security in Cloud Services

#### Security Measures

- **MSSPs (Managed Security Service Providers)**: Offer security services for other organizations.
    
- **NAC (Network Access Control)**: Uses 802.1x to analyze and control network access.
    
- **Port Security**: Limits allowed MAC addresses on a switch port.
    

#### Disaster Recovery

- **RAID (Redundant Array of Inexpensive Disks)**: Fault tolerance technique, not a backup strategy.
    
- **Disaster Recovery Sites**:
    
    - **Hot Site**: Fully operational data center.
        
    - **Cold Site**: Empty data center without servers/data.
        
    - **Warm Site**: Partial setup with hardware and software, but not fully operational.
        
- **Backup Types**:
    
    - **Full**: Complete backup.
        
    - **Differential**: Backs up data modified since the last full backup.
        
    - **Incremental**: Backs up data modified since the last full or incremental backup.
        

#### BC/DR (Business Continuity/Disaster Recovery)

- **BIA (Business Impact Assessment)**: Identifies and prioritizes risks.
    
- **Fault Tolerance**: Ensures systems remain resilient to failures.
    
- **Recovery Objectives**:
    
    - **RTO (Recovery Time Objective)**: Max recovery time after disaster.
        
    - **RPO (Recovery Point Objective)**: Max data loss after disaster.
        

---

### Miscellaneous Topics

#### Embedded Systems

- **Embedded Systems**: Technology components embedded inside larger systems (e.g., printers, self-driving cars).
    

#### Security Testing

- **Penetration Testing**: Simulating cyberattacks to identify vulnerabilities.
