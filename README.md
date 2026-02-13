
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Weekly Gym Planner</title>
  <style>
    :root{--bg:#0f1724;--card:#0b1220;--muted:#9aa4b2;--accent:#00d4ff;--accent2:#ff006e;--success:#00ff88;--warn:#ffa500}
    *{box-sizing:border-box;font-family:'Inter','Segoe UI',sans-serif}
    body{margin:0;background:linear-gradient(135deg,#0a0e27 0%,#1a1f3a 100%);color:#e6eef8;min-height:100vh;padding:20px}
    .wrap{max-width:1200px;margin:0 auto;display:grid;grid-template-columns:280px 1fr;gap:20px}
    header{grid-column:1/-1;margin-bottom:12px}
    header h1{font-size:22px;margin:0;background:linear-gradient(90deg,var(--accent),var(--accent2));-webkit-background-clip:text;-webkit-text-fill-color:transparent}
    .week-selector{grid-column:1;padding:16px;background:linear-gradient(135deg,rgba(0,212,255,0.1),rgba(255,0,110,0.1));border:1px solid rgba(0,212,255,0.2);border-radius:12px;margin-bottom:16px}
    .week-selector h3{margin:0 0 12px;font-size:13px;text-transform:uppercase;color:var(--accent);letter-spacing:1px}
    .week-btn{display:flex;align-items:center;justify-content:space-between;padding:10px 12px;margin:6px 0;background:rgba(255,255,255,0.02);border:1px solid rgba(0,212,255,0.3);border-radius:8px;cursor:pointer;transition:all 0.2s}
    .week-btn:hover{background:rgba(0,212,255,0.1);border-color:var(--accent)}
    .week-btn.active{background:linear-gradient(90deg,rgba(0,212,255,0.2),rgba(255,0,110,0.1));border-color:var(--accent)}
    .panel{background:linear-gradient(180deg,rgba(255,255,255,0.02),transparent);border:1px solid rgba(0,212,255,0.15);padding:16px;border-radius:12px}
    .days-grid{display:grid;grid-template-columns:1fr;gap:10px}
    .day-section{background:rgba(0,212,255,0.05);border:1px solid rgba(0,212,255,0.1);border-radius:10px;padding:12px;cursor:pointer;transition:all 0.2s}
    .day-section:hover{border-color:var(--accent);box-shadow:0 0 16px rgba(0,212,255,0.1)}
    .day-section.active{background:linear-gradient(90deg,rgba(0,212,255,0.15),rgba(255,0,110,0.1));border-color:var(--accent)}
    .day-title{font-weight:700;color:var(--accent);font-size:14px;margin:0 0 6px;text-transform:uppercase}
    .day-meta{color:var(--muted);font-size:12px}
    .content{display:flex;flex-direction:column;gap:14px}
    .ex-list{display:flex;flex-direction:column;gap:10px;max-height:65vh;overflow:auto;padding-right:8px}
    .exercise{display:grid;grid-template-columns:1fr 70px 70px 50px;gap:10px;align-items:center;padding:12px;border-radius:10px;background:rgba(0,212,255,0.03);border:1px solid rgba(0,212,255,0.08);transition:all 0.2s}
    .exercise:hover{background:rgba(0,212,255,0.06);border-color:var(--accent)}
    .exercise.done{background:linear-gradient(90deg,rgba(0,255,136,0.1),rgba(0,255,136,0.05));border-color:var(--success)}
    .exercise .title{font-weight:600;color:#fff}
    .exercise .meta{color:var(--muted);font-size:12px;margin-top:4px}
    .col-header{color:var(--accent);font-size:11px;text-transform:uppercase;font-weight:600;margin-bottom:4px}
    .muted{color:var(--muted);font-size:13px}
    input,select{background:rgba(255,255,255,0.03);border:1px solid rgba(0,212,255,0.2);padding:10px;border-radius:8px;color:#e6eef8;transition:all 0.2s}
    input:focus,select:focus{outline:none;background:rgba(0,212,255,0.1);border-color:var(--accent)}
    button{background:transparent;border:1px solid rgba(0,212,255,0.3);color:inherit;padding:8px 12px;border-radius:8px;cursor:pointer;transition:all 0.2s;font-size:12px}
    button:hover{border-color:var(--accent);background:rgba(0,212,255,0.1)}
    button.primary{background:linear-gradient(90deg,var(--accent),var(--accent2));border:0;color:#000;font-weight:600}
    button.primary:hover{transform:translateY(-2px);box-shadow:0 8px 16px rgba(0,212,255,0.3)}
    button.small{padding:6px 8px;font-size:11px}
    button.icon{padding:6px;width:36px;height:36px;display:flex;align-items:center;justify-content:center}
    .add-exercise{display:flex;gap:8px;flex-wrap:wrap;align-items:flex-end}
    .add-exercise input{flex:1;min-width:120px}
    .add-exercise input.short{flex:0;width:80px}
    .modal{display:none;position:fixed;top:0;left:0;right:0;bottom:0;background:rgba(0,0,0,0.7);z-index:100;align-items:center;justify-content:center}
    .modal.show{display:flex}
    .modal-content{background:linear-gradient(135deg,#0a1428,#1a2847);border:1px solid rgba(0,212,255,0.3);border-radius:16px;padding:24px;max-width:480px;width:90%}
    .modal-content h3{margin:0 0 16px;color:var(--accent)}
    .modal-content input{width:100%;margin-bottom:12px}
    .modal-buttons{display:flex;gap:8px;margin-top:20px}
    .modal-buttons button{flex:1}
    footer{grid-column:1/-1;text-align:center;color:var(--muted);font-size:12px;margin-top:20px;padding-top:20px;border-top:1px solid rgba(0,212,255,0.1)}
    @media (max-width:900px){.wrap{grid-template-columns:1fr}.ex-list{max-height:auto}}
    ::-webkit-scrollbar{width:6px}
    ::-webkit-scrollbar-track{background:transparent}
    header{position:relative} 
    .login-btn{position:absolute;right:12px;top:12px;padding:8px 12px;border-radius:8px;border:1px solid rgba(0,212,255,0.3);background:transparent;color:inherit;cursor:pointer;font-size:13px}
    .login-badge{display:inline-flex;align-items:center;gap:8px;padding:6px 10px;border-radius:8px;background:rgba(255,255,255,0.02);border:1px solid rgba(0,212,255,0.12)}
    /* auth modal will reuse .modal and .modal-content rules above */
    ::-webkit-scrollbar-thumb{background:rgba(0,212,255,0.3);border-radius:3px}
    ::-webkit-scrollbar-thumb:hover{background:rgba(0,212,255,0.5)}
  </style>
</head>
<body>
  <div class="wrap">
    <header>
      <h1>Weekly Planner</h1>
    </header>

    <aside class="panel" style="grid-column:1;grid-row:2">
      <div class="week-selector">
        <h3>📅 Select Week</h3>
        <div id="weekBtns"></div>
      </div>

      <div class="days-grid" id="daysList"></div>
    </aside>

    <main class="panel content">
      <div>
        <h2 id="dayTitle" style="margin:0;color:var(--accent)">Monday</h2>
        <div class="muted" id="dayMeta">Select a day</div>
      </div>

      <div style="display:flex;gap:8px;flex-wrap:wrap;align-items:flex-end" class="add-exercise">
        <input id="exName" placeholder="Exercise name" style="flex:1;min-width:150px" />
        <input id="exSets" placeholder="Sets" class="short" />
        <input id="exReps" placeholder="Reps" class="short" />
        <input id="exDuration" placeholder="Duration (sec)" class="short" />
        <button id="addExBtn" class="primary">+ Add</button>
      </div>

      <div class="ex-list" id="exList"></div>

      <div style="display:flex;gap:8px;justify-content:space-between;align-items:center">
        <div class="muted" id="progressText">0 / 0 completed</div>
        <div style="display:flex;gap:8px">
          <button id="shuffleBtn">🔀 Shuffle</button>
          <button id="resetBtn">↻ Reset</button>
          <button id="clearBtn">🗑 Clear Done</button>
        </div>
      </div>
    </main>

    <footer>Saved locally • Built for your gains 💪</footer>
  </div>

  <!-- Edit Exercise Modal -->
  <div class="modal" id="editModal">
    <div class="modal-content">
      <h3>Edit Exercise</h3>
      <input id="editName" placeholder="Exercise name" />
      <input id="editSets" placeholder="Sets" type="number" />
      <input id="editReps" placeholder="Reps" type="number" />
      <input id="editDuration" placeholder="Duration (sec)" type="number" />
      <div class="modal-buttons">
        <button id="editSave" class="primary">Save</button>
        <button id="editCancel">Cancel</button>
      </div>
    </div>
  </div>

  <script>
    const STORAGE_KEY = 'weekly_planner_v2'
    const DAYS = ['Monday','Tuesday','Wednesday','Thursday','Friday','Saturday','Sunday']
    let state = { weeks: [], currentWeek: 0, currentDay: 0 }
    let editingExId = null, editingExDay = null

    const uid = () => Date.now().toString(36) + Math.random().toString(36).slice(2,8)
    const qs = s => document.querySelector(s)
    const qsa = s => Array.from(document.querySelectorAll(s))

    // DOM
    const daysList = qs('#daysList')
    const dayTitle = qs('#dayTitle')
    const dayMeta = qs('#dayMeta')
    const exListEl = qs('#exList')
    const progressText = qs('#progressText')
    const exName = qs('#exName')
    const exSets = qs('#exSets')
    const exReps = qs('#exReps')
    const exDuration = qs('#exDuration')
    const addExBtn = qs('#addExBtn')
    const shuffleBtn = qs('#shuffleBtn')
    const resetBtn = qs('#resetBtn')
    const clearBtn = qs('#clearBtn')
    const weekBtns = qs('#weekBtns')
    const editModal = qs('#editModal')

    function createWeek() {
      return { id: uid(), days: DAYS.map(name => ({ id: uid(), name, exercises: [] })) }
    }

    function load() {
      try {
        const raw = localStorage.getItem(STORAGE_KEY)
        if (raw) state = JSON.parse(raw)
      } catch(e) { console.warn('load failed', e) }
      if (!state.weeks || !state.weeks.length) {
        state.weeks = [createWeek()]
        state.currentWeek = 0
        state.currentDay = 0
        save()
      }
      render()
    }

    function save() {
      localStorage.setItem(STORAGE_KEY, JSON.stringify(state))
    }

    function render() {
      renderWeekSelector()
      renderDays()
      renderExercises()
    }

    function renderWeekSelector() {
      weekBtns.innerHTML = ''
      state.weeks.forEach((w, i) => {
        const btn = document.createElement('div')
        btn.className = 'week-btn' + (i === state.currentWeek ? ' active' : '')
        btn.innerHTML = `<span>Week ${i + 1}</span><button class="small" data-idx="${i}" data-action="delweek">✕</button>`
        btn.onclick = () => { state.currentWeek = i; state.currentDay = 0; render() }
        weekBtns.appendChild(btn)
      })
      const addBtn = document.createElement('div')
      addBtn.className = 'week-btn'
      addBtn.style.justifyContent = 'center'
      addBtn.textContent = '+ Add Week'
      addBtn.onclick = () => { state.weeks.push(createWeek()); state.currentWeek = state.weeks.length - 1; render() }
      weekBtns.appendChild(addBtn)
    }

    function renderDays() {
      const week = state.weeks[state.currentWeek]
      daysList.innerHTML = ''
      week.days.forEach((day, i) => {
        const complete = day.exercises.filter(e => e.completed).length
        const el = document.createElement('div')
        el.className = 'day-section' + (i === state.currentDay ? ' active' : '')
        el.innerHTML = `<div class="day-title">${day.name}</div><div class="day-meta">${complete}/${day.exercises.length} done</div>`
        el.onclick = () => { state.currentDay = i; render() }
        daysList.appendChild(el)
      })
    }

    function renderExercises() {
      const week = state.weeks[state.currentWeek]
      const day = week.days[state.currentDay]
      dayTitle.textContent = day.name
      dayMeta.textContent = `${day.exercises.length} exercises`
      exListEl.innerHTML = ''

      day.exercises.forEach(ex => {
        const el = document.createElement('div')
        el.className = 'exercise' + (ex.completed ? ' done' : '')
        el.dataset.id = ex.id
        el.innerHTML = `
          <div>
            <div class="title">${escapeHtml(ex.name)}</div>
            <div class="meta">${ex.sets ? ex.sets + ' sets' : ''}${ex.sets && ex.reps ? ' • ' : ''}${ex.reps ? ex.reps + ' reps' : ''}${ex.duration ? ' • ' + ex.duration + 's' : ''}</div>
          </div>
          <div style="text-align:center"><div class="col-header">Sets</div><div>${ex.sets||'-'}</div></div>
          <div style="text-align:center"><div class="col-header">Reps</div><div>${ex.reps||'-'}</div></div>
          <div style="display:flex;gap:4px;justify-content:center">
            <button class="icon" data-action="toggle" title="Toggle">${ex.completed ? '✓' : '○'}</button>
            <button class="icon" data-action="edit" title="Edit">✎</button>
            <button class="icon" data-action="delete" title="Delete">✕</button>
          </div>`
        exListEl.appendChild(el)
      })

      const done = day.exercises.filter(e => e.completed).length
      progressText.textContent = `${done} / ${day.exercises.length} completed`
      save()
    }

    function escapeHtml(s){ return String(s).replace(/[&<>"']/g, c => ({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'})[c]) }

    // Events
    addExBtn.addEventListener('click', () => {
      const week = state.weeks[state.currentWeek]
      const day = week.days[state.currentDay]
      const name = exName.value.trim()
      if (!name) return alert('Enter exercise name')
      day.exercises.push({
        id: uid(),
        name: name,
        sets: parseInt(exSets.value) || 0,
        reps: parseInt(exReps.value) || 0,
        duration: parseInt(exDuration.value) || 0,
        completed: false
      })
      exName.value = exSets.value = exReps.value = exDuration.value = ''
      render()
    })

    exListEl.addEventListener('click', e => {
      const exEl = e.target.closest('.exercise')
      if (!exEl) return
      const exId = exEl.dataset.id
      const action = e.target.dataset.action
      const week = state.weeks[state.currentWeek]
      const day = week.days[state.currentDay]
      const ex = day.exercises.find(x => x.id === exId)
      if (!ex) return

      if (action === 'toggle') { ex.completed = !ex.completed; render(); return }
      if (action === 'delete') {
        if (!confirm('Delete exercise?')) return
        day.exercises = day.exercises.filter(x => x.id !== exId)
        render()
        return
      }
      if (action === 'edit') {
        editingExId = exId
        editingExDay = state.currentDay
        qs('#editName').value = ex.name
        qs('#editSets').value = ex.sets
        qs('#editReps').value = ex.reps
        qs('#editDuration').value = ex.duration
        editModal.classList.add('show')
      }
    })

    qs('#editSave').addEventListener('click', () => {
      const week = state.weeks[state.currentWeek]
      const day = week.days[editingExDay]
      const ex = day.exercises.find(x => x.id === editingExId)
      if (ex) {
        ex.name = qs('#editName').value.trim()
        ex.sets = parseInt(qs('#editSets').value) || 0
        ex.reps = parseInt(qs('#editReps').value) || 0
        ex.duration = parseInt(qs('#editDuration').value) || 0
      }
      editModal.classList.remove('show')
      render()
    })

    qs('#editCancel').addEventListener('click', () => {
      editModal.classList.remove('show')
    })

    shuffleBtn.addEventListener('click', () => {
      const week = state.weeks[state.currentWeek]
      const day = week.days[state.currentDay]
      for (let i = day.exercises.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [day.exercises[i], day.exercises[j]] = [day.exercises[j], day.exercises[i]]
      }
      render()
    })

    resetBtn.addEventListener('click', () => {
      const week = state.weeks[state.currentWeek]
      const day = week.days[state.currentDay]
      day.exercises.forEach(e => e.completed = false)
      render()
    })

    clearBtn.addEventListener('click', () => {
      const week = state.weeks[state.currentWeek]
      const day = week.days[state.currentDay]
      day.exercises = day.exercises.filter(e => !e.completed)
      render()
    })

    window.addEventListener('beforeunload', save)
    load()
  </script>
</body>
</html>
<div class="ai-section" style="margin-top:16px;padding:16px;background:rgba(255,165,0,0.1);border:1px solid rgba(255,165,0,0.3);border-radius:12px">
  <h3 style="margin:0 0 12px;color:var(--warn);font-size:13px;text-transform:uppercase">🤖 AI Planner</h3>
  <div style="display:flex;gap:8px;flex-wrap:wrap;margin-bottom:12px">
    <select id="aiGoal" style="flex:1;min-width:120px">
      <option value="">Select goal</option>
      <option value="strength">Strength Building</option>
      <option value="endurance">Endurance</option>
      <option value="hypertrophy">Muscle Growth</option>
      <option value="cardio">Cardio Fitness</option>
      <option value="flexibility">Flexibility</option>
    </select>
    <input id="aiDifficulty" type="range" min="1" max="5" value="3" style="flex:0;width:80px" title="Difficulty" />
  </div>
  <button id="aiGenerateBtn" class="primary" style="width:100%">Generate Plan</button>
</div>
