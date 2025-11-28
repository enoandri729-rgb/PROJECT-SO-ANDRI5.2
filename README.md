# PROJECT-SO-ANDRI5.2

### Langkah 1: Cek Konfigurasi IP
Gunakan perintah `ip addr show` (lebih modern di Linux) untuk melihat detail IP, subnet, gateway, dll. Ini mirip `ipconfig /all` di Windows.

- Jalankan perintah:
  ```
  ip addr show
  ```
- Simpan hasil:
  ```
  ip addr show > ip_config.txt
  ```
- **Hasil Contoh** (dari sistem saya; hasil Anda akan berbeda):
  ```
  1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
      link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
      inet 127.0.0.1/8 scope host lo
         valid_lft forever preferred_lft forever
      inet6 ::1/128 scope host
         valid_lft forever preferred_lft forever
  2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
      link/ether 08:00:27:xx:xx:xx brd ff:ff:ff:ff:ff:ff
      inet 192.168.1.100/24 brd 192.168.1.255 scope global dynamic enp0s3
         valid_lft 86399sec preferred_lft 86399sec
      inet6 fe80::a00:27ff:fexx:xxxx/64 scope link
         valid_lft forever preferred_lft forever
  ```
- **Analisis**: Jika ada `inet` (IPv4) seperti 192.168.x.x, IP valid. Jika tidak ada, jaringan mungkin tidak terhubung. Normal jika ada alamat IP dan status "UP".

### Langkah 2: Test Koneksi Internet dengan Ping ke 8.8.8.8
Ping ke server DNS Google (8.8.8.8) untuk test koneksi dasar internet tanpa DNS.

- Jalankan perintah:
  ```
  ping -c 4 8.8.8.8
  ```
  ( `-c 4` berarti 4 paket; tanpa ini, ping terus menerus – stop dengan Ctrl+C).
- Simpan hasil:
  ```
  ping -c 4 8.8.8.8 > ping_8.8.8.8.txt
  ```
- **Hasil Contoh**:
  ```
  PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
  64 bytes from 8.8.8.8: icmp_seq=1 ttl=118 time=10.2 ms
  64 bytes from 8.8.8.8: icmp_seq=2 ttl=118 time=9.8 ms
  64 bytes from 8.8.8.8: icmp_seq=3 ttl=118 time=10.1 ms
  64 bytes from 8.8.8.8: icmp_seq=4 ttl=118 time=9.9 ms

  --- 8.8.8.8 ping statistics ---
  4 packets transmitted, 4 received, 0% packet loss, time 3004ms
  rtt min/avg/max/mdev = 9.800/9.975/10.200/0.150 ms
  ```
- **Analisis**: Jika semua paket diterima (0% loss) dan waktu rendah (<100ms), koneksi internet normal. Jika loss tinggi atau timeout, ada masalah jaringan (misalnya, router bermasalah).

### Langkah 3: Cek Koneksi ke Website dengan Ping ke google.com
Ping ke domain untuk test resolusi DNS dan koneksi ke website.

- Jalankan perintah:
  ```
  ping -c 4 google.com
  ```
- Simpan hasil:
  ```
  ping -c 4 google.com > ping_google.txt
  ```
- **Hasil Contoh** (mirip Langkah 2, tapi alamat IP akan resolve dari google.com):
  ```
  PING google.com (142.250.204.78) 56(84) bytes of data.
  ... (hasil ping serupa)
  ```
- **Analisis**: Jika berhasil seperti Langkah 2, DNS dan koneksi website normal. Jika gagal resolve (unknown host), masalah DNS (coba ganti DNS di `/etc/resolv.conf`).

### Langkah 4: Trace Route dengan Traceroute ke google.com
Lacak jalur paket ke google.com untuk melihat hop (router) yang dilewati.

- Jalankan perintah:
  ```
  traceroute google.com
  ```
  (Jika belum install: `sudo apt install traceroute`).
- Simpan hasil:
  ```
  traceroute google.com > traceroute_google.txt
  ```
- **Hasil Contoh**:
  ```
  traceroute to google.com (142.250.204.78), 30 hops max, 60 byte packets
   1  192.168.1.1 (192.168.1.1)  1.234 ms  1.456 ms  1.567 ms
   2  10.0.0.1 (10.0.0.1)  10.123 ms  10.234 ms  10.345 ms
   ... (beberapa hop)
  10  142.250.204.78 (142.250.204.78)  15.678 ms  15.789 ms  15.890 ms
  ```
- **Analisis**: Jika mencapai tujuan tanpa "* * *" (timeout), rute normal. Jika ada hop yang timeout, ada bottleneck (misalnya, ISP bermasalah). Waktu tinggi menunjukkan latensi.

### Langkah 5: Lihat Koneksi Aktif dengan Netstat
Lihat semua koneksi jaringan aktif (TCP/UDP).

- Jalankan perintah:
  ```
  netstat -an
  ```
  (Jika belum install: `sudo apt install net-tools`).
- Simpan hasil:
  ```
  netstat -an > netstat_active.txt
  ```
- **Hasil Contoh** (dipotong; fokus pada baris dengan IP):
  ```
  Active Internet connections (servers and established)
  Proto Recv-Q Send-Q Local Address           Foreign Address         State
  tcp        0      0 127.0.0.1:631           0.0.0.0:*               LISTEN
  tcp        0      0 192.168.1.100:22        0.0.0.0:*               LISTEN
  tcp        0      0 192.168.1.100:44321     142.250.204.78:443      ESTABLISHED
  udp        0      0 0.0.0.0:68              0.0.0.0:*               -
  ```
- **Analisis**: Jika ada koneksi "ESTABLISHED" ke IP eksternal (misalnya, 142.250.204.78 untuk Google), jaringan aktif. Jika hanya localhost, mungkin tidak ada koneksi internet. Port 22 (SSH) atau 443 (HTTPS) normal untuk browsing.

### Dokumentasi dan Analisis Keseluruhan
- **Ringkasan Hasil**: Jalankan semua perintah di atas dan simpan ke file (misalnya, di folder `~/NetworkDiag/`). Contoh struktur folder: `mkdir ~/NetworkDiag && cd ~/NetworkDiag`, lalu jalankan perintah dengan `> nama_file.txt`.
- **Analisis Normalitas Jaringan**:
  - **Normal**: IP valid, ping berhasil (0% loss, waktu rendah), traceroute mencapai tujuan, netstat menunjukkan koneksi aktif.
  - **Tidak Normal**: Jika ping gagal, cek router/WiFi. Jika traceroute timeout, hubungi ISP. Jika netstat kosong, restart jaringan (`sudo systemctl restart NetworkManager`).
  - **Kesimpulan**: Berdasarkan hasil, jaringan saya normal (contoh di atas). Jika Anda dapat ping tapi tidak browsing, masalah aplikasi/browser.

### Tips Tambahan untuk Mahasiswa Semester 1
- **Script Otomatis**: Buat script Bash untuk menjalankan semua sekaligus. Buat file `network_diag.sh`:
  ```
  #!/bin/bash
  echo "Network Diagnostic - $(date)" > network_report.txt
  echo "=== IP Config ===" >> network_report.txt
  ip addr show >> network_report.txt
  echo "=== Ping 8.8.8.8 ===" >> network_report.txt
  ping -c 4 8.8.8.8 >> network_report.txt
  echo "=== Ping google.com ===" >> network_report.txt
  ping -c 4 google.com >> network_report.txt
  echo "=== Traceroute google.com ===" >> network_report.txt
  traceroute google.com >> network_report.txt
  echo "=== Netstat Active ===" >> network_report.txt
  netstat -an >> network_report.txt
  echo "Report saved to network_report.txt"
  ```
  - Jalankan: `chmod +x network_diag.sh && ./network_diag.sh`.
- **Troubleshooting**: Jika perintah gagal, pastikan koneksi internet aktif. Untuk Lubuntu (ringan), pastikan paket terinstall.
- **Pelajari Lebih**: Baca manual dengan `man ping` atau `man traceroute` untuk opsi lanjutan.

Jika hasil Anda berbeda atau ada error, salin outputnya dan saya bantu analisis! Semangat belajar sistem operasi! 🚀
