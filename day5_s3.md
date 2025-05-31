# Day 5: S3 (Simple Storage Service) Deep Dive

Amazon S3 (Simple Storage Service) is an object storage service that offers industry-leading scalability, data availability, security, and performance.

## 1. S3 Buckets
- **Definition:** Buckets are the fundamental containers in S3 where you store your data.
- **Naming:** Bucket names must be globally unique across all AWS accounts and all Regions. They must be DNS compliant.
- **Regions:** You create buckets in a specific AWS Region. Choosing a region closer to your users or other AWS resources can optimize latency, minimize costs, or address regulatory requirements.
- **Properties:** Buckets have various configurable properties like versioning, logging, static website hosting, lifecycle policies, and access permissions.

## 2. S3 Objects
- **Definition:** Objects are the fundamental entities stored in S3. They consist of data and metadata.
- **Data:** The actual content you are storing (e.g., images, videos, documents, backups).
- **Metadata:** A set of name-value pairs that describe the object (e.g., `Content-Type`, `Last-Modified`). There are system-defined and user-defined metadata.
- **Keys:** Each object is uniquely identified within a bucket by its key (name). You can think of a key as the full path to the object (e.g., `images/nature/forest.jpg`). S3 has a flat structure, but the `/` in keys allows for logical hierarchy.
- **Size:** Objects can range in size from 0 bytes to 5 TB. For objects larger than 5GB, multipart upload is recommended.

## 3. S3 Permissions and Access Control
Securing data in S3 involves multiple layers:

- **IAM Policies:** Define what actions IAM users, groups, or roles can perform on S3 resources (buckets and objects). These are identity-based policies.
- **Bucket Policies:** JSON-based policies attached directly to S3 buckets. They grant or deny permissions to the bucket and its objects. These are resource-based policies. Useful for cross-account access or granting public access.
- **Access Control Lists (ACLs):** A legacy access control mechanism that grants read/write permissions to other AWS accounts at the bucket or individual object level. Bucket policies are generally preferred for more fine-grained control.
- **S3 Block Public Access:** A set of settings at the account or bucket level to prevent accidental public exposure of data. Enabled by default for new buckets.
- **Presigned URLs:** Provide temporary access to specific S3 objects using your AWS credentials. Useful for allowing users to upload or download objects without needing AWS credentials or permissions.
- **VPC Endpoints for S3:** Allow resources in your VPC to access S3 without traversing the public internet, enhancing security and potentially reducing data transfer costs.

## 4. S3 Versioning
- **Definition:** Versioning keeps multiple variants of an object in the same bucket. When enabled, S3 saves all versions of an object (including all writes and even if you delete an object).
- **Benefits:**
    - **Recovery:** Easily recover from accidental deletions or overwrites by restoring a previous version.
    - **Archive:** Maintain a history of object changes.
- **How it works:** Each object version gets a unique Version ID. When you `GET` an object, you retrieve the current version. To retrieve a specific version, you must specify its Version ID.
- **Lifecycle:** Versioning can be suspended but not disabled once enabled on a bucket.
- **MFA Delete:** Can be enabled to require Multi-Factor Authentication for permanently deleting an object version or changing the versioning state of a bucket.

## 5. S3 Lifecycle Management
- **Definition:** Lifecycle policies allow you to define rules to automatically transition objects to different storage classes or delete them after a specified period.
- **Purpose:** Optimize storage costs and manage object retention.
- **Transitions:** Move objects to more cost-effective storage classes as they age or become less frequently accessed (e.g., S3 Standard -> S3 Standard-IA -> S3 Glacier Flexible Retrieval -> S3 Glacier Deep Archive).
- **Expirations:** Define when objects (current or noncurrent versions) should be permanently deleted.
- **Rules:** Lifecycle rules can be based on:
    - **Prefixes:** Apply to a subset of objects in a bucket (e.g., `logs/`, `images/archive/`).
    - **Tags:** Apply to objects with specific tags.
    - **Object Age:** Number of days since creation or since becoming a noncurrent version.
    - **Object Size:** For transitioning to S3 Intelligent-Tiering.
- **Considerations:** Small objects might incur additional costs for lifecycle transitions.

## 6. S3 Storage Classes
S3 offers a range of storage classes designed for different use cases:
- **S3 Standard:** General-purpose storage for frequently accessed data. Default storage class.
- **S3 Intelligent-Tiering:** Automatically moves data to the most cost-effective access tier based on access patterns. Good for data with unknown or changing access patterns.
- **S3 Standard-Infrequent Access (S3 Standard-IA):** For long-lived, less frequently accessed data, but requires rapid access when needed. Lower storage cost than S3 Standard, but has a retrieval fee.
- **S3 One Zone-Infrequent Access (S3 One Zone-IA):** Similar to S3 Standard-IA but stores data in a single Availability Zone. Lower cost than S3 Standard-IA, but data is lost if the AZ is destroyed.
- **S3 Glacier Instant Retrieval:** For archive data that needs millisecond retrieval. Higher storage cost than Glacier Flexible Retrieval but faster access.
- **S3 Glacier Flexible Retrieval (formerly S3 Glacier):** For long-term archival. Retrieval times from minutes to hours.
- **S3 Glacier Deep Archive:** Lowest-cost storage class for long-term retention of data that is accessed rarely (once or twice a year). Retrieval times typically within 12 hours.
- **S3 Outposts:** Delivers S3 object storage to your on-premises AWS Outposts environment.

## 7. S3 Features for Data Management & Security
- **Replication (CRR and SRR):**
    - **Cross-Region Replication (CRR):** Asynchronously copies objects across buckets in different AWS Regions. Used for disaster recovery, compliance, or lower latency access.
    - **Same-Region Replication (SRR):** Asynchronously copies objects across buckets in the same AWS Region. Used for log aggregation or production/test account synchronization.
- **Encryption:**
    - **Server-Side Encryption (SSE):** Encrypts data at rest.
        - **SSE-S3:** S3 manages the encryption keys.
        - **SSE-KMS:** S3 integrates with AWS Key Management Service (KMS) for key management. Provides an audit trail of key usage.
        - **SSE-C:** You manage the encryption keys (Client-Side Master Key).
    - **Client-Side Encryption:** Encrypt data before sending it to S3.
- **S3 Object Lock:** Provides Write-Once-Read-Many (WORM) protection for objects. Prevents objects from being deleted or overwritten for a fixed amount of time or indefinitely.
- **S3 Inventory:** Provides a flat file list (CSV, ORC, or Parquet) of your objects and their metadata on a daily or weekly basis for a bucket or a shared prefix.
- **S3 Batch Operations:** Perform large-scale batch operations on S3 objects (e.g., copy objects, invoke Lambda functions, restore from Glacier).
- **S3 Select & Glacier Select:** Retrieve only a subset of data from an object using SQL expressions, improving performance and reducing data transfer.
- **Static Website Hosting:** Configure an S3 bucket to host a static website.

---

## Sample Exam Questions for Day 5: S3

### 1. S3 Buckets and Objects
**Q1:** A developer needs to store large binary files (up to 5TB each) in AWS. The files need to be globally accessible via a unique name. Which AWS service is most appropriate?
A) Amazon EBS
B) Amazon EFS
C) Amazon S3
D) Amazon Glacier

**Answer:** C) Amazon S3
**Explanation:** S3 is designed for object storage, supports objects up to 5TB, and bucket names are globally unique. EBS is block storage for EC2, EFS is file storage for EC2, and Glacier is for archiving.

**Q2:** What is a key characteristic of S3 bucket names?
A) They are unique only within a specific AWS Region.
B) They must be at least 255 characters long.
C) They are globally unique across all AWS accounts.
D) They are case-sensitive.

**Answer:** C) They are globally unique across all AWS accounts.
**Explanation:** S3 bucket names must be unique across the entire S3 namespace. They are DNS-compliant, which means they are case-insensitive for practical purposes (though S3 itself might store them as entered).

### 2. S3 Permissions and Access Control
**Q3:** A company wants to grant read-only access to a specific S3 bucket to an external partner's AWS account. Which is the most secure and recommended way to achieve this?
A) Share the root user credentials.
B) Create an IAM user with read-only access and share the access keys.
C) Use an S3 bucket policy to grant cross-account access.
D) Make the bucket public.

**Answer:** C) Use an S3 bucket policy to grant cross-account access.
**Explanation:** Bucket policies are designed for granting cross-account access to S3 resources in a secure and granular manner. Sharing credentials or making buckets public are not secure practices.

**Q4:** A developer wants to allow users to upload files directly to an S3 bucket from a web application for a limited time without exposing AWS credentials. Which S3 feature should be used?
A) S3 Access Control Lists (ACLs)
B) IAM Roles
C) S3 Presigned URLs
D) S3 Bucket Policies granting public write access

**Answer:** C) S3 Presigned URLs
**Explanation:** Presigned URLs provide temporary, time-limited permissions to perform specific actions (like uploading or downloading) on S3 objects, without requiring the user to have AWS credentials.

### 3. S3 Versioning
**Q5:** What is the primary benefit of enabling versioning on an S3 bucket?
A) It reduces storage costs by deduplicating objects.
B) It allows for hosting multiple websites in a single bucket.
C) It protects against accidental deletion or overwriting of objects.
D) It automatically encrypts all objects in the bucket.

**Answer:** C) It protects against accidental deletion or overwriting of objects.
**Explanation:** Versioning keeps all versions of an object, allowing you to retrieve or restore previous versions if they are accidentally deleted or overwritten.

**Q6:** If versioning is enabled on an S3 bucket and an object is deleted, what happens?
A) The object is permanently removed and cannot be recovered.
B) S3 creates a delete marker for the object, and previous versions are retained.
C) The object is moved to the S3 Glacier storage class automatically.
D) Versioning is automatically suspended for that object.

**Answer:** B) S3 creates a delete marker for the object, and previous versions are retained.
**Explanation:** Deleting an object in a version-enabled bucket creates a delete marker, which becomes the "current" version. The actual data of previous versions remains and can be recovered.

### 4. S3 Lifecycle Management
**Q7:** A company stores monthly reports in S3. After 90 days, these reports are rarely accessed but must be retained for 7 years for compliance. After 1 year, they can be moved to the lowest-cost archival storage. Which S3 feature helps automate this process cost-effectively?
A) S3 Versioning
B) S3 Replication
C) S3 Lifecycle Policies
D) S3 Event Notifications

**Answer:** C) S3 Lifecycle Policies
**Explanation:** Lifecycle policies allow you to define rules to automatically transition objects to different storage classes (e.g., S3 Standard-IA, S3 Glacier Flexible Retrieval, S3 Glacier Deep Archive) or delete them based on their age.

**Q8:** An S3 lifecycle policy is configured to transition objects to S3 Glacier Deep Archive after 365 days and then expire them after 2555 days (7 years). What happens to an object that is 400 days old?
A) It is immediately deleted.
B) It is transitioned to S3 Glacier Deep Archive.
C) It remains in S3 Standard.
D) It is transitioned to S3 Standard-IA.

**Answer:** B) It is transitioned to S3 Glacier Deep Archive.
**Explanation:** Since the object is 400 days old, it has passed the 365-day threshold for transition to S3 Glacier Deep Archive.

### 5. S3 Storage Classes
**Q9:** A developer is looking for a storage solution for data that is accessed infrequently but requires millisecond retrieval when needed. The data must be resilient across multiple Availability Zones. Which S3 storage class is most suitable?
A) S3 One Zone-IA
B) S3 Glacier Flexible Retrieval
C) S3 Standard-IA
D) S3 Glacier Instant Retrieval

**Answer:** D) S3 Glacier Instant Retrieval
**Explanation:** S3 Glacier Instant Retrieval is designed for archive data that needs immediate, millisecond access. S3 Standard-IA also offers fast retrieval but is for less frequently accessed data that isn't necessarily archival. S3 One Zone-IA is not resilient across multiple AZs. S3 Glacier Flexible Retrieval has retrieval times in minutes or hours.

**Q10:** For which use case would S3 One Zone-Infrequent Access (S3 One Zone-IA) be a good choice?
A) Storing critical data that requires the highest level of durability and availability.
B) Storing frequently accessed data.
C) Storing reproducible data that is infrequently accessed and can be lost if an AZ fails.
D) Long-term archival of data that is rarely accessed.

**Answer:** C) Storing reproducible data that is infrequently accessed and can be lost if an AZ fails.
**Explanation:** S3 One Zone-IA is cheaper than S3 Standard-IA because it stores data in a single AZ. This makes it suitable for data that is infrequently accessed, can be easily reproduced, and where the cost savings outweigh the risk of data loss from an AZ failure.

### 6. S3 Security & Encryption
**Q11:** A company needs to ensure that data uploaded to an S3 bucket is encrypted at rest. They want AWS to manage the encryption keys. Which server-side encryption option should they use?
A) SSE-C (Server-Side Encryption with Customer-Provided Keys)
B) Client-Side Encryption
C) SSE-S3 (Server-Side Encryption with Amazon S3-Managed Keys)
D) SSE-KMS (Server-Side Encryption with AWS KMS-Managed Keys) where they manage the CMK.

**Answer:** C) SSE-S3 (Server-Side Encryption with Amazon S3-Managed Keys)
**Explanation:** SSE-S3 is the simplest way to achieve server-side encryption where S3 handles all aspects of key management and encryption. SSE-KMS provides more control and auditability but requires managing Customer Master Keys (CMKs) in KMS.

**Q12:** What is the purpose of S3 Object Lock?
A) To encrypt objects using customer-provided keys.
B) To prevent public access to a bucket.
C) To enforce Write-Once-Read-Many (WORM) policies on objects.
D) To automatically replicate objects to another region.

**Answer:** C) To enforce Write-Once-Read-Many (WORM) policies on objects.
**Explanation:** S3 Object Lock helps prevent objects from being deleted or overwritten for a fixed amount of time or indefinitely, supporting WORM models often required for regulatory compliance.

---
