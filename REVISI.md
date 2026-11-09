# Tabel Status Tindak Lanjut Temuan Audit Dokumen

| No | Nomor Audit & Temuan | Tindakan Perbaikan | Bukti / File Verifikasi | Status |
| :-: | :--- | :--- | :--- | :-: |
| 1 | **01 Identitas VPS** (vCPU B2ats v2) | Mengoreksi spesifikasi menjadi 2 vCPU sesuai SKU Azure & menyelaraskan nproc/lscpu. | `Bab 1.3`, `docs/evidence/ev01-vps-spec.md` | **LULUS** |
| 2 | **02 Operasi & Pemulihan** (Lokasi Backup) | Menetapkannya di GitHub Private Repo terenkripsi GPG AES-256 dengan cron job. | `Bab 1.3`, `docs/evidence/ev05-backup.md` | **LULUS** |
| 3 | **03 Operasi & Pemulihan** (Uji Restore) | Melakukan simulasi insiden INC-01: RPO 0 menit, RTO 3m 45s, integrity_check: ok. | `Lampiran D`, `docs/evidence/ev06-restore.md` | **LULUS** |
| 4 | **04 Arsitektur Cloud** (FQDN & HTTPS) | Menetapkan domain 25832072008.5opsi.id & Caddy TLS reverse proxy. | `Bab 1.3`, `docs/evidence/ev02-https.md` | **LULUS** |
| 5 | **05 Arsitektur Cloud** (Load Test) | Memasukkan hasil pengujian `hey CLI`: 465 req/sec, p95 85 ms, error 0%. | `Bab 6.3`, `docs/evidence/ev08-load-test.md` | **LULUS** |
| 6 | **06 Biaya & Kelayakan** (TCO & Azure Credit) | Memisahkan Kas Aktual Rp 0 (Kredit Student) dengan TCO Normal 12 Bulan (Rp 3.719.100). | `Bab 10.1`, `docs/evidence/ev07-tco-12bulan.md` | **LULUS** |
| 7 | **07 Eksekusi & Bukti** (Bukti EV-01) | Menautkan log uname, os-release, free, df, ss, & UFW tersanitasi ke path Markdown riil. | `Bab 13.3`, `docs/evidence/ev01-vps-spec.md` | **LULUS** |
| 8 | **08 Risiko Khusus** (Bentrok Jadwal) | Menjalankan unit test `test_domain_rules.py` dengan penolakan HTTP 409 Conflict. | `Lampiran C`, `docs/evidence/ev09-uji-domain.md` | **LULUS** |
