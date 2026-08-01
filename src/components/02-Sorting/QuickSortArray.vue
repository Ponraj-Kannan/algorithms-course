<script setup>
import { ref, reactive, computed, onMounted, onBeforeUnmount, watch } from 'vue';

defineProps({
  topic: { type: String, default: 'Quick Sort Algorithm' },
  subTopic: { type: String, default: 'Using Arrays (Partition & Conquer)' }
});

const CODES = {
  java: [
    ['', 'class QuickSort {'],
    ['c_entry', '    void quickSort(int[] arr, int low, int high) {'],
    ['c_base_check', '        if (low < high) {'],
    ['c_part_call', '            int pi = partition(arr, low, high);'],
    ['c_rec_left', '            quickSort(arr, low, pi - 1);'],
    ['c_rec_right', '            quickSort(arr, pi + 1, high);'],
    ['', '        }'],
    ['c_done', '    }'],
    ['', ''],
    ['c_part_fn', '    int partition(int[] arr, int low, int high) {'],
    ['c_pivot_set', '        int pivot = arr[high];'],
    ['c_i_init', '        int i = low - 1;'],
    ['c_j_loop', '        for (int j = low; j < high; j++) {'],
    ['c_check', '            if (arr[j] < pivot) {'],
    ['c_i_inc', '                i++;'],
    ['c_swap_inner', '                int temp = arr[i]; arr[i] = arr[j]; arr[j] = temp;'],
    ['', '            }'],
    ['', '        }'],
    ['c_pivot_swap', '        int temp = arr[i + 1]; arr[i + 1] = arr[high]; arr[high] = temp;'],
    ['c_return_pi', '        return i + 1;'],
    ['', '    }'],
    ['', '}']
  ],
  c: [
    ['c_entry', 'void quickSort(int arr[], int low, int high) {'],
    ['c_base_check', '    if (low < high) {'],
    ['c_part_call', '        int pi = partition(arr, low, high);'],
    ['c_rec_left', '        quickSort(arr, low, pi - 1);'],
    ['c_rec_right', '        quickSort(arr, pi + 1, high);'],
    ['', '    }'],
    ['c_done', '}'],
    ['', ''],
    ['c_part_fn',    'int partition(int arr[], int low, int high) {'],
    ['c_pivot_set',  '    int pivot = arr[high];'],
    ['c_i_init',     '    int i = low - 1;'],
    ['c_j_loop',     '    for (int j = low; j < high; j++) {'],
    ['c_check',      '        if (arr[j] < pivot) {'],
    ['c_i_inc',      '            i++;'],
    ['c_swap_inner', '            int temp = arr[i]; arr[i] = arr[j]; arr[j] = temp;'],
    ['',             '        }'],
    ['',             '    }'],
    ['c_pivot_swap', '    int temp = arr[i + 1]; arr[i + 1] = arr[high]; arr[high] = temp;'],
    ['c_return_pi',  '    return i + 1;'],
    ['',             '}']
  ],
  cpp: [
    ['', 'class QuickSort {'],
    ['', 'public:'],
    ['c_entry', '    void quickSort(vector<int>& arr, int low, int high) {'],
    ['c_base_check', '        if (low < high) {'],
    ['c_part_call', '            int pi = partition(arr, low, high);'],
    ['c_rec_left', '            quickSort(arr, low, pi - 1);'],
    ['c_rec_right', '            quickSort(arr, pi + 1, high);'],
    ['', '        }'],
    ['c_done', '    }'],
    ['', ''],
    ['c_part_fn',    '    int partition(vector<int>& arr, int low, int high) {'],
    ['c_pivot_set',  '        int pivot = arr[high];'],
    ['c_i_init',     '        int i = low - 1;'],
    ['c_j_loop',     '        for (int j = low; j < high; j++) {'],
    ['c_check',      '            if (arr[j] < pivot) {'],
    ['c_i_inc',      '                i++;'],
    ['c_swap_inner', '                swap(arr[i], arr[j]);'],
    ['',             '            }'],
    ['',             '        }'],
    ['c_pivot_swap', '        swap(arr[i + 1], arr[high]);'],
    ['c_return_pi',  '        return i + 1;'],
    ['',             '    }'],
    ['', '};']
  ],
  python: [
    ['', 'class QuickSort:'],
    ['c_entry', '    def quick_sort(self, arr, low, high):'],
    ['c_base_check', '        if low < high:'],
    ['c_part_call', '            pi = self.partition(arr, low, high)'],
    ['c_rec_left', '            self.quick_sort(arr, low, pi - 1)'],
    ['c_rec_right', '            self.quick_sort(arr, pi + 1, high)'],
    ['c_done', ''],
    ['', ''],
    ['c_part_fn',    '    def partition(self, arr, low, high):'],
    ['c_pivot_set',  '        pivot = arr[high]'],
    ['c_i_init',     '        i = low - 1'],
    ['c_j_loop',     '        for j in range(low, high):'],
    ['c_check',      '            if arr[j] < pivot:'],
    ['c_i_inc',      '                i += 1'],
    ['c_swap_inner', '                arr[i], arr[j] = arr[j], arr[i]'],
    ['c_pivot_swap', '        arr[i + 1], arr[high] = arr[high], arr[i + 1]'],
    ['c_return_pi',  '        return i + 1']
  ],
  javascript: [
    ['', 'class QuickSort {'],
    ['c_entry', '  quickSort(arr, low, high) {'],
    ['c_base_check', '    if (low < high) {'],
    ['c_part_call', '      let pi = this.partition(arr, low, high);'],
    ['c_rec_left', '      this.quickSort(arr, low, pi - 1);'],
    ['c_rec_right', '      this.quickSort(arr, pi + 1, high);'],
    ['', '    }'],
    ['c_done', '  }'],
    ['', ''],
    ['c_part_fn',    '  partition(arr, low, high) {'],
    ['c_pivot_set',  '    let pivot = arr[high];'],
    ['c_i_init',     '    let i = low - 1;'],
    ['c_j_loop',     '    for (let j = low; j < high; j++) {'],
    ['c_check',      '      if (arr[j] < pivot) {'],
    ['c_i_inc',      '        i++;'],
    ['c_swap_inner', '        [arr[i], arr[j]] = [arr[j], arr[i]];'],
    ['',             '      }'],
    ['',             '    }'],
    ['c_pivot_swap', '    [arr[i + 1], arr[high]] = [arr[high], arr[i + 1]];'],
    ['c_return_pi',  '    return i + 1;'],
    ['',             '  }'],
    ['', '}']
  ]
};

const PSEUDOCODE = [
  'function quickSort(arr, low, high):',
  '    if low < high:',
  '        pi = partition(arr, low, high)',
  '        quickSort(arr, low, pi - 1)    // Recursively sort left subarray',
  '        quickSort(arr, pi + 1, high)   // Recursively sort right subarray',
  '',
  'function partition(arr, low, high):',
  '    pivot = arr[high]',
  '    i = low - 1',
  '    for j = low to high - 1:',
  '        if arr[j] < pivot:',
  '            i = i + 1',
  '            swap(arr[i], arr[j])',
  '    swap(arr[i + 1], arr[high])',
  '    return i + 1',
];

function frame(title, rows) { return { title, rows }; }

function buildSteps(initialValues) {
  const steps = [];
  const arr = [...initialValues];
  const n = arr.length;
  const callStack = [];
  const sortedSet = new Set();

  function pushStep(badge, code, activeRange = null, pivotIdx = -1, iPtr = -1, jPtr = -1, swapPair = []) {
    const l = activeRange ? activeRange[0] : -1;
    const r = activeRange ? activeRange[1] : -1;
    steps.push({
      badge,
      code,
      vars: callStack.map(item => frame(item.title, item.rows)),
      cells: arr.map((val, idx) => {
        if (sortedSet.has(idx)) return { idx, val, state: 'sorted' };
        if (swapPair.includes(idx)) return { idx, val, state: 'swap' };
        if (idx === pivotIdx) return { idx, val, state: 'pivot' };
        if (idx === jPtr) return { idx, val, state: 'j_ptr' };
        if (idx === iPtr) return { idx, val, state: 'i_ptr' };
        if (l >= 0 && r >= 0 && idx >= l && idx <= r) return { idx, val, state: 'cur' };
        return { idx, val, state: 'normal' };
      }),
      low: l,
      high: r,
      pivot: pivotIdx >= 0 ? arr[pivotIdx] : null,
      i: iPtr,
      j: jPtr,
      n
    });
  }

  // Initial step
  callStack.push({ title: 'main()', rows: [] });
  pushStep(`quickSort called on array of size ${n}`, 'c_entry');

  if (n === 0) {
    pushStep('Array is empty (n = 0) → sorting complete', 'c_done');
    return steps;
  }

  function quickSortHelper(low, high) {
    callStack.push({
      title: `quickSort(arr, ${low}, ${high})`,
      rows: [['low', String(low)], ['high', String(high)]]
    });

    pushStep(`quickSort(arr, ${low}, ${high}) called for subarray range [${low}..${high}]`, 'c_entry', [low, high]);

    pushStep(`if (low < high) → checking ${low} < ${high} → ${low < high ? 'TRUE' : 'FALSE'}`, 'c_base_check', [low, high]);

    if (low < high) {
      pushStep(`partition(arr, ${low}, ${high}) initiated`, 'c_part_call', [low, high]);

      // Partition process
      const pivotVal = arr[high];
      const pivotIdx = high;

      pushStep(`int pivot = arr[${high}] → chosen pivot element = ${pivotVal}`, 'c_pivot_set', [low, high], pivotIdx);

      let i = low - 1;
      pushStep(`int i = low - 1 → i initialized to ${i}`, 'c_i_init', [low, high], pivotIdx, i);

      for (let j = low; j < high; j++) {
        pushStep(`for (j = ${j}; j < ${high}; j++) → scanning element arr[${j}] = ${arr[j]}`, 'c_j_loop', [low, high], pivotIdx, i, j);

        const condition = arr[j] < pivotVal;
        pushStep(`if (arr[${j}] < pivot) → checking ${arr[j]} < ${pivotVal} → ${condition ? 'TRUE' : 'FALSE'}`, 'c_check', [low, high], pivotIdx, i, j);

        if (condition) {
          i++;
          pushStep(`i++ → increment i to ${i}`, 'c_i_inc', [low, high], pivotIdx, i, j);

          // Swap arr[i] and arr[j]
          const temp = arr[i];
          arr[i] = arr[j];
          arr[j] = temp;
          pushStep(`swap(arr[${i}], arr[${j}]) → swapped ${arr[j]} and ${arr[i]}`, 'c_swap_inner', [low, high], pivotIdx, i, j, [i, j]);
        }
      }

      // Place pivot at i + 1
      const pi = i + 1;
      pushStep(`Partition scan done. Swapping pivot arr[${high}] (${pivotVal}) into sorted position at index i + 1 (${pi})`, 'c_pivot_swap', [low, high], pivotIdx, i, -1, [pi, high]);

      const tempPivot = arr[pi];
      arr[pi] = arr[high];
      arr[high] = tempPivot;

      sortedSet.add(pi);
      pushStep(`return i + 1 → pivot ${pivotVal} is now in final sorted position at index ${pi}`, 'c_return_pi', [low, high], pi);

      // Recursive calls
      pushStep(`quickSort(arr, ${low}, ${pi - 1}) → sorting left subarray [${low}..${pi - 1}]`, 'c_rec_left', [low, pi - 1]);
      quickSortHelper(low, pi - 1);

      pushStep(`quickSort(arr, ${pi + 1}, ${high}) → sorting right subarray [${pi + 1}..${high}]`, 'c_rec_right', [pi + 1, high]);
      quickSortHelper(pi + 1, high);
    } else if (low === high) {
      sortedSet.add(low);
      pushStep(`Base case: subarray [${low}..${high}] has size 1 → element at index ${low} is sorted`, 'c_base_check', [low, high]);
    }

    callStack.pop();
  }

  quickSortHelper(0, n - 1);

  for (let k = 0; k < n; k++) sortedSet.add(k);
  pushStep(`Quick Sort Complete! Entire array is sorted: [${arr.join(', ')}]`, 'c_done');
  steps[steps.length - 1].cells = arr.map((v, idx) => ({ idx, val: v, state: 'sorted' }));

  return steps;
}

const inpElems = ref('10 80 30 90 40 50 70');
const lang = ref('java');
const speed = ref(650);
const si = ref(0);
const playing = ref(false);
const vizHeight = ref(265);
const tableHeight = ref(60);
const leftWidth = ref(50);
const rightTab = ref('code');

const initialArray = [10, 80, 30, 90, 40, 50, 70];
const stepsData = reactive({ steps: buildSteps(initialArray) });
const steps = computed(() => stepsData.steps);
const s = computed(() => steps.value[Math.max(0, Math.min(si.value, steps.value.length - 1))] || {});
const codeLines = computed(() => CODES[lang.value] || []);

let playTimer = null;

function applyInput() {
  const raw = String(inpElems.value).trim();
  let arr = raw === '' ? [] : raw.split(/[\s,]+/).filter(x => x !== '').map(Number).filter(n => !isNaN(n));
  if (raw !== '' && !arr.length) {
    alert('Enter valid numbers separated by spaces or commas.');
    return;
  }
  inpElems.value = arr.join(' ');

  playing.value = false;
  stepsData.steps = buildSteps(arr);
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
  if (cell.state === 'pivot') tags.push('pivot');
  if (s.value.i === cell.idx) tags.push('i');
  if (s.value.j === cell.idx) tags.push('j');
  if (tags.length) return tags.join(', ') + ' \u2192';
  if (cell.state === 'sorted') return 'sorted';
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
            <label>Elements</label>
            <input type="text" v-model="inpElems" style="width: 220px;" class="ll-text-input" placeholder="e.g. 10 80 30 90" />
            
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
                    <div class="ll-ptr-chip">low = <b class="ll-c-blue">{{ s.low >= 0 ? s.low : 'N/A' }}</b></div>
                    <div class="ll-ptr-chip">high = <b class="ll-c-purple">{{ s.high >= 0 ? s.high : 'N/A' }}</b></div>
                    <div class="ll-ptr-chip">pivot = <b class="ll-c-red">{{ s.pivot !== null ? s.pivot : 'N/A' }}</b></div>
                    <div class="ll-ptr-chip">i = <b class="ll-c-green">{{ s.i >= -1 ? s.i : 'N/A' }}</b></div>
                    <div class="ll-ptr-chip">j = <b class="ll-c-orange">{{ s.j >= 0 ? s.j : 'N/A' }}</b></div>
                  </div>

                  <!-- Visual Diagram - Pastel Flat Cards -->
                  <div class="ll-arr-track">
                    <template v-for="cell in s.cells" :key="cell.idx">
                      <div class="ll-arr-cell-wrap">
                        <div class="ll-ptr-tag-wrap">
                          <span v-if="s.low === cell.idx" class="ll-ptr-lbl ll-lbl-blue">low</span>
                          <span v-if="s.high === cell.idx && cell.state !== 'pivot'" class="ll-ptr-lbl ll-lbl-purple">high</span>
                          <span v-if="cell.state === 'pivot'" class="ll-ptr-lbl ll-lbl-red">pivot</span>
                          <span v-if="s.i === cell.idx" class="ll-ptr-lbl ll-lbl-green">i</span>
                          <span v-if="s.j === cell.idx" class="ll-ptr-lbl ll-lbl-orange">j</span>
                        </div>
                        <div
                          class="ll-arr-box"
                          :class="{
                            'll-box-cur': cell.state === 'cur',
                            'll-box-pivot': cell.state === 'pivot',
                            'll-box-iptr': cell.state === 'i_ptr',
                            'll-box-jptr': cell.state === 'j_ptr',
                            'll-box-swap': cell.state === 'swap',
                            'll-box-sorted': cell.state === 'sorted'
                          }"
                        >
                          {{ cell.val }}
                        </div>
                        <div class="ll-arr-idx">[{{ cell.idx }}]</div>
                      </div>
                    </template>

                    <div v-if="!s.cells || s.cells.length === 0" class="ll-empty-state">
                      Array is empty
                    </div>
                  </div>
                </div>
              </div>

              <div class="ll-vresizer" ref="vizResizerRef"></div>

              <!-- Color Legend -->
              <div class="ll-legend">
                <span class="ll-leg"><span class="ll-legdot ll-legdot-existing"></span>Unsorted</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-cur"></span>Active Range (low..high)</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-pivot"></span>Pivot Element</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-iptr"></span>Pointer i (Smaller Side)</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-jptr"></span>Pointer j (Scanning)</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-sorted"></span>Sorted / Settled</span>
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
                      <tr><td>Best Case</td><td>O(n log n)</td><td>Balanced pivot splits array into equal halves.</td></tr>
                      <tr><td>Average Case</td><td>O(n log n)</td><td>Average partitioning splits dataset in near-equal parts.</td></tr>
                      <tr><td>Worst Case</td><td>O(n²)</td><td>Unbalanced partition (e.g. sorted array with last element pivot).</td></tr>
                      <tr><td>Space</td><td>O(log n)</td><td>Auxiliary recursion call stack space.</td></tr>
                    </tbody>
                  </table>
                  <p class="ll-note">
                    Key observation: Quick Sort picks a <b>pivot</b> element and partitions the array such that all elements smaller than pivot go to the left and larger elements go to the right.
                  </p>
                  <h3>Advantages</h3>
                  <p>In-place sorting algorithm with very small constant factors and excellent practical cache performance.</p>
                  <h3>Limitations</h3>
                  <p>Worst-case time complexity O(n²) when pivot choices are poor (mitigated with random/median pivot choices).</p>
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
.ll-ptr-lbl { font-size: 12px; font-weight: 800; font-family: 'Consolas', 'Fira Code', monospace; display: inline-flex; flex-direction: column; align-items: center; line-height: 1; gap: 1px; white-space: nowrap; animation: ll-pop 0.2s ease; }
.ll-ptr-lbl::after { content: '↓'; font-size: 11px; font-weight: 900; line-height: 1; margin-top: 1px; }
.ll-lbl-blue { color: #3b82f6; }
.ll-lbl-orange { color: #f97316; }
.ll-lbl-purple { color: #9333ea; }
.ll-lbl-green { color: #10b981; }
.ll-lbl-red { color: #ef4444; }
.ll-arr-box { width: 54px; height: 54px; display: flex; align-items: center; justify-content: center; border: 2px solid var(--blue); border-radius: var(--radius); background: #eff6ff; color: #1e293b; font-weight: 700; font-size: 16px; box-shadow: var(--shadow-sm); transition: all 0.25s ease; animation: ll-pop .3s ease; }
.ll-box-cur { border-color: #3b82f6 !important; background: #dbeafe !important; color: #1e40af !important; box-shadow: 0 0 0 3px rgba(59, 130, 246, 0.25) !important; transform: translateY(-2px); }
.ll-box-pivot { border-color: #ef4444 !important; background: #fef2f2 !important; color: #991b1b !important; box-shadow: 0 0 0 3px rgba(239, 68, 68, 0.25) !important; transform: translateY(-4px); }
.ll-box-iptr { border-color: #10b981 !important; background: #dcfce7 !important; color: #065f46 !important; }
.ll-box-jptr { border-color: #f59e0b !important; background: #fef3c7 !important; color: #92400e !important; }
.ll-box-swap { border-color: #ef4444 !important; background: #fef2f2 !important; color: #991b1b !important; box-shadow: 0 0 0 3px rgba(239, 68, 68, 0.25) !important; transform: scale(1.05); }
.ll-box-sorted { border-color: #10b981 !important; background: #dcfce7 !important; color: #065f46 !important; box-shadow: 0 0 0 3px rgba(16, 185, 129, 0.25) !important; }

.ll-arr-idx { font-size: 11px; color: var(--muted); margin-top: 4px; font-family: 'Consolas', monospace; font-weight: 600; }
.ll-empty-state { display: flex; align-items: center; justify-content: center; width: 100%; min-height: 60px; color: var(--muted); font-size: 13px; font-style: italic; }
.ll-vresizer { height: 5px; cursor: row-resize; background: var(--border); flex-shrink: 0; transition: background .15s; position: relative; z-index: 20; }
.ll-vresizer:hover, .ll-vresizer.drag { background: var(--coral); }
.ll-legend { display: flex; flex-wrap: wrap; gap: 6px 14px; padding: 6px 12px; border-bottom: 1px solid var(--border); flex-shrink: 0; background: var(--surface2); }
.ll-leg { display: flex; align-items: center; gap: 5px; font-size: 11px; color: var(--text2); font-weight: 500; }
.ll-legdot { width: 11px; height: 11px; border-radius: 3px; flex-shrink: 0; display: inline-block; }
.ll-legdot-existing { background: #eff6ff; border: 1.5px solid var(--blue); }
.ll-legdot-cur { background: #dbeafe; border: 1.5px solid #3b82f6; }
.ll-legdot-pivot { background: #fef2f2; border: 1.5px solid #ef4444; }
.ll-legdot-iptr { background: #dcfce7; border: 1.5px solid #10b981; }
.ll-legdot-jptr { background: #fef3c7; border: 1.5px solid #f59e0b; }
.ll-legdot-sorted { background: #dcfce7; border: 1.5px solid #10b981; }

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
