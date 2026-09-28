<script setup>
import { ref, reactive, computed, onMounted, onBeforeUnmount, watch, nextTick } from 'vue';

defineProps({
  topic:    { type: String, default: 'Array / String Algorithms' },
  subTopic: { type: String, default: 'Reverse String (LeetCode 344)' }
});

// ─── Multi-Language Code Definitions ──────────────────────────────────────────
const CODES = {
  java: [
    ['',             'import java.util.Scanner;'],
    ['',             ''],
    ['',             'class Solution {'],
    ['fn_entry',     '    public void reverseString(char[] s) {'],
    ['init_left',    '        int left = 0;'],
    ['init_right',   '        int right = s.length - 1;'],
    ['while_cond',   '        while (left < right) {'],
    ['swap_temp',    '            char temp = s[left];'],
    ['swap_assign1', '            s[left] = s[right];'],
    ['swap_assign2', '            s[right] = temp;'],
    ['inc_left',     '            left++;'],
    ['dec_right',    '            right--;'],
    ['',             '        }'],
    ['return_stmt',  '    }'],
    ['',             ''],
    ['',             '    public static void main(String[] args) {'],
    ['m_scanner',    '        Scanner sc = new Scanner(System.in);'],
    ['m_read_n',     '        int n = sc.nextInt();'],
    ['m_alloc',      '        char[] s = new char[n];'],
    ['m_loop',       '        for (int i = 0; i < n; i++) {'],
    ['m_read_c',     '            s[i] = sc.next().charAt(0);'],
    ['',             '        }'],
    ['m_call',       '        new Solution().reverseString(s);'],
    ['m_print',      '        System.out.println(new String(s));'],
    ['',             '    }'],
    ['',             '}']
  ],
  cpp: [
    ['',             '#include <vector>'],
    ['',             '#include <iostream>'],
    ['',             'using namespace std;'],
    ['',             ''],
    ['',             'class Solution {'],
    ['',             'public:'],
    ['fn_entry',     '    void reverseString(vector<char>& s) {'],
    ['init_left',    '        int left = 0;'],
    ['init_right',   '        int right = (int)s.size() - 1;'],
    ['while_cond',   '        while (left < right) {'],
    ['swap_temp',    '            char temp = s[left];'],
    ['swap_assign1', '            s[left] = s[right];'],
    ['swap_assign2', '            s[right] = temp;'],
    ['inc_left',     '            left++;'],
    ['dec_right',    '            right--;'],
    ['',             '        }'],
    ['return_stmt',  '    }'],
    ['',             '};'],
    ['',             ''],
    ['',             'int main() {'],
    ['m_read_n',     '    int n;'],
    ['m_scanner',    '    cin >> n;'],
    ['m_alloc',      '    vector<char> s(n);'],
    ['m_loop',       '    for (int i = 0; i < n; i++) {'],
    ['m_read_c',     '        cin >> s[i];'],
    ['',             '    }'],
    ['m_call',       '    Solution().reverseString(s);'],
    ['m_print',      '    for (char c : s) { cout << c; }'],
    ['',             '    cout << endl;'],
    ['',             '    return 0;'],
    ['',             '}']
  ],
  python: [
    ['',             'class Solution:'],
    ['fn_entry',     '    def reverseString(self, s):'],
    ['init_left',    '        left = 0'],
    ['init_right',   '        right = len(s) - 1'],
    ['while_cond',   '        while left < right:'],
    ['swap_temp',    '            temp = s[left]'],
    ['swap_assign1', '            s[left] = s[right]'],
    ['swap_assign2', '            s[right] = temp'],
    ['inc_left',     '            left += 1'],
    ['dec_right',    '            right -= 1'],
    ['return_stmt',  '        # reversed in-place'],
    ['',             ''],
    ['',             'def main():'],
    ['m_read_c',     '    s = list(input())'],
    ['m_call',       '    Solution().reverseString(s)'],
    ['m_print',      '    print("".join(s))'],
    ['',             ''],
    ['',             'if __name__ == "__main__":'],
    ['',             '    main()']
  ],
  javascript: [
    ['',             '/**'],
    ['',             ' * @param {character[]} s'],
    ['',             ' * @return {void}'],
    ['',             ' */'],
    ['fn_entry',     'var reverseString = function(s) {'],
    ['init_left',    '    let left = 0;'],
    ['init_right',   '    let right = s.length - 1;'],
    ['while_cond',   '    while (left < right) {'],
    ['swap_temp',    '        let temp = s[left];'],
    ['swap_assign1', '        s[left] = s[right];'],
    ['swap_assign2', '        s[right] = temp;'],
    ['inc_left',     '        left++;'],
    ['dec_right',    '        right--;'],
    ['',             '    }'],
    ['return_stmt',  '};'],
    ['',             ''],
    ['',             'function main() {'],
    ['m_read_c',     '    const s = lines[0].split(\'\');'],
    ['m_call',       '    reverseString(s);'],
    ['m_print',      '    console.log(s.join(\'\'));'],
    ['',             '}'],
    ['',             'main();']
  ],
  c: [
    ['',             '#include <stdio.h>'],
    ['',             '#include <string.h>'],
    ['',             ''],
    ['fn_entry',     'void reverseString(char* s, int n) {'],
    ['init_left',    '    int left = 0;'],
    ['init_right',   '    int right = n - 1;'],
    ['while_cond',   '    while (left < right) {'],
    ['swap_temp',    '        char temp = s[left];'],
    ['swap_assign1', '        s[left] = s[right];'],
    ['swap_assign2', '        s[right] = temp;'],
    ['inc_left',     '        left++;'],
    ['dec_right',    '        right--;'],
    ['',             '    }'],
    ['return_stmt',  '}'],
    ['',             ''],
    ['',             'int main() {'],
    ['m_read_n',     '    int n;'],
    ['m_scanner',    '    scanf("%d", &n);'],
    ['m_alloc',      '    char s[200];'],
    ['m_loop',       '    for (int i = 0; i < n; i++) {'],
    ['m_read_c',     '        scanf(" %c", &s[i]);'],
    ['',             '    }'],
    ['',             '    s[n] = \'\\0\';'],
    ['m_call',       '    reverseString(s, n);'],
    ['m_print',      '    printf("%s\\n", s);'],
    ['',             '    return 0;'],
    ['',             '}']
  ]
};

const PSEUDOCODE = [
  'function reverseString(s):',
  '    left  = 0',
  '    right = len(s) - 1',
  '',
  '    while left < right:',
  '        // Three-step swap',
  '        temp    = s[left]    // save left',
  '        s[left] = s[right]  // overwrite left with right',
  '        s[right]= temp       // write saved into right',
  '',
  '        left  += 1',
  '        right -= 1',
  '',
  '    // Array reversed in-place'
];

// ─── Step Builder ──────────────────────────────────────────────────────────────
function buildSteps(rawStr) {
  const steps = [];
  const initArr = rawStr.slice(0, 15).split('');
  const n = initArr.length;

  const NONE = { leftReady: false, rightReady: false };
  const ALL  = { leftReady: true,  rightReady: true  };

  function snap(left, right, arrState, tempVal, code, phase, initState, extra) {
    return {
      left, right,
      arr: [...arrState],
      tempVal,
      n,
      code, phase,
      initState: { ...initState },
      ...extra
    };
  }

  const arr = [...initArr];

  // ── Input phase ─────────────────────────────────────────────────────────────
  const partialArr = new Array(n).fill('');
  steps.push({
    ...snap(-1, -1, arr, null, 'm_scanner', 'input', NONE, { partial: [...partialArr] }),
    badge: `Scanner sc = new Scanner(System.in); → Initializing input stream.`
  });
  steps.push({
    ...snap(-1, -1, arr, null, 'm_read_n', 'input', NONE, { partial: [...partialArr] }),
    badge: `int n = sc.nextInt(); → Read n = ${n}. Array has ${n} characters.`
  });
  steps.push({
    ...snap(-1, -1, arr, null, 'm_alloc', 'input', NONE, { partial: [...partialArr] }),
    badge: `char[] s = new char[n]; → Allocated char array of size ${n}.`
  });

  for (let i = 0; i < n; i++) {
    steps.push({
      ...snap(-1, -1, arr, null, 'm_loop', 'input', NONE, { partial: [...partialArr], readIdx: i }),
      badge: `for (int i = ${i}; i < ${n}; i++) → i = ${i}. Will read s[${i}].`
    });
    partialArr[i] = arr[i];
    steps.push({
      ...snap(-1, -1, arr, null, 'm_read_c', 'input', NONE, { partial: [...partialArr], readIdx: i }),
      badge: `s[i] = sc.next().charAt(0); → s[${i}] = '${arr[i]}'. Character stored.`
    });
  }
  steps.push({
    ...snap(-1, -1, arr, null, 'm_loop', 'input', NONE, { partial: [...partialArr] }),
    badge: `for (int i = ${n}; i < ${n}; i++) → i = ${n} >= n. Loop exits.`
  });
  steps.push({
    ...snap(-1, -1, arr, null, 'm_call', 'input', NONE, { partial: [...arr] }),
    badge: `new Solution().reverseString(s); → All characters read. Calling reverseString().`
  });

  // ── reverseString() ─────────────────────────────────────────────────────────
  let left  = 0;
  let right = n - 1;
  const currArr = [...arr];

  steps.push({
    ...snap(-1, -1, currArr, null, 'fn_entry', 'init', NONE, {}),
    badge: `public void reverseString(char[] s) → Function entry. Will reverse in-place using two-pointer swap.`
  });
  steps.push({
    ...snap(0, -1, currArr, null, 'init_left', 'init', { leftReady: true, rightReady: false }, {}),
    badge: `int left = 0; → left = 0. Points to '${currArr[0]}'.`
  });
  steps.push({
    ...snap(0, right, currArr, null, 'init_right', 'init', ALL, {}),
    badge: `int right = s.length - 1; → right = ${right}. Points to '${currArr[right]}'.`
  });

  while (left < right) {
    steps.push({
      ...snap(left, right, currArr, null, 'while_cond', 'solve', ALL, {}),
      badge: `while (left < right): left=${left} < right=${right} → TRUE. Swap s[${left}]='${currArr[left]}' with s[${right}]='${currArr[right]}'.`
    });

    const tempVal = currArr[left];
    steps.push({
      ...snap(left, right, currArr, tempVal, 'swap_temp', 'solve', ALL, { swapL: left, swapR: right }),
      badge: `char temp = s[left]; → temp = '${tempVal}'. Saved s[${left}]='${tempVal}' into temp.`
    });

    currArr[left] = currArr[right];
    steps.push({
      ...snap(left, right, [...currArr], tempVal, 'swap_assign1', 'solve', ALL, { swapL: left, swapR: right }),
      badge: `s[left] = s[right]; → s[${left}] = '${currArr[left]}'. Left position now holds right's value.`
    });

    currArr[right] = tempVal;
    steps.push({
      ...snap(left, right, [...currArr], null, 'swap_assign2', 'solve', ALL, { swapL: left, swapR: right }),
      badge: `s[right] = temp; → s[${right}] = '${tempVal}'. Swap complete: s[${left}]='${currArr[left]}', s[${right}]='${currArr[right]}'.`
    });

    left++;
    steps.push({
      ...snap(left, right, [...currArr], null, 'inc_left', 'solve', ALL, {}),
      badge: `left++; → left = ${left}.`
    });

    right--;
    steps.push({
      ...snap(left, right, [...currArr], null, 'dec_right', 'solve', ALL, {}),
      badge: `right--; → right = ${right}.`
    });
  }

  steps.push({
    ...snap(left, right, [...currArr], null, 'while_cond', 'done', ALL, {}),
    badge: `while (left < right): left=${left}, right=${right} → FALSE. Pointers met. Array fully reversed!`
  });
  steps.push({
    ...snap(left, right, [...currArr], null, 'return_stmt', 'done', ALL, { finalArr: [...currArr] }),
    badge: `} → Function returns. Result: "${currArr.join('')}". Reversed in O(n) time, O(1) space.`
  });

  return steps;
}

// ─── Reactive State ───────────────────────────────────────────────────────────
const inputS      = ref('hello');
const lang        = ref('java');
const speed       = ref(650);
const si          = ref(0);
const playing     = ref(false);
const vizHeight   = ref(350);
const tableHeight = ref(60);
const leftWidth   = ref(52);
const rightTab    = ref('code');

const stepsData = reactive({ steps: buildSteps('hello') });
const steps     = computed(() => stepsData.steps);
const s         = computed(() => steps.value[Math.max(0, Math.min(si.value, steps.value.length - 1))] || {});
const codeLines = computed(() => CODES[lang.value] || []);

let playTimer = null;

function onChipsWheel(e) {
  if (e.currentTarget) {
    e.currentTarget.scrollLeft += e.deltaY;
  }
}

function applyInput() {
  const raw = inputS.value.trim().replace(/\s+/g, '').slice(0, 15);
  if (raw.length < 2) {
    alert('Please enter at least 2 characters.');
    return;
  }
  playing.value = false;
  stepsData.steps = buildSteps(raw);
  si.value = 0;
}

function stepBy(d) {
  si.value = Math.max(0, Math.min(steps.value.length - 1, si.value + d));
}

function togglePlay() {
  if (!playing.value && si.value >= steps.value.length - 1) {
    si.value = 0;
  }
  playing.value = !playing.value;
}

function tick() {
  clearTimeout(playTimer);
  if (!playing.value) {
    return;
  }
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
  if (v) {
    tick();
  } else {
    clearTimeout(playTimer);
  }
});

const codeScrollRef = ref(null);

function scrollActiveCodeLine() {
  nextTick(() => {
    const container = codeScrollRef.value;
    if (!container) {
      return;
    }
    const activeEl = container.querySelector('.ll-hl');
    if (!activeEl) {
      return;
    }
    const contRect   = container.getBoundingClientRect();
    const activeRect = activeEl.getBoundingClientRect();
    if (activeRect.top < contRect.top) {
      container.scrollTop = Math.max(0, container.scrollTop - (contRect.top - activeRect.top) - 24);
    } else if (activeRect.bottom > contRect.bottom) {
      container.scrollTop = container.scrollTop + (activeRect.bottom - contRect.bottom) + 24;
    }
  });
}

watch(() => s.value.code, scrollActiveCodeLine);
watch(lang, scrollActiveCodeLine);
watch(rightTab, v => {
  if (v === 'code') {
    scrollActiveCodeLine();
  }
});

function onKeydown(e) {
  const tag = e.target.tagName;
  if (tag === 'INPUT' || tag === 'SELECT' || tag === 'TEXTAREA') {
    return;
  }
  if (e.key === 'ArrowRight') {
    stepBy(1);
  }
  if (e.key === 'ArrowLeft') {
    stepBy(-1);
  }
  if (e.key === ' ') {
    e.preventDefault();
    togglePlay();
  }
}

// ─── Computed display helpers ──────────────────────────────────────────────────
const initState    = computed(() => s.value.initState || { leftReady: false, rightReady: false });
const displayLeft  = computed(() => initState.value.leftReady  ? s.value.left  : '?');
const displayRight = computed(() => initState.value.rightReady ? s.value.right : '?');
const displayTemp  = computed(() => s.value.tempVal !== null && s.value.tempVal !== undefined ? `'${s.value.tempVal}'` : '—');
const displayArr   = computed(() => {
  if (s.value.phase === 'input') {
    return s.value.partial || [];
  }
  return s.value.arr || [];
});
const displayN     = computed(() => s.value.n || 0);
const displayCode  = computed(() => s.value.code || '');
const isSwapPhase  = computed(() => ['swap_temp', 'swap_assign1', 'swap_assign2'].includes(displayCode.value));

function charClass(idx) {
  const phase = s.value.phase;
  const ist   = initState.value;
  if (phase === 'input') {
    const ri = s.value.readIdx;
    return ri !== undefined && idx === ri ? 'rs-char-reading' : '';
  }
  if (!ist.leftReady) {
    return '';
  }
  const left    = s.value.left;
  const right   = s.value.right;
  const code    = s.value.code;
  const swapL   = s.value.swapL;
  const swapR   = s.value.swapR;

  if (phase === 'done') {
    return 'rs-char-done';
  }

  if (!ist.rightReady) {
    return '';
  }

  if (idx < left || idx > right) {
    return 'rs-char-processed';
  }

  // Swap step highlighting
  if (swapL !== undefined && swapR !== undefined) {
    if (code === 'swap_temp') {
      if (idx === swapL) {
        return 'rs-char-reading';
      }
      if (idx === swapR) {
        return 'rs-char-right';
      }
    }
    if (code === 'swap_assign1') {
      if (idx === swapL) {
        return 'rs-char-swapped-new-l';
      }
      if (idx === swapR) {
        return 'rs-char-right';
      }
    }
    if (code === 'swap_assign2') {
      if (idx === swapL) {
        return 'rs-char-swapped-done-l';
      }
      if (idx === swapR) {
        return 'rs-char-swapped-done-r';
      }
    }
  }

  if (idx === left) {
    return 'rs-char-left';
  }
  if (idx === right) {
    return 'rs-char-right';
  }
  return 'rs-char-inner';
}

function showLeft(idx) {
  const phase = s.value.phase;
  const ist   = initState.value;
  return (phase === 'solve' || phase === 'init') && ist.leftReady && ist.rightReady && idx === s.value.left;
}

function showRight(idx) {
  const phase = s.value.phase;
  const ist   = initState.value;
  return (phase === 'solve' || phase === 'init') && ist.rightReady && idx === s.value.right && idx !== s.value.left;
}

// ─── Resizers ──────────────────────────────────────────────────────────────────
const mainRef         = ref(null);
const leftColRef      = ref(null);
const hResizerRef     = ref(null);
const vizResizerRef   = ref(null);
const tableResizerRef = ref(null);

function initHResizer() {
  const rsz  = hResizerRef.value;
  const main = mainRef.value;
  if (!rsz || !main) {
    return;
  }
  let dragging = false;
  let startX   = 0;
  let startW   = 0;
  const onDown = e => {
    dragging = true;
    startX   = e.clientX;
    startW   = leftColRef.value.offsetWidth;
    rsz.classList.add('drag');
    document.body.style.userSelect = 'none';
  };
  const onMove = e => {
    if (!dragging) {
      return;
    }
    const mainW = main.offsetWidth;
    leftWidth.value = (Math.max(200, Math.min(mainW - 200, startW + e.clientX - startX)) / mainW) * 100;
  };
  const onUp = () => {
    if (!dragging) {
      return;
    }
    dragging = false;
    rsz.classList.remove('drag');
    document.body.style.userSelect = '';
  };
  rsz.addEventListener('mousedown', onDown);
  document.addEventListener('mousemove', onMove);
  document.addEventListener('mouseup', onUp);
  return () => {
    rsz.removeEventListener('mousedown', onDown);
    document.removeEventListener('mousemove', onMove);
    document.removeEventListener('mouseup', onUp);
  };
}

function initVResizer(elRef, valueRef, minH, maxH) {
  const rsz = elRef.value;
  if (!rsz) {
    return;
  }
  let dragging = false;
  let startY   = 0;
  let startH   = 0;
  const onDown = e => {
    dragging = true;
    startY   = e.clientY;
    startH   = valueRef.value;
    rsz.classList.add('drag');
    document.body.style.userSelect = 'none';
    e.preventDefault();
  };
  const onMove = e => {
    if (!dragging) {
      return;
    }
    valueRef.value = Math.max(minH, Math.min(maxH, startH + (e.clientY - startY)));
  };
  const onUp = () => {
    if (!dragging) {
      return;
    }
    dragging = false;
    rsz.classList.remove('drag');
    document.body.style.userSelect = '';
  };
  rsz.addEventListener('mousedown', onDown);
  document.addEventListener('mousemove', onMove);
  document.addEventListener('mouseup', onUp);
  return () => {
    rsz.removeEventListener('mousedown', onDown);
    document.removeEventListener('mousemove', onMove);
    document.removeEventListener('mouseup', onUp);
  };
}

let cleanupFns = [];
onMounted(() => {
  document.addEventListener('keydown', onKeydown);
  cleanupFns.push(initHResizer());
  cleanupFns.push(initVResizer(vizResizerRef,   vizHeight,   220, 700));
  cleanupFns.push(initVResizer(tableResizerRef, tableHeight,  50, 200));
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
      <h2 class="navbar-title">{{ topic }} &mdash; {{ subTopic }}</h2>
      <img src="../../assets/logo.png" alt="Logo" />
    </div>

    <div class="slide-body">
      <div class="row-main">
        <div class="ll-root">

          <!-- Toolbar -->
          <div class="ll-toolbar">
            <div class="ll-input-group">
              <label>s (char array):</label>
              <input
                type="text"
                v-model="inputS"
                class="ll-text-input rs-str-input"
                @keyup.enter="applyInput"
                placeholder="e.g. hello"
                maxlength="15"
              />
            </div>
            <button class="ll-viz-btn" @click="applyInput">&#9654; Visualize</button>
            <div class="ll-nav-controls">
              <button class="ll-nav-btn" title="First step"    @click="stepBy(-steps.length)">&#171;</button>
              <button class="ll-nav-btn" title="Previous step" @click="stepBy(-1)">&#8249; Prev</button>
              <button class="ll-play-btn"                      @click="togglePlay">{{ playing ? '\u23F8 Pause' : '\u25B6 Play' }}</button>
              <button class="ll-nav-btn" title="Next step"     @click="stepBy(1)">Next &#8250;</button>
              <button class="ll-nav-btn" title="Last step"     @click="stepBy(steps.length)">&#187;</button>
            </div>
          </div>

          <div class="ll-main" ref="mainRef">
            <!-- Left Visualization Column -->
            <div class="ll-left-col" ref="leftColRef" :style="{ width: leftWidth + '%' }">
              <div class="ll-viz-wrap" :style="{ height: vizHeight + 'px' }">
                <div class="ll-perm-area">

                  <!-- Stats Chips -->
                  <div class="ll-ptrs ll-ptrs-compact" @wheel.passive="onChipsWheel">
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">n</span><b class="ll-c-blue">{{ displayN }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">left</span><b class="ll-c-blue">{{ displayLeft }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">right</span><b class="ll-c-green">{{ displayRight }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">temp</span><b class="ll-c-orange">{{ displayTemp }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">s[]</span><b class="ll-c-purple" style="font-family:monospace;">"{{ displayArr.join('') }}"</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">phase</span><b class="ll-c-orange">{{ s.phase || 'input' }}</b></span>
                  </div>

                  <!-- BOARD CONTAINER -->
                  <div class="ll-board-container">

                    <!-- Tier 1 — Array Visualization -->
                    <div class="rs-tier-title">
                      Tier 1 &mdash; char[] s
                      <span class="rs-len-badge">n = {{ displayN }}</span>
                    </div>

                    <div class="rs-array-frame">
                      <!-- Pointer label row (above boxes) -->
                      <div class="rs-ptr-row">
                        <div
                          v-for="(ch, idx) in displayArr"
                          :key="'ptr-' + idx"
                          class="rs-ptr-cell"
                        >
                          <span v-if="showLeft(idx)"  class="rs-ptr-label rs-ptr-left">left</span>
                          <span v-if="showRight(idx)" class="rs-ptr-label rs-ptr-right">right</span>
                        </div>
                      </div>

                      <!-- Character boxes -->
                      <div class="rs-char-row">
                        <div
                          v-for="(ch, idx) in displayArr"
                          :key="'ch-' + idx"
                          class="rs-char-box"
                          :class="charClass(idx)"
                        >
                          <span class="rs-char-val">{{ ch || '\u00A0' }}</span>
                          <span class="rs-char-idx">[{{ idx }}]</span>
                        </div>
                      </div>

                      <!-- Swap connector bar (visible during swap steps) -->
                      <div class="rs-swap-bar" v-if="isSwapPhase && s.swapL !== undefined">
                        <div class="rs-swap-connector">
                          <div class="rs-swap-left-marker">s[{{ s.swapL }}]</div>
                          <div class="rs-swap-line">
                            <span class="rs-swap-arrow-l">&#8592;</span>
                            <span class="rs-swap-label">
                              <template v-if="displayCode === 'swap_temp'">save temp = '{{ s.tempVal }}'</template>
                              <template v-else-if="displayCode === 'swap_assign1'">s[left] = s[right]</template>
                              <template v-else-if="displayCode === 'swap_assign2'">s[right] = temp</template>
                            </span>
                            <span class="rs-swap-arrow-r">&#8594;</span>
                          </div>
                          <div class="rs-swap-right-marker">s[{{ s.swapR }}]</div>
                        </div>
                      </div>
                    </div>

                    <!-- Tier 2 — Variable State Panel -->
                    <div class="rs-tier-title">Tier 2 &mdash; Variable State</div>
                    <div class="rs-var-panel">
                      <div class="rs-var-card" :class="{ 'rs-var-active-l': initState.leftReady }">
                        <span class="rs-var-name rs-name-left">left</span>
                        <span class="rs-var-val">{{ displayLeft }}</span>
                        <span class="rs-var-desc">left pointer</span>
                      </div>
                      <div class="rs-var-card" :class="{ 'rs-var-active-r': initState.rightReady }">
                        <span class="rs-var-name rs-name-right">right</span>
                        <span class="rs-var-val">{{ displayRight }}</span>
                        <span class="rs-var-desc">right pointer</span>
                      </div>
                      <div class="rs-var-card" :class="{ 'rs-var-active-t': s.tempVal !== null && s.tempVal !== undefined }">
                        <span class="rs-var-name rs-name-temp">temp</span>
                        <span class="rs-var-val rs-temp-val">{{ displayTemp }}</span>
                        <span class="rs-var-desc">swap buffer</span>
                      </div>
                      <div class="rs-var-card" :class="{ 'rs-var-done': s.phase === 'done' }">
                        <span class="rs-var-name rs-name-result">s[]</span>
                        <span class="rs-var-val" style="font-size:13px;font-family:monospace;font-weight:800;">"{{ displayArr.join('') }}"</span>
                        <span class="rs-var-desc">current array</span>
                      </div>
                    </div>

                    <!-- Swap Progress indicator -->
                    <div class="rs-progress-row" v-if="s.phase === 'solve' || s.phase === 'done'">
                      <div
                        v-for="idx in displayN"
                        :key="'prog-' + idx"
                        class="rs-progress-cell"
                        :class="{
                          'rs-prog-done':    (idx - 1) < s.left || (idx - 1) > s.right,
                          'rs-prog-active-l': (idx - 1) === s.left,
                          'rs-prog-active-r': (idx - 1) === s.right,
                          'rs-prog-inner':   (idx - 1) > s.left && (idx - 1) < s.right
                        }"
                      ></div>
                    </div>
                    <div class="rs-progress-label" v-if="s.phase === 'solve' || s.phase === 'done'">
                      Progress: {{ Math.round(((s.left || 0) / Math.ceil(displayN / 2)) * 100) }}% swapped
                    </div>

                  </div>
                  <!-- END ll-board-container -->
                </div>
              </div>

              <!-- Vertical Resizer -->
              <div class="ll-vresizer" ref="vizResizerRef"></div>

              <!-- Color Legend -->
              <div class="ll-legend">
                <span class="ll-leg"><span class="ll-legdot" style="background:#bfdbfe;border:1.5px solid #3b82f6;"></span>left</span>
                <span class="ll-leg"><span class="ll-legdot" style="background:#bbf7d0;border:1.5px solid #22c55e;"></span>right</span>
                <span class="ll-leg"><span class="ll-legdot" style="background:#fef9c3;border:1.5px solid #eab308;"></span>reading (temp)</span>
                <span class="ll-leg"><span class="ll-legdot rs-legdot-swapped"></span>swapped</span>
                <span class="ll-leg"><span class="ll-legdot" style="background:#f1f5f9;border:1.5px solid #cbd5e1;opacity:.6"></span>processed</span>
                <span class="ll-leg"><span class="ll-legdot" style="background:#dcfce7;border:1.5px solid #86efac;"></span>done</span>
              </div>

              <!-- Call Stack Area -->
              <div class="ll-table-area" :style="{ height: tableHeight + 'px' }">
                <div class="ll-table-title">Call Stack &mdash; Current Execution Frame</div>
                <div class="ll-stack-line">
                  <template v-if="s.phase === 'input'">
                    <div class="ll-frame ll-frame-cur">
                      main()
                      &nbsp;
                      <span class="ll-fname">n</span>=<span class="ll-c-blue" style="font-weight:700">{{ displayN }}</span>
                      <span class="ll-now"> &#9668; active</span>
                    </div>
                  </template>
                  <template v-else>
                    <div class="ll-frame" style="color:var(--text2);">main()</div>
                    <div class="ll-frame ll-frame-cur" style="margin-left:14px">
                      reverseString(s)
                      &nbsp;
                      <span class="ll-fname">left</span>=<span class="ll-c-blue" style="font-weight:700">{{ displayLeft }}</span>,
                      <span class="ll-fname">right</span>=<span class="ll-c-green" style="font-weight:700">{{ displayRight }}</span>
                      <template v-if="s.tempVal !== null && s.tempVal !== undefined">
                        , <span class="ll-fname">temp</span>=<span class="ll-c-orange" style="font-weight:700">'{{ s.tempVal }}'</span>
                      </template>
                      <span class="ll-now"> &#9668; active</span>
                    </div>
                  </template>
                </div>
              </div>

              <!-- Vertical Resizer -->
              <div class="ll-vresizer" ref="tableResizerRef"></div>

              <!-- Step Badge -->
              <div class="ll-badge-wrap">
                <div class="ll-badge"
                  :class="{
                    'll-badge-error':   s.badge && s.badge.includes('FALSE'),
                    'll-badge-success': s.badge && (s.badge.includes('TRUE') || s.badge.includes('reversed') || s.badge.includes('complete') || s.badge.includes('Swap complete'))
                  }"
                >
                  {{ s.badge || 'Ready to visualize Reverse String.' }}
                </div>
              </div>
            </div>

            <!-- Horizontal Resizer -->
            <div class="ll-resizer" ref="hResizerRef"></div>

            <!-- Right Column: Code & Theory -->
            <div class="ll-right-col">
              <div class="ll-code-panel">
                <div class="ll-code-header">
                  <div class="ll-tabbar">
                    <button class="ll-tab-btn" :class="{ active: rightTab === 'code' }"       @click="rightTab = 'code'">Code</button>
                    <button class="ll-tab-btn" :class="{ active: rightTab === 'pseudo' }"     @click="rightTab = 'pseudo'">Pseudocode</button>
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

                <div v-if="rightTab === 'code'" class="ll-code-scroll" ref="codeScrollRef">
                  <pre class="ll-pre"><span
                    v-for="(line, idx) in codeLines"
                    :key="idx"
                    class="ll-codeline"
                    :class="{ 'll-hl': line[0] && line[0] === s.code }"
                  >{{ line[1] === '' ? ' ' : line[1] }}</span></pre>
                </div>

                <div v-else-if="rightTab === 'pseudo'" class="ll-code-scroll">
                  <pre class="ll-pre"><span
                    v-for="(line, idx) in PSEUDOCODE"
                    :key="idx"
                    class="ll-codeline"
                  >{{ line }}</span></pre>
                </div>

                <div v-else class="ll-info-scroll">
                  <h3 class="ll-cx-heading">Reverse String (LeetCode 344) &mdash; Complexity Analysis</h3>
                  <p class="ll-cx-intro">
                    Reverse a character array <code>s</code> in-place with <strong>O(1)</strong> extra memory.
                    Two pointers <code>left</code> and <code>right</code> start at opposite ends and swap characters,
                    moving inward until they meet. Each swap uses a single <code>temp</code> variable — no extra array needed.
                  </p>

                  <h4 class="ll-cx-sub">Step-by-step Breakdown</h4>
                  <table class="ll-complexity-table">
                    <thead>
                      <tr><th>Operation</th><th>Time</th><th>Space</th><th>Reason</th></tr>
                    </thead>
                    <tbody>
                      <tr><td>Init left, right</td><td class="ll-cx-good">O(1)</td><td class="ll-cx-good">O(1)</td><td>Two assignments</td></tr>
                      <tr><td>while loop</td><td class="ll-cx-good">O(n/2)</td><td class="ll-cx-good">O(1)</td><td>Runs n/2 times (pointers meet in middle)</td></tr>
                      <tr><td>Per-iteration swap</td><td class="ll-cx-good">O(1)</td><td class="ll-cx-good">O(1)</td><td>3 assignments using one temp variable</td></tr>
                      <tr><td><strong>Total</strong></td><td class="ll-cx-good"><strong>O(n)</strong></td><td class="ll-cx-good"><strong>O(1)</strong></td><td>Linear time, constant space</td></tr>
                    </tbody>
                  </table>

                  <h4 class="ll-cx-sub">Overall Complexity</h4>
                  <div class="ll-cx-summary-grid">
                    <div class="ll-cx-card ll-cx-card-good">
                      <div class="ll-cx-card-label">Time</div>
                      <div class="ll-cx-card-val">O(n)</div>
                      <div class="ll-cx-card-note">n/2 swaps total</div>
                    </div>
                    <div class="ll-cx-card ll-cx-card-good">
                      <div class="ll-cx-card-label">Space</div>
                      <div class="ll-cx-card-val">O(1)</div>
                      <div class="ll-cx-card-note">Single temp variable only</div>
                    </div>
                    <div class="ll-cx-card ll-cx-card-good">
                      <div class="ll-cx-card-label">Swaps</div>
                      <div class="ll-cx-card-val">n/2</div>
                      <div class="ll-cx-card-note">Each position touched once</div>
                    </div>
                  </div>

                  <div class="ll-note">
                    <strong>Three-step swap:</strong> Using a temp variable to swap two array elements is the classic safe approach.
                    Some languages support simultaneous swap (<code>a, b = b, a</code> in Python), but in C/Java/C++
                    the explicit three-step swap with <code>temp</code> is required. The two-pointer technique ensures
                    we only perform exactly <code>n/2</code> swaps (or <code>(n-1)/2</code> for odd-length arrays, where
                    the middle element stays in place).
                  </div>
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
.ll-root *, .ll-root, .row-main { scrollbar-width: none !important; -ms-overflow-style: none !important; }
.ll-root *::-webkit-scrollbar, .ll-root::-webkit-scrollbar, .row-main::-webkit-scrollbar { display: none !important; width: 0 !important; height: 0 !important; }

.ll-root {
  --coral: #F04D4D; --coral-dark: #d93e3e; --coral-light: #fff0f0;
  --bg: #f5f6fa; --surface: #ffffff; --surface2: #f1f4f9;
  --border: #e2e8f0; --border2: #cbd5e1;
  --text: #1e293b; --text2: #475569; --muted: #94a3b8;
  --blue: #3b82f6; --blue-light: #eff6ff;
  --green: #22c55e; --green-light: #f0fdf4;
  --orange: #f97316; --orange-light: #fff7ed;
  --purple: #9333ea; --purple-light: #f3e8ff;
  --red: #ef4444; --red-dark: #991b1b; --red-light: #fef2f2;
  --shadow-sm: 0 1px 3px rgba(0,0,0,.08), 0 1px 2px rgba(0,0,0,.04);
  --radius: 8px; --radius-sm: 6px;
  background: var(--bg); color: var(--text);
  font-family: 'Segoe UI', system-ui, sans-serif; font-size: 12.5px;
  display: flex; flex-direction: column; height: 74vh; overflow: hidden; width: 100%;
}

@keyframes rs-flash-read  { 0%{background:#fef9c3;transform:scale(1.1);} 60%{background:#fef3c7;} 100%{transform:scale(1);} }
@keyframes rs-flash-swap  { 0%{background:#fef9c3;transform:scale(1.12);} 60%{background:#a7f3d0;} 100%{transform:scale(1);} }
@keyframes rs-flash-done  { 0%{background:#dcfce7;transform:scale(1.08);} 100%{transform:scale(1);} }

.slide-wrapper { margin-top: -10px; margin-left: -30px; width: 107%; max-height: 100%; font-size: .8rem; font-weight: 400; }
.slide-body    { display: flex; flex-direction: column; border-radius: 4px; height: 100%; }
.navbar { display: flex; flex-direction: row; justify-content: space-between; align-items: center; gap: .75rem; padding: 0 10px; background-color: #fff; position: fixed; width: 94.7%; z-index: 50; }
.navbar > img  { height: 30px; }
.navbar-title  { margin: 0; font-size: 1.35rem; font-weight: 700; background-color: #ef5050; color: #fff; width: 80%; padding: 2px 10px; margin-left: -10px; border-radius: 5px; }
.row-main      { width: 100%; height: 90%; margin-top: 36px; overflow-x: auto; overflow-y: hidden; }

/* Toolbar */
.ll-toolbar      { margin-top: 4px; display: flex; align-items: center; gap: 6px; padding: 6.5px 12px; background: var(--surface); border-bottom: 1px solid var(--border); flex-shrink: 0; flex-wrap: wrap; box-shadow: var(--shadow-sm); }
.ll-input-group  { display: flex; align-items: center; gap: 4px; }
.ll-input-group label { font-size: 11px; color: var(--muted); font-weight: 700; }
.ll-text-input   { background: var(--surface); border: 1px solid var(--border2); color: var(--text); border-radius: var(--radius-sm); padding: 3px 6px; font-size: 11.5px; font-family: monospace; }
.ll-text-input:focus { outline: none; border-color: var(--coral); box-shadow: 0 0 0 3px rgba(240,77,77,.1); }
.rs-str-input    { width: 160px; }
.ll-viz-btn      { background: var(--coral); color: #fff; border: none; padding: 5px 12px; border-radius: var(--radius-sm); cursor: pointer; font-size: 11.5px; font-weight: 600; box-shadow: var(--shadow-sm); transition: filter .15s; }
.ll-viz-btn:hover { filter: brightness(1.08); }
.ll-nav-controls { display: flex; margin-left: auto; align-items: center; gap: 4px; flex-shrink: 0; flex-wrap: wrap; }
.ll-nav-btn  { background: var(--surface2); border: 1px solid var(--border2); color: var(--text2); padding: 4px 9px; border-radius: var(--radius-sm); cursor: pointer; font-size: 11px; font-weight: 500; transition: all .15s; white-space: nowrap; }
.ll-nav-btn:hover  { background: var(--surface); border-color: var(--coral); color: var(--coral); }
.ll-play-btn  { background: var(--blue-light); border: 1px solid var(--blue); color: var(--blue); min-width: 68px; font-weight: 600; padding: 4px 9px; border-radius: var(--radius-sm); cursor: pointer; font-size: 11px; transition: all .15s; }
.ll-play-btn:hover { background: var(--blue); color: #fff; }

/* Layout */
.ll-main      { display: flex; flex: 1; overflow: hidden; position: relative; }
.ll-left-col  { display: flex; flex-direction: column; overflow: hidden; min-width: 220px; max-width: 75%; }
.ll-resizer   { width: 5px; cursor: col-resize; background: var(--border); flex-shrink: 0; transition: background .15s; position: relative; z-index: 20; }
.ll-resizer:hover, .ll-resizer.drag { background: var(--coral); }
.ll-right-col { display: flex; flex-direction: column; flex: 1; overflow: hidden; min-width: 0; height: 100%; }
.ll-viz-wrap  { flex-shrink: 0; background: var(--surface); border-bottom: 1px solid var(--border); position: relative; overflow-x: auto; overflow-y: auto; }
.ll-perm-area { display: flex; flex-direction: column; align-items: stretch; min-height: 100%; width: 100%; min-width: 0; box-sizing: border-box; }
.ll-ptrs          { display: flex; gap: 8px; flex-wrap: wrap; padding: 4px 14px; min-height: 28px; width: 100%; box-sizing: border-box; min-width: 0; align-items: center; }
.ll-ptrs-compact  { flex-wrap: nowrap; gap: 6px; padding: 3px 14px 5px; min-height: 0; overflow-x: auto; overflow-y: hidden; }
.ll-ptr-chip-inline { display: inline-flex; align-items: center; gap: 4px; background: var(--surface2); border: 1px solid var(--border); border-radius: 6px; padding: 2px 8px; font-size: 11px; font-family: monospace; white-space: nowrap; flex-shrink: 0; line-height: 1.4; }
.ll-chip-label { color: #8899aa; font-weight: 500; margin-right: 2px; }
.ll-c-blue   { color: var(--blue); }
.ll-c-green  { color: var(--green); }
.ll-c-orange { color: var(--orange); }
.ll-c-purple { color: var(--purple); }

/* ─── Board Container — Reverse String ─────────────────────────────────────── */
.ll-board-container { display: flex; flex-direction: column; align-items: flex-start; padding: 6px 14px 10px; gap: 8px; min-width: 0; width: 100%; }

.rs-tier-title {
  font-size: 10px; font-weight: 700; text-transform: uppercase; letter-spacing: .04em;
  color: var(--muted); margin-top: 4px; margin-bottom: 2px; font-family: 'Consolas', monospace;
  border-left: 3px solid var(--coral); padding-left: 6px; display: flex; align-items: center; gap: 6px;
}
.rs-len-badge { font-size: 9px; color: var(--muted); background: var(--surface2); border: 1px solid var(--border); border-radius: 3px; padding: 0 4px; font-weight: 500; text-transform: none; letter-spacing: 0; }

/* Array frame */
.rs-array-frame {
  background: #f8fafc; border: 1px solid var(--border);
  border-radius: var(--radius); padding: 6px 10px 8px; box-shadow: var(--shadow-sm);
  display: flex; flex-direction: column; gap: 4px;
}

/* Pointer row */
.rs-ptr-row { display: flex; gap: 4px; min-height: 18px; align-items: flex-end; }
.rs-ptr-cell { width: 42px; display: flex; justify-content: center; align-items: flex-end; flex-shrink: 0; }
.rs-ptr-label { font-size: 8.5px; font-weight: 800; font-family: monospace; padding: 1px 4px; border-radius: 3px; line-height: 1.2; white-space: nowrap; }
.rs-ptr-left  { background: #eff6ff; color: #1d4ed8; border: 1px solid #93c5fd; }
.rs-ptr-right { background: #f0fdf4; color: #15803d; border: 1px solid #86efac; }

/* Character row */
.rs-char-row { display: flex; gap: 4px; flex-wrap: wrap; }

/* Character box */
.rs-char-box {
  width: 42px; height: 50px; border-radius: var(--radius-sm);
  background: #e2e8f0; border: 1.5px solid #94a3b8;
  display: flex; flex-direction: column; align-items: center; justify-content: center;
  gap: 2px; flex-shrink: 0; transition: all .2s ease; cursor: default;
}
.rs-char-val { font-family: 'Cascadia Code', 'Fira Code', monospace; font-size: 18px; font-weight: 900; color: var(--text); line-height: 1; }
.rs-char-idx { font-family: monospace; font-size: 7.5px; color: var(--muted); font-weight: 600; }

/* Character state classes */
.rs-char-inner        { background: #f1f5f9 !important; }
.rs-char-left         { background: #bfdbfe !important; border: 2px solid var(--blue) !important; box-shadow: 0 0 8px rgba(59,130,246,.35); }
.rs-char-right        { background: #bbf7d0 !important; border: 2px solid var(--green) !important; box-shadow: 0 0 8px rgba(34,197,94,.35); }
.rs-char-reading      { background: #fef9c3 !important; border: 2px solid #eab308 !important; box-shadow: 0 0 8px rgba(234,179,8,.4); animation: rs-flash-read .45s ease; }
.rs-char-swapped-new-l{ background: #bfdbfe !important; border: 2px solid var(--blue) !important; box-shadow: 0 0 10px rgba(59,130,246,.5); animation: rs-flash-swap .4s ease; }
.rs-char-swapped-done-l{ background: #dcfce7 !important; border: 2px solid var(--green) !important; box-shadow: 0 0 8px rgba(34,197,94,.4); animation: rs-flash-done .4s ease; }
.rs-char-swapped-done-r{ background: #dcfce7 !important; border: 2px solid var(--green) !important; box-shadow: 0 0 8px rgba(34,197,94,.4); animation: rs-flash-done .4s ease; }
.rs-char-processed    { opacity: .4; background: #f8fafc !important; border-color: #e2e8f0 !important; }
.rs-char-done         { background: #dcfce7 !important; border: 1.5px solid #86efac !important; }

/* Swap connector bar */
.rs-swap-bar { margin-top: 4px; }
.rs-swap-connector {
  display: flex; align-items: center; gap: 4px;
  background: #fef9c3; border: 1.5px solid #fbbf24;
  border-radius: var(--radius-sm); padding: 4px 10px;
  font-size: 10.5px; font-family: monospace; font-weight: 700;
}
.rs-swap-left-marker  { color: #1d4ed8; background: #eff6ff; border: 1px solid #93c5fd; border-radius: 3px; padding: 0 5px; white-space: nowrap; }
.rs-swap-right-marker { color: #15803d; background: #f0fdf4; border: 1px solid #86efac; border-radius: 3px; padding: 0 5px; white-space: nowrap; }
.rs-swap-line { flex: 1; display: flex; align-items: center; justify-content: center; gap: 6px; }
.rs-swap-arrow-l { color: #eab308; font-size: 14px; }
.rs-swap-arrow-r { color: #eab308; font-size: 14px; }
.rs-swap-label { color: #92400e; font-weight: 700; font-size: 10px; }

/* Variable state panel */
.rs-var-panel { display: flex; gap: 6px; flex-wrap: wrap; }
.rs-var-card {
  display: flex; flex-direction: column; align-items: center; gap: 2px;
  padding: 5px 10px; border-radius: var(--radius-sm); border: 1.5px solid var(--border);
  background: var(--surface2); min-width: 72px; font-size: 10.5px; font-family: monospace;
  transition: all .15s;
}
.rs-var-active-l { border-color: var(--blue)  !important; background: #eff6ff !important; }
.rs-var-active-r { border-color: var(--green) !important; background: #f0fdf4 !important; }
.rs-var-active-t { border-color: #eab308 !important; background: #fefce8 !important; }
.rs-var-done     { border-color: var(--green) !important; background: var(--green-light) !important; }
.rs-var-name   { font-size: 9.5px; font-weight: 800; padding: 1px 5px; border-radius: 3px; }
.rs-var-val    { font-size: 17px; font-weight: 900; color: var(--text); line-height: 1.2; }
.rs-var-desc   { font-size: 8.5px; color: var(--muted); text-align: center; }
.rs-temp-val   { color: #92400e; }
.rs-name-left   { background: #eff6ff; color: #1d4ed8; border: 1px solid #93c5fd; }
.rs-name-right  { background: #f0fdf4; color: #15803d; border: 1px solid #86efac; }
.rs-name-temp   { background: #fefce8; color: #92400e; border: 1px solid #fbbf24; }
.rs-name-result { background: var(--purple-light); color: var(--purple); border: 1px solid #c4b5fd; }

/* Progress bar */
.rs-progress-row { display: flex; gap: 3px; margin-top: 4px; }
.rs-progress-cell { height: 6px; flex: 1; border-radius: 3px; background: var(--border); transition: all .2s; }
.rs-prog-done     { background: #86efac !important; }
.rs-prog-active-l { background: var(--blue) !important; }
.rs-prog-active-r { background: var(--green) !important; }
.rs-prog-inner    { background: #e2e8f0 !important; }
.rs-progress-label { font-size: 9px; color: var(--muted); font-weight: 600; font-family: monospace; }

/* Legend dot */
.rs-legdot-swapped { background: #dcfce7; border: 1.5px solid #86efac; display: inline-block; width: 11px; height: 11px; border-radius: 3px; }

/* Shared helpers */
.ll-vresizer { height: 5px; cursor: row-resize; background: var(--border); flex-shrink: 0; transition: background .15s; position: relative; z-index: 20; }
.ll-vresizer:hover, .ll-vresizer.drag { background: var(--coral); }
.ll-legend { display: flex; flex-wrap: wrap; gap: 6px 14px; padding: 6px 12px; border-bottom: 1px solid var(--border); flex-shrink: 0; background: var(--surface2); }
.ll-leg    { display: flex; align-items: center; gap: 5px; font-size: 11px; color: var(--text2); font-weight: 500; }
.ll-legdot { width: 11px; height: 11px; border-radius: 3px; flex-shrink: 0; display: inline-block; }

.ll-table-area  { flex-shrink: 0; padding: 8px 14px; border-bottom: 1px solid var(--border); overflow-x: hidden; overflow-y: auto; background: var(--surface); min-width: 0; box-sizing: border-box; }
.ll-table-title { font-size: 10px; color: var(--muted); margin-bottom: 4px; font-style: italic; }
.ll-stack-line  { font-family: 'Consolas', monospace; font-size: 12px; line-height: 1.8; }
.ll-frame       { font-family: 'Consolas', monospace; font-size: 11.5px; color: var(--text2); padding: 1px 0; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
.ll-frame-cur   { color: var(--orange); background: var(--orange-light); border-radius: 4px; padding: 1px 5px; }
.ll-fname       { color: var(--text2); }
.ll-now         { color: var(--orange); font-size: 10px; margin-left: 6px; }

.ll-badge-wrap { padding: 6px 10px; border-bottom: 1px solid var(--border); flex-shrink: 0; min-height: 36px; display: flex; align-items: center; background: var(--surface); }
.ll-badge      { display: inline-block; padding: 4px 12px; border-radius: var(--radius-sm); border-left: 3px solid var(--coral); background: var(--coral-light); font-size: 11px; color: var(--coral-dark); line-height: 1.4; word-break: break-word; font-weight: 500; }
.ll-badge-error   { border-left-color: var(--red)   !important; background: var(--red-light)   !important; color: var(--red-dark) !important; }
.ll-badge-success { border-left-color: var(--green) !important; background: var(--green-light) !important; color: #15803d !important; }

/* Code panel */
.ll-code-panel  { display: flex; flex-direction: column; height: 100%; overflow: hidden; }
.ll-code-header { display: flex; align-items: center; gap: 6px; padding: 5px 12px; background: var(--surface); border-bottom: 1px solid var(--border); box-shadow: var(--shadow-sm); flex-shrink: 0; flex-wrap: wrap; }
.ll-tabbar      { display: flex; gap: 3px; flex-wrap: wrap; }
.ll-tab-btn     { padding: 4px 9px; font-size: 10.5px; font-weight: 600; border: 1px solid var(--border2); background: var(--surface2); color: var(--text2); border-radius: var(--radius-sm); cursor: pointer; transition: all .15s ease; white-space: nowrap; }
.ll-tab-btn:hover  { border-color: var(--coral); color: var(--coral); }
.ll-tab-btn.active { background: var(--coral); border-color: var(--coral); color: #fff; }
.ll-lang-select {
  margin-left: auto; padding: 4px 24px 4px 8px; font-size: 11px; font-weight: 500;
  border: 1px solid var(--border2); border-radius: var(--radius-sm); background: var(--surface2); color: var(--text);
  cursor: pointer; appearance: none;
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='10' height='6' viewBox='0 0 10 6'%3E%3Cpath d='M0 0l5 6 5-6z' fill='%2394a3b8'/%3E%3C/svg%3E");
  background-repeat: no-repeat; background-position: right 8px center; min-width: 95px; transition: border-color .15s;
}
.ll-lang-select:focus { outline: none; border-color: var(--coral); box-shadow: 0 0 0 3px rgba(240,77,77,.1); }
.ll-code-scroll { flex: 1; overflow: auto; background: #f8fafc; padding: 10px 14px; min-width: 0; }
.ll-pre         { margin: 0; font-family: 'Cascadia Code', 'Fira Code', 'Consolas', monospace; font-size: 11px; line-height: 1.5; color: var(--text); white-space: pre; padding-bottom: 150px; }
.ll-codeline    { display: block; padding: 0 14px; margin: 0 -14px; }
.ll-hl          { background: #dcfce7; color: #15803d; font-weight: 600; border-left: 3px solid var(--green); border-radius: 3px; }

.ll-info-scroll { flex: 1; overflow: auto; padding: 12px 16px; background: var(--surface); color: var(--text2); font-size: 12px; line-height: 1.55; }
.ll-info-scroll h3, .ll-info-scroll h4 { color: var(--text); margin: 0 0 6px; }
.ll-info-scroll p { margin: 0 0 6px; }
.ll-info-scroll code { background: var(--surface2); padding: 1px 4px; border-radius: 3px; font-family: 'Cascadia Code', monospace; font-size: 11px; color: var(--coral-dark); }
.ll-cx-heading  { font-size: 13px; font-weight: 700; }
.ll-cx-intro    { font-size: 10.5px; margin: 0 0 10px; line-height: 1.55; }
.ll-cx-sub      { font-size: 11px; font-weight: 700; color: var(--text2); margin: 10px 0 4px; border-bottom: 1px solid var(--border); padding-bottom: 3px; }
.ll-complexity-table { width: 100%; border-collapse: collapse; font-size: 10.5px; margin: 8px 0; }
.ll-complexity-table th, .ll-complexity-table td { border: 1px solid var(--border); padding: 4px 8px; text-align: left; }
.ll-complexity-table th { background: var(--surface2); font-weight: 700; color: var(--text2); }
.ll-cx-good { color: #15803d; font-weight: 700; }
.ll-cx-summary-grid { display: flex; gap: 8px; flex-wrap: wrap; margin: 6px 0 10px; }
.ll-cx-card     { flex: 1; min-width: 90px; border-radius: var(--radius-sm); padding: 8px 10px; text-align: center; border: 1.5px solid var(--border); }
.ll-cx-card-good { background: #f0fdf4; border-color: #86efac; color: #15803d; }
.ll-cx-card-label { font-size: 9px; font-weight: 700; text-transform: uppercase; letter-spacing: .05em; opacity: .7; margin-bottom: 4px; }
.ll-cx-card-val  { font-size: 13px; font-weight: 800; font-family: monospace; margin-bottom: 3px; }
.ll-cx-card-note { font-size: 8.5px; opacity: .75; line-height: 1.3; }
.ll-note { background: #fefce8; border: 1px solid #fef08a; border-left: 3px solid #eab308; padding: 6px 10px; font-size: 10.5px; color: #854d0e; border-radius: 0 4px 4px 0; margin-top: 10px; margin-bottom: 120px; }
.ll-footer { display: flex; align-items: center; justify-content: space-between; padding: 4px 12px; background: var(--surface); border-top: 1px solid var(--border); font-size: 11px; color: var(--muted); font-weight: 600; flex-shrink: 0; }
.ll-speed-wrap { display: flex; align-items: center; gap: 6px; }
.ll-speed-wrap input[type="range"] { width: 80px; accent-color: var(--coral); }

/* Missing var */
.ll-c-purple { color: var(--purple); }
.ll-purple-light { color: var(--purple-light); }
</style>
