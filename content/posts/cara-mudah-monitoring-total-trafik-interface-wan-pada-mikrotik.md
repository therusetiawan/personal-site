---
title: "Cara Mudah Monitoring Total Trafik Interface WAN pada Mikrotik"
date: 2021-07-07T10:57:38+07:00
draft: false
categories: ["Networking"]
tags: ["Networking", "Mikrotik"]
---

Mikrotik merupakan sebuah router yang bisa dikatakan memiliki paket komplit. Setiap melakukan pembelian router ini maka fitur-fiturnya langsung bisa dinikmati oleh pengguna. Mulai dari pengaturan bandwidth, NAT, firewall, web proxy, DCHP, routing, monitoring interface hingga hotspot.

Untuk fitur monitoring interface, mikrotik dapat melakukan monitoring secara realtime. Buka winbox kemudian pilih menu interfaces. Pada kolom Tx dan Rx dapat dilihat berapa bandwidth yang sedang digunakan saat itu juga.

![Mikrotik Interface] (/img/mikrotik-interface.png)

Anda juga bisa melihat riwayat trafik interface dengan cara melakukan klik 2x pada interface yang anda inginkan. Kemudian pilih tab Trafik.

![Mikrotik Interface Detail Traffic] (/img/mikrotik-interface-detail-traffic.png)

Trafik Tx ditunjukkan dengan garis biru sedangkan trafik Rx ditunjukkan dengan garis merah. Namun riwayat trafik di atas tidak tersimpan pada mikrotik. Ketika window ditutup maka riwayat trafik akan hilang. Dan juga tidak bisa memonitoring total trafik (dalam satuan bytes) yang telah melewati suatu interface.

Padahal biasanya monitoring total trafik pada interface WAN cukup penting bagi pelaku RT/RW Net, khususnya yang berlangganan ke ISP ber-FUP. Yang terjadi adalah internet tiba-tiba lambat karena tidak punya perhitungan total bandwidth yang telah digunakan.

##### Solusi

Dikarenakan mikrotik tidak dapat menyimpan data riwayat monitoring interface maka dibutuhkan layanan tambahan untuk mengatasi masalah di atas. Salah satunya adalah layanan Mikrotik Winbot.

Mikrotik Winbot merupakan layanan yang dapat membantu anda melakukan monitoring dan konfigurasi Mikrotik. Winbot menggunakan service API dalam berkomunikasi dengan Mikrotik sehingga sangant ringan dan tidak membebani Mikrotik.

Yang harus disipakan :

1. Akun mikrotik winbot. Video tutorial https://youtu.be/letdFn4Gbk0
2. Menambahkan mikrotik yang akan dimonitoring . Video tutorial https://youtu.be/JEB_32l-9IU

Hasil akhir dari monitoring total trafik mikrotik (per jam) dapat dilihat pada gambar di bawah ini

![Winbot Traffic] (/img/winbot-traffic.png)

Grafik berisi beberapa data yaitu :

1. Tanggal dan jam
2. Garis berwarna hijau menunjukan total tafik Rx dalam satuan Mega Bytes
3. Garis berwarna merah menunjukkan total trafi Tx dalam satuan Mega Bytes

##### Konfigurasi

1. Pastikan anda telah memiliki akun Mikrotik Winbot dan telah berhasil menambahkan mikrotik yang akan dimonitoring

	![Mikrotik Winbot Dashboard] (/img/mikrotik-winbot-dashboard.png)

2. Tambahkan firewal mangle pada mikrotik yang berfungsi untuk menghitung jumlah trafik pada interface WAN
	<pre><code>/ip firewall mangle
	add chain=forward src-address=[[IP ADDRESS LOCAL]] out-interface=[[NAMA INTERFACE LOCAL]] action=passthrough comment=int-wan-tx
	add chain=forward dst-address=[[IP ADDRESS LOCAL]] in-interface=[[NAMA INTERFACE LOCAL]] action=passthrough comment=int-wan-rx</code></pre>

	`[[IP ADDRESS LOCAL]]` diganti dengan ip address yang digunakan oleh client mikrotik. Milsanya 192.168.100.0/24
	
	`[[NAMA INTERFACE WAN]]` diganti dengan nama interface WAN yang akan dimonitoring. Misalnya ether-WAN
3. Membuat script baru yang berfungsi untuk mengumpulkan data trafik kemudian dikirimkan ke server Mikrotik Winbot
	<pre><code>:local wantxcomment "int-wan-tx"
	:local wanrxcomment "int-wan-rx"
	:local sysnumber [/system routerboard get value-name=serial-number]
	:local txbytes [/ip firewall mangle get [/ip firewall mangle find comment="$wantxcomment"] bytes]
	:local rxbytes [/ip firewall mangle get [/ip firewall mangle find comment="$wanrxcomment"] bytes]
	/tool fetch url=("[[WINBOT URL]]") mode=http keep-result=no
	/ip firewall mangle reset-counters [/ip firewall mangle find comment="$wantxcomment"]
	/ip firewall mangle reset-counters [/ip firewall mangle find comment="$wanrxcomment"]
	:log info ("cleared counters for all mangle rules")</code></pre>

	`[[WINBOT URL]]` didapatkan dari dashboard https://mikrotik-winbot.com (https://mikrotik-winbot.com/) menu Trafffic

4. Simpan script di atas dengan nama `winbot-traffic`

5. Membuat scheduler untuk menjalankan secara otomatis script di atas setiap 1 jam
<pre><code>/system scheduler add name="send-traffic-to-winbot" interval=1h on-event="winbot-traffic"</code></pre>

Maka setiap 1 jam scheduler di atas akan menjalakan script untuk mengirimkan data total trafik ke Mikrotik Winbot. Setelah data tersimpan maka dapat langsung dilihat pada web Mikrotik Winbot dan data akan tersimpan selamanya.
