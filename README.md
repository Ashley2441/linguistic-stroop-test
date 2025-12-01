# linguistic-stroop-test
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Linguistic Stroop Test</title>
<style>
  body {
    font-family: 'Segoe UI', sans-serif;
    background: linear-gradient(135deg, #a0c4ff, #b9fbc0);
    height: 100vh;
    margin: 0;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .container {
    background: white;
    border-radius: 20px;
    box-shadow: 0 6px 30px rgba(0,0,0,0.1);
    padding: 30px;
    width: 90%;
    max-width: 600px;
    text-align: center;
    transition: 0.3s;
  }
  h1 {
    color: #222;
  }
  button {
    background-color: #0077ff;
    color: white;
    border: none;
    border-radius: 10px;
    padding: 12px 18px;
    margin: 6px;
    font-size: 16px;
    cursor: pointer;
    transition: 0.2s;
  }
  button:hover {
    background-color: #005fcc;
  }
  .hidden {
    display: none;
  }
  .question {
    font-size: 36px;
    font-weight: bold;
    margin: 25px 0;
    color: #444;
  }
  .answers button {
    background: linear-gradient(135deg, #ff9a9e, #fad0c4);
    color: #222;
    font-weight: 600;
  }
  .answers button:hover {
    background: linear-gradient(135deg, #fbc2eb, #a6c1ee);
  }
  .result {
    font-size: 20px;
    color: #333;
  }
</style>
</head>
<body>
<div class="container">
  <div id="language-screen">
    <h1>Linguistic Stroop Test</h1>
    <p>Choose your language:</p>
    <button onclick="startTest('en')">English</button>
    <button onclick="startTest('es')">Español</button>
  </div>

  <div id="test-screen" class="hidden">
    <div class="question" id="question-text"></div>
    <div class="answers" id="answer-buttons"></div>
    <div id="progress"></div>
  </div>

  <div id="result-screen" class="hidden">
    <h2>Test Complete!</h2>
    <p class="result" id="result-text"></p>
    <button onclick="restart()">Try Again</button>
  </div>
</div>

<script>
  const englishNumbers = [
    "one","two","three","four","five","six","seven","eight",
    "nine","ten","eleven","twelve","thirteen","fourteen","fifteen","sixteen"
  ];
  const spanishNumbers = [
    "uno","dos","tres","cuatro","cinco","seis","siete","ocho",
    "nueve","diez","once","doce","trece","catorce","quince","dieciséis"
  ];
  const englishNeutrals = ["chair","book","table","green","apple","cat","dog","hello"];
  const spanishNeutrals = ["silla","libro","mesa","verde","manzana","gato","perro","hola"];

  let language = 'en';
  let questions = [];
  let currentIndex = 0;
  let correctCount = 0;
  let totalTime = 0;
  let startTime = 0;

  function startTest(lang) {
    language = lang;
    document.getElementById("language-screen").classList.add("hidden");
    document.getElementById("test-screen").classList.remove("hidden");
    generateQuestions();
    currentIndex = 0;
    correctCount = 0;
    totalTime = 0;
    showQuestion();
  }

  function generateQuestions() {
    const numbers = language === 'en' ? englishNumbers : spanishNumbers;
    const neutrals = language === 'en' ? englishNeutrals : spanishNeutrals;
    const all = [
      ...numbers.map(word => ({word, type:"number"})),
      ...neutrals.map(word => ({word, type:"neutral"}))
    ];
    // Shuffle array
    questions = all.sort(() => Math.random() - 0.5);
  }

  function showQuestion() {
    if (currentIndex >= questions.length) {
      showResults();
      return;
    }
    const q = questions[currentIndex];
    document.getElementById("question-text").textContent = q.word.toUpperCase();
    const answersDiv = document.getElementById("answer-buttons");
    answersDiv.innerHTML = '';
    for (let i = 1; i <= 8; i++) {
      const btn = document.createElement("button");
      btn.textContent = i;
      btn.onclick = () => checkAnswer(i);
      answersDiv.appendChild(btn);
    }
    document.getElementById("progress").textContent =
      `Question ${currentIndex+1} of ${questions.length}`;
    startTime = performance.now();
  }

  function checkAnswer(selected) {
    const q = questions[currentIndex];
    const correct = q.word.length;
    const timeTaken = (performance.now() - startTime) / 1000;
    totalTime += timeTaken;
    if (selected === correct) correctCount++;
    currentIndex++;
    showQuestion();
  }

  function showResults() {
    document.getElementById("test-screen").classList.add("hidden");
    document.getElementById("result-screen").classList.remove("hidden");
    const avgTime = (totalTime / questions.length).toFixed(2);
    document.getElementById("result-text").textContent =
      `You got ${correctCount} out of ${questions.length} correct.
       Total time: ${totalTime.toFixed(2)}s
       Average time per question: ${avgTime}s`;
  }

  function restart() {
    document.getElementById("result-screen").classList.add("hidden");
    document.getElementById("language-screen").classList.remove("hidden");
  }
</script>
</body>
</html>
