---
title: "Manage Your Ruby Version using rbenv"
date: 2017-12-18T05:38:38+07:00
draft: false
categories: ["Tech"]
tags: ["Ruby", "Version Manager"]
---

Bisa dikatakan saya mulai mengenal Version Manager sejak menjadi Backend Developer
di [Qiscus] (https://www.qiscus.com) sekitar bulan April 2017.
Sejak saat itu saya mulai akrab dan berkutak dengan project-project dengan bahasa pemrograman Ruby
atau framework Ruby on Rails pada khususnya. Beberapa project yang saya pegang di Qiscus memiliki
requirement versi Ruby yang berbeda-beda.

Misalnya project A butuh ruby versi x, project B
butuh Ruby versi y, dan project C butuh Ruby versi z. Hal ini bisa jadi masalah karena satu
Laptop hanya bisa di install satu versi Ruby saja. Untuk beralih dari satu project ke
project lain (misal project A ke project B) maka saya harus mengubah versi Ruby yang
terinstall di laptop. Atau mengharuskan saya untuk berganti laptop.
Tentunya hal tersebut sangatlah merepotkan. Bisa jadi waktu saya akan habis hanya
untuk mengurusi versi Ruby saja. Nah Version Manager lah solusi dari masalah di atas.

##### Apa itu Version Manager?
Version Manager merupakan sebuah tools yang memungkinkan kita untuk menginstall beberapa
versi aplikasi sekaligus dalam satu komputer atau laptop. Ini artinya dengan Version
Manager kita bisa menginstall Ruby dengan versi x, y dan z dalam satu Laptop. Kemudian
dengan Version Manager kita bisa memilih versi ruby mana yang akan diaktifkan pada suatu
project. Nah di Ruby terdapat beberapa version manager, yang cukup terkenal yaitu
[Ruby Version Manager (RVM)] (https://rvm.io/) dan [rbenv] (https://github.com/rbenv/rbenv).
Hmmm, banyak perdebatan Version Manger mana yang lebih bagus, RVM atau rbenv. Setelah searching sana sini
akhirnya saya menjatuhkan pilihan pada rbenv.

##### Berikut artikel yang membahas tentang RVM vs rbenv:

[Metova blog : rbenv vs RVM] (https://blog.metova.com/choosing-a-ruby-version-management-tool)

[Jonathan Jackson : rbenv vs RVM] (http://jonathan-jackson.net/rvm-and-rbenv)

[Why rbenv?] (https://github.com/rbenv/rbenv/wiki/Why-rbenv%3F)

[Guide to switching to rbenv bliss from RVM hell] (https://gist.github.com/akdetrick/7604130)

##### Gambar untuk mempermanis perdebatan RVM vs rbenv:
![RVM vs rbenv](/img/rvm-rbenv-install.png)

Sumber: [Jonathan Jackson] (http://jonathan-jackson.net/rvm-and-rbenv)


##### Cara Insall rbenv di Ubuntu 16.04

1. Pastikan laptop telah terinstall `git`

2. Clone direktori rbenv ke `~/.rbenv`

	```
	$ git clone https://github.com/rbenv/rbenv.git ~/.rbenv
	```

3. Menambahkan `~/.rbenv/bin` ke `$PATH` agar rbenv bisa diakses dengan perintah `rbenv`
	```
	$ echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bash_profile
	```
	**Jika menggunakan Ubuntu Desktop**: ganti `~/.bash_profile.` dengan `~/.bashrc` 

	**Jika menggunakan Zsh** : ganti `~/.bash_profile.` dengan `~/.zshrc` 

4. Jalankan perintah `~/.rbenv/bin/rbenv` kemudian ikuti langkah selanjutnya agar rbenv
   dapat terintegrasi dengan shell.

5. Restart terminal agar dapat menjalankan `PATH` atau bisa dilakukan dengan membuka
   terminal baru

6. Verifikasi instalasi `rbenv` menggunakan script rbenv-doctor berikut :
```
$ curl -fsSL https://github.com/rbenv/rbenv-installer/raw/master/bin/rbenv-doctor | bash
Checking for `rbenv' in PATH: /usr/local/bin/rbenv
Checking for rbenv shims in PATH: OK
Checking `rbenv install' support: /usr/local/bin/rbenv-install (ruby-build 20170523)
Counting installed Ruby versions: none
  There aren't any Ruby versions installed under `~/.rbenv/versions'.
  You can install Ruby versions like so: rbenv install 2.2.4
Checking RubyGems settings: OK
Auditing installed plugins: OK
```

7. Selesai. Kini saatnya mencoba sensasi `rbenv` :)

###### Cara install ruby menggunakan rbenv
```
# lihat daftar versi ruby yang dapat diinstal:
$ rbenv install -l

# install salah satu versi ruby:
$ rbenv install 2.0.0-p247
```

###### Melihat versi ruby yang telah terinstall
```
$ rbenv versions
  1.8.7-p352
  1.9.2-p290
* 1.9.3-p327 (set by /Users/sam/.rbenv/version)
  jruby-1.7.1
  rbx-1.2.4
  ree-1.8.7-2011.03
```
Keterangan : tanda * berarti versi ruby yang sedang diaktifkan

###### Mengaktifkan versi ruby secara global
Mengaktifkan versi ruby secara global artinya versi ruby ini akan dipakai di semua
direktori. File konfigurasi akan disimpan di direktori `~/.rbenv/version`
```
$ rbenv global 1.8.7-p352
```

###### Mengaktifkan versi ruby secara local
Mengaktifkan versi secara local artinya versi ruby hanya akan dipakai di direktori
tertentu saja. Direktori yang telah di set versi ruby secara local maka secara otomatis
akan ada direktori `.ruby-version`
```
$ rbenv local 1.9.3-p327
```

###### Melihat versi rbenv
```
$ rbenv version
1.9.3-p327 (set by /Users/sam/.rbenv/version)
```
