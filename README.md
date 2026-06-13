<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Generador de ideas de proyecto</title>
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@tabler/icons-webfont@latest/dist/tabler-icons.min.css" />
  <style>
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    :root {
      --bg-primary: #ffffff;
      --bg-secondary: #f5f5f4;
      --bg-info: #e6f1fb;
      --text-primary: #1a1a1a;
      --text-secondary: #6b7280;
      --text-tertiary: #9ca3af;
      --text-info: #185fa5;
      --border: rgba(0,0,0,0.12);
      --border-hover: rgba(0,0,0,0.25);
      --border-info: #185fa5;
      --radius-md: 8px;
      --radius-lg: 12px;
    }

    @media (prefers-color-scheme: dark) {
      :root {
        --bg-primary: #1c1c1e;
        --bg-secondary: #2c2c2e;
        --bg-info: #0c2340;
        --text-primary: #f2f2f7;
        --text-secondary: #aeaeb2;
        --text-tertiary: #636366;
        --text-info: #85b7eb;
        --border: rgba(255,255,255,0.1);
        --border-hover: rgba(255,255,255,0.25);
        --border-info: #378add;
      }
    }

    body {
      font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
      background: url('img/fondo.jpg') center center / cover no-repeat fixed;
      color: var(--text-primary);
      min-height: 100vh;
      padding: 2rem 1rem;
    }

    .app {
      max-width: 720px;
      margin: 0 auto;
    }

    .header { margin-bottom: 1.5rem; }
    .header h1 {
      font-size: 22px; font-weight: 500;
      color: var(--text-primary);
      display: flex; align-items: center; gap: 10px;
    }
    .header h1 i { font-size: 22px; color: var(--text-info); }
    .header p { font-size: 14px; color: var(--text-secondary); margin-top: 4px; }

    .filter-row { display: flex; gap: 8px; flex-wrap: wrap; margin-bottom: 1.25rem; }
    .filter-btn {
      padding: 5px 14px; border-radius: 99px; font-size: 13px; cursor: pointer;
      border: 0.5px solid var(--border);
      background: var(--bg-primary);
      color: var(--text-secondary);
      transition: background 0.15s, color 0.15s;
    }
    .filter-btn.active {
      background: var(--bg-info);
      color: var(--text-info);
      border-color: transparent;
    }

    .idea-card {
      background: var(--bg-primary);
      border: 0.5px solid var(--border);
      border-radius: var(--radius-lg);
      padding: 1.5rem;
      margin-bottom: 1rem;
      min-height: 140px;
      display: flex;
      flex-direction: column;
      justify-content: space-between;
      transition: border-color 0.2s;
    }
    .idea-card.active { border-color: var(--border-info); }

    .idea-top { display: flex; align-items: flex-start; gap: 12px; }
    .idea-icon {
      width: 80px; height: 80px; border-radius: var(--radius-md);
      background: var(--bg-secondary);
      display: flex; align-items: center; justify-content: center;
      flex-shrink: 0;
      overflow: hidden;
    }
    .idea-icon img { width: 100%; height: 100%; object-fit: contain; border-radius: var(--radius-md); }
    .idea-icon i { font-size: 20px; color: var(--text-info); }
    .idea-title { font-size: 17px; font-weight: 500; color: var(--text-primary); line-height: 1.4; }
    .idea-desc { font-size: 14px; color: var(--text-secondary); margin-top: 6px; line-height: 1.6; }
    .idea-tags { display: flex; gap: 6px; flex-wrap: wrap; margin-top: 12px; }
    .tag {
      font-size: 12px; padding: 3px 10px; border-radius: 99px;
      background: var(--bg-secondary);
      color: var(--text-secondary);
      border: 0.5px solid var(--border);
    }
    .tag.cat {
      background: var(--bg-info);
      color: var(--text-info);
      border-color: transparent;
    }

    .controls { display: flex; gap: 10px; margin-bottom: 1.5rem; flex-wrap: wrap; }
    .btn-main {
      flex: 1; min-width: 160px; padding: 10px 20px;
      background: var(--bg-primary);
      border: 0.5px solid var(--border-hover);
      border-radius: var(--radius-md);
      color: var(--text-primary);
      font-size: 14px; cursor: pointer;
      display: flex; align-items: center; justify-content: center; gap: 8px;
      transition: background 0.15s;
    }
    .btn-main:hover { background: var(--bg-secondary); }
    .btn-main:active { transform: scale(0.98); }
    .btn-add {
      padding: 10px 16px;
      background: var(--bg-primary);
      border: 0.5px solid var(--border-hover);
      border-radius: var(--radius-md);
      color: var(--text-secondary);
      font-size: 14px; cursor: pointer;
      display: flex; align-items: center; gap: 6px;
      transition: background 0.15s;
    }
    .btn-add:hover { background: var(--bg-secondary); }

    .section-title {
      font-size: 13px; font-weight: 500; color: var(--text-secondary);
      text-transform: uppercase; letter-spacing: 0.05em; margin-bottom: 10px;
    }

    .schedule-grid {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(180px, 1fr));
      gap: 10px;
      margin-bottom: 1.5rem;
    }
    .schedule-slot {
      background: var(--bg-primary);
      border: 0.5px solid var(--border);
      border-radius: var(--radius-md);
      padding: 12px;
      min-height: 80px;
      cursor: pointer;
      transition: border-color 0.15s;
      position: relative;
    }
    .schedule-slot:hover { border-color: var(--border-hover); }
    .schedule-slot.filled { border-color: var(--border-info); }
    .slot-label { font-size: 11px; color: var(--text-tertiary); font-weight: 500; margin-bottom: 6px; }
    .slot-content { font-size: 13px; color: var(--text-primary); line-height: 1.4; }
    .slot-empty { font-size: 13px; color: var(--text-tertiary); font-style: italic; }
    .slot-remove {
      position: absolute; top: 8px; right: 8px;
      background: none; border: none; cursor: pointer;
      color: var(--text-tertiary); font-size: 14px;
      display: none;
    }
    .schedule-slot.filled:hover .slot-remove { display: block; }

    .add-form {
      background: var(--bg-secondary);
      border-radius: var(--radius-md);
      padding: 1rem;
      margin-bottom: 1.5rem;
      display: none;
    }
    .add-form.open { display: block; }
    .add-form input, .add-form select, .add-form textarea {
      width: 100%; margin-bottom: 8px; font-size: 14px;
      background: var(--bg-primary); color: var(--text-primary);
      border: 0.5px solid var(--border); border-radius: var(--radius-md);
      padding: 8px 12px; outline: none;
    }
    .add-form textarea { resize: vertical; min-height: 60px; font-family: inherit; }
    .add-form input:focus, .add-form select:focus, .add-form textarea:focus {
      border-color: var(--border-info);
    }
    .form-row { display: flex; gap: 8px; }
    .form-row input, .form-row select { margin-bottom: 0; }

    .saved-item {
      background: var(--bg-primary);
      border: 0.5px solid var(--border);
      border-radius: var(--radius-md);
      padding: 10px 14px;
      display: flex; align-items: center; gap: 10px;
      margin-bottom: 8px;
    }
    .saved-item i { font-size: 18px; color: var(--text-info); flex-shrink: 0; }
    .saved-item .info { flex: 1; }
    .saved-item .info .title { font-size: 14px; font-weight: 500; color: var(--text-primary); }
    .saved-item .info .sub { font-size: 12px; color: var(--text-secondary); }
    .saved-item button {
      background: none; border: 0.5px solid var(--border);
      border-radius: var(--radius-md); padding: 5px 10px;
      font-size: 12px; cursor: pointer; color: var(--text-secondary);
    }
    .saved-item button:hover { background: var(--bg-secondary); }

    .empty-state { text-align: center; padding: 2rem; color: var(--text-tertiary); font-size: 14px; }

    @keyframes pop { 0%{transform:scale(0.97);opacity:0.6} 100%{transform:scale(1);opacity:1} }
    .spinning { animation: pop 0.35s ease; }
  </style>
</head>
<body>
<div class="app">

  <div class="filter-row" id="filterRow">
    <button class="filter-btn active" onclick="setFilter('Todos')">Todos</button>
    <button class="filter-btn" onclick="setFilter('Sonic')">Sonic</button>
    <button class="filter-btn" onclick="setFilter('Pokemon')">Pokemon</button>
    <button class="filter-btn" onclick="setFilter('Paleontologia')">Paleontologia</button>
    <button class="filter-btn" onclick="setFilter('Writing')">Writing</button>
    <button class="filter-btn" onclick="setFilter('Reading')">Reading</button>
    <button class="filter-btn" onclick="setFilter('Walking')">Walking</button>
  </div>

  <div class="idea-card active" id="ideaCard">
    <div>
      <div class="idea-top">
        <div class="idea-icon" id="ideaIconWrapper" style="display:none;"><i class="ti ti-wand" id="ideaIcon"></i></div>
        <div>
          <div class="idea-title" id="ideaTitle">Agrega tu idea cara</div>
          <div class="idea-desc" id="ideaDesc">Usa el botón "Mi idea" para agregar tu idea amor.</div>
        </div>
      </div>
    </div>
    <div class="idea-tags" id="ideaTags" style="display:none;"></div>
  </div>

  <div class="controls">
    <button class="btn-main" onclick="toggleForm()">
      <i class="ti ti-plus"></i> Mi idea
    </button>
    <button class="btn-add" onclick="randomIdea()">
      <i class="ti ti-refresh"></i> RANDOM OMG
    </button>
    <button class="btn-add" onclick="addToSchedule()" id="btnSchedule">
      <i class="ti ti-calendar-plus"></i> Agregar al cara horario
    </button>
  </div>

  <div class="add-form" id="addForm">
    <div class="form-row" style="margin-bottom:8px;">
      <input type="text" id="newTitle" placeholder="Nombre de tu idea..." style="flex:2;" />
      <select id="newCat" style="flex:1;">
        <option>Sonic</option><option>Pokemon</option><option>Paleontologia</option>
        <option>Writing</option><option>Reading</option><option>Walking</option>
      </select>
    </div>
    <textarea id="newDesc" placeholder="Descripción breve..."></textarea>
    <div class="form-row">
      <input type="text" id="newTags" placeholder="Etiquetas separadas por coma..." style="flex:1;" />
      <button class="btn-add" onclick="saveCustomIdea()"><i class="ti ti-check"></i> Guardar</button>
    </div>
  </div>

  <div class="section-title">Cara Horario </div>
  <div class="schedule-grid" id="scheduleGrid"></div>

  <div class="section-title" style="margin-top:0.5rem;">Guardadas (<span id="savedCount">0</span>)</div>
  <div id="savedList"><div class="empty-state">Ninguna idea guardada todavía.</div></div>

</div>

<script>
const ideas = [];

let allIdeas = [...ideas];
let currentIdea = null;
let currentFilter = 'Todos';
const days = ['Lunes','Martes','Miércoles','Jueves','Viernes','Sábado','Domingo'];
const schedule = {};
const saved = [];

function setFilter(f) {
  currentFilter = f;
  document.querySelectorAll('.filter-btn').forEach(b => b.classList.toggle('active', b.textContent === f));
  const pool = currentFilter === 'Todos' ? allIdeas : allIdeas.filter(i => i.cat === currentFilter);
  if (pool.length) randomIdea();
}

function randomIdea() {
  const pool = currentFilter === 'Todos' ? allIdeas : allIdeas.filter(i => i.cat === currentFilter);
  if (!pool.length) { alert('No hay ideas en esta categoría.'); return; }
  const card = document.getElementById('ideaCard');
  card.classList.add('spinning');
  setTimeout(() => card.classList.remove('spinning'), 350);
  currentIdea = pool[Math.floor(Math.random() * pool.length)];
  renderIdea(currentIdea);
  document.getElementById('btnSchedule').style.display = 'flex';
}

const catImages = {
  'Sonic': 'img/sonic.png',
  'Pokemon': 'img/pokemon.png',
  'Paleontologia': 'img/paleontologia.png',
  'Writing': 'img/writing.png',
  'Reading': 'img/reading.png',
  'Walking': 'img/walking.png',
};

function renderIdea(idea) {
  document.getElementById('ideaTitle').textContent = idea.title;
  document.getElementById('ideaDesc').textContent = idea.desc;
  const wrapper = document.getElementById('ideaIconWrapper');
  const imgSrc = catImages[idea.cat];
  wrapper.style.display = 'flex';
  if (imgSrc) {
    wrapper.innerHTML = `<img src="${imgSrc}" alt="${idea.cat}" style="width:100%;height:100%;object-fit:cover;border-radius:var(--radius-md);" />`;
  } else {
    wrapper.innerHTML = `<i class="ti ${idea.icon || 'ti-bulb'}" style="font-size:20px;color:var(--text-info);"></i>`;
  }
  const tagsEl = document.getElementById('ideaTags');
  tagsEl.innerHTML = `<span class="tag cat">${idea.cat}</span>` +
    (idea.tags || []).map(t => `<span class="tag">${t}</span>`).join('');
}

function addToSchedule() {
  if (!currentIdea) return;
  const day = prompt(`¿A qué día del horario quieres agregar "${currentIdea.title}"?\n\n${days.map((d,i)=>`${i+1}. ${d}`).join('\n')}\n\nEscribe el número:`);
  const idx = parseInt(day) - 1;
  if (isNaN(idx) || idx < 0 || idx > 6) { alert('Número de día inválido.'); return; }
  schedule[days[idx]] = { ...currentIdea };
  renderSchedule();
  if (!saved.find(s => s.title === currentIdea.title)) {
    saved.push({ ...currentIdea });
    renderSaved();
  }
}

function renderSchedule() {
  const grid = document.getElementById('scheduleGrid');
  grid.innerHTML = days.map(day => {
    const s = schedule[day];
    return `<div class="schedule-slot ${s ? 'filled' : ''}" onclick="slotClick('${day}')">
      <div class="slot-label">${day}</div>
      ${s
        ? `<div class="slot-content"><img src="${catImages[s.cat] || ''}" alt="${s.cat}" style="width:20px;height:20px;object-fit:contain;border-radius:4px;margin-right:6px;vertical-align:-5px;">${s.title}</div>
           ${s.desc ? `<div style="font-size:12px;color:var(--text-secondary);margin-top:4px;line-height:1.4;">${s.desc}</div>` : ''}
           <button class="slot-remove" onclick="removeSlot(event,'${day}')" title="Quitar"><i class="ti ti-x"></i></button>`
        : `<div class="slot-empty">Sin idea aún</div>`
      }
    </div>`;
  }).join('');
}

function slotClick(day) {
  if (schedule[day]) return;
  if (!currentIdea) { alert('Primero genera una idea con el botón "Nueva idea".'); return; }
  schedule[day] = { ...currentIdea };
  renderSchedule();
  if (!saved.find(s => s.title === currentIdea.title)) {
    saved.push({ ...currentIdea });
    renderSaved();
  }
}

function removeSlot(e, day) {
  e.stopPropagation();
  delete schedule[day];
  renderSchedule();
}

function renderSaved() {
  document.getElementById('savedCount').textContent = saved.length;
  const el = document.getElementById('savedList');
  if (!saved.length) { el.innerHTML = '<div class="empty-state">Ninguna idea guardada todavía.</div>'; return; }
  el.innerHTML = saved.map((s, i) => `
    <div class="saved-item">
      <img src="${catImages[s.cat] || ''}" alt="${s.cat}" style="width:32px;height:32px;object-fit:cover;border-radius:6px;flex-shrink:0;" />
      <div class="info">
        <div class="title">${s.title}</div>
        <div class="sub">${s.cat} · ${(s.tags || []).slice(0, 2).join(', ')}</div>
      </div>
      <button onclick="pickSaved(${i})">Ver</button>
    </div>`).join('');
}

function pickSaved(i) {
  currentIdea = saved[i];
  renderIdea(currentIdea);
  document.getElementById('btnSchedule').style.display = 'flex';
}

function toggleForm() {
  document.getElementById('addForm').classList.toggle('open');
}

function saveCustomIdea() {
  const title = document.getElementById('newTitle').value.trim();
  const desc = document.getElementById('newDesc').value.trim();
  const cat = document.getElementById('newCat').value;
  const tags = document.getElementById('newTags').value.split(',').map(t => t.trim()).filter(Boolean);
  if (!title) { alert('Escribe un nombre para tu idea.'); return; }
  const idea = { title, desc, cat, tags, icon: 'ti-bulb' };
  allIdeas.push(idea);
  currentIdea = idea;
  renderIdea(idea);
  document.getElementById('btnSchedule').style.display = 'flex';
  document.getElementById('newTitle').value = '';
  document.getElementById('newDesc').value = '';
  document.getElementById('newTags').value = '';
  document.getElementById('addForm').classList.remove('open');
  if (!saved.find(s => s.title === title)) { saved.push(idea); renderSaved(); }
}

renderSchedule();
renderSaved();
</script>
</body>
</html>
