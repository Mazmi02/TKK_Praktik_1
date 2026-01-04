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


  { "en": "Apa Fungsi Tools Line Pada AutoCAD (Computer Aided Design)?", "id": "Membuat Garis Lurus." },
  { "en": "Sebutkan Shortcut Save Pada Arduino IDE (Integrated Development Environment)?", "id": "Tekan Ctrl + S." },
  { "en": "Apa Kegunaan Resistor Pada Simulasi Proteus (Processor for Text)?", "id": "Menghambat Arus Listrik." },
  { "en": "Bagaimana Cara Copy Objek Di CX Programmer (Omron)?", "id": "Tekan Ctrl + C." },
  { "en": "Apa Fungsi Coil Pada Ladder Diagram GX Works?", "id": "Output Logika Program." },
  { "en": "Apa Shortcut Undo Pada Aplikasi AutoCAD (Computer Aided Design)?", "id": "Tekan Ctrl + Z." },
  { "en": "Apa Fungsi Library Pada Arduino IDE (Integrated Development Environment)?", "id": "Kumpulan Kode Siap Pakai." },
  { "en": "Komponen Apa Untuk Mengukur Tegangan Di Proteus?", "id": "DC Voltmeter Atau AC Voltmeter." },
  { "en": "Apa Instruksi Timer Pada GX Works 2 (Mitsubishi)?", "id": "Menunda Waktu Eksekusi." },
  { "en": "Bagaimana Cara Membuat Lingkaran Di AutoCAD (Computer Aided Design)?", "id": "Ketik Circle Lalu Enter." },
  { "en": "Apa Fungsi Serial Monitor Pada Arduino IDE?", "id": "Menampilkan Data Komunikasi Serial." },
  { "en": "Apa Guna Ground Pada Rangkaian Simulasi Proteus?", "id": "Titik Referensi Nol Volt." },
  { "en": "Shortcut Apa Untuk Compile Di Arduino IDE?", "id": "Tekan Ctrl + R." },
  { "en": "Apa Itu NC (Normally Closed) Pada Kontak Relay?", "id": "Kontak Tertutup Saat Diam." },
  { "en": "Apa Fungsi Perintah Trim Pada AutoCAD (Computer Aided Design)?", "id": "Memotong Garis Bersinggungan." },
  { "en": "Bagaimana Cara Menambah Device Baru Di GX Works 3?", "id": "Pilih Project Lalu New." },
  { "en": "Apa Warna Kabel Netral Standar PUIL (Persyaratan Umum)?", "id": "Berwarna Biru." },
  { "en": "Apa Fungsi Void Setup Pada Coding Arduino?", "id": "Eksekusi Program Satu Kali." },
  { "en": "Alat Apa Untuk Simulasi Osiloskop Di Proteus?", "id": "Virtual Oscilloscope." },
  { "en": "Apa Shortcut Paste Pada CX Programmer?", "id": "Tekan Ctrl + V." },
  { "en": "Apa Fungsi NO (Normally Open) Pada Push Button?", "id": "Terhubung Saat Ditekan." },
  { "en": "Perintah Apa Untuk Menghapus Objek Di AutoCAD?", "id": "Ketik Erase Lalu Enter." },
  { "en": "Apa Ekstensi File Project Pada GX Works 2?", "id": "Format Gxw." },
  { "en": "Apa Fungsi Pin Digital Pada Papan Arduino Uno?", "id": "Input Output Sinyal Digital." },
  { "en": "Bagaimana Cara Rotasi Komponen Di Proteus?", "id": "Tekan Tombol Plus Keypad." },
  { "en": "Apa Shortcut Select All Pada AutoCAD (Computer Aided Design)?", "id": "Tekan Ctrl + A." },
  { "en": "Apa Fungsi Void Loop Pada Sketsa Arduino?", "id": "Eksekusi Program Berulang." },
  { "en": "Komponen Apa Yang Menyimpan Muatan Listrik Di Proteus?", "id": "Komponen Kapasitor." },
  { "en": "Apa Arti Mnemonic LD Pada Pemrograman PLC?", "id": "Load Contact Open." },
  { "en": "Bagaimana Cara Mirror Objek Di AutoCAD (Computer Aided Design)?", "id": "Ketik Mirror Lalu Enter." },
  { "en": "Apa Fungsi Breadboard Pada Simulasi Elektronika?", "id": "Tempat Merangkai Komponen Sementara." },
  { "en": "Shortcut Apa Untuk Upload Code Arduino?", "id": "Tekan Ctrl + U." },
  { "en": "Apa Fungsi Relay Pada Sistem Kendali Otomatis?", "id": "Saklar Elektromagnetik." },
  { "en": "Bagaimana Cara Zoom Extents Di AutoCAD?", "id": "Klik Dua Kali Scroll Mouse." },
  { "en": "Apa Fungsi Comment Pada Coding Arduino IDE?", "id": "Catatan Tidak Dieksekusi." },
  { "en": "Apa Itu LED (Light Emitting Diode) Pada Proteus?", "id": "Dioda Pemancar Cahaya." },
  { "en": "Apa Fungsi Counter Pada Logika PLC (Programmable Logic)?", "id": "Menghitung Jumlah Input." },
  { "en": "Shortcut Apa Untuk Print Di AutoCAD (Computer Aided Design)?", "id": "Tekan Ctrl + P." },
  { "en": "Tipe Data Apa Untuk Bilangan Bulat Di Arduino?", "id": "Tipe Data Integer." },
  { "en": "Bagaimana Cara Menghubungkan Jalur Di Proteus?", "id": "Klik Ujung Kaki Komponen." },
  { "en": "Apa Fungsi Perintah Offset Pada AutoCAD?", "id": "Duplikasi Garis Sejajar." },
  { "en": "Apa Perbedaan GX Works 2 Dan GX Works 3?", "id": "Versi Dan Fitur Perangkat." },
  { "en": "Apa Fungsi Pin Analog Pada Arduino Uno?", "id": "Membaca Sinyal Analog." },
  { "en": "Komponen Apa Penyearah Arus Di Proteus?", "id": "Komponen Dioda." },
  { "en": "Apa Instruksi Output Pada CX Programmer?", "id": "Instruksi OUT." },
  { "en": "Bagaimana Cara Memindahkan Objek Di AutoCAD?", "id": "Ketik Move Lalu Enter." },
  { "en": "Apa Fungsi Baud Rate Pada Komunikasi Serial?", "id": "Kecepatan Transfer Data." },
  { "en": "Apa Itu VCC (Voltage Common Collector) Di Rangkaian?", "id": "Sumber Tegangan Positif." },
  { "en": "Shortcut Apa Untuk New File Di Arduino?", "id": "Tekan Ctrl + N." },
  { "en": "Apa Fungsi Ladder Diagram Pada PLC (Programmable Logic)?", "id": "Bahasa Pemrograman Grafis." },
  { "en": "Bagaimana Cara Membuat Persegi Di AutoCAD?", "id": "Ketik Rectang Lalu Enter." },
  { "en": "Apa Fungsi Variabel Pada Pemrograman Arduino?", "id": "Menyimpan Nilai Data." },
  { "en": "Apa Komponen Penguat Sinyal Di Proteus?", "id": "Transistor Bipolar." },
  { "en": "Apa Fungsi Kontak OR Pada Logika PLC?", "id": "Logika Penjumlahan Paralel." },
  { "en": "Shortcut Apa Untuk Open File Di AutoCAD?", "id": "Tekan Ctrl + O." },
  { "en": "Apa Fungsi Delay Pada Program Arduino?", "id": "Jeda Waktu Eksekusi." },
  { "en": "Apa Itu PCB (Printed Circuit Board) Layout?", "id": "Tata Letak Jalur Tembaga." },
  { "en": "Bagaimana Cara Simulasi Run Di GX Works 2?", "id": "Klik Start Simulation." },
  { "en": "Apa Fungsi Command Fillet Pada AutoCAD?", "id": "Melengkungkan Sudut Garis." },
  { "en": "Apa Tipe Data Untuk Karakter Di Arduino?", "id": "Tipe Data Char." },
  { "en": "Apa Fungsi Trafo (Transformator) Di Proteus?", "id": "Mengubah Nilai Tegangan." },
  { "en": "Apa Instruksi End Pada CX Programmer?", "id": "Akhir Program PLC." },
  { "en": "Shortcut Apa Untuk Cut Di AutoCAD?", "id": "Tekan Ctrl + X." },
  { "en": "Apa Fungsi PWM (Pulse Width Modulation) Arduino?", "id": "Mengatur Tegangan Output Analog." },
  { "en": "Apa Itu 7 Segment (Seven Segment) Display?", "id": "Penampil Angka Digital." },
  { "en": "Bagaimana Cara Insert Teks Di AutoCAD?", "id": "Ketik Mtext Lalu Enter." },
  { "en": "Apa Fungsi Input Pullup Pada Arduino?", "id": "Mengaktifkan Resistor Internal." },
  { "en": "Komponen Apa Pembangkit Frekuensi Di Proteus?", "id": "Signal Generator." },
  { "en": "Apa Fungsi Memory Bit Di PLC (Programmable Logic)?", "id": "Menyimpan Status Sementara." },
  { "en": "Bagaimana Cara Mengukur Dimensi Di AutoCAD?", "id": "Gunakan Tools Dimension." },
  { "en": "Apa Fungsi If Else Pada Coding Arduino?", "id": "Percabangan Logika Kondisi." },
  { "en": "Apa Itu LCD (Liquid Crystal Display) 16x2?", "id": "Layar Penampil Karakter." },
  { "en": "Shortcut Apa Untuk Find Di Arduino IDE?", "id": "Tekan Ctrl + F." },
  { "en": "Apa Fungsi Transistor NPN (Negative Positive Negative)?", "id": "Saklar Aktif High." },
  { "en": "Bagaimana Cara Membuat Busur Di AutoCAD?", "id": "Ketik Arc Lalu Enter." },
  { "en": "Apa Fungsi Define Pada Header Arduino?", "id": "Mendefinisikan Konstanta." },
  { "en": "Apa Komponen Pengatur Tegangan Tetap Di Proteus?", "id": "IC Regulator Voltage." },
  { "en": "Apa Fungsi Holding Relay Pada Rangkaian Kontrol?", "id": "Mengunci Status Aktif." },
  { "en": "Shortcut Apa Untuk Redo Di AutoCAD?", "id": "Tekan Ctrl + Y." },
  { "en": "Apa Fungsi Serial Println Di Arduino?", "id": "Cetak Data Baris Baru." },
  { "en": "Apa Itu MOSFET (Metal Oxide Semiconductor) Di Proteus?", "id": "Transistor Efek Medan." },
  { "en": "Bagaimana Cara Transfer Program Ke PLC Omron?", "id": "Pilih PLC Transfer To." },
  { "en": "Apa Fungsi Command Extend Pada AutoCAD?", "id": "Memperpanjang Garis." },
  { "en": "Apa Tipe Data True False Di Arduino?", "id": "Tipe Data Boolean." },
  { "en": "Apa Fungsi Optocoupler Pada Rangkaian Interface?", "id": "Pemisah Sinyal Cahaya." },
  { "en": "Shortcut Apa Untuk Go To Line Arduino?", "id": "Tekan Ctrl + L." },
  { "en": "Apa Fungsi Timer On Delay (TON) PLC?", "id": "Hitung Waktu Saat On." },
  { "en": "Bagaimana Cara Explode Blok Di AutoCAD?", "id": "Ketik Explode Lalu Enter." },
  { "en": "Apa Fungsi For Loop Pada Arduino?", "id": "Perulangan Dengan Batasan." },
  { "en": "Komponen Apa Deteksi Suhu Di Proteus?", "id": "Sensor LM35." },
  { "en": "Apa Fungsi Marker Pada CX Programmer?", "id": "Relay Bantu Internal." },
  { "en": "Shortcut Apa Untuk Help Di AutoCAD?", "id": "Tekan Tombol F1." },
  { "en": "Apa Fungsi Analog Read Di Arduino?", "id": "Baca Nilai Pin Analog." },
  { "en": "Apa Itu Relay SPDT (Single Pole Double Throw)?", "id": "Satu Induk Dua Cabang." },
  { "en": "Bagaimana Cara Scale Objek Di AutoCAD?", "id": "Ketik Scale Lalu Enter." },
  { "en": "Apa Fungsi Digital Write Pada Arduino?", "id": "Kirim Sinyal High Low." },
  { "en": "Apa Fungsi Fuse (Sekering) Pada Rangkaian?", "id": "Pengaman Arus Berlebih." },
  { "en": "Apa Instruksi Keep Pada Pemrograman PLC?", "id": "Menahan Status Bit." },
  { "en": "Shortcut Apa Untuk Grid On Off AutoCAD?", "id": "Tekan Tombol F7." },
  { "en": "Apa Fungsi Perintah Hatch Pada AutoCAD (Computer Aided Design)?", "id": "Mengarsir Area Objek Tertutup." },
  { "en": "Bagaimana Cara Membuat Garis Poligon Di AutoCAD?", "id": "Ketik Polygon Lalu Enter." },
  { "en": "Apa Fungsi PinMode Pada Program Arduino IDE?", "id": "Konfigurasi Pin Sebagai Input Output." },
  { "en": "Shortcut Apa Untuk Mengaktifkan Ortho Di AutoCAD?", "id": "Tekan Tombol F8." },
  { "en": "Apa Kegunaan Ammeter Pada Simulasi Proteus?", "id": "Mengukur Kuat Arus Listrik." },
  { "en": "Bagaimana Cara Memasang Ammeter Dalam Rangkaian?", "id": "Dipasang Secara Seri." },
  { "en": "Apa Fungsi Instruksi SET Pada CX Programmer?", "id": "Mengaktifkan Bit Secara Permanen." },
  { "en": "Apa Fungsi Instruksi RSET (Reset) Pada PLC?", "id": "Mematikan Bit Yang Di Set." },
  { "en": "Komponen Apa Yang Berfungsi Sebagai Saklar Cahaya?", "id": "Komponen LDR (Light Dependent Resistor)." },
  { "en": "Apa Fungsi Perintah Chamfer Pada AutoCAD?", "id": "Memotong Sudut Menjadi Miring." },
  { "en": "Apa Itu SPI (Serial Peripheral Interface) Pada Arduino?", "id": "Protokol Komunikasi Data Sinkron." },
  { "en": "Bagaimana Cara Menambah Baris Baru Di CX Programmer?", "id": "Klik Insert Row." },
  { "en": "Apa Fungsi Tombol F3 Pada Aplikasi AutoCAD?", "id": "Mengaktifkan Object Snap." },
  { "en": "Apa Fungsi Millis Di Pemrograman Arduino?", "id": "Menghitung Waktu Sejak Dinyalakan." },
  { "en": "Komponen IC (Integrated Circuit) 555 Berfungsi Sebagai?", "id": "Pewaktu Atau Timer." },
  { "en": "Apa Fungsi Kontak DIFU (Differential Up) Pada PLC?", "id": "Aktif Saat Transisi Naik." },
  { "en": "Apa Fungsi Kontak DIFD (Differential Down) Pada PLC?", "id": "Aktif Saat Transisi Turun." },
  { "en": "Bagaimana Cara Menggabungkan Garis Di AutoCAD?", "id": "Gunakan Perintah Join." },
  { "en": "Apa Fungsi Map Pada Coding Arduino?", "id": "Mengkonversi Rentang Nilai." },
  { "en": "Apa Itu Logika AND Pada Gerbang Logika?", "id": "Output Aktif Jika Semua Input Aktif." },
  { "en": "Apa Itu Logika OR Pada Gerbang Logika?", "id": "Output Aktif Jika Salah Satu Input Aktif." },
  { "en": "Shortcut Apa Untuk Close Drawing Di AutoCAD?", "id": "Tekan Ctrl + F4." },
  { "en": "Apa Fungsi Library Wire H Pada Arduino?", "id": "Komunikasi I2C (Inter Integrated Circuit)." },
  { "en": "Bagaimana Cara Mengukur Tahanan Di Proteus?", "id": "Gunakan Ohmmeter." },
  { "en": "Apa Fungsi Instruksi CMP (Compare) Pada PLC?", "id": "Membandingkan Dua Nilai Data." },
  { "en": "Apa Fungsi Tombol F7 Pada GX Works 2?", "id": "Membuat Kontak Coil Output." },
  { "en": "Apa Fungsi Perintah Array Pada AutoCAD?", "id": "Menggandakan Objek Pola Teratur." },
  { "en": "Apa Itu TX Dan RX Pada Arduino?", "id": "Transmit Dan Receive Data." },
  { "en": "Komponen Apa Yang Mengubah Cahaya Menjadi Listrik?", "id": "Panel Surya Atau Photovoltaic." },
  { "en": "Apa Fungsi Tombol F5 Pada CX Programmer?", "id": "Membuat Garis Horizontal." },
  { "en": "Apa Fungsi Perintah Rotate Pada AutoCAD?", "id": "Memutar Objek Sesuai Sudut." },
  { "en": "Apa Fungsi Const Int Pada Deklarasi Variabel?", "id": "Nilai Integer Tetap." },
  { "en": "Apa Itu DC Motor (Direct Current) Di Proteus?", "id": "Motor Arus Searah." },
  { "en": "Apa Fungsi Mnemonic AND LD Pada PLC?", "id": "Seri Dengan Blok Paralel." },
  { "en": "Bagaimana Cara Pan Atau Geser Layar AutoCAD?", "id": "Tahan Dan Geser Scroll Mouse." },
  { "en": "Apa Fungsi Include Pada Header Program Arduino?", "id": "Memasukkan Pustaka Luar." },
  { "en": "Apa Fungsi Relay DPDT (Double Pole Double Throw)?", "id": "Dua Induk Dua Cabang." },
  { "en": "Shortcut Apa Untuk Snap Mode Di AutoCAD?", "id": "Tekan Tombol F9." },
  { "en": "Apa Fungsi Analog Write Pada Arduino?", "id": "Output Sinyal PWM." },
  { "en": "Komponen Apa Yang Menghasilkan Suara Di Proteus?", "id": "Komponen Buzzer Atau Speaker." },
  { "en": "Apa Fungsi Instruksi MOV (Move) Pada PLC?", "id": "Memindahkan Data Ke Register." },
  { "en": "Bagaimana Cara Membuat Titik Di AutoCAD?", "id": "Ketik Point Lalu Enter." },
  { "en": "Apa Itu EEPROM (Electrically Erasable Programmable Read Only Memory)?", "id": "Memori Non Volatile Arduino." },
  { "en": "Apa Fungsi Logic Probe Pada Simulasi Proteus?", "id": "Mengecek Status Logika High Low." },
  { "en": "Apa Shortcut Insert Contact NO Di GX Works?", "id": "Tekan Tombol F5." },
  { "en": "Apa Fungsi Perintah Break Pada AutoCAD?", "id": "Memutus Garis Menjadi Dua." },
  { "en": "Apa Fungsi Operator Modulo (Persen) Di Arduino?", "id": "Sisa Hasil Bagi." },
  { "en": "Apa Itu SCR (Silicon Controlled Rectifier) Di Elektronika?", "id": "Dioda Dengan Kaki Gate." },
  { "en": "Bagaimana Cara Kompilasi Ladder Di GX Works?", "id": "Tekan Tombol F4." },
  { "en": "Apa Fungsi Perintah Stretch Pada AutoCAD?", "id": "Menarik Sisi Objek." },
  { "en": "Apa Tipe Data Float Pada Arduino?", "id": "Bilangan Desimal Atau Pecahan." },
  { "en": "Komponen Apa Pemicu Triac Di Proteus?", "id": "Diac Atau Optotriac." },
  { "en": "Apa Fungsi Instruksi JMP (Jump) Pada PLC?", "id": "Lompat Ke Label Tertentu." },
  { "en": "Shortcut Apa Untuk Polar Tracking AutoCAD?", "id": "Tekan Tombol F10." },
  { "en": "Apa Fungsi Tanda Kurung Kurawal Di Arduino?", "id": "Blok Kode Program." },
  { "en": "Apa Itu Latch Pada Rangkaian Kontrol?", "id": "Rangkaian Pengunci Sinyal." },
  { "en": "Apa Fungsi Instruksi CNT (Counter) Pada Omron?", "id": "Penghitung Mundur." },
  { "en": "Bagaimana Cara Membuat Elips Di AutoCAD?", "id": "Ketik Ellipse Lalu Enter." },
  { "en": "Apa Fungsi While Loop Pada Arduino?", "id": "Perulangan Selama Syarat Benar." },
  { "en": "Apa Itu Sensor Ultrasonic HCSR04 Di Proteus?", "id": "Sensor Pengukur Jarak." },
  { "en": "Apa Shortcut Insert Contact NC Di GX Works?", "id": "Tekan Tombol F6." },
  { "en": "Apa Fungsi Perintah Spline Pada AutoCAD?", "id": "Membuat Garis Lengkung Bebas." },
  { "en": "Apa Fungsi Return Pada Fungsi Arduino?", "id": "Mengembalikan Nilai Fungsi." },
  { "en": "Komponen Apa Pengubah AC Ke DC?", "id": "Bridge Rectifier." },
  { "en": "Apa Fungsi Work Area Pada CX Programmer?", "id": "Area Memori Internal PLC." },
  { "en": "Shortcut Apa Untuk Properties Di AutoCAD?", "id": "Tekan Ctrl + 1." },
  { "en": "Apa Fungsi Digital Read Pada Arduino?", "id": "Membaca Input Digital." },
  { "en": "Apa Itu Potensiometer (Variable Resistor) Di Proteus?", "id": "Resistor Variabel Putar." },
  { "en": "Apa Fungsi Instruksi IL (Interlock) Pada PLC?", "id": "Memblokir Eksekusi Program." },
  { "en": "Bagaimana Cara Mengubah Warna Garis AutoCAD?", "id": "Ubah Melalui Layer Properties." },
  { "en": "Apa Fungsi Break Dalam Loop Arduino?", "id": "Menghentikan Perulangan Paksa." },
  { "en": "Apa Itu Transistor PNP (Positive Negative Positive)?", "id": "Saklar Aktif Low." },
  { "en": "Shortcut Apa Untuk Find Replace CX Programmer?", "id": "Tekan Ctrl + F." },
  { "en": "Apa Fungsi Perintah Divide Pada AutoCAD?", "id": "Membagi Objek Sama Rata." },
  { "en": "Apa Fungsi Tanda Semicolon (Titik Koma) Arduino?", "id": "Akhir Baris Perintah." },
  { "en": "Apa Itu Relay Solid State (SSR)?", "id": "Relay Tanpa Kontak Mekanik." },
  { "en": "Apa Fungsi Data Register (D) Pada Mitsubishi?", "id": "Menyimpan Data Nilai 16 Bit." },
  { "en": "Bagaimana Cara Membuat Layer Baru AutoCAD?", "id": "Buka Layer Properties Manager." },
  { "en": "Apa Fungsi Switch Case Pada Arduino?", "id": "Percabangan Banyak Kondisi." },
  { "en": "Apa Itu Seven Segment Common Anode?", "id": "Kutub Positif Jadi Satu." },
  { "en": "Shortcut Apa Untuk Force On CX Programmer?", "id": "Tekan Ctrl + J." },
  { "en": "Apa Fungsi Perintah Donut Pada AutoCAD?", "id": "Membuat Lingkaran Berisi." },
  { "en": "Apa Fungsi High Pada Digital Write?", "id": "Memberi Tegangan 5 Volt." },
  { "en": "Apa Itu Optoisolator Di Rangkaian Proteus?", "id": "Pemisah Tegangan Tinggi Rendah." },
  { "en": "Apa Fungsi Instruksi TIMH (High Speed Timer)?", "id": "Timer Kecepatan Tinggi." },
  { "en": "Bagaimana Cara Regenerate Model Di AutoCAD?", "id": "Ketik Regen Lalu Enter." },
  { "en": "Apa Fungsi Tanda Double Slash Di Arduino?", "id": "Komentar Satu Baris." },
  { "en": "Apa Itu Motor Stepper Di Simulasi Proteus?", "id": "Motor Bergerak Per Langkah." },
  { "en": "Shortcut Apa Untuk Force Off CX Programmer?", "id": "Tekan Ctrl + K." },
  { "en": "Apa Fungsi Perintah Area Pada AutoCAD?", "id": "Menghitung Luas Area." },
  { "en": "Apa Fungsi Low Pada Digital Write?", "id": "Memberi Tegangan 0 Volt." },
  { "en": "Apa Itu Induktor Pada Rangkaian Elektronika?", "id": "Penyimpan Energi Medan Magnet." },
  { "en": "Apa Fungsi Instruksi SFT (Shift Register)?", "id": "Menggeser Bit Data." },
  { "en": "Bagaimana Cara Membuat Blok Di AutoCAD?", "id": "Ketik Block Lalu Enter." },
  { "en": "Apa Fungsi Random Pada Arduino IDE?", "id": "Menghasilkan Angka Acak." },
  { "en": "Apa Itu ADC 10 Bit Arduino?", "id": "Resolusi Nilai 0 Sampai 1023." },
  { "en": "Shortcut Apa Untuk Cancel Force CX Programmer?", "id": "Tekan Ctrl + L." },
  { "en": "Apa Fungsi Tools Dimension Linear AutoCAD?", "id": "Dimensi Garis Lurus." },
  { "en": "Apa Fungsi Tanda Seru Pada Logika Arduino?", "id": "Logika NOT Atau Negasi." },
  { "en": "Apa Fungsi Perintah Align Pada AutoCAD (Computer Aided Design)?", "id": "Menyelaraskan Posisi Dua Objek." },
  { "en": "Apa Fungsi Perintah Lengthen Pada AutoCAD?", "id": "Mengubah Panjang Garis Terbuka." },
  { "en": "Apa Itu I2C (Inter Integrated Circuit) Pada Arduino?", "id": "Protokol Komunikasi Dua Kabel." },
  { "en": "Shortcut Apa Untuk Save As Di AutoCAD?", "id": "Tekan Ctrl + Shift + S." },
  { "en": "Apa Fungsi Komponen Logic State Di Proteus?", "id": "Memberi Input Logika Manual." },
  { "en": "Apa Fungsi Instruksi ADD Pada Pemrograman PLC?", "id": "Operasi Penjumlahan Data." },
  { "en": "Apa Fungsi Instruksi SUB Pada Pemrograman PLC?", "id": "Operasi Pengurangan Data." },
  { "en": "Komponen Apa Yang Digunakan Sebagai Detak Jantung Mikrokontroler?", "id": "Komponen Crystal Oscillator." },
  { "en": "Apa Fungsi Perintah Union Pada AutoCAD 3D?", "id": "Menggabungkan Objek Solid." },
  { "en": "Apa Fungsi Abs Pada Matematika Arduino?", "id": "Mengambil Nilai Mutlak." },
  { "en": "Bagaimana Cara Mengembalikan Perintah Undo Di CX Programmer?", "id": "Tekan Ctrl + Y." },
  { "en": "Apa Fungsi Tombol F8 Pada GX Works 2?", "id": "Membuat Coil Instruksi Output." },
  { "en": "Apa Fungsi Perintah Subtract Pada AutoCAD?", "id": "Memotong Objek Solid Beririsan." },
  { "en": "Apa Fungsi Sizeof Pada Coding Arduino?", "id": "Menghitung Ukuran Byte Data." },
  { "en": "Komponen IC (Integrated Circuit) 7408 Berisi Gerbang Apa?", "id": "Gerbang Logika AND." },
  { "en": "Apa Fungsi Instruksi MUL Pada PLC?", "id": "Operasi Perkalian Data." },
  { "en": "Apa Fungsi Instruksi DIV Pada PLC?", "id": "Operasi Pembagian Data." },
  { "en": "Bagaimana Cara Membuat Garis Konstruksi Di AutoCAD?", "id": "Gunakan Perintah Xline." },
  { "en": "Apa Fungsi AttachInterrupt Pada Arduino?", "id": "Menjalankan Fungsi Saat Sinyal Masuk." },
  { "en": "Apa Itu Logika NAND Pada Gerbang Logika?", "id": "Kebalikan Dari Logika AND." },
  { "en": "Apa Itu Logika NOR Pada Gerbang Logika?", "id": "Kebalikan Dari Logika OR." },
  { "en": "Shortcut Apa Untuk Design Center Di AutoCAD?", "id": "Tekan Ctrl + 2." },
  { "en": "Apa Fungsi Library Servo H Pada Arduino?", "id": "Mengontrol Motor Servo." },
  { "en": "Bagaimana Cara Menghapus Jalur Di Proteus?", "id": "Klik Kanan Dua Kali." },
  { "en": "Apa Fungsi Instruksi INC (Increment) Pada PLC?", "id": "Menambah Nilai Satu Angka." },
  { "en": "Apa Fungsi Instruksi DEC (Decrement) Pada PLC?", "id": "Mengurangi Nilai Satu Angka." },
  { "en": "Apa Fungsi Perintah Intersect Pada AutoCAD?", "id": "Mengambil Area Irisan Objek." },
  { "en": "Apa Itu SDA (Serial Data) Pada Komunikasi I2C?", "id": "Jalur Transfer Data." },
  { "en": "Apa Itu SCL (Serial Clock) Pada Komunikasi I2C?", "id": "Jalur Detak Sinkronisasi." },
  { "en": "Shortcut Apa Untuk Tool Palettes Di AutoCAD?", "id": "Tekan Ctrl + 3." },
  { "en": "Apa Fungsi Unsigned Int Pada Arduino?", "id": "Variabel Bilangan Bulat Positif." },
  { "en": "Apa Fungsi ARES (Advanced Routing) Di Proteus?", "id": "Merancang Layout PCB." },
  { "en": "Apa Fungsi Mnemonic OR LD Pada PLC?", "id": "Paralel Dengan Blok Seri." },
  { "en": "Bagaimana Cara Zoom In Di AutoCAD?", "id": "Scroll Mouse Ke Depan." },
  { "en": "Apa Fungsi Perintah Sqrt Pada Arduino?", "id": "Menghitung Akar Kuadrat." },
  { "en": "Apa Fungsi Relay Tipe SPST (Single Pole Single Throw)?", "id": "Satu Induk Satu Cabang." },
  { "en": "Shortcut Apa Untuk Clean Screen Di AutoCAD?", "id": "Tekan Ctrl + 0." },
  { "en": "Apa Fungsi Tone Pada Audio Arduino?", "id": "Menghasilkan Frekuensi Suara." },
  { "en": "Komponen Apa Yang Berupa Matriks LED Di Proteus?", "id": "Dot Matrix Display." },
  { "en": "Apa Fungsi Instruksi WAND (Word And) Pada PLC?", "id": "Logika AND Tingkat Word." },
  { "en": "Bagaimana Cara Mengukur Radius Di AutoCAD?", "id": "Gunakan Dimensi Radius." },
  { "en": "Apa Itu SRAM (Static Random Access Memory) Arduino?", "id": "Memori Data Sementara." },
  { "en": "Apa Fungsi Logic Analyzer Pada Simulasi Proteus?", "id": "Menganalisa Gelombang Digital." },
  { "en": "Apa Shortcut Insert Column Di GX Works?", "id": "Tekan Ctrl + Insert." },
  { "en": "Apa Fungsi Perintah Boundary Pada AutoCAD?", "id": "Membuat Area Tertutup Polyline." },
  { "en": "Apa Fungsi Operator Logika Ampersand Ganda Di Arduino?", "id": "Operator Logika AND." },
  { "en": "Apa Itu IC (Integrated Circuit) 7432?", "id": "Gerbang Logika OR." },
  { "en": "Bagaimana Cara Cek Error Di CX Programmer?", "id": "Pilih Compile PLC Program." },
  { "en": "Apa Fungsi Perintah Region Pada AutoCAD?", "id": "Mengubah Garis Jadi Bidang." },
  { "en": "Apa Tipe Data Double Pada Arduino?", "id": "Bilangan Desimal Presisi Ganda." },
  { "en": "Komponen Apa Pemicu Thyristor Di Proteus?", "id": "Gate Trigger Current." },
  { "en": "Apa Fungsi Instruksi WOR (Word Or) Pada PLC?", "id": "Logika OR Tingkat Word." },
  { "en": "Shortcut Apa Untuk Dynamic Input AutoCAD?", "id": "Tekan Tombol F12." },
  { "en": "Apa Fungsi Tanda Pagar Define Di Arduino?", "id": "Praprosesor Pengganti Teks." },
  { "en": "Apa Itu ADC (Analog To Digital Converter)?", "id": "Pengubah Sinyal Analog Digital." },
  { "en": "Apa Fungsi Instruksi SQR (Square Root) PLC?", "id": "Menghitung Akar Pangkat Dua." },
  { "en": "Bagaimana Cara Membuat Tabel Di AutoCAD?", "id": "Ketik Table Lalu Enter." },
  { "en": "Apa Fungsi Do While Loop Pada Arduino?", "id": "Jalankan Dulu Baru Cek." },
  { "en": "Apa Itu Sensor PIR (Passive Infrared) Di Proteus?", "id": "Sensor Pendeteksi Gerakan." },
  { "en": "Apa Shortcut Delete Row Di GX Works?", "id": "Tekan Shift + Delete." },
  { "en": "Apa Fungsi Perintah Extrude Pada AutoCAD 3D?", "id": "Memberi Ketebalan Objek 2D." },
  { "en": "Apa Fungsi NoTone Pada Audio Arduino?", "id": "Menghentikan Frekuensi Suara." },
  { "en": "Komponen Apa Yang Menurunkan Tegangan AC?", "id": "Trafo Step Down." },
  { "en": "Apa Fungsi Holding Register (H) Pada PLC?", "id": "Menyimpan Data Saat Padam." },
  { "en": "Shortcut Apa Untuk Hyperlink Di AutoCAD?", "id": "Tekan Ctrl + K." },
  { "en": "Apa Fungsi Serial Available Pada Arduino?", "id": "Cek Jumlah Data Masuk." },
  { "en": "Apa Itu Keypad Phone Di Library Proteus?", "id": "Input Angka Matriks." },
  { "en": "Apa Fungsi Instruksi BCD (Binary Coded Decimal)?", "id": "Konversi Biner Ke Desimal." },
  { "en": "Bagaimana Cara Mengatur Layer Default AutoCAD?", "id": "Pilih Layer 0." },
  { "en": "Apa Fungsi PulseIn Pada Sensor Ultrasonic?", "id": "Membaca Durasi Pantulan." },
  { "en": "Apa Itu Common Cathode Pada 7 Segment?", "id": "Kutub Negatif Jadi Satu." },
  { "en": "Shortcut Apa Untuk Quick Calc AutoCAD?", "id": "Tekan Ctrl + 8." },
  { "en": "Apa Fungsi Perintah Revolve Pada AutoCAD?", "id": "Memutar Profil Jadi 3D." },
  { "en": "Apa Fungsi IsDigit Pada Karakter Arduino?", "id": "Cek Apakah Karakter Angka." },
  { "en": "Apa Itu IC (Integrated Circuit) 7404?", "id": "Gerbang Logika NOT." },
  { "en": "Apa Fungsi Auxiliary Relay (AR) Pada Omron?", "id": "Area Memori Status PLC." },
  { "en": "Bagaimana Cara Rename Blok Di AutoCAD?", "id": "Gunakan Perintah Rename." },
  { "en": "Apa Fungsi Switch Default Pada Arduino?", "id": "Opsi Jika Tidak Cocok." },
  { "en": "Apa Itu DC Voltmeter Di Proteus?", "id": "Pengukur Tegangan Arus Searah." },
  { "en": "Shortcut Apa Untuk Command Line AutoCAD?", "id": "Tekan Ctrl + 9." },
  { "en": "Apa Fungsi Perintah Wipeout Pada AutoCAD?", "id": "Menutupi Objek Di Belakang." },
  { "en": "Apa Fungsi Operator Garis Vertikal Ganda Arduino?", "id": "Operator Logika OR." },
  { "en": "Apa Itu Motor Servo Di Proteus?", "id": "Motor Dengan Kendali Sudut." },
  { "en": "Apa Fungsi Instruksi PLS (Pulse) Pada PLC?", "id": "Aktif Satu Siklus Scan." },
  { "en": "Bagaimana Cara Membuat Garis Tak Hingga AutoCAD?", "id": "Ketik Ray Lalu Enter." },
  { "en": "Apa Fungsi BitRead Pada Bitwise Arduino?", "id": "Membaca Nilai Bit Tertentu." },
  { "en": "Apa Itu LCD (Liquid Crystal Display) I2C?", "id": "Layar Dengan Modul Serial." },
  { "en": "Shortcut Apa Untuk Group Objek AutoCAD?", "id": "Tekan Ctrl + Shift + A." },
  { "en": "Apa Fungsi Perintah Revcloud Pada AutoCAD?", "id": "Membuat Gambar Awan Revisi." },
  { "en": "Apa Fungsi BitWrite Pada Bitwise Arduino?", "id": "Menulis Nilai Bit Tertentu." },
  { "en": "Apa Fungsi Terminal Ground Di Proteus?", "id": "Penghubung Ke Titik Nol." },
  { "en": "Apa Fungsi Instruksi NEG (Negation) Pada PLC?", "id": "Membalik Nilai Biner." },
  { "en": "Bagaimana Cara Isolasi Layer Di AutoCAD?", "id": "Gunakan Perintah Layiso." },
  { "en": "Apa Fungsi Continue Dalam Loop Arduino?", "id": "Lompati Sisa Kode Loop." },
  { "en": "Apa Itu Transistor Darlington Di Proteus?", "id": "Dua Transistor Penguat Tinggi." },
  { "en": "Shortcut Apa Untuk Open Sheet Arduino?", "id": "Tekan Ctrl + Shift + O." },
  { "en": "Apa Fungsi Perintah Pedit Pada AutoCAD?", "id": "Mengedit Garis Polyline." },
  { "en": "Apa Fungsi IsAlpha Pada Karakter Arduino?", "id": "Cek Apakah Karakter Huruf." },
  { "en": "Komponen Apa Pengaman Lonjakan Tegangan?", "id": "Komponen Varistor." },
  { "en": "Apa Fungsi Perintah Units Pada AutoCAD (Computer Aided Design)?", "id": "Mengatur Satuan Ukuran Gambar." },
  { "en": "Apa Fungsi Perintah Matchprop Pada AutoCAD?", "id": "Menyalin Properti Objek Lain." },
  { "en": "Shortcut Apa Untuk Auto Format Coding Arduino?", "id": "Tekan Ctrl + T." },
  { "en": "Apa Fungsi Perintah Purge Pada AutoCAD?", "id": "Membersihkan File Dari Sampah." },
  { "en": "Apa Fungsi Perintah Overkill Pada AutoCAD?", "id": "Menghapus Garis Yang Bertumpuk." },
  { "en": "Apa Fungsi Virtual Terminal Pada Simulasi Proteus?", "id": "Melihat Data Komunikasi Serial." },
  { "en": "Apa Fungsi Instruksi MC (Master Control) Pada PLC?", "id": "Memulai Blok Master Control." },
  { "en": "Apa Fungsi Instruksi MCR (Master Control Reset)?", "id": "Mengakhiri Blok Master Control." },
  { "en": "Komponen LM7805 Di Proteus Berfungsi Sebagai?", "id": "Regulator Tegangan 5 Volt." },
  { "en": "Apa Fungsi Perintah Loft Pada AutoCAD 3D?", "id": "Menggabungkan Penampang Jadi Solid." },
  { "en": "Apa Fungsi String Pada Tipe Data Arduino?", "id": "Menyimpan Teks Dinamis." },
  { "en": "Bagaimana Cara Copy With Base Point Di AutoCAD?", "id": "Tekan Ctrl + Shift + C." },
  { "en": "Apa Fungsi Tombol F2 Pada Aplikasi AutoCAD?", "id": "Menampilkan Jendela Teks History." },
  { "en": "Apa Fungsi Min Pada Matematika Arduino?", "id": "Mencari Nilai Terkecil." },
  { "en": "Komponen IC 7447 Berfungsi Sebagai Apa?", "id": "Dekoder BCD Ke 7 Segment." },
  { "en": "Apa Fungsi Instruksi JME (Jump End) Pada PLC?", "id": "Penanda Akhir Lompatan Program." },
  { "en": "Apa Fungsi Instruksi SBS (Subroutine) Pada PLC?", "id": "Memanggil Program Subroutine." },
  { "en": "Bagaimana Cara Paste As Block Di AutoCAD?", "id": "Tekan Ctrl + Shift + V." },
  { "en": "Apa Fungsi Interrupts Pada Program Arduino?", "id": "Mengaktifkan Fungsi Interupsi." },
  { "en": "Apa Itu Logika XOR (Exclusive OR)?", "id": "Output Aktif Jika Input Beda." },
  { "en": "Apa Itu Logika XNOR (Exclusive NOR)?", "id": "Output Aktif Jika Input Sama." },
  { "en": "Shortcut Apa Untuk Serial Monitor Arduino?", "id": "Tekan Ctrl + Shift + M." },
  { "en": "Apa Fungsi Library LiquidCrystal H Pada Arduino?", "id": "Mengontrol Layar LCD Karakter." },
  { "en": "Bagaimana Cara Membuat Bus Data Di Proteus?", "id": "Gunakan Bus Mode." },
  { "en": "Apa Fungsi Instruksi BMOV (Block Move) Pada PLC?", "id": "Memindahkan Blok Data Memori." },
  { "en": "Apa Fungsi Instruksi FMOV (Fill Move) Pada PLC?", "id": "Mengisi Blok Dengan Nilai Sama." },
  { "en": "Apa Fungsi Perintah Sweep Pada AutoCAD 3D?", "id": "Extrude Mengikuti Jalur Garis." },
  { "en": "Apa Itu MOSI (Master Out Slave In) SPI?", "id": "Jalur Data Keluar Master." },
  { "en": "Apa Itu MISO (Master In Slave Out) SPI?", "id": "Jalur Data Masuk Master." },
  { "en": "Shortcut Apa Untuk Pindah Tab AutoCAD?", "id": "Tekan Ctrl + Tab." },
  { "en": "Apa Fungsi Max Pada Matematika Arduino?", "id": "Mencari Nilai Terbesar." },
  { "en": "Apa Fungsi ERC (Electrical Rule Check) Di Proteus?", "id": "Memeriksa Kesalahan Koneksi Elektrik." },
  { "en": "Apa Fungsi Alamat X Pada PLC Mitsubishi?", "id": "Alamat Input Fisik." },
  { "en": "Bagaimana Cara Membuat Spiral Di AutoCAD?", "id": "Gunakan Perintah Helix." },
  { "en": "Apa Fungsi Pow Pada Matematika Arduino?", "id": "Menghitung Pangkat Bilangan." },
  { "en": "Apa Fungsi Relay 5V Di Proteus?", "id": "Saklar Dengan Koil 5 Volt." },
  { "en": "Shortcut Apa Untuk Object Snap Tracking AutoCAD?", "id": "Tekan Tombol F11." },
  { "en": "Apa Fungsi NoInterrupts Pada Program Arduino?", "id": "Mematikan Fungsi Interupsi." },
  { "en": "Komponen Apa Yang Digunakan Untuk Traffic Light?", "id": "Traffic Light Model." },
  { "en": "Apa Fungsi Instruksi ZCP (Zone Compare) PLC?", "id": "Bandingkan Data Dalam Rentang." },
  { "en": "Bagaimana Cara Mengukur Sudut Di AutoCAD?", "id": "Gunakan Dimensi Angular." },
  { "en": "Apa Itu Bootloader Pada Chip Arduino?", "id": "Program Awal Mikrokontroler." },
  { "en": "Apa Fungsi Netlist Pada Software Proteus?", "id": "Daftar Koneksi Antar Komponen." },
  { "en": "Apa Shortcut Work Online Di CX Programmer?", "id": "Tekan Ctrl + W." },
  { "en": "Apa Fungsi Perintah Mline Pada AutoCAD?", "id": "Membuat Garis Ganda Paralel." },
  { "en": "Apa Fungsi Constrain Pada Arduino?", "id": "Membatasi Nilai Dalam Rentang." },
  { "en": "Apa Itu Buck Converter Di Elektronika Daya?", "id": "Penurun Tegangan DC." },
  { "en": "Bagaimana Cara Membuat Garis Penunjuk AutoCAD?", "id": "Gunakan Perintah Mleader." },
  { "en": "Apa Fungsi Perintah Slice Pada AutoCAD 3D?", "id": "Memotong Objek Solid." },
  { "en": "Apa Fungsi LowByte Pada Data Arduino?", "id": "Mengambil 8 Bit Terendah." },
  { "en": "Komponen Apa Pembangkit Gelombang Kotak Di Proteus?", "id": "Pulse Generator." },
  { "en": "Apa Fungsi Instruksi RET (Return) Pada PLC?", "id": "Kembali Dari Subroutine." },
  { "en": "Shortcut Apa Untuk Hide Objects AutoCAD?", "id": "Gunakan Perintah Isolate." },
  { "en": "Apa Fungsi Sin Pada Trigonometri Arduino?", "id": "Menghitung Nilai Sinus." },
  { "en": "Apa Itu HMI (Human Machine Interface)?", "id": "Layar Antarmuka Pengendali." },
  { "en": "Apa Fungsi Instruksi ALT (Alternate) Pada PLC?", "id": "Flip Flop Status Output." },
  { "en": "Bagaimana Cara Membuat Teks Satu Baris AutoCAD?", "id": "Ketik Text Lalu Enter." },
  { "en": "Apa Fungsi Cos Pada Trigonometri Arduino?", "id": "Menghitung Nilai Cosinus." },
  { "en": "Apa Itu IC LM7812 Di Proteus?", "id": "Regulator Tegangan 12 Volt." },
  { "en": "Apa Fungsi Alamat Y Pada PLC Mitsubishi?", "id": "Alamat Output Fisik." },
  { "en": "Apa Fungsi Perintah Torus Pada AutoCAD?", "id": "Membuat Bentuk Donat 3D." },
  { "en": "Apa Fungsi Tan Pada Trigonometri Arduino?", "id": "Menghitung Nilai Tangen." },
  { "en": "Komponen Apa Pengubah DC Ke AC?", "id": "Inverter DC Ke AC." },
  { "en": "Apa Fungsi Alamat CIO Pada PLC Omron?", "id": "Memori Input Output Utama." },
  { "en": "Shortcut Apa Untuk Plot Style AutoCAD?", "id": "Buka Page Setup Manager." },
  { "en": "Apa Fungsi ShiftOut Pada Arduino?", "id": "Mengirim Data Bit Serial." },
  { "en": "Apa Itu Bill Of Materials (BOM) Di Proteus?", "id": "Daftar Komponen Yang Digunakan." },
  { "en": "Apa Fungsi Instruksi NOP (No Operation) PLC?", "id": "Tidak Melakukan Operasi Apa Pun." },
  { "en": "Bagaimana Cara Mengatur Koordinat User AutoCAD?", "id": "Gunakan Perintah UCS." },
  { "en": "Apa Fungsi ShiftIn Pada Arduino?", "id": "Menerima Data Bit Serial." },
  { "en": "Apa Itu Boost Converter Di Proteus?", "id": "Penaik Tegangan DC." },
  { "en": "Shortcut Apa Untuk Insert Block AutoCAD?", "id": "Ketik Insert Lalu Enter." },
  { "en": "Apa Fungsi Perintah Cone Pada AutoCAD?", "id": "Membuat Kerucut Solid 3D." },
  { "en": "Apa Fungsi HighByte Pada Data Arduino?", "id": "Mengambil 8 Bit Tertinggi." },
  { "en": "Apa Itu IC 7490 Di Proteus?", "id": "Penghitung Dekade BCD." },
  { "en": "Apa Fungsi Alamat W Pada PLC Omron?", "id": "Relay Kerja Internal." },
  { "en": "Bagaimana Cara Edit Teks Di AutoCAD?", "id": "Klik Dua Kali Teks." },
  { "en": "Apa Fungsi Pin AREF Pada Arduino?", "id": "Referensi Tegangan Analog Eksternal." },
  { "en": "Apa Itu Graph Mode Di Proteus?", "id": "Menampilkan Grafik Hasil Simulasi." },
  { "en": "Shortcut Apa Untuk Matikan Grid AutoCAD?", "id": "Tekan Tombol F7." },
  { "en": "Apa Fungsi Perintah Cylinder Pada AutoCAD?", "id": "Membuat Silinder Solid 3D." },
  { "en": "Apa Fungsi Tanda Tilde Di Arduino?", "id": "Operator Bitwise NOT." },
  { "en": "Apa Itu Step Up Converter?", "id": "Penaik Tegangan Listrik." },
  { "en": "Apa Fungsi Watchdog Timer Pada PLC?", "id": "Mendeteksi Error Sistem Hang." },
  { "en": "Bagaimana Cara Membuat Bola Solid AutoCAD?", "id": "Ketik Sphere Lalu Enter." },
  { "en": "Apa Fungsi Pin Vin Pada Arduino?", "id": "Input Tegangan Sumber Eksternal." },
  { "en": "Apa Itu Subcircuit Di Proteus?", "id": "Rangkaian Dalam Satu Simbol." },
  { "en": "Apa Shortcut Comment Code Arduino?", "id": "Tekan Ctrl + Slash." },
  { "en": "Apa Fungsi Perintah Pyramid Pada AutoCAD?", "id": "Membuat Piramida Solid 3D." },
  { "en": "Apa Fungsi Tanda Persen Di Printf Arduino?", "id": "Format Specifier Karakter." },
  { "en": "Apa Fungsi Pin Reset Pada Arduino?", "id": "Mengulang Program Dari Awal." },
  { "en": "Apa Itu Modul RS485 Pada PLC?", "id": "Komunikasi Serial Jarak Jauh." },
  { "en": "Bagaimana Cara Membuat Layout Cetak AutoCAD?", "id": "Klik Tab Layout." },
  { "en": "Apa Fungsi Perintah Wedge Pada AutoCAD?", "id": "Membuat Bentuk Baji 3D." },
  { "en": "Apa Fungsi SPI Begin Pada Arduino?", "id": "Memulai Komunikasi SPI." },
  { "en": "Apa Fungsi Component Mode Di Proteus?", "id": "Memilih Dan Menempatkan Komponen." },
  { "en": "Apa Fungsi Baterai Pada PLC Compact?", "id": "Menjaga Memori Saat Mati." },
  { "en": "Bagaimana Cara Memasukkan Gambar Di AutoCAD?", "id": "Gunakan Perintah Imageattach." },
  { "en": "Apa Fungsi Perintah Box Pada AutoCAD?", "id": "Membuat Kotak Solid 3D." },
  { "en": "Apa Fungsi Perintah Pline Pada AutoCAD (Computer Aided Design)?", "id": "Membuat Garis Poligon Menyatu." },
  { "en": "Apa Fungsi Perintah Xplode Pada AutoCAD?", "id": "Memecah Blok Dengan Properti Tetap." },
  { "en": "Apa Itu WCS (World Coordinate System) Di AutoCAD?", "id": "Sistem Koordinat Global Tetap." },
  { "en": "Shortcut Apa Untuk Export Program Di CX Programmer?", "id": "Gunakan Menu File Export." },
  { "en": "Apa Fungsi BitSet Pada Operasi Bitwise Arduino?", "id": "Mengubah Nilai Bit Jadi Satu." },
  { "en": "Apa Fungsi BitClear Pada Operasi Bitwise Arduino?", "id": "Mengubah Nilai Bit Jadi Nol." },
  { "en": "Apa Fungsi Instruksi FLT (Floating Point) Pada PLC?", "id": "Konversi Integer Ke Bilangan Desimal." },
  { "en": "Apa Fungsi Instruksi INT (Integer) Pada PLC?", "id": "Konversi Desimal Ke Bilangan Bulat." },
  { "en": "Komponen Apa Yang Berfungsi Sebagai Saklar Magnetik?", "id": "Reed Switch." },
  { "en": "Apa Fungsi Perintah Dist Pada AutoCAD?", "id": "Mengukur Jarak Antara Dua Titik." },
  { "en": "Apa Fungsi Unsigned Long Pada Arduino?", "id": "Menyimpan Angka Positif Sangat Besar." },
  { "en": "Bagaimana Cara Membuat Sudut Tumpul Di AutoCAD?", "id": "Gunakan Koordinat Polar." },
  { "en": "Apa Fungsi Tombol F6 Pada Aplikasi AutoCAD?", "id": "Mengaktifkan Dynamic UCS." },
  { "en": "Apa Fungsi Setup Pada Struktur Program Arduino?", "id": "Inisialisasi Awal Program." },
  { "en": "Komponen IC 7400 Berisi Gerbang Logika Apa?", "id": "Gerbang Logika NAND." },
  { "en": "Apa Fungsi Instruksi SFTR (Shift Register Right)?", "id": "Geser Bit Data Ke Kanan." },
  { "en": "Apa Fungsi Instruksi SFTL (Shift Register Left)?", "id": "Geser Bit Data Ke Kiri." },
  { "en": "Bagaimana Cara Quick Select Di AutoCAD?", "id": "Ketik Qselect Lalu Enter." },
  { "en": "Apa Fungsi Interrupt Rising Pada Arduino?", "id": "Picu Saat Sinyal Naik." },
  { "en": "Apa Itu Kontak Transition Sensitive Pada PLC?", "id": "Kontak Deteksi Perubahan Sinyal." },
  { "en": "Apa Itu Kontak NOT Pada Ladder Diagram?", "id": "Kontak Inversi Logika." },
  { "en": "Shortcut Apa Untuk Verify Code Arduino?", "id": "Tekan Ctrl + R." },
  { "en": "Apa Fungsi Library SPI H Pada Arduino?", "id": "Komunikasi Protokol SPI." },
  { "en": "Bagaimana Cara Mengubah Grid Size Di Proteus?", "id": "Pilih Menu View Snap." },
  { "en": "Apa Fungsi Instruksi ASL (Arithmetic Shift Left)?", "id": "Geser Aritmatika Ke Kiri." },
  { "en": "Apa Fungsi Instruksi ASR (Arithmetic Shift Right)?", "id": "Geser Aritmatika Ke Kanan." },
  { "en": "Apa Fungsi Perintah Id Pada AutoCAD?", "id": "Mengetahui Koordinat Suatu Titik." },
  { "en": "Apa Itu Full Duplex Pada Komunikasi Serial?", "id": "Komunikasi Dua Arah Bersamaan." },
  { "en": "Apa Itu Half Duplex Pada Komunikasi Serial?", "id": "Komunikasi Dua Arah Bergantian." },
  { "en": "Shortcut Apa Untuk Layer Properties AutoCAD?", "id": "Ketik Layer Lalu Enter." },
  { "en": "Apa Fungsi Ceil Pada Matematika Arduino?", "id": "Pembulatan Angka Ke Atas." },
  { "en": "Apa Fungsi Ratsnest Pada ARES Proteus?", "id": "Menghubungkan Jalur Belum Terkoneksi." },
  { "en": "Apa Fungsi Memory W (Work Area) Pada Omron?", "id": "Relay Bantu Internal Sementara." },
  { "en": "Bagaimana Cara Membuat Viewport Di Layout AutoCAD?", "id": "Gunakan Perintah Mview." },
  { "en": "Apa Fungsi Floor Pada Matematika Arduino?", "id": "Pembulatan Angka Ke Bawah." },
  { "en": "Apa Fungsi Relay 12V Di Proteus?", "id": "Saklar Dengan Koil 12 Volt." },
  { "en": "Shortcut Apa Untuk Block Editor AutoCAD?", "id": "Ketik Bedit Lalu Enter." },
  { "en": "Apa Fungsi Serial End Pada Arduino?", "id": "Menghentikan Komunikasi Serial." },
  { "en": "Komponen Apa Yang Menstabilkan Tegangan Zener?", "id": "Dioda Zener." },
  { "en": "Apa Fungsi Instruksi XFER (Block Transfer) PLC?", "id": "Menyalin Banyak Data Sekaligus." },
  { "en": "Bagaimana Cara Mengukur Luas Area Tertutup AutoCAD?", "id": "Gunakan Perintah Area." },
  { "en": "Apa Itu Flash Memory Pada Arduino?", "id": "Tempat Penyimpanan Program Sketch." },
  { "en": "Apa Fungsi Debugging Pada Simulasi Proteus?", "id": "Mencari Kesalahan Program." },
  { "en": "Apa Shortcut Insert Row Di GX Works?", "id": "Tekan Shift + Insert." },
  { "en": "Apa Fungsi Perintah Multiline Style AutoCAD?", "id": "Mengatur Gaya Garis Ganda." },
  { "en": "Apa Fungsi Static Volatile Pada Arduino?", "id": "Variabel Statis Dalam Interupsi." },
  { "en": "Apa Itu Logic Toggle Di Proteus?", "id": "Input Logika Saklar Penahan." },
  { "en": "Bagaimana Cara Menampilkan Grid Di AutoCAD?", "id": "Tekan Tombol F7." },
  { "en": "Apa Fungsi Perintah Flatshot Pada AutoCAD 3D?", "id": "Membuat Proyeksi 2D Datar." },
  { "en": "Apa Tipe Data String Object Pada Arduino?", "id": "Objek Teks Dengan Fungsi." },
  { "en": "Komponen Apa Pemicu SCR Di Proteus?", "id": "Arus Pada Kaki Gate." },
  { "en": "Apa Fungsi Instruksi ILC (Interlock Clear) PLC?", "id": "Akhir Dari Blok Interlock." },
  { "en": "Shortcut Apa Untuk Full Screen AutoCAD?", "id": "Tekan Ctrl + 0." },
  { "en": "Apa Fungsi Tanpa Tanda Baca Di Arduino?", "id": "Menyebabkan Error Kompilasi." },
  { "en": "Apa Itu Resolusi PWM 8 Bit?", "id": "Nilai Antara 0 Sampai 255." },
  { "en": "Apa Fungsi Instruksi SWAP Pada PLC?", "id": "Tukar Posisi Byte Data." },
  { "en": "Bagaimana Cara Membuat Atribut Blok AutoCAD?", "id": "Gunakan Perintah Attdef." },
  { "en": "Apa Fungsi Loop Pada Struktur Program Arduino?", "id": "Menjalankan Kode Berulang Kali." },
  { "en": "Apa Itu Sensor LDR Di Proteus?", "id": "Resistor Peka Cahaya." },
  { "en": "Apa Shortcut Device Comment Di GX Works?", "id": "Tekan Ctrl + D." },
  { "en": "Apa Fungsi Perintah Section Pada AutoCAD 3D?", "id": "Membuat Bidang Potong Objek." },
  { "en": "Apa Fungsi External Interrupt Pada Arduino?", "id": "Interupsi Dari Pin Luar." },
  { "en": "Komponen Apa Yang Mengubah Listrik Jadi Gerak?", "id": "Motor Listrik." },
  { "en": "Apa Fungsi Link Relay (LR) Pada PLC?", "id": "Data Link Antar PLC." },
  { "en": "Shortcut Apa Untuk Text Window AutoCAD?", "id": "Tekan Tombol F2." },
  { "en": "Apa Fungsi Serial ParseInt Pada Arduino?", "id": "Membaca Angka Dari Serial." },
  { "en": "Apa Itu AC Voltmeter Di Proteus?", "id": "Pengukur Tegangan Arus Bolak Balik." },
  { "en": "Apa Fungsi Instruksi BIN (Binary) Pada PLC?", "id": "Konversi BCD Ke Biner." },
  { "en": "Bagaimana Cara Membuka Palet Properties AutoCAD?", "id": "Tekan Ctrl + 1." },
  { "en": "Apa Fungsi Serial ParseFloat Pada Arduino?", "id": "Membaca Desimal Dari Serial." },
  { "en": "Apa Itu Common Anode Pada 7 Segment?", "id": "Kutub Positif Gabung." },
  { "en": "Shortcut Apa Untuk Switch Layout AutoCAD?", "id": "Tekan Ctrl + Page Down." },
  { "en": "Apa Fungsi Perintah Interfere Pada AutoCAD 3D?", "id": "Cek Tabrakan Antar Objek." },
  { "en": "Apa Fungsi Word Pada Tipe Data Arduino?", "id": "Menyimpan Nilai 16 Bit." },
  { "en": "Apa Itu IC 7486 Di Proteus?", "id": "Gerbang Logika XOR." },
  { "en": "Apa Fungsi Timer Register (T) Pada PLC?", "id": "Menyimpan Nilai Waktu Timer." },
  { "en": "Bagaimana Cara Menghapus Layer Di AutoCAD?", "id": "Gunakan Layer Properties Manager." },
  { "en": "Apa Fungsi Pin IOREF Pada Arduino?", "id": "Referensi Tegangan Level I/O." },
  { "en": "Apa Itu Power Plane Di PCB Proteus?", "id": "Area Tembaga Jalur Daya." },
  { "en": "Shortcut Apa Untuk Exit AutoCAD?", "id": "Tekan Ctrl + Q." },
  { "en": "Apa Fungsi Perintah Sphere Pada AutoCAD?", "id": "Membuat Bola Solid 3D." },
  { "en": "Apa Fungsi Tanda Bintang Di Pointer Arduino?", "id": "Mengakses Nilai Alamat Memori." },
  { "en": "Apa Itu Regulator 3.3V Pada Arduino?", "id": "Penyedia Tegangan 3.3 Volt." },
  { "en": "Apa Fungsi Analog Reference Pada Arduino?", "id": "Mengatur Referensi Tegangan Analog." },
  { "en": "Bagaimana Cara Mengatur Units Di AutoCAD?", "id": "Ketik Units Lalu Enter." },
  { "en": "Apa Fungsi Pin SCL Pada Arduino?", "id": "Clock Komunikasi I2C." },
  { "en": "Apa Itu Bill Of Material Di Proteus?", "id": "Daftar Kebutuhan Komponen." },
  { "en": "Apa Shortcut Preferences Di Arduino IDE?", "id": "Tekan Ctrl + Comma." },
  { "en": "Apa Fungsi Perintah Viewports Pada AutoCAD?", "id": "Mengatur Jendela Tampilan Layout." },
  { "en": "Apa Fungsi Character Escape Backslash N?", "id": "Membuat Baris Baru." },
  { "en": "Apa Fungsi Pin SDA Pada Arduino?", "id": "Data Komunikasi I2C." },
  { "en": "Apa Itu Pull Down Resistor?", "id": "Menarik Sinyal Ke Ground." },
  { "en": "Bagaimana Cara Print Preview Di AutoCAD?", "id": "Ketik Preview Lalu Enter." },
  { "en": "Apa Fungsi Perintah Attedit Pada AutoCAD?", "id": "Mengedit Atribut Blok." },
  { "en": "Apa Fungsi SPI Transfer Pada Arduino?", "id": "Kirim Dan Terima SPI." },
  { "en": "Apa Fungsi Zone Mode Di ARES Proteus?", "id": "Membuat Area Blok Tembaga." },
  { "en": "Apa Fungsi Counter Register (C) Pada PLC?", "id": "Menyimpan Nilai Hitungan Counter." },
  { "en": "Bagaimana Cara Purge Unused Items AutoCAD?", "id": "Ketik Purge Lalu Enter." },
  { "en": "Apa Fungsi Perintah Shell Pada AutoCAD 3D?", "id": "Membuat Dinding Berongga Solid." },
  { "en": "Apa Fungsi Perintah Copybase Pada AutoCAD (Computer Aided Design)?", "id": "Menyalin Dengan Titik Basis Tertentu." },
  { "en": "Apa Fungsi Perintah Pasteblock Pada AutoCAD?", "id": "Menempel Objek Sebagai Blok." },
  { "en": "Shortcut Apa Untuk Buka Serial Plotter Arduino?", "id": "Tekan Ctrl + Shift + L." },
  { "en": "Apa Fungsi Perintah Dimaligned Pada AutoCAD?", "id": "Dimensi Miring Mengikuti Objek." },
  { "en": "Apa Fungsi Perintah Dimangular Pada AutoCAD?", "id": "Mengukur Sudut Dua Garis." },
  { "en": "Apa Fungsi Wattmeter Pada Simulasi Proteus?", "id": "Mengukur Daya Listrik Rangkaian." },
  { "en": "Apa Fungsi Instruksi CLC (Clear Carry) Pada PLC?", "id": "Mereset Flag Carry Menjadi Nol." },
  { "en": "Apa Fungsi Instruksi STC (Set Carry) Pada PLC?", "id": "Mengaktifkan Flag Carry Jadi Satu." },
  { "en": "Komponen LM317 Di Proteus Berfungsi Sebagai?", "id": "Regulator Tegangan Variabel Positif." },
  { "en": "Apa Fungsi Perintah 3D Move Pada AutoCAD?", "id": "Menggeser Objek Dalam Ruang 3D." },
  { "en": "Apa Fungsi Tipe Data Byte Pada Arduino?", "id": "Menyimpan Angka 8 Bit Positif." },
  { "en": "Bagaimana Cara Membuat Teks Paragraf Di AutoCAD?", "id": "Gunakan Perintah Mtext." },
  { "en": "Apa Fungsi Tombol F4 Pada Aplikasi AutoCAD?", "id": "Mengaktifkan Fitur 3D Object Snap." },
  { "en": "Apa Fungsi Perintah Break At Point AutoCAD?", "id": "Memutus Garis Di Satu Titik." },
  { "en": "Komponen IC 7402 Berisi Gerbang Logika Apa?", "id": "Gerbang Logika NOR." },
  { "en": "Apa Fungsi Instruksi CNTR (Reversible Counter) PLC?", "id": "Penghitung Maju Dan Mundur." },
  { "en": "Apa Fungsi Instruksi TTIM (Totalizing Timer) PLC?", "id": "Timer Akumulatif Menyimpan Waktu." },
  { "en": "Bagaimana Cara Mengatur Transparansi Objek Di AutoCAD?", "id": "Ubah Nilai Transparency Properties." },
  { "en": "Apa Fungsi Interrupt Falling Pada Arduino?", "id": "Picu Saat Sinyal Turun." },
  { "en": "Apa Itu Kontak Positive Transition (P) PLC?", "id": "Aktif Sesaat Saat Sinyal Naik." },
  { "en": "Apa Itu Kontak Negative Transition (N) PLC?", "id": "Aktif Sesaat Saat Sinyal Turun." },
  { "en": "Shortcut Apa Untuk Close Sketch Arduino?", "id": "Tekan Ctrl + W." },
  { "en": "Apa Fungsi Library EEPROM H Pada Arduino?", "id": "Akses Memori Non Volatile Internal." },
  { "en": "Bagaimana Cara Mengatur Ukuran Kertas Di Proteus?", "id": "Menu System Set Sheet Sizes." },
  { "en": "Apa Fungsi Instruksi ROL (Rotate Left) PLC?", "id": "Memutar Bit Data Ke Kiri." },
  { "en": "Apa Fungsi Instruksi ROR (Rotate Right) PLC?", "id": "Memutar Bit Data Ke Kanan." },
  { "en": "Apa Fungsi Perintah Dimradius Pada AutoCAD?", "id": "Mengukur Jari Jari Lingkaran." },
  { "en": "Apa Itu Parity Bit Pada Komunikasi Serial?", "id": "Bit Cek Kesalahan Data." },
  { "en": "Apa Itu Stop Bit Pada Komunikasi Serial?", "id": "Penanda Akhir Paket Data." },
  { "en": "Shortcut Apa Untuk Toggle Viewport AutoCAD?", "id": "Tekan Ctrl + R." },
  { "en": "Apa Fungsi Round Pada Matematika Arduino?", "id": "Pembulatan Angka Terdekat." },
  { "en": "Apa Fungsi DRC (Design Rule Check) Proteus?", "id": "Cek Kesalahan Desain PCB." },
  { "en": "Apa Fungsi Memory Link (Link Relay) PLC?", "id": "Berbagi Data Antar PLC." },
  { "en": "Bagaimana Cara Menyembunyikan Layer Di AutoCAD?", "id": "Matikan Ikon Lampu Layer." },
  { "en": "Apa Fungsi Square (Sq) Pada Arduino?", "id": "Menghitung Kuadrat Bilangan." },
  { "en": "Apa Fungsi Relay 24V Di Proteus?", "id": "Saklar Dengan Koil 24 Volt." },
  { "en": "Shortcut Apa Untuk Infer Constraints AutoCAD?", "id": "Tekan Ctrl + Shift + I." },
  { "en": "Apa Fungsi Stream Flush Pada Arduino?", "id": "Kosongkan Buffer Aliran Data." },
  { "en": "Komponen Apa Yang Menyearahkan Arus Bolak Balik?", "id": "Dioda Bridge." },
  { "en": "Apa Fungsi Instruksi NEG (Negate) Pada PLC?", "id": "Membalik Tanda Nilai Bilangan." },
  { "en": "Bagaimana Cara Mengunci Layer Di AutoCAD?", "id": "Klik Ikon Gembok Layer." },
  { "en": "Apa Itu SRAM Internal Pada Arduino Uno?", "id": "Memori Kerja 2 Kilobyte." },
  { "en": "Apa Fungsi Frequency Counter Pada Simulasi Proteus?", "id": "Mengukur Frekuensi Sinyal." },
  { "en": "Apa Shortcut Find Device Di GX Works?", "id": "Tekan Ctrl + F." },
  { "en": "Apa Fungsi Perintah Trace Pada AutoCAD?", "id": "Membuat Garis Tebal Solid." },
  { "en": "Apa Fungsi Operator Decrement (Min Min) Arduino?", "id": "Mengurangi Nilai Satu Satuan." },
  { "en": "Apa Itu Solar Cell Di Proteus?", "id": "Sumber Tegangan Dari Cahaya." },
  { "en": "Bagaimana Cara Membuat Layout Print PDF AutoCAD?", "id": "Pilih Plotter Dwg To Pdf." },
  { "en": "Apa Fungsi Perintah Thicken Pada AutoCAD 3D?", "id": "Ubah Surface Jadi Solid." },
  { "en": "Apa Tipe Data Short Pada Arduino?", "id": "Bilangan Bulat 16 Bit." },
  { "en": "Komponen Apa Pemicu Transistor BJT?", "id": "Arus Pada Kaki Basis." },
  { "en": "Apa Fungsi Instruksi XCHG (Exchange) PLC?", "id": "Menukar Isi Dua Register." },
  { "en": "Shortcut Apa Untuk Copy Clip AutoCAD?", "id": "Tekan Ctrl + C." },
  { "en": "Apa Fungsi Else If Pada Arduino?", "id": "Cek Kondisi Alternatif Lain." },
  { "en": "Apa Itu Resolusi ADC 10 Bit?", "id": "Seribu Dua Puluh Empat Tingkat." },
  { "en": "Apa Fungsi Instruksi COM (Complement) PLC?", "id": "Inversi Logika Setiap Bit." },
  { "en": "Bagaimana Cara Insert OLE Object AutoCAD?", "id": "Masuk Menu Insert OLE Object." },
  { "en": "Apa Fungsi Return Value Pada Fungsi Arduino?", "id": "Hasil Keluaran Sebuah Fungsi." },
  { "en": "Apa Itu Sensor Thermistor NTC Proteus?", "id": "Resistor Turun Saat Panas." },
  { "en": "Apa Shortcut Comment Block GX Works?", "id": "Klik Kanan Edit Comment." },
  { "en": "Apa Fungsi Perintah Solidedit Pada AutoCAD?", "id": "Mengedit Wajah Dan Sisi Solid." },
  { "en": "Apa Fungsi Interrupt Change Pada Arduino?", "id": "Picu Saat Nilai Berubah." },
  { "en": "Komponen Apa Yang Menyimpan Medan Magnet?", "id": "Induktor Atau Coil." },
  { "en": "Apa Fungsi System Register (SR) Pada PLC?", "id": "Status Sistem Dan Error PLC." },
  { "en": "Shortcut Apa Untuk Export Arduino Binary?", "id": "Tekan Ctrl + Alt + S." },
  { "en": "Apa Fungsi Serial ReadBytes Pada Arduino?", "id": "Baca Beberapa Byte Ke Buffer." },
  { "en": "Apa Itu DC Ammeter Di Proteus?", "id": "Pengukur Arus Arus Searah." },
  { "en": "Apa Fungsi Instruksi MOVB (Move Bit) PLC?", "id": "Memindahkan Status Satu Bit." },
  { "en": "Bagaimana Cara Reset Workspace AutoCAD?", "id": "Pilih Workspace Switching Reset." },
  { "en": "Apa Fungsi IsSpace Pada Karakter Arduino?", "id": "Cek Karakter Spasi Putih." },
  { "en": "Apa Itu Common Cathode RGB LED?", "id": "Ground Jadi Satu Kaki." },
  { "en": "Shortcut Apa Untuk Previous View AutoCAD?", "id": "Ketik Zoom Lalu P." },
  { "en": "Apa Fungsi Perintah Imprint Pada AutoCAD 3D?", "id": "Mencetak Garis Di Permukaan Solid." },
  { "en": "Apa Fungsi String ToInt Pada Arduino?", "id": "Konversi Teks Ke Integer." },
  { "en": "Apa Itu IC 7411 Di Proteus?", "id": "Gerbang Logika Triple 3 Input AND." },
  { "en": "Apa Fungsi Index Register (IR) Pada PLC?", "id": "Penunjuk Alamat Memori Indirect." },
  { "en": "Bagaimana Cara Mengukur Panjang Kurva AutoCAD?", "id": "Gunakan Perintah List." },
  { "en": "Apa Fungsi Pin 5V Pada Arduino?", "id": "Output Tegangan 5 Volt." },
  { "en": "Apa Itu Bus Mode Di Proteus?", "id": "Mode Jalur Data Paralel." },
  { "en": "Shortcut Apa Untuk Paste Original Coords AutoCAD?", "id": "Menu Edit Paste Original Coordinates." },
  { "en": "Apa Fungsi Perintah Polysolid Pada AutoCAD?", "id": "Membuat Dinding 3D Instan." },
  { "en": "Apa Fungsi Tanda Tanya Di Kondisi Arduino?", "id": "Operator Ternary Pengganti If." },
  { "en": "Apa Itu Regulator 5V Pada Arduino?", "id": "Penyedia Tegangan 5 Volt." },
  { "en": "Apa Fungsi Analog ReadResolution Pada Arduino?", "id": "Mengatur Resolusi Baca Analog." },
  { "en": "Bagaimana Cara Mengatur Skala Linetype AutoCAD?", "id": "Gunakan Perintah LTS." },
  { "en": "Apa Fungsi Pin GND Pada Arduino?", "id": "Ground Atau Negatif Rangkaian." },
  { "en": "Apa Itu Project Clip Di Proteus?", "id": "Navigasi Area Skematik Besar." },
  { "en": "Apa Shortcut Increase Font Arduino IDE?", "id": "Tekan Ctrl + Plus." },
  { "en": "Apa Fungsi Perintah 3D Rotate Pada AutoCAD?", "id": "Memutar Objek Sumbu 3D." },
  { "en": "Apa Fungsi Character Null Terminated?", "id": "Penanda Akhir String C." },
  { "en": "Apa Fungsi Pin REF Pada Komparator?", "id": "Tegangan Referensi Pembanding." },
  { "en": "Apa Itu Push Button Active Low?", "id": "Logika Nol Saat Ditekan." },
  { "en": "Bagaimana Cara Publish Drawing AutoCAD?", "id": "Gunakan Perintah Publish." },
  { "en": "Apa Fungsi Perintah Dimdiameter Pada AutoCAD?", "id": "Mengukur Diameter Lingkaran." },
  { "en": "Apa Fungsi Wire Begin Pada Arduino?", "id": "Memulai Bus I2C Master." },
  { "en": "Apa Fungsi 2D Graphics Mode Proteus?", "id": "Menggambar Bentuk Geometri Manual." },
  { "en": "Apa Fungsi Data Memory (DM) Pada PLC?", "id": "Menyimpan Data Word R/W." },
  { "en": "Bagaimana Cara Audit File AutoCAD?", "id": "Ketik Audit Lalu Enter." },
  { "en": "Apa Fungsi Perintah Presspull Pada AutoCAD?", "id": "Tarik Permukaan Jadi Solid." },
  { "en": "Apa Fungsi Perintah Oops Pada AutoCAD (Computer Aided Design)?", "id": "Mengembalikan Objek Yang Baru Dihapus." },
  { "en": "Apa Fungsi Perintah Imageclip Pada AutoCAD?", "id": "Memotong Tampilan Gambar Raster." },
  { "en": "Shortcut Apa Untuk Decrease Font Arduino IDE?", "id": "Tekan Ctrl + Minus." },
  { "en": "Apa Fungsi Perintah Dimlinear Pada AutoCAD?", "id": "Dimensi Tegak Lurus Sumbu." },
  { "en": "Apa Fungsi Perintah Dimordinate Pada AutoCAD?", "id": "Menampilkan Koordinat Titik Absolut." },
  { "en": "Apa Fungsi Logic Toggle Pada Simulasi Proteus?", "id": "Saklar Logika Dua Kondisi." },
  { "en": "Apa Fungsi Instruksi AND NOT Pada PLC?", "id": "Logika Seri Dengan Kontak NC." },
  { "en": "Apa Fungsi Instruksi OR NOT Pada PLC?", "id": "Logika Paralel Dengan Kontak NC." },
  { "en": "Komponen LM358 Di Proteus Berfungsi Sebagai?", "id": "Penguat Operasional Ganda." },
  { "en": "Apa Fungsi Perintah Union Pada 3D Solids?", "id": "Menggabungkan Dua Objek Solid." },
  { "en": "Apa Fungsi Tipe Data Char Array Arduino?", "id": "Menyimpan Kumpulan Karakter String." },
  { "en": "Bagaimana Cara Membuat Multiline Text AutoCAD?", "id": "Gunakan Perintah Mtext." },
  { "en": "Apa Fungsi Tombol F1 Pada Aplikasi CX Programmer?", "id": "Membuka Menu Bantuan." },
  { "en": "Apa Fungsi Perintah Join Pada Garis AutoCAD?", "id": "Menyatukan Garis Segaris Terpisah." },
  { "en": "Komponen IC 7432 Berisi Gerbang Logika Apa?", "id": "Gerbang Logika OR." },
  { "en": "Apa Fungsi Instruksi IL (Interlock) Pada PLC?", "id": "Mengunci Bagian Program Ladder." },
  { "en": "Apa Fungsi Instruksi ILC (Interlock Clear) PLC?", "id": "Membuka Kunci Interlock." },
  { "en": "Bagaimana Cara Mengatur Line Weight AutoCAD?", "id": "Buka Menu Properties Lineweight." },
  { "en": "Apa Fungsi Interrupt Low Pada Arduino?", "id": "Picu Saat Pin Logika Nol." },
  { "en": "Apa Itu Holding Register Pada Modbus PLC?", "id": "Area Data Bisa Baca Tulis." },
  { "en": "Apa Itu Input Register Pada Modbus PLC?", "id": "Area Data Hanya Baca." },
  { "en": "Shortcut Apa Untuk Upload Using Programmer Arduino?", "id": "Tekan Ctrl + Shift + U." },
  { "en": "Apa Fungsi Library Servo Attach Pada Arduino?", "id": "Menghubungkan Servo Ke Pin." },
  { "en": "Bagaimana Cara Rotate 90 Derajat Di Proteus?", "id": "Klik Kanan Pilih Rotate." },
  { "en": "Apa Fungsi Instruksi WXOR (Word Exclusive Or)?", "id": "Logika XOR Tingkat Word." },
  { "en": "Apa Fungsi Instruksi WNOT (Word Not) PLC?", "id": "Inversi Bit Tingkat Word." },
  { "en": "Apa Fungsi Perintah Dimcontinue Pada AutoCAD?", "id": "Melanjutkan Dimensi Sebelumnya." },
  { "en": "Apa Itu Baud Rate 9600 Pada Serial?", "id": "Sembilan Ribu Enam Ratus Bit Detik." },
  { "en": "Apa Itu Start Bit Pada Komunikasi Serial?", "id": "Penanda Awal Paket Data." },
  { "en": "Shortcut Apa Untuk Insert Function GX Works?", "id": "Klik Menu Edit Insert Function." },
  { "en": "Apa Fungsi Floor Pada Perhitungan Arduino?", "id": "Membulatkan Nilai Ke Bawah." },
  { "en": "Apa Fungsi BOM Report Di Proteus?", "id": "Laporan Daftar Material Komponen." },
  { "en": "Apa Fungsi Link Register (LR) Pada Omron?", "id": "Area Berbagi Data Antar PLC." },
  { "en": "Bagaimana Cara Freeze Layer Di AutoCAD?", "id": "Klik Ikon Matahari Layer." },
  { "en": "Apa Fungsi Operator Increment (Plus Plus) Arduino?", "id": "Menambah Nilai Satu Satuan." },
  { "en": "Apa Fungsi Relay 5 Kaki Di Proteus?", "id": "Relay Tipe SPDT." },
  { "en": "Shortcut Apa Untuk Constraint Settings AutoCAD?", "id": "Masuk Menu Parametric Settings." },
  { "en": "Apa Fungsi Stream Available Pada Arduino?", "id": "Cek Ketersediaan Data Stream." },
  { "en": "Komponen Apa Yang Menyimpan Muatan Magnetik?", "id": "Induktor Inti Besi." },
  { "en": "Apa Fungsi Instruksi ABS (Absolute) Pada PLC?", "id": "Mengubah Nilai Negatif Jadi Positif." },
  { "en": "Bagaimana Cara Thaw Layer Di AutoCAD?", "id": "Klik Ikon Salju Layer." },
  { "en": "Apa Itu EEPROM Write Pada Arduino?", "id": "Menulis Data Ke Memori Tetap." },
  { "en": "Apa Fungsi Counter Timer Di Simulasi Proteus?", "id": "Menghitung Detak Atau Waktu." },
  { "en": "Apa Shortcut Replace Device Di GX Works?", "id": "Tekan Ctrl + H." },
  { "en": "Apa Fungsi Perintah Ray Pada AutoCAD?", "id": "Garis Sinar Satu Arah." },
  { "en": "Apa Fungsi Operator Perkalian (Bintang) Arduino?", "id": "Mengalikan Dua Nilai Numerik." },
  { "en": "Apa Itu Photodiode Di Proteus?", "id": "Dioda Peka Cahaya Terbalik." },
  { "en": "Bagaimana Cara Convert To PDF AutoCAD?", "id": "Gunakan Perintah Export PDF." },
  { "en": "Apa Fungsi Perintah Extrude Face AutoCAD?", "id": "Menarik Permukaan Solid Tertentu." },
  { "en": "Apa Tipe Data Unsigned Char Arduino?", "id": "Nilai 0 Sampai 255." },
  { "en": "Komponen Apa Pemicu MOSFET Di Proteus?", "id": "Tegangan Pada Kaki Gate." },
  { "en": "Apa Fungsi Instruksi XNR (Exclusive Nor) PLC?", "id": "Logika XNOR Antar Bit." },
  { "en": "Shortcut Apa Untuk Copy With Basepoint AutoCAD?", "id": "Tekan Ctrl + Shift + C." },
  { "en": "Apa Fungsi Else Pada Struktur If?", "id": "Jalankan Jika Kondisi Salah." },
  { "en": "Apa Itu Resolusi DAC 8 Bit?", "id": "Dua Ratus Lima Puluh Enam Tingkat." },
  { "en": "Apa Fungsi Instruksi SETB (Set Bit) PLC?", "id": "Mengaktifkan Bit Tunggal." },
  { "en": "Bagaimana Cara Insert Hyperlink AutoCAD?", "id": "Tekan Ctrl + K." },
  { "en": "Apa Fungsi Break Pada Switch Case Arduino?", "id": "Keluar Dari Blok Case." },
  { "en": "Apa Itu Sensor Suhu LM35DZ Proteus?", "id": "Sensor Suhu Presisi Celcius." },
  { "en": "Apa Shortcut Go To Rung GX Works?", "id": "Masuk Menu Find Jump." },
  { "en": "Apa Fungsi Perintah Fillet Edge AutoCAD?", "id": "Melengkungkan Sisi Objek 3D." },
  { "en": "Apa Fungsi NoTone Pada Pin Arduino?", "id": "Matikan Sinyal Gelombang Suara." },
  { "en": "Komponen Apa Yang Membatasi Arus LED?", "id": "Resistor Seri." },
  { "en": "Apa Fungsi Error Flag (ER) Pada PLC?", "id": "Menandai Kesalahan Instruksi." },
  { "en": "Shortcut Apa Untuk New Sketch Arduino?", "id": "Tekan Ctrl + N." },
  { "en": "Apa Fungsi Serial Write Pada Arduino?", "id": "Kirim Data Biner Serial." },
  { "en": "Apa Itu AC Ammeter Di Proteus?", "id": "Pengukur Arus Bolak Balik." },
  { "en": "Apa Fungsi Instruksi RSTB (Reset Bit) PLC?", "id": "Mematikan Bit Tunggal." },
  { "en": "Bagaimana Cara Save Workspace AutoCAD?", "id": "Pilih Workspace Save Current As." },
  { "en": "Apa Fungsi IsGraph Pada Karakter Arduino?", "id": "Cek Karakter Tampil Grafis." },
  { "en": "Apa Itu Common Anode RGB LED?", "id": "Positif Jadi Satu Kaki." },
  { "en": "Shortcut Apa Untuk Zoom Realtime AutoCAD?", "id": "Tekan Enter Saat Perintah Zoom." },
  { "en": "Apa Fungsi Perintah Shell Solid AutoCAD?", "id": "Membuat Cangkang Objek Solid." },
  { "en": "Apa Fungsi String Length Pada Arduino?", "id": "Menghitung Panjang Karakter Teks." },
  { "en": "Apa Itu IC 7421 Di Proteus?", "id": "Gerbang Logika Dual 4 Input AND." },
  { "en": "Apa Fungsi Task Register (TK) Pada PLC?", "id": "Mengontrol Eksekusi Tugas Program." },
  { "en": "Bagaimana Cara Mengukur Luas Poligon AutoCAD?", "id": "Gunakan Perintah Area Object." },
  { "en": "Apa Fungsi Pin 3V3 Pada Arduino?", "id": "Output Tegangan 3.3 Volt." },
  { "en": "Apa Itu Terminal Mode Di Proteus?", "id": "Memilih Terminal Input Output." },
  { "en": "Shortcut Apa Untuk Paste As Block AutoCAD?", "id": "Tekan Ctrl + Shift + V." },
  { "en": "Apa Fungsi Perintah Helix Pada AutoCAD?", "id": "Membuat Garis Spiral 3D." },
  { "en": "Apa Fungsi Tanda Titik Dua Arduino?", "id": "Penanda Label Goto." },
  { "en": "Apa Itu Regulator 9V Pada Rangkaian?", "id": "Penyedia Tegangan 9 Volt." },
  { "en": "Apa Fungsi Analog WriteResolution Pada Arduino?", "id": "Mengatur Resolusi Tulis PWM." },
  { "en": "Bagaimana Cara Mengatur Dimensi Style AutoCAD?", "id": "Ketik Dimstyle Lalu Enter." },
  { "en": "Apa Fungsi Pin AREF Pada ADC?", "id": "Tegangan Referensi Konversi Analog." },
  { "en": "Apa Itu Sheet Properties Di Proteus?", "id": "Mengatur Properti Lembar Kerja." },
  { "en": "Apa Shortcut Auto Format Arduino?", "id": "Tekan Ctrl + T." },
  { "en": "Apa Fungsi Perintah Rotate3D Pada AutoCAD?", "id": "Memutar Objek Ruang 3D." },
  { "en": "Apa Fungsi Operator Logika NOT (!)?", "id": "Membalik Nilai Kebenaran." },
  { "en": "Apa Fungsi Pin VCC Pada IC?", "id": "Pin Supply Tegangan Positif." },
  { "en": "Apa Itu Push Button Active High?", "id": "Logika Satu Saat Ditekan." },
  { "en": "Bagaimana Cara eTransmit Drawing AutoCAD?", "id": "Gunakan Perintah Etransmit." },
  { "en": "Apa Fungsi Perintah Dimbaseline Pada AutoCAD?", "id": "Dimensi Berbasis Garis Awal." },
  { "en": "Apa Fungsi Wire EndTransmission Pada Arduino?", "id": "Akhiri Transmisi I2C." },
  { "en": "Apa Fungsi Selection Mode Proteus?", "id": "Mode Standar Memilih Objek." },
  { "en": "Apa Fungsi File Memory (FM) Pada PLC?", "id": "Memori File Eksternal PLC." },
  { "en": "Bagaimana Cara Recover File AutoCAD?", "id": "Ketik Recover Lalu Enter." },
  { "en": "Apa Fungsi Perintah Subtract Solid AutoCAD?", "id": "Kurangi Volume Solid Utama." },
  { "en": "Apa Fungsi Perintah Xref Pada AutoCAD (Computer Aided Design)?", "id": "Menautkan File Referensi Eksternal." },
  { "en": "Apa Fungsi Perintah Wblock Pada AutoCAD?", "id": "Menyimpan Blok Ke File Terpisah." },
  { "en": "Shortcut Apa Untuk Orbit 3D Di AutoCAD?", "id": "Tahan Shift + Scroll Mouse." },
  { "en": "Apa Fungsi Perintah Dimedit Pada AutoCAD?", "id": "Mengedit Teks Dan Posisi Dimensi." },
  { "en": "Apa Fungsi Perintah Qleader Pada AutoCAD?", "id": "Membuat Garis Penunjuk Cepat." },
  { "en": "Apa Fungsi Probe Mode Pada Simulasi Proteus?", "id": "Alat Ukur Tegangan Dan Arus." },
  { "en": "Apa Fungsi Instruksi MOVR (Move Register) Pada PLC?", "id": "Memindahkan Data Antar Register." },
  { "en": "Apa Fungsi Instruksi BSET (Block Set) Pada PLC?", "id": "Mengisi Blok Memori Dengan Nilai." },
  { "en": "Komponen LM324 Di Proteus Berfungsi Sebagai?", "id": "Quad Operational Amplifier." },
  { "en": "Apa Fungsi Perintah Planesurf Pada AutoCAD?", "id": "Membuat Permukaan Datar 3D." },
  { "en": "Apa Fungsi Micros Pada Waktu Arduino?", "id": "Menghitung Waktu Dalam Mikrodetik." },
  { "en": "Bagaimana Cara Membuat Multiline Style AutoCAD?", "id": "Ketik Mlstyle Lalu Enter." },
  { "en": "Apa Fungsi Tombol F11 Pada Aplikasi AutoCAD?", "id": "Mengaktifkan Object Snap Tracking." },
  { "en": "Apa Fungsi Perintah Rename Pada AutoCAD?", "id": "Mengganti Nama Objek Bernama." },
  { "en": "Komponen IC 7400 Quad NAND Gate Adalah?", "id": "Empat Gerbang NAND Dua Input." },
  { "en": "Apa Fungsi Instruksi OSR (One Shot Rising)?", "id": "Aktif Satu Scan Saat Naik." },
  { "en": "Apa Fungsi Instruksi OSF (One Shot Falling)?", "id": "Aktif Satu Scan Saat Turun." },
  { "en": "Bagaimana Cara Mengatur Units Precision AutoCAD?", "id": "Ubah Presisi Di Menu Units." },
  { "en": "Apa Fungsi Interrupt Change Pada Pin Arduino?", "id": "Picu Saat Status Pin Berubah." },
  { "en": "Apa Itu Coil Set Pada Ladder Diagram?", "id": "Mengaktifkan Output Dan Menahannya." },
  { "en": "Apa Itu Coil Reset Pada Ladder Diagram?", "id": "Mematikan Output Yang Ditahan." },
  { "en": "Shortcut Apa Untuk Serial Plotter Arduino IDE?", "id": "Tekan Ctrl + Shift + L." },
  { "en": "Apa Fungsi Library SoftwareSerial H Pada Arduino?", "id": "Membuat Port Serial Virtual." },
  { "en": "Bagaimana Cara Mengatur Grid Snap Di Proteus?", "id": "Tekan F4 F3 F2." },
  { "en": "Apa Fungsi Instruksi ADD L (Double Add)?", "id": "Penjumlahan Data 32 Bit." },
  { "en": "Apa Fungsi Instruksi SUB L (Double Sub)?", "id": "Pengurangan Data 32 Bit." },
  { "en": "Apa Fungsi Perintah Dimjogged Pada AutoCAD?", "id": "Dimensi Radius Dengan Garis Tekuk." },
  { "en": "Apa Itu Frame Error Pada Komunikasi Serial?", "id": "Kesalahan Format Paket Data." },
  { "en": "Apa Itu Overrun Error Pada Komunikasi Serial?", "id": "Data Masuk Terlalu Cepat." },
  { "en": "Shortcut Apa Untuk Insert New Rung GX Works?", "id": "Tekan Shift + Insert." },
  { "en": "Apa Fungsi Ceil Pada Perhitungan Arduino?", "id": "Membulatkan Pecahan Ke Atas." },
  { "en": "Apa Fungsi Text Mode Di Proteus?", "id": "Menambahkan Label Teks Manual." },
  { "en": "Apa Fungsi Global Variable Pada PLC?", "id": "Variabel Diakses Semua Program." },
  { "en": "Bagaimana Cara Layon Layer Di AutoCAD?", "id": "Menyalakan Semua Layer Mati." },
  { "en": "Apa Fungsi Operator Decrement Di Depan Variabel?", "id": "Kurangi Dulu Baru Gunakan." },
  { "en": "Apa Fungsi Relay 8 Kaki Di Proteus?", "id": "Relay Tipe DPDT." },
  { "en": "Shortcut Apa Untuk Constraint Bar AutoCAD?", "id": "Tekan Ctrl + Shift + I." },
  { "en": "Apa Fungsi Stream Read Pada Arduino?", "id": "Membaca Satu Karakter Stream." },
  { "en": "Komponen Apa Yang Menghambat Arus Balik?", "id": "Dioda Penyearah." },
  { "en": "Apa Fungsi Instruksi SQRT (Square Root) PLC?", "id": "Menghitung Akar Kuadrat Data." },
  { "en": "Bagaimana Cara Layoff Layer Di AutoCAD?", "id": "Mematikan Layer Objek Terpilih." },
  { "en": "Apa Itu EEPROM Read Pada Arduino?", "id": "Membaca Data Dari Memori." },
  { "en": "Apa Fungsi Generator Mode Di Simulasi Proteus?", "id": "Sumber Sinyal DC Sine Pulse." },
  { "en": "Apa Shortcut Convert Fbd Di GX Works?", "id": "Pilih Menu Convert." },
  { "en": "Apa Fungsi Perintah Xline Pada AutoCAD?", "id": "Membuat Garis Konstruksi Tanpa Batas." },
  { "en": "Apa Fungsi Operator Pembagian (Garis Miring) Arduino?", "id": "Membagi Dua Nilai Numerik." },
  { "en": "Apa Itu Varistor Di Proteus?", "id": "Resistor Bergantung Tegangan." },
  { "en": "Bagaimana Cara Export Layout AutoCAD?", "id": "Klik Kanan Tab Layout Export." },
  { "en": "Apa Fungsi Perintah Loft Solid AutoCAD?", "id": "Membuat Solid Dari Penampang Berbeda." },
  { "en": "Apa Tipe Data Unsigned Long Arduino?", "id": "Integer Positif 32 Bit." },
  { "en": "Komponen Apa Pemicu JFET Di Proteus?", "id": "Tegangan Pada Kaki Gate." },
  { "en": "Apa Fungsi Instruksi WAND (Word And) PLC?", "id": "Operasi Logika DAN 16 Bit." },
  { "en": "Shortcut Apa Untuk Match Properties AutoCAD?", "id": "Ketik Ma Lalu Enter." },
  { "en": "Apa Fungsi Switch Case Default Arduino?", "id": "Jalankan Jika Tidak Ada Match." },
  { "en": "Apa Itu Resolusi ADC 12 Bit?", "id": "Empat Ribu Sembilan Puluh Enam." },
  { "en": "Apa Fungsi Instruksi RST (Reset) PLC?", "id": "Mematikan Bit Atau Timer." },
  { "en": "Bagaimana Cara Detach Reference AutoCAD?", "id": "Klik Kanan Xref Detach." },
  { "en": "Apa Fungsi Continue Pada Loop Arduino?", "id": "Lanjut Ke Iterasi Berikutnya." },
  { "en": "Apa Itu Sensor Gas MQ2 Proteus?", "id": "Sensor Deteksi Asap Dan Gas." },
  { "en": "Apa Shortcut Verify Project GX Works?", "id": "Pilih Menu Project Verify." },
  { "en": "Apa Fungsi Perintah Chamfer Edge AutoCAD?", "id": "Memotong Miring Sudut 3D." },
  { "en": "Apa Fungsi Tone Dengan Durasi Arduino?", "id": "Bunyi Frekuensi Waktu Tertentu." },
  { "en": "Komponen Apa Yang Memblokir Arus DC?", "id": "Kapasitor." },
  { "en": "Apa Fungsi Less Than Flag (LT) PLC?", "id": "Menandai Hasil Kurang Dari." },
  { "en": "Shortcut Apa Untuk Open Example Arduino?", "id": "Menu File Examples." },
  { "en": "Apa Fungsi Serial Peek Pada Arduino?", "id": "Lihat Data Tanpa Menghapus." },
  { "en": "Apa Itu DC Current Source Proteus?", "id": "Sumber Arus Konstan DC." },
  { "en": "Apa Fungsi Instruksi OUT NOT PLC?", "id": "Output Logika Terbalik." },
  { "en": "Bagaimana Cara Restore View AutoCAD?", "id": "Gunakan Perintah View." },
  { "en": "Apa Fungsi IsPunct Pada Karakter Arduino?", "id": "Cek Karakter Tanda Baca." },
  { "en": "Apa Itu Common Cathode LED Matrix?", "id": "Baris Negatif Jadi Satu." },
  { "en": "Shortcut Apa Untuk Zoom Previous AutoCAD?", "id": "Ketik Z Lalu P." },
  { "en": "Apa Fungsi Perintah Solidedit Face AutoCAD?", "id": "Mengedit Permukaan Objek Solid." },
  { "en": "Apa Fungsi String ToCharArray Pada Arduino?", "id": "Salin String Ke Array Char." },
  { "en": "Apa Itu IC 4017 Di Proteus?", "id": "Penghitung Decade Counter Divider." },
  { "en": "Apa Fungsi Pulse Output (PLS2) PLC?", "id": "Keluaran Pulsa Ganda." },
  { "en": "Bagaimana Cara Mengukur Volume Solid AutoCAD?", "id": "Gunakan Perintah Massprop." },
  { "en": "Apa Fungsi Pin RESET Pada IC?", "id": "Mengulang Proses Mikrokontroler." },
  { "en": "Apa Itu Sinesource Di Proteus?", "id": "Sumber Tegangan Gelombang Sinus." },
  { "en": "Shortcut Apa Untuk Group Ungroup AutoCAD?", "id": "Tekan Ctrl + Shift + A." },
  { "en": "Apa Fungsi Perintah Sweep Solid AutoCAD?", "id": "Extrude Profil Sepanjang Jalur." },
  { "en": "Apa Fungsi Tanda Petik Satu Arduino?", "id": "Menandai Karakter Tunggal Char." },
  { "en": "Apa Itu Power Supply Simetris?", "id": "Positif Ground Dan Negatif." },
  { "en": "Apa Fungsi Analog ReadAverage Pada Arduino?", "id": "Rata Rata Bacaan Analog." },
  { "en": "Bagaimana Cara Mengatur Dimension Scale AutoCAD?", "id": "Ubah Dimscale Di Properties." },
  { "en": "Apa Fungsi Pin AVCC Pada AVR?", "id": "Supply Tegangan Analog Converter." },
  { "en": "Apa Itu Power Rail Di Proteus?", "id": "Jalur Distribusi Daya Utama." },
  { "en": "Apa Shortcut Find In Sketch Arduino?", "id": "Tekan Ctrl + F." },
  { "en": "Apa Fungsi Perintah Rotate3D Axis AutoCAD?", "id": "Putar Sumbu X Y Z." },
  { "en": "Apa Fungsi Operator Logika OR (Garis Dua)?", "id": "Salah Satu Benar Maka Benar." },
  { "en": "Apa Fungsi Pin XTAL Pada Mikrokontroler?", "id": "Koneksi Osilator Kristal Eksternal." },
  { "en": "Apa Itu Limit Switch Di Proteus?", "id": "Saklar Pembatas Gerakan Mekanis." },
  { "en": "Bagaimana Cara Archive Sheet Set AutoCAD?", "id": "Gunakan Perintah Archive." },
  { "en": "Apa Fungsi Perintah Dimcenter Pada AutoCAD?", "id": "Menandai Pusat Lingkaran." },
  { "en": "Apa Fungsi Wire RequestFrom Pada Arduino?", "id": "Minta Data Dari Slave." },
  { "en": "Apa Fungsi Dashboard Mode Proteus?", "id": "Panel Kontrol Visual Simulasi." },
  { "en": "Apa Fungsi Parameter Memory (PM) PLC?", "id": "Menyimpan Parameter Setup PLC." },
  { "en": "Bagaimana Cara Draw Order AutoCAD?", "id": "Ketik Draworder Lalu Enter." },
  { "en": "Apa Fungsi Perintah Intersect Solid AutoCAD?", "id": "Ambil Bagian Solid Berpotongan." },
  { "en": "Apa Fungsi Perintah Layoutwizard Pada AutoCAD (Computer Aided Design)?", "id": "Membuat Layout Baru Dengan Panduan." },
  { "en": "Apa Fungsi Perintah Pagesetup Pada AutoCAD?", "id": "Mengatur Halaman Pencetakan Layout." },
  { "en": "Shortcut Apa Untuk Menutup Jendela Sekarang AutoCAD?", "id": "Tekan Ctrl + F4." },
  { "en": "Apa Fungsi Perintah Dimbaseline Pada Dimensi AutoCAD?", "id": "Membuat Dimensi Bertingkat Dari Basis." },
  { "en": "Apa Fungsi Perintah Dimcontinue Pada Dimensi AutoCAD?", "id": "Melanjutkan Dimensi Dari Titik Terakhir." },
  { "en": "Apa Fungsi Compim Pada Simulasi Proteus?", "id": "Model Port Serial Fisik PC." },
  { "en": "Apa Fungsi Instruksi WSFT (Word Shift) Pada PLC?", "id": "Menggeser Data Satuan Word." },
  { "en": "Apa Fungsi Instruksi ASFT (Asynchronous Shift) PLC?", "id": "Geser Data Word Asinkron." },
  { "en": "Komponen LM393 Di Proteus Berfungsi Sebagai?", "id": "Komparator Tegangan Ganda." },
  { "en": "Apa Fungsi Perintah Xedges Pada AutoCAD 3D?", "id": "Ekstrak Garis Tepi Objek Solid." },
  { "en": "Apa Fungsi RandomSeed Pada Arduino?", "id": "Inisialisasi Generator Angka Acak." },
  { "en": "Bagaimana Cara Mengubah Skala Linetype Global AutoCAD?", "id": "Ubah Nilai Ltscale." },
  { "en": "Apa Fungsi Tombol F5 Pada Aplikasi AutoCAD?", "id": "Pindah Bidang Isoplane." },
  { "en": "Apa Fungsi Perintah Mslide Pada AutoCAD?", "id": "Membuat File Slide Gambar." },
  { "en": "Komponen IC 7408 Quad AND Gate Adalah?", "id": "Empat Gerbang AND Dua Input." },
  { "en": "Apa Fungsi Instruksi TLLS (Ten Millisecond Timer)?", "id": "Timer Resolusi Sepuluh Milidetik." },
  { "en": "Apa Fungsi Instruksi MTIM (Multi Output Timer)?", "id": "Timer Dengan Banyak Output." },
  { "en": "Bagaimana Cara Mengatur Point Style AutoCAD?", "id": "Ketik Ddptype Lalu Enter." },
  { "en": "Apa Fungsi DetachInterrupt Pada Arduino?", "id": "Mematikan Fungsi Interupsi." },
  { "en": "Apa Itu Internal Relay (IR) Pada PLC?", "id": "Relay Bantu Dalam Memori." },
  { "en": "Apa Itu Special Relay (SR) Pada PLC?", "id": "Relay Status Fungsi Khusus." },
  { "en": "Shortcut Apa Untuk Buka Library Manager Arduino?", "id": "Tekan Ctrl + Shift + I." },
  { "en": "Apa Fungsi Library Math H Pada Arduino?", "id": "Fungsi Matematika Lanjutan." },
  { "en": "Bagaimana Cara Mengaktifkan Auto Save Proteus?", "id": "Menu System Set Save Options." },
  { "en": "Apa Fungsi Instruksi NEG L (Double Negate)?", "id": "Negasi Nilai 32 Bit." },
  { "en": "Apa Fungsi Instruksi SIGN (Sign Extension) PLC?", "id": "Ekstensi Tanda Bilangan." },
  { "en": "Apa Fungsi Perintah Qdim Pada AutoCAD?", "id": "Membuat Dimensi Cepat Otomatis." },
  { "en": "Apa Itu Checksum Pada Komunikasi Data?", "id": "Nilai Verifikasi Integritas Data." },
  { "en": "Apa Itu ACK (Acknowledge) Pada Komunikasi?", "id": "Sinyal Konfirmasi Data Diterima." },
  { "en": "Shortcut Apa Untuk Quick Print GX Works?", "id": "Tekan Ctrl + P." },
  { "en": "Apa Fungsi IsUpperCase Pada Karakter Arduino?", "id": "Cek Apakah Huruf Besar." },
  { "en": "Apa Fungsi Script Mode Di Proteus?", "id": "Menjalankan Skrip Debugging." },
  { "en": "Apa Fungsi Local Variable Pada PLC?", "id": "Variabel Hanya Untuk Program Itu." },
  { "en": "Bagaimana Cara Membuat Attribute Definition AutoCAD?", "id": "Ketik Attdef Lalu Enter." },
  { "en": "Apa Fungsi Operator Modulus Di Depan Variabel?", "id": "Sisa Bagi Dua Bilangan." },
  { "en": "Apa Fungsi Relay Latching Di Proteus?", "id": "Relay Pengunci Posisi Kontak." },
  { "en": "Shortcut Apa Untuk Geometric Constraints AutoCAD?", "id": "Ketik Geomconstraint Lalu Enter." },
  { "en": "Apa Fungsi Serial Find Pada Arduino?", "id": "Mencari String Di Buffer Serial." },
  { "en": "Komponen Apa Yang Menghambat Frekuensi Tinggi?", "id": "Induktor Atau Choke." },
  { "en": "Apa Fungsi Instruksi PID Pada PLC?", "id": "Kontrol Proporsional Integral Derivatif." },
  { "en": "Bagaimana Cara Edit Block In Place AutoCAD?", "id": "Ketik Refedit Lalu Enter." },
  { "en": "Apa Itu EEPROM Put Pada Arduino?", "id": "Menulis Segala Tipe Data." },
  { "en": "Apa Fungsi Pattern Generator Di Simulasi Proteus?", "id": "Pembangkit Pola Digital Digital." },
  { "en": "Apa Shortcut Simulation Mode GX Works?", "id": "Menu Debug Start Simulation." },
  { "en": "Apa Fungsi Perintah Mledit Pada AutoCAD?", "id": "Mengedit Garis Ganda Multiline." },
  { "en": "Apa Fungsi Operator Pengurangan (Minus) Arduino?", "id": "Mengurangi Dua Nilai Numerik." },
  { "en": "Apa Itu Thermocouple Di Proteus?", "id": "Sensor Suhu Tipe Tegangan." },
  { "en": "Bagaimana Cara Export Ke Format STL AutoCAD?", "id": "Ketik Export Lalu Pilih STL." },
  { "en": "Apa Fungsi Perintah ConvToSolid AutoCAD?", "id": "Konversi Objek Ke Solid 3D." },
  { "en": "Apa Tipe Data Volatile Byte Arduino?", "id": "Byte Berubah Di Interupsi." },
  { "en": "Komponen Apa Pemicu IGBT Di Proteus?", "id": "Tegangan Pada Kaki Gate." },
  { "en": "Apa Fungsi Instruksi XOR NOT PLC?", "id": "Logika XNOR Antar Bit." },
  { "en": "Shortcut Apa Untuk Design Tool AutoCAD?", "id": "Tekan Ctrl + 2." },
  { "en": "Apa Fungsi Switch Case Break Arduino?", "id": "Hentikan Eksekusi Blok Case." },
  { "en": "Apa Itu Resolusi DAC 10 Bit?", "id": "Seribu Dua Puluh Empat." },
  { "en": "Apa Fungsi Instruksi SET Carry (STC) PLC?", "id": "Set Flag Carry Ke Satu." },
  { "en": "Bagaimana Cara Reload Reference AutoCAD?", "id": "Klik Kanan Xref Reload." },
  { "en": "Apa Fungsi Goto Pada Program Arduino?", "id": "Lompat Ke Label Tertentu." },
  { "en": "Apa Itu Sensor Humidity Di Proteus?", "id": "Sensor Kelembaban Udara." },
  { "en": "Apa Shortcut Online Edit GX Works?", "id": "Tekan Shift + F3." },
  { "en": "Apa Fungsi Perintah Fillet 3D AutoCAD?", "id": "Melengkungkan Sudut Solid 3D." },
  { "en": "Apa Fungsi Tone Tanpa Durasi Arduino?", "id": "Bunyi Terus Sampai Notone." },
  { "en": "Komponen Apa Yang Memblokir Arus AC?", "id": "Induktor." },
  { "en": "Apa Fungsi Greater Than Flag (GT) PLC?", "id": "Menandai Hasil Lebih Besar." },
  { "en": "Shortcut Apa Untuk Close Sketch Arduino?", "id": "Tekan Ctrl + W." },
  { "en": "Apa Fungsi Serial Flush Pada Arduino?", "id": "Tunggu Data Keluar Terkirim." },
  { "en": "Apa Itu DC Voltage Source Proteus?", "id": "Sumber Tegangan Tetap DC." },
  { "en": "Apa Fungsi Instruksi AND LD PLC?", "id": "Serikan Blok Logika." },
  { "en": "Bagaimana Cara Viewport Configuration AutoCAD?", "id": "Menu View Viewports." },
  { "en": "Apa Fungsi IsLowerCase Pada Karakter Arduino?", "id": "Cek Apakah Huruf Kecil." },
  { "en": "Apa Itu Dot Matrix 5x7 Proteus?", "id": "Matriks LED Lima Kali Tujuh." },
  { "en": "Shortcut Apa Untuk Zoom Window AutoCAD?", "id": "Ketik Z Lalu W." },
  { "en": "Apa Fungsi Perintah Separate Solid AutoCAD?", "id": "Memisahkan Solid Tidak Bersentuhan." },
  { "en": "Apa Fungsi String ToLowerCase Pada Arduino?", "id": "Ubah Teks Ke Huruf Kecil." },
  { "en": "Apa Itu IC 7414 Di Proteus?", "id": "Schmitt Trigger Inverter." },
  { "en": "Apa Fungsi Clock Pulse (P_OS) PLC?", "id": "Pulsa Detak Sistem." },
  { "en": "Bagaimana Cara Mengukur Inersia Solid AutoCAD?", "id": "Gunakan Perintah Massprop." },
  { "en": "Apa Fungsi Pin AREF Pada Arduino Uno?", "id": "Input Referensi Analog." },
  { "en": "Apa Itu Pulse Source Di Proteus?", "id": "Sumber Tegangan Pulsa Periodik." },
  { "en": "Shortcut Apa Untuk Hide Palettes AutoCAD?", "id": "Tekan Ctrl + Shift + H." },
  { "en": "Apa Fungsi Perintah Extrude Path AutoCAD?", "id": "Extrude Mengikuti Garis Jalur." },
  { "en": "Apa Fungsi Tanda Backslash T Arduino?", "id": "Karakter Tabulasi Horizontal." },
  { "en": "Apa Itu Power Supply Asimetris?", "id": "Hanya Positif Dan Ground." },
  { "en": "Apa Fungsi Analog WriteFrequency Pada Arduino?", "id": "Ubah Frekuensi Sinyal PWM." },
  { "en": "Bagaimana Cara Mengatur Text Style AutoCAD?", "id": "Ketik Style Lalu Enter." },
  { "en": "Apa Fungsi Pin MISO Pada ISP?", "id": "Data Masuk Ke Master." },
  { "en": "Apa Itu Ground Plane Di Proteus?", "id": "Area Tembaga Jalur Ground." },
  { "en": "Apa Shortcut Go To Line Arduino?", "id": "Tekan Ctrl + L." },
  { "en": "Apa Fungsi Perintah Mirror3D Pada AutoCAD?", "id": "Cermin Objek Ruang 3D." },
  { "en": "Apa Fungsi Operator Logika Equal (Dua Sama Dengan)?", "id": "Memeriksa Kesamaan Nilai." },
  { "en": "Apa Fungsi Pin RESET Pada ISP?", "id": "Reset Target Mikrokontroler." },
  { "en": "Apa Itu Dip Switch Di Proteus?", "id": "Saklar Geser Berjajar." },
  { "en": "Bagaimana Cara Publish To Web AutoCAD?", "id": "Gunakan Perintah PublishToWeb." },
  { "en": "Apa Fungsi Perintah Dimangular 3 Point?", "id": "Sudut Berdasarkan Tiga Titik." },
  { "en": "Apa Fungsi Wire OnReceive Pada Arduino?", "id": "Handler Saat Data Diterima." },
  { "en": "Apa Fungsi Source Code Tab Proteus?", "id": "Menulis Program Mikrokontroler." },
  { "en": "Apa Fungsi Temporary Relay (TR) PLC?", "id": "Menyimpan Status Percabangan Logika." },
  { "en": "Bagaimana Cara Image Adjust AutoCAD?", "id": "Ketik Imageadjust Lalu Enter." },
  { "en": "Apa Fungsi Perintah Union Region AutoCAD?", "id": "Gabungkan Area Bidang 2D." },
  { "en": "Apa Fungsi Perintah Flatten Pada AutoCAD (Computer Aided Design)?", "id": "Meratakan Objek 3D Menjadi 2D." },
  { "en": "Apa Fungsi Perintah Burst Pada AutoCAD Express Tools?", "id": "Meledakkan Blok Tanpa Merusak Atribut." },
  { "en": "Shortcut Apa Untuk Render Environment AutoCAD?", "id": "Ketik Renderenvironment Lalu Enter." },
  { "en": "Apa Fungsi Perintah Dimjogline Pada AutoCAD?", "id": "Menambahkan Garis Tekuk Pada Dimensi." },
  { "en": "Apa Fungsi Perintah Diminspect Pada AutoCAD?", "id": "Menambahkan Label Inspeksi Pada Dimensi." },
  { "en": "Apa Fungsi Auto Router Pada Software Proteus?", "id": "Membuat Jalur PCB Secara Otomatis." },
  { "en": "Apa Fungsi Instruksi DIST (Data Distribute) PLC?", "id": "Mendistribusikan Data Ke Alamat Memori." },
  { "en": "Apa Fungsi Instruksi COLL (Data Collect) PLC?", "id": "Mengumpulkan Data Dari Alamat Memori." },
  { "en": "Komponen LM741 Di Proteus Berfungsi Sebagai?", "id": "Single Operational Amplifier Standar." },
  { "en": "Apa Fungsi Perintah Sysvar Monitor Pada AutoCAD?", "id": "Memantau Perubahan Variabel Sistem." },
  { "en": "Apa Fungsi String Trim Pada Arduino?", "id": "Menghapus Spasi Awal Dan Akhir." },
  { "en": "Bagaimana Cara Mengatur Units Area AutoCAD?", "id": "Ubah Pengaturan Di Menu Units." },
  { "en": "Apa Fungsi Tombol Shift Saat Seleksi AutoCAD?", "id": "Membatalkan Seleksi Objek Tertentu." },
  { "en": "Apa Fungsi Perintah Chspace Pada Layout AutoCAD?", "id": "Memindah Objek Model Ke Layout." },
  { "en": "Komponen IC 7432 Quad OR Gate Adalah?", "id": "Empat Gerbang OR Dua Input." },
  { "en": "Apa Fungsi Instruksi TPO (Time Proportional Output)?", "id": "Output Proporsional Berbasis Waktu." },
  { "en": "Apa Fungsi Instruksi SCL (Scaling) Pada PLC?", "id": "Menskalakan Nilai Analog Input." },
  { "en": "Bagaimana Cara Mengatur Transparency Display AutoCAD?", "id": "Aktifkan Tombol Transparency Di Statusbar." },
  { "en": "Apa Fungsi Interrupt Mode Low Arduino?", "id": "Picu Terus Selama Pin Low." },
  { "en": "Apa Itu Special Auxiliary Relay (AR) PLC?", "id": "Relay Bantu Fungsi Khusus Omron." },
  { "en": "Apa Itu Temporary Relay (TR) Pada PLC?", "id": "Menyimpan Status Cabang Sementara." },
  { "en": "Shortcut Apa Untuk Sketch Include Library Arduino?", "id": "Menu Sketch Include Library." },
  { "en": "Apa Fungsi Library Wire EndTransmission Arduino?", "id": "Mengakhiri Transmisi Data I2C." },
  { "en": "Bagaimana Cara Mengukur Jarak Di Proteus?", "id": "Gunakan Alat Dimension Mode." },
  { "en": "Apa Fungsi Instruksi MAX L (Double Max)?", "id": "Nilai Terbesar Data 32 Bit." },
  { "en": "Apa Fungsi Instruksi MIN L (Double Min)?", "id": "Nilai Terkecil Data 32 Bit." },
  { "en": "Apa Fungsi Perintah Qleader Settings AutoCAD?", "id": "Mengatur Opsi Garis Penunjuk Cepat." },
  { "en": "Apa Itu Handshaking Pada Komunikasi Serial?", "id": "Proses Negosiasi Sebelum Transfer Data." },
  { "en": "Apa Itu Buffer Overflow Pada Arduino?", "id": "Data Masuk Melebihi Kapasitas Memori." },
  { "en": "Shortcut Apa Untuk PLC Read Mode GX Works?", "id": "Tekan Ctrl + F2." },
  { "en": "Apa Fungsi IsDigit Pada Karakter Arduino?", "id": "Memeriksa Apakah Karakter Angka." },
  { "en": "Apa Fungsi Spy Mode Di Proteus?", "id": "Memantau Data Komunikasi Digital." },
  { "en": "Apa Fungsi Retentive Timer (RTO) PLC?", "id": "Timer Yang Menyimpan Nilai Terakhir." },
  { "en": "Bagaimana Cara Membuat Block Attribute AutoCAD?", "id": "Gunakan Perintah Attdef." },
  { "en": "Apa Fungsi Operator Penjumlahan (Plus) Arduino?", "id": "Menjumlahkan Dua Nilai Numerik." },
  { "en": "Apa Fungsi Relay Reed Di Proteus?", "id": "Relay Kontak Tabung Kaca." },
  { "en": "Shortcut Apa Untuk Dimensional Constraints AutoCAD?", "id": "Ketik Dimconstraint Lalu Enter." },
  { "en": "Apa Fungsi Serial AvailableForWrite Arduino?", "id": "Cek Ruang Kosong Buffer Kirim." },
  { "en": "Komponen Apa Yang Menghubungkan Arus Basis?", "id": "Resistor Basis Transistor." },
  { "en": "Apa Fungsi Instruksi LIMIT Pada PLC?", "id": "Membatasi Nilai Atas Dan Bawah." },
  { "en": "Bagaimana Cara Mengedit Xref In Place AutoCAD?", "id": "Klik Kanan Edit Xref In Place." },
  { "en": "Apa Itu EEPROM Get Pada Arduino?", "id": "Membaca Segala Tipe Data Memori." },
  { "en": "Apa Fungsi I2C Debugger Di Simulasi Proteus?", "id": "Analisis Protokol Komunikasi I2C." },
  { "en": "Apa Shortcut PLC Write Mode GX Works?", "id": "Tekan F2 Atau Shift F2." },
  { "en": "Apa Fungsi Perintah Mline Edit AutoCAD?", "id": "Alat Penyuntingan Garis Ganda." },
  { "en": "Apa Fungsi Operator Modulo (Sisa Bagi) Arduino?", "id": "Mendapatkan Sisa Hasil Pembagian." },
  { "en": "Apa Itu Crystal Oscillator Di Proteus?", "id": "Sumber Detak Frekuensi Stabil." },
  { "en": "Bagaimana Cara Export Ke Format BMP AutoCAD?", "id": "Ketik Bmpout Lalu Enter." },
  { "en": "Apa Fungsi Perintah Solidedit Body AutoCAD?", "id": "Edit Keseluruhan Tubuh Solid." },
  { "en": "Apa Tipe Data Static Byte Arduino?", "id": "Nilai Byte Tetap Antar Pemanggilan." },
  { "en": "Komponen Apa Pemicu UJT Di Proteus?", "id": "Tegangan Pada Kaki Emitor." },
  { "en": "Apa Fungsi Instruksi NAND NOT PLC?", "id": "Logika NAND Dengan Input Terbalik." },
  { "en": "Shortcut Apa Untuk Layer Walk AutoCAD?", "id": "Ketik Laywalk Lalu Enter." },
  { "en": "Apa Fungsi Switch Case Default Break?", "id": "Keluar Setelah Opsi Default." },
  { "en": "Apa Itu Resolusi DAC 12 Bit?", "id": "Empat Ribu Sembilan Puluh Enam." },
  { "en": "Apa Fungsi Instruksi CLR Carry (CLC) PLC?", "id": "Reset Flag Carry Ke Nol." },
  { "en": "Bagaimana Cara Unload Reference AutoCAD?", "id": "Klik Kanan Xref Unload." },
  { "en": "Apa Fungsi Label Pada Goto Arduino?", "id": "Penanda Tujuan Lompatan Program." },
  { "en": "Apa Itu Sensor Pressure Di Proteus?", "id": "Sensor Tekanan Udara Gas." },
  { "en": "Apa Shortcut Cross Reference GX Works?", "id": "Pilih Menu View Cross Reference." },
  { "en": "Apa Fungsi Perintah Chamfer 3D AutoCAD?", "id": "Memotong Sudut Sisi Solid." },
  { "en": "Apa Fungsi PulseInLong Pada Arduino?", "id": "Baca Pulsa Panjang Interupsi Mati." },
  { "en": "Komponen Apa Yang Menyaring Noise Frekuensi?", "id": "Kapasitor Filter." },
  { "en": "Apa Fungsi Equals Flag (EQ) PLC?", "id": "Menandai Hasil Sama Dengan." },
  { "en": "Shortcut Apa Untuk Verify Sketch Arduino?", "id": "Tekan Ctrl + R." },
  { "en": "Apa Fungsi Serial Print Format Arduino?", "id": "Cetak Data Dengan Format Tertentu." },
  { "en": "Apa Itu Battery Cell Proteus?", "id": "Sumber Tegangan Baterai Tunggal." },
  { "en": "Apa Fungsi Instruksi OR LD PLC?", "id": "Paralelkan Blok Logika." },
  { "en": "Bagaimana Cara Create Camera AutoCAD?", "id": "Ketik Camera Lalu Enter." },
  { "en": "Apa Fungsi IsAlphaNumeric Pada Karakter Arduino?", "id": "Cek Karakter Huruf Atau Angka." },
  { "en": "Apa Itu Bar Graph Display Proteus?", "id": "Penampil Level LED Batang." },
  { "en": "Shortcut Apa Untuk Zoom All AutoCAD?", "id": "Ketik Z Lalu A." },
  { "en": "Apa Fungsi Perintah Union Surface AutoCAD?", "id": "Menggabungkan Permukaan Surface." },
  { "en": "Apa Fungsi String EqualsIgnoreCase Arduino?", "id": "Bandingkan Teks Tanpa Peduli Kapital." },
  { "en": "Apa Itu IC 7402 Di Proteus?", "id": "Gerbang Logika Quad NOR." },
  { "en": "Apa Fungsi Pulse I/O Refresh (IORF)?", "id": "Refresh Input Output Segera." },
  { "en": "Bagaimana Cara Mengukur Koordinat 3D AutoCAD?", "id": "Gunakan Perintah Id Point." },
  { "en": "Apa Fungsi Pin IO0 Sampai IO13?", "id": "Pin Input Output Digital." },
  { "en": "Apa Itu Clock Source Di Proteus?", "id": "Sumber Sinyal Detak Digital." },
  { "en": "Shortcut Apa Untuk Show Menu Bar AutoCAD?", "id": "Ketik Menubar Ubah Jadi 1." },
  { "en": "Apa Fungsi Perintah Extrude Taper AutoCAD?", "id": "Extrude Dengan Sudut Kemiringan." },
  { "en": "Apa Fungsi Tanda Backslash R Arduino?", "id": "Karakter Carriage Return." },
  { "en": "Apa Itu Floating Power Supply?", "id": "Sumber Tegangan Mengambang." },
  { "en": "Apa Fungsi Analog ReadResolution 12 Bit?", "id": "Resolusi Baca Empat Ribu Data." },
  { "en": "Bagaimana Cara Mengatur Point Size AutoCAD?", "id": "Ketik Ptype Lalu Enter." },
  { "en": "Apa Fungsi Pin MOSI Pada ISP?", "id": "Data Keluar Dari Master." },
  { "en": "Apa Itu Zone Mode Proteus?", "id": "Membuat Area PCB Berzona." },
  { "en": "Apa Shortcut Format Code Arduino?", "id": "Tekan Ctrl + T." },
  { "en": "Apa Fungsi Perintah Align 3D AutoCAD?", "id": "Menyelaraskan Objek Di Ruang 3D." },
  { "en": "Apa Fungsi Operator Logika Not Equal (!=)?", "id": "Memeriksa Ketidaksamaan Nilai." },
  { "en": "Apa Fungsi Pin SCK Pada ISP?", "id": "Sinyal Clock Komunikasi ISP." },
  { "en": "Apa Itu Rotary Switch Di Proteus?", "id": "Saklar Putar Banyak Posisi." },
  { "en": "Bagaimana Cara Archive Project AutoCAD?", "id": "Gunakan Sheet Set Manager Archive." },
  { "en": "Apa Fungsi Perintah Dimradius Jogged?", "id": "Dimensi Radius Pusat Jauh." },
  { "en": "Apa Fungsi Wire OnRequest Pada Arduino?", "id": "Handler Saat Master Minta Data." },
  { "en": "Apa Fungsi Bill Of Materials Proteus?", "id": "Membuat Daftar Belanja Komponen." },
  { "en": "Apa Fungsi CPU Bus Unit (n) PLC?", "id": "Area Memori Modul Khusus." },
  { "en": "Bagaimana Cara Image Clip AutoCAD?", "id": "Ketik Imageclip Lalu Enter." },
  { "en": "Apa Fungsi Perintah Subtract Region AutoCAD?", "id": "Kurangi Area Bidang 2D." },
  { "en": "Apa Fungsi Perintah Tcircle Pada AutoCAD Express Tools?", "id": "Membuat Lingkaran Di Sekeliling Teks." },
  { "en": "Apa Fungsi Perintah Tcount Pada AutoCAD Express Tools?", "id": "Memberi Nomor Urut Pada Teks." },
  { "en": "Shortcut Apa Untuk Toggle Dynamic Input AutoCAD?", "id": "Tekan Tombol F12." },
  { "en": "Apa Fungsi Perintah Qsave Pada AutoCAD?", "id": "Menyimpan Gambar Secara Cepat." },
  { "en": "Apa Fungsi Perintah Closeall Pada AutoCAD?", "id": "Menutup Semua Jendela Gambar Terbuka." },
  { "en": "Apa Fungsi Pad Mode Pada ARES Proteus?", "id": "Menambahkan Kaki Komponen PCB Manual." },
  { "en": "Apa Fungsi Instruksi MOVD (Move Digit) Pada PLC?", "id": "Memindahkan Empat Bit Data Hex." },
  { "en": "Apa Fungsi Instruksi CPS (Signed Compare) PLC?", "id": "Membandingkan Data Bertanda Negatif." },
  { "en": "Komponen LM339 Di Proteus Berfungsi Sebagai?", "id": "Quad Voltage Comparator." },
  { "en": "Apa Fungsi Perintah Superhatch Pada AutoCAD?", "id": "Mengarsir Menggunakan Gambar Atau Blok." },
  { "en": "Apa Fungsi String Substring Pada Arduino?", "id": "Mengambil Bagian Tertentu Dari Teks." },
  { "en": "Bagaimana Cara Mengatur Point Display Mode AutoCAD?", "id": "Ubah Variabel Pdmode." },
  { "en": "Apa Fungsi Tombol Shift Saat Drawing AutoCAD?", "id": "Mengaktifkan Mode Ortho Sementara." },
  { "en": "Apa Fungsi Perintah Battman Pada AutoCAD?", "id": "Mengelola Atribut Blok Secara Global." },
  { "en": "Komponen IC 4011 Quad NAND Gate Adalah?", "id": "Gerbang NAND CMOS Empat Pintu." },
  { "en": "Apa Fungsi Instruksi APR (Arithmetic Process) PLC?", "id": "Operasi Aritmatika Sinus Cosinus." },
  { "en": "Apa Fungsi Instruksi BCMP (Block Compare) PLC?", "id": "Membandingkan Nilai Dengan Tabel Blok." },
  { "en": "Bagaimana Cara Mengatur Ltscale Pada AutoCAD?", "id": "Ketik Ltscale Lalu Masukkan Nilai." },
  { "en": "Apa Fungsi IsAscii Pada Karakter Arduino?", "id": "Cek Apakah Karakter Kode ASCII." },
  { "en": "Apa Itu Data Memory Read Write (D) PLC?", "id": "Memori Data Bisa Diubah." },
  { "en": "Apa Itu Special I/O Unit Area PLC?", "id": "Memori Untuk Modul I/O Khusus." },
  { "en": "Shortcut Apa Untuk Burn Bootloader Arduino?", "id": "Menu Tools Burn Bootloader." },
  { "en": "Apa Fungsi Library SPI SetClockDivider Arduino?", "id": "Mengatur Kecepatan Clock SPI." },
  { "en": "Bagaimana Cara Mengukur Frekuensi Di Proteus?", "id": "Gunakan Counter Timer Mode Frequency." },
  { "en": "Apa Fungsi Instruksi ASL L (Double Shift Left)?", "id": "Geser Kiri Data 32 Bit." },
  { "en": "Apa Fungsi Instruksi ASR L (Double Shift Right)?", "id": "Geser Kanan Data 32 Bit." },
  { "en": "Apa Fungsi Perintah Dimtedit Pada AutoCAD?", "id": "Menggeser Dan Memutar Teks Dimensi." },
  { "en": "Apa Itu Simplex Communication Pada Serial?", "id": "Komunikasi Satu Arah Saja." },
  { "en": "Apa Itu Null Pointer Pada Arduino?", "id": "Pointer Tidak Menunjuk Kemana Mana." },
  { "en": "Shortcut Apa Untuk Step Into Simulation GX Works?", "id": "Tekan Tombol F8." },
  { "en": "Apa Fungsi IsPrintable Pada Karakter Arduino?", "id": "Cek Karakter Bisa Dicetak." },
  { "en": "Apa Fungsi I2C Debugger Mode I2C Proteus?", "id": "Melihat Data Protokol I2C." },
  { "en": "Apa Fungsi Off Delay Timer (TOF) PLC?", "id": "Timer Menunda Saat Sinyal Mati." },
  { "en": "Bagaimana Cara Membuat Dynamic Block AutoCAD?", "id": "Gunakan Block Editor Parameter." },
  { "en": "Apa Fungsi Operator Equal (Sama Dengan) Arduino?", "id": "Memberikan Nilai Ke Variabel." },
  { "en": "Apa Fungsi Relay SPST 4 Pin Proteus?", "id": "Saklar Satu Induk Satu Kutub." },
  { "en": "Shortcut Apa Untuk Hide Objects AutoCAD?", "id": "Klik Kanan Isolate Hide Objects." },
  { "en": "Apa Fungsi Serial SetTimeout Pada Arduino?", "id": "Mengatur Batas Waktu Baca Data." },
  { "en": "Komponen Apa Yang Menghubungkan Arus Kolektor?", "id": "Resistor Beban Kolektor." },
  { "en": "Apa Fungsi Instruksi BAND (Byte And) PLC?", "id": "Logika AND Tingkat Byte." },
  { "en": "Bagaimana Cara Bind Xref Di AutoCAD?", "id": "Klik Kanan Xref Pilih Bind." },
  { "en": "Apa Itu EEPROM Update Pada Arduino?", "id": "Tulis Jika Nilai Data Berbeda." },
  { "en": "Apa Fungsi SPI Debugger Di Simulasi Proteus?", "id": "Analisis Protokol Komunikasi SPI." },
  { "en": "Apa Shortcut Step Over Simulation GX Works?", "id": "Tekan Tombol F10." },
  { "en": "Apa Fungsi Perintah Ray Pada Gambar AutoCAD?", "id": "Garis Sinar Satu Titik Pusat." },
  { "en": "Apa Fungsi Operator Compound Add (Plus Sama Dengan)?", "id": "Tambah Dan Simpan Hasilnya." },
  { "en": "Apa Itu LogicProbe Big Di Proteus?", "id": "Probe Logika Ukuran Besar." },
  { "en": "Bagaimana Cara Export Ke Format WMF AutoCAD?", "id": "Ketik Wmfout Lalu Enter." },
  { "en": "Apa Fungsi Perintah Solidedit Shell AutoCAD?", "id": "Membuat Rongga Pada Objek Solid." },
  { "en": "Apa Tipe Data Const Byte Arduino?", "id": "Nilai Byte Tetap Tidak Berubah." },
  { "en": "Komponen Apa Pemicu Transistor Darlington?", "id": "Arus Basis Sangat Kecil." },
  { "en": "Apa Fungsi Instruksi BOR (Byte Or) PLC?", "id": "Logika OR Tingkat Byte." },
  { "en": "Shortcut Apa Untuk Layer Previous AutoCAD?", "id": "Ketik Layerp Lalu Enter." },
  { "en": "Apa Fungsi While True Pada Arduino?", "id": "Perulangan Tak Terbatas Sengaja." },
  { "en": "Apa Itu Resolusi ADC 16 Bit?", "id": "Enam Puluh Lima Ribu Tingkat." },
  { "en": "Apa Fungsi Instruksi STC (Set Carry) PLC?", "id": "Mengaktifkan Flag Carry." },
  { "en": "Bagaimana Cara Path Array Di AutoCAD?", "id": "Ketik Arraypath Lalu Enter." },
  { "en": "Apa Fungsi Struct Pada Program Arduino?", "id": "Kumpulan Variabel Berbeda Tipe." },
  { "en": "Apa Itu Sensor Vibration Di Proteus?", "id": "Sensor Getaran Mekanis." },
  { "en": "Apa Shortcut PLC Parameter GX Works?", "id": "Double Klik Parameter PLC." },
  { "en": "Apa Fungsi Perintah Fillet 2D AutoCAD?", "id": "Melengkungkan Sudut Garis Datar." },
  { "en": "Apa Fungsi PulseIn Mode High Arduino?", "id": "Baca Durasi Pulsa Logika Tinggi." },
  { "en": "Komponen Apa Yang Membuang Muatan Kapasitor?", "id": "Resistor Bleeder." },
  { "en": "Apa Fungsi Carry Flag (CY) PLC?", "id": "Menandai Sisa Operasi Aritmatika." },
  { "en": "Shortcut Apa Untuk Upload Arduino Sketch?", "id": "Tekan Ctrl + U." },
  { "en": "Apa Fungsi Serial Println Format BIN?", "id": "Cetak Angka Format Biner." },
  { "en": "Apa Itu Sine Generator Di Proteus?", "id": "Pembangkit Gelombang Sinus Analog." },
  { "en": "Apa Fungsi Instruksi XOR LD PLC?", "id": "Seri Blok Logika Eksklusif." },
  { "en": "Bagaimana Cara Create Light AutoCAD?", "id": "Gunakan Perintah Pointlight." },
  { "en": "Apa Fungsi IsHexadecimalDigit Pada Karakter Arduino?", "id": "Cek Karakter Hexadesimal A F." },
  { "en": "Apa Itu LED Bar Graph Proteus?", "id": "Deretan LED Indikator Level." },
  { "en": "Shortcut Apa Untuk Zoom Extents AutoCAD?", "id": "Ketik Z Lalu E." },
  { "en": "Apa Fungsi Perintah Intersect Region AutoCAD?", "id": "Ambil Area Bidang Berpotongan." },
  { "en": "Apa Fungsi String ToUpperCase Pada Arduino?", "id": "Ubah Teks Ke Huruf Besar." },
  { "en": "Apa Itu IC 555 Di Proteus?", "id": "Timer Multivibrator Astabil Monostabil." },
  { "en": "Apa Fungsi Trace Memory (TRSM) PLC?", "id": "Merekam Data Status Bit." },
  { "en": "Bagaimana Cara Mengukur Luas Region AutoCAD?", "id": "Gunakan Perintah Area Object." },
  { "en": "Apa Fungsi Pin ICSP Pada Arduino?", "id": "Jalur Pemrograman Serial In Circuit." },
  { "en": "Apa Itu Power Terminal Di Proteus?", "id": "Sumber Tegangan Simbol Panah." },
  { "en": "Shortcut Apa Untuk Toolbar AutoCAD?", "id": "Klik Kanan Area Kosong Toolbar." },
  { "en": "Apa Fungsi Perintah Sweep Alignment AutoCAD?", "id": "Atur Tegak Lurus Jalur Sweep." },
  { "en": "Apa Fungsi Tanda Backslash Zero Arduino?", "id": "Karakter Null Terminasi String." },
  { "en": "Apa Itu Voltage Divider Di Rangkaian?", "id": "Pembagi Tegangan Dua Resistor." },
  { "en": "Apa Fungsi Digital PinToInterrupt Arduino?", "id": "Konversi Pin Ke Nomor Interupsi." },
  { "en": "Bagaimana Cara Mengatur Point Shape AutoCAD?", "id": "Ubah Variabel Pdmode." },
  { "en": "Apa Fungsi Pin SS Pada ISP?", "id": "Slave Select Komunikasi SPI." },
  { "en": "Apa Itu Track Mode Proteus?", "id": "Membuat Jalur PCB Manual." },
  { "en": "Apa Shortcut New Tab Arduino?", "id": "Tekan Ctrl + Shift + N." },
  { "en": "Apa Fungsi Perintah Mirror Text AutoCAD?", "id": "Ubah Variabel Mirrtext Jadi 1." },
  { "en": "Apa Fungsi Operator Logika Greater Than (Lebih Besar)?", "id": "Membandingkan Lebih Besar Dari." },
  { "en": "Apa Fungsi Pin AREF Pada ATMega?", "id": "Analog Reference Voltage Pin." },
  { "en": "Apa Itu Thumbwheel Switch Proteus?", "id": "Saklar Putar Angka Desimal." },
  { "en": "Bagaimana Cara Update Field AutoCAD?", "id": "Ketik Updatefield Lalu Enter." },
  { "en": "Apa Fungsi Perintah Dimbreak Pada AutoCAD?", "id": "Memutus Garis Dimensi Bertabrakan." },
  { "en": "Apa Fungsi Wire SetClock Pada Arduino?", "id": "Atur Kecepatan Clock I2C." },
  { "en": "Apa Fungsi Print Layout Di Proteus?", "id": "Mencetak Jalur PCB Ke Kertas." },
  { "en": "Apa Fungsi Condition Flag (CF) PLC?", "id": "Menandai Hasil Perbandingan Data." },
  { "en": "Bagaimana Cara Image Frame AutoCAD?", "id": "Ketik Imageframe Lalu Enter." },
  { "en": "Apa Fungsi Perintah Union 2D AutoCAD?", "id": "Gabungkan Dua Region Datar." },
  { "en": "Apa Fungsi Perintah Laymcur Pada AutoCAD (Computer Aided Design)?", "id": "Mengubah Layer Aktif Sesuai Objek." },
  { "en": "Apa Fungsi Perintah Layuniso Pada AutoCAD?", "id": "Mengembalikan Layer Yang Diisolasi." },
  { "en": "Shortcut Apa Untuk Toggle Quick Properties AutoCAD?", "id": "Tekan Ctrl + Shift + P." },
  { "en": "Apa Fungsi Perintah Textalign Pada AutoCAD?", "id": "Menyelaraskan Posisi Objek Teks." },
  { "en": "Apa Fungsi Perintah Dataextraction Pada AutoCAD?", "id": "Ekspor Data Objek Ke Tabel." },
  { "en": "Apa Fungsi Zone Mode Pada ARES Proteus?", "id": "Membuat Area Tembaga PCB." },
  { "en": "Apa Fungsi Instruksi XCHG (Block Exchange) PLC?", "id": "Menukar Isi Dua Blok Memori." },
  { "en": "Apa Fungsi Instruksi APR (Arithmetic Processor) PLC?", "id": "Menghitung Fungsi Trigonometri Sin Cos." },
  { "en": "Komponen LM386 Di Proteus Berfungsi Sebagai?", "id": "Penguat Audio Daya Rendah." },
  { "en": "Apa Fungsi Perintah Appload Pada AutoCAD?", "id": "Memuat Aplikasi Tambahan Atau LISP." },
  { "en": "Apa Fungsi Library WDT (Watchdog Timer) Arduino?", "id": "Reset Otomatis Jika Sistem Hang." },
  { "en": "Bagaimana Cara Menampilkan Koordinat Kursor AutoCAD?", "id": "Tekan Ctrl + I." },
  { "en": "Apa Fungsi Tombol Shift Klik Kanan AutoCAD?", "id": "Membuka Menu Object Snap Shortcut." },
  { "en": "Apa Fungsi Perintah Ucsicon Pada AutoCAD?", "id": "Mengatur Tampilan Ikon Sumbu XY." },
  { "en": "Komponen IC 4013 Dual D Flip Flop Adalah?", "id": "Dua Flip Flop Tipe D." },
  { "en": "Apa Fungsi Instruksi FSTR (Floating Point String)?", "id": "Konversi Desimal Ke String Teks." },
  { "en": "Apa Fungsi Instruksi SDEC (Signed Decimal) PLC?", "id": "Konversi Biner Tanda Ke Desimal." },
  { "en": "Bagaimana Cara Mengatur Selection Cycling AutoCAD?", "id": "Aktifkan Tombol Selection Cycling Statusbar." },
  { "en": "Apa Fungsi Interrupt Mode Rising Arduino?", "id": "Picu Saat Sinyal Naik." },
  { "en": "Apa Itu Data Memory Fixed (D) PLC?", "id": "Memori Data Nilai Tetap." },
  { "en": "Apa Itu Link Register Area (CIO) PLC?", "id": "Area Memori Pertukaran Data Link." },
  { "en": "Shortcut Apa Untuk Export Sketch Arduino?", "id": "Menu Sketch Export Compiled Binary." },
  { "en": "Apa Fungsi Library SPI SetDataMode Arduino?", "id": "Mengatur Mode Fase Polaritas SPI." },
  { "en": "Bagaimana Cara Menambah Titik Probe Proteus?", "id": "Pilih Voltage Probe Mode." },
  { "en": "Apa Fungsi Instruksi ROL L (Rotate Left Double)?", "id": "Putar Kiri Data 32 Bit." },
  { "en": "Apa Fungsi Instruksi ROR L (Rotate Right Double)?", "id": "Putar Kanan Data 32 Bit." },
  { "en": "Apa Fungsi Perintah Diminspect Pada Dimensi AutoCAD?", "id": "Menambah Info Inspeksi Pada Dimensi." },
  { "en": "Apa Itu Synchronous Communication Pada Serial?", "id": "Komunikasi Dengan Sinyal Clock." },
  { "en": "Apa Itu Dangling Pointer Pada Arduino?", "id": "Pointer Menunjuk Memori Tidak Valid." },
  { "en": "Shortcut Apa Untuk Run Simulation GX Works?", "id": "Tekan Alt + F4 (Tutup)." },
  { "en": "Apa Fungsi IsControl Pada Karakter Arduino?", "id": "Cek Karakter Kontrol Non Cetak." },
  { "en": "Apa Fungsi Fourier Graph Di Proteus?", "id": "Analisis Spektrum Frekuensi Sinyal." },
  { "en": "Apa Fungsi Monostable Timer Pada PLC?", "id": "Output Aktif Sesaat Saat Trigger." },
  { "en": "Bagaimana Cara Reset Block AutoCAD?", "id": "Gunakan Perintah Resetblock." },
  { "en": "Apa Fungsi Operator Not (Tanda Seru) Arduino?", "id": "Membalik Nilai Kebenaran Logika." },
  { "en": "Apa Fungsi Relay DPDT 8 Pin Proteus?", "id": "Saklar Dua Induk Dua Kutub." },
  { "en": "Shortcut Apa Untuk Isolate Objects AutoCAD?", "id": "Klik Kanan Isolate Objects." },
  { "en": "Apa Fungsi Serial ReadString Pada Arduino?", "id": "Membaca Seluruh String Masuk." },
  { "en": "Komponen Apa Yang Mengatur Arus Emitter?", "id": "Resistor Stabilisasi Emitter." },
  { "en": "Apa Fungsi Instruksi BXOR (Byte Xor) PLC?", "id": "Logika XOR Tingkat Byte." },
  { "en": "Bagaimana Cara Xref Bind Insert AutoCAD?", "id": "Gabungkan Xref Jadi Blok Lokal." },
  { "en": "Apa Itu EEPROM Length Pada Arduino?", "id": "Mendapatkan Ukuran Total Memori." },
  { "en": "Apa Fungsi Distorsion Analyzer Di Proteus?", "id": "Mengukur Distorsi Sinyal Audio." },
  { "en": "Apa Shortcut Step Out Simulation GX Works?", "id": "Tekan Shift + F11." },
  { "en": "Apa Fungsi Perintah Sketch Pada AutoCAD?", "id": "Menggambar Garis Bebas Freehand." },
  { "en": "Apa Fungsi Operator Compound Subtract (Min Sama Dengan)?", "id": "Kurangi Dan Simpan Hasilnya." },
  { "en": "Apa Itu LogicProbe Small Di Proteus?", "id": "Probe Logika Ukuran Kecil." },
  { "en": "Bagaimana Cara Export Ke Format EPS AutoCAD?", "id": "Ketik Psout Lalu Enter." },
  { "en": "Apa Fungsi Perintah Solidedit Check AutoCAD?", "id": "Memeriksa Validitas Objek Solid." },
  { "en": "Apa Tipe Data Volatile Int Arduino?", "id": "Integer Berubah Di Interupsi." },
  { "en": "Komponen Apa Pemicu SCR Melalui Gate?", "id": "Arus Trigger Positif." },
  { "en": "Apa Fungsi Instruksi BNOT (Byte Not) PLC?", "id": "Inversi Logika Tingkat Byte." },
  { "en": "Shortcut Apa Untuk Quick Properties AutoCAD?", "id": "Tekan Ctrl + Shift + P." },
  { "en": "Apa Fungsi Do While Loop Arduino?", "id": "Jalankan Minimal Satu Kali." },
  { "en": "Apa Itu Resolusi PWM 10 Bit?", "id": "Seribu Dua Puluh Empat." },
  { "en": "Apa Fungsi Instruksi CLC (Clear Carry) PLC?", "id": "Matikan Flag Carry." },
  { "en": "Bagaimana Cara Polar Array Di AutoCAD?", "id": "Ketik Arraypolar Lalu Enter." },
  { "en": "Apa Fungsi Union Pada C++ Arduino?", "id": "Berbagi Memori Tipe Data Beda." },
  { "en": "Apa Itu Sensor Hall Effect Proteus?", "id": "Sensor Deteksi Medan Magnet." },
  { "en": "Apa Shortcut PLC Memory GX Works?", "id": "Menu Online Monitor Memory." },
  { "en": "Apa Fungsi Perintah Blend Curve AutoCAD?", "id": "Membuat Kurva Penghubung Halus." },
  { "en": "Apa Fungsi Analog ReadResolution 10 Bit?", "id": "Resolusi Baca Seribu Dua Puluh Empat." },
  { "en": "Komponen Apa Yang Menghubungkan Arus Source?", "id": "Resistor Source FET." },
  { "en": "Apa Fungsi Overflow Flag (OF) PLC?", "id": "Menandai Hasil Melebihi Kapasitas." },
  { "en": "Shortcut Apa Untuk Serial Monitor Arduino?", "id": "Tekan Ctrl + Shift + M." },
  { "en": "Apa Fungsi Serial Println Format OCT?", "id": "Cetak Angka Format Oktal." },
  { "en": "Apa Itu Pulse Generator Di Proteus?", "id": "Pembangkit Pulsa Digital Terprogram." },
  { "en": "Apa Fungsi Instruksi NAND LD PLC?", "id": "Seri Blok Logika NAND." },
  { "en": "Bagaimana Cara Create Material AutoCAD?", "id": "Ketik Mat Lalu Enter." },
  { "en": "Apa Fungsi IsPunctuation Pada Karakter Arduino?", "id": "Cek Karakter Tanda Baca." },
  { "en": "Apa Itu Traffic Lights Model Proteus?", "id": "Simulasi Lampu Lalu Lintas." },
  { "en": "Shortcut Apa Untuk Zoom Dynamic AutoCAD?", "id": "Ketik Z Lalu D." },
  { "en": "Apa Fungsi Perintah Subtract 2D AutoCAD?", "id": "Potong Area Region Datar." },
  { "en": "Apa Fungsi String ToInt Base Arduino?", "id": "Konversi String Ke Basis Angka." },
  { "en": "Apa Itu IC 4026 Di Proteus?", "id": "Penghitung Dekade Output 7 Segment." },
  { "en": "Apa Fungsi Index Register (IR) Omron?", "id": "Pointer Alamat Memori Indirect." },
  { "en": "Bagaimana Cara Mengukur Jarak 3D AutoCAD?", "id": "Gunakan Perintah 3dpoly Dan List." },
  { "en": "Apa Fungsi Pin XTAL1 Dan XTAL2?", "id": "Koneksi Kristal Clock Eksternal." },
  { "en": "Apa Itu Ground Terminal Di Proteus?", "id": "Referensi Nol Volt Rangkaian." },
  { "en": "Shortcut Apa Untuk Ribbon Close AutoCAD?", "id": "Ketik Ribbonclose Lalu Enter." },
  { "en": "Apa Fungsi Perintah Helix Turn Height?", "id": "Atur Jarak Antar Putaran." },
  { "en": "Apa Fungsi Tanda Backslash B Arduino?", "id": "Karakter Backspace Hapus Belakang." },
  { "en": "Apa Itu Voltage Regulator Linear?", "id": "Penstabil Tegangan Analog Panas." },
  { "en": "Apa Fungsi Analog Reference Internal Arduino?", "id": "Referensi Tegangan 1.1 Volt." },
  { "en": "Bagaimana Cara Mengatur Point Type AutoCAD?", "id": "Ketik Ptype Lalu Enter." },
  { "en": "Apa Fungsi Pin INT0 Pada Arduino?", "id": "Pin Interupsi Eksternal Nol." },
  { "en": "Apa Itu Pad Stack Di Proteus?", "id": "Bentuk Lubang Kaki Komponen." },
  { "en": "Apa Shortcut Show Sketch Folder Arduino?", "id": "Tekan Ctrl + K." },
  { "en": "Apa Fungsi Perintah Align Text AutoCAD?", "id": "Ratakan Teks Sesuai Garis." },
  { "en": "Apa Fungsi Operator Logika Less Than (Lebih Kecil)?", "id": "Membandingkan Lebih Kecil Dari." },
  { "en": "Apa Fungsi Pin AVCC Pada Arduino?", "id": "Power Supply Untuk ADC." },
  { "en": "Apa Itu Logic State Source Proteus?", "id": "Sumber Logika 0 Atau 1." },
  { "en": "Bagaimana Cara Rename Layer AutoCAD?", "id": "Ketik Rename Lalu Enter." },
  { "en": "Apa Fungsi Perintah Dimjogang Pada AutoCAD?", "id": "Dimensi Sudut Dengan Tekukan." },
  { "en": "Apa Fungsi Wire SetClockStretchLimit Arduino?", "id": "Atur Batas Waktu Clock Stretching." },
  { "en": "Apa Fungsi Output To Printer Proteus?", "id": "Cetak Desain Langsung Ke Printer." },
  { "en": "Apa Fungsi Data Register Index (DR) PLC?", "id": "Offset Alamat Memori Indirect." },
  { "en": "Bagaimana Cara Image Quality AutoCAD?", "id": "Ketik Imagequality Lalu Enter." },
  { "en": "Apa Fungsi Perintah Intersect 2D AutoCAD?", "id": "Ambil Irisan Region Datar." },
  { "en": "Apa Fungsi Perintah Laymch Pada AutoCAD (Computer Aided Design)?", "id": "Mencocokkan Layer Objek Ke Tujuan." },
  { "en": "Apa Fungsi Perintah Laycur Pada AutoCAD?", "id": "Mengubah Layer Objek Ke Current." },
  { "en": "Shortcut Apa Untuk Next Layout Tab AutoCAD?", "id": "Tekan Ctrl + Page Down." },
  { "en": "Apa Fungsi Perintah Dimspace Pada AutoCAD?", "id": "Mengatur Jarak Antar Garis Dimensi." },
  { "en": "Apa Fungsi Perintah Dimjogline Pada AutoCAD?", "id": "Menambah Garis Jog Pada Dimensi." },
  { "en": "Apa Fungsi Connectivity Mode Pada ARES Proteus?", "id": "Memeriksa Sambungan Jalur PCB." },
  { "en": "Apa Fungsi Instruksi NEG (Two's Complement) PLC?", "id": "Mengubah Nilai Positif Jadi Negatif." },
  { "en": "Apa Fungsi Instruksi FADD (Floating Point Add)?", "id": "Penjumlahan Bilangan Desimal PLC." },
  { "en": "Komponen LM35 Di Proteus Berfungsi Sebagai?", "id": "Sensor Suhu Presisi Linear." },
  { "en": "Apa Fungsi Perintah Txt2mtxt Pada AutoCAD?", "id": "Konversi Teks Biasa Ke Multiline." },
  { "en": "Apa Fungsi String ToFloat Pada Arduino?", "id": "Konversi Teks Ke Angka Desimal." },
  { "en": "Bagaimana Cara Mengatur Crosshair Size AutoCAD?", "id": "Ubah Variabel Cursorsize." },
  { "en": "Apa Fungsi Tombol F12 Pada Aplikasi AutoCAD?", "id": "Mengaktifkan Dynamic Input." },
  { "en": "Apa Fungsi Perintah Qselect Pada AutoCAD?", "id": "Seleksi Objek Berdasarkan Kriteria." },
  { "en": "Komponen IC 4511 BCD To 7 Segment?", "id": "Driver Layar 7 Segment CMOS." },
  { "en": "Apa Fungsi Instruksi FSUB (Floating Point Sub)?", "id": "Pengurangan Bilangan Desimal PLC." },
  { "en": "Apa Fungsi Instruksi FMUL (Floating Point Mul)?", "id": "Perkalian Bilangan Desimal PLC." },
  { "en": "Bagaimana Cara Mengatur Grip Size AutoCAD?", "id": "Ubah Variabel Gripsize." },
  { "en": "Apa Fungsi Interrupt Mode Change Arduino?", "id": "Picu Saat Logika Berubah." },
  { "en": "Apa Itu Data Memory Special (D) PLC?", "id": "Memori Data Fungsi Khusus." },
  { "en": "Apa Itu Holding Relay (HR) Pada Omron?", "id": "Relay Tahan Saat Listrik Mati." },
  { "en": "Shortcut Apa Untuk Comment Code Arduino?", "id": "Tekan Ctrl + Slash." },
  { "en": "Apa Fungsi Library SPI Transfer16 Arduino?", "id": "Transfer Data SPI 16 Bit." },
  { "en": "Bagaimana Cara Mengubah Warna Background Proteus?", "id": "Menu Template Set Design Colours." },
  { "en": "Apa Fungsi Instruksi WAND L (Double Word And)?", "id": "Logika AND Data 32 Bit." },
  { "en": "Apa Fungsi Instruksi WOR L (Double Word Or)?", "id": "Logika OR Data 32 Bit." },
  { "en": "Apa Fungsi Perintah Dimtmove Pada AutoCAD?", "id": "Memindahkan Teks Dimensi Bebas." },
  { "en": "Apa Itu Asynchronous Communication Pada Serial?", "id": "Komunikasi Tanpa Sinyal Clock." },
  { "en": "Apa Itu Wild Pointer Pada Arduino?", "id": "Pointer Belum Diinisialisasi." },
  { "en": "Shortcut Apa Untuk Find Instruction GX Works?", "id": "Tekan Ctrl + F." },
  { "en": "Apa Fungsi IsSpace Pada Karakter Arduino?", "id": "Cek Karakter Spasi Putih." },
  { "en": "Apa Fungsi Noise Analysis Di Proteus?", "id": "Mengukur Gangguan Sinyal Rangkaian." },
  { "en": "Apa Fungsi Timer Off Delay (TOF) PLC?", "id": "Menghitung Waktu Saat Input Mati." },
  { "en": "Bagaimana Cara Membuat Attributes Block AutoCAD?", "id": "Ketik Attdef Lalu Enter." },
  { "en": "Apa Fungsi Operator Tilde (Bitwise NOT) Arduino?", "id": "Membalik Semua Bit Data." },
  { "en": "Apa Fungsi Relay Latching 2 Coil Proteus?", "id": "Relay Dua Koil Set Reset." },
  { "en": "Shortcut Apa Untuk Unisolate Objects AutoCAD?", "id": "Klik Kanan Unisolate Objects." },
  { "en": "Apa Fungsi Serial ReadStringUntil Pada Arduino?", "id": "Baca String Sampai Karakter Tertentu." },
  { "en": "Komponen Apa Yang Membatasi Arus Basis?", "id": "Resistor Basis." },
  { "en": "Apa Fungsi Instruksi WXOR L (Double Xor)?", "id": "Logika XOR Data 32 Bit." },
  { "en": "Bagaimana Cara Xref Reload AutoCAD?", "id": "Perbarui Tampilan Referensi Eksternal." },
  { "en": "Apa Itu EEPROM Commit Pada ESP32?", "id": "Menyimpan Data Ke Flash Memory." },
  { "en": "Apa Fungsi Audio Graph Di Proteus?", "id": "Menampilkan Gelombang Sinyal Audio." },
  { "en": "Apa Shortcut Step In Simulation GX Works?", "id": "Tekan Tombol F8." },
  { "en": "Apa Fungsi Perintah Solid Pada AutoCAD?", "id": "Membuat Segitiga Atau Segiempat Solid." },
  { "en": "Apa Fungsi Operator Compound Multiply (Bintang Sama Dengan)?", "id": "Kali Dan Simpan Hasilnya." },
  { "en": "Apa Itu Voltage Probe Di Proteus?", "id": "Alat Ukur Tegangan Sesaat." },
  { "en": "Bagaimana Cara Export Ke Format DWF AutoCAD?", "id": "Ketik Dwfout Lalu Enter." },
  { "en": "Apa Fungsi Perintah Solidedit Face Extrude?", "id": "Menarik Permukaan Solid Keluar." },
  { "en": "Apa Tipe Data Const Int Arduino?", "id": "Integer Tetap Tidak Bisa Diubah." },
  { "en": "Komponen Apa Pemicu Triac Melalui Gate?", "id": "Arus Trigger Bolak Balik." },
  { "en": "Apa Fungsi Instruksi COMP (Compare) PLC?", "id": "Membandingkan Dua Nilai Data." },
  { "en": "Shortcut Apa Untuk Quick Calculator AutoCAD?", "id": "Tekan Ctrl + 8." },
  { "en": "Apa Fungsi For Loop Arduino?", "id": "Perulangan Terhitung Jumlah Iterasi." },
  { "en": "Apa Itu Resolusi PWM 8 Bit Arduino?", "id": "Dua Ratus Lima Puluh Lima." },
  { "en": "Apa Fungsi Instruksi STC (Set Carry) PLC?", "id": "Aktifkan Flag Carry." },
  { "en": "Bagaimana Cara Rectangular Array Di AutoCAD?", "id": "Ketik Arrayrect Lalu Enter." },
  { "en": "Apa Fungsi Enum Pada Program Arduino?", "id": "Mendefinisikan Tipe Data Enumerasi." },
  { "en": "Apa Itu Sensor Ultrasonic SRF04 Proteus?", "id": "Modul Pengukur Jarak Ultrasonik." },
  { "en": "Apa Shortcut PLC Transfer To GX Works?", "id": "Menu Online Write To PLC." },
  { "en": "Apa Fungsi Perintah Fillet Trim AutoCAD?", "id": "Potong Sisa Garis Fillet." },
  { "en": "Apa Fungsi Analog ReadResolution 12 Bit?", "id": "Baca Data Empat Ribu Tingkat." },
  { "en": "Komponen Apa Yang Menghubungkan Arus Drain?", "id": "Resistor Beban Drain." },
  { "en": "Apa Fungsi Underflow Flag (UF) PLC?", "id": "Menandai Hasil Bawah Batas." },
  { "en": "Shortcut Apa Untuk Show Plotter Arduino?", "id": "Tekan Ctrl + Shift + L." },
  { "en": "Apa Fungsi Serial Println Format HEX?", "id": "Cetak Angka Format Hexadesimal." },
  { "en": "Apa Itu Clock Frequency Di Proteus?", "id": "Kecepatan Detak Simulasi CPU." },
  { "en": "Apa Fungsi Instruksi NOR LD PLC?", "id": "Seri Blok Logika NOR." },
  { "en": "Bagaimana Cara Create Sun Properties AutoCAD?", "id": "Ketik Sunproperties Lalu Enter." },
  { "en": "Apa Fungsi IsGraph Pada Karakter Arduino?", "id": "Cek Karakter Memiliki Representasi Grafis." },
  { "en": "Apa Itu 14 Segment Display Proteus?", "id": "Penampil Karakter Alfanumerik Lengkap." },
  { "en": "Shortcut Apa Untuk Zoom Scale AutoCAD?", "id": "Ketik Z Lalu S." },
  { "en": "Apa Fungsi Perintah Intersect 3D AutoCAD?", "id": "Ambil Volume Solid Berpotongan." },
  { "en": "Apa Fungsi String IndexOf Pada Arduino?", "id": "Cari Posisi Karakter Dalam String." },
  { "en": "Apa Itu IC 74147 Di Proteus?", "id": "Priority Encoder 10 Line To BCD." },
  { "en": "Apa Fungsi Pulse I/O (P_IO) PLC?", "id": "Akses Input Output Pulsa." },
  { "en": "Bagaimana Cara Mengukur Volume 3D AutoCAD?", "id": "Gunakan Perintah Massprop." },
  { "en": "Apa Fungsi Pin RESET Pada ATMega328?", "id": "Restart Program Mikrokontroler." },
  { "en": "Apa Itu VCC Terminal Di Proteus?", "id": "Sumber Tegangan 5 Volt." },
  { "en": "Shortcut Apa Untuk Ribbon AutoCAD?", "id": "Ketik Ribbon Lalu Enter." },
  { "en": "Apa Fungsi Perintah Helix Base Radius?", "id": "Atur Jari Jari Dasar Spiral." },
  { "en": "Apa Fungsi Tanda Backslash F Arduino?", "id": "Karakter Form Feed Halaman." },
  { "en": "Apa Itu Power Supply Switching?", "id": "Regulator Tegangan Efisiensi Tinggi." },
  { "en": "Apa Fungsi Analog WriteResolution 10 Bit?", "id": "Tulis PWM Seribu Tingkat." },
  { "en": "Bagaimana Cara Mengatur Point Style Display?", "id": "Ketik Ddptype Lalu Enter." },
  { "en": "Apa Fungsi Pin MISO Pada Arduino?", "id": "Master In Slave Out SPI." },
  { "en": "Apa Itu Via Mode Di Proteus?", "id": "Membuat Lubang Tembus Antar Layer." },
  { "en": "Apa Shortcut Show Library Arduino?", "id": "Tekan Ctrl + Shift + I." },
  { "en": "Apa Fungsi Perintah Rotate Copy AutoCAD?", "id": "Putar Sambil Menyalin Objek." },
  { "en": "Apa Fungsi Operator Logika Equal Strict?", "id": "Cek Sama Nilai Dan Tipe." },
  { "en": "Apa Fungsi Pin AREF Pada Arduino Mega?", "id": "Tegangan Referensi Analog Input." },
  { "en": "Apa Itu SPDT Switch Di Proteus?", "id": "Saklar Tukar Satu Induk." },
  { "en": "Bagaimana Cara Update Thumbnail AutoCAD?", "id": "Ketik Updatethumbs Lalu Enter." },
  { "en": "Apa Fungsi Perintah Dimjogged Radius?", "id": "Dimensi Radius Pusat Jauh." },
  { "en": "Apa Fungsi Wire SetTimeout Pada Arduino?", "id": "Atur Batas Waktu I2C." },
  { "en": "Apa Fungsi 3D Visualization Di Proteus?", "id": "Melihat Bentuk Fisik PCB 3D." },
  { "en": "Apa Fungsi Always ON Flag (P_On)?", "id": "Bit Selalu Bernilai Satu." },
  { "en": "Bagaimana Cara Insert OLE AutoCAD?", "id": "Ketik Insertobj Lalu Enter." },
  { "en": "Apa Fungsi Perintah Subtract 3D AutoCAD?", "id": "Potong Volume Solid Utama." },
  { "en": "Apa Fungsi Perintah Laytrans Pada AutoCAD (Computer Aided Design)?", "id": "Menerjemahkan Standar Layer Gambar." },
  { "en": "Apa Fungsi Perintah Layiso Pada AutoCAD?", "id": "Mengisolasi Layer Objek Terpilih." },
  { "en": "Shortcut Apa Untuk Print Preview AutoCAD?", "id": "Pilih Menu Print Preview." },
  { "en": "Apa Fungsi Perintah Dimcenter Pada Dimensi AutoCAD?", "id": "Membuat Tanda Pusat Lingkaran." },
  { "en": "Apa Fungsi Perintah Dimradius Pada Dimensi AutoCAD?", "id": "Memberi Dimensi Jari Jari." },
  { "en": "Apa Fungsi Ratsnest Mode Pada ARES Proteus?", "id": "Melihat Koneksi Udara Belum Rut." },
  { "en": "Apa Fungsi Instruksi COM (Complement) Pada PLC?", "id": "Membalik Logika Semua Bit." },
  { "en": "Apa Fungsi Instruksi WDT (Watchdog Timer) PLC?", "id": "Refresh Timer Pengawas Sistem." },
  { "en": "Komponen LM324 Quad Op Amp Adalah?", "id": "Empat Penguat Operasional Satu Paket." },
  { "en": "Apa Fungsi Perintah Bedit Pada AutoCAD?", "id": "Membuka Editor Definisi Blok." },
  { "en": "Apa Fungsi String ToInt Pada Arduino?", "id": "Mengubah String Menjadi Integer." },
  { "en": "Bagaimana Cara Mengatur Aperture Size AutoCAD?", "id": "Ubah Variabel Aperture." },
  { "en": "Apa Fungsi Tombol F9 Pada Aplikasi AutoCAD?", "id": "Mengaktifkan Snap Mode Grid." },
  { "en": "Apa Fungsi Perintah Filter Pada AutoCAD?", "id": "Memfilter Seleksi Objek Spesifik." },
  { "en": "Komponen IC 4017 Decade Counter Adalah?", "id": "Penghitung Sepuluh Langkah Output." },
  { "en": "Apa Fungsi Instruksi SWAP (Swap Bytes) PLC?", "id": "Tukar Posisi Byte Data." },
  { "en": "Apa Fungsi Instruksi XFER (Block Transfer) PLC?", "id": "Salin Blok Memori Data." },
  { "en": "Bagaimana Cara Mengatur Pickbox Size AutoCAD?", "id": "Ubah Variabel Pickbox." },
  { "en": "Apa Fungsi Interrupt Mode Low Arduino?", "id": "Picu Terus Saat Logika Rendah." },
  { "en": "Apa Itu Data Memory Special (SD) PLC?", "id": "Memori Status Sistem PLC." },
  { "en": "Apa Itu Work Area (W) Pada Omron?", "id": "Relay Internal Kerja Sementara." },
  { "en": "Shortcut Apa Untuk Upload Using Programmer Arduino?", "id": "Tekan Ctrl + Shift + U." },
  { "en": "Apa Fungsi Library SPI BeginTransaction Arduino?", "id": "Mulai Sesi Transaksi SPI." },
  { "en": "Bagaimana Cara Mengubah Grid Spacing Proteus?", "id": "Menu View Snap." },
  { "en": "Apa Fungsi Instruksi ANDW (And Word) PLC?", "id": "Logika AND Tingkat Word." },
  { "en": "Apa Fungsi Instruksi ORW (Or Word) PLC?", "id": "Logika OR Tingkat Word." },
  { "en": "Apa Fungsi Perintah Dimedit New AutoCAD?", "id": "Mengganti Teks Dimensi Baru." },
  { "en": "Apa Itu Full Duplex Communication Serial?", "id": "Komunikasi Dua Arah Bersamaan." },
  { "en": "Apa Itu Void Pointer Pada Arduino?", "id": "Pointer Tanpa Tipe Data." },
  { "en": "Shortcut Apa Untuk Stop Simulation GX Works?", "id": "Tekan Alt + F4." },
  { "en": "Apa Fungsi IsDigit Pada Karakter Arduino?", "id": "Cek Apakah Karakter Angka." },
  { "en": "Apa Fungsi AC Sweep Graph Proteus?", "id": "Analisis Respon Frekuensi Rangkaian." },
  { "en": "Apa Fungsi Timer On Delay (TON) PLC?", "id": "Menghitung Waktu Saat Input Hidup." },
  { "en": "Bagaimana Cara Membuat Block Definition AutoCAD?", "id": "Ketik Block Lalu Enter." },
  { "en": "Apa Fungsi Operator XOR (Topi) Arduino?", "id": "Logika Exclusive OR Bitwise." },
  { "en": "Apa Fungsi Relay SPDT 5 Pin Proteus?", "id": "Saklar Tukar Satu Induk." },
  { "en": "Shortcut Apa Untuk Object Snap Settings AutoCAD?", "id": "Tekan F3 Atau Osnap." },
  { "en": "Apa Fungsi Serial ParseInt Pada Arduino?", "id": "Ambil Angka Bulat Dari Buffer." },
  { "en": "Komponen Apa Yang Menghambat Arus Kolektor?", "id": "Resistor Beban Kolektor." },
  { "en": "Apa Fungsi Instruksi XORW (Xor Word) PLC?", "id": "Logika XOR Tingkat Word." },
  { "en": "Bagaimana Cara Xref Attach AutoCAD?", "id": "Lampirkan File Referensi Eksternal." },
  { "en": "Apa Itu EEPROM Put Pada Arduino?", "id": "Menulis Data Sembarang Tipe." },
  { "en": "Apa Fungsi Digital Graph Di Proteus?", "id": "Menampilkan Logika Waktu Digital." },
  { "en": "Apa Shortcut Run Simulation GX Works?", "id": "Menu Debug Start Simulation." },
  { "en": "Apa Fungsi Perintah Donut Pada AutoCAD?", "id": "Membuat Lingkaran Cincin Solid." },
  { "en": "Apa Fungsi Operator Compound Divide (Garis Miring Sama Dengan)?", "id": "Bagi Dan Simpan Hasilnya." },
  { "en": "Apa Itu Current Probe Di Proteus?", "id": "Alat Ukur Arus Sesaat." },
  { "en": "Bagaimana Cara Export Ke Format PNG AutoCAD?", "id": "Ketik Pngout Lalu Enter." },
  { "en": "Apa Fungsi Perintah Solidedit Face Taper?", "id": "Miringkan Permukaan Solid Sudut." },
  { "en": "Apa Tipe Data Unsigned Int Arduino?", "id": "Integer Positif Tanpa Tanda." },
  { "en": "Komponen Apa Pemicu MOSFET Melalui Gate?", "id": "Tegangan Medan Listrik Gate." },
  { "en": "Apa Fungsi Instruksi NEG (Negation) PLC?", "id": "Membalik Nilai Positif Negatif." },
  { "en": "Shortcut Apa Untuk Calculator AutoCAD?", "id": "Ketik Qc Lalu Enter." },
  { "en": "Apa Fungsi While Loop Arduino?", "id": "Perulangan Selama Kondisi Benar." },
  { "en": "Apa Itu Resolusi PWM 12 Bit Arduino?", "id": "Empat Ribu Sembilan Puluh Enam." },
  { "en": "Apa Fungsi Instruksi CLC (Clear Carry) PLC?", "id": "Reset Flag Carry Ke Nol." },
  { "en": "Bagaimana Cara Path Array Di AutoCAD?", "id": "Salin Objek Sepanjang Jalur." },
  { "en": "Apa Fungsi Typedef Pada Program Arduino?", "id": "Memberi Nama Baru Tipe Data." },
  { "en": "Apa Itu Sensor Infrared Obstacle Proteus?", "id": "Sensor Deteksi Halangan Inframerah." },
  { "en": "Apa Shortcut PLC Transfer From GX Works?", "id": "Menu Online Read From PLC." },
  { "en": "Apa Fungsi Perintah Fillet Radius AutoCAD?", "id": "Atur Jari Jari Lengkungan." },
  { "en": "Apa Fungsi Analog ReadResolution 10 Bit?", "id": "Baca Data Seribu Dua Puluh Empat." },
  { "en": "Komponen Apa Yang Menghubungkan Arus Gate?", "id": "Resistor Input Gate." },
  { "en": "Apa Fungsi Equals Flag (EQ) PLC?", "id": "Menandai Hasil Sama Dengan." },
  { "en": "Shortcut Apa Untuk Open Serial Plotter Arduino?", "id": "Tekan Ctrl + Shift + L." },
  { "en": "Apa Fungsi Serial Println Format DEC?", "id": "Cetak Angka Format Desimal." },
  { "en": "Apa Itu Signal Generator Di Proteus?", "id": "Pembangkit Sinyal Analog Variable." },
  { "en": "Apa Fungsi Instruksi NANDW (Nand Word) PLC?", "id": "Logika NAND Tingkat Word." },
  { "en": "Bagaimana Cara Create Point Light AutoCAD?", "id": "Ketik Pointlight Lalu Enter." },
  { "en": "Apa Fungsi IsXdigit Pada Karakter Arduino?", "id": "Cek Karakter Hexadesimal." },
  { "en": "Apa Itu Alphanumeric LCD Proteus?", "id": "Layar Teks Karakter Standar." },
  { "en": "Shortcut Apa Untuk Zoom Center AutoCAD?", "id": "Ketik Z Lalu C." },
  { "en": "Apa Fungsi Perintah Intersect Solid AutoCAD?", "id": "Ambil Volume Solid Berpotongan." },
  { "en": "Apa Fungsi String LastIndexOf Pada Arduino?", "id": "Cari Posisi Terakhir Karakter." },
  { "en": "Apa Itu IC 7404 Hex Inverter?", "id": "Enam Gerbang Logika NOT." },
  { "en": "Apa Fungsi Pulse Control (PULS) PLC?", "id": "Mengontrol Output Pulsa PLC." },
  { "en": "Bagaimana Cara Mengukur Luas Area AutoCAD?", "id": "Gunakan Perintah Area." },
  { "en": "Apa Fungsi Pin VREF Pada ADC?", "id": "Tegangan Referensi Konverter Analog." },
  { "en": "Apa Itu Input Terminal Di Proteus?", "id": "Titik Masukan Sinyal Rangkaian." },
  { "en": "Shortcut Apa Untuk Layer Manager AutoCAD?", "id": "Ketik Layer Lalu Enter." },
  { "en": "Apa Fungsi Perintah Helix Top Radius?", "id": "Atur Jari Jari Atas Spiral." },
  { "en": "Apa Fungsi Tanda Backslash N Arduino?", "id": "Karakter Newline Baris Baru." },
  { "en": "Apa Itu Zener Diode Di Proteus?", "id": "Dioda Penstabil Tegangan Mundur." },
  { "en": "Apa Fungsi Analog WriteResolution 12 Bit?", "id": "Tulis PWM Empat Ribu Tingkat." },
  { "en": "Bagaimana Cara Mengatur Point Display Size?", "id": "Ubah Variabel Pdsize." },
  { "en": "Apa Fungsi Pin MOSI Pada Arduino?", "id": "Master Out Slave In SPI." },
  { "en": "Apa Itu Teardrop Mode Di Proteus?", "id": "Memperkuat Sambungan Pad Jalur." },
  { "en": "Apa Shortcut Add Library Arduino?", "id": "Menu Sketch Include Library." },
  { "en": "Apa Fungsi Perintah Mirror 3D AutoCAD?", "id": "Cerminkan Objek Ruang 3D." },
  { "en": "Apa Fungsi Operator Logika Or (Garis Dua)?", "id": "Logika OR Boolean." },
  { "en": "Apa Fungsi Pin SDA Pada Arduino Mega?", "id": "Data Komunikasi Serial I2C." },
  { "en": "Apa Itu Push Button Momentary Proteus?", "id": "Saklar Tekan Lepas Balik." },
  { "en": "Bagaimana Cara Update Data Link AutoCAD?", "id": "Ketik Datalinkupdate Lalu Enter." },
  { "en": "Apa Fungsi Perintah Dimjogged Angle?", "id": "Dimensi Sudut Pusat Jauh." },
  { "en": "Apa Fungsi Wire EndTransmission Stop?", "id": "Kirim Stop Condition I2C." },
  { "en": "Apa Fungsi Gerber Viewer Di Proteus?", "id": "Melihat File Produksi PCB." },
  { "en": "Apa Fungsi First Cycle Flag (P_First)?", "id": "Aktif Hanya Saat Start Awal." },
  { "en": "Bagaimana Cara Insert Raster Image AutoCAD?", "id": "Ketik Imageattach Lalu Enter." },
  { "en": "Apa Fungsi Perintah Subtract Solid Region?", "id": "Potong Area Region Solid." },
  { "en": "Apa Fungsi Perintah Laylck Pada AutoCAD (Computer Aided Design)?", "id": "Mengunci Layer Objek Yang Dipilih." },
  { "en": "Apa Fungsi Perintah Laythw Pada AutoCAD?", "id": "Mencairkan Semua Layer Yang Beku." },
  { "en": "Shortcut Apa Untuk Sheet Set Manager AutoCAD?", "id": "Tekan Ctrl + 4." },
  { "en": "Apa Fungsi Perintah Dimarc Pada AutoCAD?", "id": "Mengukur Panjang Busur Lingkaran." },
  { "en": "Apa Fungsi Perintah Dimjogline Pada AutoCAD?", "id": "Menambah Garis Jog Pada Dimensi." },
  { "en": "Apa Fungsi Jumper Wire Pada PCB Proteus?", "id": "Penghubung Jalur Di Atas Komponen." },
  { "en": "Apa Fungsi Instruksi BING (Binary To Gray)?", "id": "Konversi Biner Ke Kode Gray." },
  { "en": "Apa Fungsi Instruksi GRAY (Gray To Binary)?", "id": "Konversi Kode Gray Ke Biner." },
  { "en": "Komponen TL072 Di Proteus Berfungsi Sebagai?", "id": "Dual Low Noise Op Amp." },
  { "en": "Apa Fungsi Perintah Ncopy Pada AutoCAD?", "id": "Menyalin Objek Dari Dalam Blok." },
  { "en": "Apa Fungsi String Concat Pada Arduino?", "id": "Menggabungkan Dua String Menjadi Satu." },
  { "en": "Bagaimana Cara Mengatur Dragmode AutoCAD?", "id": "Ubah Variabel Dragmode." },
  { "en": "Apa Fungsi Tombol Ctrl Klik Objek AutoCAD?", "id": "Siklus Seleksi Objek Bertumpuk." },
  { "en": "Apa Fungsi Perintah Etransmit Pada AutoCAD?", "id": "Paket File Gambar Dan Referensi." },
  { "en": "Komponen IC 4518 Dual BCD Counter?", "id": "Dua Penghitung BCD Sinkron." },
  { "en": "Apa Fungsi Instruksi FLT L (Double Floating)?", "id": "Konversi 32 Bit Ke Desimal." },
  { "en": "Apa Fungsi Instruksi FIX (Floating To Integer)?", "id": "Konversi Desimal Ke Bilangan Bulat." },
  { "en": "Bagaimana Cara Mengatur Snap Angle AutoCAD?", "id": "Ubah Variabel Snapang." },
  { "en": "Apa Fungsi Interrupt Mode High Arduino?", "id": "Picu Terus Saat Logika Tinggi." },
  { "en": "Apa Itu Data Memory File (E) PLC?", "id": "Memori Data Bank Eksternal." },
  { "en": "Apa Itu Link Relay (Link Bit) PLC?", "id": "Bit Berbagi Antar Unit PLC." },
  { "en": "Shortcut Apa Untuk Previous Tab Arduino?", "id": "Tekan Ctrl + Alt + Left." },
  { "en": "Apa Fungsi Library SPI EndTransaction Arduino?", "id": "Mengakhiri Sesi Transaksi SPI." },
  { "en": "Bagaimana Cara Mengubah Warna Jalur Proteus?", "id": "Edit Wire Style Colour." },
  { "en": "Apa Fungsi Instruksi PUSH Pada Stack PLC?", "id": "Menyimpan Data Ke Tumpukan." },
  { "en": "Apa Fungsi Instruksi FIFO (First In First Out)?", "id": "Antrian Data Masuk Keluar." },
  { "en": "Apa Fungsi Perintah Dimoverride Pada AutoCAD?", "id": "Menimpa Variabel Sistem Dimensi." },
  { "en": "Apa Itu Isochronous Communication Pada Serial?", "id": "Komunikasi Data Waktu Nyata." },
  { "en": "Apa Itu Function Pointer Pada Arduino?", "id": "Variabel Menyimpan Alamat Fungsi." },
  { "en": "Shortcut Apa Untuk Insert Line GX Works?", "id": "Tekan Ctrl + Insert." },
  { "en": "Apa Fungsi IsCntrl Pada Karakter Arduino?", "id": "Cek Karakter Kontrol ASCII." },
  { "en": "Apa Fungsi Interactive Analysis Di Proteus?", "id": "Simulasi Langsung Dengan Interaksi User." },
  { "en": "Apa Fungsi Timer Flasher Pada PLC?", "id": "Timer Kedip Nyala Mati." },
  { "en": "Bagaimana Cara Rename Block AutoCAD?", "id": "Gunakan Perintah Rename." },
  { "en": "Apa Fungsi Operator Bitwise And (Dan Tunggal)?", "id": "Logika AND Per Bit." },
  { "en": "Apa Fungsi Relay SPST 6 Pin Proteus?", "id": "Saklar Satu Kutub Kaki Enam." },
  { "en": "Shortcut Apa Untuk DBConnect AutoCAD?", "id": "Tekan Ctrl + 6." },
  { "en": "Apa Fungsi Serial ReadBytesUntil Pada Arduino?", "id": "Baca Byte Sampai Karakter Batas." },
  { "en": "Komponen Apa Yang Menghubungkan Arus Basis 2?", "id": "Resistor Pembagi Tegangan Basis." },
  { "en": "Apa Fungsi Instruksi LIFO (Last In First Out)?", "id": "Tumpukan Data Terakhir Keluar Awal." },
  { "en": "Bagaimana Cara Xref Detach AutoCAD?", "id": "Lepaskan Tautan Referensi Eksternal." },
  { "en": "Apa Itu EEPROM Read Int Arduino?", "id": "Membaca Dua Byte Integer." },
  { "en": "Apa Fungsi Transfer Graph Di Proteus?", "id": "Analisis Karakteristik Transfer DC." },
  { "en": "Apa Shortcut Find Contact GX Works?", "id": "Tekan Ctrl + F." },
  { "en": "Apa Fungsi Perintah Trace Width AutoCAD?", "id": "Mengatur Ketebalan Garis Trace." },
  { "en": "Apa Fungsi Operator Assignment (Sama Dengan)?", "id": "Mengisi Nilai Ke Variabel." },
  { "en": "Apa Itu Tape Recorder Di Proteus?", "id": "Perekam Dan Pemutar Sinyal Audio." },
  { "en": "Bagaimana Cara Export Ke Format SVG AutoCAD?", "id": "Gunakan Perintah Export SVG." },
  { "en": "Apa Fungsi Perintah Solidedit Face Rotate?", "id": "Memutar Permukaan Solid Pada Sumbu." },
  { "en": "Apa Tipe Data Unsigned Short Arduino?", "id": "Integer 16 Bit Positif." },
  { "en": "Komponen Apa Pemicu JFET Melalui Gate?", "id": "Tegangan Bias Mundur Gate." },
  { "en": "Apa Fungsi Instruksi ABS L (Double Absolute)?", "id": "Mutlakkan Nilai 32 Bit." },
  { "en": "Shortcut Apa Untuk Markup Set Manager AutoCAD?", "id": "Tekan Ctrl + 7." },
  { "en": "Apa Fungsi Break Dalam Switch Case?", "id": "Keluar Dari Logika Switch." },
  { "en": "Apa Itu Resolusi DAC 16 Bit?", "id": "Enam Puluh Lima Ribu Tingkat." },
  { "en": "Apa Fungsi Instruksi STC (Set Carry) PLC?", "id": "Menyalakan Flag Carry." },
  { "en": "Bagaimana Cara Copy Nested Objects AutoCAD?", "id": "Gunakan Perintah Ncopy." },
  { "en": "Apa Fungsi Define Macro Pada Arduino?", "id": "Membuat Alias Kode Program." },
  { "en": "Apa Itu Sensor Flame Detector Proteus?", "id": "Sensor Deteksi Api Inframerah." },
  { "en": "Apa Shortcut PLC Verification GX Works?", "id": "Menu Project Verify." },
  { "en": "Apa Fungsi Perintah Chamfer Distance AutoCAD?", "id": "Atur Jarak Potong Sudut." },
  { "en": "Apa Fungsi Analog ReadResolution 8 Bit?", "id": "Baca Data Dua Ratus Lima Lima." },
  { "en": "Komponen Apa Yang Menghubungkan Arus Gate 2?", "id": "Resistor Pull Down Gate." },
  { "en": "Apa Fungsi Negative Flag (N) PLC?", "id": "Menandai Hasil Bernilai Negatif." },
  { "en": "Shortcut Apa Untuk Next Tab Arduino?", "id": "Tekan Ctrl + Alt + Right." },
  { "en": "Apa Fungsi Serial Println Format BINARY?", "id": "Cetak Angka Biner." },
  { "en": "Apa Itu Pulse Width Generator Proteus?", "id": "Pembangkit Sinyal PWM Manual." },
  { "en": "Apa Fungsi Instruksi XNOR LD PLC?", "id": "Seri Blok Logika XNOR." },
  { "en": "Bagaimana Cara Create Spotlight AutoCAD?", "id": "Ketik Spotlight Lalu Enter." },
  { "en": "Apa Fungsi IsBlank Pada Karakter Arduino?", "id": "Cek Karakter Spasi Atau Tab." },
  { "en": "Apa Itu I2C Debugger Monitor Proteus?", "id": "Alat Pantau Trafik I2C." },
  { "en": "Shortcut Apa Untuk Zoom Object AutoCAD?", "id": "Ketik Z Lalu O." },
  { "en": "Apa Fungsi Perintah Union 3D AutoCAD?", "id": "Gabungkan Volume Solid 3D." },
  { "en": "Apa Fungsi String StartsWith Pada Arduino?", "id": "Cek Awalan Teks String." },
  { "en": "Apa Itu IC 7400 Quad Nand Gate?", "id": "Empat Gerbang NAND Standar." },
  { "en": "Apa Fungsi Interrupt Control (INT) PLC?", "id": "Mengontrol Masker Interupsi PLC." },
  { "en": "Bagaimana Cara Mengukur Volume Massprop AutoCAD?", "id": "Hitung Properti Massa Solid." },
  { "en": "Apa Fungsi Pin XTAL Pada ATMega?", "id": "Osilator Kristal Eksternal." },
  { "en": "Apa Itu Chassis Ground Di Proteus?", "id": "Ground Terhubung Ke Casing." },
  { "en": "Shortcut Apa Untuk Clean Screen AutoCAD?", "id": "Tekan Ctrl + 0." },
  { "en": "Apa Fungsi Perintah Helix Turns?", "id": "Atur Jumlah Putaran Spiral." },
  { "en": "Apa Fungsi Tanda Backslash V Arduino?", "id": "Karakter Vertical Tab." },
  { "en": "Apa Itu Regulator LDO (Low Dropout)?", "id": "Stabilizer Selisih Tegangan Rendah." },
  { "en": "Apa Fungsi Analog WriteResolution 8 Bit?", "id": "Tulis PWM Standar Arduino." },
  { "en": "Bagaimana Cara Mengatur Point Display Relative?", "id": "Ubah Variabel Pdsize Negatif." },
  { "en": "Apa Fungsi Pin SS Pada Arduino?", "id": "Slave Select SPI." },
  { "en": "Apa Itu Signal Generator AM Proteus?", "id": "Pembangkit Sinyal Modulasi Amplitudo." },
  { "en": "Apa Shortcut Go To Definition Arduino?", "id": "Klik Kanan Go To Definition." },
  { "en": "Apa Fungsi Perintah Array Edit AutoCAD?", "id": "Mengedit Objek Asosiatif Array." },
  { "en": "Apa Fungsi Operator Bitwise XOR (Topi)?", "id": "Logika Exclusive OR Bit." },
  { "en": "Apa Fungsi Pin SCL Pada Arduino Mega?", "id": "Clock Komunikasi Serial I2C." },
  { "en": "Apa Itu DIP Switch 8 Way Proteus?", "id": "Saklar Geser Delapan Jalur." },
  { "en": "Bagaimana Cara Update Table AutoCAD?", "id": "Klik Kanan Update Table Data Links." },
  { "en": "Apa Fungsi Perintah Dimtiledit Pada AutoCAD?", "id": "Mengatur Posisi Teks Dimensi." },
  { "en": "Apa Fungsi Wire Write String Arduino?", "id": "Kirim Teks Lewat I2C." },
  { "en": "Apa Fungsi Layer Stack Manager Proteus?", "id": "Mengatur Lapisan PCB Multi Layer." },
  { "en": "Apa Fungsi Cycle Time (P_Cycle)?", "id": "Waktu Scan Siklus PLC." },
  { "en": "Bagaimana Cara Insert Field AutoCAD?", "id": "Ketik Field Lalu Enter." },
  { "en": "Apa Fungsi Perintah Interfere Check AutoCAD?", "id": "Cek Tabrakan Antar Solid." },
  { "en": "Apa Fungsi Perintah Mspace Pada Layout AutoCAD?", "id": "Pindah Ke Model Space Di Layout." },
  { "en": "Apa Fungsi Perintah Pspace Pada Layout AutoCAD?", "id": "Pindah Ke Paper Space Di Layout." },
  { "en": "Shortcut Apa Untuk Visual Styles AutoCAD?", "id": "Ketik Visualstyles Lalu Enter." },
  { "en": "Apa Fungsi Perintah Solprof Pada AutoCAD 3D?", "id": "Membuat Profil 2D Dari Solid 3D." },
  { "en": "Apa Fungsi Perintah Solview Pada AutoCAD 3D?", "id": "Membuat Viewport Ortografis Otomatis." },
  { "en": "Apa Fungsi Marker Mode Di Proteus?", "id": "Menandai Titik Referensi Pada PCB." },
  { "en": "Apa Fungsi Instruksi FDIV (Floating Point Divide)?", "id": "Pembagian Bilangan Desimal PLC." },
  { "en": "Apa Fungsi Instruksi SIN (Sine) Pada PLC?", "id": "Menghitung Nilai Sinus Sudut." },
  { "en": "Komponen LM311 Di Proteus Berfungsi Sebagai?", "id": "Komparator Tegangan Kecepatan Tinggi." },
  { "en": "Apa Fungsi Perintah Soldraw Pada AutoCAD?", "id": "Menghasilkan Profil Dari Viewport Solview." },
  { "en": "Apa Fungsi String CompareTo Pada Arduino?", "id": "Membandingkan Dua String Secara Leksikografis." },
  { "en": "Bagaimana Cara Mengatur Aperture Display AutoCAD?", "id": "Aktifkan Tombol Aperture Di Opsi." },
  { "en": "Apa Fungsi Tombol Ctrl Shift C AutoCAD?", "id": "Menyalin Objek Dengan Titik Basis." },
  { "en": "Apa Fungsi Perintah Texttofront Pada AutoCAD?", "id": "Membawa Semua Teks Ke Depan." },
  { "en": "Komponen IC 4029 Up Down Counter?", "id": "Penghitung Naik Turun Biner Desimal." },
  { "en": "Apa Fungsi Instruksi COS (Cosine) Pada PLC?", "id": "Menghitung Nilai Cosinus Sudut." },
  { "en": "Apa Fungsi Instruksi TAN (Tangent) Pada PLC?", "id": "Menghitung Nilai Tangen Sudut." },
  { "en": "Bagaimana Cara Mengatur Polar Ang Pada AutoCAD?", "id": "Ubah Variabel Polarang." },
  { "en": "Apa Fungsi Interrupt Mode Change Pin?", "id": "Picu Saat Status Pin Berubah." },
  { "en": "Apa Itu Data Memory File Register (D) PLC?", "id": "Memori Penyimpanan Data File." },
  { "en": "Apa Itu Expansion Instruction (Ext) PLC?", "id": "Instruksi Tambahan Di Luar Standar." },
  { "en": "Shortcut Apa Untuk Previous Bookmark Arduino?", "id": "Tekan Alt + Left." },
  { "en": "Apa Fungsi Library SPI SetBitOrder Arduino?", "id": "Mengatur Urutan Bit LSB MSB." },
  { "en": "Bagaimana Cara Mengubah Grid Type Proteus?", "id": "Menu View Grid Dot Atau Lines." },
  { "en": "Apa Fungsi Instruksi ASIN (Arc Sine) PLC?", "id": "Menghitung Invers Sinus Sudut." },
  { "en": "Apa Fungsi Instruksi ACOS (Arc Cosine) PLC?", "id": "Menghitung Invers Cosinus Sudut." },
  { "en": "Apa Fungsi Perintah Hatchtoback Pada AutoCAD?", "id": "Mengirim Arsiran Ke Belakang Objek." },
  { "en": "Apa Itu Master Slave Communication Serial?", "id": "Satu Pengendali Banyak Pengikut." },
  { "en": "Apa Itu Memory Leak Pada Arduino?", "id": "Memori RAM Terpakai Tidak Kembali." },
  { "en": "Shortcut Apa Untuk Replace String GX Works?", "id": "Tekan Ctrl + H." },
  { "en": "Apa Fungsi LiquidCrystal CreateChar Pada Arduino?", "id": "Membuat Karakter Kustom Di LCD." },
  { "en": "Apa Fungsi Frequency Graph Di Proteus?", "id": "Merespon Frekuensi Terhadap Gain." },
  { "en": "Apa Fungsi Timer One Shot (T) PLC?", "id": "Output Aktif Sesaat Saat Trigger." },
  { "en": "Bagaimana Cara Membuat Dynamic Parameter AutoCAD?", "id": "Gunakan Palet Block Authoring." },
  { "en": "Apa Fungsi Operator Bitwise Shift Left (<<)?", "id": "Geser Bit Ke Kiri." },
  { "en": "Apa Itu Component Label Di Proteus?", "id": "Teks Identitas Nama Komponen." },
  { "en": "Shortcut Apa Untuk Layer States AutoCAD?", "id": "Ketik Layerstate Lalu Enter." },
  { "en": "Apa Fungsi Serial ReadBytes Pada Arduino?", "id": "Membaca Byte Ke Dalam Buffer." },
  { "en": "Komponen Apa Yang Menghubungkan Arus Source FET?", "id": "Resistor Source Ke Ground." },
  { "en": "Apa Fungsi Instruksi ATAN (Arc Tangent) PLC?", "id": "Menghitung Invers Tangen Sudut." },
  { "en": "Bagaimana Cara Xref Bind Insert?", "id": "Gabungkan Xref Tanpa Nama Blok." },
  { "en": "Apa Itu PROGMEM Pada Arduino?", "id": "Menyimpan Data Di Flash Memory." },
  { "en": "Apa Fungsi DC Sweep Graph Di Proteus?", "id": "Analisis Tegangan DC Variabel." },
  { "en": "Apa Shortcut Convert All GX Works?", "id": "Tekan Shift + F4." },
  { "en": "Apa Fungsi Perintah Splinedit Pada AutoCAD?", "id": "Mengedit Kurva Spline." },
  { "en": "Apa Fungsi Operator Bitwise Shift Right (>>)?", "id": "Geser Bit Ke Kanan." },
  { "en": "Apa Itu Voltage Probe Differential Proteus?", "id": "Ukur Beda Tegangan Dua Titik." },
  { "en": "Bagaimana Cara Export Ke Format JPG AutoCAD?", "id": "Ketik Jpgout Lalu Enter." },
  { "en": "Apa Fungsi Perintah Solidedit Face Move?", "id": "Memindahkan Permukaan Solid 3D." },
  { "en": "Apa Tipe Data Unsigned Long Long Arduino?", "id": "Integer Positif 64 Bit." },
  { "en": "Komponen Apa Pemicu Triac Optocoupler?", "id": "Cahaya LED Internal." },
  { "en": "Apa Fungsi Instruksi SQRT L (Long Sqrt)?", "id": "Akar Kuadrat Data 32 Bit." },
  { "en": "Shortcut Apa Untuk Tool Properties AutoCAD?", "id": "Klik Kanan Properties Di Tool." },
  { "en": "Apa Fungsi Switch Case Without Break?", "id": "Jalankan Case Berikutnya (Fallthrough)." },
  { "en": "Apa Itu Resolusi PWM 16 Bit?", "id": "Enam Puluh Lima Ribu Tingkat." },
  { "en": "Apa Fungsi Instruksi CLC (Clear Carry) PLC?", "id": "Nonaktifkan Flag Carry." },
  { "en": "Bagaimana Cara Copy Properties AutoCAD?", "id": "Gunakan Perintah Matchprop." },
  { "en": "Apa Fungsi Volatile Bool Pada Arduino?", "id": "Boolean Berubah Di Interupsi." },
  { "en": "Apa Itu Sensor Magnetic Reed Proteus?", "id": "Saklar Magnetik Tabung Kaca." },
  { "en": "Apa Shortcut PLC Read From GX Works?", "id": "Menu Online Read From PLC." },
  { "en": "Apa Fungsi Perintah Chamfer Angle AutoCAD?", "id": "Potong Sudut Berdasarkan Derajat." },
  { "en": "Apa Fungsi LiquidCrystal ScrollDisplayLeft Arduino?", "id": "Geser Tampilan LCD Ke Kiri." },
  { "en": "Komponen Apa Yang Menghubungkan Arus Drain FET?", "id": "Resistor Drain Ke VCC." },
  { "en": "Apa Fungsi Error Flag (P_Error) PLC?", "id": "Menandai Terjadi Kesalahan Instruksi." },
  { "en": "Shortcut Apa Untuk Next Bookmark Arduino?", "id": "Tekan Alt + Right." },
  { "en": "Apa Fungsi Serial Println Format HEX?", "id": "Cetak Data Hexadesimal Baris Baru." },
  { "en": "Apa Itu Audio Generator Di Proteus?", "id": "Sumber Sinyal Audio Wav." },
  { "en": "Apa Fungsi Instruksi XNORW (Xnor Word) PLC?", "id": "Logika XNOR Tingkat Word." },
  { "en": "Bagaimana Cara Create Web Light AutoCAD?", "id": "Ketik Weblight Lalu Enter." },
  { "en": "Apa Fungsi String EndsWith Pada Arduino?", "id": "Cek Akhiran Teks String." },
  { "en": "Apa Itu 16 Segment Display Proteus?", "id": "Penampil Karakter Lebih Detail." },
  { "en": "Shortcut Apa Untuk Pan Realtime AutoCAD?", "id": "Ketik P Lalu Enter." },
  { "en": "Apa Fungsi Perintah Interfere Solid AutoCAD?", "id": "Buat Objek Dari Tabrakan Solid." },
  { "en": "Apa Fungsi LiquidCrystal ScrollDisplayRight Arduino?", "id": "Geser Tampilan LCD Ke Kanan." },
  { "en": "Apa Itu IC 7485 Magnitude Comparator?", "id": "Pembanding Nilai Dua Data Biner." },
  { "en": "Apa Fungsi Instruksi EI (Enable Interrupt) PLC?", "id": "Mengizinkan Interupsi CPU." },
  { "en": "Bagaimana Cara Mengukur Luas Hatch AutoCAD?", "id": "Pilih Hatch Lihat Properties Area." },
  { "en": "Apa Fungsi Pin AREF Pada Arduino Nano?", "id": "Referensi Tegangan ADC Eksternal." },
  { "en": "Apa Itu Digital Ground Di Proteus?", "id": "Ground Logika Digital." },
  { "en": "Shortcut Apa Untuk Status Bar AutoCAD?", "id": "Klik Kanan Status Bar." },
  { "en": "Apa Fungsi Perintah Helix Twist?", "id": "Atur Arah Putaran Spiral." },
  { "en": "Apa Fungsi Tanda Backslash Single Quote?", "id": "Karakter Petik Satu Dalam String." },
  { "en": "Apa Itu Voltage Regulator Switching?", "id": "Penstabil Tegangan Efisiensi Tinggi." },
  { "en": "Apa Fungsi LiquidCrystal Autoscroll Arduino?", "id": "Geser Teks Otomatis Saat Tulis." },
  { "en": "Bagaimana Cara Mengatur Point Display Absolute?", "id": "Ubah Variabel Pdsize Positif." },
  { "en": "Apa Fungsi Pin SCK Pada Arduino?", "id": "Clock Serial Komunikasi SPI." },
  { "en": "Apa Itu Pick And Place File?", "id": "Data Koordinat Letak Komponen." },
  { "en": "Apa Shortcut Rename Arduino Tab?", "id": "Klik Segitiga Tab Rename." },
  { "en": "Apa Fungsi Perintah Rotate Reference AutoCAD?", "id": "Putar Berdasarkan Sudut Referensi." },
  { "en": "Apa Fungsi Operator Bitwise Not (Tilde)?", "id": "Membalik Semua Bit Variabel." },
  { "en": "Apa Fungsi Pin SDA Pada Arduino Uno?", "id": "Jalur Data I2C." },
  { "en": "Apa Itu Resistor Network SIL Proteus?", "id": "Kumpulan Resistor Satu Baris." },
  { "en": "Bagaimana Cara Update Field In Text?", "id": "Klik Kanan Field Update." },
  { "en": "Apa Fungsi Perintah Dimjogged Linear?", "id": "Dimensi Linear Dengan Garis Tekuk." },
  { "en": "Apa Fungsi Wire Write Byte Arduino?", "id": "Kirim Satu Byte I2C." },
  { "en": "Apa Fungsi Pre Production Check Proteus?", "id": "Cek Kelayakan Desain Sebelum Produksi." },
  { "en": "Apa Fungsi Battery Low Flag (P_LowBatt)?", "id": "Menandai Baterai PLC Lemah." },
  { "en": "Bagaimana Cara Insert OLE Object Link?", "id": "Centang Link Pada Insert Object." },
  { "en": "Apa Fungsi Perintah Subtract 3D Region?", "id": "Potong Area Region 3D." },
  { "en": "Apa Fungsi Perintah Laydel Pada AutoCAD (Computer Aided Design)?", "id": "Menghapus Layer Dan Semua Objeknya." },
  { "en": "Apa Fungsi Perintah Laymrg Pada AutoCAD?", "id": "Menggabungkan Layer Terpilih Ke Tujuan." },
  { "en": "Shortcut Apa Untuk Tool Palettes AutoCAD?", "id": "Tekan Ctrl + 3." },
  { "en": "Apa Fungsi Perintah Dimtiledit Pada Dimensi AutoCAD?", "id": "Memindahkan Teks Dimensi Sepanjang Garis." },
  { "en": "Apa Fungsi Perintah Dimoblique Pada AutoCAD?", "id": "Memiringkan Garis Ekstensi Dimensi." },
  { "en": "Apa Fungsi Design Explorer Di Proteus?", "id": "Navigasi Hierarki Dan Komponen Desain." },
  { "en": "Apa Fungsi Instruksi BCNT (Bit Counter) PLC?", "id": "Menghitung Jumlah Bit Bernilai Satu." },
  { "en": "Apa Fungsi Instruksi GRY (Gray Code Conversion)?", "id": "Konversi Biner Ke Kode Gray." },
  { "en": "Komponen LF353 Di Proteus Berfungsi Sebagai?", "id": "Dual Wide Bandwidth JFET Op Amp." },
  { "en": "Apa Fungsi Perintah Flatshot Pada AutoCAD?", "id": "Membuat Representasi 2D Dari 3D." },
  { "en": "Apa Fungsi String Replace Pada Arduino?", "id": "Mengganti Bagian Teks Dengan Lainnya." },
  { "en": "Bagaimana Cara Mengatur Highlight AutoCAD?", "id": "Ubah Variabel Selectioneffect." },
  { "en": "Apa Fungsi Tombol Ctrl Shift S AutoCAD?", "id": "Simpan Gambar Dengan Nama Baru." },
  { "en": "Apa Fungsi Perintah Xclip Pada AutoCAD?", "id": "Memotong Tampilan Blok Atau Xref." },
  { "en": "Komponen IC 74192 Up Down Counter?", "id": "Penghitung BCD Naik Turun Sinkron." },
  { "en": "Apa Fungsi Instruksi RAD (Radians) Pada PLC?", "id": "Konversi Derajat Ke Radian." },
  { "en": "Apa Fungsi Instruksi DEG (Degrees) Pada PLC?", "id": "Konversi Radian Ke Derajat." },
  { "en": "Bagaimana Cara Mengatur Zoom Factor AutoCAD?", "id": "Ubah Variabel Zoomfactor." },
  { "en": "Apa Fungsi Interrupt Mode Rising Edge?", "id": "Picu Saat Transisi Logika Naik." },
  { "en": "Apa Itu Data Memory Index (E) PLC?", "id": "Memori Data Bank Eksternal Indeks." },
  { "en": "Apa Itu Function Block (FB) PLC?", "id": "Blok Program Fungsi Berulang." },
  { "en": "Shortcut Apa Untuk Uncomment Code Arduino?", "id": "Tekan Ctrl + Slash." },
  { "en": "Apa Fungsi Library Wire RequestFrom Arduino?", "id": "Minta Sejumlah Byte Dari Slave." },
  { "en": "Bagaimana Cara Mengubah Ukuran Kaki Komponen Proteus?", "id": "Edit Packaging Tool." },
  { "en": "Apa Fungsi Instruksi SWAP L (Double Swap)?", "id": "Tukar Data 32 Bit." },
  { "en": "Apa Fungsi Instruksi XFER L (Double Block Transfer)?", "id": "Salin Blok Data 32 Bit." },
  { "en": "Apa Fungsi Perintah Dimreassociate Pada AutoCAD?", "id": "Mengaitkan Ulang Dimensi Yang Lepas." },
  { "en": "Apa Itu Loopback Test Pada Serial?", "id": "Tes Kirim Terima Data Sendiri." },
  { "en": "Apa Itu Segmentation Fault Pada Arduino?", "id": "Akses Memori Terlarang." },
  { "en": "Shortcut Apa Untuk Go To Line GX Works?", "id": "Tekan Ctrl + G." },
  { "en": "Apa Fungsi LiquidCrystal NoDisplay Pada Arduino?", "id": "Mematikan Tampilan Teks LCD." },
  { "en": "Apa Fungsi Intermodulation Graph Di Proteus?", "id": "Analisis Intermodulasi Sinyal RF." },
  { "en": "Apa Fungsi Timer Retentive (RTO) PLC?", "id": "Timer Simpan Waktu Saat Mati." },
  { "en": "Bagaimana Cara Membuat Block Property Table?", "id": "Gunakan Block Editor Properties Table." },
  { "en": "Apa Fungsi Operator Bitwise Or (Garis Tunggal)?", "id": "Logika OR Per Bit." },
  { "en": "Apa Itu Property Definition Di Proteus?", "id": "Mendefinisikan Properti Kustom Komponen." },
  { "en": "Shortcut Apa Untuk Match Cell AutoCAD?", "id": "Pilih Sel Tabel Match Cell." },
  { "en": "Apa Fungsi Serial Write Buf Len?", "id": "Kirim Buffer Data Panjang Tertentu." },
  { "en": "Komponen Apa Yang Menstabilkan Arus Bias?", "id": "Resistor Emitter Feedback." },
  { "en": "Apa Fungsi Instruksi MLPX (Multiplexer) PLC?", "id": "Memilih Satu Dari Banyak Input." },
  { "en": "Bagaimana Cara Xref Path Type Absolute?", "id": "Set Path Tipe Mutlak." },
  { "en": "Apa Itu EEPROM Update Int Arduino?", "id": "Tulis Integer Jika Berubah." },
  { "en": "Apa Fungsi S Parameter Graph Di Proteus?", "id": "Analisis Parameter Hamburan Frekuensi." },
  { "en": "Apa Shortcut Find Device Comment GX Works?", "id": "Tekan Ctrl + F." },
  { "en": "Apa Fungsi Perintah Spline Method AutoCAD?", "id": "Pilih Metode Fit Atau CV." },
  { "en": "Apa Fungsi Operator Bitwise Xor (Topi)?", "id": "Logika XOR Per Bit." },
  { "en": "Apa Itu Voltage Probe Current Proteus?", "id": "Ukur Arus Tanpa Memutus Jalur." },
  { "en": "Bagaimana Cara Export Ke Format TIF AutoCAD?", "id": "Ketik Tifout Lalu Enter." },
  { "en": "Apa Fungsi Perintah Solidedit Face Offset?", "id": "Geser Permukaan Solid Jarak Tertentu." },
  { "en": "Apa Tipe Data Unsigned Char Array?", "id": "Array Byte Data Mentah." },
  { "en": "Komponen Apa Pemicu Diac?", "id": "Tegangan Breakdown Tercapai." },
  { "en": "Apa Fungsi Instruksi DMPX (Demultiplexer) PLC?", "id": "Membagi Satu Input Ke Banyak." },
  { "en": "Shortcut Apa Untuk Design Feed AutoCAD?", "id": "Tekan Ctrl + 0." },
  { "en": "Apa Fungsi Continue Dalam Switch Case?", "id": "Tidak Berlaku Dalam Switch." },
  { "en": "Apa Itu Resolusi ADC 24 Bit?", "id": "Enam Belas Juta Tingkat." },
  { "en": "Apa Fungsi Instruksi CLC (Clear Carry) PLC?", "id": "Hapus Status Carry Flag." },
  { "en": "Bagaimana Cara Copy With Basepoint AutoCAD?", "id": "Tekan Ctrl + Shift + C." },
  { "en": "Apa Fungsi Volatile Long Pada Arduino?", "id": "Long Berubah Di Interupsi." },
  { "en": "Apa Itu Sensor Hall Effect Linear?", "id": "Output Analog Proporsional Magnet." },
  { "en": "Apa Shortcut PLC Verify GX Works?", "id": "Menu Project Verify." },
  { "en": "Apa Fungsi Perintah Chamfer Method AutoCAD?", "id": "Pilih Metode Jarak Atau Sudut." },
  { "en": "Apa Fungsi LiquidCrystal Display Pada Arduino?", "id": "Menyalakan Kembali Tampilan LCD." },
  { "en": "Komponen Apa Yang Menghubungkan Arus Basis PNP?", "id": "Resistor Pull Up Basis." },
  { "en": "Apa Fungsi Carry Flag (P_CY) PLC?", "id": "Status Sisa Operasi Aritmatika." },
  { "en": "Shortcut Apa Untuk Show Error Arduino?", "id": "Klik Panel Output Error." },
  { "en": "Apa Fungsi Serial Println Format BIN?", "id": "Cetak Data Biner Baris Baru." },
  { "en": "Apa Itu Bus Generator Di Proteus?", "id": "Pembangkit Sinyal Bus Digital." },
  { "en": "Apa Fungsi Instruksi XNR W (Xnor Word)?", "id": "Logika XNOR Data Word." },
  { "en": "Bagaimana Cara Create Distant Light AutoCAD?", "id": "Ketik Distantlight Lalu Enter." },
  { "en": "Apa Fungsi IsSpace Pada Karakter Arduino?", "id": "Cek Spasi Tab Baris Baru." },
  { "en": "Apa Itu Serial Terminal Proteus?", "id": "Simulasi Port Serial RS232." },
  { "en": "Shortcut Apa Untuk Zoom Previous AutoCAD?", "id": "Ketik Z Lalu P." },
  { "en": "Apa Fungsi Perintah Imprint Solid AutoCAD?", "id": "Cetak Geometri Ke Permukaan Solid." },
  { "en": "Apa Fungsi String ToCharArray Buffer?", "id": "Salin String Ke Buffer Char." },
  { "en": "Apa Itu IC 74151 Multiplexer?", "id": "Pemilih Data 8 Ke 1." },
  { "en": "Apa Fungsi Interrupt Mask (MSKS) PLC?", "id": "Mengatur Masker Interupsi I/O." },
  { "en": "Bagaimana Cara Mengukur Jarak Dist AutoCAD?", "id": "Gunakan Perintah Dist." },
  { "en": "Apa Fungsi Pin VCC Pada ATMega328?", "id": "Supply Tegangan Positif Digital." },
  { "en": "Apa Itu Analog Ground Di Proteus?", "id": "Ground Sinyal Analog." },
  { "en": "Shortcut Apa Untuk Info Center AutoCAD?", "id": "Tekan Ctrl + 5." },
  { "en": "Apa Fungsi Perintah Helix Height?", "id": "Atur Tinggi Total Spiral." },
  { "en": "Apa Fungsi Tanda Backslash Double Quote?", "id": "Karakter Petik Dua Dalam String." },
  { "en": "Apa Itu Voltage Regulator Adjustable?", "id": "Penstabil Tegangan Bisa Diatur." },
  { "en": "Apa Fungsi Analog WriteFrequency Pin?", "id": "Ubah Frekuensi PWM Pin Tertentu." },
  { "en": "Bagaimana Cara Mengatur Point Display Mode?", "id": "Ubah Variabel Pdmode." },
  { "en": "Apa Fungsi Pin MISO Pada Arduino Mega?", "id": "Data Masuk Master SPI." },
  { "en": "Apa Itu Zone Mode Keepout Proteus?", "id": "Area Terlarang Untuk Jalur." },
  { "en": "Apa Shortcut Close Tab Arduino?", "id": "Tekan Ctrl + W." },
  { "en": "Apa Fungsi Perintah Scale Reference AutoCAD?", "id": "Skala Berdasarkan Panjang Referensi." },
  { "en": "Apa Fungsi Operator Bitwise Shift Assignment?", "id": "Geser Dan Simpan Hasil." },
  { "en": "Apa Fungsi Pin MOSI Pada Arduino Mega?", "id": "Data Keluar Master SPI." },
  { "en": "Apa Itu DIP Switch 4 Way Proteus?", "id": "Saklar Geser Empat Jalur." },
  { "en": "Bagaimana Cara Update Block Icon AutoCAD?", "id": "Ketik Blockicon Lalu Enter." },
  { "en": "Apa Fungsi Perintah Dimbaseline Spacing?", "id": "Atur Jarak Spasi Dimensi Baseline." },
  { "en": "Apa Fungsi Wire Available Pada Arduino?", "id": "Cek Jumlah Byte I2C Masuk." },
  { "en": "Apa Fungsi Power Plane Generator Proteus?", "id": "Membuat Bidang Tembaga Otomatis." },
  { "en": "Apa Fungsi Step Flag (P_Step)?", "id": "Status Eksekusi Langkah Step." },
  { "en": "Bagaimana Cara Insert OLE Object Embed?", "id": "Pilih Create New Pada Insert." },
  { "en": "Apa Fungsi Perintah Subtract Solid Composite?", "id": "Kurangi Volume Solid Gabungan." },
  { "en": "Apa Fungsi Perintah Laywalk Pada AutoCAD (Computer Aided Design)?", "id": "Meninjau Objek Pada Setiap Layer." },
  { "en": "Apa Fungsi Perintah Layvpi Pada AutoCAD?", "id": "Membekukan Layer Pada Viewport Terpilih." },
  { "en": "Shortcut Apa Untuk Insert Hyperlink AutoCAD?", "id": "Tekan Ctrl + K." },
  { "en": "Apa Fungsi Perintah Dimjogline Pada Dimensi?", "id": "Menambahkan Garis Tekuk Dimensi Linear." },
  { "en": "Apa Fungsi Perintah Diminspection Pada AutoCAD?", "id": "Membuat Label Inspeksi Dimensi." },
  { "en": "Apa Fungsi Package Mode Pada ARES Proteus?", "id": "Memilih Kemasan Komponen PCB." },
  { "en": "Apa Fungsi Instruksi FVAL (Floating Point Value)?", "id": "Konversi String Ke Bilangan Desimal." },
  { "en": "Apa Fungsi Instruksi STR (String) Pada PLC?", "id": "Konversi Data Hex Ke String." },
  { "en": "Komponen LM337 Di Proteus Berfungsi Sebagai?", "id": "Regulator Tegangan Negatif Variabel." },
  { "en": "Apa Fungsi Perintah Movebak Pada AutoCAD Express?", "id": "Memindahkan File Bak Ke Folder." },
  { "en": "Apa Fungsi String Remove Pada Arduino?", "id": "Menghapus Karakter Dari String." },
  { "en": "Bagaimana Cara Mengatur HLR Settings AutoCAD?", "id": "Ubah Variabel Hlrsettings." },
  { "en": "Apa Fungsi Tombol Ctrl Shift H AutoCAD?", "id": "Menyembunyikan Palet Yang Terbuka." },
  { "en": "Apa Fungsi Perintah Overkill Pada AutoCAD?", "id": "Menghapus Objek Geometri Ganda." },
  { "en": "Komponen IC 7442 BCD To Decimal Decoder?", "id": "Dekoder BCD Ke Desimal." },
  { "en": "Apa Fungsi Instruksi SUM (Summation) Pada PLC?", "id": "Menjumlahkan Data Dalam Range." },
  { "en": "Apa Fungsi Instruksi AVE (Average) Pada PLC?", "id": "Menghitung Rata Rata Data Range." },
  { "en": "Bagaimana Cara Mengatur Polar Dist Pada AutoCAD?", "id": "Ubah Variabel Polardist." },
  { "en": "Apa Fungsi Interrupt Mode Falling Edge?", "id": "Picu Saat Transisi Logika Turun." },
  { "en": "Apa Itu Data Memory Cascade (E) PLC?", "id": "Memori Data Bank Sambungan." },
  { "en": "Apa Itu Step Relay (S) Pada PLC?", "id": "Relay Kontrol Urutan Langkah." },
  { "en": "Shortcut Apa Untuk Previous Window Arduino?", "id": "Tekan Ctrl + Shift + Tab." },
  { "en": "Apa Fungsi Library SPI UsingInterrupt Arduino?", "id": "Mengamankan Transaksi SPI Dari Interupsi." },
  { "en": "Bagaimana Cara Mengubah Track Width Proteus?", "id": "Edit Trace Style Width." },
  { "en": "Apa Fungsi Instruksi PEEK Pada PLC?", "id": "Membaca Data Dari Alamat Memori." },
  { "en": "Apa Fungsi Instruksi POKE Pada PLC?", "id": "Menulis Data Ke Alamat Memori." },
  { "en": "Apa Fungsi Perintah Dimdisassociate Pada AutoCAD?", "id": "Melepas Asosiasi Dimensi Dari Objek." },
  { "en": "Apa Itu Loop Interface Pada Serial?", "id": "Antarmuka Arus Putaran Digital." },
  { "en": "Apa Itu Stack Overflow Pada Arduino?", "id": "Memori Tumpukan Melebihi Batas." },
  { "en": "Shortcut Apa Untuk Jump To Bookmark GX Works?", "id": "Tekan Ctrl + G." },
  { "en": "Apa Fungsi LiquidCrystal Blink Pada Arduino?", "id": "Membuat Kursor LCD Berkedip." },
  { "en": "Apa Fungsi Conformance Analysis Di Proteus?", "id": "Analisis Kepatuhan Desain Standar." },
  { "en": "Apa Fungsi Timer Accumulative (RTO) PLC?", "id": "Menghitung Total Waktu Akumulasi." },
  { "en": "Bagaimana Cara Membuat Block Placeholder AutoCAD?", "id": "Gunakan Field BlockPlaceholder." },
  { "en": "Apa Fungsi Operator Bitwise Shift Right Assignment?", "id": "Geser Kanan Dan Simpan." },
  { "en": "Apa Itu Net Label Di Proteus?", "id": "Memberi Nama Jalur Sambungan." },
  { "en": "Shortcut Apa Untuk Visual LISP Editor AutoCAD?", "id": "Ketik Vlide Lalu Enter." },
  { "en": "Apa Fungsi Serial FlashStringHelper Pada Arduino?", "id": "Menyimpan String Di Flash Memory." },
  { "en": "Komponen Apa Yang Menstabilkan Tegangan Bias?", "id": "Dioda Bias Transistor." },
  { "en": "Apa Fungsi Instruksi SEGL (7 Segment Decoder)?", "id": "Konversi Hex Ke 7 Segment." },
  { "en": "Bagaimana Cara Xref Path Type Relative?", "id": "Set Path Tipe Relatif." },
  { "en": "Apa Itu EEPROM Length Arduino?", "id": "Mendapatkan Kapasitas Total EEPROM." },
  { "en": "Apa Fungsi Eyediagram Graph Di Proteus?", "id": "Analisis Kualitas Sinyal Digital." },
  { "en": "Apa Shortcut Replace Text GX Works?", "id": "Tekan Ctrl + H." },
  { "en": "Apa Fungsi Perintah Join Polyline AutoCAD?", "id": "Menyambung Garis Menjadi Polyline." },
  { "en": "Apa Fungsi Operator Bitwise Shift Left Assignment?", "id": "Geser Kiri Dan Simpan." },
  { "en": "Apa Itu Voltage Probe Marker Proteus?", "id": "Penanda Referensi Tegangan Grafik." },
  { "en": "Bagaimana Cara Export Ke Format PLT AutoCAD?", "id": "Gunakan Plotter HPGL." },
  { "en": "Apa Fungsi Perintah Solidedit Face Delete?", "id": "Menghapus Permukaan Solid 3D." },
  { "en": "Apa Tipe Data Unsigned Long Array?", "id": "Array Integer 32 Bit Positif." },
  { "en": "Komponen Apa Pemicu Triac Zero Crossing?", "id": "Optoisolator Zero Crossing." },
  { "en": "Apa Fungsi Instruksi PWR (Power) Pada PLC?", "id": "Menghitung Pangkat Nilai X Y." },
  { "en": "Shortcut Apa Untuk Layer Translator AutoCAD?", "id": "Ketik Laytrans Lalu Enter." },
  { "en": "Apa Fungsi Goto Dalam Switch Case?", "id": "Lompat Keluar Dari Switch." },
  { "en": "Apa Itu Resolusi DAC 10 Bit?", "id": "Seribu Dua Puluh Empat." },
  { "en": "Apa Fungsi Instruksi STC (Set Carry) PLC?", "id": "Set Status Carry Flag." },
  { "en": "Bagaimana Cara Copy Nested Block AutoCAD?", "id": "Gunakan Perintah Ncopy." },
  { "en": "Apa Fungsi Volatile Float Pada Arduino?", "id": "Float Berubah Di Interupsi." },
  { "en": "Apa Itu Sensor Load Cell Proteus?", "id": "Sensor Pengukur Berat Tekanan." },
  { "en": "Apa Shortcut PLC Upload GX Works?", "id": "Menu Online Read From PLC." },
  { "en": "Apa Fungsi Perintah Chamfer Trim AutoCAD?", "id": "Potong Garis Sisa Chamfer." },
  { "en": "Apa Fungsi LiquidCrystal NoBlink Pada Arduino?", "id": "Matikan Kedipan Kursor LCD." },
  { "en": "Komponen Apa Yang Menghubungkan Arus Emitter PNP?", "id": "Resistor Emitter Ke VCC." },
  { "en": "Apa Fungsi Diagnostic Flag (P_Diag)?", "id": "Status Diagnostik Sistem PLC." },
  { "en": "Shortcut Apa Untuk Compile Sketch Arduino?", "id": "Tekan Ctrl + R." },
  { "en": "Apa Fungsi Serial Println Format OCTAL?", "id": "Cetak Data Oktal Baris Baru." },
  { "en": "Apa Itu Clock Generator Di Proteus?", "id": "Pembangkit Detak Digital Tetap." },
  { "en": "Apa Fungsi Instruksi XNR L (Xnor Double)?", "id": "Logika XNOR Data 32 Bit." },
  { "en": "Bagaimana Cara Create Free Light AutoCAD?", "id": "Ketik Freelight Lalu Enter." },
  { "en": "Apa Fungsi IsUpper Pada Karakter Arduino?", "id": "Cek Apakah Huruf Besar." },
  { "en": "Apa Itu I2C Debugger Spy Proteus?", "id": "Intip Data Transaksi I2C." },
  { "en": "Shortcut Apa Untuk Zoom Window AutoCAD?", "id": "Ketik Z Lalu W." },
  { "en": "Apa Fungsi Perintah Thicken Surface AutoCAD?", "id": "Memberi Ketebalan Pada Surface." },
  { "en": "Apa Fungsi String ToInt Base 16?", "id": "Konversi String Hex Ke Integer." },
  { "en": "Apa Itu IC 74153 Multiplexer Dual?", "id": "Dua Pemilih Data 4 Ke 1." },
  { "en": "Apa Fungsi High Speed Counter (HSC) PLC?", "id": "Penghitung Pulsa Kecepatan Tinggi." },
  { "en": "Bagaimana Cara Mengukur Sudut 3 Point?", "id": "Gunakan Perintah Dimangular." },
  { "en": "Apa Fungsi Pin GND Pada ISP Header?", "id": "Ground Referensi Programmer." },
  { "en": "Apa Itu Power Ground Di Proteus?", "id": "Ground Arus Besar." },
  { "en": "Shortcut Apa Untuk Markup Import AutoCAD?", "id": "Ketik Markupimport Lalu Enter." },
  { "en": "Apa Fungsi Perintah Helix Base Position?", "id": "Tentukan Titik Pusat Spiral." },
  { "en": "Apa Fungsi Tanda Backslash Question Mark?", "id": "Karakter Tanda Tanya String." },
  { "en": "Apa Itu Voltage Regulator Fixed?", "id": "Penstabil Tegangan Nilai Tetap." },
  { "en": "Apa Fungsi Analog WriteFrequency All Pins?", "id": "Ubah Frekuensi PWM Global." },
  { "en": "Bagaimana Cara Mengatur Point Display Size Rel?", "id": "Ubah Variabel Pdsize Persen." },
  { "en": "Apa Fungsi Pin RESET Pada Arduino Mega?", "id": "Reset Mikrokontroler Atmega2560." },
  { "en": "Apa Itu Keepout Mode Di Proteus?", "id": "Area Larangan Penempatan Komponen." },
  { "en": "Apa Shortcut Close Sketch Arduino?", "id": "Tekan Ctrl + W." },
  { "en": "Apa Fungsi Perintah Scale Copy AutoCAD?", "id": "Skala Sambil Menyalin Objek." },
  { "en": "Apa Fungsi Operator Bitwise Not Assignment?", "id": "Inversi Bit Dan Simpan." },
  { "en": "Apa Fungsi Pin SCL Pada Arduino Uno?", "id": "Jalur Clock I2C." },
  { "en": "Apa Itu Resistor Network DIL Proteus?", "id": "Kumpulan Resistor Dual Inline." },
  { "en": "Bagaimana Cara Update Thumbnail Preview?", "id": "Ketik Updatethumbs Lalu Enter." },
  { "en": "Apa Fungsi Perintah Dimjogged Angle?", "id": "Dimensi Sudut Pusat Jauh." },
  { "en": "Apa Fungsi Wire Write Array Arduino?", "id": "Kirim Array Byte I2C." },
  { "en": "Apa Fungsi Production File Generator Proteus?", "id": "Membuat File Gerber Drill ODB." },
  { "en": "Apa Fungsi Scan Time (P_Scan)?", "id": "Waktu Siklus Scan Program." },
  { "en": "Bagaimana Cara Insert OLE Object Link?", "id": "Pilih Link Pada Insert Object." },
  { "en": "Apa Fungsi Perintah Subtract Solid Interfere?", "id": "Potong Volume Hasil Tabrakan." },
  { "en": "Bagaimana Cara Membuat Masking Teks Di AutoCAD (Computer Aided Design)?", "id": "Gunakan Perintah Textmask." },
  { "en": "Apa Efek Perintah Overkill Pada Gambar AutoCAD?", "id": "Menghapus Garis Tumpang Tindih." },
  { "en": "Shortcut Apa Untuk Membuka Design Center AutoCAD?", "id": "Tekan Ctrl + 2." },
  { "en": "Apa Fungsi Variabel Sistem Pickbox Pada AutoCAD?", "id": "Mengatur Ukuran Kotak Seleksi." },
  { "en": "Apa Fungsi Variabel Sistem Cursorsize Pada AutoCAD?", "id": "Mengatur Ukuran Crosshair Kursor." },
  { "en": "Bagaimana Cara Simulasi Kerusakan Komponen Di Proteus?", "id": "Edit Properties Fault Type." },
  { "en": "Apa Fungsi Instruksi NEG (Negation) Pada PLC (Programmable Logic)?", "id": "Mengubah Nilai Menjadi Negatif." },
  { "en": "Apa Fungsi Instruksi ZORE (Zone Reset) Pada PLC?", "id": "Mereset Area Bit Tertentu." },
  { "en": "Komponen Optocoupler 4N25 Di Proteus Berfungsi Sebagai?", "id": "Pemisah Sinyal Optik Elektrik." },
  { "en": "Apa Fungsi Perintah Wipeout Pada Objek AutoCAD?", "id": "Menutupi Area Dengan Latar Belakang." },
  { "en": "Apa Kegunaan Keyword Static Pada Variabel Arduino?", "id": "Mempertahankan Nilai Antar Pemanggilan." },
  { "en": "Bagaimana Cara Mengatur Autosave Interval Di AutoCAD?", "id": "Ubah Variabel Savetime." },
  { "en": "Apa Fungsi Tombol Ctrl Page Up Di AutoCAD?", "id": "Pindah Ke Tab Layout Sebelumnya." },
  { "en": "Apa Fungsi Perintah Xplode Pada Blok AutoCAD?", "id": "Memecah Blok Tetap Pertahankan Atribut." },
  { "en": "Komponen IC 74HC595 Shift Register Adalah?", "id": "Penggeser Bit Serial Ke Paralel." },
  { "en": "Apa Fungsi Instruksi SWAP (Swap Bytes) Pada PLC?", "id": "Menukar Posisi Byte Tinggi Rendah." },
  { "en": "Apa Fungsi Instruksi MOVB (Move Bit) Pada PLC?", "id": "Memindahkan Status Satu Bit." },
  { "en": "Bagaimana Cara Mengatur Zoom Speed Di AutoCAD?", "id": "Ubah Variabel Zoomfactor." },
  { "en": "Apa Mode Pin Input Pullup Pada Arduino?", "id": "Input Dengan Resistor Internal High." },
  { "en": "Apa Itu Data Memory Holding (H) Pada PLC?", "id": "Memori Data Tahan Mati Listrik." },
  { "en": "Apa Itu Special Relay (SR) Area Pada PLC?", "id": "Area Status Dan Flag Sistem." },
  { "en": "Shortcut Apa Untuk Membuka Preferences Arduino IDE?", "id": "Tekan Ctrl + Comma." },
  { "en": "Apa Fungsi Library SPI Transfer Pada Arduino?", "id": "Kirim Dan Terima Data Serentak." },
  { "en": "Bagaimana Cara Menambah Titik Junction Di Proteus?", "id": "Klik Kiri Pada Persimpangan Jalur." },
  { "en": "Apa Fungsi Instruksi WXOR (Word Exclusive OR)?", "id": "Operasi Logika XOR 16 Bit." },
  { "en": "Apa Fungsi Instruksi WNOT (Word NOT) Pada PLC?", "id": "Membalik Logika Data 16 Bit." },
  { "en": "Apa Fungsi Perintah Dimjogged Pada Dimensi AutoCAD?", "id": "Membuat Dimensi Radius Terpotong." },
  { "en": "Apa Itu Parity Check Pada Komunikasi Serial?", "id": "Metode Deteksi Error Bit Data." },
  { "en": "Apa Itu Null Character Pada String Arduino?", "id": "Penanda Akhir Untaian Teks." },
  { "en": "Shortcut Apa Untuk Insert Row Di GX Works?", "id": "Tekan Shift + Insert." },
  { "en": "Apa Fungsi LiquidCrystal SetCursor Pada Arduino?", "id": "Menentukan Posisi Tulisan LCD." },
  { "en": "Apa Fungsi Transfer Function Graph Di Proteus?", "id": "Analisis Respon Input Output DC." },
  { "en": "Apa Fungsi Timer Monostable Pada Logika PLC?", "id": "Satu Pulsa Output Saat Trigger." },
  { "en": "Bagaimana Cara Insert Dynamic Block AutoCAD?", "id": "Drag Dari Tool Palettes." },
  { "en": "Apa Fungsi Operator Bitwise Shift Left (Geser Kiri)?", "id": "Menggeser Bit Mengalikan Dua." },
  { "en": "Apa Itu Bus Entry Di Skematik Proteus?", "id": "Titik Masuk Jalur Ke Bus." },
  { "en": "Shortcut Apa Untuk Membuka Layer Manager AutoCAD?", "id": "Ketik Layer Lalu Enter." },
  { "en": "Apa Fungsi Serial Flush Pada Program Arduino?", "id": "Menunggu Pengiriman Data Selesai." },
  { "en": "Komponen Apa Yang Mengatur Gain Op Amp?", "id": "Resistor Feedback Negatif." },
  { "en": "Apa Fungsi Instruksi ASFT (Asynchronous Shift Register)?", "id": "Geser Data Register Asinkron." },
  { "en": "Bagaimana Cara Mengubah Xref Path Ke Relative?", "id": "Klik Kanan Xref Change Path." },
  { "en": "Apa Itu EEPROM Commit Pada ESP8266?", "id": "Simpan Data RAM Ke Flash." },
  { "en": "Apa Fungsi Audio Analysis Graph Di Proteus?", "id": "Melihat Spektrum Gelombang Suara." },
  { "en": "Apa Shortcut Convert Ladder Di GX Works?", "id": "Tekan Tombol F4." },
  { "en": "Apa Fungsi Perintah Sketch Pada Gambar AutoCAD?", "id": "Menggambar Garis Bebas Tangan." },
  { "en": "Apa Fungsi Operator Bitwise Shift Right (Geser Kanan)?", "id": "Menggeser Bit Membagi Dua." },
  { "en": "Apa Itu Logic Probe Large Di Proteus?", "id": "Indikator Logika Ukuran Besar." },
  { "en": "Bagaimana Cara Export Layout Ke PDF AutoCAD?", "id": "Gunakan Perintah Exportpdf." },
  { "en": "Apa Fungsi Perintah Solidedit Face Color?", "id": "Mengubah Warna Permukaan Solid 3D." },
  { "en": "Apa Tipe Data Unsigned Long Pada Arduino?", "id": "Bilangan Bulat Positif 32 Bit." },
  { "en": "Komponen Apa Pemicu Thyristor (SCR)?", "id": "Arus Masuk Ke Kaki Gate." },
  { "en": "Apa Fungsi Instruksi BSET (Block Set) PLC?", "id": "Mengisi Blok Memori Nilai Sama." },
  { "en": "Shortcut Apa Untuk Membuka Calculator AutoCAD?", "id": "Tekan Ctrl + 8." },
  { "en": "Apa Fungsi Keyword Break Pada Loop Arduino?", "id": "Keluar Paksa Dari Perulangan." },
  { "en": "Apa Itu Resolusi ADC 10 Bit Arduino?", "id": "Nilai Nol Sampai 1023." },
  { "en": "Apa Fungsi Instruksi STC (Set Carry Flag)?", "id": "Mengaktifkan Bit Carry Flag." },
  { "en": "Bagaimana Cara Paste Ke Koordinat Asli AutoCAD?", "id": "Pilih Paste To Original Coordinates." },
  { "en": "Apa Fungsi Keyword Const Pada Variabel Arduino?", "id": "Membuat Nilai Variabel Tetap." },
  { "en": "Apa Itu Sensor PIR (Passive Infrared) Proteus?", "id": "Sensor Deteksi Gerak Manusia." },
  { "en": "Apa Shortcut Upload Program Ke PLC Omron?", "id": "Tekan Ctrl + T." },
  { "en": "Apa Fungsi Perintah Chamfer Distance Di AutoCAD?", "id": "Mengatur Jarak Potong Sudut." },
  { "en": "Apa Fungsi LiquidCrystal Clear Pada Arduino?", "id": "Menghapus Seluruh Tulisan LCD." },
  { "en": "Komponen Apa Yang Menghubungkan Arus Kolektor NPN?", "id": "Resistor Beban Ke VCC." },
  { "en": "Apa Fungsi Carry Flag (CY) Pada PLC?", "id": "Indikator Sisa Hasil Operasi." },
  { "en": "Shortcut Apa Untuk Auto Format Code Arduino?", "id": "Tekan Ctrl + T." },
  { "en": "Apa Fungsi Serial Println Format HEX?", "id": "Cetak Nilai Dalam Hexadesimal." },
  { "en": "Apa Itu Clock Source Digital Di Proteus?", "id": "Pembangkit Sinyal Detak Konstan." },
  { "en": "Apa Fungsi Instruksi XNR (Exclusive NOR) PLC?", "id": "Logika Jika Input Sama." },
  { "en": "Bagaimana Cara Membuat Cahaya Matahari AutoCAD?", "id": "Gunakan Perintah Sunproperties." },
  { "en": "Apa Fungsi IsDigit Pada Karakter Arduino?", "id": "Memeriksa Apakah Karakter Angka." },
  { "en": "Apa Itu I2C Debugger Di Simulasi Proteus?", "id": "Alat Analisis Protokol I2C." },
  { "en": "Shortcut Apa Untuk Zoom Extents Di AutoCAD?", "id": "Klik Dua Kali Scroll Mouse." },
  { "en": "Apa Fungsi Perintah Thicken Pada Surface AutoCAD?", "id": "Mengubah Surface Menjadi Solid." },
  { "en": "Apa Fungsi String ToInt Pada Arduino?", "id": "Konversi Teks Angka Ke Integer." },
  { "en": "Apa Itu IC 74138 Decoder Demultiplexer?", "id": "Dekoder 3 Line Ke 8." },
  { "en": "Apa Fungsi Instruksi EI (Enable Interrupt) PLC?", "id": "Mengaktifkan Fungsi Interupsi PLC." },
  { "en": "Bagaimana Cara Mengukur Luas Region Di AutoCAD?", "id": "Gunakan Perintah Massprop." },
  { "en": "Apa Fungsi Pin RESET Pada Arduino Uno?", "id": "Restart Program Dari Awal." },
  { "en": "Apa Itu Power Terminal VCC Proteus?", "id": "Sumber Tegangan Positif Rangkaian." },
  { "en": "Shortcut Apa Untuk Membuka Text Window AutoCAD?", "id": "Tekan Tombol F2." },
  { "en": "Apa Fungsi Perintah Helix Base Radius?", "id": "Mengatur Radius Dasar Spiral." },
  { "en": "Apa Fungsi Karakter Escape Backslash T?", "id": "Membuat Tabulasi Horizontal Teks." },
  { "en": "Apa Itu Buck Converter Di Elektronika Daya?", "id": "Penurun Tegangan DC Ke DC." },
  { "en": "Apa Fungsi Analog Reference External Arduino?", "id": "Gunakan Tegangan Referensi Pin AREF." },
  { "en": "Bagaimana Cara Mengatur Tampilan Titik AutoCAD?", "id": "Gunakan Perintah Ptype." },
  { "en": "Apa Fungsi Pin MOSI Pada Jalur SPI?", "id": "Master Out Slave In Data." },
  { "en": "Apa Itu Via Pada Desain PCB Proteus?", "id": "Lubang Penghubung Antar Layer." },
  { "en": "Apa Shortcut Verify Sketch Di Arduino IDE?", "id": "Tekan Ctrl + R." },
  { "en": "Apa Fungsi Perintah Rotate Reference Di AutoCAD?", "id": "Memutar Objek Dengan Sudut Acuan." },
  { "en": "Apa Fungsi Operator Logika NOT (Tanda Seru)?", "id": "Membalik Nilai Kebenaran Boolean." },
  { "en": "Apa Fungsi Pin SCL Pada Jalur I2C?", "id": "Sinyal Clock Sinkronisasi Data." },
  { "en": "Apa Itu Resistor Array Di Proteus?", "id": "Paket Resistor Dalam Satu Komponen." },
  { "en": "Bagaimana Cara Update Data Link Di AutoCAD?", "id": "Klik Kanan Data Link Update." },
  { "en": "Apa Fungsi Perintah Dimjogged Pada Lingkaran?", "id": "Dimensi Radius Pusat Jauh." },
  { "en": "Apa Fungsi Wire Write Pada Komunikasi I2C?", "id": "Mengirim Data Byte Ke Bus." },
  { "en": "Apa Fungsi Pre-Production Check Di Proteus?", "id": "Verifikasi Desain Sebelum Dicetak." },
  { "en": "Apa Fungsi First Scan Flag Pada PLC?", "id": "Bit Aktif Saat Start Pertama." },
  { "en": "Bagaimana Cara Embed Objek OLE AutoCAD?", "id": "Pilih Paste Special Paste." },
  { "en": "Apa Fungsi Perintah Subtract Pada 3D Solid?", "id": "Memotong Bagian Solid Yang Beririsan." }



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
