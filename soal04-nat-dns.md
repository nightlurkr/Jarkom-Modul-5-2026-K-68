# Soal 4: NAT dan DNS

Ketik di CONSOLE node, bukan di Edit network configuration.
Editor itu membuang baris yang mengandung tanda lebih besar, dan di sini ada
banyak, yaitu `cat >`, `> /etc/resolv.conf`, dan `2>/dev/null`.

Tiap blok di bawah menimpa seluruh isi `/root/<nama>.sh`, jadi cukup tempel
satu blok per router dan tidak perlu menggabung dengan yang lama.

---

## Bagian A: 17 router

### Gendou

```bash
cat > /root/gendou.sh <<'EOF'
#!/bin/bash

sysctl -w net.ipv4.ip_forward=1
echo "nameserver 8.8.8.8" > /etc/resolv.conf

# Yui
ip route replace 192.245.0.0/23 via 192.245.4.110
ip route replace 192.245.3.0/25 via 192.245.4.110
ip route replace 192.245.4.0/27 via 192.245.4.110
ip route replace 192.245.4.32/28 via 192.245.4.110
ip route replace 192.245.4.136/29 via 192.245.4.110
ip route replace 192.245.4.144/30 via 192.245.4.110
# Hikari
ip route replace 192.245.2.0/24 via 192.245.4.114
ip route replace 192.245.3.224/27 via 192.245.4.114
ip route replace 192.245.4.148/30 via 192.245.4.114
# Asuka
ip route replace 192.245.3.128/26 via 192.245.4.118
ip route replace 192.245.4.48/29 via 192.245.4.118
ip route replace 192.245.4.96/30 via 192.245.4.118
ip route replace 192.245.4.152/29 via 192.245.4.118
# Kaji
ip route replace 192.245.4.100/30 via 192.245.4.122
# Misato
ip route replace 192.245.4.64/29 via 192.245.4.126
# Shigeru
ip route replace 192.245.4.80/28 via 192.245.4.130
ip route replace 192.245.4.160/30 via 192.245.4.130
# Fuyutsuki
ip route replace 192.245.3.192/27 via 192.245.4.134
ip route replace 192.245.4.56/29 via 192.245.4.134
ip route replace 192.245.4.72/29 via 192.245.4.134
ip route replace 192.245.4.164/30 via 192.245.4.134
ip route replace 192.245.4.168/30 via 192.245.4.134

iptables -t nat -C POSTROUTING -s 192.245.0.0/21 -o eth0 -j MASQUERADE 2>/dev/null || \
  iptables -t nat -A POSTROUTING -s 192.245.0.0/21 -o eth0 -j MASQUERADE
EOF
chmod +x /root/gendou.sh && /root/gendou.sh
```

### Yui

```bash
cat > /root/yui.sh <<'EOF'
#!/bin/bash

sysctl -w net.ipv4.ip_forward=1
echo "nameserver 8.8.8.8" > /etc/resolv.conf

ip route replace default via 192.245.4.109
# Rei
ip route replace 192.245.0.0/23 via 192.245.4.138
# Mari
ip route replace 192.245.4.32/28 via 192.245.4.142
# Shinji
ip route replace 192.245.4.0/27 via 192.245.4.146
EOF
chmod +x /root/yui.sh && /root/yui.sh
```

### Rei

```bash
cat > /root/rei.sh <<'EOF'
#!/bin/bash

sysctl -w net.ipv4.ip_forward=1
echo "nameserver 8.8.8.8" > /etc/resolv.conf

ip route replace default via 192.245.4.137
EOF
chmod +x /root/rei.sh && /root/rei.sh
```

### Mari

```bash
cat > /root/mari.sh <<'EOF'
#!/bin/bash

sysctl -w net.ipv4.ip_forward=1
echo "nameserver 8.8.8.8" > /etc/resolv.conf

ip route replace default via 192.245.4.141
EOF
chmod +x /root/mari.sh && /root/mari.sh
```

### Shinji

```bash
cat > /root/shinji.sh <<'EOF'
#!/bin/bash

sysctl -w net.ipv4.ip_forward=1
echo "nameserver 8.8.8.8" > /etc/resolv.conf

ip route replace default via 192.245.4.145
EOF
chmod +x /root/shinji.sh && /root/shinji.sh
```

### Hikari

```bash
cat > /root/hikari.sh <<'EOF'
#!/bin/bash

sysctl -w net.ipv4.ip_forward=1
echo "nameserver 8.8.8.8" > /etc/resolv.conf

ip route replace default via 192.245.4.113
# Kaworu
ip route replace 192.245.3.224/27 via 192.245.4.150
EOF
chmod +x /root/hikari.sh && /root/hikari.sh
```

### Kaworu

```bash
cat > /root/kaworu.sh <<'EOF'
#!/bin/bash

sysctl -w net.ipv4.ip_forward=1
echo "nameserver 8.8.8.8" > /etc/resolv.conf

ip route replace default via 192.245.4.149
EOF
chmod +x /root/kaworu.sh && /root/kaworu.sh
```

### Asuka

```bash
cat > /root/asuka.sh <<'EOF'
#!/bin/bash

sysctl -w net.ipv4.ip_forward=1
echo "nameserver 8.8.8.8" > /etc/resolv.conf

ip route replace default via 192.245.4.117
# Kensuke
ip route replace 192.245.3.128/26 via 192.245.4.154
# Ritsuko
ip route replace 192.245.4.48/29 via 192.245.4.158
EOF
chmod +x /root/asuka.sh && /root/asuka.sh
```

### Kensuke

```bash
cat > /root/kensuke.sh <<'EOF'
#!/bin/bash

sysctl -w net.ipv4.ip_forward=1
echo "nameserver 8.8.8.8" > /etc/resolv.conf

ip route replace default via 192.245.4.153
EOF
chmod +x /root/kensuke.sh && /root/kensuke.sh
```

### Ritsuko

```bash
cat > /root/ritsuko.sh <<'EOF'
#!/bin/bash

sysctl -w net.ipv4.ip_forward=1
echo "nameserver 8.8.8.8" > /etc/resolv.conf

ip route replace default via 192.245.4.157
EOF
chmod +x /root/ritsuko.sh && /root/ritsuko.sh
```

### Kaji

```bash
cat > /root/kaji.sh <<'EOF'
#!/bin/bash

sysctl -w net.ipv4.ip_forward=1
echo "nameserver 8.8.8.8" > /etc/resolv.conf

ip route replace default via 192.245.4.121
EOF
chmod +x /root/kaji.sh && /root/kaji.sh
```

### Misato

```bash
cat > /root/misato.sh <<'EOF'
#!/bin/bash

sysctl -w net.ipv4.ip_forward=1
echo "nameserver 8.8.8.8" > /etc/resolv.conf

ip route replace default via 192.245.4.125
EOF
chmod +x /root/misato.sh && /root/misato.sh
```

### Shigeru

```bash
cat > /root/shigeru.sh <<'EOF'
#!/bin/bash

sysctl -w net.ipv4.ip_forward=1
echo "nameserver 8.8.8.8" > /etc/resolv.conf

ip route replace default via 192.245.4.129
# Makoto
ip route replace 192.245.4.88/29 via 192.245.4.162
EOF
chmod +x /root/shigeru.sh && /root/shigeru.sh
```

### Makoto

```bash
cat > /root/makoto.sh <<'EOF'
#!/bin/bash

sysctl -w net.ipv4.ip_forward=1
echo "nameserver 8.8.8.8" > /etc/resolv.conf

ip route replace default via 192.245.4.161
EOF
chmod +x /root/makoto.sh && /root/makoto.sh
```

### Fuyutsuki

```bash
cat > /root/fuyutsuki.sh <<'EOF'
#!/bin/bash

sysctl -w net.ipv4.ip_forward=1
echo "nameserver 8.8.8.8" > /etc/resolv.conf

ip route replace default via 192.245.4.133
# Maya
ip route replace 192.245.3.192/27 via 192.245.4.166
# Toji
ip route replace 192.245.4.72/29 via 192.245.4.170
EOF
chmod +x /root/fuyutsuki.sh && /root/fuyutsuki.sh
```

### Maya

```bash
cat > /root/maya.sh <<'EOF'
#!/bin/bash

sysctl -w net.ipv4.ip_forward=1
echo "nameserver 8.8.8.8" > /etc/resolv.conf

ip route replace default via 192.245.4.165
EOF
chmod +x /root/maya.sh && /root/maya.sh
```

### Toji

```bash
cat > /root/toji.sh <<'EOF'
#!/bin/bash

sysctl -w net.ipv4.ip_forward=1
echo "nameserver 8.8.8.8" > /etc/resolv.conf

ip route replace default via 192.245.4.169
EOF
chmod +x /root/toji.sh && /root/toji.sh
```

---

## Bagian B: node uji

Alamat sama seperti soal 3, ditambah DNS supaya bisa resolve nama domain.

### Armisael

```bash
ip addr replace 192.245.4.83/29 dev eth0
ip route replace default via 192.245.4.81
echo "nameserver 8.8.8.8" > /etc/resolv.conf
```

### Zeruel

```bash
ip addr replace 192.245.4.82/29 dev eth0
ip route replace default via 192.245.4.81
echo "nameserver 8.8.8.8" > /etc/resolv.conf
```

### Lilith

```bash
ip addr replace 192.245.4.74/29 dev eth0
ip route replace default via 192.245.4.73
echo "nameserver 8.8.8.8" > /etc/resolv.conf
```

### Sachiel

```bash
ip addr replace 192.245.3.130/26 dev eth0
ip route replace default via 192.245.3.129
echo "nameserver 8.8.8.8" > /etc/resolv.conf
```

### Comm-Net

```bash
ip addr replace 192.245.2.2/24 dev eth0
ip route replace default via 192.245.2.1
echo "nameserver 8.8.8.8" > /etc/resolv.conf
```

---

## Bagian C: bukti yang diminta soal 4

### Dari Armisael

```bash
ping -c2 8.8.8.8
ping -c2 deb.debian.org
```

### Dari Lilith

```bash
ping -c2 8.8.8.8
ping -c2 deb.debian.org
```

### Counter rule NAT, di Gendou

```bash
iptables -t nat -L POSTROUTING -v -n
```

Kolom `pkts` dan `bytes` harus lebih dari nol setelah ping di atas dijalankan.
Kalau masih nol, berarti paketnya tidak lewat rule itu.

### Membuktikan traffic antar-LAN TIDAK di-NAT

Soal minta ini juga, dan paling gampang dibuktikan dengan membandingkan counter.

```bash
iptables -t nat -Z POSTROUTING
```

Lalu dari Armisael, ping sesama LAN internal:

```bash
ping -c5 192.245.2.2    # Comm-Net
```

Cek lagi di Gendou:

```bash
iptables -t nat -L POSTROUTING -v -n
```

Counternya harus tetap NOL, karena traffic antar-LAN tidak keluar lewat eth0.
Lalu ping 8.8.8.8 dari Armisael dan cek sekali lagi, counternya baru naik.
Perbandingan dua kondisi itu bukti paling kuat untuk syarat soal ini.
