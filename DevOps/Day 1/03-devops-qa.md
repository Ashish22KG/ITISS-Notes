# DevOps — Interview Q&A (incl. Git & Cloud/Deployment)

### Q1. What is DevOps?
**A:** DevOps is a combination of practices, tools, and cultural principles that brings development and operations together to automate the software development lifecycle. It focuses on continuous integration, continuous delivery, automation, monitoring, and reliable software deployment.

### Q2. Why is DevOps important?
**A:** It enables faster software delivery, automated testing and deployment, fewer manual errors, better collaboration between teams, and faster detection and recovery from failures.

### Q3. Name some common DevOps tools by category.
**A:** Version Control — Git, GitHub; CI/CD — Jenkins, GitHub Actions, GitLab CI; Containers — Docker; Orchestration — Kubernetes; Configuration Management — Ansible; Infrastructure as Code — Terraform; Cloud — AWS, Azure, GCP; Monitoring — Prometheus, Grafana; Logging — ELK Stack.

### Q4. Is DevOps just a tool or technology?
**A:** No — DevOps is not a single tool. It is a culture + practices + automation + tools combined.

### Q5. Explain the Git workflow stages.
**A:** Working Directory (create/modify files) → `git add` → Staging Area (changes prepared for commit) → `git commit` → Local Repository (changes saved locally with a commit hash) → `git push` → Remote Repository/GitHub (commits uploaded).

### Q6. What is the staging area in Git?
**A:** An intermediate area where you select and prepare changes (via `git add`) before committing them to the local repository.

### Q7. What's the difference between Git and GitHub?
**A:** Git is a version-control system; GitHub is a platform for hosting and collaborating on Git repositories. They are not the same thing.

### Q8. What is deployment?
**A:** Deployment is the process of making an application available on a system or infrastructure — configuring dependencies, networking, database, etc. — so that users or other systems can use it.

### Q9. What is traditional (physical machine) deployment?
**A:** An approach where applications are deployed directly on physical servers, usually in an organization's own data center, with the organization responsible for purchasing, managing, maintaining, and scaling the infrastructure — including servers, storage, networking, data-center space, power/cooling, and IT staff.

### Q10. What are the advantages and disadvantages of traditional deployment?
**A:** Advantages: complete control over infrastructure, customizable hardware, data stays in-house, no cloud dependency. Disadvantages: high CAPEX, upfront hardware purchase, slow scaling, ongoing hardware maintenance, resource underutilization, costly disaster recovery.

### Q11. What is virtualization, and what is virtualized deployment?
**A:** Virtualization is the process of creating virtual computing environments on top of physical hardware using a hypervisor, allowing multiple isolated virtual machines to share one physical server's resources efficiently. Virtualized deployment uses this hypervisor to divide a physical server into multiple VMs, letting multiple applications/OSes run on the same hardware.

### Q12. Why was virtualization introduced?
**A:** To avoid wasting resources — e.g., a physical server with 32 CPU cores and 128GB RAM might only need 4 cores and 16GB for one application; virtualization lets several workloads share the same physical machine efficiently.

### Q13. What are the advantages and disadvantages of virtualized deployment?
**A:** Advantages: better resource utilization, faster provisioning, easier scaling, isolation between VMs, snapshots/cloning, easier backup/recovery, multiple OSes on one server. Disadvantages: each VM needs its own OS, more overhead than containers, VM management complexity.

### Q14. What is containerized deployment, and how does it differ from VMs?
**A:** Containerized deployment packages an application and its dependencies into lightweight, isolated containers that share the host OS kernel (unlike VMs, which each run a full guest OS on top of a hypervisor). This makes containers lightweight, fast to start, portable, and resource-efficient, and helps avoid "it works on my machine" issues since the same image runs across dev, test, and production.

### Q15. What is the difference between VM architecture and container architecture?
**A:** VM: Hardware → Hypervisor → VM (Guest OS + Application). Container: Hardware → Host OS → Container Runtime → Container (Application + Dependencies). Containers skip the guest OS layer by sharing the host kernel.

### Q16. What's the evolution of deployment technology you should remember for interviews?
**A:** Traditional (physical servers) → Virtualization (VMs) → Containerization (containers) → Orchestration (Kubernetes).

### Q17. What are the different application environments, and how do they differ from deployment technology?
**A:** Development → Testing/QA → Staging/Pre-Production → Production. Deployment technology (physical/VM/container) describes *how* the app is deployed; environments describe *where and for what purpose* the app is deployed as it moves toward release.

### Q18. What happens in the Development environment?
**A:** Developers write, modify, debug, and experiment with code (e.g., via Git → Dev Server). It's not normally exposed to end users.

### Q19. What's the difference between functional and non-functional testing in the Testing/QA environment?
**A:** Functional testing checks what the application does (e.g., does login, registration, or payment work?). Non-functional testing checks how well it performs (e.g., performance, load, stress, security, reliability, scalability — such as whether 10,000 users can access it simultaneously).

### Q20. What is the purpose of a Staging environment?
**A:** To closely replicate production architecture so problems can be identified before exposing a new version to real users — the app moves from testing → staging → final checks → production.

### Q21. What is the Production environment, and what does it focus on?
**A:** The live environment used by actual end users. It focuses heavily on availability, performance, security, scalability, monitoring, backup, disaster recovery, and high availability.

### Q22. What is High Availability (HA)?
**A:** Designing a system so the failure of one component doesn't make the entire application unavailable — e.g., a load balancer distributing traffic across multiple app servers backed by a database cluster, so if one server fails, the others keep serving users.

### Q23. What is cloud computing?
**A:** The delivery of computing resources — servers, storage, databases, networking, software — over the Internet, on-demand, without requiring the user to own or maintain the physical infrastructure, typically on a pay-as-you-go basis.

### Q24. What are the key characteristics of cloud computing?
**A:** On-demand resources, scalability, pay-as-you-go pricing, resource sharing, high availability, and access over the Internet.

### Q25. What is a data center?
**A:** A physical facility that houses and manages servers, storage devices, networking equipment, and other IT infrastructure — including power, cooling, and security systems — used to store, process, and deliver data and applications.

### Q26. What is an AWS Edge Location?
**A:** An AWS site located close to end users, primarily used by services like CloudFront and Route 53 to serve users from a location closer to them, reducing latency and improving performance.

### Q27. What is a CDN (Content Delivery Network)?
**A:** A geographically distributed network of servers/edge locations that caches and delivers content to users from a location closer to them, reducing latency and improving performance. Amazon CloudFront is AWS's CDN service.

### Q28. How does CloudFront handle a cache miss vs. a cache hit?
**A:** Cache miss (first request): the edge location has no cached copy, so it retrieves the file from the origin server, caches it, and returns it to the user. Cache hit (subsequent request): the edge location serves the cached copy directly without contacting the origin.

### Q29. What is the difference between an AWS Region, Availability Zone, and Edge Location?
**A:** Region = the main AWS geographic area where you deploy resources (e.g., EC2, S3, RDS). Availability Zone = isolated infrastructure within a Region used for high availability and fault tolerance. Edge Location = a site used by services like CloudFront/Route 53 to deliver content closer to users with low latency.

### Q30. What is EC2?
**A:** Amazon EC2 (Elastic Compute Cloud) is an AWS Infrastructure-as-a-Service (IaaS) offering that provides resizable virtual server instances in the cloud. Users choose compute resources, OS, storage, and networking, then run applications without managing physical hardware — connecting via SSH (Linux) or RDP (Windows).

### Q31. What is an AMI?
**A:** An Amazon Machine Image is a pre-configured template used to create EC2 instances, containing the operating system, software, and configuration needed to launch an instance. You can create a custom AMI from a configured EC2 instance to launch multiple identical instances.

### Q32. What is an EC2 key pair, and how does it work?
**A:** A set of cryptographic keys — a public key stored on the EC2 instance and a private key kept securely by the user — used to authenticate SSH connections to the instance (e.g., `ssh -i my-key.pem ubuntu@<EC2-Public-IP>`). The private key must never be shared, and AWS does not re-issue it if lost.
