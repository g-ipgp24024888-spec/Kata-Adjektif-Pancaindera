<!DOCTYPE html>
<html lang="ms">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Misi Adjektif: Jom Belajar!</title>
    <style>
        :root {
            --bg-color: #f7f3e8;
            --primary-color: #e67e22;
            --secondary-color: #f39c12;
            --text-color: #2c3e50;
            --card-color: #ffedcc;
            --card-info-color: #fffaf0;
            --correct: #2ecc71;
            --wrong: #e74c3c;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: var(--bg-color);
            color: var(--text-color);
            margin: 0;
            padding: 0;
            display: flex;
            flex-direction: column;
            align-items: center;
            min-height: 100vh;
            text-align: center;
        }

        header {
            width: 100%;
            background-color: var(--primary-color);
            color: white;
            padding: 20px 0;
            font-size: 2em;
            font-weight: bold;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
        }

        #level-indicator {
            margin: 20px 0;
            font-size: 1.2em;
            font-weight: bold;
        }

        .container {
            width: 90%;
            max-width: 800px;
            display: none; /* Hide levels by default */
            flex-direction: column;
            align-items: center;
            background: white;
            padding: 30px;
            border-radius: 15px;
            box-shadow: 0 10px 20px rgba(0,0,0,0.05);
            box-sizing: border-box;
            margin-bottom: 30px;
        }

        .active-level {
            display: flex;
        }

        .btn {
            background-color: var(--primary-color);
            color: white;
            border: none;
            padding: 15px 30px;
            font-size: 1.1em;
            border-radius: 10px;
            cursor: pointer;
            transition: background 0.2s, transform 0.1s;
            margin: 20px 5px 0 5px;
        }

        .btn:hover {
            background-color: var(--secondary-color);
        }

        .btn:active {
            transform: scale(0.98);
        }

        .btn:disabled {
            background-color: #bdc3c7;
            cursor: not-allowed;
        }

        /* --- Level 1: Flashcards (Click to Swap Content - Proven robust) --- */
        #flashcard-display {
            background-color: var(--card-color);
            width: 100%;
            max-width: 400px;
            border-radius: 15px;
            box-shadow: 0 5px 15px rgba(0,0,0,0.1);
            padding: 30px;
            box-sizing: border-box;
            margin: 20px 0;
            cursor: pointer;
            min-height: 200px;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
        }

        #flashcard-adjektif {
            font-size: 3em;
            font-weight: bold;
            color: var(--primary-color);
            margin: 0;
        }

        #flashcard-sense-name {
            font-size: 1.2em;
            font-weight: bold;
            color: var(--text-color);
            margin: 0 0 10px 0;
        }

        #flashcard-sense-info {
            font-size: 1em;
            color: var(--text-color);
            text-align: left;
            margin: 0;
        }
        
        #flashcard-sense-info ul { margin: 5px 0 0 20px; padding: 0; }

        #flashcard-click-hint {
            font-size: 0.9em;
            color: #888;
            margin-top: 15px;
            font-style: italic;
        }

        /* --- Level 2: Memory Game --- */
        #memory-game {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 10px;
            width: 100%;
            max-width: 600px;
            margin-top: 20px;
        }

        .memory-card {
            background-color: var(--card-color);
            height: 90px;
            display: flex;
            justify-content: center;
            align-items: center;
            color: transparent; /* Hide text initially */
            font-size: 1.1em;
            font-weight: bold;
            border-radius: 10px;
            cursor: pointer;
            box-shadow: 0 3px 6px rgba(0,0,0,0.1);
            transition: background 0.2s, color 0.1s;
            user-select: none;
            padding: 5px;
            box-sizing: border-box;
        }

        .memory-card.flipped {
            background-color: white;
            border: 2px solid var(--primary-color);
            color: var(--primary-color);
        }

        .memory-card.matched {
            visibility: hidden;
            pointer-events: none;
        }

        /* --- Level 3: Quiz --- */
        #quiz-zone {
            text-align: left;
            width: 100%;
        }

        .quiz-question {
            font-size: 1.3em;
            margin-bottom: 20px;
            font-weight: bold;
            color: var(--text-color);
        }

        .quiz-option {
            background-color: white;
            border: 2px solid var(--secondary-color);
            color: var(--text-color);
            width: 100%;
            margin-bottom: 12px;
            padding: 15px;
            text-align: left;
            border-radius: 10px;
            cursor: pointer;
            transition: background 0.1s;
            display: block;
            box-sizing: border-box;
            font-size: 1.1em;
        }

        .quiz-option:hover {
            background-color: var(--card-color);
        }

        .quiz-option.correct {
            background-color: var(--correct);
            color: white;
            border-color: var(--correct);
        }

        .quiz-option.wrong {
            background-color: var(--wrong);
            color: white;
            border-color: var(--wrong);
        }

        /* End of level popup */
        #popup-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0,0,0,0.7);
            display: none;
            justify-content: center;
            align-items: center;
            z-index: 1000;
        }

        #popup-content {
            background-color: white;
            padding: 40px;
            border-radius: 15px;
            text-align: center;
            box-shadow: 0 5px 20px rgba(0,0,0,0.3);
        }

        @media (max-width: 500px) {
            #memory-game { grid-template-columns: repeat(2, 1fr); gap: 8px;}
            .memory-card { height: 70px; font-size: 0.9em; }
            #flashcard-adjektif { font-size: 2em; }
        }
    </style>
</head>
<body>

<header>Misi Adjektif</header>
<div id="level-indicator">Jom Belajar Kata Adjektif Pancaindera!</div>

<!-- --- TAHAP 1: Flashcards (Click to Swap - Proved reliable) --- -->
<div id="level1" class="container active-level">
    <h2>Tahap 1: Kad Kata Adjektif</h2>
    <p style="font-size:0.9em; color:#666;">Klik kad untuk lihat info sense, kemudian klik sekali lagi untuk lihat kata adjektif.</p>
    
    <div id="flashcard-display">
        <!-- JS handles what to show here -->
        <p id="flashcard-adjektif"></p>
        
        <div id="flashcard-sense-content" style="display: none;">
            <p id="flashcard-sense-name"></p>
            <div id="flashcard-sense-info"></div>
        </div>

        <p id="flashcard-click-hint">Klik kad untuk lihat info</p>
    </div>

    <div style="display: flex; gap: 10px; margin-top: 10px;">
        <button id="prev-card" class="btn" onclick="changeFlashcard(-1)" disabled>Kembali</button>
        <button id="next-card" class="btn" onclick="changeFlashcard(1)">Seterusnya</button>
    </div>
    
    <button id="goto-level2" class="btn" style="display:none; margin-top: 30px; background-color: #2ecc71;" onclick="nextLevel(2)">Pergi ke Tahap 2 &rarr;</button>
</div>

<!-- --- TAHAP 2: Memory Game --- -->
<div id="level2" class="container">
    <h2>Tahap 2: Permainan Memori</h2>
    <p>Padankan <b>Pancaindera</b> dengan <b>Kata Adjektif</b> yang betul.</p>
    <p style="font-size:0.9em; color:#666;">Tekan dua kad untuk lihat kandungannya. Jika sepasang (cth: Mata & Pandang), kad akan hilang.</p>
    <div id="memory-game">
        <!-- Cards generated here -->
    </div>
    <button id="goto-level3" class="btn" style="display:none; margin-top: 30px; background-color: #2ecc71;" onclick="nextLevel(3)">Pergi ke Tahap 3 &rarr;</button>
</div>

<!-- --- TAHAP 3: Quiz --- -->
<div id="level3" class="container">
    <h2>Tahap 3: Ujian</h2>
    <p>Pilih jawapan yang betul berdasarkan Kata Adjektif Pancaindera yang dipelajari.</p>
    <div id="quiz-zone">
        <p class="quiz-question">Sila jawab soalan berikut:</p>
        <div id="quiz-options">
            <!-- Options generated here -->
        </div>
    </div>
    <button id="end-btn" class="btn" style="display:none; background-color: #2ecc71;" onclick="showPopup()">Tamat Misi</button>
</div>

<!-- End of level Popup -->
<div id="popup-overlay">
    <div id="popup-content">
        <h2>🎉 Tahniah!</h2>
        <p>Anda telah menyelesaikan Misi Belajar Kata Adjektif Pancaindera.</p>
        <button class="btn" onclick="window.location.reload()">Main Semula</button>
    </div>
</div>

<script>
    const levels = document.querySelectorAll('.container');
    const indicator = document.getElementById('level-indicator');

    // Data for levels 1 & 3
    const fullData = [
        { adj: "Cantik", senseName: "Deria Pandang (Mata)", senseInfo: "Kita guna mata untuk melihat:<ul><li>Warna, Bentuk, Saiz</li></ul>" },
        { adj: "Bising", senseName: "Deria Dengar (Telinga)", senseInfo: "Kita guna telinga untuk dengar:<ul><li>Bunyi, Muzik, Suara</li></ul>" },
        { adj: "Harum", senseName: "Deria Bau (Hidung)", senseInfo: "Kita guna hidung untuk hidu:<ul><li>Bau wangi, Bau busuk</li></ul>" },
        { adj: "Pedas", senseName: "Deria Rasa (Lidah)", senseInfo: "Kita guna lidah untuk rasa makanan:<ul><li>Manis, Masam, Pahit</li></ul>" },
        { adj: "Kasar", senseName: "Deria Sentuh (Tangan/Kulit)", senseInfo: "Kita guna tangan/kulit untuk rasa:<ul><li>Lembut, Keras, Licin</li></ul>" }
    ];

    let currentCardIndex = 0;
    let showingSenseInfo = false;

    // --- FLASHCARDS FUNCTIONS (Updated simple click swap) ---
    function updateFlashcardView() {
        const adjP = document.getElementById('flashcard-adjektif');
        const senseContentDiv = document.getElementById('flashcard-sense-content');
        const senseNameP = document.getElementById('flashcard-sense-name');
        const senseInfoDiv = document.getElementById('flashcard-sense-info');
        const hintP = document.getElementById('flashcard-click-hint');

        const card = fullData[currentCardIndex];

        if (showingSenseInfo) {
            adjP.style.display = 'none';
            senseContentDiv.style.display = 'block';
            
            senseNameP.innerText = card.senseName;
            senseInfoDiv.innerHTML = card.senseInfo;
            hintP.innerText = "Klik kad untuk lihat Kata Adjektif";
        } else {
            adjP.style.display = 'block';
            senseContentDiv.style.display = 'none';
            
            adjP.innerText = card.adj;
            hintP.innerText = "Klik kad untuk lihat info sense";
        }

        document.getElementById('prev-card').disabled = (currentCardIndex === 0);
        
        const isLastCard = (currentCardIndex === fullData.length - 1);
        document.getElementById('next-card').style.display = isLastCard ? 'none' : 'block';
        document.getElementById('goto-level2').style.display = isLastCard ? 'block' : 'none';
    }

    document.getElementById('flashcard-display').addEventListener('click', () => {
        showingSenseInfo = !showingSenseInfo;
        updateFlashcardView();
    });

    function changeFlashcard(direction) {
        currentCardIndex += direction;
        showingSenseInfo = false;
        updateFlashcardView();
    }

    // --- MEMORY GAME FUNCTIONS ---
    // Memory Game Data - Simple matching logic
    const memoryPairs = [
        { type: 'sense', text: "Mata", key: "pandang" }, { type: 'word', text: "Cantik", key: "pandang" },
        { type: 'sense', text: "Telinga", key: "dengar" }, { type: 'word', text: "Bising", key: "dengar" },
        { type: 'sense', text: "Hidung", key: "bau" }, { type: 'word', text: "Harum", key: "bau" },
        { type: 'sense', text: "Lidah", key: "rasa" }, { type: 'word', text: "Pedas", key: "rasa" },
        { type: 'sense', text: "Kulit", key: "sentuh" }, { type: 'word', text: "Kasar", key: "sentuh" }
    ];

    let flippedCards = [];
    let matchedPairs = 0;

    function setupMemoryGame() {
        const game = document.getElementById('memory-game');
        game.innerHTML = '';
        flippedCards = [];
        matchedPairs = 0;
        document.getElementById('goto-level3').style.display = 'none';

        // Shuffle
        let cardsData = [...memoryPairs];
        cardsData.sort(() => Math.random() - 0.5);

        cardsData.forEach(cardInfo => {
            const card = document.createElement('div');
            card.className = 'memory-card';
            card.innerText = cardInfo.text;
            card.setAttribute('data-key', cardInfo.key);
            card.onclick = () => flipMemoryCard(card);
            game.appendChild(card);
        });
    }

    function flipMemoryCard(card) {
        if (flippedCards.length < 2 && !card.classList.contains('flipped') && !card.classList.contains('matched')) {
            card.classList.add('flipped');
            flippedCards.push(card);

            if (flippedCards.length === 2) {
                setTimeout(checkMemoryMatch, 1000);
            }
        }
    }

    function checkMemoryMatch() {
        const [card1, card2] = flippedCards;
        if (card1.getAttribute('data-key') === card2.getAttribute('data-key')) {
            card1.classList.add('matched');
            card2.classList.add('matched');
            matchedPairs++;
            if (matchedPairs === memoryPairs.length / 2) {
                document.getElementById('goto-level3').style.display = 'block';
            }
        } else {
            card1.classList.remove('flipped');
            card2.classList.remove('flipped');
        }
        flippedCards = [];
    }

    // --- QUIZ FUNCTIONS ---
    const quizQuestions = [
        { question: "Kita guna Lidah untuk rasa makanan. Pilih Kata Adjektif Rasa:", options: ["Pedas", "Cantik", "Kasar"], correct: 0 },
        { question: "Mata digunakan untuk melihat warna dan saiz. Pilih Kata Adjektif Pandang:", options: ["Bising", "Harum", "Cantik"], correct: 2 },
        { question: "Yang manakah Kata Adjektif Bau?", options: ["Harum", "Bising", "Kasar"], correct: 0 },
        { question: "Telinga digunakan untuk dengar bunyi. Pilih Kata Adjektif Dengar:", options: ["Pedas", "Bising", "Cantik"], correct: 1 },
        { question: "Kita menggunakan tangan dan kulit untuk rasa panas atau lembut. Pilih Kata Adjektif Sentuh:", options: ["Harum", "Bising", "Kasar"], correct: 2 }
    ];

    let currentQuizIndex = 0;

    function setupQuiz() {
        currentQuizIndex = 0;
        document.getElementById('end-btn').style.display = 'none';
        showQuizQuestion();
    }

    function showQuizQuestion() {
        const quizZoneQuiz = document.getElementById('quiz-zone');
        const optionsZone = document.getElementById('quiz-options');
        optionsZone.innerHTML = '';

        if (currentQuizIndex >= quizQuestions.length) {
            quizZoneQuiz.querySelector('.quiz-question').innerText = "Tamat Misi";
            optionsZone.innerHTML = '<p>Anda telah menyelesaikan semua soalan! Klik butang di bawah.</p>';
            document.getElementById('end-btn').style.display = 'block';
            return;
        }

        const q = quizQuestions[currentQuizIndex];
        quizZoneQuiz.querySelector('.quiz-question').innerText = `Soalan ${currentQuizIndex + 1}: ${q.question}`;

        q.options.forEach((optionText, index) => {
            const btn = document.createElement('button');
            btn.className = 'quiz-option';
            btn.innerText = optionText;
            btn.onclick = () => checkQuizAnswer(btn, index);
            optionsZone.appendChild(btn);
        });
    }

    function checkQuizAnswer(selectedBtn, selectedIndex) {
        const q = quizQuestions[currentQuizIndex];
        const allOptions = document.querySelectorAll('.quiz-option');

        if (selectedIndex === q.correct) {
            selectedBtn.classList.add('correct');
        } else {
            selectedBtn.classList.add('wrong');
            allOptions[q.correct].classList.add('correct'); // Show correct
        }

        allOptions.forEach(opt => opt.onclick = null); // Disable other
        currentQuizIndex++;
        setTimeout(showQuizQuestion, 1200);
    }

    // --- LEVEL NAVIGATION FUNCTIONS ---
    function nextLevel(levelNum) {
        levels.forEach(level => level.classList.remove('active-level'));
        document.getElementById(`level${levelNum}`).classList.add('active-level');

        if (levelNum === 2) {
            indicator.innerText = "Tahap 2: Permainan Memori";
            setupMemoryGame();
        } else if (levelNum === 3) {
            indicator.innerText = "Tahap 3: Ujian Adjektif";
            setupQuiz();
        } else {
            indicator.innerText = "Tahap 1: Kad Kata Adjektif";
            currentCardIndex = 0;
            showingSenseInfo = false;
            updateFlashcardView();
        }
    }

    // Popup management
    function showPopup() {
        document.getElementById('popup-overlay').style.display = 'flex';
    }

    // Initialize first level
    nextLevel(1);
</script>

</body>
</html>
