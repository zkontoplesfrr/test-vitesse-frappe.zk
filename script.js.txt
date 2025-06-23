const textes = [
  "Le renard brun rapide saute par-dessus le chien paresseux.",
  "Apprendre à taper vite est une compétence utile pour tous.",
  "Les développeurs passent beaucoup de temps à coder rapidement."
];

const textToType = document.getElementById("text-to-type");
const input = document.getElementById("input");
const timer = document.getElementById("timer");
const result = document.getElementById("result");
const startBtn = document.getElementById("start-btn");

let countdown;
let startTime;
let selectedText = "";

function démarrerTest() {
  input.value = "";
  result.innerHTML = "";
  input.disabled = false;
  input.focus();
  startBtn.disabled = true;
  timeLeft = 30;
  selectedText = textes[Math.floor(Math.random() * textes.length)];
  textToType.textContent = selectedText;
  timer.textContent = "Temps : 30s";

  startTime = new Date().getTime();

  countdown = setInterval(() => {
    timeLeft--;
    timer.textContent = `Temps : ${timeLeft}s`;

    if (timeLeft <= 0) {
      terminerTest();
    }
  }, 1000);
}

function terminerTest() {
  clearInterval(countdown);
  input.disabled = true;
  startBtn.disabled = false;

  const typedText = input.value.trim();
  const originalWords = selectedText.trim().split(" ");
  const typedWords = typedText.split(" ");
  let correctWords = 0;

  for (let i = 0; i < typedWords.length; i++) {
    if (typedWords[i] === originalWords[i]) {
      correctWords++;
    }
  }

  const elapsedTime = (new Date().getTime() - startTime) / 60000; // en minutes
  const wpm = Math.round(typedWords.length / elapsedTime);
  const accuracy = Math.round((correctWords / originalWords.length) * 100);

  result.innerHTML = `
    <p><strong>Vitesse :</strong> ${wpm} mots/min</p>
    <p><strong>Précision :</strong> ${accuracy}%</p>
    <p><strong>Mots corrects :</strong> ${correctWords} / ${originalWords.length}</p>
  `;
}

startBtn.addEventListener("click", démarrerTest);
