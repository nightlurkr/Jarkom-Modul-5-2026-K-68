# Soal 2: Alamat tiap interface router

Aturan yang dipakai:

1. eth0 setiap router menghadap ke atas, ke arah Gendou. Gendou eth0 ke NAT1.
2. Pada link antar router, router INDUK mendapat usable pertama,
   router ANAK mendapat usable kedua.
3. Pada LAN, router gateway mendapat usable pertama.
4. Gendou eth0 memakai dhcp, bukan alamat statis, karena dari NAT1.


## Gendou

| Interface | Tujuan | Alamat |
|---|---|---|
| eth0 | NAT1 | dhcp |
| eth1 | Switch-Cmd | 192.245.4.105/30 |
| eth2 | Yui | 192.245.4.109/30 |
| eth3 | Hikari | 192.245.4.113/30 |
| eth4 | Asuka | 192.245.4.117/30 |
| eth5 | Kaji | 192.245.4.121/30 |
| eth6 | Misato | 192.245.4.125/30 |
| eth7 | Shigeru | 192.245.4.129/30 |
| eth8 | Fuyutsuki | 192.245.4.133/30 |

```
# /etc/network/interfaces
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
```

## Yui

| Interface | Tujuan | Alamat |
|---|---|---|
| eth0 | Gendou | 192.245.4.110/30 |
| eth1 | Switch-Hangar | 192.245.3.1/25 |
| eth2 | Rei | 192.245.4.137/30 |
| eth3 | Mari | 192.245.4.141/30 |
| eth4 | Shinji | 192.245.4.145/30 |

```
# /etc/network/interfaces
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
```

## Rei

| Interface | Tujuan | Alamat |
|---|---|---|
| eth0 | Yui | 192.245.4.138/30 |
| eth1 | Switch-Quarters | 192.245.0.1/23 |

```
# /etc/network/interfaces
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
```

## Mari

| Interface | Tujuan | Alamat |
|---|---|---|
| eth0 | Yui | 192.245.4.142/30 |
| eth1 | Switch-Dock | 192.245.4.33/28 |

```
# /etc/network/interfaces
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
```

## Shinji

| Interface | Tujuan | Alamat |
|---|---|---|
| eth0 | Yui | 192.245.4.146/30 |
| eth1 | Switch-Garden | 192.245.4.1/27 |

```
# /etc/network/interfaces
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
```

## Hikari

| Interface | Tujuan | Alamat |
|---|---|---|
| eth0 | Gendou | 192.245.4.114/30 |
| eth1 | Switch-Comm | 192.245.2.1/24 |
| eth2 | Kaworu | 192.245.4.149/30 |

```
# /etc/network/interfaces
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
```

## Kaworu

| Interface | Tujuan | Alamat |
|---|---|---|
| eth0 | Hikari | 192.245.4.150/30 |
| eth1 | Switch-Relay | 192.245.3.225/27 |

```
# /etc/network/interfaces
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
```

## Asuka

| Interface | Tujuan | Alamat |
|---|---|---|
| eth0 | Gendou | 192.245.4.118/30 |
| eth1 | Switch-Archive | 192.245.4.97/30 |
| eth2 | Kensuke | 192.245.4.153/30 |
| eth3 | Ritsuko | 192.245.4.157/30 |

```
# /etc/network/interfaces
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
```

## Kensuke

| Interface | Tujuan | Alamat |
|---|---|---|
| eth0 | Asuka | 192.245.4.154/30 |
| eth1 | Switch-Bunker | 192.245.3.129/26 |

```
# /etc/network/interfaces
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
```

## Ritsuko

| Interface | Tujuan | Alamat |
|---|---|---|
| eth0 | Asuka | 192.245.4.158/30 |
| eth1 | Switch-Lab | 192.245.4.49/29 |

```
# /etc/network/interfaces
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
```

## Kaji

| Interface | Tujuan | Alamat |
|---|---|---|
| eth0 | Gendou | 192.245.4.122/30 |
| eth1 | Switch-Intel | 192.245.4.101/30 |

```
# /etc/network/interfaces
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
```

## Misato

| Interface | Tujuan | Alamat |
|---|---|---|
| eth0 | Gendou | 192.245.4.126/30 |
| eth1 | Switch-Cage | 192.245.4.65/29 |

```
# /etc/network/interfaces
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
```

## Shigeru

| Interface | Tujuan | Alamat |
|---|---|---|
| eth0 | Gendou | 192.245.4.130/30 |
| eth1 | Switch-Monolith | 192.245.4.81/29 |
| eth2 | Makoto | 192.245.4.161/30 |

```
# /etc/network/interfaces
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
```

## Makoto

| Interface | Tujuan | Alamat |
|---|---|---|
| eth0 | Shigeru | 192.245.4.162/30 |
| eth1 | Switch-Radar | 192.245.4.89/29 |

```
# /etc/network/interfaces
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
```

## Fuyutsuki

| Interface | Tujuan | Alamat |
|---|---|---|
| eth0 | Gendou | 192.245.4.134/30 |
| eth1 | Switch-Terminal | 192.245.4.57/29 |
| eth2 | Maya | 192.245.4.165/30 |
| eth3 | Toji | 192.245.4.169/30 |

```
# /etc/network/interfaces
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
```

## Maya

| Interface | Tujuan | Alamat |
|---|---|---|
| eth0 | Fuyutsuki | 192.245.4.166/30 |
| eth1 | Switch-Medic | 192.245.3.193/27 |

```
# /etc/network/interfaces
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
```

## Toji

| Interface | Tujuan | Alamat |
|---|---|---|
| eth0 | Fuyutsuki | 192.245.4.170/30 |
| eth1 | Switch-Dogma | 192.245.4.73/29 |

```
# /etc/network/interfaces
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
```

## Mengaktifkan IP forwarding

Di SETIAP router, isi /root/<nama>.sh. Nanti file yang sama
ditambahi rute di soal 3, jadi jangan ditulis ulang dari nol.

```bash
#!/bin/bash
sysctl -w net.ipv4.ip_forward=1
```

Jalankan: `chmod +x /root/<nama>.sh && /root/<nama>.sh`

## Bukti yang diminta soal 2

1. `ip -br a` di tiap router, 17 screenshot
2. Ping dua ujung tiap link router ke router, 16 pengujian

Daftar ping yang harus dibuktikan:

| Link | Dari | Ke |
|---|---|---|
| Gendou-Yui | Gendou (192.245.4.109) | Yui (192.245.4.110) |
| Gendou-Hikari | Gendou (192.245.4.113) | Hikari (192.245.4.114) |
| Gendou-Asuka | Gendou (192.245.4.117) | Asuka (192.245.4.118) |
| Gendou-Kaji | Gendou (192.245.4.121) | Kaji (192.245.4.122) |
| Gendou-Misato | Gendou (192.245.4.125) | Misato (192.245.4.126) |
| Gendou-Shigeru | Gendou (192.245.4.129) | Shigeru (192.245.4.130) |
| Gendou-Fuyutsuki | Gendou (192.245.4.133) | Fuyutsuki (192.245.4.134) |
| Yui-Rei | Yui (192.245.4.137) | Rei (192.245.4.138) |
| Yui-Mari | Yui (192.245.4.141) | Mari (192.245.4.142) |
| Yui-Shinji | Yui (192.245.4.145) | Shinji (192.245.4.146) |
| Hikari-Kaworu | Hikari (192.245.4.149) | Kaworu (192.245.4.150) |
| Asuka-Kensuke | Asuka (192.245.4.153) | Kensuke (192.245.4.154) |
| Asuka-Ritsuko | Asuka (192.245.4.157) | Ritsuko (192.245.4.158) |
| Shigeru-Makoto | Shigeru (192.245.4.161) | Makoto (192.245.4.162) |
| Fuyutsuki-Maya | Fuyutsuki (192.245.4.165) | Maya (192.245.4.166) |
| Fuyutsuki-Toji | Fuyutsuki (192.245.4.169) | Toji (192.245.4.170) |