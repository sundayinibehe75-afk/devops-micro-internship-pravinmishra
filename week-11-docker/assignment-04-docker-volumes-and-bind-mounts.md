# Assignment 4 — Docker Volumes and Bind Mounts

Part of the DevOps Micro Internship (DMI) with Agentic AI

---

## Purpose

In this assignment, you will use Docker Bind Mounts and Docker Volumes to persist logs and application data outside a container’s lifecycle. You will verify that data remains available after containers are removed and recreated.

---

# Task 1 — Persist Nginx Logs Using a Bind Mount

## Goal

Deploy an Nginx container with a Bind Mount and verify that its log files remain on the VM host after the container is removed.

### Evidence

#### Screenshot 1 — Nginx Image Pull

Add a screenshot of the terminal showing successful completion of:

```bash
docker pull nginx:alpine
```

![alt text](screenshots/Assignment-04-Task-01-screenshot-01.png)

---

#### Screenshot 2 — Host Log Directory

Add a screenshot of the terminal showing the created host directory:

```text
$HOME/nginx-logs
```

![alt text](screenshots/Assignment-04-Task-01-screenshot-02.png)

---

#### Screenshot 3 — Running Nginx Container with Port Mapping

Add a screenshot of the terminal showing:

```bash
docker ps
```

The output must show the `myweb` container with:

```text
0.0.0.0:80->80/tcp
```

![alt text](screenshots/Assignment-04-Task-01-screenshot-03.png)

---

#### Screenshot 4 — Nginx Welcome Page

Add a browser screenshot showing the Nginx Welcome Page at:

```text
http://<YOUR-VM-PUBLIC-IP>
```

Ensure that the VM public IP is visible in the address bar. Add your full name as a clear caption directly below the screenshot.

![alt text](screenshots/Assignment-04-Task-01-screenshot-04.png)

---

#### Screenshot 5 — Bind-Mounted Log Files

Add a screenshot of the terminal showing the host log files and access-log content from:

```text
$HOME/nginx-logs
```

The output must show `access.log`, `error.log`, and an access-log entry created when you opened the Nginx page.

![alt text](screenshots/Assignment-04-Task-01-screenshot-05.png)

---

#### Screenshot 6 — Nginx Container Removed

Add a screenshot of the terminal showing successful completion of:

```bash
docker stop myweb
docker rm myweb
```

![alt text](screenshots/Assignment-04-Task-01-screenshot-06.png)

---

#### Screenshot 7 — Logs Persist After Container Removal

Add a screenshot of the terminal showing that `access.log` and `error.log` still exist in:

```text
$HOME/nginx-logs
```

The access log must retain its content after the container has been removed.

![alt text](screenshots/Assignment-04-Task-01-screenshot-07.png)

---

# Task 2 — Share Persistent Data Using a Docker Volume

## Goal

Deploy backend and frontend containers that share data through a named Docker Volume. Verify that the data remains after both containers are removed and recreated.

### Evidence

#### Screenshot 8 — Project File Structure

Add a screenshot of the terminal showing the `two-tier-app` project structure, including separate `backend` and `frontend` directories with a `Dockerfile` and `index.js` file in each.

![alt text](screenshots/Assignment-04-Task-02-screenshot-08.png)

---

#### Screenshot 9 — Custom Docker Network

Add a screenshot of the terminal showing `mynetwork` in:

```bash
docker network ls
```

![alt text](screenshots/Assignment-04-Task-02-screenshot-09.png)

---

#### Screenshot 10 — Docker Volume

Add a screenshot of the terminal showing `shared-data` in:

```bash
docker volume ls
```

![alt text](screenshots/Assignment-04-Task-02-screenshot-10.png)

---

#### Screenshot 11 — Backend Dockerfile

Add a screenshot of the terminal showing the completed backend `Dockerfile`.

![alt text](screenshots/Assignment-04-Task-02-screenshot-11.png)

---

#### Screenshot 12 — Backend Image Build

Add a screenshot of the terminal showing successful completion of the `backend-app:latest` image build.

![alt text](screenshots/Assignment-04-Task-02-screenshot-12.png)

---

#### Screenshot 13 — Running Backend Container

Add a screenshot of the terminal showing:

```bash
docker ps
```

The output must show the running `backend` container.

![alt text](screenshots/Assignment-04-Task-02-screenshot-13.png)

---

#### Screenshot 14 — Frontend Dockerfile

Add a screenshot of the terminal showing the completed frontend `Dockerfile`.

![alt text](screenshots/Assignment-04-Task-02-screenshot-14.png)

---

#### Screenshot 15 — Frontend Image Build

Add a screenshot of the terminal showing successful completion of the `frontend-app:latest` image build.

![alt text](screenshots/Assignment-04-Task-02-screenshot-15.png)

---

#### Screenshot 16 — Running Backend and Frontend Containers

Add a screenshot of the terminal showing:

```bash
docker ps
```

The output must show both `backend` and `frontend` containers running. Only `frontend` must have the published port mapping:

```text
0.0.0.0:80->80/tcp
```

![alt text](screenshots/Assignment-04-Task-02-screenshot-16.png)

---

#### Screenshot 17 — Backend Write Operation

Add a screenshot of the terminal showing a successful backend write operation to the shared Docker Volume.

The output must include:

```text
Data written: Hello from Backend!
```

![alt text](screenshots/Assignment-04-Task-02-screenshot-17.png)

---

#### Screenshot 18 — Frontend Reads Shared Data

Add a browser screenshot showing:

```text
Hello from Backend!
```

Add your full name as a clear caption directly below the screenshot.

![alt text](screenshots/Assignment-04-Task-02-screenshot-18.png)

---

#### Screenshot 19 — First Shared-Data Update

Add a browser screenshot showing:

```text
Test Data 1
```

Add your full name as a clear caption directly below the screenshot.

![alt text](screenshots/Assignment-04-Task-02-screenshot-19.png)

---

#### Screenshot 20 — Second Shared-Data Update

Add a browser screenshot showing:

```text
Test Data 2 - New Update
```

Add your full name as a clear caption directly below the screenshot.

![alt text](screenshots/Assignment-04-Task-02-screenshot-20.png)

---

#### Screenshot 21 — Container Removal and Recreation

Add a screenshot of the terminal showing the `frontend` and `backend` containers removed and recreated using the same `shared-data` Docker Volume.

![alt text](screenshots/Assignment-04-Task-02-screenshot-21.png)

---

#### Screenshot 22 — Data Persists After Recreation

Add a browser screenshot showing:

```text
Test Data 2 - New Update
```

This proves that the `shared-data` Docker Volume outlived both application containers.

Add your full name as a clear caption directly below the screenshot.

![alt text](screenshots/Assignment-04-Task-02-screenshot-22.png)

---

# Storage Persistence Notes

Write a short explanation covering:

- The difference between a Bind Mount and a Docker Volume
- How Task 1 proved Bind Mount persistence
- How Task 2 proved Docker Volume persistence
- Why Docker Volumes are commonly used for application data

Bind Mount vs Docker Volume: A Bind Mount maps a specific directory on the host into a container. I chose the host path myself ($HOME/nginx-logs), and the files can be read or edited directly on the VM. A Docker Volume is storage created and managed by Docker (docker volume create shared-data). It's referenced by name instead of a host path, and Docker stores it in its own area on the VM (/var/lib/docker/volumes/shared-data/_data). Both keep data outside the container's writable layer, so the data survives when a container is removed.

How Task 1 proved Bind Mount persistence: I ran the myweb Nginx container with $HOME/nginx-logs mounted to Nginx's log directory. Opening the welcome page created an entry in access.log, and both access.log and error.log appeared on the VM. After docker stop myweb and docker rm myweb, both files were still in $HOME/nginx-logs, and access.log still contained the request entry. The logs were written to the host, not to the container, so they outlived the container.

How Task 2 proved Docker Volume persistence: The backend and frontend containers both mounted the same shared-data volume, with the backend using it at /data. The backend wrote "Hello from Backend!" to /data/message.txt, and the frontend read the file and displayed it in the browser. I then updated the file through the backend with docker exec backend sh -c "echo 'Test Data 1' > /data/message.txt", and later "Test Data 2 - New Update". After each change, refreshing the browser showed the new text straight away, which proved both containers were reading the same storage. Finally, I removed both containers and recreated them with the same shared-data volume. The browser still showed "Test Data 2 - New Update", which proves the data lived in the volume, not in either container.

Why Docker Volumes are commonly used for application data: Docker manages volumes, so they don't depend on a specific folder layout on the host, and they avoid most path and permission problems. Several containers can share one volume, as the backend and frontend did here. Volumes are easy to back up, move, and inspect with Docker commands, and they can use drivers for network or cloud storage. That makes them the standard choice for data an application creates and must keep, such as database files, uploads, and shared application state. Bind Mounts are better suited to files the host provides, like configuration files or logs collected on the host.

---

# Public Application URL

**Application URL:** http://20.219.85.115

---

# LinkedIn Requirement

## Goal

Create a LinkedIn post about Docker Volumes and Bind Mounts, including one difference between them, how you verified persistent storage, and your key learning outcomes.

### Evidence

#### LinkedIn Post URL

https://www.linkedin.com/posts/emmanuel-sunday-210a08323_docker-devops-containers-ugcPost-7511884373173112833-0NCw/?utm_source=share&utm_medium=member_desktop&rcm=ACoAAFHXXywBq0IrgBBhbi5ULmCrDuZgCEYc6fQ

---

#### LinkedIn Post Screenshot

![alt text](screenshots/Assignment-04-Task-02-screenshot-23.png)

---

# Submission Instructions

- Complete all tasks in sequence.
- Include Screenshots 1–22 exactly as specified.
- Include the Storage Persistence Notes section.
- Include the public application URL.
- Include the LinkedIn post URL and screenshot.
- Ensure that your full name is visible in all terminal screenshots.
- Add your full name as a clear caption below every browser screenshot.
- Do not expose private keys, passwords, access keys, tokens, account IDs, or other sensitive information.

---

# Completion Checklist

- [ ] Nginx image pulled successfully
- [ ] Host log directory created
- [ ] Bind Mount configured successfully
- [ ] Nginx logs remain after container removal
- [ ] Custom Docker network created
- [ ] Docker Volume created
- [ ] Backend Dockerfile and image created
- [ ] Frontend Dockerfile and image created
- [ ] Both containers mount `shared-data`
- [ ] Backend writes data to the Docker Volume
- [ ] Frontend reads the same data from the Docker Volume
- [ ] Updated data appears after browser refresh
- [ ] Data remains after frontend and backend containers are removed and recreated
- [ ] All required screenshots included
- [ ] Storage Persistence Notes completed
- [ ] Public application URL included
- [ ] LinkedIn post URL and screenshot included
- [ ] Full name visible in terminal screenshots
- [ ] Browser screenshots have full-name captions
- [ ] No sensitive information exposed

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
