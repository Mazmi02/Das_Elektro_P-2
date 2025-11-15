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


  { "en": "Sebutkan Contoh Baterai (Battery) Primer?", "id": "Alkaline, Lithium (Non-Isi Ulang)." },
  { "en": "Apa Elektrolit Pada Aki (Lead-Acid Battery)?", "id": "Larutan Asam Sulfat (H2SO4)." },
  { "en": "Apa Bahan Katoda Aki (Lead-Acid Battery)?", "id": "Timbal Dioksida (PbO2)." },
  { "en": "Apa Bahan Anoda Aki (Lead-Acid Battery)?", "id": "Timbal Murni (Pb)." },
  { "en": "Apa Ion Utama Baterai Lithium-Ion (Li-Ion)?", "id": "Ion Lithium (Li+)." },
  { "en": "Apa Keunggulan Baterai Lithium-Ion (Li-Ion)?", "id": "Kepadatan Energi Sangat Tinggi." },
  { "en": "Apa Satuan Kapasitas (Capacity) Baterai?", "id": "Ampere-Hour (Ah)." },
  { "en": "Apa Itu C-Rate (C-Rate) Baterai?", "id": "Laju Pengisian Atau Pengosongan Baterai." },
  { "en": "Apa Arti 1C (1C) Pada Baterai 10Ah?", "id": "Arus 10 Ampere." },
  { "en": "Apa Kepanjangan DoD (Depth of Discharge)?", "id": "Kedalaman Pengosongan." },
  { "en": "Apa Maksud 80 Persen DoD (Depth of Discharge)?", "id": "Baterai Digunakan 80 Persen Kapasitas." },
  { "en": "Apa Itu Siklus Hidup (Cycle Life) Baterai?", "id": "Jumlah Siklus Isi Ulang Baterai." },
  { "en": "Apa Kepanjangan BMS (Battery Management System)?", "id": "Sistem Manajemen Baterai." },
  { "en": "Apa Fungsi BMS (Battery Management System)?", "id": "Melindungi Baterai (Overcharge, Overdischarge)." },
  { "en": "Bagaimana Rangkaian Baterai (Battery) Menaikkan Tegangan?", "id": "Baterai Dihubungkan Secara Seri." },
  { "en": "Bagaimana Rangkaian Baterai (Battery) Menaikkan Kapasitas?", "id": "Baterai Dihubungkan Secara Paralel." },
  { "en": "Apa Itu Efek Memori (Memory Effect) Baterai?", "id": "Penurunan Kapasitas Baterai (NiCd, NiMH)." },
  { "en": "Apa Komponen Utama Panel Surya (Solar Panel)?", "id": "Sel Fotovoltaik (Photovoltaic Cell)." },
  { "en": "Apa Bahan Sel Surya (Solar Cell) Paling Umum?", "id": "Bahan Silikon (Si)." },
  { "en": "Apa Itu Kurva I-V (I-V Curve) Panel Surya?", "id": "Grafik Hubungan Arus, Tegangan Panel." },
  { "en": "Apa Itu Arus Hubung Singkat (Isc) Panel Surya?", "id": "Arus Maksimum Panel (Tegangan Nol)." },
  { "en": "Apa Itu Tegangan Rangkaian Terbuka (Voc) Panel Surya?", "id": "Tegangan Maksimum Panel (Arus Nol)." },
  { "en": "Apa Itu Titik Daya Maksimum (Pmax) Panel Surya?", "id": "Titik Operasi Daya Output Terbesar." },
  { "en": "Apa Kepanjangan MPPT (Maximum Power Point Tracking)?", "id": "Pelacakan Titik Daya Maksimum." },
  { "en": "Apa Fungsi MPPT (Maximum Power Point Tracking) Controller?", "id": "Mengoptimalkan Daya Output Panel Surya." },
  { "en": "Apa Kepanjangan SCC (Solar Charge Controller)?", "id": "Kontroler Pengisian Tenaga Surya." },
  { "en": "Apa Fungsi SCC (Solar Charge Controller)?", "id": "Mengatur Pengisian Baterai Dari Panel." },
  { "en": "Apa Beda MPPT (Maximum Power Point Tracking) Dan PWM (Pulse Width Modulation) SCC?", "id": "MPPT (Maximum Power Point Tracking) Lebih Efisien." },
  { "en": "Apa Itu Turbin Angin (Wind Turbine)?", "id": "Mesin Pengubah Energi Angin Ke Listrik." },
  { "en": "Apa Komponen Utama Turbin Angin (Wind Turbine)?", "id": "Bilah (Blade), Generator, Gearbox." },
  { "en": "Apa Kepanjangan HAWT (Horizontal Axis Wind Turbine)?", "id": "Turbin Angin Sumbu Horizontal." },
  { "en": "Apa Kepanjangan VAWT (Vertical Axis Wind Turbine)?", "id": "Turbin Angin Sumbu Vertikal." },
  { "en": "Apa Itu Sel Bahan Bakar (Fuel Cell)?", "id": "Mengubah Energi Kimia (Hidrogen) Ke Listrik." },
  { "en": "Apa Limbah Utama Sel Bahan Bakar (Fuel Cell)?", "id": "Air Murni (H2O)." },
  { "en": "Apa Itu Kualitas Daya (Power Quality) Listrik?", "id": "Ukuran Kestabilan Tegangan, Frekuensi, Gelombang." },
  { "en": "Apa Itu Sag (Sag) Tegangan?", "id": "Penurunan Tegangan Sesaat (Jangka Pendek)." },
  { "en": "Apa Itu Swell (Swell) Tegangan?", "id": "Kenaikan Tegangan Sesaat (Jangka Pendek)." },
  { "en": "Apa Itu Interupsi (Interruption) Sesaat?", "id": "Kehilangan Tegangan Total Sangat Singkat." },
  { "en": "Apa Itu Transien (Transient) Impulsif?", "id": "Lonjakan Tegangan Cepat (Kilat, Switching)." },
  { "en": "Apa Itu Transien (Transient) Osilasi?", "id": "Lonjakan Tegangan Berosilasi Cepat." },
  { "en": "Apa Itu Notching (Notching) Gelombang?", "id": "Gangguan Periodik Akibat Konverter Daya." },
  { "en": "Apa Itu Flicker (Flicker) Tegangan?", "id": "Fluktuasi Amplitudo Tegangan Perlahan." },
  { "en": "Apa Efek Flicker (Flicker) Tegangan?", "id": "Lampu Berkedip Mengganggu Mata." },
  { "en": "Apa Kepanjangan THD (Total Harmonic Distortion)?", "id": "Total Distorsi Harmonik." },
  { "en": "Apa Batas Ideal THD (Total Harmonic Distortion) Tegangan?", "id": "Idealnya Di Bawah 5 Persen." },
  { "en": "Apa Itu Beban Linear (Linear Load)?", "id": "Arus Sefasa, Bentuk Gelombang Sama." },
  { "en": "Apa Itu Beban Non-Linear (Non-Linear Load)?", "id": "Arus Beda Bentuk Gelombang (Harmonisa)." },
  { "en": "Berikan Contoh Beban Non-Linear (Non-Linear Load)?", "id": "Komputer (SMPS), Lampu LED, VFD." },
  { "en": "Apa Itu PQA (Power Quality Analyzer)?", "id": "Penganalisis Kualitas Daya." },
  { "en": "Apa Fungsi PQA (Power Quality Analyzer)?", "id": "Merekam, Menganalisis Gangguan Kualitas Daya." },
  { "en": "Apa Itu Filter Harmonisa (Harmonic Filter) Aktif?", "id": "Menginjeksikan Arus Kompensasi Harmonisa." },
  { "en": "Apa Itu Filter Harmonisa (Harmonic Filter) Pasif?", "id": "Rangkaian Resonansi LC (Induktor Kapasitor) Redam Harmonisa." },
  { "en": "Apa Itu K-Factor (K-Factor) Transformator?", "id": "Rating Trafo Tahan Panas Harmonisa." },
  { "en": "Apa Itu Pembumian (Grounding) Listrik?", "id": "Koneksi Konduktif Ke Bumi (Tanah)." },
  { "en": "Apa Tujuan Utama Pembumian (Grounding) Keamanan?", "id": "Melindungi Manusia Dari Sengatan Listrik." },
  { "en": "Apa Tujuan Utama Pembumian (Grounding) Sistem?", "id": "Menstabilkan Tegangan Referensi Sistem." },
  { "en": "Apa Itu Elektroda Bumi (Ground Electrode)?", "id": "Konduktor Ditanam Di Tanah (Ground Rod)." },
  { "en": "Apa Itu Resistansi Pembumian (Grounding Resistance)?", "id": "Hambatan Antara Elektroda, Tanah." },
  { "en": "Berapa Nilai Resistansi Pembumian (Grounding Resistance) Ideal?", "id": "Sekecil Mungkin (Di Bawah 1 Ohm)." },
  { "en": "Apa Itu Sistem Pembumian TN-S (TN-S System)?", "id": "Netral, Ground Terpisah (5 Kawat)." },
  { "en": "Apa Itu Sistem Pembumian TN-C (TN-C System)?", "id": "Netral, Ground Gabung (PEN) (4 Kawat)." },
  { "en": "Apa Itu Sistem Pembumian TN-C-S (TN-C-S System)?", "id": "Gabungan TN-C (TN-C System) Dan TN-S (TN-S System)." },
  { "en": "Apa Itu Sistem Pembumian TT (TT System)?", "id": "Netral, Bodi Dibumikan Terpisah." },
  { "en": "Apa Itu Sistem Pembumian IT (IT System)?", "id": "Sistem Terisolasi Dari Tanah." },
  { "en": "Apa Itu Ikatan Ekuipotensial (Equipotential Bonding)?", "id": "Menghubungkan Semua Bodi Konduktif." },
  { "en": "Apa Kepanjangan LOTO (Lockout Tagout)?", "id": "Kunci Gembok, Pasang Label." },
  { "en": "Kapan Prosedur LOTO (Lockout Tagout) Digunakan?", "id": "Saat Perbaikan, Pemeliharaan Peralatan Listrik." },
  { "en": "Apa Tujuan LOTO (Lockout Tagout)?", "id": "Mencegah Peralatan Hidup Tidak Sengaja." },
  { "en": "Apa Kepanjangan APD (Alat Pelindung Diri)?", "id": "Alat Pelindung Diri." },
  { "en": "Apa Nama Lain APD (Alat Pelindung Diri)?", "id": "PPE (Personal Protective Equipment)." },
  { "en": "Sebutkan Contoh APD (Alat Pelindung Diri) Listrik?", "id": "Helm, Sarung Tangan Isolasi, Sepatu Safety." },
  { "en": "Apa Itu Sarung Tangan Isolasi (Insulating Gloves)?", "id": "Sarung Tangan Karet Pelindung Tegangan." },
  { "en": "Apa Itu Arc Flash (Kilatan Busur Api) Suit?", "id": "Pakaian Tahan Panas Ledakan Busur." },
  { "en": "Apa Itu Tes Pen (Test Pen)?", "id": "Obeng Penguji Adanya Tegangan Listrik." },
  { "en": "Bagaimana Tes Pen (Test Pen) Bekerja?", "id": "Lampu Neon Menyala Jika Ada Fasa." },
  { "en": "Apa Itu Voltmeter Tanpa Kontak (Non-Contact Voltmeter)?", "id": "Deteksi Tegangan Tanpa Sentuhan Fisik." },
  { "en": "Apa Itu Megger (Megger)?", "id": "Nama Merek Insulation Tester." },
  { "en": "Apa Fungsi Insulation Tester (Megohmmeter)?", "id": "Mengukur Resistansi Isolasi Kabel, Motor." },
  { "en": "Berapa Tegangan Uji Insulation Tester (Megohmmeter)?", "id": "Tegangan DC Sangat Tinggi (500V, 1000V)." },
  { "en": "Apa Itu Earth Tester (Penguji Bumi)?", "id": "Alat Ukur Resistansi Pembumian." },
  { "en": "Metode Apa Yang Dipakai Earth Tester (Penguji Bumi)?", "id": "Metode Tiga Titik (Fall of Potential)." },
  { "en": "Apa Itu Phase Sequence Tester (Penguji Urutan Fasa)?", "id": "Memeriksa Urutan Fasa (R-S-T)." },
  { "en": "Mengapa Urutan Fasa (Phase Sequence) Penting?", "id": "Menentukan Arah Putaran Motor Tiga Fasa." },
  { "en": "Bagaimana Membalik Putaran Motor Tiga Fasa?", "id": "Tukar Sambungan Dua Kabel Fasa." },
  { "en": "Bagaimana Membalik Putaran Motor DC (Direct Current)?", "id": "Balik Polaritas Jangkar Atau Medan." },
  { "en": "Apa Itu Motor DC (Direct Current) Brushless (Tanpa Sikat)?", "id": "Motor BLDC (Brushless DC Motor)." },
  { "en": "Apa Komponen Komutasi Motor BLDC (Brushless DC Motor)?", "id": "Sensor Hall Dan Kontroler Elektronik." },
  { "en": "Apa Keuntungan Motor BLDC (Brushless DC Motor)?", "id": "Efisien Tinggi, Awet (Tanpa Sikat)." },
  { "en": "Apa Itu Motor Servo (Servo Motor)?", "id": "Motor Kontrol Posisi Loop Tertutup." },
  { "en": "Apa Umpan Balik (Feedback) Motor Servo?", "id": "Encoder Atau Potensiometer." },
  { "en": "Apa Itu Motor Stepper (Stepper Motor)?", "id": "Motor Bergerak Dalam Langkah (Steps)." },
  { "en": "Bagaimana Kontrol Motor Stepper (Stepper Motor)?", "id": "Loop Terbuka (Open Loop)." },
  { "en": "Apa Itu Torsi (Torque) Motor?", "id": "Gaya Putar Yang Dihasilkan Motor." },
  { "en": "Apa Satuan Torsi (Torque)?", "id": "Newton-Meter (N·m)." },
  { "en": "Apa Itu Torsi Awal (Starting Torque)?", "id": "Torsi Motor Saat Mulai Berputar." },
  { "en": "Apa Itu Torsi Beban Penuh (Full Load Torque)?", "id": "Torsi Motor Pada Beban Maksimal." },
  { "en": "Apa Itu Torsi Tembus (Breakdown Torque)?", "id": "Torsi Maksimum Motor Sebelum Berhenti." },
  { "en": "Apa Itu Daya Kuda (Horsepower) (HP)?", "id": "Satuan Daya Motor (Non-SI)." },
  { "en": "Berapa Konversi 1 Daya Kuda (Horsepower) (HP)?", "id": "Sekitar 746 Watt (W)." },
  { "en": "Apa Itu Peringkat IP (Ingress Protection) Motor?", "id": "Proteksi Motor Terhadap Debu, Air." },
  { "en": "Apa Itu Peringkat NEMA (National Electrical Manufacturers Association)?", "id": "Standar Proteksi Kotak Panel (AS)." },
  { "en": "Apa Beda NEMA (National Electrical Manufacturers Association) Dan IP (Ingress Protection)?", "id": "NEMA (National Electrical Manufacturers Association) Standar AS, IP (Ingress Protection) Internasional." },
  { "en": "Apa Itu Motor Tipe TEFC (Totally Enclosed Fan Cooled)?", "id": "Motor Tertutup Total Didinginkan Kipas." },
  { "en": "Apa Itu Motor Tipe ODP (Open Drip Proof)?", "id": "Motor Terbuka Tahan Tetesan Air." },
  { "en": "Apa Itu Kelas Isolasi (Insulation Class) Motor?", "id": "Batas Suhu Operasi Maksimal Belitan." },
  { "en": "Sebutkan Contoh Kelas Isolasi (Insulation Class) Motor?", "id": "Kelas F (155°C), Kelas H (180°C)." },
  { "en": "Apa Itu Faktor Servis (Service Factor) Motor?", "id": "Kemampuan Beban Lebih Jangka Pendek." },
  { "en": "Apa Arti Faktor Servis (Service Factor) 1.15?", "id": "Bisa Dibebani 115 Persen." },
  { "en": "Apa Itu Nameplate (Papan Nama) Motor?", "id": "Label Spesifikasi Teknis Motor." },
  { "en": "Apa Itu Arus Beban Penuh (Full Load Amps) (FLA)?", "id": "Arus Motor Pada Beban Penuh." },
  { "en": "Apa Itu Arus Rotor Terkunci (Locked Rotor Amps) (LRA)?", "id": "Arus Start Motor Saat Terkunci." },
  { "en": "Apa Itu Efisiensi (Efficiency) Motor?", "id": "Rasio Daya Mekanik Output, Listrik Input." },
  { "en": "Apa Itu Pengujian Tanpa Beban (No-Load Test) Motor?", "id": "Mengukur Rugi Inti, Gesekan, Angin." },
  { "en": "Apa Itu Pengujian Rotor Terkunci (Locked Rotor Test)?", "id": "Mengukur Rugi Tembaga, Reaktansi." },
  { "en": "Apa Itu Bantalan (Bearing) Motor?", "id": "Komponen Penopang Poros Rotor." },
  { "en": "Sebutkan Jenis Bantalan (Bearing) Motor?", "id": "Ball Bearing, Sleeve Bearing." },
  { "en": "Apa Fungsi Rumah (Housing) Motor?", "id": "Melindungi Bagian Internal Motor." },
  { "en": "Apa Fungsi Kipas (Fan) Motor?", "id": "Media Pendingin Motor." },
  { "en": "Apa Itu Kotak Terminal (Terminal Box) Motor?", "id": "Tempat Sambungan Kabel Daya Motor." },
  { "en": "Apa Itu Belitan Stator (Stator Winding)?", "id": "Kumparan Diam Penghasil Medan Magnet." },
  { "en": "Apa Itu Belitan Rotor (Rotor Winding)?", "id": "Kumparan Berputar (Motor Rotor Lilit)." },
  { "en": "Apa Itu Cincin Geser (Slip Ring)?", "id": "Penghubung Arus Listrik Ke Rotor." },
  { "en": "Dimana Cincin Geser (Slip Ring) Digunakan?", "id": "Motor Rotor Lilit, Generator Sinkron." },
  { "en": "Apa Itu Sikat Arang (Carbon Brush)?", "id": "Kontak Gesek Ke Cincin Geser/Komutator." },
  { "en": "Apa Itu Motor Rotor Lilit (Wound Rotor Motor)?", "id": "Motor Induksi Dengan Belitan Rotor." },
  { "en": "Apa Keuntungan Motor Rotor Lilit (Wound Rotor Motor)?", "id": "Kontrol Torsi, Kecepatan Awal." },
  { "en": "Apa Itu Motor Satu Fasa (Single-Phase Motor)?", "id": "Motor Beroperasi Listrik Satu Fasa." },
  { "en": "Mengapa Motor Satu Fasa (Single-Phase Motor) Perlu Bantuan Start?", "id": "Tidak Punya Medan Putar Awal." },
  { "en": "Apa Itu Motor Fasa Belah (Split-Phase Motor)?", "id": "Motor Satu Fasa (Belitan Bantu)." },
  { "en": "Apa Itu Motor Kapasitor (Capacitor Motor)?", "id": "Motor Satu Fasa (Bantuan Kapasitor)." },
  { "en": "Apa Itu Motor Kapasitor Start (Capacitor Start Motor)?", "id": "Kapasitor Lepas Setelah Start." },
  { "en": "Apa Itu Motor Kapasitor Run (Capacitor Run Motor)?", "id": "Kapasitor Terhubung Terus Menerus." },
  { "en": "Apa Itu Motor Kapasitor Start-Run (Capacitor Start-Run)?", "id": "Dua Kapasitor (Start, Run)." },
  { "en": "Apa Itu Saklar Sentrifugal (Centrifugal Switch)?", "id": "Saklar Pelepas Belitan Bantu Motor." },
  { "en": "Apa Itu Motor Kutub Bayangan (Shaded-Pole Motor)?", "id": "Motor Satu Fasa Torsi Rendah." },
  { "en": "Dimana Motor Kutub Bayangan (Shaded-Pole Motor) Digunakan?", "id": "Kipas Angin Kecil, Pompa Kecil." },
  { "en": "Apa Itu Sinkronisasi (Synchronization) Generator?", "id": "Proses Paralel Generator Ke Jaringan." },
  { "en": "Apa Syarat Sinkronisasi (Synchronization) Generator?", "id": "Tegangan, Frekuensi, Urutan Fasa Sama." },
  { "en": "Apa Alat Untuk Sinkronisasi (Synchronization)?", "id": "Sinkroskop (Synchroscope) Atau Lampu Sinkron." },
  { "en": "Apa Itu Sinkroskop (Synchroscope)?", "id": "Alat Ukur Beda Frekuensi, Fasa." },
  { "en": "Apa Itu Pembagian Beban (Load Sharing) Generator?", "id": "Membagi Beban Antar Generator Paralel." },
  { "en": "Apa Itu Perburuan (Hunting) Generator Sinkron?", "id": "Osilasi Kecepatan Rotor Generator." },
  { "en": "Apa Penyebab Perburuan (Hunting) Generator?", "id": "Perubahan Beban Tiba-Tiba." },
  { "en": "Apa Itu Belitan Peredam (Damper Winding) Generator?", "id": "Belitan Meredam Osilasi (Perburuan)." },
  { "en": "Apa Itu Sistem Eksitasi (Excitation System) Generator?", "id": "Sistem Pembangkit Medan Magnet Rotor." },
  { "en": "Apa Itu Eksitasi Statis (Static Excitation)?", "id": "Eksitasi Menggunakan Penyearah Terkontrol." },
  { "en": "Apa Itu Eksitasi Tanpa Sikat (Brushless Excitation)?", "id": "Eksitasi Tanpa Sikat Arang." },
  { "en": "Apa Itu Governor (Governor) Turbin?", "id": "Pengatur Kecepatan Putaran Turbin." },
  { "en": "Apa Yang Diatur Governor (Governor)?", "id": "Aliran Fluida (Uap, Air, Gas)." },
  { "en": "Apa Fungsi Governor (Governor) Di Pembangkit?", "id": "Mengatur Frekuensi Sistem Listrik." },
  { "en": "Apa Itu Turbin Uap (Steam Turbine)?", "id": "Turbin Digerakkan Uap Bertekanan." },
  { "en": "Apa Itu Turbin Air (Water Turbine)?", "id": "Turbin Digerakkan Aliran Air." },
  { "en": "Apa Itu Turbin Gas (Gas Turbine)?", "id": "Turbin Digerakkan Gas Hasil Pembakaran." },
  { "en": "Apa Itu Siklus Kombinasi (Combined Cycle) PLTGU?", "id": "Gabungan Turbin Gas, Turbin Uap." },
  { "en": "Apa Kepanjangan PLTGU (Pembangkit Listrik Tenaga Gas Uap)?", "id": "Pembangkit Listrik Tenaga Gas Uap." },
  { "en": "Apa Itu Boiler (Ketel Uap)?", "id": "Peralatan Pembangkit Uap (PLTU)." },
  { "en": "Apa Itu Kondensor (Condenser) PLTU?", "id": "Mengubah Uap Sisa Turbin Menjadi Air." },
  { "en": "Apa Itu Menara Pendingin (Cooling Tower)?", "id": "Mendinginkan Air Pendingin Kondensor." },
  { "en": "Apa Itu Reaktor Nuklir (Nuclear Reactor)?", "id": "Pembangkit Panas (Reaksi Fisi) PLTN." },
  { "en": "Apa Kepanjangan PLTN (Pembangkit Listrik Tenaga Nuklir)?", "id": "Pembangkit Listrik Tenaga Nuklir." },
  { "en": "Apa Itu Reaksi Fisi (Fission Reaction)?", "id": "Pembelahan Inti Atom Hasilkan Energi." },
  { "en": "Apa Bahan Bakar Umum PLTN (Pembangkit Listrik Tenaga Nuklir)?", "id": "Uranium (U-235)." },
  { "en": "Apa Itu Batang Kendali (Control Rods) Reaktor?", "id": "Mengatur Laju Reaksi Fisi Nuklir." },
  { "en": "Apa Itu Moderator (Moderator) Reaktor?", "id": "Memperlambat Neutron (Contoh: Air Berat)." },
  { "en": "Apa Itu Konduktivitas (Conductivity) Bahan?", "id": "Kemampuan Bahan Menghantarkan Listrik." },
  { "en": "Apa Satuan Konduktivitas (Conductivity)?", "id": "Siemens Per Meter (S/m)." },
  { "en": "Apa Itu Resistivitas (Resistivity)?", "id": "Kebalikan Dari Konduktivitas." },
  { "en": "Bahan Apa Konduktor Listrik Terbaik?", "id": "Perak (Silver)." },
  { "en": "Mengapa Kabel Listrik Pakai Tembaga (Copper)?", "id": "Konduktivitas Tinggi, Harga Murah." },
  { "en": "Apa Itu Aluminium (Aluminum) Sebagai Konduktor?", "id": "Lebih Ringan, Konduktivitas Sedikit Rendah." },
  { "en": "Apa Itu Superkonduktor (Superconductor)?", "id": "Bahan Hambatan Nol Suhu Rendah." },
  { "en": "Apa Itu Karet (Rubber) Sebagai Isolator?", "id": "Isolator Fleksibel (Pembungkus Kabel)." },
  { "en": "Apa Itu Porselen (Porcelain)?", "id": "Keramik Isolator Tegangan Tinggi." },
  { "en": "Apa Itu Kaca (Glass) Sebagai Isolator?", "id": "Isolator Jaringan Transmisi." },
  { "en": "Apa Itu Mika (Mica)?", "id": "Isolator Tahan Panas Tinggi." },
  { "en": "Apa Itu PVC (Polyvinyl Chloride)?", "id": "Isolator Kabel Plastik Umum." },
  { "en": "Apa Itu XLPE (Cross-Linked Polyethylene)?", "id": "Isolator Kabel Tahan Suhu Tinggi." },
  { "en": "Apa Itu Gas SF6 (Sulfur Hexafluoride)?", "id": "Gas Isolator Kekuatan Dielektrik Tinggi." },
  { "en": "Apa Itu Minyak Trafo (Transformer Oil)?", "id": "Cairan Isolator, Pendingin Transformator." },
  { "en": "Apa Itu Perakitan PCB (PCB Assembly)?", "id": "Proses Pemasangan Komponen Ke PCB." },
  { "en": "Apa Itu Solder Tangan (Hand Soldering)?", "id": "Menyolder Manual Menggunakan Solder." },
  { "en": "Apa Itu Solder Gelombang (Wave Soldering)?", "id": "Metode Solder Komponen Through-Hole." },
  { "en": "Apa Itu Solder Reflow (Reflow Soldering)?", "id": "Metode Solder Komponen SMD (Surface Mount Device)." },
  { "en": "Apa Itu Pasta Solder (Solder Paste)?", "id": "Campuran Timah, Fluks (Reflow)." },
  { "en": "Apa Itu Stensil (Stencil) SMT (Surface Mount Technology)?", "id": "Cetakan Aplikasi Pasta Solder." },
  { "en": "Apa Itu Mesin Pick-and-Place (Pick-and-Place Machine)?", "id": "Mesin Pemasang Komponen SMD Otomatis." },
  { "en": "Apa Kepanjangan AOI (Automated Optical Inspection)?", "id": "Inspeksi Visual Otomatis Perakitan PCB." },
  { "en": "Apa Kepanjangan ICT (In-Circuit Test)?", "id": "Pengujian Fungsional Komponen Di Sirkuit." },
  { "en": "Apa Itu Lapisan Konformal (Conformal Coating)?", "id": "Lapisan Pelindung PCB (Kelembaban, Debu)." },
  { "en": "Apa Itu Busur Api (Arcing) Listrik?", "id": "Loncatan Listrik Lewat Udara (Plasma)." },
  { "en": "Apa Itu Perambatan (Tracking) Listrik?", "id": "Jalur Konduktif Terbentuk Permukaan Isolator." },
  { "en": "Apa Itu Jarak Bebas (Clearance) Listrik?", "id": "Jarak Terpendek Lewat Udara." },
  { "en": "Apa Itu Jarak Rambat (Creepage) Listrik?", "id": "Jarak Terpendek Lewat Permukaan Isolator." },
  { "en": "Mengapa Jarak Rambat (Creepage) Penting?", "id": "Mencegah Perambatan (Tracking) Akibat Kontaminasi." },
  { "en": "Apa Itu Hipot (Hipot Test) (High Potential)?", "id": "Pengujian Kekuatan Isolasi Tegangan Tinggi." },
  { "en": "Apa Tujuan Hipot (Hipot Test) (High Potential)?", "id": "Memastikan Isolasi Tidak Tembus (Bocor)." },
  { "en": "Apa Itu Indeks Polarisasi (Polarization Index) (PI)?", "id": "Pengujian Kondisi Isolasi (Megger)." },
  { "en": "Apa Itu Rasio Absorpsi Dielektrik (Dielectric Absorption Ratio) (DAR)?", "id": "Pengujian Kondisi Isolasi (Megger)." },
  { "en": "Apa Itu Kapasitansi Parasitik (Parasitic Capacitance)?", "id": "Kapasitansi Tidak Diinginkan Antar Komponen." },
  { "en": "Apa Itu Induktansi Parasitik (Parasitic Inductance)?", "id": "Induktansi Tidak Diinginkan Jalur PCB." },
  { "en": "Apa Itu Efek Miller (Miller Effect)?", "id": "Peningkatan Kapasitansi Input Penguat Inverting." },
  { "en": "Apa Itu Bootstrap (Bootstrap) Sirkuit?", "id": "Rangkaian Umpan Balik Positif Naikkan Impedansi." },
  { "en": "Apa Itu Penguat Logaritmik (Logarithmic Amplifier)?", "id": "Output Proporsional Logaritma Input." },
  { "en": "Apa Itu Medan Listrik (Electric Field) (E)?", "id": "Daerah Dipengaruhi Gaya Listrik." },
  { "en": "Apa Itu Medan Magnet (Magnetic Field) (B)?", "id": "Daerah Dipengaruhi Gaya Magnet." },
  { "en": "Apa Itu Gelombang Elektromagnetik (Electromagnetic Wave)?", "id": "Getaran Medan Listrik, Magnet." },
  { "en": "Siapa Perumus Persamaan Maxwell (Maxwell's Equations)?", "id": "James Clerk Maxwell." },
  { "en": "Apa Inti Persamaan Maxwell (Maxwell's Equations)?", "id": "Dasar Teori Elektromagnetisme Klasik." },
  { "en": "Apa Satuan Fluks Listrik (Electric Flux)?", "id": "Volt-Meter (V·m)." },
  { "en": "Apa Itu Arus Perpindahan (Displacement Current)?", "id": "Arus Akibat Perubahan Medan Listrik." },
  { "en": "Apa Itu Konstanta Listrik (Permitivitas) Ruang Hampa (ε₀)?", "id": "Permitivitas Ruang Hampa." },
  { "en": "Apa Itu Konstanta Magnetik (Permeabilitas) Ruang Hampa (μ₀)?", "id": "Permeabilitas Ruang Hampa." },
  { "en": "Berapa Kecepatan Cahaya (c) Dalam Ruang Hampa?", "id": "c = 1 / √(ε₀μ₀)." },
  { "en": "Apa Itu Impedansi Gelombang (Wave Impedance) Ruang Hampa?", "id": "Z₀ = √(μ₀ / ε₀) (Sekitar 377 Ω)." },
  { "en": "Apa Itu Vektor Poynting (Poynting Vector) (S)?", "id": "Arah, Laju Aliran Energi Elektromagnetik." },
  { "en": "Apa Itu Radiasi (Radiation) Elektromagnetik?", "id": "Pancaran Energi Gelombang Elektromagnetik." },
  { "en": "Apa Itu Spektrum Frekuensi (Frequency Spectrum)?", "id": "Rentang Frekuensi Gelombang Elektromagnetik." },
  { "en": "Urutkan Spektrum Frekuensi Terendah Ke Tinggi?", "id": "Radio, Mikro, Inframerah, Cahaya, UV, X-Ray." },
  { "en": "Apa Itu Dualitas Gelombang-Partikel (Wave-Particle Duality)?", "id": "Cahaya Bersifat Gelombang, Partikel." },
  { "en": "Apa Nama Partikel Cahaya (Light Particle)?", "id": "Foton (Photon)." },
  { "en": "Apa Itu Efek Fotolistrik (Photoelectric Effect)?", "id": "Elektron Lepas Dari Logam Akibat Cahaya." },
  { "en": "Apa Itu Termionik (Thermionic Emission)?", "id": "Emisi Elektron Akibat Pemanasan." },
  { "en": "Dimana Emisi Termionik (Thermionic Emission) Digunakan?", "id": "Tabung Vakum (Vacuum Tube)." },
  { "en": "Apa Itu Tabung Vakum (Vacuum Tube)?", "id": "Komponen Elektronik (Dioda, Trioda Tabung)." },
  { "en": "Apa Itu Katoda (Cathode) Tabung Vakum?", "id": "Elemen Pemanas Pemancar Elektron." },
  { "en": "Apa Itu Anoda (Anode) Tabung Vakum?", "id": "Elemen Positif Penerima Elektron." },
  { "en": "Apa Itu Grid (Grid) Tabung Vakum Trioda?", "id": "Elemen Pengontrol Aliran Elektron." },
  { "en": "Apa Itu Tabung Sinar Katoda (Cathode Ray Tube)?", "id": "CRT (Cathode Ray Tube) (Layar Tabung)." },
  { "en": "Apa Kepanjangan CRT (Cathode Ray Tube)?", "id": "Tabung Sinar Katoda." },
  { "en": "Bagaimana CRT (Cathode Ray Tube) Membentuk Gambar?", "id": "Menembakkan Elektron Ke Layar Fosfor." },
  { "en": "Apa Itu Osiloskop Analog (Analog Oscilloscope)?", "id": "Menggunakan CRT (Cathode Ray Tube) Tampilkan Sinyal." },
  { "en": "Apa Itu Magnetron (Magnetron)?", "id": "Tabung Vakum Hasilkan Gelombang Mikro." },
  { "en": "Dimana Magnetron (Magnetron) Digunakan?", "id": "Oven Microwave Dan Radar." },
  { "en": "Apa Itu Klystron (Klystron)?", "id": "Tabung Vakum Penguat Frekuensi Tinggi." },
  { "en": "Apa Itu Efek Kulit (Skin Effect)?", "id": "Arus AC Mengalir Permukaan Konduktor." },
  { "en": "Kapan Efek Kulit (Skin Effect) Signifikan?", "id": "Pada Frekuensi Sangat Tinggi." },
  { "en": "Apa Akibat Efek Kulit (Skin Effect)?", "id": "Meningkatkan Resistansi Efektif Kabel." },
  { "en": "Apa Itu Efek Kedekatan (Proximity Effect)?", "id": "Distribusi Arus Terganggu Konduktor Dekat." },
  { "en": "Apa Itu Kawat Litz (Litz Wire)?", "id": "Kabel Serabut Terisolasi Kurangi Efek Kulit." },
  { "en": "Apa Itu Saluran Transmisi (Transmission Line)?", "id": "Struktur Pemandu Gelombang Elektromagnetik." },
  { "en": "Sebutkan Contoh Saluran Transmisi (Transmission Line)?", "id": "Kabel Koaksial, Kabel Twisted Pair, Microstrip." },
  { "en": "Apa Itu Microstrip (Microstrip)?", "id": "Saluran Transmisi Di Atas PCB." },
  { "en": "Apa Itu Stripline (Stripline)?", "id": "Saluran Transmisi Di Dalam PCB." },
  { "en": "Apa Itu Pemandu Gelombang (Waveguide)?", "id": "Pipa Logam Pemandu Gelombang Mikro." },
  { "en": "Apa Itu Mode TM (Transverse Magnetic)?", "id": "Mode Gelombang (Medan Magnet Transversal)." },
  { "en": "Apa Itu Mode TE (Transverse Electric)?", "id": "Mode Gelombang (Medan Listrik Transversal)." },
  { "en": "Apa Itu Mode TEM (Transverse Electromagnetic)?", "id": "Mode Gelombang (Listrik, Magnet Transversal)." },
  { "en": "Apa Itu Frekuensi Cutoff (Cutoff Frequency) Waveguide?", "id": "Frekuensi Minimum Dapat Dilewatkan Waveguide." },
  { "en": "Apa Itu Resonator Rongga (Cavity Resonator)?", "id": "Rongga Logam Resonansi Gelombang Mikro." },
  { "en": "Apa Itu Atenuasi (Attenuation) Sinyal?", "id": "Pelemahan Kekuatan Sinyal Seiring Jarak." },
  { "en": "Apa Satuan Atenuasi (Attenuation)?", "id": "Desibel Per Meter (dB/m)." },
  { "en": "Apa Itu Kabel Serat Optik (Fiber Optic Cable)?", "id": "Media Transmisi Menggunakan Cahaya." },
  { "en": "Apa Inti (Core) Serat Optik?", "id": "Bagian Tengah Kaca (Tempat Cahaya Merambat)." },
  { "en": "Apa Selubung (Cladding) Serat Optik?", "id": "Lapisan Kaca Mengelilingi Inti." },
  { "en": "Apa Prinsip Kerja Serat Optik?", "id": "Pemantulan Internal Total Cahaya." },
  { "en": "Apa Itu Serat Optik Mode Tunggal (Single Mode)?", "id": "Inti Sangat Kecil (Satu Jalur Cahaya)." },
  { "en": "Apa Itu Serat Optik Mode Jamak (Multi Mode)?", "id": "Inti Lebih Besar (Banyak Jalur Cahaya)." },
  { "en": "Apa Keuntungan Serat Optik Mode Tunggal (Single Mode)?", "id": "Jarak Sangat Jauh, Bandwidth Tinggi." },
  { "en": "Apa Keuntungan Serat Optik Mode Jamak (Multi Mode)?", "id": "Lebih Murah, Konektor Mudah." },
  { "en": "Apa Itu Dispersi (Dispersion) Serat Optik?", "id": "Pelebaran Pulsa Cahaya (Batasi Bandwidth)." },
  { "en": "Apa Itu Splicing (Splicing) Serat Optik?", "id": "Proses Penyambungan Dua Serat Optik." },
  { "en": "Apa Itu Splicing Fusi (Fusion Splicing)?", "id": "Menyambung Serat Optik Dengan Panas." },
  { "en": "Apa Kepanjangan OTDR (Optical Time Domain Reflectometer)?", "id": "Reflektometer Domain Waktu Optik." },
  { "en": "Apa Fungsi OTDR (Optical Time Domain Reflectometer)?", "id": "Menguji, Mencari Lokasi Putus Serat Optik." },
  { "en": "Apa Itu Sumber Cahaya (Light Source) Serat Optik?", "id": "LED (Light Emitting Diode) Atau Dioda Laser." },
  { "en": "Apa Itu Detektor (Detector) Serat Optik?", "id": "Fotodioda (Menerima Sinyal Cahaya)." },
  { "en": "Apa Kepanjangan WDM (Wavelength Division Multiplexing)?", "id": "Multipleksasi Pembagian Panjang Gelombang." },
  { "en": "Apa Fungsi WDM (Wavelength Division Multiplexing)?", "id": "Mengirim Banyak Sinyal (Warna) Satu Serat." },
  { "en": "Apa Itu Penguat Optik (Optical Amplifier)?", "id": "Menguatkan Sinyal Cahaya Langsung." },
  { "en": "Apa Kepanjangan EDFA (Erbium Doped Fiber Amplifier)?", "id": "Penguat Serat Di-doping Erbium." },
  { "en": "Apa Itu Atenuator (Attenuator) Optik?", "id": "Melemahkan Sinyal Cahaya (Jika Terlalu Kuat)." },
  { "en": "Apa Itu Konektor (Connector) Serat Optik?", "id": "Konektor Sambungan Serat Optik." },
  { "en": "Sebutkan Tipe Konektor (Connector) Serat Optik?", "id": "SC (Subscriber Connector), LC (Lucent Connector), FC (Ferrule Connector)." },
  { "en": "Apa Itu Rugi Penyisipan (Insertion Loss) Konektor?", "id": "Kehilangan Daya Akibat Sambungan Konektor." },
  { "en": "Apa Itu Rugi Kembali (Return Loss) Optik?", "id": "Cahaya Yang Dipantulkan Kembali Konektor." },
  { "en": "Apa Itu Pigtail (Pigtail) Serat Optik?", "id": "Seutas Serat Optik Dengan Konektor." },
  { "en": "Apa Itu Patch Cord (Patch Cord) Serat Optik?", "id": "Kabel Optik Dua Konektor (Sambungan)." },
  { "en": "Apa Kepanjangan OLT (Optical Line Terminal)?", "id": "Terminal Jalur Optik (Pusat)." },
  { "en": "Apa Kepanjangan ONT (Optical Network Terminal)?", "id": "Terminal Jaringan Optik (Pelanggan)." },
  { "en": "Apa Kepanjangan ONU (Optical Network Unit)?", "id": "Unit Jaringan Optik (Pelanggan)." },
  { "en": "Apa Kepanjangan PON (Passive Optical Network)?", "id": "Jaringan Optik Pasif." },
  { "en": "Apa Itu Splitter (Splitter) Optik?", "id": "Membagi Sinyal Cahaya Pasif." },
  { "en": "Apa Itu Pengukur Daya Optik (Optical Power Meter)?", "id": "Mengukur Kekuatan Sinyal Cahaya (dBm)." },
  { "en": "Apa Itu Sumber Cahaya Optik (Optical Light Source)?", "id": "Sumber Cahaya Uji Redaman Jaringan." },
  { "en": "Apa Itu Visual Fault Locator (VFL)?", "id": "Senter Laser Merah (Mencari Tekukan)." },
  { "en": "Apa Kepanjangan VFL (Visual Fault Locator)?", "id": "Lokator Kesalahan Visual." },
  { "en": "Apa Itu Koefisien Refleksi (Reflection Coefficient)?", "id": "Rasio Amplitudo Gelombang Pantul, Datang." },
  { "en": "Apa Itu Transmisi (Transmission) Sinyal?", "id": "Pengiriman Sinyal Lewat Media." },
  { "en": "Apa Itu Penerimaan (Reception) Sinyal?", "id": "Penerimaan Sinyal Oleh Penerima." },
  { "en": "Apa Itu Modulator (Modulator)?", "id": "Perangkat Melakukan Modulasi." },
  { "en": "Apa Itu Demodulator (Demodulator)?", "id": "Perangkat Melakukan Demodulasi." },
  { "en": "Apa Kepanjangan Modem (Modulator Demodulator)?", "id": "Modulator Demodulator." },
  { "en": "Apa Itu Pengkodean (Coding) Kanal?", "id": "Menambahkan Redundansi Data (Deteksi Error)." },
  { "en": "Apa Itu Pengkodean (Coding) Sumber?", "id": "Kompresi Data (Mengurangi Redundansi)." },
  { "en": "Apa Itu Teori Informasi (Information Theory)?", "id": "Studi Kuantifikasi, Komunikasi Informasi." },
  { "en": "Siapa Bapak Teori Informasi (Information Theory)?", "id": "Claude Shannon." },
  { "en": "Apa Itu Entropi (Entropy) Informasi?", "id": "Ukuran Ketidakpastian Informasi." },
  { "en": "Apa Satuan Entropi (Entropy) Informasi?", "id": "Bit (Bits)." },
  { "en": "Apa Itu Laju Bit (Bit Rate)?", "id": "Jumlah Bit Ditransfer Per Detik." },
  { "en": "Apa Satuan Laju Bit (Bit Rate)?", "id": "Bit Per Detik (bps)." },
  { "en": "Apa Itu Derajat Kebebasan (Degrees of Freedom) Robot?", "id": "Jumlah Sumbu Gerak Independen." },
  { "en": "Apa Kepanjangan DOF (Degrees of Freedom)?", "id": "Derajat Kebebasan." },
  { "en": "Apa Itu Kinematika (Kinematics) Robot?", "id": "Studi Gerakan Robot Tanpa Gaya." },
  { "en": "Apa Itu Kinematika Maju (Forward Kinematics)?", "id": "Hitung Posisi End-Effector Dari Sudut." },
  { "en": "Apa Itu Kinematika Mundur (Inverse Kinematics)?", "id": "Hitung Sudut Sendi Dari Posisi." },
  { "en": "Apa Itu End-Effector (End-Effector) Robot?", "id": "Alat Di Ujung Lengan Robot." },
  { "en": "Apa Itu Ruang Kerja (Workspace) Robot?", "id": "Area Jangkauan End-Effector Robot." },
  { "en": "Apa Itu Sambungan Sendi (Joint) Robot?", "id": "Bagian Bergerak Antar Lengan Robot." },
  { "en": "Apa Itu Sambungan Prismatic (Prismatic Joint)?", "id": "Sambungan Gerak Lurus (Translasi)." },
  { "en": "Apa Itu Sambungan Revolute (Revolute Joint)?", "id": "Sambungan Gerak Putar (Rotasi)." },
  { "en": "Apa Kepanjangan MSI (Medium Scale Integration)?", "id": "Integrasi Skala Menengah." },
  { "en": "Apa Kepanjangan LSI (Large Scale Integration)?", "id": "Integrasi Skala Besar." },
  { "en": "Apa Kepanjangan VLSI (Very Large Scale Integration)?", "id": "Integrasi Skala Sangat Besar." },
  { "en": "Apa Kepanjangan ULSI (Ultra Large Scale Integration)?", "id": "Integrasi Skala Ultra Besar." },
  { "en": "Berapa Gerbang Logika Pada Chip MSI (Medium Scale Integration)?", "id": "10 Hingga 100 Gerbang." },
  { "en": "Berapa Gerbang Logika Pada Chip LSI (Large Scale Integration)?", "id": "100 Hingga 10.000 Gerbang." },
  { "en": "Berapa Gerbang Logika Pada Chip VLSI (Very Large Scale Integration)?", "id": "Di Atas 10.000 Gerbang." },
  { "en": "Contoh Chip LSI (Large Scale Integration)?", "id": "Mikroprosesor Awal." },
  { "en": "Contoh Chip VLSI (Very Large Scale Integration)?", "id": "CPU (Central Processing Unit) Modern." },
  { "en": "Apa Itu Level Fermi (Fermi Level)?", "id": "Tingkat Energi (Probabilitas Elektron 50%)." },
  { "en": "Dimana Level Fermi (Fermi Level) Semikonduktor Intrinsik?", "id": "Di Tengah Celah Energi." },
  { "en": "Dimana Level Fermi (Fermi Level) Semikonduktor Tipe-N?", "id": "Dekat Pita Konduksi." },
  { "en": "Dimana Level Fermi (Fermi Level) Semikonduktor Tipe-P?", "id": "Dekat Pita Valensi." },
  { "en": "Apa Itu Mobilitas (Mobility) Pembawa Muatan?", "id": "Kemudahan Muatan Bergerak Medan Listrik." },
  { "en": "Mana Mobilitas (Mobility) Lebih Tinggi, Elektron Atau Hole?", "id": "Elektron Memiliki Mobilitas Lebih Tinggi." },
  { "en": "Apa Itu Arus Saturasi Balik (Reverse Saturation Current)?", "id": "Arus Bocor Kecil Dioda Mundur." },
  { "en": "Apa Itu UPS (Uninterruptible Power Supply) Offline?", "id": "UPS (Uninterruptible Power Supply) Standby." },
  { "en": "Apa Itu UPS (Uninterruptible Power Supply) Line-Interactive?", "id": "UPS (Uninterruptible Power Supply) Dengan AVR (Automatic Voltage Regulator)." },
  { "en": "Apa Itu UPS (Uninterruptible Power Supply) Online (Double Conversion)?", "id": "Konversi AC-DC Lalu DC-AC Terus Menerus." },
  { "en": "UPS (Uninterruptible Power Supply) Mana Yang Proteksinya Terbaik?", "id": "UPS (Uninterruptible Power Supply) Online." },
  { "en": "Apa Itu Waktu Transfer (Transfer Time) UPS?", "id": "Waktu Pindah Mode Baterai." },
  { "en": "Waktu Transfer (Transfer Time) UPS (Uninterruptible Power Supply) Online?", "id": "Nol Milidetik (Zero Transfer Time)." },
  { "en": "Apa Bentuk Gelombang Output UPS (Uninterruptible Power Supply) Murah?", "id": "Kotak Modifikasi (Modified Sine Wave)." },
  { "en": "Apa Bentuk Gelombang Output UPS (Uninterruptible Power Supply) Bagus?", "id": "Sinus Murni (Pure Sine Wave)." },
  { "en": "Apa Itu Kategori Arc Flash (Arc Flash Category) 0?", "id": "Risiko Sangat Rendah (PPE Dasar)." },
  { "en": "Apa Itu Batas Arc Flash (Arc Flash Boundary)?", "id": "Jarak Aman Minimal Dari Peralatan." },
  { "en": "Apa Itu Energi Insiden (Incident Energy)?", "id": "Energi Panas (Kalori/cm²) Arc Flash." },
  { "en": "Apa Kepanjangan NFPA (National Fire Protection Association) 70E?", "id": "Standar Keselamatan Listrik Tempat Kerja." },
  { "en": "Apa Itu Konvolusi (Convolution) Sinyal?", "id": "Operasi Matematis (Respon Sistem LTI)." },
  { "en": "Apa Simbol Operasi Konvolusi (Convolution)?", "id": "Tanda Bintang (Asterisk *)." },
  { "en": "Apa Itu Sinyal Impuls Unit (Unit Impulse Signal)?", "id": "Sinyal Sesaat (Lebar Nol, Area Satu)." },
  { "en": "Apa Itu Sinyal Langkah Unit (Unit Step Signal)?", "id": "Sinyal Bernilai Satu (t >= 0)." },
  { "en": "Apa Itu Respon Impuls (Impulse Response) Sistem?", "id": "Output Sistem Jika Input Impuls." },
  { "en": "Apa Itu Respon Langkah (Step Response) Sistem?", "id": "Output Sistem Jika Input Langkah." },
  { "en": "Apa Itu Sistem Kausal (Causal System)?", "id": "Output Tergantung Input Sekarang, Masa Lalu." },
  { "en": "Apa Itu Sistem Non-Kausal (Non-Causal System)?", "id": "Output Tergantung Input Masa Depan." },
  { "en": "Apa Itu Transformasi Fourier (Fourier Transform) Sinyal?", "id": "Mengubah Sinyal Domain Waktu Ke Frekuensi." },
  { "en": "Apa Itu Transformasi Laplace (Laplace Transform) Sinyal?", "id": "Generalisasi Transformasi Fourier (Analisis Sistem)." },
  { "en": "Apa Itu Bidang-s (s-Plane) Analisis Laplace?", "id": "Bidang Kompleks (Sigma + j-Omega)." },
  { "en": "Apa Itu Daerah Konvergensi (Region of Convergence) (ROC)?", "id": "Daerah Bidang-s Tempat Transformasi Ada." },
  { "en": "Apa Itu Filter Digital (Digital Filter)?", "id": "Filter Diterapkan Pada Sinyal Digital." },
  { "en": "Apa Kepanjangan FIR (Finite Impulse Response) Filter?", "id": "Filter Respon Impuls Terbatas." },
  { "en": "Apa Kepanjangan IIR (Infinite Impulse Response) Filter?", "id": "Filter Respon Impuls Tak Terbatas." },
  { "en": "Apa Keunggulan Filter FIR (Finite Impulse Response)?", "id": "Selalu Stabil, Fasa Linear." },
  { "en": "Apa Keunggulan Filter IIR (Infinite Impulse Response)?", "id": "Jauh Lebih Efisien (Komputasi Ringan)." },
  { "en": "Apa Itu Kuantisasi (Quantization) Sinyal Digital?", "id": "Pembulatan Amplitudo Sinyal." },
  { "en": "Apa Itu Kesalahan Kuantisasi (Quantization Error)?", "id": "Selisih Sinyal Asli, Hasil Kuantisasi." },
  { "en": "Apa Itu Dithering (Dithering)?", "id": "Penambahan Noise Acak Kurangi Error Kuantisasi." },
  { "en": "Apa Itu Protokol SPI (Serial Peripheral Interface)?", "id": "Protokol Komunikasi Serial Sinkron." },
  { "en": "Berapa Kawat Minimal Bus SPI (Serial Peripheral Interface)?", "id": "Empat Kawat (MISO, MOSI, SCK, SS)." },
  { "en": "Apa Kepanjangan MISO (Master In Slave Out)?", "id": "Data Masuk Master Dari Slave." },
  { "en": "Apa Kepanjangan MOSI (Master Out Slave In)?", "id": "Data Keluar Master Ke Slave." },
  { "en": "Apa Kepanjangan SCK (Serial Clock) SPI?", "id": "Sinyal Clock (Detak) Serial." },
  { "en": "Apa Kepanjangan SS (Slave Select) SPI?", "id": "Sinyal Memilih Perangkat Slave." },
  { "en": "Apa Itu Protokol I2C (Inter-Integrated Circuit)?", "id": "Protokol Komunikasi Serial Dua Kawat." },
  { "en": "Apa Dua Kawat Bus I2C (Inter-Integrated Circuit)?", "id": "SDA (Serial Data), SCL (Serial Clock)." },
  { "en": "Bagaimana Bus I2C (Inter-Integrated Circuit) Mengatasi Tabrakan?", "id": "Deteksi Tabrakan, Arbitrasi Bus." },
  { "en": "Apa Itu Alamat (Address) I2C (Inter-Integrated Circuit)?", "id": "Alamat Unik Perangkat Slave." },
  { "en": "Apa Itu Sinyal Start (Start Condition) I2C?", "id": "Transisi High Ke Low SDA (SCL High)." },
  { "en": "Apa Itu Sinyal Stop (Stop Condition) I2C?", "id": "Transisi Low Ke High SDA (SCL High)." },
  { "en": "Apa Itu Bit ACK (Acknowledge) I2C?", "id": "Bit Konfirmasi Penerimaan Data." },
  { "en": "Apa Itu Bit NACK (Not Acknowledge) I2C?", "id": "Bit Penolakan Penerimaan Data." },
  { "en": "Apa Itu Penguat Antena (Antenna Amplifier)?", "id": "Penguat Sinyal Lemah Dari Antena." },
  { "en": "Apa Nama Lain Penguat Antena (Antenna Amplifier)?", "id": "Booster (Penguat)." },
  { "en": "Apa Kepanjangan LNA (Low Noise Amplifier)?", "id": "Penguat Derau Rendah." },
  { "en": "Dimana LNA (Low Noise Amplifier) Ditempatkan?", "id": "Sangat Dekat Dengan Antena Penerima." },
  { "en": "Mengapa LNA (Low Noise Amplifier) Harus Dekat Antena?", "id": "Menguatkan Sinyal Sebelum Noise Kabel." },
  { "en": "Apa Itu Angka Derau (Noise Figure) (NF)?", "id": "Ukuran Degradasi SNR (Signal-to-Noise Ratio) Penguat." },
  { "en": "Apakah Angka Derau (Noise Figure) (NF) Rendah Baik?", "id": "Ya, NF Rendah Berarti Kualitas Baik." },
  { "en": "Apa Itu Penguat Daya (Power Amplifier) (PA)?", "id": "Penguat Sinyal Besar (Tahap Akhir Pemancar)." },
  { "en": "Apa Kepanjangan PA (Power Amplifier)?", "id": "Penguat Daya." },
  { "en": "Apa Itu Titik Kompresi 1-dB (1-dB Compression Point)?", "id": "Titik Awal Non-Linearitas Penguat." },
  { "en": "Apa Itu Intersept Poin (Intercept Point) (IP3)?", "id": "Ukuran Linearitas Penguat (Intermodulasi)." },
  { "en": "Apa Itu Efek Doppler (Doppler Effect)?", "id": "Pergeseran Frekuensi Akibat Gerakan Relatif." },
  { "en": "Dimana Efek Doppler (Doppler Effect) Digunakan?", "id": "Radar Kecepatan, Komunikasi Satelit." },
  { "en": "Apa Itu Radar (Radio Detection and Ranging)?", "id": "Sistem Deteksi Objek Gelombang Radio." },
  { "en": "Apa Itu Lidar (Light Detection and Ranging)?", "id": "Sistem Deteksi Objek Menggunakan Laser." },
  { "en": "Apa Itu Sonar (Sound Navigation and Ranging)?", "id": "Sistem Deteksi Objek Gelombang Suara." },
  { "en": "Apa Itu Komunikasi Optik (Optical Communication)?", "id": "Komunikasi Menggunakan Cahaya (Serat Optik)." },
  { "en": "Apa Itu Komunikasi Nirkabel (Wireless Communication)?", "id": "Komunikasi Tanpa Media Kabel." },
  { "en": "Apa Itu Jaringan Ad-Hoc (Ad-Hoc Network)?", "id": "Jaringan Nirkabel Spontan (Tanpa Infrastruktur)." },
  { "en": "Apa Itu Jaringan Mesh (Mesh Network)?", "id": "Jaringan (Setiap Node Terhubung Banyak Node)." },
  { "en": "Apa Itu RFID (Radio Frequency Identification)?", "id": "Identifikasi Frekuensi Radio (Tag, Reader)." },
  { "en": "Apa Kepanjangan RFID (Radio Frequency Identification)?", "id": "Identifikasi Frekuensi Radio." },
  { "en": "Apa Itu Tag RFID (Radio Frequency Identification) Pasif?", "id": "Tag Tanpa Baterai (Daya Dari Reader)." },
  { "en": "Apa Itu Tag RFID (Radio Frequency Identification) Aktif?", "id": "Tag Memiliki Baterai Internal." },
  { "en": "Apa Kepanjangan NFC (Near Field Communication)?", "id": "Komunikasi Medan Dekat." },
  { "en": "Apa Basis Teknologi NFC (Near Field Communication)?", "id": "Teknologi RFID (Radio Frequency Identification) Jarak Sangat Pendek." },
  { "en": "Dimana NFC (Near Field Communication) Digunakan?", "id": "Pembayaran Nirkontak (E-Money)." },
  { "en": "Apa Itu Zigbee (Zigbee)?", "id": "Protokol Nirkabel (Daya Rendah, Jaringan Mesh)." },
  { "en": "Apa Itu Zigbee (Zigbee) Untuk Aplikasi Apa?", "id": "IoT (Internet of Things), Kontrol Rumah." },
  { "en": "Apa Itu Bluetooth (Bluetooth) Low Energy (BLE)?", "id": "Versi Bluetooth (Bluetooth) Hemat Daya." },
  { "en": "Apa Kepanjangan BLE (Bluetooth Low Energy)?", "id": "Energi Rendah Bluetooth." },
  { "en": "Apa Itu LoRa (Long Range) Komunikasi?", "id": "Teknologi Nirkabel Jarak Jauh." },
  { "en": "Apa Kepanjangan LoRaWAN (Long Range Wide Area Network)?", "id": "Jaringan Area Luas Jarak Jauh." },
  { "en": "Apa Itu Sistem RTOS (Real-Time Operating System)?", "id": "Sistem Operasi Waktu Nyata." },
  { "en": "Apa Kepanjangan RTOS (Real-Time Operating System)?", "id": "Sistem Operasi Waktu Nyata." },
  { "en": "Kapan RTOS (Real-Time Operating System) Dibutuhkan?", "id": "Saat Respon Waktu Sangat Kritis." },
  { "en": "Apa Itu Task (Tugas) Dalam RTOS (Real-Time Operating System)?", "id": "Bagian Program Independen." },
  { "en": "Apa Itu Scheduler (Penjadwal) RTOS (Real-Time Operating System)?", "id": "Komponen Pengatur Eksekusi Task." },
  { "en": "Apa Itu Prioritas (Priority) Task RTOS (Real-Time Operating System)?", "id": "Tingkat Kepentingan Eksekusi Task." },
  { "en": "Apa Itu Semaphore (Semaphore) RTOS (Real-Time Operating System)?", "id": "Objek Sinkronisasi Antar Task." },
  { "en": "Apa Itu Mutex (Mutex) RTOS (Real-Time Operating System)?", "id": "Mutual Exclusion (Pengaman Sumber Daya)." },
  { "en": "Apa Itu Antrian Pesan (Message Queue) RTOS?", "id": "Media Komunikasi Data Antar Task." },
  { "en": "Apa Itu Jitter (Jitter) Dalam RTOS (Real-Time Operating System)?", "id": "Variasi Waktu Eksekusi Task Periodik." },
  { "en": "Apa Itu Robotika (Robotics)?", "id": "Ilmu Teknologi Desain, Operasi Robot." },
  { "en": "Apa Itu Robot (Robot) Industri?", "id": "Robot Manipulator Otomatisasi Pabrik." },
  { "en": "Apa Itu Robot (Robot) Bergerak (Mobile Robot)?", "id": "Robot Dapat Berpindah Tempat." },
  { "en": "Apa Itu Robot (Robot) Humanoid?", "id": "Robot Berbentuk Menyerupai Manusia." },
  { "en": "Apa Itu Aktuator (Actuator) Robot?", "id": "Komponen Penggerak (Motor, Pneumatik)." },
  { "en": "Apa Itu Sensor (Sensor) Robot?", "id": "Komponen Input (Kamera, Lidar, Sentuh)." },
  { "en": "Apa Itu Kontroler (Controller) Robot?", "id": "Otak Robot (Pemroses Perintah Gerak)." },
  { "en": "Apa Itu Dinamika (Dynamics) Robot?", "id": "Studi Gerakan Robot Memperhitungkan Gaya." },
  { "en": "Apa Itu Perencanaan Gerak (Motion Planning) Robot?", "id": "Algoritma Penentu Jalur Gerak Robot." },
  { "en": "Apa Itu Visi Komputer (Computer Vision)?", "id": "Kemampuan Komputer 'Melihat' Dunia." },
  { "en": "Bagaimana Visi Komputer (Computer Vision) Membantu Robot?", "id": "Navigasi, Pengenalan Objek, Inspeksi." },
  { "en": "Apa Kepanjangan SLAM (Simultaneous Localization and Mapping)?", "id": "Lokalisasi, Pemetaan Simultan." },
  { "en": "Apa Fungsi SLAM (Simultaneous Localization and Mapping) Robot?", "id": "Robot Membangun Peta, Mengetahui Lokasi." },
  { "en": "Apa Itu Proprioceptive (Proprioceptive) Sensor?", "id": "Sensor Internal Robot (Posisi Sendi)." },
  { "en": "Apa Itu Exteroceptive (Exteroceptive) Sensor?", "id": "Sensor Eksternal Robot (Lingkungan)." },
  { "en": "Apa Itu Derau Putih (White Noise)?", "id": "Noise Densitas Spektral Daya Rata." },
  { "en": "Apa Itu Derau Merah Jambu (Pink Noise)?", "id": "Noise (Energi Sama Per Oktaf)." },
  { "en": "Apa Nama Lain Derau Merah Jambu (Pink Noise)?", "id": "Derau 1/f (Flicker Noise)." },
  { "en": "Apa Itu Kalang Fasa Terkunci (PLL)?", "id": "Phase-Locked Loop (PLL)." },
  { "en": "Apa Fungsi Kalang Fasa Terkunci (PLL)?", "id": "Sintesis Frekuensi, Demodulasi FM." },
  { "en": "Apa Tiga Komponen Utama PLL (Phase-Locked Loop)?", "id": "Detektor Fasa, Filter, VCO." },
  { "en": "Apa Itu Detektor Fasa (Phase Detector)?", "id": "Membandingkan Fasa Input, Fasa Output." },
  { "en": "Apa Itu Osilator Terkendali Tegangan (VCO)?", "id": "Voltage-Controlled Oscillator (VCO)." },
  { "en": "Apa Itu Keadaan Terkunci (Locked State) PLL?", "id": "Frekuensi Output Sama Input (Terkunci)." },
  { "en": "Apa Itu Rentang Tangkap (Capture Range) PLL?", "id": "Rentang Frekuensi Input Dapat Dikunci." },
  { "en": "Apa Itu Rentang Kunci (Lock Range) PLL?", "id": "Rentang Frekuensi Input Tetap Terkunci." },
  { "en": "Apa Itu Prescaler (Prescaler) Frekuensi?", "id": "Pembagi Frekuensi Digital (Input PLL)." },
  { "en": "Apa Itu Sintesiser Frekuensi (Frequency Synthesizer)?", "id": "Rangkaian Penghasil Banyak Frekuensi Stabil." },
  { "en": "Apa Itu Penguat Distribusi (Distributed Amplifier)?", "id": "Penguat Bandwidth Sangat Lebar (RF)." },
  { "en": "Apa Itu Penguat Umpan Balik Arus (Current Feedback)?", "id": "Topologi Op-Amp (Slew Rate Tinggi)." },
  { "en": "Apa Itu Penguat Umpan Balik Tegangan (Voltage Feedback)?", "id": "Topologi Op-Amp Paling Umum." },
  { "en": "Apa Itu Dioda Step Recovery (Step Recovery Diode)?", "id": "Dioda Penghasil Pulsa Sangat Cepat." },
  { "en": "Apa Itu Dioda Backward (Backward Diode)?", "id": "Jenis Dioda Tunnel (Detektor RF)." },
  { "en": "Apa Itu Dioda IMPATT (IMPATT Diode)?", "id": "Dioda Penghasil Daya Gelombang Mikro." },
  { "en": "Apa Itu Dioda Gunn (Gunn Diode)?", "id": "Dioda Penghasil Daya Gelombang Mikro." },
  { "en": "Apa Itu Parameter-S (S-Parameters) Jaringan RF?", "id": "Parameter Jaringan Frekuensi Tinggi (Hamburan)." },
  { "en": "Apa Itu S11 (S11) Parameter-S?", "id": "Koefisien Refleksi Input." },
  { "en": "Apa Itu S21 (S21) Parameter-S?", "id": "Penguatan (Gain) Maju." },
  { "en": "Apa Itu S12 (S12) Parameter-S?", "id": "Isolasi (Isolation) Mundur." },
  { "en": "Apa Itu S22 (S22) Parameter-S?", "id": "Koefisien Refleksi Output." },
  { "en": "Apa Kepanjangan VNA (Vector Network Analyzer)?", "id": "Penganalisis Jaringan Vektor." },
  { "en": "Apa Fungsi VNA (Vector Network Analyzer)?", "id": "Mengukur Parameter-S (S-Parameters) Komponen RF." },
  { "en": "Apa Itu Penguat Terdistel (Tuned Amplifier)?", "id": "Penguat Resonansi Frekuensi Tertentu." },
  { "en": "Apa Itu Resonator Keramik (Ceramic Resonator)?", "id": "Komponen Resonansi (Lebih Murah Kristal)." },
  { "en": "Apa Kepanjangan SAW (Surface Acoustic Wave) Filter?", "id": "Filter Gelombang Akustik Permukaan." },
  { "en": "Dimana Filter SAW (Surface Acoustic Wave) Digunakan?", "id": "Filter IF (Intermediate Frequency) Handphone." },
  { "en": "Apa Itu Kopler Arah (Directional Coupler)?", "id": "Komponen RF (Mencuplik Sinyal Satu Arah)." },
  { "en": "Apa Itu Sirkulator (Circulator) RF?", "id": "Komponen Tiga Port (Aliran Sinyal Satu Arah)." },
  { "en": "Apa Itu Isolator (Isolator) RF?", "id": "Komponen Dua Port (Lewatkan Sinyal Satu Arah)." },
  { "en": "Apa Fungsi Isolator (Isolator) RF?", "id": "Melindungi Sumber Sinyal Dari Refleksi." },
  { "en": "Apa Itu Duplexer (Duplexer) RF?", "id": "Memisahkan Sinyal Transmit (TX), Receive (RX)." },
  { "en": "Apa Itu Diplexer (Diplexer) RF?", "id": "Memisahkan Sinyal Berdasarkan Frekuensi." },
  { "en": "Apa Itu Atenuator (Attenuator) RF?", "id": "Komponen Pasif Pelemahan Sinyal RF." },
  { "en": "Apa Itu Saklar RF (RF Switch)?", "id": "Saklar Elektronik (PIN, GaAs) Sinyal RF." },
  { "en": "Apa Kepanjangan GaAs (Gallium Arsenide)?", "id": "Galium Arsenida (Semikonduktor RF)." },
  { "en": "Apa Kepanjangan GaN (Gallium Nitride)?", "id": "Galium Nitrida (Semikonduktor Daya RF)." },
  { "en": "Apa Keunggulan GaN (Gallium Nitride) HEMT?", "id": "Efisiensi, Daya Tinggi Frekuensi Tinggi." },
  { "en": "Apa Kepanjangan HEMT (High Electron Mobility Transistor)?", "id": "Transistor Mobilitas Elektron Tinggi." },
  { "en": "Apa Itu MMIC (Monolithic Microwave Integrated Circuit)?", "id": "IC (Integrated Circuit) Gelombang Mikro Monolitik." },
  { "en": "Apa Kepanjangan QAM (Quadrature Amplitude Modulation)?", "id": "Modulasi Amplitudo Kuadratur." },
  { "en": "Apa Itu Diagram Konstelasi (Constellation Diagram)?", "id": "Representasi Grafis Sinyal Modulasi Digital." },
  { "en": "Apa Kepanjangan OFDM (Orthogonal Frequency Division Multiplexing)?", "id": "Multipleksasi Pembagian Frekuensi Ortogonal." },
  { "en": "Dimana OFDM (Orthogonal Frequency Division Multiplexing) Digunakan?", "id": "Wi-Fi (Wireless Fidelity), 4G/5G LTE, DVB-T2." },
  { "en": "Apa Keunggulan OFDM (Orthogonal Frequency Division Multiplexing)?", "id": "Tahan Terhadap Multipath Fading." },
  { "en": "Apa Itu Multipath Fading (Redaman Multipath)?", "id": "Sinyal Diterima Dari Banyak Jalur." },
  { "en": "Apa Itu MIMO (Multiple Input Multiple Output)?", "id": "Banyak Input Banyak Output." },
  { "en": "Apa Fungsi MIMO (Multiple Input Multiple Output)?", "id": "Meningkatkan Kecepatan, Keandalan Nirkabel." },
  { "en": "Apa Itu Beamforming (Pembentukan Berkas)?", "id": "Mengonsentrasikan Sinyal Antena Arah Tertentu." },
  { "en": "Apa Itu Antena Phased Array (Phased Array)?", "id": "Array Antena (Arah Dikontrol Elektronik)." },
  { "en": "Apa Kepanjangan BER (Bit Error Rate)?", "id": "Laju Kesalahan Bit." },
  { "en": "Apa Itu BER (Bit Error Rate)?", "id": "Ukuran Kualitas Transmisi Digital." },
  { "en": "Apa Itu Channel Coding (Pengkodean Kanal)?", "id": "Menambah Bit Redundansi Deteksi Error." },
  { "en": "Apa Itu Kode Turbo (Turbo Codes)?", "id": "Jenis Kode Koreksi Kesalahan Kuat." },
  { "en": "Apa Itu Kode LDPC (Low-Density Parity-Check)?", "id": "Kode Koreksi Kesalahan Efisiensi Tinggi." },
  { "en": "Apa Itu Antena Cerdas (Smart Antenna)?", "id": "Antena Dengan Pemrosesan Sinyal (Beamforming)." },
  { "en": "Apa Itu Radio Kognitif (Cognitive Radio)?", "id": "Radio Cerdas (Adaptasi Spektrum Frekuensi)." },
  { "en": "Apa Itu Radio Perangkat Lunak (Software Defined Radio)?", "id": "SDR (Software Defined Radio)." },
  { "en": "Apa Kepanjangan SDR (Software Defined Radio)?", "id": "Radio Didefinisikan Perangkat Lunak." },
  { "en": "Apa Keuntungan SDR (Software Defined Radio)?", "id": "Sangat Fleksibel (Fungsi Diubah Software)." },
  { "en": "Apa Itu Spektrum Tersebar (Spread Spectrum)?", "id": "Teknik Modulasi (Sinyal Disebar)." },
  { "en": "Sebutkan Dua Tipe Spektrum Tersebar (Spread Spectrum)?", "id": "DSSS (Direct Sequence Spread Spectrum), FHSS (Frequency Hopping Spread Spectrum)." },
  { "en": "Apa Kepanjangan DSSS (Direct Sequence Spread Spectrum)?", "id": "Spektrum Tersebar Urutan Langsung." },
  { "en": "Apa Kepanjangan FHSS (Frequency Hopping Spread Spectrum)?", "id": "Spektrum Tersebar Lompatan Frekuensi." },
  { "en": "Dimana FHSS (Frequency Hopping Spread Spectrum) Digunakan?", "id": "Bluetooth (Bluetooth) Versi Awal." },
  { "en": "Apa Itu Interleaving (Interleaving)?", "id": "Mengacak Urutan Bit (Melawan Burst Error)." },
  { "en": "Apa Itu Burst Error (Kesalahan Beruntun)?", "id": "Kesalahan Bit Terjadi Berkelompok." },
  { "en": "Apa Itu Elektronika Daya (Power Electronics)?", "id": "Aplikasi Elektronika Kontrol Konversi Daya." },
  { "en": "Apa Empat Jenis Konversi Elektronika Daya?", "id": "AC-DC, DC-DC, DC-AC, AC-AC." },
  { "en": "Apa Nama Konverter AC-DC (AC-DC Converter)?", "id": "Penyearah (Rectifier)." },
  { "en": "Apa Nama Konverter DC-AC (DC-AC Converter)?", "id": "Inverter (Inverter)." },
  { "en": "Apa Nama Konverter DC-DC (DC-DC Converter)?", "id": "Chopper (Chopper) Atau Regulator Switching." },
  { "en": "Apa Nama Konverter AC-AC (AC-AC Converter)?", "id": "Kontroler Tegangan AC, Cycloconverter." },
  { "en": "Apa Itu Penyearah Terkendali (Controlled Rectifier)?", "id": "Penyearah Menggunakan Thyristor (SCR)." },
  { "en": "Apa Itu Penyearah Tak Terkendali (Uncontrolled Rectifier)?", "id": "Penyearah Hanya Menggunakan Dioda." },
  { "en": "Apa Itu Sudut Penyulutan (Firing Angle) SCR?", "id": "Sudut Tunda SCR (Silicon Controlled Rectifier) Mulai On." },
  { "en": "Apa Itu Sudut Pemadaman (Extinction Angle) SCR?", "id": "Sudut Saat Arus SCR (Silicon Controlled Rectifier) Menjadi Nol." },
  { "en": "Apa Itu Penyearah Enam Pulsa (Six-Pulse Rectifier)?", "id": "Penyearah Tiga Fasa Gelombang Penuh." },
  { "en": "Apa Itu Penyearah Dua Belas Pulsa (Twelve-Pulse Rectifier)?", "id": "Dua Penyearah Enam Pulsa Paralel." },
  { "en": "Apa Keuntungan Penyearah Dua Belas Pulsa (Twelve-Pulse Rectifier)?", "id": "Mengurangi Riak (Ripple), Harmonisa." },
  { "en": "Apa Itu Mode Konduksi Kontinu (Continuous Conduction Mode)?", "id": "CCM (Continuous Conduction Mode), Arus Induktor Kontinu." },
  { "en": "Apa Kepanjangan CCM (Continuous Conduction Mode)?", "id": "Mode Konduksi Berkelanjutan." },
  { "en": "Apa Itu Mode Konduksi Diskontinu (Discontinuous Conduction Mode)?", "id": "DCM (Discontinuous Conduction Mode), Arus Induktor Nol." },
  { "en": "Apa Kepanjangan DCM (Discontinuous Conduction Mode)?", "id": "Mode Konduksi Tidak Berkelanjutan." },
  { "en": "Apa Itu Topologi Isolasi (Isolated Topology) SMPS?", "id": "Input, Output Terisolasi Listrik (Trafo)." },
  { "en": "Apa Itu Topologi Non-Isolasi (Non-Isolated Topology) SMPS?", "id": "Input, Output Terhubung Ground Sama." },
  { "en": "Sebutkan Contoh Topologi Non-Isolasi (Non-Isolated Topology)?", "id": "Buck, Boost, Buck-Boost." },
  { "en": "Sebutkan Contoh Topologi Isolasi (Isolated Topology)?", "id": "Flyback, Forward, Push-Pull, Jembatan." },
  { "en": "Apa Itu Konverter Forward (Forward Converter)?", "id": "Konverter Isolasi (Energi Ditransfer Langsung)." },
  { "en": "Apa Itu Konverter Flyback (Flyback Converter)?", "id": "Konverter Isolasi (Energi Disimpan Induktor)." },
  { "en": "Apa Itu Konverter Push-Pull (Push-Pull Converter)?", "id": "Konverter Isolasi (Dua Saklar, Trafo CT)." },
  { "en": "Apa Itu Konverter Jembatan Penuh (Full-Bridge Converter)?", "id": "Konverter Isolasi (Empat Saklar)." },
  { "en": "Apa Itu Konverter Jembatan Setengah (Half-Bridge Converter)?", "id": "Konverter Isolasi (Dua Saklar, Dua Kapasitor)." },
  { "en": "Apa Itu Inverter Sumber Tegangan (Voltage Source Inverter)?", "id": "VSI (Voltage Source Inverter), Input Sumber Tegangan DC." },
  { "en": "Apa Kepanjangan VSI (Voltage Source Inverter)?", "id": "Inverter Sumber Tegangan." },
  { "en": "Apa Itu Inverter Sumber Arus (Current Source Inverter)?", "id": "CSI (Current Source Inverter), Input Sumber Arus DC." },
  { "en": "Apa Kepanjangan CSI (Current Source Inverter)?", "id": "Inverter Sumber Arus." },
  { "en": "Apa Itu PWM (Pulse Width Modulation) Sinusoidal (SPWM)?", "id": "Teknik PWM (Pulse Width Modulation) Hasilkan Sinus." },
  { "en": "Apa Itu Kontrol Vektor (Vector Control) Drive?", "id": "Kontrol Motor AC Kinerja Tinggi." },
  { "en": "Apa Nama Lain Kontrol Vektor (Vector Control)?", "id": "Field-Oriented Control (FOC)." },
  { "en": "Apa Itu Kontrol Skalar (Scalar Control) Drive?", "id": "Kontrol Motor AC Sederhana (V/f)." },
  { "en": "Apa Kepanjangan V/f (V/f Control)?", "id": "Kontrol Tegangan Per Frekuensi." },
  { "en": "Apa Itu Pengereman DC (DC Injection Braking)?", "id": "Menghentikan Motor AC Arus DC." },
  { "en": "Apa Itu Pemanasan Induksi (Induction Heating)?", "id": "Pemanasan Logam Arus Eddy Frekuensi Tinggi." },
  { "en": "Apa Itu Penggerak Motor (Motor Drive)?", "id": "Sistem Kontroler Elektronika Daya Motor." },
  { "en": "Apa Itu Komponen Pasif (Passive Component)?", "id": "Komponen Tidak Butuh Daya Eksternal." },
  { "en": "Apa Itu Komponen Aktif (Active Component)?", "id": "Komponen Butuh Daya Eksternal (Penguatan)." },
  { "en": "Sebutkan Contoh Komponen Pasif (Passive Component)?", "id": "Resistor, Kapasitor, Induktor." },
  { "en": "Sebutkan Contoh Komponen Aktif (Active Component)?", "id": "Transistor, Dioda, Op-Amp." },
  { "en": "Apa Itu Bilangan Kompleks (Complex Number)?", "id": "Bilangan Bagian Riil, Imajiner." },
  { "en": "Apa Itu Bagian Riil (Real Part) Bilangan Kompleks?", "id": "Bagian Sumbu Horizontal (Resistif)." },
  { "en": "Apa Itu Bagian Imajiner (Imaginary Part) Bilangan Kompleks?", "id": "Bagian Sumbu Vertikal (Reaktif)." },
  { "en": "Apa Simbol Bagian Imajiner (Imaginary Part)?", "id": "Simbol 'j' (Teknik Elektro)." },
  { "en": "Apa Itu Bentuk Polar (Polar Form) Bilangan Kompleks?", "id": "Representasi (Magnitudo, Sudut Fasa)." },
  { "en": "Apa Itu Bentuk Rectangular (Rectangular Form) Bilangan Kompleks?", "id": "Representasi (Riil + j Imajiner)." },
  { "en": "Apa Itu Konjugat Kompleks (Complex Conjugate)?", "id": "Mengubah Tanda Bagian Imajiner." },
  { "en": "Apa Itu Diagram Argand (Argand Diagram)?", "id": "Diagram Visualisasi Bilangan Kompleks." },
  { "en": "Apa Itu Sistem Tenaga Radial (Radial System)?", "id": "Sistem Distribusi Satu Arah." },
  { "en": "Apa Itu Sistem Tenaga Loop (Loop System)?", "id": "Sistem Distribusi Cincin (Dua Arah)." },
  { "en": "Apa Itu Sistem Tenaga Spindel (Spindle System)?", "id": "Sistem Jaringan Distribusi Kompleks." },
  { "en": "Apa Itu Jaringan Tegangan Rendah (JTR)?", "id": "Jaringan Distribusi Tegangan Rendah (220/380V)." },
  { "en": "Apa Itu Jaringan Tegangan Menengah (JTM)?", "id": "Jaringan Distribusi Tegangan Menengah (20kV)." },
  { "en": "Apa Itu Gardu Distribusi (Distribution Substation)?", "id": "Gardu Penurun Tegangan (JTM Ke JTR)." },
  { "en": "Apa Itu Trafo Tiang (Pole Mounted Transformer)?", "id": "Transformator Distribusi Di Tiang." },
  { "en": "Apa Itu Gardu Portal (Portal Substation)?", "id": "Gardu Tiang Dua Tiang." },
  { "en": "Apa Itu Gardu Beton (Kiosk Substation)?", "id": "Gardu Distribusi Bangunan Beton." },
  { "en": "Apa Itu Sambungan Rumah (SR) (Service Drop)?", "id": "Kabel Sambungan JTR Ke Rumah." },
  { "en": "Apa Itu Alat Pengukur, Pembatas (APP)?", "id": "kWh Meter Dan MCB (Miniature Circuit Breaker) Pelanggan." },
  { "en": "Apa Itu Papan Hubung Bagi (PHB) Rumah?", "id": "Kotak Sekring (MCB Group) Dalam Rumah." },
  { "en": "Apa Itu Arus Mula (Inrush Current) Motor?", "id": "Arus Tinggi Saat Motor Start." },
  { "en": "Berapa Kali Arus Mula (Inrush Current) Motor?", "id": "Lima Hingga Delapan Kali Arus Nominal." },
  { "en": "Apa Itu Sistem Kontrol Terbuka (Open Loop)?", "id": "Sistem Kontrol Tanpa Umpan Balik." },
  { "en": "Apa Itu Sistem Kontrol Tertutup (Closed Loop)?", "id": "Sistem Kontrol Menggunakan Umpan Balik." },
  { "en": "Sebutkan Contoh Kontrol Terbuka (Open Loop)?", "id": "Kipas Angin (Kecepatan Diatur Saklar)." },
  { "en": "Sebutkan Contoh Kontrol Tertutup (Closed Loop)?", "id": "AC (Air Conditioner) (Kontrol Suhu Termostat)." },
  { "en": "Apa Itu Umpan Balik (Feedback)?", "id": "Sinyal Output Dikembalikan Ke Input." },
  { "en": "Apa Itu Setpoint (Setpoint) Sistem Kontrol?", "id": "Nilai Acuan Yang Diinginkan." },
  { "en": "Apa Itu Variabel Proses (Process Variable) (PV)?", "id": "Nilai Aktual Yang Diukur." },
  { "en": "Apa Itu Kesalahan (Error) Sistem Kontrol?", "id": "Selisih Antara Setpoint, Variabel Proses." },
  { "en": "Apa Itu Elemen Kontrol Final (Final Control Element)?", "id": "Aktuator Pengubah Proses (Katup, Pemanas)." },
  { "en": "Apa Itu Waktu Mati (Dead Time) Sistem?", "id": "Waktu Tunda Respon Awal Sistem." },
  { "en": "Apa Itu Osilasi (Oscillation) Sistem Kontrol?", "id": "Output Berayun Naik Turun Setpoint." },
  { "en": "Apa Itu Kestabilan (Stability) Sistem Kontrol?", "id": "Kemampuan Sistem Kembali Seimbang." },
  { "en": "Apa Itu Diagram Routh-Hurwitz (Routh-Hurwitz Criterion)?", "id": "Metode Analisis Kestabilan Matematis." },
  { "en": "Apa Itu Penalaan (Tuning) Kontroler?", "id": "Proses Optimasi Parameter Kontroler (PID)." },
  { "en": "Apa Itu Kontrol Adaptif (Adaptive Control)?", "id": "Kontroler Otomatis Sesuaikan Parameter." },
  { "en": "Apa Itu Kontrol Prediktif (Model Predictive Control)?", "id": "Kontrol Berbasis Model Prediksi Masa Depan." },
  { "en": "Apa Itu Batang Penangkal Petir (Lightning Rod)?", "id": "Terminal Udara (Air Terminal) Penangkal Petir." },
  { "en": "Apa Itu Konduktor Turun (Down Conductor)?", "id": "Kabel Penyalur Arus Petir Ke Tanah." },
  { "en": "Apa Itu Elektroda Bumi (Ground Electrode) Petir?", "id": "Sistem Pembumian Penangkal Petir." },
  { "en": "Apa Itu Sangkar Faraday (Faraday Cage) Proteksi Petir?", "id": "Proteksi Eksternal (Selubung Konduktif)." },
  { "en": "Apa Itu Proteksi Petir Internal (Internal Protection)?", "id": "Pelindung Surja (Surge Arrester)." },
  { "en": "Apa Kepanjangan SPD (Surge Protection Device)?", "id": "Perangkat Pelindung Surja." },
  { "en": "Dimana SPD (Surge Protection Device) Tipe 1 Dipasang?", "id": "Panel Listrik Utama (Proteksi Petir)." },
  { "en": "Dimana SPD (Surge Protection Device) Tipe 2 Dipasang?", "id": "Sub-Panel Distribusi (Proteksi Switching)." },
  { "en": "Dimana SPD (Surge Protection Device) Tipe 3 Dipasang?", "id": "Dekat Peralatan Sensitif (Stop Kontak)." },
  { "en": "Apa Itu Tegangan Surja (Surge Voltage)?", "id": "Lonjakan Tegangan Transien Sangat Cepat." },
  { "en": "Apa Penyebab Surja (Surge) Listrik?", "id": "Sambaran Petir Atau Switching Jaringan." },
  { "en": "Apa Itu MOV (Metal Oxide Varistor)?", "id": "Komponen Utama SPD (Surge Protection Device)." },
  { "en": "Bagaimana MOV (Metal Oxide Varistor) Bekerja?", "id": "Resistansi Tinggi (Normal), Rendah (Surja)." },
  { "en": "Apa Itu Tabung Gas (Gas Discharge Tube) (GDT)?", "id": "Komponen SPD (Surge Protection Device) (Tegangan Tinggi)." },
  { "en": "Apa Itu Isolasi Galvanik (Galvanic Isolation)?", "id": "Memisahkan Dua Sirkuit Tanpa Koneksi Listrik." },
  { "en": "Bagaimana Isolasi Galvanik (Galvanic Isolation) Dicapai?", "id": "Menggunakan Transformator Atau Optocoupler." },
  { "en": "Apa Itu Trafo Isolasi (Isolation Transformer)?", "id": "Transformator Rasio 1:1 Untuk Isolasi." },
  { "en": "Apa Fungsi Trafo Isolasi (Isolation Transformer)?", "id": "Mengisolasi Ground, Mengurangi Noise." },
  { "en": "Apa Itu Interferensi Mode Bersama (Common-Mode Interference)?", "id": "Noise Antara Saluran Dan Ground." },
  { "en": "Apa Itu Interferensi Mode Diferensial (Differential-Mode Interference)?", "id": "Noise Antara Dua Saluran (Fasa-Netral)." },
  { "en": "Apa Itu Filter EMI (Electromagnetic Interference)?", "id": "Rangkaian Penekan Interferensi Elektromagnetik." },
  { "en": "Apa Komponen Filter EMI (Electromagnetic Interference)?", "id": "Kapasitor X, Kapasitor Y, Choke." },
  { "en": "Apa Itu Kapasitor X (X Capacitor)?", "id": "Kapasitor Filter Antara Fasa, Netral." },
  { "en": "Apa Itu Kapasitor Y (Y Capacitor)?", "id": "Kapasitor Filter Antara Fasa, Ground." },
  { "en": "Mengapa Kapasitor Y (Y Capacitor) Bernilai Kecil?", "id": "Membatasi Arus Bocor Ke Ground." },
  { "en": "Apa Itu Penguat Antide (Antilog Amplifier)?", "id": "Output Proporsional Antilogaritma Input." },
  { "en": "Apa Itu Penguat Audio (Audio Amplifier)?", "id": "Penguat Sinyal Frekuensi Audio." },
  { "en": "Apa Rentang Frekuensi Audio (Audio Frequency)?", "id": "20 Hertz Hingga 20 Kilohertz." },
  { "en": "Apa Itu Kabel Terpilin (Twisted Pair)?", "id": "Dua Kawat Dipilin Kurangi Interferensi." },
  { "en": "Mengapa Kabel Jaringan (Network Cable) Terpilin?", "id": "Mengurangi Crosstalk (Crosstalk) Antar Pasangan." },
  { "en": "Apa Itu Crosstalk (Crosstalk)?", "id": "Induksi Sinyal Antar Kabel Berdekatan." },
  { "en": "Apa Itu NEXT (Near-End Crosstalk)?", "id": "Crosstalk (Crosstalk) Diukur Di Ujung Pengirim." },
  { "en": "Apa Itu FEXT (Far-End Crosstalk)?", "id": "Crosstalk (Crosstalk) Diukur Di Ujung Penerima." },
  { "en": "Apa Itu Kabel Koaksial (Coaxial Cable)?", "id": "Kabel Konduktor Pusat Dikelilingi Pelindung." },
  { "en": "Sebutkan Dua Impedansi (Impedance) Umum Kabel Koaksial?", "id": "50 Ohm (Radio), 75 Ohm (TV)." },
  { "en": "Apa Itu Konektor BNC (Bayonet Neill-Concelman)?", "id": "Konektor RF (Frekuensi Radio) Tipe Bayonet." },
  { "en": "Apa Itu Konektor SMA (SubMiniature version A)?", "id": "Konektor RF (Frekuensi Radio) Ulir Kecil." },
  { "en": "Apa Itu Konektor N (N-Type Connector)?", "id": "Konektor RF (Frekuensi Radio) Tahan Cuaca." },
  { "en": "Apa Itu Konektor F (F-Type Connector)?", "id": "Konektor Koaksial Umum Untuk Televisi." },
  { "en": "Apa Itu Atenuasi (Attenuation) Kabel?", "id": "Pelemahan Sinyal Sepanjang Kabel." },
  { "en": "Apa Itu Rugi Balik (Return Loss)?", "id": "Ukuran Sinyal Pantulan Akibat Mismatch." },
  { "en": "Apa Itu Pembangkitan Listrik (Power Generation)?", "id": "Proses Pembangkitan Energi Listrik." },
  { "en": "Apa Itu Transmisi Listrik (Power Transmission)?", "id": "Penyaluran Listrik Jarak Jauh." },
  { "en": "Apa Itu Distribusi Listrik (Power Distribution)?", "id": "Pembagian Listrik Ke Pelanggan Akhir." },
  { "en": "Mengapa Transmisi (Transmission) Menggunakan Tegangan Tinggi?", "id": "Mengurangi Rugi-Rugi Daya (I²R)." },
  { "en": "Apa Itu Jaringan Cerdas (Smart Grid)?", "id": "Jaringan Listrik Modern Komunikasi Dua Arah." },
  { "en": "Apa Itu Meteran Cerdas (Smart Meter)?", "id": "Meteran Listrik Komunikasi Otomatis." },
  { "en": "Apa Kepanjangan AMR (Automatic Meter Reading)?", "id": "Pembacaan Meter Otomatis." },
  { "en": "Apa Kepanjangan AMI (Advanced Metering Infrastructure)?", "id": "Infrastruktur Pengukuran Tingkat Lanjut." },
  { "en": "Apa Itu Respon Sisi Permintaan (Demand Side Response)?", "id": "Pengelolaan Konsumsi Listrik Pelanggan." },
  { "en": "Apa Itu Beban Puncak (Peak Load)?", "id": "Permintaan Listrik Tertinggi Sistem." },
  { "en": "Apa Itu Beban Dasar (Base Load)?", "id": "Permintaan Listrik Minimum Konstan." },
  { "en": "Pembangkit Apa Untuk Beban Dasar (Base Load)?", "id": "PLTU (Pembangkit Listrik Tenaga Uap), PLTN (Pembangkit Listrik Tenaga Nuklir)." },
  { "en": "Pembangkit Apa Untuk Beban Puncak (Peak Load)?", "id": "PLTG (Pembangkit Listrik Tenaga Gas), PLTD (Pembangkit Listrik Tenaga Diesel)." },
  { "en": "Apa Itu Pembangkit Peaker (Peaker Plant)?", "id": "Pembangkit Cadangan Beban Puncak." },
  { "en": "Apa Itu Efek Termal (Thermal Effect) Arus?", "id": "Arus Listrik Menghasilkan Panas." },
  { "en": "Apa Itu Efek Magnetik (Magnetic Effect) Arus?", "id": "Arus Listrik Menghasilkan Medan Magnet." },
  { "en": "Apa Itu Efek Kimia (Chemical Effect) Arus?", "id": "Arus Listrik Sebabkan Reaksi Kimia." },
  { "en": "Contoh Efek Kimia (Chemical Effect) Arus?", "id": "Elektrolisis, Penyepuhan Logam (Electroplating)." },
  { "en": "Apa Itu Electroplating (Penyepuhan Listrik)?", "id": "Pelapisan Logam Menggunakan Arus Listrik." },
  { "en": "Apa Itu Anoda (Anode) Dalam Elektrolisis?", "id": "Elektroda Positif (Tempat Oksidasi)." },
  { "en": "Apa Itu Katoda (Cathode) Dalam Elektrolisis?", "id": "Elektroda Negatif (Tempat Reduksi)." },
  { "en": "Apa Itu Elektrolit (Electrolyte)?", "id": "Larutan Konduktif (Mengandung Ion)." },
  { "en": "Apa Itu Sel Galvanik (Galvanic Cell)?", "id": "Perangkat Hasilkan Listrik Reaksi Kimia." },
  { "en": "Apa Nama Lain Sel Galvanik (Galvanic Cell)?", "id": "Sel Volta (Voltaic Cell) (Baterai)." },
  { "en": "Apa Itu Potensial Elektroda (Electrode Potential)?", "id": "Beda Potensial Elektroda, Elektrolit." },
  { "en": "Apa Itu Korosi (Corrosion)?", "id": "Kerusakan Logam Akibat Reaksi Elektokimia." },
  { "en": "Apa Itu Proteksi Katodik (Cathodic Protection)?", "id": "Metode Pencegahan Korosi Logam." },
  { "en": "Apa Itu Anoda Korban (Sacrificial Anode)?", "id": "Logam Lebih Reaktif (Melindungi Logam Lain)." },
  { "en": "Apa Itu Arus Impres (Impressed Current) Proteksi Katodik?", "id": "Proteksi Katodik Menggunakan Sumber DC Eksternal." },
  { "en": "Apa Itu Medan Dekat (Near Field) Antena?", "id": "Daerah Dekat Antena (Energi Tersimpan)." },
  { "en": "Apa Itu Medan Jauh (Far Field) Antena?", "id": "Daerah Jauh Antena (Energi Radiasi)." },
  { "en": "Apa Itu EIRP (Effective Isotropic Radiated Power)?", "id": "Daya Radiasi Efektif Isotropik." },
  { "en": "Apa Kepanjangan EIRP (Effective Isotropic Radiated Power)?", "id": "Daya Radiasi Efektif Isotropik." },
  { "en": "Apa Itu ERP (Effective Radiated Power)?", "id": "Daya Radiasi Efektif (Relatif Dipol)." },
  { "en": "Apa Kepanjangan ERP (Effective Radiated Power)?", "id": "Daya Radiasi Efektif." },
  { "en": "Apa Itu Redaman Ruang Bebas (Free Space Path Loss)?", "id": "Pelemahan Sinyal Nirkabel Di Ruang Hampa." },
  { "en": "Apa Itu Fading (Redaman) Sinyal?", "id": "Fluktuasi Kekuatan Sinyal Diterima." },
  { "en": "Apa Itu Fading Multipath (Multipath Fading)?", "id": "Interferensi Akibat Pantulan Sinyal." },
  { "en": "Apa Itu Zona Fresnel (Fresnel Zone)?", "id": "Daerah Elips Antara Pemancar, Penerima." },
  { "en": "Mengapa Zona Fresnel (Fresnel Zone) Harus Bebas Hambatan?", "id": "Hambatan Sebabkan Redaman Sinyal." },
  { "en": "Apa Itu Polarisasi Gelombang (Wave Polarization)?", "id": "Orientasi Arah Getar Medan Listrik." },
  { "en": "Sebutkan Tiga Polarisasi (Polarization) Linear?", "id": "Vertikal, Horizontal, Miring (Slant)." },
  { "en": "Sebutkan Dua Polarisasi (Polarization) Sirkular?", "id": "Putar Kanan (RHCP), Putar Kiri (LHCP)." },
  { "en": "Apa Kepanjangan RHCP (Right Hand Circular Polarization)?", "id": "Polarisasi Sirkular Putar Kanan." },
  { "en": "Apa Kepanjangan LHCP (Left Hand Circular Polarization)?", "id": "Polarisasi Sirkular Putar Kiri." },
  { "en": "Apa Itu Keanekaragaman (Diversity) Penerima?", "id": "Teknik Melawan Fading (Banyak Antena)." },
  { "en": "Apa Itu Keanekaragaman Spasial (Space Diversity)?", "id": "Menggunakan Antena Penerima Terpisah." },
  { "en": "Apa Itu Keanekaragaman Frekuensi (Frequency Diversity)?", "id": "Mengirim Sinyal Frekuensi Berbeda." },
  { "en": "Apa Itu Keanekaragaman Waktu (Time Diversity)?", "id": "Mengirim Sinyal Waktu Berbeda." },
  { "en": "Apa Itu Keanekaragaman Polarisasi (Polarization Diversity)?", "id": "Menggunakan Antena Polarisasi Berbeda." },
  { "en": "Apa Itu Saluran Nirkabel (Wireless Channel)?", "id": "Media Transmisi Sinyal Nirkabel." },
  { "en": "Apa Itu Kapasitas Saluran (Channel Capacity) Shannon?", "id": "Laju Data Maksimal Teoretis." },
  { "en": "Apa Itu Teorema Shannon-Hartley (Shannon-Hartley Theorem)?", "id": "Rumus Kapasitas Saluran (SNR, Bandwidth)." },
  { "en": "Apa Itu Sinyal Analog (Analog Signal)?", "id": "Sinyal Kontinu Nilai, Waktu." },
  { "en": "Apa Itu Sinyal Digital (Digital Signal)?", "id": "Sinyal Diskrit Nilai, Waktu." },
  { "en": "Apa Itu Kuantisasi (Quantization)?", "id": "Pembulatan Nilai Sinyal Ke Level Diskrit." },
  { "en": "Apa Itu Pencuplikan (Sampling)?", "id": "Mengambil Nilai Sinyal Interval Waktu." },
  { "en": "Apa Itu Laju Pencuplikan (Sampling Rate)?", "id": "Frekuensi Pengambilan Sampel Sinyal." },
  { "en": "Apa Itu Teorema Nyquist (Nyquist Theorem)?", "id": "Sampling Rate Minimal Dua Kali Frekuensi Sinyal." },
  { "en": "Apa Itu Aliasing (Aliasing)?", "id": "Distorsi Akibat Sampling Rate Terlalu Rendah." },
  { "en": "Bagaimana Mencegah Aliasing (Aliasing)?", "id": "Menggunakan Filter Anti-Aliasing (Low-Pass)." },
  { "en": "Apa Kepanjangan PCM (Pulse Code Modulation)?", "id": "Modulasi Kode Pulsa." },
  { "en": "Apa Itu PCM (Pulse Code Modulation)?", "id": "Representasi Digital Sinyal Analog." },
  { "en": "Apa Itu Kualitas Layanan (Quality of Service) (QoS)?", "id": "Ukuran Kinerja Layanan Jaringan." },
  { "en": "Apa Kepanjangan QoS (Quality of Service)?", "id": "Kualitas Layanan." },
  { "en": "Parameter Apa Dalam QoS (Quality of Service)?", "id": "Bandwidth, Latency, Jitter, Packet Loss." },
  { "en": "Apa Itu Latency (Latency) Jaringan?", "id": "Waktu Tunda Pengiriman Data (Ping)." },
  { "en": "Apa Itu Jitter (Jitter) Jaringan?", "id": "Variasi Waktu Tunda (Latency)." },
  { "en": "Apa Itu Packet Loss (Kehilangan Paket)?", "id": "Data Gagal Sampai Tujuan." },
  { "en": "Apa Kepanjangan VoIP (Voice over Internet Protocol)?", "id": "Suara Lewat Protokol Internet." },
  { "en": "Mengapa QoS (Quality of Service) Penting Untuk VoIP (Voice over Internet Protocol)?", "id": "Memastikan Kualitas Suara Baik (Tanpa Putus)." },
  { "en": "Apa Itu Protokol (Protocol) Jaringan?", "id": "Aturan Standar Komunikasi Jaringan." },
  { "en": "Apa Itu Model TCP/IP (TCP/IP Model)?", "id": "Model Arsitektur Jaringan Internet." },
  { "en": "Berapa Lapis (Layer) Model TCP/IP (TCP/IP Model)?", "id": "Empat Lapis (Umumnya)." },
  { "en": "Sebutkan Lapis (Layer) TCP/IP (TCP/IP Model)?", "id": "Application, Transport, Internet, Link." },
  { "en": "Apa Itu Enkapsulasi (Encapsulation) Data?", "id": "Proses Penambahan Header Data (Turun Lapis)." },
  { "en": "Apa Itu Dekapsulasi (Decapsulation) Data?", "id": "Proses Pelepasan Header Data (Naik Lapis)." },
  { "en": "Apa Itu Soket (Socket) Jaringan?", "id": "Kombinasi Alamat IP Dan Nomor Port." },
  { "en": "Apa Itu Nomor Port (Port Number)?", "id": "Nomor Identifikasi Aplikasi Jaringan." },
  { "en": "Berapa Nomor Port (Port Number) HTTP (Hypertext Transfer Protocol)?", "id": "Port 80." },
  { "en": "Berapa Nomor Port (Port Number) HTTPS (Hypertext Transfer Protocol Secure)?", "id": "Port 443." },
  { "en": "Berapa Nomor Port (Port Number) FTP (File Transfer Protocol)?", "id": "Port 20 Dan 21." },
  { "en": "Berapa Nomor Port (Port Number) DNS (Domain Name System)?", "id": "Port 53." },
  { "en": "Apa Itu Port Dikenal (Well-Known Ports)?", "id": "Port 0 Hingga 1023 (Layanan Standar)." },
  { "en": "Apa Kepanjangan NAT (Network Address Translation)?", "id": "Translasi Alamat Jaringan." },
  { "en": "Apa Fungsi NAT (Network Address Translation)?", "id": "Mengubah Alamat IP Privat Ke Publik." },
  { "en": "Apa Itu Alamat IP (IP Address) Privat?", "id": "Alamat IP (IP Address) Jaringan Lokal (Internal)." },
  { "en": "Apa Itu Subnet Mask (Subnet Mask)?", "id": "Memisahkan Network ID Dan Host ID." },
  { "en": "Apa Itu Gateway (Gateway) Jaringan?", "id": "Pintu Keluar Jaringan Ke Jaringan Lain." },
  { "en": "Apa Itu Kelas Alamat IP (IP Address Class) A?", "id": "Jaringan Skala Sangat Besar." },
  { "en": "Apa Itu Kelas Alamat IP (IP Address Class) B?", "id": "Jaringan Skala Menengah Hingga Besar." },
  { "en": "Apa Itu Kelas Alamat IP (IP Address Class) C?", "id": "Jaringan Skala Kecil." },
  { "en": "Apa Rentang Alamat IP (IP Address) Privat Kelas A?", "id": "10.0.0.0 Hingga 10.255.255.255." },
  { "en": "Apa Rentang Alamat IP (IP Address) Privat Kelas C?", "id": "192.168.0.0 Hingga 192.168.255.255." },
  { "en": "Apa Itu Alamat Loopback (Loopback Address)?", "id": "Alamat Tes Diri Sendiri (127.0.0.1)." },
  { "en": "Apa Kepanjangan CIDR (Classless Inter-Domain Routing)?", "id": "Routing Antar Domain Tanpa Kelas." },
  { "en": "Apa Itu Prefiks (Prefix) Jaringan (Contoh: /24)?", "id": "Menunjukkan Jumlah Bit Subnet Mask." },
  { "en": "Berapa Bit Subnet Mask Untuk /24?", "id": "24 Bit (255.255.255.0)." },
  { "en": "Apa Kepanjangan IPv4 (Internet Protocol version 4)?", "id": "Protokol Internet Versi 4." },
  { "en": "Berapa Panjang Alamat IPv4 (Internet Protocol version 4)?", "id": "32 Bit (Empat Oktet)." },
  { "en": "Apa Kepanjangan IPv6 (Internet Protocol version 6)?", "id": "Protokol Internet Versi 6." },
  { "en": "Berapa Panjang Alamat IPv6 (Internet Protocol version 6)?", "id": "128 Bit (Heksadesimal)." },
  { "en": "Mengapa IPv6 (Internet Protocol version 6) Dibuat?", "id": "Mengatasi Keterbatasan Alamat IPv4." },
  { "en": "Apa Itu Ethernet (Ethernet)?", "id": "Teknologi Jaringan Kabel LAN Paling Umum." },
  { "en": "Apa Standar IEEE (Institute of Electrical and Electronics Engineers) Ethernet?", "id": "IEEE (Institute of Electrical and Electronics Engineers) 802.3." },
  { "en": "Apa Itu Media Akses Kontrol (MAC) Ethernet?", "id": "CSMA/CD (Carrier Sense Multiple Access/Collision Detection)." },
  { "en": "Apa Kepanjangan CSMA/CD (Carrier Sense Multiple Access/Collision Detection)?", "id": "Akses Ganda Deteksi Tabrakan." },
  { "en": "Apa Itu Tabrakan (Collision) Jaringan Ethernet?", "id": "Dua Perangkat Mengirim Data Bersamaan." },
  { "en": "Apa Itu Domain Tabrakan (Collision Domain)?", "id": "Area Jaringan Tempat Tabrakan Terjadi." },
  { "en": "Apa Itu Domain Siaran (Broadcast Domain)?", "id": "Area Jaringan Penyebaran Paket Siaran." },
  { "en": "Perangkat Apa Pemisah Domain Tabrakan (Collision Domain)?", "id": "Switch (Saklar) Jaringan, Jembatan (Bridge)." },
  { "en": "Perangkat Apa Pemisah Domain Siaran (Broadcast Domain)?", "id": "Router (Router) Jaringan." },
  { "en": "Apa Itu Kabel UTP (Unshielded Twisted Pair)?", "id": "Kabel Pasangan Terpilin Tanpa Pelindung." },
  { "en": "Apa Itu Kabel STP (Shielded Twisted Pair)?", "id": "Kabel Pasangan Terpilin Dengan Pelindung." },
  { "en": "Apa Kategori Kabel UTP (Unshielded Twisted Pair) Umum?", "id": "Cat 5e (Category 5e), Cat 6 (Category 6)." },
  { "en": "Apa Itu Kabel Straight-Through (Lurus)?", "id": "Kabel Koneksi Perangkat Berbeda (PC Ke Switch)." },
  { "en": "Apa Itu Kabel Crossover (Silang)?", "id": "Kabel Koneksi Perangkat Sama (PC Ke PC)." },
  { "en": "Standar Warna Apa Untuk Kabel Jaringan?", "id": "T568A Dan T568B." },
  { "en": "Apa Kepanjangan PoE (Power over Ethernet)?", "id": "Daya Listrik Lewat Kabel Ethernet." },
  { "en": "Apa Fungsi PoE (Power over Ethernet)?", "id": "Memberi Daya Perangkat (IP Camera, Access Point)." },
  { "en": "Apa Itu Access Point (Titik Akses) Nirkabel?", "id": "Menghubungkan Perangkat Nirkabel Ke Jaringan Kabel." },
  { "en": "Apa Kepanjangan SSID (Service Set Identifier)?", "id": "Nama Jaringan Nirkabel (Wi-Fi)." },
  { "en": "Apa Itu Keamanan Wi-Fi (Wi-Fi Security) WEP?", "id": "Wired Equivalent Privacy (Sangat Tidak Aman)." },
  { "en": "Apa Itu Keamanan Wi-Fi (Wi-Fi Security) WPA?", "id": "Wi-Fi Protected Access (Lebih Aman)." },
  { "en": "Apa Itu Keamanan Wi-Fi (Wi-Fi Security) WPA2 (Wi-Fi Protected Access 2)?", "id": "Standar Keamanan Wi-Fi (Wi-Fi) Saat Ini." },
  { "en": "Apa Itu Penyaringan MAC (MAC Filtering)?", "id": "Membatasi Akses Jaringan Berdasarkan MAC Address." },
  { "en": "Apa Itu Analisis Rangkaian (Circuit Analysis)?", "id": "Metode Perhitungan Tegangan, Arus Rangkaian." },
  { "en": "Apa Itu Analisis Simpul (Nodal Analysis)?", "id": "Analisis Rangkaian Berbasis Hukum Arus Kirchhoff (KCL)." },
  { "en": "Apa Itu Analisis Mesh (Mesh Analysis)?", "id": "Analisis Rangkaian Berbasis Hukum Tegangan Kirchhoff (KVL)." },
  { "en": "Apa Itu Sumber Tegangan (Voltage Source) Ideal?", "id": "Sumber Tegangan Konstan (Resistansi Internal Nol)." },
  { "en": "Apa Itu Sumber Arus (Current Source) Ideal?", "id": "Sumber Arus Konstan (Resistansi Internal Tak Terhingga)." },
  { "en": "Apa Itu Sumber Terikat (Dependent Source)?", "id": "Sumber (Tegangan/Arus) Dikontrol Variabel Lain." },
  { "en": "Apa Itu Teorema Superposisi (Superposition Theorem)?", "id": "Efek Total Adalah Jumlah Efek Individual." },
  { "en": "Apa Itu Teorema Thevenin (Thevenin's Theorem)?", "id": "Menyederhanakan Rangkaian Menjadi Satu V, R." },
  { "en": "Apa Itu Rangkaian Ekuivalen Thevenin (Thevenin Equivalent)?", "id": "Sumber Tegangan (Vth) Seri Resistor (Rth)." },
  { "en": "Apa Itu Teorema Norton (Norton's Theorem)?", "id": "Menyederhanakan Rangkaian Menjadi Satu I, R." },
  { "en": "Apa Itu Rangkaian Ekuivalen Norton (Norton Equivalent)?", "id": "Sumber Arus (In) Paralel Resistor (Rn)." },
  { "en": "Apa Hubungan Thevenin (Thevenin) Dan Norton (Norton)?", "id": "Rth = Rn, Vth = In x Rn." },
  { "en": "Apa Itu Teorema Transfer Daya Maksimum?", "id": "Transfer Daya Maksimal (R Beban = R Sumber)." },
  { "en": "Kapan Transfer Daya Maksimum (AC) Terjadi?", "id": "Z Beban = Z Konjugat Sumber." },
  { "en": "Apa Itu Konduktor (Conductor)?", "id": "Bahan Mudah Alirkan Arus Listrik." },
  { "en": "Apa Itu Isolator (Insulator)?", "id": "Bahan Sulit Alirkan Arus Listrik." },
  { "en": "Apa Itu Semikonduktor (Semiconductor)?", "id": "Bahan Antara Konduktor, Isolator." },
  { "en": "Contoh Semikonduktor (Semiconductor) Elemen?", "id": "Silikon (Si), Germanium (Ge)." },
  { "en": "Contoh Semikonduktor (Semiconductor) Senyawa?", "id": "Galium Arsenida (GaAs)." },
  { "en": "Apa Itu Doping (Doping) Semikonduktor?", "id": "Penambahan Ketidakmurnian (Impuritas)." },
  { "en": "Apa Itu Semikonduktor (Semiconductor) Tipe-N?", "id": "Kelebihan Elektron (Pembawa Muatan Negatif)." },
  { "en": "Apa Itu Semikonduktor (Semiconductor) Tipe-P?", "id": "Kekurangan Elektron (Kelebihan Hole)." },
  { "en": "Apa Itu Hole (Lubang) Semikonduktor?", "id": "Kekosongan Elektron (Pembawa Muatan Positif)." },
  { "en": "Apa Itu Bahan Dopan (Dopant) Tipe-N?", "id": "Atom Pentavalen (Fosfor, Arsenik)." },
  { "en": "Apa Itu Bahan Dopan (Dopant) Tipe-P?", "id": "Atom Trivalen (Boron, Galium)." },
  { "en": "Apa Itu Sambungan P-N (P-N Junction)?", "id": "Pertemuan Semikonduktor Tipe-P, Tipe-N." },
  { "en": "Apa Komponen Dasar Sambungan P-N (P-N Junction)?", "id": "Komponen Dioda Semikonduktor." },
  { "en": "Apa Itu Daerah Deplesi (Depletion Region)?", "id": "Daerah Sekitar Sambungan (Tanpa Muatan Bebas)." },
  { "en": "Apa Itu Potensial Penghalang (Barrier Potential)?", "id": "Tegangan Internal Daerah Deplesi." },
  { "en": "Apa Itu Bias Maju (Forward Bias) Dioda?", "id": "Tegangan Positif Ke P, Negatif Ke N." },
  { "en": "Apa Efek Bias Maju (Forward Bias) Dioda?", "id": "Daerah Deplesi Menyempit, Arus Mengalir." },
  { "en": "Apa Itu Bias Mundur (Reverse Bias) Dioda?", "id": "Tegangan Negatif Ke P, Positif Ke N." },
  { "en": "Apa Efek Bias Mundur (Reverse Bias) Dioda?", "id": "Daerah Deplesi Melebar, Arus Terhambat." },
  { "en": "Apa Itu Arus Bocor (Leakage Current) Dioda?", "id": "Arus Sangat Kecil Saat Bias Mundur." },
  { "en": "Apa Itu Tegangan Tembus (Breakdown Voltage) Dioda?", "id": "Tegangan Mundur Maksimal Dioda." },
  { "en": "Apa Itu Tembus Zener (Zener Breakdown)?", "id": "Tembus Tegangan Rendah (Doping Tinggi)." },
  { "en": "Apa Itu Tembus Avalanche (Avalanche Breakdown)?", "id": "Tembus Tegangan Tinggi (Tabrakan Elektron)." },
  { "en": "Apa Itu Transistor BJT (Bipolar Junction Transistor)?", "id": "Transistor Dua Sambungan (Bipolar)." },
  { "en": "Apa Dua Tipe BJT (Bipolar Junction Transistor)?", "id": "Tipe NPN Dan Tipe PNP." },
  { "en": "Apa Tiga Kaki BJT (Bipolar Junction Transistor)?", "id": "Emitor (Emitter), Basis (Base), Kolektor (Collector)." },
  { "en": "Bagaimana BJT (Bipolar Junction Transistor) Mode Aktif Dibiaskan?", "id": "Sambungan BE (Basis Emitor) Maju, BC (Basis Kolektor) Mundur." },
  { "en": "Apa Fungsi BJT (Bipolar Junction Transistor) Mode Aktif?", "id": "Sebagai Penguat (Amplifier) Sinyal." },
  { "en": "Apa Fungsi BJT (Bipolar Junction Transistor) Mode Saturasi?", "id": "Sebagai Saklar Tertutup (On)." },
  { "en": "Apa Fungsi BJT (Bipolar Junction Transistor) Mode Cut-Off?", "id": "Sebagai Saklar Terbuka (Off)." },
  { "en": "BJT (Bipolar Junction Transistor) Adalah Komponen Terkontrol Apa?", "id": "Komponen Terkontrol Arus (Arus Basis)." },
  { "en": "Apa Itu FET (Field Effect Transistor)?", "id": "Transistor Efek Medan." },
  { "en": "FET (Field Effect Transistor) Adalah Komponen Terkontrol Apa?", "id": "Komponen Terkontrol Tegangan (Tegangan Gerbang)." },
  { "en": "Apa Keunggulan FET (Field Effect Transistor) Dibanding BJT?", "id": "Impedansi Input Sangat Tinggi." },
  { "en": "Apa Dua Tipe Utama FET (Field Effect Transistor)?", "id": "JFET (Junction FET), MOSFET (Metal-Oxide-Semiconductor FET)." },
  { "en": "Apa Tiga Kaki JFET (Junction Field Effect Transistor)?", "id": "Source (Source), Gate (Gerbang), Drain (Saluran)." },
  { "en": "Bagaimana JFET (Junction Field Effect Transistor) Bekerja?", "id": "Tegangan Gerbang Atur Lebar Saluran." },
  { "en": "Apa Itu MOSFET (Metal-Oxide-Semiconductor FET) Tipe Depletion?", "id": "Memiliki Saluran Fisik Awal (Normal On)." },
  { "en": "Apa Itu MOSFET (Metal-Oxide-Semiconductor FET) Tipe Enhancement?", "id": "Tidak Ada Saluran Awal (Normal Off)." },
  { "en": "Mengapa Gerbang (Gate) MOSFET (Metal-Oxide-Semiconductor FET) Terisolasi?", "id": "Dilapisi Oksida Silikon (SiO2)." },
  { "en": "Apa Kelemahan MOSFET (Metal-Oxide-Semiconductor FET)?", "id": "Rentan Rusak Akibat Listrik Statis (ESD)." },
  { "en": "Apa Kepanjangan CMOS (Complementary Metal-Oxide-Semiconductor)?", "id": "Semikonduktor Oksida Logam Komplementer." },
  { "en": "Apa Itu Logika CMOS (Complementary Metal-Oxide-Semiconductor)?", "id": "Logika Digital Menggunakan PMOS, NMOS." },
  { "en": "Apa Keunggulan Utama Logika CMOS (Complementary Metal-Oxide-Semiconductor)?", "id": "Konsumsi Daya Statis Sangat Rendah." },
  { "en": "Apa Kepanjangan IC (Integrated Circuit)?", "id": "Sirkuit Terpadu." },
  { "en": "Apa Itu Fabrikasi (Fabrication) IC (Integrated Circuit)?", "id": "Proses Pembuatan Chip Sirkuit Terpadu." },
  { "en": "Apa Itu Wafer (Wafer) Silikon?", "id": "Substrat Dasar Pembuatan IC (Integrated Circuit)." },
  { "en": "Apa Itu Fotolitografi (Photolithography)?", "id": "Proses Pencetakan Pola Sirkuit." },
  { "en": "Apa Itu Ruang Bersih (Cleanroom)?", "id": "Ruangan Produksi IC (Bebas Debu)." },
  { "en": "Apa Itu Yield (Yield) Produksi IC?", "id": "Persentase Chip Fungsional Per Wafer." },
  { "en": "Apa Itu Gerbang Logika ECL (Emitter-Coupled Logic)?", "id": "Logika Pasangan Emitor (Sangat Cepat)." },
  { "en": "Apa Keunggulan ECL (Emitter-Coupled Logic)?", "id": "Kecepatan Switching Sangat Tinggi." },
  { "en": "Apa Kelemahan ECL (Emitter-Coupled Logic)?", "id": "Konsumsi Daya Sangat Tinggi." },
  { "en": "Apa Itu Sensor Kelembaban (Humidity Sensor)?", "id": "Sensor Pengukur Kadar Uap Air." },
  { "en": "Sebutkan Jenis Sensor Kelembaban (Humidity Sensor)?", "id": "Kapasitif Dan Resistif." },
  { "en": "Apa Itu Sensor Gas (Gas Sensor)?", "id": "Sensor Pendeteksi Keberadaan Gas Tertentu." },
  { "en": "Apa Itu Sensor Aliran (Flow Sensor)?", "id": "Sensor Pengukur Laju Aliran Fluida." },
  { "en": "Apa Itu Sensor Tekanan (Pressure Sensor)?", "id": "Sensor Pengukur Kekuatan Tekanan." },
  { "en": "Apa Satuan Tekanan (Pressure)?", "id": "Pascal (Pa) Atau Bar." },
  { "en": "Apa Itu Efek Seebeck (Seebeck Effect)?", "id": "Beda Suhu Hasilkan Tegangan Listrik." },
  { "en": "Komponen Apa Berbasis Efek Seebeck (Seebeck Effect)?", "id": "Termokopel (Thermocouple)." },
  { "en": "Apa Itu Efek Peltier (Peltier Effect)?", "id": "Arus Listrik Hasilkan Beda Suhu." },
  { "en": "Komponen Apa Berbasis Efek Peltier (Peltier Effect)?", "id": "Pendingin Termoelektrik (TEC)." },
  { "en": "Apa Itu Efek Thomson (Thomson Effect)?", "id": "Efek Pemanasan Pendinginan Konduktor Berarus." },
  { "en": "Apa Itu Termistor (Thermistor)?", "id": "Resistor Peka Terhadap Perubahan Suhu." },
  { "en": "Apa Beda NTC (Negative Temperature Coefficient) Dan PTC (Positive Temperature Coefficient)?", "id": "NTC (Negative Temperature Coefficient) Turun, PTC (Positive Temperature Coefficient) Naik." },
  { "en": "Apa Itu RTD (Resistance Temperature Detector)?", "id": "Detektor Suhu Berbasis Resistansi Logam." },
  { "en": "Logam Apa Dipakai RTD (Resistance Temperature Detector) Umum?", "id": "Platina (Platinum) (Contoh: Pt100)." },
  { "en": "Apa Itu Jembatan Wheatstone (Wheatstone Bridge)?", "id": "Rangkaian Pengukur Resistansi Tidak Dikenal." },
  { "en": "Kapan Jembatan Wheatstone (Wheatstone Bridge) Seimbang?", "id": "Tegangan Titik Tengah Nol (Setimbang)." },
  { "en": "Apa Itu Penguat Jembatan (Bridge Amplifier)?", "id": "Menguatkan Sinyal Output Jembatan Wheatstone." },
  { "en": "Apa Itu Strain Gauge (Strain Gauge)?", "id": "Sensor Pengukur Regangan Mekanis." },
  { "en": "Bagaimana Prinsip Kerja Strain Gauge (Strain Gauge)?", "id": "Perubahan Resistansi Akibat Regangan." },
  { "en": "Dimana Strain Gauge (Strain Gauge) Digunakan?", "id": "Sel Beban (Load Cell) Timbangan Digital." },
  { "en": "Apa Itu Akselerometer (Accelerometer)?", "id": "Sensor Pengukur Percepatan (Getaran)." },
  { "en": "Apa Itu Giroskop (Gyroscope)?", "id": "Sensor Pengukur Orientasi, Kecepatan Sudut." },
  { "en": "Apa Kepanjangan IMU (Inertial Measurement Unit)?", "id": "Unit Pengukuran Inersia." },
  { "en": "Apa Isi IMU (Inertial Measurement Unit)?", "id": "Akselerometer Dan Giroskop." },
  { "en": "Apa Itu Magnetometer (Magnetometer)?", "id": "Sensor Pengukur Kekuatan Medan Magnet." },
  { "en": "Apa Fungsi Magnetometer (Magnetometer) Di Ponsel?", "id": "Sebagai Kompas Digital." },
  { "en": "Apa Itu Sensor Efek Hall (Hall Effect)?", "id": "Sensor Pendeteksi Medan Magnet." },
  { "en": "Apa Itu LDR (Light Dependent Resistor)?", "id": "Resistor Peka Cahaya." },
  { "en": "Apa Itu Fotodioda (Photodiode)?", "id": "Dioda Peka Cahaya (Mode Bias Mundur)." },
  { "en": "Apa Itu Fototransistor (Phototransistor)?", "id": "Transistor Peka Cahaya (Basis Cahaya)." },
  { "en": "Apa Itu Optocoupler (Optocoupler)?", "id": "Isolator Rangkaian Menggunakan Cahaya." },
  { "en": "Apa Nama Lain Optocoupler (Optocoupler)?", "id": "Optoisolator (Optoisolator)." },
  { "en": "Apa Komponen Internal Optocoupler (Optocoupler)?", "id": "LED (Light Emitting Diode) Dan Fototransistor." },
  { "en": "Apa Fungsi Interrupter (Interrupter) Optik?", "id": "Sensor Penghalang Cahaya (Encoder Putar)." },
  { "en": "Apa Itu Sensor Ultrasonik (Ultrasonic Sensor)?", "id": "Sensor Jarak Gelombang Suara Ultrasonik." },
  { "en": "Apa Itu Sensor PIR (Passive Infrared)?", "id": "Sensor Deteksi Gerakan (Panas Tubuh)." },
  { "en": "Apa Kepanjangan PIR (Passive Infrared)?", "id": "Inframerah Pasif." },
  { "en": "Apa Itu Mikrofon (Microphone)?", "id": "Transduser Energi Suara Ke Listrik." },
  { "en": "Apa Itu Pengeras Suara (Loudspeaker)?", "id": "Transduser Energi Listrik Ke Suara." },
  { "en": "Apa Itu Buzzer (Buzzer)?", "id": "Penghasil Suara (Beep) Sederhana." },
  { "en": "Apa Itu Buzzer (Buzzer) Piezoelektrik?", "id": "Buzzer (Buzzer) Berbasis Efek Piezoelektrik." },
  { "en": "Apa Itu Relay (Relay) Elektromekanis?", "id": "Saklar Dikontrol Koil Elektromagnetik." },
  { "en": "Apa Itu SSR (Solid State Relay)?", "id": "Relay (Relay) Elektronik (Tanpa Gerakan)." },
  { "en": "Apa Keunggulan SSR (Solid State Relay)?", "id": "Cepat, Awet, Tidak Berisik." },
  { "en": "Apa Itu Dioda Flyback (Flyback Diode) Relay?", "id": "Melindungi Transistor Dari Tegangan Induksi." },
  { "en": "Apa Itu Kontaktor (Contactor)?", "id": "Relay (Relay) Daya Besar Arus Tinggi." },
  { "en": "Apa Itu Solenoida (Solenoid)?", "id": "Koil Kawat Hasilkan Medan Magnet." },
  { "en": "Apa Itu Aktuator Solenoida (Solenoid Actuator)?", "id": "Penggerak Elektromagnetik (Gerak Linear)." },
  { "en": "Apa Itu Katup Solenoida (Solenoid Valve)?", "id": "Katup Kontrol Aliran Fluida (Listrik)." },
  { "en": "Apa Itu Tampilan LCD (Liquid Crystal Display)?", "id": "Tampilan Kristal Cair." },
  { "en": "Apa Kepanjangan LCD (Liquid Crystal Display)?", "id": "Tampilan Kristal Cair." },
  { "en": "Apa Itu Tampilan LED (Light Emitting Diode)?", "id": "Tampilan Berbasis Dioda Pemancar Cahaya." },
  { "en": "Apa Itu Tampilan OLED (Organic Light Emitting Diode)?", "id": "Tampilan LED (Light Emitting Diode) Organik." },
  { "en": "Apa Keunggulan OLED (Organic Light Emitting Diode)?", "id": "Kontras Tinggi, Tipis, Fleksibel." },
  { "en": "Apa Itu Tampilan E-Ink (E-Ink Display)?", "id": "Tampilan Tinta Elektronik (Sangat Hemat Daya)." },
  { "en": "Dimana Tampilan E-Ink (E-Ink Display) Digunakan?", "id": "Pembaca Buku Elektronik (E-Reader)." },
  { "en": "Apa Itu Layar Sentuh (Touchscreen) Resistif?", "id": "Layar Sentuh Berbasis Tekanan Fisik." },
  { "en": "Apa Itu Layar Sentuh (Touchscreen) Kapasitif?", "id": "Layar Sentuh Berbasis Kapasitansi Tubuh." },
  { "en": "Apa Itu Keypad (Keypad) Matriks?", "id": "Tombol Disusun Baris, Kolom (Hemat Pin)." },
  { "en": "Apa Itu Rangkaian Debouncing (Debouncing)?", "id": "Rangkaian Penghilang Efek Pantulan Tombol." },
  { "en": "Mengapa Perlu Rangkaian Debouncing (Debouncing)?", "id": "Mencegah Pembacaan Ganda Tombol." },
  { "en": "Apa Itu Memori Stack (Stack Memory)?", "id": "Struktur Data LIFO (Last In First Out)." },
  { "en": "Apa Itu Memori Queue (Queue Memory)?", "id": "Struktur Data FIFO (First In First Out)." },
  { "en": "Apa Itu Pointer (Penunjuk) Stack?", "id": "Register Penunjuk Alamat Puncak Stack." },
  { "en": "Apa Itu Operasi Push (Push) Stack?", "id": "Menyimpan Data Ke Stack." },
  { "en": "Apa Itu Operasi Pop (Pop) Stack?", "id": "Mengambil Data Dari Stack." },
  { "en": "Apa Kepanjangan DIP (Dual In-line Package)?", "id": "Paket IC (Integrated Circuit) (Dua Baris Kaki)." },
  { "en": "Apa Kepanjangan SOIC (Small Outline Integrated Circuit)?", "id": "Paket IC (Integrated Circuit) SMD (Surface Mount Device) (Outline Kecil)." },
  { "en": "Apa Kepanjangan QFP (Quad Flat Package)?", "id": "Paket IC (Integrated Circuit) SMD (Surface Mount Device) (Empat Sisi Kaki)." },
  { "en": "Apa Kepanjangan BGA (Ball Grid Array)?", "id": "Paket IC (Integrated Circuit) (Array Bola Timah)." },
  { "en": "Apa Keunggulan Paket BGA (Ball Grid Array)?", "id": "Kepadatan Pin Sangat Tinggi." },
  { "en": "Apa Itu Heatsink (Heatsink)?", "id": "Komponen Pendingin Pasif (Penyerap Panas)." },
  { "en": "Bahan Apa Untuk Heatsink (Heatsink)?", "id": "Aluminium Atau Tembaga." },
  { "en": "Apa Itu Pasta Termal (Thermal Paste)?", "id": "Bahan Penghubung Transfer Panas." },
  { "en": "Apa Fungsi Pasta Termal (Thermal Paste)?", "id": "Mengisi Celah Udara (Komponen, Heatsink)." },
  { "en": "Apa Itu Kipas (Fan) Pendingin?", "id": "Komponen Pendingin Aktif (Hembusan Udara)." },
  { "en": "Apa Itu Pendingin Air (Water Cooling)?", "id": "Sistem Pendingin Menggunakan Cairan." },
  { "en": "Apa Itu Pipa Panas (Heat Pipe)?", "id": "Pipa Transfer Panas (Evaporasi, Kondensasi)." },
  { "en": "Apa Itu Desain Termal (Thermal Design)?", "id": "Manajemen Panas Rangkaian Elektronika." },
  { "en": "Apa Itu Resistansi Termal (Thermal Resistance)?", "id": "Ukuran Hambatan Aliran Panas." },
  { "en": "Apa Satuan Resistansi Termal (Thermal Resistance)?", "id": "Derajat Celcius Per Watt (°C/W)." },
  { "en": "Apa Itu Suhu Junction (Junction Temperature)?", "id": "Suhu Internal Inti Semikonduktor." },
  { "en": "Apa Itu Suhu Sekitar (Ambient Temperature)?", "id": "Suhu Lingkungan Sekitar Perangkat." },
  { "en": "Apa Itu Derating (Derating) Komponen?", "id": "Mengoperasikan Komponen Dibawah Rating Maksimal." },
  { "en": "Mengapa Melakukan Derating (Derating) Komponen?", "id": "Meningkatkan Keandalan, Umur Komponen." },
  { "en": "Apa Itu Keandalan (Reliability) Elektronik?", "id": "Probabilitas Komponen Berfungsi Sesuai Waktu." },
  { "en": "Apa Kepanjangan MTBF (Mean Time Between Failures)?", "id": "Waktu Rata-Rata Antar Kegagalan." },
  { "en": "Apa Kepanjangan MTTF (Mean Time To Failure)?", "id": "Waktu Rata-Rata Menuju Kegagalan." },
  { "en": "Apa Beda MTBF (Mean Time Between Failures) Dan MTTF (Mean Time To Failure)?", "id": "MTBF (Mean Time Between Failures) (Dapat Diperbaiki), MTTF (Mean Time To Failure) (Tidak)." },
  { "en": "Apa Itu Kurva Bak Mandi (Bathtub Curve)?", "id": "Grafik Laju Kegagalan Produk." },
  { "en": "Sebutkan Tiga Fase Kurva Bak Mandi?", "id": "Kegagalan Awal, Acak, Usang." },
  { "en": "Apa Itu Kegagalan Awal (Infant Mortality)?", "id": "Kegagalan Awal Akibat Cacat Produksi." },
  { "en": "Apa Itu Kegagalan Usang (Wear-Out Failure)?", "id": "Kegagalan Akhir Akibat Penuaan Komponen." },
  { "en": "Apa Itu Tes Burn-In (Burn-In Test)?", "id": "Tes Operasi Awal Deteksi Kegagalan Awal." },
  { "en": "Apa Itu Analisis FMEA (Failure Mode and Effects Analysis)?", "id": "Analisis Mode Kegagalan, Efeknya." },
  { "en": "Apa Kepanjangan FMEA (Failure Mode and Effects Analysis)?", "id": "Analisis Mode Kegagalan, Efeknya." },
  { "en": "Apa Itu DFT (Discrete Fourier Transform)?", "id": "Transformasi Fourier Waktu Diskrit." },
  { "en": "Apa Itu FFT (Fast Fourier Transform)?", "id": "Algoritma Cepat Hitung DFT." },
  { "en": "Apa Fungsi FFT (Fast Fourier Transform)?", "id": "Analisis Spektrum Frekuensi Digital." },
  { "en": "Apa Itu Jendela (Windowing) Sinyal DFT?", "id": "Mengurangi Kebocoran Spektral (Spectral Leakage)." },
  { "en": "Sebutkan Contoh Fungsi Jendela (Window Function)?", "id": "Hann (Hanning), Hamming, Blackman." },
  { "en": "Apa Itu Kebocoran Spektral (Spectral Leakage)?", "id": "Energi Sinyal Bocor Ke Frekuensi Lain." },
  { "en": "Apa Itu Efek Gibbs (Gibbs Phenomenon)?", "id": "Osilasi Dekat Diskontinuitas Sinyal." },
  { "en": "Apa Itu Penguat Operasional (Op-Amp) Rail-to-Rail?", "id": "Output Dapat Capai Tegangan Catu Daya." },
  { "en": "Apa Itu Common Mode Voltage Range (CMVR)?", "id": "Rentang Tegangan Input Mode Bersama." },
  { "en": "Apa Itu Power Supply Rejection Ratio (PSRR)?", "id": "Rasio Penolakan Derau Catu Daya." },
  { "en": "Apa Kepanjangan PSRR (Power Supply Rejection Ratio)?", "id": "Rasio Penolakan Catu Daya." },
  { "en": "Apa Itu Sirkuit Terbuka (Open Circuit)?", "id": "Rangkaian Terputus (Arus Nol)." },
  { "en": "Apa Resistansi (Resistance) Sirkuit Terbuka?", "id": "Resistansi Tak Terhingga." },
  { "en": "Apa Itu Hubungan Singkat (Short Circuit)?", "id": "Rangkaian Terhubung Langsung (Hambatan Nol)." },
  { "en": "Apa Resistansi (Resistance) Hubungan Singkat?", "id": "Resistansi Nol Ideal." },
  { "en": "Apa Itu Arus Hubungan Singkat (Short Circuit Current)?", "id": "Arus Sangat Besar Mengalir." },
  { "en": "Apa Itu Kapasitor Elektrolit (Electrolytic Capacitor) Bipolar?", "id": "Kapasitor Elektrolit Non-Polar (AC)." },
  { "en": "Dimana Kapasitor Bipolar (Bipolar Capacitor) Digunakan?", "id": "Crossover (Crossover) Audio Speaker." },
  { "en": "Apa Itu Kapasitor Super (Supercapacitor)?", "id": "Kapasitor Nilai Sangat Besar (Penyimpan Energi)." },
  { "en": "Apa Nama Lain Kapasitor Super (Supercapacitor)?", "id": "Ultra Capacitor (Ultrakapasitor)." },
  { "en": "Apa Keunggulan Kapasitor Super (Supercapacitor)?", "id": "Pengisian Cepat, Siklus Hidup Tinggi." },
  { "en": "Apa Itu Konstanta Dielektrik (Dielectric Constant) (k)?", "id": "Ukuran Kemampuan Simpan Energi Listrik." },
  { "en": "Apa Itu Kekuatan Dielektrik (Dielectric Strength)?", "id": "Tegangan Tembus Maksimum Isolator." },
  { "en": "Apa Itu Dioda Varactor (Varactor Diode)?", "id": "Kapasitansi Dioda Diatur Tegangan Mundur." },
  { "en": "Apa Nama Lain Dioda Varactor (Varactor Diode)?", "id": "Varicap (Varicap)." },
  { "en": "Dimana Dioda Varactor (Varactor Diode) Digunakan?", "id": "VCO (Voltage-Controlled Oscillator), Penala Radio." },
  { "en": "Apa Itu Dioda PIN (PIN Diode)?", "id": "Dioda Lapisan Intrinsik (I)." },
  { "en": "Apa Fungsi Dioda PIN (PIN Diode)?", "id": "Saklar RF, Atenuator RF." },
  { "en": "Apa Itu Dioda Gunn (Gunn Diode)?", "id": "Dioda Resistansi Negatif (Osilator Microwave)." },
  { "en": "Apa Itu Dioda IMPATT (IMPATT Diode)?", "id": "Dioda Osilator Microwave Daya Tinggi." },
  { "en": "Apa Itu Dioda Tunnel (Tunnel Diode)?", "id": "Dioda Resistansi Negatif (Efek Terowongan)." },
  { "en": "Apa Itu Dioda Step Recovery (Step Recovery Diode)?", "id": "Dioda Penghasil Harmonisa (Multiplier Frekuensi)." },
  { "en": "Bagaimana Efek Miller (Miller Effect) Mempengaruhi Frekuensi?", "id": "Mengurangi Bandwidth (Lebar Pita) Penguat." },
  { "en": "Apa Itu Rangkaian Cascode (Cascode Amplifier)?", "id": "Gabungan CE-CB (Common Emitter-Common Base)." },
  { "en": "Apa Keuntungan Rangkaian Cascode (Cascode Amplifier)?", "id": "Bandwidth (Lebar Pita) Tinggi (Kurangi Efek Miller)." },
  { "en": "Apa Itu Rangkaian Pasangan Darlington (Darlington Pair)?", "id": "Gabungan Dua BJT (Bipolar Junction Transistor)." },
  { "en": "Apa Keuntungan Pasangan Darlington (Darlington Pair)?", "id": "Penguatan Arus (Beta) Sangat Tinggi." },
  { "en": "Apa Kerugian Pasangan Darlington (Darlington Pair)?", "id": "Tegangan Saturasi Tinggi (Vce Sat)." },
  { "en": "Apa Itu Pasangan Sziklai (Sziklai Pair)?", "id": "Pasangan Darlington (Darlington Pair) Komplementer (NPN-PNP)." },
  { "en": "Apa Itu Cermin Arus (Current Mirror)?", "id": "Rangkaian Menyalin Arus Referensi." },
  { "en": "Apa Fungsi Cermin Arus (Current Mirror)?", "id": "Sebagai Sumber Arus (Beban Aktif)." },
  { "en": "Apa Itu Penguat Diferensial (Differential Amplifier)?", "id": "Menguatkan Selisih Dua Sinyal Input." },
  { "en": "Apa Itu Penguatan Mode Bersama (Common-Mode Gain)?", "id": "Penguatan Sinyal Sama Di Kedua Input." },
  { "en": "Apa Itu Penguatan Mode Diferensial (Differential-Mode Gain)?", "id": "Penguatan Selisih Sinyal Input." },
  { "en": "Apa Itu CMRR (Common Mode Rejection Ratio)?", "id": "Rasio Tolakan Mode Bersama." },
  { "en": "Apa Kepanjangan CMRR (Common Mode Rejection Ratio)?", "id": "Rasio Tolakan Mode Bersama." },
  { "en": "Apakah Nilai CMRR (Common Mode Rejection Ratio) Tinggi Baik?", "id": "Ya, Sangat Baik Menolak Noise." },
  { "en": "Apa Itu Penguat Instrumentasi (Instrumentation Amplifier)?", "id": "Penguat Diferensial Presisi Tinggi." },
  { "en": "Apa Ciri Penguat Instrumentasi (Instrumentation Amplifier)?", "id": "Impedansi Input Tinggi, CMRR (Common Mode Rejection Ratio) Tinggi." },
  { "en": "Apa Itu Logika Transistor-Transistor (TTL)?", "id": "Keluarga Logika Digital Berbasis BJT." },
  { "en": "Apa Kepanjangan TTL (Transistor-Transistor Logic)?", "id": "Logika Transistor-Transistor." },
  { "en": "Berapa Tegangan Catu Daya TTL (Transistor-Transistor Logic)?", "id": "Tepat 5 Volt." },
  { "en": "Apa Itu Gerbang Totem-Pole (Totem-Pole Output) TTL?", "id": "Konfigurasi Output Standar TTL." },
  { "en": "Apa Itu Floating (Floating) Input TTL?", "id": "Input TTL (Transistor-Transistor Logic) Tidak Terhubung." },
  { "en": "Input TTL (Transistor-Transistor Logic) Floating (Floating) Dianggap Apa?", "id": "Dianggap Logika Tinggi (High)." },
  { "en": "Apa Itu Logika CMOS (Complementary Metal-Oxide-Semiconductor)?", "id": "Keluarga Logika Digital Berbasis MOSFET." },
  { "en": "Apa Keunggulan Logika CMOS (Complementary Metal-Oxide-Semiconductor)?", "id": "Konsumsi Daya Statis Rendah." },
  { "en": "Berapa Rentang Tegangan Catu Daya CMOS (Complementary Metal-Oxide-Semiconductor)?", "id": "Rentang Lebar (Contoh: 3-18 Volt)." },
  { "en": "Apa Yang Terjadi Input CMOS (Complementary Metal-Oxide-Semiconductor) Floating?", "id": "Tidak Terdefinisi, Rentan Noise." },
  { "en": "Bagaimana Menangani Input CMOS (Complementary Metal-Oxide-Semiconductor) Tak Terpakai?", "id": "Harus Diikat (Pull-Up Atau Pull-Down)." },
  { "en": "Apa Itu Antarmuka (Interfacing) Logika?", "id": "Menghubungkan Keluarga Logika Berbeda (TTL, CMOS)." },
  { "en": "Apa Itu Latch-Up (Latch-Up) CMOS?", "id": "Kondisi Hubung Singkat Internal CMOS." },
  { "en": "Apa Penyebab Latch-Up (Latch-Up) CMOS?", "id": "Tegangan Input Melebihi Catu Daya." },
  { "en": "Apa Itu Sistem Bilangan Biner (Binary System)?", "id": "Sistem Bilangan Basis Dua (0, 1)." },
  { "en": "Apa Itu Sistem Bilangan Oktal (Octal System)?", "id": "Sistem Bilangan Basis Delapan (0-7)." },
  { "en": "Apa Itu Sistem Bilangan Desimal (Decimal System)?", "id": "Sistem Bilangan Basis Sepuluh (0-9)." },
  { "en": "Apa Itu Sistem Bilangan Heksadesimal (Hexadecimal System)?", "id": "Sistem Bilangan Basis Enam Belas (0-F)." },
  { "en": "Apa Itu Bit (Bit)?", "id": "Satuan Terkecil Data Digital (0/1)." },
  { "en": "Apa Itu Nibble (Nibble)?", "id": "Empat Bit (Setengah Byte)." },
  { "en": "Apa Itu Byte (Byte)?", "id": "Delapan Bit." },
  { "en": "Apa Itu Word (Word) Komputer?", "id": "Ukuran Data Alami Prosesor (16/32/64 Bit)." },
  { "en": "Apa Itu Kode BCD (Binary Coded Decimal)?", "id": "Representasi Biner Angka Desimal." },
  { "en": "Apa Kepanjangan BCD (Binary Coded Decimal)?", "id": "Desimal Dikodekan Biner." },
  { "en": "Apa Itu Kode Gray (Gray Code)?", "id": "Kode Biner (Hanya Beda Satu Bit)." },
  { "en": "Dimana Kode Gray (Gray Code) Digunakan?", "id": "Rotary Encoder (Mencegah Error Transisi)." },
  { "en": "Apa Itu Paritas (Parity) Bit?", "id": "Bit Tambahan Untuk Deteksi Kesalahan." },
  { "en": "Apa Itu Paritas Genap (Even Parity)?", "id": "Jumlah Total Bit '1' Genap." },
  { "en": "Apa Itu Paritas Ganjil (Odd Parity)?", "id": "Jumlah Total Bit '1' Ganjil." },
  { "en": "Apa Itu Checksum (Checksum)?", "id": "Metode Deteksi Kesalahan Blok Data." },
  { "en": "Apa Kepanjangan CRC (Cyclic Redundancy Check)?", "id": "Pengecekan Redundansi Siklik." },
  { "en": "Apa Itu Kode Hamming (Hamming Code)?", "id": "Kode Koreksi Kesalahan (ECC)." },
  { "en": "Apa Itu Aljabar Boolean (Boolean Algebra)?", "id": "Matematika Logika Digital (True/False)." },
  { "en": "Apa Itu Teorema De Morgan (De Morgan's Theorem)?", "id": "Aturan Ekuivalensi Gerbang Logika." },
  { "en": "Apa Hukum De Morgan (De Morgan's Law) Pertama?", "id": "(A + B)' = A' . B'." },
  { "en": "Apa Hukum De Morgan (De Morgan's Law) Kedua?", "id": "(A . B)' = A' + B'." },
  { "en": "Apa Itu Peta Karnaugh (Karnaugh Map)?", "id": "Metode Grafis Penyederhanaan Logika." },
  { "en": "Apa Kepanjangan K-Map (Karnaugh Map)?", "id": "Peta Karnaugh." },
  { "en": "Apa Itu Rangkaian Kombinasional (Combinational Circuit)?", "id": "Output Tergantung Input Saat Ini." },
  { "en": "Contoh Rangkaian Kombinasional (Combinational Circuit)?", "id": "Encoder, Decoder, Multiplexer, Adder." },
  { "en": "Apa Itu Rangkaian Sekuensial (Sequential Circuit)?", "id": "Output Tergantung Input, Memori." },
  { "en": "Contoh Rangkaian Sekuensial (Sequential Circuit)?", "id": "Flip-Flop, Counter, Register." },
  { "en": "Apa Itu Sinyal Clock (Clock Signal)?", "id": "Pulsa Sinkronisasi Rangkaian Sekuensial." },
  { "en": "Apa Itu Tepi Naik (Rising Edge) Clock?", "id": "Transisi Sinyal Dari Rendah Ke Tinggi." },
  { "en": "Apa Itu Tepi Turun (Falling Edge) Clock?", "id": "Transisi Sinyal Dari Tinggi Ke Rendah." },
  { "en": "Apa Itu Latch (Latch) SR (Set-Reset)?", "id": "Elemen Memori Dasar (Bistabil)." },
  { "en": "Apa Itu Flip-Flop (Flip-Flop)?", "id": "Elemen Memori (Terkendali Tepi Clock)." },
  { "en": "Apa Beda Latch (Latch) Dan Flip-Flop (Flip-Flop)?", "id": "Latch (Level-Triggered), Flip-Flop (Edge-Triggered)." },
  { "en": "Apa Itu Flip-Flop (Flip-Flop) D (Data)?", "id": "Menyimpan Nilai Input D Saat Clock." },
  { "en": "Apa Itu Flip-Flop (Flip-Flop) T (Toggle)?", "id": "Mengubah (Toggle) Output Saat Clock." },
  { "en": "Apa Itu Flip-Flop (Flip-Flop) JK?", "id": "Flip-Flop (Flip-Flop) Universal (Set, Reset, Toggle)." },
  { "en": "Apa Itu Kondisi Toggle (Toggle Condition) JK?", "id": "J = 1 Dan K = 1." },
  { "en": "Apa Itu Register (Register)?", "id": "Sekumpulan Flip-Flop (Penyimpan Data)." },
  { "en": "Apa Itu Register Geser (Shift Register)?", "id": "Register Menggeser Data Bit Per Bit." },
  { "en": "Apa Itu Counter (Pencacah) Asinkron?", "id": "Pencacah Riak (Ripple Counter)." },
  { "en": "Apa Itu Counter (Pencacah) Sinkron?", "id": "Semua Flip-Flop Di-clock Bersamaan." },
  { "en": "Apa Itu Register PIPO (Parallel In Parallel Out)?", "id": "Data Masuk Paralel, Keluar Paralel." },
  { "en": "Apa Itu Register PISO (Parallel In Serial Out)?", "id": "Data Masuk Paralel, Keluar Serial." },
  { "en": "Apa Itu Register SIPO (Serial In Parallel Out)?", "id": "Data Masuk Serial, Keluar Paralel." },
  { "en": "Apa Itu Register SISO (Serial In Serial Out)?", "id": "Data Masuk Serial, Keluar Serial." },
  { "en": "Apa Fungsi Register Universal (Universal Register)?", "id": "Gabungan PIPO, PISO, SIPO, SISO." },
  { "en": "Apa Itu Tampilan Matriks Titik (Dot Matrix Display)?", "id": "Tampilan LED (Light Emitting Diode) Susunan Matriks." },
  { "en": "Apa Itu Scanning (Pemindaian) Tampilan Matriks?", "id": "Menghidupkan Baris, Kolom Bergantian Cepat." },
  { "en": "Apa Itu Multiplexing (Multipleksasi) Tampilan?", "id": "Berbagi Pin Kontrol Tampilan Bergantian." },
  { "en": "Apa Itu Memori ROM (Read Only Memory)?", "id": "Memori Hanya Baca, Non-Volatil." },
  { "en": "Apa Itu Memori PROM (Programmable Read Only Memory)?", "id": "ROM (Read Only Memory) Dapat Diprogram Sekali." },
  { "en": "Apa Itu Memori EPROM (Erasable Programmable ROM)?", "id": "ROM (Read Only Memory) Dihapus Sinar UV." },
  { "en": "Apa Itu Memori EEPROM (Electrically Erasable PROM)?", "id": "ROM (Read Only Memory) Dihapus Listrik." },
  { "en": "Apa Itu Memori Flash (Flash Memory)?", "id": "Jenis EEPROM (Electrically Erasable PROM) Cepat." },
  { "en": "Apa Itu Memori SRAM (Static Random Access Memory)?", "id": "RAM (Random Access Memory) Statis (Flip-Flop)." },
  { "en": "Apa Itu Memori DRAM (Dynamic Random Access Memory)?", "id": "RAM (Random Access Memory) Dinamis (Kapasitor)." },
  { "en": "Memori Apa Yang Perlu Disegarkan (Refresh)?", "id": "Memori DRAM (Dynamic Random Access Memory)." },
  { "en": "Memori Apa Yang Lebih Cepat, SRAM (Static RAM) Atau DRAM (Dynamic RAM)?", "id": "SRAM (Static RAM) Lebih Cepat." },
  { "en": "Memori Apa Yang Lebih Padat, SRAM (Static RAM) Atau DRAM (Dynamic RAM)?", "id": "DRAM (Dynamic RAM) Lebih Padat." },
  { "en": "Apa Itu Memori Cache (Cache Memory)?", "id": "Memori Cepat Antara CPU, RAM." },
  { "en": "Jenis Memori Apa Untuk Cache (Cache)?", "id": "SRAM (Static RAM)." },
  { "en": "Apa Itu ADC (Analog-to-Digital Converter)?", "id": "Pengubah Sinyal Analog Ke Digital." },
  { "en": "Apa Itu DAC (Digital-to-Analog Converter)?", "id": "Pengubah Sinyal Digital Ke Analog." },
  { "en": "Apa Itu Resolusi (Resolution) ADC (Analog-to-Digital Converter)?", "id": "Jumlah Bit Output Digital." },
  { "en": "Apa Itu Laju Cuplik (Sampling Rate) ADC?", "id": "Seberapa Cepat Sinyal Dicuplik." },
  { "en": "Apa Itu Kesalahan Kuantisasi (Quantization Error) ADC?", "id": "Error Akibat Pembulatan Sinyal." },
  { "en": "Apa Itu Filter Anti-Aliasing (Anti-Aliasing Filter)?", "id": "Filter Low-Pass (LPF) Sebelum ADC." },
  { "en": "Apa Itu Rangkaian Op-Amp (Operational Amplifier) Komparator?", "id": "Pembanding Dua Sinyal Tegangan." },
  { "en": "Apa Output Rangkaian Komparator (Comparator)?", "id": "Tegangan Saturasi Positif Atau Negatif." },
  { "en": "Apa Itu Rangkaian Op-Amp (Operational Amplifier) Penjumlah?", "id": "Menjumlahkan Beberapa Tegangan Input." },
  { "en": "Apa Itu Rangkaian Op-Amp (Operational Amplifier) Pengurang?", "id": "Mengurangkan Dua Tegangan Input." },
  { "en": "Apa Itu Rangkaian Op-Amp (Operational Amplifier) Integrator?", "id": "Output Adalah Integral Input." },
  { "en": "Apa Itu Rangkaian Op-Amp (Operational Amplifier) Diferensiator?", "id": "Output Adalah Turunan Input." },
  { "en": "Apa Itu Pengikut Tegangan (Voltage Follower) Op-Amp?", "id": "Penguatan Satu (Buffer Impedansi)." },
  { "en": "Apa Itu Sensor Suhu (Temperature Sensor)?", "id": "Mengukur Besaran Panas Atau Dingin." },
  { "en": "Apa Itu Sensor Cahaya (Light Sensor)?", "id": "Mengukur Intensitas Cahaya." },
  { "en": "Apa Itu Sensor Jarak (Distance Sensor)?", "id": "Mengukur Jarak Objek." },
  { "en": "Apa Itu Sensor Getaran (Vibration Sensor)?", "id": "Mengukur Getaran Mekanis." },
  { "en": "Apa Itu Sensor Suara (Sound Sensor)?", "id": "Mengukur Intensitas Suara (Mikrofon)." },
  { "en": "Apa Itu Aktuator (Actuator)?", "id": "Perangkat Penggerak Mekanis." },
  { "en": "Apa Itu Motor DC (Direct Current)?", "id": "Aktuator Putar Arus Searah." },
  { "en": "Apa Itu Motor Stepper (Stepper Motor)?", "id": "Aktuator Putar Per Langkah (Diskret)." },
  { "en": "Apa Itu Motor Servo (Servo Motor)?", "id": "Aktuator Putar Kontrol Posisi." },
  { "en": "Apa Itu Solenoida (Solenoid)?", "id": "Aktuator Gerak Linear (Maju-Mundur)." },
  { "en": "Apa Itu Nilai RMS (Root Mean Square)?", "id": "Nilai Efektif Sinyal AC." },
  { "en": "Apa Itu Nilai Puncak (Peak Value)?", "id": "Nilai Amplitudo Maksimum Sinyal." },
  { "en": "Apa Itu Nilai Rata-Rata (Average Value) AC Sinus?", "id": "Nol Volt Selama Satu Siklus." },
  { "en": "Apa Itu Faktor Bentuk (Form Factor)?", "id": "Rasio Nilai RMS (Root Mean Square), Rata-Rata." },
  { "en": "Apa Itu Faktor Puncak (Crest Factor)?", "id": "Rasio Nilai Puncak, RMS (Root Mean Square)." },
  { "en": "Apa Itu Analisis Fourier (Fourier Analysis)?", "id": "Memecah Sinyal Menjadi Komponen Frekuensi." },
  { "en": "Apa Itu Frekuensi Fundamental (Fundamental Frequency)?", "id": "Frekuensi Terendah Sinyal Periodik." },
  { "en": "Apa Itu Harmonisa (Harmonics)?", "id": "Frekuensi Kelipatan Bulat Fundamental." },
  { "en": "Apa Penyebab Harmonisa (Harmonics) Sistem Tenaga?", "id": "Beban Non-Linear (Penyearah, SMPS)." },
  { "en": "Apa Itu CPU (Central Processing Unit)?", "id": "Unit Pemroses Pusat (Otak Komputer)." },
  { "en": "Apa Kepanjangan CPU (Central Processing Unit)?", "id": "Unit Pemroses Pusat." },
  { "en": "Apa Itu ALU (Arithmetic Logic Unit)?", "id": "Bagian CPU (Central Processing Unit) Hitungan Aritmetika, Logika." },
  { "en": "Apa Kepanjangan ALU (Arithmetic Logic Unit)?", "id": "Unit Aritmetika Logika." },
  { "en": "Apa Itu Unit Kontrol (Control Unit) CPU?", "id": "Mengatur Operasi CPU (Central Processing Unit)." },
  { "en": "Apa Itu Register (Register) CPU?", "id": "Memori Internal CPU (Central Processing Unit) Super Cepat." },
  { "en": "Apa Itu Bus (Bus) Alamat CPU?", "id": "Jalur Penentu Alamat Memori." },
  { "en": "Apa Itu Bus (Bus) Data CPU?", "id": "Jalur Transfer Data CPU (Central Processing Unit), Memori." },
  { "en": "Apa Itu Bus (Bus) Kontrol CPU?", "id": "Jalur Sinyal Kontrol (Baca, Tulis)." },
  { "en": "Apa Itu Siklus Instruksi (Instruction Cycle) CPU?", "id": "Proses Ambil, Dekode, Eksekusi Instruksi." },
  { "en": "Apa Itu Mikrokontroler (Microcontroller)?", "id": "Komputer Kecil Dalam Satu Chip." },
  { "en": "Apa Isi Mikrokontroler (Microcontroller)?", "id": "CPU (Central Processing Unit), RAM (Random Access Memory), ROM (Read Only Memory), I/O." },
  { "en": "Apa Beda Mikroprosesor (Microprocessor) Dan Mikrokontroler (Microcontroller)?", "id": "Mikrokontroler (Microcontroller) Lebih Terintegrasi." },
  { "en": "Apa Kepanjangan I/O (Input Output)?", "id": "Masukan Keluaran." },
  { "en": "Apa Fungsi Port I/O (Input Output) Mikrokontroler?", "id": "Terhubung Dunia Luar (Sensor, Aktuator)." },
  { "en": "Apa Itu Arsitektur Harvard (Harvard Architecture)?", "id": "Memori Data, Program Terpisah." },
  { "en": "Apa Itu Arsitektur Von Neumann (Von Neumann Architecture)?", "id": "Memori Data, Program Bersatu." },
  { "en": "Apa Itu Bahasa Pemrograman C (C Language)?", "id": "Bahasa Umum Pemrograman Mikrokontroler." },
  { "en": "Apa Itu Bahasa Assembly (Assembly Language)?", "id": "Bahasa Level Rendah Spesifik CPU." },
  { "en": "Apa Itu Kompilator (Compiler)?", "id": "Penerjemah Bahasa Tinggi Ke Mesin." },
  { "en": "Apa Itu Debugger (Debugger) Rangkaian?", "id": "Alat Cari Kesalahan (Bug) Hardware/Software." },
  { "en": "Apa Kepanjangan JTAG (Joint Test Action Group)?", "id": "Standar Antarmuka Debugging Hardware." },
  { "en": "Apa Itu Bootloader (Bootloader) Mikrokontroler?", "id": "Program Kecil Memuat Program Utama." },
  { "en": "Apa Kepanjangan UART (Universal Asynchronous Receiver-Transmitter)?", "id": "Komunikasi Serial Asinkron Universal." },
  { "en": "Berapa Kawat Minimal UART (Universal Asynchronous Receiver-Transmitter)?", "id": "Dua Kawat (TX, RX)." },
  { "en": "Apa Itu Laju Baud (Baud Rate)?", "id": "Kecepatan Transfer Simbol Serial." },
  { "en": "Apa Itu Bit Paritas (Parity Bit) UART?", "id": "Bit Ekstra Deteksi Kesalahan." },
  { "en": "Apa Itu Stop Bit (Stop Bit) UART?", "id": "Sinyal Akhir Transmisi Data." },
  { "en": "Apa Itu Protokol RS-232 (RS-232 Protocol)?", "id": "Standar Level Tegangan UART (Universal Asynchronous Receiver-Transmitter)." },
  { "en": "Apa Itu IC (Integrated Circuit) MAX232 (MAX232)?", "id": "IC (Integrated Circuit) Konverter Level TTL (Transistor-Transistor Logic) Ke RS-232." },
  { "en": "Apa Itu Protokol RS-485 (RS-485 Protocol)?", "id": "Standar Komunikasi Serial Diferensial." },
  { "en": "Apa Keunggulan RS-485 (RS-485)?", "id": "Jarak Jauh, Multi-Perangkat." },
  { "en": "Apa Itu Resistor Terminator (Terminator Resistor) RS-485?", "id": "Resistor Ujung Bus (Cegah Pantulan)." },
  { "en": "Apa Itu Kabel Serat Optik (Fiber Optic)?", "id": "Media Transmisi Data Cahaya." },
  { "en": "Apa Keunggulan Kabel Serat Optik (Fiber Optic)?", "id": "Bandwidth (Lebar Pita) Tinggi, Kebal EMI." },
  { "en": "Apa Itu Mode Tunggal (Single Mode) Serat Optik?", "id": "Serat Optik Jarak Sangat Jauh." },
  { "en": "Apa Itu Mode Jamak (Multi Mode) Serat Optik?", "id": "Serat Optik Jarak Lebih Pendek." },
  { "en": "Apa Itu Splicing (Splicing) Serat Optik?", "id": "Teknik Penyambungan Inti Serat Optik." },
  { "en": "Apa Itu Atenuasi (Attenuation) Cahaya Optik?", "id": "Pelemahan Sinyal Cahaya Di Serat." },
  { "en": "Apa Itu Dispersi (Dispersion) Cahaya Optik?", "id": "Pelebaran Pulsa Cahaya Di Serat." },
  { "en": "Apa Itu Sumber Cahaya (Light Source) Optik?", "id": "LED (Light Emitting Diode) Atau Laser." },
  { "en": "Apa Itu Detektor (Detector) Cahaya Optik?", "id": "Fotodioda (Photodiode)." },
  { "en": "Apa Itu Solder (Solder)?", "id": "Logam Paduan (Timah) Penyambung Komponen." },
  { "en": "Apa Itu Fluks (Flux) Solder?", "id": "Bahan Kimia Pembersih Oksida." },
  { "en": "Apa Itu Solder Tanpa Timbal (Lead-Free Solder)?", "id": "Solder Tanpa Kandungan Timbal (Pb)." },
  { "en": "Mengapa Pakai Solder Tanpa Timbal (Lead-Free Solder)?", "id": "Regulasi Lingkungan (RoHS)." },
  { "en": "Apa Kepanjangan RoHS (Restriction of Hazardous Substances)?", "id": "Pembatasan Bahan Berbahaya." },
  { "en": "Apa Itu Desoldering (Desoldering)?", "id": "Proses Melepas Solderan Komponen." },
  { "en": "Apa Itu Solder Wick (Sumbu Solder)?", "id": "Serabut Tembaga Penyerap Timah Cair." },
  { "en": "Apa Itu Solder Sucker (Penyedot Timah)?", "id": "Alat Vakum Penyedot Timah Cair." },
  { "en": "Apa Itu Cold Joint (Sambungan Dingin)?", "id": "Hasil Solderan Buruk (Tidak Mengkilap)." },
  { "en": "Apa Itu Komponen SMD (Surface Mount Device)?", "id": "Komponen Pemasangan Permukaan PCB." },
  { "en": "Apa Itu Komponen Through-Hole (Tembus Lubang)?", "id": "Komponen Kaki Menembus Lubang PCB." },
  { "en": "Apa Itu PCB (Printed Circuit Board) Layout?", "id": "Desain Tata Letak Jalur PCB." },
  { "en": "Perangkat Lunak Apa Untuk Desain PCB?", "id": "Eagle, KiCad, Altium Designer." },
  { "en": "Apa Itu Gerber (Gerber File)?", "id": "Format File Standar Manufaktur PCB." },
  { "en": "Apa Itu Drill File (File Bor)?", "id": "File Posisi, Ukuran Lubang Bor PCB." },
  { "en": "Apa Itu Etsa (Etching) PCB?", "id": "Proses Pengikisan Lapisan Tembaga." },
  { "en": "Larutan Apa Untuk Etsa (Etching) PCB?", "id": "Ferri Klorida (FeCl3)." },
  { "en": "Apa Itu Solder Mask (Masker Solder)?", "id": "Lapisan Pelindung (Hijau) Permukaan PCB." },
  { "en": "Apa Itu Silkscreen (Sablon Sutra) PCB?", "id": "Lapisan Teks Putih (Label Komponen)." },
  { "en": "Apa Itu Via (Via) Pada PCB?", "id": "Lubang Penghubung Antar Lapisan PCB." },
  { "en": "Apa Itu Blind Via (Via Buta)?", "id": "Via (Via) Terhubung Lapisan Luar, Dalam." },
  { "en": "Apa Itu Buried Via (Via Terkubur)?", "id": "Via (Via) Terhubung Antar Lapisan Dalam." },
  { "en": "Apa Itu Jalur (Trace) PCB?", "id": "Garis Tembaga Penghubung Komponen." },
  { "en": "Apa Itu Pad (Pad) PCB?", "id": "Area Tembaga Tempat Solder Komponen." },
  { "en": "Apa Itu Ground Plane (Bidang Tanah) PCB?", "id": "Area Tembaga Luas Terhubung Ground." },
  { "en": "Apa Fungsi Ground Plane (Bidang Tanah) PCB?", "id": "Mengurangi Noise, Impedansi Ground." },
  { "en": "Apa Itu Power Plane (Bidang Daya) PCB?", "id": "Area Tembaga Luas Sumber Daya (VCC)." },
  { "en": "Apa Itu Decoupling Capacitor (Kapasitor Decoupling)?", "id": "Kapasitor Bypass Dekat Pin IC." },
  { "en": "Apa Fungsi Kapasitor Decoupling (Decoupling Capacitor)?", "id": "Menstabilkan Catu Daya IC Lokal." },
  { "en": "Nilai Kapasitor Decoupling (Decoupling Capacitor) Umum?", "id": "0.1 uF (100 nF) Keramik." },
  { "en": "Apa Itu Ferit Bead (Ferrite Bead)?", "id": "Komponen Penekan Noise Frekuensi Tinggi." },
  { "en": "Apa Itu Impedansi Terkontrol (Controlled Impedance) PCB?", "id": "Desain Jalur Impedansi Spesifik (RF)." },
  { "en": "Apa Itu DRC (Design Rule Check)?", "id": "Pengecekan Aturan Desain PCB Otomatis." },
  { "en": "Apa Kepanjangan DRC (Design Rule Check)?", "id": "Pengecekan Aturan Desain." },
  { "en": "Apa Itu ERC (Electrical Rule Check)?", "id": "Pengecekan Aturan Kelistrikan Skematik." },
  { "en": "Apa Kepanjangan ERC (Electrical Rule Check)?", "id": "Pengecekan Aturan Kelistrikan." },
  { "en": "Apa Itu Diagram Skematik (Schematic Diagram)?", "id": "Gambar Rangkaian Logis (Simbol Komponen)." },
  { "en": "Apa Itu Netlist (Netlist)?", "id": "Daftar Koneksi Antar Komponen Skematik." },
  { "en": "Apa Itu Bill of Materials (BOM)?", "id": "Daftar Komponen Diperlukan Rakit PCB." },
  { "en": "Apa Kepanjangan BOM (Bill of Materials)?", "id": "Daftar Material." },
  { "en": "Apa Itu Perakitan PCB (PCB Assembly) Otomatis?", "id": "Menggunakan Mesin Pick-and-Place." },
  { "en": "Apa Itu Reflow Oven (Oven Reflow)?", "id": "Oven Pemanas Pasta Solder (SMD)." },
  { "en": "Apa Itu Solder Gelombang (Wave Soldering)?", "id": "Mesin Solder Komponen Through-Hole." },
  { "en": "Apa Itu Pengujian Fungsional (Functional Testing)?", "id": "Tes Fungsi Rangkaian Setelah Perakitan." },
  { "en": "Apa Itu Pengujian In-Circuit (In-Circuit Testing) (ICT)?", "id": "Tes Komponen Individual Di Sirkuit." },
  { "en": "Apa Itu JIG (JIG) Pengujian?", "id": "Alat Bantu Penjepit, Koneksi PCB Tes." },
  { "en": "Apa Itu Lapisan Konformal (Conformal Coating)?", "id": "Lapisan Tipis Pelindung PCB Jadi." },
  { "en": "Apa Fungsi Lapisan Konformal (Conformal Coating)?", "id": "Melindungi Dari Kelembaban, Debu, Korosi." },
  { "en": "Apa Itu Potting (Potting) Elektronik?", "id": "Mengisi Enklosur Dengan Resin Pelindung." },
  { "en": "Apa Itu Enklosur (Enclosure) Elektronik?", "id": "Kotak Wadah Rangkaian Elektronik." },
  { "en": "Apa Itu Pembumian Chassis (Chassis Grounding)?", "id": "Menghubungkan Enklosur Logam Ke Ground." },
  { "en": "Apa Itu Pengukur LCR (LCR Meter)?", "id": "Alat Ukur Induktansi, Kapasitansi, Resistansi." },
  { "en": "Apa Itu Osiloskop Penyimpanan Digital (DSO)?", "id": "Digital Storage Oscilloscope (DSO)." },
  { "en": "Apa Kepanjangan DSO (Digital Storage Oscilloscope)?", "id": "Osiloskop Penyimpanan Digital." },
  { "en": "Apa Itu Penganalisis Logika (Logic Analyzer)?", "id": "Alat Ukur Sinyal Digital Multi-Kanal." },
  { "en": "Apa Itu Penganalisis Spektrum (Spectrum Analyzer)?", "id": "Alat Ukur Sinyal Domain Frekuensi." },
  { "en": "Apa Itu Generator Sinyal (Signal Generator)?", "id": "Alat Pembangkit Sinyal Uji." },
  { "en": "Apa Nama Lain Generator Sinyal (Signal Generator)?", "id": "Function Generator (Generator Fungsi)." },
  { "en": "Bentuk Sinyal Apa Dihasilkan Generator Fungsi?", "id": "Sinus, Kotak, Segitiga, Gigi Gergaji." },
  { "en": "Apa Itu Generator Pulsa (Pulse Generator)?", "id": "Alat Pembangkit Sinyal Pulsa Digital." },
  { "en": "Apa Itu Generator Arbitrer (Arbitrary Waveform Generator)?", "id": "Pembangkit Bentuk Gelombang Kustom." },
  { "en": "Apa Itu Catu Daya (Power Supply) Laboratorium?", "id": "Sumber Tegangan DC Variabel Teregulasi." },
  { "en": "Apa Itu Beban Elektronik (Electronic Load)?", "id": "Alat Simulasi Beban Listrik Terkontrol." },
  { "en": "Apa Itu Ruang Anechoic (Anechoic Chamber)?", "id": "Ruangan Kedap Gema (Pengujian RF/Audio)." },
  { "en": "Apa Itu Uji Kompatibilitas Elektromagnetik (EMC)?", "id": "Electromagnetic Compatibility (EMC) Testing." },
  { "en": "Apa Kepanjangan EMC (Electromagnetic Compatibility)?", "id": "Kompatibilitas Elektromagnetik." },
  { "en": "Apa Itu Emisi (Emissions) EMC (Electromagnetic Compatibility)?", "id": "Gangguan Elektromagnetik Dipancarkan Perangkat." },
  { "en": "Apa Itu Kekebalan (Immunity) EMC (Electromagnetic Compatibility)?", "id": "Kemampuan Perangkat Tahan Gangguan Eksternal." },
  { "en": "Apa Itu ESD (Electrostatic Discharge)?", "id": "Pelepasan Muatan Listrik Statis." },
  { "en": "Apa Kepanjangan ESD (Electrostatic Discharge)?", "id": "Pelepasan Muatan Elektrostatik." },
  { "en": "Bagaimana Mencegah Kerusakan ESD (Electrostatic Discharge)?", "id": "Gelang Anti-Statis, Meja Kerja Ground." },
  { "en": "Apa Itu Tikar Anti-Statis (Anti-Static Mat)?", "id": "Tikar Disipatif Muatan Statis." },
  { "en": "Apa Itu Tas Anti-Statis (Anti-Static Bag)?", "id": "Kantong Pelindung Komponen Elektronik." },
  { "en": "Apa Itu Simbol Peringatan ESD (Electrostatic Discharge)?", "id": "Segitiga Kuning Dengan Tangan Tersilang." },
  { "en": "Apa Itu Kelas Kebersihan (Cleanroom Class) ISO?", "id": "Standar Jumlah Partikel Ruang Bersih." },
  { "en": "Apa Itu Filter HEPA (High-Efficiency Particulate Air)?", "id": "Filter Udara Partikel Sangat Kecil." },
  { "en": "Apa Kepanjangan HEPA (High-Efficiency Particulate Air)?", "id": "Udara Partikulat Efisiensi Tinggi." },
  { "en": "Apa Itu Filter ULPA (Ultra-Low Particulate Air)?", "id": "Filter Udara Lebih Efisien Dari HEPA." },
  { "en": "Apa Kepanjangan ULPA (Ultra-Low Particulate Air)?", "id": "Udara Partikulat Ultra Rendah." },
  { "en": "Apa Itu Ionizer (Ionizer) Ruang Bersih?", "id": "Penetral Muatan Statis Di Udara." },
  { "en": "Apa Itu Kendali Numerik Komputer (CNC)?", "id": "Computer Numerical Control (CNC)." },
  { "en": "Apa Kepanjangan CNC (Computer Numerical Control)?", "id": "Kendali Numerik Komputer." },
  { "en": "Apa Fungsi Mesin CNC (Computer Numerical Control)?", "id": "Otomatisasi Mesin Perkakas (Milling, Bubut)." },
  { "en": "Apa Itu Kode G (G-Code)?", "id": "Bahasa Pemrograman Mesin CNC." },
  { "en": "Apa Itu Robotika Kolaboratif (Collaborative Robotics)?", "id": "Robot Aman Bekerja Bersama Manusia." },
  { "en": "Apa Nama Lain Robot Kolaboratif (Collaborative Robotics)?", "id": "Cobot (Cobot)." },
  { "en": "Apa Itu Sistem Visi Mesin (Machine Vision)?", "id": "Penggunaan Kamera Otomatisasi Industri." },
  { "en": "Apa Fungsi Sistem Visi Mesin (Machine Vision)?", "id": "Inspeksi Kualitas, Pemandu Robot." },
  { "en": "Apa Itu Kecerdasan Buatan (Artificial Intelligence) (AI)?", "id": "Kemampuan Mesin Meniru Kecerdasan Manusia." },
  { "en": "Apa Itu Pembelajaran Mesin (Machine Learning) (ML)?", "id": "Cabang AI (Artificial Intelligence) (Belajar Dari Data)." },
  { "en": "Apa Itu Pembelajaran Mendalam (Deep Learning) (DL)?", "id": "Cabang ML (Machine Learning) (Jaringan Saraf Dalam)." },
  { "en": "Apa Itu Jaringan Saraf Konvolusional (CNN)?", "id": "Convolutional Neural Network (CNN) (Visi Komputer)." },
  { "en": "Apa Kepanjangan CNN (Convolutional Neural Network)?", "id": "Jaringan Saraf Konvolusional." },
  { "en": "Apa Itu Jaringan Saraf Berulang (RNN)?", "id": "Recurrent Neural Network (RNN) (Data Urutan)." },
  { "en": "Apa Kepanjangan RNN (Recurrent Neural Network)?", "id": "Jaringan Saraf Berulang." },
  { "en": "Dimana RNN (Recurrent Neural Network) Digunakan?", "id": "Pemrosesan Bahasa Alami (NLP)." },
  { "en": "Apa Kepanjangan NLP (Natural Language Processing)?", "id": "Pemrosesan Bahasa Alami." },
  { "en": "Apa Itu Big Data (Big Data)?", "id": "Kumpulan Data Sangat Besar, Kompleks." },
  { "en": "Apa Itu Internet of Things (IoT)?", "id": "Jaringan Perangkat Fisik Terhubung Internet." },
  { "en": "Apa Kepanjangan IoT (Internet of Things)?", "id": "Internet Untuk Segala." },
  { "en": "Apa Itu Komputasi Awan (Cloud Computing)?", "id": "Penyediaan Layanan Komputasi Lewat Internet." },
  { "en": "Apa Itu Komputasi Tepi (Edge Computing)?", "id": "Pemrosesan Data Dekat Sumber Data." },
  { "en": "Apa Itu Blockchain (Blockchain)?", "id": "Teknologi Buku Besar Digital Terdistribusi." },
  { "en": "Apa Itu Mata Uang Kripto (Cryptocurrency)?", "id": "Mata Uang Digital (Bitcoin, Ethereum)." },
  { "en": "Apa Itu Realitas Virtual (Virtual Reality) (VR)?", "id": "Simulasi Lingkungan Tiga Dimensi Interaktif." },
  { "en": "Apa Kepanjangan VR (Virtual Reality)?", "id": "Realitas Virtual." },
  { "en": "Apa Itu Realitas Tertambah (Augmented Reality) (AR)?", "id": "Menambahkan Elemen Digital Ke Dunia Nyata." },
  { "en": "Apa Kepanjangan AR (Augmented Reality)?", "id": "Realitas Tertambah." },
  { "en": "Apa Itu Komputasi Kuantum (Quantum Computing)?", "id": "Komputasi Berbasis Prinsip Mekanika Kuantum." },
  { "en": "Apa Itu Qubit (Qubit) (Quantum Bit)?", "id": "Unit Dasar Informasi Komputer Kuantum." },
  { "en": "Apa Itu Superposisi (Superposition) Medan Magnet?", "id": "Total Medan Adalah Jumlah Vektor Medan." },
  { "en": "Apa Itu Momen Dipol Magnetik (Magnetic Dipole Moment)?", "id": "Ukuran Kekuatan Sumber Medan Magnet." },
  { "en": "Apa Satuan Momen Dipol Magnetik (Magnetic Dipole Moment)?", "id": "Ampere Meter Persegi (A·m²)." },
  { "en": "Apa Itu Magnetisasi (Magnetization) (M)?", "id": "Momen Dipol Magnetik Per Satuan Volume." },
  { "en": "Apa Hubungan B (Medan Magnet), H (Kekuatan Medan), M (Magnetisasi)?", "id": "B = μ₀ (H + M)." },
  { "en": "Apa Itu Kerentanan Magnetik (Magnetic Susceptibility) (Χm)?", "id": "Ukuran Derajat Magnetisasi Bahan." },
  { "en": "Bagaimana Kerentanan (Susceptibility) Bahan Diamagnetik?", "id": "Nilainya Negatif Kecil." },
  { "en": "Bagaimana Kerentanan (Susceptibility) Bahan Paramagnetik?", "id": "Nilainya Positif Kecil." },
  { "en": "Bagaimana Kerentanan (Susceptibility) Bahan Feromagnetik?", "id": "Nilainya Positif Sangat Besar." },
  { "en": "Apa Itu Medan Listrik Statis (Electrostatics)?", "id": "Studi Medan Listrik Muatan Diam." },
  { "en": "Apa Itu Medan Magnet Statis (Magnetostatics)?", "id": "Studi Medan Magnet Arus Konstan." },
  { "en": "Apa Itu Elektrodinamika (Electrodynamics)?", "id": "Studi Medan Listrik, Magnet Berubah Waktu." },
  { "en": "Apa Itu Potensial Skalar Listrik (Electric Scalar Potential)?", "id": "Potensial Tegangan (V)." },
  { "en": "Apa Itu Potensial Vektor Magnetik (Magnetic Vector Potential)?", "id": "Potensial Vektor (A)." },
  { "en": "Apa Itu Persamaan Gelombang (Wave Equation)?", "id": "Persamaan Diferensial Deskripsi Gelombang." },
  { "en": "Apa Itu Bilangan Gelombang (Wave Number) (k)?", "id": "Ukuran Spasial Frekuensi Gelombang." },
  { "en": "Apa Itu Polarisasi (Polarization) Linear?", "id": "Medan Listrik Bergetar Satu Garis." },
  { "en": "Apa Itu Polarisasi (Polarization) Sirkular?", "id": "Medan Listrik Berputar Membentuk Lingkaran." },
  { "en": "Apa Itu Polarisasi (Polarization) Eliptik?", "id": "Medan Listrik Berputar Membentuk Elips." },
  { "en": "Apa Itu Hukum Snellius (Snell's Law)?", "id": "Hukum Pembiasan Gelombang Elektromagnetik." },
  { "en": "Apa Itu Indeks Bias (Refractive Index) (n)?", "id": "Rasio Kecepatan Cahaya Vakum, Medium." },
  { "en": "Apa Itu Sudut Brewster (Brewster's Angle)?", "id": "Sudut Pantul Tanpa Komponen Polarisasi Paralel." },
  { "en": "Apa Itu Efek Faraday (Faraday Effect)?", "id": "Rotasi Polarisasi Cahaya Medan Magnet." },
  { "en": "Apa Itu Efek Kerr (Kerr Effect)?", "id": "Perubahan Indeks Bias Akibat Medan Listrik." },
  { "en": "Apa Itu Metamaterial (Metamaterial)?", "id": "Bahan Artifisial Sifat Elektromagnetik Unik." },
  { "en": "Apa Itu Resistor Non-Induktif (Non-Inductive Resistor)?", "id": "Resistor Dirancang Induktansi Parasitik Rendah." },
  { "en": "Dimana Resistor Non-Induktif (Non-Inductive Resistor) Digunakan?", "id": "Rangkaian Frekuensi Tinggi, Snubber." },
  { "en": "Apa Itu Kapasitor Feedthrough (Feedthrough Capacitor)?", "id": "Kapasitor Filter EMI (Electromagnetic Interference) Dipasang Panel." },
  { "en": "Apa Itu Penguat Lock-In (Lock-In Amplifier)?", "id": "Penguat Ekstraksi Sinyal Derau Sangat Rendah." },
  { "en": "Apa Prinsip Penguat Lock-In (Lock-In Amplifier)?", "id": "Deteksi Sensitif Fasa (Phase-Sensitive Detection)." },
  { "en": "Apa Itu Transformasi Hilbert (Hilbert Transform)?", "id": "Transformasi Sinyal (Analisis Fasa, Amplitudo)." },
  { "en": "Apa Itu Sinyal Analitik (Analytic Signal)?", "id": "Representasi Kompleks Sinyal Riil." },
  { "en": "Apa Itu Pengambilan Sampel Kuadratur (Quadrature Sampling)?", "id": "Sampling Sinyal (Komponen In-Phase, Quadrature)." },
  { "en": "Apa Itu Komponen In-Phase (I)?", "id": "Komponen Sefasa Dengan Carrier." },
  { "en": "Apa Itu Komponen Kuadratur (Q)?", "id": "Komponen Beda Fasa 90 Derajat Carrier." },
  { "en": "Apa Itu Modulasi IQ (IQ Modulation)?", "id": "Modulasi Menggunakan Komponen I Dan Q." },
  { "en": "Apa Itu Penguat Kelas E (Class E Amplifier)?", "id": "Penguat Switching Efisiensi Tinggi (RF)." },
  { "en": "Apa Itu Penguat Kelas F (Class F Amplifier)?", "id": "Penguat Switching (Bentuk Gelombang Kotak)." },
  { "en": "Apa Itu Penguat Doherty (Doherty Amplifier)?", "id": "Penguat Daya RF (Efisiensi Tinggi)." },
  { "en": "Apa Itu Linierisasi (Linearization) Penguat Daya?", "id": "Teknik Mengurangi Distorsi Penguat RF." },
  { "en": "Apa Itu Predistorsi (Predistortion)?", "id": "Teknik Linierisasi (Mengubah Input)." },
  { "en": "Apa Itu Efek Memori (Memory Effect) Penguat?", "id": "Output Penguat Tergantung Input Sebelumnya." },
  { "en": "Apa Kepanjangan PAPR (Peak-to-Average Power Ratio)?", "id": "Rasio Daya Puncak Terhadap Rata-Rata." },
  { "en": "Mengapa PAPR (Peak-to-Average Power Ratio) Tinggi Buruk?", "id": "Mengurangi Efisiensi Penguat Daya." },
  { "en": "Sinyal Apa Punya PAPR (Peak-to-Average Power Ratio) Tinggi?", "id": "Sinyal OFDM (Orthogonal Frequency Division Multiplexing)." },
  { "en": "Apa Itu Crest Factor Reduction (CFR)?", "id": "Teknik Mengurangi PAPR (Peak-to-Average Power Ratio) Sinyal." },
  { "en": "Apa Itu Pengkondisian Sinyal (Signal Conditioning)?", "id": "Memproses Sinyal Sensor Agar Sesuai." },
  { "en": "Contoh Pengkondisian Sinyal (Signal Conditioning)?", "id": "Penguatan, Filtering, Linierisasi, Isolasi." },
  { "en": "Apa Itu Linierisasi (Linearization) Sensor?", "id": "Membuat Respon Sensor Menjadi Linear." },
  { "en": "Apa Itu Kompensasi Cold Junction (Cold Junction Compensation)?", "id": "Koreksi Suhu Referensi Termokopel." },
  { "en": "Apa Kepanjangan CJC (Cold Junction Compensation)?", "id": "Kompensasi Sambungan Dingin." },
  { "en": "Apa Itu Isolasi Sinyal (Signal Isolation)?", "id": "Memisahkan Ground Sensor, Sistem Ukur." },
  { "en": "Mengapa Perlu Isolasi Sinyal (Signal Isolation)?", "id": "Menghilangkan Ground Loop, Keamanan." },
  { "en": "Apa Itu Penguat Isolasi (Isolation Amplifier)?", "id": "Penguat Dengan Isolasi Galvanik." },
  { "en": "Apa Itu Filter Kalman (Kalman Filter)?", "id": "Algoritma Estimasi Keadaan Optimal." },
  { "en": "Dimana Filter Kalman (Kalman Filter) Digunakan?", "id": "Navigasi, Pelacakan Objek, Kontrol." },
  { "en": "Apa Itu Transformasi Wavelet (Wavelet Transform)?", "id": "Analisis Sinyal (Representasi Waktu-Frekuensi)." },
  { "en": "Apa Beda Wavelet (Wavelet) Dan Fourier (Fourier)?", "id": "Wavelet (Wavelet) Lokal Waktu, Frekuensi." },
  { "en": "Apa Itu Teori Kontrol Optimal (Optimal Control)?", "id": "Desain Kontroler Kinerja Terbaik." },
  { "en": "Apa Itu Kontroler LQR (Linear Quadratic Regulator)?", "id": "Kontrol Optimal Sistem Linear." },
  { "en": "Apa Kepanjangan LQR (Linear Quadratic Regulator)?", "id": "Regulator Kuadratik Linear." },
  { "en": "Apa Itu Sistem MIMO (Multiple Input Multiple Output) Kontrol?", "id": "Sistem Dengan Banyak Input, Output." },
  { "en": "Apa Itu Dekopling (Decoupling) Kontrol MIMO?", "id": "Memisahkan Interaksi Antar Loop Kontrol." },
  { "en": "Apa Itu Kontrol Kuat (Robust Control)?", "id": "Desain Kontroler Tahan Ketidakpastian Model." },
  { "en": "Apa Itu H-Infinity (H-Infinity) Kontrol?", "id": "Metode Desain Kontrol Kuat." },
  { "en": "Apa Itu Estimasi Keadaan (State Estimation)?", "id": "Memperkirakan Keadaan Internal Sistem." },
  { "en": "Apa Itu Pengamat Keadaan (State Observer)?", "id": "Algoritma Estimasi Keadaan (Luenberger)." },
  { "en": "Apa Itu Sistem Waktu Diskrit (Discrete-Time System)?", "id": "Sistem Beroperasi Pada Waktu Diskrit." },
  { "en": "Apa Itu Transformasi Z (Z-Transform)?", "id": "Alat Analisis Sistem Waktu Diskrit." },
  { "en": "Apa Itu Kestabilan (Stability) Sistem Diskrit?", "id": "Pole Di Dalam Lingkaran Satuan." },
  { "en": "Apa Itu Lingkaran Satuan (Unit Circle) Bidang-Z?", "id": "Lingkaran Jari-Jari Satu Pusat Nol." },
  { "en": "Apa Itu Kontrol Digital (Digital Control)?", "id": "Implementasi Kontroler Menggunakan Komputer." },
  { "en": "Apa Keuntungan Kontrol Digital (Digital Control)?", "id": "Fleksibel, Presisi, Tahan Noise." },
  { "en": "Apa Itu Discretization (Diskritisasi)?", "id": "Mengubah Model Kontinu Ke Diskrit." },
  { "en": "Metode Apa Untuk Discretization (Diskritisasi)?", "id": "Euler, Tustin (Bilinear Transform)." },
  { "en": "Apa Itu Aliasing (Aliasing) Kontrol Digital?", "id": "Frekuensi Tinggi Muncul Frekuensi Rendah." },
  { "en": "Apa Itu Teori Grafik (Graph Theory)?", "id": "Studi Matematika Grafik (Simpul, Sisi)." },
  { "en": "Bagaimana Teori Grafik (Graph Theory) Digunakan Elektro?", "id": "Analisis Rangkaian, Jaringan." },
  { "en": "Apa Itu Matriks Insidensi (Incidence Matrix)?", "id": "Representasi Hubungan Simpul, Cabang Grafik." },
  { "en": "Apa Itu Cut Set (Cut Set) Grafik?", "id": "Kumpulan Cabang Pemutus Konektivitas." },
  { "en": "Apa Itu Tie Set (Tie Set) Grafik?", "id": "Kumpulan Cabang Pembentuk Loop." },
  { "en": "Apa Itu Finite Element Method (FEM)?", "id": "Metode Numerik Solusi Persamaan Diferensial." },
  { "en": "Apa Kepanjangan FEM (Finite Element Method)?", "id": "Metode Elemen Hingga." },
  { "en": "Dimana FEM (Finite Element Method) Digunakan Elektro?", "id": "Simulasi Medan Elektromagnetik, Termal." },
  { "en": "Apa Itu Boundary Element Method (BEM)?", "id": "Metode Numerik (Diskritisasi Batas)." },
  { "en": "Apa Kepanjangan BEM (Boundary Element Method)?", "id": "Metode Elemen Batas." },
  { "en": "Apa Itu Metode Monte Carlo (Monte Carlo Method)?", "id": "Metode Simulasi Berbasis Angka Acak." },
  { "en": "Apa Itu Optimasi (Optimization)?", "id": "Proses Mencari Solusi Terbaik." },
  { "en": "Apa Itu Pemrograman Linear (Linear Programming)?", "id": "Optimasi Masalah Fungsi Tujuan Linear." },
  { "en": "Apa Itu Pemrograman Non-Linear (Nonlinear Programming)?", "id": "Optimasi Masalah Fungsi Tujuan Non-Linear." },
  { "en": "Apa Itu Algoritma Gradient Descent (Gradient Descent)?", "id": "Algoritma Optimasi Pencarian Minimum." },
  { "en": "Apa Itu Simulated Annealing (Simulated Annealing)?", "id": "Algoritma Optimasi Probabilistik Global." },
  { "en": "Apa Itu Partikel Swarm Optimization (PSO)?", "id": "Algoritma Optimasi Terinspirasi Perilaku Kawanan." },
  { "en": "Apa Kepanjangan PSO (Particle Swarm Optimization)?", "id": "Optimasi Kawanan Partikel." },
  { "en": "Apa Itu Rekayasa Perangkat Lunak (Software Engineering)?", "id": "Disiplin Pengembangan Perangkat Lunak." },
  { "en": "Apa Itu Siklus Hidup Perangkat Lunak (SDLC)?", "id": "Software Development Life Cycle (SDLC)." },
  { "en": "Apa Kepanjangan SDLC (Software Development Life Cycle)?", "id": "Siklus Hidup Pengembangan Perangkat Lunak." },
  { "en": "Sebutkan Model SDLC (Software Development Life Cycle)?", "id": "Waterfall, Agile, Spiral." },
  { "en": "Apa Itu Model Waterfall (Waterfall Model)?", "id": "Model SDLC (Software Development Life Cycle) Sekuensial Linear." },
  { "en": "Apa Itu Metodologi Agile (Agile Methodology)?", "id": "Model SDLC (Software Development Life Cycle) Iteratif, Inkremental." },
  { "en": "Apa Itu Scrum (Scrum)?", "id": "Kerangka Kerja Manajemen Proyek Agile." },
  { "en": "Apa Itu Pengujian Perangkat Lunak (Software Testing)?", "id": "Proses Verifikasi, Validasi Perangkat Lunak." },
  { "en": "Apa Itu Pengujian Unit (Unit Testing)?", "id": "Pengujian Komponen Terkecil Perangkat Lunak." },
  { "en": "Apa Itu Pengujian Integrasi (Integration Testing)?", "id": "Pengujian Interaksi Antar Komponen." },
  { "en": "Apa Itu Pengujian Sistem (System Testing)?", "id": "Pengujian Sistem Perangkat Lunak Keseluruhan." },
  { "en": "Apa Itu Pengujian Penerimaan (Acceptance Testing)?", "id": "Pengujian Oleh Pengguna Akhir." },
  { "en": "Apa Itu Manajemen Versi (Version Control)?", "id": "Sistem Pelacakan Perubahan Kode Sumber." },
  { "en": "Sistem Manajemen Versi (Version Control) Populer?", "id": "Git (Git), Subversion (SVN)." },
  { "en": "Apa Itu Repositori (Repository)?", "id": "Tempat Penyimpanan Kode, Riwayat Perubahan." },
  { "en": "Apa Itu Commit (Commit) Dalam Git?", "id": "Menyimpan Snapshot Perubahan Kode." },
  { "en": "Apa Itu Branch (Cabang) Dalam Git?", "id": "Garis Pengembangan Kode Terpisah." },
  { "en": "Apa Itu Merge (Menggabungkan) Dalam Git?", "id": "Menggabungkan Perubahan Antar Branch." },
  { "en": "Apa Itu Pull Request (Permintaan Tarik)?", "id": "Mekanisme Review, Merge Kode Kolaboratif." },
  { "en": "Apa Itu Continuous Integration (CI)?", "id": "Integrasi Kode Otomatis Secara Berkala." },
  { "en": "Apa Kepanjangan CI (Continuous Integration)?", "id": "Integrasi Berkelanjutan." },
  { "en": "Apa Itu Continuous Deployment (CD)?", "id": "Deployment (Penyebaran) Kode Otomatis Ke Produksi." },
  { "en": "Apa Kepanjangan CD (Continuous Deployment)?", "id": "Penyebaran Berkelanjutan." },
  { "en": "Apa Itu Pipeline (Pipeline) CI/CD?", "id": "Alur Kerja Otomatis (Build, Test, Deploy)." },
  { "en": "Apa Itu Docker (Docker)?", "id": "Platform Kontainerisasi Aplikasi." },
  { "en": "Apa Itu Kontainer (Container)?", "id": "Paket Aplikasi Beserta Dependensinya." },
  { "en": "Apa Keuntungan Kontainerisasi (Containerization)?", "id": "Portabilitas, Konsistensi Lingkungan Aplikasi." },
  { "en": "Apa Itu Kubernetes (Kubernetes)?", "id": "Platform Orkestrasi Kontainer Otomatis." },
  { "en": "Apa Itu Microservices (Layanan Mikro)?", "id": "Arsitektur Aplikasi (Layanan Kecil Independen)." },
  { "en": "Apa Itu API (Application Programming Interface)?", "id": "Antarmuka Pemrograman Aplikasi." },
  { "en": "Apa Kepanjangan API (Application Programming Interface)?", "id": "Antarmuka Pemrograman Aplikasi." },
  { "en": "Apa Fungsi API (Application Programming Interface)?", "id": "Jembatan Komunikasi Antar Aplikasi." },
  { "en": "Apa Itu REST (Representational State Transfer) API?", "id": "Gaya Arsitektur API (Application Programming Interface) Web Umum." },
  { "en": "Apa Kepanjangan REST (Representational State Transfer)?", "id": "Transfer Keadaan Representasional." },
  { "en": "Apa Itu JSON (JavaScript Object Notation)?", "id": "Format Pertukaran Data Ringan." },
  { "en": "Apa Kepanjangan JSON (JavaScript Object Notation)?", "id": "Notasi Objek JavaScript." },
  { "en": "Apa Itu XML (Extensible Markup Language)?", "id": "Bahasa Markup Struktur Data." },
  { "en": "Apa Kepanjangan XML (Extensible Markup Language)?", "id": "Bahasa Markup Yang Dapat Diperluas." },
  { "en": "Apa Itu Basis Data (Database)?", "id": "Kumpulan Data Terstruktur Tersimpan." },
  { "en": "Apa Itu Basis Data Relasional (Relational Database)?", "id": "Basis Data Berbasis Tabel Relasi." },
  { "en": "Contoh Basis Data Relasional (Relational Database)?", "id": "MySQL, PostgreSQL, SQL Server." },
  { "en": "Apa Itu SQL (Structured Query Language)?", "id": "Bahasa Kueri Basis Data Relasional." },
  { "en": "Apa Kepanjangan SQL (Structured Query Language)?", "id": "Bahasa Kueri Terstruktur." },
  { "en": "Apa Itu Basis Data NoSQL (Not Only SQL)?", "id": "Basis Data Non-Relasional." },
  { "en": "Contoh Basis Data NoSQL (Not Only SQL)?", "id": "MongoDB, Cassandra, Redis." },
  { "en": "Apa Itu Key-Value Store (Penyimpanan Kunci-Nilai)?", "id": "Jenis Basis Data NoSQL Sederhana." },
  { "en": "Apa Itu Document Store (Penyimpanan Dokumen)?", "id": "Basis Data NoSQL (Simpan Dokumen JSON/BSON)." },
  { "en": "Apa Itu Columnar Database (Basis Data Kolom)?", "id": "Basis Data Optimalisasi Baca Per Kolom." },
  { "en": "Apa Itu Graph Database (Basis Data Graf)?", "id": "Basis Data Menyimpan Relasi Entitas." },
  { "en": "Apa Itu Sistem Operasi (Operating System)?", "id": "Perangkat Lunak Pengelola Sumber Daya Komputer." },
  { "en": "Contoh Sistem Operasi (Operating System) Desktop?", "id": "Windows, MacOS, Linux." },
  { "en": "Contoh Sistem Operasi (Operating System) Mobile?", "id": "Android, iOS." },
  { "en": "Apa Itu Kernel (Kernel) Sistem Operasi?", "id": "Inti Sistem Operasi (Manajemen Utama)." },
  { "en": "Apa Itu Proses (Process) Sistem Operasi?", "id": "Program Yang Sedang Dieksekusi." },
  { "en": "Apa Itu Thread (Thread) Sistem Operasi?", "id": "Unit Eksekusi Terkecil Dalam Proses." },
  { "en": "Apa Itu Penjadwalan (Scheduling) Proses?", "id": "Algoritma Pemilihan Proses Untuk CPU." },
  { "en": "Apa Itu Memori Virtual (Virtual Memory)?", "id": "Teknik Manajemen Memori (Gunakan Disk)." },
  { "en": "Apa Itu Paging (Paging) Memori?", "id": "Membagi Memori Menjadi Halaman Tetap." },
  { "en": "Apa Itu Segmentasi (Segmentation) Memori?", "id": "Membagi Memori Menjadi Segmen Logis." },
  { "en": "Apa Itu Deadlock (Deadlock) Sistem Operasi?", "id": "Kondisi Proses Saling Tunggu Sumber Daya." },
  { "en": "Apa Itu Sistem Berkas (File System)?", "id": "Struktur Penyimpanan, Pengelolaan Berkas." },
  { "en": "Contoh Sistem Berkas (File System) Windows?", "id": "NTFS (New Technology File System), FAT32 (File Allocation Table 32)." },
  { "en": "Contoh Sistem Berkas (File System) Linux?", "id": "Ext4 (Fourth Extended Filesystem), Btrfs (B-tree file system)." },
  { "en": "Apa Itu Command Line Interface (CLI)?", "id": "Antarmuka Baris Perintah (Teks)." },
  { "en": "Apa Kepanjangan CLI (Command Line Interface)?", "id": "Antarmuka Baris Perintah." },
  { "en": "Apa Itu Graphical User Interface (GUI)?", "id": "Antarmuka Pengguna Grafis." },
  { "en": "Apa Kepanjangan GUI (Graphical User Interface)?", "id": "Antarmuka Pengguna Grafis." },
  { "en": "Apa Itu Shell (Shell) Sistem Operasi?", "id": "Program Antarmuka Pengguna (CLI/GUI)." },
  { "en": "Contoh Shell (Shell) Linux?", "id": "Bash (Bourne Again SHell), Zsh (Z Shell)." },
  { "en": "Apa Itu Skrip Shell (Shell Script)?", "id": "Berkas Berisi Perintah Shell Otomatisasi." },
  { "en": "Apa Itu Open Source (Sumber Terbuka)?", "id": "Perangkat Lunak Kode Sumber Terbuka." },
  { "en": "Apa Itu Lisensi Perangkat Lunak (Software License)?", "id": "Aturan Penggunaan, Distribusi Perangkat Lunak." },
  { "en": "Contoh Lisensi Open Source (Sumber Terbuka)?", "id": "GPL (General Public License), MIT (Massachusetts Institute of Technology), Apache." },
  { "en": "Apa Itu Proprietary Software (Perangkat Lunak Proprietary)?", "id": "Perangkat Lunak Sumber Tertutup." },
  { "en": "Apa Itu Freeware (Perangkat Gratis)?", "id": "Perangkat Lunak Gratis (Tanpa Kode Sumber)." },
  { "en": "Apa Itu Shareware (Perangkat Berbagi)?", "id": "Perangkat Lunak Uji Coba Gratis." },
  { "en": "Apa Itu Bug (Bug) Perangkat Lunak?", "id": "Kesalahan Atau Cacat Program." },
  { "en": "Apa Itu Debugging (Debugging)?", "id": "Proses Mencari, Memperbaiki Bug." },
  { "en": "Apa Itu Algoritma (Algorithm)?", "id": "Langkah-Langkah Solusi Masalah." },
  { "en": "Apa Itu Struktur Data (Data Structure)?", "id": "Cara Mengorganisasi Data Di Memori." },
  { "en": "Contoh Struktur Data (Data Structure)?", "id": "Array, Linked List, Stack, Queue." },
  { "en": "Apa Itu Array (Larik)?", "id": "Kumpulan Elemen Tipe Sama Berurutan." },
  { "en": "Apa Itu Linked List (Senarai Berantai)?", "id": "Kumpulan Elemen (Node) Terhubung Pointer." },
  { "en": "Apa Itu Stack (Tumpukan)?", "id": "Struktur Data LIFO (Last In First Out)." },
  { "en": "Apa Itu Queue (Antrean)?", "id": "Struktur Data FIFO (First In First Out)." },
  { "en": "Apa Itu Tree (Pohon)?", "id": "Struktur Data Hierarkis." },
  { "en": "Apa Itu Binary Tree (Pohon Biner)?", "id": "Pohon (Setiap Node Maksimal Dua Anak)." },
  { "en": "Apa Itu Graph (Graf)?", "id": "Struktur Data (Simpul, Sisi)." },
  { "en": "Apa Itu Hash Table (Tabel Hash)?", "id": "Struktur Data Pencarian Cepat (Kunci-Nilai)." },
  { "en": "Apa Itu Kompleksitas Waktu (Time Complexity)?", "id": "Ukuran Efisiensi Waktu Eksekusi Algoritma." },
  { "en": "Apa Itu Notasi Big O (Big O Notation)?", "id": "Notasi Asimtotik Kompleksitas Algoritma." },
  { "en": "Apa Itu Kompleksitas Ruang (Space Complexity)?", "id": "Ukuran Penggunaan Memori Algoritma." },
  { "en": "Apa Itu Algoritma Pengurutan (Sorting Algorithm)?", "id": "Algoritma Mengurutkan Data." },
  { "en": "Contoh Algoritma Pengurutan (Sorting Algorithm)?", "id": "Bubble Sort, Merge Sort, Quick Sort." },
  { "en": "Apa Itu Algoritma Pencarian (Searching Algorithm)?", "id": "Algoritma Menemukan Data." },
  { "en": "Contoh Algoritma Pencarian (Searching Algorithm)?", "id": "Linear Search, Binary Search." },
  { "en": "Kapan Binary Search (Pencarian Biner) Efektif?", "id": "Hanya Pada Data Yang Terurut." },
  { "en": "Apa Itu Rekursi (Recursion)?", "id": "Fungsi Memanggil Dirinya Sendiri." },
  { "en": "Apa Itu Pemrograman Berorientasi Objek (OOP)?", "id": "Object-Oriented Programming (OOP)." },
  { "en": "Apa Kepanjangan OOP (Object-Oriented Programming)?", "id": "Pemrograman Berorientasi Objek." },
  { "en": "Apa Konsep Utama OOP (Object-Oriented Programming)?", "id": "Kelas, Objek, Enkapsulasi, Pewarisan, Polimorfisme." },
  { "en": "Apa Itu Kelas (Class) OOP?", "id": "Blueprint (Cetakan Biru) Objek." },
  { "en": "Apa Itu Objek (Object) OOP?", "id": "Instansi (Instance) Dari Kelas." },
  { "en": "Apa Itu Enkapsulasi (Encapsulation) OOP?", "id": "Menyembunyikan Detail Implementasi Internal." },
  { "en": "Apa Itu Pewarisan (Inheritance) OOP?", "id": "Kelas Anak Mewarisi Sifat Kelas Induk." },
  { "en": "Apa Itu Polimorfisme (Polymorphism) OOP?", "id": "Kemampuan Objek Berbeda Respon Sama." },
  { "en": "Contoh Bahasa Pemrograman OOP (Object-Oriented Programming)?", "id": "Java, C++, Python." },
  { "en": "Apa Itu Garbage Collection (Pengumpulan Sampah)?", "id": "Manajemen Memori Otomatis (Hapus Objek)." },
  { "en": "Apa Itu Exception Handling (Penanganan Eksepsi)?", "id": "Mekanisme Penanganan Error Saat Runtime." },
  { "en": "Apa Itu Pemrograman Fungsional (Functional Programming)?", "id": "Paradigma Pemrograman (Fungsi Murni)." },
  { "en": "Apa Itu Lambda Expression (Ekspresi Lambda)?", "id": "Fungsi Anonim Singkat." },
  { "en": "Apa Itu Unit Testing Framework (Kerangka Pengujian Unit)?", "id": "Alat Bantu Penulisan, Eksekusi Tes Unit." },
  { "en": "Contoh Unit Testing Framework (Kerangka Pengujian Unit)?", "id": "JUnit (Java), PyTest (Python)." },
  { "en": "Apa Itu Test-Driven Development (TDD)?", "id": "Pengembangan Berbasis Tes." },
  { "en": "Apa Kepanjangan TDD (Test-Driven Development)?", "id": "Pengembangan Berbasis Tes." },
  { "en": "Apa Alur Kerja TDD (Test-Driven Development)?", "id": "Red (Tes Gagal), Green (Tes Lulus), Refactor." },
  { "en": "Apa Itu Refactoring (Refaktorisasi) Kode?", "id": "Memperbaiki Struktur Kode Tanpa Ubah Fungsi." },
  { "en": "Apa Itu Kode Bersih (Clean Code)?", "id": "Kode Mudah Dibaca, Dimengerti, Dipelihara." },
  { "en": "Apa Itu Prinsip SOLID (SOLID Principles)?", "id": "Lima Prinsip Desain Berorientasi Objek." },
  { "en": "Apa Prinsip S (Single Responsibility) SOLID?", "id": "Satu Kelas, Satu Tanggung Jawab." },
  { "en": "Apa Prinsip O (Open/Closed) SOLID?", "id": "Terbuka Ekstensi, Tertutup Modifikasi." },
  { "en": "Apa Prinsip L (Liskov Substitution) SOLID?", "id": "Objek Anak Gantikan Objek Induk." },
  { "en": "Apa Prinsip I (Interface Segregation) SOLID?", "id": "Antarmuka Spesifik Klien (Tidak Gemuk)." },
  { "en": "Apa Prinsip D (Dependency Inversion) SOLID?", "id": "Bergantung Abstraksi, Bukan Implementasi." },
  { "en": "Apa Itu Pola Desain (Design Pattern)?", "id": "Solusi Umum Masalah Desain Software." },
  { "en": "Contoh Pola Desain (Design Pattern) Kreasi?", "id": "Singleton, Factory, Builder." },
  { "en": "Contoh Pola Desain (Design Pattern) Struktural?", "id": "Adapter, Decorator, Facade." },
  { "en": "Contoh Pola Desain (Design Pattern) Perilaku?", "id": "Observer, Strategy, Command." },
  { "en": "Apa Itu Pola Singleton (Singleton Pattern)?", "id": "Memastikan Kelas Hanya Satu Instansi." },
  { "en": "Apa Itu Pola Observer (Observer Pattern)?", "id": "Objek Berlangganan Notifikasi Perubahan." },
  { "en": "Apa Itu Antipattern (Antipattern)?", "id": "Solusi Umum Yang Buruk." },
  { "en": "Apa Itu Kode Spaghetti (Spaghetti Code)?", "id": "Kode Tidak Terstruktur, Sulit Diikuti." },
  { "en": "Apa Itu Kode Duplikat (Duplicate Code)?", "id": "Blok Kode Sama Muncul Berulang." },
  { "en": "Apa Itu Magic Number (Angka Ajaib)?", "id": "Angka Literal Tanpa Nama Konstanta." },
  { "en": "Apa Itu Komentar Kode (Code Comment)?", "id": "Penjelasan Tambahan Dalam Kode Sumber." },
  { "en": "Kapan Komentar Kode (Code Comment) Diperlukan?", "id": "Menjelaskan 'Mengapa', Bukan 'Apa'." },
  { "en": "Apa Itu Dokumentasi (Documentation) Perangkat Lunak?", "id": "Deskripsi Cara Kerja, Penggunaan Software." },
  { "en": "Apa Itu UML (Unified Modeling Language)?", "id": "Bahasa Pemodelan Visual Perangkat Lunak." },
  { "en": "Apa Kepanjangan UML (Unified Modeling Language)?", "id": "Bahasa Pemodelan Terpadu." },
  { "en": "Contoh Diagram UML (Unified Modeling Language)?", "id": "Class Diagram, Use Case Diagram." },
  { "en": "Apa Itu Diagram Kelas (Class Diagram)?", "id": "Diagram Struktur Kelas, Atribut, Metode." },
  { "en": "Apa Itu Diagram Kasus Penggunaan (Use Case Diagram)?", "id": "Diagram Interaksi Aktor, Sistem." },
  { "en": "Apa Itu Rekayasa Balik (Reverse Engineering)?", "id": "Menganalisis Produk Pahami Cara Kerja." },
  { "en": "Apa Itu Keamanan Siber (Cybersecurity)?", "id": "Praktik Melindungi Sistem Dari Serangan." },
  { "en": "Apa Itu Malware (Malware)?", "id": "Perangkat Lunak Berbahaya." },
  { "en": "Contoh Malware (Malware)?", "id": "Virus, Worm, Trojan, Ransomware." },
  { "en": "Apa Itu Virus (Virus) Komputer?", "id": "Malware (Malware) Replikasi Diri Sisip Kode." },
  { "en": "Apa Itu Worm (Cacing) Komputer?", "id": "Malware (Malware) Replikasi Diri Lewat Jaringan." },
  { "en": "Apa Itu Trojan Horse (Kuda Troya)?", "id": "Malware (Malware) Menyamar Program Sah." },
  { "en": "Apa Itu Ransomware (Ransomware)?", "id": "Malware (Malware) Enkripsi Data Minta Tebusan." },
  { "en": "Apa Itu Spyware (Spyware)?", "id": "Malware (Malware) Memata-Matai Aktivitas Pengguna." },
  { "en": "Apa Itu Adware (Adware)?", "id": "Malware (Malware) Menampilkan Iklan Tidak Diinginkan." },
  { "en": "Apa Itu Phishing (Phishing)?", "id": "Upaya Penipuan Dapatkan Informasi Sensitif." },
  { "en": "Apa Itu Serangan DDoS (Distributed Denial of Service)?", "id": "Membanjiri Server Hingga Tidak Responsif." },
  { "en": "Apa Kepanjangan DDoS (Distributed Denial of Service)?", "id": "Penolakan Layanan Terdistribusi." },
  { "en": "Apa Itu Serangan Man-in-the-Middle (MITM)?", "id": "Penyadapan Komunikasi Antara Dua Pihak." },
  { "en": "Apa Kepanjangan MITM (Man-in-the-Middle)?", "id": "Orang Di Tengah." },
  { "en": "Apa Itu Injeksi SQL (SQL Injection)?", "id": "Serangan Memanipulasi Kueri Basis Data." },
  { "en": "Apa Itu Cross-Site Scripting (XSS)?", "id": "Menyisipkan Skrip Berbahaya Ke Situs Web." },
  { "en": "Apa Kepanjangan XSS (Cross-Site Scripting)?", "id": "Skrip Lintas Situs." },
  { "en": "Apa Itu Firewall (Firewall)?", "id": "Sistem Keamanan Penyaring Lalu Lintas Jaringan." },
  { "en": "Apa Itu Antivirus (Antivirus)?", "id": "Perangkat Lunak Deteksi, Hapus Malware." },
  { "en": "Apa Itu VPN (Virtual Private Network)?", "id": "Jaringan Pribadi Virtual (Koneksi Aman)." },
  { "en": "Apa Itu Enkripsi (Encryption)?", "id": "Proses Pengkodean Data Agar Aman." },
  { "en": "Apa Itu Kunci Publik (Public Key) Kriptografi?", "id": "Kunci Untuk Enkripsi (Dibagikan)." },
  { "en": "Apa Itu Kunci Privat (Private Key) Kriptografi?", "id": "Kunci Untuk Dekripsi (Dirahasiakan)." },
  { "en": "Apa Itu Tanda Tangan Digital (Digital Signature)?", "id": "Memastikan Keaslian, Integritas Dokumen Digital." },
  { "en": "Apa Itu Sertifikat Digital (Digital Certificate)?", "id": "Mengikat Kunci Publik Ke Identitas." },
  { "en": "Apa Itu Otoritas Sertifikat (Certificate Authority) (CA)?", "id": "Penerbit Terpercaya Sertifikat Digital." },
  { "en": "Apa Itu Autentikasi (Authentication)?", "id": "Proses Verifikasi Identitas Pengguna." },
  { "en": "Apa Itu Otorisasi (Authorization)?", "id": "Proses Pemberian Hak Akses." },
  { "en": "Apa Itu Autentikasi Dua Faktor (2FA)?", "id": "Two-Factor Authentication (2FA) (Dua Lapisan Verifikasi)." },
  { "en": "Apa Kepanjangan 2FA (Two-Factor Authentication)?", "id": "Autentikasi Dua Faktor." },
  { "en": "Apa Itu Biometrik (Biometrics)?", "id": "Autentikasi Berbasis Ciri Fisik (Sidik Jari)." },
  { "en": "Apa Itu Hashing (Hashing)?", "id": "Fungsi Satu Arah (Ubah Data Unik)." },
  { "en": "Apa Fungsi Hashing (Hashing) Kata Sandi?", "id": "Menyimpan Kata Sandi Secara Aman." },
  { "en": "Apa Itu Salt (Salt) Dalam Hashing?", "id": "Data Acak Tambahan Sebelum Hashing." },
  { "en": "Apa Itu Forensik Digital (Digital Forensics)?", "id": "Investigasi Kejahatan Digital." },
  { "en": "Apa Itu Pengujian Penetrasi (Penetration Testing)?", "id": "Simulasi Serangan Temukan Kerentanan." },
  { "en": "Apa Nama Lain Pengujian Penetrasi (Penetration Testing)?", "id": "Pen Testing (Pentest) Atau Ethical Hacking." },
  { "en": "Apa Itu Kerentanan (Vulnerability)?", "id": "Kelemahan Sistem Keamanan." },
  { "en": "Apa Itu Eksploit (Exploit)?", "id": "Kode Memanfaatkan Kerentanan Sistem." },
  { "en": "Apa Itu Zero-Day Exploit (Eksploit Hari Nol)?", "id": "Eksploit Untuk Kerentanan Belum Diketahui." },
  { "en": "Apa Itu Patch (Tambalan) Keamanan?", "id": "Perbaikan Perangkat Lunak Menutup Kerentanan." },
  { "en": "Apa Itu Social Engineering (Rekayasa Sosial)?", "id": "Manipulasi Psikologis Dapatkan Informasi." },
  { "en": "Contoh Social Engineering (Rekayasa Sosial)?", "id": "Phishing (Phishing), Pretexting (Pretexting)." },
  { "en": "Apa Itu Kebijakan Keamanan (Security Policy)?", "id": "Aturan, Prosedur Keamanan Organisasi." },
  { "en": "Apa Itu Manajemen Risiko (Risk Management)?", "id": "Identifikasi, Penilaian, Mitigasi Risiko." },
  { "en": "Apa Itu Penilaian Risiko (Risk Assessment)?", "id": "Proses Evaluasi Potensi Ancaman." },
  { "en": "Apa Itu Rencana Pemulihan Bencana (DRP)?", "id": "Disaster Recovery Plan (DRP)." },
  { "en": "Apa Kepanjangan DRP (Disaster Recovery Plan)?", "id": "Rencana Pemulihan Bencana." },
  { "en": "Apa Itu Rencana Kelangsungan Bisnis (BCP)?", "id": "Business Continuity Plan (BCP)." },
  { "en": "Apa Kepanjangan BCP (Business Continuity Plan)?", "id": "Rencana Kelangsungan Bisnis." },
  { "en": "Apa Itu Cadangan Data (Data Backup)?", "id": "Menyalin Data Untuk Pemulihan." },
  { "en": "Apa Itu Cadangan Penuh (Full Backup)?", "id": "Menyalin Semua Data Dipilih." },
  { "en": "Apa Itu Cadangan Inkremental (Incremental Backup)?", "id": "Menyalin Data Berubah Sejak Cadangan Terakhir." },
  { "en": "Apa Itu Cadangan Diferensial (Differential Backup)?", "id": "Menyalin Data Berubah Sejak Cadangan Penuh." },
  { "en": "Apa Itu Redundansi (Redundancy)?", "id": "Duplikasi Komponen Kritis Sistem." },
  { "en": "Apa Itu RAID (Redundant Array of Independent Disks)?", "id": "Teknologi Penyimpanan Disk Redundan." },
  { "en": "Apa Kepanjangan RAID (Redundant Array of Independent Disks)?", "id": "Array Redundan Disk Independen." },
  { "en": "Apa Itu RAID 0 (RAID 0)?", "id": "Striping (Striping) (Tanpa Redundansi)." },
  { "en": "Apa Itu RAID 1 (RAID 1)?", "id": "Mirroring (Mirroring) (Duplikasi Disk)." },
  { "en": "Apa Itu RAID 5 (RAID 5)?", "id": "Striping (Striping) Dengan Paritas Terdistribusi." },
  { "en": "Apa Itu RAID 6 (RAID 6)?", "id": "Striping (Striping) Dengan Paritas Ganda." },
  { "en": "Apa Itu Toleransi Kesalahan (Fault Tolerance)?", "id": "Kemampuan Sistem Tetap Operasi Saat Gagal." },
  { "en": "Apa Itu Ketersediaan Tinggi (High Availability)?", "id": "Sistem Minim Downtime (Waktu Henti)." },
  { "en": "Apa Itu Load Balancing (Penyeimbangan Beban)?", "id": "Membagi Lalu Lintas Jaringan/Server." },
  { "en": "Apa Itu Klaster (Cluster) Komputer?", "id": "Sekelompok Komputer Bekerja Bersama." },
  { "en": "Apa Itu Failover (Failover)?", "id": "Pengalihan Otomatis Ke Sistem Cadangan." },
  { "en": "Apa Itu Pusat Data (Data Center)?", "id": "Fasilitas Penyimpanan, Pengelolaan Server." },
  { "en": "Apa Itu Pendinginan Pusat Data (Data Center Cooling)?", "id": "Sistem Menjaga Suhu Optimal Server." },
  { "en": "Apa Itu Hot Aisle / Cold Aisle?", "id": "Tata Letak Rak Server Efisiensi Pendinginan." },
  { "en": "Apa Itu PUE (Power Usage Effectiveness)?", "id": "Ukuran Efisiensi Energi Pusat Data." },
  { "en": "Apa Kepanjangan PUE (Power Usage Effectiveness)?", "id": "Efektivitas Penggunaan Daya." },
  { "en": "Berapa Nilai PUE (Power Usage Effectiveness) Ideal?", "id": "Mendekati 1.0 (Sangat Efisien)." },
  { "en": "Apa Itu Virtualisasi (Virtualization)?", "id": "Membuat Versi Virtual Sumber Daya Komputer." },
  { "en": "Apa Itu Mesin Virtual (Virtual Machine) (VM)?", "id": "Simulasi Komputer Di Dalam Komputer." },
  { "en": "Apa Kepanjangan VM (Virtual Machine)?", "id": "Mesin Virtual." },
  { "en": "Apa Itu Hypervisor (Hypervisor)?", "id": "Perangkat Lunak Pembuat, Pengelola VM." },
  { "en": "Contoh Hypervisor (Hypervisor)?", "id": "VMware ESXi, Microsoft Hyper-V, KVM." },
  { "en": "Apa Itu Penyimpanan Terpasang Jaringan (NAS)?", "id": "Network Attached Storage (NAS)." },
  { "en": "Apa Kepanjangan NAS (Network Attached Storage)?", "id": "Penyimpanan Terpasang Jaringan." },
  { "en": "Apa Itu Jaringan Area Penyimpanan (SAN)?", "id": "Storage Area Network (SAN)." },
  { "en": "Apa Kepanjangan SAN (Storage Area Network)?", "id": "Jaringan Area Penyimpanan." },
  { "en": "Apa Beda NAS (Network Attached Storage) Dan SAN (Storage Area Network)?", "id": "NAS (Network Attached Storage) (Level File), SAN (Storage Area Network) (Level Blok)." },
  { "en": "Apa Itu Fiber Channel (FC)?", "id": "Protokol Jaringan Kecepatan Tinggi (SAN)." },
  { "en": "Apa Kepanjangan FC (Fiber Channel)?", "id": "Saluran Serat." },
  { "en": "Apa Itu iSCSI (Internet Small Computer System Interface)?", "id": "Protokol Penyimpanan Blok Lewat IP." },
  { "en": "Apa Kepanjangan iSCSI (Internet Small Computer System Interface)?", "id": "Antarmuka Sistem Komputer Kecil Internet." },
  { "en": "Apa Itu RAID (Redundant Array of Independent Disks) Perangkat Keras?", "id": "RAID (Redundant Array of Independent Disks) Dikelola Kontroler Khusus." },
  { "en": "Apa Itu RAID (Redundant Array of Independent Disks) Perangkat Lunak?", "id": "RAID (Redundant Array of Independent Disks) Dikelola Sistem Operasi." },
  { "en": "Apa Itu Hot Swapping (Hot Swapping)?", "id": "Mengganti Komponen Saat Sistem Jalan." },
  { "en": "Disk Apa Yang Mendukung Hot Swapping (Hot Swapping)?", "id": "Disk SCSI (Small Computer System Interface), SAS (Serial Attached SCSI), SATA (Serial ATA) Tertentu." },
  { "en": "Apa Kepanjangan SCSI (Small Computer System Interface)?", "id": "Antarmuka Sistem Komputer Kecil." },
  { "en": "Apa Kepanjangan SAS (Serial Attached SCSI)?", "id": "SCSI (Small Computer System Interface) Terpasang Serial." },
  { "en": "Apa Kepanjangan SATA (Serial Advanced Technology Attachment)?", "id": "Lampiran Teknologi Canggih Serial." },
  { "en": "Apa Itu Motherboard (Papan Induk)?", "id": "Papan Sirkuit Utama Komputer." },
  { "en": "Apa Itu Chipset (Chipset) Motherboard?", "id": "Kumpulan IC (Integrated Circuit) Pengatur Komunikasi." },
  { "en": "Apa Itu Northbridge (Northbridge) Chipset (Lama)?", "id": "Mengatur Komunikasi Cepat (CPU, RAM, GPU)." },
  { "en": "Apa Itu Southbridge (Southbridge) Chipset (Lama)?", "id": "Mengatur Komunikasi Lambat (I/O, PCI)." },
  { "en": "Apa Itu BIOS (Basic Input Output System)?", "id": "Firmware (Perangkat Tegar) Inisialisasi Booting." },
  { "en": "Apa Itu UEFI (Unified Extensible Firmware Interface)?", "id": "Pengganti BIOS (Basic Input Output System) Modern." },
  { "en": "Apa Itu CMOS (Complementary Metal-Oxide-Semiconductor) Setup?", "id": "Pengaturan Konfigurasi BIOS (Basic Input Output System)/UEFI." },
  { "en": "Apa Itu Baterai CMOS (CMOS Battery)?", "id": "Baterai Penyimpan Pengaturan CMOS." },
  { "en": "Apa Itu Slot Ekspansi (Expansion Slot)?", "id": "Slot Tambahan Kartu (GPU, Sound)." },
  { "en": "Apa Kepanjangan PCI (Peripheral Component Interconnect)?", "id": "Interkoneksi Komponen Periferal." },
  { "en": "Apa Kepanjangan PCIe (PCI Express)?", "id": "PCI (Peripheral Component Interconnect) Express (Lebih Cepat)." },
  { "en": "Apa Itu Kartu Grafis (Graphics Card)?", "id": "Pengolah Grafis (GPU)." },
  { "en": "Apa Kepanjangan GPU (Graphics Processing Unit)?", "id": "Unit Pemroses Grafis." },
  { "en": "Apa Itu VRAM (Video RAM)?", "id": "Memori Khusus Kartu Grafis." },
  { "en": "Apa Itu Unit Catu Daya (PSU)?", "id": "Power Supply Unit (PSU)." },
  { "en": "Apa Kepanjangan PSU (Power Supply Unit)?", "id": "Unit Catu Daya." },
  { "en": "Apa Fungsi PSU (Power Supply Unit)?", "id": "Mengubah AC Ke DC Komponen Komputer." },
  { "en": "Apa Itu Peringkat Efisiensi 80 Plus?", "id": "Standar Efisiensi PSU (Power Supply Unit)." },
  { "en": "Apa Itu Pendingin CPU (CPU Cooler)?", "id": "Pendingin Prosesor (Udara Atau Cair)." },
  { "en": "Apa Itu Antarmuka M.2 (M.2 Interface)?", "id": "Konektor SSD (Solid State Drive) Kecepatan Tinggi." },
  { "en": "Apa Kepanjangan NVMe (Non-Volatile Memory Express)?", "id": "Protokol SSD (Solid State Drive) M.2 Cepat." },
  { "en": "Apa Itu Memori ECC (Error-Correcting Code)?", "id": "Memori RAM (Random Access Memory) Deteksi, Koreksi Error." },
  { "en": "Dimana Memori ECC (Error-Correcting Code) Digunakan?", "id": "Server Dan Workstation." },
  { "en": "Apa Itu Overclocking (Overclocking)?", "id": "Menjalankan Komponen Lebih Cepat Dari Standar." },
  { "en": "Apa Itu Underclocking (Underclocking)?", "id": "Menjalankan Komponen Lebih Lambat (Hemat Daya)." },
  { "en": "Apa Itu Latensi (Latency) RAM?", "id": "Waktu Tunda Akses Memori RAM." },
  { "en": "Apa Itu Dual Channel (Kanal Ganda) RAM?", "id": "Menggunakan Dua Modul RAM Bersamaan." },
  { "en": "Apa Keuntungan Dual Channel (Kanal Ganda) RAM?", "id": "Meningkatkan Bandwidth (Lebar Pita) Memori." },
  { "en": "Apa Itu SO-DIMM (Small Outline DIMM)?", "id": "Modul RAM (Random Access Memory) Ukuran Laptop." },
  { "en": "Apa Kepanjangan DIMM (Dual In-line Memory Module)?", "id": "Modul Memori Dual In-line." },
  { "en": "Apa Itu Hard Disk Drive (HDD)?", "id": "Penyimpanan Magnetik Berbasis Piringan." },
  { "en": "Apa Kepanjangan HDD (Hard Disk Drive)?", "id": "Penggerak Cakram Keras." },
  { "en": "Apa Itu Solid State Drive (SSD)?", "id": "Penyimpanan Berbasis Memori Flash." },
  { "en": "Apa Keunggulan SSD (Solid State Drive) Dibanding HDD?", "id": "Jauh Lebih Cepat, Tahan Guncangan." },
  { "en": "Apa Itu Kecepatan Putar (RPM) HDD?", "id": "Rotations Per Minute (RPM) Piringan." },
  { "en": "Apa Kepanjangan RPM (Rotations Per Minute)?", "id": "Rotasi Per Menit." },
  { "en": "Apa Itu Defragmentasi (Defragmentation) Disk?", "id": "Menata Ulang Data Disk HDD." },
  { "en": "Apakah SSD (Solid State Drive) Perlu Defragmentasi?", "id": "Tidak Perlu, Bahkan Memperpendek Umur." },
  { "en": "Apa Itu TRIM (TRIM Command) SSD?", "id": "Perintah Optimasi Kinerja SSD." },
  { "en": "Apa Itu Umur Tulis (Write Endurance) SSD?", "id": "Batas Jumlah Data Dapat Ditulis SSD." },
  { "en": "Apa Satuan Umur Tulis (Write Endurance) SSD?", "id": "Terabytes Written (TBW)." },
  { "en": "Apa Itu Wear Leveling (Perataan Keausan) SSD?", "id": "Distribusi Penulisan Data Merata Sel Flash." },
  { "en": "Apa Itu Sel Memori SLC (Single-Level Cell)?", "id": "Satu Bit Per Sel (Cepat, Awet)." },
  { "en": "Apa Itu Sel Memori MLC (Multi-Level Cell)?", "id": "Dua Bit Per Sel." },
  { "en": "Apa Itu Sel Memori TLC (Triple-Level Cell)?", "id": "Tiga Bit Per Sel (Murah, Kapasitas)." },
  { "en": "Apa Itu Sel Memori QLC (Quad-Level Cell)?", "id": "Empat Bit Per Sel (Kapasitas Besar)." },
  { "en": "Mana Yang Paling Awet, SLC (Single-Level Cell) Atau QLC (Quad-Level Cell)?", "id": "SLC (Single-Level Cell) Paling Awet." },
  { "en": "Apa Itu USB (Universal Serial Bus)?", "id": "Antarmuka Serial Standar Periferal." },
  { "en": "Apa Kepanjangan USB (Universal Serial Bus)?", "id": "Bus Serial Universal." },
  { "en": "Apa Itu USB (Universal Serial Bus) Tipe-A?", "id": "Konektor USB (Universal Serial Bus) Persegi Panjang Standar." },
  { "en": "Apa Itu USB (Universal Serial Bus) Tipe-C?", "id": "Konektor USB (Universal Serial Bus) Oval Reversibel." },
  { "en": "Apa Itu Thunderbolt (Thunderbolt)?", "id": "Antarmuka Kecepatan Tinggi (Intel, Apple)." },
  { "en": "Apa Itu HDMI (High-Definition Multimedia Interface)?", "id": "Antarmuka Audio/Video Digital." },
  { "en": "Apa Kepanjangan HDMI (High-Definition Multimedia Interface)?", "id": "Antarmuka Multimedia Definisi Tinggi." },
  { "en": "Apa Itu DisplayPort (DisplayPort)?", "id": "Antarmuka Video Digital (Alternatif HDMI)." },
  { "en": "Apa Itu DVI (Digital Visual Interface)?", "id": "Antarmuka Video Digital (Lebih Tua)." },
  { "en": "Apa Kepanjangan DVI (Digital Visual Interface)?", "id": "Antarmuka Visual Digital." },
  { "en": "Apa Itu VGA (Video Graphics Array)?", "id": "Antarmuka Video Analog (Sudah Usang)." },
  { "en": "Apa Kepanjangan VGA (Video Graphics Array)?", "id": "Array Grafis Video." },
  { "en": "Apa Itu Bluetooth (Bluetooth)?", "id": "Standar Komunikasi Nirkabel Jarak Pendek." },
  { "en": "Apa Itu Wi-Fi Direct (Wi-Fi Direct)?", "id": "Koneksi Nirkabel Langsung Antar Perangkat." },
  { "en": "Apa Itu Near Field Communication (NFC)?", "id": "Komunikasi Nirkabel Jarak Sangat Dekat." },
  { "en": "Apa Itu Keyboard (Papan Ketik)?", "id": "Perangkat Input Teks Komputer." },
  { "en": "Apa Itu Mouse (Tetikus)?", "id": "Perangkat Input Penunjuk Grafis." },
  { "en": "Apa Itu Monitor (Layar)?", "id": "Perangkat Output Visual Komputer." },
  { "en": "Apa Itu Resolusi (Resolution) Layar?", "id": "Jumlah Piksel Horizontal, Vertikal." },
  { "en": "Apa Itu Refresh Rate (Laju Penyegaran) Layar?", "id": "Seberapa Cepat Gambar Diperbarui (Hz)." },
  { "en": "Apa Itu Response Time (Waktu Respon) Layar?", "id": "Waktu Piksel Berubah Warna (ms)." },
  { "en": "Apa Itu Panel Layar TN (Twisted Nematic)?", "id": "Panel Murah, Waktu Respon Cepat." },
  { "en": "Apa Itu Panel Layar IPS (In-Plane Switching)?", "id": "Panel Warna Akurat, Sudut Pandang Luas." },
  { "en": "Apa Itu Panel Layar VA (Vertical Alignment)?", "id": "Panel Kontras Tinggi." },
  { "en": "Apa Itu Printer (Pencetak)?", "id": "Perangkat Output Mencetak Dokumen." },
  { "en": "Apa Itu Printer Inkjet (Inkjet)?", "id": "Printer Semprot Tinta Cair." },
  { "en": "Apa Itu Printer Laser (Laser)?", "id": "Printer Menggunakan Bubuk Toner, Laser." },
  { "en": "Apa Itu Scanner (Pemindai)?", "id": "Perangkat Input Memindai Dokumen/Gambar." },
  { "en": "Apa Itu Webcam (Kamera Web)?", "id": "Kamera Video Terhubung Komputer." },
  { "en": "Apa Itu Sistem Pendingin Cair (Liquid Cooling)?", "id": "Pendingin Komponen Komputer Cairan." },
  { "en": "Apa Itu Radiator (Radiator) Pendingin Cair?", "id": "Penukar Panas Cairan Ke Udara." },
  { "en": "Apa Itu Pompa (Pump) Pendingin Cair?", "id": "Mensirkulasikan Cairan Pendingin." },
  { "en": "Apa Itu Blok Air (Water Block)?", "id": "Komponen Menyerap Panas (CPU/GPU)." },
  { "en": "Apa Itu Cadangan Baterai (UPS)?", "id": "Uninterruptible Power Supply (UPS)." },
  { "en": "Apa Fungsi UPS (Uninterruptible Power Supply)?", "id": "Menyediakan Daya Darurat Saat Listrik Padam." },
  { "en": "Apa Itu Pelindung Lonjakan (Surge Protector)?", "id": "Melindungi Perangkat Dari Lonjakan Tegangan." },
  { "en": "Apa Beda UPS (Uninterruptible Power Supply) Dan Surge Protector?", "id": "UPS (Uninterruptible Power Supply) Punya Baterai Cadangan." },
  { "en": "Apa Itu Benchmarking (Benchmarking) Komputer?", "id": "Mengukur Kinerja Komponen Komputer." },
  { "en": "Perangkat Lunak Apa Untuk Benchmarking (Benchmarking) CPU?", "id": "Cinebench, Geekbench." },
  { "en": "Perangkat Lunak Apa Untuk Benchmarking (Benchmarking) GPU?", "id": "3DMark, FurMark." },
  { "en": "Apa Itu Stabilitas Sistem (System Stability)?", "id": "Kemampuan Komputer Berjalan Tanpa Crash." },
  { "en": "Apa Itu Blue Screen of Death (BSOD)?", "id": "Layar Error Fatal Windows." },
  { "en": "Apa Kepanjangan BSOD (Blue Screen of Death)?", "id": "Layar Biru Kematian." },
  { "en": "Apa Itu Kernel Panic (Kernel Panic)?", "id": "Error Fatal Sistem Operasi Unix-like." },
  { "en": "Apa Itu Task Manager (Pengelola Tugas) Windows?", "id": "Utilitas Pemantau Proses, Kinerja." },
  { "en": "Apa Itu Activity Monitor (Monitor Aktivitas) MacOS?", "id": "Utilitas Pemantau Proses MacOS." },
  { "en": "Apa Itu Periferal (Peripheral) Komputer?", "id": "Perangkat Eksternal Terhubung Komputer." },
  { "en": "Contoh Periferal (Peripheral) Input?", "id": "Keyboard (Papan Ketik), Mouse (Tetikus), Scanner (Pemindai)." },
  { "en": "Contoh Periferal (Peripheral) Output?", "id": "Monitor (Layar), Printer (Pencetak), Speaker (Pengeras Suara)." },
  { "en": "Apa Itu Kabel Pita (Ribbon Cable)?", "id": "Kabel Datar Banyak Konduktor Paralel." },
  { "en": "Dimana Kabel Pita (Ribbon Cable) Digunakan?", "id": "Koneksi Internal Komputer (IDE Lama)." },
  { "en": "Apa Kepanjangan IDE (Integrated Drive Electronics)?", "id": "Elektronik Penggerak Terpadu (Antarmuka HDD)." },
  { "en": "Apa Itu Konektor IDC (Insulation Displacement Connector)?", "id": "Konektor Kabel Pita (Tanpa Kupas)." },
  { "en": "Apa Kepanjangan IDC (Insulation Displacement Connector)?", "id": "Konektor Perpindahan Isolasi." },
  { "en": "Apa Itu Jumper (Jumper)?", "id": "Penghubung Singkat Pin Konfigurasi." },
  { "en": "Apa Itu Saklar DIP (Dual In-line Package)?", "id": "Saklar Kecil Paket DIP (Konfigurasi)." },
  { "en": "Apa Kepanjangan DIP (Dual In-line Package)?", "id": "Paket Dual In-line." },
  { "en": "Apa Itu Breadboard (Papan Roti)?", "id": "Papan Prototipe Elektronik Tanpa Solder." },
  { "en": "Bagaimana Koneksi Lubang Breadboard (Papan Roti)?", "id": "Lubang Baris Sama Terhubung Vertikal." },
  { "en": "Apa Itu Jalur Bus (Bus Strips) Breadboard?", "id": "Jalur Horizontal Panjang (Daya, Ground)." },
  { "en": "Apa Itu Protoboard (Papan Prototipe) (Perfboard)?", "id": "Papan PCB Berlubang (Perlu Solder)." },
  { "en": "Apa Itu Wire Wrapping (Pembungkusan Kawat)?", "id": "Teknik Koneksi Kawat Tanpa Solder." },
  { "en": "Apa Itu Skematik (Schematic) Elektronik?", "id": "Diagram Rangkaian Logis (Simbol)." },
  { "en": "Apa Itu Layout (Tata Letak) PCB?", "id": "Desain Fisik Penempatan Komponen, Jalur." },
  { "en": "Apa Itu Autorouter (Autorouter) PCB?", "id": "Fitur Perangkat Lunak Buat Jalur Otomatis." },
  { "en": "Apa Itu Ground Loop (Loop Tanah) PCB?", "id": "Jalur Ground Membentuk Loop Tertutup." },
  { "en": "Mengapa Ground Loop (Loop Tanah) PCB Buruk?", "id": "Dapat Menyebabkan Noise, Interferensi." },
  { "en": "Apa Itu Grounding Bintang (Star Grounding) PCB?", "id": "Semua Ground Terhubung Satu Titik Pusat." },
  { "en": "Apa Itu Via Stitching (Jahitan Via) PCB?", "id": "Menghubungkan Area Ground Antar Lapisan." },
  { "en": "Apa Fungsi Via Stitching (Jahitan Via)?", "id": "Memperbaiki Integritas Ground, Kurangi Noise." },
  { "en": "Apa Itu Panjang Elektrik (Electrical Length)?", "id": "Panjang Fisik Relatif Kecepatan Sinyal." },
  { "en": "Apa Itu Integritas Sinyal (Signal Integrity)?", "id": "Kualitas Sinyal Listrik (Bentuk, Waktu)." },
  { "en": "Apa Masalah Integritas Sinyal (Signal Integrity)?", "id": "Refleksi, Crosstalk, Ground Bounce." },
  { "en": "Apa Itu Ground Bounce (Pantulan Ground)?", "id": "Fluktuasi Tegangan Ground Akibat Switching." },
  { "en": "Apa Itu Power Integrity (Integritas Daya)?", "id": "Kualitas Distribusi Daya Rangkaian." },
  { "en": "Apa Itu Decoupling (Decoupling)?", "id": "Menggunakan Kapasitor Stabilkan Catu Daya Lokal." },
  { "en": "Apa Itu Impedansi Karakteristik (Characteristic Impedance) Jalur?", "id": "Impedansi Jalur Transmisi PCB." },
  { "en": "Mengapa Impedansi Terkontrol (Controlled Impedance) Penting?", "id": "Pencocokan Impedansi (Matching) Sinyal Cepat." },
  { "en": "Apa Itu Kabel Koaksial Mikro (Micro Coaxial)?", "id": "Kabel Koaksial Sangat Kecil (Internal Laptop)." },
  { "en": "Apa Itu Konektor FPC (Flexible Printed Circuit)?", "id": "Konektor Kabel PCB Fleksibel." },
  { "en": "Apa Kepanjangan FPC (Flexible Printed Circuit)?", "id": "Sirkuit Tercetak Fleksibel." },
  { "en": "Apa Itu Konektor ZIF (Zero Insertion Force)?", "id": "Konektor Tanpa Gaya Pemasangan." },
  { "en": "Apa Kepanjangan ZIF (Zero Insertion Force)?", "id": "Gaya Penyisipan Nol." },
  { "en": "Apa Itu Heat Spreader (Penyebar Panas)?", "id": "Pelat Logam Sebar Panas Komponen." },
  { "en": "Apa Itu Thermal Interface Material (TIM)?", "id": "Bahan Antarmuka Termal (Pasta, Pad)." },
  { "en": "Apa Kepanjangan TIM (Thermal Interface Material)?", "id": "Material Antarmuka Termal." },
  { "en": "Apa Fungsi TIM (Thermal Interface Material)?", "id": "Meningkatkan Transfer Panas Antar Permukaan." },
  { "en": "Apa Itu Termoelektrik Generator (TEG)?", "id": "Thermoelectric Generator (TEG) (Efek Seebeck)." },
  { "en": "Apa Kepanjangan TEG (Thermoelectric Generator)?", "id": "Generator Termoelektrik." },
  { "en": "Apa Itu Energy Harvesting (Pemanenan Energi)?", "id": "Mengumpulkan Energi Kecil Dari Lingkungan." },
  { "en": "Contoh Sumber Energy Harvesting (Pemanenan Energi)?", "id": "Getaran, Cahaya, Panas, RF." },
  { "en": "Apa Itu Piezoelektrik Energy Harvesting (Pemanenan Energi)?", "id": "Memanen Energi Getaran Mekanis." },
  { "en": "Apa Itu RFID (Radio Frequency Identification) Energy Harvesting?", "id": "Tag Pasif Dapat Daya Dari Reader." },
  { "en": "Apa Itu Wireless Power Transfer (Transfer Daya Nirkabel)?", "id": "Mengirim Energi Listrik Tanpa Kabel." },
  { "en": "Metode Apa Untuk Transfer Daya Nirkabel?", "id": "Induksi Magnetik, Resonansi Magnetik, RF." },
  { "en": "Apa Itu Standar Qi (Qi Standard) Wireless Charging?", "id": "Standar Pengisian Nirkabel Induktif." },
  { "en": "Apa Itu Rectenna (Rectenna)?", "id": "Antena Penyearah (Ubah RF Ke DC)." },
  { "en": "Apa Itu Dioda Terobosan Balik Terkendali (Controlled Avalanche)?", "id": "Dioda Dirancang Operasi Daerah Avalanche." },
  { "en": "Apa Itu IGBT (Insulated Gate Bipolar Transistor)?", "id": "Gabungan MOSFET (Metal-Oxide-Semiconductor FET) Input, BJT (Bipolar Junction Transistor) Output." },
  { "en": "Apa Kepanjangan IGBT (Insulated Gate Bipolar Transistor)?", "id": "Transistor Bipolar Gerbang Terisolasi." },
  { "en": "Apa Keunggulan IGBT (Insulated Gate Bipolar Transistor)?", "id": "Switching Daya Tinggi, Kecepatan Sedang." },
  { "en": "Dimana IGBT (Insulated Gate Bipolar Transistor) Digunakan?", "id": "Inverter Daya, Drive Motor." },
  { "en": "Apa Itu Driver Gerbang (Gate Driver)?", "id": "Rangkaian Penggerak Gerbang MOSFET/IGBT." },
  { "en": "Mengapa Perlu Driver Gerbang (Gate Driver)?", "id": "Memberi Arus Cukup Switching Cepat." },
  { "en": "Apa Itu Waktu Mati (Dead Time) Inverter?", "id": "Waktu Tunda Cegah Shoot-Through." },
  { "en": "Apa Itu Shoot-Through (Tembus Tembak)?", "id": "Kondisi Saklar Atas Bawah Konduksi Bersamaan." },
  { "en": "Apa Akibat Shoot-Through (Tembus Tembak)?", "id": "Hubungan Singkat Catu Daya (Kerusakan)." },
  { "en": "Apa Itu Konverter Resonansi (Resonant Converter)?", "id": "Konverter Switching Gunakan Resonansi LC." },
  { "en": "Apa Keuntungan Konverter Resonansi (Resonant Converter)?", "id": "Switching Tegangan Nol (ZVS) / Arus Nol (ZCS)." },
  { "en": "Apa Kepanjangan ZVS (Zero Voltage Switching)?", "id": "Pengalihan Tegangan Nol." },
  { "en": "Apa Kepanjangan ZCS (Zero Current Switching)?", "id": "Pengalihan Arus Nol." },
  { "en": "Apa Keuntungan ZVS (Zero Voltage Switching) / ZCS (Zero Current Switching)?", "id": "Mengurangi Rugi Switching, EMI." },
  { "en": "Apa Itu Kapasitor Snubber (Snubber Capacitor)?", "id": "Kapasitor Meredam Lonjakan Tegangan Switching." },
  { "en": "Apa Itu Resistor Snubber (Snubber Resistor)?", "id": "Resistor Membatasi Arus Pengisian Snubber." },
  { "en": "Apa Itu Kontrol Mode Arus (Current Mode Control)?", "id": "Metode Kontrol SMPS (Switch-Mode Power Supply) Umpan Balik Arus." },
  { "en": "Apa Itu Kontrol Mode Tegangan (Voltage Mode Control)?", "id": "Metode Kontrol SMPS (Switch-Mode Power Supply) Umpan Balik Tegangan." },
  { "en": "Apa Itu Power Factor Correction (PFC)?", "id": "Koreksi Faktor Daya Aktif." },
  { "en": "Apa Kepanjangan PFC (Power Factor Correction)?", "id": "Koreksi Faktor Daya." },
  { "en": "Apa Fungsi Rangkaian PFC (Power Factor Correction)?", "id": "Membuat Input AC Seperti Beban Resistif." },
  { "en": "Apa Topologi PFC (Power Factor Correction) Umum?", "id": "Boost Converter (Konverter Penaik)." },
  { "en": "Apa Itu Arus Harmonisa (Harmonic Current)?", "id": "Komponen Arus Frekuensi Harmonisa." },
  { "en": "Standar Apa Mengatur Arus Harmonisa (Harmonic Current)?", "id": "Standar IEC 61000-3-2." },
  { "en": "Apa Itu Total Harmonic Distortion (THD) Arus?", "id": "Ukuran Distorsi Harmonisa Arus." },
  { "en": "Apa Itu Ballast (Ballast) Lampu Fluorescent?", "id": "Rangkaian Pembatas Arus, Penyala Lampu." },
  { "en": "Apa Itu Ballast (Ballast) Elektronik?", "id": "Ballast (Ballast) Frekuensi Tinggi Lebih Efisien." },
  { "en": "Apa Itu Driver (Driver) LED?", "id": "Catu Daya Khusus Lampu LED." },
  { "en": "Mengapa LED (Light Emitting Diode) Perlu Driver (Driver)?", "id": "LED (Light Emitting Diode) Sensitif Arus (Butuh Arus Konstan)." },
  { "en": "Apa Itu TRIAC (Triode for Alternating Current)?", "id": "Saklar Daya AC Dua Arah." },
  { "en": "Apa Fungsi TRIAC (Triode for Alternating Current)?", "id": "Kontrol Daya AC (Dimmer Lampu)." },
  { "en": "Apa Itu DIAC (Diode for Alternating Current)?", "id": "Komponen Pemicu (Trigger) TRIAC." },
  { "en": "Apa Itu Kontrol Sudut Fasa (Phase Angle Control)?", "id": "Metode Kontrol Daya AC (Potong Gelombang)." },
  { "en": "Apa Itu Zero Crossing Switching (Saklar Persilangan Nol)?", "id": "Menghidupkan Beban Saat Tegangan Nol." },
  { "en": "Apa Keuntungan Zero Crossing Switching (Saklar Persilangan Nol)?", "id": "Mengurangi EMI (Electromagnetic Interference) Saat Switching." },
  { "en": "Apa Itu Kontrol Burst Firing (Burst Firing Control)?", "id": "Menghidup/Matikan Beban Beberapa Siklus AC." },
  { "en": "Dimana Kontrol Burst Firing (Burst Firing Control) Digunakan?", "id": "Kontrol Pemanas Resistif." },
  { "en": "Apa Itu SCR (Silicon Controlled Rectifier)?", "id": "Saklar Daya DC Terkendali." },
  { "en": "Bagaimana Cara Mematikan SCR (Silicon Controlled Rectifier)?", "id": "Arus Anoda Dibawah Arus Holding." },
  { "en": "Apa Itu Komutasi Paksa (Forced Commutation)?", "id": "Mematikan SCR (Silicon Controlled Rectifier) Paksa (Rangkaian Tambahan)." },
  { "en": "Apa Itu Inverter Multilevel (Multilevel Inverter)?", "id": "Inverter Hasilkan Output Bertingkat." },
  { "en": "Apa Keuntungan Inverter Multilevel (Multilevel Inverter)?", "id": "Output Lebih Mirip Sinus, THD Rendah." },
  { "en": "Apa Itu Penggerak Frekuensi Variabel (VFD)?", "id": "Variable Frequency Drive (VFD)." },
  { "en": "Apa Kepanjangan VFD (Variable Frequency Drive)?", "id": "Penggerak Frekuensi Variabel." },
  { "en": "Apa Fungsi VFD (Variable Frequency Drive)?", "id": "Mengatur Kecepatan Motor Induksi AC." },
  { "en": "Bagaimana VFD (Variable Frequency Drive) Mengatur Kecepatan?", "id": "Mengubah Frekuensi Tegangan Output." },
  { "en": "Apa Itu Pengereman Regeneratif (Regenerative Braking) VFD?", "id": "Energi Pengereman Dikembalikan Ke Jaringan." },
  { "en": "Apa Itu Pengereman Dinamis (Dynamic Braking) VFD?", "id": "Energi Pengereman Dibuang Ke Resistor." },
  { "en": "Apa Itu Bypass (Bypass) Kontaktor VFD?", "id": "Menjalankan Motor Langsung Jaringan (Jika VFD Gagal)." },
  { "en": "Apa Itu Filter dV/dt (dV/dt Filter)?", "id": "Filter Output VFD (Variable Frequency Drive) Lindungi Motor." },
  { "en": "Mengapa Perlu Filter dV/dt (dV/dt Filter)?", "id": "Tegangan Switching Cepat Rusak Isolasi Motor." },
  { "en": "Apa Itu Common Mode Choke (Choke Mode Bersama) VFD?", "id": "Mengurangi Noise Mode Bersama Output VFD." },
  { "en": "Apa Itu EMC (Electromagnetic Compatibility) Filter Input VFD?", "id": "Filter Kurangi Gangguan EMI Ke Jaringan." },
  { "en": "Apa Itu Soft Starter (Soft Starter) Elektronik?", "id": "Mengurangi Tegangan Start Motor Perlahan." },
  { "en": "Apa Beda Soft Starter (Soft Starter) Dan VFD (Variable Frequency Drive)?", "id": "VFD (Variable Frequency Drive) Kontrol Kecepatan, Soft Starter Start Saja." },
  { "en": "Apa Itu Elektromagnetisme (Electromagnetism)?", "id": "Studi Hubungan Listrik, Magnet." },
  { "en": "Apa Itu Persamaan Maxwell (Maxwell's Equations)?", "id": "Empat Persamaan Dasar Elektromagnetisme." },
  { "en": "Apa Hukum Gauss (Gauss's Law) Untuk Listrik?", "id": "Fluks Listrik Terkait Muatan Tertutup." },
  { "en": "Apa Hukum Gauss (Gauss's Law) Untuk Magnet?", "id": "Fluks Magnet Total Selalu Nol." },
  { "en": "Apa Implikasi Hukum Gauss (Gauss's Law) Magnet?", "id": "Tidak Ada Monopol Magnetik." },
  { "en": "Apa Hukum Induksi Faraday (Faraday's Law)?", "id": "Perubahan Fluks Magnet Hasilkan GGL." },
  { "en": "Apa Hukum Ampere-Maxwell (Ampere-Maxwell Law)?", "id": "Medan Magnet Dihasilkan Arus, Medan Listrik." },
  { "en": "Apa Itu Arus Perpindahan (Displacement Current) Maxwell?", "id": "Kontribusi Perubahan Medan Listrik." },
  { "en": "Apa Itu Gelombang Elektromagnetik (Electromagnetic Wave)?", "id": "Getaran Medan E, B Merambat." },
  { "en": "Bagaimana Arah E (Medan Listrik), B (Medan Magnet), Propagasi?", "id": "Saling Tegak Lurus Satu Sama Lain." },
  { "en": "Apa Itu Kondisi Batas (Boundary Conditions) Elektromagnetik?", "id": "Aturan Medan E, B Antarmuka Medium." },
  { "en": "Apa Itu Vektor Poynting (Poynting Vector)?", "id": "Arah, Laju Aliran Energi Elektromagnetik." },
  { "en": "Apa Satuan Vektor Poynting (Poynting Vector)?", "id": "Watt Per Meter Persegi (W/m²)." },
  { "en": "Apa Itu Daya Radiasi (Radiated Power)?", "id": "Total Daya Dipancarkan Sumber Elektromagnetik." },
  { "en": "Apa Itu Resistansi Radiasi (Radiation Resistance) Antena?", "id": "Resistansi Ekuivalen Pemancar Daya." },
  { "en": "Apa Itu Efisiensi Radiasi (Radiation Efficiency) Antena?", "id": "Rasio Daya Radiasi, Daya Input." },
  { "en": "Apa Itu Antena Setengah Gelombang (Half-Wave Dipole)?", "id": "Antena Dipol Panjang Setengah Lambda." },
  { "en": "Apa Itu Antena Seperempat Gelombang (Quarter-Wave Monopole)?", "id": "Antena Monopol Panjang Seperempat Lambda." },
  { "en": "Apa Fungsi Ground Plane (Bidang Tanah) Antena?", "id": "Bertindak Sebagai Cermin (Reflektor)." },
  { "en": "Apa Itu Polarisasi Silang (Cross-Polarization)?", "id": "Komponen Polarisasi Yang Tidak Diinginkan." },
  { "en": "Apa Itu Diskriminasi Polarisasi Silang (XPD)?", "id": "Ukuran Kemampuan Antena Pisahkan Polarisasi." },
  { "en": "Apa Kepanjangan XPD (Cross-Polarization Discrimination)?", "id": "Diskriminasi Polarisasi Silang." },
  { "en": "Apa Itu Antena Array (Antenna Array)?", "id": "Sekumpulan Elemen Antena Bekerja Sama." },
  { "en": "Apa Tujuan Antena Array (Antenna Array)?", "id": "Meningkatkan Penguatan (Gain), Keterarahan." },
  { "en": "Apa Itu Broadside Array (Array Broadside)?", "id": "Array Memancar Tegak Lurus Sumbu Array." },
  { "en": "Apa Itu End-Fire Array (Array End-Fire)?", "id": "Array Memancar Searah Sumbu Array." },
  { "en": "Apa Itu Catu Daya Linier (Linear Power Supply)?", "id": "Regulator Tegangan Menggunakan Komponen Linear." },
  { "en": "Apa Komponen Utama Catu Daya Linier?", "id": "Trafo, Penyearah, Filter Kapasitor, Regulator." },
  { "en": "Apa Keuntungan Catu Daya Linier (Linear Power Supply)?", "id": "Output Bersih (Riak, Noise Rendah)." },
  { "en": "Apa Kerugian Catu Daya Linier (Linear Power Supply)?", "id": "Efisiensi Rendah, Ukuran Besar, Berat." },
  { "en": "Apa Itu Catu Daya Switching (SMPS)?", "id": "Switch Mode Power Supply (SMPS)." },
  { "en": "Apa Keuntungan SMPS (Switch Mode Power Supply)?", "id": "Efisiensi Tinggi, Ringan, Ukuran Kecil." },
  { "en": "Apa Kerugian SMPS (Switch Mode Power Supply)?", "id": "Output Lebih Berisik (Noise Tinggi)." },
  { "en": "Apa Itu Efisiensi (Efficiency) Catu Daya?", "id": "Rasio Daya Output DC, Input AC." },
  { "en": "Apa Itu Regulasi Beban (Load Regulation)?", "id": "Perubahan Tegangan Output Akibat Beban." },
  { "en": "Apa Itu Regulasi Saluran (Line Regulation)?", "id": "Perubahan Output Akibat Input Berubah." },
  { "en": "Apa Itu Tegangan Riak (Ripple Voltage)?", "id": "Sisa Komponen AC Pada Output DC." },
  { "en": "Apa Itu Noise (Derau) Output Catu Daya?", "id": "Komponen Frekuensi Tinggi Tak Diinginkan." },
  { "en": "Apa Itu Respon Transien (Transient Response) Catu Daya?", "id": "Respon Output Terhadap Perubahan Beban." },
  { "en": "Apa Itu Soft Start (Soft Start) Catu Daya?", "id": "Fitur Menaikkan Tegangan Output Perlahan." },
  { "en": "Apa Fungsi Soft Start (Soft Start)?", "id": "Membatasi Arus Inrush Saat Dihidupkan." },
  { "en": "Apa Itu Proteksi Arus Lebih (Overcurrent Protection)?", "id": "Fitur Pembatas Arus Output Maksimal." },
  { "en": "Apa Itu Proteksi Tegangan Lebih (Overvoltage Protection)?", "id": "Fitur Pembatas Tegangan Output Maksimal." },
  { "en": "Apa Kepanjangan OCP (Overcurrent Protection)?", "id": "Proteksi Arus Lebih." },
  { "en": "Apa Kepanjangan OVP (Overvoltage Protection)?", "id": "Proteksi Tegangan Lebih." },
  { "en": "Apa Itu Proteksi Suhu Lebih (Overtemperature Protection)?", "id": "Fitur Mematikan Catu Daya Jika Panas." },
  { "en": "Apa Kepanjangan OTP (Overtemperature Protection)?", "id": "Proteksi Suhu Berlebih." },
  { "en": "Apa Itu Kapasitor Bulk (Bulk Capacitor)?", "id": "Kapasitor Filter Utama Setelah Penyearah." },
  { "en": "Apa Itu Induktor Filter (Filter Inductor) DC?", "id": "Induktor Menghaluskan Arus Output DC." },
  { "en": "Apa Itu Dioda Schottky (Schottky Diode) Penyearah?", "id": "Dioda Cepat, Tegangan Maju Rendah." },
  { "en": "Mengapa Pakai Dioda Schottky (Schottky Diode) Di SMPS?", "id": "Mengurangi Rugi-Rugi Switching." },
  { "en": "Apa Itu Kontrol Lebar Pulsa (PWM)?", "id": "Pulse Width Modulation (PWM)." },
  { "en": "Apa Fungsi IC (Integrated Circuit) Kontroler PWM?", "id": "Menghasilkan Sinyal PWM Atur Switching." },
  { "en": "Apa Itu Umpan Balik (Feedback) Optocoupler SMPS?", "id": "Isolasi Sinyal Umpan Balik Output." },
  { "en": "Apa Itu Referensi Tegangan (Voltage Reference)?", "id": "Sumber Tegangan Stabil Presisi." },
  { "en": "Contoh IC (Integrated Circuit) Referensi Tegangan (Voltage Reference)?", "id": "TL431 (TL431)." },
  { "en": "Apa Itu Kapasitor Keramik Multilayer (MLCC)?", "id": "Multilayer Ceramic Capacitor (MLCC)." },
  { "en": "Apa Kepanjangan MLCC (Multilayer Ceramic Capacitor)?", "id": "Kapasitor Keramik Multilapis." },
  { "en": "Apa Itu Kapasitor Elektrolit (Electrolytic Capacitor) Polimer?", "id": "Kapasitor ESR (Equivalent Series Resistance) Rendah." },
  { "en": "Apa Itu Kapasitor Tantalum (Tantalum Capacitor)?", "id": "Kapasitor Elektrolit Padat (Stabil)." },
  { "en": "Apa Itu Mode Diferensial (Differential Mode) Sinyal?", "id": "Sinyal Antara Dua Konduktor." },
  { "en": "Apa Itu Mode Bersama (Common Mode) Sinyal?", "id": "Sinyal Relatif Terhadap Ground Bersama." },
  { "en": "Apa Fungsi Choke (Choke) Mode Bersama?", "id": "Meredam Noise Mode Bersama." },
  { "en": "Apa Fungsi Kapasitor X (X Capacitor)?", "id": "Meredam Noise Mode Diferensial." },
  { "en": "Apa Fungsi Kapasitor Y (Y Capacitor)?", "id": "Meredam Noise Mode Bersama." },
  { "en": "Apa Itu Induktansi Bocor (Leakage Inductance) Trafo?", "id": "Induktansi Akibat Fluks Tidak Terkopling." },
  { "en": "Apa Itu Kapasitansi Antar Belitan (Inter-Winding Capacitance)?", "id": "Kapasitansi Parasitik Antara Primer, Sekunder." },
  { "en": "Apa Itu Pelindung Faraday (Faraday Shield) Trafo?", "id": "Lapisan Konduktif Kurangi Kapasitansi." },
  { "en": "Apa Itu Efisiensi (Efficiency) Energi?", "id": "Rasio Energi Berguna Terhadap Total Energi." },
  { "en": "Apa Itu Konservasi (Conservation) Energi?", "id": "Upaya Mengurangi Pemakaian Energi." },
  { "en": "Apa Itu Audit (Audit) Energi?", "id": "Evaluasi Penggunaan Energi Fasilitas." },
  { "en": "Apa Itu Manajemen (Management) Energi?", "id": "Praktik Pengelolaan Konsumsi Energi Efisien." },
  { "en": "Apa Itu Standar Efisiensi Energi (Energy Star)?", "id": "Label Efisiensi Produk Elektronik (AS)." },
  { "en": "Apa Itu Daya Siaga (Standby Power)?", "id": "Daya Dikonsumsi Alat Saat Mati (Plugged-In)." },
  { "en": "Apa Nama Lain Daya Siaga (Standby Power)?", "id": "Vampire Power (Daya Vampir)." },
  { "en": "Apa Itu Bangunan Hijau (Green Building)?", "id": "Bangunan Dirancang Ramah Lingkungan." },
  { "en": "Apa Itu Pencahayaan LED (Light Emitting Diode)?", "id": "Sumber Cahaya Efisiensi Sangat Tinggi." },
  { "en": "Apa Itu Lumen (Lumen) (lm)?", "id": "Satuan Fluks Cahaya (Kecerahan)." },
  { "en": "Apa Itu Lux (Lux) (lx)?", "id": "Satuan Iluminansi (Cahaya Per Area)." },
  { "en": "Apa Itu Efikasi Luminous (Luminous Efficacy)?", "id": "Ukuran Efisiensi Lampu (Lumen/Watt)." },
  { "en": "Apa Itu Suhu Warna (Color Temperature)?", "id": "Ukuran Warna Cahaya (Kelvin)." },
  { "en": "Suhu Warna (Color Temperature) Apa Untuk Warm White?", "id": "Sekitar 2700K - 3000K." },
  { "en": "Suhu Warna (Color Temperature) Apa Untuk Cool White?", "id": "Sekitar 4000K - 5000K." },
  { "en": "Suhu Warna (Color Temperature) Apa Untuk Daylight?", "id": "Sekitar 6000K - 6500K." },
  { "en": "Apa Itu Indeks Rendering Warna (CRI)?", "id": "Color Rendering Index (CRI)." },
  { "en": "Apa Kepanjangan CRI (Color Rendering Index)?", "id": "Indeks Rendering Warna." },
  { "en": "Apa Fungsi CRI (Color Rendering Index)?", "id": "Ukuran Kemampuan Lampu Tampilkan Warna Akurat." },
  { "en": "Berapa Nilai CRI (Color Rendering Index) Ideal?", "id": "Mendekati 100 (Warna Sangat Akurat)." },
  { "en": "Apa Itu Umur Lampu (Lamp Life)?", "id": "Estimasi Waktu Operasi Lampu." },
  { "en": "Apa Itu Fotometri (Photometry)?", "id": "Ilmu Pengukuran Cahaya (Terlihat Mata)." },
  { "en": "Apa Itu Radiometri (Radiometry)?", "id": "Ilmu Pengukuran Radiasi Elektromagnetik." },
  { "en": "Apa Itu Hukum Kuadrat Terbalik (Inverse Square Law) Cahaya?", "id": "Intensitas Berkurang Kuadrat Jarak." },
  { "en": "Apa Itu Sensor Gerak (Motion Sensor) Lampu?", "id": "Otomatisasi Lampu Berdasarkan Gerakan." },
  { "en": "Apa Itu Sensor Cahaya (Light Sensor) Lampu?", "id": "Otomatisasi Lampu Berdasarkan Cahaya Sekitar." },
  { "en": "Apa Itu Dimmer (Dimmer) Lampu?", "id": "Pengatur Tingkat Kecerahan Lampu." },
  { "en": "Apa Itu Sistem Kontrol Pencahayaan (Lighting Control)?", "id": "Sistem Pengelolaan Pencahayaan Otomatis." },
  { "en": "Apa Kepanjangan DALI (Digital Addressable Lighting Interface)?", "id": "Antarmuka Pencahayaan Dapat Dialamati Digital." },
  { "en": "Apa Itu KNX (KNX Standard)?", "id": "Standar Otomatisasi Bangunan, Rumah." },
  { "en": "Apa Itu Motor DC (Direct Current) Ber-sikat?", "id": "Brushed DC (Direct Current) Motor." },
  { "en": "Apa Itu Komutator (Commutator) Motor DC?", "id": "Membalik Arah Arus Di Rotor." },
  { "en": "Apa Itu Magnet Permanen (Permanent Magnet) Motor DC?", "id": "Motor DC (Direct Current) Medan Magnet Permanen." },
  { "en": "Apa Itu Elektromagnet (Electromagnet)?", "id": "Magnet Dihasilkan Arus Listrik." },
  { "en": "Apa Itu Gaya Gerak Magnet (MMF)?", "id": "Magnetomotive Force (MMF)." },
  { "en": "Apa Kepanjangan MMF (Magnetomotive Force)?", "id": "Gaya Gerak Magnet." },
  { "en": "Apa Rumus MMF (Magnetomotive Force)?", "id": "MMF = N x I (Lilitan x Arus)." },
  { "en": "Apa Itu Reluktansi (Reluctance) Magnetik?", "id": "Hambatan Terhadap Aliran Fluks Magnet." },
  { "en": "Apa Itu Fluks (Flux) Magnetik (Φ)?", "id": "Ukuran Jumlah Medan Magnet." },
  { "en": "Apa Satuan Fluks (Flux) Magnetik?", "id": "Weber (Wb)." }


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
