# RTOS-EXERCISE 5
## Deskripsi
Latihan ini adalah implementasi praktis dari kontensi sumber daya dalam sistem multitasking, berdasarkan Exercise 5 dari *Real-Time Operating Systems Book 2 – The Practice* oleh Jim Cooling. Program ini mendemonstrasikan masalah akses sumber daya bersama tanpa sinkronisasi, yang menyebabkan interferensi dan kontensi.
#### **Tujuan**
Menyoroti tantangan kontensi sumber daya dalam lingkungan multitasking menggunakan FreeRTOS. Secara khusus:
1. Menunjukkan bagaimana akses sumber daya bersama dapat menyebabkan masalah ketika beberapa tugas berjalan secara bersamaan.
2. Memvisualisasikan dampak kontensi menggunakan LED untuk menunjukkan kondisi normal dan interferensi.
## Pin IOC
![Exercise 6](https://github.com/user-attachments/assets/45d20600-90d9-4e65-8ad7-d3694e4c801b)
##### **1. Tugas dan Fungsionalitas**
- **GreenLEDTask**: Menyalakan/mematikan LED hijau sambil mengakses sumber daya bersama.
- **RedLEDTask**: Menyalakan/mematikan LED merah sambil mengakses sumber daya yang sama.
- **Akses Sumber Daya Bersama**:
  - Disimulasikan dengan fungsi `accessSharedData()`.
  - Menggunakan variabel global `startFlag` untuk mendeteksi kontensi.
  - Interferensi ditunjukkan dengan menyalakan LED biru.

##### **2. Hubungan Antar Task**
- Kedua task, **GreenLEDTask** dan **RedLEDTask**, bekerja secara independen namun berbagi sumber daya yang sama (variabel `startFlag`).
- **GreenLEDTask** memiliki prioritas lebih rendah (`osPriorityIdle`) dibandingkan dengan **RedLEDTask** (`osPriorityNormal`).
- Scheduler FreeRTOS memastikan bahwa tugas dengan prioritas lebih tinggi (RedLEDTask) mendapatkan eksekusi lebih sering.
- Tanpa mekanisme sinkronisasi, kedua task dapat mengakses `accessSharedData()` secara bersamaan, menyebabkan interferensi yang ditunjukkan oleh LED biru.

##### **3. Cara Kerja Program**
1. **Inisialisasi Sistem**:
   - GPIO diatur untuk tiga LED (Hijau, Merah, dan Biru).
   - Scheduler FreeRTOS dimulai, dan kedua task dibuat.

2. **Proses Tugas**:
   - **GreenLEDTask** dan **RedLEDTask** berulang kali memanggil `accessSharedData()`.
   - Fungsi ini memeriksa `startFlag` untuk melihat apakah sumber daya sedang digunakan oleh task lain:
     - Jika `startFlag == 1`, sumber daya diakses, dan `startFlag` diubah menjadi `0`.
     - Jika `startFlag == 0`, kontensi terdeteksi, dan LED biru dinyalakan.

3. **Efek Kontensi**:
   - Kontensi terjadi ketika kedua task mencoba mengakses `accessSharedData()` secara bersamaan. 
   - Waktu akses yang lama (`osDelay(500)`) dalam fungsi tersebut memperbesar kemungkinan interferensi.

4. **Indikasi Visual**:
   - LED hijau dan merah menunjukkan aktivitas masing-masing task.
   - LED biru menyala ketika kontensi sumber daya terjadi.

---

#### **Hasil Program**

- LED Hijau dan Merah menyala bergantian sesuai intervalnya masing-masing (penundaan `500 ms` untuk GreenLEDTask dan `100 ms` untuk RedLEDTask).
- LED Biru menyala jika terjadi kontensi, menunjukkan bahwa kedua task mengakses sumber daya secara bersamaan.

---

#### **Cara Menjalankan**

1. **Pengaturan Perangkat Keras**:
   - Hubungkan LED ke pin GPIO yang sesuai pada papan STM32F4.

2. **Pengaturan Perangkat Lunak**:
   - Hasilkan file proyek menggunakan STM32CubeMX.
   - Impor file ke IDE Anda dan bangun proyek.

3. **Eksekusi**:
   - Flash firmware ke papan STM32F4.
   - Amati LED:
     - LED Hijau dan Merah bergantian menyala/mati.
     - LED Biru menyala jika terjadi kontensi.

---

## Video Demo
https://github.com/user-attachments/assets/bd5e7dba-e2d9-4ffe-ac9a-7c8d452876e4
### Contributor
- Wildan Faizin (3222600011)
- Dhanang Fadhila Trisnandi (3222600015)
