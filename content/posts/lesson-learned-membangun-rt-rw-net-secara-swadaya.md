---
title: "Lesson Learned : Membangun RT/RW Net Secara Swadaya"
date: 2020-09-21T09:06:23+07:00
draft: false

categories: ["Networking"]
tags: ["Mikrotik", "RTRWNet"]
---

Saat ini kebutuhan akan koneksi internet sangatlah tinggi. Terutama di masa pandemi yang
mana segala aktivitas disarankan untuk dilakukan secara online. Beruntung di awal tahun
2020 kawasan rumah saya *(re : Beran)* telah dijangkau oleh jaringan Indihome.

Sebelum dijangkau jaringan Indihome, warga Beran telah berlangganan internet di
salah satu warga. Warga ini punya tower sehingga bisa mengambil koneksi internet
(Indihome) dari tempat lain. Secara harga juga relatif murah, hanya 65ribu/bulan dengan
kecepatan 1 Mbps unlimited. Sistemnya dari satu rumah ke rumah lain dikoneksikan dengan
kabel UTP. Total ada 15 rumah di Beran yang berlangganan internet dari warga di atas.

Kemudian ada suatu moment di mana, warga penyedia RT/RW Net di atas memiliki kendala
sehingga tidak bisa menyediakan internet. Di sisi lain jangkauan jaringan Indihome makin
luas sehingga bisa menjangkau kawasan Beran.

Hasil dari diskusi pemuda Beran memutuskan untuk membangun jaringan RT/RW Net secara
swadaya dengan memanfaatkan jaringan yang telah ada (hasil dari berlangganan RT/RW Net di
atas). 
Alasannya jika setiap rumah 
berlangganan Indihome secara mandiri maka biayanya akan mahal *(300rb/bulan)*. Dan juga
kecepatan 10 Mbps untuk satu rumah tergolong mubazir.

##### Rancangan Jaringan

Sebelum merancang jaringan, pemuda Beran mengumumkan kepada semua warga bahwa akan membuat
jaringan RT/RW Net yang sifatnya swadaya dan non profit. Hasil dari pendataan ada 28 rumah
yang akan ikut berlangganan internet ini. 

Skenarionya akan berlangganan Indihome dengan kecepatan 100 Mbps yang mana setiap rumah
mendapatkan kecepatan 5 Mbps. Untuk iuran per bulan disepakati 65 ribu. Khusus untuk rumah
yang baru berlangganan internet maka perlu mengeluarkan biaya ekstra. Rinciannya akan 
dibahas pada bagian *kebutuhan dana.*

Untuk kemudahan penggunaan maka dibuat sistem hotspot di mana setiap rumah
mendapatkan jatah 10 device. Harapannya dimanapun lokasinya, asal masih terhubung ke
jaringan Beran maka tidak perlu login ke router. Hal ini sangat bermanfaat ketika ada
kumpulan/pengajian di suatu rumah, hp bisa terhubung ke internet tanpa perlu bertanya
password :) Juga untuk penghitungan penggunaan kuota pada
masing-masing rumah.

##### Kebutuhan Alat

Kebutuhan alat ini bisa dibagi menjadi 2 kategori, pertama alat di titik pusat sebagai
pembagi kecepatan dan kedua alat yang berada di setiap rumah.

Alat yang berada di titik pusat menggunakan **MikroTik RB450gx4** yang mana koneksi
internet dari router Indihome akan dimasukkan ke Mikrotik. Pada Mikrotik ada beberapa hal
yang perlu dikonfigurasi yaitu :

- Dial koneksi ke Indihome menggunakan PPPoE (bridge mode)

- DHCP Server

- Masquerade NAT

- Hotspot

- Pembatasan kecepatan menggunakan Queue Tree dan Mangle


Selain itu di titik pusat juga ada UPS sebagai penyuplai daya ketika terjadi mati listrik.
Kemudian alat yang berada di setiap rumah yaitu :

- Wireless Router yang disetting sebagai Access Point Mode

- Kabel UTP

- Switch _(optional)_

Panjang kabel UTP menyesuaikan dengan jarak rumah satu dengan lainnya. Rata-rata kebutuhan kabel
UTP adalah 40 meter tiap rumah. Khusus untuk menghubungkan rumah tertentu menggunakan Fiber Optic
yang mana dijadikan sebagai backbone. Karena RB450gx4 dan Wireless Router yang digunakan
belum support port FO sehingga perlu alat tambahan berupa HTB FO.

Berikut kondisi titik utama di mana terdapat Router dari Indihome, Mikrotik RB450gx4 dan 
HTB FO :

![Titik Utama] (/img/titik-utama.jpg)

##### Kebutuhan Dana

Untuk membeli berbagai peralatan di atas, maka diadakan sistem iuran wajib 100ribu/rumah.
Secara garis besar berikut kebutuhan dana instalasi awal RT/RW Net di Beran :

![Pengeluaran Titik Utama] (/img/pengeluaran-titik-utama.png)

Karena dari rincian dana di atas hasilnya *minus* sehingga untuk sementara bisa ditalangi
menggunakan dana kas pemuda Beran. Kemudian untuk kebutuhan dana di setiap rumah seperti
berikut :

![Kebutuhan Dana Setiap Rumah] (/img/kebutuhan-dana-rumah.png)

Daftar di atas merupakan kebutuhan dana untuk rumah yang sebelumnya tidak berlangganan
internet. Artinya butuh alat berupa Switch, Router dan kabel UTP. Untuk kabel UTP dihitung
2ribu per meter. Dan berikut salah satu moment yang terabadikan ketika setting
Wireless Router sebagai Access Point mode :

![Instalasi Internet Awal] (/img/instalasi-internet-awal.jpg)


##### Monitoring Jaringan

Setelah instalasi dan konfigurasi selesai, selanjutnya perlu memikirkan mekanisme
monitoring jaringan. Hal ini karena RT/RW Net yang dibangun bisa dikatakan tidak memenuhi
standart, terutama penggunaan kabel. Kabel UTP yang harusnya digunakan di indoor kami
gunakan di outdoor yang bahkan rawan tertimpa pohon.

[The Dude] (https://mikrotik.com/thedude) merupakan package yang dari Mikrotik untuk
melakukan berbagai macam kebutuhan monitoring. Untuk RT/RW Net Beran, saya hanya akan
menggunakan The Dude untuk memonitoring ping ke semua Router.

Skenarionya mikrotik secara berkala melakukan ping ke semua Router, jika ada router yang
status ping-nya timemout maka akan mengirimkan notifikasi ke grup telegram.

![Notifikasi The Dude Telegram] (/img/notifikasi-the-dude-telegram.jpg)

Nah, grup telegram di atas berisi beberapa orang yang ditugasi sebagai teknisi. Ketika ada
notifikasi maka teknisi yang available akan segera mengecek ke lokasi.
Karena RT/RW Net ini bersifat swadaya, jadi tidak ada bayaran apapun untuk teknisi :)

Gambaran monitoring Wireless Router menggunakan The Dude :

![The Dude Topologi] (/img/the-dude.jpg)

##### Jualan Voucher Hotspot

Secara keseluruhan jumlah rumah di Beran sekitar 50 rumah padahal yang berlangganan RT/RW
NET hanya 28 rumah. Artinya masih ada lebih dari 20 rumah yang tidak berlangganan. Karena
RT/RW Net ini bersifat swadaya akhirnya diputuskan untuk menjual voucher. 

Harapannya dengan adanya voucher ini, warga yang tidak berlangganan pun dapat menikmati koneksi
internet yang stabil dengan harga murah. Terutama warga yang memiliki anak sekolah.
Voucher dijual dengan harga 10ribu dengan kuota 5 GB. 

![Voucher Hotspot] (/img/voucher-hotspot.jpg)

Voucher di atas di-*generate* menggunakan [Mikhmon](https://mikhmon.online/), yaitu sebuah
aplikasi untuk mengelola hotspot vocuher. Alasan saya lebih memilih **Mikhmon**
dibanding **Userman** (paket bawaan dari Mikrotik untuk mengelola hotspot voucher) adalah :

- Mikhmon lebih ringan dibanding Userman

- Mikhmon tidak perlu aktif selama 24 jam, karena bukan radius server seperti Userman yang
harus aktif 24 jam

- Kemudahan konfigurasi Mikhmon

##### Kendala

###### 1. Internet Indihome Ber-FUP

Sudah menjadi rahasia umum bahwa internet Indihome yang katanya *unlimited* ternyata
ber-FUP. FUP (Fair Usage Policy) adalah kebijakan batasan pemakaian internet yang
disediakan oleh provider penyedia layanan internet. Mudahnya jika pemakaian internet sudah
mencapai sekian GB maka kecepatan internet diturunkan.

Untuk kasus RT/RW Net Beran yang berkecepatan 100 Mbps, jika pemakaian sudah melebihi 2000
GB maka kecepatan diturunkan menjadi 20 Mbps. Ini menjadi masalah karena secara rata-rata
penggunaan RT/RW Net Beran mencapai 30-40 Mbps. Dan baru tanggal 15 saja penggunaan sudah
melebihi 2000 GB, terutama masa pandemi seperti ini.

![Beran Network Average] (/img/beran-network-avg.jpg)

Solusinya setiap rumah diberikan kuota 100 GB. Setelah melebihi kuota maka kecepatan akan
diturunkan menjadi 256 kbps. Hal ini dilakukan untuk menjaga RT/RW Net yang sifatnya
swadaya tetap adil. Karena secara pola pemakaian setiap rumah berbeda.

Kemudian bagi rumah yang penggunaannya telah diturunkan menjadi 256 kbps, ada opsi untuk
membeli voucher hotspot. Dan penjualan voucher ini berfungsi untuk menambah uang kas.

Karena permasalahan FUP di atas, ada 6 rumah yang melepaskan diri dari RT/RW Net Beran
karena kurang setuju dengan sistem kuota. Ya inilah yang dinamakan lika liku menjalankan
RT/RW Net secara swadaya.

###### 2. Sidak dari Telkom

Setelah selesai dengan permasalahan FUP, sekitar bulan Juni pihak telkom secara mendadak
melakukan sidak terhadap pelanggan yang menggunakan kecepatan di atas 50 Mbps. Alasannya
koneksi Indihome tidak boleh dishare ke rumah lain. Menurut telkom ini termasuk tindakan
illegal.

Kemudian pihak telkom memberikan 3 pilihan :

- Kecepatan internet diturunkan menjadi 20 Mbps

- Kecepatan internet diturunkan menjadi 10 Mbps

- Internet Indihome diputus secara total

Jika tidak memilih salah satu dari 3 pilihan di atas, maka pihak Telkom mengancam akan
di bawa ke ranah hukum. Berikut surat perjanjian dengan pihak telkom :

![Surat Telkom] (/img/surat-telkom.png)

Akhirnya kami memilih untuk menurunkan kecepatan menjadi 20 Mbps. Konsekuensinya kecepatan
masing-masing rumah diturunkan menjadi 1 Mbps. Ini dilakukan demi keadilan dan juga FUP
kecepatan 20 Mbps hanya 600 GB


###### 3. Tambah Upstream : Telkom WMS

Dampak dari penurunan kecepatan Indihome menjadi 20 Mbps, maka kami harus menambah koneksi
baru. Pilihan yang paling mudah dengan menambah koneksi dari Indihome dengan kecepatan 20
Mbps juga. Tapi pertimbangannya jika menggunakan Indihome lagi maka akan bermasalah dengan
FUP.

Setelah tanya sana-sini akhirnya pilihan jatuh kepada Telkom WMS. WMS (Wifi Managed
Service) merupakan layanan wifi dari telkom yang terdiri dari 3 ssid (@wifi.id,
seamless@wifi.id dan ssid venue).

Dengan menggunakan WMS maka tidak perlu memikirkan FUP dan secara harga juga lebih murah.
WMS dengan kecepatan 20 Mbps harganya sekitar 300 ribu. Kemudian karena WMS berbasis
wireless sehingga perlu tambahan alat. Akhirnya kami membeli **Mikrotik RB952ac** yang
bisa support wifi dengan frekuensi 2,4 GHz dan 5 GHz.

Sehingga saat ini RT/RW Net Beran memiliki 2 sumber internet dengan pembagian :

- Indihome khusus untuk trafik Youtube

- WMS untuk semua trafik kecuali Youtube

Dengan skenario seperti di atas, setiap rumah bisa mendapatkan kecepatan 3 Mbps dan tanpa
kuota.

##### RT/RW Net Illegal?

Untuk menghindari label RT/RW Net Illegal, harapannya ke depan kami bisa membeli koneksi
internet dari ISP sehingga ada perijinan. Dan juga dalam waktu dekat akan ada pembagunan
jalan tol yang mana berpotensi mengganggu jalur Fiber Optic dari telkom, sehingga ada
kemungkinan kami tidak bisa lagi berlangganan Indihome/WMS.

##### Dibalik Layar RT/RW Net Beran

![Beran Squad] (/img/beran-squad.jpg)

Foto diambil tahun 2018 di Pantai Cemara Sewu
