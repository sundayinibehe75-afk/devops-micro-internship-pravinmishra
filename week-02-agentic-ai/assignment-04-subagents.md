# Assignment 4 — Building Your AI Team

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

In this assignment, you will build and configure a set of specialized AI subagents inside your project. You will learn how different models and tool permissions define agent behavior, and you will trigger two real agent delegations to analyze security and cost aspects of your Terraform infrastructure.

---

# Task 1 — Create the Agents Folder and Add Files

## Goal

Create the `.claude/agents/` directory and add all required agent files.

### Evidence

#### Screenshot 1 — VS Code sidebar showing `.claude/agents/` with all 3 files

![alt text](screenshots/assignment-04-screenshot-01.png)

# Task 2 — Compare the Agent Configurations

## Goal

Analyze the configuration differences between the three agents and demonstrate understanding of model and tool selection.

### Written Answers

#### 1. Why does the cost optimizer use Haiku instead of Sonnet?

Cost Optimizer sub agent uses Haiku instead of sonnet because of its speed in performing tasks such as repetitive tasks, and generating responses in milliseconds as well as the cheapest model compared to sonnet that is general purpose for everyday used tasks.

#### 2. Why does the security auditor NOT have Write in its tools list?

Security auditor sub agent only need read tools to audit terraform infrastructure, because it checks security related issues in the infrastructure and generated possible findings, recommendations on how to fix security issues without modifying the infrastructure.

#### 3. Why does the tf-writer use `inherit` instead of a specific model?

Using model: inherit lets the tf-writer instantly match the main terminal's reasoning power. If the Terraform task gets complex, i can can upgrade the subagent's intelligence on the fly just by changing my parent session. This keeps the workflow flexible and avoids hardcoding models that might become too weak or too expensive.

### Evidence

#### Screenshot 2 — `security-auditor.md` frontmatter showing model and tools configuration

![alt text](screenshots/assignment-04-screenshot-02.png)

#### Screenshot 3 — `cost-optimizer.md` frontmatter showing the model and tools configuration

![alt text](screenshots/assignment-04-screenshot-03.png)

---

# Task 3 — Run the Security Auditor

## Goal

Trigger the security auditor agent and analyze the generated security report for your Terraform infrastructure.

### Evidence

#### Screenshot 4 — The delegation message showing Claude launched the security-auditor

![alt text](screenshots/assignment-04-screenshot-04.png)

---

#### Screenshot 5 — Security audit report output

![alt text](screenshots/assignment-04-screenshot-05.png)

---

# Task 4 — Run the Cost Optimizer

## Goal

Trigger the cost optimizer agent and review the generated cost optimization report.

### Evidence

#### Screenshot 6 — The full cost optimization report

![alt text](screenshots/assignment-04-screenshot-06.png)

---

# Submission Instructions

- Ensure all agent files are committed in `.claude/agents/`
- Complete all written answers in your GitHub Repo
- Push final changes to your forked GitHub repository

---

## GitHub Repository URL

<<<<<<< HEAD
https://github.com/sundayinibehe75-afk/Ultimate-Agentic-DevOps-with-Claude-Code
=======
Paste your forked repository URL here:

`Add your URL here`
>>>>>>> upstream/main

---

# Completion Checklist

- [ ] `.claude/agents/` folder contains all 3 agent files
- [ ] Screenshot 2 shows correct `security-auditor.md` configuration
- [ ] Screenshot 3 shows correct `cost-optimizer.md` configuration
- [ ] All 3 written answers completed 
- [ ] Security auditor executed successfully
- [ ] Cost optimizer executed successfully
- [ ] Security report is visible with findings
- [ ] Cost report is visible with recommendations
- [ ] All required screenshots added
- [ ] GitHub repo updated with agents

---

## 📌 About DMI & CloudAdvisory

DevOps Micro Internship (DMI) is a project-based DevOps program run by Pravin Mishra (The CloudAdvisory) focused on real-world execution, systems thinking, and career readiness.

It helps learners build strong DevOps foundations with hands-on experience.

---

## 📌 Resources

- 🌐 DMI Official Website: https://pravinmishra.com/dmi  
- 🎓 DevOps for Beginners (Udemy): https://www.udemy.com/course/devops-for-beginners-docker-k8s-cloud-cicd-4-projects/  
- 🎓 Agentic AI DevOps with Claude Code: https://www.udemy.com/course/ultimate-agentic-ai-devops-with-claude-code/  
- 🎓 DevOps with Claude Code: Terraform, EKS, ArgoCD & Helm: https://www.udemy.com/course/devops-with-claude-code-terraform-eks-argocd-helm/  
- ▶️ YouTube Playlist: https://www.youtube.com/playlist?list=PLFeSNDtI4Cho  
- 🔗 Pravin Mishra (LinkedIn): https://www.linkedin.com/in/pravin-mishra-aws-trainer/  
- 🏢 CloudAdvisory (LinkedIn): https://www.linkedin.com/company/thecloudadvisory/

---

*This submission is part of DevOps Micro Internship (DMI) Cohort 3 — Agentic AI Track.*