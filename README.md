
[Untitled-1.html](https://github.com/user-attachments/files/25292737/Untitled-1.html)
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width,initial-scale=1" />
    <title>Daily Routine & Gym Planner</title>
    <style>
        :root{--bg:#0f1724;--card:#0b1220;--muted:#9aa4b2;--accent:#60a5fa;--success:#22c55e}
        *{box-sizing:border-box;font-family:Inter,ui-sans-serif,system-ui,Segoe UI,Roboto,Helvetica,Arial}
        body{margin:0;background:linear-gradient(180deg,#071028 0%, #071a2b 100%);color:#e6eef8;min-height:100vh;padding:20px}
        .wrap{max-width:1000px;margin:0 auto;display:grid;grid-template-columns:260px 1fr;gap:18px}
        header{grid-column:1/-1;display:flex;align-items:center;justify-content:space-between;margin-bottom:6px}
        header h1{font-size:18px;margin:0}
        header .meta{color:var(--muted);font-size:13px}
        .panel{background:linear-gradient(180deg,rgba(255,255,255,0.02),transparent);border:1px solid rgba(255,255,255,0.03);padding:14px;border-radius:10px}
        .sidebar{display:flex;flex-direction:column;gap:12px}
        .routines{display:flex;flex-direction:column;gap:8px;max-height:56vh;overflow:auto;padding-right:6px}
        .routine-item{display:flex;align-items:center;justify-content:space-between;padding:8px 10px;border-radius:8px;cursor:pointer}
        .routine-item.active{background:linear-gradient(90deg,rgba(96,165,250,0.12),rgba(96,165,250,0.06));box-shadow:0 6px 14px rgba(10,26,40,0.25)}
        .routine-item small{color:var(--muted);font-size:12px}
        .controls{display:flex;gap:8px}
        button{background:transparent;border:1px solid rgba(255,255,255,0.06);color:inherit;padding:8px 10px;border-radius:8px;cursor:pointer}
        button.primary{background:var(--accent);border:0;color:#04263b;font-weight:600}
        .content{display:flex;flex-direction:column;gap:12px}
        .ex-list{display:flex;flex-direction:column;gap:10px;max-height:58vh;overflow:auto;padding-right:6px}
        .exercise{display:grid;grid-template-columns:1fr 90px 90px 36px;gap:8px;align-items:center;padding:10px;border-radius:8px;background:rgba(255,255,255,0.01);border:1px solid rgba(255,255,255,0.02)}
        .exercise .title{font-weight:600}
        .muted{color:var(--muted);font-size:13px}
        .form-row{display:flex;gap:8px}
        input,select{background:transparent;border:1px solid rgba(255,255,255,0.06);padding:8px;border-radius:8px;color:inherit}
        .small{font-size:13px;padding:6px 8px}
        .done{background:linear-gradient(90deg,#052f1f,#093924);border-color:rgba(34,197,94,0.2)}
        .timer{display:flex;gap:8px;align-items:center}
        .progress{height:6px;background:rgba(255,255,255,0.03);border-radius:6px;overflow:hidden}
        .progress > i{display:block;height:100%;background:linear-gradient(90deg,var(--accent),#7dd3fc);width:0%}
        footer{grid-column:1/-1;display:flex;justify-content:space-between;align-items:center;margin-top:8px;color:var(--muted);font-size:13px}
        @media (max-width:820px){.wrap{grid-template-columns:1fr;}.sidebar{order:2}}
    </style>
</head>
<body>
    <div class="wrap">
        <header>
            <div>
                <h1>Daily Routine & Gym Planner</h1>
                <div class="meta">Create routines, add exercises, mark completed — data saved locally</div>
            </div>
            <div>
                <button id="exportBtn" title="Export routines">Export</button>
                <button id="importBtn" title="Import routines">Import</button>
                <input id="fileIn" type="file" accept="application/json" style="display:none" />
            </div>
        </header>

        <aside class="panel sidebar">
            <div style="display:flex;justify-content:space-between;align-items:center">
                <strong>Routines</strong>
                <div class="controls">
                    <button id="addRoutineBtn" title="Add routine">+ Add</button>
                    <button id="renameRoutineBtn" title="Rename">Rename</button>
                </div>
            </div>

            <div class="routines" id="routines"></div>

            <div style="display:flex;gap:8px;margin-top:8px">
                <select id="filterDay">
                    <option value="">All days</option>
                    <option>Monday</option><option>Tuesday</option><option>Wednesday</option>
                    <option>Thursday</option><option>Friday</option><option>Saturday</option><option>Sunday</option>
                </select>
                <button id="clearCompleted" title="Clear completed from current routine">Clear</button>
            </div>
        </aside>

        <main class="panel content">
            <div style="display:flex;justify-content:space-between;gap:12px;align-items:center">
                <div>
                    <h2 id="routineTitle" style="margin:0">—</h2>
                    <div class="muted" id="routineMeta">Select or create a routine</div>
                </div>
                <div style="display:flex;gap:8px">
                    <button id="duplicateRoutine" title="Duplicate">Duplicate</button>
                    <button id="deleteRoutine" title="Delete">Delete</button>
                </div>
            </div>

            <div style="display:flex;gap:8px;flex-direction:column">
                <div style="display:flex;gap:8px;align-items:center">
                    <input id="exName" placeholder="Exercise name" />
                    <input id="exSets" placeholder="Sets" class="small" style="width:80px" />
                    <input id="exReps" placeholder="Reps" class="small" style="width:80px" />
                    <input id="exDuration" placeholder="Duration (sec)" class="small" style="width:110px" />
                    <button id="addExBtn" class="primary">Add exercise</button>
                </div>
                <div class="ex-list" id="exList"></div>
            </div>

            <div style="display:flex;gap:8px;align-items:center;justify-content:space-between">
                <div class="muted" id="progressText">0 / 0 completed</div>
                <div style="display:flex;gap:8px">
                    <button id="shuffleBtn" title="Shuffle exercises">Shuffle</button>
                    <button id="resetBtn" title="Reset completed">Reset</button>
                </div>
            </div>
        </main>

        <footer>
            <div>Saved locally • Try creating a "Gym" routine</div>
            <div class="muted">Tip: give durations in seconds to enable timers</div>
        </footer>
    </div>

    <script>
        // Simple local routines manager
        const STORAGE_KEY = 'daily_routines_v1'
        let state = { routines: [], selected: null }

        // Utilities
        const uid = () => Date.now().toString(36) + Math.random().toString(36).slice(2,8)
        const qs = s => document.querySelector(s)
        const qsa = s => Array.from(document.querySelectorAll(s))

        // DOM refs
        const routinesEl = qs('#routines')
        const exListEl = qs('#exList')
        const routineTitle = qs('#routineTitle')
        const routineMeta = qs('#routineMeta')
        const progressText = qs('#progressText')
        const addRoutineBtn = qs('#addRoutineBtn')
        const renameRoutineBtn = qs('#renameRoutineBtn')
        const deleteRoutineBtn = qs('#deleteRoutine')
        const duplicateRoutineBtn = qs('#duplicateRoutine')
        const addExBtn = qs('#addExBtn')
        const exName = qs('#exName')
        const exSets = qs('#exSets')
        const exReps = qs('#exReps')
        const exDuration = qs('#exDuration')
        const clearCompleted = qs('#clearCompleted')
        const shuffleBtn = qs('#shuffleBtn')
        const resetBtn = qs('#resetBtn')
        const filterDay = qs('#filterDay')
        const exportBtn = qs('#exportBtn')
        const importBtn = qs('#importBtn')
        const fileIn = qs('#fileIn')

        function load() {
            try {
                const raw = localStorage.getItem(STORAGE_KEY)
                if (raw) state = JSON.parse(raw)
            } catch(e){ console.warn('load failed', e) }
            if (!state.routines || !Array.isArray(state.routines)) state.routines = []
            if (!state.selected && state.routines[0]) state.selected = state.routines[0].id
            render()
        }

        function save() {
            localStorage.setItem(STORAGE_KEY, JSON.stringify(state))
        }

        function createDefault() {
            if (state.routines.length) return
            const gym = { id: uid(), name: 'Gym', day: '', exercises: [
                { id: uid(), name: 'Squat', sets: 4, reps: 6, duration: 0, completed: false },
                { id: uid(), name: 'Bench Press', sets: 4, reps: 6, duration: 0, completed: false },
                { id: uid(), name: 'Deadlift', sets: 3, reps: 5, duration: 0, completed: false },
                { id: uid(), name: 'Plank', sets: 3, reps: 1, duration: 60, completed: false }
            ]}
            const daily = { id: uid(), name: 'Daily Routine', day: '', exercises: [
                { id: uid(), name: 'Morning Stretch', sets: 1, reps: 1, duration: 300, completed: false },
                { id: uid(), name: 'Walk', sets: 1, reps: 1, duration: 1200, completed: false }
            ]}
            state.routines.push(gym, daily)
            state.selected = gym.id
            save()
        }

        function render() {
            // routines list
            const filter = filterDay.value
            routinesEl.innerHTML = ''
            state.routines.filter(r => !filter || r.day === filter).forEach(r => {
                const el = document.createElement('div')
                el.className = 'routine-item' + (r.id === state.selected ? ' active' : '')
                el.dataset.id = r.id
                el.innerHTML = `<div>
                        <div style="display:flex;gap:8px;align-items:center">
                            <strong style="font-size:14px">${escapeHtml(r.name)}</strong>
                            <small class="muted">${r.day || ''}</small>
                        </div>
                        <div class="muted small">${r.exercises.length} exercises</div>
                    </div>
                    <div style="display:flex;gap:6px;align-items:center">
                        <button data-action="dup">⧉</button>
                        <button data-action="del">✕</button>
                    </div>`
                routinesEl.appendChild(el)
            })

            // selected routine
            const current = state.routines.find(r => r.id === state.selected)
            if (!current) {
                routineTitle.textContent = '—'
                routineMeta.textContent = 'Select or create a routine'
                exListEl.innerHTML = ''
                progressText.textContent = '0 / 0 completed'
                return
            }
            routineTitle.textContent = current.name
            routineMeta.textContent = current.day ? `${current.day}` : 'No day set'
            // exercises
            exListEl.innerHTML = ''
            current.exercises.forEach(ex => {
                const el = document.createElement('div')
                el.className = 'exercise' + (ex.completed ? ' done' : '')
                el.dataset.id = ex.id
                el.innerHTML = `
                    <div>
                        <div class="title">${escapeHtml(ex.name)}</div>
                        <div class="muted">${ex.sets ? ex.sets + ' sets' : ''}${ex.sets && ex.reps ? ' • ' : ''}${ex.reps ? ex.reps + ' reps' : ''}${ex.duration ? ' • ' + ex.duration + 's' : ''}</div>
                    </div>
                    <div style="display:flex;flex-direction:column;gap:6px;align-items:flex-end">
                        <div class="muted">Sets</div><div>${ex.sets||'-'}</div>
                    </div>
                    <div style="display:flex;flex-direction:column;gap:6px;align-items:flex-end">
                        <div class="muted">Reps</div><div>${ex.reps||'-'}</div>
                    </div>
                    <div style="display:flex;flex-direction:column;gap:6px;align-items:flex-end">
                        <button data-action="toggle">${ex.completed ? '✓' : '○'}</button>
                        <div style="height:6px;width:36px" data-timer="${ex.duration||0}">
                            ${ex.duration ? `<div class="timer" style="margin-top:6px"><button data-action="start">▶</button></div>` : ''}
                        </div>
                    </div>`
                exListEl.appendChild(el)
            })

            const done = current.exercises.filter(e => e.completed).length
            progressText.textContent = `${done} / ${current.exercises.length} completed`
            save()
        }

        // helpers
        function findRoutine(id){ return state.routines.find(r => r.id === id) }
        function escapeHtml(s){ return String(s).replace(/[&<>"']/g, c => ({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'})[c]) }

        // actions
        addRoutineBtn.addEventListener('click', ()=> {
            const name = prompt('Routine name','New Routine')
            if (!name) return
            const r = { id: uid(), name: name.trim(), day: '', exercises: [] }
            state.routines.unshift(r)
            state.selected = r.id
            render()
        })

        renameRoutineBtn.addEventListener('click', ()=> {
            const cur = findRoutine(state.selected)
            if (!cur) return alert('Select a routine')
            const name = prompt('Rename routine', cur.name)
            if (name) { cur.name = name.trim(); render() }
        })

        routinesEl.addEventListener('click', e=>{
            const item = e.target.closest('.routine-item')
            if (!item) return
            const id = item.dataset.id
            const action = e.target.dataset.action
            if (action === 'del') {
                if (!confirm('Delete routine?')) return
                state.routines = state.routines.filter(r => r.id !== id)
                if (state.selected === id) state.selected = state.routines[0]?.id || null
                render()
                return
            }
            if (action === 'dup') {
                const src = findRoutine(id)
                if (!src) return
                const copy = { id: uid(), name: src.name + ' (copy)', day: src.day, exercises: src.exercises.map(ex => ({ ...ex, id: uid(), completed:false })) }
                state.routines.unshift(copy)
                state.selected = copy.id
                render()
                return
            }
            state.selected = id
            render()
        })

        deleteRoutineBtn.addEventListener('click', ()=>{
            const cur = findRoutine(state.selected)
            if (!cur) return alert('Select a routine')
            if (!confirm('Delete "'+cur.name+'"?')) return
            state.routines = state.routines.filter(r => r.id !== cur.id)
            state.selected = state.routines[0]?.id || null
            render()
        })

        addExBtn.addEventListener('click', ()=>{
            const cur = findRoutine(state.selected)
            if (!cur) return alert('Select a routine first')
            const name = exName.value.trim()
            if (!name) return alert('Enter an exercise name')
            const sets = parseInt(exSets.value) || 0
            const reps = parseInt(exReps.value) || 0
            const duration = parseInt(exDuration.value) || 0
            cur.exercises.push({ id: uid(), name, sets, reps, duration, completed: false })
            exName.value = ''; exSets.value=''; exReps.value=''; exDuration.value=''
            render()
        })

        exListEl.addEventListener('click', e=>{
            const exEl = e.target.closest('.exercise')
            if (!exEl) return
            const exId = exEl.dataset.id
            const action = e.target.dataset.action
            const cur = findRoutine(state.selected)
            if (!cur) return
            const ex = cur.exercises.find(x => x.id === exId)
            if (!ex) return
            if (action === 'toggle') { ex.completed = !ex.completed; render(); return }
            if (action === 'start' && ex.duration) {
                startTimer(exEl, ex)
            }
        })

        function startTimer(exEl, ex) {
            const timerWrap = exEl.querySelector('[data-timer]')
            if (!timerWrap) return
            let remaining = ex.duration
            const progress = document.createElement('div')
            progress.className = 'progress'
            const bar = document.createElement('i')
            progress.appendChild(bar)
            timerWrap.innerHTML = ''
            timerWrap.appendChild(progress)
            const label = document.createElement('div')
            label.className = 'muted small'
            label.textContent = formatTime(remaining)
            timerWrap.appendChild(label)

            const interval = setInterval(()=> {
                remaining--
                const pct = Math.max(0, 100 - (remaining / ex.duration) * 100)
                bar.style.width = pct + '%'
                label.textContent = formatTime(Math.max(0, remaining))
                if (remaining <= 0) {
                    clearInterval(interval)
                    ex.completed = true
                    render()
                }
            }, 1000)
        }

        function formatTime(s){
            if (s >= 60) return Math.floor(s/60) + 'm ' + (s%60) + 's'
            return s + 's'
        }

        clearCompleted.addEventListener('click', ()=> {
            const cur = findRoutine(state.selected)
            if (!cur) return
            cur.exercises = cur.exercises.filter(e => !e.completed)
            render()
        })

        shuffleBtn.addEventListener('click', ()=> {
            const cur = findRoutine(state.selected)
            if (!cur) return
            for (let i = cur.exercises.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random()*(i+1))
                ;[cur.exercises[i], cur.exercises[j]] = [cur.exercises[j], cur.exercises[i]]
            }
            render()
        })

        resetBtn.addEventListener('click', ()=> {
            const cur = findRoutine(state.selected)
            if (!cur) return
            cur.exercises.forEach(e => e.completed = false)
            render()
        })

        filterDay.addEventListener('change', render)

        duplicateRoutineBtn.addEventListener('click', ()=> {
            const cur = findRoutine(state.selected)
            if (!cur) return alert('Select a routine')
            const copy = { id: uid(), name: cur.name + ' (copy)', day: cur.day, exercises: cur.exercises.map(ex => ({ ...ex, id: uid(), completed:false })) }
            state.routines.unshift(copy)
            state.selected = copy.id
            render()
        })

        exportBtn.addEventListener('click', ()=> {
            const blob = new Blob([JSON.stringify(state, null, 2)], { type: 'application/json' })
            const url = URL.createObjectURL(blob)
            const a = document.createElement('a')
            a.href = url; a.download = 'routines.json'; a.click()
            URL.revokeObjectURL(url)
        })

        importBtn.addEventListener('click', ()=> fileIn.click())
        fileIn.addEventListener('change', async (e) => {
            const file = e.target.files[0]; if (!file) return
            const text = await file.text()
            try {
                const obj = JSON.parse(text)
                if (obj && Array.isArray(obj.routines)) {
                    state = obj
                    if (!state.selected && state.routines[0]) state.selected = state.routines[0].id
                    render()
                } else alert('Invalid file')
            } catch (err) { alert('Import failed') }
            fileIn.value = ''
        })

        // quick save on unload
        window.addEventListener('beforeunload', save)

        // init
        createDefault()
        load()
        render()
    </script>
</body>
</html>
