<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8" />
<title>Horloge Lettres Interactive</title>
<style>
  body { font-family: Arial, sans-serif; text-align: center; margin-top: 40px; }
  canvas { background: #f0f0f0; border: 1px solid #ccc; cursor: pointer; }
  #instructions { margin-bottom: 20px; font-size: 1.2em; }
  #result { margin-top: 30px; font-size: 1.5em; font-weight: bold; letter-spacing: 5px; }
  #correct { color: green; margin-top: 10px; font-size: 1.2em; }
  #error { color: red; margin-top: 10px; font-size: 1.2em; }
  button { margin-top: 20px; padding: 10px 20px; font-size: 1em; }
</style>
</head>
<body>

<h2>Horloge Lettres Interactive</h2>
<div id="instructions">Match the time given onto the clock.</div>
<canvas id="horloge" width="400" height="400"></canvas>

<div id="result"></div>
<div id="correct"></div>
<div id="error"></div>
<button onclick="resetJeu()">Recommencer</button>

<script>
const canvas = document.getElementById('horloge');
const ctx = canvas.getContext('2d');
const centerX = canvas.width / 2;
const centerY = canvas.height / 2;
const radius = 150;

const lettres = ['R', 'L', 'F', 'E', 'W', 'K', 'E', 'P', '0', 'S', 'B', 'G'];
const motCible = 'REBELS';
let motActuel = '';

function dessinerHorloge() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);

  ctx.beginPath();
  ctx.arc(centerX, centerY, radius, 0, 2 * Math.PI);
  ctx.stroke();

  ctx.font = "24px Arial";
  ctx.textAlign = "center";
  ctx.textBaseline = "middle";
  for(let i = 0; i < 12; i++) {
    let angle = (i * 30 - 90) * Math.PI / 180;
    let x = centerX + Math.cos(angle) * (radius - 30);
    let y = centerY + Math.sin(angle) * (radius - 30);
    ctx.fillText(lettres[i], x, y);
  }
}

dessinerHorloge();

canvas.addEventListener('click', (event) => {
  if (motActuel.length >= motCible.length) return; // plus d’ajout si 6 lettres atteintes

  let rect = canvas.getBoundingClientRect();
  let x = event.clientX - rect.left;
  let y = event.clientY - rect.top;
  let angle = Math.atan2(y - centerY, x - centerX);
  let angleDeg = (angle * 180 / Math.PI + 360 + 90) % 360;
  let index = Math.round(angleDeg / 30) % 12;
  let lettreCliquee = lettres[index];

  motActuel += lettreCliquee;
  document.getElementById('result').innerText = motActuel;
  document.getElementById('correct').innerText = '';
  document.getElementById('error').innerText = '';

  if (motActuel.length === motCible.length) {
    if (motActuel === motCible) {
      document.getElementById('correct').innerText = 'Bravo ! Tu as trouvé le bon mot : REBELS ✅';
    } else {
      document.getElementById('error').innerText = 'Ce n’est pas le bon mot, essaie encore.';
    }
  }
});

function resetJeu() {
  motActuel = '';
  document.getElementById('result').innerText = '';
  document.getElementById('correct').innerText = '';
  document.getElementById('error').innerText = '';
  dessinerHorloge();
}
</script>

</body>
</html>
