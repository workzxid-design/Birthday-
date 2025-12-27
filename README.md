# Birthday-
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>For You 🌙</title>
<link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@300;500;700&display=swap" rel="stylesheet">

<style>
*{margin:0;padding:0;box-sizing:border-box;font-family:'Montserrat',sans-serif}
body{
  height:100vh; overflow:hidden; color:#fff;
  background:radial-gradient(circle at top,#050516,#000);
}

/* stars */
.star{position:absolute;width:2px;height:2px;background:#fff;border-radius:50%;
  opacity:.6;animation:twinkle 4s infinite alternate}

/* constellation canvas */
#sky{position:absolute;inset:0;pointer-events:none}

/* moon */
.moon{position:absolute;top:6%;right:7%;width:110px;height:110px;background:#fff;border-radius:50%;
  box-shadow:0 0 60px rgba(255,255,255,.7);display:flex;align-items:center;justify-content:center;
  color:#000;font-weight:700}

/* lock */
.lock{height:100vh;display:flex;align-items:center;justify-content:center;text-align:center;padding:20px}
.lock h1{font-size:2rem;color:#ffd6e8;text-shadow:0 0 15px #ff6fb7}

/* card */
.card{display:none;position:absolute;inset:0;margin:auto;width:90%;max-width:520px;
  background:rgba(255,255,255,.06);backdrop-filter:blur(18px);border-radius:30px;
  padding:40px;text-align:center;box-shadow:0 0 90px rgba(255,105,180,.45);
  animation:fadeUp 2.4s ease forwards}
h2{font-size:2.6rem;color:#ff6fb7;text-shadow:0 0 20px #ff6fb7}
.message{margin-top:20px;line-height:1.8;color:#ffd6e8;min-height:160px}

/* buttons */
button{margin-top:18px;padding:14px 34px;border:none;border-radius:50px;
  background:#ff6fb7;color:#000;font-weight:700;cursor:pointer;box-shadow:0 0 25px #ff6fb7}

/* secret + photo */
.secret,.secret2,.photo{display:none;margin-top:20px}
.photo img{width:170px;height:170px;border-radius:22px;object-fit:cover;
  box-shadow:0 0 35px rgba(255,105,180,.7)}

/* hearts */
.heart{position:absolute;font-size:22px;animation:burst 2s ease forwards}

/* anims */
@keyframes fadeUp{from{opacity:0;transform:translateY(80px) scale(.9)}
  to{opacity:1;transform:translateY(0) scale(1)}}
@keyframes twinkle{from{opacity:.2}to{opacity:1}}
@keyframes burst{to{transform:translateY(-160px) scale(2);opacity:0}}
</style>
</head>

<body>
<canvas id="sky"></canvas>
<div class="moon" id="moonName"></div>

<div class="lock" id="lock">
  <h1>🌙 Waiting for destiny…<br/>Midnight unlocks everything ✨</h1>
</div>

<div class="card" id="card">
  <h2>Happy Birthday 🎂</h2>
  <div class="message" id="text"></div>

  <button onclick="reveal()">Open My Heart 💖</button>

  <div id="passwordBox">
    <input id="pass" placeholder="Enter secret password"
      style="margin-top:15px;padding:10px;border-radius:20px;border:none"/>
    <button onclick="unlock()">Unlock 🔐</button>
  </div>

  <div class="secret" id="secret"></div>
  <div class="secret2" id="secret2"></div>

  <div class="photo" id="photoBox">
    <img id="photo" />
  </div>

  <div style="margin-top:25px;font-style:italic;color:#ffb6d5">— Kabir ❤️</div>
</div>

<audio id="voice"><source id="voiceSrc" type="audio/mpeg"></audio>

<script>
/* ===== CUSTOMIZE ===== */
const HER_NAME = "HER_NAME";
const BIRTHDAY_DATE = "YYYY-MM-DD"; // e.g. 2025-12-28
const SECRET_PASSWORD = "LOVE";
const PHOTO_URL = "PHOTO_URL";
const VOICE_NOTE_URL = "VOICE_NOTE_URL";
/* ==================== */

moonName.innerText = HER_NAME;
photo.src = PHOTO_URL;
voiceSrc.src = VOICE_NOTE_URL;

/* stars bg */
for(let i=0;i<180;i++){
  const s=document.createElement("div");
  s.className="star";
  s.style.left=Math.random()*100+"vw";
  s.style.top=Math.random()*100+"vh";
  s.style.animationDuration=(2+Math.random()*4)+"s";
  document.body.appendChild(s);
}

/* unlock on date */
function check(){
  const today=new Date().toISOString().slice(0,10);
  if(today===BIRTHDAY_DATE){
    lock.style.display="none";
    card.style.display="block";
    typeMsg();
    drawName();
  }
}
check(); setInterval(check,1000);

/* typewriter */
const msg=`Aaj tumhara din hai…
meri duniya thodi zyada roshan hai 💗

Tum meri aadat ho,
meri dua ho,
meri kahani ho.`;
let i=0;
function typeMsg(){
  if(i<msg.length){
    text.innerHTML+=msg.charAt(i++);
    setTimeout(typeMsg,36);
  }
}

/* constellation draw */
function drawName(){
  const c=sky,ctx=c.getContext("2d");
  c.width=innerWidth; c.height=innerHeight;
  ctx.strokeStyle="#fff"; ctx.lineWidth=1.2;
  const pts=[...HER_NAME].map((_,i)=>({
    x:c.width/2 - HER_NAME.length*18 + i*36,
    y:c.height*0.18 + (i%2?8:-8)
  }));
  let k=0;
  (function draw(){
    if(k<pts.length-1){
      ctx.beginPath();
      ctx.moveTo(pts[k].x,pts[k].y);
      ctx.lineTo(pts[k+1].x,pts[k+1].y);
      ctx.stroke();
      k++; requestAnimationFrame(draw);
    }
  })();
}

/* reveal */
function reveal(){
  photoBox.style.display="block";
  // hearts
  for(let i=0;i<30;i++){
    const h=document.createElement("div");
    h.className="heart"; h.innerHTML="❤️";
    h.style.left=(50+Math.random()*20-10)+"%";
    h.style.bottom="40%"; document.body.appendChild(h);
    setTimeout(()=>h.remove(),2000);
  }
  // voice fade-in
  const a=voice; a.volume=0; a.play();
  let v=0; const fade=setInterval(()=>{v+=.02; a.volume=Math.min(v,1); if(v>=1)clearInterval(fade)},120);
}

/* password + delayed secret */
function unlock(){
  if(pass.value===SECRET_PASSWORD){
    secret.style.display="block";
    secret.innerHTML="This heart opens only for you. Always. 💫";
    passwordBox.style.display="none";
    setTimeout(()=>{
      secret2.style.display="block";
      secret2.innerHTML="Five seconds later… I still choose you. Every time. ♾️";
    },5000);
  }
}
</script>
</body>
</html>
