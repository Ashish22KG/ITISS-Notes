# DevOps — Interview Notes (incl. Git & Cloud/Deployment)

## 1. What is DevOps?
A set of practices, tools, and cultural principles combining **Dev**elopment and **Op**erations to automate and improve developing, testing, deploying, and maintaining software.

**Why it matters:**
- Faster software delivery
- Automated testing/deployment
- Fewer manual errors
- Better team collaboration
- Faster failure detection/recovery

### Common DevOps tools
| Area | Tools |
|---|---|
| Version Control | Git, GitHub |
| CI/CD | Jenkins, GitHub Actions, GitLab CI |
| Containers | Docker |
| Orchestration | Kubernetes |
| Config Management | Ansible |
| Infrastructure as Code | Terraform |
| Cloud | AWS, Azure, GCP |
| Monitoring | Prometheus, Grafana |
| Logging | ELK Stack |

**Interview answer:** "DevOps is a combination of practices, tools, and cultural principles that brings development and operations together to automate the software development lifecycle. It focuses on continuous integration, continuous delivery, automation, monitoring, and reliable software deployment."

> DevOps = culture + practices + automation + tools — not a single tool.

---

## 2. Git & GitHub Stages

### Workflow
```
Working Directory
       ↓ git add
Staging Area
       ↓ git commit
Local Repository
       ↓ git push
Remote Repository (GitHub)
```

1. **Working Directory** — where you create/modify files (e.g. `app.py`).
2. **Staging Area** — `git add app.py` (or `git add .`) prepares changes for the next commit.
3. **Local Repository** — `git commit -m "..."` permanently records staged changes locally, generating a commit hash.
4. **Remote Repository (GitHub)** — `git push origin main` uploads local commits to GitHub.

### Example
```bash
git status
git add login.py
git commit -m "Added login functionality"
git push origin main
```

**Easy recall:** Modify → Add → Commit → Push

| Stage | Command | Purpose |
|---|---|---|
| Working Directory | — | Create/modify files |
| Staging Area | `git add` | Prepare changes |
| Local Repository | `git commit` | Save changes locally |
| Remote Repository | `git push` | Upload commits to GitHub |

> Git ≠ GitHub. Git is a version-control system; GitHub is a hosting/collaboration platform for Git repos.

---

## 3. Deployment Evolution

**Progression:** Traditional (physical) → Virtualization (VMs) → Containerization → Orchestration (Kubernetes)

### 3.1 Traditional Deployment (Physical Machines / On-Prem)
Application runs directly on company-owned physical servers.

Must manage: physical servers, storage, network devices, data-center space, power/cooling, IT staff, OS, apps.

- **Advantages:** full control, customizable hardware, data stays in-house, no cloud dependency
- **Disadvantages:** high CAPEX, upfront hardware purchase, slow scaling, maintenance overhead, resource underutilization, costly DR

**Definition:** "Traditional deployment is an approach where applications are deployed directly on physical servers, usually in an organization's own data center, with the organization responsible for purchasing, managing, maintaining, and scaling the infrastructure."

### 3.2 Virtualized Deployment (VMs)
A **hypervisor** divides one physical server into multiple isolated VMs (each with virtual CPU, RAM, disk, NIC, OS).

```
Physical Server
       ↓
    Hypervisor
   ↙    ↓     ↘
 VM1    VM2    VM3
  ↓      ↓      ↓
 App1   App2   App3
```

- **Advantages:** better resource utilization, faster provisioning, easier scaling, isolation, snapshots/cloning, easier backup/recovery, multi-OS on one host
- **Disadvantages:** each VM needs its own OS, more overhead than containers, management complexity

**Definition:** "Virtualized deployment uses a hypervisor to divide a physical server into multiple isolated virtual machines, allowing multiple applications or operating systems to run on the same physical infrastructure."

### 3.3 Containerized Deployment
Packages an application with its dependencies into a lightweight container. Containers normally **share the host OS kernel** (unlike VMs, which each carry a full guest OS).

```
VM:  Hardware → Hypervisor → VM(Guest OS + App)
Container: Hardware → Host OS → Container Runtime → Container(App + Dependencies)
```

- Lightweight, fast to start, portable, easy to replicate, resource-efficient
- Solves "it works on my machine" — the same image runs on laptop, test server, and production

**Definition:** "Containerized deployment packages an application and its dependencies into lightweight, isolated containers that share the host operating system kernel and can run consistently across different environments."

---

## 4. Environments (Dev → Test → Staging → Production)

> Different from deployment tech above: deployment tech = **how**; environments = **where/why**.

```
Development → Testing/QA → Staging/Pre-Production → Production
```

- **Development** — developers write/modify/debug code; not exposed to end users.
- **Testing/QA** — verifies the app works.
  - *Functional testing* — does the feature work (login, payment, password reset)?
  - *Non-functional testing* — performance, load, stress, security, reliability, scalability.
- **Staging/Pre-Production** — mirrors production architecture closely to catch issues before release.
- **Production** — the live environment for real end users; focuses on availability, performance, security, scalability, monitoring, backup, disaster recovery, high availability.

### High Availability (HA)
Design so that one component failing doesn't take down the whole app.
```
              Load Balancer
               /    |    \
           Server1 Server2 Server3
              \      |      /
               Database Cluster
```
If Server1 fails, Server2/Server3 keep serving users.

**Key distinction:** Physical/VM/containerized deployment = the infrastructure technology running the app. Dev/Test/Staging/Production = the environments the app moves through before reaching users.

---

## 5. Virtualization (definition)
Technology that creates virtual versions of computing resources (servers, OS, storage, networks) via software, letting multiple isolated virtual environments run on one physical machine — using a **hypervisor** (e.g., VMware ESXi, Hyper-V, KVM).

**Interview answer:** "Virtualization is the process of creating virtual computing environments on top of physical hardware using a hypervisor. It allows multiple isolated virtual machines to share the resources of a single physical server efficiently."

---

## 6. Cloud Computing (definition)
Delivery of computing resources (servers, storage, databases, networking, software) over the Internet, on-demand, without owning/maintaining physical infrastructure.

**Key characteristics:** on-demand resources, scalability, pay-as-you-go, resource sharing, high availability, Internet access.

---

## 7. Data Center (definition)
A physical facility housing servers, storage, networking equipment, and supporting systems (power, cooling, security) used to store, process, and deliver data/applications.

| Component | Purpose |
|---|---|
| Servers | Process apps/data |
| Storage | Store files, DBs, backups |
| Network devices | Connect servers/users |
| Power systems | Continuous electricity |
| Cooling systems | Prevent overheating |
| Fire/security systems | Protect physical infra |
| Backup systems | Maintain availability |

---

## 8. AWS Global Infrastructure: Edge Locations & CDN

**Edge Location** — AWS site in major cities/populous areas, used by services like CloudFront/Route 53 to serve content closer to users, reducing latency.

**CDN (Content Delivery Network)** — geographically distributed network of servers/edge locations that caches and delivers content from a location closer to the user.

```
Origin (Mumbai) → Edge (London/USA/Japan) → Users
```

**Cache miss (first request):** User → Edge → (miss) → Origin → cached at Edge → User
**Cache hit (later request):** User → Edge → (hit, served from cache) → User

**Latency:** time for data to travel between user and server + response start. Closer edge → lower latency.

### Region vs Availability Zone vs Edge Location
| Feature | Region | Availability Zone | Edge Location |
|---|---|---|---|
| Purpose | Main AWS geographic area | Isolated infra within a Region | Bring services/content closer to users |
| Example | Mumbai Region | AZ within Mumbai | CloudFront/Route 53 site |
| Used for | Running AWS resources | High availability/fault tolerance | Low-latency delivery |
| Services | EC2, S3, RDS | EC2, RDS, etc. | CloudFront, Route 53 |

**Recall:** Region = where you deploy · AZ = where you achieve HA · Edge Location = where you deliver content closer to users.

CloudFront = AWS's managed CDN service (CDN is the general concept; others include Cloudflare, Akamai).

Route 53 = AWS's DNS service, using globally distributed infra for low-latency DNS responses/routing.

---

## 9. EC2 (Elastic Compute Cloud)
An AWS IaaS service providing resizable virtual server **instances** in the cloud.

- Choose OS, CPU/RAM, storage, networking → AWS provisions the VM in a chosen AZ
- Connect via SSH (Linux) or RDP (Windows)
- Scalable up/down; can create multiple instances

**Interview answer:** "EC2 is an AWS Infrastructure-as-a-Service (IaaS) service that provides scalable virtual servers in the cloud. It allows users to choose compute resources, operating systems, storage, and networking according to their requirements and run applications without managing physical hardware."

### AMI (Amazon Machine Image)
A pre-configured template used to create EC2 instances — contains OS, software, and configuration needed to launch an instance. You can create a custom AMI from a configured EC2 instance and launch identical instances from it.

**Recall:** AMI = template → EC2 = virtual server created from that template.

### EC2 Key Pairs
Cryptographic key pair used to securely authenticate/SSH into an EC2 instance.

- **Public key** → stored on the EC2 instance
- **Private key** → downloaded and kept by the user (never shared; AWS doesn't re-issue it if lost)

```bash
ssh -i my-key.pem ubuntu@<EC2-Public-IP>
```

**Interview answer:** "An EC2 key pair is a pair of cryptographic keys, consisting of a public key and a private key, used for secure authentication to an EC2 instance. The public key is stored on the instance, while the private key is kept by the user and used to establish an authenticated connection, such as SSH to a Linux instance."

---

*See [[01-networking.md]] for networking fundamentals and [[02-security.md]] for security concepts referenced (e.g. hardening, encryption).*
