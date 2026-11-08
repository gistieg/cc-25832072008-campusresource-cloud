# EV-09: Laporan Pengujian Spesifik Domain & Logika Bisnis Aplikasi

- **Nama Mahasiswa**: Gistie Gusniar Putri Darmawan
- **NIM**: 25832072008
- **Kode Proyek**: CC-PTI2802-25832072008-CRC
- **Tanggal Penyusunan**: 8 November 2026

---

## 1. Matriks Skenario Uji Logika Bisnis (Domain-Specific Testing)

| ID Uji | Fitur / Skenario Domain | Ekspektasi Logika | Hasil Eksekusi | Status |
| :--- | :--- | :--- | :--- | :--- |
| **TC-01** | Validasi Bentrok Jadwal Ruangan | Penolakan reservasi jika waktu & ruangan sama | HTTP 409 Conflict / Warning | **Lulus** |
| **TC-02** | Otorisasi Role Mahasiswa vs Admin | Mahasiswa tidak dapat menyetujui (*approve*) reservasi | HTTP 403 Forbidden | **Lulus** |
| **TC-03** | Sanitasi Input Form Reservasi | Pencegahan SQL Injection pada nama peminjam | Input Di-escape / Parameterized | **Lulus** |
| **TC-04** | Pembatasan Kuota Pinjam Alat | Maksimal 2 alat per organisasi per hari | Validation Error Message | **Lulus** |

---

## 2. Pengujian Unit & Logika Bisnis (Automated Test Suite)

### Skrip Pengujian Matriks Akses & Bentrok (`tests/test_domain_rules.py`)

```python
import unittest
from app import app, db

class TestCampusResourceDomain(unittest.TestCase):
    def setUp(self):
        self.app = app.test_client()
        self.app.testing = True

    def test_schedule_conflict_prevention(self):
        """TC-01: Memastikan jadwal yang sama ditolak sistem"""
        # Reservasi Pertama
        res1 = self.app.post('/api/reservations', json={
            'facility_name': 'Ruang Rapat A',
            'borrower': 'HIMADIKTI',
            'date': '2026-11-10',
            'status': 'approved'
        })
        # Reservasi Kedua pada Jadwal Sama (Harus Ditolak)
        res2 = self.app.post('/api/reservations', json={
            'facility_name': 'Ruang Rapat A',
            'borrower': 'UKM Seni',
            'date': '2026-11-10',
            'status': 'pending'
        })
        self.assertEqual(res2.status_code, 409)

if __name__ == '__main__':
    unittest.main()

```

---

## 3. Hasil Ringkasan Auditing Sistem

1. **Integritas Aturan Bisnis**: Seluruh fungsi spesifik domain aplikasi reservasi sumber daya kampus telah berjalan sesuai dengan spesifikasi kebutuhan pengguna.
2. **Keamanan Otorisasi**: Proteksi hak akses (*Role-Based Access Control*) memastikan pengguna non-admin tidak dapat mengubah status reservasi.

---

## 4. Kesimpulan Auditing

Seluruh parameter audit teknis untuk proyek **CampusResource Cloud** (EV-01 sampai EV-09) telah diselesaikan, terverifikasi secara fungsional, dan tersimpan utuh di dalam repositori ini.

```
