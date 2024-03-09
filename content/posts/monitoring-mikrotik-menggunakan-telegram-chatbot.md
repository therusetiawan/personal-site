---
title: "Monitoring Mikrotik Menggunakan Telegram Chatbot"
date: 2020-05-12T15:38:25+07:00
draft: false
categories: ["Networking"]
tags: ["Mikrotik", "Chatbot", "Winbot"]
---

Dewasa ini aplikasi chat sangatlah populer. Setiap orang selalu berkomunikasi menggunakan aplikasi chat mulai dari urusan pekerjaan, bisnis ataupun hanya sekedar say hello. Pun aplikasi chat banyak macamnya, mulai dari WhatsApp, Line, Facebook Messenger sampai Telegram. Praktis dan interaktif menjadi alasan di balik kepopuleran aplikasi chat.

Populernya aplikasi chat menjadi hal yang cukup menarik ketika dibawa ke dalam dunia jaringan. Misalnya seorang admin jaringan yang sehari-hari bekerja untuk memonitor dan menjaga kestabilan sebuah jaringan akan sangat sering melakukan pengecekan jaringan. Tools yang digunakan pun bermacam-macam. Sehingga hal ini akan sangat merepotkan. Bayangkan jika admin jaringan tersebut cukup menggunakan sebuah aplikasi chat untuk melakukan monitoring jaringan, pekerjaannya akan sangat mudah bukan :)

Dari berbagai macam aplikasi chat di atas, Telegram merupakan salah satu penyedia yang mendukung adanya chabot. Chatbot merupakan sebuah aplikasi yang dapat berjalan di dalam sebuah chat. Artinya pengguna dapat berinteraksi dengan chatbot menggunakan pesan atau command. Chatbot yang disediakan telegram relatif mudah digunakan dan bersifat gratis.

Namun sayangnya untuk membuat chatbot di telegram dibutuhkan kemampuan programming yang
mumpuni. Kemudian juga membutuhkan server yang digunakan untuk menjalankan chatbot.
Hal-hal demikian yang menjadi penghalang admin jaringan untuk melakukan monitoring
jaringan menggunakan aplikasi chat. Mereka rela menggunakan banyak tools daripada harus
repot membuat sebuah chatbot.

Beruntung saat ini ada sebuah layanan yang dapat mempermudah dalam membuat Telegram Chatbot. [Mikrotik Winbot](https://mikrotik-winbot.com) yang dirilis pada bulan Mei 2020 merupakan sebuah layanan yang menyediakan Telegram chatbot untuk melakukan konfigurasi dan monitoring Mikrotik. Untuk dapat menggunakan Mikrotik Winbot tidak perlu mempunyai kemampuan programming. Anda hanya cukup mendaftar kemudian memasukkan informasi mikrotik maka chatbot pun sudah siap digunakan.

![Alur Kerja Winbot] (/img/alur-kerja-winbot.png)

Mikrotik Winbot v1.0.0 mempunyai beberapa fitur yaitu:

1. Mikrotik Winbot bisa melakukan monitoring 2 Mikrotik
2. Mikrotik Winbot secara otomatis melakukan cek koneksi ke Mikrotik secara berkala setiap 5 menit, jika ada perubahan status koneksi dari connect ke disconnect atau sebaliknya maka akan ada pesan ke telegram
3. Mikrotik Winbot secara otomatis melakukan cek log Mikrotik secara berkala setiap 5 menit berdasarkan keyword kemudian akan mengirimkan log tersebut ke telegram

	- /mymikrotik : melihat daftar mikrotik
	- /info : melihat info mikrotik seperti Board Name, Platform, Version
	- /resource : melihat resource mikrotik seperti cpu load, RAM, Harddisk, Uptime
	- /public : melihat public ip dan ddns mikrotik
	- /ping : melakukan ping ke 8.8.8.8

Bagi anda yang berminat mencoba monitoring Mikrotik menggunakan Telegram Chatbot silahkan langsung mendaftar ke Mikrotik Winbot. Layanan ini masih dalam proses pengembangan dan penggunaan sampai 30 Juni 2020 tidak dikenakan biaya.
