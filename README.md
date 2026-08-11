# mike-bible-game
A fun, interactive Bible Games Center where users can test their Bible knowledge through quizzes, challenges, scores, levels, and educational games.
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Bible Games Center</title>

<style>
* {
    box-sizing: border-box;
}

body {
    margin: 0;
    font-family: Arial, sans-serif;
    background: linear-gradient(135deg, #111827, #312e81);
    color: white;
    min-height: 100vh;
}

header {
    text-align: center;
    padding: 30px 15px;
}

header h1 {
    font-size: 32px;
    margin: 0 0 8px;
}

header p {
    color: #d1d5db;
}

.container {
    max-width: 600px;
    margin: auto;
    padding: 15px;
}

.card {
    background: rgba(255,255,255,0.1);
    border-radius: 20px;
    padding: 25px;
    margin-bottom: 20px;
    box-shadow: 0 10px 30px rgba(0,0,0,0.25);
}

.stats {
    display: flex;
    justify-content: space-between;
    gap: 10px;
    margin-bottom: 20px;
}

.stat {
    flex: 1;
    background: rgba(255,255,255,0.1);
    padding: 12px;
    border-radius: 12px;
    text-align: center;
}

.stat strong {
    display: block;
    font-size: 22px;
}

.question {
    font-size: 21px;
    font-weight: bold;
    margin-bottom: 20px;
}

.answer {
    width: 100%;
    padding: 15px;
    margin: 7px 0;
    border: none;
    border-radius: 12px;
    background: white;
    color: #111827;
    font-size: 16px;
    cursor: pointer;
}

.answer:hover {
    background: #e5e7eb;
}

.correct {
    background: #22c55e !important;
    color: white !important;
}

.wrong {
    background: #ef4444 !important;
    color: white !important;
}

button {
    font-family: inherit;
}

#nextBtn, #restartBtn {
    width: 100%;
    padding: 15px;
    border: none;
    border-radius: 12px;
    background: #f59e0b;
    color: white;
    font-size: 17px;
    font-weight: bold;
    cursor: pointer;
    margin-top: 15px;
}

#nextBtn {
    display: none;
}

#gameOver {
    display: none;
    text-align: center;
}

#gameOver h2 {
    font-size: 30px;
}

.home {
    text-align: center;
}

.startBtn {
    padding: 16px 30px;
    border: none;
    border-radius: 12px;
    background: #22c55e;
    color: white;
    font-size: 18px;
    font-weight: bold;
    cursor: pointer;
}

.hidden {
    display: none;
}

footer {
    text-align: center;
    padding: 25px;
    color: #cbd5e1;
    font-size: 14px;
}
</style>
</head>

<body>

<header>
    <h1>📖 Bible Games Center</h1>
    <p>Test your Bible knowledge!</p>
</header>

<div class="container">

    <!-- HOME -->
    <div id="home" class="card home">
        <h2>Welcome! 🙏</h2>
        <p>Answer Bible questions and earn points.</p>
        <p><strong>10 questions • 3 lives</strong></p>
        <button class="startBtn" onclick="startGame()">START GAME</button>
    </div>

    <!-- GAME -->
    <div id="game" class="hidden">

        <div class="stats">
            <div class="stat">
                <span>Score</span>
                <strong id="score">0</strong>
            </div>

            <div class="stat">
                <span>Lives</span>
                <strong id="lives">❤️❤️❤️</strong>
            </div>

            <div class="stat">
                <span>Time</span>
                <strong id="timer">15</strong>
            </div>
        </div>

        <div class="card">
            <div id="questionNumber"></div>
            <br>

            <div class="question" id="question">
                Question
            </div>

            <div id="answers"></div>

            <button id="nextBtn" onclick="nextQuestion()">
                NEXT QUESTION →
            </button>
        </div>

    </div>

    <!-- GAME OVER -->
    <div id="gameOver" class="card">
        <h2>Game Finished! 🎉</h2>

        <p>Your final score:</p>

        <h1 id="finalScore">0</h1>

        <p id="resultMessage"></p>

        <button id="restartBtn" onclick="startGame()">
            PLAY AGAIN
        </button>
    </div>

</div>

<footer>
    © 2026 Bible Games Center
</footer>

<script>

const questions = [

{
    question: "Who built the ark?",
    answers: ["Moses", "Noah", "David", "Peter"],
    correct: "Noah"
},

{
    question: "Who was the first man?",
    answers: ["Adam", "Abraham", "Paul", "John"],
    correct: "Adam"
},

{
    question: "Who defeated Goliath?",
    answers: ["David", "Solomon", "Samuel", "Joseph"],
    correct: "David"
},

{
    question: "How many disciples did Jesus have?",
    answers: ["10", "11", "12", "13"],
    correct: "12"
},

{
    question: "Who was swallowed by a great fish?",
    answers: ["Jonah", "Peter", "Daniel", "Elijah"],
    correct: "Jonah"
},

{
    question: "Where was Jesus born?",
    answers: ["Jerusalem", "Bethlehem", "Nazareth", "Rome"],
    correct: "Bethlehem"
},

{
    question: "Who betrayed Jesus?",
    answers: ["Peter", "Judas Iscariot", "Thomas", "Matthew"],
    correct: "Judas Iscariot"
},

{
    question: "Who received the Ten Commandments?",
    answers: ["Moses", "David", "Paul", "Joshua"],
    correct: "Moses"
},

{
    question: "What was Jesus' first miracle according to John?",
    answers: [
        "Healing a blind man",
        "Walking on water",
        "Turning water into wine",
        "Feeding 5,000 people"
    ],
    correct: "Turning water into wine"
},

{
    question: "Who was known for his great strength?",
    answers: ["Samson", "Solomon", "Isaiah", "Luke"],
    correct: "Samson"
}

];

let currentQuestion = 0;
let score = 0;
let lives = 3;
let time = 15;
let timerInterval;
let answered = false;

function startGame() {

    currentQuestion = 0;
    score = 0;
    lives = 3;

    document.getElementById("home").classList.add("hidden");
    document.getElementById("gameOver").style.display = "none";
    document.getElementById("game").classList.remove("hidden");

    document.getElementById("score").textContent = score;

    showQuestion();
}

function showQuestion() {

    clearInterval(timerInterval);

    answered = false;
    time = 15;

    document.getElementById("timer").textContent = time;

    const q = questions[currentQuestion];

    document.getElementById("questionNumber").textContent =
        "Question " + (currentQuestion + 1) + " of " + questions.length;

    document.getElementById("question").textContent = q.question;

    const answersDiv = document.getElementById("answers");

    answersDiv.innerHTML = "";

    q.answers.forEach(answer => {

        const button = document.createElement("button");

        button.textContent = answer;

        button.className = "answer";

        button.onclick = () => checkAnswer(button, answer);

        answersDiv.appendChild(button);
    });

    document.getElementById("nextBtn").style.display = "none";

    startTimer();
}

function startTimer() {

    timerInterval = setInterval(() => {

        time--;

        document.getElementById("timer").textContent = time;

        if (time <= 0) {

            clearInterval(timerInterval);

            if (!answered) {

                answered = true;

                lives--;

                updateLives();

                showCorrectAnswer();

                document.getElementById("nextBtn").style.display = "block";

                if (lives <= 0) {

                    setTimeout(gameOver, 700);

                }
            }
        }

    }, 1000);
}

function checkAnswer(button, answer) {

    if (answered) return;

    answered = true;

    clearInterval(timerInterval);

    const correct = questions[currentQuestion].correct;

    if (answer === correct) {

        button.classList.add("correct");

        score += 10;

        document.getElementById("score").textContent = score;

    } else {

        button.classList.add("wrong");

        lives--;

        updateLives();

        showCorrectAnswer();
    }

    document.getElementById("nextBtn").style.display = "block";

    if (lives <= 0) {

        setTimeout(gameOver, 700);
    }
}

function showCorrectAnswer() {

    const correct = questions[currentQuestion].correct;

    document.querySelectorAll(".answer").forEach(button => {

        if (button.textContent === correct) {

            button.classList.add("correct");
        }

        button.disabled = true;
    });
}

function updateLives() {

    let hearts = "";

    for (let i = 0; i < lives; i++) {
        hearts += "❤️";
    }

    document.getElementById("lives").textContent =
        hearts || "💔";
}

function nextQuestion() {

    if (lives <= 0) {

        gameOver();

        return;
    }

    currentQuestion++;

    if (currentQuestion >= questions.length) {

        gameOver();

    } else {

        showQuestion();
    }
}

function gameOver() {

    clearInterval(timerInterval);

    document.getElementById("game").classList.add("hidden");

    document.getElementById("gameOver").style.display = "block";

    document.getElementById("finalScore").textContent =
        score + " points";

    let message = "";

    if (score >= 90) {

        message = "Amazing! 🏆 You really know your Bible!";

    } else if (score >= 60) {

        message = "Great job! 👏 Keep learning!";

    } else {

        message = "Good effort! 📖 Keep studying and try again!";
    }

    document.getElementById("resultMessage").textContent = message;
}

</script>

</body>
</html>