# Giselle-my-love
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>For Giselle ❤️</title>
<style>
    html, body {
        margin: 0; padding: 0; overflow: hidden;
        font-family: 'Arial', sans-serif; touch-action: none; background: #ffe6f2;
    }
    #gamePhase, #valentinePhase, #finalMessage, #startOverlay {
        width: 100vw; height: 100vh; position: absolute; top: 0; left: 0; display: none;
    }
    #startOverlay {
        background: #ffe6f2; display: flex; flex-direction: column; 
        justify-content: center; align-items: center; z-index: 100;
    }
    #gamePhase { background: linear-gradient(to top, #ff9a9e 0%, #fecfef 100%); }
    #basket {
        position: absolute; bottom: 5vh; left: 50%;
        width: 90px; height: 90px;
        transform: translateX(-50%);
        background: url('https://i.imgur.com/NdD1V8A.png') no-repeat center;
        background-size: contain; z-index: 10;
        font-size: 50px; display: flex; justify-content: center; align-items: center;
    }/* Fallback if image fails */
    #basket:empty::after { content: '🧺'; }

    .tulip {
        position: absolute; width: 50px; height: 50px;
        font-size: 40px; text-align: center; z-index: 5;
    }
    #timer, #score {
        position: absolute; top: 5vh; font-size: 22px;
        font-weight: bold; color: #fff; text-shadow: 1px 1px 5px rgba(0,0,0,0.2); z-index: 20;
    }
    #timer { left: 5vw; }
    #score { right: 5vw; }

    #valentinePhase { background-color: #ffcce6; text-align: center; flex-direction: column; justify-content: center; }
    #finalMessage { 
        background: white; display: none; flex-direction: column; 
        justify-content: center; align-items: center; text-align: center;
        padding: 20px; box-sizing: border-box;
    }
    .message-card {
        background: #fff0f5; padding: 30px; border-radius: 20px;
        box-shadow: 0 10px 30px rgba(0,0,0,0.1); border: 2px solid #ffb3d1;
    }
    .btn { padding: 15px 30px; font-size: 20px; border-radius: 50px; border: none; cursor: pointer; position: absolute; }
    #yesBtn { background: #ff4d94; color: white; bottom: 20vh; left: 20%; box-shadow: 0 5px 15px rgba(255,77,148,0.4); }
    #noBtn { background: #aaa; color: white; bottom: 20vh; right: 20%; }
    
    .falling-emoji {
        position: absolute; top: -50px; z-index: 99;
        animation: fall linear forwards;
    }
    @keyframes fall {
        to { transform: translateY(110vh) rotate(360deg); }
    }
</style>
</head>
<body>

<div id="startOverlay" style="display: flex;">
    <h1 style="color: #ff4d94; font-size: 32px;">Hi Giselle! ❤️</h1>
    <p style="color: #d6336c;">Collect the tulips to unlock a surprise</p>
    <button onclick="initGame()" style="padding: 20px 40px; font-size: 22px; border-radius: 50px; border: none; background: #ff4d94; color: white; margin-top: 20px;">Start Game 🌷</button>
</div>

<div id="gamePhase">
    <div id="timer">Time: 15</div>
    <div id="score">Tulips: 0</div>
    <div id="basket"></div>
</div>

<div id="valentinePhase">
    <h1 style="color: #ff4d94; margin-top: 20vh;">You caught them all! 🌷</h1>
    <p style="font-size: 24px; color: #d6336c;">Now, I have a question...</p>
    <h2 style="font-size: 28px; color: #ff4d94;">Will you be my Valentine? 💌</h2>
    <button id="yesBtn" class="btn">YES! 💖</button>
    <button id="noBtn" class="btn">No 😢</button>
</div>

<div id="finalMessage">
    <div class="message-card">
        <h1 style="color: #ff4d94;">Yay! 💖</h1>
        <p style="font-size: 20px; color: #555; line-height: 1.6;">
            Giselle, you make every day brighter.<br>
            I'm so lucky to have you in my life.<br><br>
            <strong>Happy Valentine's Day, my love!</strong><br>
            💋💋💋
        </p>
        <button onclick="location.reload()" style="background:none; border:none; color:#ff4d94; text-decoration:underline;">Play again?</button>
    </div>
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
    music.play().catch(() => {});
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
            showValentine();
        }
    }, 1000);
}

function spawnTulips() {
    if (!gameActive) return;
    createTulip();
    setTimeout(spawnTulips, 600); // Faster spawning for more tulips!
}

function createTulip() {
    const tulip = document.createElement('div');
    tulip.className = 'tulip';
    tulip.innerHTML = '🌷';
    tulip.style.left = Math.random() * (window.innerWidth - 50) + 'px';
    tulip.style.top = '-50px';
    document.getElementById('gamePhase').appendChild(tulip);

    let pos = -50;
    const fall = setInterval(() => {
        pos += 5;
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

document.addEventListener('touchmove', (e) => {
    if (!gameActive) return;
    let touchX = e.touches[0].clientX;
    let newLeft = touchX - (basket.offsetWidth / 2);
    if (newLeft < 0) newLeft = 0;
    if (newLeft > window.innerWidth - basket.offsetWidth) newLeft = window.innerWidth - basket.offsetWidth;
    basket.style.left = newLeft + 'px';
}, { passive: false });

function showValentine() {
    document.getElementById('gamePhase').style.display = 'none';
    document.getElementById('valentinePhase').style.display = 'flex';
}

// "No" button runs away
document.getElementById('noBtn').addEventListener('touchstart', function(e) {
    e.preventDefault();
    this.style.left = Math.random() * 60 + 20 + '%';
    this.style.top = Math.random() * 60 + 20 + '%';
});

document.getElementById('yesBtn').addEventListener('click', () => {
    document.getElementById('valentinePhase').style.display = 'none';
    document.getElementById('finalMessage').style.display = 'flex';
    celebrate();
});

function celebrate() {
    const emojis = ['🌷', '💖', '✨', '💋', '🌸'];
    for(let i=0; i<50; i++) {
        setTimeout(() => {
            const el = document.createElement('div');
            el.className = 'falling-emoji';
            el.innerHTML = emojis[Math.floor(Math.random() * emojis.length)];
            el.style.left = Math.random() * 100 + 'vw';
            el.style.fontSize = Math.random() * 20 + 20 + 'px';
            el.style.animationDuration = Math.random() * 2 + 2 + 's';
            document.body.appendChild(el);
            setTimeout(() => el.remove(), 4000);
        }, i * 100);
    }
}
</script>
</body>
</html>
