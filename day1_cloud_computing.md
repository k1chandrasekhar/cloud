# Day 1: Introduction to AWS

## What is Cloud Computing?
- Cloud computing is the on-demand delivery of IT resources over the Internet with pay-as-you-go pricing.
- Instead of buying, owning, and maintaining physical data centers and servers, you can access technology services (such as computing power, storage, and databases) on an as-needed basis from a cloud provider like AWS.
- Benefits: Cost savings, scalability, flexibility, high availability, and global reach.

## AWS Global Infrastructure
- **Regions:** Geographical areas that contain multiple, isolated locations known as Availability Zones (AZs).
- **Availability Zones (AZs):** Data centers within a region, isolated from failures in other AZs, but connected through low-latency links.
- **Edge Locations:** Used by AWS CloudFront to cache content closer to users for faster delivery.
- AWS has regions and AZs all over the world, allowing you to deploy applications close to your users.

## AWS Free Tier
- AWS Free Tier offers limited free access to many AWS services for 12 months following your AWS sign-up date.
- Examples:
  - 750 hours/month of EC2 (t2.micro or t3.micro)
  - 5 GB of S3 storage
  - 1 million Lambda requests/month
- Great for hands-on practice and learning without incurring costs.

## What is Amazon CloudFront?
Amazon CloudFront is a Content Delivery Network (CDN) service provided by AWS. It securely delivers data, videos, applications, and APIs to users globally with low latency and high transfer speeds.

- **How it works:** CloudFront caches copies of your content at Edge Locations around the world. When a user requests content, CloudFront serves it from the nearest Edge Location, reducing latency and improving performance.
- **Use cases:** Website acceleration, video streaming, API acceleration, software distribution, and security (DDoS protection, HTTPS, etc.).
- **Integration:** Works seamlessly with other AWS services like S3, EC2, Elastic Load Balancer, and Lambda@Edge.
- **Security:** Supports encryption, access controls, and integrates with AWS Shield for DDoS protection.

**In summary:** CloudFront helps deliver your content to users faster and more securely by using a global network of Edge Locations.

---

## Sample Exam Questions for Day 1 Topics

### What is Cloud Computing?
- **Q:** What is cloud computing and what are its main benefits?
  - **A:** Cloud computing is the on-demand delivery of IT resources over the Internet with pay-as-you-go pricing. Main benefits include cost savings, scalability, flexibility, high availability, and global reach.
- **Q:** Which of the following is NOT a benefit of cloud computing? (Options: Cost savings, Scalability, Manual server maintenance, Flexibility)
  - **A:** Manual server maintenance (cloud computing reduces the need for manual server maintenance).
- **Q:** Which AWS pricing model allows you to pay only for what you use?
  - **A:** Pay-as-you-go.

### AWS Global Infrastructure
- **Q:** What is an AWS Region?
  - **A:** A geographical area that contains multiple, isolated locations known as Availability Zones (AZs).
- **Q:** What is an Availability Zone (AZ) in AWS?
  - **A:** An AZ is a physically separate data center within a region, isolated from failures in other AZs but connected through low-latency links.
- **Q:** How do Availability Zones improve application availability?
  - **A:** By allowing you to deploy applications across multiple AZs, reducing the risk of downtime due to a single data center failure.
- **Q:** What is the purpose of AWS Edge Locations?
  - **A:** Edge Locations are used by AWS CloudFront to cache content closer to users for faster delivery.
- **Q:** Which AWS service uses Edge Locations to deliver content?
  - **A:** Amazon CloudFront.

### AWS Free Tier
- **Q:** How long does the AWS Free Tier last for a new account?
  - **A:** 12 months from the date you create your AWS account (for most services).
- **Q:** Which of the following is included in the AWS Free Tier? (Options: 750 hours/month of EC2 t2.micro, Unlimited S3 storage, 1 million Lambda requests/month, 10 TB RDS storage)
  - **A:** 750 hours/month of EC2 t2.micro and 1 million Lambda requests/month are included. Unlimited S3 storage and 10 TB RDS storage are not included.
- **Q:** Can you use the AWS Free Tier benefits on an account older than 12 months?
  - **A:** No, the Free Tier for most services expires after 12 months. Some services have an "Always Free" tier, but EC2 and S3 are not included after 12 months.

### What is Amazon CloudFront?
- **Q:** What is Amazon CloudFront?
  - **A:** Amazon CloudFront is a Content Delivery Network (CDN) service provided by AWS that securely delivers data, videos, applications, and APIs to users globally with low latency and high transfer speeds.
- **Q:** How does Amazon CloudFront improve content delivery?
  - **A:** CloudFront caches copies of your content at Edge Locations around the world, serving it from the nearest location to the user, thus reducing latency and improving performance.
- **Q:** What are the main use cases for Amazon CloudFront?
  - **A:** Website acceleration, video streaming, API acceleration, software distribution, and security (such as DDoS protection and HTTPS).
- **Q:** How does Amazon CloudFront integrate with other AWS services?
  - **A:** CloudFront works seamlessly with other AWS services like S3, EC2, Elastic Load Balancer, and Lambda@Edge.
- **Q:** What security features does Amazon CloudFront offer?
  - **A:** CloudFront supports encryption, access controls, and integrates with AWS Shield for DDoS protection.

---

## Deep Dive: Day 1 Topics

### What is Cloud Computing? (In-Depth)
- **Service Models:**
  - **IaaS (Infrastructure as a Service):** Provides virtualized computing resources over the internet (e.g., EC2, S3).
  - **PaaS (Platform as a Service):** Provides a platform allowing customers to develop, run, and manage applications (e.g., Elastic Beanstalk, Lambda).
  - **SaaS (Software as a Service):** Delivers software applications over the internet (e.g., AWS Chime, Salesforce).
- **Deployment Models:**
  - **Public Cloud:** Services offered over the public internet and available to anyone (e.g., AWS).
  - **Private Cloud:** Cloud infrastructure operated solely for a single organization.
  - **Hybrid Cloud:** Combines public and private clouds, allowing data and applications to be shared between them.
- **Key Characteristics:**
  - On-demand self-service, broad network access, resource pooling, rapid elasticity, measured service.
- **Cloud Computing Benefits (Expanded):**
  - **Agility:** Quickly develop, test, and launch applications.
  - **Elasticity:** Scale resources up or down as needed.
  - **Global Reach:** Deploy applications in multiple regions worldwide.
  - **Security:** AWS provides robust security controls and compliance certifications.

### AWS Global Infrastructure (In-Depth)
- **Regions:**
  - Each region is a separate geographic area.
  - Regions are isolated from each other for fault tolerance and stability.
  - Example: us-east-1 (N. Virginia), eu-west-1 (Ireland).
- **Availability Zones (AZs):**
  - Each region has multiple AZs (usually 3 or more).
  - AZs are physically separated, with independent power, cooling, and networking.
  - You can design applications to run in multiple AZs for high availability.
- **Edge Locations:**
  - Over 400+ Edge Locations globally (as of 2025).
  - Used by CloudFront and Route 53 for caching and DNS resolution.
- **Local Zones & Wavelength Zones:**
  - **Local Zones:** Extend AWS infrastructure to locations closer to end-users for low-latency applications.
  - **Wavelength Zones:** Bring AWS services to the edge of 5G networks for ultra-low latency.
- **Best Practices:**
  - Deploy across multiple AZs for fault tolerance.
  - Choose regions based on compliance, latency, and service availability.

### AWS Free Tier (In-Depth)
- **Types of Free Tier:**
  - **12-Month Free Tier:** Most popular services free for 12 months (EC2, S3, RDS, etc.).
  - **Always Free:** Some services have ongoing free usage limits (e.g., Lambda, DynamoDB, CloudWatch).
  - **Trials:** Short-term free trials for specific services.
- **Free Tier Limits (Examples):**
  - **EC2:** 750 hours/month of t2.micro or t3.micro (Linux/Windows) for 12 months.
  - **S3:** 5 GB Standard Storage, 20,000 GET, 2,000 PUT requests/month for 12 months.
  - **Lambda:** 1 million requests and 400,000 GB-seconds compute time/month, always free.
  - **DynamoDB:** 25 GB storage, always free.
- **Monitoring Free Tier Usage:**
  - Use AWS Billing Dashboard to track free tier usage and avoid unexpected charges.
- **Important Notes:**
  - Free tier is only for new accounts (except "Always Free" services).
  - Exceeding free tier limits incurs standard charges.

### Amazon CloudFront (In-Depth)
- **How CloudFront Works:**
  - Distributes content via a global network of Edge Locations.
  - Requests for content are automatically routed to the nearest Edge Location.
  - Supports both static (images, videos) and dynamic (APIs, web apps) content.
- **Key Features:**
  - **Lambda@Edge:** Run code closer to users for custom content delivery.
  - **Origin Failover:** Automatically switch to a backup origin if the primary fails.
  - **HTTPS Support:** Secure content delivery with SSL/TLS.
  - **Geo-Restriction:** Restrict content delivery based on geographic location.
- **Pricing:**
  - Pay only for data transfer and requests used to deliver content to customers.
- **Security:**
  - Integrates with AWS WAF (Web Application Firewall) for application layer protection.
  - DDoS protection via AWS Shield.

---

