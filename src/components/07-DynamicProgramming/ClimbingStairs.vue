<script setup>
import { ref, computed, reactive, onMounted, onUnmounted, nextTick } from 'vue';

/* ==================================================================== */
/* PROPS & COMPONENT CONFIGURATION                                      */
/* ==================================================================== */
const props = defineProps({
  topic: { type: String, default: 'Dynamic Programming' },
  subTopic: { type: String, default: 'Climbing Stairs (Recursion vs DP)' }
});

/* ==================================================================== */
/* APPROACHES & MULTI-LANGUAGE 100% EXECUTABLE CODE SPECIFICATIONS      */
/* ==================================================================== */
const APPROACHES = [
  { id: 'recursion', label: 'Brute Force', desc: 'O(2^n) Exponential — Evaluates Overlapping Jump Branches' },
  { id: 'memoization', label: 'Memoization', desc: 'O(n) Linear Time & O(n) Space — Top-Down with Cache' },
  { id: 'tabulation', label: 'Tabulation', desc: 'O(n) Linear Time & O(n) Space — Bottom-Up 1D Array' }
];

const CODES = {
  recursion: {
    java: [
      ['',             'import java.util.Scanner;'],
      ['',             ''],
      ['',             'public class Main {'],
      ['c_entry',      '    static int climbStairs(int n) {'],
      ['c_base_check', '        if (n <= 2) {'],
      ['c_ret_base',   '            return n;'],
      ['',             '        }'],
      ['c_rec_one',    '        int oneStep = climbStairs(n - 1);'],
      ['c_rec_two',    '        int twoStep = climbStairs(n - 2);'],
      ['c_ret_sum',    '        return oneStep + twoStep;'],
      ['',             '    }'],
      ['',             ''],
      ['',             '    public static void main(String[] args) {'],
      ['m_scanner',    '        Scanner sc = new Scanner(System.in);'],
      ['m_read_n',     '        int n = sc.nextInt();'],
      ['m_call_climb', '        int ways = climbStairs(n);'],
      ['m_print',      '        System.out.println(ways);'],
      ['m_done',       '    }'],
      ['',             '}']
    ],
    c: [
      ['',             '#include <stdio.h>'],
      ['',             ''],
      ['c_entry',      'int climbStairs(int n) {'],
      ['c_base_check', '    if (n <= 2) {'],
      ['c_ret_base',   '        return n;'],
      ['',             '    }'],
      ['c_rec_one',    '    int oneStep = climbStairs(n - 1);'],
      ['c_rec_two',    '    int twoStep = climbStairs(n - 2);'],
      ['c_ret_sum',    '    return oneStep + twoStep;'],
      ['',             '}'],
      ['',             ''],
      ['',             'int main() {'],
      ['m_scanner',    '    int n;'],
      ['m_read_n',     '    scanf("%d", &n);'],
      ['m_call_climb', '    int ways = climbStairs(n);'],
      ['m_print',      '    printf("%d\\n", ways);'],
      ['m_done',       '    return 0;'],
      ['',             '}']
    ],
    cpp: [
      ['',             '#include <iostream>'],
      ['',             'using namespace std;'],
      ['',             ''],
      ['c_entry',      'int climbStairs(int n) {'],
      ['c_base_check', '    if (n <= 2) {'],
      ['c_ret_base',   '        return n;'],
      ['',             '    }'],
      ['c_rec_one',    '    int oneStep = climbStairs(n - 1);'],
      ['c_rec_two',    '    int twoStep = climbStairs(n - 2);'],
      ['c_ret_sum',    '    return oneStep + twoStep;'],
      ['',             '}'],
      ['',             ''],
      ['',             'int main() {'],
      ['m_scanner',    '    int n;'],
      ['m_read_n',     '    cin >> n;'],
      ['m_call_climb', '    int ways = climbStairs(n);'],
      ['m_print',      '    cout << ways << endl;'],
      ['m_done',       '    return 0;'],
      ['',             '}']
    ],
    python: [
      ['',             'import sys'],
      ['',             ''],
      ['c_entry',      'def climb_stairs(n):'],
      ['c_base_check', '    if n <= 2:'],
      ['c_ret_base',   '        return n'],
      ['c_rec_one',    '    one_step = climb_stairs(n - 1)'],
      ['c_rec_two',    '    two_step = climb_stairs(n - 2)'],
      ['c_ret_sum',    '    return one_step + two_step'],
      ['',             ''],
      ['',             'if __name__ == "__main__":'],
      ['m_read_n',     '    n = int(input())'],
      ['m_call_climb', '    ways = climb_stairs(n)'],
      ['m_print',      '    print(ways)'],
      ['m_done',       '    sys.exit(0)']
    ],
    javascript: [
      ['',             'const readline = require("readline");'],
      ['',             ''],
      ['c_entry',      'function climbStairs(n) {'],
      ['c_base_check', '    if (n <= 2) {'],
      ['c_ret_base',   '        return n;'],
      ['',             '    }'],
      ['c_rec_one',    '    const oneStep = climbStairs(n - 1);'],
      ['c_rec_two',    '    const twoStep = climbStairs(n - 2);'],
      ['c_ret_sum',    '    return oneStep + twoStep;'],
      ['',             '}'],
      ['',             ''],
      ['m_scanner',    'const rl = readline.createInterface({ input: process.stdin, output: process.stdout });'],
      ['m_read_n',     'rl.on("line", (line) => {'],
      ['',             '    const n = parseInt(line.trim(), 10);'],
      ['m_call_climb', '    const ways = climbStairs(n);'],
      ['m_print',      '    console.log(ways);'],
      ['m_done',       '    process.exit(0);'],
      ['',             '});']
    ]
  },
  memoization: {
    java: [
      ['',             'import java.util.Scanner;'],
      ['',             'import java.util.Arrays;'],
      ['',             ''],
      ['',             'public class Main {'],
      ['c_entry',      '    static int solve(int n, int[] memo) {'],
      ['c_base_check', '        if (n <= 2) {'],
      ['c_ret_base',   '            return n;'],
      ['',             '        }'],
      ['c_memo_check', '        if (memo[n] != -1) {'],
      ['c_ret_memo',   '            return memo[n];'],
      ['',             '        }'],
      ['c_rec_one',    '        int oneStep = solve(n - 1, memo);'],
      ['c_rec_two',    '        int twoStep = solve(n - 2, memo);'],
      ['c_store_memo', '        memo[n] = oneStep + twoStep;'],
      ['c_ret_sum',    '        return memo[n];'],
      ['',             '    }'],
      ['',             ''],
      ['',             '    public static void main(String[] args) {'],
      ['m_scanner',    '        Scanner sc = new Scanner(System.in);'],
      ['m_read_n',     '        int n = sc.nextInt();'],
      ['m_alloc_memo', '        int[] memo = new int[n + 1];'],
      ['m_fill_memo',  '        Arrays.fill(memo, -1);'],
      ['m_call_climb', '        int ways = solve(n, memo);'],
      ['m_print',      '        System.out.println(ways);'],
      ['m_done',       '    }'],
      ['',             '}']
    ],
    c: [
      ['',             '#include <stdio.h>'],
      ['',             '#include <string.h>'],
      ['',             ''],
      ['c_entry',      'int solve(int n, int memo[]) {'],
      ['c_base_check', '    if (n <= 2) {'],
      ['c_ret_base',   '        return n;'],
      ['',             '    }'],
      ['c_memo_check', '    if (memo[n] != -1) {'],
      ['c_ret_memo',   '        return memo[n];'],
      ['',             '    }'],
      ['c_rec_one',    '    int oneStep = solve(n - 1, memo);'],
      ['c_rec_two',    '    int twoStep = solve(n - 2, memo);'],
      ['c_store_memo', '    memo[n] = oneStep + twoStep;'],
      ['c_ret_sum',    '    return memo[n];'],
      ['',             '}'],
      ['',             ''],
      ['',             'int main() {'],
      ['m_scanner',    '    int n;'],
      ['m_read_n',     '    scanf("%d", &n);'],
      ['m_alloc_memo', '    int memo[n + 1];'],
      ['m_fill_memo',  '    memset(memo, -1, sizeof(memo));'],
      ['m_call_climb', '    int ways = solve(n, memo);'],
      ['m_print',      '    printf("%d\\n", ways);'],
      ['m_done',       '    return 0;'],
      ['',             '}']
    ],
    cpp: [
      ['',             '#include <iostream>'],
      ['',             '#include <vector>'],
      ['',             'using namespace std;'],
      ['',             ''],
      ['c_entry',      'int solve(int n, vector<int>& memo) {'],
      ['c_base_check', '    if (n <= 2) {'],
      ['c_ret_base',   '        return n;'],
      ['',             '    }'],
      ['c_memo_check', '    if (memo[n] != -1) {'],
      ['c_ret_memo',   '        return memo[n];'],
      ['',             '    }'],
      ['c_rec_one',    '    int oneStep = solve(n - 1, memo);'],
      ['c_rec_two',    '    int twoStep = solve(n - 2, memo);'],
      ['c_store_memo', '    memo[n] = oneStep + twoStep;'],
      ['c_ret_sum',    '    return memo[n];'],
      ['',             '}'],
      ['',             ''],
      ['',             'int main() {'],
      ['m_scanner',    '    int n;'],
      ['m_read_n',     '    cin >> n;'],
      ['m_alloc_memo', '    vector<int> memo(n + 1, -1);'],
      ['m_fill_memo',  '    // memo initialized with -1'],
      ['m_call_climb', '    int ways = solve(n, memo);'],
      ['m_print',      '    cout << ways << endl;'],
      ['m_done',       '    return 0;'],
      ['',             '}']
    ],
    python: [
      ['',             'import sys'],
      ['',             ''],
      ['c_entry',      'def solve(n, memo):'],
      ['c_base_check', '    if n <= 2:'],
      ['c_ret_base',   '        return n'],
      ['c_memo_check', '    if memo[n] != -1:'],
      ['c_ret_memo',   '        return memo[n]'],
      ['c_rec_one',    '    one_step = solve(n - 1, memo)'],
      ['c_rec_two',    '    two_step = solve(n - 2, memo)'],
      ['c_store_memo', '    memo[n] = one_step + two_step'],
      ['c_ret_sum',    '    return memo[n]'],
      ['',             ''],
      ['',             'if __name__ == "__main__":'],
      ['m_read_n',     '    n = int(input())'],
      ['m_alloc_memo', '    memo = [-1] * (n + 1)'],
      ['m_call_climb', '    ways = solve(n, memo)'],
      ['m_print',      '    print(ways)'],
      ['m_done',       '    sys.exit(0)']
    ],
    javascript: [
      ['',             'const readline = require("readline");'],
      ['',             ''],
      ['c_entry',      'function solve(n, memo) {'],
      ['c_base_check', '    if (n <= 2) {'],
      ['c_ret_base',   '        return n;'],
      ['',             '    }'],
      ['c_memo_check', '    if (memo[n] !== -1) {'],
      ['c_ret_memo',   '        return memo[n];'],
      ['',             '    }'],
      ['c_rec_one',    '    const oneStep = solve(n - 1, memo);'],
      ['c_rec_two',    '    const twoStep = solve(n - 2, memo);'],
      ['c_store_memo', '    memo[n] = oneStep + twoStep;'],
      ['c_ret_sum',    '    return memo[n];'],
      ['',             '}'],
      ['',             ''],
      ['m_scanner',    'const rl = readline.createInterface({ input: process.stdin, output: process.stdout });'],
      ['m_read_n',     'rl.on("line", (line) => {'],
      ['',             '    const n = parseInt(line.trim(), 10);'],
      ['m_alloc_memo', '    const memo = new Array(n + 1).fill(-1);'],
      ['m_call_climb', '    const ways = solve(n, memo);'],
      ['m_print',      '    console.log(ways);'],
      ['m_done',       '    process.exit(0);'],
      ['',             '});']
    ]
  },
  tabulation: {
    java: [
      ['',             'import java.util.Scanner;'],
      ['',             ''],
      ['',             'public class Main {'],
      ['c_entry',      '    static int climbStairs(int n) {'],
      ['c_base_check', '        if (n <= 2) {'],
      ['c_ret_base',   '            return n;'],
      ['',             '        }'],
      ['c_alloc_dp',   '        int[] dp = new int[n + 1];'],
      ['c_init_base1', '        dp[1] = 1;'],
      ['c_init_base2', '        dp[2] = 2;'],
      ['c_for_loop',   '        for (int i = 3; i <= n; i++) {'],
      ['c_dp_trans',   '            dp[i] = dp[i - 1] + dp[i - 2];'],
      ['',             '        }'],
      ['c_ret_dpn',    '        return dp[n];'],
      ['',             '    }'],
      ['',             ''],
      ['',             '    public static void main(String[] args) {'],
      ['m_scanner',    '        Scanner sc = new Scanner(System.in);'],
      ['m_read_n',     '        int n = sc.nextInt();'],
      ['m_call_climb', '        int ways = climbStairs(n);'],
      ['m_print',      '        System.out.println(ways);'],
      ['m_done',       '    }'],
      ['',             '}']
    ],
    c: [
      ['',             '#include <stdio.h>'],
      ['',             ''],
      ['c_entry',      'int climbStairs(int n) {'],
      ['c_base_check', '    if (n <= 2) {'],
      ['c_ret_base',   '        return n;'],
      ['',             '    }'],
      ['c_alloc_dp',   '    int dp[n + 1];'],
      ['c_init_base1', '    dp[1] = 1;'],
      ['c_init_base2', '    dp[2] = 2;'],
      ['c_for_loop',   '    for (int i = 3; i <= n; i++) {'],
      ['c_dp_trans',   '        dp[i] = dp[i - 1] + dp[i - 2];'],
      ['',             '    }'],
      ['c_ret_dpn',    '    return dp[n];'],
      ['',             '}'],
      ['',             ''],
      ['',             'int main() {'],
      ['m_scanner',    '    int n;'],
      ['m_read_n',     '    scanf("%d", &n);'],
      ['m_call_climb', '    int ways = climbStairs(n);'],
      ['m_print',      '    printf("%d\\n", ways);'],
      ['m_done',       '    return 0;'],
      ['',             '}']
    ],
    cpp: [
      ['',             '#include <iostream>'],
      ['',             '#include <vector>'],
      ['',             'using namespace std;'],
      ['',             ''],
      ['c_entry',      'int climbStairs(int n) {'],
      ['c_base_check', '    if (n <= 2) {'],
      ['c_ret_base',   '        return n;'],
      ['',             '    }'],
      ['c_alloc_dp',   '    vector<int> dp(n + 1);'],
      ['c_init_base1', '    dp[1] = 1;'],
      ['c_init_base2', '    dp[2] = 2;'],
      ['c_for_loop',   '    for (int i = 3; i <= n; i++) {'],
      ['c_dp_trans',   '        dp[i] = dp[i - 1] + dp[i - 2];'],
      ['',             '    }'],
      ['c_ret_dpn',    '    return dp[n];'],
      ['',             '}'],
      ['',             ''],
      ['',             'int main() {'],
      ['m_scanner',    '    int n;'],
      ['m_read_n',     '    cin >> n;'],
      ['m_call_climb', '    int ways = climbStairs(n);'],
      ['m_print',      '    cout << ways << endl;'],
      ['m_done',       '    return 0;'],
      ['',             '}']
    ],
    python: [
      ['',             'import sys'],
      ['',             ''],
      ['c_entry',      'def climb_stairs(n):'],
      ['c_base_check', '    if n <= 2:'],
      ['c_ret_base',   '        return n'],
      ['c_alloc_dp',   '    dp = [0] * (n + 1)'],
      ['c_init_base1', '    dp[1] = 1'],
      ['c_init_base2', '    dp[2] = 2'],
      ['c_for_loop',   '    for i in range(3, n + 1):'],
      ['c_dp_trans',   '        dp[i] = dp[i - 1] + dp[i - 2]'],
      ['c_ret_dpn',    '    return dp[n]'],
      ['',             ''],
      ['',             'if __name__ == "__main__":'],
      ['m_read_n',     '    n = int(input())'],
      ['m_call_climb', '    ways = climb_stairs(n)'],
      ['m_print',      '    print(ways)'],
      ['m_done',       '    sys.exit(0)']
    ],
    javascript: [
      ['',             'const readline = require("readline");'],
      ['',             ''],
      ['c_entry',      'function climbStairs(n) {'],
      ['c_base_check', '    if (n <= 2) {'],
      ['c_ret_base',   '        return n;'],
      ['',             '    }'],
      ['c_alloc_dp',   '    const dp = new Array(n + 1).fill(0);'],
      ['c_init_base1', '    dp[1] = 1;'],
      ['c_init_base2', '    dp[2] = 2;'],
      ['c_for_loop',   '    for (let i = 3; i <= n; i++) {'],
      ['c_dp_trans',   '        dp[i] = dp[i - 1] + dp[i - 2];'],
      ['',             '    }'],
      ['c_ret_dpn',    '    return dp[n];'],
      ['',             '}'],
      ['',             ''],
      ['m_scanner',    'const rl = readline.createInterface({ input: process.stdin, output: process.stdout });'],
      ['m_read_n',     'rl.on("line", (line) => {'],
      ['',             '    const n = parseInt(line.trim(), 10);'],
      ['m_call_climb', '    const ways = climbStairs(n);'],
      ['m_print',      '    console.log(ways);'],
      ['m_done',       '    process.exit(0);'],
      ['',             '});']
    ]
  }
};

const PSEUDOCODES = {
  recursion: [
    'Algorithm ClimbStairs_Recursive(n):',
    '  1. If n <= 2: Return n',
    '  2. oneStep = ClimbStairs_Recursive(n - 1)',
    '  3. twoStep = ClimbStairs_Recursive(n - 2)',
    '  4. Return oneStep + twoStep',
    '',
    'Time Complexity:  O(2^n) Exponential (Duplicates identical subproblems)',
    'Space Complexity: O(n) Maximum Call Stack Depth'
  ],
  memoization: [
    'Algorithm ClimbStairs_Memo(n, memo):',
    '  1. If n <= 2: Return n',
    '  2. If memo[n] != -1: Return memo[n]   // Cache Hit in O(1)',
    '  3. oneStep = ClimbStairs_Memo(n - 1, memo)',
    '  4. twoStep = ClimbStairs_Memo(n - 2, memo)',
    '  5. memo[n] = oneStep + twoStep',
    '  6. Return memo[n]',
    '',
    'Time Complexity:  O(n) Linear Time (Each step solved exactly once)',
    'Space Complexity: O(n) Memo Table + Call Stack'
  ],
  tabulation: [
    'Algorithm ClimbStairs_Tabulation(n):',
    '  1. If n <= 2: Return n',
    '  2. Initialize 1D array dp[1...n]',
    '  3. Base cases: dp[1] = 1, dp[2] = 2',
    '  4. For i = 3 to n:',
    '       dp[i] = dp[i - 1] + dp[i - 2]   // 1-step jump + 2-step jump',
    '  5. Return dp[n]',
    '',
    'Time Complexity:  O(n) Single Linear Pass',
    'Space Complexity: O(n) Table (Can be optimized to O(1) with two variables)'
  ]
};

/* ==================================================================== */
/* STATIC TREE GENERATOR FOR RECURSION & MEMOIZATION                    */
/* ==================================================================== */
function generateStaticTree(approach, n) {
  const nodes = [];
  const edges = [];
  let nextId = 0;

  if (approach === 'recursion') {
    function build(k, parentId, isLeft) {
      const id = nextId++;
      const node = { id, k, parentId, isLeft, isRedundant: false, children: [] };
      nodes.push(node);

      if (parentId !== null) {
        edges.push({ from: parentId, to: id, isLeft });
      }

      if (k > 2) {
        const leftId = build(k - 1, id, true);
        const rightId = build(k - 2, id, false);
        node.children = [leftId, rightId];
      }
      return id;
    }
    build(n, null, false);
  } else if (approach === 'memoization') {
    const memo = new Array(n + 1).fill(-1);
    function buildMemo(k, parentId, isLeft) {
      const id = nextId++;
      const isCacheHit = memo[k] !== -1;
      const node = { id, k, parentId, isLeft, isCacheHit, children: [] };
      nodes.push(node);

      if (parentId !== null) {
        edges.push({ from: parentId, to: id, isLeft });
      }

      if (k <= 2) {
        memo[k] = k;
      } else if (isCacheHit) {
        // Pruned subtree — no children spawned!
      } else {
        const leftId = buildMemo(k - 1, id, true);
        const rightId = buildMemo(k - 2, id, false);
        node.children = [leftId, rightId];
        memo[k] = 1;
      }
      return id;
    }
    buildMemo(n, null, false);
  }

  // Compute Layout Positions (x, y)
  const nodeMap = new Map();
  nodes.forEach(nd => nodeMap.set(nd.id, { ...nd }));

  let leafCounter = 0;
  function assignSubtreeMetrics(nId, depth = 0) {
    const nd = nodeMap.get(nId);
    if (!nd) return;
    nd.depth = depth;
    if (!nd.children || !nd.children.length) {
      nd.leafIndex = leafCounter++;
    } else {
      nd.children.forEach(cId => assignSubtreeMetrics(cId, depth + 1));
    }
  }
  if (nodes.length > 0) {
    assignSubtreeMetrics(0, 0);
  }

  const totalLeaves = Math.max(1, leafCounter);
  const maxDepth = Math.max(0, ...Array.from(nodeMap.values()).map(nd => nd.depth || 0));

  // Balanced spacing with clear gaps between containers
  const leafSpacing = 58;
  const levelHeight = 44;

  const computedWidth = Math.max(140, totalLeaves * leafSpacing + 30);
  const computedHeight = Math.max(70, maxDepth * levelHeight + 40);

  function assignCoords(nId) {
    const nd = nodeMap.get(nId);
    if (!nd) return 0;
    if (!nd.children || !nd.children.length) {
      nd.x = 15 + nd.leafIndex * leafSpacing + leafSpacing / 2;
    } else {
      const childXs = nd.children.map(cId => assignCoords(cId));
      nd.x = (childXs[0] + childXs[childXs.length - 1]) / 2;
    }
    nd.y = 24 + nd.depth * levelHeight;
    return nd.x;
  }
  if (nodes.length > 0) {
    assignCoords(0);
  }

  const layoutMap = {};
  nodeMap.forEach((nd, id) => {
    layoutMap[id] = { x: nd.x, y: nd.y, depth: nd.depth };
  });

  return { nodes, edges, layoutMap, width: computedWidth, height: computedHeight };
}

/* ==================================================================== */
/* STRICT ZERO-SKIP LINE-BY-LINE EXECUTION STEP GENERATOR               */
/* ==================================================================== */
function buildSteps(approach, targetN) {
  const steps = [];
  const maxLimit = approach === 'tabulation' ? 20 : 5;
  const rawN = Math.max(1, parseInt(targetN, 10) || 1);
  const n = Math.min(rawN, maxLimit);

  /* ------------------------------------------------------------------ */
  /* APPROACH 1: BRUTE FORCE RECURSION                                   */
  /* ------------------------------------------------------------------ */
  if (approach === 'recursion') {
    const staticTree = generateStaticTree('recursion', n);
    const nodeStateMap = {};
    staticTree.nodes.forEach(nd => {
      nodeStateMap[nd.id] = {
        ...nd,
        x: staticTree.layoutMap[nd.id].x,
        y: staticTree.layoutMap[nd.id].y,
        state: 'hidden', // hidden -> active -> solved
        retVal: null
      };
    });

    const stackFrames = [];
    let nextNodeId = 0;
    let callCounter = 0;
    let redundantCounter = 0;
    const computedValuesMap = {};

    function getVisibleNodes() {
      return Object.values(nodeStateMap)
        .filter(nd => nd.state !== 'hidden')
        .map(nd => ({ ...nd }));
    }
    function getVisibleEdges() {
      return staticTree.edges.filter(e => {
        const fromNd = nodeStateMap[e.from];
        const toNd = nodeStateMap[e.to];
        return fromNd && toNd && fromNd.state !== 'hidden' && toNd.state !== 'hidden';
      }).map(e => ({
        ...e,
        x1: staticTree.layoutMap[e.from].x,
        y1: staticTree.layoutMap[e.from].y + 12,
        x2: staticTree.layoutMap[e.to].x,
        y2: staticTree.layoutMap[e.to].y - 12
      }));
    }

    // Step 0: Input ingestion
    steps.push({
      code: 'm_read_n',
      badge: `Read user input: n = ${n} staircase steps`,
      badgeType: 'info',
      state: {
        n,
        curN: n,
        totalCalls: 0,
        redundantCalls: 0,
        currentReturn: null,
        activeNodeId: null,
        frames: [{ name: 'main()', args: `n=${n}` }],
        treeNodes: getVisibleNodes(),
        treeEdges: getVisibleEdges(),
        treeWidth: staticTree.width,
        treeHeight: staticTree.height
      }
    });

    // Step 1: Call climbStairs(n)
    steps.push({
      code: 'm_call_climb',
      badge: `main() calls climbStairs(n = ${n})`,
      badgeType: 'info',
      state: {
        n,
        curN: n,
        totalCalls: 0,
        redundantCalls: 0,
        currentReturn: null,
        activeNodeId: null,
        frames: [{ name: 'main()', args: `n=${n}` }],
        treeNodes: getVisibleNodes(),
        treeEdges: getVisibleEdges(),
        treeWidth: staticTree.width,
        treeHeight: staticTree.height
      }
    });

    function recurse(k) {
      const myNodeId = nextNodeId++;
      callCounter++;
      const isDuplicate = (computedValuesMap[k] !== undefined);
      if (isDuplicate) redundantCounter++;

      nodeStateMap[myNodeId].state = 'active';
      nodeStateMap[myNodeId].isRedundant = isDuplicate;

      stackFrames.push({ name: `climbStairs(n = ${k})`, args: `n = ${k}` });

      // Step: Function entry
      steps.push({
        code: 'c_entry',
        badge: isDuplicate
          ? `[DUPLICATE SUBPROBLEM] Invoking climbStairs(n = ${k}). Notice: climbStairs(${k}) was already evaluated earlier!`
          : `Entering climbStairs(n = ${k}) [Call #${callCounter}]`,
        badgeType: isDuplicate ? 'warn' : 'info',
        state: {
          n,
          curN: k,
          totalCalls: callCounter,
          redundantCalls: redundantCounter,
          currentReturn: null,
          activeNodeId: myNodeId,
          frames: [...stackFrames],
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height
        }
      });

      // Step: Base condition check
      steps.push({
        code: 'c_base_check',
        badge: `Check base case: (n <= 2) &rarr; ${k} <= 2 is ${k <= 2 ? 'TRUE (Base Case Reached)' : 'FALSE (Needs Subproblem Evaluation)'}`,
        badgeType: 'info',
        state: {
          n,
          curN: k,
          totalCalls: callCounter,
          redundantCalls: redundantCounter,
          currentReturn: null,
          activeNodeId: myNodeId,
          frames: [...stackFrames],
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height
        }
      });

      if (k <= 2) {
        const ret = k;
        nodeStateMap[myNodeId].state = 'solved';
        nodeStateMap[myNodeId].retVal = ret;
        computedValuesMap[k] = ret;

        steps.push({
          code: 'c_ret_base',
          badge: `Base case matched: climbStairs(${k}) = ${ret} ways. Returning ${ret} directly.`,
          badgeType: 'success',
          state: {
            n,
            curN: k,
            totalCalls: callCounter,
            redundantCalls: redundantCounter,
            currentReturn: ret,
            activeNodeId: myNodeId,
            frames: [...stackFrames],
            treeNodes: getVisibleNodes(),
            treeEdges: getVisibleEdges(),
            treeWidth: staticTree.width,
            treeHeight: staticTree.height
          }
        });

        stackFrames.pop();
        return ret;
      }

      // 1-step jump branch (left)
      steps.push({
        code: 'c_rec_one',
        badge: `climbStairs(${k}) evaluates 1-step jump: spawning branch climbStairs(n - 1 = ${k - 1})`,
        badgeType: 'info',
        state: {
          n,
          curN: k,
          totalCalls: callCounter,
          redundantCalls: redundantCounter,
          currentReturn: null,
          activeNodeId: myNodeId,
          frames: [...stackFrames],
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height
        }
      });

      const oneStepVal = recurse(k - 1);

      // 2-step jump branch (right)
      steps.push({
        code: 'c_rec_two',
        badge: `climbStairs(${k}) evaluates 2-step jump: spawning branch climbStairs(n - 2 = ${k - 2})`,
        badgeType: 'info',
        state: {
          n,
          curN: k,
          totalCalls: callCounter,
          redundantCalls: redundantCounter,
          currentReturn: null,
          activeNodeId: myNodeId,
          frames: [...stackFrames],
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height
        }
      });

      const twoStepVal = recurse(k - 2);

      const sumVal = oneStepVal + twoStepVal;
      nodeStateMap[myNodeId].state = 'solved';
      nodeStateMap[myNodeId].retVal = sumVal;
      computedValuesMap[k] = sumVal;

      steps.push({
        code: 'c_ret_sum',
        badge: `climbStairs(${k}) = (1-step: ${oneStepVal}) + (2-step: ${twoStepVal}) = ${sumVal} total distinct ways. Returning to caller.`,
        badgeType: 'success',
        state: {
          n,
          curN: k,
          totalCalls: callCounter,
          redundantCalls: redundantCounter,
          currentReturn: sumVal,
          activeNodeId: myNodeId,
          frames: [...stackFrames],
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height
        }
      });

      stackFrames.pop();
      return sumVal;
    }

    const finalAnswer = recurse(n);

    // Print & Done
    steps.push({
      code: 'm_print',
      badge: `Output Result: Print total distinct ways = ${finalAnswer}`,
      badgeType: 'success',
      state: {
        n,
        totalCalls: callCounter,
        redundantCalls: redundantCounter,
        currentReturn: finalAnswer,
        activeNodeId: 0,
        frames: [{ name: 'main()', args: `n=${n}, ways=${finalAnswer}` }],
        treeNodes: getVisibleNodes(),
        treeEdges: getVisibleEdges(),
        treeWidth: staticTree.width,
        treeHeight: staticTree.height
      }
    });

    steps.push({
      code: 'm_done',
      badge: `Execution Complete! Total distinct ways to climb ${n} stairs: ${finalAnswer}. (Notice ${redundantCounter} duplicate redundant calls occurred!)`,
      badgeType: 'success',
      state: {
        n,
        totalCalls: callCounter,
        redundantCalls: redundantCounter,
        currentReturn: finalAnswer,
        activeNodeId: null,
        frames: [{ name: 'main()', args: `n=${n}, ways=${finalAnswer}` }],
        treeNodes: getVisibleNodes(),
        treeEdges: getVisibleEdges(),
        treeWidth: staticTree.width,
        treeHeight: staticTree.height
      }
    });
  }

  /* ------------------------------------------------------------------ */
  /* APPROACH 2: DP MEMOIZATION (TOP-DOWN)                             */
  /* ------------------------------------------------------------------ */
  else if (approach === 'memoization') {
    const staticTree = generateStaticTree('memoization', n);
    const nodeStateMap = {};
    staticTree.nodes.forEach(nd => {
      nodeStateMap[nd.id] = {
        ...nd,
        x: staticTree.layoutMap[nd.id].x,
        y: staticTree.layoutMap[nd.id].y,
        state: 'hidden',
        retVal: null
      };
    });

    const memoArray = new Array(n + 1).fill(-1);
    const stackFrames = [];
    let nextNodeId = 0;
    let callCounter = 0;
    let cacheHits = 0;

    function getVisibleNodes() {
      return Object.values(nodeStateMap)
        .filter(nd => nd.state !== 'hidden')
        .map(nd => ({ ...nd }));
    }
    function getVisibleEdges() {
      return staticTree.edges.filter(e => {
        const fromNd = nodeStateMap[e.from];
        const toNd = nodeStateMap[e.to];
        return fromNd && toNd && fromNd.state !== 'hidden' && toNd.state !== 'hidden';
      }).map(e => ({
        ...e,
        x1: staticTree.layoutMap[e.from].x,
        y1: staticTree.layoutMap[e.from].y + 12,
        x2: staticTree.layoutMap[e.to].x,
        y2: staticTree.layoutMap[e.to].y - 12
      }));
    }

    // Step 0: Input ingestion
    steps.push({
      code: 'm_read_n',
      badge: `Read user input: n = ${n} staircase steps`,
      badgeType: 'info',
      state: {
        n,
        curN: n,
        totalCalls: 0,
        cacheHits: 0,
        currentReturn: null,
        activeNodeId: null,
        memo: [...memoArray],
        frames: [{ name: 'main()', args: `n=${n}` }],
        treeNodes: getVisibleNodes(),
        treeEdges: getVisibleEdges(),
        treeWidth: staticTree.width,
        treeHeight: staticTree.height
      }
    });

    // Step 1: Allocate memo
    steps.push({
      code: 'm_alloc_memo',
      badge: `Allocate lookup table memo[0...${n}] of size ${n + 1}`,
      badgeType: 'info',
      state: {
        n,
        curN: n,
        totalCalls: 0,
        cacheHits: 0,
        currentReturn: null,
        activeNodeId: null,
        memo: [...memoArray],
        frames: [{ name: 'main()', args: `n=${n}` }],
        treeNodes: getVisibleNodes(),
        treeEdges: getVisibleEdges(),
        treeWidth: staticTree.width,
        treeHeight: staticTree.height
      }
    });

    // Step 2: Initialize memo with -1
    steps.push({
      code: 'm_fill_memo',
      badge: `Initialize all cache entries in memo table to -1 (indicating uncalculated state)`,
      badgeType: 'info',
      state: {
        n,
        curN: n,
        totalCalls: 0,
        cacheHits: 0,
        currentReturn: null,
        activeNodeId: null,
        memo: [...memoArray],
        frames: [{ name: 'main()', args: `n=${n}` }],
        treeNodes: getVisibleNodes(),
        treeEdges: getVisibleEdges(),
        treeWidth: staticTree.width,
        treeHeight: staticTree.height
      }
    });

    function recurseMemo(k) {
      const myNodeId = nextNodeId++;
      callCounter++;
      nodeStateMap[myNodeId].state = 'active';

      stackFrames.push({ name: `solve(n = ${k})`, args: `n = ${k}` });

      // Step: Function entry
      steps.push({
        code: 'c_entry',
        badge: `Entering solve(n = ${k}, memo) [Call #${callCounter}]`,
        badgeType: 'info',
        state: {
          n,
          curN: k,
          totalCalls: callCounter,
          cacheHits,
          currentReturn: null,
          activeNodeId: myNodeId,
          memo: [...memoArray],
          frames: [...stackFrames],
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height
        }
      });

      // Step: Base condition
      steps.push({
        code: 'c_base_check',
        badge: `Check base case: ${k} <= 2 &rarr; ${k <= 2 ? 'TRUE (Base Case Reached)' : 'FALSE'}`,
        badgeType: 'info',
        state: {
          n,
          curN: k,
          totalCalls: callCounter,
          cacheHits,
          currentReturn: null,
          activeNodeId: myNodeId,
          memo: [...memoArray],
          frames: [...stackFrames],
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height
        }
      });

      if (k <= 2) {
        const ret = k;
        nodeStateMap[myNodeId].state = 'solved';
        nodeStateMap[myNodeId].retVal = ret;
        memoArray[k] = ret;

        steps.push({
          code: 'c_ret_base',
          badge: `Base step: climbStairs(${k}) = ${ret} ways. Returning ${ret} directly.`,
          badgeType: 'success',
          state: {
            n,
            curN: k,
            totalCalls: callCounter,
            cacheHits,
            currentReturn: ret,
            activeNodeId: myNodeId,
            memo: [...memoArray],
            frames: [...stackFrames],
            treeNodes: getVisibleNodes(),
            treeEdges: getVisibleEdges(),
            treeWidth: staticTree.width,
            treeHeight: staticTree.height
          }
        });

        stackFrames.pop();
        return ret;
      }

      // Step: Cache check
      steps.push({
        code: 'c_memo_check',
        badge: `Checking lookup table memo[${k}] != -1 &rarr; memo[${k}] is ${memoArray[k] !== -1 ? `${memoArray[k]} (CACHE HIT!)` : '-1 (CACHE MISS)'}`,
        badgeType: memoArray[k] !== -1 ? 'success' : 'info',
        state: {
          n,
          curN: k,
          totalCalls: callCounter,
          cacheHits,
          currentReturn: null,
          activeNodeId: myNodeId,
          memo: [...memoArray],
          frames: [...stackFrames],
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height
        }
      });

      if (memoArray[k] !== -1) {
        cacheHits++;
        const hitVal = memoArray[k];
        nodeStateMap[myNodeId].state = 'cache-hit';
        nodeStateMap[myNodeId].retVal = hitVal;

        steps.push({
          code: 'c_ret_memo',
          badge: `[CACHE HIT] memo[${k}] = ${hitVal} already cached! Returning in O(1) time without recursive branching!`,
          badgeType: 'success',
          state: {
            n,
            curN: k,
            totalCalls: callCounter,
            cacheHits,
            currentReturn: hitVal,
            activeNodeId: myNodeId,
            memo: [...memoArray],
            frames: [...stackFrames],
            treeNodes: getVisibleNodes(),
            treeEdges: getVisibleEdges(),
            treeWidth: staticTree.width,
            treeHeight: staticTree.height
          }
        });

        stackFrames.pop();
        return hitVal;
      }

      // 1-step jump branch
      steps.push({
        code: 'c_rec_one',
        badge: `solve(${k}) calls 1-step subproblem solve(n - 1 = ${k - 1})`,
        badgeType: 'info',
        state: {
          n,
          curN: k,
          totalCalls: callCounter,
          cacheHits,
          currentReturn: null,
          activeNodeId: myNodeId,
          memo: [...memoArray],
          frames: [...stackFrames],
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height
        }
      });

      const oneStepVal = recurseMemo(k - 1);

      // 2-step jump branch
      steps.push({
        code: 'c_rec_two',
        badge: `solve(${k}) calls 2-step subproblem solve(n - 2 = ${k - 2})`,
        badgeType: 'info',
        state: {
          n,
          curN: k,
          totalCalls: callCounter,
          cacheHits,
          currentReturn: null,
          activeNodeId: myNodeId,
          memo: [...memoArray],
          frames: [...stackFrames],
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height
        }
      });

      const twoStepVal = recurseMemo(k - 2);

      const sumVal = oneStepVal + twoStepVal;
      memoArray[k] = sumVal;

      steps.push({
        code: 'c_store_memo',
        badge: `Store result: memo[${k}] = (1-step: ${oneStepVal}) + (2-step: ${twoStepVal}) = ${sumVal}`,
        badgeType: 'info',
        state: {
          n,
          curN: k,
          totalCalls: callCounter,
          cacheHits,
          currentReturn: sumVal,
          activeNodeId: myNodeId,
          memo: [...memoArray],
          frames: [...stackFrames],
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height
        }
      });

      nodeStateMap[myNodeId].state = 'solved';
      nodeStateMap[myNodeId].retVal = sumVal;

      steps.push({
        code: 'c_ret_sum',
        badge: `Returning memo[${k}] = ${sumVal} to caller`,
        badgeType: 'success',
        state: {
          n,
          curN: k,
          totalCalls: callCounter,
          cacheHits,
          currentReturn: sumVal,
          activeNodeId: myNodeId,
          memo: [...memoArray],
          frames: [...stackFrames],
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height
        }
      });

      stackFrames.pop();
      return sumVal;
    }

    const finalAnswer = recurseMemo(n);

    // Print & Done
    steps.push({
      code: 'm_print',
      badge: `Output Result: Print total distinct ways = ${finalAnswer}`,
      badgeType: 'success',
      state: {
        n,
        curN: n,
        totalCalls: callCounter,
        cacheHits,
        currentReturn: finalAnswer,
        activeNodeId: 0,
        memo: [...memoArray],
        frames: [{ name: 'main()', args: `result=${finalAnswer}` }],
        treeNodes: getVisibleNodes(),
        treeEdges: getVisibleEdges(),
        treeWidth: staticTree.width,
        treeHeight: staticTree.height
      }
    });

    steps.push({
      code: 'm_done',
      badge: `Top-Down DP Memoization Complete! Total ways to climb ${n} stairs: ${finalAnswer}. Cache Hits saved ${cacheHits} expensive subtrees!`,
      badgeType: 'success',
      state: {
        n,
        totalCalls: callCounter,
        cacheHits,
        currentReturn: finalAnswer,
        activeNodeId: null,
        memo: [...memoArray],
        frames: [{ name: 'main()', args: `n=${n}, ways=${finalAnswer}` }],
        treeNodes: getVisibleNodes(),
        treeEdges: getVisibleEdges(),
        treeWidth: staticTree.width,
        treeHeight: staticTree.height
      }
    });
  }

  /* ------------------------------------------------------------------ */
  /* APPROACH 3: DP TABULATION (BOTTOM-UP)                             */
  /* ------------------------------------------------------------------ */
  else if (approach === 'tabulation') {
    const dpArr = new Array(n + 1).fill(null);

    function getDPCells(activeI = -1) {
      return dpArr.map((val, idx) => ({
        idx,
        val: val !== null ? val : '?',
        status: val !== null ? (idx === activeI ? 'active' : (idx <= 2 ? 'base' : 'computed')) : 'uncalculated'
      }));
    }

    // Step 0: Input read
    steps.push({
      code: 'm_read_n',
      badge: `Read user input: n = ${n} staircase steps`,
      badgeType: 'info',
      state: {
        n,
        i: 0,
        iterations: 0,
        totalOps: 0,
        currentReturn: null,
        dpCells: getDPCells(),
        frames: [{ name: 'main()', args: `n=${n}` }]
      }
    });

    // Step 1: Call climbStairs(n)
    steps.push({
      code: 'm_call_climb',
      badge: `main() invokes climbStairs(n = ${n})`,
      badgeType: 'info',
      state: {
        n,
        i: 0,
        iterations: 0,
        totalOps: 0,
        currentReturn: null,
        dpCells: getDPCells(),
        frames: [{ name: 'main()', args: `n=${n}` }, { name: 'climbStairs()', args: `n=${n}` }]
      }
    });

    // Step 2: Base check for n <= 2
    steps.push({
      code: 'c_base_check',
      badge: `Check base condition: n <= 2 &rarr; ${n} <= 2 is ${n <= 2 ? 'TRUE' : 'FALSE (Proceed to allocate DP table)'}`,
      badgeType: 'info',
      state: {
        n,
        i: 0,
        iterations: 0,
        totalOps: 1,
        currentReturn: null,
        dpCells: getDPCells(),
        frames: [{ name: 'main()', args: `n=${n}` }, { name: 'climbStairs()', args: `n=${n}` }]
      }
    });

    if (n <= 2) {
      dpArr[n] = n;
      steps.push({
        code: 'c_ret_base',
        badge: `Base condition met: Return n = ${n} directly.`,
        badgeType: 'success',
        state: {
          n,
          i: n,
          iterations: 0,
          totalOps: 2,
          currentReturn: n,
          dpCells: getDPCells(n),
          frames: [{ name: 'main()', args: `n=${n}` }, { name: 'climbStairs()', args: `n=${n}` }]
        }
      });
      return steps;
    }

    // Step 3: Allocate DP array
    steps.push({
      code: 'c_alloc_dp',
      badge: `Allocate 1D array dp[0...${n}] of size ${n + 1} for bottom-up computation`,
      badgeType: 'info',
      state: {
        n,
        i: 0,
        iterations: 0,
        totalOps: 2,
        currentReturn: null,
        dpCells: getDPCells(),
        frames: [{ name: 'main()', args: `n=${n}` }, { name: 'climbStairs()', args: `n=${n}` }]
      }
    });

    // Step 4: Base case dp[1] = 1
    dpArr[1] = 1;
    steps.push({
      code: 'c_init_base1',
      badge: `Initialize base case: dp[1] = 1 (Exactly 1 way to climb 1 step: [1])`,
      badgeType: 'info',
      state: {
        n,
        i: 1,
        iterations: 0,
        totalOps: 3,
        currentReturn: null,
        dpCells: getDPCells(1),
        frames: [{ name: 'main()', args: `n=${n}` }, { name: 'climbStairs()', args: `n=${n}` }]
      }
    });

    // Step 5: Base case dp[2] = 2
    dpArr[2] = 2;
    steps.push({
      code: 'c_init_base2',
      badge: `Initialize base case: dp[2] = 2 (2 ways to climb 2 steps: [1+1] and [2])`,
      badgeType: 'info',
      state: {
        n,
        i: 2,
        iterations: 0,
        totalOps: 4,
        currentReturn: null,
        dpCells: getDPCells(2),
        frames: [{ name: 'main()', args: `n=${n}` }, { name: 'climbStairs()', args: `n=${n}` }]
      }
    });

    // Step 6: Loop iteratively from i = 3 to n
    let ops = 4;
    for (let i = 3; i <= n; i++) {
      ops++;
      const currentIter = i - 2;
      steps.push({
        code: 'c_for_loop',
        badge: `For loop iteration #${currentIter}: i = ${i} (i <= ${n} is TRUE)`,
        badgeType: 'info',
        state: {
          n,
          i,
          iterations: currentIter,
          totalOps: ops,
          currentReturn: null,
          dpCells: getDPCells(i),
          frames: [{ name: 'main()', args: `n=${n}` }, { name: 'climbStairs()', args: `n=${n}, i=${i}` }]
        }
      });

      dpArr[i] = dpArr[i - 1] + dpArr[i - 2];
      ops++;

      steps.push({
        code: 'c_dp_trans',
        badge: `Transition: dp[${i}] = (1-step from ${i - 1}: dp[${i - 1}] = ${dpArr[i - 1]}) + (2-step from ${i - 2}: dp[${i - 2}] = ${dpArr[i - 2]}) &rarr; dp[${i}] = ${dpArr[i]}`,
        badgeType: 'success',
        state: {
          n,
          i,
          iterations: currentIter,
          totalOps: ops,
          currentReturn: dpArr[i],
          dpCells: getDPCells(i),
          frames: [{ name: 'main()', args: `n=${n}` }, { name: 'climbStairs()', args: `n=${n}, i=${i}` }]
        }
      });
    }

    const totalIterations = Math.max(0, n - 2);

    // Step 7: Return dp[n]
    steps.push({
      code: 'c_ret_dpn',
      badge: `Return dp[${n}] = ${dpArr[n]} to main()`,
      badgeType: 'success',
      state: {
        n,
        i: n,
        iterations: totalIterations,
        totalOps: ops + 1,
        currentReturn: dpArr[n],
        dpCells: getDPCells(n),
        frames: [{ name: 'main()', args: `n=${n}` }, { name: 'climbStairs()', args: `n=${n}` }]
      }
    });

    // Step 8: Print
    steps.push({
      code: 'm_print',
      badge: `Output Result: Print total distinct ways = ${dpArr[n]}`,
      badgeType: 'success',
      state: {
        n,
        i: n,
        iterations: totalIterations,
        totalOps: ops + 2,
        currentReturn: dpArr[n],
        dpCells: getDPCells(n),
        frames: [{ name: 'main()', args: `n=${n}, ways=${dpArr[n]}` }]
      }
    });

    // Step 9: Done
    steps.push({
      code: 'm_done',
      badge: `Bottom-Up Tabulation Complete! Computed dp[${n}] = ${dpArr[n]} in single O(n) pass (${totalIterations} iterations)!`,
      badgeType: 'success',
      state: {
        n,
        i: n,
        iterations: totalIterations,
        totalOps: ops + 2,
        currentReturn: dpArr[n],
        dpCells: getDPCells(n),
        frames: [{ name: 'main()', args: `n=${n}, ways=${dpArr[n]}` }]
      }
    });
  }

  return steps;
}

/* ==================================================================== */
/* REACTIVE STATE & CONTROLS                                            */
/* ==================================================================== */
const currentApproach = ref('recursion');
const inpN = ref(5);
const lang = ref('java');
const speed = ref(650);
const si = ref(0);
const playing = ref(false);
const vizHeight = ref(210);
const tableHeight = ref(38);
const leftWidth = ref(54);
const rightTab = ref('code');

// Warning Popup Modal State
const showWarningModal = ref(false);
const warningModalTitle = ref('Input Limit Notice');
const warningModalMsg = ref('');
const warningModalLimit = ref(5);
const requestedValue = ref(0);

function closeWarningModal() {
  showWarningModal.value = false;
}

const maxAllowedN = computed(() => {
  if (currentApproach.value === 'tabulation') return 20;
  return 5; // 'recursion' and 'memoization'
});

const stepsData = reactive({ steps: buildSteps('recursion', 5) });
const steps = computed(() => stepsData.steps);
const s = computed(() => steps.value[Math.max(0, Math.min(si.value, steps.value.length - 1))] || { state: {} });
const st = computed(() => s.value.state || {});

const codeLines = computed(() => {
  const appCodes = CODES[currentApproach.value] || CODES.recursion;
  return appCodes[lang.value] || appCodes.java || [];
});

const pseudocodeLines = computed(() => {
  return PSEUDOCODES[currentApproach.value] || PSEUDOCODES.recursion;
});

let playTimer = null;

function applyApproach(newApproach) {
  currentApproach.value = newApproach;
  const maxN = newApproach === 'tabulation' ? 20 : 5;
  let val = parseInt(inpN.value, 10);
  if (isNaN(val) || val < 1) val = 1;

  if (val > maxN) {
    const origVal = val;
    val = maxN;
    inpN.value = val;
    requestedValue.value = origVal;
    warningModalLimit.value = maxN;
    if (newApproach === 'tabulation') {
      warningModalTitle.value = 'Tabulation Limit (Max: 20)';
      warningModalMsg.value = `Input n = ${origVal} exceeds the maximum allowed limit for DP Tabulation. The input has been adjusted to n = 20 for optimal display.`;
    } else {
      const approachName = newApproach === 'recursion' ? 'Brute Force Recursion' : 'DP Memoization';
      warningModalTitle.value = `${approachName} Limit (Max: 5)`;
      warningModalMsg.value = `Input n = ${origVal} exceeds the maximum allowed limit for ${approachName}. The input has been adjusted to n = 5 to preserve recursion tree readability.`;
    }
    showWarningModal.value = true;
  } else {
    inpN.value = val;
  }

  playing.value = false;
  stepsData.steps = buildSteps(newApproach, val);
  si.value = 0;
}

function applyInput() {
  let val = parseInt(inpN.value, 10);
  if (isNaN(val) || val < 1) val = 1;
  const maxN = maxAllowedN.value;

  if (val > maxN) {
    const origVal = val;
    val = maxN;
    inpN.value = val;
    requestedValue.value = origVal;
    warningModalLimit.value = maxN;
    if (currentApproach.value === 'tabulation') {
      warningModalTitle.value = 'Tabulation Limit (Max: 20)';
      warningModalMsg.value = `Input n = ${origVal} exceeds the maximum limit for DP Tabulation (max: 20). Processing has been capped at n = 20.`;
    } else {
      const approachName = currentApproach.value === 'recursion' ? 'Brute Force Recursion' : 'DP Memoization';
      warningModalTitle.value = `${approachName} Limit (Max: 5)`;
      warningModalMsg.value = `Input n = ${origVal} exceeds the maximum limit for ${approachName} (max: 5). Processing has been capped at n = 5 to prevent exponential recursion call overhead.`;
    }
    showWarningModal.value = true;
  } else {
    inpN.value = val;
  }

  playing.value = false;
  stepsData.steps = buildSteps(currentApproach.value, val);
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
  if (si.value < steps.value.length - 1) {
    si.value++;
    playTimer = setTimeout(tick, speed.value);
  } else {
    playing.value = false;
  }
}

function onKeydown(e) {
  if (e.target.tagName === 'INPUT' || e.target.tagName === 'SELECT' || e.target.tagName === 'TEXTAREA') return;
  if (e.key === ' ' || e.key === 'Spacebar') {
    e.preventDefault();
    togglePlay();
  } else if (e.key === 'ArrowRight' || e.key === 'Right') {
    e.preventDefault();
    stepBy(1);
  } else if (e.key === 'ArrowLeft' || e.key === 'Left') {
    e.preventDefault();
    stepBy(-1);
  } else if (e.key === 'Escape') {
    closeWarningModal();
  }
}

/* ==================================================================== */
/* RESIZABLE SPLIT PANELS (HORIZONTAL & VERTICAL)                      */
/* ==================================================================== */
const mainRef = ref(null);
const leftColRef = ref(null);
const hResizerRef = ref(null);
const vizResizerRef = ref(null);
const tableResizerRef = ref(null);

function initHResizer(resizerEl, leftWidthRef, minPct = 25, maxPct = 80) {
  if (!resizerEl) return () => {};
  let startX = 0;
  let startW = 0;

  function onMouseDown(e) {
    startX = e.clientX;
    const container = resizerEl.parentElement;
    if (!container) return;
    startW = leftWidthRef.value;
    resizerEl.classList.add('drag');
    document.addEventListener('mousemove', onMouseMove);
    document.addEventListener('mouseup', onMouseUp);
    e.preventDefault();
  }

  function onMouseMove(e) {
    const container = resizerEl.parentElement;
    if (!container) return;
    const rect = container.getBoundingClientRect();
    const dx = e.clientX - startX;
    const newPct = startW + (dx / rect.width) * 100;
    leftWidthRef.value = Math.max(minPct, Math.min(maxPct, newPct));
  }

  function onMouseUp() {
    resizerEl.classList.remove('drag');
    document.removeEventListener('mousemove', onMouseMove);
    document.removeEventListener('mouseup', onMouseUp);
  }

  resizerEl.addEventListener('mousedown', onMouseDown);
  return () => {
    resizerEl.removeEventListener('mousedown', onMouseDown);
    document.removeEventListener('mousemove', onMouseMove);
    document.removeEventListener('mouseup', onMouseUp);
  };
}

function initVResizer(resizerEl, heightRef, minPx = 60, maxPx = 400) {
  if (!resizerEl) return () => {};
  let startY = 0;
  let startH = 0;

  function onMouseDown(e) {
    startY = e.clientY;
    startH = heightRef.value;
    resizerEl.classList.add('drag');
    document.addEventListener('mousemove', onMouseMove);
    document.addEventListener('mouseup', onMouseUp);
    e.preventDefault();
  }

  function onMouseMove(e) {
    const dy = e.clientY - startY;
    heightRef.value = Math.max(minPx, Math.min(maxPx, startH + dy));
  }

  function onMouseUp() {
    resizerEl.classList.remove('drag');
    document.removeEventListener('mousemove', onMouseMove);
    document.removeEventListener('mouseup', onMouseUp);
  }

  resizerEl.addEventListener('mousedown', onMouseDown);
  return () => {
    resizerEl.removeEventListener('mousedown', onMouseDown);
    document.removeEventListener('mousemove', onMouseMove);
    document.removeEventListener('mouseup', onMouseUp);
  };
}

let cleanupFns = [];

onMounted(() => {
  document.addEventListener('keydown', onKeydown);
  cleanupFns.push(initHResizer(hResizerRef.value, leftWidth, 30, 75));
  cleanupFns.push(initVResizer(vizResizerRef.value, vizHeight, 100, 360));
  cleanupFns.push(initVResizer(tableResizerRef.value, tableHeight, 30, 140));
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

            <!-- Input for N -->
            <div class="ll-input-group">
              <label>n =</label>
              <input
                type="number"
                v-model.number="inpN"
                class="ll-num-input"
                :min="1"
                :max="maxAllowedN"
                @keyup.enter="applyInput"
              />
              <span class="ll-input-hint">(max {{ maxAllowedN }})</span>
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
              <div class="ll-viz-wrap" :style="{ height: vizHeight + 'px' }">
                <div class="ll-perm-area">
                  <!-- Metrics Chips Bar -->
                  <div class="ll-ptrs">
                    <div class="ll-ptr-chip">n = <b class="ll-c-blue">{{ st.curN !== undefined ? st.curN : (st.n !== undefined ? st.n : inpN) }}</b></div>
                    <div class="ll-ptr-chip" v-if="currentApproach === 'recursion' || currentApproach === 'memoization'">
                      Calls: <b class="ll-c-orange">{{ st.totalCalls || 0 }}</b>
                    </div>
                    <div class="ll-ptr-chip" v-if="currentApproach === 'tabulation'">
                      Iterations: <b class="ll-c-orange">{{ st.iterations !== undefined ? st.iterations : 0 }}</b>
                    </div>
                    <div
                      class="ll-ptr-chip"
                      v-if="currentApproach === 'recursion'"
                      :class="{ 'll-chip-warn': (st.redundantCalls || 0) > 0 }"
                    >
                      Redundant: <b class="ll-c-red">{{ st.redundantCalls || 0 }}</b>
                    </div>
                    <div class="ll-ptr-chip" v-if="currentApproach === 'memoization'">
                      Cache Hits: <b class="ll-c-green">{{ st.cacheHits || 0 }}</b>
                    </div>
                    <div class="ll-ptr-chip" v-if="st.currentReturn !== null && st.currentReturn !== undefined">
                      Ways: <b class="ll-c-green">{{ st.currentReturn }}</b>
                    </div>
                  </div>

                  <!-- Visual Diagram 1: DP Tabulation (Bottom-Up 1D Array Strip) -->
                  <div v-if="currentApproach === 'tabulation'" class="ll-dp-tab-view">
                    <div class="ll-section-caption">
                      <span>1D Staircase DP Strip &mdash; <code>dp[1...{{ st.n }}]</code></span>
                      <span v-if="st.i >= 3 && st.i <= st.n" class="ll-calc-pill">
                        dp[{{ st.i }}] = dp[{{ st.i - 1 }}] (+1 step) + dp[{{ st.i - 2 }}]
                      </span>
                    </div>

                    <div class="ll-arr-track">
                      <template v-for="cell in st.dpCells" :key="cell.idx">
                        <div v-if="cell.idx >= 1" class="ll-arr-cell-wrap">
                          <!-- Pointer Tag Above Cell -->
                          <div class="ll-ptr-tag-wrap">
                            <span v-if="st.i >= 3 && cell.idx === st.i - 2" class="ll-ptr-lbl ll-lbl-purple">&darr; i-2</span>
                            <span v-else-if="st.i >= 3 && cell.idx === st.i - 1" class="ll-ptr-lbl ll-lbl-orange">&darr; i-1</span>
                            <span v-else-if="cell.idx === st.i" class="ll-ptr-lbl ll-lbl-blue">&darr; i</span>
                          </div>

                          <!-- Array Box Value -->
                          <div
                            class="ll-arr-box"
                            :class="{
                              'll-box-base': cell.status === 'base',
                              'll-box-cur': cell.idx === st.i,
                              'll-box-found': cell.status === 'computed',
                              'll-box-uncalc': cell.status === 'uncalculated'
                            }"
                          >
                            {{ cell.val }}
                          </div>
                          <!-- Step Index Label -->
                          <div class="ll-arr-idx">Step {{ cell.idx }}</div>
                        </div>
                      </template>
                    </div>
                  </div>

                  <!-- Visual Diagram 2: Recursion & Memoization Call Tree -->
                  <div v-else class="ll-tree-container">
                    <div class="ll-section-caption">
                      <span>{{ currentApproach === 'recursion' ? 'Recursive Call Tree (Exponential Explosion O(2^n))' : 'Pruned Memoization Call Tree (Linear O(n))' }}</span>
                      <span v-if="currentApproach === 'memoization'" class="ll-memo-badge-info">Lookup Cache Active</span>
                    </div>

                    <div class="ll-tree-scroll-area">
                      <svg
                        :viewBox="`0 0 ${st.treeWidth || 488} ${st.treeHeight || 216}`"
                        :style="{
                          width: '100%',
                          maxWidth: (st.treeWidth || 488) + 'px',
                          height: (st.treeHeight || 216) + 'px',
                          maxHeight: (st.treeHeight || 216) + 'px'
                        }"
                        class="ll-tree-svg"
                      >
                        <!-- Connector Edges -->
                        <g class="ll-tree-edges">
                          <line
                            v-for="(edge, idx) in st.treeEdges"
                            :key="idx"
                            :x1="edge.x1"
                            :y1="edge.y1"
                            :x2="edge.x2"
                            :y2="edge.y2"
                            class="ll-tree-edge"
                            :class="{
                              'll-edge-left': edge.isLeft,
                              'll-edge-right': !edge.isLeft
                            }"
                          />
                        </g>

                        <!-- Tree Node Badges -->
                        <g class="ll-tree-nodes">
                          <g
                            v-for="node in st.treeNodes"
                            :key="node.id"
                            :transform="`translate(${node.x}, ${node.y})`"
                            class="ll-tree-node-group"
                          >
                            <!-- Redundant Warning Badge Above Node -->
                            <g v-if="node.isRedundant" transform="translate(0, -17)">
                              <rect x="-23" y="-6" width="46" height="12" rx="3" fill="#ef4444" stroke="#dc2626" stroke-width="0.5" />
                              <text x="0" y="0" text-anchor="middle" class="ll-badge-text-red">Recomputing!</text>
                            </g>
                            <!-- Cache Hit Badge Above Node -->
                            <g v-else-if="node.state === 'cache-hit'" transform="translate(0, -17)">
                              <rect x="-23" y="-6" width="46" height="12" rx="3" fill="#9333ea" stroke="#7e22ce" stroke-width="0.5" />
                              <text x="0" y="0" text-anchor="middle" class="ll-badge-text-purple">Cache Hit O(1)</text>
                            </g>

                            <!-- Node Outer Card -->
                            <rect
                              x="-23"
                              y="-12"
                              width="46"
                              height="24"
                              rx="4"
                              class="ll-node-rect"
                              :class="{
                                'll-node-active': node.id === st.activeNodeId,
                                'll-node-solved': node.state === 'solved',
                                'll-node-redundant': node.isRedundant,
                                'll-node-cachehit': node.state === 'cache-hit'
                              }"
                            />
                            <!-- Node Text: ways(k) -->
                            <text x="0" y="-3" text-anchor="middle" class="ll-node-text-call">
                              ways({{ node.k }})
                            </text>
                            <!-- Node Return Value or Status -->
                            <text
                              x="0"
                              y="7.5"
                              text-anchor="middle"
                              class="ll-node-text-val"
                              :class="{
                                'll-val-active': node.id === st.activeNodeId,
                                'll-val-solved': node.state === 'solved',
                                'll-val-hit': node.state === 'cache-hit'
                              }"
                            >
                              {{
                                node.retVal !== null ? `= ${node.retVal}` :
                                (node.state === 'cache-hit' ? '⚡ Hit' : '?')
                              }}
                            </text>
                          </g>
                        </g>
                      </svg>

                      <!-- Memoization Lookup Cache Table Strip (Only in Memoization Mode) -->
                      <div v-if="currentApproach === 'memoization' && st.memo" class="ll-memo-strip-wrap">
                        <div class="ll-memo-title">Memoization Cache Table &mdash; <code>memo[1...{{ st.n }}]</code>:</div>
                        <div class="ll-memo-strip">
                          <template v-for="(mVal, mIdx) in st.memo" :key="mIdx">
                            <div v-if="mIdx >= 1" class="ll-memo-cell-wrap">
                              <div
                                class="ll-memo-cell"
                                :class="{
                                  'll-memo-hit': mVal !== -1,
                                  'll-memo-empty': mVal === -1
                                }"
                              >
                                {{ mVal !== -1 ? mVal : '?' }}
                              </div>
                              <span class="ll-memo-idx">Step {{ mIdx }}</span>
                            </div>
                          </template>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>

              <!-- Vertical Resizer for Viz Panel -->
              <div class="ll-vresizer" ref="vizResizerRef"></div>

              <!-- Visual Color Legend -->
              <div class="ll-legend">
                <template v-if="currentApproach === 'tabulation'">
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-base"></span>Base Cases (1, 2)</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-purple"></span>Term (dp[i-2])</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-cur"></span>Term (dp[i-1])</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-new"></span>Calculated dp[i]</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-uncalc"></span>Uncalculated</span>
                </template>
                <template v-else-if="currentApproach === 'memoization'">
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-cur"></span>Active Call</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-new"></span>Solved &amp; Cached</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-purple"></span>Cache Hit (O(1) Return)</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-uncalc"></span>Uncalled</span>
                </template>
                <template v-else>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-cur"></span>Active Call</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-new"></span>Solved / Base Case</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-red"></span>Redundant Call</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-uncalc"></span>Uncalled</span>
                </template>
              </div>

              <!-- Call Stack & Variable Environment Panel -->
              <div class="ll-table-area" :style="{ height: tableHeight + 'px' }">
                <div class="ll-table-title">Call Stack &amp; Variables &mdash; innermost = active:</div>
                <div class="ll-stack-line">
                  <div
                    v-for="(f, i) in (st.frames || [])"
                    :key="i"
                    class="ll-frame"
                    :class="{ 'll-frame-cur': i === (st.frames.length - 1) }"
                    :style="{ marginLeft: i * 8 + 'px' }"
                  >
                    <span class="ll-fname">{{ f.name }}</span>({{ f.args }})
                    <span v-if="i === (st.frames.length - 1)" class="ll-now">&larr; active</span>
                  </div>
                </div>
              </div>

              <!-- Vertical Resizer for Call Stack Panel -->
              <div class="ll-vresizer" ref="tableResizerRef"></div>

              <!-- Current Execution Step Badge Description -->
              <div class="ll-badge-wrap">
                <div
                  class="ll-badge"
                  :class="{
                    'll-badge-error': s.badgeType === 'warn',
                    'll-badge-success': s.badgeType === 'success'
                  }"
                  v-html="s.badge || 'Initializing algorithm state...'"
                ></div>
              </div>
            </div>

            <!-- Horizontal Resizer -->
            <div class="ll-resizer" ref="hResizerRef"></div>

            <!-- Right Column: Synchronized Code, Pseudocode, Complexity -->
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

                <!-- Code Tab with 1-to-1 Synchronized Highlighting -->
                <div v-if="rightTab === 'code'" class="ll-code-scroll">
                  <pre class="ll-pre"><span
                    v-for="(line, i) in codeLines"
                    :key="i"
                    class="ll-codeline"
                    :class="{ 'll-hl': line[0] && line[0] === s.code }"
                  >{{ line[1] === '' ? ' ' : line[1] }}</span></pre>
                </div>

                <!-- Pseudocode Tab -->
                <div v-else-if="rightTab === 'pseudo'" class="ll-code-scroll">
                  <pre class="ll-pre"><span
                    v-for="(line, i) in pseudocodeLines"
                    :key="i"
                    class="ll-codeline"
                  >{{ line }}</span></pre>
                </div>

                <!-- Complexity & Faculty Analysis Tab -->
                <div v-else class="ll-info-scroll">
                  <h3>Why Dynamic Programming? (Climbing Stairs Insight)</h3>
                  <p>
                    The <strong>Climbing Stairs</strong> problem is a textbook example of a combinatorial counting problem modeled via Dynamic Programming:
                  </p>
                  <ul>
                    <li>
                      <strong>1. Optimal Substructure:</strong> To reach step <code>n</code>, the final jump can only come from step <code>n-1</code> (taking 1 step) or step <code>n-2</code> (taking 2 steps). Thus:
                      <br /><code>ways(n) = ways(n - 1) + ways(n - 2)</code>
                    </li>
                    <li>
                      <strong>2. Overlapping Subproblems:</strong> Naive recursion independently re-computes identical staircase subproblems (e.g. <code>ways(3)</code> and <code>ways(2)</code> are evaluated multiple times across branches).
                    </li>
                  </ul>

                  <h3>Approaches Comparison</h3>
                  <table class="ll-complexity-table">
                    <thead>
                      <tr>
                        <th>Approach</th>
                        <th>Time Complexity</th>
                        <th>Space Complexity</th>
                        <th>Repeated Subproblems</th>
                      </tr>
                    </thead>
                    <tbody>
                      <tr>
                        <td><strong>Naive Recursion</strong></td>
                        <td>O(2<sup>n</sup>) Exponential</td>
                        <td>O(n) Stack</td>
                        <td><span class="ll-c-red" style="font-weight:700">YES</span> (Exponential Duplication)</td>
                      </tr>
                      <tr>
                        <td><strong>DP Memoization</strong></td>
                        <td>O(n) Linear</td>
                        <td>O(n) Cache + Stack</td>
                        <td><span class="ll-c-green" style="font-weight:700">NO</span> (O(1) Lookup on Hit)</td>
                      </tr>
                      <tr>
                        <td><strong>DP Tabulation</strong></td>
                        <td>O(n) Linear</td>
                        <td>O(n) 1D Table</td>
                        <td><span class="ll-c-green" style="font-weight:700">NO</span> (Iterative Single Pass)</td>
                      </tr>
                      <tr>
                        <td><strong>Space-Optimized DP</strong></td>
                        <td>O(n) Linear</td>
                        <td>O(1) Two Variables</td>
                        <td><span class="ll-c-green" style="font-weight:700">NO</span> (Keeps Only Last 2 Steps)</td>
                      </tr>
                    </tbody>
                  </table>

                  <h3>Base Cases &amp; Sequence Progression</h3>
                  <p>
                    Starting from step 1:
                  </p>
                  <p class="ll-math-box">
                    ways(1) = 1 &nbsp;|&nbsp; ways(2) = 2 &nbsp;|&nbsp; ways(3) = 3 &nbsp;|&nbsp; ways(4) = 5 &nbsp;|&nbsp; ways(5) = 8
                  </p>
                  <p>
                    For <code>n = 45</code>, Naive Recursion would perform over <strong>2,000,000,000</strong> calls, while DP Tabulation computes the exact answer in just <strong>45 operations</strong>!
                  </p>

                  <div class="ll-note">
                    <strong>Faculty Takeaway:</strong> Dynamic Programming transforms an exponential combinatorial explosion into a simple linear addition sequence by caching subproblem solutions in memory.
                  </div>
                </div>
              </div>
            </div>
          </div>

          <!-- Bottom Footer -->
          <div class="ll-footer">
            Step {{ si + 1 }} / {{ steps.length }}
            <span class="ll-speed-wrap">Speed <input type="range" min="100" max="2000" step="100" v-model.number="speed" /></span>
          </div>

          <!-- Warning Modal for Input Limit Exceeded -->
          <transition name="ll-modal-fade">
            <div v-if="showWarningModal" class="ll-modal-backdrop" @click.self="closeWarningModal">
              <div class="ll-modal-card" @click.stop>
                <!-- Modal Header -->
                <div class="ll-modal-header">
                  <div class="ll-modal-title-wrap">
                    <div class="ll-modal-icon-badge">
                      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" class="ll-modal-svg-icon">
                        <path d="M10.29 3.86L1.82 18a2 2 0 0 0 1.71 3h16.94a2 2 0 0 0 1.71-3L13.71 3.86a2 2 0 0 0-3.42 0z"/>
                        <line x1="12" y1="9" x2="12" y2="13"/>
                        <line x1="12" y1="17" x2="12.01" y2="17"/>
                      </svg>
                    </div>
                    <span class="ll-modal-title">{{ warningModalTitle }}</span>
                  </div>
                  <button class="ll-modal-close-btn" @click="closeWarningModal" title="Close warning">&times;</button>
                </div>

                <!-- Modal Body -->
                <div class="ll-modal-body">
                  <div class="ll-modal-badge-row">
                    <span class="ll-modal-tag-entered">Entered: n = {{ requestedValue }}</span>
                    <span class="ll-modal-arrow">&rarr;</span>
                    <span class="ll-modal-tag-applied">Adjusted to Max: n = {{ warningModalLimit }}</span>
                  </div>
                  <p class="ll-modal-message">{{ warningModalMsg }}</p>
                </div>

                <!-- Modal Footer -->
                <div class="ll-modal-footer">
                  <button class="ll-modal-confirm-btn" @click="closeWarningModal">
                    Got it, Proceed &#10003;
                  </button>
                </div>
              </div>
            </div>
          </transition>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.ll-root * { box-sizing: border-box; }
.ll-root *, .ll-root, .row-main {
  scrollbar-width: none !important;
  -ms-overflow-style: none !important;
}
.ll-root *::-webkit-scrollbar,
.ll-root::-webkit-scrollbar,
.row-main::-webkit-scrollbar {
  display: none !important;
  width: 0 !important;
  height: 0 !important;
}

.ll-root {
  --coral: #F04D4D; --coral-dark: #d93e3e; --coral-light: #fff0f0;
  --bg: #f5f6fa; --surface: #ffffff; --surface2: #f1f4f9;
  --border: #e2e8f0; --border2: #cbd5e1; --text: #1e293b; --text2: #475569; --muted: #94a3b8;
  --blue: #3b82f6; --blue-light: #eff6ff; --green: #22c55e; --green-light: #f0fdf4;
  --orange: #f97316; --orange-light: #fff7ed; --purple: #9333ea; --purple-light: #f3e8ff;
  --red: #ef4444; --red-dark: #991b1b; --red-light: #fef2f2;
  --shadow-sm: 0 1px 3px rgba(0,0,0,.08), 0 1px 2px rgba(0,0,0,.04);
  --radius: 8px; --radius-sm: 6px;
  background: var(--bg); color: var(--text);
  font-family: 'Segoe UI', system-ui, sans-serif; font-size: 12.5px;
  display: flex; flex-direction: column; overflow: hidden; width: 100%; height:58vh;  /* modified */
}

@keyframes ll-pop { from { transform: scale(.85); opacity: 0; } to { transform: scale(1); opacity: 1; } }
@keyframes ll-pulse { 0%, 100% { transform: scale(1); } 50% { transform: scale(1.05); } }

.slide-wrapper { margin-top: -10px; margin-left: -30px; width: 107%; max-height: 100%; font-size: 0.8rem; font-weight: 400; }
.slide-body { display: flex; flex-direction: column; border-radius: 4px; height: 100%; }
.navbar { display: flex; flex-direction: row; justify-content: space-between; align-items: center; gap: 0.75rem; padding: 0 10px; background-color: #ffffff; position: fixed; width: 94.7%; }
.navbar > img { height: 30px; }
.navbar-title { margin: 0; font-size: 1.35rem; font-weight: 700; background-color: #ef5050; color: #ffffff; width: 80%; padding: 2px 10px; margin-left: -10px; border-radius: 5px; }
.row-main { width: 100%; height: 90%; margin-top: 36px; overflow-x: auto; overflow-y: auto; }

/* Control Panel Toolbar */
.ll-toolbar { margin-top: 4px; display: flex; align-items: center; gap: 6px; padding: 5px 12px; background: var(--surface); border-bottom: 1px solid var(--border); flex-shrink: 0; flex-wrap: wrap; box-shadow: var(--shadow-sm); }
.ll-approach-group { display: flex; gap: 2px; background: var(--surface2); padding: 2px; border-radius: var(--radius-sm); border: 1px solid var(--border); }
.ll-approach-btn { background: transparent; border: none; padding: 4px 8px; font-size: 11px; font-weight: 600; color: var(--text2); border-radius: 4px; cursor: pointer; transition: all .15s; white-space: nowrap; }
.ll-approach-btn:hover { color: var(--coral); }
.ll-approach-btn.active { background: var(--coral); color: #fff; box-shadow: var(--shadow-sm); }

.ll-input-group { display: flex; align-items: center; gap: 4px; }
.ll-input-group label { font-size: 11.5px; color: var(--muted); font-weight: 700; }
.ll-num-input { background: var(--surface); border: 1px solid var(--border2); color: var(--text); border-radius: var(--radius-sm); padding: 3px 6px; font-size: 12px; font-family: monospace; width: 45px; }
.ll-num-input:focus { outline: none; border-color: var(--coral); box-shadow: 0 0 0 3px rgba(240,77,77,.1); }
.ll-input-hint { font-size: 10px; color: var(--muted); }

.ll-viz-btn { background: var(--coral); color: #fff; border: none; padding: 5px 12px; border-radius: var(--radius-sm); cursor: pointer; font-size: 11.5px; font-weight: 600; box-shadow: var(--shadow-sm); transition: filter .15s; }
.ll-viz-btn:hover { filter: brightness(1.08); }

.ll-nav-controls { display: flex; margin-left: auto; align-items: center; gap: 4px; flex-shrink: 0; flex-wrap: wrap; }
.ll-nav-btn { background: var(--surface2); border: 1px solid var(--border2); color: var(--text2); padding: 4px 9px; border-radius: var(--radius-sm); cursor: pointer; font-size: 11px; font-weight: 500; transition: all .15s; white-space: nowrap; }
.ll-nav-btn:hover { background: var(--surface); border-color: var(--coral); color: var(--coral); }
.ll-play-btn { background: var(--blue-light); border: 1px solid var(--blue); color: var(--blue); min-width: 68px; font-weight: 600; padding: 4px 9px; border-radius: var(--radius-sm); cursor: pointer; font-size: 11px; transition: all .15s; }
.ll-play-btn:hover { background: var(--blue); color: #fff; }

/* Main Split Layout */
.ll-main { display: flex; flex: 1; overflow: hidden; position: relative; }
.ll-left-col { display: flex; flex-direction: column; overflow: hidden; min-width: 220px; max-width: 75%; }
.ll-resizer { width: 5px; cursor: col-resize; background: var(--border); flex-shrink: 0; transition: background .15s; position: relative; z-index: 20; }
.ll-resizer:hover, .ll-resizer.drag { background: var(--coral); }
.ll-right-col { display: flex; flex-direction: column; flex: 1; overflow: hidden; min-width: 0; }

/* Left Visualization Wrappers */
.ll-viz-wrap { flex-shrink: 0; background: var(--surface); border-bottom: 1px solid var(--border); position: relative; overflow-x: auto; overflow-y: auto; }
.ll-perm-area { display: flex; flex-direction: column; align-items: stretch; min-height: 100%; width: 100%; min-width: 0; box-sizing: border-box; }
.ll-ptrs { display: flex; gap: 6px; flex-wrap: wrap; padding: 4px 10px; min-height: 28px; width: 100%; box-sizing: border-box; min-width: 0; }
.ll-ptr-chip { background: var(--surface2); border: 1px solid var(--border); border-radius: var(--radius-sm); padding: 2px 7px; font-size: 11px; font-family: monospace; box-shadow: var(--shadow-sm); white-space: nowrap; flex-shrink: 0; }
.ll-chip-warn { background: #fef2f2 !important; border-color: #fca5a5 !important; }

.ll-c-blue { color: var(--blue); } .ll-c-orange { color: var(--orange); } .ll-c-green { color: var(--green); } .ll-c-purple { color: var(--purple); } .ll-c-red { color: var(--red); }

.ll-section-caption { display: flex; justify-content: space-between; align-items: center; padding: 3px 10px; font-size: 10.5px; font-weight: 700; color: var(--text2); background: var(--surface2); border-top: 1px solid var(--border); border-bottom: 1px solid var(--border); }
.ll-calc-pill { background: #dcfce7; color: #15803d; padding: 1px 6px; border-radius: 10px; font-size: 10.5px; font-family: monospace; font-weight: 700; }
.ll-memo-badge-info { background: var(--purple-light); color: var(--purple); padding: 1px 6px; border-radius: 10px; font-size: 10px; }

/* Diagram 1: DP Tabulation 1D Array Strip */
.ll-dp-tab-view { display: flex; flex-direction: column; padding: 6px 10px; }
.ll-arr-track { display: flex; align-items: flex-start; flex-wrap: wrap; padding: 6px 4px; gap: 8px; width: 100%; box-sizing: border-box; min-width: 0; }
.ll-arr-cell-wrap { display: flex; flex-direction: column; align-items: center; min-width: 0; }
.ll-ptr-tag-wrap { height: 22px; display: flex; align-items: flex-end; justify-content: center; margin-bottom: 2px; }
.ll-ptr-lbl { font-size: 10.5px; font-weight: 800; font-family: 'Consolas', 'Fira Code', monospace; display: inline-flex; align-items: center; line-height: 1; white-space: nowrap; animation: ll-pop 0.2s ease; }
.ll-lbl-orange { color: #f97316; } .ll-lbl-purple { color: #9333ea; } .ll-lbl-green { color: #10b981; }

.ll-arr-box { width: 46px; height: 46px; display: flex; align-items: center; justify-content: center; border: 2px solid var(--border2); border-radius: var(--radius); background: var(--surface); color: var(--text); font-weight: 700; font-size: 14px; box-shadow: var(--shadow-sm); transition: all 0.25s ease; animation: ll-pop .3s ease; }
.ll-box-base { border-color: #3b82f6 !important; background: #eff6ff !important; color: #1d4ed8 !important; }
.ll-box-cur { border-color: #f59e0b !important; background: #fef3c7 !important; color: #92400e !important; box-shadow: 0 0 0 3px rgba(245, 158, 11, 0.25) !important; transform: translateY(-2px); }
.ll-box-found { border-color: #10b981 !important; background: #dcfce7 !important; color: #065f46 !important; box-shadow: 0 0 0 3px rgba(16, 185, 129, 0.2) !important; transform: translateY(-2px); }
.ll-box-uncalc { border: 2px dashed var(--border2) !important; background: var(--surface2) !important; color: var(--muted) !important; }
.ll-arr-idx { font-size: 10px; color: var(--muted); margin-top: 3px; font-family: 'Consolas', monospace; font-weight: 600; }

/* Diagram 2: Recursion & Memoization Call Tree (Fixed 1:1 Pixel Scale) */
.ll-tree-container { display: flex; flex-direction: column; width: 100%; height: 100%; min-height: 0; }
.ll-tree-scroll-area { flex: 1; overflow: auto; padding: 2px 6px 4px; display: flex; flex-direction: column; align-items: center; justify-content: flex-start; min-height: 0; width: 100%; box-sizing: border-box; }
.ll-tree-svg { display: block; flex-shrink: 0; max-width: 100%; margin: 0 auto; }
.ll-tree-edge { stroke: #cbd5e1; stroke-width: 1.8px; transition: stroke .2s; }
.ll-edge-left { stroke: #94a3b8; }
.ll-edge-right { stroke: #cbd5e1; stroke-dasharray: 4, 3; }

.ll-tree-node-group { cursor: default; }
.ll-node-rect { fill: #ffffff; stroke: #cbd5e1; stroke-width: 1.4px; filter: drop-shadow(0 1px 2px rgba(0,0,0,0.05)); transition: all .25s ease; }
.ll-node-active { stroke: #f59e0b !important; stroke-width: 2.2px !important; fill: #fffbeb !important; filter: drop-shadow(0 0 5px rgba(245, 158, 11, 0.45)) !important; animation: ll-pulse 1.3s infinite ease-in-out; }
.ll-node-solved { stroke: #10b981 !important; fill: #dcfce7 !important; stroke-width: 1.6px !important; }
.ll-node-redundant { stroke: #ef4444 !important; stroke-dasharray: 3, 2 !important; fill: #fef2f2 !important; }
.ll-node-cachehit { stroke: #9333ea !important; stroke-width: 1.8px !important; fill: #f3e8ff !important; }

/* High-specificity SVG text styling to prevent Slidev font size overrides */
.ll-tree-svg text {
  font-family: 'Segoe UI', system-ui, sans-serif !important;
  user-select: none;
}
.ll-node-text-call {
  font-family: 'Consolas', 'Fira Code', monospace !important;
  font-size: 10px !important;
  font-weight: 700 !important;
  fill: #1e293b !important;
  dominant-baseline: central !important;
}
.ll-node-text-val {
  font-family: 'Consolas', 'Fira Code', monospace !important;
  font-size: 9px !important;
  font-weight: 700 !important;
  fill: #64748b !important;
  dominant-baseline: central !important;
}
.ll-val-active { fill: #b45309 !important; }
.ll-val-solved { fill: #047857 !important; }
.ll-val-hit { fill: #7e22ce !important; }

.ll-badge-text-red {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif !important;
  font-size: 6.8px !important;
  font-weight: 700 !important;
  fill: #ffffff !important;
  dominant-baseline: central !important;
}
.ll-badge-text-purple {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif !important;
  font-size: 6.8px !important;
  font-weight: 700 !important;
  fill: #ffffff !important;
  dominant-baseline: central !important;
}

/* Memoization Cache Table Strip */
.ll-memo-strip-wrap { width: 100%; padding: 4px 8px 6px; border-top: 1px dashed var(--border); background: var(--surface2); flex-shrink: 0; margin-top: 4px; box-sizing: border-box; }
.ll-memo-title { font-size: 10px; font-weight: 700; color: var(--text2); margin-bottom: 2px; }
.ll-memo-strip { display: flex; gap: 5px; flex-wrap: wrap; }
.ll-memo-cell-wrap { display: flex; flex-direction: column; align-items: center; }
.ll-memo-cell { width: 30px; height: 26px; display: flex; align-items: center; justify-content: center; font-size: 11px; font-family: monospace; font-weight: 700; border-radius: 4px; border: 1px solid var(--border); background: var(--surface); color: var(--text); }
.ll-memo-hit { background: #f3e8ff !important; border-color: #a855f7 !important; color: #6b21a8 !important; }
.ll-memo-empty { color: var(--muted); border-style: dashed; }
.ll-memo-idx { font-size: 9px; color: var(--muted); font-family: monospace; }

/* Resizers & Legend */
.ll-vresizer { height: 5px; cursor: row-resize; background: var(--border); flex-shrink: 0; transition: background .15s; position: relative; z-index: 20; }
.ll-vresizer:hover, .ll-vresizer.drag { background: var(--coral); }

.ll-legend { display: flex; flex-wrap: wrap; gap: 4px 10px; padding: 4px 10px; border-bottom: 1px solid var(--border); flex-shrink: 0; background: var(--surface2); }
.ll-leg { display: flex; align-items: center; gap: 4px; font-size: 10.5px; color: var(--text2); font-weight: 500; }
.ll-legdot { width: 9px; height: 9px; border-radius: 2px; flex-shrink: 0; display: inline-block; }
.ll-legdot-base { background: #eff6ff; border: 1.5px solid #3b82f6; }
.ll-legdot-cur { background: #fef3c7; border: 1.5px solid #f59e0b; }
.ll-legdot-new { background: #dcfce7; border: 1.5px solid #10b981; }
.ll-legdot-purple { background: #f3e8ff; border: 1.5px solid #9333ea; }
.ll-legdot-red { background: #fef2f2; border: 1.5px dashed #ef4444; }
.ll-legdot-uncalc { background: var(--surface2); border: 1.5px dashed var(--border2); }

/* Variable Frames & Call Stack Area */
.ll-table-area { flex-shrink: 0; padding: 4px 10px; border-bottom: 1px solid var(--border); overflow-x: hidden; overflow-y: auto; background: var(--surface); min-width: 0; box-sizing: border-box; }
.ll-table-title { font-size: 9.5px; color: var(--muted); margin-bottom: 2px; font-style: italic; }
.ll-stack-line { font-family: 'Consolas', monospace; font-size: 11px; line-height: 1.6; }
.ll-frame { font-family: 'Consolas', monospace; font-size: 10.5px; color: var(--text2); padding: 1px 0; white-space: nowrap; }
.ll-frame-cur { color: var(--orange); background: var(--orange-light); border-radius: 4px; padding: 1px 4px; }
.ll-fname { color: var(--text2); }
.ll-now { color: var(--orange); font-size: 9.5px; margin-left: 4px; }

/* Step Badge */
.ll-badge-wrap { padding: 4px 8px; border-bottom: 1px solid var(--border); flex-shrink: 0; min-height: 30px; display: flex; align-items: center; background: var(--surface); }
.ll-badge { display: inline-block; padding: 3px 10px; border-radius: var(--radius-sm); border-left: 3px solid var(--coral); background: var(--coral-light); font-size: 10.5px; color: var(--coral-dark); line-height: 1.35; word-break: break-word; font-weight: 500; }
.ll-badge-error { border-left-color: var(--red); background: var(--red-light); color: var(--red-dark); font-weight: 600; }
.ll-badge-success { border-left-color: var(--green); background: var(--green-light); color: #166534; font-weight: 600; }

/* Right Column: Code & Theory */
.ll-code-panel { display: flex; flex-direction: column; height: 100%; overflow: hidden; }
.ll-code-header { display: flex; align-items: center; gap: 6px; padding: 5px 12px; background: var(--surface); border-bottom: 1px solid var(--border); flex-shrink: 0; box-shadow: var(--shadow-sm); flex-wrap: wrap; }
.ll-tabbar { display: flex; gap: 3px; flex-wrap: wrap; }
.ll-tab-btn { background: var(--surface2); border: 1px solid var(--border2); color: var(--text2); padding: 4px 9px; border-radius: var(--radius-sm); cursor: pointer; font-size: 10.5px; font-weight: 600; transition: all .15s; white-space: nowrap; }
.ll-tab-btn:hover { border-color: var(--coral); color: var(--coral); }
.ll-tab-btn.active { background: var(--coral); border-color: var(--coral); color: #fff; }
.ll-lang-select { background: var(--surface2); border: 1px solid var(--border2); color: var(--text); padding: 4px 24px 4px 8px; border-radius: var(--radius-sm); font-size: 11px; font-weight: 500; cursor: pointer; min-width: 95px; margin-left: auto; transition: border-color .15s; appearance: none; background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='10' height='6' viewBox='0 0 10 6'%3E%3Cpath d='M0 0l5 6 5-6z' fill='%2394a3b8'/%3E%3C/svg%3E"); background-repeat: no-repeat; background-position: right 8px center; }
.ll-lang-select:focus { outline: none; border-color: var(--coral); box-shadow: 0 0 0 3px rgba(240,77,77,.1); }

.ll-code-scroll { flex: 1; overflow: auto; padding: 10px 14px; background: #f8fafc; min-width: 0; }
.ll-pre { font-family: 'Cascadia Code', 'Fira Code', 'Consolas', monospace; font-size: 11px; line-height: 1.5; white-space: pre; color: var(--text); margin: 0; }
.ll-codeline { display: block; padding: 0 14px; margin: 0 -14px; }
.ll-hl { background: #dcfce7; color: #15803d; border-radius: 3px; border-left: 3px solid var(--green); font-weight: 600; }

.ll-info-scroll { flex: 1; overflow: auto; padding: 12px 16px; background: var(--surface); color: var(--text2); font-size: 12px; line-height: 1.55; }
.ll-info-scroll h3 { margin: 0 0 6px; color: var(--text); font-size: 13px; font-weight: 700; }
.ll-info-scroll h3:not(:first-child) { margin-top: 14px; }
.ll-info-scroll p { margin: 0 0 6px; }
.ll-info-scroll ul { margin: 0 0 10px 16px; padding: 0; }
.ll-info-scroll li { margin-bottom: 3px; }
.ll-info-scroll code { background: var(--surface2); border: 1px solid var(--border); border-radius: 4px; padding: 1px 4px; font-family: 'Consolas', monospace; font-size: 11px; color: var(--coral-dark); }
.ll-complexity-table { width: 100%; border-collapse: collapse; margin-bottom: 10px; font-size: 11.5px; }
.ll-complexity-table th, .ll-complexity-table td { border: 1px solid var(--border); padding: 6px 8px; text-align: left; }
.ll-complexity-table th { background: var(--surface2); color: var(--text); font-weight: 700; }
.ll-complexity-table td:nth-child(2), .ll-complexity-table td:nth-child(3) { font-family: 'Consolas', monospace; }
.ll-math-box { background: var(--surface2); border: 1px solid var(--border2); padding: 6px 10px; border-radius: var(--radius-sm); font-family: 'Consolas', monospace; font-weight: 700; color: #1e293b; margin-bottom: 6px; }
.ll-note { background: var(--orange-light); border-left: 3px solid var(--orange); border-radius: var(--radius-sm); padding: 6px 10px; font-size: 11.5px; color: var(--text2); margin-top: 8px; }

/* Footer */
.ll-footer { padding: 3px 12px; font-size: 10.5px; color: var(--muted); border-top: 1px solid var(--border); background: var(--surface); flex-shrink: 0; display: flex; align-items: center; }
.ll-speed-wrap { display: flex; align-items: center; gap: 4px; margin-left: 14px; }
.ll-speed-wrap input[type=range] { width: 80px; accent-color: var(--coral); }

@media (max-width: 900px) {
  .ll-main { flex-direction: column; }
  .ll-left-col, .ll-right-col { max-width: 100% !important; width: 100% !important; }
  .ll-resizer { display: none; }
  .ll-toolbar { flex-direction: column; align-items: stretch; }
  .ll-nav-controls { margin-left: 0; justify-content: center; }
}

/* ==================================================================== */
/* WARNING POPUP MODAL STYLING                                          */
/* ==================================================================== */
.ll-modal-backdrop {
  position: fixed;
  inset: 0;
  background: rgba(15, 23, 42, 0.45);
  backdrop-filter: blur(4px);
  z-index: 99999;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 16px;
}

.ll-modal-card {
  background: #ffffff;
  border-radius: 12px;
  border: 1px solid #fee2e2;
  box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.2), 0 8px 10px -6px rgba(0, 0, 0, 0.1);
  width: 440px;
  max-width: 92vw;
  overflow: hidden;
  display: flex;
  flex-direction: column;
}

.ll-modal-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 12px 16px;
  background: #fff5f5;
  border-bottom: 1px solid #fee2e2;
}

.ll-modal-title-wrap {
  display: flex;
  align-items: center;
  gap: 10px;
}

.ll-modal-icon-badge {
  width: 30px;
  height: 30px;
  border-radius: 8px;
  background: #fee2e2;
  color: #ef4444;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}

.ll-modal-svg-icon {
  width: 17px;
  height: 17px;
}

.ll-modal-title {
  font-size: 13.5px;
  font-weight: 700;
  color: #991b1b;
  font-family: 'Segoe UI', system-ui, sans-serif;
}

.ll-modal-close-btn {
  background: none;
  border: none;
  font-size: 20px;
  color: #94a3b8;
  cursor: pointer;
  padding: 0 4px;
  border-radius: 4px;
  line-height: 1;
  transition: color 0.15s;
}

.ll-modal-close-btn:hover {
  color: #ef4444;
}

.ll-modal-body {
  padding: 16px 18px;
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.ll-modal-badge-row {
  display: flex;
  align-items: center;
  gap: 8px;
  font-size: 11.5px;
  font-weight: 600;
}

.ll-modal-tag-entered {
  background: #fee2e2;
  color: #dc2626;
  padding: 3px 8px;
  border-radius: 4px;
}

.ll-modal-arrow {
  color: #94a3b8;
  font-weight: 700;
}

.ll-modal-tag-applied {
  background: #dcfce7;
  color: #16a34a;
  padding: 3px 8px;
  border-radius: 4px;
}

.ll-modal-message {
  font-size: 12px;
  line-height: 1.55;
  color: #334155;
  margin: 0;
}

.ll-modal-footer {
  padding: 10px 16px 14px;
  display: flex;
  justify-content: flex-end;
  background: #fafafa;
  border-top: 1px solid #f1f5f9;
}

.ll-modal-confirm-btn {
  background: #ef5050;
  color: #ffffff;
  border: none;
  padding: 7px 18px;
  border-radius: 6px;
  font-size: 12px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.15s ease;
  box-shadow: 0 2px 4px rgba(239, 80, 80, 0.25);
}

.ll-modal-confirm-btn:hover {
  background: #dc2626;
  transform: translateY(-1px);
  box-shadow: 0 4px 8px rgba(239, 80, 80, 0.35);
}

.ll-modal-fade-enter-active,
.ll-modal-fade-leave-active {
  transition: opacity 0.2s ease;
}

.ll-modal-fade-enter-active .ll-modal-card,
.ll-modal-fade-leave-active .ll-modal-card {
  transition: transform 0.2s cubic-bezier(0.16, 1, 0.3, 1), opacity 0.2s ease;
}

.ll-modal-fade-enter-from,
.ll-modal-fade-leave-to {
  opacity: 0;
}

.ll-modal-fade-enter-from .ll-modal-card,
.ll-modal-fade-leave-to .ll-modal-card {
  opacity: 0;
  transform: scale(0.95) translateY(-8px);
}
</style>

