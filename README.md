
# 🚀 Jenkins CI/CD Setup and Website Hosting on AWS EC2

[![Build Status](https://img.shields.io/badge/build-passing-brightgreen)](https://github.com/iam-ganeshjadhav/Jenkins-CICD)
[![AWS](https://img.shields.io/badge/cloud-AWS-orange)](https://aws.amazon.com/)
[![Jenkins](https://img.shields.io/badge/jenkins-2.414-blue)](https://www.jenkins.io/)
[![License](https://img.shields.io/badge/license-MIT-lightgrey)](LICENSE)

This project demonstrates how to **install, configure, and automate Jenkins** on an **AWS EC2 instance (Ubuntu)** and how to **host a static website**.  
It includes all steps — from EC2 setup, Jenkins installation, pipeline creation, and web hosting — with clear explanations.

## 📘 Table of Contents

1. [Project Overview](#project-overview)
2. [Architecture Diagram](#architecture-diagram)  
3. [First Step Cross the Buildup-Process](#first-step-cross-the-buildup-proces)
4. [Requirements](#requirements)  
5. [Step-by-Step Setup](#step-by-step-setup)    
6. [Troubleshooting](#troubleshooting)  
7. [Useful Commands](#useful-commands)   
8. [Author](#author)  
9. [License](#license)

## 🧩 Project Overview

This project sets up a **Jenkins CI/CD environment** on an **AWS EC2 Ubuntu instance** and hosts an **HTML-based website** that provides step-by-step Jenkins installation and setup guidance.

**Key Features:**
- Jenkins installation using official package repository  
- Configured to run as a service  
- GitHub integration for pulling website code  
- Static website hosting via Jenkins job  
- Fully automated build and deployment pipeline  

## 🏗️ Architecture Diagram

![Diagram](IMG/1.png)

<h1> First Step Cross the Buildup-Process</h1>
## ⚙️ Requirements

| Component | Description |
|------------|-------------|
| **OS** | Ubuntu 20.04 / 22.04 LTS |
| **Cloud** | AWS EC2 instance (t2.micro recommended) |
| **Java** | OpenJDK 11 or higher |
| **Web Port** | 8080 for Jenkins, 80 for Website |
| **Tools** | Git, Nginx, Jenkins, curl, wget |

## 🧭 Step-by-Step Setup

### 1️⃣ Launch AWS EC2 Instance
- Login to **AWS Console** → **EC2 Dashboard**  <br>
- Click **Launch Instance**   <br>
- Choose **Ubuntu Server 22.04 LTS**   <br>
- Select instance type: `t2.micro` (Free Tier) <br>
  <p align="center">
  <img src="https://github.com/nikiimisal/Jenkins/blob/main/img/Screenshot%202025-10-17%20160934.png?raw=true" width="500" alt="Initialize Repository Screenshot">
</p>
- Configure security group: <br>
  - Port **22 (SSH)** → Anywhere (or My IP) <br>
  - Port **8080 (Jenkins)** → Anywhere <br>
  - Port **80 (HTTP)** → Anywhere <br>
- Launch instance and download `.pem` key <br>
<p align="center">
  <img src="https://github.com/nikiimisal/Jenkins/blob/main/img/Screenshot%202025-10-17%20161019.png?raw=true" width="500" alt="Initialize Repository Screenshot">
</p>

### 2️⃣ Connect via SSH
```bash
ssh -i your-key.pem ubuntu@<public-ip>
```

### 3️⃣ Update System and Install Java

Change the hostname so that it’s easier to identify which server it is — because the IP address isn’t always clear, and it becomes confusing to know which server you’ve logged into.

    sudo hostnamectl hostname jenkins
    exit
    ssh -i your-key.pem ubuntu@<ec2-public-ip>

<p align="center">
  <img src="https://github.com/nikiimisal/Jenkins/blob/main/img/Screenshot%202025-10-17%20162010.png?raw=true" width="500" alt="Initialize Repository Screenshot">
</p>
<br>
    
    sudo apt update
    sudo apt install -y openjdk-17-jdk
    java -version

<p align="center">
  <img src="https://github.com/nikiimisal/Jenkins/blob/main/img/Screenshot%202025-10-17%20162151.png?raw=true" width="500" alt="Initialize Repository Screenshot">
</p>

<p align="center">
  <img src="https://github.com/nikiimisal/Jenkins/blob/main/img/Screenshot%202025-10-17%20163219.png?raw=true" width="500" alt="Initialize Repository Screenshot">
</p>

### 4️⃣ Install Jenkins
```bash
wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo gpg --dearmor -o /usr/share/keyrings/jenkins-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.gpg] https://pkg.jenkins.io/debian-stable binary/" | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt update
sudo apt install -y jenkins
```
>These commands keep getting updated from time to time, so make sure to check them regularly.

  https://www.jenkins.io/doc/book/installing/linux/


### 5️⃣ Start and Enable Jenkins
```bash
sudo systemctl enable --now jenkins
sudo systemctl status jenkins
```

### 6️⃣ Configure Jenkins via Web UI
1. Copy the initial admin password:
   ```bash
   sudo cat /var/lib/jenkins/secrets/initialAdminPassword
   ```

<p align="center">
  <img src="https://github.com/nikiimisal/Jenkins/blob/main/img/Screenshot%202025-10-17%20164756.png?raw=true" width="500" alt="Initialize Repository Screenshot">
</p>
<p align="center">
  <img src="https://github.com/nikiimisal/Jenkins/blob/main/img/Screenshot%202025-10-17%20165034.png?raw=true" width="500" alt="Initialize Repository Screenshot">
</p>


2. Paste it in Jenkins UI (Unlock Jenkins)
3. Install **Suggested Plugins**
4. Create your **Admin User**
5. Continue to Jenkins Dashboard

       http://<ip>:8080

### 7️⃣ Setup Git and Clone Repository (Optional)
```bash
sudo apt install git -y
git --version
git clone https://github.com/iam-ganeshjadhav/Jenkins-CICD.git
```

### 8️⃣ Create Jenkins Job
1. Go to Jenkins Dashboard → **New Item**
   
   <p align="center">
  <img src="https://github.com/nikiimisal/Jenkins/blob/main/img/Screenshot%202025-10-17%20165456.png?raw=true" width="500" alt="Initialize Repository Screenshot">
   </p>
2. Select **Freestyle Project**<br>
3. Enter name (e.g., `Jenkins-CICD`)<br>
4. (optional) Under **Source Code Management** → Choose **Git**<br>
   Repository URL: `**https://github.com/nikiimisal/Jenkins**`<br>
5. Under **Build Steps** → Choose **Execute Shell**<br>

     sudo mkdir /home/ubuntu/nikhil-dir

6. Click **Save** → **Build Now**
  <p align="center">
  <img src="https://github.com/nikiimisal/Jenkins/blob/main/img/Screenshot%202025-10-19%20091440.png?raw=true" width="500" alt="Initialize Repository Screenshot">
</p>
7. Click consol output and see whats the problem  <br>  
8. If permissions error occurs:
<p align="center">
  <img src="https://github.com/nikiimisal/Jenkins/blob/main/img/Screenshot%202025-10-21%20122634.png?raw=true" width="500" alt="Initialize Repository Screenshot">
</p> 

### Problem Solver Section

9. Then go to linux console (Powershell Console) <br>
   --- At that point, you will need to make changes in the sudoers file to grant the necessary permissions for Jenkins.

       cd /etc
       sudo vim sudoers           or
       sudo nano sudoers
   <p align="center">
  <img src="https://github.com/nikiimisal/Jenkins/blob/main/img/Screenshot%202025-10-19%20092022.png?raw=true" width="500" alt="Initialize Repository Screenshot">
</p>     


<br>or

       sudo nano /etc/sudoers

If an error occurs while making changes to the file (when you need to write or modify it), <br>
`W10: Warning: Changing a randomly file` <br>
it means you don’t have permission to edit the file — so make sure to grant the required permissions first. <br>

10. So, grant only the specific permissions that are actually required.

        ls -l sudoers
        sudo chmod 740 sudoers
        ls -l sudoers
  <p align="center">
  <img src="https://github.com/nikiimisal/Jenkins/blob/main/img/Screenshot%202025-10-19%20092022.png?raw=true" width="500" alt="Initialize Repository Screenshot">
</p>    
   
12. Now, make the necessary changes in the sudoers file.

          jenkins ALL = (ALL:ALL) NOPASSWD:ALL

  <p align="center">
  <img src="https://github.com/nikiimisal/Jenkins/blob/main/img/Screenshot%202025-10-20%20000517.png?raw=true" width="500" alt="Initialize Repository Screenshot">
</p>     

>Earlier, whenever I practiced or ran this project, I had to make changes manually using commands. But at this point you only save that changes, & you can directly go to the Jenkins server and click **“Build”** — the issue should be resolved.
>From here on in future, the system will continue to undergo changes, so make sure to stay updated.

12. Hers is done . now check build process is succesfully proceed 
<p align="center">
  <img src="https://github.com/nikiimisal/Jenkins/blob/main/img/Screenshot%202025-10-20%20000227.png?raw=true" width="500" alt="Initialize Repository Screenshot">
</p>   

## 🧰 Troubleshooting
| Issue | Possible Fix |
|-------|--------------|
| Jenkins not starting | Check Java version with `java -version` |
| Port 8080 blocked | Update AWS security group rules |
| Permission denied | Use `sudo chmod 740`   |
| Wrong admin password | `sudo cat /var/lib/jenkins/secrets/initialAdminPassword` |
| Git fetch failed | Verify repo URL or branch name |

## 🧩 Useful Commands
```bash
sudo systemctl status jenkins
sudo systemctl restart jenkins
sudo journalctl -u jenkins -f
sudo systemctl stop jenkins
sudo apt remove --purge jenkins -y
```


## 👨‍💻 Author
**nikiimisal**  
 
📧 [Email Me](nik0misal@gmail.com)  
🌐 [GitHub Profile](https://github.com/nikiimisal)

## 🏷️ License
Open-source, free for educational purposes.
