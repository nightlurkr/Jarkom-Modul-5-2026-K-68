# Soal 3: Routing statis

Ketik di CONSOLE node, bukan di Edit network configuration.

Dipakai `ip route replace`, bukan `ip route add`, supaya aman dijalankan
berkali-kali. Ini syarat idempotency yang diminta soal 20.

Total rute statis: 31. 
Sebelas router daun tidak punya rute statis sama sekali, cukup default route.

---

Blok script 17 router dipindah ke `soal04-nat-dns.md`, karena di sana
isinya sudah lengkap dengan DNS dan NAT sekaligus. Pakai yang itu saja
supaya tidak ada dua versi file yang berbeda.

## Hook otomatis setelah restart

Opsional tapi disarankan. Tambahkan di interface TERAKHIR tiap router,
bukan di semua interface, supaya script jalan setelah semua interface hidup.

Contoh di Gendou, interface terakhirnya eth8:

```
auto eth8
iface eth8 inet static
	address 192.245.4.133
	netmask 255.255.255.252
	post-up bash /root/gendou.sh
```

Interface terakhir tiap router:

| Router | Interface terakhir |
|---|---|
| Gendou | eth8 |
| Yui | eth4 |
| Rei | eth1 |
| Mari | eth1 |
| Shinji | eth1 |
| Hikari | eth2 |
| Kaworu | eth1 |
| Asuka | eth3 |
| Kensuke | eth1 |
| Ritsuko | eth1 |
| Kaji | eth1 |
| Misato | eth1 |
| Shigeru | eth2 |
| Makoto | eth1 |
| Fuyutsuki | eth3 |
| Maya | eth1 |
| Toji | eth1 |

---

## IP sementara untuk node uji soal 3

Soal minta membuktikan Armisael bisa ping Lilith, Sachiel, dan Comm-Net.
Alamat di bawah ini sudah sesuai aturan soal 6, jadi tidak terbuang.

| Node | Alamat | Gateway |
|---|---|---|
| Armisael | 192.245.4.83/29 | 192.245.4.81 |
| Zeruel | 192.245.4.82/29 | 192.245.4.81 |
| Lilith | 192.245.4.74/29 | 192.245.4.73 |
| Sachiel | 192.245.3.130/26 | 192.245.3.129 |
| Comm-Net | 192.245.2.2/24 | 192.245.2.1 |

Pasang SEMUA lima node di bawah ini, bukan Armisael saja.
Node tujuan juga butuh alamat dan default route, karena balasan ping
harus tahu jalan pulang ke subnet pengirim.

**Armisael** (Switch-Monolith, console Armisael):

```bash
ip addr add 192.245.4.83/29 dev eth0
ip route add default via 192.245.4.81
```

**Zeruel** (Switch-Monolith, console Zeruel):

```bash
ip addr add 192.245.4.82/29 dev eth0
ip route add default via 192.245.4.81
```

**Lilith** (Switch-Dogma, console Lilith):

```bash
ip addr add 192.245.4.74/29 dev eth0
ip route add default via 192.245.4.73
```

**Sachiel** (Switch-Bunker, console Sachiel):

```bash
ip addr add 192.245.3.130/26 dev eth0
ip route add default via 192.245.3.129
```

**Comm-Net** (Switch-Comm, console Comm-Net):

```bash
ip addr add 192.245.2.2/24 dev eth0
ip route add default via 192.245.2.1
```

Cek dulu di tiap node dengan `ip -br a`, alamatnya harus muncul.

Baru setelah kelimanya terpasang, uji dari Armisael:

```bash
ping -c2 192.245.4.74    # Lilith
ping -c2 192.245.3.130    # Sachiel
ping -c2 192.245.2.2    # Comm-Net
```

Ketiganya harus 0% packet loss.

## Cara membaca kalau gagal

| Pesan | Artinya |
|---|---|
| Network is unreachable | router tidak punya rute, masalah routing |
| Destination Host Unreachable | rute ada, tapi tidak ada node di alamat tujuan |
| Timeout tanpa pesan | paket dibuang diam-diam, biasanya firewall |

Alamat pengirim pesan error menunjukkan sampai mana paket berhasil menempuh jalur.
