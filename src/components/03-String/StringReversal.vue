<script setup>
import { ref, reactive, computed, onMounted, onBeforeUnmount, watch } from 'vue';

defineProps({
  topic: { type: String, default: 'String Reversal Algorithm' },
  subTopic: { type: String, default: 'Two-Pointer Character Swap' }
});

const CODES = {
  java: [
    ['', 'class StringReversal {'],
    ['c_entry', '    String reverseString(char[] str) {'],
    ['c_left', '        int left = 0;'],
    ['c_right', '        int right = str.length - 1;'],
    ['c_while', '        while (left < right) {'],
    ['c_swap_temp', '            char temp = str[left];'],
    ['c_swap_w1', '            str[left] = str[right];'],
    ['c_swap_w2', '            str[right] = temp;'],
    ['c_inc', '            left++; right--;'],
    ['', '        }'],
    ['c_return', '        return new String(str);'],
    ['c_done', '    }'],
    ['', '}']
  ],
  c: [
    ['c_entry', 'void reverseString(char* str) {'],
    ['c_left', '    int left = 0;'],
    ['c_right', '    int right = strlen(str) - 1;'],
    ['c_while', '    while (left < right) {'],
    ['c_swap_temp', '        char temp = str[left];'],
    ['c_swap_w1', '        str[left] = str[right];'],
    ['c_swap_w2', '        str[right] = temp;'],
    ['c_inc', '        left++; right--;'],
    ['', '    }'],
    ['c_done', '}']
  ],
  cpp: [
    ['', 'class StringReversal {'],
    ['', 'public:'],
    ['c_entry', '    string reverseString(string str) {'],
    ['c_left', '        int left = 0;'],
    ['c_right', '        int right = str.length() - 1;'],
    ['c_while', '        while (left < right) {'],
    ['c_swap_temp', '            char temp = str[left];'],
    ['c_swap_w1', '            str[left] = str[right];'],
    ['c_swap_w2', '            str[right] = temp;'],
    ['c_inc', '            left++; right--;'],
    ['', '        }'],
    ['c_return', '        return str;'],
    ['c_done', '    }'],
    ['', '};']
  ],
  python: [
    ['', 'class StringReversal:'],
    ['c_entry', '    def reverse_string(self, s):'],
    ['c_left', '        chars = list(s)'],
    ['c_right', '        left, right = 0, len(chars) - 1'],
    ['c_while', '        while left < right:'],
    ['c_swap_temp', '            temp = chars[left]'],
    ['c_swap_w1', '            chars[left] = chars[right]'],
    ['c_swap_w2', '            chars[right] = temp'],
    ['c_inc', '            left += 1; right -= 1'],
    ['c_return', '        return "".join(chars)'],
    ['c_done', '']
  ],
  javascript: [
    ['', 'class StringReversal {'],
    ['c_entry', '  reverseString(str) {'],
    ['c_left', '    let chars = str.split("");'],
    ['c_right', '    let left = 0, right = chars.length - 1;'],
    ['c_while', '    while (left < right) {'],
    ['c_swap_temp', '      let temp = chars[left];'],
    ['c_swap_w1', '      chars[left] = chars[right];'],
    ['c_swap_w2', '      chars[right] = temp;'],
    ['c_inc', '      left++; right--;'],
    ['', '    }'],
    ['c_return', '    return chars.join("");'],
    ['c_done', '  }'],
    ['', '}']
  ]
};

const PSEUDOCODE = [
  'function reverseString(str):',
  '    left = 0',
  '    right = length(str) - 1',
  '    while left < right:',
  '        swap(str[left], str[right])',
  '        left = left + 1',
  '        right = right - 1',
  '    return str',
];

function frame(title, rows) { return { title, rows }; }

function buildSteps(initialStr) {
  const steps = [];
  const chars = initialStr.split('');
  const n = chars.length;
  const fnLabel = 'reverseString(str)';
  const swappedSet = new Set();

  function currentCellStates(leftIdx, rightIdx, isSwapping = false) {
    return chars.map((val, idx) => {
      if (isSwapping && (idx === leftIdx || idx === rightIdx)) return { idx, val, state: 'swap' };
      if (idx === leftIdx) return { idx, val, state: 'left_ptr' };
      if (idx === rightIdx) return { idx, val, state: 'right_ptr' };
      if (swappedSet.has(idx)) return { idx, val, state: 'swapped' };
      return { idx, val, state: 'normal' };
    });
  }

  // 1. Entry
  steps.push({
    badge: `reverseString called with text "${initialStr}"`,
    code: 'c_entry',
    vars: [
      frame('main()', []),
      frame(fnLabel, [['str', `"${initialStr}"`]])
    ],
    cells: currentCellStates(-1, -1),
    left: -1, right: -1, swaps: 0, n
  });

  if (n === 0) {
    steps.push({
      badge: 'Text string is empty (n = 0) → return ""',
      code: 'c_return',
      vars: [frame('main()', []), frame(fnLabel, [['return', '""']])],
      cells: [],
      left: -1, right: -1, swaps: 0, n: 0
    });
    return steps;
  }

  // 2. left = 0
  let left = 0;
  steps.push({
    badge: `int left = 0 → left pointer initialized to index 0 ('${chars[0]}')`,
    code: 'c_left',
    vars: [
      frame('main()', []),
      frame(fnLabel, [['left', '0'], ['str[left]', `'${chars[0]}'`]])
    ],
    cells: currentCellStates(left, -1),
    left, right: -1, swaps: 0, n
  });

  // 3. right = n - 1
  let right = n - 1;
  steps.push({
    badge: `int right = str.length - 1 → right pointer initialized to index ${right} ('${chars[right]}')`,
    code: 'c_right',
    vars: [
      frame('main()', []),
      frame(fnLabel, [['left', '0'], ['right', String(right)], ['str[right]', `'${chars[right]}'`]])
    ],
    cells: currentCellStates(left, right),
    left, right, swaps: 0, n
  });

  let swaps = 0;
  let tempVal = null;

  // 4. While loop
  while (left < right) {
    steps.push({
      badge: `while (left < right) → checking left (${left}) < right (${right}) → TRUE`,
      code: 'c_while',
      vars: [
        frame('main()', []),
        frame(fnLabel, [['left', String(left)], ['right', String(right)], ['str[left]', `'${chars[left]}'`], ['str[right]', `'${chars[right]}'`]])
      ],
      cells: currentCellStates(left, right),
      left, right, temp: tempVal, swaps, n
    });

    tempVal = chars[left];
    steps.push({
      badge: `char temp = str[left] → temp set to '${tempVal}'`,
      code: 'c_swap_temp',
      vars: [
        frame('main()', []),
        frame(fnLabel, [['left', String(left)], ['right', String(right)], ['temp', `'${tempVal}'`]])
      ],
      cells: currentCellStates(left, right, true),
      left, right, temp: tempVal, swaps, n
    });

    chars[left] = chars[right];
    steps.push({
      badge: `str[left] = str[right] → str[${left}] set to '${chars[right]}'`,
      code: 'c_swap_w1',
      vars: [
        frame('main()', []),
        frame(fnLabel, [['left', String(left)], ['right', String(right)], ['str[left]', `'${chars[left]}'`]])
      ],
      cells: currentCellStates(left, right, true),
      left, right, temp: tempVal, swaps, n
    });

    chars[right] = tempVal;
    swaps++;
    swappedSet.add(left);
    swappedSet.add(right);

    steps.push({
      badge: `str[right] = temp → str[${right}] set to '${tempVal}'. Swapped indices ${left} and ${right}`,
      code: 'c_swap_w2',
      vars: [
        frame('main()', []),
        frame(fnLabel, [['left', String(left)], ['right', String(right)], ['str[right]', `'${chars[right]}'`]])
      ],
      cells: currentCellStates(left, right, true),
      left, right, temp: tempVal, swaps, n
    });

    left++;
    right--;
    steps.push({
      badge: `left++ (${left}), right-- (${right}) → moved pointers inward`,
      code: 'c_inc',
      vars: [
        frame('main()', []),
        frame(fnLabel, [['left', String(left)], ['right', String(right)]])
      ],
      cells: currentCellStates(left, right),
      left, right, temp: tempVal, swaps, n
    });
  }

  if (left === right) {
    swappedSet.add(left);
    steps.push({
      badge: `while (left < right) → left (${left}) < right (${right}) → FALSE (middle element '${chars[left]}' remains in place)`,
      code: 'c_while',
      vars: [
        frame('main()', []),
        frame(fnLabel, [['left', String(left)], ['right', String(right)]])
      ],
      cells: currentCellStates(left, right),
      left, right, swaps, n
    });
  }

  const finalStr = chars.join('');
  steps.push({
    badge: `return new String(str) → Reversed string: "${finalStr}"`,
    code: 'c_return',
    vars: [
      frame('main()', []),
      frame(fnLabel, [['return', `"${finalStr}"`]])
    ],
    cells: chars.map((v, idx) => ({ idx, val: v, state: 'swapped' })),
    left: -1, right: -1, swaps, n
  });

  steps.push({
    badge: `String Reversal Complete! Original: "${initialStr}" → Reversed: "${finalStr}"`,
    code: 'c_done',
    vars: [
      frame('main()', []),
      frame(fnLabel, [['result', `"${finalStr}"`]])
    ],
    cells: chars.map((v, idx) => ({ idx, val: v, state: 'swapped' })),
    left: -1, right: -1, swaps, n
  });

  return steps;
}

const inpText = ref('ALGORITHM');
const lang = ref('java');
const speed = ref(650);
const si = ref(0);
const playing = ref(false);
const vizHeight = ref(265);
const tableHeight = ref(60);
const leftWidth = ref(50);
const rightTab = ref('code');

const stepsData = reactive({ steps: buildSteps('ALGORITHM') });
const steps = computed(() => stepsData.steps);
const s = computed(() => steps.value[Math.max(0, Math.min(si.value, steps.value.length - 1))] || {});
const codeLines = computed(() => CODES[lang.value] || []);

let playTimer = null;

function applyInput() {
  const textStr = String(inpText.value || '').trim();
  if (!textStr.length) {
    alert('Please enter a non-empty string.');
    return;
  }

  playing.value = false;
  stepsData.steps = buildSteps(textStr);
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

function cellLabel(cell) {
  const tags = [];
  if (s.value.left === cell.idx) tags.push('left');
  if (s.value.right === cell.idx) tags.push('right');
  if (tags.length) return tags.join(', ');
  if (cell.state === 'swapped') return 'reversed';
  return '';
}

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
            <label>Text</label>
            <input type="text" v-model="inpText" style="width: 220px;" class="ll-text-input" placeholder="e.g. ALGORITHM" />

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
                    <div class="ll-ptr-chip">left = <b class="ll-c-blue">{{ s.left >= 0 ? s.left : 'N/A' }}</b></div>
                    <div class="ll-ptr-chip">right = <b class="ll-c-purple">{{ s.right >= 0 ? s.right : 'N/A' }}</b></div>
                    <div class="ll-ptr-chip">temp = <b class="ll-c-orange">{{ s.temp ? '\'' + s.temp + '\'' : 'N/A' }}</b></div>
                    <div class="ll-ptr-chip">Swaps = <b class="ll-c-green">{{ s.swaps }}</b></div>
                  </div>

                  <!-- Visual Diagram - Character Cards -->
                  <div class="ll-arr-track">
                    <template v-for="cell in s.cells" :key="cell.idx">
                      <div class="ll-arr-cell-wrap">
                        <div class="ll-ptr-tag-wrap">
                          <span v-if="cell.idx === s.left" class="ll-ptr-lbl ll-lbl-blue">left</span>
                          <span v-if="cell.idx === s.right" class="ll-ptr-lbl ll-lbl-purple">right</span>
                        </div>
                        <div
                          class="ll-arr-box"
                          :class="{
                            'll-box-left': cell.state === 'left_ptr',
                            'll-box-right': cell.state === 'right_ptr',
                            'll-box-swap': cell.state === 'swap',
                            'll-box-swapped': cell.state === 'swapped'
                          }"
                        >
                          {{ cell.val }}
                        </div>
                        <div class="ll-arr-idx">[{{ cell.idx }}]</div>
                      </div>
                    </template>

                    <div v-if="!s.cells || s.cells.length === 0" class="ll-empty-state">
                      Text string is empty
                    </div>
                  </div>
                </div>
              </div>

              <div class="ll-vresizer" ref="vizResizerRef"></div>

              <!-- Color Legend -->
              <div class="ll-legend">
                <span class="ll-leg"><span class="ll-legdot ll-legdot-existing"></span>Normal</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-left"></span>Left Pointer</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-right"></span>Right Pointer</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-swap"></span>Swapping</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-swapped"></span>Reversed Position</span>
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
                <div class="ll-badge" :class="{ 'll-badge-error': s.invalid }">{{ s.badge }}</div>
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
                      <tr><td>Best Case</td><td>O(n)</td><td>Performs n/2 character swaps.</td></tr>
                      <tr><td>Average Case</td><td>O(n)</td><td>Performs n/2 character swaps.</td></tr>
                      <tr><td>Worst Case</td><td>O(n)</td><td>Performs n/2 character swaps for string of length n.</td></tr>
                      <tr><td>Space</td><td>O(1)</td><td>In-place character array reversal using O(1) auxiliary space.</td></tr>
                    </tbody>
                  </table>
                  <p class="ll-note">
                    Key observation: Two-pointer string reversal uses <code>left</code> and <code>right</code> pointers moving toward each other, swapping characters until <code>left &gt;= right</code>.
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
.ll-text-input { width: 220px; padding: 5px 10px; }
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

/* Pastel Flat Visual Diagram Box System */
.ll-arr-track { display: flex; align-items: flex-start; flex-wrap: wrap; padding: 10px 16px 8px; min-height: 100px; gap: 10px; width: 100%; box-sizing: border-box; min-width: 0; }
.ll-arr-cell-wrap { display: flex; flex-direction: column; align-items: center; min-width: 0; }
.ll-ptr-tag-wrap { height: 28px; display: flex; align-items: flex-end; justify-content: center; gap: 4px; margin-bottom: 2px; }
.ll-ptr-lbl { font-size: 13px; font-weight: 800; font-family: 'Consolas', 'Fira Code', monospace; display: inline-flex; flex-direction: column; align-items: center; line-height: 1; gap: 1px; white-space: nowrap; animation: ll-pop 0.2s ease; }
.ll-ptr-lbl::after { content: '↓'; font-size: 11px; font-weight: 900; line-height: 1; margin-top: 1px; }
.ll-lbl-blue { color: #3b82f6; }
.ll-lbl-orange { color: #f97316; }
.ll-lbl-purple { color: #9333ea; }
.ll-lbl-green { color: #10b981; }
.ll-arr-box { width: 54px; height: 54px; display: flex; align-items: center; justify-content: center; border: 2px solid var(--blue); border-radius: var(--radius); background: #eff6ff; color: #1e293b; font-weight: 700; font-size: 18px; font-family: monospace; box-shadow: var(--shadow-sm); transition: all 0.25s ease; animation: ll-pop .3s ease; }
.ll-box-left { border-color: #3b82f6 !important; background: #dbeafe !important; color: #1e40af !important; box-shadow: 0 0 0 3px rgba(59, 130, 246, 0.25) !important; transform: translateY(-2px); }
.ll-box-right { border-color: #8b5cf6 !important; background: #f3e8ff !important; color: #5b21b6 !important; box-shadow: 0 0 0 3px rgba(139, 92, 246, 0.25) !important; transform: translateY(-2px); }
.ll-box-swap { border-color: #ef4444 !important; background: #fef2f2 !important; color: #991b1b !important; box-shadow: 0 0 0 3px rgba(239, 68, 68, 0.25) !important; transform: scale(1.05); }
.ll-box-swapped { border-color: #10b981 !important; background: #dcfce7 !important; color: #065f46 !important; box-shadow: 0 0 0 3px rgba(16, 185, 129, 0.25) !important; }

.ll-arr-idx { font-size: 11px; color: var(--muted); margin-top: 4px; font-family: 'Consolas', monospace; font-weight: 600; }
.ll-empty-state { display: flex; align-items: center; justify-content: center; width: 100%; min-height: 60px; color: var(--muted); font-size: 13px; font-style: italic; }
.ll-vresizer { height: 5px; cursor: row-resize; background: var(--border); flex-shrink: 0; transition: background .15s; position: relative; z-index: 20; }
.ll-vresizer:hover, .ll-vresizer.drag { background: var(--coral); }
.ll-legend { display: flex; flex-wrap: wrap; gap: 6px 14px; padding: 6px 12px; border-bottom: 1px solid var(--border); flex-shrink: 0; background: var(--surface2); }
.ll-leg { display: flex; align-items: center; gap: 5px; font-size: 11px; color: var(--text2); font-weight: 500; }
.ll-legdot { width: 11px; height: 11px; border-radius: 3px; flex-shrink: 0; display: inline-block; }
.ll-legdot-existing { background: #eff6ff; border: 1.5px solid var(--blue); }
.ll-legdot-left { background: #dbeafe; border: 1.5px solid #3b82f6; }
.ll-legdot-right { background: #f3e8ff; border: 1.5px solid #8b5cf6; }
.ll-legdot-swap { background: #fef2f2; border: 1.5px solid #ef4444; }
.ll-legdot-swapped { background: #dcfce7; border: 1.5px solid #10b981; }

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
