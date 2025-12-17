# WordPress Self Hosting

## Executive Summary

This document proposes a **self-hosted, WordPress hosting platform** to serve multiple customers, each with their own domain. This approach provides **strong isolation, security, fast deployment, predictable costs, and long-term scalability**, similar to how managed WordPress providers (e.g. WP Engine) operate internally.

The goal is to:

- Host multiple customer WordPress sites on a single Linux server
- Keep customers isolated from one another
- Deploy new customer sites quickly
- Maintain full control over infrastructure and cost

---

## Architecture

### High-Level Design

Each customer runs on their own **WordPress stack**, composed of:

- WordPress
- Dedicated database (MariaDB/MySQL)
- Persistent storage volumes
- Routed by web server using the customer’s domain

```
Internet
   ↓
(Reverse Proxy + HTTPS)
   ↓
 ├─ customer1.com → WordPress + DB
 ├─ customer2.com → WordPress + DB
 └─ customer3.com → WordPress + DB
```

Reverse Proxy automatically:

- Routes traffic by domain name
- Issues and renews HTTPS certificates
- Prevents unintended service exposure

---

## Why This Approach Is the Best Option

### 1. Strong Customer Isolation

- Each customer has their **own WordPress instance and database**
- Plugin or security issues affect **only one customer**, not others
- Easier compliance, debugging, and recovery

### 2. Security by Design

- Containers isolate applications at the OS level
- Unique database credentials per customer
- No shared WordPress files or databases
- Reduced attack surface compared to shared hosting or multisite

### 3. Fast Deployment

- New customer sites can be deployed in **5–10 minutes**
- Process is automated
- No manual server configuration per customer

---

## Cost Implications (What We Pay For)

### 1. Infrastructure Costs (Required)

| Item               | Description                         | Cost Impact        |
| ------------------ | ----------------------------------- | ------------------ |
| Linux Server       | Hosts, WordPress, and Reverse Proxy | Fixed monthly cost |
| Storage            | WordPress files, uploads, databases | Grows per customer |
| Backups Storage    | Off-server backups (S3-compatible)  | Low–Medium         |
| Domains (Optional) | If we manage customer domains       | Per domain         |

### 2. Software Costs

| Component         | Cost                     |
| ----------------- | ------------------------ |
| Docker            | Free (Open Source)       |
| WordPress         | Free (Open Source)       |
| Reverse Proxy     | Free (Community Edition) |
| MariaDB/MySQL     | Free                     |
| Let’s Encrypt SSL | Free                     |

**No licensing fees** for the core platform.

---

## Operational Responsibilities (What We Manage)

### We Are Responsible For:

- Server uptime and OS updates
- Docker and Reverse Proxy maintenance
- WordPress core updates (can be automated)
- Database backups and restores (can be automated)
- Security hardening (firewall, access control)

### We Are NOT Paying For:

- Shared hosting fees
- Per-site WordPress licenses
- SSL certificates
- Expensive managed WordPress platforms

---

## Cost vs Managed Hosting Comparison

| Category      | Managed WP Hosting      | Proposed Solution          |
| ------------- | ----------------------- | -------------------------- |
| Cost per site | High (monthly per site) | Marginal increase per site |
| Control       | Limited                 | Full                       |
| Isolation     | Partial                 | Full                       |
| Scaling       | Vendor dependent        | Under our control          |
| Automation    | Limited                 | Fully customizable         |

This approach significantly reduces **long-term operational cost** as the number of customers grows.

---

## Scalability & Future Growth

This design supports:

- Adding customers without downtime
- Migrating individual customers to new servers
- Horizontal scaling for high-traffic customers
- Optional future move to an Ochestration tool

We can also integrate:

- CDN (Cloudflare)
- Redis caching
- Centralized monitoring and logging

---

## Risk Management

### Risks

- Requires internal DevOps knowledge
- Infrastructure responsibility is in-house

---

## Conclusion

This WordPress self hosting approach offers:

- **Fast customer onboarding**
- **Strong security and isolation**
- **Low and predictable costs**
- **Enterprise-grade architecture**

It is a future-proof solution that balances **cost efficiency**, **control**, and **scalability**, making it the best choice for hosting multiple customer WordPress sites on our own infrastructure.

---
