# 📘 Week 2: Footprinting, Scanning, dan Enumeration

## 🎯 Tujuan Minggu Ini
- Memahami tahapan awal serangan (Recon → Enumeration)
- Menggunakan tools nyata seperti **Nmap**, **Nikto**, dan **Gobuster**
- Menganalisis target sebelum eksploitasi
- Memahami teknik scanning port, service, dan direktori tersembunyi

---

## 🧠 Konsep Kunci & Ringkasan

### 🔹 1. Footprinting vs Scanning vs Enumeration

| Tahap        | Tujuan                           | Contoh Tools                          |
|--------------|----------------------------------|---------------------------------------|
| Footprinting | Kumpulkan info pasif             | WHOIS, Google Dork, Shodan            |
| Scanning     | Identifikasi host, port, service | nmap, netcat, masscan                 |
| Enumeration  | Dapatkan detail login, banner    | nikto, gobuster, enum4linux, snmpwalk |

---

### 🔹 2. Passive vs Active Reconnaissance

- **Passive**: Tidak menyentuh target langsung (Google, DNSdumpster, whois)  
- **Active**: Melakukan scan langsung ke target (Nmap, ping, nikto)

> 🧠 **Analogi**  
> - Passive Recon = Mengintip dari jauh  
> - Active Recon = Menyentuh pagar rumah target buat ngecek gemboknya

---

### 🔹 3. Nmap: Network Mapper

```bash
# Ping scan
nmap -sn 192.168.56.0/24

# TCP connect scan
nmap -sT [IP]

# Stealth SYN scan
nmap -sS [IP]

# Port + Service + Version Detection
nmap -sV -p- [IP]

# Script scan (vuln detection)
nmap -sC -sV [IP]


---


🧠 Tips:

-p- → Scan semua 65535 port

-sC → Aktifkan default scripts

-A → All-in-one scan (bising & mudah terdeteksi 😬)

### 🔹 4. Nikto – Web Vulnerability Scanner
```bash
nikto -h http://[targetIP]
```
Tujuan: Mendeteksi file sensitif, config error, directory listing, dll.

### 🔹 5. Gobuster – Directory/Content Bruteforcer
```bash
gobuster dir -u http://[targetIP] -w /usr/share/wordlists/dirb/common.txt
```
Tujuan: Temukan direktori tersembunyi seperti /admin, /uploads, /backup

### 🔹 6. Service Enumeration (Manual + Tools)
| Service | Tool                  |
| ------- | --------------------- |
| HTTP    | Gobuster, Nikto       |
| SMB     | enum4linux, smbclient |
| FTP     | ftp, hydra            |
| SSH     | hydra                 |
| SNMP    | snmpwalk              |


---

## 🛠️ Lab Manual: Praktikum Minggu Ini
- Target VM: Ubuntu atau Metasploitable2
- Attacker VM: Kali Linux


### 📌 1. Scan Jaringan
```bash
nmap -sn 192.168.56.0/24
```
📍 Temukan IP target yang hidup.


### 📌 2. Full Port Scan + Service Detection
```bash
nmap -sC -sV -p- [IP Target]
```
📍 Catat port terbuka & service yang berjalan.


### 📌 3. Web Enumeration
Akses via browser: http://[IP]:80
Scan menggunakan:
```bash
nikto -h http://[IP]
gobuster dir -u http://[IP] -w /usr/share/wordlists/dirb/common.txt
```

### 📌 4. Enumerasi SMB (jika port 139/445 terbuka)
```bash
enum4linux -a [IP]
```

---


## 🧩 Challenge Mingguan: Recon & Enumerate! 🕵️‍♀️
🎯 Goal: Temukan “pintu masuk terbaik” ke sistem
Langkah:
1. Scan IP target
2. Temukan port dan service yang aktif
3. Enumerasi web dan service lain
4. Temukan direktori tersembunyi atau konfigurasi lemah
5. 📋 Laporkan:
      - Port terbuka
      - Versi service
      - Path mencurigakan
      - Potensi celah
📝 Dokumentasikan hasil sebagai log serangan ringan. Ini adalah latihan penting sebelum exploitasi.


---


# ✅ Evaluasi Jawaban Soal Week 2

### 🔹 1. Passive vs Active Recon
✅ **Tepat.** Passive tidak menyentuh langsung (whois, search engine), sedangkan active seperti `nmap`, `nikto`, dll menyentuh sistem target secara langsung.

### 🔹 2. Arti `nmap -sC -sV -p-`
✅ **Benar.** Itu artinya:
* Scan semua port (`-p-`)
* Jalankan default script (`-sC`)
* Deteksi versi service (`-sV`)

### 🔹 3. Kelemahan Scan `-A`
✅ **Tepat.** Scan jadi sangat **lambat**, dan **mudah terdeteksi** oleh IDS/IPS karena menggunakan banyak fitur (OS detect, script, traceroute, dll).

### 🔹 4. Fungsi Nikto
✅ **Tepat.** Untuk mendeteksi **file sensitif, konfigurasi salah, dan potensi celah** di web server.

### 🔹 5. Directory Brute Force vs Vulnerability Scan
✅ **Benar.**
* Directory brute force (`gobuster`) = cari path tersembunyi
* Vulnerability scanning (`nikto`, `nmap scripts`) = cari celah keamanan

### 🔹 6. Port 21 Terbuka – Langkah?
✅ **Cukup baik.**
* Cek banner pakai `ftp [ip]`
* Coba anonymous login
* Brute force pakai `hydra` kalau diperlukan
✍️ Catatan kecil: Port 21 = FTP, jadi pastikan kamu sebut "akses FTP" bukan TCP secara umum.

### 🔹 7. Mengetahui Versi Apache
✅ **Tepat.**
* Bisa pakai `nmap -sV`
* Atau lewat banner HTTP (via curl/wget)
* Juga bisa via `nikto` atau bahkan source HTML

### 🔹 8. Port 139/445 Terbuka – Langkah?
✅ **Tepat sekali.** Gunakan:
* `nmap --script=smb*`
* `enum4linux`
* `smbclient`
Tujuan: Enumerasi share, user, password policy, OS info, dll.

### 🔹 9. Risiko Scanning Tanpa Izin
✅ **Jawaban kamu sangat profesional.** Scanning tanpa izin = **bisa melanggar hukum (UU ITE, GDPR, CFAA, dll)** Hanya boleh dilakukan dengan **izin tertulis / lisensi / skenario lab.**

---

## 🎓 Nilai Akhir: 9/9 — Excellent!
💯 Kamu punya **kombinasi mindset dan skill** yang tepat untuk dunia pentesting:
* Tahu teknisnya
* Paham batasan etis dan legal
* Tahu kapan brute force, kapan recon, dan kapan hold







