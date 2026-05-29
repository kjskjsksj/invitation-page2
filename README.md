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
    box-shadow: 0 30px 60px rgba(0,0,0,0.5);
    transition: 0.8s cubic-bezier(0.68, -0.55, 0.27, 1.55);
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
    min-height: 120px;
    line-height: 1.6;
}

#estadoEnvio {
    margin-top: 10px;
    font-size: 14px;
}

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

        <div id="inicio">
            <h2>Para TN</h2>
            <p>No es una invitación común.</p>
            <button class="primary fadeIn" onclick="start(event)">Abrir</button>
        </div>

        <div id="p1" class="hidden">
            <p>Si pudieras elegir una noche sin interrupciones, ¿cómo sería?</p>
            <button onclick="next()" class="fadeIn">Buena conversación, sin prisa</button>
            <button onclick="next()" class="fadeIn">Algo espontáneo, sin plan previo</button>
        </div>

        <div id="p2" class="hidden">
            <p>¿Te animarías a descubrir qué podría pasar si salimos juntos?</p>
            <button onclick="acepta()" class="fadeIn">Sí</button>
            <button onclick="rechazar()" class="fadeIn">No</button>
        </div>

        <div id="p3" class="hidden">
            <p>¿Cómo te gustaría que sea...?</p>
            <button onclick="final()" class="fadeIn">Planeada</button>
            <button onclick="final()" class="fadeIn">Improvisada</button>
        </div>

        <div id="finalBox" class="hidden">
            <div class="finalText" id="textoFinal"></div>

            <form id="formFinal">
                <input type="hidden" name="respuesta" value="Hoy 22:30">

                <button type="submit" id="btnHoy" class="primary">
                    Hoy 22:30
                </button>
            </form>

            <p id="estadoEnvio"></p>

            <button id="btnNo" class="option">No aceptar</button>
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

    if(player){
        player.playVideo();
        player.setVolume(20);
    }
}

function start(e){
    e.stopPropagation();

    document.getElementById("inicio").classList.add("hidden");
    document.getElementById("p1").classList.remove("hidden");
}

function next(){
    document.getElementById("p1").classList.add("hidden");
    document.getElementById("p2").classList.remove("hidden");
}

function acepta(){
    document.getElementById("p2").classList.add("hidden");
    document.getElementById("p3").classList.remove("hidden");

    if (navigator.vibrate) {
        navigator.vibrate(50);
    }
}

const frases = ["¿Seguro?", "Pensalo otra vez 😏", "No tan rápido...", "Error 😅"];
let i = 0;

function rechazar(){
    const btn = document.getElementById("btnNo");
    btn.textContent = frases[i % frases.length];
    i++;
}

function final(){
    document.getElementById("p3").classList.add("hidden");
    document.getElementById("finalBox").classList.remove("hidden");

    escribirFinal();
}

function escribirFinal(){
    const texto = "Entonces... creo que ya sabes a dónde va esto.\n\nMe gustaría invitarte a una noche diferente.\nSin presión, sin guión...\nSolo vos y yo.\n\n¿Aceptás?";
    let i = 0;
    let box = document.getElementById("textoFinal");

    function escribir(){
        if(i < texto.length){
            box.innerHTML += texto[i] === "\n" ? "<br>" : texto[i];
            i++;
            setTimeout(escribir, 40);
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
    },500);
}

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
            headers: { "Accept": "application/json" }
        });

        if (res.ok) {
            estado.textContent = "💌 Perfecto... entonces nos vemos 😉";
            btn.disabled = true;
            btn.textContent = "Confirmado ✔";
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
