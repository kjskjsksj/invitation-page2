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

/* BOTÓN NO */
#btnNo {
    position: relative;
    transition: transform 0.25s ease, background 0.3s, color 0.3s, opacity 0.3s;
}

/* SHAKE SCREEN */
.shake {
    animation: shake 0.4s;
}

@keyframes shake {
    0% { transform: translate(0,0); }
    20% { transform: translate(-8px,4px); }
    40% { transform: translate(8px,-4px); }
    60% { transform: translate(-6px,3px); }
    80% { transform: translate(6px,-3px); }
    100% { transform: translate(0,0); }
}

/* POPUP ERROR 404 */
.popup {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    display: none;
    justify-content: center;
    align-items: center;
    background: rgba(0,0,0,0.6);
    z-index: 9999;
}

.popup-box {
    background: white;
    padding: 30px;
    border-radius: 10px;
    text-align: center;
    animation: pop 0.3s ease;
    font-family: Georgia, serif;
}

@keyframes pop {
    from { transform: scale(0.5); opacity: 0; }
    to { transform: scale(1); opacity: 1; }
}

</style>
</head>

<body>

<div id="player"></div>

<!-- POPUP ERROR -->
<div id="popupError" class="popup">
    <div class="popup-box">
        <h2>ERROR 404</h2>
        <p>Opción no válida</p>
    </div>
</div>

<div class="envelope" id="envelope" onclick="abrirSobre()">
    <div class="flap"></div>

    <div class="card">

        <div id="inicio">
            <h2>Para TN</h2>
            <p>No es una invitación común.</p>
            <button class="primary fadeIn" onclick="start(event)">Abrir</button>
        </div>

        <div id="p1" class="hidden">
            <p>Si pudieras elegir una noche para salir con una persona, ¿cómo sería?</p>
            <button onclick="next()" class="fadeIn">Buena conversación, sin prisa</button>
            <button onclick="next()" class="fadeIn">Algo espontáneo, sin plan previo</button>
        </div>

        <div id="p2" class="hidden">
            <p>Suponiendo que ya sepas quien soy, ¿Te animarías a descubrir qué podría pasar si salimos juntos?</p>
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

            <button id="btnNo" class="option" onclick="rechazar()">No aceptar</button>
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

    if (navigator.vibrate) navigator.vibrate(50);
}

/* =========================
   BOTÓN NO HUYE
   ========================= */

let btnNo = null;
let followActive = false;

document.addEventListener("mousemove", (e) => {
    if (!followActive || !btnNo) return;

    const rect = btnNo.getBoundingClientRect();

    const dx = e.clientX - (rect.left + rect.width / 2);
    const dy = e.clientY - (rect.top + rect.height / 2);

    const distance = Math.sqrt(dx * dx + dy * dy);

    if (distance < 140) {
        const angle = Math.atan2(dy, dx);

        const moveX = Math.cos(angle + Math.PI) * 90;
        const moveY = Math.sin(angle + Math.PI) * 90;

        btnNo.style.transform = `translate(${moveX}px, ${moveY}px)`;
    }
});

/* =========================
   RECHAZO CON ERROR 404
   ========================= */

function rechazar(){
    const btn = document.getElementById("btnNo");
    btnNo = btn;

    followActive = true;

    // popup
    const popup = document.getElementById("popupError");
    popup.style.display = "flex";

    setTimeout(() => {
        popup.style.display = "none";
    }, 1500);

    // shake screen
    document.body.classList.add("shake");

    setTimeout(() => {
        document.body.classList.remove("shake");
    }, 400);

    // feedback botón
    btn.textContent = "ERROR";
    btn.style.background = "#ffebee";
    btn.style.color = "#b71c1c";

    // vibración
    if (navigator.vibrate) navigator.vibrate(100);
}

/* FINAL */
function final(){
    document.getElementById("p3").classList.add("hidden");
    document.getElementById("finalBox").classList.remove("hidden");

    escribirFinal();
}

function escribirFinal(){
    const texto = "Entonces... creo que ya sabes a dónde va esto.\n\nMe gustaría invitarte a una noche diferente.\nSolo vos y yo.\n\n¿Aceptás?";
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
        const btn = document.getElementById("btnNo");
        btn.classList.add("fadeIn");

        btnNo = btn;
        followActive = true;
    },500);
}

/* ENVÍO */
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
