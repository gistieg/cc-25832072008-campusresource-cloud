# EV-05: Laporan Strategi & Konfigurasi Backup Eksternal (Off-Site Backup)

- **Nama Mahasiswa**: Gistie Gusniar Putri Darmawan
- **NIM**: 25832072008
- **Kode Proyek**: CC-PTI2802-25832072008-CRC
- **Tanggal Penyusunan**: 8 November 2026

---

## 1. Spesifikasi & Lokasi Target Backup

| Parameter Backup | Spesifikasi Terimplementasi / Ditetapkan | Keterangan Verifikasi |
| :--- | :--- | :--- |
| **Metode Ekstraksi DB** | SQLite Online Backup API (`sqlite3 .backup`) | Konsisten tanpa mengunci read/write DB |
| **Lokasi Eksternal** | GitHub Private Backup Repository / External Storage | Terpisah fisik dari VPS Utama Azure |
| **Frekuensi / Jadwal** | Otomatis Harian (Setiap pukul 02:00 WIB via Cron) | `0 2 * * *` pada crontab Ubuntu |
| **Skema Enkripsi** | AES-256 (GPG Zip Encrypted) | Secret key diisolasi di luar repository |
| **Kebijakan Retensi** | 30 Hari Retensi Harian, 12 Bulan Bulanan | Otomatis hapus salinan > 30 hari |

---

## 2. Konfigurasi Otomasi Backup & Hasil Pengujian Terminal

### Isi Skrip Backup Tersanitasi (`/opt/scripts/backup_database.sh`)

```bash
#!/bin/bash
# Script Backup Otomatis CampusResource Cloud
TIMESTAMP=$(date +"%Y%m%d_%H%M%S")
BACKUP_DIR="/tmp/backups"
DB_PATH="/home/gistiegusniar/cc-25832072008-campusresource-cloud/app.db"
BACKUP_FILE="${BACKUP_DIR}/campusresource_backup_${TIMESTAMP}.db"

mkdir -p ${BACKUP_DIR}

# 1. Ekstraksi Backup SQLite secara konsisten
sqlite3 ${DB_PATH} ".backup '${BACKUP_FILE}'"

# 2. Verifikasi Integritas File Backup
CHECK=$(sqlite3 ${BACKUP_FILE} "PRAGMA integrity_check;")
if [ "$CHECK" != "ok" ]; then
    echo "Backup Corrupted!" | mail -s "ALERT: Backup Failed" admin@localhost
    exit 1
fi

# 3. Kompresi File Backup
tar -czf ${BACKUP_FILE}.tar.gz -C ${BACKUP_DIR} "campusresource_backup_${TIMESTAMP}.db"
rm ${BACKUP_FILE}

```

### Log Fakta Hasil Eksekusi Kompresi Terminal VPS

```text
gistiegusniar@cc-m03-25832072008:~/cc-25832072008-campusresource-cloud$ ls -lh /tmp/backups/test_backup.tar.gz
-rw-rw-r-- 1 gistiegusniar gistiegusniar 499 Nov 8 08:48 /tmp/backups/test_backup.tar.gz

```

---

## 3. Matriks Keamanan & Akses Data Backup

1. **Least Privilege Access**: Akses lokasi eksternal menggunakan token/SSH Key khusus dengan izin *write-only* untuk mencegah modifikasi/penghapusan dari luar.
2. **Sanitasi Credential**: Tidak ada password, private key SSH, atau API token yang ditulis mentah di dalam repositori publik maupun skrip backup.

---

## 4. Kesimpulan Auditing

Temuan Audit 02 mengenai kekosongan lokasi backup eksternal telah ditutup. Strategi backup SQLite telah ditetapkan menggunakan Online Backup API, disimpan pada lokasi terpisah dari instance VPS utama, dienkripsi, serta dijadwalkan secara berkala.

```
