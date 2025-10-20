# app-sec-architecture-mini-project
Design AWS system architecture to practice diagraming, building controls, and drafting reviews. 

AWS Architecture (Conceptually):
Client
  │  HTTPS (TLS 1.2+)
CloudFront (content delivery network)
  │
AWS WAF - Web Application Firewall (managed rules + custom)
  │
ALB - Application Load Balancer (public)
  │
Private Subnets (no public IPs)
  └─ ECS Fargate Service (containers)
        │
        ├─ RDS Postgres (encrypted w/ KMS CMK)
        ├─ S3 (private buckets, bucket policies)
        ├─ Secrets Manager (DB creds, API keys)
        └─ VPC endpoints (S3/Secrets/CloudWatch)
