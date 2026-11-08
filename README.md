# CampusResource Cloud

Sistem Peminjaman dan Reservasi Fasilitas Kampus Berbasis Cloud  
Mata Kuliah Cloud Computing (PTI 2802) — Institut Pendidikan Indonesia

## Author
- **Nama**: Gistie Gusniar Putri Darmawan
- **NIM**: 25832072008
- **Kelas**: 2B — S1 Pendidikan Teknologi Informasi

## Problem & Value Proposition
Informasi ketersediaan fasilitas kampus tidak terpusat sehingga mahasiswa/ormawa harus mengecek manual dan melakukan pengajuan bertingkat yang rawan bentrok jadwal mendadak. CampusResource Cloud menyediakan platform terpusat untuk melihat ketersediaan fasilitas secara real-time, mengajukan reservasi, serta melacak status persetujuan.

## Target Users
- Mahasiswa / Pengurus Organisasi Kampus (HIMA/UKM)
- Petugas / Pengelola Fasilitas & Ruangan Kampus
- Administrator Sistem

## Core Features (M03 Baseline)
- Dashboard ketersediaan fasilitas kampus
- Formulir pengajuan reservasi & pelacakan status
- Role-Based Access Control (Mahasiswa & Petugas/Admin)
- Endpoint kesehatan sistem: `/health`, `/api/info`, `/api/facilities`
- Deployment awal di cloud infrastructure (Microsoft Azure)

## Architecture
Mahasiswa Browser --> Caddy Reverse Proxy --> Gunicorn WSGI Server --> Flask Application Core --> SQLite Database

## Infrastructure Specifications
- **Provider**: Microsoft Azure (East Asia Region)
- **SKU / Instance**: Standard B2ats v2
- **OS**: Ubuntu Server 24.04 LTS
- **Resource**: 2 vCPU, 1 GB RAM, 20 GB Disk (System) / 61 GB Temp
- **Web Stack**: Flask, Gunicorn, Caddy, systemd, SQLite3
- **Network & Firewall**: UFW Enabled, Backend Loopback-Only

## Health Check
`GET /health` (Menampilkan status HTTP 200 OK untuk service check)

## Security Controls
- Key-only SSH authentication (Ed25519)
- Root SSH login disabled
- Firewall UFW restrictive rules (Port 22, 80, 443)
- Loopback isolation (127.0.0.1:8000) untuk Gunicorn WSGI
- Sanitized environment variables & secret isolation

## Audit & Evidence Register
Arsip pengujian dan bukti teknis audit proposal tersimpan pada folder `docs/evidence/`:
- `ev01-vps-spec.md` : Spesifikasi & Metadata VPS Azure
- `ev02-https.md` : Konfigurasi FQDN, DNS, & TLS
- `ev05-backup.md` : Strategi Lokasi Backup Eksternal
- `ev06-restore.md` : Simulasi Integrity Check & Restore RTO/RPO
- `ev07-tco-12bulan.md` : FinOps, Budget Alert, & TCO 12 Bulan
- `ev08-load-test.md` : Uji Beban & Batas Kapasitas Performa
- `ev09-uji-domain.md` : Matriks Pengujian Bentrok Jadwal & RBAC

## Project Roadmap
- **M03**: Project Inception & Baseline Flask Deployment
- **M04**: DNS Setup, Domain FQDN, & Caddy HTTPS/TLS
- **M05**: Persistent Data Storage, Overlapping Check, & Off-site Backup
- **M06**: Containerization (Docker)
- **M07**: Infrastructure as Code (IaC)
- **M09**: Automated CI/CD Pipeline (GitHub Actions)
- **M16**: Production Readiness & Final UAS Defense
