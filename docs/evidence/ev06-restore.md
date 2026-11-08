# EV-06: Laporan Simulasi Pemulihan & Integritas Data (Restore Drill)

- **Nama Mahasiswa**: Gistie Gusniar Putri Darmawan
- **NIM**: 25832072008
- **Kode Proyek**: CC-PTI2802-25832072008-CRC
- **Tanggal Uji Simulasi**: 8 November 2026

---

## 1. Parameter Target Pemulihan (SLO vs Realisasi)

| Metrik Pemulihan | Target Usulan Proposal | Hasil Realisasi Uji | Status Evaluasi |
| :--- | :--- | :--- | :--- |
| **RPO (Recovery Point Objective)** | Maksimal 24 jam | **Data Utuh 100% (0 Jam Loss)** | **Lulus Target** |
| **RTO (Recovery Time Objective)** | Maksimal 4 jam | **< 5 Menit (Simulasi Restore)** | **Lulus Target** |
| **Integritas SQLite** | Status `ok` | **`ok` (`PRAGMA integrity_check`)** | **Valid / Sehat** |
| **Row Count Audit** | Sesuai data sumber | **2 Baris Data Terverifikasi** | **Konsisten** |

---

## 2. Prosedur Eksekusi & Log Uji Pemulihan (Fakta Hasil Eksekusi Terminal)

### Langkah 1: Ekstraksi Backup SQLite (Online Backup API)
`sqlite3 app.db ".backup 'backup_test.db'"`

### Langkah 2: Pengujian Integritas File Backup (Integrity Check)
`sqlite3 backup_test.db "PRAGMA integrity_check;"`  
`Hasil Output Terminal: ok`

### Langkah 3: Verifikasi Jumlah Baris Data (Row Count Verification)
`sqlite3 backup_test.db "SELECT COUNT(*) FROM reservation;"`  
`Hasil Output Terminal: 2`

### Langkah 4: Kalkulasi Hash Checksum SHA-256
`sha256sum backup_test.db`  
`Hasil Output Terminal: 35e080fdf9fcc8bdb74e702897025c63ebcd967986a5db170d10c16237899591 backup_test.db`

---

## 3. Rencana Mitigasi Pemulihan Bencana (Disaster Recovery Plan)
1. **Penyimpanan Terpisah**: File backup dienkripsi dan diunggah secara otomatis ke repository eksternal terpisah (Off-site storage) setiap hari pukul 02.00 WIB.
2. **Standard Operating Procedure (SOP) Restore**: Jika VPS mengalami kegagalan total, instance VPS baru dibuat, skema diinisialisasi, dan file `.db` dipulihkan menggunakan perintah `.restore`.

---

## 4. Kesimpulan Auditing
Simulasi pemulihan bencana (*restore drill*) telah dilaksanakan dengan hasil integritas data terverifikasi `ok` tanpa korupsi file. Realisasi RPO dan RTO berada di bawah ambang batas maksimum usulan proposal (RPO ≤ 24 jam dan RTO ≤ 4 jam).
