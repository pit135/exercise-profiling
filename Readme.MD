PERFORMANCE TESTING
![All Student Name](all_student_name.png)
![All Student Request](all_student_req.png)
![Highest GPA](highest_gpa.png)

Profiling
(SEBELUM)
![Profiling before](profiling.png)
![Profiling before](profiling2.png)
![Profiling before](profiling3.png)
![Profiling before](profiling4.png)
(SESUDAH)
![Profiling after](profilingaf.png)
![Profiling after](profilingaf2.png)
![Profiling after](profilingaf3.png)
![Profiling after](profilingaf4.png)

PENJELASAN PERUBAHAN
Perbedaan hasil di JMeter itu keliatan banget karena saya udah ngerubah cara aplikasi ngambil data dari database. 
Awalnya, aplikasi itu kena masalah N+1 Query. Jadi, setiap kali ngehit endpoint /all-student, si aplikasi ini bolak-balik ke database berkali-kali cuma buat nyari data kursus 
tiap mahasiswa satu-satu. Di JMeter, itu efeknya, angka average Response Timenya jadi tinggi (lemot) dan Throughputnya kecil karena server sibuk banget nungguin antrean query ke database yang nggak habis-habis. 
Nah, setelah optimasi pake join fetch, cara kerjanya aku ubah total, yaitu paksa aplikasi buat ngambil semua data mahasiswa dan kursusnya dalam satu kali query aja. 
Ibaratnya, kalau dulu belanja ke pasar bolak-balik seratus kali buat beli seratus barang, sekarang sekali jalan langsung bawa truk. Hasilnya di JMeter langsung keliatan, Response Timenya turun drastis 
(jauh lebih cepat dari target minimal 20%) karena nggak ada lagi waktu yang kebuang buat nunggu koneksi database berulang-ulang. Selain itu, aplikasinya jadi lebih kuat nahan beban banyak user (Throughput naik) 
karena kerja CPU dan I/O-nya sekarang jauh lebih efisien dan enteng.

REFLECTION
1. Perbedaan JMeter vs IntelliJ Profiler
   JMeter itu dipakai buat performance testing dari luar aplikasi, misalnya ngetest berapa banyak request yang bisa ditangani server, response time, dll. Fokusnya ke behavior aplikasi secara keseluruhan.
   Sedangkan IntelliJ Profiler dipakai dari dalam aplikasi (codelevel), buat liat bagian mana yang makan CPU besar, memory, atau lambat. Jadi lebih ke ngulik isi dalam kode.

2. Peran profiling dalam cari weak point
   Profiling bantu banget karena kita bisa lihat method mana yang paling sering dipanggil, paling lama jalan, atau paling boros resource. Dari situ keliatan bagian mana yang jadi bottleneck, jadi nggak nebak-nebak lagi.

3. Efektivitas IntelliJ Profiler
   Menurutku efektif, apalagi buat ngerti masalah di level kode. Visualisasi seperti call tree atau flame graph bikin kita cepat lihat bagian yang bermasalah tanpa harus cek satu-satu.

4. Tantangan & cara ngatasinnya
   Tantangannya biasanya:
   * data hasil test banyak dan kadang bingung bacanya
   * hasil bisa beda-beda tiap run
   * susah reproduksi kondisi real user
   Cara ngatasinnya:
   * fokus ke metrik penting aja (CPU, memory, response time)
   * jalankan test beberapa kali biar lebih konsisten
   * coba simulasi kondisi yang mendekati real (load, data, dll)

5. Manfaat IntelliJ Profiler
   * bisa tau bagian kode yang lambat secara detail
   * bantu optimasi lebih tepat sasaran
   * hemat waktu debugging karena langsung kelihatan problemnya
   * ada visualisasi yang gampang dipahami

6. Kalau hasil JMeter & Profiler beda
   Ini wajar, karena JMeter lihat dari luar, Profiler dari dalam, jadi paling bisa:
   * bandingin dua-duanya buat cari pola
   * cek lagi skenario testing (apakah sama atau nggak)
   * gunakan keduanya sebagai pelengkap, bukan saling menggantikan

7. Strategi optimasi & menjaga fungsi tetap aman
   Strateginya:
   * optimasi bagian yang paling berat dulu (hotspot)
   * perbaiki algoritma atau query yang nggak efisien
   * kurangi object creation atau looping yang nggak perlu
   biar nggak merusak fungsi:
   * selalu pakai testing (unit test / integration test)
   * bandingin hasil sebelum & sesudah optimasi
   * lakuin perubahan sedikit-sedikit, jangan langsung besar

