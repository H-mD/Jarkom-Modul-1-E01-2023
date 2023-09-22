# Laporan Resmi Praktikum 1 E01
|Anggota        |NRP        |
|---------------|-----------|
|Hammuda Arsyad |5025211146 |

## 1

Soal: 

### _User melakukan berbagai aktivitas dengan menggunakan protokol FTP. Salah satunya adalah mengunggah suatu file._

karena yang diminta adalah aktifitas mengunggah, maka command ftp yang digunakan adalah STOR

Sehingga untuk mencari paket yang menggunakan command STOR gunakan filter berikut:
```
ftp contains "STOR"
```

![1-stor](src/1-stor.png)

> Kendala yang saya alami adalah menggunakan referensi command clients yang tidak standar (send, put), dan tidak menggunakan raw command yang telah diatur oleh IETF sendiri (STOR)

Pada ncat yang diberikan terdapat beberapa sub soal:

### _a. Berapakah sequence number (raw) pada packet yang menunjukkan aktivitas tersebut?_

* Dapat ditemukan pada bagian `Transmission Control Protocol`
    ```
    Sequence Number (raw): 258040667
    ```

### _b. Berapakah acknowledge number (raw) pada packet yang menunjukkan aktivitas tersebut?_

* Dapat ditemukan pada bagian `Transmission Control Protocol`
    ```
    Acknowledgment number (raw): 1044861039
    ```

### _c. Berapakah sequence number (raw) pada packet yang menunjukkan response dari aktivitas tersebut?_

* Dengan menggunakan `follow tcp stream`, kita akan diarahkan ke paket respons secara langsung

![1-response](src/1-response.png)

* Kemudian, seq number raw dapat ditemukan pada bagian `Transmission Control Protocol`
    ```
    Sequence Number (raw): 1044861039
    ```

### _d. Berapakah acknowledge number (raw) pada packet yang menunjukkan response dari aktivitas tersebut?_

* Dapat ditemukan pada bagian `Transmission Control Protocol`
    ```
    Acknowledgment number (raw): 258040696
    ```

Setelah semua subsoal telah dijawab maka flag akan diberikan.

![flag_1](src/flag_1.png)

## 2

Soal:

### _Sebutkan web server yang digunakan pada portal praktikum Jaringan Komputer!_

Karena ip dari web praktikum diketahui maka dapat di cari menggunakan filter berikut:
```
ip.addr == 10.21.78.111 && http contains "Praktikum"
```

pilih paket yang berisi `200 ok` yang artinya request berhasil,

pada bagian `Hypertext Transfer Protocol` terdapat field `Server` dengan value nama dari web server yang digunakan

![2](src/2.png)

value tersebut dapat dimasukkan kedalam netcat pada soal dan akan didapatkan flag

![flag_2](src/flag_2.png)

## 3

Soal:

Diberikan netcat, didalamnya ada 2 subsoal

### _a. Berapa banyak paket yang tercapture dengan IP source maupun destination address adalah 239.255.255.250 dengan port 3702?_

* Dapat menggunakan filter berikut:
    ```
    ip.addr == 239.255.255.250 && udp.port == 3702
    ```

* Jumlah paket dapat dilihat pada bagian bawah window wireshark

![3_a](src/3a.png)

    > Dapat menggunakan fungsi statistic dari wireshark dengan menuju tab statistic -> ipv4 Statistics -> destination and ports

### _b. Protokol layer transport apa yang digunakan?_

* Dapat dilihat secara langsung maupun dari analitics bahwa protocol yang menggunakan port 3702 pada ip 239.255.255.250 adalah 
    ```
    UDP
    ```

![3_b](src/3b.png)

Setelah menjawab kedua subsoal diatas flag akan diberikan

![flag_3](src/flag_3.png)

## 4

### _Berapa nilai checksum yang didapat dari header pada paket nomor 130?_

Untuk menuju paket tertentu dapat menggunakan hotkeys `ctrl + g` dan masukkan 130

Checksum paket yang dimaksud ada pada bagian `User Datagram Protocol`

![4](src/4.png)

Masukkan value checksum pada netcat pada soal dan akan didapatkan flagnya

![flag_4](src/flag_4.png)

## 5

Diberikan file pcap dan protected zip

password dari file zip dapat ditemukan pada file pcap dengan melakukan follow pada salah satu paket

![5-follow](src/5-follow.png)

Di dalam conversation tersebut didapatkan password yang terenkripsi beserta instruksi dekripsi nya.

Decrypt password menggunakan base64

![5-decrypt](src/5-decrypt.png)

Gunakan password yang didapatkan untuk membuka file zip.

Dalam file zip terdapat txt file berisi netcat

![5-netcat](src/5-netcat.png)

Masuk kedalam netcat tersebut dan akan muncul beberapa subsoal

### _a. Berapa banyak packet yang berhasil di capture dari file pcap tersebut?_

* Jumlah paket dapat dilihat pada bagian bawah window wireshark

![5a](src/5a.png)

### _b. Port berapakah pada server yang digunakan untuk service SMTP?_

* SMTP standar nya menggunakan port 25

### _c. Dari semua alamat IP yang tercapture, IP berapakah yang merupakan public IP?_

Gunakan fitur statistics wireshark untuk melihat daftar ip yang tertangkap

![5c](src/5c.png)

* 192.168.x.x merupakan ip private yang dibagikan oleh server dhcp pada router konvensional

* 10.10.x.x merupakan ip private yang dimiliki oleh jarikan ITS

* Dengan mempertimbangkan 2 poin diatas, maka ip yang tersisa adalah 74.53.140.153

Setelah menjawab semua subsoal, flag akan diberikan

![flag_5](src/flag_5.png)


## 6 (REVISI)

Soal:

### _Seorang anak bernama Udin Berteman dengan SlameT yang merupakan seorang penggemar film detektif. sebagai teman yang baik, Ia selalu mengajak slamet untuk bermain valoranT bersama. suatu malam, terjadi sebuah hal yang tak terdUga. ketika udin mereka membuka game tersebut, laptop udin menunjukkan sebuah field text dan Sebuah kode Invalid bertuliskan "server SOURCE ADDRESS 7812 is invalid". ketika ditelusuri di google, hasil pencarian hanya menampilkan a1 e5 u21. jiwa detektif slamet pun bergejolak. bantulah udin dan slamet untuk menemukan solusi kode error tersebut._

* "a1 e5 u21" dapat diartikan substitution cypher dimana tiap huruf diganti menjadi representasi angkanya sesuai urutan alfabet, ataupun sebaliknya.

* "SOURCE ADDRESS 7812" dapat diartikan bahwa source ip dari paket nomor 7812 perlu diperhatikan

![6](src/6.png)

Dari kedua petunjuk diatas dapat disimpulkan bahwa kita perlu melakukan substitusi terhadap source ip dari paket nomor 7812

Hasilnya adalah:
```
104.18.14.101 
    = 10 4 18 14 10 1 
    = J D R N J A
```

Masukkan hasil yang didapat (huruf balok tanpa spasi) kedalam netcat yang diberikan, maka flag akan didapatkan

![flag_6](src/flag_6.png)

## 7
Soal:
### _Berapa jumlah packet yang menuju IP 184.87.193.88?_

Dapat menggunakan filter berikut:
```
ip.dst == 184.87.193.88
```

Jumlah paket dapat dilihat pada bagian bawah window wireshark

![7](src/7.png)

> Dapat juga menggunakan fungsi statistics wireshark dengan memilih opsi: analitics -> ipv4 Statistics -> all addresses

![glaf_7](src/flag_7.png)

## 8

Soal:

### _Berikan kueri filter sehingga wireshark hanya mengambil semua protokol paket yang menuju port 80! (Jika terdapat lebih dari 1 port, maka urutkan sesuai dengan abjad)_

* Filter port tcp dengan menggunakan filter berikut:
    ```
    tcp.dstport == 80
    ```

* Filter port udp menggunakan filter berikut:
    ```
    udp.dstport == 80
    ```

Gabungkan kedua filter tersebut menggunakan operrand or

```
tcp.dstport == 80 || udp.dstport == 80
```

masukkab kedalam netcat yang ada pada soal dan flag akan ditemukan

![flag_8](src/flag_8.png)

## 9

Soal: 

### _Berikan kueri filter sehingga wireshark hanya mengambil paket yang berasal dari alamat 10.51.40.1 tetapi tidak menuju ke alamat 10.39.55.34!_

* Filter ip 10.51.40.1 menggunakan filter berikut:
    ```
    ip.src == 10.51.40.1
    ```

* Filter ip 10.39.55.34 menggunakan filter berikut:
    ```
    ip.dst != 10.39.55.34
    ```

Gabungkan kedua filter tersebut menggunakan operrand and

```
ip.src == 10.51.40.1 && ip.dst != 10.39.55.34
```

masukkab kedalam netcat yang ada pada soal dan flag akan ditemukan

![flag_9](src/flag_9.png)

## 10

Soal:

### _Sebutkan kredensial yang benar ketika user mencoba login menggunakan Telnet!_

filter paket sehingga hanya menampilkan protokol telnet dengan filter berikut:
```
telnet
```

> Kendala yang saya alami pada soal ini adalah salah paham bahwa paket berisi username dan password hanyalah data biasa dan belum terjamin kebenarannya sehingga perlu mencari paket yang menunjukkan aktifitas user login itu sendiri

untuk mendapatkan aktifitas login user telnet dapat menggunakan filter berikut:
```
telnet contains "Login"
```

![10-filter](src/10-filter.png)

kemudian lakukan follow tcp stream dan filter sehingga hanya menampilkan dialog client

akan didapatkan username dan password pada baris awal stream

![10-follow](src/10-follow.png)

Masukkan credential yang didapatkan kedalam netcat pada soal sesuai format yang tertera, setelah itu akan diberikan flagnya

![flag_10](src/flag_10.png)



