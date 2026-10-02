---



This project demonstrates how to deploy a highly available **WordPress application with a MySQL database** on AWS using a **self-managed Kubernetes cluster (kubeadm)** running on EC2 instances.

It integrates persistent storage using both:

* **Amazon EBS CSI Driver** → for MySQL (block storage)
* **Amazon EFS CSI Driver** → for WordPress (shared file system)

---

# Architecture Overview

- **three EC2 Instances (Ubuntu)**
  - 1 Master Node
  - 2 Worker Nodes

- Kubernetes cluster initialized using `kubeadm`
- WordPress deployed as a stateless app (but uses shared storage)
- MySQL deployed with persistent storage (EBS volume)
- Storage handled via CSI drivers

## Technologies

- Kubernetes (kubeadm)
- Docker
- AWS EC2
- Amazon EBS (Block Storage)
- Amazon EFS (Shared File Storage)
- CSI Drivers
- kubectl

---

## Prerequisites

Before starting, ensure you have:

### Infrastructure

- ✅ 3 EC2 instances (Ubuntu recommended)
  - Minimum: t2.medium

- ✅ Security groups allowing:
  - SSH (22)
  - HTTP (80)
  - HTTPS (443)
  - Kubernetes ports (6443, 10250, etc.)

### AWS Requirements

- AWS account
- IAM roles or credentials configured on instances like (ebsdriverpolicy,efsdriverpolicy)
- loadbalancer available
- EFS file system created (with mount targets in same VPC)

### Kubernetes Setup

- kubeadm cluster initialized
- kubectl configured on master node
- CNI plugin installed

### Storage Drivers

#### 1. Amazon EBS CSI Driver

- Required for MySQL persistent storage
- Handles lifecycle of EBS volumes in Kubernetes

#### 2. Amazon EFS CSI Driver

- Required for shared WordPress storage
- Enables multiple pods to access the same filesystem

---

## IAM & Permissions

Since this is a **self-managed cluster (NOT EKS)**:

You must manually configure:

- IAM Roles
- OR attach permissions to EC2 instance

### Required Policies:

- `AmazonEBSCSIDriverPolicy`
- `AmazonEFSCSIDriverPolicy`

Both drivers require IAM permissions to interact with AWS storage services ([docs.aws.amazon.com])

## Notes

- EBS volumes can only be mounted to **one node at a time**
- EFS supports **multi-node access**, ideal for WordPress
- Ensure EFS mount targets exist in all AZs used by your nodes
- CSI drivers require correct IAM setup or they will fail silently

**Created by**

Hesham Osman
