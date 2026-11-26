# Modul 5 - Operation Geofront, K-68

Deadline: Jumat 27 November 2026, 13.00 WIB (diperpanjang dari Kamis 23.59).

Pembagian: Ryan = Babak 1 dan 2 (soal 1-12). Made = Babak 3 dan Final (soal 13-20).

Prefix kelompok `192.245`, akar VLSM `192.245.0.0/21`, tidak pernah berganti.
Controller Group C `http://10.4.89.250/`, hanya lewat jaringan atau VPN ITS.
Jangan pernah mencoba connect ke IP controller langsung dari sesi ini.

## Bagian Ryan

| No | Soal | Node | Status |
|---|---|---|---|
| 1 | VLSM 17 LAN + 16 link, tabel, pohon IP | perhitungan | selesai |
| 2 | Bangun topologi, IP interface router, ip_forward | semua router | selesai |
| 3 | Routing statis + summarization | semua router | selesai |
| 4 | NAT lewat Gendou, tahan IP NAT1 berganti, DNS | Gendou | selesai |
| 5 | Firewall Gendou, INPUT dan FORWARD DROP | Gendou | selesai |
| 6 | IP statis semua node non-router | semua host | selesai |
| 7 | Install paket + listener MAGI | Casper, Balthasar, Melchior | selesai |
| 8 | Listener Pen-Pen dan Tokyo3-Core | Pen-Pen, Tokyo3-Core | selesai |
| 9 | Sabotase break_routing.sh | Shigeru | selesai |
| 10 | Sabotase break_nat.sh | Misato + Gendou | selesai |
| 11 | Casper jadi hantu | Magi-Casper | selesai |
| 12 | Sabotase break_fw.sh | Magi-Casper | selesai |

## Bagian Made

| No | Soal | Node | Status |
|---|---|---|---|
| 13 | Brankas Balthasar | Magi-Balthasar | selesai |
| 14 | Jam kunjungan Pen-Pen | Pen-Pen | belum |
| 15 | Rate limit 5 ping per detik | Tokyo3-Core | belum |
| 16 | Log SCAN-NERV + reject port scan | Tokyo3-Core | belum |
| 17 | DNAT Casper ke Zeruel dibelokkan ke Pen-Pen | Fuyutsuki | belum |
| 18 | Gerbang zona Monolith dan Radar | Shigeru | belum |
| 19 | Isolasi Armisael | Shigeru | belum |
| 20 | Backup dan restore idempotent | 3 node pilihan | belum |

Saran urutan untuk Made: kerjakan 18 dan 19 lebih dulu, bukan 14 sampai 17.
Shigeru sudah bersih setelah perbaikan soal 9, jadi dia bisa langsung pakai.

## Node yang disentuh dua orang, hati-hati menimpa

| Node | Ryan | Made |
|---|---|---|
| Magi-Casper | soal 7, 11, 12 | soal 17 (sebagai sumber trafik) |
| Magi-Balthasar | soal 7 | soal 13 |
| Pen-Pen | soal 8 | soal 14 |
| Tokyo3-Core | soal 8 | soal 15, 16 |
| Shigeru | soal 3, 9 | soal 18, 19 |
| Fuyutsuki | soal 2, 3 | soal 17 |

Aturan: Ryan menulis bagian atas file /root/<nama>.sh, Made menyambung di bawahnya.
Jangan pernah menulis ulang file dari nol.

## Hasil diagnosis sabotase, jangan diulang dari nol

### Soal 9, break_routing.sh di Shigeru, empat penyebab

1. Default route dibajak dari `192.245.4.129` ke `192.245.4.162`
2. `blackhole 192.245.4.72/29`, yaitu subnet Lilith
3. `net.ipv4.ip_forward=0`
4. `ip rule pref 90: from all iif eth1 lookup 100` dengan `unreachable default table 100`

Penyebab 4 hampir terlewat karena snapshot awal hanya merekam `ip route` tabel main.
Gejala khasnya: Armisael masih bisa ping gateway sendiri tapi tidak bisa diteruskan,
karena tabel `local` di prioritas 0 dibaca lebih dulu daripada rule 90.

Perbaikan: jalankan ulang `/root/shigeru.sh`, lalu
`ip route del blackhole 192.245.4.72/29`, `ip rule del pref 90`,
`ip route flush table 100`.

### Soal 10, break_nat.sh di Misato dan Gendou, dua penyebab

1. Misato dapat tabel `*nat` baru berisi
   `-A POSTROUTING -s 192.245.4.64/29 ! -d 192.245.0.0/16 -o eth0 -j SNAT --to-source 172.31.99.1`
2. Gendou dapat `-A POSTROUTING -s 192.245.4.64/29 -o eth0 -j RETURN`
   disisipkan sebelum MASQUERADE

Counter rule RETURN di Gendou tetap nol karena SNAT di Misato menyala lebih dulu.
Kalau hanya Misato yang diperbaiki, rule Gendou justru baru aktif. Harus dua-duanya.

Perbaikan: `iptables -t nat -F POSTROUTING` di Misato, lalu
`iptables -t nat -D POSTROUTING -s 192.245.4.64/29 -o eth0 -j RETURN` di Gendou.

Jawaban pertanyaan soal: translasi alamat sumber terjadi di Gendou lewat MASQUERADE di eth0.

### Soal 11, Casper jadi hantu

Satu rule di `/root/magi-casper.sh`:

    iptables -A INPUT -p icmp --icmp-type echo-request -j DROP

Hanya `echo-request`, supaya `echo-reply` untuk ping Casper sendiri tetap masuk.
`DROP` bukan `REJECT`, supaya Casper benar-benar diam dan tidak ketahuan hidup.
Hanya `-p icmp`, supaya TCP 5000 tetap melayani.

Bukti TTL sekaligus memvalidasi topologi: ttl=64 ke Balthasar (satu switch),
ttl=62 ke Lilith (dua hop router, lewat Fuyutsuki lalu Toji).

### Soal 12, break_fw.sh di Magi-Casper, tiga penyebab

Link Drive: id `1Yb5kX1NKafOGDTbAE7WMYHZkQgpWLTvZ`, md5 `e69cb66046b8da903ba7624c001f6293`,
539 byte. Unduh lewat `drive.usercontent.google.com/download?id=...&export=download&confirm=t`,
lalu cocokkan `wc -c` dan `md5sum`.

Routing, `ip rule`, alamat, dan sysctl TIDAK disentuh. Listener tetap hidup di `ps`.
Murni tabel filter.

1. `:INPUT ACCEPT` diubah jadi `:INPUT DROP` tanpa satu pun rule ACCEPT pendamping
2. Rule soal 11 `--icmp-type 8 -j DROP` diganti jadi `-p icmp -j DROP`, jadi echo-reply
   untuk ping Casper sendiri ikut dibuang
3. Ditambah `-A OUTPUT -p udp --dport 53 -j DROP`, mematikan DNS keluar

Rule `-A INPUT -i lo -j ACCEPT` yang ikut ditambahkan justru benar dan dipertahankan.

Pembacaan gejala:
- Ping 8.8.8.8 gagal tanpa pesan error, karena yang rusak jalan pulangnya bukan jalan
  keluarnya. Beda dengan soal 9 yang memberi `Network is unreachable`.
- `apt-get` berhenti di `Temporary failure resolving`, bukan `Failed to connect`,
  artinya gagal sebelum ada koneksi TCP, cocok dengan rule DROP di UDP 53.
- nc dari Unit-00 tidak masuk log padahal listener hidup, karena SYN jatuh ke policy DROP.

Jebakan: chain INPUT tidak punya rule `ESTABLISHED,RELATED` sama sekali. Menghapus tiga
rule sabotase saja tidak cukup, `apt-get` akan tetap gagal dengan pesan berubah jadi
`Failed to connect`. Rule conntrack itu memang yang hilang dari script hardening.

Perbaikan: tulis ulang `/root/magi-casper.sh` dengan urutan buka sementara, apt, listener,
lalu hardening. Rule conntrack WAJIB di atas rule ICMP supaya echo-reply lolos sebagai
ESTABLISHED sedangkan echo-request dari luar tetap NEW dan dibuang. Itu yang membuat
soal 11 dan soal 12 berlaku bersamaan.

Temuan sampingan: ada dua loop listener berjalan bersamaan karena
`pkill -f 'nc -l -p 5000'` hanya membunuh proses `nc`, bukan loop `while` pembungkusnya.
Diperbaiki dengan bentuk `nohup bash -c 'while true; do nc -l -p 5000 ...'`, yang command
line-nya memuat string itu sehingga ikut terbunuh `pkill -f` pada run berikutnya.

## Resep snapshot sebelum menjalankan script sabotase

Dipakai sejak soal 10 setelah penyebab keempat soal 9 hampir terlewat.

    mkdir -p /root/snap
    ip route show table all > /root/snap/route-sebelum.txt
    ip rule > /root/snap/rule-sebelum.txt
    ip -br a > /root/snap/addr-sebelum.txt
    iptables-save > /root/snap/iptables-sebelum.txt
    sysctl -a 2>/dev/null | grep '^net.ipv4' | sort > /root/snap/sysctl-sebelum.txt
    ps -ef > /root/snap/ps-sebelum.txt

Yang penting dan sering terlewat: `ip rule`, `table all`, dan `ps -ef`.
`ps -ef` dipakai untuk membedakan layanan dimatikan versus hanya diblokir firewall.

## Sisa pekerjaan

- [ ] Selesaikan soal 12
- [ ] Tambah bagian Babak 2 (soal 9-12) ke README.md, sekarang baru sampai soal 8
- [ ] Pasang 22 hook `up bash` dari hook-autostart.md
- [ ] Ubah CHECKLIST.md jadi CATATAN-PENGERJAAN.md mengikuti format Modul 2 dan 3
- [ ] Ekspor project GNS3 ke K-68_Modul5.gns3project
- [ ] Commit dan push ke repo GitHub
- [ ] Hapus folder config/ yang kosong
- [ ] Booking slot demo Jumat
