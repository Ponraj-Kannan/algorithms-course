<script setup>
import { ref, computed, reactive, onMounted, onUnmounted, watch } from 'vue';

/* ==================================================================== */
/* PROPS & COMPONENT CONFIGURATION                                      */
/* ==================================================================== */
const props = defineProps({
  topic: { type: String, default: 'Dynamic Programming' },
  subTopic: { type: String, default: 'Longest Increasing Subsequence (LIS)' }
});

/* ==================================================================== */
/* APPROACHES & MULTI-LANGUAGE 100% EXECUTABLE CODE SPECIFICATIONS      */
/* ==================================================================== */
const APPROACHES = [
  { id: 'recursion', label: 'Brute Force', desc: 'O(2^n) Exponential — Recursive Include/Exclude Decision Tree' },
  { id: 'memoization', label: 'Memoization', desc: 'O(n^2) Time & O(n^2) Space — Top-Down with 2D State Cache' },
  { id: 'tabulation', label: 'Tabulation', desc: 'O(n^2) Time & O(n) Space — Bottom-Up 1D DP Array with Traceback' }
];

const CODES = {
  recursion: {
    java: [
      ['',             'import java.util.Scanner;'],
      ['',             ''],
      ['',             'public class Main {'],
      ['c_entry',      '    static int lis(int[] arr, int i, int prevIdx) {'],
      ['c_base',       '        if (i == arr.length) {'],
      ['c_base_ret',   '            return 0;'],
      ['',             '        }'],
      ['c_init_take',  '        int take = 0;'],
      ['c_take_check', '        if (prevIdx == -1 || arr[i] > arr[prevIdx]) {'],
      ['c_take_rec',   '            take = 1 + lis(arr, i + 1, i);'],
      ['',             '        }'],
      ['c_nottake_rec','        int notTake = lis(arr, i + 1, prevIdx);'],
      ['c_ret_max',    '        return Math.max(take, notTake);'],
      ['',             '    }'],
      ['',             ''],
      ['',             '    public static void main(String[] args) {'],
      ['m_scanner',    '        Scanner sc = new Scanner(System.in);'],
      ['m_read_arr',   '        int n = sc.nextInt();'],
      ['',             '        int[] arr = new int[n];'],
      ['',             '        for (int i = 0; i < n; i++) {'],
      ['',             '            arr[i] = sc.nextInt();'],
      ['',             '        }'],
      ['m_call_lis',   '        int ans = lis(arr, 0, -1);'],
      ['m_print',      '        System.out.println(ans);'],
      ['m_done',       '    }'],
      ['',             '}']
    ],
    c: [
      ['',             '#include <stdio.h>'],
      ['',             '#define MAX(a, b) ((a) > (b) ? (a) : (b))'],
      ['',             ''],
      ['c_entry',      'int lis(int arr[], int n, int i, int prevIdx) {'],
      ['c_base',       '    if (i == n) {'],
      ['c_base_ret',   '        return 0;'],
      ['',             '    }'],
      ['c_init_take',  '    int take = 0;'],
      ['c_take_check', '    if (prevIdx == -1 || arr[i] > arr[prevIdx]) {'],
      ['c_take_rec',   '        take = 1 + lis(arr, n, i + 1, i);'],
      ['',             '    }'],
      ['c_nottake_rec','    int notTake = lis(arr, n, i + 1, prevIdx);'],
      ['c_ret_max',    '    return MAX(take, notTake);'],
      ['',             '}'],
      ['',             ''],
      ['',             'int main() {'],
      ['m_read_arr',   '    int n;'],
      ['',             '    if (scanf("%d", &n) != 1) {'],
      ['',             '        return 0;'],
      ['',             '    }'],
      ['',             '    int arr[n];'],
      ['',             '    for (int i = 0; i < n; i++) {'],
      ['',             '        scanf("%d", &arr[i]);'],
      ['',             '    }'],
      ['m_call_lis',   '    int ans = lis(arr, n, 0, -1);'],
      ['m_print',      '    printf("%d\\n", ans);'],
      ['m_done',       '    return 0;'],
      ['',             '}']
    ],
    cpp: [
      ['',             '#include <iostream>'],
      ['',             '#include <vector>'],
      ['',             '#include <algorithm>'],
      ['',             'using namespace std;'],
      ['',             ''],
      ['c_entry',      'int lis(const vector<int>& arr, int i, int prevIdx) {'],
      ['c_base',       '    if (i == (int)arr.size()) {'],
      ['c_base_ret',   '        return 0;'],
      ['',             '    }'],
      ['c_init_take',  '    int take = 0;'],
      ['c_take_check', '    if (prevIdx == -1 || arr[i] > arr[prevIdx]) {'],
      ['c_take_rec',   '        take = 1 + lis(arr, i + 1, i);'],
      ['',             '    }'],
      ['c_nottake_rec','    int notTake = lis(arr, i + 1, prevIdx);'],
      ['c_ret_max',    '    return max(take, notTake);'],
      ['',             '}'],
      ['',             ''],
      ['',             'int main() {'],
      ['m_read_arr',   '    int n;'],
      ['',             '    if (!(cin >> n)) {'],
      ['',             '        return 0;'],
      ['',             '    }'],
      ['',             '    vector<int> arr(n);'],
      ['',             '    for (int i = 0; i < n; i++) {'],
      ['',             '        cin >> arr[i];'],
      ['',             '    }'],
      ['m_call_lis',   '    int ans = lis(arr, 0, -1);'],
      ['m_print',      '    cout << ans << endl;'],
      ['m_done',       '    return 0;'],
      ['',             '}']
    ],
    python: [
      ['',             'import sys'],
      ['',             ''],
      ['c_entry',      'def lis(arr, i, prev_idx):'],
      ['c_base',       '    if i == len(arr):'],
      ['c_base_ret',   '        return 0'],
      ['c_init_take',  '    take = 0'],
      ['c_take_check', '    if prev_idx == -1 or arr[i] > arr[prev_idx]:'],
      ['c_take_rec',   '        take = 1 + lis(arr, i + 1, i)'],
      ['c_nottake_rec','    not_take = lis(arr, i + 1, prev_idx)'],
      ['c_ret_max',    '    return max(take, not_take)'],
      ['',             ''],
      ['',             'if __name__ == "__main__":'],
      ['m_read_arr',   '    lines = sys.stdin.read().split()'],
      ['',             '    if not lines:'],
      ['',             '        sys.exit(0)'],
      ['',             '    n = int(lines[0])'],
      ['',             '    arr = [int(x) for x in lines[1:n + 1]]'],
      ['m_call_lis',   '    ans = lis(arr, 0, -1)'],
      ['m_print',      '    print(ans)'],
      ['m_done',       '    sys.exit(0)']
    ],
    javascript: [
      ['',             'const fs = require("fs");'],
      ['',             ''],
      ['c_entry',      'function lis(arr, i, prevIdx) {'],
      ['c_base',       '    if (i === arr.length) {'],
      ['c_base_ret',   '        return 0;'],
      ['',             '    }'],
      ['c_init_take',  '    let take = 0;'],
      ['c_take_check', '    if (prevIdx === -1 || arr[i] > arr[prevIdx]) {'],
      ['c_take_rec',   '        take = 1 + lis(arr, i + 1, i);'],
      ['',             '    }'],
      ['c_nottake_rec','    const notTake = lis(arr, i + 1, prevIdx);'],
      ['c_ret_max',    '    return Math.max(take, notTake);'],
      ['',             '}'],
      ['',             ''],
      ['m_read_arr',   'const input = fs.readFileSync(0, "utf-8").trim().split(/\\s+/);'],
      ['',             'const n = parseInt(input[0], 10);'],
      ['',             'const arr = input.slice(1, n + 1).map(Number);'],
      ['m_call_lis',   'const ans = lis(arr, 0, -1);'],
      ['m_print',      'console.log(ans);'],
      ['m_done',       'process.exit(0);']
    ]
  },
  memoization: {
    java: [
      ['',             'import java.util.Scanner;'],
      ['',             'import java.util.Arrays;'],
      ['',             ''],
      ['',             'public class Main {'],
      ['c_entry',      '    static int solve(int[] arr, int i, int prevIdx, int[][] memo) {'],
      ['c_base',       '        if (i == arr.length) {'],
      ['c_base_ret',   '            return 0;'],
      ['',             '        }'],
      ['c_memo_check', '        if (memo[i][prevIdx + 1] != -1) {'],
      ['c_memo_ret',   '            return memo[i][prevIdx + 1];'],
      ['',             '        }'],
      ['c_init_take',  '        int take = 0;'],
      ['c_take_check', '        if (prevIdx == -1 || arr[i] > arr[prevIdx]) {'],
      ['c_take_rec',   '            take = 1 + solve(arr, i + 1, i, memo);'],
      ['',             '        }'],
      ['c_nottake_rec','        int notTake = solve(arr, i + 1, prevIdx, memo);'],
      ['c_store_memo', '        memo[i][prevIdx + 1] = Math.max(take, notTake);'],
      ['c_ret_memo',   '        return memo[i][prevIdx + 1];'],
      ['',             '    }'],
      ['',             ''],
      ['',             '    public static void main(String[] args) {'],
      ['m_scanner',    '        Scanner sc = new Scanner(System.in);'],
      ['m_read_arr',   '        int n = sc.nextInt();'],
      ['',             '        int[] arr = new int[n];'],
      ['',             '        for (int i = 0; i < n; i++) {'],
      ['',             '            arr[i] = sc.nextInt();'],
      ['',             '        }'],
      ['m_alloc_memo', '        int[][] memo = new int[n + 1][n + 1];'],
      ['m_fill_memo',  '        for (int[] row : memo) {'],
      ['',             '            Arrays.fill(row, -1);'],
      ['',             '        }'],
      ['m_call_lis',   '        int ans = solve(arr, 0, -1, memo);'],
      ['m_print',      '        System.out.println(ans);'],
      ['m_done',       '    }'],
      ['',             '}']
    ],
    c: [
      ['',             '#include <stdio.h>'],
      ['',             '#include <string.h>'],
      ['',             '#define MAX(a, b) ((a) > (b) ? (a) : (b))'],
      ['',             ''],
      ['c_entry',      'int solve(int arr[], int n, int i, int prevIdx, int memo[n + 1][n + 1]) {'],
      ['c_base',       '    if (i == n) {'],
      ['c_base_ret',   '        return 0;'],
      ['',             '    }'],
      ['c_memo_check', '    if (memo[i][prevIdx + 1] != -1) {'],
      ['c_memo_ret',   '        return memo[i][prevIdx + 1];'],
      ['',             '    }'],
      ['c_init_take',  '    int take = 0;'],
      ['c_take_check', '    if (prevIdx == -1 || arr[i] > arr[prevIdx]) {'],
      ['c_take_rec',   '        take = 1 + solve(arr, n, i + 1, i, memo);'],
      ['',             '    }'],
      ['c_nottake_rec','    int notTake = solve(arr, n, i + 1, prevIdx, memo);'],
      ['c_store_memo', '    memo[i][prevIdx + 1] = MAX(take, notTake);'],
      ['c_ret_memo',   '    return memo[i][prevIdx + 1];'],
      ['',             '}'],
      ['',             ''],
      ['',             'int main() {'],
      ['m_read_arr',   '    int n;'],
      ['',             '    if (scanf("%d", &n) != 1) {'],
      ['',             '        return 0;'],
      ['',             '    }'],
      ['',             '    int arr[n];'],
      ['',             '    for (int i = 0; i < n; i++) {'],
      ['',             '        scanf("%d", &arr[i]);'],
      ['',             '    }'],
      ['m_alloc_memo', '    int memo[n + 1][n + 1];'],
      ['m_fill_memo',  '    memset(memo, -1, sizeof(memo));'],
      ['m_call_lis',   '    int ans = solve(arr, n, 0, -1, memo);'],
      ['m_print',      '    printf("%d\\n", ans);'],
      ['m_done',       '    return 0;'],
      ['',             '}']
    ],
    cpp: [
      ['',             '#include <iostream>'],
      ['',             '#include <vector>'],
      ['',             '#include <algorithm>'],
      ['',             'using namespace std;'],
      ['',             ''],
      ['c_entry',      'int solve(const vector<int>& arr, int i, int prevIdx, vector<vector<int>>& memo) {'],
      ['c_base',       '    if (i == (int)arr.size()) {'],
      ['c_base_ret',   '        return 0;'],
      ['',             '    }'],
      ['c_memo_check', '    if (memo[i][prevIdx + 1] != -1) {'],
      ['c_memo_ret',   '        return memo[i][prevIdx + 1];'],
      ['',             '    }'],
      ['c_init_take',  '    int take = 0;'],
      ['c_take_check', '    if (prevIdx == -1 || arr[i] > arr[prevIdx]) {'],
      ['c_take_rec',   '        take = 1 + solve(arr, i + 1, i, memo);'],
      ['',             '    }'],
      ['c_nottake_rec','    int notTake = solve(arr, i + 1, prevIdx, memo);'],
      ['c_store_memo', '    memo[i][prevIdx + 1] = max(take, notTake);'],
      ['c_ret_memo',   '    return memo[i][prevIdx + 1];'],
      ['',             '}'],
      ['',             ''],
      ['',             'int main() {'],
      ['m_read_arr',   '    int n;'],
      ['',             '    if (!(cin >> n)) {'],
      ['',             '        return 0;'],
      ['',             '    }'],
      ['',             '    vector<int> arr(n);'],
      ['',             '    for (int i = 0; i < n; i++) {'],
      ['',             '        cin >> arr[i];'],
      ['',             '    }'],
      ['m_alloc_memo', '    vector<vector<int>> memo(n + 1, vector<int>(n + 1));'],
      ['m_fill_memo',  '    for (auto& row : memo) {'],
      ['',             '        fill(row.begin(), row.end(), -1);'],
      ['',             '    }'],
      ['m_call_lis',   '    int ans = solve(arr, 0, -1, memo);'],
      ['m_print',      '    cout << ans << endl;'],
      ['m_done',       '    return 0;'],
      ['',             '}']
    ],
    python: [
      ['',             'import sys'],
      ['',             ''],
      ['c_entry',      'def solve(arr, i, prev_idx, memo):'],
      ['c_base',       '    if i == len(arr):'],
      ['c_base_ret',   '        return 0'],
      ['c_memo_check', '    if memo[i][prev_idx + 1] != -1:'],
      ['c_memo_ret',   '        return memo[i][prev_idx + 1]'],
      ['c_init_take',  '    take = 0'],
      ['c_take_check', '    if prev_idx == -1 or arr[i] > arr[prev_idx]:'],
      ['c_take_rec',   '        take = 1 + solve(arr, i + 1, i, memo)'],
      ['c_nottake_rec','    not_take = solve(arr, i + 1, prev_idx, memo)'],
      ['c_store_memo', '    memo[i][prev_idx + 1] = max(take, not_take)'],
      ['c_ret_memo',   '    return memo[i][prev_idx + 1]'],
      ['',             ''],
      ['',             'if __name__ == "__main__":'],
      ['m_read_arr',   '    lines = sys.stdin.read().split()'],
      ['',             '    if not lines:'],
      ['',             '        sys.exit(0)'],
      ['',             '    n = int(lines[0])'],
      ['',             '    arr = [int(x) for x in lines[1:n + 1]]'],
      ['m_alloc_memo', '    memo = [[-1] * (n + 1) for _ in range(n + 1)]'],
      ['m_fill_memo',  '    # cache filled with -1 sentinel values'],
      ['m_call_lis',   '    ans = solve(arr, 0, -1, memo)'],
      ['m_print',      '    print(ans)'],
      ['m_done',       '    sys.exit(0)']
    ],
    javascript: [
      ['',             'const fs = require("fs");'],
      ['',             ''],
      ['c_entry',      'function solve(arr, i, prevIdx, memo) {'],
      ['c_base',       '    if (i === arr.length) {'],
      ['c_base_ret',   '        return 0;'],
      ['',             '    }'],
      ['c_memo_check', '    if (memo[i][prevIdx + 1] !== -1) {'],
      ['c_memo_ret',   '        return memo[i][prevIdx + 1];'],
      ['',             '    }'],
      ['c_init_take',  '    let take = 0;'],
      ['c_take_check', '    if (prevIdx === -1 || arr[i] > arr[prevIdx]) {'],
      ['c_take_rec',   '        take = 1 + solve(arr, i + 1, i, memo);'],
      ['',             '    }'],
      ['c_nottake_rec','    const notTake = solve(arr, i + 1, prevIdx, memo);'],
      ['c_store_memo', '    memo[i][prevIdx + 1] = Math.max(take, notTake);'],
      ['c_ret_memo',   '    return memo[i][prevIdx + 1];'],
      ['',             '}'],
      ['',             ''],
      ['m_read_arr',   'const input = fs.readFileSync(0, "utf-8").trim().split(/\\s+/);'],
      ['',             'const n = parseInt(input[0], 10);'],
      ['',             'const arr = input.slice(1, n + 1).map(Number);'],
      ['m_alloc_memo', 'const memo = Array.from({ length: n + 1 }, () => new Array(n + 1));'],
      ['m_fill_memo',  'memo.forEach(row => row.fill(-1));'],
      ['m_call_lis',   'const ans = solve(arr, 0, -1, memo);'],
      ['m_print',      'console.log(ans);'],
      ['m_done',       'process.exit(0);']
    ]
  },
  tabulation: {
    java: [
      ['',             'import java.util.Scanner;'],
      ['',             'import java.util.Arrays;'],
      ['',             ''],
      ['',             'public class Main {'],
      ['c_entry',      '    static int lengthOfLIS(int[] arr) {'],
      ['c_init_dims',  '        int n = arr.length;'],
      ['c_check_empty','        if (n == 0) {'],
      ['c_ret_empty',  '            return 0;'],
      ['',             '        }'],
      ['c_alloc_dp',   '        int[] dp = new int[n];'],
      ['c_fill_dp',    '        Arrays.fill(dp, 1);'],
      ['c_init_max',   '        int maxLIS = 1;'],
      ['c_loop_i',     '        for (int i = 1; i < n; i++) {'],
      ['c_loop_j',     '            for (int j = 0; j < i; j++) {'],
      ['c_comp_chk',   '                if (arr[i] > arr[j]) {'],
      ['c_dp_trans',   '                    dp[i] = Math.max(dp[i], 1 + dp[j]);'],
      ['',             '                }'],
      ['',             '            }'],
      ['c_update_max', '            maxLIS = Math.max(maxLIS, dp[i]);'],
      ['',             '        }'],
      ['c_ret_dp',     '        return maxLIS;'],
      ['',             '    }'],
      ['',             ''],
      ['',             '    public static void main(String[] args) {'],
      ['m_scanner',    '        Scanner sc = new Scanner(System.in);'],
      ['m_read_arr',   '        int n = sc.nextInt();'],
      ['',             '        int[] arr = new int[n];'],
      ['',             '        for (int i = 0; i < n; i++) {'],
      ['',             '            arr[i] = sc.nextInt();'],
      ['',             '        }'],
      ['m_call_lis',   '        int ans = lengthOfLIS(arr);'],
      ['m_print',      '        System.out.println(ans);'],
      ['m_done',       '    }'],
      ['',             '}']
    ],
    c: [
      ['',             '#include <stdio.h>'],
      ['',             '#define MAX(a, b) ((a) > (b) ? (a) : (b))'],
      ['',             ''],
      ['c_entry',      'int lengthOfLIS(int arr[], int n) {'],
      ['c_init_dims',  '    int len = n;'],
      ['c_check_empty','    if (len == 0) {'],
      ['c_ret_empty',  '        return 0;'],
      ['',             '    }'],
      ['c_alloc_dp',   '    int dp[len];'],
      ['c_fill_dp',    '    for (int i = 0; i < len; i++) {'],
      ['',             '        dp[i] = 1;'],
      ['',             '    }'],
      ['c_init_max',   '    int maxLIS = 1;'],
      ['c_loop_i',     '    for (int i = 1; i < len; i++) {'],
      ['c_loop_j',     '        for (int j = 0; j < i; j++) {'],
      ['c_comp_chk',   '            if (arr[i] > arr[j]) {'],
      ['c_dp_trans',   '                dp[i] = MAX(dp[i], 1 + dp[j]);'],
      ['',             '            }'],
      ['',             '        }'],
      ['c_update_max', '        maxLIS = MAX(maxLIS, dp[i]);'],
      ['',             '    }'],
      ['c_ret_dp',     '    return maxLIS;'],
      ['',             '}'],
      ['',             ''],
      ['',             'int main() {'],
      ['m_read_arr',   '    int n;'],
      ['',             '    if (scanf("%d", &n) != 1) {'],
      ['',             '        return 0;'],
      ['',             '    }'],
      ['',             '    int arr[n];'],
      ['',             '    for (int i = 0; i < n; i++) {'],
      ['',             '        scanf("%d", &arr[i]);'],
      ['',             '    }'],
      ['m_call_lis',   '    int ans = lengthOfLIS(arr, n);'],
      ['m_print',      '    printf("%d\\n", ans);'],
      ['m_done',       '    return 0;'],
      ['',             '}']
    ],
    cpp: [
      ['',             '#include <iostream>'],
      ['',             '#include <vector>'],
      ['',             '#include <algorithm>'],
      ['',             'using namespace std;'],
      ['',             ''],
      ['c_entry',      'int lengthOfLIS(const vector<int>& arr) {'],
      ['c_init_dims',  '    int n = arr.size();'],
      ['c_check_empty','    if (n == 0) {'],
      ['c_ret_empty',  '        return 0;'],
      ['',             '    }'],
      ['c_alloc_dp',   '    vector<int> dp(n);'],
      ['c_fill_dp',    '    fill(dp.begin(), dp.end(), 1);'],
      ['c_init_max',   '    int maxLIS = 1;'],
      ['c_loop_i',     '    for (int i = 1; i < n; i++) {'],
      ['c_loop_j',     '        for (int j = 0; j < i; j++) {'],
      ['c_comp_chk',   '            if (arr[i] > arr[j]) {'],
      ['c_dp_trans',   '                dp[i] = max(dp[i], 1 + dp[j]);'],
      ['',             '            }'],
      ['',             '        }'],
      ['c_update_max', '        maxLIS = max(maxLIS, dp[i]);'],
      ['',             '    }'],
      ['c_ret_dp',     '    return maxLIS;'],
      ['',             '}'],
      ['',             ''],
      ['',             'int main() {'],
      ['m_read_arr',   '    int n;'],
      ['',             '    if (!(cin >> n)) {'],
      ['',             '        return 0;'],
      ['',             '    }'],
      ['',             '    vector<int> arr(n);'],
      ['',             '    for (int i = 0; i < n; i++) {'],
      ['',             '        cin >> arr[i];'],
      ['',             '    }'],
      ['m_call_lis',   '    int ans = lengthOfLIS(arr);'],
      ['m_print',      '    cout << ans << endl;'],
      ['m_done',       '    return 0;'],
      ['',             '}']
    ],
    python: [
      ['',             'import sys'],
      ['',             ''],
      ['c_entry',      'def length_of_lis(arr):'],
      ['c_init_dims',  '    n = len(arr)'],
      ['c_check_empty','    if n == 0:'],
      ['c_ret_empty',  '        return 0'],
      ['c_alloc_dp',   '    dp = [0] * n'],
      ['c_fill_dp',    '    dp = [1] * n'],
      ['c_init_max',   '    max_lis = 1'],
      ['c_loop_i',     '    for i in range(1, n):'],
      ['c_loop_j',     '        for j in range(i):'],
      ['c_comp_chk',   '            if arr[i] > arr[j]:'],
      ['c_dp_trans',   '                dp[i] = max(dp[i], 1 + dp[j])'],
      ['c_update_max', '        max_lis = max(max_lis, dp[i])'],
      ['c_ret_dp',     '    return max_lis'],
      ['',             ''],
      ['',             'if __name__ == "__main__":'],
      ['m_read_arr',   '    lines = sys.stdin.read().split()'],
      ['',             '    if not lines:'],
      ['',             '        sys.exit(0)'],
      ['',             '    n = int(lines[0])'],
      ['',             '    arr = [int(x) for x in lines[1:n + 1]]'],
      ['m_call_lis',   '    ans = length_of_lis(arr)'],
      ['m_print',      '    print(ans)'],
      ['m_done',       '    sys.exit(0)']
    ],
    javascript: [
      ['',             'const fs = require("fs");'],
      ['',             ''],
      ['c_entry',      'function lengthOfLIS(arr) {'],
      ['c_init_dims',  '    const n = arr.length;'],
      ['c_check_empty','    if (n === 0) {'],
      ['c_ret_empty',  '        return 0;'],
      ['',             '    }'],
      ['c_alloc_dp',   '    const dp = new Array(n);'],
      ['c_fill_dp',    '    dp.fill(1);'],
      ['c_init_max',   '    let maxLIS = 1;'],
      ['c_loop_i',     '    for (let i = 1; i < n; i++) {'],
      ['c_loop_j',     '        for (let j = 0; j < i; j++) {'],
      ['c_comp_chk',   '            if (arr[i] > arr[j]) {'],
      ['c_dp_trans',   '                dp[i] = Math.max(dp[i], 1 + dp[j]);'],
      ['',             '            }'],
      ['',             '        }'],
      ['c_update_max', '        maxLIS = Math.max(maxLIS, dp[i]);'],
      ['',             '    }'],
      ['c_ret_dp',     '    return maxLIS;'],
      ['',             '}'],
      ['',             ''],
      ['m_read_arr',   'const input = fs.readFileSync(0, "utf-8").trim().split(/\\s+/);'],
      ['',             'const n = parseInt(input[0], 10);'],
      ['',             'const arr = input.slice(1, n + 1).map(Number);'],
      ['m_call_lis',   'const ans = lengthOfLIS(arr);'],
      ['m_print',      'console.log(ans);'],
      ['m_done',       'process.exit(0);']
    ]
  }
};

const PSEUDOCODES = {
  recursion: [
    'function lis(arr, i, prevIdx):',
    '  if i == arr.length:',
    '    return 0                             // Base case: end of array reached',
    '  take = 0',
    '  if prevIdx == -1 or arr[i] > arr[prevIdx]:',
    '    take = 1 + lis(arr, i + 1, i)        // Include arr[i] if strictly greater',
    '  notTake = lis(arr, i + 1, prevIdx)     // Exclude arr[i]',
    '  return max(take, notTake)',
    '',
    'Time Complexity:  O(2^n) Exponential',
    'Space Complexity: O(n) Maximum Call Stack Depth'
  ],
  memoization: [
    'memo = 2D array of size (n+1) x (n+1) initialized with -1',
    '',
    'function solve(arr, i, prevIdx, memo):',
    '  if i == arr.length:',
    '    return 0',
    '  if memo[i][prevIdx + 1] != -1:',
    '    return memo[i][prevIdx + 1]          // O(1) Cache Hit',
    '  take = 0',
    '  if prevIdx == -1 or arr[i] > arr[prevIdx]:',
    '    take = 1 + solve(arr, i + 1, i, memo)',
    '  notTake = solve(arr, i + 1, prevIdx, memo)',
    '  memo[i][prevIdx + 1] = max(take, notTake)',
    '  return memo[i][prevIdx + 1]',
    '',
    'Time Complexity:  O(n^2) Quadratic',
    'Space Complexity: O(n^2) 2D Table + O(n) Call Stack'
  ],
  tabulation: [
    'function lengthOfLIS(arr):',
    '  n = arr.length',
    '  if n == 0:',
    '    return 0',
    '  dp = array of size n initialized with 1',
    '  maxLIS = 1',
    '  for i = 1 to n - 1:',
    '    for j = 0 to i - 1:',
    '      if arr[i] > arr[j]:',
    '        dp[i] = max(dp[i], 1 + dp[j])    // Transition',
    '    maxLIS = max(maxLIS, dp[i])',
    '  return maxLIS',
    '',
    'Time Complexity:  O(n^2) Quadratic (Can be optimized to O(n log n) with Binary Search)',
    'Space Complexity: O(n) 1D Table'
  ]
};

/* ==================================================================== */
/* STATIC TREE GENERATOR FOR LIS DECISION TREE                          */
/* ==================================================================== */
function generateStaticTree(approach, arr) {
  const n = arr.length;
  const nodes = [];
  const edges = [];
  let nextId = 0;

  if (approach === 'recursion') {
    function build(i, prevIdx, parentId, branchType) {
      const id = nextId++;
      const node = { id, i, prevIdx, parentId, branchType, children: [] };
      nodes.push(node);

      if (parentId !== null) {
        edges.push({ from: parentId, to: id, branchType });
      }

      if (i < n) {
        const canTake = prevIdx === -1 || arr[i] > arr[prevIdx];
        if (canTake) {
          const takeId = build(i + 1, i, id, 'take');
          const notTakeId = build(i + 1, prevIdx, id, 'nottake');
          node.children = [takeId, notTakeId];
        } else {
          const notTakeId = build(i + 1, prevIdx, id, 'nottake');
          node.children = [notTakeId];
        }
      }
      return id;
    }
    if (n >= 0) build(0, -1, null, 'root');
  } else if (approach === 'memoization') {
    const memo = Array.from({ length: n + 1 }, () => new Array(n + 2).fill(false));
    function buildMemo(i, prevIdx, parentId, branchType) {
      const id = nextId++;
      const isCacheHit = i >= 0 && memo[i][prevIdx + 1];
      const node = { id, i, prevIdx, parentId, branchType, isCacheHit, children: [] };
      nodes.push(node);

      if (parentId !== null) {
        edges.push({ from: parentId, to: id, branchType });
      }

      if (i >= n) {
        // base
      } else if (isCacheHit) {
        // Pruned subtree
      } else {
        memo[i][prevIdx + 1] = true;
        const canTake = prevIdx === -1 || arr[i] > arr[prevIdx];
        if (canTake) {
          const takeId = buildMemo(i + 1, i, id, 'take');
          const notTakeId = buildMemo(i + 1, prevIdx, id, 'nottake');
          node.children = [takeId, notTakeId];
        } else {
          const notTakeId = buildMemo(i + 1, prevIdx, id, 'nottake');
          node.children = [notTakeId];
        }
      }
      return id;
    }
    if (n >= 0) buildMemo(0, -1, null, 'root');
  }

  // Layout Engine (x, y)
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
  if (nodes.length > 0) assignSubtreeMetrics(0, 0);

  const totalLeaves = Math.max(1, leafCounter);
  const maxDepth = Math.max(0, ...Array.from(nodeMap.values()).map(nd => nd.depth || 0));

  const leafSpacing = 64;
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
function buildSteps(approach, rawArr) {
  const steps = [];
  const maxLimit = approach === 'tabulation' ? 8 : 5;
  const arr = (rawArr && rawArr.length ? rawArr : [10, 9, 2, 5, 3, 7, 101, 18]).slice(0, maxLimit);
  const n = arr.length;

  /* ------------------------------------------------------------------ */
  /* APPROACH 1: BRUTE FORCE RECURSION                                   */
  /* ------------------------------------------------------------------ */
  if (approach === 'recursion') {
    const staticTree = generateStaticTree('recursion', arr);
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

    // Step 0: Read array
    steps.push({
      code: 'm_read_arr',
      badge: `Input Array: [${arr.join(', ')}] (n=${n})`,
      badgeType: 'info',
      state: {
        arr, n,
        totalCalls: 0,
        treeNodes: [],
        treeEdges: [],
        treeWidth: staticTree.width,
        treeHeight: staticTree.height,
        activeNodeId: null,
        currentReturn: null,
        frames: []
      }
    });

    // Step 1: Call root
    steps.push({
      code: 'm_call_lis',
      badge: `Invoking root recursive call lis(arr, i=0, prevIdx=-1)`,
      badgeType: 'info',
      state: {
        arr, n,
        totalCalls: 0,
        treeNodes: [],
        treeEdges: [],
        treeWidth: staticTree.width,
        treeHeight: staticTree.height,
        activeNodeId: null,
        currentReturn: null,
        frames: [{ name: 'main()', args: `arr=[${arr.join(', ')}]` }]
      }
    });

    function solveRec(i, prevIdx) {
      const myId = nextNodeId++;
      callCounter++;

      nodeStateMap[myId].state = 'active';
      stackFrames.push({ name: `lis(i=${i}, prev=${prevIdx})`, args: `val=${i < n ? arr[i] : 'END'}` });

      // Line: Function entry
      steps.push({
        code: 'c_entry',
        badge: `Entering lis(i=${i}, prevIdx=${prevIdx}) &mdash; evaluating arr[${i}] = ${i < n ? arr[i] : 'OUT_OF_BOUNDS'}`,
        badgeType: 'info',
        state: {
          arr, n, curI: i, prevIdx,
          totalCalls: callCounter,
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height,
          activeNodeId: myId,
          currentReturn: null,
          frames: [...stackFrames]
        }
      });

      // Line: Base case check
      steps.push({
        code: 'c_base',
        badge: i === n
          ? `Base Case Check: (i == n=${n}) is TRUE &rarr; End of array reached`
          : `Base Case Check: (i=${i} == n=${n}) is FALSE &rarr; Array element available, proceed`,
        badgeType: i === n ? 'warn' : 'info',
        state: {
          arr, n, curI: i, prevIdx,
          totalCalls: callCounter,
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height,
          activeNodeId: myId,
          currentReturn: null,
          frames: [...stackFrames]
        }
      });

      if (i === n) {
        // Line: Base case return 0
        steps.push({
          code: 'c_base_ret',
          badge: `Base Case Return: return 0 (Length of empty subsequence is 0)`,
          badgeType: 'success',
          state: {
            arr, n, curI: i, prevIdx,
            totalCalls: callCounter,
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

      // Line: Initialize take = 0
      steps.push({
        code: 'c_init_take',
        badge: `int take = 0; Initialize take option length to 0`,
        badgeType: 'info',
        state: {
          arr, n, curI: i, prevIdx,
          totalCalls: callCounter,
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height,
          activeNodeId: myId,
          currentReturn: null,
          frames: [...stackFrames]
        }
      });

      const canTake = prevIdx === -1 || arr[i] > arr[prevIdx];

      // Line: Condition check
      steps.push({
        code: 'c_take_check',
        badge: canTake
          ? `Condition Check: ${prevIdx === -1 ? 'prevIdx == -1 (First Element)' : `arr[${i}] (${arr[i]}) > arr[${prevIdx}] (${arr[prevIdx]})`} is TRUE &rarr; CAN TAKE arr[${i}]`
          : `Condition Check: arr[${i}] (${arr[i]}) &le; arr[${prevIdx}] (${arr[prevIdx]}) is FALSE &rarr; CANNOT TAKE (Must Skip)`,
        badgeType: canTake ? 'success' : 'warn',
        state: {
          arr, n, curI: i, prevIdx,
          canTake,
          totalCalls: callCounter,
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height,
          activeNodeId: myId,
          currentReturn: null,
          frames: [...stackFrames]
        }
      });

      let takeVal = 0;
      if (canTake) {
        // Line: Take recursive call
        steps.push({
          code: 'c_take_rec',
          badge: `Take Option: Including arr[${i}] (${arr[i]}). Recursing take branch: 1 + lis(i=${i + 1}, prevIdx=${i})`,
          badgeType: 'info',
          state: {
            arr, n, curI: i, prevIdx,
            totalCalls: callCounter,
            treeNodes: getVisibleNodes(),
            treeEdges: getVisibleEdges(),
            treeWidth: staticTree.width,
            treeHeight: staticTree.height,
            activeNodeId: myId,
            currentReturn: null,
            frames: [...stackFrames]
          }
        });

        const subTake = solveRec(i + 1, i);
        takeVal = 1 + subTake;
      }

      // Line: Not-take recursive call
      steps.push({
        code: 'c_nottake_rec',
        badge: `Not-Take Option: Excluding arr[${i}] (${arr[i]}). Recursing notTake branch: lis(i=${i + 1}, prevIdx=${prevIdx})`,
        badgeType: 'info',
        state: {
          arr, n, curI: i, prevIdx,
          totalCalls: callCounter,
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height,
          activeNodeId: myId,
          currentReturn: null,
          frames: [...stackFrames]
        }
      });

      const notTakeVal = solveRec(i + 1, prevIdx);

      const maxResult = Math.max(takeVal, notTakeVal);
      nodeStateMap[myId].state = 'solved';
      nodeStateMap[myId].retVal = maxResult;

      // Line: Return max
      steps.push({
        code: 'c_ret_max',
        badge: `Combining decisions for i=${i} (arr[${i}]=${arr[i]}): max(take=${takeVal}, notTake=${notTakeVal}) = ${maxResult}. Returning ${maxResult}.`,
        badgeType: 'success',
        state: {
          arr, n, curI: i, prevIdx,
          totalCalls: callCounter,
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height,
          activeNodeId: myId,
          currentReturn: maxResult,
          frames: [...stackFrames]
        }
      });

      stackFrames.pop();
      return maxResult;
    }

    const finalAnswer = solveRec(0, -1);

    steps.push({
      code: 'm_print',
      badge: `Output Result: Print Longest Increasing Subsequence Length = ${finalAnswer}`,
      badgeType: 'success',
      state: {
        arr, n,
        totalCalls: callCounter,
        treeNodes: getVisibleNodes(),
        treeEdges: getVisibleEdges(),
        treeWidth: staticTree.width,
        treeHeight: staticTree.height,
        activeNodeId: 0,
        currentReturn: finalAnswer,
        frames: [{ name: 'main()', args: `result=${finalAnswer}` }]
      }
    });

    steps.push({
      code: 'm_done',
      badge: `Execution Complete! Total LIS length: ${finalAnswer}. (Evaluated ${callCounter} recursive calls)`,
      badgeType: 'success',
      state: {
        arr, n,
        totalCalls: callCounter,
        treeNodes: getVisibleNodes(),
        treeEdges: getVisibleEdges(),
        treeWidth: staticTree.width,
        treeHeight: staticTree.height,
        activeNodeId: null,
        currentReturn: finalAnswer,
        frames: [{ name: 'main()', args: `result=${finalAnswer}` }]
      }
    });
  }

  /* ------------------------------------------------------------------ */
  /* APPROACH 2: DP MEMOIZATION (TOP-DOWN)                             */
  /* ------------------------------------------------------------------ */
  else if (approach === 'memoization') {
    const staticTree = generateStaticTree('memoization', arr);
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

    const memoArray = Array.from({ length: n + 1 }, () => new Array(n + 1).fill(-1));
    const zeroMemo = Array.from({ length: n + 1 }, () => new Array(n + 1).fill(0));
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
      code: 'm_read_arr',
      badge: `Input Array: [${arr.join(', ')}] (n=${n}). Starting Top-Down DP with 2D Memoization.`,
      badgeType: 'info',
      state: {
        arr, n,
        totalCalls: 0,
        cacheHits: 0,
        treeNodes: [],
        treeEdges: [],
        treeWidth: staticTree.width,
        treeHeight: staticTree.height,
        activeNodeId: null,
        currentReturn: null,
        memo: zeroMemo.map(r => [...r]),
        frames: [{ name: 'main()', args: `n=${n}` }]
      }
    });

    // Step 1: Allocate memo
    steps.push({
      code: 'm_alloc_memo',
      badge: `int[][] memo = new int[${n + 1}][${n + 1}]; Allocated 2D heap matrix (default Java int array values are 0)`,
      badgeType: 'info',
      state: {
        arr, n,
        totalCalls: 0,
        cacheHits: 0,
        treeNodes: [],
        treeEdges: [],
        treeWidth: staticTree.width,
        treeHeight: staticTree.height,
        activeNodeId: null,
        currentReturn: null,
        memo: zeroMemo.map(r => [...r]),
        frames: [{ name: 'main()', args: `n=${n}` }]
      }
    });

    // Step 2: Initialize memo with -1
    steps.push({
      code: 'm_fill_memo',
      badge: `Arrays.fill(memo, -1); Filling all cache entries in 2D memo matrix with sentinel value -1 (uncalculated state)`,
      badgeType: 'info',
      state: {
        arr, n,
        totalCalls: 0,
        cacheHits: 0,
        treeNodes: [],
        treeEdges: [],
        treeWidth: staticTree.width,
        treeHeight: staticTree.height,
        activeNodeId: null,
        currentReturn: null,
        memo: memoArray.map(r => [...r]),
        frames: [{ name: 'main()', args: `n=${n}` }]
      }
    });

    // Step 3: Call solve
    steps.push({
      code: 'm_call_lis',
      badge: `Invoking root recursive memo call solve(arr, i=0, prevIdx=-1, memo)`,
      badgeType: 'info',
      state: {
        arr, n,
        totalCalls: 0,
        cacheHits: 0,
        treeNodes: [],
        treeEdges: [],
        treeWidth: staticTree.width,
        treeHeight: staticTree.height,
        activeNodeId: null,
        currentReturn: null,
        memo: memoArray.map(r => [...r]),
        frames: [{ name: 'main()', args: `arr=[${arr.join(',')}]` }]
      }
    });

    function solveMemo(i, prevIdx) {
      const myId = nextNodeId++;
      callCounter++;

      nodeStateMap[myId].state = 'active';
      stackFrames.push({ name: `solve(i=${i}, prev=${prevIdx})`, args: `val=${i < n ? arr[i] : 'END'}` });

      // Line: Function entry
      steps.push({
        code: 'c_entry',
        badge: `Entering solve(i=${i}, prevIdx=${prevIdx}, memo)`,
        badgeType: 'info',
        state: {
          arr, n, curI: i, prevIdx,
          totalCalls: callCounter,
          cacheHits,
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height,
          activeNodeId: myId,
          currentReturn: null,
          memo: memoArray.map(r => [...r]),
          frames: [...stackFrames]
        }
      });

      // Line: Base case check
      steps.push({
        code: 'c_base',
        badge: i === n
          ? `Base Case Check: (i == n=${n}) is TRUE &rarr; Return length 0`
          : `Base Case Check: (i=${i} == n=${n}) is FALSE &rarr; Proceed to cache check`,
        badgeType: i === n ? 'warn' : 'info',
        state: {
          arr, n, curI: i, prevIdx,
          totalCalls: callCounter,
          cacheHits,
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height,
          activeNodeId: myId,
          currentReturn: null,
          memo: memoArray.map(r => [...r]),
          frames: [...stackFrames]
        }
      });

      if (i === n) {
        // Line: Base case return 0
        steps.push({
          code: 'c_base_ret',
          badge: `Base Case Return: return 0 (Length of empty subsequence is 0)`,
          badgeType: 'success',
          state: {
            arr, n, curI: i, prevIdx,
            totalCalls: callCounter,
            cacheHits,
            treeNodes: getVisibleNodes(),
            treeEdges: getVisibleEdges(),
            treeWidth: staticTree.width,
            treeHeight: staticTree.height,
            activeNodeId: myId,
            currentReturn: 0,
            memo: memoArray.map(r => [...r]),
            frames: [...stackFrames]
          }
        });
        nodeStateMap[myId].state = 'solved';
        nodeStateMap[myId].retVal = 0;
        stackFrames.pop();
        return 0;
      }

      // Line: Memo cache check
      const cachedVal = memoArray[i][prevIdx + 1];
      steps.push({
        code: 'c_memo_check',
        badge: cachedVal !== -1
          ? `[CACHE HIT!] memo[${i}][${prevIdx + 1}] = ${cachedVal} already computed! Returning in O(1) time!`
          : `Cache Check: memo[${i}][${prevIdx + 1}] == -1 &rarr; Uncached subproblem. Computing options.`,
        badgeType: cachedVal !== -1 ? 'success' : 'info',
        state: {
          arr, n, curI: i, prevIdx,
          totalCalls: callCounter,
          cacheHits: cachedVal !== -1 ? cacheHits + 1 : cacheHits,
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height,
          activeNodeId: myId,
          currentReturn: cachedVal !== -1 ? cachedVal : null,
          memo: memoArray.map(r => [...r]),
          frames: [...stackFrames]
        }
      });

      if (cachedVal !== -1) {
        // Line: Memo cache return
        steps.push({
          code: 'c_memo_ret',
          badge: `Return Cached Result: return memo[${i}][${prevIdx + 1}] = ${cachedVal}`,
          badgeType: 'success',
          state: {
            arr, n, curI: i, prevIdx,
            totalCalls: callCounter,
            cacheHits: cacheHits + 1,
            treeNodes: getVisibleNodes(),
            treeEdges: getVisibleEdges(),
            treeWidth: staticTree.width,
            treeHeight: staticTree.height,
            activeNodeId: myId,
            currentReturn: cachedVal,
            memo: memoArray.map(r => [...r]),
            frames: [...stackFrames]
          }
        });
        cacheHits++;
        nodeStateMap[myId].state = 'cache-hit';
        nodeStateMap[myId].retVal = cachedVal;
        stackFrames.pop();
        return cachedVal;
      }

      // Line: Initialize take = 0
      steps.push({
        code: 'c_init_take',
        badge: `int take = 0; Initialize take option to 0`,
        badgeType: 'info',
        state: {
          arr, n, curI: i, prevIdx,
          totalCalls: callCounter,
          cacheHits,
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height,
          activeNodeId: myId,
          currentReturn: null,
          memo: memoArray.map(r => [...r]),
          frames: [...stackFrames]
        }
      });

      const canTake = prevIdx === -1 || arr[i] > arr[prevIdx];

      // Line: Condition check
      steps.push({
        code: 'c_take_check',
        badge: canTake
          ? `Condition Check: ${prevIdx === -1 ? 'prevIdx == -1' : `arr[${i}] (${arr[i]}) > arr[${prevIdx}] (${arr[prevIdx]})`} is TRUE &rarr; CAN TAKE arr[${i}]`
          : `Condition Check: arr[${i}] (${arr[i]}) &le; arr[${prevIdx}] (${arr[prevIdx]}) is FALSE &rarr; CANNOT TAKE`,
        badgeType: canTake ? 'success' : 'warn',
        state: {
          arr, n, curI: i, prevIdx,
          canTake,
          totalCalls: callCounter,
          cacheHits,
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height,
          activeNodeId: myId,
          currentReturn: null,
          memo: memoArray.map(r => [...r]),
          frames: [...stackFrames]
        }
      });

      let takeVal = 0;
      if (canTake) {
        // Line: Take recursive call
        steps.push({
          code: 'c_take_rec',
          badge: `Take Option: Recursing solve(i=${i + 1}, prevIdx=${i}, memo)`,
          badgeType: 'info',
          state: {
            arr, n, curI: i, prevIdx,
            totalCalls: callCounter,
            cacheHits,
            treeNodes: getVisibleNodes(),
            treeEdges: getVisibleEdges(),
            treeWidth: staticTree.width,
            treeHeight: staticTree.height,
            activeNodeId: myId,
            currentReturn: null,
            memo: memoArray.map(r => [...r]),
            frames: [...stackFrames]
          }
        });

        const subTake = solveMemo(i + 1, i);
        takeVal = 1 + subTake;
      }

      // Line: Not-take recursive call
      steps.push({
        code: 'c_nottake_rec',
        badge: `Not-Take Option: Recursing solve(i=${i + 1}, prevIdx=${prevIdx}, memo)`,
        badgeType: 'info',
        state: {
          arr, n, curI: i, prevIdx,
          totalCalls: callCounter,
          cacheHits,
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height,
          activeNodeId: myId,
          currentReturn: null,
          memo: memoArray.map(r => [...r]),
          frames: [...stackFrames]
        }
      });

      const notTakeVal = solveMemo(i + 1, prevIdx);

      const maxResult = Math.max(takeVal, notTakeVal);
      memoArray[i][prevIdx + 1] = maxResult;

      // Line: Store in memo
      steps.push({
        code: 'c_store_memo',
        badge: `Both decisions computed (take=${takeVal}, notTake=${notTakeVal}). Updating memo[${i}][${prevIdx + 1}] = max(${takeVal}, ${notTakeVal}) = ${maxResult}.`,
        badgeType: 'info',
        state: {
          arr, n, curI: i, prevIdx,
          totalCalls: callCounter,
          cacheHits,
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height,
          activeNodeId: myId,
          currentReturn: maxResult,
          memo: memoArray.map(r => [...r]),
          frames: [...stackFrames]
        }
      });

      nodeStateMap[myId].state = 'solved';
      nodeStateMap[myId].retVal = maxResult;

      // Line: Return memo
      steps.push({
        code: 'c_ret_memo',
        badge: `Returning memo[${i}][${prevIdx + 1}] = ${maxResult} to caller`,
        badgeType: 'success',
        state: {
          arr, n, curI: i, prevIdx,
          totalCalls: callCounter,
          cacheHits,
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height,
          activeNodeId: myId,
          currentReturn: maxResult,
          memo: memoArray.map(r => [...r]),
          frames: [...stackFrames]
        }
      });

      stackFrames.pop();
      return maxResult;
    }

    const finalAnswer = solveMemo(0, -1);

    steps.push({
      code: 'm_print',
      badge: `Output Result: Print Longest Increasing Subsequence Length = ${finalAnswer}`,
      badgeType: 'success',
      state: {
        arr, n,
        totalCalls: callCounter,
        cacheHits,
        treeNodes: getVisibleNodes(),
        treeEdges: getVisibleEdges(),
        treeWidth: staticTree.width,
        treeHeight: staticTree.height,
        activeNodeId: 0,
        currentReturn: finalAnswer,
        memo: memoArray.map(r => [...r]),
        frames: [{ name: 'main()', args: `result=${finalAnswer}` }]
      }
    });

    steps.push({
      code: 'm_done',
      badge: `Top-Down DP Memoization Complete! Total LIS length: ${finalAnswer}. Cache Hits saved ${cacheHits} expensive subtrees!`,
      badgeType: 'success',
      state: {
        arr, n,
        totalCalls: callCounter,
        cacheHits,
        treeNodes: getVisibleNodes(),
        treeEdges: getVisibleEdges(),
        treeWidth: staticTree.width,
        treeHeight: staticTree.height,
        activeNodeId: null,
        currentReturn: finalAnswer,
        memo: memoArray.map(r => [...r]),
        frames: [{ name: 'main()', args: `result=${finalAnswer}` }]
      }
    });
  }

  /* ------------------------------------------------------------------ */
  /* APPROACH 3: DP TABULATION (BOTTOM-UP 1D DP ARRAY)                  */
  /* ------------------------------------------------------------------ */
  else if (approach === 'tabulation') {
    const dpArr = new Array(n).fill(null);

    function getDPCells(activeI = -1, activeJ = -1) {
      return dpArr.map((val, idx) => ({
        idx,
        val: val !== null ? val : 0,
        status: val !== null ? (idx === activeI ? 'active' : (idx === activeJ ? 'compare' : 'computed')) : 'uncalculated'
      }));
    }

    // Step 0: Input read
    steps.push({
      code: 'm_read_arr',
      badge: `Input Array: [${arr.join(', ')}] (n=${n}). Starting Bottom-Up 1D DP Tabulation.`,
      badgeType: 'info',
      state: {
        arr, n, i: -1, j: -1, maxLIS: 1,
        dpCells: getDPCells(),
        frames: [{ name: 'main()', args: `n=${n}` }]
      }
    });

    // Step 1: Call lengthOfLIS
    steps.push({
      code: 'm_call_lis',
      badge: `Calling lengthOfLIS(arr) with array of length ${n}`,
      badgeType: 'info',
      state: {
        arr, n, i: -1, j: -1, maxLIS: 1,
        dpCells: getDPCells(),
        frames: [{ name: 'main()', args: `n=${n}` }]
      }
    });

    // Step 2: Function Entry
    steps.push({
      code: 'c_entry',
      badge: `Entering lengthOfLIS(arr) &mdash; initializing 1D DP array of size ${n}`,
      badgeType: 'info',
      state: {
        arr, n, i: -1, j: -1, maxLIS: 1,
        dpCells: getDPCells(),
        frames: [{ name: 'lengthOfLIS()', args: `arr=[${arr.join(', ')}]` }]
      }
    });

    // Step 3: int n = arr.length;
    steps.push({
      code: 'c_init_dims',
      badge: `int n = arr.length = ${n}`,
      badgeType: 'info',
      state: {
        arr, n, i: -1, j: -1, maxLIS: 1,
        dpCells: getDPCells(),
        frames: [{ name: 'lengthOfLIS()', args: `n=${n}` }]
      }
    });

    // Step 4: if (n == 0) check
    steps.push({
      code: 'c_check_empty',
      badge: n === 0
        ? `Base Check: n == 0 is TRUE &rarr; Return 0`
        : `Base Check: n = ${n} > 0 is FALSE &rarr; Non-empty array, proceed with DP`,
      badgeType: n === 0 ? 'warn' : 'info',
      state: {
        arr, n, i: -1, j: -1, maxLIS: 1,
        dpCells: getDPCells(),
        frames: [{ name: 'lengthOfLIS()', args: `n=${n}` }]
      }
    });

    if (n === 0) {
      steps.push({
        code: 'c_ret_empty',
        badge: `return 0;`,
        badgeType: 'success',
        state: { arr, n, i: -1, j: -1, maxLIS: 0, dpCells: [], frames: [] }
      });
      return steps;
    }

    // Step 5: Allocate DP array
    steps.push({
      code: 'c_alloc_dp',
      badge: `int[] dp = new int[${n}]; Allocated 1D DP array for memoizing LIS ending at each index`,
      badgeType: 'info',
      state: {
        arr, n, i: -1, j: -1, maxLIS: 1,
        dpCells: getDPCells(),
        frames: [{ name: 'lengthOfLIS()', args: `n=${n}` }]
      }
    });

    // Step 6: Arrays.fill(dp, 1)
    for (let k = 0; k < n; k++) dpArr[k] = 1;

    steps.push({
      code: 'c_fill_dp',
      badge: `Arrays.fill(dp, 1); Base state: each single element arr[i] alone forms a valid subsequence of length 1`,
      badgeType: 'info',
      state: {
        arr, n, i: 0, j: -1, maxLIS: 1,
        dpCells: getDPCells(0),
        frames: [{ name: 'lengthOfLIS()', args: `dp filled with 1` }]
      }
    });

    // Step 7: int maxLIS = 1;
    let maxLIS = 1;
    steps.push({
      code: 'c_init_max',
      badge: `int maxLIS = 1; Tracking the global maximum length found across all dp[i]`,
      badgeType: 'info',
      state: {
        arr, n, i: 0, j: -1, maxLIS: 1,
        dpCells: getDPCells(0),
        frames: [{ name: 'lengthOfLIS()', args: `maxLIS=1` }]
      }
    });

    // Loops i and j:
    for (let i = 1; i < n; i++) {
      // Step: Outer loop header
      steps.push({
        code: 'c_loop_i',
        badge: `Outer Loop: for i = ${i} (< ${n}) &mdash; Target element arr[${i}] = ${arr[i]}. Checking preceding elements j < ${i}`,
        badgeType: 'info',
        state: {
          arr, n, i, j: -1, maxLIS,
          dpCells: getDPCells(i),
          frames: [{ name: 'lengthOfLIS()', args: `i=${i}, arr[i]=${arr[i]}` }]
        }
      });

      for (let j = 0; j < i; j++) {
        // Step: Inner loop header
        steps.push({
          code: 'c_loop_j',
          badge: `Inner Loop: for j = ${j} (< ${i}) &mdash; Checking preceding element arr[${j}] = ${arr[j]}`,
          badgeType: 'info',
          state: {
            arr, n, i, j, maxLIS,
            dpCells: getDPCells(i, j),
            frames: [{ name: 'lengthOfLIS()', args: `i=${i}, j=${j}, arr[i]=${arr[i]}, arr[j]=${arr[j]}` }]
          }
        });

        const isStrictlyGreater = arr[i] > arr[j];

        // Step: Condition check
        steps.push({
          code: 'c_comp_chk',
          badge: isStrictlyGreater
            ? `Condition Check: if (arr[${i}] (${arr[i]}) > arr[${j}] (${arr[j]})) is TRUE &rarr; arr[${i}] can extend LIS ending at j=${j}`
            : `Condition Check: if (arr[${i}] (${arr[i]}) > arr[${j}] (${arr[j]})) is FALSE &rarr; Not strictly increasing`,
          badgeType: isStrictlyGreater ? 'success' : 'warn',
          state: {
            arr, n, i, j, maxLIS,
            isStrictlyGreater,
            dpCells: getDPCells(i, j),
            frames: [{ name: 'lengthOfLIS()', args: `i=${i}, j=${j}` }]
          }
        });

        if (isStrictlyGreater) {
          const oldVal = dpArr[i];
          const candidateVal = 1 + dpArr[j];
          dpArr[i] = Math.max(dpArr[i], candidateVal);

          // Step: DP Transition
          steps.push({
            code: 'c_dp_trans',
            badge: `Transition: dp[${i}] = Math.max(dp[${i}] (${oldVal}), 1 + dp[${j}] (${candidateVal})) &rarr; dp[${i}] = ${dpArr[i]}`,
            badgeType: 'success',
            state: {
              arr, n, i, j, maxLIS,
              isStrictlyGreater: true,
              dpCells: getDPCells(i, j),
              frames: [{ name: 'lengthOfLIS()', args: `dp[${i}]=${dpArr[i]}` }]
            }
          });
        }
      }

      // Step: Update maxLIS
      maxLIS = Math.max(maxLIS, dpArr[i]);
      steps.push({
        code: 'c_update_max',
        badge: `End of Inner Loop for i=${i}: maxLIS = Math.max(${maxLIS}, dp[${i}] (${dpArr[i]})) = ${maxLIS}`,
        badgeType: 'success',
        state: {
          arr, n, i, j: -1, maxLIS,
          dpCells: getDPCells(i),
          frames: [{ name: 'lengthOfLIS()', args: `maxLIS=${maxLIS}` }]
        }
      });
    }

    // Step: Return maxLIS
    steps.push({
      code: 'c_ret_dp',
      badge: `Return result: return maxLIS = ${maxLIS}`,
      badgeType: 'success',
      state: {
        arr, n, i: n - 1, j: -1, maxLIS,
        currentReturn: maxLIS,
        dpCells: getDPCells(n - 1),
        frames: [{ name: 'main()', args: `result=${maxLIS}` }]
      }
    });

    // Step: Print
    steps.push({
      code: 'm_print',
      badge: `Output Result: Print Longest Increasing Subsequence Length = ${maxLIS}`,
      badgeType: 'success',
      state: {
        arr, n, i: n - 1, j: -1, maxLIS,
        currentReturn: maxLIS,
        dpCells: getDPCells(),
        frames: [{ name: 'main()', args: `result=${maxLIS}` }]
      }
    });

    // Step: Done
    steps.push({
      code: 'm_done',
      badge: `Tabulation Complete! Final Longest Increasing Subsequence Length: ${maxLIS}`,
      badgeType: 'success',
      state: {
        arr, n, i: n - 1, j: -1, maxLIS,
        currentReturn: maxLIS,
        dpCells: getDPCells(),
        frames: [{ name: 'main()', args: `result=${maxLIS}` }]
      }
    });
  }

  return steps;
}

/* ==================================================================== */
/* REACTIVE STATE & CONTROLS                                            */
/* ==================================================================== */
const currentApproach = ref('recursion');
const inputArrText = ref('10, 9, 2, 5, 3, 7, 101, 18');
const lang = ref('java');
const speed = ref(650);
const si = ref(0);
const playing = ref(false);

const vizHeight = ref(210);
const tableHeight = ref(42);
const leftWidth = ref(54);
const rightTab = ref('code');

// Warning Modal State
const showWarningModal = ref(false);
const warningModalTitle = ref('Input Limit Notice');
const warningModalMsg = ref('');
const warningModalLimit = ref(5);
const requestedLength = ref(0);

function closeWarningModal() {
  showWarningModal.value = false;
}

function parseInputArray(text) {
  if (!text) return [10, 9, 2, 5, 3, 7, 101, 18];
  return text
    .split(/[\s,]+/)
    .map(x => parseInt(x, 10))
    .filter(x => !isNaN(x));
}

const maxAllowedLength = computed(() => {
  return currentApproach.value === 'tabulation' ? 8 : 5;
});

const currentParsedArray = computed(() => parseInputArray(inputArrText.value));

const stepsData = reactive({ steps: buildSteps('recursion', [10, 9, 2, 5, 3, 7, 101, 18]) });
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
  const maxLen = newApproach === 'tabulation' ? 8 : 5;
  const arr = parseInputArray(inputArrText.value);

  if (arr.length > maxLen) {
    requestedLength.value = arr.length;
    warningModalLimit.value = maxLen;
    const appName = newApproach === 'tabulation' ? 'DP Tabulation' : (newApproach === 'recursion' ? 'Brute Force Recursion' : 'DP Memoization');
    warningModalTitle.value = `${appName} Limit (Max: ${maxLen})`;
    warningModalMsg.value = `Input array has ${arr.length} elements, which exceeds the maximum limit of ${maxLen} for ${appName}. Array will be capped at the first ${maxLen} elements to preserve layout clarity.`;
    showWarningModal.value = true;
  }

  playing.value = false;
  stepsData.steps = buildSteps(newApproach, arr);
  si.value = 0;
}

function applyInput() {
  const maxLen = maxAllowedLength.value;
  const arr = parseInputArray(inputArrText.value);

  if (arr.length > maxLen) {
    requestedLength.value = arr.length;
    warningModalLimit.value = maxLen;
    const appName = currentApproach.value === 'tabulation' ? 'DP Tabulation' : (currentApproach.value === 'recursion' ? 'Brute Force Recursion' : 'DP Memoization');
    warningModalTitle.value = `${appName} Limit (Max: ${maxLen})`;
    warningModalMsg.value = `Input array has ${arr.length} elements, exceeding maximum limit of ${maxLen}. Processing has been capped at the first ${maxLen} elements.`;
    showWarningModal.value = true;
  }

  playing.value = false;
  stepsData.steps = buildSteps(currentApproach.value, arr);
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
  cleanupFns.push(initVResizer(vizResizerRef, vizHeight, 130, 360));
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

            <!-- Custom Input Text -->
            <div class="ll-input-group">
              <label>arr =</label>
              <input
                type="text"
                v-model="inputArrText"
                class="ll-text-input"
                placeholder="10, 9, 2, 5, 3, 7, 101, 18"
                @keyup.enter="applyInput"
              />
              <span class="ll-input-hint">(max {{ maxAllowedLength }} items)</span>
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
              <!-- Top Array Strip Display -->
              <div class="ll-house-banner">
                <div class="ll-house-title">
                  Input Array <code>arr[0...{{ (st.arr || []).length - 1 }}]</code>:
                </div>
                <div class="ll-house-strip">
                  <div
                    v-for="(val, idx) in st.arr"
                    :key="idx"
                    class="ll-house-card"
                    :class="{
                      'll-house-cur': idx === st.curI,
                      'll-house-prev': idx === st.prevIdx,
                      'll-house-compare': currentApproach === 'tabulation' && idx === st.j
                    }"
                  >
                    <div class="ll-house-val">{{ val }}</div>
                    <div class="ll-house-idx">
                      <span v-if="idx === st.curI" class="ll-idx-tag ll-tag-cur">i</span>
                      <span v-else-if="idx === st.prevIdx" class="ll-idx-tag ll-tag-prev">prev</span>
                      <span v-else-if="currentApproach === 'tabulation' && idx === st.j" class="ll-idx-tag ll-tag-comp">j</span>
                      <span v-else>[{{ idx }}]</span>
                    </div>
                  </div>
                </div>
              </div>

              <!-- Primary Visualization Panel -->
              <div class="ll-viz-wrap" :style="{ height: vizHeight + 'px' }">
                <div class="ll-perm-area">
                  <!-- Metrics Chips Bar -->
                  <div class="ll-ptrs">
                    <div class="ll-ptr-chip">Array Length n = <b class="ll-c-blue">{{ (st.arr || []).length }}</b></div>
                    <div class="ll-ptr-chip" v-if="currentApproach === 'recursion' || currentApproach === 'memoization'">
                      Calls: <b class="ll-c-orange">{{ st.totalCalls || 0 }}</b>
                    </div>
                    <div class="ll-ptr-chip" v-if="currentApproach === 'memoization'">
                      Cache Hits: <b class="ll-c-purple">{{ st.cacheHits || 0 }}</b>
                    </div>
                    <div class="ll-ptr-chip" v-if="currentApproach === 'tabulation'">
                      Max LIS Length: <b class="ll-c-green">{{ st.maxLIS || 1 }}</b>
                    </div>
                    <div class="ll-ptr-chip" v-if="st.currentReturn !== null && st.currentReturn !== undefined">
                      Return: <b class="ll-c-green">{{ st.currentReturn }}</b>
                    </div>
                  </div>

                  <!-- Diagram 1: DP Tabulation (Bottom-Up 1D DP Array) -->
                  <div v-if="currentApproach === 'tabulation'" class="ll-dp-tab-view">
                    <div class="ll-section-caption">
                      <span>1D DP Array Strip &mdash; <code>dp[i]</code> = Length of LIS ending at index <code>i</code></span>
                      <span v-if="st.isStrictlyGreater" class="ll-calc-pill">
                        arr[{{ st.i }}] &gt; arr[{{ st.j }}] &rarr; dp[{{ st.i }}] = max(dp[{{ st.i }}], 1 + dp[{{ st.j }}])
                      </span>
                    </div>

                    <div class="ll-arr-track">
                      <template v-for="cell in st.dpCells" :key="cell.idx">
                        <div class="ll-arr-cell-wrap">
                          <!-- Pointer Tag Above Cell -->
                          <div class="ll-ptr-tag-wrap">
                            <span v-if="cell.idx === st.j" class="ll-ptr-lbl ll-lbl-orange">&darr; j</span>
                            <span v-else-if="cell.idx === st.i" class="ll-ptr-lbl ll-lbl-green">&darr; i</span>
                          </div>

                          <!-- Flat Box -->
                          <div
                            class="ll-arr-box"
                            :class="{
                              'll-box-cur': cell.status === 'active' || cell.idx === st.i,
                              'll-box-comp': cell.status === 'compare' || cell.idx === st.j,
                              'll-box-found': cell.status === 'computed',
                              'll-box-uncalc': cell.status === 'uncalculated'
                            }"
                          >
                            {{ cell.val }}
                          </div>
                          <div class="ll-arr-idx">dp[{{ cell.idx }}]</div>
                        </div>
                      </template>
                    </div>
                  </div>

                  <!-- Diagram 2: Recursion & Memoization Decision Tree -->
                  <div v-else class="ll-tree-container">
                    <div class="ll-section-caption">
                      <span>{{ currentApproach === 'recursion' ? 'Recursive Include/Exclude Decision Tree O(2^n)' : 'Pruned Memoization Decision Tree O(n^2)' }}</span>
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
                              'll-edge-take': edge.branchType === 'take',
                              'll-edge-nottake': edge.branchType === 'nottake'
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
                            <!-- Cache Hit Badge Above Node -->
                            <g v-if="node.state === 'cache-hit'" transform="translate(0, -17)">
                              <rect x="-25" y="-6" width="50" height="12" rx="3" fill="#9333ea" stroke="#7e22ce" stroke-width="0.5" />
                              <text x="0" y="0" text-anchor="middle" class="ll-badge-text-purple">Cache Hit O(1)</text>
                            </g>

                            <!-- Node Outer Card -->
                            <rect
                              x="-25"
                              y="-12"
                              width="50"
                              height="24"
                              rx="4"
                              class="ll-node-rect"
                              :class="{
                                'll-node-active': node.id === st.activeNodeId,
                                'll-node-solved': node.state === 'solved',
                                'll-node-cachehit': node.state === 'cache-hit'
                              }"
                            />
                            <!-- Node Text: lis(i, prev) -->
                            <text x="0" y="-3" text-anchor="middle" class="ll-node-text-call">
                              lis({{ node.i }},{{ node.prevIdx }})
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

                      <!-- 2D Memoization Cache Matrix Strip (Only in Memoization Mode) -->
                      <div v-if="currentApproach === 'memoization' && st.memo" class="ll-memo-strip-wrap">
                        <div class="ll-memo-title">Memoization 2D Cache Matrix &mdash; <code>memo[i][prevIdx + 1]</code>:</div>
                        <div class="ll-memo-grid">
                          <div v-for="(row, rIdx) in st.memo" :key="rIdx" class="ll-memo-row">
                            <span class="ll-memo-row-lbl">i={{ rIdx }}:</span>
                            <div v-for="(cVal, cIdx) in row" :key="cIdx" class="ll-memo-cell-wrap">
                              <div
                                class="ll-memo-cell"
                                :class="{
                                  'll-memo-hit': cVal !== -1,
                                  'll-memo-empty': cVal === -1
                                }"
                              >
                                {{ cVal }}
                              </div>
                              <span class="ll-memo-idx">p={{ cIdx - 1 }}</span>
                            </div>
                          </div>
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
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-cur"></span>Current Index (i)</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-purple"></span>Comparison Index (j)</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-new"></span>Computed LIS Length</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-uncalc"></span>Uncalculated</span>
                </template>
                <template v-else-if="currentApproach === 'memoization'">
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-cur"></span>Active Call</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-new"></span>Solved &amp; Cached</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-purple"></span>Cache Hit (O(1) Return)</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-uncalc"></span>Uncalled</span>
                </template>
                <template v-else>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-cur"></span>Active Decision</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-new"></span>Solved Branch</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-uncalc"></span>Pending</span>
                </template>
              </div>

              <!-- Variable Frames & Call Stack Area -->
              <div class="ll-table-area" :style="{ height: tableHeight + 'px' }">
                <div class="ll-table-title">Variable frames &mdash; innermost = current</div>
                <div class="ll-stack-line">
                  <template v-if="st.frames && st.frames.length">
                    <div
                      v-for="(f, depth) in st.frames"
                      :key="depth"
                      class="ll-frame"
                      :class="{ 'll-frame-cur': depth === st.frames.length - 1 }"
                      :style="{ marginLeft: depth * 12 + 'px' }"
                    >
                      {{ f.name }}(<span class="ll-fname">{{ f.args }}</span>)<span v-if="depth === st.frames.length - 1" class="ll-now"> &#9668; current</span>
                    </div>
                  </template>
                  <template v-else>&mdash;</template>
                </div>
              </div>

              <!-- Vertical Resizer 2 for Stack Panel -->
              <div class="ll-vresizer" ref="tableResizerRef"></div>

              <!-- Explanatory Step Badge -->
              <div class="ll-badge-wrap">
                <div
                  class="ll-badge"
                  :class="{
                    'll-badge-error': s.badgeType === 'warn',
                    'll-badge-success': s.badgeType === 'success'
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
                  <h3>Why Longest Increasing Subsequence? (Faculty Insight)</h3>
                  <p>
                    The LIS problem is a fundamental DP problem that models ordering and growth patterns:
                  </p>
                  <ul>
                    <li>
                      <strong>1. Optimal Substructure:</strong> The longest increasing sequence ending at index <code>i</code> is formed by extending the optimal LIS ending at some previous index <code>j &lt; i</code> where <code>arr[i] &gt; arr[j]</code>.
                    </li>
                    <li>
                      <strong>2. Overlapping Subproblems:</strong> Naive recursion evaluates identical decision branches <code>lis(i, prevIdx)</code> repeatedly across different paths.
                    </li>
                  </ul>

                  <h3>Approaches Comparison</h3>
                  <table class="ll-complexity-table">
                    <thead>
                      <tr>
                        <th>Approach</th>
                        <th>Time Complexity</th>
                        <th>Space Complexity</th>
                        <th>Subproblem Handling</th>
                      </tr>
                    </thead>
                    <tbody>
                      <tr>
                        <td><strong>Naive Recursion</strong></td>
                        <td>O(2<sup>n</sup>) Exponential</td>
                        <td>O(n) Stack</td>
                        <td><span class="ll-c-red" style="font-weight:700">Recomputes All</span></td>
                      </tr>
                      <tr>
                        <td><strong>DP Memoization</strong></td>
                        <td>O(n<sup>2</sup>) Quadratic</td>
                        <td>O(n<sup>2</sup>) 2D Table + Stack</td>
                        <td><span class="ll-c-purple" style="font-weight:700">O(1) Cache Hit</span></td>
                      </tr>
                      <tr>
                        <td><strong>DP Tabulation</strong></td>
                        <td>O(n<sup>2</sup>) Quadratic</td>
                        <td>O(n) 1D Table</td>
                        <td><span class="ll-c-green" style="font-weight:700">Iterative 1D Pass</span></td>
                      </tr>
                      <tr>
                        <td><strong>Patience Sorting (Binary Search)</strong></td>
                        <td>O(n log n) Optimal</td>
                        <td>O(n) Tails Array</td>
                        <td><span class="ll-c-green" style="font-weight:700">Binary Search Insertion</span></td>
                      </tr>
                    </tbody>
                  </table>

                  <h3>Recurrence Relation</h3>
                  <p class="ll-math-box">
                    dp[i] = 1 + max({ dp[j] | 0 &le; j &lt; i and arr[i] &gt; arr[j] } &cup; {0})
                  </p>
                  <p>
                    The final answer is <code>max(dp[0...n-1])</code>.
                  </p>
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
                    <span class="ll-modal-tag-entered">Entered Length: {{ requestedLength }}</span>
                    <span class="ll-modal-arrow">&rarr;</span>
                    <span class="ll-modal-tag-applied">Capped at Max: {{ warningModalLimit }}</span>
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
.ll-input-group label { font-size: 11px; color: var(--muted); font-weight: 700; }
.ll-text-input { background: var(--surface); border: 1px solid var(--border2); color: var(--text); border-radius: var(--radius-sm); padding: 3px 6px; font-size: 11px; font-family: monospace; width: 140px; }
.ll-text-input:focus { outline: none; border-color: var(--coral); box-shadow: 0 0 0 3px rgba(240,77,77,.1); }
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

/* Top Array Banner */
.ll-house-banner { padding: 4px 10px; background: var(--surface2); border-bottom: 1px solid var(--border); flex-shrink: 0; }
.ll-house-title { font-size: 10.5px; font-weight: 700; color: var(--text2); margin-bottom: 2px; }
.ll-house-strip { display: flex; gap: 6px; flex-wrap: wrap; }
.ll-house-card { width: 44px; height: 38px; display: flex; flex-direction: column; align-items: center; justify-content: center; background: var(--surface); border: 1.5px solid var(--border2); border-radius: var(--radius-sm); box-shadow: var(--shadow-sm); transition: all 0.2s ease; }
.ll-house-cur { border-color: #f59e0b !important; background: #fffbeb !important; transform: translateY(-2px); box-shadow: 0 0 0 3px rgba(245,158,11,0.25) !important; }
.ll-house-prev { border-color: #3b82f6 !important; background: #eff6ff !important; transform: translateY(-2px); }
.ll-house-compare { border-color: #a855f7 !important; background: #f3e8ff !important; transform: translateY(-2px); }
.ll-house-val { font-size: 13px; font-weight: 800; font-family: monospace; color: var(--text); }
.ll-house-idx { font-size: 9px; color: var(--muted); font-family: monospace; margin-top: 1px; }
.ll-idx-tag { font-weight: 800; padding: 0 3px; border-radius: 3px; }
.ll-tag-cur { color: #b45309; background: #fef3c7; }
.ll-tag-prev { color: #1d4ed8; background: #dbeafe; }
.ll-tag-comp { color: #6b21a8; background: #f3e8ff; }

/* Left Visualization Wrappers */
.ll-viz-wrap { flex-shrink: 0; background: var(--surface); border-bottom: 1px solid var(--border); position: relative; overflow-x: auto; overflow-y: auto; }
.ll-perm-area { display: flex; flex-direction: column; align-items: stretch; min-height: 100%; width: 100%; min-width: 0; box-sizing: border-box; }
.ll-ptrs { display: flex; gap: 6px; flex-wrap: wrap; padding: 4px 10px; min-height: 28px; width: 100%; box-sizing: border-box; min-width: 0; }
.ll-ptr-chip { background: var(--surface2); border: 1px solid var(--border); border-radius: var(--radius-sm); padding: 2px 7px; font-size: 11px; font-family: monospace; box-shadow: var(--shadow-sm); white-space: nowrap; flex-shrink: 0; }

.ll-c-blue { color: var(--blue); } .ll-c-orange { color: var(--orange); } .ll-c-green { color: var(--green); } .ll-c-purple { color: var(--purple); } .ll-c-red { color: var(--red); }

.ll-section-caption { display: flex; justify-content: space-between; align-items: center; padding: 3px 10px; font-size: 10.5px; font-weight: 700; color: var(--text2); background: var(--surface2); border-top: 1px solid var(--border); border-bottom: 1px solid var(--border); }
.ll-calc-pill { background: #dcfce7; color: #15803d; padding: 1px 6px; border-radius: 10px; font-size: 10px; font-family: monospace; font-weight: 700; }
.ll-memo-badge-info { background: var(--purple-light); color: var(--purple); padding: 1px 6px; border-radius: 10px; font-size: 10px; }

/* Diagram 1: DP Tabulation 1D Array Strip */
.ll-dp-tab-view { display: flex; flex-direction: column; padding: 6px 10px; }
.ll-arr-track { display: flex; align-items: flex-start; flex-wrap: wrap; padding: 6px 4px; gap: 8px; width: 100%; box-sizing: border-box; min-width: 0; }
.ll-arr-cell-wrap { display: flex; flex-direction: column; align-items: center; min-width: 0; }
.ll-ptr-tag-wrap { height: 22px; display: flex; align-items: flex-end; justify-content: center; margin-bottom: 2px; }
.ll-ptr-lbl { font-size: 10.5px; font-weight: 800; font-family: 'Consolas', 'Fira Code', monospace; display: inline-flex; align-items: center; line-height: 1; white-space: nowrap; animation: ll-pop 0.2s ease; }
.ll-lbl-orange { color: #f97316; } .ll-lbl-green { color: #10b981; }

.ll-arr-box { width: 44px; height: 44px; display: flex; align-items: center; justify-content: center; border: 2px solid var(--border2); border-radius: var(--radius); background: var(--surface); color: var(--text); font-weight: 700; font-size: 14px; box-shadow: var(--shadow-sm); transition: all 0.25s ease; animation: ll-pop .3s ease; }
.ll-box-cur { border-color: #f59e0b !important; background: #fef3c7 !important; color: #92400e !important; box-shadow: 0 0 0 3px rgba(245, 158, 11, 0.25) !important; transform: translateY(-2px); }
.ll-box-comp { border-color: #a855f7 !important; background: #f3e8ff !important; color: #6b21a8 !important; transform: translateY(-2px); }
.ll-box-found { border-color: #10b981 !important; background: #dcfce7 !important; color: #065f46 !important; box-shadow: 0 0 0 3px rgba(16, 185, 129, 0.2) !important; }
.ll-box-uncalc { border: 2px dashed var(--border2) !important; background: var(--surface2) !important; color: var(--muted) !important; }
.ll-arr-idx { font-size: 10px; color: var(--muted); margin-top: 3px; font-family: 'Consolas', monospace; font-weight: 600; }

/* Diagram 2: Recursion & Memoization Decision Tree */
.ll-tree-container { display: flex; flex-direction: column; width: 100%; height: 100%; min-height: 0; }
.ll-tree-scroll-area { flex: 1; overflow: auto; padding: 2px 6px 4px; display: flex; flex-direction: column; align-items: center; justify-content: flex-start; min-height: 0; width: 100%; box-sizing: border-box; }
.ll-tree-svg { display: block; flex-shrink: 0; max-width: 100%; margin: 0 auto; }
.ll-tree-edge { stroke: #cbd5e1; stroke-width: 1.8px; transition: stroke .2s; }
.ll-edge-take { stroke: #10b981; }
.ll-edge-nottake { stroke: #94a3b8; stroke-dasharray: 4, 3; }

.ll-tree-node-group { cursor: default; }
.ll-node-rect { fill: #ffffff; stroke: #cbd5e1; stroke-width: 1.4px; filter: drop-shadow(0 1px 2px rgba(0,0,0,0.05)); transition: all .25s ease; }
.ll-node-active { stroke: #f59e0b !important; stroke-width: 2.2px !important; fill: #fffbeb !important; filter: drop-shadow(0 0 5px rgba(245, 158, 11, 0.45)) !important; animation: ll-pulse 1.3s infinite ease-in-out; }
.ll-node-solved { stroke: #10b981 !important; fill: #dcfce7 !important; stroke-width: 1.6px !important; }
.ll-node-cachehit { stroke: #9333ea !important; stroke-width: 1.8px !important; fill: #f3e8ff !important; }

/* SVG Text Styling */
.ll-tree-svg text { font-family: 'Segoe UI', system-ui, sans-serif !important; user-select: none; }
.ll-node-text-call { font-family: 'Consolas', 'Fira Code', monospace !important; font-size: 9.5px !important; font-weight: 700 !important; fill: #1e293b !important; dominant-baseline: central !important; }
.ll-node-text-val { font-family: 'Consolas', 'Fira Code', monospace !important; font-size: 9px !important; font-weight: 700 !important; fill: #64748b !important; dominant-baseline: central !important; }
.ll-val-active { fill: #b45309 !important; }
.ll-val-solved { fill: #047857 !important; }
.ll-val-hit { fill: #7e22ce !important; }

.ll-badge-text-purple { font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif !important; font-size: 6.8px !important; font-weight: 700 !important; fill: #ffffff !important; dominant-baseline: central !important; }

/* 2D Memoization Matrix Strip */
.ll-memo-strip-wrap { width: 100%; padding: 4px 8px 6px; border-top: 1px dashed var(--border); background: var(--surface2); flex-shrink: 0; margin-top: 4px; box-sizing: border-box; }
.ll-memo-title { font-size: 10px; font-weight: 700; color: var(--text2); margin-bottom: 3px; }
.ll-memo-grid { display: flex; flex-direction: column; gap: 3px; overflow-x: auto; }
.ll-memo-row { display: flex; align-items: center; gap: 4px; }
.ll-memo-row-lbl { font-size: 9.5px; font-family: monospace; font-weight: 700; color: var(--muted); width: 32px; flex-shrink: 0; }
.ll-memo-cell-wrap { display: flex; flex-direction: column; align-items: center; }
.ll-memo-cell { width: 28px; height: 24px; display: flex; align-items: center; justify-content: center; font-size: 10.5px; font-family: monospace; font-weight: 700; border-radius: 4px; border: 1px solid var(--border); background: var(--surface); color: var(--text); }
.ll-memo-hit { background: #f3e8ff !important; border-color: #a855f7 !important; color: #6b21a8 !important; }
.ll-memo-empty { color: var(--muted); border-style: dashed; }
.ll-memo-idx { font-size: 8.5px; color: var(--muted); font-family: monospace; }

/* Resizers & Legend */
.ll-vresizer { height: 5px; cursor: row-resize; background: var(--border); flex-shrink: 0; transition: background .15s; position: relative; z-index: 20; }
.ll-vresizer:hover, .ll-vresizer.drag { background: var(--coral); }

.ll-legend { display: flex; flex-wrap: wrap; gap: 4px 10px; padding: 4px 10px; border-bottom: 1px solid var(--border); flex-shrink: 0; background: var(--surface2); }
.ll-leg { display: flex; align-items: center; gap: 4px; font-size: 10.5px; color: var(--text2); font-weight: 500; }
.ll-legdot { width: 9px; height: 9px; border-radius: 2px; flex-shrink: 0; display: inline-block; }
.ll-legdot-cur { background: #fef3c7; border: 1.5px solid #f59e0b; }
.ll-legdot-new { background: #dcfce7; border: 1.5px solid #10b981; }
.ll-legdot-purple { background: #f3e8ff; border: 1.5px solid #9333ea; }
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
.ll-info-scroll ul { margin: 0 0 10px; padding-left: 18px; }
.ll-info-scroll li { margin-bottom: 4px; }

.ll-complexity-table { width: 100%; border-collapse: collapse; font-size: 11px; margin: 8px 0 12px; }
.ll-complexity-table th, .ll-complexity-table td { padding: 5px 8px; border: 1px solid var(--border); text-align: left; }
.ll-complexity-table th { background: var(--surface2); color: var(--text); font-weight: 700; }

.ll-math-box { background: var(--surface2); padding: 6px 12px; border-radius: var(--radius-sm); border-left: 3px solid var(--blue); font-family: monospace; font-size: 11.5px; margin: 6px 0; color: var(--text); }

/* Footer */
.ll-footer { padding: 4px 12px; background: var(--surface); border-top: 1px solid var(--border); display: flex; align-items: center; justify-content: space-between; font-size: 11px; color: var(--muted); flex-shrink: 0; }
.ll-speed-wrap { display: flex; align-items: center; gap: 6px; }
.ll-speed-wrap input { width: 80px; accent-color: var(--coral); }

/* Warning Modal */
.ll-modal-backdrop { position: fixed; top: 0; left: 0; width: 100vw; height: 100vh; background: rgba(15, 23, 42, 0.45); backdrop-filter: blur(3px); z-index: 9999; display: flex; align-items: center; justify-content: center; padding: 16px; }
.ll-modal-card { background: #ffffff; width: 100%; max-width: 440px; border-radius: 12px; box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1), 0 10px 10px -5px rgba(0, 0, 0, 0.04); overflow: hidden; border: 1px solid var(--border); animation: ll-pop 0.25s cubic-bezier(0.16, 1, 0.3, 1); }
.ll-modal-header { display: flex; align-items: center; justify-content: space-between; padding: 14px 18px; background: #fff7ed; border-bottom: 1px solid #ffedd5; }
.ll-modal-title-wrap { display: flex; align-items: center; gap: 10px; }
.ll-modal-icon-badge { width: 32px; height: 32px; border-radius: 50%; background: #ffedd5; color: #c2410c; display: flex; align-items: center; justify-content: center; flex-shrink: 0; }
.ll-modal-svg-icon { width: 18px; height: 18px; }
.ll-modal-title { font-size: 14px; font-weight: 700; color: #9a3412; }
.ll-modal-close-btn { background: transparent; border: none; font-size: 20px; color: #9a3412; cursor: pointer; padding: 0; line-height: 1; }
.ll-modal-body { padding: 18px; color: var(--text2); font-size: 12.5px; line-height: 1.5; }
.ll-modal-badge-row { display: flex; align-items: center; gap: 8px; margin-bottom: 12px; }
.ll-modal-tag-entered { background: #fef2f2; color: #dc2626; padding: 2px 8px; border-radius: 4px; font-size: 11px; font-weight: 700; font-family: monospace; }
.ll-modal-arrow { color: var(--muted); font-weight: 700; }
.ll-modal-tag-applied { background: #f0fdf4; color: #16a34a; padding: 2px 8px; border-radius: 4px; font-size: 11px; font-weight: 700; font-family: monospace; }
.ll-modal-message { margin: 0; color: var(--text); }
.ll-modal-footer { padding: 12px 18px; background: var(--surface2); border-top: 1px solid var(--border); display: flex; justify-content: flex-end; }
.ll-modal-confirm-btn { background: var(--coral); color: #ffffff; border: none; padding: 7px 16px; border-radius: 6px; font-size: 12px; font-weight: 600; cursor: pointer; transition: filter 0.15s; }
.ll-modal-confirm-btn:hover { filter: brightness(1.08); }

.ll-modal-fade-enter-active, .ll-modal-fade-leave-active { transition: opacity 0.2s ease; }
.ll-modal-fade-enter-from, .ll-modal-fade-leave-to { opacity: 0; }
</style>
