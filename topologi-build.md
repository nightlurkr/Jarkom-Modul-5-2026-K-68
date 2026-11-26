# Panduan Membangun Topologi, Modul 5

Dibaca dari topo_5.png. Link antar router berjumlah 16, sesuai soal.

Sudah dikonfirmasi: Toji tersambung ke Fuyutsuki, bukan ke Gendou.

## Pohon router

```
NAT1
 └─ Gendou .......... Switch-Cmd (Tokyo3-Core)
    ├─ Yui .......... Switch-Hangar (Hangar-Crew 100)
    │  ├─ Rei ....... Switch-Quarters (Quarters 500)
    │  ├─ Mari ...... Switch-Dock (Dock-Crew 12)
    │  └─ Shinji .... Switch-Garden (Garden-Staff 14)
    ├─ Hikari ....... Switch-Comm (Comm-Net 200)
    │  └─ Kaworu .... Switch-Relay (Relay-Node 25)
    ├─ Asuka ........ Switch-Archive (Archive-Vault 1)
    │  ├─ Kensuke ... Switch-Bunker (Sachiel 55)
    │  └─ Ritsuko ... Switch-Lab (Entry-Plug 3, Dummy-Plug 2)
    ├─ Kaji ......... Switch-Intel (Spy 1)
    ├─ Misato ....... Switch-Cage (Unit-01, Unit-02, Unit-00)
    ├─ Shigeru ...... Switch-Monolith (Zeruel, Armisael)
    │  └─ Makoto .... Switch-Radar (Radar-Array 4)
    └─ Fuyutsuki .... Switch-Terminal (Casper, Balthasar, Melchior)
       ├─ Maya ...... Switch-Medic (Pen-Pen 1, Medic-Bay 25)
       └─ Toji ...... Switch-Dogma (Lilith, Adam)
```

## Jumlah adapter tiap router

Atur lewat klik kanan node, Configure, Network, Adapters.
Jangan lewat Preferences, itu controller bersama.

| Router | Adapter | Tersambung ke |
|---|---|---|
| Gendou | 9 | NAT1, Switch-Cmd, Yui, Hikari, Asuka, Kaji, Misato, Shigeru, Fuyutsuki |
| Yui | 5 | Gendou, Switch-Hangar, Rei, Mari, Shinji |
| Asuka | 4 | Gendou, Switch-Archive, Kensuke, Ritsuko |
| Hikari | 3 | Gendou, Switch-Comm, Kaworu |
| Shigeru | 3 | Gendou, Switch-Monolith, Makoto |
| Fuyutsuki | 4 | Gendou, Switch-Terminal, Maya, Toji |
| Rei | 2 | Yui, Switch-Quarters |
| Mari | 2 | Yui, Switch-Dock |
| Shinji | 2 | Yui, Switch-Garden |
| Kaworu | 2 | Hikari, Switch-Relay |
| Kensuke | 2 | Asuka, Switch-Bunker |
| Ritsuko | 2 | Asuka, Switch-Lab |
| Kaji | 2 | Gendou, Switch-Intel |
| Misato | 2 | Gendou, Switch-Cage |
| Makoto | 2 | Shigeru, Switch-Radar |
| Toji | 2 | Fuyutsuki, Switch-Dogma |
| Maya | 2 | Fuyutsuki, Switch-Medic |

Total interface router = 50. Container cukup 1 adapter, bawaan.

Gendou butuh 9 adapter. Ini yang paling gampang terlewat, dan akibatnya
baru ketahuan waktu kabel keenam tidak bisa dicolok.

## Daftar 59 kabel

Aturan yang dipakai: eth0 tiap router selalu menghadap ke atas, ke arah Gendou.
Ini memudahkan soal 18 nanti, yang minta filter berdasarkan interface ke arah Gendou.

### Router ke router, 16 kabel

| No | Dari | Ke | Catatan |
|---|---|---|---|
| 1 | Gendou | Yui | eth0 Yui |
| 2 | Gendou | Hikari | eth0 Hikari |
| 3 | Gendou | Asuka | eth0 Asuka |
| 4 | Gendou | Kaji | eth0 Kaji |
| 5 | Gendou | Misato | eth0 Misato |
| 6 | Gendou | Shigeru | eth0 Shigeru |
| 7 | Fuyutsuki | Toji | eth0 Toji |
| 8 | Gendou | Fuyutsuki | eth0 Fuyutsuki |
| 9 | Yui | Rei | eth0 Rei |
| 10 | Yui | Mari | eth0 Mari |
| 11 | Yui | Shinji | eth0 Shinji |
| 12 | Hikari | Kaworu | eth0 Kaworu |
| 13 | Asuka | Kensuke | eth0 Kensuke |
| 14 | Asuka | Ritsuko | eth0 Ritsuko |
| 15 | Shigeru | Makoto | eth0 Makoto |
| 16 | Fuyutsuki | Maya | eth0 Maya |

### Router ke switch, 17 kabel

| No | Router | Switch |
|---|---|---|
| 17 | Gendou | Switch-Cmd |
| 18 | Yui | Switch-Hangar |
| 19 | Rei | Switch-Quarters |
| 20 | Mari | Switch-Dock |
| 21 | Shinji | Switch-Garden |
| 22 | Hikari | Switch-Comm |
| 23 | Kaworu | Switch-Relay |
| 24 | Asuka | Switch-Archive |
| 25 | Kensuke | Switch-Bunker |
| 26 | Ritsuko | Switch-Lab |
| 27 | Kaji | Switch-Intel |
| 28 | Misato | Switch-Cage |
| 29 | Shigeru | Switch-Monolith |
| 30 | Makoto | Switch-Radar |
| 31 | Toji | Switch-Dogma |
| 32 | Fuyutsuki | Switch-Terminal |
| 33 | Maya | Switch-Medic |

### Switch ke container, 25 kabel

Urutan penulisan nama node di switch menentukan urutan IP di soal 6.
Pasang sesuai urutan ini, kiri ke kanan seperti di gambar soal.

| No | Switch | Container |
|---|---|---|
| 34 | Switch-Cmd | Tokyo3-Core |
| 35 | Switch-Hangar | Hangar-Crew |
| 36 | Switch-Quarters | Quarters |
| 37 | Switch-Dock | Dock-Crew |
| 38 | Switch-Garden | Garden-Staff |
| 39 | Switch-Comm | Comm-Net |
| 40 | Switch-Relay | Relay-Node |
| 41 | Switch-Archive | Archive-Vault |
| 42 | Switch-Bunker | Sachiel |
| 43 | Switch-Lab | Entry-Plug |
| 44 | Switch-Lab | Dummy-Plug |
| 45 | Switch-Intel | Spy |
| 46 | Switch-Cage | Unit-01 |
| 47 | Switch-Cage | Unit-02 |
| 48 | Switch-Cage | Unit-00 |
| 49 | Switch-Monolith | Zeruel |
| 50 | Switch-Monolith | Armisael |
| 51 | Switch-Radar | Radar-Array |
| 52 | Switch-Dogma | Lilith |
| 53 | Switch-Dogma | Adam |
| 54 | Switch-Terminal | Magi-Casper |
| 55 | Switch-Terminal | Magi-Balthasar |
| 56 | Switch-Terminal | Magi-Melchior |
| 57 | Switch-Medic | Pen-Pen |
| 58 | Switch-Medic | Medic-Bay |

### NAT, 1 kabel

| No | Dari | Ke |
|---|---|---|
| 59 | Gendou | NAT1 |

## Image yang dipakai

Aturan: node yang harus menjalankan iptables atau apt-get memakai debinet.
alpinet berbasis Alpine, jadi apk bukan apt, dan iptables belum tentu lengkap.

**debinet (22 node):**
- 17 router: Gendou, Yui, Rei, Mari, Shinji, Hikari, Kaworu, Asuka, Kensuke,
  Ritsuko, Kaji, Misato, Shigeru, Makoto, Toji, Fuyutsuki, Maya
- Magi-Casper, apt soal 7 dan 12, iptables soal 11 dan 12
- Magi-Balthasar, apt soal 7, iptables soal 13
- Magi-Melchior, apt soal 7
- Pen-Pen, iptables dengan match time soal 14
- Tokyo3-Core, iptables soal 15 dan 16

**alpinet (20 node):**
Quarters, Comm-Net, Hangar-Crew, Sachiel, Medic-Bay, Relay-Node, Garden-Staff,
Dock-Crew, Entry-Plug, Dummy-Plug, Unit-01, Unit-02, Unit-00, Lilith, Adam,
Zeruel, Armisael, Radar-Array, Archive-Vault, Spy

Total 60 node: 22 debinet, 20 alpinet, 17 switch, 1 NAT1.

## Urutan mengerjakan

1. Pasang 17 router dulu, beri nama, atur adapter satu per satu
2. Pasang 17 switch, beri nama
3. Pasang 25 container plus NAT1, beri nama
4. Baru tarik kabel, urut nomor 1 sampai 59
5. Nyalakan semua node, pastikan tidak ada yang gagal start

Jangan campur memasang node dengan menarik kabel. Kalau dicampur, yang terlewat
baru ketahuan waktu routing, dan mencarinya jauh lebih lama.

## Catatan dari Modul 4

- alpinet memakai BusyBox, opsi ping ditulis sebelum target
- Hanya /root dan /etc/network/interfaces yang bertahan setelah node restart
- Editor "Edit network configuration" di GNS3 membuang baris yang mengandung tanda lebih besar
