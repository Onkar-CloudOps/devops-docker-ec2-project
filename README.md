# 🌙 Dark Theme Website – AWS EC2 & Docker Deployment

## 📌 Project Overview

This project demonstrates deploying a **Dark Theme Website ("DailyNeeds" – a neighbourhood grocery store landing page)** using Docker containers on an AWS EC2 instance running Amazon Linux.

I created an Amazon Linux EC2 instance (`MyWebServer`), installed Docker, pulled a pre-built Docker image from Docker Hub, and created a container to host the website.

Inside the Docker container, the website files live in `/var/www/html` and are served by the Apache2 web server. The container's port 80 is mapped to port **90** on the EC2 instance, so the site is reachable through the instance's public IP address over HTTP.

The project also demonstrates Git version control and GitHub repository management, including resolving a merge with the remote `main` branch.

## 🛠️ Technologies & Tools

| Technology | Purpose |
|---|---|
| AWS EC2 | Cloud virtual server |
| Amazon Linux | Operating system |
| Docker | Containerization |
| Docker Hub | Docker image repository |
| Apache2 | Web server |
| HTML & CSS | Website development |
| Git | Version control |
| GitHub | Source code repository |
| Linux | Server administration |

## 🏗️ Deployment Architecture

```text
                  Internet
                     |
                     |
              AWS EC2 Instance
           (Amazon Linux, t3.micro)
                     |
                     |
                   Docker
                     |
              Docker Container
                     |
                 Apache2
                 Web Server
                     |
                Website Files
          (/var/www/html/index.html)
                     |
               Container Port 80
                     |
                EC2 Port 90
                     |
               Public IP Address
                     |
                 Web Browser
```

## 🚀 Implementation Steps

### 1. AWS EC2 Instance Creation

- Created an Amazon Linux EC2 instance named **MyWebServer** (`t3.micro`) in the `us-east-2` (Ohio) region.
- Configured the EC2 security group.
- Enabled SSH access.
- Connected to the instance using SSH.
- Retrieved the EC2 public IPv4 address.

The instance is in the **Running** state with **3/3 status checks passed**.

### 2. Docker Installation and Configuration

Installed and configured Docker on the Amazon Linux EC2 instance.

```bash
sudo yum update -y

sudo yum install docker -y

sudo systemctl start docker

sudo systemctl enable docker

sudo systemctl status docker
```

### 3. Pull Docker Image from Docker Hub

Pulled the required Docker image from Docker Hub.

```bash
docker pull onkarborse11/image11

docker images
```

### 4. Create and Run Docker Container

Created a Docker container using the downloaded image and mapped container port 80 to host port 90.

```bash
docker run --name container1 -p 90:80 -it onkarborse11/image11 bash
```

Verify the container:

```bash
docker ps -a
```

The full sequence of commands run on the EC2 instance (install Docker, pull the images, install Git, and run the container) can be viewed with the `history` command.

### 5. Apache2 Web Server Configuration

Inside the container, started the Apache2 web server so it serves the files from `/var/www/html`:

```bash
service apache2 start

service apache2 status
```

The website's `index.html` (dark theme, custom CSS variables, Google Fonts) is located at `/var/www/html/index.html`.

### 6. EC2 Security Group Configuration

Configured the inbound security group rules:

| Type | Protocol | Port | Source |
|---|---|---|---|
| SSH | TCP | 22 | My IP |
| Custom TCP | TCP | 90 | 0.0.0.0/0 |

### 7. Access the Deployed Website

Opened the website using the EC2 public IP address and the mapped port:

```text
http://3.141.106.235:90
```

The Docker container serves the website through Apache2, and the EC2 instance forwards external HTTP traffic on port 90 to port 80 in the container.

## 🐳 Useful Docker Commands

```bash
# List Docker images
docker images

# List running containers
docker ps

# List all containers
docker ps -a

# Pull image
docker pull IMAGE_NAME

# Start container
docker start container1

# Stop container
docker stop container1

# Access container
docker exec -it container1 bash

# View container logs
docker logs container1

# Remove container
docker rm -f container1
```

## 🔄 Git & GitHub Integration

Maintained the website project using Git and GitHub, directly from the website folder inside the container (`/var/www/html`).

### 1. Initialize the repository

```bash
cd /var/www/html
ls
git init
git status
```

`git init` creates the repository, and `git status` shows `index.html` as an untracked file.

### 2. Stage the file

```bash
git add index.html
git status
```

`index.html` now appears under "Changes to be committed".

### 3. Configure identity and commit

The first commit failed with **"Author identity unknown"**, so I set my Git identity and committed again:

```bash
git config --global user.email "onkarborse11@gmail.com"
git config --global user.name "Onkar-CloudOps"

git commit -m "Index.html file for dark theme website"
git status
```

The commit succeeded (204 insertions) and the working tree is clean.

### 4. Connect to GitHub, merge, and push

```bash
git remote add origin https://github.com/Onkar-CloudOps/dark-theme-website.git

git branch -M main

git pull origin main --allow-unrelated-histories

git commit -m "Merge remote main branch"

git push -u origin main
```

The remote repository already contained a `README.md`, so the local and remote histories had to be merged before pushing. After resolving the merge and committing, the push to `origin/main` succeeded and the local `main` branch now tracks `origin/main`.

> 🔐 **Note:** GitHub no longer accepts account passwords for HTTPS pushes. Use a **Personal Access Token (PAT)** in place of the password when prompted.

## 🧯 Troubleshooting Notes

| Issue | Fix |
|---|---|
| `Author identity unknown` on `git commit` | Set `user.name` and `user.email` with `git config --global` |
| Push rejected / merge needed because remote had a `README.md` | Pull the remote `main`, resolve the merge, commit, then push |
| Website not loading in the browser | Confirm the container is running, Apache2 is started, and port **90** is open in the security group |

📸 Important Screenshots During the Execution of This Project

AWS EC2 instance (MyWebServer) in the Running state

<p align="center"> <img src="https://github.com/user-attachments/assets/cbcd5c6f-4957-403c-949c-3c79f9e7d44e" alt="EC2 instance running" width="850" height="650"> </p>

Commands executed on the EC2 instance

<p align="center"> <img src="https://github.com/user-attachments/assets/7ab0e0de-8f58-486e-ac31-cb347d400be9" alt="Command history on EC2" width="850" height="650"> </p>

Website index.html inside the Docker container

<p align="center"> <img src="https://github.com/user-attachments/assets/7f28026a-6fee-40b0-8227-9ee118a690fe" alt="index.html inside the container" width="850" height="650"> </p>

Git repository initialized

<p align="center"> <img src="https://github.com/user-attachments/assets/7f0f358f-7c62-4d61-b2c1-ef732246d31f" alt="git init and git status" width="850" height="650"> </p>

File staged with git add

<p align="center"> <img src="https://github.com/user-attachments/assets/5f3f3125-cd53-4944-8319-877166a04f3a" alt="git add" width="850" height="650"> </p>

Git identity configured and first commit created

<p align="center"> <img src="https://github.com/user-attachments/assets/6b469ffa-8368-4e89-92bc-51361140f177" alt="git config and git commit" width="850" height="650"> </p>

Website live at the EC2 public IP on port 90

<p align="center"> <img src="https://github.com/user-attachments/assets/67ac1dff-f0fb-406b-8a24-d42e26e5d1b5" alt="DailyNeeds website live on EC2" width="850" height="650"> </p>

## 🎯 Key Learning Outcomes

- AWS EC2 instance provisioning and configuration.
- Amazon Linux administration.
- Docker installation and configuration.
- Docker Hub image pulling and management.
- Docker container creation and execution.
- Apache2 web server configuration inside a container.
- Docker port mapping and networking (`-p 90:80`).
- AWS Security Group configuration.
- Website hosting using an EC2 public IP address.
- Git version control, merge handling, and GitHub integration.

## 📂 GitHub Repository

[Dark Theme Website – Onkar-CloudOps](https://github.com/Onkar-CloudOps/dark-theme-website)

## 👨‍💻 Author

**Onkar-CloudOps**

AWS Cloud | Linux | Docker | Git | GitHub | DevOps

---

⭐ If you find this project useful, feel free to explore the repository!
