# Soal 5: Firewall Gendou

Ketik di CONSOLE Gendou. Blok ini menimpa seluruh isi `/root/gendou.sh`,
jadi sudah termasuk soal 2, 3, dan 4. Tidak perlu menggabung sendiri.

---

## Gendou

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

# NAT ke NAT1
iptables -t nat -C POSTROUTING -s 192.245.0.0/21 -o eth0 -j MASQUERADE 2>/dev/null || \
  iptables -t nat -A POSTROUTING -s 192.245.0.0/21 -o eth0 -j MASQUERADE

# policy
iptables -P INPUT DROP
iptables -P FORWARD DROP
iptables -P OUTPUT ACCEPT

# INPUT
iptables -C INPUT -i lo -j ACCEPT 2>/dev/null || \
  iptables -A INPUT -i lo -j ACCEPT
iptables -C INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT 2>/dev/null || \
  iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
iptables -C INPUT -i eth0 -p udp --sport 67 --dport 68 -j ACCEPT 2>/dev/null || \
  iptables -A INPUT -i eth0 -p udp --sport 67 --dport 68 -j ACCEPT
iptables -C INPUT ! -i eth0 -s 192.245.0.0/21 -j ACCEPT 2>/dev/null || \
  iptables -A INPUT ! -i eth0 -s 192.245.0.0/21 -j ACCEPT

# FORWARD
iptables -C FORWARD -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT 2>/dev/null || \
  iptables -A FORWARD -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
iptables -C FORWARD -s 192.245.0.0/21 -o eth0 -j ACCEPT 2>/dev/null || \
  iptables -A FORWARD -s 192.245.0.0/21 -o eth0 -j ACCEPT
iptables -C FORWARD ! -i eth0 ! -o eth0 -s 192.245.0.0/21 -d 192.245.0.0/21 -j ACCEPT 2>/dev/null || \
  iptables -A FORWARD ! -i eth0 ! -o eth0 -s 192.245.0.0/21 -d 192.245.0.0/21 -j ACCEPT
EOF
chmod +x /root/gendou.sh && /root/gendou.sh
```

---

## Bukti yang diminta soal

### 1. Aturan di Gendou

```bash
iptables -S
```

Dua baris pertama harus `-P INPUT DROP` dan `-P FORWARD DROP`.

### 2. Dari Armisael, internet masih jalan

```bash
ping -c2 8.8.8.8
ping -c2 deb.debian.org
```

---

## Uji bahwa soal 2 sampai 4 tidak rusak

Soal menulis syarat ini eksplisit, jadi sebaiknya dibuktikan juga.

### Soal 2, Gendou ping tetangganya

```bash
ping -c2 192.245.4.110    # Yui
ping -c2 192.245.4.134    # Fuyutsuki
```

### Soal 2, tetangga ping Gendou

Dari console Yui:

```bash
ping -c2 192.245.4.109    # Gendou
```

### Soal 3, antar-LAN lewat Gendou

Dari console Armisael:

```bash
ping -c2 192.245.4.74     # Lilith
ping -c2 192.245.2.2      # Comm-Net
```

### Soal 4, DHCP eth0 tidak putus

Di Gendou:

```bash
ip -br a show eth0
```

Alamat `192.168.122.x` harus masih ada. Kalau hilang, rule DHCP-nya salah.
