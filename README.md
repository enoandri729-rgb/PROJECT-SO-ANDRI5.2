
#### Langkah 1: Buat Script Batch
1. Buka Notepad (atau editor teks lainnya).
2. Salin kode script berikut ke dalam file baru:
   ```
   @echo off
   echo Network Diagnostic Report > network_diagnostic.txt
   echo. >> network_diagnostic.txt
   echo 1. IP Configuration: >> network_diagnostic.txt
   ipconfig /all >> network_diagnostic.txt
   echo. >> network_diagnostic.txt
   echo 2. Ping to 8.8.8.8: >> network_diagnostic.txt
   ping 8.8.8.8 >> network_diagnostic.txt
   echo. >> network_diagnostic.txt
   echo 3. Ping to google.com: >> network_diagnostic.txt
   ping google.com >> network_diagnostic.txt
   echo. >> network_diagnostic.txt
   echo 4. Trace route to google.com: >> network_diagnostic.txt
   tracert google.com >> network_diagnostic.txt
   echo. >> network_diagnostic.txt
   echo 5. Active Connections: >> network_diagnostic.txt
   netstat -an >> network_diagnostic.txt
   echo Diagnostic complete. Check network_diagnostic.txt
   ```
3. Simpan file sebagai `network_diagnostic.bat` (pastikan ekstensi .bat, bukan .txt). Simpan di folder yang mudah diakses, seperti Desktop.

#### Langkah 2: Jalankan Script
1. Buka Command Prompt sebagai Administrator (cari CMD, klik kanan, "Run as administrator").
2. Navigasi ke folder tempat script disimpan, misalnya: `cd Desktop`.
3. Jalankan script: `network_diagnostic.bat`.
4. Script akan berjalan otomatis dan membuat file `network_diagnostic.txt` di folder yang sama.

#### Langkah 3: Dokumentasi dan Analisis Hasil
Buka file `network_diagnostic.txt` untuk melihat hasil. Berikut adalah penjelasan singkat untuk setiap langkah dan analisis apakah jaringan berfungsi normal (berdasarkan hasil tipikal):

1. **IP Configuration (ipconfig /all)**: Menampilkan detail IP, subnet mask, gateway, DNS, dll.  
   - **Normal**: IP valid (mis. 192.168.x.x), gateway dan DNS terdeteksi, tidak ada error seperti "Media disconnected". Jika IP kosong atau error, periksa koneksi Wi-Fi/kabel.

2. **Ping to 8.8.8.8**: Menguji koneksi internet ke server Google DNS.  
   - **Normal**: Mendapat reply dalam 10-100ms, tanpa packet loss (0% loss). Jika timeout atau loss tinggi, ada masalah koneksi internet.

3. **Ping to google.com**: Menguji resolusi DNS dan koneksi ke website.  
   - **Normal**: Reply berhasil dengan waktu rendah, mirip dengan ping sebelumnya. Jika gagal, masalah DNS atau firewall.

4. **Trace Route to google.com (tracert)**: Melacak jalur paket ke Google.  
   - **Normal**: Menampilkan beberapa hop (langkah) dengan waktu rendah, mencapai tujuan. Jika stuck atau error, ada bottleneck atau routing issue.

5. **Active Connections (netstat -an)**: Menampilkan koneksi TCP/UDP aktif.  
   - **Normal**: Daftar koneksi (mis. ke port 80/443 untuk web), tanpa koneksi mencurigakan. Jika banyak koneksi tidak dikenal, periksa malware.

Jika semua hasil normal, jaringan berfungsi baik. Jika ada masalah, coba restart router, periksa firewall, atau hubungi penyedia internet. Untuk troubleshooting lanjutan, gunakan tools seperti Wireshark.
