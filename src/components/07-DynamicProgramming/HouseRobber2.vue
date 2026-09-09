<script setup>
import { ref, computed, reactive, onMounted, onUnmounted, nextTick } from 'vue';

/* ==================================================================== */
/* PROPS & COMPONENT CONFIGURATION                                      */
/* ==================================================================== */
const props = defineProps({
  topic: { type: String, default: 'Dynamic Programming' },
  subTopic: { type: String, default: 'House Robber II (Circular Houses)' }
});

/* ==================================================================== */
/* APPROACHES                                                            */
/* ==================================================================== */
const APPROACHES = [
  { id: 'recursion',   label: 'Brute Force',  desc: 'O(2^n) Exponential — Run two sub-ranges, evaluate Rob vs Skip at each house' },
  { id: 'memoization', label: 'Memoization',  desc: 'O(n) Linear Time & O(n) Space — Top-Down with cache over two sub-ranges' },
  { id: 'tabulation',  label: 'Tabulation',   desc: 'O(n) Linear Time & O(n) Space — Bottom-Up 1D DP over two sub-ranges' },
];

/* ==================================================================== */
/* MULTI-LANGUAGE 100% EXECUTABLE CODE SPECIFICATIONS                   */
/* ==================================================================== */
const CODES = {
  recursion: {
    java: [
      ['',            'import java.util.Scanner;'],
      ['',            ''],
      ['',            'public class Main {'],
      ['h_entry',     '    static int rob(int[] nums, int i, int s) {'],
      ['h_base_neg',  '        if (i < s) {'],
      ['h_ret_neg',   '            return 0;'],
      ['',            '        }'],
      ['h_base_s',    '        if (i == s) {'],
      ['h_ret_s',     '            return nums[s];'],
      ['',            '        }'],
      ['h_rec_skip',  '        int skip = rob(nums, i - 1, s);'],
      ['h_rec_rob',   '        int robCurrent = nums[i] + rob(nums, i - 2, s);'],
      ['h_ret_max',   '        return Math.max(skip, robCurrent);'],
      ['',            '    }'],
      ['',            ''],
      ['m_entry',     '    static int robCircular(int[] nums) {'],
      ['m_get_n',     '        int n = nums.length;'],
      ['m_check_one', '        if (n == 1) {'],
      ['m_ret_one',   '            return nums[0];'],
      ['',            '        }'],
      ['m_case1',     '        int case1 = rob(nums, n - 2, 0);'],
      ['m_case2',     '        int case2 = rob(nums, n - 1, 1);'],
      ['m_ret_max2',  '        return Math.max(case1, case2);'],
      ['',            '    }'],
      ['',            ''],
      ['',            '    public static void main(String[] args) {'],
      ['',            '        Scanner sc = new Scanner(System.in);'],
      ['m_read_n',    '        int n = sc.nextInt();'],
      ['m_alloc_arr', '        int[] nums = new int[n];'],
      ['m_read_arr',  '        for (int i = 0; i < n; i++) {'],
      ['',            '            nums[i] = sc.nextInt();'],
      ['',            '        }'],
      ['m_call_rob',  '        int maxLoot = robCircular(nums);'],
      ['m_print',     '        System.out.println(maxLoot);'],
      ['m_done',      '    }'],
      ['',            '}']
    ],
    c: [
      ['',            '#include <stdio.h>'],
      ['',            '#define MAX(a, b) ((a) > (b) ? (a) : (b))'],
      ['',            ''],
      ['h_entry',     'int rob(int* nums, int i, int s) {'],
      ['h_base_neg',  '    if (i < s) {'],
      ['h_ret_neg',   '        return 0;'],
      ['',            '    }'],
      ['h_base_s',    '    if (i == s) {'],
      ['h_ret_s',     '        return nums[s];'],
      ['',            '    }'],
      ['h_rec_skip',  '    int skip = rob(nums, i - 1, s);'],
      ['h_rec_rob',   '    int robCurrent = nums[i] + rob(nums, i - 2, s);'],
      ['h_ret_max',   '    return MAX(skip, robCurrent);'],
      ['',            '}'],
      ['',            ''],
      ['m_entry',     'int robCircular(int* nums, int n) {'],
      ['m_get_n',     '    int len = n;'],
      ['m_check_one', '    if (len == 1) {'],
      ['m_ret_one',   '        return nums[0];'],
      ['',            '    }'],
      ['m_case1',     '    int case1 = rob(nums, len - 2, 0);'],
      ['m_case2',     '    int case2 = rob(nums, len - 1, 1);'],
      ['m_ret_max2',  '    return MAX(case1, case2);'],
      ['',            '}'],
      ['',            ''],
      ['',            'int main() {'],
      ['m_read_n',    '    int n; scanf("%d", &n);'],
      ['m_alloc_arr', '    int nums[n];'],
      ['m_read_arr',  '    for (int i = 0; i < n; i++) {'],
      ['',            '        scanf("%d", &nums[i]);'],
      ['',            '    }'],
      ['m_call_rob',  '    int maxLoot = robCircular(nums, n);'],
      ['m_print',     '    printf(\"%d\\n\", maxLoot);'],
      ['m_done',      '    return 0;'],
      ['',            '}']
    ],
    cpp: [
      ['',            '#include <iostream>'],
      ['',            '#include <vector>'],
      ['',            '#include <algorithm>'],
      ['',            'using namespace std;'],
      ['',            ''],
      ['h_entry',     'int rob(const vector<int>& nums, int i, int s) {'],
      ['h_base_neg',  '    if (i < s) {'],
      ['h_ret_neg',   '        return 0;'],
      ['',            '    }'],
      ['h_base_s',    '    if (i == s) {'],
      ['h_ret_s',     '        return nums[s];'],
      ['',            '    }'],
      ['h_rec_skip',  '    int skip = rob(nums, i - 1, s);'],
      ['h_rec_rob',   '    int robCurrent = nums[i] + rob(nums, i - 2, s);'],
      ['h_ret_max',   '    return max(skip, robCurrent);'],
      ['',            '}'],
      ['',            ''],
      ['m_entry',     'int robCircular(const vector<int>& nums) {'],
      ['m_get_n',     '    int n = nums.size();'],
      ['m_check_one', '    if (n == 1) {'],
      ['m_ret_one',   '        return nums[0];'],
      ['',            '    }'],
      ['m_case1',     '    int case1 = rob(nums, n - 2, 0);'],
      ['m_case2',     '    int case2 = rob(nums, n - 1, 1);'],
      ['m_ret_max2',  '    return max(case1, case2);'],
      ['',            '}'],
      ['',            ''],
      ['',            'int main() {'],
      ['m_read_n',    '    int n; cin >> n;'],
      ['m_alloc_arr', '    vector<int> nums(n);'],
      ['m_read_arr',  '    for (int i = 0; i < n; i++) {'],
      ['',            '        cin >> nums[i];'],
      ['',            '    }'],
      ['m_call_rob',  '    int maxLoot = robCircular(nums);'],
      ['m_print',     '    cout << maxLoot << endl;'],
      ['m_done',      '    return 0;'],
      ['',            '}']
    ],
    python: [
      ['',            'import sys'],
      ['',            ''],
      ['h_entry',     'def rob(nums, i, s):'],
      ['h_base_neg',  '    if i < s:'],
      ['h_ret_neg',   '        return 0'],
      ['h_base_s',    '    if i == s:'],
      ['h_ret_s',     '        return nums[s]'],
      ['h_rec_skip',  '    skip = rob(nums, i - 1, s)'],
      ['h_rec_rob',   '    rob_current = nums[i] + rob(nums, i - 2, s)'],
      ['h_ret_max',   '    return max(skip, rob_current)'],
      ['',            ''],
      ['m_entry',     'def rob_circular(nums):'],
      ['m_get_n',     '    n = len(nums)'],
      ['m_check_one',  '    if n == 1:'],
      ['m_ret_one',    '        return nums[0]'],
      ['m_case1',      '    case1 = rob(nums, n - 2, 0)'],
      ['m_case2',      '    case2 = rob(nums, n - 1, 1)'],
      ['m_ret_max2',   '    return max(case1, case2)'],
      ['',            ''],
      ['',            'if __name__ == "__main__":'],
      ['',            '    tokens = sys.stdin.read().split()'],
      ['m_read_n',    '    n = int(tokens[0])'],
      ['m_alloc_arr', '    nums = [0] * n'],
      ['m_read_arr',  '    for i in range(n): nums[i] = int(tokens[i + 1])'],
      ['m_call_rob',  '    max_loot = rob_circular(nums)'],
      ['m_print',     '    print(max_loot)'],
      ['m_done',      '    sys.exit(0)']
    ],
    javascript: [
      ['',            'const fs = require("fs");'],
      ['',            ''],
      ['h_entry',     'function rob(nums, i, s) {'],
      ['h_base_neg',  '    if (i < s) {'],
      ['h_ret_neg',   '        return 0;'],
      ['',            '    }'],
      ['h_base_s',    '    if (i === s) {'],
      ['h_ret_s',     '        return nums[s];'],
      ['',            '    }'],
      ['h_rec_skip',  '    const skip = rob(nums, i - 1, s);'],
      ['h_rec_rob',   '    const robCurrent = nums[i] + rob(nums, i - 2, s);'],
      ['h_ret_max',   '    return Math.max(skip, robCurrent);'],
      ['',            '}'],
      ['',            ''],
      ['m_entry',     'function robCircular(nums) {'],
      ['m_get_n',     '    const n = nums.length;'],
      ['m_check_one', '    if (n === 1) {'],
      ['m_ret_one',   '        return nums[0];'],
      ['',            '    }'],
      ['m_case1',     '    const case1 = rob(nums, n - 2, 0);'],
      ['m_case2',     '    const case2 = rob(nums, n - 1, 1);'],
      ['m_ret_max2',  '    return Math.max(case1, case2);'],
      ['',            '}'],
      ['',            ''],
      ['',            'const tokens = fs.readFileSync(0, "utf-8").trim().split(/\\s+/);'],
      ['m_read_n',    'const n = parseInt(tokens[0], 10);'],
      ['m_alloc_arr', 'const nums = new Array(n);'],
      ['m_read_arr',  'for (let i = 0; i < n; i++) { nums[i] = parseInt(tokens[i + 1], 10); }'],
      ['m_call_rob',  'const maxLoot = robCircular(nums);'],
      ['m_print',     'console.log(maxLoot);'],
      ['m_done',      'process.exit(0);']
    ]
  },
  memoization: {
    java: [
      ['',            'import java.util.Scanner;'],
      ['',            'import java.util.Arrays;'],
      ['',            ''],
      ['',            'public class Main {'],
      ['h_entry',     '    static int solve(int[] nums, int i, int s, int[] memo) {'],
      ['h_base_neg',  '        if (i < s) {'],
      ['h_ret_neg',   '            return 0;'],
      ['',            '        }'],
      ['h_base_s',    '        if (i == s) {'],
      ['h_ret_s',     '            return nums[s];'],
      ['',            '        }'],
      ['h_memo_check','        if (memo[i - s] != -1) {'],
      ['h_ret_memo',  '            return memo[i - s];'],
      ['',            '        }'],
      ['h_rec_skip',  '        int skip = solve(nums, i - 1, s, memo);'],
      ['h_rec_rob',   '        int robCurrent = nums[i] + solve(nums, i - 2, s, memo);'],
      ['h_store_memo','        memo[i - s] = Math.max(skip, robCurrent);'],
      ['h_ret_max',   '        return memo[i - s];'],
      ['',            '    }'],
      ['',            ''],
      ['m_entry',     '    static int robCircular(int[] nums) {'],
      ['m_get_n',     '        int n = nums.length;'],
      ['m_check_one', '        if (n == 1) {'],
      ['m_ret_one',   '            return nums[0];'],
      ['',            '        }'],
      ['m_alloc_m1',  '        int[] memo1 = new int[n - 1];'],
      ['',            '        Arrays.fill(memo1, -1);'],
      ['m_case1',     '        int case1 = solve(nums, n - 2, 0, memo1);'],
      ['m_alloc_m2',  '        int[] memo2 = new int[n - 1];'],
      ['',            '        Arrays.fill(memo2, -1);'],
      ['m_case2',     '        int case2 = solve(nums, n - 1, 1, memo2);'],
      ['m_ret_max2',  '        return Math.max(case1, case2);'],
      ['',            '    }'],
      ['',            ''],
      ['',            '    public static void main(String[] args) {'],
      ['',            '        Scanner sc = new Scanner(System.in);'],
      ['m_read_n',    '        int n = sc.nextInt();'],
      ['m_alloc_arr', '        int[] nums = new int[n];'],
      ['m_read_arr',  '        for (int i = 0; i < n; i++) {'],
      ['',            '            nums[i] = sc.nextInt();'],
      ['',            '        }'],
      ['m_call_rob',  '        int maxLoot = robCircular(nums);'],
      ['m_print',     '        System.out.println(maxLoot);'],
      ['m_done',      '    }'],
      ['',            '}']
    ],
    c: [
      ['',            '#include <stdio.h>'],
      ['',            '#include <string.h>'],
      ['',            '#define MAX(a, b) ((a) > (b) ? (a) : (b))'],
      ['',            ''],
      ['h_entry',     'int solve(int* nums, int i, int s, int* memo) {'],
      ['h_base_neg',  '    if (i < s) {'],
      ['h_ret_neg',   '        return 0;'],
      ['',            '    }'],
      ['h_base_s',    '    if (i == s) {'],
      ['h_ret_s',     '        return nums[s];'],
      ['',            '    }'],
      ['h_memo_check','    if (memo[i - s] != -1) {'],
      ['h_ret_memo',  '        return memo[i - s];'],
      ['',            '    }'],
      ['h_rec_skip',  '    int skip = solve(nums, i - 1, s, memo);'],
      ['h_rec_rob',   '    int robCurrent = nums[i] + solve(nums, i - 2, s, memo);'],
      ['h_store_memo','    memo[i - s] = MAX(skip, robCurrent);'],
      ['h_ret_max',   '    return memo[i - s];'],
      ['',            '}'],
      ['',            ''],
      ['m_entry',     'int robCircular(int* nums, int n) {'],
      ['m_get_n',     '    int len = n;'],
      ['m_check_one', '    if (len == 1) {'],
      ['m_ret_one',   '        return nums[0];'],
      ['',            '    }'],
      ['m_alloc_m1',  '    int memo1[len - 1];'],
      ['',            '    memset(memo1, -1, sizeof(memo1));'],
      ['m_case1',     '    int case1 = solve(nums, len - 2, 0, memo1);'],
      ['m_alloc_m2',  '    int memo2[len - 1];'],
      ['',            '    memset(memo2, -1, sizeof(memo2));'],
      ['m_case2',     '    int case2 = solve(nums, len - 1, 1, memo2);'],
      ['m_ret_max2',  '    return MAX(case1, case2);'],
      ['',            '}'],
      ['',            ''],
      ['',            'int main() {'],
      ['m_read_n',    '    int n; scanf("%d", &n);'],
      ['m_alloc_arr', '    int nums[n];'],
      ['m_read_arr',  '    for (int i = 0; i < n; i++) {'],
      ['',            '        scanf("%d", &nums[i]);'],
      ['',            '    }'],
      ['m_call_rob',  '    int maxLoot = robCircular(nums, n);'],
      ['m_print',     '    printf(\"%d\\n\", maxLoot);'],
      ['m_done',      '    return 0;'],
      ['',            '}']
    ],
    cpp: [
      ['',            '#include <iostream>'],
      ['',            '#include <vector>'],
      ['',            '#include <algorithm>'],
      ['',            'using namespace std;'],
      ['',            ''],
      ['h_entry',     'int solve(const vector<int>& nums, int i, int s, vector<int>& memo) {'],
      ['h_base_neg',  '    if (i < s) {'],
      ['h_ret_neg',   '        return 0;'],
      ['',            '    }'],
      ['h_base_s',    '    if (i == s) {'],
      ['h_ret_s',     '        return nums[s];'],
      ['',            '    }'],
      ['h_memo_check','    if (memo[i - s] != -1) {'],
      ['h_ret_memo',  '        return memo[i - s];'],
      ['',            '    }'],
      ['h_rec_skip',  '    int skip = solve(nums, i - 1, s, memo);'],
      ['h_rec_rob',   '    int robCurrent = nums[i] + solve(nums, i - 2, s, memo);'],
      ['h_store_memo','    memo[i - s] = max(skip, robCurrent);'],
      ['h_ret_max',   '    return memo[i - s];'],
      ['',            '}'],
      ['',            ''],
      ['m_entry',     'int robCircular(const vector<int>& nums) {'],
      ['m_get_n',     '    int n = nums.size();'],
      ['m_check_one', '    if (n == 1) {'],
      ['m_ret_one',   '        return nums[0];'],
      ['',            '    }'],
      ['m_alloc_m1',  '    vector<int> memo1(n - 1, -1);'],
      ['m_case1',     '    int case1 = solve(nums, n - 2, 0, memo1);'],
      ['m_alloc_m2',  '    vector<int> memo2(n - 1, -1);'],
      ['m_case2',     '    int case2 = solve(nums, n - 1, 1, memo2);'],
      ['m_ret_max2',  '    return max(case1, case2);'],
      ['',            '}'],
      ['',            ''],
      ['',            'int main() {'],
      ['m_read_n',    '    int n; cin >> n;'],
      ['m_alloc_arr', '    vector<int> nums(n);'],
      ['m_read_arr',  '    for (int i = 0; i < n; i++) {'],
      ['',            '        cin >> nums[i];'],
      ['',            '    }'],
      ['m_call_rob',  '    int maxLoot = robCircular(nums);'],
      ['m_print',     '    cout << maxLoot << endl;'],
      ['m_done',      '    return 0;'],
      ['',            '}']
    ],
    python: [
      ['',            'import sys'],
      ['',            ''],
      ['h_entry',     'def solve(nums, i, s, memo):'],
      ['h_base_neg',  '    if i < s:'],
      ['h_ret_neg',   '        return 0'],
      ['h_base_s',    '    if i == s:'],
      ['h_ret_s',     '        return nums[s]'],
      ['h_memo_check','    if memo[i - s] != -1:'],
      ['h_ret_memo',  '        return memo[i - s]'],
      ['h_rec_skip',  '    skip = solve(nums, i - 1, s, memo)'],
      ['h_rec_rob',   '    rob_current = nums[i] + solve(nums, i - 2, s, memo)'],
      ['h_store_memo','    memo[i - s] = max(skip, rob_current)'],
      ['h_ret_max',   '    return memo[i - s]'],
      ['',            ''],
      ['m_entry',     'def rob_circular(nums):'],
      ['m_get_n',     '    n = len(nums)'],
      ['m_check_one',  '    if n == 1:'],
      ['m_ret_one',    '        return nums[0]'],
      ['m_alloc_m1',   '    memo1 = [-1] * (n - 1)'],
      ['m_case1',      '    case1 = solve(nums, n - 2, 0, memo1)'],
      ['m_alloc_m2',   '    memo2 = [-1] * (n - 1)'],
      ['m_case2',      '    case2 = solve(nums, n - 1, 1, memo2)'],
      ['m_ret_max2',   '    return max(case1, case2)'],
      ['',            ''],
      ['',            'if __name__ == "__main__":'],
      ['',            '    tokens = sys.stdin.read().split()'],
      ['m_read_n',    '    n = int(tokens[0])'],
      ['m_alloc_arr', '    nums = [0] * n'],
      ['m_read_arr',  '    for i in range(n): nums[i] = int(tokens[i + 1])'],
      ['m_call_rob',  '    max_loot = rob_circular(nums)'],
      ['m_print',     '    print(max_loot)'],
      ['m_done',      '    sys.exit(0)']
    ],
    javascript: [
      ['',            'const fs = require("fs");'],
      ['',            ''],
      ['h_entry',     'function solve(nums, i, s, memo) {'],
      ['h_base_neg',  '    if (i < s) {'],
      ['h_ret_neg',   '        return 0;'],
      ['',            '    }'],
      ['h_base_s',    '    if (i === s) {'],
      ['h_ret_s',     '        return nums[s];'],
      ['',            '    }'],
      ['h_memo_check','    if (memo[i - s] !== -1) {'],
      ['h_ret_memo',  '        return memo[i - s];'],
      ['',            '    }'],
      ['h_rec_skip',  '    const skip = solve(nums, i - 1, s, memo);'],
      ['h_rec_rob',   '    const robCurrent = nums[i] + solve(nums, i - 2, s, memo);'],
      ['h_store_memo','    memo[i - s] = Math.max(skip, robCurrent);'],
      ['h_ret_max',   '    return memo[i - s];'],
      ['',            '}'],
      ['',            ''],
      ['m_entry',     'function robCircular(nums) {'],
      ['m_get_n',     '    const n = nums.length;'],
      ['m_check_one', '    if (n === 1) {'],
      ['m_ret_one',   '        return nums[0];'],
      ['',            '    }'],
      ['m_alloc_m1',  '    const memo1 = new Array(n - 1).fill(-1);'],
      ['m_case1',     '    const case1 = solve(nums, n - 2, 0, memo1);'],
      ['m_alloc_m2',  '    const memo2 = new Array(n - 1).fill(-1);'],
      ['m_case2',     '    const case2 = solve(nums, n - 1, 1, memo2);'],
      ['m_ret_max2',  '    return Math.max(case1, case2);'],
      ['',            '}'],
      ['',            ''],
      ['',            'const tokens = fs.readFileSync(0, "utf-8").trim().split(/\\s+/);'],
      ['m_read_n',    'const n = parseInt(tokens[0], 10);'],
      ['m_alloc_arr', 'const nums = new Array(n);'],
      ['m_read_arr',  'for (let i = 0; i < n; i++) { nums[i] = parseInt(tokens[i + 1], 10); }'],
      ['m_call_rob',  'const maxLoot = robCircular(nums);'],
      ['m_print',     'console.log(maxLoot);'],
      ['m_done',      'process.exit(0);']
    ]
  },
  tabulation: {
    java: [
      ['',            'import java.util.Scanner;'],
      ['',            ''],
      ['',            'public class Main {'],
      ['h_entry',     '    static int robRange(int[] nums, int s, int e) {'],
      ['h_base_s',    '        if (s == e) {'],
      ['h_ret_s',     '            return nums[s];'],
      ['',            '        }'],
      ['h_alloc_dp',  '        int[] dp = new int[e - s + 1];'],
      ['h_dp0',       '        dp[0] = nums[s];'],
      ['h_dp1',       '        dp[1] = Math.max(nums[s], nums[s + 1]);'],
      ['h_for_loop',  '        for (int i = 2; i <= e - s; i++) {'],
      ['h_dp_trans',  '            dp[i] = Math.max(dp[i - 1], nums[s + i] + dp[i - 2]);'],
      ['',            '        }'],
      ['h_ret_dp',    '        return dp[e - s];'],
      ['',            '    }'],
      ['',            ''],
      ['m_entry',     '    static int robCircular(int[] nums) {'],
      ['m_get_n',     '        int n = nums.length;'],
      ['m_check_one', '        if (n == 1) {'],
      ['m_ret_one',   '            return nums[0];'],
      ['',            '        }'],
      ['m_case1',     '        int case1 = robRange(nums, 0, n - 2);'],
      ['m_case2',     '        int case2 = robRange(nums, 1, n - 1);'],
      ['m_ret_max2',  '        return Math.max(case1, case2);'],
      ['',            '    }'],
      ['',            ''],
      ['',            '    public static void main(String[] args) {'],
      ['',            '        Scanner sc = new Scanner(System.in);'],
      ['m_read_n',    '        int n = sc.nextInt();'],
      ['m_alloc_arr', '        int[] nums = new int[n];'],
      ['m_read_arr',  '        for (int i = 0; i < n; i++) {'],
      ['',            '            nums[i] = sc.nextInt();'],
      ['',            '        }'],
      ['m_call_rob',  '        int maxLoot = robCircular(nums);'],
      ['m_print',     '        System.out.println(maxLoot);'],
      ['m_done',      '    }'],
      ['',            '}']
    ],
    c: [
      ['',            '#include <stdio.h>'],
      ['',            '#define MAX(a, b) ((a) > (b) ? (a) : (b))'],
      ['',            ''],
      ['h_entry',     'int robRange(int* nums, int s, int e) {'],
      ['h_base_s',    '    if (s == e) {'],
      ['h_ret_s',     '        return nums[s];'],
      ['',            '    }'],
      ['h_alloc_dp',  '    int dp[e - s + 1];'],
      ['h_dp0',       '    dp[0] = nums[s];'],
      ['h_dp1',       '    dp[1] = MAX(nums[s], nums[s + 1]);'],
      ['h_for_loop',  '    for (int i = 2; i <= e - s; i++) {'],
      ['h_dp_trans',  '        dp[i] = MAX(dp[i - 1], nums[s + i] + dp[i - 2]);'],
      ['',            '    }'],
      ['h_ret_dp',    '    return dp[e - s];'],
      ['',            '}'],
      ['',            ''],
      ['m_entry',     'int robCircular(int* nums, int n) {'],
      ['m_get_n',     '    int len = n;'],
      ['m_check_one', '    if (len == 1) {'],
      ['m_ret_one',   '        return nums[0];'],
      ['',            '    }'],
      ['m_case1',     '    int case1 = robRange(nums, 0, len - 2);'],
      ['m_case2',     '    int case2 = robRange(nums, 1, len - 1);'],
      ['m_ret_max2',  '    return MAX(case1, case2);'],
      ['',            '}'],
      ['',            ''],
      ['',            'int main() {'],
      ['m_read_n',    '    int n; scanf("%d", &n);'],
      ['m_alloc_arr', '    int nums[n];'],
      ['m_read_arr',  '    for (int i = 0; i < n; i++) {'],
      ['',            '        scanf("%d", &nums[i]);'],
      ['',            '    }'],
      ['m_call_rob',  '    int maxLoot = robCircular(nums, n);'],
      ['m_print',     '    printf(\"%d\\n\", maxLoot);'],
      ['m_done',      '    return 0;'],
      ['',            '}']
    ],
    cpp: [
      ['',            '#include <iostream>'],
      ['',            '#include <vector>'],
      ['',            '#include <algorithm>'],
      ['',            'using namespace std;'],
      ['',            ''],
      ['h_entry',     'int robRange(const vector<int>& nums, int s, int e) {'],
      ['h_base_s',    '    if (s == e) {'],
      ['h_ret_s',     '        return nums[s];'],
      ['',            '    }'],
      ['h_alloc_dp',  '    vector<int> dp(e - s + 1);'],
      ['h_dp0',       '    dp[0] = nums[s];'],
      ['h_dp1',       '    dp[1] = max(nums[s], nums[s + 1]);'],
      ['h_for_loop',  '    for (int i = 2; i <= e - s; i++) {'],
      ['h_dp_trans',  '        dp[i] = max(dp[i - 1], nums[s + i] + dp[i - 2]);'],
      ['',            '    }'],
      ['h_ret_dp',    '    return dp[e - s];'],
      ['',            '}'],
      ['',            ''],
      ['m_entry',     'int robCircular(const vector<int>& nums) {'],
      ['m_get_n',     '    int n = nums.size();'],
      ['m_check_one', '    if (n == 1) {'],
      ['m_ret_one',   '        return nums[0];'],
      ['',            '    }'],
      ['m_case1',     '    int case1 = robRange(nums, 0, n - 2);'],
      ['m_case2',     '    int case2 = robRange(nums, 1, n - 1);'],
      ['m_ret_max2',  '    return max(case1, case2);'],
      ['',            '}'],
      ['',            ''],
      ['',            'int main() {'],
      ['m_read_n',    '    int n; cin >> n;'],
      ['m_alloc_arr', '    vector<int> nums(n);'],
      ['m_read_arr',  '    for (int i = 0; i < n; i++) {'],
      ['',            '        cin >> nums[i];'],
      ['',            '    }'],
      ['m_call_rob',  '    int maxLoot = robCircular(nums);'],
      ['m_print',     '    cout << maxLoot << endl;'],
      ['m_done',      '    return 0;'],
      ['',            '}']
    ],
    python: [
      ['',            'import sys'],
      ['',            ''],
      ['h_entry',     'def rob_range(nums, s, e):'],
      ['h_base_s',    '    if s == e:'],
      ['h_ret_s',     '        return nums[s]'],
      ['h_alloc_dp',  '    dp = [0] * (e - s + 1)'],
      ['h_dp0',       '    dp[0] = nums[s]'],
      ['h_dp1',       '    dp[1] = max(nums[s], nums[s + 1])'],
      ['h_for_loop',  '    for i in range(2, e - s + 1):'],
      ['h_dp_trans',  '        dp[i] = max(dp[i - 1], nums[s + i] + dp[i - 2])'],
      ['h_ret_dp',    '    return dp[e - s]'],
      ['',            ''],
      ['m_entry',     'def rob_circular(nums):'],
      ['m_get_n',     '    n = len(nums)'],
      ['m_check_one',  '    if n == 1:'],
      ['m_ret_one',    '        return nums[0]'],
      ['m_case1',      '    case1 = rob_range(nums, 0, n - 2)'],
      ['m_case2',      '    case2 = rob_range(nums, 1, n - 1)'],
      ['m_ret_max2',   '    return max(case1, case2)'],
      ['',            ''],
      ['',            'if __name__ == "__main__":'],
      ['',            '    tokens = sys.stdin.read().split()'],
      ['m_read_n',    '    n = int(tokens[0])'],
      ['m_alloc_arr', '    nums = [0] * n'],
      ['m_read_arr',  '    for i in range(n): nums[i] = int(tokens[i + 1])'],
      ['m_call_rob',  '    max_loot = rob_circular(nums)'],
      ['m_print',     '    print(max_loot)'],
      ['m_done',      '    sys.exit(0)']
    ],
    javascript: [
      ['',            'const fs = require("fs");'],
      ['',            ''],
      ['h_entry',     'function robRange(nums, s, e) {'],
      ['h_base_s',    '    if (s === e) {'],
      ['h_ret_s',     '        return nums[s];'],
      ['',            '    }'],
      ['h_alloc_dp',  '    const dp = new Array(e - s + 1);'],
      ['h_dp0',       '    dp[0] = nums[s];'],
      ['h_dp1',       '    dp[1] = Math.max(nums[s], nums[s + 1]);'],
      ['h_for_loop',  '    for (let i = 2; i <= e - s; i++) {'],
      ['h_dp_trans',  '        dp[i] = Math.max(dp[i - 1], nums[s + i] + dp[i - 2]);'],
      ['',            '    }'],
      ['h_ret_dp',    '    return dp[e - s];'],
      ['',            '}'],
      ['',            ''],
      ['m_entry',     'function robCircular(nums) {'],
      ['m_get_n',     '    const n = nums.length;'],
      ['m_check_one', '    if (n === 1) {'],
      ['m_ret_one',   '        return nums[0];'],
      ['',            '    }'],
      ['m_case1',     '    const case1 = robRange(nums, 0, n - 2);'],
      ['m_case2',     '    const case2 = robRange(nums, 1, n - 1);'],
      ['m_ret_max2',  '    return Math.max(case1, case2);'],
      ['',            '}'],
      ['',            ''],
      ['',            'const tokens = fs.readFileSync(0, "utf-8").trim().split(/\\s+/);'],
      ['m_read_n',    'const n = parseInt(tokens[0], 10);'],
      ['m_alloc_arr', 'const nums = new Array(n);'],
      ['m_read_arr',  'for (let i = 0; i < n; i++) { nums[i] = parseInt(tokens[i + 1], 10); }'],
      ['m_call_rob',  'const maxLoot = robCircular(nums);'],
      ['m_print',     'console.log(maxLoot);'],
      ['m_done',      'process.exit(0);']
    ]
  }
};

const PSEUDOCODES = {
  recursion: [
    'function rob(nums, i, s):   // s = start index of sub-range',
    '  if i < s: return 0',
    '  if i == s: return nums[s]',
    '  skip       = rob(nums, i - 1, s)        // Skip house i',
    '  robCurrent = nums[i] + rob(nums, i - 2, s) // Rob house i',
    '  return max(skip, robCurrent)',
    '',
    'function robCircular(nums):',
    '  n = nums.length',
    '  if n == 1: return nums[0]',
    '  case1 = rob(nums, n - 2, 0)   // Sub-range 0 .. n-2 (exclude last)',
    '  case2 = rob(nums, n - 1, 1)   // Sub-range 1 .. n-1 (exclude first)',
    '  return max(case1, case2)',
    '',
    'Time Complexity:  O(2^n) — Two exponential sub-trees',
    'Space Complexity: O(n) Call Stack depth'
  ],
  memoization: [
    'memo = array of size (n - 1), initialized to -1',
    '',
    'function solve(nums, i, s, memo):',
    '  if i < s: return 0',
    '  if i == s: return nums[s]',
    '  if memo[i - s] != -1: return memo[i - s]   // O(1) Cache Hit',
    '  skip       = solve(nums, i - 1, s, memo)',
    '  robCurrent = nums[i] + solve(nums, i - 2, s, memo)',
    '  memo[i - s]  = max(skip, robCurrent)',
    '  return memo[i - s]',
    '',
    'function robCircular(nums):',
    '  n = nums.length',
    '  if n == 1: return nums[0]',
    '  case1 = solve(nums, n - 2, 0, memo1)   // Exclude last house',
    '  case2 = solve(nums, n - 1, 1, memo2)   // Exclude first house',
    '  return max(case1, case2)',
    '',
    'Time Complexity:  O(n) — Each sub-problem solved once',
    'Space Complexity: O(n) Cache + O(n) Call Stack'
  ],
  tabulation: [
    'function robRange(nums, s, e):',
    '  if s == e: return nums[s]',
    '  dp = array of size (e - s + 1)',
    '  dp[0] = nums[s]',
    '  dp[1] = max(nums[s], nums[s + 1])',
    '  for i = 2 to (e - s):',
    '    dp[i] = max(dp[i - 1], nums[s + i] + dp[i - 2])',
    '  return dp[e - s]',
    '',
    'function robCircular(nums):',
    '  n = nums.length',
    '  if n == 1: return nums[0]',
    '  case1 = robRange(nums, 0, n - 2)   // Exclude last house',
    '  case2 = robRange(nums, 1, n - 1)   // Exclude first house',
    '  return max(case1, case2)',
    '',
    'Time Complexity:  O(n) — Two linear passes',
    'Space Complexity: O(n) — Two DP arrays of size n - 1'
  ]
};

/* ==================================================================== */
/* STATIC TREE GENERATOR FOR RECURSIVE CALL TREE                        */
/* ==================================================================== */
function generateStaticTree(approach, nums, rangeStart, rangeEnd) {
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
      if (i > rangeStart) {
        const leftId  = build(i - 1, id, true);
        const rightId = build(i - 2, id, false);
        node.children = [leftId, rightId];
      }
      return id;
    }
    if (rangeEnd >= rangeStart) {
      build(rangeEnd, null, false);
    }
  } else if (approach === 'memoization') {
    const memo = {};
    function buildMemo(i, parentId, isLeft) {
      const id = nextId++;
      const isCacheHit = (i >= rangeStart) && (memo[i] !== undefined);
      const node = { id, i, parentId, isLeft, isCacheHit, children: [] };
      nodes.push(node);
      if (parentId !== null) {
        edges.push({ from: parentId, to: id, isLeft });
      }
      if (i < rangeStart || isCacheHit) {
        /* Leaf */
      } else {
        const leftId  = buildMemo(i - 1, id, true);
        const rightId = buildMemo(i - 2, id, false);
        node.children = [leftId, rightId];
        memo[i] = 1;
      }
      return id;
    }
    if (rangeEnd >= rangeStart) {
      buildMemo(rangeEnd, null, false);
    }
  }

  const nodeMap = new Map();
  nodes.forEach(nd => nodeMap.set(nd.id, { ...nd }));
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
  const leafSpacing = 60, levelHeight = 44;
  const computedWidth  = Math.max(160, totalLeaves * leafSpacing + 30);
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
  nodeMap.forEach((nd, id) => { layoutMap[id] = { x: nd.x, y: nd.y, depth: nd.depth }; });
  return { nodes, edges, layoutMap, width: computedWidth, height: computedHeight };
}

/* ==================================================================== */
/* BUILD STEPS — STRICT ZERO-SKIP LINE-BY-LINE EXECUTION TRACE          */
/* ==================================================================== */
function buildSteps(approach, numsArr) {
  const steps = [];
  const nums  = (numsArr && numsArr.length > 0) ? numsArr : [2, 3, 2];
  const n     = nums.length;

  /* Helper for standardized input reading steps */
  function pushInputSteps(extraState) {
    steps.push({
      code: 'm_read_n',
      badge: `Read n = ${n}`,
      badgeType: 'info',
      state: { nums, n, ...extraState, frames: [{ name: 'main()', args: `n=${n}` }] }
    });
    steps.push({
      code: 'm_alloc_arr',
      badge: `Allocate nums array of size n = ${n}`,
      badgeType: 'info',
      state: { nums: new Array(n).fill(0), n, ...extraState, frames: [{ name: 'main()', args: `n=${n}` }] }
    });
    steps.push({
      code: 'm_read_arr',
      badge: `Read array elements: [${nums.join(', ')}] (n=${n})`,
      badgeType: 'info',
      state: { nums: [...nums], n, ...extraState, frames: [{ name: 'main()', args: `n=${n}` }] }
    });
  }

  /* ------------------------------------------------------------------ */
  /* APPROACH 1: BRUTE FORCE RECURSION                                   */
  /* ------------------------------------------------------------------ */
  if (approach === 'recursion') {
    const rangeEnd1 = n - 2, rangeStart1 = 0;
    const rangeEnd2 = n - 1, rangeStart2 = 1;

    const staticTree1 = generateStaticTree('recursion', nums, rangeStart1, rangeEnd1);
    const staticTree2 = generateStaticTree('recursion', nums, rangeStart2, rangeEnd2);

    const nodeStateMap1 = {};
    staticTree1.nodes.forEach(nd => {
      nodeStateMap1[nd.id] = { ...nd, x: staticTree1.layoutMap[nd.id].x, y: staticTree1.layoutMap[nd.id].y, state: 'hidden', retVal: null };
    });
    const nodeStateMap2 = {};
    staticTree2.nodes.forEach(nd => {
      nodeStateMap2[nd.id] = { ...nd, x: staticTree2.layoutMap[nd.id].x, y: staticTree2.layoutMap[nd.id].y, state: 'hidden', retVal: null };
    });

    let nodeStateMap = nodeStateMap1;
    let activeStaticTree = staticTree1;

    function getVisibleNodes() {
      return Object.values(nodeStateMap).filter(nd => nd.state !== 'hidden').map(nd => ({ ...nd }));
    }
    function getVisibleEdges() {
      return activeStaticTree.edges.filter(e => {
        const fn = nodeStateMap[e.from], tn = nodeStateMap[e.to];
        return fn && tn && fn.state !== 'hidden' && tn.state !== 'hidden';
      }).map(e => ({
        ...e,
        x1: activeStaticTree.layoutMap[e.from].x,
        y1: activeStaticTree.layoutMap[e.from].y + 12,
        x2: activeStaticTree.layoutMap[e.to].x,
        y2: activeStaticTree.layoutMap[e.to].y - 12,
      }));
    }
    function makeTreeState(extra) {
      return {
        ...extra,
        case1: curCase1,
        case2: curCase2,
        treeNodes: getVisibleNodes(),
        treeEdges: getVisibleEdges(),
        treeWidth:  activeStaticTree.width,
        treeHeight: activeStaticTree.height,
      };
    }

    let curCase1 = null;
    let curCase2 = null;
    const baseExtra = { totalCalls: 0, redundantCalls: 0, cacheHits: 0, currentReturn: null, activeNodeId: null, caseLabel: '', case1: null, case2: null };
    pushInputSteps({ ...baseExtra, treeNodes: [], treeEdges: [], treeWidth: staticTree1.width, treeHeight: staticTree1.height });

    steps.push({
      code: 'm_call_rob',
      badge: `main() calls robCircular(nums)`,
      badgeType: 'info',
      state: makeTreeState({ nums, n, ...baseExtra, frames: [{ name: 'main()', args: `n=${n}` }] })
    });
    steps.push({
      code: 'm_entry',
      badge: `robCircular(nums) — houses form a circle: House 0 and House ${n-1} are adjacent, cannot both be robbed`,
      badgeType: 'info',
      state: makeTreeState({ nums, n, ...baseExtra, frames: [{ name: 'main()', args: `n=${n}` }, { name: 'robCircular()', args: `n=${n}` }] })
    });
    steps.push({
      code: 'm_get_n',
      badge: `n = ${n}`,
      badgeType: 'info',
      state: makeTreeState({ nums, n, ...baseExtra, frames: [{ name: 'main()', args: `n=${n}` }, { name: 'robCircular()', args: `n=${n}` }] })
    });
    steps.push({
      code: 'm_check_one',
      badge: `n == 1? ${n} == 1 -> ${n === 1 ? 'TRUE' : 'FALSE'}`,
      badgeType: 'info',
      state: makeTreeState({ nums, n, ...baseExtra, frames: [{ name: 'main()', args: `n=${n}` }, { name: 'robCircular()', args: `n=${n}` }] })
    });

    if (n === 1) {
      steps.push({
        code: 'm_ret_one',
        badge: `Only 1 house: return ${nums[0]}`,
        badgeType: 'success',
        state: makeTreeState({ nums, n, totalCalls: 0, redundantCalls: 0, cacheHits: 0, currentReturn: nums[0], activeNodeId: null, caseLabel: '',
          frames: [{ name: 'main()', args: `maxLoot=${nums[0]}` }] })
      });
      steps.push({
        code: 'm_print',
        badge: `Output: ${nums[0]}`,
        badgeType: 'success',
        state: makeTreeState({ nums, n, totalCalls: 0, redundantCalls: 0, cacheHits: 0, currentReturn: nums[0], activeNodeId: null, caseLabel: '',
          frames: [{ name: 'main()', args: `maxLoot=${nums[0]}` }] })
      });
      steps.push({
        code: 'm_done',
        badge: `Execution complete. Answer = ${nums[0]}`,
        badgeType: 'success',
        state: makeTreeState({ nums, n, totalCalls: 0, redundantCalls: 0, cacheHits: 0, currentReturn: nums[0], activeNodeId: null, caseLabel: '',
          frames: [{ name: 'main()', args: `maxLoot=${nums[0]}` }] })
      });
      return steps;
    }

    const stackFrames = [];
    let nextNodeId = 0, callCounter = 0, redundantCounter = 0;
    const computedValuesMap = {};

    function recurse(i, s) {
      const myNodeId = nextNodeId++;
      callCounter++;
      const isDuplicate = (computedValuesMap[`${s}-${i}`] !== undefined);
      if (isDuplicate) redundantCounter++;

      nodeStateMap[myNodeId].state = 'active';
      nodeStateMap[myNodeId].isRedundant = isDuplicate;
      stackFrames.push({ name: `rob(${i}, ${s})`, args: i >= s ? `H[${i}]=$${nums[i]}` : 'i < s' });

      steps.push({
        code: 'h_entry',
        badge: isDuplicate
          ? `[REDUNDANT] rob(i=${i}, s=${s}) solved again — duplicate computation`
          : (i >= s ? `Entering rob(i=${i}, s=${s}) [Call #${callCounter}] — House ${i} ($${nums[i]})` : `Entering rob(i=${i}, s=${s}) — out of range`),
        badgeType: isDuplicate ? 'warn' : 'info',
        state: makeTreeState({ nums, n, totalCalls: callCounter, redundantCalls: redundantCounter, cacheHits: 0,
          currentReturn: null, activeNodeId: myNodeId, activeHouseIdx: i >= s ? i : null, caseLabel: activeStaticTree === staticTree1 ? 'Case 1' : 'Case 2',
          frames: [...stackFrames] })
      });

      steps.push({
        code: 'h_base_neg',
        badge: `i < s? ${i} < ${s} -> ${i < s ? 'TRUE (out of range, return 0)' : 'FALSE'}`,
        badgeType: 'info',
        state: makeTreeState({ nums, n, totalCalls: callCounter, redundantCalls: redundantCounter, cacheHits: 0,
          currentReturn: null, activeNodeId: myNodeId, activeHouseIdx: i >= s ? i : null, caseLabel: activeStaticTree === staticTree1 ? 'Case 1' : 'Case 2',
          frames: [...stackFrames] })
      });

      if (i < s) {
        nodeStateMap[myNodeId].state = 'solved';
        nodeStateMap[myNodeId].retVal = 0;
        steps.push({
          code: 'h_ret_neg',
          badge: `Base case: rob(${i}, ${s}) returns $0 (no houses in range)`,
          badgeType: 'success',
          state: makeTreeState({ nums, n, totalCalls: callCounter, redundantCalls: redundantCounter, cacheHits: 0,
            currentReturn: 0, activeNodeId: myNodeId, activeHouseIdx: null, caseLabel: activeStaticTree === staticTree1 ? 'Case 1' : 'Case 2',
            frames: [...stackFrames] })
        });
        stackFrames.pop();
        return 0;
      }

      steps.push({
        code: 'h_base_s',
        badge: `i == s? ${i} == ${s} -> ${i === s ? `TRUE — only house ${s} in range, return nums[${s}]=$${nums[s]}` : 'FALSE'}`,
        badgeType: 'info',
        state: makeTreeState({ nums, n, totalCalls: callCounter, redundantCalls: redundantCounter, cacheHits: 0,
          currentReturn: null, activeNodeId: myNodeId, activeHouseIdx: i, caseLabel: activeStaticTree === staticTree1 ? 'Case 1' : 'Case 2',
          frames: [...stackFrames] })
      });

      if (i === s) {
        const ret = nums[s];
        nodeStateMap[myNodeId].state = 'solved';
        nodeStateMap[myNodeId].retVal = ret;
        computedValuesMap[`${s}-${i}`] = ret;
        steps.push({
          code: 'h_ret_s',
          badge: `Base case: rob(${i}, ${s}) returns nums[${s}]=$${ret}`,
          badgeType: 'success',
          state: makeTreeState({ nums, n, totalCalls: callCounter, redundantCalls: redundantCounter, cacheHits: 0,
            currentReturn: ret, activeNodeId: myNodeId, activeHouseIdx: i, caseLabel: activeStaticTree === staticTree1 ? 'Case 1' : 'Case 2',
            frames: [...stackFrames] })
        });
        stackFrames.pop();
        return ret;
      }

      steps.push({
        code: 'h_rec_skip',
        badge: `Skip House ${i}: call rob(i-1=${i-1}, s=${s})`,
        badgeType: 'info',
        state: makeTreeState({ nums, n, totalCalls: callCounter, redundantCalls: redundantCounter, cacheHits: 0,
          currentReturn: null, activeNodeId: myNodeId, activeHouseIdx: i, caseLabel: activeStaticTree === staticTree1 ? 'Case 1' : 'Case 2',
          frames: [...stackFrames] })
      });
      const skipVal = recurse(i - 1, s);

      steps.push({
        code: 'h_rec_rob',
        badge: `Rob House ${i} ($${nums[i]}): call rob(i-2=${i-2}, s=${s})`,
        badgeType: 'info',
        state: makeTreeState({ nums, n, totalCalls: callCounter, redundantCalls: redundantCounter, cacheHits: 0,
          currentReturn: null, activeNodeId: myNodeId, activeHouseIdx: i, caseLabel: activeStaticTree === staticTree1 ? 'Case 1' : 'Case 2',
          frames: [...stackFrames] })
      });
      const robPrevVal = recurse(i - 2, s);
      const robTotal   = nums[i] + robPrevVal;
      const maxLoot    = Math.max(skipVal, robTotal);
      nodeStateMap[myNodeId].state = 'solved';
      nodeStateMap[myNodeId].retVal = maxLoot;
      computedValuesMap[`${s}-${i}`] = maxLoot;

      steps.push({
        code: 'h_ret_max',
        badge: `House ${i}: max(Skip=$${skipVal}, Rob=$${nums[i]}+$${robPrevVal}=$${robTotal}) -> $${maxLoot}`,
        badgeType: 'success',
        state: makeTreeState({ nums, n, totalCalls: callCounter, redundantCalls: redundantCounter, cacheHits: 0,
          currentReturn: maxLoot, activeNodeId: myNodeId, activeHouseIdx: i, caseLabel: activeStaticTree === staticTree1 ? 'Case 1' : 'Case 2',
          frames: [...stackFrames] })
      });
      stackFrames.pop();
      return maxLoot;
    }

    // Case 1: houses 0 .. n-2
    steps.push({
      code: 'm_case1',
      badge: `CASE 1: rob(nums, i=${rangeEnd1}, s=${rangeStart1}) — exclude last house (${n-1}), range 0..${n-2}`,
      badgeType: 'info',
      state: makeTreeState({ nums, n, totalCalls: callCounter, redundantCalls: redundantCounter, cacheHits: 0,
        currentReturn: null, activeNodeId: null, caseLabel: 'Case 1',
        frames: [{ name: 'main()', args: `n=${n}` }, { name: 'robCircular()', args: 'case1=?' }] })
    });

    const case1Result = recurse(rangeEnd1, rangeStart1);
    curCase1 = case1Result;

    // Switch to Case 2 tree
    nodeStateMap = nodeStateMap2;
    activeStaticTree = staticTree2;
    nextNodeId = 0;

    steps.push({
      code: 'm_case2',
      badge: `CASE 2: rob(nums, i=${rangeEnd2}, s=${rangeStart2}) — exclude first house (0), range 1..${n-1}`,
      badgeType: 'info',
      state: makeTreeState({ nums, n, totalCalls: callCounter, redundantCalls: redundantCounter, cacheHits: 0,
        currentReturn: null, activeNodeId: null, caseLabel: 'Case 2',
        frames: [{ name: 'main()', args: `n=${n}` }, { name: 'robCircular()', args: `case1=$${case1Result}` }] })
    });

    const case2Result = recurse(rangeEnd2, rangeStart2);
    curCase2 = case2Result;
    const finalResult = Math.max(case1Result, case2Result);

    steps.push({
      code: 'm_ret_max2',
      badge: `Return max(case1=$${case1Result}, case2=$${case2Result}) = $${finalResult}`,
      badgeType: 'success',
      state: makeTreeState({ nums, n, totalCalls: callCounter, redundantCalls: redundantCounter, cacheHits: 0,
        currentReturn: finalResult, activeNodeId: null, caseLabel: 'Final',
        frames: [{ name: 'main()', args: `maxLoot=$${finalResult}` }] })
    });
    steps.push({
      code: 'm_print',
      badge: `Output: Maximum circular loot = $${finalResult} (${callCounter} total calls)`,
      badgeType: 'success',
      state: makeTreeState({ nums, n, totalCalls: callCounter, redundantCalls: redundantCounter, cacheHits: 0,
        currentReturn: finalResult, activeNodeId: null, caseLabel: 'Final',
        frames: [{ name: 'main()', args: `maxLoot=$${finalResult}` }] })
    });
    steps.push({
      code: 'm_done',
      badge: `Execution complete. Final result = $${finalResult}`,
      badgeType: 'success',
      state: makeTreeState({ nums, n, totalCalls: callCounter, redundantCalls: redundantCounter, cacheHits: 0,
        currentReturn: finalResult, activeNodeId: null, caseLabel: 'Final',
        frames: [{ name: 'main()', args: `maxLoot=$${finalResult}` }] })
    });
  }

  /* ------------------------------------------------------------------ */
  /* APPROACH 2: MEMOIZATION (TOP-DOWN)                                 */
  /* ------------------------------------------------------------------ */
  else if (approach === 'memoization') {
    const rangeEnd1 = n - 2, rangeStart1 = 0;
    const rangeEnd2 = n - 1, rangeStart2 = 1;
    const staticTree1 = generateStaticTree('memoization', nums, rangeStart1, rangeEnd1);
    const staticTree2 = generateStaticTree('memoization', nums, rangeStart2, rangeEnd2);

    const nodeStateMap1 = {};
    staticTree1.nodes.forEach(nd => {
      nodeStateMap1[nd.id] = { ...nd, x: staticTree1.layoutMap[nd.id].x, y: staticTree1.layoutMap[nd.id].y, state: 'hidden', retVal: null };
    });
    const nodeStateMap2 = {};
    staticTree2.nodes.forEach(nd => {
      nodeStateMap2[nd.id] = { ...nd, x: staticTree2.layoutMap[nd.id].x, y: staticTree2.layoutMap[nd.id].y, state: 'hidden', retVal: null };
    });

    let nodeStateMap = nodeStateMap1;
    let activeStaticTree = staticTree1;

    function getVisibleNodes() {
      return Object.values(nodeStateMap).filter(nd => nd.state !== 'hidden').map(nd => ({ ...nd }));
    }
    function getVisibleEdges() {
      return activeStaticTree.edges.filter(e => {
        const fn = nodeStateMap[e.from], tn = nodeStateMap[e.to];
        return fn && tn && fn.state !== 'hidden' && tn.state !== 'hidden';
      }).map(e => ({
        ...e,
        x1: activeStaticTree.layoutMap[e.from].x,
        y1: activeStaticTree.layoutMap[e.from].y + 12,
        x2: activeStaticTree.layoutMap[e.to].x,
        y2: activeStaticTree.layoutMap[e.to].y - 12,
      }));
    }

    let memoArr1 = new Array(Math.max(0, n - 1)).fill(-1);
    let memoArr2 = new Array(Math.max(0, n - 1)).fill(-1);
    let activeMemoArr = memoArr1;

    function makeTreeState(extra) {
      return {
        ...extra,
        case1: curCase1,
        case2: curCase2,
        treeNodes: getVisibleNodes(),
        treeEdges: getVisibleEdges(),
        treeWidth:  activeStaticTree.width,
        treeHeight: activeStaticTree.height,
        memo: [...activeMemoArr],
      };
    }

    let curCase1 = null;
    let curCase2 = null;
    const baseExtra = { totalCalls: 0, cacheHits: 0, redundantCalls: 0, currentReturn: null, activeNodeId: null, caseLabel: '', case1: null, case2: null };
    pushInputSteps({ ...baseExtra, treeNodes: [], treeEdges: [], treeWidth: staticTree1.width, treeHeight: staticTree1.height, memo: [] });

    steps.push({
      code: 'm_call_rob',
      badge: `main() calls robCircular(nums)`,
      badgeType: 'info',
      state: makeTreeState({ nums, n, ...baseExtra, frames: [{ name: 'main()', args: `n=${n}` }] })
    });
    steps.push({
      code: 'm_entry',
      badge: `robCircular(nums) — circular constraint: House 0 adjacent to House ${n-1}`,
      badgeType: 'info',
      state: makeTreeState({ nums, n, ...baseExtra, frames: [{ name: 'main()', args: `n=${n}` }, { name: 'robCircular()', args: `n=${n}` }] })
    });
    steps.push({
      code: 'm_get_n',
      badge: `n = ${n}`,
      badgeType: 'info',
      state: makeTreeState({ nums, n, ...baseExtra, frames: [{ name: 'main()', args: `n=${n}` }, { name: 'robCircular()', args: `n=${n}` }] })
    });
    steps.push({
      code: 'm_check_one',
      badge: `n == 1? -> ${n === 1 ? 'TRUE' : 'FALSE'}`,
      badgeType: 'info',
      state: makeTreeState({ nums, n, ...baseExtra, frames: [{ name: 'main()', args: `n=${n}` }, { name: 'robCircular()', args: `n=${n}` }] })
    });

    if (n === 1) {
      steps.push({
        code: 'm_ret_one',
        badge: `Only 1 house: return ${nums[0]}`,
        badgeType: 'success',
        state: makeTreeState({ nums, n, totalCalls: 0, cacheHits: 0, redundantCalls: 0, currentReturn: nums[0], activeNodeId: null, caseLabel: '',
          frames: [{ name: 'main()', args: `maxLoot=${nums[0]}` }] })
      });
      steps.push({
        code: 'm_print',
        badge: `Output: ${nums[0]}`,
        badgeType: 'success',
        state: makeTreeState({ nums, n, totalCalls: 0, cacheHits: 0, redundantCalls: 0, currentReturn: nums[0], activeNodeId: null, caseLabel: '',
          frames: [{ name: 'main()', args: `maxLoot=${nums[0]}` }] })
      });
      steps.push({
        code: 'm_done',
        badge: `Done. Answer = ${nums[0]}`,
        badgeType: 'success',
        state: makeTreeState({ nums, n, totalCalls: 0, cacheHits: 0, redundantCalls: 0, currentReturn: nums[0], activeNodeId: null, caseLabel: '',
          frames: [{ name: 'main()', args: `maxLoot=${nums[0]}` }] })
      });
      return steps;
    }

    let nextNodeId = 0, callCounter = 0, cacheHits = 0;
    const stackFrames = [];

    function recurseMemo(i, s, memoRef) {
      const myNodeId = nextNodeId++;
      callCounter++;
      nodeStateMap[myNodeId].state = 'active';
      stackFrames.push({ name: `solve(${i},${s})`, args: i >= s ? `H[${i}]=$${nums[i]}` : 'i < s' });

      const cLabel = activeStaticTree === staticTree1 ? 'Case 1' : 'Case 2';

      steps.push({
        code: 'h_entry',
        badge: i >= s
          ? `Entering solve(i=${i}, s=${s}, memo) [Call #${callCounter}] — House ${i} ($${nums[i]})`
          : `Entering solve(i=${i}, s=${s}, memo) — out of range`,
        badgeType: 'info',
        state: makeTreeState({ nums, n, totalCalls: callCounter, cacheHits, redundantCalls: 0,
          currentReturn: null, activeNodeId: myNodeId, activeHouseIdx: i >= s ? i : null,
          caseLabel: cLabel, frames: [...stackFrames] })
      });

      steps.push({
        code: 'h_base_neg',
        badge: `i < s? ${i} < ${s} -> ${i < s ? 'TRUE (return 0)' : 'FALSE'}`,
        badgeType: 'info',
        state: makeTreeState({ nums, n, totalCalls: callCounter, cacheHits, redundantCalls: 0,
          currentReturn: null, activeNodeId: myNodeId, activeHouseIdx: i >= s ? i : null,
          caseLabel: cLabel, frames: [...stackFrames] })
      });

      if (i < s) {
        nodeStateMap[myNodeId].state = 'solved';
        nodeStateMap[myNodeId].retVal = 0;
        steps.push({
          code: 'h_ret_neg',
          badge: `Return 0 — no houses in range`,
          badgeType: 'success',
          state: makeTreeState({ nums, n, totalCalls: callCounter, cacheHits, redundantCalls: 0,
            currentReturn: 0, activeNodeId: myNodeId, activeHouseIdx: null,
            caseLabel: cLabel, frames: [...stackFrames] })
        });
        stackFrames.pop();
        return 0;
      }

      steps.push({
        code: 'h_base_s',
        badge: `i == s? ${i} == ${s} -> ${i === s ? `TRUE — return nums[${s}]=$${nums[s]}` : 'FALSE'}`,
        badgeType: 'info',
        state: makeTreeState({ nums, n, totalCalls: callCounter, cacheHits, redundantCalls: 0,
          currentReturn: null, activeNodeId: myNodeId, activeHouseIdx: i, caseLabel: cLabel, frames: [...stackFrames] })
      });

      if (i === s) {
        nodeStateMap[myNodeId].state = 'solved';
        nodeStateMap[myNodeId].retVal = nums[s];
        steps.push({
          code: 'h_ret_s',
          badge: `Base case: return nums[${s}]=$${nums[s]}`,
          badgeType: 'success',
          state: makeTreeState({ nums, n, totalCalls: callCounter, cacheHits, redundantCalls: 0,
            currentReturn: nums[s], activeNodeId: myNodeId, activeHouseIdx: i, caseLabel: cLabel, frames: [...stackFrames] })
        });
        stackFrames.pop();
        return nums[s];
      }

      const mk = i - s;
      steps.push({
        code: 'h_memo_check',
        badge: `memo[${mk}] != -1? -> ${memoRef[mk] !== -1 ? `TRUE (CACHE HIT = $${memoRef[mk]})` : 'FALSE (cache miss)'}`,
        badgeType: memoRef[mk] !== -1 ? 'success' : 'info',
        state: makeTreeState({ nums, n, totalCalls: callCounter, cacheHits, redundantCalls: 0,
          currentReturn: null, activeNodeId: myNodeId, activeHouseIdx: i, caseLabel: cLabel, frames: [...stackFrames] })
      });

      if (memoRef[mk] !== -1) {
        cacheHits++;
        const hitVal = memoRef[mk];
        nodeStateMap[myNodeId].state = 'cache-hit';
        nodeStateMap[myNodeId].retVal = hitVal;
        steps.push({
          code: 'h_ret_memo',
          badge: `[CACHE HIT] memo[${mk}] = $${hitVal} — O(1) return, subtree pruned`,
          badgeType: 'success',
          state: makeTreeState({ nums, n, totalCalls: callCounter, cacheHits, redundantCalls: 0,
            currentReturn: hitVal, activeNodeId: myNodeId, activeHouseIdx: i, caseLabel: cLabel, frames: [...stackFrames] })
        });
        stackFrames.pop();
        return hitVal;
      }

      steps.push({
        code: 'h_rec_skip',
        badge: `Skip House ${i}: call solve(${i-1}, ${s}, memo)`,
        badgeType: 'info',
        state: makeTreeState({ nums, n, totalCalls: callCounter, cacheHits, redundantCalls: 0,
          currentReturn: null, activeNodeId: myNodeId, activeHouseIdx: i, caseLabel: cLabel, frames: [...stackFrames] })
      });
      const skipVal = recurseMemo(i - 1, s, memoRef);

      steps.push({
        code: 'h_rec_rob',
        badge: `Rob House ${i} ($${nums[i]}): call solve(${i-2}, ${s}, memo)`,
        badgeType: 'info',
        state: makeTreeState({ nums, n, totalCalls: callCounter, cacheHits, redundantCalls: 0,
          currentReturn: null, activeNodeId: myNodeId, activeHouseIdx: i, caseLabel: cLabel, frames: [...stackFrames] })
      });
      const robPrevVal = recurseMemo(i - 2, s, memoRef);
      const robTotal   = nums[i] + robPrevVal;
      const maxLoot    = Math.max(skipVal, robTotal);

      memoRef[mk] = maxLoot;
      nodeStateMap[myNodeId].state = 'solved';
      nodeStateMap[myNodeId].retVal = maxLoot;

      steps.push({
        code: 'h_store_memo',
        badge: `Store memo[${mk}] = max(skip=$${skipVal}, rob=$${nums[i]}+$${robPrevVal}=$${robTotal}) = $${maxLoot}`,
        badgeType: 'success',
        state: makeTreeState({ nums, n, totalCalls: callCounter, cacheHits, redundantCalls: 0,
          currentReturn: maxLoot, activeNodeId: myNodeId, activeHouseIdx: i, caseLabel: cLabel, frames: [...stackFrames] })
      });
      steps.push({
        code: 'h_ret_max',
        badge: `Return memo[${mk}] = $${maxLoot}`,
        badgeType: 'success',
        state: makeTreeState({ nums, n, totalCalls: callCounter, cacheHits, redundantCalls: 0,
          currentReturn: maxLoot, activeNodeId: myNodeId, activeHouseIdx: i, caseLabel: cLabel, frames: [...stackFrames] })
      });
      stackFrames.pop();
      return maxLoot;
    }

    // Case 1
    steps.push({
      code: 'm_alloc_m1',
      badge: `Allocate memo1[${n-1}] = [-1, ...] for Case 1`,
      badgeType: 'info',
      state: makeTreeState({ nums, n, totalCalls: callCounter, cacheHits, redundantCalls: 0,
        currentReturn: null, activeNodeId: null, caseLabel: '',
        frames: [{ name: 'main()', args: `n=${n}` }, { name: 'robCircular()', args: 'memo1=[-1...]' }] })
    });
    steps.push({
      code: 'm_case1',
      badge: `CASE 1: solve(nums, ${rangeEnd1}, 0, memo1) — exclude house ${n-1}, range 0..${n-2}`,
      badgeType: 'info',
      state: makeTreeState({ nums, n, totalCalls: callCounter, cacheHits, redundantCalls: 0,
        currentReturn: null, activeNodeId: null, caseLabel: 'Case 1',
        frames: [{ name: 'main()', args: `n=${n}` }, { name: 'robCircular()', args: 'case1=?' }] })
    });

    const case1Result = recurseMemo(rangeEnd1, rangeStart1, memoArr1);
    curCase1 = case1Result;

    // Switch to Case 2
    nodeStateMap = nodeStateMap2;
    activeStaticTree = staticTree2;
    activeMemoArr = memoArr2;
    nextNodeId = 0;

    steps.push({
      code: 'm_alloc_m2',
      badge: `Allocate memo2[${n-1}] = [-1, ...] for Case 2`,
      badgeType: 'info',
      state: makeTreeState({ nums, n, totalCalls: callCounter, cacheHits, redundantCalls: 0,
        currentReturn: null, activeNodeId: null, caseLabel: 'Case 1 done',
        frames: [{ name: 'main()', args: `n=${n}` }, { name: 'robCircular()', args: `case1=$${case1Result}` }] })
    });
    steps.push({
      code: 'm_case2',
      badge: `CASE 2: solve(nums, ${rangeEnd2}, 1, memo2) — exclude house 0, range 1..${n-1}`,
      badgeType: 'info',
      state: makeTreeState({ nums, n, totalCalls: callCounter, cacheHits, redundantCalls: 0,
        currentReturn: null, activeNodeId: null, caseLabel: 'Case 2',
        frames: [{ name: 'main()', args: `n=${n}` }, { name: 'robCircular()', args: `case1=$${case1Result}` }] })
    });

    const case2Result = recurseMemo(rangeEnd2, rangeStart2, memoArr2);
    curCase2 = case2Result;
    const finalResult = Math.max(case1Result, case2Result);

    steps.push({
      code: 'm_ret_max2',
      badge: `Return max(case1=$${case1Result}, case2=$${case2Result}) = $${finalResult}`,
      badgeType: 'success',
      state: makeTreeState({ nums, n, totalCalls: callCounter, cacheHits, redundantCalls: 0,
        currentReturn: finalResult, activeNodeId: null, caseLabel: 'Final',
        frames: [{ name: 'main()', args: `maxLoot=$${finalResult}` }] })
    });
    steps.push({
      code: 'm_print',
      badge: `Output: $${finalResult} (${callCounter} calls, ${cacheHits} cache hits)`,
      badgeType: 'success',
      state: makeTreeState({ nums, n, totalCalls: callCounter, cacheHits, redundantCalls: 0,
        currentReturn: finalResult, activeNodeId: null, caseLabel: 'Final',
        frames: [{ name: 'main()', args: `maxLoot=$${finalResult}` }] })
    });
    steps.push({
      code: 'm_done',
      badge: `Done. Answer = $${finalResult}`,
      badgeType: 'success',
      state: makeTreeState({ nums, n, totalCalls: callCounter, cacheHits, redundantCalls: 0,
        currentReturn: finalResult, activeNodeId: null, caseLabel: 'Final',
        frames: [{ name: 'main()', args: `maxLoot=$${finalResult}` }] })
    });
  }

  /* ------------------------------------------------------------------ */
  /* APPROACH 3: TABULATION (BOTTOM-UP)                                 */
  /* ------------------------------------------------------------------ */
  else if (approach === 'tabulation') {
    const dpArr1 = new Array(Math.max(0, n - 1)).fill(null);
    const dpArr2 = new Array(Math.max(0, n - 1)).fill(null);
    let case1Val = null;
    let case2Val = null;
    let curCase1 = null;
    let curCase2 = null;
    let totalIters = 0;

    function getDPCells1(activeHouseIdx = -1) {
      return dpArr1.map((val, idx) => ({
        idx, houseVal: nums[idx], houseGlobalIdx: idx,
        val: val !== null ? val : '?',
        status: val !== null ? (idx === activeHouseIdx ? 'active' : (idx <= 1 ? 'base' : 'computed')) : 'uncalculated'
      }));
    }
    function getDPCells2(activeHouseIdx = -1) {
      return dpArr2.map((val, idx) => ({
        idx, houseVal: nums[1 + idx], houseGlobalIdx: 1 + idx,
        val: val !== null ? val : '?',
        status: val !== null ? ((1 + idx) === activeHouseIdx ? 'active' : (idx <= 1 ? 'base' : 'computed')) : 'uncalculated'
      }));
    }

    const baseExtra = {
      totalCalls: 0,
      totalOps: 0,
      currentReturn: null,
      caseLabel: '',
      activeHouseIdx1: -1,
      activeHouseIdx2: -1
    };

    function mkState(extra = {}) {
      return {
        nums,
        n,
        ...baseExtra,
        ...extra,
        iterations: totalIters,
        case1: curCase1,
        case2: curCase2,
        dpCells1: getDPCells1(extra && extra.activeHouseIdx1 !== undefined ? extra.activeHouseIdx1 : -1),
        dpCells2: getDPCells2(extra && extra.activeHouseIdx2 !== undefined ? extra.activeHouseIdx2 : -1)
      };
    }

    pushInputSteps(mkState({}));

    steps.push({
      code: 'm_call_rob',
      badge: `main() calls robCircular(nums)`,
      badgeType: 'info',
      state: mkState({ frames: [{ name: 'main()', args: `n=${n}` }] })
    });
    steps.push({
      code: 'm_entry',
      badge: `robCircular(nums) — circular layout, two sub-problems needed`,
      badgeType: 'info',
      state: mkState({ frames: [{ name: 'main()', args: `n=${n}` }, { name: 'robCircular()', args: `n=${n}` }] })
    });
    steps.push({
      code: 'm_get_n',
      badge: `n = ${n}`,
      badgeType: 'info',
      state: mkState({ frames: [{ name: 'main()', args: `n=${n}` }, { name: 'robCircular()', args: `n=${n}` }] })
    });
    steps.push({
      code: 'm_check_one',
      badge: `n == 1? -> ${n === 1 ? 'TRUE' : 'FALSE'}`,
      badgeType: 'info',
      state: mkState({ frames: [{ name: 'main()', args: `n=${n}` }, { name: 'robCircular()', args: `n=${n}` }] })
    });

    if (n === 1) {
      steps.push({
        code: 'm_ret_one',
        badge: `Only 1 house: return ${nums[0]}`,
        badgeType: 'success',
        state: mkState({ currentReturn: nums[0], frames: [{ name: 'main()', args: `maxLoot=${nums[0]}` }] })
      });
      steps.push({
        code: 'm_print',
        badge: `Output: ${nums[0]}`,
        badgeType: 'success',
        state: mkState({ currentReturn: nums[0], frames: [{ name: 'main()', args: `maxLoot=${nums[0]}` }] })
      });
      steps.push({
        code: 'm_done',
        badge: `Bottom-Up Tabulation complete. Answer = ${nums[0]}`,
        badgeType: 'success',
        state: mkState({ currentReturn: nums[0], frames: [{ name: 'main()', args: `maxLoot=${nums[0]}` }] })
      });
      return steps;
    }

    // Case 1: robRange(nums, 0, n-2)
    steps.push({
      code: 'm_case1',
      badge: `CASE 1: robRange(nums, 0, ${n-2}) — exclude last house (${n-1})`,
      badgeType: 'info',
      state: mkState({ caseLabel: 'Case 1',
        frames: [{ name: 'main()', args: `n=${n}` }, { name: 'robCircular()', args: 'case1=?' }] })
    });
    steps.push({
      code: 'h_entry',
      badge: `Entering robRange(s=0, e=${n-2}) — linear rob on houses 0..${n-2}`,
      badgeType: 'info',
      state: mkState({ caseLabel: 'Case 1',
        frames: [{ name: 'main()', args: `n=${n}` }, { name: 'robCircular()', args: 'case1=?' }, { name: `robRange(0,${n-2})`, args: '' }] })
    });
    steps.push({
      code: 'h_base_s',
      badge: `s == e? 0 == ${n-2} -> ${n === 2 ? 'TRUE' : 'FALSE'}`,
      badgeType: 'info',
      state: mkState({ caseLabel: 'Case 1',
        frames: [{ name: 'main()', args: `n=${n}` }, { name: 'robCircular()', args: 'case1=?' }, { name: `robRange(0,${n-2})`, args: '' }] })
    });

    if (n === 2) {
      case1Val = nums[0];
      curCase1 = case1Val;
      steps.push({
        code: 'h_ret_s',
        badge: `Single element range: return nums[0]=$${nums[0]}`,
        badgeType: 'success',
        state: mkState({ currentReturn: case1Val, caseLabel: 'Case 1 done',
          frames: [{ name: 'main()', args: `n=${n}` }, { name: 'robCircular()', args: `case1=$${case1Val}` }] })
      });
    } else {
      const c1frames = (extra) => [{ name: 'main()', args: `n=${n}` }, { name: 'robCircular()', args: 'case1=?' }, { name: `robRange(0,${n-2})`, args: extra }];
      steps.push({
        code: 'h_alloc_dp',
        badge: `Allocate dp[0..${n-2}] of size ${n-1}`,
        badgeType: 'info',
        state: mkState({ caseLabel: 'Case 1', frames: c1frames('dp=[]') })
      });
      dpArr1[0] = nums[0];
      curCase1 = dpArr1[0];
      steps.push({
        code: 'h_dp0',
        badge: `dp[0] = nums[0] = $${nums[0]}`,
        badgeType: 'info',
        state: mkState({ caseLabel: 'Case 1', activeHouseIdx1: 0, frames: c1frames(`dp[0]=$${dpArr1[0]}`) })
      });
      dpArr1[1] = Math.max(nums[0], nums[1]);
      curCase1 = dpArr1[1];
      steps.push({
        code: 'h_dp1',
        badge: `dp[1] = max(nums[0]=$${nums[0]}, nums[1]=$${nums[1]}) = $${dpArr1[1]}`,
        badgeType: 'info',
        state: mkState({ caseLabel: 'Case 1', activeHouseIdx1: 1, frames: c1frames(`dp[1]=$${dpArr1[1]}`) })
      });
      let ops = 2;
      for (let k = 2; k <= n - 2; k++) {
        totalIters++;
        steps.push({
          code: 'h_for_loop',
          badge: `Iteration #${totalIters}: k=${k} (House ${k}, $${nums[k]})`,
          badgeType: 'info',
          state: mkState({ totalOps: ops, caseLabel: 'Case 1', activeHouseIdx1: k, frames: c1frames(`k=${k}`) })
        });
        const sl = dpArr1[k - 1], rl = nums[k] + dpArr1[k - 2];
        dpArr1[k] = Math.max(sl, rl);
        curCase1 = dpArr1[k];
        ops++;
        steps.push({
          code: 'h_dp_trans',
          badge: `dp[${k}] = max(skip:dp[${k-1}]=$${sl}, rob:$${nums[k]}+dp[${k-2}]=$${dpArr1[k-2]}=$${rl}) -> $${dpArr1[k]}`,
          badgeType: 'success',
          state: mkState({ totalOps: ops, caseLabel: 'Case 1', activeHouseIdx1: k, frames: c1frames(`dp[${k}]=$${dpArr1[k]}`) })
        });
      }
      steps.push({
        code: 'h_for_loop',
        badge: `Loop exit: k=${n-1} > ${n-2} - Case 1 done (${totalIters} iterations)`,
        badgeType: 'info',
        state: mkState({ totalOps: ops, caseLabel: 'Case 1', frames: c1frames('done') })
      });
      case1Val = dpArr1[n - 2];
      curCase1 = case1Val;
      steps.push({
        code: 'h_ret_dp',
        badge: `Return dp[${n-2}] = $${case1Val} — Case 1 max loot`,
        badgeType: 'success',
        state: mkState({ totalOps: ops, caseLabel: 'Case 1 done', currentReturn: case1Val,
          frames: [{ name: 'main()', args: `n=${n}` }, { name: 'robCircular()', args: `case1=$${case1Val}` }] })
      });
    }

    // Case 2: robRange(nums, 1, n-1)
    steps.push({
      code: 'm_case2',
      badge: `CASE 2: robRange(nums, 1, ${n-1}) — exclude first house (0)`,
      badgeType: 'info',
      state: mkState({ caseLabel: 'Case 2', currentReturn: null,
        frames: [{ name: 'main()', args: `n=${n}` }, { name: 'robCircular()', args: `case1=$${case1Val}` }] })
    });
    steps.push({
      code: 'h_entry',
      badge: `Entering robRange(s=1, e=${n-1}) — linear rob on houses 1..${n-1}`,
      badgeType: 'info',
      state: mkState({ caseLabel: 'Case 2', frames: [{ name: 'main()', args: `n=${n}` }, { name: 'robCircular()', args: `case1=$${case1Val}` }, { name: `robRange(1,${n-1})`, args: '' }] })
    });
    steps.push({
      code: 'h_base_s',
      badge: `s == e? 1 == ${n-1} -> ${n === 2 ? 'TRUE' : 'FALSE'}`,
      badgeType: 'info',
      state: mkState({ caseLabel: 'Case 2', frames: [{ name: 'main()', args: `n=${n}` }, { name: 'robCircular()', args: `case1=$${case1Val}` }, { name: `robRange(1,${n-1})`, args: '' }] })
    });

    case2Val = null;
    if (n === 2) {
      case2Val = nums[1];
      curCase2 = case2Val;
      steps.push({
        code: 'h_ret_s',
        badge: `Single element range: return nums[1]=$${nums[1]}`,
        badgeType: 'success',
        state: mkState({ caseLabel: 'Case 2 done', currentReturn: case2Val,
          frames: [{ name: 'main()', args: `n=${n}` }, { name: 'robCircular()', args: `case1=$${case1Val},case2=$${case2Val}` }] })
      });
    } else {
      const c2frames = (extra) => [{ name: 'main()', args: `n=${n}` }, { name: 'robCircular()', args: `case1=$${case1Val}` }, { name: `robRange(1,${n-1})`, args: extra }];
      steps.push({
        code: 'h_alloc_dp',
        badge: `Allocate dp[0..${n-2}] of size ${n-1} for Case 2`,
        badgeType: 'info',
        state: mkState({ caseLabel: 'Case 2', frames: c2frames('dp=[]') })
      });
      dpArr2[0] = nums[1];
      curCase2 = dpArr2[0];
      steps.push({
        code: 'h_dp0',
        badge: `dp[0] = nums[1] = $${nums[1]}`,
        badgeType: 'info',
        state: mkState({ caseLabel: 'Case 2', activeHouseIdx2: 1, frames: c2frames(`dp[0]=$${dpArr2[0]}`) })
      });
      dpArr2[1] = Math.max(nums[1], nums[2]);
      curCase2 = dpArr2[1];
      steps.push({
        code: 'h_dp1',
        badge: `dp[1] = max(nums[1]=$${nums[1]}, nums[2]=$${nums[2]}) = $${dpArr2[1]}`,
        badgeType: 'info',
        state: mkState({ caseLabel: 'Case 2', activeHouseIdx2: 2, frames: c2frames(`dp[1]=$${dpArr2[1]}`) })
      });
      let ops = 2;
      for (let k = 2; k <= n - 2; k++) {
        totalIters++;
        const gh = 1 + k;
        steps.push({
          code: 'h_for_loop',
          badge: `Iteration #${totalIters}: k=${k} (House ${gh}, $${nums[gh]})`,
          badgeType: 'info',
          state: mkState({ totalOps: ops, caseLabel: 'Case 2', activeHouseIdx2: gh, frames: c2frames(`k=${k}`) })
        });
        const sl = dpArr2[k - 1], rl = nums[gh] + dpArr2[k - 2];
        dpArr2[k] = Math.max(sl, rl);
        curCase2 = dpArr2[k];
        ops++;
        steps.push({
          code: 'h_dp_trans',
          badge: `dp[${k}] = max(skip:dp[${k-1}]=$${sl}, rob:$${nums[gh]}+dp[${k-2}]=$${dpArr2[k-2]}=$${rl}) -> $${dpArr2[k]}`,
          badgeType: 'success',
          state: mkState({ totalOps: ops, caseLabel: 'Case 2', activeHouseIdx2: gh, frames: c2frames(`dp[${k}]=$${dpArr2[k]}`) })
        });
      }
      steps.push({
        code: 'h_for_loop',
        badge: `Loop exit: k=${n-1} > ${n-2} - Case 2 done (${totalIters} iterations)`,
        badgeType: 'info',
        state: mkState({ totalOps: ops, caseLabel: 'Case 2', frames: c2frames('done') })
      });
      case2Val = dpArr2[n - 2];
      curCase2 = case2Val;
      steps.push({
        code: 'h_ret_dp',
        badge: `Return dp[${n-2}] = $${case2Val} — Case 2 max loot`,
        badgeType: 'success',
        state: mkState({ totalOps: ops, caseLabel: 'Case 2 done', currentReturn: case2Val,
          frames: [{ name: 'main()', args: `n=${n}` }, { name: 'robCircular()', args: `case1=$${case1Val},case2=$${case2Val}` }] })
      });
    }

    const finalResult = Math.max(case1Val, case2Val);
    curCase1 = case1Val;
    curCase2 = case2Val;
    steps.push({
      code: 'm_ret_max2',
      badge: `Return max(case1=$${case1Val}, case2=$${case2Val}) = $${finalResult}`,
      badgeType: 'success',
      state: mkState({ caseLabel: 'Final', currentReturn: finalResult,
        frames: [{ name: 'main()', args: `maxLoot=$${finalResult}` }] })
    });
    steps.push({
      code: 'm_print',
      badge: `Output: $${finalResult}`,
      badgeType: 'success',
      state: mkState({ caseLabel: 'Final', currentReturn: finalResult,
        frames: [{ name: 'main()', args: `maxLoot=$${finalResult}` }] })
    });
    steps.push({
      code: 'm_done',
      badge: `Bottom-Up Tabulation complete. Answer = $${finalResult}`,
      badgeType: 'success',
      state: mkState({ caseLabel: 'Final', currentReturn: finalResult,
        frames: [{ name: 'main()', args: `maxLoot=$${finalResult}` }] })
    });
  }

  return steps;
}

/* ==================================================================== */
/* REACTIVE STATE & CONTROLS                                            */
/* ==================================================================== */
const currentApproach   = ref('recursion');
const inputN            = ref(4);
const inputArrText      = ref('1, 2, 3, 1');
const currentNums       = ref([1, 2, 3, 1]);
const lang              = ref('java');
const speed             = ref(650);
const si                = ref(0);
const playing           = ref(false);
const vizHeight         = ref(210);
const tableHeight       = ref(38);
const leftWidth         = ref(54);
const rightTab          = ref('code');

// Warning Limit Modal state
const showWarningModal  = ref(false);
const warningModalTitle = ref('');
const warningModalMsg   = ref('');
const warningModalLimit = ref(6);
const requestedLength   = ref(0);

const maxAllowedLength = computed(() => {
  if (currentApproach.value === 'recursion') return 6;
  if (currentApproach.value === 'memoization') return 10;
  return 12;
});

const stepsData = reactive({ steps: buildSteps('recursion', [1, 2, 3, 1]) });
const steps     = computed(() => stepsData.steps);
const s         = computed(() => steps.value[Math.max(0, Math.min(si.value, steps.value.length - 1))] || { state: {} });
const st        = computed(() => s.value.state || {});

const codeLines = computed(() => {
  const appCodes = CODES[currentApproach.value] || CODES.recursion;
  return appCodes[lang.value] || appCodes.java || [];
});
const pseudocodeLines = computed(() => PSEUDOCODES[currentApproach.value] || PSEUDOCODES.recursion);

function applyApproach(appId) {
  currentApproach.value = appId;
  applyInput();
}

let playTimer = null;

function parseNums(str) {
  const parts = str.split(/[\s,]+/).map(v => parseInt(v.trim(), 10)).filter(v => !isNaN(v) && v >= 0);
  return parts.length >= 1 ? parts : [1, 2, 3, 1];
}

function applyInput() {
  let parsed = parseNums(inputArrText.value);
  let nVal = parseInt(inputN.value, 10);
  if (isNaN(nVal) || nVal < 1) {
    nVal = parsed.length;
    inputN.value = nVal;
  }

  let arr = parsed.slice(0, nVal);
  if (arr.length < nVal) {
    nVal = arr.length;
    inputN.value = nVal;
  }

  const maxLen = maxAllowedLength.value;
  if (arr.length > maxLen) {
    requestedLength.value = arr.length;
    warningModalLimit.value = maxLen;
    const appName = currentApproach.value === 'tabulation' ? 'Tabulation' : (currentApproach.value === 'recursion' ? 'Brute Force' : 'Memoization');
    warningModalTitle.value = `${appName} Limit (Max: ${maxLen})`;
    warningModalMsg.value = `Input array has ${arr.length} elements, which exceeds the limit of ${maxLen} for responsive animation. Processing has been capped at ${maxLen} elements.`;
    showWarningModal.value = true;
    arr = arr.slice(0, maxLen);
    nVal = arr.length;
    inputN.value = nVal;
    inputArrText.value = arr.join(', ');
  }

  currentNums.value = arr;
  playing.value = false;
  stepsData.steps = buildSteps(currentApproach.value, arr);
  si.value = 0;
}

function closeWarningModal() {
  showWarningModal.value = false;
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
  }
}

const mainRef         = ref(null);
const hResizerRef     = ref(null);
const vizResizerRef   = ref(null);
const tableResizerRef = ref(null);

function initHResizer(resizerEl, leftWidthRef, minPct = 25, maxPct = 80) {
  if (!resizerEl) return () => {};
  let startX = 0, startW = 0;
  function onMouseDown(e) {
    startX = e.clientX;
    const c = resizerEl.parentElement;
    if (!c) return;
    startW = leftWidthRef.value;
    resizerEl.classList.add('drag');
    document.addEventListener('mousemove', onMouseMove);
    document.addEventListener('mouseup', onMouseUp);
    e.preventDefault();
  }
  function onMouseMove(e) {
    const c = resizerEl.parentElement;
    if (!c) return;
    const rect = c.getBoundingClientRect();
    leftWidthRef.value = Math.max(minPct, Math.min(maxPct, startW + (e.clientX - startX) / rect.width * 100));
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
  let startY = 0, startH = 0;
  function onMouseDown(e) {
    startY = e.clientY;
    startH = heightRef.value;
    resizerEl.classList.add('drag');
    document.addEventListener('mousemove', onMouseMove);
    document.addEventListener('mouseup', onMouseUp);
    e.preventDefault();
  }
  function onMouseMove(e) {
    heightRef.value = Math.max(minPx, Math.min(maxPx, startH + (e.clientY - startY)));
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
    <div class="navbar">
      <h2 class="navbar-title">{{ topic }} &mdash; {{ subTopic }}</h2>
      <img src="../../assets/logo.png" alt="Logo" />
    </div>

    <div class="slide-body">
      <div class="row-main">
        <div class="ll-root">
          <!-- Control Panel Toolbar -->
          <div class="ll-toolbar">
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

            <!-- Custom Input: n -->
            <div class="ll-input-group">
              <label>n =</label>
              <input
                type="number"
                v-model.number="inputN"
                class="ll-text-input ll-n-input"
                placeholder="4"
                min="1"
                :max="maxAllowedLength"
                @keyup.enter="applyInput"
              />
            </div>

            <!-- Custom Input: nums -->
            <div class="ll-input-group">
              <label>Houses =</label>
              <input
                type="text"
                v-model="inputArrText"
                class="ll-text-input"
                placeholder="1, 2, 3, 1"
                @keyup.enter="applyInput"
              />
              <span class="ll-input-hint">(max {{ maxAllowedLength }})</span>
            </div>

            <button class="ll-viz-btn" @click="applyInput">&#9654; Visualize</button>

            <!-- Playback Navigation Controls -->
            <div class="ll-nav-controls">
              <button class="ll-nav-btn" title="First step" @click="stepBy(-steps.length)">&#171;</button>
              <button class="ll-nav-btn" title="Previous step" @click="stepBy(-1)">&#8249; Prev</button>
              <button class="ll-play-btn" @click="togglePlay">
                {{ playing ? '⏸ Pause' : '▶ Play' }}
              </button>
              <button class="ll-nav-btn" title="Next step" @click="stepBy(1)">Next &#8250;</button>
              <button class="ll-nav-btn" title="Last step" @click="stepBy(steps.length)">&#187;</button>
            </div>
          </div>

          <!-- Main Workspace Split: Left Visuals & Right Code/Theory -->
          <div class="ll-main" ref="mainRef">
            <!-- Left Column -->
            <div class="ll-left-col" :style="{ width: leftWidth + '%' }">
              <!-- Top Houses Strip (Circular Layout Indicator) -->
              <div class="ll-house-banner">
                <div class="ll-house-banner-title">
                  Circular Layout &mdash; House 0 and House {{ currentNums.length - 1 }} are adjacent (cannot both be robbed):
                </div>
                <div class="ll-house-track">
                  <div
                    v-for="(val, hIdx) in currentNums"
                    :key="hIdx"
                    class="ll-house-card"
                    :class="{
                      'll-house-active': st.activeHouseIdx === hIdx || st.activeHouseIdx1 === hIdx || st.activeHouseIdx2 === hIdx,
                      'll-house-skip': (st.caseLabel === 'Case 1' || st.caseLabel === 'Case 1 done') && hIdx === currentNums.length - 1,
                      'll-house-rob':  st.caseLabel === 'Case 2' && hIdx === 0
                    }"
                  >
                    <div class="ll-house-roof">&#9650;</div>
                    <div class="ll-house-body">
                      <span class="ll-house-val">${{ val }}</span>
                    </div>
                    <div class="ll-house-idx">H{{ hIdx }}</div>
                    <div
                      v-if="((st.caseLabel === 'Case 1' || st.caseLabel === 'Case 1 done') && hIdx === currentNums.length - 1) || (st.caseLabel === 'Case 2' && hIdx === 0)"
                      class="ll-excl-tag"
                    >
                      excluded
                    </div>
                  </div>
                </div>
              </div>

              <!-- Visual Workspace Panel -->
              <div class="ll-viz-wrap" :style="{ height: vizHeight + 'px' }">
                <div class="ll-perm-area">
                  <!-- Metrics Ribbon -->
                  <div class="ll-ptrs">
                    <div class="ll-ptr-chip">n = <b class="ll-c-blue">{{ st.n || currentNums.length }}</b></div>
                    <div class="ll-ptr-chip" v-if="currentApproach === 'recursion' || currentApproach === 'memoization'">
                      Calls: <b class="ll-c-orange">{{ st.totalCalls || 0 }}</b>
                    </div>
                    <div class="ll-ptr-chip" v-if="currentApproach === 'tabulation'">
                      Iterations: <b class="ll-c-orange">{{ st.iterations !== undefined ? st.iterations : 0 }}</b>
                    </div>
                    <div class="ll-ptr-chip" v-if="currentApproach === 'recursion' && st.redundantCalls">
                      Redundant: <b class="ll-c-red">{{ st.redundantCalls }}</b>
                    </div>
                    <div class="ll-ptr-chip" v-if="currentApproach === 'memoization' && st.cacheHits">
                      Cache Hits: <b class="ll-c-green">{{ st.cacheHits }}</b>
                    </div>
                    <div class="ll-ptr-chip">
                      case1 = <b :class="st.case1 !== null && st.case1 !== undefined ? 'll-c-green' : 'll-c-purple'">{{ st.case1 !== null && st.case1 !== undefined ? '$' + st.case1 : '?' }}</b>
                    </div>
                    <div class="ll-ptr-chip">
                      case2 = <b :class="st.case2 !== null && st.case2 !== undefined ? 'll-c-green' : 'll-c-purple'">{{ st.case2 !== null && st.case2 !== undefined ? '$' + st.case2 : '?' }}</b>
                    </div>
                    <div class="ll-ptr-chip" v-if="st.currentReturn !== null && st.currentReturn !== undefined">
                      Result: <b class="ll-c-green">${{ st.currentReturn }}</b>
                    </div>
                  </div>

                  <!-- TABULATION: Two DP Array Strips -->
                  <div v-if="currentApproach === 'tabulation'" class="ll-dp-tab-view">
                    <!-- Case 1 Strip -->
                    <div class="ll-section-caption">
                      <span>Case 1 &mdash; Houses 0..{{ currentNums.length - 2 }} (exclude last) &mdash; <code>dp[0..{{ currentNums.length - 2 }}]</code></span>
                    </div>
                    <div class="ll-arr-track">
                      <template v-for="cell in st.dpCells1" :key="'c1-' + cell.idx">
                        <div class="ll-arr-cell-wrap">
                          <div class="ll-ptr-tag-wrap">
                            <span v-if="cell.status === 'active'" class="ll-ptr-lbl ll-lbl-blue">&darr; i</span>
                          </div>
                          <div
                            class="ll-arr-box"
                            :class="{
                              'll-box-base': cell.status === 'base',
                              'll-box-cur':  cell.status === 'active',
                              'll-box-found': cell.status === 'computed',
                              'll-box-uncalc': cell.status === 'uncalculated'
                            }"
                          >
                            {{ cell.val !== '?' ? `$${cell.val}` : '?' }}
                          </div>
                          <div class="ll-arr-idx">dp[{{ cell.idx }}]</div>
                          <div class="ll-arr-house-tag">H{{ cell.houseGlobalIdx }}</div>
                        </div>
                      </template>
                    </div>

                    <!-- Case 2 Strip -->
                    <div class="ll-section-caption" style="margin-top: 6px;">
                      <span>Case 2 &mdash; Houses 1..{{ currentNums.length - 1 }} (exclude first) &mdash; <code>dp[0..{{ currentNums.length - 2 }}]</code></span>
                    </div>
                    <div class="ll-arr-track">
                      <template v-for="cell in st.dpCells2" :key="'c2-' + cell.idx">
                        <div class="ll-arr-cell-wrap">
                          <div class="ll-ptr-tag-wrap">
                            <span v-if="cell.status === 'active'" class="ll-ptr-lbl ll-lbl-purple">&darr; i</span>
                          </div>
                          <div
                            class="ll-arr-box"
                            :class="{
                              'll-box-base':  cell.status === 'base',
                              'll-box-cur2':  cell.status === 'active',
                              'll-box-found': cell.status === 'computed',
                              'll-box-uncalc': cell.status === 'uncalculated'
                            }"
                          >
                            {{ cell.val !== '?' ? `$${cell.val}` : '?' }}
                          </div>
                          <div class="ll-arr-idx">dp[{{ cell.idx }}]</div>
                          <div class="ll-arr-house-tag">H{{ cell.houseGlobalIdx }}</div>
                        </div>
                      </template>
                    </div>
                  </div>

                  <!-- RECURSION / MEMOIZATION: Decision Tree -->
                  <div v-else class="ll-tree-container">
                    <div class="ll-section-caption">
                      <span>{{ currentApproach === 'recursion'
                        ? 'Rob vs Skip Decision Tree — Two sub-ranges shown sequentially'
                        : 'Pruned Memoization Tree — Cache eliminates duplicate subproblems' }}</span>
                      <span v-if="currentApproach === 'memoization' && st.caseLabel" class="ll-memo-badge-info">{{ st.caseLabel }}</span>
                      <span v-if="currentApproach === 'recursion' && st.caseLabel" class="ll-memo-badge-info">{{ st.caseLabel }}</span>
                    </div>

                    <div class="ll-tree-scroll-area">
                      <svg
                        :viewBox="`0 0 ${st.treeWidth || 300} ${st.treeHeight || 150}`"
                        :style="{ width: '100%', maxWidth: (st.treeWidth || 300) + 'px', height: (st.treeHeight || 150) + 'px', maxHeight: (st.treeHeight || 150) + 'px' }"
                        class="ll-tree-svg"
                      >
                        <g class="ll-tree-edges">
                          <line
                            v-for="(edge, idx) in st.treeEdges"
                            :key="idx"
                            :x1="edge.x1" :y1="edge.y1" :x2="edge.x2" :y2="edge.y2"
                            class="ll-tree-edge"
                            :class="{ 'll-edge-left': edge.isLeft, 'll-edge-right': !edge.isLeft }"
                          />
                        </g>
                        <g class="ll-tree-nodes">
                          <g
                            v-for="node in st.treeNodes"
                            :key="node.id"
                            :transform="`translate(${node.x}, ${node.y})`"
                            class="ll-tree-node-group"
                          >
                            <!-- Redundant badge -->
                            <g v-if="node.isRedundant" transform="translate(0, -17)">
                              <rect x="-27" y="-6" width="54" height="12" rx="3" fill="#ef4444" stroke="#dc2626" stroke-width="0.5" />
                              <text x="0" y="0" text-anchor="middle" class="ll-badge-text-red">Recomputing</text>
                            </g>
                            <!-- Cache-hit badge -->
                            <g v-else-if="node.state === 'cache-hit'" transform="translate(0, -17)">
                              <rect x="-27" y="-6" width="54" height="12" rx="3" fill="#9333ea" stroke="#7e22ce" stroke-width="0.5" />
                              <text x="0" y="0" text-anchor="middle" class="ll-badge-text-purple">Cache Hit O(1)</text>
                            </g>
                            <!-- Node rect -->
                            <rect
                              x="-24" y="-12" width="48" height="24" rx="4"
                              class="ll-node-rect"
                              :class="{
                                'll-node-active':   node.id === st.activeNodeId,
                                'll-node-solved':   node.state === 'solved',
                                'll-node-redundant': node.isRedundant,
                                'll-node-cachehit': node.state === 'cache-hit'
                              }"
                            />
                            <text x="0" y="-3" text-anchor="middle" class="ll-node-text-call">
                              rob({{ node.i }})
                            </text>
                            <text
                              x="0" y="7.5" text-anchor="middle" class="ll-node-text-val"
                              :class="{ 'll-val-active': node.id === st.activeNodeId, 'll-val-solved': node.state === 'solved', 'll-val-hit': node.state === 'cache-hit' }"
                            >
                              {{ node.retVal !== null ? `=$${node.retVal}` : (node.state === 'cache-hit' ? 'Hit' : '?') }}
                            </text>
                          </g>
                        </g>
                      </svg>

                      <!-- Memoization Cache Strip -->
                      <div v-if="currentApproach === 'memoization' && st.memo && st.memo.length > 0" class="ll-memo-strip-wrap">
                        <div class="ll-memo-title">Cache Table &mdash; <code>memo[0..{{ st.memo.length - 1 }}]</code> (indexed from sub-range start):</div>
                        <div class="ll-memo-strip">
                          <template v-for="(mVal, mIdx) in st.memo" :key="mIdx">
                            <div class="ll-memo-cell-wrap">
                              <div class="ll-memo-cell" :class="{ 'll-memo-hit': mVal !== -1, 'll-memo-empty': mVal === -1 }">
                                {{ mVal !== -1 ? `$${mVal}` : -1 }}
                              </div>
                              <span class="ll-memo-idx">m[{{ mIdx }}]</span>
                            </div>
                          </template>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>

              <!-- Vertical Resizer -->
              <div class="ll-vresizer" ref="vizResizerRef"></div>

              <!-- Legend -->
              <div class="ll-legend">
                <template v-if="currentApproach === 'tabulation'">
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-base"></span>Base Cases</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-cur"></span>Active (Case 1)</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-purple"></span>Active (Case 2)</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-new"></span>Computed</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-uncalc"></span>Uncalculated</span>
                </template>
                <template v-else-if="currentApproach === 'memoization'">
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-cur"></span>Active Frame</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-new"></span>Solved &amp; Cached</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-purple"></span>Cache Hit (O(1))</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-uncalc"></span>Uncalled</span>
                </template>
                <template v-else>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-cur"></span>Active Frame</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-new"></span>Solved</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-red"></span>Redundant Call</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-uncalc"></span>Uncalled</span>
                </template>
              </div>

              <!-- Call Stack -->
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

              <!-- Vertical Resizer -->
              <div class="ll-vresizer" ref="tableResizerRef"></div>

              <!-- Step Badge -->
              <div class="ll-badge-wrap">
                <div
                  class="ll-badge"
                  :class="{ 'll-badge-error': s.badgeType === 'warn', 'll-badge-success': s.badgeType === 'success' }"
                  v-html="s.badge || 'Initializing...'"
                ></div>
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

                <div v-if="rightTab === 'code'" class="ll-code-scroll">
                  <pre class="ll-pre"><span
                    v-for="(line, i) in codeLines"
                    :key="i"
                    class="ll-codeline"
                    :class="{ 'll-hl': line[0] && line[0] === s.code }"
                  >{{ line[1] === '' ? ' ' : line[1] }}</span></pre>
                </div>

                <div v-else-if="rightTab === 'pseudo'" class="ll-code-scroll">
                  <pre class="ll-pre"><span
                    v-for="(line, i) in pseudocodeLines"
                    :key="i"
                    class="ll-codeline"
                  >{{ line }}</span></pre>
                </div>

                <div v-else class="ll-info-scroll">
                  <h3>House Robber II — The Circular Constraint</h3>
                  <p>
                    In <strong>House Robber II</strong> (LeetCode 213), houses are arranged in a <strong>circle</strong>: House 0 and House n&minus;1 are neighbours. The thief cannot rob two adjacent houses, meaning houses 0 and n&minus;1 cannot both be robbed.
                  </p>
                  <p><strong>Key Insight:</strong> Break the circle into two independent linear problems:</p>
                  <ul>
                    <li><strong>Case 1:</strong> Rob houses <code>0</code> to <code>n&minus;2</code> (exclude the last house)</li>
                    <li><strong>Case 2:</strong> Rob houses <code>1</code> to <code>n&minus;1</code> (exclude the first house)</li>
                  </ul>
                  <p>Answer = <code>max(case1, case2)</code></p>

                  <h3>Approaches Comparison</h3>
                  <table class="ll-complexity-table">
                    <thead><tr><th>Approach</th><th>Time</th><th>Space</th><th>Repeated Subproblems</th></tr></thead>
                    <tbody>
                      <tr><td><strong>Brute Force</strong></td><td>O(2<sup>n</sup>)</td><td>O(n) Stack</td><td><span class="ll-c-red" style="font-weight:700">YES</span> — Exponential duplication</td></tr>
                      <tr><td><strong>Memoization</strong></td><td>O(n)</td><td>O(n) Cache + Stack</td><td><span class="ll-c-green" style="font-weight:700">NO</span> — O(1) lookup on hit</td></tr>
                      <tr><td><strong>Tabulation</strong></td><td>O(n)</td><td>O(n) DP Array</td><td><span class="ll-c-green" style="font-weight:700">NO</span> — Single iterative pass</td></tr>
                    </tbody>
                  </table>

                  <h3>Recurrence</h3>
                  <p class="ll-math-box">robCircular(nums) = max(robRange(0, n-2), robRange(1, n-1))</p>
                  <p class="ll-math-box">robRange(s, e): dp[i] = max(dp[i-1], nums[s+i] + dp[i-2])</p>

                  <div class="ll-note">
                    <strong>Key Takeaway:</strong> The circular adjacency between House 0 and House n&minus;1 is resolved by running two independent linear subproblems — one excluding each endpoint. The maximum of the two cases is always the global optimum.
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

          <!-- Centered Warning Limit Modal -->
          <transition name="ll-modal-fade">
            <div v-if="showWarningModal" class="ll-modal-backdrop" @click.self="closeWarningModal">
              <div class="ll-modal-card" @click.stop>
                <div class="ll-modal-header">
                  <div class="ll-modal-title-wrap">
                    <div class="ll-modal-icon-badge">
                      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.2" class="ll-modal-svg-icon">
                        <circle cx="12" cy="12" r="10"></circle>
                        <line x1="12" y1="8" x2="12" y2="12"></line>
                        <line x1="12" y1="16" x2="12.01" y2="16"></line>
                      </svg>
                    </div>
                    <span class="ll-modal-title">{{ warningModalTitle }}</span>
                  </div>
                  <button class="ll-modal-close-btn" @click="closeWarningModal" title="Close warning">&times;</button>
                </div>

                <div class="ll-modal-body">
                  <div class="ll-modal-badge-row">
                    <span class="ll-modal-tag-entered">Entered Length: {{ requestedLength }}</span>
                    <span class="ll-modal-arrow">&rarr;</span>
                    <span class="ll-modal-tag-applied">Capped at Max: {{ warningModalLimit }}</span>
                  </div>
                  <p class="ll-modal-message">{{ warningModalMsg }}</p>
                </div>

                <div class="ll-modal-footer">
                  <button class="ll-modal-confirm-btn" @click="closeWarningModal">
                    Got it, Proceed
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

.ll-toolbar { margin-top: 4px; display: flex; align-items: center; gap: 6px; padding: 5px 12px; background: var(--surface); border-bottom: 1px solid var(--border); flex-shrink: 0; flex-wrap: wrap; box-shadow: var(--shadow-sm); }
.ll-approach-group { display: flex; gap: 2px; background: var(--surface2); padding: 2px; border-radius: var(--radius-sm); border: 1px solid var(--border); }
.ll-approach-btn { background: transparent; border: none; padding: 4px 8px; font-size: 11px; font-weight: 600; color: var(--text2); border-radius: 4px; cursor: pointer; transition: all .15s; white-space: nowrap; }
.ll-approach-btn:hover { color: var(--coral); }
.ll-approach-btn.active { background: var(--coral); color: #fff; box-shadow: var(--shadow-sm); }

.ll-input-group { display: flex; align-items: center; gap: 4px; }
.ll-input-group label { font-size: 11.5px; color: var(--muted); font-weight: 700; }
.ll-text-input { background: var(--surface); border: 1px solid var(--border2); color: var(--text); border-radius: var(--radius-sm); padding: 3px 6px; font-size: 11.5px; font-family: monospace; width: 95px; }
.ll-text-input:focus { outline: none; border-color: var(--coral); box-shadow: 0 0 0 3px rgba(240,77,77,.1); }
.ll-n-input { width: 44px !important; text-align: center; }
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
.ll-right-col { display: flex; flex-direction: column; flex: 1; overflow: hidden; min-width: 0; }

.ll-house-banner { padding: 6px 12px 10px; background: var(--surface2); border-bottom: 1px solid var(--border); flex-shrink: 0; }
.ll-house-banner-title { font-size: 10px; font-weight: 700; color: var(--text2); margin-bottom: 4px; }
.ll-house-track { display: flex; gap: 8px; overflow-x: auto; padding: 4px 4px 6px 4px; }
.ll-house-card { position: relative; display: flex; flex-direction: column; align-items: center; background: var(--surface); border: 1.5px solid var(--border2); border-radius: 6px; padding: 4px 6px; min-width: 50px; min-height: 56px; justify-content: flex-start; transition: all .2s ease; box-shadow: var(--shadow-sm); }
.ll-house-roof { font-size: 10px; color: #ef5050; line-height: 1; margin-bottom: -2px; }
.ll-house-body { display: flex; align-items: center; }
.ll-house-val { font-family: monospace; font-size: 11px; font-weight: 800; color: #047857; }
.ll-house-idx { font-size: 9px; color: var(--muted); margin-top: 1px; }
.ll-house-active { border-color: #f59e0b !important; background: #fffbeb !important; transform: translateY(-2px); box-shadow: 0 0 0 2.5px rgba(245,158,11,.3) !important; animation: ll-pulse 1.3s infinite ease-in-out; }
.ll-house-skip { border-color: #f97316 !important; background: #fff7ed !important; opacity: 0.6; }
.ll-house-rob  { border-color: #9333ea !important; background: #f3e8ff !important; opacity: 0.6; }
.ll-excl-tag { font-size: 8px; font-weight: 700; color: #dc2626; white-space: nowrap; background: #fef2f2; border: 1px solid #fecaca; padding: 1px 4px; border-radius: 3px; margin-top: 2px; line-height: 1.2; }

.ll-viz-wrap { flex-shrink: 0; background: var(--surface); border-bottom: 1px solid var(--border); position: relative; overflow-x: auto; overflow-y: auto; }
.ll-perm-area { display: flex; flex-direction: column; align-items: stretch; min-height: 100%; width: 100%; min-width: 0; box-sizing: border-box; }
.ll-ptrs { display: flex; gap: 6px; flex-wrap: wrap; padding: 4px 10px; min-height: 28px; width: 100%; box-sizing: border-box; min-width: 0; }
.ll-ptr-chip { background: var(--surface2); border: 1px solid var(--border); border-radius: var(--radius-sm); padding: 2px 7px; font-size: 11px; font-family: monospace; box-shadow: var(--shadow-sm); white-space: nowrap; flex-shrink: 0; }
.ll-c-blue { color: var(--blue); } .ll-c-orange { color: var(--orange); } .ll-c-green { color: var(--green); } .ll-c-purple { color: var(--purple); } .ll-c-red { color: var(--red); }

.ll-section-caption { display: flex; justify-content: space-between; align-items: center; padding: 3px 10px; font-size: 10.5px; font-weight: 700; color: var(--text2); background: var(--surface2); border-top: 1px solid var(--border); border-bottom: 1px solid var(--border); }
.ll-calc-pill { background: #dcfce7; color: #15803d; padding: 1px 6px; border-radius: 10px; font-size: 10.5px; font-family: monospace; font-weight: 700; }
.ll-memo-badge-info { background: var(--purple-light); color: var(--purple); padding: 1px 6px; border-radius: 10px; font-size: 10px; }

.ll-dp-tab-view { display: flex; flex-direction: column; padding: 4px 8px; }
.ll-arr-track { display: flex; align-items: flex-start; flex-wrap: wrap; padding: 4px 4px; gap: 8px; width: 100%; box-sizing: border-box; }
.ll-arr-cell-wrap { display: flex; flex-direction: column; align-items: center; }
.ll-ptr-tag-wrap { height: 22px; display: flex; align-items: flex-end; justify-content: center; margin-bottom: 2px; }
.ll-ptr-lbl { font-size: 10px; font-weight: 800; font-family: 'Consolas', monospace; display: inline-flex; align-items: center; line-height: 1; white-space: nowrap; animation: ll-pop 0.2s ease; }
.ll-lbl-orange { color: #f97316; } .ll-lbl-purple { color: #9333ea; } .ll-lbl-blue { color: #2563eb; }
.ll-arr-box { width: 46px; height: 40px; display: flex; align-items: center; justify-content: center; border: 2px solid var(--border2); border-radius: var(--radius); background: var(--surface); color: var(--text); font-weight: 700; font-size: 12px; box-shadow: var(--shadow-sm); transition: all 0.25s ease; animation: ll-pop .3s ease; }
.ll-box-base  { border-color: #3b82f6 !important; background: #eff6ff !important; color: #1d4ed8 !important; }
.ll-box-cur   { border-color: #f59e0b !important; background: #fef3c7 !important; color: #92400e !important; box-shadow: 0 0 0 3px rgba(245,158,11,.25) !important; transform: translateY(-2px); }
.ll-box-cur2  { border-color: #9333ea !important; background: #f3e8ff !important; color: #6b21a8 !important; box-shadow: 0 0 0 3px rgba(147,51,234,.25) !important; transform: translateY(-2px); }
.ll-box-found { border-color: #10b981 !important; background: #dcfce7 !important; color: #065f46 !important; }
.ll-box-uncalc { border: 2px dashed var(--border2) !important; background: var(--surface2) !important; color: var(--muted) !important; }
.ll-arr-idx { font-size: 10px; color: var(--muted); margin-top: 2px; font-family: 'Consolas', monospace; font-weight: 600; }
.ll-arr-house-tag { font-size: 9px; color: var(--muted); font-family: monospace; }

.ll-tree-container { display: flex; flex-direction: column; width: 100%; height: 100%; min-height: 0; }
.ll-tree-scroll-area { flex: 1; overflow: auto; padding: 2px 6px 4px; display: flex; flex-direction: column; align-items: center; justify-content: flex-start; min-height: 0; width: 100%; box-sizing: border-box; }
.ll-tree-svg { display: block; flex-shrink: 0; max-width: 100%; margin: 0 auto; }
.ll-tree-edge { stroke: #cbd5e1; stroke-width: 1.8px; transition: stroke .2s; }
.ll-edge-left { stroke: #94a3b8; }
.ll-edge-right { stroke: #cbd5e1; stroke-dasharray: 4, 3; }
.ll-tree-node-group { cursor: default; }
.ll-node-rect { fill: #ffffff; stroke: #cbd5e1; stroke-width: 1.4px; filter: drop-shadow(0 1px 2px rgba(0,0,0,.05)); transition: all .25s ease; }
.ll-node-active   { stroke: #f59e0b !important; stroke-width: 2.2px !important; fill: #fffbeb !important; filter: drop-shadow(0 0 5px rgba(245,158,11,.45)) !important; animation: ll-pulse 1.3s infinite ease-in-out; }
.ll-node-solved   { stroke: #10b981 !important; fill: #dcfce7 !important; stroke-width: 1.6px !important; }
.ll-node-redundant { stroke: #ef4444 !important; stroke-dasharray: 3, 2 !important; fill: #fef2f2 !important; }
.ll-node-cachehit { stroke: #9333ea !important; stroke-width: 1.8px !important; fill: #f3e8ff !important; }
.ll-tree-svg text { font-family: 'Segoe UI', system-ui, sans-serif !important; user-select: none; }
.ll-node-text-call { font-family: 'Consolas','Fira Code',monospace !important; font-size: 9.5px !important; font-weight: 700 !important; fill: #1e293b !important; dominant-baseline: central !important; }
.ll-node-text-val  { font-family: 'Consolas','Fira Code',monospace !important; font-size: 9px !important; font-weight: 700 !important; fill: #64748b !important; dominant-baseline: central !important; }
.ll-val-active { fill: #b45309 !important; } .ll-val-solved { fill: #047857 !important; } .ll-val-hit { fill: #7e22ce !important; }
.ll-badge-text-red    { font-family: -apple-system,BlinkMacSystemFont,'Segoe UI',sans-serif !important; font-size: 6.8px !important; font-weight: 700 !important; fill: #ffffff !important; dominant-baseline: central !important; }
.ll-badge-text-purple { font-family: -apple-system,BlinkMacSystemFont,'Segoe UI',sans-serif !important; font-size: 6.8px !important; font-weight: 700 !important; fill: #ffffff !important; dominant-baseline: central !important; }

.ll-memo-strip-wrap { width: 100%; padding: 4px 8px 6px; border-top: 1px dashed var(--border); background: var(--surface2); flex-shrink: 0; margin-top: 4px; box-sizing: border-box; }
.ll-memo-title { font-size: 10px; font-weight: 700; color: var(--text2); margin-bottom: 2px; }
.ll-memo-strip { display: flex; gap: 5px; flex-wrap: wrap; }
.ll-memo-cell-wrap { display: flex; flex-direction: column; align-items: center; }
.ll-memo-cell { width: 44px; height: 26px; display: flex; align-items: center; justify-content: center; font-size: 11px; font-family: monospace; font-weight: 700; border-radius: 4px; border: 1px solid var(--border); background: var(--surface); color: var(--text); }
.ll-memo-hit   { background: #f3e8ff !important; border-color: #a855f7 !important; color: #6b21a8 !important; }
.ll-memo-empty { color: var(--muted); border-style: dashed; }
.ll-memo-idx { font-size: 9px; color: var(--muted); font-family: monospace; }

.ll-vresizer { height: 5px; cursor: row-resize; background: var(--border); flex-shrink: 0; transition: background .15s; position: relative; z-index: 20; }
.ll-vresizer:hover, .ll-vresizer.drag { background: var(--coral); }

.ll-legend { display: flex; flex-wrap: wrap; gap: 4px 10px; padding: 4px 10px; border-bottom: 1px solid var(--border); flex-shrink: 0; background: var(--surface2); }
.ll-leg { display: flex; align-items: center; gap: 4px; font-size: 10.5px; color: var(--text2); font-weight: 500; }
.ll-legdot { width: 9px; height: 9px; border-radius: 2px; flex-shrink: 0; display: inline-block; }
.ll-legdot-base   { background: #eff6ff; border: 1.5px solid #3b82f6; }
.ll-legdot-cur    { background: #fef3c7; border: 1.5px solid #f59e0b; }
.ll-legdot-new    { background: #dcfce7; border: 1.5px solid #10b981; }
.ll-legdot-purple { background: #f3e8ff; border: 1.5px solid #9333ea; }
.ll-legdot-red    { background: #fef2f2; border: 1.5px dashed #ef4444; }
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
.ll-badge-error   { border-left-color: var(--red);   background: var(--red-light);   color: var(--red-dark); font-weight: 600; }
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
.ll-pre { font-family: 'Cascadia Code','Fira Code','Consolas',monospace; font-size: 11px; line-height: 1.5; white-space: pre; color: var(--text); margin: 0; }
.ll-codeline { display: block; padding: 0 14px; margin: 0 -14px; }
.ll-hl { background: #dcfce7; color: #15803d; border-radius: 3px; border-left: 3px solid var(--green); font-weight: 600; }

.ll-info-scroll { flex: 1; overflow: auto; padding: 12px 16px; background: var(--surface); color: var(--text2); font-size: 12px; line-height: 1.55; }
.ll-info-scroll h3 { margin: 0 0 6px; color: var(--text); font-size: 13px; font-weight: 700; }
.ll-info-scroll h3:not(:first-child) { margin-top: 14px; }
.ll-info-scroll p { margin: 0 0 6px; }
.ll-info-scroll ul { margin: 0 0 10px 16px; padding: 0; }
.ll-info-scroll li { margin-bottom: 3px; }
.ll-info-scroll code { background: var(--surface2); border: 1px solid var(--border); border-radius: 4px; padding: 1px 4px; font-family: 'Consolas',monospace; font-size: 11px; color: var(--coral-dark); }
.ll-complexity-table { width: 100%; border-collapse: collapse; margin-bottom: 10px; font-size: 11.5px; }
.ll-complexity-table th, .ll-complexity-table td { border: 1px solid var(--border); padding: 6px 8px; text-align: left; }
.ll-complexity-table th { background: var(--surface2); color: var(--text); font-weight: 700; }
.ll-math-box { background: var(--surface2); border: 1px solid var(--border2); padding: 6px 10px; border-radius: var(--radius-sm); font-family: 'Consolas',monospace; font-weight: 700; color: #1e293b; margin-bottom: 6px; }
.ll-note { background: var(--orange-light); border-left: 3px solid var(--orange); border-radius: var(--radius-sm); padding: 6px 10px; font-size: 11.5px; color: var(--text2); margin-top: 8px; }

.ll-footer { padding: 3px 12px; font-size: 10.5px; color: var(--muted); border-top: 1px solid var(--border); background: var(--surface); flex-shrink: 0; display: flex; align-items: center; }
.ll-speed-wrap { display: flex; align-items: center; gap: 4px; margin-left: 14px; }
.ll-speed-wrap input[type=range] { width: 80px; accent-color: var(--coral); }

/* Warning popup modal styling with centered layout */
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
  animation: ll-pop 0.25s cubic-bezier(0.16, 1, 0.3, 1);
}

.ll-modal-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 12px 16px;
  background: #fef2f2;
  border-bottom: 1px solid #fee2e2;
}

.ll-modal-title-wrap {
  display: flex;
  align-items: center;
  gap: 10px;
}

.ll-modal-icon-badge {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 28px;
  height: 28px;
  border-radius: 50%;
  background: #fee2e2;
  color: #ef4444;
  flex-shrink: 0;
}

.ll-modal-svg-icon {
  width: 16px;
  height: 16px;
}

.ll-modal-title {
  font-size: 13px;
  font-weight: 700;
  color: #991b1b;
}

.ll-modal-close-btn {
  background: transparent;
  border: none;
  font-size: 20px;
  line-height: 1;
  color: #9ca3af;
  cursor: pointer;
  padding: 2px 6px;
  border-radius: 4px;
  transition: all 0.15s;
}

.ll-modal-close-btn:hover {
  background: rgba(0, 0, 0, 0.06);
  color: #374151;
}

.ll-modal-body {
  padding: 14px 16px;
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.ll-modal-badge-row {
  display: flex;
  align-items: center;
  gap: 8px;
  flex-wrap: wrap;
  font-family: 'Consolas', monospace;
  font-size: 11px;
}

.ll-modal-tag-entered {
  padding: 3px 8px;
  border-radius: 4px;
  background: #fef2f2;
  color: #b91c1c;
  border: 1px solid #fca5a5;
  font-weight: 600;
}

.ll-modal-arrow {
  color: #64748b;
  font-weight: 700;
}

.ll-modal-tag-applied {
  padding: 3px 8px;
  border-radius: 4px;
  background: #f0fdf4;
  color: #15803d;
  border: 1px solid #86efac;
  font-weight: 600;
}

.ll-modal-message {
  font-size: 12px;
  color: #475569;
  line-height: 1.5;
  margin: 0;
}

.ll-modal-footer {
  padding: 10px 16px;
  background: #f8fafc;
  border-top: 1px solid #f1f5f9;
  display: flex;
  justify-content: flex-end;
}

.ll-modal-confirm-btn {
  background: #ef4444;
  color: #ffffff;
  border: none;
  padding: 6px 14px;
  border-radius: 6px;
  font-size: 11.5px;
  font-weight: 600;
  cursor: pointer;
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.05);
  transition: all 0.15s;
}

.ll-modal-confirm-btn:hover {
  background: #dc2626;
  box-shadow: 0 2px 4px rgba(220, 38, 38, 0.25);
}

.ll-modal-fade-enter-active, .ll-modal-fade-leave-active {
  transition: opacity 0.2s ease;
}
.ll-modal-fade-enter-from, .ll-modal-fade-leave-to {
  opacity: 0;
}

@media (max-width: 900px) {
  .ll-main { flex-direction: column; }
  .ll-left-col, .ll-right-col { max-width: 100% !important; width: 100% !important; }
  .ll-resizer { display: none; }
  .ll-toolbar { flex-direction: column; align-items: stretch; }
  .ll-nav-controls { margin-left: 0; justify-content: center; }
}
</style>
