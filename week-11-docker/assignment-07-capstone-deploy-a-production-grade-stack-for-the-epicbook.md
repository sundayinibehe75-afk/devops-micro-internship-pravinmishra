# Assignment 7 — Capstone: Deploy a Production-Grade Stack for The EpicBook

Part of the DevOps Micro Internship (DMI) with Agentic AI

---

## Purpose

In this assignment, you will deploy the EpicBook application as a production-oriented Docker Compose stack on a cloud VM. You will use optimized container images, isolated networks, health checks, persistent MySQL storage, a selected reverse proxy, logging, backup and restore testing, and reliability procedures.

---

# Task 0 — App Discovery and Architecture

## Goal

Review the EpicBook repository and design the intended application architecture.

### Evidence

#### Screenshot 1 — EpicBook Project Structure

Add a terminal screenshot showing the EpicBook project structure after cloning the repository.

![alt text](screenshots/Assignment-07-Task-00-screenshot-01.png)

---

#### Screenshot 2 — Architecture Diagram

Add a screenshot of your architecture diagram showing:

- Public user
- Reverse proxy
- Frontend
- Backend
- Database
- Docker networks
- Public and private ports
- Persistent database storage

Add your full name inside the diagram or as a clear caption below it.

![alt text](screenshots/Assignment-07-Task-00-screenshot-02.png)

---

#### Screenshot 3 — Environment Variables and Ports Document

Add a screenshot showing the contents of:

```text
docs/02-env-and-ports.md
```

It must document environment-variable names, internal ports, persistent-data details, and the health-check method. Do not expose real credentials or values.

![alt text](screenshots/Assignment-07-Task-00-screenshot-03.png)

---

# Task 1 — Create Production Docker Images

## Goal

Create optimized production images for the EpicBook backend and frontend.

### Evidence

#### Screenshot 4 — Backend Dockerfile

Add a screenshot showing `backend/Dockerfile`, including:

- Dependency stage
- Minimal runtime stage
- Production startup command
- Internal backend port
- Non-root user configuration

![alt text](screenshots/Assignment-07-Task-01-screenshot-04.png)

---

#### Screenshot 5 — Frontend Dockerfile

Add a screenshot showing `frontend/Dockerfile`, including:

- Nginx runtime image
- Static frontend files copied to the Nginx web root

![alt text](screenshots/Assignment-07-Task-01-screenshot-05.png)

---

#### Screenshot 6 — Docker Ignore Files

Add a screenshot showing both:

```text
backend/.dockerignore
frontend/.dockerignore
```

![alt text](screenshots/Assignment-07-Task-01-screenshot-06.png)

---

#### Screenshot 7 — Docker Image Builds and Size Comparison

Add a terminal screenshot showing successful builds of:

- Baseline backend image
- Optimized backend image
- Frontend image

The screenshot must also show the baseline and optimized backend image-size comparison.

![alt text](screenshots/Assignment-07-Task-01-screenshot-07.png)

---

#### Screenshot 8 — Backend Running as Non-Root User

Add a terminal screenshot showing the optimized backend container running as a non-root user.

![alt text](screenshots/Assignment-07-Task-01-screenshot-08.png)

---

### Notes

Write a short note covering:

- Baseline and optimized backend image sizes
- The image-size reduction achieved
- One Docker layer-caching optimization used
- The security benefit of running the backend as a non-root user

Image sizes: the baseline single-stage backend image (epicbook-backend:single) is 1.68GB. The optimized multi-stage image (epicbook-backend:1.0) is 238MB.

Reduction: about 1.44GB smaller, an 86% reduction. The optimized image uses a dependency stage to install packages and a minimal runtime stage that copies in only what the app needs to run, leaving build tools and caches behind.

Layer caching: package.json and package-lock.json are copied and dependencies are installed before the application source is copied. Code changes therefore don't invalidate the dependency layer, and rebuilds reuse the cached npm install instead of downloading every package again.

Non-root user: the backend runs as a non-root user. If an attacker exploited a vulnerability in the app, they would only get a limited user's permissions inside the container, not root. That makes it much harder to modify system files, install tools or escape to the host.

---

# Task 2 — Create the Docker Compose Stack and Networks

## Goal

Create one Docker Compose stack containing the reverse proxy, frontend, backend, and MySQL database.

### Evidence

#### Screenshot 9 — Docker Compose Services

Add a screenshot showing `docker-compose.yml` with all four services:

```text
reverse-proxy
frontend
backend
database
```

![alt text](screenshots/Assignment-07-Task-02-screenshot-09a.png)
![alt text](screenshots/Assignment-07-Task-02-screenshot-09b.png)

---

#### Screenshot 10 — Networks and Named Volume

Add a screenshot showing:

- `front-tier` network
- `back-tier` network
- `db_data` named volume

![alt text](screenshots/Assignment-07-Task-02-screenshot-10.png)

---

#### Screenshot 11 — Docker Compose Validation

Add a terminal screenshot showing successful Docker Compose validation without exposing environment-variable values or secrets.

![alt text](screenshots/Assignment-07-Task-02-screenshot-11.png)

---

# Task 3 — Configure Health Checks and Startup Dependencies

## Goal

Configure health checks and ensure services start only after their dependencies are healthy.

### Evidence

#### Screenshot 12 — Backend Health Endpoint

Add a screenshot showing the backend application configuration for the `/health` endpoint.

![alt text](screenshots/Assignment-07-Task-03-screenshot-12.png)

---

#### Screenshot 13 — MySQL and Backend Health Checks

Add a screenshot showing `docker-compose.yml` with health checks for MySQL and the backend.

![alt text](screenshots/Assignment-07-Task-03-screenshot-13.png)

---

#### Screenshot 14 — Frontend and Reverse-Proxy Health Checks

Add a screenshot showing:

- Frontend health check
- Reverse-proxy health check
- `depends_on` conditions using `service_healthy`

![alt text](screenshots/Assignment-07-Task-03-screenshot-14.png)

---

#### Screenshot 15 — Running Healthy Services

Add a terminal screenshot showing Docker Compose service status. The database, backend, frontend, and reverse proxy must be running successfully.

![alt text](screenshots/Assignment-07-Task-03-screenshot-15.png)

---

#### Screenshot 16 — Public Health Endpoint

Add a terminal screenshot showing a successful response from the public application health endpoint through the reverse proxy.

![alt text](screenshots/Assignment-07-Task-03-screenshot-16.png)

---

#### Screenshot 17 — Health-Check and Startup-Order Document

Add a screenshot showing the contents of:

```text
docs/03-healthchecks-and-depends-on.md
```

Explain the health-check method for each service and the startup dependency order.

![alt text](screenshots/Assignment-07-Task-03-screenshot-17.png)

---

# Task 4 — Configure the Reverse Proxy and Same-Origin Routing

## Goal

Use either Nginx or Traefik as the only public entry point for the EpicBook application.

### Evidence

#### Screenshot 18 — Selected Reverse-Proxy Configuration

Add a screenshot showing the configuration for your selected reverse proxy.

It must show routes for:

- Static frontend assets
- Application pages
- API requests
- Health endpoint

![alt text](screenshots/Assignment-07-Task-04-screenshot-18.png)

---

#### Screenshot 19 — Only Reverse Proxy Publishes Port 80

Add a screenshot of `docker-compose.yml` showing that only the `reverse-proxy` service publishes port 80.

![alt text](screenshots/Assignment-07-Task-04-screenshot-19.png)

---

#### Screenshot 20 — Reverse-Proxy Route Testing

Add a terminal screenshot showing successful requests through the selected reverse proxy to:

- Application page
- One API endpoint
- One static asset
- Health endpoint

![alt text](screenshots/Assignment-07-Task-04-screenshot-20.png)

---

#### Screenshot 21 — EpicBook Application Through Public IP

Add a browser screenshot showing the EpicBook application loaded through the VM public IP address.

Add your full name as a clear caption below the screenshot.

![alt text](screenshots/Assignment-07-Task-04-screenshot-21.png)

---

#### Screenshot 22 — Proxy Routing and CORS Document

Add a screenshot showing the contents of:

```text
docs/04-proxy-routing-and-cors.md
```

Explain the proxy routes and state whether CORS was required and why.

![alt text](screenshots/Assignment-07-Task-04-screenshot-22.png)

---

# Task 5 — Prove Data Persistence, Backup, and Restore

## Goal

Verify MySQL persistence and perform a controlled backup and restore drill.

### Evidence

#### Screenshot 23 — MySQL Volume Configuration

Add a terminal screenshot showing the `db_data` named volume and its MySQL mount configuration.

![alt text](screenshots/Assignment-07-Task-05-screenshot-23.png)

---

#### Screenshot 24 — Test Data Before Backup

Add a terminal screenshot showing the selected test data before the backup and restore drill.

![alt text](screenshots/Assignment-07-Task-05-screenshot-24.png)

---

#### Screenshot 25 — Successful Backup Creation

Add a terminal screenshot showing successful backup creation and the backup file stored in the host backup directory.

![alt text](screenshots/Assignment-07-Task-05-screenshot-25.png)

---

#### Screenshot 26 — Controlled Data-Loss Test

Add a terminal screenshot showing that the selected test record was removed during the controlled data-loss test.

![alt text](screenshots/Assignment-07-Task-05-screenshot-26.png)

---

#### Screenshot 27 — Restore Verification

Add a terminal screenshot showing successful restore and verification that the deleted test record is available again.

![alt text](screenshots/Assignment-07-Task-05-screenshot-27.png)

---

#### Screenshot 28 — Persistence After Down/Up Cycle

Add a terminal screenshot showing that database data remains available after a non-destructive Docker Compose down/up cycle.

Do not use `docker compose down -v`.

![alt text](screenshots/Assignment-07-Task-05-screenshot-28.png)

---

#### Screenshot 29 — Persistence and Backup Document

Add a screenshot showing the contents of:

```text
docs/05-persistence-and-backup.md
```

Include the backup plan and restore procedure.

![alt text](screenshots/Assignment-07-Task-05-screenshot-29.png)

---

# Task 6 — Configure Logging and Observability

## Goal

Configure useful reverse-proxy and backend logs without exposing sensitive information.

### Evidence

#### Screenshot 30 — Logging Configuration

Add a screenshot showing:

- Configuration for the selected reverse proxy
- Proxy log format
- Docker Compose host log-directory bind mount

![alt text](screenshots/Assignment-07-Task-06-screenshot-30b.png)

---

#### Screenshot 31 — Persistent Proxy Logs and Backend Logs

Add a terminal screenshot showing:

- Selected reverse-proxy logs available from the host directory after a proxy restart
- Backend logs displayed through Docker Compose

![alt text](screenshots/Assignment-07-Task-06-screenshot-31a.png)
![alt text](screenshots/Assignment-07-Task-06-screenshot-31b.png)

---

### Notes

Write a short note covering:

- The selected reverse proxy
- Host path used for reverse-proxy logs
- How backend logs are viewed
- Whether JSON or standard text logs were used
- Why passwords, tokens, headers, and database connection strings must not appear in logs

Reverse proxy: Nginx (nginx:alpine).

Host log path: ~/theepicbook/logs/nginx/ (access.log and error.log), bind-mounted into the container's /var/log/nginx. The logs live on the VM's disk, so they survive proxy restarts and container recreation.

Backend logs: written to stdout and viewed with docker compose logs backend (add -f to follow live).

Log format: JSON. Each access log entry records the time, client IP, request line, status code, response size, response time, referrer and user agent. That makes the logs easy to filter and parse with tools.

Why secrets must not appear in logs: logs are read by many people and tools, copied to other systems and often kept for a long time, so they are much less protected than a secrets store. A password, token, Authorization header or database connection string written to a log could be read by anyone with log access and used to get into the system. The log format here records only request metadata. It does not record request headers, cookies or bodies, and the database connection string is never logged.

---

# Task 7 — Deploy and Verify the Stack on a Cloud VM

## Goal

Deploy the completed Docker Compose stack on an AWS or Azure VM and verify public access.

### Evidence

#### Screenshot 32 — VM Public IP and Inbound Rules

Add a cloud-console screenshot showing:

- VM public IP address
- SSH port 22 restricted to your IP address
- HTTP port 80 allowed from Anywhere

![alt text](screenshots/Assignment-07-Task-07-screenshot-32a.png)
![alt text](screenshots/Assignment-07-Task-07-screenshot-32b.png)

---

#### Screenshot 33 — Cloud VM Stack Verification

Add a VM terminal screenshot showing:

- Docker Compose service status
- Successful public health or API response
- No published database, frontend, or backend ports

![alt text](screenshots/Assignment-07-Task-07-screenshot-33.png)

---

#### Screenshot 34 — EpicBook Application on Cloud VM

Add a browser screenshot showing the EpicBook application loaded through the VM public IP address.

Add your full name as a clear caption below the screenshot.

![alt text](screenshots/Assignment-07-Task-07-screenshot-34.png)

---

### Notes

Write a short note covering:

- Cloud provider used
- VM operating system
- Public port exposed
- Security rules configured
- Confirmation that the application and backend API worked through the reverse proxy

Cloud provider: Microsoft Azure.

VM operating system: Ubuntu 24.04.5 LTS.

Public port exposed: only TCP 80 (HTTP), published by the reverse-proxy service. The frontend, backend and database ports are not published and are only reachable inside the Docker networks.

Security rules (NSG): inbound TCP 80 allowed from Any, and inbound TCP 22 (SSH) restricted to my own IP address. All other inbound traffic is denied by default.

Verification: the EpicBook app loaded through the VM's public IP in the browser, and /health and the API returned 200 through the reverse proxy. The stack was built and tested directly on the Azure VM from Task 1 onward, so this task focused on locking down the network rules and confirming public access.

With these three, all the Notes sections are covered. What's left is the LinkedIn post, with the backup and restore drill and the bind mounts as you wanted.

---

# Task 8 — Automate Deployment with CI/CD (Optional)

## Goal

Optionally automate image build, image push, and deployment through GitHub Actions or Azure Pipelines.

### Optional Evidence

#### Optional Screenshot — Successful CI/CD Pipeline Run

Add a screenshot showing a successful pipeline run with build, image push, deployment, and verification stages.

![alt text](screenshots/Assignment-07-Task-08-screenshot-34i.png)

---

### Optional Notes

Write a short note covering:

- CI/CD platform used
- Image-tagging method
- Registry used
- Deployment trigger
- Manual approval or secret-handling approach

CI/CD platform: Azure Pipelines, using a Microsoft-hosted ubuntu-latest agent (free tier).

Image tagging: each image is tagged with branch-name-commit-SHA (e.g. capstone-theepicbook-docker-<sha>), plus latest. The commit-based tag means every deployment can be traced back to the exact commit that produced it, and an older version can be redeployed if needed.

Registry: Docker Hub (inibehe/epicbook-backend, inibehe/epicbook-frontend).

Deployment trigger: a push to the capstone/theepicbook-docker branch that changes the backend, frontend, proxy config, compose file or pipeline file.

Secret handling: the Docker Hub token is stored as a locked secret in an Azure DevOps variable group, and the VM's SSH private key is stored as a Secure File. Neither appears in the repo or the logs. Database passwords stay in the .env file on the VM and are never handled by the pipeline. The first run failed with "access token has insufficient scopes" because the Docker Hub token was read-only. Creating a new token with write access fixed it.

Network trade-off: SSH on the VM is restricted to my IP, and Microsoft-hosted agents use changing IPs, so port 22 was opened temporarily for the deployment and restricted again afterwards. A self-hosted agent on the VM would remove the need for this.

---

# Task 9 — Perform Reliability Tests and Create an Operations Runbook

## Goal

Test controlled service failures and document safe operating procedures.

### Evidence

#### Screenshot 35 — Backend Failure and Recovery

Add a terminal screenshot showing:

- Backend failure test
- Expected unavailable response through the reverse proxy
- Backend restart
- Successful health-check recovery

![alt text](screenshots/Assignment-07-Task-08-screenshot-35.png)

---

#### Screenshot 36 — Database Failure and Recovery

Add a terminal screenshot showing:

- Database outage test
- Failed database-dependent request
- Database restart
- Successful application recovery

![alt text](screenshots/Assignment-07-Task-08-screenshot-36.png)

---

### Notes

Write a short operations runbook covering:

- Safe restart procedure for reverse proxy, frontend, backend, and database
- Backup and restore procedure
- Secret-rotation approach
- Database recovery procedure
- What to check when the application returns an error
- Results of backend and database reliability tests

Operations Runbook (full version: docs/09-runbook.md in the repo)

Safe restart: restart one service at a time with docker compose restart <service> (reverse-proxy, frontend, backend, database) and confirm it shows (healthy) with docker compose ps. Use restart when only a mounted file changed, and docker compose up -d when docker-compose.yml or .env changed, because restart does not re-read configuration.

Backup and restore: mysqldump of the bookstore database saved to backups/ on the host. Restore by piping the dump file into mysql inside the database container. Full procedure in docs/05-persistence-and-backup.md.

Secret rotation: change the DB password inside MySQL with ALTER USER, update .env, then apply with docker compose up -d backend. The Docker Hub token and SSH key are rotated in Docker Hub and in the Azure DevOps variable group / Secure Files.

Database recovery: check docker compose ps and the database logs, start the database and wait for (healthy), then test the home page. Restart the backend if it doesn't reconnect, and restore from backup if data is lost.

When the app returns an error: check docker compose ps for stopped or unhealthy services, test /health through the proxy, then read the status code. 502 means the backend is down, 500 usually means a database problem. Then check the Nginx logs and docker compose logs backend.

Reliability test results:

Backend failure: /health went from 200 to 502 while the backend was stopped, then back to 200 once it was healthy again.
Database outage: the home page went from 200 to 502 while MySQL was stopped. After MySQL restarted, the app recovered automatically to 200 with no manual backend restart.

---

# Final Public Application URL

**EpicBook URL:** http://20.219.85.115

Replace the placeholder with your working public URL.

---

# GitHub Repository URL

**Your Fork or Repository URL:** https://github.com/sundayinibehe75-afk/theepicbook-docker

---

# LinkedIn Requirement

## Goal

Create a professional LinkedIn post of 6–10 lines about your EpicBook capstone deployment.

Your post must include:

- The architectural decision that most improved reliability
- Your biggest image-size reduction, with numbers
- Key production-hardening lessons
- A deployment verification image

### Evidence

**LinkedIn Post URL:** https://www.linkedin.com/posts/emmanuel-sunday-210a08323_devops-docker-azure-ugcPost-7513565488195575808-a3fN/?utm_source=share&utm_medium=member_desktop&rcm=ACoAAFHXXywBq0IrgBBhbi5ULmCrDuZgCEYc6fQ

#### LinkedIn Post Screenshot

![alt text](screenshots/Assignment-07-Task-08-screenshot-37.png)

---

# Submission Checklist

- [ ] EpicBook repository reviewed and architecture diagram created
- [ ] Environment variables, ports, persistence, and health-check details documented
- [ ] Backend and frontend production Dockerfiles created
- [ ] Backend runs as a non-root user
- [ ] Docker image-size comparison completed
- [ ] Docker Compose stack includes reverse proxy, frontend, backend, and database
- [ ] `front-tier` and `back-tier` networks configured
- [ ] `db_data` named volume configured
- [ ] MySQL, backend, frontend, and reverse-proxy health checks configured
- [ ] Startup dependencies use `service_healthy`
- [ ] Nginx or Traefik selected as the only public reverse proxy
- [ ] Only reverse-proxy port 80 is publicly published
- [ ] Same-origin routing configured and CORS used only when required
- [ ] Backup, restore, and persistence testing completed
- [ ] Reverse-proxy and backend logs verified
- [ ] Cloud VM deployment verified through the public IP
- [ ] Backend and database reliability tests completed
- [ ] Screenshots 1–36 included
- [ ] Required notes completed
- [ ] LinkedIn post URL and screenshot included
- [ ] Full name visible in required screenshots or captions
- [ ] No passwords, tokens, private keys, account IDs, or other sensitive information exposed

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

*This submission is part of DevOps Micro Internship (DMI) — Agentic AI Track.*
