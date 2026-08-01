<script setup>
import { ref, reactive, computed, onMounted, onBeforeUnmount, watch } from 'vue';

defineProps({
  topic: { type: String, default: 'Longest Common Prefix (LCP)' },
  subTopic: { type: String, default: 'Horizontal Scanning Algorithm' }
});

const CODES = {
  java: [
    ['', 'class LongestCommonPrefix {'],
    ['c_entry', '    String longestCommonPrefix(String[] strs) {'],
    ['c_empty_check', '        if (strs == null || strs.length == 0) return "";'],
    ['c_init', '        String prefix = strs[0];'],
    ['', ''],
    ['c_loop', '        for (int i = 1; i < strs.length; i++) {'],
    ['c_while_check', '            while (strs[i].indexOf(prefix) != 0) {'],
    ['c_shorten', '                prefix = prefix.substring(0, prefix.length() - 1);'],
    ['c_prefix_empty', '                if (prefix.isEmpty()) return "";'],
    ['', '            }'],
    ['', '        }'],
    ['c_return', '        return prefix;'],
    ['c_done', '    }'],
    ['', '}']
  ],
  c: [
    ['c_entry', 'char* longestCommonPrefix(char** strs, int strsSize) {'],
    ['c_empty_check', '    if (strsSize == 0) return "";'],
    ['c_init', '    static char prefix[100]; strcpy(prefix, strs[0]);'],
    ['c_loop', '    for (int i = 1; i < strsSize; i++) {'],
    ['c_while_check', '        while (strncmp(strs[i], prefix, strlen(prefix)) != 0) {'],
    ['c_shorten', '            prefix[strlen(prefix) - 1] = \'\\0\';'],
    ['c_prefix_empty', '            if (strlen(prefix) == 0) return "";'],
    ['', '        }'],
    ['', '    }'],
    ['c_return', '    return prefix;'],
    ['c_done', '}']
  ],
  cpp: [
    ['', 'class LongestCommonPrefix {'],
    ['', 'public:'],
    ['c_entry', '    string longestCommonPrefix(vector<string>& strs) {'],
    ['c_empty_check', '        if (strs.empty()) return "";'],
    ['c_init', '        string prefix = strs[0];'],
    ['c_loop', '        for (int i = 1; i < strs.size(); i++) {'],
    ['c_while_check', '            while (strs[i].find(prefix) != 0) {'],
    ['c_shorten', '                prefix = prefix.substr(0, prefix.length() - 1);'],
    ['c_prefix_empty', '                if (prefix.empty()) return "";'],
    ['', '            }'],
    ['', '        }'],
    ['c_return', '        return prefix;'],
    ['c_done', '    }'],
    ['', '};']
  ],
  python: [
    ['', 'class LongestCommonPrefix:'],
    ['c_entry', '    def longestCommonPrefix(self, strs):'],
    ['c_empty_check', '        if not strs: return ""'],
    ['c_init', '        prefix = strs[0]'],
    ['c_loop', '        for i in range(1, len(strs)):'],
    ['c_while_check', '            while not strs[i].startswith(prefix):'],
    ['c_shorten', '                prefix = prefix[:-1]'],
    ['c_prefix_empty', '                if not prefix: return ""'],
    ['c_return', '        return prefix'],
    ['c_done', '']
  ],
  javascript: [
    ['', 'class LongestCommonPrefix {'],
    ['c_entry', '  longestCommonPrefix(strs) {'],
    ['c_empty_check', '    if (!strs || strs.length === 0) return "";'],
    ['c_init', '    let prefix = strs[0];'],
    ['c_loop', '    for (let i = 1; i < strs.length; i++) {'],
    ['c_while_check', '      while (!strs[i].startsWith(prefix)) {'],
    ['c_shorten', '        prefix = prefix.substring(0, prefix.length - 1);'],
    ['c_prefix_empty', '        if (prefix === "") return "";'],
    ['', '      }'],
    ['', '    }'],
    ['c_return', '    return prefix;'],
    ['c_done', '  }'],
    ['', '}']
  ]
};

const PSEUDOCODE = [
  'function LongestCommonPrefix(strs):',
  '    if length(strs) == 0: return ""',
  '    prefix = strs[0]',
  '    for i = 1 to length(strs) - 1:',
  '        while strs[i] does not start with prefix:',
  '            prefix = prefix[0..length(prefix)-2]',
  '            if prefix is empty: return ""',
  '    return prefix',
];

function frame(title, rows) { return { title, rows }; }

function parseInputStrings(rawInput) {
  return String(rawInput || '')
    .split(',')
    .map(s => s.trim().replace(/^["']|["']$/g, ''))
    .filter(s => s.length > 0);
}

function buildSteps(rawInput) {
  const steps = [];
  const strs = parseInputStrings(rawInput);
  const fnLabel = 'longestCommonPrefix(strs)';

  function getStringRows(curI = -1, curPrefix = '') {
    return strs.map((strVal, sIdx) => {
      let state = 'normal';
      if (sIdx === 0) state = 'anchor';
      else if (sIdx === curI) state = 'comparing';

      const matchLen = curPrefix ? (strVal.startsWith(curPrefix) ? curPrefix.length : 0) : 0;

      const chars = strVal.split('').map((charVal, cIdx) => ({
        cIdx,
        charVal,
        isMatched: curPrefix.length > 0 && cIdx < curPrefix.length && strVal.startsWith(curPrefix)
      }));

      return { sIdx, strVal, state, matchLen, chars };
    });
  }

  // 1. Entry
  steps.push({
    badge: `longestCommonPrefix called with strs = [${strs.map(s => `"${s}"`).join(', ')}]`,
    code: 'c_entry',
    vars: [
      frame('main()', []),
      frame(fnLabel, [['strs', JSON.stringify(strs)]])
    ],
    strRows: getStringRows(),
    i: -1, prefix: '', strs
  });

  // 2. Empty check
  steps.push({
    badge: `if (strs == null || strs.length == 0) → checking bounds (length = ${strs.length})`,
    code: 'c_empty_check',
    vars: [
      frame('main()', []),
      frame(fnLabel, [['strs.length', String(strs.length)]])
    ],
    strRows: getStringRows(),
    i: -1, prefix: '', strs
  });

  if (strs.length === 0) {
    steps.push({
      badge: `strs is empty → return ""`,
      code: 'c_return',
      vars: [frame('main()', []), frame(fnLabel, [['return', '""']])],
      strRows: getStringRows(),
      i: -1, prefix: '', strs
    });
    return steps;
  }

  // 3. String prefix = strs[0]
  let prefix = strs[0];
  steps.push({
    badge: `String prefix = strs[0] → Set initial prefix candidate to "${prefix}"`,
    code: 'c_init',
    vars: [
      frame('main()', []),
      frame(fnLabel, [['prefix', `"${prefix}"`], ['strs[0]', `"${strs[0]}"`]])
    ],
    strRows: getStringRows(0, prefix),
    i: 0, prefix, strs
  });

  // 4. Main loop i = 1 to strs.length - 1
  for (let i = 1; i < strs.length; i++) {
    steps.push({
      badge: `for (int i = ${i}; i < strs.length (${strs.length}); i++) → comparing candidate prefix "${prefix}" against strs[${i}] ("${strs[i]}")`,
      code: 'c_loop',
      vars: [
        frame('main()', []),
        frame(fnLabel, [['i', String(i)], ['prefix', `"${prefix}"`], [`strs[${i}]`, `"${strs[i]}"`]])
      ],
      strRows: getStringRows(i, prefix),
      i, prefix, strs
    });

    while (!strs[i].startsWith(prefix)) {
      steps.push({
        badge: `while (strs[${i}].indexOf(prefix) != 0) → "${strs[i]}" does NOT start with "${prefix}" → TRUE (must shorten prefix)`,
        code: 'c_while_check',
        vars: [
          frame('main()', []),
          frame(fnLabel, [['i', String(i)], ['prefix', `"${prefix}"`], [`strs[${i}]`, `"${strs[i]}"`]])
        ],
        strRows: getStringRows(i, prefix),
        i, prefix, strs
      });

      const oldPrefix = prefix;
      prefix = prefix.substring(0, prefix.length - 1);
      steps.push({
        badge: `prefix = prefix.substring(0, prefix.length() - 1) → shortened prefix from "${oldPrefix}" to "${prefix}"`,
        code: 'c_shorten',
        vars: [
          frame('main()', []),
          frame(fnLabel, [['i', String(i)], ['old_prefix', `"${oldPrefix}"`], ['prefix', `"${prefix}"`]])
        ],
        strRows: getStringRows(i, prefix),
        i, prefix, strs
      });

      steps.push({
        badge: `if (prefix.isEmpty()) → checking if prefix is empty ("${prefix}") → ${prefix === '' ? 'TRUE (no common prefix)' : 'FALSE'}`,
        code: 'c_prefix_empty',
        vars: [
          frame('main()', []),
          frame(fnLabel, [['prefix', `"${prefix}"`]])
        ],
        strRows: getStringRows(i, prefix),
        i, prefix, strs
      });

      if (prefix === '') {
        steps.push({
          badge: `prefix is empty → Common prefix not found. Return ""`,
          code: 'c_return',
          vars: [frame('main()', []), frame(fnLabel, [['return', '""']])],
          strRows: getStringRows(i, ''),
          i, prefix: '', strs
        });
        return steps;
      }
    }

    steps.push({
      badge: `while (strs[${i}].indexOf(prefix) != 0) → "${strs[i]}" starts with "${prefix}" → MATCH CONFIRMED for strs[${i}]!`,
      code: 'c_while_check',
      vars: [
        frame('main()', []),
        frame(fnLabel, [['i', String(i)], ['prefix', `"${prefix}"`], [`strs[${i}]`, `"${strs[i]}"`]])
      ],
      strRows: getStringRows(i, prefix),
      i, prefix, strs
    });
  }

  // 5. Return prefix
  steps.push({
    badge: `return prefix → Longest Common Prefix found: "${prefix}"`,
    code: 'c_return',
    vars: [
      frame('main()', []),
      frame(fnLabel, [['return', `"${prefix}"`]])
    ],
    strRows: getStringRows(-1, prefix),
    i: strs.length, prefix, strs
  });

  steps.push({
    badge: `LCP Algorithm Complete! Longest Common Prefix: "${prefix}"`,
    code: 'c_done',
    vars: [
      frame('main()', []),
      frame(fnLabel, [['result_lcp', `"${prefix}"`]])
    ],
    strRows: getStringRows(-1, prefix),
    i: strs.length, prefix, strs
  });

  return steps;
}

const inpStrs = ref('flower, flow, flight');
const lang = ref('java');
const speed = ref(650);
const si = ref(0);
const playing = ref(false);
const vizHeight = ref(265);
const tableHeight = ref(60);
const leftWidth = ref(50);
const rightTab = ref('code');

const stepsData = reactive({ steps: buildSteps('flower, flow, flight') });
const steps = computed(() => stepsData.steps);
const s = computed(() => steps.value[Math.max(0, Math.min(si.value, steps.value.length - 1))] || {});
const codeLines = computed(() => CODES[lang.value] || []);

let playTimer = null;

function applyInput() {
  const rawStr = String(inpStrs.value || '').trim();
  if (!rawStr.length) {
    alert('Please enter comma-separated strings.');
    return;
  }

  playing.value = false;
  stepsData.steps = buildSteps(rawStr);
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
            <label>Input Strings (comma-separated)</label>
            <input type="text" v-model="inpStrs" style="width: 260px;" class="ll-text-input" placeholder="e.g. flower, flow, flight" />

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
                    <div class="ll-ptr-chip">i = <b class="ll-c-blue">{{ s.i >= 0 ? s.i : 'N/A' }}</b></div>
                    <div class="ll-ptr-chip">prefix candidate = <b class="ll-c-green">"{{ s.prefix }}"</b></div>
                  </div>

                  <!-- String Array Visualization Cards -->
                  <div class="ll-lcp-tracks">
                    <div v-for="row in s.strRows" :key="'s-' + row.sIdx" class="ll-lcp-row" :class="{ 'll-row-anchor': row.state === 'anchor', 'll-row-comparing': row.state === 'comparing' }">
                      <div class="ll-row-label">strs[{{ row.sIdx }}]:</div>
                      <div class="ll-row-chars">
                        <div v-for="c in row.chars" :key="c.cIdx" class="ll-arr-box" :class="{ 'll-box-matched': c.isMatched, 'll-box-anchor': row.state === 'anchor' }">
                          {{ c.charVal }}
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>

              <div class="ll-vresizer" ref="vizResizerRef"></div>

              <!-- Color Legend -->
              <div class="ll-legend">
                <span class="ll-leg"><span class="ll-legdot ll-legdot-anchor"></span>strs[0] (Anchor)</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-comparing"></span>strs[i] (Comparing Target)</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-matched"></span>Prefix Match</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-normal"></span>Normal</span>
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
                      {{ f.title }}(<span v-for="(r, idx) in f.rows" :key="idx">
                        <span v-if="idx > 0">, </span>
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
                <div class="ll-badge" :class="{ 'll-badge-error': s.badge.includes('empty') || s.badge.includes('NOT start') }">{{ s.badge }}</div>
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
                    v-for="(line, idx) in codeLines"
                    :key="idx"
                    class="ll-codeline"
                    :class="{ 'll-hl': line[0] && line[0] === s.code }"
                  >{{ line[1] === '' ? ' ' : line[1] }}</span></pre>
                </div>

                <!-- Pseudocode Scroll -->
                <div v-else-if="rightTab === 'pseudo'" class="ll-code-scroll">
                  <pre class="ll-pre"><span
                    v-for="(line, idx) in PSEUDOCODE"
                    :key="idx"
                    class="ll-codeline"
                  >{{ line }}</span></pre>
                </div>

                <!-- Complexity Scroll -->
                <div v-else class="ll-info-scroll">
                  <h3>Time &amp; Space Complexity</h3>
                  <table class="ll-complexity-table">
                    <thead><tr><th>Case</th><th>Time</th><th>Why</th></tr></thead>
                    <tbody>
                      <tr><td>Best Case</td><td>O(N &times; M)</td><td>Where N is number of strings and M is length of shortest string.</td></tr>
                      <tr><td>Average Case</td><td>O(N &times; M)</td><td>Horizontal scanning compares prefix against each string iteratively.</td></tr>
                      <tr><td>Worst Case</td><td>O(N &times; M)</td><td>When all strings are identical or share a long prefix.</td></tr>
                      <tr><td>Space</td><td>O(1)</td><td>Constant auxiliary space (excluding returned prefix string).</td></tr>
                    </tbody>
                  </table>
                  <p class="ll-note">
                    Key observation: <b>Horizontal Scanning</b> initializes <code>prefix = strs[0]</code> and iteratively shortens <code>prefix</code> until <code>strs[i].startsWith(prefix)</code> is true for all strings!
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
.ll-text-input { padding: 5px 10px; }
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

/* LCP Diagram */
.ll-lcp-tracks { display: flex; flex-direction: column; gap: 8px; padding: 8px 16px; width: 100%; }
.ll-lcp-row { display: flex; align-items: center; gap: 8px; padding: 4px 8px; border-radius: var(--radius-sm); border: 1px solid transparent; }
.ll-row-anchor { background: #faf5ff; border-color: #a855f7; }
.ll-row-comparing { background: #eff6ff; border-color: #3b82f6; }
.ll-row-label { font-size: 11px; font-weight: 700; color: var(--text2); font-family: monospace; min-width: 60px; }
.ll-row-chars { display: flex; gap: 4px; }
.ll-arr-box { width: 34px; height: 34px; display: flex; align-items: center; justify-content: center; border: 2px solid var(--border2); border-radius: var(--radius-sm); background: #ffffff; color: #1e293b; font-weight: 700; font-size: 14px; font-family: monospace; box-shadow: var(--shadow-sm); transition: all 0.25s ease; animation: ll-pop .3s ease; }
.ll-box-anchor { border-color: #a855f7; background: #faf5ff; color: #7e22ce; }
.ll-box-matched { border-color: #10b981 !important; background: #dcfce7 !important; color: #065f46 !important; transform: translateY(-2px); }

.ll-vresizer { height: 5px; cursor: row-resize; background: var(--border); flex-shrink: 0; transition: background .15s; position: relative; z-index: 20; }
.ll-vresizer:hover, .ll-vresizer.drag { background: var(--coral); }
.ll-legend { display: flex; flex-wrap: wrap; gap: 6px 14px; padding: 6px 12px; border-bottom: 1px solid var(--border); flex-shrink: 0; background: var(--surface2); }
.ll-leg { display: flex; align-items: center; gap: 5px; font-size: 11px; color: var(--text2); font-weight: 500; }
.ll-legdot { width: 11px; height: 11px; border-radius: 3px; flex-shrink: 0; display: inline-block; }
.ll-legdot-normal { background: #ffffff; border: 1.5px solid var(--border2); }
.ll-legdot-anchor { background: #faf5ff; border: 1.5px solid #a855f7; }
.ll-legdot-comparing { background: #eff6ff; border: 1.5px solid #3b82f6; }
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
