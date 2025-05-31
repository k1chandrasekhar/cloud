# Day 3: EC2 (Elastic Compute Cloud) - Part 1

## 1. Amazon EC2 (Elastic Compute Cloud) Overview
- **What is EC2?** Amazon Elastic Compute Cloud (EC2) is a web service that provides secure, resizable compute capacity in the cloud. It is designed to make web-scale cloud computing easier for developers.
- **Key Concepts:**
    - **Instances:** Virtual servers in the cloud.
    - **Amazon Machine Images (AMIs):** Pre-configured templates for your instances that package the OS, application server, and applications.
    - **Instance Types:** Various configurations of CPU, memory, storage, and networking capacity for your instances (e.g., t2.micro, m5.large).
    - **Key Pairs:** Secure login information for your instances (public key on instance, private key with you).
    - **Security Groups:** Virtual firewalls that control inbound and outbound traffic to your instances.
    - **Elastic IP Addresses:** Static IPv4 addresses for dynamic cloud computing.
    - **Amazon EBS (Elastic Block Store):** Persistent block storage volumes for use with EC2 instances.

## 2. Launching EC2 Instances
- **Steps to Launch an Instance:**
    1.  **Choose an AMI:** Select an Amazon Machine Image (e.g., Amazon Linux 2, Ubuntu, Windows Server).
    2.  **Choose an Instance Type:** Select the hardware configuration (CPU, memory, storage, network).
    3.  **Configure Instance Details:**
        -   Number of instances.
        -   Network (VPC) and Subnet.
        -   IAM role (to grant permissions to applications running on EC2).
        -   User data (scripts to run on instance launch).
    4.  **Add Storage:** Configure root volume and add additional EBS volumes if needed.
    5.  **Add Tags:** Key-value pairs to organize and manage resources.
    6.  **Configure Security Group:** Define firewall rules (ports, protocols, source IPs).
    7.  **Review and Launch:** Verify settings and select or create a key pair.
- **User Data:** Scripts (e.g., shell scripts, cloud-init directives) that run when an instance starts for the first time. Used for bootstrapping, installing software, etc.
    - Example:
      ```bash
      #!/bin/bash
      yum update -y
      yum install -y httpd
      systemctl start httpd
      systemctl enable httpd
      echo "<h1>Hello from EC2 instance $(hostname -f)</h1>" > /var/www/html/index.html
      ```

## 3. Security Groups
- **Function:** Act as a stateful firewall for your EC2 instances to control inbound and outbound traffic at the instance level.
- **Stateful:** If you allow inbound traffic on a port, outbound traffic on that port is automatically allowed, and vice-versa.
- **Rules:**
    - **Inbound Rules:** Control incoming traffic to the instance.
    - **Outbound Rules:** Control outgoing traffic from the instance. By default, all outbound traffic is allowed.
- **Key Features:**
    - You can specify source/destination IP addresses, CIDR blocks, or other security groups.
    - Security groups are associated with network interfaces.
    - Changes to security group rules are applied immediately.
    - You can have multiple security groups attached to an instance.
- **Default Security Group:** If you don't specify a security group during launch, the instance is associated with the default security group for the VPC, which typically allows all outbound traffic and inbound traffic only from other instances in the same security group.
- **Best Practice:** Create specific security groups for different roles/applications, following the principle of least privilege.

## 4. Key Pairs
- **Purpose:** Used to securely connect to your EC2 instances.
- **Mechanism:** AWS uses public-key cryptography.
    - **Public Key:** Stored by AWS and placed on your instance.
    - **Private Key:** You download and store this securely. You need it to SSH (Linux) or RDP (Windows) into your instance.
- **Creation:** You can create a key pair in the AWS Management Console or upload your own public key.
- **Formats:**
    - `.pem` (for OpenSSH on Linux/macOS)
    - `.ppk` (for PuTTY on Windows)
- **Security:**
    - **Guard your private key:** If lost, you cannot generate it again. You'll lose access to instances launched with that key pair (unless you have other access methods configured).
    - Do not embed private keys in your code or share them.
- **Connecting to Linux Instances (SSH):**
  ```bash
  ssh -i /path/to/your-key.pem ec2-user@your-instance-public-dns
  ```
- **Connecting to Windows Instances (RDP):** You use the private key to decrypt the Administrator password.

---

## Sample Exam Questions for Day 3: EC2 Part 1

### 1. EC2 Basics & Launching
- **Q:** What is an Amazon Machine Image (AMI)?
  - A) A type of EC2 instance with high CPU
  - B) A template containing the software configuration (OS, application server, and applications) required to launch your instance
  - C) A static IP address for an EC2 instance
  - D) A script that runs when an EC2 instance launches
  - **Answer:** B) A template containing the software configuration (OS, application server, and applications) required to launch your instance
  - **Explanation:** AMIs are the blueprints for EC2 instances, defining the initial software state.

- **Q:** A developer wants to run a script to install software on an EC2 instance when it first launches. Which EC2 feature should they use?
  - A) IAM Role
  - B) Security Group
  - C) User Data
  - D) Elastic IP
  - **Answer:** C) User Data
  - **Explanation:** User data scripts are executed automatically on the first boot of an instance, ideal for bootstrapping.

### 2. Security Groups
- **Q:** Which statement accurately describes EC2 Security Groups?
  - A) They are stateless firewalls.
  - B) They operate at the subnet level.
  - C) They control inbound and outbound traffic for EC2 instances and are stateful.
  - D) They can only control inbound traffic.
  - **Answer:** C) They control inbound and outbound traffic for EC2 instances and are stateful.
  - **Explanation:** Security Groups are stateful (return traffic is automatically allowed) and operate at the instance level.

- **Q:** A web server running on an EC2 instance needs to accept HTTP traffic on port 80 from any IP address. How should the Security Group be configured?
  - A) Add an inbound rule: Protocol TCP, Port Range 80, Source 0.0.0.0/0
  - B) Add an outbound rule: Protocol TCP, Port Range 80, Destination 0.0.0.0/0
  - C) Add an inbound rule: Protocol UDP, Port Range 80, Source 0.0.0.0/0
  - D) Add an outbound rule: Protocol HTTP, Port Range All, Destination Any
  - **Answer:** A) Add an inbound rule: Protocol TCP, Port Range 80, Source 0.0.0.0/0
  - **Explanation:** HTTP uses TCP port 80. An inbound rule with source 0.0.0.0/0 allows traffic from any IP.

### 3. Key Pairs
- **Q:** What is the primary purpose of an EC2 key pair?
  - A) To encrypt data stored on EBS volumes
  - B) To securely connect to your EC2 instances using SSH or RDP
  - C) To assign permissions to EC2 instances
  - D) To provide a static IP address for an instance
  - **Answer:** B) To securely connect to your EC2 instances using SSH or RDP
  - **Explanation:** Key pairs are used for authenticating users when they connect to their instances.

- **Q:** A developer has lost the private key for an EC2 instance. What is the most likely outcome?
  - A) AWS can regenerate the private key for them.
  - B) They can no longer connect to the instance using that key pair.
  - C) The instance will automatically shut down.
  - D) They can retrieve the private key from the instance metadata.
  - **Answer:** B) They can no longer connect to the instance using that key pair.
  - **Explanation:** AWS does not store your private key. If lost, and no other access method is configured (like SSM Agent), you lose SSH/RDP access via that key.

### 4. Scenario-Based Questions
- **Q:** You are launching a fleet of EC2 instances that will host a web application. These instances need to download updates from the internet and allow inbound HTTP traffic from users. Which Security Group configuration is most appropriate?
  - A) Inbound: Allow TCP port 80 from 0.0.0.0/0. Outbound: Deny all.
  - B) Inbound: Allow TCP port 22 from your IP. Outbound: Allow all.
  - C) Inbound: Allow TCP port 80 from 0.0.0.0/0. Outbound: Allow all.
  - D) Inbound: Deny all. Outbound: Allow TCP port 80 to 0.0.0.0/0.
  - **Answer:** C) Inbound: Allow TCP port 80 from 0.0.0.0/0. Outbound: Allow all.
  - **Explanation:** This allows web traffic in and allows instances to reach the internet for updates. "Allow all" outbound is default and often acceptable for this use case, but can be restricted further if needed.

- **Q:** A company wants to ensure that only specific IP addresses can access their EC2 instances via SSH (port 22). How should they configure their Security Group?
  - A) Create an inbound rule allowing TCP port 22 from source 0.0.0.0/0.
  - B) Create an inbound rule allowing TCP port 22 from the specific IP addresses/CIDR blocks.
  - C) Create an outbound rule allowing TCP port 22 to the specific IP addresses/CIDR blocks.
  - D) Use an IAM policy to restrict SSH access.
  - **Answer:** B) Create an inbound rule allowing TCP port 22 from the specific IP addresses/CIDR blocks.
  - **Explanation:** Security Groups are the correct tool for IP-based access control to instances. IAM policies manage AWS API access, not direct instance SSH access.

---

**Let me know if you want hands-on lab ideas, more detailed explanations, or to move on to Day 4!**
