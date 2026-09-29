<script setup>
import { ref, reactive, computed, onMounted, onBeforeUnmount, watch, nextTick } from 'vue';

defineProps({
  topic: { type: String, default: 'Array Algorithms' },
  subTopic: { type: String, default: 'Container With Most Water (LeetCode 11)' }
});

// ─── Multi-Language Code Definitions ─────────────────────────────────────────
const CODES = {
  java: [
    ['',            'import java.util.Scanner;'],
    ['',            ''],
    ['',            'class Solution {'],
    ['fn_entry',    '    public int maxArea(int[] height) {'],
    ['init_left',   '        int left = 0;'],
    ['init_right',  '        int right = height.length - 1;'],
    ['init_max',    '        int maxArea = 0;'],
    ['while_cond',  '        while (left < right) {'],
    ['calc_width',  '            int width = right - left;'],
    ['calc_h',      '            int h = Math.min(height[left], height[right]);'],
    ['calc_area',   '            int area = width * h;'],
    ['if_area',     '            if (area > maxArea) {'],
    ['update_max',  '                maxArea = area;'],
    ['',            '            }'],
    ['if_ptr',      '            if (height[left] < height[right]) {'],
    ['inc_left',    '                left++;'],
    ['',            '            } else {'],
    ['dec_right',   '                right--;'],
    ['',            '            }'],
    ['',            '        }'],
    ['return_stmt', '        return maxArea;'],
    ['',            '    }'],
    ['',            ''],
    ['',            '    public static void main(String[] args) {'],
    ['m_scanner',   '        Scanner sc = new Scanner(System.in);'],
    ['m_read_n',    '        int n = sc.nextInt();'],
    ['m_alloc',     '        int[] height = new int[n];'],
    ['m_loop',      '        for (int i = 0; i < n; i++) {'],
    ['m_read_h',    '            height[i] = sc.nextInt();'],
    ['',            '        }'],
    ['m_call',      '        System.out.println(new Solution().maxArea(height));'],
    ['',            '    }'],
    ['',            '}']
  ],
  cpp: [
    ['',            '#include <vector>'],
    ['',            '#include <algorithm>'],
    ['',            '#include <iostream>'],
    ['',            'using namespace std;'],
    ['',            ''],
    ['',            'class Solution {'],
    ['',            'public:'],
    ['fn_entry',    '    int maxArea(vector<int>& height) {'],
    ['init_left',   '        int left = 0;'],
    ['init_right',  '        int right = height.size() - 1;'],
    ['init_max',    '        int maxArea = 0;'],
    ['while_cond',  '        while (left < right) {'],
    ['calc_width',  '            int width = right - left;'],
    ['calc_h',      '            int h = min(height[left], height[right]);'],
    ['calc_area',   '            int area = width * h;'],
    ['if_area',     '            if (area > maxArea) {'],
    ['update_max',  '                maxArea = area;'],
    ['',            '            }'],
    ['if_ptr',      '            if (height[left] < height[right]) {'],
    ['inc_left',    '                left++;'],
    ['',            '            } else {'],
    ['dec_right',   '                right--;'],
    ['',            '            }'],
    ['',            '        }'],
    ['return_stmt', '        return maxArea;'],
    ['',            '    }'],
    ['',            '};'],
    ['',            ''],
    ['',            'int main() {'],
    ['m_read_n',    '    int n;'],
    ['m_scanner',   '    cin >> n;'],
    ['m_alloc',     '    vector<int> height(n);'],
    ['m_loop',      '    for (int i = 0; i < n; i++) {'],
    ['m_read_h',    '        cin >> height[i];'],
    ['',            '    }'],
    ['m_call',      '    cout << Solution().maxArea(height) << endl;'],
    ['',            '    return 0;'],
    ['',            '}']
  ],
  python: [
    ['',            'class Solution:'],
    ['fn_entry',    '    def maxArea(self, height):'],
    ['init_left',   '        left = 0'],
    ['init_right',  '        right = len(height) - 1'],
    ['init_max',    '        max_area = 0'],
    ['while_cond',  '        while left < right:'],
    ['calc_width',  '            width = right - left'],
    ['calc_h',      '            h = min(height[left], height[right])'],
    ['calc_area',   '            area = width * h'],
    ['if_area',     '            if area > max_area:'],
    ['update_max',  '                max_area = area'],
    ['if_ptr',      '            if height[left] < height[right]:'],
    ['inc_left',    '                left += 1'],
    ['',            '            else:'],
    ['dec_right',   '                right -= 1'],
    ['return_stmt', '        return max_area'],
    ['',            ''],
    ['',            'def main():'],
    ['m_read_n',    '    n = int(input())'],
    ['m_alloc',     '    height = list(map(int, input().split()))'],
    ['m_call',      '    print(Solution().maxArea(height))'],
    ['',            ''],
    ['',            'if __name__ == "__main__":'],
    ['',            '    main()']
  ],
  javascript: [
    ['',            '/**'],
    ['',            ' * @param {number[]} height'],
    ['',            ' * @return {number}'],
    ['',            ' */'],
    ['fn_entry',    'var maxArea = function(height) {'],
    ['init_left',   '    let left = 0;'],
    ['init_right',  '    let right = height.length - 1;'],
    ['init_max',    '    let maxArea = 0;'],
    ['while_cond',  '    while (left < right) {'],
    ['calc_width',  '        let width = right - left;'],
    ['calc_h',      '        let h = Math.min(height[left], height[right]);'],
    ['calc_area',   '        let area = width * h;'],
    ['if_area',     '        if (area > maxArea) {'],
    ['update_max',  '            maxArea = area;'],
    ['',            '        }'],
    ['if_ptr',      '        if (height[left] < height[right]) {'],
    ['inc_left',    '            left++;'],
    ['',            '        } else {'],
    ['dec_right',   '            right--;'],
    ['',            '        }'],
    ['',            '    }'],
    ['return_stmt', '    return maxArea;'],
    ['',            '};'],
    ['',            ''],
    ['',            'function main() {'],
    ['m_read_n',    '    const n = parseInt(tokens[0]);'],
    ['m_alloc',     '    const height = tokens.slice(1, n + 1).map(Number);'],
    ['m_call',      '    console.log(maxArea(height));'],
    ['',            '}'],
    ['',            'main();']
  ],
  c: [
    ['',            '#include <stdio.h>'],
    ['',            ''],
    ['fn_entry',    'int maxArea(int* height, int n) {'],
    ['init_left',   '    int left = 0;'],
    ['init_right',  '    int right = n - 1;'],
    ['init_max',    '    int maxArea = 0;'],
    ['while_cond',  '    while (left < right) {'],
    ['calc_width',  '        int width = right - left;'],
    ['calc_h',      '        int h = height[left] < height[right] ? height[left] : height[right];'],
    ['calc_area',   '        int area = width * h;'],
    ['if_area',     '        if (area > maxArea) {'],
    ['update_max',  '            maxArea = area;'],
    ['',            '        }'],
    ['if_ptr',      '        if (height[left] < height[right]) {'],
    ['inc_left',    '            left++;'],
    ['',            '        } else {'],
    ['dec_right',   '            right--;'],
    ['',            '        }'],
    ['',            '    }'],
    ['return_stmt', '    return maxArea;'],
    ['',            '}'],
    ['',            ''],
    ['',            'int main() {'],
    ['m_read_n',    '    int n;'],
    ['m_scanner',   '    scanf("%d", &n);'],
    ['m_alloc',     '    int height[100];'],
    ['m_loop',      '    for (int i = 0; i < n; i++) {'],
    ['m_read_h',    '        scanf("%d", &height[i]);'],
    ['',            '    }'],
    ['m_call',      '    printf("%d\\n", maxArea(height, n));'],
    ['',            '    return 0;'],
    ['',            '}']
  ]
};

const PSEUDOCODE = [
  'function maxArea(height):',
  '    left     = 0',
  '    right    = len(height) - 1',
  '    max_area = 0',
  '',
  '    while left < right:',
  '        width = right - left',
  '        h     = min(height[left], height[right])',
  '        area  = width * h',
  '',
  '        if area > max_area:',
  '            max_area = area',
  '',
  '        // Move the shorter wall inward',
  '        if height[left] < height[right]:',
  '            left += 1',
  '        else:',
  '            right -= 1',
  '',
  '    return max_area'
];

// ─── Step Builder ─────────────────────────────────────────────────────────────
function buildSteps(rawHeight) {
  const steps = [];
  const height = rawHeight.slice(0, 10);
  const n = height.length;
  if (n < 2) {
    return steps;
  }
  const maxH = Math.max(...height);

  const NONE = { leftReady: false, rightReady: false, maxReady: false };
  const ALL  = { leftReady: true,  rightReady: true,  maxReady: true  };

  function snap(lv, rv, maxV, wV, hV, aV, code, phase, initState, extra) {
    return {
      left: lv,
      right: rv,
      maxAreaVal: maxV,
      widthVal: wV,
      hVal: hV,
      areaVal: aV,
      stepHeight: [...(extra.partial || height)],
      n,
      maxH,
      code,
      phase,
      initState: { ...initState },
      ...extra
    };
  }

  // ── Phase 1: main() — reading inputs ───────────────────────────────────────
  const partial = new Array(n).fill(0);

  steps.push({
    ...snap(-1, -1, -1, -1, -1, -1, 'm_scanner', 'input', NONE, { partial: [...partial] }),
    badge: `Scanner sc = new Scanner(System.in); → Initializing input stream in main().`
  });
  steps.push({
    ...snap(-1, -1, -1, -1, -1, -1, 'm_read_n', 'input', NONE, { partial: [...partial] }),
    badge: `int n = sc.nextInt(); → Read n = ${n}. height[] will have ${n} elements.`
  });
  steps.push({
    ...snap(-1, -1, -1, -1, -1, -1, 'm_alloc', 'input', NONE, { partial: [...partial] }),
    badge: `int[] height = new int[n]; → Allocated height[] with ${n} slots.`
  });

  for (let i = 0; i < n; i++) {
    steps.push({
      ...snap(-1, -1, -1, -1, -1, -1, 'm_loop', 'input', NONE, { partial: [...partial], readIdx: i }),
      badge: `for (int i = ${i}; i < ${n}; i++) → i = ${i}. Will read height[${i}].`
    });
    partial[i] = height[i];
    steps.push({
      ...snap(-1, -1, -1, -1, -1, -1, 'm_read_h', 'input', NONE, { partial: [...partial], readIdx: i, justReadIdx: i }),
      badge: `height[i] = sc.nextInt(); → height[${i}] = ${height[i]}. Stored at index ${i}.`
    });
  }
  steps.push({
    ...snap(-1, -1, -1, -1, -1, -1, 'm_loop', 'input', NONE, { partial: [...partial], readIdx: n }),
    badge: `for (int i = ${n}; i < ${n}; i++) → i = ${n} is NOT less than n. Exit loop.`
  });
  steps.push({
    ...snap(-1, -1, -1, -1, -1, -1, 'm_call', 'input', NONE, { partial: [...height] }),
    badge: `System.out.println(new Solution().maxArea(height)); → All input read. Calling maxArea() with n=${n} elements.`
  });

  // ── Phase 2: maxArea() function ───────────────────────────────────────────
  const INIT_ENTRY  = { leftReady: false, rightReady: false, maxReady: false };
  const AFTER_LEFT  = { leftReady: true,  rightReady: false, maxReady: false };
  const AFTER_RIGHT = { leftReady: true,  rightReady: true,  maxReady: false };

  steps.push({
    ...snap(-1, -1, -1, -1, -1, -1, 'fn_entry', 'init', INIT_ENTRY, {}),
    badge: `public int maxArea(int[] height) → Function entry. Two-pointer approach will start from both ends.`
  });
  steps.push({
    ...snap(0, -1, -1, -1, -1, -1, 'init_left', 'init', AFTER_LEFT, {}),
    badge: `int left = 0; → left = 0. Points to the leftmost bar.`
  });
  steps.push({
    ...snap(0, n - 1, -1, -1, -1, -1, 'init_right', 'init', AFTER_RIGHT, {}),
    badge: `int right = height.length - 1; → right = ${n - 1}. Points to the rightmost bar.`
  });
  steps.push({
    ...snap(0, n - 1, 0, -1, -1, -1, 'init_max', 'init', ALL, {}),
    badge: `int maxArea = 0; → maxArea = 0. Will track the largest container seen so far.`
  });

  let left = 0;
  let right = n - 1;
  let maxAreaVal = 0;

  while (left < right) {
    steps.push({
      ...snap(left, right, maxAreaVal, -1, -1, -1, 'while_cond', 'solve', ALL, {}),
      badge: `while (left < right): left=${left} < right=${right} → TRUE. Continue scanning.`
    });

    const widthVal = right - left;
    steps.push({
      ...snap(left, right, maxAreaVal, widthVal, -1, -1, 'calc_width', 'solve', ALL, {}),
      badge: `int width = right - left; → width = ${right} - ${left} = ${widthVal}.`
    });

    const hVal = Math.min(height[left], height[right]);
    steps.push({
      ...snap(left, right, maxAreaVal, widthVal, hVal, -1, 'calc_h', 'solve', ALL, {}),
      badge: `int h = Math.min(height[left], height[right]); → h = min(height[${left}]=${height[left]}, height[${right}]=${height[right]}) = ${hVal}.`
    });

    const areaVal = widthVal * hVal;
    steps.push({
      ...snap(left, right, maxAreaVal, widthVal, hVal, areaVal, 'calc_area', 'solve', ALL, {}),
      badge: `int area = width * h; → area = ${widthVal} × ${hVal} = ${areaVal}.`
    });

    const areaGreater = areaVal > maxAreaVal;
    steps.push({
      ...snap(left, right, maxAreaVal, widthVal, hVal, areaVal, 'if_area', 'solve', ALL, { areaGreater }),
      badge: `if (area > maxArea): ${areaVal} > ${maxAreaVal} → ${areaGreater ? 'TRUE. Update maxArea.' : 'FALSE. Skip update.'}`
    });

    if (areaGreater) {
      maxAreaVal = areaVal;
      steps.push({
        ...snap(left, right, maxAreaVal, widthVal, hVal, areaVal, 'update_max', 'solve', ALL, {}),
        badge: `maxArea = area; → maxArea updated to ${maxAreaVal}.`
      });
    }

    const moveLeft = height[left] < height[right];
    steps.push({
      ...snap(left, right, maxAreaVal, widthVal, hVal, areaVal, 'if_ptr', 'solve', ALL, { moveLeft }),
      badge: `if (height[left] < height[right]): height[${left}]=${height[left]} < height[${right}]=${height[right]} → ${moveLeft ? 'TRUE. Move left pointer right.' : 'FALSE. Move right pointer left.'}`
    });

    if (moveLeft) {
      left++;
      steps.push({
        ...snap(left, right, maxAreaVal, widthVal, hVal, areaVal, 'inc_left', 'solve', ALL, {}),
        badge: `left++; → left = ${left}. The left bar was shorter; moving inward cannot decrease the max.`
      });
    } else {
      right--;
      steps.push({
        ...snap(left, right, maxAreaVal, widthVal, hVal, areaVal, 'dec_right', 'solve', ALL, {}),
        badge: `right--; → right = ${right}. The right bar was shorter (or equal); moving inward.`
      });
    }
  }

  steps.push({
    ...snap(left, right, maxAreaVal, -1, -1, -1, 'while_cond', 'done', ALL, {}),
    badge: `while (left < right): left=${left}, right=${right} → FALSE. Pointers met. All pairs checked.`
  });
  steps.push({
    ...snap(left, right, maxAreaVal, -1, -1, -1, 'return_stmt', 'done', ALL, {}),
    badge: `return maxArea; → Answer = ${maxAreaVal}. This is the maximum water the container can hold.`
  });

  return steps;
}

// ─── Reactive State ───────────────────────────────────────────────────────────
const DEFAULT_HEIGHT = [1, 8, 6, 2, 5, 4, 8, 3, 7];
const inputHeightStr = ref('1,8,6,2,5,4,8,3,7');
const lang      = ref('java');
const speed     = ref(650);
const si        = ref(0);
const playing   = ref(false);
const vizHeight = ref(360);
const tableHeight = ref(60);
const leftWidth = ref(52);
const rightTab  = ref('code');

const stepsData = reactive({ steps: buildSteps([...DEFAULT_HEIGHT]) });
const steps     = computed(() => stepsData.steps);
const s         = computed(() => steps.value[Math.max(0, Math.min(si.value, steps.value.length - 1))] || {});
const codeLines = computed(() => CODES[lang.value] || []);

let playTimer = null;

function onChipsWheel(e) {
  if (e.currentTarget) {
    e.currentTarget.scrollLeft += e.deltaY;
  }
}

function parseArr(str) {
  return str.trim().split(/[\s,]+/).map(x => parseInt(x, 10)).filter(x => !isNaN(x) && x >= 0);
}

function applyInput() {
  const arr = parseArr(inputHeightStr.value);
  if (arr.length < 2) {
    alert('Please enter at least 2 values for the height array.');
    return;
  }
  if (arr.length > 10) {
    alert('Maximum 10 values allowed for visualization.');
    return;
  }
  playing.value = false;
  stepsData.steps = buildSteps(arr);
  si.value = 0;
}

function stepBy(d) {
  si.value = Math.max(0, Math.min(steps.value.length - 1, si.value + d));
}

function togglePlay() {
  const next = !playing.value;
  if (next && si.value >= steps.value.length - 1) {
    si.value = 0;
  }
  playing.value = next;
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
    const contRect  = container.getBoundingClientRect();
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

// ─── Computed display helpers ─────────────────────────────────────────────────
const initState   = computed(() => s.value.initState || { leftReady: false, rightReady: false, maxReady: false });
const displayLeft = computed(() => initState.value.leftReady  ? s.value.left         : '?');
const displayRight= computed(() => initState.value.rightReady ? s.value.right        : '?');
const displayMax  = computed(() => initState.value.maxReady   ? s.value.maxAreaVal   : '?');
const displayWidth= computed(() => s.value.widthVal  != null && s.value.widthVal  >= 0 ? s.value.widthVal  : '?');
const displayH    = computed(() => s.value.hVal      != null && s.value.hVal      >= 0 ? s.value.hVal      : '?');
const displayArea = computed(() => s.value.areaVal   != null && s.value.areaVal   >= 0 ? s.value.areaVal   : '?');
const displayBars = computed(() => s.value.stepHeight || []);
const displayMaxH = computed(() => s.value.maxH || 1);
const displayN    = computed(() => s.value.n    || 0);

// bar pixel height (max 140px)
const BAR_MAX_PX = 140;
function barPx(val) {
  const mh = displayMaxH.value;
  return mh > 0 ? Math.max(8, Math.round((val / mh) * BAR_MAX_PX)) : 8;
}

// Water level pixel height (capped at min(left, right) bar height)
const waterLevelPx = computed(() => {
  if (s.value.phase !== 'solve' || !initState.value.leftReady || !initState.value.rightReady) {
    return 0;
  }
  if (s.value.hVal == null || s.value.hVal < 0) {
    return 0;
  }
  return barPx(s.value.hVal);
});

function barClass(idx) {
  const phase = s.value.phase;
  const ist   = initState.value;
  const left  = s.value.left;
  const right = s.value.right;
  const classes = [];
  if (phase === 'done') {
    classes.push('cw-bar-done');
    return classes.join(' ');
  }
  if (phase === 'input') {
    if (s.value.justReadIdx === idx) {
      classes.push('cw-bar-just-read');
    }
    return classes.join(' ');
  }
  if (!ist.leftReady || !ist.rightReady) {
    return classes.join(' ');
  }
  if (idx === left) {
    classes.push('cw-bar-left');
  }
  if (idx === right) {
    classes.push('cw-bar-right');
  }
  if (idx > left && idx < right) {
    classes.push('cw-bar-inner');
  }
  return classes.join(' ');
}

function showWater(idx) {
  if (waterLevelPx.value <= 0) {
    return false;
  }
  const left  = s.value.left;
  const right = s.value.right;
  return idx >= left && idx <= right;
}

// ─── Resizer setup ────────────────────────────────────────────────────────────
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
  cleanupFns.push(initVResizer(vizResizerRef,   vizHeight,   200, 700));
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
              <label>height[]:</label>
              <input type="text" v-model="inputHeightStr" class="ll-text-input cw-arr-input"
                @keyup.enter="applyInput" placeholder="e.g. 1,8,6,2,5,4,8,3,7" />
            </div>
            <button class="ll-viz-btn" @click="applyInput">&#9654; Visualize</button>
            <div class="ll-nav-controls">
              <button class="ll-nav-btn" title="First step" @click="stepBy(-steps.length)">&#171;</button>
              <button class="ll-nav-btn" title="Previous step" @click="stepBy(-1)">&#8249; Prev</button>
              <button class="ll-play-btn" @click="togglePlay">{{ playing ? '\u23F8 Pause' : '\u25B6 Play' }}</button>
              <button class="ll-nav-btn" title="Next step" @click="stepBy(1)">Next &#8250;</button>
              <button class="ll-nav-btn" title="Last step" @click="stepBy(steps.length)">&#187;</button>
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
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">width</span><b class="ll-c-purple">{{ displayWidth }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">h</span><b class="ll-c-purple">{{ displayH }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">area</span><b class="ll-c-orange">{{ displayArea }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">maxArea</span><b class="ll-c-orange">{{ displayMax }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">phase</span><b class="ll-c-orange">{{ s.phase || 'input' }}</b></span>
                  </div>

                  <!-- BOARD CONTAINER -->
                  <div class="ll-board-container">

                    <!-- Tier 1 — Bar Chart -->
                    <div class="cw-tier-title">
                      Tier 1 &mdash; height[] Bar Chart
                      <code>n = {{ displayN }}</code>
                    </div>
                    <div class="cw-chart-frame">
                      <!-- Pointer labels row (top) -->
                      <div class="cw-ptr-row">
                        <div
                          v-for="(val, idx) in displayBars"
                          :key="'ptr-' + idx"
                          class="cw-ptr-cell"
                        >
                          <span
                            v-if="initState.leftReady && idx === s.left && (s.phase === 'solve' || s.phase === 'init' || s.phase === 'done')"
                            class="cw-ptr-label cw-ptr-left"
                          >left</span>
                          <span
                            v-if="initState.rightReady && idx === s.right && (s.phase === 'solve' || s.phase === 'init' || s.phase === 'done')"
                            class="cw-ptr-label cw-ptr-right"
                          >right</span>
                        </div>
                      </div>

                      <!-- Bars -->
                      <div class="cw-bar-row">
                        <div
                          v-for="(val, idx) in displayBars"
                          :key="'bar-col-' + idx"
                          class="cw-bar-col"
                        >
                          <!-- Water overlay (rendered below the bar, aligned to bottom) -->
                          <div class="cw-bar-slot">
                            <!-- Water fill between left and right -->
                            <div
                              v-if="showWater(idx)"
                              class="cw-water-fill"
                              :style="{ height: waterLevelPx + 'px' }"
                            ></div>
                            <!-- The actual bar -->
                            <div
                              class="cw-bar"
                              :class="barClass(idx)"
                              :style="{ height: barPx(val) + 'px' }"
                            >
                              <span class="cw-bar-val">{{ val }}</span>
                            </div>
                          </div>
                          <span class="cw-bar-idx">[{{ idx }}]</span>
                        </div>
                      </div>
                    </div>

                    <!-- Tier 2 — Variable State Panel -->
                    <div class="cw-tier-title">Tier 2 &mdash; Variable State</div>
                    <div class="cw-var-panel">
                      <div class="cw-var-card" :class="{ 'cw-var-active': initState.leftReady }">
                        <span class="cw-var-name cw-ptr-left">left</span>
                        <span class="cw-var-val">{{ !initState.leftReady ? '?' : s.left }}</span>
                        <span class="cw-var-desc">left pointer</span>
                      </div>
                      <div class="cw-var-card" :class="{ 'cw-var-active': initState.rightReady }">
                        <span class="cw-var-name cw-ptr-right">right</span>
                        <span class="cw-var-val">{{ !initState.rightReady ? '?' : s.right }}</span>
                        <span class="cw-var-desc">right pointer</span>
                      </div>
                      <div class="cw-var-card" :class="{ 'cw-var-active': s.widthVal >= 0 }">
                        <span class="cw-var-name" style="color:var(--purple)">width</span>
                        <span class="cw-var-val">{{ displayWidth }}</span>
                        <span class="cw-var-desc">right - left</span>
                      </div>
                      <div class="cw-var-card" :class="{ 'cw-var-active': s.hVal >= 0 }">
                        <span class="cw-var-name" style="color:var(--purple)">h</span>
                        <span class="cw-var-val">{{ displayH }}</span>
                        <span class="cw-var-desc">min height</span>
                      </div>
                      <div class="cw-var-card" :class="{ 'cw-var-active': s.areaVal >= 0 }">
                        <span class="cw-var-name" style="color:var(--orange)">area</span>
                        <span class="cw-var-val">{{ displayArea }}</span>
                        <span class="cw-var-desc">current area</span>
                      </div>
                      <div class="cw-var-card" :class="{ 'cw-var-active': initState.maxReady }">
                        <span class="cw-var-name" style="color:var(--orange)">maxArea</span>
                        <span class="cw-var-val">{{ displayMax }}</span>
                        <span class="cw-var-desc">best so far</span>
                      </div>
                    </div>

                  </div>
                  <!-- END ll-board-container -->
                </div>
              </div>

              <!-- Vertical Resizer -->
              <div class="ll-vresizer" ref="vizResizerRef"></div>

              <!-- Color Legend -->
              <div class="ll-legend">
                <span class="ll-leg"><span class="ll-legdot" style="background:#eff6ff;border:1.5px solid #3b82f6;"></span>left</span>
                <span class="ll-leg"><span class="ll-legdot" style="background:#f0fdf4;border:1.5px solid #22c55e;"></span>right</span>
                <span class="ll-leg"><span class="ll-legdot" style="background:#e0f2fe;border:1.5px dashed #38bdf8;"></span>inner bars</span>
                <span class="ll-leg"><span class="ll-legdot cw-legdot-water"></span>water</span>
                <span class="ll-leg"><span class="ll-legdot cw-legdot-done"></span>done</span>
              </div>

              <!-- Call Stack Area -->
              <div class="ll-table-area" :style="{ height: tableHeight + 'px' }">
                <div class="ll-table-title">Call Stack &mdash; Current Execution Frame</div>
                <div class="ll-stack-line">
                  <template v-if="s.phase === 'input'">
                    <div class="ll-frame ll-frame-cur">
                      main()
                      &nbsp;&nbsp;
                      <span class="ll-fname">n</span>=<span class="ll-c-blue" style="font-weight:700">{{ displayN }}</span>
                      <span class="ll-now"> &#9668; active</span>
                    </div>
                  </template>
                  <template v-else>
                    <div class="ll-frame" style="color:var(--text2);">main()</div>
                    <div class="ll-frame ll-frame-cur" style="margin-left:14px">
                      maxArea(height, n={{ displayN }})
                      &nbsp;&nbsp;
                      <span class="ll-fname">left</span>=<span class="ll-c-blue" style="font-weight:700">{{ displayLeft }}</span>,
                      <span class="ll-fname">right</span>=<span class="ll-c-green" style="font-weight:700">{{ displayRight }}</span>,
                      <span class="ll-fname">maxArea</span>=<span class="ll-c-orange" style="font-weight:700">{{ displayMax }}</span>
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
                    'll-badge-error':   s.badge && (s.badge.includes('FALSE') || s.badge.includes('Skip')),
                    'll-badge-success': s.badge && (s.badge.includes('Answer') || s.badge.includes('TRUE') || s.badge.includes('updated'))
                  }"
                >
                  {{ s.badge || 'Ready to visualize Container With Most Water.' }}
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
                  <h3 class="ll-cx-heading">Container With Most Water (LeetCode 11) &mdash; Complexity Analysis</h3>
                  <p class="ll-cx-intro">
                    Given an array <code>height</code> of <code>n</code> non-negative integers where each element
                    represents a vertical bar, find two bars that together with the x-axis form a container holding the
                    most water. The key insight is the <strong>two-pointer technique</strong>: always move the pointer
                    pointing to the shorter bar, because the area is bounded by the shorter bar.
                  </p>

                  <h4 class="ll-cx-sub">Step-by-step Breakdown</h4>
                  <table class="ll-complexity-table">
                    <thead><tr><th>Operation</th><th>Time</th><th>Space</th><th>Reason</th></tr></thead>
                    <tbody>
                      <tr><td>Initialize left, right, maxArea</td><td class="ll-cx-good">O(1)</td><td class="ll-cx-good">O(1)</td><td>Three constant-time assignments</td></tr>
                      <tr><td>While loop (two-pointer scan)</td><td class="ll-cx-good">O(n)</td><td class="ll-cx-good">O(1)</td><td>Each pointer moves at most n steps total; loop runs at most n-1 times</td></tr>
                      <tr><td>Per-iteration work (width, h, area, compare)</td><td class="ll-cx-good">O(1)</td><td class="ll-cx-good">O(1)</td><td>All arithmetic in constant time</td></tr>
                      <tr><td>Return maxArea</td><td class="ll-cx-good">O(1)</td><td class="ll-cx-good">O(1)</td><td>Single return statement</td></tr>
                      <tr><td><strong>Total</strong></td><td class="ll-cx-good">O(n)</td><td class="ll-cx-good">O(1)</td><td>Single-pass, no extra storage needed</td></tr>
                    </tbody>
                  </table>

                  <h4 class="ll-cx-sub">Overall Complexity</h4>
                  <div class="ll-cx-summary-grid">
                    <div class="ll-cx-card ll-cx-card-good">
                      <div class="ll-cx-card-label">Time</div>
                      <div class="ll-cx-card-val">O(n)</div>
                      <div class="ll-cx-card-note">Single pass — left and right meet exactly once</div>
                    </div>
                    <div class="ll-cx-card ll-cx-card-good">
                      <div class="ll-cx-card-label">Space</div>
                      <div class="ll-cx-card-val">O(1)</div>
                      <div class="ll-cx-card-note">Only 6 integer variables — no auxiliary array</div>
                    </div>
                    <div class="ll-cx-card ll-cx-card-good">
                      <div class="ll-cx-card-label">Passes</div>
                      <div class="ll-cx-card-val">1</div>
                      <div class="ll-cx-card-note">Optimal — brute-force would be O(n²)</div>
                    </div>
                  </div>

                  <div class="ll-note">
                    <strong>Why move the shorter bar?</strong> If we move the taller bar's pointer inward, the width
                    decreases and the height is still bounded by the same shorter bar — the area can only decrease or stay
                    the same. Moving the shorter bar gives us the only chance to find a taller bar that might compensate
                    for the reduced width.
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

@keyframes cw-flash-read   { 0% { background:#fef9c3; } 60% { background:#86efac; } 100% { background:#f0fdf4; } }
@keyframes cw-flash-maxupd { 0% { background:#fef9c3; } 60% { background:#fdba74; } 100% { background:#fff7ed; } }
@keyframes cw-water-ripple { 0% { opacity:.4; } 50% { opacity:.85; } 100% { opacity:.6; } }

.slide-wrapper { margin-top: -10px; margin-left: -30px; width: 107%; max-height: 100%; font-size: 0.8rem; font-weight: 400; }
.slide-body    { display: flex; flex-direction: column; border-radius: 4px; height: 100%; }
.navbar { display: flex; flex-direction: row; justify-content: space-between; align-items: center; gap: .75rem; padding: 0 10px; background-color: #fff; position: fixed; width: 94.7%; z-index: 50; }
.navbar > img  { height: 30px; }
.navbar-title  { margin: 0; font-size: 1.35rem; font-weight: 700; background-color: #ef5050; color: #fff; width: 80%; padding: 2px 10px; margin-left: -10px; border-radius: 5px; }
.row-main      { width: 100%; height: 90%; margin-top: 36px; overflow-x: auto; overflow-y: hidden; }

/* Toolbar */
.ll-toolbar { margin-top: 4px; display: flex; align-items: center; gap: 6px; padding: 6.5px 12px; background: var(--surface); border-bottom: 1px solid var(--border); flex-shrink: 0; flex-wrap: wrap; box-shadow: var(--shadow-sm); }
.ll-input-group { display: flex; align-items: center; gap: 4px; }
.ll-input-group label { font-size: 11px; color: var(--muted); font-weight: 700; }
.ll-text-input { background: var(--surface); border: 1px solid var(--border2); color: var(--text); border-radius: var(--radius-sm); padding: 3px 6px; font-size: 11.5px; font-family: monospace; }
.ll-text-input:focus { outline: none; border-color: var(--coral); box-shadow: 0 0 0 3px rgba(240,77,77,.1); }
.cw-arr-input  { width: 260px; }
.ll-viz-btn    { background: var(--coral); color: #fff; border: none; padding: 5px 12px; border-radius: var(--radius-sm); cursor: pointer; font-size: 11.5px; font-weight: 600; box-shadow: var(--shadow-sm); transition: filter .15s; }
.ll-viz-btn:hover { filter: brightness(1.08); }
.ll-nav-controls { display: flex; margin-left: auto; align-items: center; gap: 4px; flex-shrink: 0; flex-wrap: wrap; }
.ll-nav-btn  { background: var(--surface2); border: 1px solid var(--border2); color: var(--text2); padding: 4px 9px; border-radius: var(--radius-sm); cursor: pointer; font-size: 11px; font-weight: 500; transition: all .15s; white-space: nowrap; }
.ll-nav-btn:hover { background: var(--surface); border-color: var(--coral); color: var(--coral); }
.ll-play-btn { background: var(--blue-light); border: 1px solid var(--blue); color: var(--blue); min-width: 68px; font-weight: 600; padding: 4px 9px; border-radius: var(--radius-sm); cursor: pointer; font-size: 11px; transition: all .15s; }
.ll-play-btn:hover { background: var(--blue); color: #fff; }

/* Layout */
.ll-main      { display: flex; flex: 1; overflow: hidden; position: relative; }
.ll-left-col  { display: flex; flex-direction: column; overflow: hidden; min-width: 220px; max-width: 75%; }
.ll-resizer   { width: 5px; cursor: col-resize; background: var(--border); flex-shrink: 0; transition: background .15s; position: relative; z-index: 20; }
.ll-resizer:hover, .ll-resizer.drag { background: var(--coral); }
.ll-right-col { display: flex; flex-direction: column; flex: 1; overflow: hidden; min-width: 0; height: 100%; }
.ll-viz-wrap  { flex-shrink: 0; background: var(--surface); border-bottom: 1px solid var(--border); position: relative; overflow-x: auto; overflow-y: auto; }
.ll-perm-area { display: flex; flex-direction: column; align-items: stretch; min-height: 100%; width: 100%; min-width: 0; box-sizing: border-box; }
.ll-ptrs      { display: flex; gap: 8px; flex-wrap: wrap; padding: 4px 14px; min-height: 28px; width: 100%; box-sizing: border-box; min-width: 0; align-items: center; }
.ll-ptrs-compact { flex-wrap: nowrap; gap: 6px; padding: 3px 14px 5px; min-height: 0; overflow-x: auto; overflow-y: hidden; }
.ll-ptr-chip-inline { display: inline-flex; align-items: center; gap: 4px; background: var(--surface2); border: 1px solid var(--border); border-radius: 6px; padding: 2px 8px; font-size: 11px; font-family: monospace; white-space: nowrap; flex-shrink: 0; line-height: 1.4; }
.ll-chip-label { color: #8899aa; font-weight: 500; margin-right: 2px; }
.ll-c-blue   { color: var(--blue); }
.ll-c-orange { color: var(--orange); }
.ll-c-green  { color: var(--green); }
.ll-c-purple { color: var(--purple); }

/* ─── Board Container — Container With Most Water ─────────────────────────── */
.ll-board-container { display: flex; flex-direction: column; align-items: flex-start; padding: 6px 14px 10px; gap: 8px; min-width: 0; }

.cw-tier-title {
  font-size: 10px; font-weight: 700; text-transform: uppercase; letter-spacing: .04em;
  color: var(--muted); margin-top: 4px; margin-bottom: 2px; font-family: 'Consolas', monospace;
  border-left: 3px solid var(--coral); padding-left: 6px;
}

/* Chart frame */
.cw-chart-frame {
  display: flex; flex-direction: column; background: #f8fafc; border: 1px solid var(--border);
  border-radius: var(--radius); padding: 8px 12px 10px; box-shadow: var(--shadow-sm); gap: 4px;
}

/* Pointer row above bars */
.cw-ptr-row  { display: flex; gap: 4px; height: 18px; align-items: flex-end; }
.cw-ptr-cell { flex: 1; min-width: 36px; max-width: 56px; display: flex; justify-content: center; align-items: flex-end; gap: 2px; }
.cw-ptr-label { font-size: 9px; font-weight: 800; font-family: monospace; padding: 1px 4px; border-radius: 3px; line-height: 1.2; white-space: nowrap; }
.cw-ptr-left  { background: #eff6ff; color: #1d4ed8; border: 1px solid #93c5fd; }
.cw-ptr-right { background: #f0fdf4; color: #15803d; border: 1px solid #86efac; }

/* Bar row */
.cw-bar-row { display: flex; gap: 4px; align-items: flex-end; }
.cw-bar-col { display: flex; flex-direction: column; align-items: center; flex: 1; min-width: 32px; max-width: 56px; }

/* The slot: fixed height, bars + water align from bottom */
.cw-bar-slot {
  position: relative; width: 100%;
  height: 156px;   /* matches BAR_MAX_PX + 16 buffer */
  display: flex; align-items: flex-end; justify-content: center;
}

/* Water fill (behind bar, full width, aligned from bottom) */
.cw-water-fill {
  position: absolute; bottom: 0; left: 0; right: 0;
  background: rgba(56, 189, 248, 0.35);
  border-top: 2px solid rgba(56, 189, 248, 0.7);
  border-radius: 3px 3px 0 0;
  animation: cw-water-ripple 1.4s ease-in-out infinite;
  z-index: 1;
}

/* The bar itself */
.cw-bar {
  position: relative; z-index: 2;
  width: 90%; min-width: 24px;
  border-radius: 4px 4px 0 0;
  background: #cbd5e1;
  border: 1.5px solid #94a3b8;
  display: flex; align-items: flex-start; justify-content: center;
  transition: all .2s ease;
}
.cw-bar-val {
  font-size: 9px; font-weight: 800; font-family: monospace;
  color: var(--text); padding-top: 2px; line-height: 1;
}
.cw-bar-idx {
  font-size: 8px; color: var(--muted); font-family: monospace; font-weight: 700; margin-top: 3px;
}

/* Bar state classes */
.cw-bar-left  { background: #bfdbfe !important; border: 2px solid var(--blue) !important; box-shadow: 0 0 8px rgba(59,130,246,.35); }
.cw-bar-right { background: #bbf7d0 !important; border: 2px solid var(--green) !important; box-shadow: 0 0 8px rgba(34,197,94,.35); }
.cw-bar-inner { background: #e0f2fe !important; border: 1.5px dashed #38bdf8 !important; }
.cw-bar-just-read { background: #dcfce7 !important; border: 2px solid #10b981 !important; animation: cw-flash-read .45s ease; }
.cw-bar-done  { background: #dcfce7 !important; border: 1.5px solid #86efac !important; }

/* Variable State Panel */
.cw-var-panel { display: flex; gap: 6px; flex-wrap: wrap; }
.cw-var-card {
  display: flex; flex-direction: column; align-items: center; gap: 2px;
  padding: 5px 12px; border-radius: var(--radius-sm); border: 1.5px solid var(--border);
  background: var(--surface2); min-width: 76px; font-size: 10.5px; font-family: monospace;
  transition: all .15s;
}
.cw-var-active { border-color: var(--orange) !important; background: var(--orange-light) !important; }
.cw-var-name { font-size: 10px; font-weight: 800; padding: 1px 5px; border-radius: 3px; }
.cw-var-val  { font-size: 17px; font-weight: 900; color: var(--text); line-height: 1.2; }
.cw-var-desc { font-size: 8.5px; color: var(--muted); text-align: center; }

/* Legend dots */
.cw-legdot-water { background: rgba(56,189,248,.45); border: 1.5px solid #38bdf8; display: inline-block; width: 11px; height: 11px; border-radius: 3px; }
.cw-legdot-done  { background: #dcfce7; border: 1.5px solid #86efac; display: inline-block; width: 11px; height: 11px; border-radius: 3px; }

/* Shared layout helpers */
.ll-vresizer { height: 5px; cursor: row-resize; background: var(--border); flex-shrink: 0; transition: background .15s; position: relative; z-index: 20; }
.ll-vresizer:hover, .ll-vresizer.drag { background: var(--coral); }
.ll-legend   { display: flex; flex-wrap: wrap; gap: 6px 14px; padding: 6px 12px; border-bottom: 1px solid var(--border); flex-shrink: 0; background: var(--surface2); }
.ll-leg      { display: flex; align-items: center; gap: 5px; font-size: 11px; color: var(--text2); font-weight: 500; }
.ll-legdot   { width: 11px; height: 11px; border-radius: 3px; flex-shrink: 0; display: inline-block; }

.ll-table-area  { flex-shrink: 0; padding: 8px 14px; border-bottom: 1px solid var(--border); overflow-x: hidden; overflow-y: auto; background: var(--surface); min-width: 0; box-sizing: border-box; }
.ll-table-title { font-size: 10px; color: var(--muted); margin-bottom: 4px; font-style: italic; }
.ll-stack-line  { font-family: 'Consolas', monospace; font-size: 12px; line-height: 1.8; }
.ll-frame       { font-family: 'Consolas', monospace; font-size: 11.5px; color: var(--text2); padding: 1px 0; white-space: nowrap; }
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
.ll-tab-btn:hover     { border-color: var(--coral); color: var(--coral); }
.ll-tab-btn.active    { background: var(--coral); border-color: var(--coral); color: #fff; }
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
</style>
