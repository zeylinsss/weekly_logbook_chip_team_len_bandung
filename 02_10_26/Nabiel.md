# Logbook Mingguan MagangHub

**Periode:** 28 September 2026 - 2 Oktober 2026  
**Program:** Firmware  
**Fokus:** Pembelajaran firmware, persiapan ujian, dan pengantar desain analog  

## Ringkasan Mingguan

Pada periode ini, kegiatan berfokus pada pendalaman firmware menggunakan buku
dari mentor (Bab 0-9), latihan dan ujian untuk mengukur pemahaman, serta
pengenalan awal desain firmware dan analog.

Hasil utama minggu ini:

- Menyelesaikan pembelajaran firmware Bab 0 sampai Bab 9.
- Mengerjakan tugas tambahan sebagai persiapan tes hari Rabu.
- Memahami struktur section ELF, perbedaan VMA/LMA, dan dasar debugging firmware.
- Mengikuti ujian firmware (4 dari 15 soal terjawab) sebagai bahan evaluasi.
- Mempelajari alur desain firmware dari penentuan kebutuhan hingga spesifikasi.
- Mendapat hands-on firmware dan pengantar analog dalam sesi mentoring.

## Log Harian

### 28 September 2026 (Day 6) - Firmware Bab 4 dan 5

#### Pekerjaan

1. Melanjutkan pembelajaran firmware, yaitu Bab 4 dan Bab 5.
2. Mengerjakan tugas tambahan dari mentor sebagai latihan untuk tes hari Rabu.

#### Pembelajaran

- Bab 0: perbedaan fungsionalitas pin dengan konvensi penamaan.
- Bab 1: kondisi FPGA saat baru dinyalakan, terkait reset.
- Bab 2: perbedaan peran ISA dan platform.
- Bab 3: kompilasi kode C menjadi assembly dengan tools open source beserta
  optimasi compiler.
- Bab 4: konsep register, stack, dan ABI.
- Bab 5: address contract dari memori.

### 29 September 2026 (Day 7) - Firmware Bab 6-9

#### Pekerjaan

1. Mengerjakan Bab 6 sampai Bab 9 dari buku firmware.
2. Mereview materi untuk persiapan ujian Day 8.

#### Pembelajaran

- Empat section ELF utama: `.text`, `.rodata`, `.data`, dan `.bss`, yang memiliki
  penempatan memori berbeda.
- `.text` dan `.rodata` ditempatkan di ROM/Flash, sedangkan `.data` (variabel
  global) ditempatkan di RAM saat dieksekusi.
- Perbedaan VMA dan LMA.
- Cara debugging ketika terjadi masalah pada firmware.

### 30 September 2026 (Day 8) - Review Materi dan Ujian Firmware

#### Pekerjaan

1. Mereview materi firmware dari Bab 0 sampai Bab 9.
2. Mengerjakan soal ujian dari mentor.

#### Hasil

- Ujian: 4 dari 15 soal berhasil dikerjakan. Hasil ini menjadi bahan evaluasi
  materi yang perlu diperdalam.

#### Pembelajaran

- Proses mendesain firmware dari awal (penentuan kebutuhan) hingga penentuan
  spesifikasi firmware.
- Proses ini menjadi bekal untuk mendesain firmware mulai minggu depan.

### 1 Oktober 2026 (Day 9) - Tugas Firmware dan Hands-on

#### Pekerjaan

1. Mengerjakan tugas firmware dari mentor.
2. Eksplorasi mandiri secara hands-on dalam pembuatan firmware.

#### Pembelajaran

- Proses pembuatan firmware.
- Pengalaman langsung (hands-on) dalam pembuatan firmware.

### 2 Oktober 2026 (Day 10) - Tugas Firmware dan Mentoring Analog

#### Pekerjaan

1. Menyelesaikan ujian firmware hingga 15 soal.
2. Mengikuti mentoring yang membahas progress tugas dan pengantar analog.

#### Pembelajaran

- Lanjutan proses desain firmware.
- Intuisi dasar desain analog dari sesi pengantar.

## Status Akhir Minggu

**Status:** Selesai sesuai target pembelajaran minggu ini.

Materi firmware Bab 0-9 telah dipelajari dan diuji.

## Rencana Minggu Depan

- Membagi role firmware menjadi tiga role
- Mulai mendesain firmware berdasarkan alur kebutuhan hingga spesifikasi.
- Mengulang materi yang belum dikuasai berdasarkan hasil ujian.
- Melanjutkan pembelajaran analog.
