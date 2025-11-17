<div align="center">

# TUGAS PRAKTIKUM KONSEP JARINGAN

## **LAYER 4 ~ TRANSPORTATION METHOD**

![Image](https://github.com/user-attachments/assets/3ad88b6e-7159-44a2-a004-c909b974a88c)

---

**Dosen Pengampu:**  
Mohammad Robihul Mufid S.ST., M.Tr.Kom  
(199408222020121002)

**Disusun Oleh:**  
Pius Purba (3124521015)

---

## **PROGRAM STUDI D3 TEKNIK INFORMATIKA PSDKU LAMONGAN**  
**DEPARTEMEN TEKNIK INFORMATIKA DAN KOMPUTER**  
**POLITEKNIK ELEKTRONIKA NEGERI SURABAYA**  
**2025**

</div>

---

<div align="center">

# Laporan Transport Layer - OSI Model

</div>

---

## Pendahuluan

Pada bab kali ini akan mempelajari mengenai Transport Layer pada OSI Model setelah sekian lama mempelajari Network layer dengan segala macam jenis dan praktikumnya. Sederhananya transport layer sendiri dibagi menjadi 3 jenis yaitu TCP(ransmission Control Protocol), UDP(User Datagram Protocol) serta pengabunggan kedua metode ini yaitu SCTP(Stream Control Transmission Protocol).

Sedikit akan saya perjelaskan lagi mengenai OSI Model sebagai rangkuman pada Semester kali ini.

---

## Layer-Layer pada OSI Model

### Physical Layer

**Physical Layer** adalah bagian sejati fisik internet itu sendiri. hal ini dipelajari pada pekan 2 yaitu mengenai perkabelan yang meliputi Media Terpandu seperti STP, UTP ( Praktikum) , coaxial cable (untuk TV) serta Fiber Optic sebagai kabel dengan transaksi data tercepat di kelasnya dikarenakan memakai pulsa cahaya melalui serat kaca atau plastik, bukan sinyal listrik seperti pada kabel tembaga sehingga membuat bandwidth nya lah yang terbesar.

---

### Data Link Layer

Lalu ada **Data Link Layer** yang merupakan anjutan dari layer 1 yang menghubungkan tiap end device dengan memakai Access Point seperti switch. Segala yang ada di dalam end device hingga ke access point merupakan bagian dari layer 2 (meliputi MAC address bahkan hingga kecepatan bandwidth hingga bagaimana pesebaran akses internetnya apakah dengan VLAN/TRUNK). Dan juga pada bagan ini juga ketika tiap kali ada error pada config di layer atasnya, layer 2 dapat memberi petunjuk letak kesalahannya.

---

### Network Layer

Kemudian ada **Network Layer** yang diaman ini ada adalah yang kami lakukan sedari pekan ke-6 hingga ke-13. Layer ini meliputi banyak hal seperti routing antar jaringan (seperti protocol yang hendak dipakai), penentuan IP di router serta forwarding packeage sehingga peran device seperti Router adalah kunci dari ini semua.

---

### Transport Layer

Serta di last layer pada jaringan yaitu layer 4 **Transport Layer** merupakan layer yang bertanggung jawab mengenai bagaimana data yang dikirim tiba ke tujuan. Disini kita akan mengenal **TCP** (Transmission Control Protocol) yang merupakan protocol yang perfeksionis didalam mengirim maupun menerima data serta handal seperti akan mengulang kembali jika ada trouble, melakukan error checking serta juga mengurutkan data yang munngkin saja sudah acak ketika dalam proses pengiriman sehingga sangat bagus untuk pengaplikasian WEB, E-mail, Aplikasi hingga transfer file. Kemudian **UDP** (User Datagram Protocol) yang merupakan kebalikan TCP tapi ungul didalam kecepatan serta ringan sehingga sangat cocok ketika live streaming, request DNS, gaming, bahkan ketika kita memakai protocol DHCP pun sejatinya kita secara tidak langsung memakai transport method ini (oleh karena itu kadang ketika baru menghubungkan akan ada RTO message sebagai tanda ada data yang tidak tiba/sampai). Sebenarnya di layer ini juga ada **SCTP** (Stream Control Transmission Protocol) yang merupakan protocol yang membawa keunggulan ke-2 protokol sebelumnya tapi terlalu kompleks untuk dunia yang sudah nyaman dengan TCP serta UDP yang dinilai sudah "cukup" untuk saat ini atau mungkin kedepannya.

---

## Praktikum

### Praktikum 1: Uji Koneksi dengan Eksekusi pada Linux Menggunakan UDP Method

<div align="center">

#### Code udpserver.py

</div>

```python
import socket

localIP = "127.0.0.1"
localPort = 9997
buffer = 1024

serverSocket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
serverSocket.bind((localIP, localPort)) #menyalakan server UDP

print("Server Up")

#listening
while(True):
    data = serverSocket.recvfrom(buffer)
    pesan = data[0]
    ip_addr = data[1]

    print("Pesan dari client: \"{}\"".format(pesan))
    print("IP client: \"{}\"".format(ip_addr))

    serverSocket.sendto(b"Selamat datang di UDP server", ip_addr)
```

<div align="center">

#### Code udpsocket.py

</div>

```python
import socket

target_host = "127.0.0.1"
target_port = 9997

client = socket.socket(socket.AF_INET, socket.SOCK_DGRAM) #UDP

client.sendto(b"Tes aja kok", (target_host, target_port))

data, addr = client.recvfrom(4096)
print("Pesan dari server: \"{}\"".format(data.decode()))

client.close()
```

<div align="center">

#### Output Praktikum 1

</div>

*(Output akan ditampilkan di sini)*

---

### Praktikum 2: Uji Koneksi dengan Eksekusi pada Linux Menggunakan TCP

<div align="center">

#### Code Client

</div>

```python
import socket

if __name__ == "__main__":
    ip = "127.0.0.1"
    port = 4444

    server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    server.connect((ip, port))

    string = input("Enter string: ")
    server.send(bytes(string, "utf-8"))
    buffer = server.recv(1024)
    buffer = buffer.decode("utf-8")
    print(f"Server : {buffer}")
```

<div align="center">

#### Code Server

</div>

```python
import socket

if __name__ == "__main__":
    ip = "127.0.0.1"
    port = 4444

    server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    server.bind((ip, port))
    server.listen(5)

    while True:
        client, address = server.accept()
        print(f"Connection Established - {address[0]}:{address[1]}")

        string = client.recv(1024)
        string = string.decode("utf-8")
        string = string.upper()
        client.send(bytes(string, "utf-8"))
        client.close()
```

<div align="center">

#### Output Praktikum 2

</div>

*(Output akan ditampilkan di sini)*

---

## Hasil dan Kesimpulan

Pada pic sebelumnya output memang sederhana yaitu :

* Pernyataan koneksi berhasil "Connection Established" pada UTP method dan "Selamat datang di UDP server" pada UDP method.
* IP tujuan "127.0.0.1" (local host)
* Serta port yang merespon (Port 41221, 48123, 51822, dan lain-lain)

> ⚠️ Tapi apa hubungannya ini dengan Hacking didalam networking? **PORT adalah "MENU MANIS INCARAN para CRACKER".**

Akses ke port merupakan izin langsung atas layanan yang disediain oleh port tersebut.

Sebagai contoh akan dijelaskan dengan lebih rinci oleh LLM berikut :

---

### 1. Menu: Port 3389 (RDP - Remote Desktop Protocol)

**Layanan yang Disediakan:** Mengendalikan komputer Windows dari jarak jauh (melihat desktop, menggerakkan mouse, mengetik).

**Mengapa Ini "Menu Manis"?** Ini adalah *jackpot*. Jika *cracker* berhasil masuk, dia *mengambil alih seluruh server* seolah-olah dia duduk di depannya.

**Cara "Memakannya" (Metode Hack):**

* **Brute Force Attack:** *Cracker* menemukan Port 3389 terbuka. Dia kemudian menjalankan program yang mencoba login ke akun Administrator dengan 50.000 *password* yang paling umum (seperti 123456, password, Admin@123).

* **Membeli Kredensial:** *Cracker* membeli *username* dan *password* server ini di *dark web* (yang mungkin dicuri dari serangan *phishing*). Dia lalu "login" secara normal lewat Port 3389.

* **Exploit (cth: BlueKeep):** Ini yang paling parah. Jika layanan RDP di server itu *belum di-update*, *cracker* bisa mengirimkan satu paket data ajaib (disebut *exploit*) ke Port 3389. Tanpa perlu *password*, paket itu *menipu* layanan RDP untuk memberikan akses *full control* (Administrator) kepada si *cracker*.

---

### 2️. Menu: Port 21 (FTP - File Transfer Protocol)

**Layanan yang Disediakan:** Mengunggah (upload) dan mengunduh (download) file.

**Mengapa Ini "Menu Manis"?** Seringkali digunakan untuk mengelola file website. Jika *cracker* bisa masuk, dia bisa mengubah tampilan website (defacement) atau menanam *malware*.

**Cara "Memakannya" (Metode Hack):**

* **Anonymous Login:** *Cracker* *scan* dan menemukan Port 21 terbuka. Dia mencoba "trik" lama: login dengan *username* `anonymous` dan *password* `anonymous` (atau email acak). Jika admin server salah konfigurasi, login ini berhasil!

* **Web Shell:** Jika *cracker* berhasil masuk (misal lewat *anonymous login* yang mengizinkan *upload*), dia akan mengunggah satu file kecil bernama `shell.php`. Ini adalah "pintu belakang". Setelah itu, dia bisa menjalankan perintah apa pun di server hanya dengan membuka file itu di browser.

---

### 3️. Menu: Port 80 (HTTP) atau 443 (HTTPS)

**Layanan yang Disediakan:** Website atau Aplikasi Web.

**Mengapa Ini "Menu Manis"?** Port ini *harus* terbuka agar orang bisa mengunjungi website. Ini berarti *hack*-nya tidak pada port, tapi pada *aplikasi* yang berjalan di baliknya.

**Cara "Memakannya" (Metode Hack):**

* **SQL Injection:** *Cracker* membuka website (mengakses Port 443). Di halaman login, pada kolom *username*, dia tidak mengetik nama, tapi mengetik kode: `' OR 1=1; --`

  Jika aplikasi web-nya rentan, *database* akan tertipu dan berpikir "Perintah ini benar!" dan memberikan *izin login penuh* tanpa perlu *password*. *Cracker* baru saja "memakan" seluruh *database* pengguna lewat "menu" website.

---

## 📝 Kesimpulan Akhir

Secara sederhana PORT adalah salah 1 dari menu incaran para cracker dan inilah yang paling sering di execute oleh mereka.

Namun sebenarnya juga jika semua port Tangguh dan secure, port bukanlah satu-satunya menu incarannya tetapi juga ada Router itu sendiri serta yang pasti adalah manusia itu sendiri sebagai objek yang sejatinya paling rentan dan rapuh.

> 🔔 Penting untuk aware and careful terhadap segala yang kita lihat di jagat maya.

Data pribadi sejatinya tidaklah berharga akan tetapi jika vulnerability bisa di execute oleh pihak ke-3 yang jahat maka benefit yang mereka dapatkan bukanlah main-main dan bagi korban yang ia dapatkan hanyalah kerugian dan kehilangan.

---

<div align="center">

**🖋️ Catatan:** Dokumen ini merupakan laporan praktikum Transport Layer pada OSI Model yang mencakup implementasi UDP dan TCP menggunakan Python serta analisis keamanan port dalam konteks network security.

</div>
