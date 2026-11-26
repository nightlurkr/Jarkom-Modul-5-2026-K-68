# Hook post-up: pulihkan konfigurasi otomatis setelah restart

Ketik di CONSOLE router. Blok ini menimpa `/etc/network/interfaces`
dan menambahkan satu baris `post-up` di interface TERAKHIR tiap router,
supaya `/root/<nama>.sh` dijalankan setelah semua interface hidup.

Pola `up bash /root/<nama>.sh` ini sudah dipakai dan terbukti di Modul 2
(13 node debinet) dan Modul 3. Jadi tidak perlu diuji dulu, langsung pasang
ke 17 router.

Catatan dari Modul 2 dan 3: node harus dalam keadaan MATI saat mengedit
`/etc/network/interfaces` lewat menu GNS3. Kalau diketik lewat console
dengan `cat`, node boleh menyala.

---

## Toji

```bash
cat > /etc/network/interfaces <<'EOF'
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
	address 192.245.4.170
	netmask 255.255.255.252

auto eth1
iface eth1 inet static
	address 192.245.4.73
	netmask 255.255.255.248
	up bash /root/toji.sh
EOF
```

## Gendou

```bash
cat > /etc/network/interfaces <<'EOF'
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 192.245.4.105
	netmask 255.255.255.252

auto eth2
iface eth2 inet static
	address 192.245.4.109
	netmask 255.255.255.252

auto eth3
iface eth3 inet static
	address 192.245.4.113
	netmask 255.255.255.252

auto eth4
iface eth4 inet static
	address 192.245.4.117
	netmask 255.255.255.252

auto eth5
iface eth5 inet static
	address 192.245.4.121
	netmask 255.255.255.252

auto eth6
iface eth6 inet static
	address 192.245.4.125
	netmask 255.255.255.252

auto eth7
iface eth7 inet static
	address 192.245.4.129
	netmask 255.255.255.252

auto eth8
iface eth8 inet static
	address 192.245.4.133
	netmask 255.255.255.252
	up bash /root/gendou.sh
EOF
```

## Yui

```bash
cat > /etc/network/interfaces <<'EOF'
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
	address 192.245.4.110
	netmask 255.255.255.252

auto eth1
iface eth1 inet static
	address 192.245.3.1
	netmask 255.255.255.128

auto eth2
iface eth2 inet static
	address 192.245.4.137
	netmask 255.255.255.252

auto eth3
iface eth3 inet static
	address 192.245.4.141
	netmask 255.255.255.252

auto eth4
iface eth4 inet static
	address 192.245.4.145
	netmask 255.255.255.252
	up bash /root/yui.sh
EOF
```

## Rei

```bash
cat > /etc/network/interfaces <<'EOF'
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
	address 192.245.4.138
	netmask 255.255.255.252

auto eth1
iface eth1 inet static
	address 192.245.0.1
	netmask 255.255.254.0
	up bash /root/rei.sh
EOF
```

## Mari

```bash
cat > /etc/network/interfaces <<'EOF'
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
	address 192.245.4.142
	netmask 255.255.255.252

auto eth1
iface eth1 inet static
	address 192.245.4.33
	netmask 255.255.255.240
	up bash /root/mari.sh
EOF
```

## Shinji

```bash
cat > /etc/network/interfaces <<'EOF'
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
	address 192.245.4.146
	netmask 255.255.255.252

auto eth1
iface eth1 inet static
	address 192.245.4.1
	netmask 255.255.255.224
	up bash /root/shinji.sh
EOF
```

## Hikari

```bash
cat > /etc/network/interfaces <<'EOF'
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
	address 192.245.4.114
	netmask 255.255.255.252

auto eth1
iface eth1 inet static
	address 192.245.2.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 192.245.4.149
	netmask 255.255.255.252
	up bash /root/hikari.sh
EOF
```

## Kaworu

```bash
cat > /etc/network/interfaces <<'EOF'
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
	address 192.245.4.150
	netmask 255.255.255.252

auto eth1
iface eth1 inet static
	address 192.245.3.225
	netmask 255.255.255.224
	up bash /root/kaworu.sh
EOF
```

## Asuka

```bash
cat > /etc/network/interfaces <<'EOF'
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
	address 192.245.4.118
	netmask 255.255.255.252

auto eth1
iface eth1 inet static
	address 192.245.4.97
	netmask 255.255.255.252

auto eth2
iface eth2 inet static
	address 192.245.4.153
	netmask 255.255.255.252

auto eth3
iface eth3 inet static
	address 192.245.4.157
	netmask 255.255.255.252
	up bash /root/asuka.sh
EOF
```

## Kensuke

```bash
cat > /etc/network/interfaces <<'EOF'
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
	address 192.245.4.154
	netmask 255.255.255.252

auto eth1
iface eth1 inet static
	address 192.245.3.129
	netmask 255.255.255.192
	up bash /root/kensuke.sh
EOF
```

## Ritsuko

```bash
cat > /etc/network/interfaces <<'EOF'
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
	address 192.245.4.158
	netmask 255.255.255.252

auto eth1
iface eth1 inet static
	address 192.245.4.49
	netmask 255.255.255.248
	up bash /root/ritsuko.sh
EOF
```

## Kaji

```bash
cat > /etc/network/interfaces <<'EOF'
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
	address 192.245.4.122
	netmask 255.255.255.252

auto eth1
iface eth1 inet static
	address 192.245.4.101
	netmask 255.255.255.252
	up bash /root/kaji.sh
EOF
```

## Misato

```bash
cat > /etc/network/interfaces <<'EOF'
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
	address 192.245.4.126
	netmask 255.255.255.252

auto eth1
iface eth1 inet static
	address 192.245.4.65
	netmask 255.255.255.248
	up bash /root/misato.sh
EOF
```

## Shigeru

```bash
cat > /etc/network/interfaces <<'EOF'
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
	address 192.245.4.130
	netmask 255.255.255.252

auto eth1
iface eth1 inet static
	address 192.245.4.81
	netmask 255.255.255.248

auto eth2
iface eth2 inet static
	address 192.245.4.161
	netmask 255.255.255.252
	up bash /root/shigeru.sh
EOF
```

## Makoto

```bash
cat > /etc/network/interfaces <<'EOF'
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
	address 192.245.4.162
	netmask 255.255.255.252

auto eth1
iface eth1 inet static
	address 192.245.4.89
	netmask 255.255.255.248
	up bash /root/makoto.sh
EOF
```

## Fuyutsuki

```bash
cat > /etc/network/interfaces <<'EOF'
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
	address 192.245.4.134
	netmask 255.255.255.252

auto eth1
iface eth1 inet static
	address 192.245.4.57
	netmask 255.255.255.248

auto eth2
iface eth2 inet static
	address 192.245.4.165
	netmask 255.255.255.252

auto eth3
iface eth3 inet static
	address 192.245.4.169
	netmask 255.255.255.252
	up bash /root/fuyutsuki.sh
EOF
```

## Maya

```bash
cat > /etc/network/interfaces <<'EOF'
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
	address 192.245.4.166
	netmask 255.255.255.252

auto eth1
iface eth1 inet static
	address 192.245.3.193
	netmask 255.255.255.224
	up bash /root/maya.sh
EOF
```

---

# Node layanan debinet, 5 node

Lebih penting daripada router, karena di sini yang hilang saat restart
bukan cuma konfigurasi tapi juga PAKETNYA. Tanpa ini, setelah restart
iptables dan nc lenyap dan semua listener mati.

## Magi-Casper

```bash
cat > /etc/network/interfaces <<'EOF'
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
	address 192.245.4.58
	netmask 255.255.255.248
	gateway 192.245.4.57
	up bash /root/magi-casper.sh
EOF
```

## Magi-Balthasar

```bash
cat > /etc/network/interfaces <<'EOF'
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
	address 192.245.4.59
	netmask 255.255.255.248
	gateway 192.245.4.57
	up bash /root/magi-balthasar.sh
EOF
```

## Magi-Melchior

```bash
cat > /etc/network/interfaces <<'EOF'
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
	address 192.245.4.60
	netmask 255.255.255.248
	gateway 192.245.4.57
	up bash /root/magi-melchior.sh
EOF
```

## Pen-Pen

```bash
cat > /etc/network/interfaces <<'EOF'
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
	address 192.245.3.195
	netmask 255.255.255.224
	gateway 192.245.3.193
	up bash /root/pen-pen.sh
EOF
```

## Tokyo3-Core

```bash
cat > /etc/network/interfaces <<'EOF'
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
	address 192.245.4.106
	netmask 255.255.255.252
	gateway 192.245.4.105
	up bash /root/tokyo3-core.sh
EOF
```
