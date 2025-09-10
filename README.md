<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Flores Amarillas 💛</title>
  <style>
    * { margin:0; padding:0; box-sizing:border-box; }
    body {
      background: #000;
      overflow: hidden;
      min-height: 100vh;
      font-family: 'Arial', sans-serif;
      color: #fff;
    }

    /* Fondo de estrellas fijas */
    .star {
      position: fixed;
      top: 0; left: 0;
      width: 2px; height: 2px;
      background: white;
      border-radius: 50%;
      opacity: 0.8;
      animation: twinkle 1s infinite alternate;
      z-index: 0;
      box-shadow: 0 0 6px #fff, 0 0 12px #ffff99, 0 0 18px #fffacd;
    }

    @keyframes twinkle {
      0% { opacity: 0.2; transform: scale(0.8); }
      50% { opacity: 1; transform: scale(1.5); }
      100% { opacity: 0.2; transform: scale(0.8); }
    }

    /* Texto principal */
    .titulo {
      position: absolute;
      top: 5%;
      left: 50%;
      transform: translateX(-50%);
      font-size: 4vmin;
      text-align: center;
      color: #ffe066;
      font-family: 'Pacifico', cursive;
      text-shadow: 2px 2px 10px #000, 0 0 18px #fff200, 0 0 4px #ffec80;
      opacity: 0;
      animation: fadeIn 3s forwards;
      z-index: 10;
    }
    @keyframes fadeIn { to {opacity:1;} }

    /* Frases románticas cayendo */
    .frase {
      position: fixed;
      top: -50px;
      font-size: 2.5vmin;
      color: #fffbe0;
      text-shadow: 0 0 10px #ffe066, 0 0 20px #fff;
      opacity: 0.9;
      animation: caerFrase linear forwards;
      pointer-events: none;
      z-index: 15;
    }
    @keyframes caerFrase {
      0% { transform: translateY(-50px) rotate(0deg); opacity:1; }
      100% { transform: translateY(110vh) rotate(360deg); opacity:0; }
    }

    /* Gato */
    .gato {
      position: absolute;
      left: 50%;
      top: 70%;
      transform: translate(-50%, -50%);
      width: 150px;
      max-width: 40vw;
      cursor: pointer;
      z-index: 20;
    }

    /* Estrellas fugaces */
    .shooting-stars { position: fixed; top:0; left:0; width:100%; height:100%; pointer-events:none; z-index:1; }
    .shooting-star {
      position: absolute;
      width: 5px; height: 5px;
      background: #fff;
      border-radius: 50%;
      box-shadow: 0 0 15px 10px rgba(255, 255, 220, 1);
      opacity: 0;
      animation: shootingStar 1.2s linear forwards;
    }
    @keyframes shootingStar {
      0% {opacity:0; transform:translateX(0) translateY(0);}
      10% {opacity:1;}
      100% {opacity:0; transform:translateX(120vw) translateY(50vh);}
    }

    /* Partículas doradas */
    .bg-particle {
      position: fixed;
      left: 0; top: 0;
      pointer-events: none;
      z-index: 2;
      width: 10px; height: 10px;
      border-radius: 50%;
      background: radial-gradient(circle at 60% 40%, #fffbe0 0%, #ffe066 60%, #e6c200 100%);
      opacity: 0.9;
      filter: blur(1px);
      animation: fallBgParticle linear forwards;
    }
    @keyframes fallBgParticle {
      0% {opacity:0.9; transform:translateY(-20px) scale(1);}
      100% {opacity:0; transform:translateY(110vh) scale(1.2);}
    }

    /* Flores amarillas en los costados */
    .flowers-side {
      position: fixed;
      top: 0;
      bottom: 0;
      width: 120px;
      background-image: url('https://i.pinimg.com/originals/3d/3c/b1/3d3cb1090cb648efa2c55d542ae5d49d.gif');
      background-size: cover;
      background-repeat: repeat-y;
      z-index: 5;
      pointer-events: none;
      opacity: 0.9;
      animation: loopScroll 12s linear infinite;
    }
    .flowers-left { left: 0; }
    .flowers-right { right: 0; }

    @keyframes loopScroll {
      from { background-position-y: 0; }
      to { background-position-y: 100%; }
    }

    /* Botón reiniciar */
    .boton {
      position: absolute;
      bottom: 5%;
      left: 50%;
      transform: translateX(-50%);
      background: #ffe066;
      color: #000;
      padding: 10px 20px;
      font-size: 2.5vmin;
      font-weight: bold;
      border-radius: 12px;
      border: none;
      cursor: pointer;
      z-index: 30;
      display: none;
      box-shadow: 0 0 10px #fff200;
    }

    /* Música */
    audio { display:none; }
  </style>
</head>
<body class="not-loaded">

  <h1 class="titulo">Las flores amarillas simbolizan la felicidad.<br> Tú eres la mía ❤️</h1>

  <!-- Flores a los costados -->
  <div class="flowers-side flowers-left"></div>
  <div class="flowers-side flowers-right"></div>

  <!-- Gato central -->
  <img class="gato" id="gato" src="https://i.pinimg.com/originals/9b/8b/16/9b8b16e0435d8cccd334dc65fffb5fe1.gif" alt="Gato tierno">

  <!-- Estrellas fugaces -->
  <div class="shooting-stars" id="shooting-stars"></div>

  <!-- Música -->
  <audio id="bg-music" src="https://github.com/Asprog3rX/Tareas/raw/refs/heads/main/saim.co.za%20-%20Coldplay%20-%20Yellow(Sub%20Espa%C3%B1ol%20Lyrics)%20(256%20KBps)%20(1).mp3" loop></audio>

  <!-- Botón reiniciar -->
  <button class="boton" id="reiniciar">🌻 Volver a empezar la magia</button>

  <script>
    const frases = [
      "Eres mi sol en los días grises",
      "Las flores amarillas me recuerdan a ti",
      "Eres mi pedacito de felicidad",
      "Contigo todo florece",
      "Tú eres mi milagro en primavera"
    ];

    let starInterval, particleInterval, fraseInterval;

    // Crear estrellas fijas de fondo más abundantes y rápidas
    function createStarsBackground(count = 300) {
      for(let i = 0; i < count; i++){
        const star = document.createElement('div');
        star.className = 'star';
        star.style.top = Math.random() * 100 + 'vh';
        star.style.left = Math.random() * 100 + 'vw';
        const size = Math.random() * 3 + 1;
        star.style.width = size + 'px';
        star.style.height = size + 'px';
        star.style.animationDuration = (0.5 + Math.random()*1) + 's';
        document.body.appendChild(star);
      }
    }

    // Crear estrellas fugaces
    function createShootingStar() {
      const star = document.createElement('div');
      star.className = 'shooting-star';
      star.style.top = Math.random() * 60 + '%';
      star.style.left = '-10%';
      star.style.animationDuration = (Math.random() * 0.6 + 0.6) + 's';
      document.getElementById('shooting-stars').appendChild(star);
      setTimeout(() => star.remove(), 1500);
    }

    // Crear partículas doradas más rápidas
    function createBgParticle() {
      const p = document.createElement('div');
      p.className = 'bg-particle';
      p.style.left = (Math.random() * 100) + 'vw';
      const size = 10 + Math.random() * 15;
      p.style.width = size + 'px';
      p.style.height = size + 'px';
      p.style.animationDuration = (2 + Math.random() * 2) + 's';
      document.body.appendChild(p);
      p.addEventListener('animationend', () => p.remove());
    }

    // Crear frases cayendo
    function createFrase() {
      const f = document.createElement('div');
      f.className = 'frase';
      f.textContent = frases[Math.floor(Math.random() * frases.length)];
      f.style.left = Math.random() * 90 + 'vw';
      f.style.animationDuration = (5 + Math.random() * 4) + 's';
      document.body.appendChild(f);
      f.addEventListener('animationend', () => f.remove());
    }

    // Iniciar animaciones
    function iniciarMagia() {
      const audio = document.getElementById('bg-music');
      if (audio.paused) audio.play();

      createStarsBackground(); // estrellas fijas

      starInterval = setInterval(createShootingStar, 500);
      particleInterval = setInterval(createBgParticle, 200);
      fraseInterval = setInterval(createFrase, 4000);

      document.getElementById('reiniciar').style.display = "block";
    }

    // Detener animaciones
    function detenerMagia() {
      clearInterval(starInterval);
      clearInterval(particleInterval);
      clearInterval(fraseInterval);
      document.getElementById('shooting-stars').innerHTML = "";
      document.querySelectorAll('.bg-particle, .frase').forEach(e => e.remove());
    }

    // Click en el gato
    document.getElementById('gato').addEventListener('click', () => {
      iniciarMagia();
      alert("🌻 La magia comienza... escucha la canción 💛");
    });

    // Botón reiniciar
    document.getElementById('reiniciar').addEventListener('click', () => {
      detenerMagia();
      iniciarMagia();
    });
  </script>
</body>
</html>
