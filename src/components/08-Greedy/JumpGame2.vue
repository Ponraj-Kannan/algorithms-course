<script setup>
import { ref, reactive, computed, onMounted, onUnmounted, watch } from 'vue';

const props = defineProps({
  topic: { type: String, default: 'Greedy Algorithms' },
  subTopic: { type: String, default: 'Jump Game II' }
});

/* ==================================================================== */
/* APPROACH METADATA                                                    */
/* Brute Force comes before Greedy                                      */
/* ==================================================================== */
const APPROACHES = [
  { id: 'brute', label: 'Brute Force', desc: 'O(2^n) Exponential — Exhaustive Recursive Backtracking' },
  { id: 'greedy', label: 'Greedy', desc: 'O(n) Linear Time & O(1) Space — Optimal BFS Window Forward Scan' }
];

/* ==================================================================== */
/* CODE SNIPPETS (Java, C, C++, Python)                                 */
/* Strict curly brace formatting on all control statements              */
/* ==================================================================== */
const CODES = {
  brute: {
    java: [
      ['',                 'import java.util.Scanner;'],
      ['',                 ''],
      ['',                 'public class Main {'],
      ['c_entry',          '    static int minJumpsFrom(int idx, int[] nums, int n) {'],
      ['c_base_check',     '        if (idx >= n - 1) {'],
      ['c_ret_base',       '            return 0;'],
      ['',                 '        }'],
      ['c_get_max',        '        int maxJump = nums[idx];'],
      ['c_check_zero',     '        if (maxJump == 0) {'],
      ['c_ret_zero',       '            return 1000000;'],
      ['',                 '        }'],
      ['c_init_min',       '        int minJumps = 1000000;'],
      ['c_for_step',       '        for (int step = 1; step <= maxJump; step++) {'],
      ['c_call_rec',       '            int sub = minJumpsFrom(idx + step, nums, n);'],
      ['c_check_valid',    '            if (sub != 1000000) {'],
      ['c_check_better',   '                if (1 + sub < minJumps) {'],
      ['c_update_min',     '                    minJumps = 1 + sub;'],
      ['',                 '                }'],
      ['',                 '            }'],
      ['',                 '        }'],
      ['c_ret_min',        '        return minJumps;'],
      ['',                 '    }'],
      ['',                 ''],
      ['',                 '    public static void main(String[] args) {'],
      ['m_scanner',        '        Scanner sc = new Scanner(System.in);'],
      ['m_read_n',         '        int n = sc.nextInt();'],
      ['m_alloc_nums',     '        int[] nums = new int[n];'],
      ['m_for_input',      '        for (int i = 0; i < n; i++) {'],
      ['m_read_num',       '            nums[i] = sc.nextInt();'],
      ['',                 '        }'],
      ['m_call_jump',      '        int ans = minJumpsFrom(0, nums, n);'],
      ['m_print_ans',      '        System.out.println(ans);'],
      ['m_done',           '    }'],
      ['',                 '}']
    ],
    c: [
      ['',                 '#include <stdio.h>'],
      ['',                 ''],
      ['c_entry',          'int minJumpsFrom(int idx, int nums[], int n) {'],
      ['c_base_check',     '    if (idx >= n - 1) {'],
      ['c_ret_base',       '        return 0;'],
      ['',                 '    }'],
      ['c_get_max',        '    int maxJump = nums[idx];'],
      ['c_check_zero',     '    if (maxJump == 0) {'],
      ['c_ret_zero',       '        return 1000000;'],
      ['',                 '    }'],
      ['c_init_min',       '    int minJumps = 1000000;'],
      ['c_for_step',       '    for (int step = 1; step <= maxJump; step++) {'],
      ['c_call_rec',       '        int sub = minJumpsFrom(idx + step, nums, n);'],
      ['c_check_valid',    '        if (sub != 1000000) {'],
      ['c_check_better',   '            if (1 + sub < minJumps) {'],
      ['c_update_min',     '                minJumps = 1 + sub;'],
      ['',                 '            }'],
      ['',                 '        }'],
      ['',                 '    }'],
      ['c_ret_min',        '    return minJumps;'],
      ['',                 '}'],
      ['',                 ''],
      ['',                 'int main() {'],
      ['m_scanner',        '    int n;'],
      ['m_read_n',         '    scanf("%d", &n);'],
      ['m_alloc_nums',     '    int nums[100];'],
      ['m_for_input',      '    for (int i = 0; i < n; i++) {'],
      ['m_read_num',       '        scanf("%d", &nums[i]);'],
      ['',                 '    }'],
      ['m_call_jump',      '    int ans = minJumpsFrom(0, nums, n);'],
      ['m_print_ans',      '    printf("%d\\n", ans);'],
      ['m_done',           '    return 0;'],
      ['',                 '}']
    ],
    cpp: [
      ['',                 '#include <iostream>'],
      ['',                 '#include <vector>'],
      ['',                 'using namespace std;'],
      ['',                 ''],
      ['c_entry',          'int minJumpsFrom(int idx, const vector<int>& nums, int n) {'],
      ['c_base_check',     '    if (idx >= n - 1) {'],
      ['c_ret_base',       '        return 0;'],
      ['',                 '    }'],
      ['c_get_max',        '    int maxJump = nums[idx];'],
      ['c_check_zero',     '    if (maxJump == 0) {'],
      ['c_ret_zero',       '        return 1000000;'],
      ['',                 '    }'],
      ['c_init_min',       '    int minJumps = 1000000;'],
      ['c_for_step',       '    for (int step = 1; step <= maxJump; step++) {'],
      ['c_call_rec',       '        int sub = minJumpsFrom(idx + step, nums, n);'],
      ['c_check_valid',    '        if (sub != 1000000) {'],
      ['c_check_better',   '            if (1 + sub < minJumps) {'],
      ['c_update_min',     '                minJumps = 1 + sub;'],
      ['',                 '            }'],
      ['',                 '        }'],
      ['',                 '    }'],
      ['c_ret_min',        '    return minJumps;'],
      ['',                 '}'],
      ['',                 ''],
      ['',                 'int main() {'],
      ['m_scanner',        '    int n;'],
      ['m_read_n',         '    cin >> n;'],
      ['m_alloc_nums',     '    vector<int> nums(n);'],
      ['m_for_input',      '    for (int i = 0; i < n; i++) {'],
      ['m_read_num',       '        cin >> nums[i];'],
      ['',                 '    }'],
      ['m_call_jump',      '    int ans = minJumpsFrom(0, nums, n);'],
      ['m_print_ans',      '    cout << ans << endl;'],
      ['m_done',           '    return 0;'],
      ['',                 '}']
    ],
    python: [
      ['',                 'import sys'],
      ['',                 ''],
      ['c_entry',          'def min_jumps_from(idx, nums, n):'],
      ['c_base_check',     '    if idx >= n - 1:'],
      ['c_ret_base',       '        return 0'],
      ['c_get_max',        '    max_jump = nums[idx]'],
      ['c_check_zero',     '    if max_jump == 0:'],
      ['c_ret_zero',       '        return 1000000'],
      ['c_init_min',       '    min_jumps = 1000000'],
      ['c_for_step',       '    for step in range(1, max_jump + 1):'],
      ['c_call_rec',       '        sub = min_jumps_from(idx + step, nums, n)'],
      ['c_check_valid',    '        if sub != 1000000:'],
      ['c_check_better',   '            if 1 + sub < min_jumps:'],
      ['c_update_min',     '                min_jumps = 1 + sub'],
      ['c_ret_min',        '    return min_jumps'],
      ['',                 ''],
      ['',                 'def main():'],
      ['m_scanner',        '    tokens = sys.stdin.read().split()'],
      ['m_read_n',         '    if not tokens: return'],
      ['',                 '    n = int(tokens[0])'],
      ['m_alloc_nums',     '    nums = []'],
      ['m_for_input',      '    idx = 1'],
      ['',                 '    for i in range(n):'],
      ['m_read_num',       '        nums.append(int(tokens[idx]))'],
      ['',                 '        idx += 1'],
      ['m_call_jump',      '    ans = min_jumps_from(0, nums, n)'],
      ['m_print_ans',      '    print(ans)'],
      ['m_done',           '    return'],
      ['',                 ''],
      ['',                 'if __name__ == "__main__":'],
      ['',                 '    main()']
    ]
  },
  greedy: {
    java: [
      ['',                 'import java.util.Scanner;'],
      ['',                 ''],
      ['',                 'public class Main {'],
      ['c_entry',          '    static int jump(int[] nums, int n) {'],
      ['c_base_check',     '        if (n <= 1) {'],
      ['c_ret_zero',       '            return 0;'],
      ['',                 '        }'],
      ['c_init_jumps',     '        int jumps = 0;'],
      ['c_init_end',       '        int currentEnd = 0;'],
      ['c_init_farthest',  '        int farthest = 0;'],
      ['c_for_loop',       '        for (int i = 0; i < n - 1; i++) {'],
      ['c_check_farthest', '            if (i + nums[i] > farthest) {'],
      ['c_update_farthest','                farthest = i + nums[i];'],
      ['',                 '            }'],
      ['c_check_window',   '            if (i == currentEnd) {'],
      ['c_inc_jumps',      '                jumps++;'],
      ['c_update_window',  '                currentEnd = farthest;'],
      ['',                 '            }'],
      ['',                 '        }'],
      ['c_ret_jumps',      '        return jumps;'],
      ['',                 '    }'],
      ['',                 ''],
      ['',                 '    public static void main(String[] args) {'],
      ['m_scanner',        '        Scanner sc = new Scanner(System.in);'],
      ['m_read_n',         '        int n = sc.nextInt();'],
      ['m_alloc_nums',     '        int[] nums = new int[n];'],
      ['m_for_input',      '        for (int i = 0; i < n; i++) {'],
      ['m_read_num',       '            nums[i] = sc.nextInt();'],
      ['',                 '        }'],
      ['m_call_jump',      '        int ans = jump(nums, n);'],
      ['m_print_ans',      '        System.out.println(ans);'],
      ['m_done',           '    }'],
      ['',                 '}']
    ],
    c: [
      ['',                 '#include <stdio.h>'],
      ['',                 ''],
      ['c_entry',          'int jump(int nums[], int n) {'],
      ['c_base_check',     '    if (n <= 1) {'],
      ['c_ret_zero',       '        return 0;'],
      ['',                 '    }'],
      ['c_init_jumps',     '    int jumps = 0;'],
      ['c_init_end',       '    int currentEnd = 0;'],
      ['c_init_farthest',  '    int farthest = 0;'],
      ['c_for_loop',       '    for (int i = 0; i < n - 1; i++) {'],
      ['c_check_farthest', '        if (i + nums[i] > farthest) {'],
      ['c_update_farthest','            farthest = i + nums[i];'],
      ['',                 '        }'],
      ['c_check_window',   '        if (i == currentEnd) {'],
      ['c_inc_jumps',      '            jumps++;'],
      ['c_update_window',  '            currentEnd = farthest;'],
      ['',                 '        }'],
      ['',                 '    }'],
      ['c_ret_jumps',      '    return jumps;'],
      ['',                 '}'],
      ['',                 ''],
      ['',                 'int main() {'],
      ['m_scanner',        '    int n;'],
      ['m_read_n',         '    scanf("%d", &n);'],
      ['m_alloc_nums',     '    int nums[100];'],
      ['m_for_input',      '    for (int i = 0; i < n; i++) {'],
      ['m_read_num',       '        scanf("%d", &nums[i]);'],
      ['',                 '    }'],
      ['m_call_jump',      '    int ans = jump(nums, n);'],
      ['m_print_ans',      '    printf("%d\\n", ans);'],
      ['m_done',           '    return 0;'],
      ['',                 '}']
    ],
    cpp: [
      ['',                 '#include <iostream>'],
      ['',                 '#include <vector>'],
      ['',                 'using namespace std;'],
      ['',                 ''],
      ['c_entry',          'int jump(const vector<int>& nums, int n) {'],
      ['c_base_check',     '    if (n <= 1) {'],
      ['c_ret_zero',       '        return 0;'],
      ['',                 '    }'],
      ['c_init_jumps',     '    int jumps = 0;'],
      ['c_init_end',       '    int currentEnd = 0;'],
      ['c_init_farthest',  '    int farthest = 0;'],
      ['c_for_loop',       '    for (int i = 0; i < n - 1; i++) {'],
      ['c_check_farthest', '        if (i + nums[i] > farthest) {'],
      ['c_update_farthest','            farthest = i + nums[i];'],
      ['',                 '        }'],
      ['c_check_window',   '        if (i == currentEnd) {'],
      ['c_inc_jumps',      '            jumps++;'],
      ['c_update_window',  '            currentEnd = farthest;'],
      ['',                 '        }'],
      ['',                 '    }'],
      ['c_ret_jumps',      '    return jumps;'],
      ['',                 '}'],
      ['',                 ''],
      ['',                 'int main() {'],
      ['m_scanner',        '    int n;'],
      ['m_read_n',         '    cin >> n;'],
      ['m_alloc_nums',     '    vector<int> nums(n);'],
      ['m_for_input',      '    for (int i = 0; i < n; i++) {'],
      ['m_read_num',       '        cin >> nums[i];'],
      ['',                 '    }'],
      ['m_call_jump',      '    int ans = jump(nums, n);'],
      ['m_print_ans',      '    cout << ans << endl;'],
      ['m_done',           '    return 0;'],
      ['',                 '}']
    ],
    python: [
      ['',                 'import sys'],
      ['',                 ''],
      ['c_entry',          'def jump(nums, n):'],
      ['c_base_check',     '    if n <= 1:'],
      ['c_ret_zero',       '        return 0'],
      ['c_init_jumps',     '    jumps = 0'],
      ['c_init_end',       '    current_end = 0'],
      ['c_init_farthest',  '    farthest = 0'],
      ['c_for_loop',       '    for i in range(n - 1):'],
      ['c_check_farthest', '        if i + nums[i] > farthest:'],
      ['c_update_farthest','            farthest = i + nums[i]'],
      ['c_check_window',   '        if i == current_end:'],
      ['c_inc_jumps',      '            jumps += 1'],
      ['c_update_window',  '            current_end = farthest'],
      ['c_ret_jumps',      '    return jumps'],
      ['',                 ''],
      ['',                 'def main():'],
      ['m_scanner',        '    tokens = sys.stdin.read().split()'],
      ['m_read_n',         '    if not tokens: return'],
      ['',                 '    n = int(tokens[0])'],
      ['m_alloc_nums',     '    nums = []'],
      ['m_for_input',      '    idx = 1'],
      ['',                 '    for i in range(n):'],
      ['m_read_num',       '        nums.append(int(tokens[idx]))'],
      ['',                 '        idx += 1'],
      ['m_call_jump',      '    ans = jump(nums, n)'],
      ['m_print_ans',      '    print(ans)'],
      ['m_done',           '    return'],
      ['',                 ''],
      ['',                 'if __name__ == "__main__":'],
      ['',                 '    main()']
    ]
  }
};

/* ==================================================================== */
/* PSEUDOCODE FOR STUDENTS                                              */
/* ==================================================================== */
const PSEUDOCODES = {
  brute: [
    'function minJumpsFrom(idx, nums, n):',
    '    if idx >= n - 1:                            // Base case: at or beyond destination',
    '        return 0                                // 0 additional jumps needed',
    '    maxJump = nums[idx]',
    '    if maxJump == 0: return INF                 // Dead end, cannot advance',
    '    minJumps = INF',
    '    for step = 1 to maxJump:                    // Try every viable leap distance',
    '        sub = minJumpsFrom(idx + step, nums, n)',
    '        if sub != INF: minJumps = min(minJumps, 1 + sub)',
    '    return minJumps                             // Time: O(2^n) exponential search'
  ],
  greedy: [
    'function jump(nums, n):',
    '    if n <= 1: return 0                         // Already at destination',
    '    jumps = 0, currentEnd = 0, farthest = 0',
    '    for i = 0 to n - 2:                         // BFS levels forward scan',
    '        farthest = max(farthest, i + nums[i])   // Update horizon of next jump',
    '        if i == currentEnd:                     // Reached boundary of current jump',
    '            jumps = jumps + 1                   // Must commit to another jump',
    '            currentEnd = farthest               // Advance current window to farthest',
    '    return jumps                                // Optimal Time: O(n), Space: O(1)'
  ]
};

/* ==================================================================== */
/* DEFAULT SAMPLE DATA                                                  */
/* ==================================================================== */
const DEFAULT_NUMS = [2, 3, 1, 1, 4];

function frame(title, rows) {
  return { title, rows };
}

/* ==================================================================== */
/* BUILD STEPS FOR ANIMATION (STRICT 1-TO-1 LINE-BY-LINE)               */
/* ==================================================================== */
function buildSteps(approach, rawNums) {
  const steps = [];
  const nums = (rawNums && rawNums.length) ? [...rawNums] : [...DEFAULT_NUMS];
  const n = nums.length;

  /* ------------------------------------------------------------------ */
  /* APPROACH 1: BRUTE FORCE (RECURSIVE BACKTRACKING TREE)              */
  /* ------------------------------------------------------------------ */
  if (approach === 'brute') {
    const allNodes = [];
    let nodeIdCounter = 0;

    function buildBruteTree(idx, parentId, jumpStep) {
      const nodeId = nodeIdCounter++;
      const node = {
        id: nodeId,
        idx,
        parentId,
        jumpStep,
        children: [],
        isGoal: idx >= n - 1,
        minJumps: (idx >= n - 1) ? 0 : 1000000
      };
      allNodes.push(node);
      if (parentId !== null) {
        const par = allNodes.find(x => x.id === parentId);
        if (par) par.children.push(nodeId);
      }
      if (idx >= n - 1) {
        return 0;
      }
      const maxJ = Math.min(nums[idx] || 0, n);
      if (maxJ === 0) {
        return 1000000;
      }
      let best = 1000000;
      for (let s = 1; s <= maxJ; s++) {
        const sub = buildBruteTree(idx + s, nodeId, s);
        if (sub !== 1000000 && 1 + sub < best) {
          best = 1 + sub;
        }
      }
      node.minJumps = best;
      return best;
    }
    buildBruteTree(0, null, 0);

    const nodeStateMap = {};
    allNodes.forEach(nd => { nodeStateMap[nd.id] = { ...nd, state: 'hidden', computedMin: null }; });

    const leafSpacing = 44;
    const levelHeight = 36;
    let leafCounter = 0;

    function assignSubtreeLeaves(nodeId, depth) {
      const nd = allNodes.find(x => x.id === nodeId);
      if (!nd) return;
      nd._depth = depth;
      if (nd.children.length === 0) {
        nd._leafIdx = leafCounter++;
      } else {
        nd.children.forEach(cid => assignSubtreeLeaves(cid, depth + 1));
      }
    }
    assignSubtreeLeaves(0, 0);

    const totalLeaves = Math.max(1, leafCounter);
    const maxDepth = Math.max(0, ...allNodes.map(nd => nd._depth || 0));
    const treeWidth = Math.max(240, totalLeaves * leafSpacing + 30);
    const treeHeight = Math.max(120, maxDepth * levelHeight + 46);

    function assignCoords(nodeId) {
      const nd = allNodes.find(x => x.id === nodeId);
      if (!nd) return 0;
      if (nd.children.length === 0) {
        nd._x = 15 + nd._leafIdx * leafSpacing + leafSpacing / 2;
      } else {
        const childXs = nd.children.map(cid => assignCoords(cid));
        nd._x = (childXs[0] + childXs[childXs.length - 1]) / 2;
      }
      nd._y = 20 + nd._depth * levelHeight;
      return nd._x;
    }
    assignCoords(0);

    function getVisibleNodes() {
      return allNodes.filter(nd => nodeStateMap[nd.id].state !== 'hidden')
        .map(nd => ({ ...nd, ...nodeStateMap[nd.id], x: nd._x, y: nd._y }));
    }
    function getVisibleEdges() {
      const edges = [];
      allNodes.forEach(nd => {
        if (nd.parentId !== null && nodeStateMap[nd.id].state !== 'hidden' && nodeStateMap[nd.parentId].state !== 'hidden') {
          const par = allNodes.find(x => x.id === nd.parentId);
          if (par) {
            edges.push({
              x1: par._x, y1: par._y + 11,
              x2: nd._x, y2: nd._y - 11,
              step: nd.jumpStep,
              isOptimal: nd.minJumps !== 1000000
            });
          }
        }
      });
      return edges;
    }

    function curBruteState(extra = {}) {
      return {
        approach: 'brute',
        n,
        nums,
        treeWidth,
        treeHeight,
        treeNodes: getVisibleNodes(),
        treeEdges: getVisibleEdges(),
        ...extra
      };
    }

    steps.push({
      code: 'm_scanner',
      badge: 'Faculty Note: Scanner sc = new Scanner(System.in); \u2014 Initializing input stream.',
      vars: [frame('main()', [['n', '?']])],
      state: curBruteState({ callCounter: 0, activeNodeId: null, currentIdx: -1, numsFilled: 0 })
    });
    steps.push({
      code: 'm_read_n',
      badge: `Faculty Note: int n = sc.nextInt(); &rarr; Read array size n = ${n}.`,
      vars: [frame('main()', [['n', String(n)]])],
      state: curBruteState({ callCounter: 0, activeNodeId: null, currentIdx: -1, numsFilled: 0 })
    });
    steps.push({
      code: 'm_alloc_nums',
      badge: `Faculty Note: int[] nums = new int[${n}]; &mdash; Allocating jump capacity array.`,
      vars: [frame('main()', [['n', String(n)]])],
      state: curBruteState({ callCounter: 0, activeNodeId: null, currentIdx: -1, numsFilled: 0 })
    });

    for (let idx = 0; idx < n; idx++) {
      steps.push({
        code: 'm_for_input',
        badge: `Reading element at index ${idx}.`,
        vars: [frame('main()', [['i', String(idx)], ['n', String(n)]])],
        state: curBruteState({ callCounter: 0, activeNodeId: null, currentIdx: idx, numsFilled: idx })
      });
      steps.push({
        code: 'm_read_num',
        badge: `nums[${idx}] = sc.nextInt(); \u2192 nums[${idx}] = ${nums[idx]} stored.`,
        vars: [frame('main()', [['i', String(idx)], ['nums[' + idx + ']', String(nums[idx])]])],
        state: curBruteState({ callCounter: 0, activeNodeId: null, currentIdx: idx, numsFilled: idx + 1 })
      });
    }

    steps.push({
      code: 'm_call_jump',
      badge: `Invoking minJumpsFrom(idx=0, nums, n=${n}). Exhaustive backtracking search for minimum jumps begins!`,
      vars: [frame('main()', [['n', String(n)]])],
      state: curBruteState({ callCounter: 0, activeNodeId: null, currentIdx: 0 })
    });

    let calls = 0;

    function simulateBrute(idx, nodeId, callStack) {
      calls++;
      nodeStateMap[nodeId].state = 'active';
      const cs = [
        frame('main()', [['n', String(n)]]),
        ...callStack,
        frame(`minJumpsFrom(idx=${idx})`, [['idx', String(idx)], ['nums[idx]', String(nums[idx] || 0)]])
      ];

      steps.push({
        code: 'c_entry',
        badge: `Entering minJumpsFrom(idx=${idx}). Recursive Call #${calls}.`,
        vars: cs,
        state: curBruteState({ callCounter: calls, activeNodeId: nodeId, currentIdx: idx })
      });

      const isBase = (idx >= n - 1);
      steps.push({
        code: 'c_base_check',
        badge: `Base check: if (idx=${idx} >= n - 1 = ${n - 1}) &rarr; ${isBase ? 'TRUE (Already at destination!)' : 'FALSE (Must jump forward)'}`,
        vars: cs,
        state: curBruteState({ callCounter: calls, activeNodeId: nodeId, currentIdx: idx })
      });

      if (isBase) {
        nodeStateMap[nodeId].state = 'leaf';
        nodeStateMap[nodeId].computedMin = 0;
        steps.push({
          code: 'c_ret_base',
          badge: `return 0; Destination reached at index ${idx}. Requires 0 additional jumps.`,
          vars: cs,
          state: curBruteState({ callCounter: calls, activeNodeId: nodeId, currentIdx: idx })
        });
        nodeStateMap[nodeId].state = 'solved';
        return 0;
      }

      const maxJump = nums[idx] || 0;
      steps.push({
        code: 'c_get_max',
        badge: `int maxJump = nums[${idx}] = ${maxJump}. Stone ${idx} allows jumping 1 to ${maxJump} steps forward.`,
        vars: cs,
        state: curBruteState({ callCounter: calls, activeNodeId: nodeId, currentIdx: idx })
      });

      const isZero = (maxJump === 0);
      steps.push({
        code: 'c_check_zero',
        badge: `Check zero capacity: if (maxJump == 0) &rarr; ${isZero ? 'TRUE (Zero trap dead end!)' : 'FALSE (Can make forward progress)'}`,
        vars: cs,
        state: curBruteState({ callCounter: calls, activeNodeId: nodeId, currentIdx: idx })
      });

      if (isZero) {
        nodeStateMap[nodeId].state = 'solved';
        nodeStateMap[nodeId].computedMin = 1000000;
        steps.push({
          code: 'c_ret_zero',
          badge: `return 1000000 (INF); Cannot leap anywhere from stone ${idx}.`,
          vars: cs,
          state: curBruteState({ callCounter: calls, activeNodeId: nodeId, currentIdx: idx })
        });
        return 1000000;
      }

      let minJumps = 1000000;
      steps.push({
        code: 'c_init_min',
        badge: 'int minJumps = 1000000; Initializing minimum jumps tracker to infinity.',
        vars: cs,
        state: curBruteState({ callCounter: calls, activeNodeId: nodeId, currentIdx: idx })
      });

      const ndObj = allNodes.find(x => x.id === nodeId);
      const childIds = ndObj ? ndObj.children : [];

      for (let s = 1; s <= maxJump; s++) {
        steps.push({
          code: 'c_for_step',
          badge: `Trying step = ${s} from stone ${idx} to stone ${idx + s}.`,
          vars: cs,
          state: curBruteState({ callCounter: calls, activeNodeId: nodeId, currentIdx: idx })
        });

        steps.push({
          code: 'c_call_rec',
          badge: `Evaluating recursive branch minJumpsFrom(idx + step = ${idx + s})...`,
          vars: cs,
          state: curBruteState({ callCounter: calls, activeNodeId: nodeId, currentIdx: idx })
        });

        const cId = childIds[s - 1];
        let sub = 1000000;
        if (cId !== undefined) {
          sub = simulateBrute(idx + s, cId, [...callStack, frame(`minJumpsFrom(idx=${idx})`, [['idx', String(idx)]])]);
        }

        const isValid = (sub !== 1000000);
        steps.push({
          code: 'c_check_valid',
          badge: `Check branch validity: if (sub=${sub === 1000000 ? 'INF' : sub} != INF) &rarr; ${isValid ? 'TRUE (Path reaches destination)' : 'FALSE (Dead end path)'}`,
          vars: cs,
          state: curBruteState({ callCounter: calls, activeNodeId: nodeId, currentIdx: idx })
        });

        if (isValid) {
          const isBetter = (1 + sub < minJumps);
          steps.push({
            code: 'c_check_better',
            badge: `Check if better: if (1 + sub = ${1 + sub} < minJumps = ${minJumps === 1000000 ? 'INF' : minJumps}) &rarr; ${isBetter ? 'TRUE (New minimum found!)' : 'FALSE (Existing option is shorter or equal)'}`,
            vars: cs,
            state: curBruteState({ callCounter: calls, activeNodeId: nodeId, currentIdx: idx })
          });

          if (isBetter) {
            minJumps = 1 + sub;
            steps.push({
              code: 'c_update_min',
              badge: `minJumps updated to ${minJumps} (1 jump from stone ${idx} + ${sub} from stone ${idx + s})!`,
              vars: cs,
              state: curBruteState({ callCounter: calls, activeNodeId: nodeId, currentIdx: idx })
            });
          }
        }
      }

      nodeStateMap[nodeId].state = 'solved';
      nodeStateMap[nodeId].computedMin = minJumps;

      steps.push({
        code: 'c_ret_min',
        badge: `return minJumps = ${minJumps === 1000000 ? 'INF' : minJumps}; Best path from stone ${idx} requires ${minJumps} jumps.`,
        vars: cs,
        state: curBruteState({ callCounter: calls, activeNodeId: nodeId, currentIdx: idx })
      });

      return minJumps;
    }

    const finalAns = simulateBrute(0, 0, []);

    steps.push({
      code: 'm_print_ans',
      badge: `Output Result: System.out.println(${finalAns}); — Exhaustive search complete! Total calls explored: ${calls}.`,
      vars: [frame('main()', [['ans', String(finalAns)]])],
      state: curBruteState({ callCounter: calls, activeNodeId: null, currentIdx: -1 })
    });

    steps.push({
      code: 'm_done',
      badge: `Program execution finished. Minimum jumps required = ${finalAns}.`,
      vars: [frame('main()', [['status', 'finished']])],
      state: curBruteState({ callCounter: calls, activeNodeId: null, currentIdx: -1 })
    });

    return steps;
  }

  /* ------------------------------------------------------------------ */
  /* APPROACH 2: GREEDY (BFS INTERVAL LEVEL FORWARD SCAN)               */
  /* ------------------------------------------------------------------ */
  else {
    function curGreedyState(extra = {}) {
      return {
        approach: 'greedy',
        n,
        nums,
        ...extra
      };
    }

    steps.push({
      code: 'm_scanner',
      badge: 'Faculty Note: Scanner sc = new Scanner(System.in); \u2014 Initializing input stream.',
      vars: [frame('main()', [['n', '?']])],
      state: curGreedyState({ i: -1, jumps: 0, currentEnd: 0, farthest: 0, status: 'input', numsFilled: 0 })
    });
    steps.push({
      code: 'm_read_n',
      badge: `Faculty Note: int n = sc.nextInt(); &rarr; Read array size n = ${n}.`,
      vars: [frame('main()', [['n', String(n)]])],
      state: curGreedyState({ i: -1, jumps: 0, currentEnd: 0, farthest: 0, status: 'input', numsFilled: 0 })
    });
    steps.push({
      code: 'm_alloc_nums',
      badge: `Faculty Note: int[] nums = new int[${n}]; &mdash; Allocated jump capacity array.`,
      vars: [frame('main()', [['n', String(n)]])],
      state: curGreedyState({ i: -1, jumps: 0, currentEnd: 0, farthest: 0, status: 'input', numsFilled: 0 })
    });

    for (let idx = 0; idx < n; idx++) {
      steps.push({
        code: 'm_for_input',
        badge: `Reading jump capacity at index ${idx}.`,
        vars: [frame('main()', [['i', String(idx)], ['n', String(n)]])],
        state: curGreedyState({ i: -1, jumps: 0, currentEnd: 0, farthest: 0, status: 'input', numsFilled: idx })
      });
      steps.push({
        code: 'm_read_num',
        badge: `nums[${idx}] = sc.nextInt(); \u2192 nums[${idx}] = ${nums[idx]} stored. (From stone ${idx}, you can leap 1 to ${nums[idx]} stones forward).`,
        vars: [frame('main()', [['i', String(idx)], ['nums[' + idx + ']', String(nums[idx])]])],
        state: curGreedyState({ i: -1, jumps: 0, currentEnd: 0, farthest: 0, status: 'input', numsFilled: idx + 1 })
      });
    }

    steps.push({
      code: 'm_call_jump',
      badge: `Calling jump(nums, n=${n}). Optimal BFS Greedy algorithm begins!`,
      vars: [frame('main()', [['n', String(n)]])],
      state: curGreedyState({ i: -1, jumps: 0, currentEnd: 0, farthest: 0, status: 'call' })
    });

    steps.push({
      code: 'c_entry',
      badge: `Entering jump(nums, n=${n}). Goal: reach destination index ${n - 1} in minimal jumps.`,
      vars: [frame('main()', [['n', String(n)]]), frame('jump()', [['n', String(n)]])],
      state: curGreedyState({ i: -1, jumps: 0, currentEnd: 0, farthest: 0, status: 'entry' })
    });

    const isSmall = (n <= 1);
    steps.push({
      code: 'c_base_check',
      badge: `Check base condition: if (n <= 1) &rarr; ${isSmall ? 'TRUE (Already at destination)' : 'FALSE (Array length > 1, jumps required)'}`,
      vars: [frame('main()', [['n', String(n)]]), frame('jump()', [['n', String(n)]])],
      state: curGreedyState({ i: -1, jumps: 0, currentEnd: 0, farthest: 0, status: isSmall ? 'done' : 'running' })
    });

    if (isSmall) {
      steps.push({
        code: 'c_ret_zero',
        badge: 'return 0; Already starting on the destination stone. 0 jumps needed.',
        vars: [frame('main()', [['n', String(n)]]), frame('jump()', [['return', '0']])],
        state: curGreedyState({ i: 0, jumps: 0, currentEnd: 0, farthest: 0, status: 'success' })
      });
      steps.push({
        code: 'm_print_ans',
        badge: 'Output Result: System.out.println(0);',
        vars: [frame('main()', [['ans', '0']])],
        state: curGreedyState({ i: 0, jumps: 0, currentEnd: 0, farthest: 0, status: 'success' })
      });
      steps.push({
        code: 'm_done',
        badge: 'Program execution complete.',
        vars: [frame('main()', [['status', 'finished']])],
        state: curGreedyState({ i: 0, jumps: 0, currentEnd: 0, farthest: 0, status: 'success' })
      });
      return steps;
    }

    let jumps = 0;
    steps.push({
      code: 'c_init_jumps',
      badge: 'int jumps = 0; Number of forward leaps taken initialized to 0.',
      vars: [frame('main()', [['n', String(n)]]), frame('jump()', [['jumps', '0']])],
      state: curGreedyState({ i: 0, jumps: 0, currentEnd: 0, farthest: 0, status: 'running' })
    });

    let currentEnd = 0;
    steps.push({
      code: 'c_init_end',
      badge: 'int currentEnd = 0; The boundary of the current jump horizon level is index 0.',
      vars: [frame('main()', [['n', String(n)]]), frame('jump()', [['jumps', '0'], ['currentEnd', '0']])],
      state: curGreedyState({ i: 0, jumps: 0, currentEnd: 0, farthest: 0, status: 'running' })
    });

    let farthest = 0;
    steps.push({
      code: 'c_init_farthest',
      badge: 'int farthest = 0; Maximum forward reach discoverable within current horizon level initialized to 0.',
      vars: [frame('main()', [['n', String(n)]]), frame('jump()', [['jumps', '0'], ['currentEnd', '0'], ['farthest', '0']])],
      state: curGreedyState({ i: 0, jumps: 0, currentEnd: 0, farthest: 0, status: 'running' })
    });

    for (let i = 0; i < n - 1; i++) {
      steps.push({
        code: 'c_for_loop',
        badge: `Scanning stone i = ${i} (nums[${i}] = ${nums[i]}). Current jump boundary end = ${currentEnd}.`,
        vars: [frame('main()', [['n', String(n)]]), frame('jump()', [['i', String(i)], ['nums[i]', String(nums[i])], ['jumps', String(jumps)], ['currentEnd', String(currentEnd)], ['farthest', String(farthest)]])],
        state: curGreedyState({ i, jumps, currentEnd, farthest, status: 'running' })
      });

      const potReach = i + nums[i];
      const expands = potReach > farthest;
      steps.push({
        code: 'c_check_farthest',
        badge: `Evaluate reach from stone ${i}: i + nums[${i}] = ${i} + ${nums[i]} = ${potReach}. Check: potReach > farthest (${potReach} > ${farthest}) &rarr; ${expands ? 'TRUE (New farthest reach discovered!)' : 'FALSE (Does not exceed current farthest)'}`,
        vars: [frame('main()', [['n', String(n)]]), frame('jump()', [['i', String(i)], ['potReach', String(potReach)], ['farthest', String(farthest)]])],
        state: curGreedyState({ i, jumps, currentEnd, farthest, arcFrom: i, arcTo: potReach, status: 'running' })
      });

      if (expands) {
        farthest = potReach;
        steps.push({
          code: 'c_update_farthest',
          badge: `farthest updated to ${farthest}! Next jump level boundary pushed further forward.`,
          vars: [frame('main()', [['n', String(n)]]), frame('jump()', [['i', String(i)], ['farthest', String(farthest)]])],
          state: curGreedyState({ i, jumps, currentEnd, farthest, arcFrom: i, arcTo: potReach, status: 'running' })
        });
      }

      const hitEnd = (i === currentEnd);
      steps.push({
        code: 'c_check_window',
        badge: `Check window horizon: if (i=${i} == currentEnd=${currentEnd}) &rarr; ${hitEnd ? 'TRUE! Reached boundary of current jump level! Must commit to a new jump.' : 'FALSE (Continue scanning remaining stones in current window)'}`,
        vars: [frame('main()', [['n', String(n)]]), frame('jump()', [['i', String(i)], ['currentEnd', String(currentEnd)]])],
        state: curGreedyState({ i, jumps, currentEnd, farthest, status: hitEnd ? 'transition' : 'running' })
      });

      if (hitEnd) {
        jumps++;
        steps.push({
          code: 'c_inc_jumps',
          badge: `jumps++ &rarr; Total jumps committed = ${jumps}! Level transition completed.`,
          vars: [frame('main()', [['n', String(n)]]), frame('jump()', [['jumps', String(jumps)]])],
          state: curGreedyState({ i, jumps, currentEnd, farthest, status: 'transition' })
        });

        currentEnd = farthest;
        steps.push({
          code: 'c_update_window',
          badge: `currentEnd = farthest = ${currentEnd}! Jump boundary extends forward to index ${currentEnd}.`,
          vars: [frame('main()', [['n', String(n)]]), frame('jump()', [['currentEnd', String(currentEnd)]])],
          state: curGreedyState({ i, jumps, currentEnd, farthest, status: 'running' })
        });
      }
    }

    steps.push({
      code: 'c_ret_jumps',
      badge: `return jumps = ${jumps}; Finished scan. Minimum jumps needed to reach destination index ${n - 1} is ${jumps}.`,
      vars: [frame('main()', [['n', String(n)]]), frame('jump()', [['return', String(jumps)]])],
      state: curGreedyState({ i: n - 1, jumps, currentEnd, farthest, status: 'success' })
    });

    steps.push({
      code: 'm_print_ans',
      badge: `Output Result: System.out.println(${jumps}); — Solved in O(n) single forward pass!`,
      vars: [frame('main()', [['ans', String(jumps)]])],
      state: curGreedyState({ i: -1, jumps, currentEnd, farthest, status: 'success' })
    });

    steps.push({
      code: 'm_done',
      badge: `Program execution finished. Minimum number of jumps = ${jumps}.`,
      vars: [frame('main()', [['status', 'finished']])],
      state: curGreedyState({ i: -1, jumps, currentEnd, farthest, status: 'success' })
    });

    return steps;
  }
}

/* ==================================================================== */
/* COMPONENT STATE & REACTIVE SETUP                                     */
/* ==================================================================== */
const currentApproach = ref('brute');
const inputNumsText = ref('2, 3, 1, 1, 4');
const lang = ref('java');
const speed = ref(650);
const si = ref(0);
const playing = ref(false);
const vizHeight = ref(195);
const tableHeight = ref(42);
const leftWidth = ref(54);
const rightTab = ref('code');

function parseArray(text) {
  return text.split(/[,;\s]+/)
    .map(x => parseInt(x.trim(), 10))
    .filter(x => !isNaN(x));
}

const currentNums = computed(() => {
  const arr = parseArray(inputNumsText.value);
  return arr.length >= 1 ? arr : DEFAULT_NUMS;
});

const stepsData = reactive({
  steps: buildSteps('brute', DEFAULT_NUMS)
});

const steps = computed(() => stepsData.steps);
const s = computed(() => steps.value[Math.max(0, Math.min(si.value, steps.value.length - 1))] || { state: {} });
const st = computed(() => s.value.state || {});

const codeLines = computed(() => {
  const appCodes = CODES[currentApproach.value] || CODES.brute;
  return appCodes[lang.value] || appCodes.java || [];
});
const pseudocodeLines = computed(() => PSEUDOCODES[currentApproach.value] || PSEUDOCODES.brute);

let playTimer = null;

function applyApproach(appId) {
  currentApproach.value = appId;
  resetAll();
}

function applyInput() {
  resetAll();
}

function resetAll() {
  pause();
  stepsData.steps = buildSteps(currentApproach.value, currentNums.value);
  si.value = 0;
}

function stepBy(delta) {
  pause();
  const next = si.value + delta;
  if (next >= 0 && next < steps.value.length) {
    si.value = next;
  }
}

function togglePlay() {
  if (playing.value) {
    pause();
  } else {
    play();
  }
}

function play() {
  if (si.value >= steps.value.length - 1) {
    si.value = 0;
  }
  playing.value = true;
  scheduleNext();
}

function pause() {
  playing.value = false;
  if (playTimer) {
    clearTimeout(playTimer);
    playTimer = null;
  }
}

function scheduleNext() {
  if (!playing.value) return;
  playTimer = setTimeout(() => {
    if (!playing.value) return;
    if (si.value < steps.value.length - 1) {
      si.value++;
      scheduleNext();
    } else {
      pause();
    }
  }, speed.value);
}

/* Horizontal resizer */
const mainRef = ref(null);
const hResizerRef = ref(null);
let isDraggingH = false;

function onHMouseDown(e) {
  isDraggingH = true;
  hResizerRef.value?.classList.add('drag');
  document.addEventListener('mousemove', onHMouseMove);
  document.addEventListener('mouseup', onHMouseUp);
}
function onHMouseMove(e) {
  if (!isDraggingH || !mainRef.value) return;
  const rect = mainRef.value.getBoundingClientRect();
  const pct = ((e.clientX - rect.left) / rect.width) * 100;
  if (pct >= 25 && pct <= 75) {
    leftWidth.value = Math.round(pct);
  }
}
function onHMouseUp() {
  isDraggingH = false;
  hResizerRef.value?.classList.remove('drag');
  document.removeEventListener('mousemove', onHMouseMove);
  document.removeEventListener('mouseup', onHMouseUp);
}

/* Vertical resizers */
const leftColRef = ref(null);
const vizResizerRef = ref(null);
let isDraggingViz = false;
let startYViz = 0;
let startHeightViz = 0;

function onVizMouseDown(e) {
  isDraggingViz = true;
  startYViz = e.clientY;
  startHeightViz = vizHeight.value;
  vizResizerRef.value?.classList.add('drag');
  document.addEventListener('mousemove', onVizMouseMove);
  document.addEventListener('mouseup', onVizMouseUp);
}
function onVizMouseMove(e) {
  if (!isDraggingViz) return;
  const dy = e.clientY - startYViz;
  vizHeight.value = Math.max(160, Math.min(480, startHeightViz + dy));
}
function onVizMouseUp() {
  isDraggingViz = false;
  vizResizerRef.value?.classList.remove('drag');
  document.removeEventListener('mousemove', onVizMouseMove);
  document.removeEventListener('mouseup', onVizMouseUp);
}

const tableResizerRef = ref(null);
let isDraggingTable = false;
let startYTable = 0;
let startHeightTable = 0;

function onTableMouseDown(e) {
  isDraggingTable = true;
  startYTable = e.clientY;
  startHeightTable = tableHeight.value;
  tableResizerRef.value?.classList.add('drag');
  document.addEventListener('mousemove', onTableMouseMove);
  document.addEventListener('mouseup', onTableMouseUp);
}
function onTableMouseMove(e) {
  if (!isDraggingTable) return;
  const dy = e.clientY - startYTable;
  tableHeight.value = Math.max(30, Math.min(160, startHeightTable + dy));
}
function onTableMouseUp() {
  isDraggingTable = false;
  tableResizerRef.value?.classList.remove('drag');
  document.removeEventListener('mousemove', onTableMouseMove);
  document.removeEventListener('mouseup', onTableMouseUp);
}

onMounted(() => {
  hResizerRef.value?.addEventListener('mousedown', onHMouseDown);
  vizResizerRef.value?.addEventListener('mousedown', onVizMouseDown);
  tableResizerRef.value?.addEventListener('mousedown', onTableMouseDown);
  resetAll();
});

onUnmounted(() => {
  pause();
  hResizerRef.value?.removeEventListener('mousedown', onHMouseDown);
  vizResizerRef.value?.removeEventListener('mousedown', onVizMouseDown);
  tableResizerRef.value?.removeEventListener('mousedown', onTableMouseDown);
});

watch(lang, () => {});
</script>

<template>
  <div class="slide-wrapper">
    <!-- Top Navbar with FACE Prep Logo -->
    <div class="navbar">
      <h2 class="navbar-title">{{ topic }} &mdash; {{ subTopic }}</h2>
      <img src="../../assets/logo.png" alt="FACE Prep" />
    </div>

    <div class="slide-body">
      <div class="row-main">
        <div class="ll-root">
          <!-- Control Toolbar -->
          <div class="ll-toolbar">
            <!-- Approach Selector Button Group (Brute Force first) -->
            <div class="ll-approach-group">
              <button
                v-for="app in APPROACHES"
                :key="app.id"
                class="ll-approach-btn"
                :class="{ active: currentApproach === app.id }"
                :title="app.desc"
                @click="applyApproach(app.id)"
              >
                {{ app.label }}
              </button>
            </div>

            <!-- Custom Array Input -->
            <div class="ll-input-group">
              <label>nums =</label>
              <input
                v-model="inputNumsText"
                class="ll-text-input"
                placeholder="2, 3, 1, 1, 4"
                @keyup.enter="applyInput"
                style="width: 140px;"
              />
            </div>

            <button class="ll-viz-btn" @click="applyInput">&#9654; Visualize</button>

            <!-- Playback Navigation Controls -->
            <div class="ll-nav-controls">
              <button class="ll-nav-btn" title="First step" @click="stepBy(-steps.length)">&#171;</button>
              <button class="ll-nav-btn" title="Previous step" @click="stepBy(-1)">&#8249; Prev</button>
              <button class="ll-play-btn" @click="togglePlay">
                {{ playing ? '\u23F8 Pause' : '\u25B6 Play' }}
              </button>
              <button class="ll-nav-btn" title="Next step" @click="stepBy(1)">Next &#8250;</button>
              <button class="ll-nav-btn" title="Last step" @click="stepBy(steps.length)">&#187;</button>
            </div>
          </div>

          <div class="ll-main" ref="mainRef">
            <!-- Left Visualization Column -->
            <div class="ll-left-col" ref="leftColRef" :style="{ width: leftWidth + '%' }">
              <!-- Top Banner Displaying the Stepping Stone Array -->
              <div class="ll-house-banner">
                <div class="ll-house-title">
                  Stepping Stone Array: <code>nums[0 ... {{ currentNums.length - 1 }}]</code>
                </div>

                <div class="ll-dual-arrays-wrap">
                  <div class="ll-arr-row">
                    <span class="ll-arr-tag">nums[]:</span>
                    <div class="ll-arr-cells">
                      <div
                        v-for="(num, idx) in currentNums"
                        :key="'n' + idx"
                        class="ll-arr-cell"
                        :class="{
                          'll-cell-active': st.i === idx || st.currentIdx === idx,
                          'll-cell-reach': st.farthest !== undefined && idx <= st.farthest,
                          'll-cell-goal': idx === currentNums.length - 1
                        }"
                      >
                        <span class="ll-cell-idx">[{{ idx }}]</span>
                        <span class="ll-cell-val">{{ (st.numsFilled !== undefined && idx >= st.numsFilled) ? '?' : num }}</span>
                      </div>
                    </div>
                  </div>
                </div>
              </div>

              <!-- Real-time Stats Chips Bar -->
              <div class="ll-ptrs">
                <div class="ll-ptr-chip">Array Size n: <b class="ll-c-blue">{{ st.n || currentNums.length }}</b></div>
                <template v-if="currentApproach === 'greedy'">
                  <div class="ll-ptr-chip" v-if="st.i >= 0">
                    Scanning Stone: <b class="ll-c-orange">i = {{ st.i }} (nums = {{ currentNums[st.i] }})</b>
                  </div>
                  <div class="ll-ptr-chip">
                    Jumps Committed: <b class="ll-c-green">{{ st.jumps || 0 }}</b>
                  </div>
                  <div class="ll-ptr-chip" v-if="st.currentEnd !== undefined">
                    Current Level End: <b class="ll-c-purple">index {{ st.currentEnd }}</b>
                  </div>
                  <div class="ll-ptr-chip" v-if="st.farthest !== undefined">
                    Farthest Reach: <b class="ll-c-blue">index {{ st.farthest }}</b>
                  </div>
                  <div class="ll-ptr-chip">
                    Destination: <b class="ll-c-purple">Index {{ currentNums.length - 1 }} [Target]</b>
                  </div>
                </template>
                <template v-else>
                  <div class="ll-ptr-chip">Recursive Calls: <b class="ll-c-orange">{{ st.callCounter || 0 }}</b></div>
                  <div class="ll-ptr-chip">
                    Current Stone: <b class="ll-c-blue">{{ st.currentIdx >= 0 ? 'idx = ' + st.currentIdx : 'Start (0)' }}</b>
                  </div>
                  <div class="ll-ptr-chip">
                    Destination: <b class="ll-c-purple">Index {{ currentNums.length - 1 }} [Target]</b>
                  </div>
                </template>
              </div>

              <!-- Visualization Viewport Container -->
              <div class="ll-viz-wrap" :style="{ height: vizHeight + 'px' }">
                <!-- VIEW 1: GREEDY BFS HORIZON WINDOW & ARC ANIMATION -->
                <div v-if="currentApproach === 'greedy'" class="ll-timeline-container">
                  <div class="ll-section-caption">
                    <span>Greedy BFS Horizon Track &mdash; Level Horizons &amp; Jump Expansion</span>
                    <span v-if="st.farthest !== undefined && st.farthest >= currentNums.length - 1" class="ll-calc-pill">&#10003; Destination In Reach</span>
                  </div>

                  <div class="ll-jump-track-scroll">
                    <div class="ll-track-board" :style="{ width: Math.max(340, currentNums.length * 68) + 'px' }">
                      <!-- 1. Arc Layer (Curved path showing leap from i to i + nums[i]) -->
                      <div class="ll-arc-svg-wrap">
                        <svg class="ll-arc-svg" :style="{ width: Math.max(340, currentNums.length * 68) + 'px', height: '42px' }">
                          <path
                            v-if="st.arcFrom !== undefined && st.arcTo !== undefined && st.arcTo > st.arcFrom"
                            :d="`M ${26 + st.arcFrom * 68} 38 Q ${26 + ((st.arcFrom + Math.min(currentNums.length - 1, st.arcTo)) / 2) * 68} ${Math.max(6, 38 - (st.arcTo - st.arcFrom) * 9)} ${26 + Math.min(currentNums.length - 1, st.arcTo) * 68} 38`"
                            fill="none"
                            stroke="#f59e0b"
                            stroke-width="2.5"
                            stroke-dasharray="4 2"
                            class="ll-arc-jump"
                          />
                          <text
                            v-if="st.arcFrom !== undefined && st.arcTo !== undefined && st.arcTo > st.arcFrom"
                            :x="26 + ((st.arcFrom + Math.min(currentNums.length - 1, st.arcTo)) / 2) * 68"
                            :y="Math.max(12, 30 - (st.arcTo - st.arcFrom) * 7)"
                            text-anchor="middle"
                            class="ll-arc-txt"
                          >
                            Leap +{{ currentNums[st.arcFrom] }} (to [{{ st.arcTo }}])
                          </text>
                        </svg>
                      </div>

                      <!-- 2. Stepping Stones Track Row -->
                      <div class="ll-stones-track" :style="{ width: Math.max(340, currentNums.length * 68) + 'px' }">
                        <div
                          v-for="(val, idx) in currentNums"
                          :key="'st-' + idx"
                          class="ll-stone-pod"
                          :class="{
                            'll-pod-active': st.i === idx,
                            'll-pod-reach': st.farthest !== undefined && idx <= st.farthest,
                            'll-pod-window': st.currentEnd !== undefined && idx <= st.currentEnd,
                            'll-pod-goal': idx === currentNums.length - 1
                          }"
                        >
                          <!-- Top Marker (CURRENT / TARGET) -->
                          <div class="ll-pod-pointer">
                            <span v-if="st.i === idx" class="ll-pin ll-pin-active">
                              CURRENT
                            </span>
                            <span v-else-if="idx === currentNums.length - 1" class="ll-pin ll-pin-goal">
                              TARGET
                            </span>
                            <span v-else class="ll-pin ll-pin-space"></span>
                          </div>

                          <!-- Stone Card -->
                          <div class="ll-stone-card">
                            <div class="ll-stone-card-idx">[{{ idx }}]</div>
                            <div class="ll-stone-card-val">{{ val }}</div>
                            <div class="ll-stone-card-sub">cap: {{ val }}</div>
                          </div>

                          <!-- Bottom Level Boundary & Farthest Pins -->
                          <div class="ll-pod-barrier">
                            <div
                              v-if="st.currentEnd !== undefined && idx === Math.min(currentNums.length - 1, st.currentEnd)"
                              class="ll-barrier-tag"
                            >
                              <div class="ll-barrier-arrow">&#9650;</div>
                              <div class="ll-barrier-lbl">levelEnd={{ st.currentEnd }}</div>
                            </div>
                            <div
                              v-else-if="st.farthest !== undefined && idx === Math.min(currentNums.length - 1, st.farthest)"
                              class="ll-barrier-tag"
                            >
                              <div class="ll-barrier-arrow ll-arrow-blue">&#9650;</div>
                              <div class="ll-barrier-lbl ll-lbl-blue">farthest={{ st.farthest }}</div>
                            </div>
                          </div>
                        </div>
                      </div>

                      <!-- 3. Horizon Baseline Line below stones -->
                      <div class="ll-horizon-line-wrap" :style="{ width: Math.max(340, currentNums.length * 68) + 'px' }">
                        <div class="ll-horizon-track">
                          <div
                            class="ll-horizon-active"
                            :style="{
                              width: st.farthest !== undefined
                                ? Math.min(100, (((Math.min(currentNums.length - 1, st.farthest)) + 0.5) / currentNums.length) * 100) + '%'
                                : '0%'
                            }"
                          ></div>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>

                <!-- VIEW 2: BRUTE FORCE DECISION TREE -->
                <div v-else class="ll-tree-container">
                  <div class="ll-tree-scroll-area">
                    <svg
                      :viewBox="`0 0 ${st.treeWidth || 480} ${st.treeHeight || 240}`"
                      :style="{
                        width: '100%',
                        maxWidth: (st.treeWidth || 480) + 'px',
                        height: (st.treeHeight || 240) + 'px',
                        maxHeight: (st.treeHeight || 240) + 'px'
                      }"
                      class="ll-tree-svg"
                    >
                      <!-- Dynamic Edges -->
                      <g class="ll-tree-edges">
                        <line
                          v-for="(edge, idx) in st.treeEdges"
                          :key="idx"
                          :x1="edge.x1"
                          :y1="edge.y1"
                          :x2="edge.x2"
                          :y2="edge.y2"
                          class="ll-tree-edge"
                          :class="{ 'll-edge-pick': edge.isOptimal }"
                        />
                        <!-- Edge step labels -->
                        <text
                          v-for="(edge, idx) in st.treeEdges"
                          :key="'elbl-' + idx"
                          :x="(edge.x1 + edge.x2) / 2 + 5"
                          :y="(edge.y1 + edge.y2) / 2"
                          class="ll-node-text-val"
                        >
                          +{{ edge.step }}
                        </text>
                      </g>

                      <!-- Tree Nodes -->
                      <g class="ll-tree-nodes">
                        <g
                          v-for="node in st.treeNodes"
                          :key="node.id"
                          class="ll-tree-node-group"
                          :transform="`translate(${node.x}, ${node.y})`"
                        >
                          <rect
                            :x="-19"
                            :y="-11"
                            :width="38"
                            :height="22"
                            rx="4"
                            class="ll-node-rect"
                            :class="{
                              'll-node-active': node.id === st.activeNodeId,
                              'll-node-solved': node.state === 'solved',
                              'll-node-leaf': node.isGoal
                            }"
                          />
                          <text
                            x="0"
                            y="-3"
                            text-anchor="middle"
                            class="ll-node-text-call"
                          >
                            idx={{ node.idx }}
                          </text>
                          <text
                            x="0"
                            y="6.5"
                            text-anchor="middle"
                            class="ll-node-text-val"
                            :class="{
                              'll-val-active': node.id === st.activeNodeId,
                              'll-val-solved': node.state === 'solved'
                            }"
                          >
                            {{ node.isGoal ? 'Goal (0)' : (node.computedMin !== null ? (node.computedMin === 1000000 ? 'Dead' : `min=${node.computedMin}`) : `cap=${currentNums[node.idx] || 0}`) }}
                          </text>
                        </g>
                      </g>
                    </svg>
                  </div>
                </div>
              </div>

              <!-- Vertical Resizer for Viz Panel -->
              <div class="ll-vresizer" ref="vizResizerRef"></div>

              <!-- Dedicated Legend Strip (Exact match with ActivitySelection / JumpGame design) -->
              <div class="ll-legend">
                <template v-if="currentApproach === 'greedy'">
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-cur"></span>Active Stone i</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-purple"></span>Current Level Horizon</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-new"></span>Farthest Reach</span>
                  <span class="ll-leg"><span class="ll-legdot" style="background: #a855f7;"></span>Goal Stone (n - 1)</span>
                </template>
                <template v-else>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-cur"></span>Active Call</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-new"></span>Feasible Path</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-purple"></span>Goal (0 jumps)</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-red"></span>Dead End</span>
                </template>
              </div>

              <!-- Call Stack & Variable Environment Panel -->
              <div class="ll-table-area" :style="{ height: tableHeight + 'px' }">
                <div class="ll-table-title">Call Stack &amp; Environment Variables:</div>
                <div class="ll-stack-line" v-for="(fr, fIdx) in s.vars" :key="fIdx">
                  <span class="ll-fname">{{ fr.title }}</span>
                  <span class="ll-now">&rarr;</span>
                  <span v-for="(v, vIdx) in fr.rows" :key="vIdx" class="ll-frame">
                    {{ v[0] }} = <b>{{ v[1] }}</b><span v-if="vIdx < fr.rows.length - 1">, </span>
                  </span>
                </div>
              </div>

              <!-- Vertical Resizer for Call Stack Panel -->
              <div class="ll-vresizer" ref="tableResizerRef"></div>

              <!-- Current Execution Step Badge Description -->
              <div class="ll-badge-wrap">
                <div
                  class="ll-badge"
                  :class="{
                    'll-badge-error': s.badge && (s.badge.includes('dead') || s.badge.includes('Dead')),
                    'll-badge-success': s.badge && (s.badge.includes('finished') || s.badge.includes('Minimum') || s.badge.includes('complete'))
                  }"
                >
                  {{ s.badge || 'Ready to visualize Jump Game II.' }}
                </div>
              </div>
            </div>

            <!-- Horizontal Resizer between Left and Right Columns -->
            <div class="ll-resizer" ref="hResizerRef"></div>

            <!-- Right Code & Explanation Column -->
            <div class="ll-right-col">
              <div class="ll-code-panel">
                <!-- Header Toolbar: Tabs + Language Selector -->
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
                  </select>
                </div>

                <!-- Code Tab -->
                <div v-if="rightTab === 'code'" class="ll-code-scroll">
                  <pre class="ll-pre"><code class="ll-code-block"><span
                    v-for="(line, lIdx) in codeLines"
                    :key="lIdx"
                    class="ll-codeline"
                    :class="{ 'll-hl': line[0] && line[0] === s.code }"
                  >{{ line[1] }}</span></code></pre>
                </div>

                <!-- Pseudocode Tab -->
                <div v-else-if="rightTab === 'pseudo'" class="ll-code-scroll">
                  <pre class="ll-pre"><code class="ll-code-block"><span
                    v-for="(line, pIdx) in pseudocodeLines"
                    :key="pIdx"
                    class="ll-codeline"
                  >{{ line }}</span></code></pre>
                </div>

                <!-- Time and Space Complexity Tab -->
                <div v-else class="ll-info-scroll">

                  <!-- ════════ BRUTE FORCE COMPLEXITY ════════ -->
                  <template v-if="currentApproach === 'brute'">
                    <h3 class="ll-cx-heading">Brute Force &mdash; Complexity Analysis</h3>
                    <p class="ll-cx-intro">
                      Exhaustively explores every possible jump sequence using recursion.
                      At each stone <code>idx</code>, it tries <strong>every step</strong> from 1 to <code>nums[idx]</code>,
                      branching into a potentially exponential tree and returning the minimum cost path.
                    </p>

                    <h4 class="ll-cx-sub">Step-by-step Breakdown</h4>
                    <table class="ll-complexity-table">
                      <thead>
                        <tr><th>Operation</th><th>Time</th><th>Space</th><th>Reason</th></tr>
                      </thead>
                      <tbody>
                        <tr>
                          <td>Read input nums[]</td>
                          <td class="ll-cx-good">O(n)</td>
                          <td class="ll-cx-good">O(n)</td>
                          <td>Single pass to fill the array</td>
                        </tr>
                        <tr>
                          <td>Recursive branching (try each step)</td>
                          <td class="ll-cx-bad">O(2<sup>n</sup>)</td>
                          <td class="ll-cx-mid">O(n)</td>
                          <td>Up to nums[i] branches per node; each path explores the full depth n; stack depth = n</td>
                        </tr>
                        <tr>
                          <td>Base case &amp; min-cost update</td>
                          <td class="ll-cx-good">O(1)</td>
                          <td class="ll-cx-good">O(1)</td>
                          <td>Simple comparison <code>idx &ge; n - 1</code> and scalar minimum update</td>
                        </tr>
                      </tbody>
                    </table>

                    <h4 class="ll-cx-sub">Overall Complexity</h4>
                    <div class="ll-cx-summary-grid">
                      <div class="ll-cx-card ll-cx-card-bad">
                        <div class="ll-cx-card-label">Time</div>
                        <div class="ll-cx-card-val">O(2<sup>n</sup>)</div>
                        <div class="ll-cx-card-note">Exponential &mdash; infeasible for large n</div>
                      </div>
                      <div class="ll-cx-card ll-cx-card-mid">
                        <div class="ll-cx-card-label">Space</div>
                        <div class="ll-cx-card-val">O(n)</div>
                        <div class="ll-cx-card-note">Recursion call stack depth</div>
                      </div>
                      <div class="ll-cx-card ll-cx-card-good">
                        <div class="ll-cx-card-label">Optimal?</div>
                        <div class="ll-cx-card-val">YES</div>
                        <div class="ll-cx-card-note">Exhaustive &rarr; guaranteed correct</div>
                      </div>
                    </div>

                    <div class="ll-note">
                      <strong>When is Brute Force used?</strong> Only for very small inputs (n &le; 20).
                      For real minimum-jump problems with large arrays, the Greedy BFS window approach is the only practical choice.
                    </div>
                  </template>

                  <!-- ════════ GREEDY COMPLEXITY ════════ -->
                  <template v-else>
                    <h3 class="ll-cx-heading">Greedy Algorithm &mdash; Complexity Analysis</h3>
                    <p class="ll-cx-intro">
                      Simulates a BFS level expansion using two scalars: <code>currentEnd</code> (boundary
                      of the current jump level) and <code>farthest</code> (maximum index reachable from
                      any stone in the current level). One left-to-right pass counts how many level
                      transitions are needed to cover the destination.
                    </p>

                    <h4 class="ll-cx-sub">Step-by-step Breakdown</h4>
                    <table class="ll-complexity-table">
                      <thead>
                        <tr><th>Operation</th><th>Time</th><th>Space</th><th>Reason</th></tr>
                      </thead>
                      <tbody>
                        <tr>
                          <td>Read input nums[]</td>
                          <td class="ll-cx-good">O(n)</td>
                          <td class="ll-cx-good">O(n)</td>
                          <td>Single pass to fill the array</td>
                        </tr>
                        <tr>
                          <td>Initialise <code>jumps</code>, <code>currentEnd</code>, <code>farthest</code></td>
                          <td class="ll-cx-good">O(1)</td>
                          <td class="ll-cx-good">O(1)</td>
                          <td>Three scalar integer variables</td>
                        </tr>
                        <tr>
                          <td>Greedy BFS forward scan</td>
                          <td class="ll-cx-good">O(n)</td>
                          <td class="ll-cx-good">O(1)</td>
                          <td>Single left-to-right pass; farthest update and window check are O(1) per step</td>
                        </tr>
                      </tbody>
                    </table>

                    <h4 class="ll-cx-sub">Overall Complexity</h4>
                    <div class="ll-cx-summary-grid">
                      <div class="ll-cx-card ll-cx-card-good">
                        <div class="ll-cx-card-label">Time</div>
                        <div class="ll-cx-card-val">O(n)</div>
                        <div class="ll-cx-card-note">Single BFS forward scan &mdash; optimal</div>
                      </div>
                      <div class="ll-cx-card ll-cx-card-good">
                        <div class="ll-cx-card-label">Space</div>
                        <div class="ll-cx-card-val">O(1)</div>
                        <div class="ll-cx-card-note">Only three scalar variables</div>
                      </div>
                      <div class="ll-cx-card ll-cx-card-good">
                        <div class="ll-cx-card-label">Optimal?</div>
                        <div class="ll-cx-card-val">YES</div>
                        <div class="ll-cx-card-note">Provable via BFS level optimality</div>
                      </div>
                    </div>

                    <h4 class="ll-cx-sub">Approach Comparison</h4>
                    <table class="ll-complexity-table">
                      <thead>
                        <tr><th>Approach</th><th>Time</th><th>Space</th><th>Practical?</th></tr>
                      </thead>
                      <tbody>
                        <tr>
                          <td><strong>Brute Force</strong></td>
                          <td class="ll-cx-bad">O(2<sup>n</sup>)</td>
                          <td class="ll-cx-mid">O(n)</td>
                          <td class="ll-cx-bad">No &mdash; exponential blowup</td>
                        </tr>
                        <tr>
                          <td><strong>Dynamic Programming</strong></td>
                          <td class="ll-cx-mid">O(n<sup>2</sup>)</td>
                          <td class="ll-cx-good">O(n)</td>
                          <td class="ll-cx-mid">Partial &mdash; quadratic for large n</td>
                        </tr>
                        <tr>
                          <td><strong>Greedy (BFS Window)</strong></td>
                          <td class="ll-cx-good">O(n)</td>
                          <td class="ll-cx-good">O(1)</td>
                          <td class="ll-cx-good">Yes &mdash; scales to millions</td>
                        </tr>
                      </tbody>
                    </table>

                    <div class="ll-note">
                      <strong>Key Insight:</strong> By grouping indices into BFS level windows
                      <code>[0, currentEnd]</code>, Jump Game II reduces the minimum jump count
                      to the number of times the window boundary expands before covering index
                      <code>n - 1</code> &mdash; achieved in O(1) space with a single forward pass.
                    </div>
                  </template>

                </div>
              </div>
            </div>
          </div>

          <!-- Bottom Footer Toolbar -->
          <div class="ll-footer">
            Step {{ si + 1 }} / {{ steps.length }}
            <span class="ll-speed-wrap">
              Speed
              <input type="range" min="100" max="2000" step="100" v-model.number="speed" />
            </span>
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
  --coral: #F04D4D;
  --coral-dark: #d93e3e;
  --coral-light: #fff0f0;
  --bg: #f5f6fa;
  --surface: #ffffff;
  --surface2: #f1f4f9;
  --border: #e2e8f0;
  --border2: #cbd5e1;
  --text: #1e293b;
  --text2: #475569;
  --muted: #94a3b8;
  --blue: #3b82f6;
  --blue-light: #eff6ff;
  --green: #22c55e;
  --green-light: #f0fdf4;
  --orange: #f97316;
  --orange-light: #fff7ed;
  --purple: #9333ea;
  --purple-light: #f3e8ff;
  --red: #ef4444;
  --red-dark: #991b1b;
  --red-light: #fef2f2;
  --shadow-sm: 0 1px 3px rgba(0,0,0,0.08), 0 1px 2px rgba(0,0,0,0.04);
  --radius: 8px;
  --radius-sm: 6px;

  display: flex;
  flex-direction: column;
  width: 100%;
  height: 75vh;
  background: var(--bg);
  color: var(--text);
  font-family: 'Segoe UI', system-ui, sans-serif;
  font-size: 12.5px;
  overflow: hidden;
}

@keyframes ll-pop {
  0% { transform: scale(0.85); opacity: 0; }
  100% { transform: scale(1); opacity: 1; }
}
@keyframes ll-pulse {
  0%, 100% { transform: scale(1); }
  50% { transform: scale(1.05); }
}

.slide-wrapper {
  width: 107%;
  max-height: 100%;
  margin-top: -10px;
  margin-left: -30px;
  font-size: 0.8rem;
  font-weight: 400;
}

.slide-body {
  display: flex;
  flex-direction: column;
  height: 100%;
  border-radius: 4px;
}

.navbar {
  display: flex;
  position: fixed;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  width: 94.7%;
  padding: 0 10px;
  gap: 0.75rem;
  background-color: #ffffff;
}
.navbar > img { height: 30px; }
.navbar-title {
  color: white;
  margin: 0 0 0 -10px;
  padding: 2px 10px;
  background-color: #ef5050;
  border-radius: 5px;
  width: 80%;
  font-weight: 700;
  font-size: 1.35rem;
}

.row-main {
  width: 100%;
  height: 90%;
  margin-top: 36px;
  overflow: auto;
}

/* Control Toolbar */
.ll-toolbar {
  display: flex;
  align-items: center;
  gap: 6px;
  padding: 5px 12px;
  background: var(--surface);
  border-bottom: 1px solid var(--border);
  box-shadow: var(--shadow-sm);
  flex-shrink: 0;
  flex-wrap: wrap;
  margin-top: 4px;
}

.ll-approach-group {
  display: flex;
  background: var(--surface2);
  border: 1px solid var(--border);
  border-radius: var(--radius-sm);
  padding: 2px;
  gap: 2px;
}

.ll-approach-btn {
  padding: 4px 8px;
  border: none;
  background: transparent;
  color: var(--text2);
  font-size: 11px;
  font-weight: 600;
  border-radius: 4px;
  cursor: pointer;
  transition: all 0.15s;
  white-space: nowrap;
}
.ll-approach-btn:hover { color: var(--coral); }
.ll-approach-btn.active {
  background: var(--coral);
  color: #fff;
  box-shadow: var(--shadow-sm);
}

.ll-input-group {
  display: flex;
  align-items: center;
  gap: 4px;
}
.ll-input-group label {
  font-size: 11px;
  font-weight: 700;
  color: var(--muted);
}

.ll-text-input {
  padding: 3px 6px;
  border: 1px solid var(--border2);
  border-radius: var(--radius-sm);
  font-family: monospace;
  font-size: 11px;
  color: var(--text);
  background: var(--surface);
}
.ll-text-input:focus {
  outline: none;
  border-color: var(--coral);
  box-shadow: 0 0 0 3px rgba(240,77,77,0.1);
}

.ll-viz-btn {
  padding: 5px 12px;
  border: none;
  border-radius: var(--radius-sm);
  background: var(--coral);
  color: #fff;
  font-size: 11.5px;
  font-weight: 600;
  cursor: pointer;
  transition: filter 0.15s;
  box-shadow: var(--shadow-sm);
}
.ll-viz-btn:hover { filter: brightness(1.08); }

.ll-nav-controls {
  display: flex;
  align-items: center;
  gap: 4px;
  margin-left: auto;
  flex-shrink: 0;
  flex-wrap: wrap;
}

.ll-nav-btn {
  padding: 4px 9px;
  border: 1px solid var(--border2);
  border-radius: var(--radius-sm);
  background: var(--surface2);
  color: var(--text2);
  font-size: 11px;
  font-weight: 500;
  cursor: pointer;
  transition: all 0.15s;
  white-space: nowrap;
}
.ll-nav-btn:hover {
  border-color: var(--coral);
  color: var(--coral);
  background: var(--surface);
}

.ll-play-btn {
  padding: 4px 9px;
  border: 1px solid var(--blue);
  border-radius: var(--radius-sm);
  background: var(--blue-light);
  color: var(--blue);
  font-size: 11px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.15s;
  min-width: 68px;
}
.ll-play-btn:hover {
  background: var(--blue);
  color: #fff;
}

/* Main Split View */
.ll-main {
  display: flex;
  flex: 1;
  overflow: hidden;
  position: relative;
}

.ll-left-col {
  display: flex;
  flex-direction: column;
  overflow: hidden;
  min-width: 240px;
  max-width: 75%;
}

.ll-right-col {
  display: flex;
  flex-direction: column;
  flex: 1;
  overflow: hidden;
  min-width: 0;
}

/* Stepping Stone Array Banner */
.ll-house-banner {
  padding: 4px 10px;
  background: var(--surface2);
  border-bottom: 1px solid var(--border);
  flex-shrink: 0;
}
.ll-house-title {
  font-size: 10.5px;
  font-weight: 700;
  color: var(--text2);
  margin-bottom: 3px;
}
.ll-dual-arrays-wrap {
  display: flex;
  flex-direction: column;
  gap: 3px;
}
.ll-arr-row {
  display: flex;
  align-items: center;
  gap: 6px;
}
.ll-arr-tag {
  font-family: monospace;
  font-size: 10.5px;
  font-weight: 700;
  color: var(--coral);
  width: 48px;
  flex-shrink: 0;
}
.ll-arr-cells {
  display: flex;
  gap: 6px;
  overflow-x: auto;
  padding: 1px 0;
}
.ll-arr-cell {
  width: 38px;
  height: 34px;
  background: var(--surface);
  border: 1.5px solid var(--border2);
  border-radius: var(--radius-sm);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  box-shadow: var(--shadow-sm);
  transition: all 0.2s ease;
  flex-shrink: 0;
}
.ll-cell-idx {
  font-size: 8px;
  color: var(--muted);
  font-family: monospace;
}
.ll-cell-val {
  font-size: 12.5px;
  font-weight: 800;
  font-family: monospace;
  color: var(--text);
}
.ll-cell-active {
  border-color: #f59e0b !important;
  background: #fffbeb !important;
  box-shadow: 0 0 0 2.5px rgba(245,158,11,0.25) !important;
  transform: translateY(-1px);
}
.ll-cell-reach {
  border-color: #10b981 !important;
}
.ll-cell-goal {
  border-color: #9333ea !important;
}

/* Stats Chips Bar */
.ll-ptrs {
  display: flex;
  align-items: center;
  gap: 5px;
  padding: 4px 10px;
  background: var(--surface2);
  border-bottom: 1px solid var(--border);
  min-height: 26px;
  flex-shrink: 0;
  flex-wrap: wrap;
  box-sizing: border-box;
}
.ll-ptr-chip {
  font-size: 10.5px;
  font-family: monospace;
  background: var(--surface);
  border: 1px solid var(--border);
  padding: 2px 7px;
  border-radius: var(--radius-sm);
  box-shadow: var(--shadow-sm);
  white-space: nowrap;
}
.ll-c-blue { color: var(--blue); }
.ll-c-orange { color: var(--orange); }
.ll-c-green { color: var(--green); }
.ll-c-purple { color: var(--purple); }
.ll-c-red { color: var(--red); }

/* Visualization Viewport */
.ll-viz-wrap {
  position: relative;
  background: var(--surface);
  border-bottom: 1px solid var(--border);
  overflow: hidden;
  flex-shrink: 0;
  display: flex;
  flex-direction: column;
}

.ll-timeline-container {
  display: flex;
  flex-direction: column;
  width: 100%;
  height: 100%;
  overflow: hidden;
}

.ll-section-caption {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 3px 10px;
  background: var(--surface2);
  border-top: 1px solid var(--border);
  border-bottom: 1px solid var(--border);
  font-size: 10.5px;
  font-weight: 700;
  color: var(--text2);
}

.ll-calc-pill {
  font-size: 10px;
  font-weight: 700;
  font-family: monospace;
  padding: 1px 6px;
  border-radius: 10px;
  background: #dcfce7;
  color: #15803d;
}

.ll-jump-track-scroll {
  flex: 1;
  width: 100%;
  overflow-x: auto;
  overflow-y: auto;
  padding: 6px 12px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.ll-track-board {
  display: flex;
  flex-direction: column !important;
  align-items: center;
  justify-content: flex-start;
  margin: 0 auto;
  padding: 4px 0 8px;
}

/* Modern Greedy Jump Track Styles */
.ll-arc-svg-wrap {
  height: 42px;
  display: flex;
  justify-content: center;
  align-items: flex-end;
  width: 100%;
}

.ll-arc-svg {
  display: block;
  overflow: visible;
}

.ll-arc-txt {
  font-family: Consolas, 'Fira Code', monospace !important;
  font-size: 9.5px !important;
  font-weight: 700 !important;
  fill: #b45309 !important;
  user-select: none !important;
}

.ll-stones-track {
  display: flex;
  flex-direction: row;
  justify-content: flex-start;
  align-items: flex-start;
  gap: 16px;
  padding: 0;
}

.ll-stone-pod {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 52px;
  flex-shrink: 0;
}

.ll-pod-pointer {
  height: 18px;
  display: flex;
  align-items: flex-end;
  justify-content: center;
  margin-bottom: 3px;
  width: 100%;
}

.ll-pin {
  font-family: Consolas, 'Fira Code', monospace;
  font-size: 8px;
  font-weight: 800;
  padding: 1px 4px;
  border-radius: 3px;
  letter-spacing: 0.3px;
  line-height: 1.1;
  text-align: center;
  white-space: nowrap;
}

.ll-pin-active {
  color: #b45309;
  background: #fef3c7;
  border: 1px solid #f59e0b;
}

.ll-pin-goal {
  color: #6b21a8;
  background: #f3e8ff;
  border: 1px solid #9333ea;
}

.ll-pin-space {
  visibility: hidden;
}

.ll-stone-card {
  width: 52px;
  height: 48px;
  background: var(--surface);
  border: 1.5px solid var(--border2);
  border-radius: 6px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  box-shadow: var(--shadow-sm);
  transition: all 0.2s ease;
  position: relative;
}

.ll-stone-card-idx {
  font-family: Consolas, monospace;
  font-size: 8.5px;
  color: var(--muted);
  font-weight: 600;
  line-height: 1;
  margin-bottom: 1px;
}

.ll-stone-card-val {
  font-family: Consolas, monospace;
  font-size: 15px;
  font-weight: 800;
  color: var(--text);
  line-height: 1.1;
}

.ll-stone-card-sub {
  font-family: Consolas, monospace;
  font-size: 7.5px;
  color: var(--muted);
  font-weight: 600;
  line-height: 1;
  margin-top: 1px;
}

/* Pod states */
.ll-pod-active .ll-stone-card {
  border-color: #f59e0b !important;
  background: #fffbeb !important;
  box-shadow: 0 0 0 3px rgba(245, 158, 11, 0.25) !important;
  transform: translateY(-2px);
}
.ll-pod-active .ll-stone-card-val {
  color: #b45309 !important;
}

.ll-pod-window .ll-stone-card {
  border-color: #9333ea !important;
}

.ll-pod-reach .ll-stone-card {
  border-color: #10b981 !important;
  background: #f0fdf4 !important;
}
.ll-pod-reach .ll-stone-card-val {
  color: #065f46 !important;
}

.ll-pod-goal .ll-stone-card {
  border-color: #9333ea !important;
  background: #faf5ff !important;
}
.ll-pod-goal .ll-stone-card-val {
  color: #6b21a8 !important;
}

/* Bottom barrier */
.ll-pod-barrier {
  height: 22px;
  display: flex;
  justify-content: center;
  align-items: flex-start;
  margin-top: 3px;
  width: 100%;
}

.ll-barrier-tag {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.ll-barrier-arrow {
  color: #9333ea;
  font-size: 7.5px;
  line-height: 1;
}
.ll-arrow-blue {
  color: #2563eb !important;
}

.ll-barrier-lbl {
  font-family: Consolas, monospace;
  font-size: 8px;
  font-weight: 700;
  color: #ffffff;
  background: #9333ea;
  border-radius: 3px;
  padding: 1px 4px;
  white-space: nowrap;
}
.ll-lbl-blue {
  background: #2563eb !important;
}

/* Horizon progress bar */
.ll-horizon-line-wrap {
  width: 100%;
  display: flex;
  justify-content: center;
  padding: 4px 0 0;
}

.ll-horizon-track {
  width: 100%;
  height: 4px;
  background: #e2e8f0;
  border-radius: 2px;
  overflow: hidden;
}

.ll-horizon-active {
  height: 100%;
  background: #10b981;
  transition: width 0.25s ease;
}

.ll-arc-jump {
  animation: ll-pop 0.3s ease;
}

/* Standard Legend Strip */
.ll-legend {
  display: flex;
  flex-wrap: wrap;
  gap: 4px 10px;
  padding: 4px 10px;
  border-bottom: 1px solid var(--border);
  flex-shrink: 0;
  background: var(--surface2);
}

.ll-leg {
  display: flex;
  align-items: center;
  gap: 4px;
  font-size: 10px;
  color: var(--text2);
  font-weight: 500;
}

.ll-legdot {
  width: 8px;
  height: 8px;
  border-radius: 2px;
  display: inline-block;
}

.ll-legdot-cur { background: #fef3c7; border: 1.5px solid #f59e0b; }
.ll-legdot-new { background: #dcfce7; border: 1.5px solid #10b981; }
.ll-legdot-red { background: #fee2e2; border: 1.5px solid #ef4444; }
.ll-legdot-purple { background: #f3e8ff; border: 1.5px solid #9333ea; }

/* Tree view for Brute Force */
.ll-tree-container {
  display: flex;
  flex-direction: column;
  width: 100%;
  height: 100%;
}

.ll-tree-scroll-area {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-start;
  flex: 1;
  padding: 4px 6px;
  overflow: auto;
  box-sizing: border-box;
}

.ll-tree-svg {
  display: block;
  flex-shrink: 0;
  max-width: 100%;
  margin: 0 auto;
}

.ll-tree-edge {
  stroke: #cbd5e1;
  stroke-width: 1.5px;
}

.ll-edge-pick {
  stroke: #22c55e !important;
  stroke-width: 2px !important;
}

.ll-tree-node-group { cursor: default; }

.ll-node-rect {
  fill: #ffffff;
  stroke: #cbd5e1;
  stroke-width: 1.4px;
  filter: drop-shadow(0 1px 2px rgba(0,0,0,.05));
  transition: all .25s ease;
}

.ll-node-active {
  stroke: #f59e0b !important;
  stroke-width: 2.2px !important;
  fill: #fffbeb !important;
  filter: drop-shadow(0 0 5px rgba(245,158,11,.45)) !important;
}

.ll-node-solved {
  stroke: #10b981 !important;
  fill: #dcfce7 !important;
  stroke-width: 1.6px !important;
}

.ll-node-leaf {
  stroke: #9333ea !important;
  fill: #f3e8ff !important;
  stroke-width: 1.6px !important;
}

.ll-tree-svg text {
  font-family: 'Segoe UI', system-ui, sans-serif !important;
  user-select: none;
}

.ll-node-text-call {
  font-family: 'Consolas','Fira Code',monospace !important;
  font-size: 8.5px !important;
  font-weight: 700 !important;
  fill: #1e293b !important;
  dominant-baseline: central !important;
}

.ll-node-text-val {
  font-family: 'Consolas','Fira Code',monospace !important;
  font-size: 7.5px !important;
  font-weight: 700 !important;
  fill: #64748b !important;
  dominant-baseline: central !important;
}

.ll-val-active { fill: #b45309 !important; }
.ll-val-solved { fill: #047857 !important; }

/* Resizers */
.ll-resizer {
  width: 5px;
  cursor: col-resize;
  background: var(--border);
  transition: background .15s;
  flex-shrink: 0;
  position: relative;
  z-index: 20;
}
.ll-resizer:hover, .ll-resizer.drag { background: var(--coral); }

.ll-vresizer {
  height: 5px;
  cursor: row-resize;
  background: var(--border);
  transition: background .15s;
  flex-shrink: 0;
  position: relative;
  z-index: 20;
}
.ll-vresizer:hover, .ll-vresizer.drag { background: var(--coral); }

/* Call Stack & Environment Panel */
.ll-table-area {
  padding: 4px 10px;
  background: var(--surface);
  border-bottom: 1px solid var(--border);
  flex-shrink: 0;
  overflow: auto;
  box-sizing: border-box;
}

.ll-table-title {
  font-size: 9.5px;
  color: var(--muted);
  margin-bottom: 2px;
  font-style: italic;
}

.ll-stack-line {
  font-family: Consolas, monospace;
  font-size: 11px;
  line-height: 1.6;
}

.ll-frame {
  color: var(--text2);
  font-size: 10.5px;
  padding: 1px 0;
  white-space: nowrap;
}

.ll-fname { color: var(--text2); }
.ll-now { color: var(--orange); margin-left: 4px; font-size: 9.5px; }

/* Step Badge Wrap */
.ll-badge-wrap {
  display: flex;
  align-items: center;
  padding: 4px 8px;
  background: var(--surface);
  border-bottom: 1px solid var(--border);
  min-height: 30px;
  flex-shrink: 0;
}

.ll-badge {
  display: inline-block;
  padding: 3px 10px;
  font-size: 10.5px;
  line-height: 1.35;
  font-weight: 500;
  color: var(--coral-dark);
  background: var(--coral-light);
  border-left: 3px solid var(--coral);
  border-radius: var(--radius-sm);
  word-break: break-word;
}
.ll-badge-error {
  border-left-color: var(--red);
  background: var(--red-light);
  color: var(--red-dark);
  font-weight: 600;
}
.ll-badge-success {
  border-left-color: var(--green);
  background: var(--green-light);
  color: #166534;
  font-weight: 600;
}

/* Right Panel Styles */
.ll-code-panel {
  display: flex;
  flex-direction: column;
  height: 100%;
  overflow: hidden;
}

.ll-code-header {
  display: flex;
  align-items: center;
  gap: 6px;
  padding: 5px 12px;
  background: var(--surface);
  border-bottom: 1px solid var(--border);
  box-shadow: var(--shadow-sm);
  flex-shrink: 0;
  flex-wrap: wrap;
}

.ll-tabbar {
  display: flex;
  gap: 3px;
  flex-wrap: wrap;
}

.ll-tab-btn {
  padding: 4px 9px;
  border: 1px solid var(--border2);
  border-radius: var(--radius-sm);
  background: var(--surface2);
  color: var(--text2);
  font-size: 10.5px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.15s;
  white-space: nowrap;
}
.ll-tab-btn:hover {
  border-color: var(--coral);
  color: var(--coral);
}
.ll-tab-btn.active {
  background: var(--coral);
  border-color: var(--coral);
  color: #fff;
}

.ll-lang-select {
  margin-left: auto;
  padding: 4px 24px 4px 8px;
  font-size: 11px;
  font-weight: 500;
  border: 1px solid var(--border2);
  border-radius: var(--radius-sm);
  background: var(--surface2);
  color: var(--text);
  cursor: pointer;
  appearance: none;
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='10' height='6' viewBox='0 0 10 6'%3E%3Cpath d='M0 0l5 6 5-6z' fill='%2394a3b8'/%3E%3C/svg%3E");
  background-repeat: no-repeat;
  background-position: right 8px center;
  transition: border-color 0.15s;
  min-width: 95px;
}
.ll-lang-select:focus {
  outline: none;
  border-color: var(--coral);
  box-shadow: 0 0 0 3px rgba(240,77,77,0.1);
}

.ll-code-scroll {
  flex: 1;
  overflow: auto;
  padding: 10px 14px;
  background: #f8fafc;
  min-width: 0;
}

.ll-pre {
  margin: 0;
  font-family: 'Cascadia Code', 'Fira Code', 'Consolas', monospace;
  font-size: 11px;
  line-height: 1.5;
  color: var(--text);
  white-space: pre;
  padding-bottom: 150px;
}

.ll-codeline {
  display: block;
  padding: 0 14px;
  margin: 0 -14px;
}
.ll-hl {
  background: #dcfce7;
  border-left: 3px solid var(--green);
  color: #15803d;
  font-weight: 600;
  border-radius: 3px;
}

/* Time & Space Complexity Tab */
.ll-info-scroll {
  flex: 1;
  overflow: auto;
  padding: 12px 16px;
  font-size: 12px;
  line-height: 1.55;
  color: var(--text2);
  background: var(--surface);
}

.ll-info-scroll h3 {
  font-size: 13px;
  font-weight: 700;
  color: var(--text);
  margin: 0 0 6px;
}
.ll-info-scroll h3:not(:first-child) { margin-top: 14px; }

.ll-info-scroll h4 {
  font-size: 12px;
  font-weight: 700;
  color: var(--text);
  margin: 10px 0 4px;
}

.ll-info-scroll p { margin: 0 0 6px; }
.ll-info-scroll ul { margin: 0 0 10px; padding-left: 18px; }
.ll-info-scroll li { margin-bottom: 4px; }

.ll-complexity-table {
  width: 100%;
  border-collapse: collapse;
  margin: 8px 0 12px;
  font-size: 11px;
}
.ll-complexity-table th, .ll-complexity-table td {
  border: 1px solid var(--border);
  padding: 5px 8px;
  text-align: left;
}
.ll-complexity-table th {
  background: var(--surface2);
  font-weight: 700;
  color: var(--text);
}

.ll-math-box {
  background: var(--surface2);
  border-left: 3px solid var(--blue);
  border-radius: var(--radius-sm);
  padding: 6px 12px;
  font-family: monospace;
  font-size: 11.5px;
  color: var(--text);
  margin: 6px 0;
}

/* Complexity Tab Styles */
.ll-cx-heading {
  font-size: 13px;
  font-weight: 700;
  color: var(--text);
  margin: 0 0 6px;
}

.ll-cx-intro {
  font-size: 10.5px;
  color: var(--text2);
  margin: 0 0 10px;
  line-height: 1.55;
}

.ll-cx-sub {
  font-size: 11px;
  font-weight: 700;
  color: var(--text2);
  margin: 10px 0 4px;
  border-bottom: 1px solid var(--border);
  padding-bottom: 3px;
}

/* Colored complexity cells */
.ll-cx-good { color: #15803d; font-weight: 700; }
.ll-cx-mid  { color: #b45309; font-weight: 700; }
.ll-cx-bad  { color: #b91c1c; font-weight: 700; }

/* Summary card grid */
.ll-cx-summary-grid {
  display: flex;
  gap: 8px;
  flex-wrap: wrap;
  margin: 6px 0 10px;
}

.ll-cx-card {
  flex: 1;
  min-width: 90px;
  border-radius: var(--radius-sm);
  padding: 8px 10px;
  text-align: center;
  border: 1.5px solid var(--border);
}

.ll-cx-card-label {
  font-size: 9px;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: .05em;
  opacity: .7;
  margin-bottom: 4px;
}

.ll-cx-card-val {
  font-size: 13px;
  font-weight: 800;
  font-family: monospace;
  margin-bottom: 3px;
}

.ll-cx-card-note {
  font-size: 8.5px;
  opacity: .75;
  line-height: 1.3;
}

.ll-cx-card-good {
  background: #f0fdf4;
  border-color: #86efac;
  color: #15803d;
}

.ll-cx-card-mid {
  background: #fff7ed;
  border-color: #fed7aa;
  color: #c2410c;
}

.ll-cx-card-bad {
  background: #fef2f2;
  border-color: #fca5a5;
  color: #b91c1c;
}

.ll-note {
  background: #fefce8;
  border: 1px solid #fef08a;
  border-left: 3px solid #eab308;
  padding: 6px 10px;
  font-size: 10.5px;
  color: #854d0e;
  border-radius: 0 4px 4px 0;
  margin-top: 10px;
  margin-bottom: 120px;
}

/* Footer Toolbar */
.ll-footer {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 4px 12px;
  background: var(--surface);
  border-top: 1px solid var(--border);
  font-size: 11px;
  color: var(--muted);
  flex-shrink: 0;
}

.ll-speed-wrap {
  display: flex;
  align-items: center;
  gap: 6px;
}
.ll-speed-wrap input {
  width: 80px;
  accent-color: var(--coral);
}
</style>
