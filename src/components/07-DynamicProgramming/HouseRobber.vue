<script setup>
import { ref, computed, reactive, onMounted, onUnmounted, nextTick } from 'vue';

/* ==================================================================== */
/* PROPS & COMPONENT CONFIGURATION                                      */
/* ==================================================================== */
const props = defineProps({
  topic: { type: String, default: 'Dynamic Programming' },
  subTopic: { type: String, default: 'House Robber Problem' }
});

/* ==================================================================== */
/* APPROACHES & MULTI-LANGUAGE 100% EXECUTABLE CODE SPECIFICATIONS      */
/* ==================================================================== */
const APPROACHES = [
  { id: 'recursion', label: 'Brute Force', desc: 'O(2^n) Exponential — Evaluates Rob vs Skip at Each House' },
  { id: 'memoization', label: 'Memoization', desc: 'O(n) Linear Time & O(n) Space — Top-Down with Subproblem Cache' },
  { id: 'tabulation', label: 'Tabulation', desc: 'O(n) Linear Time & O(n) Space — Bottom-Up 1D DP Array' }
];

const PRESETS = [
  { label: '[2, 7, 9, 3, 1]', desc: '5 Houses ($12 Max Loot)', val: [2, 7, 9, 3, 1] },
  { label: '[1, 2, 3, 1]', desc: '4 Houses ($4 Max Loot)', val: [1, 2, 3, 1] },
  { label: '[2, 1, 1, 2]', desc: '4 Houses ($4 Max Loot)', val: [2, 1, 1, 2] },
  { label: '[5, 1, 2, 6]', desc: '4 Houses ($11 Max Loot)', val: [5, 1, 2, 6] },
  { label: '[2, 3, 2]', desc: '3 Houses ($4 Max Loot)', val: [2, 3, 2] }
];

const CODES = {
  recursion: {
    java: [
      ['',             'import java.util.Scanner;'],
      ['',             ''],
      ['',             'public class Main {'],
      ['c_entry',      '    static int rob(int[] nums, int i) {'],
      ['c_base_neg',   '        if (i < 0) return 0;'],
      ['c_base_zero',  '        if (i == 0) return nums[0];'],
      ['c_rec_skip',   '        int skip = rob(nums, i - 1);'],
      ['c_rec_rob',    '        int robCurrent = nums[i] + rob(nums, i - 2);'],
      ['c_ret_max',    '        return Math.max(skip, robCurrent);'],
      ['',             '    }'],
      ['',             ''],
      ['',             '    public static void main(String[] args) {'],
      ['m_scanner',    '        Scanner sc = new Scanner(System.in);'],
      ['m_read_n',     '        int n = sc.nextInt();'],
      ['m_read_arr',   '        int[] nums = new int[n];'],
      ['',             '        for (int j = 0; j < n; j++) nums[j] = sc.nextInt();'],
      ['m_call_rob',   '        int maxLoot = rob(nums, n - 1);'],
      ['m_print',      '        System.out.println(maxLoot);'],
      ['m_done',       '    }'],
      ['',             '}']
    ],
    c: [
      ['',             '#include <stdio.h>'],
      ['',             '#define MAX(a, b) ((a) > (b) ? (a) : (b))'],
      ['',             ''],
      ['c_entry',      'int rob(int* nums, int i) {'],
      ['c_base_neg',   '    if (i < 0) return 0;'],
      ['c_base_zero',  '    if (i == 0) return nums[0];'],
      ['c_rec_skip',   '    int skip = rob(nums, i - 1);'],
      ['c_rec_rob',    '    int robCurrent = nums[i] + rob(nums, i - 2);'],
      ['c_ret_max',    '    return MAX(skip, robCurrent);'],
      ['',             '}'],
      ['',             ''],
      ['',             'int main() {'],
      ['m_read_n',     '    int n; scanf("%d", &n);'],
      ['m_read_arr',   '    int nums[n];'],
      ['',             '    for (int j = 0; j < n; j++) scanf("%d", &nums[j]);'],
      ['m_call_rob',   '    int maxLoot = rob(nums, n - 1);'],
      ['m_print',      '    printf("%d\\n", maxLoot);'],
      ['m_done',       '    return 0;'],
      ['',             '}']
    ],
    cpp: [
      ['',             '#include <iostream>'],
      ['',             '#include <vector>'],
      ['',             '#include <algorithm>'],
      ['',             'using namespace std;'],
      ['',             ''],
      ['c_entry',      'int rob(vector<int>& nums, int i) {'],
      ['c_base_neg',   '    if (i < 0) return 0;'],
      ['c_base_zero',  '    if (i == 0) return nums[0];'],
      ['c_rec_skip',   '    int skip = rob(nums, i - 1);'],
      ['c_rec_rob',    '    int robCurrent = nums[i] + rob(nums, i - 2);'],
      ['c_ret_max',    '    return max(skip, robCurrent);'],
      ['',             '}'],
      ['',             ''],
      ['',             'int main() {'],
      ['m_read_n',     '    int n; cin >> n;'],
      ['m_read_arr',   '    vector<int> nums(n);'],
      ['',             '    for (int j = 0; j < n; j++) cin >> nums[j];'],
      ['m_call_rob',   '    int maxLoot = rob(nums, n - 1);'],
      ['m_print',      '    cout << maxLoot << endl;'],
      ['m_done',       '    return 0;'],
      ['',             '}']
    ],
    python: [
      ['',             'import sys'],
      ['',             ''],
      ['c_entry',      'def rob(nums, i):'],
      ['c_base_neg',   '    if i < 0:'],
      ['',             '        return 0'],
      ['c_base_zero',  '    if i == 0:'],
      ['',             '        return nums[0]'],
      ['c_rec_skip',   '    skip = rob(nums, i - 1)'],
      ['c_rec_rob',    '    rob_current = nums[i] + rob(nums, i - 2)'],
      ['c_ret_max',    '    return max(skip, rob_current)'],
      ['',             ''],
      ['',             'if __name__ == "__main__":'],
      ['m_read_n',     '    data = list(map(int, sys.stdin.read().split()))'],
      ['m_read_arr',   '    n, nums = data[0], data[1:data[0]+1]'],
      ['m_call_rob',   '    max_loot = rob(nums, n - 1)'],
      ['m_print',      '    print(max_loot)'],
      ['m_done',       '    sys.exit(0)']
    ],
    javascript: [
      ['',             'const fs = require("fs");'],
      ['',             ''],
      ['c_entry',      'function rob(nums, i) {'],
      ['c_base_neg',   '    if (i < 0) return 0;'],
      ['c_base_zero',  '    if (i === 0) return nums[0];'],
      ['c_rec_skip',   '    const skip = rob(nums, i - 1);'],
      ['c_rec_rob',    '    const robCurrent = nums[i] + rob(nums, i - 2);'],
      ['c_ret_max',    '    return Math.max(skip, robCurrent);'],
      ['',             '}'],
      ['',             ''],
      ['m_read_n',     'const input = fs.readFileSync("/dev/stdin", "utf-8").trim().split(/\\s+/).map(Number);'],
      ['m_read_arr',   'const n = input[0]; const nums = input.slice(1, n + 1);'],
      ['m_call_rob',   'const maxLoot = rob(nums, n - 1);'],
      ['m_print',      'console.log(maxLoot);'],
      ['m_done',       'process.exit(0);']
    ]
  },
  memoization: {
    java: [
      ['',             'import java.util.Scanner;'],
      ['',             'import java.util.Arrays;'],
      ['',             ''],
      ['',             'public class Main {'],
      ['c_entry',      '    static int solve(int[] nums, int i, int[] memo) {'],
      ['c_base_neg',   '        if (i < 0) return 0;'],
      ['c_base_zero',  '        if (i == 0) return nums[0];'],
      ['c_memo_check', '        if (memo[i] != -1) return memo[i];'],
      ['c_rec_skip',   '        int skip = solve(nums, i - 1, memo);'],
      ['c_rec_rob',    '        int robCurrent = nums[i] + solve(nums, i - 2, memo);'],
      ['c_store_memo', '        memo[i] = Math.max(skip, robCurrent);'],
      ['c_ret_max',    '        return memo[i];'],
      ['',             '    }'],
      ['',             ''],
      ['',             '    public static void main(String[] args) {'],
      ['m_scanner',    '        Scanner sc = new Scanner(System.in);'],
      ['m_read_n',     '        int n = sc.nextInt();'],
      ['m_read_arr',   '        int[] nums = new int[n];'],
      ['',             '        for (int j = 0; j < n; j++) nums[j] = sc.nextInt();'],
      ['m_alloc_memo', '        int[] memo = new int[n];'],
      ['m_fill_memo',  '        Arrays.fill(memo, -1);'],
      ['m_call_rob',   '        int maxLoot = solve(nums, n - 1, memo);'],
      ['m_print',      '        System.out.println(maxLoot);'],
      ['m_done',       '    }'],
      ['',             '}']
    ],
    c: [
      ['',             '#include <stdio.h>'],
      ['',             '#include <string.h>'],
      ['',             '#define MAX(a, b) ((a) > (b) ? (a) : (b))'],
      ['',             ''],
      ['c_entry',      'int solve(int* nums, int i, int* memo) {'],
      ['c_base_neg',   '    if (i < 0) return 0;'],
      ['c_base_zero',  '    if (i == 0) return nums[0];'],
      ['c_memo_check', '    if (memo[i] != -1) return memo[i];'],
      ['c_rec_skip',   '    int skip = solve(nums, i - 1, memo);'],
      ['c_rec_rob',    '    int robCurrent = nums[i] + solve(nums, i - 2, memo);'],
      ['c_store_memo', '    memo[i] = MAX(skip, robCurrent);'],
      ['c_ret_max',    '    return memo[i];'],
      ['',             '}'],
      ['',             ''],
      ['',             'int main() {'],
      ['m_read_n',     '    int n; scanf("%d", &n);'],
      ['m_read_arr',   '    int nums[n];'],
      ['',             '    for (int j = 0; j < n; j++) scanf("%d", &nums[j]);'],
      ['m_alloc_memo', '    int memo[n];'],
      ['m_fill_memo',  '    memset(memo, -1, sizeof(memo));'],
      ['m_call_rob',   '    int maxLoot = solve(nums, n - 1, memo);'],
      ['m_print',      '    printf("%d\\n", maxLoot);'],
      ['m_done',       '    return 0;'],
      ['',             '}']
    ],
    cpp: [
      ['',             '#include <iostream>'],
      ['',             '#include <vector>'],
      ['',             '#include <algorithm>'],
      ['',             'using namespace std;'],
      ['',             ''],
      ['c_entry',      'int solve(vector<int>& nums, int i, vector<int>& memo) {'],
      ['c_base_neg',   '    if (i < 0) return 0;'],
      ['c_base_zero',  '    if (i == 0) return nums[0];'],
      ['c_memo_check', '    if (memo[i] != -1) return memo[i];'],
      ['c_rec_skip',   '    int skip = solve(nums, i - 1, memo);'],
      ['c_rec_rob',    '    int robCurrent = nums[i] + solve(nums, i - 2, memo);'],
      ['c_store_memo', '    memo[i] = max(skip, robCurrent);'],
      ['c_ret_max',    '    return memo[i];'],
      ['',             '}'],
      ['',             ''],
      ['',             'int main() {'],
      ['m_read_n',     '    int n; cin >> n;'],
      ['m_read_arr',   '    vector<int> nums(n);'],
      ['',             '    for (int j = 0; j < n; j++) cin >> nums[j];'],
      ['m_alloc_memo', '    vector<int> memo(n, -1);'],
      ['m_call_rob',   '    int maxLoot = solve(nums, n - 1, memo);'],
      ['m_print',      '    cout << maxLoot << endl;'],
      ['m_done',       '    return 0;'],
      ['',             '}']
    ],
    python: [
      ['',             'import sys'],
      ['',             ''],
      ['c_entry',      'def solve(nums, i, memo):'],
      ['c_base_neg',   '    if i < 0:'],
      ['',             '        return 0'],
      ['c_base_zero',  '    if i == 0:'],
      ['',             '        return nums[0]'],
      ['c_memo_check', '    if memo[i] != -1:'],
      ['',             '        return memo[i]'],
      ['c_rec_skip',   '    skip = solve(nums, i - 1, memo)'],
      ['c_rec_rob',    '    rob_current = nums[i] + solve(nums, i - 2, memo)'],
      ['c_store_memo', '    memo[i] = max(skip, rob_current)'],
      ['c_ret_max',    '    return memo[i]'],
      ['',             ''],
      ['',             'if __name__ == "__main__":'],
      ['m_read_n',     '    data = list(map(int, sys.stdin.read().split()))'],
      ['m_read_arr',   '    n, nums = data[0], data[1:data[0]+1]'],
      ['m_alloc_memo', '    memo = [-1] * n'],
      ['m_call_rob',   '    max_loot = solve(nums, n - 1, memo)'],
      ['m_print',      '    print(max_loot)'],
      ['m_done',       '    sys.exit(0)']
    ],
    javascript: [
      ['',             'const fs = require("fs");'],
      ['',             ''],
      ['c_entry',      'function solve(nums, i, memo) {'],
      ['c_base_neg',   '    if (i < 0) return 0;'],
      ['c_base_zero',  '    if (i === 0) return nums[0];'],
      ['c_memo_check', '    if (memo[i] !== -1) return memo[i];'],
      ['c_rec_skip',   '    const skip = solve(nums, i - 1, memo);'],
      ['c_rec_rob',    '    const robCurrent = nums[i] + solve(nums, i - 2, memo);'],
      ['c_store_memo', '    memo[i] = Math.max(skip, robCurrent);'],
      ['c_ret_max',    '    return memo[i];'],
      ['',             '}'],
      ['',             ''],
      ['m_read_n',     'const input = fs.readFileSync("/dev/stdin", "utf-8").trim().split(/\\s+/).map(Number);'],
      ['m_read_arr',   'const n = input[0]; const nums = input.slice(1, n + 1);'],
      ['m_alloc_memo', 'const memo = new Array(n).fill(-1);'],
      ['m_call_rob',   'const maxLoot = solve(nums, n - 1, memo);'],
      ['m_print',      'console.log(maxLoot);'],
      ['m_done',       'process.exit(0);']
    ]
  },
  tabulation: {
    java: [
      ['',             'import java.util.Scanner;'],
      ['',             ''],
      ['',             'public class Main {'],
      ['c_entry',      '    static int rob(int[] nums) {'],
      ['c_base_empty', '        int n = nums.length;'],
      ['',             '        if (n == 0) return 0;'],
      ['c_base_one',   '        if (n == 1) return nums[0];'],
      ['c_alloc_dp',   '        int[] dp = new int[n];'],
      ['c_init_base0', '        dp[0] = nums[0];'],
      ['c_init_base1', '        dp[1] = Math.max(nums[0], nums[1]);'],
      ['c_for_loop',   '        for (int i = 2; i < n; i++) {'],
      ['c_dp_trans',   '            dp[i] = Math.max(dp[i - 1], nums[i] + dp[i - 2]);'],
      ['',             '        }'],
      ['c_ret_dpn',    '        return dp[n - 1];'],
      ['',             '    }'],
      ['',             ''],
      ['',             '    public static void main(String[] args) {'],
      ['m_scanner',    '        Scanner sc = new Scanner(System.in);'],
      ['m_read_n',     '        int n = sc.nextInt();'],
      ['m_read_arr',   '        int[] nums = new int[n];'],
      ['',             '        for (int j = 0; j < n; j++) nums[j] = sc.nextInt();'],
      ['m_call_rob',   '        int maxLoot = rob(nums);'],
      ['m_print',      '        System.out.println(maxLoot);'],
      ['m_done',       '    }'],
      ['',             '}']
    ],
    c: [
      ['',             '#include <stdio.h>'],
      ['',             '#define MAX(a, b) ((a) > (b) ? (a) : (b))'],
      ['',             ''],
      ['c_entry',      'int rob(int* nums, int n) {'],
      ['c_base_empty', '    if (n == 0) return 0;'],
      ['c_base_one',   '    if (n == 1) return nums[0];'],
      ['c_alloc_dp',   '    int dp[n];'],
      ['c_init_base0', '    dp[0] = nums[0];'],
      ['c_init_base1', '    dp[1] = MAX(nums[0], nums[1]);'],
      ['c_for_loop',   '    for (int i = 2; i < n; i++) {'],
      ['c_dp_trans',   '        dp[i] = MAX(dp[i - 1], nums[i] + dp[i - 2]);'],
      ['',             '    }'],
      ['c_ret_dpn',    '    return dp[n - 1];'],
      ['',             '}'],
      ['',             ''],
      ['int main() {'],
      ['m_read_n',     '    int n; scanf("%d", &n);'],
      ['m_read_arr',   '    int nums[n];'],
      ['',             '    for (int j = 0; j < n; j++) scanf("%d", &nums[j]);'],
      ['m_call_rob',   '    int maxLoot = rob(nums, n);'],
      ['m_print',      '    printf("%d\\n", maxLoot);'],
      ['m_done',       '    return 0;'],
      ['',             '}']
    ],
    cpp: [
      ['',             '#include <iostream>'],
      ['',             '#include <vector>'],
      ['',             '#include <algorithm>'],
      ['',             'using namespace std;'],
      ['',             ''],
      ['c_entry',      'int rob(vector<int>& nums) {'],
      ['c_base_empty', '    int n = nums.size();'],
      ['',             '    if (n == 0) return 0;'],
      ['c_base_one',   '    if (n == 1) return nums[0];'],
      ['c_alloc_dp',   '    vector<int> dp(n);'],
      ['c_init_base0', '    dp[0] = nums[0];'],
      ['c_init_base1', '    dp[1] = max(nums[0], nums[1]);'],
      ['c_for_loop',   '    for (int i = 2; i < n; i++) {'],
      ['c_dp_trans',   '        dp[i] = max(dp[i - 1], nums[i] + dp[i - 2]);'],
      ['',             '    }'],
      ['c_ret_dpn',    '    return dp[n - 1];'],
      ['',             '}'],
      ['',             ''],
      ['int main() {'],
      ['m_read_n',     '    int n; cin >> n;'],
      ['m_read_arr',   '    vector<int> nums(n);'],
      ['',             '    for (int j = 0; j < n; j++) cin >> nums[j];'],
      ['m_call_rob',   '    int maxLoot = rob(nums);'],
      ['m_print',      '    cout << maxLoot << endl;'],
      ['m_done',       '    return 0;'],
      ['',             '}']
    ],
    python: [
      ['',             'import sys'],
      ['',             ''],
      ['c_entry',      'def rob(nums):'],
      ['c_base_empty', '    n = len(nums)'],
      ['',             '    if n == 0: return 0'],
      ['c_base_one',   '    if n == 1: return nums[0]'],
      ['c_alloc_dp',   '    dp = [0] * n'],
      ['c_init_base0', '    dp[0] = nums[0]'],
      ['c_init_base1', '    dp[1] = max(nums[0], nums[1])'],
      ['c_for_loop',   '    for i in range(2, n):'],
      ['c_dp_trans',   '        dp[i] = max(dp[i - 1], nums[i] + dp[i - 2])'],
      ['c_ret_dpn',    '    return dp[n - 1]'],
      ['',             ''],
      ['if __name__ == "__main__":'],
      ['m_read_n',     '    data = list(map(int, sys.stdin.read().split()))'],
      ['m_read_arr',   '    n, nums = data[0], data[1:data[0]+1]'],
      ['m_call_rob',   '    max_loot = rob(nums)'],
      ['m_print',      '    print(max_loot)'],
      ['m_done',       '    sys.exit(0)']
    ],
    javascript: [
      ['',             'const fs = require("fs");'],
      ['',             ''],
      ['c_entry',      'function rob(nums) {'],
      ['c_base_empty', '    const n = nums.length;'],
      ['',             '    if (n === 0) return 0;'],
      ['c_base_one',   '    if (n === 1) return nums[0];'],
      ['c_alloc_dp',   '    const dp = new Array(n);'],
      ['c_init_base0', '    dp[0] = nums[0];'],
      ['c_init_base1', '    dp[1] = Math.max(nums[0], nums[1]);'],
      ['c_for_loop',   '    for (let i = 2; i < n; i++) {'],
      ['c_dp_trans',   '        dp[i] = Math.max(dp[i - 1], nums[i] + dp[i - 2]);'],
      ['',             '    }'],
      ['c_ret_dpn',    '    return dp[n - 1];'],
      ['',             '}'],
      ['',             ''],
      ['m_read_n',     'const input = fs.readFileSync("/dev/stdin", "utf-8").trim().split(/\\s+/).map(Number);'],
      ['m_read_arr',   'const n = input[0]; const nums = input.slice(1, n + 1);'],
      ['m_call_rob',   'const maxLoot = rob(nums);'],
      ['m_print',      'console.log(maxLoot);'],
      ['m_done',       'process.exit(0);']
    ]
  }
};

const PSEUDOCODES = {
  recursion: [
    'function rob(nums, i):',
    '  if i < 0: return 0',
    '  if i == 0: return nums[0]',
    '  skip = rob(nums, i - 1)              // Option 1: Do not rob house i',
    '  robCurrent = nums[i] + rob(nums, i - 2) // Option 2: Rob house i + loot from i-2',
    '  return max(skip, robCurrent)',
    '',
    'Time Complexity:  O(2^n) Exponential',
    'Space Complexity: O(n) Call Stack'
  ],
  memoization: [
    'memo = array of size n, initialized to -1',
    '',
    'function solve(nums, i, memo):',
    '  if i < 0: return 0',
    '  if i == 0: return nums[0]',
    '  if memo[i] != -1: return memo[i]     // O(1) Cache Hit',
    '  skip = solve(nums, i - 1, memo)',
    '  robCurrent = nums[i] + solve(nums, i - 2, memo)',
    '  memo[i] = max(skip, robCurrent)',
    '  return memo[i]',
    '',
    'Time Complexity:  O(n) Linear',
    'Space Complexity: O(n) Cache Table + Call Stack'
  ],
  tabulation: [
    'function rob(nums):',
    '  n = nums.length',
    '  if n == 0: return 0',
    '  if n == 1: return nums[0]',
    '  dp = array of size n',
    '  dp[0] = nums[0]',
    '  dp[1] = max(nums[0], nums[1])',
    '  for i = 2 to n - 1:',
    '    dp[i] = max(dp[i - 1], nums[i] + dp[i - 2])',
    '  return dp[n - 1]',
    '',
    'Time Complexity:  O(n) Single Pass',
    'Space Complexity: O(n) Table (Can be optimized to O(1) with 2 variables)'
  ]
};

/* ==================================================================== */
/* STATIC TREE GENERATOR FOR DECISION TREE (ROB VS SKIP)                */
/* ==================================================================== */
function generateStaticTree(approach, nums) {
  const n = nums.length;
  const nodes = [];
  const edges = [];
  let nextId = 0;

  if (approach === 'recursion') {
    function build(i, parentId, isLeft) {
      const id = nextId++;
      const node = { id, i, parentId, isLeft, isRedundant: false, children: [] };
      nodes.push(node);

      if (parentId !== null) {
        edges.push({ from: parentId, to: id, isLeft });
      }

      if (i >= 1) {
        // Left child: Skip house i -> evaluate subproblem i - 1
        const leftId = build(i - 1, id, true);
        // Right child: Rob house i -> evaluate subproblem i - 2
        const rightId = (i - 2 >= 0) ? build(i - 2, id, false) : null;
        node.children = rightId !== null ? [leftId, rightId] : [leftId];
      }
      return id;
    }
    if (n > 0) build(n - 1, null, false);
  } else if (approach === 'memoization') {
    const memo = new Array(n).fill(-1);
    function buildMemo(i, parentId, isLeft) {
      const id = nextId++;
      const isCacheHit = i >= 0 && memo[i] !== -1;
      const node = { id, i, parentId, isLeft, isCacheHit, children: [] };
      nodes.push(node);

      if (parentId !== null) {
        edges.push({ from: parentId, to: id, isLeft });
      }

      if (i <= 0) {
        if (i === 0) memo[0] = nums[0];
      } else if (isCacheHit) {
        // Pruned subtree — no children spawned!
      } else {
        const leftId = buildMemo(i - 1, id, true);
        const rightId = (i - 2 >= 0) ? buildMemo(i - 2, id, false) : null;
        node.children = rightId !== null ? [leftId, rightId] : [leftId];
        memo[i] = 1;
      }
      return id;
    }
    if (n > 0) buildMemo(n - 1, null, false);
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
  const leafSpacing = 60;
  const levelHeight = 44;

  const computedWidth = Math.max(160, totalLeaves * leafSpacing + 30);
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
function buildSteps(approach, numsArr) {
  const steps = [];
  const maxLimit = approach === 'tabulation' ? 16 : 6;
  const rawNums = (numsArr && numsArr.length > 0) ? numsArr : [2, 7, 9, 3, 1];
  const nums = rawNums.slice(0, maxLimit);
  const n = nums.length;

  /* ------------------------------------------------------------------ */
  /* APPROACH 1: BRUTE FORCE RECURSION                                   */
  /* ------------------------------------------------------------------ */
  if (approach === 'recursion') {
    const staticTree = generateStaticTree('recursion', nums);
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

    // Step 0: Input read
    steps.push({
      code: 'm_read_n',
      badge: `Input: ${n} houses with cash values [${nums.join(', ')}]`,
      badgeType: 'info',
      state: {
        nums,
        n,
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

    // Step 1: Call rob(nums, n - 1)
    steps.push({
      code: 'm_call_rob',
      badge: `main() calls rob(nums, i = ${n - 1}) to determine max loot from houses 0...${n - 1}`,
      badgeType: 'info',
      state: {
        nums,
        n,
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

    function recurse(i) {
      if (i < 0) return 0;
      const myNodeId = nextNodeId++;
      callCounter++;
      const isDuplicate = (computedValuesMap[i] !== undefined);
      if (isDuplicate) redundantCounter++;

      nodeStateMap[myNodeId].state = 'active';
      nodeStateMap[myNodeId].isRedundant = isDuplicate;

      stackFrames.push({ name: `rob(i = ${i})`, args: `house[${i}]=$${nums[i]}` });

      // Step: Function entry
      steps.push({
        code: 'c_entry',
        badge: isDuplicate
          ? `[REDUNDANT SUBPROBLEM] rob(i = ${i}) evaluated again! Subproblem i = ${i} was already computed earlier.`
          : `Entering rob(i = ${i}) [Call #${callCounter}] for House ${i} ($${nums[i]})`,
        badgeType: isDuplicate ? 'warn' : 'info',
        state: {
          nums,
          n,
          totalCalls: callCounter,
          redundantCalls: redundantCounter,
          currentReturn: null,
          activeNodeId: myNodeId,
          activeHouseIdx: i,
          frames: [...stackFrames],
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height
        }
      });

      // Step: Base check i == 0
      steps.push({
        code: 'c_base_zero',
        badge: `Check base condition: i == 0 &rarr; ${i === 0 ? `TRUE. Only 1 house available: return nums[0] = $${nums[0]}` : 'FALSE (Must choose between Skip or Rob)'}`,
        badgeType: 'info',
        state: {
          nums,
          n,
          totalCalls: callCounter,
          redundantCalls: redundantCounter,
          currentReturn: null,
          activeNodeId: myNodeId,
          activeHouseIdx: i,
          frames: [...stackFrames],
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height
        }
      });

      if (i === 0) {
        const ret = nums[0];
        nodeStateMap[myNodeId].state = 'solved';
        nodeStateMap[myNodeId].retVal = ret;
        computedValuesMap[i] = ret;

        steps.push({
          code: 'c_base_zero',
          badge: `Base case: rob(i = 0) returns nums[0] = $${ret}`,
          badgeType: 'success',
          state: {
            nums,
            n,
            totalCalls: callCounter,
            redundantCalls: redundantCounter,
            currentReturn: ret,
            activeNodeId: myNodeId,
            activeHouseIdx: i,
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

      // Branch 1: Skip House i -> rob(i - 1)
      steps.push({
        code: 'c_rec_skip',
        badge: `Option 1 (Skip House ${i}): Evaluate max loot by skipping house ${i} &rarr; call rob(i - 1 = ${i - 1})`,
        badgeType: 'info',
        state: {
          nums,
          n,
          totalCalls: callCounter,
          redundantCalls: redundantCounter,
          currentReturn: null,
          activeNodeId: myNodeId,
          activeHouseIdx: i,
          frames: [...stackFrames],
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height
        }
      });

      const skipVal = recurse(i - 1);

      // Branch 2: Rob House i -> nums[i] + rob(i - 2)
      steps.push({
        code: 'c_rec_rob',
        badge: `Option 2 (Rob House ${i}): Rob current house ($${nums[i]}) + loot from remaining houses rob(i - 2 = ${i - 2})`,
        badgeType: 'info',
        state: {
          nums,
          n,
          totalCalls: callCounter,
          redundantCalls: redundantCounter,
          currentReturn: null,
          activeNodeId: myNodeId,
          activeHouseIdx: i,
          frames: [...stackFrames],
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height
        }
      });

      const robPrevVal = (i - 2 >= 0) ? recurse(i - 2) : 0;
      const robTotal = nums[i] + robPrevVal;

      // Decision: max(skipVal, robTotal)
      const maxLoot = Math.max(skipVal, robTotal);
      nodeStateMap[myNodeId].state = 'solved';
      nodeStateMap[myNodeId].retVal = maxLoot;
      computedValuesMap[i] = maxLoot;

      steps.push({
        code: 'c_ret_max',
        badge: `House ${i}: max(Skip: $${skipVal}, Rob: $${nums[i]} + $${robPrevVal} = $${robTotal}) &rarr; Optimal Loot = $${maxLoot}`,
        badgeType: 'success',
        state: {
          nums,
          n,
          totalCalls: callCounter,
          redundantCalls: redundantCounter,
          currentReturn: maxLoot,
          activeNodeId: myNodeId,
          activeHouseIdx: i,
          frames: [...stackFrames],
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height
        }
      });

      stackFrames.pop();
      return maxLoot;
    }

    const finalResult = recurse(n - 1);

    steps.push({
      code: 'm_print',
      badge: `Recursion Complete! Maximum loot stolen without triggering alarms = $${finalResult} (${callCounter} recursive calls)`,
      badgeType: 'success',
      state: {
        nums,
        n,
        totalCalls: callCounter,
        redundantCalls: redundantCounter,
        currentReturn: finalResult,
        activeNodeId: null,
        frames: [{ name: 'main()', args: `maxLoot=$${finalResult}` }],
        treeNodes: getVisibleNodes(),
        treeEdges: getVisibleEdges(),
        treeWidth: staticTree.width,
        treeHeight: staticTree.height
      }
    });

    steps.push({
      code: 'm_done',
      badge: `Execution Done. Final Result = $${finalResult}`,
      badgeType: 'success',
      state: {
        nums,
        n,
        totalCalls: callCounter,
        redundantCalls: redundantCounter,
        currentReturn: finalResult,
        activeNodeId: null,
        frames: [{ name: 'main()', args: `maxLoot=$${finalResult}` }],
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
    const staticTree = generateStaticTree('memoization', nums);
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

    const memoArray = new Array(n).fill(-1);
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

    // Step 0: Input read
    steps.push({
      code: 'm_read_n',
      badge: `Input: ${n} houses [${nums.join(', ')}]. Starting Top-Down DP with Memoization.`,
      badgeType: 'info',
      state: {
        nums,
        n,
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
      badge: `Allocate lookup table memo[0...${n - 1}] of size ${n}`,
      badgeType: 'info',
      state: {
        nums,
        n,
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
        nums,
        n,
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

    function recurseMemo(i) {
      if (i < 0) return 0;
      const myNodeId = nextNodeId++;
      callCounter++;
      nodeStateMap[myNodeId].state = 'active';

      stackFrames.push({ name: `solve(i = ${i})`, args: `house[${i}]=$${nums[i]}` });

      // Step: Function entry
      steps.push({
        code: 'c_entry',
        badge: `Entering solve(i = ${i}, memo) [Call #${callCounter}] for House ${i} ($${nums[i]})`,
        badgeType: 'info',
        state: {
          nums,
          n,
          totalCalls: callCounter,
          cacheHits,
          currentReturn: null,
          activeNodeId: myNodeId,
          activeHouseIdx: i,
          memo: [...memoArray],
          frames: [...stackFrames],
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height
        }
      });

      // Step: Base condition
      if (i === 0) {
        const ret = nums[0];
        nodeStateMap[myNodeId].state = 'solved';
        nodeStateMap[myNodeId].retVal = ret;
        memoArray[0] = ret;

        steps.push({
          code: 'c_base_zero',
          badge: `Base case matched: solve(i = 0) returns nums[0] = $${ret}. Stored in memo[0].`,
          badgeType: 'success',
          state: {
            nums,
            n,
            totalCalls: callCounter,
            cacheHits,
            currentReturn: ret,
            activeNodeId: myNodeId,
            activeHouseIdx: i,
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
        badge: `Checking lookup table memo[${i}] != -1 &rarr; memo[${i}] is ${memoArray[i] !== -1 ? `$${memoArray[i]} (CACHE HIT!)` : '-1 (CACHE MISS)'}`,
        badgeType: memoArray[i] !== -1 ? 'success' : 'info',
        state: {
          nums,
          n,
          totalCalls: callCounter,
          cacheHits,
          currentReturn: null,
          activeNodeId: myNodeId,
          activeHouseIdx: i,
          memo: [...memoArray],
          frames: [...stackFrames],
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height
        }
      });

      if (memoArray[i] !== -1) {
        cacheHits++;
        const hitVal = memoArray[i];
        nodeStateMap[myNodeId].state = 'cache-hit';
        nodeStateMap[myNodeId].retVal = hitVal;

        steps.push({
          code: 'c_memo_check',
          badge: `[CACHE HIT] memo[${i}] = $${hitVal} already cached! Returning in O(1) time without evaluating subproblems!`,
          badgeType: 'success',
          state: {
            nums,
            n,
            totalCalls: callCounter,
            cacheHits,
            currentReturn: hitVal,
            activeNodeId: myNodeId,
            activeHouseIdx: i,
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

      // Option 1: Skip
      steps.push({
        code: 'c_rec_skip',
        badge: `Option 1: Skip House ${i} &rarr; call solve(i - 1 = ${i - 1}, memo)`,
        badgeType: 'info',
        state: {
          nums,
          n,
          totalCalls: callCounter,
          cacheHits,
          currentReturn: null,
          activeNodeId: myNodeId,
          activeHouseIdx: i,
          memo: [...memoArray],
          frames: [...stackFrames],
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height
        }
      });

      const skipVal = recurseMemo(i - 1);

      // Option 2: Rob
      steps.push({
        code: 'c_rec_rob',
        badge: `Option 2: Rob House ${i} ($${nums[i]}) + call solve(i - 2 = ${i - 2}, memo)`,
        badgeType: 'info',
        state: {
          nums,
          n,
          totalCalls: callCounter,
          cacheHits,
          currentReturn: null,
          activeNodeId: myNodeId,
          activeHouseIdx: i,
          memo: [...memoArray],
          frames: [...stackFrames],
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height
        }
      });

      const robPrevVal = (i - 2 >= 0) ? recurseMemo(i - 2) : 0;
      const robTotal = nums[i] + robPrevVal;

      const maxLoot = Math.max(skipVal, robTotal);
      memoArray[i] = maxLoot;
      nodeStateMap[myNodeId].state = 'solved';
      nodeStateMap[myNodeId].retVal = maxLoot;

      // Store in memo
      steps.push({
        code: 'c_store_memo',
        badge: `Store result: memo[${i}] = max(Skip: $${skipVal}, Rob: $${nums[i]} + $${robPrevVal} = $${robTotal}) = $${maxLoot}`,
        badgeType: 'success',
        state: {
          nums,
          n,
          totalCalls: callCounter,
          cacheHits,
          currentReturn: maxLoot,
          activeNodeId: myNodeId,
          activeHouseIdx: i,
          memo: [...memoArray],
          frames: [...stackFrames],
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height
        }
      });

      steps.push({
        code: 'c_ret_max',
        badge: `Returning optimal loot memo[${i}] = $${maxLoot} to caller`,
        badgeType: 'success',
        state: {
          nums,
          n,
          totalCalls: callCounter,
          cacheHits,
          currentReturn: maxLoot,
          activeNodeId: myNodeId,
          activeHouseIdx: i,
          memo: [...memoArray],
          frames: [...stackFrames],
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height
        }
      });

      stackFrames.pop();
      return maxLoot;
    }

    const finalAnswer = recurseMemo(n - 1);

    steps.push({
      code: 'm_print',
      badge: `Top-Down DP Complete! Max loot = $${finalAnswer} (${callCounter} total calls, ${cacheHits} cache hits pruned!)`,
      badgeType: 'success',
      state: {
        nums,
        n,
        totalCalls: callCounter,
        cacheHits,
        currentReturn: finalAnswer,
        activeNodeId: null,
        memo: [...memoArray],
        frames: [{ name: 'main()', args: `maxLoot=$${finalAnswer}` }],
        treeNodes: getVisibleNodes(),
        treeEdges: getVisibleEdges(),
        treeWidth: staticTree.width,
        treeHeight: staticTree.height
      }
    });

    steps.push({
      code: 'm_done',
      badge: `Execution Done. Final Result = $${finalAnswer}`,
      badgeType: 'success',
      state: {
        nums,
        n,
        totalCalls: callCounter,
        cacheHits,
        currentReturn: finalAnswer,
        activeNodeId: null,
        memo: [...memoArray],
        frames: [{ name: 'main()', args: `maxLoot=$${finalAnswer}` }],
        treeNodes: getVisibleNodes(),
        treeEdges: getVisibleEdges(),
        treeWidth: staticTree.width,
        treeHeight: staticTree.height
      }
    });
  }

  /* ------------------------------------------------------------------ */
  /* APPROACH 3: DP TABULATION (BOTTOM-UP 1D ARRAY)                     */
  /* ------------------------------------------------------------------ */
  else if (approach === 'tabulation') {
    const dpArr = new Array(n).fill(null);

    function getDPCells(activeI = -1) {
      return dpArr.map((val, idx) => ({
        idx,
        houseVal: nums[idx],
        val: val !== null ? val : '?',
        status: val !== null ? (idx === activeI ? 'active' : (idx <= 1 ? 'base' : 'computed')) : 'uncalculated'
      }));
    }

    // Step 0: Input read
    steps.push({
      code: 'm_read_n',
      badge: `Input: ${n} houses [${nums.join(', ')}]. Starting Bottom-Up Tabulation.`,
      badgeType: 'info',
      state: {
        nums,
        n,
        i: 0,
        iterations: 0,
        totalOps: 0,
        currentReturn: null,
        dpCells: getDPCells(),
        frames: [{ name: 'main()', args: `n=${n}` }]
      }
    });

    // Step 1: Call rob(nums)
    steps.push({
      code: 'm_call_rob',
      badge: `main() calls rob(nums)`,
      badgeType: 'info',
      state: {
        nums,
        n,
        i: 0,
        iterations: 0,
        totalOps: 0,
        currentReturn: null,
        dpCells: getDPCells(),
        frames: [{ name: 'main()', args: `n=${n}` }, { name: 'rob()', args: `n=${n}` }]
      }
    });

    // Step 2: Base check for n == 1
    if (n === 1) {
      dpArr[0] = nums[0];
      steps.push({
        code: 'c_base_one',
        badge: `Only 1 house: dp[0] = nums[0] = $${nums[0]}. Return directly.`,
        badgeType: 'success',
        state: {
          nums,
          n,
          i: 0,
          iterations: 0,
          totalOps: 1,
          currentReturn: nums[0],
          dpCells: getDPCells(0),
          frames: [{ name: 'main()', args: `n=1` }, { name: 'rob()', args: `maxLoot=$${nums[0]}` }]
        }
      });
      return steps;
    }

    // Step 3: Allocate DP array
    steps.push({
      code: 'c_alloc_dp',
      badge: `Allocate 1D array dp[0...${n - 1}] of size ${n} for bottom-up computation`,
      badgeType: 'info',
      state: {
        nums,
        n,
        i: 0,
        iterations: 0,
        totalOps: 1,
        currentReturn: null,
        dpCells: getDPCells(),
        frames: [{ name: 'main()', args: `n=${n}` }, { name: 'rob()', args: `n=${n}` }]
      }
    });

    // Step 4: Base case dp[0] = nums[0]
    dpArr[0] = nums[0];
    steps.push({
      code: 'c_init_base0',
      badge: `Initialize base case: dp[0] = nums[0] = $${nums[0]} (Only House 0 can be robbed)`,
      badgeType: 'info',
      state: {
        nums,
        n,
        i: 0,
        iterations: 0,
        totalOps: 2,
        currentReturn: null,
        dpCells: getDPCells(0),
        frames: [{ name: 'main()', args: `n=${n}` }, { name: 'rob()', args: `n=${n}` }]
      }
    });

    // Step 5: Base case dp[1] = max(nums[0], nums[1])
    dpArr[1] = Math.max(nums[0], nums[1]);
    steps.push({
      code: 'c_init_base1',
      badge: `Initialize base case: dp[1] = max(nums[0]=$${nums[0]}, nums[1]=$${nums[1]}) = $${dpArr[1]} (Choose richer house)`,
      badgeType: 'info',
      state: {
        nums,
        n,
        i: 1,
        iterations: 0,
        totalOps: 3,
        currentReturn: null,
        dpCells: getDPCells(1),
        frames: [{ name: 'main()', args: `n=${n}` }, { name: 'rob()', args: `n=${n}` }]
      }
    });

    // Step 6: Loop iteratively from i = 2 to n - 1
    let ops = 3;
    for (let i = 2; i < n; i++) {
      ops++;
      const currentIter = i - 1;
      steps.push({
        code: 'c_for_loop',
        badge: `For loop iteration #${currentIter}: i = ${i} (House ${i}, cash = $${nums[i]})`,
        badgeType: 'info',
        state: {
          nums,
          n,
          i,
          iterations: currentIter,
          totalOps: ops,
          currentReturn: null,
          dpCells: getDPCells(i),
          frames: [{ name: 'main()', args: `n=${n}` }, { name: 'rob()', args: `i=${i}` }]
        }
      });

      const skipLoot = dpArr[i - 1];
      const robLoot = nums[i] + dpArr[i - 2];
      dpArr[i] = Math.max(skipLoot, robLoot);
      ops++;

      steps.push({
        code: 'c_dp_trans',
        badge: `Transition: dp[${i}] = max(Skip: dp[${i - 1}]=$${skipLoot}, Rob: nums[${i}]($${nums[i]}) + dp[${i - 2}]($${dpArr[i - 2]}) = $${robLoot}) &rarr; dp[${i}] = $${dpArr[i]}`,
        badgeType: 'success',
        state: {
          nums,
          n,
          i,
          iterations: currentIter,
          totalOps: ops,
          currentReturn: dpArr[i],
          dpCells: getDPCells(i),
          frames: [{ name: 'main()', args: `n=${n}` }, { name: 'rob()', args: `i=${i}, dp[${i}]=$${dpArr[i]}` }]
        }
      });
    }

    const totalIterations = Math.max(0, n - 2);

    // Step 7: Return dp[n - 1]
    steps.push({
      code: 'c_ret_dpn',
      badge: `Return dp[${n - 1}] = $${dpArr[n - 1]} to main()`,
      badgeType: 'success',
      state: {
        nums,
        n,
        i: n - 1,
        iterations: totalIterations,
        totalOps: ops + 1,
        currentReturn: dpArr[n - 1],
        dpCells: getDPCells(n - 1),
        frames: [{ name: 'main()', args: `n=${n}` }, { name: 'rob()', args: `maxLoot=$${dpArr[n - 1]}` }]
      }
    });

    // Step 8: Print
    steps.push({
      code: 'm_print',
      badge: `Output Result: Print maximum loot = $${dpArr[n - 1]}`,
      badgeType: 'success',
      state: {
        nums,
        n,
        i: n - 1,
        iterations: totalIterations,
        totalOps: ops + 2,
        currentReturn: dpArr[n - 1],
        dpCells: getDPCells(n - 1),
        frames: [{ name: 'main()', args: `maxLoot=$${dpArr[n - 1]}` }]
      }
    });

    // Step 9: Done
    steps.push({
      code: 'm_done',
      badge: `Bottom-Up Tabulation Complete! Computed max loot $${dpArr[n - 1]} in single linear pass (${totalIterations} iterations)!`,
      badgeType: 'success',
      state: {
        nums,
        n,
        i: n - 1,
        iterations: totalIterations,
        totalOps: ops + 2,
        currentReturn: dpArr[n - 1],
        dpCells: getDPCells(n - 1),
        frames: [{ name: 'main()', args: `maxLoot=$${dpArr[n - 1]}` }]
      }
    });
  }

  return steps;
}

/* ==================================================================== */
/* REACTIVE STATE & CONTROLS                                            */
/* ==================================================================== */
const currentApproach = ref('recursion');
const customInputStr = ref('2, 7, 9, 3, 1');
const currentNums = ref([2, 7, 9, 3, 1]);
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
const warningModalLimit = ref(6);
const requestedCount = ref(0);

function closeWarningModal() {
  showWarningModal.value = false;
}

const maxAllowedHouses = computed(() => {
  if (currentApproach.value === 'tabulation') return 16;
  return 6; // 'recursion' and 'memoization'
});

const stepsData = reactive({ steps: buildSteps('recursion', [2, 7, 9, 3, 1]) });
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

const showPresetsDropdown = ref(false);
const presetDropdownRef = ref(null);

function selectPreset(val) {
  applyPreset(val);
  showPresetsDropdown.value = false;
}

function handleDocClick(e) {
  if (presetDropdownRef.value && !presetDropdownRef.value.contains(e.target)) {
    showPresetsDropdown.value = false;
  }
}

function parseNums(str) {
  const parts = str.split(/[\s,]+/).map(v => parseInt(v.trim(), 10)).filter(v => !isNaN(v) && v >= 0);
  return parts.length > 0 ? parts : [2, 7, 9, 3, 1];
}

function applyPreset(arr) {
  customInputStr.value = arr.join(', ');
  applyInput();
}

function applyApproach(appId) {
  currentApproach.value = appId;
  applyInput();
}

let playTimer = null;

function applyInput() {
  const parsed = parseNums(customInputStr.value);
  const maxLimit = maxAllowedHouses.value;

  if (parsed.length > maxLimit) {
    const origLen = parsed.length;
    requestedCount.value = origLen;
    warningModalLimit.value = maxLimit;
    currentNums.value = parsed.slice(0, maxLimit);
    customInputStr.value = currentNums.value.join(', ');

    const approachName = APPROACHES.find(a => a.id === currentApproach.value)?.label || 'Algorithm';
    if (currentApproach.value === 'tabulation') {
      warningModalTitle.value = 'Tabulation Array Limit (Max: 16 Houses)';
      warningModalMsg.value = `You entered ${origLen} houses. To maintain optimal presentation visibility, input has been capped to the first 16 houses.`;
    } else {
      warningModalTitle.value = `${approachName} Limit (Max: 6 Houses)`;
      warningModalMsg.value = `Input of ${origLen} houses exceeds the maximum limit for ${approachName} (max: 6). Processing has been capped at 6 houses to prevent exponential tree call overhead.`;
    }
    showWarningModal.value = true;
  } else {
    currentNums.value = parsed;
  }

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
  document.addEventListener('click', handleDocClick);
  cleanupFns.push(initHResizer(hResizerRef.value, leftWidth, 30, 75));
  cleanupFns.push(initVResizer(vizResizerRef.value, vizHeight, 100, 360));
  cleanupFns.push(initVResizer(tableResizerRef.value, tableHeight, 30, 140));
});

onUnmounted(() => {
  document.removeEventListener('keydown', onKeydown);
  document.removeEventListener('click', handleDocClick);
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

            <!-- Input for Houses Array -->
            <div class="ll-input-group">
              <label>Houses:</label>
              <input
                type="text"
                v-model="customInputStr"
                class="ll-text-input"
                placeholder="2, 7, 9, 3, 1"
                @keyup.enter="applyInput"
              />
              <!-- <span class="ll-input-hint">(max {{ maxAllowedHouses }})</span> -->
            </div>

            <!-- Presets Dropdown Button -->
            <div class="ll-dropdown-wrap" ref="presetDropdownRef">
              <button
                class="ll-dropdown-btn"
                :class="{ active: showPresetsDropdown }"
                @click.stop="showPresetsDropdown = !showPresetsDropdown"
                title="Choose a preset houses array"
              >
                 &#9662;
              </button>
              <div v-if="showPresetsDropdown" class="ll-dropdown-menu">
                <button
                  v-for="(p, pi) in PRESETS"
                  :key="pi"
                  class="ll-dropdown-item"
                  @click="selectPreset(p.val)"
                >
                  <span class="ll-drop-label">{{ p.label }}</span>
                  <span class="ll-drop-desc" v-if="p.desc">{{ p.desc }}</span>
                </button>
              </div>
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

          <!-- Main Workspace Split: Left Visuals & Right Code/Theory -->
          <div class="ll-main" ref="mainRef">
            <!-- Left Column: Dynamic Visualizations -->
            <div class="ll-left-col" :style="{ width: leftWidth + '%' }">
              <!-- Top Houses Strip (Always Visible) -->
              <div class="ll-house-banner">
                <div class="ll-house-banner-title">Neighborhood Houses &amp; Money:</div>
                <div class="ll-house-track">
                  <div
                    v-for="(val, hIdx) in currentNums"
                    :key="hIdx"
                    class="ll-house-card"
                    :class="{
                      'll-house-active': st.activeHouseIdx === hIdx || st.i === hIdx,
                      'll-house-skip': st.i >= 2 && hIdx === st.i - 1,
                      'll-house-rob': st.i >= 2 && hIdx === st.i - 2
                    }"
                  >
                    <div class="ll-house-roof">&#9650;</div>
                    <div class="ll-house-body">
                      <span class="ll-house-icon">&#127968;</span>
                      <span class="ll-house-val">${{ val }}</span>
                    </div>
                    <div class="ll-house-idx">House {{ hIdx }}</div>
                  </div>
                </div>
              </div>

              <!-- Visual Workspace Panel -->
              <div class="ll-viz-wrap" :style="{ height: vizHeight + 'px' }">
                <div class="ll-perm-area">
                  <!-- Metrics Ribbon Chips -->
                  <div class="ll-ptrs">
                    <div class="ll-ptr-chip">
                      Houses: <b class="ll-c-blue">{{ st.n || currentNums.length }}</b>
                    </div>
                    <div class="ll-ptr-chip" v-if="currentApproach === 'recursion' || currentApproach === 'memoization'">
                      Calls: <b class="ll-c-orange">{{ st.totalCalls || 0 }}</b>
                    </div>
                    <div class="ll-ptr-chip" v-if="currentApproach === 'tabulation'">
                      Iterations: <b class="ll-c-orange">{{ st.iterations !== undefined ? st.iterations : 0 }}</b>
                    </div>
                    <div class="ll-ptr-chip" v-if="currentApproach === 'recursion'">
                      Redundant: <b class="ll-c-red">{{ st.redundantCalls || 0 }}</b>
                    </div>
                    <div class="ll-ptr-chip" v-if="currentApproach === 'memoization'">
                      Cache Hits: <b class="ll-c-green">{{ st.cacheHits || 0 }}</b>
                    </div>
                    <div class="ll-ptr-chip" v-if="st.currentReturn !== null && st.currentReturn !== undefined">
                      Max Loot: <b class="ll-c-green">${{ st.currentReturn }}</b>
                    </div>
                  </div>

                  <!-- Visual Diagram 1: DP Tabulation (Bottom-Up 1D Array Strip) -->
                  <div v-if="currentApproach === 'tabulation'" class="ll-dp-tab-view">
                    <div class="ll-section-caption">
                      <span>1D Tabulation Array Strip &mdash; <code>dp[0...{{ st.n - 1 }}]</code></span>
                      <span v-if="st.i >= 2 && st.i < st.n" class="ll-calc-pill">
                        dp[{{ st.i }}] = max(dp[{{ st.i - 1 }}], ${{ currentNums[st.i] }} + dp[{{ st.i - 2 }}])
                      </span>
                    </div>

                    <div class="ll-arr-track">
                      <template v-for="cell in st.dpCells" :key="cell.idx">
                        <div class="ll-arr-cell-wrap">
                          <!-- Pointer Tag Above Cell -->
                          <div class="ll-ptr-tag-wrap">
                            <span v-if="st.i >= 2 && cell.idx === st.i - 2" class="ll-ptr-lbl ll-lbl-purple">&darr; i-2</span>
                            <span v-else-if="st.i >= 2 && cell.idx === st.i - 1" class="ll-ptr-lbl ll-lbl-orange">&darr; i-1</span>
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
                            {{ cell.val !== '?' ? `$${cell.val}` : '?' }}
                          </div>
                          <!-- Step Index Label -->
                          <div class="ll-arr-idx">dp[{{ cell.idx }}]</div>
                        </div>
                      </template>
                    </div>
                  </div>

                  <!-- Visual Diagram 2: Recursion & Memoization Decision Tree -->
                  <div v-else class="ll-tree-container">
                    <div class="ll-section-caption">
                      <span>{{ currentApproach === 'recursion' ? 'Rob vs Skip Decision Tree (Exponential Explosion O(2^n))' : 'Pruned Memoization Decision Tree (Linear O(n))' }}</span>
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
                              <rect x="-25" y="-6" width="50" height="12" rx="3" fill="#ef4444" stroke="#dc2626" stroke-width="0.5" />
                              <text x="0" y="0" text-anchor="middle" class="ll-badge-text-red">Recomputing!</text>
                            </g>
                            <!-- Cache Hit Badge Above Node -->
                            <g v-else-if="node.state === 'cache-hit'" transform="translate(0, -17)">
                              <rect x="-25" y="-6" width="50" height="12" rx="3" fill="#9333ea" stroke="#7e22ce" stroke-width="0.5" />
                              <text x="0" y="0" text-anchor="middle" class="ll-badge-text-purple">Cache Hit O(1)</text>
                            </g>

                            <!-- Node Outer Card -->
                            <rect
                              x="-24"
                              y="-12"
                              width="48"
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
                            <!-- Node Text: rob(i) -->
                            <text x="0" y="-3" text-anchor="middle" class="ll-node-text-call">
                              rob({{ node.i }})
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
                                node.retVal !== null ? `=$${node.retVal}` :
                                (node.state === 'cache-hit' ? '⚡ Hit' : '?')
                              }}
                            </text>
                          </g>
                        </g>
                      </svg>

                      <!-- Memoization Lookup Cache Table Strip (Only in Memoization Mode) -->
                      <div v-if="currentApproach === 'memoization' && st.memo" class="ll-memo-strip-wrap">
                        <div class="ll-memo-title">Memoization Cache Table &mdash; <code>memo[0...{{ st.n - 1 }}]</code>:</div>
                        <div class="ll-memo-strip">
                          <template v-for="(mVal, mIdx) in st.memo" :key="mIdx">
                            <div class="ll-memo-cell-wrap">
                              <div
                                class="ll-memo-cell"
                                :class="{
                                  'll-memo-hit': mVal !== -1,
                                  'll-memo-empty': mVal === -1
                                }"
                              >
                                {{ mVal !== -1 ? `$${mVal}` : '?' }}
                              </div>
                              <span class="ll-memo-idx">memo[{{ mIdx }}]</span>
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
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-base"></span>Base Cases (0, 1)</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-purple"></span>Rob Option (i-2)</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-cur"></span>Skip Option (i-1)</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-new"></span>Max Loot dp[i]</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-uncalc"></span>Uncalculated</span>
                </template>
                <template v-else-if="currentApproach === 'memoization'">
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-cur"></span>Active Subproblem</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-new"></span>Solved &amp; Cached</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-purple"></span>Cache Hit (O(1) Return)</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-uncalc"></span>Uncalled</span>
                </template>
                <template v-else>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-cur"></span>Active Frame</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-new"></span>Solved Optimal Loot</span>
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
                  <h3>Why Dynamic Programming? (House Robber Insight)</h3>
                  <p>
                    The <strong>House Robber</strong> problem illustrates a classic binary decision process (Rob vs Skip) with non-adjacent constraints:
                  </p>
                  <ul>
                    <li>
                      <strong>1. Optimal Substructure:</strong> At house <code>i</code>, the optimal decision breaks into two smaller subproblems:
                      <br />&bull; <strong>Option 1 (Skip house i):</strong> Max loot is <code>dp[i - 1]</code>
                      <br />&bull; <strong>Option 2 (Rob house i):</strong> Max loot is <code>nums[i] + dp[i - 2]</code>
                      <br /><code>dp[i] = max(dp[i - 1], nums[i] + dp[i - 2])</code>
                    </li>
                    <li>
                      <strong>2. Overlapping Subproblems:</strong> Naive recursion branches exponentially, repeatedly solving identical house prefixes (e.g. <code>rob(2)</code> and <code>rob(1)</code> are evaluated dozens of times across branches).
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
                        <td><span class="ll-c-green" style="font-weight:700">NO</span> (Keeps Only prev1 &amp; prev2)</td>
                      </tr>
                    </tbody>
                  </table>

                  <h3>Recurrence Formula Proof</h3>
                  <p>
                    For each house <code>i</code>, the thief chooses the maximum between skipping or robbing:
                  </p>
                  <p class="ll-math-box">
                    dp[i] = max(dp[i - 1], nums[i] + (i &ge; 2 ? dp[i - 2] : 0))
                  </p>
                  <p>
                    For <code>n = 40</code> houses, Naive Recursion requires over <strong>1 Billion</strong> recursive calls, while DP Tabulation finds the optimal solution in just <strong>40 operations</strong>!
                  </p>

                  <div class="ll-note">
                    <strong>Faculty Takeaway:</strong> Dynamic Programming transforms a combinatorial decision space into a linear sequence of locally optimal choices that guarantee global optimality without alarm triggers.
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
                    <span class="ll-modal-tag-entered">Entered: {{ requestedCount }} Houses</span>
                    <span class="ll-modal-arrow">&rarr;</span>
                    <span class="ll-modal-tag-applied">Adjusted to Max: {{ warningModalLimit }} Houses</span>
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
  display: flex; flex-direction: column; overflow: hidden; width: 100%; height:58vh;
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
.ll-text-input { background: var(--surface); border: 1px solid var(--border2); color: var(--text); border-radius: var(--radius-sm); padding: 3px 6px; font-size: 11.5px; font-family: monospace; width: 95px; }
.ll-text-input:focus { outline: none; border-color: var(--coral); box-shadow: 0 0 0 3px rgba(240,77,77,.1); }
.ll-input-hint { font-size: 10px; color: var(--muted); }

.ll-dropdown-wrap {
  position: relative;
  display: inline-block;
}
.ll-dropdown-btn {
  background: var(--surface2);
  border: 1px solid var(--border2);
  color: var(--text2);
  padding: 4px 10px;
  border-radius: var(--radius-sm);
  font-size: 11px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.15s ease;
  display: flex;
  align-items: center;
  gap: 4px;
  white-space: nowrap;
}
.ll-dropdown-btn:hover,
.ll-dropdown-btn.active {
  background: var(--surface);
  border-color: var(--coral);
  color: var(--coral);
}
.ll-dropdown-menu {
  position: absolute;
  top: calc(100% + 4px);
  left: 0;
  background: #ffffff;
  border: 1px solid var(--border2);
  border-radius: 6px;
  box-shadow: 0 4px 14px rgba(0, 0, 0, 0.14);
  padding: 4px;
  z-index: 9999;
  min-width: 175px;
  display: flex;
  flex-direction: column;
  gap: 2px;
  animation: ll-pop 0.15s ease;
}
.ll-dropdown-item {
  background: transparent;
  border: none;
  padding: 6px 10px;
  border-radius: 4px;
  text-align: left;
  cursor: pointer;
  display: flex;
  flex-direction: column;
  transition: background 0.15s ease;
  width: 100%;
}
.ll-dropdown-item:hover {
  background: var(--coral-light);
}
.ll-dropdown-item:hover .ll-drop-label {
  color: var(--coral);
}
.ll-drop-label {
  font-family: 'Consolas', monospace;
  font-size: 11.5px;
  font-weight: 700;
  color: var(--text);
}
.ll-drop-desc {
  font-size: 9.5px;
  color: var(--muted);
  margin-top: 1px;
}

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

/* Top Houses Banner */
.ll-house-banner { padding: 4px 10px; background: var(--surface2); border-bottom: 1px solid var(--border); flex-shrink: 0; }
.ll-house-banner-title { font-size: 10px; font-weight: 700; color: var(--text2); margin-bottom: 3px; }
.ll-house-track { display: flex; gap: 8px; overflow-x: auto; padding: 2px 0; }
.ll-house-card { display: flex; flex-direction: column; align-items: center; background: var(--surface); border: 1.5px solid var(--border2); border-radius: 6px; padding: 3px 6px; min-width: 52px; transition: all .2s ease; box-shadow: var(--shadow-sm); }
.ll-house-roof { font-size: 10px; color: #ef5050; line-height: 1; margin-bottom: -2px; }
.ll-house-body { display: flex; align-items: center; gap: 3px; }
.ll-house-icon { font-size: 13px; }
.ll-house-val { font-family: monospace; font-size: 11px; font-weight: 800; color: #047857; }
.ll-house-idx { font-size: 9px; color: var(--muted); margin-top: 2px; }
.ll-house-active { border-color: #f59e0b !important; background: #fffbeb !important; transform: translateY(-2px); box-shadow: 0 0 0 2.5px rgba(245, 158, 11, 0.3) !important; }
.ll-house-skip { border-color: #f97316 !important; background: #fff7ed !important; }
.ll-house-rob { border-color: #9333ea !important; background: #f3e8ff !important; }

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
.ll-ptr-lbl { font-size: 10px; font-weight: 800; font-family: 'Consolas', 'Fira Code', monospace; display: inline-flex; align-items: center; line-height: 1; white-space: nowrap; animation: ll-pop 0.2s ease; }
.ll-lbl-orange { color: #f97316; } .ll-lbl-purple { color: #9333ea; } .ll-lbl-blue { color: #2563eb; }

.ll-arr-box { width: 50px; height: 46px; display: flex; align-items: center; justify-content: center; border: 2px solid var(--border2); border-radius: var(--radius); background: var(--surface); color: var(--text); font-weight: 700; font-size: 13.5px; box-shadow: var(--shadow-sm); transition: all 0.25s ease; animation: ll-pop .3s ease; }
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
  font-size: 9.5px !important;
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
.ll-memo-cell { width: 44px; height: 26px; display: flex; align-items: center; justify-content: center; font-size: 11px; font-family: monospace; font-weight: 700; border-radius: 4px; border: 1px solid var(--border); background: var(--surface); color: var(--text); }
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
