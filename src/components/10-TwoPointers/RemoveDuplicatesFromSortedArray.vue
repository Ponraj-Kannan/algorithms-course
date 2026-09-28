<script setup>
import { ref, reactive, computed, onMounted, onBeforeUnmount, watch, nextTick } from 'vue';

defineProps({
  topic:    { type: String, default: 'Two Pointer Algorithms' },
  subTopic: { type: String, default: 'Remove Duplicates from Sorted Array (LeetCode 26)' }
});

// ─── Multi-Language Code Definitions ──────────────────────────────────────────
const CODES = {
  java: [
    ['',            'import java.util.Scanner;'],
    ['',            ''],
    ['',            'class Solution {'],
    ['fn_entry',    '    public int removeDuplicates(int[] nums) {'],
    ['init_k',      '        int k = 0;'],
    ['for_init',    '        for (int i = 1; i < nums.length; i++) {'],
    ['if_not_dup',  '            if (nums[i] != nums[k]) {'],
    ['inc_k',       '                k++;'],
    ['assign_nums', '                nums[k] = nums[i];'],
    ['',            '            }'],
    ['',            '        }'],
    ['return_stmt', '        return k + 1;'],
    ['',            '    }'],
    ['',            ''],
    ['',            '    public static void main(String[] args) {'],
    ['m_scanner',   '        Scanner sc = new Scanner(System.in);'],
    ['m_read_n',    '        int n = sc.nextInt();'],
    ['m_alloc',     '        int[] nums = new int[n];'],
    ['m_loop',      '        for (int i = 0; i < n; i++) {'],
    ['m_read_elem', '            nums[i] = sc.nextInt();'],
    ['',            '        }'],
    ['m_call',      '        int result = new Solution().removeDuplicates(nums);'],
    ['m_print',     '        System.out.println(result);'],
    ['',            '    }'],
    ['',            '}']
  ],
  cpp: [
    ['',            '#include <vector>'],
    ['',            '#include <iostream>'],
    ['',            'using namespace std;'],
    ['',            ''],
    ['',            'class Solution {'],
    ['',            'public:'],
    ['fn_entry',    '    int removeDuplicates(vector<int>& nums) {'],
    ['init_k',      '        int k = 0;'],
    ['for_init',    '        for (int i = 1; i < (int)nums.size(); i++) {'],
    ['if_not_dup',  '            if (nums[i] != nums[k]) {'],
    ['inc_k',       '                k++;'],
    ['assign_nums', '                nums[k] = nums[i];'],
    ['',            '            }'],
    ['',            '        }'],
    ['return_stmt', '        return k + 1;'],
    ['',            '    }'],
    ['',            '};'],
    ['',            ''],
    ['',            'int main() {'],
    ['m_read_n',    '    int n;'],
    ['m_scanner',   '    cin >> n;'],
    ['m_alloc',     '    vector<int> nums(n);'],
    ['m_loop',      '    for (int i = 0; i < n; i++) {'],
    ['m_read_elem', '        cin >> nums[i];'],
    ['',            '    }'],
    ['m_call',      '    cout << Solution().removeDuplicates(nums) << endl;'],
    ['',            '    return 0;'],
    ['',            '}']
  ],
  python: [
    ['',            'class Solution:'],
    ['fn_entry',    '    def removeDuplicates(self, nums):'],
    ['init_k',      '        k = 0'],
    ['for_init',    '        for i in range(1, len(nums)):'],
    ['if_not_dup',  '            if nums[i] != nums[k]:'],
    ['inc_k',       '                k += 1'],
    ['assign_nums', '                nums[k] = nums[i]'],
    ['return_stmt', '        return k + 1'],
    ['',            ''],
    ['',            'def main():'],
    ['m_read_elem', '    nums = list(map(int, input().split()))'],
    ['m_call',      '    print(Solution().removeDuplicates(nums))'],
    ['',            ''],
    ['',            'if __name__ == "__main__":'],
    ['',            '    main()']
  ],
  javascript: [
    ['',            '/**'],
    ['',            ' * @param {number[]} nums'],
    ['',            ' * @return {number}'],
    ['',            ' */'],
    ['fn_entry',    'var removeDuplicates = function(nums) {'],
    ['init_k',      '    let k = 0;'],
    ['for_init',    '    for (let i = 1; i < nums.length; i++) {'],
    ['if_not_dup',  '        if (nums[i] !== nums[k]) {'],
    ['inc_k',       '            k++;'],
    ['assign_nums', '            nums[k] = nums[i];'],
    ['',            '        }'],
    ['',            '    }'],
    ['return_stmt', '    return k + 1;'],
    ['',            '};'],
    ['',            ''],
    ['',            'function main() {'],
    ['m_read_elem', '    const nums = lines[0].split(\' \').map(Number);'],
    ['m_call',      '    console.log(removeDuplicates(nums));'],
    ['',            '}'],
    ['',            'main();']
  ],
  c: [
    ['',            '#include <stdio.h>'],
    ['',            ''],
    ['fn_entry',    'int removeDuplicates(int* nums, int n) {'],
    ['init_k',      '    int k = 0;'],
    ['for_init',    '    for (int i = 1; i < n; i++) {'],
    ['if_not_dup',  '        if (nums[i] != nums[k]) {'],
    ['inc_k',       '            k++;'],
    ['assign_nums', '            nums[k] = nums[i];'],
    ['',            '        }'],
    ['',            '    }'],
    ['return_stmt', '    return k + 1;'],
    ['',            '}'],
    ['',            ''],
    ['',            'int main() {'],
    ['m_read_n',    '    int n;'],
    ['m_scanner',   '    scanf("%d", &n);'],
    ['m_alloc',     '    int nums[200];'],
    ['m_loop',      '    for (int i = 0; i < n; i++) {'],
    ['m_read_elem', '        scanf("%d", &nums[i]);'],
    ['',            '    }'],
    ['m_call',      '    printf("%d\\n", removeDuplicates(nums, n));'],
    ['',            '    return 0;'],
    ['',            '}']
  ]
};

const PSEUDOCODE = [
  'function removeDuplicates(nums):',
  '    k = 0                   // write pointer',
  '',
  '    for i = 1 to len-1:     // read pointer',
  '        if nums[i] != nums[k]:',
  '            k  += 1          // advance write head',
  '            nums[k] = nums[i]// place unique value',
  '',
  '    return k + 1             // count of unique elements'
];

// ─── Step Builder ──────────────────────────────────────────────────────────────
function parseInput(raw) {
  return raw
    .replace(/[^\d,\-\s]/g, '')
    .split(/[,\s]+/)
    .map(x => parseInt(x.trim(), 10))
    .filter(x => !isNaN(x))
    .slice(0, 12);
}

function buildSteps(rawInput) {
  const steps = [];
  const arr   = parseInput(rawInput);
  if (arr.length === 0) {
    return steps;
  }
  const n = arr.length;

  const NONE = { kReady: false };
  const ALL  = { kReady: true };

  function snap(k, i, arrState, code, phase, initState, extra) {
    return {
      k, i,
      arr: [...arrState],
      n, code, phase,
      initState: { ...initState },
      ...extra
    };
  }

  const currArr = [...arr];

  // ── Input phase ─────────────────────────────────────────────────────────────
  steps.push({
    ...snap(-1, -1, currArr, 'm_scanner', 'input', NONE, { partial: new Array(n).fill('') }),
    badge: `Scanner sc = new Scanner(System.in); → Initializing input stream.`
  });
  steps.push({
    ...snap(-1, -1, currArr, 'm_read_n', 'input', NONE, { partial: new Array(n).fill('') }),
    badge: `int n = sc.nextInt(); → n = ${n}. Will read ${n} sorted integers.`
  });
  steps.push({
    ...snap(-1, -1, currArr, 'm_alloc', 'input', NONE, { partial: new Array(n).fill('') }),
    badge: `int[] nums = new int[n]; → Allocated array of size ${n}.`
  });

  const partialArr = new Array(n).fill('');
  for (let ii = 0; ii < n; ii++) {
    steps.push({
      ...snap(-1, -1, currArr, 'm_loop', 'input', NONE, { partial: [...partialArr], readIdx: ii }),
      badge: `for (int i = ${ii}; i < ${n}; i++) → i = ${ii}. Reading nums[${ii}].`
    });
    partialArr[ii] = currArr[ii];
    steps.push({
      ...snap(-1, -1, currArr, 'm_read_elem', 'input', NONE, { partial: [...partialArr], readIdx: ii }),
      badge: `nums[${ii}] = sc.nextInt(); → nums[${ii}] = ${currArr[ii]}.`
    });
  }
  steps.push({
    ...snap(-1, -1, currArr, 'm_loop', 'input', NONE, { partial: [...partialArr] }),
    badge: `for (int i = ${n}; i < ${n}; i++) → i = ${n} ≥ n. Input loop exits.`
  });
  steps.push({
    ...snap(-1, -1, currArr, 'm_call', 'input', NONE, { partial: [...partialArr] }),
    badge: `int result = new Solution().removeDuplicates(nums); → All elements read. Calling removeDuplicates().`
  });

  // ── removeDuplicates() ──────────────────────────────────────────────────────
  steps.push({
    ...snap(-1, -1, currArr, 'fn_entry', 'init', NONE, {}),
    badge: `public int removeDuplicates(int[] nums) → Function entry. k is the write pointer; i is the read pointer.`
  });

  let k = 0;
  steps.push({
    ...snap(0, -1, currArr, 'init_k', 'init', ALL, {}),
    badge: `int k = 0; → k = 0. Write pointer starts at index 0 (first element is always unique).`
  });

  // ── For loop ────────────────────────────────────────────────────────────────
  let i = 1;
  steps.push({
    ...snap(k, i, currArr, 'for_init', 'solve', ALL, {}),
    badge: `for (int i = 1; ...) → i = 1. Read pointer starts at index 1.`
  });

  while (i < n) {
    steps.push({
      ...snap(k, i, currArr, 'for_cond', 'solve', ALL, { condTrue: true }),
      badge: `i < nums.length: ${i} < ${n} → TRUE. Check nums[${i}]=${currArr[i]} vs nums[k=${k}]=${currArr[k]}.`
    });

    const isDup = currArr[i] === currArr[k];
    steps.push({
      ...snap(k, i, currArr, 'if_not_dup', 'solve', ALL, { isDup }),
      badge: `if (nums[${i}] != nums[${k}]): ${currArr[i]} ${isDup ? '==' : '!='} ${currArr[k]} → ${isDup ? 'FALSE (duplicate — skip).' : 'TRUE (unique — write it).'}`
    });

    if (!isDup) {
      k++;
      steps.push({
        ...snap(k, i, currArr, 'inc_k', 'solve', ALL, {}),
        badge: `k++; → k = ${k}. Write pointer advanced to next slot.`
      });

      currArr[k] = currArr[i];
      steps.push({
        ...snap(k, i, [...currArr], 'assign_nums', 'solve', ALL, { justWritten: k }),
        badge: `nums[k] = nums[i]; → nums[${k}] = ${currArr[k]}. Unique element placed at write position.`
      });
    }

    i++;
    steps.push({
      ...snap(k, i, [...currArr], 'for_cond', 'solve', ALL, { isInc: true }),
      badge: `i++; → i = ${i}. Read pointer advances.`
    });
  }

  steps.push({
    ...snap(k, i, [...currArr], 'for_cond', 'done', ALL, { condTrue: false }),
    badge: `i < nums.length: ${i} < ${n} → FALSE. All elements scanned. ${k + 1} unique element${k + 1 !== 1 ? 's' : ''} found.`
  });
  steps.push({
    ...snap(k, i, [...currArr], 'return_stmt', 'done', ALL, { result: k + 1 }),
    badge: `return k + 1; → k = ${k}, return ${k + 1}. The first ${k + 1} element${k + 1 !== 1 ? 's' : ''} of nums[] are unique: [${currArr.slice(0, k + 1).join(', ')}].`
  });

  return steps;
}

// ─── Reactive State ───────────────────────────────────────────────────────────
const inputNums   = ref('1,1,2,2,3');
const lang        = ref('java');
const speed       = ref(650);
const si          = ref(0);
const playing     = ref(false);
const vizHeight   = ref(360);
const tableHeight = ref(60);
const leftWidth   = ref(52);
const rightTab    = ref('code');

const stepsData = reactive({ steps: buildSteps('1,1,2,2,3') });
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
  const parsed = parseInput(inputNums.value);
  if (parsed.length < 2) {
    alert('Please enter at least 2 integers.');
    return;
  }
  playing.value = false;
  stepsData.steps = buildSteps(inputNums.value);
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
const initState    = computed(() => s.value.initState || { kReady: false });
const displayK     = computed(() => initState.value.kReady ? s.value.k : '?');
const displayI     = computed(() => (initState.value.kReady && s.value.i >= 0) ? s.value.i : '?');
const displayArr   = computed(() => {
  if (s.value.phase === 'input') {
    return s.value.partial || [];
  }
  return s.value.arr || [];
});
const displayN     = computed(() => s.value.n || 0);
const displayResult= computed(() => s.value.result !== undefined ? s.value.result : '—');
const displayNumsK = computed(() => {
  const arr = s.value.arr;
  const k   = s.value.k;
  if (!arr || k < 0 || !initState.value.kReady) {
    return '?';
  }
  return arr[k] !== undefined ? arr[k] : '?';
});
const displayNumsI = computed(() => {
  const arr = s.value.arr;
  const i   = s.value.i;
  if (!arr || i < 0 || !initState.value.kReady) {
    return '?';
  }
  return arr[i] !== undefined ? arr[i] : '?';
});

function numClass(idx) {
  const phase = s.value.phase;
  const ist   = initState.value;

  if (phase === 'input') {
    const ri = s.value.readIdx;
    return ri !== undefined && idx === ri ? 'rd-num-reading' : '';
  }
  if (!ist.kReady) {
    return '';
  }

  const k    = s.value.k;
  const i    = s.value.i;
  const code = s.value.code;
  const isDup= s.value.isDup;
  const jw   = s.value.justWritten;

  if (phase === 'done') {
    return idx <= k ? 'rd-num-unique-final' : 'rd-num-tail';
  }

  if (idx < k) {
    return 'rd-num-unique';
  }
  if (idx === k) {
    if (code === 'assign_nums' && jw === k) {
      return 'rd-num-just-written';
    }
    return 'rd-num-write';
  }
  if (i >= 0) {
    if (idx < i) {
      return 'rd-num-skipped';
    }
    if (idx === i) {
      if (code === 'if_not_dup') {
        return isDup ? 'rd-num-dup' : 'rd-num-unique-read';
      }
      return 'rd-num-read';
    }
  }
  return 'rd-num-unread';
}

function showK(idx) {
  const phase = s.value.phase;
  const ist   = initState.value;
  return (phase === 'solve' || phase === 'init' || phase === 'done') && ist.kReady && idx === s.value.k;
}

function showI(idx) {
  const phase = s.value.phase;
  const ist   = initState.value;
  return phase === 'solve' && ist.kReady && s.value.i >= 0 && idx === s.value.i && idx !== s.value.k;
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
              <label>nums (sorted):</label>
              <input
                type="text"
                v-model="inputNums"
                class="ll-text-input rd-nums-input"
                @keyup.enter="applyInput"
                placeholder="e.g. 1,1,2,2,3"
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
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">k</span><b class="ll-c-blue">{{ displayK }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">i</span><b class="ll-c-green">{{ displayI }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">nums[k]</span><b class="ll-c-blue">{{ displayNumsK }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">nums[i]</span><b class="ll-c-green">{{ displayNumsI }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">result</span><b class="ll-c-orange">{{ displayResult }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">phase</span><b class="ll-c-orange">{{ s.phase || 'input' }}</b></span>
                  </div>

                  <!-- BOARD CONTAINER -->
                  <div class="ll-board-container">

                    <!-- Tier 1 — Array Visualization -->
                    <div class="rd-tier-title">
                      Tier 1 &mdash; int[] nums
                      <span class="rd-badge">n = {{ displayN }}</span>
                      <span v-if="s.phase === 'solve' || s.phase === 'done'" class="rd-badge rd-badge-k">unique prefix: 0..{{ s.k }}</span>
                    </div>

                    <div class="rd-array-frame">
                      <!-- Pointer label row (above boxes) -->
                      <div class="rd-ptr-row">
                        <div
                          v-for="(val, idx) in displayArr"
                          :key="'ptr-' + idx"
                          class="rd-ptr-cell"
                        >
                          <span v-if="showK(idx)" class="rd-ptr-label rd-ptr-k">k</span>
                          <span v-if="showI(idx)" class="rd-ptr-label rd-ptr-i">i</span>
                        </div>
                      </div>

                      <!-- Number boxes -->
                      <div class="rd-num-row">
                        <div
                          v-for="(val, idx) in displayArr"
                          :key="'num-' + idx"
                          class="rd-num-box"
                          :class="numClass(idx)"
                        >
                          <span class="rd-num-val">{{ val !== '' ? val : '\u00A0' }}</span>
                          <span class="rd-num-idx">[{{ idx }}]</span>
                        </div>
                      </div>

                      <!-- Unique-prefix zone bar -->
                      <div class="rd-zone-row" v-if="(s.phase === 'solve' || s.phase === 'done') && s.k >= 0">
                        <div
                          v-for="(val, idx) in displayArr"
                          :key="'zone-' + idx"
                          class="rd-zone-cell"
                          :class="{
                            'rd-zone-unique': idx <= s.k,
                            'rd-zone-cur-i':  idx === s.i && s.phase === 'solve'
                          }"
                        ></div>
                      </div>
                      <div class="rd-zone-labels" v-if="(s.phase === 'solve' || s.phase === 'done') && s.k >= 0">
                        <span class="rd-zone-label-u">unique [0..{{ s.k }}]</span>
                        <span v-if="s.phase === 'solve'" class="rd-zone-label-i">i={{ s.i }}</span>
                      </div>
                    </div>

                    <!-- Tier 2 — Variable State Panel -->
                    <div class="rd-tier-title">Tier 2 &mdash; Variable State</div>
                    <div class="rd-var-panel">
                      <div class="rd-var-card" :class="{ 'rd-var-active-k': initState.kReady }">
                        <span class="rd-var-name rd-name-k">k</span>
                        <span class="rd-var-val">{{ displayK }}</span>
                        <span class="rd-var-desc">write pointer</span>
                      </div>
                      <div class="rd-var-card" :class="{ 'rd-var-active-i': initState.kReady && s.i >= 0 }">
                        <span class="rd-var-name rd-name-i">i</span>
                        <span class="rd-var-val">{{ displayI }}</span>
                        <span class="rd-var-desc">read pointer</span>
                      </div>
                      <div class="rd-var-card" :class="{ 'rd-var-active-k': initState.kReady }">
                        <span class="rd-var-name rd-name-k">nums[k]</span>
                        <span class="rd-var-val">{{ displayNumsK }}</span>
                        <span class="rd-var-desc">last unique</span>
                      </div>
                      <div class="rd-var-card" :class="{ 'rd-var-active-i': initState.kReady && s.i >= 0 }">
                        <span class="rd-var-name rd-name-i">nums[i]</span>
                        <span class="rd-var-val">{{ displayNumsI }}</span>
                        <span class="rd-var-desc">current read</span>
                      </div>
                      <div class="rd-var-card" :class="{ 'rd-var-result': s.phase === 'done' }">
                        <span class="rd-var-name rd-name-result">result</span>
                        <span
                          class="rd-var-val"
                          :style="{ color: s.phase === 'done' ? '#15803d' : '#94a3b8' }"
                        >{{ displayResult }}</span>
                        <span class="rd-var-desc">k + 1</span>
                      </div>
                    </div>

                    <!-- Duplicate indicator panel (when if_not_dup is active) -->
                    <div
                      class="rd-dup-panel"
                      v-if="s.code === 'if_not_dup'"
                      :class="{ 'rd-dup-is-dup': s.isDup, 'rd-dup-is-unique': !s.isDup }"
                    >
                      <span class="rd-dup-icon">{{ s.isDup ? '\u26A0' : '\u2713' }}</span>
                      <span v-if="s.isDup">
                        nums[{{ s.i }}] = {{ s.arr && s.arr[s.i] }} equals nums[{{ s.k }}] = {{ s.arr && s.arr[s.k] }} &mdash; <strong>duplicate, skip</strong>
                      </span>
                      <span v-else>
                        nums[{{ s.i }}] = {{ s.arr && s.arr[s.i] }} differs from nums[{{ s.k }}] = {{ s.arr && s.arr[s.k] }} &mdash; <strong>unique, write</strong>
                      </span>
                    </div>

                  </div>
                  <!-- END ll-board-container -->
                </div>
              </div>

              <!-- Vertical Resizer -->
              <div class="ll-vresizer" ref="vizResizerRef"></div>

              <!-- Color Legend -->
              <div class="ll-legend">
                <span class="ll-leg"><span class="ll-legdot" style="background:#bfdbfe;border:1.5px solid #3b82f6;"></span>k</span>
                <span class="ll-leg"><span class="ll-legdot" style="background:#bbf7d0;border:1.5px solid #22c55e;"></span>i</span>
                <span class="ll-leg"><span class="ll-legdot" style="background:#d1fae5;border:1.5px solid #10b981;"></span>unique prefix</span>
                <span class="ll-leg"><span class="ll-legdot rd-legdot-dup"></span>duplicate</span>
                <span class="ll-leg"><span class="ll-legdot rd-legdot-written"></span>just written</span>
                <span class="ll-leg"><span class="ll-legdot" style="background:#f1f5f9;border:1.5px solid #cbd5e1;opacity:.5"></span>skipped</span>
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
                      removeDuplicates(nums)
                      &nbsp;
                      <span class="ll-fname">k</span>=<span class="ll-c-blue" style="font-weight:700">{{ displayK }}</span>,
                      <span class="ll-fname">i</span>=<span class="ll-c-green" style="font-weight:700">{{ displayI }}</span>
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
                    'll-badge-error':   s.badge && (s.badge.includes('FALSE') || s.badge.includes('duplicate')),
                    'll-badge-success': s.badge && (s.badge.includes('TRUE') || s.badge.includes('unique') || s.badge.includes('placed'))
                  }"
                >
                  {{ s.badge || 'Ready to visualize Remove Duplicates from Sorted Array.' }}
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
                    :class="{ 'll-hl': line[0] && (line[0] === s.code || (line[0] === 'for_init' && (s.code === 'for_cond' || s.code === 'for_inc'))) }"
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
                  <h3 class="ll-cx-heading">Remove Duplicates from Sorted Array — Complexity Analysis</h3>
                  <p class="ll-cx-intro">
                    Given a sorted integer array <code>nums</code>, remove duplicates in-place and return the count
                    of unique elements <code>k</code>. The first <code>k</code> elements of <code>nums</code> hold the
                    unique values. Uses a two-pointer approach: <code>k</code> is the write pointer, <code>i</code> is
                    the read pointer scanning every element.
                  </p>

                  <h4 class="ll-cx-sub">Step-by-step Breakdown</h4>
                  <table class="ll-complexity-table">
                    <thead>
                      <tr><th>Operation</th><th>Time</th><th>Space</th><th>Reason</th></tr>
                    </thead>
                    <tbody>
                      <tr><td>Init k</td><td class="ll-cx-good">O(1)</td><td class="ll-cx-good">O(1)</td><td>Single assignment</td></tr>
                      <tr><td>For loop (i = 1 to n-1)</td><td class="ll-cx-good">O(n)</td><td class="ll-cx-good">O(1)</td><td>Each element visited exactly once</td></tr>
                      <tr><td>if-check + write</td><td class="ll-cx-good">O(1)</td><td class="ll-cx-good">O(1)</td><td>Comparison + at most one write per iteration</td></tr>
                      <tr><td><strong>Total</strong></td><td class="ll-cx-good"><strong>O(n)</strong></td><td class="ll-cx-good"><strong>O(1)</strong></td><td>Single pass, in-place, no extra array</td></tr>
                    </tbody>
                  </table>

                  <h4 class="ll-cx-sub">Overall Complexity</h4>
                  <div class="ll-cx-summary-grid">
                    <div class="ll-cx-card ll-cx-card-good">
                      <div class="ll-cx-card-label">Time</div>
                      <div class="ll-cx-card-val">O(n)</div>
                      <div class="ll-cx-card-note">Single left-to-right scan</div>
                    </div>
                    <div class="ll-cx-card ll-cx-card-good">
                      <div class="ll-cx-card-label">Space</div>
                      <div class="ll-cx-card-val">O(1)</div>
                      <div class="ll-cx-card-note">Modifies array in-place</div>
                    </div>
                    <div class="ll-cx-card ll-cx-card-good">
                      <div class="ll-cx-card-label">Passes</div>
                      <div class="ll-cx-card-val">1</div>
                      <div class="ll-cx-card-note">One loop, no sorting needed</div>
                    </div>
                  </div>

                  <div class="ll-note">
                    <strong>Key insight:</strong> Because the array is already sorted, all duplicates of a value
                    are adjacent. The write pointer <code>k</code> stays behind the read pointer <code>i</code>;
                    every time <code>nums[i] != nums[k]</code>, we advance <code>k</code> and overwrite the
                    "slot" with the new unique value. Elements after index <code>k</code> are irrelevant —
                    only the first <code>k+1</code> positions matter.
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

@keyframes rd-flash-write  { 0%{background:#fef3c7;transform:scale(1.12);} 60%{background:#bbf7d0;} 100%{transform:scale(1);} }
@keyframes rd-flash-dup    { 0%{background:#fee2e2;transform:scale(1.06);} 100%{transform:scale(1);} }
@keyframes rd-flash-unique { 0%{background:#d1fae5;transform:scale(1.06);} 100%{transform:scale(1);} }

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
.rd-nums-input   { width: 200px; }
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
.ll-ptrs         { display: flex; gap: 8px; flex-wrap: wrap; padding: 4px 14px; min-height: 28px; width: 100%; box-sizing: border-box; min-width: 0; align-items: center; }
.ll-ptrs-compact { flex-wrap: nowrap; gap: 6px; padding: 3px 14px 5px; min-height: 0; overflow-x: auto; overflow-y: hidden; }
.ll-ptr-chip-inline { display: inline-flex; align-items: center; gap: 4px; background: var(--surface2); border: 1px solid var(--border); border-radius: 6px; padding: 2px 8px; font-size: 11px; font-family: monospace; white-space: nowrap; flex-shrink: 0; line-height: 1.4; }
.ll-chip-label { color: #8899aa; font-weight: 500; margin-right: 2px; }
.ll-c-blue   { color: var(--blue); }
.ll-c-green  { color: var(--green); }
.ll-c-orange { color: var(--orange); }

/* ─── Board Container — Remove Duplicates ────────────────────────────────── */
.ll-board-container { display: flex; flex-direction: column; align-items: flex-start; padding: 6px 14px 10px; gap: 8px; min-width: 0; width: 100%; }

.rd-tier-title {
  font-size: 10px; font-weight: 700; text-transform: uppercase; letter-spacing: .04em;
  color: var(--muted); font-family: 'Consolas', monospace;
  border-left: 3px solid var(--coral); padding-left: 6px;
  display: flex; align-items: center; gap: 6px;
}
.rd-badge { font-size: 9px; color: var(--muted); background: var(--surface2); border: 1px solid var(--border); border-radius: 3px; padding: 0 4px; font-weight: 500; text-transform: none; letter-spacing: 0; }
.rd-badge-k { background: #d1fae5; border-color: #10b981; color: #065f46; }

/* Array frame */
.rd-array-frame {
  background: #f8fafc; border: 1px solid var(--border);
  border-radius: var(--radius); padding: 6px 10px 8px;
  box-shadow: var(--shadow-sm); display: flex; flex-direction: column; gap: 3px;
}

/* Pointer label row */
.rd-ptr-row  { display: flex; gap: 4px; min-height: 18px; align-items: flex-end; }
.rd-ptr-cell { width: 44px; display: flex; justify-content: center; align-items: flex-end; flex-shrink: 0; }
.rd-ptr-label { font-size: 8.5px; font-weight: 800; font-family: monospace; padding: 1px 4px; border-radius: 3px; line-height: 1.2; white-space: nowrap; }
.rd-ptr-k { background: #eff6ff; color: #1d4ed8; border: 1px solid #93c5fd; }
.rd-ptr-i { background: #f0fdf4; color: #15803d; border: 1px solid #86efac; }

/* Number row */
.rd-num-row { display: flex; gap: 4px; flex-wrap: wrap; }

/* Number box */
.rd-num-box {
  width: 44px; height: 50px; border-radius: var(--radius-sm);
  background: #e2e8f0; border: 1.5px solid #94a3b8;
  display: flex; flex-direction: column; align-items: center; justify-content: center;
  gap: 2px; flex-shrink: 0; transition: all .2s ease; cursor: default;
}
.rd-num-val { font-family: 'Cascadia Code', 'Fira Code', monospace; font-size: 16px; font-weight: 900; color: var(--text); line-height: 1; }
.rd-num-idx { font-family: monospace; font-size: 7.5px; color: var(--muted); font-weight: 600; }

/* Number state classes */
.rd-num-reading      { background: #fef9c3 !important; border: 2px solid #eab308 !important; box-shadow: 0 0 6px rgba(234,179,8,.35); }
.rd-num-unique       { background: #d1fae5 !important; border: 1.5px solid #6ee7b7 !important; }
.rd-num-write        { background: #bfdbfe !important; border: 2px solid var(--blue) !important; box-shadow: 0 0 8px rgba(59,130,246,.35); }
.rd-num-just-written { background: #dcfce7 !important; border: 2px solid var(--green) !important; box-shadow: 0 0 10px rgba(34,197,94,.4); animation: rd-flash-write .4s ease; }
.rd-num-read         { background: #bbf7d0 !important; border: 2px solid var(--green) !important; box-shadow: 0 0 8px rgba(34,197,94,.35); }
.rd-num-dup          { background: #fee2e2 !important; border: 2px solid #f87171 !important; box-shadow: 0 0 6px rgba(239,68,68,.3); animation: rd-flash-dup .35s ease; }
.rd-num-unique-read  { background: #a7f3d0 !important; border: 2px solid #10b981 !important; box-shadow: 0 0 8px rgba(16,185,129,.4); animation: rd-flash-unique .35s ease; }
.rd-num-skipped      { opacity: .38; background: #f1f5f9 !important; border-color: #e2e8f0 !important; }
.rd-num-unread       { background: #f1f5f9 !important; border-color: #cbd5e1 !important; opacity: .75; }
.rd-num-unique-final { background: #d1fae5 !important; border: 1.5px solid #6ee7b7 !important; }
.rd-num-tail         { background: #f1f5f9 !important; border-color: #e2e8f0 !important; opacity: .3; }

/* Zone bar */
.rd-zone-row { display: flex; gap: 4px; margin-top: 4px; }
.rd-zone-cell { width: 44px; height: 5px; border-radius: 3px; background: var(--border); transition: all .2s; flex-shrink: 0; }
.rd-zone-unique { background: #6ee7b7 !important; }
.rd-zone-cur-i  { background: var(--green) !important; }
.rd-zone-labels { display: flex; gap: 6px; font-size: 8.5px; color: var(--muted); font-family: monospace; font-weight: 600; margin-top: 2px; }
.rd-zone-label-u { color: #059669; }
.rd-zone-label-i { color: #15803d; }

/* Variable state panel */
.rd-var-panel { display: flex; gap: 6px; flex-wrap: wrap; }
.rd-var-card {
  display: flex; flex-direction: column; align-items: center; gap: 2px;
  padding: 5px 10px; border-radius: var(--radius-sm); border: 1.5px solid var(--border);
  background: var(--surface2); min-width: 70px; font-size: 10.5px; font-family: monospace;
  transition: all .15s;
}
.rd-var-active-k { border-color: var(--blue)  !important; background: #eff6ff !important; }
.rd-var-active-i { border-color: var(--green) !important; background: #f0fdf4 !important; }
.rd-var-result   { border-color: var(--green) !important; background: var(--green-light) !important; }
.rd-var-name     { font-size: 9.5px; font-weight: 800; padding: 1px 5px; border-radius: 3px; }
.rd-var-val      { font-size: 17px; font-weight: 900; color: var(--text); line-height: 1.2; }
.rd-var-desc     { font-size: 8.5px; color: var(--muted); text-align: center; }
.rd-name-k      { background: #eff6ff; color: #1d4ed8; border: 1px solid #93c5fd; }
.rd-name-i      { background: #f0fdf4; color: #15803d; border: 1px solid #86efac; }
.rd-name-result { background: var(--purple-light); color: #7c3aed; border: 1px solid #c4b5fd; }

/* Duplicate indicator panel */
.rd-dup-panel {
  padding: 6px 12px; border-radius: var(--radius-sm); font-size: 11px;
  font-weight: 600; display: flex; align-items: center; gap: 8px;
  border: 1.5px solid var(--border); transition: all .2s; width: 100%; box-sizing: border-box;
}
.rd-dup-is-dup    { background: #fee2e2; border-color: #fca5a5; color: #991b1b; }
.rd-dup-is-unique { background: #d1fae5; border-color: #6ee7b7; color: #065f46; }
.rd-dup-icon { font-size: 14px; flex-shrink: 0; }

/* Legend dot */
.rd-legdot-dup     { background: #fee2e2; border: 1.5px solid #f87171; display: inline-block; width: 11px; height: 11px; border-radius: 3px; }
.rd-legdot-written { background: #dcfce7; border: 1.5px solid #6ee7b7; display: inline-block; width: 11px; height: 11px; border-radius: 3px; }

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
.ll-info-scroll p  { margin: 0 0 6px; }
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
</style>
