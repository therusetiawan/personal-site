---
title: "Monitoring Jaringan Tanpa the Dude"
date: 2020-05-17T16:33:07+07:00
draft: false
categories: ["Networking"]
tags: ["Mikrotik", "Chatbot", "Winbot"]
---

Jika anda pengguna mikrotik pasti sudah tidak asing lagi dengan aplikasi [The Dude](https://mikrotik.com/thedude). Ya aplikasi ini digunakan untuk memonitoring suatu jaringan komputer. The Dude mempunyai banyak fitur, salah satu yang sering digunakan yakni monitoring perangkat yang terhubung ke jaringan. Cara kerjanya ketika suatu perangkat mengalami gangguan (re: down) maka The Dude akan mengirimkan notifikasi. Menariknya The Dude menawarkan berbagai platform untuk notifikasi di antaranya bunyi beep pada mikrotik, flash, email, sms, dan popup. Bahkan The Dude juga memberikan support untuk membuat notifkasi sendiri (custom notification). Kemudian protokol yang dapat dimonitoring menggunakan The Dude pun bermacam-macam mulai dari icmp, http, ssh, ftp dan sebagainya.

Secara default The Dude belum terinstall pada mikrotik sehingga harus mendownload package kemudian menginstallnya. Namun sebelum menginstall The Dude ada beberapa faktor yang perlu diperhatikan, di antaranya:

- Tidak semua versi RouterOS bisa di-install The Dude, hanya yang sudah menggunakan versi v6.34rc13 atau di atasnya

- Pada mikrotik yang memiliki RAM 16 MB maka harus menambahkan USB Storage

- The Dude membutuhkan The Dude client yang harus diinstall pada komputer/laptop

- Jika hanya ingin memonitoring jaringan (up/down) maka banyak fitur The Dude yang tidak terpakai (overkill)

Melihat berbagai faktor di atas, maka alangkah baiknya ketika akan memonitoring jaringan menggunakan tool selain The Dude sehingga dapat menghemat resource mikrotik. Salah satu tool yang dapat digunakan yaitu Netwatch. Netwatch merupakan tool pada mikrotik yang dapat memonitoring suatu host dengan cara mengirimkan paket icmp (ping) ke suatu ip address yang telah ditentukan. Kemudian netwatch dilengkapi dengan fasilitas script ketika suatu host mengalami up/down. Nah script inilah yang dapat dimanfaatkan untuk mengirimkan notifikasi ke platform tertentu (email, sms, telegram, dsb).

![Mikrotik Netwatch] (/img/mikrotik-netwatch.png)

Saat ini platform notifikasi yang paling populer ketika memonitoring jaringan menggunakan netwatch yaitu notifikasi dikirimkan ke telegram. Untuk itu ada beberapa hal yang perlu dipersiapkan, di antaranya :

- Membuat bot menggunakan BotFather

- Menyiapakan bot token

- Menyiapkan chat id

- Menyiapkan script untuk mengirimkan notifikasi ke telegram

Jika anda tidak ingin direpotkan dengan hal-hal di atas dalam memonitoring jaringan maka anda perlu mencoba [Mikrotik Winbot](https://mikrotik-winbot). Mikrotik Winbot merupakan sebuah layanan yang menyediakan telegram chatbot untuk melakukan konfigurasi dan monitoring jaringan. Pada versi 1.0.1 yang dirilis pada 17 Mei 2020, Winbot memperkenalkan fitur baru yang dinamakan monitoring device. Fitur ini merupakan bentuk penyederhaan penggunaan netwatch untuk monitoring jaringan.

![Mikrotik Winbot Telegram] (/img/mikrotik-winbot-telegram.jpeg)

Pengguna tidak perlu membuat bot menggunakan BotFather, tidak perlu menyediakan bot token dan chat id. Bahkan pengguna pun tidak perlu membuat script. Hal-hal tersebut sudah secara otomatis ditangani oleh Winbot. Pengguna hanya cukup mengirimkan pesan ke [@MikrotikWinbot](https://t.me/MikrotikWinbot)

Untuk dapat menggunakan fitur monitoring device pada Mikrotik Winbot silahkan lakukan pendaftaran ke website https://mikrotik-winbot.com. Selesaikan proses registrasi kemudian lakukan aktivasi akun telegram dan tambahkan informasi mikrotik anda. Langkah-langkah secara detail dapat dibaca [di link berikut](https://bit.ly/PanduanMikrotikWinbot).

Nah kali ini saya akan memberikan contoh topologi jaringan yang akan dimonitoring menggunakan Winbot. Jaringan terdiri dari satu unit Mikrotik dan dua unit Access Point. Skenarionya ketika ada Access Point yang down maka mikrotik akan mengirimkan notifikasi ke telegram. Pun ketika Access Point up kembali, mikrotik juga akan mengirimkan notifikasi.

![Mikrotik Winbot Telegram] (/img/mikrotik-winbot-telegram.jpeg)

Pengguna tidak perlu membuat bot menggunakan BotFather, tidak perlu menyediakan bot token dan chat id. Bahkan pengguna pun tidak perlu membuat script. Hal-hal tersebut sudah secara otomatis ditangani oleh Winbot. Pengguna hanya cukup mengirimkan pesan ke [@MikrotikWinbot](https://t.me/MikrotikWinbot)

Untuk dapat menggunakan fitur monitoring device pada Mikrotik Winbot silahkan lakukan pendaftaran ke website https://mikrotik-winbot.com. Selesaikan proses registrasi kemudian lakukan aktivasi akun telegram dan tambahkan informasi mikrotik anda. Langkah-langkah secara detail dapat dibaca [di link berikut](https://bit.ly/PanduanMikrotikWinbot).

Nah kali ini saya akan memberikan contoh topologi jaringan yang akan dimonitoring menggunakan Winbot. Jaringan terdiri dari satu unit Mikrotik dan dua unit Access Point. Skenarionya ketika ada Access Point yang down maka mikrotik akan mengirimkan notifikasi ke telegram. Pun ketika Access Point up kembali, mikrotik juga akan mengirimkan notifikasi.

![Mikrotik Winbot Telegram] (/img/topologi-jaringan.png)

Sebelum pembahasan lebih jauh, perlu diketahui bahwa fitur monitor device pada Winbot memiliki 3 command yaitu :

- /mydevices : melihat daftar device yang dimonitor

- /newdevice : menambahkan device baru untuk dimonitor

- /deletedevice : menghapus device dari daftar

Di sini saya asumsikan akun telegram telah terkoneksi ke @MikrotikWinbot. Pertama yang perlu dilakukan yaitu menambahkan access-point-1 ke daftar device yang akan dimonitor. Kirimkan pesan ke @MikrotikWinbot dengan format :

```
/newdevice access-point-1 192.168.10.254
```

Kemudian tambahkan access-point-2 . Format pesannya seperti berikut :

```
/newdevice access-point-2 192.168.10.253
```

Cek apakah penambahan kedua access point di atas sudah berhasil atau belum dengan mengirimkan pesan ke @MikrotikWinbot dengan format :

```
/mydevices
```

Sampai di sini langkah sudah selesai. Tinggal menunggu waktu ketika ada Access Point yang mati maka akan ada notifikasi berupa pesan di telegram.

Saat artikel ini dibuat versi terbaru dari Mikrotik Winbot yaitu v 1.0.1 dan layanan masih digratiskan sampai tanggal 30 Juni 2020. Selamat mencoba!
