# AWS High Availability & Scalable Infrastructure (Terraform)

![Terraform](https://img.shields.io/badge/terraform-%235835CC.svg?style=for-the-badge&logo=terraform&logoColor=white)
![Ansible](https://img.shields.io/badge/ansible-%23990000.svg?style=for-the-badge&logo=ansible&logoColor=white)
![AWS](https://img.shields.io/badge/AWS-%23FF9900.svg?style=for-the-badge&logo=amazon-aws&logoColor=white)
![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white)

Provisioning of a highly available AWS infrastructure (VPC, ASG, ALB) with dynamic scaling policies using Terraform for infrastructure and Ansible for configuration management.

## Project Objective

The objective is to deploy a resilient, fault-tolerant High Availability (HA) architecture. By implementing Auto Scaling Groups and Load Balancers, the infrastructure ensures continuous application uptime and automatically adapts computing resources to real-time traffic demands.

### Technical Comparison: Static vs. Scalable Architecture

| Feature | Simple Provisioning | HA & Scalable Cluster |
| :--- | :--- | :--- |
| **Availability** | **Single Point of Failure.** If the specific EC2 instance crashes, the application goes down immediately. | **Multi-AZ Resilience.** Instances are distributed across multiple Availability Zones. If one fails, ASG automatically replaces it to maintain uptime. |
| **Scalability** | **Static.** Fixed number of servers (e.g., `count = 2`). Handling traffic spikes requires manual code changes. | **Dynamic.** Auto Scaling Group automatically adjusts the fleet size (Scale-Out/Scale-In) based on real-time CPU demand. |
| **Traffic Distribution** | **Direct Access.** Users connect directly to the server IP. No load balancing mechanism. | **Load Balanced.** Application Load Balancer (ALB) intelligently distributes traffic only to healthy instances. |
| **Cost Efficiency** | **Fixed.** You pay for a fixed capacity 24/7, regardless of actual usage. | **Optimized.** The cluster scales down to the minimum size during low-traffic periods, significantly reducing infrastructure costs. |
| **Network Control** | **Basic.** Often relies on the default AWS network configuration with limited control over addressing. | **Custom VPC Topology.** Implements a fully managed Virtual Private Cloud with specific subnets defined for each Availability Zone. |

## Technology Stack

* **Infrastructure (IaC):** Terraform
* **Configuration:** Ansible
* **Cloud Provider:** AWS

## Key Features

* **Target Tracking Scaling Policy:** Implemented a dynamic scaling rule based on `ASGAverageCPUUtilization`. The infrastructure automatically adds instances when CPU load exceeds **70%** and removes them when load drops.
* **Launch Templates:** Utilizes AWS Launch Templates (`aws_launch_template`) instead of legacy Launch Configurations for modern feature support and versioning.
* **Self-Healing Infrastructure:** The ASG monitors instance health. If an instance becomes unhealthy, it is automatically terminated and replaced to maintain the `desired_capacity`.

## Prerequisites

Before running the project, ensure you have the following installed:

* **AWS CLI** (configured with credentials)
* **Terraform**
* **Ansible**
* **SSH Key Pair** (generated and available in `~/.ssh/`)

## Deployment Workflow

**1. Clone the repository**
```bash
git clone https://github.com/KurosuSama/aws-terraform-ansible.git
cd aws-terraform-ansible/terraform
```
**2. Provision Infrastructure (Terraform) Initialize the provider, preview the changes, and apply the configuration to AWS**
```bash
terraform init
terraform plan
terraform apply
```
**3. Configure Servers (Ansible) Navigate to the ansible directory and execute the playbook**
```
cd ../ansible
ansible-playbook playbook.yml
```
