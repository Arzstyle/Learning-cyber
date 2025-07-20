# 📘 Week 1: Linux Lanjut, Virtualisasi, dan Lab Setup

## 🎯 Tujuan Minggu Ini
- Menguasai command line Linux (navigasi, scripting ringan, permission, user, logging)
- Setup environment lab pribadi: **Kali Linux (Attacker)** ↔ **Ubuntu (Target)**
- Menyiapkan tools penting dan koneksi antar VM untuk simulasi keamanan

---

## 🧠 Konsep Kunci & Ringkasan

### 🔹 1. Struktur Sistem Linux & User Privilege
- Struktur direktori: `/`, `/etc`, `/var`, `/home`, `/opt`, `/usr`
- Akses user: `sudo`, `su`, dan hak akses pengguna
- Manajemen user: `adduser`, `passwd`, `usermod`

### 🔹 2. Permission & Ownership
- Format permission: `drwxr-xr--`
- Perintah utama: `chmod`, `chown`, `chgrp`
- Spesial permission: SUID, SGID, Sticky Bit

> 🧠 **Analogi:**
> - `chmod`: Mengatur siapa yang boleh masuk rumah
> - `chown`: Pindah kepemilikan rumah
> - `sudo`: Meminjam kunci rumah bos

### 🔹 3. Navigasi File & Logging
- Navigasi file: `ls -al`, `cat`, `less`, `tail -f`, `grep`, `find`
- Lokasi log penting:
  - `/var/log/auth.log`
  - `/var/log/syslog`
  - `/var/log/ufw.log` (jika firewall aktif)
  - Gunakan `journalctl` untuk log systemd

---

## 💻 Setup Lab Virtualisasi

1. Install **VirtualBox** (jika belum tersedia)
2. Konfigurasi jaringan **host-only** agar Kali ↔ Ubuntu dapat saling terhubung
3. Gunakan **Snapshot VM** untuk backup sebelum/after simulasi

---

## 🛠 Tools Setup

### ✅ Kali Linux (Attacker)
| Tool         | Fungsi                        |
|--------------|-------------------------------|
| `nmap`       | Network scanner               |
| `netcat`     | Port scanner / listener       |
| `wireshark`  | Packet capture                |
| `hydra`      | Brute-force login tool        |
| `ping`, `traceroute`, `arp`, `dig` | Basic network tools |

### ✅ Ubuntu (Target)
| Komponen          | Fungsi                        |
|-------------------|-------------------------------|
| `openssh-server`  | Akses remote via SSH          |
| `apache2`         | Web server simulasi           |
| `ufw`             | Firewall sederhana            |
| `logrotate`, `rsyslog` | Logging dan manajemen log  |

---

## 🧪 Lab Manual: Simulasi & Praktik

### 📌 Simulasi 1: Ping & SSH dari Kali ke Ubuntu
```bash
ping [IP Ubuntu]
ssh [user]@[IP Ubuntu]
```


### 📌 Simulasi 2: Scan Port dari Kali (Nmap)
```bash
nmap -sS -Pn [IP Ubuntu]
```

### 📌 Simulasi 3: Cek Log SSH di Ubuntu
```bash
sudo tail -f /var/log/auth.log
```

---

## 🧩 Challenge Mingguan: “Silent Intrusion” 🕵️‍♂️

🎯 Simulasi:
Melakukan SSH brute-force atau port scanning ke server Ubuntu secara stealthy.

💡 Langkah:
Dari Kali Linux:

```bash
hydra -l user -P /usr/share/wordlists/rockyou.txt ssh://[IP Ubuntu]
```

Cek log di Ubuntu:
```bash
sudo tail -f /var/log/auth.log
```

🎯 Target:
- Identifikasi log yang muncul saat brute force
- Pahami seperti apa serangan terlihat dari sisi defender


---

✅ Evaluasi Jawaban Soal Week 2
🔹 1. "Permission -rw-r--r--"
✅ Benar! Owner bisa baca & tulis, group hanya baca, others hanya baca.
🔹 2. "Memberi akses eksekusi hanya ke owner"
✅ Benar secara konsep Tinggal implementasinya: chmod u+x file.sh
🔹 3. "Perbedaan sudo dan su"
✅ Tepat! sudo: jalankan satu perintah sebagai root, su: masuk ke shell root penuh
🔹 4. "10 log terakhir auth.log"
⚠️ Hampir benar Kamu tulis tail -f, tapi "-f" untuk live follow. Untuk lihat 10 baris terakhir: tail /var/log/auth.log atau tail -n 10 /var/log/auth.log
🔹 5. "chmod 777 artinya?"
✅ Tepat Itu full permission: read/write/execute untuk semua user (berbahaya kalau asal pakai 😬)
🔹 6. "Ganti owner ke alice"
✅ Benar! chown alice file.txt
🔹 7. "Scan port 22 tidak merespon, kenapa?"
✅ Tepat Beberapa kemungkinan: Firewall aktif, SSH belum jalan, Sistem block ICMP (jadi hasil scan terkesan "tidak hidup")
🔹 8. "Perbedaan nmap -sS -Pn vs -sT"
✅ Jawaban kamu bagus -sS: Stealth Scan / SYN Scan (tidak buka koneksi penuh → susah dideteksi), -sT: Full TCP Connect Scan (mudah dideteksi), -Pn: Skip ping → langsung scan walaupun host tidak merespon ping
🔹 9. "Deteksi brute force SSH"
✅ Tepat dan realistik Melalui: tail -f /var/log/auth.log Kamu akan lihat: Failed password for invalid user root from 192.168.x.x



| Skill                                            | Status |
| ------------------------------------------------ | ------ |
| Bisa koneksi Kali ↔ Ubuntu via SSH               | ✅ / ❌  |
| Bisa menjalankan Nmap scan                       | ✅ / ❌  |
| Paham struktur permission Linux                  | ✅ / ❌  |
| Bisa melihat dan membaca log Linux               | ✅ / ❌  |
| Bisa mempersiapkan VM untuk simulasi red vs blue | ✅ / ❌  |

















