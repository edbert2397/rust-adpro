#  Commit 1 Reflection notes
- Terdapat parameter steram yang mutable pada handle_connection yang didapatkan dari ownership TcpStream sehingga dapat dimodifikasi di dalam fungsi.
- Terdapat juga buf_reader yaitu variabel yang berisi hasil bacaan menggunakan BufReader dari buffer stream yang di pass.
- Setelah itu, terdapat variabel  yaitu http_request yang mana berisi request dari HTTP.
- Pada buff_reader akan membaca line dengan menggunakan .line(). Fungsi ini mereturn Lines<BufReader<&mut TcpStream>> yang dapat diiterasi.
- Kemudian, iterasi menggunakan .map() dan jika terjadi error, unwrap() akan menyebabkan panic dan menghentikan program.
- Terdapat take_while(!line || !line.is_empty()) yang berfungsi untuk mengambil line-line yang ada sampai bertemu baris kosong dan akhirnya berhenti.
- .collect() akan mengambil semua line yang dihasilkan dan menyimpan elemen dalam bentuk Vector. Kemudian, hasil akan di print.