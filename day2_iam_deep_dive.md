# Day 2: AWS IAM Deep Dive

## 1. Root User
- The root user is the account created when you first set up your AWS account.
- Has full, unrestricted access to all resources in the account.
- **Best Practice:** Use the root user only for account and service management tasks that require root. Secure it with MFA and strong password.

## 2. IAM Users
- Represents a person or application that interacts with AWS resources.
- Each user has a unique name within the account and can have programmatic (API/CLI) and/or console access.
- Users are assigned permissions via policies.

## 3. IAM Groups
- A collection of IAM users.
- Groups let you manage permissions for multiple users at once (e.g., Admins, Developers, Auditors).
- Users inherit permissions assigned to the group.

## 4. IAM Roles
- An IAM role is an identity with specific permissions, not associated with a single user.
- Used for granting AWS resources or users temporary access to resources (e.g., EC2 instance role, cross-account access, federated users).
- Roles are assumed by trusted entities (users, services, applications).

## 5. Policies
- Policies are JSON documents that define permissions (allow/deny actions on resources).
- Types:
  - **Managed Policies:** AWS managed (predefined) or customer managed (your own).
  - **Inline Policies:** Embedded directly in a user, group, or role.
- Policies are attached to users, groups, or roles.

## 6. Permission Sets (AWS SSO)
- Used with AWS IAM Identity Center (formerly AWS SSO).
- Define a set of permissions that can be assigned to users/groups for access to AWS accounts.
- Useful for organizations with multiple AWS accounts.

## 7. Security Groups
- Act as virtual firewalls for EC2 instances to control inbound and outbound traffic.
- Operate at the instance level (not user level).
- Rules specify allowed protocols, ports, and source/destination IPs.

## 8. Whitelisting & Access Controls
- **IAM Policies:** Whitelist actions/resources for users, groups, or roles.
- **Resource Policies:** Attach directly to resources (e.g., S3 bucket policy, Lambda resource policy).
- **Security Groups:** Whitelist IPs/ports for EC2 instances.
- **NACLs (Network ACLs):** Whitelist/blacklist subnets at the VPC level.
- **Service Control Policies (SCPs):** Organization-level policies to control what accounts can do.
- **VPC Endpoint Policies:** Control access to VPC endpoints.

## 9. Levels of Users & Access
- **Root User:** Full access, use only for critical tasks.
- **IAM Users:** Individual users with assigned permissions.
- **Federated Users:** External identities (e.g., corporate directory, SAML, OIDC) mapped to roles.
- **Service Roles:** Used by AWS services to access resources on your behalf.
- **Cross-Account Roles:** Allow users in one AWS account to access resources in another.

## 10. Best Practices
- Enable MFA for all users, especially root.
- Grant least privilege (only permissions needed).
- Use groups to manage permissions.
- Rotate credentials regularly.
- Monitor with AWS CloudTrail and IAM Access Analyzer.

---

## Sample Exam Questions for Day 2: IAM, Security Groups, and Access Controls

### 1. IAM Users, Groups, and Roles
- **Q:** Which of the following statements about IAM users and groups is TRUE?
  - A) A user can belong to multiple groups.
  - B) A group can belong to another group.
  - C) A user can only belong to one group.
  - D) Groups can have policies, but users cannot.
  - **Answer:** A) A user can belong to multiple groups.
  - **Explanation:** Groups cannot be nested, but users can be in multiple groups. Both users and groups can have policies attached.

- **Q:** What is the main purpose of an IAM role?
  - A) To provide long-term credentials to users
  - B) To allow trusted entities to assume temporary permissions
  - C) To group users for easier management
  - D) To restrict access to the AWS root user
  - **Answer:** B) To allow trusted entities to assume temporary permissions
  - **Explanation:** Roles are used for temporary access, often for AWS services, federated users, or cross-account access.

### 2. Policies and Permissions
- **Q:** Which of the following is NOT a type of IAM policy?
  - A) Managed Policy
  - B) Inline Policy
  - C) Resource Policy
  - D) Security Group Policy
  - **Answer:** D) Security Group Policy
  - **Explanation:** Security Groups are not policies; they are virtual firewalls. The other three are valid policy types.

- **Q:** What does the principle of "least privilege" mean in AWS IAM?
  - A) Granting all permissions to all users
  - B) Granting only the permissions required to perform a task
  - C) Using the root user for daily tasks
  - D) Allowing users to create their own policies
  - **Answer:** B) Granting only the permissions required to perform a task
  - **Explanation:** Least privilege is a security best practice to minimize risk.

### 3. Security Groups and Whitelisting
- **Q:** What is a Security Group in AWS?
  - A) A group of IAM users
  - B) A virtual firewall for EC2 instances
  - C) A policy attached to S3 buckets
  - D) A set of permissions for Lambda functions
  - **Answer:** B) A virtual firewall for EC2 instances
  - **Explanation:** Security Groups control inbound and outbound traffic at the instance level.

- **Q:** Which of the following can you whitelist in a Security Group rule? (Select TWO)
  - A) IP address
  - B) IAM user
  - C) Port number
  - D) S3 bucket name
  - **Answer:** A) IP address, C) Port number
  - **Explanation:** Security Groups use IP addresses and port numbers to control traffic.

### 4. Root User and Best Practices
- **Q:** Which action should you take immediately after creating your AWS root user?
  - A) Share the credentials with your team
  - B) Enable Multi-Factor Authentication (MFA)
  - C) Delete the root user
  - D) Use the root user for all daily tasks
  - **Answer:** B) Enable Multi-Factor Authentication (MFA)
  - **Explanation:** MFA adds an extra layer of security to the root account.

- **Q:** Which of the following is a recommended best practice for IAM?
  - A) Use the root user for all administrative tasks
  - B) Assign permissions directly to individual users
  - C) Use groups to assign permissions
  - D) Allow all users to create their own policies
  - **Answer:** C) Use groups to assign permissions
  - **Explanation:** Groups make permission management easier and more secure.

### 5. Advanced Access Controls
- **Q:** What is the purpose of a Service Control Policy (SCP) in AWS Organizations?
  - A) To control network traffic to EC2 instances
  - B) To restrict permissions for IAM users in an account
  - C) To set permission guardrails for AWS accounts in an organization
  - D) To manage billing alerts
  - **Answer:** C) To set permission guardrails for AWS accounts in an organization
  - **Explanation:** SCPs define the maximum available permissions for accounts in an AWS Organization.

---

## Additional Unique & Scenario-Based IAM Questions (AWS Developer Associate)

### 1. Scenario: Temporary Access
- **Q:** A developer needs to access an S3 bucket for a limited time. What is the best way to grant this access?
  - A) Attach a policy directly to the developer's IAM user
  - B) Create a new IAM user and delete it after use
  - C) Create an IAM role with the required permissions and let the developer assume the role
  - D) Make the S3 bucket public
  - **Answer:** C) Create an IAM role with the required permissions and let the developer assume the role
  - **Explanation:** Roles are best for granting temporary, controlled access.

### 2. Scenario: Cross-Account Access
- **Q:** How can you allow an application in Account A to access resources in Account B securely?
  - A) Share root credentials between accounts
  - B) Use IAM roles with trust policies for cross-account access
  - C) Use Security Groups
  - D) Use inline policies only
  - **Answer:** B) Use IAM roles with trust policies for cross-account access
  - **Explanation:** Cross-account roles are the secure, recommended way.

### 3. Policy Evaluation
- **Q:** If a user is explicitly denied an action in one policy but allowed in another, what is the result?
  - A) The allow overrides the deny
  - B) The deny overrides the allow
  - C) The user can request permission from AWS Support
  - D) The most recently attached policy takes precedence
  - **Answer:** B) The deny overrides the allow
  - **Explanation:** Explicit deny always takes precedence in AWS policy evaluation.

### 4. Inline vs Managed Policies
- **Q:** What is a key difference between inline and managed policies?
  - A) Inline policies can be attached to multiple users
  - B) Managed policies are embedded in a single user, group, or role
  - C) Inline policies are embedded in a single user, group, or role
  - D) Managed policies cannot be reused
  - **Answer:** C) Inline policies are embedded in a single user, group, or role
  - **Explanation:** Managed policies can be reused; inline policies are unique to one identity.

### 5. IAM Policy Simulator
- **Q:** What is the purpose of the IAM Policy Simulator?
  - A) To simulate EC2 instance launches
  - B) To test and troubleshoot IAM and resource policies
  - C) To create new IAM users
  - D) To monitor billing alerts
  - **Answer:** B) To test and troubleshoot IAM and resource policies
  - **Explanation:** The IAM Policy Simulator helps you understand and validate permissions.

### 6. MFA-Required Policy
- **Q:** How can you enforce that users must use MFA to perform certain actions?
  - A) Attach a policy with a condition requiring MFA authentication
  - B) Enable MFA on the root user only
  - C) Use Security Groups
  - D) Use a Service Control Policy
  - **Answer:** A) Attach a policy with a condition requiring MFA authentication
  - **Explanation:** IAM policies can require MFA via the aws:MultiFactorAuthPresent condition key.

### 7. S3 Bucket Policy vs IAM Policy
- **Q:** What is a key difference between an S3 bucket policy and an IAM policy?
  - A) S3 bucket policies are attached to users
  - B) IAM policies are attached to resources
  - C) S3 bucket policies are attached to resources, IAM policies to identities
  - D) Both are always required for access
  - **Answer:** C) S3 bucket policies are attached to resources, IAM policies to identities
  - **Explanation:** S3 bucket policies are resource-based; IAM policies are identity-based.

### 8. Permission Boundaries
- **Q:** What is a permission boundary in AWS IAM?
  - A) A limit on the number of users in a group
  - B) A managed policy that sets the maximum permissions an IAM entity can have
  - C) A firewall rule for EC2
  - D) A type of resource policy
  - **Answer:** B) A managed policy that sets the maximum permissions an IAM entity can have
  - **Explanation:** Permission boundaries are advanced controls for limiting permissions.

### 9. IAM Access Analyzer
- **Q:** What does IAM Access Analyzer do?
  - A) Analyzes EC2 instance performance
  - B) Identifies resources that are shared with external entities
  - C) Monitors billing usage
  - D) Creates IAM users automatically
  - **Answer:** B) Identifies resources that are shared with external entities
  - **Explanation:** Access Analyzer helps you find unintended public or cross-account access.

### 10. Service-Linked Roles
- **Q:** What is a service-linked role in AWS?
  - A) A role created and managed by an AWS service to perform actions on your behalf
  - B) A role for cross-account access
  - C) A role for federated users
  - D) A role for EC2 instances only
  - **Answer:** A) A role created and managed by an AWS service to perform actions on your behalf
  - **Explanation:** Service-linked roles are required for some AWS services to function properly.

