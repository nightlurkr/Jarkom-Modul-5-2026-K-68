# Catatan yang dibawa dari Modul 1 sampai 4

Dirangkum dari CATATAN-PENGERJAAN Modul 2 dan 3, serta README Modul 4.
Dibaca sebelum mulai, supaya pelajaran lama tidak diulang dari nol.

## Persistensi node

- Hanya `/root` dan `/etc/network/interfaces` yang BERTAHAN saat node restart.
  Isi `/etc` lain dan paket yang di-install HILANG.
- Semua config ditulis lewat script di `/root`, jangan ketik langsung ke `/etc`.
- Script yang butuh paket WAJIB memuat `apt-get install` di dalamnya,
  karena paket ikut hilang saat restart.

## Autostart setelah restart

Pola yang sudah terbukti sejak Modul 2 (13 node debinet) dan dipakai lagi di Modul 3:

    up bash /root/<nama>.sh

ditulis di dalam blok interface pada `/etc/network/interfaces`.
Node harus MATI saat mengedit lewat menu GNS3. Kalau lewat console
dengan `cat`, node boleh menyala.

## Jebakan GNS3

- Editor "Edit network configuration" MEMBUANG baris yang mengandung `>`.
  Sembunyikan perintah redirect di dalam file `.sh`, panggil lewat `up bash`.
- Jangan ubah template Docker lewat Edit > Preferences. Controller dipakai
  bareng semua kelompok. Ubah adapter PER NODE lewat klik kanan > Configure > Network.
- Link bisa nyangkut kalau node di-restart setelah kabel terpasang.
  Gejalanya konfigurasi benar tapi traffic tidak lewat, dan ARP satu arah saja.
  Perbaikan: restart node, atau hapus dan pasang ulang kabelnya.

## iptables

- Pakai pola `iptables -C ... || iptables -A ...`, jangan `-A` polos.
  Pelajaran Modul 2: `-A` polos bikin rule menumpuk satu per restart.
- MASQUERADE dipilih, bukan SNAT, kalau alamat luar datang dari DHCP.
- Tabel `nat` hanya dilewati paket PERTAMA tiap koneksi. Paket berikutnya
  ditangani conntrack. Jadi `ping -c2` menambah counter 1, bukan 2.

## Perbedaan image

| | debinet | alpinet |
|---|---|---|
| Paket | `apt-get` | `apk add` |
| DHCP client | `dhclient` | `udhcpc -i eth0` |
| ifup / ifdown | TIDAK ADA | ada, dari busybox |
| ping | opsi bebas | opsi WAJIB sebelum target |

Untuk debinet, terapkan alamat dengan `ip addr replace` dan `ip route replace`,
bukan `ifup`.

## Membaca hasil uji

- TTL 64 berarti satu subnet, tidak lewat router.
  TTL 63 berarti lewat tepat satu router. Tiap router mengurangi satu.
- `Network is unreachable` berarti router tidak punya rute.
  `Destination Host Unreachable` berarti rute ada, tujuannya yang tidak menjawab ARP.
  Timeout tanpa pesan biasanya firewall membuang diam-diam.
- Alamat pengirim pesan error menunjukkan sampai mana paket berhasil menempuh jalur.
- MAC container berpola `02:42:xx:xx:xx:00`. Jangan diturunkan dari alamat IPv6.

## Kebiasaan kerja

- Satu file catatan pengerjaan per modul, formatnya seperti
  `Modul 2/CATATAN-PENGERJAAN.md` dan `Modul 3/CATATAN-PENGERJAAN.md`.
- Screenshot diberi nama `soalXX-<isi>.png` dan isinya HANYA pengerjaan.
  Gambar topologi dari asisten tidak masuk folder screenshot.
- Script shell tanpa komentar penjelasan. Penanda satu baris boleh.
  Penjelasan panjang masuk README.
- Prefix IP kelompok K-68 selalu `192.245`, tidak pernah berganti.
