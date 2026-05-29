<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<title>Invitación</title>

<style>

body {
    margin: 0;
    font-family: "Georgia", serif;
    background-image: url("https://images.unsplash.com/photo-1506784983877-45594efa4cbe");
    background-size: cover;
    background-position: center;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    position: relative;
}

body::after {
    content: "";
    position: absolute;
    width: 100%;
    height: 100%;
    background: rgba(255, 220, 180, 0.25);
    backdrop-filter: blur(4px);
}

/* SOBRE */
.envelope {
    width: 320px;
    height: 220px;
    background: #8d6e63;
    position: relative;
    cursor: pointer;
    border-radius: 6px;
    z-index: 2;
    transform: rotate(-2deg);
    box-shadow: 0 25px 50px rgba(0,0,0,0.4);
}

.flap {
    position: absolute;
    width: 100%;
    height: 100%;
    background: #a1887f;
    clip-path: polygon(0 0, 100% 0, 50% 60%);
    transform-origin: top;
    transition: 0.6s;
}

.open .flap {
    transform: rotateX(180deg);
}

/* CARTA */
.card {
    position: absolute;
    width: 90%;
    left: 5%;
    top: 30%;
    background: #fffdf8;
    padding: 20px;
    border-radius: 10px;
    opacity: 0;
    transform: translateY(60px);
    transition: 0.8s ease;
    z-index: 3;
}

.open .card {
    opacity: 1;
    transform: translateY(-10px);
}

.hidden { display:none; }

button {
    margin-top:10px;
    padding:10px;
    width:100%;
    border:none;
    border-radius:6px;
    cursor:pointer;
    opacity: 0;
}

.primary { background:#5d4037; color:white; }
.option { background:#efebe9; }

.fadeIn {
    animation: aparecer 0.6s forwards;
}

@keyframes aparecer {
    to { opacity: 1; }
}

.finalText {
    min-height: 80px;
    line-height: 1.5;
}

.escape {
    position: relative;
    z-index: 1;
}

#btnHoy {
    position: relative;
    z-index: 10;
}

/* ESTADO ENVÍO */
#estadoEnvio {
    margin-top: 10px;
    font-size: 14px;
}

/* PLAYER OCULTO */
#player {
    position: absolute;
    width: 0;
    height: 0;
    opacity: 0;
}

</style>
</head>

<body>

<div id="player"></div>

<div class="envelope" id="envelope" onclick="abrirSobre()">
    <div class="flap"></div>

    <div class="card">

        <!-- INICIO -->
        <div id="inicio">
            <h2>Para TN</h2>
            <p>No es una invitación común.<br></p>
            <button class="primary fadeIn" onclick="start(event)">Abrir</button>
        </div>

        <!-- P1 -->
        <div id="p1" class="hidden">
            <p>Si pudieras elegir una noche sin interrupciones, ¿cómo sería?</p>
            <button onclick="next()" class="fadeIn">Buena conversación, sin prisa</button>
            <button onclick="next()" class="fadeIn">Algo espontáneo, sin plan previo</button>
        </div>

        <!-- P2 -->
        <div id="p2" class="hidden">
            <p>Teniendo en cuenta lo anterior y suponiendo ya que ya sepas quien soy, ¿Te gustaría salir conmigo?</p>
            <button onclick="acepta()" class="fadeIn">Sí</button>
            <button onclick="rechazar()" class="fadeIn">No</button>
        </div>

        <!-- P3 -->
        <div id="p3" class="hidden">
            <p>¿Cómo te gustaria que sea...?</p>
            <button onclick="final()" class="fadeIn">Planeada</button>
            <button onclick="final()" class="fadeIn">Improvisada</button>
        </div>

        <!-- FINAL -->
        <div id="finalBox" class="hidden">
            <div class="finalText" id="textoFinal"></div>

            <!-- FORM DEFINITIVO (NO BREAKS) -->
            <form id="formFinal">
                <input type="hidden" name="respuesta" value="Hoy 22:30">

                <button type="submit" id="btnHoy" class="primary">
                    Hoy 22:30
                </button>
            </form>

            <p id="estadoEnvio"></p>

            <button id="btnNo" class="option escape">No aceptar</button>
        </div>

    </div>
</div>

<script src="https://www.youtube.com/iframe_api"></script>

<script>

let player;

function onYouTubeIframeAPIReady() {
    player = new YT.Player('player', {
        videoId: 'v8oqbWrP1QY',
        playerVars: {
            autoplay: 0,
            controls: 0,
            loop: 1,
            playlist: 'v8oqbWrP1QY'
        }
    });
}

function abrirSobre(){
    document.getElementById("envelope").classList.add("open");
}

function start(e){
    e.stopPropagation();

    document.getElementById("inicio").classList.add("hidden");
    document.getElementById("p1").classList.remove("hidden");

    if(player){
        player.playVideo();
        player.setVolume(20);
    }
}

function next(){
    document.getElementById("p1").classList.add("hidden");
    document.getElementById("p2").classList.remove("hidden");
}

function acepta(){
    document.getElementById("p2").classList.add("hidden");
    document.getElementById("p3").classList.remove("hidden");
}

function rechazar(){
    alert("Respuesta inválida.");
}

function final(){
    document.getElementById("p3").classList.add("hidden");
    document.getElementById("finalBox").classList.remove("hidden");

    escribirFinal();
}

function escribirFinal(){
    const texto = "Entonces...";
    let i = 0;
    let box = document.getElementById("textoFinal");

    function escribir(){
        if(i < texto.length){
            box.innerHTML += texto[i];
            i++;
            setTimeout(escribir, 70);
        } else {
            setTimeout(mostrarBotones, 600);
        }
    }

    escribir();
}

function mostrarBotones(){
    document.getElementById("btnHoy").classList.add("fadeIn");

    setTimeout(()=>{
        document.getElementById("btnNo").classList.add("fadeIn");
        moverBoton();
    },500);
}

function moverBoton(){
    const btn = document.getElementById("btnNo");

    let x = 0, y = 0;
    let vx = 0.25, vy = 0.2;

    function anim(){
        x += vx;
        y += vy;

        if(x > 80 || x < -80) vx *= -1;
        if(y > 60 || y < -60) vy *= -1;

        btn.style.transform = `translate(${x}px, ${y}px)`;

        requestAnimationFrame(anim);
    }

    anim();
}

/* =========================
   🔥 ENVÍO FIABLE (FIX FINAL)
   ========================= */

document.getElementById("formFinal").addEventListener("submit", async function(e){
    e.preventDefault();

    const estado = document.getElementById("estadoEnvio");
    const btn = document.getElementById("btnHoy");

    estado.textContent = "Enviando...";

    const formData = new FormData();
    formData.append("respuesta", "Hoy 22:30");

    try {
        const res = await fetch("https://formspree.io/f/meedqzob", {
            method: "POST",
            body: formData,
            headers: {
                "Accept": "application/json"
            }
        });

        if (res.ok) {
            estado.textContent = "✔ Enviado correctamente";
            btn.disabled = true;
            btn.textContent = "Enviado ✔";
        } else {
            estado.textContent = "❌ Error al enviar";
        }

    } catch (error) {
        estado.textContent = "❌ Error de conexión";
    }
});

</script>

</body>
</html>
