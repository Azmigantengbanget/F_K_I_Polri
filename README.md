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


  { "en": "Definisi forensik kelistrikan?", "id": "Investigasi insiden terkait sistem kelistrikan." },
  { "en": "Tujuan utama forensik kelistrikan?", "id": "Menentukan penyebab pasti insiden kelistrikan." },
  { "en": "Penyebab umum kebakaran listrik?", "id": "Hubungan singkat dan beban berlebih." },
  { "en": "Apa itu hubungan singkat (short circuit)?", "id": "Kontak langsung antar konduktor listrik." },
  { "en": "Apa itu beban berlebih (overload)?", "id": "Arus melebihi kapasitas aman sirkuit." },
  { "en": "Tanda khas hubungan singkat?", "id": "Lelehan lokal pada ujung konduktor." },
  { "en": "Tanda khas beban berlebih?", "id": "Kerusakan isolasi kabel merata panjang." },
  { "en": "Fungsi utama sekring (fuse)?", "id": "Melindungi sirkuit dari arus berlebih." },
  { "en": "Fungsi MCB (Miniature Circuit Breaker)?", "id": "Memutus sirkuit saat terjadi gangguan." },
  { "en": "Apa itu arc mapping?", "id": "Teknik melacak titik awal api." },
  { "en": "Jejak busur api disebut?", "id": "Arc bead atau butiran lelehan." },
  { "en": "Bagaimana petir sebabkan kebakaran?", "id": "Lonjakan tegangan merusak sistem listrik." },
  { "en": "Panel listrik penting diperiksa?", "id": "Ya, pusat distribusi kelistrikan utama." },
  { "en": "Apa itu KWh meter?", "id": "Alat pengukur konsumsi energi listrik." },
  { "en": "Investigasi meteran KWh untuk apa?", "id": "Mendeteksi kasus pencurian arus listrik." },
  { "en": "Modus pencurian listrik umum?", "id": "Memanipulasi atau bypass meteran KWh." },
  { "en": "Analisis komponen elektronik untuk?", "id": "Menemukan modus kegagalan perangkat bukti." },
  { "en": "Komponen daya sering gagal?", "id": "Kapasitor, transformator, dan dioda." },
  { "en": "Kegagalan kapasitor sebabkan apa?", "id": "Ledakan kecil atau hubungan singkat." },
  { "en": "TKP singkatan dari apa?", "id": "Tempat Kejadian Perkara." },
  { "en": "Langkah pertama di TKP kebakaran?", "id": "Amankan area dan identifikasi bahaya." },
  { "en": "Mengapa jalur kabel dianalisis?", "id": "Menemukan sumber atau titik api." },
  { "en": "Apa itu reverse engineering?", "id": "Menganalisis perangkat untuk tahu fungsinya." },
  { "en": "Contoh aplikasi reverse engineering?", "id": "Analisis pemicu bom atau skimmer." },
  { "en": "Apa itu skimmer ATM?", "id": "Alat ilegal mencuri data kartu." },
  { "en": "Bagaimana prosedur pengambilan sampel?", "id": "Dokumentasikan, beri label, kemas aman." },
  { "en": "Mengapa sampel harus dikemas baik?", "id": "Mencegah kontaminasi atau kerusakan lanjut." },
  { "en": "Alat dasar investigator kelistrikan?", "id": "Multimeter, tang ampere, kamera termal." },
  { "en": "Fungsi multimeter dalam investigasi?", "id": "Mengukur tegangan, arus, dan resistansi." },
  { "en": "Fungsi kamera termal (thermal camera)?", "id": "Mendeteksi titik panas abnormal (hotspot)." },
  { "en": "Apa yang dianalisis di lab?", "id": "Sampel kabel, sisa komponen elektronik." },
  { "en": "Pentingnya dokumentasi TKP?", "id": "Merekam kondisi asli sebelum diubah." },
  { "en": "Bentuk dokumentasi TKP apa?", "id": "Foto, video, sketsa, dan catatan." },
  { "en": "Apa itu Electrical Fire Investigator (EFI)?", "id": "Penyelidik kebakaran khusus bidang kelistrikan." },
  { "en": "Sertifikasi penting untuk investigator?", "id": "CFEI (Certified Fire Investigator)." },
  { "en": "Pola V (V-pattern) pada dinding?", "id": "Menunjukkan area asal mula api." },
  { "en": "Apa itu ground fault?", "id": "Hubungan singkat ke jalur ground." },
  { "en": "Perangkat pelindung ground fault?", "id": "GFCI (Ground Fault Circuit Interrupter)." },
  { "en": "Di mana GFCI biasa dipasang?", "id": "Area basah seperti kamar mandi." },
  { "en": "Analisis stop kontak terbakar?", "id": "Periksa bekas lelehan dan arcing." },
  { "en": "Penyebab stop kontak longgar?", "id": "Pemanasan berlebih dan potensi busur." },
  { "en": "Apa itu busur api (arc flash)?", "id": "Ledakan listrik bersuhu sangat tinggi." },
  { "en": "Bahaya utama arc flash?", "id": "Luka bakar parah, kebutaan, ledakan." },
  { "en": "Analisis saklar lampu di TKP?", "id": "Menentukan posisi on atau off." },
  { "en": "Bagaimana kabel aluminium jadi penyebab?", "id": "Oksidasi menyebabkan koneksi buruk memanas." },
  { "en": "Fungsi kapasitor pada perangkat?", "id": "Menyimpan dan melepaskan muatan listrik." },
  { "en": "Fungsi transformator pada perangkat?", "id": "Mengubah level tegangan listrik AC." },
  { "en": "Tanda kegagalan transformator?", "id": "Hangus, bau terbakar, kebocoran minyak." },
  { "en": "Apa itu sirkuit tercetak (PCB)?", "id": "Papan penghubung komponen-komponen elektronik." },
  { "en": "Analisis jejak pada PCB?", "id": "Mencari jalur putus atau terbakar." },
  { "en": "Mengapa warna kabel penting?", "id": "Identifikasi fungsi: fasa, netral, ground." },
  { "en": "Standar warna kabel di Indonesia?", "id": "PUIL (Persyaratan Umum Instalasi Listrik)." },
  { "en": "Warna kabel fasa (live)?", "id": "Hitam, cokelat, atau abu-abu." },
  { "en": "Warna kabel netral?", "id": "Biru." },
  { "en": "Warna kabel ground (arde)?", "id": "Hijau-kuning." },
  { "en": "Pentingnya sistem grounding?", "id": "Melindungi dari sengatan dan petir." },
  { "en": "Apa itu resistansi/hambatan?", "id": "Ukuran penolakan terhadap aliran arus." },
  { "en": "Apa satuan resistansi?", "id": "Ohm (Ω)." },
  { "en": "Apa satuan tegangan (voltage)?", "id": "Volt (V)." },
  { "en": "Apa satuan arus (current)?", "id": "Ampere (A)." },
  { "en": "Hukum Ohm menghubungkan apa?", "id": "Tegangan, arus, dan juga resistansi." },
  { "en": "Rumus daya listrik (power)?", "id": "Daya sama dengan Tegangan kali Arus." },
  { "en": "Satuan daya listrik apa?", "id": "Watt (W)." },
  { "en": "Jenis arus listrik?", "id": "Arus searah (DC) dan bolak-balik (AC)." },
  { "en": "Sumber arus DC?", "id": "Baterai, aki, dan adaptor." },
  { "en": "Sumber arus AC?", "id": "PLN (Perusahaan Listrik Negara)." },
  { "en": "Bahaya listrik bagi manusia?", "id": "Sengatan (electric shock) dan luka bakar." },
  { "en": "Faktor penentu keparahan sengatan?", "id": "Besar arus, durasi, jalur lintasan." },
  { "en": "Apa itu penyadap (bug)?", "id": "Alat pendengar atau perekam rahasia." },
  { "en": "Analisis alat penyadap untuk?", "id": "Identifikasi sumber daya, cara kerja." },
  { "en": "Apa itu sabotase kelistrikan?", "id": "Tindakan merusak sistem listrik sengaja." },
  { "en": "Contoh sabotase kelistrikan?", "id": "Memutus kabel, merusak panel listrik." },
  { "en": "Peran saksi ahli kelistrikan?", "id": "Memberi kesaksian teknis di pengadilan." },
  { "en": "Laporan forensik berisi apa?", "id": "Temuan, analisis, metodologi, dan kesimpulan." },
  { "en": "Apa itu rantai pengamanan bukti?", "id": "Catatan kronologis penanganan barang bukti." },
  { "en": "Mengapa rantai pengamanan penting?", "id": "Menjaga keaslian dan integritas bukti." },
  { "en": "Apa itu arus sisa?", "id": "Arus bocor ke jalur ground." },
  { "en": "Pelindung dari arus sisa?", "id": "ELCB (Earth Leakage Circuit Breaker)." },
  { "en": "Kabel ekstensi bisa berbahaya?", "id": "Ya, jika kualitas buruk atau overload." },
  { "en": "Tanda bahaya kabel ekstensi?", "id": "Panas, berubah warna, atau rusak." },
  { "en": "Penyebab umum kegagalan sambungan?", "id": "Koneksi longgar atau korosi." },
  { "en": "Apa itu induksi elektromagnetik?", "id": "Proses menghasilkan arus listrik dari magnet." },
  { "en": "Peran induksi dalam forensik?", "id": "Menganalisis motor, generator, transformator." },
  { "en": "Mengapa lampu pijar dianalisis?", "id": "Filamen bisa tunjukkan guncangan/panas." },
  { "en": "Filamen putus menandakan apa?", "id": "Lampu mati sebelum api terjadi." },
  { "en": "Filamen melar menandakan apa?", "id": "Lampu menyala saat api terjadi." },
  { "en": "Apa itu dioda?", "id": "Komponen penyearah arus listrik." },
  { "en": "Tanda kegagalan dioda?", "id": "Terbakar, retak, atau korsleting." },
  { "en": "Investigasi sistem alarm kebakaran?", "id": "Periksa apakah sistem berfungsi semestinya." },
  { "en": "Bagaimana data logger membantu?", "id": "Merekam data kelistrikan sebelum insiden." },
  { "en": "Apa itu osiloskop?", "id": "Alat visualisasi sinyal listrik." },
  { "en": "Fungsi osiloskop dalam forensik?", "id": "Analisis sinyal anomali perangkat elektronik." },
  { "en": "Pentingnya kalibrasi alat ukur?", "id": "Memastikan hasil pengukuran akurat." },
  { "en": "Apa itu energi listrik?", "id": "Daya listrik dikalikan dengan waktu." },
  { "en": "Satuan energi listrik?", "id": "Watt-hour (Wh) atau kilowatt-hour (kWh)." },
  { "en": "Hubungan forensik dengan asuransi?", "id": "Menentukan penyebab untuk klaim asuransi." },
  { "en": "Apa itu fire debris?", "id": "Puing-puing sisa dari suatu kebakaran." },
  { "en": "Analisis fire debris untuk?", "id": "Mencari residu pemicu api." },
  { "en": "Apa itu surge protector?", "id": "Pelindung dari lonjakan tegangan listrik." },
  { "en": "Kegagalan surge protector sebabkan?", "id": "Tidak melindungi perangkat saat terjadi lonjakan." },
  { "en": "Apa standar investigasi kebakaran?", "id": "NFPA 921, panduan investigasi ilmiah." },
  { "en": "Analisis metalurgi pada kabel?", "id": "Bedakan lelehan api vs listrik." },
  { "en": "Struktur butir logam lelehan listrik?", "id": "Struktur dendritik atau kolom." },
  { "en": "Struktur butir logam lelehan api?", "id": "Struktur equiaxed atau butiran seragam." },
  { "en": "Apa itu spoliation of evidence?", "id": "Perusakan atau penghilangan barang bukti." },
  { "en": "Konsekuensi spoliation of evidence?", "id": "Sanksi hukum, melemahkan kasus." },
  { "en": "Fungsi X-ray dalam forensik?", "id": "Melihat komponen internal tanpa merusak." },
  { "en": "Contoh penggunaan X-ray forensik?", "id": "Inspeksi saklar, relay, atau bom." },
  { "en": "Apa itu electrical fault analysis?", "id": "Analisis untuk menemukan kegagalan listrik." },
  { "en": "Tujuan utama electrical fault analysis?", "id": "Mencegah kegagalan serupa terjadi kembali." },
  { "en": "Apa itu thermal runaway?", "id": "Kondisi panas berlebih tak terkendali." },
  { "en": "Perangkat rentan thermal runaway?", "id": "Baterai lithium-ion dan kapasitor." },
  { "en": "Penyebab kebakaran listrik mobil?", "id": "Korsleting kabel, modifikasi, alternator." },
  { "en": "Pemeriksaan sistem kelistrikan kendaraan?", "id": "Periksa sekring, aki, jalur kabel." },
  { "en": "Apa itu hipotetis negatif?", "id": "Menyingkirkan penyebab yang tidak mungkin." },
  { "en": "Pentingnya hipotetis negatif?", "id": "Mempersempit fokus pada penyebab potensial." },
  { "en": "Apa itu fire modeling?", "id": "Simulasi komputer untuk dinamika kebakaran." },
  { "en": "Kegunaan fire modeling?", "id": "Membantu validasi hipotesis asal api." },
  { "en": "Analisis pada peralatan rumah tangga?", "id": "Cari cacat desain atau kegagalan." },
  { "en": "Contoh kegagalan alat rumah tangga?", "id": "Thermostat macet, kabel internal terkelupas." },
  { "en": "Apa itu sistem proteksi petir?", "id": "Sistem penyalur energi petir aman." },
  { "en": "Komponen utama proteksi petir?", "id": "Air terminal, konduktor, grounding." },
  { "en": "Apa itu tegangan sentuh (touch voltage)?", "id": "Tegangan pada benda berarus bocor." },
  { "en": "Bahaya utama tegangan sentuh?", "id": "Risiko sengatan listrik fatal." },
  { "en": "Apa itu tegangan langkah (step voltage)?", "id": "Perbedaan tegangan antara dua kaki." },
  { "en": "Di mana step voltage berbahaya?", "id": "Dekat titik sambaran petir tanah." },
  { "en": "Fungsi SEM (Scanning Electron Microscope)?", "id": "Analisis permukaan resolusi sangat tinggi." },
  { "en": "Apa yang dilihat SEM?", "id": "Morfologi lelehan dan kontaminasi mikro." },
  { "en": "Forensik pada perangkat IoT?", "id": "Analisis data, firmware, kegagalan komponen." },
  { "en": "Contoh kasus forensik IoT?", "id": "Kebakaran akibat smart plug rusak." },
  { "en": "Apa itu relay?", "id": "Saklar yang dioperasikan secara elektrik." },
  { "en": "Analisis kegagalan relay?", "id": "Kontak lengket, koil terbakar." },
  { "en": "Penyebab kontak relay lengket?", "id": "Busur api saat switching beban." },
  { "en": "Investigasi sistem panel surya?", "id": "Periksa inverter, konektor, dan panel." },
  { "en": "Potensi bahaya panel surya?", "id": "Arus DC tinggi, risiko arc." },
  { "en": "Apa itu inverter?", "id": "Mengubah arus DC menjadi arus AC." },
  { "en": "Apa itu Daubert Standard?", "id": "Standar untuk penerimaan kesaksian ahli." },
  { "en": "Pentingnya Daubert Standard?", "id": "Memastikan metodologi ilmiah dapat diandalkan." },
  { "en": "Apa itu grounding loop?", "id": "Jalur ground ganda sebabkan gangguan." },
  { "en": "Efek grounding loop?", "id": "Noise pada sinyal audio-video." },
  { "en": "Pemeriksaan kabel bawah tanah?", "id": "Gunakan TDR (Time-Domain Reflectometer)." },
  { "en": "Fungsi TDR apa?", "id": "Mencari lokasi putus atau korsleting." },
  { "en": "Apa itu efek Joule?", "id": "Panas dihasilkan oleh arus listrik." },
  { "en": "Bagaimana efek Joule relevan?", "id": "Dasar dari pemanasan berlebih (overheating)." },
  { "en": "Apa itu glow-to-arc transition?", "id": "Transisi dari koneksi panas ke arc." },
  { "en": "Pentingnya glow-to-arc transition?", "id": "Mekanisme umum pemicu api listrik." },
  { "en": "Apa itu resistivitas?", "id": "Sifat intrinsik material menahan arus." },
  { "en": "Hubungan suhu dan resistivitas konduktor?", "id": "Suhu naik, resistivitas ikut naik." },
  { "en": "Analisis sidik jari pada bukti?", "id": "Bisa mengidentifikasi pelaku sabotase." },
  { "en": "Apa itu magnetic field analysis?", "id": "Analisis medan magnet sisa." },
  { "en": "Kegunaan magnetic field analysis?", "id": "Tentukan arus terakhir sebelum putus." },
  { "en": "Apa itu kapasitansi?", "id": "Kemampuan menyimpan energi dalam medan listrik." },
  { "en": "Satuan kapasitansi apa?", "id": "Farad (F)." },
  { "en": "Apa itu induktansi?", "id": "Kecenderungan menentang perubahan arus listrik." },
  { "en": "Satuan induktansi apa?", "id": "Henry (H)." },
  { "en": "Mengapa saksi mata tidak cukup?", "id": "Membutuhkan bukti fisik untuk verifikasi." },
  { "en": "Apa itu 'fire tetrahedron'?", "id": "Panas, bahan bakar, oksigen, reaksi." },
  { "en": "Bagaimana listrik masuk fire tetrahedron?", "id": "Sebagai sumber panas atau pemicu." },
  { "en": "Apa itu 'arcing through char'?", "id": "Busur api melalui material terkarbonisasi." },
  { "en": "Pentingnya 'arcing through char'?", "id": "Bisa terjadi setelah api dimulai." },
  { "en": "Pemeriksaan sistem catu daya (PSU)?", "id": "Cek kapasitor, sekring, dan ventilasi." },
  { "en": "Penyebab kegagalan PSU komputer?", "id": "Debu, overheating, komponen tua." },
  { "en": "Apa itu electroporation?", "id": "Kerusakan membran sel akibat listrik." },
  { "en": "Relevansi electroporation dalam forensik?", "id": "Mekanisme cedera pada sengatan listrik." },
  { "en": "Apa itu 'Lichtenberg figure'?", "id": "Pola seperti pakis pada kulit." },
  { "en": "Tanda 'Lichtenberg figure' menunjukkan?", "id": "Jalur arus dari sambaran petir." },
  { "en": "Apa itu 'pyrophoric carbon'?", "id": "Karbon yang mudah menyala suhu rendah." },
  { "en": "Bagaimana 'pyrophoric carbon' terbentuk?", "id": "Pemanasan material organik jangka panjang." },
  { "en": "Apa itu 'cable whip'?", "id": "Gerakan kabel akibat gaya magnetik." },
  { "en": "Tanda 'cable whip' menunjukkan?", "id": "Terjadi arus gangguan sangat tinggi." },
  { "en": "Pemeriksaan alat pemanas (heater)?", "id": "Cek elemen pemanas dan saklar." },
  { "en": "Bahaya alat pemanas portabel?", "id": "Risiko tinggi bila dekat material mudah terbakar." },
  { "en": "Apa itu 'overcurrent protection device'?", "id": "Perangkat proteksi arus berlebih." },
  { "en": "Contoh 'overcurrent protection device'?", "id": "Sekring (fuse) dan MCB." },
  { "en": "Apa itu 'series arcing'?", "id": "Busur api pada satu konduktor." },
  { "en": "Penyebab 'series arcing'?", "id": "Koneksi longgar atau kabel putus." },
  { "en": "Apa itu 'parallel arcing'?", "id": "Busur api antara dua konduktor." },
  { "en": "Penyebab 'parallel arcing'?", "id": "Kerusakan isolasi, contohnya hubungan singkat." },
  { "en": "Pemeriksaan sistem pencahayaan darurat?", "id": "Periksa baterai dan fungsi otomatis." },
  { "en": "Pentingnya pencahayaan darurat?", "id": "Panduan evakuasi saat listrik padam." },
  { "en": "Apa itu 'fault current'?", "id": "Arus yang mengalir saat gangguan." },
  { "en": "Kenapa 'fault current' diukur?", "id": "Menentukan kapasitas perangkat proteksi." },
  { "en": "Peran analisis data cuaca?", "id": "Verifikasi kemungkinan adanya sambaran petir." },
  { "en": "Apa itu 'lightning arrester'?", "id": "Perangkat pelindung dari tegangan petir." },
  { "en": "Bagaimana 'lightning arrester' bekerja?", "id": "Membuang lonjakan tegangan ke ground." },
  { "en": "Apa itu 'dielectric breakdown'?", "id": "Kegagalan material isolator menjadi konduktif." },
  { "en": "Penyebab 'dielectric breakdown'?", "id": "Tegangan sangat tinggi melampaui batas." },
  { "en": "Apa itu 'hot spot'?", "id": "Titik dengan suhu abnormal tinggi." },
  { "en": "Cara deteksi 'hot spot'?", "id": "Menggunakan kamera inframerah atau termal." },
  { "en": "Fungsi 'strain relief' pada kabel?", "id": "Mencegah tarikan merusak sambungan internal." },
  { "en": "Pemeriksaan 'strain relief' penting?", "id": "Ya, kegagalannya sebabkan kabel putus." },
  { "en": "Apa itu 'electrical signature analysis'?", "id": "Analisis sinyal untuk deteksi kerusakan." },
  { "en": "Kegunaan 'electrical signature analysis'?", "id": "Memprediksi kegagalan pada mesin motor." },
  { "en": "Apa itu 'insulation resistance test'?", "id": "Tes mengukur kualitas isolasi kabel." },
  { "en": "Alat untuk 'insulation resistance test'?", "id": "Megohmmeter atau insulation tester." },
  { "en": "Hasil tes resistansi isolasi rendah?", "id": "Menandakan isolasi buruk, risiko bocor." },
  { "en": "Pemeriksaan alat pemicu bom?", "id": "Identifikasi sumber daya dan mekanisme pemicu." },
  { "en": "Analisis kimia jelaga untuk?", "id": "Identifikasi material sumber yang terbakar." },
  { "en": "Apa itu 'transient voltage'?", "id": "Lonjakan tegangan listrik sangat singkat." },
  { "en": "Sumber 'transient voltage' internal?", "id": "Motor besar, saklar, dan relay." },
  { "en": "Forensik pada peralatan medis?", "id": "Analisis kegagalan dan cedera pasien." },
  { "en": "Pemeriksaan instalasi listrik tua?", "id": "Degradasi isolasi, sambungan, dan beban." },
  { "en": "Bahan isolasi kabel kuno?", "id": "Kain, karet, atau gutta-percha." },
  { "en": "Risiko isolasi kain atau karet?", "id": "Menjadi rapuh dan mudah terbakar." },
  { "en": "Investigasi kegagalan gardu induk?", "id": "Analisis relay proteksi dan pemutus." },
  { "en": "Apa itu 'catastrophic transformer failure'?", "id": "Kegagalan trafo besar sebabkan ledakan." },
  { "en": "Analisis memori non-volatil alat?", "id": "Mendapatkan status terakhir sebelum gagal." },
  { "en": "Contoh memori non-volatil?", "id": "Pengaturan terakhir pada oven microwave." },
  { "en": "Apa itu 'knob-and-tube wiring'?", "id": "Sistem perkabelan sangat tua." },
  { "en": "Bahaya 'knob-and-tube wiring'?", "id": "Tidak ada ground, isolasi rusak." },
  { "en": "Apa itu 'eddy current'?", "id": "Arus sirkular diinduksi dalam konduktor." },
  { "en": "Kerugian akibat 'eddy current'?", "id": "Menyebabkan kehilangan energi dan panas." },
  { "en": "Contoh anti-forensik kelistrikan?", "id": "Sengaja merusak bukti, memakai timer." },
  { "en": "Bagaimana timer digunakan untuk kejahatan?", "id": "Menunda aksi, menciptakan alibi palsu." },
  { "en": "Apa itu 'water-treeing' pada kabel?", "id": "Degradasi isolasi kabel bawah tanah." },
  { "en": "Penyebab 'water-treeing'?", "id": "Kelembaban dan tegangan listrik tinggi." },
  { "en": "Analisis 'fuse box' mobil?", "id": "Identifikasi sirkuit mana yang gagal." },
  { "en": "Sekring meleleh vs putus?", "id": "Lelehan indikasikan beban berlebih lama." },
  { "en": "Apa itu 'inductive heating'?", "id": "Pemanasan logam oleh medan magnet." },
  { "en": "Forensik pada turbin angin?", "id": "Analisis generator, gearbox, dan sistem kontrol." },
  { "en": "Penyebab kebakaran turbin angin?", "id": "Petir, rem gagal, atau hubung singkat." },
  { "en": "Apa itu 'stray current'?", "id": "Arus listrik mengalir di luar sirkuit." },
  { "en": "Bahaya 'stray current'?", "id": "Menyebabkan korosi dan risiko kejutan." },
  { "en": "Analisis kegagalan baterai asam timbal?", "id": "Korosi terminal, korsleting internal." },
  { "en": "Gas berbahaya dari baterai?", "id": "Gas hidrogen, sangat mudah meledak." },
  { "en": "Pentingnya ventilasi ruang baterai?", "id": "Mencegah akumulasi gas hidrogen berbahaya." },
  { "en": "Apa itu 'electrical fire cause matrix'?", "id": "Alat bantu analisis penyebab kebakaran." },
  { "en": "Fungsi diagram listrik forensik?", "id": "Visualisasi dan analisis sistem kelistrikan." },
  { "en": "Apa itu 'timeline analysis'?", "id": "Membuat urutan kronologis suatu kejadian." },
  { "en": "Pentingnya 'timeline analysis'?", "id": "Membantu merekonstruksi dan memahami insiden." },
  { "en": "Apa itu 'galvanic corrosion'?", "id": "Korosi akibat dua logam berbeda." },
  { "en": "Contoh 'galvanic corrosion' kelistrikan?", "id": "Sambungan kabel tembaga dan aluminium." },
  { "en": "Pemeriksaan sistem UPS (Uninterruptible Power Supply)?", "id": "Analisis baterai, inverter, dan kipas." },
  { "en": "Penyebab umum kegagalan UPS?", "id": "Baterai habis, overload, kegagalan komponen." },
  { "en": "Apa itu 'circuit tracing'?", "id": "Proses identifikasi jalur suatu sirkuit." },
  { "en": "Alat untuk 'circuit tracing'?", "id": "Tone generator dan probe." },
  { "en": "Apa itu 'phantom voltage'?", "id": "Tegangan terukur tanpa koneksi langsung." },
  { "en": "Penyebab 'phantom voltage'?", "id": "Induksi kapasitif dari kabel terdekat." },
  { "en": "Apa itu 'in-rush current'?", "id": "Arus awal tinggi saat alat dinyalakan." },
  { "en": "Alat dengan 'in-rush current' tinggi?", "id": "Motor, transformator, dan power supply." },
  { "en": "Analisis kegagalan motor listrik?", "id": "Gulungan terbakar, bearing macet, overload." },
  { "en": "Apa itu 'single-phasing' pada motor?", "id": "Kehilangan satu fasa pada motor 3-fasa." },
  { "en": "Akibat dari 'single-phasing'?", "id": "Motor panas berlebih dan terbakar." },
  { "en": "Fungsi 'thermal overload relay'?", "id": "Melindungi motor dari beban berlebih." },
  { "en": "Pemeriksaan sisa-sisa lampu LED?", "id": "Analisis driver dan chip LED." },
  { "en": "Penyebab kegagalan lampu LED?", "id": "Driver rusak atau chip overheat." },
  { "en": "Apa itu 'corona discharge'?", "id": "Lucutan listrik dari konduktor tegangan tinggi." },
  { "en": "Tanda-tanda 'corona discharge'?", "id": "Cahaya ungu, suara mendesis, bau ozon." },
  { "en": "Pemeriksaan kabel data (Ethernet)?", "id": "Bisa menghantarkan daya (PoE)." },
  { "en": "Apa itu PoE (Power over Ethernet)?", "id": "Daya listrik melalui kabel jaringan." },
  { "en": "Risiko PoE dalam kebakaran?", "id": "Beban berlebih pada kabel data." },
  { "en": "Forensik pada mesin industri?", "id": "Analisis kontrol panel dan sensor keselamatan." },
  { "en": "Peran 'human error' dalam insiden?", "id": "Kesalahan instalasi, perawatan, atau operasi." },
  { "en": "Contoh 'human error' instalasi?", "id": "Sambungan longgar, salah ukuran kabel." },
  { "en": "Apa itu 'lockout/tagout' (LOTO)?", "id": "Prosedur keselamatan mematikan mesin." },
  { "en": "Kegagalan LOTO sebabkan apa?", "id": "Kecelakaan kerja akibat mesin menyala." },
  { "en": "Apa itu 'vibrational fatigue'?", "id": "Kerusakan material akibat getaran terus-menerus." },
  { "en": "Contoh 'vibrational fatigue' kelistrikan?", "id": "Sambungan kabel pada mesin bergetar." },
  { "en": "Analisis jelaga bermagnet (magnetic soot)?", "id": "Indikasi material besi terbakar." },
  { "en": "Apa itu 'annealing' pada kabel?", "id": "Pelunakan tembaga akibat paparan panas." },
  { "en": "Pentingnya 'annealing' dalam analisis?", "id": "Memperkirakan suhu dan durasi api." },
  { "en": "Apa itu 'component sweating'?", "id": "Lelehan solder dari komponen PCB." },
  { "en": "Arti dari 'component sweating'?", "id": "PCB terpapar suhu sangat tinggi." },
  { "en": "Apa itu 'tin whiskers'?", "id": "Pertumbuhan kristal timah dari solder." },
  { "en": "Bahaya dari 'tin whiskers'?", "id": "Menyebabkan hubungan singkat antar komponen." },
  { "en": "Investigasi sistem alarm keamanan?", "id": "Analisis panel, sensor, dan catu daya." },
  { "en": "Bagaimana alarm bisa sebabkan api?", "id": "Korsleting pada baterai atau trafo." },
  { "en": "Pemeriksaan alat las listrik?", "id": "Cek kabel, konektor, dan siklus." },
  { "en": "Bahaya percikan las?", "id": "Dapat menyulut material mudah terbakar." },
  { "en": "Apa itu 'neutral-to-earth fault'?", "id": "Hubungan antara kabel netral dan ground." },
  { "en": "Akibat 'neutral-to-earth fault'?", "id": "Trip pada ELCB, arus liar." },
  { "en": "Apa itu 'harmonics' dalam listrik?", "id": "Frekuensi tambahan pengganggu pada sistem." },
  { "en": "Sumber 'harmonics'?", "id": "Komputer, charger, lampu neon modern." },
  { "en": "Dampak buruk 'harmonics'?", "id": "Panas berlebih pada kabel netral." },
  { "en": "Apa itu 'power quality analysis'?", "id": "Analisis kualitas daya listrik." },
  { "en": "Alat untuk 'power quality analysis'?", "id": "Power Quality Analyzer (PQA)." },
  { "en": "Apa itu 'non-destructive testing' (NDT)?", "id": "Pengujian bukti tanpa merusaknya." },
  { "en": "Contoh metode NDT?", "id": "X-ray, ultrasonik, dan visual inspeksi." },
  { "en": "Pemeriksaan saklar dimmer lampu?", "id": "Komponen elektronik rentan panas berlebih." },
  { "en": "Apa itu 'solid-state relay' (SSR)?", "id": "Relay elektronik tanpa bagian bergerak." },
  { "en": "Kegagalan pada 'solid-state relay'?", "id": "Gagal putus (stuck on), overheat." },
  { "en": "Apa itu 'impedance'?", "id": "Total hambatan pada sirkuit AC." },
  { "en": "Komponen dari 'impedance'?", "id": "Resistansi dan reaktansi (induktif/kapasitif)." },
  { "en": "Analisis kabel koaksial (coaxial)?", "id": "Periksa shield, dielektrik, dan konduktor." },
  { "en": "Pemeriksaan pemanas air listrik?", "id": "Termostat, elemen pemanas, kebocoran air." },
  { "en": "Bahaya pemanas air listrik?", "id": "Kebocoran air bertemu listrik." },
  { "en": "Apa itu 'current transformer' (CT)?", "id": "Sensor untuk mengukur arus tinggi." },
  { "en": "Fungsi CT dalam proteksi?", "id": "Memberi sinyal arus ke relay." },
  { "en": "Apa itu 'potential transformer' (PT)?", "id": "Sensor untuk mengukur tegangan tinggi." },
  { "en": "Pemeriksaan 'junction box' (T-dus)?", "id": "Cek kualitas sambungan dan kerapian." },
  { "en": "Penyebab umum masalah 'junction box'?", "id": "Sambungan longgar, kabel terlalu banyak." },
  { "en": "Apa itu 'class' pada sekring?", "id": "Menunjukkan kecepatan putus dan kapasitas." },
  { "en": "Pentingnya 'class' sekring?", "id": "Pemilihan salah bisa gagal melindungi." },
  { "en": "Apa itu Arc Fault Circuit Interrupter (AFCI)?", "id": "Alat deteksi busur api berbahaya." },
  { "en": "Bedanya AFCI dan GFCI?", "id": "AFCI deteksi busur, GFCI deteksi bocor." },
  { "en": "Mengapa AFCI bisa 'trip'?", "id": "Mendeteksi pola arus busur api." },
  { "en": "Fungsi forensik data AFCI?", "id": "Beberapa model simpan data penyebab trip." },
  { "en": "Apa itu fulgurite?", "id": "Kaca alamiah dari sambaran petir." },
  { "en": "Analisis fulgurite untuk apa?", "id": "Konfirmasi lokasi sambaran petir langsung." },
  { "en": "Bedanya sambaran petir langsung/tidak langsung?", "id": "Langsung mengenai objek, tidak langsung induksi." },
  { "en": "Teknik foto forensik listrik?", "id": "Makro, ultraviolet (UV), inframerah (IR)." },
  { "en": "Fungsi fotografi makro?", "id": "Menunjukkan detail kecil seperti arc bead." },
  { "en": "Apa itu photogrammetry TKP?", "id": "Membuat model 3D dari foto." },
  { "en": "Kegunaan model 3D TKP?", "id": "Analisis dan presentasi TKP virtual." },
  { "en": "Investigasi ledakan debu (dust explosion)?", "id": "Cari sumber pemicu seperti statis." },
  { "en": "Sumber listrik statis industri?", "id": "Gesekan material, aliran serbuk." },
  { "en": "Bagaimana listrik statis picu api?", "id": "Lucutan (discharge) membakar uap/debu." },
  { "en": "Analisis kegagalan isolator porselen?", "id": "Retak, kontaminasi permukaan, flashover." },
  { "en": "Apa itu 'flashover' pada isolator?", "id": "Busur api lompat melalui permukaan." },
  { "en": "Fungsi simulasi SPICE forensik?", "id": "Analisis dan reka ulang sirkuit." },
  { "en": "Apa itu 'Finite Element Analysis' (FEA)?", "id": "Simulasi stres termal dan mekanik." },
  { "en": "Kegunaan FEA dalam forensik?", "id": "Analisis kegagalan komponen akibat panas." },
  { "en": "Analisis timer mekanis alat?", "id": "Mengetahui sisa waktu siklus." },
  { "en": "Bagaimana timer mekanis bisa gagal?", "id": "Kontak macet, motor penggerak rusak." },
  { "en": "Efek korosi garam (salt spray)?", "id": "Mempercepat kegagalan konektor dan terminal." },
  { "en": "Daerah berisiko korosi garam?", "id": "Area pesisir pantai atau industri." },
  { "en": "Analisis degradasi polimer isolasi?", "id": "Identifikasi penuaan akibat UV/panas." },
  { "en": "Apa itu 'tracking' pada isolasi?", "id": "Jalur konduktif terbentuk di permukaan." },
  { "en": "Penyebab 'tracking' pada isolasi?", "id": "Kelembaban, debu, dan tegangan listrik." },
  { "en": "Standar kelistrikan internasional?", "id": "IEC (International Electrotechnical Commission)." },
  { "en": "Peran standar IEC?", "id": "Menyelaraskan standar teknis antar negara." },
  { "en": "Pemeriksaan sistem pemadam api otomatis?", "id": "Analisis apakah sistem terpicu benar." },
  { "en": "Kegagalan sistem pemadam api?", "id": "Sensor gagal, tekanan kurang, aktivasi salah." },
  { "en": "Apa itu 'hot slag'?", "id": "Lelehan logam panas dari pengelasan/korsleting." },
  { "en": "Bahaya dari 'hot slag'?", "id": "Dapat menjadi sumber penyulut api." },
  { "en": "Pemeriksaan saklar sentrifugal motor?", "id": "Mekanisme start motor satu fasa." },
  { "en": "Kegagalan saklar sentrifugal?", "id": "Kontak macet, sebabkan gulungan terbakar." },
  { "en": "Apa itu 'dielectric constant'?", "id": "Kemampuan material menyimpan energi listrik." },
  { "en": "Pentingnya 'dielectric constant'?", "id": "Mempengaruhi performa kapasitor dan isolator." },
  { "en": "Analisis pada sekering semikonduktor?", "id": "Melindungi perangkat elektronik sensitif." },
  { "en": "Karakteristik sekering semikonduktor?", "id": "Beraksi sangat cepat (fast-acting)." },
  { "en": "Apa itu 'crowbar circuit'?", "id": "Sirkuit proteksi dari tegangan berlebih." },
  { "en": "Cara kerja 'crowbar circuit'?", "id": "Sengaja membuat korsleting untuk putuskan sekring." },
  { "en": "Apa itu 'bleeder resistor'?", "id": "Resistor pembuang muatan sisa kapasitor." },
  { "en": "Bahaya jika 'bleeder resistor' gagal?", "id": "Risiko sengatan dari kapasitor bermuatan." },
  { "en": "Pemeriksaan kabel pemanas (heating cable)?", "id": "Cek kerusakan fisik, thermostat." },
  { "en": "Penggunaan kabel pemanas?", "id": "Pemanas lantai, pencegah pipa beku." },
  { "en": "Apa itu 'destructive analysis'?", "id": "Analisis yang merusak barang bukti." },
  { "en": "Kapan 'destructive analysis' dilakukan?", "id": "Setelah semua tes non-destruktif selesai." },
  { "en": "Apa itu 'exemplar testing'?", "id": "Menguji perangkat sejenis yang baru." },
  { "en": "Tujuan 'exemplar testing'?", "id": "Membandingkan modus kegagalan dengan bukti." },
  { "en": "Pemeriksaan selungkup (enclosure) listrik?", "id": "Cari tanda overheat, intrusi air." },
  { "en": "Rating IP (Ingress Protection) artinya?", "id": "Tingkat proteksi terhadap debu dan air." },
  { "en": "Contoh rating IP?", "id": "IP67, tahan debu dan rendaman air." },
  { "en": "Apa itu 'conformal coating' pada PCB?", "id": "Lapisan pelindung dari lembab/debu." },
  { "en": "Kegagalan 'conformal coating'?", "id": "Retak, mengelupas, gagal melindungi sirkuit." },
  { "en": "Pemeriksaan 'busbar' pada panel?", "id": "Cari perubahan warna, koneksi longgar." },
  { "en": "Perubahan warna 'busbar' menandakan?", "id": "Terjadi pemanasan berlebih (overheating)." },
  { "en": "Apa itu 'ferroresonance'?", "id": "Osilasi tegangan tinggi di sistem." },
  { "en": "Bahaya dari 'ferroresonance'?", "id": "Merusak transformator dan arester petir." },
  { "en": "Apa itu 'voltage sag'?", "id": "Penurunan tegangan listrik sementara." },
  { "en": "Apa itu 'voltage swell'?", "id": "Kenaikan tegangan listrik sementara." },
  { "en": "Dampak 'sag' dan 'swell'?", "id": "Menyebabkan malfungsi pada perangkat elektronik." },
  { "en": "Analisis kegagalan 'variable frequency drive' (VFD)?", "id": "Periksa IGBT, kapasitor, dan kontrol." },
  { "en": "Fungsi VFD (inverter)?", "id": "Mengatur kecepatan putaran motor AC." },
  { "en": "Apa itu 'carbon tracking'?", "id": "Jejak karbon konduktif pada isolator." },
  { "en": "Penyebab 'carbon tracking'?", "id": "Busur api kecil berulang-ulang." },
  { "en": "Apa itu 'inter-turn short'?", "id": "Hubungan singkat antar lilitan trafo/motor." },
  { "en": "Akibat 'inter-turn short'?", "id": "Pemanasan lokal, kegagalan kumparan total." },
  { "en": "Pemeriksaan pada konektor tusuk (IDC)?", "id": "Korosi, koneksi tidak sempurna." },
  { "en": "Apa itu 'fretting corrosion'?", "id": "Korosi pada kontak akibat getaran." },
  { "en": "Pemeriksaan sel surya (photovoltaic cell)?", "id": "Cari retakan mikro (microcracks), hot spot." },
  { "en": "Dampak 'microcracks' pada sel surya?", "id": "Menurunkan efisiensi dan umur panel." },
  { "en": "Apa itu 'bypass diode' panel surya?", "id": "Melindungi sel dari efek bayangan." },
  { "en": "Kegagalan 'bypass diode'?", "id": "Menyebabkan hot spot dan kebakaran." },
  { "en": "Analisis sistem pengisian kendaraan listrik (EVSE)?", "id": "Periksa konektor, kabel, dan kontrol." },
  { "en": "Bahaya pengisian kendaraan listrik?", "id": "Overheating konektor, korsleting kabel." },
  { "en": "Apa itu 'outgassing'?", "id": "Pelepasan gas dari material panas." },
  { "en": "Bahaya 'outgassing' dari PVC?", "id": "Melepaskan gas hidrogen klorida korosif." },
  { "en": "Pemeriksaan ballast lampu neon?", "id": "Cari kebocoran, bau terbakar, overheat." },
  { "en": "Jenis ballast lampu?", "id": "Magnetik (lama) dan elektronik (baru)." },
  { "en": "Apa itu 'potting compound'?", "id": "Material untuk melindungi sirkuit elektronik." },
  { "en": "Analisis 'potting compound' terbakar?", "id": "Bisa menunjukkan lokasi komponen gagal." },
  { "en": "Apa itu 'resistive heating'?", "id": "Pemanasan akibat resistansi suatu material." },
  { "en": "Contoh 'resistive heating'?", "id": "Elemen pemanas pada setrika, toaster." },
  { "en": "Pemeriksaan konektor tipe 'wire nut'?", "id": "Pemasangan tidak benar, longgar, overheat." },
  { "en": "Kapasitas 'wire nut' penting?", "id": "Ya, harus sesuai jumlah/ukuran kabel." },
  { "en": "Apa itu 'electrical noise'?", "id": "Sinyal listrik yang tidak diinginkan." },
  { "en": "Dampak 'electrical noise'?", "id": "Mengganggu operasi perangkat elektronik sensitif." },
  { "en": "Apa itu 'Faraday cage'?", "id": "Selungkup pelindung dari medan listrik." },
  { "en": "Fungsi 'Faraday cage' forensik?", "id": "Melindungi bukti digital dari penghapusan jarak jauh." },
  { "en": "Apa itu 'electromigration'?", "id": "Perpindahan ion logam akibat arus." },
  { "en": "Dampak 'electromigration'?", "id": "Menyebabkan kegagalan pada sirkuit terpadu." },
  { "en": "Analisis kegagalan 'reed switch'?", "id": "Kontak lengket, pecah, sensitivitas hilang." },
  { "en": "Fungsi 'reed switch'?", "id": "Saklar diaktifkan oleh medan magnet." },
  { "en": "Apa itu 'Curie temperature'?", "id": "Suhu di mana magnet kehilangan magnetnya." },
  { "en": "Pentingnya 'Curie temperature' forensik?", "id": "Analisis kerusakan magnet akibat api." },
  { "en": "Apa itu 'thermoelectric effect'?", "id": "Tegangan timbul dari perbedaan suhu." },
  { "en": "Contoh aplikasi 'thermoelectric effect'?", "id": "Termokopel (sensor suhu)." },
  { "en": "Analisis kegagalan termokopel?", "id": "Degradasi material, koneksi putus." },
  { "en": "Apa itu Metal-Oxide Varistor (MOV)?", "id": "Komponen pelindung dari lonjakan tegangan." },
  { "en": "Tanda kegagalan umum MOV?", "id": "Retak, hangus, atau hancur meledak." },
  { "en": "Analisis data forensik alat pacu jantung?", "id": "Rekam aktivitas jantung sebelum insiden." },
  { "en": "Forensik pada defibrilator implan (ICD)?", "id": "Periksa data kejutan dan log peristiwa." },
  { "en": "Fungsi GC/MS pada jelaga?", "id": "Identifikasi senyawa kimia residu terbakar." },
  { "en": "Apa yang bisa diidentifikasi GC/MS?", "id": "Jenis plastik, akseleran, atau pelarut." },
  { "en": "Forensik sistem kelistrikan kapal?", "id": "Periksa korosi, getaran, dan grounding." },
  { "en": "Tantangan forensik kelistrikan kapal?", "id": "Lingkungan air asin yang korosif." },
  { "en": "Forensik kelistrikan kereta api?", "id": "Analisis sistem pantograf dan propulsi." },
  { "en": "Apa itu 'stray current corrosion'?", "id": "Korosi akibat arus bocor dari rel." },
  { "en": "Peran SCADA dalam investigasi?", "id": "Menyediakan log data operasional historis." },
  { "en": "Bagaimana SCADA bisa jadi bukti?", "id": "Menunjukkan alarm atau parameter abnormal." },
  { "en": "Apa itu confirmation bias investigator?", "id": "Mencari bukti yang mendukung teori awal." },
  { "en": "Cara menghindari confirmation bias?", "id": "Uji semua hipotesis yang masuk akal." },
  { "en": "Investigasi kebakaran sistem ESS?", "id": "Analisis BMS, sel baterai, pendingin." },
  { "en": "Apa itu Battery Management System (BMS)?", "id": "Sistem kontrol untuk proteksi baterai." },
  { "en": "Penyebab utama kebakaran ESS?", "id": "Korsleting internal sel, thermal runaway." },
  { "en": "Beda luka sengatan AC/DC?", "id": "AC sebabkan kejang otot, DC tolakan." },
  { "en": "Analisis luka masuk dan keluar?", "id": "Menentukan jalur aliran arus listrik." },
  { "en": "Analisis audio ledakan listrik?", "id": "Membedakan suara arc dari sumber lain." },
  { "en": "Analisis video kelip lampu?", "id": "Bisa indikasikan gangguan listrik sesaat." },
  { "en": "Definisi 'proximate cause' hukum?", "id": "Penyebab langsung yang menghasilkan kerusakan." },
  { "en": "Definisi 'standard of care'?", "id": "Tingkat kehati-hatian yang wajar." },
  { "en": "Pemeriksaan termistor (thermistor)?", "id": "Komponen sensor suhu, bisa gagal." },
  { "en": "Jenis termistor?", "id": "NTC (Negative) dan PTC (Positive)." },
  { "en": "Kegagalan termistor PTC sebabkan?", "id": "Kehilangan fungsi proteksi arus berlebih." },
  { "en": "Apa itu 'case-and-desist' letter forensik?", "id": "Surat permintaan penghentian perusakan bukti." },
  { "en": "Pemeriksaan optocoupler/opto-isolator?", "id": "Isolasi sinyal antar dua sirkuit." },
  { "en": "Kegagalan optocoupler sebabkan apa?", "id": "Kehilangan isolasi, kerusakan sirkuit kontrol." },
  { "en": "Apa itu 'wet tracking' pada isolator?", "id": "Jejak karbon akibat kontaminasi basah." },
  { "en": "Pemeriksaan kabel bawah laut?", "id": "Analisis kerusakan akibat jangkar, abrasi." },
  { "en": "Apa itu 'Fourier analysis' forensik?", "id": "Analisis frekuensi sinyal listrik terekam." },
  { "en": "Kegunaan 'Fourier analysis'?", "id": "Identifikasi 'noise' atau frekuensi harmonik." },
  { "en": "Apa itu 'dendritic growth'?", "id": "Pertumbuhan struktur seperti cabang di PCB." },
  { "en": "Penyebab 'dendritic growth'?", "id": "Migrasi ion logam dalam kelembaban." },
  { "en": "Analisis pada 'fuse holder'?", "id": "Kontak longgar sebabkan panas berlebih." },
  { "en": "Apa itu 'positive train control' (PTC)?", "id": "Sistem keselamatan kereta berbasis elektronik." },
  { "en": "Forensik pada sistem PTC?", "id": "Analisis data log untuk penyebab kecelakaan." },
  { "en": "Apa itu 'creepage distance'?", "id": "Jarak terpendek di permukaan isolator." },
  { "en": "Pentingnya 'creepage distance'?", "id": "Mencegah flashover akibat kontaminasi." },
  { "en": "Apa itu 'clearance distance'?", "id": "Jarak terpendek melalui udara." },
  { "en": "Pentingnya 'clearance distance'?", "id": "Mencegah busur api antar konduktor." },
  { "en": "Pemeriksaan sistem 'cathodic protection'?", "id": "Sistem pencegah korosi pada logam." },
  { "en": "Kegagalan 'cathodic protection' sebabkan?", "id": "Korosi cepat pada pipa atau kapal." },
  { "en": "Apa itu 'thermite reaction'?", "id": "Reaksi kimia hasilkan panas ekstrem." },
  { "en": "Hubungan 'thermite reaction' dengan forensik?", "id": "Bisa salah diira sebagai busur listrik." },
  { "en": "Analisis saklar membran (membrane switch)?", "id": "Cari retakan, intrusi cairan." },
  { "en": "Apa itu 'electrolytic capacitor'?", "id": "Kapasitor polarisasi dengan cairan elektrolit." },
  { "en": "Tanda kegagalan kapasitor elektrolit?", "id": "Menggembung, bocor, atau meledak." },
  { "en": "Apa itu 'dry joint' solder?", "id": "Sambungan solder buruk, tidak mengkilap." },
  { "en": "Masalah dari 'dry joint'?", "id": "Koneksi intermiten, panas, dan gagal." },
  { "en": "Apa itu 'hot-plugging'?", "id": "Mencolok/mencabut konektor saat berdaya." },
  { "en": "Bahaya 'hot-plugging'?", "id": "Menyebabkan busur api merusak konektor." },
  { "en": "Pemeriksaan 'terminal block'?", "id": "Retak, sekrup longgar, tanda panas." },
  { "en": "Apa itu 'inductive spike'?", "id": "Lonjakan tegangan saat beban induktif mati." },
  { "en": "Pelindung dari 'inductive spike'?", "id": "Flyback diode atau snubber circuit." },
  { "en": "Fungsi 'flyback diode'?", "id": "Memberi jalur aman bagi arus induksi." },
  { "en": "Pemeriksaan pada 'supercapacitor'?", "id": "Cari kebocoran elektrolit, pembengkakan." },
  { "en": "Apa itu 'event data recorder' (EDR)?", "id": "Perekam data kejadian, seperti 'kotak hitam'." },
  { "en": "Penggunaan EDR dalam forensik?", "id": "Menyediakan data parameter sebelum insiden." },
  { "en": "Apa itu 'compliance testing'?", "id": "Pengujian produk terhadap standar keselamatan." },
  { "en": "Pentingnya 'compliance testing' forensik?", "id": "Menentukan apakah produk cacat desain." },
  { "en": "Analisis kegagalan 'piezoelectric igniter'?", "id": "Kristal retak, kabel putus." },
  { "en": "Fungsi 'piezoelectric igniter'?", "id": "Menghasilkan percikan api dari tekanan." },
  { "en": "Apa itu 'glowing connection'?", "id": "Sambungan listrik memijar karena resistansi tinggi." },
  { "en": "Bahaya 'glowing connection'?", "id": "Sumber penyulut api yang tersembunyi." },
  { "en": "Apa itu 'copper plating'?", "id": "Pelapisan tembaga pada suatu material." },
  { "en": "Forensik 'copper plating'?", "id": "Bisa menunjukkan jalur arus bocor." },
  { "en": "Pemeriksaan motor servo (servo motor)?", "id": "Analisis kontroler, encoder, dan motor." },
  { "en": "Apa itu 'back-EMF'?", "id": "Tegangan yang dihasilkan oleh motor berputar." },
  { "en": "Pentingnya 'back-EMF'?", "id": "Digunakan dalam kontrol kecepatan motor." },
  { "en": "Apa itu 'accelerated aging test'?", "id": "Tes mempercepat penuaan produk." },
  { "en": "Tujuan 'accelerated aging test' forensik?", "id": "Memprediksi dan memahami mekanisme kegagalan." },
  { "en": "Apa itu 'wire drawing'?", "id": "Penipisan kawat akibat tarikan/pemanasan." },
  { "en": "Analisis 'wire drawing' forensik?", "id": "Menunjukkan area tegangan mekanik tinggi." },
  { "en": "Pemeriksaan kabel pita (ribbon cable)?", "id": "Robek, konduktor putus, konektor rusak." },
  { "en": "Apa itu 'Zener diode'?", "id": "Dioda untuk meregulasi tegangan." },
  { "en": "Kegagalan 'Zener diode'?", "id": "Korsleting atau sirkuit terbuka." },
  { "en": "Apa itu 'heat sink'?", "id": "Komponen penyerap panas dari elektronik." },
  { "en": "Pemeriksaan 'heat sink'?", "id": "Pemasangan longgar, pasta termal kering." },
  { "en": "Apa itu 'thermal paste'?", "id": "Material penghantar panas antar komponen." },
  { "en": "Pentingnya 'thermal paste'?", "id": "Memastikan transfer panas yang efisien." },
  { "en": "Analisis kegagalan 'Hall effect sensor'?", "id": "Sensor medan magnet, bisa gagal." },
  { "en": "Apa itu 'current limiting resistor'?", "id": "Resistor pembatas arus, contohnya di LED." },
  { "en": "Kegagalan 'current limiting resistor'?", "id": "Terbakar karena daya terlalu besar." },
  { "en": "Apa itu 'leakage current'?", "id": "Arus kecil mengalir melalui isolator." },
  { "en": "Kapan 'leakage current' berbahaya?", "id": "Jika cukup besar untuk kejut/api." },
  { "en": "Pemeriksaan pada 'solder mask' PCB?", "id": "Lapisan pelindung dari hubungan singkat." },
  { "en": "Kerusakan 'solder mask' sebabkan?", "id": "Korosi dan korsleting pada jalur PCB." },
  { "en": "Apa itu 'first-party spoliation'?", "id": "Perusakan bukti oleh pihak terlibat." },
  { "en": "Apa itu 'third-party spoliation'?", "id": "Perusakan bukti oleh pihak luar." },
  { "en": "Pemeriksaan sistem anti-statis?", "id": "Gelang, alas, dan grounding." },
  { "en": "Tujuan sistem anti-statis?", "id": "Mencegah kerusakan komponen elektronik sensitif." },
  { "en": "Apa itu 'hot joint'?", "id": "Sama dengan 'glowing connection'." },
  { "en": "Pemeriksaan 'load bank'?", "id": "Alat untuk menguji sistem daya." },
  { "en": "Fungsi animasi forensik 3D?", "id": "Visualisasi insiden untuk non-ahli." },
  { "en": "Apa itu X-Ray Diffraction (XRD)?", "id": "Teknik identifikasi material kristalin." },
  { "en": "Kegunaan XRD dalam forensik?", "id": "Analisis korosi atau residu tak dikenal." },
  { "en": "Apa itu Differential Scanning Calorimetry (DSC)?", "id": "Mengukur sifat termal suatu material." },
  { "en": "Kegunaan DSC dalam forensik?", "id": "Analisis degradasi polimer atau plastik." },
  { "en": "Penyebab kegagalan listrik pesawat?", "id": "Gesekan kabel, kegagalan generator." },
  { "en": "Apa itu 'wire chafing'?", "id": "Isolasi kabel terkikis akibat gesekan." },
  { "en": "Forensik pada avionik (avionics)?", "id": "Analisis kegagalan sistem elektronik penerbangan." },
  { "en": "Apa itu gangguan EMC?", "id": "Satu perangkat mengganggu perangkat lain." },
  { "en": "Contoh kasus gangguan EMC?", "id": "Ponsel sebabkan malfungsi alat medis." },
  { "en": "Analisis data 'fault recorder' PLN?", "id": "Menentukan waktu pasti gangguan jaringan." },
  { "en": "Informasi dari 'fault recorder'?", "id": "Besaran arus, tegangan, durasi gangguan." },
  { "en": "Apa itu 'cold flow' isolasi?", "id": "Deformasi permanen isolasi akibat tekanan." },
  { "en": "Bahaya dari 'cold flow'?", "id": "Penipisan isolasi, risiko hubungan singkat." },
  { "en": "Tanda kerusakan kabel oleh hewan?", "id": "Bekas gigitan atau cakaran spesifik." },
  { "en": "Kombinasi forensik listrik & akuntansi?", "id": "Membuktikan kerugian akibat pencurian listrik." },
  { "en": "Analisis spektrum suara busur api?", "id": "Memiliki frekuensi khas yang lebar." },
  { "en": "Apa itu 'contact welding'?", "id": "Kontak saklar atau relay melekat." },
  { "en": "Penyebab 'contact welding'?", "id": "Arus tinggi saat membuka atau menutup." },
  { "en": "Peran ahli forensik subrogasi?", "id": "Membantu asuransi menuntut pihak ketiga." },
  { "en": "Apa itu LIBS (Laser-Induced Breakdown Spectroscopy)?", "id": "Analisis komposisi elemen dengan laser." },
  { "en": "Kegunaan LIBS di TKP?", "id": "Analisis material cepat tanpa persiapan." },
  { "en": "Apa itu 'failure modes and effects analysis' (FMEA)?", "id": "Metodologi analisis risiko kegagalan produk." },
  { "en": "Pentingnya FMEA dalam forensik?", "id": "Identifikasi potensi cacat desain produk." },
  { "en": "Pemeriksaan 'printed circuit board delamination'?", "id": "Pemisahan lapisan-lapisan pada papan PCB." },
  { "en": "Penyebab PCB 'delamination'?", "id": "Panas berlebih atau cacat produksi." },
  { "en": "Apa itu 'work hardening' pada logam?", "id": "Logam menjadi keras setelah deformasi." },
  { "en": "Pentingnya 'work hardening' forensik?", "id": "Analisis kegagalan mekanik pada konduktor." },
  { "en": "Pemeriksaan pada 'programmable logic controller' (PLC)?", "id": "Analisis input, output, dan programnya." },
  { "en": "Fungsi PLC dalam industri?", "id": "Otak kontrol mesin-mesin otomatis." },
  { "en": "Apa itu 'latent print' pada bukti?", "id": "Sidik jari tak terlihat." },
  { "en": "Cara menemukan 'latent print'?", "id": "Menggunakan serbuk atau bahan kimia." },
  { "en": "Pemeriksaan 'oil-filled circuit breaker'?", "id": "Analisis kualitas dan kontaminasi minyak." },
  { "en": "Fungsi minyak pada pemutus sirkuit?", "id": "Isolasi dan pendingin pemadam busur." },
  { "en": "Apa itu 'gas chromatograph' portabel?", "id": "Alat deteksi senyawa kimia di lapangan." },
  { "en": "Fungsi 'gas chromatograph' portabel?", "id": "Deteksi uap akseleran di TKP." },
  { "en": "Apa itu 'ultrasonic testing'?", "id": "Pengujian material menggunakan gelombang suara." },
  { "en": "Kegunaan 'ultrasonic testing' forensik?", "id": "Deteksi retakan internal pada komponen." },
  { "en": "Analisis kegagalan 'switchgear'?", "id": "Pemeriksaan busbar, pemutus, dan relay." },
  { "en": "Apa itu 'arc chute'?", "id": "Komponen pemadam busur pada saklar." },
  { "en": "Kegagalan 'arc chute'?", "id": "Gagal memadamkan busur api, sebabkan kerusakan." },
  { "en": "Pemeriksaan sistem 'emergency power off' (EPO)?", "id": "Tombol pemutus darurat sistem besar." },
  { "en": "Pentingnya sistem EPO?", "id": "Keselamatan saat terjadi insiden darurat." },
  { "en": "Apa itu 'coefficient of performance' (COP)?", "id": "Ukuran efisiensi pompa kalor/pendingin." },
  { "en": "Hubungan COP dengan forensik?", "id": "Analisis kegagalan sistem HVAC." },
  { "en": "Analisis 'solder splash' pada PCB?", "id": "Percikan solder bisa sebabkan korsleting." },
  { "en": "Apa itu 'pinhole failure' pada isolasi?", "id": "Lubang kecil pada isolasi kabel." },
  { "en": "Bahaya 'pinhole failure'?", "id": "Titik awal arus bocor/korsleting." },
  { "en": "Pemeriksaan pada 'flexible printed circuit' (FPC)?", "id": "Cari retakan akibat tekukan berulang." },
  { "en": "Penggunaan FPC di mana?", "id": "Ponsel, kamera, dan perangkat portabel." },
  { "en": "Apa itu 'thermal cycling'?", "id": "Perubahan suhu berulang (panas-dingin)." },
  { "en": "Dampak 'thermal cycling'?", "id": "Menyebabkan kelelahan material dan kegagalan." },
  { "en": "Apa itu 'neutral inversion'?", "id": "Kondisi netral menjadi bertegangan." },
  { "en": "Bahaya 'neutral inversion'?", "id": "Risiko sengatan, kerusakan peralatan." },
  { "en": "Analisis pada 'ball grid array' (BGA)?", "id": "Sambungan solder di bawah chip." },
  { "en": "Kegagalan pada BGA?", "id": "Retak pada bola solder kecil." },
  { "en": "Apa itu 'black box warning'?", "id": "Peringatan paling serius pada obat/alat." },
  { "en": "Hubungan 'black box warning' forensik?", "id": "Menunjukkan produsen tahu risiko produk." },
  { "en": "Apa itu 'destructive scrape' pada kabel?", "id": "Goresan yang merusak isolasi konduktor." },
  { "en": "Pemeriksaan 'conduit' (pipa kabel)?", "id": "Cari korosi, kerusakan, atau air." },
  { "en": "Apa itu 'eddy current testing'?", "id": "Metode NDT untuk deteksi cacat permukaan." },
  { "en": "Pemeriksaan pada sistem 'data center'?", "id": "Analisis UPS, PDU, dan pendinginan." },
  { "en": "Apa itu 'Power Distribution Unit' (PDU)?", "id": "Unit distribusi daya di rak server." },
  { "en": "Kegagalan PDU sebabkan apa?", "id": "Matinya seluruh rak server." },
  { "en": "Apa itu 'dendrite' pada lelehan?", "id": "Struktur kristal seperti pakis/pohon." },
  { "en": "Arti dari 'dendrite'?", "id": "Pendinginan cepat, khas lelehan listrik." },
  { "en": "Apa itu 'equiaxed grains'?", "id": "Butiran kristal berukuran hampir sama." },
  { "en": "Arti 'equiaxed grains'?", "id": "Pendinginan lambat, khas lelehan api." },
  { "en": "Apa itu 'voltage divider'?", "id": "Sirkuit untuk menghasilkan tegangan lebih rendah." },
  { "en": "Kegagalan 'voltage divider'?", "id": "Resistor terbakar, tegangan output salah." },
  { "en": "Analisis pada 'liquid-crystal display' (LCD)?", "id": "Periksa backlight, inverter, dan driver." },
  { "en": "Penyebab kegagalan 'backlight' LCD?", "id": "Lampu CCFL atau LED rusak." },
  { "en": "Apa itu 'cold solder joint'?", "id": "Sambungan solder tidak matang sempurna." },
  { "en": "Ciri 'cold solder joint'?", "id": "Permukaan kasar, retak, atau kusam." },
  { "en": "Apa itu 'witness mark'?", "id": "Bekas fisik interaksi antar komponen." },
  { "en": "Contoh 'witness mark'?", "id": "Goresan pada selungkup dari kabel." },
  { "en": "Apa itu 'cause and origin' report?", "id": "Laporan penyebab dan titik awal insiden." },
  { "en": "Pemeriksaan 'immersion heater'?", "id": "Pemanas air celup, periksa isolasi." },
  { "en": "Apa itu 'strain gauge'?", "id": "Sensor pengukur regangan atau deformasi." },
  { "en": "Kegunaan 'strain gauge' forensik?", "id": "Analisis kegagalan mekanik suatu struktur." },
  { "en": "Apa itu 'hertz' (Hz)?", "id": "Satuan frekuensi, siklus per detik." },
  { "en": "Frekuensi listrik PLN?", "id": "50 Hertz (Hz)." },
  { "en": "Pemeriksaan 'electric fence controller'?", "id": "Analisis output tegangan dan energi." },
  { "en": "Pemeriksaan sistem elevator/lift?", "id": "Analisis kontroler, motor, dan sensor." },
  { "en": "Apa itu 'regenerative braking'?", "id": "Pengereman yang menghasilkan energi listrik." },
  { "en": "Hubungan 'regenerative braking' forensik?", "id": "Analisis pada sistem lift atau EV." },
  { "en": "Apa itu 'pyrolysis'?", "id": "Dekomposisi kimia oleh panas." },
  { "en": "Pentingnya 'pyrolysis' dalam kebakaran?", "id": "Proses menghasilkan gas yang mudah terbakar." },
  { "en": "Apa itu 'flash point'?", "id": "Suhu terendah uap bisa menyala." },
  { "en": "Apa itu 'ignition temperature'?", "id": "Suhu di mana material menyala sendiri." },
  { "en": "Analisis pada 'magnetic starter'?", "id": "Kontaktor dan pelindung beban lebih." },
  { "en": "Apa itu 'human-computer interaction' (HCI) log?", "id": "Log interaksi pengguna dengan sistem." },
  { "en": "Kegunaan log HCI forensik?", "id": "Mengetahui apa yang dilakukan operator." },
  { "en": "Apa itu 'over-voltage protection' (OVP)?", "id": "Sirkuit proteksi dari tegangan lebih." },
  { "en": "Apa itu 'under-voltage lockout' (UVLO)?", "id": "Mematikan sirkuit saat tegangan rendah." },
  { "en": "Analisis linguistik manual produk?", "id": "Mengevaluasi kejelasan instruksi dan peringatan." },
  { "en": "Apa itu 'foreseeable misuse'?", "id": "Kesalahan penggunaan produk yang dapat diprediksi." },
  { "en": "Investigasi kegagalan tungku induksi?", "id": "Analisis koil, catu daya, pendingin." },
  { "en": "Bahaya utama tungku induksi?", "id": "Kebocoran air pendingin ke logam cair." },
  { "en": "Analisis debu mikro pada PCB?", "id": "Identifikasi kontaminan lingkungan atau proses." },
  { "en": "Kegunaan data meteorologi mikro?", "id": "Korelasi insiden dengan angin atau petir lokal." },
  { "en": "Apa itu 'latch-up' pada CMOS?", "id": "Kondisi korsleting internal pada chip." },
  { "en": "Penyebab 'latch-up' pada chip?", "id": "Lonjakan tegangan atau radiasi." },
  { "en": "Forensik pada 'tidal turbine'?", "id": "Analisis korosi air laut, segel, generator." },
  { "en": "Analisis log HMI (Human-Machine Interface)?", "id": "Rekonstruksi tindakan operator sebelum insiden." },
  { "en": "Pemeriksaan forensik kabel fiber optik?", "id": "Cari retakan, bengkokan, ujung terbakar." },
  { "en": "Bagaimana fiber optik bisa rusak api?", "id": "Jaket pelindung terbakar, merusak serat." },
  { "en": "Reverse engineering untuk kasus paten?", "id": "Membuktikan peniruan desain sirkuit." },
  { "en": "Ciri komponen elektronik palsu?", "id": "Cetakan buruk, spesifikasi tidak cocok." },
  { "en": "Bahaya komponen elektronik palsu?", "id": "Kegagalan dini, tidak ada proteksi." },
  { "en": "Forensik pada 'smartwatch' terbakar?", "id": "Analisis baterai dan sirkuit pengisian." },
  { "en": "Apa itu 'gate oxide breakdown'?", "id": "Kegagalan lapisan isolasi pada transistor." },
  { "en": "Penyebab 'gate oxide breakdown'?", "id": "Listrik statis (ESD) atau tegangan lebih." },
  { "en": "Apa itu 'Electrostatic Discharge' (ESD)?", "id": "Pelepasan muatan listrik statis." },
  { "en": "Analisis kegagalan akibat ESD?", "id": "Kerusakan mikroskopis pada semikonduktor." },
  { "en": "Pemeriksaan pada proses elektroplating?", "id": "Analisis catu daya, anoda, katoda." },
  { "en": "Bahaya listrik pada elektroplating?", "id": "Arus tinggi, cairan konduktif korosif." },
  { "en": "Apa itu 'wearable technology' forensik?", "id": "Investigasi perangkat yang dipakai tubuh." },
  { "en": "Pemeriksaan 'electric vehicle battery pack'?", "id": "Analisis sel individual, BMS, konektor." },
  { "en": "Apa itu 'cascading failure' baterai?", "id": "Satu sel gagal, memicu sel lain." },
  { "en": "Fungsi 'contactor' pada baterai EV?", "id": "Relay besar untuk menghubungkan/memutus daya." },
  { "en": "Apa itu 'focal depth analysis'?", "id": "Analisis kedalaman kerusakan akibat panas." },
  { "en": "Kegunaan 'focal depth analysis'?", "id": "Menentukan arah dan durasi panas." },
  { "en": "Apa itu 'Infrared Spectroscopy' (FTIR)?", "id": "Analisis untuk identifikasi material organik." },
  { "en": "Kegunaan FTIR dalam forensik?", "id": "Identifikasi jenis plastik atau isolasi kabel." },
  { "en": "Apa itu 'anechoic chamber'?", "id": "Ruangan pengujian bebas gema radio." },
  { "en": "Fungsi 'anechoic chamber' forensik?", "id": "Menguji emisi EMC dari suatu perangkat." },
  { "en": "Apa itu 'interrogation' barang bukti?", "id": "Proses mengekstrak data dari perangkat." },
  { "en": "Pemeriksaan pada 'brushless DC motor'?", "id": "Analisis kontroler elektronik dan sensor." },
  { "en": "Apa itu 'back-drive'?", "id": "Gaya eksternal memutar motor." },
  { "en": "Bahaya 'back-drive' pada robot?", "id": "Bisa hasilkan tegangan tak terduga." },
  { "en": "Apa itu 'class action lawsuit'?", "id": "Gugatan hukum oleh kelompok besar." },
  { "en": "Hubungan 'class action' dan forensik?", "id": "Satu analisis forensik untuk banyak penggugat." },
  { "en": "Apa itu 'probabilistic risk assessment'?", "id": "Analisis kuantitatif risiko suatu kegagalan." },
  { "en": "Pemeriksaan pada 'audio amplifier'?", "id": "Analisis transistor output dan catu daya." },
  { "en": "Apa itu 'clipping' pada amplifier?", "id": "Distorsi sinyal akibat daya berlebih." },
  { "en": "Dampak 'clipping' pada speaker?", "id": "Dapat merusak voice coil speaker." },
  { "en": "Apa itu 'accelerometer log'?", "id": "Log data dari sensor percepatan." },
  { "en": "Kegunaan 'accelerometer log' forensik?", "id": "Rekonstruksi guncangan atau jatuh perangkat." },
  { "en": "Pemeriksaan 'wire bonding' dalam chip?", "id": "Kabel emas kecil penghubung die." },
  { "en": "Kegagalan 'wire bonding'?", "id": "Putus akibat getaran atau korosi." },
  { "en": "Apa itu 'semiconductor die'?", "id": "Inti silikon dari sebuah chip." },
  { "en": "Analisis 'semiconductor die'?", "id": "Pemeriksaan sirkuit dengan mikroskop kuat." },
  { "en": "Apa itu 'hot embossing'?", "id": "Jejak leleh plastik pada benda lain." },
  { "en": "Pentingnya 'hot embossing'?", "id": "Menunjukkan kontak saat material panas." },
  { "en": "Pemeriksaan sistem 'fly-by-wire'?", "id": "Sistem kontrol elektronik tanpa kabel mekanik." },
  { "en": "Penerapan 'fly-by-wire'?", "id": "Pesawat modern, mobil, dan mesin." },
  { "en": "Apa itu 'state machine analysis'?", "id": "Analisis status logis suatu sistem." },
  { "en": "Kegunaan 'state machine analysis'?", "id": "Memahami perilaku software sebelum gagal." },
  { "en": "Pemeriksaan 'linear actuator'?", "id": "Analisis motor, gearbox, dan sensor." },
  { "en": "Apa itu 'brownout'?", "id": "Penurunan tegangan listrik yang disengaja." },
  { "en": "Beda 'brownout' dan 'voltage sag'?", "id": "Brownout disengaja, sag tidak disengaja." },
  { "en": "Pemeriksaan 'electrosurgical unit' (ESU)?", "id": "Analisis penyebab luka bakar pasien." },
  { "en": "Bahaya dari ESU?", "id": "Luka bakar di luar area operasi." },
  { "en": "Pemeriksaan 'return electrode' ESU?", "id": "Kontak buruk sebabkan luka bakar." },
  { "en": "Apa itu 'capacitive coupling'?", "id": "Transfer energi listrik tanpa kontak." },
  { "en": "Bahaya 'capacitive coupling' medis?", "id": "Dapat menyebabkan luka bakar internal." },
  { "en": "Apa itu 'wear level' pada flash memory?", "id": "Ukuran tingkat keausan sel memori." },
  { "en": "Kegunaan 'wear level' forensik?", "id": "Memperkirakan umur dan penggunaan perangkat." },
  { "en": "Apa itu 'hydrogen embrittlement'?", "id": "Logam menjadi rapuh karena hidrogen." },
  { "en": "Hubungan 'hydrogen embrittlement' forensik?", "id": "Kegagalan baut atau komponen struktural." },
  { "en": "Pemeriksaan pada 'nickel-cadmium' (NiCd) baterai?", "id": "Cari 'efek memori' dan kristalisasi." },
  { "en": "Apa itu 'efek memori' baterai?", "id": "Penurunan kapasitas jika tidak dikosongkan penuh." },
  { "en": "Pemeriksaan 'nickel-metal hydride' (NiMH) baterai?", "id": "Cari kebocoran elektrolit, panas berlebih." },
  { "en": "Apa itu 'thermal imaging microscopy'?", "id": "Pencitraan termal pada skala mikro." },
  { "en": "Kegunaan 'thermal imaging microscopy'?", "id": "Menemukan titik panas pada die chip." },
  { "en": "Apa itu 'root cause analysis'?", "id": "Metodologi menemukan akar penyebab masalah." },
  { "en": "Langkah pertama 'root cause analysis'?", "id": "Definisikan masalah secara jelas." },
  { "en": "Pemeriksaan 'laser diode'?", "id": "Analisis degradasi, kerusakan akibat panas." },
  { "en": "Apa itu 'catastrophic optical damage'?", "id": "Kerusakan cermin pada dioda laser." },
  { "en": "Pemeriksaan 'stepper motor'?", "id": "Analisis driver, gulungan, dan mekanik." },
  { "en": "Apa itu 'phase angle'?", "id": "Perbedaan waktu antara tegangan dan arus." },
  { "en": "Apa itu 'power factor'?", "id": "Ukuran efisiensi penggunaan daya listrik." },
  { "en": "Penyebab 'power factor' buruk?", "id": "Banyaknya beban induktif seperti motor." },
  { "en": "Analisis pada 'cryogenic equipment'?", "id": "Kegagalan isolasi vakum, sensor suhu." },
  { "en": "Pemeriksaan pada 'welding robot'?", "id": "Analisis kontroler, kabel, dan mekanisme." },
  { "en": "Apa itu 'ionizing radiation sensor'?", "id": "Sensor pendeteksi radiasi pengion." },
  { "en": "Forensik pada 'radiation sensor'?", "id": "Verifikasi paparan radiasi dalam insiden." },
  { "en": "Apa itu 'destructive physical analysis' (DPA)?", "id": "Analisis merusak untuk memeriksa komponen." },
  { "en": "Tujuan dari DPA?", "id": "Memverifikasi kualitas dan keandalan komponen." },
  { "en": "Pemeriksaan 'printed electronics'?", "id": "Sirkuit yang dicetak pada substrat fleksibel." },
  { "en": "Kegagalan 'printed electronics'?", "id": "Retak, delaminasi, degradasi tinta konduktif." },
  { "en": "Apa itu 'silver migration'?", "id": "Perpindahan perak sebabkan korsleting." },
  { "en": "Penyebab 'silver migration'?", "id": "Kelembaban dan tegangan listrik." },
  { "en": "Pemeriksaan 'rotary encoder'?", "id": "Sensor posisi sudut, periksa disk/optik." },
  { "en": "Apa itu 'gray code'?", "id": "Sistem pengkodean pada encoder." },
  { "en": "Apa itu 'inter-symbol interference'?", "id": "Gangguan dalam transmisi data digital." },
  { "en": "Apa itu 'eye diagram'?", "id": "Visualisasi kualitas sinyal digital." },
  { "en": "Kegunaan 'eye diagram' forensik?", "id": "Analisis penyebab kegagalan komunikasi data." },
  { "en": "Apa itu 'JTAG' (Joint Test Action Group)?", "id": "Antarmuka untuk debugging sirkuit." },
  { "en": "Kegunaan JTAG dalam forensik?", "id": "Mengekstrak firmware atau data dari chip." },
  { "en": "Pemeriksaan sistem 'solar thermal'?", "id": "Analisis pompa, kontroler, dan sensor." },
  { "en": "Apa itu 'system-on-chip' (SoC)?", "id": "Seluruh sistem dalam satu chip." },
  { "en": "Fungsi acoustic microscopy forensik?", "id": "Deteksi delaminasi atau rongga internal." },
  { "en": "Apa itu 'stiction' pada MEMS?", "id": "Kondisi komponen mikro saling menempel." },
  { "en": "Kegunaan GPR (Ground Penetrating Radar) forensik?", "id": "Melacak kabel atau pipa ilegal." },
  { "en": "Analisis kegagalan perangkat MEMS?", "id": "Stiction, retak mikro, partikel debu." },
  { "en": "Tantangan listrik di lingkungan vakum?", "id": "Outgassing material, pelepasan panas." },
  { "en": "Apa itu 'multipactor effect'?", "id": "Elektron beresonansi dalam vakum RF." },
  { "en": "Apa itu 'race condition' software?", "id": "Bug akibat urutan eksekusi tak terduga." },
  { "en": "Contoh 'race condition' fatal?", "id": "Dua perintah bertabrakan, sebabkan overheat." },
  { "en": "Forensik pada material superkonduktor?", "id": "Analisis 'quenching' atau kehilangan superkonduktivitas." },
  { "en": "Apa itu 'quenching' superkonduktor?", "id": "Transisi mendadak ke kondisi resistif." },
  { "en": "Investigasi kepatuhan 'recall' produk?", "id": "Memeriksa apakah produsen bertindak benar." },
  { "en": "Apa itu 'triboelectric series'?", "id": "Daftar material berdasarkan kecenderungan elektron." },
  { "en": "Pola sabotase untuk fraud asuransi?", "id": "Kerusakan tampak lebih parah dari seharusnya." },
  { "en": "Analisis kegagalan 'heat pipe'?", "id": "Kehilangan vakum, sumbu (wick) kering." },
  { "en": "Fungsi 'heat pipe' elektronik?", "id": "Memindahkan panas secara sangat efisien." },
  { "en": "Apa itu 'inverse forensic analysis'?", "id": "Menentukan kondisi awal dari hasil akhir." },
  { "en": "Pemeriksaan pada 'thermoelectric generator' (TEG)?", "id": "Analisis degradasi material dan sambungan." },
  { "en": "Fungsi TEG?", "id": "Mengubah panas langsung menjadi listrik." },
  { "en": "Apa itu 'atomic force microscopy' (AFM)?", "id": "Pencitraan permukaan resolusi tingkat atom." },
  { "en": "Kegunaan AFM dalam forensik?", "id": "Analisis keausan atau cacat mikro." },
  { "en": "Apa itu 'lock-in thermography'?", "id": "Teknik deteksi 'hot spot' sensitivitas tinggi." },
  { "en": "Forensik pada sistem 'wireless charging'?", "id": "Analisis koil, sirkuit kontrol, overheating." },
  { "en": "Apa itu 'foreign object detection'?", "id": "Fitur keamanan pada wireless charging." },
  { "en": "Kegagalan 'foreign object detection'?", "id": "Benda logam ikut panas, picu api." },
  { "en": "Analisis 'event reconstruction'?", "id": "Membangun kembali urutan kejadian insiden." },
  { "en": "Apa itu 'photonic integrated circuit' (PIC)?", "id": "Sirkuit yang memproses data dengan cahaya." },
  { "en": "Forensik pada PIC?", "id": "Analisis kerusakan waveguide atau laser." },
  { "en": "Apa itu 'quantum tunneling'?", "id": "Partikel menembus penghalang energi." },
  { "en": "Efek 'quantum tunneling' pada flash memory?", "id": "Kebocoran muatan, kehilangan data." },
  { "en": "Pemeriksaan pada 'haptic feedback system'?", "id": "Analisis motor getar, aktuator piezo." },
  { "en": "Apa itu 'fretting' pada konektor?", "id": "Oksidasi akibat gerakan mikro berulang." },
  { "en": "Beda 'fretting' dan korosi biasa?", "id": "Fretting memerlukan getaran atau gerakan." },
  { "en": "Apa itu 'highly accelerated life test' (HALT)?", "id": "Pengujian untuk menemukan titik lemah produk." },
  { "en": "Tujuan HALT dalam forensik?", "id": "Membuktikan kelemahan desain yang ada." },
  { "en": "Apa itu 'shielding effectiveness'?", "id": "Kemampuan selungkup memblokir medan EM." },
  { "en": "Pengujian 'shielding effectiveness'?", "id": "Penting dalam kasus spionase atau EMC." },
  { "en": "Apa itu 'magnetic flux leakage'?", "id": "Metode NDT untuk deteksi korosi/retak." },
  { "en": "Pemeriksaan 'permanent magnet motor'?", "id": "Analisis demagnetisasi akibat panas." },
  { "en": "Apa itu demagnetisasi?", "id": "Kehilangan sifat kemagnetan secara permanen." },
  { "en": "Pemeriksaan pada 'optical switch'?", "id": "Analisis cermin MEMS atau kristal." },
  { "en": "Apa itu 'paschen's law'?", "id": "Hukum tegangan tembus gas." },
  { "en": "Penerapan 'paschen's law' forensik?", "id": "Analisis busur api pada berbagai tekanan." },
  { "en": "Apa itu 'charge trapping'?", "id": "Muatan terperangkap dalam lapisan isolator." },
  { "en": "Dampak 'charge trapping'?", "id": "Pergeseran tegangan, kegagalan transistor." },
  { "en": "Pemeriksaan 'explosively formed fuse' (EFF)?", "id": "Sekring kecepatan tinggi untuk arus besar." },
  { "en": "Apa itu 'whistleblower' dalam kasus produk?", "id": "Orang dalam yang mengungkap cacat produk." },
  { "en": "Pemeriksaan 'rotary joint' frekuensi tinggi?", "id": "Analisis keausan dan kebocoran sinyal." },
  { "en": "Apa itu 'spintronics'?", "id": "Elektronik berbasis spin elektron." },
  { "en": "Forensik pada memori MRAM?", "id": "Analisis kegagalan pada sel memori magnetik." },
  { "en": "Apa itu 'software defined radio' (SDR)?", "id": "Radio yang fungsinya ditentukan software." },
  { "en": "Forensik pada SDR?", "id": "Analisis konfigurasi software untuk aktivitas ilegal." },
  { "en": "Apa itu 'material fatigue'?", "id": "Kelelahan material akibat beban berulang." },
  { "en": "Apa itu 'S-N curve'?", "id": "Grafik hubungan tegangan dan siklus kegagalan." },
  { "en": "Kegunaan 'S-N curve' forensik?", "id": "Memperkirakan sisa umur suatu komponen." },
  { "en": "Pemeriksaan 'gasket' EMI?", "id": "Gasket konduktif untuk penyekat elektromagnetik." },
  { "en": "Kegagalan 'gasket' EMI?", "id": "Kehilangan konduktivitas, korosi, robek." },
  { "en": "Apa itu 'near-field communication' (NFC)?", "id": "Komunikasi nirkabel jarak sangat dekat." },
  { "en": "Forensik pada sistem pembayaran NFC?", "id": "Analisis penyadapan atau manipulasi data." },
  { "en": "Apa itu 'Arrhenius model'?", "id": "Model prediksi laju reaksi kimia." },
  { "en": "Kegunaan 'Arrhenius model' forensik?", "id": "Memperkirakan penuaan isolasi akibat suhu." },
  { "en": "Apa itu 'ferrite bead'?", "id": "Komponen untuk menekan noise frekuensi tinggi." },
  { "en": "Kegagalan 'ferrite bead'?", "id": "Retak atau hancur akibat tekanan/panas." },
  { "en": "Pemeriksaan pada 'linear motor'?", "id": "Analisis track, forcer, dan sensor posisi." },
  { "en": "Apa itu 'Finite Impulse Response' (FIR) filter?", "id": "Filter digital dalam pemrosesan sinyal." },
  { "en": "Analisis forensik pada filter digital?", "id": "Rekonstruksi sinyal asli yang diproses." },
  { "en": "Apa itu 'electrochemical migration' (ECM)?", "id": "Pertumbuhan filamen logam sebabkan korsleting." },
  { "en": "Beda ECM dan 'silver migration'?", "id": "ECM istilah umum, silver migration spesifik." },
  { "en": "Apa itu 'clean room'?", "id": "Ruangan terkontrol bebas partikel debu." },
  { "en": "Pentingnya 'clean room' forensik?", "id": "Mencegah kontaminasi pada bukti mikroelektronik." },
  { "en": "Pemeriksaan 'surface acoustic wave' (SAW) filter?", "id": "Analisis kerusakan substrat atau transduser." },
  { "en": "Apa itu 'passive intermodulation' (PIM)?", "id": "Sinyal interferensi dari komponen pasif." },
  { "en": "Penyebab PIM?", "id": "Konektor longgar, korosi, material feromagnetik." },
  { "en": "Pemeriksaan pada 'digital signal processor' (DSP)?", "id": "Analisis kode dan eksekusi algoritma." },
  { "en": "Apa itu 'watchdog timer'?", "id": "Timer untuk mereset sistem macet." },
  { "en": "Kegagalan 'watchdog timer'?", "id": "Sistem gagal reset, macet permanen." },
  { "en": "Apa itu 'Design for Testability' (DFT)?", "id": "Prinsip desain untuk memudahkan pengujian." },
  { "en": "Hubungan DFT dan forensik?", "id": "DFT bisa permudah ekstraksi data forensik." },
  { "en": "Apa itu 'Raman spectroscopy'?", "id": "Teknik analisis getaran molekuler." },
  { "en": "Kegunaan 'Raman spectroscopy'?", "id": "Identifikasi material, stres, dan suhu." },
  { "en": "Pemeriksaan 'thermoelectric cooler' (TEC)?", "id": "Analisis degradasi sambungan, efisiensi." },
  { "en": "Fungsi TEC (Peltier device)?", "id": "Mendinginkan atau memanaskan dengan listrik." },
  { "en": "Apa itu 'dark silicon'?", "id": "Bagian chip yang dimatikan hemat daya." },
  { "en": "Hubungan 'dark silicon' dan forensik?", "id": "Analisis manajemen daya sebelum kegagalan." },
  { "en": "Pemeriksaan 'electron gun' (misal: CRT)?", "id": "Analisis katoda, filamen, dan lensa." },
  { "en": "Apa itu 'field-programmable gate array' (FPGA)?", "id": "Chip yang konfigurasinya bisa diprogram." },
  { "en": "Forensik pada FPGA?", "id": "Analisis bitstream konfigurasi yang dimuat." },
  { "en": "Apa itu 'bitstream'?", "id": "File data konfigurasi untuk FPGA." },
  { "en": "Apa itu 'design rule check' (DRC)?", "id": "Pemeriksaan desain chip terhadap aturan." },
  { "en": "Hubungan DRC dan forensik?", "id": "Melanggar DRC bisa sebabkan kegagalan." },
  { "en": "Pemeriksaan pada 'plasma display'?", "id": "Analisis sel, elektroda, dan driver." },
  { "en": "Apa itu 'burn-in' pada layar?", "id": "Bayangan permanen akibat gambar statis." },
  { "en": "Apa itu 'radiation hardening'?", "id": "Membuat komponen tahan terhadap radiasi." },
  { "en": "Forensik pada komponen 'rad-hard'?", "id": "Analisis kegagalan di lingkungan radiasi." },
  { "en": "Analisis historis standar kelistrikan?", "id": "Memahami alasan di balik suatu aturan." },
  { "en": "Pengaruh budaya pada investigasi forensik?", "id": "Perbedaan pendekatan pada perawatan dan keselamatan." },
  { "en": "Analisis 'tractography' korban sengatan?", "id": "Memetakan kerusakan jalur saraf otak." },
  { "en": "Forensik kegagalan akibat badai matahari?", "id": "Analisis data GIC (arus induksi)." },
  { "en": "Apa itu Geomagnetically Induced Current (GIC)?", "id": "Arus liar diinduksi badai matahari." },
  { "en": "Dampak GIC pada jaringan listrik?", "id": "Saturasi trafo, pemadaman skala luas." },
  { "en": "Fungsi simulasi Monte Carlo forensik?", "id": "Analisis probabilitas rantai kegagalan kompleks." },
  { "en": "Forensik pada Baterai Baghdad?", "id": "Analisis potensi fungsi sebagai sel galvanik." },
  { "en": "Fungsi 'taggant' kimia di isolasi?", "id": "Identifikasi produsen setelah bukti terbakar." },
  { "en": "Analisis kegagalan pada 'smart fabric'?", "id": "Konektor putus, korsleting oleh keringat." },
  { "en": "Kegunaan pencitraan Terahertz (THz)?", "id": "Melihat cacat internal material non-logam." },
  { "en": "Fungsi Cryo-EM dalam forensik?", "id": "Analisis sampel biologis tanpa merusak." },
  { "en": "Integritas bukti pada log SCADA?", "id": "Verifikasi checksum, timestamp, dan akses." },
  { "en": "Contoh konflik kepentingan ahli forensik?", "id": "Pernah bekerja untuk perusahaan tergugat." },
  { "en": "Apa itu 'expert witness testimony'?", "id": "Kesaksian ahli di muka pengadilan." },
  { "en": "Apa itu 'deposition' hukum?", "id": "Kesaksian di luar sidang di bawah sumpah." },
  { "en": "Pemeriksaan pada 'magnetohydrodynamic' (MHD) generator?", "id": "Analisis elektroda, saluran, dan magnet." },
  { "en": "Forensik pada 'holographic data storage'?", "id": "Analisis degradasi material dan laser." },
  { "en": "Apa itu 'phononic crystal'?", "id": "Material pengontrol aliran panas/suara." },
  { "en": "Hubungan 'phononic crystal' forensik?", "id": "Analisis kegagalan pada manajemen termal." },
  { "en": "Apa itu 'metamaterial'?", "id": "Material rekayasa dengan sifat tak biasa." },
  { "en": "Forensik pada 'metamaterial antenna'?", "id": "Analisis kegagalan struktur resonansinya." },
  { "en": "Apa itu 'digital twin'?", "id": "Model virtual dari aset fisik." },
  { "en": "Kegunaan 'digital twin' forensik?", "id": "Simulasi insiden pada model virtual identik." },
  { "en": "Apa itu 'transfer learning' AI?", "id": "AI belajar dari satu domain ke lain." },
  { "en": "Penerapan 'transfer learning' forensik?", "id": "AI deteksi pola kegagalan baru." },
  { "en": "Apa itu 'generative adversarial network' (GAN)?", "id": "AI untuk menghasilkan data palsu." },
  { "en": "Risiko GAN dalam forensik?", "id": "Pemalsuan log data atau bukti digital." },
  { "en": "Pemeriksaan 'quantum dot display' (QLED)?", "id": "Analisis degradasi quantum dot." },
  { "en": "Apa itu 'quantum dot'?", "id": "Nanokristal semikonduktor pemancar cahaya." },
  { "en": "Analisis 'focused ion beam' (FIB)?", "id": "Memotong dan menganalisis chip skala nano." },
  { "en": "Apa itu 'scanning electrochemical microscopy' (SECM)?", "id": "Memetakan aktivitas elektrokimia permukaan." },
  { "en": "Kegunaan SECM forensik?", "id": "Deteksi titik awal korosi mikro." },
  { "en": "Apa itu 'time-of-flight secondary ion mass spectrometry' (ToF-SIMS)?", "id": "Analisis komposisi permukaan sangat sensitif." },
  { "en": "Apa itu 'fault tree analysis' (FTA)?", "id": "Analisis deduktif untuk mencari akar penyebab." },
  { "en": "Beda FTA dan FMEA?", "id": "FTA top-down, FMEA bottom-up." },
  { "en": "Apa itu 'event tree analysis' (ETA)?", "id": "Analisis induktif respons sistem terhadap kejadian." },
  { "en": "Forensik pada 'neuromorphic computing' chip?", "id": "Chip meniru struktur otak manusia." },
  { "en": "Apa itu 'memristor'?", "id": "Komponen elektronik dengan properti memori." },
  { "en": "Kegagalan pada 'memristor'?", "id": "Degradasi material, 'stuck-on/off'." },
  { "en": "Apa itu 'optical tweezers'?", "id": "Alat laser untuk memanipulasi partikel." },
  { "en": "Kegunaan 'optical tweezers' forensik?", "id": "Memisahkan partikel mikro untuk analisis." },
  { "en": "Apa itu 'X-ray fluorescence' (XRF)?", "id": "Analisis komposisi elemen non-destruktif." },
  { "en": "Kegunaan XRF portabel?", "id": "Identifikasi material logam di TKP." },
  { "en": "Pemeriksaan sistem 'distributed acoustic sensing' (DAS)?", "id": "Fiber optik sebagai sensor getaran." },
  { "en": "Forensik dengan data DAS?", "id": "Deteksi aktivitas sebelum kegagalan struktur." },
  { "en": "Apa itu 'Li-Fi' (Light Fidelity)?", "id": "Transmisi data menggunakan cahaya LED." },
  { "en": "Forensik pada sistem Li-Fi?", "id": "Analisis interferensi atau kegagalan driver." },
  { "en": "Apa itu 'information asymmetry'?", "id": "Satu pihak punya info lebih banyak." },
  { "en": "Hubungan 'information asymmetry' forensik?", "id": "Produsen tahu cacat, konsumen tidak." },
  { "en": "Apa itu 'negligent entrustment'?", "id": "Memberi alat bahaya ke orang tak kompeten." },
  { "en": "Apa itu 'res ipsa loquitur'?", "id": "Doktrin hukum: 'kejadian berbicara sendiri'." },
  { "en": "Penerapan 'res ipsa loquitur'?", "id": "Saat kecelakaan mustahil tanpa kelalaian." },
  { "en": "Pemeriksaan 'triboelectric nanogenerator' (TENG)?", "id": "Pembangkit listrik dari gesekan." },
  { "en": "Forensik pada TENG?", "id": "Analisis keausan material, degradasi output." },
  { "en": "Apa itu 'Auger electron spectroscopy' (AES)?", "id": "Analisis komposisi elemen lapisan permukaan." },
  { "en": "Pemeriksaan pada 'concentrated solar power' (CSP)?", "id": "Analisis sistem cermin dan penerima." },
  { "en": "Pemeriksaan pada 'Stirling engine'?", "id": "Analisis pemanas, pendingin, dan segel." },
  { "en": "Apa itu 'cognitive forensics'?", "id": "Analisis proses pengambilan keputusan manusia." },
  { "en": "Penerapan 'cognitive forensics'?", "id": "Memahami mengapa operator membuat kesalahan." },
  { "en": "Apa itu 'near miss' analysis?", "id": "Analisis insiden yang hampir terjadi." },
  { "en": "Pentingnya 'near miss' analysis?", "id": "Mencegah kecelakaan sebenarnya di masa depan." },
  { "en": "Apa itu 'organizational accident'?", "id": "Kecelakaan akibat kegagalan sistemik organisasi." },
  { "en": "Pemeriksaan 'computed tomography' (CT) scan forensik?", "id": "Membuat citra 3D internal bukti." },
  { "en": "Beda CT scan dan X-ray?", "id": "CT scan 3D, X-ray 2D." },
  { "en": "Apa itu 'void analysis' pada solder?", "id": "Analisis rongga udara dalam sambungan solder." },
  { "en": "Dampak 'void' pada solder?", "id": "Melemahkan sambungan, sebabkan 'hot spot'." },
  { "en": "Pemeriksaan pada 'ultrasonic motor'?", "id": "Analisis elemen piezoelektrik dan stator." },
  { "en": "Apa itu 'stick-slip' pada motor?", "id": "Gerakan tidak mulus (berhenti-jalan)." },
  { "en": "Pemeriksaan 'power-line communication' (PLC) system?", "id": "Sistem data lewat kabel listrik." },
  { "en": "Gangguan pada 'power-line communication'?", "id": "Noise dari peralatan listrik lain." },
  { "en": "Apa itu 'sentinel event'?", "id": "Kejadian tak terduga sebabkan kematian/cedera." },
  { "en": "Contoh 'sentinel event' kelistrikan?", "id": "Sengatan listrik fatal di rumah sakit." },
  { "en": "Pemeriksaan 'RFID' (Radio-Frequency Identification) system?", "id": "Analisis tag, reader, dan interferensi." },
  { "en": "Penyadapan sinyal RFID?", "id": "Bisa mencuri data identifikasi." },
  { "en": "Apa itu 'Curie point' pyrolysis?", "id": "Teknik analisis material dengan pemanasan cepat." },
  { "en": "Pemeriksaan 'magneto-optical drive'?", "id": "Analisis laser dan lapisan magnetik." },
  { "en": "Apa itu 'edgewise winding'?", "id": "Teknik gulungan kawat untuk trafo." },
  { "en": "Apa itu 'electret microphone'?", "id": "Mikrofon dengan muatan listrik permanen." },
  { "en": "Kegagalan 'electret microphone'?", "id": "Kehilangan muatan, kontaminasi diafragma." },
  { "en": "Apa itu 'Lambertian emitter'?", "id": "Sumber cahaya memancar seragam semua arah." },
  { "en": "Hubungan 'Lambertian emitter' forensik?", "id": "Karakterisasi sumber cahaya seperti LED." },
  { "en": "Pemeriksaan 'galvanometer'?", "id": "Analisis koil, jarum, dan suspensi." },
  { "en": "Apa itu 'buck converter'?", "id": "Konverter penurun tegangan DC." },
  { "en": "Apa itu 'boost converter'?", "id": "Konverter penaik tegangan DC." },
  { "en": "Kegagalan umum konverter DC-DC?", "id": "Kegagalan saklar (MOSFET), dioda, induktor." },
  { "en": "Apa itu 'Lidar' (Light Detection and Ranging)?", "id": "Sensor pengukuran jarak pakai laser." },
  { "en": "Forensik pada sistem Lidar?", "id": "Analisis laser, detektor, dan mekanik." },
  { "en": "Apa itu 'dead band'?", "id": "Area di mana perubahan input diabaikan." },
  { "en": "Masalah 'dead band' pada kontrol?", "id": "Sistem tidak responsif terhadap perubahan kecil." },
  { "en": "Apa itu 'bifurcation' dalam sistem?", "id": "Perubahan perilaku kualitatif mendadak." },
  { "en": "Apa itu 'chaos theory' forensik?", "id": "Sistem kompleks bisa sangat sensitif." },
  { "en": "Apa itu 'plasmonics'?", "id": "Studi interaksi cahaya dengan elektron." },
  { "en": "Forensik pada perangkat 'plasmonics'?", "id": "Analisis degradasi struktur nano logam." },
  { "en": "Apa itu 'kalman filter'?", "id": "Algoritma estimasi untuk kurangi noise." },
  { "en": "Kegunaan 'kalman filter' forensik?", "id": "Rekonstruksi data sensor yang 'noisy'." },
  { "en": "Investigasi kegagalan AI sistem listrik?", "id": "Analisis data training dan algoritma." },
  { "en": "Keabsahan saksi ahli berupa AI?", "id": "Masih dalam perdebatan hukum dan etis." },
  { "en": "Forensik pada 'brain-computer interface' (BCI)?", "id": "Analisis elektroda, sinyal, dan software." },
  { "en": "Apa itu 'sonoluminescence'?", "id": "Emisi cahaya dari gelembung akustik." },
  { "en": "Hubungan 'sonoluminescence' forensik?", "id": "Fenomena langka pada kegagalan cairan." },
  { "en": "Peran 'blockchain' forensik komponen?", "id": "Menciptakan riwayat komponen tak terubah." },
  { "en": "Fungsi 'e-nose' di TKP?", "id": "Identifikasi senyawa kimia dari bau." },
  { "en": "Prediksi kegagalan listrik dengan AI?", "id": "Analisis data historis untuk temukan pola." },
  { "en": "Hubungan kegagalan hidrolik-listrik?", "id": "Kebocoran minyak merusak isolasi kabel." },
  { "en": "Apa itu ASIL dalam ISO 26262?", "id": "Tingkat risiko keselamatan fungsional otomotif." },
  { "en": "Forensik kegagalan 'quantum computer'?", "id": "Analisis 'decoherence' dan kontrol qubit." },
  { "en": "Apa itu 'quantum decoherence'?", "id": "Kehilangan sifat kuantum akibat lingkungan." },
  { "en": "Apa itu 'explainable AI' (XAI)?", "id": "AI yang bisa jelaskan keputusannya." },
  { "en": "Pentingnya XAI dalam forensik?", "id": "Memahami mengapa AI membuat keputusan salah." },
  { "en": "Forensik pada 'organic electronics' (OLED)?", "id": "Analisis degradasi material organik." },
  { "en": "Pemeriksaan 'DNA data storage'?", "id": "Analisis kerusakan untaian DNA." },
  { "en": "Apa itu 'adversarial attack' pada AI?", "id": "Input data untuk menipu AI." },
  { "en": "Contoh 'adversarial attack' forensik?", "id": "Manipulasi sensor untuk sebabkan kecelakaan." },
  { "en": "Apa itu 'zero-knowledge proof'?", "id": "Verifikasi pernyataan tanpa ungkap data." },
  { "en": "Hubungan 'zero-knowledge proof' forensik?", "id": "Verifikasi integritas bukti tanpa membuka." },
  { "en": "Apa itu 'homomorphic encryption'?", "id": "Enkripsi yang memungkinkan komputasi." },
  { "en": "Forensik pada data terenkripsi homomorfik?", "id": "Analisis hasil tanpa dekripsi data asli." },
  { "en": "Apa itu 'digital forensics readiness'?", "id": "Kesiapan organisasi mengumpulkan bukti digital." },
  { "en": "Apa itu 'annealing' pada polimer?", "id": "Proses pemanasan untuk mengurangi stres." },
  { "en": "Pemeriksaan 'glass transition temperature' (Tg)?", "id": "Suhu di mana polimer melunak." },
  { "en": "Pentingnya Tg dalam forensik?", "id": "Menentukan batas suhu operasional plastik." },
  { "en": "Apa itu 'Langevin dynamics'?", "id": "Model matematika gerak partikel." },
  { "en": "Fungsi 'Langevin dynamics' forensik?", "id": "Simulasi pergerakan ion dalam kegagalan baterai." },
  { "en": "Pemeriksaan 'time-domain reflectometry' (TDR) optik?", "id": "Mencari kerusakan pada kabel fiber." },
  { "en": "Apa itu 'single event upset' (SEU)?", "id": "Perubahan bit memori akibat radiasi." },
  { "en": "Dampak SEU pada sistem kritis?", "id": "Bisa menyebabkan kegagalan fungsi fatal." },
  { "en": "Apa itu 'Byzantine fault tolerance'?", "id": "Kemampuan sistem tangani komponen rusak." },
  { "en": "Pemeriksaan pada 'space-grade' komponen?", "id": "Analisis kegagalan di lingkungan luar angkasa." },
  { "en": "Forensik pada sistem 'hypersonic'?", "id": "Analisis kegagalan plasma dan panas ekstrem." },
  { "en": "Apa itu 'phase-change memory' (PCM)?", "id": "Memori non-volatil berbasis perubahan fasa." },
  { "en": "Kegagalan pada PCM?", "id": "Degradasi material, 'stuck-at' state." },
  { "en": "Apa itu 'spalling' pada beton?", "id": "Pecahnya permukaan beton akibat panas." },
  { "en": "Hubungan 'spalling' dengan forensik listrik?", "id": "Indikator panas hebat dari busur api." },
  { "en": "Apa itu 'scanning thermal microscopy' (SThM)?", "id": "Memetakan suhu permukaan skala nano." },
  { "en": "Kegunaan SThM forensik?", "id": "Menemukan 'hot spot' di dalam chip." },
  { "en": "Apa itu 'data remanence'?", "id": "Sisa data setelah dihapus." },
  { "en": "Forensik 'data remanence'?", "id": "Memulihkan data yang sengaja dihapus." },
  { "en": "Apa itu 'wear-out' failure?", "id": "Kegagalan yang terjadi di akhir umur." },
  { "en": "Beda 'wear-out' dan 'infant mortality'?", "id": "Infant mortality kegagalan di awal." },
  { "en": "Apa itu 'bathtub curve'?", "id": "Grafik laju kegagalan produk." },
  { "en": "Apa itu 'causal inference'?", "id": "Proses menyimpulkan hubungan sebab-akibat." },
  { "en": "Pentingnya 'causal inference' forensik?", "id": "Membedakan korelasi dan sebab-akibat." },
  { "en": "Apa itu 'counterfactual analysis'?", "id": "Analisis 'apa yang terjadi jika'." },
  { "en": "Pemeriksaan pada 'DC fast charger'?", "id": "Analisis sistem pendingin dan kontrol." },
  { "en": "Apa itu 'liquid cooling' elektronik?", "id": "Pendinginan komponen dengan cairan dielektrik." },
  { "en": "Kegagalan 'liquid cooling'?", "id": "Kebocoran, penyumbatan, kegagalan pompa." },
  { "en": "Forensik pada 'fusion reactor' (konseptual)?", "id": "Analisis kegagalan magnet dan sistem vakum." },
  { "en": "Apa itu 'tokamak'?", "id": "Desain reaktor fusi magnetik." },
  { "en": "Apa itu 'magnetic reconnection'?", "id": "Penyusunan ulang garis medan magnet." },
  { "en": "Hubungan 'magnetic reconnection' forensik?", "id": "Analisis ledakan plasma matahari atau fusi." },
  { "en": "Apa itu 'tunnel magnetoresistance' (TMR)?", "id": "Efek kuantum pada 'magnetic sensor'." },
  { "en": "Pemeriksaan 'giant magnetoresistance' (GMR) head?", "id": "Analisis kegagalan pada 'hard drive'." },
  { "en": "Apa itu 'electron backscatter diffraction' (EBSD)?", "id": "Analisis struktur kristal material." },
  { "en": "Kegunaan EBSD forensik?", "id": "Memetakan deformasi logam akibat insiden." },
  { "en": "Apa itu 'charge-coupled device' (CCD)?", "id": "Sensor gambar pada kamera lama." },
  { "en": "Forensik pada sensor CCD?", "id": "Analisis piksel rusak, 'blooming', 'smear'." },
  { "en": "Apa itu 'blooming' pada sensor?", "id": "Piksel terlalu jenuh 'bocor' ke tetangga." },
  { "en": "Apa itu 'Bayesian inference' forensik?", "id": "Memperbarui probabilitas hipotesis dengan bukti." },
  { "en": "Pemeriksaan pada 'capacitor bank'?", "id": "Analisis kegagalan kaskade dan sekring." },
  { "en": "Apa itu 'in-situ' analysis?", "id": "Analisis yang dilakukan di lokasi asli." },
  { "en": "Apa itu 'ex-situ' analysis?", "id": "Analisis yang dilakukan di laboratorium." },
  { "en": "Pemeriksaan 'gas-insulated switchgear' (GIS)?", "id": "Analisis kebocoran gas SF6." },
  { "en": "Bahaya gas SF6 (sulfur heksafluorida)?", "id": "Gas rumah kaca yang sangat kuat." },
  { "en": "Apa itu 'photoelectric effect'?", "id": "Emisi elektron saat cahaya mengenai material." },
  { "en": "Forensik pada 'photomultiplier tube' (PMT)?", "id": "Analisis degradasi katoda dan dinoda." },
  { "en": "Apa itu 'Cherenkov radiation'?", "id": "Radiasi dari partikel lebih cepat cahaya." },
  { "en": "Forensik 'Cherenkov radiation'?", "id": "Deteksi material nuklir ilegal." },
  { "en": "Apa itu 'lock-in amplifier'?", "id": "Alat ukur sinyal sangat lemah." },
  { "en": "Kegunaan 'lock-in amplifier' forensik?", "id": "Mendeteksi sinyal sisa yang tersembunyi." },
  { "en": "Apa itu 'Boltzmann constant'?", "id": "Konstanta fisika penghubung energi-suhu." },
  { "en": "Apa itu 'Planck's constant'?", "id": "Konstanta dasar dalam mekanika kuantum." },
  { "en": "Pemeriksaan 'tunnel diode'?", "id": "Analisis degradasi akibat efek kuantum." },
  { "en": "Apa itu 'negative resistance'?", "id": "Kondisi di mana arus turun saat tegangan naik." },
  { "en": "Pemeriksaan 'Gunn diode'?", "id": "Komponen untuk menghasilkan sinyal microwave." },
  { "en": "Apa itu 'IMPATT diode'?", "id": "Dioda penghasil daya frekuensi tinggi." },
  { "en": "Apa itu 'PIN diode'?", "id": "Dioda untuk saklar atau attenuator RF." },
  { "en": "Apa itu 'Schottky diode'?", "id": "Dioda dengan 'voltage drop' rendah." },
  { "en": "Apa itu 'point-contact transistor'?", "id": "Transistor pertama yang pernah dibuat." },
  { "en": "Apa itu 'Moore's law'?", "id": "Pengamatan jumlah transistor chip berlipat." },
  { "en": "Hubungan 'Moore's law' forensik?", "id": "Komponen semakin kecil, semakin sulit dianalisis." },
  { "en": "Apa itu 'exculpatory evidence'?", "id": "Bukti yang bisa membebaskan terdakwa." },
  { "en": "Apa itu 'inculpatory evidence'?", "id": "Bukti yang bisa memberatkan terdakwa." },
  { "en": "Pemeriksaan pada 'surface-mount device' (SMD)?", "id": "Analisis kegagalan sambungan solder." },
  { "en": "Apa itu 'tombstoning' pada SMD?", "id": "Komponen SMD berdiri saat disolder." },
  { "en": "Penyebab 'tombstoning'?", "id": "Pemanasan tidak merata pada papan PCB." },
  { "en": "Apa itu 'popcorning' pada chip?", "id": "Chip retak akibat uap air terperangkap." },
  { "en": "Pemeriksaan pada 'ballast' elektronik?", "id": "Analisis kegagalan sirkuit PFC dan inverter." },
  { "en": "Apa itu 'Power Factor Correction' (PFC)?", "id": "Sirkuit untuk perbaiki faktor daya." },
  { "en": "Apa itu 'quasistatic' process?", "id": "Proses yang terjadi sangat lambat." },
  { "en": "Pemeriksaan pada 'MEMS microphone'?", "id": "Analisis kontaminasi atau kerusakan diafragma." },
  { "en": "Pemeriksaan pada 'MEMS accelerometer'?", "id": "Analisis kerusakan akibat guncangan keras." },
  { "en": "Apa itu 'stiction' pada hard drive?", "id": "Head menempel pada piringan disk." },
  { "en": "Batas kepastian dalam sains forensik?", "id": "Probabilitas tinggi, bukan kepastian absolut." },
  { "en": "Peran komunikasi buruk dalam insiden?", "id": "Sebabkan salah persepsi dan tindakan salah." },
  { "en": "Analisis efek kaskade lintas-sistem?", "id": "Mencari hubungan antar kegagalan sistem." },
  { "en": "Forensik pada komputer tabung vakum?", "id": "Analisis kegagalan filamen dan kebocoran vakum." },
  { "en": "Pengaruh 'cognitive load' pada operator?", "id": "Meningkatkan risiko kesalahan saat stres." },
  { "en": "Fungsi 'laser vibrometer' forensik?", "id": "Deteksi getaran abnormal sebelum kegagalan." },
  { "en": "Apa itu 'voltage notching'?", "id": "Distorsi tegangan akibat konverter daya." },
  { "en": "Arti dari tidak adanya bukti?", "id": "Bisa sama pentingnya dengan adanya bukti." },
  { "en": "Fungsi 'data mining' forensik?", "id": "Menemukan pola cacat tersembunyi." },
  { "en": "Membedakan artefak insiden dan penyebab?", "id": "Kunci utama dalam analisis TKP." },
  { "en": "Analogi untuk 'gate oxide breakdown'?", "id": "Seperti bendungan retak menahan tegangan." },
  { "en": "Apa itu 'pre-event' data?", "id": "Data kondisi sebelum terjadinya insiden." },
  { "en": "Apa itu 'post-event' data?", "id": "Data kondisi setelah terjadinya insiden." },
  { "en": "Forensik pada 'vacuum interrupter'?", "id": "Analisis kebocoran vakum dan erosi kontak." },
  { "en": "Apa itu 'Swiss cheese model'?", "id": "Model kecelakaan akibat lubang pertahanan." },
  { "en": "Penerapan 'Swiss cheese model' forensik?", "id": "Analisis kegagalan berlapis dalam sistem." },
  { "en": "Apa itu 'normalization of deviance'?", "id": "Penyimpangan prosedur menjadi dianggap normal." },
  { "en": "Contoh 'normalization of deviance'?", "id": "Mengabaikan alarm karena sering berbunyi." },
  { "en": "Apa itu 'confirmation measurement'?", "id": "Pengukuran ulang untuk validasi data." },
  { "en": "Pemeriksaan 'corona motor'?", "id": "Motor elektrostatik bertegangan sangat tinggi." },
  { "en": "Apa itu 'Dunning-Kruger effect'?", "id": "Orang tidak kompeten, tidak sadar." },
  { "en": "Hubungan efek Dunning-Kruger forensik?", "id": "Kesalahan akibat kepercayaan diri berlebih." },
  { "en": "Apa itu 'Occam's razor'?", "id": "Penjelasan paling sederhana biasanya benar." },
  { "en": "Penerapan 'Occam's razor' forensik?", "id": "Pilih hipotesis dengan asumsi paling sedikit." },
  { "en": "Apa itu 'Hick's law'?", "id": "Waktu reaksi meningkat dengan jumlah pilihan." },
  { "en": "Hubungan 'Hick's law' forensik?", "id": "Analisis desain panel kontrol operator." },
  { "en": "Pemeriksaan 'Marx generator'?", "id": "Analisis kegagalan saklar dan kapasitor." },
  { "en": "Fungsi 'Marx generator'?", "id": "Menghasilkan pulsa tegangan sangat tinggi." },
  { "en": "Apa itu 'protocol analysis'?", "id": "Analisis komunikasi data antar perangkat." },
  { "en": "Forensik 'protocol analysis'?", "id": "Mencari perintah salah atau data rusak." },
  { "en": "Apa itu 'silent data corruption'?", "id": "Kerusakan data tanpa adanya peringatan." },
  { "en": "Apa itu 'galvanic skin response' (GSR)?", "id": "Perubahan konduktivitas kulit akibat emosi." },
  { "en": "Hubungan GSR dengan forensik?", "id": "Analisis stres pada interogasi (lie detector)." },
  { "en": "Pemeriksaan 'telluric current'?", "id": "Arus listrik alami di kerak bumi." },
  { "en": "Dampak 'telluric current'?", "id": "Dapat mengganggu pipa dan kabel panjang." },
  { "en": "Apa itu 'maser' (maser)?", "id": "Amplifikasi gelombang mikro, seperti laser." },
  { "en": "Forensik pada sistem maser?", "id": "Analisis kegagalan pendingin dan rongga resonansi." },
  { "en": "Apa itu 'cryocooler'?", "id": "Pendingin untuk suhu sangat rendah." },
  { "en": "Pemeriksaan 'Josephson junction'?", "id": "Komponen dasar pada sirkuit superkonduktor." },
  { "en": "Apa itu SQUID (Superconducting Quantum Interference Device)?", "id": "Sensor medan magnet paling sensitif." },
  { "en": "Kegunaan SQUID forensik?", "id": "Mendeteksi aktivitas magnetik sangat lemah." },
  { "en": "Pemeriksaan pada 'field emission display' (FED)?", "id": "Analisis kegagalan emitor dan fosfor." },
  { "en": "Apa itu 'Ohmic contact'?", "id": "Sambungan semikonduktor-logam yang ideal." },
  { "en": "Kegagalan 'Ohmic contact'?", "id": "Resistansi tinggi, sebabkan panas." },
  { "en": "Pemeriksaan pada 'varactor diode'?", "id": "Dioda yang kapasitansinya bisa diatur." },
  { "en": "Apa itu 'negative feedback'?", "id": "Proses menstabilkan output suatu sistem." },
  { "en": "Apa itu 'positive feedback'?", "id": "Proses memperkuat output, bisa tak stabil." },
  { "en": "Contoh 'positive feedback' fatal?", "id": "'Thermal runaway' pada komponen elektronik." },
  { "en": "Pemeriksaan 'synchro' dan 'resolver'?", "id": "Sensor posisi sudut elektromekanik." },
  { "en": "Apa itu 'common-mode failure'?", "id": "Satu penyebab gagalkan beberapa sistem." },
  { "en": "Contoh 'common-mode failure'?", "id": "Satu catu daya gagal, matikan semua." },
  { "en": "Apa itu 'safe operating area' (SOA)?", "id": "Batas kondisi operasi aman komponen." },
  { "en": "Pelanggaran SOA sebabkan apa?", "id": "Kegagalan komponen secara langsung." },
  { "en": "Apa itu 'load-line analysis'?", "id": "Metode grafis analisis sirkuit transistor." },
  { "en": "Pemeriksaan pada 'transmission line'?", "id": "Analisis impedansi, refleksi, dan kerugian." },
  { "en": "Apa itu 'voltage standing wave ratio' (VSWR)?", "id": "Ukuran ketidakcocokan impedansi." },
  { "en": "VSWR tinggi sebabkan apa?", "id": "Daya terpantul, merusak pemancar." },
  { "en": "Apa itu 'Smith chart'?", "id": "Diagram grafis untuk analisis transmisi." },
  { "en": "Apa itu 'eddy-current brake'?", "id": "Rem non-kontak menggunakan arus eddy." },
  { "en": "Kegagalan 'eddy-current brake'?", "id": "Overheating, kehilangan kekuatan magnet." },
  { "en": "Pemeriksaan 'homopolar motor'?", "id": "Motor DC paling sederhana." },
  { "en": "Apa itu 'Kelvin probe force microscope' (KPFM)?", "id": "Memetakan potensi listrik permukaan." },
  { "en": "Kegunaan KPFM forensik?", "id": "Analisis korosi atau cacat semikonduktor." },
  { "en": "Apa itu 'Coulomb blockade'?", "id": "Efek kuantum pada sirkuit nano." },
  { "en": "Apa itu 'single-electron transistor'?", "id": "Transistor yang beroperasi dengan satu elektron." },
  { "en": "Forensik pada 'molecular electronics'?", "id": "Analisis kegagalan pada level molekul." },
  { "en": "Pemeriksaan 'crossbar latch'?", "id": "Arsitektur memori non-volatil nano." },
  { "en": "Apa itu 'piezoresistive effect'?", "id": "Perubahan resistansi akibat tekanan mekanik." },
  { "en": "Contoh sensor 'piezoresistive'?", "id": "Sensor tekanan udara, akselerometer." },
  { "en": "Apa itu 'pyroelectric effect'?", "id": "Tegangan timbul dari perubahan suhu." },
  { "en": "Contoh sensor 'pyroelectric'?", "id": "Sensor gerak pasif inframerah (PIR)." },
  { "en": "Apa itu 'ferroelectric' material?", "id": "Material dengan polarisasi listrik permanen." },
  { "en": "Aplikasi 'ferroelectric'?", "id": "Memori (FeRAM), kapasitor, sensor." },
  { "en": "Pemeriksaan 'theremin'?", "id": "Alat musik elektronik non-kontak." },
  { "en": "Prinsip kerja 'theremin'?", "id": "Gangguan osilator oleh kapasitansi tubuh." },
  { "en": "Apa itu 'human body model' (HBM)?", "id": "Model standar untuk simulasi ESD." },
  { "en": "Apa itu 'charged device model' (CDM)?", "id": "Model ESD dari perangkat bermuatan." },
  { "en": "Pemeriksaan 'Nixie tube'?", "id": "Layar digital teknologi tabung gas." },
  { "en": "Kegagalan 'Nixie tube'?", "id": "Endapan katoda, kebocoran gas." },
  { "en": "Apa itu 'grid-leak' resistor?", "id": "Komponen pada sirkuit tabung vakum." },
  { "en": "Pemeriksaan 'cathode poisoning'?", "id": "Kontaminasi pada katoda tabung vakum." },
  { "en": "Apa itu 'g-force' survivability?", "id": "Kemampuan komponen bertahan dari percepatan." },
  { "en": "Pentingnya 'g-force' survivability?", "id": "Forensik pada avionik dan proyektil." },
  { "en": "Apa itu 'gyroscope' MEMS?", "id": "Sensor kecepatan sudut berbasis MEMS." },
  { "en": "Kegagalan 'gyroscope' MEMS?", "id": "Kerusakan akibat guncangan, 'drift'." },
  { "en": "Apa itu 'magnetometer'?", "id": "Sensor pengukur medan magnet." },
  { "en": "Pemeriksaan 'fluxgate magnetometer'?", "id": "Analisis inti, gulungan, dan sirkuit." },
  { "en": "Apa itu 'Pascal's law'?", "id": "Hukum tekanan pada fluida." },
  { "en": "Hubungan 'Pascal's law' forensik?", "id": "Analisis pada sistem hidrolik-elektrik." },
  { "en": "Apa itu 'Archimedes' principle'?", "id": "Prinsip gaya apung pada benda." },
  { "en": "Hubungan prinsip Archimedes forensik?", "id": "Analisis kegagalan pada peralatan bawah air." },
  { "en": "Apa itu 'Bernoulli's principle'?", "id": "Prinsip tekanan dan kecepatan fluida." },
  { "en": "Hubungan prinsip Bernoulli forensik?", "id": "Analisis kegagalan sensor aliran udara." },
  { "en": "Apa itu 'Heisenberg's uncertainty principle'?", "id": "Prinsip ketidakpastian dalam mekanika kuantum." }


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
