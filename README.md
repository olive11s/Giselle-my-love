# Giselle-my-love
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>Valentine Game 💌</title>
<style>
html, body {
    margin: 0;
    padding: 0;
    overflow: hidden;
    font-family: 'Arial', sans-serif;
    touch-action: none;
    background: #ffe6f2;
}
#gamePhase, #valentinePhase, #secretPhase {
    display: none;
    width: 100vw;
    height: 100vh;
    position: relative;
}
#gamePhase { background: linear-gradient(to top, #87ceeb, #ffffff); }
#basket {
    position: absolute;
    bottom: 2vh;
    left: 50%;
    width: 15vw;
    height: auto;
    transform: translateX(-50%);
    background: url('https://i.imgur.com/NdD1V8A.png') no-repeat center;
    background-size: contain;
    transition: transform 0.2s;
}
.tulip {
    position: absolute;
    width: 10vw;
    height: auto;
    background: url('https://i.imgur.com/5hK1ZhK.png') no-repeat center;
    background-size: contain;
}
#timer, #score {
    position: absolute;
    top: 2vh;
    font-size: 4vw;
    font-weight: bold;
    color: #d6336c;
}
#timer { left: 2vw; }
#score { right: 2vw; }
#valentinePhase {
    background: url('https://images.unsplash.com/photo-1522199710521-72d69614c702?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&q=80&w=1080') no-repeat center;
    background-size: cover;
}
#valentineText {
    position: absolute;
    top: 25vh;
    width: 100%;
    font-size: 7vw;
    color: white;
    text-shadow: 2px 2px 5px #000;
}
#yesBtn, #noBtn {
    position: absolute;
    padding: 2vw 4vw;
    font-size: 5vw;
    border: none;
    border-radius: 1vw;
    cursor: pointer;
    transition: transform 0.2s;
}
#yesBtn {
    background-color: #28a745;
    color: white;
    left: 25vw;
    bottom: 30vh;
    transform: scale(1);
    animation: pulse 1.5s infinite;
}
#noBtn {
    background-color: #dc3545;
    color: white;
    right: 25vw;
    bottom: 30vh;
}
@keyframes pulse { 0%{transform:scale(1);}50%{transform:scale(1.05);}100%{transform:scale(1);} }
.balloon {
    position: absolute;
    width: 15vw;
    height: auto;
    background: url('https://i.imgur.com/yA2PqOv.png') no-repeat center;
    background-size: contain;
    pointer-events: none;
}
.confetti, .heart, .flower, .floatingHeart {
    position: absolute;
    width: 7vw;
    height: auto;
    pointer-events: none;
    z-index: 9999;
    background-size: contain;
    background-repeat: no-repeat;
}
#secretHeart {
    position: absolute;
    width: 12vw;
    height: auto;
    bottom: 10vh;
    right: 5vw;
    background: url('https://i.imgur.com/3XoymQI.png') no-repeat center;
    background-size: contain;
    cursor: pointer;
}
#secretMessage {
    display: none;
    font-size: 7vw;
    color: #d6336c;
    position: absolute;
    top: 40vh;
    width: 100%;
    text-align: center;
}
</style>
</head>
<body>

<div id="gamePhase">
    <div id="basket"></div>
    <div id="timer">Time: 15</div>
    <div id="score">Tulips: 0</div>
</div>

<div id="valentinePhase">
    <div id="valentineText">Will you be my Valentine? 💌</div>
    <button id="yesBtn">Yes 💖</button>
    <button id="noBtn">No 😢</button>
</div>

<div id="secretPhase">
    <div id="secretMessage">🎉 Congratulations! You unlocked unlimited kisses! 💋💋💋</div>
</div>

<audio id="music" loop>
    <source src="https://www.bensound.com/bensound-music/bensound-romantic.mp3" type="audio/mpeg">
</audio>
<audio id="popSound">
    <source src="https://freesound.org/data/previews/146/146725_2437358-lq.mp3" type="audio/mpeg">
</audio>

<div id="secretHeart"></div>

<script>
const gamePhase = document.getElementById('gamePhase');
const valentinePhase = document.getElementById('valentinePhase');
const secretPhase = document.getElementById('secretPhase');
const basket = document.getElementById('basket');
const timerEl = document.getElementById('timer');
const scoreEl = document.getElementById('score');
const yesBtn = document.getElementById('yesBtn');
const noBtn = document.getElementById('noBtn');
const music = document.getElementById('music');
const popSound = document.getElementById('popSound');
const secretHeart = document.getElementById('secretHeart');
const secretMessage = document.getElementById('secretMessage');

let score=0, timeLeft=15, tulipInterval, timerInterval, yesScale=1, startX=null;

// Play music
music.play().catch(()=>console.log("Autoplay blocked"));

// Start game
gamePhase.style.display='block';
startGame();

function startGame(){
    tulipInterval=setInterval(createTulipBatch,1000);
    timerInterval=setInterval(()=>{
        timeLeft--;
        timerEl.textContent=`Time: ${timeLeft}`;
        if(timeLeft<=0){
            clearInterval(timerInterval);
            clearInterval(tulipInterval);
            if(score>=30) goToValentinePhase();
            else { alert("Time's up! Try again 💐"); location.reload();}
        }
    },1000);
}

function createTulipBatch(){
    const count=Math.floor(Math.random()*3+2);
    for(let i=0;i<count;i++) createTulip();
}

function createTulip(){
    const tulip=document.createElement('div');
    tulip.classList.add('tulip');
    tulip.style.left=Math.random()*(window.innerWidth*0.9)+'px';
    tulip.style.top='-10vw';
    tulip.style.transform=`rotate(${Math.random()*360}deg)`;
    document.body.appendChild(tulip);
    const speed=Math.random()*3+2;
    const rotateSpeed=Math.random()*5;
    let angle=0;
    let fall=setInterval(()=>{
        let tulipTop=parseFloat(tulip.style.top);
        tulipTop+=speed;
        angle+=rotateSpeed;
        tulip.style.top=tulipTop+'px';
        tulip.style.transform=`rotate(${angle}deg)`;
        const basketRect=basket.getBoundingClientRect();
        const tulipRect=tulip.getBoundingClientRect();
        if(tulipRect.bottom>=basketRect.top &&
           tulipRect.left>=basketRect.left &&
           tulipRect.right<=basketRect.right){
            score++;
            scoreEl.textContent=`Tulips: ${score}`;
            tulip.remove();
            clearInterval(fall);
            popSound.currentTime=0;
            popSound.play();
            basket.style.transform='translateX(-50%) rotate(-10deg)';
            setTimeout(()=>{basket.style.transform='translateX(-50%)';},150);
        }
        if(tulipTop>window.innerHeight){ tulip.remove(); clearInterval(fall);}
    },30);
}

// Touch swipe basket
basket.addEventListener('touchstart',(e)=>{ startX=e.touches[0].clientX; });
basket.addEventListener('touchmove',(e)=>{
    if(startX===null) return;
    let touchX=e.touches[0].clientX;
    let diff=touchX-startX;
    let newLeft=basket.offsetLeft+diff;
    newLeft=Math.max(0, Math.min(window.innerWidth-basket.offsetWidth, newLeft));
    basket.style.left=newLeft+'px';
    startX=touchX;
    createFloatingHeart(newLeft+basket.offsetWidth/2);
});

function createFloatingHeart(x){
    const heart=document.createElement('div');
    heart.classList.add('floatingHeart');
    heart.style.left=x+'px';
    heart.style.top=basket.offsetTop+'px';
    heart.style.background="url('https://i.imgur.com/3XoymQI.png') no-repeat center";
    document.body.appendChild(heart);
    let top=basket.offsetTop;
    const fall=setInterval(()=>{
        top-=2;
        heart.style.top=top+'px';
        if(top<-50){ heart.remove(); clearInterval(fall);}
    },20);
}

// Valentine Phase
function goToValentinePhase(){
    gamePhase.style.display='none';
    valentinePhase.style.display='block';
    spawnBalloons();
}

noBtn.addEventListener('click',()=>{
    const btnWidth=noBtn.offsetWidth, btnHeight=noBtn.offsetHeight;
    const maxX=window.innerWidth-btnWidth-5;
    const maxY=window.innerHeight-btnHeight-5;
    noBtn.style.left=Math.random()*maxX+'px';
    noBtn.style.top=Math.random()*maxY+'px';
    yesScale+=0.1;
    yesBtn.style.transform=`scale(${yesScale})`;
});

yesBtn.addEventListener('click',()=>{
    valentinePhase.style.display='none';
    celebrate();
});

// Secret Heart
let holdTime;
secretHeart.addEventListener('touchstart',()=>{
    holdTime=setTimeout(()=>{
        valentinePhase.style.display='none';
        gamePhase.style.display='none';
        secretPhase.style.display='block';
    },5000);
});
secretHeart.addEventListener('touchend',()=>{ clearTimeout(holdTime); });

// Celebration
function celebrate(){
    secretPhase.style.display='block';
    secretMessage.textContent="💖 You said YES! Happy Valentine's Day! 💖";
    for(let i=0;i<100;i++){
        createFalling('https://i.imgur.com/Mv0u6uG.png','confetti');
        createFalling('https://i.imgur.com/3XoymQI.png','heart');
        createFalling('https://i.imgur.com/5hK1ZhK.png','flower');
    }
}

function createFalling(img,className){
    const el=document.createElement('div');
    el.classList.add(className);
    el.style.left=Math.random()*window.innerWidth+'px';
    el.style.top='-30px';
    el.style.background=`url(${img}) no-repeat center`;
    document.body.appendChild(el);
    animateFall(el);
}

function animateFall(el){
    let top=-30;
    const speed=Math.random()*3+2;
    let rotate=0;
    const rotateSpeed=Math.random()*5;
    const fall=setInterval(()=>{
        top+=speed;
        rotate+=rotateSpeed;
        el.style.top=top+'px';
        el.style.transform=`rotate(${rotate}deg)`;
        if(top>window.innerHeight){ el.remove(); clearInterval(fall);}
    },20);
}

// Balloons
function spawnBalloons(){
    for(let i=0;i<5;i++){
        const b=document.createElement('div');
        b.classList.add('balloon');
        b.style.left=Math.random()*window.innerWidth+'px';
        b.style.top=window.innerHeight+'px';
        valentinePhase.appendChild(b);
        animateBalloon(b);
    }
}
function animateBalloon(el){
    let top=window.innerHeight;
    const speed=Math.random()*1+0.5;
    const sway=Math.random()*2-1;
    const move=setInterval(()=>{
        top-=speed;
        let left=parseFloat(el.style.left)+sway;
        el.style.top=top+'px';
        el.style.left=left+'px';
        if(top<-100){ el.remove(); clearInterval(move);}
    },30);
}
</script>
</body>
</html>
