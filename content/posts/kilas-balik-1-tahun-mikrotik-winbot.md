---
title: "Kilas Balik 1 Tahun Mikrotik Winbot"
date: 2021-07-21T20:38:32+07:00
draft: false
categories: ["Networking"]
tags: ["Mikrotik", "Chatbot", "Winbot"]
---

##### Backstory

Cerita bermula ketika di awal tahun 2020 saya didapuk menjadi admin RT/RW Net di kampung. Idenya adalah menyediakan internet murah bagi warga. Waktu itu mayoritas warga sepakat untuk berlangganan internet ke ISP plat merah yang kemudian disalurkan dari satu rumah ke rumah lain. Guna pengaturan yang terpusat maka saya memutuskan untuk membeli router [Mikrotik seri RB450Gx4] (https://mikrotik.com/product/rb450gx4). Router tersebut mengatur beberapa hal, di antaranya :

1. Dial koneksi ke ISP
2. DHCP Server
3. Masquerade NAT
4. Hotspot
5. Pembatasan kecepatan menggunakan Queue Tree dan Mangle
6. Load balancer

Seiring berjalannya waktu, saya sebagai admin sering mendapat komplain terkait performa jaringan internet yang lemot/mati. Jika dilihat dari sisi teknis, internet lemot/mati bisa terjadi karena beberapa hal. Misalnya perangkat di suatu rumah mati, atau bisa juga jatah FUP sudah habis atau memang terjadi gangguan pada ISP.

Biasanya ketika ada komplain seperti di atas, saya harus membuat Winbox (tool untuk remote Mikrotik), kemudian melakukan pengecekan secara manual. Tentunya hal ini jika dilakukan secara berulang akan sangat melelahkan.

Seketika jiwa programmer saya tergugah untuk melakukan otomatisasi. Ide awalnya adalah membuat sebuah chatbot untuk melakukan monitoring Mikrotik. Jika ada kejadian tertentu (misalnya router mikrotik mati atau perangkat di suatu rumah mati), maka saya akan mendapatkan notifikasi berupa chat. Juga ketika melakukan pengecekan sesuatu tidak harus masuk Winbox, cukup kirim pesan ke bot saja. Chatbot tersebut saya beri nama Mikrotik Winbot (https://mikrotik-winbot.com).

Note : jika ingin tau lebih detail terkait RT/RW Net di tempat saya, bisa baca artikel [Lesson Learned : Membangun RT/RW Net Secara Swadaya] (https://herusetiawan.id/posts/lesson-learned-membangun-rt-rw-net-secara-swadaya/).

##### Apa itu Mikrotik Winbot?

Mikrotik Winbot merupakan sebuah chatbot yang bisa digunakan untuk konfigurasi dan monitoring Mikrotik. Winbot memanfaatkan service API pada mikrotik untuk bisa mengambil data serta memodifikasi pengaturan pada Mikrotik.

Pada awalnya layanan winbot hanya saya gunakan secara pribadi. Kemudian karena dirasa bisa memberikan kemudahan bagi para admin jaringan lain, maka saya memutukan untuk merilis ke publik pada tanggal 11 Mei 2020. Saat perilisan awal Winbot hanya memiliki beberapa fitur dasar di antaranya :

1. Hanya bisa diintegrasikan dengan platform Telegram
2. Mengirimkan notifikasi ketika router Mikrotik mati
3. Monitoring log Mikrotik yang telah ditentukan keywordnya
4. Dapat melakukan montoring CPU, RAM, dan Disk pada Mikrotik

Selama satu tahun terakhir, Winbot telah merilis 14 kali versi baru. Setiap perilisan versi baru selalu memberikan fitur baru, improvement dan perbaikan bug. Berikut beberapa improvement signifikan dalam satu tahun terakhir :

1. Winbot bisa dintegrasikan dengan platform WhatsApp
2. Winbot bisa merekap laporan penjulaan voucher hotspot
3. Billing untuk user PPPoE

Winbot yang awalnya hanya sebatas chatbot untuk monitoring Mikrotik, kini telah berevolusi memiiki fitur-fitur yang lebih advanced sehingga dapat lebih memudahkan pekerjaan admin jaringan.

Note : untuk cek detail perkembangan fitur Winbot bisa cek https://www.mikrotik-winbot.com/changelog


##### Komponen Winbot

Jika dijabarkan secara rinci, maka Winbot memiliki 2 komponen utama dan 1 komponen tambahan.  Komponen utama yaitu engine dan dashboard. Engine berfungsi sebagai otaknya chatbot. Berbagai macam operasi dijalankan menggunakan engine terutama komunikasi antara mikrotik dan platform chat (whatsapp maupun telegram. Sedangkan dashbaord berfungsi untuk pendaftaran user baru, login, dan pengaturan koneksi ke Mikrotik.

Sementara komponen tambahan yaitu VPN Remote menggunakan Mikrotik CHR. Komponen ini berfungsi agar router Mikrotik yang tidak mempunyai ip publik tetap bisa diakses dari jaringan luar (internet). Fungsi dari komponen ini adalah optional artinya pengguna winbot tidak harus menggunakan VPN Remote (jika memiliki ip publik).

Note : untuk detail tech stack yang digunakan Winbot akan saya tulis pada artikel khusus.

##### Jumlah User

Saat artikel ini diposting, jumlah user yang terdaftar pada sistem Winbot sebanyak 777 user. Jika di rata-rata maka lebih dari 50 user baru melakukan registrasi setiap bulan. Mayoritas user tersebut datang secara organic berkat konten di Youtube.

![Mikrotik Winbot Playlist] (/img/mikrotik-winbot-playlist-yt.png)

Berikut grafik penambahan user baru Winbot setiap bulan :

![Mikrotik Winbot Monthly New User] (/img/mikrotik-winbot-new-user-monthly.png)

Khusus untuk bulan Mei 2020 dan Maret 2021, penambahan user baru lebih dari 100 dikarenakan saya melakukan promosi melalui beberapa grup facebook.

##### Winbot Payment

Winbot memiliki 3 paket berbayar yaitu paket Starter, Intermediate dan Advanced. Pada awalnya harga bulanan paket starter 40 ribu, Intermediate 60 ribu dan Advacend 100 ribu. Kemudian harga Winbot berbayar diganti menjadi Starter 25 ribu, Intermediate 40 ribu, dan Advanced 60 ribu.

Hingga artikel ini diposting, jumlah pembelian Winbot berbayar sebanyak 53 kali. Berikut grafik setiap bulan :

![Mikrotik Winbot Monthly Paid User] (/img/mikrotik-winbot-paid-user-monthly.png)

Dan berikut perbandingan antara user baru dan user berbayar :

![Mikrotik Winbot Monthly New vs User] (/img/mikrotik-winbot-new-paid-monthly.png)
