# Logbook Mingguan FPGA-A

**Periode:** 28 September 2026 - 2 Oktober 2026  
**Proyek:** `core_fpga-a`  
**Fokus:** Implementasi, integrasi, dan verifikasi jalur event FPGA-A  

## Ringkasan Mingguan

Dalam periode ini, pekerjaan berfokus pada pembangunan fondasi FPGA-A untuk
menerima event eksternal, menghitung event, menyediakan register yang dapat
dibaca CPU, mendefinisikan kontrak antarmuka, dan menyiapkan bukti verifikasi.

Hasil utama minggu ini:

- Membuat definisi antarmuka dan menandai bagian yang belum terverifikasi.
- Membuat skeleton `fpga_a_top` serta model simulasi clock wizard untuk Icarus.
- Membuat dan menguji penangkap pulsa asynchronous.
- Membuat event counter dan register bank FPGA-A.
- Mengintegrasikan jalur event dari `btn1_i` sampai register yang dapat dibaca CPU.
- Membuat paket bukti Day-0 berisi log, waveform, hash sumber, dan image simulasi.
- Mendefinisikan register FPGA-A, status valid/stale/error, dan kontrak firmware.
- Membuat coverage test A2 untuk single event, burst, asynchronous edge, duplicate,
  missing event, reset saat aktivitas, dan wrap counter.
- Menyusun laporan preboard verification FPGA-A untuk T-01 sampai T-10.

## Log Harian

### Fondasi FPGA-A dan Integrasi Awal

#### Pekerjaan

1. Membuat dokumen `interfaces.md` yang mendefinisikan:
   - port board-level;
   - pemetaan tombol dan LED;
   - clock dan reset;
   - antarmuka UART;
   - GPIO;
   - SRAM;
   - AXI-Lite;
   - antarmuka CPU;
   - register FPGA-A.
2. Menandai kontrak yang belum memiliki bukti hardware atau timing sebagai
   `TBD/UNVERIFIED`.
3. Membuat skeleton top-level `fpga_a_top` karena sumber IP `clk_wiz_0` dari
   Vivado tidak tersedia untuk elaborasi Icarus secara langsung.
4. Membuat `clk_wiz_0_sim.v` sebagai model clock wizard khusus simulasi.
5. Membuat `async_pulse_catcher.v` untuk mengubah rising edge asynchronous
   menjadi pulsa satu siklus pada domain clock sistem.
6. Membuat `event_counter.v` untuk menghitung pulsa event.
7. Membuat `fpga_a_register_bank.v` untuk menyediakan register ID, version,
   status, event count, dan error.
8. Mengintegrasikan jalur aktif:

   ```text
   btn1_i
     -> async_pulse_catcher
     -> event_counter
     -> fpga_a_register_bank
     -> register CPU-readable
   ```

9. Menjelaskan batas tanggung jawab jalur event terhadap core CPU: jalur ini
   terutama berada di sisi periferal dan tidak mengubah logika eksekusi
   instruksi core.
10. Membuat paket bukti Day-0 di `evidence/day0`.

#### Hasil dan Verifikasi

- Pengujian register event mencakup jumlah event `1`, `10`, dan `100`.
- Pembacaan register ID, version, status, event count, dan error berhasil.
- Jalur integrasi satu event dari tombol sampai register CPU berhasil.
- Reset register GPIO output, result, UART RX, dan UART status terverifikasi.
- Paket Day-0 menyimpan:
  - log simulasi;
  - ringkasan pass/fail;
  - waveform VCD;
  - image simulasi VVP;
  - hash sumber.

#### Artefak

- [interfaces.md](interfaces.md)
- [fpga_a_top.v](fpga_a_top.v)
- [clk_wiz_0_sim.v](clk_wiz_0_sim.v)
- [async_pulse_catcher.v](async_pulse_catcher.v)
- [event_counter.v](event_counter.v)
- [fpga_a_register_bank.v](fpga_a_register_bank.v)
- [tb_a0_event_registers.v](tb_a0_event_registers.v)
- [tb_fpga_a_integration.v](tb_fpga_a_integration.v)
- [evidence/day0/README.md](evidence/day0/README.md)
- [evidence/day0/day0_sim.log](evidence/day0/day0_sim.log)
- [evidence/day0/day0_integration.vcd](evidence/day0/day0_integration.vcd)
- [evidence/day0/source_hashes.sha256](evidence/day0/source_hashes.sha256)

### 29 September 2026 - Kontrak Register dan Coverage Event

#### Pekerjaan

1. Menetapkan nama register dan semantik akses FPGA-A:
   - ID;
   - VERSION;
   - STATUS;
   - EVENT_COUNT;
   - ERROR.
2. Mendokumentasikan lebar register, alamat, akses read-only, nilai reset,
   dan arti setiap status.
3. Mendefinisikan status `UNKNOWN`, `VALID`, `STALE`, dan `ERROR`.
4. Membuat coverage test A2 di `tb_a2_event_coverage.v`.
5. Menambahkan skenario:
   - satu event;
   - 1000 event;
   - asynchronous edge dengan fase berbeda;
   - burst event;
   - duplicate atau held-high stimulus;
   - missing event melalui toggle-cancel;
   - reset saat aktivitas;
   - counter wrap dengan lebar 8 bit.

#### Hasil dan Verifikasi

- Single event berhasil dihitung.
- 1000 event berhasil dihitung tanpa kehilangan atau duplikasi.
- Burst 16 event berhasil dihitung satu kali per event.
- Tiga asynchronous edge pada fase berbeda berhasil ditangkap satu kali.
- Held-high input hanya menghasilkan satu rising-edge capture.
- Dua edge sebelum sampling menghasilkan konsekuensi toggle-cancel yang telah
  didefinisikan, yaitu tidak ada capture yang diamati.
- Reset saat aktivitas membuang event yang terjadi saat reset dan sistem pulih
  untuk menghitung event setelah reset.
- Counter 8-bit berhasil membungkus dari 255 kembali ke 0.

#### Artefak

- [interfaces.md](interfaces.md)
- [tb_a2_event_coverage.v](tb_a2_event_coverage.v)
- [a2_sim.vvp](a2_sim.vvp)
- [a2_event_coverage.vcd](a2_event_coverage.vcd)

### Kontrak Firmware dan Register FPGA-A

#### Pekerjaan

1. Menstabilkan model MMIO/register FPGA-A untuk kebutuhan firmware.
2. Membuat header firmware canonical `fpga_a_regs.h`.
3. Mendefinisikan alamat register, versi, status, dan kode error.
4. Menambahkan nama konstanta status pada register bank Verilog agar kontrak
   firmware lebih jelas.
5. Memverifikasi pembacaan identity, version, status, event count, serta
   pembedaan status valid, stale, dan error.
6. Mendokumentasikan bahwa klaim interrupt belum diimplementasikan.

#### Hasil dan Verifikasi

- Firmware dapat membaca ID, version, status, dan event count dari model MMIO.
- Status valid, stale, error, dan unknown dapat dibedakan.
- Error code dapat dibaca saat status berada pada kondisi error.
- Batasan interrupt dicatat sebagai belum terverifikasi atau belum tersedia.

#### Artefak

- [fpga_a_regs.h](fpga_a_regs.h)
- [fpga_a_register_bank.v](fpga_a_register_bank.v)
- [tb_fw1_mmio_contract.v](tb_fw1_mmio_contract.v)
- [A3_FIRMWARE_CONTRACT_WALKTHROUGH.md](A3_FIRMWARE_CONTRACT_WALKTHROUGH.md)
- [HDS_PI_INTERFACE_CONTRACT_v0_1.md](HDS_PI_INTERFACE_CONTRACT_v0_1.md)

### Laporan Preboard FPGA-A

#### Pekerjaan

1. Membatasi verifikasi hanya pada FPGA-A sesuai kebutuhan.
2. Menjalankan kembali image simulasi FPGA-A:
   - A2 event coverage;
   - A0 event/register test;
   - integrasi top-level FPGA-A.
3. Memastikan hasil simulasi menunjukkan pass untuk coverage event dan register.
4. Menyusun laporan `FPGA_PREBOARD_VERIFICATION_REPORT_2026-10-02.md`.
5. Mengisi laporan untuk T-01 sampai T-10 dengan stimulus, expected, actual,
   PASS/FAIL, dan evidence path.
6. Memvalidasi bahwa seluruh evidence path yang dicantumkan di laporan ada.

#### Hasil dan Verifikasi

- A2 event coverage menghasilkan:

  ```text
  A2 EVENT COVERAGE PASSED: single, 1000, burst, async phase, duplicate, missing, reset, wrap
  ```

- A0 register test menghasilkan:

  ```text
  A0.5 EVENT COUNTS PASSED: 1, 10, 100
  A0.6 REGISTER READS PASSED: ID, VERSION, STATUS, EVENT_COUNT, ERROR
  ```

- Integrasi top-level menghasilkan:

  ```text
  INTEGRATED EVENT/REGISTER PATH PASSED
  COMPILE_EXIT=0
  RUN_EXIT=0
  ```

- Validasi laporan menghasilkan:

  ```text
  REPORT_VALIDATION_PASS
  EVIDENCE_LINKS_CHECKED=13
  ```

#### Catatan Batasan

- T-08 membuktikan perilaku wrap melalui counter terparameterisasi 8 bit dan
  kontrak counter FPGA-A 32 bit. Simulasi penuh hingga `2^32` event tidak
  dijalankan karena tidak praktis.
- T-10 membuktikan respons default `0` untuk alamat register tidak dikenal.
  Ini bukan pengujian protokol bus malformed transaction tingkat AXI-Lite.
- Bukti yang tersedia adalah simulasi RTL/Icarus, bukan bukti board fisik,
  timing Vivado, atau pengukuran listrik pada FPGA.

#### Artefak

- [FPGA_PREBOARD_VERIFICATION_REPORT_2026-10-02.md](FPGA_PREBOARD_VERIFICATION_REPORT_2026-10-02.md)
- [a2_sim.vvp](a2_sim.vvp)
- [a0_event_registers_check.vvp](a0_event_registers_check.vvp)
- [a0_fpga_top_check.vvp](a0_fpga_top_check.vvp)
- [evidence/day0/day0_sim.log](evidence/day0/day0_sim.log)

## Status Akhir Minggu

**Status:** Selesai dalam lingkup verifikasi RTL FPGA-A.

Fondasi event FPGA-A, register CPU-readable, kontrak firmware, coverage test,
paket evidence Day-0, dan laporan preboard telah tersedia. Seluruh T-01 sampai
T-10 memiliki hasil PASS dalam lingkup bukti yang dicatat, dengan keterbatasan
T-08 dan T-10 yang telah dijelaskan secara eksplisit.

## Pekerjaan Lanjutan yang Masih Terbuka

- Verifikasi clock 100 MHz ke 25 MHz menggunakan konfigurasi Vivado yang
  sebenarnya.
- Verifikasi timing dan generated-clock report.
- Verifikasi reset release pada hardware.
- Verifikasi electrical UART dan pin constraint board.
- Verifikasi button debounce.
- Verifikasi polaritas LED.
- Pengujian malformed transaction pada protokol bus jika antarmuka tersebut
  akan diaktifkan.
- Pengujian overflow penuh atau metode pembuktian formal untuk counter 32 bit.
