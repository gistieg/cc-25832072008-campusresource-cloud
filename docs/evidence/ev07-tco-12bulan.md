# EV-07: Analisis TCO 12 Bulan & Manajemen Kredit Azure for Students

- **Nama Mahasiswa**: Gistie Gusniar Putri Darmawan
- **NIM**: 25832072008
- **Kode Proyek**: CC-PTI2802-25832072008-CRC
- **Tanggal Penyusunan**: 8 November 2026

---

## 1. Transparansi Status Kredit & Provider Azure (Temuan Audit 06)

| Parameter Layanan | Status Aktual (Azure for Students) | Proyeksi Pasca Kredit (Tarif Normal) |
| :--- | :--- | :--- |
| **Sumber Pendanaan** | Kredit Azure for Students ($100 USD / Tahun) | Anggaran Akademik / Mandiri |
| **Tarif Komputasi (VM)** | Rp 0 (Dicover Kredit Azure) | Rp 180.000 / bulan |
| **Public IPv4** | Rp 0 (Dicover Kredit Azure) | Rp 45.000 / bulan |
| **OS Disk (Storage)** | Rp 0 (Dicover Kredit Azure) | Rp 30.000 / bulan |
| **Domain FQDN** | Rp 0 (Belum membeli domain) | Rp 201.000 / tahun (Asumsi) |
| **Burn-Rate Bulanan** | ~$12–14 USD / bulan | Rp 255.000 / bulan |
| **Estimasi Umur Kredit** | 7–8 Bulan Operasional Kontinu | N/A |

---

## 2. Register Biaya Total Cost of Ownership (TCO) 12 Bulan

```text
+------------------------------------+------------------+------------------+
| Komponen Biaya                     | Biaya Kas Aktual | Estimasi TCO     |
|                                    | (Dengan Kredit)  | (Skenario Penuh) |
+------------------------------------+------------------+------------------+
| Compute & Network Azure (12 Bulan) | Rp 0             | Rp 2.160.000     |
| Public IP & Storage (12 Bulan)     | Rp 0             | Rp 900.000       |
| Alokasi Domain (.ac.id / .com)     | Rp 0             | Rp 201.000       |
| Backup External Storage (12 Bulan) | Rp 0 (GitHub)    | Rp 120.000       |
| Contingency (10%)                  | Rp 0             | Rp 338.100       |
+------------------------------------+------------------+------------------+
| TOTAL TCO 12 BULAN                 | Rp 0             | Rp 3.719.100     |
+------------------------------------+------------------+------------------+

```

---

## 3. Strategi Mitigasi Saldo Kredit (Budget Alerts & Shutdown Policy)

1. **Setting Azure Cost Alert**: Memasang peringatan email pada threshold 50%, 75%, dan 90% penggunaan saldo kredit $100 Azure.
2. **Auto-Shutdown Schedule**: Mengaktifkan fitur Auto-Shutdown di Portal Azure pada pukul 23.00 WIB setiap hari jika VM tidak digunakan untuk aktivitas praktikum, guna menghemat *burn-rate* CPU.
3. **Kriteria Go/No-Go Pasca Kredit**: Jika kredit Azure habis sebelum M16, instance akan ditransisi ke *deallocated state* dan basis data di-export via skrip `ev05-backup.md`.

---

## 4. Kesimpulan Auditing

Temuan Audit 06 telah ditutup. Dokumen telah memperjelas perbedaan antara tarif kas aktual (Rp 0 selama menggunakan kredit Azure for Students) dengan tarif TCO operasional normal 12 bulan (Rp 3.719.100). Peringatan anggaran dan skenario transisi setelah kredit habis telah ditetapkan.

```
