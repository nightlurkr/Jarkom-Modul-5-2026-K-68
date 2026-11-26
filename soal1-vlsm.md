# Soal 1: Pembagian IP VLSM

Kelompok K-68. Prefix kelompok 192.245.0.0/16.

## Aturan yang dipakai

1. Kebutuhan IP satu subnet = jumlah host + 1 untuk gateway.
   Dasarnya glosarium, "gateway adalah IP usable pertama subnet", jadi gateway
   memakai satu alamat usable. Bukti bahwa ini bacaan yang benar: tanpa menghitung
   gateway, Switch-Dogma dan Switch-Monolith yang berisi 2 host akan jatuh ke /30
   yang hanya punya 2 alamat usable, sehingga tidak tersisa alamat untuk gateway.
2. Prefix tiap subnet adalah yang terkecil yang masih muat, sesuai perintah soal
   "tiap subnet memakai prefix terkecil yang muat".
3. Urutan alokasi dari kebutuhan terbesar ke terkecil.
   Tie-break sesuai soal: ukuran sama maka LAN dulu daripada link, lalu urutan tabel.
   Urutan 16 link mengikuti urutan pohon dari Gendou ke bawah, karena soal tidak
   menomori link.
4. Alamat awal tiap blok wajib kelipatan ukuran bloknya.

## Penentuan prefix akar

Jumlah kapasitas yang dialokasikan, bukan yang dibutuhkan:

    /23  x1   = 512
    /24  x1   = 256
    /25  x1   = 128
    /26  x1   = 64
    /27  x3   = 96
    /28  x1   = 16
    /29  x6   = 48
    /30  x19  = 76      (3 LAN + 16 link)
                ----
                1196

Pangkat dua terkecil yang menampung 1196 adalah 2048, jadi akarnya /21.

Blok yang dipakai: 192.245.0.0/21, yaitu 192.245.0.0 sampai 192.245.7.255.
Terpakai sampai 192.245.4.171, sisa 852 alamat di ekor blok.

## Contoh hitungan satu subnet (diminta soal)

Switch-Bunker, berisi container Sachiel yang mewakili 55 host.

1. Kebutuhan = 55 host + 1 gateway = 56 alamat usable
2. Cari prefix terkecil yang muat.
   /27 memberi 32 alamat, usable 30. Tidak cukup.
   /26 memberi 64 alamat, usable 62. Cukup. Jadi dipakai /26.
3. Giliran alokasi. Switch-Bunker adalah blok terbesar keempat, setelah
   Quarters /23, Comm /24, dan Hangar /25. Alamat berikutnya yang bebas
   adalah 192.245.3.128.
4. Cek alignment. /26 berisi 64 alamat, jadi alamat awal harus kelipatan 64.
   Oktet keempat 128 adalah kelipatan 64, jadi sah.
5. Hasilnya 192.245.3.128/26
   Netmask    255.255.255.192
   Gateway    192.245.3.129  (usable pertama, milik Kensuke)
   Sachiel    192.245.3.130  (usable berikutnya)
   Broadcast  192.245.3.191
   Terpakai 56, sisa 6

## Jalur Dummy-Plug ke Armisael

Soal meminta subnet di jalur ini dilingkari pada gambar pohon.
Jalurnya melewati 5 subnet:

    Dummy-Plug -> Switch-Lab -> Ritsuko -> Asuka -> Gendou -> Shigeru -> Switch-Monolith -> Armisael

| Subnet | Network |
|---|---|
| Switch-Lab | 192.245.4.48/29 |
| Asuka-Ritsuko | 192.245.4.156/30 |
| Gendou-Asuka | 192.245.4.116/30 |
| Gendou-Shigeru | 192.245.4.128/30 |
| Switch-Monolith | 192.245.4.80/29 |

## Tabel lengkap

| No | Subnet | Isi | Butuh | Network | Prefix | Netmask | Gateway | Broadcast | Terpakai | Sisa |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Switch-Quarters | Quarters 500 | 501 | 192.245.0.0 | /23 | 255.255.254.0 | 192.245.0.1 | 192.245.1.255 | 501 | 9 |
| 2 | Switch-Comm | Comm-Net 200 | 201 | 192.245.2.0 | /24 | 255.255.255.0 | 192.245.2.1 | 192.245.2.255 | 201 | 53 |
| 3 | Switch-Hangar | Hangar-Crew 100 | 101 | 192.245.3.0 | /25 | 255.255.255.128 | 192.245.3.1 | 192.245.3.127 | 101 | 25 |
| 4 | Switch-Bunker | Sachiel 55 | 56 | 192.245.3.128 | /26 | 255.255.255.192 | 192.245.3.129 | 192.245.3.191 | 56 | 6 |
| 5 | Switch-Medic | Pen-Pen 1, Medic-Bay 25 | 27 | 192.245.3.192 | /27 | 255.255.255.224 | 192.245.3.193 | 192.245.3.223 | 27 | 3 |
| 6 | Switch-Relay | Relay-Node 25 | 26 | 192.245.3.224 | /27 | 255.255.255.224 | 192.245.3.225 | 192.245.3.255 | 26 | 4 |
| 7 | Switch-Garden | Garden-Staff 14 | 15 | 192.245.4.0 | /27 | 255.255.255.224 | 192.245.4.1 | 192.245.4.31 | 15 | 15 |
| 8 | Switch-Dock | Dock-Crew 12 | 13 | 192.245.4.32 | /28 | 255.255.255.240 | 192.245.4.33 | 192.245.4.47 | 13 | 1 |
| 9 | Switch-Lab | Entry-Plug 3, Dummy-Plug 2 | 6 | 192.245.4.48 | /29 | 255.255.255.248 | 192.245.4.49 | 192.245.4.55 | 6 | 0 |
| 10 | Switch-Terminal | Magi-Casper 1, Magi-Balthasar 1, Magi-Melchior 1 | 4 | 192.245.4.56 | /29 | 255.255.255.248 | 192.245.4.57 | 192.245.4.63 | 4 | 2 |
| 11 | Switch-Cage | Unit-01 1, Unit-02 1, Unit-00 1 | 4 | 192.245.4.64 | /29 | 255.255.255.248 | 192.245.4.65 | 192.245.4.71 | 4 | 2 |
| 12 | Switch-Dogma | Lilith 1, Adam 1 | 3 | 192.245.4.72 | /29 | 255.255.255.248 | 192.245.4.73 | 192.245.4.79 | 3 | 3 |
| 13 | Switch-Monolith | Zeruel 1, Armisael 1 | 3 | 192.245.4.80 | /29 | 255.255.255.248 | 192.245.4.81 | 192.245.4.87 | 3 | 3 |
| 14 | Switch-Radar | Radar-Array 4 | 5 | 192.245.4.88 | /29 | 255.255.255.248 | 192.245.4.89 | 192.245.4.95 | 5 | 1 |
| 15 | Switch-Archive | Archive-Vault 1 | 2 | 192.245.4.96 | /30 | 255.255.255.252 | 192.245.4.97 | 192.245.4.99 | 2 | 0 |
| 16 | Switch-Intel | Spy 1 | 2 | 192.245.4.100 | /30 | 255.255.255.252 | 192.245.4.101 | 192.245.4.103 | 2 | 0 |
| 17 | Switch-Cmd | Tokyo3-Core 1 | 2 | 192.245.4.104 | /30 | 255.255.255.252 | 192.245.4.105 | 192.245.4.107 | 2 | 0 |
| 18 | Gendou-Yui | 2 interface router | 2 | 192.245.4.108 | /30 | 255.255.255.252 | 192.245.4.109 | 192.245.4.111 | 2 | 0 |
| 19 | Gendou-Hikari | 2 interface router | 2 | 192.245.4.112 | /30 | 255.255.255.252 | 192.245.4.113 | 192.245.4.115 | 2 | 0 |
| 20 | Gendou-Asuka | 2 interface router | 2 | 192.245.4.116 | /30 | 255.255.255.252 | 192.245.4.117 | 192.245.4.119 | 2 | 0 |
| 21 | Gendou-Kaji | 2 interface router | 2 | 192.245.4.120 | /30 | 255.255.255.252 | 192.245.4.121 | 192.245.4.123 | 2 | 0 |
| 22 | Gendou-Misato | 2 interface router | 2 | 192.245.4.124 | /30 | 255.255.255.252 | 192.245.4.125 | 192.245.4.127 | 2 | 0 |
| 23 | Gendou-Shigeru | 2 interface router | 2 | 192.245.4.128 | /30 | 255.255.255.252 | 192.245.4.129 | 192.245.4.131 | 2 | 0 |
| 24 | Gendou-Fuyutsuki | 2 interface router | 2 | 192.245.4.132 | /30 | 255.255.255.252 | 192.245.4.133 | 192.245.4.135 | 2 | 0 |
| 25 | Yui-Rei | 2 interface router | 2 | 192.245.4.136 | /30 | 255.255.255.252 | 192.245.4.137 | 192.245.4.139 | 2 | 0 |
| 26 | Yui-Mari | 2 interface router | 2 | 192.245.4.140 | /30 | 255.255.255.252 | 192.245.4.141 | 192.245.4.143 | 2 | 0 |
| 27 | Yui-Shinji | 2 interface router | 2 | 192.245.4.144 | /30 | 255.255.255.252 | 192.245.4.145 | 192.245.4.147 | 2 | 0 |
| 28 | Hikari-Kaworu | 2 interface router | 2 | 192.245.4.148 | /30 | 255.255.255.252 | 192.245.4.149 | 192.245.4.151 | 2 | 0 |
| 29 | Asuka-Kensuke | 2 interface router | 2 | 192.245.4.152 | /30 | 255.255.255.252 | 192.245.4.153 | 192.245.4.155 | 2 | 0 |
| 30 | Asuka-Ritsuko | 2 interface router | 2 | 192.245.4.156 | /30 | 255.255.255.252 | 192.245.4.157 | 192.245.4.159 | 2 | 0 |
| 31 | Shigeru-Makoto | 2 interface router | 2 | 192.245.4.160 | /30 | 255.255.255.252 | 192.245.4.161 | 192.245.4.163 | 2 | 0 |
| 32 | Fuyutsuki-Maya | 2 interface router | 2 | 192.245.4.164 | /30 | 255.255.255.252 | 192.245.4.165 | 192.245.4.167 | 2 | 0 |
| 33 | Fuyutsuki-Toji | 2 interface router | 2 | 192.245.4.168 | /30 | 255.255.255.252 | 192.245.4.169 | 192.245.4.171 | 2 | 0 |
