<!doctype html>
<html lang="vi">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Kim Khánh — Portfolio Aesthetic (Music Player)</title>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;500;700;800&display=swap" rel="stylesheet">
<style>
:root{
  --bg:#e7f3ff;
  --card:#ffffffee;
  --text:#13202b;
  --muted:#5c6b7a;
  --accent:#5fa8ff;
  --accent-2:#3f7fe0;
  --glass: rgba(255,255,255,0.55);
  --radius:16px;
  --shadow:0 12px 30px rgba(50,90,160,0.14);
  --soft:0 8px 18px rgba(20,40,80,0.06);
}
body.dark{
  --bg:#07101a;
  --card:rgba(20,30,45,0.88);
  --text:#dbe9ff;
  --muted:#8faad0;
  --accent:#7abbff;
  --accent-2:#3d8aff;
}
*{box-sizing:border-box;margin:0;padding:0}
html,body{height:100%}
body{
  font-family:'Inter',system-ui,-apple-system,Segoe UI,Roboto,Arial;
  background:linear-gradient(180deg,var(--bg),#d8eefd);
  color:var(--text);
  -webkit-font-smoothing:antialiased;
  overflow-x:hidden;
  line-height:1.55;
  transition:background .35s,color .35s;
}

/* Floating shapes */
.shape{position:fixed;width:260px;height:260px;border-radius:50%;
  background:radial-gradient(circle at 20% 30%, var(--accent), transparent 60%);
  filter:blur(60px);opacity:.4;z-index:0;pointer-events:none;transform:translate3d(0,0,0);animation:float 10s ease-in-out infinite}
.shape.b{background:radial-gradient(circle at 80% 60%, var(--accent-2), transparent 60%);animation-delay:2s}
@keyframes float{0%{transform:translateY(0)}50%{transform:translateY(36px)}100%{transform:translateY(0)}}

/* NAV */
nav{position:sticky;top:14px;z-index:30;margin:12px auto;max-width:1100px;display:flex;gap:12px;justify-content:center;padding:10px;background:var(--glass);border-radius:999px;backdrop-filter:blur(10px) saturate(1.05);box-shadow:var(--soft);border:1px solid rgba(255,255,255,0.45)}
body.dark nav{background:rgba(12,20,34,0.55);border:1px solid rgba(255,255,255,0.06)}
nav a{color:var(--text);text-decoration:none;padding:8px 14px;border-radius:999px;font-weight:700;font-size:14px;transition:transform .18s, background .18s}
nav a:hover{transform:translateY(-3px); background:var(--accent); color:white}
nav a.active{background:linear-gradient(90deg,var(--accent),var(--accent-2)); color:white; transform:translateY(-4px)}

/* Header */
.header{position:relative;height:320px;display:flex;align-items:center;justify-content:center;overflow:hidden;margin-bottom:18px;border-bottom-left-radius:18px;border-bottom-right-radius:18px}
.header video{position:absolute;width:100%;height:100%;object-fit:cover;filter:brightness(.6) saturate(.95);z-index:0}
.header .overlay{position:absolute;inset:0;background:linear-gradient(180deg, rgba(4,30,60,0.35), rgba(4,30,60,0.08));z-index:1}
.title-wrap{position:relative;z-index:2;text-align:center;color:white}
.title-typing{font-size:30px;font-weight:800;white-space:nowrap;overflow:hidden;border-right:3px solid rgba(255,255,255,0.9);width:28ch;animation:typing 3s steps(28,end), blink .8s step-end infinite}
@keyframes typing{from{width:0} to{width:28ch}} @keyframes blink{50%{border-color:transparent}}
.subtitle{margin-top:10px;color:rgba(255,255,255,0.95);font-weight:500}

/* Container / cards */
.container{max-width:1100px;margin:-70px auto 80px;padding:24px;position:relative;z-index:5}
.card{background:var(--card);border-radius:14px;padding:20px;margin-bottom:22px;box-shadow:var(--shadow);border:1px solid rgba(0,0,0,0.04)}

/* Sections layout */
.hero{display:flex;gap:20px;align-items:center;flex-wrap:wrap}
.avatar{width:200px;height:200px;border-radius:16px;overflow:hidden;border:6px solid rgba(255,255,255,0.64);box-shadow:0 16px 36px rgba(30,70,140,0.12)}
.avatar img{width:100%;height:100%;object-fit:cover;display:block}
.lead{color:var(--muted);max-width:640px}

/* Music section */
.music-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(240px,1fr));gap:18px;margin-top:12px}
.track{
  position:relative;border-radius:12px;overflow:hidden;cursor:pointer;
  background:linear-gradient(180deg, rgba(255,255,255,0.72), rgba(255,255,255,0.45));
  box-shadow:0 10px 26px rgba(20,40,80,0.06);border:1px solid rgba(0,0,0,0.04);
  transition:transform .22s, box-shadow .22s;
}
.track:hover{transform:translateY(-6px);box-shadow:0 20px 50px rgba(20,40,80,0.12)}
.track img{width:100%;height:160px;object-fit:cover;display:block;filter:saturate(.95)}
.track .meta{padding:12px}
.track .title{font-weight:700}
.track .subtitle{color:var(--muted);font-size:13px;margin-top:6px}

/* play overlay */
.play-btn{
  position:absolute;left:50%;top:50%;transform:translate(-50%,-50%);
  width:62px;height:62px;border-radius:999px;background:linear-gradient(135deg,var(--accent),var(--accent-2));
  display:grid;place-items:center;color:white;font-weight:800;box-shadow:0 12px 30px rgba(30,90,160,0.18);opacity:0.98}
.play-icon{width:0;height:0;border-left:16px solid white;border-top:10px solid transparent;border-bottom:10px solid transparent;margin-left:4px}

/* slide-up reveal */
.fade{opacity:0;transform:translateY(26px);transition:all .8s cubic-bezier(.2,.9,.2,1)}
.fade.show{opacity:1;transform:translateY(0)}

/* popup mini-player (Spotify style) */
.player-popup{
  position:fixed;right:18px;bottom:18px;width:340px;max-width:calc(100% - 40px);z-index:200;
  background:linear-gradient(180deg,rgba(20,25,40,0.98),rgba(12,18,30,0.98));color:white;border-radius:14px;padding:12px;box-shadow:0 18px 60px rgba(10,20,40,0.5);display:flex;gap:12px;align-items:center;
  transform:translateY(20px);opacity:0;pointer-events:none;transition:all .3s ease;
}
.player-popup.show{transform:translateY(0);opacity:1;pointer-events:auto}
.player-thumb{width:72px;height:72px;border-radius:8px;overflow:hidden;flex-shrink:0}
.player-thumb img{width:100%;height:100%;object-fit:cover;display:block}
.player-info{flex:1;min-width:0}
.player-title{font-weight:800;font-size:15px;line-height:1.05}
.player-sub{color:#bcd4ff;font-size:13px;margin-top:4px}
.player-controls{display:flex;align-items:center;gap:12px;margin-top:10px}
.icon-btn{width:44px;height:44px;border-radius:10px;display:grid;place-items:center;background:rgba(255,255,255,0.06);cursor:pointer}
.icon-btn.play{background:linear-gradient(90deg,var(--accent),var(--accent-2));color:white}
.progress{height:6px;background:rgba(255,255,255,0.08);border-radius:6px;margin-top:10px;overflow:hidden}
.progress > i{display:block;height:100%;width:0;background:linear-gradient(90deg,#9fd1ff,#5fa8ff);border-radius:6px;transition:width .12s linear}

/* player small close */
.player-close{position:absolute;top:8px;right:8px;font-size:13px;color:rgba(255,255,255,0.7);cursor:pointer}

/* responsive */
@media(max-width:520px){.player-popup{right:12px;left:12px;width:auto;padding:10px}.player-thumb{width:60px;height:60px}}
</style>
</head>
<body>

<!-- floating shapes -->
<div class="shape" style="left:-80px;top:40px"></div>
<div class="shape b" style="right:-80px;bottom:60px"></div>

<!-- NAV -->
<nav id="mainNav">
  <a href="#home" class="active">Trang chủ</a>
  <a href="#about">Về tôi</a>
  <a href="#categories">Danh mục</a>
  <a href="#projects">Projects</a>
  <a href="#music">Âm nhạc</a>
  <a href="#contact">Liên hệ</a>
</nav>

<!-- HEADER -->
<header class="header">
  <video autoplay muted loop playsinline>
    <source src="https://cdn.coverr.co/videos/blue-ocean-waves/download?token=demo" type="video/mp4">
  </video>
  <div class="overlay"></div>
  <div class="title-wrap">
    <div class="title-typing">Kim Khánh — Portfolio 12A • Music • Projects</div>
    <div class="subtitle">Cuộn xuống để xem music player đẹp + popup mini-player</div>
  </div>
</header>

<!-- Dark toggle -->
<button id="darkToggle" style="position:fixed;right:18px;bottom:90px;z-index:80;padding:10px 12px;border-radius:999px;border:0;background:linear-gradient(90deg,var(--accent),var(--accent-2));color:white;box-shadow:var(--shadow)">🌙</button>

<!-- MAIN -->
<main class="container">

  <!-- HOME -->
  <section id="home" class="card fade">
    <div class="hero">
      <div class="avatar"><img src="https://raw.githubusercontent.com/daisubinta/Nhom4tin12anh.github.io/refs/heads/main/golden-retriever-tongue-out.jpg" alt="avatar"></div>
      <div>
        <h1>Xin chào! Mình là Kim Khánh 👋</h1>
        <p class="lead">Học sinh lớp 12A — thích làm web aesthetic, học tiếng Anh và sáng tạo nội dung. Đây là trang portfolio mình làm để trình bày dự án và playlist yêu thích.</p>
      </div>
    </div>
  </section>

  <!-- ABOUT -->
  <section id="about" class="card fade">
    <h2>Về mình</h2>
    <p>Mình là Kim Khánh — thích viết, thiết kế và học ngôn ngữ. Mục tiêu: IELTS 8.0 và làm freelance copywriting.</p>
  </section>

  <!-- CATEGORIES -->
  <section id="categories" class="card fade">
    <h2>Danh mục</h2>
    <div style="display:grid;grid-template-columns:repeat(auto-fit,minmax(180px,1fr));gap:12px;margin-top:12px">
      <div class="card" style="padding:14px">IELTS — tips & practise</div>
      <div class="card" style="padding:14px">Copywriting — email & landing</div>
      <div class="card" style="padding:14px">Pet Brand — research</div>
      <div class="card" style="padding:14px">Beach Boutique — projects</div>
    </div>
  </section>

  <!-- PROJECTS -->
  <section id="projects" class="card fade">
    <h2>Projects</h2>
    <ul style="margin-top:10px;color:var(--muted)">
      <li>Beach Boutique — welcome flow (3 emails)</li>
      <li>Bulldogology — landing page rewrite (mock)</li>
      <li>IELTS Notes Hub — study resources</li>
    </ul>
  </section>

  <!-- MUSIC SECTION -->
  <section id="music" class="card fade">
    <h2>Âm nhạc yêu thích</h2>
    <p style="color:var(--muted);margin-top:6px">Click hình để mở player. Popup player sẽ ghim góc để bạn vẫn duyệt trang.</p>

    <div class="music-grid" id="musicGrid">
      <!-- Tracks are injected by JS -->
    </div>
  </section>

  <!-- CONTACT (separate) -->
  <section id="contact" class="card fade">
    <h2>Liên hệ</h2>
    <div style="display:flex;gap:12px;flex-wrap:wrap;margin-top:12px">
      <div style="min-width:200px;background:var(--card);padding:12px;border-radius:10px">✉️ Email: yourmail@example.com</div>
      <div style="min-width:200px;background:var(--card);padding:12px;border-radius:10px">📸 Instagram: @yourhandle</div>
    </div>
  </section>

  <div style="height:28px"></div>
</main>

<!-- POPUP MINI PLAYER -->
<div id="playerPopup" class="player-popup" aria-hidden="true">
  <div class="player-close" id="playerClose">✕</div>
  <div class="player-thumb"><img id="playerThumb" src=""></div>
  <div class="player-info">
    <div class="player-title" id="playerTitle">Title</div>
    <div class="player-sub" id="playerSub">Artist</div>

    <div class="player-controls">
      <div class="icon-btn" id="prevBtn" title="Previous">⏮</div>
      <div class="icon-btn play" id="playBtn" title="Play/Pause">▶</div>
      <div class="icon-btn" id="nextBtn" title="Next">⏭</div>
      <div style="flex:1"></div>
      <div class="icon-btn" id="closeMini" title="Close">⬇</div>
    </div>

    <div class="progress"><i id="progressFill" style="width:0%"></i></div>
  </div>
</div>

<!-- Hidden iframe holder for YouTube player (API) -->
<div id="yt-player" style="display:none"></div>

<!-- SCRIPTS -->
<script>
/* =========================
  Track list (edit to change songs)
  Each track: { id, title, artist }
========================= */
const tracks = [
  { id: '_E-7A81Ac8U', title: 'Bài 1 (Noo/Track)', artist: 'Noo Phước Thịnh / Track 1' },
  { id: 'stvWuowo1dU', title: 'Bài 2 (Noo/Track)', artist: 'Noo Phước Thịnh / Track 2' },
  { id: 'Rzm_kltwHbg', title: 'Bài 3 (Noo/Track)', artist: 'Noo Phước Thịnh / Track 3' },
  { id: '4eCtPlJ0aQ8', title: 'Bài 4', artist: 'Artist' }
];

/* Build music grid with thumbnails (YouTube hq thumbnails) */
const grid = document.getElementById('musicGrid');
tracks.forEach((t, i) => {
  const card = document.createElement('div');
  card.className = 'track';
  card.dataset.index = i;
  card.innerHTML = `
    <img src="https://img.youtube.com/vi/${t.id}/hqdefault.jpg" alt="${t.title}">
    <div class="play-btn" aria-hidden="true"><span class="play-icon"></span></div>
    <div class="meta">
      <div class="title">${t.title}</div>
      <div class="subtitle">${t.artist}</div>
    </div>`;
  grid.appendChild(card);

  card.addEventListener('click', ()=> openPlayer(i));
});

/* Slide-up reveal for sections */
const faders = document.querySelectorAll('.fade');
const io = new IntersectionObserver((entries) => {
  entries.forEach(e => {
    if(e.isIntersecting) e.target.classList.add('show');
  });
}, {threshold: 0.15, rootMargin:'0px 0px -60px 0px'});
faders.forEach(f => io.observe(f));

/* Dark mode toggle */
const darkBtn = document.getElementById('darkToggle');
darkBtn.addEventListener('click', () => {
  document.body.classList.toggle('dark');
  darkBtn.textContent = document.body.classList.contains('dark') ? '☀️' : '🌙';
});

/* Smooth nav scrolling */
document.querySelectorAll('nav a').forEach(a=>{
  a.addEventListener('click', (e)=>{
    e.preventDefault();
    document.querySelectorAll('nav a').forEach(x=>x.classList.remove('active'));
    a.classList.add('active');
    const id = a.getAttribute('href').slice(1);
    const el = document.getElementById(id);
    if(el) el.scrollIntoView({behavior:'smooth', block:'start'});
  });
});

/* =========================
  YouTube IFrame API player + mini-player logic
========================= */
let player; // YT player instance
let currentIndex = null;
let progressInterval = null;

function onYouTubeIframeAPIReady(){
  // create a hidden player (will load videos programmatically)
  player = new YT.Player('yt-player', {
    height: '0', width: '0',
    videoId: tracks[0].id,
    playerVars: { 'playsinline': 1, 'rel': 0 },
    events: {
      'onReady': onPlayerReady,
      'onStateChange': onPlayerStateChange
    }
  });
}

function onPlayerReady(){ /* nothing for now */ }
function onPlayerStateChange(event){
  // update play button icon
  updatePlayIcon();
}

/* load API */
(function loadYT(){
  const tag = document.createElement('script');
  tag.src = "https://www.youtube.com/iframe_api";
  document.head.appendChild(tag);
})();

/* UI elements */
const playerPopup = document.getElementById('playerPopup');
const playerThumb = document.getElementById('playerThumb');
const playerTitle = document.getElementById('playerTitle');
const playerSub = document.getElementById('playerSub');
const playBtn = document.getElementById('playBtn');
const prevBtn = document.getElementById('prevBtn');
const nextBtn = document.getElementById('nextBtn');
const playerClose = document.getElementById('playerClose');
const closeMini = document.getElementById('closeMini');
const progressFill = document.getElementById('progressFill');

playBtn.addEventListener('click', togglePlay);
prevBtn.addEventListener('click', playPrev);
nextBtn.addEventListener('click', playNext);
playerClose.addEventListener('click', closePlayer);
closeMini.addEventListener('click', closePlayer);

/* open player popup and load track */
function openPlayer(index){
  currentIndex = index;
  const t = tracks[index];
  // update UI
  playerThumb.src = `https://img.youtube.com/vi/${t.id}/mqdefault.jpg`;
  playerTitle.textContent = t.title;
  playerSub.textContent = t.artist;
  // show popup
  playerPopup.classList.add('show');
  playerPopup.setAttribute('aria-hidden','false');
  // load video in yt player
  if(player && player.loadVideoById){
    player.loadVideoById(t.id);
    player.playVideo();
    startProgressUpdater();
  } else {
    // if player not ready yet, wait until API ready
    const wait = setInterval(()=>{
      if(player && player.loadVideoById){
        clearInterval(wait);
        player.loadVideoById(t.id);
        player.playVideo();
        startProgressUpdater();
      }
    },200);
  }
}

/* Toggle play/pause */
function togglePlay(){
  if(!player) return;
  const state = player.getPlayerState();
  // states: -1 unstarted, 0 ended, 1 playing, 2 paused, 3 buffering, 5 cued
  if(state === 1){ player.pauseVideo(); }
  else { player.playVideo(); startProgressUpdater(); }
  updatePlayIcon();
}

/* Update play icon based on state */
function updatePlayIcon(){
  if(!player) return;
  const st = player.getPlayerState();
  playBtn.textContent = (st === 1) ? '⏸' : '▶';
}

/* prev / next */
function playPrev(){
  if(currentIndex === null) return;
  const prev = (currentIndex - 1 + tracks.length) % tracks.length;
  openPlayer(prev);
}
function playNext(){
  if(currentIndex === null) return;
  const nxt = (currentIndex + 1) % tracks.length;
  openPlayer(nxt);
}

/* close popup player */
function closePlayer(){
  playerPopup.classList.remove('show');
  playerPopup.setAttribute('aria-hidden','true');
  if(player && player.pauseVideo) player.pauseVideo();
  clearInterval(progressInterval);
}

/* progress updater */
function startProgressUpdater(){
  clearInterval(progressInterval);
  progressInterval = setInterval(()=>{
    if(!player || !player.getDuration) return;
    const dur = player.getDuration();
    const pos = player.getCurrentTime();
    if(dur && !isNaN(dur)) {
      const pct = Math.max(0, Math.min(100, (pos/dur)*100));
      progressFill.style.width = pct + '%';
      // if ended, auto next
      if(player.getPlayerState() === 0){ // ended
        playNext();
      }
    }
    updatePlayIcon();
  }, 400);
}

/* ensure clicking a track pauses others (not necessary because single player) */

/* optional: initial hide player until user opens */
playerPopup.classList.remove('show');

/* keep nav active on scroll (simple) */
const sections = document.querySelectorAll('main section, main');
window.addEventListener('scroll', ()=>{
  let idx = 0;
  sections.forEach((sec,i)=>{
    const r = sec.getBoundingClientRect();
    if(r.top <= 100) idx = i;
  });
  document.querySelectorAll('nav a').forEach(a=>a.classList.remove('active'));
  const allNav = Array.from(document.querySelectorAll('nav a'));
  const target = allNav[Math.min(allNav.length-1, Math.max(0, idx))];
  if(target) target.classList.add('active');
});
</script>
</body>
</html>
