#  Commit 1 Reflection notes
- Terdapat parameter steram yang mutable pada handle_connection yang didapatkan dari ownership TcpStream sehingga dapat dimodifikasi di dalam fungsi.
- Terdapat juga buf_reader yaitu variabel yang berisi hasil bacaan menggunakan BufReader dari buffer stream yang di pass.
- Setelah itu, terdapat variabel  yaitu http_request yang mana berisi request dari HTTP.
- Pada buff_reader akan membaca line dengan menggunakan .line(). Fungsi ini mereturn Lines<BufReader<&mut TcpStream>> yang dapat diiterasi.
- Kemudian, iterasi menggunakan .map() dan jika terjadi error, unwrap() akan menyebabkan panic dan menghentikan program.
- Terdapat take_while(!line || !line.is_empty()) yang berfungsi untuk mengambil line-line yang ada sampai bertemu baris kosong dan akhirnya berhenti.
- .collect() akan mengambil semua line yang dihasilkan dan menyimpan elemen dalam bentuk Vector. Kemudian, hasil akan di print.

# Commit 2 Reflection notes
- menambahkan response untuk fungsi handle_connection dari yang sebelumnya print di terminal, sekarang memberikan response.
- Response berbentuk status_line, yaitu "HTTP/1.1 200 OK" yang menandakan proses berhasil.
- Kemudian, membaca isi dari hello.html menjadi string dan menyimpannya di contents. Jika terjadi error, maka unwrap() akan menyebabkan panic dan menghentikan program.
- Terdapat length yang merupakan panjang dari content yang ada.
- Kemudian, format response akan berbentuk sesuai dengan status_line, Content-length, dan contents yang sudah dibuat sebelumnya dalam bentuk HTTP response.
- Terakhir, kita menggunakan stream.write_all() yang akan mengembalikan ke TCP yang dikonversi menjadi bentuk bytes. Jika terjadi error, unwrap() akan menyebabkan panic dan menghentikan program.
![Commit 2](/images/commit2.png)
