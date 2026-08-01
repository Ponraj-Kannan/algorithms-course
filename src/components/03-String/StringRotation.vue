<script setup>
import { ref, reactive, computed, onMounted, onBeforeUnmount, watch } from 'vue';

defineProps({
  topic: { type: String, default: 'String Rotation Algorithm' },
  subTopic: { type: String, default: 'Concatenation & Substring Search' }
});

const CODES = {
  java: [
    ['', 'class StringRotation {'],
    ['c_entry', '    boolean isRotation(String s1, String s2) {'],
    ['c_len_check', '        if (s1.length() != s2.length() || s1.length() == 0) {'],
    ['c_len_return', '            return false;'],
    ['', '        }'],
    ['c_concat', '        String concat = s1 + s1;'],
    ['c_search', '        return concat.contains(s2);'],
    ['c_done', '    }'],
    ['', '}']
  ],
  c: [
    ['c_entry', 'bool isRotation(const char* s1, const char* s2) {'],
    ['c_len_check', '    if (strlen(s1) != strlen(s2) || strlen(s1) == 0) return false;'],
    ['c_concat', '    char concat[512]; strcpy(concat, s1); strcat(concat, s1);'],
    ['c_search', '    return strstr(concat, s2) != NULL;'],
    ['c_done', '}']
  ],
  cpp: [
    ['', 'class StringRotation {'],
    ['', 'public:'],
    ['c_entry', '    bool isRotation(string s1, string s2) {'],
    ['c_len_check', '        if (s1.length() != s2.length() || s1.length() == 0) return false;'],
    ['c_concat', '        string concat = s1 + s1;'],
    ['c_search', '        return concat.find(s2) != string::npos;'],
    ['c_done', '    }'],
    ['', '};']
  ],
  python: [
    ['', 'class StringRotation:'],
    ['c_entry', '    def is_rotation(self, s1, s2):'],
    ['c_len_check', '        if len(s1) != len(s2) or len(s1) == 0:'],
    ['c_len_return', '            return False'],
    ['c_concat', '        concat = s1 + s1'],
    ['c_search', '        return s2 in concat'],
    ['c_done', '']
  ],
  javascript: [
    ['', 'class StringRotation {'],
    ['c_entry', '  isRotation(s1, s2) {'],
    ['c_len_check', '    if (s1.length !== s2.length || s1.length === 0) return false;'],
    ['c_concat', '    let concat = s1 + s1;'],
    ['c_search', '    return concat.includes(s2);'],
    ['c_done', '  }'],
    ['', '}']
  ]
};

const PSEUDOCODE = [
  'function isRotation(s1, s2):',
  '    if length(s1) != length(s2) or length(s1) == 0:',
  '        return false   // Length mismatch or empty',
  '    concat = s1 + s1',
  '    return concat.contains(s2)   // Check if s2 is substring of (s1 + s1)',
];

function frame(title, rows) { return { title, rows }; }

function buildSteps(str1, str2) {
  const steps = [];
  const s1 = String(str1).trim();
  const s2 = String(str2).trim();
  const n1 = s1.length;
  const n2 = s2.length;
  const fnLabel = 'isRotation(s1, s2)';

  // 1. Entry
  steps.push({
    badge: `isRotation called with s1="${s1}" and s2="${s2}"`,
    code: 'c_entry',
    vars: [
      frame('main()', []),
      frame(fnLabel, [['s1', `"${s1}"`], ['s2', `"${s2}"`]])
    ],
    s1Cells: s1.split('').map((v, idx) => ({ idx, val: v, state: 'normal' })),
    s2Cells: s2.split('').map((v, idx) => ({ idx, val: v, state: 'normal' })),
    concatCells: [],
    matchStart: -1, matchEnd: -1, result: null
  });

  // 2. Length check
  const lenCheck = n1 !== n2 || n1 === 0;
  steps.push({
    badge: `if (s1.length != s2.length || s1.length == 0) → ${n1} != ${n2} → ${lenCheck ? 'TRUE (Invalid length)' : 'FALSE'}`,
    code: 'c_len_check',
    vars: [
      frame('main()', []),
      frame(fnLabel, [['s1.length', String(n1)], ['s2.length', String(n2)]])
    ],
    s1Cells: s1.split('').map((v, idx) => ({ idx, val: v, state: 'normal' })),
    s2Cells: s2.split('').map((v, idx) => ({ idx, val: v, state: 'normal' })),
    concatCells: [],
    matchStart: -1, matchEnd: -1, result: null
  });

  if (lenCheck) {
    steps.push({
      badge: `return false → String lengths differ (${n1} vs ${n2}) → NOT a rotation`,
      code: 'c_len_return',
      vars: [
        frame('main()', []),
        frame(fnLabel, [['return', 'false'], ['reason', 'LENGTH_MISMATCH']])
      ],
      s1Cells: s1.split('').map((v, idx) => ({ idx, val: v, state: 'mismatch' })),
      s2Cells: s2.split('').map((v, idx) => ({ idx, val: v, state: 'mismatch' })),
      concatCells: [],
      matchStart: -1, matchEnd: -1, result: false
    });
    return steps;
  }

  // 3. String concat = s1 + s1
  const concatStr = s1 + s1;
  const concatChars = concatStr.split('');
  steps.push({
    badge: `String concat = s1 + s1 → concat set to "${concatStr}"`,
    code: 'c_concat',
    vars: [
      frame('main()', []),
      frame(fnLabel, [['concat', `"${concatStr}"`]])
    ],
    s1Cells: s1.split('').map((v, idx) => ({ idx, val: v, state: 'normal' })),
    s2Cells: s2.split('').map((v, idx) => ({ idx, val: v, state: 'normal' })),
    concatCells: concatChars.map((v, idx) => ({ idx, val: v, state: 'normal' })),
    matchStart: -1, matchEnd: -1, result: null
  });

  // 4. Substring search for s2 in concat
  const foundIdx = concatStr.indexOf(s2);

  // Animate window sliding across concat
  for (let windowStart = 0; windowStart <= n1; windowStart++) {
    const isCurrentMatch = windowStart === foundIdx;
    const windowEnd = windowStart + n2 - 1;

    steps.push({
      badge: `Searching substring "${s2}" in concat at index ${windowStart}..${windowEnd} → ${isCurrentMatch ? 'MATCH FOUND!' : 'Checking...'}`,
      code: 'c_search',
      vars: [
        frame('main()', []),
        frame(fnLabel, [['concat', `"${concatStr}"`], ['s2', `"${s2}"`], ['window_start', String(windowStart)]])
      ],
      s1Cells: s1.split('').map((v, idx) => ({ idx, val: v, state: 'normal' })),
      s2Cells: s2.split('').map((v, idx) => ({ idx, val: v, state: isCurrentMatch ? 'matched' : 'normal' })),
      concatCells: concatChars.map((v, idx) => {
        if (idx >= windowStart && idx <= windowEnd) return { idx, val: v, state: isCurrentMatch ? 'matched' : 'cur' };
        return { idx, val: v, state: 'normal' };
      }),
      matchStart: isCurrentMatch ? windowStart : -1,
      matchEnd: isCurrentMatch ? windowEnd : -1,
      result: isCurrentMatch ? true : null
    });

    if (isCurrentMatch) break;
  }

  const isRotation = foundIdx !== -1;
  steps.push({
    badge: `return concat.contains(s2) → Substring "${s2}" ${isRotation ? 'FOUND at index ' + foundIdx + ' in "' + concatStr + '" → IS a rotation!' : 'NOT found → NOT a rotation!'}`,
    code: 'c_search',
    vars: [
      frame('main()', []),
      frame(fnLabel, [['result', String(isRotation)], ['found_at_index', String(foundIdx)]])
    ],
    s1Cells: s1.split('').map((v, idx) => ({ idx, val: v, state: isRotation ? 'matched' : 'normal' })),
    s2Cells: s2.split('').map((v, idx) => ({ idx, val: v, state: isRotation ? 'matched' : 'mismatch' })),
    concatCells: concatChars.map((v, idx) => {
      if (foundIdx !== -1 && idx >= foundIdx && idx < foundIdx + n2) return { idx, val: v, state: 'matched' };
      return { idx, val: v, state: 'normal' };
    }),
    matchStart: foundIdx,
    matchEnd: foundIdx !== -1 ? foundIdx + n2 - 1 : -1,
    result: isRotation
  });

  steps.push({
    badge: `String Rotation Check Complete! Result: "${s2}" ${isRotation ? 'IS A ROTATION of' : 'IS NOT A ROTATION of'} "${s1}"`,
    code: 'c_done',
    vars: [
      frame('main()', []),
      frame(fnLabel, [['result', String(isRotation)]])
    ],
    s1Cells: s1.split('').map((v, idx) => ({ idx, val: v, state: isRotation ? 'matched' : 'normal' })),
    s2Cells: s2.split('').map((v, idx) => ({ idx, val: v, state: isRotation ? 'matched' : 'mismatch' })),
    concatCells: concatChars.map((v, idx) => {
      if (foundIdx !== -1 && idx >= foundIdx && idx < foundIdx + n2) return { idx, val: v, state: 'matched' };
      return { idx, val: v, state: 'normal' };
    }),
    matchStart: foundIdx,
    matchEnd: foundIdx !== -1 ? foundIdx + n2 - 1 : -1,
    result: isRotation
  });

  return steps;
}

const inpS1 = ref('waterbottle');
const inpS2 = ref('erbottlewat');
const lang = ref('java');
const speed = ref(650);
const si = ref(0);
const playing = ref(false);
const vizHeight = ref(265);
const tableHeight = ref(60);
const leftWidth = ref(50);
const rightTab = ref('code');

const stepsData = reactive({ steps: buildSteps('waterbottle', 'erbottlewat') });
const steps = computed(() => stepsData.steps);
const s = computed(() => steps.value[Math.max(0, Math.min(si.value, steps.value.length - 1))] || {});
const codeLines = computed(() => CODES[lang.value] || []);

let playTimer = null;

function applyInput() {
  const str1 = String(inpS1.value || '').trim();
  const str2 = String(inpS2.value || '').trim();
  if (!str1.length || !str2.length) {
    alert('Please enter two non-empty strings.');
    return;
  }

  playing.value = false;
  stepsData.steps = buildSteps(str1, str2);
  si.value = 0;
}

function stepBy(d) {
  si.value = Math.max(0, Math.min(steps.value.length - 1, si.value + d));
}

function togglePlay() {
  const next = !playing.value;
  if (next && si.value >= steps.value.length - 1) si.value = 0;
  playing.value = next;
}

function tick() {
  clearTimeout(playTimer);
  if (!playing.value) return;
  if (si.value >= steps.value.length - 1) {
    playing.value = false;
    return;
  }
  playTimer = setTimeout(() => {
    si.value = Math.min(steps.value.length - 1, si.value + 1);
    tick();
  }, 2100 - speed.value);
}

watch(playing, v => {
  if (v) tick();
  else clearTimeout(playTimer);
});

function onKeydown(e) {
  const tag = e.target.tagName;
  if (tag === 'INPUT' || tag === 'SELECT' || tag === 'TEXTAREA') return;
  if (e.key === 'ArrowRight') stepBy(1);
  if (e.key === 'ArrowLeft') stepBy(-1);
  if (e.key === ' ') { e.preventDefault(); togglePlay(); }
}

const mainRef = ref(null);
const leftColRef = ref(null);
const hResizerRef = ref(null);
const vizResizerRef = ref(null);
const tableResizerRef = ref(null);

function initHResizer() {
  const rsz = hResizerRef.value, main = mainRef.value;
  if (!rsz || !main) return;
  let dragging = false, startX = 0, startW = 0;
  const onDown = e => { dragging = true; startX = e.clientX; startW = leftColRef.value.offsetWidth; rsz.classList.add('drag'); document.body.style.userSelect = 'none'; };
  const onMove = e => { if (!dragging) return; const mainW = main.offsetWidth; leftWidth.value = (Math.max(200, Math.min(mainW - 200, startW + e.clientX - startX)) / mainW) * 100; };
  const onUp = () => { if (!dragging) return; dragging = false; rsz.classList.remove('drag'); document.body.style.userSelect = ''; };
  rsz.addEventListener('mousedown', onDown);
  document.addEventListener('mousemove', onMove);
  document.addEventListener('mouseup', onUp);
  return () => { rsz.removeEventListener('mousedown', onDown); document.removeEventListener('mousemove', onMove); document.removeEventListener('mouseup', onUp); };
}

function initVResizer(elRef, valueRef, minH, maxH) {
  const rsz = elRef.value;
  if (!rsz) return;
  let dragging = false, startY = 0, startH = 0;
  const onDown = e => { dragging = true; startY = e.clientY; startH = valueRef.value; rsz.classList.add('drag'); document.body.style.userSelect = 'none'; e.preventDefault(); };
  const onMove = e => { if (!dragging) return; valueRef.value = Math.max(minH, Math.min(maxH, startH + (e.clientY - startY))); };
  const onUp = () => { if (!dragging) return; dragging = false; rsz.classList.remove('drag'); document.body.style.userSelect = ''; };
  rsz.addEventListener('mousedown', onDown);
  document.addEventListener('mousemove', onMove);
  document.addEventListener('mouseup', onUp);
  return () => { rsz.removeEventListener('mousedown', onDown); document.removeEventListener('mousemove', onMove); document.removeEventListener('mouseup', onUp); };
}

let cleanupFns = [];
onMounted(() => {
  document.addEventListener('keydown', onKeydown);
  cleanupFns.push(initHResizer());
  cleanupFns.push(initVResizer(vizResizerRef, vizHeight, 160, 480));
  cleanupFns.push(initVResizer(tableResizerRef, tableHeight, 50, 200));
});

onBeforeUnmount(() => {
  document.removeEventListener('keydown', onKeydown);
  clearTimeout(playTimer);
  cleanupFns.forEach(fn => fn && fn());
});
</script>

<template>
  <div class="slide-wrapper">
    <div class="navbar">
      <h2 class="navbar-title">{{ topic }}</h2>
      <img src="../../assets/logo.png" alt="Logo" />
    </div>

    <div class="slide-body">
      <div class="row-main">
        <div class="ll-root">
          <!-- Control Panel Toolbar -->
          <div class="ll-toolbar">
            <label>s1</label>
            <input type="text" v-model="inpS1" style="width: 120px;" class="ll-text-input" placeholder="e.g. waterbottle" />

            <label>s2</label>
            <input type="text" v-model="inpS2" style="width: 120px;" class="ll-text-input" placeholder="e.g. erbottlewat" />

            <button class="ll-viz-btn" @click="applyInput">&#9654; Visualize</button>

            <div class="ll-nav-controls">
              <button class="ll-nav-btn" title="First step" @click="stepBy(-steps.length)">&#171;</button>
              <button class="ll-nav-btn" @click="stepBy(-1)">&#8249; Prev</button>
              <button class="ll-play-btn" @click="togglePlay">{{ playing ? '\u23F8 Pause' : '\u25B6 Play' }}</button>
              <button class="ll-nav-btn" @click="stepBy(1)">Next &#8250;</button>
              <button class="ll-nav-btn" title="Last step" @click="stepBy(steps.length)">&#187;</button>
            </div>
          </div>

          <div class="ll-main" ref="mainRef">
            <!-- Left Visualization Column -->
            <div class="ll-left-col" ref="leftColRef" :style="{ width: leftWidth + '%' }">
              <div class="ll-viz-wrap" :style="{ height: vizHeight + 'px' }">
                <div class="ll-perm-area">
                  <div class="ll-ptrs">
                    <div class="ll-ptr-chip">Result = <b :class="s.result === true ? 'll-c-green' : (s.result === false ? 'll-c-red' : 'll-c-orange')">{{ s.result === null ? 'Searching...' : String(s.result) }}</b></div>
                  </div>

                  <!-- Strings and Concatenated Track Diagram -->
                  <div class="ll-multi-tracks">
                    <div class="ll-track-group">
                      <div class="ll-track-label">s1 (original):</div>
                      <div class="ll-arr-track">
                        <div v-for="cell in s.s1Cells" :key="'s1-' + cell.idx" class="ll-arr-cell-wrap">
                          <div class="ll-arr-box" :class="{ 'll-box-matched': cell.state === 'matched' }">{{ cell.val }}</div>
                          <div class="ll-arr-idx">[{{ cell.idx }}]</div>
                        </div>
                      </div>
                    </div>

                    <div class="ll-track-group">
                      <div class="ll-track-label">s2 (rotation target):</div>
                      <div class="ll-arr-track">
                        <div v-for="cell in s.s2Cells" :key="'s2-' + cell.idx" class="ll-arr-cell-wrap">
                          <div class="ll-arr-box" :class="{ 'll-box-matched': cell.state === 'matched', 'll-box-mismatch': cell.state === 'mismatch' }">{{ cell.val }}</div>
                          <div class="ll-arr-idx">[{{ cell.idx }}]</div>
                        </div>
                      </div>
                    </div>

                    <div v-if="s.concatCells && s.concatCells.length" class="ll-track-group">
                      <div class="ll-track-label">concat = s1 + s1:</div>
                      <div class="ll-arr-track">
                        <div v-for="cell in s.concatCells" :key="'c-' + cell.idx" class="ll-arr-cell-wrap">
                          <div class="ll-arr-box" :class="{ 'll-box-cur': cell.state === 'cur', 'll-box-matched': cell.state === 'matched' }">{{ cell.val }}</div>
                          <div class="ll-arr-idx">[{{ cell.idx }}]</div>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>

              <div class="ll-vresizer" ref="vizResizerRef"></div>

              <!-- Color Legend -->
              <div class="ll-legend">
                <span class="ll-leg"><span class="ll-legdot ll-legdot-existing"></span>Normal</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-cur"></span>Sliding Substring Window</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-matched"></span>Rotation Substring Match</span>
              </div>

              <!-- Variable Frames & Call Stack -->
              <div class="ll-table-area" :style="{ height: tableHeight + 'px' }">
                <div class="ll-table-title">Variable frames &mdash; innermost = current</div>
                <div class="ll-stack-line">
                  <template v-if="s.vars && s.vars.length">
                    <div
                      v-for="(f, depth) in s.vars"
                      :key="depth"
                      class="ll-frame"
                      :class="{ 'll-frame-cur': depth === s.vars.length - 1 }"
                      :style="{ marginLeft: depth * 14 + 'px' }"
                    >
                      {{ f.title }}(<span v-for="(r, i) in f.rows" :key="i">
                        <span v-if="i > 0">, </span>
                        <span class="ll-fname">{{ r[0] }}</span>=<span :class="r[2] ? 'll-c-blue' : (depth === s.vars.length - 1 ? 'll-c-orange' : 'll-c-green')" style="font-weight:700">{{ r[1] }}</span>
                      </span>)<span v-if="depth === s.vars.length - 1" class="ll-now"> &#9668; current</span>
                    </div>
                  </template>
                  <template v-else>&mdash;</template>
                </div>
              </div>

              <div class="ll-vresizer" ref="tableResizerRef"></div>

              <!-- Step Badge -->
              <div class="ll-badge-wrap">
                <div class="ll-badge" :class="{ 'll-badge-error': s.result === false }">{{ s.badge }}</div>
              </div>
            </div>

            <div class="ll-resizer" ref="hResizerRef"></div>

            <!-- Right Column: Code & Theory -->
            <div class="ll-right-col">
              <div class="ll-code-panel">
                <div class="ll-code-header">
                  <div class="ll-tabbar">
                    <button class="ll-tab-btn" :class="{ active: rightTab === 'code' }" @click="rightTab = 'code'">Code</button>
                    <button class="ll-tab-btn" :class="{ active: rightTab === 'pseudo' }" @click="rightTab = 'pseudo'">Pseudocode</button>
                    <button class="ll-tab-btn" :class="{ active: rightTab === 'complexity' }" @click="rightTab = 'complexity'">Complexity</button>
                  </div>
                  <select v-if="rightTab === 'code'" v-model="lang" class="ll-lang-select">
                    <option value="java">Java</option>
                    <option value="c">C</option>
                    <option value="cpp">C++</option>
                    <option value="python">Python</option>
                    <option value="javascript">JavaScript</option>
                  </select>
                </div>

                <!-- Code Scroll with 1-to-1 Line Highlighting -->
                <div v-if="rightTab === 'code'" class="ll-code-scroll">
                  <pre class="ll-pre"><span
                    v-for="(line, i) in codeLines"
                    :key="i"
                    class="ll-codeline"
                    :class="{ 'll-hl': line[0] && line[0] === s.code }"
                  >{{ line[1] === '' ? ' ' : line[1] }}</span></pre>
                </div>

                <!-- Pseudocode Scroll -->
                <div v-else-if="rightTab === 'pseudo'" class="ll-code-scroll">
                  <pre class="ll-pre"><span
                    v-for="(line, i) in PSEUDOCODE"
                    :key="i"
                    class="ll-codeline"
                  >{{ line }}</span></pre>
                </div>

                <!-- Complexity Scroll -->
                <div v-else class="ll-info-scroll">
                  <h3>Time &amp; Space Complexity</h3>
                  <table class="ll-complexity-table">
                    <thead><tr><th>Case</th><th>Time</th><th>Why</th></tr></thead>
                    <tbody>
                      <tr><td>Best Case</td><td>O(1)</td><td>String lengths differ s1.length() != s2.length().</td></tr>
                      <tr><td>Average Case</td><td>O(n)</td><td>Substring search of s2 inside concatenated string (s1 + s1) of length 2n.</td></tr>
                      <tr><td>Worst Case</td><td>O(n)</td><td>Substring search across concatenated string of length 2n.</td></tr>
                      <tr><td>Space</td><td>O(n)</td><td>Requires O(n) space to create concatenated string (s1 + s1).</td></tr>
                    </tbody>
                  </table>
                  <p class="ll-note">
                    Key observation: Any valid rotation of string <code>s1</code> is guaranteed to be a contiguous substring inside <code>s1 + s1</code>!
                  </p>
                </div>
              </div>
            </div>
          </div>

          <!-- Footer -->
          <div class="ll-footer">
            Step {{ si + 1 }} / {{ steps.length }}
            <span class="ll-speed-wrap">Speed <input type="range" min="100" max="2000" step="100" v-model.number="speed" /></span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.ll-root * { box-sizing: border-box; }
.ll-root {
  --coral: #F04D4D; --coral-dark: #d93e3e; --coral-light: #fff0f0;
  --bg: #f5f6fa; --surface: #ffffff; --surface2: #f1f4f9;
  --border: #e2e8f0; --border2: #cbd5e1; --text: #1e293b; --text2: #475569; --muted: #94a3b8;
  --blue: #3b82f6; --blue-light: #eff6ff; --green: #22c55e; --green-light: #f0fdf4;
  --orange: #f97316; --orange-light: #fff7ed; --purple: #9333ea;
  --red: #ef4444; --red-dark: #991b1b; --red-light: #fef2f2;
  --shadow-sm: 0 1px 3px rgba(0,0,0,.08), 0 1px 2px rgba(0,0,0,.04);
  --radius: 8px; --radius-sm: 6px;
  background: var(--bg); color: var(--text);
  font-family: 'Segoe UI', system-ui, sans-serif; font-size: 13px;
  display: flex; flex-direction: column; height: 50vh; min-height: 600px; overflow: hidden; width: 100%;
}
@keyframes ll-pop { from { transform: scale(.85); opacity: 0; } to { transform: scale(1); opacity: 1; } }
.slide-wrapper { margin-top: -10px; margin-left: -30px; width: 107%; max-height: 100%; font-size: 0.8rem; font-weight: 400; }
.slide-body { display: flex; flex-direction: column; border-radius: 4px; height: 100%; }
.navbar { display: flex; flex-direction: row; justify-content: space-between; align-items: center; gap: 0.75rem; padding: 0 10px; background-color: #ffffff; position: fixed; width: 94.7%; }
.navbar > img { height: 30px; }
.navbar-title { margin: 0; font-size: 1.5rem; font-weight: 700; background-color: #ef5050; color: #ffffff; width: 80%; padding-left: 10px; margin-left: -10px; border-radius: 5px; }
.row-main { width: 100%; height: 90%; margin-top: 37px; overflow-x: auto; overflow-y: auto; scrollbar-width: thin; }
.ll-toolbar { display: flex; align-items: center; gap: 8px; padding: 7px 16px; background: var(--surface); border-bottom: 1px solid var(--border); flex-shrink: 0; flex-wrap: wrap; box-shadow: var(--shadow-sm); }
.ll-toolbar label { font-size: 11px; color: var(--muted); white-space: nowrap; font-weight: 600; }
.ll-num-input, .ll-text-input { background: var(--surface); border: 1px solid var(--border2); color: var(--text); border-radius: var(--radius-sm); padding: 5px 8px; font-size: 13px; font-family: monospace; transition: border-color .15s; }
.ll-num-input { width: 60px; }
.ll-text-input { width: 120px; padding: 5px 10px; }
.ll-num-input:focus, .ll-text-input:focus { outline: none; border-color: var(--coral); box-shadow: 0 0 0 3px rgba(240,77,77,.1); }
.ll-viz-btn { background: var(--coral); color: #fff; border: none; padding: 6px 16px; border-radius: var(--radius-sm); cursor: pointer; font-size: 12px; font-weight: 600; box-shadow: var(--shadow-sm); transition: filter .15s; }
.ll-viz-btn:hover { filter: brightness(1.08); }
.ll-nav-controls { display: flex; margin-left: auto; align-items: center; gap: 4px; flex-shrink: 0; flex-wrap: wrap; }
.ll-nav-btn { background: var(--surface2); border: 1px solid var(--border2); color: var(--text2); padding: 5px 11px; border-radius: var(--radius-sm); cursor: pointer; font-size: 12px; font-weight: 500; transition: all .15s; white-space: nowrap; }
.ll-nav-btn:hover { background: var(--surface); border-color: var(--coral); color: var(--coral); }
.ll-play-btn { background: var(--blue-light); border: 1px solid var(--blue); color: var(--blue); min-width: 72px; font-weight: 600; padding: 5px 11px; border-radius: var(--radius-sm); cursor: pointer; font-size: 12px; transition: all .15s; }
.ll-play-btn:hover { background: var(--blue); color: #fff; }
.ll-main { display: flex; flex: 1; overflow: hidden; position: relative; }
.ll-left-col { display: flex; flex-direction: column; overflow: hidden; min-width: 200px; max-width: 72%; }
.ll-resizer { width: 5px; cursor: col-resize; background: var(--border); flex-shrink: 0; transition: background .15s; position: relative; z-index: 20; }
.ll-resizer:hover, .ll-resizer.drag { background: var(--coral); }
.ll-right-col { display: flex; flex-direction: column; flex: 1; overflow: hidden; min-width: 0; height: 78%; }
.ll-viz-wrap { flex-shrink: 0; background: var(--surface); border-bottom: 1px solid var(--border); position: relative; overflow-x: hidden; overflow-y: auto; }
.ll-perm-area { display: flex; flex-direction: column; align-items: stretch; min-height: 100%; width: 100%; min-width: 0; box-sizing: border-box; }
.ll-ptrs { display: flex; gap: 8px; flex-wrap: wrap; padding: 10px 16px 4px; min-height: 36px; width: 100%; box-sizing: border-box; min-width: 0; }
.ll-ptr-chip { background: var(--surface2); border: 1px solid var(--border); border-radius: var(--radius-sm); padding: 3px 10px; font-size: 12px; font-family: monospace; box-shadow: var(--shadow-sm); white-space: nowrap; flex-shrink: 0; }
.ll-c-blue { color: var(--blue); } .ll-c-orange { color: var(--orange); } .ll-c-green { color: var(--green); } .ll-c-purple { color: var(--purple); } .ll-c-red { color: var(--red); }

/* Multi Tracks Diagram */
.ll-multi-tracks { display: flex; flex-direction: column; gap: 4px; padding: 4px 16px; width: 100%; }
.ll-track-group { display: flex; flex-direction: column; gap: 2px; }
.ll-track-label { font-size: 11px; font-weight: 700; color: var(--text2); font-family: monospace; }
.ll-arr-track { display: flex; align-items: flex-start; flex-wrap: wrap; gap: 6px; width: 100%; }
.ll-arr-cell-wrap { display: flex; flex-direction: column; align-items: center; min-width: 0; }
.ll-arr-box { width: 38px; height: 38px; display: flex; align-items: center; justify-content: center; border: 2px solid var(--blue); border-radius: var(--radius-sm); background: #eff6ff; color: #1e293b; font-weight: 700; font-size: 15px; font-family: monospace; box-shadow: var(--shadow-sm); transition: all 0.25s ease; animation: ll-pop .3s ease; }
.ll-box-cur { border-color: #f59e0b !important; background: #fef3c7 !important; color: #92400e !important; transform: translateY(-2px); }
.ll-box-matched { border-color: #10b981 !important; background: #dcfce7 !important; color: #065f46 !important; box-shadow: 0 0 0 3px rgba(16, 185, 129, 0.25) !important; }
.ll-box-mismatch { border-color: #ef4444 !important; background: #fef2f2 !important; color: #991b1b !important; }

.ll-arr-idx { font-size: 10px; color: var(--muted); margin-top: 2px; font-family: 'Consolas', monospace; font-weight: 600; }
.ll-vresizer { height: 5px; cursor: row-resize; background: var(--border); flex-shrink: 0; transition: background .15s; position: relative; z-index: 20; }
.ll-vresizer:hover, .ll-vresizer.drag { background: var(--coral); }
.ll-legend { display: flex; flex-wrap: wrap; gap: 6px 14px; padding: 6px 12px; border-bottom: 1px solid var(--border); flex-shrink: 0; background: var(--surface2); }
.ll-leg { display: flex; align-items: center; gap: 5px; font-size: 11px; color: var(--text2); font-weight: 500; }
.ll-legdot { width: 11px; height: 11px; border-radius: 3px; flex-shrink: 0; display: inline-block; }
.ll-legdot-existing { background: #eff6ff; border: 1.5px solid var(--blue); }
.ll-legdot-cur { background: #fef3c7; border: 1.5px solid #f59e0b; }
.ll-legdot-matched { background: #dcfce7; border: 1.5px solid #10b981; }

.ll-table-area { flex-shrink: 0; padding: 8px 14px; border-bottom: 1px solid var(--border); overflow-x: hidden; overflow-y: auto; background: var(--surface); min-width: 0; box-sizing: border-box; }
.ll-table-title { font-size: 10px; color: var(--muted); margin-bottom: 4px; font-style: italic; }
.ll-stack-line { font-family: 'Consolas', monospace; font-size: 12px; line-height: 1.8; }
.ll-frame { font-family: 'Consolas', monospace; font-size: 11.5px; color: var(--text2); padding: 1px 0; white-space: nowrap; }
.ll-frame-cur { color: var(--orange); background: var(--orange-light); border-radius: 4px; padding: 1px 5px; }
.ll-fname { color: var(--text2); }
.ll-now { color: var(--orange); font-size: 10px; margin-left: 6px; }
.ll-badge-wrap { padding: 6px 10px; border-bottom: 1px solid var(--border); flex-shrink: 0; min-height: 36px; display: flex; align-items: center; background: var(--surface); }
.ll-badge { display: inline-block; padding: 4px 12px; border-radius: var(--radius-sm); border-left: 3px solid var(--coral); background: var(--coral-light); font-size: 11px; color: var(--coral-dark); line-height: 1.4; word-break: break-word; font-weight: 500; }
.ll-badge-error { border-left-color: var(--red); background: var(--red-light); color: var(--red-dark); }
.ll-code-panel { display: flex; flex-direction: column; height: 100%; overflow: hidden; }
.ll-code-header { display: flex; align-items: center; gap: 8px; padding: 8px 14px; background: var(--surface); border-bottom: 1px solid var(--border); flex-shrink: 0; box-shadow: var(--shadow-sm); flex-wrap: wrap; }
.ll-tabbar { display: flex; gap: 4px; flex-wrap: wrap; }
.ll-tab-btn { background: var(--surface2); border: 1px solid var(--border2); color: var(--text2); padding: 5px 11px; border-radius: var(--radius-sm); cursor: pointer; font-size: 11px; font-weight: 600; transition: all .15s; white-space: nowrap; }
.ll-tab-btn:hover { border-color: var(--coral); color: var(--coral); }
.ll-tab-btn.active { background: var(--coral); border-color: var(--coral); color: #fff; }
.ll-lang-select { background: var(--surface2); border: 1px solid var(--border2); color: var(--text); padding: 5px 28px 5px 10px; border-radius: var(--radius-sm); font-size: 12px; font-weight: 500; cursor: pointer; min-width: 110px; margin-left: auto; transition: border-color .15s; appearance: none; background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='10' height='6' viewBox='0 0 10 6'%3E%3Cpath d='M0 0l5 6 5-6z' fill='%2394a3b8'/%3E%3C/svg%3E"); background-repeat: no-repeat; background-position: right 10px center; }
.ll-lang-select:focus { outline: none; border-color: var(--coral); box-shadow: 0 0 0 3px rgba(240,77,77,.1); }
.ll-code-scroll { flex: 1; overflow: auto; padding: 14px 16px; background: #f8fafc; min-width: 0; }
.ll-pre { font-family: 'Cascadia Code', 'Fira Code', 'Consolas', monospace; font-size: 12px; line-height: 1.65; white-space: pre-wrap; word-break: break-all; color: var(--text); margin: 0; }
.ll-codeline { display: block; padding: 0 16px; margin: 0 -16px; }
.ll-hl { background: #dcfce7; color: #15803d; border-radius: 3px; border-left: 3px solid var(--green); font-weight: 600; }
.ll-info-scroll { flex: 1; overflow: auto; padding: 16px 20px; background: var(--surface); color: var(--text2); font-size: 13px; line-height: 1.6; }
.ll-info-scroll h3 { margin: 0 0 10px; color: var(--text); font-size: 15px; font-weight: 700; }
.ll-info-scroll h3:not(:first-child) { margin-top: 18px; }
.ll-info-scroll p { margin: 0 0 10px; }
.ll-info-scroll code { background: var(--surface2); border: 1px solid var(--border); border-radius: 4px; padding: 1px 5px; font-family: 'Consolas', monospace; font-size: 12px; color: var(--coral-dark); }
.ll-complexity-table { width: 100%; border-collapse: collapse; margin-bottom: 14px; font-size: 12.5px; }
.ll-complexity-table th, .ll-complexity-table td { border: 1px solid var(--border); padding: 8px 10px; text-align: left; }
.ll-complexity-table th { background: var(--surface2); color: var(--text); font-weight: 700; }
.ll-complexity-table td:nth-child(2) { font-family: 'Consolas', monospace; font-weight: 700; color: var(--coral-dark); }
.ll-note { background: var(--orange-light); border-left: 3px solid var(--orange); border-radius: var(--radius-sm); padding: 8px 12px; font-size: 12.5px; color: var(--text2); }
.ll-footer { padding: 4px 16px; font-size: 11px; color: var(--muted); border-top: 1px solid var(--border); background: var(--surface); flex-shrink: 0; display: flex; align-items: center; }
.ll-speed-wrap { display: flex; align-items: center; gap: 5px; margin-left: 16px; }
.ll-speed-wrap input[type=range] { width: 90px; accent-color: var(--coral); }
@media (max-width: 900px) {
  .ll-main { flex-direction: column; }
  .ll-left-col, .ll-right-col { max-width: 100% !important; width: 100% !important; }
  .ll-resizer { display: none; }
  .ll-toolbar { flex-direction: column; align-items: stretch; }
  .ll-nav-controls { margin-left: 0; justify-content: center; }
}
</style>
