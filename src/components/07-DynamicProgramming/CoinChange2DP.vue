<script setup>
import { ref, computed, reactive, onMounted, onUnmounted, watch } from 'vue';

const props = defineProps({
  topic: { type: String, default: 'Dynamic Programming' },
  subTopic: { type: String, default: 'Coin Change II (Number of Ways)' }
});

/* ==================================================================== */
/* APPROACHES & MULTI-LANGUAGE 100% EXECUTABLE CODE SPECIFICATIONS      */
/* ==================================================================== */
const APPROACHES = [
  { id: 'recursion', label: 'Brute Force', desc: 'O(2^(n+amt)) Exponential — Recursive Decision (Take vs Skip)' },
  { id: 'memoization', label: 'Memoization', desc: 'O(n×amt) Time & Space — Top-Down 2D Memo Grid' },
  { id: 'tabulation', label: 'Tabulation', desc: 'O(n×amt) Time & O(amt) Space — Bottom-Up 1D DP Array Accumulation' }
];

/* ==================================================================== */
/* MULTI-LANGUAGE CODE SNIPPETS WITH EXACT SYNCHRONIZED TAGS            */
/* ==================================================================== */
const CODES = {
  recursion: {
    java: [
      ['',             'import java.util.Scanner;'],
      ['',             ''],
      ['',             'public class Main {'],
      ['c_entry',      '    static int change(int i, int rem, int[] coins) {'],
      ['c_base_zero',  '        if (rem == 0) {'],
      ['c_ret_zero',   '            return 1;'],
      ['',             '        }'],
      ['c_base_oob',   '        if (rem < 0 || i >= coins.length) {'],
      ['c_ret_oob',    '            return 0;'],
      ['',             '        }'],
      ['c_init_take',  '        int take = 0;'],
      ['c_check_take', '        if (rem - coins[i] >= 0) {'],
      ['c_call_take',  '            take = change(i, rem - coins[i], coins);'],
      ['',             '        }'],
      ['c_call_skip',  '        int skip = change(i + 1, rem, coins);'],
      ['c_ret_sum',    '        return take + skip;'],
      ['',             '    }'],
      ['',             ''],
      ['',             '    public static void main(String[] args) {'],
      ['m_scanner',    '        Scanner sc = new Scanner(System.in);'],
      ['m_read_n',     '        int n = sc.nextInt();'],
      ['m_alloc_arr',  '        int[] coins = new int[n];'],
      ['m_for_read',   '        for (int i = 0; i < n; i++) {'],
      ['m_read_elem',  '            coins[i] = sc.nextInt();'],
      ['',             '        }'],
      ['m_read_amt',   '        int amount = sc.nextInt();'],
      ['m_call_fn',    '        int ans = change(0, amount, coins);'],
      ['m_print',      '        System.out.println(ans);'],
      ['m_done',       '    }'],
      ['',             '}']
    ],
    c: [
      ['',             '#include <stdio.h>'],
      ['',             ''],
      ['c_entry',      'int change(int i, int rem, int coins[], int n) {'],
      ['c_base_zero',  '    if (rem == 0) {'],
      ['c_ret_zero',   '        return 1;'],
      ['',             '    }'],
      ['c_base_oob',   '    if (rem < 0 || i >= n) {'],
      ['c_ret_oob',    '        return 0;'],
      ['',             '    }'],
      ['c_init_take',  '    int take = 0;'],
      ['c_check_take', '    if (rem - coins[i] >= 0) {'],
      ['c_call_take',  '        take = change(i, rem - coins[i], coins, n);'],
      ['',             '    }'],
      ['c_call_skip',  '    int skip = change(i + 1, rem, coins, n);'],
      ['c_ret_sum',    '    return take + skip;'],
      ['',             '}'],
      ['',             ''],
      ['',             'int main() {'],
      ['m_scanner',    '    int n;'],
      ['m_read_n',     '    scanf("%d", &n);'],
      ['m_alloc_arr',  '    int coins[100];'],
      ['m_for_read',   '    for (int i = 0; i < n; i++) {'],
      ['m_read_elem',  '        scanf("%d", &coins[i]);'],
      ['',             '    }'],
      ['m_read_amt',   '    int amount; scanf("%d", &amount);'],
      ['m_call_fn',    '    int ans = change(0, amount, coins, n);'],
      ['m_print',      '    printf("%d\\n", ans);'],
      ['m_done',       '    return 0;'],
      ['',             '}']
    ],
    cpp: [
      ['',             '#include <iostream>'],
      ['',             '#include <vector>'],
      ['',             'using namespace std;'],
      ['',             ''],
      ['c_entry',      'int change(int i, int rem, const vector<int>& coins) {'],
      ['c_base_zero',  '    if (rem == 0) {'],
      ['c_ret_zero',   '        return 1;'],
      ['',             '    }'],
      ['c_base_oob',   '    if (rem < 0 || i >= coins.size()) {'],
      ['c_ret_oob',    '        return 0;'],
      ['',             '    }'],
      ['c_init_take',  '    int take = 0;'],
      ['c_check_take', '    if (rem - coins[i] >= 0) {'],
      ['c_call_take',  '        take = change(i, rem - coins[i], coins);'],
      ['',             '    }'],
      ['c_call_skip',  '    int skip = change(i + 1, rem, coins);'],
      ['c_ret_sum',    '    return take + skip;'],
      ['',             '}'],
      ['',             ''],
      ['',             'int main() {'],
      ['m_scanner',    '    int n;'],
      ['m_read_n',     '    cin >> n;'],
      ['m_alloc_arr',  '    vector<int> coins(n);'],
      ['m_for_read',   '    for (int i = 0; i < n; i++) {'],
      ['m_read_elem',  '        cin >> coins[i];'],
      ['',             '    }'],
      ['m_read_amt',   '    int amount; cin >> amount;'],
      ['m_call_fn',    '    int ans = change(0, amount, coins);'],
      ['m_print',      '    cout << ans << endl;'],
      ['m_done',       '    return 0;'],
      ['',             '}']
    ],
    python: [
      ['',             'import sys'],
      ['',             ''],
      ['c_entry',      'def change(i, rem, coins):'],
      ['c_base_zero',  '    if rem == 0:'],
      ['c_ret_zero',   '        return 1'],
      ['c_base_oob',   '    if rem < 0 or i >= len(coins):'],
      ['c_ret_oob',    '        return 0'],
      ['c_init_take',  '    take = 0'],
      ['c_check_take', '    if rem - coins[i] >= 0:'],
      ['c_call_take',  '        take = change(i, rem - coins[i], coins)'],
      ['c_call_skip',  '    skip = change(i + 1, rem, coins)'],
      ['c_ret_sum',    '    return take + skip'],
      ['',             ''],
      ['if __name__ == "__main__":'],
      ['m_scanner',    '    tokens = sys.stdin.read().split()'],
      ['m_read_n',     '    if tokens:'],
      ['',             '        n = int(tokens[0])'],
      ['m_alloc_arr',  '        coins = []'],
      ['m_for_read',   '        for i in range(n):'],
      ['m_read_elem',  '            coins.append(int(tokens[1 + i]))'],
      ['m_read_amt',   '        amount = int(tokens[n+1])'],
      ['m_call_fn',    '        ans = change(0, amount, coins)'],
      ['m_print',      '        print(ans)'],
      ['m_done',       '        sys.exit(0)']
    ],
    javascript: [
      ['',             'const fs = require("fs");'],
      ['',             ''],
      ['c_entry',      'function change(i, rem, coins) {'],
      ['c_base_zero',  '    if (rem === 0) {'],
      ['c_ret_zero',   '        return 1;'],
      ['',             '    }'],
      ['c_base_oob',   '    if (rem < 0 || i >= coins.length) {'],
      ['c_ret_oob',    '        return 0;'],
      ['',             '    }'],
      ['c_init_take',  '    let take = 0;'],
      ['c_check_take', '    if (rem - coins[i] >= 0) {'],
      ['c_call_take',  '        take = change(i, rem - coins[i], coins);'],
      ['',             '    }'],
      ['c_call_skip',  '    const skip = change(i + 1, rem, coins);'],
      ['c_ret_sum',    '    return take + skip;'],
      ['',             '}'],
      ['',             ''],
      ['m_scanner',    'const tokens = fs.readFileSync(0, "utf-8").trim().split(/\\s+/);'],
      ['m_read_n',     'if (tokens.length > 1) {'],
      ['',             '    const n = parseInt(tokens[0], 10);'],
      ['m_alloc_arr',  '    const coins = [];'],
      ['m_for_read',   '    for (let i = 0; i < n; i++) {'],
      ['m_read_elem',  '        coins.push(parseInt(tokens[1 + i], 10));'],
      ['',             '    }'],
      ['m_read_amt',   '    const amount = parseInt(tokens[n + 1], 10);'],
      ['m_call_fn',    '    const ans = change(0, amount, coins);'],
      ['m_print',      '    console.log(ans);'],
      ['m_done',       '    process.exit(0);'],
      ['',             '}']
    ]
  },
  memoization: {
    java: [
      ['',             'import java.util.Scanner;'],
      ['',             'import java.util.Arrays;'],
      ['',             ''],
      ['',             'public class Main {'],
      ['c_entry',      '    static int solve(int i, int rem, int[] coins, int[][] memo) {'],
      ['c_base_zero',  '        if (rem == 0) {'],
      ['c_ret_zero',   '            return 1;'],
      ['',             '        }'],
      ['c_base_oob',   '        if (rem < 0 || i >= coins.length) {'],
      ['c_ret_oob',    '            return 0;'],
      ['',             '        }'],
      ['c_memo_chk',   '        if (memo[i][rem] != -1) {'],
      ['c_ret_cache',  '            return memo[i][rem];'],
      ['',             '        }'],
      ['c_init_take',  '        int take = 0;'],
      ['c_check_take', '        if (rem - coins[i] >= 0) {'],
      ['c_call_take',  '            take = solve(i, rem - coins[i], coins, memo);'],
      ['',             '        }'],
      ['c_call_skip',  '        int skip = solve(i + 1, rem, coins, memo);'],
      ['c_store_memo', '        memo[i][rem] = take + skip;'],
      ['c_ret_memo',   '        return memo[i][rem];'],
      ['',             '    }'],
      ['',             ''],
      ['',             '    public static void main(String[] args) {'],
      ['m_scanner',    '        Scanner sc = new Scanner(System.in);'],
      ['m_read_n',     '        int n = sc.nextInt();'],
      ['m_alloc_arr',  '        int[] coins = new int[n];'],
      ['m_for_read',   '        for (int i = 0; i < n; i++) {'],
      ['m_read_elem',  '            coins[i] = sc.nextInt();'],
      ['',             '        }'],
      ['m_read_amt',   '        int amount = sc.nextInt();'],
      ['m_alloc_memo', '        int[][] memo = new int[n][amount + 1];'],
      ['m_fill_memo',  '        for (int[] row : memo) {'],
      ['',             '            Arrays.fill(row, -1);'],
      ['',             '        }'],
      ['m_call_fn',    '        int ans = solve(0, amount, coins, memo);'],
      ['m_print',      '        System.out.println(ans);'],
      ['m_done',       '    }'],
      ['',             '}']
    ],
    c: [
      ['',             '#include <stdio.h>'],
      ['',             '#include <string.h>'],
      ['',             ''],
      ['c_entry',      'int solve(int i, int rem, int coins[], int n, int memo[][1001]) {'],
      ['c_base_zero',  '    if (rem == 0) {'],
      ['c_ret_zero',   '        return 1;'],
      ['',             '    }'],
      ['c_base_oob',   '    if (rem < 0 || i >= n) {'],
      ['c_ret_oob',    '        return 0;'],
      ['',             '    }'],
      ['c_memo_chk',   '    if (memo[i][rem] != -1) {'],
      ['c_ret_cache',  '        return memo[i][rem];'],
      ['',             '    }'],
      ['c_init_take',  '    int take = 0;'],
      ['c_check_take', '    if (rem - coins[i] >= 0) {'],
      ['c_call_take',  '        take = solve(i, rem - coins[i], coins, n, memo);'],
      ['',             '    }'],
      ['c_call_skip',  '    int skip = solve(i + 1, rem, coins, n, memo);'],
      ['c_store_memo', '    memo[i][rem] = take + skip;'],
      ['c_ret_memo',   '    return memo[i][rem];'],
      ['',             '}'],
      ['',             ''],
      ['',             'int main() {'],
      ['m_scanner',    '    int n;'],
      ['m_read_n',     '    scanf("%d", &n);'],
      ['m_alloc_arr',  '    int coins[100];'],
      ['m_for_read',   '    for (int i = 0; i < n; i++) {'],
      ['m_read_elem',  '        scanf("%d", &coins[i]);'],
      ['',             '    }'],
      ['m_read_amt',   '    int amount; scanf("%d", &amount);'],
      ['m_alloc_memo', '    int memo[100][1001];'],
      ['m_fill_memo',  '    memset(memo, -1, sizeof(memo));'],
      ['m_call_fn',    '    int ans = solve(0, amount, coins, n, memo);'],
      ['m_print',      '    printf("%d\\n", ans);'],
      ['m_done',       '    return 0;'],
      ['',             '}']
    ],
    cpp: [
      ['',             '#include <iostream>'],
      ['',             '#include <vector>'],
      ['',             'using namespace std;'],
      ['',             ''],
      ['c_entry',      'int solve(int i, int rem, const vector<int>& coins, vector<vector<int>>& memo) {'],
      ['c_base_zero',  '    if (rem == 0) {'],
      ['c_ret_zero',   '        return 1;'],
      ['',             '    }'],
      ['c_base_oob',   '    if (rem < 0 || i >= coins.size()) {'],
      ['c_ret_oob',    '        return 0;'],
      ['',             '    }'],
      ['c_memo_chk',   '    if (memo[i][rem] != -1) {'],
      ['c_ret_cache',  '        return memo[i][rem];'],
      ['',             '    }'],
      ['c_init_take',  '    int take = 0;'],
      ['c_check_take', '    if (rem - coins[i] >= 0) {'],
      ['c_call_take',  '        take = solve(i, rem - coins[i], coins, memo);'],
      ['',             '    }'],
      ['c_call_skip',  '    int skip = solve(i + 1, rem, coins, memo);'],
      ['c_store_memo', '    memo[i][rem] = take + skip;'],
      ['c_ret_memo',   '    return memo[i][rem];'],
      ['',             '}'],
      ['',             ''],
      ['',             'int main() {'],
      ['m_scanner',    '    int n;'],
      ['m_read_n',     '    cin >> n;'],
      ['m_alloc_arr',  '    vector<int> coins(n);'],
      ['m_for_read',   '    for (int i = 0; i < n; i++) {'],
      ['m_read_elem',  '        cin >> coins[i];'],
      ['',             '    }'],
      ['m_read_amt',   '    int amount; cin >> amount;'],
      ['m_alloc_memo', '    vector<vector<int>> memo(n, vector<int>(amount + 1, -1));'],
      ['m_fill_memo',  '    // memo table initialized to -1'],
      ['m_call_fn',    '    int ans = solve(0, amount, coins, memo);'],
      ['m_print',      '    cout << ans << endl;'],
      ['m_done',       '    return 0;'],
      ['',             '}']
    ],
    python: [
      ['',             'import sys'],
      ['',             ''],
      ['c_entry',      'def solve(i, rem, coins, memo):'],
      ['c_base_zero',  '    if rem == 0:'],
      ['c_ret_zero',   '        return 1'],
      ['c_base_oob',   '    if rem < 0 or i >= len(coins):'],
      ['c_ret_oob',    '        return 0'],
      ['c_memo_chk',   '    if memo[i][rem] != -1:'],
      ['c_ret_cache',  '        return memo[i][rem]'],
      ['c_init_take',  '    take = 0'],
      ['c_check_take', '    if rem - coins[i] >= 0:'],
      ['c_call_take',  '        take = solve(i, rem - coins[i], coins, memo)'],
      ['c_call_skip',  '    skip = solve(i + 1, rem, coins, memo)'],
      ['c_store_memo', '    memo[i][rem] = take + skip'],
      ['c_ret_memo',   '    return memo[i][rem]'],
      ['',             ''],
      ['if __name__ == "__main__":'],
      ['m_scanner',    '    tokens = sys.stdin.read().split()'],
      ['m_read_n',     '    if tokens:'],
      ['',             '        n = int(tokens[0])'],
      ['m_alloc_arr',  '        coins = []'],
      ['m_for_read',   '        for i in range(n):'],
      ['m_read_elem',  '            coins.append(int(tokens[1 + i]))'],
      ['m_read_amt',   '        amount = int(tokens[n+1])'],
      ['m_alloc_memo', '        memo = [[-1] * (amount + 1) for _ in range(n)]'],
      ['m_fill_memo',  '        # 2D memo grid ready'],
      ['m_call_fn',    '        ans = solve(0, amount, coins, memo)'],
      ['m_print',      '        print(ans)'],
      ['m_done',       '        sys.exit(0)']
    ],
    javascript: [
      ['',             'const fs = require("fs");'],
      ['',             ''],
      ['c_entry',      'function solve(i, rem, coins, memo) {'],
      ['c_base_zero',  '    if (rem === 0) {'],
      ['c_ret_zero',   '        return 1;'],
      ['',             '    }'],
      ['c_base_oob',   '    if (rem < 0 || i >= coins.length) {'],
      ['c_ret_oob',    '        return 0;'],
      ['',             '    }'],
      ['c_memo_chk',   '    if (memo[i][rem] !== -1) {'],
      ['c_ret_cache',  '        return memo[i][rem];'],
      ['',             '    }'],
      ['c_init_take',  '    let take = 0;'],
      ['c_check_take', '    if (rem - coins[i] >= 0) {'],
      ['c_call_take',  '        take = solve(i, rem - coins[i], coins, memo);'],
      ['',             '    }'],
      ['c_call_skip',  '    const skip = solve(i + 1, rem, coins, memo);'],
      ['c_store_memo', '    memo[i][rem] = take + skip;'],
      ['c_ret_memo',   '    return memo[i][rem];'],
      ['',             '}'],
      ['',             ''],
      ['m_scanner',    'const tokens = fs.readFileSync(0, "utf-8").trim().split(/\\s+/);'],
      ['m_read_n',     'if (tokens.length > 1) {'],
      ['',             '    const n = parseInt(tokens[0], 10);'],
      ['m_alloc_arr',  '    const coins = [];'],
      ['m_for_read',   '    for (let i = 0; i < n; i++) {'],
      ['m_read_elem',  '        coins.push(parseInt(tokens[1 + i], 10));'],
      ['',             '    }'],
      ['m_read_amt',   '    const amount = parseInt(tokens[n + 1], 10);'],
      ['m_alloc_memo', '    const memo = Array.from({ length: n }, () => new Array(amount + 1).fill(-1));'],
      ['m_fill_memo',  '    // 2D memo grid initialized'],
      ['m_call_fn',    '    const ans = solve(0, amount, coins, memo);'],
      ['m_print',      '    console.log(ans);'],
      ['m_done',       '    process.exit(0);'],
      ['',             '}']
    ]
  },
  tabulation: {
    java: [
      ['',             'import java.util.Scanner;'],
      ['',             ''],
      ['',             'public class Main {'],
      ['c_entry',      '    static int change(int[] coins, int amount) {'],
      ['c_init_dp',    '        int[] dp = new int[amount + 1];'],
      ['c_base_zero',  '        dp[0] = 1;'],
      ['c_loop_coin',  '        for (int coin : coins) {'],
      ['c_loop_amt',   '            for (int a = coin; a <= amount; a++) {'],
      ['c_update_dp',  '                dp[a] += dp[a - coin];'],
      ['',             '            }'],
      ['',             '        }'],
      ['c_ret_dp',     '        return dp[amount];'],
      ['',             '    }'],
      ['',             ''],
      ['',             '    public static void main(String[] args) {'],
      ['m_scanner',    '        Scanner sc = new Scanner(System.in);'],
      ['m_read_n',     '        int n = sc.nextInt();'],
      ['m_alloc_arr',  '        int[] coins = new int[n];'],
      ['m_for_read',   '        for (int i = 0; i < n; i++) {'],
      ['m_read_elem',  '            coins[i] = sc.nextInt();'],
      ['',             '        }'],
      ['m_read_amt',   '        int amount = sc.nextInt();'],
      ['m_call_fn',    '        int ans = change(coins, amount);'],
      ['m_print',      '        System.out.println(ans);'],
      ['m_done',       '    }'],
      ['',             '}']
    ],
    c: [
      ['',             '#include <stdio.h>'],
      ['',             ''],
      ['c_entry',      'int change(int coins[], int n, int amount) {'],
      ['c_init_dp',    '    int dp[amount + 1];'],
      ['c_fill_zero',  '    for (int i = 0; i <= amount; i++) {'],
      ['',             '        dp[i] = 0;'],
      ['',             '    }'],
      ['c_base_zero',  '    dp[0] = 1;'],
      ['c_loop_coin',  '    for (int i = 0; i < n; i++) {'],
      ['c_loop_amt',   '        for (int a = coins[i]; a <= amount; a++) {'],
      ['c_update_dp',  '            dp[a] += dp[a - coins[i]];'],
      ['',             '        }'],
      ['',             '    }'],
      ['c_ret_dp',     '    return dp[amount];'],
      ['',             '}'],
      ['',             ''],
      ['',             'int main() {'],
      ['m_scanner',    '    int n;'],
      ['m_read_n',     '    scanf("%d", &n);'],
      ['m_alloc_arr',  '    int coins[100];'],
      ['m_for_read',   '    for (int i = 0; i < n; i++) {'],
      ['m_read_elem',  '        scanf("%d", &coins[i]);'],
      ['',             '    }'],
      ['m_read_amt',   '    int amount; scanf("%d", &amount);'],
      ['m_call_fn',    '    int ans = change(coins, n, amount);'],
      ['m_print',      '    printf("%d\\n", ans);'],
      ['m_done',       '    return 0;'],
      ['',             '}']
    ],
    cpp: [
      ['',             '#include <iostream>'],
      ['',             '#include <vector>'],
      ['',             'using namespace std;'],
      ['',             ''],
      ['c_entry',      'int change(const vector<int>& coins, int amount) {'],
      ['c_init_dp',    '    vector<int> dp(amount + 1, 0);'],
      ['c_base_zero',  '    dp[0] = 1;'],
      ['c_loop_coin',  '    for (int coin : coins) {'],
      ['c_loop_amt',   '        for (int a = coin; a <= amount; a++) {'],
      ['c_update_dp',  '            dp[a] += dp[a - coin];'],
      ['',             '        }'],
      ['',             '    }'],
      ['c_ret_dp',     '    return dp[amount];'],
      ['',             '}'],
      ['',             ''],
      ['',             'int main() {'],
      ['m_scanner',    '    int n;'],
      ['m_read_n',     '    cin >> n;'],
      ['m_alloc_arr',  '    vector<int> coins(n);'],
      ['m_for_read',   '    for (int i = 0; i < n; i++) {'],
      ['m_read_elem',  '        cin >> coins[i];'],
      ['',             '    }'],
      ['m_read_amt',   '    int amount; cin >> amount;'],
      ['m_call_fn',    '    int ans = change(coins, amount);'],
      ['m_print',      '    cout << ans << endl;'],
      ['m_done',       '    return 0;'],
      ['',             '}']
    ],
    python: [
      ['',             'import sys'],
      ['',             ''],
      ['c_entry',      'def change(coins, amount):'],
      ['c_init_dp',    '    dp = [0] * (amount + 1)'],
      ['c_base_zero',  '    dp[0] = 1'],
      ['c_loop_coin',  '    for coin in coins:'],
      ['c_loop_amt',   '        for a in range(coin, amount + 1):'],
      ['c_update_dp',  '            dp[a] += dp[a - coin]'],
      ['c_ret_dp',     '    return dp[amount]'],
      ['',             ''],
      ['if __name__ == "__main__":'],
      ['m_scanner',    '    tokens = sys.stdin.read().split()'],
      ['m_read_n',     '    if tokens:'],
      ['',             '        n = int(tokens[0])'],
      ['m_alloc_arr',  '        coins = []'],
      ['m_for_read',   '        for i in range(n):'],
      ['m_read_elem',  '            coins.append(int(tokens[1 + i]))'],
      ['m_read_amt',   '        amount = int(tokens[n+1])'],
      ['m_call_fn',    '        ans = change(coins, amount)'],
      ['m_print',      '        print(ans)'],
      ['m_done',       '        sys.exit(0)']
    ],
    javascript: [
      ['',             'const fs = require("fs");'],
      ['',             ''],
      ['c_entry',      'function change(coins, amount) {'],
      ['c_init_dp',    '    const dp = new Array(amount + 1).fill(0);'],
      ['c_base_zero',  '    dp[0] = 1;'],
      ['c_loop_coin',  '    for (const coin of coins) {'],
      ['c_loop_amt',   '        for (let a = coin; a <= amount; a++) {'],
      ['c_update_dp',  '            dp[a] += dp[a - coin];'],
      ['',             '        }'],
      ['',             '    }'],
      ['c_ret_dp',     '    return dp[amount];'],
      ['',             '}'],
      ['',             ''],
      ['m_scanner',    'const tokens = fs.readFileSync(0, "utf-8").trim().split(/\\s+/);'],
      ['m_read_n',     'if (tokens.length > 1) {'],
      ['',             '    const n = parseInt(tokens[0], 10);'],
      ['m_alloc_arr',  '    const coins = [];'],
      ['m_for_read',   '    for (let i = 0; i < n; i++) {'],
      ['m_read_elem',  '        coins.push(parseInt(tokens[1 + i], 10));'],
      ['',             '    }'],
      ['m_read_amt',   '    const amount = parseInt(tokens[n + 1], 10);'],
      ['m_call_fn',    '    const ans = change(coins, amount);'],
      ['m_print',      '    console.log(ans);'],
      ['m_done',       '    process.exit(0);'],
      ['',             '}']
    ]
  }
};

const PSEUDOCODES = {
  recursion: [
    'function change(i, rem, coins):',
    '  if rem == 0: return 1                      // Found a valid combination',
    '  if rem < 0 or i >= coins.length: return 0  // Invalid branch or out of coins',
    '  take = 0',
    '  if rem - coins[i] >= 0:                    // Option 1: Take coin at index i',
    '    take = change(i, rem - coins[i], coins)',
    '  skip = change(i + 1, rem, coins)           // Option 2: Skip to next denomination',
    '  return take + skip                         // Total unique ways'
  ],
  memoization: [
    'function solve(i, rem, coins, memo):',
    '  if rem == 0: return 1                      // Base: Valid combination',
    '  if rem < 0 or i >= coins.length: return 0  // Base: Invalid / exhausted',
    '  if memo[i][rem] != -1: return memo[i][rem] // Return cached subproblem',
    '  take = 0',
    '  if rem - coins[i] >= 0:',
    '    take = solve(i, rem - coins[i], coins, memo)',
    '  skip = solve(i + 1, rem, coins, memo)',
    '  memo[i][rem] = take + skip',
    '  return memo[i][rem]'
  ],
  tabulation: [
    'function change(coins, amount):',
    '  dp = array of size (amount + 1) filled with 0',
    '  dp[0] = 1                                  // Base: 1 way to form sum 0 (empty set)',
    '  for coin in coins:                         // Outer loop: Fix coin order (avoids permutations)',
    '    for a = coin to amount:                  // Inner loop: Unbounded Knapsack accumulation',
    '      dp[a] += dp[a - coin]',
    '  return dp[amount]'
  ]
};

/* ==================================================================== */
/* STATIC TREE GENERATION (UNBOUNDED KNAPSACK DECISION TREE)             */
/* ==================================================================== */
function generateStaticTree(approach, coins, amount) {
  const nodes = [];
  const edges = [];
  const visitedMemo = new Set();
  let nextId = 0;

  function dfs(i, rem, parentId, branchType, depth) {
    if (depth > 8) return null; // Safe guard
    const myId = nextId++;
    const isBaseSuccess = rem === 0;
    const isBaseFail = rem < 0 || i >= coins.length;
    const key = `${i},${rem}`;
    const isRedundant = approach === 'recursion' && visitedMemo.has(key);
    const isCacheHit = approach === 'memoization' && visitedMemo.has(key);

    const node = {
      id: myId,
      i,
      rem,
      branchType, // 'take' | 'skip' | 'root'
      depth,
      coinVal: i < coins.length ? coins[i] : null,
      isBaseSuccess,
      isBaseFail,
      isRedundant,
      isCacheHit,
      children: [],
      retVal: null
    };
    nodes.push(node);

    if (parentId !== null) {
      edges.push({
        id: `e-${parentId}-${myId}`,
        from: parentId,
        to: myId,
        label: branchType === 'take' ? `+${coins[i]}` : 'skip',
        type: branchType
      });
    }

    if (isBaseSuccess || isBaseFail) {
      return myId;
    }

    if (approach === 'memoization' && isCacheHit) {
      return myId;
    }
    visitedMemo.add(key);

    // Option 1: Take (stay at index i, reduce rem by coins[i])
    if (rem - coins[i] >= 0) {
      const takeId = dfs(i, rem - coins[i], myId, 'take', depth + 1);
      if (takeId !== null) node.children.push(takeId);
    }

    // Option 2: Skip (move to index i + 1, rem stays same)
    if (i + 1 < coins.length) {
      const skipId = dfs(i + 1, rem, myId, 'skip', depth + 1);
      if (skipId !== null) node.children.push(skipId);
    }

    return myId;
  }

  if (coins.length > 0) {
    dfs(0, amount, null, 'root', 0);
  }

  // Layout assignment
  const depthGroups = {};
  nodes.forEach(nd => {
    if (!depthGroups[nd.depth]) depthGroups[nd.depth] = [];
    depthGroups[nd.depth].push(nd);
  });

  const maxDepth = Math.max(0, ...Object.keys(depthGroups).map(Number));
  let maxPerLevel = 1;
  Object.values(depthGroups).forEach(group => {
    if (group.length > maxPerLevel) maxPerLevel = group.length;
  });

  const levelWidth = Math.max(580, maxPerLevel * 72);
  const levelHeight = 64;
  const layoutMap = {};

  Object.entries(depthGroups).forEach(([dStr, group]) => {
    const d = Number(dStr);
    const count = group.length;
    const spacing = levelWidth / (count + 1);
    group.forEach((nd, idx) => {
      layoutMap[nd.id] = {
        x: Math.round(spacing * (idx + 1)),
        y: 40 + d * levelHeight
      };
    });
  });

  return {
    nodes,
    edges,
    layoutMap,
    width: levelWidth,
    height: Math.max(280, (maxDepth + 1) * levelHeight + 60)
  };
}

/* ==================================================================== */
/* BUILD STEPS FOR ANIMATION (STRICT 1-TO-1 LINE-BY-LINE)               */
/* ==================================================================== */
function buildSteps(approach, rawCoins, rawAmount) {
  const steps = [];
  const coins = Array.isArray(rawCoins)
    ? rawCoins.filter(x => typeof x === 'number' && !isNaN(x) && x > 0)
    : [1, 2, 5];
  const amount = Math.max(0, parseInt(rawAmount, 10) || 0);

  /* ------------------------------------------------------------------ */
  /* APPROACH 1: BRUTE FORCE RECURSION                                   */
  /* ------------------------------------------------------------------ */
  if (approach === 'recursion') {
    const staticTree = generateStaticTree('recursion', coins, amount);
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

    const baseState = {
      coins,
      amount,
      curI: 0,
      curRem: amount,
      totalCalls: 0,
      currentReturn: null,
      activeNodeId: null,
      frames: [{ name: 'main', args: `amount=${amount}` }],
      treeNodes: getVisibleNodes(),
      treeEdges: getVisibleEdges(),
      treeWidth: staticTree.width,
      treeHeight: staticTree.height
    };

    // main: line 1 -> scanner
    steps.push({
      code: 'm_scanner',
      badge: 'Scanner sc = new Scanner(System.in);',
      badgeType: 'info',
      state: { ...baseState }
    });

    // main: line 2 -> read n
    steps.push({
      code: 'm_read_n',
      badge: `Read number of coins: n = ${coins.length}`,
      badgeType: 'info',
      state: { ...baseState }
    });

    // main: line 3 -> alloc arr
    steps.push({
      code: 'm_alloc_arr',
      badge: `int[] coins = new int[${coins.length}];`,
      badgeType: 'info',
      state: { ...baseState }
    });

    // main: line 4-5 -> read coins
    for (let i = 0; i < coins.length; i++) {
      steps.push({
        code: 'm_for_read',
        badge: `for (int i = 0; i < n; i++) &rarr; i = ${i}`,
        badgeType: 'info',
        state: { ...baseState, curI: i }
      });
      steps.push({
        code: 'm_read_elem',
        badge: `coins[${i}] = ${coins[i]};`,
        badgeType: 'info',
        state: { ...baseState, curI: i }
      });
    }

    // main: line 6 -> read amount
    steps.push({
      code: 'm_read_amt',
      badge: `int amount = sc.nextInt(); (amount = ${amount})`,
      badgeType: 'info',
      state: { ...baseState }
    });

    // main: line 7 -> call change
    steps.push({
      code: 'm_call_fn',
      badge: `int ans = change(0, amount = ${amount}, coins);`,
      badgeType: 'info',
      state: {
        ...baseState,
        activeNodeId: 0,
        frames: [
          { name: 'main', args: `amount=${amount}` },
          { name: 'change', args: `i=0, rem=${amount}` }
        ]
      }
    });

    function simulateRec(nId, i, rem) {
      const nd = nodeStateMap[nId];
      if (!nd) return 0;
      callCounter++;
      nd.state = 'active';

      const frames = [
        { name: 'main', args: `amount=${amount}` },
        { name: 'change', args: `i=${i}, rem=${rem}` }
      ];

      // c_entry
      steps.push({
        code: 'c_entry',
        badge: `Entering change(i = ${i} [coin=${i < coins.length ? coins[i] : 'none'}], rem = ${rem})`,
        badgeType: 'info',
        state: {
          coins,
          amount,
          curI: i,
          curRem: rem,
          totalCalls: callCounter,
          currentReturn: null,
          activeNodeId: nId,
          frames,
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height
        }
      });

      // c_base_zero
      const isZero = rem === 0;
      steps.push({
        code: 'c_base_zero',
        badge: isZero
          ? `Base check: if (rem == 0) &rarr; TRUE (Combination successfully formed!)`
          : `Base check: if (rem == 0) &rarr; FALSE (${rem} != 0)`,
        badgeType: isZero ? 'success' : 'info',
        state: {
          coins,
          amount,
          curI: i,
          curRem: rem,
          totalCalls: callCounter,
          currentReturn: isZero ? 1 : null,
          activeNodeId: nId,
          frames,
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height
        }
      });

      if (isZero) {
        steps.push({
          code: 'c_ret_zero',
          badge: `return 1; (Found 1 valid combination!)`,
          badgeType: 'success',
          state: {
            coins,
            amount,
            curI: i,
            curRem: rem,
            totalCalls: callCounter,
            currentReturn: 1,
            activeNodeId: nId,
            frames,
            treeNodes: getVisibleNodes(),
            treeEdges: getVisibleEdges(),
            treeWidth: staticTree.width,
            treeHeight: staticTree.height
          }
        });
        nd.state = 'solved';
        nd.retVal = 1;
        return 1;
      }

      // c_base_oob
      const isOob = rem < 0 || i >= coins.length;
      steps.push({
        code: 'c_base_oob',
        badge: isOob
          ? `Base check: if (rem < 0 || i >= coins.length) &rarr; TRUE (Invalid branch)`
          : `Base check: if (rem < 0 || i >= coins.length) &rarr; FALSE (Valid state)`,
        badgeType: isOob ? 'warn' : 'info',
        state: {
          coins,
          amount,
          curI: i,
          curRem: rem,
          totalCalls: callCounter,
          currentReturn: isOob ? 0 : null,
          activeNodeId: nId,
          frames,
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height
        }
      });

      if (isOob) {
        steps.push({
          code: 'c_ret_oob',
          badge: `return 0; (No valid combinations along this branch)`,
          badgeType: 'warn',
          state: {
            coins,
            amount,
            curI: i,
            curRem: rem,
            totalCalls: callCounter,
            currentReturn: 0,
            activeNodeId: nId,
            frames,
            treeNodes: getVisibleNodes(),
            treeEdges: getVisibleEdges(),
            treeWidth: staticTree.width,
            treeHeight: staticTree.height
          }
        });
        nd.state = 'solved';
        nd.retVal = 0;
        return 0;
      }

      // c_init_take
      steps.push({
        code: 'c_init_take',
        badge: `int take = 0; (Initialize take branch count)`,
        badgeType: 'info',
        state: {
          coins,
          amount,
          curI: i,
          curRem: rem,
          totalCalls: callCounter,
          currentReturn: null,
          activeNodeId: nId,
          frames,
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height
        }
      });

      const fits = rem - coins[i] >= 0;
      let takeWays = 0;

      // c_check_take
      steps.push({
        code: 'c_check_take',
        badge: fits
          ? `if (rem - coins[${i}] >= 0) &rarr; TRUE (${rem} - ${coins[i]} = ${rem - coins[i]} >= 0)`
          : `if (rem - coins[${i}] >= 0) &rarr; FALSE (Coin ${coins[i]} exceeds remainder ${rem})`,
        badgeType: fits ? 'info' : 'warn',
        state: {
          coins,
          amount,
          curI: i,
          curRem: rem,
          totalCalls: callCounter,
          currentReturn: null,
          activeNodeId: nId,
          frames,
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height
        }
      });

      if (fits) {
        // c_call_take
        steps.push({
          code: 'c_call_take',
          badge: `take = change(i = ${i}, rem = ${rem - coins[i]}, coins); // Take coin ${coins[i]}`,
          badgeType: 'info',
          state: {
            coins,
            amount,
            curI: i,
            curRem: rem,
            totalCalls: callCounter,
            currentReturn: null,
            activeNodeId: nId,
            frames,
            treeNodes: getVisibleNodes(),
            treeEdges: getVisibleEdges(),
            treeWidth: staticTree.width,
            treeHeight: staticTree.height
          }
        });

        const takeChildId = nd.children.find(cid => nodeStateMap[cid].branchType === 'take');
        if (takeChildId !== undefined) {
          takeWays = simulateRec(takeChildId, i, rem - coins[i]);
        } else {
          takeWays = (rem - coins[i] === 0) ? 1 : 0;
        }
      }

      // c_call_skip
      steps.push({
        code: 'c_call_skip',
        badge: `int skip = change(i = ${i + 1}, rem = ${rem}, coins); // Skip denomination ${coins[i]}`,
        badgeType: 'info',
        state: {
          coins,
          amount,
          curI: i,
          curRem: rem,
          totalCalls: callCounter,
          currentReturn: null,
          activeNodeId: nId,
          frames,
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height
        }
      });

      let skipWays = 0;
      const skipChildId = nd.children.find(cid => nodeStateMap[cid].branchType === 'skip');
      if (skipChildId !== undefined) {
        skipWays = simulateRec(skipChildId, i + 1, rem);
      } else {
        skipWays = (i + 1 >= coins.length) ? 0 : 0;
      }

      const totalWays = takeWays + skipWays;
      nd.state = 'solved';
      nd.retVal = totalWays;

      // c_ret_sum
      steps.push({
        code: 'c_ret_sum',
        badge: `return take (${takeWays}) + skip (${skipWays}) = ${totalWays};`,
        badgeType: 'success',
        state: {
          coins,
          amount,
          curI: i,
          curRem: rem,
          totalCalls: callCounter,
          currentReturn: totalWays,
          activeNodeId: nId,
          frames,
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height
        }
      });

      return totalWays;
    }

    if (staticTree.nodes.length > 0) {
      const finalResult = simulateRec(0, 0, amount);

      // m_print
      steps.push({
        code: 'm_print',
        badge: `System.out.println(${finalResult}); Print total combinations`,
        badgeType: 'success',
        state: {
          coins,
          amount,
          curI: 0,
          curRem: amount,
          totalCalls: callCounter,
          currentReturn: finalResult,
          activeNodeId: 0,
          frames: [{ name: 'main', args: `ans=${finalResult}` }],
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height
        }
      });

      // m_done
      steps.push({
        code: 'm_done',
        badge: `Program execution finished. Number of ways = ${finalResult}`,
        badgeType: 'info',
        state: {
          coins,
          amount,
          curI: 0,
          curRem: amount,
          totalCalls: callCounter,
          currentReturn: finalResult,
          activeNodeId: null,
          frames: [],
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height
        }
      });
    }
  }

  /* ------------------------------------------------------------------ */
  /* APPROACH 2: TOP-DOWN MEMOIZATION                                    */
  /* ------------------------------------------------------------------ */
  else if (approach === 'memoization') {
    const staticTree = generateStaticTree('memoization', coins, amount);
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

    const memoGrid = Array.from({ length: coins.length }, () => new Array(amount + 1).fill(-1));
    let callCounter = 0;
    let cacheHitCounter = 0;

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

    const baseState = {
      coins,
      amount,
      curI: 0,
      curRem: amount,
      totalCalls: 0,
      cacheHits: 0,
      currentReturn: null,
      activeNodeId: null,
      memoGrid: JSON.parse(JSON.stringify(memoGrid)),
      frames: [{ name: 'main', args: `amount=${amount}` }],
      treeNodes: getVisibleNodes(),
      treeEdges: getVisibleEdges(),
      treeWidth: staticTree.width,
      treeHeight: staticTree.height
    };

    // main: line 1 -> scanner
    steps.push({
      code: 'm_scanner',
      badge: 'Scanner sc = new Scanner(System.in);',
      badgeType: 'info',
      state: { ...baseState }
    });

    // main: line 2 -> read n
    steps.push({
      code: 'm_read_n',
      badge: `int n = sc.nextInt(); (n = ${coins.length})`,
      badgeType: 'info',
      state: { ...baseState }
    });

    // main: line 3 -> alloc arr
    steps.push({
      code: 'm_alloc_arr',
      badge: `int[] coins = new int[${coins.length}];`,
      badgeType: 'info',
      state: { ...baseState }
    });

    // main: line 4-5 -> read coins
    for (let i = 0; i < coins.length; i++) {
      steps.push({
        code: 'm_for_read',
        badge: `for (int i = 0; i < n; i++) &rarr; i = ${i}`,
        badgeType: 'info',
        state: { ...baseState, curI: i }
      });
      steps.push({
        code: 'm_read_elem',
        badge: `coins[${i}] = ${coins[i]};`,
        badgeType: 'info',
        state: { ...baseState, curI: i }
      });
    }

    // main: line 6 -> read amount
    steps.push({
      code: 'm_read_amt',
      badge: `int amount = sc.nextInt(); (amount = ${amount})`,
      badgeType: 'info',
      state: { ...baseState }
    });

    // main: line 7 -> alloc memo
    steps.push({
      code: 'm_alloc_memo',
      badge: `int[][] memo = new int[${coins.length}][${amount + 1}];`,
      badgeType: 'info',
      state: { ...baseState }
    });

    // main: line 8 -> fill memo
    steps.push({
      code: 'm_fill_memo',
      badge: 'Initialize 2D cache with -1 (unvisited)',
      badgeType: 'info',
      state: { ...baseState }
    });

    // main: line 9 -> call solve
    steps.push({
      code: 'm_call_fn',
      badge: `int ans = solve(0, amount = ${amount}, coins, memo);`,
      badgeType: 'info',
      state: {
        ...baseState,
        activeNodeId: 0,
        frames: [
          { name: 'main', args: `amount=${amount}` },
          { name: 'solve', args: `i=0, rem=${amount}` }
        ]
      }
    });

    function simulateMemo(nId, i, rem) {
      const nd = nodeStateMap[nId];
      if (!nd) return 0;
      callCounter++;
      nd.state = 'active';

      const frames = [
        { name: 'main', args: `amount=${amount}` },
        { name: 'solve', args: `i=${i}, rem=${rem}` }
      ];

      // c_entry
      steps.push({
        code: 'c_entry',
        badge: `Entering solve(i = ${i}, rem = ${rem})`,
        badgeType: 'info',
        state: {
          coins,
          amount,
          curI: i,
          curRem: rem,
          totalCalls: callCounter,
          cacheHits: cacheHitCounter,
          currentReturn: null,
          activeNodeId: nId,
          memoGrid: JSON.parse(JSON.stringify(memoGrid)),
          frames,
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height
        }
      });

      // c_base_zero
      const isZero = rem === 0;
      steps.push({
        code: 'c_base_zero',
        badge: isZero
          ? `Base check: if (rem == 0) &rarr; TRUE (Combination completed!)`
          : `Base check: if (rem == 0) &rarr; FALSE (${rem} != 0)`,
        badgeType: isZero ? 'success' : 'info',
        state: {
          coins,
          amount,
          curI: i,
          curRem: rem,
          totalCalls: callCounter,
          cacheHits: cacheHitCounter,
          currentReturn: isZero ? 1 : null,
          activeNodeId: nId,
          memoGrid: JSON.parse(JSON.stringify(memoGrid)),
          frames,
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height
        }
      });

      if (isZero) {
        steps.push({
          code: 'c_ret_zero',
          badge: `return 1; (Found 1 valid combination!)`,
          badgeType: 'success',
          state: {
            coins,
            amount,
            curI: i,
            curRem: rem,
            totalCalls: callCounter,
            cacheHits: cacheHitCounter,
            currentReturn: 1,
            activeNodeId: nId,
            memoGrid: JSON.parse(JSON.stringify(memoGrid)),
            frames,
            treeNodes: getVisibleNodes(),
            treeEdges: getVisibleEdges(),
            treeWidth: staticTree.width,
            treeHeight: staticTree.height
          }
        });
        nd.state = 'solved';
        nd.retVal = 1;
        return 1;
      }

      // c_base_oob
      const isOob = rem < 0 || i >= coins.length;
      steps.push({
        code: 'c_base_oob',
        badge: isOob
          ? `Base check: if (rem < 0 || i >= coins.length) &rarr; TRUE (Out of bounds)`
          : `Base check: if (rem < 0 || i >= coins.length) &rarr; FALSE (Valid state)`,
        badgeType: isOob ? 'warn' : 'info',
        state: {
          coins,
          amount,
          curI: i,
          curRem: rem,
          totalCalls: callCounter,
          cacheHits: cacheHitCounter,
          currentReturn: isOob ? 0 : null,
          activeNodeId: nId,
          memoGrid: JSON.parse(JSON.stringify(memoGrid)),
          frames,
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height
        }
      });

      if (isOob) {
        steps.push({
          code: 'c_ret_oob',
          badge: `return 0; (No valid combination)`,
          badgeType: 'warn',
          state: {
            coins,
            amount,
            curI: i,
            curRem: rem,
            totalCalls: callCounter,
            cacheHits: cacheHitCounter,
            currentReturn: 0,
            activeNodeId: nId,
            memoGrid: JSON.parse(JSON.stringify(memoGrid)),
            frames,
            treeNodes: getVisibleNodes(),
            treeEdges: getVisibleEdges(),
            treeWidth: staticTree.width,
            treeHeight: staticTree.height
          }
        });
        nd.state = 'solved';
        nd.retVal = 0;
        return 0;
      }

      // c_memo_chk
      const isCached = memoGrid[i][rem] !== -1;
      steps.push({
        code: 'c_memo_chk',
        badge: isCached
          ? `⚡ Cache check: if (memo[${i}][${rem}] != -1) &rarr; TRUE (Cache hit: ${memoGrid[i][rem]})`
          : `Cache check: if (memo[${i}][${rem}] != -1) &rarr; FALSE (Unvisited subproblem)`,
        badgeType: isCached ? 'success' : 'info',
        state: {
          coins,
          amount,
          curI: i,
          curRem: rem,
          totalCalls: callCounter,
          cacheHits: cacheHitCounter,
          currentReturn: isCached ? memoGrid[i][rem] : null,
          activeNodeId: nId,
          memoGrid: JSON.parse(JSON.stringify(memoGrid)),
          frames,
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height
        }
      });

      if (isCached) {
        cacheHitCounter++;
        nd.state = 'cache-hit';
        nd.retVal = memoGrid[i][rem];

        steps.push({
          code: 'c_ret_cache',
          badge: `return memo[${i}][${rem}]; &rarr; Returning ${memoGrid[i][rem]} in O(1)!`,
          badgeType: 'success',
          state: {
            coins,
            amount,
            curI: i,
            curRem: rem,
            totalCalls: callCounter,
            cacheHits: cacheHitCounter,
            currentReturn: memoGrid[i][rem],
            activeNodeId: nId,
            memoGrid: JSON.parse(JSON.stringify(memoGrid)),
            frames,
            treeNodes: getVisibleNodes(),
            treeEdges: getVisibleEdges(),
            treeWidth: staticTree.width,
            treeHeight: staticTree.height
          }
        });
        return memoGrid[i][rem];
      }

      // c_init_take
      steps.push({
        code: 'c_init_take',
        badge: `int take = 0; (Initialize take option)`,
        badgeType: 'info',
        state: {
          coins,
          amount,
          curI: i,
          curRem: rem,
          totalCalls: callCounter,
          cacheHits: cacheHitCounter,
          currentReturn: null,
          activeNodeId: nId,
          memoGrid: JSON.parse(JSON.stringify(memoGrid)),
          frames,
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height
        }
      });

      const fits = rem - coins[i] >= 0;
      let takeWays = 0;

      // c_check_take
      steps.push({
        code: 'c_check_take',
        badge: fits
          ? `if (rem - coins[${i}] >= 0) &rarr; TRUE (${rem} - ${coins[i]} = ${rem - coins[i]} >= 0)`
          : `if (rem - coins[${i}] >= 0) &rarr; FALSE (Coin ${coins[i]} exceeds remainder ${rem})`,
        badgeType: fits ? 'info' : 'warn',
        state: {
          coins,
          amount,
          curI: i,
          curRem: rem,
          totalCalls: callCounter,
          cacheHits: cacheHitCounter,
          currentReturn: null,
          activeNodeId: nId,
          memoGrid: JSON.parse(JSON.stringify(memoGrid)),
          frames,
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height
        }
      });

      if (fits) {
        // c_call_take
        steps.push({
          code: 'c_call_take',
          badge: `take = solve(i = ${i}, rem = ${rem - coins[i]}, coins, memo); // Take coin ${coins[i]}`,
          badgeType: 'info',
          state: {
            coins,
            amount,
            curI: i,
            curRem: rem,
            totalCalls: callCounter,
            cacheHits: cacheHitCounter,
            currentReturn: null,
            activeNodeId: nId,
            memoGrid: JSON.parse(JSON.stringify(memoGrid)),
            frames,
            treeNodes: getVisibleNodes(),
            treeEdges: getVisibleEdges(),
            treeWidth: staticTree.width,
            treeHeight: staticTree.height
          }
        });

        const takeChildId = nd.children.find(cid => nodeStateMap[cid].branchType === 'take');
        if (takeChildId !== undefined) {
          takeWays = simulateMemo(takeChildId, i, rem - coins[i]);
        } else {
          takeWays = (rem - coins[i] === 0) ? 1 : 0;
        }
      }

      // c_call_skip
      steps.push({
        code: 'c_call_skip',
        badge: `skip = solve(i = ${i + 1}, rem = ${rem}, coins, memo); // Skip coin ${coins[i]}`,
        badgeType: 'info',
        state: {
          coins,
          amount,
          curI: i,
          curRem: rem,
          totalCalls: callCounter,
          cacheHits: cacheHitCounter,
          currentReturn: null,
          activeNodeId: nId,
          memoGrid: JSON.parse(JSON.stringify(memoGrid)),
          frames,
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height
        }
      });

      let skipWays = 0;
      const skipChildId = nd.children.find(cid => nodeStateMap[cid].branchType === 'skip');
      if (skipChildId !== undefined) {
        skipWays = simulateMemo(skipChildId, i + 1, rem);
      } else {
        skipWays = (i + 1 >= coins.length) ? 0 : 0;
      }

      const totalWays = takeWays + skipWays;
      memoGrid[i][rem] = totalWays;
      nd.state = 'solved';
      nd.retVal = totalWays;

      // c_store_memo
      steps.push({
        code: 'c_store_memo',
        badge: `memo[${i}][${rem}] = take (${takeWays}) + skip (${skipWays}) = ${totalWays}; (Cached)`,
        badgeType: 'success',
        state: {
          coins,
          amount,
          curI: i,
          curRem: rem,
          totalCalls: callCounter,
          cacheHits: cacheHitCounter,
          currentReturn: totalWays,
          activeNodeId: nId,
          memoGrid: JSON.parse(JSON.stringify(memoGrid)),
          frames,
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height
        }
      });

      // c_ret_memo
      steps.push({
        code: 'c_ret_memo',
        badge: `return memo[${i}][${rem}]; &rarr; Returning ${totalWays}`,
        badgeType: 'success',
        state: {
          coins,
          amount,
          curI: i,
          curRem: rem,
          totalCalls: callCounter,
          cacheHits: cacheHitCounter,
          currentReturn: totalWays,
          activeNodeId: nId,
          memoGrid: JSON.parse(JSON.stringify(memoGrid)),
          frames,
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height
        }
      });

      return totalWays;
    }

    if (staticTree.nodes.length > 0) {
      const finalResult = simulateMemo(0, 0, amount);

      // m_print
      steps.push({
        code: 'm_print',
        badge: `System.out.println(ans); &rarr; Output: ${finalResult}`,
        badgeType: 'success',
        state: {
          coins,
          amount,
          curI: 0,
          curRem: amount,
          totalCalls: callCounter,
          cacheHits: cacheHitCounter,
          currentReturn: finalResult,
          activeNodeId: 0,
          memoGrid: JSON.parse(JSON.stringify(memoGrid)),
          frames: [{ name: 'main', args: `ans=${finalResult}` }],
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height
        }
      });

      // m_done
      steps.push({
        code: 'm_done',
        badge: `Program finished. Total combinations for amount ${amount} = ${finalResult}`,
        badgeType: 'info',
        state: {
          coins,
          amount,
          curI: 0,
          curRem: amount,
          totalCalls: callCounter,
          cacheHits: cacheHitCounter,
          currentReturn: finalResult,
          activeNodeId: null,
          memoGrid: JSON.parse(JSON.stringify(memoGrid)),
          frames: [],
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height
        }
      });
    }
  }

  /* ------------------------------------------------------------------ */
  /* APPROACH 3: BOTTOM-UP TABULATION (1D DP ARRAY)                     */
  /* ------------------------------------------------------------------ */
  else if (approach === 'tabulation') {
    const dpCells = [];
    for (let i = 0; i <= amount; i++) {
      dpCells.push({
        idx: i,
        val: i === 0 ? 1 : 0,
        status: i === 0 ? 'computed' : 'uncalculated'
      });
    }

    const dp = new Array(amount + 1).fill(0);
    dp[0] = 1;
    let iterations = 0;

    const baseTabState = {
      coins,
      amount,
      a: null,
      c: null,
      refA: null,
      iterations: 0,
      isUpdating: false,
      dpCells: JSON.parse(JSON.stringify(dpCells)),
      currentReturn: null,
      frames: [{ name: 'main', args: `amount=${amount}` }]
    };

    // main: line 1 -> scanner
    steps.push({
      code: 'm_scanner',
      badge: 'Scanner sc = new Scanner(System.in);',
      badgeType: 'info',
      state: { ...baseTabState }
    });

    // main: line 2 -> read n
    steps.push({
      code: 'm_read_n',
      badge: `int n = sc.nextInt(); (n = ${coins.length})`,
      badgeType: 'info',
      state: { ...baseTabState }
    });

    // main: line 3 -> alloc arr
    steps.push({
      code: 'm_alloc_arr',
      badge: `int[] coins = new int[${coins.length}];`,
      badgeType: 'info',
      state: { ...baseTabState }
    });

    // main: line 4-5 -> read coins
    for (let i = 0; i < coins.length; i++) {
      steps.push({
        code: 'm_for_read',
        badge: `for (int i = 0; i < n; i++) &rarr; i = ${i}`,
        badgeType: 'info',
        state: { ...baseTabState, c: coins[i] }
      });
      steps.push({
        code: 'm_read_elem',
        badge: `coins[${i}] = ${coins[i]};`,
        badgeType: 'info',
        state: { ...baseTabState, c: coins[i] }
      });
    }

    // main: line 6 -> read amount
    steps.push({
      code: 'm_read_amt',
      badge: `int amount = sc.nextInt(); (amount = ${amount})`,
      badgeType: 'info',
      state: { ...baseTabState }
    });

    // main: line 7 -> call change
    steps.push({
      code: 'm_call_fn',
      badge: `int ans = change(coins, amount = ${amount});`,
      badgeType: 'info',
      state: {
        ...baseTabState,
        frames: [
          { name: 'main', args: `amount=${amount}` },
          { name: 'change', args: `amount=${amount}` }
        ]
      }
    });

    // change: c_entry
    steps.push({
      code: 'c_entry',
      badge: `Entering change(coins, amount = ${amount})`,
      badgeType: 'info',
      state: {
        ...baseTabState,
        frames: [
          { name: 'main', args: `amount=${amount}` },
          { name: 'change', args: `amount=${amount}` }
        ]
      }
    });

    // c_init_dp
    steps.push({
      code: 'c_init_dp',
      badge: `int[] dp = new int[${amount + 1}]; (allocate DP table of size ${amount + 1})`,
      badgeType: 'info',
      state: {
        ...baseTabState,
        frames: [
          { name: 'main', args: `amount=${amount}` },
          { name: 'change', args: `amount=${amount}` }
        ]
      }
    });

    // c_base_zero
    steps.push({
      code: 'c_base_zero',
      badge: 'dp[0] = 1; (Base case: exactly 1 way to make sum 0 &mdash; using 0 coins)',
      badgeType: 'success',
      state: {
        coins,
        amount,
        a: 0,
        c: null,
        refA: null,
        iterations: 0,
        isUpdating: false,
        dpCells: JSON.parse(JSON.stringify(dpCells)),
        currentReturn: 1,
        frames: [
          { name: 'main', args: `amount=${amount}` },
          { name: 'change', args: `dp[0]=1` }
        ]
      }
    });

    // Outer loop: iterate over denominations (avoids counting permutations)
    for (let ci = 0; ci < coins.length; ci++) {
      const coin = coins[ci];

      // c_loop_coin
      steps.push({
        code: 'c_loop_coin',
        badge: `for (int coin : coins) &rarr; Considering denomination: <b>${coin}</b>`,
        badgeType: 'info',
        state: {
          coins,
          amount,
          a: null,
          c: coin,
          refA: null,
          iterations,
          isUpdating: false,
          dpCells: JSON.parse(JSON.stringify(dpCells)),
          currentReturn: null,
          frames: [
            { name: 'main', args: `amount=${amount}` },
            { name: 'change', args: `coin=${coin}` }
          ]
        }
      });

      // Inner loop: iterate sub-amounts from coin to amount
      for (let a = coin; a <= amount; a++) {
        iterations++;

        // c_loop_amt
        steps.push({
          code: 'c_loop_amt',
          badge: `for (int a = ${coin}; a <= ${amount}; a++) &rarr; Sub-amount a = ${a}`,
          badgeType: 'info',
          state: {
            coins,
            amount,
            a,
            c: coin,
            refA: a - coin,
            iterations,
            isUpdating: false,
            dpCells: JSON.parse(JSON.stringify(dpCells)),
            currentReturn: dp[a],
            frames: [
              { name: 'main', args: `amount=${amount}` },
              { name: 'change', args: `coin=${coin}, a=${a}` }
            ]
          }
        });

        const prevWays = dp[a];
        const addedWays = dp[a - coin];
        dp[a] += addedWays;
        dpCells[a].val = dp[a];
        dpCells[a].status = 'computed';

        // c_update_dp
        steps.push({
          code: 'c_update_dp',
          badge: `dp[${a}] += dp[${a} - ${coin}] &rarr; ${prevWays} + ${addedWays} = <b>${dp[a]}</b> combinations!`,
          badgeType: addedWays > 0 ? 'success' : 'info',
          state: {
            coins,
            amount,
            a,
            c: coin,
            refA: a - coin,
            iterations,
            isUpdating: true,
            dpCells: JSON.parse(JSON.stringify(dpCells)),
            currentReturn: dp[a],
            frames: [
              { name: 'main', args: `amount=${amount}` },
              { name: 'change', args: `dp[${a}]=${dp[a]}` }
            ]
          }
        });
      }
    }

    // c_ret_dp
    steps.push({
      code: 'c_ret_dp',
      badge: `return dp[${amount}]; &rarr; Returning final result: <b>${dp[amount]}</b> ways!`,
      badgeType: 'success',
      state: {
        coins,
        amount,
        a: amount,
        c: null,
        refA: null,
        iterations,
        isUpdating: false,
        dpCells: JSON.parse(JSON.stringify(dpCells)),
        currentReturn: dp[amount],
        frames: [
          { name: 'main', args: `amount=${amount}` },
          { name: 'change', args: `return ${dp[amount]}` }
        ]
      }
    });

    const finalResult = dp[amount];

    // main: m_print
    steps.push({
      code: 'm_print',
      badge: `System.out.println(ans); &rarr; Output: ${finalResult}`,
      badgeType: 'success',
      state: {
        coins,
        amount,
        a: amount,
        c: null,
        refA: null,
        iterations,
        isUpdating: false,
        dpCells: JSON.parse(JSON.stringify(dpCells)),
        currentReturn: finalResult,
        frames: [{ name: 'main', args: `output=${finalResult}` }]
      }
    });

    // main: m_done
    steps.push({
      code: 'm_done',
      badge: `Program execution finished. Total combinations for amount ${amount} = ${finalResult}`,
      badgeType: 'info',
      state: {
        coins,
        amount,
        a: amount,
        c: null,
        refA: null,
        iterations,
        isUpdating: false,
        dpCells: JSON.parse(JSON.stringify(dpCells)),
        currentReturn: finalResult,
        frames: []
      }
    });
  }

  return steps;
}

/* ==================================================================== */
/* COMPONENT STATE & REACTIVE CONTROLS                                  */
/* ==================================================================== */
const currentApproach = ref('tabulation');
const inputCoinsText = ref('1, 2, 5');
const inputAmount = ref(5);

const rightTab = ref('code');
const lang = ref('java');

const si = ref(0);
const playing = ref(false);
const speed = ref(1500);

const showWarningModal = ref(false);
const warningModalTitle = ref('');
const warningModalMsg = ref('');
const warningModalLimit = ref('');
const requestedLength = ref(0);

// Resizers
const leftWidth = ref(52);
const vizHeight = ref(240);
const tableHeight = ref(48);

const mainRef = ref(null);
const leftColRef = ref(null);
const hResizerRef = ref(null);
const vizResizerRef = ref(null);
const tableResizerRef = ref(null);

function parseInputCoins(text) {
  if (!text) return [1, 2, 5];
  return text
    .split(/[\s,]+/)
    .map(x => parseInt(x, 10))
    .filter(x => !isNaN(x) && x > 0);
}

const currentCoins = computed(() => parseInputCoins(inputCoinsText.value));
const currentAmount = computed(() => Math.max(0, parseInt(inputAmount.value, 10) || 0));

const maxAllowedCoins = computed(() => {
  return currentApproach.value === 'tabulation' ? 6 : 4;
});

const maxAllowedAmount = computed(() => {
  return currentApproach.value === 'tabulation' ? 18 : 10;
});

const stepsData = reactive({
  steps: buildSteps('tabulation', [1, 2, 5], 5)
});

const steps = computed(() => stepsData.steps);
const s = computed(() => steps.value[Math.max(0, Math.min(si.value, steps.value.length - 1))] || { state: {} });
const st = computed(() => s.value.state || {});

const codeLines = computed(() => {
  const appCodes = CODES[currentApproach.value] || CODES.tabulation;
  return appCodes[lang.value] || appCodes.java || [];
});
const pseudocodeLines = computed(() => {
  return PSEUDOCODES[currentApproach.value] || PSEUDOCODES.tabulation;
});

let playTimer = null;

function applyApproach(newApproach) {
  if (currentApproach.value === newApproach) return;
  currentApproach.value = newApproach;
  const maxAmt = maxAllowedAmount.value;
  const maxC = maxAllowedCoins.value;

  let coins = parseInputCoins(inputCoinsText.value);
  let amt = parseInt(inputAmount.value, 10);
  if (isNaN(amt) || amt < 0) amt = 5;

  if (amt > maxAmt || coins.length > maxC) {
    requestedLength.value = amt;
    warningModalLimit.value = `Amount ≤ ${maxAmt}, Coins ≤ ${maxC}`;
    const appName = newApproach === 'tabulation' ? 'DP Tabulation' : (newApproach === 'recursion' ? 'Brute Force Recursion' : 'DP Memoization');
    warningModalTitle.value = `${appName} Limits`;
    warningModalMsg.value = `Input exceeded visualization constraints. Amount capped to ${maxAmt}, and coins capped to first ${maxC} denominations.`;
    showWarningModal.value = true;
    amt = Math.min(amt, maxAmt);
    coins = coins.slice(0, maxC);
    inputAmount.value = amt;
    inputCoinsText.value = coins.join(', ');
  }

  playing.value = false;
  stepsData.steps = buildSteps(newApproach, coins, amt);
  si.value = 0;
}

function applyInput() {
  const maxAmt = maxAllowedAmount.value;
  const maxC = maxAllowedCoins.value;

  let coins = parseInputCoins(inputCoinsText.value);
  let amt = parseInt(inputAmount.value, 10);
  if (isNaN(amt) || amt < 0) {
    amt = 5;
    inputAmount.value = amt;
  }

  if (amt > maxAmt || coins.length > maxC) {
    requestedLength.value = amt;
    warningModalLimit.value = `Amount ≤ ${maxAmt}, Coins ≤ ${maxC}`;
    const appName = currentApproach.value === 'tabulation' ? 'DP Tabulation' : (currentApproach.value === 'recursion' ? 'Brute Force Recursion' : 'DP Memoization');
    warningModalTitle.value = `${appName} Limits`;
    warningModalMsg.value = `Input exceeded visualization constraints. Amount capped to ${maxAmt}, and coins capped to first ${maxC} denominations.`;
    showWarningModal.value = true;
    amt = Math.min(amt, maxAmt);
    coins = coins.slice(0, maxC);
    inputAmount.value = amt;
    inputCoinsText.value = coins.join(', ');
  }

  playing.value = false;
  stepsData.steps = buildSteps(currentApproach.value, coins, amt);
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

            <!-- Custom Input Text: amount -->
            <div class="ll-input-group">
              <label>amount =</label>
              <input
                type="number"
                v-model.number="inputAmount"
                class="ll-text-input ll-n-input"
                placeholder="5"
                min="0"
                :max="maxAllowedAmount"
                @keyup.enter="applyInput"
              />
            </div>

            <!-- Custom Input Text: coins -->
            <div class="ll-input-group">
              <label>coins</label>
              <input
                type="text"
                v-model="inputCoinsText"
                class="ll-text-input"
                placeholder="1, 2, 5"
                @keyup.enter="applyInput"
                style="width: 100px;"
              />
              <span class="ll-input-hint">(max {{ maxAllowedCoins }})</span>
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
                  Coin Denominations <code>coins[0...{{ currentCoins.length - 1 }}]</code> &amp; Target <code>amount={{ currentAmount }}</code>:
                </div>
                <div class="ll-house-strip">
                  <div
                    v-for="(val, idx) in currentCoins"
                    :key="idx"
                    class="ll-house-card"
                    :class="{
                      'll-house-cur': (currentApproach !== 'tabulation' && idx === st.curI) || (currentApproach === 'tabulation' && val === st.c),
                      'll-house-compare': currentApproach === 'tabulation' && val === st.c
                    }"
                  >
                    <div class="ll-house-val">{{ val }}</div>
                    <div class="ll-house-idx">
                      <span v-if="currentApproach === 'tabulation' && val === st.c" class="ll-idx-tag ll-tag-comp">coin</span>
                      <span v-else-if="currentApproach !== 'tabulation' && idx === st.curI" class="ll-idx-tag ll-tag-cur">i</span>
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
                    <div class="ll-ptr-chip">Target Amount = <b class="ll-c-blue">{{ currentAmount }}</b></div>
                    <div class="ll-ptr-chip">Coins = <b class="ll-c-blue">[{{ currentCoins.join(', ') }}]</b></div>
                    <div class="ll-ptr-chip" v-if="currentApproach === 'recursion' || currentApproach === 'memoization'">
                      Calls: <b class="ll-c-orange">{{ st.totalCalls || 0 }}</b>
                    </div>
                    <div class="ll-ptr-chip" v-if="currentApproach === 'memoization'">
                      Cache Hits: <b class="ll-c-purple">{{ st.cacheHits || 0 }}</b>
                    </div>
                    <div class="ll-ptr-chip" v-if="currentApproach === 'tabulation'">
                      Iterations: <b class="ll-c-orange">{{ st.iterations !== undefined ? st.iterations : 0 }}</b>
                    </div>
                    <div class="ll-ptr-chip" v-if="st.currentReturn !== null && st.currentReturn !== undefined">
                      Combinations:
                      <b class="ll-c-green">
                        {{ st.currentReturn }}
                      </b>
                    </div>
                  </div>

                  <!-- Diagram 1: DP Tabulation (Bottom-Up 1D DP Array) -->
                  <div v-if="currentApproach === 'tabulation'" class="ll-dp-tab-view">
                    <div class="ll-section-caption">
                      <span>1D DP Array Strip &mdash; <code>dp[a]</code> = Number of combinations to form amount <code>a</code></span>
                      <span v-if="st.isUpdating" class="ll-calc-pill">
                        dp[{{ st.a }}] += dp[{{ st.a }} - {{ st.c }}]
                      </span>
                    </div>

                    <div class="ll-arr-track">
                      <template v-for="cell in st.dpCells" :key="cell.idx">
                        <div class="ll-arr-cell-wrap">
                          <!-- Pointer Tag Above Cell -->
                          <div class="ll-ptr-tag-wrap">
                            <span v-if="cell.idx === st.refA" class="ll-ptr-lbl ll-lbl-orange">&darr; a-coin</span>
                            <span v-else-if="cell.idx === st.a" class="ll-ptr-lbl ll-lbl-green">&darr; a</span>
                          </div>

                          <!-- Flat Box -->
                          <div
                            class="ll-arr-box"
                            :class="{
                              'll-box-cur': cell.idx === st.a,
                              'll-box-comp': cell.idx === st.refA,
                              'll-box-found': cell.val > 0,
                              'll-box-uncalc': cell.val === 0 && cell.idx !== 0
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
                      <span>{{ currentApproach === 'recursion' ? 'Recursive Decision Tree (Take vs Skip)' : 'Pruned Memoization Decision Tree O(n × amount)' }}</span>
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
                          <g v-for="(edge, idx) in st.treeEdges" :key="idx">
                            <line
                              :x1="edge.x1"
                              :y1="edge.y1"
                              :x2="edge.x2"
                              :y2="edge.y2"
                              class="ll-tree-edge"
                              :class="{ 'll-edge-take': edge.type === 'take' }"
                            />
                            <text
                              :x="(edge.x1 + edge.x2) / 2"
                              :y="(edge.y1 + edge.y2) / 2"
                              class="ll-node-text-val"
                              text-anchor="middle"
                            >
                              {{ edge.label }}
                            </text>
                          </g>
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
                            <g v-else-if="node.isRedundant" transform="translate(0, -17)">
                              <rect x="-25" y="-6" width="50" height="12" rx="3" fill="#ef4444" stroke="#dc2626" stroke-width="0.5" />
                              <text x="0" y="0" text-anchor="middle" class="ll-badge-text-purple">Recomputing!</text>
                            </g>

                            <!-- Node Outer Card -->
                            <rect
                              x="-28"
                              y="-12"
                              width="56"
                              height="24"
                              rx="4"
                              class="ll-node-rect"
                              :class="{
                                'll-node-active': node.id === st.activeNodeId,
                                'll-node-solved': node.state === 'solved',
                                'll-node-cachehit': node.state === 'cache-hit'
                              }"
                            />
                            <!-- Node Text: (i, rem) -->
                            <text x="0" y="-3" text-anchor="middle" class="ll-node-text-call">
                              ({{ node.i }}, {{ node.rem }})
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
                                node.retVal !== null && node.retVal !== undefined ? `= ${node.retVal}` :
                                (node.state === 'cache-hit' ? '⚡ Hit' : '?')
                              }}
                            </text>
                          </g>
                        </g>
                      </svg>

                      <!-- 2D Memoization Cache Strip (Only in Memoization Mode) -->
                      <div v-if="currentApproach === 'memoization' && st.memoGrid" class="ll-memo-strip-wrap">
                        <div class="ll-memo-title">Memoization 2D Cache Table &mdash; <code>memo[i][rem]</code>:</div>
                        <div class="ll-memo-grid">
                          <div v-for="(row, rIdx) in st.memoGrid" :key="rIdx" class="ll-memo-row">
                            <span class="ll-memo-row-lbl">i={{ rIdx }}:</span>
                            <div v-for="(cVal, cIdx) in row" :key="cIdx" class="ll-memo-cell-wrap">
                              <div
                                class="ll-memo-cell"
                                :class="{
                                  'll-memo-hit': cVal !== -1,
                                  'll-memo-empty': cVal === -1
                                }"
                              >
                                {{ cVal === -1 ? '?' : cVal }}
                              </div>
                              <span class="ll-memo-idx">{{ cIdx }}</span>
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
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-cur"></span>Current Sub-amount (a)</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-orange"></span>Referenced State (a - coin)</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-new"></span>Ways Accumulated (&gt; 0)</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-uncalc"></span>Zero Ways (0)</span>
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
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-red"></span>Recomputed (Redundant)</span>
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
                  v-html="s.badge"
                >
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
                  >{{ (line[1] !== undefined ? line[1] : line[0]) === '' ? ' ' : (line[1] !== undefined ? line[1] : line[0]) }}</span></pre>
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
                    The <strong>Coin Change II</strong> problem (LeetCode 518) asks for the <em>number of unique combinations</em> of coins that add up to a target amount, with an infinite supply of each coin denomination available.
                  </p>
                  <ul>
                    <li>
                      <strong>1. Combinations vs. Permutations:</strong> If we iterate through the amount in the outer loop and coins in the inner loop, we would count ordered sequences (e.g. <code>[1, 2]</code> and <code>[2, 1]</code> separately). By putting <code>coins</code> in the <strong>outer loop</strong>, we fix the order of denominations, ensuring each unique multiset combination is counted exactly once.
                    </li>
                    <li>
                      <strong>2. Optimal Substructure:</strong> The number of ways to form amount <code>a</code> using the first <code>i</code> coin denominations is the sum of ways without coin <code>i</code> plus ways with at least one coin <code>i</code> (reducing remaining sum to <code>a - coins[i]</code>).
                    </li>
                    <li>
                      <strong>3. Overlapping Subproblems:</strong> In the decision tree, the subproblem state is defined by <code>(i, rem)</code>: the current coin index <code>i</code> and remaining amount <code>rem</code>. Both paths reach identical <code>(i, rem)</code> subproblems, which memoization prunes.
                    </li>
                  </ul>

                  <h3>Approaches Comparison</h3>
                  <table class="ll-complexity-table">
                    <thead>
                      <tr>
                        <th>Approach</th>
                        <th>Time Complexity</th>
                        <th>Space Complexity</th>
                        <th>Performance Characteristic</th>
                      </tr>
                    </thead>
                    <tbody>
                      <tr>
                        <td><strong>Brute Force</strong></td>
                        <td>O(2<sup>N + amount</sup>) Exponential</td>
                        <td>O(N + amount) Call Stack</td>
                        <td>Recursively takes or skips each coin. Suffers from exponential subproblem recomputation.</td>
                      </tr>
                      <tr>
                        <td><strong>DP Memoization</strong></td>
                        <td>O(N &times; amount) Linear-Proportional</td>
                        <td>O(N &times; amount) Cache + Stack</td>
                        <td>Top-down recursion with 2D lookup table caching <code>(i, rem)</code> states in O(1).</td>
                      </tr>
                      <tr>
                        <td><strong>DP Tabulation</strong></td>
                        <td>O(N &times; amount) Systematic</td>
                        <td>O(amount) 1D DP Array</td>
                        <td>Bottom-up array accumulation. Outer loop over coins prevents duplicate permutations and saves space.</td>
                      </tr>
                    </tbody>
                  </table>

                  <h3>Recurrence Formula</h3>
                  <p class="ll-math-box">
                    dp[a] = dp[a] + dp[a - coin] &nbsp;&nbsp;&forall;&nbsp; coin &isin; coins, a &ge; coin
                  </p>
                  <p>
                    Base conditions: <code>dp[0] = 1</code> (exactly 1 way to form amount 0: selecting no coins), and <code>dp[a] = 0</code> initially for all <code>a &gt; 0</code>.
                  </p>

                  <div class="ll-note">
                    <strong>Takeaway:</strong> Outer loop over coins ensures combinations (unordered multisets). Reversing the loop order computes permutations (LeetCode 377: Combination Sum IV).
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

                <div class="ll-modal-body">
                  <div class="ll-modal-badge-row">
                    <span class="ll-modal-tag-entered">Entered: {{ requestedLength }}</span>
                    <span class="ll-modal-arrow">&rarr;</span>
                    <span class="ll-modal-tag-applied">Limit: {{ warningModalLimit }}</span>
                  </div>
                  <p class="ll-modal-message">{{ warningModalMsg }}</p>
                </div>

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
  display: flex; flex-direction: column; overflow: hidden; width: 100%; height: 75vh;
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
.ll-n-input { width: 44px !important; text-align: center; }
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

/* Top Banner */
.ll-house-banner { padding: 4px 10px; background: var(--surface2); border-bottom: 1px solid var(--border); flex-shrink: 0; }
.ll-house-title { font-size: 10.5px; font-weight: 700; color: var(--text2); margin-bottom: 2px; }
.ll-house-strip { display: flex; gap: 6px; flex-wrap: wrap; }
.ll-house-card { width: 44px; height: 38px; display: flex; flex-direction: column; align-items: center; justify-content: center; background: var(--surface); border: 1.5px solid var(--border2); border-radius: var(--radius-sm); box-shadow: var(--shadow-sm); transition: all 0.2s ease; }
.ll-house-cur { border-color: #f59e0b !important; background: #fffbeb !important; transform: translateY(-2px); box-shadow: 0 0 0 3px rgba(245,158,11,0.25) !important; }
.ll-house-compare { border-color: #a855f7 !important; background: #f3e8ff !important; transform: translateY(-2px); }
.ll-house-val { font-size: 13px; font-weight: 800; font-family: monospace; color: var(--text); }
.ll-house-idx { font-size: 9px; color: var(--muted); font-family: monospace; margin-top: 1px; }
.ll-idx-tag { font-weight: 800; padding: 0 3px; border-radius: 3px; }
.ll-tag-cur { color: #b45309; background: #fef3c7; }
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

/* 2D Memoization Table Strip */
.ll-memo-strip-wrap { width: 100%; padding: 4px 8px 6px; border-top: 1px dashed var(--border); background: var(--surface2); flex-shrink: 0; margin-top: 4px; box-sizing: border-box; }
.ll-memo-title { font-size: 10px; font-weight: 700; color: var(--text2); margin-bottom: 3px; }
.ll-memo-grid { display: flex; flex-direction: column; gap: 3px; overflow-x: auto; }
.ll-memo-row { display: flex; align-items: center; gap: 4px; }
.ll-memo-row-lbl { font-size: 9.5px; font-family: monospace; font-weight: 700; color: var(--muted); width: 36px; flex-shrink: 0; }
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
.ll-legdot-orange { background: #fff7ed; border: 1.5px solid #f97316; }
.ll-legdot-red { background: #fee2e2; border: 1.5px solid #ef4444; }
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
.ll-pre { font-family: 'Cascadia Code', 'Fira Code', 'Consolas', monospace; font-size: 11px; line-height: 1.5; white-space: pre; color: var(--text); margin: 0; min-height: 650px; }
.ll-codeline { display: block; padding: 0 14px; margin: 0 -14px; }
.ll-hl { background: #dcfce7; color: #15803d; border-radius: 3px; border-left: 3px solid var(--green); font-weight: 600; }

.ll-info-scroll { flex: 1; overflow: auto; padding: 12px 16px; background: var(--surface); color: var(--text2); font-size: 12px; padding-bottom: 100px; }
.ll-info-scroll h3 { margin: 0 0 6px; color: var(--text); font-size: 13px; font-weight: 700; }
.ll-info-scroll h3:not(:first-child) { margin-top: 14px; }
.ll-info-scroll p { margin: 0 0 6px; }
.ll-info-scroll ul { margin: 0 0 10px; padding-left: 18px; }
.ll-info-scroll li { margin-bottom: 4px; }

.ll-complexity-table { width: 100%; border-collapse: collapse; font-size: 11px; margin: 8px 0 12px; }
.ll-complexity-table th, .ll-complexity-table td { padding: 5px 8px; border: 1px solid var(--border); text-align: left; }
.ll-complexity-table th { background: var(--surface2); color: var(--text); font-weight: 700; }

.ll-math-box { background: var(--surface2); padding: 6px 12px; border-radius: var(--radius-sm); border-left: 3px solid var(--blue); font-family: monospace; font-size: 11.5px; margin: 6px 0; color: var(--text); }
.ll-note { background: #eff6ff; border-left: 3px solid var(--blue); padding: 6px 10px; border-radius: var(--radius-sm); font-size: 11px; margin: 8px 0; color: #1e40af; }

/* Footer */
.ll-footer { padding: 4px 12px; background: var(--surface); border-top: 1px solid var(--border); display: flex; align-items: center; justify-content: space-between; font-size: 11px; color: var(--muted); flex-shrink: 0; }
.ll-speed-wrap { display: flex; align-items: center; gap: 6px; }
.ll-speed-wrap input { width: 80px; accent-color: var(--coral); }

/* Warning Modal */
.ll-modal-backdrop { position: fixed; inset: 0; background: rgba(15, 23, 42, 0.45); backdrop-filter: blur(4px); z-index: 99999; display: flex; align-items: center; justify-content: center; padding: 16px; }
.ll-modal-card { background: #ffffff; border-radius: 12px; border: 1px solid #fee2e2; box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.2), 0 8px 10px -6px rgba(0, 0, 0, 0.1); width: 440px; max-width: 92vw; overflow: hidden; display: flex; flex-direction: column; animation: ll-pop 0.25s cubic-bezier(0.16, 1, 0.3, 1); }
.ll-modal-header { display: flex; align-items: center; justify-content: space-between; padding: 12px 16px; background: #fff5f5; border-bottom: 1px solid #fee2e2; }
.ll-modal-title-wrap { display: flex; align-items: center; gap: 10px; }
.ll-modal-icon-badge { width: 30px; height: 30px; border-radius: 50%; background: #fee2e2; color: #ef4444; display: flex; align-items: center; justify-content: center; flex-shrink: 0; }
.ll-modal-svg-icon { width: 16px; height: 16px; }
.ll-modal-title { font-size: 14px; font-weight: 700; color: #991b1b; }
.ll-modal-close-btn { background: transparent; border: none; font-size: 20px; line-height: 1; color: #94a3b8; cursor: pointer; padding: 2px 6px; border-radius: 4px; transition: all 0.15s ease; }
.ll-modal-close-btn:hover { background: #fee2e2; color: #ef4444; }
.ll-modal-body { padding: 16px 18px; display: flex; flex-direction: column; gap: 10px; }
.ll-modal-badge-row { display: flex; align-items: center; gap: 8px; font-size: 11.5px; font-weight: 600; }
.ll-modal-tag-entered { background: #fee2e2; color: #dc2626; padding: 3px 8px; border-radius: 4px; font-size: 11px; font-weight: 700; font-family: monospace; }
.ll-modal-arrow { color: #94a3b8; font-weight: 700; }
.ll-modal-tag-applied { background: #dcfce7; color: #16a34a; padding: 3px 8px; border-radius: 4px; font-size: 11px; font-weight: 700; font-family: monospace; }
.ll-modal-message { font-size: 12px; line-height: 1.55; color: #334155; margin: 0; }
.ll-modal-footer { padding: 10px 16px 14px; display: flex; justify-content: flex-end; background: #fafafa; border-top: 1px solid #f1f5f9; }
.ll-modal-confirm-btn { background: #ef5050; color: #ffffff; border: none; padding: 7px 18px; border-radius: 6px; font-size: 12px; font-weight: 600; cursor: pointer; transition: filter 0.15s; }
.ll-modal-confirm-btn:hover { filter: brightness(1.08); }

.ll-modal-fade-enter-active, .ll-modal-fade-leave-active { transition: opacity 0.2s ease; }
.ll-modal-fade-enter-from, .ll-modal-fade-leave-to { opacity: 0; }
</style>
