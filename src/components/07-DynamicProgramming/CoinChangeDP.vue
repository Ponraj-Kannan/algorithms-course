<script setup>
import { ref, computed, reactive, onMounted, onUnmounted, watch } from 'vue';

/* ==================================================================== */
/* PROPS & COMPONENT CONFIGURATION                                      */
/* ==================================================================== */
const props = defineProps({
  topic: { type: String, default: 'Dynamic Programming' },
  subTopic: { type: String, default: 'Coin Change (LeetCode 322)' }
});

/* ==================================================================== */
/* APPROACHES & MULTI-LANGUAGE 100% EXECUTABLE CODE SPECIFICATIONS      */
/* ==================================================================== */
const APPROACHES = [
  { id: 'recursion', label: 'Brute Force', desc: 'O(S^n) Exponential — Recursive Branching on Each Coin' },
  { id: 'memoization', label: 'Memoization', desc: 'O(S×n) Time & O(S) Space — Top-Down with 1D Memo Array' },
  { id: 'tabulation', label: 'Tabulation', desc: 'O(S×n) Time & O(S) Space — Bottom-Up 1D DP Array with Traceback' }
];

const CODES = {
  recursion: {
    java: [
      ['',             'import java.util.Scanner;'],
      ['',             ''],
      ['',             'public class Main {'],
      ['',             '    static final int INF = 100000;'],
      ['',             ''],
      ['c_entry',      '    static int minCoins(int[] coins, int rem) {'],
      ['c_base_zero',  '        if (rem == 0) {'],
      ['c_ret_zero',   '            return 0;'],
      ['',             '        }'],
      ['c_base_neg',   '        if (rem < 0) {'],
      ['c_ret_neg',    '            return INF;'],
      ['',             '        }'],
      ['c_init_min',   '        int minCount = INF;'],
      ['c_loop_coins', '        for (int coin : coins) {'],
      ['c_check_coin', '            if (rem - coin >= 0) {'],
      ['c_rec_call',   '                int sub = minCoins(coins, rem - coin);'],
      ['c_check_sub',  '                if (sub != INF) {'],
      ['c_update_min', '                    minCount = Math.min(minCount, 1 + sub);'],
      ['',             '                }'],
      ['',             '            }'],
      ['',             '        }'],
      ['c_ret_min',    '        return minCount;'],
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
      ['m_call_fn',    '        int ans = minCoins(coins, amount);'],
      ['m_print',      '        System.out.println(ans >= INF ? -1 : ans);'],
      ['m_done',       '    }'],
      ['',             '}']
    ],
    c: [
      ['',             '#include <stdio.h>'],
      ['',             '#define INF 100000'],
      ['',             '#define MIN(a, b) ((a) < (b) ? (a) : (b))'],
      ['',             ''],
      ['c_entry',      'int minCoins(int coins[], int n, int rem) {'],
      ['c_base_zero',  '    if (rem == 0) {'],
      ['c_ret_zero',   '        return 0;'],
      ['',             '    }'],
      ['c_base_neg',   '    if (rem < 0) {'],
      ['c_ret_neg',    '        return INF;'],
      ['',             '    }'],
      ['c_init_min',   '    int minCount = INF;'],
      ['c_loop_coins', '    for (int i = 0; i < n; i++) {'],
      ['c_check_coin', '        if (rem - coins[i] >= 0) {'],
      ['c_rec_call',   '            int sub = minCoins(coins, n, rem - coins[i]);'],
      ['c_check_sub',  '            if (sub != INF) {'],
      ['c_update_min', '                minCount = MIN(minCount, 1 + sub);'],
      ['',             '            }'],
      ['',             '        }'],
      ['',             '    }'],
      ['c_ret_min',    '    return minCount;'],
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
      ['m_call_fn',    '    int ans = minCoins(coins, n, amount);'],
      ['m_print',      '    printf("%d\\n", ans >= INF ? -1 : ans);'],
      ['m_done',       '    return 0;'],
      ['',             '}']
    ],
    cpp: [
      ['',             '#include <iostream>'],
      ['',             '#include <vector>'],
      ['',             '#include <algorithm>'],
      ['',             'using namespace std;'],
      ['',             'const int INF = 100000;'],
      ['',             ''],
      ['c_entry',      'int minCoins(const vector<int>& coins, int rem) {'],
      ['c_base_zero',  '    if (rem == 0) {'],
      ['c_ret_zero',   '        return 0;'],
      ['',             '    }'],
      ['c_base_neg',   '    if (rem < 0) {'],
      ['c_ret_neg',    '        return INF;'],
      ['',             '    }'],
      ['c_init_min',   '    int minCount = INF;'],
      ['c_loop_coins', '    for (int coin : coins) {'],
      ['c_check_coin', '        if (rem - coin >= 0) {'],
      ['c_rec_call',   '            int sub = minCoins(coins, rem - coin);'],
      ['c_check_sub',  '            if (sub != INF) {'],
      ['c_update_min', '                minCount = min(minCount, 1 + sub);'],
      ['',             '            }'],
      ['',             '        }'],
      ['',             '    }'],
      ['c_ret_min',    '    return minCount;'],
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
      ['m_call_fn',    '    int ans = minCoins(coins, amount);'],
      ['m_print',      '    cout << (ans >= INF ? -1 : ans) << endl;'],
      ['m_done',       '    return 0;'],
      ['',             '}']
    ],
    python: [
      ['',             'import sys'],
      ['',             'INF = float("inf")'],
      ['',             ''],
      ['c_entry',      'def min_coins(coins, rem):'],
      ['c_base_zero',  '    if rem == 0:'],
      ['c_ret_zero',   '        return 0'],
      ['c_base_neg',   '    if rem < 0:'],
      ['c_ret_neg',    '        return INF'],
      ['c_init_min',   '    min_count = INF'],
      ['c_loop_coins', '    for coin in coins:'],
      ['c_check_coin', '        if rem - coin >= 0:'],
      ['c_rec_call',   '            sub = min_coins(coins, rem - coin)'],
      ['c_check_sub',  '            if sub != INF:'],
      ['c_update_min', '                min_count = min(min_count, 1 + sub)'],
      ['c_ret_min',    '    return min_count'],
      ['',             ''],
      ['',             'if __name__ == "__main__":'],
      ['m_scanner',    '    tokens = sys.stdin.read().split()'],
      ['m_read_n',     '    if tokens:'],
      ['',             '        n = int(tokens[0])'],
      ['m_alloc_arr',  '        coins = [int(x) for x in tokens[1:n+1]]'],
      ['m_read_amt',   '        amount = int(tokens[n+1])'],
      ['m_call_fn',    '        ans = min_coins(coins, amount)'],
      ['m_print',      '        print(-1 if ans == INF else ans)'],
      ['m_done',       '        sys.exit(0)']
    ],
    javascript: [
      ['',             'const fs = require("fs");'],
      ['',             'const INF = 1e9;'],
      ['',             ''],
      ['c_entry',      'function minCoins(coins, rem) {'],
      ['c_base_zero',  '    if (rem === 0) {'],
      ['c_ret_zero',   '        return 0;'],
      ['',             '    }'],
      ['c_base_neg',   '    if (rem < 0) {'],
      ['c_ret_neg',    '        return INF;'],
      ['',             '    }'],
      ['c_init_min',   '    let minCount = INF;'],
      ['c_loop_coins', '    for (const coin of coins) {'],
      ['c_check_coin', '        if (rem - coin >= 0) {'],
      ['c_rec_call',   '            const sub = minCoins(coins, rem - coin);'],
      ['c_check_sub',  '            if (sub !== INF) {'],
      ['c_update_min', '                minCount = Math.min(minCount, 1 + sub);'],
      ['',             '            }'],
      ['',             '        }'],
      ['',             '    }'],
      ['c_ret_min',    '    return minCount;'],
      ['',             '}'],
      ['',             ''],
      ['m_scanner',    'const tokens = fs.readFileSync(0, "utf-8").trim().split(/\\s+/);'],
      ['m_read_n',     'if (tokens.length > 1) {'],
      ['',             '    const n = parseInt(tokens[0], 10);'],
      ['m_alloc_arr',  '    const coins = tokens.slice(1, n + 1).map(Number);'],
      ['m_read_amt',   '    const amount = parseInt(tokens[n + 1], 10);'],
      ['m_call_fn',    '    const ans = minCoins(coins, amount);'],
      ['m_print',      '    console.log(ans >= INF ? -1 : ans);'],
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
      ['',             '    static final int INF = 100000;'],
      ['',             ''],
      ['c_entry',      '    static int solve(int[] coins, int rem, int[] memo) {'],
      ['c_base_zero',  '        if (rem == 0) {'],
      ['c_ret_zero',   '            return 0;'],
      ['',             '        }'],
      ['c_base_neg',   '        if (rem < 0) {'],
      ['c_ret_neg',    '            return INF;'],
      ['',             '        }'],
      ['c_memo_chk',   '        if (memo[rem] != -2) {'],
      ['c_ret_cache',  '            return memo[rem];'],
      ['',             '        }'],
      ['c_init_min',   '        int minCount = INF;'],
      ['c_loop_coins', '        for (int coin : coins) {'],
      ['c_check_coin', '            if (rem - coin >= 0) {'],
      ['c_rec_call',   '                int sub = solve(coins, rem - coin, memo);'],
      ['c_check_sub',  '                if (sub != INF) {'],
      ['c_update_min', '                    minCount = Math.min(minCount, 1 + sub);'],
      ['',             '                }'],
      ['',             '            }'],
      ['',             '        }'],
      ['c_chk_inf',    '        if (minCount >= INF) {'],
      ['c_store_unreach','            memo[rem] = -1;'],
      ['c_else_store', '        } else {'],
      ['c_store_memo', '            memo[rem] = minCount;'],
      ['',             '        }'],
      ['c_ret_memo',   '        return memo[rem];'],
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
      ['m_alloc_memo', '        int[] memo = new int[amount + 1];'],
      ['m_fill_memo',  '        Arrays.fill(memo, -2);'],
      ['m_call_fn',    '        int ans = solve(coins, amount, memo);'],
      ['m_print',      '        System.out.println(ans);'],
      ['m_done',       '    }'],
      ['',             '}']
    ],
    c: [
      ['',             '#include <stdio.h>'],
      ['',             '#include <string.h>'],
      ['',             '#define INF 100000'],
      ['',             '#define MIN(a, b) ((a) < (b) ? (a) : (b))'],
      ['',             ''],
      ['c_entry',      'int solve(int coins[], int n, int rem, int memo[]) {'],
      ['c_base_zero',  '    if (rem == 0) {'],
      ['c_ret_zero',   '        return 0;'],
      ['',             '    }'],
      ['c_base_neg',   '    if (rem < 0) {'],
      ['c_ret_neg',    '        return INF;'],
      ['',             '    }'],
      ['c_memo_chk',   '    if (memo[rem] != -2) {'],
      ['c_ret_cache',  '        return memo[rem];'],
      ['',             '    }'],
      ['c_init_min',   '    int minCount = INF;'],
      ['c_loop_coins', '    for (int i = 0; i < n; i++) {'],
      ['c_check_coin', '        if (rem - coins[i] >= 0) {'],
      ['c_rec_call',   '            int sub = solve(coins, n, rem - coins[i], memo);'],
      ['c_check_sub',  '            if (sub != INF) {'],
      ['c_update_min', '                minCount = MIN(minCount, 1 + sub);'],
      ['',             '            }'],
      ['',             '        }'],
      ['',             '    }'],
      ['c_chk_inf',    '    if (minCount >= INF) {'],
      ['c_store_unreach','        memo[rem] = -1;'],
      ['c_else_store', '    } else {'],
      ['c_store_memo', '        memo[rem] = minCount;'],
      ['',             '    }'],
      ['c_ret_memo',   '    return memo[rem];'],
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
      ['m_alloc_memo', '    int memo[10001];'],
      ['m_fill_memo',  '    for (int i = 0; i <= amount; i++) {'],
      ['',             '        memo[i] = -2;'],
      ['',             '    }'],
      ['m_call_fn',    '    int ans = solve(coins, n, amount, memo);'],
      ['m_print',      '    printf("%d\\n", ans);'],
      ['m_done',       '    return 0;'],
      ['',             '}']
    ],
    cpp: [
      ['',             '#include <iostream>'],
      ['',             '#include <vector>'],
      ['',             '#include <algorithm>'],
      ['',             'using namespace std;'],
      ['',             'const int INF = 100000;'],
      ['',             ''],
      ['c_entry',      'int solve(const vector<int>& coins, int rem, vector<int>& memo) {'],
      ['c_base_zero',  '    if (rem == 0) {'],
      ['c_ret_zero',   '        return 0;'],
      ['',             '    }'],
      ['c_base_neg',   '    if (rem < 0) {'],
      ['c_ret_neg',    '        return INF;'],
      ['',             '    }'],
      ['c_memo_chk',   '    if (memo[rem] != -2) {'],
      ['c_ret_cache',  '        return memo[rem];'],
      ['',             '    }'],
      ['c_init_min',   '    int minCount = INF;'],
      ['c_loop_coins', '    for (int coin : coins) {'],
      ['c_check_coin', '        if (rem - coin >= 0) {'],
      ['c_rec_call',   '            int sub = solve(coins, rem - coin, memo);'],
      ['c_check_sub',  '            if (sub != INF) {'],
      ['c_update_min', '                minCount = min(minCount, 1 + sub);'],
      ['',             '            }'],
      ['',             '        }'],
      ['',             '    }'],
      ['c_chk_inf',    '    if (minCount >= INF) {'],
      ['c_store_unreach','        memo[rem] = -1;'],
      ['c_else_store', '    } else {'],
      ['c_store_memo', '        memo[rem] = minCount;'],
      ['',             '    }'],
      ['c_ret_memo',   '    return memo[rem];'],
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
      ['m_alloc_memo', '    vector<int> memo(amount + 1);'],
      ['m_fill_memo',  '    fill(memo.begin(), memo.end(), -2);'],
      ['m_call_fn',    '    int ans = solve(coins, amount, memo);'],
      ['m_print',      '    cout << ans << endl;'],
      ['m_done',       '    return 0;'],
      ['',             '}']
    ],
    python: [
      ['',             'import sys'],
      ['',             'INF = float("inf")'],
      ['',             ''],
      ['c_entry',      'def solve(coins, rem, memo):'],
      ['c_base_zero',  '    if rem == 0:'],
      ['c_ret_zero',   '        return 0'],
      ['c_base_neg',   '    if rem < 0:'],
      ['c_ret_neg',    '        return INF'],
      ['c_memo_chk',   '    if memo[rem] != -2:'],
      ['c_ret_cache',  '        return memo[rem]'],
      ['c_init_min',   '    min_count = INF'],
      ['c_loop_coins', '    for coin in coins:'],
      ['c_check_coin', '        if rem - coin >= 0:'],
      ['c_rec_call',   '            sub = solve(coins, rem - coin, memo)'],
      ['c_check_sub',  '            if sub != INF:'],
      ['c_update_min', '                min_count = min(min_count, 1 + sub)'],
      ['c_chk_inf',    '    if min_count >= INF:'],
      ['c_store_unreach','        memo[rem] = -1'],
      ['c_else_store', '    else:'],
      ['c_store_memo', '        memo[rem] = min_count'],
      ['c_ret_memo',   '    return memo[rem]'],
      ['',             ''],
      ['',             'if __name__ == "__main__":'],
      ['m_scanner',    '    tokens = sys.stdin.read().split()'],
      ['m_read_n',     '    if tokens:'],
      ['',             '        n = int(tokens[0])'],
      ['m_alloc_arr',  '        coins = [int(x) for x in tokens[1:n+1]]'],
      ['m_read_amt',   '        amount = int(tokens[n+1])'],
      ['m_alloc_memo', '        memo = [-2] * (amount + 1)'],
      ['m_fill_memo',  '        # -2: unvisited, -1: unreachable'],
      ['m_call_fn',    '        ans = solve(coins, amount, memo)'],
      ['m_print',      '        print(ans)'],
      ['m_done',       '        sys.exit(0)']
    ],
    javascript: [
      ['',             'const fs = require("fs");'],
      ['',             'const INF = 1e9;'],
      ['',             ''],
      ['c_entry',      'function solve(coins, rem, memo) {'],
      ['c_base_zero',  '    if (rem === 0) {'],
      ['c_ret_zero',   '        return 0;'],
      ['',             '    }'],
      ['c_base_neg',   '    if (rem < 0) {'],
      ['c_ret_neg',    '        return INF;'],
      ['',             '    }'],
      ['c_memo_chk',   '    if (memo[rem] !== -2) {'],
      ['c_ret_cache',  '        return memo[rem];'],
      ['',             '    }'],
      ['c_init_min',   '    let minCount = INF;'],
      ['c_loop_coins', '    for (const coin of coins) {'],
      ['c_check_coin', '        if (rem - coin >= 0) {'],
      ['c_rec_call',   '            const sub = solve(coins, rem - coin, memo);'],
      ['c_check_sub',  '            if (sub !== INF) {'],
      ['c_update_min', '                minCount = Math.min(minCount, 1 + sub);'],
      ['',             '            }'],
      ['',             '        }'],
      ['',             '    }'],
      ['c_chk_inf',    '    if (minCount >= INF) {'],
      ['c_store_unreach','        memo[rem] = -1;'],
      ['c_else_store', '    } else {'],
      ['c_store_memo', '        memo[rem] = minCount;'],
      ['',             '    }'],
      ['c_ret_memo',   '    return memo[rem];'],
      ['',             '}'],
      ['',             ''],
      ['m_scanner',    'const tokens = fs.readFileSync(0, "utf-8").trim().split(/\\s+/);'],
      ['m_read_n',     'if (tokens.length > 1) {'],
      ['',             '    const n = parseInt(tokens[0], 10);'],
      ['m_alloc_arr',  '    const coins = tokens.slice(1, n + 1).map(Number);'],
      ['m_read_amt',   '    const amount = parseInt(tokens[n + 1], 10);'],
      ['m_alloc_memo', '    const memo = new Array(amount + 1).fill(-2);'],
      ['m_fill_memo',  '    // -2: unvisited, -1: unreachable'],
      ['m_call_fn',    '    const ans = solve(coins, amount, memo);'],
      ['m_print',      '    console.log(ans);'],
      ['m_done',       '    process.exit(0);'],
      ['',             '}']
    ]
  },
  tabulation: {
    java: [
      ['',             'import java.util.Scanner;'],
      ['',             'import java.util.Arrays;'],
      ['',             ''],
      ['',             'public class Main {'],
      ['c_entry',      '    static int coinChange(int[] coins, int amount) {'],
      ['c_init_dp',    '        int[] dp = new int[amount + 1];'],
      ['c_fill_inf',   '        Arrays.fill(dp, amount + 1);'],
      ['c_base_zero',  '        dp[0] = 0;'],
      ['c_loop_amt',   '        for (int a = 1; a <= amount; a++) {'],
      ['c_loop_coin',  '            for (int coin : coins) {'],
      ['c_check_fit',  '                if (a - coin >= 0 && dp[a - coin] != amount + 1) {'],
      ['c_update_dp',  '                    dp[a] = Math.min(dp[a], 1 + dp[a - coin]);'],
      ['',             '                }'],
      ['',             '            }'],
      ['',             '        }'],
      ['c_chk_inf',    '        if (dp[amount] > amount) {'],
      ['c_ret_neg',    '            return -1;'],
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
      ['m_call_fn',    '        int ans = coinChange(coins, amount);'],
      ['m_print',      '        System.out.println(ans);'],
      ['m_done',       '    }'],
      ['',             '}']
    ],
    c: [
      ['',             '#include <stdio.h>'],
      ['',             '#define MIN(a, b) ((a) < (b) ? (a) : (b))'],
      ['',             ''],
      ['c_entry',      'int coinChange(int coins[], int n, int amount) {'],
      ['c_init_dp',    '    int dp[amount + 1];'],
      ['c_fill_inf',   '    for (int i = 0; i <= amount; i++) {'],
      ['',             '        dp[i] = amount + 1;'],
      ['',             '    }'],
      ['c_base_zero',  '    dp[0] = 0;'],
      ['c_loop_amt',   '    for (int a = 1; a <= amount; a++) {'],
      ['c_loop_coin',  '        for (int i = 0; i < n; i++) {'],
      ['c_check_fit',  '            if (a - coins[i] >= 0 && dp[a - coins[i]] != amount + 1) {'],
      ['c_update_dp',  '                dp[a] = MIN(dp[a], 1 + dp[a - coins[i]]);'],
      ['',             '            }'],
      ['',             '        }'],
      ['',             '    }'],
      ['c_chk_inf',    '    if (dp[amount] > amount) {'],
      ['c_ret_neg',    '        return -1;'],
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
      ['m_call_fn',    '    int ans = coinChange(coins, n, amount);'],
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
      ['c_entry',      'int coinChange(const vector<int>& coins, int amount) {'],
      ['c_init_dp',    '    vector<int> dp(amount + 1);'],
      ['c_fill_inf',   '    fill(dp.begin(), dp.end(), amount + 1);'],
      ['c_base_zero',  '    dp[0] = 0;'],
      ['c_loop_amt',   '    for (int a = 1; a <= amount; a++) {'],
      ['c_loop_coin',  '        for (int coin : coins) {'],
      ['c_check_fit',  '            if (a - coin >= 0 && dp[a - coin] != amount + 1) {'],
      ['c_update_dp',  '                dp[a] = min(dp[a], 1 + dp[a - coin]);'],
      ['',             '            }'],
      ['',             '        }'],
      ['',             '    }'],
      ['c_chk_inf',    '    if (dp[amount] > amount) {'],
      ['c_ret_neg',    '        return -1;'],
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
      ['m_call_fn',    '    int ans = coinChange(coins, amount);'],
      ['m_print',      '    cout << ans << endl;'],
      ['m_done',       '    return 0;'],
      ['',             '}']
    ],
    python: [
      ['',             'import sys'],
      ['',             ''],
      ['c_entry',      'def coin_change(coins, amount):'],
      ['c_init_dp',    '    dp = [amount + 1] * (amount + 1)'],
      ['c_fill_inf',   '    # Initialized table with INF (amount + 1)'],
      ['c_base_zero',  '    dp[0] = 0'],
      ['c_loop_amt',   '    for a in range(1, amount + 1):'],
      ['c_loop_coin',  '        for coin in coins:'],
      ['c_check_fit',  '            if a - coin >= 0 and dp[a - coin] != amount + 1:'],
      ['c_update_dp',  '                dp[a] = min(dp[a], 1 + dp[a - coin])'],
      ['c_chk_inf',    '    if dp[amount] > amount:'],
      ['c_ret_neg',    '        return -1'],
      ['c_ret_dp',     '    return dp[amount]'],
      ['',             ''],
      ['',             'if __name__ == "__main__":'],
      ['m_scanner',    '    tokens = sys.stdin.read().split()'],
      ['m_read_n',     '    if tokens:'],
      ['',             '        n = int(tokens[0])'],
      ['m_alloc_arr',  '        coins = [int(x) for x in tokens[1:n+1]]'],
      ['m_read_amt',   '        amount = int(tokens[n+1])'],
      ['m_call_fn',    '        ans = coin_change(coins, amount)'],
      ['m_print',      '        print(ans)'],
      ['m_done',       '        sys.exit(0)']
    ],
    javascript: [
      ['',             'const fs = require("fs");'],
      ['',             ''],
      ['c_entry',      'function coinChange(coins, amount) {'],
      ['c_init_dp',    '    const dp = new Array(amount + 1);'],
      ['c_fill_inf',   '    dp.fill(amount + 1);'],
      ['c_base_zero',  '    dp[0] = 0;'],
      ['c_loop_amt',   '    for (let a = 1; a <= amount; a++) {'],
      ['c_loop_coin',  '        for (const coin of coins) {'],
      ['c_check_fit',  '            if (a - coin >= 0 && dp[a - coin] !== amount + 1) {'],
      ['c_update_dp',  '                dp[a] = Math.min(dp[a], 1 + dp[a - coin]);'],
      ['',             '            }'],
      ['',             '        }'],
      ['',             '    }'],
      ['c_chk_inf',    '    if (dp[amount] > amount) {'],
      ['c_ret_neg',    '        return -1;'],
      ['',             '    }'],
      ['c_ret_dp',     '    return dp[amount];'],
      ['',             '}'],
      ['',             ''],
      ['m_scanner',    'const tokens = fs.readFileSync(0, "utf-8").trim().split(/\\s+/);'],
      ['m_read_n',     'if (tokens.length > 1) {'],
      ['',             '    const n = parseInt(tokens[0], 10);'],
      ['m_alloc_arr',  '    const coins = tokens.slice(1, n + 1).map(Number);'],
      ['m_read_amt',   '    const amount = parseInt(tokens[n + 1], 10);'],
      ['m_call_fn',    '    const ans = coinChange(coins, amount);'],
      ['m_print',      '    console.log(ans);'],
      ['m_done',       '    process.exit(0);'],
      ['',             '}']
    ]
  }
};

const PSEUDOCODES = {
  recursion: [
    'function minCoins(coins, rem):',
    '  if rem == 0: return 0                     // Base: 0 coins needed for sum 0',
    '  if rem < 0: return infinity               // Base: Exceeded amount (invalid)',
    '  minCount = infinity',
    '  for coin in coins:',
    '    if rem - coin >= 0:                     // Prune branches exceeding rem',
    '      sub = minCoins(coins, rem - coin)     // Recurse on subproblem',
    '      if sub != infinity:',
    '        minCount = min(minCount, 1 + sub)',
    '  return minCount == infinity ? -1 : minCount',
    '',
    'Time Complexity:  O(S^n) Exponential Branching (S = amount, n = coins)',
    'Space Complexity: O(S) Call Stack Depth'
  ],
  memoization: [
    'memo = 1D array of size (amount + 1), initialized with -2 (unvisited)',
    '',
    'function solve(coins, rem, memo):',
    '  if rem == 0: return 0',
    '  if rem < 0: return infinity',
    '  if memo[rem] != -2: return memo[rem]      // O(1) Cache Hit',
    '  minCount = infinity',
    '  for coin in coins:',
    '    if rem - coin >= 0:',
    '      sub = solve(coins, rem - coin, memo)',
    '      if sub != infinity:',
    '        minCount = min(minCount, 1 + sub)',
    '  memo[rem] = (minCount == infinity) ? -1 : minCount',
    '  return memo[rem]',
    '',
    'Time Complexity:  O(S * n) Linear with Respect to Subproblems',
    'Space Complexity: O(S) Memoization Table + O(S) Call Stack'
  ],
  tabulation: [
    'function coinChange(coins, amount):',
    '  dp = array of size (amount + 1) filled with (amount + 1)',
    '  dp[0] = 0                                 // Base: 0 coins for amount 0',
    '  for a = 1 to amount:                      // Solve for every sub-amount',
    '    for coin in coins:                      // Test each coin denomination',
    '      if a - coin >= 0 and dp[a - coin] != infinity:',
    '        dp[a] = min(dp[a], 1 + dp[a - coin]) // Choose minimum coin path',
    '  return dp[amount] > amount ? -1 : dp[amount]',
    '',
    'Time Complexity:  O(S * n) Iterative Systematic Nested Loops',
    'Space Complexity: O(S) 1D Table'
  ]
};

/* ==================================================================== */
/* STATIC TREE GENERATOR FOR DECISION TREE (RECURSION / MEMOIZATION)    */
/* ==================================================================== */
function generateStaticTree(approach, coins, amount) {
  const nodes = [];
  const edges = [];
  let nextId = 0;
  const sortedCoins = [...coins].filter(c => c > 0).sort((a, b) => b - a);

  if (approach === 'recursion') {
    const visitedSet = new Set();
    function build(rem, parentId, coinUsed) {
      const id = nextId++;
      const isRedundant = visitedSet.has(rem) && rem > 0;
      visitedSet.add(rem);

      const node = { id, rem, parentId, coinUsed, isRedundant, children: [] };
      nodes.push(node);

      if (parentId !== null) {
        edges.push({ from: parentId, to: id, coinUsed });
      }

      if (rem > 0 && !isRedundant && nodes.length < 55) {
        const childIds = [];
        for (const c of sortedCoins) {
          if (rem - c >= 0) {
            const cId = build(rem - c, id, c);
            childIds.push(cId);
          }
        }
        node.children = childIds;
      }
      return id;
    }
    if (amount >= 0) build(amount, null, null);
  } else if (approach === 'memoization') {
    const memo = new Array(amount + 1).fill(false);
    function buildMemo(rem, parentId, coinUsed) {
      const id = nextId++;
      const isCacheHit = rem >= 0 && rem <= amount && memo[rem];
      const node = { id, rem, parentId, coinUsed, isCacheHit, children: [] };
      nodes.push(node);

      if (parentId !== null) {
        edges.push({ from: parentId, to: id, coinUsed });
      }

      if (rem > 0 && !isCacheHit && nodes.length < 55) {
        memo[rem] = true;
        const childIds = [];
        for (const c of sortedCoins) {
          if (rem - c >= 0) {
            const cId = buildMemo(rem - c, id, c);
            childIds.push(cId);
          }
        }
        node.children = childIds;
      }
      return id;
    }
    if (amount >= 0) buildMemo(amount, null, null);
  }

  // Layout assignment
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

  const leafSpacing = 68;
  const levelHeight = 50;

  const computedWidth = Math.max(220, totalLeaves * leafSpacing + 40);
  const computedHeight = Math.max(80, maxDepth * levelHeight + 50);

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

  return { nodes, edges, layoutMap, width: computedWidth, height: computedHeight };
}

/* ==================================================================== */
/* STRICT ZERO-SKIP LINE-BY-LINE EXECUTION STEP GENERATOR               */
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
    let redundantCounter = 0;

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
      curRem: amount,
      curCoin: null,
      totalCalls: 0,
      redundantCalls: 0,
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
      badge: 'Initialize scanner: Scanner sc = new Scanner(System.in);',
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
      badge: `Allocate array: int[] coins = new int[${coins.length}];`,
      badgeType: 'info',
      state: { ...baseState }
    });

    // main: line 4-5 -> read coins in loop
    for (let i = 0; i < coins.length; i++) {
      steps.push({
        code: 'm_for_read',
        badge: `For loop iteration: i = ${i} < ${coins.length}`,
        badgeType: 'info',
        state: { ...baseState, curCoin: coins[i] }
      });
      steps.push({
        code: 'm_read_elem',
        badge: `Read coin[${i}] = ${coins[i]}`,
        badgeType: 'info',
        state: { ...baseState, curCoin: coins[i] }
      });
    }

    // main: line 6 -> read amount
    steps.push({
      code: 'm_read_amt',
      badge: `Read target amount: amount = ${amount}`,
      badgeType: 'info',
      state: { ...baseState }
    });

    // main: line 7 -> call minCoins
    steps.push({
      code: 'm_call_fn',
      badge: `Invoke minCoins(coins, amount = ${amount})`,
      badgeType: 'info',
      state: {
        ...baseState,
        activeNodeId: 0,
        frames: [
          { name: 'main', args: `amount=${amount}` },
          { name: 'minCoins', args: `rem=${amount}` }
        ]
      }
    });

    function simulateRec(nId, rem) {
      const nd = nodeStateMap[nId];
      if (!nd) return -1;
      callCounter++;
      nd.state = 'active';

      if (nd.isRedundant) redundantCounter++;

      const frames = [
        { name: 'main', args: `amount=${amount}` },
        { name: 'minCoins', args: `rem=${rem}` }
      ];

      // c_entry
      steps.push({
        code: 'c_entry',
        badge: nd.isRedundant
          ? `⚠️ Overlapping subproblem: minCoins(rem = ${rem}) is being recomputed!`
          : `Entering minCoins(rem = ${rem})`,
        badgeType: nd.isRedundant ? 'warn' : 'info',
        state: {
          coins,
          amount,
          curRem: rem,
          curCoin: nd.coinUsed,
          totalCalls: callCounter,
          redundantCalls: redundantCounter,
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
      steps.push({
        code: 'c_base_zero',
        badge: rem === 0
          ? `Base check: if (rem == 0) &rarr; TRUE (exact amount formed!)`
          : `Base check: if (rem == 0) &rarr; FALSE (${rem} != 0)`,
        badgeType: rem === 0 ? 'success' : 'info',
        state: {
          coins,
          amount,
          curRem: rem,
          curCoin: nd.coinUsed,
          totalCalls: callCounter,
          redundantCalls: redundantCounter,
          currentReturn: rem === 0 ? 0 : null,
          activeNodeId: nId,
          frames,
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height
        }
      });

      if (rem === 0) {
        steps.push({
          code: 'c_ret_zero',
          badge: `return 0; (0 coins needed for remainder 0)`,
          badgeType: 'success',
          state: {
            coins,
            amount,
            curRem: 0,
            curCoin: nd.coinUsed,
            totalCalls: callCounter,
            redundantCalls: redundantCounter,
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

      // c_base_neg
      steps.push({
        code: 'c_base_neg',
        badge: rem < 0
          ? `Base check: if (rem < 0) &rarr; TRUE (negative remainder)`
          : `Base check: if (rem < 0) &rarr; FALSE (${rem} >= 0)`,
        badgeType: rem < 0 ? 'warn' : 'info',
        state: {
          coins,
          amount,
          curRem: rem,
          curCoin: nd.coinUsed,
          totalCalls: callCounter,
          redundantCalls: redundantCounter,
          currentReturn: null,
          activeNodeId: nId,
          frames,
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height
        }
      });

      if (rem < 0) {
        steps.push({
          code: 'c_ret_neg',
          badge: `return INF; (invalid branch exceeding target)`,
          badgeType: 'warn',
          state: {
            coins,
            amount,
            curRem: rem,
            curCoin: nd.coinUsed,
            totalCalls: callCounter,
            redundantCalls: redundantCounter,
            currentReturn: -1,
            activeNodeId: nId,
            frames,
            treeNodes: getVisibleNodes(),
            treeEdges: getVisibleEdges(),
            treeWidth: staticTree.width,
            treeHeight: staticTree.height
          }
        });
        nd.state = 'solved';
        nd.retVal = -1;
        return -1;
      }

      // c_init_min
      steps.push({
        code: 'c_init_min',
        badge: `Initialize candidate: int minCount = INF;`,
        badgeType: 'info',
        state: {
          coins,
          amount,
          curRem: rem,
          curCoin: nd.coinUsed,
          totalCalls: callCounter,
          redundantCalls: redundantCounter,
          currentReturn: null,
          activeNodeId: nId,
          frames,
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height
        }
      });

      let minCount = Infinity;
      const sorted = [...coins].sort((a, b) => b - a);

      for (let ci = 0; ci < sorted.length; ci++) {
        const c = sorted[ci];

        // c_loop_coins
        steps.push({
          code: 'c_loop_coins',
          badge: `Loop coin choice: coin = ${c}`,
          badgeType: 'info',
          state: {
            coins,
            amount,
            curRem: rem,
            curCoin: c,
            totalCalls: callCounter,
            redundantCalls: redundantCounter,
            currentReturn: minCount === Infinity ? null : minCount,
            activeNodeId: nId,
            frames,
            treeNodes: getVisibleNodes(),
            treeEdges: getVisibleEdges(),
            treeWidth: staticTree.width,
            treeHeight: staticTree.height
          }
        });

        // c_check_coin
        const fits = rem - c >= 0;
        steps.push({
          code: 'c_check_coin',
          badge: fits
            ? `if (rem - coin >= 0) &rarr; TRUE (${rem} - ${c} = ${rem - c} >= 0)`
            : `if (rem - coin >= 0) &rarr; FALSE (${rem} - ${c} < 0, coin too large)`,
          badgeType: fits ? 'info' : 'warn',
          state: {
            coins,
            amount,
            curRem: rem,
            curCoin: c,
            totalCalls: callCounter,
            redundantCalls: redundantCounter,
            currentReturn: minCount === Infinity ? null : minCount,
            activeNodeId: nId,
            frames,
            treeNodes: getVisibleNodes(),
            treeEdges: getVisibleEdges(),
            treeWidth: staticTree.width,
            treeHeight: staticTree.height
          }
        });

        if (fits) {
          // c_rec_call
          steps.push({
            code: 'c_rec_call',
            badge: `int sub = minCoins(coins, ${rem} - ${c} = ${rem - c});`,
            badgeType: 'info',
            state: {
              coins,
              amount,
              curRem: rem,
              curCoin: c,
              totalCalls: callCounter,
              redundantCalls: redundantCounter,
              currentReturn: minCount === Infinity ? null : minCount,
              activeNodeId: nId,
              frames,
              treeNodes: getVisibleNodes(),
              treeEdges: getVisibleEdges(),
              treeWidth: staticTree.width,
              treeHeight: staticTree.height
            }
          });

          const childId = nd.children.find(cid => nodeStateMap[cid].coinUsed === c);
          let sub = Infinity;
          if (childId !== undefined) {
            sub = simulateRec(childId, rem - c);
          } else {
            sub = rem - c === 0 ? 0 : 1;
          }

          // c_check_sub
          const validSub = sub !== Infinity && sub !== -1;
          steps.push({
            code: 'c_check_sub',
            badge: validSub
              ? `if (sub != INF) &rarr; TRUE (sub = ${sub})`
              : `if (sub != INF) &rarr; FALSE (sub is unreachable / INF)`,
            badgeType: validSub ? 'info' : 'warn',
            state: {
              coins,
              amount,
              curRem: rem,
              curCoin: c,
              totalCalls: callCounter,
              redundantCalls: redundantCounter,
              currentReturn: minCount === Infinity ? null : minCount,
              activeNodeId: nId,
              frames,
              treeNodes: getVisibleNodes(),
              treeEdges: getVisibleEdges(),
              treeWidth: staticTree.width,
              treeHeight: staticTree.height
            }
          });

          if (validSub) {
            const candidate = 1 + sub;
            const isBetter = candidate < minCount;
            minCount = Math.min(minCount, candidate);

            // c_update_min
            steps.push({
              code: 'c_update_min',
              badge: isBetter
                ? `minCount = Math.min(minCount, 1 + ${sub}) = ${minCount} (new best!)`
                : `minCount = Math.min(minCount, 1 + ${sub}) = ${minCount} (retaining existing best)`,
              badgeType: isBetter ? 'success' : 'info',
              state: {
                coins,
                amount,
                curRem: rem,
                curCoin: c,
                totalCalls: callCounter,
                redundantCalls: redundantCounter,
                currentReturn: minCount,
                activeNodeId: nId,
                frames,
                treeNodes: getVisibleNodes(),
                treeEdges: getVisibleEdges(),
                treeWidth: staticTree.width,
                treeHeight: staticTree.height
              }
            });
          }
        }
      }

      const finalVal = minCount === Infinity ? -1 : minCount;
      nd.state = 'solved';
      nd.retVal = finalVal;

      // c_ret_min
      steps.push({
        code: 'c_ret_min',
        badge: `return minCount; (returning ${finalVal === -1 ? 'INF (-1)' : finalVal + ' coins'})`,
        badgeType: finalVal === -1 ? 'warn' : 'success',
        state: {
          coins,
          amount,
          curRem: rem,
          curCoin: nd.coinUsed,
          totalCalls: callCounter,
          redundantCalls: redundantCounter,
          currentReturn: finalVal,
          activeNodeId: nId,
          frames,
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height
        }
      });

      return finalVal;
    }

    if (staticTree.nodes.length > 0) {
      const finalResult = simulateRec(0, amount);

      // m_print
      steps.push({
        code: 'm_print',
        badge: `System.out.println(${finalResult}); Print final result`,
        badgeType: finalResult === -1 ? 'warn' : 'success',
        state: {
          coins,
          amount,
          curRem: amount,
          curCoin: null,
          totalCalls: callCounter,
          redundantCalls: redundantCounter,
          currentReturn: finalResult,
          activeNodeId: 0,
          frames: [{ name: 'main', args: `result=${finalResult}` }],
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height
        }
      });

      // m_done
      steps.push({
        code: 'm_done',
        badge: `Program execution finished. Fewest coins = ${finalResult}`,
        badgeType: 'info',
        state: {
          coins,
          amount,
          curRem: amount,
          curCoin: null,
          totalCalls: callCounter,
          redundantCalls: redundantCounter,
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

    const memoArray = new Array(amount + 1).fill(-2);
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
      curRem: amount,
      curCoin: null,
      totalCalls: 0,
      cacheHits: 0,
      currentReturn: null,
      activeNodeId: null,
      memo: [...memoArray],
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

    // main: line 4-5 -> loop read
    for (let i = 0; i < coins.length; i++) {
      steps.push({
        code: 'm_for_read',
        badge: `for (int i = 0; i < n; i++) &rarr; i = ${i}`,
        badgeType: 'info',
        state: { ...baseState, curCoin: coins[i] }
      });
      steps.push({
        code: 'm_read_elem',
        badge: `coins[${i}] = ${coins[i]};`,
        badgeType: 'info',
        state: { ...baseState, curCoin: coins[i] }
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
      badge: `int[] memo = new int[${amount + 1}];`,
      badgeType: 'info',
      state: { ...baseState }
    });

    // main: line 8 -> fill memo
    steps.push({
      code: 'm_fill_memo',
      badge: 'Arrays.fill(memo, -2); (initialize unvisited cache)',
      badgeType: 'info',
      state: { ...baseState }
    });

    // main: line 9 -> call solve
    steps.push({
      code: 'm_call_fn',
      badge: `int ans = solve(coins, amount = ${amount}, memo);`,
      badgeType: 'info',
      state: {
        ...baseState,
        activeNodeId: 0,
        frames: [
          { name: 'main', args: `amount=${amount}` },
          { name: 'solve', args: `rem=${amount}` }
        ]
      }
    });

    function simulateMemo(nId, rem) {
      const nd = nodeStateMap[nId];
      if (!nd) return -1;
      callCounter++;
      nd.state = 'active';

      const frames = [
        { name: 'main', args: `amount=${amount}` },
        { name: 'solve', args: `rem=${rem}` }
      ];

      // c_entry
      steps.push({
        code: 'c_entry',
        badge: `Entering solve(rem = ${rem})`,
        badgeType: 'info',
        state: {
          coins,
          amount,
          curRem: rem,
          curCoin: nd.coinUsed,
          totalCalls: callCounter,
          cacheHits: cacheHitCounter,
          currentReturn: null,
          activeNodeId: nId,
          memo: [...memoArray],
          frames,
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height
        }
      });

      // c_base_zero
      steps.push({
        code: 'c_base_zero',
        badge: rem === 0
          ? `Base check: if (rem == 0) &rarr; TRUE (exact amount reached!)`
          : `Base check: if (rem == 0) &rarr; FALSE (${rem} != 0)`,
        badgeType: rem === 0 ? 'success' : 'info',
        state: {
          coins,
          amount,
          curRem: rem,
          curCoin: nd.coinUsed,
          totalCalls: callCounter,
          cacheHits: cacheHitCounter,
          currentReturn: rem === 0 ? 0 : null,
          activeNodeId: nId,
          memo: [...memoArray],
          frames,
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height
        }
      });

      if (rem === 0) {
        steps.push({
          code: 'c_ret_zero',
          badge: `return 0; (0 coins needed for remainder 0)`,
          badgeType: 'success',
          state: {
            coins,
            amount,
            curRem: 0,
            curCoin: nd.coinUsed,
            totalCalls: callCounter,
            cacheHits: cacheHitCounter,
            currentReturn: 0,
            activeNodeId: nId,
            memo: [...memoArray],
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

      // c_base_neg
      steps.push({
        code: 'c_base_neg',
        badge: rem < 0
          ? `Base check: if (rem < 0) &rarr; TRUE (negative remainder)`
          : `Base check: if (rem < 0) &rarr; FALSE (${rem} >= 0)`,
        badgeType: rem < 0 ? 'warn' : 'info',
        state: {
          coins,
          amount,
          curRem: rem,
          curCoin: nd.coinUsed,
          totalCalls: callCounter,
          cacheHits: cacheHitCounter,
          currentReturn: null,
          activeNodeId: nId,
          memo: [...memoArray],
          frames,
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height
        }
      });

      if (rem < 0) {
        steps.push({
          code: 'c_ret_neg',
          badge: `return INF; (invalid branch exceeding target)`,
          badgeType: 'warn',
          state: {
            coins,
            amount,
            curRem: rem,
            curCoin: nd.coinUsed,
            totalCalls: callCounter,
            cacheHits: cacheHitCounter,
            currentReturn: -1,
            activeNodeId: nId,
            memo: [...memoArray],
            frames,
            treeNodes: getVisibleNodes(),
            treeEdges: getVisibleEdges(),
            treeWidth: staticTree.width,
            treeHeight: staticTree.height
          }
        });
        nd.state = 'solved';
        nd.retVal = -1;
        return -1;
      }

      // c_memo_chk
      const isCached = memoArray[rem] !== -2;
      steps.push({
        code: 'c_memo_chk',
        badge: isCached
          ? `⚡ Cache check: if (memo[${rem}] != -2) &rarr; TRUE (hit: ${memoArray[rem]})`
          : `Cache check: if (memo[${rem}] != -2) &rarr; FALSE (unvisited)`,
        badgeType: isCached ? 'success' : 'info',
        state: {
          coins,
          amount,
          curRem: rem,
          curCoin: nd.coinUsed,
          totalCalls: callCounter,
          cacheHits: cacheHitCounter,
          currentReturn: isCached ? memoArray[rem] : null,
          activeNodeId: nId,
          memo: [...memoArray],
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
        nd.retVal = memoArray[rem];

        steps.push({
          code: 'c_ret_cache',
          badge: `return memo[${rem}]; &rarr; Return ${memoArray[rem]} in O(1)!`,
          badgeType: 'success',
          state: {
            coins,
            amount,
            curRem: rem,
            curCoin: nd.coinUsed,
            totalCalls: callCounter,
            cacheHits: cacheHitCounter,
            currentReturn: memoArray[rem],
            activeNodeId: nId,
            memo: [...memoArray],
            frames,
            treeNodes: getVisibleNodes(),
            treeEdges: getVisibleEdges(),
            treeWidth: staticTree.width,
            treeHeight: staticTree.height
          }
        });
        return memoArray[rem];
      }

      // c_init_min
      steps.push({
        code: 'c_init_min',
        badge: `int minCount = INF; (testing coins for rem = ${rem})`,
        badgeType: 'info',
        state: {
          coins,
          amount,
          curRem: rem,
          curCoin: nd.coinUsed,
          totalCalls: callCounter,
          cacheHits: cacheHitCounter,
          currentReturn: null,
          activeNodeId: nId,
          memo: [...memoArray],
          frames,
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height
        }
      });

      let minCount = Infinity;
      const sorted = [...coins].sort((a, b) => b - a);

      for (let ci = 0; ci < sorted.length; ci++) {
        const c = sorted[ci];

        // c_loop_coins
        steps.push({
          code: 'c_loop_coins',
          badge: `for (int coin : coins) &rarr; coin = ${c}`,
          badgeType: 'info',
          state: {
            coins,
            amount,
            curRem: rem,
            curCoin: c,
            totalCalls: callCounter,
            cacheHits: cacheHitCounter,
            currentReturn: minCount === Infinity ? null : minCount,
            activeNodeId: nId,
            memo: [...memoArray],
            frames,
            treeNodes: getVisibleNodes(),
            treeEdges: getVisibleEdges(),
            treeWidth: staticTree.width,
            treeHeight: staticTree.height
          }
        });

        // c_check_coin
        const fits = rem - c >= 0;
        steps.push({
          code: 'c_check_coin',
          badge: fits
            ? `if (rem - coin >= 0) &rarr; TRUE (${rem} - ${c} = ${rem - c} >= 0)`
            : `if (rem - coin >= 0) &rarr; FALSE (${rem} - ${c} < 0, coin exceeds rem)`,
          badgeType: fits ? 'info' : 'warn',
          state: {
            coins,
            amount,
            curRem: rem,
            curCoin: c,
            totalCalls: callCounter,
            cacheHits: cacheHitCounter,
            currentReturn: minCount === Infinity ? null : minCount,
            activeNodeId: nId,
            memo: [...memoArray],
            frames,
            treeNodes: getVisibleNodes(),
            treeEdges: getVisibleEdges(),
            treeWidth: staticTree.width,
            treeHeight: staticTree.height
          }
        });

        if (fits) {
          // c_rec_call
          steps.push({
            code: 'c_rec_call',
            badge: `int sub = solve(coins, ${rem} - ${c} = ${rem - c}, memo);`,
            badgeType: 'info',
            state: {
              coins,
              amount,
              curRem: rem,
              curCoin: c,
              totalCalls: callCounter,
              cacheHits: cacheHitCounter,
              currentReturn: minCount === Infinity ? null : minCount,
              activeNodeId: nId,
              memo: [...memoArray],
              frames,
              treeNodes: getVisibleNodes(),
              treeEdges: getVisibleEdges(),
              treeWidth: staticTree.width,
              treeHeight: staticTree.height
            }
          });

          const childId = nd.children.find(cid => nodeStateMap[cid].coinUsed === c);
          let sub = Infinity;
          if (childId !== undefined) {
            sub = simulateMemo(childId, rem - c);
          } else {
            sub = memoArray[rem - c] !== -2 ? memoArray[rem - c] : (rem - c === 0 ? 0 : 1);
          }

          // c_check_sub
          const validSub = sub !== Infinity && sub !== -1;
          steps.push({
            code: 'c_check_sub',
            badge: validSub
              ? `if (sub != INF) &rarr; TRUE (sub = ${sub})`
              : `if (sub != INF) &rarr; FALSE (sub is unreachable / INF)`,
            badgeType: validSub ? 'info' : 'warn',
            state: {
              coins,
              amount,
              curRem: rem,
              curCoin: c,
              totalCalls: callCounter,
              cacheHits: cacheHitCounter,
              currentReturn: minCount === Infinity ? null : minCount,
              activeNodeId: nId,
              memo: [...memoArray],
              frames,
              treeNodes: getVisibleNodes(),
              treeEdges: getVisibleEdges(),
              treeWidth: staticTree.width,
              treeHeight: staticTree.height
            }
          });

          if (validSub) {
            const candidate = 1 + sub;
            const isBetter = candidate < minCount;
            minCount = Math.min(minCount, candidate);

            // c_update_min
            steps.push({
              code: 'c_update_min',
              badge: isBetter
                ? `minCount = Math.min(minCount, 1 + ${sub}) = ${minCount} (new best!)`
                : `minCount = Math.min(minCount, 1 + ${sub}) = ${minCount} (retaining existing best)`,
              badgeType: isBetter ? 'success' : 'info',
              state: {
                coins,
                amount,
                curRem: rem,
                curCoin: c,
                totalCalls: callCounter,
                cacheHits: cacheHitCounter,
                currentReturn: minCount,
                activeNodeId: nId,
                memo: [...memoArray],
                frames,
                treeNodes: getVisibleNodes(),
                treeEdges: getVisibleEdges(),
                treeWidth: staticTree.width,
                treeHeight: staticTree.height
              }
            });
          }
        }
      }

      // c_chk_inf
      const isInf = minCount >= Infinity;
      steps.push({
        code: 'c_chk_inf',
        badge: isInf
          ? `if (minCount >= INF) &rarr; TRUE (no valid coin combinations)`
          : `if (minCount >= INF) &rarr; FALSE (found valid minCount = ${minCount})`,
        badgeType: isInf ? 'warn' : 'info',
        state: {
          coins,
          amount,
          curRem: rem,
          curCoin: nd.coinUsed,
          totalCalls: callCounter,
          cacheHits: cacheHitCounter,
          currentReturn: isInf ? -1 : minCount,
          activeNodeId: nId,
          memo: [...memoArray],
          frames,
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height
        }
      });

      if (isInf) {
        memoArray[rem] = -1;
        steps.push({
          code: 'c_store_unreach',
          badge: `memo[${rem}] = -1; (store unreachable in cache)`,
          badgeType: 'warn',
          state: {
            coins,
            amount,
            curRem: rem,
            curCoin: nd.coinUsed,
            totalCalls: callCounter,
            cacheHits: cacheHitCounter,
            currentReturn: -1,
            activeNodeId: nId,
            memo: [...memoArray],
            frames,
            treeNodes: getVisibleNodes(),
            treeEdges: getVisibleEdges(),
            treeWidth: staticTree.width,
            treeHeight: staticTree.height
          }
        });
      } else {
        memoArray[rem] = minCount;
        steps.push({
          code: 'c_else_store',
          badge: `else clause &rarr; store computed minCount in memo[${rem}]`,
          badgeType: 'info',
          state: {
            coins,
            amount,
            curRem: rem,
            curCoin: nd.coinUsed,
            totalCalls: callCounter,
            cacheHits: cacheHitCounter,
            currentReturn: minCount,
            activeNodeId: nId,
            memo: [...memoArray],
            frames,
            treeNodes: getVisibleNodes(),
            treeEdges: getVisibleEdges(),
            treeWidth: staticTree.width,
            treeHeight: staticTree.height
          }
        });
        steps.push({
          code: 'c_store_memo',
          badge: `memo[${rem}] = ${minCount}; (cached for future O(1) lookups)`,
          badgeType: 'success',
          state: {
            coins,
            amount,
            curRem: rem,
            curCoin: nd.coinUsed,
            totalCalls: callCounter,
            cacheHits: cacheHitCounter,
            currentReturn: minCount,
            activeNodeId: nId,
            memo: [...memoArray],
            frames,
            treeNodes: getVisibleNodes(),
            treeEdges: getVisibleEdges(),
            treeWidth: staticTree.width,
            treeHeight: staticTree.height
          }
        });
      }

      const finalVal = memoArray[rem];
      nd.state = 'solved';
      nd.retVal = finalVal;

      // c_ret_memo
      steps.push({
        code: 'c_ret_memo',
        badge: `return memo[${rem}]; &rarr; Returning ${finalVal}`,
        badgeType: finalVal === -1 ? 'warn' : 'success',
        state: {
          coins,
          amount,
          curRem: rem,
          curCoin: nd.coinUsed,
          totalCalls: callCounter,
          cacheHits: cacheHitCounter,
          currentReturn: finalVal,
          activeNodeId: nId,
          memo: [...memoArray],
          frames,
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height
        }
      });

      return finalVal;
    }

    if (staticTree.nodes.length > 0) {
      const finalResult = simulateMemo(0, amount);

      // m_print
      steps.push({
        code: 'm_print',
        badge: `System.out.println(ans); &rarr; Print ${finalResult}`,
        badgeType: finalResult === -1 ? 'warn' : 'success',
        state: {
          coins,
          amount,
          curRem: amount,
          curCoin: null,
          totalCalls: callCounter,
          cacheHits: cacheHitCounter,
          currentReturn: finalResult,
          activeNodeId: 0,
          memo: [...memoArray],
          frames: [{ name: 'main', args: `result=${finalResult}` }],
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height
        }
      });

      // m_done
      steps.push({
        code: 'm_done',
        badge: `Program execution finished. Fewest coins = ${finalResult}`,
        badgeType: 'info',
        state: {
          coins,
          amount,
          curRem: amount,
          curCoin: null,
          totalCalls: callCounter,
          cacheHits: cacheHitCounter,
          currentReturn: finalResult,
          activeNodeId: null,
          memo: [...memoArray],
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
        val: i === 0 ? 0 : 'INF',
        status: i === 0 ? 'computed' : 'uncalculated'
      });
    }

    const dp = new Array(amount + 1).fill('INF');
    const bestCoin = new Array(amount + 1).fill(null);
    dp[0] = 0;
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

    // main: line 4-5 -> for read
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

    // main: line 6 -> read amt
    steps.push({
      code: 'm_read_amt',
      badge: `int amount = sc.nextInt(); (amount = ${amount})`,
      badgeType: 'info',
      state: { ...baseTabState }
    });

    // main: line 7 -> call coinChange
    steps.push({
      code: 'm_call_fn',
      badge: `int ans = coinChange(coins, amount = ${amount});`,
      badgeType: 'info',
      state: {
        ...baseTabState,
        frames: [
          { name: 'main', args: `amount=${amount}` },
          { name: 'coinChange', args: `amount=${amount}` }
        ]
      }
    });

    // coinChange: c_entry
    steps.push({
      code: 'c_entry',
      badge: `Entering coinChange(int[] coins, int amount = ${amount})`,
      badgeType: 'info',
      state: {
        ...baseTabState,
        frames: [
          { name: 'main', args: `amount=${amount}` },
          { name: 'coinChange', args: `amount=${amount}` }
        ]
      }
    });

    // c_init_dp
    steps.push({
      code: 'c_init_dp',
      badge: `int[] dp = new int[${amount + 1}]; (allocate DP table)`,
      badgeType: 'info',
      state: {
        ...baseTabState,
        frames: [
          { name: 'main', args: `amount=${amount}` },
          { name: 'coinChange', args: `amount=${amount}` }
        ]
      }
    });

    // c_fill_inf
    steps.push({
      code: 'c_fill_inf',
      badge: `Arrays.fill(dp, ${amount + 1}); (fill all entries with sentinel INF)`,
      badgeType: 'info',
      state: {
        ...baseTabState,
        frames: [
          { name: 'main', args: `amount=${amount}` },
          { name: 'coinChange', args: `amount=${amount}` }
        ]
      }
    });

    // c_base_zero
    steps.push({
      code: 'c_base_zero',
      badge: `dp[0] = 0; (base case: 0 coins needed to make sum 0)`,
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
        currentReturn: 0,
        frames: [
          { name: 'main', args: `amount=${amount}` },
          { name: 'coinChange', args: `dp[0]=0` }
        ]
      }
    });

    for (let a = 1; a <= amount; a++) {
      // c_loop_amt
      steps.push({
        code: 'c_loop_amt',
        badge: `for (int a = 1; a <= ${amount}; a++) &rarr; Sub-amount a = ${a}`,
        badgeType: 'info',
        state: {
          coins,
          amount,
          a,
          c: null,
          refA: null,
          iterations,
          isUpdating: false,
          dpCells: JSON.parse(JSON.stringify(dpCells)),
          currentReturn: dp[a] === 'INF' ? null : dp[a],
          frames: [
            { name: 'main', args: `amount=${amount}` },
            { name: 'coinChange', args: `a=${a}` }
          ]
        }
      });

      for (let ci = 0; ci < coins.length; ci++) {
        const coin = coins[ci];
        iterations++;

        // c_loop_coin
        steps.push({
          code: 'c_loop_coin',
          badge: `for (int coin : coins) &rarr; Testing coin = ${coin} at a = ${a}`,
          badgeType: 'info',
          state: {
            coins,
            amount,
            a,
            c: coin,
            refA: a - coin >= 0 ? a - coin : null,
            iterations,
            isUpdating: false,
            dpCells: JSON.parse(JSON.stringify(dpCells)),
            currentReturn: dp[a] === 'INF' ? null : dp[a],
            frames: [
              { name: 'main', args: `amount=${amount}` },
              { name: 'coinChange', args: `a=${a}, coin=${coin}` }
            ]
          }
        });

        const fits = a - coin >= 0;
        const validPrev = fits && dp[a - coin] !== 'INF';
        const condition = fits && validPrev;

        // c_check_fit
        steps.push({
          code: 'c_check_fit',
          badge: condition
            ? `if (a - coin >= 0 && dp[a - coin] != INF) &rarr; TRUE (${a} - ${coin} = ${a - coin}, dp[${a - coin}] = ${dp[a - coin]})`
            : fits
              ? `if (a - coin >= 0 && dp[a - coin] != INF) &rarr; FALSE (dp[${a - coin}] is unreachable INF)`
              : `if (a - coin >= 0 && dp[a - coin] != INF) &rarr; FALSE (${a} - ${coin} < 0, coin exceeds sub-amount)`,
          badgeType: condition ? 'info' : 'warn',
          state: {
            coins,
            amount,
            a,
            c: coin,
            refA: fits ? a - coin : null,
            iterations,
            isUpdating: false,
            dpCells: JSON.parse(JSON.stringify(dpCells)),
            currentReturn: dp[a] === 'INF' ? null : dp[a],
            frames: [
              { name: 'main', args: `amount=${amount}` },
              { name: 'coinChange', args: `fit=${condition}` }
            ]
          }
        });

        if (condition) {
          const prev = dp[a - coin];
          const candidate = prev + 1;
          const isBetter = dp[a] === 'INF' || candidate < dp[a];

          if (isBetter) {
            dp[a] = candidate;
            bestCoin[a] = coin;
            dpCells[a].val = candidate;
            dpCells[a].status = 'computed';
          }

          // c_update_dp
          steps.push({
            code: 'c_update_dp',
            badge: isBetter
              ? `dp[${a}] = Math.min(dp[${a}], 1 + dp[${a - coin}]) &rarr; New minimum: <b>${candidate}</b>!`
              : `dp[${a}] = Math.min(dp[${a}], 1 + dp[${a - coin}]) &rarr; Existing dp[${a}] = ${dp[a]} remains optimal`,
            badgeType: isBetter ? 'success' : 'info',
            state: {
              coins,
              amount,
              a,
              c: coin,
              refA: a - coin,
              iterations,
              isUpdating: isBetter,
              dpCells: JSON.parse(JSON.stringify(dpCells)),
              currentReturn: dp[a],
              frames: [
                { name: 'main', args: `amount=${amount}` },
                { name: 'coinChange', args: `dp[${a}]=${dp[a]}` }
              ]
            }
          });
        }
      }
    }

    const isInf = dp[amount] === 'INF';

    // c_chk_inf
    steps.push({
      code: 'c_chk_inf',
      badge: isInf
        ? `if (dp[amount] > amount) &rarr; TRUE (target ${amount} remains unreachable INF)`
        : `if (dp[amount] > amount) &rarr; FALSE (dp[${amount}] = ${dp[amount]} <= ${amount})`,
      badgeType: isInf ? 'warn' : 'info',
      state: {
        coins,
        amount,
        a: amount,
        c: null,
        refA: null,
        iterations,
        isUpdating: false,
        dpCells: JSON.parse(JSON.stringify(dpCells)),
        currentReturn: isInf ? -1 : dp[amount],
        frames: [
          { name: 'main', args: `amount=${amount}` },
          { name: 'coinChange', args: `check_inf` }
        ]
      }
    });

    if (isInf) {
      // c_ret_neg
      steps.push({
        code: 'c_ret_neg',
        badge: 'return -1; (cannot form target amount with given coins)',
        badgeType: 'warn',
        state: {
          coins,
          amount,
          a: amount,
          c: null,
          refA: null,
          iterations,
          isUpdating: false,
          dpCells: JSON.parse(JSON.stringify(dpCells)),
          currentReturn: -1,
          frames: [
            { name: 'main', args: `amount=${amount}` },
            { name: 'coinChange', args: `return -1` }
          ]
        }
      });
    } else {
      // c_ret_dp
      steps.push({
        code: 'c_ret_dp',
        badge: `return dp[amount]; &rarr; Returning optimal ${dp[amount]} coins!`,
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
            { name: 'coinChange', args: `return ${dp[amount]}` }
          ]
        }
      });
    }

    const finalResult = isInf ? -1 : dp[amount];

    // main: m_print
    steps.push({
      code: 'm_print',
      badge: `System.out.println(ans); &rarr; Output: ${finalResult}`,
      badgeType: finalResult === -1 ? 'warn' : 'success',
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
      badge: `Program finished. Fewest coins for amount ${amount} = ${finalResult}`,
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
const inputAmount = ref(11);

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
  return currentApproach.value === 'tabulation' ? 18 : 12;
});

const stepsData = reactive({
  steps: buildSteps('tabulation', [1, 2, 5], 11)
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
  currentApproach.value = newApproach;
  const maxAmt = newApproach === 'tabulation' ? 18 : 12;
  const maxC = newApproach === 'tabulation' ? 6 : 4;

  let coins = parseInputCoins(inputCoinsText.value);
  let amt = parseInt(inputAmount.value, 10);
  if (isNaN(amt) || amt < 0) amt = 11;

  if (amt > maxAmt || coins.length > maxC) {
    requestedLength.value = amt;
    warningModalLimit.value = `${maxAmt} (Amount)`;
    const appName = newApproach === 'tabulation' ? 'DP Tabulation' : (newApproach === 'recursion' ? 'Brute Force Recursion' : 'DP Memoization');
    warningModalTitle.value = `${appName} Limit Exceeded`;
    warningModalMsg.value = `Input amount (${amt}) or coins count (${coins.length}) exceeds the maximum safe limit for ${appName}. Values have been safely capped.`;
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
    amt = 11;
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

/* ==================================================================== */
/* PANEL RESIZERS                                                       */
/* ==================================================================== */
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
                placeholder="11"
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
                      'll-house-cur': val === st.curCoin,
                      'll-house-compare': currentApproach === 'tabulation' && val === st.c
                    }"
                  >
                    <div class="ll-house-val">{{ val }}</div>
                    <div class="ll-house-idx">
                      <span v-if="val === st.curCoin" class="ll-idx-tag ll-tag-cur">coin</span>
                      <span v-else-if="currentApproach === 'tabulation' && val === st.c" class="ll-idx-tag ll-tag-comp">c</span>
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
                      Fewest Coins:
                      <b :class="st.currentReturn === -1 ? 'll-c-red' : 'll-c-green'">
                        {{ st.currentReturn === -1 ? 'Impossible (-1)' : st.currentReturn }}
                      </b>
                    </div>
                  </div>

                  <!-- Diagram 1: DP Tabulation (Bottom-Up 1D DP Array) -->
                  <div v-if="currentApproach === 'tabulation'" class="ll-dp-tab-view">
                    <div class="ll-section-caption">
                      <span>1D DP Array Strip &mdash; <code>dp[a]</code> = Fewest coins to form sub-amount <code>a</code></span>
                      <span v-if="st.isUpdating" class="ll-calc-pill">
                        dp[{{ st.a }}] = min(dp[{{ st.a }}], 1 + dp[{{ st.a }} - {{ st.c }}])
                      </span>
                    </div>

                    <div class="ll-arr-track">
                      <template v-for="cell in st.dpCells" :key="cell.idx">
                        <div class="ll-arr-cell-wrap">
                          <!-- Pointer Tag Above Cell -->
                          <div class="ll-ptr-tag-wrap">
                            <span v-if="cell.idx === st.refA" class="ll-ptr-lbl ll-lbl-orange">&darr; a-c</span>
                            <span v-else-if="cell.idx === st.a" class="ll-ptr-lbl ll-lbl-green">&darr; a</span>
                          </div>

                          <!-- Flat Box -->
                          <div
                            class="ll-arr-box"
                            :class="{
                              'll-box-cur': cell.idx === st.a,
                              'll-box-comp': cell.idx === st.refA,
                              'll-box-found': cell.status === 'computed',
                              'll-box-uncalc': cell.val === 'INF'
                            }"
                          >
                            {{ cell.val === 'INF' ? '∞' : cell.val }}
                          </div>
                          <div class="ll-arr-idx">dp[{{ cell.idx }}]</div>
                        </div>
                      </template>
                    </div>
                  </div>

                  <!-- Diagram 2: Recursion & Memoization Decision Tree -->
                  <div v-else class="ll-tree-container">
                    <div class="ll-section-caption">
                      <span>{{ currentApproach === 'recursion' ? 'Recursive Decision Tree O(S^n)' : 'Pruned Memoization Decision Tree O(S×n)' }}</span>
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
                              :class="{ 'll-edge-take': edge.coinUsed }"
                            />
                            <text
                              :x="(edge.x1 + edge.x2) / 2"
                              :y="(edge.y1 + edge.y2) / 2"
                              class="ll-node-text-val"
                              text-anchor="middle"
                            >
                              -{{ edge.coinUsed }}
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
                            <!-- Node Text: rem=X -->
                            <text x="0" y="-3" text-anchor="middle" class="ll-node-text-call">
                              rem={{ node.rem }}
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

                      <!-- 1D Memoization Cache Strip (Only in Memoization Mode) -->
                      <div v-if="currentApproach === 'memoization' && st.memo" class="ll-memo-strip-wrap">
                        <div class="ll-memo-title">Memoization 1D Cache Table &mdash; <code>memo[rem]</code>:</div>
                        <div class="ll-memo-grid">
                          <div class="ll-memo-row">
                            <span class="ll-memo-row-lbl">rem:</span>
                            <div v-for="(cVal, cIdx) in st.memo" :key="cIdx" class="ll-memo-cell-wrap">
                              <div
                                class="ll-memo-cell"
                                :class="{
                                  'll-memo-hit': cVal !== -2,
                                  'll-memo-empty': cVal === -2
                                }"
                              >
                                {{ cVal === -2 ? '?' : cVal }}
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
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-new"></span>Optimal Coins Computed</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-uncalc"></span>Uncalculated (&infin;)</span>
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
                    The <strong>Coin Change</strong> problem (LeetCode 322) is an Unbounded Knapsack problem that demonstrates why simple greedy choice heuristics fail:
                  </p>
                  <ul>
                    <li>
                      <strong>1. Counterexample to Greedy:</strong> For <code>coins = [1, 3, 4]</code> and <code>amount = 6</code>, greedy chooses <code>4 + 1 + 1</code> (3 coins), whereas the global optimal is <code>3 + 3</code> (2 coins).
                    </li>
                    <li>
                      <strong>2. Optimal Substructure:</strong> The minimum coins to form amount <code>a</code> is <code>1 + min(dp[a - c])</code> across all valid denominations <code>c</code>.
                    </li>
                    <li>
                      <strong>3. Overlapping Subproblems:</strong> Both paths <code>11 - 5 - 1</code> and <code>11 - 1 - 5</code> reduce to finding the fewest coins for remaining sum <code>5</code>.
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
                        <td>O(S<sup>n</sup>) Exponential</td>
                        <td>O(S) Call Stack</td>
                        <td>Explores redundant branches repeatedly. Exceeds time limit quickly.</td>
                      </tr>
                      <tr>
                        <td><strong>DP Memoization</strong></td>
                        <td>O(S &times; n) Linear-Proportional</td>
                        <td>O(S) Cache + Stack</td>
                        <td>Top-down recursion with O(1) state caching. Optimal time complexity.</td>
                      </tr>
                      <tr>
                        <td><strong>DP Tabulation</strong></td>
                        <td>O(S &times; n) Systematic</td>
                        <td>O(S) 1D DP Array</td>
                        <td>Bottom-up iterative array fill. Zero recursion overhead and cache friendly.</td>
                      </tr>
                    </tbody>
                  </table>

                  <h3>Recurrence Formula</h3>
                  <p class="ll-math-box">
                    dp[a] = min<sub>c &isin; coins, c &le; a</sub> ( 1 + dp[a - c] )
                  </p>
                  <p>
                    Base conditions: <code>dp[0] = 0</code> (0 coins needed for amount 0), and <code>dp[a] = &infin;</code> for all <code>a &gt; 0</code>.
                  </p>

                  <div class="ll-note">
                    <strong>Takeaway:</strong> If <code>dp[amount] == &infin;</code> after the iterative loops complete, no combination of coin denominations can form the target amount, so the algorithm returns <code>-1</code>.
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

/* 1D Memoization Table Strip */
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
