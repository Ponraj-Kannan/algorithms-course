<script setup>
import { ref, reactive, computed, onMounted, onBeforeUnmount, watch, nextTick } from 'vue';

defineProps({
  topic: { type: String, default: 'Array Algorithms' },
  subTopic: { type: String, default: 'Merge Sorted Array (LeetCode 88)' }
});

// ─── Multi-Language Code Definitions ─────────────────────────────────────────
const CODES = {
  java: [
    ['',           'import java.util.Scanner;'],
    ['',           ''],
    ['',           'class Solution {'],
    ['fn_entry',   '    public void merge(int[] nums1, int m, int[] nums2, int n) {'],
    ['init_p1',    '        int p1 = m - 1;'],
    ['init_p2',    '        int p2 = n - 1;'],
    ['init_p',     '        int p = m + n - 1;'],
    ['while_cond', '        while (p1 >= 0 && p2 >= 0) {'],
    ['if_cmp',     '            if (nums1[p1] > nums2[p2]) {'],
    ['place_n1',   '                nums1[p] = nums1[p1];'],
    ['dec_p1',     '                p1--;'],
    ['',           '            } else {'],
    ['place_n2',   '                nums1[p] = nums2[p2];'],
    ['dec_p2',     '                p2--;'],
    ['',           '            }'],
    ['dec_p',      '            p--;'],
    ['',           '        }'],
    ['tail_while', '        while (p2 >= 0) {'],
    ['tail_place', '            nums1[p] = nums2[p2];'],
    ['tail_p2',    '            p2--;'],
    ['tail_p',     '            p--;'],
    ['',           '        }'],
    ['',           '    }'],
    ['',           ''],
    ['',           '    public static void main(String[] args) {'],
    ['m_scanner',  '        Scanner sc = new Scanner(System.in);'],
    ['m_read_m',   '        int m = sc.nextInt();'],
    ['m_read_n',   '        int n = sc.nextInt();'],
    ['m_alloc_n1', '        int[] nums1 = new int[m + n];'],
    ['m_loop_n1',  '        for (int i = 0; i < m; i++) {'],
    ['m_read_n1',  '            nums1[i] = sc.nextInt();'],
    ['',           '        }'],
    ['m_alloc_n2', '        int[] nums2 = new int[n];'],
    ['m_loop_n2',  '        for (int i = 0; i < n; i++) {'],
    ['m_read_n2',  '            nums2[i] = sc.nextInt();'],
    ['',           '        }'],
    ['m_call',     '        new Solution().merge(nums1, m, nums2, n);'],
    ['',           '    }'],
    ['',           '}']
  ],
  cpp: [
    ['',           '#include <vector>'],
    ['',           '#include <iostream>'],
    ['',           'using namespace std;'],
    ['',           ''],
    ['',           'class Solution {'],
    ['',           'public:'],
    ['fn_entry',   '    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {'],
    ['init_p1',    '        int p1 = m - 1;'],
    ['init_p2',    '        int p2 = n - 1;'],
    ['init_p',     '        int p = m + n - 1;'],
    ['while_cond', '        while (p1 >= 0 && p2 >= 0) {'],
    ['if_cmp',     '            if (nums1[p1] > nums2[p2]) {'],
    ['place_n1',   '                nums1[p] = nums1[p1];'],
    ['dec_p1',     '                p1--;'],
    ['',           '            } else {'],
    ['place_n2',   '                nums1[p] = nums2[p2];'],
    ['dec_p2',     '                p2--;'],
    ['',           '            }'],
    ['dec_p',      '            p--;'],
    ['',           '        }'],
    ['tail_while', '        while (p2 >= 0) {'],
    ['tail_place', '            nums1[p] = nums2[p2];'],
    ['tail_p2',    '            p2--;'],
    ['tail_p',     '            p--;'],
    ['',           '        }'],
    ['',           '    }'],
    ['',           '};'],
    ['',           ''],
    ['',           'int main() {'],
    ['m_read_m',   '    int m;'],
    ['m_read_n',   '    int n;'],
    ['m_scanner',  '    cin >> m >> n;'],
    ['m_alloc_n1', '    vector<int> nums1(m + n, 0);'],
    ['m_loop_n1',  '    for (int i = 0; i < m; i++) {'],
    ['m_read_n1',  '        cin >> nums1[i];'],
    ['',           '    }'],
    ['m_alloc_n2', '    vector<int> nums2(n);'],
    ['m_loop_n2',  '    for (int i = 0; i < n; i++) {'],
    ['m_read_n2',  '        cin >> nums2[i];'],
    ['',           '    }'],
    ['m_call',     '    Solution().merge(nums1, m, nums2, n);'],
    ['',           '    return 0;'],
    ['',           '}']
  ],
  python: [
    ['',           'class Solution:'],
    ['fn_entry',   '    def merge(self, nums1, m, nums2, n):'],
    ['init_p1',    '        p1 = m - 1'],
    ['init_p2',    '        p2 = n - 1'],
    ['init_p',     '        p = m + n - 1'],
    ['while_cond', '        while p1 >= 0 and p2 >= 0:'],
    ['if_cmp',     '            if nums1[p1] > nums2[p2]:'],
    ['place_n1',   '                nums1[p] = nums1[p1]'],
    ['dec_p1',     '                p1 -= 1'],
    ['',           '            else:'],
    ['place_n2',   '                nums1[p] = nums2[p2]'],
    ['dec_p2',     '                p2 -= 1'],
    ['dec_p',      '            p -= 1'],
    ['tail_while', '        while p2 >= 0:'],
    ['tail_place', '            nums1[p] = nums2[p2]'],
    ['tail_p2',    '            p2 -= 1'],
    ['tail_p',     '            p -= 1'],
    ['',           ''],
    ['',           'def main():'],
    ['m_scanner',  '    data = input().split()'],
    ['m_read_m',   '    m = int(data[0])'],
    ['m_read_n',   '    n = int(data[1])'],
    ['m_alloc_n1', '    nums1 = list(map(int, input().split())) + [0] * n'],
    ['m_alloc_n2', '    nums2 = list(map(int, input().split()))'],
    ['m_call',     '    Solution().merge(nums1, m, nums2, n)'],
    ['',           ''],
    ['',           'if __name__ == "__main__":'],
    ['',           '    main()']
  ],
  javascript: [
    ['',           '/**'],
    ['',           ' * @param {number[]} nums1'],
    ['',           ' * @param {number} m'],
    ['',           ' * @param {number[]} nums2'],
    ['',           ' * @param {number} n'],
    ['',           ' * @return {void}'],
    ['',           ' */'],
    ['fn_entry',   'var merge = function(nums1, m, nums2, n) {'],
    ['init_p1',    '    let p1 = m - 1;'],
    ['init_p2',    '    let p2 = n - 1;'],
    ['init_p',     '    let p = m + n - 1;'],
    ['while_cond', '    while (p1 >= 0 && p2 >= 0) {'],
    ['if_cmp',     '        if (nums1[p1] > nums2[p2]) {'],
    ['place_n1',   '            nums1[p] = nums1[p1];'],
    ['dec_p1',     '            p1--;'],
    ['',           '        } else {'],
    ['place_n2',   '            nums1[p] = nums2[p2];'],
    ['dec_p2',     '            p2--;'],
    ['',           '        }'],
    ['dec_p',      '        p--;'],
    ['',           '    }'],
    ['tail_while', '    while (p2 >= 0) {'],
    ['tail_place', '        nums1[p] = nums2[p2];'],
    ['tail_p2',    '        p2--;'],
    ['tail_p',     '        p--;'],
    ['',           '    }'],
    ['',           '};'],
    ['',           ''],
    ['',           'function main() {'],
    ['m_read_m',   '    const m = parseInt(tokens[0]);'],
    ['m_read_n',   '    const n = parseInt(tokens[1]);'],
    ['m_alloc_n1', '    const nums1 = new Array(m + n).fill(0);'],
    ['m_loop_n1',  '    for (let i = 0; i < m; i++) {'],
    ['m_read_n1',  '        nums1[i] = parseInt(tokens[2 + i]);'],
    ['',           '    }'],
    ['m_alloc_n2', '    const nums2 = new Array(n).fill(0);'],
    ['m_loop_n2',  '    for (let i = 0; i < n; i++) {'],
    ['m_read_n2',  '        nums2[i] = parseInt(tokens[2 + m + i]);'],
    ['',           '    }'],
    ['m_call',     '    merge(nums1, m, nums2, n);'],
    ['',           '}'],
    ['',           'main();']
  ],
  c: [
    ['',           '#include <stdio.h>'],
    ['',           ''],
    ['fn_entry',   'void merge(int* nums1, int m, int* nums2, int n) {'],
    ['init_p1',    '    int p1 = m - 1;'],
    ['init_p2',    '    int p2 = n - 1;'],
    ['init_p',     '    int p = m + n - 1;'],
    ['while_cond', '    while (p1 >= 0 && p2 >= 0) {'],
    ['if_cmp',     '        if (nums1[p1] > nums2[p2]) {'],
    ['place_n1',   '            nums1[p] = nums1[p1];'],
    ['dec_p1',     '            p1--;'],
    ['',           '        } else {'],
    ['place_n2',   '            nums1[p] = nums2[p2];'],
    ['dec_p2',     '            p2--;'],
    ['',           '        }'],
    ['dec_p',      '        p--;'],
    ['',           '    }'],
    ['tail_while', '    while (p2 >= 0) {'],
    ['tail_place', '        nums1[p] = nums2[p2];'],
    ['tail_p2',    '        p2--;'],
    ['tail_p',     '        p--;'],
    ['',           '    }'],
    ['',           '}'],
    ['',           ''],
    ['',           'int main() {'],
    ['m_read_m',   '    int m;'],
    ['m_read_n',   '    int n;'],
    ['m_scanner',  '    scanf("%d %d", &m, &n);'],
    ['m_alloc_n1', '    int nums1[12] = {0};'],
    ['m_loop_n1',  '    for (int i = 0; i < m; i++) {'],
    ['m_read_n1',  '        scanf("%d", &nums1[i]);'],
    ['',           '    }'],
    ['m_alloc_n2', '    int nums2[6];'],
    ['m_loop_n2',  '    for (int i = 0; i < n; i++) {'],
    ['m_read_n2',  '        scanf("%d", &nums2[i]);'],
    ['',           '    }'],
    ['m_call',     '    merge(nums1, m, nums2, n);'],
    ['',           '    return 0;'],
    ['',           '}']
  ]
};

const PSEUDOCODE = [
  'function merge(nums1, m, nums2, n):',
  '    p1 = m - 1          // pointer to end of valid nums1',
  '    p2 = n - 1          // pointer to end of nums2',
  '    p  = m + n - 1      // pointer to end of merged region',
  '',
  '    while p1 >= 0 and p2 >= 0:',
  '        if nums1[p1] > nums2[p2]:',
  '            nums1[p] = nums1[p1]',
  '            p1 -= 1',
  '        else:',
  '            nums1[p] = nums2[p2]',
  '            p2 -= 1',
  '        p -= 1',
  '',
  '    // Copy remaining nums2 elements (if any)',
  '    while p2 >= 0:',
  '        nums1[p] = nums2[p2]',
  '        p2 -= 1',
  '        p  -= 1'
];

// ─── Step Builder ─────────────────────────────────────────────────────────────
function buildSteps(rawNums1, mParam, rawNums2, nParam) {
  const steps = [];
  const m = Math.max(0, Math.min(6, parseInt(mParam, 10) || 0));
  const n = Math.max(0, Math.min(6, parseInt(nParam, 10) || 0));

  const nums1 = rawNums1.slice(0, m + n);
  while (nums1.length < m + n) {
    nums1.push(0);
  }
  const nums2 = rawNums2.slice(0, n);
  while (nums2.length < n) {
    nums2.push(0);
  }

  // initState tracks which pointers have been declared so far.
  // p1Ready / p2Ready / pReady become true only after their init line executes.
  function snap(p1v, p2v, pv, n1, n2, code, phase, initState, extra) {
    return {
      p1: p1v,
      p2: p2v,
      p: pv,
      nums1: [...n1],
      nums2: [...n2],
      m,
      n,
      code,
      phase,
      initState: { ...initState },
      ...extra
    };
  }

  // ── Phase 1: main() — reading inputs ─────────────────────────────────────
  const NONE = { p1Ready: false, p2Ready: false, pReady: false };
  const ALL  = { p1Ready: true,  p2Ready: true,  pReady: true  };

  // Build partial arrays for animation (progressive fill)
  const partial1 = new Array(m + n).fill(0);
  const partial2 = new Array(n).fill(0);

  steps.push({
    ...snap(-1, -1, -1, partial1, partial2, 'm_scanner', 'input', NONE, {}),
    badge: `Scanner sc = new Scanner(System.in); → Initializing input stream in main().`
  });
  steps.push({
    ...snap(-1, -1, -1, partial1, partial2, 'm_read_m', 'input', NONE, {}),
    badge: `int m = sc.nextInt(); → Read m = ${m}. nums1 has ${m} valid elements.`
  });
  steps.push({
    ...snap(-1, -1, -1, partial1, partial2, 'm_read_n', 'input', NONE, {}),
    badge: `int n = sc.nextInt(); → Read n = ${n}. nums2 has ${n} elements.`
  });
  steps.push({
    ...snap(-1, -1, -1, partial1, partial2, 'm_alloc_n1', 'input', NONE, { inputHighlight: 'n1alloc' }),
    badge: `int[] nums1 = new int[m + n]; → Allocated nums1 with ${m + n} slots. First ${m} for valid elements, last ${n} are 0-padded for merging.`
  });

  for (let i = 0; i < m; i++) {
    steps.push({
      ...snap(-1, -1, -1, partial1, partial2, 'm_loop_n1', 'input', NONE, { readingN1: i }),
      badge: `for (int i = ${i}; i < ${m}; i++) → Outer loop: i = ${i}. Will read nums1[${i}].`
    });
    partial1[i] = nums1[i];
    steps.push({
      ...snap(-1, -1, -1, partial1, partial2, 'm_read_n1', 'input', NONE, { readingN1: i, justReadN1: i }),
      badge: `nums1[i] = sc.nextInt(); → nums1[${i}] = ${nums1[i]}. Stored at index ${i}.`
    });
  }
  if (m > 0) {
    steps.push({
      ...snap(-1, -1, -1, partial1, partial2, 'm_loop_n1', 'input', NONE, { readingN1: m }),
      badge: `for (int i = ${m}; i < ${m}; i++) → i = ${m} is NOT less than m = ${m}. Exit loop.`
    });
  }

  steps.push({
    ...snap(-1, -1, -1, partial1, partial2, 'm_alloc_n2', 'input', NONE, { inputHighlight: 'n2alloc' }),
    badge: `int[] nums2 = new int[n]; → Allocated nums2 with ${n} slots.`
  });

  for (let i = 0; i < n; i++) {
    steps.push({
      ...snap(-1, -1, -1, partial1, partial2, 'm_loop_n2', 'input', NONE, { readingN2: i }),
      badge: `for (int i = ${i}; i < ${n}; i++) → Inner loop: i = ${i}. Will read nums2[${i}].`
    });
    partial2[i] = nums2[i];
    steps.push({
      ...snap(-1, -1, -1, partial1, partial2, 'm_read_n2', 'input', NONE, { readingN2: i, justReadN2: i }),
      badge: `nums2[i] = sc.nextInt(); → nums2[${i}] = ${nums2[i]}. Stored at index ${i}.`
    });
  }
  if (n > 0) {
    steps.push({
      ...snap(-1, -1, -1, partial1, partial2, 'm_loop_n2', 'input', NONE, { readingN2: n }),
      badge: `for (int i = ${n}; i < ${n}; i++) → i = ${n} is NOT less than n = ${n}. Exit loop.`
    });
  }

  steps.push({
    ...snap(-1, -1, -1, [...nums1], [...nums2], 'm_call', 'input', NONE, {}),
    badge: `new Solution().merge(nums1, m, nums2, n); → All input read. Invoking merge with m=${m}, n=${n}.`
  });

  // ── Phase 2: merge() function ─────────────────────────────────────────────
  const INIT_ENTRY = { p1Ready: false, p2Ready: false, pReady: false };
  steps.push({
    ...snap(-1, -1, -1, [...nums1], [...nums2], 'fn_entry', 'init', INIT_ENTRY, {}),
    badge: `merge(nums1, m=${m}, nums2, n=${n}) → Function entry. Will merge two sorted arrays in-place into nums1.`
  });

  const AFTER_P1 = { p1Ready: true,  p2Ready: false, pReady: false };
  steps.push({
    ...snap(m - 1, -1, -1, [...nums1], [...nums2], 'init_p1', 'init', AFTER_P1, {}),
    badge: `int p1 = m - 1; → p1 = ${m} - 1 = ${m - 1}. Points to last valid element of nums1.`
  });

  const AFTER_P2 = { p1Ready: true,  p2Ready: true,  pReady: false };
  steps.push({
    ...snap(m - 1, n - 1, -1, [...nums1], [...nums2], 'init_p2', 'init', AFTER_P2, {}),
    badge: `int p2 = n - 1; → p2 = ${n} - 1 = ${n - 1}. Points to last element of nums2.`
  });

  steps.push({
    ...snap(m - 1, n - 1, m + n - 1, [...nums1], [...nums2], 'init_p', 'init', ALL, {}),
    badge: `int p = m + n - 1; → p = ${m} + ${n} - 1 = ${m + n - 1}. Points to last position of nums1 (write cursor).`
  });

  const cur1 = [...nums1];
  const cur2 = [...nums2];
  let p1 = m - 1;
  let p2 = n - 1;
  let p = m + n - 1;

  while (p1 >= 0 && p2 >= 0) {
    steps.push({
      ...snap(p1, p2, p, cur1, cur2, 'while_cond', 'merge', ALL, { lastAction: null }),
      badge: `while (p1 >= 0 && p2 >= 0): p1=${p1} >= 0 && p2=${p2} >= 0 → TRUE. Continue merging.`
    });

    const cmpResult = cur1[p1] > cur2[p2];
    steps.push({
      ...snap(p1, p2, p, cur1, cur2, 'if_cmp', 'merge', ALL, { cmpResult, lastAction: null }),
      badge: `if (nums1[p1] > nums2[p2]): nums1[${p1}]=${cur1[p1]} > nums2[${p2}]=${cur2[p2]} → ${cmpResult ? 'TRUE. Place nums1[p1] at nums1[p].' : 'FALSE. Place nums2[p2] at nums1[p].'}`
    });

    if (cmpResult) {
      const placed = cur1[p1];
      cur1[p] = placed;
      steps.push({
        ...snap(p1, p2, p, cur1, cur2, 'place_n1', 'merge', ALL, { justPlaced: p, placedFrom: 'n1', lastAction: 'place' }),
        badge: `nums1[p] = nums1[p1]; → nums1[${p}] = nums1[${p1}] = ${placed}. Placed value ${placed} at write position ${p}.`
      });
      p1--;
      steps.push({
        ...snap(p1, p2, p, cur1, cur2, 'dec_p1', 'merge', ALL, { justPlaced: p, lastAction: 'dec_p1' }),
        badge: `p1--; → p1 = ${p1}. Move p1 pointer left.`
      });
    } else {
      const placed = cur2[p2];
      cur1[p] = placed;
      steps.push({
        ...snap(p1, p2, p, cur1, cur2, 'place_n2', 'merge', ALL, { justPlaced: p, placedFrom: 'n2', lastAction: 'place' }),
        badge: `nums1[p] = nums2[p2]; → nums1[${p}] = nums2[${p2}] = ${placed}. Placed value ${placed} at write position ${p}.`
      });
      p2--;
      steps.push({
        ...snap(p1, p2, p, cur1, cur2, 'dec_p2', 'merge', ALL, { justPlaced: p, lastAction: 'dec_p2' }),
        badge: `p2--; → p2 = ${p2}. Move p2 pointer left.`
      });
    }
    p--;
    steps.push({
      ...snap(p1, p2, p, cur1, cur2, 'dec_p', 'merge', ALL, { lastAction: 'dec_p' }),
      badge: `p--; → p = ${p}. Move write cursor left.`
    });
  }

  steps.push({
    ...snap(p1, p2, p, cur1, cur2, 'while_cond', 'merge', ALL, { lastAction: null }),
    badge: `while (p1 >= 0 && p2 >= 0): p1=${p1}, p2=${p2} → FALSE. Exit main loop.`
  });

  if (p2 >= 0) {
    while (p2 >= 0) {
      steps.push({
        ...snap(p1, p2, p, cur1, cur2, 'tail_while', 'tail', ALL, { lastAction: null }),
        badge: `while (p2 >= 0): p2=${p2} >= 0 → TRUE. Remaining nums2 elements must be placed.`
      });
      const placed = cur2[p2];
      cur1[p] = placed;
      steps.push({
        ...snap(p1, p2, p, cur1, cur2, 'tail_place', 'tail', ALL, { justPlaced: p, placedFrom: 'n2', lastAction: 'place' }),
        badge: `nums1[p] = nums2[p2]; → nums1[${p}] = nums2[${p2}] = ${placed}. Copying remaining element.`
      });
      p2--;
      steps.push({
        ...snap(p1, p2, p, cur1, cur2, 'tail_p2', 'tail', ALL, { lastAction: 'dec_p2' }),
        badge: `p2--; → p2 = ${p2}.`
      });
      p--;
      steps.push({
        ...snap(p1, p2, p, cur1, cur2, 'tail_p', 'tail', ALL, { lastAction: 'dec_p' }),
        badge: `p--; → p = ${p}.`
      });
    }
    steps.push({
      ...snap(p1, p2, p, cur1, cur2, 'tail_while', 'done', ALL, { lastAction: null }),
      badge: `while (p2 >= 0): p2=${p2} < 0 → FALSE. All elements placed.`
    });
  } else {
    steps.push({
      ...snap(p1, p2, p, cur1, cur2, 'tail_while', 'done', ALL, { lastAction: null }),
      badge: `while (p2 >= 0): p2=${p2} < 0 → Skipped. No remaining nums2 elements.`
    });
  }

  steps.push({
    ...snap(p1, p2, p, cur1, cur2, 'fn_entry', 'done', ALL, { lastAction: null }),
    badge: `merge() complete. nums1 is now fully sorted: [${cur1.join(', ')}].`
  });

  return steps;
}

// ─── Reactive State ───────────────────────────────────────────────────────────
const DEFAULT_M = 3;
const DEFAULT_N = 3;
const DEFAULT_NUMS1 = [1, 2, 3, 0, 0, 0];
const DEFAULT_NUMS2 = [2, 5, 6];

const inputM = ref(DEFAULT_M);
const inputN = ref(DEFAULT_N);
const inputNums1Str = ref('1,2,3,0,0,0');
const inputNums2Str = ref('2,5,6');
const lang = ref('java');
const speed = ref(650);
const si = ref(0);
const playing = ref(false);
const vizHeight = ref(340);
const tableHeight = ref(60);
const leftWidth = ref(52);
const rightTab = ref('code');

const stepsData = reactive({ steps: buildSteps([...DEFAULT_NUMS1], DEFAULT_M, [...DEFAULT_NUMS2], DEFAULT_N) });
const steps = computed(() => stepsData.steps);
const s = computed(() => steps.value[Math.max(0, Math.min(si.value, steps.value.length - 1))] || {});
const codeLines = computed(() => CODES[lang.value] || []);

let playTimer = null;

function onChipsWheel(e) {
  if (e.currentTarget) {
    e.currentTarget.scrollLeft += e.deltaY;
  }
}

function parseArr(str) {
  return str.trim().split(/[\s,]+/).map(x => parseInt(x, 10)).filter(x => !isNaN(x));
}

function applyInput() {
  const mVal = parseInt(inputM.value, 10);
  const nVal = parseInt(inputN.value, 10);
  if (isNaN(mVal) || mVal < 0 || mVal > 6) { alert('Please enter m between 0 and 6.'); inputM.value = 3; return; }
  if (isNaN(nVal) || nVal < 0 || nVal > 6) { alert('Please enter n between 0 and 6.'); inputN.value = 3; return; }
  const n1 = parseArr(inputNums1Str.value);
  const n2 = parseArr(inputNums2Str.value);
  playing.value = false;
  stepsData.steps = buildSteps(n1, mVal, n2, nVal);
  si.value = 0;
  if (typeof window !== 'undefined') {
    window.scrollTo(0, 0);
  }
}

function loadPreset(p) {
  if (p === 'ex1') {
    inputM.value = 3;
    inputN.value = 3;
    inputNums1Str.value = '1,2,3,0,0,0';
    inputNums2Str.value = '2,5,6';
  } else if (p === 'ex2') {
    inputM.value = 1;
    inputN.value = 1;
    inputNums1Str.value = '1,0';
    inputNums2Str.value = '2';
  } else if (p === 'ex3') {
    inputM.value = 0;
    inputN.value = 1;
    inputNums1Str.value = '0';
    inputNums2Str.value = '1';
  } else if (p === 'ex4') {
    inputM.value = 4;
    inputN.value = 4;
    inputNums1Str.value = '1,3,5,7,0,0,0,0';
    inputNums2Str.value = '2,4,6,8';
  }
  applyInput();
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
    const contRect = container.getBoundingClientRect();
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
const displayNums1 = computed(() => s.value.nums1 || []);
const displayNums2 = computed(() => s.value.nums2 || []);
const displayM = computed(() => s.value.m !== undefined ? s.value.m : inputM.value);
const displayN = computed(() => s.value.n !== undefined ? s.value.n : inputN.value);
const initState = computed(() => s.value.initState || { p1Ready: false, p2Ready: false, pReady: false });

// Only expose a pointer value in the chip/arrow when its init line has executed.
const displayP1 = computed(() => initState.value.p1Ready ? s.value.p1 : '?');
const displayP2 = computed(() => initState.value.p2Ready ? s.value.p2 : '?');
const displayP  = computed(() => initState.value.pReady  ? s.value.p  : '?');

function cellClass1(idx) {
  const classes = [];
  const ist = initState.value;
  const justPlaced = s.value.justPlaced;
  const phase = s.value.phase;
  const isMergedRegion = idx >= displayM.value;
  // p1 arrow: only after p1 is initialized, and only in merge/init/tail phases
  if (ist.p1Ready && idx === s.value.p1 && (phase === 'merge' || phase === 'init')) {
    classes.push('ms-ptr-p1');
  }
  // p (write cursor) arrow: only after p is initialized
  if (ist.pReady && idx === s.value.p) {
    classes.push('ms-ptr-p');
  }
  if (idx === justPlaced && s.value.lastAction === 'place') {
    classes.push('ms-just-placed');
  }
  if (phase === 'done') {
    classes.push('ms-sorted');
  }
  if (isMergedRegion && phase !== 'done' && idx !== justPlaced) {
    classes.push('ms-zero-slot');
  }
  return classes.join(' ');
}

function cellClass2(idx) {
  const classes = [];
  const ist = initState.value;
  const phase = s.value.phase;
  // p2 arrow: only after p2 is initialized
  if (ist.p2Ready && idx === s.value.p2 && (phase === 'merge' || phase === 'tail' || phase === 'init')) {
    classes.push('ms-ptr-p2');
  }
  if (phase === 'done') {
    classes.push('ms-sorted');
  }
  // Highlight the cell being read during input phase
  if (phase === 'input' && s.value.readingN2 === idx) {
    classes.push('ms-ptr-p2');
  }
  return classes.join(' ');
}

const mainRef = ref(null);
const leftColRef = ref(null);
const hResizerRef = ref(null);
const vizResizerRef = ref(null);
const tableResizerRef = ref(null);

function initHResizer() {
  const rsz = hResizerRef.value;
  const main = mainRef.value;
  if (!rsz || !main) {
    return;
  }
  let dragging = false;
  let startX = 0;
  let startW = 0;
  const onDown = e => {
    dragging = true;
    startX = e.clientX;
    startW = leftColRef.value.offsetWidth;
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
  let startY = 0;
  let startH = 0;
  const onDown = e => {
    dragging = true;
    startY = e.clientY;
    startH = valueRef.value;
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
  cleanupFns.push(initVResizer(vizResizerRef, vizHeight, 200, 700));
  cleanupFns.push(initVResizer(tableResizerRef, tableHeight, 50, 200));
  if (typeof window !== 'undefined') {
    window.scrollTo(0, 0);
  }
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
          <!-- Control Panel Toolbar -->
          <div class="ll-toolbar">
            <div class="ll-input-group">
              <label>m:</label>
              <input type="number" min="0" max="6" v-model.number="inputM" class="ll-text-input" @keyup.enter="applyInput" style="width: 44px;" />
            </div>
            <div class="ll-input-group">
              <label>n:</label>
              <input type="number" min="0" max="6" v-model.number="inputN" class="ll-text-input" @keyup.enter="applyInput" style="width: 44px;" />
            </div>
            <div class="ll-input-group">
              <label>nums1[]:</label>
              <input type="text" v-model="inputNums1Str" class="ll-text-input ms-arr-input" @keyup.enter="applyInput" placeholder="e.g. 1,2,3,0,0,0" />
            </div>
            <div class="ll-input-group">
              <label>nums2[]:</label>
              <input type="text" v-model="inputNums2Str" class="ll-text-input ms-arr-input" @keyup.enter="applyInput" placeholder="e.g. 2,5,6" />
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

                  <!-- Real-time Stats Chips -->
                  <div class="ll-ptrs ll-ptrs-compact" @wheel.passive="onChipsWheel">
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">m</span><b class="ll-c-blue">{{ displayM }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">n</span><b class="ll-c-blue">{{ displayN }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">p1</span><b class="ll-c-purple">{{ displayP1 }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">p2</span><b class="ll-c-green">{{ displayP2 }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">p</span><b class="ll-c-orange">{{ displayP }}</b></span>
                    <span class="ll-ptr-chip-inline"><span class="ll-chip-label">phase</span><b class="ll-c-orange">{{ s.phase || 'init' }}</b></span>
                  </div>

                  <!-- BOARD CONTAINER -->
                  <div class="ll-board-container">

                    <!-- Tier 1: nums1 Array -->
                    <div class="ms-tier-title">Tier 1 &mdash; nums1 Array <code>int[] nums1 (size = m + n = {{ displayM + displayN }})</code></div>
                    <div class="ms-array-frame">
                      <!-- Pointer row top -->
                      <div class="ms-ptr-row">
                        <template v-for="(val, idx) in displayNums1" :key="'p-row-' + idx">
                          <div class="ms-ptr-cell">
                            <span v-if="initState.p1Ready && idx === s.p1 && (s.phase === 'merge' || s.phase === 'init')" class="ms-ptr-label ms-ptr-label-p1">p1</span>
                            <span v-if="initState.pReady && idx === s.p" class="ms-ptr-label ms-ptr-label-p">p</span>
                          </div>
                        </template>
                      </div>
                      <!-- Main array cells -->
                      <div class="ms-array-row">
                        <div
                          v-for="(val, idx) in displayNums1"
                          :key="'n1-' + idx"
                          class="ms-cell"
                          :class="cellClass1(idx)"
                          :title="`nums1[${idx}]`"
                        >
                          <span class="ms-cell-idx">[{{ idx }}]</span>
                          <span class="ms-cell-val">{{ val }}</span>
                          <span v-if="idx < displayM && s.phase !== 'done'" class="ms-region-badge ms-badge-valid">valid</span>
                          <span v-else-if="idx >= displayM && s.phase !== 'done'" class="ms-region-badge ms-badge-slot">slot</span>
                        </div>
                      </div>
                      <!-- Section labels -->
                      <div class="ms-section-labels">
                        <div class="ms-section-block" :style="{ width: (displayM * 58) + 'px' }">
                          <span class="ms-section-text" v-if="displayM > 0">nums1 valid [0..{{ displayM - 1 }}]</span>
                        </div>
                        <div class="ms-section-block" :style="{ width: (displayN * 58) + 'px' }">
                          <span class="ms-section-text" v-if="displayN > 0">extra slots [{{ displayM }}..{{ displayM + displayN - 1 }}]</span>
                        </div>
                      </div>
                    </div>

                    <!-- Tier 2: nums2 Array -->
                    <div class="ms-tier-title">Tier 2 &mdash; nums2 Array <code>int[] nums2 (size = n = {{ displayN }})</code></div>
                    <div class="ms-array-frame ms-array-frame-n2">
                      <!-- Pointer row: p2 -->
                      <div class="ms-ptr-row">
                        <template v-for="(val, idx) in displayNums2" :key="'p2-row-' + idx">
                          <div class="ms-ptr-cell">
                            <span v-if="initState.p2Ready && idx === s.p2 && (s.phase === 'merge' || s.phase === 'tail' || s.phase === 'init')" class="ms-ptr-label ms-ptr-label-p2">p2</span>
                          </div>
                        </template>
                      </div>
                      <!-- Main array cells -->
                      <div class="ms-array-row">
                        <div
                          v-for="(val, idx) in displayNums2"
                          :key="'n2-' + idx"
                          class="ms-cell ms-cell-n2"
                          :class="cellClass2(idx)"
                          :title="`nums2[${idx}]`"
                        >
                          <span class="ms-cell-idx">[{{ idx }}]</span>
                          <span class="ms-cell-val">{{ val }}</span>
                        </div>
                        <div v-if="displayN === 0" class="ms-empty-msg">&lang; empty &rang;</div>
                      </div>
                    </div>

                    <!-- Tier 3: Pointer State Panel -->
                    <div class="ms-tier-title">Tier 3 &mdash; Pointer State</div>
                    <div class="ms-ptr-state-panel">
                      <div class="ms-ptr-state-card" :class="{ 'ms-ptr-state-active': initState.p1Ready && (s.phase === 'merge' || s.phase === 'init') }">
                        <span class="ms-ptr-state-name ms-ptr-label-p1">p1</span>
                        <span class="ms-ptr-state-val">{{ !initState.p1Ready ? '?' : (s.p1 >= 0 ? s.p1 : 'exhausted') }}</span>
                        <span class="ms-ptr-state-desc">nums1 read cursor</span>
                      </div>
                      <div class="ms-ptr-state-card" :class="{ 'ms-ptr-state-active': initState.p2Ready && (s.phase === 'merge' || s.phase === 'tail' || s.phase === 'init') }">
                        <span class="ms-ptr-state-name ms-ptr-label-p2">p2</span>
                        <span class="ms-ptr-state-val">{{ !initState.p2Ready ? '?' : (s.p2 >= 0 ? s.p2 : 'exhausted') }}</span>
                        <span class="ms-ptr-state-desc">nums2 read cursor</span>
                      </div>
                      <div class="ms-ptr-state-card" :class="{ 'ms-ptr-state-active': initState.pReady }">
                        <span class="ms-ptr-state-name ms-ptr-label-p">p</span>
                        <span class="ms-ptr-state-val">{{ !initState.pReady ? '?' : s.p }}</span>
                        <span class="ms-ptr-state-desc">write cursor (end)</span>
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
                <span class="ll-leg"><span class="ll-legdot" style="background:#eff6ff;border:1.5px solid #3b82f6;"></span>p1</span>
                <span class="ll-leg"><span class="ll-legdot" style="background:#f0fdf4;border:1.5px solid #22c55e;"></span>p2</span>
                <span class="ll-leg"><span class="ll-legdot" style="background:#fff7ed;border:1.5px solid #f97316;"></span>p</span>
                <span class="ll-leg"><span class="ll-legdot ms-legdot-placed"></span>Just Placed</span>
                <span class="ll-leg"><span class="ll-legdot ms-legdot-sorted"></span>Sorted</span>
                <span class="ll-leg"><span class="ll-legdot ms-legdot-slot"></span>Empty Slot</span>
              </div>

              <!-- Call Stack Area -->
              <div class="ll-table-area" :style="{ height: tableHeight + 'px' }">
                <div class="ll-table-title">Call Stack &mdash; Current Execution Frame</div>
                <div class="ll-stack-line">
                  <!-- Input phase: show main() -->                  
                  <template v-if="s.phase === 'input'">
                    <div class="ll-frame ll-frame-cur">
                      main()
                      &nbsp;&nbsp;
                      <span class="ll-fname">m</span>=<span class="ll-c-blue" style="font-weight:700">{{ displayM }}</span>,
                      <span class="ll-fname">n</span>=<span class="ll-c-blue" style="font-weight:700">{{ displayN }}</span>
                      <span class="ll-now"> &#9668; active</span>
                    </div>
                  </template>
                  <!-- merge() phase -->
                  <template v-else>
                    <div class="ll-frame" style="color:var(--text2);">main()</div>
                    <div class="ll-frame ll-frame-cur" style="margin-left:14px">
                      merge(nums1, m={{ displayM }}, nums2, n={{ displayN }})
                      &nbsp;&nbsp;
                      <span class="ll-fname">p1</span>=<span class="ll-c-purple" style="font-weight:700">{{ displayP1 }}</span>,
                      <span class="ll-fname">p2</span>=<span class="ll-c-green" style="font-weight:700">{{ displayP2 }}</span>,
                      <span class="ll-fname">p</span>=<span class="ll-c-orange" style="font-weight:700">{{ displayP }}</span>
                      <span class="ll-now"> &#9668; active</span>
                    </div>
                  </template>
                </div>
              </div>

              <!-- Vertical Resizer -->
              <div class="ll-vresizer" ref="tableResizerRef"></div>

              <!-- Step Badge Banner -->
              <div class="ll-badge-wrap">
                <div class="ll-badge"
                  :class="{
                    'll-badge-error': s.badge && (s.badge.includes('FALSE') || s.badge.includes('exhausted')),
                    'll-badge-success': s.badge && (s.badge.includes('complete') || s.badge.includes('sorted') || s.badge.includes('TRUE'))
                  }"
                >
                  {{ s.badge || 'Ready to run Merge Sorted Array algorithm.' }}
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

                <!-- Code Scroll with Active Line Highlighting -->
                <div v-if="rightTab === 'code'" class="ll-code-scroll" ref="codeScrollRef">
                  <pre class="ll-pre"><span v-for="(line, idx) in codeLines" :key="idx" class="ll-codeline" :class="{ 'll-hl': line[0] && line[0] === s.code }">{{ line[1] === '' ? ' ' : line[1] }}</span></pre>
                </div>

                <!-- Pseudocode Scroll -->
                <div v-else-if="rightTab === 'pseudo'" class="ll-code-scroll">
                  <pre class="ll-pre"><span v-for="(line, idx) in PSEUDOCODE" :key="idx" class="ll-codeline">{{ line }}</span></pre>
                </div>

                <!-- Complexity Tab -->
                <div v-else class="ll-info-scroll">
                  <h3 class="ll-cx-heading">Merge Sorted Array (LeetCode 88) &mdash; Complexity Analysis</h3>
                  <p class="ll-cx-intro">
                    Given two sorted integer arrays <code>nums1</code> (size <code>m + n</code>, with <code>m</code> valid elements)
                    and <code>nums2</code> (size <code>n</code>), merge <code>nums2</code> into <code>nums1</code> in-place such
                    that the result is sorted. The key insight is to fill <strong>from the end</strong> using three pointers
                    (<code>p1</code>, <code>p2</code>, <code>p</code>) to avoid overwriting valid elements.
                  </p>

                  <h4 class="ll-cx-sub">Step-by-step Breakdown</h4>
                  <table class="ll-complexity-table">
                    <thead><tr><th>Operation / Phase</th><th>Time</th><th>Space</th><th>Reason</th></tr></thead>
                    <tbody>
                      <tr><td>Initialization (p1, p2, p)</td><td class="ll-cx-good">O(1)</td><td class="ll-cx-good">O(1)</td><td>Three constant-time pointer assignments</td></tr>
                      <tr><td>Main while loop</td><td class="ll-cx-good">O(m + n)</td><td class="ll-cx-good">O(1)</td><td>Each element from nums1 or nums2 is placed at most once</td></tr>
                      <tr><td>Tail while loop (remaining nums2)</td><td class="ll-cx-good">O(n)</td><td class="ll-cx-good">O(1)</td><td>At most n remaining elements from nums2 are copied</td></tr>
                      <tr><td>Total</td><td class="ll-cx-good">O(m + n)</td><td class="ll-cx-good">O(1)</td><td>Single-pass, in-place merge. No extra array needed.</td></tr>
                    </tbody>
                  </table>

                  <h4 class="ll-cx-sub">Overall Complexity</h4>
                  <div class="ll-cx-summary-grid">
                    <div class="ll-cx-card ll-cx-card-good">
                      <div class="ll-cx-card-label">Time</div>
                      <div class="ll-cx-card-val">O(m + n)</div>
                      <div class="ll-cx-card-note">Linear — each element placed exactly once</div>
                    </div>
                    <div class="ll-cx-card ll-cx-card-good">
                      <div class="ll-cx-card-label">Space</div>
                      <div class="ll-cx-card-val">O(1)</div>
                      <div class="ll-cx-card-note">In-place. Only 3 pointer variables used</div>
                    </div>
                    <div class="ll-cx-card ll-cx-card-good">
                      <div class="ll-cx-card-label">Passes</div>
                      <div class="ll-cx-card-val">1</div>
                      <div class="ll-cx-card-note">Single backward scan over both arrays</div>
                    </div>
                  </div>

                  <div class="ll-note">
                    <strong>Key Insight:</strong> By iterating from the <em>end</em> of both arrays and writing to the
                    <em>end</em> of <code>nums1</code>, we never overwrite a valid <code>nums1</code> element before it
                    has been compared. The tail loop handles the edge case where <code>nums2</code> still has elements
                    remaining after <code>nums1</code> is exhausted. Elements remaining in <code>nums1</code> (when
                    <code>p2 &lt; 0</code> first) are already in the correct position.
                  </div>
                </div>
              </div>
            </div>
          </div>

          <!-- Footer Toolbar -->
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

@keyframes ll-pop { 0% { transform: scale(0.6); opacity: 0; } 70% { transform: scale(1.15); opacity: 1; } 100% { transform: scale(1); opacity: 1; } }
@keyframes ms-flash-placed { 0% { background: #fef9c3; } 50% { background: #10b981; } 100% { background: #dcfce7; } }
@keyframes ms-glow-sorted { 0% { box-shadow: 0 0 0 0 rgba(34,197,94,0); } 50% { box-shadow: 0 0 8px 3px rgba(34,197,94,0.4); } 100% { box-shadow: none; } }

.slide-wrapper { margin-top: -10px; margin-left: -30px; width: 107%; max-height: 100%; font-size: 0.8rem; font-weight: 400; }
.slide-body { display: flex; flex-direction: column; border-radius: 4px; height: 100%; }
.navbar { display: flex; flex-direction: row; justify-content: space-between; align-items: center; gap: 0.75rem; padding: 0 10px; background-color: #ffffff; position: fixed; width: 94.7%; z-index: 50; }
.navbar > img { height: 30px; }
.navbar-title { margin: 0; font-size: 1.35rem; font-weight: 700; background-color: #ef5050; color: #ffffff; width: 80%; padding: 2px 10px; margin-left: -10px; border-radius: 5px; }
.row-main { width: 100%; height: 90%; margin-top: 36px; overflow-x: auto; overflow-y: hidden; }

.ll-toolbar { margin-top: 4px; display: flex; align-items: center; gap: 6px; padding: 6.5px 12px; background: var(--surface); border-bottom: 1px solid var(--border); flex-shrink: 0; flex-wrap: wrap; box-shadow: var(--shadow-sm); }
.ll-input-group { display: flex; align-items: center; gap: 4px; }
.ll-input-group label { font-size: 11px; color: var(--muted); font-weight: 700; }
.ll-text-input { background: var(--surface); border: 1px solid var(--border2); color: var(--text); border-radius: var(--radius-sm); padding: 3px 6px; font-size: 11.5px; font-family: monospace; }
.ll-text-input:focus { outline: none; border-color: var(--coral); box-shadow: 0 0 0 3px rgba(240,77,77,.1); }
.ms-arr-input { width: 140px; }
.ll-preset-group { display: flex; gap: 3px; }
.ll-preset-btn { background: var(--surface2); border: 1px solid var(--border2); color: var(--text2); padding: 3px 6px; border-radius: 4px; font-size: 10.5px; cursor: pointer; transition: all .12s; }
.ll-preset-btn:hover { background: var(--coral-light); border-color: var(--coral); color: var(--coral-dark); }
.ll-viz-btn { background: var(--coral); color: #fff; border: none; padding: 5px 12px; border-radius: var(--radius-sm); cursor: pointer; font-size: 11.5px; font-weight: 600; box-shadow: var(--shadow-sm); transition: filter .15s; }
.ll-viz-btn:hover { filter: brightness(1.08); }
.ll-nav-controls { display: flex; margin-left: auto; align-items: center; gap: 4px; flex-shrink: 0; flex-wrap: wrap; }
.ll-nav-btn { background: var(--surface2); border: 1px solid var(--border2); color: var(--text2); padding: 4px 9px; border-radius: var(--radius-sm); cursor: pointer; font-size: 11px; font-weight: 500; transition: all .15s; white-space: nowrap; }
.ll-nav-btn:hover { background: var(--surface); border-color: var(--coral); color: var(--coral); }
.ll-play-btn { background: var(--blue-light); border: 1px solid var(--blue); color: var(--blue); min-width: 68px; font-weight: 600; padding: 4px 9px; border-radius: var(--radius-sm); cursor: pointer; font-size: 11px; transition: all .15s; }
.ll-play-btn:hover { background: var(--blue); color: #fff; }

.ll-main { display: flex; flex: 1; overflow: hidden; position: relative; }
.ll-left-col { display: flex; flex-direction: column; overflow: hidden; min-width: 220px; max-width: 75%; }
.ll-resizer { width: 5px; cursor: col-resize; background: var(--border); flex-shrink: 0; transition: background .15s; position: relative; z-index: 20; }
.ll-resizer:hover, .ll-resizer.drag { background: var(--coral); }
.ll-right-col { display: flex; flex-direction: column; flex: 1; overflow: hidden; min-width: 0; height: 100%; }

.ll-viz-wrap { flex-shrink: 0; background: var(--surface); border-bottom: 1px solid var(--border); position: relative; overflow-x: auto; overflow-y: auto; }
.ll-perm-area { display: flex; flex-direction: column; align-items: stretch; min-height: 100%; width: 100%; min-width: 0; box-sizing: border-box; }
.ll-ptrs { display: flex; gap: 8px; flex-wrap: wrap; padding: 4px 14px; min-height: 28px; width: 100%; box-sizing: border-box; min-width: 0; align-items: center; }
.ll-ptrs-compact { flex-wrap: nowrap; gap: 6px; padding: 3px 14px 5px; min-height: 0; overflow-x: auto; overflow-y: hidden; scrollbar-width: thin; scrollbar-color: var(--border) transparent; }
.ll-ptrs-compact::-webkit-scrollbar { height: 3px; }
.ll-ptrs-compact::-webkit-scrollbar-track { background: transparent; }
.ll-ptrs-compact::-webkit-scrollbar-thumb { background: var(--border); border-radius: 3px; }
.ll-ptrs-compact::-webkit-scrollbar-thumb:hover { background: var(--muted); }
.ll-ptr-chip-inline { display: inline-flex; align-items: center; gap: 4px; background: var(--surface2); border: 1px solid var(--border); border-radius: 6px; padding: 2px 8px; font-size: 11px; font-family: monospace; white-space: nowrap; flex-shrink: 0; line-height: 1.4; }
.ll-chip-label { color: var(--text-muted, #8899aa); font-weight: 500; margin-right: 2px; }
.ll-c-blue { color: var(--blue); } .ll-c-orange { color: var(--orange); } .ll-c-green { color: var(--green); } .ll-c-purple { color: var(--purple); } .ll-c-red { color: var(--red); }

/* Board Container — customized for Merge Sorted Array */
.ll-board-container { display: flex; flex-direction: column; align-items: flex-start; padding: 6px 14px 10px; gap: 5px; min-width: 0; }

.ms-tier-title { font-size: 10px; font-weight: 700; text-transform: uppercase; letter-spacing: 0.04em; color: var(--muted); margin-top: 4px; margin-bottom: 2px; font-family: 'Consolas', monospace; border-left: 3px solid var(--coral); padding-left: 6px; }

/* Array Frame */
.ms-array-frame { display: flex; flex-direction: column; background: #f8fafc; border: 1px solid var(--border); border-radius: var(--radius); padding: 6px 10px 8px; box-shadow: var(--shadow-sm); gap: 2px; }
.ms-array-frame-n2 { background: #f0fdf4; border-color: #bbf7d0; }

/* Pointer indicator row */
.ms-ptr-row { display: flex; gap: 2px; height: 16px; align-items: flex-end; }
.ms-ptr-cell { width: 56px; display: flex; justify-content: center; align-items: flex-end; gap: 2px; flex-shrink: 0; }
.ms-ptr-label { font-size: 9px; font-weight: 800; font-family: monospace; padding: 1px 4px; border-radius: 3px; line-height: 1.2; }
.ms-ptr-label-p1 { background: #eff6ff; color: #1d4ed8; border: 1px solid #93c5fd; }
.ms-ptr-label-p2 { background: #f0fdf4; color: #15803d; border: 1px solid #86efac; }
.ms-ptr-label-p { background: #fff7ed; color: #c2410c; border: 1px solid #fdba74; }

/* Array Row */
.ms-array-row { display: flex; gap: 2px; flex-wrap: nowrap; }
.ms-cell {
  width: 56px; min-height: 54px; position: relative; display: flex; flex-direction: column;
  align-items: center; justify-content: center; border-radius: 5px; user-select: none;
  border: 1.5px solid var(--border2); background: #f8fafc;
  transition: all .18s ease;
}
.ms-cell-n2 { background: #f0fdf4; border-color: #bbf7d0; }
.ms-cell-idx { font-size: 8px; color: var(--muted); font-weight: 700; font-family: monospace; margin-bottom: 2px; }
.ms-cell-val { font-size: 16px; font-weight: 800; font-family: monospace; color: var(--text); line-height: 1; }

/* Cell state classes */
.ms-ptr-p1 { background: #eff6ff !important; border: 2px solid var(--blue) !important; transform: scale(1.06); z-index: 5; box-shadow: 0 0 8px rgba(59,130,246,.3); }
.ms-ptr-p2 { background: #f0fdf4 !important; border: 2px solid var(--green) !important; transform: scale(1.06); z-index: 5; box-shadow: 0 0 8px rgba(34,197,94,.3); }
.ms-ptr-p { background: #fff7ed !important; border: 2px solid var(--orange) !important; box-shadow: 0 0 8px rgba(249,115,22,.35); }
.ms-just-placed { background: #dcfce7 !important; border: 2px solid #10b981 !important; animation: ms-flash-placed 0.45s ease; z-index: 10; transform: scale(1.1); }
.ms-zero-slot { background: #f1f5f9 !important; border: 1.5px dashed #94a3b8 !important; opacity: 0.75; }
.ms-sorted { background: #dcfce7 !important; border: 1.5px solid #86efac !important; animation: ms-glow-sorted 0.6s ease; }

/* Region badges */
.ms-region-badge { position: absolute; font-size: 7px; font-weight: 800; font-family: monospace; line-height: 1; padding: 1px 2px; border-radius: 2px; white-space: nowrap; bottom: 2px; left: 50%; transform: translateX(-50%); }
.ms-badge-valid { background: #dbeafe; color: #1d4ed8; border: 1px solid #93c5fd; }
.ms-badge-slot { background: #f1f5f9; color: #94a3b8; border: 1px solid #cbd5e1; }

/* Section labels under nums1 */
.ms-section-labels { display: flex; gap: 2px; margin-top: 2px; }
.ms-section-block { text-align: center; overflow: hidden; }
.ms-section-text { font-size: 8.5px; font-family: monospace; color: var(--muted); font-weight: 600; white-space: nowrap; }

/* Pointer State Panel (Tier 3) */
.ms-ptr-state-panel { display: flex; gap: 8px; flex-wrap: wrap; }
.ms-ptr-state-card {
  display: flex; flex-direction: column; align-items: center; gap: 2px;
  padding: 6px 14px; border-radius: var(--radius-sm); border: 1.5px solid var(--border);
  background: var(--surface2); min-width: 90px; font-size: 10.5px; font-family: monospace;
  transition: all .15s;
}
.ms-ptr-state-active { border-color: var(--orange) !important; background: var(--orange-light) !important; }
.ms-ptr-state-name { font-size: 11px; font-weight: 800; padding: 1px 6px; border-radius: 3px; }
.ms-ptr-state-val { font-size: 18px; font-weight: 900; color: var(--text); line-height: 1.2; }
.ms-ptr-state-desc { font-size: 9px; color: var(--muted); text-align: center; }

.ms-empty-msg { color: var(--muted); font-style: italic; font-size: 11px; padding: 4px 8px; }

/* Legend dots */
.ms-legdot-placed { background: #dcfce7; border: 1.5px solid #10b981; display: inline-block; width: 11px; height: 11px; border-radius: 3px; }
.ms-legdot-sorted { background: #dcfce7; border: 1.5px solid #86efac; display: inline-block; width: 11px; height: 11px; border-radius: 3px; }
.ms-legdot-slot { background: #f1f5f9; border: 1.5px dashed #94a3b8; display: inline-block; width: 11px; height: 11px; border-radius: 3px; }

.ll-vresizer { height: 5px; cursor: row-resize; background: var(--border); flex-shrink: 0; transition: background .15s; position: relative; z-index: 20; }
.ll-vresizer:hover, .ll-vresizer.drag { background: var(--coral); }
.ll-legend { display: flex; flex-wrap: wrap; gap: 6px 14px; padding: 6px 12px; border-bottom: 1px solid var(--border); flex-shrink: 0; background: var(--surface2); }
.ll-leg { display: flex; align-items: center; gap: 5px; font-size: 11px; color: var(--text2); font-weight: 500; }
.ll-legdot { width: 11px; height: 11px; border-radius: 3px; flex-shrink: 0; display: inline-block; }

.ll-table-area { flex-shrink: 0; padding: 8px 14px; border-bottom: 1px solid var(--border); overflow-x: hidden; overflow-y: auto; background: var(--surface); min-width: 0; box-sizing: border-box; }
.ll-table-title { font-size: 10px; color: var(--muted); margin-bottom: 4px; font-style: italic; }
.ll-stack-line { font-family: 'Consolas', monospace; font-size: 12px; line-height: 1.8; }
.ll-frame { font-family: 'Consolas', monospace; font-size: 11.5px; color: var(--text2); padding: 1px 0; white-space: nowrap; }
.ll-frame-cur { color: var(--orange); background: var(--orange-light); border-radius: 4px; padding: 1px 5px; }
.ll-fname { color: var(--text2); }
.ll-now { color: var(--orange); font-size: 10px; margin-left: 6px; }

.ll-badge-wrap { padding: 6px 10px; border-bottom: 1px solid var(--border); flex-shrink: 0; min-height: 36px; display: flex; align-items: center; background: var(--surface); }
.ll-badge { display: inline-block; padding: 4px 12px; border-radius: var(--radius-sm); border-left: 3px solid var(--coral); background: var(--coral-light); font-size: 11px; color: var(--coral-dark); line-height: 1.4; word-break: break-word; font-weight: 500; }
.ll-badge-error { border-left-color: var(--red) !important; background: var(--red-light) !important; color: var(--red-dark) !important; }
.ll-badge-success { border-left-color: var(--green) !important; background: var(--green-light) !important; color: #15803d !important; }

.ll-code-panel { display: flex; flex-direction: column; height: 100%; overflow: hidden; }
.ll-code-header { display: flex; align-items: center; gap: 6px; padding: 5px 12px; background: var(--surface); border-bottom: 1px solid var(--border); box-shadow: var(--shadow-sm); flex-shrink: 0; flex-wrap: wrap; }
.ll-tabbar { display: flex; gap: 3px; flex-wrap: wrap; }
.ll-tab-btn { padding: 4px 9px; font-size: 10.5px; font-weight: 600; border: 1px solid var(--border2); background: var(--surface2); color: var(--text2); border-radius: var(--radius-sm); cursor: pointer; transition: all .15s ease; white-space: nowrap; }
.ll-tab-btn:hover { border-color: var(--coral); color: var(--coral); }
.ll-tab-btn.active { background: var(--coral); border-color: var(--coral); color: #fff; }
.ll-lang-select { margin-left: auto; padding: 4px 24px 4px 8px; font-size: 11px; font-weight: 500; border: 1px solid var(--border2); border-radius: var(--radius-sm); background: var(--surface2); color: var(--text); cursor: pointer; appearance: none; background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='10' height='6' viewBox='0 0 10 6'%3E%3Cpath d='M0 0l5 6 5-6z' fill='%2394a3b8'/%3E%3C/svg%3E"); background-repeat: no-repeat; background-position: right 8px center; min-width: 95px; transition: border-color .15s; }
.ll-lang-select:focus { outline: none; border-color: var(--coral); box-shadow: 0 0 0 3px rgba(240,77,77,.1); }
.ll-code-scroll { flex: 1; overflow: auto; background: #f8fafc; padding: 10px 14px; min-width: 0; }
.ll-pre { margin: 0; font-family: 'Cascadia Code', 'Fira Code', 'Consolas', monospace; font-size: 11px; line-height: 1.5; color: var(--text); white-space: pre; padding-bottom: 150px; }
.ll-codeline { display: block; padding: 0 14px; margin: 0 -14px; }
.ll-hl { background: #dcfce7; color: #15803d; font-weight: 600; border-left: 3px solid var(--green); border-radius: 3px; }

.ll-info-scroll { flex: 1; overflow: auto; padding: 12px 16px; background: var(--surface); color: var(--text2); font-size: 12px; line-height: 1.55; }
.ll-info-scroll h3 { font-size: 13px; font-weight: 700; color: var(--text); margin: 0 0 6px; }
.ll-info-scroll h4 { font-size: 12px; font-weight: 700; color: var(--text); margin: 10px 0 4px; }
.ll-info-scroll p { margin: 0 0 6px; }
.ll-info-scroll code { background: var(--surface2); padding: 1px 4px; border-radius: 3px; font-family: 'Cascadia Code', monospace; font-size: 11px; color: var(--coral-dark); }
.ll-cx-heading { font-size: 13px; font-weight: 700; color: var(--text); margin: 0 0 6px; }
.ll-cx-intro { font-size: 10.5px; color: var(--text2); margin: 0 0 10px; line-height: 1.55; }
.ll-cx-sub { font-size: 11px; font-weight: 700; color: var(--text2); margin: 10px 0 4px; border-bottom: 1px solid var(--border); padding-bottom: 3px; }
.ll-complexity-table { width: 100%; border-collapse: collapse; font-size: 10.5px; margin: 8px 0; }
.ll-complexity-table th, .ll-complexity-table td { border: 1px solid var(--border); padding: 4px 8px; text-align: left; }
.ll-complexity-table th { background: var(--surface2); font-weight: 700; color: var(--text2); }
.ll-cx-good { color: #15803d; font-weight: 700; } .ll-cx-mid { color: #b45309; font-weight: 700; } .ll-cx-bad { color: #b91c1c; font-weight: 700; }
.ll-cx-summary-grid { display: flex; gap: 8px; flex-wrap: wrap; margin: 6px 0 10px; }
.ll-cx-card { flex: 1; min-width: 90px; border-radius: var(--radius-sm); padding: 8px 10px; text-align: center; border: 1.5px solid var(--border); }
.ll-cx-card-good { background: #f0fdf4; border-color: #86efac; color: #15803d; }
.ll-cx-card-mid { background: #fff7ed; border-color: #fed7aa; color: #c2410c; }
.ll-cx-card-bad { background: #fef2f2; border-color: #fca5a5; color: #b91c1c; }
.ll-cx-card-label { font-size: 9px; font-weight: 700; text-transform: uppercase; letter-spacing: .05em; opacity: .7; margin-bottom: 4px; }
.ll-cx-card-val { font-size: 13px; font-weight: 800; font-family: monospace; margin-bottom: 3px; }
.ll-cx-card-note { font-size: 8.5px; opacity: .75; line-height: 1.3; }
.ll-note { background: #fefce8; border: 1px solid #fef08a; border-left: 3px solid #eab308; padding: 6px 10px; font-size: 10.5px; color: #854d0e; border-radius: 0 4px 4px 0; margin-top: 10px; margin-bottom: 120px; }

.ll-footer { display: flex; align-items: center; justify-content: space-between; padding: 4px 12px; background: var(--surface); border-top: 1px solid var(--border); font-size: 11px; color: var(--muted); font-weight: 600; flex-shrink: 0; }
.ll-speed-wrap { display: flex; align-items: center; gap: 6px; }
.ll-speed-wrap input[type="range"] { width: 80px; accent-color: var(--coral); }
</style>
