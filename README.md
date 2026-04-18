<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no"/>
<title>Negocios Digitales Pro</title>
<meta name="theme-color" content="#0a0a0f"/>
<meta name="apple-mobile-web-app-capable" content="yes"/>
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent"/>
<link rel="manifest" href="manifest.json"/>
<link rel="apple-touch-icon" href="https://i.ibb.co/3S4S4Xp/icon-192.png"/>
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@700;900&family=DM+Sans:wght@400;500;700&display=swap" rel="stylesheet"/>
<style>
:root{--bg:#0a0a0f;--bg-panel:rgba(255,255,255,0.04);--border:rgba(255,255,255,0.06);--text:#e8e0f0;--text-muted:#8a7aa0;--primary:#a070e0;--primary-dim:rgba(160,112,224,0.15);--success:#47E8A0;--accent:#f0c040;}
*{box-sizing:border-box;margin:0;padding:0;-webkit-tap-highlight-color:transparent;}
body{font-family:'DM Sans',sans-serif;background:var(--bg);color:var(--text);min-height:100vh;overflow-x:hidden;overscroll-behavior-y:contain;}
.anim{animation:fadeUp 0.3s ease out both;}
.card{transition:transform 0.15s ease,background 0.15s ease;cursor:pointer;border:1px solid var(--border);background:var(--bg-panel);}
.card:active{transform:scale(0.97);background:rgba(255,255,255,0.07);}
.btn{border:none;cursor:pointer;font-family:'DM Sans',sans-serif;font-weight:700;transition:opacity 0.2s,transform 0.2s;text-align:center;text-decoration:none;display:inline-block;}
#xp-toast{position:fixed;top:20px;right:20px;z-index:1000;background:linear-gradient(135deg,var(--accent),#e8a020);color:#000;padding:12px 24px;border-radius:100px;font-weight:700;animation:xpFloat 2.5s ease forwards;display:none;}
.screen{display:none;max-width:680px;margin:0 auto;padding-bottom:80px;}
.screen.active{display:block;}
.tab-bar{position:fixed;bottom:0;left:0;right:0;background:rgba(10,10,15,0.95);backdrop-filter:blur(10px);border-top:1px solid var(--border);display:flex;padding:12px;z-index:900;}
.tab-item{color:var(--text-muted);display:flex;flex-direction:column;align-items:center;flex:1;font-size:11px;text-decoration:none;}
.tab-item.active{color:var(--primary);}
@keyframes fadeUp{from{opacity:0;transform:translateY(15px);}to{opacity:1;transform:translateY(0);}}
@keyframes xpFloat{0%{opacity:0;transform:translateY(0);}20%{opacity:1;}100%{opacity:0;transform:translateY(-50px);}}
</style>
</head>
<body>
<div id="xp-toast"></div>
<nav class="tab-bar" id="main-nav">
  <a href="#" class="tab-item active" onclick="showScreen('home', this)"><span>📚</span>Módulos</a>
  <a href="#" class="tab-item" onclick="showScreen('logros', this)"><span>🏆</span>Logros</a>
</nav>

<div id="screen-home" class="screen active">
  <div style="padding:40px 24px; border-bottom:1px solid var(--border);">
    <h1 style="font-family:'Playfair Display',serif;font-size:32px;color:#fff;">Negocios <span style="color:var(--primary);">Digitales Pro</span></h1>
  </div>
  <div style="display:grid;grid-template-columns:1fr 1fr;border-bottom:1px solid var(--border);">
    <div style="padding:20px;text-align:center;border-right:1px solid var(--border);"><div id="stat-xp" style="font-size:24px;color:var(--accent);font-weight:900;">0</div><div style="font-size:10px;color:var(--text-muted);">XP</div></div>
    <div style="padding:20px;text-align:center;"><div id="stat-med" style="font-size:24px;color:#fff;font-weight:900;">0/5</div><div style="font-size:10px;color:var(--text-muted);">MEDALLAS</div></div>
  </div>
  <div id="modulos-list" style="padding:20px;"></div>
</div>

<div id="screen-logros" class="screen">
  <div style="padding:40px 24px;"><h1 style="color:#fff;">Tus Trofeos</h1></div>
  <div id="medallas-grid" style="display:grid;grid-template-columns:1fr 1fr;gap:15px;padding:20px;"></div>
</div>

<div id="screen-modulo" class="screen">
    <div id="mod-header" style="padding:20px;">
        <button class="btn" onclick="backToMain()" style="color:var(--text-muted);background:none;">← Volver</button>
        <h2 id="mod-titulo" style="color:#fff;margin-top:10px;"></h2>
    </div>
    <div id="lecciones-list" style="padding:20px;"></div>
</div>

<div id="screen-leccion" class="screen">
    <div id="lec-header" style="padding:20px;"><button class="btn" onclick="goModulo(currentModuloId)" style="color:var(--text-muted);background:none;">←</button></div>
    <div style="padding:20px;">
        <h1 id="lec-titulo" style="color:#fff;margin-bottom:20px;"></h1>
        <div id="lec-cuerpo" style="color:var(--text-muted);line-height:1.6;white-space:pre-wrap;margin-bottom:30px;"></div>
        <div id="lec-actions"></div>
    </div>
</div>

<script>
const MODULOS=[
  {id:1,titulo:"Fundamentos",icono:"◈",color:"#E8C547",medal:"🥇",lecciones:[
    {titulo:"¿Qué es un negocio digital?",cuerpo:"Un negocio digital opera 24/7 sin fronteras. Es un sistema automatizado.",concepto:"Ensambla, no inventes."},
    {titulo:"Mentalidad Pro",cuerpo:"Crea activos, no busques salarios.",concepto:"Invierte en activos."},
    {titulo:"Tu Nicho",cuerpo:"Sé el pez grande en una pecera pequeña.",concepto:"Especialización = Dinero."}
  ]},
  {id:2,titulo:"Marketing IA",icono:"◎",color:"#47E8A0",medal:"🤖",lecciones:[
    {titulo:"Algoritmos",cuerpo:"Retención es la clave.",concepto:"Engancha en 3 seg."},
    {titulo:"Copywriting",cuerpo:"Problema -> Agitación -> Solución.",concepto:"Las palabras venden."},
    {titulo:"Embudos",cuerpo:"Atención a Acción de forma automática.",concepto:"Sistematiza todo."}
  ]}
];

let estado = JSON.parse(localStorage.getItem('nd_pro_v1')) || { completadas: [], xp: 0, medallas: [] };
let currentModuloId = null;

function save(){ localStorage.setItem('nd_pro_v1', JSON.stringify(estado)); }

function renderModulos(){
  const container = document.getElementById('modulos-list');
  container.innerHTML = MODULOS.map(m => {
    const total = m.lecciones.length;
    const done = m.lecciones.filter((_, i) => estado.completadas.includes(`${m.id}-${i}`)).length;
    const pct = Math.round((done/total)*100);
    return `<div class="card" onclick="goModulo(${m.id})" style="padding:20px;border-radius:12px;margin-bottom:15px;">
        <div style="color:${m.color};font-size:20px;">${m.icono} ${m.titulo}</div>
        <div style="margin-top:10px;height:4px;background:rgba(255,255,255,0.1);"><div style="width:${pct}%;height:100%;background:${m.color};"></div></div>
    </div>`;
  }).join('');
}

function renderMedallas(){
  const container = document.getElementById('medallas-grid');
  container.innerHTML = MODULOS.map(m => {
    const unlocked = estado.medallas.includes(m.id);
    return `<div class="card" style="padding:20px;text-align:center;border-radius:12px;opacity:${unlocked?1:0.3};">
        <div style="font-size:40px;">${m.medal}</div>
        <div style="font-size:12px;margin-top:10px;">${m.titulo}</div>
    </div>`;
  }).join('');
}

function goModulo(id){
  currentModuloId = id;
  const mod = MODULOS.find(m => m.id === id);
  showScreen('modulo');
  document.getElementById('mod-titulo').innerText = mod.titulo;
  document.getElementById('lecciones-list').innerHTML = mod.lecciones.map((l, i) => {
    const isDone = estado.completadas.includes(`${id}-${i}`);
    return `<div class="card" onclick="goLeccion(${id}, ${i})" style="padding:15px;margin-bottom:10px;border-radius:10px;">
        ${isDone ? '✅' : '○'} ${l.titulo}
    </div>`;
  }).join('');
}

function goLeccion(mId, lIdx){
  const lec = MODULOS.find(m => m.id === mId).lecciones[lIdx];
  showScreen('leccion');
  document.getElementById('lec-titulo').innerText = lec.titulo;
  document.getElementById('lec-cuerpo').innerText = lec.cuerpo;
  const isDone = estado.completadas.includes(`${mId}-${lIdx}`);
  document.getElementById('lec-actions').innerHTML = isDone ? '' : `<button class="btn" onclick="completar(${mId}, ${lIdx})" style="width:100%;padding:15px;background:var(--primary);color:#fff;border-radius:10px;">Completar +50 XP</button>`;
}

function completar(mId, lIdx){
  const key = `${mId}-${lIdx}`;
  if(!estado.completadas.includes(key)){
    estado.completadas.push(key);
    estado.xp += 50;
    const mod = MODULOS.find(m => m.id === mId);
    const totalLec = mod.lecciones.length;
    const doneLec = mod.lecciones.filter((_, i) => estado.completadas.includes(`${mId}-${i}`)).length;
    if(totalLec === doneLec && !estado.medallas.includes(mId)){
        estado.medallas.push(mId);
        estado.xp += 200;
        alert("¡Nueva Medalla Desbloqueada!");
    }
    save();
    actualizarStats();
    goModulo(mId);
  }
}

function actualizarStats(){
  document.getElementById('stat-xp').
  
