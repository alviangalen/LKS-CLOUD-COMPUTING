# Hands-on Lab: Deploying Containerized Web App on AWS EC2 & Custom VPC

## Objective
In this practical assignment, you will learn the foundational concepts of Cloud Networking and DevOps operations:
1. Designing a custom cloud network (VPC, Subnet, Internet Gateway, Route Table).
2. Provisioning an Ubuntu-based virtual machine (`t3.micro`).
3. Configuring the host operating system with Git, Docker, and Docker Compose.
4. Deploying a containerized full-stack application from a remote Git repository.

---

## Prerequisites
- An active AWS Account (Free Tier eligible).
- Basic familiarity with bash/terminal commands.
- An SSH client (Terminal, PuTTY, or AWS EC2 Instance Connect).

---

## Task Instructions

### Step 1: Network Infrastructure Setup (VPC)
Build an isolated network environment for your workload:

1. **Create a Virtual Private Cloud (VPC):**
   - **Name tag:** `yourname-vpc`
   - **IPv4 CIDR block:** `10.0.0.0/16`
2. **Create a Public Subnet:**
   - **VPC:** Select `yourname-vpc`
   - **Subnet name:** `yourname-public-subnet-1`
   - **Availability Zone:** Choose any available AZ in your region (e.g., `us-east-1a` or `ap-southeast-1a`)
   - **IPv4 CIDR block:** `10.0.1.0/24`
   - **Auto-assign public IP:** Enable **"Auto-assign public IPv4 address"** in subnet settings.
3. **Create and Attach an Internet Gateway (IGW):**
   - **Name tag:** `yourname-igw`
   - Attach `yourname-igw` to your `yourname-vpc`.
4. **Configure Route Table:**
   - **Name tag:** `yourname-public-rt`
   - **VPC:** `yourname-vpc`
   - Add a route:
     - **Destination:** `0.0.0.0/0`
     - **Target:** Internet Gateway (`yourname-igw`)
   - **Subnet Associations:** Associate `yourname-public-subnet-1` with this route table.

---

### Step 2: Configure Security Group
Create a virtual firewall to control inbound traffic to your server:

1. Create a Security Group named `yourname-music-app-sg` inside `yourname-vpc`.
2. Configure **Inbound Rules**:
   - **SSH:** Port `22` | Source: `0.0.0.0/0` (Anywhere IPv4)
   - **HTTP:** Port `80` | Source: `0.0.0.0/0` (Anywhere IPv4)
3. Keep default **Outbound Rules** (`All traffic` to `0.0.0.0/0`).

---

### Step 3: Launch an EC2 Instance
1. Navigate to the **EC2 Console** and click **Launch Instances**.
2. **Name:** `yourname-music-app-server`
3. **Operating System (AMI):** Ubuntu Server (22.04 LTS or 24.04 LTS, 64-bit x86).
4. **Instance Type:** `t3.micro` (Free tier eligible).
5. **Key Pair:** Select an existing key pair or generate a new `.pem` key pair.
6. **Network Settings** (Click *Edit*):
   - **VPC:** `yourname-vpc`
   - **Subnet:** `yourname-public-subnet-1`
   - **Auto-assign public IP:** `Enable`
   - **Firewall (Security Groups):** Select existing security group `yourname-music-app-sg`.
7. **Storage:** Default `8 GiB gp3` is sufficient.
8. Click **Launch Instance**.

---

### Step 4: Server Setup (Docker & Git Installation)
Once your instance is in the `Running` state:

1. Connect to your instance via AWS console:
   - Select your instance
   - click connect at the top
   - choose EC2 Instance connect, then click connect button on the botom
   
2. Update package repositories:
   ```bash
   sudo apt-get update && sudo apt-get upgrade -y
   ```
3. Install **Git**:
   ```bash
   sudo apt-get install -y git
   ```
4. Install **Docker** and **Docker Compose Plugin**:
   ```bash
   # Install prerequisites
   sudo apt-get install -y ca-certificates curl gnupg

   # Add Docker official GPG key
   sudo install -m 0755 -d /etc/apt/keyrings
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
   sudo chmod a+r /etc/apt/keyrings/docker.gpg

   # Set up repository
   echo \
     "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
     $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
     sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

   # Install Docker packages
   sudo apt-get update
   sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
   ```
5. Allow your `ubuntu` user to run Docker commands without `sudo`:
   ```bash
   sudo usermod -aG docker $USER
   newgrp docker
   ```
6. Verify installations:
   ```bash
   git --version
   docker --version
   docker compose version
   ```

---

### Step 5: Clone Repository and Deploy
1. Clone the project repository:
   ```bash
   git clone https://github.com/alviangalen/music-app.git
   cd music-app
   ```
2. Inspect the project files (verify `docker-compose.yml` presence):
   ```bash
   ls -la
   ```
3. Build and run containers in detached mode:
   ```bash
   docker compose up -d --build
   ```
4. Check running containers status:
   ```bash
   docker compose ps
   ```

---

### Step 6: Verification
1. Open your web browser.
2. Navigate to:
   ```text
   http://<YOUR_EC2_PUBLIC_IP>
   ```
3. Verify that:
   - The frontend loads correctly.
   - The search functionality returns tracks.
   - Audio preview plays without errors.

---

## Submission Deliverables
Prepare a markdown report or PDF containing:
1. **Screenshot 1:** AWS Console showing your VPC, Subnet, and Route Table association.
2. **Screenshot 2:** Terminal output of `docker compose ps` running on the EC2 instance.
3. **Screenshot 3:** Web browser accessing `http://<YOUR_EC2_PUBLIC_IP>` showing a successful search result.
4. **EC2 Public IP link** (keep it active until grading is complete).

---

## Clean-Up Checklist
To avoid unnecessary AWS charges after grading:
- [ ] Stop or Terminate the EC2 instance.
- [ ] Release any Elastic IPs (if used).
- [ ] Delete the NAT Gateway (if created).
- [ ] Delete the VPC and attached resources.
