# AWS Hybrid File Services with Active Directory Authentication

[![AWS](https://img.shields.io/badge/AWS-Advanced_Architecture-orange?logo=amazon-aws)](https://aws.amazon.com/)
[![Terraform](https://img.shields.io/badge/IaC-Terraform_v1.0+-purple?logo=terraform)](https://www.terraform.io/)
[![Active Directory](https://img.shields.io/badge/Directory_Service-Active_Directory-blue?logo=microsoft-active-directory)](https://microsoft.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](https://opensource.org/licenses/MIT)

## 📌 Project Executive Summary

Project ini mengimplementasikan solusi **Hybrid Cloud Storage** tingkat enterprise yang menghubungkan infrastruktur lokal (*On-Premises*) dengan AWS Cloud secara aman. Solusi ini menggunakan **AWS Storage Gateway (File Gateway)** untuk menyediakan akses protokol SMB (*Server Message Block*) dengan latensi rendah kepada pengguna lokal, sementara seluruh data di-back up secara *scalable* ke **Amazon S3**. 

Keamanan kontrol akses diatur secara terpusat dengan mengintegrasikan AWS Storage Gateway ke dalam **Microsoft Active Directory (AD)**, memastikan bahwa hak akses file (ACL) tetap mematuhi kebijakan tata kelola IT perusahaan yang sudah ada.

### 🌟 Key Business & Technical Benefits:
* **Hybrid Storage Integration:** Pengguna lokal dapat mengakses cloud storage (S3) layaknya network share lokal biasa (`\\smb-gateway\share`).
* **Centralized Identity & Access Management:** Autentikasi menggunakan kredensial Active Directory user yang sudah ada tanpa perlu membuat user IAM baru di AWS.
* **Cost Optimization:** Mengurangi biaya pengadaan hardware storage fisik lokal (SAN/NAS) dengan memanfaatkan S3 Storage Classes yang fleksibel.
* **Infrastructure as Code (IaC):** Seluruh arsitektur AWS dideploy secara otomatis menggunakan **Terraform** untuk memastikan kepatuhan tata kelola, konsistensi, dan auditabilitas.

---

## 🗺️ High-Level Architecture Diagram

Berikut adalah desain arsitektur hybrid yang dibangun dalam project ini:

![Hybrid Storage Architecture](architecture-diagram/hybrid-storage-v1.png)

### Alur Kerja Sistem:
1. **Client Request:** User lokal di kantor mengakses berkas melalui SMB Share Endpoint (`\\storage-gateway.corp.local\marketing`).
2. **AD Authentication:** AWS Storage Gateway meneruskan permintaan token autentikasi ke Active Directory Domain Controller untuk memeriksa izin akses user (Kerberos/NTLM).
3. **Cache & Cloud Sync:** Jika diizinkan, file dibaca dari *local cache* Storage Gateway (untuk performa cepat). File baru atau perubahan yang terjadi akan disinkronisasikan secara otomatis dan aman (SSL/TLS) ke Amazon S3 Bucket di AWS Cloud.

---

## 🛠️ Tech Stack & Components

* **Cloud Provider:** Amazon Web Services (AWS)
  * AWS Storage Gateway (SMB File Gateway)
  * AWS Directory Service (AWS Managed Microsoft AD / AD Connector)
  * Amazon S3 (Simple Storage Service)
  * AWS VPC, Site-to-Site VPN / Transit Gateway (Simulasi konektivitas Hybrid)
  * Amazon EC2 (Digunakan untuk simulasi On-Premises Domain Controller & Testing Client)
* **Infrastructure as Code:** Terraform
* **On-Premises Automation:** PowerShell Scripting (Active Directory Domain Services automation)

---

## 🚀 Step-by-Step Deployment Guide

### Prerequisites
* AWS CLI terkonfigurasi dengan hak akses Administrator.
* Terraform CLI (v1.0.0+) terinstall.
* Git untuk melakukan clone repository.

### Langkah 1: Kloning Repositori
```bash
git clone [https://github.com/username/AWS-Hybrid-File-Services-with-Active-Directory-Authentication.git](https://github.com/username/AWS-Hybrid-File-Services-with-Active-Directory-Authentication.git)
cd AWS-Hybrid-File-Services-with-Active-Directory-Authentication
