# Mini Terraform Lab – Docker Load Balancer Architecture

## Overview

Project ini adalah simulasi arsitektur mini production menggunakan Terraform dan Docker untuk membangun sistem multi-server dengan load balancing secara lokal.

Teknologi yang digunakan:

- Terraform (Infrastructure as Code)
- Docker (Container Runtime)
- Nginx (Load Balancer dan Backend Server)
- WSL (Windows Subsystem for Linux)
- VS Code

Semua berjalan secara lokal tanpa biaya cloud.

---

# 1. Apa Itu Terraform?

Terraform adalah tool Infrastructure as Code (IaC) yang digunakan untuk membuat dan mengelola infrastruktur menggunakan file konfigurasi.

Infrastructure as Code berarti infrastruktur didefinisikan dalam bentuk kode, bukan dibuat manual melalui klik dashboard.

Dengan Terraform, kita dapat membuat:

- Server
- Network
- Container Docker
- Database
- Infrastruktur cloud seperti AWS, Azure, dan GCP

Semua resource didefinisikan menggunakan file `.tf`.

---

# 2. Mengapa Menggunakan Terraform?

Tanpa Terraform:

- Setup dilakukan secara manual
- Sulit direplikasi
- Sulit melakukan scaling
- Risiko human error tinggi

Dengan Terraform:

- Infrastruktur bisa dibuat ulang kapan saja
- Mudah melakukan scaling
- Mendukung version control (Git)
- Bisa dihancurkan dalam satu command
- Cocok untuk DevOps, Cloud Engineering, dan MLOps

---

# 3. Arsitektur Project

Arsitektur yang dibangun dalam project ini:

```
Browser (localhost:8080)
        ↓
Nginx Load Balancer
        ↓
5 Backend Nginx Server
        ↓
Docker Network
```

Konsep yang dipelajari:

- Modular Terraform
- Provider declaration
- Output passing antar module
- Docker networking
- Reverse proxy
- Round robin load balancing
- Destroy dan recreate infrastructure

---

# 4. Struktur Folder

```
terra_lab/
│
├── main.tf
├── variables.tf
├── outputs.tf
│
├── lb/
│   └── nginx.conf
│
└── modules/
    ├── backend/
    │   ├── main.tf
    │   └── variables.tf
    │
    └── loadbalancer/
        ├── main.tf
        └── variables.tf
```

---

# 5. Instalasi Awal

## 5.1 Install Terraform di WSL

```bash
sudo apt update
sudo apt install unzip wget -y

wget https://releases.hashicorp.com/terraform/1.6.6/terraform_1.6.6_linux_amd64.zip
unzip terraform_1.6.6_linux_amd64.zip
sudo mv terraform /usr/local/bin/

terraform -v
```

Pastikan versi Terraform muncul.

---

## 5.2 Install Docker di WSL

```bash
sudo apt install docker.io -y
sudo usermod -aG docker $USER
exit
```

Masuk kembali ke WSL lalu jalankan:

```bash
docker run hello-world
```

Jika berhasil, Docker sudah siap digunakan.

---

## 5.3 Buat dan Buka Project

```bash
mkdir terra_lab
cd terra_lab
code .
```

Disarankan menggunakan extension:

- Remote - WSL
- Terraform

---

# 6. Cara Menjalankan Project

## 6.1 Inisialisasi

```bash
terraform init
```

Command ini akan mengunduh provider dan menginisialisasi project.

---

## 6.2 Melihat Rencana Eksekusi

```bash
terraform plan
```

Terraform akan menampilkan resource yang akan dibuat.

---

## 6.3 Membuat Infrastruktur

```bash
terraform apply
```

Ketik:

```
yes
```

Jika berhasil, output akan menampilkan:

```
backend_container_names = [
  "backend_0",
  "backend_1",
  "backend_2",
  "backend_3",
  "backend_4",
]

load_balancer_url = "http://localhost:8080"
```

---

# 7. Akses Aplikasi

Buka browser dan akses:

```
http://localhost:8080
```

Refresh beberapa kali untuk melihat load balancing bekerja.

---

# 8. Testing Load Balancing via CLI

Gunakan perintah berikut:

```bash
for i in {1..10}; do curl localhost:8080; echo; done
```

Output akan berganti-ganti antara backend_0 sampai backend_4.

---

# 9. Menghapus Semua Resource

Untuk menghapus semua container dan network:

```bash
terraform destroy
```

Ketik:

```
yes
```

Semua resource yang dikelola Terraform akan dihapus.

---

# 10. Workflow Terraform Profesional

```
terraform init
terraform plan
terraform apply
terraform destroy
```

---

# 11. Konsep yang Dipelajari

Melalui project ini, dipelajari:

- Infrastructure as Code
- Modular Terraform
- Provider isolation
- Docker networking
- Reverse proxy
- Round robin load balancing
- High availability simulation
- Clean destroy lifecycle

---

# 12. Pengembangan Lanjutan

Beberapa upgrade yang dapat dilakukan:

- Dynamic nginx upstream tanpa hardcode backend
- Auto scaling simulation
- Backend menggunakan Python Flask
- Tambah database module
- Integrasi ke AWS EC2
- Monitoring container

---

# 13. Kesimpulan

Project ini menunjukkan bahwa:

- Infrastruktur dapat dikelola sebagai kode
- Multi-server dapat dibuat secara otomatis
- Load balancing dapat disimulasikan secara lokal
- Infrastruktur dapat dihancurkan dan dibuat ulang kapan saja

Project ini menjadi fondasi penting untuk DevOps, Cloud Engineering, MLOps, dan Distributed Systems.
