<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Drakon Birthday System</title>
<style>
body {
  margin:0;
  font-family: monospace;
  background: #0a0a0a;
  color: #00ff99;
  overflow-x: hidden;
  text-align: center;
}

/* Subtle moving stars background */
body::before {
  content:"";
  position: fixed;
  top:0; left:0;
  width:100%; height:100%;
  background: radial-gradient(white, transparent 1px) repeat;
  background-size: 50px 50px;
  opacity:0.05;
  pointer-events:none;
  animation: starsMove 60s linear infinite;
}
@keyframes starsMove { 0%{background-position:0 0;} 100%{background-position:1000px 1000px;} }

/* Center container */
.center {
  position:absolute;
  top:50%; left:50%;
  transform: translate(-50%,-50%);
  width:90%;
  max-width:600px;
}
.hidden { display:none; }

/* Input & Button */
input {
  padding:12px;
  width:80%;
  font-size:18px;
  border:none;
  border-radius:6px;
  text-align:center;
  margin-top:10px;
}
button {
  padding:12px 22px;
  margin-top:12px;
  background:#00ff99;
  border:none;
  border-radius:6px;
  cursor:pointer;
  font-size:16px;
}

/* Card panel */
.card {
  background: rgba(255,255,255,0.05);
  padding:30px;
  border-radius:15px;
  backdrop-filter: blur(10px);
  box-shadow: 0 0 25px #00ff9955;
  color:white;
}

/* Message styling */
#message {
  white-space: pre-line;
  line-height:1.8;
  font-size:18px;
  color:#00ff99;
  opacity:0;
  transition:1s;
  text-shadow:0 0 5px #00ff99,0 0 10px #00ff99;
}

/* Dragon animation */
#dragon {
  font-size:50px;
  margin-bottom:10px;
  animation: dragonPulse 1.5s infinite alternate;
}
@keyframes dragonPulse { 0%{transform:scale(1);opacity:0.7;}100%{transform:scale(1.2);opacity:1;} }

/* Reward */
#reward {
  margin-top:20px;
  font-weight:bold;
  color: gold;
  font-size:18px;
  opacity:0;
  transition:1s;
}
.show {opacity:1;}

/* Sparkles */
.sparkle {
  position: absolute;
  width:4px;
  height:4px;
  background:white;
  border-radius:50%;
  pointer-events:none;
  animation: sparkleMove 1s forwards;
}
@keyframes sparkleMove {
  0% {transform: translate(0,0) scale(1); opacity:1;}
  100% {transform: translate(var(--x), var(--y)) scale(0); opacity:0;}
}

/* System boot text */
#bootText {
  font-size:16px;
  color:#00ff99;
  white-space: pre-line;
  text-align:left;
  margin:0 auto;
  max-width:500px;
}
</style>
</head>
<body>

<!-- Background music -->
<audio id="music" loop>
  <source src="https://www.soundhelix.com/examples/mp3/SoundHelix-Song-1.mp3" type="audio/mpeg">
</audio>

<!-- Boot Screen -->
<div id="boot" class="center">
<pre id="bootText"></pre>
</div>

<!-- Login Screen -->
<div id="login" class="center hidden">
<h2>System Authentication</h2>
<input id="nameInput" placeholder="Enter Codename"><br>
<button onclick="checkLogin()">Unlock</button>
<p id="error"></p>
</div>

<!-- Gift Panel -->
<div id="gift" class="center hidden">
<div class="card">
<div id="dragon">🐉</div>
<h1>Welcome, Drakon</h1>
<p id="message"></p>
<button id="rewardBtn" class="hidden" onclick="showReward()">Accept Reward</button>
<div id="reward">
Wealth: <span id="wealth">0</span><br>
Good Health: <span id="health">0</span><br>
Happiness: <span id="happiness">0</span>
</div>
</div>
</div>

<script>
// Boot sequence
const bootMessage = 
"System Initializing...\nScanning Host...\nSoul Signature Detected...\nAccessing Core...\nWelcome, Drakon.\n";
let b=0;
function boot(){
  if(b<bootMessage.length){
    document.getElementById("bootText").innerText += bootMessage.charAt(b);
    b++;
    setTimeout(boot,35);
  } else {
    setTimeout(()=>{
      document.getElementById("boot").classList.add("hidden");
      document.getElementById("login").classList.remove("hidden");
    },1200);
  }
}
boot();

// Login check
function checkLogin(){
  let name = document.getElementById("nameInput").value.trim().toLowerCase();
  if(name==="drakon"){
    document.getElementById("login").classList.add("hidden");
    document.getElementById("gift").classList.remove("hidden");
    document.getElementById("music").play().catch(()=>{});
    showMessage();
  } else {
    document.getElementById("error").innerText="Identity not recognized ❌";
  }
}

// Show birthday message
const msg=`Today we celebrate someone extraordinary.

Emmanuel • Praise • Sunbola

May your life be filled with joy, success, and happiness.
Your dreams will be realized.
Your future is bright and unstoppable.

Keep conquering, Drakon! 🥂✨`;

function showMessage(){
  const messageEl=document.getElementById("message");
  messageEl.innerText=msg;
  setTimeout(()=>{messageEl.style.opacity=1;},800);
  setTimeout(()=>{document.getElementById("rewardBtn").classList.remove("hidden");},1800);
}
function showReward(){
  let code = prompt("Enter Secret Code to unlock perks:");
  if(code && code.trim().toUpperCase() === "D"){
    const rewardEl = document.getElementById("reward");
    rewardEl.classList.add("show");
    document.getElementById("rewardBtn").disabled = true;

    // Animate numbers
    animateValue("wealth",0,100,1200);
    animateValue("health",0,100,1200);
    animateValue("happiness",0,100,1200);

    // Sparkles
    for(let i=0;i<30;i++){createSparkle();}

  } else {
    alert("Access Denied ❌");
  }
}

// Sparkle effect
function createSparkle(){
  let sparkle=document.createElement("div");
  sparkle.className="sparkle";
  sparkle.style.top=Math.random()*80+"%";
  sparkle.style.left=Math.random()*80+"%";
  sparkle.style.setProperty("--x",(Math.random()*200-100)+"px");
  sparkle.style.setProperty("--y",(Math.random()*-200-50)+"px");
  document.body.appendChild(sparkle);
  setTimeout(()=>{sparkle.remove();},1000);
}
</script>
</body>
</html>
