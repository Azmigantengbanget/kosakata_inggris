<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Pengetahuan Kosa-kata Inggris</title>
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
            margin-bottom: 10px; /* Mengurangi margin bawah h1 */
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
            transition: background-color 0.3s ease;
        }

        .btn:hover:not([disabled]) {
            background-color: #0056b3;
        }

        .btn.correct {
            background-color: #28a745; /* Hijau */
        }

        .btn.wrong {
            background-color: #dc3545; /* Merah */
        }

        .btn.correct:hover {
            background-color: #1e7e34;
        }

        .btn.wrong:hover {
            background-color: #b02a37;
        }

        .btn:disabled {
            cursor: not-allowed;
            opacity: 0.7;
        }

        .controls {
            display: flex;
            justify-content: center;
            gap: 10px;
        }

        .start-btn, .next-btn, #restart-btn {
            font-weight: bold;
        }

        .hide {
            display: none;
        }

        #result-container h2 {
            margin-bottom: 10px;
        }

        #score {
            font-size: 1.5em;
            font-weight: bold;
            color: #007bff;
            margin-bottom: 20px;
        }
    </style>
</head>
<body>
    <div class="quiz-container">
        <h1>Pengetahuan Kosa-kata Inggris</h1>
        <div id="question-counter" class="question-counter-text hide">0/0</div>
        <div id="question-container" class="hide">
            <div id="question">Kata Bahasa Inggris</div>
            <div id="answer-buttons" class="btn-grid">
                </div>
        </div>
        <div class="controls">
            <button id="start-btn" class="start-btn btn">Mulai</button>
            <button id="next-btn" class="next-btn btn hide">Berikutnya</button>
        </div>
        <div id="result-container" class="hide">
            <h2>Skor Akhir Anda:</h2>
            <p id="score">0 / 0</p>
            <button id="restart-btn" class="btn">Ulangi Kuis</button>
        </div>
    </div>

    <script>
        const startButton = document.getElementById('start-btn');
        const nextButton = document.getElementById('next-btn');
        const restartButton = document.getElementById('restart-btn');
        const questionContainerElement = document.getElementById('question-container');
        const questionElement = document.getElementById('question');
        const answerButtonsElement = document.getElementById('answer-buttons');
        const resultContainerElement = document.getElementById('result-container');
        const scoreElement = document.getElementById('score');
        const questionCounterElement = document.getElementById('question-counter');

        let shuffledQuestions, currentQuestionIndex;
        let score = 0;

        // == MULAI DAFTAR PERTANYAAN ==
        // Anda bisa menambahkan lebih banyak pertanyaan di sini (target 100 soal)
        // Saat ini ada 25 contoh soal.
        const questions = [
            {
                question: 'Happy',
                answers: [
                    { text: 'Sedih', correct: false }, { text: 'Senang', correct: true },
                    { text: 'Marah', correct: false }, { text: 'Lapar', correct: false }
                ]
            },
            {
                question: 'Big',
                answers: [
                    { text: 'Besar', correct: true }, { text: 'Kecil', correct: false },
                    { text: 'Tinggi', correct: false }, { text: 'Panjang', correct: false }
                ]
            },
            {
                question: 'Water',
                answers: [
                    { text: 'Api', correct: false }, { text: 'Tanah', correct: false },
                    { text: 'Udara', correct: false }, { text: 'Air', correct: true }
                ]
            },
            {
                question: 'Book',
                answers: [
                    { text: 'Pensil', correct: false }, { text: 'Buku', correct: true },
                    { text: 'Meja', correct: false }, { text: 'Kertas', correct: false }
                ]
            },
            {
                question: 'Run',
                answers: [
                    { text: 'Jalan', correct: false }, { text: 'Lompat', correct: false },
                    { text: 'Lari', correct: true }, { text: 'Duduk', correct: false }
                ]
            },
            {
                question: 'Quick',
                answers: [
                    { text: 'Cepat', correct: true }, { text: 'Lambat', correct: false },
                    { text: 'Kuat', correct: false }, { text: 'Lemah', correct: false }
                ]
            },
            {
                question: 'Beautiful',
                answers: [
                    { text: 'Jelek', correct: false }, { text: 'Indah', correct: true },
                    { text: 'Kotor', correct: false }, { text: 'Bersih', correct: false }
                ]
            },
            {
                question: 'House',
                answers: [
                    { text: 'Mobil', correct: false }, { text: 'Sekolah', correct: false },
                    { text: 'Rumah', correct: true }, { text: 'Taman', correct: false }
                ]
            },
            {
                question: 'Eat',
                answers: [
                    { text: 'Minum', correct: false }, { text: 'Tidur', correct: false },
                    { text: 'Makan', correct: true }, { text: 'Belajar', correct: false }
                ]
            },
            {
                question: 'Small',
                answers: [
                    { text: 'Besar', correct: false }, { text: 'Kecil', correct: true },
                    { text: 'Luas', correct: false }, { text: 'Sempit', correct: false }
                ]
            },
            {
                question: 'Clever',
                answers: [
                    { text: 'Bodoh', correct: false }, { text: 'Cerdas', correct: true },
                    { text: 'Lambat', correct: false }, { text: 'Malas', correct: false }
                ]
            },
            {
                question: 'Brave',
                answers: [
                    { text: 'Takut', correct: false }, { text: 'Lemah', correct: false },
                    { text: 'Berani', correct: true }, { text: 'Gugup', correct: false }
                ]
            },
            {
                question: 'Kind',
                answers: [
                    { text: 'Baik hati', correct: true }, { text: 'Jahat', correct: false },
                    { text: 'Kasar', correct: false }, { text: 'Sombong', correct: false }
                ]
            },
            {
                question: 'Strong',
                answers: [
                    { text: 'Lemah', correct: false }, { text: 'Kuat', correct: true },
                    { text: 'Kecil', correct: false }, { text: 'Rapuh', correct: false }
                ]
            },
            {
                question: 'Quiet',
                answers: [
                    { text: 'Berisik', correct: false }, { text: 'Ramai', correct: false },
                    { text: 'Tenang', correct: true }, { text: 'Keras', correct: false }
                ]
            },
            {
                question: 'Fast',
                answers: [
                    { text: 'Cepat', correct: true }, { text: 'Lambat', correct: false },
                    { text: 'Pelan', correct: false }, { text: 'Diam', correct: false }
                ]
            },
            {
                question: 'Honest',
                answers: [
                    { text: 'Bohong', correct: false }, { text: 'Jujur', correct: true },
                    { text: 'Curang', correct: false }, { text: 'Menipu', correct: false }
                ]
            },
            {
                question: 'Polite',
                answers: [
                    { text: 'Kasar', correct: false }, { text: 'Angkuh', correct: false },
                    { text: 'Sopan', correct: true }, { text: 'Sombong', correct: false }
                ]
            },
            {
                question: 'Famous',
                answers: [
                    { text: 'Tidak dikenal', correct: false }, { text: 'Terkenal', correct: true },
                    { text: 'Biasa', correct: false }, { text: 'Asing', correct: false }
                ]
            },
            {
                question: 'Difficult',
                answers: [
                    { text: 'Mudah', correct: false }, { text: 'Gampang', correct: false },
                    { text: 'Sulit', correct: true }, { text: 'Ringan', correct: false }
                ]
            },
            {
                question: 'Easy',
                answers: [
                    { text: 'Sulit', correct: false }, { text: 'Mudah', correct: true },
                    { text: 'Berat', correct: false }, { text: 'Rumit', correct: false }
                ]
            },
            {
                question: 'Early',
                answers: [
                    { text: 'Telat', correct: false }, { text: 'Siang', correct: false },
                    { text: 'Awal', correct: true }, { text: 'Akhir', correct: false }
                ]
            },
            {
                question: 'Late',
                answers: [
                    { text: 'Awal', correct: false }, { text: 'Telat', correct: true },
                    { text: 'Cepat', correct: false }, { text: 'Pagi', correct: false }
                ]
            },
            {
                question: 'Interesting',
                answers: [
                    { text: 'Membosankan', correct: false }, { text: 'Menarik', correct: true },
                    { text: 'Biasa', correct: false }, { text: 'Datar', correct: false }
                ]
            },
            {
                question: 'Boring',
                answers: [
                    { text: 'Menarik', correct: false }, { text: 'Seru', correct: false },
                    { text: 'Membosankan', correct: true }, { text: 'Asyik', correct: false }
                ]
            }
            // == TAMBAHKAN 75 SOAL LAGI DI SINI UNTUK MENCAPAI 100 ==
            // Contoh format:
            // {
            //     question: 'NextWord',
            //     answers: [
            //         { text: 'PilihanSalah1', correct: false }, { text: 'JawabanBenar', correct: true },
            //         { text: 'PilihanSalah2', correct: false }, { text: 'PilihanSalah3', correct: false }
            //     ]
            // },
        ];
        // == AKHIR DAFTAR PERTANYAAN ==

        startButton.addEventListener('click', startGame);
        nextButton.addEventListener('click', () => {
            currentQuestionIndex++;
            setNextQuestion();
        });
        restartButton.addEventListener('click', startGame);

        function startGame() {
            startButton.classList.add('hide');
            resultContainerElement.classList.add('hide');
            questionContainerElement.classList.remove('hide');
            questionCounterElement.classList.remove('hide'); // Tampilkan counter
            shuffledQuestions = questions.sort(() => Math.random() - 0.5);
            currentQuestionIndex = 0;
            score = 0;
            setNextQuestion();
        }

        function setNextQuestion() {
            resetState();
            if (currentQuestionIndex < shuffledQuestions.length) {
                questionCounterElement.innerText = `${currentQuestionIndex + 1} / ${shuffledQuestions.length}`;
                showQuestion(shuffledQuestions[currentQuestionIndex]);
            } else {
                showResults();
            }
        }

        function showQuestion(questionData) {
            questionElement.innerText = questionData.question;
            const shuffledAnswers = questionData.answers.sort(() => Math.random() - 0.5);

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
            clearStatusClass(document.body);
            nextButton.classList.add('hide');
            nextButton.innerText = 'Berikutnya';
            while (answerButtonsElement.firstChild) {
                answerButtonsElement.removeChild(answerButtonsElement.firstChild);
            }
        }

        function selectAnswer(e) {
            const selectedButton = e.target;
            const correct = selectedButton.dataset.correct === 'true';

            if (correct) {
                score++;
            }

            Array.from(answerButtonsElement.children).forEach(button => {
                setStatusClass(button, button.dataset.correct === 'true');
                button.disabled = true;
            });

            if (shuffledQuestions.length > currentQuestionIndex + 1) {
                nextButton.classList.remove('hide');
            } else {
                nextButton.innerText = 'Lihat Hasil';
                nextButton.classList.remove('hide');
            }
        }

        function setStatusClass(element, correct) {
            clearStatusClass(element);
            if (correct) {
                element.classList.add('correct');
            } else {
                element.classList.add('wrong');
            }
        }

        function clearStatusClass(element) {
            element.classList.remove('correct');
            element.classList.remove('wrong');
        }

        function showResults() {
            questionContainerElement.classList.add('hide');
            questionCounterElement.classList.add('hide'); // Sembunyikan counter saat hasil ditampilkan
            nextButton.classList.add('hide');
            resultContainerElement.classList.remove('hide');
            scoreElement.innerText = `${score} / ${questions.length}`;
        }
    </script>
</body>
</html>
