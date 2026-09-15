<template>
  <div class="gbfs">
    <div class="wrap">

      <header class="page-header">
        <h1>Why greedy best-first search wins the race</h1>
        <p class="subtitle">Both searches start from the same cell and chase the same goal. Watch how many cells each one has to check before it gets there — that difference is the whole argument.</p>
      </header>

      <section class="controls">
        <div class="control-group">
          <span class="control-label">Course</span>
          <div class="segmented" role="group" aria-label="Course">
            <button type="button" class="seg-btn" :aria-pressed="scenario === 'open'" :disabled="isRunning" @click="selectScenario('open')">Open field</button>
            <button type="button" class="seg-btn" :aria-pressed="scenario === 'obstacles'" :disabled="isRunning" @click="selectScenario('obstacles')">With obstacles</button>
          </div>
        </div>
        <div class="control-group">
          <span class="control-label">Reveal speed</span>
          <div class="segmented" role="group" aria-label="Reveal speed">
            <button type="button" class="seg-btn" :aria-pressed="speed === 30" :disabled="isRunning" @click="selectSpeed(30)">Slow</button>
            <button type="button" class="seg-btn" :aria-pressed="speed === 14" :disabled="isRunning" @click="selectSpeed(14)">Normal</button>
            <button type="button" class="seg-btn" :aria-pressed="speed === 5" :disabled="isRunning" @click="selectSpeed(5)">Fast</button>
          </div>
        </div>
        <div class="control-actions">
          <button type="button" class="btn btn-primary" :disabled="isRunning" @click="handleAnimateClick">Animate</button>
          <button type="button" class="btn btn-ghost" @click="handleResetClick">Reset</button>
        </div>
      </section>

      <main class="panels">

        <article class="panel panel-bfs">
          <div class="panel-head">
            <div class="panel-title-group">
              <h2>Breadth-first search</h2>
              <p class="panel-rule">Expands whichever cell has been waiting longest — spreads outward evenly in every direction.</p>
            </div>
            <span class="status" :class="bfsStats.statusClass">{{ bfsStats.statusText }}</span>
          </div>
          <div class="grid" :style="gridStyle">
            <div
              v-for="cell in bfsCells"
              :key="'bfs-' + cell.row + '-' + cell.col"
              class="cell"
              :class="{ wall: cell.isWall, start: cell.isStart, goal: cell.isGoal, visited: cell.visited, path: cell.path }"
            >
              <svg v-if="cell.isStart" class="marker marker--start" viewBox="0 0 24 24" fill="currentColor" xmlns="http://www.w3.org/2000/svg">
                <path d="M12 2.5l2.9 6.6 7.1.6-5.4 4.7 1.6 7-6.2-3.7L6 21.4l1.6-7L2.2 9.7l7.1-.6L12 2.5z"/>
              </svg>
              <svg v-else-if="cell.isGoal" class="marker marker--goal" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                <path d="M5 5l14 14M19 5L5 19" stroke="currentColor" stroke-width="3" stroke-linecap="round"/>
              </svg>
            </div>
          </div>
          <div class="panel-foot">
            <div class="stats">
              <div class="stat">
                <span class="stat-label">Cells checked</span>
                <span class="stat-value">{{ bfsStats.visited }}</span>
              </div>
              <div class="stat">
                <span class="stat-label">Path length</span>
                <span class="stat-value">{{ bfsStats.pathLength ?? '–' }}</span>
              </div>
            </div>
            <div class="legend">
              <span class="legend-item"><span class="legend-swatch" style="background:var(--bfs-soft)"></span>Checked</span>
              <span class="legend-item"><span class="legend-swatch" style="background:var(--path-glow)"></span>Path</span>
              <span v-if="scenario === 'obstacles'" class="legend-item legend-wall"><span class="legend-swatch" style="background:var(--wall)"></span>Wall</span>
            </div>
          </div>
        </article>

        <article class="panel panel-greedy">
          <div class="panel-head">
            <div class="panel-title-group">
              <h2>Greedy best-first search</h2>
              <p class="panel-rule">Expands whichever cell looks closest to the goal — heads toward the target directly.</p>
            </div>
            <span class="status" :class="greedyStats.statusClass">{{ greedyStats.statusText }}</span>
          </div>
          <div class="grid" :style="gridStyle">
            <div
              v-for="cell in greedyCells"
              :key="'greedy-' + cell.row + '-' + cell.col"
              class="cell"
              :class="{ wall: cell.isWall, start: cell.isStart, goal: cell.isGoal, visited: cell.visited, path: cell.path }"
            >
              <svg v-if="cell.isStart" class="marker marker--start" viewBox="0 0 24 24" fill="currentColor" xmlns="http://www.w3.org/2000/svg">
                <path d="M12 2.5l2.9 6.6 7.1.6-5.4 4.7 1.6 7-6.2-3.7L6 21.4l1.6-7L2.2 9.7l7.1-.6L12 2.5z"/>
              </svg>
              <svg v-else-if="cell.isGoal" class="marker marker--goal" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                <path d="M5 5l14 14M19 5L5 19" stroke="currentColor" stroke-width="3" stroke-linecap="round"/>
              </svg>
            </div>
          </div>
          <div class="panel-foot">
            <div class="stats">
              <div class="stat">
                <span class="stat-label">Cells checked</span>
                <span class="stat-value">{{ greedyStats.visited }}</span>
              </div>
              <div class="stat">
                <span class="stat-label">Path length</span>
                <span class="stat-value">{{ greedyStats.pathLength ?? '–' }}</span>
              </div>
            </div>
            <div class="legend">
              <span class="legend-item"><span class="legend-swatch" style="background:var(--greedy-soft)"></span>Checked</span>
              <span class="legend-item"><span class="legend-swatch" style="background:var(--path-glow)"></span>Path</span>
              <span v-if="scenario === 'obstacles'" class="legend-item legend-wall"><span class="legend-swatch" style="background:var(--wall)"></span>Wall</span>
            </div>
          </div>
        </article>

      </main>

      <section class="verdict" :class="{ hidden: !verdictVisible, show: verdictShow }">
        <h2>The verdict</h2>
        <div class="bars">
          <div class="bar-row">
            <span class="bar-label bar-label--bfs">Breadth-first</span>
            <div class="bar-track"><div class="bar-fill bar-fill--bfs" :style="{ width: barBfsWidth + '%' }"></div></div>
            <span class="bar-value">{{ barBfsValue }}</span>
          </div>
          <div class="bar-row">
            <span class="bar-label bar-label--greedy">Greedy</span>
            <div class="bar-track"><div class="bar-fill bar-fill--greedy" :style="{ width: barGreedyWidth + '%' }"></div></div>
            <span class="bar-value">{{ barGreedyValue }}</span>
          </div>
        </div>
        <p>{{ verdictSummary }}</p>
        <div class="caveat">
          <p><strong>Worth knowing:</strong> greedy only asks "how close does this look to the goal?" — it never adds up the distance already travelled. That's what makes it fast, but it's also why it can't promise the shortest route on every map. A tricky, U-shaped wall can send it into a dead end before it backs out, while breadth-first search will always find the true shortest path eventually. A* keeps greedy's sense of direction but adds up real travel cost too, which is why it's usually the better default when both speed and correctness matter.</p>
        </div>
      </section>

      <footer class="page-footer">
        <p>A self-contained demo comparing two search strategies, cell by cell, on an open grid and a simple obstacle course.</p>
      </footer>

    </div>
  </div>
</template>

<script setup>
import { ref, reactive, computed } from 'vue'

/* ---------------- grid setup (same layout/logic as the original) ---------------- */
const COLS = 18
const ROWS = 12
const START = { col: 1, row: 9 }
const GOAL = { col: 15, row: 2 }

const gridDataCache = {}

function getGridData(scenarioName) {
  if (gridDataCache[scenarioName]) return gridDataCache[scenarioName]
  const walls = new Set()
  if (scenarioName === 'obstacles') {
    for (let r = 0; r < ROWS; r++) {
      if (r === 5 || r === 6) continue // the one gap in the wall
      const wc = 13 - r
      walls.add(wc + ',' + r)
      walls.add((wc + 1) + ',' + r)
    }
  }
  const data = { cols: COLS, rows: ROWS, walls, start: START, goal: GOAL }
  gridDataCache[scenarioName] = data
  return data
}

function key(node) {
  return node.col + ',' + node.row
}

function neighbors(node, gridData) {
  const deltas = [[0, -1], [1, 0], [0, 1], [-1, 0]]
  const out = []
  for (const [dc, dr] of deltas) {
    const c = node.col + dc
    const r = node.row + dr
    if (c < 0 || c >= gridData.cols || r < 0 || r >= gridData.rows) continue
    if (gridData.walls.has(c + ',' + r)) continue
    out.push({ col: c, row: r })
  }
  return out
}

function reconstructPath(cameFrom, start, goal) {
  if (goal.col === start.col && goal.row === start.row) return [start]
  if (!cameFrom.has(key(goal))) return []
  const path = [goal]
  let current = goal
  while (!(current.col === start.col && current.row === start.row)) {
    current = cameFrom.get(key(current))
    if (!current) return []
    path.push(current)
  }
  path.reverse()
  return path
}

/* ---------------- algorithms (unchanged) ---------------- */
function computeBFS(gridData) {
  const { start, goal } = gridData
  const visitedOrder = []
  const cameFrom = new Map()
  const visited = new Set([key(start)])
  const queue = [start]
  let qi = 0
  while (qi < queue.length) {
    const current = queue[qi++]
    visitedOrder.push(current)
    if (current.col === goal.col && current.row === goal.row) break
    for (const n of neighbors(current, gridData)) {
      const k = key(n)
      if (!visited.has(k)) {
        visited.add(k)
        cameFrom.set(k, current)
        queue.push(n)
      }
    }
  }
  return { visitedOrder, path: reconstructPath(cameFrom, start, goal) }
}

function heuristic(node, goal, start) {
  const dx1 = node.col - goal.col
  const dy1 = node.row - goal.row
  const manhattan = Math.abs(dx1) + Math.abs(dy1)
  const dx2 = start.col - goal.col
  const dy2 = start.row - goal.row
  const cross = Math.abs(dx1 * dy2 - dx2 * dy1)
  return manhattan + cross * 0.001
}

function computeGreedy(gridData) {
  const { start, goal } = gridData
  const visitedOrder = []
  const cameFrom = new Map()
  const visited = new Set()
  const frontier = [{ node: start, h: heuristic(start, goal, start) }]
  while (frontier.length) {
    let minIdx = 0
    for (let i = 1; i < frontier.length; i++) {
      if (frontier[i].h < frontier[minIdx].h) minIdx = i
    }
    const current = frontier.splice(minIdx, 1)[0].node
    const k = key(current)
    if (visited.has(k)) continue
    visited.add(k)
    visitedOrder.push(current)
    if (current.col === goal.col && current.row === goal.row) break
    for (const n of neighbors(current, gridData)) {
      const nk = key(n)
      if (!visited.has(nk)) {
        if (!cameFrom.has(nk)) cameFrom.set(nk, current)
        frontier.push({ node: n, h: heuristic(n, goal, start) })
      }
    }
  }
  return { visitedOrder, path: reconstructPath(cameFrom, start, goal) }
}

/* ---------------- cell grid state ---------------- */
function buildCells(gridData) {
  const cells = []
  for (let r = 0; r < gridData.rows; r++) {
    for (let c = 0; c < gridData.cols; c++) {
      cells.push({
        row: r,
        col: c,
        isWall: gridData.walls.has(c + ',' + r),
        isStart: c === gridData.start.col && r === gridData.start.row,
        isGoal: c === gridData.goal.col && r === gridData.goal.row,
        visited: false,
        path: false,
      })
    }
  }
  return cells
}

/* ---------------- reactive state ---------------- */
const scenario = ref('open')
const speed = ref(14)
const isRunning = ref(false)

const gridData = computed(() => getGridData(scenario.value))
const gridStyle = computed(() => ({ gridTemplateColumns: `repeat(${gridData.value.cols}, 1fr)` }))

const bfsCells = ref(buildCells(gridData.value))
const greedyCells = ref(buildCells(gridData.value))

const bfsStats = reactive({ visited: 0, pathLength: null, statusText: 'Idle', statusClass: '' })
const greedyStats = reactive({ visited: 0, pathLength: null, statusText: 'Idle', statusClass: '' })

const verdictVisible = ref(false)
const verdictShow = ref(false)
const barBfsWidth = ref(0)
const barGreedyWidth = ref(0)
const barBfsValue = ref(0)
const barGreedyValue = ref(0)
const verdictSummary = ref('')

let currentRunId = 0

/* ---------------- helpers ---------------- */
function rebuildGrids() {
  const gd = gridData.value
  bfsCells.value = buildCells(gd)
  greedyCells.value = buildCells(gd)
}

function resetStats() {
  bfsStats.visited = 0
  bfsStats.pathLength = null
  bfsStats.statusText = 'Idle'
  bfsStats.statusClass = ''
  greedyStats.visited = 0
  greedyStats.pathLength = null
  greedyStats.statusText = 'Idle'
  greedyStats.statusClass = ''
}

function hideVerdict() {
  verdictShow.value = false
  verdictVisible.value = false
}

function showVerdict(bfsResult, greedyResult) {
  const maxNodes = Math.max(bfsResult.nodesVisited, greedyResult.nodesVisited, 1)
  barBfsWidth.value = (bfsResult.nodesVisited / maxNodes) * 100
  barGreedyWidth.value = (greedyResult.nodesVisited / maxNodes) * 100
  barBfsValue.value = bfsResult.nodesVisited
  barGreedyValue.value = greedyResult.nodesVisited

  if (bfsResult.nodesVisited === 0 || greedyResult.nodesVisited === 0) {
    verdictSummary.value = 'One of the searches could not reach the goal on this course.'
  } else {
    const ratio = (bfsResult.nodesVisited / greedyResult.nodesVisited).toFixed(1)
    let pathNote
    if (greedyResult.pathLength === bfsResult.pathLength) {
      pathNote = 'and still found a path exactly as short as breadth-first search\u2019s.'
    } else if (greedyResult.pathLength > bfsResult.pathLength) {
      const extra = greedyResult.pathLength - bfsResult.pathLength
      pathNote = `\u2014 though this time its path ran ${extra} step${extra === 1 ? '' : 's'} longer than the shortest route breadth-first search guarantees.`
    } else {
      pathNote = 'and, on this course, even matched or beat breadth-first search on path length too.'
    }
    verdictSummary.value = `Greedy best-first search reached the goal after checking ${greedyResult.nodesVisited} cells, versus ${bfsResult.nodesVisited} for breadth-first search \u2014 about ${ratio}\u00d7 fewer, ${pathNote}`
  }

  verdictVisible.value = true
  requestAnimationFrame(() => {
    verdictShow.value = true
  })
}

function animatePanel(cellsRef, statsObj, gd, result, stepDelay, runId, kind) {
  return new Promise((resolve) => {
    const { visitedOrder, path } = result
    statsObj.statusText = 'Searching…'
    statsObj.statusClass = `status--running status--${kind}`

    let i = 0
    function stepVisited() {
      if (runId !== currentRunId) return
      if (i < visitedOrder.length) {
        const node = visitedOrder[i]
        const idx = node.row * gd.cols + node.col
        const cell = cellsRef.value[idx]
        if (!cell.isStart && !cell.isGoal) {
          cell.visited = true
        }
        i++
        statsObj.visited = i
        setTimeout(stepVisited, stepDelay)
      } else {
        stepPath()
      }
    }

    let j = 0
    function stepPath() {
      if (runId !== currentRunId) return
      if (path.length === 0) {
        statsObj.statusText = 'No path found'
        statsObj.statusClass = 'status--fail'
        resolve({ nodesVisited: visitedOrder.length, pathLength: 0 })
        return
      }
      if (j < path.length) {
        const node = path[j]
        const idx = node.row * gd.cols + node.col
        cellsRef.value[idx].path = true
        j++
        statsObj.pathLength = j
        setTimeout(stepPath, Math.max(Math.round(stepDelay * 0.6), 8))
      } else {
        statsObj.statusText = 'Reached goal'
        statsObj.statusClass = 'status--done'
        resolve({ nodesVisited: visitedOrder.length, pathLength: path.length })
      }
    }

    stepVisited()
  })
}

/* ---------------- controls ---------------- */
function selectScenario(name) {
  if (isRunning.value) return
  scenario.value = name
  rebuildGrids()
  resetStats()
  hideVerdict()
}

function selectSpeed(value) {
  if (isRunning.value) return
  speed.value = value
}

function handleAnimateClick() {
  if (isRunning.value) return
  currentRunId++
  const runId = currentRunId
  const gd = gridData.value

  rebuildGrids()
  resetStats()
  hideVerdict()

  isRunning.value = true

  const bfsResult = computeBFS(gd)
  const greedyResult = computeGreedy(gd)

  Promise.all([
    animatePanel(bfsCells, bfsStats, gd, bfsResult, speed.value, runId, 'bfs'),
    animatePanel(greedyCells, greedyStats, gd, greedyResult, speed.value, runId, 'greedy'),
  ]).then(([bfsRes, greedyRes]) => {
    if (runId !== currentRunId) return
    isRunning.value = false
    showVerdict(bfsRes, greedyRes)
  })
}

function handleResetClick() {
  currentRunId++
  isRunning.value = false
  rebuildGrids()
  resetStats()
  hideVerdict()
}
</script>

<style scoped>
.gbfs{
  --paper:#EEF1F5;
  --card:#FFFFFF;
  --ink:#17222F;
  --muted:#5B6B80;
  --border:#D9E0E7;

  --bfs:#B96A2C;
  --bfs-soft:#F1D6AE;
  --bfs-soft-strong:#E2AE6E;

  --greedy:#3B54C4;
  --greedy-soft:#CBD5F7;
  --greedy-soft-strong:#9FB0EC;

  --path-glow:#F0C75E;
  --path-line:#8452C8;

  --wall:#2B3440;
  --wall-stripe:#3B4654;

  --start:#D6402F;
  --goal:#C23B93;

  --font-ui: -apple-system, BlinkMacSystemFont, "Segoe UI", "Inter", "Helvetica Neue", Arial, sans-serif;
  --font-mono: ui-monospace, "SF Mono", "SFMono-Regular", Menlo, Consolas, "Liberation Mono", monospace;

  font-family:var(--font-ui);
  color:var(--ink);
  background-color:var(--paper);
  background-image: radial-gradient(circle, rgba(23,34,48,0.07) 1px, transparent 1px);
  background-size: 18px 18px;
  line-height:1.45;
}

.gbfs *,.gbfs *::before,.gbfs *::after{ box-sizing:border-box; }

.gbfs button{ font-family:inherit; }
.gbfs button:focus-visible, .gbfs .seg-btn:focus-visible{
  outline: 2px solid var(--ink);
  outline-offset: 2px;
}

.gbfs .wrap{
  max-width:1040px;
  margin:0 auto;
  padding: 14px 18px 20px;
  height: 75vh;
  overflow-y: auto;
  scrollbar-width: thin;
  scrollbar-color: rgba(23,34,48,0.15) transparent;
  padding-bottom: 130px;
}

/* ---------- Header ---------- */
.page-header h1{
  font-size: clamp(16px, 1.8vw, 20px);
  font-weight:700;
  letter-spacing:-0.01em;
  margin:0 0 4px;
}
.page-header .subtitle{
  margin:0;
  max-width:76ch;
  color:var(--muted);
  font-size:11.5px;
  line-height:1.4;
}

/* ---------- Controls ---------- */
.controls{
  margin-top:10px;
  background:var(--card);
  border:1px solid var(--border);
  border-radius:8px;
  padding:7px 12px;
  display:flex;
  flex-wrap:wrap;
  align-items:center;
  gap:12px;
  justify-content:space-between;
}
.control-group{
  display:flex;
  align-items:center;
  gap:6px;
  flex-wrap:wrap;
}
.control-label{
  font-size:11px;
  color:var(--muted);
  white-space:nowrap;
  font-weight:500;
}
.segmented{
  display:inline-flex;
  background:var(--paper);
  border:1px solid var(--border);
  border-radius:6px;
  padding:2px;
  gap:2px;
}
.seg-btn{
  border:none;
  background:transparent;
  color:var(--muted);
  font-size:11px;
  padding:3px 8px;
  border-radius:4px;
  cursor:pointer;
  white-space:nowrap;
  transition: background-color .15s ease, color .15s ease;
}
.seg-btn:hover:not(:disabled){ color:var(--ink); }
.seg-btn[aria-pressed="true"]{
  background:var(--ink);
  color:#fff;
}
.seg-btn:disabled{ opacity:.5; cursor:not-allowed; }

.control-actions{
  display:flex;
  gap:6px;
}
.btn{
  border-radius:6px;
  padding:4px 12px;
  font-size:11.5px;
  font-weight:600;
  cursor:pointer;
  white-space:nowrap;
  border:1px solid transparent;
  transition: background-color .15s ease, border-color .15s ease, opacity .15s ease;
}
.btn-primary{
  background:var(--ink);
  color:#fff;
}
.btn-primary:hover:not(:disabled){ background:#243346; }
.btn-primary:disabled{ opacity:.5; cursor:not-allowed; }
.btn-ghost{
  background:transparent;
  color:var(--ink);
  border-color:var(--border);
}
.btn-ghost:hover{ background:var(--paper); }

/* ---------- Panels ---------- */
.panels{
  margin-top:10px;
  display:grid;
  grid-template-columns: 1fr 1fr;
  gap:12px;
}
@media (max-width: 860px){
  .panels{ grid-template-columns: 1fr; }
}

.panel{
  background:var(--card);
  border:1px solid var(--border);
  border-radius:8px;
  padding:10px 12px 8px;
  display:flex;
  flex-direction:column;
}

.panel-head{
  display:flex;
  align-items:flex-start;
  justify-content:space-between;
  gap:8px;
  margin-bottom:8px;
}
.panel-title-group h2{
  font-size:13.5px;
  font-weight:600;
  margin:0 0 2px;
}
.panel-bfs h2{ color:var(--bfs); }
.panel-greedy h2{ color:var(--greedy); }
.panel-rule{
  margin:0;
  font-size:10.5px;
  color:var(--muted);
  max-width:42ch;
  line-height:1.35;
}

.status{
  flex-shrink:0;
  font-size:10.5px;
  color:var(--muted);
  padding:2px 7px;
  border:1px solid var(--border);
  border-radius:999px;
  white-space:nowrap;
}
.status--running.status--bfs{ color:var(--bfs); border-color:var(--bfs-soft-strong); }
.status--running.status--greedy{ color:var(--greedy); border-color:var(--greedy-soft-strong); }
.status--done{ color:#2E8B57; border-color:#B9DEC7; }
.status--fail{ color:var(--start); border-color:#EFC0BA; }

.grid{
  display:grid;
  width:100%;
  max-width:330px;
  margin:0 auto;
  gap:1.5px;
}
.cell{
  position:relative;
  aspect-ratio:1/1;
  background:#FBFCFD;
  border:1px solid var(--border);
  border-radius:1.5px;
  display:flex;
  align-items:center;
  justify-content:center;
  transition: background-color .22s ease, border-color .22s ease;
}
.panel-bfs .cell.visited{ background:var(--bfs-soft); border-color:var(--bfs-soft-strong); }
.panel-greedy .cell.visited{ background:var(--greedy-soft); border-color:var(--greedy-soft-strong); }

.panel-bfs .cell.path, .panel-greedy .cell.path{
  background:var(--path-glow);
  border-color:#D9A93A;
}
.cell.path:not(.start):not(.goal)::after{
  content:'';
  width:36%;
  height:36%;
  border-radius:50%;
  background:var(--path-line);
  opacity:.85;
}

.cell.wall{
  background:repeating-linear-gradient(45deg, var(--wall), var(--wall) 4px, var(--wall-stripe) 4px, var(--wall-stripe) 8px);
  border-color:var(--wall);
}

.marker{ width:58%; height:58%; display:block; }
.marker--start{ color:var(--start); }
.marker--goal{ color:var(--goal); }

.panel-foot{
  margin-top:8px;
  display:flex;
  align-items:center;
  justify-content:space-between;
  flex-wrap:wrap;
  gap:8px;
}
.stats{ display:flex; gap:14px; }
.stat-label{
  display:block;
  font-size:9.5px;
  color:var(--muted);
  margin-bottom:1px;
}
.stat-value{
  display:block;
  font-family:var(--font-mono);
  font-size:14px;
  font-weight:600;
}
.legend{
  display:flex;
  gap:8px;
  flex-wrap:wrap;
  font-size:10.5px;
  color:var(--muted);
}
.legend-item{ display:flex; align-items:center; gap:4px; }
.legend-swatch{
  width:9px; height:9px; border-radius:2px; display:inline-block;
  border:1px solid rgba(0,0,0,.08);
}

/* ---------- Verdict ---------- */
.verdict{
  margin-top:12px;
  background:var(--card);
  border:1px solid var(--border);
  border-radius:8px;
  padding:12px 14px;
  opacity:0;
  transform:translateY(6px);
  transition:opacity .35s ease, transform .35s ease;
}
.verdict.hidden{ display:none; }
.verdict.show{ opacity:1; transform:translateY(0); }

.verdict h2{
  font-size:13.5px;
  margin:0 0 8px;
}

.bars{ display:flex; flex-direction:column; gap:6px; margin-bottom:10px; }
.bar-row{ display:grid; grid-template-columns: 95px 1fr 36px; align-items:center; gap:8px; }
.bar-label{ font-size:11px; font-weight:600; white-space:nowrap; }
.bar-label--bfs{ color:var(--bfs); }
.bar-label--greedy{ color:var(--greedy); }
.bar-track{
  background:var(--paper);
  border:1px solid var(--border);
  border-radius:4px;
  height:10px;
  overflow:hidden;
}
.bar-fill{ height:100%; width:0%; border-radius:4px 0 0 4px; }
.bar-fill--bfs{ background:var(--bfs); }
.bar-fill--greedy{ background:var(--greedy); }
.bar-value{
  font-family:var(--font-mono);
  font-size:11px;
  text-align:right;
}

.verdict p{
  margin:0 0 8px;
  font-size:11px;
  max-width:74ch;
  line-height:1.45;
}

.caveat{
  border-left:3px solid var(--border);
  padding-left:10px;
  color:var(--muted);
  font-size:10.5px;
  max-width:76ch;
  line-height:1.4;
}
.caveat p{ margin:0; }
.caveat strong{ color:var(--ink); }

.page-footer{
  margin-top:14px;
  font-size:10px;
  color:var(--muted);
}

@media (prefers-reduced-motion: reduce){
  .cell, .verdict{ transition:none; }
}
</style>
