# Soal 2: Command siap tempel

PENTING: ketik di CONSOLE node, bukan di menu Edit network configuration.
Editor itu membuang baris yang mengandung tanda lebih besar, dan heredoc
`cat > file` akan hilang diam-diam tanpa pesan error.

---

## Bagian A: aktifkan IP forwarding di 17 router

File ini nanti ditambahi rute di soal 3, jadi jangan ditulis ulang dari nol.
Khusus Gendou, nanti juga ditambahi NAT di soal 4 dan firewall di soal 5.

### Gendou

```bash
cat > /root/gendou.sh <<'EOF'
#!/bin/bash
sysctl -w net.ipv4.ip_forward=1
EOF
chmod +x /root/gendou.sh && /root/gendou.sh
```

### Yui

```bash
cat > /root/yui.sh <<'EOF'
#!/bin/bash
sysctl -w net.ipv4.ip_forward=1
EOF
chmod +x /root/yui.sh && /root/yui.sh
```

### Rei

```bash
cat > /root/rei.sh <<'EOF'
#!/bin/bash
sysctl -w net.ipv4.ip_forward=1
EOF
chmod +x /root/rei.sh && /root/rei.sh
```

### Mari

```bash
cat > /root/mari.sh <<'EOF'
#!/bin/bash
sysctl -w net.ipv4.ip_forward=1
EOF
chmod +x /root/mari.sh && /root/mari.sh
```

### Shinji

```bash
cat > /root/shinji.sh <<'EOF'
#!/bin/bash
sysctl -w net.ipv4.ip_forward=1
EOF
chmod +x /root/shinji.sh && /root/shinji.sh
```

### Hikari

```bash
cat > /root/hikari.sh <<'EOF'
#!/bin/bash
sysctl -w net.ipv4.ip_forward=1
EOF
chmod +x /root/hikari.sh && /root/hikari.sh
```

### Kaworu

```bash
cat > /root/kaworu.sh <<'EOF'
#!/bin/bash
sysctl -w net.ipv4.ip_forward=1
EOF
chmod +x /root/kaworu.sh && /root/kaworu.sh
```

### Asuka

```bash
cat > /root/asuka.sh <<'EOF'
#!/bin/bash
sysctl -w net.ipv4.ip_forward=1
EOF
chmod +x /root/asuka.sh && /root/asuka.sh
```

### Kensuke

```bash
cat > /root/kensuke.sh <<'EOF'
#!/bin/bash
sysctl -w net.ipv4.ip_forward=1
EOF
chmod +x /root/kensuke.sh && /root/kensuke.sh
```

### Ritsuko

```bash
cat > /root/ritsuko.sh <<'EOF'
#!/bin/bash
sysctl -w net.ipv4.ip_forward=1
EOF
chmod +x /root/ritsuko.sh && /root/ritsuko.sh
```

### Kaji

```bash
cat > /root/kaji.sh <<'EOF'
#!/bin/bash
sysctl -w net.ipv4.ip_forward=1
EOF
chmod +x /root/kaji.sh && /root/kaji.sh
```

### Misato

```bash
cat > /root/misato.sh <<'EOF'
#!/bin/bash
sysctl -w net.ipv4.ip_forward=1
EOF
chmod +x /root/misato.sh && /root/misato.sh
```

### Shigeru

```bash
cat > /root/shigeru.sh <<'EOF'
#!/bin/bash
sysctl -w net.ipv4.ip_forward=1
EOF
chmod +x /root/shigeru.sh && /root/shigeru.sh
```

### Makoto

```bash
cat > /root/makoto.sh <<'EOF'
#!/bin/bash
sysctl -w net.ipv4.ip_forward=1
EOF
chmod +x /root/makoto.sh && /root/makoto.sh
```

### Fuyutsuki

```bash
cat > /root/fuyutsuki.sh <<'EOF'
#!/bin/bash
sysctl -w net.ipv4.ip_forward=1
EOF
chmod +x /root/fuyutsuki.sh && /root/fuyutsuki.sh
```

### Maya

```bash
cat > /root/maya.sh <<'EOF'
#!/bin/bash
sysctl -w net.ipv4.ip_forward=1
EOF
chmod +x /root/maya.sh && /root/maya.sh
```

### Toji

```bash
cat > /root/toji.sh <<'EOF'
#!/bin/bash
sysctl -w net.ipv4.ip_forward=1
EOF
chmod +x /root/toji.sh && /root/toji.sh
```

Keluarannya harus `net.ipv4.ip_forward = 1`.

Cek ulang kapan saja dengan `sysctl net.ipv4.ip_forward`.

---

## Bagian B: 16 ping link antar router

Dikerjakan dari sisi INDUK saja, cukup 6 console.
Satu ping berhasil sudah membuktikan kedua ujung hidup,
karena balasannya datang dari ujung seberang.

### Dari Gendou, 7 link

Cek cepat dulu, semua harus OK:

```bash
for ip in 192.245.4.110 192.245.4.114 192.245.4.118 192.245.4.122 192.245.4.126 192.245.4.130 192.245.4.134; do printf '%-16s ' $ip; ping -c1 -W1 $ip >/dev/null 2>&1 && echo OK || echo GAGAL; done
```

Lalu untuk screenshot bukti:

```bash
ping -c2 192.245.4.110    # Yui
ping -c2 192.245.4.114    # Hikari
ping -c2 192.245.4.118    # Asuka
ping -c2 192.245.4.122    # Kaji
ping -c2 192.245.4.126    # Misato
ping -c2 192.245.4.130    # Shigeru
ping -c2 192.245.4.134    # Fuyutsuki
```

### Dari Yui, 3 link

Cek cepat dulu, semua harus OK:

```bash
for ip in 192.245.4.138 192.245.4.142 192.245.4.146; do printf '%-16s ' $ip; ping -c1 -W1 $ip >/dev/null 2>&1 && echo OK || echo GAGAL; done
```

Lalu untuk screenshot bukti:

```bash
ping -c2 192.245.4.138    # Rei
ping -c2 192.245.4.142    # Mari
ping -c2 192.245.4.146    # Shinji
```

### Dari Hikari, 1 link

Cek cepat dulu, semua harus OK:

```bash
for ip in 192.245.4.150; do printf '%-16s ' $ip; ping -c1 -W1 $ip >/dev/null 2>&1 && echo OK || echo GAGAL; done
```

Lalu untuk screenshot bukti:

```bash
ping -c2 192.245.4.150    # Kaworu
```

### Dari Asuka, 2 link

Cek cepat dulu, semua harus OK:

```bash
for ip in 192.245.4.154 192.245.4.158; do printf '%-16s ' $ip; ping -c1 -W1 $ip >/dev/null 2>&1 && echo OK || echo GAGAL; done
```

Lalu untuk screenshot bukti:

```bash
ping -c2 192.245.4.154    # Kensuke
ping -c2 192.245.4.158    # Ritsuko
```

### Dari Shigeru, 1 link

Cek cepat dulu, semua harus OK:

```bash
for ip in 192.245.4.162; do printf '%-16s ' $ip; ping -c1 -W1 $ip >/dev/null 2>&1 && echo OK || echo GAGAL; done
```

Lalu untuk screenshot bukti:

```bash
ping -c2 192.245.4.162    # Makoto
```

### Dari Fuyutsuki, 2 link

Cek cepat dulu, semua harus OK:

```bash
for ip in 192.245.4.166 192.245.4.170; do printf '%-16s ' $ip; ping -c1 -W1 $ip >/dev/null 2>&1 && echo OK || echo GAGAL; done
```

Lalu untuk screenshot bukti:

```bash
ping -c2 192.245.4.166    # Maya
ping -c2 192.245.4.170    # Toji
```

---

## Daftar lengkap 16 link

| Link | Induk | Anak |
|---|---|---|
| Gendou - Yui | Gendou 192.245.4.109 | Yui 192.245.4.110 |
| Gendou - Hikari | Gendou 192.245.4.113 | Hikari 192.245.4.114 |
| Gendou - Asuka | Gendou 192.245.4.117 | Asuka 192.245.4.118 |
| Gendou - Kaji | Gendou 192.245.4.121 | Kaji 192.245.4.122 |
| Gendou - Misato | Gendou 192.245.4.125 | Misato 192.245.4.126 |
| Gendou - Shigeru | Gendou 192.245.4.129 | Shigeru 192.245.4.130 |
| Gendou - Fuyutsuki | Gendou 192.245.4.133 | Fuyutsuki 192.245.4.134 |
| Yui - Rei | Yui 192.245.4.137 | Rei 192.245.4.138 |
| Yui - Mari | Yui 192.245.4.141 | Mari 192.245.4.142 |
| Yui - Shinji | Yui 192.245.4.145 | Shinji 192.245.4.146 |
| Hikari - Kaworu | Hikari 192.245.4.149 | Kaworu 192.245.4.150 |
| Asuka - Kensuke | Asuka 192.245.4.153 | Kensuke 192.245.4.154 |
| Asuka - Ritsuko | Asuka 192.245.4.157 | Ritsuko 192.245.4.158 |
| Shigeru - Makoto | Shigeru 192.245.4.161 | Makoto 192.245.4.162 |
| Fuyutsuki - Maya | Fuyutsuki 192.245.4.165 | Maya 192.245.4.166 |
| Fuyutsuki - Toji | Fuyutsuki 192.245.4.169 | Toji 192.245.4.170 |