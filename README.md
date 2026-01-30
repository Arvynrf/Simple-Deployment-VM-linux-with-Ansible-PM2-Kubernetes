# DevOps Technical Test

## Overview

Repository ini berisi implementasi **DevOps Technical Test** yang mencakup provisioning VM, konfigurasi runtime aplikasi, CI/CD, containerization, deployment ke Kubernetes, serta visibility (monitoring dasar).

Aplikasi simpel Node.js yang digunakan berada pada repository terpisah:
👉 https://github.com/Arvynrf/simple-app-nodejs

Arsitektur dibagi menjadi tiga bagian utama:

* **Part A**: VM Provisioning & Configuration Management (Ansible)
* **Part B**: CI/CD, Docker, dan Kubernetes Deployment (Minikube)

---

## Part A – VM Provisioning & Application Runtime (Ansible)

### Objective

Menyiapkan satu VM Ubuntu lokal (VMware) dan mengotomatisasi instalasi runtime aplikasi menggunakan **Ansible**.

### Environment

* Host: Windows (WSL + Ubuntu)
* Guest VM: Ubuntu (VMware)
* Configuration Management: Ansible
* Runtime: Node.js 18, PM2
### Directory Structure

Struktur direktori Part A dirancang mengikuti **Ansible Best Practices**, sehingga mudah dipahami, scalable, dan reusable.

```
part-a/
├── ansible.cfg            # Konfigurasi Ansible (inventory, privilege escalation)
├── inventory.ini          # Daftar host VM target
├── playbooks/
│   ├── runtime.yml        # Setup runtime (Node.js, PM2)
│   └── deploy.yml         # Deploy & run aplikasi menggunakan PM2
│   └──roles/
│       └── runtime/
│           └── tasks/
│               └── main.yml   # Task utama instalasi & konfigurasi runtime
└── 
```

### Implementation

* **inventory.ini** digunakan untuk mendefinisikan VM target.
* **runtime.yml** memanggil role `runtime` untuk setup environment.
* **roles/runtime/tasks/main.yml** berisi task modular (install Node.js, PM2, dependency).
* **deploy.yml** bertanggung jawab untuk cloning repo aplikasi dan menjalankannya dengan PM2.

Pendekatan ini memisahkan:

* *what to do* (playbook)
* *how to do* (role)

Sehingga konfigurasi lebih maintainable.

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
