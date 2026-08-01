<script setup>
import { ref, reactive, computed, onMounted, onBeforeUnmount, watch } from 'vue';

defineProps({
  topic: { type: String, default: 'Longest Common Subsequence (LCS)' },
  subTopic: { type: String, default: '2D Dynamic Programming Grid' }
});

const CODES = {
  java: [
    ['', 'class LongestCommonSubsequence {'],
    ['c_entry', '    int lcs(String s1, String s2) {'],
    ['c_m', '        int m = s1.length();'],
    ['c_n', '        int n = s2.length();'],
    ['c_dp_init', '        int[][] dp = new int[m + 1][n + 1];'],
    ['', ''],
    ['c_outer_loop', '        for (int i = 1; i <= m; i++) {'],
    ['c_inner_loop', '            for (int j = 1; j <= n; j++) {'],
    ['c_char_check', '                if (s1.charAt(i - 1) == s2.charAt(j - 1)) {'],
    ['c_match_calc', '                    dp[i][j] = dp[i - 1][j - 1] + 1;'],
    ['', '                } else {'],
    ['c_mismatch_calc', '                    dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);'],
    ['', '                }'],
    ['', '            }'],
    ['', '        }'],
    ['c_return', '        return dp[m][n];'],
    ['c_done', '    }'],
    ['', '}']
  ],
  c: [
    ['c_entry', 'int lcs(const char* s1, const char* s2) {'],
    ['c_m', '    int m = strlen(s1);'],
    ['c_n', '    int n = strlen(s2);'],
    ['c_dp_init', '    int dp[100][100] = {0};'],
    ['c_outer_loop', '    for (int i = 1; i <= m; i++) {'],
    ['c_inner_loop', '        for (int j = 1; j <= n; j++) {'],
    ['c_char_check', '            if (s1[i - 1] == s2[j - 1]) {'],
    ['c_match_calc', '                dp[i][j] = dp[i - 1][j - 1] + 1;'],
    ['', '            } else {'],
    ['c_mismatch_calc', '                dp[i][j] = (dp[i - 1][j] > dp[i][j - 1]) ? dp[i - 1][j] : dp[i][j - 1];'],
    ['', '            }'],
    ['', '        }'],
    ['', '    }'],
    ['c_return', '    return dp[m][n];'],
    ['c_done', '}']
  ],
  cpp: [
    ['', 'class LongestCommonSubsequence {'],
    ['', 'public:'],
    ['c_entry', '    int lcs(string s1, string s2) {'],
    ['c_m', '        int m = s1.length();'],
    ['c_n', '        int n = s2.length();'],
    ['c_dp_init', '        vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));'],
    ['c_outer_loop', '        for (int i = 1; i <= m; i++) {'],
    ['c_inner_loop', '            for (int j = 1; j <= n; j++) {'],
    ['c_char_check', '                if (s1[i - 1] == s2[j - 1]) {'],
    ['c_match_calc', '                    dp[i][j] = dp[i - 1][j - 1] + 1;'],
    ['', '                } else {'],
    ['c_mismatch_calc', '                    dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);'],
    ['', '                }'],
    ['', '            }'],
    ['', '        }'],
    ['c_return', '        return dp[m][n];'],
    ['c_done', '    }'],
    ['', '};']
  ],
  python: [
    ['', 'class LongestCommonSubsequence:'],
    ['c_entry', '    def lcs(self, s1: str, s2: str) -> int:'],
    ['c_m', '        m = len(s1)'],
    ['c_n', '        n = len(s2)'],
    ['c_dp_init', '        dp = [[0] * (n + 1) for _ in range(m + 1)]'],
    ['c_outer_loop', '        for i in range(1, m + 1):'],
    ['c_inner_loop', '            for j in range(1, n + 1):'],
    ['c_char_check', '                if s1[i - 1] == s2[j - 1]:'],
    ['c_match_calc', '                    dp[i][j] = dp[i - 1][j - 1] + 1'],
    ['', '                else:'],
    ['c_mismatch_calc', '                    dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])'],
    ['c_return', '        return dp[m][n]'],
    ['c_done', '']
  ],
  javascript: [
    ['', 'class LongestCommonSubsequence {'],
    ['c_entry', '  lcs(s1, s2) {'],
    ['c_m', '    const m = s1.length;'],
    ['c_n', '    const n = s2.length;'],
    ['c_dp_init', '    const dp = Array.from({ length: m + 1 }, () => new Array(n + 1).fill(0));'],
    ['c_outer_loop', '    for (let i = 1; i <= m; i++) {'],
    ['c_inner_loop', '      for (let j = 1; j <= n; j++) {'],
    ['c_char_check', '        if (s1[i - 1] === s2[j - 1]) {'],
    ['c_match_calc', '          dp[i][j] = dp[i - 1][j - 1] + 1;'],
    ['', '        } else {'],
    ['c_mismatch_calc', '          dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);'],
    ['', '        }'],
    ['', '      }'],
    ['', '    }'],
    ['c_return', '    return dp[m][n];'],
    ['c_done', '  }'],
    ['', '}']
  ]
};

const PSEUDOCODE = [
  'function LCS(s1, s2):',
  '    m = length(s1), n = length(s2)',
  '    dp = 2D array of size (m+1) x (n+1) initialized to 0',
  '    for i = 1 to m:',
  '        for j = 1 to n:',
  '            if s1[i-1] == s2[j-1]:',
  '                dp[i][j] = dp[i-1][j-1] + 1',
  '            else:',
  '                dp[i][j] = max(dp[i-1][j], dp[i][j-1])',
  '    return dp[m][n]',
];

function frame(title, rows) { return { title, rows }; }

function reconstructLCS(s1, s2, dp) {
  let i = s1.length, j = s2.length;
  const lcsChars = [];
  const pathSet = new Set();
  pathSet.add(`${i},${j}`);

  while (i > 0 && j > 0) {
    if (s1[i - 1] === s2[j - 1]) {
      lcsChars.unshift(s1[i - 1]);
      i--; j--;
    } else if (dp[i - 1][j] >= dp[i][j - 1]) {
      i--;
    } else {
      j--;
    }
    pathSet.add(`${i},${j}`);
  }
  return { lcsStr: lcsChars.join(''), pathSet };
}

function buildSteps(initialS1, initialS2) {
  const steps = [];
  const s1 = String(initialS1).trim();
  const s2 = String(initialS2).trim();
  const m = s1.length;
  const n = s2.length;
  const fnLabel = 'lcs(s1, s2)';

  const dp = Array.from({ length: m + 1 }, () => new Array(n + 1).fill(0));

  function getGridCells(curI = -1, curJ = -1, isMatch = false, isMismatch = false, pathSet = null) {
    const grid = [];
    for (let r = 0; r <= m; r++) {
      const rowCells = [];
      for (let c = 0; c <= n; c++) {
        let state = 'normal';
        if (pathSet && pathSet.has(`${r},${c}`)) {
          state = 'path';
        } else if (r === curI && c === curJ) {
          if (isMatch) state = 'match';
          else if (isMismatch) state = 'mismatch';
          else state = 'active';
        } else if (curI > 0 && curJ > 0) {
          if (isMatch && r === curI - 1 && c === curJ - 1) state = 'parent_diag';
          else if (isMismatch && ((r === curI - 1 && c === curJ) || (r === curI && c === curJ - 1))) state = 'parent_neighbor';
        }
        rowCells.push({ r, c, val: dp[r][c], state });
      }
      grid.push(rowCells);
    }
    return grid;
  }

  function getS1Cells(curI = -1) {
    return s1.split('').map((val, idx) => ({
      idx, val, state: idx === curI - 1 ? 'active' : 'normal'
    }));
  }

  function getS2Cells(curJ = -1) {
    return s2.split('').map((val, idx) => ({
      idx, val, state: idx === curJ - 1 ? 'active' : 'normal'
    }));
  }

  // 1. Entry
  steps.push({
    badge: `lcs called with s1="${s1}", s2="${s2}"`,
    code: 'c_entry',
    vars: [
      frame('main()', []),
      frame(fnLabel, [['s1', `"${s1}"`], ['s2', `"${s2}"`]])
    ],
    grid: getGridCells(),
    s1Cells: getS1Cells(), s2Cells: getS2Cells(),
    i: -1, j: -1, s1, s2, m, n, lcsStr: ''
  });

  if (m === 0 || n === 0) {
    steps.push({
      badge: `Empty string input → return 0`,
      code: 'c_return',
      vars: [frame('main()', []), frame(fnLabel, [['return', '0']])],
      grid: getGridCells(),
      s1Cells: getS1Cells(), s2Cells: getS2Cells(),
      i: -1, j: -1, s1, s2, m, n, lcsStr: ''
    });
    return steps;
  }

  // 2. int m = s1.length()
  steps.push({
    badge: `int m = s1.length() → m = ${m}`,
    code: 'c_m',
    vars: [
      frame('main()', []),
      frame(fnLabel, [['m', String(m)], ['s1', `"${s1}"`]])
    ],
    grid: getGridCells(),
    s1Cells: getS1Cells(), s2Cells: getS2Cells(),
    i: -1, j: -1, s1, s2, m, n, lcsStr: ''
  });

  // 3. int n = s2.length()
  steps.push({
    badge: `int n = s2.length() → n = ${n}`,
    code: 'c_n',
    vars: [
      frame('main()', []),
      frame(fnLabel, [['m', String(m)], ['n', String(n)], ['s2', `"${s2}"`]])
    ],
    grid: getGridCells(),
    s1Cells: getS1Cells(), s2Cells: getS2Cells(),
    i: -1, j: -1, s1, s2, m, n, lcsStr: ''
  });

  // 4. int[][] dp = new int[m + 1][n + 1]
  steps.push({
    badge: `int[][] dp = new int[${m + 1}][${n + 1}] → Initialized 2D DP matrix with base cases dp[0][j]=0, dp[i][0]=0`,
    code: 'c_dp_init',
    vars: [
      frame('main()', []),
      frame(fnLabel, [['dp', `matrix ${m + 1}x${n + 1}`]])
    ],
    grid: getGridCells(),
    s1Cells: getS1Cells(), s2Cells: getS2Cells(),
    i: 0, j: 0, s1, s2, m, n, lcsStr: ''
  });

  // 5. Nested Loops i = 1..m and j = 1..n
  for (let i = 1; i <= m; i++) {
    steps.push({
      badge: `for (int i = ${i}; i <= m (${m}); i++) → processing s1[${i - 1}] = '${s1[i - 1]}'`,
      code: 'c_outer_loop',
      vars: [
        frame('main()', []),
        frame(fnLabel, [['i', String(i)], ['s1[i-1]', `'${s1[i - 1]}'`]])
      ],
      grid: getGridCells(i, 0),
      s1Cells: getS1Cells(i), s2Cells: getS2Cells(),
      i, j: 0, s1, s2, m, n, lcsStr: ''
    });

    for (let j = 1; j <= n; j++) {
      steps.push({
        badge: `for (int j = ${j}; j <= n (${n}); j++) → comparing s1[${i - 1}] ('${s1[i - 1]}') vs s2[${j - 1}] ('${s2[j - 1]}')`,
        code: 'c_inner_loop',
        vars: [
          frame('main()', []),
          frame(fnLabel, [['i', String(i)], ['j', String(j)], ['s1[i-1]', `'${s1[i - 1]}'`], ['s2[j-1]', `'${s2[j - 1]}'`]])
        ],
        grid: getGridCells(i, j),
        s1Cells: getS1Cells(i), s2Cells: getS2Cells(j),
        i, j, s1, s2, m, n, lcsStr: ''
      });

      const isMatch = s1[i - 1] === s2[j - 1];
      steps.push({
        badge: `if (s1.charAt(${i - 1}) == s2.charAt(${j - 1})) → '${s1[i - 1]}' == '${s2[j - 1]}' → ${isMatch ? 'MATCH!' : 'MISMATCH'}`,
        code: 'c_char_check',
        vars: [
          frame('main()', []),
          frame(fnLabel, [['i', String(i)], ['j', String(j)], ['match', String(isMatch)]])
        ],
        grid: getGridCells(i, j, isMatch, !isMatch),
        s1Cells: getS1Cells(i), s2Cells: getS2Cells(j),
        i, j, s1, s2, m, n, lcsStr: ''
      });

      if (isMatch) {
        dp[i][j] = dp[i - 1][j - 1] + 1;
        steps.push({
          badge: `dp[${i}][${j}] = dp[${i - 1}][${j - 1}] + 1 → ${dp[i - 1][j - 1]} + 1 = ${dp[i][j]}`,
          code: 'c_match_calc',
          vars: [
            frame('main()', []),
            frame(fnLabel, [['i', String(i)], ['j', String(j)], ['dp[i-1][j-1]', String(dp[i - 1][j - 1])], ['dp[i][j]', String(dp[i][j])]])
          ],
          grid: getGridCells(i, j, true, false),
          s1Cells: getS1Cells(i), s2Cells: getS2Cells(j),
          i, j, s1, s2, m, n, lcsStr: ''
        });
      } else {
        dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
        steps.push({
          badge: `dp[${i}][${j}] = Math.max(dp[${i - 1}][${j}], dp[${i}][${j - 1}]) → max(${dp[i - 1][j]}, ${dp[i][j - 1]}) = ${dp[i][j]}`,
          code: 'c_mismatch_calc',
          vars: [
            frame('main()', []),
            frame(fnLabel, [['i', String(i)], ['j', String(j)], ['dp[i-1][j]', String(dp[i - 1][j])], ['dp[i][j-1]', String(dp[i][j - 1])], ['dp[i][j]', String(dp[i][j])]])
          ],
          grid: getGridCells(i, j, false, true),
          s1Cells: getS1Cells(i), s2Cells: getS2Cells(j),
          i, j, s1, s2, m, n, lcsStr: ''
        });
      }
    }
  }

  // 6. Traceback Reconstruction of LCS String
  const { lcsStr, pathSet } = reconstructLCS(s1, s2, dp);

  steps.push({
    badge: `Backtracking 2D DP table to reconstruct LCS string: "${lcsStr}" (length = ${dp[m][n]})`,
    code: 'c_return',
    vars: [
      frame('main()', []),
      frame(fnLabel, [['dp[m][n]', String(dp[m][n])], ['reconstructed_lcs', `"${lcsStr}"`]])
    ],
    grid: getGridCells(-1, -1, false, false, pathSet),
    s1Cells: getS1Cells(), s2Cells: getS2Cells(),
    i: m, j: n, s1, s2, m, n, lcsStr
  });

  steps.push({
    badge: `LCS Algorithm Complete! Longest Common Subsequence: "${lcsStr}" (length = ${dp[m][n]})`,
    code: 'c_done',
    vars: [
      frame('main()', []),
      frame(fnLabel, [['result_lcs_length', String(dp[m][n])], ['result_lcs_str', `"${lcsStr}"`]])
    ],
    grid: getGridCells(-1, -1, false, false, pathSet),
    s1Cells: getS1Cells(), s2Cells: getS2Cells(),
    i: m, j: n, s1, s2, m, n, lcsStr
  });

  return steps;
}

const inpS1 = ref('AGGTAB');
const inpS2 = ref('GXTXAYB');
const lang = ref('java');
const speed = ref(650);
const si = ref(0);
const playing = ref(false);
const vizHeight = ref(290);
const tableHeight = ref(60);
const leftWidth = ref(54);
const rightTab = ref('code');

const stepsData = reactive({ steps: buildSteps('AGGTAB', 'GXTXAYB') });
const steps = computed(() => stepsData.steps);
const s = computed(() => steps.value[Math.max(0, Math.min(si.value, steps.value.length - 1))] || {});
const codeLines = computed(() => CODES[lang.value] || []);

let playTimer = null;

function applyInput() {
  const str1 = String(inpS1.value || '').trim();
  const str2 = String(inpS2.value || '').trim();
  if (!str1.length || !str2.length) {
    alert('Please enter non-empty strings for both S1 and S2.');
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
  cleanupFns.push(initVResizer(vizResizerRef, vizHeight, 180, 520));
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
            <label>String S1</label>
            <input type="text" v-model="inpS1" style="width: 110px;" class="ll-text-input" placeholder="e.g. AGGTAB" />

            <label>String S2</label>
            <input type="text" v-model="inpS2" style="width: 110px;" class="ll-text-input" placeholder="e.g. GXTXAYB" />

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
                    <div class="ll-ptr-chip">i = <b class="ll-c-blue">{{ s.i > 0 ? s.i : '0' }}</b></div>
                    <div class="ll-ptr-chip">j = <b class="ll-c-purple">{{ s.j > 0 ? s.j : '0' }}</b></div>
                    <div class="ll-ptr-chip">s1[i-1] = <b class="ll-c-blue">{{ s.i > 0 ? `'${s.s1[s.i - 1]}'` : "''" }}</b></div>
                    <div class="ll-ptr-chip">s2[j-1] = <b class="ll-c-purple">{{ s.j > 0 ? `'${s.s2[s.j - 1]}'` : "''" }}</b></div>
                    <div class="ll-ptr-chip">LCS String = <b class="ll-c-green">"{{ s.lcsStr }}"</b></div>
                  </div>

                  <!-- 2D DP Matrix Visualization -->
                  <div class="ll-dp-table-wrap">
                    <table class="ll-dp-grid">
                      <thead>
                        <tr>
                          <th class="ll-dp-header">dp</th>
                          <th class="ll-dp-header">""</th>
                          <th v-for="(cCell, cIdx) in s.s2Cells" :key="'col-' + cIdx" class="ll-dp-header" :class="{ 'll-header-active': cCell.state === 'active' }">
                            {{ cCell.val }}
                          </th>
                        </tr>
                      </thead>
                      <tbody>
                        <tr v-for="(row, rIdx) in s.grid" :key="'row-' + rIdx">
                          <td class="ll-dp-header" :class="{ 'll-header-active': rIdx > 0 && s.s1Cells[rIdx - 1] && s.s1Cells[rIdx - 1].state === 'active' }">
                            {{ rIdx === 0 ? '""' : s.s1[rIdx - 1] }}
                          </td>
                          <td v-for="(cell, cIdx) in row" :key="'cell-' + rIdx + '-' + cIdx" class="ll-dp-cell" :class="{ 'll-cell-active': cell.state === 'active', 'll-cell-match': cell.state === 'match', 'll-cell-mismatch': cell.state === 'mismatch', 'll-cell-diag': cell.state === 'parent_diag', 'll-cell-neighbor': cell.state === 'parent_neighbor', 'll-cell-path': cell.state === 'path' }">
                            {{ cell.val }}
                          </td>
                        </tr>
                      </tbody>
                    </table>
                  </div>
                </div>
              </div>

              <div class="ll-vresizer" ref="vizResizerRef"></div>

              <!-- Color Legend -->
              <div class="ll-legend">
                <span class="ll-leg"><span class="ll-legdot ll-legdot-existing"></span>Normal</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-active"></span>Active (i, j)</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-match"></span>Match (+1)</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-mismatch"></span>Mismatch (Max)</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-path"></span>LCS Backtrack Path</span>
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
                <div class="ll-badge">{{ s.badge }}</div>
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
                      <tr><td>All Cases</td><td>O(m &times; n)</td><td>Fills a 2D matrix of size (m+1) x (n+1) where each cell requires O(1) computation.</td></tr>
                      <tr><td>Space Complexity</td><td>O(m &times; n)</td><td>Stores the 2D dynamic programming state table.</td></tr>
                    </tbody>
                  </table>
                  <p class="ll-note">
                    Key observation: <b>Longest Common Subsequence (LCS)</b> computes <code>dp[i][j] = dp[i-1][j-1] + 1</code> on character match, or <code>max(dp[i-1][j], dp[i][j-1])</code> on mismatch!
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
.ll-viz-wrap { flex-shrink: 0; background: var(--surface); border-bottom: 1px solid var(--border); position: relative; overflow-x: auto; overflow-y: auto; }
.ll-perm-area { display: flex; flex-direction: column; align-items: stretch; min-height: 100%; width: 100%; min-width: 0; box-sizing: border-box; }
.ll-ptrs { display: flex; gap: 8px; flex-wrap: wrap; padding: 8px 16px 4px; min-height: 36px; width: 100%; box-sizing: border-box; min-width: 0; }
.ll-ptr-chip { background: var(--surface2); border: 1px solid var(--border); border-radius: var(--radius-sm); padding: 3px 10px; font-size: 12px; font-family: monospace; box-shadow: var(--shadow-sm); white-space: nowrap; flex-shrink: 0; }
.ll-c-blue { color: var(--blue); } .ll-c-orange { color: var(--orange); } .ll-c-green { color: var(--green); } .ll-c-purple { color: var(--purple); } .ll-c-red { color: var(--red); }

/* 2D DP Table Styling */
.ll-dp-table-wrap { padding: 4px 16px 12px; overflow-x: auto; }
.ll-dp-grid { border-collapse: collapse; font-family: monospace; font-size: 13px; }
.ll-dp-header { background: #f1f5f9; color: var(--text2); font-weight: 700; padding: 6px 10px; text-align: center; border: 1px solid var(--border2); min-width: 34px; }
.ll-header-active { background: #dbeafe; color: #1e40af; border-color: #3b82f6; }
.ll-dp-cell { border: 1px solid var(--border2); padding: 6px 10px; text-align: center; font-weight: 700; background: #ffffff; color: var(--text); transition: all 0.2s ease; min-width: 34px; }
.ll-cell-active { background: #fff7ed !important; border: 2px solid #f97316 !important; color: #c2410c !important; transform: scale(1.05); }
.ll-cell-match { background: #dcfce7 !important; border: 2px solid #10b981 !important; color: #065f46 !important; transform: scale(1.05); }
.ll-cell-mismatch { background: #eff6ff !important; border: 2px solid #3b82f6 !important; color: #1e40af !important; transform: scale(1.05); }
.ll-cell-diag { background: #f0fdf4 !important; border: 1.5px dashed #22c55e !important; }
.ll-cell-neighbor { background: #faf5ff !important; border: 1.5px dashed #a855f7 !important; }
.ll-cell-path { background: #d1fae5 !important; border: 2px solid #059669 !important; color: #047857 !important; font-size: 14px; font-weight: 900; }

.ll-vresizer { height: 5px; cursor: row-resize; background: var(--border); flex-shrink: 0; transition: background .15s; position: relative; z-index: 20; }
.ll-vresizer:hover, .ll-vresizer.drag { background: var(--coral); }
.ll-legend { display: flex; flex-wrap: wrap; gap: 6px 14px; padding: 6px 12px; border-bottom: 1px solid var(--border); flex-shrink: 0; background: var(--surface2); }
.ll-leg { display: flex; align-items: center; gap: 5px; font-size: 11px; color: var(--text2); font-weight: 500; }
.ll-legdot { width: 11px; height: 11px; border-radius: 3px; flex-shrink: 0; display: inline-block; }
.ll-legdot-existing { background: #ffffff; border: 1.5px solid var(--border2); }
.ll-legdot-active { background: #fff7ed; border: 1.5px solid #f97316; }
.ll-legdot-match { background: #dcfce7; border: 1.5px solid #10b981; }
.ll-legdot-mismatch { background: #eff6ff; border: 1.5px solid #3b82f6; }
.ll-legdot-path { background: #d1fae5; border: 1.5px solid #059669; }

.ll-table-area { flex-shrink: 0; padding: 8px 14px; border-bottom: 1px solid var(--border); overflow-x: hidden; overflow-y: auto; background: var(--surface); min-width: 0; box-sizing: border-box; }
.ll-table-title { font-size: 10px; color: var(--muted); margin-bottom: 4px; font-style: italic; }
.ll-stack-line { font-family: 'Consolas', monospace; font-size: 12px; line-height: 1.8; }
.ll-frame { font-family: 'Consolas', monospace; font-size: 11.5px; color: var(--text2); padding: 1px 0; white-space: nowrap; }
.ll-frame-cur { color: var(--orange); background: var(--orange-light); border-radius: 4px; padding: 1px 5px; }
.ll-fname { color: var(--text2); }
.ll-now { color: var(--orange); font-size: 10px; margin-left: 6px; }
.ll-badge-wrap { padding: 6px 10px; border-bottom: 1px solid var(--border); flex-shrink: 0; min-height: 36px; display: flex; align-items: center; background: var(--surface); }
.ll-badge { display: inline-block; padding: 4px 12px; border-radius: var(--radius-sm); border-left: 3px solid var(--coral); background: var(--coral-light); font-size: 11px; color: var(--coral-dark); line-height: 1.4; word-break: break-word; font-weight: 500; }
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
