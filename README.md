<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<!-- EDIT: your name and a one-line description (shows in search results and link previews) -->
<title>Calvin Moras: light, code, and hardware</title>
<meta name="description" content="Real-time visuals in TouchDesigner, laser systems, reverse engineering, Python, and photography.">
<meta property="og:title" content="Your Name">
<meta property="og:description" content="Real-time visuals, lasers, reverse engineering, Python, and photography.">
<meta name="theme-color" content="#16132B">
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Anybody:wdth,wght@50..150,400..900&family=Newsreader:ital,opsz,wght@0,6..72,400..600;1,6..72,400&display=swap">
<style>
:root{
  --haze:#16132B;      /* stage haze: the page background */
  --haze-2:#221E42;    /* raised surfaces */
  --line:#3A3566;      /* rules and borders */
  --text:#F1E7D6;      /* tungsten white */
  --dim:#A49EC2;       /* secondary text */
  --beam:#FF4A3D;      /* current laser color, set by the wavelength switch */
  --display:"Anybody", system-ui, sans-serif;
  --body:"Newsreader", Georgia, serif;
  --ease:cubic-bezier(.2,.7,.2,1);
  --pad:clamp(1rem,4vw,3rem);
  interpolate-size:allow-keywords;
}
*,*::before,*::after{box-sizing:border-box}
html{background:var(--haze);color:var(--text);-webkit-text-size-adjust:100%}
@media (prefers-reduced-motion:no-preference){html{scroll-behavior:smooth}}
body{margin:0;font-family:var(--body);font-size:1.125rem;line-height:1.6;font-optical-sizing:auto;-webkit-font-smoothing:antialiased}
img,video,canvas{display:block;max-width:100%}
a{color:inherit;text-decoration-thickness:1px;text-underline-offset:.22em;text-decoration-color:color-mix(in oklab,var(--beam) 75%,transparent)}
a:hover{text-decoration-color:var(--beam)}
:focus-visible{outline:2px solid var(--text);outline-offset:3px;border-radius:2px}
::selection{background:var(--beam);color:var(--haze)}
.skip{position:absolute;left:1rem;top:-4rem;z-index:20;background:var(--text);color:var(--haze);padding:.5rem 1rem;border-radius:4px}
.skip:focus{top:1rem}

/* Navigation */
.nav{position:fixed;inset:0 0 auto 0;z-index:10;display:flex;justify-content:space-between;align-items:center;gap:1rem;
  padding:max(.9rem,env(safe-area-inset-top)) var(--pad) .9rem;transition:background-color .35s}
.nav.solid{background:rgb(22 19 43 / .9);box-shadow:0 1px 0 var(--line)}
.home{font-family:var(--display);font-weight:750;font-stretch:125%;font-size:1rem;text-decoration:none;white-space:nowrap}
.nav ul{display:flex;gap:clamp(1rem,3vw,2.25rem);list-style:none;margin:0;padding:0}
.nav ul a{text-decoration:none;color:var(--dim);font-size:1rem;transition:color .2s}
.nav ul a:hover,.nav ul a[aria-current="true"]{color:var(--text)}

/* Hero: the laser stage */
.hero{position:relative;height:100svh;min-height:34rem;overflow:hidden;touch-action:pan-y;cursor:crosshair;
  display:grid;align-content:end;padding:0 var(--pad) clamp(1.5rem,5vh,3.5rem)}
.hero canvas{position:absolute;inset:0;width:100%;height:100%}
.hero-copy,.hero-controls{position:relative}
h1{font-family:var(--display);font-weight:850;font-stretch:150%;font-size:clamp(3rem,11vw,10.5rem);
  line-height:.84;letter-spacing:-.015em;margin:0 0 1.4rem}
h1 span{display:block}
.lede{max-width:38ch;margin:0;font-size:clamp(1.15rem,1.6vw,1.35rem);line-height:1.5;text-shadow:0 1px 14px rgb(22 19 43 / .9)}
.hero-controls{display:flex;flex-wrap:wrap;justify-content:space-between;align-items:center;gap:1rem 2rem;margin-top:2rem}
.hint{margin:0;color:var(--dim);font-size:.95rem}
.nm{display:flex;gap:.5rem}
.nm button{font:inherit;font-family:var(--display);font-stretch:110%;font-size:.85rem;color:var(--dim);background:rgb(22 19 43 / .5);
  border:1px solid var(--line);border-radius:999px;min-height:42px;padding:0 .9rem 0 .7rem;display:flex;align-items:center;gap:.5rem;cursor:pointer;
  transition:color .2s,border-color .2s}
.nm button::before{content:"";width:.6rem;height:.6rem;border-radius:50%;background:var(--c);transition:box-shadow .3s}
.nm button[aria-pressed="true"]{color:var(--text);border-color:var(--c)}
.nm button[aria-pressed="true"]::before{box-shadow:0 0 10px 2px var(--c)}

/* Sections */
.section{max-width:90rem;margin:0 auto;padding:clamp(5rem,12vh,9rem) var(--pad) 0;scroll-margin-top:2rem}
.section-head{display:flex;flex-wrap:wrap;justify-content:space-between;align-items:flex-end;gap:.75rem 2rem;margin-bottom:clamp(2rem,5vh,3.5rem)}
h2{font-family:var(--display);font-weight:800;font-stretch:140%;font-size:clamp(2.5rem,7vw,5.5rem);line-height:.95;letter-spacing:-.01em;margin:0}
.section-head p{margin:0;color:var(--dim);max-width:34ch}

/* Work: expandable disciplines */
.discipline{border-top:1px solid var(--line)}
.discipline:last-of-type{border-bottom:1px solid var(--line)}
.discipline summary{list-style:none;cursor:pointer;display:grid;grid-template-columns:1fr auto;gap:.4rem 1.5rem;align-items:baseline;
  padding:clamp(1.3rem,3vh,2rem) 0}
.discipline summary::-webkit-details-marker{display:none}
.discipline h3{margin:0;font-family:var(--display);font-weight:720;font-stretch:112%;font-size:clamp(1.8rem,5vw,3.8rem);line-height:1;
  transition:font-stretch .5s var(--ease)}
.count{color:var(--dim);font-size:.95rem;white-space:nowrap}
.blurb{grid-column:1/-1;margin:0;color:var(--dim);max-width:60ch}
.beamline{grid-column:1/-1;height:2px;margin-top:.8rem;transform:scaleX(.08);transform-origin:left;transition:transform .7s var(--ease);
  background:linear-gradient(90deg,var(--beam),color-mix(in oklab,var(--beam) 35%,transparent) 55%,transparent);
  box-shadow:0 0 10px color-mix(in oklab,var(--beam) 60%,transparent)}
.discipline[open] .beamline{transform:scaleX(1)}
.discipline[open] h3{font-stretch:135%}
@media (hover:hover){.discipline summary:hover h3{font-stretch:135%}}
.discipline::details-content{block-size:0;overflow:hidden;transition:block-size .55s var(--ease),content-visibility .55s allow-discrete}
.discipline[open]::details-content{block-size:auto}

.projects{display:grid;grid-template-columns:repeat(auto-fit,minmax(min(100%,19rem),1fr));gap:2.5rem 2rem;padding:.75rem 0 3rem}
.project h4{font-family:var(--display);font-weight:700;font-stretch:110%;font-size:1.3rem;line-height:1.2;margin:1rem 0 .4rem}
.project p{margin:0;color:var(--dim);font-size:1.02rem}
.links{display:flex;flex-wrap:wrap;gap:.5rem 1.25rem;margin-top:.7rem;font-size:1rem}
.media{aspect-ratio:16/10;border-radius:6px;overflow:hidden;background:var(--haze-2)}
.media img,.media video{width:100%;height:100%;object-fit:cover}
.ph{display:grid;place-items:center;text-align:center;padding:1rem;color:var(--dim);font-size:.95rem;
  background:radial-gradient(ellipse at 25% 115%,color-mix(in oklab,var(--beam) 32%,transparent),transparent 62%),var(--haze-2)}
.pkg{display:grid;grid-template-columns:minmax(0,1.3fr) minmax(0,1fr);gap:2rem 3rem;align-items:start;padding:.75rem 0 2.5rem}
.pkg h4{font-family:var(--display);font-weight:800;font-stretch:125%;font-size:clamp(1.6rem,3vw,2.3rem);margin:0 0 .5rem;line-height:1.1}
.pkg p{margin:0 0 1.2rem;color:var(--dim);max-width:55ch}
.pip{display:inline-flex;align-items:center;gap:1.2rem;max-width:100%;font:inherit;color:var(--text);background:var(--haze-2);
  border:1px solid var(--line);border-radius:6px;padding:.75rem 1rem;cursor:pointer;text-align:left}
.pip code{font-family:ui-monospace,SFMono-Regular,Menlo,Consolas,monospace;font-size:.95rem;overflow-wrap:anywhere}
.pip span{color:var(--dim);font-size:.9rem;white-space:nowrap}
.pip:hover{border-color:var(--beam)}

/* Photographs */
.gallery{display:flex;gap:1rem;align-items:flex-start}
.col{flex:1;min-width:0;display:flex;flex-direction:column;gap:1rem}
.shot{position:relative;display:block;width:100%;padding:0;border:0;border-radius:4px;overflow:hidden;background:var(--haze-2);cursor:zoom-in}
.shot img,.shot video,.shot .demo{width:100%;height:100%;object-fit:cover}
@media (hover:hover){
  .shot img,.shot video,.shot .demo{filter:brightness(.82);transition:filter .45s var(--ease)}
  .shot:hover img,.shot:hover video,.shot:hover .demo,.shot:focus-visible img{filter:brightness(1)}
}
.chip{position:absolute;left:.6rem;bottom:.6rem;font-family:var(--display);font-size:.75rem;padding:.2rem .55rem;border-radius:999px;
  background:rgb(22 19 43 / .75);color:var(--text)}

/* About */
.about{display:grid;grid-template-columns:minmax(0,1.4fr) minmax(0,1fr);gap:2rem 4rem}
.about .prose p{margin:0 0 1.2rem;max-width:62ch}
.contact{list-style:none;margin:0;padding:0}
.contact li{border-top:1px solid var(--line)}
.contact li:last-child{border-bottom:1px solid var(--line)}
.contact a{display:flex;justify-content:space-between;align-items:baseline;gap:1rem;padding:1rem 0;text-decoration:none;
  font-family:var(--display);font-weight:650;font-stretch:115%;font-size:1.25rem}
.contact a span{font-family:var(--body);font-weight:400;font-stretch:normal;font-size:1rem;color:var(--dim);transition:color .2s}
.contact a:hover span{color:var(--text)}
footer{max-width:90rem;margin:0 auto;padding:clamp(4rem,10vh,7rem) var(--pad) max(2rem,env(safe-area-inset-bottom));color:var(--dim);font-size:.95rem}

/* Lightbox */
.lightbox{padding:0;border:0;margin:0;width:100vw;height:100dvh;max-width:none;max-height:none;background:transparent;color:var(--text);overflow:hidden}
.lightbox::backdrop{background:rgb(12 10 26 / .97)}
.lightbox[open]{animation:lb-in .25s var(--ease)}
@keyframes lb-in{from{opacity:0;transform:scale(.985)}}
.lightbox figure{margin:0;height:100%;display:grid;grid-template-rows:1fr auto;place-items:center;padding:4.5rem 1rem 1.25rem;touch-action:pan-y}
.lb-media{display:grid;place-items:center;min-height:0;max-height:100%}
.lb-media img,.lb-media video{max-width:min(100%,92vw);max-height:calc(100dvh - 9rem);object-fit:contain;border-radius:3px}
.lightbox figcaption{display:flex;flex-wrap:wrap;gap:.25rem 1.5rem;justify-content:center;color:var(--dim);font-size:1rem;padding-top:1rem;text-align:center}
.lb-count{white-space:nowrap}
.lb-btn{position:absolute;display:grid;place-items:center;width:48px;height:48px;border-radius:50%;cursor:pointer;color:var(--text);
  background:rgb(22 19 43 / .7);border:1px solid var(--line)}
.lb-btn:hover{border-color:var(--text)}
.lb-btn svg{width:20px;height:20px}
.lb-close{top:max(1rem,env(safe-area-inset-top));right:1rem}
.lb-prev{left:1rem;top:50%;translate:0 -50%}
.lb-next{right:1rem;top:50%;translate:0 -50%}

@media (max-width:760px){
  .about,.pkg{grid-template-columns:1fr}
  .nav ul{gap:1rem}
}
@media (max-width:640px){
  h1{font-stretch:108%;font-size:clamp(2.8rem,15vw,5rem)}
  .home{font-stretch:100%}
  .lb-prev,.lb-next{top:auto;bottom:1rem;translate:none}
  .lightbox figure{padding-bottom:5rem}
}
@media (prefers-reduced-motion:reduce){
  *,*::before,*::after{transition-duration:.01ms!important;animation-duration:.01ms!important}
}
</style>
</head>
<body>
<a class="skip" href="#work">Skip to work</a>

<header class="nav">
  <a class="home" href="#top">Calvin Moras</a>
  <nav aria-label="Sections">
    <ul>
      <li><a href="#work">Work</a></li>
      <li><a href="#photos">Photos</a></li>
      <li><a href="#about">About</a></li>
    </ul>
  </nav>
</header>

<main>
  <section class="hero" id="top">
    <canvas id="beam" aria-hidden="true"></canvas>
    <div class="hero-copy">
      <h1><span>Calvin</span><span>Moras</span></h1>
      <!-- EDIT: one or two sentences, in your voice -->
      <p class="lede">I build with light: real-time visuals in TouchDesigner, laser systems, and teardowns of hardware I wasn't supposed to open. I also write Python and carry a camera most places.</p>
    </div>
    <div class="hero-controls">
      <p class="hint" id="hint">Move your cursor to aim the beam.</p>
      <div class="nm" role="group" aria-label="Beam color">
        <button type="button" data-hex="#FF4A3D" style="--c:#FF4A3D" aria-pressed="true">638 nm</button>
        <button type="button" data-hex="#4DFF3F" style="--c:#4DFF3F" aria-pressed="false">520 nm</button>
        <button type="button" data-hex="#6A5BFF" style="--c:#6A5BFF" aria-pressed="false">450 nm</button>
      </div>
    </div>
  </section>

  <section class="section" id="work">
    <div class="section-head">
      <h2>Work</h2>
      <p>Open a discipline to see the projects in it.</p>
    </div>

    <!--
      Each discipline is a native <details> element, so it works without JavaScript and with a keyboard.
      To use a real image:  <div class="media"><img src="media/work/thing.webp" alt="..." loading="lazy" width="1600" height="1000"></div>
      To use a looping clip: <div class="media"><video src="media/work/thing.mp4" poster="media/work/thing.jpg" muted loop playsinline preload="none" data-inview></video></div>
      (data-inview makes the clip play only while it's on screen.)
    -->

    <details class="discipline" open>
      <summary>
        <h3>Real-time visuals</h3>
        <span class="count">3 projects</span>
        <p class="blurb">TouchDesigner networks for live shows, installations, and audio-reactive pieces.</p><!-- EDIT -->
        <span class="beamline" aria-hidden="true"></span>
      </summary>
      <div class="projects">
        <article class="project">
          <div class="media ph">Add a render or screen capture</div>
          <h4>Project title</h4>
          <p>What it is, where it ran, and the one technique you're proud of.</p>
          <div class="links"><a href="#">Watch</a><a href="#">.toe file</a></div>
        </article>
        <article class="project">
          <div class="media ph">Add a render or screen capture</div>
          <h4>Project title</h4>
          <p>What it is, where it ran, and the one technique you're proud of.</p>
          <div class="links"><a href="#">Watch</a></div>
        </article>
        <article class="project">
          <div class="media ph">Add a render or screen capture</div>
          <h4>Project title</h4>
          <p>What it is, where it ran, and the one technique you're proud of.</p>
          <div class="links"><a href="#">Watch</a></div>
        </article>
      </div>
    </details>

    <details class="discipline">
      <summary>
        <h3>Lasers</h3>
        <span class="count">2 projects</span>
        <p class="blurb">Laser shows, scanner builds, and beam effects tuned for haze.</p><!-- EDIT -->
        <span class="beamline" aria-hidden="true"></span>
      </summary>
      <div class="projects">
        <article class="project">
          <div class="media ph">Add a photo or clip of the beams</div>
          <h4>Project title</h4>
          <p>The hardware, the control path, and what it looked like in the room.</p>
          <div class="links"><a href="#">Watch</a></div>
        </article>
        <article class="project">
          <div class="media ph">Add a photo or clip of the beams</div>
          <h4>Project title</h4>
          <p>The hardware, the control path, and what it looked like in the room.</p>
          <div class="links"><a href="#">Build notes</a></div>
        </article>
      </div>
    </details>

    <details class="discipline">
      <summary>
        <h3>Reverse engineering</h3>
        <span class="count">2 write-ups</span>
        <p class="blurb">Firmware, protocols, and devices taken apart to learn how they work.</p><!-- EDIT -->
        <span class="beamline" aria-hidden="true"></span>
      </summary>
      <div class="projects">
        <article class="project">
          <div class="media ph">Add a board shot or disassembly screenshot</div>
          <h4>Write-up title</h4>
          <p>The target, the question you started with, and what you found.</p>
          <div class="links"><a href="#">Read the write-up</a><a href="#">Code</a></div>
        </article>
        <article class="project">
          <div class="media ph">Add a board shot or disassembly screenshot</div>
          <h4>Write-up title</h4>
          <p>The target, the question you started with, and what you found.</p>
          <div class="links"><a href="#">Read the write-up</a></div>
        </article>
      </div>
    </details>

    <details class="discipline">
      <summary>
        <h3>Software</h3>
        <span class="count">1 package, more on GitHub</span>
        <p class="blurb">Python, tools, and the code that holds the other projects together.</p><!-- EDIT -->
        <span class="beamline" aria-hidden="true"></span>
      </summary>
      <div class="pkg">
        <div>
          <h4>your-package</h4><!-- EDIT: package name -->
          <p>One sentence on what it does and who it's for. A second on why you built it.</p>
          <button class="pip" type="button" data-copy="pip install your-package"><code>pip install your-package</code><span>Copy</span></button>
          <div class="links"><a href="#">PyPI</a><a href="#">Source</a><a href="#">Docs</a></div>
        </div>
        <div class="media ph">Add a terminal screenshot or a small demo GIF</div>
      </div>
      <div class="projects">
        <article class="project">
          <h4>Other project</h4>
          <p>Short description and the stack.</p>
          <div class="links"><a href="#">Source</a></div>
        </article>
        <article class="project">
          <h4>Other project</h4>
          <p>Short description and the stack.</p>
          <div class="links"><a href="#">Source</a></div>
        </article>
      </div>
    </details>
  </section>

  <section class="section" id="photos">
    <div class="section-head">
      <h2>Photographs</h2>
      <p>Select any frame to view it large. Use the arrow keys or swipe to move between them.</p>
    </div>
    <div class="gallery" id="gallery"></div>
    <noscript><p>The gallery needs JavaScript to load.</p></noscript>
  </section>

  <section class="section" id="about">
    <div class="section-head"><h2>About</h2></div>
    <div class="about">
      <div class="prose"><!-- EDIT -->
        <p>I've always had a fascination with lights from an early age, as well as electronics and computers.</p>
        <p>I'm always looking for new light sources to play with, especially if I can write a keyboard shortcut to control them!</p>
      </div>
      <ul class="contact"><!-- EDIT: your links -->
        <li><a href="mailto:you@example.com">Email <span>calvinmoras117@gmail.com</span></a></li>
        <li><a href="https://github.com/CryptokidFH">GitHub <span>@CryptokidFH</span></a></li>
        <li><a href="https://pypi.org/project/pyrava">PyPI <span>Pyrava</span></a></li>
        <li><a href="resume.pdf">Résumé <span>PDF</span></a></li>
      </ul>
    </div>
  </section>
</main>

<footer>© 2026 Your Name</footer>

<dialog class="lightbox" id="lightbox" aria-label="Photo viewer">
  <figure>
    <div class="lb-media"></div>
    <figcaption><span class="lb-cap"></span><span class="lb-count"></span></figcaption>
  </figure>
  <button class="lb-btn lb-close" type="button" aria-label="Close viewer"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M6 6l12 12M18 6L6 18"/></svg></button>
  <button class="lb-btn lb-prev" type="button" aria-label="Previous photo"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M15 5l-7 7 7 7"/></svg></button>
  <button class="lb-btn lb-next" type="button" aria-label="Next photo"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M9 5l7 7-7 7"/></svg></button>
</dialog>

<!-- Generated by prepare_photos.py. If it's missing, the gallery shows placeholder frames. -->
<script src="photos.js"></script>
<script>
(() => {
const root = document.documentElement;
const reduce = matchMedia('(prefers-reduced-motion: reduce)').matches;
const coarse = matchMedia('(hover: none)').matches;

/* ================= Laser stage ================= */
const hero = document.querySelector('.hero');
const cv = document.getElementById('beam');
const ctx = cv.getContext('2d', { alpha: false });
const HAZE = '22,19,43';
const BEAMS = 7;
const dirs = new Float32Array(BEAMS * 2);
let W = 0, H = 0, maxDpr = coarse ? 1.25 : 1.5, density = 5200;
let particles = [], color = [255, 74, 61];
let running = false, visible = true, raf = 0;
const aim = { x: 0, y: 0 }, cur = { x: 0, y: 0 };
let lastInput = -1e9;

const hexToRgb = h => { const n = parseInt(h.slice(1), 16); return [n >> 16 & 255, n >> 8 & 255, n & 255]; };

function seed() {
  const n = Math.min(280, Math.floor(W * H / density));
  particles = Array.from({ length: n }, () => ({
    x: Math.random() * W, y: Math.random() * H,
    vx: (Math.random() - .3) * .12, vy: (Math.random() - .5) * .06,
    s: Math.random() * 1.4 + .8, a: Math.random() * .6 + .4
  }));
}

function resize() {
  const r = hero.getBoundingClientRect();
  if (!r.width) return;
  W = r.width; H = r.height;
  const dpr = Math.min(window.devicePixelRatio || 1, maxDpr);
  cv.width = Math.round(W * dpr); cv.height = Math.round(H * dpr);
  ctx.setTransform(dpr, 0, 0, dpr, 0, 0);
  ctx.fillStyle = `rgb(${HAZE})`; ctx.fillRect(0, 0, W, H);
  seed();
  if (!cur.x) { cur.x = aim.x = W * .38; cur.y = aim.y = H * .3; }
  if (reduce || !running) draw(performance.now(), true);
}

function draw(t, still) {
  const ex = W + 6, ey = H * (W < 700 ? .24 : .36);   // projector mounted stage right, clear of the text
  if (!reduce && t - lastInput > 2200) {          // idle: slow scan, like a show running on its own
    aim.x = W * (.4 + .3 * Math.sin(t * .00029));
    aim.y = H * (.45 + .3 * Math.sin(t * .00043 + 1.3));
  }
  const k = (reduce || still) ? 1 : .075;
  cur.x += (aim.x - cur.x) * k; cur.y += (aim.y - cur.y) * k;

  ctx.globalCompositeOperation = 'source-over'; ctx.globalAlpha = 1;
  ctx.fillStyle = (reduce || still) ? `rgb(${HAZE})` : `rgba(${HAZE},.38)`;   // partial clear leaves a short persistence trail
  ctx.fillRect(0, 0, W, H);
  ctx.globalCompositeOperation = 'lighter';
  const c = color.join(',');

  const eg = ctx.createRadialGradient(ex, ey, 0, ex, ey, 160);
  eg.addColorStop(0, `rgba(${c},.5)`); eg.addColorStop(1, `rgba(${c},0)`);
  ctx.fillStyle = eg; ctx.fillRect(ex - 160, ey - 160, 320, 320);

  const base = Math.atan2(cur.y - ey, cur.x - ex);
  const spread = .17 + .1 * Math.sin(t * .0006);
  const L = Math.hypot(W, H) * 1.1;
  for (let i = 0; i < BEAMS; i++) {
    const a = base + (i / (BEAMS - 1) - .5) * 2 * spread;
    const dx = Math.cos(a), dy = Math.sin(a);
    dirs[i * 2] = dx; dirs[i * 2 + 1] = dy;
    const x2 = ex + dx * L, y2 = ey + dy * L;
    const gr = ctx.createLinearGradient(ex, ey, x2, y2);
    gr.addColorStop(0, `rgba(${c},1)`); gr.addColorStop(.45, `rgba(${c},.35)`); gr.addColorStop(1, `rgba(${c},0)`);
    ctx.strokeStyle = gr;
    ctx.beginPath(); ctx.moveTo(ex, ey); ctx.lineTo(x2, y2);
    ctx.lineWidth = 12;  ctx.globalAlpha = .05; ctx.stroke();
    ctx.lineWidth = 3.5; ctx.globalAlpha = .16; ctx.stroke();
    ctx.lineWidth = 1.1; ctx.globalAlpha = .9;  ctx.stroke();
  }

  // Haze particles light up when a beam passes through them
  ctx.fillStyle = `rgb(${c})`;
  for (const p of particles) {
    if (!reduce) {
      p.x += p.vx; p.y += p.vy;
      if (p.x > W) p.x = 0; else if (p.x < 0) p.x = W;
      if (p.y > H) p.y = 0; else if (p.y < 0) p.y = H;
    }
    const vx = p.x - ex, vy = p.y - ey;
    let lit = .05;
    for (let i = 0; i < BEAMS; i++) {
      const dx = dirs[i * 2], dy = dirs[i * 2 + 1];
      const along = vx * dx + vy * dy;
      if (along <= 0) continue;
      const perp = vx * dy - vy * dx, sig = 6 + along * .012;
      lit += Math.exp(-(perp * perp) / (2 * sig * sig)) * (1 - along / L);
    }
    ctx.globalAlpha = Math.min(1, lit) * p.a;
    ctx.fillRect(p.x, p.y, p.s, p.s);
  }
  ctx.globalAlpha = 1;
}

// Adaptive quality: if frames run long on a slow device, drop resolution and particle count once.
let last = 0, acc = 0, frames = 0, degraded = false;
function loop(t) {
  raf = requestAnimationFrame(loop);
  if (last && !degraded) {
    acc += t - last; frames++;
    if (frames === 90) {
      if (acc / frames > 22) { degraded = true; maxDpr = 1; density *= 2; resize(); }
      acc = 0; frames = 0;
    }
  }
  last = t;
  draw(t);
}
function setRunning() {
  const should = visible && !document.hidden && !reduce;
  if (should && !running) { running = true; last = 0; raf = requestAnimationFrame(loop); }
  else if (!should && running) { running = false; cancelAnimationFrame(raf); }
}

hero.addEventListener('pointermove', e => {
  const r = hero.getBoundingClientRect();
  aim.x = e.clientX - r.left; aim.y = e.clientY - r.top;
  lastInput = performance.now();
  if (reduce) requestAnimationFrame(() => draw(performance.now(), true));
}, { passive: true });

document.querySelectorAll('.nm button').forEach(btn => btn.addEventListener('click', () => {
  document.querySelectorAll('.nm button').forEach(b => b.setAttribute('aria-pressed', String(b === btn)));
  root.style.setProperty('--beam', btn.dataset.hex);
  color = hexToRgb(btn.dataset.hex);
  if (!running) draw(performance.now(), true);
}));

if (coarse) document.getElementById('hint').textContent = 'Drag sideways across the stage to aim the beam.';

new ResizeObserver(resize).observe(hero);
document.addEventListener('visibilitychange', setRunning);

/* ================= Navigation state ================= */
const nav = document.querySelector('.nav');
new IntersectionObserver(([e]) => {
  visible = e.isIntersecting;
  nav.classList.toggle('solid', e.intersectionRatio < .12);
  setRunning();
}, { threshold: [0, .12] }).observe(hero);

const navLinks = [...document.querySelectorAll('.nav ul a')];
const sectionObs = new IntersectionObserver(entries => {
  entries.forEach(e => {
    if (!e.isIntersecting) return;
    navLinks.forEach(a => a.setAttribute('aria-current', String(a.hash === '#' + e.target.id)));
  });
}, { rootMargin: '-45% 0px -50% 0px' });
document.querySelectorAll('main .section').forEach(s => sectionObs.observe(s));

/* ================= Clips play only while on screen ================= */
const clipObs = new IntersectionObserver(entries => {
  entries.forEach(e => {
    const v = e.target;
    if (e.isIntersecting && !reduce) v.play().catch(() => {});
    else v.pause();
  });
}, { threshold: .5 });
document.querySelectorAll('video[data-inview]').forEach(v => clipObs.observe(v));

/* ================= Copy pip command ================= */
document.querySelectorAll('[data-copy]').forEach(btn => btn.addEventListener('click', async () => {
  const label = btn.querySelector('span');
  try { await navigator.clipboard.writeText(btn.dataset.copy); label.textContent = 'Copied'; }
  catch { label.textContent = 'Select and copy'; }
  setTimeout(() => (label.textContent = 'Copy'), 1800);
}));

/* ================= Photographs ================= */
const grid = document.getElementById('gallery');
const data = (Array.isArray(window.PHOTOS) && window.PHOTOS.length) ? window.PHOTOS : demoPhotos();

function demoPhotos() {
  const ratios = [[3, 2], [2, 3], [4, 5], [16, 9], [1, 1], [3, 4], [3, 2], [4, 5]];
  const hues = [8, 262, 118, 300, 22, 212, 46, 160, 332, 248, 96, 4];
  return hues.map((h, i) => {
    const [w, hh] = ratios[i % ratios.length];
    return { demo: true, type: i % 5 === 3 ? 'video' : 'image', w, h: hh, hue: h,
             caption: 'Placeholder. Run prepare_photos.py to load your album.' };
  });
}
function demoBg(p) {
  const x = 15 + (p.hue * 7) % 65, y = 20 + (p.hue * 3) % 55;
  return `radial-gradient(ellipse at ${x}% ${y}%, hsl(${p.hue} 95% 62% / .95), transparent 55%),
          radial-gradient(circle at ${85 - p.hue % 45}% 85%, hsl(${(p.hue + 140) % 360} 90% 55% / .55), transparent 45%),
          linear-gradient(160deg, hsl(250 45% 15%), hsl(255 40% 7%))`;
}

const tiles = data.map((p, i) => {
  const b = document.createElement('button');
  b.type = 'button'; b.className = 'shot';
  b.style.aspectRatio = `${p.w} / ${p.h}`;
  b.setAttribute('aria-label', `Open ${p.alt || (p.type === 'video' ? 'clip' : 'photo') + ' ' + (i + 1)}`);
  let el;
  if (p.demo) {
    el = document.createElement('div'); el.className = 'demo'; el.style.background = demoBg(p);
  } else if (p.type === 'video') {
    el = document.createElement('video');
    el.muted = true; el.loop = true; el.playsInline = true; el.preload = 'none';
    if (p.poster) el.poster = p.poster;
    el.src = p.src;
    clipObs.observe(el);
  } else {
    el = document.createElement('img');
    el.loading = 'lazy'; el.decoding = 'async'; el.alt = p.alt || '';
    el.width = p.w; el.height = p.h; el.src = p.thumb;
  }
  b.append(el);
  if (p.type === 'video') { const c = document.createElement('span'); c.className = 'chip'; c.textContent = 'Clip'; b.append(c); }
  b.addEventListener('click', () => openViewer(i));
  return b;
});

// Masonry: place each frame in the shortest column. Aspect ratios are known, so no image needs to load first.
let cols = 0;
const mqA = matchMedia('(min-width: 560px)'), mqB = matchMedia('(min-width: 1024px)');
function layout() {
  const n = mqB.matches ? 4 : mqA.matches ? 3 : 2;
  if (n === cols) return; cols = n;
  const colEls = Array.from({ length: n }, () => { const d = document.createElement('div'); d.className = 'col'; return d; });
  const heights = new Array(n).fill(0);
  tiles.forEach((el, i) => {
    let m = 0; for (let j = 1; j < n; j++) if (heights[j] < heights[m] - .01) m = j;
    colEls[m].append(el); heights[m] += data[i].h / data[i].w;
  });
  grid.replaceChildren(...colEls);
}
mqA.addEventListener('change', layout); mqB.addEventListener('change', layout);
layout();

/* ================= Viewer ================= */
const lb = document.getElementById('lightbox');
const slot = lb.querySelector('.lb-media'), cap = lb.querySelector('.lb-cap'), count = lb.querySelector('.lb-count');
let idx = 0;
function show(i) {
  idx = (i + data.length) % data.length;
  const p = data[idx];
  let el;
  if (p.demo) {
    el = document.createElement('div'); el.className = 'demo';
    el.style.cssText = `aspect-ratio:${p.w}/${p.h};width:min(92vw, calc((100dvh - 9rem) * ${p.w / p.h}));border-radius:3px;background:${demoBg(p)}`;
  } else if (p.type === 'video') {
    el = document.createElement('video');
    el.controls = true; el.loop = true; el.playsInline = true; el.muted = true; el.autoplay = !reduce;
    if (p.poster) el.poster = p.poster; el.src = p.src;
  } else {
    el = document.createElement('img');
    el.decoding = 'async'; el.alt = p.alt || ''; el.src = p.full || p.thumb;
  }
  slot.replaceChildren(el);
  cap.textContent = p.caption || p.alt || '';
  count.textContent = `${idx + 1} of ${data.length}`;
  [1, -1].forEach(d => {                       // warm the cache for neighbours
    const q = data[(idx + d + data.length) % data.length];
    if (q && !q.demo && q.type !== 'video') new Image().src = q.full || q.thumb;
  });
}
function openViewer(i) { show(i); lb.showModal(); }
lb.querySelector('.lb-close').addEventListener('click', () => lb.close());
lb.querySelector('.lb-prev').addEventListener('click', () => show(idx - 1));
lb.querySelector('.lb-next').addEventListener('click', () => show(idx + 1));
lb.addEventListener('close', () => slot.replaceChildren());
lb.addEventListener('keydown', e => {
  if (e.key === 'ArrowRight') show(idx + 1);
  else if (e.key === 'ArrowLeft') show(idx - 1);
});
let sx = null, swiped = false;
lb.addEventListener('click', e => {
  if (swiped) { swiped = false; return; }        // a swipe shouldn't also count as a tap-to-close
  if (e.target === lb || e.target.tagName === 'FIGURE') lb.close();
});
lb.addEventListener('pointerdown', e => { if (e.target.tagName !== 'VIDEO') sx = e.clientX; });
lb.addEventListener('pointerup', e => {
  if (sx === null) return;
  const dx = e.clientX - sx; sx = null;
  if (Math.abs(dx) > 50) { swiped = true; show(idx + (dx < 0 ? 1 : -1)); }
});
})();
</script>
</body>
</html>
