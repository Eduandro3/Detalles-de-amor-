<!DOCTYPE html><html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Detalles con Amor</title>
  <link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@400;700&display=swap" rel="stylesheet">
  <style>
    body {
      margin: 0;
      font-family: 'Montserrat', sans-serif;
      background: #fff0f5;
      color: #333;
    }
    .container {
      max-width: 600px;
      margin: auto;
      padding: 20px;
      background: white;
      border-radius: 16px;
      box-shadow: 0 8px 16px rgba(0,0,0,0.1);
      margin-top: 40px;
    }
    h1 {
      text-align: center;
      color: #e91e63;
    }
    label {
      display: block;
      margin-top: 15px;
    }
    input, textarea, select {
      width: 100%;
      padding: 10px;
      border-radius: 8px;
      border: 1px solid #ccc;
      margin-top: 5px;
    }
    button {
      background: #e91e63;
      color: white;
      padding: 12px 20px;
      border: none;
      border-radius: 8px;
      margin-top: 20px;
      cursor: pointer;
      font-size: 16px;
    }
    canvas {
      display: none;
      background: #ffe4ec;
      border-radius: 16px;
      margin-top: 30px;
    }
    #descargarBtn, #whatsappBtn {
      display: none;
      margin-top: 20px;
      text-align: center;
    }
    a.boton {
      background: #4caf50;
      color: white;
      padding: 10px 20px;
      border-radius: 8px;
      text-decoration: none;
      margin: 5px;
      display: inline-block;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>Detalles con Amor</h1>
    <label for="nombre">Nombre de tu persona especial</label>
    <input type="text" id="nombre" placeholder="Ej: Sofía"><label for="mensaje">Mensaje personalizado</label>
<textarea id="mensaje" rows="4" placeholder="Escribe algo bonito..."></textarea>

<label for="regalo">Elige una sorpresa</label>
<select id="regalo">
  <option value="rosas">Rosas</option>
  <option value="chocolates">Chocolates</option>
  <option value="carta">Carta romántica</option>
  <option value="cancion">Canción especial</option>
</select>

<button onclick="generarVideoRomantico()">Crear Video Romántico</button>
<canvas id="videoCanvas" width="600" height="400"></canvas>

<div id="descargarBtn">
  <a id="descargarVideo" class="boton" download="detalle_amoroso.webm">Descargar Video</a>
</div>
<div id="whatsappBtn">
  <a id="whatsappLink" class="boton" target="_blank">Compartir por WhatsApp</a>
</div>

<audio id="musica" src="https://cdn.pixabay.com/download/audio/2023/03/06/audio_635a4e2431.mp3?filename=romantic-soul-ambient-142366.mp3" preload="auto"></audio>

  </div>  <script>
    async function generarVideoRomantico() {
      const nombre = document.getElementById('nombre').value;
      const mensaje = document.getElementById('mensaje').value;
      const regalo = document.getElementById('regalo').value;

      const regalosTextos = {
        rosas: 'Un ramo de rosas rojas lleno de cariño.',
        chocolates: 'Una caja de chocolates dulces como tú.',
        carta: 'Una carta llena de palabras que nacen del corazón.',
        cancion: 'Una canción que expresa todo mi amor por ti.'
      };

      const canvas = document.getElementById('videoCanvas');
      const ctx = canvas.getContext('2d');
      canvas.style.display = 'block';

      const musica = document.getElementById('musica');
      musica.currentTime = 0;
      musica.play();

      let frame = 0;
      const duracion = 10 * 30; // 10 segundos a 30fps

      const stream = canvas.captureStream(30);
      const recorder = new MediaRecorder(stream);
      const chunks = [];

      recorder.ondataavailable = e => chunks.push(e.data);
      recorder.onstop = () => {
        const blob = new Blob(chunks, { type: 'video/webm' });
        const url = URL.createObjectURL(blob);

        document.getElementById('descargarVideo').href = url;
        document.getElementById('descargarBtn').style.display = 'block';

        const textoWhatsApp = encodeURIComponent(`Mira el detalle que hice para ti: ${mensaje}`);
        document.getElementById('whatsappLink').href = `https://wa.me/?text=${textoWhatsApp}`;
        document.getElementById('whatsappBtn').style.display = 'block';
      };

      recorder.start();

      const drawFrame = () => {
        ctx.clearRect(0, 0, canvas.width, canvas.height);
        ctx.fillStyle = '#ffe4ec';
        ctx.fillRect(0, 0, canvas.width, canvas.height);

        ctx.fillStyle = `rgba(233,30,99,${Math.abs(Math.sin(frame * 0.1))})`;
        for (let i = 0; i < 5; i++) {
          const x = Math.random() * canvas.width;
          const y = Math.random() * canvas.height;
          ctx.beginPath();
          ctx.arc(x, y, 10, 0, Math.PI * 2);
          ctx.fill();
        }

        ctx.fillStyle = '#e91e63';
        ctx.font = 'bold 24px Montserrat';
        ctx.fillText(`Para: ${nombre}`, 20, 60);

        ctx.fillStyle = '#333';
        ctx.font = '18px Montserrat';
        ctx.fillText(mensaje, 20, 100);

        ctx.fillStyle = '#e91e63';
        ctx.fillText(regalosTextos[regalo], 20, 150);

        frame++;
        if (frame < duracion) {
          requestAnimationFrame(drawFrame);
        } else {
          recorder.stop();
        }
      };
      drawFrame();
    }
  </script></body>
</html>
