<script setup>
import { ref, computed, reactive, onMounted, onUnmounted, nextTick } from 'vue';

/* ==================================================================== */
/* PROPS & COMPONENT CONFIGURATION                                      */
/* ==================================================================== */
const props = defineProps({
  topic: { type: String, default: 'Dynamic Programming' },
  subTopic: { type: String, default: '0/1 Knapsack Problem' }
});

/* ==================================================================== */
/* APPROACHES & MULTI-LANGUAGE 100% EXECUTABLE CODE SPECIFICATIONS      */
/* ==================================================================== */
const APPROACHES = [
  { id: 'recursion', label: 'Brute Force', desc: 'O(2^N) Exponential — Include vs Exclude 2-Way Branch Decisions' },
  { id: 'memoization', label: 'Memoization', desc: 'O(N×W) Time & O(N×W) Space — Top-Down with 2D State Cache' },
  { id: 'tabulation', label: 'Tabulation', desc: 'O(N×W) Time & O(N×W) Space — Bottom-Up 2D DP Table with Item Traceback' }
];

const PRESETS = [
  {
    label: '4 Items (Cap: 7kg)',
    desc: '4 Items: [Watch, Ring, Camera, Phone] (Cap: 7kg)',
    capacity: 7,
    items: [
      { id: 1, name: 'Watch', weight: 1, val: 15 },
      { id: 2, name: 'Ring', weight: 3, val: 50 },
      { id: 3, name: 'Camera', weight: 4, val: 60 },
      { id: 4, name: 'Phone', weight: 5, val: 90 }
    ]
  },
  {
    label: '3 Items (Cap: 5kg)',
    desc: '3 Items: [Item A, Item B, Item C] (Cap: 5kg)',
    capacity: 5,
    items: [
      { id: 1, name: 'Item A', weight: 1, val: 6 },
      { id: 2, name: 'Item B', weight: 2, val: 10 },
      { id: 3, name: 'Item C', weight: 3, val: 12 }
    ]
  },
  {
    label: '4 Items (Cap: 5kg)',
    desc: '4 Items: [Book, Torch, Kit, Shoes] (Cap: 5kg)',
    capacity: 5,
    items: [
      { id: 1, name: 'Book', weight: 2, val: 12 },
      { id: 2, name: 'Torch', weight: 1, val: 10 },
      { id: 3, name: 'Kit', weight: 3, val: 20 },
      { id: 4, name: 'Shoes', weight: 2, val: 15 }
    ]
  },
  {
    label: '3 Gems (Cap: 6kg)',
    desc: '3 Gems: [Ruby, Sapphire, Diamond] (Cap: 6kg)',
    capacity: 6,
    items: [
      { id: 1, name: 'Ruby', weight: 2, val: 30 },
      { id: 2, name: 'Sapphire', weight: 3, val: 40 },
      { id: 3, name: 'Diamond', weight: 4, val: 70 }
    ]
  }
];

const CODES = {
  recursion: {
    java: [
      ['',              'import java.util.Scanner;'],
      ['',              ''],
      ['',              'public class Main {'],
      ['c_entry',       '    static int knapRec(int i, int w, int[] wt, int[] val) {'],
      ['c_base',        '        if (i == 0 || w == 0) return 0;'],
      ['c_weight_chk',  '        if (wt[i - 1] > w) {'],
      ['c_excl_heavy',  '            return knapRec(i - 1, w, wt, val);'],
      ['c_branch',      '        } else {'],
      ['c_excl_rec',    '            int exclude = knapRec(i - 1, w, wt, val);'],
      ['c_incl_rec',    '            int include = val[i - 1] + knapRec(i - 1, w - wt[i - 1], wt, val);'],
      ['c_ret_max',     '            return Math.max(exclude, include);'],
      ['',              '        }'],
      ['',              '    }'],
      ['',              ''],
      ['',              '    public static void main(String[] args) {'],
      ['m_scanner',     '        Scanner sc = new Scanner(System.in);'],
      ['m_read_input',  '        int n = sc.nextInt(), W = sc.nextInt();'],
      ['',              '        int[] val = new int[n], wt = new int[n];'],
      ['',              '        for (int i = 0; i < n; i++) { val[i] = sc.nextInt(); wt[i] = sc.nextInt(); }'],
      ['m_call_knap',   '        int maxVal = knapRec(n, W, wt, val);'],
      ['m_print',       '        System.out.println(maxVal);'],
      ['m_done',        '    }'],
      ['',              '}']
    ],
    c: [
      ['',              '#include <stdio.h>'],
      ['',              '#define MAX(a, b) ((a) > (b) ? (a) : (b))'],
      ['',              ''],
      ['c_entry',       'int knapRec(int i, int w, int wt[], int val[]) {'],
      ['c_base',        '    if (i == 0 || w == 0) return 0;'],
      ['c_weight_chk',  '    if (wt[i - 1] > w)'],
      ['c_excl_heavy',  '        return knapRec(i - 1, w, wt, val);'],
      ['c_branch',      '    else'],
      ['c_ret_max',     '        return MAX(knapRec(i - 1, w, wt, val), val[i - 1] + knapRec(i - 1, w - wt[i - 1], wt, val));'],
      ['',              '}'],
      ['',              ''],
      ['int main() {'],
      ['m_read_input',  '    int n, W; scanf("%d %d", &n, &W);'],
      ['',              '    int val[n], wt[n];'],
      ['',              '    for (int i = 0; i < n; i++) scanf("%d %d", &val[i], &wt[i]);'],
      ['m_call_knap',   '    int maxVal = knapRec(n, W, wt, val);'],
      ['m_print',       '    printf("%d\\n", maxVal);'],
      ['m_done',        '    return 0;'],
      ['',              '}']
    ],
    cpp: [
      ['',              '#include <iostream>'],
      ['',              '#include <vector>'],
      ['',              '#include <algorithm>'],
      ['',              'using namespace std;'],
      ['',              ''],
      ['c_entry',       'int knapRec(int i, int w, const vector<int>& wt, const vector<int>& val) {'],
      ['c_base',        '    if (i == 0 || w == 0) return 0;'],
      ['c_weight_chk',  '    if (wt[i - 1] > w)'],
      ['c_excl_heavy',  '        return knapRec(i - 1, w, wt, val);'],
      ['c_branch',      '    int exclude = knapRec(i - 1, w, wt, val);'],
      ['c_incl_rec',    '    int include = val[i - 1] + knapRec(i - 1, w - wt[i - 1], wt, val);'],
      ['c_ret_max',     '    return max(exclude, include);'],
      ['',              '}'],
      ['',              ''],
      ['int main() {'],
      ['m_read_input',  '    int n, W; cin >> n >> W;'],
      ['',              '    vector<int> val(n), wt(n);'],
      ['',              '    for (int i = 0; i < n; i++) cin >> val[i] >> wt[i];'],
      ['m_call_knap',   '    int maxVal = knapRec(n, W, wt, val);'],
      ['m_print',       '    cout << maxVal << endl;'],
      ['m_done',        '    return 0;'],
      ['',              '}']
    ],
    python: [
      ['',              'import sys'],
      ['',              ''],
      ['c_entry',       'def knap_rec(i, w, wt, val):'],
      ['c_base',        '    if i == 0 or w == 0:'],
      ['',              '        return 0'],
      ['c_weight_chk',  '    if wt[i - 1] > w:'],
      ['c_excl_heavy',  '        return knap_rec(i - 1, w, wt, val)'],
      ['c_branch',      '    exclude = knap_rec(i - 1, w, wt, val)'],
      ['c_incl_rec',    '    include = val[i - 1] + knap_rec(i - 1, w - wt[i - 1], wt, val)'],
      ['c_ret_max',     '    return max(exclude, include)'],
      ['',              ''],
      ['if __name__ == "__main__":'],
      ['m_read_input',  '    data = list(map(int, sys.stdin.read().split()))'],
      ['',              '    n, W = data[0], data[1]'],
      ['m_call_knap',   '    max_val = knap_rec(n, W, wt, val)'],
      ['m_print',       '    print(max_val)'],
      ['m_done',        '    sys.exit(0)']
    ],
    javascript: [
      ['',              'const fs = require("fs");'],
      ['',              ''],
      ['c_entry',       'function knapRec(i, w, wt, val) {'],
      ['c_base',        '    if (i === 0 || w === 0) return 0;'],
      ['c_weight_chk',  '    if (wt[i - 1] > w) {'],
      ['c_excl_heavy',  '        return knapRec(i - 1, w, wt, val);'],
      ['c_branch',      '    }'],
      ['c_excl_rec',    '    const exclude = knapRec(i - 1, w, wt, val);'],
      ['c_incl_rec',    '    const include = val[i - 1] + knapRec(i - 1, w - wt[i - 1], wt, val);'],
      ['c_ret_max',     '    return Math.max(exclude, include);'],
      ['',              '}'],
      ['',              ''],
      ['const maxVal = knapRec(n, W, wt, val);'],
      ['console.log(maxVal);']
    ]
  },
  memoization: {
    java: [
      ['',              'import java.util.Scanner;'],
      ['',              'import java.util.Arrays;'],
      ['',              ''],
      ['',              'public class Main {'],
      ['c_entry',       '    static int knapMemo(int i, int w, int[] wt, int[] val, int[][] memo) {'],
      ['c_base',        '        if (i == 0 || w == 0) return 0;'],
      ['c_memo_check',  '        if (memo[i][w] != -1) return memo[i][w];'],
      ['c_weight_chk',  '        if (wt[i - 1] > w) {'],
      ['c_excl_calc',   '            return memo[i][w] = knapMemo(i - 1, w, wt, val, memo);'],
      ['c_branch',      '        } else {'],
      ['c_memo_calc',   '            int exclude = knapMemo(i - 1, w, wt, val, memo);'],
      ['',              '            int include = val[i - 1] + knapMemo(i - 1, w - wt[i - 1], wt, val, memo);'],
      ['c_ret_memo',    '            return memo[i][w] = Math.max(exclude, include);'],
      ['',              '        }'],
      ['',              '    }'],
      ['',              ''],
      ['',              '    public static void main(String[] args) {'],
      ['m_scanner',     '        Scanner sc = new Scanner(System.in);'],
      ['m_read_input',  '        int n = sc.nextInt(), W = sc.nextInt();'],
      ['m_alloc_memo',  '        int[][] memo = new int[n + 1][W + 1];'],
      ['m_fill_memo',   '        for (int[] row : memo) Arrays.fill(row, -1);'],
      ['m_call_knap',   '        int maxVal = knapMemo(n, W, wt, val, memo);'],
      ['m_print',       '        System.out.println(maxVal);'],
      ['m_done',        '    }'],
      ['',              '}']
    ],
    c: [
      ['',              '#include <stdio.h>'],
      ['',              '#include <string.h>'],
      ['',              '#define MAX(a, b) ((a) > (b) ? (a) : (b))'],
      ['',              ''],
      ['c_entry',       'int knapMemo(int i, int w, int wt[], int val[], int memo[20][20]) {'],
      ['c_base',        '    if (i == 0 || w == 0) return 0;'],
      ['c_memo_check',  '    if (memo[i][w] != -1) return memo[i][w];'],
      ['c_weight_chk',  '    if (wt[i - 1] > w)'],
      ['c_excl_calc',   '        return memo[i][w] = knapMemo(i - 1, w, wt, val, memo);'],
      ['c_memo_calc',   '    return memo[i][w] = MAX(knapMemo(i - 1, w, wt, val, memo), val[i - 1] + knapMemo(i - 1, w - wt[i - 1], wt, val, memo));'],
      ['',              '}'],
      ['',              ''],
      ['int main() {'],
      ['m_alloc_memo',  '    int memo[20][20]; memset(memo, -1, sizeof(memo));'],
      ['m_call_knap',   '    int maxVal = knapMemo(n, W, wt, val, memo);'],
      ['m_print',       '    printf("%d\\n", maxVal);'],
      ['m_done',        '    return 0;'],
      ['',              '}']
    ],
    cpp: [
      ['',              '#include <iostream>'],
      ['',              '#include <vector>'],
      ['',              '#include <algorithm>'],
      ['',              'using namespace std;'],
      ['',              ''],
      ['c_entry',       'int knapMemo(int i, int w, const vector<int>& wt, const vector<int>& val, vector<vector<int>>& memo) {'],
      ['c_base',        '    if (i == 0 || w == 0) return 0;'],
      ['c_memo_check',  '    if (memo[i][w] != -1) return memo[i][w];'],
      ['c_weight_chk',  '    if (wt[i - 1] > w)'],
      ['c_excl_calc',   '        return memo[i][w] = knapMemo(i - 1, w, wt, val, memo);'],
      ['c_memo_calc',   '    return memo[i][w] = max(knapMemo(i - 1, w, wt, val, memo), val[i - 1] + knapMemo(i - 1, w - wt[i - 1], wt, val, memo));'],
      ['',              '}'],
      ['',              ''],
      ['int main() {'],
      ['m_alloc_memo',  '    vector<vector<int>> memo(n + 1, vector<int>(W + 1, -1));'],
      ['m_call_knap',   '    int maxVal = knapMemo(n, W, wt, val, memo);'],
      ['m_print',       '    cout << maxVal << endl;'],
      ['m_done',        '    return 0;'],
      ['',              '}']
    ],
    python: [
      ['',              'import sys'],
      ['',              ''],
      ['c_entry',       'def knap_memo(i, w, wt, val, memo):'],
      ['c_base',        '    if i == 0 or w == 0:'],
      ['',              '        return 0'],
      ['c_memo_check',  '    if memo[i][w] != -1:'],
      ['',              '        return memo[i][w]'],
      ['c_weight_chk',  '    if wt[i - 1] > w:'],
      ['c_excl_calc',   '        memo[i][w] = knap_memo(i - 1, w, wt, val, memo)'],
      ['c_branch',      '    else:'],
      ['c_memo_calc',   '        memo[i][w] = max(knap_memo(i - 1, w, wt, val, memo), val[i - 1] + knap_memo(i - 1, w - wt[i - 1], wt, val, memo))'],
      ['c_ret_memo',    '    return memo[i][w]'],
      ['',              ''],
      ['if __name__ == "__main__":'],
      ['m_alloc_memo',  '    memo = [[-1] * (W + 1) for _ in range(n + 1)]'],
      ['m_call_knap',   '    max_val = knap_memo(n, W, wt, val, memo)'],
      ['m_print',       '    print(max_val)'],
      ['m_done',        '    sys.exit(0)']
    ],
    javascript: [
      ['',              'const fs = require("fs");'],
      ['',              ''],
      ['c_entry',       'function knapMemo(i, w, wt, val, memo) {'],
      ['c_base',        '    if (i === 0 || w === 0) return 0;'],
      ['c_memo_check',  '    if (memo[i][w] !== -1) return memo[i][w];'],
      ['c_weight_chk',  '    if (wt[i - 1] > w) {'],
      ['c_excl_calc',   '        return memo[i][w] = knapMemo(i - 1, w, wt, val, memo);'],
      ['c_branch',      '    }'],
      ['c_memo_calc',   '    return memo[i][w] = Math.max(knapMemo(i - 1, w, wt, val, memo), val[i - 1] + knapMemo(i - 1, w - wt[i - 1], wt, val, memo));'],
      ['',              '}']
    ]
  },
  tabulation: {
    java: [
      ['',              'import java.util.Scanner;'],
      ['',              ''],
      ['',              'public class Main {'],
      ['c_entry',       '    static int knapTab(int W, int[] wt, int[] val, int n) {'],
      ['c_alloc_dp',    '        int[][] dp = new int[n + 1][W + 1];'],
      ['c_loop_i',      '        for (int i = 1; i <= n; i++) {'],
      ['c_loop_w',      '            for (int w = 1; w <= W; w++) {'],
      ['c_weight_chk',  '                if (wt[i - 1] <= w) {'],
      ['c_dp_incl',     '                    dp[i][w] = Math.max(dp[i - 1][w], val[i - 1] + dp[i - 1][w - wt[i - 1]]);'],
      ['c_weight_heavy','                } else {'],
      ['c_dp_excl',     '                    dp[i][w] = dp[i - 1][w];'],
      ['',              '                }'],
      ['',              '            }'],
      ['',              '        }'],
      ['c_ret_dp',      '        return dp[n][W];'],
      ['',              '    }'],
      ['',              ''],
      ['',              '    public static void main(String[] args) {'],
      ['m_scanner',     '        Scanner sc = new Scanner(System.in);'],
      ['m_read_input',  '        int n = sc.nextInt(), W = sc.nextInt();'],
      ['',              '        int[] val = new int[n], wt = new int[n];'],
      ['',              '        for (int i = 0; i < n; i++) { val[i] = sc.nextInt(); wt[i] = sc.nextInt(); }'],
      ['m_call_knap',   '        int maxVal = knapTab(W, wt, val, n);'],
      ['m_print',       '        System.out.println(maxVal);'],
      ['m_done',        '    }'],
      ['',              '}']
    ],
    c: [
      ['',              '#include <stdio.h>'],
      ['',              '#include <string.h>'],
      ['',              '#define MAX(a, b) ((a) > (b) ? (a) : (b))'],
      ['',              ''],
      ['c_entry',       'int knapTab(int W, int wt[], int val[], int n) {'],
      ['c_alloc_dp',    '    int dp[n + 1][W + 1]; memset(dp, 0, sizeof(dp));'],
      ['c_loop_i',      '    for (int i = 1; i <= n; i++) {'],
      ['c_loop_w',      '        for (int w = 1; w <= W; w++) {'],
      ['c_weight_chk',  '            if (wt[i - 1] <= w)'],
      ['c_dp_incl',     '                dp[i][w] = MAX(dp[i - 1][w], val[i - 1] + dp[i - 1][w - wt[i - 1]]);'],
      ['c_weight_heavy','            else'],
      ['c_dp_excl',     '                dp[i][w] = dp[i - 1][w];'],
      ['',              '        }'],
      ['',              '    }'],
      ['c_ret_dp',      '    return dp[n][W];'],
      ['',              '}'],
      ['',              ''],
      ['int main() {'],
      ['m_call_knap',   '    int maxVal = knapTab(W, wt, val, n);'],
      ['m_print',       '    printf("%d\\n", maxVal);'],
      ['m_done',        '    return 0;'],
      ['',              '}']
    ],
    cpp: [
      ['',              '#include <iostream>'],
      ['',              '#include <vector>'],
      ['',              '#include <algorithm>'],
      ['',              'using namespace std;'],
      ['',              ''],
      ['c_entry',       'int knapTab(int W, const vector<int>& wt, const vector<int>& val, int n) {'],
      ['c_alloc_dp',    '    vector<vector<int>> dp(n + 1, vector<int>(W + 1, 0));'],
      ['c_loop_i',      '    for (int i = 1; i <= n; i++) {'],
      ['c_loop_w',      '        for (int w = 1; w <= W; w++) {'],
      ['c_weight_chk',  '            if (wt[i - 1] <= w)'],
      ['c_dp_incl',     '                dp[i][w] = max(dp[i - 1][w], val[i - 1] + dp[i - 1][w - wt[i - 1]]);'],
      ['c_weight_heavy','            else'],
      ['c_dp_excl',     '                dp[i][w] = dp[i - 1][w];'],
      ['',              '        }'],
      ['',              '    }'],
      ['c_ret_dp',      '    return dp[n][W];'],
      ['',              '}'],
      ['',              ''],
      ['int main() {'],
      ['m_call_knap',   '    int maxVal = knapTab(W, wt, val, n);'],
      ['m_print',       '    cout << maxVal << endl;'],
      ['m_done',        '    return 0;'],
      ['',              '}']
    ],
    python: [
      ['',              'import sys'],
      ['',              ''],
      ['c_entry',       'def knap_tab(W, wt, val, n):'],
      ['c_alloc_dp',    '    dp = [[0] * (W + 1) for _ in range(n + 1)]'],
      ['c_loop_i',      '    for i in range(1, n + 1):'],
      ['c_loop_w',      '        for w in range(1, W + 1):'],
      ['c_weight_chk',  '            if wt[i - 1] <= w:'],
      ['c_dp_incl',     '                dp[i][w] = max(dp[i - 1][w], val[i - 1] + dp[i - 1][w - wt[i - 1]])'],
      ['c_weight_heavy','            else:'],
      ['c_dp_excl',     '                dp[i][w] = dp[i - 1][w]'],
      ['c_ret_dp',      '    return dp[n][W]'],
      ['',              ''],
      ['if __name__ == "__main__":'],
      ['m_call_knap',   '    max_val = knap_tab(W, wt, val, n)'],
      ['m_print',       '    print(max_val)'],
      ['m_done',        '    sys.exit(0)']
    ],
    javascript: [
      ['',              'function knapTab(W, wt, val, n) {'],
      ['c_alloc_dp',    '    const dp = Array.from({ length: n + 1 }, () => new Array(W + 1).fill(0));'],
      ['c_loop_i',      '    for (let i = 1; i <= n; i++) {'],
      ['c_loop_w',      '        for (let w = 1; w <= W; w++) {'],
      ['c_weight_chk',  '            if (wt[i - 1] <= w) {'],
      ['c_dp_incl',     '                dp[i][w] = Math.max(dp[i - 1][w], val[i - 1] + dp[i - 1][w - wt[i - 1]]);'],
      ['c_weight_heavy','            } else {'],
      ['c_dp_excl',     '                dp[i][w] = dp[i - 1][w];'],
      ['',              '            }'],
      ['',              '        }'],
      ['',              '    }'],
      ['c_ret_dp',      '    return dp[n][W];'],
      ['',              '}']
    ]
  }
};

const PSEUDOCODES = {
  recursion: [
    'Algorithm Knapsack_BruteForce(i, w, wt[], val[]):',
    '  if i == 0 or w == 0 then:',
    '    return 0',
    '  if wt[i - 1] > w then:',
    '    return Knapsack_BruteForce(i - 1, w, wt, val) // Exclude (Cannot fit)',
    '  else:',
    '    exclude = Knapsack_BruteForce(i - 1, w, wt, val)',
    '    include = val[i - 1] + Knapsack_BruteForce(i - 1, w - wt[i - 1], wt, val)',
    '    return max(exclude, include)'
  ],
  memoization: [
    'Algorithm Knapsack_Memo(i, w, wt[], val[], memo[][]):',
    '  if i == 0 or w == 0 then return 0',
    '  if memo[i][w] != -1 then return memo[i][w] // Cache Hit O(1)',
    '  if wt[i - 1] > w then:',
    '    memo[i][w] = Knapsack_Memo(i - 1, w, wt, val, memo)',
    '  else:',
    '    exclude = Knapsack_Memo(i - 1, w, wt, val, memo)',
    '    include = val[i - 1] + Knapsack_Memo(i - 1, w - wt[i - 1], wt, val, memo)',
    '    memo[i][w] = max(exclude, include)',
    '  return memo[i][w]'
  ],
  tabulation: [
    'Algorithm Knapsack_Tabulation(W, wt[], val[], n):',
    '  Table dp[0...n][0...W] initialized to 0',
    '  for i from 1 to n do: // For each item',
    '    for w from 1 to W do: // For each capacity',
    '      if wt[i - 1] <= w then:',
    '        dp[i][w] = max(dp[i - 1][w], val[i - 1] + dp[i - 1][w - wt[i - 1]])',
    '      else:',
    '        dp[i][w] = dp[i - 1][w]',
    '  Reconstruct chosen items by backtracking from dp[n][W]',
    '  return dp[n][W]'
  ]
};

/* ==================================================================== */
/* STATIC TREE BUILDER (FOR RECURSION & MEMOIZATION)                    */
/* ==================================================================== */
function generateStaticTree(approach, items, capacity) {
  const nodes = [];
  const edges = [];
  let nodeId = 0;
  const memoSeen = new Set();

  function buildNode(i, w, depth, parentId = null, isLeft = true) {
    const myId = nodeId++;
    const label = `knap(${i}, ${w})`;
    const subKey = `${i},${w}`;
    const isMemoPruned = approach === 'memoization' && memoSeen.has(subKey);

    const nd = {
      id: myId,
      i,
      w,
      label,
      depth,
      state: 'unvisited',
      retVal: null,
      isMemoPruned,
      children: []
    };
    nodes.push(nd);

    if (parentId !== null) {
      edges.push({ from: parentId, to: myId, isLeft });
      const parentNode = nodes.find(n => n.id === parentId);
      if (parentNode) parentNode.children.push(myId);
    }

    if (isMemoPruned) return myId;
    if (approach === 'memoization') memoSeen.add(subKey);

    if (i === 0 || w === 0) return myId;

    const curWt = items[i - 1].weight;
    if (curWt > w) {
      // Only Exclude branch
      buildNode(i - 1, w, depth + 1, myId, true);
    } else {
      // 2 branches: Exclude (Left) & Include (Right)
      buildNode(i - 1, w, depth + 1, myId, true);
      buildNode(i - 1, w - curWt, depth + 1, myId, false);
    }
    return myId;
  }

  buildNode(items.length, capacity, 0);

  // Compute Layout
  const nodeMap = new Map();
  nodes.forEach(nd => nodeMap.set(nd.id, nd));

  let leafCounter = 0;
  function assignSubtreeMetrics(nId, depth) {
    const nd = nodeMap.get(nId);
    if (!nd) return;
    nd.depth = depth;
    if (!nd.children || !nd.children.length) {
      nd.leafIndex = leafCounter++;
    } else {
      nd.children.forEach(cId => assignSubtreeMetrics(cId, depth + 1));
    }
  }
  if (nodes.length > 0) assignSubtreeMetrics(0, 0);

  const totalLeaves = Math.max(1, leafCounter);
  const maxDepth = Math.max(0, ...Array.from(nodeMap.values()).map(nd => nd.depth || 0));

  const leafSpacing = 68;
  const levelHeight = 44;
  const computedWidth = Math.max(220, totalLeaves * leafSpacing + 40);
  const computedHeight = Math.max(80, maxDepth * levelHeight + 45);

  function assignCoords(nId) {
    const nd = nodeMap.get(nId);
    if (!nd) return 0;
    if (!nd.children || !nd.children.length) {
      nd.x = 24 + nd.leafIndex * leafSpacing + leafSpacing / 2;
    } else {
      const childXs = nd.children.map(cId => assignCoords(cId));
      nd.x = (childXs[0] + childXs[childXs.length - 1]) / 2;
    }
    nd.y = 22 + nd.depth * levelHeight;
    return nd.x;
  }
  if (nodes.length > 0) assignCoords(0);

  const layoutMap = {};
  nodeMap.forEach((nd, id) => {
    layoutMap[id] = { x: nd.x, y: nd.y, depth: nd.depth };
  });

  return { nodes, edges, layoutMap, width: computedWidth, height: computedHeight };
}

/* ==================================================================== */
/* STRICT ZERO-SKIP LINE-BY-LINE EXECUTION STEP GENERATOR               */
/* ==================================================================== */
function buildSteps(approach, rawItems, rawCapacity) {
  const steps = [];
  const maxItemsLimit = approach === 'tabulation' ? 6 : 4;
  const maxCapLimit = approach === 'tabulation' ? 10 : 7;

  const items = (rawItems && rawItems.length > 0 ? rawItems : PRESETS[0].items).slice(0, maxItemsLimit);
  const capacity = Math.min(rawCapacity || 7, maxCapLimit);
  const n = items.length;
  const W = capacity;

  /* ------------------------------------------------------------------ */
  /* APPROACH 1: BRUTE FORCE RECURSION                                   */
  /* ------------------------------------------------------------------ */
  if (approach === 'recursion') {
    const staticTree = generateStaticTree('recursion', items, capacity);
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
    const visitedSubproblems = new Set();

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
        y1: staticTree.layoutMap[e.from].y + 10,
        x2: staticTree.layoutMap[e.to].x,
        y2: staticTree.layoutMap[e.to].y - 10
      }));
    }

    // Step 0: Read input
    steps.push({
      code: 'm_read_input',
      badge: `Knapsack Input: ${n} items, Capacity W = ${W}kg. Starting Brute-Force Recursion`,
      badgeType: 'info',
      state: {
        items, W, n,
        totalCalls: 0,
        redundantCalls: 0,
        treeNodes: [],
        treeEdges: [],
        treeWidth: staticTree.width,
        treeHeight: staticTree.height,
        activeNodeId: null,
        activeHouseIdx: null,
        currentReturn: null,
        frames: []
      }
    });

    // Step 1: Call knapRec
    steps.push({
      code: 'm_call_knap',
      badge: `main() invokes knapRec(i = ${n}, w = ${W})`,
      badgeType: 'info',
      state: {
        items, W, n,
        totalCalls: 0,
        redundantCalls: 0,
        treeNodes: [],
        treeEdges: [],
        treeWidth: staticTree.width,
        treeHeight: staticTree.height,
        activeNodeId: null,
        activeHouseIdx: null,
        currentReturn: null,
        frames: [{ name: 'main()', args: `n=${n}, W=${W}` }]
      }
    });

    function solveRec(i, w) {
      const myId = nextNodeId++;
      callCounter++;
      const subKey = `${i},${w}`;
      const isRedundant = visitedSubproblems.has(subKey);
      if (isRedundant) redundantCounter++;
      visitedSubproblems.add(subKey);

      nodeStateMap[myId].state = 'active';
      nodeStateMap[myId].isRedundant = isRedundant;

      stackFrames.push({ name: `knap(i=${i}, w=${w})`, args: i > 0 ? `Item:${items[i-1].name}, Cap:${w}kg` : `Base` });

      steps.push({
        code: 'c_entry',
        badge: `Entering knap(i=${i}, w=${w}) &mdash; considering first ${i} items with ${w}kg capacity`,
        badgeType: isRedundant ? 'warn' : 'info',
        state: {
          items, W, n, curI: i, curW: w,
          curItem: i > 0 ? items[i - 1] : null,
          activeHouseIdx: i > 0 ? i - 1 : null,
          totalCalls: callCounter,
          redundantCalls: redundantCounter,
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height,
          activeNodeId: myId,
          currentReturn: null,
          frames: [...stackFrames]
        }
      });

      // Base Case Check
      steps.push({
        code: 'c_base',
        badge: i === 0 || w === 0
          ? `Base Case Reached! (i=${i} || w=${w}) &rarr; 0 items remaining or 0 capacity, return 0`
          : `Base Case Check: (i=${i} != 0 && w=${w} != 0) &rarr; Proceeding to weight check`,
        badgeType: i === 0 || w === 0 ? 'success' : 'info',
        state: {
          items, W, n, curI: i, curW: w,
          curItem: i > 0 ? items[i - 1] : null,
          activeHouseIdx: i > 0 ? i - 1 : null,
          totalCalls: callCounter,
          redundantCalls: redundantCounter,
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height,
          activeNodeId: myId,
          currentReturn: i === 0 || w === 0 ? 0 : null,
          frames: [...stackFrames]
        }
      });

      if (i === 0 || w === 0) {
        nodeStateMap[myId].state = 'solved';
        nodeStateMap[myId].retVal = 0;
        stackFrames.pop();
        return 0;
      }

      const curItem = items[i - 1];

      // Weight Check
      const isHeavy = curItem.weight > w;
      steps.push({
        code: 'c_weight_chk',
        badge: isHeavy
          ? `Weight Check: Item #${i} ("${curItem.name}", wt=${curItem.weight}kg) > remaining capacity (${w}kg) &rarr; CANNOT INCLUDE`
          : `Weight Check: Item #${i} ("${curItem.name}", wt=${curItem.weight}kg) &le; remaining capacity (${w}kg) &rarr; Branching Include vs Exclude`,
        badgeType: isHeavy ? 'warn' : 'info',
        state: {
          items, W, n, curI: i, curW: w,
          curItem,
          activeHouseIdx: i - 1,
          isHeavy,
          totalCalls: callCounter,
          redundantCalls: redundantCounter,
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height,
          activeNodeId: myId,
          currentReturn: null,
          frames: [...stackFrames]
        }
      });

      let ans = 0;
      if (isHeavy) {
        steps.push({
          code: 'c_excl_heavy',
          badge: `Must Exclude Item #${i} ("${curItem.name}") &rarr; Recursively solving knap(i=${i - 1}, w=${w})`,
          badgeType: 'info',
          state: {
            items, W, n, curI: i, curW: w, curItem,
            activeHouseIdx: i - 1,
            totalCalls: callCounter,
            redundantCalls: redundantCounter,
            treeNodes: getVisibleNodes(),
            treeEdges: getVisibleEdges(),
            treeWidth: staticTree.width,
            treeHeight: staticTree.height,
            activeNodeId: myId,
            currentReturn: null,
            frames: [...stackFrames]
          }
        });

        ans = solveRec(i - 1, w);
      } else {
        steps.push({
          code: 'c_excl_rec',
          badge: `Branch 1 (Exclude Item #${i} "${curItem.name}"): Try skipping &rarr; knap(i=${i - 1}, w=${w})`,
          badgeType: 'info',
          state: {
            items, W, n, curI: i, curW: w, curItem,
            activeHouseIdx: i - 1,
            totalCalls: callCounter,
            redundantCalls: redundantCounter,
            treeNodes: getVisibleNodes(),
            treeEdges: getVisibleEdges(),
            treeWidth: staticTree.width,
            treeHeight: staticTree.height,
            activeNodeId: myId,
            currentReturn: null,
            frames: [...stackFrames]
          }
        });

        const exclVal = solveRec(i - 1, w);

        steps.push({
          code: 'c_incl_rec',
          badge: `Branch 2 (Include Item #${i} "${curItem.name}"): Take val $${curItem.val} + knap(i=${i - 1}, w=${w - curItem.weight})`,
          badgeType: 'info',
          state: {
            items, W, n, curI: i, curW: w, curItem,
            activeHouseIdx: i - 1,
            exclVal,
            totalCalls: callCounter,
            redundantCalls: redundantCounter,
            treeNodes: getVisibleNodes(),
            treeEdges: getVisibleEdges(),
            treeWidth: staticTree.width,
            treeHeight: staticTree.height,
            activeNodeId: myId,
            currentReturn: null,
            frames: [...stackFrames]
          }
        });

        const inclVal = curItem.val + solveRec(i - 1, w - curItem.weight);
        ans = Math.max(exclVal, inclVal);

        steps.push({
          code: 'c_ret_max',
          badge: `Decision for knap(${i}, ${w}): max(Exclude: $${exclVal}, Include: $${inclVal}) = $${ans}`,
          badgeType: 'success',
          state: {
            items, W, n, curI: i, curW: w, curItem,
            activeHouseIdx: i - 1,
            exclVal, inclVal,
            totalCalls: callCounter,
            redundantCalls: redundantCounter,
            treeNodes: getVisibleNodes(),
            treeEdges: getVisibleEdges(),
            treeWidth: staticTree.width,
            treeHeight: staticTree.height,
            activeNodeId: myId,
            currentReturn: ans,
            frames: [...stackFrames]
          }
        });
      }

      nodeStateMap[myId].state = 'solved';
      nodeStateMap[myId].retVal = ans;
      stackFrames.pop();
      return ans;
    }

    const finalResult = solveRec(n, W);

    steps.push({
      code: 'm_print',
      badge: `Brute Force Finished! Max Knapsack Value = $${finalResult} (Total Calls: ${callCounter}, Redundant: ${redundantCounter})`,
      badgeType: 'success',
      state: {
        items, W, n,
        totalCalls: callCounter,
        redundantCalls: redundantCounter,
        treeNodes: getVisibleNodes(),
        treeEdges: getVisibleEdges(),
        treeWidth: staticTree.width,
        treeHeight: staticTree.height,
        activeNodeId: null,
        activeHouseIdx: null,
        currentReturn: finalResult,
        frames: [{ name: 'main()', args: `result=$${finalResult}` }]
      }
    });

    return steps;
  }

  /* ------------------------------------------------------------------ */
  /* APPROACH 2: TOP-DOWN DP (MEMOIZATION)                              */
  /* ------------------------------------------------------------------ */
  else if (approach === 'memoization') {
    const staticTree = generateStaticTree('memoization', items, capacity);
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

    const memoGrid = Array.from({ length: n + 1 }, () => new Array(W + 1).fill(-1));
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
        y1: staticTree.layoutMap[e.from].y + 10,
        x2: staticTree.layoutMap[e.to].x,
        y2: staticTree.layoutMap[e.to].y - 10
      }));
    }

    steps.push({
      code: 'm_alloc_memo',
      badge: `Allocating 2D memo table memo[${n + 1}][${W + 1}] initialized with -1`,
      badgeType: 'info',
      state: {
        items, W, n,
        totalCalls: 0,
        cacheHits: 0,
        memoGrid: memoGrid.map(r => [...r]),
        treeNodes: [],
        treeEdges: [],
        treeWidth: staticTree.width,
        treeHeight: staticTree.height,
        activeNodeId: null,
        activeHouseIdx: null,
        currentReturn: null,
        frames: [{ name: 'main()', args: `n=${n}, W=${W}` }]
      }
    });

    function solveMemo(i, w) {
      const myId = nextNodeId++;
      callCounter++;

      nodeStateMap[myId].state = 'active';
      stackFrames.push({ name: `knapMemo(i=${i}, w=${w})`, args: i > 0 ? `Item:${items[i-1].name}, ${w}kg` : 'Base' });

      steps.push({
        code: 'c_entry',
        badge: `Entering knapMemo(i=${i}, w=${w})`,
        badgeType: 'info',
        state: {
          items, W, n, curI: i, curW: w,
          curItem: i > 0 ? items[i - 1] : null,
          activeHouseIdx: i > 0 ? i - 1 : null,
          totalCalls: callCounter,
          cacheHits,
          memoGrid: memoGrid.map(r => [...r]),
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height,
          activeNodeId: myId,
          currentReturn: null,
          frames: [...stackFrames]
        }
      });

      // Base case
      if (i === 0 || w === 0) {
        steps.push({
          code: 'c_base',
          badge: `Base Case Reached (i=${i} || w=${w}) &rarr; Return 0`,
          badgeType: 'success',
          state: {
            items, W, n, curI: i, curW: w,
            curItem: i > 0 ? items[i - 1] : null,
            activeHouseIdx: i > 0 ? i - 1 : null,
            totalCalls: callCounter,
            cacheHits,
            memoGrid: memoGrid.map(r => [...r]),
            treeNodes: getVisibleNodes(),
            treeEdges: getVisibleEdges(),
            treeWidth: staticTree.width,
            treeHeight: staticTree.height,
            activeNodeId: myId,
            currentReturn: 0,
            frames: [...stackFrames]
          }
        });
        nodeStateMap[myId].state = 'solved';
        nodeStateMap[myId].retVal = 0;
        stackFrames.pop();
        return 0;
      }

      // Memo Lookup
      if (memoGrid[i][w] !== -1) {
        cacheHits++;
        const cachedVal = memoGrid[i][w];
        nodeStateMap[myId].state = 'cache-hit';

        steps.push({
          code: 'c_memo_check',
          badge: `CACHE HIT! memo[${i}][${w}] = $${cachedVal} already computed. Instant O(1) return!`,
          badgeType: 'success',
          state: {
            items, W, n, curI: i, curW: w,
            curItem: items[i - 1],
            activeHouseIdx: i - 1,
            isCacheHit: true,
            totalCalls: callCounter,
            cacheHits,
            memoGrid: memoGrid.map(r => [...r]),
            treeNodes: getVisibleNodes(),
            treeEdges: getVisibleEdges(),
            treeWidth: staticTree.width,
            treeHeight: staticTree.height,
            activeNodeId: myId,
            currentReturn: cachedVal,
            frames: [...stackFrames]
          }
        });

        nodeStateMap[myId].state = 'solved';
        nodeStateMap[myId].retVal = cachedVal;
        stackFrames.pop();
        return cachedVal;
      }

      steps.push({
        code: 'c_memo_check',
        badge: `Cache Miss: memo[${i}][${w}] == -1 &rarr; Must calculate subproblem`,
        badgeType: 'info',
        state: {
          items, W, n, curI: i, curW: w,
          curItem: items[i - 1],
          activeHouseIdx: i - 1,
          isCacheHit: false,
          totalCalls: callCounter,
          cacheHits,
          memoGrid: memoGrid.map(r => [...r]),
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height,
          activeNodeId: myId,
          currentReturn: null,
          frames: [...stackFrames]
        }
      });

      const curItem = items[i - 1];
      const isHeavy = curItem.weight > w;

      let calculated = 0;
      if (isHeavy) {
        steps.push({
          code: 'c_excl_calc',
          badge: `Item #${i} (${curItem.name}, ${curItem.weight}kg) > ${w}kg &rarr; Exclude and solve knapMemo(${i - 1}, ${w})`,
          badgeType: 'info',
          state: {
            items, W, n, curI: i, curW: w, curItem,
            activeHouseIdx: i - 1,
            totalCalls: callCounter,
            cacheHits,
            memoGrid: memoGrid.map(r => [...r]),
            treeNodes: getVisibleNodes(),
            treeEdges: getVisibleEdges(),
            treeWidth: staticTree.width,
            treeHeight: staticTree.height,
            activeNodeId: myId,
            currentReturn: null,
            frames: [...stackFrames]
          }
        });

        calculated = solveMemo(i - 1, w);
      } else {
        const exclVal = solveMemo(i - 1, w);
        const inclVal = curItem.val + solveMemo(i - 1, w - curItem.weight);
        calculated = Math.max(exclVal, inclVal);

        steps.push({
          code: 'c_memo_calc',
          badge: `Computed max for knapMemo(${i}, ${w}): max(Excl: $${exclVal}, Incl: $${inclVal}) = $${calculated}`,
          badgeType: 'info',
          state: {
            items, W, n, curI: i, curW: w, curItem,
            activeHouseIdx: i - 1,
            totalCalls: callCounter,
            cacheHits,
            memoGrid: memoGrid.map(r => [...r]),
            treeNodes: getVisibleNodes(),
            treeEdges: getVisibleEdges(),
            treeWidth: staticTree.width,
            treeHeight: staticTree.height,
            activeNodeId: myId,
            currentReturn: calculated,
            frames: [...stackFrames]
          }
        });
      }

      memoGrid[i][w] = calculated;

      steps.push({
        code: 'c_ret_memo',
        badge: `Storing into Cache: memo[${i}][${w}] = $${calculated}. Returning value.`,
        badgeType: 'success',
        state: {
          items, W, n, curI: i, curW: w, curItem,
          activeHouseIdx: i - 1,
          totalCalls: callCounter,
          cacheHits,
          memoGrid: memoGrid.map(r => [...r]),
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height,
          activeNodeId: myId,
          currentReturn: calculated,
          frames: [...stackFrames]
        }
      });

      nodeStateMap[myId].state = 'solved';
      nodeStateMap[myId].retVal = calculated;
      stackFrames.pop();
      return calculated;
    }

    const finalResult = solveMemo(n, W);

    steps.push({
      code: 'm_print',
      badge: `Memoization Complete! Optimal Value = $${finalResult} (Total Calls: ${callCounter}, Cache Hits: ${cacheHits})`,
      badgeType: 'success',
      state: {
        items, W, n,
        totalCalls: callCounter,
        cacheHits,
        memoGrid: memoGrid.map(r => [...r]),
        treeNodes: getVisibleNodes(),
        treeEdges: getVisibleEdges(),
        treeWidth: staticTree.width,
        treeHeight: staticTree.height,
        activeNodeId: null,
        activeHouseIdx: null,
        currentReturn: finalResult,
        frames: [{ name: 'main()', args: `result=$${finalResult}` }]
      }
    });

    return steps;
  }

  /* ------------------------------------------------------------------ */
  /* APPROACH 3: DP TABULATION (BOTTOM-UP 2D TABLE)                     */
  /* ------------------------------------------------------------------ */
  else if (approach === 'tabulation') {
    const dpGrid = Array.from({ length: n + 1 }, () => new Array(W + 1).fill(0));
    let opCounter = 0;

    // Step 0: Read Inputs
    steps.push({
      code: 'm_read_input',
      badge: `Input: ${n} items, Knapsack Capacity W = ${W}kg. Starting Bottom-Up Tabulation`,
      badgeType: 'info',
      state: {
        items, W, n,
        iterations: 0,
        totalOps: 0,
        i: 0, w: 0,
        activeHouseIdx: null,
        dpGrid: dpGrid.map(r => [...r]),
        tracebackCells: [],
        selectedItems: [],
        currentReturn: null,
        frames: [{ name: 'main()', args: `n=${n}, W=${W}` }]
      }
    });

    // Step 1: Alloc DP 2D Matrix
    steps.push({
      code: 'c_alloc_dp',
      badge: `Allocate 2D table dp[0...${n}][0...${W}]. Row 0 (0 items) & Col 0 (0kg) initialized to $0`,
      badgeType: 'info',
      state: {
        items, W, n,
        iterations: 0,
        totalOps: 1,
        i: 0, w: 0,
        activeHouseIdx: null,
        dpGrid: dpGrid.map(r => [...r]),
        tracebackCells: [],
        selectedItems: [],
        currentReturn: null,
        frames: [{ name: 'main()', args: `n=${n}` }, { name: 'knapTab()', args: `W=${W}` }]
      }
    });

    for (let i = 1; i <= n; i++) {
      const curItem = items[i - 1];

      steps.push({
        code: 'c_loop_i',
        badge: `Outer Loop: Processing Item #${i} &mdash; "${curItem.name}" (Weight: ${curItem.weight}kg, Value: $${curItem.val})`,
        badgeType: 'info',
        state: {
          items, W, n,
          iterations: Math.max(0, (i - 1) * W),
          totalOps: opCounter,
          i, w: 0,
          curItem,
          activeHouseIdx: i - 1,
          dpGrid: dpGrid.map(r => [...r]),
          tracebackCells: [],
          selectedItems: [],
          currentReturn: null,
          frames: [{ name: 'main()', args: `n=${n}` }, { name: 'knapTab()', args: `i=${i}` }]
        }
      });

      for (let w = 1; w <= W; w++) {
        opCounter++;
        const currentIter = (i - 1) * W + w;
        const canFit = curItem.weight <= w;

        steps.push({
          code: 'c_loop_w',
          badge: `Inner Loop (Iteration #${currentIter}/${n * W}): Evaluating capacity w = ${w}kg for Item #${i} (${curItem.name})`,
          badgeType: 'info',
          state: {
            items, W, n,
            iterations: currentIter,
            totalOps: opCounter,
            i, w,
            curItem,
            activeHouseIdx: i - 1,
            canFit,
            dpGrid: dpGrid.map(r => [...r]),
            tracebackCells: [],
            selectedItems: [],
            currentReturn: null,
            frames: [{ name: 'main()', args: `n=${n}` }, { name: 'knapTab()', args: `i=${i}, w=${w}` }]
          }
        });

        if (canFit) {
          const exclVal = dpGrid[i - 1][w];
          const remCap = w - curItem.weight;
          const inclVal = curItem.val + dpGrid[i - 1][remCap];
          const calculated = Math.max(exclVal, inclVal);
          dpGrid[i][w] = calculated;

          steps.push({
            code: 'c_dp_incl',
            badge: `dp[${i}][${w}] = max(Excl: dp[${i-1}][${w}]=$${exclVal}, Incl: val($${curItem.val}) + dp[${i-1}][${remCap}]($${dpGrid[i-1][remCap]}) = $${inclVal}) &rarr; $${calculated}`,
            badgeType: 'success',
            state: {
              items, W, n,
              iterations: currentIter,
              totalOps: opCounter,
              i, w,
              curItem,
              activeHouseIdx: i - 1,
              exclVal,
              inclVal,
              remCap,
              canFit: true,
              dpGrid: dpGrid.map(r => [...r]),
              tracebackCells: [],
              selectedItems: [],
              currentReturn: calculated,
              frames: [{ name: 'main()', args: `n=${n}` }, { name: 'knapTab()', args: `dp[${i}][${w}]=$${calculated}` }]
            }
          });
        } else {
          const calculated = dpGrid[i - 1][w];
          dpGrid[i][w] = calculated;

          steps.push({
            code: 'c_dp_excl',
            badge: `Cannot Fit: Item #${i} wt (${curItem.weight}kg) > capacity (${w}kg) &rarr; Copy top cell dp[${i-1}][${w}] = $${calculated}`,
            badgeType: 'info',
            state: {
              items, W, n,
              iterations: currentIter,
              totalOps: opCounter,
              i, w,
              curItem,
              activeHouseIdx: i - 1,
              canFit: false,
              dpGrid: dpGrid.map(r => [...r]),
              tracebackCells: [],
              selectedItems: [],
              currentReturn: calculated,
              frames: [{ name: 'main()', args: `n=${n}` }, { name: 'knapTab()', args: `dp[${i}][${w}]=$${calculated}` }]
            }
          });
        }
      }
    }

    const totalIterations = n * W;

    // Reconstruction (Backtracking) of Chosen Items
    let curI = n, curW = W;
    const pathCells = [];
    const chosenItems = [];
    while (curI > 0 && curW > 0) {
      pathCells.push({ i: curI, w: curW });
      if (dpGrid[curI][curW] !== dpGrid[curI - 1][curW]) {
        // Item was included!
        const taken = items[curI - 1];
        chosenItems.push(taken);
        curW -= taken.weight;
        curI--;
      } else {
        // Item was excluded
        curI--;
      }
    }
    pathCells.push({ i: curI, w: curW });
    chosenItems.reverse();
    const finalValue = dpGrid[n][W];
    const totalTakenWeight = chosenItems.reduce((acc, it) => acc + it.weight, 0);

    steps.push({
      code: 'c_ret_dp',
      badge: `Table Complete! Max Knapsack Value = $${finalValue}. Backtracking reveals ${chosenItems.length} selected item(s).`,
      badgeType: 'success',
      state: {
        items, W, n,
        iterations: totalIterations,
        totalOps: opCounter,
        i: n, w: W,
        activeHouseIdx: null,
        dpGrid: dpGrid.map(r => [...r]),
        tracebackCells: pathCells,
        selectedItems: chosenItems,
        totalTakenWeight,
        currentReturn: finalValue,
        frames: [{ name: 'main()', args: `n=${n}` }, { name: 'knapTab()', args: `maxVal=$${finalValue}` }]
      }
    });

    const chosenNames = chosenItems.map(it => `${it.name} ($${it.val}, ${it.weight}kg)`).join(', ') || 'None';
    steps.push({
      code: 'm_print',
      badge: `Tabulation Complete (${totalIterations} iterations)! Selected: [${chosenNames}] &rarr; Max Value: $${finalValue} (Total Weight: ${totalTakenWeight}kg / ${W}kg)`,
      badgeType: 'success',
      state: {
        items, W, n,
        iterations: totalIterations,
        totalOps: opCounter,
        i: null, w: null,
        activeHouseIdx: null,
        dpGrid: dpGrid.map(r => [...r]),
        tracebackCells: pathCells,
        selectedItems: chosenItems,
        totalTakenWeight,
        currentReturn: finalValue,
        frames: [{ name: 'main()', args: `maxVal=$${finalValue}` }]
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
const inputValuesStr = ref(PRESETS[0].items.map(it => it.val).join(', '));
const inputWeightsStr = ref(PRESETS[0].items.map(it => it.weight).join(', '));
const customInputStr = ref(String(PRESETS[0].capacity));
const lang = ref('java');
const rightTab = ref('code');

const currentItems = ref([...PRESETS[0].items]);
const currentCapacity = ref(PRESETS[0].capacity);

const steps = ref([]);
const currentStepIdx = ref(0);
const playing = ref(false);
const speed = ref(800);
let playTimer = null;

// Warning Modal
const showWarningModal = ref(false);
const warningModalTitle = ref('');
const warningModalMsg = ref('');
const requestedCount = ref(0);
const warningModalLimit = ref(0);

function closeWarningModal() {
  showWarningModal.value = false;
}

// Current State Computeds
const s = computed(() => {
  if (!steps.value.length) return {};
  return steps.value[Math.min(currentStepIdx.value, steps.value.length - 1)] || {};
});

const st = computed(() => s.value.state || {});
const codeLines = computed(() => CODES[currentApproach.value]?.[lang.value] || CODES[currentApproach.value]?.['java'] || []);
const pseudocodeLines = computed(() => PSEUDOCODES[currentApproach.value] || []);

/* ==================================================================== */
/* INITIALIZATION & STEP RUNNER                                         */
/* ==================================================================== */
function rebuildSteps() {
  stopPlay();
  steps.value = buildSteps(currentApproach.value, currentItems.value, currentCapacity.value);
  currentStepIdx.value = 0;
}

function applyApproach(appId) {
  currentApproach.value = appId;
  const maxItems = appId === 'tabulation' ? 6 : 4;
  const maxCap = appId === 'tabulation' ? 10 : 7;
  let adjusted = false;

  if (currentItems.value.length > maxItems) {
    currentItems.value = currentItems.value.slice(0, maxItems);
    inputValuesStr.value = currentItems.value.map(it => it.val).join(', ');
    inputWeightsStr.value = currentItems.value.map(it => it.weight).join(', ');
    adjusted = true;
  }
  if (currentCapacity.value > maxCap) {
    currentCapacity.value = maxCap;
    customInputStr.value = String(maxCap);
    adjusted = true;
  }

  if (adjusted) {
    const appLabel = APPROACHES.find(a => a.id === appId)?.label || appId;
    warningModalTitle.value = `${appLabel} Visual Limit (Max ${maxItems} Items, ${maxCap}kg)`;
    warningModalMsg.value = `Switched to ${appLabel}. Input size adjusted to ${maxItems} items and ${maxCap}kg capacity for optimal visual presentation.`;
    requestedCount.value = currentItems.value.length;
    warningModalLimit.value = maxItems;
    showWarningModal.value = true;
  }

  rebuildSteps();
}

function applyInput() {
  // 1. Parse Capacity
  let c = parseInt(customInputStr.value, 10);
  const maxCap = currentApproach.value === 'tabulation' ? 10 : 7;
  if (isNaN(c) || c < 1) c = 1;
  let capAdjusted = false;
  const originalCap = c;
  if (c > maxCap) {
    capAdjusted = true;
    c = maxCap;
  }
  customInputStr.value = String(c);
  currentCapacity.value = c;

  // 2. Parse Values and Weights
  const parseList = (str) => {
    return str
      .split(/[,\s]+/)
      .map(x => parseInt(x.trim(), 10))
      .filter(x => !isNaN(x) && x > 0);
  };

  let vals = parseList(inputValuesStr.value);
  let wts = parseList(inputWeightsStr.value);

  // Fallbacks if empty
  if (!vals.length) vals = [15, 50, 60, 90];
  if (!wts.length) wts = [1, 3, 4, 5];

  // Match length to the minimum of both arrays
  const itemLen = Math.min(vals.length, wts.length);
  vals = vals.slice(0, itemLen);
  wts = wts.slice(0, itemLen);

  const maxItemsLimit = currentApproach.value === 'tabulation' ? 6 : 4;
  let itemsAdjusted = false;
  const originalCount = itemLen;

  if (vals.length > maxItemsLimit) {
    itemsAdjusted = true;
    vals = vals.slice(0, maxItemsLimit);
    wts = wts.slice(0, maxItemsLimit);
  }

  // Construct item list
  const newItems = vals.map((v, idx) => {
    return {
      id: idx + 1,
      name: `Item ${idx + 1}`,
      weight: wts[idx],
      val: v
    };
  });

  currentItems.value = newItems;
  inputValuesStr.value = vals.join(', ');
  inputWeightsStr.value = wts.join(', ');

  // Show warning modal if either capacity or item count was adjusted
  if (itemsAdjusted || capAdjusted) {
    const approachName = APPROACHES.find(a => a.id === currentApproach.value)?.label || 'Algorithm';
    if (itemsAdjusted) {
      requestedCount.value = originalCount;
      warningModalLimit.value = maxItemsLimit;
      warningModalTitle.value = `${approachName} Item Limit (Max: ${maxItemsLimit} Items)`;
      warningModalMsg.value = `Entered ${originalCount} items exceed the optimal visual layout for ${approachName}. Processing capped to the first ${maxItemsLimit} items.`;
    } else {
      requestedCount.value = originalCap;
      warningModalLimit.value = maxCap;
      warningModalTitle.value = `Capacity Adjusted (Max: ${maxCap}kg)`;
      warningModalMsg.value = `Entered capacity of ${originalCap}kg exceeds recommended visual limits for ${approachName}. Adjusting to ${maxCap}kg for optimal layout.`;
    }
    showWarningModal.value = true;
  }

  rebuildSteps();
}

function stepBy(delta) {
  stopPlay();
  const next = currentStepIdx.value + delta;
  currentStepIdx.value = Math.max(0, Math.min(steps.value.length - 1, next));
}

function togglePlay() {
  if (playing.value) {
    stopPlay();
  } else {
    if (currentStepIdx.value >= steps.value.length - 1) {
      currentStepIdx.value = 0;
    }
    playing.value = true;
    runPlayLoop();
  }
}

function runPlayLoop() {
  if (!playing.value) return;
  playTimer = setTimeout(() => {
    if (currentStepIdx.value < steps.value.length - 1) {
      currentStepIdx.value++;
      runPlayLoop();
    } else {
      stopPlay();
    }
  }, speed.value);
}

function stopPlay() {
  playing.value = false;
  if (playTimer) {
    clearTimeout(playTimer);
    playTimer = null;
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
const leftWidth = ref(58);
const vizHeight = ref(210);
const tableHeight = ref(48);

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
  rebuildSteps();
});

onUnmounted(() => {
  document.removeEventListener('keydown', onKeydown);
  stopPlay();
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

            <!-- Input for Values -->
            <div class="ll-input-group">
              <label>Values:</label>
              <input
                type="text"
                v-model="inputValuesStr"
                class="ll-text-input ll-input-vals"
                placeholder="15, 50, 60, 90"
                title="Comma-separated item values ($)"
                @keyup.enter="applyInput"
              />
            </div>

            <!-- Input for Weights -->
            <div class="ll-input-group">
              <label>Weights:</label>
              <input
                type="text"
                v-model="inputWeightsStr"
                class="ll-text-input ll-input-wts"
                placeholder="1, 3, 4, 5"
                title="Comma-separated item weights (kg)"
                @keyup.enter="applyInput"
              />
            </div>

            <!-- Input for Capacity -->
            <div class="ll-input-group">
              <label>Capacity:</label>
              <input
                type="number"
                v-model.number="customInputStr"
                class="ll-text-input ll-input-cap"
                min="1"
                :max="currentApproach === 'tabulation' ? 10 : 7"
                placeholder="7"
                title="Knapsack maximum capacity (kg)"
                @keyup.enter="applyInput"
              />
              <span class="ll-input-hint">kg</span>
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
              <!-- Top Items Strip (Always Visible) -->
              <div class="ll-house-banner">
                <div class="ll-house-banner-title">Available Items &amp; Values (Knapsack Cap: {{ currentCapacity }}kg):</div>
                <div class="ll-house-track">
                  <div
                    v-for="(item, idx) in currentItems"
                    :key="item.id"
                    class="ll-house-card"
                    :class="{
                      'll-house-active': st.activeHouseIdx === idx || st.i === idx + 1,
                      'll-house-rob': st.selectedItems && st.selectedItems.some(it => it.id === item.id)
                    }"
                  >
                    <div class="ll-house-roof">&#9650;</div>
                    <div class="ll-house-body">
                      <span class="ll-house-icon">&#128188;</span>
                      <span class="ll-house-val">${{ item.val }}</span>
                    </div>
                    <div class="ll-house-idx">{{ item.name }} ({{ item.weight }}kg)</div>
                  </div>
                </div>
              </div>

              <!-- Visual Workspace Panel -->
              <div class="ll-viz-wrap" :style="{ height: vizHeight + 'px' }">
                <div class="ll-perm-area">
                  <!-- Metrics Ribbon Chips -->
                  <div class="ll-ptrs">
                    <div class="ll-ptr-chip">
                      Items: <b class="ll-c-blue">{{ currentItems.length }}</b>, Cap: <b class="ll-c-purple">{{ currentCapacity }}kg</b>
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
                      Max Value: <b class="ll-c-green">${{ st.currentReturn }}</b>
                    </div>
                    <div class="ll-ptr-chip" v-if="st.selectedItems && st.selectedItems.length">
                      Selected: <b class="ll-c-purple">{{ st.selectedItems.map(it => it.name).join(', ') }}</b>
                    </div>
                  </div>

                  <!-- Visual Diagram 1: DP Tabulation (Bottom-Up 2D Matrix Table) -->
                  <div v-if="currentApproach === 'tabulation'" class="ll-dp-tab-view">
                    <div class="ll-section-caption">
                      <span>2D DP Table &mdash; <code>dp[0...{{ currentItems.length }}][0...{{ currentCapacity }}]</code></span>
                      <span v-if="st.i >= 1 && st.w >= 1" class="ll-calc-pill">
                        {{ st.canFit ? `Fit: max(dp[${st.i-1}][${st.w}], $${st.curItem.val} + dp[${st.i-1}][${st.w - st.curItem.weight}])` : `Heavy: copy dp[${st.i-1}][${st.w}]` }}
                      </span>
                    </div>

                    <div class="ll-grid-scroll-wrap">
                      <table class="ll-dp-table">
                        <thead>
                          <tr>
                            <th class="ll-th-corner">Item \ Cap</th>
                            <th v-for="w in (currentCapacity + 1)" :key="w" class="ll-th-col">
                              {{ w - 1 }}kg
                            </th>
                          </tr>
                        </thead>
                        <tbody>
                          <tr
                            v-for="(row, rIdx) in st.dpGrid"
                            :key="rIdx"
                            :class="{ 'll-row-active': st.i === rIdx }"
                          >
                            <td class="ll-td-rowhead">
                              <span v-if="rIdx === 0">0: (Empty)</span>
                              <span v-else class="ll-rowhead-item">
                                <b>{{ rIdx }}:</b> {{ currentItems[rIdx - 1]?.name }} (${{ currentItems[rIdx - 1]?.val }}, {{ currentItems[rIdx - 1]?.weight }}kg)
                              </span>
                            </td>
                            <td
                              v-for="(cellVal, cIdx) in row"
                              :key="cIdx"
                              class="ll-dp-cell"
                              :class="{
                                'll-cell-active': st.i === rIdx && st.w === cIdx,
                                'll-cell-top': st.i === rIdx && rIdx > 0 && rIdx === st.i - 1 && cIdx === st.w,
                                'll-cell-diag': st.i === rIdx && st.canFit && rIdx === st.i - 1 && cIdx === (st.w - st.curItem.weight),
                                'll-cell-trace': st.tracebackCells && st.tracebackCells.some(tc => tc.i === rIdx && tc.w === cIdx),
                                'll-cell-base': rIdx === 0 || cIdx === 0,
                                'll-cell-filled': cellVal !== '?' && cellVal !== -1
                              }"
                            >
                              <span class="ll-cell-val">${{ cellVal }}</span>
                            </td>
                          </tr>
                        </tbody>
                      </table>
                    </div>
                  </div>

                  <!-- Visual Diagram 2: Recursion & Memoization Decision Tree -->
                  <div v-else class="ll-tree-container">
                    <div class="ll-section-caption">
                      <span>{{ currentApproach === 'recursion' ? 'Include vs Exclude Decision Tree (Exponential Explosion O(2^N))' : 'Pruned Memoization Decision Tree (Linear O(N×W))' }}</span>
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
                              <rect x="-26" y="-6" width="52" height="12" rx="3" fill="#ef4444" stroke="#dc2626" stroke-width="0.5" />
                              <text x="0" y="0" text-anchor="middle" class="ll-badge-text-red">Recomputing!</text>
                            </g>
                            <!-- Cache Hit Badge Above Node -->
                            <g v-else-if="node.state === 'cache-hit'" transform="translate(0, -17)">
                              <rect x="-26" y="-6" width="52" height="12" rx="3" fill="#9333ea" stroke="#7e22ce" stroke-width="0.5" />
                              <text x="0" y="0" text-anchor="middle" class="ll-badge-text-purple">Cache Hit O(1)</text>
                            </g>

                            <!-- Node Outer Card -->
                            <rect
                              x="-26"
                              y="-12"
                              width="52"
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
                            <!-- Node Text: knap(i, w) -->
                            <text x="0" y="-3" text-anchor="middle" class="ll-node-text-call">
                              {{ node.label }}
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
                      <div v-if="currentApproach === 'memoization' && st.memoGrid" class="ll-memo-strip-wrap">
                        <div class="ll-memo-title">Memoization Cache Table &mdash; <code>memo[0...{{ currentItems.length }}][0...{{ currentCapacity }}]</code>:</div>
                        <div class="ll-memo-strip">
                          <template v-for="(row, rI) in st.memoGrid" :key="rI">
                            <template v-for="(mVal, cI) in row" :key="cI">
                              <div class="ll-memo-cell-wrap" v-if="rI > 0 && cI > 0">
                                <div
                                  class="ll-memo-cell"
                                  :class="{
                                    'll-memo-hit': mVal !== -1,
                                    'll-memo-empty': mVal === -1
                                  }"
                                >
                                  {{ mVal !== -1 ? `$${mVal}` : '?' }}
                                </div>
                                <span class="ll-memo-idx">[{{ rI }},{{ cI }}]</span>
                              </div>
                            </template>
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
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-base"></span>Base Cases (Row 0 / Col 0)</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-cur"></span>Active Cell dp[i][w]</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-purple"></span>Exclude (Top Cell)</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-new"></span>Include (Offset Cell)</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-purple"></span>Chosen Backtrack</span>
                </template>
                <template v-else-if="currentApproach === 'memoization'">
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-cur"></span>Active Subproblem</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-new"></span>Solved &amp; Cached</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-purple"></span>Cache Hit (O(1) Return)</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-uncalc"></span>Uncalled</span>
                </template>
                <template v-else>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-cur"></span>Active Frame</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-new"></span>Solved Optimal Value</span>
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
                  v-html="s.badge || 'Initializing knapsack state...'"
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

                <!-- Complexity & Analysis Tab -->
                <div v-else class="ll-info-scroll">
                  <h3>Why Dynamic Programming? (0/1 Knapsack Insight)</h3>
                  <p>
                    The <strong>0/1 Knapsack Problem</strong> is a foundational dynamic programming challenge exhibiting optimal substructure and overlapping subproblems:
                  </p>
                  <ul>
                    <li>
                      <strong>1. Optimal Substructure:</strong> At each item <code>i</code>, the optimal decision breaks down into:
                      <br />&bull; <strong>Option 1 (Exclude Item i):</strong> Max value is <code>dp[i - 1][w]</code>
                      <br />&bull; <strong>Option 2 (Include Item i):</strong> Max value is <code>val[i - 1] + dp[i - 1][w - wt[i - 1]]</code> (if <code>wt[i - 1] &le; w</code>)
                      <br /><code>dp[i][w] = max(dp[i - 1][w], val[i - 1] + dp[i - 1][w - wt[i - 1]])</code>
                    </li>
                    <li>
                      <strong>2. Overlapping Subproblems:</strong> Naive recursion branches exponentially into identical capacity-item subproblems (e.g. <code>knap(2, 4)</code> is evaluated repeatedly across different exclusion/inclusion combinations).
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
                        <td><strong>Brute Force Recursion</strong></td>
                        <td>O(2<sup>N</sup>) Exponential</td>
                        <td>O(N) Stack</td>
                        <td><span class="ll-c-red" style="font-weight:700">YES</span> (Exponential Duplication)</td>
                      </tr>
                      <tr>
                        <td><strong>DP Memoization</strong></td>
                        <td>O(N &times; W) Pseudo-polynomial</td>
                        <td>O(N &times; W) Cache + Stack</td>
                        <td><span class="ll-c-green" style="font-weight:700">NO</span> (O(1) Lookup on Hit)</td>
                      </tr>
                      <tr>
                        <td><strong>DP Tabulation</strong></td>
                        <td>O(N &times; W) Pseudo-polynomial</td>
                        <td>O(N &times; W) 2D Table</td>
                        <td><span class="ll-c-green" style="font-weight:700">NO</span> (Iterative 2D Grid Pass)</td>
                      </tr>
                      <tr>
                        <td><strong>Space-Optimized DP</strong></td>
                        <td>O(N &times; W) Pseudo-polynomial</td>
                        <td>O(W) 1D Array</td>
                        <td><span class="ll-c-green" style="font-weight:700">NO</span> (Traverse weights backwards)</td>
                      </tr>
                    </tbody>
                  </table>

                  <h3>Recurrence Formula Proof</h3>
                  <p>
                    For each item <code>i</code> and capacity <code>w</code>:
                  </p>
                  <p class="ll-math-box">
                    dp[i][w] = wt[i-1] &le; w ? max(dp[i-1][w], val[i-1] + dp[i-1][w - wt[i-1]]) : dp[i-1][w]
                  </p>
                  <p>
                    For <code>N = 40</code> items, Brute Force recursion requires over <strong>1 Trillion</strong> recursive calls, while DP Tabulation computes the exact maximum value in just <strong>O(N &times; W) iterations</strong>!
                  </p>

                  <div class="ll-note">
                    <strong>Takeaway:</strong> Dynamic Programming avoids exploring all $2^N$ subsets by systematically memorizing optimal sub-capacities in a bottom-up 2D grid.
                  </div>
                </div>
              </div>
            </div>
          </div>

          <!-- Bottom Footer -->
          <div class="ll-footer">
            Step {{ currentStepIdx + 1 }} / {{ steps.length }}
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
                    <span class="ll-modal-tag-entered">Entered: {{ requestedCount }}kg</span>
                    <span class="ll-modal-arrow">&rarr;</span>
                    <span class="ll-modal-tag-applied">Adjusted to Max: {{ warningModalLimit }}kg</span>
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

<style>
/* Unscoped Global Scrollbar Remover for Slidev Slides */
.slide-wrapper *::-webkit-scrollbar,
.slide-wrapper::-webkit-scrollbar,
.row-main::-webkit-scrollbar,
.row-main *::-webkit-scrollbar,
.ll-root::-webkit-scrollbar,
.ll-root *::-webkit-scrollbar,
.ll-grid-scroll-wrap::-webkit-scrollbar,
.ll-code-scroll::-webkit-scrollbar,
.ll-info-scroll::-webkit-scrollbar,
.ll-table-area::-webkit-scrollbar,
.ll-viz-wrap::-webkit-scrollbar,
.ll-tree-scroll-area::-webkit-scrollbar,
.ll-memo-strip::-webkit-scrollbar {
  display: none !important;
  width: 0 !important;
  height: 0 !important;
  background: transparent !important;
  -webkit-appearance: none !important;
}

.slide-wrapper,
.slide-wrapper *,
.row-main,
.row-main *,
.ll-root,
.ll-root * {
  scrollbar-width: none !important;
  -ms-overflow-style: none !important;
}
</style>

<style scoped>
.ll-root * { box-sizing: border-box; }
.ll-root *, .ll-root, .row-main {
  scrollbar-width: none !important;
  -ms-overflow-style: none !important;
}
.ll-root *::-webkit-scrollbar,
.ll-root::-webkit-scrollbar,
.row-main::-webkit-scrollbar,
.ll-grid-scroll-wrap::-webkit-scrollbar,
.ll-tree-scroll-area::-webkit-scrollbar,
.ll-code-scroll::-webkit-scrollbar,
.ll-info-scroll::-webkit-scrollbar,
.ll-house-track::-webkit-scrollbar,
.ll-table-area::-webkit-scrollbar {
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
  display: flex; flex-direction: column; overflow: hidden; width: 100%; height: 58vh;
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
.ll-text-input { background: var(--surface); border: 1px solid var(--border2); color: var(--text); border-radius: var(--radius-sm); padding: 3px 6px; font-size: 11.5px; font-family: monospace; }
.ll-text-input:focus { outline: none; border-color: var(--coral); box-shadow: 0 0 0 3px rgba(240,77,77,.1); }
.ll-input-vals { width: 105px; }
.ll-input-wts { width: 85px; }
.ll-input-cap { width: 44px; }
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

/* Top Items / Houses Banner */
.ll-house-banner { padding: 4px 10px; background: var(--surface2); border-bottom: 1px solid var(--border); flex-shrink: 0; }
.ll-house-banner-title { font-size: 10px; font-weight: 700; color: var(--text2); margin-bottom: 3px; }
.ll-house-track { display: flex; gap: 8px; overflow-x: auto; padding: 2px 0; }
.ll-house-card { display: flex; flex-direction: column; align-items: center; background: var(--surface); border: 1.5px solid var(--border2); border-radius: 6px; padding: 3px 6px; min-width: 58px; transition: all .2s ease; box-shadow: var(--shadow-sm); }
.ll-house-roof { font-size: 10px; color: #ef5050; line-height: 1; margin-bottom: -2px; }
.ll-house-body { display: flex; align-items: center; gap: 3px; }
.ll-house-icon { font-size: 13px; }
.ll-house-val { font-family: monospace; font-size: 11px; font-weight: 800; color: #047857; }
.ll-house-idx { font-size: 9px; color: var(--muted); margin-top: 2px; white-space: nowrap; }
.ll-house-active { border-color: #f59e0b !important; background: #fffbeb !important; transform: translateY(-2px); box-shadow: 0 0 0 2.5px rgba(245, 158, 11, 0.3) !important; }
.ll-house-rob { border-color: #9333ea !important; background: #f3e8ff !important; box-shadow: 0 0 0 2px rgba(147, 51, 234, 0.25) !important; }

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

/* Diagram 1: 2D DP Table */
.ll-dp-tab-view { display: flex; flex-direction: column; padding: 4px 8px; flex: 1; min-height: 0; }
.ll-grid-scroll-wrap { flex: 1; overflow: auto; border: 1px solid var(--border); border-radius: 4px; background: #fafafa; }
.ll-dp-table { width: 100%; border-collapse: collapse; font-family: monospace; font-size: 11px; }
.ll-th-corner { background: var(--surface2); padding: 3px 6px; font-size: 10px; color: var(--text2); border: 1px solid var(--border); position: sticky; top: 0; left: 0; z-index: 4; }
.ll-th-col { background: var(--surface2); padding: 3px 6px; font-size: 10px; color: var(--blue); border: 1px solid var(--border); text-align: center; position: sticky; top: 0; z-index: 3; }
.ll-td-rowhead { background: var(--surface2); padding: 3px 6px; font-size: 10px; color: var(--text2); border: 1px solid var(--border); position: sticky; left: 0; z-index: 2; white-space: nowrap; }
.ll-rowhead-item { font-size: 10px; }
.ll-rowhead-item b { color: var(--orange); }

.ll-dp-cell { padding: 3px 6px; border: 1px solid var(--border); text-align: center; transition: all .15s; }
.ll-cell-val { font-weight: 600; color: var(--text2); }
.ll-cell-base { background: #f8fafc; }
.ll-cell-filled .ll-cell-val { color: var(--text); }
.ll-cell-active { background: #fed7aa !important; transform: scale(1.04); box-shadow: 0 0 6px rgba(249,115,22,.4); }
.ll-cell-active .ll-cell-val { color: #9a3412 !important; font-weight: 700; }
.ll-cell-top { background: #dbeafe !important; }
.ll-cell-top .ll-cell-val { color: #1e40af !important; }
.ll-cell-diag { background: #dcfce7 !important; }
.ll-cell-diag .ll-cell-val { color: #166534 !important; }
.ll-cell-trace { background: #f3e8ff !important; border: 1.5px solid var(--purple) !important; }
.ll-cell-trace .ll-cell-val { color: var(--purple) !important; font-weight: 700; }

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
  font-size: 9px !important;
  font-weight: 700 !important;
  fill: #1e293b !important;
  dominant-baseline: central !important;
}
.ll-node-text-val {
  font-family: 'Consolas', 'Fira Code', monospace !important;
  font-size: 8.5px !important;
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
