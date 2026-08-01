<script setup>
import { ref, reactive, computed, onMounted, onBeforeUnmount, watch } from 'vue';

defineProps({
  topic: { type: String, default: 'Longest Palindromic Substring' },
  subTopic: { type: String, default: 'Expand Around Center Algorithm' }
});

const CODES = {
  java: [
    ['', 'class LongestPalindromicSubstring {'],
    ['c_entry', '    String longestPalindrome(String s) {'],
    ['c_null_check', '        if (s == null || s.length() == 0) {'],
    ['', '            return "";'],
    ['', '        }'],
    ['c_init', '        int start = 0; int maxLen = 0;'],
    ['', ''],
    ['c_loop', '        for (int i = 0; i < s.length(); i++) {'],
    ['c_expand_odd', '            int len1 = expandAroundCenter(s, i, i);'],
    ['c_expand_even', '            int len2 = expandAroundCenter(s, i, i + 1);'],
    ['c_len_max', '            int len = Math.max(len1, len2);'],
    ['', ''],
    ['c_check_max', '            if (len > maxLen) {'],
    ['c_update_max', '                maxLen = len; start = i - (len - 1) / 2;'],
    ['', '            }'],
    ['', '        }'],
    ['c_return', '        return s.substring(start, start + maxLen);'],
    ['c_done', '    }'],
    ['', ''],
    ['c_exp_entry', '    int expandAroundCenter(String s, int left, int right) {'],
    ['c_exp_init', '        int L = left; int R = right;'],
    ['c_exp_while', '        while (L >= 0 && R < s.length() && s.charAt(L) == s.charAt(R)) {'],
    ['c_exp_l_dec', '            L--;'],
    ['c_exp_r_inc', '            R++;'],
    ['', '        }'],
    ['c_exp_return', '        return R - L - 1;'],
    ['', '    }'],
    ['', '}']
  ],
  c: [
    ['c_exp_entry', 'int expandAroundCenter(const char* s, int left, int right, int n) {'],
    ['c_exp_init', '    int L = left; int R = right;'],
    ['c_exp_while', '    while (L >= 0 && R < n && s[L] == s[R]) {'],
    ['c_exp_l_dec', '        L--;'],
    ['c_exp_r_inc', '        R++;'],
    ['', '    }'],
    ['c_exp_return', '    return R - L - 1;'],
    ['', '}'],
    ['', ''],
    ['c_entry', 'void longestPalindrome(const char* s, char* result) {'],
    ['c_null_check', '    int n = strlen(s); if (n == 0) { result[0] = \'\\0\'; return; }'],
    ['c_init', '    int start = 0; int maxLen = 0;'],
    ['c_loop', '    for (int i = 0; i < n; i++) {'],
    ['c_expand_odd', '        int len1 = expandAroundCenter(s, i, i, n);'],
    ['c_expand_even', '        int len2 = expandAroundCenter(s, i, i + 1, n);'],
    ['c_len_max', '        int len = (len1 > len2) ? len1 : len2;'],
    ['c_check_max', '        if (len > maxLen) {'],
    ['c_update_max', '            maxLen = len; start = i - (len - 1) / 2;'],
    ['', '        }'],
    ['', '    }'],
    ['c_return', '    strncpy(result, s + start, maxLen); result[maxLen] = \'\\0\';'],
    ['c_done', '}']
  ],
  cpp: [
    ['', 'class LongestPalindromicSubstring {'],
    ['', 'public:'],
    ['c_entry', '    string longestPalindrome(string s) {'],
    ['c_null_check', '        if (s.empty()) { return ""; }'],
    ['c_init', '        int start = 0; int maxLen = 0;'],
    ['c_loop', '        for (int i = 0; i < s.length(); i++) {'],
    ['c_expand_odd', '            int len1 = expandAroundCenter(s, i, i);'],
    ['c_expand_even', '            int len2 = expandAroundCenter(s, i, i + 1);'],
    ['c_len_max', '            int len = max(len1, len2);'],
    ['c_check_max', '            if (len > maxLen) {'],
    ['c_update_max', '                maxLen = len; start = i - (len - 1) / 2;'],
    ['', '            }'],
    ['', '        }'],
    ['c_return', '        return s.substr(start, maxLen);'],
    ['c_done', '    }'],
    ['', 'private:'],
    ['c_exp_entry', '    int expandAroundCenter(const string& s, int left, int right) {'],
    ['c_exp_init', '        int L = left; int R = right;'],
    ['c_exp_while', '        while (L >= 0 && R < s.length() && s[L] == s[R]) {'],
    ['c_exp_l_dec', '            L--;'],
    ['c_exp_r_inc', '            R++;'],
    ['', '        }'],
    ['c_exp_return', '        return R - L - 1;'],
    ['', '    }'],
    ['', '};']
  ],
  python: [
    ['', 'class LongestPalindromicSubstring:'],
    ['c_entry', '    def longestPalindrome(self, s: str) -> str:'],
    ['c_null_check', '        if not s: return ""'],
    ['c_init', '        start = 0; maxLen = 0'],
    ['c_loop', '        for i in range(len(s)):'],
    ['c_expand_odd', '            len1 = self.expandAroundCenter(s, i, i)'],
    ['c_expand_even', '            len2 = self.expandAroundCenter(s, i, i + 1)'],
    ['c_len_max', '            length = max(len1, len2)'],
    ['c_check_max', '            if length > maxLen:'],
    ['c_update_max', '                maxLen = length; start = i - (length - 1) // 2'],
    ['c_return', '        return s[start : start + maxLen]'],
    ['c_done', ''],
    ['c_exp_entry', '    def expandAroundCenter(self, s: str, left: int, right: int) -> int:'],
    ['c_exp_init', '        L, R = left, right'],
    ['c_exp_while', '        while L >= 0 and R < len(s) and s[L] == s[R]:'],
    ['c_exp_l_dec', '            L -= 1'],
    ['c_exp_r_inc', '            R += 1'],
    ['c_exp_return', '        return R - L - 1']
  ],
  javascript: [
    ['', 'class LongestPalindromicSubstring {'],
    ['c_entry', '  longestPalindrome(s) {'],
    ['c_null_check', '    if (!s || s.length === 0) {'],
    ['', '      return "";'],
    ['', '    }'],
    ['c_init', '    let start = 0; let maxLen = 0;'],
    ['c_loop', '    for (let i = 0; i < s.length; i++) {'],
    ['c_expand_odd', '      const len1 = this.expandAroundCenter(s, i, i);'],
    ['c_expand_even', '      const len2 = this.expandAroundCenter(s, i, i + 1);'],
    ['c_len_max', '      const len = Math.max(len1, len2);'],
    ['c_check_max', '      if (len > maxLen) {'],
    ['c_update_max', '        maxLen = len; start = i - Math.floor((len - 1) / 2);'],
    ['', '      }'],
    ['', '    }'],
    ['c_return', '    return s.substring(start, start + maxLen);'],
    ['c_done', '  }'],
    ['', '  expandAroundCenter(s, left, right) {'],
    ['c_exp_init', '    let L = left; let R = right;'],
    ['c_exp_while', '    while (L >= 0 && R < s.length && s[L] === s[R]) {'],
    ['c_exp_l_dec', '      L--;'],
    ['c_exp_r_inc', '      R++;'],
    ['', '    }'],
    ['c_exp_return', '    return R - L - 1;'],
    ['', '  }'],
    ['', '}']
  ]
};

const PSEUDOCODE = [
  'function LongestPalindrome(s):',
  '    if s is empty: return ""',
  '    start = 0, maxLen = 0',
  '    for i = 0 to length(s) - 1:',
  '        len1 = ExpandAroundCenter(s, i, i)',
  '        len2 = ExpandAroundCenter(s, i, i + 1)',
  '        len = max(len1, len2)',
  '        if len > maxLen:',
  '            maxLen = len',
  '            start = i - (len - 1) / 2',
  '    return substring(s, start, maxLen)',
  '',
  'function ExpandAroundCenter(s, left, right):',
  '    L = left, R = right',
  '    while L >= 0 and R < length(s) and s[L] == s[R]:',
  '        L--, R++',
  '    return R - L - 1',
];

function frame(title, rows) { return { title, rows }; }

function buildSteps(initialS) {
  const steps = [];
  const str = String(initialS).trim();
  const n = str.length;
  const fnLabel = 'longestPalindrome(s)';

  let bestStart = 0;
  let maxLen = 0;

  function getCells(curI = -1, curL = -99, curR = -99, isExpanding = false) {
    return str.split('').map((val, idx) => {
      let state = 'normal';
      if (maxLen > 0 && idx >= bestStart && idx < bestStart + maxLen) {
        state = 'best_match';
      }

      if (curL >= 0 && curR >= 0 && idx >= curL && idx <= curR && isExpanding) {
        state = 'expanding';
      }

      if (idx === curI) {
        state = 'center';
      }

      return { idx, val, state };
    });
  }

  function getBestPal() {
    return maxLen > 0 ? str.substring(bestStart, bestStart + maxLen) : '';
  }

  // 1. Entry
  steps.push({
    badge: `longestPalindrome called with s="${str}"`,
    code: 'c_entry',
    vars: [
      frame('main()', []),
      frame(fnLabel, [['s', `"${str}"`]])
    ],
    cells: getCells(),
    i: -1, L: -99, R: -99, n, maxLen: 0, bestPal: ''
  });

  if (n === 0) {
    steps.push({
      badge: `s is empty → return ""`,
      code: 'c_null_check',
      vars: [frame('main()', []), frame(fnLabel, [['return', '""']])],
      cells: getCells(),
      i: -1, L: -99, R: -99, n, maxLen: 0, bestPal: ''
    });
    return steps;
  }

  // 2. int start = 0; int maxLen = 0;
  steps.push({
    badge: `int start = 0; int maxLen = 0; → Initialized tracking variables`,
    code: 'c_init',
    vars: [
      frame('main()', []),
      frame(fnLabel, [['start', '0'], ['maxLen', '0']])
    ],
    cells: getCells(),
    i: -1, L: -99, R: -99, n, maxLen: 0, bestPal: ''
  });

  function simulateExpand(left, right, centerI, isEven) {
    const expLabel = `expandAroundCenter(s, left=${left}, right=${right})`;

    steps.push({
      badge: `${isEven ? 'Even' : 'Odd'} center expansion around left=${left}, right=${right}`,
      code: isEven ? 'c_expand_even' : 'c_expand_odd',
      vars: [
        frame('main()', []),
        frame(fnLabel, [['i', String(centerI)], ['left', String(left)], ['right', String(right)]]),
        frame(expLabel, [['left', String(left)], ['right', String(right)]])
      ],
      cells: getCells(centerI, left, right, true),
      i: centerI, L: left, R: right, n, maxLen, bestPal: getBestPal()
    });

    let L = left;
    let R = right;

    steps.push({
      badge: `int L = ${left}; int R = ${right};`,
      code: 'c_exp_init',
      vars: [
        frame('main()', []),
        frame(fnLabel, [['i', String(centerI)]]),
        frame(expLabel, [['L', String(L)], ['R', String(R)]])
      ],
      cells: getCells(centerI, L, R, true),
      i: centerI, L, R, n, maxLen, bestPal: getBestPal()
    });

    while (L >= 0 && R < n && str[L] === str[R]) {
      const matchChar = str[L];
      steps.push({
        badge: `while (L >= 0 && R < n && s[L] == s[R]) → L=${L} ('${str[L]}') == R=${R} ('${str[R]}') → MATCH!`,
        code: 'c_exp_while',
        vars: [
          frame('main()', []),
          frame(fnLabel, [['i', String(centerI)]]),
          frame(expLabel, [['L', String(L)], ['R', String(R)], ['matchChar', `'${matchChar}'`]])
        ],
        cells: getCells(centerI, L, R, true),
        i: centerI, L, R, n, maxLen, bestPal: getBestPal()
      });

      L--;
      steps.push({
        badge: `L-- → L updated to ${L}`,
        code: 'c_exp_l_dec',
        vars: [
          frame('main()', []),
          frame(fnLabel, [['i', String(centerI)]]),
          frame(expLabel, [['L', String(L)], ['R', String(R)]])
        ],
        cells: getCells(centerI, L + 1, R, true),
        i: centerI, L, R, n, maxLen, bestPal: getBestPal()
      });

      R++;
      steps.push({
        badge: `R++ → R updated to ${R}`,
        code: 'c_exp_r_inc',
        vars: [
          frame('main()', []),
          frame(fnLabel, [['i', String(centerI)]]),
          frame(expLabel, [['L', String(L)], ['R', String(R)]])
        ],
        cells: getCells(centerI, L + 1, R - 1, true),
        i: centerI, L, R, n, maxLen, bestPal: getBestPal()
      });
    }

    steps.push({
      badge: `while condition false (L=${L}, R=${R}) → stop expansion`,
      code: 'c_exp_while',
      vars: [
        frame('main()', []),
        frame(fnLabel, [['i', String(centerI)]]),
        frame(expLabel, [['L', String(L)], ['R', String(R)]])
      ],
      cells: getCells(centerI, L >= 0 ? L : 0, R < n ? R : n - 1, false),
      i: centerI, L, R, n, maxLen, bestPal: getBestPal()
    });

    const expLen = R - L - 1;
    steps.push({
      badge: `return R - L - 1 → (${R} - ${L} - 1) = ${expLen}`,
      code: 'c_exp_return',
      vars: [
        frame('main()', []),
        frame(fnLabel, [['i', String(centerI)]]),
        frame(expLabel, [['return', String(expLen)]])
      ],
      cells: getCells(centerI),
      i: centerI, L: -99, R: -99, n, maxLen, bestPal: getBestPal()
    });

    return expLen;
  }

  // 3. Outer loop i = 0 to n - 1
  for (let i = 0; i < n; i++) {
    steps.push({
      badge: `for (int i = ${i}; i < s.length() (${n}); i++) → evaluating center index i=${i} ('${str[i]}')`,
      code: 'c_loop',
      vars: [
        frame('main()', []),
        frame(fnLabel, [['i', String(i)], ['char', `'${str[i]}'`]])
      ],
      cells: getCells(i),
      i, L: -99, R: -99, n, maxLen, bestPal: getBestPal()
    });

    const len1 = simulateExpand(i, i, i, false);
    const len2 = simulateExpand(i, i + 1, i, true);

    const len = Math.max(len1, len2);
    steps.push({
      badge: `int len = Math.max(len1, len2) → max(${len1}, ${len2}) = ${len}`,
      code: 'c_len_max',
      vars: [
        frame('main()', []),
        frame(fnLabel, [['i', String(i)], ['len1', String(len1)], ['len2', String(len2)], ['len', String(len)]])
      ],
      cells: getCells(i),
      i, L: -99, R: -99, n, maxLen, bestPal: getBestPal()
    });

    const isNewMax = len > maxLen;
    steps.push({
      badge: `if (len > maxLen) → ${len} > ${maxLen} → ${isNewMax ? 'NEW LONGEST PALINDROME!' : 'NO UPDATE'}`,
      code: 'c_check_max',
      vars: [
        frame('main()', []),
        frame(fnLabel, [['i', String(i)], ['len', String(len)], ['maxLen', String(maxLen)], ['isNewMax', String(isNewMax)]])
      ],
      cells: getCells(i),
      i, L: -99, R: -99, n, maxLen, bestPal: getBestPal()
    });

    if (isNewMax) {
      maxLen = len;
      bestStart = i - Math.floor((len - 1) / 2);

      steps.push({
        badge: `maxLen = ${maxLen}, start = ${bestStart} → Updated longest palindrome to "${getBestPal()}"`,
        code: 'c_update_max',
        vars: [
          frame('main()', []),
          frame(fnLabel, [['i', String(i)], ['maxLen', String(maxLen)], ['start', String(bestStart)], ['bestPal', `"${getBestPal()}"`]])
        ],
        cells: getCells(i),
        i, L: -99, R: -99, n, maxLen, bestPal: getBestPal()
      });
    }
  }

  // 4. Return result
  const resultStr = getBestPal();
  steps.push({
    badge: `return s.substring(start, start + maxLen) → Longest Palindromic Substring: "${resultStr}" (length = ${maxLen})`,
    code: 'c_return',
    vars: [
      frame('main()', []),
      frame(fnLabel, [['return', `"${resultStr}"`], ['maxLen', String(maxLen)]])
    ],
    cells: getCells(),
    i: -1, L: -99, R: -99, n, maxLen, bestPal: resultStr
  });

  steps.push({
    badge: `Longest Palindromic Substring Complete! Result: "${resultStr}" (length = ${maxLen})`,
    code: 'c_done',
    vars: [
      frame('main()', []),
      frame(fnLabel, [['result_palindrome', `"${resultStr}"`], ['result_length', String(maxLen)]])
    ],
    cells: getCells(),
    i: -1, L: -99, R: -99, n, maxLen, bestPal: resultStr
  });

  return steps;
}

const inpStr = ref('babad');
const lang = ref('java');
const speed = ref(650);
const si = ref(0);
const playing = ref(false);
const vizHeight = ref(265);
const tableHeight = ref(60);
const leftWidth = ref(50);
const rightTab = ref('code');

const stepsData = reactive({ steps: buildSteps('babad') });
const steps = computed(() => stepsData.steps);
const s = computed(() => steps.value[Math.max(0, Math.min(si.value, steps.value.length - 1))] || {});
const codeLines = computed(() => CODES[lang.value] || []);

let playTimer = null;

function applyInput() {
  const textStr = String(inpStr.value || '').trim();
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
            <label>String Input</label>
            <input type="text" v-model="inpStr" style="width: 180px;" class="ll-text-input" placeholder="e.g. babad" />

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
                    <div class="ll-ptr-chip">maxLen = <b class="ll-c-green">{{ s.maxLen }}</b></div>
                    <div class="ll-ptr-chip">Longest Palindrome = <b class="ll-c-green">"{{ s.bestPal }}"</b></div>
                  </div>

                  <!-- Dynamic Array & Pointers Tracks -->
                  <div class="ll-lps-tracks">
                    <div class="ll-track-group">
                      <div class="ll-track-label">String characters with dynamic pointers i, L, R:</div>
                      <div class="ll-arr-track">

                        <!-- Left Out-Of-Bounds Slot for L < 0 -->
                        <div class="ll-arr-cell-wrap ll-oob-cell">
                          <div class="ll-ptr-tag-wrap">
                            <span v-if="s.L < 0 && s.L !== -99" class="ll-ptr-lbl ll-lbl-orange">L</span>
                          </div>
                          <div class="ll-arr-box ll-box-oob">&empty;</div>
                          <div class="ll-arr-idx">[-1]</div>
                        </div>

                        <!-- In-Bounds Character Array Slots -->
                        <div v-for="cell in s.cells" :key="'c-' + cell.idx" class="ll-arr-cell-wrap">
                          <div class="ll-ptr-tag-wrap">
                            <span v-if="cell.idx === s.i" class="ll-ptr-lbl ll-lbl-blue">i</span>
                            <span v-if="cell.idx === s.L" class="ll-ptr-lbl ll-lbl-orange">L</span>
                            <span v-if="cell.idx === s.R" class="ll-ptr-lbl ll-lbl-orange">R</span>
                          </div>
                          <div class="ll-arr-box" :class="{ 'll-box-center': cell.state === 'center', 'll-box-expanding': cell.state === 'expanding', 'll-box-best': cell.state === 'best_match' }">
                            {{ cell.val === ' ' ? '\u2423' : cell.val }}
                          </div>
                          <div class="ll-arr-idx">[{{ cell.idx }}]</div>
                        </div>

                        <!-- Right Out-Of-Bounds Slot for R >= n -->
                        <div class="ll-arr-cell-wrap ll-oob-cell">
                          <div class="ll-ptr-tag-wrap">
                            <span v-if="s.R >= s.n && s.n > 0" class="ll-ptr-lbl ll-lbl-orange">R</span>
                          </div>
                          <div class="ll-arr-box ll-box-oob">&empty;</div>
                          <div class="ll-arr-idx">[{{ s.n }}]</div>
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
                <span class="ll-leg"><span class="ll-legdot ll-legdot-center"></span>Center Pointer (i)</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-expanding"></span>Expansion Pointers (L, R)</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-best"></span>Longest Palindrome Match</span>
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
                      <tr><td>Best Case</td><td>O(N)</td><td>When all characters are distinct.</td></tr>
                      <tr><td>Average / Worst Case</td><td>O(N<sup>2</sup>)</td><td>Expanding around center takes O(N) time for each of the 2N-1 centers.</td></tr>
                      <tr><td>Space Complexity</td><td>O(1)</td><td>Constant extra space using two-pointer center expansion.</td></tr>
                    </tbody>
                  </table>
                  <p class="ll-note">
                    Key observation: <b>Expand Around Center</b> evaluates odd centers <code>expand(i, i)</code> and even centers <code>expand(i, i+1)</code> in <code>O(N^2)</code> time using <code>O(1)</code> space!
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
.ll-ptrs { display: flex; gap: 8px; flex-wrap: wrap; padding: 10px 16px 4px; min-height: 36px; width: 100%; box-sizing: border-box; min-width: 0; }
.ll-ptr-chip { background: var(--surface2); border: 1px solid var(--border); border-radius: var(--radius-sm); padding: 3px 10px; font-size: 12px; font-family: monospace; box-shadow: var(--shadow-sm); white-space: nowrap; flex-shrink: 0; }
.ll-c-blue { color: var(--blue); } .ll-c-orange { color: var(--orange); } .ll-c-green { color: var(--green); } .ll-c-purple { color: var(--purple); } .ll-c-red { color: var(--red); }

/* Dynamic Pointer Tag Styling */
.ll-lps-tracks { display: flex; flex-direction: column; gap: 4px; padding: 4px 16px; width: 100%; }
.ll-track-group { display: flex; flex-direction: column; gap: 2px; }
.ll-track-label { font-size: 11px; font-weight: 700; color: var(--text2); font-family: monospace; }
.ll-arr-track { display: flex; align-items: flex-start; gap: 6px; width: 100%; padding-top: 4px; }
.ll-arr-cell-wrap { display: flex; flex-direction: column; align-items: center; min-width: 0; }

.ll-ptr-tag-wrap { height: 28px; display: flex; align-items: flex-end; justify-content: center; gap: 4px; margin-bottom: 2px; }
.ll-ptr-lbl { font-size: 13px; font-weight: 800; font-family: 'Consolas', 'Fira Code', monospace; display: inline-flex; flex-direction: column; align-items: center; line-height: 1; gap: 1px; white-space: nowrap; animation: ll-pop 0.2s ease; }
.ll-ptr-lbl::after { content: '↓'; font-size: 11px; font-weight: 900; line-height: 1; margin-top: 1px; }
.ll-lbl-blue { color: #3b82f6; }
.ll-lbl-orange { color: #f97316; }
.ll-lbl-purple { color: #9333ea; }
.ll-lbl-green { color: #10b981; }

.ll-arr-box { width: 38px; height: 38px; display: flex; align-items: center; justify-content: center; border: 2px solid var(--blue); border-radius: var(--radius-sm); background: #eff6ff; color: #1e293b; font-weight: 700; font-size: 15px; font-family: monospace; box-shadow: var(--shadow-sm); transition: all 0.25s ease; animation: ll-pop .3s ease; }
.ll-box-center { border-color: #a855f7 !important; background: #faf5ff !important; color: #7e22ce !important; transform: translateY(-2px); }
.ll-box-expanding { border-color: #3b82f6 !important; background: #dbeafe !important; color: #1e40af !important; transform: translateY(-2px); }
.ll-box-best { border-color: #10b981 !important; background: #dcfce7 !important; color: #065f46 !important; transform: translateY(-2px); }
.ll-box-oob { border-style: dashed !important; border-color: #cbd5e1 !important; background: #f8fafc !important; color: #94a3b8 !important; opacity: 0.6; }

.ll-arr-idx { font-size: 10px; color: var(--muted); margin-top: 2px; font-family: 'Consolas', monospace; font-weight: 600; }
.ll-vresizer { height: 5px; cursor: row-resize; background: var(--border); flex-shrink: 0; transition: background .15s; position: relative; z-index: 20; }
.ll-vresizer:hover, .ll-vresizer.drag { background: var(--coral); }
.ll-legend { display: flex; flex-wrap: wrap; gap: 6px 14px; padding: 6px 12px; border-bottom: 1px solid var(--border); flex-shrink: 0; background: var(--surface2); }
.ll-leg { display: flex; align-items: center; gap: 5px; font-size: 11px; color: var(--text2); font-weight: 500; }
.ll-legdot { width: 11px; height: 11px; border-radius: 3px; flex-shrink: 0; display: inline-block; }
.ll-legdot-existing { background: #eff6ff; border: 1.5px solid var(--blue); }
.ll-legdot-center { background: #faf5ff; border: 1.5px solid #a855f7; }
.ll-legdot-expanding { background: #dbeafe; border: 1.5px solid #3b82f6; }
.ll-legdot-best { background: #dcfce7; border: 1.5px solid #10b981; }

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
