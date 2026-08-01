<script setup>
import { ref, reactive, computed, onMounted, onBeforeUnmount, watch } from 'vue';

defineProps({
  topic: { type: String, default: 'Merge Sort Algorithm' },
  subTopic: { type: String, default: 'Using Arrays (Divide and Conquer)' }
});

const CODES = {
  java: [
    ['', 'class MergeSort {'],
    ['c_entry', '    void mergeSort(int[] arr, int l, int r) {'],
    ['c_base_check', '        if (l < r) {'],
    ['c_mid', '            int m = l + (r - l) / 2;'],
    ['c_rec_left', '            mergeSort(arr, l, m);'],
    ['c_rec_right', '            mergeSort(arr, m + 1, r);'],
    ['c_merge_call', '            merge(arr, l, m, r);'],
    ['', '        }'],
    ['c_done', '    }'],
    ['', ''],
    ['c_merge_fn',    '    void merge(int[] arr, int l, int m, int r) {'],
    ['c_n1_n2',       '        int n1 = m - l + 1, n2 = r - m;'],
    ['c_alloc',       '        int[] L = new int[n1], R = new int[n2];'],
    ['c_copy_l',      '        for (int i = 0; i < n1; i++) L[i] = arr[l + i];'],
    ['c_copy_r',      '        for (int j = 0; j < n2; j++) R[j] = arr[m + 1 + j];'],
    ['c_ijk_init',    '        int i = 0, j = 0, k = l;'],
    ['c_merge_while', '        while (i < n1 && j < n2) {'],
    ['c_merge_if_l',  '            if (L[i] <= R[j]) arr[k++] = L[i++];'],
    ['c_merge_else_r','            else             arr[k++] = R[j++];'],
    ['',              '        }'],
    ['c_remain_l',    '        while (i < n1) arr[k++] = L[i++];'],
    ['c_remain_r',    '        while (j < n2)  arr[k++] = R[j++];'],
    ['',              '    }'],
    ['',              '}']
  ],
  c: [
    ['c_entry', 'void mergeSort(int arr[], int l, int r) {'],
    ['c_base_check', '    if (l < r) {'],
    ['c_mid', '        int m = l + (r - l) / 2;'],
    ['c_rec_left', '        mergeSort(arr, l, m);'],
    ['c_rec_right', '        mergeSort(arr, m + 1, r);'],
    ['c_merge_call', '        merge(arr, l, m, r);'],
    ['', '    }'],
    ['c_done', '}'],
    ['', ''],
    ['c_merge_fn',    'void merge(int arr[], int l, int m, int r) {'],
    ['c_n1_n2',       '    int n1 = m - l + 1, n2 = r - m;'],
    ['c_alloc',       '    int *L = malloc(n1 * sizeof(int));'],
    ['c_alloc',       '    int *R = malloc(n2 * sizeof(int));'],
    ['c_copy_l',      '    for (int i = 0; i < n1; i++) L[i] = arr[l + i];'],
    ['c_copy_r',      '    for (int j = 0; j < n2; j++) R[j] = arr[m + 1 + j];'],
    ['c_ijk_init',    '    int i = 0, j = 0, k = l;'],
    ['c_merge_while', '    while (i < n1 && j < n2) {'],
    ['c_merge_if_l',  '        if (L[i] <= R[j]) arr[k++] = L[i++];'],
    ['c_merge_else_r','        else             arr[k++] = R[j++];'],
    ['',              '    }'],
    ['c_remain_l',    '    while (i < n1) arr[k++] = L[i++];'],
    ['c_remain_r',    '    while (j < n2) arr[k++] = R[j++];'],
    ['',              '    free(L); free(R);'],
    ['',              '}']
  ],
  cpp: [
    ['', 'class MergeSort {'],
    ['', 'public:'],
    ['c_entry', '    void mergeSort(vector<int>& arr, int l, int r) {'],
    ['c_base_check', '        if (l < r) {'],
    ['c_mid', '            int m = l + (r - l) / 2;'],
    ['c_rec_left', '            mergeSort(arr, l, m);'],
    ['c_rec_right', '            mergeSort(arr, m + 1, r);'],
    ['c_merge_call', '            merge(arr, l, m, r);'],
    ['', '        }'],
    ['c_done', '    }'],
    ['', ''],
    ['c_merge_fn',    '    void merge(vector<int>& arr, int l, int m, int r) {'],
    ['c_n1_n2',       '        int n1 = m - l + 1, n2 = r - m;'],
    ['c_alloc',       '        vector<int> L(arr.begin() + l, arr.begin() + m + 1);'],
    ['c_alloc',       '        vector<int> R(arr.begin() + m + 1, arr.begin() + r + 1);'],
    ['c_ijk_init',    '        int i = 0, j = 0, k = l;'],
    ['c_merge_while', '        while (i < n1 && j < n2) {'],
    ['c_merge_if_l',  '            if (L[i] <= R[j]) arr[k++] = L[i++];'],
    ['c_merge_else_r','            else             arr[k++] = R[j++];'],
    ['',              '        }'],
    ['c_remain_l',    '        while (i < n1) arr[k++] = L[i++];'],
    ['c_remain_r',    '        while (j < n2)  arr[k++] = R[j++];'],
    ['',              '    }'],
    ['', '};']
  ],
  python: [
    ['', 'class MergeSort:'],
    ['c_entry', '    def merge_sort(self, arr, l, r):'],
    ['c_base_check', '        if l < r:'],
    ['c_mid', '            m = (l + r) // 2'],
    ['c_rec_left', '            self.merge_sort(arr, l, m)'],
    ['c_rec_right', '            self.merge_sort(arr, m + 1, r)'],
    ['c_merge_call', '            self.merge(arr, l, m, r)'],
    ['c_done', ''],
    ['', ''],
    ['c_merge_fn',    '    def merge(self, arr, l, m, r):'],
    ['c_n1_n2',       '        n1 = m - l + 1'],
    ['c_n1_n2',       '        n2 = r - m'],
    ['c_copy_l',      '        L = arr[l : m + 1]'],
    ['c_copy_r',      '        R = arr[m + 1 : r + 1]'],
    ['c_ijk_init',    '        i = j = 0'],
    ['c_ijk_init',    '        k = l'],
    ['c_merge_while', '        while i < n1 and j < n2:'],
    ['c_merge_if_l',  '            if L[i] <= R[j]:'],
    ['c_merge_if_l',  '                arr[k] = L[i]; i += 1'],
    ['c_merge_else_r','            else:'],
    ['c_merge_else_r','                arr[k] = R[j]; j += 1'],
    ['',              '            k += 1'],
    ['c_remain_l',    '        while i < n1:'],
    ['c_remain_l',    '            arr[k] = L[i]; i += 1; k += 1'],
    ['c_remain_r',    '        while j < n2:'],
    ['c_remain_r',    '            arr[k] = R[j]; j += 1; k += 1']
  ],
  javascript: [
    ['', 'class MergeSort {'],
    ['c_entry', '  mergeSort(arr, l, r) {'],
    ['c_base_check', '    if (l < r) {'],
    ['c_mid', '      let m = Math.floor((l + r) / 2);'],
    ['c_rec_left', '      this.mergeSort(arr, l, m);'],
    ['c_rec_right', '      this.mergeSort(arr, m + 1, r);'],
    ['c_merge_call', '      this.merge(arr, l, m, r);'],
    ['', '    }'],
    ['c_done', '  }'],
    ['', ''],
    ['c_merge_fn',    '  merge(arr, l, m, r) {'],
    ['c_n1_n2',       '    const n1 = m - l + 1, n2 = r - m;'],
    ['c_copy_l',      '    const L = arr.slice(l, m + 1);'],
    ['c_copy_r',      '    const R = arr.slice(m + 1, r + 1);'],
    ['c_ijk_init',    '    let i = 0, j = 0, k = l;'],
    ['c_merge_while', '    while (i < n1 && j < n2) {'],
    ['c_merge_if_l',  '      if (L[i] <= R[j]) arr[k++] = L[i++];'],
    ['c_merge_else_r','      else             arr[k++] = R[j++];'],
    ['',              '    }'],
    ['c_remain_l',    '    while (i < n1) arr[k++] = L[i++];'],
    ['c_remain_r',    '    while (j < n2)  arr[k++] = R[j++];'],
    ['',              '  }'],
    ['', '}']
  ]
};

const PSEUDOCODE = [
  'function mergeSort(arr, l, r):',
  '    if l < r:',
  '        m = (l + r) / 2',
  '        mergeSort(arr, l, m)          // Sort left half',
  '        mergeSort(arr, m+1, r)        // Sort right half',
  '        merge(arr, l, m, r)           // Merge sorted halves',
  '',
  'function merge(arr, l, m, r):',
  '    n1 = m-l+1;  n2 = r-m',
  '    L[0..n1-1] = arr[l..m]            // Copy left subarray',
  '    R[0..n2-1] = arr[m+1..r]          // Copy right subarray',
  '    i = 0;  j = 0;  k = l',
  '    while i < n1 and j < n2:',
  "        if L[i] <= R[j]: arr[k++] = L[i++]",
  '        else:             arr[k++] = R[j++]',
  '    while i < n1: arr[k++] = L[i++]  // Copy remaining left',
  '    while j < n2: arr[k++] = R[j++]  // Copy remaining right',
];

function frame(title, rows) { return { title, rows }; }

function buildSteps(initialValues) {
  const steps = [];
  const arr = [...initialValues];
  const n = arr.length;
  const callStack = [];

  function pushStep(badge, code, opts = {}) {
    const {
      activeRange = null, midIdx = -1, mergingIndices = [],
      mi = -1, mj = -1, mk = -1, n1 = -1, n2 = -1
    } = opts;
    const l = activeRange ? activeRange[0] : -1;
    const r = activeRange ? activeRange[1] : -1;
    steps.push({
      badge, code,
      vars: callStack.map(item => frame(item.title, item.rows)),
      cells: arr.map((val, idx) => {
        if (mergingIndices.includes(idx)) return { idx, val, state: 'swap' };
        if (idx === midIdx) return { idx, val, state: 'mid' };
        if (l >= 0 && r >= 0 && idx >= l && idx <= r) return { idx, val, state: 'cur' };
        return { idx, val, state: 'normal' };
      }),
      left: l, right: r, mid: midIdx,
      mi, mj, mk, n1, n2, n
    });
  }

  function updateTopFrame(rows) {
    if (callStack.length > 0) callStack[callStack.length - 1].rows = rows;
  }

  callStack.push({ title: 'main()', rows: [] });
  pushStep(`mergeSort called on array of size ${n}`, 'c_entry');

  if (n === 0) {
    pushStep('Array is empty (n = 0) → sorting complete', 'c_done');
    return steps;
  }

  function mergeSortHelper(l, r) {
    callStack.push({ title: `mergeSort(${l},${r})`, rows: [['l', String(l)], ['r', String(r)]] });

    pushStep(`mergeSort(arr, ${l}, ${r}) → entering for subarray [${l}..${r}]`,
      'c_entry', { activeRange: [l, r] });

    pushStep(`if (l < r) → ${l} < ${r} → ${l < r ? 'TRUE' : 'FALSE (base case)'}`,
      'c_base_check', { activeRange: [l, r] });

    if (l < r) {
      const m = Math.floor((l + r) / 2);
      updateTopFrame([['l', String(l)], ['r', String(r)], ['m', String(m)]]);
      pushStep(`int m = l + (r - l) / 2 = ${l} + (${r} - ${l}) / 2 = ${m}`,
        'c_mid', { activeRange: [l, r], midIdx: m });

      pushStep(`mergeSort(arr, ${l}, ${m}) → recurse on left half [${l}..${m}]`,
        'c_rec_left', { activeRange: [l, m], midIdx: m });
      mergeSortHelper(l, m);

      pushStep(`mergeSort(arr, ${m + 1}, ${r}) → recurse on right half [${m + 1}..${r}]`,
        'c_rec_right', { activeRange: [m + 1, r], midIdx: m });
      mergeSortHelper(m + 1, r);

      pushStep(`merge(arr, ${l}, ${m}, ${r}) → merging [${l}..${m}] and [${m + 1}..${r}]`,
        'c_merge_call', { activeRange: [l, r], midIdx: m });

      // ── enter merge function ──
      callStack.push({ title: `merge(${l},${m},${r})`, rows: [['l', String(l)], ['m', String(m)], ['r', String(r)]] });
      pushStep(`void merge(l=${l}, m=${m}, r=${r}) → entering merge function`,
        'c_merge_fn', { activeRange: [l, r], midIdx: m });

      const n1 = m - l + 1;
      const n2 = r - m;
      updateTopFrame([['l', String(l)], ['m', String(m)], ['r', String(r)], ['n1', String(n1)], ['n2', String(n2)]]);
      pushStep(`n1 = m-l+1 = ${m}-${l}+1 = ${n1};  n2 = r-m = ${r}-${m} = ${n2}`,
        'c_n1_n2', { activeRange: [l, r], midIdx: m, n1, n2 });

      pushStep(`int[] L = new int[${n1}], R = new int[${n2}] → allocate temp arrays`,
        'c_alloc', { activeRange: [l, r], midIdx: m, n1, n2 });

      // Copy left subarray element by element
      const L = [];
      for (let ci = 0; ci < n1; ci++) {
        L.push(arr[l + ci]);
        updateTopFrame([['l', String(l)], ['m', String(m)], ['r', String(r)], ['n1', String(n1)], ['n2', String(n2)], ['i', String(ci)]]);
        pushStep(`L[${ci}] = arr[${l + ci}] = ${arr[l + ci]}  (copy left element ${ci + 1}/${n1})`,
          'c_copy_l', { activeRange: [l, r], midIdx: m, n1, n2, mi: ci, mergingIndices: [l + ci] });
      }

      // Copy right subarray element by element
      const R = [];
      for (let cj = 0; cj < n2; cj++) {
        R.push(arr[m + 1 + cj]);
        updateTopFrame([['l', String(l)], ['m', String(m)], ['r', String(r)], ['n1', String(n1)], ['n2', String(n2)], ['j', String(cj)]]);
        pushStep(`R[${cj}] = arr[${m + 1 + cj}] = ${arr[m + 1 + cj]}  (copy right element ${cj + 1}/${n2})`,
          'c_copy_r', { activeRange: [l, r], midIdx: m, n1, n2, mj: cj, mergingIndices: [m + 1 + cj] });
      }

      // Initialize merge pointers
      let i = 0, j = 0, k = l;
      updateTopFrame([['l', String(l)], ['m', String(m)], ['r', String(r)], ['i', '0'], ['j', '0'], ['k', String(l)]]);
      pushStep(`int i = 0, j = 0, k = ${l} → initialize merge pointers (i=L index, j=R index, k=output)`,
        'c_ijk_init', { activeRange: [l, r], midIdx: m, n1, n2, mi: 0, mj: 0, mk: l });

      // Main merge while loop
      while (true) {
        const condOk = i < n1 && j < n2;
        updateTopFrame([['l', String(l)], ['m', String(m)], ['r', String(r)], ['i', String(i)], ['j', String(j)], ['k', String(k)]]);
        pushStep(
          `while (i<n1 && j<n2): i=${i}<${n1} && j=${j}<${n2} → ${condOk ? 'TRUE' : 'FALSE, exit loop'}`,
          'c_merge_while',
          { activeRange: [l, r], midIdx: m, n1, n2, mi: i, mj: j, mk: k }
        );
        if (!condOk) break;

        const comp = L[i] <= R[j];
        if (comp) {
          arr[k] = L[i];
          updateTopFrame([['l', String(l)], ['m', String(m)], ['r', String(r)], ['i', String(i)], ['j', String(j)], ['k', String(k)]]);
          pushStep(`L[${i}]=${L[i]} \u2264 R[${j}]=${R[j]} \u2192 arr[${k}]=L[${i}]=${L[i]}, i++, k++`,
            'c_merge_if_l', { activeRange: [l, r], midIdx: m, n1, n2, mi: i, mj: j, mk: k, mergingIndices: [k] });
          i++;
        } else {
          arr[k] = R[j];
          updateTopFrame([['l', String(l)], ['m', String(m)], ['r', String(r)], ['i', String(i)], ['j', String(j)], ['k', String(k)]]);
          pushStep(`L[${i}]=${L[i]} > R[${j}]=${R[j]} \u2192 arr[${k}]=R[${j}]=${R[j]}, j++, k++`,
            'c_merge_else_r', { activeRange: [l, r], midIdx: m, n1, n2, mi: i, mj: j, mk: k, mergingIndices: [k] });
          j++;
        }
        k++;
      }

      // Copy remaining left elements
      while (i < n1) {
        arr[k] = L[i];
        updateTopFrame([['l', String(l)], ['m', String(m)], ['r', String(r)], ['i', String(i)], ['j', String(j)], ['k', String(k)]]);
        pushStep(`while(i<n1): arr[${k}] = L[${i}] = ${L[i]} \u2192 copy remaining left element`,
          'c_remain_l', { activeRange: [l, r], midIdx: m, n1, n2, mi: i, mj: j, mk: k, mergingIndices: [k] });
        i++; k++;
      }

      // Copy remaining right elements
      while (j < n2) {
        arr[k] = R[j];
        updateTopFrame([['l', String(l)], ['m', String(m)], ['r', String(r)], ['i', String(i)], ['j', String(j)], ['k', String(k)]]);
        pushStep(`while(j<n2): arr[${k}] = R[${j}] = ${R[j]} \u2192 copy remaining right element`,
          'c_remain_r', { activeRange: [l, r], midIdx: m, n1, n2, mi: i, mj: j, mk: k, mergingIndices: [k] });
        j++; k++;
      }

      callStack.pop(); // pop merge frame

      pushStep(`merge(${l},${m},${r}) complete \u2192 [${l}..${r}] sorted: [${arr.slice(l, r + 1).join(', ')}]`,
        'c_merge_call', { activeRange: [l, r], midIdx: m });

    } else {
      pushStep(`Base case: l = r = ${l}, single element (${arr[l]}) \u2192 already sorted`,
        'c_base_check', { activeRange: [l, r] });
    }

    callStack.pop(); // pop mergeSort frame
  }

  mergeSortHelper(0, n - 1);

  pushStep(`Merge Sort Complete! Array sorted: [${arr.join(', ')}]`, 'c_done');
  steps[steps.length - 1].cells = arr.map((v, idx) => ({ idx, val: v, state: 'sorted' }));

  return steps;
}

const inpElems = ref('38 27 43 3 9 82 10');
const lang = ref('java');
const speed = ref(650);
const si = ref(0);
const playing = ref(false);
const vizHeight = ref(265);
const tableHeight = ref(60);
const leftWidth = ref(50);
const rightTab = ref('code');

const initialArray = [38, 27, 43, 3, 9, 82, 10];
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
  if (s.value.left === cell.idx) tags.push('left');
  if (s.value.mid === cell.idx) tags.push('mid');
  if (s.value.right === cell.idx) tags.push('right');
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
            <input type="text" v-model="inpElems" style="width: 220px;" class="ll-text-input" placeholder="e.g. 38 27 43 3 9" />
            
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
                    <div class="ll-ptr-chip">mid = <b class="ll-c-orange">{{ s.mid >= 0 ? s.mid : 'N/A' }}</b></div>
                    <div class="ll-ptr-chip">right = <b class="ll-c-purple">{{ s.right >= 0 ? s.right : 'N/A' }}</b></div>
                    <div class="ll-ptr-chip">i = <b class="ll-c-green">{{ s.mi >= 0 ? s.mi : 'N/A' }}</b></div>
                    <div class="ll-ptr-chip">j = <b class="ll-c-red">{{ s.mj >= 0 ? s.mj : 'N/A' }}</b></div>
                    <div class="ll-ptr-chip">k = <b class="ll-c-teal">{{ s.mk >= 0 ? s.mk : 'N/A' }}</b></div>
                  </div>

                  <!-- Visual Diagram - Pastel Flat Cards -->
                  <div class="ll-arr-track">
                    <template v-for="cell in s.cells" :key="cell.idx">
                      <div class="ll-arr-cell-wrap">
                        <div class="ll-ptr-tag-wrap">
                          <span v-if="s.left === cell.idx" class="ll-ptr-lbl ll-lbl-blue">left</span>
                          <span v-if="s.mid === cell.idx" class="ll-ptr-lbl ll-lbl-orange">mid</span>
                          <span v-if="s.right === cell.idx" class="ll-ptr-lbl ll-lbl-purple">right</span>
                          <span v-if="s.mi >= 0 && cell.idx === (s.left + s.mi)" class="ll-ptr-lbl ll-lbl-green">i</span>
                          <span v-if="s.mj >= 0 && cell.idx === (s.mid + 1 + s.mj)" class="ll-ptr-lbl ll-lbl-red">j</span>
                          <span v-if="s.mk === cell.idx" class="ll-ptr-lbl ll-lbl-teal">k</span>
                        </div>
                        <div
                          class="ll-arr-box"
                          :class="{
                            'll-box-cur': cell.state === 'cur',
                            'll-box-mid': cell.state === 'mid',
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
                <span class="ll-leg"><span class="ll-legdot ll-legdot-existing"></span>Idle</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-cur"></span>Active Subarray (l..r)</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-mid"></span>Mid Element (m)</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-swap"></span>Merging Elements</span>
                <span class="ll-leg"><span class="ll-legdot ll-legdot-sorted"></span>Sorted / Merged</span>
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
                      <tr><td>Best Case</td><td>O(n log n)</td><td>Guaranteed logarithmic division tree of depth log₂ n.</td></tr>
                      <tr><td>Average Case</td><td>O(n log n)</td><td>Performs n comparisons per level across log₂ n recursion levels.</td></tr>
                      <tr><td>Worst Case</td><td>O(n log n)</td><td>Guaranteed O(n log n) even for reverse-sorted input arrays.</td></tr>
                      <tr><td>Space</td><td>O(n)</td><td>Requires auxiliary space O(n) for temporary subarrays during merging.</td></tr>
                    </tbody>
                  </table>
                  <p class="ll-note">
                    Key observation: Merge Sort is a <b>Divide and Conquer</b> algorithm that recursively breaks the array down into sub-problems until individual elements remain, then merges them back in sorted order.
                  </p>
                  <h3>Advantages</h3>
                  <p>Guaranteed O(n log n) time complexity in all cases. Stable sorting algorithm.</p>
                  <h3>Limitations</h3>
                  <p>Requires O(n) additional space, unlike in-place algorithms like Quick Sort or Heap Sort.</p>
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
.ll-c-blue { color: var(--blue); } .ll-c-orange { color: var(--orange); } .ll-c-green { color: var(--green); } .ll-c-purple { color: var(--purple); } .ll-c-red { color: var(--red); } .ll-c-teal { color: #0d9488; }

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
.ll-lbl-teal { color: #0d9488; }
.ll-arr-box { width: 54px; height: 54px; display: flex; align-items: center; justify-content: center; border: 2px solid var(--blue); border-radius: var(--radius); background: #eff6ff; color: #1e293b; font-weight: 700; font-size: 16px; box-shadow: var(--shadow-sm); transition: all 0.25s ease; animation: ll-pop .3s ease; }
.ll-box-cur { border-color: #3b82f6 !important; background: #dbeafe !important; color: #1e40af !important; box-shadow: 0 0 0 3px rgba(59, 130, 246, 0.25) !important; transform: translateY(-2px); }
.ll-box-mid { border-color: #f59e0b !important; background: #fef3c7 !important; color: #92400e !important; box-shadow: 0 0 0 3px rgba(245, 158, 11, 0.25) !important; transform: translateY(-3px); }
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
.ll-legdot-mid { background: #fef3c7; border: 1.5px solid #f59e0b; }
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
@media (max-width: 900px) {
  .ll-main { flex-direction: column; }
  .ll-left-col, .ll-right-col { max-width: 100% !important; width: 100% !important; }
  .ll-resizer { display: none; }
  .ll-toolbar { flex-direction: column; align-items: stretch; }
  .ll-nav-controls { margin-left: 0; justify-content: center; }
}
</style>
