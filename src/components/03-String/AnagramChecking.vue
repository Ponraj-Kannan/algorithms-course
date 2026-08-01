<script setup>
import { ref, reactive, computed, onMounted, onBeforeUnmount, watch } from 'vue';

defineProps({
  topic: { type: String, default: 'Anagram Checking Algorithm' },
  subTopic: { type: String, default: 'Frequency Table Comparison' }
});

const CODES = {
  java: [
    ['', 'class AnagramChecking {'],
    ['c_entry', '    boolean isAnagram(String s1, String s2) {'],
    ['c_len_check', '        if (s1.length() != s2.length()) {'],
    ['c_len_return', '            return false;'],
    ['', '        }'],
    ['c_freq_init', '        int[] freq = new int[26];'],
    ['c_count_loop', '        for (int i = 0; i < s1.length(); i++) {'],
    ['c_inc_s1', '            freq[s1.charAt(i) - \'a\']++;'],
    ['c_dec_s2', '            freq[s2.charAt(i) - \'a\']--;'],
    ['', '        }'],
    ['c_verify_loop', '        for (int count : freq) {'],
    ['c_verify_check', '            if (count != 0) return false;'],
    ['', '        }'],
    ['c_return_true', '        return true;'],
    ['c_done', '    }'],
    ['', '}']
  ],
  c: [
    ['c_entry', 'bool isAnagram(const char* s1, const char* s2) {'],
    ['c_len_check', '    if (strlen(s1) != strlen(s2)) return false;'],
    ['c_freq_init', '    int freq[26] = {0};'],
    ['c_count_loop', '    for (int i = 0; s1[i] != \'\\0\'; i++) {'],
    ['c_inc_s1', '        freq[s1[i] - \'a\']++;'],
    ['c_dec_s2', '        freq[s2[i] - \'a\']--;'],
    ['', '    }'],
    ['c_verify_loop', '    for (int i = 0; i < 26; i++) {'],
    ['c_verify_check', '        if (freq[i] != 0) return false;'],
    ['', '    }'],
    ['c_return_true', '    return true;'],
    ['c_done', '}']
  ],
  cpp: [
    ['', 'class AnagramChecking {'],
    ['', 'public:'],
    ['c_entry', '    bool isAnagram(string s1, string s2) {'],
    ['c_len_check', '        if (s1.length() != s2.length()) return false;'],
    ['c_freq_init', '        vector<int> freq(26, 0);'],
    ['c_count_loop', '        for (int i = 0; i < s1.length(); i++) {'],
    ['c_inc_s1', '            freq[s1[i] - \'a\']++;'],
    ['c_dec_s2', '            freq[s2[i] - \'a\']--;'],
    ['', '        }'],
    ['c_verify_loop', '        for (int count : freq) {'],
    ['c_verify_check', '            if (count != 0) return false;'],
    ['', '        }'],
    ['c_return_true', '        return true;'],
    ['c_done', '    }'],
    ['', '};']
  ],
  python: [
    ['', 'class AnagramChecking:'],
    ['c_entry', '    def is_anagram(self, s1, s2):'],
    ['c_len_check', '        if len(s1) != len(s2):'],
    ['c_len_return', '            return False'],
    ['c_freq_init', '        freq = {}'],
    ['c_count_loop', '        for i in range(len(s1)):'],
    ['c_inc_s1', '            freq[s1[i]] = freq.get(s1[i], 0) + 1'],
    ['c_dec_s2', '            freq[s2[i]] = freq.get(s2[i], 0) - 1'],
    ['c_verify_loop', '        for char, count in freq.items():'],
    ['c_verify_check', '            if count != 0: return False'],
    ['c_return_true', '        return True'],
    ['c_done', '']
  ],
  javascript: [
    ['', 'class AnagramChecking {'],
    ['c_entry', '  isAnagram(s1, s2) {'],
    ['c_len_check', '    if (s1.length !== s2.length) return false;'],
    ['c_freq_init', '    const freq = new Array(26).fill(0);'],
    ['c_count_loop', '    for (let i = 0; i < s1.length; i++) {'],
    ['c_inc_s1', '      freq[s1.charCodeAt(i) - 97]++;'],
    ['c_dec_s2', '      freq[s2.charCodeAt(i) - 97]--;'],
    ['', '    }'],
    ['c_verify_loop', '    for (let count of freq) {'],
    ['c_verify_check', '      if (count !== 0) return false;'],
    ['', '    }'],
    ['c_return_true', '    return true;'],
    ['c_done', '  }'],
    ['', '}']
  ]
};

const PSEUDOCODE = [
  'function isAnagram(s1, s2):',
  '    if length(s1) != length(s2):',
  '        return false   // Length mismatch',
  '    freq = array of size 26 initialized to 0',
  '    for i = 0 to length(s1) - 1:',
  '        freq[s1[i] - \'a\']++',
  '        freq[s2[i] - \'a\']--',
  '    for count in freq:',
  '        if count != 0:',
  '            return false   // Non-zero frequency remaining',
  '    return true            // Same character frequencies',
];

function frame(title, rows) { return { title, rows }; }

function buildSteps(str1, str2) {
  const steps = [];
  const s1 = String(str1).toLowerCase().replace(/[^a-z]/g, '');
  const s2 = String(str2).toLowerCase().replace(/[^a-z]/g, '');
  const n1 = s1.length;
  const n2 = s2.length;
  const fnLabel = 'isAnagram(s1, s2)';

  const freqMap = reactive({});
  function getFreqObj() {
    const obj = {};
    for (let ch in freqMap) if (freqMap[ch] !== 0) obj[ch] = freqMap[ch];
    return obj;
  }

  function getCells(active1 = -1, active2 = -1, isMatched = false) {
    const cells1 = s1.split('').map((v, idx) => ({
      idx, val: v,
      state: isMatched ? 'matched' : (idx === active1 ? 'inc' : 'normal')
    }));
    const cells2 = s2.split('').map((v, idx) => ({
      idx, val: v,
      state: isMatched ? 'matched' : (idx === active2 ? 'dec' : 'normal')
    }));
    return { cells1, cells2 };
  }

  // 1. Entry
  steps.push({
    badge: `isAnagram called with s1="${s1}" and s2="${s2}"`,
    code: 'c_entry',
    vars: [
      frame('main()', []),
      frame(fnLabel, [['s1', `"${s1}"`], ['s2', `"${s2}"`]])
    ],
    ...getCells(),
    freq: {}, i: -1, result: null
  });

  // 2. Length check
  steps.push({
    badge: `if (s1.length != s2.length) → comparing ${n1} != ${n2} → ${n1 !== n2 ? 'TRUE (Lengths differ)' : 'FALSE'}`,
    code: 'c_len_check',
    vars: [
      frame('main()', []),
      frame(fnLabel, [['s1.length', String(n1)], ['s2.length', String(n2)]])
    ],
    ...getCells(),
    freq: {}, i: -1, result: null
  });

  if (n1 !== n2) {
    steps.push({
      badge: `return false → Lengths differ (${n1} vs ${n2}) → NOT anagrams`,
      code: 'c_len_return',
      vars: [
        frame('main()', []),
        frame(fnLabel, [['return', 'false'], ['reason', 'LENGTH_MISMATCH']])
      ],
      ...getCells(),
      freq: {}, i: -1, result: false
    });
    return steps;
  }

  // 3. freq = new int[26]
  steps.push({
    badge: `int[] freq = new int[26] → character frequency array initialized to 0`,
    code: 'c_freq_init',
    vars: [
      frame('main()', []),
      frame(fnLabel, [['freq', 'all 0']])
    ],
    ...getCells(),
    freq: {}, i: -1, result: null
  });

  // 4. Count Loop
  for (let i = 0; i < n1; i++) {
    const ch1 = s1[i];
    const ch2 = s2[i];

    steps.push({
      badge: `for (i = ${i}; i < ${n1}; i++) → processing index ${i} ('${ch1}' in s1, '${ch2}' in s2)`,
      code: 'c_count_loop',
      vars: [
        frame('main()', []),
        frame(fnLabel, [['i', String(i)], ['s1[i]', `'${ch1}'`], ['s2[i]', `'${ch2}'`]])
      ],
      ...getCells(i, i),
      freq: { ...freqMap }, i, result: null
    });

    freqMap[ch1] = (freqMap[ch1] || 0) + 1;
    steps.push({
      badge: `freq['${ch1}']++ → increment count for '${ch1}' to ${freqMap[ch1]}`,
      code: 'c_inc_s1',
      vars: [
        frame('main()', []),
        frame(fnLabel, [['i', String(i)], [`freq['${ch1}']`, String(freqMap[ch1])]])
      ],
      ...getCells(i, -1),
      freq: { ...freqMap }, i, result: null
    });

    freqMap[ch2] = (freqMap[ch2] || 0) - 1;
    steps.push({
      badge: `freq['${ch2}']-- → decrement count for '${ch2}' to ${freqMap[ch2]}`,
      code: 'c_dec_s2',
      vars: [
        frame('main()', []),
        frame(fnLabel, [['i', String(i)], [`freq['${ch2}']`, String(freqMap[ch2])]])
      ],
      ...getCells(-1, i),
      freq: { ...freqMap }, i, result: null
    });
  }

  // 5. Verify Loop
  steps.push({
    badge: `Verifying character frequency array counts...`,
    code: 'c_verify_loop',
    vars: [
      frame('main()', []),
      frame(fnLabel, [['freq_check', 'verifying all 0']])
    ],
    ...getCells(),
    freq: { ...freqMap }, i: n1, result: null
  });

  let isAnagram = true;
  for (let ch in freqMap) {
    if (freqMap[ch] !== 0) {
      isAnagram = false;
      steps.push({
        badge: `if (count != 0) → freq['${ch}'] = ${freqMap[ch]} != 0 → NOT anagrams`,
        code: 'c_verify_check',
        vars: [
          frame('main()', []),
          frame(fnLabel, [[`freq['${ch}']`, String(freqMap[ch])], ['return', 'false']])
        ],
        ...getCells(),
        freq: { ...freqMap }, i: n1, result: false
      });
      break;
    }
  }

  if (isAnagram) {
    steps.push({
      badge: `return true → All character frequencies balanced (all 0)! "${s1}" and "${s2}" ARE anagrams`,
      code: 'c_return_true',
      vars: [
        frame('main()', []),
        frame(fnLabel, [['return', 'true'], ['status', 'ANAGRAMS']])
      ],
      ...getCells(-1, -1, true),
      freq: { ...freqMap }, i: n1, result: true
    });
  }

  steps.push({
    badge: `Anagram Check Complete! Result: "${s1}" & "${s2}" ${isAnagram ? 'ARE ANAGRAMS (true)' : 'ARE NOT ANAGRAMS (false)'}`,
    code: 'c_done',
    vars: [
      frame('main()', []),
      frame(fnLabel, [['result', String(isAnagram)]])
    ],
    ...getCells(-1, -1, isAnagram),
    freq: { ...freqMap }, i: n1, result: isAnagram
  });

  return steps;
}

const inpS1 = ref('LISTEN');
const inpS2 = ref('SILENT');
const lang = ref('java');
const speed = ref(650);
const si = ref(0);
const playing = ref(false);
const vizHeight = ref(265);
const tableHeight = ref(60);
const leftWidth = ref(50);
const rightTab = ref('code');

const stepsData = reactive({ steps: buildSteps('LISTEN', 'SILENT') });
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
            <label>S1</label>
            <input type="text" v-model="inpS1" style="width: 110px;" class="ll-text-input" placeholder="e.g. LISTEN" />

            <label>S2</label>
            <input type="text" v-model="inpS2" style="width: 110px;" class="ll-text-input" placeholder="e.g. SILENT" />

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
                    <div class="ll-ptr-chip">Result = <b :class="s.result === true ? 'll-c-green' : (s.result === false ? 'll-c-red' : 'll-c-orange')">{{ s.result === null ? 'Comparing...' : String(s.result) }}</b></div>
                  </div>

                  <!-- Dual String Tracks + Frequency Counter Bar -->
                  <div class="ll-dual-tracks">
                    <div class="ll-track-title">s1:</div>
                    <div class="ll-arr-track">
                      <template v-for="cell in s.cells1" :key="'s1-' + cell.idx">
                        <div class="ll-arr-cell-wrap">
                          <div class="ll-ptr-tag-wrap">
                            <span v-if="cell.idx === s.i" class="ll-ptr-lbl ll-lbl-blue">i</span>
                          </div>
                          <div class="ll-arr-box" :class="{ 'll-box-inc': cell.state === 'inc', 'll-box-matched': cell.state === 'matched' }">
                            {{ cell.val }}
                          </div>
                          <div class="ll-arr-idx">[{{ cell.idx }}]</div>
                        </div>
                      </template>
                    </div>

                    <div class="ll-track-title">s2:</div>
                    <div class="ll-arr-track">
                      <template v-for="cell in s.cells2" :key="'s2-' + cell.idx">
                        <div class="ll-arr-cell-wrap">
                          <div class="ll-ptr-tag-wrap">
                            <span v-if="cell.idx === s.i" class="ll-ptr-lbl ll-lbl-blue">i</span>
                          </div>
                          <div class="ll-arr-box" :class="{ 'll-box-dec': cell.state === 'dec', 'll-box-matched': cell.state === 'matched' }">
                            {{ cell.val }}
                          </div>
                          <div class="ll-arr-idx">[{{ cell.idx }}]</div>
                        </div>
                      </template>
                    </div>

                    <!-- Character Frequency Table Bar -->
                    <div v-if="s.freq && Object.keys(s.freq).length" class="ll-freq-bar">
                      <span class="ll-freq-title">Character Frequency Map:</span>
                      <span v-for="(cnt, char) in s.freq" :key="char" class="ll-freq-chip" :class="{ 'll-freq-zero': cnt === 0 }">
                        '{{ char }}': <b>{{ cnt }}</b>
                      </span>
                    </div>
                  </div>
                </div>
              </div>

              <div class="ll-vresizer" ref="vizResizerRef"></div>

              <!-- Color Legend -->
              <div class="ll-legend">
                <span class="ll-leg"><span class="ll-legdot ll-legdot-existing"></span>Normal</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-inc"></span>s1 char (+1 freq)</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-dec"></span>s2 char (-1 freq)</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-matched"></span>Anagram Matched</span>
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
                      <tr><td>Average Case</td><td>O(n)</td><td>Scans both strings of length n to build frequency table.</td></tr>
                      <tr><td>Worst Case</td><td>O(n)</td><td>Full string traversal + 26-element alphabet check.</td></tr>
                      <tr><td>Space</td><td>O(1)</td><td>Fixed 26-element frequency array for English alphabet.</td></tr>
                    </tbody>
                  </table>
                  <p class="ll-note">
                    Key observation: Anagram checking increments frequency counts for characters in <code>s1</code> and decrements for <code>s2</code>. If all final counts are <code>0</code>, the strings are valid anagrams.
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
.ll-text-input { width: 110px; padding: 5px 10px; }
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

/* Dual Track + Frequency Bar */
.ll-dual-tracks { display: flex; flex-direction: column; gap: 6px; padding: 6px 16px; width: 100%; }
.ll-track-title { font-size: 12px; font-weight: 700; color: var(--text2); font-family: monospace; }
.ll-arr-track { display: flex; align-items: flex-start; flex-wrap: wrap; gap: 8px; width: 100%; }
.ll-arr-cell-wrap { display: flex; flex-direction: column; align-items: center; min-width: 0; }
.ll-ptr-tag-wrap { height: 28px; display: flex; align-items: flex-end; justify-content: center; gap: 4px; margin-bottom: 2px; }
.ll-ptr-lbl { font-size: 13px; font-weight: 800; font-family: 'Consolas', 'Fira Code', monospace; display: inline-flex; flex-direction: column; align-items: center; line-height: 1; gap: 1px; white-space: nowrap; animation: ll-pop 0.2s ease; }
.ll-ptr-lbl::after { content: '↓'; font-size: 11px; font-weight: 900; line-height: 1; margin-top: 1px; }
.ll-lbl-blue { color: #3b82f6; }
.ll-lbl-orange { color: #f97316; }
.ll-lbl-purple { color: #9333ea; }
.ll-lbl-green { color: #10b981; }
.ll-arr-box { width: 44px; height: 44px; display: flex; align-items: center; justify-content: center; border: 2px solid var(--blue); border-radius: var(--radius); background: #eff6ff; color: #1e293b; font-weight: 700; font-size: 16px; font-family: monospace; box-shadow: var(--shadow-sm); transition: all 0.25s ease; animation: ll-pop .3s ease; }
.ll-box-inc { border-color: #3b82f6 !important; background: #dbeafe !important; color: #1e40af !important; transform: translateY(-2px); }
.ll-box-dec { border-color: #8b5cf6 !important; background: #f3e8ff !important; color: #5b21b6 !important; transform: translateY(-2px); }
.ll-box-matched { border-color: #10b981 !important; background: #dcfce7 !important; color: #065f46 !important; }

.ll-freq-bar { display: flex; align-items: center; gap: 6px; flex-wrap: wrap; margin-top: 6px; padding: 6px 10px; background: var(--surface2); border: 1px solid var(--border); border-radius: var(--radius-sm); }
.ll-freq-title { font-size: 11px; font-weight: 700; color: var(--muted); }
.ll-freq-chip { font-size: 11.5px; font-family: monospace; padding: 2px 7px; background: #fff; border: 1px solid var(--border2); border-radius: 4px; color: var(--coral-dark); }
.ll-freq-zero { border-color: #10b981; color: #065f46; background: #f0fdf4; }

.ll-arr-idx { font-size: 10px; color: var(--muted); margin-top: 2px; font-family: 'Consolas', monospace; font-weight: 600; }
.ll-vresizer { height: 5px; cursor: row-resize; background: var(--border); flex-shrink: 0; transition: background .15s; position: relative; z-index: 20; }
.ll-vresizer:hover, .ll-vresizer.drag { background: var(--coral); }
.ll-legend { display: flex; flex-wrap: wrap; gap: 6px 14px; padding: 6px 12px; border-bottom: 1px solid var(--border); flex-shrink: 0; background: var(--surface2); }
.ll-leg { display: flex; align-items: center; gap: 5px; font-size: 11px; color: var(--text2); font-weight: 500; }
.ll-legdot { width: 11px; height: 11px; border-radius: 3px; flex-shrink: 0; display: inline-block; }
.ll-legdot-existing { background: #eff6ff; border: 1.5px solid var(--blue); }
.ll-legdot-inc { background: #dbeafe; border: 1.5px solid #3b82f6; }
.ll-legdot-dec { background: #f3e8ff; border: 1.5px solid #8b5cf6; }
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
