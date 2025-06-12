<!DOCTYPE html>
<html lang="pt-br">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Quem conhece melhor Andrew e Emilli?</title>
  <style>
    body {
      font-family: 'Arial', sans-serif;
      background-color: #1a1a1a;
      color: #f8d1da;
      padding: 20px;
      max-width: 700px;
      margin: auto;
      transition: all 0.5s ease;
    }
    h1, h2, h3 {
      text-align: center;
      color: #ff6699;
    }
    .question {
      margin-bottom: 20px;
    }
    .question h3 {
      margin-bottom: 10px;
    }
    button {
      background-color: #ff6699;
      color: white;
      border: none;
      padding: 10px 15px;
      margin-top: 20px;
      cursor: pointer;
      border-radius: 8px;
    }
    .result {
      font-weight: bold;
      margin-top: 20px;
      font-size: 18px;
      text-align: center;
    }
    #secret-section {
      display: none;
      background-color: #2a2a2a;
      padding: 20px;
      border-radius: 10px;
      margin-top: 30px;
    }
    input[type="text"] {
      padding: 8px;
      width: 100%;
      margin-top: 10px;
      border-radius: 6px;
    }
    .heart {
      position: absolute;
      animation: float 2s ease-out forwards;
      color: #ff6699;
      font-size: 20px;
    }
    @keyframes float {
      0% { transform: translateY(0); opacity: 1; }
      100% { transform: translateY(-100px); opacity: 0; }
    }
    #ranking {
      margin-top: 40px;
      background: #2a2a2a;
      padding: 20px;
      border-radius: 10px;
    }
    #ranking h3 {
      margin-bottom: 10px;
      text-align: center;
    }
    #ranking ul {
      list-style: none;
      padding: 0;
    }
    #ranking li {
      background-color: #3a3a3a;
      padding: 10px;
      margin: 5px 0;
      border-radius: 6px;
    }
    #final-message {
      display: none;
      text-align: center;
      margin-top: 40px;
      font-size: 20px;
      animation: fadeIn 2s ease-in;
    }
    @keyframes fadeIn {
      0% { opacity: 0; transform: scale(0.8); }
      100% { opacity: 1; transform: scale(1); }
    }
  </style>
</head>
<body>
  <h1>Quem nos conhece melhor: Andrew e Emilli?</h1>
  <div id="avatar">
    <img src="avatar.png" alt="Avatar do casal" width="120">
    <p>Olá! Sou o Cupido 💘 e vou acompanhar você nesse quiz!</p>
  </div>

  <form id="quiz-form">
    <div class="question">
      <h3>Seu nome:</h3>
      <input type="text" id="player-name" placeholder="Digite seu nome" required />
    </div>

    <div class="question">
      <h3>1. Onde nos vimos pela primeira vez?</h3>
      <label><input type="radio" name="q1" value="errado"> Escola</label><br />
      <label><input type="radio" name="q1" value="errado"> Amigos/parentes conhecidos</label><br />
      <label><input type="radio" name="q1" value="certo"> Aniversário de amigos(a)</label>
    </div>

    <!-- Adicione mais perguntas seguindo o mesmo padrão -->

    <button type="submit">Enviar Respostas</button>
  </form>

  <div class="result" id="score"></div>

  <div id="ranking">
    <h3>🏆 Ranking dos Amigos</h3>
    <ul id="ranking-list">
      <li>Em desenvolvimento - em breve aqui!</li>
    </ul>
  </div>

  <div id="secret-entry">
    <h3>Tem uma surpresa especial... digite a senha:</h3>
    <input type="text" id="secret-code" placeholder="Digite a senha">
    <button onclick="checkSecret()">Ver Surpresa</button>
  </div>

  <div id="secret-section">
    <h2>💖 Emilli, essa parte é só para você 💖</h2>
    <p>Você é a razão de tudo isso existir. Prepare-se para relembrar nossos melhores momentos juntos.</p>
    <video width="100%" controls>
      <source src="video-dos-momentos.mp4" type="video/mp4">
      Seu navegador não suporta o vídeo.
    </video>
  </div>

  <div id="final-message"></div>

  <script>
    document.getElementById('quiz-form').addEventListener('submit', function (e) {
      e.preventDefault();

      const name = document.getElementById('player-name').value.trim();
      if (!name) {
        alert('Por favor, digite seu nome antes de enviar.');
        return;
      }

      let score = 0;
      for (let i = 1; i <= 7; i++) {
        const resposta = document.querySelector('input[name="q' + i + '"]:checked');
        if (resposta && resposta.value === 'certo') {
          score++;
          launchHeart();
        }
      }

      document.getElementById('score').innerText = `${name}, você acertou ${score} de 7 perguntas!`;

      const finalMsg = document.getElementById('final-message');
      finalMsg.style.display = 'block';

      if (score <= 3) {
        finalMsg.innerText = '😅 Opa! Parece que precisa nos conhecer um pouco melhor!';
      } else if (score <= 5) {
        finalMsg.innerText = '😍 Muito bem! Você conhece bastante nossa história!';
      } else {
        finalMsg.innerText = '💖 UAU! Você realmente conhece cada detalhe nosso!';
      }

      saveToRanking(name, score);
    });

    function launchHeart() {
      const heart = document.createElement('div');
      heart.classList.add('heart');
      heart.style.left = Math.random() * 90 + '%';
      heart.innerText = '❤️';
      document.body.appendChild(heart);
      setTimeout(() => heart.remove(), 2000);
    }

    function checkSecret() {
      const code = document.getElementById('secret-code').value.trim();
      if (code === 'amor2023') {
        document.getElementById('secret-section').style.display = 'block';
      } else {
        alert('Senha incorreta. Tente novamente.');
      }
    }

    function saveToRanking(name, score) {
      let ranking = JSON.parse(localStorage.getItem('rankingQuiz')) || [];

      const index = ranking.findIndex(p => p.name.toLowerCase() === name.toLowerCase());
      if (index !== -1) {
        if (score > ranking[index].score) {
          ranking[index].score = score;
        }
      } else {
        ranking.push({ name, score });
      }

      localStorage.setItem('rankingQuiz', JSON.stringify(ranking));
      updateRankingDisplay();
    }

    function updateRankingDisplay() {
      const rankingList = document.getElementById('ranking-list');
      rankingList.innerHTML = '';

      const ranking = JSON.parse(localStorage.getItem('rankingQuiz')) || [];
      ranking.sort((a, b) => b.score - a.score);

      if (ranking.length === 0) {
        rankingList.innerHTML = '<li>Ninguém no ranking ainda!</li>';
        return;
      }

      ranking.forEach((entry, index) => {
        const li = document.createElement('li');
        li.innerText = `${index + 1}º - ${entry.name} 🎯 ${entry.score}/7`;
        rankingList.appendChild(li);
      });
    }

    window.onload = updateRankingDisplay;
  </script>
</body>
</html>
