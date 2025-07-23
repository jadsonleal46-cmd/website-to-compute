<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Quiz Educativo - 3º e 4º Ano</title>
  <style>
    body { font-family: 'Comic Sans MS', cursive; text-align: center; background: #f0f8ff; margin: 0; padding: 0; }
    h1 { background: #007bff; color: white; padding: 20px; margin: 0; }
    .screen { display: none; padding: 20px; }
    .active { display: block; }
    .avatar { cursor: pointer; width: 100px; margin: 10px; border: 4px solid transparent; border-radius: 50%; }
    .avatar.selected { border-color: gold; }
    .question { font-size: 20px; margin: 20px 0; }
    .answers button { padding: 10px 20px; margin: 10px; font-size: 18px; }
    .team { margin: 10px; display: inline-block; padding: 10px; border-radius: 12px; background: #eee; cursor: pointer; }
    .team.selected { background: #ffc107; }
    .podium { font-size: 24px; }
  </style>
</head>
<body>
  <h1>Quiz Educativo 🧠🎉</h1>

  <div class="screen active" id="screen-start">
    <h2>Digite seu nome:</h2>
    <input type="text" id="player-name" placeholder="Seu nome">
    <h2>Escolha sua equipe:</h2>
    <div id="teams">
      <div class="team" data-team="Equipe Azul">Equipe Azul 💙</div>
      <div class="team" data-team="Equipe Vermelha">Equipe Vermelha ❤️</div>
      <div class="team" data-team="Equipe Verde">Equipe Verde 💚</div>
      <div class="team" data-team="Equipe Amarela">Equipe Amarela 💛</div>
      <div class="team" data-team="Equipe Roxa">Equipe Roxa 💜</div>
    </div>
    <h2>Escolha seu avatar:</h2>
    <div>
      <img src="https://i.imgur.com/1X1x1x1.png" class="avatar" data-avatar="avatar1">
      <img src="https://i.imgur.com/2X2x2x2.png" class="avatar" data-avatar="avatar2">
      <img src="https://i.imgur.com/3X3x3x3.png" class="avatar" data-avatar="avatar3">
    </div>
    <br>
    <button onclick="startQuiz()">Começar o Quiz</button>
  </div>

  <div class="screen" id="screen-quiz">
    <div id="quiz-box">
      <div id="question-number"></div>
      <div class="question" id="question-text"></div>
      <div class="answers" id="answers"></div>
    </div>
  </div>

  <div class="screen" id="screen-end">
    <h2>Fim do Quiz!</h2>
    <div id="podium" class="podium"></div>
    <button onclick="restartQuiz()">Jogar Novamente</button>
  </div>

  <script>
    const questions = [
      { q: "Quanto é 3 + 4?", a: ["6", "7", "8"], correct: 1 },
      { q: "Qual é o antônimo de 'alto'?", a: ["grande", "baixo", "forte"], correct: 1 },
      { q: "Quanto é 9 - 5?", a: ["3", "4", "5"], correct: 1 },
      { q: "Qual é o plural de 'pão'?", a: ["pães", "pãos", "pãez"], correct: 0 }
    ];

    let current = 0, score = 0, playerName = '', playerTeam = '', teamsScore = {};

    const teamElements = document.querySelectorAll('.team');
    teamElements.forEach(el => el.addEventListener('click', () => {
      teamElements.forEach(t => t.classList.remove('selected'));
      el.classList.add('selected');
      playerTeam = el.dataset.team;
    }));

    const avatarElements = document.querySelectorAll('.avatar');
    avatarElements.forEach(el => el.addEventListener('click', () => {
      avatarElements.forEach(a => a.classList.remove('selected'));
      el.classList.add('selected');
    }));

    function startQuiz() {
      playerName = document.getElementById('player-name').value;
      if (!playerName || !playerTeam) return alert("Preencha seu nome e equipe!");
      document.getElementById('screen-start').classList.remove('active');
      document.getElementById('screen-quiz').classList.add('active');
      score = 0;
      current = 0;
      showQuestion();
    }

    function showQuestion() {
      const q = questions[current];
      document.getElementById('question-number').textContent = `Pergunta ${current + 1} de ${questions.length}`;
      document.getElementById('question-text').textContent = q.q;
      const answersDiv = document.getElementById('answers');
      answersDiv.innerHTML = '';
      q.a.forEach((alt, i) => {
        const btn = document.createElement('button');
        btn.textContent = alt;
        btn.onclick = () => checkAnswer(i);
        answersDiv.appendChild(btn);
      });
    }

    function checkAnswer(index) {
      if (index === questions[current].correct) score++;
      current++;
      if (current < questions.length) {
        showQuestion();
      } else {
        endQuiz();
      }
    }

    function endQuiz() {
      document.getElementById('screen-quiz').classList.remove('active');
      document.getElementById('screen-end').classList.add('active');
      teamsScore[playerTeam] = (teamsScore[playerTeam] || 0) + score;
      const sorted = Object.entries(teamsScore).sort((a,b) => b[1] - a[1]);
      let podiumHTML = '<h3>🏆 Pódio por Equipes:</h3>';
      sorted.forEach(([team, pts], i) => {
        podiumHTML += `<div>${i+1}º - ${team}: ${pts} pontos</div>`;
      });
      document.getElementById('podium').innerHTML = podiumHTML;
    }

    function restartQuiz() {
      document.getElementById('screen-end').classList.remove('active');
      document.getElementById('screen-start').classList.add('active');
    }
  </script>
</body>
</html>

