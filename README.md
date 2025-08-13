<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SUPPORT CLASS - Persiapan Wawancara Kaigo</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Font Awesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Font: Inter -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f3f4f6;
            transition: background-color 0.3s ease;
        }
        body.dark {
            background-color: #1a202c;
            color: #e2e8f0;
        }
        .container {
            max-width: 90%;
            margin: auto;
            padding: 2rem 0;
        }
        .category-button, .question-card, .answer-card-content { /* Renamed answer-card to answer-card-content */
            border-radius: 0.75rem;
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
            transition: all 0.3s ease;
        }
        .category-button:hover, .question-card:hover {
            transform: translateY(-3px);
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05);
        }
        .category-button.active {
            background-color: #4f46e5;
            color: white;
            transform: translateY(-3px);
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05);
        }
        /* Style for furigana (ruby text) */
        ruby {
            ruby-align: center;
            ruby-position: over;
        }
        rt {
            font-size: 0.6em;
            line-height: 1;
        }
        /* Loading spinner */
        .spinner {
            border: 4px solid rgba(0, 0, 0, 0.1);
            border-radius: 50%;
            border-top: 4px solid #4f46e5;
            width: 40px;
            height: 40px;
            animation: spin 1s linear infinite;
            margin: 20px auto;
        }
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
        /* Progress indicator */
        .progress-bar {
            height: 6px;
            background-color: #e0e7ff;
            border-radius: 3px;
            overflow: hidden;
        }
        .progress {
            height: 100%;
            background-color: #4f46e5;
            transition: width 0.3s ease;
        }
    </style>
</head>
<body class="bg-gray-100 min-h-screen flex flex-col items-center justify-center py-8">

    <div class="container bg-white dark:bg-gray-800 p-6 md:p-10 rounded-xl shadow-lg w-full max-w-4xl">
        <!-- Header with dark mode toggle -->
        <div class="flex justify-between items-center mb-6">
            <div>
                <h1 class="text-3xl md:text-4xl font-bold text-indigo-700 dark:text-indigo-400 mb-0">SUPPORT CLASS</h1>
                <p class="text-lg md:text-xl font-semibold text-gray-700 dark:text-gray-300">介護面接の準備 (Persiapan Wawancara Kaigo)</p>
            </div>
            <button id="dark-mode-toggle" class="p-2 rounded-full bg-gray-200 dark:bg-gray-600">
                <i class="fas fa-moon dark:hidden"></i>
                <i class="fas fa-sun hidden dark:block"></i>
            </button>
        </div>

        <!-- Progress bar -->
        <div class="mb-6">
            <div class="flex justify-between text-sm text-gray-600 dark:text-gray-300 mb-1">
                <span>Progress: <span id="progress-count">0</span>/<span id="total-questions">0</span></span>
                <span id="progress-percent">0%</span>
            </div>
            <div class="progress-bar">
                <div id="progress" class="progress" style="width: 0%"></div>
            </div>
        </div>

        <!-- Mode selection -->
        <div class="flex space-x-4 mb-8">
            <button id="study-mode-btn" class="flex-1 bg-indigo-600 hover:bg-indigo-700 text-white font-semibold py-2 px-4 rounded-lg transition">
                <i class="fas fa-book mr-2"></i>Mode Belajar
            </button>
            <button id="quiz-mode-btn" class="flex-1 bg-gray-200 hover:bg-gray-300 dark:bg-gray-600 dark:hover:bg-gray-700 font-semibold py-2 px-4 rounded-lg transition">
                <i class="fas fa-question-circle mr-2"></i>Mode Quiz
            </button>
        </div>

        <!-- Bagian Kategori Pembahasan -->
        <div id="categories-section">
            <h2 class="text-xl md:text-2xl font-semibold text-gray-800 dark:text-gray-200 mb-4 text-center">Pilih Kategori Pembahasan:</h2>
            <div id="category-buttons" class="grid grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4">
                <!-- Tombol kategori akan dimuat di sini oleh JavaScript -->
            </div>
        </div>

        <!-- Bagian Pertanyaan -->
        <div id="questions-section" class="hidden">
            <div class="flex justify-between items-center mb-4">
                <h2 class="text-xl md:text-2xl font-semibold text-gray-800 dark:text-gray-200">Pertanyaan dalam Kategori:</h2>
                <button id="back-to-categories" class="text-indigo-600 dark:text-indigo-400 hover:underline">
                    <i class="fas fa-arrow-left mr-1"></i>Kembali
                </button>
            </div>
            <div id="questions-container" class="space-y-4">
                <!-- Pertanyaan akan dimuat di sini oleh JavaScript -->
            </div>
        </div>

        <!-- Quiz section -->
        <div id="quiz-section" class="hidden">
            <div class="flex justify-between items-center mb-4">
                <h2 class="text-xl md:text-2xl font-semibold text-gray-800 dark:text-gray-200">Mode Quiz</h2>
                <button id="stop-quiz" class="text-red-600 dark:text-red-400 hover:underline">
                    <i class="fas fa-stop mr-1"></i>Berhenti
                </button>
            </div>
            <div class="text-center mb-4">
                <div class="inline-block bg-indigo-100 dark:bg-indigo-900 text-indigo-800 dark:text-indigo-200 px-4 py-2 rounded-full">
                    <i class="fas fa-clock mr-2"></i>
                    <span id="quiz-timer">00:10</span>
                </div>
            </div>
            <div id="quiz-question" class="bg-gradient-to-r from-blue-50 to-indigo-50 dark:from-blue-900 dark:to-indigo-900 p-5 rounded-xl mb-4">
                <!-- Quiz question will appear here -->
            </div>
            <div id="quiz-answer" class="hidden bg-white dark:bg-gray-700 p-4 rounded-xl border border-gray-200 dark:border-gray-600">
                <!-- Quiz answer will appear here -->
            </div>
            <button id="next-quiz-question" class="w-full bg-indigo-600 hover:bg-indigo-700 text-white font-semibold py-3 px-4 rounded-lg mt-4">
                Pertanyaan Berikutnya <i class="fas fa-arrow-right ml-2"></i>
            </button>
        </div>

        <!-- Loading indicator -->
        <div id="loading-indicator" class="hidden">
            <div class="spinner"></div>
            <p class="text-center text-gray-600 dark:text-gray-300">Memuat...</p>
        </div>
    </div>

    <script>
        // Data pembelajaran (sama seperti sebelumnya, diisi dari PDF)
        const japaneseData = {
            "Tujuan & Rencana": [
                {
                    question: "将(しょう)来(らい)の目(もく)標(ひょう)は何(なん)ですか?",
                    meaning: "Apa tujuan Anda di masa depan?",
                    answers: [
                        { text: "将(しょう)来(らい)は介護(かいご)の経験(けいけん)を積(つ)んで、日本(にほん)語(ご)も上達(じょうたつ)させ、家族(かぞく)のために安定(あんてい)した生(せい)活(かつ)を送(おく)りたいです。", meaning: "Saya ingin menambah pengalaman di bidang kaigo, meningkatkan kemampuan bahasa Jepang, dan memberikan kehidupan yang stabil untuk keluarga." }
                    ]
                },
                {
                    question: "介護(かいご)は何(なん)年(ねん)間(かん)やりたいですか?",
                    meaning: "Berapa tahun Anda ingin bekerja di bidang keperawatan lansia?",
                    answers: [
                        { text: "最(さい)低(てい)五(ご)年(ねん)間(かん)は働(はたら)きたいです。", meaning: "Saya ingin bekerja minimal 5 tahun." }
                    ]
                },
                {
                    question: "いくら貯金(ちょきん)したいですか?",
                    meaning: "Berapa banyak uang yang ingin Anda tabung?",
                    answers: [
                        { text: "五(ご)年(ねん)間(かん)で約(やく)二百(にひゃく)万(まん)円(えん)貯金(ちょきん)したいです。", meaning: "Dalam 5 tahun, saya ingin menabung sekitar 2 juta yen." }
                    ]
                },
                {
                    question: "何(なん)年(ねん)後(ご)に帰国(きこく)しますか?",
                    meaning: "Setelah berapa tahun Anda akan kembali ke negara asal?",
                    answers: [
                        { text: "五(ご)年(ねん)後(ご)に帰国(きこく)する予定(よてい)です。", meaning: "Saya berencana pulang setelah 5 tahun." }
                    ]
                },
                {
                    question: "五(ご)年(ねん)経(た)つ前(まえ)に一時(いちじ)帰国(きこく)をしたいですか?",
                    meaning: "Apakah Anda ingin pulang sementara sebelum 5 tahun berlalu?",
                    answers: [
                        { text: "はい、可能(かのう)であれば帰(かえ)りたいです。", meaning: "Ya, kalau memungkinkan saya ingin pulang sebentar." }
                    ]
                }
            ],
            "Keuangan & Akomodasi": [
                {
                    question: "日本(にほん)に来(く)るために借金(しゃっきん)をしましたか?",
                    meaning: "Apakah Anda memiliki utang untuk pergi ke Jepang?",
                    answers: [
                        { text: "いいえ、借金(しゃっきん)はしていません。", meaning: "Tidak, saya tidak berutang." },
                        { text: "はい、親戚(しんせき)から三十(さんじゅう)万(まん)円(えん)借(か)りて、二年(にねん)間(かん)で返済(へんさい)します。", meaning: "Ya, saya meminjam 300 ribu yen dari saudara dan akan melunasi dalam 2 tahun." }
                    ]
                },
                {
                    question: "飛行機(ひこうき)代(だい)は、ありますか?",
                    meaning: "Apakah Anda memiliki biaya (tiket pesawat) untuk pergi ke Jepang?",
                    answers: [
                        { text: "はい、準備(じゅんび)しています。", meaning: "Ya, sudah saya siapkan." }
                    ]
                },
                {
                    question: "アパートはシェアルームできますか?",
                    meaning: "Apakah Anda bisa tinggal di apartemen bersama?",
                    answers: [
                        { text: "はい、同(おな)じ会(かい)社(しゃ)の同僚(どうりょう)なら三人(さんにん)まで可能(かのう)です。", meaning: "Ya, bisa berbagi dengan rekan kerja, maksimal 3 orang." }
                    ]
                },
                {
                    question: "シェアルーム生活(せいかつ)をしたことがありますか?",
                    meaning: "Apakah Anda pernah tinggal bersama orang lain sebelumnya?",
                    answers: [
                        { text: "はい、以前(いぜん)学(がく)生(せい)寮(りょう)で暮(くら)した経験(けいけん)があります。", meaning: "Ya, saya pernah tinggal di asrama." }
                    ]
                },
                {
                    question: "毎月(まいつき)いくら家族(かぞく)に送金(そうきん)しますか?",
                    meaning: "Apakah Anda berencana mengirim uang ke keluarga setiap bulan (berapa)?",
                    answers: [
                        { text: "毎月(まいつき)五(ご)万(まん)円(えん)ぐらい送金(そうきん)したいです。", meaning: "Saya ingin mengirim sekitar 50 ribu yen per bulan." }
                    ]
                }
            ],
            "Motivasi & Bahasa": [
                {
                    question: "なぜ、日本(にほん)へ行(い)きたいですか?",
                    meaning: "Mengapa Anda ingin pergi ke Jepang?",
                    answers: [
                        { text: "介護(かいご)の技術(ぎじゅつ)を学(まな)び、日本(にほん)語(ご)を上達(じょうたつ)させたいからです。", meaning: "Karena saya ingin belajar teknik kaigo dan meningkatkan kemampuan bahasa Jepang." }
                    ]
                },
                {
                    question: "日本(にほん)には友人(ゆうじん)や家族(かぞく)はいますか?",
                    meaning: "Apakah Anda memiliki teman atau keluarga di Jepang?",
                    answers: [
                        { text: "はい、友人(ゆうじん)が一人(ひとり)います。", meaning: "Ya, saya punya satu teman." },
                        { text: "いいえ、いません。", meaning: "Tidak ada." }
                    ]
                },
                {
                    question: "外(がい)食(しょく)、介護(かいご)、農業(のうぎょう)と試験(しけん)がありますが、なぜ介護(かいご)で働(はたら)きたいですか?",
                    meaning: "Apa alasan spesifik Anda ingin bekerja di bidang keperawatan lansia?",
                    answers: [
                        { text: "高(こう)齢(れい)者(しゃ)の役(やく)に立(た)ちたいと思(おも)い、介護(かいご)を選(えら)びました。", meaning: "Karena saya ingin bermanfaat bagi lansia, saya memilih kaigo." }
                    ]
                },
                {
                    question: "普(ふ)段(だん)日本(にほん)語(ご)は話(はな)しますか?",
                    meaning: "Apakah Anda berbicara bahasa Jepang dalam keseharian?",
                    answers: [
                        { text: "はい、毎日(まいにち)少(すこ)し話(はな)しています。", meaning: "Ya, setiap hari saya berbicara sedikit bahasa Jepang." }
                    ]
                },
                {
                    question: "日本(にほん)語(ご)はどうやって勉強(べんきょう)していますか? 何(なん)時間(じかん)?",
                    meaning: "Bagaimana Anda belajar bahasa Jepang? Berapa jam belajar per hari?",
                    answers: [
                        { text: "教科書(きょうかしょ)と動画(どうが)で毎日(まいにち)二(に)時間(じかん)勉強(べんきょう)しています。", meaning: "Saya belajar dengan buku dan video selama 2 jam setiap hari." }
                    ]
                },
                {
                    question: "日本(にほん)で働(はたら)きたい理由(りゆう)",
                    meaning: "Mengapa Anda ingin bekerja di Jepang?",
                    answers: [
                        { text: "介護(かいご)技術(ぎじゅつ)を学(まな)び、将(しょう)来(らい)に活(い)かしたいからです。", meaning: "Karena saya ingin belajar teknik kaigo dan memanfaatkannya di masa depan." }
                    ]
                }
            ],
            "Lingkungan Kerja & Adaptasi": [
                {
                    question: "働(はたら)く会社(かいしゃ)の施設(しせつ)の写真(しゃしん)やホームページを見(み)ましたか?",
                    meaning: "Apakah Anda sudah melihat foto atau situs dari fasilitas tempat Anda melamar?",
                    answers: [
                        { text: "はい、見(み)ました。綺麗(きれい)で安心(あんしん)しました。", meaning: "Ya, sudah saya lihat. Tempatnya bersih dan membuat saya tenang." }
                    ]
                },
                {
                    question: "先輩(せんぱい)スタッフとどうやって仲良(なかよ)くなりますか?",
                    meaning: "Bagaimana Anda akan menjalin hubungan baik dengan staf?",
                    answers: [
                        { text: "挨拶(あいさつ)をして、仕事(しごと)を手伝(てつだ)いながら仲良(なかよ)くなります。", meaning: "Dengan memberi salam dan membantu pekerjaan, saya akan akrab dengan staf senior." }
                    ]
                },
                {
                    question: "時間(じかん)の短縮(たんしゅく)と細(こま)やかな対応(たいおう)どちらを選(えら)びますか?",
                    meaning: "Jika harus memilih, Anda pilih efisiensi waktu atau perhatian yang rinci?",
                    answers: [
                        { text: "細(こま)やかな対応(たいおう)を選(えら)びます。", meaning: "Saya memilih pelayanan yang teliti." }
                    ]
                },
                {
                    question: "日本(にほん)のどんな所(ところ)が好(す)きですか?",
                    meaning: "Apa yang Anda sukai dari Jepang?",
                    answers: [
                        { text: "綺麗(きれい)な環境(かんきょう)と安全(あんぜん)な街(まち)が好(す)きです。", meaning: "Saya suka lingkungan yang bersih dan kota yang aman." }
                    ]
                },
                {
                    question: "日本(にほん)に行(い)ってみたい場(ば)所(しょ)は、ありますか?",
                    meaning: "Tempat mana di Jepang yang ingin Anda kunjungi?",
                    answers: [
                        { text: "京都(きょうと)と富士山(ふじさん)に行(い)ってみたいです。", meaning: "Saya ingin pergi ke Kyoto dan Gunung Fuji." }
                    ]
                }
            ],
            "Karakter & Pengalaman": [
                {
                    question: "あなたの長(ちょう)所(しょ)と短所(たんしょ)を教(おし)えてください。",
                    meaning: "Sebutkan kelebihan dan kekurangan Anda.",
                    answers: [
                        { text: "長所(ちょうしょ)は責任(せきにん)感(かん)が強(つよ)いこと、短所(たんしょ)は少(すこ)し心配(しんぱい)しすぎる所(ところ)です。", meaning: "Kelebihan saya adalah rasa tanggung jawab yang tinggi, kekurangan saya terlalu banyak khawatir." }
                    ]
                },
                {
                    question: "介護(かいご)に役立(やくだ)つと思(おも)う自分(じぶん)の長(ちょう)所(しょ)や経験(けいけん)をおしえてください。",
                    meaning: "Apa kelebihan atau pengalaman Anda yang berguna untuk keperawatan?",
                    answers: [
                        { text: "忍耐(にんたい)力(りょく)と協調(きょうちょう)性(せい)があります。家(いえ)で祖母(そぼ)の世話(せわ)をした経験(けいけん)があります。", meaning: "Saya punya kesabaran dan kerja sama yang baik. Saya pernah merawat nenek di rumah." }
                    ]
                },
                {
                    question: "普(ふ)段(だん)高(こう)齢(れい)者(しゃ)と接(せっ)することはありますか?",
                    meaning: "Apakah Anda biasa berinteraksi dengan orang lanjut usia?",
                    answers: [
                        { text: "はい、祖父(そふ)とよく話(はな)します。", meaning: "Ya, saya sering berbicara dengan kakek-nenek." }
                    ]
                },
                {
                    question: "介護(かいご)で、一番(いちばん)大切(たいせつ)なことは何(なん)だとおもいますか?",
                    meaning: "Menurut Anda, apa hal yang paling penting dalam pekerjaan keperawatan?",
                    answers: [
                        { text: "思(おも)いやりと安全(あんぜん)だとおもいます。", meaning: "Menurut saya, empati dan keselamatan adalah yang terpenting." }
                    ]
                },
                {
                    question: "性(せい)格(かく)を教(おし)えてください。",
                    meaning: "Bagaimana kepribadian Anda?",
                    answers: [
                        { text: "明(あか)るくて真面目(まじめ)です。", meaning: "Saya ceria dan serius." }
                    ]
                },
                {
                    question: "インドネシアの良(よ)い所(ところ)を教(おし)えてください。",
                    meaning: "Apa hal baik dari Indonesia?",
                    answers: [
                        { text: "自然(しぜん)が綺麗(きれい)で、人(ひと)が優(やさ)しい所(ところ)です。", meaning: "Alamnya indah dan orang-orangnya ramah." }
                    ]
                },
                {
                    question: "料(りょう)理(り)は得意(とくい)ですか?",
                    meaning: "Apakah Anda pandai memasak?",
                    answers: [
                        { text: "はい、得意(とくい)です。特(とく)に炒(いた)め物(もの)です。", meaning: "Ya, saya pandai memasak, terutama tumisan." }
                    ]
                },
                {
                    question: "日本(にほん)語(ご)で好(す)きな言(こと)葉(ば)は何(なん)ですか?",
                    meaning: "Apa kata-kata favorit Anda dalam bahasa Jepang?",
                    answers: [
                        { text: "「ありがとう」です。", meaning: "Kata 'terima kasih'." }
                    ]
                },
                {
                    question: "今(いま)、インドネシアでどんな仕(し)事(ごと)をしていますか?",
                    meaning: "Apa pekerjaan Anda saat ini di Indonesia?",
                    answers: [
                        { text: "介護(かいご)の勉強(べんきょう)をしながら、パートで働(はたら)いています。", meaning: "Saya belajar kaigo sambil bekerja paruh waktu." }
                    ]
                },
                {
                    question: "特技(とくぎ)はありますか?",
                    meaning: "Apakah Anda punya keahlian khusus?",
                    answers: [
                        { text: "人(ひと)とすぐ仲良(なかよ)くなれることです。", meaning: "Saya mudah akrab dengan orang baru." }
                    ]
                },
                {
                    question: "今(いま)までの仕(し)事(ごと)で、一(いち)番(ばん)頑(がん)張(ば)ったことは何(なに)ですか？",
                    meaning: "Apa hal yang paling Anda usahakan dalam pekerjaan sebelumnya?",
                    answers: [
                        { text: "一番(いちばん)頑張(がんば)ったことは、[proyek/tugas]で[hasil positif]を達成(たっせい)したことです。", meaning: "Hal yang paling saya usahakan adalah mencapai [hasil positif] pada [proyek/tugas]." },
                        { text: "困難(こんなん)な状況(じょうきょう)を乗(の)り越(こ)えるために、[usaha]をしました。", meaning: "Saya melakukan [usaha] untuk mengatasi situasi sulit." }
                    ]
                }
            ],
            "Kesehatan & Fisik": [
                {
                    question: "夜勤(やきん)はできますか?",
                    meaning: "Apakah Anda bisa bekerja shift malam?",
                    answers: [
                        { text: "はい、できます。", meaning: "Ya, bisa." }
                    ]
                },
                {
                    question: "身体(からだ)で痛(いた)い所(ところ)はありますか?",
                    meaning: "Apakah ada bagian tubuh Anda yang sakit?",
                    answers: [
                        { text: "いいえ、ありません。", meaning: "Tidak ada." }
                    ]
                },
                {
                    question: "自(じ)転(てん)車(しゃ)に乗(の)れますか?",
                    meaning: "Apakah Anda bisa naik sepeda?",
                    answers: [
                        { text: "はい、乗(の)れます。", meaning: "Ya, bisa." }
                    ]
                },
                {
                    question: "体(たい)力(りょく)に自信(じしん)はありますか?",
                    meaning: "Apakah Anda percaya diri dengan kekuatan fisik Anda?",
                    answers: [
                        { text: "はい、自信(じしん)があります。", meaning: "Ya, saya percaya diri." }
                    ]
                }
            ],
            "Keluarga & Kekhawatiran": [
                {
                    question: "来日(らいにち)について家族(かぞく)は了承(りょうしょう)していますか?",
                    meaning: "Apakah keluarga Anda setuju dengan rencana Anda pergi ke Jepang?",
                    answers: [
                        { text: "はい、応援(おうえん)してくれています。", meaning: "Ya, keluarga saya mendukung." }
                    ]
                },
                {
                    question: "日本(にほん)に来(く)ることで何(なに)か心配(しんぱい)や不安(ふあん)はありますか?",
                    meaning: "Apakah Anda merasa khawatir atau cemas tentang pergi ke Jepang?",
                    answers: [
                        { text: "最初(さいしょ)の生活(せいかつ)に少(すこ)し不安(ふあん)がありますが、頑張(がんば)ります。", meaning: "Ada sedikit khawatir tentang awal kehidupan di sana, tapi saya akan berusaha." }
                    ]
                },
                {
                    question: "家族(かぞく)と離(はな)れて寂(さび)しくないですか?",
                    meaning: "Apakah Anda tidak merasa kesepian jika berpisah dari keluarga?",
                    answers: [
                        { text: "少(すこ)し寂(さび)しいですが、頑張(がんば)ります。", meaning: "Akan sedikit rindu, tapi saya akan berusaha." }
                    ]
                }
            ],
            "Kemampuan Bahasa Jepang Lanjutan": [
                {
                    question: "平仮名(ひらがな)とカタカナどちらが得意(とくい)ですか?",
                    meaning: "Anda lebih mahir Hiragana atau Katakana?",
                    answers: [
                        { text: "平仮名(ひらがな)が得意(とくい)です。", meaning: "Lebih mahir hiragana." }
                    ]
                },
                {
                    question: "漢(かん)字(じ)は、読(よ)み書(か)きできますか?",
                    meaning: "Apakah Anda bisa membaca dan menulis Kanji?",
                    answers: [
                        { text: "少(すこ)しできます。", meaning: "Sedikit bisa." }
                    ]
                }
            ],
            "Hobi & Lain-lain": [
                 {
                    question: "何(なに)かスポーツはやっていますか?",
                    meaning: "Apakah Anda melakukan olahraga?",
                    answers: [
                        { text: "はい、バドミントンをしています。", meaning: "Ya, saya bermain bulu tangkis." }
                    ]
                },
                {
                    question: "趣(しゅ)味(み)は何(なん)ですか?",
                    meaning: "Apa hobi Anda?",
                    answers: [
                        { text: "音楽(おんがく)を聴(き)くことです。", meaning: "Mendengarkan musik." }
                    ]
                },
                 {
                    question: "介護(かいご)ビザと特定(とくてい)技能(ぎのう)一(いち)号(ごう)介護(かいご)の違(ちが)いは分(わ)かりますか?",
                    meaning: "Apakah Anda tahu perbedaan antara visa keperawatan dan Tokutei Ginou No.1 bidang keperawatan?",
                    answers: [
                        { text: "はい、介護(かいご)ビザは資格(しかく)取得(しゅとく)後(ご)に取得(しゅとく)でき、特定(とくてい)技能(ぎのう)一(いち)号(ごう)介護(かいご)は試験(しけん)に合格(ごうかく)して取得(しゅとく)できます。", meaning: "Ya, saya tahu. Visa kaigo didapat setelah lulus kualifikasi resmi, sedangkan Tokutei Ginou No.1 kaigo setelah lulus ujian." }
                    ]
                },
                {
                    question: "今(いま)までに企(き)業(ぎょう)面接(めんせつ)は受(う)けたことがありますか?",
                    meaning: "Apakah Anda pernah mengikuti wawancara kerja sebelumnya?",
                    answers: [
                        { text: "はい、あります。", meaning: "Ya, pernah." },
                        { text: "いいえ、ありません。", meaning: "Tidak pernah." }
                    ]
                }
            ],
            "Pembukaan & Penutupan Wawancara": [
                 {
                    question: "本(ほん)日(じつ)はよろしくお願(ねが)いいたします。",
                    meaning: "Saya berharap dapat bekerja sama dengan Anda hari ini. (Ucapan pembuka saat bertemu)",
                    answers: [
                        { text: "こちらこそ、よろしくお願(ねが)いいたします。", meaning: "Sama-sama, saya juga berharap dapat bekerja sama dengan Anda." },
                        { text: "ありがとうございます。", meaning: "Terima kasih." }
                    ]
                },
                {
                    question: "自己(じこ)紹介(しょうかい)をお願(ねが)いします。",
                    meaning: "Tolong perkenalkan diri Anda.",
                    answers: [
                        { text: "はい。私(わたし)の名前(なまえ)は[nama Anda]です。どうぞよろしくお願(ね)いいたします。", meaning: "Ya. Nama saya [nama Anda]. Senang bertemu Anda." },
                        { text: "私(わたし)は[usia]歳(さい)で、[asal negara/kota]から来(き)ました。", meaning: "Saya berumur [usia] tahun dan berasal dari [negara/kota Anda]." }
                    ]
                },
                {
                    question: "本(ほん)日(じつ)はありがとうございました。",
                    meaning: "Terima kasih banyak untuk hari ini. (Ucapan penutup wawancara)",
                    answers: [
                        { text: "こちらこそ、お時(じ)間(かん)をいただきありがとうございました。", meaning: "Sama-sama, terima kasih atas waktu yang telah diberikan." },
                        { text: "結(けっ)果(か)はいつ頃(ごろ)分(わ)かりますか？", meaning: "Kapan kira-kira hasilnya akan diketahui?" }
                    ]
                },
                {
                    question: "何(なに)か質(しつ)問(もん)はありますか？",
                    meaning: "Apakah ada pertanyaan?",
                    answers: [
                        { text: "はい、一(ひと)つ質(しつ)問(もん)があります。御社(おんしゃ)で働(はたら)く上(うえ)で、最(もっと)も重(じゅう)要(よう)なスキルは何(なに)ですか？", meaning: "Ya, saya ada satu pertanyaan. Apa keterampilan yang paling penting untuk bekerja di perusahaan ini?" },
                        { text: "御社(おんしゃ)の今(こん)後(ご)の展(てん)望(ぼう)について教(おし)えていただけますか？", meaning: "Bisakah Anda ceritakan tentang prospek masa depan perusahaan ini?" },
                        { text: "今(いま)のところございません。ありがとうございます。", meaning: "Saat ini tidak ada. Terima kasih banyak." }
                    ]
                }
            ]
        };

        // DOM Elements
        const categoryButtonsContainer = document.getElementById('category-buttons');
        const categoriesSection = document.getElementById('categories-section');
        const questionsSection = document.getElementById('questions-section');
        const questionsContainer = document.getElementById('questions-container');
        const quizSection = document.getElementById('quiz-section');
        const quizQuestion = document.getElementById('quiz-question');
        const quizAnswer = document.getElementById('quiz-answer');
        // const searchInput = document.getElementById('search-input'); // Removed search input
        const progressCount = document.getElementById('progress-count');
        const totalQuestions = document.getElementById('total-questions');
        const progressPercent = document.getElementById('progress-percent');
        const progressBar = document.getElementById('progress');
        const loadingIndicator = document.getElementById('loading-indicator');
        const quizTimer = document.getElementById('quiz-timer');

        // State variables
        let currentActiveButton = null;
        let currentCategory = null;
        let currentQuestions = [];
        let quizQuestions = [];
        let currentQuizIndex = 0;
        let timerInterval = null;
        let timeLeft = 10;
        let answeredQuestions = JSON.parse(localStorage.getItem('answeredQuestions')) || {};
        let activeAnswerContainer = null; // To track the currently open answer section

        // Initialize the app
        function init() {
            displayCategories();
            setupEventListeners();
            updateProgress();
            checkDarkModePreference();
        }

        // Set up event listeners
        function setupEventListeners() {
            // Dark mode toggle
            document.getElementById('dark-mode-toggle').addEventListener('click', toggleDarkMode);
            
            // Navigation buttons
            document.getElementById('back-to-categories').addEventListener('click', () => {
                questionsSection.classList.add('hidden');
                categoriesSection.classList.remove('hidden');
                // Reset active button state
                if (currentActiveButton) {
                    currentActiveButton.classList.remove('active', 'bg-indigo-600');
                    currentActiveButton.classList.add('bg-indigo-500');
                    currentActiveButton = null; // Clear active button
                }
                currentCategory = null; // Clear current category
                updateProgress(); // Reset progress display
                // Close any open answer sections
                if (activeAnswerContainer) {
                    activeAnswerContainer.classList.add('hidden');
                    activeAnswerContainer = null;
                }
            });
            
            // Mode selection
            document.getElementById('study-mode-btn').addEventListener('click', () => {
                setActiveMode('study');
            });
            
            document.getElementById('quiz-mode-btn').addEventListener('click', () => {
                setActiveMode('quiz');
            });
            
            // Quiz controls
            document.getElementById('stop-quiz').addEventListener('click', stopQuiz);
            document.getElementById('next-quiz-question').addEventListener('click', showNextQuizQuestion);
            
            // Search functionality removed, so no event listener for search input
            // searchInput.addEventListener('input', handleSearch); 
        }

        // Display categories
        function displayCategories() {
            categoryButtonsContainer.innerHTML = '';
            for (const category in japaneseData) {
                const button = document.createElement('button');
                button.textContent = category;
                button.classList.add('category-button', 'bg-indigo-500', 'hover:bg-indigo-600', 'text-white', 'font-semibold', 'py-3', 'px-5', 'rounded-xl', 'cursor-pointer', 'text-lg', 'text-center', 'break-words');
                button.addEventListener('click', () => {
                    currentCategory = category;
                    displayQuestions(category);
                    if (currentActiveButton) {
                        currentActiveButton.classList.remove('active', 'bg-indigo-600');
                        currentActiveButton.classList.add('bg-indigo-500');
                    }
                    button.classList.add('active', 'bg-indigo-600');
                    currentActiveButton = button;
                });
                categoryButtonsContainer.appendChild(button);
            }
        }

        // Display questions for a category
        function displayQuestions(category) {
            loadingIndicator.classList.remove('hidden');
            questionsContainer.innerHTML = '';
            
            setTimeout(() => {
                loadingIndicator.classList.add('hidden');
                categoriesSection.classList.add('hidden');
                questionsSection.classList.remove('hidden');
                quizSection.classList.add('hidden'); // Ensure quiz section is hidden
                
                currentQuestions = japaneseData[category];
                totalQuestions.textContent = currentQuestions.length;
                
                currentQuestions.forEach((item, index) => {
                    const questionCard = document.createElement('div');
                    questionCard.classList.add('question-card', 'bg-gradient-to-r', 'from-blue-50', 'to-indigo-50', 'dark:from-blue-900', 'dark:to-indigo-900', 'p-5', 'rounded-xl', 'cursor-pointer', 'relative', 'overflow-hidden'); // Added 'overflow-hidden' for smooth height transition if implemented
                    
                    // Create an answers container specific to this question card
                    const answersContainerForCard = document.createElement('div');
                    answersContainerForCard.classList.add('answers-container-for-card', 'mt-4', 'pt-4', 'border-t', 'border-gray-200', 'dark:border-gray-600', 'hidden'); // Hidden by default

                    // Populate answersContainerForCard
                    answersContainerForCard.innerHTML = `
                        <h3 class="text-lg font-semibold text-gray-800 dark:text-gray-200 mb-2">Alternatif Jawaban:</h3>
                        ${item.answers.map(answer => `
                            <div class="answer-card-content bg-white dark:bg-gray-700 p-4 rounded-xl border border-gray-200 dark:border-gray-600 flex flex-col items-start mb-2">
                                <div class="flex items-center justify-between w-full mb-2">
                                    <p class="text-lg font-medium text-gray-900 dark:text-gray-100">${createRubyText(answer.text)}</p>
                                    <button class="speak-button bg-indigo-500 hover:bg-indigo-600 text-white px-3 py-1 rounded-md text-sm" data-text="${answer.text}">
                                        <i class="fas fa-volume-up mr-1"></i>Dengar
                                    </button>
                                </div>
                                <p class="text-gray-500 dark:text-gray-400 text-sm"><em>${answer.meaning}</em></p>
                                <p class="text-indigo-600 dark:text-indigo-400 text-sm mt-3">
                                    <i class="fas fa-lightbulb mr-1"></i>Tips: Gunakan ini dengan senyuman untuk tampak percaya diri!
                                </p>
                            </div>
                        `).join('')}
                    `;
                    
                    // Add checkmark if question was answered
                    const isAnswered = answeredQuestions[item.question];
                    
                    questionCard.innerHTML = `
                        <div class="flex justify-between items-start">
                            <div class="flex-1 pr-8">
                                <p class="text-xl font-medium text-indigo-800 dark:text-indigo-200">${createRubyText(item.question)}</p>
                                <p class="text-gray-600 dark:text-gray-300 mt-1"><em>${item.meaning}</em></p>
                            </div>
                            ${isAnswered ? '<i class="fas fa-check-circle text-green-500 text-2xl absolute top-4 right-4"></i>' : ''}
                        </div>
                    `;
                    questionCard.appendChild(answersContainerForCard); // Append the answers container to the question card
                    
                    questionCard.addEventListener('click', (event) => {
                        // Prevent clicking on speak button from toggling answer section
                        if (event.target.closest('.speak-button')) {
                            return;
                        }

                        // Close currently active answer container if different
                        if (activeAnswerContainer && activeAnswerContainer !== answersContainerForCard) {
                            activeAnswerContainer.classList.add('hidden');
                        }

                        // Toggle visibility of the answers for THIS question
                        answersContainerForCard.classList.toggle('hidden');
                        activeAnswerContainer = answersContainerForCard.classList.contains('hidden') ? null : answersContainerForCard;

                        // Mark as answered if answers are shown
                        if (!answersContainerForCard.classList.contains('hidden')) {
                            if (!answeredQuestions[item.question]) {
                                answeredQuestions[item.question] = true;
                                localStorage.setItem('answeredQuestions', JSON.stringify(answeredQuestions));
                                updateProgress();
                                // Add checkmark dynamically if not already present
                                if (!questionCard.querySelector('.fa-check-circle')) {
                                    const checkmark = document.createElement('i');
                                    checkmark.classList.add('fas', 'fa-check-circle', 'text-green-500', 'text-2xl', 'absolute', 'top-4', 'right-4');
                                    questionCard.appendChild(checkmark);
                                }
                            }
                            // Scroll to the clicked question to show answers fully
                            questionCard.scrollIntoView({ behavior: 'smooth', block: 'start' });
                        }
                    });

                    // Add event listeners for speak buttons within this card's answer container
                    answersContainerForCard.querySelectorAll('.speak-button').forEach(button => {
                        button.addEventListener('click', (event) => {
                            event.stopPropagation(); // Prevent the card click from happening when speak button is clicked
                            speakText(button.dataset.text);
                        });
                    });
                    
                    questionsContainer.appendChild(questionCard);
                });
                
                questionsSection.scrollIntoView({ behavior: 'smooth', block: 'start' });
            }, 500);
        }

        // Set active mode (study or quiz)
        function setActiveMode(mode) {
            // Reset UI visibility for modes
            categoriesSection.classList.add('hidden');
            questionsSection.classList.add('hidden');
            quizSection.classList.add('hidden');

            // Reset quiz timer if active
            if (timerInterval) {
                clearInterval(timerInterval);
                timerInterval = null;
            }

            // Update button styles
            const studyBtn = document.getElementById('study-mode-btn');
            const quizBtn = document.getElementById('quiz-mode-btn');
            
            studyBtn.classList.remove('bg-indigo-600', 'hover:bg-indigo-700', 'text-white', 'bg-gray-200', 'hover:bg-gray-300', 'dark:bg-gray-600', 'dark:hover:bg-gray-700');
            quizBtn.classList.remove('bg-indigo-600', 'hover:bg-indigo-700', 'text-white', 'bg-gray-200', 'hover:bg-gray-300', 'dark:bg-gray-600', 'dark:hover:bg-gray-700');

            if (mode === 'study') {
                studyBtn.classList.add('bg-indigo-600', 'hover:bg-indigo-700', 'text-white');
                quizBtn.classList.add('bg-gray-200', 'hover:bg-gray-300', 'dark:bg-gray-600', 'dark:hover:bg-gray-700');
                categoriesSection.classList.remove('hidden'); // Show categories for study mode
                currentCategory = null; // Reset category selection
                if (currentActiveButton) { // Deselect active category button
                    currentActiveButton.classList.remove('active', 'bg-indigo-600');
                    currentActiveButton.classList.add('bg-indigo-500');
                    currentActiveButton = null;
                }
                updateProgress(); // Reset progress display
                // Close any open answer sections when switching back to study mode (categories)
                if (activeAnswerContainer) {
                    activeAnswerContainer.classList.add('hidden');
                    activeAnswerContainer = null;
                }
            } else if (mode === 'quiz') {
                quizBtn.classList.add('bg-indigo-600', 'hover:bg-indigo-700', 'text-white');
                studyBtn.classList.add('bg-gray-200', 'hover:bg-gray-300', 'dark:bg-gray-600', 'dark:hover:bg-gray-700');
                startQuiz(); // Start quiz mode
            }
        }

        // Start quiz mode
        function startQuiz() {
            if (!currentCategory) {
                // Use a custom modal instead of alert()
                const modal = document.createElement('div');
                modal.classList.add('fixed', 'inset-0', 'bg-gray-900', 'bg-opacity-75', 'flex', 'items-center', 'justify-center', 'z-50');
                modal.innerHTML = `
                    <div class="bg-white dark:bg-gray-700 p-6 rounded-lg shadow-xl text-center">
                        <p class="text-lg font-semibold text-gray-800 dark:text-gray-100 mb-4">Pilih kategori terlebih dahulu!</p>
                        <button id="close-modal" class="bg-indigo-600 hover:bg-indigo-700 text-white font-semibold py-2 px-4 rounded-lg">OK</button>
                    </div>
                `;
                document.body.appendChild(modal);
                document.getElementById('close-modal').addEventListener('click', () => {
                    document.body.removeChild(modal);
                    setActiveMode('study'); // Go back to study mode if no category selected
                });
                return;
            }
            
            quizQuestions = [...japaneseData[currentCategory]];
            currentQuizIndex = 0;
            
            // Shuffle questions
            quizQuestions = shuffleArray(quizQuestions);
            
            categoriesSection.classList.add('hidden');
            questionsSection.classList.add('hidden');
            quizSection.classList.remove('hidden');
            
            showNextQuizQuestion();
        }

        // Show next quiz question
        function showNextQuizQuestion() {
            if (currentQuizIndex >= quizQuestions.length) {
                // Quiz completed screen
                quizSection.innerHTML = `
                    <div class="text-center py-8">
                        <i class="fas fa-check-circle text-green-500 text-5xl mb-4"></i>
                        <h2 class="text-2xl font-bold text-gray-800 dark:text-gray-200 mb-2">Quiz Selesai!</h2>
                        <p class="text-gray-600 dark:text-gray-300 mb-4">Anda telah menyelesaikan semua pertanyaan di kategori ini.</p>
                        <button id="restart-quiz" class="bg-indigo-600 hover:bg-indigo-700 text-white font-semibold py-2 px-6 rounded-lg">
                            <i class="fas fa-redo mr-2"></i>Mulai Lagi
                        </button>
                        <button id="back-to-categories-from-quiz" class="bg-gray-200 hover:bg-gray-300 dark:bg-gray-600 dark:hover:bg-gray-700 font-semibold py-2 px-6 rounded-lg ml-4">
                            <i class="fas fa-home mr-2"></i>Kembali ke Kategori
                        </button>
                    </div>
                `;
                
                document.getElementById('restart-quiz').addEventListener('click', startQuiz);
                document.getElementById('back-to-categories-from-quiz').addEventListener('click', () => {
                    setActiveMode('study');
                });
                
                return;
            }
            
            const question = quizQuestions[currentQuizIndex];
            quizAnswer.classList.add('hidden');
            
            // Reset timer
            timeLeft = 10;
            updateTimerDisplay();
            
            if (timerInterval) {
                clearInterval(timerInterval);
            }
            
            timerInterval = setInterval(() => {
                timeLeft--;
                updateTimerDisplay();
                
                if (timeLeft <= 0) {
                    clearInterval(timerInterval);
                    showQuizAnswer(question);
                }
            }, 1000);
            
            quizQuestion.innerHTML = `
                <div class="mb-4">
                    <p class="text-xl font-medium text-indigo-800 dark:text-indigo-200">${createRubyText(question.question)}</p>
                    <p class="text-gray-600 dark:text-gray-300 mt-1"><em>${question.meaning}</em></p>
                </div>
                <button id="show-answer-btn" class="bg-indigo-100 hover:bg-indigo-200 dark:bg-indigo-900 dark:hover:bg-indigo-800 text-indigo-800 dark:text-indigo-200 font-medium py-2 px-4 rounded-lg">
                    <i class="fas fa-eye mr-2"></i>Tampilkan Jawaban
                </button>
            `;
            
            document.getElementById('show-answer-btn').addEventListener('click', () => {
                clearInterval(timerInterval);
                showQuizAnswer(question);
            });
            
            currentQuizIndex++;
        }

        // Show quiz answer
        function showQuizAnswer(question) {
            quizAnswer.classList.remove('hidden');
            quizAnswer.innerHTML = `
                <h3 class="text-lg font-semibold text-gray-800 dark:text-gray-200 mb-2">Jawaban:</h3>
                ${question.answers.map(ans => `
                    <div class="mb-2">
                        <p class="text-gray-700 dark:text-gray-300">${createRubyText(ans.text)}</p>
                        <p class="text-gray-600 dark:text-gray-400 text-sm"><em>${ans.meaning}</em></p>
                    </div>
                `).join('')}
                <p class="text-indigo-600 dark:text-indigo-400 text-sm mt-3">
                    <i class="fas fa-lightbulb mr-1"></i>Tips: Gunakan ini dengan senyuman untuk tampak percaya diri!
                </p>
            `;
        }

        // Stop quiz
        function stopQuiz() {
            if (timerInterval) {
                clearInterval(timerInterval);
                timerInterval = null;
            }
            setActiveMode('study');
        }

        // Update timer display
        function updateTimerDisplay() {
            const minutes = Math.floor(timeLeft / 60);
            const seconds = timeLeft % 60;
            quizTimer.textContent = `${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`;
        }

        // Handle search functionality - REMOVED
        /*
        function handleSearch() {
            const searchTerm = searchInput.value.toLowerCase();
            if (currentCategory && !questionsSection.classList.contains('hidden')) {
                const filteredQuestions = japaneseData[currentCategory].filter(item => {
                    const questionText = item.question.toLowerCase().replace(/\(.*?\)/g, '');
                    const meaningText = item.meaning.toLowerCase();
                    return questionText.includes(searchTerm) || meaningText.includes(searchTerm);
                });
                
                questionsContainer.innerHTML = '';
                if (activeAnswerContainer) {
                    activeAnswerContainer.classList.add('hidden');
                    activeAnswerContainer = null;
                }

                filteredQuestions.forEach(item => {
                    const questionCard = document.createElement('div');
                    questionCard.classList.add('question-card', 'bg-gradient-to-r', 'from-blue-50', 'to-indigo-50', 'dark:from-blue-900', 'dark:to-indigo-900', 'p-5', 'rounded-xl', 'cursor-pointer', 'relative', 'overflow-hidden');
                    
                    const answersContainerForCard = document.createElement('div');
                    answersContainerForCard.classList.add('answers-container-for-card', 'mt-4', 'pt-4', 'border-t', 'border-gray-200', 'dark:border-gray-600', 'hidden');

                    answersContainerForCard.innerHTML = `
                        <h3 class="text-lg font-semibold text-gray-800 dark:text-gray-200 mb-2">Alternatif Jawaban:</h3>
                        ${item.answers.map(answer => `
                            <div class="answer-card-content bg-white dark:bg-gray-700 p-4 rounded-xl border border-gray-200 dark:border-gray-600 flex flex-col items-start mb-2">
                                <div class="flex items-center justify-between w-full mb-2">
                                    <p class="text-lg font-medium text-gray-900 dark:text-gray-100">${createRubyText(answer.text)}</p>
                                    <button class="speak-button bg-indigo-500 hover:bg-indigo-600 text-white px-3 py-1 rounded-md text-sm" data-text="${answer.text}">
                                        <i class="fas fa-volume-up mr-1"></i>Dengar
                                    </button>
                                </div>
                                <p class="text-gray-500 dark:text-gray-400 text-sm"><em>${answer.meaning}</em></p>
                                <p class="text-indigo-600 dark:text-indigo-400 text-sm mt-3">
                                    <i class="fas fa-lightbulb mr-1"></i>Tips: Gunakan ini dengan senyuman untuk tampak percaya diri!
                                </p>
                            </div>
                        `).join('')}
                    `;

                    const isAnswered = answeredQuestions[item.question];
                    questionCard.innerHTML = `
                        <div class="flex justify-between items-start">
                            <div class="flex-1 pr-8">
                                <p class="text-xl font-medium text-indigo-800 dark:text-indigo-200">${createRubyText(item.question)}</p>
                                <p class="text-gray-600 dark:text-gray-300 mt-1"><em>${item.meaning}</em></p>
                            </div>
                            ${isAnswered ? '<i class="fas fa-check-circle text-green-500 text-2xl absolute top-4 right-4"></i>' : ''}
                        </div>
                    `;
                    questionCard.appendChild(answersContainerForCard);
                    
                    questionCard.addEventListener('click', (event) => {
                         if (event.target.closest('.speak-button')) {
                            return;
                        }

                        if (activeAnswerContainer && activeAnswerContainer !== answersContainerForCard) {
                            activeAnswerContainer.classList.add('hidden');
                        }

                        answersContainerForCard.classList.toggle('hidden');
                        activeAnswerContainer = answersContainerForCard.classList.contains('hidden') ? null : answersContainerForCard;

                        if (!answersContainerForCard.classList.contains('hidden')) {
                                if (!answeredQuestions[item.question]) {
                                answeredQuestions[item.question] = true;
                                localStorage.setItem('answeredQuestions', JSON.stringify(answeredQuestions));
                                updateProgress();
                                if (!questionCard.querySelector('.fa-check-circle')) {
                                    const checkmark = document.createElement('i');
                                    checkmark.classList.add('fas', 'fa-check-circle', 'text-green-500', 'text-2xl', 'absolute', 'top-4', 'right-4');
                                    questionCard.appendChild(checkmark);
                                }
                            }
                            questionCard.scrollIntoView({ behavior: 'smooth', block: 'start' });
                        }
                    });

                    answersContainerForCard.querySelectorAll('.speak-button').forEach(button => {
                        button.addEventListener('click', (event) => {
                            event.stopPropagation();
                            speakText(button.dataset.text);
                        });
                    });

                    questionsContainer.appendChild(questionCard);
                });
            } else {
                questionsContainer.innerHTML = '';
            }
        }
        */

        // Update progress
        function updateProgress() {
            if (!currentCategory) {
                progressCount.textContent = 0;
                totalQuestions.textContent = 0;
                progressPercent.textContent = `0%`;
                progressBar.style.width = `0%`;
                return;
            }
            
            const total = japaneseData[currentCategory].length;
            const answered = Object.keys(answeredQuestions).filter(q => japaneseData[currentCategory].some(item => item.question === q)).length;
            
            const percent = total > 0 ? Math.round((answered / total) * 100) : 0;
            
            progressCount.textContent = answered;
            totalQuestions.textContent = total;
            progressPercent.textContent = `${percent}%`;
            progressBar.style.width = `${percent}%`;
        }

        // Toggle dark mode
        function toggleDarkMode() {
            document.body.classList.toggle('dark');
            document.querySelector('.container').classList.toggle('dark:bg-gray-800');
            document.querySelector('.container').classList.toggle('bg-white');
            document.querySelector('h1').classList.toggle('dark:text-indigo-400');
            // Update subtitle as well
            document.querySelector('p.text-lg').classList.toggle('dark:text-gray-300');

            document.querySelectorAll('h2').forEach(h2 => h2.classList.toggle('dark:text-gray-200'));
            // Search input removed, so no need to toggle its dark mode classes
            // document.getElementById('search-input').classList.toggle('dark:border-gray-600');
            // document.getElementById('search-input').classList.toggle('dark:bg-gray-700');
            // document.getElementById('search-input').classList.toggle('dark:text-white');
            document.querySelectorAll('.question-card').forEach(card => {
                card.classList.toggle('dark:from-blue-900');
                card.classList.toggle('dark:to-indigo-900');
                card.querySelectorAll('p').forEach(p => {
                    p.classList.toggle('dark:text-indigo-200');
                    p.classList.toggle('dark:text-gray-300');
                });
                // Also update answers-container-for-card inside question-card
                const answersContainer = card.querySelector('.answers-container-for-card');
                if (answersContainer) {
                    answersContainer.classList.toggle('dark:border-gray-600');
                    answersContainer.querySelectorAll('.answer-card-content').forEach(ansCard => {
                        ansCard.classList.toggle('dark:bg-gray-700');
                        ansCard.classList.toggle('dark:border-gray-600');
                        ansCard.querySelectorAll('p').forEach(p => {
                            p.classList.toggle('dark:text-gray-100');
                            p.classList.toggle('dark:text-gray-300');
                            p.classList.toggle('dark:text-gray-400');
                            p.classList.toggle('dark:text-indigo-400');
                        });
                    });
                }
            });
            // Removed direct selection of .answer-card-content as they are now children of .question-card
            // document.querySelectorAll('.answer-card-content').forEach(card => {
            //     card.classList.toggle('dark:bg-gray-700');
            //     card.classList.toggle('dark:border-gray-600');
            //     card.querySelectorAll('p').forEach(p => {
            //         p.classList.toggle('dark:text-gray-100');
            //         p.classList.toggle('dark:text-gray-300');
            //         p.classList.toggle('dark:text-gray-400');
            //         p.classList.toggle('dark:text-indigo-400');
            //     });
            // });
            document.querySelectorAll('.text-gray-600').forEach(el => el.classList.toggle('dark:text-gray-300')); // For progress text
            
            localStorage.setItem('darkMode', document.body.classList.contains('dark'));
        }

        // Check dark mode preference
        function checkDarkModePreference() {
            if (localStorage.getItem('darkMode') === 'true' || 
                (window.matchMedia && window.matchMedia('(prefers-color-scheme: dark)').matches)) {
                document.body.classList.add('dark');
                // Manually apply dark mode classes to elements that aren't toggled by default
                document.querySelector('.container').classList.add('dark:bg-gray-800');
                document.querySelector('.container').classList.remove('bg-white');
                document.querySelector('h1').classList.add('dark:text-indigo-400');
                // Update subtitle as well
                document.querySelector('p.text-lg').classList.add('dark:text-gray-300');

                document.querySelectorAll('h2').forEach(h2 => h2.classList.add('dark:text-gray-200'));
                // Search input removed
                // document.getElementById('search-input').classList.add('dark:border-gray-600', 'dark:bg-gray-700', 'dark:text-white');
                 document.querySelectorAll('.question-card').forEach(card => {
                    card.classList.add('dark:from-blue-900', 'dark:to-indigo-900');
                    card.querySelectorAll('p').forEach(p => {
                        p.classList.add('dark:text-indigo-200');
                        p.classList.add('dark:text-gray-300');
                    });
                    // Also update answers-container-for-card inside question-card
                    const answersContainer = card.querySelector('.answers-container-for-card');
                    if (answersContainer) {
                        answersContainer.classList.add('dark:border-gray-600');
                        answersContainer.querySelectorAll('.answer-card-content').forEach(ansCard => {
                            ansCard.classList.add('dark:bg-gray-700', 'dark:border-gray-600');
                            ansCard.querySelectorAll('p').forEach(p => {
                                p.classList.add('dark:text-gray-100');
                                p.classList.add('dark:text-gray-300');
                                p.classList.add('dark:text-gray-400');
                                p.classList.add('dark:text-indigo-400');
                            });
                        });
                    }
                });
                // Removed direct selection of .answer-card-content as they are now children of .question-card
                // document.querySelectorAll('.answer-card-content').forEach(card => {
                //     card.classList.add('dark:bg-gray-700', 'dark:border-gray-600');
                //     card.querySelectorAll('p').forEach(p => {
                //         p.classList.add('dark:text-gray-100');
                //         p.classList.add('dark:text-gray-300');
                //         p.classList.add('dark:text-gray-400');
                //         p.classList.add('dark:text-indigo-400');
                //     });
                // });
                document.querySelectorAll('.text-gray-600').forEach(el => el.classList.add('dark:text-gray-300'));
                // Ensure active category button is styled correctly on load if dark mode is default
                if (currentActiveButton) {
                    currentActiveButton.classList.add('active', 'bg-indigo-600');
                    currentActiveButton.classList.remove('bg-indigo-500');
                }
            }
        }

        // Helper function to create ruby text
        function createRubyText(japaneseTextWithFurigana) {
            const regex = /([\u4e00-\u9faf\u3400-\u4dbf\s]+)\(([\u3040-\u309f\u30a0-\u30ff・\s]+)\)/g;
            let result = japaneseTextWithFurigana.replace(regex, (match, kanji, furigana) => {
                // Ensure no extra spaces before or after kanji/furigana within the ruby tag
                return `<ruby>${kanji.trim()}<rt>${furigana.trim()}</rt></ruby>`;
            });
            
            // Handle cases where there are multiple answers separated by '/'
            // This is primarily for the `answer.text` which might contain multiple options
            const parts = result.split(' / ');
            if (parts.length > 1) {
                // Return each part formatted, joined by '<br> / '
                return parts.map(part => `${part}`).join('<br> / ');
            }
            return result;
        }

        // Helper function to speak text
        function speakText(text) {
            // Remove furigana text in parentheses and any HTML tags for clean speech synthesis
            const cleanText = text.replace(/\(.*?\)/g, '').replace(/<[^>]*>/g, '');
            const utterance = new SpeechSynthesisUtterance(cleanText);
            utterance.lang = 'ja-JP'; // Set language to Japanese
            utterance.rate = 0.9; // Adjust speech rate slightly for natural feel
            speechSynthesis.speak(utterance);
        }

        // Helper function to shuffle array
        function shuffleArray(array) {
            const newArray = [...array];
            for (let i = newArray.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [newArray[i], newArray[j]] = [newArray[j], newArray[i]];
            }
            return newArray;
        }

        // Initialize the app when the DOM is fully loaded
        document.addEventListener('DOMContentLoaded', init);
    </script>
</body>
</html>
