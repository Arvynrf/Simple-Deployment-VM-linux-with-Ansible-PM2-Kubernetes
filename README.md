# DevOps Technical Test

## Overview

Repository ini berisi implementasi **DevOps Technical Test** yang mencakup provisioning VM, konfigurasi runtime aplikasi, CI/CD, containerization, deployment ke Kubernetes, serta visibility (monitoring dasar).

Arsitektur dibagi menjadi tiga bagian utama:

* **Part A**: VM Provisioning & Configuration Management (Ansible)
* **Part B**: CI/CD, Docker, dan Kubernetes Deployment (Minikube)
* **Part C**: Visibility / Monitoring (Bonus)

---

## Part A – VM Provisioning & Application Runtime (Ansible)

### Objective

Menyiapkan satu VM Ubuntu lokal (VMware) dan mengotomatisasi instalasi runtime aplikasi menggunakan **Ansible**.

### Environment

* Host: Windows (WSL + Ubuntu)
* Guest VM: Ubuntu (VMware)
* Configuration Management: Ansible
* Runtime: Node.js 18, PM2

### Implementation

* Ansible inventory digunakan untuk mendefinisikan target VM.
* Role `runtime` bertanggung jawab untuk:

  * Update package repository
  * Install dependency dasar
  * Install Node.js 18
  * Install PM2 secara global
  * Menjalankan aplikasi menggunakan PM2

### Deliverables

* `inventory.ini`
* `ansible.cfg`
* `playbooks/runtime.yml`
* `playbooks/deploy.yml`
* `roles/runtime/tasks/main.yml`

### Verification

* Aplikasi berjalan di VM
* Status aplikasi dapat dicek menggunakan:

  ```bash
  pm2 status
  pm2 logs
  ```

---

## Part B – CI/CD, Docker & Kubernetes Deployment

### Objective

Membangun pipeline CI/CD untuk build Docker image dan melakukan deployment aplikasi ke Kubernetes cluster lokal menggunakan **Minikube**.

### CI/CD Pipeline

* Platform: **GitHub Actions**
* Trigger: Push ke branch `main`
* Steps:

  * Checkout source code
  * Build Docker image
  * Push image ke Docker Hub

### Containerization

* Aplikasi dikemas menggunakan Docker
* Image dipublish ke Docker Hub dan digunakan oleh Kubernetes Deployment

### Kubernetes Setup

* Kubernetes Cluster: **Minikube (Docker driver)**
* Resource yang digunakan:

  * Deployment
  * Service (NodePort)

### Deliverables

* `Dockerfile`
* `.github/workflows/ci.yml`
* `deployment.yml`
* `service.yml`

### Verification

* Pod status:

  ```bash
  kubectl get pods
  ```
* Service status:

  ```bash
  kubectl get svc
  ```
* Aplikasi dapat diakses melalui:

  ```bash
  minikube service node-service
  ```
---

## Architecture Summary

```
Developer -> GitHub -> GitHub Actions
               |
               v
        Docker Hub (Image Registry)
               |
               v
          Minikube (Kubernetes)
               |
               v
           Node.js Application
```


## Notes

* Deployment menggunakan Minikube bersifat lokal dan tidak menggunakan cloud load balancer.
