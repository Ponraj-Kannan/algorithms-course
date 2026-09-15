<script setup>
import { ref, reactive, computed, onMounted, onUnmounted, watch } from 'vue';

const props = defineProps({
  topic: { type: String, default: 'Greedy Algorithms' },
  subTopic: { type: String, default: 'Jump Game' }
});

/* ==================================================================== */
/* APPROACH METADATA                                                    */
/* ==================================================================== */
const APPROACHES = [
  { id: 'brute', label: 'Brute Force', desc: 'O(2^n) Exponential — Exhaustive Recursive Backtracking' },
  { id: 'greedy', label: 'Greedy', desc: 'O(n) Linear Time & O(1) Space — Optimal Max Reach Forward Scan' }
];

/* ==================================================================== */
/* CODE SNIPPETS (Java, C, C++, Python)                                 */
/* Strict curly brace formatting on all control statements              */
/* ==================================================================== */
const CODES = {
  greedy: {
    java: [
      ['',                 'import java.util.Scanner;'],
      ['',                 ''],
      ['',                 'public class Main {'],
      ['c_entry',          '    static boolean canJump(int[] nums, int n) {'],
      ['c_init_reach',     '        int maxReach = 0;'],
      ['c_for_loop',       '        for (int i = 0; i < n; i++) {'],
      ['c_check_stranded', '            if (i > maxReach) {'],
      ['c_ret_stranded',   '                return false;'],
      ['',                 '            }'],
      ['c_check_reach',    '            if (i + nums[i] > maxReach) {'],
      ['c_update_reach',   '                maxReach = i + nums[i];'],
      ['',                 '            }'],
      ['c_check_goal',     '            if (maxReach >= n - 1) {'],
      ['c_ret_goal',       '                return true;'],
      ['',                 '            }'],
      ['',                 '        }'],
      ['',                 '        return true;'],
      ['',                 '    }'],
      ['',                 ''],
      ['',                 '    public static void main(String[] args) {'],
      ['m_scanner',        '        Scanner sc = new Scanner(System.in);'],
      ['m_read_n',         '        int n = sc.nextInt();'],
      ['m_alloc_nums',     '        int[] nums = new int[n];'],
      ['m_for_input',      '        for (int i = 0; i < n; i++) {'],
      ['m_read_num',       '            nums[i] = sc.nextInt();'],
      ['',                 '        }'],
      ['m_call_jump',      '        boolean ans = canJump(nums, n);'],
      ['m_print_ans',      '        System.out.println(ans ? "true" : "false");'],
      ['m_done',           '    }'],
      ['',                 '}']
    ],
    c: [
      ['',                 '#include <stdio.h>'],
      ['',                 '#include <stdbool.h>'],
      ['',                 ''],
      ['c_entry',          'bool canJump(int nums[], int n) {'],
      ['c_init_reach',     '    int maxReach = 0;'],
      ['c_for_loop',       '    for (int i = 0; i < n; i++) {'],
      ['c_check_stranded', '        if (i > maxReach) {'],
      ['c_ret_stranded',   '            return false;'],
      ['',                 '        }'],
      ['c_check_reach',    '        if (i + nums[i] > maxReach) {'],
      ['c_update_reach',   '            maxReach = i + nums[i];'],
      ['',                 '        }'],
      ['c_check_goal',     '        if (maxReach >= n - 1) {'],
      ['c_ret_goal',       '            return true;'],
      ['',                 '        }'],
      ['',                 '    }'],
      ['',                 '    return true;'],
      ['',                 '}'],
      ['',                 ''],
      ['',                 'int main() {'],
      ['m_scanner',        '    int n;'],
      ['m_read_n',         '    scanf("%d", &n);'],
      ['m_alloc_nums',     '    int nums[100];'],
      ['m_for_input',      '    for (int i = 0; i < n; i++) {'],
      ['m_read_num',       '        scanf("%d", &nums[i]);'],
      ['',                 '    }'],
      ['m_call_jump',      '    bool ans = canJump(nums, n);'],
      ['m_print_ans',      '    printf("%s\\n", ans ? "true" : "false");'],
      ['m_done',           '    return 0;'],
      ['',                 '}']
    ],
    cpp: [
      ['',                 '#include <iostream>'],
      ['',                 '#include <vector>'],
      ['',                 'using namespace std;'],
      ['',                 ''],
      ['c_entry',          'bool canJump(const vector<int>& nums, int n) {'],
      ['c_init_reach',     '    int maxReach = 0;'],
      ['c_for_loop',       '    for (int i = 0; i < n; i++) {'],
      ['c_check_stranded', '        if (i > maxReach) {'],
      ['c_ret_stranded',   '            return false;'],
      ['',                 '        }'],
      ['c_check_reach',    '        if (i + nums[i] > maxReach) {'],
      ['c_update_reach',   '            maxReach = i + nums[i];'],
      ['',                 '        }'],
      ['c_check_goal',     '        if (maxReach >= n - 1) {'],
      ['c_ret_goal',       '            return true;'],
      ['',                 '        }'],
      ['',                 '    }'],
      ['',                 '    return true;'],
      ['',                 '}'],
      ['',                 ''],
      ['',                 'int main() {'],
      ['m_scanner',        '    int n;'],
      ['m_read_n',         '    cin >> n;'],
      ['m_alloc_nums',     '    vector<int> nums(n);'],
      ['m_for_input',      '    for (int i = 0; i < n; i++) {'],
      ['m_read_num',       '        cin >> nums[i];'],
      ['',                 '    }'],
      ['m_call_jump',      '    bool ans = canJump(nums, n);'],
      ['m_print_ans',      '    cout << (ans ? "true" : "false") << endl;'],
      ['m_done',           '    return 0;'],
      ['',                 '}']
    ],
    python: [
      ['',                 'import sys'],
      ['',                 ''],
      ['c_entry',          'def can_jump(nums, n):'],
      ['c_init_reach',     '    max_reach = 0'],
      ['c_for_loop',       '    for i in range(n):'],
      ['c_check_stranded', '        if i > max_reach:'],
      ['c_ret_stranded',   '            return False'],
      ['c_check_reach',    '        if i + nums[i] > max_reach:'],
      ['c_update_reach',   '            max_reach = i + nums[i]'],
      ['c_check_goal',     '        if max_reach >= n - 1:'],
      ['c_ret_goal',       '            return True'],
      ['',                 '    return True'],
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
      ['m_call_jump',      '    ans = can_jump(nums, n)'],
      ['m_print_ans',      '    print("true" if ans else "false")'],
      ['m_done',           '    return'],
      ['',                 ''],
      ['',                 'if __name__ == "__main__":'],
      ['',                 '    main()']
    ]
  },
  brute: {
    java: [
      ['',                 'import java.util.Scanner;'],
      ['',                 ''],
      ['',                 'public class Main {'],
      ['c_entry',          '    static boolean canJumpFrom(int idx, int[] nums, int n) {'],
      ['c_base_check',     '        if (idx >= n - 1) {'],
      ['c_ret_base',       '            return true;'],
      ['',                 '        }'],
      ['c_get_max',        '        int maxJump = nums[idx];'],
      ['c_for_step',       '        for (int step = 1; step <= maxJump; step++) {'],
      ['c_call_rec',       '            if (canJumpFrom(idx + step, nums, n)) {'],
      ['c_ret_found',      '                return true;'],
      ['',                 '            }'],
      ['',                 '        }'],
      ['c_ret_fail',       '        return false;'],
      ['',                 '    }'],
      ['',                 ''],
      ['',                 '    public static void main(String[] args) {'],
      ['m_scanner',        '        Scanner sc = new Scanner(System.in);'],
      ['m_read_n',         '        int n = sc.nextInt();'],
      ['m_alloc_nums',     '        int[] nums = new int[n];'],
      ['m_for_input',      '        for (int i = 0; i < n; i++) {'],
      ['m_read_num',       '            nums[i] = sc.nextInt();'],
      ['',                 '        }'],
      ['m_call_jump',      '        boolean ans = canJumpFrom(0, nums, n);'],
      ['m_print_ans',      '        System.out.println(ans ? "true" : "false");'],
      ['m_done',           '    }'],
      ['',                 '}']
    ],
    c: [
      ['',                 '#include <stdio.h>'],
      ['',                 '#include <stdbool.h>'],
      ['',                 ''],
      ['c_entry',          'bool canJumpFrom(int idx, int nums[], int n) {'],
      ['c_base_check',     '    if (idx >= n - 1) {'],
      ['c_ret_base',       '        return true;'],
      ['',                 '    }'],
      ['c_get_max',        '    int maxJump = nums[idx];'],
      ['c_for_step',       '    for (int step = 1; step <= maxJump; step++) {'],
      ['c_call_rec',       '        if (canJumpFrom(idx + step, nums, n)) {'],
      ['c_ret_found',      '            return true;'],
      ['',                 '        }'],
      ['',                 '    }'],
      ['c_ret_fail',       '    return false;'],
      ['',                 '}'],
      ['',                 ''],
      ['',                 'int main() {'],
      ['m_scanner',        '    int n;'],
      ['m_read_n',         '    scanf("%d", &n);'],
      ['m_alloc_nums',     '    int nums[100];'],
      ['m_for_input',      '    for (int i = 0; i < n; i++) {'],
      ['m_read_num',       '        scanf("%d", &nums[i]);'],
      ['',                 '    }'],
      ['m_call_jump',      '    bool ans = canJumpFrom(0, nums, n);'],
      ['m_print_ans',      '    printf("%s\\n", ans ? "true" : "false");'],
      ['m_done',           '    return 0;'],
      ['',                 '}']
    ],
    cpp: [
      ['',                 '#include <iostream>'],
      ['',                 '#include <vector>'],
      ['',                 'using namespace std;'],
      ['',                 ''],
      ['c_entry',          'bool canJumpFrom(int idx, const vector<int>& nums, int n) {'],
      ['c_base_check',     '    if (idx >= n - 1) {'],
      ['c_ret_base',       '        return true;'],
      ['',                 '    }'],
      ['c_get_max',        '    int maxJump = nums[idx];'],
      ['c_for_step',       '    for (int step = 1; step <= maxJump; step++) {'],
      ['c_call_rec',       '        if (canJumpFrom(idx + step, nums, n)) {'],
      ['c_ret_found',      '            return true;'],
      ['',                 '        }'],
      ['',                 '    }'],
      ['c_ret_fail',       '    return false;'],
      ['',                 '}'],
      ['',                 ''],
      ['',                 'int main() {'],
      ['m_scanner',        '    int n;'],
      ['m_read_n',         '    cin >> n;'],
      ['m_alloc_nums',     '    vector<int> nums(n);'],
      ['m_for_input',      '    for (int i = 0; i < n; i++) {'],
      ['m_read_num',       '        cin >> nums[i];'],
      ['',                 '    }'],
      ['m_call_jump',      '    bool ans = canJumpFrom(0, nums, n);'],
      ['m_print_ans',      '    cout << (ans ? "true" : "false") << endl;'],
      ['m_done',           '    return 0;'],
      ['',                 '}']
    ],
    python: [
      ['',                 'import sys'],
      ['',                 ''],
      ['c_entry',          'def can_jump_from(idx, nums, n):'],
      ['c_base_check',     '    if idx >= n - 1:'],
      ['c_ret_base',       '        return True'],
      ['c_get_max',        '    max_jump = nums[idx]'],
      ['c_for_step',       '    for step in range(1, max_jump + 1):'],
      ['c_call_rec',       '        if can_jump_from(idx + step, nums, n):'],
      ['c_ret_found',      '            return True'],
      ['c_ret_fail',       '    return False'],
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
      ['m_call_jump',      '    ans = can_jump_from(0, nums, n)'],
      ['m_print_ans',      '    print("true" if ans else "false")'],
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
  greedy: [
    'function canJump(nums, n):',
    '    maxReach = 0                                // Furthest index reachable so far',
    '    for i = 0 to n - 1:                         // Scan array linearly',
    '        if i > maxReach:                        // Stranded! Cannot reach index i',
    '            return false',
    '        if i + nums[i] > maxReach:              // New furthest reachable index found',
    '            maxReach = i + nums[i]              // Advance boundary forward',
    '        if maxReach >= n - 1:                   // Reached or surpassed destination!',
    '            return true',
    '    return true                                 // Time: O(n), Space: O(1)'
  ],
  brute: [
    'function canJumpFrom(idx, nums, n):',
    '    if idx >= n - 1:                            // Base case: destination reached',
    '        return true',
    '    maxJump = nums[idx]                         // Available jump distance',
    '    for step = 1 to maxJump:                    // Try every step length recursively',
    '        if canJumpFrom(idx + step, nums, n):    // Found a valid forward path',
    '            return true',
    '    return false                                // All jump paths exhausted (Dead end)',
    '// Time: O(2^n) exponential exploration'
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
  /* APPROACH 1: GREEDY (MAX REACH FORWARD SCAN)                        */
  /* ------------------------------------------------------------------ */
  if (approach === 'greedy') {
    function curGreedyState(extra = {}) {
      return {
        approach: 'greedy',
        n,
        nums,
        ...extra
      };
    }

    // Step 1: m_scanner
    steps.push({
      code: 'm_scanner',
      badge: 'Faculty Note: Scanner sc = new Scanner(System.in); \u2014 Initializing standard input stream.',
      vars: [frame('main()', [['n', '?']])],
      state: curGreedyState({ i: -1, maxReach: 0, status: 'input', numsFilled: 0 })
    });

    // Step 2: m_read_n
    steps.push({
      code: 'm_read_n',
      badge: `Faculty Note: int n = sc.nextInt(); &rarr; Read array size n = ${n}.`,
      vars: [frame('main()', [['n', String(n)]])],
      state: curGreedyState({ i: -1, maxReach: 0, status: 'input', numsFilled: 0 })
    });

    // Step 3: m_alloc_nums
    steps.push({
      code: 'm_alloc_nums',
      badge: `Faculty Note: int[] nums = new int[${n}]; &mdash; Allocated jump capability array.`,
      vars: [frame('main()', [['n', String(n)]])],
      state: curGreedyState({ i: -1, maxReach: 0, status: 'input', numsFilled: 0 })
    });

    // Step 4: m_for_input & m_read_num — one value at a time
    for (let idx = 0; idx < n; idx++) {
      steps.push({
        code: 'm_for_input',
        badge: `Reading element at index ${idx} of ${n}.`,
        vars: [frame('main()', [['i', String(idx)], ['n', String(n)]])],
        state: curGreedyState({ i: -1, maxReach: 0, status: 'input', numsFilled: idx })
      });
      steps.push({
        code: 'm_read_num',
        badge: `nums[${idx}] = sc.nextInt(); \u2192 nums[${idx}] = ${nums[idx]} read. (From stone ${idx}, you can jump up to ${nums[idx]} steps forward).`,
        vars: [frame('main()', [['i', String(idx)], ['nums[' + idx + ']', String(nums[idx])]])],
        state: curGreedyState({ i: -1, maxReach: 0, status: 'input', numsFilled: idx + 1 })
      });
    }

    // Step 5: m_call_jump
    steps.push({
      code: 'm_call_jump',
      badge: `Calling canJump(nums, n=${n}). Optimal Greedy algorithm begins!`,
      vars: [frame('main()', [['n', String(n)]])],
      state: curGreedyState({ i: -1, maxReach: 0, status: 'call' })
    });

    // Step 6: c_entry
    steps.push({
      code: 'c_entry',
      badge: 'Entering canJump(nums, n). Target: Can we reach last index ' + (n - 1) + '?',
      vars: [frame('main()', [['n', String(n)]]), frame('canJump()', [['n', String(n)]])],
      state: curGreedyState({ i: -1, maxReach: 0, status: 'entry' })
    });

    // Step 7: c_init_reach
    let maxReach = 0;
    steps.push({
      code: 'c_init_reach',
      badge: 'int maxReach = 0; Starting at index 0. Initial furthest reach boundary set to index 0.',
      vars: [frame('main()', [['n', String(n)]]), frame('canJump()', [['maxReach', '0']])],
      state: curGreedyState({ i: 0, maxReach: 0, status: 'running' })
    });

    let reachedGoal = false;
    let gotStranded = false;

    // Loop
    for (let i = 0; i < n; i++) {
      steps.push({
        code: 'c_for_loop',
        badge: `Scanning index i = ${i} (nums[${i}] = ${nums[i]}). Current maxReach = ${maxReach}.`,
        vars: [frame('main()', [['n', String(n)]]), frame('canJump()', [['i', String(i)], ['nums[i]', String(nums[i])], ['maxReach', String(maxReach)]])],
        state: curGreedyState({ i, maxReach, status: 'running' })
      });

      const isStranded = (i > maxReach);
      steps.push({
        code: 'c_check_stranded',
        badge: `Check stranded condition: if (i=${i} > maxReach=${maxReach}) &rarr; ${isStranded ? 'TRUE! Cannot reach this stone!' : 'FALSE (Stone is reachable)'}`,
        vars: [frame('main()', [['n', String(n)]]), frame('canJump()', [['i', String(i)], ['maxReach', String(maxReach)]])],
        state: curGreedyState({ i, maxReach, status: isStranded ? 'stranded' : 'running' })
      });

      if (isStranded) {
        gotStranded = true;
        steps.push({
          code: 'c_ret_stranded',
          badge: `return false; Stranded at index ${i}! Unable to leap across previous zero trap. Destination unreachable.`,
          vars: [frame('main()', [['n', String(n)]]), frame('canJump()', [['return', 'false']])],
          state: curGreedyState({ i, maxReach, status: 'failed' })
        });
        break;
      }

      const potReach = i + nums[i];
      const improves = potReach > maxReach;
      steps.push({
        code: 'c_check_reach',
        badge: `Evaluate jump from stone ${i}: i + nums[${i}] = ${i} + ${nums[i]} = ${potReach}. Check: potReach > maxReach (${potReach} > ${maxReach}) &rarr; ${improves ? 'TRUE (Boundary expands!)' : 'FALSE (Does not exceed current reach)'}`,
        vars: [frame('main()', [['n', String(n)]]), frame('canJump()', [['i', String(i)], ['potReach', String(potReach)], ['maxReach', String(maxReach)]])],
        state: curGreedyState({ i, maxReach, arcFrom: i, arcTo: potReach, status: 'running' })
      });

      if (improves) {
        maxReach = potReach;
        steps.push({
          code: 'c_update_reach',
          badge: `maxReach updated forward to index ${maxReach}! Laser boundary advances.`,
          vars: [frame('main()', [['n', String(n)]]), frame('canJump()', [['i', String(i)], ['maxReach', String(maxReach)]])],
          state: curGreedyState({ i, maxReach, arcFrom: i, arcTo: potReach, status: 'running' })
        });
      }

      const hitGoal = maxReach >= n - 1;
      steps.push({
        code: 'c_check_goal',
        badge: `Check destination: if (maxReach=${maxReach} >= n - 1 = ${n - 1}) &rarr; ${hitGoal ? 'TRUE! Destination stone ' + (n - 1) + ' reached or surpassed!' : 'FALSE (Continue scanning)'}`,
        vars: [frame('main()', [['n', String(n)]]), frame('canJump()', [['maxReach', String(maxReach)], ['goal', String(n - 1)]])],
        state: curGreedyState({ i, maxReach, status: hitGoal ? 'goal' : 'running' })
      });

      if (hitGoal) {
        reachedGoal = true;
        steps.push({
          code: 'c_ret_goal',
          badge: `return true; Goal index ${n - 1} is within guaranteed reach! Early exit optimal return.`,
          vars: [frame('main()', [['n', String(n)]]), frame('canJump()', [['return', 'true']])],
          state: curGreedyState({ i, maxReach, status: 'success' })
        });
        break;
      }
    }

    if (!gotStranded && !reachedGoal) {
      steps.push({
        code: 'c_ret_goal',
        badge: 'return true; Completed full scan of all stones. Destination reached.',
        vars: [frame('main()', [['n', String(n)]]), frame('canJump()', [['return', 'true']])],
        state: curGreedyState({ i: n - 1, maxReach, status: 'success' })
      });
      reachedGoal = true;
    }

    const finalAns = reachedGoal && !gotStranded;
    steps.push({
      code: 'm_print_ans',
      badge: `Output Result: System.out.println("${finalAns ? 'true' : 'false'}"); — Solved in O(n) linear time!`,
      vars: [frame('main()', [['ans', finalAns ? 'true' : 'false']])],
      state: curGreedyState({ i: -1, maxReach, status: finalAns ? 'success' : 'failed' })
    });

    steps.push({
      code: 'm_done',
      badge: `Program execution finished. Destination was ${finalAns ? 'SUCCESSFULLY REACHED' : 'UNREACHABLE'}.`,
      vars: [frame('main()', [['status', 'finished']])],
      state: curGreedyState({ i: -1, maxReach, status: finalAns ? 'success' : 'failed' })
    });

    return steps;
  }

  /* ------------------------------------------------------------------ */
  /* APPROACH 2: BRUTE FORCE (RECURSIVE BACKTRACKING TREE)              */
  /* ------------------------------------------------------------------ */
  else {
    const allNodes = [];
    let nodeIdCounter = 0;

    function buildBruteTree(idx, parentId, jumpStep) {
      const nodeId = nodeIdCounter++;
      const node = { id: nodeId, idx, parentId, jumpStep, children: [], isGoal: idx >= n - 1, canReach: false };
      allNodes.push(node);
      if (parentId !== null) {
        const par = allNodes.find(x => x.id === parentId);
        if (par) par.children.push(nodeId);
      }
      if (idx >= n - 1) {
        node.canReach = true;
        return true;
      }
      const maxJ = Math.min(nums[idx] || 0, n);
      let found = false;
      for (let s = 1; s <= maxJ; s++) {
        const childCan = buildBruteTree(idx + s, nodeId, s);
        if (childCan) found = true;
      }
      node.canReach = found;
      return found;
    }
    buildBruteTree(0, null, 0);

    const nodeStateMap = {};
    allNodes.forEach(nd => { nodeStateMap[nd.id] = { ...nd, state: 'hidden', retVal: null }; });

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
    const treeWidth = Math.max(220, totalLeaves * leafSpacing + 30);
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
              canReach: nd.canReach
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
      badge: `Faculty Note: int[] nums = new int[${n}]; &mdash; Allocating jump array.`,
      vars: [frame('main()', [['n', String(n)]])],
      state: curBruteState({ callCounter: 0, activeNodeId: null, currentIdx: -1, numsFilled: 0 })
    });

    for (let idx = 0; idx < n; idx++) {
      steps.push({
        code: 'm_for_input',
        badge: `Reading jump capacity at index ${idx}.`,
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
      badge: `Invoking canJumpFrom(idx=0, nums, n=${n}). Exhaustive backtracking search begins!`,
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
        frame(`canJumpFrom(idx=${idx})`, [['idx', String(idx)], ['maxJump', String(nums[idx] || 0)]])
      ];

      steps.push({
        code: 'c_entry',
        badge: `Entering canJumpFrom(idx=${idx}). Recursive Call #${calls}.`,
        vars: cs,
        state: curBruteState({ callCounter: calls, activeNodeId: nodeId, currentIdx: idx })
      });

      const isBase = idx >= n - 1;
      steps.push({
        code: 'c_base_check',
        badge: `Base check: if (idx=${idx} >= n - 1 = ${n - 1}) &rarr; ${isBase ? 'TRUE (Goal reached!)' : 'FALSE (Must branch further)'}`,
        vars: cs,
        state: curBruteState({ callCounter: calls, activeNodeId: nodeId, currentIdx: idx })
      });

      if (isBase) {
        nodeStateMap[nodeId].state = 'leaf';
        nodeStateMap[nodeId].retVal = true;
        steps.push({
          code: 'c_ret_base',
          badge: `return true; Destination reached at stone ${idx}!`,
          vars: cs,
          state: curBruteState({ callCounter: calls, activeNodeId: nodeId, currentIdx: idx })
        });
        nodeStateMap[nodeId].state = 'solved';
        return true;
      }

      const maxJump = nums[idx] || 0;
      steps.push({
        code: 'c_get_max',
        badge: `int maxJump = nums[${idx}] = ${maxJump}. Stone ${idx} allows jumps of 1 to ${maxJump} steps forward.`,
        vars: cs,
        state: curBruteState({ callCounter: calls, activeNodeId: nodeId, currentIdx: idx })
      });

      let reached = false;
      const ndObj = allNodes.find(x => x.id === nodeId);
      const childIds = ndObj ? ndObj.children : [];

      for (let s = 1; s <= maxJump; s++) {
        steps.push({
          code: 'c_for_step',
          badge: `Trying jump of step = ${s} from stone ${idx} to stone ${idx + s}.`,
          vars: cs,
          state: curBruteState({ callCounter: calls, activeNodeId: nodeId, currentIdx: idx })
        });

        steps.push({
          code: 'c_call_rec',
          badge: `Evaluating branch canJumpFrom(idx + step = ${idx + s})...`,
          vars: cs,
          state: curBruteState({ callCounter: calls, activeNodeId: nodeId, currentIdx: idx })
        });

        const cId = childIds[s - 1];
        if (cId !== undefined) {
          const res = simulateBrute(idx + s, cId, [...callStack, frame(`canJumpFrom(idx=${idx})`, [['idx', String(idx)]])]);
          if (res) {
            reached = true;
            steps.push({
              code: 'c_ret_found',
              badge: `return true; Forward path from stone ${idx + s} confirmed viable! Propagating true back to caller.`,
              vars: cs,
              state: curBruteState({ callCounter: calls, activeNodeId: nodeId, currentIdx: idx })
            });
            break;
          }
        }
      }

      if (!reached) {
        steps.push({
          code: 'c_ret_fail',
          badge: `return false; All jumps from stone ${idx} lead to dead ends! Backtracking...`,
          vars: cs,
          state: curBruteState({ callCounter: calls, activeNodeId: nodeId, currentIdx: idx })
        });
        nodeStateMap[nodeId].state = 'solved';
        nodeStateMap[nodeId].retVal = false;
        return false;
      }

      nodeStateMap[nodeId].state = 'solved';
      nodeStateMap[nodeId].retVal = true;
      return true;
    }

    const finalResult = simulateBrute(0, 0, []);

    steps.push({
      code: 'm_print_ans',
      badge: `Output Result: System.out.println("${finalResult ? 'true' : 'false'}"); — Total recursive branches explored: ${calls}.`,
      vars: [frame('main()', [['ans', finalResult ? 'true' : 'false']])],
      state: curBruteState({ callCounter: calls, activeNodeId: null, currentIdx: -1 })
    });

    steps.push({
      code: 'm_done',
      badge: `Brute Force Search Complete! Result: ${finalResult ? 'Goal is Reachable' : 'Goal is Unreachable'}.`,
      vars: [frame('main()', [['status', 'finished']])],
      state: curBruteState({ callCounter: calls, activeNodeId: null, currentIdx: -1 })
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
const vizHeight = ref(185);
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
  playing.value = false;
  stepsData.steps = buildSteps(appId, currentNums.value);
  si.value = 0;
}

function applyInput() {
  playing.value = false;
  stepsData.steps = buildSteps(currentApproach.value, currentNums.value);
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

/* ==================================================================== */
/* PANEL RESIZERS                                                       */
/* ==================================================================== */
const mainRef = ref(null);
const leftColRef = ref(null);
const hResizerRef = ref(null);
const vizResizerRef = ref(null);
const tableResizerRef = ref(null);

function initHResizer() {
  const rsz = hResizerRef.value, main = mainRef.value;
  if (!rsz || !main) return;
  let dragging = false, startX = 0, startW = 0;
  const onDown = e => {
    dragging = true; startX = e.clientX; startW = leftColRef.value.offsetWidth;
    rsz.classList.add('drag'); document.body.style.userSelect = 'none';
  };
  const onMove = e => {
    if (!dragging) return;
    const mainW = main.offsetWidth;
    leftWidth.value = (Math.max(220, Math.min(mainW - 220, startW + e.clientX - startX)) / mainW) * 100;
  };
  const onUp = () => {
    if (!dragging) return;
    dragging = false; rsz.classList.remove('drag'); document.body.style.userSelect = '';
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
  if (!rsz) return;
  let dragging = false, startY = 0, startH = 0;
  const onDown = e => {
    dragging = true; startY = e.clientY; startH = valueRef.value;
    rsz.classList.add('drag'); document.body.style.userSelect = 'none'; e.preventDefault();
  };
  const onMove = e => {
    if (!dragging) return;
    valueRef.value = Math.max(minH, Math.min(maxH, startH + (e.clientY - startY)));
  };
  const onUp = () => {
    if (!dragging) return;
    dragging = false; rsz.classList.remove('drag'); document.body.style.userSelect = '';
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
  cleanupFns.push(initVResizer(vizResizerRef, vizHeight, 130, 420));
  cleanupFns.push(initVResizer(tableResizerRef, tableHeight, 30, 140));
});

onUnmounted(() => {
  document.removeEventListener('keydown', onKeydown);
  clearTimeout(playTimer);
  cleanupFns.forEach(fn => fn && fn());
});
</script>

<template>
  <div class="slide-wrapper">
    <!-- Top Navbar -->
    <div class="navbar">
      <h2 class="navbar-title">{{ topic }} &mdash; {{ subTopic }}</h2>
      <img src="../../assets/logo.png" alt="Logo" />
    </div>

    <div class="slide-body">
      <div class="row-main">
        <div class="ll-root">
          <!-- Control Panel Toolbar -->
          <div class="ll-toolbar">
            <!-- Approach Selector Button Group -->
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

            <!-- Input Array nums -->
            <div class="ll-input-group">
              <label>nums =</label>
              <input
                type="text"
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
              <!-- Top Banner: Array Display with Index Cards -->
              <div class="ll-house-banner">
                <div class="ll-house-title">
                  Stepping Stone Array: <code>nums[0 ... {{ currentNums.length - 1 }}]</code>
                </div>

                <div class="ll-dual-arrays-wrap">
                  <div class="ll-arr-row">
                    <span class="ll-arr-tag">nums[]:</span>
                    <div class="ll-arr-cells">
                      <div
                        v-for="(val, idx) in currentNums"
                        :key="idx"
                        class="ll-arr-cell"
                        :class="{
                          'll-cell-active': st.i === idx || st.currentIdx === idx,
                          'll-cell-reach': st.maxReach !== undefined && idx <= st.maxReach && idx > 0,
                          'll-cell-goal': idx === currentNums.length - 1
                        }"
                      >
                        <span class="ll-cell-idx">[{{ idx }}]</span>
                        <span class="ll-cell-val">{{ (st.numsFilled !== undefined && idx >= st.numsFilled) ? '?' : val }}</span>
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
                    Current Stone: <b class="ll-c-orange">i = {{ st.i }} (nums = {{ currentNums[st.i] }})</b>
                  </div>
                  <div class="ll-ptr-chip" v-if="st.maxReach !== undefined">
                    Max Reach: <b class="ll-c-green">{{ st.maxReach }}</b>
                  </div>
                  <div class="ll-ptr-chip">
                    Destination: <b class="ll-c-purple">Index {{ currentNums.length - 1 }} [Target]</b>
                  </div>
                  <div class="ll-ptr-chip" :class="{ 'll-chip-warn': st.status === 'stranded', 'll-chip-success': st.status === 'success' }">
                    Status: <b :class="{ 'll-c-red': st.status === 'stranded' || st.status === 'failed', 'll-c-green': st.status === 'success' || st.status === 'goal', 'll-c-blue': st.status === 'running' }">
                      {{ st.status === 'stranded' ? 'Stranded (Zero-Trap)' : (st.status === 'success' || st.status === 'goal' ? 'Destination Reachable' : (st.status === 'failed' ? 'Unreachable' : 'Scanning Forward')) }}
                    </b>
                  </div>
                </template>
                <template v-else>
                  <div class="ll-ptr-chip">Recursive Calls: <b class="ll-c-orange">{{ st.callCounter || 0 }}</b></div>
                  <div class="ll-ptr-chip">
                    Current Stone: <b class="ll-c-blue">{{ st.currentIdx >= 0 ? 'idx = ' + st.currentIdx : 'Start (0)' }}</b>
                  </div>
                </template>
              </div>

              <!-- Visualization Viewport Container -->
              <div class="ll-viz-wrap" :style="{ height: vizHeight + 'px' }">
                <!-- VIEW 1: GREEDY JUMP TRACK & ARC ANIMATION -->
                <div v-if="currentApproach === 'greedy'" class="ll-timeline-container">
                  <div class="ll-section-caption">
                    <span>Greedy Jump Track &mdash; Forward Scan &amp; Max Reach Horizon</span>
                    <span v-if="st.maxReach !== undefined && st.maxReach >= currentNums.length - 1" class="ll-calc-pill">&#10003; Goal In Range</span>
                    <span v-else-if="st.status === 'stranded'" class="ll-calc-pill ll-pill-error">&#10007; Stranded</span>
                  </div>

                  <div class="ll-jump-track-scroll">
                    <div class="ll-track-board" :style="{ width: Math.max(340, currentNums.length * 68) + 'px' }">
                      <!-- 1. Arc Layer (Curved path showing jump from i to i + nums[i]) -->
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
                            'll-pod-reach': st.maxReach !== undefined && idx <= st.maxReach,
                            'll-pod-stranded': st.i === idx && st.status === 'stranded',
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
                            <div class="ll-stone-card-val">{{ (st.numsFilled !== undefined && idx >= st.numsFilled) ? '?' : val }}</div>
                            <div class="ll-stone-card-sub">cap: {{ (st.numsFilled !== undefined && idx >= st.numsFilled) ? '?' : val }}</div>
                          </div>

                          <!-- Bottom Laser Barrier Pill (maxReach) -->
                          <div class="ll-pod-barrier">
                            <div
                              v-if="st.maxReach !== undefined && idx === Math.min(currentNums.length - 1, st.maxReach)"
                              class="ll-barrier-tag"
                            >
                              <div class="ll-barrier-arrow">&#9650;</div>
                              <div class="ll-barrier-lbl">reach={{ st.maxReach }}</div>
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
                              width: st.maxReach !== undefined
                                ? Math.min(100, (((Math.min(currentNums.length - 1, st.maxReach)) + 0.5) / currentNums.length) * 100) + '%'
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
                          :class="{ 'll-edge-pick': edge.canReach, 'll-edge-skip': !edge.canReach }"
                        />
                      </g>

                      <!-- Dynamic Nodes (Compact 38x22 with 6px spacing) -->
                      <g class="ll-tree-nodes">
                        <g
                          v-for="node in st.treeNodes"
                          :key="node.id"
                          :transform="`translate(${node.x}, ${node.y})`"
                          class="ll-tree-node-group"
                        >
                          <rect
                            x="-19"
                            y="-11"
                            width="38"
                            height="22"
                            rx="4"
                            class="ll-node-rect"
                            :class="{
                              'll-node-active': node.id === st.activeNodeId,
                              'll-node-solved': node.state === 'solved',
                              'll-node-leaf': node.isGoal
                            }"
                          />
                          <text x="0" y="-2.5" text-anchor="middle" class="ll-node-text-call">
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
                            {{ node.isGoal ? 'Goal' : (node.retVal !== null ? (node.retVal ? 'True' : 'Dead') : `jump≤${currentNums[node.idx] || 0}`) }}
                          </text>
                        </g>
                      </g>
                    </svg>
                  </div>
                </div>
              </div>

              <!-- Vertical Resizer for Viz Panel -->
              <div class="ll-vresizer" ref="vizResizerRef"></div>

              <!-- Dedicated Legend Strip (Exact match with ActivitySelection / CoinChange design) -->
              <div class="ll-legend">
                <template v-if="currentApproach === 'greedy'">
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-cur"></span>Active Stone i</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-new"></span>Reachable Zone</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-red"></span>maxReach Barrier</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-purple"></span>Goal Stone (n - 1)</span>
                </template>
                <template v-else>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-cur"></span>Active Call</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-new"></span>Feasible (True)</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-purple"></span>Goal Node</span>
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
                    'll-badge-error': s.badge && (s.badge.includes('Stranded') || s.badge.includes('dead') || s.badge.includes('false')),
                    'll-badge-success': s.badge && (s.badge.includes('Destination reached') || s.badge.includes('reached') || s.badge.includes('true'))
                  }"
                >
                  {{ s.badge || 'Ready to visualize Jump Game.' }}
                </div>
              </div>
            </div>

            <!-- Horizontal Resizer between Left and Right Columns -->
            <div class="ll-resizer" ref="hResizerRef"></div>

            <!-- Right Column: Code & Faculty Lecture Panel -->
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
                  </select>
                </div>

                <!-- Code Tab -->
                <div v-if="rightTab === 'code'" class="ll-code-scroll">
                  <pre class="ll-pre"><span v-for="(line, i) in codeLines" :key="i" class="ll-codeline" :class="{ 'll-hl': line[0] && line[0] === s.code }">{{ line[1] === '' ? ' ' : line[1] }}</span></pre>
                </div>

                <!-- Pseudocode Tab -->
                <div v-else-if="rightTab === 'pseudo'" class="ll-code-scroll">
                  <pre class="ll-pre"><span v-for="(line, i) in pseudocodeLines" :key="i" class="ll-codeline">{{ line }}</span></pre>
                </div>

                <!-- Time and Space Complexity Tab -->
                <div v-else class="ll-info-scroll">

                  <!-- ════════ BRUTE FORCE COMPLEXITY ════════ -->
                  <template v-if="currentApproach === 'brute'">
                    <h3 class="ll-cx-heading">Brute Force &mdash; Complexity Analysis</h3>
                    <p class="ll-cx-intro">
                      Exhaustively explores every possible sequence of jumps using recursion.
                      At each stone <code>idx</code>, it tries <strong>every valid step</strong> from 1 to <code>nums[idx]</code>,
                      branching into a potentially exponential tree of recursive calls.
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
                          <td>Up to nums[i] branches per node; recursion tree can grow exponentially; stack depth = n</td>
                        </tr>
                        <tr>
                          <td>Base case check at each node</td>
                          <td class="ll-cx-good">O(1)</td>
                          <td class="ll-cx-good">O(1)</td>
                          <td>Simple index comparison <code>idx &ge; n - 1</code></td>
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
                      For real jump game problems with large arrays, the Greedy single-pass approach is the only practical choice.
                    </div>
                  </template>

                  <!-- ════════ GREEDY COMPLEXITY ════════ -->
                  <template v-else>
                    <h3 class="ll-cx-heading">Greedy Algorithm &mdash; Complexity Analysis</h3>
                    <p class="ll-cx-intro">
                      Maintains a single scalar <code>maxReach</code> representing the furthest index
                      reachable by any valid sequence of jumps seen so far.
                      One left-to-right pass is enough &mdash; no sorting, no extra array needed.
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
                          <td>Initialise <code>maxReach = 0</code></td>
                          <td class="ll-cx-good">O(1)</td>
                          <td class="ll-cx-good">O(1)</td>
                          <td>Single integer variable</td>
                        </tr>
                        <tr>
                          <td>Greedy forward scan</td>
                          <td class="ll-cx-good">O(n)</td>
                          <td class="ll-cx-good">O(1)</td>
                          <td>Single left-to-right pass; stranded / goal check is O(1) per step</td>
                        </tr>
                      </tbody>
                    </table>

                    <h4 class="ll-cx-sub">Overall Complexity</h4>
                    <div class="ll-cx-summary-grid">
                      <div class="ll-cx-card ll-cx-card-good">
                        <div class="ll-cx-card-label">Time</div>
                        <div class="ll-cx-card-val">O(n)</div>
                        <div class="ll-cx-card-note">Single forward scan &mdash; optimal</div>
                      </div>
                      <div class="ll-cx-card ll-cx-card-good">
                        <div class="ll-cx-card-label">Space</div>
                        <div class="ll-cx-card-val">O(1)</div>
                        <div class="ll-cx-card-note">Only a scalar <code>maxReach</code> variable</div>
                      </div>
                      <div class="ll-cx-card ll-cx-card-good">
                        <div class="ll-cx-card-label">Optimal?</div>
                        <div class="ll-cx-card-val">YES</div>
                        <div class="ll-cx-card-note">Provable via Greedy Choice Property</div>
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
                          <td><strong>Greedy (Max Reach)</strong></td>
                          <td class="ll-cx-good">O(n)</td>
                          <td class="ll-cx-good">O(1)</td>
                          <td class="ll-cx-good">Yes &mdash; scales to millions</td>
                        </tr>
                      </tbody>
                    </table>

                    <div class="ll-note">
                      <strong>Key Insight:</strong> Reachability is a <em>contiguous interval</em> &mdash; if index <code>k</code> is
                      reachable, every index before <code>k</code> was reachable too. Maximising
                      <code>maxReach</code> captures all valid paths in O(1) auxiliary space,
                      making the Greedy approach strictly superior to DP and backtracking.
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
  --shadow-sm: 0 1px 3px rgba(0,0,0,.08), 0 1px 2px rgba(0,0,0,.04);
  --radius: 8px;
  --radius-sm: 6px;
  background: var(--bg);
  color: var(--text);
  font-family: 'Segoe UI', system-ui, sans-serif;
  font-size: 12.5px;
  height: 75vh;
  width: 100%;
  display: flex;
  flex-direction: column;
  overflow: hidden;
}

@keyframes ll-pop { 0% { transform: scale(0.85); opacity: 0; } 100% { transform: scale(1); opacity: 1; } }
@keyframes ll-pulse { 0%, 100% { transform: scale(1); } 50% { transform: scale(1.05); } }

.slide-wrapper {
  width: 107%;
  max-height: 100%;
  margin-top: -10px;
  margin-left: -30px;
  font-size: 0.8rem;
  font-weight: 400;
}

.slide-body {
  height: 100%;
  display: flex;
  flex-direction: column;
  border-radius: 4px;
}

.navbar {
  background-color: #ffffff;
  padding: 0 10px;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  gap: 0.75rem;
  width: 94.7%;
  position: fixed;
}
.navbar > img { height: 30px; }
.navbar-title {
  font-size: 1.35rem;
  font-weight: 700;
  color: #ffffff;
  background-color: #ef5050;
  border-radius: 5px;
  padding: 2px 10px;
  width: 80%;
  margin: 0 0 0 -10px;
}

.row-main {
  width: 100%;
  height: 90%;
  margin-top: 36px;
  overflow: auto;
}

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
  gap: 2px;
  background: var(--surface2);
  padding: 2px;
  border-radius: var(--radius-sm);
  border: 1px solid var(--border);
}

.ll-approach-btn {
  padding: 4px 8px;
  font-size: 11px;
  font-weight: 600;
  border: none;
  background: transparent;
  color: var(--text2);
  border-radius: 4px;
  cursor: pointer;
  transition: all .15s ease;
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
  font-size: 11.5px;
  color: var(--muted);
  font-weight: 700;
}

.ll-text-input {
  width: 135px;
  padding: 3px 6px;
  font-family: monospace;
  font-size: 11px;
  border: 1px solid var(--border2);
  border-radius: var(--radius-sm);
  background: var(--surface);
  color: var(--text);
}
.ll-text-input:focus {
  outline: none;
  border-color: var(--coral);
  box-shadow: 0 0 0 3px rgba(240,77,77,.1);
}

.ll-viz-btn {
  padding: 5px 12px;
  font-size: 11.5px;
  font-weight: 600;
  background: var(--coral);
  color: #fff;
  border: none;
  border-radius: var(--radius-sm);
  cursor: pointer;
  box-shadow: var(--shadow-sm);
  transition: filter .15s;
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
  font-size: 11px;
  font-weight: 500;
  background: var(--surface2);
  border: 1px solid var(--border2);
  color: var(--text2);
  border-radius: var(--radius-sm);
  cursor: pointer;
  transition: all .15s ease;
  white-space: nowrap;
}
.ll-nav-btn:hover {
  background: var(--surface);
  border-color: var(--coral);
  color: var(--coral);
}

.ll-play-btn {
  padding: 4px 9px;
  font-size: 11px;
  font-weight: 600;
  background: var(--blue-light);
  border: 1px solid var(--blue);
  color: var(--blue);
  border-radius: var(--radius-sm);
  cursor: pointer;
  min-width: 68px;
  transition: all .15s ease;
}
.ll-play-btn:hover {
  background: var(--blue);
  color: #fff;
}

.ll-main {
  display: flex;
  flex: 1;
  overflow: hidden;
  position: relative;
}

.ll-left-col {
  display: flex;
  flex-direction: column;
  min-width: 220px;
  max-width: 75%;
  overflow: hidden;
}

.ll-house-banner {
  background: var(--surface);
  border-bottom: 1px solid var(--border);
  padding: 4px 10px 6px;
  flex-shrink: 0;
}

.ll-house-title {
  font-size: 10.5px;
  font-weight: 700;
  color: var(--text2);
  margin-bottom: 2px;
}

.ll-dual-arrays-wrap {
  display: flex;
  flex-direction: column;
  gap: 5px;
  margin-top: 2px;
}

.ll-arr-row {
  display: flex;
  align-items: center;
  gap: 6px;
}

.ll-arr-tag {
  font-size: 10px;
  font-family: monospace;
  font-weight: 800;
  color: var(--coral);
  width: 45px;
  flex-shrink: 0;
}

.ll-arr-cells {
  display: flex;
  gap: 7px;
}

.ll-arr-cell {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  background: var(--surface2);
  border: 1px solid var(--border2);
  border-radius: 5px;
  width: 40px;
  padding: 2px 3px;
  transition: all .2s ease;
}

.ll-cell-idx {
  font-size: 8.5px;
  color: var(--muted);
  font-family: monospace;
}

.ll-cell-val {
  font-size: 11px;
  font-weight: 700;
  color: var(--text);
  font-family: monospace;
}

.ll-cell-active {
  border-color: #f59e0b !important;
  background: #fef3c7 !important;
  transform: translateY(-2px);
  box-shadow: 0 0 0 2px rgba(245,158,11,.3);
}

.ll-cell-reach {
  border-color: #10b981 !important;
  background: #f0fdf4 !important;
}

.ll-cell-goal {
  border-color: #9333ea !important;
  background: #f3e8ff !important;
}

.ll-ptrs {
  display: flex;
  gap: 6px;
  padding: 3px 10px;
  min-height: 26px;
  flex-wrap: wrap;
  width: 100%;
  box-sizing: border-box;
}

.ll-ptr-chip {
  padding: 2px 7px;
  font-size: 10.5px;
  background: var(--surface2);
  border: 1px solid var(--border);
  border-radius: var(--radius-sm);
  box-shadow: var(--shadow-sm);
  white-space: nowrap;
  font-family: monospace;
}

.ll-chip-warn { background: #fef2f2 !important; border-color: #fca5a5 !important; }
.ll-chip-success { background: #f0fdf4 !important; border-color: #86efac !important; }

.ll-c-blue { color: var(--blue); }
.ll-c-orange { color: var(--orange); }
.ll-c-green { color: var(--green); }
.ll-c-purple { color: var(--purple); }
.ll-c-red { color: var(--red); }

.ll-viz-wrap {
  flex-shrink: 0;
  position: relative;
  background: var(--surface);
  border-bottom: 1px solid var(--border);
  overflow: auto;
}

.ll-timeline-container {
  display: flex;
  flex-direction: column;
  width: 100%;
  height: 100%;
  min-height: 0;
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
  overflow: auto;
  padding: 6px 10px;
  display: flex;
  flex-direction: column;
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

.ll-pod-stranded .ll-stone-card {
  border-color: #ef4444 !important;
  background: #fef2f2 !important;
  box-shadow: 0 0 0 3px rgba(239, 68, 68, 0.25) !important;
}
.ll-pod-stranded .ll-stone-card-val {
  color: #991b1b !important;
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
  color: #ef4444;
  font-size: 7.5px;
  line-height: 1;
}

.ll-barrier-lbl {
  font-family: Consolas, monospace;
  font-size: 8px;
  font-weight: 700;
  color: #ffffff;
  background: #ef4444;
  border-radius: 3px;
  padding: 1px 4px;
  white-space: nowrap;
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

.ll-pill-error {
  background: #fee2e2 !important;
  color: #991b1b !important;
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
  flex: 1;
  overflow: auto;
  padding: 4px 6px;
  display: flex;
  flex-direction: column;
  align-items: center;
  min-height: 0;
  width: 100%;
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

.ll-edge-skip {
  stroke: #94a3b8 !important;
  stroke-dasharray: 4,3 !important;
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

.ll-right-col {
  flex: 1;
  display: flex;
  flex-direction: column;
  min-width: 0;
  overflow: hidden;
}

.ll-table-area {
  padding: 4px 10px;
  background: var(--surface);
  border-bottom: 1px solid var(--border);
  flex-shrink: 0;
  overflow-y: auto;
  overflow-x: hidden;
  box-sizing: border-box;
  min-width: 0;
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

.ll-fname { color: var(--text2); }
.ll-now { color: var(--orange); margin-left: 4px; font-size: 9.5px; }
.ll-frame {
  font-family: Consolas, monospace;
  font-size: 10.5px;
  color: var(--text2);
  white-space: nowrap;
  padding: 1px 0;
}

.ll-badge-wrap {
  padding: 4px 8px;
  background: var(--surface);
  border-bottom: 1px solid var(--border);
  display: flex;
  align-items: center;
  min-height: 30px;
  flex-shrink: 0;
}

.ll-badge {
  font-size: 10.5px;
  line-height: 1.35;
  padding: 3px 10px;
  border-radius: var(--radius-sm);
  background: var(--coral-light);
  color: var(--coral-dark);
  border-left: 3px solid var(--coral);
  word-break: break-word;
  font-weight: 500;
  display: inline-block;
}

.ll-badge-error {
  background: var(--red-light);
  color: var(--red-dark);
  border-left-color: var(--red);
  font-weight: 600;
}
.ll-badge-success {
  background: var(--green-light);
  color: #166534;
  border-left-color: var(--green);
  font-weight: 600;
}

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
  font-size: 10.5px;
  font-weight: 600;
  border: 1px solid var(--border2);
  background: var(--surface2);
  color: var(--text2);
  border-radius: var(--radius-sm);
  cursor: pointer;
  transition: all .15s ease;
  white-space: nowrap;
}
.ll-tab-btn:hover { border-color: var(--coral); color: var(--coral); }
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
  min-width: 95px;
  transition: border-color .15s;
}
.ll-lang-select:focus {
  outline: none;
  border-color: var(--coral);
  box-shadow: 0 0 0 3px rgba(240,77,77,.1);
}

.ll-code-scroll {
  flex: 1;
  overflow: auto;
  background: #f8fafc;
  padding: 10px 14px;
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
  color: #15803d;
  font-weight: 600;
  border-left: 3px solid var(--green);
  border-radius: 3px;
}

.ll-info-scroll {
  flex: 1;
  overflow: auto;
  padding: 12px 16px;
  background: var(--surface);
  color: var(--text2);
  font-size: 12px;
  line-height: 1.55;
}
.ll-info-scroll h3 { font-size: 13px; font-weight: 700; color: var(--text); margin: 0 0 6px; }
.ll-info-scroll h3:not(:first-child) { margin-top: 14px; }
.ll-info-scroll h4 { font-size: 12px; font-weight: 700; color: var(--text); margin: 10px 0 4px; }
.ll-info-scroll p { margin: 0 0 6px; }
.ll-info-scroll ul { margin: 0 0 10px 16px; padding: 0; }
.ll-info-scroll li { margin-bottom: 3px; }
.ll-info-scroll code {
  font-family: Consolas, monospace;
  font-size: 11px;
  background: var(--surface2);
  padding: 1px 4px;
  border-radius: 4px;
  border: 1px solid var(--border);
  color: var(--coral-dark);
}

.ll-complexity-table {
  width: 100%;
  border-collapse: collapse;
  margin-bottom: 10px;
  font-size: 11.5px;
}
.ll-complexity-table th, .ll-complexity-table td {
  border: 1px solid var(--border);
  padding: 6px 8px;
  text-align: left;
}
.ll-complexity-table th { background: var(--surface2); color: var(--text); font-weight: 700; }
.ll-complexity-table td:nth-child(2), .ll-complexity-table td:nth-child(3) { font-family: Consolas, monospace; }

.ll-math-box {
  background: var(--surface2);
  border: 1px solid var(--border2);
  border-radius: var(--radius-sm);
  padding: 6px 10px;
  font-family: Consolas, monospace;
  font-weight: 700;
  color: #1e293b;
  margin-bottom: 6px;
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

.ll-footer {
  display: flex;
  align-items: center;
  padding: 3px 12px;
  background: var(--surface);
  border-top: 1px solid var(--border);
  font-size: 10.5px;
  color: var(--muted);
  flex-shrink: 0;
}

.ll-speed-wrap {
  display: flex;
  align-items: center;
  gap: 4px;
  margin-left: 14px;
}
.ll-speed-wrap input[type=range] {
  width: 80px;
  accent-color: var(--coral);
}
</style>
