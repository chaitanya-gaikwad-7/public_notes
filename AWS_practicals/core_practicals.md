### 🌐 **Core AWS Services – Must-Do Practicals**

#### 1. **EC2**
- Launch a Linux EC2 instance.
- Connect via SSH and install a web server (Apache/Nginx).
- Create and assign security groups and key pairs.
- Create an AMI from your instance and launch a copy.

#### 2. **VPC & Networking**
- Create a custom VPC with public/private subnets.
- Add an Internet Gateway and route tables.
- Launch EC2s in private/public subnets and enable communication.
- Set up a NAT Gateway for private subnet internet access.

#### 3. **S3**
- Create an S3 bucket and upload/download files.
- Enable versioning, encryption (SSE-S3 or SSE-KMS), and lifecycle policies.
- Host a static website using S3.

#### 4. **IAM**
- Create users, groups, and roles.
- Apply custom policies (e.g., S3 read-only).
- Set up a role for an EC2 instance to access S3 without credentials.

#### 5. **RDS**
- Launch an RDS instance (MySQL/PostgreSQL).
- Connect from an EC2 in the same VPC.
- Practice snapshot and restore, multi-AZ setup.

#### 6. **ELB & Auto Scaling**
- Deploy EC2s behind a Load Balancer (ALB).
- Configure Auto Scaling Group to increase/decrease based on CPU usage.

#### 7. **Route 53**
- Register a domain or use a free one (Freenom).
- Set up a hosted zone and configure DNS records.
- Use health checks and routing policies (weighted, failover).

#### 8. **CloudFront**
- Create a CloudFront distribution for an S3 static site.
- Restrict S3 bucket access using an Origin Access Identity (OAI).

#### 9. **CloudWatch**
- Set up custom EC2 metrics and alarms.
- Create a dashboard to monitor CPU, memory (using CloudWatch Agent).

#### 10. **SNS & SQS**
- Set up a notification system using SNS.
- Create an SQS queue and simulate a producer-consumer pattern.

---