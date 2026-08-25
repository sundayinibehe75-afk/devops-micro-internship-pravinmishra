# Assignment 6 — Capstone: Deploy Book Review App (Three-Tier Architecture) on Azure

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

This is the most important assignment of the course. You will deploy the Book Review App in a production-ready, best-practice-compliant three-tier architecture on Azure: separated presentation, application, and database tiers, least-privilege network access, a controlled public entry point, protected secrets, and availability/monitoring evidence.

---

# Task 1 — Design the Azure Three-Tier Architecture

## Goal

Create an architecture diagram and implementation plan identifying the presentation, application, and database components, the chosen Azure services, the public entry point, and the internal traffic paths.

### Evidence

#### Screenshot 1 — Architecture diagram showing the public entry point, three tiers, network boundaries, and traffic flow

![alt text](screenshots/Assignment-06-Task-01-screenshot-01.png)

---

#### Screenshot 2 — Written architecture assumptions and selected Azure services

Architecture Assumptions — Book Review App (Azure, Three-Tier)

App maps cleanly to the three tiers: Next.js frontend (web), Express backend (app), MySQL (database) — genuinely separate frontend/backend folders in the repo.

Single region (South India), single Resource Group — required since MySQL Flexible Server's private VNet integration must match the VNet's region.

5 subnets in bookreview-vnet (10.0.0.0/16): appgw-public-subnet, web-subnet, appgw-internal-subnet, app-subnet, db-subnet. Two subnets exist purely for Application Gateway since Azure requires it to have a dedicated subnet, on both public and internal hops.

Compute: Azure VM(s) for both web tier (web-subnet) and application tier (app-subnet).

Public entry: Application Gateway (Standard V2), public frontend IP, backend pool = web-tier VMs on port 3000.

Web→app hop: a second Application Gateway, private frontend IP only — public IP still provisioned (Azure platform requirement for V2 SKU) but unused, since no listener binds to it.

Database: Azure Database for MySQL Flexible Server, Private access (VNet Integration), in db-subnet, public access disabled.

Traffic path: Internet → public AGW → web-tier VM (3000) → internal AGW → app-tier VM (5000) → MySQL (3306). Each hop enforced by subnet-scoped NSG rules.

SSH to app tier allowed only from web-subnet (jump host), used for deployment only — app tier has no public IP, so no direct internet path exists regardless.

Secrets: intended for Azure Key Vault rather than plaintext config. Monitoring: Azure Monitor + diagnostics on both Gateways and the database. Backup: MySQL Flexible Server automated backup/retention enabled; web/app tiers are stateless, recovery via redeploy

---

# Task 2 — Create the Azure Network Foundation

## Goal

Create a dedicated Resource Group and VNet with separate subnets for the web, application, and database tiers, keeping the application and database tiers without direct public access.

### Evidence

#### Screenshot 3 — Resource Group overview showing the assignment resources

![alt text](screenshots/Assignment-06-Task-02-screenshot-03a.png)
![alt text](screenshots/Assignment-06-Task-02-screenshot-03b.png)

---

#### Screenshot 4 — VNet overview showing the address space and all required subnets

![alt text](screenshots/Assignment-06-Task-02-screenshot-04.png)
---

#### Screenshot 5 — Route-table or Private DNS evidence where applicable

![alt text](screenshots/Assignment-06-Task-02-screenshot-05.png)

---

# Task 3 — Configure Security and Secret Management

## Goal

Apply least-privilege NSG rules so traffic flows Internet → public entry point → web tier → application tier → database tier, and store credentials in Azure Key Vault or another approved secure mechanism.

### Evidence

#### Screenshot 6 — NSG rules proving least-privilege access between the tiers

![alt text](screenshots/Assignment-06-Task-03-screenshot-06.png)

---

#### Screenshot 7 — Key Vault or approved secret-management configuration (without displaying secret values)

![alt text](screenshots/Assignment-06-Task-03-screenshot-07.png)

---

# Task 4 — Deploy the Presentation (Web) Tier

## Goal

Deploy the Book Review App presentation layer on the approved web-tier compute service, configured to route requests to the internal application-tier endpoint, and not directly exposed except through the public entry service.

### Evidence

#### Screenshot 8 — Web-tier compute overview showing subnet and availability configuration

![alt text](screenshots/Assignment-06-Task-04-screenshot-08.png)

---

#### Screenshot 9 — Terminal or service output proving the presentation layer is running

![alt text](screenshots/Assignment-06-Task-04-screenshot-09a.png)
![alt text](screenshots/Assignment-06-Task-04-screenshot-09b.png)

---

# Task 5 — Deploy the Business (Application) Tier

## Goal

Deploy the Book Review App backend privately in the application subnet, configured to use the private database endpoint and secured environment values, reachable only through its internal endpoint.

### Evidence

#### Screenshot 10 — Application-tier compute overview showing private subnet placement

![alt text](screenshots/Assignment-06-Task-05-screenshot-10.png)

---

#### Screenshot 11 — Backend process, service, or listening-port evidence

![alt text](screenshots/Assignment-06-Task-05-screenshot-11.png)

---

#### Screenshot 12 — Internal health-check or API response (without exposing secrets)

![alt text](screenshots/Assignment-06-Task-05-screenshot-12.png)

---

# Task 6 — Deploy the Managed Database Tier

## Goal

Create a private Azure managed database (public access disabled), with availability/backup/retention settings, the Book Review App schema imported, and access restricted to the application tier only.

### Evidence

#### Screenshot 13 — Database overview showing private connectivity and public access disabled

![alt text](screenshots/Assignment-06-Task-06-screenshot-13.png)

---

#### Screenshot 14 — Availability, backup, and retention configuration

![alt text](screenshots/Assignment-06-Task-06-screenshot-14.png)

---

#### Screenshot 15 — Successful schema or connectivity verification (without exposing credentials)

![alt text](screenshots/Assignment-06-Task-06-screenshot-15.png)

---

# Task 7 — Configure Traffic Management, Availability, and Monitoring

## Goal

Configure the approved public entry service with health probes and backend pools, internal routing for the application tier where required, and enable Azure Monitor/diagnostics/logs/alerts for the key resources.

### Evidence

#### Screenshot 16 — Public entry service showing listener, frontend endpoint, and healthy web targets

![alt text](screenshots/Assignment-06-Task-07-screenshot-16a.png)
![alt text](screenshots/Assignment-06-Task-07-screenshot-16b.png)

---

#### Screenshot 17 — Internal application-tier load-balancing or routing configuration where applicable

![alt text](screenshots/Assignment-06-Task-07-screenshot-17.png)

---

#### Screenshot 18 — Azure Monitor, diagnostic settings, logs, metrics, or alert evidence

![alt text](screenshots/Assignment-06-Task-07-screenshot-18.png)

---

# Task 8 — Validate the Production-Style Deployment

## Goal

Confirm the Book Review App works end to end through the public endpoint, with at least one database read and one write, confirm private tiers are not internet-reachable, and complete a safe availability test.

### Evidence

#### Screenshot 19 — Browser showing the Book Review App through the public endpoint

![alt text](screenshots/Assignment-06-Task-08-screenshot-19.png)

---

#### Screenshot 20 — Proof of successful database-backed read and write operations

![alt text](screenshots/Assignment-06-Task-08-screenshot-20.png)

---

#### Screenshot 21 — Evidence that private tiers are not publicly accessible

![alt text](screenshots/Assignment-06-Task-08-screenshot-21a.png)
![alt text](screenshots/Assignment-06-Task-08-screenshot-21b.png)

---

#### Screenshot 22 — Availability-test and healthy-target evidence

![alt text](screenshots/Assignment-06-Task-08-screenshot-22a.png)
![alt text](screenshots/Assignment-06-Task-08-screenshot-22b.png)

---

#### Public Endpoint

Paste your public endpoint URL here:

http://52.140.54.224

---

### Notes

Summarize what worked, issues encountered and how they were fixed, and the availability/security/secrets/monitoring/backup choices made.

Deployed the Book Review App (Next.js, Express, MySQL) across a 5-subnet Azure VNet: public Application Gateway → web tier → internal Application Gateway → app tier → MySQL Flexible Server. Neither app nor database tier has a public IP; NSG rules are subnet-scoped end to end.

What worked: Frontend and backend both deployed and connect to MySQL over a private, SSL-enforced connection. Backend auto-creates its schema and seeds sample data on startup — confirmed via the API. Both Application Gateways report healthy backend targets. Public endpoint serves login/register pages successfully.

Issues and fixes:

Application Gateway requires its own dedicated subnet (public and internal) — drove the 5-subnet design.
Standard V2 needs an explicit NSG rule allowing GatewayManager on ports 65200-65535, or deployment fails.
Hit a 3-IP quota limit; internal Application Gateway still needs a public IP as a platform requirement even though only its private listener is used.
Private subnets have no outbound internet without a NAT Gateway, which I skipped to conserve IP quota. Worked around it by cloning the repo and installing dependencies on the internet-connected web VM, then copying the repo (with node_modules) and a Node.js binary to the app VM over the private network.
A MySQL "access denied" error traced back to a typo in the .env password.
Frontend currently calls an undefined API URL due to a missing env var, so writes (register/review) fail through the public path; reads work fine. Documented as a known limitation given time constraints.
No internet access also meant pm2 couldn't be installed on the app tier; used nohup instead, which doesn't survive a full reboot.

Availability: Both Gateways have health probes and autoscaling; single VM per tier given quota/time constraints, so no true failover test was possible.

Security: Subnet-scoped NSGs throughout; SSH to the app tier restricted to the web subnet only, used as a deployment jump host.

Secrets: Database credentials and JWT secret stored in Azure Key Vault, RBAC-gated access.

Monitoring: Diagnostic settings on both Application Gateways streaming to a shared Log Analytics workspace.

Backup: MySQL Flexible Server automated backup/retention enabled; both compute tiers are stateless, so recovery is via redeploy.

---

# Submission Instructions

- Add all required screenshots and links in your submission
- Do not expose passwords, keys, connection strings, or subscription IDs

---

# Completion Checklist

- [ ] Task 1: Architecture diagram and assumptions documented (Screenshots 1–2)
- [ ] Task 2: Network foundation created with isolated tiers (Screenshots 3–5)
- [ ] Task 3: Least-privilege security and secret management configured (Screenshots 6–7)
- [ ] Task 4: Presentation tier deployed (Screenshots 8–9)
- [ ] Task 5: Application tier deployed privately (Screenshots 10–12)
- [ ] Task 6: Managed database tier deployed privately (Screenshots 13–15)
- [ ] Task 7: Public entry, internal routing, and monitoring configured (Screenshots 16–18)
- [ ] Task 8: End-to-end validation and availability test completed (Screenshots 19–22, Public Endpoint, Notes)
- [ ] No sensitive data exposed

---

## 📌 About DMI & CloudAdvisory

DevOps Micro Internship (DMI) is a project-based DevOps program run by Pravin Mishra (The CloudAdvisory) focused on real-world execution, systems thinking, and career readiness.

It helps learners build strong DevOps foundations with hands-on experience.

---

## 📌 Resources

- 🌐 DMI Official Website: https://dmi.pravinmishra.com?utm_source=github&utm_medium=readme  
- 🎓 University: https://university.pravinmishra.com?utm_source=github&utm_medium=readme  
- 💬 Discord Community: https://discord.pravinmishra.com?utm_source=github&utm_medium=readme  
- 📝 Blog: https://dmi.pravinmishra.com/blog?utm_source=github&utm_medium=readme  
- ▶️ YouTube Playlist: https://www.youtube.com/playlist?list=PLFeSNDtI4Cho  
- 🔗 Pravin Mishra (LinkedIn): https://www.linkedin.com/in/pravin-mishra-aws-trainer/  
- 🏢 CloudAdvisory (LinkedIn): https://www.linkedin.com/company/thecloudadvisory/

---

*This submission is part of DevOps Micro Internship (DMI) Cohort 3 — Agentic AI Track.*
