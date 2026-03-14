# 🚀 Kubernetty Project: HA Kubernetes Cluster on AWS

## 📋 Overview
Built a production-grade 2-node High Availability Kubernetes cluster using k3s on AWS EC2. This project demonstrates infrastructure as code, cloud networking, container orchestration, and cluster management.

## 🏗️ Architecture
![Architecture Diagram](images/01-architecture.png)

*Above: 2-node HA Kubernetes cluster with master node (k3s-server1) running control plane and nginx pod, worker node (k3s-server2) ready for workloads, and security group rules for cluster communication.*

## 🛠️ Technologies Used
- **Cloud:** AWS EC2 (t2.micro instances), Elastic IPs, Security Groups
- **Kubernetes:** k3s v1.34.5 (lightweight K8s)
- **Container:** Docker, nginx (test application)
- **Management:** Kubernetes Dashboard, kubectl
- **OS:** Ubuntu 24.04 LTS

## 📸 Project Screenshots

### 1. AWS Infrastructure
![AWS EC2 Instances](images/02-aws-instances.png)
*Two EC2 instances running Ubuntu (master and worker nodes)*

### 2. Security Configuration
![Security Group Rules](images/03-security-group-rules.png)
*Security group with proper rules for cluster communication (ports 6443, 10250, 8472) and external access (SSH, NodePorts)*


![Nodes with Metrics](images/05-nodes-final.png)
*Real-time CPU and memory metrics for both nodes*

### 3. Application Deployment
![nginx Pod Running](images/06-nginx-pod-running.png)
*nginx test application deployed and running on the cluster*

![Dashboard Overview](images/08-dashboard-overview.png)
*Dashboard showing workload status with running deployments and pods*

### 4. Working Application
![nginx Welcome Page](images/09-nginx-welcome-page.png)
*Browser accessing the deployed nginx application via NodePort 31779 — PROOF THAT IT WORKS!*

## ✅ Key Achievements
- Successfully provisioned 2 AWS EC2 instances with Elastic IPs
- Configured security groups for secure cluster communication
- Installed k3s on master node and joined worker node
- Deployed Kubernetes Dashboard for visual management
- Ran test nginx application accessible via browser
- Implemented proper security practices (SSH restricted to my IP, internal cluster communication via security group)

### 🔧 Challenges Solved
| Challenge | Solution |
|-----------|----------|
| Worker node couldn't connect to master | Fixed security group inbound rules to allow port 6443 |
| Invalid token error | Generated fresh token from master |
| Dashboard nodes not showing | Created proper cluster role bindings |
| App inaccessible from browser | Added NodePort range (30000-32767) to security group |

📖 *For detailed troubleshooting steps, see the Challenges.md file in this repository.*

## 🚀 Future Improvements
- Add more worker nodes for scalability
- Implement auto-scaling based on load
- Deploy a real microservices application
- Add Prometheus/Grafana monitoring stack


## 📁 Repository Structure
```
kubernetty-project/
├── README.md
├── images/
│   ├── 01-architecture.png
│   ├── 02-aws-instances.png
│   ├── 03-security-group-rules.png
│   ├── 04-nodes-both-ready.png
│   ├── 05-nodes-final.png
│   ├── 06-nginx-pod-running.png
│   ├── dashboard-overview.png
│   └── 09-nginx-welcome-page.png
└── CHALLENGES.md
```
📖 See [Challenges.md](challenges.md) for detailed troubleshooting steps.

## 🎓 What I Learned
- Kubernetes architecture and components (control plane, worker nodes)
- AWS networking and security group configuration
- Debugging cluster connectivity issues
- Managing secrets and tokens in Kubernetes
- Exposing applications via NodePort services


- ## 📫 Connect With Me
- GitHub: [donaemeka](https://github.com/donaemeka)
- LinkedIn: [Donatus Emeka Anyalebechi](https://linkedin.com/in/donatus-devops)
- Email: donaemeka92@gmail.com

---

**Built with 💪 and lots of coffee during my DevOps learning journey.**
