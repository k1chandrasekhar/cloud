# Day 4: EC2 (Elastic Compute Cloud) - Part 2

## 1. Amazon EBS (Elastic Block Store)
- **What is EBS?** Amazon Elastic Block Store (EBS) provides persistent block-level storage volumes for use with Amazon EC2 instances. Each EBS volume is automatically replicated within its Availability Zone to protect you from component failure, offering high availability and durability.
- **Key Features:**
    - **Persistence:** EBS volumes persist independently from the life of an EC2 instance. You can stop/start an instance, and the data on attached EBS volumes remains.
    - **Volume Types:** Different types optimized for various workloads:
        - **General Purpose SSD (gp2/gp3):** Balance of price and performance for a wide variety of transactional workloads (boot volumes, dev/test environments).
        - **Provisioned IOPS SSD (io1/io2/io2 Block Express):** Highest performance SSD volumes for mission-critical, low-latency, or high-throughput workloads (large databases).
        - **Throughput Optimized HDD (st1):** Low-cost HDD volumes designed for frequently accessed, throughput-intensive workloads (big data, data warehouses).
        - **Cold HDD (sc1):** Lowest-cost HDD volumes for less frequently accessed workloads (file storage).
        - **Magnetic (standard):** Previous generation, suitable for infrequent access.
    - **Snapshots:** Point-in-time copies of your EBS volumes stored in Amazon S3. Used for backup, disaster recovery, and creating new EBS volumes.
    - **Encryption:** EBS volumes can be encrypted using AWS Key Management Service (KMS) for data at rest encryption. Snapshots of encrypted volumes are also encrypted.
    - **Elastic Volumes:** Modify volume type, size, and IOPS without detaching the volume or restarting the instance (for some volume types and instance configurations).
- **Root Volumes vs. Data Volumes:**
    - **Root Volume:** Contains the operating system. By default, the root volume is deleted when the instance terminates (this can be changed).
    - **Data Volumes:** Additional volumes attached to an instance for data storage. By default, data volumes are not deleted on instance termination.

## 2. Amazon Machine Images (AMIs)
- **Recap:** An AMI is a pre-configured template for your EC2 instances.
- **AMI Components:**
    - A template for the root volume for the instance (e.g., an operating system, an application server, and applications).
    - Launch permissions that control which AWS accounts can use the AMI to launch instances.
    - A block device mapping that specifies the volumes to attach to the instance when it's launched.
- **Types of AMIs:**
    - **AWS Provided AMIs:** Maintained by AWS (e.g., Amazon Linux 2, Ubuntu, Windows Server).
    - **AWS Marketplace AMIs:** Commercial AMIs from third-party vendors (e.g., with specific software pre-installed).
    - **Community AMIs:** Shared by other AWS users (use with caution).
    - **My AMIs:** AMIs you create yourself from your configured EC2 instances or from snapshots.
- **Creating Custom AMIs:**
    1.  Launch an EC2 instance from an existing AMI.
    2.  Connect to the instance and customize it (install software, configure settings).
    3.  Create an AMI from the configured instance. This process typically involves stopping the instance to ensure data consistency, though it's not always strictly required.
- **Copying AMIs:** You can copy AMIs within the same region or to different AWS regions. This is useful for disaster recovery or deploying applications globally.

## 3. EC2 Instance Pricing Models
- **On-Demand Instances:**
    - Pay for compute capacity by the hour or second (depending on instance type and OS) with no long-term commitments.
    - Use cases: Applications with short-term, spiky, or unpredictable workloads that cannot be interrupted.
- **Reserved Instances (RIs):**
    - Provide a significant discount (up to 72%) compared to On-Demand pricing in exchange for a commitment to use EC2 over a 1-year or 3-year term.
    - Types: Standard RIs, Convertible RIs.
    - Payment Options: All Upfront, Partial Upfront, No Upfront.
    - Use cases: Applications with steady-state or predictable usage.
- **Savings Plans:**
    - Flexible pricing model offering lower prices compared to On-Demand, in exchange for a commitment to a consistent amount of usage (measured in $/hour) for a 1 or 3-year term.
    - Types: Compute Savings Plans, EC2 Instance Savings Plans.
    - More flexible than RIs as they apply across instance families and regions (for Compute Savings Plans).
- **Spot Instances:**
    - Request spare EC2 computing capacity for up to 90% off the On-Demand price.
    - AWS can reclaim Spot Instances with a 2-minute notification if capacity is needed elsewhere.
    - Use cases: Applications that have flexible start and end times, are fault-tolerant, or can withstand interruptions (e.g., big data analysis, batch jobs, background processing).
- **Dedicated Hosts:**
    - Physical EC2 server dedicated for your use. Helps meet compliance requirements or use existing server-bound software licenses.
    - Most expensive option.
- **Dedicated Instances:**
    - Instances that run in a VPC on hardware that's dedicated to a single customer. They are physically isolated at the host hardware level from instances that belong to other AWS accounts.

---

## Sample Exam Questions for Day 4: EC2 Part 2

### 1. EBS Volumes
- **Q:** A developer needs persistent block storage for an EC2 instance that will host a database. Which AWS service should they use?
  - A) Amazon S3
  - B) Amazon EBS
  - C) Amazon EFS
  - D) Instance Store
  - **Answer:** B) Amazon EBS
  - **Explanation:** EBS provides persistent block storage suitable for databases and operating systems on EC2 instances.

- **Q:** Which EBS volume type is best suited for high-performance, IOPS-intensive database workloads?
  - A) Throughput Optimized HDD (st1)
  - B) General Purpose SSD (gp3)
  - C) Provisioned IOPS SSD (io2)
  - D) Cold HDD (sc1)
  - **Answer:** C) Provisioned IOPS SSD (io2)
  - **Explanation:** Provisioned IOPS SSD volumes (io1, io2) are designed for workloads requiring high, consistent IOPS.

- **Q:** How can you back up an EBS volume?
  - A) Copy the volume to another Availability Zone.
  - B) Create an EBS snapshot.
  - C) Attach the volume to another instance.
  - D) Enable versioning on the volume.
  - **Answer:** B) Create an EBS snapshot.
  - **Explanation:** EBS snapshots are point-in-time copies stored in S3, used for backups and creating new volumes.

### 2. AMIs
- **Q:** You have configured an EC2 instance with specific software and settings. You want to launch multiple identical instances. What should you do?
  - A) Manually configure each new instance.
  - B) Create a custom AMI from the configured instance.
  - C) Use multiple Elastic IP addresses.
  - D) Attach the same EBS volume to all instances.
  - **Answer:** B) Create a custom AMI from the configured instance.
  - **Explanation:** Creating a custom AMI allows you to replicate the configuration easily for new instances.

- **Q:** Which of the following is NOT a component of an AMI?
  - A) A template for the root volume
  - B) Launch permissions
  - C) A block device mapping
  - D) An EC2 instance type
  - **Answer:** D) An EC2 instance type
  - **Explanation:** The instance type is selected at launch time, not defined within the AMI itself. The AMI defines the software and initial storage configuration.

### 3. EC2 Pricing
- **Q:** Which EC2 pricing model offers the most significant discount but can be interrupted by AWS?
  - A) On-Demand Instances
  - B) Reserved Instances
  - C) Spot Instances
  - D) Dedicated Hosts
  - **Answer:** C) Spot Instances
  - **Explanation:** Spot Instances offer the largest discounts (up to 90%) but can be terminated if AWS needs the capacity back.

- **Q:** A company has a predictable, steady-state workload running on EC2 for the next 3 years. Which pricing model would provide the best cost savings while ensuring capacity?
  - A) On-Demand Instances
  - B) Spot Instances
  - C) Reserved Instances or Savings Plans
  - D) Dedicated Hosts
  - **Answer:** C) Reserved Instances or Savings Plans
  - **Explanation:** For long-term, predictable workloads, Reserved Instances or Savings Plans offer significant discounts over On-Demand pricing.

### 4. Scenario-Based Questions
- **Q:** A developer is creating a CI/CD pipeline that needs to run build jobs. These jobs are fault-tolerant and can be interrupted. Which EC2 pricing model is most cost-effective for these build jobs?
  - A) On-Demand
  - B) Reserved Instances
  - C) Spot Instances
  - D) Dedicated Hosts
  - **Answer:** C) Spot Instances
  - **Explanation:** Spot Instances are ideal for fault-tolerant, interruptible workloads like CI/CD build jobs due to their low cost.

- **Q:** An application requires its data to be encrypted at rest. The application runs on an EC2 instance with an EBS root volume. How can this be achieved?
  - A) Encrypt the EC2 instance itself.
  - B) Enable encryption when creating the EBS volume or launch an instance with an encrypted AMI.
  - C) Store encryption keys in the instance metadata.
  - D) Use HTTPS for all traffic to the instance.
  - **Answer:** B) Enable encryption when creating the EBS volume or launch an instance with an encrypted AMI.
  - **Explanation:** EBS encryption (using KMS) provides encryption at rest for EBS volumes. You can enable it during volume creation or by using an AMI that specifies encrypted volumes.

---

**Let me know if you want to discuss specific EBS volume types, AMI creation steps, or pricing model comparisons in more detail!**
