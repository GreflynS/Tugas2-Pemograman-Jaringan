# Tugas2-Pemograman-Jaringan

Greflyn Felinstya Salhuteru
IF.0201 / 1203220024
------------------------------
# Soal
Buat sebuah program file transfer protocol menggunakan socket programming dengan beberapa perintah dari client seperti berikut :
ls : ketika client menginputkan command tersebut, maka server akan memberikan daftar file dan folder. 
rm {nama file} : ketika client menginputkan command tersebut, maka server akan menghapus file dengan acuan nama file yang diberikan pada parameter pertama.
download {nama file} : ketika client menginputkan command tersebut, maka server akan memberikan file dengan acuan nama file yang diberikan pada parameter pertama.
upload {nama file} : ketika client menginputkan command tersebut, maka server akan menerima dan menyimpan file dengan acuan nama file yang diberikan pada parameter pertama.
size {nama file} : ketika client menginputkan command tersebut, maka server akan memberikan informasi file dalam satuan.
MB (Mega bytes) dengan acuan nama file yang diberikan pada parameter pertama byebye : ketika client menginputkan command tersebut, maka hubungan socket client akan diputus.
connme : ketika client menginputkan command tersebut, maka hubungan socket client akan terhubung.

- buat readme.md dengan memberikan Nama dan nim serta penjelasan dan cara menggunakan setiap command yang tersedia penilaian 50% : program 50% : readme

- Upload di github dan kumpulkan url repositorinya
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
# Hasil

**1. Command Ls**
   
   ![Screenshot 2024-04-10 150757](https://github.com/GreflynS/Tugas2-Pemograman-Jaringan/assets/163794459/8d0e6003-af0a-406a-a028-702e29ac1de2)
   
Fungsi tersebut memiliki tujuan untuk mengirim daftar file dan folder dalam sebuah direktori kepada koneksi socket yang diberikan. Berikut adalah penjelasanmya :

`def ls(conn, directory='.'):`
   - Fungsi `ls` menerima dua argumen. Argumen pertama adalah `conn`. Argumen kedua adalah `directory`, yang merupakan direktori yang akan di-list. Argumen ini bersifat opsional, dengan nilai default `'.'`, yang artinya direktori saat ini.
`parser = argparse.ArgumentParser(description='List files in a directory')`
   - Membuat objek parser untuk mengurai argumen baris perintah. Ini adalah bagian dari modul `argparse` yang digunakan untuk memproses argumen baris perintah secara sistematis.
`parser.add_argument('directory', type=str, nargs='?', default='.')`
   - Menambahkan argumen `directory` ke parser. Argumen ini adalah argumen opsional yang mengizinkan kita untuk menentukan direktori yang akan di-list. `type=str` menunjukkan bahwa argumen ini harus berupa string. `nargs='?'` menunjukkan bahwa argumen ini adalah opsional. `default='.'` menunjukkan bahwa jika argumen ini tidak disediakan, nilai defaultnya adalah direktori saat ini (`'.'`).
`args = parser.parse_args()`
   - Mengurai argumen baris perintah yang diberikan kepada fungsi `ls` dan menyimpan hasilnya dalam objek `args`. Objek ini akan berisi nilai-nilai yang diberikan untuk setiap argumen yang didefinisikan sebelumnya pada objek parser.
`files = os.listdir(args.directory)`
   - Mengambil daftar file dan folder dalam direktori yang ditentukan oleh argumen `directory`. Nilai `args.directory` adalah nilai yang diberikan oleh pengguna atau nilai default `'.'` jika tidak ada nilai yang diberikan.
`files_str = '\n'.join(files)`
   - Menggabungkan daftar file dan folder menjadi satu string dengan pemisah baris baru (`'\n'`).
`conn.sendall(files_str.encode('utf-8'))`
   - Mengirim string yang berisi daftar file dan folder ke koneksi socket yang diberikan. Sebelum mengirim, string dikodekan ke format byte menggunakan UTF-8 agar dapat dikirim melalui koneksi socket. Metode `sendall` digunakan untuk memastikan bahwa semua data dikirimkan.

   - Output yang di dapat :  
     ![Screenshot 2024-04-10 150155](https://github.com/GreflynS/Tugas2-Pemograman-Jaringan/assets/163794459/e224aebb-3639-48ce-a9ef-94016b33fb5b)

**2. Command rm**

![image](https://github.com/GreflynS/Tugas2-Pemograman-Jaringan/assets/163794459/99ac0d01-1fa3-4fa1-b61d-da092851f8e1)

Fungsi `remove_file` digunakan untuk menghapus file dengan nama yang diberikan. 
Pada awalnya, fungsi memeriksa apakah file tersebut ada dalam sistem file dengan menggunakan `os.path.exists()`. 
Jika file tersebut ada, maka fungsi `os.remove()` akan digunakan untuk menghapusnya, diikuti dengan mengembalikan pesan yang menyatakan bahwa file telah dihapus. 
Jika file tidak ditemukan, fungsi akan mengembalikan pesan yang menyatakan bahwa file tersebut tidak ada dalam sistem file.

Outputnya:

![Screenshot 2024-04-10 150206](https://github.com/GreflynS/Tugas2-Pemograman-Jaringan/assets/163794459/79ddf201-339f-4f04-b814-c4047409e325)


**3. Command Upload**

![image](https://github.com/GreflynS/Tugas2-Pemograman-Jaringan/assets/163794459/3cf2c297-1e45-4ba8-b3e5-cb2cbeaadc30)

Fungsi `upload` bertanggung jawab untuk menerima dan menyimpan file yang dikirimkan melalui koneksi socket ke dalam sistem file. Pertama, fungsi membuka file dengan mode write-binary (`'wb'`) menggunakan perintah `open`. Selanjutnya, data diterima dari koneksi socket menggunakan `conn.recv(1024)`, yang berarti menerima data dalam potongan sebesar 1024 byte. Proses penerimaan data dilakukan dalam loop `while` untuk memastikan semua data file diterima dengan lengkap. Setelah data diterima, data tersebut ditulis ke file yang dibuka menggunakan `f.write(data)`. Loop akan berakhir ketika tidak ada data yang diterima lagi. Terakhir, fungsi akan mencetak pesan yang menyatakan bahwa file telah berhasil diunggah ke sistem file.

Outputnya :

![Screenshot 2024-04-10 150320](https://github.com/GreflynS/Tugas2-Pemograman-Jaringan/assets/163794459/5357ce95-eb87-47a2-9a18-e081ab65be12)

![Screenshot 2024-04-10 150327](https://github.com/GreflynS/Tugas2-Pemograman-Jaringan/assets/163794459/10d601d1-8cfc-43ef-bad0-a479cbad0321)

**4. Command Download**

![image](https://github.com/GreflynS/Tugas2-Pemograman-Jaringan/assets/163794459/8e02ee47-8ddf-4645-821c-4af20dcbfb23)

Fungsi `download` bertugas untuk mengirim file kepada klien melalui koneksi socket jika file dengan nama yang diberikan ada dalam sistem file. Pertama, fungsi memeriksa apakah file tersebut ada dalam sistem file menggunakan `os.path.exists()`. Jika file tersebut ada, maka file akan dibuka dengan mode read-binary (`'rb'`) menggunakan perintah `open`. Selanjutnya, data dari file dibaca dalam potongan sebesar 1024 byte menggunakan `f.read(1024)`, dan kemudian dikirimkan melalui koneksi socket ke klien menggunakan `conn.sendall(data)`. Proses ini dilakukan dalam loop `while` untuk memastikan semua data file terkirim. Setelah semua data terkirim, fungsi akan mengembalikan pesan yang menyatakan bahwa file berhasil diunduh. Jika file tidak ditemukan, fungsi akan mengembalikan pesan yang menyatakan bahwa file tidak ada dalam sistem file.

Output : 
![Screenshot 2024-04-10 150245](https://github.com/GreflynS/Tugas2-Pemograman-Jaringan/assets/163794459/4ba92965-707d-4532-b324-f5a9b6d7ed8b)

![Screenshot 2024-04-10 150314](https://github.com/GreflynS/Tugas2-Pemograman-Jaringan/assets/163794459/ffc2e974-d584-4009-826f-96e64cfb74ad)

**5. Command Size**

![image](https://github.com/GreflynS/Tugas2-Pemograman-Jaringan/assets/163794459/8e73fbc4-b61c-4ba4-bbef-a8ce015706d8)

Fungsi `get_file_size` bertujuan untuk mendapatkan ukuran file dengan nama yang diberikan. Pertama, fungsi memeriksa apakah file tersebut ada dalam sistem file menggunakan `os.path.exists()`. Jika file tersebut ada, ukuran file dalam byte diambil menggunakan `os.path.getsize(filename)`. Ukuran file tersebut kemudian dikonversi menjadi string dan dikembalikan. Jika file tidak ditemukan, fungsi akan mengembalikan pesan yang menyatakan bahwa file tidak ada dalam sistem file.

Output :

![Screenshot 2024-04-10 150435](https://github.com/GreflynS/Tugas2-Pemograman-Jaringan/assets/163794459/11e8a83f-c3f0-4184-87e4-e9e659420bad)

**6.Command Byebye**

![image](https://github.com/GreflynS/Tugas2-Pemograman-Jaringan/assets/163794459/6345dfa5-b05f-4c5f-98b0-0f2ab4df5ec8)

Jika perintah yang diterima dari klien adalah 'byebye', maka server akan mengirimkan pesan "Goodbye!" kepada klien menggunakan conn.sendall(response.encode('utf-8')). Setelah itu, koneksi socket (conn) akan ditutup menggunakan conn.close(), yang mengakhiri hubungan antara server dan klien. Dengan kata lain, jika klien mengirimkan perintah 'byebye', server akan mengirimkan pesan perpisahan kepada klien dan kemudian menutup koneksi.

Output :

![Screenshot 2024-04-10 150448](https://github.com/GreflynS/Tugas2-Pemograman-Jaringan/assets/163794459/44520193-e6d0-46cc-93a8-764fc1403e36)

**7. Command Connme**

![image](https://github.com/GreflynS/Tugas2-Pemograman-Jaringan/assets/163794459/369820ac-15ea-45fe-a4d9-7f486a187c6e)

Jika perintah yang diterima adalah 'connme', server akan mengirimkan pesan "Connection established successfully." kepada klien menggunakan conn.sendall(response.encode('utf-8')), menandakan bahwa koneksi berhasil dibuat. Setelah itu, perulangan akan dilanjutkan ke iterasi selanjutnya menggunakan continue, sehingga server dapat terus menerima perintah-perintah berikutnya dari klien. Jika perintah yang diterima tidak valid (tidak cocok dengan perintah yang didefinisikan), server akan mengirimkan pesan "Invalid command." kepada klien menggunakan conn.sendall(response.encode('utf-8')).

Output :

![Screenshot 2024-04-10 150646](https://github.com/GreflynS/Tugas2-Pemograman-Jaringan/assets/163794459/4412ddc4-6802-45cf-9b52-76661f8a7eeb)

# Sekian <3
