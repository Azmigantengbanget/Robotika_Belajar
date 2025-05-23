<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Pengetahuan Elektronika Lengkap</title>
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
            max-width: 700px; /* Sedikit lebih lebar untuk teks panjang */
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
            font-size: 1.4em; /* Sedikit disesuaikan */
            font-weight: bold;
            margin-bottom: 25px;
            color: #444;
            min-height: 70px;
            line-height: 1.4; /* Untuk keterbacaan pertanyaan panjang */
        }

        .btn-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr)); /* Lebih responsif */
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
            font-size: 0.95em; /* Sedikit disesuaikan */
            transition: background-color 0.2s ease, box-shadow 0.2s ease;
            word-wrap: break-word;
            min-height: 65px;
            display: flex;
            align-items: center;
            justify-content: center;
            outline: none;
            font-weight: bold;
            text-align: center;
            line-height: 1.3;
        }

        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) { background-color: #007bff; }
        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):hover {
             background-color: #0056b3;
        }
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus:hover {
            background-color: #0056b3;
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
        .btn:disabled:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) {
            background-color: #6c757d !important;
            color: #ccc !important;
        }

        .controls {
            display: flex;
            justify-content: center;
            gap: 10px;
            flex-wrap: wrap; /* Agar tombol muat di layar kecil */
        }

        #skip-navigation-controls {
            justify-content: space-between;
            margin-top: 40px;
            margin-bottom: 10px;
        }

        .skip-btn, .btn-prev-q {
            padding: 8px 12px;
            font-size: 0.9em;
            min-width: 70px; /* Sedikit lebih kecil agar muat */
        }
        .skip-btn { background-color: #28a745; color: white; }
        .skip-btn:hover:not([disabled]) { background-color: #218838; }
        .skip-btn:disabled { background-color: #a3d8b0 !important; color: #e9f5ec !important; }

        .btn-prev-q { background-color: #5F9EA0; color: white; }
        .btn-prev-q:hover:not([disabled]) { background-color: #4682B4; }
        .btn-prev-q:disabled { background-color: #B0C4DE !important; color: #666666 !important; }

        .hide { display: none !important; }
    </style>
</head>
<body>
    <div class="quiz-container">
        <h1>Kuis Pengetahuan Elektronika</h1>
        <p id="completion-message" class="hide">Selamat Kuis Sudah Selesai 🎉</p>
        <div id="initial-controls" class="controls">
            <button id="start-btn" class="btn">Mulai</button>
            <button id="continue-btn" class="btn hide">Lanjutkan</button>
        </div>
        <div id="question-counter" class="question-counter-text hide">0/0</div>
        <div id="question-container" class="hide">
            <div id="question">Pertanyaan Elektronika</div>
            <div id="answer-buttons" class="btn-grid">
            </div>
            <div id="skip-navigation-controls" class="controls hide">
                <button id="prev-50-btn" class="btn skip-btn">&laquo; 50</button>
                <button id="prev-question-btn" class="btn btn-prev-q">&lt;</button>
                <button id="next-50-btn" class="btn skip-btn">50 &raquo;</button>
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
        const prevQuestionButton = document.getElementById('prev-question-btn');
        const next50Button = document.getElementById('next-50-btn');
        const JUMP_AMOUNT = 50;

        let orderedQuestions, currentQuestionIndex;
        let score = 0;
        let questionTimeout;

        // ==========================================================================================
        // DAFTAR SOAL ELEKTRONIKA
        // ==========================================================================================
        // Harap tambahkan soal di sini hingga mencapai 1000.
        // Saat ini berisi 209 soal awal + sekitar 170 soal baru = ~379 soal.
        // Anda perlu menambahkan sekitar 621 soal lagi.
        const rawVocabularyList = [
            // SOAL-SOAL LAMA (1-209)


  { "en": "Apa prinsip dasar kerja algoritma YOLO?", "id": "YOLO mendeteksi objek dengan sekali lihat pada seluruh gambar." },
  { "en": "Bagaimana YOLO memproses gambar untuk deteksi objek?", "id": "YOLO membagi gambar menjadi grid dan memprediksi bounding box serta kelas per grid." },
  { "en": "Apa yang dimaksud dengan pembagian grid dalam YOLO?", "id": "Gambar dibagi menjadi S x S sel grid imajiner." },
  { "en": "Apa fungsi setiap sel grid dalam YOLO?", "id": "Setiap sel grid bertanggung jawab mendeteksi objek yang pusatnya ada di sel tersebut." },
  { "en": "Bagaimana pembagian grid 13x13 mempengaruhi jumlah prediksi?", "id": "Setiap dari 169 sel grid akan membuat prediksi." },
  { "en": "Pada gambar 416x416 dengan grid 13x13, berapa ukuran satu sel grid?", "id": "Ukuran satu sel grid adalah 32x32 piksel." },
  { "en": "Apa yang diprediksi oleh setiap sel grid selain lokasi objek?", "id": "Setiap sel grid juga memprediksi probabilitas kelas objek." },
  { "en": "Apa itu *bounding box* dalam konteks YOLO?", "id": "*Bounding box* adalah kotak yang menandai lokasi objek terdeteksi." },
  { "en": "Berapa banyak *bounding box* yang biasanya diprediksi oleh satu sel grid?", "id": "Satu sel grid biasanya memprediksi beberapa *bounding box* (B)." },
  { "en": "Apa informasi yang terkandung dalam prediksi *bounding box*?", "id": "Informasi meliputi koordinat (x,y), lebar (w), tinggi (h), dan skor kepercayaan." },
  { "en": "Bagaimana kecepatan YOLO dibandingkan dengan Faster R-CNN?", "id": "YOLO umumnya jauh lebih cepat daripada Faster R-CNN." },
  { "en": "Bagaimana akurasi YOLO dibandingkan dengan SSD?", "id": "Akurasi YOLO dan SSD kompetitif, tergantung versi dan dataset." },
  { "en": "Algoritma mana yang lebih cocok untuk robot bergerak yang butuh kecepatan tinggi?", "id": "YOLO seringkali lebih cocok untuk robot bergerak karena kecepatannya." },
  { "en": "Apa keunggulan utama SSD dibandingkan YOLO dalam beberapa kasus?", "id": "SSD terkadang lebih baik dalam mendeteksi objek kecil." },
  { "en": "Mengapa Faster R-CNN cenderung lebih lambat?", "id": "Faster R-CNN menggunakan dua tahap proses deteksi." },
  { "en": "Dalam hal apa Faster R-CNN bisa lebih unggul dari YOLO?", "id": "Faster R-CNN seringkali menawarkan akurasi deteksi yang sedikit lebih tinggi." },
  { "en": "Apa itu deteksi *real-time*?", "id": "Deteksi *real-time* berarti memproses frame video secepat frame tersebut masuk." },
  { "en": "Mengapa kecepatan penting untuk deteksi objek pada robot bergerak?", "id": "Kecepatan penting agar robot dapat merespons lingkungannya dengan cepat." },
  { "en": "Algoritma mana yang menggunakan pendekatan *single-shot detector*?", "id": "YOLO dan SSD adalah contoh *single-shot detector*." },
  { "en": "Apa yang dimaksud dengan *region proposal network* (RPN) pada Faster R-CNN?", "id": "RPN adalah jaringan yang mengusulkan kandidat wilayah objek." },
  { "en": "Jika YOLO membagi gambar menjadi S x S grid, apa arti S?", "id": "S adalah jumlah pembagian grid pada satu dimensi gambar." },
  { "en": "Apa arti B dalam prediksi tiap grid YOLO?", "id": "B adalah jumlah *bounding box* yang diprediksi oleh tiap sel grid." },
  { "en": "Apa arti C dalam prediksi YOLO?", "id": "C adalah jumlah kelas objek yang dapat dideteksi." },
  { "en": "Bagaimana rumus matematis total output size YOLO?", "id": "Total output size adalah S x S x (B x 5 + C)." },
  { "en": "Mengapa ada angka 5 dalam komponen B x 5?", "id": "Angka 5 merepresentasikan 4 koordinat *bounding box* ditambah 1 skor kepercayaan." },
  { "en": "Untuk S=7, B=2, C=20, berapa total output tensor?", "id": "Output tensor adalah 7 x 7 x (2 x 5 + 20) = 7 x 7 x 30." },
  { "en": "Apa representasi dari S x S dalam output?", "id": "S x S merepresentasikan sel-sel grid pada gambar." },
  { "en": "Apa yang direpresentasikan oleh bagian B x 5 dari output?", "id": "Bagian B x 5 merepresentasikan prediksi *bounding box* dan skor kepercayaannya." },
  { "en": "Apa yang direpresentasikan oleh bagian C dari output per sel?", "id": "Bagian C merepresentasikan probabilitas kelas untuk objek dalam sel tersebut." },
  { "en": "Apakah output size ini berlaku untuk semua versi YOLO?", "id": "Struktur dasar outputnya mirip, namun detail bisa berbeda antar versi." },
  { "en": "Apa itu *True Positive* (TP) dalam deteksi objek?", "id": "TP adalah deteksi yang benar mengidentifikasi objek." },
  { "en": "Apa itu *False Positive* (FP) dalam deteksi objek?", "id": "FP adalah deteksi yang salah mengidentifikasi non-objek sebagai objek." },
  { "en": "Apa itu *False Negative* (FN) dalam deteksi objek?", "id": "FN adalah kegagalan mendeteksi objek yang sebenarnya ada." },
  { "en": "Bagaimana cara menghitung *Precision*?", "id": "*Precision* dihitung sebagai TP / (TP + FP)." },
  { "en": "Bagaimana cara menghitung *Recall* (True Positive Rate)?", "id": "*Recall* dihitung sebagai TP / (TP + FN)." },
  { "en": "Jika dari 100 deteksi, 80 benar dan 20 salah, berapa *Precision*?", "id": "*Precision* adalah 80 / (80 + 20) = 0.80." },
  { "en": "Jika TPR adalah 0.85 dan ada 80 deteksi benar (TP), berapa jumlah objek aktual positif (TP+FN)?", "id": "Jumlah objek aktual positif adalah 80 / 0.85 ≈ 94." },
  { "en": "Apa arti nilai *Precision* yang tinggi dalam robotika penyelamatan?", "id": "*Precision* tinggi berarti sedikit alarm palsu dari robot." },
  { "en": "Apa arti nilai *Recall* yang tinggi dalam robotika penyelamatan?", "id": "*Recall* tinggi berarti robot menemukan sebagian besar korban yang ada." },
  { "en": "Mengapa kedua nilai ini (Precision dan Recall) penting dalam robotika penyelamatan?", "id": "Keduanya penting untuk memastikan efektivitas dan keandalan deteksi korban." },
  { "en": "Bagaimana YOLO dapat mengenali kendaraan melawan arus?", "id": "YOLO mendeteksi kendaraan dan arah geraknya dapat dianalisis antar frame." },
  { "en": "Bagaimana YOLO bisa mendeteksi kendaraan berhenti di area terlarang?", "id": "YOLO mendeteksi kendaraan dan durasi diamnya di zona terlarang dipantau." },
  { "en": "Apa output YOLO yang berguna untuk mendeteksi pelanggaran lalu lintas?", "id": "Output berupa kelas objek (kendaraan) dan lokasinya (*bounding box*)." },
  { "en": "Dapatkah YOLO membedakan jenis kendaraan (misalnya, mobil dan motor)?", "id": "Ya, jika dilatih dengan dataset yang mencakup kelas-kelas tersebut." },
  { "en": "Bagaimana YOLO membantu dalam analisis kepadatan lalu lintas?", "id": "YOLO dapat menghitung jumlah kendaraan yang terdeteksi di suatu area." },
  { "en": "Apakah YOLO dapat digunakan untuk membaca plat nomor secara langsung?", "id": "YOLO dapat mendeteksi area plat nomor, tetapi OCR diperlukan untuk membaca teksnya." },
  { "en": "Bagaimana informasi waktu ditambahkan pada deteksi YOLO untuk analisis lalu lintas?", "id": "Setiap deteksi diberi stempel waktu untuk melacak perilaku objek." },
  { "en": "Apa tantangan penggunaan YOLO untuk pengawasan lalu lintas di malam hari?", "id": "Kondisi pencahayaan rendah dapat mengurangi akurasi deteksi." },
  { "en": "Bisakah YOLO mendeteksi pejalan kaki yang menyeberang sembarangan?", "id": "Ya, jika model dilatih untuk mendeteksi kelas \"pejalan kaki\"." },
  { "en": "Bagaimana YOLO membantu dalam sistem tilang elektronik (ETLE)?", "id": "YOLO dapat mengidentifikasi kendaraan yang melakukan pelanggaran secara otomatis." },
  { "en": "Sebutkan satu tantangan utama menerapkan YOLO dalam robotika nyata.", "id": "Keterbatasan sumber daya komputasi pada robot adalah tantangan utama." },
  { "en": "Sebutkan tantangan kedua dalam implementasi YOLO di robotika.", "id": "Variasi kondisi lingkungan (cahaya, cuaca) dapat mempengaruhi performa." },
  { "en": "Sebutkan tantangan ketiga penggunaan YOLO pada robot.", "id": "Kebutuhan akan dataset yang representatif dan berkualitas tinggi." },
  { "en": "Bagaimana perhitungan jarak Euclidean antara *bounding box* digunakan untuk *tracking*?", "id": "Jarak Euclidean membantu mencocokkan deteksi objek yang sama antar frame." },
  { "en": "Informasi apa dari *bounding box* yang digunakan untuk menghitung jarak Euclidean?", "id": "Koordinat pusat *bounding box* (xc, yc) sering digunakan." },
  { "en": "Mengapa *object tracking* penting dalam robotika?", "id": "*Tracking* memungkinkan robot memahami pergerakan objek dan interaksi dinamis." },
  { "en": "Apa masalah yang bisa timbul jika *tracking* objek gagal?", "id": "Robot bisa kehilangan target atau salah menginterpretasi skenario." },
  { "en": "Bagaimana oklusi (objek terhalang) mempengaruhi YOLO dan *tracking*?", "id": "Oklusi dapat menyebabkan YOLO gagal mendeteksi atau *tracker* kehilangan objek." },
  { "en": "Apakah kecepatan gerakan robot mempengaruhi kinerja YOLO?", "id": "Gerakan cepat bisa menyebabkan *motion blur* yang menyulitkan deteksi." },
  { "en": "Bagaimana kalibrasi kamera mempengaruhi akurasi *tracking* berbasis YOLO?", "id": "Kalibrasi yang buruk dapat menyebabkan kesalahan estimasi posisi objek." },
  { "en": "Apa kepanjangan dari IoU?", "id": "IoU adalah singkatan dari *Intersection over Union*." },
  { "en": "Apa fungsi utama IoU dalam deteksi objek?", "id": "IoU mengukur seberapa tumpang tindih prediksi *bounding box* dengan *ground truth*." },
  { "en": "Bagaimana cara menghitung area irisan (Intersection) dua *bounding box*?", "id": "Area irisan adalah luas daerah di mana kedua kotak saling tumpang tindih." },
  { "en": "Bagaimana cara menghitung area gabungan (Union) dua *bounding box*?", "id": "Area gabungan adalah luas total yang dicakup oleh kedua kotak dikurangi irisannya." },
  { "en": "Untuk Box A (x=30, y=40, w=100, h=100) dan Box B (x=50, y=60, w=100, h=100), berapa luas area Box A?", "id": "Luas Box A adalah 100 x 100 = 10000 unit persegi." },
  { "en": "Berapa area irisan (Intersection) antara Box A (x=30,y=40,w=100,h=100) dan Box B (x=50,y=60,w=100,h=100)?", "id": "Area irisan adalah 80 x 80 = 6400 unit persegi." },
  { "en": "Berapa area gabungan (Union) antara Box A dan Box B tersebut?", "id": "Area gabungan adalah 10000 + 10000 - 6400 = 13600 unit persegi." },
  { "en": "Berapa nilai IoU antara Box A dan Box B tersebut?", "id": "Nilai IoU adalah 6400 / 13600 ≈ 0.47." },
  { "en": "Apakah nilai IoU > 0.5 untuk Box A dan Box B tersebut?", "id": "Tidak, nilai IoU-nya adalah sekitar 0.47, jadi kurang dari 0.5." },
  { "en": "Apa implikasi jika IoU < 0.5 terhadap hasil deteksi YOLO?", "id": "Deteksi tersebut mungkin dianggap sebagai deteksi yang kurang akurat atau salah." },
  { "en": "Bagaimana dataset mempengaruhi performa YOLO?", "id": "Kualitas dan keragaman dataset sangat menentukan akurasi YOLO." },
  { "en": "Apa tujuan utama dari proses *training* model YOLO?", "id": "*Training* bertujuan agar model belajar mengenali pola objek dari data." },
  { "en": "Mengapa diversitas dalam dataset penting?", "id": "Diversitas membantu model melakukan generalisasi pada data yang belum pernah dilihat." },
  { "en": "Apa itu *imbalanced dataset*?", "id": "*Imbalanced dataset* adalah dataset di mana jumlah sampel antar kelas tidak seimbang." },
  { "en": "Apa dampak dari *imbalanced dataset* pada training YOLO?", "id": "Model cenderung bias terhadap kelas dengan sampel lebih banyak." },
  { "en": "Apa itu *data augmentation*?", "id": "*Data augmentation* adalah teknik memperbanyak data latih dengan modifikasi data yang ada." },
  { "en": "Mengapa anotasi dataset yang akurat itu krusial?", "id": "Anotasi yang salah akan mengajarkan model hal yang salah." },
  { "en": "Apa potensi masalah jika dataset kurang bervariasi?", "id": "Model mungkin tidak bekerja baik di lingkungan yang berbeda dari data latih." },
  { "en": "Apa itu *overfitting* dalam training YOLO?", "id": "*Overfitting* terjadi ketika model terlalu hafal data latih dan buruk pada data baru." },
  { "en": "Bagaimana cara mengatasi *overfitting*?", "id": "Menggunakan lebih banyak data, augmentasi, atau regularisasi bisa membantu." },
  { "en": "Jika akurasi model deteksi objek 82%, berapa persentase kesalahan prediksi?", "id": "Persentase kesalahan prediksi adalah 100% - 82% = 18%." },
  { "en": "Jika robot mendeteksi 200 objek dengan akurasi 82%, berapa objek yang diprediksi benar?", "id": "Jumlah objek yang diprediksi benar adalah 0.82 x 200 = 164 objek." },
  { "en": "Berapa objek yang diprediksi salah dari 200 objek dengan akurasi 82%?", "id": "Jumlah objek yang diprediksi salah adalah 200 - 164 = 36 objek." },
  { "en": "Apa konsekuensi *false positive* (deteksi salah) pada robot penjelajah?", "id": "Robot bisa salah mengidentifikasi rintangan atau target yang tidak ada." },
  { "en": "Apa konsekuensi *false negative* (kegagalan deteksi) pada robot penyelamat?", "id": "Robot bisa gagal mendeteksi korban yang sebenarnya ada." },
  { "en": "Bagaimana prediksi yang salah mempengaruhi navigasi robot?", "id": "Prediksi salah dapat menyebabkan robot mengambil jalur yang salah atau berbahaya." },
  { "en": "Mengapa akurasi tinggi sangat penting dalam misi eksplorasi robot?", "id": "Akurasi tinggi memastikan data yang dikumpulkan valid dan keputusan tepat." },
  { "en": "Apakah akurasi 82% cukup untuk semua aplikasi robotika?", "id": "Tidak, kebutuhan akurasi bervariasi tergantung tingkat kritis aplikasi." },
  { "en": "Apa risiko jika robot salah mengklasifikasikan objek berbahaya?", "id": "Dapat membahayakan robot itu sendiri atau lingkungan sekitarnya." },
  { "en": "Bagaimana sistem robotika dapat menangani ketidakpastian dari prediksi YOLO?", "id": "Sistem dapat menggunakan mekanisme validasi atau fusi sensor." },
  { "en": "Bagaimana resolusi input mempengaruhi akurasi deteksi YOLO?", "id": "Resolusi lebih tinggi umumnya meningkatkan akurasi, terutama untuk objek kecil." },
  { "en": "Bagaimana resolusi input mempengaruhi waktu proses (inferensi) YOLO?", "id": "Resolusi lebih tinggi secara signifikan meningkatkan waktu proses." },
  { "en": "Apa *trade-off* utama terkait resolusi input pada YOLO?", "id": "*Trade-off* terjadi antara akurasi deteksi dan kecepatan pemrosesan." },
  { "en": "Jika resolusi input dilipatgandakan (misalnya, dari 320x320 ke 640x640), bagaimana perkiraan kenaikan waktu inferensi?", "id": "Waktu inferensi bisa meningkat sekitar empat kali lipat atau lebih." },
  { "en": "Mengapa resolusi lebih tinggi membantu mendeteksi objek kecil?", "id": "Objek kecil akan memiliki lebih banyak piksel representasi pada resolusi tinggi." },
  { "en": "Apa yang dimaksud dengan waktu inferensi?", "id": "Waktu inferensi adalah waktu yang dibutuhkan model untuk membuat prediksi pada satu input." },
  { "en": "Mana yang lebih diprioritaskan untuk robot *real-time*: akurasi maksimal atau waktu inferensi cepat?", "id": "Waktu inferensi cepat sering lebih prioritas untuk operasi *real-time*." },
  { "en": "Apakah penggunaan resolusi input yang sangat rendah selalu buruk?", "id": "Tidak, jika kecepatan sangat krusial dan objek target cukup besar." },
  { "en": "Bagaimana cara memilih resolusi input yang optimal?", "id": "Pemilihan resolusi optimal tergantung pada kebutuhan spesifik aplikasi dan kapabilitas perangkat keras." },
  { "en": "Apakah semua objek mendapat manfaat yang sama dari peningkatan resolusi?", "id": "Objek yang lebih kecil dan detail cenderung mendapat manfaat lebih besar." },
  { "en": "Bagaimana cara menghitung luas area objek dari *bounding box* (w, h)?", "id": "Luas area dihitung dengan mengalikan lebar (w) dengan tinggi (h)." },
  { "en": "Jika *bounding box* memiliki w=100 dan h=120, berapa luas areanya?", "id": "Luas areanya adalah 100 x 120 = 12000 unit persegi." },
  { "en": "Apa itu rasio aspek (*aspect ratio*) dari sebuah *bounding box*?", "id": "Rasio aspek adalah perbandingan antara lebar dan tinggi *bounding box*." },
  { "en": "Bagaimana cara menghitung rasio aspek?", "id": "Rasio aspek dihitung sebagai lebar dibagi tinggi (w/h) atau sebaliknya." },
  { "en": "Mengapa rasio aspek penting dalam klasifikasi objek secara akurat?", "id": "Rasio aspek dapat membantu membedakan objek dengan bentuk khas (misalnya, manusia vs mobil)." },
  { "en": "Dapatkah rasio aspek membantu YOLO membedakan antara tiang dan manusia?", "id": "Ya, karena umumnya rasio aspek keduanya berbeda signifikan." },
  { "en": "Apakah YOLO secara eksplisit menggunakan rasio aspek sebagai fitur input?", "id": "YOLO mempelajari bentuk, termasuk rasio aspek, secara implisit dari data." },
  { "en": "Bagaimana *anchor boxes* dalam YOLO terkait dengan rasio aspek?", "id": "*Anchor boxes* memiliki berbagai rasio aspek awal untuk membantu deteksi." },
  { "en": "Apa yang mungkin diindikasikan oleh rasio aspek yang sangat tidak biasa untuk objek tertentu?", "id": "Bisa jadi itu adalah deteksi yang salah atau objek dalam pose tidak biasa." },
  { "en": "Apakah rasio aspek sensitif terhadap skala objek?", "id": "Tidak, rasio aspek adalah properti bentuk yang relatif invarian terhadap skala." },
  { "en": "Bagaimana robot yang dilengkapi YOLO dapat membedakan objek statis dan dinamis?", "id": "Dengan melacak posisi objek yang terdeteksi YOLO antar beberapa frame video." },
  { "en": "Informasi apa yang krusial untuk membedakan objek statis dari dinamis?", "id": "Perubahan posisi objek dari waktu ke waktu adalah informasi krusial." },
  { "en": "Apakah YOLO sendiri langsung mengklasifikasikan objek sebagai statis atau dinamis?", "id": "Tidak, YOLO hanya mendeteksi objek; klasifikasi statis/dinamis memerlukan pemrosesan tambahan." },
  { "en": "Metode apa yang bisa digabungkan dengan YOLO untuk identifikasi objek dinamis?", "id": "Metode seperti *optical flow* atau *background subtraction* dapat digunakan." },
  { "en": "Bagaimana *object tracking* membantu dalam membedakan objek dinamis?", "id": "*Tracking* membangun histori pergerakan objek yang terdeteksi." },
  { "en": "Apa peran analisis gerakan dalam konteks ini?", "id": "Analisis gerakan mengidentifikasi objek yang berpindah atau berubah posisi." },
  { "en": "Mengapa penting bagi robot untuk membedakan objek statis dan dinamis?", "id": "Penting untuk navigasi aman dan perencanaan interaksi yang tepat." },
  { "en": "Bagaimana robot merespons objek statis?", "id": "Robot mungkin menganggap objek statis sebagai rintangan tetap." },
  { "en": "Bagaimana robot merespons objek dinamis?", "id": "Robot mungkin perlu memprediksi jalur objek dinamis untuk menghindarinya." },
  { "en": "Bisakah histori deteksi YOLO digunakan untuk klasifikasi ini?", "id": "Ya, histori posisi deteksi objek dari YOLO sangat berguna." },
  { "en": "Apa salah satu penyebab umum YOLO salah mengklasifikasikan objek?", "id": "Kurangnya variasi atau kualitas data latih untuk kelas tertentu." },
  { "en": "Bagaimana kondisi pencahayaan yang buruk dapat menyebabkan salah klasifikasi?", "id": "Pencahayaan buruk dapat mengubah penampilan objek sehingga sulit dikenali." },
  { "en": "Jika YOLO mengira tiang sebagai manusia, apa kemungkinan penyebab dari sisi data?", "id": "Mungkin dataset kurang memiliki contoh tiang atau contoh manusia yang mirip tiang." },
  { "en": "Apa solusi jika YOLO sering salah mengklasifikasikan objek tertentu?", "id": "Melatih ulang model dengan data tambahan yang lebih representatif dan beragam." },
  { "en": "Bagaimana *fine-tuning* model dapat membantu mengurangi salah klasifikasi?", "id": "*Fine-tuning* pada dataset spesifik dapat meningkatkan akurasi untuk kasus tersebut." },
  { "en": "Apakah oklusi objek dapat menyebabkan salah klasifikasi?", "id": "Ya, jika bagian penting objek terhalang, model bisa salah menebak." },
  { "en": "Bagaimana kemiripan antar kelas objek mempengaruhi tingkat salah klasifikasi?", "id": "Semakin mirip dua kelas, semakin besar kemungkinan salah klasifikasi antar keduanya." },
  { "en": "Dapatkah penyesuaian ambang batas kepercayaan (*confidence threshold*) membantu?", "id": "Ya, menaikkan ambang batas dapat mengurangi *false positive* tetapi bisa menaikkan *false negative*." },
  { "en": "Bagaimana teknik *post-processing* dapat memperbaiki hasil klasifikasi?", "id": "*Post-processing* dapat menerapkan aturan logika atau filter untuk memperbaiki deteksi." },
  { "en": "Apakah kualitas kamera berpengaruh pada akurasi klasifikasi YOLO?", "id": "Ya, kualitas gambar yang lebih baik dari kamera dapat meningkatkan akurasi." },
  { "en": "Bagaimana YOLO dapat membantu robot pencari korban bencana?", "id": "YOLO dapat mendeteksi manusia atau tanda-tanda keberadaan manusia di reruntuhan." },
  { "en": "Objek apa saja yang bisa dideteksi YOLO untuk membantu navigasi di area bencana?", "id": "YOLO bisa mendeteksi rintangan, jalan yang bisa dilalui, atau struktur bangunan." },
  { "en": "Bagaimana YOLO berkontribusi pada kesadaran situasional (*situational awareness*) robot?", "id": "YOLO memberikan informasi visual tentang objek-objek penting di sekitar robot." },
  { "en": "Dalam skenario bencana, kelas objek apa yang krusial untuk dideteksi YOLO?", "id": "Kelas seperti \"manusia\", \"puing-puing besar\", atau \"tanda bahaya\" sangat krusial." },
  { "en": "Bagaimana output YOLO (lokasi objek) digunakan untuk perencanaan jalur robot?", "id": "Lokasi objek terdeteksi digunakan untuk membuat peta rintangan dan jalur aman." },
  { "en": "Bisakah YOLO mendeteksi gerakan kecil yang mungkin mengindikasikan korban hidup?", "id": "YOLO mendeteksi objek; analisis gerakan antar frame diperlukan untuk deteksi gerakan." },
  { "en": "Apa tantangan penggunaan YOLO di lingkungan bencana yang dinamis dan tidak terstruktur?", "id": "Debu, asap, pencahayaan buruk, dan objek tak terduga menjadi tantangan besar." },
  { "en": "Bagaimana YOLO dapat membantu memetakan area yang belum terjamah?", "id": "Dengan mendeteksi dan melokalisasi objek-objek kunci sebagai referensi spasial." },
  { "en": "Apakah YOLO bisa mendeteksi sumber panas jika digabung dengan kamera thermal?", "id": "YOLO memproses data visual; data thermal memerlukan model atau pendekatan berbeda." },
  { "en": "Bagaimana robot penyelamat memastikan deteksi manusianya akurat?", "id": "Dengan menggunakan model YOLO yang sangat andal dan mungkin sensor konfirmasi lain." },
  { "en": "Bagaimana YOLOv3 digunakan untuk navigasi *mobile robot* di lingkungan dinamis?", "id": "YOLOv3 mendeteksi objek di sekitar, yang informasinya digunakan untuk perencanaan jalur dan penghindaran." },
  { "en": "Peran apa yang dimainkan *bounding box* dari YOLOv3 dalam strategi penghindaran rintangan?", "id": "*Bounding box* menandai area yang ditempati rintangan sehingga robot dapat menghindarinya." },
  { "en": "Apakah YOLOv3 secara langsung menyediakan informasi jarak ke objek?", "id": "Tidak, YOLOv3 menyediakan lokasi 2D; jarak memerlukan sensor tambahan atau estimasi." },
  { "en": "Bagaimana robot menggunakan informasi kelas objek dari YOLOv3 untuk navigasi?", "id": "Kelas objek membantu robot memutuskan bagaimana merespons (misalnya, manusia vs. tembok)." },
  { "en": "Apa yang dimaksud lingkungan \"dinamis\" bagi *mobile robot*?", "id": "Lingkungan dinamis adalah lingkungan di mana terdapat objek bergerak." },
  { "en": "Bagaimana YOLOv3 membantu robot membuat peta lokal lingkungannya?", "id": "Dengan mendeteksi dan melokalisasi rintangan statis secara terus-menerus." },
  { "en": "Strategi umum apa yang digunakan robot untuk menghindari rintangan yang dideteksi YOLOv3?", "id": "Robot akan merencanakan jalur alternatif di sekitar *bounding box* rintangan." },
  { "en": "Apakah YOLOv3 bisa memprediksi pergerakan rintangan dinamis?", "id": "YOLOv3 sendiri tidak, tetapi outputnya dapat diumpankan ke modul *tracking* dan prediksi." },
  { "en": "Mengapa deteksi objek yang cepat penting untuk navigasi di lingkungan dinamis?", "id": "Deteksi cepat memungkinkan robot bereaksi tepat waktu terhadap perubahan." },
  { "en": "Bisakah YOLOv3 membantu robot mengikuti objek tertentu?", "id": "Ya, dengan terus menerus mendeteksi dan melacak *bounding box* objek target." },
  { "en": "Bagaimana data kelas objek dari YOLOv3 digunakan oleh robot manipulator?", "id": "Kelas objek menentukan bagaimana manipulator harus berinteraksi dengan objek tersebut." },
  { "en": "Bagaimana koordinat objek dari YOLOv3 diterjemahkan menjadi perintah gerak manipulator?", "id": "Koordinat gambar diubah menjadi koordinat dunia, lalu ke perintah sendi lengan." },
  { "en": "Transformasi koordinat apa yang diperlukan dari output YOLOv3 ke ruang kerja manipulator?", "id": "Transformasi dari koordinat piksel kamera ke koordinat 3D dunia robot." },
  { "en": "Apa peran kalibrasi *hand-eye* dalam menggunakan output YOLOv3 untuk manipulator?", "id": "Kalibrasi *hand-eye* menghubungkan sistem koordinat kamera dengan sistem koordinat lengan robot." },
  { "en": "Bagaimana informasi kedalaman (depth) melengkapi output YOLOv3 untuk tugas manipulasi?", "id": "Informasi kedalaman memberikan koordinat Z untuk lokasi 3D objek yang presisi." },
  { "en": "Bagaimana robot manipulator bisa menargetkan titik tengah objek yang dideteksi YOLOv3?", "id": "Dengan menghitung pusat *bounding box* dan mentransformasikannya ke koordinat dunia." },
  { "en": "Apa peran kinematika robot dalam menerjemahkan deteksi YOLOv3?", "id": "Kinematika (maju dan balik) menghitung posisi sendi untuk mencapai target." },
  { "en": "Bisakah YOLOv3 digunakan untuk memandu tugas perakitan halus oleh manipulator?", "id": "Ya, jika dikombinasikan dengan presisi tinggi dan kontrol yang sesuai." },
  { "en": "Apakah output YOLOv3 langsung mengontrol aktuator motor manipulator?", "id": "Tidak, output YOLOv3 adalah input untuk sistem kontrol gerak manipulator." },
  { "en": "Bagaimana manipulator menyesuaikan *gripper* berdasarkan ukuran *bounding box* dari YOLOv3?", "id": "Ukuran *bounding box* dapat memberi estimasi ukuran objek untuk penyesuaian *gripper*." },
  { "en": "Apa tantangan utama YOLOv3 saat objek sebagian tersembunyi (oklusi)?", "id": "Oklusi dapat menyebabkan kegagalan deteksi atau estimasi *bounding box* yang tidak akurat." },
  { "en": "Bagaimana objek yang tumpang tindih menyulitkan deteksi YOLOv3 untuk manipulasi?", "id": "Sulit membedakan batas antar objek dan menentukan objek mana yang akan diambil." },
  { "en": "Bisakah YOLOv3 menentukan titik genggam (*grasp point*) ideal pada objek yang teroklusi?", "id": "YOLOv3 sendiri tidak; ini memerlukan analisis bentuk atau model tambahan." },
  { "en": "Mengapa presisi lokalisasi sangat penting dalam tugas manipulasi robot?", "id": "Presisi tinggi dibutuhkan agar manipulator dapat mengambil atau berinteraksi dengan objek secara benar." },
  { "en": "Bagaimana pantulan cahaya atau objek transparan mempengaruhi YOLOv3?", "id": "Pantulan dan transparansi dapat mengganggu fitur visual yang dideteksi YOLOv3." },
  { "en": "Apakah manipulator itu sendiri dapat menyebabkan oklusi objek dari pandangan kamera?", "id": "Ya, lengan atau *gripper* robot dapat menghalangi pandangan kamera ke objek." },
  { "en": "Bagaimana variasi skala objek dalam jarak dekat mempengaruhi YOLOv3 pada manipulator?", "id": "Model harus robust terhadap perubahan skala objek yang signifikan." },
  { "en": "Dataset seperti apa yang dibutuhkan untuk meningkatkan performa YOLOv3 pada kasus oklusi?", "id": "Dataset perlu banyak contoh objek teroklusi dalam berbagai skenario." },
  { "en": "Apakah YOLOv3 sensitif terhadap orientasi objek saat manipulasi?", "id": "Ya, perubahan orientasi bisa menyulitkan jika tidak terwakili baik di data latih." },
  { "en": "Bagaimana cara mengatasi masalah tumpang tindih objek dalam manipulasi?", "id": "Menggunakan segmentasi instans atau strategi pengambilan yang hati-hati." },
  { "en": "Pada *bounding box* (x=200, y=150, w=60, h=60) di gambar 416x416, apa arti x=200?", "id": "x=200 adalah koordinat horizontal (sumbu x) dari sudut kiri atas *bounding box*." },
  { "en": "Informasi tambahan apa yang dibutuhkan untuk mengubah koordinat gambar (x,y) menjadi koordinat dunia (X,Y,Z)?", "id": "Parameter intrinsik & ekstrinsik kamera serta informasi kedalaman (depth) dibutuhkan." },
  { "en": "Apa fungsi parameter intrinsik kamera dalam konversi ini?", "id": "Parameter intrinsik (misalnya, panjang fokus, titik utama) menghubungkan piksel ke koordinat kamera." },
  { "en": "Apa fungsi parameter ekstrinsik kamera?", "id": "Parameter ekstrinsik mendefinisikan posisi dan orientasi kamera relatif terhadap dunia." },
  { "en": "Bagaimana informasi kedalaman (misalnya, dari sensor Kinect atau Lidar) digunakan?", "id": "Kedalaman memberikan koordinat Z yang mengubah titik 2D menjadi titik 3D." },
  { "en": "Bisakah *bounding box* 2D saja memberikan koordinat dunia 3D yang lengkap?", "id": "Tidak, tanpa informasi kedalaman, hanya bisa diproyeksikan ke bidang 2D di dunia." },
  { "en": "Sensor apa yang umum digunakan bersama YOLOv3 untuk mendapatkan informasi kedalaman?", "id": "Kamera RGB-D (seperti Kinect) atau sensor LiDAR sering digunakan." },
  { "en": "Mengapa koordinat dunia penting bagi robot manipulator?", "id": "Manipulator beroperasi dalam ruang 3D dunia, jadi target harus dalam koordinat tersebut." },
  { "en": "Bagaimana pusat *bounding box* (x=200, y=150, w=60, h=60) dihitung dalam koordinat gambar?", "id": "Pusat xc = x + w/2 = 230, pusat yc = y + h/2 = 180." },
  { "en": "Apa itu \"world frame\" dalam robotika?", "id": "\"World frame\" adalah sistem koordinat referensi global yang tetap." },
  { "en": "Sistem mana yang umumnya lebih menuntut latensi rendah: robot bergerak atau manipulator?", "id": "Robot bergerak biasanya lebih menuntut latensi rendah untuk reaksi cepat." },
  { "en": "Sistem mana yang seringkali membutuhkan akurasi deteksi lebih tinggi: robot bergerak atau manipulator?", "id": "Manipulator sering membutuhkan akurasi lebih tinggi untuk interaksi presisi." },
  { "en": "Kebutuhan komputasi YOLOv3 biasanya lebih besar pada sistem mana?", "id": "Kebutuhan komputasi bisa sama, namun robot bergerak mungkin punya batasan daya lebih ketat." },
  { "en": "Mengapa sistem manipulator lebih sensitif terhadap kesalahan deteksi YOLOv3?", "id": "Kesalahan kecil pada manipulator dapat menyebabkan kegagalan menggenggam atau tabrakan." },
  { "en": "Apa konsekuensi jika robot bergerak salah mendeteksi rintangan jauh?", "id": "Mungkin ada cukup waktu untuk koreksi atau dampaknya tidak langsung." },
  { "en": "Apa konsekuensi jika manipulator salah mendeteksi posisi objek yang akan digenggam?", "id": "Manipulator bisa gagal menggenggam, merusak objek, atau merusak dirinya sendiri." },
  { "en": "Bagaimana bidang pandang (*field of view*) kamera biasanya berbeda untuk kedua sistem?", "id": "Robot bergerak sering memiliki FoV lebih lebar, manipulator lebih sempit dan fokus." },
  { "en": "Sistem mana yang lebih mungkin menghadapi objek yang bergerak sangat cepat relatif terhadap kamera?", "id": "Robot bergerak lebih mungkin menghadapi objek eksternal yang bergerak cepat." },
  { "en": "Bagaimana dampak *false positive* berbeda antara robot bergerak dan manipulator?", "id": "*False positive* pada robot bergerak bisa sebabkan pengereman tak perlu, pada manipulator bisa sebabkan gerakan salah." },
  { "en": "Sistem mana yang lebih sering menggunakan YOLOv3 untuk pemetaan lingkungan?", "id": "Robot bergerak lebih sering menggunakan YOLOv3 untuk pemetaan dan SLAM." },
  { "en": "Apa potensi risiko jika YOLOv3 memberikan *false positive* pada robot yang bergerak cepat?", "id": "Robot bisa melakukan manuver penghindaran yang tidak perlu dan berbahaya." },
  { "en": "Bagaimana *false positive* dapat menyebabkan robot bergerak berperilaku tidak stabil?", "id": "Reaksi berulang terhadap deteksi hantu dapat membuat gerakan menjadi tak terduga." },
  { "en": "Strategi mitigasi apa yang bisa digunakan untuk mengurangi dampak *false positive*?", "id": "Menggunakan filter temporal atau fusi dengan sensor lain dapat membantu." },
  { "en": "Bagaimana penyesuaian *confidence threshold* YOLOv3 dapat memitigasi *false positive*?", "id": "Menaikkan *threshold* akan mengurangi deteksi dengan keyakinan rendah, termasuk banyak *false positive*." },
  { "en": "Apa peran pengecekan konsistensi temporal dalam mitigasi *false positive*?", "id": "Deteksi yang hanya muncul sesaat dan tidak konsisten antar frame bisa diabaikan." },
  { "en": "Apakah penggunaan beberapa sensor (*sensor fusion*) bisa membantu mengurangi *false positive*?", "id": "Ya, konfirmasi dari sensor lain dapat memvalidasi deteksi dari YOLOv3." },
  { "en": "Dalam kasus ekstrim, tindakan apa yang bisa diambil robot jika menerima banyak *false positive*?", "id": "Robot bisa berhenti atau masuk mode aman jika lingkungan dianggap terlalu bising." },
  { "en": "Mengapa pengujian menyeluruh penting untuk memitigasi risiko *false positive*?", "id": "Pengujian dapat mengidentifikasi skenario pemicu *false positive* untuk perbaikan model." },
  { "en": "Bisakah sistem melacak \"kepercayaan\" pada deteksi YOLOv3 dari waktu ke waktu?", "id": "Ya, histori kepercayaan deteksi dapat digunakan untuk menilai reliabilitas." },
  { "en": "Apakah *false positive* selalu lebih buruk daripada *false negative* untuk robot bergerak?", "id": "Tergantung aplikasi; *false negative* terhadap rintangan berbahaya bisa lebih fatal." }



// AKHIR DARI BATCH SOAL BARU FINAL INI
        ];

        // ==========================================================================================
        // AKHIR DARI DAFTAR SOAL
        // ==========================================================================================


        let questions = [];

        // Mengurutkan soal tidak wajib jika diacak, tapi bisa membantu saat pengembangan
        // rawVocabularyList.sort((a, b) => a.en.toLowerCase().localeCompare(b.en.toLowerCase()));

        function generateQuestions() {
            const allIndonesianTranslations = rawVocabularyList.map(item => item.id);
            questions = []; // Kosongkan array questions sebelum generate ulang
            rawVocabularyList.forEach(vocabItem => {
                const correctAnswer = vocabItem.id;
                const distractors = [];
                let attempts = 0;
                while (distractors.length < 3 && attempts < allIndonesianTranslations.length * 3) { // Tingkatkan attempts
                    const randomIndex = Math.floor(Math.random() * allIndonesianTranslations.length);
                    const potentialDistractor = allIndonesianTranslations[randomIndex];
                    if (potentialDistractor !== correctAnswer && !distractors.includes(potentialDistractor) && potentialDistractor.trim() !== "") {
                        distractors.push(potentialDistractor);
                    }
                    attempts++;
                }
                while (distractors.length < 3) {
                    const fallbackOptions = ["Jawaban salah acak A", "Pilihan keliru B ini", "Opsi tidak tepat C itu", "Alternatif lain D", "Penyangkalan E yang berbeda", "Jawaban lain F spesial"];
                    let fallbackIndex = Math.floor(Math.random() * fallbackOptions.length); // Acak indeks awal
                    let safetyNet = 0;
                    while(distractors.length < 3 && safetyNet < fallbackOptions.length * 5) {
                        const fbOption = fallbackOptions[fallbackIndex % fallbackOptions.length] + ` (${Math.floor(Math.random()*10000)})`;
                        if (fbOption !== correctAnswer && !distractors.includes(fbOption)) {
                             distractors.push(fbOption);
                        }
                        fallbackIndex++;
                        safetyNet++;
                    }
                     if(distractors.length < 3) {
                         for(let i=0; i < (4-distractors.length); i++){ // Pastikan ada 3 pengecoh
                             distractors.push("Pilihan default super unik " + (i+1+distractors.length) + Math.random().toString(36).substring(2, 10));
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

        // Panggil generateQuestions() setelah rawVocabularyList siap dan sebelum event listener window 'load'
        generateQuestions();


        function saveProgress() {
            if (!questionContainerElement.classList.contains('hide') && orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                const progress = {
                    currentQuestionIndex: currentQuestionIndex,
                    score: score,
                    orderedQuestions: orderedQuestions.map(q => ({
                        question: q.question,
                        answers: q.answers.map(a => ({ text: a.text, correct: a.correct }))
                    }))
                };
                localStorage.setItem('quizProgressElectronikaV2', JSON.stringify(progress)); // Nama item unik
            }
        }

        function loadProgress() {
            const savedProgress = localStorage.getItem('quizProgressElectronikaV2'); // Nama item unik
            if (savedProgress) {
                try {
                    const progressData = JSON.parse(savedProgress);
                    if (progressData && typeof progressData.currentQuestionIndex === 'number' &&
                        typeof progressData.score === 'number' && Array.isArray(progressData.orderedQuestions) &&
                        progressData.orderedQuestions.length > 0 &&
                        progressData.currentQuestionIndex < progressData.orderedQuestions.length &&
                        progressData.orderedQuestions.every(q => q.question && Array.isArray(q.answers) && q.answers.length === 4) &&
                        progressData.orderedQuestions.length === questions.length // Validasi jumlah soal
                        ) {
                        return progressData;
                    } else {
                        clearProgress(); return null;
                    }
                } catch (e) {
                    clearProgress(); return null;
                }
            }
            return null;
        }

        function clearProgress() {
            localStorage.removeItem('quizProgressElectronikaV2'); // Nama item unik
        }

        prev50Button.addEventListener('click', () => navigateQuestions(-JUMP_AMOUNT));
        prevQuestionButton.addEventListener('click', () => navigateQuestions(-1));
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
                if(prevQuestionButton) prevQuestionButton.disabled = true;
                if(next50Button) next50Button.disabled = true;
                return;
            }
            skipNavigationControls.classList.remove('hide');
            const isFirstQuestion = currentQuestionIndex === 0;
            const isLastQuestion = orderedQuestions.length > 0 ? (currentQuestionIndex === (orderedQuestions.length - 1)) : true;

            if(prevQuestionButton) prevQuestionButton.disabled = isFirstQuestion;
            if(prev50Button) prev50Button.disabled = currentQuestionIndex < JUMP_AMOUNT;
            if(next50Button) next50Button.disabled = currentQuestionIndex >= orderedQuestions.length - JUMP_AMOUNT;


            if (orderedQuestions.length <= 1) {
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true;
                if(next50Button) next50Button.disabled = true;
            }
        }

        window.addEventListener('load', () => {
            // `generateQuestions()` sudah dipanggil di atas.
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
                 updateSkipButtonStates();
            }
        });

        startButton.addEventListener('click', () => startGame(false));
        continueButton.addEventListener('click', () => startGame(true));

        function startGame(isContinuing = false) {
            clearTimeout(questionTimeout);
            completionMessageElement.classList.add('hide');
            initialControls.classList.add('hide');
            questionContainerElement.classList.remove('hide');
            questionCounterElement.classList.remove('hide');

            const savedData = isContinuing ? loadProgress() : null;

            if (isContinuing && savedData) {
                if (savedData.orderedQuestions && savedData.orderedQuestions.length === questions.length) { // Pastikan jumlah soal cocok
                    orderedQuestions = savedData.orderedQuestions.map(sq => ({
                        question: sq.question,
                        answers: sq.answers.map(sa => ({text: sa.text, correct: sa.correct}))
                    }));
                    currentQuestionIndex = savedData.currentQuestionIndex;
                    score = savedData.score;
                } else {
                    isContinuing = false; clearProgress(); // Data tidak cocok, mulai baru
                }
            }
            
            if (!isContinuing || !savedData) {
                clearProgress();
                orderedQuestions = [...questions].sort(() => Math.random() - 0.5); // Acak urutan
                currentQuestionIndex = 0;
                score = 0;
            }

            if (!orderedQuestions || orderedQuestions.length === 0) {
                showResults();
                questionCounterElement.innerText = "0/0";
                completionMessageElement.innerText = "Tidak ada soal yang tersedia.";
                completionMessageElement.style.color = "#dc3545";
                completionMessageElement.classList.remove('hide');
                startButton.innerText = 'Mulai';
                initialControls.classList.remove('hide');
                questionContainerElement.classList.add('hide');
                skipNavigationControls.classList.add('hide');
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
            updateSkipButtonStates();
        }

        function showQuestion(questionData) {
            questionElement.innerHTML = questionData.question;
            answerButtonsElement.innerHTML = '';
            const shuffledAnswers = [...questionData.answers].sort(() => Math.random() - 0.5);
            shuffledAnswers.forEach(answer => {
                const button = document.createElement('button');
                button.innerHTML = answer.text;
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
            Array.from(answerButtonsElement.children).forEach(button => {
                clearStatusClass(button);
                button.disabled = false;
            });
        }

        function selectAnswer(e) {
            clearTimeout(questionTimeout);
            const selectedButton = e.target;
            const correct = selectedButton.dataset.correct === 'true';
            if (correct) { score++; }
            Array.from(answerButtonsElement.children).forEach(button => {
                setStatusClass(button, button.dataset.correct === 'true');
                button.disabled = true;
            });
            saveProgress();
            if (orderedQuestions && currentQuestionIndex < orderedQuestions.length -1) {
                 questionTimeout = setTimeout(() => {
                    currentQuestionIndex++;
                    setNextQuestion();
                }, 2000);
            } else {
                questionTimeout = setTimeout(() => {
                    showResults();
                }, 2000);
            }
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
            completionMessageElement.innerHTML = `Selamat Kuis Sudah Selesai 🎉<br>Skor Anda: ${score} dari ${orderedQuestions ? orderedQuestions.length : 0}`;
            completionMessageElement.style.color = "#28a745";
            completionMessageElement.classList.remove('hide');
            startButton.innerText = 'Ulangi Kuis';
            initialControls.classList.remove('hide');
            continueButton.classList.add('hide');
            clearProgress();
        }
    </script>
</body>
</html>
