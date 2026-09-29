<script setup>
import { ref, reactive, computed, onMounted, onBeforeUnmount, watch, nextTick } from 'vue';

defineProps({
  topic:    { type: String, default: 'Array Algorithms' },
  subTopic: { type: String, default: 'Trapping Rain Water (LeetCode 42)' }
});

// ─── Multi-Language Code Definitions ─────────────────────────────────────────
const CODES = {
  java: [
    ['',            'import java.util.Scanner;'],
    ['',            ''],
    ['',            'class Solution {'],
    ['fn_entry',    '    public int trap(int[] height) {'],
    ['init_left',   '        int left = 0;'],
    ['init_right',  '        int right = height.length - 1;'],
    ['init_lmax',   '        int leftMax = 0;'],
    ['init_rmax',   '        int rightMax = 0;'],
    ['init_water',  '        int water = 0;'],
    ['while_cond',  '        while (left < right) {'],
    ['if_side',     '            if (height[left] < height[right]) {'],
    ['if_lmax',     '                if (height[left] >= leftMax) {'],
    ['update_lmax', '                    leftMax = height[left];'],
    ['',            '                } else {'],
    ['add_water_l', '                    water += leftMax - height[left];'],
    ['',            '                }'],
    ['inc_left',    '                left++;'],
    ['',            '            } else {'],
    ['if_rmax',     '                if (height[right] >= rightMax) {'],
    ['update_rmax', '                    rightMax = height[right];'],
    ['',            '                } else {'],
    ['add_water_r', '                    water += rightMax - height[right];'],
    ['',            '                }'],
    ['dec_right',   '                right--;'],
    ['',            '            }'],
    ['',            '        }'],
    ['return_stmt', '        return water;'],
    ['',            '    }'],
    ['',            ''],
    ['',            '    public static void main(String[] args) {'],
    ['m_scanner',   '        Scanner sc = new Scanner(System.in);'],
    ['m_read_n',    '        int n = sc.nextInt();'],
    ['m_alloc',     '        int[] height = new int[n];'],
    ['m_loop',      '        for (int i = 0; i < n; i++) {'],
    ['m_read_h',    '            height[i] = sc.nextInt();'],
    ['',            '        }'],
    ['m_call',      '        System.out.println(new Solution().trap(height));'],
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
    ['fn_entry',    '    int trap(vector<int>& height) {'],
    ['init_left',   '        int left = 0;'],
    ['init_right',  '        int right = height.size() - 1;'],
    ['init_lmax',   '        int leftMax = 0;'],
    ['init_rmax',   '        int rightMax = 0;'],
    ['init_water',  '        int water = 0;'],
    ['while_cond',  '        while (left < right) {'],
    ['if_side',     '            if (height[left] < height[right]) {'],
    ['if_lmax',     '                if (height[left] >= leftMax) {'],
    ['update_lmax', '                    leftMax = height[left];'],
    ['',            '                } else {'],
    ['add_water_l', '                    water += leftMax - height[left];'],
    ['',            '                }'],
    ['inc_left',    '                left++;'],
    ['',            '            } else {'],
    ['if_rmax',     '                if (height[right] >= rightMax) {'],
    ['update_rmax', '                    rightMax = height[right];'],
    ['',            '                } else {'],
    ['add_water_r', '                    water += rightMax - height[right];'],
    ['',            '                }'],
    ['dec_right',   '                right--;'],
    ['',            '            }'],
    ['',            '        }'],
    ['return_stmt', '        return water;'],
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
    ['m_call',      '    cout << Solution().trap(height) << endl;'],
    ['',            '    return 0;'],
    ['',            '}']
  ],
  python: [
    ['',            'class Solution:'],
    ['fn_entry',    '    def trap(self, height):'],
    ['init_left',   '        left = 0'],
    ['init_right',  '        right = len(height) - 1'],
    ['init_lmax',   '        left_max = 0'],
    ['init_rmax',   '        right_max = 0'],
    ['init_water',  '        water = 0'],
    ['while_cond',  '        while left < right:'],
    ['if_side',     '            if height[left] < height[right]:'],
    ['if_lmax',     '                if height[left] >= left_max:'],
    ['update_lmax', '                    left_max = height[left]'],
    ['',            '                else:'],
    ['add_water_l', '                    water += left_max - height[left]'],
    ['inc_left',    '                left += 1'],
    ['',            '            else:'],
    ['if_rmax',     '                if height[right] >= right_max:'],
    ['update_rmax', '                    right_max = height[right]'],
    ['',            '                else:'],
    ['add_water_r', '                    water += right_max - height[right]'],
    ['dec_right',   '                right -= 1'],
    ['return_stmt', '        return water'],
    ['',            ''],
    ['',            'def main():'],
    ['m_read_n',    '    n = int(input())'],
    ['m_alloc',     '    height = list(map(int, input().split()))'],
    ['m_call',      '    print(Solution().trap(height))'],
    ['',            ''],
    ['',            'if __name__ == "__main__":'],
    ['',            '    main()']
  ],
  javascript: [
    ['',            '/**'],
    ['',            ' * @param {number[]} height'],
    ['',            ' * @return {number}'],
    ['',            ' */'],
    ['fn_entry',    'var trap = function(height) {'],
    ['init_left',   '    let left = 0;'],
    ['init_right',  '    let right = height.length - 1;'],
    ['init_lmax',   '    let leftMax = 0;'],
    ['init_rmax',   '    let rightMax = 0;'],
    ['init_water',  '    let water = 0;'],
    ['while_cond',  '    while (left < right) {'],
    ['if_side',     '        if (height[left] < height[right]) {'],
    ['if_lmax',     '            if (height[left] >= leftMax) {'],
    ['update_lmax', '                leftMax = height[left];'],
    ['',            '            } else {'],
    ['add_water_l', '                water += leftMax - height[left];'],
    ['',            '            }'],
    ['inc_left',    '            left++;'],
    ['',            '        } else {'],
    ['if_rmax',     '            if (height[right] >= rightMax) {'],
    ['update_rmax', '                rightMax = height[right];'],
    ['',            '            } else {'],
    ['add_water_r', '                water += rightMax - height[right];'],
    ['',            '            }'],
    ['dec_right',   '            right--;'],
    ['',            '        }'],
    ['',            '    }'],
    ['return_stmt', '    return water;'],
    ['',            '};'],
    ['',            ''],
    ['',            'function main() {'],
    ['m_read_n',    '    const n = parseInt(tokens[0]);'],
    ['m_alloc',     '    const height = tokens.slice(1, n + 1).map(Number);'],
    ['m_call',      '    console.log(trap(height));'],
    ['',            '}'],
    ['',            'main();']
  ],
  c: [
    ['',            '#include <stdio.h>'],
    ['',            ''],
    ['fn_entry',    'int trap(int* height, int n) {'],
    ['init_left',   '    int left = 0;'],
    ['init_right',  '    int right = n - 1;'],
    ['init_lmax',   '    int leftMax = 0;'],
    ['init_rmax',   '    int rightMax = 0;'],
    ['init_water',  '    int water = 0;'],
    ['while_cond',  '    while (left < right) {'],
    ['if_side',     '        if (height[left] < height[right]) {'],
    ['if_lmax',     '            if (height[left] >= leftMax) {'],
    ['update_lmax', '                leftMax = height[left];'],
    ['',            '            } else {'],
    ['add_water_l', '                water += leftMax - height[left];'],
    ['',            '            }'],
    ['inc_left',    '            left++;'],
    ['',            '        } else {'],
    ['if_rmax',     '            if (height[right] >= rightMax) {'],
    ['update_rmax', '                rightMax = height[right];'],
    ['',            '            } else {'],
    ['add_water_r', '                water += rightMax - height[right];'],
    ['',            '            }'],
    ['dec_right',   '            right--;'],
    ['',            '        }'],
    ['',            '    }'],
    ['return_stmt', '    return water;'],
    ['',            '}'],
    ['',            ''],
    ['',            'int main() {'],
    ['m_read_n',    '    int n;'],
    ['m_scanner',   '    scanf("%d", &n);'],
    ['m_alloc',     '    int height[100];'],
    ['m_loop',      '    for (int i = 0; i < n; i++) {'],
    ['m_read_h',    '        scanf("%d", &height[i]);'],
    ['',            '    }'],
    ['m_call',      '    printf("%d\\n", trap(height, n));'],
    ['',            '    return 0;'],
    ['',            '}']
  ]
};

const PSEUDOCODE = [
  'function trap(height):',
  '    left     = 0',
  '    right    = len(height) - 1',
  '    leftMax  = 0',
  '    rightMax = 0',
  '    water    = 0',
  '',
  '    while left < right:',
  '        if height[left] < height[right]:',
  '            if height[left] >= leftMax:',
  '                leftMax = height[left]   // new left wall',
  '            else:',
  '                water += leftMax - height[left]  // trapped!',
  '            left += 1',
  '        else:',
  '            if height[right] >= rightMax:',
  '                rightMax = height[right]  // new right wall',
  '            else:',
  '                water += rightMax - height[right] // trapped!',
  '            right -= 1',
  '',
  '    return water'
];

// ─── Step Builder ─────────────────────────────────────────────────────────────
function buildSteps(rawHeight) {
  const steps = [];
  const height = rawHeight.slice(0, 12);
  const n = height.length;
  if (n < 2) {
    return steps;
  }
  const maxH = Math.max(...height, 1);

  const NONE = { leftReady: false, rightReady: false, lmaxReady: false, rmaxReady: false, waterReady: false };
  const ALL  = { leftReady: true,  rightReady: true,  lmaxReady: true,  rmaxReady: true,  waterReady: true  };

  function snap(lv, rv, lmaxV, rmaxV, wV, code, phase, initState, waterAt, extra) {
    return {
      left: lv,
      right: rv,
      leftMax: lmaxV,
      rightMax: rmaxV,
      waterTotal: wV,
      stepHeight: [...height],
      waterAt: [...waterAt],
      n,
      maxH,
      code,
      phase,
      initState: { ...initState },
      ...extra
    };
  }

  const emptyWater = new Array(n).fill(0);
  const partial    = new Array(n).fill(0);

  // ── Phase 1: main() — reading inputs ───────────────────────────────────────
  steps.push({
    ...snap(-1, -1, -1, -1, -1, 'm_scanner', 'input', NONE, emptyWater, { partial: [...partial] }),
    badge: `Scanner sc = new Scanner(System.in); → Initializing input stream in main().`
  });
  steps.push({
    ...snap(-1, -1, -1, -1, -1, 'm_read_n', 'input', NONE, emptyWater, { partial: [...partial] }),
    badge: `int n = sc.nextInt(); → Read n = ${n}. height[] will have ${n} elements.`
  });
  steps.push({
    ...snap(-1, -1, -1, -1, -1, 'm_alloc', 'input', NONE, emptyWater, { partial: [...partial] }),
    badge: `int[] height = new int[n]; → Allocated height[] with ${n} slots.`
  });

  for (let i = 0; i < n; i++) {
    steps.push({
      ...snap(-1, -1, -1, -1, -1, 'm_loop', 'input', NONE, emptyWater, { partial: [...partial], readIdx: i }),
      badge: `for (int i = ${i}; i < ${n}; i++) → i = ${i}. Will read height[${i}].`
    });
    partial[i] = height[i];
    steps.push({
      ...snap(-1, -1, -1, -1, -1, 'm_read_h', 'input', NONE, emptyWater, { partial: [...partial], readIdx: i, justReadIdx: i }),
      badge: `height[i] = sc.nextInt(); → height[${i}] = ${height[i]}. Stored at index ${i}.`
    });
  }
  steps.push({
    ...snap(-1, -1, -1, -1, -1, 'm_loop', 'input', NONE, emptyWater, { partial: [...partial], readIdx: n }),
    badge: `for (int i = ${n}; i < ${n}; i++) → i = ${n} is NOT less than n. Exit loop.`
  });
  steps.push({
    ...snap(-1, -1, -1, -1, -1, 'm_call', 'input', NONE, emptyWater, { partial: [...height] }),
    badge: `System.out.println(new Solution().trap(height)); → All input read. Calling trap() with n=${n} elements.`
  });

  // ── Phase 2: trap() function ──────────────────────────────────────────────
  const INIT_ENTRY  = { leftReady: false, rightReady: false, lmaxReady: false, rmaxReady: false, waterReady: false };
  const AFTER_LEFT  = { leftReady: true,  rightReady: false, lmaxReady: false, rmaxReady: false, waterReady: false };
  const AFTER_RIGHT = { leftReady: true,  rightReady: true,  lmaxReady: false, rmaxReady: false, waterReady: false };
  const AFTER_LMAX  = { leftReady: true,  rightReady: true,  lmaxReady: true,  rmaxReady: false, waterReady: false };
  const AFTER_RMAX  = { leftReady: true,  rightReady: true,  lmaxReady: true,  rmaxReady: true,  waterReady: false };

  const waterAt = new Array(n).fill(0);

  steps.push({
    ...snap(-1, -1, -1, -1, -1, 'fn_entry', 'init', INIT_ENTRY, [...waterAt], {}),
    badge: `public int trap(int[] height) → Function entry. Two-pointer approach starting from both ends.`
  });
  steps.push({
    ...snap(0, -1, -1, -1, -1, 'init_left', 'init', AFTER_LEFT, [...waterAt], {}),
    badge: `int left = 0; → left = 0. Left pointer starts at index 0.`
  });
  steps.push({
    ...snap(0, n - 1, -1, -1, -1, 'init_right', 'init', AFTER_RIGHT, [...waterAt], {}),
    badge: `int right = height.length - 1; → right = ${n - 1}. Right pointer starts at last index.`
  });
  steps.push({
    ...snap(0, n - 1, 0, -1, -1, 'init_lmax', 'init', AFTER_LMAX, [...waterAt], {}),
    badge: `int leftMax = 0; → leftMax = 0. Tracks the highest bar seen from the left.`
  });
  steps.push({
    ...snap(0, n - 1, 0, 0, -1, 'init_rmax', 'init', AFTER_RMAX, [...waterAt], {}),
    badge: `int rightMax = 0; → rightMax = 0. Tracks the highest bar seen from the right.`
  });
  steps.push({
    ...snap(0, n - 1, 0, 0, 0, 'init_water', 'init', ALL, [...waterAt], {}),
    badge: `int water = 0; → water = 0. Accumulates total trapped water.`
  });

  let left = 0;
  let right = n - 1;
  let leftMax = 0;
  let rightMax = 0;
  let water = 0;

  while (left < right) {
    steps.push({
      ...snap(left, right, leftMax, rightMax, water, 'while_cond', 'solve', ALL, [...waterAt], {}),
      badge: `while (left < right): left=${left} < right=${right} → TRUE. Continue scanning.`
    });

    const leftSmaller = height[left] < height[right];
    steps.push({
      ...snap(left, right, leftMax, rightMax, water, 'if_side', 'solve', ALL, [...waterAt], { leftSmaller }),
      badge: `if (height[left] < height[right]): height[${left}]=${height[left]} < height[${right}]=${height[right]} → ${leftSmaller ? 'TRUE. Process left side.' : 'FALSE. Process right side (else branch).'}`
    });

    if (leftSmaller) {
      const lmaxUpdate = height[left] >= leftMax;
      steps.push({
        ...snap(left, right, leftMax, rightMax, water, 'if_lmax', 'solve', ALL, [...waterAt], { lmaxUpdate }),
        badge: `if (height[left] >= leftMax): height[${left}]=${height[left]} >= leftMax=${leftMax} → ${lmaxUpdate ? 'TRUE. Update leftMax (new left wall).' : 'FALSE. This bar traps water.'}`
      });

      if (lmaxUpdate) {
        leftMax = height[left];
        waterAt[left] = 0;
        steps.push({
          ...snap(left, right, leftMax, rightMax, water, 'update_lmax', 'solve', ALL, [...waterAt], {}),
          badge: `leftMax = height[left]; → leftMax = ${leftMax}. Bar at [${left}] is the new left wall. No water here.`
        });
      } else {
        const trapped = leftMax - height[left];
        water += trapped;
        waterAt[left] = trapped;
        steps.push({
          ...snap(left, right, leftMax, rightMax, water, 'add_water_l', 'solve', ALL, [...waterAt], { justWatered: left }),
          badge: `water += leftMax - height[left]; → ${leftMax} - ${height[left]} = ${trapped} units trapped at [${left}]. Total water = ${water}.`
        });
      }

      left++;
      steps.push({
        ...snap(left, right, leftMax, rightMax, water, 'inc_left', 'solve', ALL, [...waterAt], {}),
        badge: `left++; → left = ${left}. Move left pointer right.`
      });
    } else {
      const rmaxUpdate = height[right] >= rightMax;
      steps.push({
        ...snap(left, right, leftMax, rightMax, water, 'if_rmax', 'solve', ALL, [...waterAt], { rmaxUpdate }),
        badge: `if (height[right] >= rightMax): height[${right}]=${height[right]} >= rightMax=${rightMax} → ${rmaxUpdate ? 'TRUE. Update rightMax (new right wall).' : 'FALSE. This bar traps water.'}`
      });

      if (rmaxUpdate) {
        rightMax = height[right];
        waterAt[right] = 0;
        steps.push({
          ...snap(left, right, leftMax, rightMax, water, 'update_rmax', 'solve', ALL, [...waterAt], {}),
          badge: `rightMax = height[right]; → rightMax = ${rightMax}. Bar at [${right}] is the new right wall. No water here.`
        });
      } else {
        const trapped = rightMax - height[right];
        water += trapped;
        waterAt[right] = trapped;
        steps.push({
          ...snap(left, right, leftMax, rightMax, water, 'add_water_r', 'solve', ALL, [...waterAt], { justWatered: right }),
          badge: `water += rightMax - height[right]; → ${rightMax} - ${height[right]} = ${trapped} units trapped at [${right}]. Total water = ${water}.`
        });
      }

      right--;
      steps.push({
        ...snap(left, right, leftMax, rightMax, water, 'dec_right', 'solve', ALL, [...waterAt], {}),
        badge: `right--; → right = ${right}. Move right pointer left.`
      });
    }
  }

  steps.push({
    ...snap(left, right, leftMax, rightMax, water, 'while_cond', 'done', ALL, [...waterAt], {}),
    badge: `while (left < right): left=${left}, right=${right} → FALSE. Pointers met. All bars processed.`
  });
  steps.push({
    ...snap(left, right, leftMax, rightMax, water, 'return_stmt', 'done', ALL, [...waterAt], {}),
    badge: `return water; → Answer = ${water}. Total trapped rain water = ${water} units.`
  });

  return steps;
}

// ─── Reactive State ───────────────────────────────────────────────────────────
const DEFAULT_HEIGHT  = [0, 1, 0, 2, 1, 0, 1, 3, 2, 1, 2, 1];
const inputHeightStr  = ref('0,1,0,2,1,0,1,3,2,1,2,1');
const lang            = ref('java');
const speed           = ref(650);
const si              = ref(0);
const playing         = ref(false);
const vizHeight       = ref(370);
const tableHeight     = ref(60);
const leftWidth       = ref(52);
const rightTab        = ref('code');

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
  if (arr.length > 12) {
    alert('Maximum 12 values allowed for visualization.');
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
const initState    = computed(() => s.value.initState || { leftReady: false, rightReady: false, lmaxReady: false, rmaxReady: false, waterReady: false });
const displayLeft  = computed(() => initState.value.leftReady  ? s.value.left       : '?');
const displayRight = computed(() => initState.value.rightReady ? s.value.right      : '?');
const displayLmax  = computed(() => initState.value.lmaxReady  ? s.value.leftMax    : '?');
const displayRmax  = computed(() => initState.value.rmaxReady  ? s.value.rightMax   : '?');
const displayWater = computed(() => initState.value.waterReady ? s.value.waterTotal : '?');
const displayBars  = computed(() => s.value.phase === 'input' ? (s.value.partial || []) : (s.value.stepHeight || []));
const displayWaterAt = computed(() => s.value.waterAt || []);
const displayMaxH  = computed(() => s.value.maxH || 1);
const displayN     = computed(() => s.value.n || 0);

const BAR_MAX_PX = 130;

function barPx(val) {
  const mh = displayMaxH.value;
  return mh > 0 ? Math.max(4, Math.round((val / mh) * BAR_MAX_PX)) : 4;
}
function waterPx(idx) {
  const mh = displayMaxH.value;
  const wa = displayWaterAt.value[idx] || 0;
  return mh > 0 && wa > 0 ? Math.max(0, Math.round((wa / mh) * BAR_MAX_PX)) : 0;
}

function barClass(idx) {
  const phase = s.value.phase;
  const ist   = initState.value;
  const left  = s.value.left;
  const right = s.value.right;
  const classes = [];
  if (phase === 'input') {
    if (s.value.justReadIdx === idx) {
      classes.push('trw-bar-just-read');
    }
    return classes.join(' ');
  }
  if (phase === 'done') {
    classes.push('trw-bar-done');
    return classes.join(' ');
  }
  if (!ist.leftReady || !ist.rightReady) {
    return classes.join(' ');
  }
  if (idx === left) {
    classes.push('trw-bar-left');
  } else if (idx === right) {
    classes.push('trw-bar-right');
  } else if (idx < left) {
    classes.push('trw-bar-processed-l');
  } else if (idx > right) {
    classes.push('trw-bar-processed-r');
  } else {
    classes.push('trw-bar-inner');
  }
  if (s.value.justWatered === idx) {
    classes.push('trw-bar-watered');
  }
  return classes.join(' ');
}

// leftMax/rightMax pixel level for indicator line inside chart
const leftMaxLinePx  = computed(() => {
  if (!initState.value.lmaxReady || s.value.leftMax <= 0) {
    return 0;
  }
  return barPx(s.value.leftMax);
});
const rightMaxLinePx = computed(() => {
  if (!initState.value.rmaxReady || s.value.rightMax <= 0) {
    return 0;
  }
  return barPx(s.value.rightMax);
});

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
              <label>height[]:</label>
              <input
                type="text"
                v-model="inputHeightStr"
                class="ll-text-input trw-arr-input"
                @keyup.enter="applyInput"
                placeholder="e.g. 0,1,0,2,1,0,1,3,2,1,2,1"
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
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">leftMax</span><b class="ll-c-purple">{{ displayLmax }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">rightMax</span><b class="ll-c-orange">{{ displayRmax }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">water</span><b class="ll-c-water">{{ displayWater }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">phase</span><b class="ll-c-orange">{{ s.phase || 'input' }}</b></span>
                  </div>

                  <!-- BOARD CONTAINER -->
                  <div class="ll-board-container">

                    <!-- Tier 1 — Terrain + Water Bar Chart -->
                    <div class="trw-tier-title">
                      Tier 1 &mdash; height[] Terrain &amp; Trapped Water
                      <code>n = {{ displayN }}</code>
                    </div>
                    <div class="trw-chart-frame">
                      <!-- Pointer label row above bars -->
                      <div class="trw-ptr-row">
                        <div
                          v-for="(val, idx) in displayBars"
                          :key="'ptr-' + idx"
                          class="trw-ptr-cell"
                        >
                          <span
                            v-if="initState.leftReady && idx === s.left && (s.phase === 'solve' || s.phase === 'init' || s.phase === 'done')"
                            class="trw-ptr-label trw-ptr-left"
                          >left</span>
                          <span
                            v-if="initState.rightReady && idx === s.right && (s.phase === 'solve' || s.phase === 'init' || s.phase === 'done')"
                            class="trw-ptr-label trw-ptr-right"
                          >right</span>
                        </div>
                      </div>

                      <!-- Bar + Water columns -->
                      <div class="trw-bar-row">
                        <div
                          v-for="(val, idx) in displayBars"
                          :key="'col-' + idx"
                          class="trw-bar-col"
                        >
                          <!-- Fixed-height slot with absolute-positioned bar and water -->
                          <div class="trw-bar-slot">
                            <!-- Water fill (above the bar) -->
                            <div
                              v-if="waterPx(idx) > 0"
                              class="trw-water-fill"
                              :style="{
                                bottom: barPx(val) + 'px',
                                height: waterPx(idx) + 'px'
                              }"
                            ></div>
                            <!-- Terrain bar -->
                            <div
                              class="trw-bar"
                              :class="barClass(idx)"
                              :style="{ height: barPx(val) + 'px' }"
                            >
                              <span class="trw-bar-val">{{ val }}</span>
                            </div>
                          </div>
                          <span class="trw-bar-idx">[{{ idx }}]</span>
                        </div>
                      </div>

                      <!-- leftMax / rightMax indicator lines -->
                      <div class="trw-max-lines-wrap" v-if="s.phase === 'solve' || s.phase === 'done'">
                        <div
                          v-if="leftMaxLinePx > 0"
                          class="trw-max-line trw-lmax-line"
                          :style="{ bottom: leftMaxLinePx + 'px' }"
                          :title="`leftMax = ${s.leftMax}`"
                        >
                          <span class="trw-max-label">lMax={{ s.leftMax }}</span>
                        </div>
                        <div
                          v-if="rightMaxLinePx > 0"
                          class="trw-max-line trw-rmax-line"
                          :style="{ bottom: rightMaxLinePx + 'px' }"
                          :title="`rightMax = ${s.rightMax}`"
                        >
                          <span class="trw-max-label trw-max-label-r">rMax={{ s.rightMax }}</span>
                        </div>
                      </div>
                    </div>

                    <!-- Tier 2 — Variable State Panel -->
                    <div class="trw-tier-title">Tier 2 &mdash; Variable State</div>
                    <div class="trw-var-panel">
                      <div class="trw-var-card" :class="{ 'trw-var-active': initState.leftReady }">
                        <span class="trw-var-name trw-ptr-left">left</span>
                        <span class="trw-var-val">{{ !initState.leftReady ? '?' : s.left }}</span>
                        <span class="trw-var-desc">left pointer</span>
                      </div>
                      <div class="trw-var-card" :class="{ 'trw-var-active': initState.rightReady }">
                        <span class="trw-var-name trw-ptr-right">right</span>
                        <span class="trw-var-val">{{ !initState.rightReady ? '?' : s.right }}</span>
                        <span class="trw-var-desc">right pointer</span>
                      </div>
                      <div class="trw-var-card" :class="{ 'trw-var-active': initState.lmaxReady }">
                        <span class="trw-var-name" style="color:var(--purple);background:#f3e8ff;border:1px solid #d8b4fe;">leftMax</span>
                        <span class="trw-var-val">{{ !initState.lmaxReady ? '?' : s.leftMax }}</span>
                        <span class="trw-var-desc">left wall</span>
                      </div>
                      <div class="trw-var-card" :class="{ 'trw-var-active': initState.rmaxReady }">
                        <span class="trw-var-name" style="color:var(--orange);background:#fff7ed;border:1px solid #fdba74;">rightMax</span>
                        <span class="trw-var-val">{{ !initState.rmaxReady ? '?' : s.rightMax }}</span>
                        <span class="trw-var-desc">right wall</span>
                      </div>
                      <div class="trw-var-card" :class="{ 'trw-var-active': initState.waterReady }">
                        <span class="trw-var-name" style="color:#0369a1;background:#e0f2fe;border:1px solid #7dd3fc;">water</span>
                        <span class="trw-var-val trw-water-val">{{ !initState.waterReady ? '?' : s.waterTotal }}</span>
                        <span class="trw-var-desc">total trapped</span>
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
                <span class="ll-leg"><span class="ll-legdot" style="background:#bfdbfe;border:1.5px solid #3b82f6;"></span>left</span>
                <span class="ll-leg"><span class="ll-legdot" style="background:#bbf7d0;border:1.5px solid #22c55e;"></span>right</span>
                <span class="ll-leg"><span class="ll-legdot" style="background:#ddd6fe;border:1.5px dashed #8b5cf6;"></span>leftMax</span>
                <span class="ll-leg"><span class="ll-legdot" style="background:#fed7aa;border:1.5px dashed #f97316;"></span>rightMax</span>
                <span class="ll-leg"><span class="ll-legdot trw-legdot-water"></span>water</span>
                <span class="ll-leg"><span class="ll-legdot" style="background:#dcfce7;border:1.5px solid #86efac;"></span>done</span>
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
                      trap(height, n={{ displayN }})
                      &nbsp;&nbsp;
                      <span class="ll-fname">left</span>=<span class="ll-c-blue" style="font-weight:700">{{ displayLeft }}</span>,
                      <span class="ll-fname">right</span>=<span class="ll-c-green" style="font-weight:700">{{ displayRight }}</span>,
                      <span class="ll-fname">leftMax</span>=<span class="ll-c-purple" style="font-weight:700">{{ displayLmax }}</span>,
                      <span class="ll-fname">rightMax</span>=<span class="ll-c-orange" style="font-weight:700">{{ displayRmax }}</span>,
                      <span class="ll-fname">water</span>=<span style="color:#0369a1;font-weight:700">{{ displayWater }}</span>
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
                    'll-badge-error':   s.badge && (s.badge.includes('FALSE') || s.badge.includes('NOT')),
                    'll-badge-success': s.badge && (s.badge.includes('Answer') || s.badge.includes('TRUE') || s.badge.includes('trapped!') || s.badge.includes('trapped'))
                  }"
                >
                  {{ s.badge || 'Ready to visualize Trapping Rain Water.' }}
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
                  <h3 class="ll-cx-heading">Trapping Rain Water (LeetCode 42) &mdash; Complexity Analysis</h3>
                  <p class="ll-cx-intro">
                    Given an array <code>height</code> representing terrain bars, compute how much water can be trapped
                    between bars after rain. The optimal approach uses <strong>two pointers</strong>: <code>left</code>
                    and <code>right</code> move inward, always processing the side with the shorter current max wall.
                    Water trapped at any bar = max wall on the constraining side &minus; bar height.
                  </p>

                  <h4 class="ll-cx-sub">Step-by-step Breakdown</h4>
                  <table class="ll-complexity-table">
                    <thead>
                      <tr><th>Operation</th><th>Time</th><th>Space</th><th>Reason</th></tr>
                    </thead>
                    <tbody>
                      <tr><td>Initialize 5 variables</td><td class="ll-cx-good">O(1)</td><td class="ll-cx-good">O(1)</td><td>Constant-time assignments</td></tr>
                      <tr><td>while loop (two-pointer)</td><td class="ll-cx-good">O(n)</td><td class="ll-cx-good">O(1)</td><td>Each index visited at most once (left or right moves each step)</td></tr>
                      <tr><td>Per-iteration work</td><td class="ll-cx-good">O(1)</td><td class="ll-cx-good">O(1)</td><td>Two comparisons, optional addition</td></tr>
                      <tr><td><strong>Total</strong></td><td class="ll-cx-good"><strong>O(n)</strong></td><td class="ll-cx-good"><strong>O(1)</strong></td><td>Single-pass, in-place</td></tr>
                    </tbody>
                  </table>

                  <h4 class="ll-cx-sub">Overall Complexity</h4>
                  <div class="ll-cx-summary-grid">
                    <div class="ll-cx-card ll-cx-card-good">
                      <div class="ll-cx-card-label">Time</div>
                      <div class="ll-cx-card-val">O(n)</div>
                      <div class="ll-cx-card-note">Each bar processed exactly once</div>
                    </div>
                    <div class="ll-cx-card ll-cx-card-good">
                      <div class="ll-cx-card-label">Space</div>
                      <div class="ll-cx-card-val">O(1)</div>
                      <div class="ll-cx-card-note">Only 5 integers — no prefix arrays</div>
                    </div>
                    <div class="ll-cx-card ll-cx-card-good">
                      <div class="ll-cx-card-label">vs DP</div>
                      <div class="ll-cx-card-val">Better</div>
                      <div class="ll-cx-card-note">DP uses O(n) space for prefix max arrays</div>
                    </div>
                  </div>

                  <div class="ll-note">
                    <strong>Key Insight:</strong> When <code>height[left] &lt; height[right]</code>, the amount of water
                    at <code>left</code> is determined by <code>leftMax</code> (the shorter wall), regardless of what's
                    on the right &mdash; because we know <code>height[right]</code> is already taller. So we can safely
                    compute and move. The symmetric argument applies on the right side.
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

@keyframes trw-flash-read    { 0% { background:#fef9c3; } 60% { background:#86efac; } 100% { background:#f0fdf4; } }
@keyframes trw-water-ripple  { 0% { opacity:.45; } 50% { opacity:.9; } 100% { opacity:.6; } }
@keyframes trw-water-flash   { 0% { background:rgba(56,189,248,.3); } 50% { background:rgba(56,189,248,.85); } 100% { background:rgba(56,189,248,.5); } }

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
.trw-arr-input   { width: 290px; }
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
.ll-c-water  { color: #0369a1; font-weight: 700; }

/* ─── Board Container — Trapping Rain Water ──────────────────────────────── */
.ll-board-container { display: flex; flex-direction: column; align-items: flex-start; padding: 6px 14px 10px; gap: 8px; min-width: 0; }

.trw-tier-title {
  font-size: 10px; font-weight: 700; text-transform: uppercase; letter-spacing: .04em;
  color: var(--muted); margin-top: 4px; margin-bottom: 2px; font-family: 'Consolas', monospace;
  border-left: 3px solid var(--coral); padding-left: 6px;
}

/* Chart frame */
.trw-chart-frame {
  display: flex; flex-direction: column; position: relative;
  background: #f8fafc; border: 1px solid var(--border);
  border-radius: var(--radius); padding: 8px 12px 10px; box-shadow: var(--shadow-sm); gap: 4px;
}

/* Pointer label row */
.trw-ptr-row  { display: flex; gap: 3px; height: 18px; align-items: flex-end; }
.trw-ptr-cell { flex: 1; min-width: 28px; max-width: 48px; display: flex; justify-content: center; align-items: flex-end; gap: 2px; }
.trw-ptr-label { font-size: 8.5px; font-weight: 800; font-family: monospace; padding: 1px 3px; border-radius: 3px; line-height: 1.2; white-space: nowrap; }
.trw-ptr-left  { background: #eff6ff; color: #1d4ed8; border: 1px solid #93c5fd; }
.trw-ptr-right { background: #f0fdf4; color: #15803d; border: 1px solid #86efac; }

/* Bar row */
.trw-bar-row { display: flex; gap: 3px; align-items: flex-end; }
.trw-bar-col { display: flex; flex-direction: column; align-items: center; flex: 1; min-width: 24px; max-width: 48px; }

/* Fixed-height slot for bar + water */
.trw-bar-slot {
  position: relative;
  width: 100%;
  height: 146px; /* BAR_MAX_PX + 16 buffer */
}

/* Water fill (absolutely positioned above bar) */
.trw-water-fill {
  position: absolute;
  left: 5%; right: 5%;
  background: rgba(56, 189, 248, 0.45);
  border-top: 2px solid rgba(14, 165, 233, 0.7);
  border-left: 1px solid rgba(56, 189, 248, 0.4);
  border-right: 1px solid rgba(56, 189, 248, 0.4);
  border-radius: 2px 2px 0 0;
  animation: trw-water-ripple 1.6s ease-in-out infinite;
  z-index: 2;
  transition: height .25s ease, bottom .25s ease;
}

/* Terrain bar */
.trw-bar {
  position: absolute;
  bottom: 0;
  left: 10%; width: 80%;
  border-radius: 3px 3px 0 0;
  background: #94a3b8;
  border: 1.5px solid #64748b;
  display: flex; align-items: flex-start; justify-content: center;
  z-index: 3;
  transition: all .2s ease;
  min-height: 4px;
}
.trw-bar-val {
  font-size: 8.5px; font-weight: 800; font-family: monospace;
  color: var(--text); padding-top: 1px; line-height: 1;
}
.trw-bar-idx {
  font-size: 7.5px; color: var(--muted); font-family: monospace; font-weight: 700; margin-top: 3px;
}

/* Bar state classes */
.trw-bar-left        { background: #bfdbfe !important; border: 2px solid var(--blue) !important; box-shadow: 0 0 7px rgba(59,130,246,.4); }
.trw-bar-right       { background: #bbf7d0 !important; border: 2px solid var(--green) !important; box-shadow: 0 0 7px rgba(34,197,94,.4); }
.trw-bar-processed-l { background: #ddd6fe !important; border: 1.5px solid #a78bfa !important; opacity: .85; }
.trw-bar-processed-r { background: #fed7aa !important; border: 1.5px solid #fb923c !important; opacity: .85; }
.trw-bar-inner       { background: #e2e8f0 !important; border: 1.5px dashed #94a3b8 !important; }
.trw-bar-just-read   { background: #dcfce7 !important; border: 2px solid #10b981 !important; animation: trw-flash-read .45s ease; }
.trw-bar-watered     { animation: trw-water-flash .55s ease; }
.trw-bar-done        { background: #dcfce7 !important; border: 1.5px solid #86efac !important; }

/* leftMax / rightMax indicator lines */
.trw-max-lines-wrap { position: absolute; left: 12px; right: 12px; bottom: 34px; height: 146px; pointer-events: none; }
.trw-max-line {
  position: absolute; left: 0; right: 0; height: 0;
  border-top: 1.5px dashed;
  z-index: 4; transition: bottom .2s ease;
}
.trw-lmax-line { border-color: #8b5cf6; }
.trw-rmax-line { border-color: #f97316; }
.trw-max-label {
  position: absolute; left: 2px; top: -14px;
  font-size: 8px; font-weight: 800; font-family: monospace;
  color: #7c3aed; background: #f3e8ff; border: 1px solid #c4b5fd;
  padding: 1px 4px; border-radius: 3px; white-space: nowrap;
}
.trw-max-label-r { color: #c2410c; background: #fff7ed; border-color: #fdba74; left: auto; right: 2px; }

/* Variable State Panel */
.trw-var-panel { display: flex; gap: 6px; flex-wrap: wrap; }
.trw-var-card {
  display: flex; flex-direction: column; align-items: center; gap: 2px;
  padding: 5px 10px; border-radius: var(--radius-sm); border: 1.5px solid var(--border);
  background: var(--surface2); min-width: 72px; font-size: 10.5px; font-family: monospace;
  transition: all .15s;
}
.trw-var-active { border-color: var(--orange) !important; background: var(--orange-light) !important; }
.trw-var-name { font-size: 9.5px; font-weight: 800; padding: 1px 5px; border-radius: 3px; }
.trw-var-val  { font-size: 17px; font-weight: 900; color: var(--text); line-height: 1.2; }
.trw-var-desc { font-size: 8.5px; color: var(--muted); text-align: center; }
.trw-water-val { color: #0369a1; }

/* Legend */
.trw-legdot-water { background: rgba(56,189,248,.5); border: 1.5px solid #38bdf8; display: inline-block; width: 11px; height: 11px; border-radius: 3px; }

/* Shared helpers */
.ll-vresizer { height: 5px; cursor: row-resize; background: var(--border); flex-shrink: 0; transition: background .15s; position: relative; z-index: 20; }
.ll-vresizer:hover, .ll-vresizer.drag { background: var(--coral); }
.ll-legend { display: flex; flex-wrap: wrap; gap: 6px 14px; padding: 6px 12px; border-bottom: 1px solid var(--border); flex-shrink: 0; background: var(--surface2); }
.ll-leg    { display: flex; align-items: center; gap: 5px; font-size: 11px; color: var(--text2); font-weight: 500; }
.ll-legdot { width: 11px; height: 11px; border-radius: 3px; flex-shrink: 0; display: inline-block; }

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
</style>
