---
title: "Memulai Petualangan Menggunakan Vim (Part 2)"
date: 2018-01-15T20:53:23+07:00
draft: false
categories: ["Tech"]
tags: ["Terminal", "VIM"]
---

Halo, kembali lagi bertemu dengan post yang mengupas tentang `VIM`.
Post ini merupakan lanjutan dari `Memulai Petualangan Menggunakan VIM (Part 1)`.
Kali ini saya akan bercerita step by step untuk bisa mahir menggunakan VIM. Ya belajar
menggunakan VIM memang berat di awal dan butuh banyak waktu. Maka dari itu jangan berharap
bisa mahir dalam hitungan hari atau bahkan jam. Perlu latihan berulang-ulang agar
jari-jari anda terbiasa dengan perintah di VIM. Kesulitan-kesulitan saat belajar
VIM akan terbayarkan saat anda sudah mahir menggunakannya. So, jangan takut
mencoba karena perjuanganmu tidak akan sia-sia. 

![VIM Learning Curve] (/img/vim-learn-curve.jpg)
Sumber : https://canvas.bham.ac.uk/courses/17216/pages/introduction-to-vim

##### Mengenal mode di VIM
Di VIM ada 3 mode yang bakal sering digunakan yaitu `normal`, `insert` dan `visual`.
Ini sangat berbeda dengan kebanyakan text editor. 
Di text editor lain anda tinggal mengetik sesuatu di keyboard dan dapat langsung
melihat perubahannya di layar. Nah untuk dapat mengetikkan sesuatu di VIM anda harus masuk
ke mode `insert` terlebih dahulu dengan menekan `i`. Kemudian tekan `esc`
untuk kembali ke mode normal. Secara default saat masuk ke VIM maka akan menggunakan mode
`normal`. Di VIM kita akan lebih banyak berinteraksi dengan mode `normal`.

```
- i = masuk ke mode insert
- esc = kembali ke mode normal
- v = masuk ke mode visual
```

##### Menyimpan file di VIM
Setelah mengenal mode dan dapat mengetikkan sesuatu di VIM, maka langkah selanjutnya harus
tahu bagaimana cara menyimpan file. Untuk dapat menyimpan file maka harus masuk ke normal
terlebih dahulu ( tekan `esc`). Kemudian ketikkan perintah `:w` dan tekan enter. Untuk
menyimpan file sekaligus memberi nama file maka perintahnya `:w nama_file.txt`.

```
- :w = menyimpan file
- :wq = menyimpan file dan keluar dari VIM
- :q! = keluar dari VIM tanpa menyimpan file
```

##### Navigasi Dasar VIM
VIM tidak merekomendasikan penggunanya memindahkan kursor menggunakan navigasi arah panah
(arrow) di keyboard. Navigasi tersebut diganti menggunakan `hjkl`. Alasannya karena letak
arrow terlalu jauh sehingga tidak efisien. Dengan menggunakan `hjkl` harapannya posisi
jari tetap stay di keyboard dengan posisi yang konsisten (posisi 10 jari). Yang jelas
perlu pembiasaan untuk dapat menggunakan navigasi `hjkl`. Menariknya `hjkl` bisa
dikombinasikan dengan angka. Misal kita ingin berpindah 10 baris ke bawah, nah cukup
menggunakan `10j`. Hal ini juga berlaku untuk berpindah ke atas, kiri dan kanan.

```
- h = berpindah ke kiri
- j = berpindah ke bawah
- k = berpindah ke atas
- l = berpiindah ke kanan
```

##### Navigasi Antar Kata
Banyak pilihan navigasi di VIM atau dengan kata lain `hjkl` bukan satu-satunya navigasi.
VIM menawarkan navigasi antar kata atau bahkan antar baris. Misalnya dengan menekan 
`w` maka akan berpindah ke kata selanjutnya. Ini dapat menghemat daripada kita harus
menggunakan `l` (berpindah ke kanan) sebanyak n kali. Yang lebih menarik untuk berpindah ke 5 kata
selanjutnya cukup menggunakan `5w`. Selain itu ada juga `b` untuk berpindah ke kata
sebelumnya.

```
- w = berpindah ke kata selanjutnya
- e = berpindah ke kata selanjutnya dan posisi kursor berada di akhir kata
- b = berpindah ke kata sebelumnya
- ^ = berpindah ke awal baris
- $ =  berpindah ke akhir baris
- gg = berpindah ke baris paling atas
- G =  berpindah ke baris paling bawah
- 10G = berpindah ke baris nomor 10
```

##### Delete, Copy, Cut dan Paste
Sama seperti text editor lain, untuk menghapus sesuatu di VIM bisa menggunakan backspace
atau delete. Namun cara ini tidak efektif karena kita harus masuk ke mode `insert`
terlebih dahulu. Nah ada cara yang lebih efektif yaitu menggunakan `x` di mode `normal`.
Untuk menghapus 5 karater sekaligus bisa menggunakan `5x` atau untuk menghapus satu
kata bisa menggunakan `dw`. Operasi hapus juga bisa dikombinasikan dengan angka misal
ingin menghapus 5 kata sekaligus menggunakan `5dw`.

Tentu kita sudah sangat akrab dengan shourtcul Ctrl + c dan Ctrl + v untuk melakukan copy dan
paste. Nah di vim ada sedikit perbedaan. Operasi copy dan paste sebaiknya
dilakukan di mode `normal`. Misalnya untuk meng-copy satu baris sekaligus bisa menggunakan
`yy` kemudian tekan `p` untuk melakukan paste. Atau tekan `wp` untuk meng-copy satu kata.
Yang menarik, di VIM tidak ada operasi khusus cut. Kita cukup mendelete suatu kata misal
`dw` kemudian cukup tekan `p` untuk  mem-paste. Ini yang disebut dengan paste di VIM.

```
- x = menghapus satu karakter
- dd = menghapus satu baris sekaligus
- yy = meng-copy satu baris sekaligus
- p = melakukan pasti di bawah kursor
- P = melaukan paste di atas kursor
- 3dw = menghapus 3 kata sekaligus
- 5dj = menghapus 5 baris sekaligus
- cw = menghapus satu kata kemudian masuk ke mode insert
- cc = menghapus satu baris kemudian masuk ke mode insert
- D = menghapus sampai akhir baris
- C = menghapus sampai akhir baris kemudian masuk ke mode insert
```

###### Blok tulisan
Untuk melakukan blok tulisan di VIM kita harus masuk ke mode visual terlebih dahulu.
Ada 2 cara yaitu menggunakan `v` atau `V`. Untuk memblok baris demi baris direkomendasikan
menggunakan `V`. Namun jika hanya ingin memblok beberapa karater saja menggunakan `v`.
Setelah tulisan berhasil diblok, maka kita bisa melakukan operasi lanjutan. Misalnya jika
ingin menghapus bisa menekan `d` atau jika ingin mengcopy dengn menekan `y` diikuti `p`.

```
- v = memblok karakter demi karakter
- V = memblok baris demi baris
```

##### Searching di VIM
VIM menawarkan 2 cara untuk melakukan pencarian, yaitu forward dan backward search.
Di text editor lain, pencarian sangat akrab dengan shortcut Ctrl + f. Di VIM pencarian
menggunakan `/kata_yang_mau_dicari` kemudian tekan enter. Jika ingin mengetahui hasil
pencarian lain yang cocok bisa menggunakan `n` atau `N`. `n` untuk mencari kata selanjutnya
dan `N` untuk mencari kata sebelumnya.

`````
- /pencarian = forward search
- ?inijugapencarian = backward search
`````

##### Tips Membiasakan Perintah VIM
Dengan banyaknya perintah di VIM tentunya mustahil untuk dapat mengingat dalam waktu
singkat. Salah satu cara untuk membiasakan perintah VIM yaitu menerapkannya di browser.
Dengan menggunakan [Vimium] (https://chrome.google.com/webstore/detail/vimium/), kita bisa
menggunakan navigasi `hjkl` pada browser Google Chrome. Berikut beberapa perintah yang
bisa digunakan di Vimium:
![Vimium Help] (/img/vimium-help.png)
