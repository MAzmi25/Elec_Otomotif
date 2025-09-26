<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Pengetahuan Umum Dasar</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background-color: #f0f0f0;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }

        .quiz-container {
            background-color: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
            width: 100%;
            max-width: 600px;
            text-align: center;
        }

        h1 {
            color: #333;
            margin-bottom: 10px;
        }

        #completion-message {
            color: #28a745;
            font-size: 1.2em;
            font-weight: bold;
            margin-top: 5px;
            margin-bottom: 20px;
        }

        .question-counter-text {
            font-size: 0.9em;
            color: #666;
            margin-bottom: 20px;
        }

        #question-container {
            margin-bottom: 20px;
        }

        #question {
            font-size: 1.5em;
            font-weight: bold;
            margin-bottom: 25px;
            color: #444;
        }

        .btn-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin-bottom: 20px;
        }

        .btn {
            background-color: #007bff;
            color: white;
            border: none;
            padding: 12px 15px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1em;
            transition: background-color 0.2s ease, box-shadow 0.2s ease;
            word-wrap: break-word;
            min-height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            outline: none;
            font-weight: bold;
        }

        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) { background-color: #007bff; }
        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):hover {}
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus:hover {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }

        .btn.correct { background-color: #28a745 !important; box-shadow: none; }
        .btn.correct:hover { background-color: #218838 !important; }
        .btn.correct:focus {
            background-color: #28a745 !important;
            box-shadow: 0 0 0 3px rgba(40, 167, 69, 0.6) !important;
        }

        .btn.wrong { background-color: #dc3545 !important; box-shadow: none; }
        .btn.wrong:hover { background-color: #c82333 !important; }
        .btn.wrong:focus {
            background-color: #dc3545 !important;
            box-shadow: 0 0 0 3px rgba(220, 53, 69, 0.6) !important;
        }

        .btn:disabled {
            cursor: not-allowed;
            opacity: 0.65;
        }
        /* Adjusted to not conflict with new button's disabled state if it's not a skip-btn or answer btn */
        .btn:disabled:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) {
            background-color: #6c757d !important;
            color: #ccc !important;
        }


        .controls {
            display: flex;
            justify-content: center;
            gap: 10px;
        }

        #skip-navigation-controls {
            justify-content: space-between; /* Adjusted to space-around or similar if needed for 3 buttons */
            margin-top: 40px;
            margin-bottom: 10px;
        }

        .skip-btn { /* This style is for prev-50 and next-50 */
            background-color: #28a745; /* Green */
            color: white;
            padding: 8px 12px;
            font-size: 0.9em;
            min-width: 80px; /* Ensures same width for all skip-type buttons */
        }
        .skip-btn:hover {
            background-color: #218838; /* Darker Green */
            color: white;
        }
        .skip-btn:disabled { /* Default disabled for green skip buttons */
            background-color: #a3d8b0 !important;
            color: #e9f5ec !important;
            /* cursor: not-allowed; is inherited from .btn:disabled */
            /* opacity: 0.65; is inherited from .btn:disabled */
        }

        /* New button style for "Previous Question" */
        .btn-prev-q {
            background-color: #5F9EA0; /* CadetBlue - "biru terang" */
            color: white; /* Text color */
            padding: 8px 12px; /* Same padding as skip-btn */
            font-size: 0.9em; /* Same font size as skip-btn */
            min-width: 80px; /* Same min-width as skip-btn */
        }
        .btn-prev-q:hover:not([disabled]) {
            background-color: #4682B4; /* SteelBlue - darker for hover */
            color: white;
        }
        .btn-prev-q:disabled {
            background-color: #B0C4DE !important; /* LightSteelBlue - for disabled state */
            color: #666666 !important; /* Darker text for readability on light blue */
            /* opacity will be applied by .btn:disabled */
        }


        .hide { display: none !important; }
    </style>
</head>
<body>
    <div class="quiz-container">
        <h1>Pengetahuan Umum Dasar</h1>
        <p id="completion-message" class="hide">Selamat Kuis Sudah Selesai 🎉</p>
        <div id="initial-controls" class="controls">
            <button id="start-btn" class="btn">Mulai</button>
            <button id="continue-btn" class="btn hide">Lanjutkan</button>
        </div>
        <div id="question-counter" class="question-counter-text hide">0/0</div>
        <div id="question-container" class="hide">
            <div id="question">Kata Bahasa Inggris</div>
            <div id="answer-buttons" class="btn-grid">
            </div>
            <div id="skip-navigation-controls" class="controls hide">
                <button id="prev-50-btn" class="btn skip-btn">&laquo; 50</button>
                <button id="prev-question-btn" class="btn btn-prev-q">&lt;</button> <button id="next-50-btn" class="btn skip-btn">50 &raquo;</button>
            </div>
        </div>
    </div>

    <script>
        const startButton = document.getElementById('start-btn');
        const continueButton = document.getElementById('continue-btn');
        const initialControls = document.getElementById('initial-controls');
        const completionMessageElement = document.getElementById('completion-message');
        const questionContainerElement = document.getElementById('question-container');
        const questionElement = document.getElementById('question');
        const answerButtonsElement = document.getElementById('answer-buttons');
        const questionCounterElement = document.getElementById('question-counter');

        const skipNavigationControls = document.getElementById('skip-navigation-controls');
        const prev50Button = document.getElementById('prev-50-btn');
        const prevQuestionButton = document.getElementById('prev-question-btn'); // Referensi untuk tombol baru
        const next50Button = document.getElementById('next-50-btn');
        const JUMP_AMOUNT = 50;

        let orderedQuestions, currentQuestionIndex;
        let score = 0;
        let questionTimeout;

        // Daftar kata mentah dari PDF (Inggris: Indonesia) - Total 1580 kata
        const rawVocabularyList = [


  { "en": "Apa Itu Elektronika Otomotif?", "id": "Aplikasi Sistem Elektronik Pada Kendaraan." },
  { "en": "Apa Kepanjangan Dari ECU (Electronic Control Unit)?", "id": "Unit Kontrol Elektronik." },
  { "en": "Apa Fungsi Utama ECU (Electronic Control Unit)?", "id": "Mengontrol Berbagai Sistem Di Dalam Mobil." },
  { "en": "Apa Itu Sensor Dalam Otomotif?", "id": "Perangkat Yang Mendeteksi Perubahan Fisik." },
  { "en": "Apa Itu Aktuator Dalam Otomotif?", "id": "Perangkat Yang Menghasilkan Gerakan Fisik." },
  { "en": "Apa Fungsi Baterai Mobil?", "id": "Menyediakan Daya Listrik Awal." },
  { "en": "Berapa Tegangan Standar Baterai Mobil?", "id": "Sekitar 12.6 Volt DC." },
  { "en": "Apa Fungsi Alternator?", "id": "Mengisi Ulang Baterai Saat Mesin Hidup." },
  { "en": "Apa Kepanjangan Dari CAN (Controller Area Network)?", "id": "Jaringan Area Pengontrol." },
  { "en": "Apa Fungsi CAN Bus?", "id": "Memungkinkan Komunikasi Antar ECU." },
  { "en": "Apa Itu Sensor TPS (Throttle Position Sensor)?", "id": "Mengukur Posisi Katup Gas." },
  { "en": "Apa Itu Sensor MAP (Manifold Absolute Pressure)?", "id": "Mengukur Tekanan Absolut Di Manifold." },
  { "en": "Apa Itu Sensor IAT (Intake Air Temperature)?", "id": "Mengukur Suhu Udara Masuk." },
  { "en": "Apa Itu Injektor Bahan Bakar?", "id": "Aktuator Yang Menyemprotkan Bahan Bakar." },
  { "en": "Apa Itu Koil Pengapian (Ignition Coil)?", "id": "Meningkatkan Tegangan Untuk Busi." },
  { "en": "Apa Itu Busi (Spark Plug)?", "id": "Menciptakan Percikan Api Untuk Pembakaran." },
  { "en": "Apa Kepanjangan Dari OBD (On-Board Diagnostics)?", "id": "Diagnostik Di Dalam Kendaraan." },
  { "en": "Apa Fungsi Port OBD-II?", "id": "Antarmuka Untuk Membaca Kode Kesalahan." },
  { "en": "Apa Itu Kode Kesalahan Diagnostik (DTC)?", "id": "Kode Yang Menunjukkan Masalah Sistem." },
  { "en": "Apa Itu Lampu Indikator Kerusakan (MIL)?", "id": "Lampu Peringatan 'Check Engine'." },
  { "en": "Apa Itu Sensor Oksigen (O2 Sensor)?", "id": "Mengukur Kadar Oksigen Gas Buang." },
  { "en": "Apa Itu Sensor CKP (Crankshaft Position Sensor)?", "id": "Mendeteksi Posisi Dan Kecepatan Kruk As." },
  { "en": "Apa Itu Sensor CMP (Camshaft Position Sensor)?", "id": "Mendeteksi Posisi Noken As." },
  { "en": "Apa Itu Modul Kontrol Mesin (ECM)?", "id": "ECU Khusus Untuk Manajemen Mesin." },
  { "en": "Apa Itu Modul Kontrol Transmisi (TCM)?", "id": "ECU Khusus Untuk Transmisi Otomatis." },
  { "en": "Apa Itu Modul Kontrol Bodi (BCM)?", "id": "Mengontrol Lampu, Kunci, Dan Fitur Bodi." },
  { "en": "Apa Itu Aktuator IAC (Idle Air Control)?", "id": "Mengatur Udara Saat Stasioner." },
  { "en": "Apa Itu Sensor Kecepatan Kendaraan (VSS)?", "id": "Mengukur Kecepatan Putaran Roda." },
  { "en": "Apa Itu Sistem Rem Anti-Terkunci (ABS)?", "id": "Mencegah Roda Terkunci Saat Pengereman." },
  { "en": "Bagaimana ABS (Anti-lock Braking System) Bekerja?", "id": "Memodulasi Tekanan Rem Secara Cepat." },
  { "en": "Apa Itu Relai (Relay)?", "id": "Saklar Elektromagnetik Untuk Arus Besar." },
  { "en": "Apa Itu Sekering (Fuse)?", "id": "Komponen Pelindung Sirkuit Dari Arus Berlebih." },
  { "en": "Apa Itu Solenoida?", "id": "Aktuator Yang Menghasilkan Gerakan Linier." },
  { "en": "Apa Itu Motor Starter?", "id": "Motor Listrik Untuk Memutar Mesin Awal." },
  { "en": "Apa Itu Sensor Knock?", "id": "Mendeteksi Ketukan Atau Detonasi Mesin." },
  { "en": "Apa Itu Sistem Injeksi Bahan Bakar Elektronik (EFI)?", "id": "Sistem Pasokan Bahan Bakar Modern." },
  { "en": "Apa Itu Karburator?", "id": "Sistem Pasokan Bahan Bakar Mekanis Lama." },
  { "en": "Apa Itu Throttle Body?", "id": "Rumah Katup Gas Dan Sensor TPS." },
  { "en": "Apa Itu Sensor Aliran Udara Massa (MAF)?", "id": "Mengukur Jumlah Massa Udara Masuk." },
  { "en": "Apa Itu Sistem Pengapian Elektronik?", "id": "Mengontrol Waktu Percikan Api Secara Elektronik." },
  { "en": "Apa Itu Distributor?", "id": "Mendistribusikan Tegangan Tinggi Ke Busi." },
  { "en": "Apa Itu Sistem Pengapian Tanpa Distributor (DLI)?", "id": "Setiap Busi Memiliki Koil Sendiri." },
  { "en": "Apa Itu EGR (Exhaust Gas Recirculation) Valve?", "id": "Mengurangi Emisi Nitrogen Oksida (NOx)." },
  { "en": "Apa Itu Sensor Suhu Pendingin (CTS)?", "id": "Mengukur Suhu Cairan Pendingin Mesin." },
  { "en": "Apa Itu Kipas Pendingin Elektrik?", "id": "Kipas Radiator Yang Digerakkan Motor." },
  { "en": "Apa Itu Pompa Bahan Bakar Elektrik?", "id": "Memompa Bahan Bakar Dari Tangki." },
  { "en": "Apa Itu Sistem Kontrol Jelajah (Cruise Control)?", "id": "Mempertahankan Kecepatan Kendaraan Secara Otomatis." },
  { "en": "Apa Itu Kantung Udara (Airbag)?", "id": "Fitur Keselamatan Pasif Saat Tabrakan." },
  { "en": "Apa Itu Modul Kontrol SRS (Supplemental Restraint System)?", "id": "ECU Khusus Untuk Airbag." },
  { "en": "Apa Itu Sensor Akselerometer?", "id": "Mengukur Percepatan Atau Getaran." },
  { "en": "Di Mana Akselerometer Digunakan?", "id": "Dalam Sistem Airbag Dan Kontrol Stabilitas." },
  { "en": "Apa Itu Sistem Kontrol Traksi (TCS)?", "id": "Mencegah Roda Selip Saat Akselerasi." },
  { "en": "Apa Itu Kontrol Stabilitas Elektronik (ESC)?", "id": "Membantu Menjaga Kontrol Kendaraan." },
  { "en": "Apa Itu Immobilizer?", "id": "Sistem Keamanan Anti-Pencurian." },
  { "en": "Bagaimana Immobilizer Bekerja?", "id": "Mencegah Mesin Hidup Tanpa Kunci Benar." },
  { "en": "Apa Itu Kunci Transponder?", "id": "Kunci Dengan Chip Elektronik Di Dalamnya." },
  { "en": "Apa Itu Power Steering Elektronik (EPS)?", "id": "Menggunakan Motor Listrik Untuk Bantuan Setir." },
  { "en": "Apa Itu Rem Parkir Elektronik (EPB)?", "id": "Mengaktifkan Rem Parkir Dengan Tombol." },
  { "en": "Apa Itu Sistem Pemantauan Tekanan Ban (TPMS)?", "id": "Memantau Tekanan Udara Di Dalam Ban." },
  { "en": "Apa Itu Sensor Hujan?", "id": "Mengaktifkan Wiper Kaca Depan Secara Otomatis." },
  { "en": "Apa Itu Sensor Cahaya?", "id": "Mengaktifkan Lampu Depan Secara Otomatis." },
  { "en": "Apa Itu Sistem Start-Stop?", "id": "Mematikan Mesin Saat Berhenti." },
  { "en": "Apa Itu Pemanas Kursi?", "id": "Elemen Pemanas Di Dalam Kursi." },
  { "en": "Apa Itu Sistem Infotainment?", "id": "Menggabungkan Informasi Dan Hiburan." },
  { "en": "Apa Itu Tampilan Head-Up (HUD)?", "id": "Memproyeksikan Informasi Ke Kaca Depan." },
  { "en": "Apa Itu Modulator?", "id": "Perangkat Yang Mengubah Sinyal." },
  { "en": "Apa Itu Kluster Instrumen Digital?", "id": "Panel Instrumen Berbasis Layar." },
  { "en": "Apa Itu Dioda?", "id": "Komponen Yang Melewatkan Arus Satu Arah." },
  { "en": "Apa Fungsi Dioda Dalam Alternator?", "id": "Untuk Menyearahkan Tegangan AC Menjadi DC." },
  { "en": "Apa Itu Transistor?", "id": "Saklar Atau Penguat Elektronik." },
  { "en": "Apa Itu Potensiometer?", "id": "Resistor Variabel, Digunakan Dalam Sensor." },
  { "en": "Apa Itu Termistor?", "id": "Resistor Yang Nilainya Berubah Dengan Suhu." },
  { "en": "Apa Itu Sensor Efek Hall?", "id": "Mendeteksi Kehadiran Medan Magnet." },
  { "en": "Apa Itu Sensor Piezoelektrik?", "id": "Menghasilkan Tegangan Saat Diberi Tekanan." },
  { "en": "Apa Itu Wiring Harness?", "id": "Kumpulan Kabel Yang Menghubungkan Komponen." },
  { "en": "Apa Itu Ground (Massa)?", "id": "Titik Referensi Listrik Umum." },
  { "en": "Apa Itu Hubung Singkat (Short Circuit)?", "id": "Koneksi Arus Listrik Yang Tidak Diinginkan." },
  { "en": "Apa Itu Sirkuit Terbuka (Open Circuit)?", "id": "Jalur Arus Listrik Yang Terputus." },
  { "en": "Apa Itu Multimeter?", "id": "Alat Ukur Listrik Serbaguna." },
  { "en": "Apa Yang Dapat Diukur Multimeter?", "id": "Tegangan, Arus, Dan Resistansi." },
  { "en": "Apa Itu Osiloskop?", "id": "Menampilkan Bentuk Gelombang Sinyal Listrik." },
  { "en": "Apa Itu Pemindai Diagnostik (Scan Tool)?", "id": "Alat Untuk Berkomunikasi Dengan ECU." },
  { "en": "Apa Itu Mode Failsafe?", "id": "Mode Operasi Terbatas Saat Terjadi Kesalahan." },
  { "en": "Apa Itu Peta Bahan Bakar (Fuel Map)?", "id": "Tabel Data Untuk Kontrol Injeksi." },
  { "en": "Apa Itu Waktu Pengapian (Ignition Timing)?", "id": "Waktu Percikan Busi Terjadi." },
  { "en": "Apa Itu Sistem Kontrol Loop Tertutup?", "id": "Sistem Menggunakan Umpan Balik." },
  { "en": "Apa Itu Sistem Kontrol Loop Terbuka?", "id": "Sistem Tanpa Umpan Balik." },
  { "en": "Apa Itu Actuator EGR?", "id": "Aktuator Untuk Mengontrol Aliran Gas Buang." },
  { "en": "Apa Itu Modul Kontrol Pompa Bahan Bakar?", "id": "Mengontrol Kecepatan Pompa Bahan Bakar." },
  { "en": "Apa Itu Sistem EVAP (Evaporative Emission Control)?", "id": "Menangkap Uap Bahan Bakar." },
  { "en": "Apa Itu Katup Purge?", "id": "Aktuator Dalam Sistem EVAP." },
  { "en": "Apa Itu Variable Valve Timing (VVT)?", "id": "Mengubah Waktu Buka Tutup Katup." },
  { "en": "Apa Itu Solenoida VVT?", "id": "Aktuator Untuk Mengontrol Sistem VVT." },
  { "en": "Apa Itu Kendaraan Hibrida?", "id": "Menggunakan Mesin Pembakaran Dan Motor Listrik." },
  { "en": "Apa Itu Baterai Tegangan Tinggi?", "id": "Baterai Utama Dalam Kendaraan Hibrida." },
  { "en": "Apa Itu Inverter?", "id": "Mengubah Arus DC Baterai Menjadi AC." },
  { "en": "Apa Itu Pengereman Regeneratif?", "id": "Mengubah Energi Pengereman Menjadi Listrik." },
  { "en": "Apa Itu Kendaraan Listrik (EV)?", "id": "Sepenuhnya Digerakkan Oleh Motor Listrik." },
  { "en": "Apa Itu Modul Kontrol Daya (PCM)?", "id": "Menggabungkan Fungsi ECU Mesin Dan Transmisi." },
  { "en": "Apa Itu Flash Memory?", "id": "Memori Non-Volatile Untuk Menyimpan Perangkat Lunak." },
  { "en": "Apa Itu Firmware Dalam ECU (Electronic Control Unit)?", "id": "Perangkat Lunak Yang Menjalankan ECU." },
  { "en": "Apa Itu Kalibrasi Mesin?", "id": "Penyesuaian Parameter Perangkat Lunak ECU." },
  { "en": "Apa Itu Electronic Throttle Control (ETC)?", "id": "Mengontrol Katup Gas Secara Elektronik." },
  { "en": "Apa Nama Lain Untuk ETC?", "id": "Drive-by-Wire." },
  { "en": "Apa Itu Accelerator Pedal Position (APP) Sensor?", "id": "Sensor Pada Pedal Gas Elektronik." },
  { "en": "Apa Itu Sensor Roda ABS (Anti-lock Braking System)?", "id": "Mengukur Kecepatan Putaran Setiap Roda." },
  { "en": "Apa Itu Yaw Rate Sensor?", "id": "Mengukur Tingkat Rotasi Kendaraan." },
  { "en": "Untuk Sistem Apa Yaw Rate Sensor Digunakan?", "id": "Kontrol Stabilitas Elektronik (ESC)." },
  { "en": "Apa Itu Steering Angle Sensor?", "id": "Mengukur Posisi Dan Sudut Roda Kemudi." },
  { "en": "Apa Itu Fuel Trim?", "id": "Penyesuaian Injeksi Bahan Bakar." },
  { "en": "Apa Itu Short Term Fuel Trim (STFT)?", "id": "Penyesuaian Bahan Bakar Jangka Pendek." },
  { "en": "Apa Itu Long Term Fuel Trim (LTFT)?", "id": "Adaptasi Bahan Bakar Jangka Panjang." },
  { "en": "Apa Itu Sistem Injeksi Langsung Bensin (GDI)?", "id": "Menyemprotkan Bensin Langsung Ke Ruang Bakar." },
  { "en": "Apa Itu Injeksi Port?", "id": "Menyemprotkan Bensin Ke Port Intake." },
  { "en": "Apa Itu Common Rail Injection?", "id": "Sistem Injeksi Untuk Mesin Diesel." },
  { "en": "Apa Itu Glow Plug?", "id": "Elemen Pemanas Untuk Membantu Start Mesin Diesel." },
  { "en": "Apa Itu Turbocharger?", "id": "Meningkatkan Tenaga Mesin Menggunakan Gas Buang." },
  { "en": "Apa Itu Wastegate?", "id": "Aktuator Untuk Mengontrol Tekanan Turbo." },
  { "en": "Apa Itu Intercooler?", "id": "Mendinginkan Udara Dari Turbocharger." },
  { "en": "Apa Itu Sistem Kontrol Iklim (Climate Control)?", "id": "Mengatur Suhu Dan Aliran Udara Kabin." },
  { "en": "Apa Itu Blend Door Actuator?", "id": "Mengontrol Campuran Udara Panas Dan Dingin." },
  { "en": "Apa Itu Blower Motor?", "id": "Motor Kipas Untuk Sirkulasi Udara Kabin." },
  { "en": "Apa Itu Sunload Sensor?", "id": "Mendeteksi Intensitas Sinar Matahari." },
  { "en": "Apa Itu Sistem Keyless Entry?", "id": "Membuka Kunci Pintu Tanpa Kunci Fisik." },
  { "en": "Apa Itu Sistem Push-to-Start?", "id": "Menghidupkan Mesin Dengan Tombol." },
  { "en": "Apa Itu LIN (Local Interconnect Network) Bus?", "id": "Jaringan Berbiaya Rendah Untuk Sistem Sederhana." },
  { "en": "Apa Contoh Aplikasi LIN Bus?", "id": "Kontrol Jendela, Kursi, Dan Kaca Spion." },
  { "en": "Apa Itu MOST (Media Oriented Systems Transport) Bus?", "id": "Jaringan Serat Optik Untuk Multimedia." },
  { "en": "Apa Keuntungan MOST Bus?", "id": "Bandwidth Sangat Tinggi Untuk Audio Dan Video." },
  { "en": "Apa Itu Battery Management System (BMS)?", "id": "Mengelola Dan Melindungi Baterai Tegangan Tinggi." },
  { "en": "Apa Fungsi Penyeimbangan Sel (Cell Balancing)?", "id": "Menjaga Semua Sel Baterai Pada Tegangan Sama." },
  { "en": "Apa Itu Inverter Dalam Mobil Hibrida/Listrik?", "id": "Mengubah DC Baterai Menjadi AC Motor." },
  { "en": "Apa Itu Konverter DC-DC?", "id": "Mengubah Tegangan DC Tinggi Ke Rendah." },
  { "en": "Apa Itu Standar Pengisian J1772?", "id": "Standar Konektor Pengisian AC." },
  { "en": "Apa Itu Combined Charging System (CCS)?", "id": "Standar Konektor Pengisian Cepat DC." },
  { "en": "Apa Itu CHAdeMO?", "id": "Standar Pengisian Cepat DC Lainnya." },
  { "en": "Apa Itu Advanced Driver-Assistance Systems (ADAS)?", "id": "Sistem Elektronik Untuk Membantu Pengemudi." },
  { "en": "Apa Itu Sensor Ultrasonik?", "id": "Menggunakan Gelombang Suara Untuk Mendeteksi Objek." },
  { "en": "Di Mana Sensor Ultrasonik Digunakan?", "id": "Dalam Sistem Bantuan Parkir." },
  { "en": "Apa Itu RADAR (Radio Detection and Ranging)?", "id": "Menggunakan Gelombang Radio Untuk Mendeteksi Objek." },
  { "en": "Apa Itu LIDAR (Light Detection and Ranging)?", "id": "Menggunakan Laser Untuk Mengukur Jarak." },
  { "en": "Apa Itu Adaptive Cruise Control (ACC)?", "id": "Cruise Control Yang Menjaga Jarak." },
  { "en": "Apa Itu Lane Keeping Assist?", "id": "Membantu Menjaga Kendaraan Di Jalurnya." },
  { "en": "Apa Itu Blind Spot Monitoring?", "id": "Mendeteksi Kendaraan Di Area Blind Spot." },
  { "en": "Apa Itu Sensor Temperatur Fluida Transmisi?", "id": "Mengukur Suhu Oli Transmisi." },
  { "en": "Apa Itu Sensor Tekanan Tangki Bahan Bakar?", "id": "Digunakan Dalam Sistem EVAP." },
  { "en": "Apa Itu Modul Kontrol Penggerak (Powertrain Control Module)?", "id": "Nama Lain Untuk PCM." },
  { "en": "Apa Itu Air-Fuel Ratio (AFR) Sensor?", "id": "Sensor Oksigen Tipe Wideband." },
  { "en": "Apa Itu Catalytic Converter?", "id": "Mengurangi Emisi Gas Buang Berbahaya." },
  { "en": "Apa Itu Post-Catalyst O2 Sensor?", "id": "Memantau Efisiensi Catalytic Converter." },
  { "en": "Apa Itu Katup Kontrol Canister Purge?", "id": "Bagian Dari Sistem EVAP." },
  { "en": "Apa Itu Sistem Injeksi Udara Sekunder?", "id": "Mengurangi Emisi Saat Start Dingin." },
  { "en": "Apa Itu Suspensi Udara Elektronik?", "id": "Menggunakan Udara Untuk Mengatur Ketinggian." },
  { "en": "Apa Itu Transduser Tekanan?", "id": "Sensor Yang Mengukur Tekanan." },
  { "en": "Apa Itu Throttle Actuator Control (TAC) Module?", "id": "Modul Untuk Mengontrol Throttle Elektronik." },
  { "en": "Apa Itu Komunikasi Multiplexing?", "id": "Mengirim Banyak Sinyal Melalui Sedikit Kabel." },
  { "en": "Apa Keuntungan Multiplexing?", "id": "Mengurangi Jumlah Dan Berat Kabel." },
  { "en": "Apa Itu Gateway Module?", "id": "Menghubungkan Jaringan Kendaraan Yang Berbeda." },
  { "en": "Apa Itu CAN (Controller Area Network) Terminator?", "id": "Resistor Di Ujung Jaringan CAN." },
  { "en": "Berapa Nilai Resistor Terminator CAN?", "id": "Biasanya 120 Ohm." },
  { "en": "Apa Itu Differential Signal?", "id": "Menggunakan Dua Kabel Dengan Tegangan Berlawanan." },
  { "en": "Mengapa CAN Bus Menggunakan Sinyal Diferensial?", "id": "Sangat Tahan Terhadap Derau (Noise)." },
  { "en": "Apa Itu Data Link Connector (DLC)?", "id": "Nama Lain Untuk Port OBD-II." },
  { "en": "Apa Itu Mode Tidur (Sleep Mode) ECU?", "id": "Mode Daya Rendah Saat Mobil Mati." },
  { "en": "Apa Itu Wake-Up Signal?", "id": "Sinyal Yang Membangunkan ECU." },
  { "en": "Apa Itu Pulse Width Modulation (PWM)?", "id": "Teknik Untuk Mengontrol Aktuator." },
  { "en": "Bagaimana PWM Mengontrol Kecepatan Motor?", "id": "Dengan Mengubah Siklus Tugas (Duty Cycle)." },
  { "en": "Apa Itu H-Bridge?", "id": "Sirkuit Untuk Mengontrol Arah Motor DC." },
  { "en": "Apa Itu Jendela Daya (Power Window) Otomatis?", "id": "Jendela Yang Menutup Dengan Satu Sentuhan." },
  { "en": "Apa Itu Sensor Jepit (Pinch Sensor)?", "id": "Mendeteksi Hambatan Pada Jendela." },
  { "en": "Apa Itu Kunci Pintu Daya?", "id": "Mengunci Dan Membuka Pintu Secara Elektrik." },
  { "en": "Apa Itu Kursi Daya (Power Seat)?", "id": "Kursi Yang Dapat Diatur Secara Elektrik." },
  { "en": "Apa Itu Memori Kursi?", "id": "Menyimpan Dan Memanggil Posisi Kursi." },
  { "en": "Apa Itu Spion Elektrokromik?", "id": "Spion Yang Meredup Otomatis." },
  { "en": "Apa Itu Sistem Audio Premium?", "id": "Sistem Suara Dengan Amplifier Eksternal." },
  { "en": "Apa Itu Amplifier?", "id": "Menguatkan Sinyal Audio." },
  { "en": "Apa Itu Global Positioning System (GPS)?", "id": "Sistem Navigasi Berbasis Satelit." },
  { "en": "Apa Itu Telematika?", "id": "Menggabungkan Telekomunikasi Dan Informatika." },
  { "en": "Apa Itu Sistem Panggilan Darurat (eCall)?", "id": "Secara Otomatis Menghubungi Layanan Darurat." },
  { "en": "Apa Itu Black Box (Event Data Recorder)?", "id": "Merekam Data Kendaraan Sebelum Kecelakaan." },
  { "en": "Apa Itu Tegangan Referensi Sensor?", "id": "Tegangan Stabil Yang Disediakan ECU." },
  { "en": "Berapa Tegangan Referensi Umum?", "id": "Biasanya 5 Volt." },
  { "en": "Apa Itu Ground Sensor?", "id": "Jalur Kembali Khusus Untuk Sensor." },
  { "en": "Apa Itu Dioda Zener?", "id": "Digunakan Untuk Regulasi Tegangan." },
  { "en": "Apa Itu Kapasitor?", "id": "Menyimpan Muatan Listrik." },
  { "en": "Apa Itu Induktor?", "id": "Menyimpan Energi Dalam Medan Magnet." },
  { "en": "Apa Itu Vehicle Identification Number (VIN)?", "id": "Nomor Identifikasi Unik Kendaraan." },
  { "en": "Apa Itu Prosedur Relearn?", "id": "Mengajarkan Komponen Baru Kepada ECU." },
  { "en": "Apa Itu Freeze Frame Data?", "id": "Data Sensor Saat Kode Kesalahan Terjadi." },
  { "en": "Apa Itu Alternator Cerdas?", "id": "Output Pengisian Diatur Oleh ECU." },
  { "en": "Apa Itu Sensor Arus Baterai?", "id": "Memantau Arus Masuk Dan Keluar Baterai." },
  { "en": "Apa Itu State of Charge (SoC)?", "id": "Tingkat Pengisian Baterai." },
  { "en": "Apa Itu State of Health (SoH)?", "id": "Kondisi Kesehatan Baterai." },
  { "en": "Apa Itu Drive Cycle?", "id": "Pola Mengemudi Untuk Pengujian." },
  { "en": "Apa Itu Pemanas Blok Mesin?", "id": "Memanaskan Mesin Sebelum Start." },
  { "en": "Apa Itu Pemanas Kaca Spion?", "id": "Menghilangkan Embun Atau Es." },
  { "en": "Apa Itu Lampu Daytime Running Light (DRL)?", "id": "Lampu Yang Menyala Otomatis." },
  { "en": "Apa Itu Lampu High-Intensity Discharge (HID)?", "id": "Lampu Depan Yang Menggunakan Gas Xenon." },
  { "en": "Apa Itu Ballast Dalam Lampu HID?", "id": "Menyediakan Tegangan Tinggi Untuk Menyalakan." },
  { "en": "Apa Itu LED (Light Emitting Diode) Headlight?", "id": "Lampu Depan Yang Menggunakan Teknologi LED." },
  { "en": "Apa Itu Adaptive Front-Lighting System (AFS)?", "id": "Mengarahkan Sinar Lampu Sesuai Arah." },
  { "en": "Apa Itu Jaringan Area Lokal (LAN)?", "id": "Jaringan Komunikasi Lokal Di Kendaraan." },
  { "en": "Apa Itu FlexRay?", "id": "Protokol Komunikasi Berkecepatan Tinggi Dan Deterministik." },
  { "en": "Kapan FlexRay Digunakan?", "id": "Untuk Sistem Kritis Seperti Kontrol Sasis." },
  { "en": "Apa Itu Ethernet Otomotif?", "id": "Ethernet Yang Diadaptasi Untuk Penggunaan." },
  { "en": "Apa Keuntungan Ethernet Otomotif?", "id": "Bandwidth Sangat Tinggi Dan Biaya Rendah." },
  { "en": "Apa Itu Sensor Posisi Pedal Rem?", "id": "Mendeteksi Seberapa Jauh Pedal Rem Ditekan." },
  { "en": "Apa Itu Electric Power Steering (EPS)?", "id": "Nama Lain Untuk Power Steering Elektronik." },
  { "en": "Apa Itu Torque Sensor?", "id": "Mengukur Torsi Yang Diterapkan Pada Roda." },
  { "en": "Apa Itu Modul Kontrol Kemudi Daya?", "id": "ECU Untuk Sistem Power Steering." },
  { "en": "Apa Itu Sistem Injeksi Bahan Bakar?", "id": "Menyemprotkan Bahan Bakar Ke Mesin." },
  { "en": "Apa Itu Lebar Pulsa Injektor?", "id": "Durasi Waktu Injektor Terbuka." },
  { "en": "Bagaimana ECU Mengontrol Jumlah Bahan Bakar?", "id": "Dengan Mengubah Lebar Pulsa Injektor." },
  { "en": "Apa Itu Closed Loop Fuel Control?", "id": "ECU Menggunakan Umpan Balik Sensor O2." },
  { "en": "Apa Itu Open Loop Fuel Control?", "id": "ECU Menggunakan Peta Tanpa Umpan Balik." },
  { "en": "Kapan Mode Open Loop Digunakan?", "id": "Saat Start Dingin Dan Akselerasi Penuh." },
  { "en": "Apa Itu Pemanas Sensor Oksigen?", "id": "Memanaskan Sensor Agar Cepat Mencapai Suhu." },
  { "en": "Apa Itu Drive-by-Wire?", "id": "Sistem Kontrol Kendaraan Elektronik." },
  { "en": "Apa Itu Shift-by-Wire?", "id": "Transmisi Yang Dikontrol Secara Elektronik." },
  { "en": "Apa Itu Brake-by-Wire?", "id": "Sistem Pengereman Yang Dikontrol Secara." },
  { "en": "Apa Itu Sistem Hibrida Ringan (Mild Hybrid)?", "id": "Motor Listrik Hanya Membantu Mesin." },
  { "en": "Apa Itu Sistem Hibrida Penuh (Full Hybrid)?", "id": "Dapat Berjalan Dengan Listrik Saja." },
  { "en": "Apa Itu Plug-in Hybrid Electric Vehicle (PHEV)?", "id": "Hibrida Dengan Baterai Yang Dapat Diisi." },
  { "en": "Apa Itu Battery Electric Vehicle (BEV)?", "id": "Kendaraan Yang Sepenuhnya Listrik." },
  { "en": "Apa Itu On-Board Charger (OBC)?", "id": "Mengisi Baterai Tegangan Tinggi Dari AC." },
  { "en": "Apa Itu DC Fast Charging?", "id": "Mengisi Baterai Langsung Dengan Arus DC." },
  { "en": "Apa Itu Thermal Management System?", "id": "Menjaga Suhu Optimal Baterai." },
  { "en": "Mengapa Pendinginan Baterai Penting?", "id": "Untuk Kinerja Dan Umur Panjang Baterai." },
  { "en": "Apa Itu Resistansi Internal Baterai?", "id": "Hambatan Di Dalam Baterai." },
  { "en": "Apa Itu Siklus Hidup Baterai?", "id": "Jumlah Siklus Pengisian-Pengosongan." },
  { "en": "Apa Itu Sistem Pemanas Kabin Elektrik?", "id": "Digunakan Pada Mobil Listrik." },
  { "en": "Apa Itu Kompresor AC Elektrik?", "id": "Kompresor AC Yang Digerakkan Motor." },
  { "en": "Apa Itu Analog-to-Digital Converter (ADC)?", "id": "Mengubah Sinyal Sensor Analog Ke Digital." },
  { "en": "Apa Itu Digital-to-Analog Converter (DAC)?", "id": "Mengubah Sinyal Digital Ke Analog." },
  { "en": "Apa Itu Microcontroller?", "id": "Komputer Kecil Di Dalam ECU." },
  { "en": "Apa Itu Prosesor?", "id": "Otak Dari Microcontroller." },
  { "en": "Apa Itu Random Access Memory (RAM)?", "id": "Memori Sementara Untuk Data." },
  { "en": "Apa Itu Read Only Memory (ROM)?", "id": "Memori Permanen Untuk Program." },
  { "en": "Apa Itu Electrically Erasable Programmable ROM (EEPROM)?", "id": "ROM Yang Dapat Dihapus Secara Elektrik." },
  { "en": "Untuk Apa EEPROM Digunakan?", "id": "Menyimpan Data Konfigurasi Dan Odometer." },
  { "en": "Apa Itu Peta Diagnostik?", "id": "Alur Logika Untuk Menemukan Kesalahan." },
  { "en": "Apa Itu Skematik Kelistrikan?", "id": "Diagram Sirkuit Kelistrikan Kendaraan." },
  { "en": "Apa Itu Nomor Identifikasi Kendaraan (VIN)?", "id": "Kode Unik Untuk Setiap Kendaraan." },
  { "en": "Apa Itu Reprogramming ECU (Flashing)?", "id": "Memperbarui Perangkat Lunak Di Dalam ECU." },
  { "en": "Apa Itu J2534?", "id": "Standar Antarmuka Untuk Reprogramming." },
  { "en": "Apa Itu Automatic Emergency Braking (AEB)?", "id": "Mengerem Otomatis Untuk Menghindari Tabrakan." },
  { "en": "Apa Itu Forward Collision Warning (FCW)?", "id": "Memberi Peringatan Akan Potensi Tabrakan." },
  { "en": "Apa Itu Kamera Dasbor (Dashcam)?", "id": "Merekam Video Perjalanan." },
  { "en": "Apa Itu Pengapian Kapasitif (CDI)?", "id": "Sistem Pengapian Menggunakan Pelepasan." },
  { "en": "Apa Itu Sensor Sudut Kemudi?", "id": "Nama Lain Steering Angle Sensor." },
  { "en": "Apa Itu Kontrol Iklim Zona Ganda?", "id": "Suhu Berbeda Untuk Sisi." },
  { "en": "Apa Itu Sensor Kualitas Udara?", "id": "Mendeteksi Polutan Di Udara Luar." },
  { "en": "Apa Itu Mode Resirkulasi Otomatis?", "id": "Menutup Ventilasi Saat Udara Luar." },
  { "en": "Apa Itu Sistem Suspensi Adaptif?", "id": "Mengubah Kekakuan Suspensi Secara." },
  { "en": "Apa Itu Peredam Magnetoreologi?", "id": "Peredam Yang Diisi Cairan." },
  { "en": "Apa Itu Sistem Bantuan Parkir?", "id": "Membantu Pengemudi Saat Parkir." },
  { "en": "Apa Itu Kamera 360 Derajat?", "id": "Menampilkan Pandangan Mata Burung." },
  { "en": "Apa Itu Parkir Otomatis?", "id": "Mobil Dapat Parkir Sendiri." },
  { "en": "Apa Itu Sensor Titik Buta?", "id": "Nama Lain Blind Spot Monitoring." },
  { "en": "Apa Itu Rear Cross Traffic Alert?", "id": "Memberi Peringatan Saat Mundur." },
  { "en": "Apa Itu Pemanas Roda Kemudi?", "id": "Elemen Pemanas Di Roda Kemudi." },
  { "en": "Apa Itu Wiper Kaca Otomatis?", "id": "Nama Lain Untuk Rain Sensing Wipers." },
  { "en": "Apa Itu Sensor Getaran?", "id": "Nama Lain Untuk Knock Sensor." },
  { "en": "Apa Itu Katup Solenoida?", "id": "Katup Yang Dioperasikan Secara Elektromagnetik." },
  { "en": "Apa Itu Sensor Tekanan Oli?", "id": "Memantau Tekanan Oli Mesin." },
  { "en": "Apa Itu Saklar Tekanan Oli?", "id": "Hanya Menunjukkan Ada Atau Tidaknya." },
  { "en": "Apa Itu Sensor Level Bahan Bakar?", "id": "Mengukur Jumlah Bahan Bakar." },
  { "en": "Apa Itu Pelampung?", "id": "Komponen Utama Sensor Level." },
  { "en": "Apa Itu Termokopel?", "id": "Sensor Suhu Untuk Suhu Sangat Tinggi." },
  { "en": "Di Mana Termokopel Digunakan?", "id": "Pada Sensor Suhu Gas Buang." },
  { "en": "Apa Itu Exhaust Gas Temperature (EGT) Sensor?", "id": "Mengukur Suhu Gas Buang." },
  { "en": "Apa Itu Filter Partikulat Diesel (DPF)?", "id": "Menangkap Jelaga Dari Gas Buang." },
  { "en": "Apa Itu Regenerasi DPF?", "id": "Proses Membakar Jelaga Yang Terkumpul." },
  { "en": "Apa Itu Sensor Tekanan Diferensial?", "id": "Mengukur Perbedaan Tekanan." },
  { "en": "Apa Itu Reduksi Katalitik Selektif (SCR)?", "id": "Sistem Kontrol Emisi Diesel." },
  { "en": "Apa Itu Diesel Exhaust Fluid (DEF)?", "id": "Cairan Urea Yang Digunakan." },
  { "en": "Apa Itu Sistem Hibrida Paralel?", "id": "Mesin Dan Motor Dapat Menggerakkan." },
  { "en": "Apa Itu Sistem Hibrida Seri?", "id": "Hanya Motor Listrik Yang Menggerakkan." },
  { "en": "Apa Itu Peringatan Keberangkatan Jalur?", "id": "Memberi Peringatan Saat Mobil." },
  { "en": "Apa Itu Sistem Navigasi?", "id": "Menyediakan Peta Dan Arah." },
  { "en": "Apa Itu Penerima GPS (Global Positioning System)?", "id": "Menerima Sinyal Dari Satelit." },
  { "en": "Apa Itu Antena?", "id": "Menerima Sinyal Radio Atau Satelit." },
  { "en": "Apa Itu Modul Kontrol Pintu?", "id": "Mengontrol Fungsi Di Pintu." },
  { "en": "Apa Itu Modul Kontrol Kursi?", "id": "Mengontrol Fungsi Kursi Daya." },
  { "en": "Apa Itu Lampu High-Beam Otomatis?", "id": "Beralih Antara Lampu Jauh." },
  { "en": "Apa Itu Sistem Pengenalan Rambu Lalu Lintas?", "id": "Membaca Dan Menampilkan Rambu." },
  { "en": "Apa Itu Pengisian Nirkabel (Wireless Charging)?", "id": "Mengisi Daya Ponsel Tanpa Kabel." },
  { "en": "Apa Itu Sistem Audio Digital?", "id": "Memproses Sinyal Audio Secara Digital." },
  { "en": "Apa Itu Radio Satelit?", "id": "Layanan Radio Berlangganan." },
  { "en": "Apa Itu Radio HD?", "id": "Siaran Radio Digital." },
  { "en": "Apa Itu Speaker?", "id": "Mengubah Sinyal Listrik Menjadi Suara." },
  { "en": "Apa Itu Tweeter?", "id": "Speaker Untuk Frekuensi Tinggi." },
  { "en": "Apa Itu Mid-range?", "id": "Speaker Untuk Frekuensi Tengah." },
  { "en": "Apa Itu Woofer?", "id": "Speaker Untuk Frekuensi Rendah." },
  { "en": "Apa Itu Subwoofer?", "id": "Speaker Untuk Frekuensi Sangat Rendah." },
  { "en": "Apa Itu Crossover Audio?", "id": "Membagi Sinyal Audio Ke Speaker." },
  { "en": "Apa Itu Equalizer?", "id": "Menyesuaikan Respons Frekuensi Audio." },
  { "en": "Apa Itu Konektivitas Bluetooth?", "id": "Komunikasi Nirkabel Jarak Pendek." },
  { "en": "Apa Itu Sistem Start Tanpa Kunci?", "id": "Nama Lain Sistem Push-to-Start." },
  { "en": "Apa Itu Fob Kunci?", "id": "Pemancar Remote Untuk Sistem Keyless." },
  { "en": "Apa Itu Antena Frekuensi Rendah?", "id": "Digunakan Untuk Mendeteksi Fob Kunci." },
  { "en": "Apa Itu Alternator?", "id": "Menghasilkan Listrik AC (Alternating Current)." },
  { "en": "Apa Itu Penyearah (Rectifier)?", "id": "Mengubah AC Menjadi DC (Direct Current)." },
  { "en": "Apa Itu Regulator Tegangan?", "id": "Menjaga Tegangan Output Alternator Stabil." },
  { "en": "Apa Itu Roda Gigi Cincin (Ring Gear)?", "id": "Gigi Di Sekeliling Roda Gila." },
  { "en": "Apa Yang Diputar Oleh Motor Starter?", "id": "Roda Gigi Cincin." },
  { "en": "Apa Itu Solenoida Starter?", "id": "Menghubungkan Gigi Starter Dan Menyalakan." },
  { "en": "Apa Itu Sinyal Analog?", "id": "Sinyal Dengan Nilai Yang Berubah." },
  { "en": "Apa Itu Sinyal Digital?", "id": "Sinyal Dengan Dua Nilai (Tinggi/Rendah)." },
  { "en": "Apa Itu Siklus Tugas (Duty Cycle)?", "id": "Persentase Waktu Sinyal Aktif." },
  { "en": "Bagaimana Siklus Tugas Mengontrol Aktuator?", "id": "Dengan Mengubah Daya Rata-rata." },
  { "en": "Apa Itu Bus Satu Kawat?", "id": "Contoh: LIN (Local Interconnect Network)." },
  { "en": "Apa Itu Mode Master-Slave?", "id": "Satu Perangkat Mengontrol Perangkat Lain." },
  { "en": "Apa Itu Pesan CAN (Controller Area Network)?", "id": "Paket Data Yang Dikirim Di Bus." },
  { "en": "Apa Itu Identifier?", "id": "Menentukan Prioritas Pesan CAN." },
  { "en": "Apa Itu Arbitrasi?", "id": "Proses Menentukan Pesan Prioritas." },
  { "en": "Identifier Mana Yang Menang Arbitrasi?", "id": "Identifier Dengan Nilai Lebih Rendah." },
  { "en": "Apa Itu Kesalahan Bus CAN?", "id": "Kondisi Abnormal Di Jaringan." },
  { "en": "Apa Itu Mode Bus-Off?", "id": "Node Memutuskan Diri Dari Jaringan." },
  { "en": "Apa Itu Kecepatan Baud?", "id": "Tingkat Transmisi Bit." },
  { "en": "Apa Itu CAN Kecepatan Tinggi?", "id": "Digunakan Untuk Sistem Penting." },
  { "en": "Apa Itu CAN Toleran Kesalahan?", "id": "Dapat Beroperasi Dengan Satu Kawat." },
  { "en": "Apa Itu Body Control Module (BCM)?", "id": "Nama Lain Modul Kontrol Bodi." },
  { "en": "Apa Itu Lampu Peringatan?", "id": "Memberi Tahu Pengemudi Tentang Masalah." },
  { "en": "Apa Itu Sensor Level Minyak?", "id": "Mengukur Ketinggian Minyak Di Mesin." },
  { "en": "Apa Itu Sensor Tekanan Bahan Bakar?", "id": "Memantau Tekanan Dalam Sistem Bahan Bakar." },
  { "en": "Apa Itu Modul Kontrol ABS (Anti-lock Braking System)?", "id": "ECU Untuk Sistem Rem." },
  { "en": "Apa Itu Katup Modulator ABS?", "id": "Mengontrol Tekanan Rem Ke Roda." },
  { "en": "Apa Itu Distribusi Tenaga Pengereman Elektronik (EBD)?", "id": "Mengoptimalkan Pengereman Antara Roda." },
  { "en": "Apa Itu Bantuan Rem (Brake Assist)?", "id": "Meningkatkan Tekanan Rem Saat Darurat." },
  { "en": "Apa Itu Sistem Bantuan Pengemudi Canggih?", "id": "Nama Lain Untuk ADAS." },
  { "en": "Apa Itu Kamera Penglihatan Malam?", "id": "Menggunakan Inframerah Untuk Melihat." },
  { "en": "Apa Itu Deteksi Pejalan Kaki?", "id": "Sistem ADAS Untuk Mendeteksi Orang." },
  { "en": "Apa Itu Peringatan Lalu Lintas Silang Depan?", "id": "Memberi Peringatan Saat Menyeberang." },
  { "en": "Apa Itu Wiper Sensing Kecepatan?", "id": "Kecepatan Wiper Menyesuaikan Kecepatan." },
  { "en": "Apa Itu Sistem Pendingin Udara (AC)?", "id": "Mendinginkan Udara Di Dalam Kabin." },
  { "en": "Apa Itu Sensor Tekanan AC?", "id": "Memantau Tekanan Refrigeran." },
  { "en": "Apa Itu Kopling Kompresor?", "id": "Menghubungkan Dan Memutuskan Kompresor." },
  { "en": "Apa Itu Sensor Suhu Evaporator?", "id": "Mencegah Evaporator Membeku." },
  { "en": "Apa Itu Kaca Spion Pemanas?", "id": "Menghilangkan Embun Atau Es." },
  { "en": "Apa Itu Defroster Kaca Belakang?", "id": "Elemen Pemanas Di Kaca Belakang." },
  { "en": "Apa Itu Sistem Suara Aktif?", "id": "Menghasilkan Suara Mesin Buatan." },
  { "en": "Apa Itu Sistem Pembatalan Derau Aktif?", "id": "Mengurangi Derau Kabin." },
  { "en": "Apa Itu Sistem Pencahayaan Ambient?", "id": "Pencahayaan Dekoratif Di Dalam Kabin." },
  { "en": "Apa Itu Kaca Spion Lipat Daya?", "id": "Spion Yang Dapat Dilipat." },
  { "en": "Apa Itu Kolom Kemudi Daya?", "id": "Kolom Kemudi Yang Dapat Diatur." },
  { "en": "Apa Itu Suspensi Aktif?", "id": "Suspensi Yang Dikontrol Secara Elektronik." },
  { "en": "Apa Itu Sensor Ketinggian Kendaraan?", "id": "Mengukur Jarak Antara Bodi." },
  { "en": "Apa Itu Sistem Penggerak Empat Roda (4WD)?", "id": "Tenaga Dikirim Ke Empat Roda." },
  { "en": "Apa Itu Transfer Case?", "id": "Membagi Tenaga Antara As Roda." },
  { "en": "Apa Itu Locking Differential?", "id": "Mengunci Roda Kiri Dan Kanan." },
  { "en": "Apa Itu Sistem Start Jarak Jauh?", "id": "Menghidupkan Mesin Dari Jarak." },
  { "en": "Apa Itu Wi-Fi Hotspot?", "id": "Menyediakan Koneksi Internet Di Mobil." },
  { "en": "Apa Itu Integrasi Smartphone?", "id": "Menghubungkan Ponsel Ke Sistem." },
  { "en": "Apa Itu Apple CarPlay?", "id": "Antarmuka Apple Untuk Mobil." },
  { "en": "Apa Itu Android Auto?", "id": "Antarmuka Android Untuk Mobil." },
  { "en": "Apa Itu Resistor?", "id": "Menghambat Aliran Arus." },
  { "en": "Apa Itu Voltmeter?", "id": "Mengukur Tegangan." },
  { "en": "Apa Itu Ammeter?", "id": "Mengukur Arus." },
  { "en": "Apa Itu Ohmmeter?", "id": "Mengukur Resistansi." },
  { "en": "Apa Itu Penurunan Tegangan (Voltage Drop)?", "id": "Kehilangan Tegangan Akibat Resistansi." },
  { "en": "Apa Itu Kontinuitas?", "id": "Memeriksa Apakah Sirkuit Lengkap." },
  { "en": "Apa Itu Resistansi Tinggi?", "id": "Koneksi Buruk Atau Korosi." },
  { "en": "Apa Itu Sensor Suhu Udara Ambient?", "id": "Mengukur Suhu Udara Luar." },
  { "en": "Apa Itu Sistem EGR (Exhaust Gas Recirculation) Elektronik?", "id": "Menggunakan Katup Elektronik." },
  { "en": "Apa Itu Sensor Posisi EGR?", "id": "Memantau Posisi Katup EGR." },
  { "en": "Apa Itu Variable Geometry Turbocharger (VGT)?", "id": "Turbo Dengan Vane Yang Dapat." },
  { "en": "Apa Itu Aktuator VGT?", "id": "Mengontrol Posisi Vane." },
  { "en": "Apa Itu Sistem Kemudi Empat Roda?", "id": "Roda Belakang Dapat Berbelok." },
  { "en": "Apa Itu Pemanas Udara Intake?", "id": "Memanaskan Udara Masuk Untuk Start." },
  { "en": "Apa Itu Sistem Kontrol Emisi?", "id": "Mengurangi Polutan Dari Kendaraan." },
  { "en": "Apa Itu Karbon Canister?", "id": "Menyimpan Uap Bahan Bakar." },
  { "en": "Apa Itu Katup Vent Canister?", "id": "Katup Dalam Sistem EVAP." },
  { "en": "Apa Itu Pemanas PTC (Positive Temperature Coefficient)?", "id": "Elemen Pemanas Keramik." },
  { "en": "Apa Itu Kontrol Kipas Variabel?", "id": "Kecepatan Kipas Pendingin Dapat." },
  { "en": "Apa Itu Motor Kipas Brushless?", "id": "Motor Kipas Tanpa Sikat." },
  { "en": "Apa Itu Lampu Sein?", "id": "Lampu Indikator Arah." },
  { "en": "Apa Itu Flasher Relay?", "id": "Membuat Lampu Sein Berkedip." },
  { "en": "Apa Itu Lampu Bahaya (Hazard Lights)?", "id": "Mengedipkan Semua Lampu Sein." },
  { "en": "Apa Itu Klakson?", "id": "Perangkat Peringatan Suara." },
  { "en": "Apa Itu Sirkuit Terpadu (IC)?", "id": "Chip Elektronik Dengan Banyak." },
  { "en": "Apa Itu Memori?", "id": "Komponen Untuk Menyimpan Data." },
  { "en": "Apa Itu Perangkat Lunak?", "id": "Instruksi Yang Menjalankan Perangkat." },
  { "en": "Apa Itu Pembaruan Perangkat Lunak Over-the-Air (OTA)?", "id": "Memperbarui Perangkat Lunak Secara." },
  { "en": "Apa Itu Keamanan Siber Otomotif?", "id": "Melindungi Kendaraan Dari Serangan." },
  { "en": "Apa Itu Firewall Kendaraan?", "id": "Mencegah Akses Tidak Sah." },
  { "en": "Apa Itu Sistem Deteksi Intrusi?", "id": "Mendeteksi Aktivitas Jaringan." },
  { "en": "Apa Itu Sensor Barometrik?", "id": "Mengukur Tekanan Atmosfer." },
  { "en": "Untuk Apa Sensor Barometrik Digunakan?", "id": "Kompensasi Ketinggian Untuk Campuran." },
  { "en": "Apa Itu Transmisi Otomatis?", "id": "Transmisi Yang Berpindah Gigi." },
  { "en": "Apa Itu Solenoida Pindah (Shift Solenoid)?", "id": "Mengontrol Aliran Fluida Untuk." },
  { "en": "Apa Itu Solenoida Kontrol Tekanan?", "id": "Mengatur Tekanan Fluida Transmisi." },
  { "en": "Apa Itu Sensor Kecepatan Input Turbin?", "id": "Mengukur Kecepatan Input Transmisi." },
  { "en": "Apa Itu Sensor Kecepatan Output?", "id": "Mengukur Kecepatan Output Transmisi." },
  { "en": "Apa Itu Mode Limp-Home?", "id": "Mode Failsafe Untuk Transmisi." },
  { "en": "Apa Itu Continuously Variable Transmission (CVT)?", "id": "Transmisi Dengan Rasio Gigi." },
  { "en": "Apa Itu Torque Converter Clutch (TCC)?", "id": "Mengunci Torque Converter Untuk Efisiensi." },
  { "en": "Apa Itu Solenoida TCC?", "id": "Aktuator Untuk Mengontrol Kopling." },
  { "en": "Apa Itu Transmisi Otomatis Kopling Ganda?", "id": "Transmisi Dengan Dua Kopling." },
  { "en": "Apa Keuntungan Kopling Ganda?", "id": "Perpindahan Gigi Sangat Cepat." },
  { "en": "Apa Itu Mechatronic Unit?", "id": "Menggabungkan ECU Dan Aktuator." },
  { "en": "Apa Itu Sistem Injeksi Udara Sekunder?", "id": "Mengurangi Emisi Saat Pemanasan." },
  { "en": "Apa Itu Pompa Udara Sekunder?", "id": "Memompa Udara Ke Sistem." },
  { "en": "Apa Itu Katup Switching Udara?", "id": "Mengontrol Aliran Udara Sekunder." },
  { "en": "Apa Itu Sistem Kontrol Suara Aktif?", "id": "Mengubah Suara Mesin Di Kabin." },
  { "en": "Apa Itu Sistem Pembatalan Derau Aktif?", "id": "Mengurangi Derau Jalan Dan Mesin." },
  { "en": "Apa Itu Mikrofon Di Dalam Kabin?", "id": "Mendeteksi Derau Untuk Dibatalkan." },
  { "en": "Apa Itu Amplifier Audio?", "id": "Menguatkan Sinyal Audio Ke Speaker." },
  { "en": "Apa Itu Digital Signal Processor (DSP)?", "id": "Memproses Sinyal Audio Secara Digital." },
  { "en": "Apa Itu Equalization?", "id": "Menyesuaikan Respons Frekuensi Suara." },
  { "en": "Apa Itu Sistem Peringatan Keberangkatan Jalur?", "id": "Memberi Peringatan Saat Keluar." },
  { "en": "Apa Itu Lane Departure Warning (LDW)?", "id": "Nama Lain Sistem Peringatan." },
  { "en": "Apa Itu Lane Keeping Assist (LKA)?", "id": "Secara Aktif Membantu Menjaga." },
  { "en": "Apa Itu Kamera Menghadap Depan?", "id": "Digunakan Untuk Sistem LDW Dan LKA." },
  { "en": "Apa Itu Pengenalan Gambar?", "id": "Perangkat Lunak Menganalisis Gambar." },
  { "en": "Apa Itu Cruise Control?", "id": "Nama Lain Sistem Kontrol Jelajah." },
  { "en": "Apa Itu Saklar Rem?", "id": "Membatalkan Cruise Control Saat." },
  { "en": "Apa Itu Saklar Kopling?", "id": "Membatalkan Cruise Control Di Mobil." },
  { "en": "Apa Itu Aktuator Cruise Control?", "id": "Mengontrol Posisi Katup Gas." },
  { "en": "Apa Itu Pemanas Kaca Spion?", "id": "Elemen Pemanas Di Belakang." },
  { "en": "Apa Itu Lampu Sein LED?", "id": "Menggunakan LED Untuk Lampu Sein." },
  { "en": "Apa Itu Modul Kontrol Lampu?", "id": "ECU Untuk Mengontrol Pencahayaan." },
  { "en": "Apa Itu Leveling Lampu Otomatis?", "id": "Menyesuaikan Arah Sinar Lampu." },
  { "en": "Apa Itu Sensor Ketinggian Suspensi?", "id": "Digunakan Untuk Leveling Lampu." },
  { "en": "Apa Itu Lampu Kabut?", "id": "Lampu Untuk Kondisi Kabut." },
  { "en": "Apa Itu Kaca Film?", "id": "Lapisan Tipis Pada Kaca." },
  { "en": "Apa Itu Sensor Solar?", "id": "Nama Lain Untuk Sunload Sensor." },
  { "en": "Apa Itu Sistem Pendingin Mesin?", "id": "Menjaga Suhu Operasi Mesin." },
  { "en": "Apa Itu Termostat?", "id": "Mengatur Aliran Cairan Pendingin." },
  { "en": "Apa Itu Radiator?", "id": "Membuang Panas Dari Cairan." },
  { "en": "Apa Itu Kipas Pendingin?", "id": "Menarik Udara Melalui Radiator." },
  { "en": "Apa Itu Modul Kontrol Kipas?", "id": "Mengontrol Kecepatan Kipas Pendingin." },
  { "en": "Apa Itu Pemanas Blok?", "id": "Memanaskan Mesin Di Cuaca." },
  { "en": "Apa Itu Glow Plug Controller?", "id": "Mengontrol Operasi Glow Plug." },
  { "en": "Apa Itu Sistem Injeksi Diesel?", "id": "Menyemprotkan Solar Ke Mesin." },
  { "en": "Apa Itu Tekanan Injeksi?", "id": "Tekanan Bahan Bakar Diesel." },
  { "en": "Apa Itu Injektor Piezoelektrik?", "id": "Injektor Diesel Berkecepatan Sangat." },
  { "en": "Apa Itu Sistem Bahan Bakar Returnless?", "id": "Tidak Ada Jalur Kembali." },
  { "en": "Apa Itu Sensor Tekanan Rel?", "id": "Mengukur Tekanan Di Common Rail." },
  { "en": "Apa Itu Katup Kontrol Tekanan?", "id": "Mengatur Tekanan Di Common Rail." },
  { "en": "Apa Itu Sistem Pelumasan?", "id": "Melumasi Komponen Mesin Bergerak." },
  { "en": "Apa Itu Pompa Oli?", "id": "Mensirkulasikan Oli Mesin." },
  { "en": "Apa Itu Filter Oli?", "id": "Menyaring Kotoran Dari Oli." },
  { "en": "Apa Itu Oil Life Monitor?", "id": "Memperkirakan Sisa Umur Oli." },
  { "en": "Apa Itu Sensor Level Oli?", "id": "Mendeteksi Ketinggian Oli." },
  { "en": "Apa Itu Sensor Kualitas Oli?", "id": "Menganalisis Kondisi Oli Mesin." },
  { "en": "Apa Itu Transmisi Manual Otomatis (AMT)?", "id": "Transmisi Manual Dengan Kopling." },
  { "en": "Apa Itu Aktuator Kopling?", "id": "Mengoperasikan Kopling Secara Hidrolik." },
  { "en": "Apa Itu Aktuator Pindah Gigi?", "id": "Memindahkan Gigi Transmisi." },
  { "en": "Apa Itu Paddle Shifter?", "id": "Tuas Di Balik Roda Kemudi." },
  { "en": "Apa Itu Sistem Penggerak Semua Roda (AWD)?", "id": "Tenaga Dikirim Ke Empat Roda." },
  { "en": "Apa Perbedaan AWD Dan 4WD?", "id": "AWD Selalu Aktif, 4WD Dapat." },
  { "en": "Apa Itu Kopling Viskos?", "id": "Kopling Yang Menggunakan Fluida." },
  { "en": "Apa Itu Kopling Multi-Pelat?", "id": "Kopling Dengan Beberapa Pelat." },
  { "en": "Apa Itu Modul Kontrol Drivetrain?", "id": "Mengontrol Sistem AWD Atau 4WD." },
  { "en": "Apa Itu Ban?", "id": "Komponen Karet Di Roda." },
  { "en": "Apa Itu Sistem Peringatan Tekanan Ban?", "id": "Nama Lain Untuk TPMS." },
  { "en": "Apa Itu TPMS (Tire Pressure Monitoring System) Langsung?", "id": "Menggunakan Sensor Di Setiap." },
  { "en": "Apa Itu TPMS (Tire Pressure Monitoring System) Tidak Langsung?", "id": "Menggunakan Sensor Roda ABS." },
  { "en": "Apa Itu Sensor Suhu Ban?", "id": "Mengukur Suhu Udara Di Ban." },
  { "en": "Apa Itu Rangka (Chassis)?", "id": "Kerangka Utama Kendaraan." },
  { "en": "Apa Itu Sistem Suspensi?", "id": "Menghubungkan Roda Ke Bodi." },
  { "en": "Apa Itu Pegas?", "id": "Menyerap Guncangan Jalan." },
  { "en": "Apa Itu Peredam Kejut (Shock Absorber)?", "id": "Meredam Osilasi Pegas." },
  { "en": "Apa Itu Suspensi Udara?", "id": "Menggunakan Pegas Udara." },
  { "en": "Apa Itu Kompresor Udara?", "id": "Menghasilkan Udara Bertekanan." },
  { "en": "Apa Itu Katup Solenoida Suspensi?", "id": "Mengontrol Aliran Udara Ke Pegas." },
  { "en": "Apa Itu Modul Kontrol Suspensi?", "id": "ECU Untuk Sistem Suspensi." },
  { "en": "Apa Itu Sistem Kemudi?", "id": "Mengontrol Arah Kendaraan." },
  { "en": "Apa Itu Roda Kemudi?", "id": "Roda Yang Dipegang Pengemudi." },
  { "en": "Apa Itu Rak Kemudi?", "id": "Mekanisme Yang Memutar Roda." },
  { "en": "Apa Itu Pompa Power Steering?", "id": "Menghasilkan Tekanan Hidrolik." },
  { "en": "Apa Itu Power Steering Hidrolik?", "id": "Menggunakan Fluida Untuk Bantuan." },
  { "en": "Apa Itu Power Steering Elektro-Hidrolik?", "id": "Pompa Hidrolik Digerakkan Motor." },
  { "en": "Apa Itu Sistem Rem?", "id": "Memperlambat Atau Menghentikan Kendaraan." },
  { "en": "Apa Itu Rem Cakram?", "id": "Menggunakan Kaliper Dan Piringan." },
  { "en": "Apa Itu Rem Tromol?", "id": "Menggunakan Sepatu Dan Tromol." },
  { "en": "Apa Itu Kaliper Rem?", "id": "Menjepit Piringan Rem." },
  { "en": "Apa Itu Piringan Rem (Rotor)?", "id": "Piringan Logam Yang Berputar." },
  { "en": "Apa Itu Kampas Rem?", "id": "Bahan Gesekan Di Kaliper." },
  { "en": "Apa Itu Fluida Rem?", "id": "Cairan Hidrolik Untuk Sistem." },
  { "en": "Apa Itu Silinder Master?", "id": "Menghasilkan Tekanan Hidrolik." },
  { "en": "Apa Itu Booster Rem?", "id": "Menggunakan Vakum Mesin Untuk." },
  { "en": "Apa Itu Sensor Keausan Kampas Rem?", "id": "Mendeteksi Saat Kampas Rem." },
  { "en": "Apa Itu Pengukur Akselerasi?", "id": "Nama Lain Akselerometer." },
  { "en": "Apa Itu Giroskop?", "id": "Mengukur Orientasi Atau Kecepatan." },
  { "en": "Apa Itu Inertial Measurement Unit (IMU)?", "id": "Menggabungkan Akselerometer Dan Giroskop." },
  { "en": "Apa Itu Sistem Bantuan Tampilan Titik Buta?", "id": "Menampilkan Gambar Dari Kamera." },
  { "en": "Apa Itu Peringatan Tabrakan Belakang?", "id": "Mendeteksi Potensi Tabrakan Dari." },
  { "en": "Apa Itu Sistem Keselamatan Pre-Crash?", "id": "Menyiapkan Kendaraan Untuk Tabrakan." },
  { "en": "Apa Itu Pengencang Sabuk Pengaman?", "id": "Mengencangkan Sabuk Pengaman." },
  { "en": "Apa Itu Pembatas Beban?", "id": "Mengurangi Gaya Sabuk Pengaman." },
  { "en": "Apa Itu Sabuk Pengaman?", "id": "Perangkat Penahan Keselamatan." },
  { "en": "Apa Itu Sensor Impak?", "id": "Mendeteksi Tabrakan Untuk Memicu Airbag." },
  { "en": "Di Mana Sensor Impak Terletak?", "id": "Di Bagian Depan, Samping, Dan Belakang." },
  { "en": "Apa Itu Modul Kontrol Airbag?", "id": "Nama Lain Modul Kontrol SRS." },
  { "en": "Apa Itu Inflator Airbag?", "id": "Menghasilkan Gas Untuk Mengembangkan Kantung." },
  { "en": "Apa Itu Clock Spring?", "id": "Menghubungkan Listrik Ke Roda Kemudi." },
  { "en": "Untuk Apa Clock Spring Digunakan?", "id": "Untuk Airbag Pengemudi, Klakson, Dan Tombol." },
  { "en": "Apa Itu Sensor Klasifikasi Penumpang?", "id": "Mendeteksi Apakah Ada Penumpang." },
  { "en": "Apa Itu Kunci Pintu Otomatis?", "id": "Mengunci Pintu Secara Otomatis." },
  { "en": "Apa Itu Pembuka Bagasi Daya?", "id": "Membuka Dan Menutup Bagasi." },
  { "en": "Apa Itu Sistem Audio?", "id": "Menghasilkan Suara Di Dalam Kendaraan." },
  { "en": "Apa Itu Head Unit?", "id": "Radio Dan Pusat Kontrol Audio." },
  { "en": "Apa Itu Tuner?", "id": "Menerima Siaran Radio." },
  { "en": "Apa Itu CD (Compact Disc) Player?", "id": "Memutar Musik Dari CD." },
  { "en": "Apa Itu Port USB (Universal Serial Bus)?", "id": "Menghubungkan Perangkat Eksternal." },
  { "en": "Apa Itu Bluetooth?", "id": "Komunikasi Nirkabel Jarak Pendek." },
  { "en": "Apa Itu Streaming Audio Bluetooth?", "id": "Memutar Musik Nirkabel Dari Ponsel." },
  { "en": "Apa Itu Panggilan Bebas Genggam?", "id": "Menelepon Menggunakan Sistem Audio." },
  { "en": "Apa Itu Mikrofon?", "id": "Menangkap Suara Untuk Panggilan." },
  { "en": "Apa Itu Antena Radio?", "id": "Menerima Gelombang Siaran Radio." },
  { "en": "Apa Itu Antena Sirip Hiu?", "id": "Antena Modern Di Atap." },
  { "en": "Apa Itu Digital To Analog Converter (DAC)?", "id": "Mengubah Sinyal Audio Digital." },
  { "en": "Apa Itu Pre-Amplifier?", "id": "Menguatkan Sinyal Audio Lemah." },
  { "en": "Apa Itu Power Amplifier?", "id": "Menguatkan Sinyal Untuk Menggerakkan." },
  { "en": "Apa Itu Kontrol Volume?", "id": "Menyesuaikan Kekerasaan Suara." },
  { "en": "Apa Itu Kontrol Bass?", "id": "Menyesuaikan Frekuensi Rendah." },
  { "en": "Apa Itu Kontrol Treble?", "id": "Menyesuaikan Frekuensi Tinggi." },
  { "en": "Apa Itu Kontrol Balance?", "id": "Menyesuaikan Volume Antara Kiri." },
  { "en": "Apa Itu Kontrol Fader?", "id": "Menyesuaikan Volume Antara Depan." },
  { "en": "Apa Itu Sistem Navigasi?", "id": "Menyediakan Peta Dan Arah." },
  { "en": "Apa Itu Layar Sentuh?", "id": "Antarmuka Pengguna Untuk Sistem." },
  { "en": "Apa Itu Kontrol Suara?", "id": "Mengoperasikan Sistem Dengan Perintah." },
  { "en": "Apa Itu Vehicle-to-Vehicle (V2V) Communication?", "id": "Komunikasi Antar Kendaraan." },
  { "en": "Apa Itu Vehicle-to-Infrastructure (V2I) Communication?", "id": "Komunikasi Antara Kendaraan Dan." },
  { "en": "Apa Itu V2X (Vehicle-to-Everything)?", "id": "Menggabungkan Semua Bentuk Komunikasi." },
  { "en": "Apa Itu Sistem Operasi Real-Time (RTOS)?", "id": "Sistem Operasi Untuk ECU." },
  { "en": "Apa Itu AUTOSAR (Automotive Open System Architecture)?", "id": "Standar Arsitektur Perangkat Lunak." },
  { "en": "Apa Itu CAN FD (Controller Area Network Flexible Data-Rate)?", "id": "Versi CAN Dengan Kecepatan." },
  { "en": "Apa Itu Jaringan Berbasis Waktu?", "id": "Jaringan Dengan Sinkronisasi Waktu." },
  { "en": "Apa Itu Time-Triggered Ethernet?", "id": "Ethernet Deterministik." },
  { "en": "Apa Itu Deterministik?", "id": "Perilaku Sistem Dapat Diprediksi." },
  { "en": "Apa Itu Sensor Posisi?", "id": "Mengukur Lokasi Atau Sudut." },
  { "en": "Apa Itu Sensor Kecepatan?", "id": "Mengukur Seberapa Cepat Sesuatu." },
  { "en": "Apa Itu Sensor Percepatan?", "id": "Mengukur Laju Perubahan Kecepatan." },
  { "en": "Apa Itu Sensor Tekanan?", "id": "Mengukur Gaya Per Satuan." },
  { "en": "Apa Itu Sensor Suhu?", "id": "Mengukur Panas Atau Dingin." },
  { "en": "Apa Itu Sensor Aliran?", "id": "Mengukur Laju Aliran Fluida." },
  { "en": "Apa Itu Sensor Kimia?", "id": "Mendeteksi Kehadiran Zat Kimia." },
  { "en": "Apa Itu Sensor Optik?", "id": "Menggunakan Cahaya Untuk Mendeteksi." },
  { "en": "Apa Itu Sensor Magnetik?", "id": "Mendeteksi Medan Magnet." },
  { "en": "Apa Itu Efek Hall?", "id": "Prinsip Kerja Sensor Magnetik." },
  { "en": "Apa Itu Magnetoresistive Sensor?", "id": "Sensor Yang Resistansinya Berubah." },
  { "en": "Apa Itu Sensor Inersia?", "id": "Mengukur Percepatan Dan Rotasi." },
  { "en": "Apa Itu Inertial Measurement Unit (IMU)?", "id": "Menggabungkan Beberapa Sensor Inersia." },
  { "en": "Apa Itu Dead Reckoning?", "id": "Menghitung Posisi Berdasarkan Posisi." },
  { "en": "Apa Itu Kalibrasi Sensor?", "id": "Menyesuaikan Sensor Agar Akurat." },
  { "en": "Apa Itu Fusi Sensor?", "id": "Menggabungkan Data Dari Berbagai." },
  { "en": "Apa Itu Filter Kalman?", "id": "Algoritma Untuk Fusi Sensor." },
  { "en": "Apa Itu Motor DC (Direct Current)?", "id": "Motor Yang Bekerja Dengan Arus." },
  { "en": "Apa Itu Motor DC Sikat?", "id": "Menggunakan Sikat Untuk Komutasi." },
  { "en": "Apa Itu Motor DC Tanpa Sikat (BLDC)?", "id": "Komutasi Dilakukan Secara Elektronik." },
  { "en": "Apa Keuntungan Motor BLDC?", "id": "Lebih Andal Dan Efisien." },
  { "en": "Apa Itu Motor Stepper?", "id": "Motor Yang Bergerak Dalam Langkah." },
  { "en": "Apa Itu Motor Servo?", "id": "Motor Dengan Umpan Balik Posisi." },
  { "en": "Apa Itu Aktuator Hidrolik?", "id": "Menggunakan Tekanan Fluida." },
  { "en": "Apa Itu Aktuator Pneumatik?", "id": "Menggunakan Tekanan Udara." },
  { "en": "Apa Itu Aktuator Piezoelektrik?", "id": "Menggunakan Efek Piezoelektrik." },
  { "en": "Apa Itu Aktuator Shape Memory Alloy?", "id": "Logam Yang Mengingat Bentuknya." },
  { "en": "Apa Itu Modul Kontrol Pintu Geser?", "id": "Mengontrol Pintu Geser Otomatis." },
  { "en": "Apa Itu Sunroof?", "id": "Panel Kaca Di Atap." },
  { "en": "Apa Itu Modul Kontrol Sunroof?", "id": "Mengontrol Gerakan Sunroof." },
  { "en": "Apa Itu Sistem Peringatan Dini Tabrakan?", "id": "Nama Lain Forward Collision Warning." },
  { "en": "Apa Itu Sistem Pengereman Darurat Otonom?", "id": "Nama Lain Automatic Emergency Braking." },
  { "en": "Apa Itu Deteksi Kantuk Pengemudi?", "id": "Memantau Kewaspadaan Pengemudi." },
  { "en": "Apa Itu Peringatan Perhatian Pengemudi?", "id": "Memberi Peringatan Jika Pengemudi." },
  { "en": "Apa Itu Kamera Inframerah?", "id": "Digunakan Untuk Mendeteksi Kelelahan." },
  { "en": "Apa Itu Sistem Bantuan Penglihatan Malam?", "id": "Menampilkan Gambar Termal." },
  { "en": "Apa Itu Pemanasan Baterai?", "id": "Memanaskan Baterai Di Cuaca." },
  { "en": "Apa Itu Prekondisi Kabin?", "id": "Memanaskan Atau Mendinginkan Kabin." },
  { "en": "Apa Itu Pengisian Daya Cerdas?", "id": "Mengisi Daya Saat Harga Listrik." },
  { "en": "Apa Itu Vehicle-to-Grid (V2G)?", "id": "Mobil Memberi Daya Ke Jaringan." },
  { "en": "Apa Itu Vehicle-to-Home (V2H)?", "id": "Mobil Memberi Daya Pada Rumah." },
  { "en": "Apa Itu Vehicle-to-Load (V2L)?", "id": "Mobil Sebagai Stopkontak." },
  { "en": "Apa Itu Arus Sisa?", "id": "Arus Kecil Saat Perangkat." },
  { "en": "Apa Itu Parasitic Draw?", "id": "Arus Yang Menguras Baterai." },
  { "en": "Apa Itu Sensor Posisi Roda Gigi?", "id": "Mendeteksi Posisi Tuas Transmisi." },
  { "en": "Apa Itu Mode Parkir?", "id": "Mengunci Transmisi." },
  { "en": "Apa Itu Mode Netral?", "id": "Memutuskan Transmisi Dari Roda." },
  { "en": "Apa Itu Mode Mundur?", "id": "Menggerakkan Kendaraan Ke Belakang." },
  { "en": "Apa Itu Mode Berkendara?", "id": "Menggerakkan Kendaraan Ke Depan." },
  { "en": "Apa Itu Kunci Pintu Sentral?", "id": "Mengunci Semua Pintu Sekaligus." },
  { "en": "Apa Itu Sistem Anti-Pencurian?", "id": "Mencegah Pencurian Kendaraan." },
  { "en": "Apa Itu Alarm Mobil?", "id": "Menghasilkan Suara Keras." },
  { "en": "Apa Itu Sensor Kemiringan?", "id": "Mendeteksi Jika Mobil Didongkrak." },
  { "en": "Apa Itu Sensor Pecah Kaca?", "id": "Mendeteksi Suara Kaca Pecah." },
  { "en": "Apa Itu Pelacakan GPS (Global Positioning System)?", "id": "Memantau Lokasi Kendaraan." },
  { "en": "Apa Itu Geofencing?", "id": "Membuat Batas Virtual." },
  { "en": "Apa Itu Pembatas Kecepatan?", "id": "Membatasi Kecepatan Maksimum Kendaraan." },
  { "en": "Apa Itu Sistem Start-Stop?", "id": "Mematikan Mesin Saat Berhenti." },
  { "en": "Apa Itu Baterai AGM (Absorbent Glass Mat)?", "id": "Baterai Tahan Guncangan." },
  { "en": "Mengapa Sistem Start-Stop Membutuhkan Baterai Khusus?", "id": "Karena Siklus Start Yang Sering." },
  { "en": "Apa Itu Pendingin Udara (Intercooler)?", "id": "Mendinginkan Udara Dari Turbo." },
  { "en": "Apa Itu Katup Bypass Udara?", "id": "Mencegah Tekanan Berlebih Saat." },
  { "en": "Apa Itu Aktuator Katup Bypass?", "id": "Mengontrol Katup Bypass Udara." },
  { "en": "Apa Itu Sistem Kontrol Emisi?", "id": "Mengurangi Polutan Dari Gas Buang." },
  { "en": "Apa Itu Oksidasi Katalitik?", "id": "Mengubah Karbon Monoksida Dan." },
  { "en": "Apa Itu Konverter Tiga Arah?", "id": "Mengurangi Tiga Polutan Utama." },
  { "en": "Apa Tiga Polutan Tersebut?", "id": "CO, NOx, Dan Hidrokarbon." },
  { "en": "Apa Itu Pemanas Konverter Katalitik?", "id": "Mempercepat Pemanasan Konverter." },
  { "en": "Apa Itu Sensor Suhu Gas Buang?", "id": "Mengukur Suhu Gas Buang." },
  { "en": "Apa Itu EGR (Exhaust Gas Recirculation) Cooler?", "id": "Mendinginkan Gas Buang Sebelum." },
  { "en": "Apa Itu Sensor Tekanan Gas Buang?", "id": "Memantau Tekanan Dalam Sistem." },
  { "en": "Apa Itu Sistem Bahan Bakar Fleksibel?", "id": "Dapat Menggunakan Bensin Atau." },
  { "en": "Apa Itu Sensor Etanol?", "id": "Mengukur Kadar Etanol Dalam." },
  { "en": "Apa Itu Injektor Start Dingin?", "id": "Injektor Tambahan Untuk Kondisi." },
  { "en": "Apa Itu Sistem Intake Udara?", "id": "Menyalurkan Udara Ke Mesin." },
  { "en": "Apa Itu Filter Udara?", "id": "Menyaring Kotoran Dari Udara." },
  { "en": "Apa Itu Sensor Pembatas Udara?", "id": "Mendeteksi Jika Filter Udara." },
  { "en": "Apa Itu Resonator Intake?", "id": "Mengurangi Kebisingan Intake Udara." },
  { "en": "Apa Itu Variable Intake Manifold?", "id": "Mengubah Panjang Saluran Intake." },
  { "en": "Apa Tujuan Variable Intake Manifold?", "id": "Mengoptimalkan Torsi Pada Berbagai." },
  { "en": "Apa Itu Aktuator Intake Manifold Runner?", "id": "Mengontrol Katup Di Dalam." },
  { "en": "Apa Itu Sistem Kontrol Bodi?", "id": "Mengelola Fitur Kenyamanan Dan." },
  { "en": "Apa Itu Pintu Geser Daya?", "id": "Pintu Yang Membuka Dan Menutup." },
  { "en": "Apa Itu Motor Pintu Geser?", "id": "Motor Yang Menggerakkan Pintu." },
  { "en": "Apa Itu Modul Kontrol Pintu?", "id": "ECU Untuk Fungsi Pintu." },
  { "en": "Apa Itu Atap Konvertibel Daya?", "id": "Atap Yang Dapat Dilipat." },
  { "en": "Apa Itu Motor Atap Konvertibel?", "id": "Motor Untuk Melipat Atap." },
  { "en": "Apa Itu Sistem Hidrolik?", "id": "Menggunakan Fluida Bertekanan." },
  { "en": "Apa Itu Pompa Hidrolik?", "id": "Menghasilkan Tekanan Fluida." },
  { "en": "Apa Itu Silinder Hidrolik?", "id": "Aktuator Yang Digerakkan Fluida." },
  { "en": "Apa Itu Katup Solenoida Hidrolik?", "id": "Mengontrol Aliran Fluida Hidrolik." },
  { "en": "Apa Itu Suspensi Hidropneumatik?", "id": "Menggunakan Fluida Dan Gas." },
  { "en": "Apa Itu Sistem Anti-Maling?", "id": "Nama Lain Untuk Immobilizer." },
  { "en": "Apa Itu Sensor Ultrasonik Interior?", "id": "Mendeteksi Gerakan Di Dalam." },
  { "en": "Apa Itu Kunci Roda Kemudi?", "id": "Mekanisme Pengunci Mekanis." },
  { "en": "Apa Itu Kunci Roda Kemudi Elektronik?", "id": "Solenoida Yang Mengunci Kolom." },
  { "en": "Apa Itu Saklar Pengapian?", "id": "Saklar Yang Dioperasikan Kunci." },
  { "en": "Apa Itu Posisi ACC (Accessory)?", "id": "Menyalakan Radio Dan Aksesori." },
  { "en": "Apa Itu Posisi ON?", "id": "Menyalakan Semua Sistem." },
  { "en": "Apa Itu Posisi START?", "id": "Mengaktifkan Motor Starter." },
  { "en": "Apa Itu Sistem Pengisian Daya?", "id": "Menjaga Baterai Tetap Terisi." },
  { "en": "Apa Itu Regulator Tegangan?", "id": "Mengontrol Output Alternator." },
  { "en": "Apa Itu Penyearah Jembatan?", "id": "Mengubah AC Alternator Menjadi." },
  { "en": "Apa Itu Rotor?", "id": "Bagian Yang Berputar." },
  { "en": "Apa Itu Stator?", "id": "Bagian Yang Diam." },
  { "en": "Apa Itu Kumparan Medan?", "id": "Kumparan Di Rotor Alternator." },
  { "en": "Apa Itu Slip Ring?", "id": "Menyalurkan Arus Ke Kumparan." },
  { "en": "Apa Itu Sikat (Brush)?", "id": "Kontak Geser Pada Slip." },
  { "en": "Apa Itu Sirkuit Penginderaan Beban?", "id": "Memberi Tahu Regulator Tentang." },
  { "en": "Apa Itu Baterai?", "id": "Menyimpan Energi Kimia." },
  { "en": "Apa Itu Elektrolit?", "id": "Cairan Konduktif Dalam Baterai." },
  { "en": "Apa Itu Sel Baterai?", "id": "Unit Dasar Baterai." },
  { "en": "Apa Itu Ampere-Hour (Ah)?", "id": "Ukuran Kapasitas Baterai." },
  { "en": "Apa Itu Cold Cranking Amps (CCA)?", "id": "Kemampuan Start Di Suhu." },
  { "en": "Apa Itu Kabel Jumper?", "id": "Menghubungkan Dua Baterai." },
  { "en": "Apa Itu Polaritas?", "id": "Terminal Positif Dan Negatif." },
  { "en": "Apa Itu Korosi Terminal?", "id": "Penumpukan Putih Di Terminal." },
  { "en": "Apa Itu Sistem Pengapian?", "id": "Menciptakan Percikan Untuk Pembakaran." },
  { "en": "Apa Itu Koil Pengapian?", "id": "Transformator Step-Up." },
  { "en": "Apa Itu Modul Pengapian?", "id": "Mengontrol Waktu Koil." },
  { "en": "Apa Itu Waktu Diam (Dwell Time)?", "id": "Waktu Koil Diisi Energi." },
  { "en": "Apa Itu Sudut Diam?", "id": "Ukuran Waktu Diam Dalam." },
  { "en": "Apa Itu Titik Kontak?", "id": "Saklar Mekanis Dalam Sistem." },
  { "en": "Apa Itu Kondensor?", "id": "Mencegah Percikan Di Titik." },
  { "en": "Apa Itu Waktu Maju (Ignition Advance)?", "id": "Memajukan Waktu Percikan." },
  { "en": "Apa Itu Waktu Mundur (Ignition Retard)?", "id": "Memundurkan Waktu Percikan." },
  { "en": "Apa Itu Knock Sensor?", "id": "Mendeteksi Ketukan Mesin." },
  { "en": "Apa Itu Detonasi?", "id": "Pembakaran Abnormal." },
  { "en": "Apa Itu Pra-Pengapian?", "id": "Pengapian Sebelum Percikan Busi." },
  { "en": "Apa Itu Angka Oktan?", "id": "Ukuran Resistansi Bensin." },
  { "en": "Apa Itu Sensor Fleksibel Bahan Bakar?", "id": "Mengukur Kandungan Etanol." },
  { "en": "Apa Itu Sistem Hibrida Seri-Paralel?", "id": "Kombinasi Dari Keduanya." },
  { "en": "Apa Itu Planetary Gear Set?", "id": "Digunakan Dalam Transmisi Hibrida." },
  { "en": "Apa Itu Motor-Generator?", "id": "Dapat Berfungsi Sebagai Motor." },
  { "en": "Apa Itu Konverter DC/DC Dua Arah?", "id": "Arus Dapat Mengalir Dua Arah." },
  { "en": "Apa Itu Pemanas PTC (Positive Temperature Coefficient)?", "id": "Pemanas Kabin Untuk EV." },
  { "en": "Apa Itu Pemanas Resistif?", "id": "Elemen Pemanas Sederhana." },
  { "en": "Apa Itu Pompa Panas?", "id": "Sistem Pemanas Dan Pendingin." },
  { "en": "Apa Itu Vehicle-to-Everything (V2X)?", "id": "Mobil Berkomunikasi Dengan Lingkungannya." },
  { "en": "Apa Itu Komunikasi Jarak Pendek Khusus?", "id": "Standar Komunikasi Untuk V2X." },
  { "en": "Apa Itu Keamanan Siber?", "id": "Melindungi Sistem Dari Serangan." },
  { "en": "Apa Itu Firewall Jaringan?", "id": "Membatasi Akses Jaringan." },
  { "en": "Apa Itu Sistem Deteksi Intrusi?", "id": "Mendeteksi Aktivitas Mencurigakan." },
  { "en": "Apa Itu Pesan CAN Terotentikasi?", "id": "Mencegah Pesan Palsu." },
  { "en": "Apa Itu Gateway Aman?", "id": "Titik Akses Aman Ke Jaringan." },
  { "en": "Apa Itu Pembaruan Perangkat Lunak Aman?", "id": "Memastikan Pembaruan Asli." },
  { "en": "Apa Itu Bootloader Aman?", "id": "Memverifikasi Perangkat Lunak Saat." },
  { "en": "Apa Itu Lingkungan Eksekusi Terpercaya?", "id": "Area Aman Di Dalam Prosesor." },
  { "en": "Apa Itu Modul Keamanan Perangkat Keras?", "id": "Chip Khusus Untuk Kriptografi." },
  { "en": "Apa Itu Pengujian Penetrasi?", "id": "Mencoba Meretas Sistem." },
  { "en": "Apa Itu Fuzzing?", "id": "Memberi Input Acak Untuk." },
  { "en": "Apa Itu Kode Statis Analisis?", "id": "Menganalisis Kode Sumber." },
  { "en": "Apa Itu Arsitektur Perangkat Lunak?", "id": "Struktur Tingkat Tinggi." },
  { "en": "Apa Itu Model Lapisan OSI?", "id": "Model Konseptual Untuk Komunikasi Jaringan." },
  { "en": "Apa Itu Lapisan Fisik?", "id": "Mentransmisikan Bit Mentah." },
  { "en": "Apa Itu Lapisan Data Link?", "id": "Menyediakan Transfer Data Antar Node." },
  { "en": "Apa Itu Lapisan Jaringan?", "id": "Menangani Pengalamatan Dan Perutean." },
  { "en": "Apa Itu Lapisan Transport?", "id": "Menyediakan Transfer Data End-to-End." },
  { "en": "Apa Itu Lapisan Sesi?", "id": "Mengelola Sesi Komunikasi." },
  { "en": "Apa Itu Lapisan Presentasi?", "id": "Memformat Dan Mengenkripsi Data." },
  { "en": "Apa Itu Lapisan Aplikasi?", "id": "Antarmuka Untuk Aplikasi Pengguna." },
  { "en": "Apa Itu Diagnostik?", "id": "Proses Mengidentifikasi Masalah." },
  { "en": "Apa Itu Kode Kesiapan (Readiness Code)?", "id": "Menunjukkan Monitor Diagnostik Telah." },
  { "en": "Apa Itu Monitor Diagnostik?", "id": "Tes Internal Yang Dijalankan." },
  { "en": "Apa Itu Misfire Monitor?", "id": "Mendeteksi Kegagalan Pengapian Mesin." },
  { "en": "Apa Itu Fuel System Monitor?", "id": "Memantau Kinerja Sistem Bahan Bakar." },
  { "en": "Apa Itu Comprehensive Component Monitor?", "id": "Memantau Komponen Terkait Emisi." },
  { "en": "Apa Itu EGR (Exhaust Gas Recirculation) System Monitor?", "id": "Memantau Sistem Resirkulasi Gas." },
  { "en": "Apa Itu Oxygen Sensor Monitor?", "id": "Memantau Kinerja Sensor Oksigen." },
  { "en": "Apa Itu Catalyst Monitor?", "id": "Memantau Efisiensi Konverter Katalitik." },
  { "en": "Apa Itu Heated Catalyst Monitor?", "id": "Memantau Pemanas Konverter Katalitik." },
  { "en": "Apa Itu EVAP (Evaporative Emission) System Monitor?", "id": "Memantau Sistem Kontrol Uap." },
  { "en": "Apa Itu Secondary Air System Monitor?", "id": "Memantau Sistem Injeksi Udara." },
  { "en": "Apa Itu A/C (Air Conditioning) System Refrigerant Monitor?", "id": "Memantau Refrigeran AC." },
  { "en": "Apa Itu Oxygen Sensor Heater Monitor?", "id": "Memantau Pemanas Sensor Oksigen." },
  { "en": "Apa Itu Mode $01?", "id": "Meminta Data Diagnostik Saat." },
  { "en": "Apa Itu Mode $02?", "id": "Meminta Data Freeze Frame." },
  { "en": "Apa Itu Mode $03?", "id": "Meminta Kode Kesalahan Emisi." },
  { "en": "Apa Itu Mode $04?", "id": "Menghapus Kode Kesalahan Emisi." },
  { "en": "Apa Itu Mode $05?", "id": "Meminta Hasil Tes Sensor Oksigen." },
  { "en": "Apa Itu Mode $06?", "id": "Meminta Hasil Tes Monitor." },
  { "en": "Apa Itu Mode $07?", "id": "Meminta Kode Kesalahan Emisi." },
  { "en": "Apa Itu Mode $08?", "id": "Meminta Kontrol Sistem On-Board." },
  { "en": "Apa Itu Mode $09?", "id": "Meminta Informasi Kendaraan." },
  { "en": "Apa Itu Mode $0A?", "id": "Meminta Kode Kesalahan Permanen." },
  { "en": "Apa Itu Parameter ID (PID)?", "id": "Pengenal Untuk Data Tertentu." },
  { "en": "Apa Itu Sensor Pijar?", "id": "Nama Lain Untuk Glow Plug." },
  { "en": "Apa Itu Relai Pijar?", "id": "Mengontrol Daya Ke Glow Plug." },
  { "en": "Apa Itu Injektor Unit Elektronik?", "id": "Injektor Dan Pompa Terintegrasi." },
  { "en": "Apa Itu Sistem Injeksi Tekanan Tinggi?", "id": "Digunakan Pada Mesin Diesel Modern." },
  { "en": "Apa Itu Sensor Air-dalam-Bahan Bakar?", "id": "Mendeteksi Air Di Filter." },
  { "en": "Apa Itu Pemanas Bahan Bakar?", "id": "Mencegah Pembekuan Bahan Bakar." },
  { "en": "Apa Itu Transmisi Otomatis?", "id": "Berpindah Gigi Secara Otomatis." },
  { "en": "Apa Itu Fluida Transmisi Otomatis (ATF)?", "id": "Minyak Khusus Untuk Transmisi." },
  { "en": "Apa Itu Torque Converter?", "id": "Kopling Fluida Antara Mesin." },
  { "en": "Apa Itu Planetary Gear Set?", "id": "Mekanisme Roda Gigi Dalam." },
  { "en": "Apa Itu Valve Body?", "id": "Pusat Kontrol Hidrolik." },
  { "en": "Apa Itu Solenoida Kontrol?", "id": "Mengontrol Aliran Fluida." },
  { "en": "Apa Itu Sensor Kecepatan Turbin?", "id": "Mengukur Kecepatan Input Transmisi." },
  { "en": "Apa Itu Sensor Kecepatan Output?", "id": "Mengukur Kecepatan Output Transmisi." },
  { "en": "Apa Itu Program Pindah Gigi?", "id": "Logika Yang Menentukan Kapan." },
  { "en": "Apa Itu Mode Sport?", "id": "Program Pindah Gigi Agresif." },
  { "en": "Apa Itu Mode Ekonomi?", "id": "Program Pindah Gigi Efisien." },
  { "en": "Apa Itu Mode Manual?", "id": "Pengemudi Mengontrol Pindah Gigi." },
  { "en": "Apa Itu Adaptive Shift Logic?", "id": "Logika Pindah Gigi Belajar." },
  { "en": "Apa Itu Hill Holder?", "id": "Mencegah Mundur Di Tanjakan." },
  { "en": "Apa Itu Downhill Assist Control?", "id": "Membantu Mengontrol Kecepatan Saat." },
  { "en": "Apa Itu Sistem Penggerak Empat Roda?", "id": "Tenaga Ke Empat Roda." },
  { "en": "Apa Itu Transfer Case?", "id": "Membagi Tenaga Ke As Roda." },
  { "en": "Apa Itu Mode 4-High?", "id": "Penggerak Empat Roda Untuk." },
  { "en": "Apa Itu Mode 4-Low?", "id": "Rasio Gigi Rendah Untuk." },
  { "en": "Apa Itu Aktuator Transfer Case?", "id": "Motor Untuk Mengubah Mode." },
  { "en": "Apa Itu Diferensial?", "id": "Memungkinkan Roda Berputar." },
  { "en": "Apa Itu Limited-Slip Differential (LSD)?", "id": "Membatasi Perbedaan Kecepatan Roda." },
  { "en": "Apa Itu Locking Differential?", "id": "Mengunci Kedua Roda Bersama." },
  { "en": "Apa Itu Torque Vectoring?", "id": "Mengontrol Torsi Ke Setiap." },
  { "en": "Apa Itu Sistem Audio?", "id": "Menghasilkan Suara." },
  { "en": "Apa Itu Antena?", "id": "Menerima Sinyal Radio." },
  { "en": "Apa Itu Amplifier?", "id": "Menguatkan Sinyal Audio." },
  { "en": "Apa Itu Speaker?", "id": "Mengubah Sinyal Listrik." },
  { "en": "Apa Itu Crossover?", "id": "Membagi Frekuensi Audio." },
  { "en": "Apa Itu Konektivitas Nirkabel?", "id": "Seperti Bluetooth Dan Wi-Fi." },
  { "en": "Apa Itu Sistem Navigasi?", "id": "Menentukan Posisi Dan Arah." },
  { "en": "Apa Itu Dead Reckoning?", "id": "Menghitung Posisi Berdasarkan Kecepatan." },
  { "en": "Apa Itu Sistem Keamanan?", "id": "Mencegah Pencurian Atau Akses." },
  { "en": "Apa Itu Alarm?", "id": "Menghasilkan Suara Peringatan." },
  { "en": "Apa Itu Immobilizer?", "id": "Mencegah Mesin Hidup." },
  { "en": "Apa Itu Remote Start?", "id": "Menghidupkan Mesin Dari Jarak." },
  { "en": "Apa Itu Sistem Listrik?", "id": "Menghasilkan, Menyimpan, Mendistribusikan." },
  { "en": "Apa Itu Baterai?", "id": "Menyimpan Energi Kimia." },
  { "en": "Apa Itu Alternator?", "id": "Menghasilkan Listrik Saat Mesin." },
  { "en": "Apa Itu Motor Starter?", "id": "Memutar Mesin Untuk Start." },
  { "en": "Apa Itu Wiring Harness?", "id": "Kumpulan Kabel." },
  { "en": "Apa Itu Kotak Sekering?", "id": "Menampung Sekering Dan Relai." },
  { "en": "Apa Itu Titik Ground?", "id": "Koneksi Ke Sasis." },
  { "en": "Apa Itu Sensor?", "id": "Mengukur Besaran Fisik." },
  { "en": "Apa Itu Aktuator?", "id": "Menghasilkan Gerakan Atau Aksi." },
  { "en": "Apa Itu Modul Kontrol?", "id": "Unit Komputer Kecil." },
  { "en": "Apa Itu Jaringan Kendaraan?", "id": "Komunikasi Antar Modul." },
  { "en": "Apa Itu Protokol Komunikasi?", "id": "Aturan Untuk Pertukaran Data." },
  { "en": "Apa Itu Gateway?", "id": "Menerjemahkan Antar Jaringan." },
  { "en": "Apa Itu Perangkat Lunak Diagnostik?", "id": "Digunakan Untuk Mendiagnosis Masalah." },
  { "en": "Apa Itu Pemrograman Ulang (Flashing)?", "id": "Memperbarui Perangkat Lunak." },
  { "en": "Apa Itu Kalibrasi?", "id": "Menyesuaikan Parameter Sistem." },
  { "en": "Apa Itu Kendaraan Otonom?", "id": "Kendaraan Yang Dapat Mengemudi." },
  { "en": "Apa Itu Level Otonomi?", "id": "Tingkat Kemampuan Mengemudi." },
  { "en": "Apa Itu Fusi Sensor?", "id": "Menggabungkan Data Dari Sensor." },
  { "en": "Apa Itu Pengenalan Objek?", "id": "Mengidentifikasi Objek Di Jalan." },
  { "en": "Apa Itu Perencanaan Jalur?", "id": "Menentukan Jalur Yang Aman." },
  { "en": "Apa Itu Kontrol Kendaraan?", "id": "Mengoperasikan Kemudi, Gas, Rem." },
  { "en": "Apa Itu Redundansi?", "id": "Memiliki Sistem Cadangan." },
  { "en": "Apa Itu Failsafe?", "id": "Sistem Gagal Dalam Mode." },
  { "en": "Apa Itu Fail-Operational?", "id": "Sistem Terus Berfungsi." }



        ];

        let questions = [];

        rawVocabularyList.sort((a, b) => {
            const enA = a.en.toLowerCase();
            const enB = b.en.toLowerCase();
            if (enA < enB) return -1;
            if (enA > enB) return 1;
            return 0;
        });

        function generateQuestions() {
            const allIndonesianTranslations = rawVocabularyList.map(item => item.id);
            questions = [];
            rawVocabularyList.forEach(vocabItem => {
                const correctAnswer = vocabItem.id;
                const distractors = [];
                let attempts = 0;
                while (distractors.length < 3 && attempts < allIndonesianTranslations.length * 2) {
                    const randomIndex = Math.floor(Math.random() * allIndonesianTranslations.length);
                    const potentialDistractor = allIndonesianTranslations[randomIndex];
                    if (potentialDistractor !== correctAnswer && !distractors.includes(potentialDistractor)) {
                        distractors.push(potentialDistractor);
                    }
                    attempts++;
                }
                while (distractors.length < 3) {
                    const fallbackOptions = ["opsi lain A", "opsi lain B", "opsi lain C", "opsi lain D", "opsi lain E", "opsi lain F"];
                    let fallbackIndex = 0;
                    let safetyNet = 0;
                    while(distractors.length < 3 && safetyNet < fallbackOptions.length * 3) {
                        const fbOption = fallbackOptions[fallbackIndex % fallbackOptions.length] + `_${distractors.length}${Math.floor(Math.random()*100)}`;
                        if (fbOption !== correctAnswer && !distractors.includes(fbOption)) {
                             distractors.push(fbOption);
                        }
                        fallbackIndex++;
                        safetyNet++;
                    }
                     if(distractors.length < 3) {
                        for(let i=0; i < (3-distractors.length); i++){
                            distractors.push("pilihan default " + (i+1+distractors.length) + Math.random().toString(36).substring(7));
                        }
                     }
                }
                const answerOptions = [
                    { text: correctAnswer, correct: true },
                    { text: distractors[0], correct: false },
                    { text: distractors[1], correct: false },
                    { text: distractors[2], correct: false }
                ];
                questions.push({
                    question: vocabItem.en,
                    answers: answerOptions
                });
            });
        }

        generateQuestions();

        function saveProgress() {
            if (!questionContainerElement.classList.contains('hide') && orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                 const progress = {
                    currentQuestionIndex: currentQuestionIndex,
                    score: score,
                    orderedQuestions: orderedQuestions
                };
                localStorage.setItem('quizProgress', JSON.stringify(progress));
            }
        }

        function loadProgress() {
            const savedProgress = localStorage.getItem('quizProgress');
            if (savedProgress) {
                try {
                    const progressData = JSON.parse(savedProgress);
                    if (progressData && typeof progressData.currentQuestionIndex === 'number' &&
                        typeof progressData.score === 'number' && Array.isArray(progressData.orderedQuestions) &&
                        progressData.orderedQuestions.length > 0 &&
                        progressData.currentQuestionIndex < progressData.orderedQuestions.length &&
                        progressData.orderedQuestions.length === questions.length) { // Validasi tambahan: jumlah soal harus sama
                        return progressData;
                    } else {
                        clearProgress();
                        return null;
                    }
                } catch (e) {
                    console.error("Error parsing saved progress:", e);
                    clearProgress();
                    return null;
                }
            }
            return null;
        }

        function clearProgress() {
            localStorage.removeItem('quizProgress');
        }

        prev50Button.addEventListener('click', () => navigateQuestions(-JUMP_AMOUNT));
        prevQuestionButton.addEventListener('click', () => navigateQuestions(-1)); // Event listener untuk tombol baru
        next50Button.addEventListener('click', () => navigateQuestions(JUMP_AMOUNT));

        function navigateQuestions(amount) {
            clearTimeout(questionTimeout);
            if (!orderedQuestions || orderedQuestions.length === 0) return;

            let newIndex = currentQuestionIndex + amount;
            if (newIndex < 0) newIndex = 0;
            else if (newIndex >= orderedQuestions.length) newIndex = orderedQuestions.length - 1;

            if (newIndex !== currentQuestionIndex) {
                currentQuestionIndex = newIndex;
                setNextQuestion();
            } else {
                updateSkipButtonStates();
            }
        }

        function updateSkipButtonStates() {
            if (!orderedQuestions || orderedQuestions.length === 0 || questionContainerElement.classList.contains('hide')) {
                skipNavigationControls.classList.add('hide');
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Nonaktifkan tombol baru
                if(next50Button) next50Button.disabled = true;
                return;
            }
            skipNavigationControls.classList.remove('hide');
            const isFirstQuestion = currentQuestionIndex === 0;
            const isLastQuestion = currentQuestionIndex === (orderedQuestions.length - 1);

            if(prev50Button) prev50Button.disabled = isFirstQuestion;
            if(prevQuestionButton) prevQuestionButton.disabled = isFirstQuestion; // Atur status disabled tombol baru
            if(next50Button) next50Button.disabled = isLastQuestion;

            if (orderedQuestions.length <= 1) {
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Atur status disabled tombol baru
                if(next50Button) next50Button.disabled = true;
            }
        }


        window.addEventListener('load', () => {
            const savedData = loadProgress();
            startButton.innerText = 'Mulai';
            completionMessageElement.classList.add('hide');
            if (savedData) {
                continueButton.classList.remove('hide');
            } else {
                continueButton.classList.add('hide');
            }
            if (questionContainerElement.classList.contains('hide')) {
                initialControls.classList.remove('hide');
                skipNavigationControls.classList.add('hide');
            } else {
                 initialControls.classList.add('hide');
                 // Mungkin juga perlu updateSkipButtonStates() di sini jika kuis dilanjutkan
                 // dan langsung menampilkan soal.
            }
        });

        startButton.addEventListener('click', () => startGame(false));
        continueButton.addEventListener('click', () => startGame(true));

        function startGame(isContinuing = false) {
            clearTimeout(questionTimeout);
            completionMessageElement.classList.add('hide');
            if (!isContinuing) {
                startButton.innerText = 'Mulai';
            }
            initialControls.classList.add('hide');
            questionContainerElement.classList.remove('hide');
            questionCounterElement.classList.remove('hide');

            const savedData = loadProgress();
            if (isContinuing && savedData && savedData.orderedQuestions && savedData.orderedQuestions.length === questions.length) {
                orderedQuestions = savedData.orderedQuestions;
                currentQuestionIndex = savedData.currentQuestionIndex;
                score = savedData.score;
            } else {
                clearProgress();
                orderedQuestions = [...questions];
                currentQuestionIndex = 0;
                score = 0;
            }

            if (!orderedQuestions || orderedQuestions.length === 0) {
                showResults();
                completionMessageElement.innerText = "Tidak ada soal untuk ditampilkan.";
                completionMessageElement.style.color = "#dc3545";
                completionMessageElement.classList.remove('hide');
                startButton.innerText = 'Mulai';
                return;
            }
            setNextQuestion();
        }

        function setNextQuestion() {
            resetState();
            if (orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                questionCounterElement.innerText = `${currentQuestionIndex + 1} / ${orderedQuestions.length}`;
                showQuestion(orderedQuestions[currentQuestionIndex]);
                saveProgress();
                if (document.activeElement && typeof document.activeElement.blur === 'function') {
                    document.activeElement.blur();
                }
            } else {
                showResults();
            }
            updateSkipButtonStates(); // Panggil di sini untuk memastikan state tombol selalu update
        }

        function showQuestion(questionData) {
            questionElement.innerText = questionData.question;
            answerButtonsElement.innerHTML = '';
            const shuffledAnswers = [...questionData.answers].sort(() => Math.random() - 0.5);
            shuffledAnswers.forEach(answer => {
                const button = document.createElement('button');
                button.innerText = answer.text;
                button.classList.add('btn');
                if (answer.correct) {
                    button.dataset.correct = answer.correct;
                }
                button.addEventListener('click', selectAnswer);
                answerButtonsElement.appendChild(button);
            });
        }

        function resetState() {
            clearTimeout(questionTimeout);
            while (answerButtonsElement.firstChild) {
                answerButtonsElement.removeChild(answerButtonsElement.firstChild);
            }
        }

        function selectAnswer(e) {
            const selectedButton = e.target;
            const correct = selectedButton.dataset.correct === 'true';
            if (correct) { score++; }
            Array.from(answerButtonsElement.children).forEach(button => {
                setStatusClass(button, button.dataset.correct === 'true');
                button.disabled = true;
            });
            saveProgress();
            questionTimeout = setTimeout(() => {
                if (orderedQuestions && currentQuestionIndex < orderedQuestions.length -1) {
                    currentQuestionIndex++;
                    setNextQuestion();
                } else if (orderedQuestions && currentQuestionIndex === orderedQuestions.length -1) {
                    showResults();
                }
            }, 7000);
        }

        function setStatusClass(element, correct) {
            clearStatusClass(element);
            if (correct) { element.classList.add('correct'); }
            else { element.classList.add('wrong'); }
        }

        function clearStatusClass(element) {
            element.classList.remove('correct');
            element.classList.remove('wrong');
        }

        function showResults() {
            clearTimeout(questionTimeout);
            questionContainerElement.classList.add('hide');
            questionCounterElement.classList.add('hide');
            skipNavigationControls.classList.add('hide');
            clearProgress();
            completionMessageElement.innerText = "Selamat Kuis Sudah Selesai 🎉";
            completionMessageElement.style.color = "#28a745";
            completionMessageElement.classList.remove('hide');
            startButton.innerText = 'Ulangi Kuis';
            initialControls.classList.remove('hide');
            continueButton.classList.add('hide');
        }
    </script>
</body>
</html>
