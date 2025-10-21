# Architecture Review: Multi-Tenanant AWS SaaS Platform

## Overview
This multi-tenant AWS system design supports public facing traffic, that processes sensitive user data (PII). The system, conceptually, must remain highly available, scalable, and secure. The design process adheres to compliance requirements such as SOC2 and ISO 27001.

## Data Flow
Client to CloudFront (CDN):
Users connect to the nearest AWS edge location via HTTPS. CloudFront provides caching, DDoS protection (AWS Shield), and initial TLS termination.

CloudFront to AWS WAF to Application Load Balancer (ALB):
The Web Application Firewall filters malicious requests (e.g., injection, XSS) before forwarding traffic to the ALB in a public subnet.

ALB to ECS Fargate (Application Layer):
The ALB routes validated traffic internally to containerized application workloads running in private subnets, unreachable from the internet.

ECS to RDS and S3 (Data Layer):
The app securely connects to RDS for transactional data and to S3 for file storage. Both are encrypted with AWS KMS and accessed through private endpoints.