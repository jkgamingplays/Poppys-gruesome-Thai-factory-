<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
<title>Gruesome Thai Factory - Mobile</title>

<style>
body {
    margin:0;
    background:black;
    color:white;
    font-family:monospace;
    overflow:hidden;
    touch-action:none;
}

#hud {
    position:absolute;
    top:10px;
    left:10px;
    font-size:14px;
}

#eyes {
    position:fixed;
    top:40%;
    width:100%;
    text-align:center;
    font-size:50px;
    display:none;
}

#controls {
    position:absolute;
    bottom:30px;
    width:100%;
    display:flex;
    justify-content:space-around;
}

.btn {
    width:120px;
    height:120px;
    border-radius:50%;
    background:#222;
    display:flex;
    justify-content:center;
    align-items:center;
    font-size:30px;
}

#door {
    position:absolute;
    top:40%;
    right:20px;
    padding:20px;
    background:#333;
}

#vhs {
    position:absolute;
    bottom:160px;
    left:20px;
    background:#444;
    padding:10px;
}

#tv {
    position:absolute;
    top:30%;
    width:100%;
    text-align:center;
    display:none;
    color:#0f0;
}
</style>
</head>

<body>

<div id="hud">Power: <span id="power">0</span>%</div>

<div id="eyes">👁️ 👁️</div>

<div id="vhs">📼</div>

<div id="tv">"THE ROOTS... THEY MOVE..."</div>

<div id="door">EXIT</div>

<div id="controls">
    <div class="btn" ontouchstart="useHand('l')">L</div>
    <div class="btn" ontouchstart="useHand('r')">R</div>
</div>

<script>
let power = 0;
let monsterDistance = 100;
let hasTape = false;
let chase = false;

function update() {
    document.getElementById("power").innerText = power;
}

function useHand(side) {
    if (!chase) {
        power += 5;
        if (power > 100) power = 100;

        monsterMove();

        if (power === 100) startChase();
        update();
    }
}

// MONSTER
function monsterMove() {
    monsterDistance -= Math.random() * 5;

    if (monsterDistance < 50) showEyes();

    if (monsterDistance <= 0) gameOver();
}

function showEyes() {
    const eyes = document.getElementById("eyes");
    eyes.style.display = "block";
    setTimeout(() => eyes.style.display = "none", 150);
}

// VHS
document.getElementById("vhs").ontouchstart = () => {
    hasTape = true;
    alert("Tape collected");
};

// TV
document.body.ontouchstart = (e) => {
    if (hasTape && e.target.id !== "vhs") {
        const tv = document.getElementById("tv");
        tv.style.display = "block";

        setTimeout(() => tv.style.display = "none", 2000);
    }
};

// CHASE
function startChase() {
    chase = true;
    alert("RUN");

    let interval = setInterval(() => {
        monsterDistance -= 10;
        if (monsterDistance <= 0) {
            clearInterval(interval);
            gameOver();
        }
    }, 500);
}

// EXIT
document.getElementById("door").ontouchstart = () => {
    if (chase) ending();
};

// ENDING
function ending() {
    document.body.innerHTML = "<h1 style='text-align:center;margin-top:40vh;'>YOU ESCAPED...</h1>";
    setTimeout(() => {
        document.body.innerHTML = "<h1 style='text-align:center;margin-top:40vh;color:red;'>...OR DID YOU?</h1>";
    }, 2000);
}

function gameOver() {
    alert("ROOTLASH GOT YOU");
    location.reload();
}
</script>

</body>
</html>
