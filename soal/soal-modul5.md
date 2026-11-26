# Soal Praktikum Modul 5: Operation Geofront

Komunikasi Data dan Jaringan Komputer 2026. Disalin dari soal asisten untuk arsip K-68.

## Peraturan praktikum

- Dilaksanakan Senin 23 November 2026 sampai Kamis 26 November 2026 pukul 23.59 WIB. Diperpanjang ke Jumat 27 November 2026 pukul 13.00.
- Saat selesai: ekspor file GNS3 project, gambar pohon pembagian IP, dan template spreadsheet.
- Wajib image docker `ardhptr21/alpinet:latest` atau `ardhptr21/debinet:latest`.
- Wajib GNS3 Web, Client, atau VM. Tidak boleh tools lain.
- Script instalasi dan konfigurasi harus diletakkan di `/root`.
- Tidak boleh bekerja sama atau bertanya pengerjaan ke luar anggota kelompok.
- Tidak boleh bertanya konfigurasi ke asisten. Boleh bertanya cara atau letak kesalahan.
- Boleh memakai AI selama tidak mengkoneksikan agent langsung ke controller.
- Demo ke asisten penguji paling lambat Kamis 26 November 2026.
- Laporan resmi berupa link repository GitHub.
- Saran asisten: backup konfigurasi di luar project GNS3.

IP host controller, wajib jaringan atau VPN ITS:

| Group | Alamat |
|---|---|
| A | 10.4.89.246 |
| B | 10.4.89.247 |
| C | 10.4.89.250 |

## Cerita

Tokyo-3, 2015. SEELE menyusup ke jaringan NERV lewat Dummy System dan menanam pengkhianat
Armisael di Switch-Monolith, di sebelah Zeruel yang masih setia. Kalau Instrumentality
dimulai, MAGI akan membuka seluruh data Geofront. Gendo hanya mengizinkan satu pintu
keluar, yaitu NAT1, dan bersama 16 router di bawahnya (total 17 router) masing-masing
menjaga satu LAN. Misi: bangun Geofront, amankan MAGI, dan isolasi Armisael sebelum
hitung mundur habis, tanpa menyeret Zeruel.

Topologi: `topo_5.png`

## Glosarium

- Satu container mewakili satu grup host. Jumlah host hanya dipakai untuk hitungan VLSM. Semua node pada satu switch berada di satu subnet.
- Alamat node: gateway adalah IP usable pertama subnet. Node di switch diberi IP usable berikutnya sesuai urutan penulisan nama node pada switch di gambar, kiri ke kanan.
- Seele adalah alamat internet 8.8.8.8. Dipakai untuk uji ping, dan untuk uji nc memakai TCP port 53.
- Sebagian node memakai nc versi busybox yang tidak mencetak pesan error. Gunakan exit code, durasi koneksi, dan log penerima sebagai bukti.

## Peran router

| Router | Peran |
|---|---|
| Gendou | Router pusat dan satu-satunya yang terhubung ke NAT1 |
| Fuyutsuki | Gateway Switch-Terminal |
| Shigeru | Gateway Switch-Monolith |
| Asuka | Gateway Switch-Archive dan induk dua cabang di bawahnya |
| Kaworu | Gateway Switch-Relay, ujung cabang di bawah Hikari |
| Shinji | Gateway Switch-Garden |
| Rei | Gateway Switch-Quarters |

## Kategori node

| Kategori | Anggota |
|---|---|
| Pilot | Unit-01, Unit-02, Unit-00, Sachiel |
| Staf Geofront | Lilith, Adam |
| Terduga Dummy System | Dummy-Plug, Entry-Plug, Spy |
| Perangkat MAGI | Magi-Casper, Magi-Balthasar, Magi-Melchior |
| Lainnya | Semua node di luar kategori di atas. Tidak dibatasi di soal 14. |
| Zona sensitif | Switch-Monolith dan Switch-Radar, dijaga Shigeru |

## Nilai tetap

| Nilai | Isi | Keterangan |
|---|---|---|
| T1 | 08.00 WIB | Awal jam pilot |
| T2 | 16.00 WIB | Akhir jam pilot |
| Jam Staf Geofront | 17.00 sampai 23.00 WIB | Jam kunjungan staf |
| L | 5 ping per detik | Batas ping |

## Layanan listener nc

| Node | Port TCP |
|---|---|
| Magi-Casper | 5000 |
| Magi-Balthasar | 5000 dan 5001 |
| Magi-Melchior | 5000 |
| Pen-Pen | 80 |
| Tokyo3-Core | 80 dan 8080 |

## Tabel Host

Urutan tie-break VLSM: ukuran sama, LAN dulu daripada link, lalu urutan tabel.

| No | Switch | Router gateway | Container dan jumlah host |
|---|---|---|---|
| 1 | Switch-Quarters | Rei | Quarters = 500 |
| 2 | Switch-Comm | Hikari | Comm-Net = 200 |
| 3 | Switch-Hangar | Yui | Hangar-Crew = 100 |
| 4 | Switch-Bunker | Kensuke | Sachiel = 55 |
| 5 | Switch-Medic | Maya | 1. Pen-Pen = 1, 2. Medic-Bay = 25 |
| 6 | Switch-Relay | Kaworu | Relay-Node = 25 |
| 7 | Switch-Garden | Shinji | Garden-Staff = 14 |
| 8 | Switch-Dock | Mari | Dock-Crew = 12 |
| 9 | Switch-Lab | Ritsuko | 1. Entry-Plug = 3, 2. Dummy-Plug = 2 |
| 10 | Switch-Terminal | Fuyutsuki | 1. Magi-Casper = 1, 2. Magi-Balthasar = 1, 3. Magi-Melchior = 1 |
| 11 | Switch-Cage | Misato | 1. Unit-01 = 1, 2. Unit-02 = 1, 3. Unit-00 = 1 |
| 12 | Switch-Dogma | Toji | 1. Lilith = 1, 2. Adam = 1 |
| 13 | Switch-Monolith | Shigeru | 1. Zeruel = 1, 2. Armisael = 1 |
| 14 | Switch-Radar | Makoto | Radar-Array = 4 |
| 15 | Switch-Archive | Asuka | Archive-Vault = 1 |
| 16 | Switch-Intel | Kaji | Spy = 1 |
| 17 | Switch-Cmd | Gendo | Tokyo3-Core = 1 |

---

# Babak 1: Membangun Geofront

## Soal 1. Markas tanpa peta

Tengah malam alarm meraung, dan Ritsuko terdiam. Markas sebesar ini dibangun tanpa peta
alamat. Gendo anti pemborosan, jadi tiap subnet memakai prefix terkecil yang muat. Bagi
prefix IP kelompokmu dengan VLSM untuk 17 LAN dan 16 link antar-router, urut dari
kebutuhan terbesar. Tunjukkan tabel berisi network, prefix, gateway, broadcast, IP
terpakai, dan sisa. Tunjukkan juga gambar pohon IP, dengan subnet jalur Dummy-Plug ke
Armisael dilingkari. Di laporan, jelaskan hitungan satu subnet.

## Soal 2. Geofront gelap gulita

Kabel sudah ditarik, tapi saat Misato menekan tombol daya, Geofront tetap gelap. Tidak
ada alamat dan tidak ada rute. Pasang semua node di GNS3 sesuai topologi. Beri IP ke tiap
interface router sesuai soal 1, lalu aktifkan IP forwarding di semua router. Hanya Gendo
yang terhubung ke NAT1 dan mendapat alamat lewat DHCP. Router lain baru bisa memasang
paket setelah soal 4, karena internet baru ada saat itu. Tunjukkan `ip -br` di tiap router
dan ping dua ujung tiap link router ke router.

## Soal 3. Pulau-pulau yang terpisah

Lampu mulai menyala, tapi tiap segmen masih seperti pulau terpisah. Fuyutsuki
mengingatkan: bangun jalur dari Gendo ke bawah, cabang demi cabang. Hubungkan semua subnet
dengan routing statis dan jumlah rute sesedikit mungkin. Router selain Gendo cukup punya
satu default route ke atas dan rute untuk subnet di bawahnya. Gendo memakai default route
ke NAT1. Subnet satu cabang yang bisa digabung tepat menjadi satu CIDR harus diringkas.
Untuk soal 3 sampai 5, beri IP dan gateway sementara pada node uji. IP permanen dibuat di
soal 6. Buktikan Armisael bisa ping ke Lilith, Sachiel, dan Comm-Net.

## Soal 4. Haus kabar dari dunia luar

Maya melapor bahwa semua sistem butuh update dari luar, padahal pintu keluarnya hanya
Gendo. IP dari NAT1 juga bisa berganti kapan saja. Beri seluruh jaringan akses internet
lewat Gendo. Konfigurasinya tidak boleh perlu diubah saat IP NAT1 berganti. Hanya traffic
dari prefix internal yang keluar lewat NAT1 yang boleh diterjemahkan alamatnya, sedangkan
traffic antar-LAN harus tetap membawa alamat asli pengirimnya. Node yang nanti memasang
paket juga harus bisa me-resolve nama domain. Selesaikan sebelum node lain memasang paket.
Buktikan ping 8.8.8.8 dan ping deb.debian.org dari Armisael dan Lilith berhasil, dan
tunjukkan counter rule NAT.

## Soal 5. Pintu depan dikunci rapat

Kaji memperingatkan Kaworu bahwa SEELE cukup mengetuk pintu depan. Gendo mengunci NAT1:
boleh keluar, tidak boleh dimasuki. Di Gendo, ubah policy INPUT dan FORWARD menjadi DROP.
Lalu izinkan empat hal: (a) balasan koneksi yang sudah ada, (b) traffic dari jaringan
internal ke internet, (c) traffic antar-LAN, dan (d) traffic dari jaringan internal ke
Gendo sendiri. Loopback dan DHCP dari NAT1 juga harus tetap jalan. Koneksi baru dari sisi
NAT1 tidak boleh masuk. Soal 2 sampai 4 harus tetap jalan, dan semuanya ditulis di
`gendo.sh`. Tunjukkan `iptables -S` di Gendo, dan buktikan ping 8.8.8.8 dari Armisael
masih berhasil.

## Soal 6. Alamat para pilot

Jaringan sudah hidup, tapi para pilot dan staf tidak tahu alamat mereka sendiri. Unit-01
bahkan tidak tahu siapa gateway-nya. Beri IP statis dan gateway ke semua node non-router,
sesuai aturan alamat di glosarium. Simpan agar bertahan setelah restart. Tunjukkan
`ip -br` dan ping ke gateway di Unit-01.

## Soal 7. Membangunkan MAGI

Casper, Balthasar, dan Melchior di Switch-Terminal menunggu diaktifkan. Ritsuko ingin
ketiganya melayani dulu, urusan keamanan dibahas nanti. Pasang iptables dan
netcat-traditional lewat `/root/<nama>.sh`, lalu nyalakan listener sesuai port di
glosarium. Dari Lilith, kirim pesan ke ketiganya dan ke port 5001 Balthasar. Tampilkan
pesan yang diterima di tiap node.

## Soal 8. Mengenali kondisi normal

Pen-Pen dan Tokyo3-Core sudah ditandai sebagai calon sasaran pertama SEELE. Sebelum
dilindungi, keduanya harus melayani dulu agar kamu tahu kondisi normalnya. Koneksi ke port
80 dijawab `Welcome to {hostname}`. Koneksi ke port 8080 di Tokyo3-Core dijawab
`syifa imutz`. Tunjukkan nc dari Lilith ke ketiga port itu. Tunjukkan juga bahwa port yang
tidak dilayani, misalnya 22, langsung gagal, bukan menggantung.

---

# Babak 2: Sabotase dan Hantu MAGI

Geofront berjalan, tapi tangan tak dikenal mulai merusaknya. Pada soal sabotase, yaitu
soal 9, 10, dan 12, jalankan script rusak dari asisten **tanpa membaca isinya**. Temukan
semua penyebab, karena bisa lebih dari satu. Perbaiki, lalu jelaskan kenapa gejalanya
begitu.

## Soal 9. Jalur belakang diutak-atik

Tengah malam masuk laporan bahwa ada yang mengutak-atik jalur belakang di sekitar Shigeru.
Lilith tidak mendapat jawaban dari Armisael, padahal Armisael masih bisa ping gateway-nya
sendiri. Jalankan `break_routing.sh` di Shigeru. Buktikan Lilith bisa ping Armisael
kembali setelah perbaikan.

## Soal 10. Para pilot terkunci di dalam

Paginya giliran para pilot mengeluh. Unit-01, Unit-02, dan Unit-00 di Cage tidak bisa ping
8.8.8.8. Padahal Spy di Intel baik-baik saja, dan ping dari Cage ke Lilith normal. Jalankan
`break_nat.sh` di Misato dan di Gendo. Filenya sama, dijalankan di masing-masing node.
Jelaskan kenapa Cage terdampak tapi Intel tidak. Buktikan Unit-01 bisa ping 8.8.8.8, dan
sebutkan di router mana alamat sumber diterjemahkan.

## Soal 11. Casper harus jadi hantu

Casper adalah MAGI paling rahasia. Kalau SEELE tahu ia masih hidup, mereka tahu ke mana
harus menyerang. Tidak ada perangkat lain yang boleh mem-ping Casper, termasuk yang satu
switch. Casper sendiri tetap boleh mem-ping siapa saja. Layanan TCP 5000 Casper tetap
hidup. Buktikan ping ke Casper dari Balthasar dan Lilith gagal, sedangkan ping dari Casper
ke keduanya berhasil.

## Soal 12. Teknisi yang kelewat semangat

Seorang teknisi menjalankan script hardening di Casper lalu pulang tanpa mengecek. Paginya
Casper tidak bisa ping 8.8.8.8 dan tidak bisa `apt-get update`. Unit-00 juga tidak bisa
menyambung ke TCP 5000 Casper. Ping ke Casper tetap ditolak, dan itu memang benar.
Jalankan `break_fw.sh` di Magi-Casper, lalu perbaiki **tanpa membatalkan tujuan
hardening**. Casper tetap menolak semua lalu lintas masuk yang tidak diizinkan, dan policy
INPUT tetap DROP. Buktikan nc dari Unit-00 ke Casper port 5000 berhasil dan `apt-get
update` di Casper berhasil.

---

# Babak 3: Mengamankan MAGI dan Zona Sensitif

## Soal 13. Brankas Balthasar

Balthasar menyimpan seluruh data Geofront, tapi terlalu banyak yang bisa mengetuk
pintunya, termasuk tetangga satu switch. Hanya Casper dan semua Pilot yang boleh
menyambung ke TCP 5000 dan 5001. Node lain, termasuk yang satu switch, harus langsung
ditolak di kedua port itu, bukan dibiarkan menggantung sampai timeout. Lalu lintas lain
tidak boleh terganggu. Balthasar tetap harus bisa ping 8.8.8.8 dan menerima balasannya.
Buktikan nc ke port 5000 dan 5001: Unit-01 berhasil, Lilith dan Melchior ditolak seketika.

## Soal 14. Jam kunjungan sang pinguin

Pen-Pen, pinguin Misato, menyimpan sesuatu yang bikin banyak orang penasaran. Ia hanya
boleh dihubungi sesuai jadwal di bawah. Jadwal berlaku untuk ping dan TCP 80 ke Pen-Pen,
dari mana pun asalnya. Jam komputer tidak boleh diubah.

| Kelompok | Jadwal | Keterangan |
|---|---|---|
| Pilot | 08.00 sampai 16.00 WIB | Hanya Senin sampai Jumat |
| Staf Geofront | 17.00 sampai 23.00 WIB | Setiap hari |
| Terduga Dummy System | Tidak pernah | Selalu ditolak |
| Lainnya | Bebas | Tanpa batasan |

Simpan jadwal sebagai variabel di awal script Pen-Pen, bernama `PILOT_START`,
`PILOT_STOP`, `PILOT_DAYS`, `STAF_START`, dan `STAF_STOP`. Untuk uji, ubah variabel itu
sampai jadwalnya cocok dengan jam sekarang, lalu sampai tidak cocok. Ingat, iptables
membaca jam UTC, 7 jam lebih lambat dari WIB. Tunjukkan hasil ping dari Unit-01, Lilith,
dan Spy pada kedua kondisi.

## Soal 15. Banjir ping di pusat komando

Instrumentality makin dekat, dan semua orang berebut mem-ping Tokyo3-Core. Banjir ping
bisa melumpuhkannya. Batasi Tokyo3-Core agar hanya menjawab maksimal 5 ping per detik, dan
buang kelebihannya. Aturan ini tidak boleh mengganggu ping di bawah batas. Aturan ini juga
tidak boleh membatasi ping yang dikirim Tokyo3-Core sendiri. Pastikan rule benar-benar
mengenai paket yang dituju. Periksa urutan rule dan jenis paketnya. Buktikan ping rapat
dari Lilith, misalnya `ping -i 0.1 -c50`, sebagian hilang, dan tunjukkan counter rule
pembuangan naik. Buktikan ping biasa semuanya berhasil, dengan jeda minimal 3 detik di
antaranya.

## Soal 16. Pencuri yang mencoba setiap pintu

Dari Switch-Lab, Dummy-Plug mengetuk port sensitif Tokyo3-Core satu per satu seperti
pencuri. Ritsuko ingin tahu setiap percobaannya, dan pintunya harus dibanting. Setiap
koneksi TCP dari Dummy-Plug ke port 22, 23, dan 3306 di Tokyo3-Core harus dicatat di log
dengan prefix huruf besar `SCAN-NERV:`, lalu ditolak seketika. Port 80 dan 8080 tetap bisa
diakses, dan klien lain tidak boleh terganggu. Buktikan `nc -w` dari Dummy-Plug ke port
22, 23, dan 3306 langsung ditolak, dan ke port 80 dan 8080 berhasil. Tunjukkan counter
rule pencatatan, karena log kernel sering tidak terlihat di container.

## Soal 17. Sihir hitam di jalur paket

Ritsuko meninggalkan memo di meja Gendo. Paket dari Casper ke Zeruel tidak pernah sampai
ke Zeruel, karena ada router yang diam-diam mengalihkannya ke tempat lain. "Cari saja
router yang menjadi gateway Switch-Terminal." Gendo menyuruhmu meniru router yang
diam-diam itu untuk menjebak siapa pun. Di Fuyutsuki, alihkan setiap koneksi TCP dari
Magi-Casper ke Zeruel supaya sampai di Pen-Pen. Casper tidak boleh sadar. Balasan yang ia
terima harus tetap terlihat datang dari Zeruel. Ping tidak dialihkan. Nyalakan listener
port 9000 di Pen-Pen dan Zeruel. Dari Casper, kirim pesan ke IP Zeruel port 9000. Buktikan
pesan itu muncul di Pen-Pen, bukan di Zeruel. Tunjukkan juga tabel conntrack di Fuyutsuki.

## Soal 18. Gerbang Monolith dan Radar

Monolith dan Radar menyimpan rahasia terbesar NERV, dan Shigeru menjaganya tanpa
kompromi. Pasang aturan di Shigeru. Node di dalam zona boleh memulai koneksi ke mana saja,
dan balasannya tetap sampai. Node di luar zona tidak boleh memulai koneksi ke dalam zona.
Satu pengecualian: ping dari subnet Terminal boleh masuk. Tolak berdasarkan arah
datangnya, yaitu semua yang datang dari sisi Shigeru yang menghadap Gendo. Jangan menulis
subnet luar satu per satu, supaya subnet baru langsung ikut terblokir. Buktikan ping
Casper ke Zeruel berhasil, sedangkan ping Lilith ke Zeruel gagal.

## Soal 19. Hukuman untuk sang pengkhianat

Hitung mundur tinggal beberapa menit. Armisael tidak boleh lagi bicara dengan siapa pun,
tapi Zeruel di sebelahnya tidak boleh ikut terhukum. Blokir semua lalu lintas masuk dan
keluar Armisael, termasuk ke gateway dan tetangga satu switch. Isolasi tidak boleh
dikerjakan di dalam Armisael, karena pengkhianat tidak bisa dipercaya. Untuk uji nc,
nyalakan listener TCP 9000 di Armisael, Zeruel, dan Lilith. Ke Seele pakai TCP 53.
Buktikan ping dan nc dari Armisael ke Seele, Zeruel, dan Lilith gagal. Buktikan ping dan
nc dari Zeruel ke Seele dan Lilith berhasil.

---

# Babak Final: Siap Dihancurkan, Siap Dibangun Lagi

## Soal 20. Uji kebakaran Geofront

NERV harus bisa dibangun ulang dengan cepat. Pilih tiga node sendiri, minimal satu router.
Di tiap node, simpan dulu rule dengan `iptables-save` ke `/root/<nama>.rules`. Lalu hapus
semua rule iptables termasuk tabel nat, dan hapus rute yang kamu tambahkan sendiri.
Pulihkan rule dengan `iptables-restore`, dan pulihkan rute dengan `/root/<nama>.sh`.
Lakukan pemulihan itu dua kali berturut-turut. Hasilnya harus sama: rule dan rute tidak
dobel, dan node berfungsi seperti semula. Buktikan jumlah baris `iptables -S` dan
`iptables -t nat -S` sama setelah pemulihan pertama dan kedua. Buktikan juga node itu
berfungsi lagi.

---

Catatan penutup asisten: jangan dikerjakan mepet deadline.
