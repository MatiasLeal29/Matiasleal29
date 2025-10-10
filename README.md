<!DOCTYPE html>


<script>
// --- Mensajes que rotan (edítalos libremente) ---
const messages = [
"Cientista político interesado en tecnología.",
"Desarrollador full‑stack JavaScript (junior).",
"Frontend: React + CSS + JSX.",
"Backend: Node.js + MongoDB.",
"Análisis de datos para decidir mejor.",
"Scrum + Jira: iteraciones claras.",
"Objetivo: construir productos útiles y accesibles."
];


// --- Ajustes de la máquina de escribir ---
const el = document.getElementById('message');
const toggleBtn = document.getElementById('toggle');
const nextBtn = document.getElementById('next');


let i = 0; // índice de mensaje
let p = 0; // índice de carácter
let typing = true; // escribiendo o borrando
let playing = true; // play/pause


const typeSpeed = 32; // ms entre letras al escribir
const eraseSpeed = 18; // ms entre letras al borrar
const holdTime = 1400; // ms a pantalla completa antes de borrar


// Cursor parpadeante
const cursor = document.createElement('span');
cursor.className = 'cursor';
cursor.textContent = '▌';


let holdTimer;


function render(){
const txt = messages[i];
if(typing){
p++;
if(p > txt.length){
// mantener texto completo un rato y luego borrar
typing = false;
holdTimer = setTimeout(()=>{ requestAnimationFrame(loop); }, holdTime);
return;
}
} else {
p--;
if(p < 0){
typing = true; p = 0; i = (i + 1) % messages.length;
}
}
el.textContent = txt.slice(0, p);
el.appendChild(cursor);
setTimeout(()=>{ requestAnimationFrame(loop); }, typing ? typeSpeed : eraseSpeed);
}


function loop(){ if(!playing) return; render(); }


// Controles
toggleBtn.addEventListener('click', ()=>{
playing = !playing;
toggleBtn.textContent = playing ? 'Pausar' : 'Reanudar';
if(playing){
clearTimeout(holdTimer);
requestAnimationFrame(loop);
}
});


nextBtn.addEventListener('click', ()=>{
clearTimeout(holdTimer);
typing = true; p = 0; i = (i + 1) % messages.length;
el.textContent = '';
if(!playing){ playing = true; toggleBtn.textContent = 'Pausar'; }
requestAnimationFrame(loop);
});


// Iniciar
requestAnimationFrame(loop);
</script>
</body>
</html>
