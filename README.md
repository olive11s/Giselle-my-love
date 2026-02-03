# Giselle-my-love
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>Valentine Game 💌</title>
<style>
    html, body {
        margin: 0; padding: 0; overflow: hidden;
        font-family: 'Arial', sans-serif; touch-action: none; background: #ffe6f2;
    }
    #gamePhase, #valentinePhase, #secretPhase, #startOverlay {
        width: 100vw; height: 100vh; position: absolute; top: 0; left: 0;
    }
    #startOverlay {
        background: rgba(255, 230, 242, 0.9); z-index: 100;
        display: flex; flex-direction: column; justify-content: center; align-items: center;
    }
    #gamePhase { background: linear-gradient(to top, #87ceeb, #ffffff); display: none; }
    #basket {
        position: absolute; bottom: 5vh; left: 50%;
        width: 80px; height: 80px; /* Fixed size for reliability */
        transform: translateX(-50%);
        background: url('https://i.imgur.com/NdD1V8A.png') no-repeat center;
        background-size: contain; z-index: 10;
    }
    .tulip {
        position: absolute; width: 40px; height: 40px;
        background: url('https://i.imgur.com/5hK1ZhK.png') no-repeat center;
        background-size: contain;
    }
    #timer, #score {
        position: absolute; top: 2vh; font-size: 20px;
        font-weight: bold; color: #d6336c; z-index: 20;
    }
    #timer { left: 5vw; }
    #score { right: 5vw; }
    #valentinePhase { display: none; background-color: #ffcce6; text-align: center; }
    #valentineText { margin-top: 20vh; font-size: 24px; color: #d6336c; }
    .btn { padding: 15px 30px; font-size: 20px; border-radius: 10px; border: none; cursor: pointer; position: absolute; }
    #yesBtn { background: #28a745; color: white; bottom: 20vh; left: 20%; }
    #noBtn { background: #dc3545; color: white; bottom: 20vh; right: 20%; }
</style>
</head>
<body>

<div id="startOverlay">
    <h1 style="color: #d6336c;">For Giselle ❤️</h1>
    <button onclick="initGame()" style="padding: 20px; font-size: 20px; border-radius: 50px; border: none; background: #d6336c; color: white;">Start Game</button>
</div>

<div id="gamePhase">
    <div id="timer">Time: 15</div>
    <div id="score">Tulips: 0</div>
    <div id="basket"></div>
</div>

<div id="valentinePhase">
    <div id="valentineText"><h1>Will you be my Valentine? 💌</h1></div>
    <button id="yesBtn" class="btn">Yes 💖</button>
    <button id="noBtn" class="btn">No 😢</button>
</div>

<audio id="music" loop>
    <source src="https://www.bensound.com/bensound-music/bensound-romantic.mp3" type="audio/mpeg">
</audio>

<script>
const basket = document.getElementById('basket');
const scoreEl = document.getElementById('score');
const timerEl = document.getElementById('timer');
const music = document.getElementById('music');

let score = 0;
let timeLeft = 15;
let gameActive = false;

function initGame() {
    document.getElementById('startOverlay').style.display = 'none';
    document.getElementById('gamePhase').style.display = 'block';
    music.play();
    gameActive = true;
    startTimer();
    spawnTulips();
}

function startTimer() {
    const timerInterval = setInterval(() => {
        timeLeft--;
        timerEl.textContent = `Time: ${timeLeft}`;
        if (timeLeft <= 0) {
            clearInterval(timerInterval);
            gameActive = false;
            if (score >= 5) { // Lowered requirement for testing
                showValentine();
            } else {
                alert("Collect more tulips for Giselle! Try again.");
                location.reload();
            }
        }
    }, 1000);
}

function spawnTulips() {
    if (!gameActive) return;
    createTulip();
    setTimeout(spawnTulips, 800);
}

function createTulip() {
    const tulip = document.createElement('div');
    tulip.className = 'tulip';
    tulip.style.left = Math.random() * (window.innerWidth - 40) + 'px';
    tulip.style.top = '-50px';
    document.getElementById('gamePhase').appendChild(tulip);

    let pos = -50;
    const fall = setInterval(() => {
        pos += 4;
        tulip.style.top = pos + 'px';
        
        const bRect = basket.getBoundingClientRect();
        const tRect = tulip.getBoundingClientRect();

        if (tRect.bottom >= bRect.top && tRect.right >= bRect.left && tRect.left <= bRect.right && tRect.top <= bRect.bottom) {
            score++;
            scoreEl.textContent = `Tulips: ${score}`;
            tulip.remove();
            clearInterval(fall);
        }

        if (pos > window.innerHeight) {
            tulip.remove();
            clearInterval(fall);
        }
    }, 20);
}

// Mobile Touch Control
document.addEventListener('touchmove', (e) => {
    if (!gameActive) return;
    let touchX = e.touches[0].clientX;
    let newLeft = touchX - (basket.offsetWidth / 2);
    // Keep basket in bounds
    if (newLeft < 0) newLeft = 0;
    if (newLeft > window.innerWidth - basket.offsetWidth) newLeft = window.innerWidth - basket.offsetWidth;
    basket.style.left = newLeft + 'px';
}, { passive: false });

function showValentine() {
    document.getElementById('gamePhase').style.display = 'none';
    document.getElementById('valentinePhase').style.display = 'block';
}

// "No" button escapes
document.getElementById('noBtn').addEventListener('touchstart', function() {
    this.style.left = Math.random() * 70 + '%';
    this.style.top = Math.random() * 70 + '%';
});

document.getElementById('yesBtn').addEventListener('click', () => {
    alert("Yay! Best Valentine's ever! 💖");
});
</script>
</body>
</html>
