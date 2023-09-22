# Laporan Resmi Praktikum 1 E01
|Anggota        |NRP        |
|---------------|-----------|
|Hammuda Arsyad |5025211146 |

## 1

Soal: 

*User melakukan berbagai aktivitas dengan menggunakan protokol FTP. Salah satunya adalah mengunggah suatu file.*

karena yang diminta adalah aktifitas mengunggah, maka command ftp yang digunakan adalah STOR

Sehingga untuk mencari paket yang menggunakan command STOR gunakan filter berikut:
```
ftp contains "STOR"
```

![filter_stor]()

> Kendala yang saya alami adalah menggunakan referensi command clients yang tidak standar (send, put), dan tidak menggunakan raw command yang telah diatur oleh IETF sendiri (STOR)

Pada ncat yang diberikan terdapat beberapa sub soal:

*a. Berapakah sequence number (raw) pada packet yang menunjukkan aktivitas tersebut?*

* Dapat ditemukan pada bagian `Transmission Control Protocol`
    ```
    Sequence Number (raw): 258040667
    ```

![1_a]()

*b. Berapakah acknowledge number (raw) pada packet yang menunjukkan aktivitas tersebut?*

* Dapat ditemukan pada bagian `Transmission Control Protocol`
    ```
    Acknowledgment number (raw): 1044861039
    ```

![1_b]()

*c. Berapakah sequence number (raw) pada packet yang menunjukkan response dari aktivitas tersebut?*

* Dengan menggunakan `follow tcp stream`, kita akan diarahkan ke paket respons secara langsung
* Kemudian, seq number raw dapat ditemukan pada bagian `Transmission Control Protocol`
    ```
    Sequence Number (raw): 1044861039
    ```

![1_c]()

*d. Berapakah acknowledge number (raw) pada packet yang menunjukkan response dari aktivitas tersebut?*

* Dapat ditemukan pada bagian `Transmission Control Protocol`
    ```
    Acknowledgment number (raw): 258040696
    ```

![1_d]()

Setelah semua subsoal telah dijawab maka flag akan diberikan.

![flag_a]()

## 2

Soal:

*Sebutkan web server yang digunakan pada portal praktikum Jaringan Komputer!*

Karena ip dari web praktikum diketahui maka dapat di cari menggunakan filter berikut:
```
ip.addr =
```

## 3

## 4

## 5

## 6

## 7

## 8

## 9

## 10
