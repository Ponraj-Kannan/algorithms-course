<script setup>
import { ref, reactive, computed, onMounted, onBeforeUnmount, watch } from 'vue';

const props = defineProps({
  topic: { type: String, default: 'Dynamic Programming' },
  subTopic: { type: String, default: 'Minimum Number of Jumps to Reach End' }
});

/* ==================================================================== */
/* APPROACHES & MULTI-LANGUAGE 100% EXECUTABLE CODE SPECIFICATIONS      */
/* ==================================================================== */
const APPROACHES = [
  { id: 'recursion',   label: 'Brute Force',  desc: 'Exponential — Explores All Jump Paths Recursively' },
  { id: 'memoization', label: 'Memoization',  desc: 'O(n²) Time & O(n) Space — Top-Down DP with Cache' },
  { id: 'tabulation',  label: 'Tabulation',   desc: 'O(n²) Time & O(n) Space — Bottom-Up 1D DP Table' }
];

const CODES = {
  recursion: {
    java: [
      ['',             'import java.util.Scanner;'],
      ['',             ''],
      ['',             'public class Main {'],
      ['',             '    static final int INF = Integer.MAX_VALUE / 2;'],
      ['',             ''],
      ['c_entry',      '    static int minJumps(int[] arr, int n, int pos) {'],
      ['c_base_end',   '        if (pos >= n - 1) return 0;'],
      ['c_base_zero',  '        if (arr[pos] == 0) return INF;'],
      ['c_init_min',   '        int minCost = INF;'],
      ['c_loop',       '        for (int step = 1; step <= arr[pos]; step++) {'],
      ['c_rec_call',   '            int sub = minJumps(arr, n, pos + step);'],
      ['c_update_min', '            if (sub != INF) minCost = Math.min(minCost, 1 + sub);'],
      ['',             '        }'],
      ['c_ret_min',    '        return minCost;'],
      ['',             '    }'],
      ['',             ''],
      ['',             '    public static void main(String[] args) {'],
      ['',             '        Scanner sc = new Scanner(System.in);'],
      ['c_input_n',    '        int n = sc.nextInt();'],
      ['',             '        int[] arr = new int[n];'],
      ['c_input_arr',  '        for (int i = 0; i < n; i++) arr[i] = sc.nextInt();'],
      ['c_main_call',  '        int result = minJumps(arr, n, 0);'],
      ['c_main_ret',   '        System.out.println(result == INF ? -1 : result);'],
      ['',             '    }'],
      ['',             '}']
    ],
    c: [
      ['',             '#include <stdio.h>'],
      ['',             '#define INF 1000000'],
      ['',             ''],
      ['c_entry',      'int minJumps(int* arr, int n, int pos) {'],
      ['c_base_end',   '    if (pos >= n - 1) return 0;'],
      ['c_base_zero',  '    if (arr[pos] == 0) return INF;'],
      ['c_init_min',   '    int minCost = INF;'],
      ['c_loop',       '    for (int step = 1; step <= arr[pos]; step++) {'],
      ['c_rec_call',   '        int sub = minJumps(arr, n, pos + step);'],
      ['c_update_min', '        if (sub < INF) { int t = 1+sub; if (t < minCost) minCost = t; }'],
      ['',             '    }'],
      ['c_ret_min',    '    return minCost;'],
      ['',             '}'],
      ['',             ''],
      ['',             'int main() {'],
      ['c_input_n',    '    int n; scanf("%d", &n);'],
      ['',             '    int arr[n];'],
      ['c_input_arr',  '    for (int i = 0; i < n; i++) scanf("%d", &arr[i]);'],
      ['c_main_call',  '    int result = minJumps(arr, n, 0);'],
      ['c_main_ret',   '    printf("%d\\n", result >= INF ? -1 : result);'],
      ['',             '    return 0;'],
      ['',             '}']
    ],
    cpp: [
      ['',             '#include <iostream>'],
      ['',             '#include <climits>'],
      ['',             'using namespace std;'],
      ['',             ''],
      ['c_entry',      'int minJumps(int arr[], int n, int pos) {'],
      ['c_base_end',   '    if (pos >= n - 1) return 0;'],
      ['c_base_zero',  '    if (arr[pos] == 0) return INT_MAX / 2;'],
      ['c_init_min',   '    int minCost = INT_MAX / 2;'],
      ['c_loop',       '    for (int step = 1; step <= arr[pos]; step++) {'],
      ['c_rec_call',   '        int sub = minJumps(arr, n, pos + step);'],
      ['c_update_min', '        if (sub != INT_MAX/2) minCost = min(minCost, 1 + sub);'],
      ['',             '    }'],
      ['c_ret_min',    '    return minCost;'],
      ['',             '}'],
      ['',             ''],
      ['',             'int main() {'],
      ['c_input_n',    '    int n; cin >> n;'],
      ['',             '    int arr[n];'],
      ['c_input_arr',  '    for (int i = 0; i < n; i++) cin >> arr[i];'],
      ['c_main_call',  '    int result = minJumps(arr, n, 0);'],
      ['c_main_ret',   '    cout << (result >= INT_MAX/2 ? -1 : result) << endl;'],
      ['',             '    return 0;'],
      ['',             '}']
    ],
    python: [
      ['c_entry',      'def min_jumps(arr, n, pos):'],
      ['c_base_end',   '    if pos >= n - 1: return 0'],
      ['c_base_zero',  '    if arr[pos] == 0: return float("inf")'],
      ['c_init_min',   '    min_cost = float("inf")'],
      ['c_loop',       '    for step in range(1, arr[pos] + 1):'],
      ['c_rec_call',   '        sub = min_jumps(arr, n, pos + step)'],
      ['c_update_min', '        if sub != float("inf"): min_cost = min(min_cost, 1 + sub)'],
      ['c_ret_min',    '    return min_cost'],
      ['',             ''],
      ['',             'if __name__ == "__main__":'],
      ['c_input_n',    '    n = int(input())'],
      ['c_input_arr',  '    arr = list(map(int, input().split()))'],
      ['c_main_call',  '    result = min_jumps(arr, n, 0)'],
      ['c_main_ret',   '    print(-1 if result == float("inf") else result)']
    ],
    javascript: [
      ['c_entry',      'function minJumps(arr, n, pos) {'],
      ['c_base_end',   '  if (pos >= n - 1) return 0;'],
      ['c_base_zero',  '  if (arr[pos] === 0) return Infinity;'],
      ['c_init_min',   '  let minCost = Infinity;'],
      ['c_loop',       '  for (let step = 1; step <= arr[pos]; step++) {'],
      ['c_rec_call',   '    const sub = minJumps(arr, n, pos + step);'],
      ['c_update_min', '    if (sub !== Infinity) minCost = Math.min(minCost, 1 + sub);'],
      ['',             '  }'],
      ['c_ret_min',    '  return minCost;'],
      ['',             '}'],
      ['',             ''],
      ['c_input_n',    'const lines = require("fs").readFileSync("/dev/stdin","utf8").trim().split("\\n");'],
      ['c_input_arr',  'const arr = lines[1].split(" ").map(Number);'],
      ['c_main_call',  'const result = minJumps(arr, arr.length, 0);'],
      ['c_main_ret',   'console.log(result === Infinity ? -1 : result);']
    ]
  },
  memoization: {
    java: [
      ['',             'import java.util.Scanner;'],
      ['',             'import java.util.Arrays;'],
      ['',             ''],
      ['',             'public class Main {'],
      ['',             '    static final int INF = Integer.MAX_VALUE / 2;'],
      ['',             ''],
      ['c_entry',      '    static int minJumps(int[] arr, int n, int pos, int[] memo) {'],
      ['c_base_end',   '        if (pos >= n - 1) return 0;'],
      ['c_base_zero',  '        if (arr[pos] == 0) return INF;'],
      ['c_memo_check', '        if (memo[pos] != -1) return memo[pos]; // Cache Hit'],
      ['c_init_min',   '        int minCost = INF;'],
      ['c_loop',       '        for (int step = 1; step <= arr[pos]; step++) {'],
      ['c_rec_call',   '            int sub = minJumps(arr, n, pos + step, memo);'],
      ['c_update_min', '            if (sub != INF) minCost = Math.min(minCost, 1 + sub);'],
      ['',             '        }'],
      ['c_memo_store', '        memo[pos] = minCost;'],
      ['c_ret_min',    '        return memo[pos];'],
      ['',             '    }'],
      ['',             ''],
      ['',             '    public static void main(String[] args) {'],
      ['',             '        Scanner sc = new Scanner(System.in);'],
      ['c_input_n',    '        int n = sc.nextInt();'],
      ['',             '        int[] arr = new int[n];'],
      ['c_input_arr',  '        for (int i = 0; i < n; i++) arr[i] = sc.nextInt();'],
      ['c_memo_alloc', '        int[] memo = new int[n];'],
      ['c_memo_fill',  '        Arrays.fill(memo, -1);'],
      ['c_main_call',  '        int result = minJumps(arr, n, 0, memo);'],
      ['c_main_ret',   '        System.out.println(result == INF ? -1 : result);'],
      ['',             '    }'],
      ['',             '}']
    ],
    c: [
      ['',             '#include <stdio.h>'],
      ['',             '#include <string.h>'],
      ['',             '#define INF 1000000'],
      ['',             ''],
      ['c_entry',      'int minJumps(int* arr, int n, int pos, int* memo) {'],
      ['c_base_end',   '    if (pos >= n - 1) return 0;'],
      ['c_base_zero',  '    if (arr[pos] == 0) return INF;'],
      ['c_memo_check', '    if (memo[pos] != -1) return memo[pos];'],
      ['c_init_min',   '    int minCost = INF;'],
      ['c_loop',       '    for (int step = 1; step <= arr[pos]; step++) {'],
      ['c_rec_call',   '        int sub = minJumps(arr, n, pos + step, memo);'],
      ['c_update_min', '        if (sub < INF && 1+sub < minCost) minCost = 1+sub;'],
      ['',             '    }'],
      ['c_memo_store', '    memo[pos] = minCost;'],
      ['c_ret_min',    '    return memo[pos];'],
      ['',             '}'],
      ['',             ''],
      ['',             'int main() {'],
      ['c_input_n',    '    int n; scanf("%d", &n);'],
      ['',             '    int arr[n];'],
      ['c_input_arr',  '    for (int i = 0; i < n; i++) scanf("%d", &arr[i]);'],
      ['c_memo_alloc', '    int memo[n];'],
      ['c_memo_fill',  '    memset(memo, -1, sizeof(memo));'],
      ['c_main_call',  '    int result = minJumps(arr, n, 0, memo);'],
      ['c_main_ret',   '    printf("%d\\n", result >= INF ? -1 : result);'],
      ['',             '    return 0;'],
      ['',             '}']
    ],
    cpp: [
      ['',             '#include <iostream>'],
      ['',             '#include <vector>'],
      ['',             '#include <climits>'],
      ['',             'using namespace std;'],
      ['',             ''],
      ['c_entry',      'int minJumps(int arr[], int n, int pos, vector<int>& memo) {'],
      ['c_base_end',   '    if (pos >= n - 1) return 0;'],
      ['c_base_zero',  '    if (arr[pos] == 0) return INT_MAX / 2;'],
      ['c_memo_check', '    if (memo[pos] != -1) return memo[pos];'],
      ['c_init_min',   '    int minCost = INT_MAX / 2;'],
      ['c_loop',       '    for (int step = 1; step <= arr[pos]; step++) {'],
      ['c_rec_call',   '        int sub = minJumps(arr, n, pos + step, memo);'],
      ['c_update_min', '        if (sub != INT_MAX/2) minCost = min(minCost, 1 + sub);'],
      ['',             '    }'],
      ['c_memo_store', '    memo[pos] = minCost;'],
      ['c_ret_min',    '    return memo[pos];'],
      ['',             '}'],
      ['',             ''],
      ['',             'int main() {'],
      ['c_input_n',    '    int n; cin >> n;'],
      ['',             '    int arr[n];'],
      ['c_input_arr',  '    for (int i = 0; i < n; i++) cin >> arr[i];'],
      ['c_memo_alloc', '    vector<int> memo(n, -1);'],
      ['c_main_call',  '    int result = minJumps(arr, n, 0, memo);'],
      ['c_main_ret',   '    cout << (result >= INT_MAX/2 ? -1 : result) << endl;'],
      ['',             '    return 0;'],
      ['',             '}']
    ],
    python: [
      ['c_entry',      'def min_jumps(arr, n, pos, memo):'],
      ['c_base_end',   '    if pos >= n - 1: return 0'],
      ['c_base_zero',  '    if arr[pos] == 0: return float("inf")'],
      ['c_memo_check', '    if memo[pos] != -1: return memo[pos]  # Cache Hit'],
      ['c_init_min',   '    min_cost = float("inf")'],
      ['c_loop',       '    for step in range(1, arr[pos] + 1):'],
      ['c_rec_call',   '        sub = min_jumps(arr, n, pos + step, memo)'],
      ['c_update_min', '        if sub != float("inf"): min_cost = min(min_cost, 1 + sub)'],
      ['c_memo_store', '    memo[pos] = min_cost'],
      ['c_ret_min',    '    return memo[pos]'],
      ['',             ''],
      ['',             'if __name__ == "__main__":'],
      ['c_input_n',    '    n = int(input())'],
      ['c_input_arr',  '    arr = list(map(int, input().split()))'],
      ['c_memo_alloc', '    memo = [-1] * n'],
      ['c_main_call',  '    result = min_jumps(arr, n, 0, memo)'],
      ['c_main_ret',   '    print(-1 if result == float("inf") else result)']
    ],
    javascript: [
      ['c_entry',      'function minJumps(arr, n, pos, memo) {'],
      ['c_base_end',   '  if (pos >= n - 1) return 0;'],
      ['c_base_zero',  '  if (arr[pos] === 0) return Infinity;'],
      ['c_memo_check', '  if (memo[pos] !== -1) return memo[pos]; // Cache Hit'],
      ['c_init_min',   '  let minCost = Infinity;'],
      ['c_loop',       '  for (let step = 1; step <= arr[pos]; step++) {'],
      ['c_rec_call',   '    const sub = minJumps(arr, n, pos + step, memo);'],
      ['c_update_min', '    if (sub !== Infinity) minCost = Math.min(minCost, 1 + sub);'],
      ['',             '  }'],
      ['c_memo_store', '  memo[pos] = minCost;'],
      ['c_ret_min',    '  return memo[pos];'],
      ['',             '}'],
      ['',             ''],
      ['c_input_n',    'const lines = require("fs").readFileSync("/dev/stdin","utf8").trim().split("\\n");'],
      ['c_input_arr',  'const arr = lines[1].split(" ").map(Number);'],
      ['c_memo_alloc', 'const memo = new Array(arr.length).fill(-1);'],
      ['c_main_call',  'const result = minJumps(arr, arr.length, 0, memo);'],
      ['c_main_ret',   'console.log(result === Infinity ? -1 : result);']
    ]
  },
  tabulation: {
    java: [
      ['',             'import java.util.Scanner;'],
      ['',             'import java.util.Arrays;'],
      ['',             ''],
      ['',             'public class Main {'],
      ['c_entry',      '    static int minJumps(int[] arr, int n) {'],
      ['c_base_check', '        if (n == 1) return 0;'],
      ['c_dp_alloc',   '        int[] dp = new int[n];'],
      ['c_dp_fill',    '        Arrays.fill(dp, Integer.MAX_VALUE);'],
      ['c_dp_base',    '        dp[0] = 0;'],
      ['c_outer_loop', '        for (int i = 1; i < n; i++) {'],
      ['c_inner_loop', '            for (int j = 0; j < i; j++) {'],
      ['c_reachable',  '                if (dp[j] != Integer.MAX_VALUE && j + arr[j] >= i) {'],
      ['c_dp_update',  '                    dp[i] = Math.min(dp[i], dp[j] + 1);'],
      ['',             '                }'],
      ['',             '            }'],
      ['',             '        }'],
      ['c_dp_ret',     '        return dp[n-1] == Integer.MAX_VALUE ? -1 : dp[n-1];'],
      ['',             '    }'],
      ['',             ''],
      ['',             '    public static void main(String[] args) {'],
      ['',             '        Scanner sc = new Scanner(System.in);'],
      ['c_input_n',    '        int n = sc.nextInt();'],
      ['',             '        int[] arr = new int[n];'],
      ['c_input_arr',  '        for (int i = 0; i < n; i++) arr[i] = sc.nextInt();'],
      ['c_main_call',  '        int result = minJumps(arr, n);'],
      ['c_main_ret',   '        System.out.println(result);'],
      ['',             '    }'],
      ['',             '}']
    ],
    c: [
      ['',             '#include <stdio.h>'],
      ['',             '#include <limits.h>'],
      ['',             ''],
      ['c_entry',      'int minJumps(int* arr, int n) {'],
      ['c_base_check', '    if (n == 1) return 0;'],
      ['c_dp_alloc',   '    int dp[n];'],
      ['c_dp_fill',    '    for (int k=0;k<n;k++) dp[k]=INT_MAX;'],
      ['c_dp_base',    '    dp[0] = 0;'],
      ['c_outer_loop', '    for (int i = 1; i < n; i++) {'],
      ['c_inner_loop', '        for (int j = 0; j < i; j++) {'],
      ['c_reachable',  '            if (dp[j] != INT_MAX && j + arr[j] >= i) {'],
      ['c_dp_update',  '                if (dp[j]+1 < dp[i]) dp[i] = dp[j]+1;'],
      ['',             '            }'],
      ['',             '        }'],
      ['',             '    }'],
      ['c_dp_ret',     '    return dp[n-1] == INT_MAX ? -1 : dp[n-1];'],
      ['',             '}'],
      ['',             ''],
      ['',             'int main() {'],
      ['c_input_n',    '    int n; scanf("%d", &n);'],
      ['',             '    int arr[n];'],
      ['c_input_arr',  '    for (int i=0;i<n;i++) scanf("%d",&arr[i]);'],
      ['c_main_call',  '    int result = minJumps(arr, n);'],
      ['c_main_ret',   '    printf("%d\\n", result);'],
      ['',             '    return 0;'],
      ['',             '}']
    ],
    cpp: [
      ['',             '#include <iostream>'],
      ['',             '#include <vector>'],
      ['',             '#include <climits>'],
      ['',             'using namespace std;'],
      ['',             ''],
      ['c_entry',      'int minJumps(vector<int>& arr, int n) {'],
      ['c_base_check', '    if (n == 1) return 0;'],
      ['c_dp_alloc',   '    vector<int> dp(n, INT_MAX);'],
      ['c_dp_base',    '    dp[0] = 0;'],
      ['c_outer_loop', '    for (int i = 1; i < n; i++) {'],
      ['c_inner_loop', '        for (int j = 0; j < i; j++) {'],
      ['c_reachable',  '            if (dp[j] != INT_MAX && j + arr[j] >= i) {'],
      ['c_dp_update',  '                dp[i] = min(dp[i], dp[j] + 1);'],
      ['',             '            }'],
      ['',             '        }'],
      ['',             '    }'],
      ['c_dp_ret',     '    return dp[n-1] == INT_MAX ? -1 : dp[n-1];'],
      ['',             '}'],
      ['',             ''],
      ['',             'int main() {'],
      ['c_input_n',    '    int n; cin >> n;'],
      ['',             '    vector<int> arr(n);'],
      ['c_input_arr',  '    for (int i=0;i<n;i++) cin >> arr[i];'],
      ['c_main_call',  '    int result = minJumps(arr, n);'],
      ['c_main_ret',   '    cout << result << endl;'],
      ['',             '    return 0;'],
      ['',             '}']
    ],
    python: [
      ['c_entry',      'def min_jumps(arr, n):'],
      ['c_base_check', '    if n == 1: return 0'],
      ['c_dp_alloc',   '    dp = [float("inf")] * n'],
      ['c_dp_base',    '    dp[0] = 0'],
      ['c_outer_loop', '    for i in range(1, n):'],
      ['c_inner_loop', '        for j in range(i):'],
      ['c_reachable',  '            if dp[j] != float("inf") and j + arr[j] >= i:'],
      ['c_dp_update',  '                dp[i] = min(dp[i], dp[j] + 1)'],
      ['c_dp_ret',     '    return -1 if dp[n-1] == float("inf") else dp[n-1]'],
      ['',             ''],
      ['',             'if __name__ == "__main__":'],
      ['c_input_n',    '    n = int(input())'],
      ['c_input_arr',  '    arr = list(map(int, input().split()))'],
      ['c_main_call',  '    result = min_jumps(arr, n)'],
      ['c_main_ret',   '    print(result)']
    ],
    javascript: [
      ['c_entry',      'function minJumps(arr, n) {'],
      ['c_base_check', '  if (n === 1) return 0;'],
      ['c_dp_alloc',   '  const dp = new Array(n).fill(Infinity);'],
      ['c_dp_base',    '  dp[0] = 0;'],
      ['c_outer_loop', '  for (let i = 1; i < n; i++) {'],
      ['c_inner_loop', '    for (let j = 0; j < i; j++) {'],
      ['c_reachable',  '      if (dp[j] !== Infinity && j + arr[j] >= i) {'],
      ['c_dp_update',  '        dp[i] = Math.min(dp[i], dp[j] + 1);'],
      ['',             '      }'],
      ['',             '    }'],
      ['',             '  }'],
      ['c_dp_ret',     '  return dp[n-1] === Infinity ? -1 : dp[n-1];'],
      ['',             '}'],
      ['',             ''],
      ['c_input_n',    'const lines = require("fs").readFileSync("/dev/stdin","utf8").trim().split("\\n");'],
      ['c_input_arr',  'const arr = lines[1].split(" ").map(Number);'],
      ['c_main_call',  'const result = minJumps(arr, arr.length);'],
      ['c_main_ret',   'console.log(result);']
    ]
  }
};

const PSEUDOCODES = {
  recursion: [
    'function minJumps(arr, n, pos):',
    '    if pos >= n-1: return 0        // Reached or passed the end',
    '    if arr[pos] == 0: return INF   // Stuck! Cannot jump from here',
    '    minCost = INF',
    '    for step = 1 to arr[pos]:      // Try every possible jump length',
    '        sub = minJumps(arr, n, pos + step)',
    '        minCost = min(minCost, 1 + sub)',
    '    return minCost                 // Overlapping subproblems recomputed!'
  ],
  memoization: [
    'memo = array of size n, initialized to -1',
    'function minJumps(arr, n, pos, memo):',
    '    if pos >= n-1: return 0',
    '    if arr[pos] == 0: return INF',
    '    if memo[pos] != -1: return memo[pos]  // Cache Lookup O(1)',
    '    minCost = INF',
    '    for step = 1 to arr[pos]:',
    '        sub = minJumps(arr, n, pos + step, memo)',
    '        minCost = min(minCost, 1 + sub)',
    '    memo[pos] = minCost            // Store result in cache',
    '    return memo[pos]'
  ],
  tabulation: [
    'function minJumps(arr, n):',
    '    dp = array of size n, filled with INF   // Allocate table',
    '    dp[0] = 0                               // 0 jumps to reach start',
    '    for i = 1 to n-1:                       // Fill table bottom-up',
    '        for j = 0 to i-1:',
    '            if dp[j] != INF and j + arr[j] >= i:',
    '                dp[i] = min(dp[i], dp[j] + 1)',
    '    return dp[n-1]                          // O(n^2) time, two loops'
  ]
};

function frame(title, rows) { return { title, rows }; }

const DEFAULT_ARRAY = [2, 3, 1, 1, 4];
const INF = 999999;

/* ==================================================================== */
/* RECURSIVE CALL TREE LAYOUT BUILDER                                    */
/* ==================================================================== */
function buildTreeLayout(approach, arr, n) {
  const nodes = [];
  const edges = [];
  let nextId = 0;
  const seenMap = {};

  if (approach === 'recursion') {
    function buildRec(pos, parentId) {
      const id = nextId++;
      const isRedundant = (seenMap[pos] || 0) > 0;
      seenMap[pos] = (seenMap[pos] || 0) + 1;
      const isEnd = pos >= n - 1;
      const isStuck = !isEnd && arr[pos] === 0;
      const node = { id, pos, parentId, isRedundant, isEnd, isStuck, children: [] };
      nodes.push(node);
      if (parentId !== null) {
        edges.push({ from: parentId, to: id });
      }
      if (!isEnd && !isStuck) {
        const maxS = Math.min(arr[pos], n - 1 - pos);
        for (let s = 1; s <= maxS; s++) {
          const cid = buildRec(pos + s, id);
          node.children.push(cid);
        }
      }
      return id;
    }
    buildRec(0, null);
  } else if (approach === 'memoization') {
    const memo = new Array(n).fill(-1);
    function buildMemo(pos, parentId) {
      const id = nextId++;
      const isCacheHit = memo[pos] !== -1;
      const isEnd = pos >= n - 1;
      const isStuck = !isEnd && arr[pos] === 0;
      const node = { id, pos, parentId, isCacheHit, isEnd, isStuck, children: [] };
      nodes.push(node);
      if (parentId !== null) {
        edges.push({ from: parentId, to: id });
      }
      if (pos >= n - 1) {
        // Base case: reached end
      } else if (isStuck) {
        // Stuck: arr[pos] === 0
      } else if (isCacheHit) {
        // Cache Hit: subtree pruned
      } else {
        const maxS = Math.min(arr[pos], n - 1 - pos);
        for (let s = 1; s <= maxS; s++) {
          const cid = buildMemo(pos + s, id);
          node.children.push(cid);
        }
        memo[pos] = 1; // Marked computed
      }
      return id;
    }
    buildMemo(0, null);
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

  const leafSpacing = 64;
  const levelHeight = 52;

  const computedWidth = Math.max(260, totalLeaves * leafSpacing + 40);
  const computedHeight = Math.max(130, maxDepth * levelHeight + 52);

  function assignCoords(nId) {
    const nd = nodeMap.get(nId);
    if (!nd) return 0;
    if (!nd.children || !nd.children.length) {
      nd.x = 20 + nd.leafIndex * leafSpacing + leafSpacing / 2;
    } else {
      const childXs = nd.children.map(cId => assignCoords(cId));
      nd.x = (childXs[0] + childXs[childXs.length - 1]) / 2;
    }
    nd.y = 26 + nd.depth * levelHeight;
    return nd.x;
  }
  if (nodes.length > 0) {
    assignCoords(0);
  }

  const layoutMap = {};
  nodeMap.forEach((nd, id) => {
    layoutMap[id] = { x: nd.x, y: nd.y, depth: nd.depth };
  });

  const computedEdges = edges.map(e => ({
    from: e.from,
    to: e.to,
    x1: layoutMap[e.from].x,
    y1: layoutMap[e.from].y + 13,
    x2: layoutMap[e.to].x,
    y2: layoutMap[e.to].y - 13,
    isDashed: !!nodeMap.get(e.to)?.isCacheHit
  }));

  return { nodes, edges, computedEdges, layoutMap, width: computedWidth, height: computedHeight };
}

/* ==================================================================== */
/* STEP GENERATOR                                                        */
/* ==================================================================== */
function buildSteps(approach, inputArr) {
  const steps = [];
  const arr = inputArr.slice();
  const n = arr.length;

  /* ------------------------------------------------------------------ */
  /* APPROACH 1: BRUTE FORCE RECURSION                                   */
  /* ------------------------------------------------------------------ */
  if (approach === 'recursion') {
    const TL = buildTreeLayout('recursion', arr, n);
    const nStates = {};
    TL.nodes.forEach(nd => { nStates[nd.id] = { state: 'hidden', returnVal: null }; });
    let callIdx = 0;
    let callCounter = 0;
    let redundantCalls = 0;
    const seenMap = {};
    const stackFrames = [];

    function getTree(activeId) {
      return {
        treeNodes: TL.nodes
          .filter(nd => nStates[nd.id].state !== 'hidden')
          .map(nd => ({
            id: nd.id,
            pos: nd.pos,
            x: TL.layoutMap[nd.id].x,
            y: TL.layoutMap[nd.id].y,
            label: `mJ(${nd.pos})`,
            state: nd.id === activeId ? 'active' : nStates[nd.id].state,
            returnVal: nStates[nd.id].returnVal,
            isCacheHit: !!nd.isCacheHit,
            isRedundant: !!nd.isRedundant,
          })),
        treeEdges: TL.computedEdges
          .filter(e => nStates[e.from].state !== 'hidden' && nStates[e.to].state !== 'hidden')
          .map(e => ({ x1: e.x1, y1: e.y1, x2: e.x2, y2: e.y2, isDashed: e.isDashed })),
        activeNodeId: activeId,
        treeWidth: TL.width,
        treeHeight: TL.height,
      };
    }

    steps.push({
      badge: `Input read: n = ${n}. Array length = ${n}.`,
      code: 'c_input_n',
      vars: [frame('main()', [['n', String(n)]])],
      state: { approach: 'recursion', arr, n, activePos: 0, callCounter: 0, redundantCalls: 0, currentReturn: null, ...getTree(null) }
    });
    steps.push({
      badge: `Input read: arr = [${arr.join(', ')}]. Starting Naive Recursion.`,
      code: 'c_input_arr',
      vars: [frame('main()', [['n', String(n)], ['arr', '[' + arr.join(',') + ']']])],
      state: { approach: 'recursion', arr, n, activePos: 0, callCounter: 0, redundantCalls: 0, currentReturn: null, ...getTree(null) }
    });
    steps.push({
      badge: `main(): Invoking minJumps(arr, ${n}, 0). Solving from index 0.`,
      code: 'c_main_call',
      vars: [frame('main()', [['n', String(n)]])],
      state: { approach: 'recursion', arr, n, activePos: 0, callCounter: 0, redundantCalls: 0, currentReturn: null, ...getTree(null) }
    });

    function solveRec(pos) {
      const myId = callIdx++;
      callCounter++;
      const isRedundant = (seenMap[pos] || 0) > 0;
      seenMap[pos] = (seenMap[pos] || 0) + 1;
      if (isRedundant) redundantCalls++;
      nStates[myId] = { state: isRedundant ? 'redundant' : 'active', returnVal: null };

      const curStack = [...stackFrames, { title: `minJumps(pos=${pos})`, rows: [['pos', String(pos)], ['arr[pos]', pos < n ? String(arr[pos]) : 'N/A']] }];

      steps.push({
        badge: isRedundant
          ? `⚠️ Overlapping! minJumps(pos=${pos}) already explored earlier. Brute force re-explores it! Call #${callCounter}.`
          : `Entering minJumps(pos=${pos}). arr[${pos}]=${arr[pos]}. Call #${callCounter}.`,
        code: 'c_entry',
        vars: curStack.map(f => frame(f.title, f.rows)),
        state: { approach: 'recursion', arr, n, activePos: pos, callCounter, redundantCalls, currentReturn: null, ...getTree(myId) }
      });

      steps.push({
        badge: `Checking base: pos (${pos}) >= n-1 (${n-1}) → ${pos >= n-1 ? 'TRUE ✅ Reached end!' : 'FALSE — continue'}`,
        code: 'c_base_end',
        vars: curStack.map(f => frame(f.title, f.rows)),
        state: { approach: 'recursion', arr, n, activePos: pos, callCounter, redundantCalls, currentReturn: null, ...getTree(myId) }
      });

      if (pos >= n - 1) {
        nStates[myId] = { state: 'solved', returnVal: '0' };
        steps.push({
          badge: `✅ Reached end at pos=${pos}! Returns 0 jumps needed.`,
          code: 'c_base_end',
          vars: curStack.map((f, i) => i === curStack.length - 1 ? frame(f.title, [['pos', String(pos)], ['return', '0']]) : frame(f.title, f.rows)),
          state: { approach: 'recursion', arr, n, activePos: pos, callCounter, redundantCalls, currentReturn: 0, ...getTree(myId) }
        });
        return 0;
      }

      steps.push({
        badge: `Checking: arr[${pos}]==${arr[pos]}${arr[pos] === 0 ? ' → TRUE — Stuck! Cannot jump.' : ' → FALSE — Can jump up to ' + arr[pos] + ' step(s).' }`,
        code: 'c_base_zero',
        vars: curStack.map(f => frame(f.title, f.rows)),
        state: { approach: 'recursion', arr, n, activePos: pos, callCounter, redundantCalls, currentReturn: null, ...getTree(myId) }
      });

      if (arr[pos] === 0) {
        nStates[myId] = { state: 'solved', returnVal: 'INF' };
        steps.push({
          badge: `🚫 Stuck at pos=${pos} (arr[${pos}]=0)! Returns INF (unreachable).`,
          code: 'c_base_zero',
          vars: curStack.map((f, i) => i === curStack.length - 1 ? frame(f.title, [['pos', String(pos)], ['return', 'INF']]) : frame(f.title, f.rows)),
          state: { approach: 'recursion', arr, n, activePos: pos, callCounter, redundantCalls, currentReturn: null, ...getTree(myId) }
        });
        return INF;
      }

      nStates[myId] = { state: isRedundant ? 'redundant' : 'active', returnVal: null };
      steps.push({
        badge: `Initializing minCost = INF. Will try jump steps 1 to arr[${pos}]=${arr[pos]}.`,
        code: 'c_init_min',
        vars: curStack.map(f => frame(f.title, f.rows)),
        state: { approach: 'recursion', arr, n, activePos: pos, callCounter, redundantCalls, currentReturn: null, ...getTree(myId) }
      });

      let minCost = INF;
      const maxSteps = Math.min(arr[pos], n - 1 - pos);
      for (let step = 1; step <= maxSteps; step++) {
        const nextPos = pos + step;
        steps.push({
          badge: `Loop step=${step}/${arr[pos]}: Trying jump from pos ${pos} → pos ${nextPos}.`,
          code: 'c_loop',
          vars: curStack.map((f, i) => i === curStack.length - 1 ? frame(f.title, [...f.rows, ['step', String(step)]]) : frame(f.title, f.rows)),
          state: { approach: 'recursion', arr, n, activePos: pos, callCounter, redundantCalls, currentReturn: null, ...getTree(myId) }
        });

        steps.push({
          badge: `Recursing: minJumps(arr, ${n}, pos=${nextPos}).`,
          code: 'c_rec_call',
          vars: curStack.map((f, i) => i === curStack.length - 1 ? frame(f.title, [...f.rows, ['step', String(step)]]) : frame(f.title, f.rows)),
          state: { approach: 'recursion', arr, n, activePos: pos, callCounter, redundantCalls, currentReturn: null, ...getTree(myId) }
        });

        stackFrames.push({ title: `minJumps(pos=${pos})`, rows: [['pos', String(pos)], ['step', String(step)]] });
        const sub = solveRec(nextPos);
        stackFrames.pop();

        if (sub !== INF) {
          const newCost = 1 + sub;
          const improved = newCost < minCost;
          if (improved) minCost = newCost;
          steps.push({
            badge: `sub=${sub}. 1+sub=${newCost}. ${improved ? `New best! minCost updated to ${minCost}.` : `No improvement (minCost remains ${minCost}).`}`,
            code: 'c_update_min',
            vars: curStack.map((f, i) => i === curStack.length - 1 ? frame(f.title, [...f.rows, ['step', String(step)], ['minCost', String(minCost)]]) : frame(f.title, f.rows)),
            state: { approach: 'recursion', arr, n, activePos: pos, callCounter, redundantCalls, currentReturn: minCost, ...getTree(myId) }
          });
        } else {
          steps.push({
            badge: `sub=INF (unreachable via pos ${nextPos}). minCost remains ${minCost === INF ? 'INF' : minCost}.`,
            code: 'c_update_min',
            vars: curStack.map((f, i) => i === curStack.length - 1 ? frame(f.title, [...f.rows, ['step', String(step)], ['minCost', minCost === INF ? 'INF' : String(minCost)]]) : frame(f.title, f.rows)),
            state: { approach: 'recursion', arr, n, activePos: pos, callCounter, redundantCalls, currentReturn: null, ...getTree(myId) }
          });
        }
      }

      nStates[myId] = { state: 'solved', returnVal: minCost === INF ? 'INF' : String(minCost) };
      steps.push({
        badge: `All jumps from pos=${pos} explored. minCost=${minCost === INF ? 'INF' : minCost}. Returning.`,
        code: 'c_ret_min',
        vars: curStack.map((f, i) => i === curStack.length - 1 ? frame(f.title, [['pos', String(pos)], ['return', minCost === INF ? 'INF' : String(minCost)]]) : frame(f.title, f.rows)),
        state: { approach: 'recursion', arr, n, activePos: pos, callCounter, redundantCalls, currentReturn: minCost === INF ? null : minCost, ...getTree(myId) }
      });
      return minCost;
    }

    const finalAnswer = solveRec(0);
    steps.push({
      badge: `✅ Brute Force Complete! Minimum jumps = ${finalAnswer === INF ? -1 : finalAnswer}. Total calls: ${callCounter}. Redundant: ${redundantCalls}.`,
      code: 'c_main_ret',
      vars: [frame('main()', [['n', String(n)], ['result', finalAnswer === INF ? '-1' : String(finalAnswer)]])],
      state: { approach: 'recursion', arr, n, activePos: n - 1, callCounter, redundantCalls, currentReturn: finalAnswer === INF ? -1 : finalAnswer, ...getTree(null) }
    });
    return steps;
  }

  /* ------------------------------------------------------------------ */
  /* APPROACH 2: MEMOIZATION (TOP-DOWN DP)                               */
  /* ------------------------------------------------------------------ */
  if (approach === 'memoization') {
    const TL = buildTreeLayout('memoization', arr, n);
    const nStates = {};
    TL.nodes.forEach(nd => { nStates[nd.id] = { state: 'hidden', returnVal: null }; });
    let callIdx = 0;
    const memo = new Array(n).fill(-1);
    let callCounter = 0;
    let cacheHits = 0;
    const stackFrames = [];

    function getTree(activeId) {
      return {
        treeNodes: TL.nodes
          .filter(nd => nStates[nd.id].state !== 'hidden')
          .map(nd => ({
            id: nd.id,
            pos: nd.pos,
            x: TL.layoutMap[nd.id].x,
            y: TL.layoutMap[nd.id].y,
            label: `mJ(${nd.pos})`,
            state: nd.id === activeId ? 'active' : nStates[nd.id].state,
            returnVal: nStates[nd.id].returnVal,
            isCacheHit: !!nd.isCacheHit,
            isRedundant: !!nd.isRedundant,
          })),
        treeEdges: TL.computedEdges
          .filter(e => nStates[e.from].state !== 'hidden' && nStates[e.to].state !== 'hidden')
          .map(e => ({ x1: e.x1, y1: e.y1, x2: e.x2, y2: e.y2, isDashed: e.isDashed })),
        activeNodeId: activeId,
        treeWidth: TL.width,
        treeHeight: TL.height,
      };
    }

    steps.push({
      badge: `Input read: n = ${n}. Array length = ${n}.`,
      code: 'c_input_n',
      vars: [frame('main()', [['n', String(n)]])],
      state: { approach: 'memoization', arr, n, activePos: 0, callCounter: 0, cacheHits: 0, memo: [...memo], currentReturn: null, ...getTree(null) }
    });
    steps.push({
      badge: `Input read: arr = [${arr.join(', ')}]. Starting Top-Down DP with Memoization.`,
      code: 'c_input_arr',
      vars: [frame('main()', [['n', String(n)], ['arr', '[' + arr.join(',') + ']']])],
      state: { approach: 'memoization', arr, n, activePos: 0, callCounter: 0, cacheHits: 0, memo: [...memo], currentReturn: null, ...getTree(null) }
    });
    steps.push({
      badge: `Allocating memo[0..${n-1}] table of size ${n}.`,
      code: 'c_memo_alloc',
      vars: [frame('main()', [['n', String(n)], ['memo.length', String(n)]])],
      state: { approach: 'memoization', arr, n, activePos: 0, callCounter: 0, cacheHits: 0, memo: [...memo], currentReturn: null, ...getTree(null) }
    });
    steps.push({
      badge: `Initializing memo table with -1 sentinel: all ${n} subproblems marked uncached.`,
      code: 'c_memo_fill',
      vars: [frame('main()', [['n', String(n)], ['memo[0..n-1]', '-1']])],
      state: { approach: 'memoization', arr, n, activePos: 0, callCounter: 0, cacheHits: 0, memo: [...memo], currentReturn: null, ...getTree(null) }
    });
    steps.push({
      badge: `main(): Invoking minJumps(arr, ${n}, 0, memo). Starting from index 0.`,
      code: 'c_main_call',
      vars: [frame('main()', [['n', String(n)]])],
      state: { approach: 'memoization', arr, n, activePos: 0, callCounter: 0, cacheHits: 0, memo: [...memo], currentReturn: null, ...getTree(null) }
    });

    function solveMemo(pos) {
      const myId = callIdx++;
      callCounter++;
      const isCacheHit = memo[pos] !== -1;
      if (isCacheHit) cacheHits++;
      const cachedVal = memo[pos];
      const cachedStr = cachedVal === INF ? 'INF' : String(cachedVal);
      nStates[myId] = { state: isCacheHit ? 'cachehit' : 'active', returnVal: isCacheHit ? cachedStr : null };
      const curStack = [...stackFrames, { title: `minJumps(pos=${pos})`, rows: [['pos', String(pos)]] }];

      steps.push({
        badge: isCacheHit
          ? `🚀 Cache Hit! memo[${pos}] = ${cachedStr}. Instantly available without recursion! Call #${callCounter}.`
          : `Entering minJumps(pos=${pos}). Not cached yet. Call #${callCounter}.`,
        code: 'c_entry',
        vars: curStack.map(f => frame(f.title, f.rows)),
        state: { approach: 'memoization', arr, n, activePos: pos, callCounter, cacheHits, memo: [...memo], currentReturn: null, ...getTree(myId) }
      });

      steps.push({
        badge: `Checking base: pos (${pos}) >= n-1 (${n-1}) → ${pos >= n-1 ? 'TRUE ✅ Reached end!' : 'FALSE — continue'}`,
        code: 'c_base_end',
        vars: curStack.map(f => frame(f.title, f.rows)),
        state: { approach: 'memoization', arr, n, activePos: pos, callCounter, cacheHits, memo: [...memo], currentReturn: null, ...getTree(myId) }
      });

      if (pos >= n - 1) {
        nStates[myId] = { state: 'solved', returnVal: '0' };
        steps.push({
          badge: `✅ Reached end at pos=${pos}! Returns 0 jumps needed.`,
          code: 'c_base_end',
          vars: curStack.map((f, i) => i === curStack.length - 1 ? frame(f.title, [['pos', String(pos)], ['return', '0']]) : frame(f.title, f.rows)),
          state: { approach: 'memoization', arr, n, activePos: pos, callCounter, cacheHits, memo: [...memo], currentReturn: 0, ...getTree(myId) }
        });
        return 0;
      }

      steps.push({
        badge: `Checking: arr[${pos}]==${arr[pos]}${arr[pos] === 0 ? ' → TRUE — Stuck! Cannot jump.' : ' → FALSE — Can jump up to ' + arr[pos] + ' step(s).' }`,
        code: 'c_base_zero',
        vars: curStack.map(f => frame(f.title, f.rows)),
        state: { approach: 'memoization', arr, n, activePos: pos, callCounter, cacheHits, memo: [...memo], currentReturn: null, ...getTree(myId) }
      });

      if (arr[pos] === 0) {
        nStates[myId] = { state: 'solved', returnVal: 'INF' };
        steps.push({
          badge: `🚫 arr[${pos}]=0! Stuck. Returns INF.`,
          code: 'c_base_zero',
          vars: curStack.map((f, i) => i === curStack.length - 1 ? frame(f.title, [['pos', String(pos)], ['return', 'INF']]) : frame(f.title, f.rows)),
          state: { approach: 'memoization', arr, n, activePos: pos, callCounter, cacheHits, memo: [...memo], currentReturn: null, ...getTree(myId) }
        });
        return INF;
      }

      steps.push({
        badge: `Checking cache: memo[${pos}] != -1: ${isCacheHit ? `TRUE → Hit! Cached value = ${cachedStr}` : 'FALSE → Miss! Computing first time.'}`,
        code: 'c_memo_check',
        vars: curStack.map(f => frame(f.title, f.rows)),
        state: { approach: 'memoization', arr, n, activePos: pos, callCounter, cacheHits, memo: [...memo], currentReturn: null, ...getTree(myId) }
      });

      if (isCacheHit) {
        nStates[myId] = { state: 'cachehit', returnVal: cachedStr };
        steps.push({
          badge: `⚡ Instant O(1) Return! memo[${pos}] = ${cachedStr}. Subtree pruned!`,
          code: 'c_memo_check',
          vars: curStack.map((f, i) => i === curStack.length - 1 ? frame(f.title, [['pos', String(pos)], ['cache_hit', cachedStr]]) : frame(f.title, f.rows)),
          state: { approach: 'memoization', arr, n, activePos: pos, callCounter, cacheHits, memo: [...memo], currentReturn: cachedVal === INF ? null : cachedVal, ...getTree(myId) }
        });
        return cachedVal;
      }

      nStates[myId] = { state: 'active', returnVal: null };
      steps.push({
        badge: `Initializing minCost = INF. Trying jump steps 1 to arr[${pos}]=${arr[pos]}.`,
        code: 'c_init_min',
        vars: curStack.map(f => frame(f.title, f.rows)),
        state: { approach: 'memoization', arr, n, activePos: pos, callCounter, cacheHits, memo: [...memo], currentReturn: null, ...getTree(myId) }
      });

      let minCost = INF;
      const maxSteps = Math.min(arr[pos], n - 1 - pos);
      for (let step = 1; step <= maxSteps; step++) {
        const nextPos = pos + step;
        steps.push({
          badge: `Loop step=${step}: Trying pos ${pos} → pos ${nextPos}.`,
          code: 'c_loop',
          vars: curStack.map((f, i) => i === curStack.length - 1 ? frame(f.title, [...f.rows, ['step', String(step)]]) : frame(f.title, f.rows)),
          state: { approach: 'memoization', arr, n, activePos: pos, callCounter, cacheHits, memo: [...memo], currentReturn: null, ...getTree(myId) }
        });

        steps.push({
          badge: `Recursing: minJumps(arr, ${n}, ${nextPos}, memo).`,
          code: 'c_rec_call',
          vars: curStack.map((f, i) => i === curStack.length - 1 ? frame(f.title, [...f.rows, ['step', String(step)]]) : frame(f.title, f.rows)),
          state: { approach: 'memoization', arr, n, activePos: pos, callCounter, cacheHits, memo: [...memo], currentReturn: null, ...getTree(myId) }
        });

        stackFrames.push({ title: `minJumps(pos=${pos})`, rows: [['pos', String(pos)], ['step', String(step)]] });
        const sub = solveMemo(nextPos);
        stackFrames.pop();

        if (sub !== INF) {
          const newCost = 1 + sub;
          const improved = newCost < minCost;
          if (improved) minCost = newCost;
          steps.push({
            badge: `sub=${sub}. 1+sub=${newCost}. ${improved ? `minCost updated to ${minCost}.` : 'No improvement (minCost stays ' + minCost + ').'}`,
            code: 'c_update_min',
            vars: curStack.map((f, i) => i === curStack.length - 1 ? frame(f.title, [...f.rows, ['step', String(step)], ['minCost', String(minCost)]]) : frame(f.title, f.rows)),
            state: { approach: 'memoization', arr, n, activePos: pos, callCounter, cacheHits, memo: [...memo], currentReturn: minCost, ...getTree(myId) }
          });
        } else {
          steps.push({
            badge: `sub=INF (unreachable via pos ${nextPos}). minCost remains ${minCost === INF ? 'INF' : minCost}.`,
            code: 'c_update_min',
            vars: curStack.map((f, i) => i === curStack.length - 1 ? frame(f.title, [...f.rows, ['step', String(step)], ['minCost', minCost === INF ? 'INF' : String(minCost)]]) : frame(f.title, f.rows)),
            state: { approach: 'memoization', arr, n, activePos: pos, callCounter, cacheHits, memo: [...memo], currentReturn: null, ...getTree(myId) }
          });
        }
      }

      memo[pos] = minCost;
      nStates[myId] = { state: 'solved', returnVal: minCost === INF ? 'INF' : String(minCost) };

      steps.push({
        badge: `Storing memo[${pos}] = ${minCost === INF ? 'INF' : minCost}. Future calls for pos=${pos} will get O(1) cache return!`,
        code: 'c_memo_store',
        vars: curStack.map((f, i) => i === curStack.length - 1 ? frame(f.title, [['pos', String(pos)], ['memo[pos]', minCost === INF ? 'INF' : String(minCost)]]) : frame(f.title, f.rows)),
        state: { approach: 'memoization', arr, n, activePos: pos, callCounter, cacheHits, memo: [...memo], currentReturn: minCost === INF ? null : minCost, ...getTree(myId) }
      });

      steps.push({
        badge: `Returning memo[${pos}] = ${minCost === INF ? 'INF' : minCost}.`,
        code: 'c_ret_min',
        vars: curStack.map((f, i) => i === curStack.length - 1 ? frame(f.title, [['pos', String(pos)], ['return', minCost === INF ? 'INF' : String(minCost)]]) : frame(f.title, f.rows)),
        state: { approach: 'memoization', arr, n, activePos: pos, callCounter, cacheHits, memo: [...memo], currentReturn: minCost === INF ? null : minCost, ...getTree(myId) }
      });

      return minCost;
    }

    const finalAnswer = solveMemo(0);
    const finalResult = finalAnswer === INF ? -1 : finalAnswer;
    steps.push({
      badge: `✅ Memoization Complete! minJumps = ${finalResult}. Solved in O(n²) with ${cacheHits} O(1) cache hits!`,
      code: 'c_main_ret',
      vars: [frame('main()', [['n', String(n)], ['result', String(finalResult)]])],
      state: { approach: 'memoization', arr, n, activePos: n - 1, callCounter, cacheHits, memo: [...memo], currentReturn: finalResult, ...getTree(null) }
    });
    return steps;
  }

  /* ------------------------------------------------------------------ */
  /* APPROACH 3: DP TABULATION (BOTTOM-UP)                              */
  /* ------------------------------------------------------------------ */
  if (approach === 'tabulation') {
    const dpVals = new Array(n).fill(INF);
    const dpCells = dpVals.map((v,idx) => ({ idx, val:v, state:'uncalc' }));
    let totalOps = 0;
    let iterations = 0;

    steps.push({
      badge: `Input read: n = ${n}. Array length = ${n}.`,
      code: 'c_input_n',
      vars: [frame('main()', [['n',String(n)]])],
      state: { approach:'tabulation', arr, n, dpCells:dpCells.map(c=>({...c})), i:-1, j:-1, totalOps, iterations, currentReturn:null }
    });
    steps.push({
      badge: `Input read: arr = [${arr.join(', ')}]. Starting Bottom-Up DP Tabulation.`,
      code: 'c_input_arr',
      vars: [frame('main()', [['n',String(n)],['arr','['+arr.join(',')+']']])],
      state: { approach:'tabulation', arr, n, dpCells:dpCells.map(c=>({...c})), i:-1, j:-1, totalOps, iterations, currentReturn:null }
    });
    steps.push({
      badge: `main(): Invoking minJumps(arr, ${n}).`,
      code: 'c_main_call',
      vars: [frame('main()', [['n',String(n)]])],
      state: { approach:'tabulation', arr, n, dpCells:dpCells.map(c=>({...c})), i:-1, j:-1, totalOps, iterations, currentReturn:null }
    });
    steps.push({
      badge: `Entering minJumps(n=${n}).`,
      code: 'c_entry',
      vars: [frame('main()',[['n',String(n)]]), frame('minJumps()',[['n',String(n)]])],
      state: { approach:'tabulation', arr, n, dpCells:dpCells.map(c=>({...c})), i:-1, j:-1, totalOps:++totalOps, iterations, currentReturn:null }
    });
    steps.push({
      badge: `if (n == 1): ${n===1 ? 'TRUE — Already at end, return 0.' : 'FALSE — Proceed.'}`,
      code: 'c_base_check',
      vars: [frame('main()',[['n',String(n)]]), frame('minJumps()',[['n',String(n)]])],
      state: { approach:'tabulation', arr, n, dpCells:dpCells.map(c=>({...c})), i:-1, j:-1, totalOps, iterations, currentReturn:null }
    });
    if (n===1) {
      steps.push({ badge:'n=1: Only one element. Returns 0.', code:'c_base_check', vars:[frame('main()',[['n',String(n)],['result','0']]), frame('minJumps()',[['n',String(n)],['return','0']])], state:{ approach:'tabulation', arr, n, dpCells:dpCells.map(c=>({...c})), i:-1, j:-1, totalOps, iterations, currentReturn:0 } });
      return steps;
    }

    steps.push({
      badge: `Allocating dp[0..${n-1}] table of size ${n}.`,
      code: 'c_dp_alloc',
      vars: [frame('main()',[['n',String(n)]]), frame('minJumps()',[['n',String(n)],['dp.length',String(n)]])],
      state: { approach:'tabulation', arr, n, dpCells:dpCells.map(c=>({...c})), i:-1, j:-1, totalOps:++totalOps, iterations, currentReturn:null }
    });
    steps.push({
      badge: `Initializing dp[0..${n-1}] table with INF (unreachable).`,
      code: 'c_dp_fill',
      vars: [frame('main()',[['n',String(n)]]), frame('minJumps()',[['n',String(n)],['dp[0..n-1]','INF']])],
      state: { approach:'tabulation', arr, n, dpCells:dpCells.map(c=>({...c})), i:-1, j:-1, totalOps:++totalOps, iterations, currentReturn:null }
    });
    dpCells[0] = { idx:0, val:0, state:'base' };
    steps.push({
      badge: 'Setting dp[0] = 0. Start index needs 0 jumps.',
      code: 'c_dp_base',
      vars: [frame('main()',[['n',String(n)]]), frame('minJumps()',[['n',String(n)],['dp[0]','0']])],
      state: { approach:'tabulation', arr, n, dpCells:dpCells.map(c=>({...c})), i:0, j:-1, totalOps:++totalOps, iterations, currentReturn:null }
    });

    for (let i = 1; i < n; i++) {
      iterations++;
      steps.push({
        badge: `Outer loop i=${i}: Computing minimum jumps to reach index ${i}.`,
        code: 'c_outer_loop',
        vars: [frame('main()',[['n',String(n)]]), frame('minJumps()',[['n',String(n)],['i',String(i)]])],
        state: {
          approach:'tabulation', arr, n,
          dpCells: dpCells.map((c,idx) => ({...c, state: idx===i ? 'adding' : (idx===0 ? 'base' : (idx<i ? 'solved' : 'uncalc'))})),
          i, j:-1, totalOps:++totalOps, iterations, currentReturn:null
        }
      });

      for (let j = 0; j < i; j++) {
        steps.push({
          badge: `Inner j=${j}: dp[${j}]=${dpCells[j].val===INF?'INF':dpCells[j].val}, j+arr[${j}]=${j+arr[j]}, target i=${i}.`,
          code: 'c_inner_loop',
          vars: [frame('main()',[['n',String(n)]]), frame('minJumps()',[['n',String(n)],['i',String(i)],['j',String(j)]])],
          state: {
            approach:'tabulation', arr, n,
            dpCells: dpCells.map((c,idx) => ({...c, state: idx===j ? 'source' : (idx===i ? 'adding' : (idx===0 ? 'base' : (idx<i ? 'solved' : 'uncalc')))})),
            i, j, totalOps:++totalOps, iterations, currentReturn:null
          }
        });
        const canReach = dpCells[j].val !== INF && j+arr[j] >= i;
        steps.push({
          badge: `Reachability: dp[${j}]!=INF (${dpCells[j].val!==INF}) AND j+arr[j]=${j+arr[j]} >= i=${i} (${j+arr[j]>=i}). ${canReach ? '✅ Reachable!' : '❌ Not reachable.'}`,
          code: 'c_reachable',
          vars: [frame('main()',[['n',String(n)]]), frame('minJumps()',[['n',String(n)],['i',String(i)],['j',String(j)],['j+arr[j]',String(j+arr[j])]])],
          state: {
            approach:'tabulation', arr, n,
            dpCells: dpCells.map((c,idx) => ({...c, state: idx===j ? 'source' : (idx===i ? 'adding' : (idx===0 ? 'base' : (idx<i ? 'solved' : 'uncalc')))})),
            i, j, totalOps:++totalOps, iterations, currentReturn:null
          }
        });
        if (canReach) {
          const candidate = dpCells[j].val+1;
          const improved = candidate < dpCells[i].val;
          if (improved) dpCells[i] = { idx:i, val:candidate, state:'adding' };
          steps.push({
            badge: `dp[${j}]+1=${candidate}. ${improved ? `Improvement! dp[${i}] = ${candidate}.` : `No improvement (dp[${i}]=${dpCells[i].val}).`}`,
            code: 'c_dp_update',
            vars: [frame('main()',[['n',String(n)]]), frame('minJumps()',[['n',String(n)],['i',String(i)],['j',String(j)],['dp[i]',dpCells[i].val===INF?'INF':String(dpCells[i].val)]])],
            state: {
              approach:'tabulation', arr, n,
              dpCells: dpCells.map((c,idx) => ({...c, state: idx===j ? 'source' : (idx===i ? 'adding' : (idx===0 ? 'base' : (idx<i ? 'solved' : 'uncalc')))})),
              i, j, totalOps:++totalOps, iterations, currentReturn: dpCells[i].val===INF?null:dpCells[i].val
            }
          });
        }
      }

      dpCells[i] = { ...dpCells[i], state:'solved' };
      steps.push({
        badge: `i=${i} finalized: dp[${i}] = ${dpCells[i].val===INF ? 'INF (unreachable)' : dpCells[i].val+' jump(s)'}.`,
        code: 'c_outer_loop',
        vars: [frame('main()',[['n',String(n)]]), frame('minJumps()',[['n',String(n)],['i',String(i)],['dp[i]',dpCells[i].val===INF?'INF':String(dpCells[i].val)]])],
        state: {
          approach:'tabulation', arr, n,
          dpCells: dpCells.map((c,idx) => ({...c, state: idx<=i ? (idx===0?'base':'solved') : 'uncalc'})),
          i, j:-1, totalOps, iterations, currentReturn: dpCells[i].val===INF?null:dpCells[i].val
        }
      });
    }

    const finalResult = dpCells[n-1].val===INF ? -1 : dpCells[n-1].val;
    steps.push({
      badge: `return dp[${n-1}] = ${dpCells[n-1].val===INF?'INF → -1 (unreachable)':finalResult+' jump(s)'}. Tabulation done in O(n²).`,
      code: 'c_dp_ret',
      vars: [frame('main()',[['n',String(n)],['result',String(finalResult)]]), frame('minJumps()',[['n',String(n)],['return',String(finalResult)]])],
      state: { approach:'tabulation', arr, n, dpCells:dpCells.map(c=>({...c})), i:n-1, j:-1, totalOps:++totalOps, iterations, currentReturn:finalResult }
    });
    steps.push({
      badge: `✅ Main Execution Complete! Minimum jumps to reach end = ${finalResult}.`,
      code: 'c_main_ret',
      vars: [frame('main()', [['n',String(n)],['result',String(finalResult)]])],
      state: { approach:'tabulation', arr, n, dpCells:dpCells.map(c=>({...c})), i:n-1, j:-1, totalOps:++totalOps, iterations, currentReturn:finalResult }
    });
    return steps;
  }

  return steps;
}

/* ==================================================================== */
/* REACTIVE STATE & CONTROLS                                            */
/* ==================================================================== */
const currentApproach = ref('recursion');
const inputArrayStr = ref(DEFAULT_ARRAY.join(', '));
const lang = ref('java');
const speed = ref(650);
const si = ref(0);
const playing = ref(false);
const vizHeight = ref(250);
const tableHeight = ref(42);
const leftWidth = ref(54);
const rightTab = ref('code');

const showWarningModal = ref(false);
const warningModalTitle = ref('Input Notice');
const warningModalMsg = ref('');
const warningModalLimit = ref(10);
const requestedValue = ref('');

function closeWarningModal() { showWarningModal.value = false; }

const maxAllowedN = computed(() => currentApproach.value === 'tabulation' ? 10 : 6);

function parseInputArray() {
  return inputArrayStr.value
    .replace(/[\[\]]/g, '')
    .split(/[\s,]+/)
    .map(s => parseInt(s.trim(), 10))
    .filter(v => !isNaN(v) && v >= 0);
}

const stepsData = reactive({ steps: buildSteps('recursion', DEFAULT_ARRAY) });
const steps = computed(() => stepsData.steps);
const s = computed(() => steps.value[Math.max(0, Math.min(si.value, steps.value.length - 1))] || { state: {} });
const st = computed(() => s.value.state || {});

const codeLines = computed(() => {
  const appCodes = CODES[currentApproach.value] || CODES.recursion;
  return appCodes[lang.value] || appCodes.java || [];
});

function isLineActive(line) {
  if (!line[0] || !s.value.code) return false;
  if (line[0] === s.value.code) return true;
  if (s.value.code === 'c_memo_fill' && line[0] === 'c_memo_alloc' && !codeLines.value.some(l => l[0] === 'c_memo_fill')) {
    return true;
  }
  if (s.value.code === 'c_dp_fill' && line[0] === 'c_dp_alloc' && !codeLines.value.some(l => l[0] === 'c_dp_fill')) {
    return true;
  }
  return false;
}

const pseudocodeLines = computed(() => PSEUDOCODES[currentApproach.value] || PSEUDOCODES.recursion);

let playTimer = null;

function applyApproach(newApproach) {
  currentApproach.value = newApproach;
  playing.value = false;
  const arr = parseInputArray();
  const maxN = newApproach === 'tabulation' ? 10 : 6;
  if (arr.length > maxN) {
    requestedValue.value = inputArrayStr.value;
    const clamped = arr.slice(0, maxN);
    inputArrayStr.value = clamped.join(', ');
    warningModalTitle.value = `Limit (Max ${maxN} elements)`;
    warningModalMsg.value = `Array has ${arr.length} elements exceeding max ${maxN} for this approach. Clamped to first ${maxN} elements.`;
    warningModalLimit.value = maxN;
    showWarningModal.value = true;
    stepsData.steps = buildSteps(newApproach, clamped);
  } else {
    stepsData.steps = buildSteps(newApproach, arr.length > 0 ? arr : DEFAULT_ARRAY);
  }
  si.value = 0;
}

function applyInput() {
  playing.value = false;
  const arr = parseInputArray();
  const maxN = maxAllowedN.value;
  if (arr.length === 0) {
    stepsData.steps = buildSteps(currentApproach.value, DEFAULT_ARRAY);
    inputArrayStr.value = DEFAULT_ARRAY.join(', ');
    si.value = 0;
    return;
  }
  if (arr.length > maxN) {
    requestedValue.value = inputArrayStr.value;
    const clamped = arr.slice(0, maxN);
    inputArrayStr.value = clamped.join(', ');
    warningModalTitle.value = `Input Limit (Max ${maxN} elements)`;
    warningModalMsg.value = `Array has ${arr.length} elements. Max for ${currentApproach.value} is ${maxN}. Clamped to first ${maxN}.`;
    warningModalLimit.value = maxN;
    showWarningModal.value = true;
    stepsData.steps = buildSteps(currentApproach.value, clamped);
  } else {
    stepsData.steps = buildSteps(currentApproach.value, arr);
  }
  si.value = 0;
}

function stepBy(d) { si.value = Math.max(0, Math.min(steps.value.length - 1, si.value + d)); }
function togglePlay() {
  const next = !playing.value;
  if (next && si.value >= steps.value.length - 1) si.value = 0;
  playing.value = next;
}
function tick() {
  clearTimeout(playTimer);
  if (!playing.value) return;
  if (si.value >= steps.value.length - 1) { playing.value = false; return; }
  playTimer = setTimeout(() => { si.value = Math.min(steps.value.length - 1, si.value + 1); tick(); }, 2100 - speed.value);
}
watch(playing, v => { if (v) tick(); else clearTimeout(playTimer); });

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
  const onDown = e => { dragging = true; startX = e.clientX; startW = leftColRef.value.offsetWidth; rsz.classList.add('drag'); document.body.style.userSelect = 'none'; };
  const onMove = e => { if (!dragging) return; const mainW = main.offsetWidth; leftWidth.value = (Math.max(220, Math.min(mainW - 220, startW + e.clientX - startX)) / mainW) * 100; };
  const onUp = () => { if (!dragging) return; dragging = false; rsz.classList.remove('drag'); document.body.style.userSelect = ''; };
  rsz.addEventListener('mousedown', onDown); document.addEventListener('mousemove', onMove); document.addEventListener('mouseup', onUp);
  return () => { rsz.removeEventListener('mousedown', onDown); document.removeEventListener('mousemove', onMove); document.removeEventListener('mouseup', onUp); };
}

function initVResizer(elRef, valueRef, minH, maxH) {
  const rsz = elRef.value;
  if (!rsz) return;
  let dragging = false, startY = 0, startH = 0;
  const onDown = e => { dragging = true; startY = e.clientY; startH = valueRef.value; rsz.classList.add('drag'); document.body.style.userSelect = 'none'; e.preventDefault(); };
  const onMove = e => { if (!dragging) return; valueRef.value = Math.max(minH, Math.min(maxH, startH + (e.clientY - startY))); };
  const onUp = () => { if (!dragging) return; dragging = false; rsz.classList.remove('drag'); document.body.style.userSelect = ''; };
  rsz.addEventListener('mousedown', onDown); document.addEventListener('mousemove', onMove); document.addEventListener('mouseup', onUp);
  return () => { rsz.removeEventListener('mousedown', onDown); document.removeEventListener('mousemove', onMove); document.removeEventListener('mouseup', onUp); };
}

watch([() => s.value.code, () => si.value, () => lang.value, () => rightTab.value], () => {
  if (rightTab.value !== 'code') return;
  setTimeout(() => {
    const el = document.querySelector('.ll-code-scroll .ll-hl');
    if (el) {
      el.scrollIntoView({ block: 'nearest', behavior: 'smooth' });
    }
  }, 10);
});

let cleanupFns = [];
onMounted(() => {
  document.addEventListener('keydown', onKeydown);
  cleanupFns.push(initHResizer());
  cleanupFns.push(initVResizer(vizResizerRef, vizHeight, 130, 500));
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
    <div class="navbar">
      <h2 class="navbar-title">{{ topic }} &mdash; {{ subTopic }}</h2>
      <img src="../../assets/logo.png" alt="Logo" />
    </div>

    <div class="slide-body">
      <div class="row-main">
        <div class="ll-root">
          <!-- Toolbar -->
          <div class="ll-toolbar">
            <div class="ll-approach-group">
              <button v-for="app in APPROACHES" :key="app.id" class="ll-approach-btn"
                :class="{ active: currentApproach === app.id }" :title="app.desc" @click="applyApproach(app.id)">
                {{ app.label }}
              </button>
            </div>
            <div class="ll-input-group">
              <label>arr =</label>
              <input type="text" v-model="inputArrayStr" class="ll-arr-input"
                :placeholder="`e.g. ${DEFAULT_ARRAY.join(', ')}`" @keyup.enter="applyInput" />
              <span class="ll-input-hint">(max {{ maxAllowedN }} elements)</span>
            </div>
            <button class="ll-viz-btn" @click="applyInput">&#9654; Visualize</button>
            <div class="ll-nav-controls">
              <button class="ll-nav-btn" @click="stepBy(-steps.length)">&#171;</button>
              <button class="ll-nav-btn" @click="stepBy(-1)">&#8249; Prev</button>
              <button class="ll-play-btn" @click="togglePlay">{{ playing ? '\u23F8 Pause' : '\u25B6 Play' }}</button>
              <button class="ll-nav-btn" @click="stepBy(1)">Next &#8250;</button>
              <button class="ll-nav-btn" @click="stepBy(steps.length)">&#187;</button>
            </div>
          </div>

          <div class="ll-main" ref="mainRef">
            <!-- Left Column -->
            <div class="ll-left-col" ref="leftColRef" :style="{ width: leftWidth + '%' }">
              <div class="ll-viz-wrap" :style="{ height: vizHeight + 'px' }">
                <div class="ll-perm-area">
                  <!-- Chips -->
                  <div class="ll-ptrs">
                    <div class="ll-ptr-chip">n = <b class="ll-c-blue">{{ st.n !== undefined ? st.n : (st.arr ? st.arr.length : 0) }}</b></div>
                    <div class="ll-ptr-chip" v-if="currentApproach !== 'tabulation'">Calls: <b class="ll-c-orange">{{ st.callCounter || 0 }}</b></div>
                    <div class="ll-ptr-chip" v-if="currentApproach === 'tabulation'">Iterations: <b class="ll-c-orange">{{ st.iterations !== undefined ? st.iterations : 0 }}</b></div>
                    <div class="ll-ptr-chip" v-if="currentApproach === 'recursion'" :class="{ 'll-chip-warn': (st.redundantCalls || 0) > 0 }">Redundant: <b class="ll-c-red">{{ st.redundantCalls || 0 }}</b></div>
                    <div class="ll-ptr-chip" v-if="currentApproach === 'memoization'">Cache Hits: <b class="ll-c-green">{{ st.cacheHits || 0 }}</b></div>
                    <div class="ll-ptr-chip" v-if="currentApproach === 'tabulation' && st.i >= 0">i = <b class="ll-c-purple">{{ st.i }}</b></div>
                    <div class="ll-ptr-chip" v-if="currentApproach === 'tabulation' && st.j >= 0">j = <b class="ll-c-orange">{{ st.j }}</b></div>
                    <div class="ll-ptr-chip" v-if="st.currentReturn !== null && st.currentReturn !== undefined">Result: <b class="ll-c-green">{{ st.currentReturn }}</b></div>
                  </div>

                  <!-- Tabulation View -->
                  <div v-if="currentApproach === 'tabulation'" class="ll-dp-tab-view">
                    <div class="ll-section-caption">
                      <span>1D DP Table &mdash; <code>dp[0..{{ ((st.n || 1) - 1) }}]</code> (minimum jumps to each index)</span>
                      <span v-if="st.i >= 0 && st.j >= 0" class="ll-calc-pill">dp[{{ st.i }}] = min(dp[{{ st.i }}], dp[{{ st.j }}] + 1)</span>
                    </div>
                    <div class="ll-section-label">Input Array (arr[i] = max jump length):</div>
                    <div class="ll-arr-track">
                      <template v-for="(val, idx) in (st.arr || [])" :key="'inp-' + idx">
                        <div class="ll-arr-cell-wrap">
                          <div class="ll-ptr-tag-wrap">
                            <span v-if="idx === st.j" class="ll-ptr-lbl ll-lbl-orange">&#8595; j</span>
                            <span v-else-if="idx === st.i" class="ll-ptr-lbl ll-lbl-green">&#8595; i</span>
                          </div>
                          <div class="ll-arr-box ll-arr-box-sm"
                            :class="{ 'll-box-source': idx===st.j, 'll-box-active': idx===st.i && idx!==st.j, 'll-box-end': idx===(st.arr?st.arr.length-1:-1) && idx!==st.i && idx!==st.j }">
                            {{ val }}
                          </div>
                          <div class="ll-arr-idx">arr[{{ idx }}]</div>
                        </div>
                      </template>
                    </div>
                    <div class="ll-section-label" style="margin-top:8px">DP Table (dp[i] = min jumps to reach index i):</div>
                    <div class="ll-arr-track">
                      <template v-for="cell in (st.dpCells || [])" :key="cell.idx">
                        <div class="ll-arr-cell-wrap">
                          <div class="ll-ptr-tag-wrap">
                            <span v-if="cell.idx === st.j" class="ll-ptr-lbl ll-lbl-orange">&#8595; j</span>
                            <span v-else-if="cell.idx === st.i" class="ll-ptr-lbl ll-lbl-green">&#8595; i</span>
                          </div>
                          <div class="ll-arr-box"
                            :class="{ 'll-box-base': cell.state==='base', 'll-box-cur': cell.state==='adding', 'll-box-source': cell.state==='source', 'll-box-found': cell.state==='solved', 'll-box-uncalc': cell.state==='uncalc' }">
                            {{ cell.val >= 999999 ? '∞' : cell.val }}
                          </div>
                          <div class="ll-arr-idx">dp[{{ cell.idx }}]</div>
                        </div>
                      </template>
                    </div>
                  </div>

                  <!-- Recursion & Memoization View: Recursive Call Tree Diagram -->
                  <div v-else class="ll-tree-container">
                    <div class="ll-section-caption">
                      <span>{{ currentApproach === 'recursion' ? 'Recursive Call Tree (Exponential Search O(2ⁿ))' : 'Pruned Memoization Call Tree (Top-Down DP O(n²))' }}</span>
                      <span v-if="currentApproach === 'memoization'" class="ll-calc-pill" style="background:#f3e8ff;color:#6b21a8">Cache Table Active</span>
                      <span v-else-if="(st.redundantCalls || 0) > 0" class="ll-calc-pill" style="background:#fee2e2;color:#b91c1c">{{ st.redundantCalls }} Redundant Subtrees</span>
                    </div>

                    <!-- Compact Jump Array Preview Strip -->
                    <div class="ll-jump-preview-bar">
                      <div class="ll-mini-track">
                        <div v-for="(val, idx) in (st.arr || [])" :key="idx" class="ll-mini-cell-wrap">
                          <div class="ll-mini-ptr-wrap">
                            <span v-if="idx === st.activePos" class="ll-mini-ptr">&#8595; pos</span>
                          </div>
                          <div class="ll-mini-cell"
                            :class="{
                              'll-mini-active': idx === st.activePos,
                              'll-mini-end': idx === (st.arr ? st.arr.length - 1 : -1) && idx !== st.activePos,
                              'll-mini-reach': st.activePos !== undefined && idx > st.activePos && idx <= st.activePos + (st.arr ? st.arr[st.activePos] : 0) && idx !== st.activePos
                            }">
                            {{ val }}
                          </div>
                          <span class="ll-mini-idx">[{{ idx }}]</span>
                        </div>
                      </div>
                      <div class="ll-mini-info" v-if="st.arr && st.activePos !== undefined && st.arr[st.activePos] !== undefined">
                        <span v-if="st.arr[st.activePos] === 0" class="ll-jump-stuck-mini">🚫 arr[{{ st.activePos }}]=0 Stuck</span>
                        <span v-else class="ll-jump-info-mini">From <b>pos={{ st.activePos }}</b> can jump to: <span v-for="step in Math.min(st.arr[st.activePos], (st.arr.length - 1 - st.activePos))" :key="step" class="ll-mini-jump-pill">{{ st.activePos + step }}</span></span>
                      </div>
                    </div>

                    <!-- Scrollable Call Tree Area -->
                    <div class="ll-tree-scroll-area">
                      <svg
                        :viewBox="`0 0 ${st.treeWidth || 300} ${st.treeHeight || 160}`"
                        :style="{
                          width: '100%',
                          maxWidth: (st.treeWidth || 300) + 'px',
                          height: (st.treeHeight || 160) + 'px',
                          maxHeight: (st.treeHeight || 160) + 'px'
                        }"
                        class="ll-tree-svg"
                      >
                        <!-- Connector Edges -->
                        <g class="ll-tree-edges">
                          <line
                            v-for="(edge, idx) in (st.treeEdges || [])"
                            :key="idx"
                            :x1="edge.x1"
                            :y1="edge.y1"
                            :x2="edge.x2"
                            :y2="edge.y2"
                            class="ll-tree-edge"
                            :class="{
                              'll-edge-dashed': edge.isDashed
                            }"
                          />
                        </g>

                        <!-- Tree Node Badges -->
                        <g class="ll-tree-nodes">
                          <g
                            v-for="node in (st.treeNodes || [])"
                            :key="node.id"
                            :transform="`translate(${node.x}, ${node.y})`"
                            class="ll-tree-node-group"
                          >
                            <!-- Redundant Warning Badge Above Node -->
                            <g v-if="node.isRedundant" transform="translate(0, -17)">
                              <rect x="-24" y="-6" width="48" height="12" rx="3" fill="#ef4444" stroke="#dc2626" stroke-width="0.5" />
                              <text x="0" y="0" text-anchor="middle" class="ll-badge-text-red">Recomputing!</text>
                            </g>
                            <!-- Cache Hit Badge Above Node -->
                            <g v-else-if="node.isCacheHit || node.state === 'cachehit'" transform="translate(0, -17)">
                              <rect x="-24" y="-6" width="48" height="12" rx="3" fill="#9333ea" stroke="#7e22ce" stroke-width="0.5" />
                              <text x="0" y="0" text-anchor="middle" class="ll-badge-text-purple">Cache Hit O(1)</text>
                            </g>

                            <!-- Node Outer Card -->
                            <rect
                              x="-24"
                              y="-13"
                              width="48"
                              height="26"
                              rx="4"
                              class="ll-node-rect"
                              :class="{
                                'll-node-active': node.id === st.activeNodeId,
                                'll-node-solved': node.state === 'solved',
                                'll-node-redundant': node.isRedundant || node.state === 'redundant',
                                'll-node-cachehit': node.isCacheHit || node.state === 'cachehit'
                              }"
                            />
                            <!-- Node Text: mJ(pos) -->
                            <text x="0" y="-3" text-anchor="middle" class="ll-node-text-call">
                              mJ({{ node.pos }})
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
                                'll-val-hit': node.isCacheHit || node.state === 'cachehit'
                              }"
                            >
                              {{
                                node.returnVal !== null && node.returnVal !== undefined ? `= ${node.returnVal}` :
                                (node.isCacheHit || node.state === 'cachehit' ? '⚡ Hit' : '?')
                              }}
                            </text>
                          </g>
                        </g>
                      </svg>

                      <!-- Memoization Cache Table Strip -->
                      <div v-if="currentApproach === 'memoization' && st.memo" class="ll-memo-strip-wrap">
                        <div class="ll-memo-title">Memoization Cache Table &mdash; <code>memo[0..{{ (st.arr || []).length - 1 }}]</code>:</div>
                        <div class="ll-memo-strip">
                          <div v-for="(val, idx) in st.memo" :key="idx" class="ll-memo-cell-wrap">
                            <div class="ll-memo-cell" :class="{ 'll-memo-hit': val !== -1 && val !== null, 'll-memo-empty': val === -1 || val === null }">
                              {{ val === -1 || val === null ? '?' : (val === -2 || val >= INF ? '∞' : val) }}
                            </div>
                            <div class="ll-memo-idx">[{{ idx }}]</div>
                          </div>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>

              <div class="ll-vresizer" ref="vizResizerRef"></div>

              <!-- Legend -->
              <div class="ll-legend">
                <template v-if="currentApproach === 'tabulation'">
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-base"></span>Base dp[0]=0</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-purple"></span>Source j</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-cur"></span>Target i</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-new"></span>Finalized</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-uncalc"></span>Uncalculated</span>
                </template>
                <template v-else-if="currentApproach === 'memoization'">
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-cur"></span>Active pos</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-new"></span>Solved</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-purple"></span>Cache Hit O(1)</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-red"></span>End pos</span>
                </template>
                <template v-else>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-cur"></span>Active pos</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-new"></span>Solved</span>
                  <span class="ll-leg"><span class="ll-legdot" style="background:#fef2f2;border:1.5px dashed #ef4444"></span>Recomputing!</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-red"></span>End pos</span>
                </template>
              </div>

              <!-- Variable Frames -->
              <div class="ll-table-area" :style="{ height: tableHeight + 'px' }">
                <div class="ll-table-title">Variable frames &mdash; innermost = current</div>
                <div class="ll-stack-line">
                  <template v-if="s.vars && s.vars.length">
                    <div v-for="(f, depth) in s.vars" :key="depth" class="ll-frame"
                      :class="{ 'll-frame-cur': depth === s.vars.length - 1 }"
                      :style="{ marginLeft: depth * 12 + 'px' }">
                      {{ f.title }}(<span v-for="(r, i) in f.rows" :key="i">
                        <span v-if="i > 0">, </span>
                        <span class="ll-fname">{{ r[0] }}</span>=<span
                          :class="r[0]==='return'||r[0]==='result' ? 'll-c-green' : (depth===s.vars.length-1 ? 'll-c-orange' : 'll-c-blue')"
                          style="font-weight:700">{{ r[1] }}</span>
                      </span>)<span v-if="depth === s.vars.length - 1" class="ll-now"> &#9668; current</span>
                    </div>
                  </template>
                  <template v-else>&mdash;</template>
                </div>
              </div>

              <div class="ll-vresizer" ref="tableResizerRef"></div>

              <!-- Step Badge -->
              <div class="ll-badge-wrap">
                <div class="ll-badge"
                  :class="{ 'll-badge-error': s.badge && (s.badge.includes('Overlapping') || s.badge.includes('Stuck') || s.badge.includes('🚫')), 'll-badge-success': s.badge && (s.badge.includes('✅') || s.badge.includes('Complete')) }">
                  {{ s.badge }}
                </div>
              </div>
            </div>

            <!-- Horizontal Resizer -->
            <div class="ll-resizer" ref="hResizerRef"></div>

            <!-- Right Column -->
            <div class="ll-right-col">
              <div class="ll-code-panel">
                <div class="ll-code-header">
                  <div class="ll-tabbar">
                    <button class="ll-tab-btn" :class="{ active: rightTab==='code' }" @click="rightTab='code'">Code</button>
                    <button class="ll-tab-btn" :class="{ active: rightTab==='pseudo' }" @click="rightTab='pseudo'">Pseudocode</button>
                    <button class="ll-tab-btn" :class="{ active: rightTab==='complexity' }" @click="rightTab='complexity'">Complexity</button>
                  </div>
                  <select v-if="rightTab==='code'" v-model="lang" class="ll-lang-select">
                    <option value="java">Java</option>
                    <option value="c">C</option>
                    <option value="cpp">C++</option>
                    <option value="python">Python</option>
                    <option value="javascript">JavaScript</option>
                  </select>
                </div>

                <div v-if="rightTab==='code'" class="ll-code-scroll">
                  <pre class="ll-pre"><span v-for="(line, i) in codeLines" :key="i" class="ll-codeline" :class="{ 'll-hl': isLineActive(line) }">{{ line[1]==='' ? ' ' : line[1] }}</span></pre>
                </div>

                <div v-else-if="rightTab==='pseudo'" class="ll-code-scroll">
                  <pre class="ll-pre"><span v-for="(line, i) in pseudocodeLines" :key="i" class="ll-codeline">{{ line }}</span></pre>
                </div>

                <div v-else class="ll-info-scroll">
                  <h3>Problem Statement</h3>
                  <p>Given an array <code>arr[]</code> of non-negative integers where <code>arr[i]</code> is the <strong>maximum jump length</strong> from index <code>i</code>, find the <strong>minimum number of jumps</strong> to reach the last index from index 0.</p>
                  <p>Example: <code>[2, 3, 1, 1, 4]</code> → Min jumps = <strong>2</strong> &nbsp;(path: 0 → 1 → 4)</p>

                  <h3>Why Dynamic Programming?</h3>
                  <ul>
                    <li><strong>1. Optimal Substructure:</strong> min jumps to reach index <code>i</code> = min over all reachable predecessors <code>j</code> of <code>dp[j] + 1</code>.</li>
                    <li><strong>2. Overlapping Subproblems:</strong> Brute force explores the same positions many times. DP stores each answer once.</li>
                  </ul>

                  <h3>Approaches Comparison</h3>
                  <table class="ll-complexity-table">
                    <thead><tr><th>Approach</th><th>Time</th><th>Space</th><th>Repeated Work</th></tr></thead>
                    <tbody>
                      <tr><td><strong>Brute Force</strong></td><td>Exponential</td><td>O(n) Stack</td><td><span class="ll-c-red" style="font-weight:700">YES</span></td></tr>
                      <tr><td><strong>DP Memoization</strong></td><td>O(n²)</td><td>O(n) Cache+Stack</td><td><span class="ll-c-green" style="font-weight:700">NO</span></td></tr>
                      <tr><td><strong>DP Tabulation</strong></td><td>O(n²)</td><td>O(n) Table</td><td><span class="ll-c-green" style="font-weight:700">NO</span></td></tr>
                      <tr><td><strong>Greedy (BFS)</strong></td><td>O(n)</td><td>O(1)</td><td><span class="ll-c-green" style="font-weight:700">NO</span></td></tr>
                    </tbody>
                  </table>

                  <h3>DP Recurrence</h3>
                  <p class="ll-math-box">dp[i] = min( dp[j] + 1 ) &nbsp; for all j &lt; i where dp[j] &ne; INF and j + arr[j] &ge; i</p>
                  <p>Base: <code>dp[0] = 0</code>. Answer: <code>dp[n-1]</code> (or -1 if still INF).</p>

                  <div class="ll-note">
                    <strong>Key Takeaway:</strong> Tabulation fills dp bottom-up iteratively, avoiding all recursive overhead. For each target index <code>i</code>, we scan all <code>j &lt; i</code> that can reach it in one jump and pick the cheapest path — a canonical O(n²) DP pattern.
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

          <!-- Warning Modal -->
          <transition name="ll-modal-fade">
            <div v-if="showWarningModal" class="ll-modal-backdrop" @click.self="closeWarningModal">
              <div class="ll-modal-card" @click.stop>
                <div class="ll-modal-header">
                  <div class="ll-modal-title-wrap">
                    <div class="ll-modal-icon-badge">
                      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" class="ll-modal-svg-icon">
                        <path d="M10.29 3.86L1.82 18a2 2 0 0 0 1.71 3h16.94a2 2 0 0 0 1.71-3L13.71 3.86a2 2 0 0 0-3.42 0z"/>
                        <line x1="12" y1="9" x2="12" y2="13"/><line x1="12" y1="17" x2="12.01" y2="17"/>
                      </svg>
                    </div>
                    <span class="ll-modal-title">{{ warningModalTitle }}</span>
                  </div>
                  <button class="ll-modal-close-btn" @click="closeWarningModal">&times;</button>
                </div>
                <div class="ll-modal-body">
                  <div class="ll-modal-badge-row">
                    <span class="ll-modal-tag-entered">Entered: {{ requestedValue }}</span>
                    <span class="ll-modal-arrow">&rarr;</span>
                    <span class="ll-modal-tag-applied">Clamped to: {{ warningModalLimit }} elements</span>
                  </div>
                  <p class="ll-modal-message">{{ warningModalMsg }}</p>
                </div>
                <div class="ll-modal-footer">
                  <button class="ll-modal-confirm-btn" @click="closeWarningModal">Got it, Proceed &#10003;</button>
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
.ll-root *, .ll-root, .row-main { scrollbar-width: none !important; -ms-overflow-style: none !important; }
.ll-root *::-webkit-scrollbar, .ll-root::-webkit-scrollbar, .row-main::-webkit-scrollbar { display: none !important; width: 0 !important; height: 0 !important; }
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
  display: flex; flex-direction: column; height: 50vh; min-height: 600px; overflow: hidden; width: 100%;
}

@keyframes ll-pop { from { transform: scale(.85); opacity: 0; } to { transform: scale(1); opacity: 1; } }
@keyframes ll-pulse { 0%, 100% { transform: scale(1); } 50% { transform: scale(1.05); } }

.slide-wrapper { margin-top: -10px; margin-left: -30px; width: 107%; max-height: 100%; font-size: 0.8rem; font-weight: 400; }
.slide-body { display: flex; flex-direction: column; border-radius: 4px; height: 100%; }
.navbar { display: flex; flex-direction: row; justify-content: space-between; align-items: center; gap: 0.75rem; padding: 0 10px; background-color: #ffffff; position: fixed; width: 94.7%; }
.navbar > img { height: 30px; }
.navbar-title { margin: 0; font-size: 1.35rem; font-weight: 700; background-color: #ef5050; color: #ffffff; width: 80%; padding: 2px 10px; margin-left: -10px; border-radius: 5px; }
.row-main { width: 100%; height: 90%; margin-top: 36px; overflow-x: auto; overflow-y: auto; }

.ll-toolbar { margin-top: 4px; display: flex; align-items: center; gap: 6px; padding: 5px 12px; background: var(--surface); border-bottom: 1px solid var(--border); flex-shrink: 0; flex-wrap: wrap; box-shadow: var(--shadow-sm); }
.ll-approach-group { display: flex; gap: 2px; background: var(--surface2); padding: 2px; border-radius: var(--radius-sm); border: 1px solid var(--border); }
.ll-approach-btn { background: transparent; border: none; padding: 4px 8px; font-size: 11px; font-weight: 600; color: var(--text2); border-radius: 4px; cursor: pointer; transition: all .15s; white-space: nowrap; }
.ll-approach-btn:hover { color: var(--coral); }
.ll-approach-btn.active { background: var(--coral); color: #fff; box-shadow: var(--shadow-sm); }

.ll-input-group { display: flex; align-items: center; gap: 4px; }
.ll-input-group label { font-size: 11.5px; color: var(--muted); font-weight: 700; }
.ll-arr-input { background: var(--surface); border: 1px solid var(--border2); color: var(--text); border-radius: var(--radius-sm); padding: 3px 6px; font-size: 11px; font-family: monospace; width: 130px; }
.ll-arr-input:focus { outline: none; border-color: var(--coral); box-shadow: 0 0 0 3px rgba(240,77,77,.1); }
.ll-input-hint { font-size: 10px; color: var(--muted); }
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
.ll-right-col { display: flex; flex-direction: column; flex: 1; overflow: hidden; min-width: 0; height: 78%; }

.ll-viz-wrap { flex-shrink: 0; background: var(--surface); border-bottom: 1px solid var(--border); position: relative; overflow-x: auto; overflow-y: auto; }
.ll-perm-area { display: flex; flex-direction: column; align-items: stretch; min-height: 100%; width: 100%; min-width: 0; box-sizing: border-box; }
.ll-ptrs { display: flex; gap: 6px; flex-wrap: wrap; padding: 4px 10px; min-height: 28px; width: 100%; box-sizing: border-box; min-width: 0; }
.ll-ptr-chip { background: var(--surface2); border: 1px solid var(--border); border-radius: var(--radius-sm); padding: 2px 7px; font-size: 11px; font-family: monospace; box-shadow: var(--shadow-sm); white-space: nowrap; flex-shrink: 0; }
.ll-chip-warn { background: #fef2f2 !important; border-color: #fca5a5 !important; }
.ll-c-blue { color: var(--blue); } .ll-c-orange { color: var(--orange); } .ll-c-green { color: var(--green); } .ll-c-purple { color: var(--purple); } .ll-c-red { color: var(--red); }

.ll-section-caption { display: flex; justify-content: space-between; align-items: center; padding: 3px 10px; font-size: 10.5px; font-weight: 700; color: var(--text2); background: var(--surface2); border-top: 1px solid var(--border); border-bottom: 1px solid var(--border); }
.ll-section-label { font-size: 10px; font-weight: 700; color: var(--text2); padding: 2px 4px 0; }
.ll-calc-pill { background: #dcfce7; color: #15803d; padding: 1px 6px; border-radius: 10px; font-size: 10.5px; font-family: monospace; font-weight: 700; }

/* Diagram 2: Recursion & Memoization Call Tree (Fixed 1:1 Pixel Scale) */
.ll-tree-container { display: flex; flex-direction: column; width: 100%; height: 100%; min-height: 0; }
.ll-tree-scroll-area { flex: 1; overflow: auto; padding: 4px 8px; display: flex; flex-direction: column; align-items: center; justify-content: flex-start; min-height: 0; width: 100%; box-sizing: border-box; }
.ll-tree-svg { display: block; flex-shrink: 0; max-width: 100%; margin: 0 auto; }
.ll-tree-edge { stroke: #cbd5e1; stroke-width: 1.8px; transition: stroke .2s; }
.ll-edge-dashed { stroke: #a855f7 !important; stroke-dasharray: 4, 3 !important; }

.ll-tree-node-group { cursor: default; }
.ll-node-rect { fill: #ffffff; stroke: #cbd5e1; stroke-width: 1.4px; filter: drop-shadow(0 1px 2px rgba(0,0,0,0.05)); transition: all .25s ease; }
.ll-node-active { stroke: #f59e0b !important; stroke-width: 2.2px !important; fill: #fffbeb !important; filter: drop-shadow(0 0 5px rgba(245, 158, 11, 0.45)) !important; animation: ll-pulse 1.3s infinite ease-in-out; }
.ll-node-solved { stroke: #10b981 !important; fill: #dcfce7 !important; stroke-width: 1.6px !important; }
.ll-node-redundant { stroke: #ef4444 !important; stroke-dasharray: 3, 2 !important; fill: #fef2f2 !important; }
.ll-node-cachehit { stroke: #9333ea !important; stroke-width: 1.8px !important; fill: #f3e8ff !important; }

/* SVG Text styling */
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

/* Compact Jump Array Preview Strip */
.ll-jump-preview-bar { display: flex; align-items: center; justify-content: space-between; gap: 8px; padding: 3px 10px; background: var(--surface2); border-bottom: 1px solid var(--border); flex-wrap: wrap; flex-shrink: 0; }
.ll-mini-track { display: flex; align-items: flex-end; gap: 4px; }
.ll-mini-cell-wrap { display: flex; flex-direction: column; align-items: center; }
.ll-mini-ptr-wrap { height: 11px; display: flex; align-items: center; justify-content: center; }
.ll-mini-ptr { font-size: 9px; font-weight: 800; color: #10b981; line-height: 1; }
.ll-mini-cell { width: 26px; height: 22px; display: flex; align-items: center; justify-content: center; font-size: 11px; font-weight: 700; border-radius: 4px; border: 1.5px solid var(--border2); background: var(--surface); color: var(--text); }
.ll-mini-active { border-color: #f59e0b !important; background: #fef3c7 !important; color: #92400e !important; box-shadow: 0 0 0 2px rgba(245,158,11,0.2) !important; font-weight: 800; }
.ll-mini-end { border-color: #ef4444 !important; background: #fef2f2 !important; color: #991b1b !important; }
.ll-mini-reach { border-color: #f97316 !important; background: #fff7ed !important; color: #92400e !important; }
.ll-mini-idx { font-size: 8.5px; color: var(--muted); font-family: monospace; }
.ll-mini-info { font-size: 10.5px; color: var(--text2); display: flex; align-items: center; gap: 4px; }
.ll-jump-info-mini { font-size: 10.5px; color: var(--text2); }
.ll-mini-jump-pill { background: var(--orange-light); color: var(--orange); border: 1px solid #fdba74; border-radius: 8px; padding: 0 5px; font-size: 10px; font-weight: 700; font-family: monospace; margin-left: 2px; }
.ll-jump-stuck-mini { background: var(--red-light); color: var(--red-dark); border: 1px solid #fca5a5; border-radius: 8px; padding: 0 6px; font-size: 10px; font-weight: 700; }

.ll-dp-tab-view { display: flex; flex-direction: column; padding: 6px 10px; }

.ll-arr-track { display: flex; align-items: flex-start; flex-wrap: wrap; padding: 6px 4px; gap: 8px; width: 100%; box-sizing: border-box; min-width: 0; }
.ll-arr-cell-wrap { display: flex; flex-direction: column; align-items: center; min-width: 0; }
.ll-ptr-tag-wrap { height: 22px; display: flex; align-items: flex-end; justify-content: center; margin-bottom: 2px; }
.ll-ptr-lbl { font-size: 10.5px; font-weight: 800; font-family: 'Consolas', 'Fira Code', monospace; display: inline-flex; align-items: center; line-height: 1; white-space: nowrap; animation: ll-pop 0.2s ease; }
.ll-lbl-orange { color: #f97316; } .ll-lbl-purple { color: #9333ea; } .ll-lbl-green { color: #10b981; }

.ll-arr-box { width: 46px; height: 46px; display: flex; align-items: center; justify-content: center; border: 2px solid var(--border2); border-radius: var(--radius); background: var(--surface); color: var(--text); font-weight: 700; font-size: 14px; box-shadow: var(--shadow-sm); transition: all 0.25s ease; animation: ll-pop .3s ease; }
.ll-arr-box-sm { width: 40px; height: 40px; font-size: 12px; }
.ll-box-base     { border-color: #3b82f6 !important; background: #eff6ff !important; color: #1d4ed8 !important; }
.ll-box-cur      { border-color: #f59e0b !important; background: #fef3c7 !important; color: #92400e !important; box-shadow: 0 0 0 3px rgba(245,158,11,0.25) !important; transform: translateY(-2px); }
.ll-box-source   { border-color: #9333ea !important; background: #f3e8ff !important; color: #6b21a8 !important; box-shadow: 0 0 0 3px rgba(147,51,234,0.2) !important; transform: translateY(-2px); }
.ll-box-active   { border-color: #f59e0b !important; background: #fef3c7 !important; color: #92400e !important; box-shadow: 0 0 0 3px rgba(245,158,11,0.25) !important; transform: translateY(-2px); animation: ll-pulse 1.3s infinite ease-in-out; }
.ll-box-found    { border-color: #10b981 !important; background: #dcfce7 !important; color: #065f46 !important; box-shadow: 0 0 0 3px rgba(16,185,129,0.2) !important; }
.ll-box-end      { border-color: #ef4444 !important; background: #fef2f2 !important; color: #991b1b !important; }
.ll-box-reachable { border-color: #f97316 !important; background: #fff7ed !important; color: #92400e !important; }
.ll-box-uncalc   { border: 2px dashed var(--border2) !important; background: var(--surface2) !important; color: var(--muted) !important; }
.ll-arr-idx { font-size: 10px; color: var(--muted); margin-top: 3px; font-family: 'Consolas', monospace; font-weight: 600; }

.ll-memo-strip-wrap { width: 100%; padding: 4px 8px 6px; border-top: 1px dashed var(--border); background: var(--surface2); flex-shrink: 0; margin-top: 4px; box-sizing: border-box; }
.ll-memo-title { font-size: 10px; font-weight: 700; color: var(--text2); margin-bottom: 2px; }
.ll-memo-strip { display: flex; gap: 5px; flex-wrap: wrap; }
.ll-memo-cell-wrap { display: flex; flex-direction: column; align-items: center; }
.ll-memo-cell { width: 30px; height: 26px; display: flex; align-items: center; justify-content: center; font-size: 11px; font-family: monospace; font-weight: 700; border-radius: 4px; border: 1px solid var(--border); background: var(--surface); color: var(--text); }
.ll-memo-hit { background: #f3e8ff !important; border-color: #a855f7 !important; color: #6b21a8 !important; }
.ll-memo-empty { color: var(--muted); border-style: dashed; }
.ll-memo-idx { font-size: 9px; color: var(--muted); font-family: monospace; }

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

.ll-table-area { flex-shrink: 0; padding: 4px 10px; border-bottom: 1px solid var(--border); overflow-x: hidden; overflow-y: auto; background: var(--surface); min-width: 0; box-sizing: border-box; }
.ll-table-title { font-size: 9.5px; color: var(--muted); margin-bottom: 2px; font-style: italic; }
.ll-stack-line { font-family: 'Consolas', monospace; font-size: 11px; line-height: 1.6; }
.ll-frame { font-family: 'Consolas', monospace; font-size: 10.5px; color: var(--text2); padding: 1px 0; white-space: nowrap; }
.ll-frame-cur { color: var(--orange); background: var(--orange-light); border-radius: 4px; padding: 1px 4px; }
.ll-fname { color: var(--text2); }
.ll-now { color: var(--orange); font-size: 9.5px; margin-left: 4px; }

.ll-badge-wrap { padding: 4px 8px; border-bottom: 1px solid var(--border); flex-shrink: 0; min-height: 30px; display: flex; align-items: center; background: var(--surface); }
.ll-badge { display: inline-block; padding: 3px 10px; border-radius: var(--radius-sm); border-left: 3px solid var(--coral); background: var(--coral-light); font-size: 10.5px; color: var(--coral-dark); line-height: 1.35; word-break: break-word; font-weight: 500; }
.ll-badge-error { border-left-color: var(--red); background: var(--red-light); color: var(--red-dark); font-weight: 600; }
.ll-badge-success { border-left-color: var(--green); background: var(--green-light); color: #166534; font-weight: 600; }

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

.ll-info-scroll { flex: 1; overflow: auto; padding: 12px 16px; background: var(--surface); color: var(--text2); font-size: 12px; padding-bottom:100px;}
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

.ll-footer { padding: 3px 12px; font-size: 10.5px; color: var(--muted); border-top: 1px solid var(--border); background: var(--surface); flex-shrink: 0; display: flex; align-items: center; }
.ll-speed-wrap { display: flex; align-items: center; gap: 4px; margin-left: 14px; }
.ll-speed-wrap input[type=range] { width: 80px; accent-color: var(--coral); }

.ll-modal-backdrop { position: fixed; inset: 0; background: rgba(15,23,42,0.45); display: flex; align-items: center; justify-content: center; z-index: 9999; backdrop-filter: blur(2px); }
.ll-modal-card { background: var(--surface); border-radius: 12px; box-shadow: 0 20px 60px rgba(0,0,0,.25); width: 380px; max-width: 95vw; overflow: hidden; animation: ll-pop .25s ease; }
.ll-modal-header { display: flex; align-items: center; justify-content: space-between; padding: 14px 18px 10px; border-bottom: 1px solid var(--border); }
.ll-modal-title-wrap { display: flex; align-items: center; gap: 8px; }
.ll-modal-icon-badge { width: 28px; height: 28px; background: #fef3c7; border-radius: 8px; display: flex; align-items: center; justify-content: center; flex-shrink: 0; }
.ll-modal-svg-icon { width: 16px; height: 16px; stroke: #d97706; }
.ll-modal-title { font-size: 13px; font-weight: 700; color: var(--text); }
.ll-modal-close-btn { background: none; border: none; font-size: 18px; color: var(--muted); cursor: pointer; padding: 0 4px; line-height: 1; transition: color .15s; }
.ll-modal-close-btn:hover { color: var(--red); }
.ll-modal-body { padding: 14px 18px; }
.ll-modal-badge-row { display: flex; align-items: center; gap: 8px; margin-bottom: 10px; flex-wrap: wrap; }
.ll-modal-tag-entered { background: var(--red-light); color: var(--red-dark); border: 1px solid #fca5a5; border-radius: var(--radius-sm); padding: 3px 8px; font-size: 11px; font-family: monospace; font-weight: 700; }
.ll-modal-arrow { color: var(--muted); font-size: 16px; }
.ll-modal-tag-applied { background: #fef3c7; color: #92400e; border: 1px solid #fcd34d; border-radius: var(--radius-sm); padding: 3px 8px; font-size: 11px; font-family: monospace; font-weight: 700; }
.ll-modal-message { font-size: 11.5px; color: var(--text2); line-height: 1.5; margin: 0; }
.ll-modal-footer { padding: 10px 18px 14px; display: flex; justify-content: flex-end; border-top: 1px solid var(--border); }
.ll-modal-confirm-btn { background: var(--coral); color: #fff; border: none; padding: 7px 18px; border-radius: var(--radius-sm); cursor: pointer; font-size: 12px; font-weight: 700; transition: filter .15s; box-shadow: var(--shadow-sm); }
.ll-modal-confirm-btn:hover { filter: brightness(1.08); }
.ll-modal-fade-enter-active, .ll-modal-fade-leave-active { transition: opacity .2s ease; }
.ll-modal-fade-enter-from, .ll-modal-fade-leave-to { opacity: 0; }
</style>

<style>
/* Global scrollbar styling for slide presentation */
.ll-tree-scroll-area::-webkit-scrollbar,
.ll-code-scroll::-webkit-scrollbar,
.ll-table-scroll::-webkit-scrollbar,
.ll-info-scroll::-webkit-scrollbar {
  width: 4px;
  height: 4px;
}
.ll-tree-scroll-area::-webkit-scrollbar-thumb,
.ll-code-scroll::-webkit-scrollbar-thumb,
.ll-table-scroll::-webkit-scrollbar-thumb,
.ll-info-scroll::-webkit-scrollbar-thumb {
  background: #cbd5e1;
  border-radius: 4px;
}
</style>
