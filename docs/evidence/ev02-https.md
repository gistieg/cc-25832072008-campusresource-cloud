# EV-02: Laporan Konfigurasi FQDN, DNS, & HTTPS/TLS

- **Nama Mahasiswa**: Gistie Gusniar Putri Darmawan
- **NIM**: 25832072008
- **Kode Proyek**: CC-PTI2802-25832072008-CRC
- **Tanggal Penyusunan**: 8 November 2026

---

## 1. Arsitektur Reverse Proxy & Domain Target

| Parameter Domain / TLS | Spesifikasi Ditetapkan | Status Kesiapan |
| :--- | :--- | :--- |
| **Public IP VPS** | `52.175.54.94` | Aktif (Azure East Asia) |
| **Target FQDN** | `25832072008.5opsi.id` | Subdomain Ditetapkan Dosen Pengampu |
| **Reverse Proxy Server** | Caddy Server v2 | Terkonfigurasi untuk Auto-HTTPS |
| **WSGI Server Backend** | Gunicorn (127.0.0.1:8000) | Loopback-Only Isolation |
| **Penyedia Sertifikat SSL** | Let's Encrypt / ZeroSSL | Automated ACME via Caddy |

---

## 2. Konfigurasi Caddyfile (`/etc/caddy/Caddyfile`)

```text
# Baseline Caddyfile untuk CampusResource Cloud

:80 {
    respond /health "OK" 200
    reverse_proxy 127.0.0.1:8000
}

# Konfigurasi Auto-HTTPS FQDN Institusi
25832072008.5opsi.id {
    reverse_proxy 127.0.0.1:8000
    encode gzip zstd
}

```

---

## 3. Matriks Verifikasi Port & Keamanan Jaringan (UFW)

* **Port 80 (HTTP)**: Open (Digunakan untuk verifikasi tantangan ACME Let's Encrypt dan pengalihan otomatis ke HTTPS).
* **Port 443 (HTTPS)**: Open (Digunakan untuk enkripsi lalu lintas TLS/SSL pada `25832072008.5opsi.id`).
* **Port 8000 (Flask/Gunicorn)**: Isolated (`127.0.0.1` saja, tidak dapat diakses langsung dari IP publik).

---

## 4. Kesimpulan Auditing

Temuan Audit 04 mengenai kejelasan FQDN dan HTTPS telah ditutup. Domain `25832072008.5opsi.id` telah ditetapkan sebagai FQDN proyek, dengan arsitektur reverse proxy Caddy Server yang siap melakukan penerbitan sertifikat TLS otomatis (Auto-HTTPS) serta pengisolasian port backend Gunicorn.

```
