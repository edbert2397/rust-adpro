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

# Commit 3 Reflection notes
- menambahkan error page, 404.html
```
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="utf-8">
        <title>Hello!</title>
    </head>
    <body>
        <h1>Oops!</h1>
        <p>Sorry, I don't know what you're asking for.</p>
    </body>
</html>
```

- mengubah code pada handle_connection sehingga memunculkan 404.html jika page tidak dikenal
```
fn handle_connection(mut stream: TcpStream) {
    let buf_reader = BufReader::new(&mut stream);
    let request_line = buf_reader.lines().next().unwrap().unwrap();

    let (status_line, filename) = if request_line == "GET / HTTP/1.1" {
        ("HTTP/1.1 200 OK", "hello.html")
    } else {
        ("HTTP/1.1 404 NOT FOUND", "404.html")
    };

    let file_content = fs::read_to_string(filename).unwrap();
    let file_length = file_content.len();

    let response = format!("{status_line}\r\nContent-Length: {file_length}\r\n\r\n{file_content}");

    stream.write_all(response.as_bytes()).unwrap();
}
```
- mengecek apakah request nya itu GET request yang valid. Jika iya, maka statusnya adalah 200 OK dan file "hello.html", jika tidak maka statusnya adalah 404 Not Found dan file "404.html"
- Stream.write_all akan menkonversi ke bentuk bytes dan mengembalikan ke TCP. Jika error, maka unwrap() akan panic() dan program terhenti
![Commit 3](/images/commit3.png)

# 4 Commit 4 reflection notes
- edit fungsi handle_connection dengan tambahan code ini
```
let (status_line, filename) = match &request_line[..] {
        "GET / HTTP/1.1" => ("HTTP/1.1 200 OK", "hello.html"), "GET /sleep HTTP/1.1" => {
        thread::sleep(Duration::from_secs(10)); ("HTTP/1.1 200 OK", "hello.html") }
        _ => ("HTTP/1.1 404 NOT FOUND", "404.html"),
    };
```
- Menambahkan delay 10 detik untuk program bekerja kembali sehingga menyebabkan program tidak dapat menangani banyak concurrent request karena program tersebut single threaded
