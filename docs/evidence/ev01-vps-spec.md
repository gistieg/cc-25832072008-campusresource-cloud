# EV-01: Verifikasi Spesifikasi & Metadata VPS Azure

- **Nama Mahasiswa**: Gistie Gusniar Putri Darmawan
- **NIM**: 25832072008
- **Kode Proyek**: CC-PTI2802-25832072008-CRC
- **Tanggal Verifikasi**: 8 November 2026

---

## 1. Metadata Resource Provider
- **Cloud Provider**: Microsoft Azure
- **Region / Datacenter**: East Asia (Hong Kong)
- **SKU Instance / Size**: Standard B2ats v2
- **Public IPv4**: 52.175.54.94
- **Sistem Operasi**: Ubuntu Server 24.04.5 LTS (x86_64)

---

## 2. Tabel Deviasi & Pelurusan Spesifikasi (Temuan Audit 01)

| Parameter | Katalog Azure | Dokumen Lama | Hasil Pengukuran Real (Terminal) | Status Matriks |
| :--- | :--- | :--- | :--- | :--- |
| **vCPU Virtual** | 2 vCPU | 1 vCPU | **2 vCPU** | **Diperbaiki (Sesuai Output `nproc` & SKU)** |
| **RAM System** | 1.0 GiB | 1 GB | **887 MiB Total (632 MiB Available)** | **Valid** |
| **Disk Storage** | 20 GB OS | 20 GB | **61 GB Root Partition (4.9G Used / 9% Usage)** | **Valid** |
| **Architecture** | x86_64 | x86_64 | **x86_64 (AMD EPYC 7763 64-Core)** | **Valid** |

---

## 3. Log Output Terminal VPS (Fakta Hasil Pengukuran)

### Output `nproc`
`2`

### Output `free -h`
`Mem: 887Mi total | 254Mi used | 171Mi free | 3.7Mi shared | 487Mi buff/cache | 632Mi available`  
`Swap: 1.0Gi total | 69Mi used | 954Mi free`

### Output `df -h /`
`Filesystem: /dev/root | Size: 61G | Used: 4.9G | Avail: 57G | Use%: 9% | Mounted on: /`

### Output `cat /etc/os-release`
`PRETTY_NAME="Ubuntu 24.04.5 LTS"`  
`NAME="Ubuntu"`  
`VERSION_ID="24.04"`  
`VERSION="24.04.5 LTS (Noble Numbat)"`  
`VERSION_CODENAME=noble`  
`ID=ubuntu`  
`ID_LIKE=debian`  
`UBUNTU_CODENAME=noble`

### Output `lscpu` (Ringkasan Spesifikasi CPU)
`Architecture: x86_64`  
`CPU(s): 2`  
`On-line CPU(s) list: 0,1`  
`Vendor ID: AuthenticAMD`  
`Model name: AMD EPYC 7763 64-Core Processor`  
`Thread(s) per core: 2`  
`Core(s) per socket: 1`  
`Virtualization type: full (Microsoft Hypervisor)`

---

## 4. Kesimpulan Auditing
Spesifikasi VPS pada dokumen proposal Subbab 1.3 dan Tabel 6.2 telah diluruskan menjadi **2 vCPU** sesuai fakta SKU `Standard B2ats v2` pada portal Azure dan hasil eksekusi langsung `nproc`/`lscpu`. Kapasitas memori terukur sebesar 887 MiB dan disk utama sebesar 61 GB. Tidak ada kontradiksi spesifikasi.
