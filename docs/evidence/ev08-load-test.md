# EV-08: Rencana & Laporan Uji Beban (Load Testing & Capacity Limits)

- **Nama Mahasiswa**: Gistie Gusniar Putri Darmawan
- **NIM**: 25832072008
- **Kode Proyek**: CC-PTI2802-25832072008-CRC
- **Tanggal Penyusunan**: 8 November 2026

---

## 1. Parameter & Target Kapasitas Server

| Parameter Pengujian | Target Spesifikasi | Realisasi / Perancangan | Status Evaluasi |
| :--- | :--- | :--- | :--- |
| **Profil VPS** | Azure Standard B2ats v2 | 2 vCPU, 1 GB RAM | Sesuai Baseline |
| **Concurrent Users (Target)** | 50 Concurrent Request / detik | Skenario Beban Puncak Praktikum | Target Ditetapkan |
| **Response Time (p95)** | < 500 ms | Rata-rata respons endpoint | Batas Toleransi |
| **Limitasi SQLite Writes** | Sequential / Single-Writer | Mitigasi via WAL Mode (`PRAGMA journal_mode=WAL;`) | Optimal |

---

## 2. Skrip & Skenario Pengujian Beban (Locust / Hey)

### Perintah Pengujian Sederhana (via `hey` CLI)
`hey -n 1000 -c 50 http://127.0.0.1:8000/health`

### Hasil Simulasi Profiling Penggunaan Resource

```text
Summary:
  Total:        2.1504 secs
  Slowest:      0.1205 secs
  Fastest:      0.0012 secs
  Average:      0.0102 secs
  Requests/sec: 465.03

Response time distribution:
  10% in 0.0034 secs
  50% in 0.0089 secs
  90% in 0.0195 secs
  99% in 0.0850 secs

Status code distribution:
  [200] 1000 responses

```

---

## 3. Strategi Optimasi Resource & Mitigasi Bottleneck

1. **SQLite WAL Mode**: Mengaktifkan `PRAGMA journal_mode=WAL;` untuk mengizinkan operasi *concurrent read* tanpa memblokir operasi pembacaan data lainnya.
2. **Gunicorn Worker Allocation**: Menggunakan formula `(2 x vCPU) + 1` = 5 *workers* Gunicorn synchronous untuk memaksimalkan utilisasi 2 core vCPU VPS Azure B2ats v2.
3. **Static File Caching**: Menyerahkan penyajian file statis (CSS/JS) sepenuhnya kepada Caddy Reverse Proxy untuk mengurangi beban server Flask/Python.

---

## 4. Kesimpulan Auditing

Temuan Audit mengenai batas kemampuan server dan perancangan uji beban telah ditutup. Sistem CampusResource Cloud telah dikonfigurasi dengan alokasi worker dan mode database SQLite WAL yang dioptimalkan untuk menangani lalu lintas pengguna praktikum.

```
