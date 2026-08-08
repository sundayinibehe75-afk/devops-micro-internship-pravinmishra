# Assignment 5 — AI-Assisted Sprint Health Report via Jira MCP

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

In this assignment, you will connect Claude Code to your Jira board through an MCP server, the same way you connected it to GitHub in Week 2, and build a read-only `/sprint-health` skill. The skill reads your current sprint through Jira's API and reports sprint velocity, stories at risk of missing the sprint, and items missing an estimate — but it must never create, edit, comment on, or transition a single ticket itself. You will prove that boundary holds by making a real change on the board yourself and confirming the skill only ever reports, never acts.

---

# Task 1 — Create a Jira API Token

## Goal

Generate an API token from your Atlassian account that the MCP server will use to authenticate with your Jira site. Do not screenshot the token value itself.

### Evidence

#### Screenshot 1 — Jira API token creation confirmation page showing the token name, with the token value not visible

![alt text](screenshots/Assignment-05-Task-01-screenshot-01.png)

### Notes You Must Write (Very Important):

Why does the MCP server need your site URL and account email in addition to the token?

The site URL tells the server WHICH Jira instance to connect to, the email identifies WHO is making the request, and the token proves that identity is authorized. I experienced this directly — leaving JIRA_URL as a placeholder caused the connection to fail completely, since it was trying to reach a domain that doesn't exist

---

# Task 2 — Create .mcp.json at the Project Root

## Goal

Create or update `.mcp.json` at your project root with a Jira MCP server block, following the same shape as the GitHub MCP server you configured in Week 2.

### Evidence

#### Screenshot 2 — `.mcp.json` open in VS Code showing the Jira server configuration

![alt text](screenshots/Assignment-05-Task-02-screenshot-02.png)

### Notes You Must Write (Very Important):

Compare this jira block to the github block from Week 2 Assignment 5. The GitHub server ran via npx (a Node.js package); this one runs via uvx (a Python package) — what stays exactly the same shape despite that difference, and why doesn't Claude Code care which language a given MCP server is written in?

The JSON structure stays exactly the same — command, args, and env — regardless of language. Claude Code doesn't care because MCP is a standardized protocol; as long as a server correctly implements it, Claude Code communicates with it the same way, whether it's built in Python or Node.js

---

# Task 3 — Add Your Credentials to settings.local.json

## Goal

Add your Jira site URL, account email, and API token to `.claude/settings.local.json`, and confirm that file is listed in `.gitignore` so it is never committed.

### Evidence

#### Screenshot 3 — `settings.local.json` open in VS Code showing the `env` section, with the actual token value blurred or covered

![alt text](screenshots/Assignment-05-Task-03-screenshot-03.png)

### Notes You Must Write (Very Important):

Why must JIRA_API_TOKEN live in settings.local.json and never in .mcp.json?

mcp.json is meant to be committed and shared — it just describes the connection shape. settings.local.json is gitignored specifically so real credentials never get pushed to a public repo. I actually pasted a real token into this chat by mistake tonight, which is exactly why this separation matters — a leaked token in a committed file could be found and used by anyone

---

# Task 4 — Verify the Connection with /mcp

## Goal

Restart Claude Code and confirm the Jira MCP server shows as connected.

### Evidence

#### Screenshot 4 — `/mcp` output showing `jira: connected`

![alt text](screenshots/Assignment-05-Task-04-screenshot-04.png)

---

# Task 5 — Run a Live Query to Prove Real Board Data

## Goal

Ask Claude to list the issues in your current active sprint through the Jira MCP connection, and confirm the result matches what you see on your live board in the browser.

### Evidence

#### Screenshot 5 — Claude's response showing the live sprint issue list retrieved via Jira MCP

![alt text](screenshots/Assignment-05-Task-05-screenshot-05.png)

### Notes You Must Write (Very Important):

How did you confirm this was real board data and not something Claude guessed?

I compared the issue titles, statuses, and story points Claude returned against my actual Jira board open in the browser at the same time. The details matched exactly — including specific story names I hadn't mentioned in the conversation — which confirmed it was pulling real data through the MCP connection, not generating a plausible-sounding guess.

---

# Task 6 — Build the /sprint-health Skill

## Goal

Create a `/sprint-health` skill restricted to read-only Jira tools plus `Read`, with no issue-mutating tools and no `Write`. Run it and confirm it produces a report covering sprint velocity, at-risk stories, and items missing an estimate.

### Evidence

#### Screenshot 6 — `SKILL.md` frontmatter showing `allowed-tools` limited to read-only Jira tools plus `Read`, with `disable-model-invocation: true`

![alt text](screenshots/Assignment-05-Task-06-screenshot-06.png)

#### Screenshot 7 — `/sprint-health` output showing the full triage report against your real sprint

![alt text](screenshots/Assignment-05-Task-06-screenshot-07.png)

### Notes You Must Write (Very Important):

1. Which Jira MCP tools does this skill's allowed-tools list include, and which mutating tools (create issue, update issue, transition issue, add comment) does it deliberately exclude?

The allowed-tools list includes only read-focused Jira tools: jira_search, jira_get_issue, jira_get_sprint, jira_get_board, plus Read. It deliberately excludes any mutating tool — nothing that creates, updates, transitions, or comments on an issue is listed, so the skill physically cannot write to the board even if it wanted to."

2. Why does a Scrum Master need this restriction more than almost any other role in this course?

A Scrum Master relies on the board being trusted as ground truth for the whole team — velocity, burndown, and sprint health all depend on that data being accurate and only changed by deliberate human decisions. If an AI assistant could silently create, edit, or transition tickets, the Scrum Master would lose confidence that the board reflects what the team actually did, undermining the entire purpose of using it for transparency and inspection

Add your answer here

---

# Task 7 — Prove the Skill Never Mutates the Board

## Goal

Manually update one ticket on your board in the browser (for example, move a story to "Done" or add a missing estimate), then run `/sprint-health` again and confirm the new report reflects your change — proving the skill only ever reads live state and never wrote to the board itself.

### Evidence

#### Screenshot 8 — Second `/sprint-health` run showing the report now reflects your manual board change

![alt text](screenshots/Assignment-05-Task-07-screenshot-08.png)

### Notes You Must Write (Very Important):

Map this assignment to Gather → Analyze → Human Act → Verify from Week 3 Assignment 6. Which step did you perform manually in the browser, and why must that step stay human?

Gather was jira_search/jira_get_sprint pulling raw issue data. Analyze was the skill turning that into velocity, at-risk stories, and missing estimates. Human Act was me manually updating a ticket in the browser myself. Verify was re-running /sprint-health and confirming the report reflected my change. That step must stay human because only a person can be accountable for changing what the whole team treats as the source of truth — the skill can report a risk, but deciding what to actually do about it is a judgment call, not a data lookup.

---

# Submission Instructions

Complete all tasks in sequence.

Your submission must include:
- All 8 required screenshots
- All the required notes

---

# Completion Checklist

- [ ] Task 1: Jira API token created, value never screenshotted (Screenshot 1)
- [ ] Task 2: `.mcp.json` has the Jira server block (Screenshot 2)
- [ ] Task 3: Credentials stored in `settings.local.json`, token blurred, file gitignored (Screenshot 3)
- [ ] Task 4: `/mcp` shows the Jira server connected (Screenshot 4)
- [ ] Task 5: Live query returned real sprint data, verified against the browser (Screenshot 5)
- [ ] Task 6: `/sprint-health` skill created with correct read-only `allowed-tools`, and produced a full report (Screenshots 6–7)
- [ ] Task 7: A manual board change was reflected in a second `/sprint-health` run (Screenshot 8)
- [ ] Skill never created, edited, transitioned, or commented on any issue
- [ ] Reflection answered (Notes)
- [ ] No API token value exposed

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
