<script setup>
import { ref, reactive, computed, onMounted, onBeforeUnmount, watch } from 'vue';

defineProps({
  topic: { type: String, default: 'Heap Sort Algorithm' },
  subTopic: { type: String, default: 'Using Arrays & Binary Heap Tree' }
});

const CODES = {
  java: [
    ['', 'class HeapSort {'],
    ['c_entry', '    void heapSort(int[] arr) {'],
    ['c_n', '        int n = arr.length;'],
    ['c_build_heap', '        for (int i = n / 2 - 1; i >= 0; i--) {'],
    ['c_build_heapify', '            heapify(arr, n, i);'],
    ['', '        }'],
    ['c_extract_loop', '        for (int i = n - 1; i > 0; i--) {'],
    ['c_swap_root', '            int temp = arr[0]; arr[0] = arr[i]; arr[i] = temp;'],
    ['c_heapify_call', '            heapify(arr, i, 0);'],
    ['', '        }'],
    ['c_done', '    }'],
    ['', ''],
    ['c_heapify_fn', '    void heapify(int[] arr, int heapSize, int i) {'],
    ['c_largest_init', '        int largest = i; int l = 2 * i + 1; int r = 2 * i + 2;'],
    ['c_check_left', '        if (l < heapSize && arr[l] > arr[largest]) largest = l;'],
    ['c_check_right', '        if (r < heapSize && arr[r] > arr[largest]) largest = r;'],
    ['c_heapify_swap', '        if (largest != i) {'],
    ['c_heapify_swap', '            int swap = arr[i]; arr[i] = arr[largest]; arr[largest] = swap;'],
    ['c_heapify_rec', '            heapify(arr, heapSize, largest);'],
    ['', '        }'],
    ['', '    }'],
    ['', '}']
  ],
  c: [
    ['c_entry', 'void heapSort(int arr[], int n) {'],
    ['c_build_heap', '    for (int i = n / 2 - 1; i >= 0; i--) {'],
    ['c_build_heapify', '        heapify(arr, n, i);'],
    ['', '    }'],
    ['c_extract_loop', '    for (int i = n - 1; i > 0; i--) {'],
    ['c_swap_root', '        int temp = arr[0]; arr[0] = arr[i]; arr[i] = temp;'],
    ['c_heapify_call', '        heapify(arr, i, 0);'],
    ['', '    }'],
    ['c_done', '}']
  ],
  cpp: [
    ['', 'class HeapSort {'],
    ['', 'public:'],
    ['c_entry', '    void heapSort(vector<int>& arr) {'],
    ['c_n', '        int n = arr.size();'],
    ['c_build_heap', '        for (int i = n / 2 - 1; i >= 0; i--) heapify(arr, n, i);'],
    ['c_extract_loop', '        for (int i = n - 1; i > 0; i--) {'],
    ['c_swap_root', '            swap(arr[0], arr[i]);'],
    ['c_heapify_call', '            heapify(arr, i, 0);'],
    ['', '        }'],
    ['c_done', '    }'],
    ['', '};']
  ],
  python: [
    ['', 'class HeapSort:'],
    ['c_entry', '    def heap_sort(self, arr):'],
    ['c_n', '        n = len(arr)'],
    ['c_build_heap', '        for i in range(n // 2 - 1, -1, -1):'],
    ['c_build_heapify', '            self.heapify(arr, n, i)'],
    ['c_extract_loop', '        for i in range(n - 1, 0, -1):'],
    ['c_swap_root', '            arr[0], arr[i] = arr[i], arr[0]'],
    ['c_heapify_call', '            self.heapify(arr, i, 0)'],
    ['c_done', '']
  ],
  javascript: [
    ['', 'class HeapSort {'],
    ['c_entry', '  heapSort(arr) {'],
    ['c_n', '    let n = arr.length;'],
    ['c_build_heap', '    for (let i = Math.floor(n / 2) - 1; i >= 0; i--) {'],
    ['c_build_heapify', '      this.heapify(arr, n, i);'],
    ['', '    }'],
    ['c_extract_loop', '    for (let i = n - 1; i > 0; i--) {'],
    ['c_swap_root', '      let temp = arr[0]; arr[0] = arr[i]; arr[i] = temp;'],
    ['c_heapify_call', '      this.heapify(arr, i, 0);'],
    ['', '    }'],
    ['c_done', '  }'],
    ['', '}']
  ]
};

const PSEUDOCODE = [
  'function heapSort(arr):',
  '    n = length(arr)',
  '    // Step 1: Build Max-Heap',
  '    for i = floor(n / 2) - 1 down to 0:',
  '        heapify(arr, n, i)',
  '',
  '    // Step 2: Extract elements from Max-Heap',
  '    for i = n - 1 down to 1:',
  '        swap(arr[0], arr[i])   // Move max element to end',
  '        heapify(arr, i, 0)     // Heapify reduced heap',
  '',
  'function heapify(arr, heapSize, i):',
  '    largest = i, left = 2*i + 1, right = 2*i + 2',
  '    if left < heapSize and arr[left] > arr[largest]: largest = left',
  '    if right < heapSize and arr[right] > arr[largest]: largest = right',
  '    if largest != i:',
  '        swap(arr[i], arr[largest])',
  '        heapify(arr, heapSize, largest)',
];

function frame(title, rows) { return { title, rows }; }

function buildSteps(initialValues) {
  const steps = [];
  const arr = [...initialValues];
  const n = arr.length;
  const callStack = [];

  function pushStep(badge, code, heapSize, activeIndices = [], swapPair = [], largestIdx = -1) {
    steps.push({
      badge,
      code,
      vars: callStack.map(item => frame(item.title, item.rows)),
      cells: arr.map((val, idx) => {
        if (idx >= heapSize) return { idx, val, state: 'sorted' };
        if (swapPair.includes(idx)) return { idx, val, state: 'swap' };
        if (idx === largestIdx) return { idx, val, state: 'largest' };
        if (activeIndices.includes(idx)) return { idx, val, state: 'cur' };
        return { idx, val, state: 'normal' };
      }),
      heapSize,
      largestIdx,
      n
    });
  }

  // Initial step
  callStack.push({ title: 'main()', rows: [] });
  pushStep(`heapSort called on array of size ${n}`, 'c_entry', n);

  if (n === 0) {
    pushStep('Array is empty (n = 0) → sorting complete', 'c_done', 0);
    return steps;
  }

  pushStep(`int n = arr.length → n set to ${n}`, 'c_n', n);

  function heapifyHelper(heapSize, i) {
    callStack.push({
      title: `heapify(arr, ${heapSize}, ${i})`,
      rows: [['heapSize', String(heapSize)], ['i', String(i)]]
    });

    pushStep(`heapify called for node index ${i} (val = ${arr[i]}) in heap of size ${heapSize}`, 'c_heapify_fn', heapSize, [i]);

    let largest = i;
    const left = 2 * i + 1;
    const right = 2 * i + 2;

    pushStep(`int largest = ${i}, left = ${left}, right = ${right}`, 'c_largest_init', heapSize, [i], [], largest);

    // Check left
    if (left < heapSize) {
      const compL = arr[left] > arr[largest];
      pushStep(`if (left < heapSize && arr[left] > arr[largest]) → arr[${left}] (${arr[left]}) > arr[${largest}] (${arr[largest]}) → ${compL ? 'TRUE' : 'FALSE'}`, 'c_check_left', heapSize, [i, left], [], largest);
      if (compL) {
        largest = left;
        pushStep(`largest updated to left child index ${left} (${arr[left]})`, 'c_check_left', heapSize, [i, left], [], largest);
      }
    }

    // Check right
    if (right < heapSize) {
      const compR = arr[right] > arr[largest];
      pushStep(`if (right < heapSize && arr[right] > arr[largest]) → arr[${right}] (${arr[right]}) > arr[${largest}] (${arr[largest]}) → ${compR ? 'TRUE' : 'FALSE'}`, 'c_check_right', heapSize, [i, right], [], largest);
      if (compR) {
        largest = right;
        pushStep(`largest updated to right child index ${right} (${arr[right]})`, 'c_check_right', heapSize, [i, right], [], largest);
      }
    }

    if (largest !== i) {
      pushStep(`if (largest != i) → swapping arr[${i}] (${arr[i]}) and arr[${largest}] (${arr[largest]})`, 'c_heapify_swap', heapSize, [i, largest], [i, largest], largest);

      const temp = arr[i];
      arr[i] = arr[largest];
      arr[largest] = temp;

      pushStep(`Swapped arr[${i}] and arr[${largest}]. Re-heapifying subtree at index ${largest}`, 'c_heapify_rec', heapSize, [i, largest], [i, largest], largest);
      heapifyHelper(heapSize, largest);
    } else {
      pushStep(`Subtree at index ${i} already satisfies Max-Heap property`, 'c_heapify_fn', heapSize, [i], [], largest);
    }

    callStack.pop();
  }

  // Step 1: Build Max-Heap
  pushStep(`Building Max-Heap by calling heapify from index ${Math.floor(n / 2) - 1} down to 0`, 'c_build_heap', n);

  for (let i = Math.floor(n / 2) - 1; i >= 0; i--) {
    pushStep(`Build Heap Pass: heapify(arr, ${n}, ${i})`, 'c_build_heapify', n, [i]);
    heapifyHelper(n, i);
  }

  pushStep(`Max-Heap built successfully! Root element arr[0] (${arr[0]}) is the maximum value`, 'c_build_heap', n);

  // Step 2: Extract elements
  for (let i = n - 1; i > 0; i--) {
    pushStep(`Extracting max element arr[0] (${arr[0]}) and swapping with arr[${i}] (${arr[i]})`, 'c_swap_root', i + 1, [0, i], [0, i]);

    const temp = arr[0];
    arr[0] = arr[i];
    arr[i] = temp;

    pushStep(`Element ${temp} placed at final sorted index ${i}. Reduced heap size to ${i}`, 'c_extract_loop', i, [], [], -1);

    pushStep(`Restoring Max-Heap: heapify(arr, ${i}, 0)`, 'c_heapify_call', i, [0]);
    heapifyHelper(i, 0);
  }

  pushStep(`Heap Sort Complete! Entire array is sorted: [${arr.join(', ')}]`, 'c_done', 0);
  steps[steps.length - 1].cells = arr.map((v, idx) => ({ idx, val: v, state: 'sorted' }));

  return steps;
}

const inpElems = ref('1 12 9 5 6 10');
const lang = ref('java');
const speed = ref(650);
const si = ref(0);
const playing = ref(false);
const showTreeModal = ref(false); // Floating Binary Tree Modal toggle (defaults to hidden)
const vizHeight = ref(265);
const tableHeight = ref(60);
const leftWidth = ref(50);
const rightTab = ref('code');

const initialArray = [1, 12, 9, 5, 6, 10];
const stepsData = reactive({ steps: buildSteps(initialArray) });
const steps = computed(() => stepsData.steps);
const s = computed(() => steps.value[Math.max(0, Math.min(si.value, steps.value.length - 1))] || {});
const codeLines = computed(() => CODES[lang.value] || []);

const hoveredNodeIdx = ref(null);
const hoveredParentChildEdge = ref(null);

// ── Binary Heap Tree Geometry Calculation ──
const treeNodes = computed(() => {
  if (!s.value || !s.value.cells) return [];
  const cells = s.value.cells;
  const count = cells.length;
  if (count === 0) return [];

  const width = 720;
  const levelHeight = 90;
  const startY = 60;

  return cells.map((cell) => {
    const k = cell.idx;
    const depth = Math.floor(Math.log2(k + 1));
    const posInLevel = (k + 1) - (1 << depth);
    const numNodesInLevel = 1 << depth;
    const segment = width / numNodesInLevel;
    const x = (posInLevel + 0.5) * segment;
    const y = startY + depth * levelHeight;

    let parentX = 0, parentY = 0, parentIdx = -1;
    if (k > 0) {
      parentIdx = Math.floor((k - 1) / 2);
      const pDepth = Math.floor(Math.log2(parentIdx + 1));
      const pPos = (parentIdx + 1) - (1 << pDepth);
      const pNum = 1 << pDepth;
      parentX = (pPos + 0.5) * (width / pNum);
      parentY = startY + pDepth * levelHeight;
    }

    return {
      idx: k,
      val: cell.val,
      state: cell.state,
      x,
      y,
      parentIdx,
      parentX,
      parentY,
      hasParent: k > 0
    };
  });
});

function isEdgeHovered(pIdx, cIdx) {
  return hoveredParentChildEdge.value &&
    hoveredParentChildEdge.value.parentIdx === pIdx &&
    hoveredParentChildEdge.value.childIdx === cIdx;
}
function isNodeHoveredParent(nodeIdx) {
  return hoveredParentChildEdge.value && hoveredParentChildEdge.value.parentIdx === nodeIdx;
}
function isNodeHoveredChild(nodeIdx) {
  return hoveredParentChildEdge.value && hoveredParentChildEdge.value.childIdx === nodeIdx;
}

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
  clearTimeout(playTimer);
  stepsData.steps = buildSteps(arr);
  si.value = 0;
}

function stepBy(d) {
  si.value = Math.max(0, Math.min(steps.value.length - 1, si.value + d));
}

function togglePlay() {
  playing.value = !playing.value;
  if (playing.value) {
    if (si.value >= steps.value.length - 1) si.value = 0;
    playNext();
  } else {
    clearTimeout(playTimer);
  }
}

function playNext() {
  if (!playing.value) return;
  if (si.value < steps.value.length - 1) {
    si.value++;
    playTimer = setTimeout(playNext, 2100 - speed.value);
  } else {
    playing.value = false;
  }
}

function cellLabel(cell) {
  const tags = [];
  if (cell.idx === 0) tags.push('root');
  if (s.value.largestIdx === cell.idx) tags.push('largest');
  if (tags.length) return tags.join(', ') + ' \u2192';
  if (cell.state === 'sorted') return 'sorted';
  return '';
}

function nodeColor(state) {
  if (state === 'sorted') return '#10b981';
  if (state === 'swap') return '#ef4444';
  if (state === 'largest') return '#f59e0b';
  if (state === 'cur') return '#3b82f6';
  return '#38bdf8';
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
            <input type="text" v-model="inpElems" style="width: 200px;" class="ll-text-input" placeholder="e.g. 1 12 9 5 6 10" />
            
            <button class="ll-viz-btn" @click="applyInput">&#9654; Visualize</button>
            <button class="ll-graph-modal-btn" @click="showTreeModal = !showTreeModal">
              {{ showTreeModal ? 'Hide' : 'Show' }}
            </button>

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
                    <div class="ll-ptr-chip">heapSize = <b class="ll-c-blue">{{ s.heapSize !== undefined ? s.heapSize : 'N/A' }}</b></div>
                    <div class="ll-ptr-chip">largest = <b class="ll-c-orange">{{ s.largestIdx >= 0 ? s.largestIdx : 'N/A' }}</b></div>
                    <div class="ll-ptr-chip">n = <b class="ll-c-purple">{{ s.n }}</b></div>
                  </div>

                  <!-- Visual Diagram - Array Representation -->
                  <div class="ll-arr-track">
                    <template v-for="cell in s.cells" :key="cell.idx">
                      <div
                        class="ll-arr-cell-wrap"
                        @mouseenter="hoveredNodeIdx = cell.idx"
                        @mouseleave="hoveredNodeIdx = null"
                        style="cursor: pointer;"
                      >
                        <div
                          class="ll-who"
                          :class="{
                            'll-c-orange': cell.state === 'largest' || cell.state === 'cur',
                            'll-c-red': cell.state === 'swap',
                            'll-c-green': cell.state === 'sorted'
                          }"
                        >
                          {{ cellLabel(cell) }}
                        </div>
                        <div
                          class="ll-arr-box"
                          :class="{
                            'll-box-cur': cell.state === 'cur',
                            'll-box-largest': cell.state === 'largest',
                            'll-box-swap': cell.state === 'swap',
                            'll-box-sorted': cell.state === 'sorted',
                            'll-box-hovered': hoveredNodeIdx === cell.idx
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
                <span class="ll-leg"><span class="ll-legdot ll-legdot-existing"></span>In Heap</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-cur"></span>Inspecting / Heapifying</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-largest"></span>Largest Element</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-swap"></span>Swapping</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-sorted"></span>Extracted / Sorted</span>
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
                      <tr><td>Best Case</td><td>O(n log n)</td><td>Max-Heap construction O(n) + n extractions of O(log n) each.</td></tr>
                      <tr><td>Average Case</td><td>O(n log n)</td><td>Each of the n elements is heapified through log₂ n tree levels.</td></tr>
                      <tr><td>Worst Case</td><td>O(n log n)</td><td>Guaranteed O(n log n) time performance regardless of input data order.</td></tr>
                      <tr><td>Space</td><td>O(1)</td><td>In-place sorting algorithm using constant auxiliary memory.</td></tr>
                    </tbody>
                  </table>
                  <p class="ll-note">
                    Key observation: <b>Heap Sort</b> visualizes an array as a Complete Binary Tree. Parent index <code>i</code> has left child <code>2*i + 1</code> and right child <code>2*i + 2</code>.
                  </p>
                  <h3>Advantages</h3>
                  <p>Guaranteed O(n log n) worst-case time complexity with O(1) auxiliary space (unlike Merge Sort which requires O(n) space).</p>
                  <h3>Limitations</h3>
                  <p>Unstable sort with poorer cache locality compared to Quick Sort due to distant memory index jumps.</p>
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

    <!-- Floating Binary Heap Tree Modal Dialog Container -->
    <div v-if="showTreeModal" class="tree-modal-backdrop" @click.self="showTreeModal = false">
      <div class="tree-modal-card">
        <div class="tree-modal-header">
          <div class="tree-modal-title">
            <span>Binary Heap Tree View</span>
            <span class="tree-subtitle">(Synchronized with Array)</span>
          </div>
          <button class="tree-close-btn" @click="showTreeModal = false" title="Close modal">&times;</button>
        </div>

        <div class="tree-modal-body">
          <svg class="tree-svg" viewBox="0 0 720 440">
            <g v-if="!treeNodes || treeNodes.length === 0">
              <text x="360" y="220" text-anchor="middle" font-size="16" font-weight="600" fill="#94a3b8" font-family="sans-serif">
                No tree to display. Please enter valid elements (n &gt; 0).
              </text>
            </g>
            <template v-else>
              <!-- Connecting Edges between Parent and Children -->
              <g class="tree-edges">
                <template v-for="node in treeNodes" :key="'edge-' + node.idx">
                  <g v-if="node.hasParent"
                     @mouseenter="hoveredParentChildEdge = { parentIdx: node.parentIdx, childIdx: node.idx }"
                     @mouseleave="hoveredParentChildEdge = null"
                     style="cursor: pointer;"
                  >
                    <!-- Invisible Hit Target Path for Smooth Hovering -->
                    <line
                      :x1="node.parentX"
                      :y1="node.parentY"
                      :x2="node.x"
                      :y2="node.y"
                      stroke="transparent"
                      stroke-width="16"
                      stroke-linecap="round"
                    />
                    <!-- Visible Edge Line -->
                    <line
                      :x1="node.parentX"
                      :y1="node.parentY"
                      :x2="node.x"
                      :y2="node.y"
                      class="tree-edge-line"
                      :class="{ 'tree-edge-hovered': isEdgeHovered(node.parentIdx, node.idx) }"
                      stroke-linecap="round"
                    />
                  </g>
                </template>
              </g>

              <!-- Binary Heap Nodes -->
              <g class="tree-nodes">
                <g
                  v-for="node in treeNodes"
                  :key="'node-' + node.idx"
                  class="tree-node-group"
                  @mouseenter="hoveredNodeIdx = node.idx"
                  @mouseleave="hoveredNodeIdx = null"
                  style="cursor: pointer;"
                >
                  <!-- Pointer Labels above Nodes -->
                  <g v-if="node.idx === 0 || s.largestIdx === node.idx || isNodeHoveredParent(node.idx) || isNodeHoveredChild(node.idx)">
                    <text
                      :x="node.x"
                      :y="node.y - 28"
                      class="tree-svg-ptr"
                      :class="isNodeHoveredParent(node.idx) ? 'tree-ptr-orange' : (isNodeHoveredChild(node.idx) ? 'tree-ptr-green' : (node.idx === s.largestIdx ? 'tree-ptr-orange' : 'tree-ptr-blue'))"
                    >
                      {{ isNodeHoveredParent(node.idx) ? 'parent' : (isNodeHoveredChild(node.idx) ? 'child' : (node.idx === 0 ? 'root' : 'largest')) }}
                    </text>
                    <text
                      :x="node.x"
                      :y="node.y - 17"
                      class="tree-svg-ptr"
                      :class="isNodeHoveredParent(node.idx) ? 'tree-ptr-orange' : (isNodeHoveredChild(node.idx) ? 'tree-ptr-green' : (node.idx === s.largestIdx ? 'tree-ptr-orange' : 'tree-ptr-blue'))"
                    >
                      &darr;
                    </text>
                  </g>

                  <!-- Circle Node -->
                  <circle
                    :cx="node.x"
                    :cy="node.y"
                    r="22"
                    class="tree-node-circle"
                    :class="{
                      'tree-node-cur': node.state === 'cur',
                      'tree-node-largest': node.state === 'largest',
                      'tree-node-swap': node.state === 'swap',
                      'tree-node-sorted': node.state === 'sorted',
                      'tree-node-hover-src': isNodeHoveredParent(node.idx),
                      'tree-node-hover-tgt': isNodeHoveredChild(node.idx),
                      'tree-node-hovered': hoveredNodeIdx === node.idx
                    }"
                  />

                  <!-- Node Value inside Circle -->
                  <text
                    :x="node.x"
                    :y="node.y + 5"
                    class="tree-node-text"
                    :class="{
                      'tree-text-hover-src': isNodeHoveredParent(node.idx),
                      'tree-text-hover-tgt': isNodeHoveredChild(node.idx)
                    }"
                  >
                    {{ node.val }}
                  </text>
                  <text :x="node.x" :y="node.y + 36" class="tree-node-sub-idx">[{{ node.idx }}]</text>
                </g>
              </g>
            </template>
          </svg>
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
.ll-text-input { width: 200px; padding: 5px 10px; }
.ll-num-input:focus, .ll-text-input:focus { outline: none; border-color: var(--coral); box-shadow: 0 0 0 3px rgba(240,77,77,.1); }
.ll-viz-btn { background: var(--coral); color: #fff; border: none; padding: 6px 16px; border-radius: var(--radius-sm); cursor: pointer; font-size: 12px; font-weight: 600; box-shadow: var(--shadow-sm); transition: filter .15s; }
.ll-viz-btn:hover { filter: brightness(1.08); }
.ll-tree-modal-btn { background: #eff6ff; border: 1px solid #3b82f6; color: #1d4ed8; padding: 5px 12px; border-radius: var(--radius-sm); cursor: pointer; font-size: 12px; font-weight: 600; transition: all .15s; }
.ll-tree-modal-btn:hover { background: #3b82f6; color: #fff; }

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
.ll-who { font-size: 11px; font-weight: 700; height: 16px; margin-bottom: 2px; text-align: center; font-family: monospace; }
.ll-arr-box { width: 54px; height: 54px; display: flex; align-items: center; justify-content: center; border: 2px solid var(--blue); border-radius: var(--radius); background: #eff6ff; color: #1e293b; font-weight: 700; font-size: 16px; box-shadow: var(--shadow-sm); transition: all 0.25s ease; animation: ll-pop .3s ease; }
.ll-box-cur { border-color: #3b82f6 !important; background: #dbeafe !important; color: #1e40af !important; box-shadow: 0 0 0 3px rgba(59, 130, 246, 0.25) !important; transform: translateY(-2px); }
.ll-box-largest { border-color: #f59e0b !important; background: #fef3c7 !important; color: #92400e !important; box-shadow: 0 0 0 3px rgba(245, 158, 11, 0.25) !important; transform: translateY(-3px); }
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
.ll-legdot-largest { background: #fef3c7; border: 1.5px solid #f59e0b; }
.ll-legdot-swap { background: #fef2f2; border: 1.5px solid #ef4444; }
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

.ll-graph-modal-btn { background: var(--purple); color: #fff; border: none; padding: 5px 12px; border-radius: var(--radius-sm); cursor: pointer; font-size: 12px; font-weight: 600; box-shadow: var(--shadow-sm); transition: filter .15s; }
.ll-graph-modal-btn:hover { filter: brightness(1.08); }

.ll-box-hovered {
  background: #eff6ff !important;
  border: 2px solid #3b82f6 !important;
  color: #1d4ed8 !important;
  font-weight: 900 !important;
  transform: scale(1.06);
}

/* Floating Binary Tree Modal Container Styles - Synchronized with Graph Visualization */
.tree-modal-backdrop {
  position: fixed;
  top: 0; left: 0; right: 0; bottom: 55px;
  background: rgba(15, 23, 42, 0.55);
  backdrop-filter: blur(3px);
  z-index: 9999;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 16px 24px 20px 24px;
  height:100%;
}
.tree-modal-card {
  background: #ffffff;
  border-radius: 12px;
  box-shadow: 0 20px 25px -5px rgba(0,0,0,0.15), 0 10px 10px -5px rgba(0,0,0,0.04);
  width: 740px;
  max-width: 95vw;
  max-height: calc(100vh - 120px);
  display: flex;
  flex-direction: column;
  overflow: hidden;
  border: 1px solid var(--border);
  animation: ll-pop 0.25s ease-out;
}
.tree-modal-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 12px 18px;
  background: #f8fafc;
  border-bottom: 1px solid var(--border);
}
.tree-modal-title {
  font-size: 14px;
  font-weight: 700;
  color: #1e293b;
  display: flex;
  align-items: center;
  gap: 8px;
}
.tree-subtitle {
  font-size: 11px;
  font-weight: 500;
  color: #64748b;
}
.tree-close-btn {
  background: none;
  border: none;
  font-size: 22px;
  color: #64748b;
  cursor: pointer;
  line-height: 1;
  padding: 0 4px;
  border-radius: 4px;
  transition: color 0.15s;
}
.tree-close-btn:hover { color: #ef4444; }
.tree-modal-body {
  padding: 12px 16px;
  display: flex;
  justify-content: center;
  background: #ffffff;
  overflow: auto;
}
.tree-svg {
  width: 100%;
  max-height: 390px;
}

/* Tree Edges & Nodes Styling */
.tree-edge-line {
  stroke: #cbd5e1;
  stroke-width: 2.5px;
  transition: all 0.15s ease;
}
.tree-edge-line.tree-edge-hovered {
  stroke: #3b82f6 !important;
  stroke-width: 4px !important;
  filter: drop-shadow(0 0 6px rgba(59, 130, 246, 0.6));
}

.tree-node-circle {
  fill: #eff6ff;
  stroke: #3b82f6;
  stroke-width: 2.5px;
  transition: all 0.2s ease;
}
.tree-node-cur { fill: #dbeafe !important; stroke: #3b82f6 !important; stroke-width: 3.5px !important; }
.tree-node-largest { fill: #fff7ed !important; stroke: #f97316 !important; stroke-width: 3.5px !important; }
.tree-node-swap { fill: #fef2f2 !important; stroke: #ef4444 !important; stroke-width: 3.5px !important; }
.tree-node-sorted { fill: #f0fdf4 !important; stroke: #22c55e !important; stroke-width: 2.5px !important; }

/* Node Hover Highlights */
.tree-node-hover-src {
  fill: #fff7ed !important;
  stroke: #f97316 !important;
  stroke-width: 3.5px !important;
  filter: drop-shadow(0 0 8px rgba(249, 115, 22, 0.55));
}
.tree-node-hover-tgt {
  fill: #f0fdf4 !important;
  stroke: #22c55e !important;
  stroke-width: 3.5px !important;
  filter: drop-shadow(0 0 8px rgba(34, 197, 94, 0.55));
}
.tree-node-hovered {
  stroke-width: 3.5px !important;
  filter: drop-shadow(0 0 8px rgba(59, 130, 246, 0.55));
}

.tree-node-text {
  font-size: 14px;
  font-weight: 800;
  font-family: monospace;
  text-anchor: middle;
  fill: #1e293b;
}
.tree-text-hover-src { fill: #c2410c !important; font-weight: 900 !important; }
.tree-text-hover-tgt { fill: #15803d !important; font-weight: 900 !important; }

.tree-node-sub-idx {
  font-size: 11px;
  font-weight: 700;
  font-family: monospace;
  text-anchor: middle;
  fill: #64748b;
}

.tree-svg-ptr {
  font-size: 12px;
  font-weight: 800;
  font-family: monospace;
  text-anchor: middle;
}
.tree-ptr-blue { fill: #3b82f6; }
.tree-ptr-orange { fill: #ea580c; }
.tree-ptr-green { fill: #16a34a; }

@media (max-width: 900px) {
  .ll-main { flex-direction: column; }
  .ll-left-col, .ll-right-col { max-width: 100% !important; width: 100% !important; }
  .ll-resizer { display: none; }
  .ll-toolbar { flex-direction: column; align-items: stretch; }
  .ll-nav-controls { margin-left: 0; justify-content: center; }
}
</style>
