# Soal 7 dan 8: Listener

Ketik di CONSOLE node. Blok ini menimpa `/root/<nama>.sh`.

Paket dipasang DI DALAM script, karena paket hilang setiap node restart.
Pola ini dari Modul 2 dan 3.

Pastikan jaringan dan NAT sudah hidup sebelum menjalankan, karena
`apt-get` butuh internet.

---

# Soal 7: MAGI

## Magi-Casper  (192.245.4.58)

```bash
cat > /root/magi-casper.sh <<'EOF'
#!/bin/bash

echo "nameserver 8.8.8.8" > /etc/resolv.conf

for i in $(seq 1 30); do ping -c1 -W1 8.8.8.8 >/dev/null 2>&1 && break; sleep 2; done

apt-get update -qq
apt-get install -y -qq iptables netcat-traditional

pkill -f 'nc -l -p 5000'
(while true; do nc -l -p 5000 >> /root/log-5000.txt; done) &
EOF
chmod +x /root/magi-casper.sh && /root/magi-casper.sh
```

## Magi-Balthasar  (192.245.4.59)

```bash
cat > /root/magi-balthasar.sh <<'EOF'
#!/bin/bash

echo "nameserver 8.8.8.8" > /etc/resolv.conf

for i in $(seq 1 30); do ping -c1 -W1 8.8.8.8 >/dev/null 2>&1 && break; sleep 2; done

apt-get update -qq
apt-get install -y -qq iptables netcat-traditional

pkill -f 'nc -l -p 5000'
(while true; do nc -l -p 5000 >> /root/log-5000.txt; done) &
pkill -f 'nc -l -p 5001'
(while true; do nc -l -p 5001 >> /root/log-5001.txt; done) &
EOF
chmod +x /root/magi-balthasar.sh && /root/magi-balthasar.sh
```

## Magi-Melchior  (192.245.4.60)

```bash
cat > /root/magi-melchior.sh <<'EOF'
#!/bin/bash

echo "nameserver 8.8.8.8" > /etc/resolv.conf

for i in $(seq 1 30); do ping -c1 -W1 8.8.8.8 >/dev/null 2>&1 && break; sleep 2; done

apt-get update -qq
apt-get install -y -qq iptables netcat-traditional

pkill -f 'nc -l -p 5000'
(while true; do nc -l -p 5000 >> /root/log-5000.txt; done) &
EOF
chmod +x /root/magi-melchior.sh && /root/magi-melchior.sh
```

## Kirim pesan dari Lilith

WAJIB pakai `-w 1`. Tanpa itu, nc versi BusyBox di alpinet tidak menutup
koneksi setelah mengirim, dan terlihat seperti nge-stuck padahal pesannya
sudah sampai.

Bersihkan log dulu di tiap MAGI supaya buktinya bersih:

```bash
> /root/log-5000.txt
```

Khusus Magi-Balthasar, dua file:

```bash
> /root/log-5000.txt
> /root/log-5001.txt
```

```bash
echo "halo Magi-Casper port 5000 dari Lilith" | nc -w 1 192.245.4.58 5000
echo "halo Magi-Balthasar port 5000 dari Lilith" | nc -w 1 192.245.4.59 5000
echo "halo Magi-Balthasar port 5001 dari Lilith" | nc -w 1 192.245.4.59 5001
echo "halo Magi-Melchior port 5000 dari Lilith" | nc -w 1 192.245.4.60 5000
```

## Tampilkan pesan yang diterima

Di console masing-masing MAGI:

```bash
cat /root/log-5000.txt
```

Khusus Magi-Balthasar, dua file:

```bash
cat /root/log-5000.txt
cat /root/log-5001.txt
```

---

# Soal 8: Pen-Pen dan Tokyo3-Core

Port 80 menjawab `Welcome to {hostname}`.
Port 8080 di Tokyo3-Core menjawab `syifa imutz`.

## Pen-Pen  (192.245.3.195)

```bash
cat > /root/pen-pen.sh <<'EOF'
#!/bin/bash

echo "nameserver 8.8.8.8" > /etc/resolv.conf

for i in $(seq 1 30); do ping -c1 -W1 8.8.8.8 >/dev/null 2>&1 && break; sleep 2; done

apt-get update -qq
apt-get install -y -qq iptables netcat-traditional

pkill -f 'nc -l -p 80'
(while true; do echo "Welcome to $(hostname)" | nc -l -p 80; done) &
EOF
chmod +x /root/pen-pen.sh && /root/pen-pen.sh
```

## Tokyo3-Core  (192.245.4.106)

```bash
cat > /root/tokyo3-core.sh <<'EOF'
#!/bin/bash

echo "nameserver 8.8.8.8" > /etc/resolv.conf

for i in $(seq 1 30); do ping -c1 -W1 8.8.8.8 >/dev/null 2>&1 && break; sleep 2; done

apt-get update -qq
apt-get install -y -qq iptables netcat-traditional

pkill -f 'nc -l -p 80'
(while true; do echo "Welcome to $(hostname)" | nc -l -p 80; done) &
pkill -f 'nc -l -p 8080'
(while true; do echo "syifa imutz" | nc -l -p 8080; done) &
EOF
chmod +x /root/tokyo3-core.sh && /root/tokyo3-core.sh
```

## Bukti dari Lilith

```bash
nc -w 2 192.245.3.195 80        # Pen-Pen, Welcome to Pen-Pen
nc -w 2 192.245.4.106 80        # Tokyo3-Core, Welcome to Tokyo3-Core
nc -w 2 192.245.4.106 8080      # Tokyo3-Core, syifa imutz
```

## Bukti port yang tidak dilayani gagal seketika

Soal minta ini eksplisit, harus gagal cepat, bukan menggantung.

```bash
time nc -w 5 192.245.4.106 22
echo $?
```

Harus selesai dalam waktu di bawah satu detik, bukan 5 detik, dan exit code 1.

Diuji dengan netcat-traditional versi 1.10-48: durasi 0,003 detik, exit code 1.
Penyebabnya tidak ada yang mendengarkan di port 22 dan tidak ada rule DROP,
jadi kernel langsung mengirim TCP RST. Kalau nanti di soal 16 Made memasang
rule DROP, perilakunya berubah jadi menggantung. Itu beda yang penting.

Catatan: `nc` versi BusyBox di alpinet tidak mencetak pesan error,
jadi pakai `echo $?` untuk melihat exit code kalau outputnya kosong.
