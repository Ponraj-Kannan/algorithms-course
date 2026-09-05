<script setup>
import { ref, reactive, computed, onMounted, onBeforeUnmount, watch } from 'vue';

const props = defineProps({
  topic: { type: String, default: 'Dynamic Programming' },
  subTopic: { type: String, default: 'Fibonacci Number' }
});

/* ==================================================================== */
/* APPROACHES & MULTI-LANGUAGE 100% EXECUTABLE CODE SPECIFICATIONS      */
/* ==================================================================== */
const APPROACHES = [
  { id: 'recursion', label: 'Brute Force', desc: 'O(2^n) Exponential — Recomputes Overlapping Subproblems' },
  { id: 'memoization', label: 'Memoization', desc: 'O(n) Linear Time & O(n) Space — Recursive with Cache' },
  { id: 'tabulation', label: 'Tabulation', desc: 'O(n) Linear Time & O(n) Space — Iterative 1D Table' }
];

const CODES = {
  recursion: {
    java: [
      ['',             'import java.util.Scanner;'],
      ['',             ''],
      ['',             'public class Main {'],
      ['c_entry',      '    static int fib(int n) {'],
      ['c_base_check', '        if (n <= 1) {'],
      ['c_ret_base',   '            return n;'],
      ['',             '        }'],
      ['c_rec_left',   '        int left = fib(n - 1);'],
      ['c_rec_right',  '        int right = fib(n - 2);'],
      ['c_ret_sum',    '        return left + right;'],
      ['',             '    }'],
      ['',             ''],
      ['',             '    public static void main(String[] args) {'],
      ['c_input',      '        Scanner sc = new Scanner(System.in);'],
      ['c_input',      '        int n = sc.nextInt();'],
      ['c_main_call',  '        int result = fib(n);'],
      ['c_main_ret',   '        System.out.println(result);'],
      ['',             '    }'],
      ['',             '}']
    ],
    c: [
      ['',             '#include <stdio.h>'],
      ['',             ''],
      ['c_entry',      'int fib(int n) {'],
      ['c_base_check', '    if (n <= 1) {'],
      ['c_ret_base',   '        return n;'],
      ['',             '    }'],
      ['c_rec_left',   '    int left = fib(n - 1);'],
      ['c_rec_right',  '    int right = fib(n - 2);'],
      ['c_ret_sum',    '    return left + right;'],
      ['',             '}'],
      ['',             ''],
      ['',             'int main() {'],
      ['c_input',      '    int n;'],
      ['c_input',      '    scanf("%d", &n);'],
      ['c_main_call',  '    int result = fib(n);'],
      ['c_main_ret',   '    printf("%d\\n", result);'],
      ['',             '    return 0;'],
      ['',             '}']
    ],
    cpp: [
      ['',             '#include <iostream>'],
      ['',             'using namespace std;'],
      ['',             ''],
      ['c_entry',      'int fib(int n) {'],
      ['c_base_check', '    if (n <= 1) {'],
      ['c_ret_base',   '        return n;'],
      ['',             '    }'],
      ['c_rec_left',   '    int left = fib(n - 1);'],
      ['c_rec_right',  '    int right = fib(n - 2);'],
      ['c_ret_sum',    '    return left + right;'],
      ['',             '}'],
      ['',             ''],
      ['',             'int main() {'],
      ['c_input',      '    int n;'],
      ['c_input',      '    cin >> n;'],
      ['c_main_call',  '    int result = fib(n);'],
      ['c_main_ret',   '    cout << result << endl;'],
      ['',             '    return 0;'],
      ['',             '}']
    ],
    python: [
      ['c_entry',      'def fib(n):'],
      ['c_base_check', '    if n <= 1:'],
      ['c_ret_base',   '        return n'],
      ['c_rec_left',   '    left = fib(n - 1)'],
      ['c_rec_right',  '    right = fib(n - 2)'],
      ['c_ret_sum',    '    return left + right'],
      ['',             ''],
      ['',             'if __name__ == "__main__":'],
      ['c_input',      '    n = int(input())'],
      ['c_main_call',  '    result = fib(n)'],
      ['c_main_ret',   '    print(result)']
    ],
    javascript: [
      ['c_entry',      'function fib(n) {'],
      ['c_base_check', '  if (n <= 1) {'],
      ['c_ret_base',   '    return n;'],
      ['',             '  }'],
      ['c_rec_left',   '  const left = fib(n - 1);'],
      ['c_rec_right',  '  const right = fib(n - 2);'],
      ['c_ret_sum',    '  return left + right;'],
      ['',             '}'],
      ['',             ''],
      ['c_input',      'const readline = require("readline").createInterface({ input: process.stdin, output: process.stdout });'],
      ['c_input',      'readline.question("", input => {'],
      ['c_input',      '  const n = parseInt(input);'],
      ['c_main_call',  '  const result = fib(n);'],
      ['c_main_ret',   '  console.log(result);'],
      ['',             '  readline.close();'],
      ['',             '});']
    ]
  },
  memoization: {
    java: [
      ['',             'import java.util.Scanner;'],
      ['',             'import java.util.Arrays;'],
      ['',             ''],
      ['',             'public class Main {'],
      ['c_entry',      '    static int fib(int n, int[] memo) {'],
      ['c_base_check', '        if (n <= 1) return n;'],
      ['c_memo_check', '        if (memo[n] != -1) return memo[n]; // Cache Hit'],
      ['c_rec_left',   '        int left = fib(n - 1, memo);'],
      ['c_rec_right',  '        int right = fib(n - 2, memo);'],
      ['c_ret_sum',    '        memo[n] = left + right;'],
      ['c_dp_ret',     '        return memo[n];'],
      ['',             '    }'],
      ['',             ''],
      ['',             '    public static void main(String[] args) {'],
      ['c_input',      '        Scanner sc = new Scanner(System.in);'],
      ['c_input',      '        int n = sc.nextInt();'],
      ['c_memo_alloc', '        int[] memo = new int[n + 1];'],
      ['c_memo_alloc', '        Arrays.fill(memo, -1);'],
      ['c_main_call',  '        int result = fib(n, memo);'],
      ['c_main_ret',   '        System.out.println(result);'],
      ['',             '    }'],
      ['',             '}']
    ],
    c: [
      ['',             '#include <stdio.h>'],
      ['',             '#include <string.h>'],
      ['',             ''],
      ['c_entry',      'int fib(int n, int memo[]) {'],
      ['c_base_check', '    if (n <= 1) return n;'],
      ['c_memo_check', '    if (memo[n] != -1) return memo[n]; // Cache Hit'],
      ['c_rec_left',   '    int left = fib(n - 1, memo);'],
      ['c_rec_right',  '    int right = fib(n - 2, memo);'],
      ['c_ret_sum',    '    memo[n] = left + right;'],
      ['c_dp_ret',     '    return memo[n];'],
      ['',             '}'],
      ['',             ''],
      ['',             'int main() {'],
      ['c_input',      '    int n;'],
      ['c_input',      '    scanf("%d", &n);'],
      ['c_memo_alloc', '    int memo[n + 1];'],
      ['c_memo_alloc', '    memset(memo, -1, sizeof(memo));'],
      ['c_main_call',  '    int result = fib(n, memo);'],
      ['c_main_ret',   '    printf("%d\\n", result);'],
      ['',             '    return 0;'],
      ['',             '}']
    ],
    cpp: [
      ['',             '#include <iostream>'],
      ['',             '#include <vector>'],
      ['',             'using namespace std;'],
      ['',             ''],
      ['c_entry',      'int fib(int n, vector<int>& memo) {'],
      ['c_base_check', '    if (n <= 1) return n;'],
      ['c_memo_check', '    if (memo[n] != -1) return memo[n]; // Cache Hit'],
      ['c_rec_left',   '    int left = fib(n - 1, memo);'],
      ['c_rec_right',  '    int right = fib(n - 2, memo);'],
      ['c_ret_sum',    '    memo[n] = left + right;'],
      ['c_dp_ret',     '    return memo[n];'],
      ['',             '}'],
      ['',             ''],
      ['',             'int main() {'],
      ['c_input',      '    int n;'],
      ['c_input',      '    cin >> n;'],
      ['c_memo_alloc', '    vector<int> memo(n + 1, -1);'],
      ['c_main_call',  '    int result = fib(n, memo);'],
      ['c_main_ret',   '    cout << result << endl;'],
      ['',             '    return 0;'],
      ['',             '}']
    ],
    python: [
      ['c_entry',      'def fib(n, memo):'],
      ['c_base_check', '    if n <= 1: return n'],
      ['c_memo_check', '    if n in memo: return memo[n]  # Cache Hit'],
      ['c_rec_left',   '    left = fib(n - 1, memo)'],
      ['c_rec_right',  '    right = fib(n - 2, memo)'],
      ['c_ret_sum',    '    memo[n] = left + right'],
      ['c_dp_ret',     '    return memo[n]'],
      ['',             ''],
      ['',             'if __name__ == "__main__":'],
      ['c_input',      '    n = int(input())'],
      ['c_memo_alloc', '    memo = {}'],
      ['c_main_call',  '    result = fib(n, memo)'],
      ['c_main_ret',   '    print(result)']
    ],
    javascript: [
      ['c_entry',      'function fib(n, memo) {'],
      ['c_base_check', '  if (n <= 1) return n;'],
      ['c_memo_check', '  if (memo[n] !== -1) return memo[n]; // Cache Hit'],
      ['c_rec_left',   '  const left = fib(n - 1, memo);'],
      ['c_rec_right',  '  const right = fib(n - 2, memo);'],
      ['c_ret_sum',    '  memo[n] = left + right;'],
      ['c_dp_ret',     '  return memo[n];'],
      ['',             '}'],
      ['',             ''],
      ['c_input',      'const readline = require("readline").createInterface({ input: process.stdin, output: process.stdout });'],
      ['c_input',      'readline.question("", input => {'],
      ['c_input',      '  const n = parseInt(input);'],
      ['c_memo_alloc', '  const memo = new Array(n + 1).fill(-1);'],
      ['c_main_call',  '  const result = fib(n, memo);'],
      ['c_main_ret',   '  console.log(result);'],
      ['',             '  readline.close();'],
      ['',             '});']
    ]
  },
  tabulation: {
    java: [
      ['',             'import java.util.Scanner;'],
      ['',             ''],
      ['',             'public class Main {'],
      ['c_entry',      '    static int fib(int n) {'],
      ['c_base_check', '        if (n <= 1) return n;'],
      ['c_dp_alloc',   '        int[] dp = new int[n + 1];'],
      ['c_dp_base0',   '        dp[0] = 0;'],
      ['c_dp_base1',   '        dp[1] = 1;'],
      ['c_loop',       '        for (int i = 2; i <= n; i++) {'],
      ['c_dp_calc',    '            dp[i] = dp[i - 1] + dp[i - 2];'],
      ['',             '        }'],
      ['c_dp_ret',     '        return dp[n];'],
      ['',             '    }'],
      ['',             ''],
      ['',             '    public static void main(String[] args) {'],
      ['c_input',      '        Scanner sc = new Scanner(System.in);'],
      ['c_input',      '        int n = sc.nextInt();'],
      ['c_main_call',  '        int result = fib(n);'],
      ['c_main_ret',   '        System.out.println(result);'],
      ['',             '    }'],
      ['',             '}']
    ],
    c: [
      ['',             '#include <stdio.h>'],
      ['',             '#include <stdlib.h>'],
      ['',             ''],
      ['c_entry',      'int fib(int n) {'],
      ['c_base_check', '    if (n <= 1) return n;'],
      ['c_dp_alloc',   '    int dp[n + 1];'],
      ['c_dp_base0',   '    dp[0] = 0;'],
      ['c_dp_base1',   '    dp[1] = 1;'],
      ['c_loop',       '    for (int i = 2; i <= n; i++) {'],
      ['c_dp_calc',    '        dp[i] = dp[i - 1] + dp[i - 2];'],
      ['',             '    }'],
      ['c_dp_ret',     '    return dp[n];'],
      ['',             '}'],
      ['',             ''],
      ['',             'int main() {'],
      ['c_input',      '    int n;'],
      ['c_input',      '    scanf("%d", &n);'],
      ['c_main_call',  '    int result = fib(n);'],
      ['c_main_ret',   '    printf("%d\\n", result);'],
      ['',             '    return 0;'],
      ['',             '}']
    ],
    cpp: [
      ['',             '#include <iostream>'],
      ['',             '#include <vector>'],
      ['',             'using namespace std;'],
      ['',             ''],
      ['c_entry',      'int fib(int n) {'],
      ['c_base_check', '    if (n <= 1) return n;'],
      ['c_dp_alloc',   '    vector<int> dp(n + 1);'],
      ['c_dp_base0',   '    dp[0] = 0;'],
      ['c_dp_base1',   '    dp[1] = 1;'],
      ['c_loop',       '    for (int i = 2; i <= n; i++) {'],
      ['c_dp_calc',    '        dp[i] = dp[i - 1] + dp[i - 2];'],
      ['',             '    }'],
      ['c_dp_ret',     '    return dp[n];'],
      ['',             '}'],
      ['',             ''],
      ['',             'int main() {'],
      ['c_input',      '    int n;'],
      ['c_input',      '    cin >> n;'],
      ['c_main_call',  '    int result = fib(n);'],
      ['c_main_ret',   '    cout << result << endl;'],
      ['',             '    return 0;'],
      ['',             '}']
    ],
    python: [
      ['c_entry',      'def fib(n):'],
      ['c_base_check', '    if n <= 1: return n'],
      ['c_dp_alloc',   '    dp = [0] * (n + 1)'],
      ['c_dp_base0',   '    dp[0] = 0'],
      ['c_dp_base1',   '    dp[1] = 1'],
      ['c_loop',       '    for i in range(2, n + 1):'],
      ['c_dp_calc',    '        dp[i] = dp[i - 1] + dp[i - 2]'],
      ['c_dp_ret',     '    return dp[n]'],
      ['',             ''],
      ['',             'if __name__ == "__main__":'],
      ['c_input',      '    n = int(input())'],
      ['c_main_call',  '    result = fib(n)'],
      ['c_main_ret',   '    print(result)']
    ],
    javascript: [
      ['c_entry',      'function fib(n) {'],
      ['c_base_check', '  if (n <= 1) return n;'],
      ['c_dp_alloc',   '  const dp = new Array(n + 1);'],
      ['c_dp_base0',   '  dp[0] = 0;'],
      ['c_dp_base1',   '  dp[1] = 1;'],
      ['c_loop',       '  for (let i = 2; i <= n; i++) {'],
      ['c_dp_calc',    '    dp[i] = dp[i - 1] + dp[i - 2];'],
      ['',             '  }'],
      ['c_dp_ret',     '  return dp[n];'],
      ['',             '}'],
      ['',             ''],
      ['c_input',      'const readline = require("readline").createInterface({ input: process.stdin, output: process.stdout });'],
      ['c_input',      'readline.question("", input => {'],
      ['c_input',      '  const n = parseInt(input);'],
      ['c_main_call',  '  const result = fib(n);'],
      ['c_main_ret',   '  console.log(result);'],
      ['',             '  readline.close();'],
      ['',             '});']
    ]
  }
};

const PSEUDOCODES = {
  recursion: [
    'function fib(n):',
    '    if n <= 1:',
    '        return n                 // Base cases: fib(0) = 0, fib(1) = 1',
    '    left = fib(n - 1)            // Recursive call for (n - 1)',
    '    right = fib(n - 2)           // Recursive call for (n - 2)',
    '    return left + right          // Overlapping subproblems recomputed!'
  ],
  memoization: [
    'memo = array/map initialized with -1',
    'function fib(n, memo):',
    '    if n <= 1: return n',
    '    if memo[n] != -1:            // Cache lookup (O(1) hit)',
    '        return memo[n]',
    '    memo[n] = fib(n - 1, memo) + fib(n - 2, memo) // Cache result',
    '    return memo[n]'
  ],
  tabulation: [
    'function fib(n):',
    '    if n <= 1: return n',
    '    dp = array of size (n + 1)   // Allocate table',
    '    dp[0] = 0; dp[1] = 1         // Seed base cases',
    '    for i = 2 to n:              // Fill table iteratively bottom-up',
    '        dp[i] = dp[i - 1] + dp[i - 2]',
    '    return dp[n]                 // O(n) time, single pass'
  ]
};

function frame(title, rows) {
  return { title, rows };
}

/* ==================================================================== */
/* STATIC FULL-TREE PRECOMPUTATION & BALANCED LAYOUT ENGINE            */
/* ==================================================================== */
function generateStaticTree(approach, targetN) {
  const n = Math.max(0, parseInt(targetN, 10) || 0);
  const nodes = [];
  const edges = [];
  let nextId = 0;
  const seenMap = {};

  if (approach === 'recursion') {
    function build(k, parentId, isLeft) {
      const id = nextId++;
      const isRedundant = (seenMap[k] || 0) > 0 && k > 1;
      seenMap[k] = (seenMap[k] || 0) + 1;

      const node = { id, k, parentId, isLeft, isRedundant, children: [] };
      nodes.push(node);

      if (parentId !== null) {
        edges.push({ from: parentId, to: id, isLeft });
      }

      if (k > 1) {
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

      if (k <= 1) {
        memo[k] = k;
      } else if (isCacheHit) {
        // Pruned subtree — no children spawned!
      } else {
        const leftId = buildMemo(k - 1, id, true);
        const rightId = buildMemo(k - 2, id, false);
        node.children = [leftId, rightId];
        memo[k] = 1; // Mark computed
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
  const rawN = Math.max(0, parseInt(targetN, 10) || 0);
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
      badge: `User input: n = ${n}. Starting naive recursive Fibonacci execution.`,
      code: 'c_input',
      vars: [frame('main()', [['n', String(n)]])],
      state: {
        approach: 'recursion',
        n,
        curN: n,
        totalCalls: 0,
        redundantCalls: 0,
        currentReturn: null,
        treeNodes: [],
        treeEdges: [],
        treeWidth: staticTree.width,
        treeHeight: staticTree.height,
        activeNodeId: null
      }
    });

    // Step 1: Main calls fib(n)
    steps.push({
      badge: `main(): Invoking fib(${n}).`,
      code: 'c_main_call',
      vars: [frame('main()', [['n', String(n)]])],
      state: {
        approach: 'recursion',
        n,
        curN: n,
        totalCalls: 0,
        redundantCalls: 0,
        currentReturn: null,
        treeNodes: [],
        treeEdges: [],
        treeWidth: staticTree.width,
        treeHeight: staticTree.height,
        activeNodeId: null
      }
    });

    function solveRecursion(currentN, parentId, isLeft) {
      const nodeId = nextNodeId++;
      callCounter++;

      const isSubproblemComputed = (computedValuesMap[currentN] || 0) > 0;
      if (isSubproblemComputed && currentN > 1) {
        redundantCounter++;
      }

      const nodeObj = nodeStateMap[nodeId];
      nodeObj.state = 'active';

      const currentStack = [...stackFrames, { title: `fib(${currentN})`, rows: [['n', String(currentN)]] }];

      // Step: Function call entry
      steps.push({
        badge: isSubproblemComputed && currentN > 1
          ? `⚠️ Overlapping subproblem! fib(${currentN}) was already computed earlier! Naive recursion re-calculates it.`
          : `Entering fib(${currentN}). Function call #${callCounter}.`,
        code: 'c_entry',
        vars: currentStack.map(f => frame(f.title, f.rows)),
        state: {
          approach: 'recursion',
          n,
          curN: currentN,
          totalCalls: callCounter,
          redundantCalls: redundantCounter,
          currentReturn: null,
          treeNodes: getVisibleNodes().map(nd => ({ ...nd })),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height,
          activeNodeId: nodeId
        }
      });

      // Step: Base case check
      steps.push({
        badge: `Checking base case: n (${currentN}) <= 1 is ${currentN <= 1 ? 'TRUE (Base Case Hit)' : 'FALSE (Proceed to recurse)'}.`,
        code: 'c_base_check',
        vars: currentStack.map(f => frame(f.title, f.rows)),
        state: {
          approach: 'recursion',
          n,
          curN: currentN,
          totalCalls: callCounter,
          redundantCalls: redundantCounter,
          currentReturn: null,
          treeNodes: getVisibleNodes().map(nd => ({ ...nd })),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height,
          activeNodeId: nodeId
        }
      });

      if (currentN <= 1) {
        nodeObj.state = 'solved';
        nodeObj.retVal = currentN;
        computedValuesMap[currentN] = (computedValuesMap[currentN] || 0) + 1;

        const retStack = currentStack.map((f, idx) => {
          if (idx === currentStack.length - 1) {
            return { title: f.title, rows: [['n', String(currentN)], ['return', String(currentN)]] };
          }
          return f;
        });

        steps.push({
          badge: `fib(${currentN}) is base case → returns ${currentN}.`,
          code: 'c_ret_base',
          vars: retStack.map(f => frame(f.title, f.rows)),
          state: {
            approach: 'recursion',
            n,
            curN: currentN,
            totalCalls: callCounter,
            redundantCalls: redundantCounter,
            currentReturn: currentN,
            treeNodes: getVisibleNodes().map(nd => ({ ...nd })),
            treeEdges: getVisibleEdges(),
            treeWidth: staticTree.width,
            treeHeight: staticTree.height,
            activeNodeId: nodeId
          }
        });

        return currentN;
      }

      // Step: Left recursive call
      steps.push({
        badge: `fib(${currentN}): Computing left child fib(${currentN - 1}).`,
        code: 'c_rec_left',
        vars: currentStack.map(f => frame(f.title, f.rows)),
        state: {
          approach: 'recursion',
          n,
          curN: currentN,
          totalCalls: callCounter,
          redundantCalls: redundantCounter,
          currentReturn: null,
          treeNodes: getVisibleNodes().map(nd => ({ ...nd })),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height,
          activeNodeId: nodeId
        }
      });

      stackFrames.push({ title: `fib(${currentN})`, rows: [['n', String(currentN)]] });
      const leftVal = solveRecursion(currentN - 1, nodeId, true);
      stackFrames.pop();

      // Step: Right recursive call
      const afterLeftStack = [...stackFrames, { title: `fib(${currentN})`, rows: [['n', String(currentN)], ['left', String(leftVal)]] }];
      steps.push({
        badge: `fib(${currentN}): Left returned ${leftVal}. Now computing right child fib(${currentN - 2}).`,
        code: 'c_rec_right',
        vars: afterLeftStack.map(f => frame(f.title, f.rows)),
        state: {
          approach: 'recursion',
          n,
          curN: currentN,
          totalCalls: callCounter,
          redundantCalls: redundantCounter,
          currentReturn: leftVal,
          treeNodes: getVisibleNodes().map(nd => ({ ...nd })),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height,
          activeNodeId: nodeId
        }
      });

      stackFrames.push({ title: `fib(${currentN})`, rows: [['n', String(currentN)], ['left', String(leftVal)]] });
      const rightVal = solveRecursion(currentN - 2, nodeId, false);
      stackFrames.pop();

      // Step: Summation and return
      const totalVal = leftVal + rightVal;
      nodeObj.state = 'solved';
      nodeObj.retVal = totalVal;
      computedValuesMap[currentN] = (computedValuesMap[currentN] || 0) + 1;

      const finishStack = [...stackFrames, {
        title: `fib(${currentN})`,
        rows: [['n', String(currentN)], ['left', String(leftVal)], ['right', String(rightVal)], ['return', String(totalVal)]]
      }];

      steps.push({
        badge: `fib(${currentN}) = left (${leftVal}) + right (${rightVal}) = ${totalVal}. Storing & returning ${totalVal}.`,
        code: 'c_ret_sum',
        vars: finishStack.map(f => frame(f.title, f.rows)),
        state: {
          approach: 'recursion',
          n,
          curN: currentN,
          totalCalls: callCounter,
          redundantCalls: redundantCounter,
          currentReturn: totalVal,
          treeNodes: getVisibleNodes().map(nd => ({ ...nd })),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height,
          activeNodeId: nodeId
        }
      });

      return totalVal;
    }

    const finalAnswer = solveRecursion(n, null, false);

    // Final main return step
    steps.push({
      badge: `Main Execution Complete! fib(${n}) = ${finalAnswer}. Total function calls: ${callCounter}.`,
      code: 'c_main_ret',
      vars: [
        frame('main()', [['n', String(n)], ['result', String(finalAnswer)]])
      ],
      state: {
        approach: 'recursion',
        n,
        curN: n,
        totalCalls: callCounter,
        redundantCalls: redundantCounter,
        currentReturn: finalAnswer,
        treeNodes: getVisibleNodes().map(nd => ({ ...nd })),
        treeEdges: getVisibleEdges(),
        treeWidth: staticTree.width,
        treeHeight: staticTree.height,
        activeNodeId: 0
      }
    });

    return steps;
  }

  /* ------------------------------------------------------------------ */
  /* APPROACH 2: DP MEMOIZATION (TOP-DOWN WITH CACHE)                   */
  /* ------------------------------------------------------------------ */
  if (approach === 'memoization') {
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

    const stackFrames = [];
    const memo = new Array(n + 1).fill(-1);
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
      badge: `User input: n = ${n}. Starting Top-Down DP with Memoization.`,
      code: 'c_input',
      vars: [frame('main()', [['n', String(n)]])],
      state: {
        approach: 'memoization',
        n,
        curN: n,
        totalCalls: 0,
        cacheHits: 0,
        currentReturn: null,
        treeNodes: [],
        treeEdges: [],
        treeWidth: staticTree.width,
        treeHeight: staticTree.height,
        memo: [...memo],
        activeNodeId: null
      }
    });

    // Step 1: Allocate memo cache
    steps.push({
      badge: `Allocating memo cache table memo[0...${n}] initialized with -1 (uncached).`,
      code: 'c_memo_alloc',
      vars: [frame('main()', [['n', String(n)], ['memo.length', String(n + 1)]])],
      state: {
        approach: 'memoization',
        n,
        curN: n,
        totalCalls: 0,
        cacheHits: 0,
        currentReturn: null,
        treeNodes: [],
        treeEdges: [],
        treeWidth: staticTree.width,
        treeHeight: staticTree.height,
        memo: [...memo],
        activeNodeId: null
      }
    });

    // Step 2: Main calls fib(n, memo)
    steps.push({
      badge: `main(): Invoking fib(${n}, memo).`,
      code: 'c_main_call',
      vars: [frame('main()', [['n', String(n)]])],
      state: {
        approach: 'memoization',
        n,
        curN: n,
        totalCalls: 0,
        cacheHits: 0,
        currentReturn: null,
        treeNodes: [],
        treeEdges: [],
        treeWidth: staticTree.width,
        treeHeight: staticTree.height,
        memo: [...memo],
        activeNodeId: null
      }
    });

    function solveMemo(currentN, parentId, isLeft) {
      const nodeId = nextNodeId++;
      callCounter++;

      const isCacheHit = memo[currentN] !== -1;
      if (isCacheHit) cacheHits++;

      const nodeObj = nodeStateMap[nodeId];
      nodeObj.state = isCacheHit ? 'cache-hit' : 'active';
      nodeObj.retVal = isCacheHit ? memo[currentN] : null;

      const currentStack = [...stackFrames, { title: `fib(${currentN}, memo)`, rows: [['n', String(currentN)]] }];

      // Function Entry
      steps.push({
        badge: isCacheHit
          ? `🚀 Cache Hit! fib(${currentN}) was previously calculated. memo[${currentN}] = ${memo[currentN]}.`
          : `Entering fib(${currentN}, memo). Subproblem not yet in cache.`,
        code: 'c_entry',
        vars: currentStack.map(f => frame(f.title, f.rows)),
        state: {
          approach: 'memoization',
          n,
          curN: currentN,
          totalCalls: callCounter,
          cacheHits,
          currentReturn: null,
          treeNodes: getVisibleNodes().map(nd => ({ ...nd })),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height,
          memo: [...memo],
          activeNodeId: nodeId
        }
      });

      // Base case check
      steps.push({
        badge: `Checking base case: n (${currentN}) <= 1 is ${currentN <= 1 ? 'TRUE' : 'FALSE'}.`,
        code: 'c_base_check',
        vars: currentStack.map(f => frame(f.title, f.rows)),
        state: {
          approach: 'memoization',
          n,
          curN: currentN,
          totalCalls: callCounter,
          cacheHits,
          currentReturn: null,
          treeNodes: getVisibleNodes().map(nd => ({ ...nd })),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height,
          memo: [...memo],
          activeNodeId: nodeId
        }
      });

      if (currentN <= 1) {
        memo[currentN] = currentN;
        nodeObj.state = 'solved';
        nodeObj.retVal = currentN;

        steps.push({
          badge: `fib(${currentN}) is base case → returns ${currentN}. Stored in memo[${currentN}] = ${currentN}.`,
          code: 'c_ret_base',
          vars: currentStack.map((f, idx) => idx === currentStack.length - 1
            ? frame(f.title, [['n', String(currentN)], ['return', String(currentN)]])
            : frame(f.title, f.rows)
          ),
          state: {
            approach: 'memoization',
            n,
            curN: currentN,
            totalCalls: callCounter,
            cacheHits,
            currentReturn: currentN,
            treeNodes: getVisibleNodes().map(nd => ({ ...nd })),
            treeEdges: getVisibleEdges(),
            treeWidth: staticTree.width,
            treeHeight: staticTree.height,
            memo: [...memo],
            activeNodeId: nodeId
          }
        });

        return currentN;
      }

      // Check Memo Cache Hit
      steps.push({
        badge: `Checking memo[${currentN}] != -1: ${isCacheHit ? `TRUE (Cached value is ${memo[currentN]})` : 'FALSE (Not cached)'}.`,
        code: 'c_memo_check',
        vars: currentStack.map(f => frame(f.title, f.rows)),
        state: {
          approach: 'memoization',
          n,
          curN: currentN,
          totalCalls: callCounter,
          cacheHits,
          currentReturn: null,
          treeNodes: getVisibleNodes().map(nd => ({ ...nd })),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height,
          memo: [...memo],
          activeNodeId: nodeId
        }
      });

      if (isCacheHit) {
        steps.push({
          badge: `⚡ Instant O(1) Return! Returning cached value memo[${currentN}] = ${memo[currentN]} without expanding branch!`,
          code: 'c_memo_check',
          vars: currentStack.map((f, idx) => idx === currentStack.length - 1
            ? frame(f.title, [['n', String(currentN)], ['cache_return', String(memo[currentN])]])
            : frame(f.title, f.rows)
          ),
          state: {
            approach: 'memoization',
            n,
            curN: currentN,
            totalCalls: callCounter,
            cacheHits,
            currentReturn: memo[currentN],
            treeNodes: getVisibleNodes().map(nd => ({ ...nd })),
            treeEdges: getVisibleEdges(),
            treeWidth: staticTree.width,
            treeHeight: staticTree.height,
            memo: [...memo],
            activeNodeId: nodeId
          }
        });

        return memo[currentN];
      }

      // Left recursive call
      steps.push({
        badge: `fib(${currentN}): Recursing left child fib(${currentN - 1}, memo).`,
        code: 'c_rec_left',
        vars: currentStack.map(f => frame(f.title, f.rows)),
        state: {
          approach: 'memoization',
          n,
          curN: currentN,
          totalCalls: callCounter,
          cacheHits,
          currentReturn: null,
          treeNodes: getVisibleNodes().map(nd => ({ ...nd })),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height,
          memo: [...memo],
          activeNodeId: nodeId
        }
      });

      stackFrames.push({ title: `fib(${currentN})`, rows: [['n', String(currentN)]] });
      const leftVal = solveMemo(currentN - 1, nodeId, true);
      stackFrames.pop();

      // Right recursive call
      const afterLeftStack = [...stackFrames, { title: `fib(${currentN})`, rows: [['n', String(currentN)], ['left', String(leftVal)]] }];
      steps.push({
        badge: `fib(${currentN}): Left returned ${leftVal}. Recursing right child fib(${currentN - 2}, memo).`,
        code: 'c_rec_right',
        vars: afterLeftStack.map(f => frame(f.title, f.rows)),
        state: {
          approach: 'memoization',
          n,
          curN: currentN,
          totalCalls: callCounter,
          cacheHits,
          currentReturn: leftVal,
          treeNodes: getVisibleNodes().map(nd => ({ ...nd })),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height,
          memo: [...memo],
          activeNodeId: nodeId
        }
      });

      stackFrames.push({ title: `fib(${currentN})`, rows: [['n', String(currentN)], ['left', String(leftVal)]] });
      const rightVal = solveMemo(currentN - 2, nodeId, false);
      stackFrames.pop();

      // Summation & memo storage
      const totalVal = leftVal + rightVal;
      memo[currentN] = totalVal;
      nodeObj.state = 'solved';
      nodeObj.retVal = totalVal;

      const finishStack = [...stackFrames, {
        title: `fib(${currentN})`,
        rows: [['n', String(currentN)], ['left', String(leftVal)], ['right', String(rightVal)], ['memo[n]', String(totalVal)]]
      }];

      steps.push({
        badge: `memo[${currentN}] = ${leftVal} + ${rightVal} = ${totalVal}. Result cached into memo table!`,
        code: 'c_ret_sum',
        vars: finishStack.map(f => frame(f.title, f.rows)),
        state: {
          approach: 'memoization',
          n,
          curN: currentN,
          totalCalls: callCounter,
          cacheHits,
          currentReturn: totalVal,
          treeNodes: getVisibleNodes().map(nd => ({ ...nd })),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height,
          memo: [...memo],
          activeNodeId: nodeId
        }
      });

      // Return
      steps.push({
        badge: `Returning memo[${currentN}] = ${totalVal}.`,
        code: 'c_dp_ret',
        vars: finishStack.map((f, idx) => idx === finishStack.length - 1
          ? frame(f.title, [['n', String(currentN)], ['return', String(totalVal)]])
          : frame(f.title, f.rows)
        ),
        state: {
          approach: 'memoization',
          n,
          curN: currentN,
          totalCalls: callCounter,
          cacheHits,
          currentReturn: totalVal,
          treeNodes: getVisibleNodes().map(nd => ({ ...nd })),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height,
          memo: [...memo],
          activeNodeId: nodeId
        }
      });

      return totalVal;
    }

    const finalAnswer = solveMemo(n, null, false);

    // Final Completion Step
    steps.push({
      badge: `Memoization Complete! fib(${n}) = ${finalAnswer}. Solved in O(n) calls with ${cacheHits} O(1) cache hits!`,
      code: 'c_main_ret',
      vars: [
        frame('main()', [['n', String(n)], ['result', String(finalAnswer)]])
      ],
      state: {
        approach: 'memoization',
        n,
        curN: n,
        totalCalls: callCounter,
        cacheHits,
        currentReturn: finalAnswer,
        treeNodes: getVisibleNodes().map(nd => ({ ...nd })),
        treeEdges: getVisibleEdges(),
        treeWidth: staticTree.width,
        treeHeight: staticTree.height,
        memo: [...memo],
        activeNodeId: 0
      }
    });

    return steps;
  }

  /* ------------------------------------------------------------------ */
  /* APPROACH 3: DP TABULATION (BOTTOM-UP 1D ARRAY)                     */
  /* ------------------------------------------------------------------ */
  if (approach === 'tabulation') {
    const dpCells = Array.from({ length: n + 1 }, (_, idx) => ({
      idx,
      val: '?',
      state: 'uncalc'
    }));

    // Step 0: Input ingestion
    // Step 0: Input read
    steps.push({
      badge: `User input: n = ${n}. Starting Bottom-Up Tabulation.`,
      code: 'c_input',
      vars: [frame('main()', [['n', String(n)]])],
      state: {
        approach: 'tabulation',
        n,
        iterations: 0,
        totalOps: 0,
        i: -1,
        dpCells: dpCells.map(c => ({ ...c })),
        currentReturn: null
      }
    });

    // Step 1: Main calls fib(n)
    steps.push({
      badge: `main(): Invoking fib(${n}).`,
      code: 'c_main_call',
      vars: [frame('main()', [['n', String(n)]])],
      state: {
        approach: 'tabulation',
        n,
        iterations: 0,
        totalOps: 0,
        i: -1,
        dpCells: dpCells.map(c => ({ ...c })),
        currentReturn: null
      }
    });

    // Step 2: Function entry
    steps.push({
      badge: `Entering fib(n = ${n}).`,
      code: 'c_entry',
      vars: [frame('main()', [['n', String(n)]]), frame('fib(n)', [['n', String(n)]])],
      state: {
        approach: 'tabulation',
        n,
        iterations: 0,
        totalOps: 1,
        i: -1,
        dpCells: dpCells.map(c => ({ ...c })),
        currentReturn: null
      }
    });

    // Step 3: Base check for n <= 1
    steps.push({
      badge: `if (n <= 1) → check ${n} <= 1: ${n <= 1 ? 'TRUE (Direct return)' : 'FALSE (Proceed to table allocation)'}.`,
      code: 'c_base_check',
      vars: [frame('main()', [['n', String(n)]]), frame('fib(n)', [['n', String(n)]])],
      state: {
        approach: 'tabulation',
        n,
        iterations: 0,
        totalOps: 1,
        i: -1,
        dpCells: dpCells.map(c => ({ ...c })),
        currentReturn: null
      }
    });

    if (n <= 1) {
      dpCells[n] = { idx: n, val: n, state: 'solved' };
      steps.push({
        badge: `n = ${n} is a base case → return ${n}.`,
        code: 'c_base_check',
        vars: [frame('main()', [['n', String(n)]]), frame('fib(n)', [['n', String(n)], ['return', String(n)]])],
        state: {
          approach: 'tabulation',
          n,
          iterations: 0,
          totalOps: 1,
          i: n,
          dpCells: dpCells.map(c => ({ ...c })),
          currentReturn: n
        }
      });
      return steps;
    }

    // Step 4: Allocate DP array
    steps.push({
      badge: `Allocating 1D DP table dp[0...${n}] of size ${n + 1}.`,
      code: 'c_dp_alloc',
      vars: [frame('main()', [['n', String(n)]]), frame('fib(n)', [['n', String(n)], ['dp.length', String(n + 1)]])],
      state: {
        approach: 'tabulation',
        n,
        iterations: 0,
        totalOps: 2,
        i: -1,
        dpCells: dpCells.map(c => ({ ...c })),
        currentReturn: null
      }
    });

    // Step 5: dp[0] = 0
    dpCells[0] = { idx: 0, val: 0, state: 'base' };
    steps.push({
      badge: 'Setting base case dp[0] = 0.',
      code: 'c_dp_base0',
      vars: [frame('main()', [['n', String(n)]]), frame('fib(n)', [['n', String(n)], ['dp[0]', '0']])],
      state: {
        approach: 'tabulation',
        n,
        iterations: 0,
        totalOps: 3,
        i: 0,
        dpCells: dpCells.map(c => ({ ...c })),
        currentReturn: null
      }
    });

    // Step 6: dp[1] = 1
    dpCells[1] = { idx: 1, val: 1, state: 'base' };
    steps.push({
      badge: 'Setting base case dp[1] = 1.',
      code: 'c_dp_base1',
      vars: [frame('main()', [['n', String(n)]]), frame('fib(n)', [['n', String(n)], ['dp[0]', '0'], ['dp[1]', '1']])],
      state: {
        approach: 'tabulation',
        n,
        iterations: 0,
        totalOps: 4,
        i: 1,
        dpCells: dpCells.map(c => ({ ...c })),
        currentReturn: null
      }
    });

    // Step 7: Loop from i = 2 to n
    let ops = 4;
    for (let i = 2; i <= n; i++) {
      ops++;
      const currentIter = i - 1;
      steps.push({
        badge: `for (i = ${i}; i <= ${n}; i++) → Condition TRUE (Iteration #${currentIter}). Computing dp[${i}] from dp[${i - 1}] and dp[${i - 2}].`,
        code: 'c_loop',
        vars: [
          frame('main()', [['n', String(n)]]),
          frame('fib(n)', [
            ['n', String(n)],
            ['i', String(i)],
            ['dp[i-2]', String(dpCells[i - 2].val)],
            ['dp[i-1]', String(dpCells[i - 1].val)]
          ])
        ],
        state: {
          approach: 'tabulation',
          n,
          iterations: currentIter,
          totalOps: ops,
          i,
          dpCells: dpCells.map((c, idx) => ({
            ...c,
            state: idx === i ? 'adding' : (idx < i ? 'solved' : 'uncalc')
          })),
          currentReturn: null
        }
      });

      ops++;
      const computedVal = dpCells[i - 1].val + dpCells[i - 2].val;
      dpCells[i] = { idx: i, val: computedVal, state: 'solved' };

      steps.push({
        badge: `dp[${i}] = dp[${i - 1}] (${dpCells[i - 1].val}) + dp[${i - 2}] (${dpCells[i - 2].val}) = ${computedVal} → Stored in dp[${i}].`,
        code: 'c_dp_calc',
        vars: [
          frame('main()', [['n', String(n)]]),
          frame('fib(n)', [
            ['n', String(n)],
            ['i', String(i)],
            ['dp[i-2]', String(dpCells[i - 2].val)],
            ['dp[i-1]', String(dpCells[i - 1].val)],
            ['dp[i]', String(computedVal)]
          ])
        ],
        state: {
          approach: 'tabulation',
          n,
          iterations: currentIter,
          totalOps: ops,
          i,
          dpCells: dpCells.map((c, idx) => ({
            ...c,
            state: idx <= i ? 'solved' : 'uncalc'
          })),
          currentReturn: computedVal
        }
      });
    }

    const totalIterations = Math.max(0, n - 1);

    // Step 8: Loop finished
    steps.push({
      badge: `for loop completed (i = ${n + 1} > ${n}) → Exit loop (${totalIterations} iterations).`,
      code: 'c_loop',
      vars: [
        frame('main()', [['n', String(n)]]),
        frame('fib(n)', [['n', String(n)], ['i', String(n + 1)], ['dp[n]', String(dpCells[n].val)]])
      ],
      state: {
        approach: 'tabulation',
        n,
        iterations: totalIterations,
        totalOps: ops + 1,
        i: n,
        dpCells: dpCells.map(c => ({ ...c })),
        currentReturn: dpCells[n].val
      }
    });

    // Step 9: Return dp[n]
    steps.push({
      badge: `return dp[${n}] = ${dpCells[n].val}. Tabulation computed in single pass (${totalIterations} iterations)!`,
      code: 'c_dp_ret',
      vars: [
        frame('main()', [['n', String(n)], ['result', String(dpCells[n].val)]]),
        frame('fib(n)', [['n', String(n)], ['return', String(dpCells[n].val)]])
      ],
      state: {
        approach: 'tabulation',
        n,
        iterations: totalIterations,
        totalOps: ops + 2,
        i: n,
        dpCells: dpCells.map(c => ({ ...c })),
        currentReturn: dpCells[n].val
      }
    });

    // Step 10: Print result in main
    steps.push({
      badge: `Main Execution Complete! fib(${n}) = ${dpCells[n].val}.`,
      code: 'c_main_ret',
      vars: [
        frame('main()', [['n', String(n)], ['result', String(dpCells[n].val)]])
      ],
      state: {
        approach: 'tabulation',
        n,
        iterations: totalIterations,
        totalOps: ops + 3,
        i: n,
        dpCells: dpCells.map(c => ({ ...c })),
        currentReturn: dpCells[n].val
      }
    });

    return steps;
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
  if (isNaN(val) || val < 0) val = 0;

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
  if (isNaN(val) || val < 0) val = 0;
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
    dragging = true;
    startX = e.clientX;
    startW = leftColRef.value.offsetWidth;
    rsz.classList.add('drag');
    document.body.style.userSelect = 'none';
  };
  const onMove = e => {
    if (!dragging) return;
    const mainW = main.offsetWidth;
    leftWidth.value = (Math.max(220, Math.min(mainW - 220, startW + e.clientX - startX)) / mainW) * 100;
  };
  const onUp = () => {
    if (!dragging) return;
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
  if (!rsz) return;
  let dragging = false, startY = 0, startH = 0;
  const onDown = e => {
    dragging = true;
    startY = e.clientY;
    startH = valueRef.value;
    rsz.classList.add('drag');
    document.body.style.userSelect = 'none';
    e.preventDefault();
  };
  const onMove = e => {
    if (!dragging) return;
    valueRef.value = Math.max(minH, Math.min(maxH, startH + (e.clientY - startY)));
  };
  const onUp = () => {
    if (!dragging) return;
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
  cleanupFns.push(initVResizer(vizResizerRef, vizHeight, 130, 360));
  cleanupFns.push(initVResizer(tableResizerRef, tableHeight, 30, 140));
});

onBeforeUnmount(() => {
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
                :min="0"
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
                      Return: <b class="ll-c-green">{{ st.currentReturn }}</b>
                    </div>
                  </div>

                  <!-- Visual Diagram 1: DP Tabulation (Bottom-Up 1D Array) -->
                  <div v-if="currentApproach === 'tabulation'" class="ll-dp-tab-view">
                    <div class="ll-section-caption">
                      <span>1D Tabulation Array Strip &mdash; <code>dp[0...{{ st.n }}]</code></span>
                      <span v-if="st.i >= 2 && st.i <= st.n" class="ll-calc-pill">
                        dp[{{ st.i }}] = dp[{{ st.i - 1 }}] + dp[{{ st.i - 2 }}]
                      </span>
                    </div>

                    <div class="ll-arr-track">
                      <template v-for="cell in st.dpCells" :key="cell.idx">
                        <div class="ll-arr-cell-wrap">
                          <!-- Pointer Tag Above Cell -->
                          <div class="ll-ptr-tag-wrap">
                            <span v-if="st.i >= 2 && cell.idx === st.i - 2" class="ll-ptr-lbl ll-lbl-purple">↓ i-2</span>
                            <span v-else-if="st.i >= 2 && cell.idx === st.i - 1" class="ll-ptr-lbl ll-lbl-orange">↓ i-1</span>
                            <span v-else-if="cell.idx === st.i" class="ll-ptr-lbl ll-lbl-green">↓ i</span>
                          </div>

                          <!-- Pastel Flat Card -->
                          <div
                            class="ll-arr-box"
                            :class="{
                              'll-box-base': cell.state === 'base',
                              'll-box-cur': cell.state === 'adding' || cell.idx === st.i,
                              'll-box-found': cell.state === 'solved',
                              'll-box-uncalc': cell.state === 'uncalc'
                            }"
                          >
                            {{ cell.val }}
                          </div>
                          <div class="ll-arr-idx">dp[{{ cell.idx }}]</div>
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
                            <!-- Node Text: fib(k) -->
                            <text x="0" y="-3" text-anchor="middle" class="ll-node-text-call">
                              fib({{ node.k }})
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
                        <div class="ll-memo-title">Memoization Cache Table &mdash; <code>memo[0...{{ st.n }}]</code>:</div>
                        <div class="ll-memo-strip">
                          <div v-for="(val, idx) in st.memo" :key="idx" class="ll-memo-cell-wrap">
                            <div
                              class="ll-memo-cell"
                              :class="{
                                'll-memo-hit': val !== -1,
                                'll-memo-empty': val === -1
                              }"
                            >
                              {{ val === -1 ? '?' : val }}
                            </div>
                            <div class="ll-memo-idx">[{{ idx }}]</div>
                          </div>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>

              <!-- Vertical Resizer -->
              <div class="ll-vresizer" ref="vizResizerRef"></div>

              <!-- Color Legend -->
              <div class="ll-legend">
                <template v-if="currentApproach === 'tabulation'">
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-base"></span>Base Cases</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-purple"></span>Term (dp[i-2])</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-cur"></span>Term (dp[i-1])</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-new"></span>Calculated dp[i]</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-uncalc"></span>Uncalculated</span>
                </template>
                <template v-else-if="currentApproach === 'memoization'">
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-cur"></span>Active Call</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-new"></span>Computed &amp; Cached</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-purple"></span>Cache Hit (O(1) Return)</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-uncalc"></span>Uncalled</span>
                </template>
                <template v-else>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-cur"></span>Active Frame</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-new"></span>Returned Value</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-red"></span>Redundant Call</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-uncalc"></span>Pending</span>
                </template>
              </div>

              <!-- Variable Frames & Call Stack Area -->
              <div class="ll-table-area" :style="{ height: tableHeight + 'px' }">
                <div class="ll-table-title">Variable frames &mdash; innermost = current</div>
                <div class="ll-stack-line">
                  <template v-if="s.vars && s.vars.length">
                    <div
                      v-for="(f, depth) in s.vars"
                      :key="depth"
                      class="ll-frame"
                      :class="{ 'll-frame-cur': depth === s.vars.length - 1 }"
                      :style="{ marginLeft: depth * 12 + 'px' }"
                    >
                      {{ f.title }}(<span v-for="(r, i) in f.rows" :key="i">
                        <span v-if="i > 0">, </span>
                        <span class="ll-fname">{{ r[0] }}</span>=<span
                          :class="r[0] === 'return' || r[0] === 'result' ? 'll-c-green' : (depth === s.vars.length - 1 ? 'll-c-orange' : 'll-c-blue')"
                          style="font-weight:700"
                        >{{ r[1] }}</span>
                      </span>)<span v-if="depth === s.vars.length - 1" class="ll-now"> &#9668; current</span>
                    </div>
                  </template>
                  <template v-else>&mdash;</template>
                </div>
              </div>

              <!-- Vertical Resizer 2 -->
              <div class="ll-vresizer" ref="tableResizerRef"></div>

              <!-- Explanatory Step Badge -->
              <div class="ll-badge-wrap">
                <div
                  class="ll-badge"
                  :class="{
                    'll-badge-error': s.badge && s.badge.includes('Overlapping'),
                    'll-badge-success': s.badge && (s.badge.includes('Finished') || s.badge.includes('Complete'))
                  }"
                >
                  {{ s.badge }}
                </div>
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
                  <h3>Why Dynamic Programming? (Faculty Insight)</h3>
                  <p>
                    The Fibonacci sequence exhibits two textbook properties that define Dynamic Programming:
                  </p>
                  <ul>
                    <li>
                      <strong>1. Optimal Substructure:</strong> The solution to subproblem <code>fib(n)</code> is composed directly from the solutions of smaller independent subproblems <code>fib(n-1)</code> and <code>fib(n-2)</code>.
                    </li>
                    <li>
                      <strong>2. Overlapping Subproblems:</strong> Naive recursion branches into identical function calls repeatedly (e.g. <code>fib(3)</code> and <code>fib(2)</code> are evaluated dozens of times across different branches).
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
                        <td>O(1) Two Pointers</td>
                        <td><span class="ll-c-green" style="font-weight:700">NO</span> (Keeps Only Last 2 Terms)</td>
                      </tr>
                    </tbody>
                  </table>

                  <h3>Mathematical Proof of Recurrence</h3>
                  <p>
                    For Naive Recursion, the number of operations follows the recurrence:
                  </p>
                  <p class="ll-math-box">
                    T(n) = T(n - 1) + T(n - 2) + O(1) &rArr; O(&phi;<sup>n</sup>) &approx; O(1.618<sup>n</sup>) &rArr; O(2<sup>n</sup>)
                  </p>
                  <p>
                    For <code>n = 40</code>, Naive Recursion requires over <strong>2,000,000,000 (2 Billion)</strong> calls, taking seconds to minutes, while DP Tabulation/Memoization computes it in just <strong>40 operations</strong> (&lt; 1 &mu;s)!
                  </p>

                  <div class="ll-note">
                    <strong>Pedagogical Key Takeaway:</strong> Dynamic Programming trades a small amount of auxiliary memory (table / cache) to eliminate exponential redundant recomputations, converting impossible O(2<sup>n</sup>) algorithms into blisteringly fast O(n) solutions.
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
  display: flex; flex-direction: column; overflow: hidden; width: 100%; height:58vh; /* modified */
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
.ll-toolbar { margin-top: 4px; display: flex; align-items: center; gap: 6px; padding: 5px 12px; background: var(--surface); border-bottom: 1px solid var(--border); flex-shrink: 0; flex-wrap: wrap; box-shadow: var(--shadow-sm); } /* modified */
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

/* Variable Frames & Call Stack */
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
