# Peta Interface, Modul 5

Aturan: eth0 tiap router SELALU menghadap ke atas, ke arah Gendou.
Gendou sendiri eth0 menghadap NAT1.

Ini bukan sekadar kerapian. Soal 18 minta Shigeru menolak berdasarkan arah
datangnya paket, yaitu sisi yang menghadap Gendou, dan melarang menulis subnet
luar satu per satu. Dengan aturan ini Made cukup menulis -i eth0 di Shigeru.

## Tahap 1: kabel antar router dan NAT1, 17 kabel

| No | Node A | Interface A | Node B | Interface B |
|---|---|---|---|---|
| 1 | Gendou | eth0 | NAT1 | - |
| 2 | Gendou | eth2 | Yui | eth0 |
| 3 | Gendou | eth3 | Hikari | eth0 |
| 4 | Gendou | eth4 | Asuka | eth0 |
| 5 | Gendou | eth5 | Kaji | eth0 |
| 6 | Gendou | eth6 | Misato | eth0 |
| 7 | Gendou | eth7 | Shigeru | eth0 |
| 8 | Gendou | eth8 | Fuyutsuki | eth0 |
| 9 | Yui | eth2 | Rei | eth0 |
| 10 | Yui | eth3 | Mari | eth0 |
| 11 | Yui | eth4 | Shinji | eth0 |
| 12 | Hikari | eth2 | Kaworu | eth0 |
| 13 | Asuka | eth2 | Kensuke | eth0 |
| 14 | Asuka | eth3 | Ritsuko | eth0 |
| 15 | Shigeru | eth2 | Makoto | eth0 |
| 16 | Fuyutsuki | eth2 | Maya | eth0 |
| 17 | Fuyutsuki | eth3 | Toji | eth0 |

Catatan: Gendou eth1 sengaja dilewati, itu jatah Switch-Cmd di tahap 2.

## Tahap 2: router ke switch, 17 kabel

| No | Router | Interface | Switch |
|---|---|---|---|
| 18 | Gendou | eth1 | Switch-Cmd |
| 19 | Yui | eth1 | Switch-Hangar |
| 20 | Rei | eth1 | Switch-Quarters |
| 21 | Mari | eth1 | Switch-Dock |
| 22 | Shinji | eth1 | Switch-Garden |
| 23 | Hikari | eth1 | Switch-Comm |
| 24 | Kaworu | eth1 | Switch-Relay |
| 25 | Asuka | eth1 | Switch-Archive |
| 26 | Kensuke | eth1 | Switch-Bunker |
| 27 | Ritsuko | eth1 | Switch-Lab |
| 28 | Kaji | eth1 | Switch-Intel |
| 29 | Misato | eth1 | Switch-Cage |
| 30 | Shigeru | eth1 | Switch-Monolith |
| 31 | Makoto | eth1 | Switch-Radar |
| 32 | Toji | eth1 | Switch-Dogma |
| 33 | Fuyutsuki | eth1 | Switch-Terminal |
| 34 | Maya | eth1 | Switch-Medic |

## Tahap 3: switch ke container, 25 kabel

Urutan colok menentukan urutan IP di soal 6, jadi ikuti urutan ini persis.
Untuk switch berisi lebih dari satu node, urutannya kiri ke kanan sesuai gambar soal.

| No | Switch | Container |
|---|---|---|
| 35 | Switch-Cmd | Tokyo3-Core |
| 36 | Switch-Hangar | Hangar-Crew |
| 37 | Switch-Quarters | Quarters |
| 38 | Switch-Dock | Dock-Crew |
| 39 | Switch-Garden | Garden-Staff |
| 40 | Switch-Comm | Comm-Net |
| 41 | Switch-Relay | Relay-Node |
| 42 | Switch-Archive | Archive-Vault |
| 43 | Switch-Bunker | Sachiel |
| 44 | Switch-Lab | Entry-Plug |
| 45 | Switch-Lab | Dummy-Plug |
| 46 | Switch-Intel | Spy |
| 47 | Switch-Cage | Unit-01 |
| 48 | Switch-Cage | Unit-02 |
| 49 | Switch-Cage | Unit-00 |
| 50 | Switch-Monolith | Zeruel |
| 51 | Switch-Monolith | Armisael |
| 52 | Switch-Radar | Radar-Array |
| 53 | Switch-Dogma | Lilith |
| 54 | Switch-Dogma | Adam |
| 55 | Switch-Terminal | Magi-Casper |
| 56 | Switch-Terminal | Magi-Balthasar |
| 57 | Switch-Terminal | Magi-Melchior |
| 58 | Switch-Medic | Medic-Bay |
| 59 | Switch-Medic | Pen-Pen |

Catatan Switch-Medic: urutan ikut gambar, jadi Medic-Bay dulu baru Pen-Pen.
Tabel host di soal menomori sebaliknya. Glosarium bilang ikut gambar, jadi
gambar yang dipakai. Tulis asumsi ini di laporan.

## Cara memeriksa setelah selesai

Jumlah adapter tiap router sudah pas dengan jumlah kabelnya, tidak ada yang lebih.
Jadi setelah 59 kabel terpasang, TIDAK BOLEH ada satu pun router yang masih
punya interface kosong.

Kalau ada yang kosong, berarti ada kabel terlewat di router itu.

| Router | Interface terpakai |
|---|---|
| Gendou | eth0 sampai eth8, 9 |
| Yui | eth0 sampai eth4, 5 |
| Asuka | eth0 sampai eth3, 4 |
| Fuyutsuki | eth0 sampai eth3, 4 |
| Hikari | eth0 sampai eth2, 3 |
| Shigeru | eth0 sampai eth2, 3 |
| 11 router sisanya | eth0 dan eth1, 2 |
