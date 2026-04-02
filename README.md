# AWS Claude AI Agent — Cloud Infrastructure

A production-grade AWS cloud infrastructure built to host and operate an AI-powered multi-agent system. This repository documents the architecture, security configuration, and AWS services used to deploy and manage the environment.

---

## Infrastructure Overview

This environment was designed and deployed from the ground up with security, scalability, and agent orchestration in mind. It hosts a Claude-powered AI agent with sub-agents, integrates AWS-native services, and enforces strict access controls at every layer.

---

## AWS Services & Configuration

### Compute
- **EC2 Instance** — hosts the primary Claude AI agent runtime
- Custom **Security Group** configured to control inbound/outbound traffic with least-privilege rules
- **Security Token** issued to authorize local machine access to the web control interface

### Storage
- **S3 Bucket** — centralized file storage for the agent ecosystem
  - Stores and serves files accessed by sub-agents during task execution
  - Delivers output to end users via **pre-signed temporary URLs** — secure, time-limited access with no public exposure

### AI & Voice Services
- **AWS Polly** — integrated for AI-driven text-to-speech capabilities
- Polly access managed through scoped **IAM role** attached to the instance

### Systems Management
- **AWS Systems Manager (SSM)** — SSM Manager role attached to the EC2 instance
  - Enables secure instance management without opening SSH ports
  - Reduces attack surface by eliminating the need for direct remote access

### Identity & Access Management (IAM)
- **Root account** locked down — not used for day-to-day operations
- Dedicated **Admin IAM account** created for all management tasks
- **MFA (Multi-Factor Authentication)** enforced on the admin account
- IAM roles scoped to individual services (Polly, SSM) following least-privilege principles

### Networking & Security
- Custom **Security Group** with defined inbound/outbound rules
- **Pre-signed S3 URLs** used for secure, temporary content delivery
- **Security token** generated and issued to authorize Mac client access to the agent's web interface

---

## Agent Architecture

The system follows a hierarchical multi-agent design:

```
[ Claude Primary Agent ]
        |
  ┌─────┴─────┐
  │           │
[Sub-Agent] [Sub-Agent]
  │
[S3 Bucket]
  └── Input files (accessed by sub-agents)
  └── Output files (delivered via pre-signed URL)
```

Sub-agents access the S3 bucket to retrieve and process files as part of their workflow. Completed outputs are packaged and delivered through temporary pre-signed URLs — keeping content private while making it accessible on demand.

---

## Security Highlights

| Layer | Implementation |
|---|---|
| Account access | Root account isolated; admin account with MFA |
| Instance access | SSM Manager — no open SSH port required |
| Network | Custom security group with restricted rules |
| Storage | Private S3 bucket with pre-signed URL delivery |
| Client auth | Security token required for web interface access |
| Service permissions | IAM roles scoped per service (least privilege) |

---

## Tools & Technologies

- AWS EC2
- AWS S3
- AWS Polly
- AWS SSM (Systems Manager)
- AWS IAM
- Claude AI (Anthropic)
- Python / Boto3

---

## Notes

This repository documents infrastructure architecture and security posture only. No credentials, API keys, environment variables, or proprietary logic are included. All sensitive configuration is managed through AWS IAM roles and environment-level secrets.

Diagram reference: ![Image](https://github.com/user-attachments/assets/49103a42-ed2e-4117-8010-14c8ff00bfb9)
