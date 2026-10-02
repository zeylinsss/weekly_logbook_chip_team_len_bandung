**Uraian Aktivitas**

Perancangan, implementasi RTL skeleton, standarisasi observation contract, serta self-checking verification subsistem FPGA-B sebagai independent hardware observer untuk persiapan FPGA Smart Meter Pre-Sprint (periode 28 September–2 Oktober 2026). Aktivitas mencakup freeze hierarchy modul observer_top, implementasi independent pulse/event counter dan independent timestamp counter, penetapan format event record, pendefinisian resolusi serta overflow behavior, pembuatan host-output model dan event logger, integrasi self-checking scoreboard, serta eksekusi verification campaign dan divergence detection testing.

**Pembelajaran yang Diperoleh**

FPGA-B bertindak sebagai independent hardware observer yang bertugas memverifikasi integritas operasional serta fungsionalitas DUT (FPGA-A) secara objektif tanpa menduplikasi algoritma internal DUT sebagai fake reference. Subsistem ini bertanggung jawab mencatat event secara independen via common synthetic stimulus, menghasilkan high-precision timestamping, mengelola event-log buffering, mengevaluasi expected sequence dan interval timing pada scoreboard PASS/FAIL, mengidentifikasi failure reason via first-divergence capture, serta menyediakan deterministic host-output model untuk integrasi firmware dan tooling sebelum physical board bring-up. Rincian pembelajaran dan capaian fungsional harian selama satu minggu ini meliputi:

<img width="3636" height="2128" alt="image" src="https://github.com/user-attachments/assets/5640005e-a63e-4630-81ca-fee694702542" />


1. **Senin, 28 September 2026**

    Membuat modul observer_top berhasil di-freeze secara terisolasi dari domain internal DUT. Pembelajaran difokuskan pada implementasi independent timestamp counter dan independent pulse/event counter untuk mencatat transisi stimulus tanpa coupling logika ke DUT. Ditetapkan struktur event record awal (EVENT_ID, TYPE, TIMESTAMP, VALUE, FLAGS) serta basic observer testbench dengan evidence logging (source hash, simulation log, waveform, expected vs actual count) guna memenuhi kriteria day gate.

2. **Selasa, 29 September 2026**

   Struktur field event record dibekukan dengan format baku EVENT_ID, TYPE, TIMESTAMP, VALUE, FLAGS. Pembelajaran mencakup penentuan timestamp resolution, mekanisme penanganan counter overflow behavior, serta penetapan kandidat observation input dan input/output scoreboard. Selain itu, dirumuskan first-divergence record untuk menangkap event ID, waktu, dan failure reason saat terjadi mismatch, yang diselaraskan ke dalam dokumen kontrak bersama FPGA_DUT_OBSERVER_INTERFACE_CONTRACT_v0_1.md.

3. **Rabu, 30 September 2026**

   Verifikasi observer ditingkatkan dari manual waveform inspection menjadi fully self-checking testbench. Pembelajaran mencakup penerapan logika timestamping pada common event, interval calculation/check, expected-sequence checking, event-log buffering model, dan scoreboard PASS/FAIL otomatis. Melalui intentional DUT mismatch/suppression stimulus (kondisi expected = 1000, DUT = 999, observer = 1000), testbench terbukti mampu mendeteksi anomali secara deterministik melalui notifikasi First Divergence = DETECTED.

5. **Kamis, 1 Oktober 2026**

   Format event record distabilkan untuk kebutuhan software/tooling interface melalui pembangunan host-output model. Pembelajaran meliputi penambahan observer health/status monitoring, penyusunan parser expectations untuk host/firmware, serta validasi deterministic export dalam format text/CSV-like. Hal ini memungkinkan pelaksanaan joint log replay dan pengujian software tanpa ketergantungan pada physical FPGA hardware.

7. **Jumat, 2 Oktober 2026**

    Eksekusi menyeluruh verification test campaign (T-01 hingga T-10) untuk memvalidasi observer behavior terhadap reset, one-event, 100-event, burst traffic, asynchronous edge, missing event, duplicate event, counter overflow wrap/saturation, reset during activity, serta invalid/malformed stimulus. Setiap test didokumentasikan secara ketat berbasis evidence (test ID, stimulus profile, expected, actual, PASS/FAIL status, evidence path), memastikan seluruh P0 simulation defects tertutup sebelum memasuki fase physical bring-up.
