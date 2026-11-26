# Soal 6: IP statis 25 node

Ketik di CONSOLE node. Blok ini menulis `/etc/network/interfaces`,
satu-satunya file selain `/root` yang bertahan setelah node restart,
jadi alamatnya otomatis pulih sendiri tanpa perlu dijalankan ulang.

Dua baris `ip` di akhir menerapkan alamatnya SEKARANG juga tanpa restart.
Sengaja tidak memakai `ifup`, karena perintah itu tidak ada di image debinet.

Aturan alamat dari glosarium: gateway memakai IP usable pertama,
node berikutnya urut kiri ke kanan sesuai gambar soal.

CATATAN ASUMSI: di Switch-Medic, gambar menaruh Medic-Bay di kiri dan
Pen-Pen di kanan, sedangkan tabel host di soal menomori Pen-Pen lebih dulu.
Glosarium bilang ikut gambar, jadi Medic-Bay yang dapat alamat lebih dulu.
Tulis asumsi ini di laporan.

---

## Tabel alamat

| Node | Switch | Alamat | Netmask | Gateway |
|---|---|---|---|---|
| Quarters | Switch-Quarters | 192.245.0.2/23 | 255.255.254.0 | 192.245.0.1 |
| Comm-Net | Switch-Comm | 192.245.2.2/24 | 255.255.255.0 | 192.245.2.1 |
| Hangar-Crew | Switch-Hangar | 192.245.3.2/25 | 255.255.255.128 | 192.245.3.1 |
| Sachiel | Switch-Bunker | 192.245.3.130/26 | 255.255.255.192 | 192.245.3.129 |
| Medic-Bay | Switch-Medic | 192.245.3.194/27 | 255.255.255.224 | 192.245.3.193 |
| Pen-Pen | Switch-Medic | 192.245.3.195/27 | 255.255.255.224 | 192.245.3.193 |
| Relay-Node | Switch-Relay | 192.245.3.226/27 | 255.255.255.224 | 192.245.3.225 |
| Garden-Staff | Switch-Garden | 192.245.4.2/27 | 255.255.255.224 | 192.245.4.1 |
| Dock-Crew | Switch-Dock | 192.245.4.34/28 | 255.255.255.240 | 192.245.4.33 |
| Entry-Plug | Switch-Lab | 192.245.4.50/29 | 255.255.255.248 | 192.245.4.49 |
| Dummy-Plug | Switch-Lab | 192.245.4.51/29 | 255.255.255.248 | 192.245.4.49 |
| Magi-Casper | Switch-Terminal | 192.245.4.58/29 | 255.255.255.248 | 192.245.4.57 |
| Magi-Balthasar | Switch-Terminal | 192.245.4.59/29 | 255.255.255.248 | 192.245.4.57 |
| Magi-Melchior | Switch-Terminal | 192.245.4.60/29 | 255.255.255.248 | 192.245.4.57 |
| Unit-01 | Switch-Cage | 192.245.4.66/29 | 255.255.255.248 | 192.245.4.65 |
| Unit-02 | Switch-Cage | 192.245.4.67/29 | 255.255.255.248 | 192.245.4.65 |
| Unit-00 | Switch-Cage | 192.245.4.68/29 | 255.255.255.248 | 192.245.4.65 |
| Lilith | Switch-Dogma | 192.245.4.74/29 | 255.255.255.248 | 192.245.4.73 |
| Adam | Switch-Dogma | 192.245.4.75/29 | 255.255.255.248 | 192.245.4.73 |
| Zeruel | Switch-Monolith | 192.245.4.82/29 | 255.255.255.248 | 192.245.4.81 |
| Armisael | Switch-Monolith | 192.245.4.83/29 | 255.255.255.248 | 192.245.4.81 |
| Radar-Array | Switch-Radar | 192.245.4.90/29 | 255.255.255.248 | 192.245.4.89 |
| Archive-Vault | Switch-Archive | 192.245.4.98/30 | 255.255.255.252 | 192.245.4.97 |
| Spy | Switch-Intel | 192.245.4.102/30 | 255.255.255.252 | 192.245.4.101 |
| Tokyo3-Core | Switch-Cmd | 192.245.4.106/30 | 255.255.255.252 | 192.245.4.105 |

---

## Command per node

### Quarters

```bash
cat > /etc/network/interfaces <<'EOF'
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
	address 192.245.0.2
	netmask 255.255.254.0
	gateway 192.245.0.1
	post-up echo "nameserver 8.8.8.8" > /etc/resolv.conf
EOF
echo "nameserver 8.8.8.8" > /etc/resolv.conf
ip addr replace 192.245.0.2/23 dev eth0
ip route replace default via 192.245.0.1
```

### Comm-Net

```bash
cat > /etc/network/interfaces <<'EOF'
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
	address 192.245.2.2
	netmask 255.255.255.0
	gateway 192.245.2.1
	post-up echo "nameserver 8.8.8.8" > /etc/resolv.conf
EOF
echo "nameserver 8.8.8.8" > /etc/resolv.conf
ip addr replace 192.245.2.2/24 dev eth0
ip route replace default via 192.245.2.1
```

### Hangar-Crew

```bash
cat > /etc/network/interfaces <<'EOF'
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
	address 192.245.3.2
	netmask 255.255.255.128
	gateway 192.245.3.1
	post-up echo "nameserver 8.8.8.8" > /etc/resolv.conf
EOF
echo "nameserver 8.8.8.8" > /etc/resolv.conf
ip addr replace 192.245.3.2/25 dev eth0
ip route replace default via 192.245.3.1
```

### Sachiel

```bash
cat > /etc/network/interfaces <<'EOF'
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
	address 192.245.3.130
	netmask 255.255.255.192
	gateway 192.245.3.129
	post-up echo "nameserver 8.8.8.8" > /etc/resolv.conf
EOF
echo "nameserver 8.8.8.8" > /etc/resolv.conf
ip addr replace 192.245.3.130/26 dev eth0
ip route replace default via 192.245.3.129
```

### Medic-Bay

```bash
cat > /etc/network/interfaces <<'EOF'
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
	address 192.245.3.194
	netmask 255.255.255.224
	gateway 192.245.3.193
	post-up echo "nameserver 8.8.8.8" > /etc/resolv.conf
EOF
echo "nameserver 8.8.8.8" > /etc/resolv.conf
ip addr replace 192.245.3.194/27 dev eth0
ip route replace default via 192.245.3.193
```

### Pen-Pen

```bash
cat > /etc/network/interfaces <<'EOF'
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
	address 192.245.3.195
	netmask 255.255.255.224
	gateway 192.245.3.193
	post-up echo "nameserver 8.8.8.8" > /etc/resolv.conf
EOF
echo "nameserver 8.8.8.8" > /etc/resolv.conf
ip addr replace 192.245.3.195/27 dev eth0
ip route replace default via 192.245.3.193
```

### Relay-Node

```bash
cat > /etc/network/interfaces <<'EOF'
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
	address 192.245.3.226
	netmask 255.255.255.224
	gateway 192.245.3.225
	post-up echo "nameserver 8.8.8.8" > /etc/resolv.conf
EOF
echo "nameserver 8.8.8.8" > /etc/resolv.conf
ip addr replace 192.245.3.226/27 dev eth0
ip route replace default via 192.245.3.225
```

### Garden-Staff

```bash
cat > /etc/network/interfaces <<'EOF'
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
	address 192.245.4.2
	netmask 255.255.255.224
	gateway 192.245.4.1
	post-up echo "nameserver 8.8.8.8" > /etc/resolv.conf
EOF
echo "nameserver 8.8.8.8" > /etc/resolv.conf
ip addr replace 192.245.4.2/27 dev eth0
ip route replace default via 192.245.4.1
```

### Dock-Crew

```bash
cat > /etc/network/interfaces <<'EOF'
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
	address 192.245.4.34
	netmask 255.255.255.240
	gateway 192.245.4.33
	post-up echo "nameserver 8.8.8.8" > /etc/resolv.conf
EOF
echo "nameserver 8.8.8.8" > /etc/resolv.conf
ip addr replace 192.245.4.34/28 dev eth0
ip route replace default via 192.245.4.33
```

### Entry-Plug

```bash
cat > /etc/network/interfaces <<'EOF'
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
	address 192.245.4.50
	netmask 255.255.255.248
	gateway 192.245.4.49
	post-up echo "nameserver 8.8.8.8" > /etc/resolv.conf
EOF
echo "nameserver 8.8.8.8" > /etc/resolv.conf
ip addr replace 192.245.4.50/29 dev eth0
ip route replace default via 192.245.4.49
```

### Dummy-Plug

```bash
cat > /etc/network/interfaces <<'EOF'
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
	address 192.245.4.51
	netmask 255.255.255.248
	gateway 192.245.4.49
	post-up echo "nameserver 8.8.8.8" > /etc/resolv.conf
EOF
echo "nameserver 8.8.8.8" > /etc/resolv.conf
ip addr replace 192.245.4.51/29 dev eth0
ip route replace default via 192.245.4.49
```

### Magi-Casper

```bash
cat > /etc/network/interfaces <<'EOF'
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
	address 192.245.4.58
	netmask 255.255.255.248
	gateway 192.245.4.57
	post-up echo "nameserver 8.8.8.8" > /etc/resolv.conf
EOF
echo "nameserver 8.8.8.8" > /etc/resolv.conf
ip addr replace 192.245.4.58/29 dev eth0
ip route replace default via 192.245.4.57
```

### Magi-Balthasar

```bash
cat > /etc/network/interfaces <<'EOF'
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
	address 192.245.4.59
	netmask 255.255.255.248
	gateway 192.245.4.57
	post-up echo "nameserver 8.8.8.8" > /etc/resolv.conf
EOF
echo "nameserver 8.8.8.8" > /etc/resolv.conf
ip addr replace 192.245.4.59/29 dev eth0
ip route replace default via 192.245.4.57
```

### Magi-Melchior

```bash
cat > /etc/network/interfaces <<'EOF'
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
	address 192.245.4.60
	netmask 255.255.255.248
	gateway 192.245.4.57
	post-up echo "nameserver 8.8.8.8" > /etc/resolv.conf
EOF
echo "nameserver 8.8.8.8" > /etc/resolv.conf
ip addr replace 192.245.4.60/29 dev eth0
ip route replace default via 192.245.4.57
```

### Unit-01

```bash
cat > /etc/network/interfaces <<'EOF'
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
	address 192.245.4.66
	netmask 255.255.255.248
	gateway 192.245.4.65
	post-up echo "nameserver 8.8.8.8" > /etc/resolv.conf
EOF
echo "nameserver 8.8.8.8" > /etc/resolv.conf
ip addr replace 192.245.4.66/29 dev eth0
ip route replace default via 192.245.4.65
```

### Unit-02

```bash
cat > /etc/network/interfaces <<'EOF'
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
	address 192.245.4.67
	netmask 255.255.255.248
	gateway 192.245.4.65
	post-up echo "nameserver 8.8.8.8" > /etc/resolv.conf
EOF
echo "nameserver 8.8.8.8" > /etc/resolv.conf
ip addr replace 192.245.4.67/29 dev eth0
ip route replace default via 192.245.4.65
```

### Unit-00

```bash
cat > /etc/network/interfaces <<'EOF'
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
	address 192.245.4.68
	netmask 255.255.255.248
	gateway 192.245.4.65
	post-up echo "nameserver 8.8.8.8" > /etc/resolv.conf
EOF
echo "nameserver 8.8.8.8" > /etc/resolv.conf
ip addr replace 192.245.4.68/29 dev eth0
ip route replace default via 192.245.4.65
```

### Lilith

```bash
cat > /etc/network/interfaces <<'EOF'
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
	address 192.245.4.74
	netmask 255.255.255.248
	gateway 192.245.4.73
	post-up echo "nameserver 8.8.8.8" > /etc/resolv.conf
EOF
echo "nameserver 8.8.8.8" > /etc/resolv.conf
ip addr replace 192.245.4.74/29 dev eth0
ip route replace default via 192.245.4.73
```

### Adam

```bash
cat > /etc/network/interfaces <<'EOF'
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
	address 192.245.4.75
	netmask 255.255.255.248
	gateway 192.245.4.73
	post-up echo "nameserver 8.8.8.8" > /etc/resolv.conf
EOF
echo "nameserver 8.8.8.8" > /etc/resolv.conf
ip addr replace 192.245.4.75/29 dev eth0
ip route replace default via 192.245.4.73
```

### Zeruel

```bash
cat > /etc/network/interfaces <<'EOF'
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
	address 192.245.4.82
	netmask 255.255.255.248
	gateway 192.245.4.81
	post-up echo "nameserver 8.8.8.8" > /etc/resolv.conf
EOF
echo "nameserver 8.8.8.8" > /etc/resolv.conf
ip addr replace 192.245.4.82/29 dev eth0
ip route replace default via 192.245.4.81
```

### Armisael

```bash
cat > /etc/network/interfaces <<'EOF'
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
	address 192.245.4.83
	netmask 255.255.255.248
	gateway 192.245.4.81
	post-up echo "nameserver 8.8.8.8" > /etc/resolv.conf
EOF
echo "nameserver 8.8.8.8" > /etc/resolv.conf
ip addr replace 192.245.4.83/29 dev eth0
ip route replace default via 192.245.4.81
```

### Radar-Array

```bash
cat > /etc/network/interfaces <<'EOF'
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
	address 192.245.4.90
	netmask 255.255.255.248
	gateway 192.245.4.89
	post-up echo "nameserver 8.8.8.8" > /etc/resolv.conf
EOF
echo "nameserver 8.8.8.8" > /etc/resolv.conf
ip addr replace 192.245.4.90/29 dev eth0
ip route replace default via 192.245.4.89
```

### Archive-Vault

```bash
cat > /etc/network/interfaces <<'EOF'
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
	address 192.245.4.98
	netmask 255.255.255.252
	gateway 192.245.4.97
	post-up echo "nameserver 8.8.8.8" > /etc/resolv.conf
EOF
echo "nameserver 8.8.8.8" > /etc/resolv.conf
ip addr replace 192.245.4.98/30 dev eth0
ip route replace default via 192.245.4.97
```

### Spy

```bash
cat > /etc/network/interfaces <<'EOF'
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
	address 192.245.4.102
	netmask 255.255.255.252
	gateway 192.245.4.101
	post-up echo "nameserver 8.8.8.8" > /etc/resolv.conf
EOF
echo "nameserver 8.8.8.8" > /etc/resolv.conf
ip addr replace 192.245.4.102/30 dev eth0
ip route replace default via 192.245.4.101
```

### Tokyo3-Core

```bash
cat > /etc/network/interfaces <<'EOF'
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
	address 192.245.4.106
	netmask 255.255.255.252
	gateway 192.245.4.105
	post-up echo "nameserver 8.8.8.8" > /etc/resolv.conf
EOF
echo "nameserver 8.8.8.8" > /etc/resolv.conf
ip addr replace 192.245.4.106/30 dev eth0
ip route replace default via 192.245.4.105
```

---

## Bukti yang diminta soal

Di console Unit-01:

```bash
ip -br a
ping -c2 192.245.4.65
```

Alamatnya harus `192.245.4.66/29` dan ping ke gateway 0% loss.

## Uji tambahan: bertahan setelah restart

Soal minta alamatnya bertahan, jadi sebaiknya dibuktikan.

Stop lalu start node Unit-01 dari GNS3, buka console lagi:

```bash
ip -br a
ping -c2 192.245.4.65
```

Alamatnya harus muncul sendiri tanpa diketik ulang.
