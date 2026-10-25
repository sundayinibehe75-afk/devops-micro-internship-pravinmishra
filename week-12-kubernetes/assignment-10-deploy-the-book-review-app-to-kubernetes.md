# Assignment 10 — Capstone: Deploy the Book Review App to Kubernetes

Part of the DevOps Micro Internship (DMI) with Agentic AI

---

## Purpose

Deploy the Book Review App as a production-style three-tier application using Kubernetes manifests or Helm charts, with an accessible frontend, an internal backend, and a managed or persistent MySQL database.

---

## Submission Details

**Full Name:** Inibehe Emmanuel Sunday

**Kubernetes Platform:** Azure Kubernetes service in Microsoft Azure 

**Namespace:** book-review

**Frontend URL or IP:** http://20.235.33.65

**Access Method:** Public LoadBalancer

**GitHub Manifests or Helm Chart URL:** https://github.com/sundayinibehe75-afk/book-review-app-aks

**Architecture Diagram:** ![alt text](screenshots/Assignment-10-Task-02-screenshot-03.png)

---

# Task 1 — Prepare the Kubernetes Cluster and Project

## Goal

Prepare the cluster, application source, container images, and tooling needed for the deployment.

### Evidence

#### Screenshot 1 — Active Kubernetes context and Ready nodes

![alt text](screenshots/Assignment-10-Task-01-screenshot-01.png)

---

#### Screenshot 2 — If using Helm: Output of `helm version`

Required only if you choose Helm. Skip this screenshot if using YAML manifests.

Not applicable — using YAML manifests.

---

# Task 2 — Design the Three-Tier Kubernetes Architecture

## Goal

Document the workloads, networking, configuration, storage, and permitted communication paths before deployment.

### Evidence

#### Screenshot 3 — Completed Kubernetes architecture diagram

![alt text](screenshots/Assignment-10-Task-02-screenshot-03.png)

---

# Task 3 — Create ConfigMaps and Secrets

## Goal

Provide application configuration and credentials without embedding sensitive values in images or committing them to Git.

### Evidence

#### Screenshot 4 — ConfigMap configuration or describe output showing non-sensitive values

![alt text](screenshots/Assignment-10-Task-03-screenshot-04.png)

---

#### Screenshot 5 — Secret references or describe output showing names and keys without sensitive values

![alt text](screenshots/Assignment-10-Task-03-screenshot-05.png)

---

# Task 4 — Deploy and Configure the MySQL Data Tier

## Goal

Provide persistent MySQL storage and allow the backend to connect without exposing the database publicly.

### Evidence

#### Screenshot 6 — Managed MySQL status or MySQL StatefulSet and PVC status

![alt text](screenshots/Assignment-10-Task-04-screenshot-06.png)

---

#### Screenshot 7 — Successful database connectivity or schema verification with credentials hidden

![alt text](screenshots/Assignment-10-Task-04-screenshot-07.png)

---

# Task 5 — Deploy the Backend Application Tier

## Goal

Run one or two Node.js/Express backend replicas on port 3001 and expose them through an internal ClusterIP Service.

### Evidence

#### Screenshot 8 — Backend Deployment, ready Pods, and internal Service

![alt text](screenshots/Assignment-10-Task-05-screenshot-08.png)

---

#### Screenshot 9 — Backend startup logs and database connectivity evidence

If the application does not log successful database connections, provide a successful database-backed request alongside the startup logs.

![alt text](screenshots/Assignment-10-Task-05-screenshot-09.png)

---

# Task 6 — Deploy the Frontend and Configure Nginx

## Goal

Run the Next.js frontend behind Nginx on port 80 and proxy API requests to the internal backend Service.

### Evidence

#### Screenshot 10 — Frontend Deployment and Ready Pods

![alt text](screenshots/Assignment-10-Task-06-screenshot-10.png)

---

#### Screenshot 11 — Nginx reverse-proxy configuration showing the internal backend Service name and port 3001

![alt text](screenshots/Assignment-10-Task-06-screenshot-11.png)

---

# Task 7 — Configure External and Internal Networking

## Goal

Expose the frontend and enforce the intended communication path between application tiers.

### Evidence

#### Screenshot 12 — Frontend LoadBalancer or Ingress configuration and assigned endpoint, or documented local access method

![alt text](screenshots/Assignment-10-Task-07-screenshot-12.png)

---

#### Screenshot 13 — Internal backend Service and NetworkPolicy or equivalent restriction, with access-test evidence

![alt text](screenshots/Assignment-10-Task-07-screenshot-13.png)

---

# Task 8 — Validate the Application End to End

## Goal

Prove that the frontend, backend, and MySQL database work together and collect operational evidence.

### Evidence

#### Screenshot 14 — `kubectl get all` output for the book-review namespace

![alt text](screenshots/Assignment-10-Task-08-screenshot-14.png)

---

#### Screenshot 15 — Functional Book Review App in the browser with the URL visible, including evidence of a successful user flow

![alt text](screenshots/Assignment-10-Task-08-screenshot-15.png)

---

#### Screenshot 16 — Frontend or Nginx container logs

![alt text](screenshots/Assignment-10-Task-08-screenshot-16.png)

---

#### Screenshot 17 — Backend logs and evidence of a successful database-backed operation

![alt text](screenshots/Assignment-10-Task-08-screenshot-17.png)

---

#### Screenshot 18 — Optional: Kubernetes dashboard

![alt text](screenshots/Assignment-10-Task-08-screenshot-18.png)

Multiple images may be included under a screenshot number when needed to demonstrate the complete user flow.

---

# Task 9 — Optional: Implement Production Improvements

## Goal

Add selected improvements without breaking the required three-tier architecture.

### Evidence

#### Screenshot 19 — Optional: Evidence of selected Helm, HPA, probe, or TLS improvements

![alt text](screenshots/Assignment-10-Task-09-screenshot-19.png)

---

# Task 10 — Publish Your Book Review App Deployment on LinkedIn

## Goal

Explain your three-tier architecture, demonstrate the working application, and share what you learned.

### Evidence

#### Screenshot 20 — Published LinkedIn post about your Book Review App deployment on Kubernetes

![alt text](screenshots/Assignment-10-Task-10-screenshot-20.png)

### LinkedIn Post URL

https://www.linkedin.com/posts/emmanuel-sunday-210a08323_kubernetes-azure-aks-ugcPost-7519498055889588226-oE9H/?utm_source=share&utm_medium=member_desktop&rcm=ACoAAFHXXywBq0IrgBBhbi5ULmCrDuZgCEYc6fQ

---

# Final Summary

## What Worked

Describe the application components and user flows you successfully deployed and tested.

The Book Review App capstone involved deploying a containerized application consisting of a Next.js frontend, a Node.js backend API, and a MySQL database. Docker was used to package the application components, while Kubernetes manifests were created to manage application deployment and networking.

The Kubernetes configuration included Deployments, Services, a MySQL StatefulSet, persistent volume storage, ConfigMaps, and a Secret example. Additional manifests were prepared for NGINX, backend autoscaling, and network policies.

The application components and Kubernetes configuration were developed and worked on during the assignment. Only the user flows and components that were successfully verified in the running environment should be considered fully tested.

## Issues and Fixes

Describe the main issues encountered, their causes, and how you resolved them.

Several challenges were encountered during the assignment:

Docker image builds: Building the backend image from the Node.js base image took a considerable amount of time. The build was allowed to complete before proceeding.

Kubernetes configuration: Deployment, Service, and StatefulSet configurations required careful attention to labels, selectors, ports, and resource relationships.

Persistent storage: The MySQL configuration required a PersistentVolumeClaim to support persistent database storage.

Application configuration: Frontend and backend communication required the correct API URL and appropriate service configuration.

Environment differences: Some instructions needed to be adapted to the Azure Kubernetes environment rather than followed exactly as AWS EKS commands.

These challenges reinforced the importance of checking application configuration, image builds, Kubernetes resource status, logs, and service connectivity when troubleshooting deployments.

## Tools and Sources Used

List the documentation and tools that helped you complete the assignment.

Microsoft Azure / Azure Kubernetes Service (AKS): Kubernetes hosting and cluster management.

Docker: Building container images for the frontend and backend.

Kubernetes and kubectl: Managing and inspecting application resources.

Git and GitHub: Version control and source-code documentation.

Visual Studio Code: Editing application code, Dockerfiles, and Kubernetes manifests.

Node.js and Next.js: Backend API and frontend application technologies.

MySQL: Relational database.

Official Docker documentation: https://docs.docker.com/

Kubernetes documentation: https://kubernetes.io/docs/

Microsoft Azure documentation: https://learn.microsoft.com/azure/

GitHub repository: https://github.com/sundayinibehe75-afk/book-review-app-aks

## Optional Improvements

Describe any optional improvements implemented, or write “Not attempted.”

Additional improvements included preparing manifests for backend horizontal pod autoscaling, network policies, and NGINX-based traffic routing. These configurations were included in the project repository; their operational behaviour should be confirmed through testing before describing them as fully implemented and verified.

Further improvements could include comprehensive application testing, database backup and recovery, monitoring and alerting, secure secret management, and automated CI/CD deployment.

## Cleanup or Reuse Plan

State which resources were removed or why the environment is being retained.

The Azure Kubernetes environment is being retained to complete Assignment 11 — AI-Assisted Kubernetes Incident Triage. The existing Book Review App deployment will be reused to establish a healthy baseline, simulate a controlled failure, collect diagnostic evidence using a read-only Bash script, and analyze the results with Claude Code through the /k8s-triage skill. Recovery will be performed manually, followed by verification and documentation. The environment will be reviewed after the exercise to determine whether any resources can be safely removed and to manage ongoing cloud costs.

---

# Submission Instructions

- Include your Kubernetes manifests or Helm chart in the linked repository.
- Submit Secret examples containing placeholders only.
- Include Screenshot 1, Screenshots 3–17, and Screenshot 20.
- Include Screenshot 2 only if using Helm.
- Screenshots 18–19 are optional.
- Keep screenshot numbering consistent with the guidelines.
- Use clear, readable screenshots showing relevant configuration, commands, and outputs.
- Make your full name visible in required screenshots.
- Complete the submission details and Final Summary.
- Include your published LinkedIn post URL.
- Capture required evidence before cleanup.
- Do not expose passwords, tokens, JWT secrets, actual Secret values, kubeconfig contents, cloud credentials, or confidential account information.

---

# Completion Checklist

- [ ] Selected and configured a supported Kubernetes platform
- [ ] Confirmed the context and Ready nodes (Screenshot 1)
- [ ] Created and selected the book-review namespace
- [ ] Inspected the application repository
- [ ] Prepared images accessible to the cluster
- [ ] Confirmed Helm if using charts (Screenshot 2, conditional)
- [ ] Created the architecture diagram (Screenshot 3)
- [ ] Created ConfigMaps and Secrets (Screenshots 4–5)
- [ ] Excluded actual Secret values from Git and screenshots
- [ ] Provisioned private managed MySQL or persistent Kubernetes MySQL (Screenshot 6)
- [ ] Initialized the required database and schema
- [ ] Verified database connectivity or schema (Screenshot 7)
- [ ] Deployed one or two backend replicas listening on port 3001
- [ ] Created the backend ClusterIP Service (Screenshot 8)
- [ ] Verified backend-to-database connectivity (Screenshot 9)
- [ ] Deployed one or two frontend replicas behind Nginx (Screenshot 10)
- [ ] Configured API proxying through the internal backend Service (Screenshot 11)
- [ ] Configured and documented frontend access (Screenshot 12)
- [ ] Enforced backend access restrictions (Screenshot 13)
- [ ] Confirmed that the backend and database are not publicly exposed
- [ ] Captured resource status for book-review (Screenshot 14)
- [ ] Tested application functionality and database writes (Screenshots 15 and 17)
- [ ] Verified saved data after application restart
- [ ] Captured frontend and backend logs (Screenshots 16–17)
- [ ] Included optional dashboard or improvement evidence if attempted (Screenshots 18–19)
- [ ] Published the LinkedIn post (Screenshot 20)
- [ ] Included the LinkedIn post URL
- [ ] Documented issues, fixes, and useful sources
- [ ] Recorded cleanup status or reuse plans
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

*This submission is part of DevOps Micro Internship (DMI) — Agentic AI Track.*
