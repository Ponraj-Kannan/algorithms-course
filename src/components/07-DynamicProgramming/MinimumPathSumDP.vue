<script setup>
import { ref, computed, reactive, watch, onMounted, onUnmounted } from 'vue';

const props = defineProps({
  topic: { type: String, default: 'Dynamic Programming' },
  subTopic: { type: String, default: 'Minimum Path Sum (LeetCode 64)' }
});

/* ==================================================================== */
/* APPROACHES & MULTI-LANGUAGE 100% EXECUTABLE CODE SPECIFICATIONS      */
/* ==================================================================== */
const APPROACHES = [
  { id: 'recursion',   label: 'Brute Force', desc: 'O(2^(m+n)) Exponential — Top-Down Exhaustive Search' },
  { id: 'memoization', label: 'Memoization', desc: 'O(m×n) Time & Space — Top-Down with 2D Lookup Table' },
  { id: 'tabulation',  label: 'Tabulation',  desc: 'O(m×n) Time & Space — Bottom-Up Iterative 2D DP Matrix' }
];

const currentApproach = ref('tabulation');
const lang = ref('java');
const rightTab = ref('code'); // 'code' | 'pseudo' | 'complexity'

/* ==================================================================== */
/* MULTI-LANGUAGE CODE SNIPPETS WITH EXACT SYNCHRONIZED TAGS            */
/* ==================================================================== */
const CODES = {
  recursion: {
    java: [
      ['',             'import java.util.Scanner;'],
      ['',             ''],
      ['',             'public class Main {'],
      ['c_entry',      '    static int minCost(int r, int c, int[][] grid, int m, int n) {'],
      ['c_base_dest',  '        if (r == m - 1 && c == n - 1) {'],
      ['c_ret_dest',   '            return grid[r][c];'],
      ['',             '        }'],
      ['c_base_oob',   '        if (r >= m || c >= n) {'],
      ['c_ret_oob',    '            return Integer.MAX_VALUE;'],
      ['',             '        }'],
      ['c_call_down',  '        int down = minCost(r + 1, c, grid, m, n);'],
      ['c_call_right', '        int right = minCost(r, c + 1, grid, m, n);'],
      ['c_calc_min',   '        int minSub = Math.min(down, right);'],
      ['c_ret_min',    '        return grid[r][c] + minSub;'],
      ['',             '    }'],
      ['',             ''],
      ['',             '    public static void main(String[] args) {'],
      ['m_scanner',    '        Scanner sc = new Scanner(System.in);'],
      ['m_read_dims',  '        int m = sc.nextInt(); int n = sc.nextInt();'],
      ['m_alloc_grid', '        int[][] grid = new int[m][n];'],
      ['m_for_r',      '        for (int r = 0; r < m; r++) {'],
      ['m_for_c',      '            for (int c = 0; c < n; c++) {'],
      ['m_read_cell',  '                grid[r][c] = sc.nextInt();'],
      ['',             '            }'],
      ['',             '        }'],
      ['m_call_fn',    '        int ans = minCost(0, 0, grid, m, n);'],
      ['m_print',      '        System.out.println(ans);'],
      ['m_done',       '    }'],
      ['',             '}']
    ],
    c: [
      ['',             '#include <stdio.h>'],
      ['',             '#define INF 1000000000'],
      ['',             ''],
      ['',             'int min(int a, int b) {'],
      ['',             '    if (a < b) {'],
      ['',             '        return a;'],
      ['',             '    }'],
      ['',             '    return b;'],
      ['',             '}'],
      ['',             ''],
      ['c_entry',      'int minCost(int r, int c, int grid[10][10], int m, int n) {'],
      ['c_base_dest',  '    if (r == m - 1 && c == n - 1) {'],
      ['c_ret_dest',   '        return grid[r][c];'],
      ['',             '    }'],
      ['c_base_oob',   '    if (r >= m || c >= n) {'],
      ['c_ret_oob',    '        return INF;'],
      ['',             '    }'],
      ['c_call_down',  '    int down = minCost(r + 1, c, grid, m, n);'],
      ['c_call_right', '    int right = minCost(r, c + 1, grid, m, n);'],
      ['c_calc_min',   '    int minSub = min(down, right);'],
      ['c_ret_min',    '    return grid[r][c] + minSub;'],
      ['',             '}'],
      ['',             ''],
      ['',             'int main() {'],
      ['m_scanner',    '    int m, n;'],
      ['m_read_dims',  '    scanf("%d %d", &m, &n);'],
      ['m_alloc_grid', '    int grid[10][10];'],
      ['m_for_r',      '    for (int r = 0; r < m; r++) {'],
      ['m_for_c',      '        for (int c = 0; c < n; c++) {'],
      ['m_read_cell',  '            scanf("%d", &grid[r][c]);'],
      ['',             '        }'],
      ['',             '    }'],
      ['m_call_fn',    '    int ans = minCost(0, 0, grid, m, n);'],
      ['m_print',      '    printf("%d\\n", ans);'],
      ['m_done',       '    return 0;'],
      ['',             '}']
    ],
    cpp: [
      ['',             '#include <iostream>'],
      ['',             '#include <vector>'],
      ['',             '#include <algorithm>'],
      ['',             '#include <climits>'],
      ['',             'using namespace std;'],
      ['',             ''],
      ['c_entry',      'int minCost(int r, int c, const vector<vector<int>>& grid, int m, int n) {'],
      ['c_base_dest',  '    if (r == m - 1 && c == n - 1) {'],
      ['c_ret_dest',   '        return grid[r][c];'],
      ['',             '    }'],
      ['c_base_oob',   '    if (r >= m || c >= n) {'],
      ['c_ret_oob',    '        return INT_MAX;'],
      ['',             '    }'],
      ['c_call_down',  '    int down = minCost(r + 1, c, grid, m, n);'],
      ['c_call_right', '    int right = minCost(r, c + 1, grid, m, n);'],
      ['c_calc_min',   '    int minSub = min(down, right);'],
      ['c_ret_min',    '    return grid[r][c] + minSub;'],
      ['',             '}'],
      ['',             ''],
      ['',             'int main() {'],
      ['m_scanner',    '    int m, n;'],
      ['m_read_dims',  '    cin >> m >> n;'],
      ['m_alloc_grid', '    vector<vector<int>> grid(m, vector<int>(n));'],
      ['m_for_r',      '    for (int r = 0; r < m; r++) {'],
      ['m_for_c',      '        for (int c = 0; c < n; c++) {'],
      ['m_read_cell',  '            cin >> grid[r][c];'],
      ['',             '        }'],
      ['',             '    }'],
      ['m_call_fn',    '    int ans = minCost(0, 0, grid, m, n);'],
      ['m_print',      '    cout << ans << endl;'],
      ['m_done',       '    return 0;'],
      ['',             '}']
    ],
    python: [
      ['',             'import sys'],
      ['',             'INF = float("inf")'],
      ['',             ''],
      ['c_entry',      'def min_cost(r, c, grid, m, n):'],
      ['c_base_dest',  '    if r == m - 1 and c == n - 1:'],
      ['c_ret_dest',   '        return grid[r][c]'],
      ['c_base_oob',   '    if r >= m or c >= n:'],
      ['c_ret_oob',    '        return INF'],
      ['c_call_down',  '    down = min_cost(r + 1, c, grid, m, n)'],
      ['c_call_right', '    right = min_cost(r, c + 1, grid, m, n)'],
      ['c_calc_min',   '    min_sub = min(down, right)'],
      ['c_ret_min',    '    return grid[r][c] + min_sub'],
      ['',             ''],
      ['if __name__ == "__main__":'],
      ['m_scanner',    '    tokens = sys.stdin.read().split()'],
      ['m_read_dims',  '    if len(tokens) >= 2:'],
      ['',             '        m, n = int(tokens[0]), int(tokens[1])'],
      ['m_alloc_grid', '        grid = []'],
      ['',             '        idx = 2'],
      ['m_for_r',      '        for r in range(m):'],
      ['',             '            row = []'],
      ['m_for_c',      '            for c in range(n):'],
      ['m_read_cell',  '                row.append(int(tokens[idx]))'],
      ['',             '                idx += 1'],
      ['',             '            grid.append(row)'],
      ['m_call_fn',    '        ans = min_cost(0, 0, grid, m, n)'],
      ['m_print',      '        print(ans)'],
      ['m_done',       '        sys.exit(0)']
    ],
    javascript: [
      ['',             'const fs = require("fs");'],
      ['',             ''],
      ['c_entry',      'function minCost(r, c, grid, m, n) {'],
      ['c_base_dest',  '    if (r === m - 1 && c === n - 1) {'],
      ['c_ret_dest',   '        return grid[r][c];'],
      ['',             '    }'],
      ['c_base_oob',   '    if (r >= m || c >= n) {'],
      ['c_ret_oob',    '        return Infinity;'],
      ['',             '    }'],
      ['c_call_down',  '    const down = minCost(r + 1, c, grid, m, n);'],
      ['c_call_right', '    const right = minCost(r, c + 1, grid, m, n);'],
      ['c_calc_min',   '    const minSub = Math.min(down, right);'],
      ['c_ret_min',    '    return grid[r][c] + minSub;'],
      ['',             '}'],
      ['',             ''],
      ['m_scanner',    'const tokens = fs.readFileSync(0, "utf-8").trim().split(/\\s+/);'],
      ['m_read_dims',  'if (tokens.length >= 2) {'],
      ['',             '    const m = parseInt(tokens[0], 10);'],
      ['',             '    const n = parseInt(tokens[1], 10);'],
      ['m_alloc_grid', '    const grid = [];'],
      ['',             '    let idx = 2;'],
      ['m_for_r',      '    for (let r = 0; r < m; r++) {'],
      ['',             '        const row = [];'],
      ['m_for_c',      '        for (let c = 0; c < n; c++) {'],
      ['m_read_cell',  '            row.push(parseInt(tokens[idx++], 10));'],
      ['',             '        }'],
      ['',             '        grid.push(row);'],
      ['',             '    }'],
      ['m_call_fn',    '    const ans = minCost(0, 0, grid, m, n);'],
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
      ['c_entry',      '    static int solve(int r, int c, int[][] grid, int m, int n, int[][] memo) {'],
      ['c_base_dest',  '        if (r == m - 1 && c == n - 1) {'],
      ['c_ret_dest',   '            return grid[r][c];'],
      ['',             '        }'],
      ['c_base_oob',   '        if (r >= m || c >= n) {'],
      ['c_ret_oob',    '            return Integer.MAX_VALUE;'],
      ['',             '        }'],
      ['c_memo_chk',   '        if (memo[r][c] != -1) {'],
      ['c_ret_cache',  '            return memo[r][c];'],
      ['',             '        }'],
      ['c_call_down',  '        int down = solve(r + 1, c, grid, m, n, memo);'],
      ['c_call_right', '        int right = solve(r, c + 1, grid, m, n, memo);'],
      ['c_calc_min',   '        int minSub = Math.min(down, right);'],
      ['c_store_memo', '        memo[r][c] = grid[r][c] + minSub;'],
      ['c_ret_memo',   '        return memo[r][c];'],
      ['',             '    }'],
      ['',             ''],
      ['',             '    public static void main(String[] args) {'],
      ['m_scanner',    '        Scanner sc = new Scanner(System.in);'],
      ['m_read_dims',  '        int m = sc.nextInt(); int n = sc.nextInt();'],
      ['m_alloc_grid', '        int[][] grid = new int[m][n];'],
      ['m_for_r',      '        for (int r = 0; r < m; r++) {'],
      ['m_for_c',      '            for (int c = 0; c < n; c++) {'],
      ['m_read_cell',  '                grid[r][c] = sc.nextInt();'],
      ['',             '            }'],
      ['',             '        }'],
      ['m_alloc_memo', '        int[][] memo = new int[m][n];'],
      ['m_fill_memo',  '        for (int[] row : memo) {'],
      ['',             '            Arrays.fill(row, -1);'],
      ['',             '        }'],
      ['m_call_fn',    '        int ans = solve(0, 0, grid, m, n, memo);'],
      ['m_print',      '        System.out.println(ans);'],
      ['m_done',       '    }'],
      ['',             '}']
    ],
    c: [
      ['',             '#include <stdio.h>'],
      ['',             '#include <string.h>'],
      ['',             '#define INF 1000000000'],
      ['',             ''],
      ['',             'int min(int a, int b) {'],
      ['',             '    if (a < b) {'],
      ['',             '        return a;'],
      ['',             '    }'],
      ['',             '    return b;'],
      ['',             '}'],
      ['',             ''],
      ['c_entry',      'int solve(int r, int c, int grid[10][10], int m, int n, int memo[10][10]) {'],
      ['c_base_dest',  '    if (r == m - 1 && c == n - 1) {'],
      ['c_ret_dest',   '        return grid[r][c];'],
      ['',             '    }'],
      ['c_base_oob',   '    if (r >= m || c >= n) {'],
      ['c_ret_oob',    '        return INF;'],
      ['',             '    }'],
      ['c_memo_chk',   '    if (memo[r][c] != -1) {'],
      ['c_ret_cache',  '        return memo[r][c];'],
      ['',             '    }'],
      ['c_call_down',  '    int down = solve(r + 1, c, grid, m, n, memo);'],
      ['c_call_right', '    int right = solve(r, c + 1, grid, m, n, memo);'],
      ['c_calc_min',   '    int minSub = min(down, right);'],
      ['c_store_memo', '    memo[r][c] = grid[r][c] + minSub;'],
      ['c_ret_memo',   '    return memo[r][c];'],
      ['',             '}'],
      ['',             ''],
      ['',             'int main() {'],
      ['m_scanner',    '    int m, n;'],
      ['m_read_dims',  '    scanf("%d %d", &m, &n);'],
      ['m_alloc_grid', '    int grid[10][10];'],
      ['m_for_r',      '    for (int r = 0; r < m; r++) {'],
      ['m_for_c',      '        for (int c = 0; c < n; c++) {'],
      ['m_read_cell',  '            scanf("%d", &grid[r][c]);'],
      ['',             '        }'],
      ['',             '    }'],
      ['m_alloc_memo', '    int memo[10][10];'],
      ['m_fill_memo',  '    memset(memo, -1, sizeof(memo));'],
      ['m_call_fn',    '    int ans = solve(0, 0, grid, m, n, memo);'],
      ['m_print',      '    printf("%d\\n", ans);'],
      ['m_done',       '    return 0;'],
      ['',             '}']
    ],
    cpp: [
      ['',             '#include <iostream>'],
      ['',             '#include <vector>'],
      ['',             '#include <algorithm>'],
      ['',             '#include <climits>'],
      ['',             'using namespace std;'],
      ['',             ''],
      ['c_entry',      'int solve(int r, int c, const vector<vector<int>>& grid, int m, int n, vector<vector<int>>& memo) {'],
      ['c_base_dest',  '    if (r == m - 1 && c == n - 1) {'],
      ['c_ret_dest',   '        return grid[r][c];'],
      ['',             '    }'],
      ['c_base_oob',   '    if (r >= m || c >= n) {'],
      ['c_ret_oob',    '        return INT_MAX;'],
      ['',             '    }'],
      ['c_memo_chk',   '    if (memo[r][c] != -1) {'],
      ['c_ret_cache',  '        return memo[r][c];'],
      ['',             '    }'],
      ['c_call_down',  '    int down = solve(r + 1, c, grid, m, n, memo);'],
      ['c_call_right', '    int right = solve(r, c + 1, grid, m, n, memo);'],
      ['c_calc_min',   '    int minSub = min(down, right);'],
      ['c_store_memo', '    memo[r][c] = grid[r][c] + minSub;'],
      ['c_ret_memo',   '    return memo[r][c];'],
      ['',             '}'],
      ['',             ''],
      ['',             'int main() {'],
      ['m_scanner',    '    int m, n;'],
      ['m_read_dims',  '    cin >> m >> n;'],
      ['m_alloc_grid', '    vector<vector<int>> grid(m, vector<int>(n));'],
      ['m_for_r',      '    for (int r = 0; r < m; r++) {'],
      ['m_for_c',      '        for (int c = 0; c < n; c++) {'],
      ['m_read_cell',  '            cin >> grid[r][c];'],
      ['',             '        }'],
      ['',             '    }'],
      ['m_alloc_memo', '    vector<vector<int>> memo(m, vector<int>(n, -1));'],
      ['m_fill_memo',  '    // memo matrix initialized with -1'],
      ['m_call_fn',    '    int ans = solve(0, 0, grid, m, n, memo);'],
      ['m_print',      '    cout << ans << endl;'],
      ['m_done',       '    return 0;'],
      ['',             '}']
    ],
    python: [
      ['',             'import sys'],
      ['',             'INF = float("inf")'],
      ['',             ''],
      ['c_entry',      'def solve(r, c, grid, m, n, memo):'],
      ['c_base_dest',  '    if r == m - 1 and c == n - 1:'],
      ['c_ret_dest',   '        return grid[r][c]'],
      ['c_base_oob',   '    if r >= m or c >= n:'],
      ['c_ret_oob',    '        return INF'],
      ['c_memo_chk',   '    if memo[r][c] != -1:'],
      ['c_ret_cache',  '        return memo[r][c]'],
      ['c_call_down',  '    down = solve(r + 1, c, grid, m, n, memo)'],
      ['c_call_right', '    right = solve(r, c + 1, grid, m, n, memo)'],
      ['c_calc_min',   '    min_sub = min(down, right)'],
      ['c_store_memo', '    memo[r][c] = grid[r][c] + min_sub'],
      ['c_ret_memo',   '    return memo[r][c]'],
      ['',             ''],
      ['if __name__ == "__main__":'],
      ['m_scanner',    '    tokens = sys.stdin.read().split()'],
      ['m_read_dims',  '    if len(tokens) >= 2:'],
      ['',             '        m, n = int(tokens[0]), int(tokens[1])'],
      ['m_alloc_grid', '        grid = []'],
      ['',             '        idx = 2'],
      ['m_for_r',      '        for r in range(m):'],
      ['',             '            row = []'],
      ['m_for_c',      '            for c in range(n):'],
      ['m_read_cell',  '                row.append(int(tokens[idx]))'],
      ['',             '                idx += 1'],
      ['',             '            grid.append(row)'],
      ['m_alloc_memo', '        memo = [[-1] * n for _ in range(m)]'],
      ['m_fill_memo',  '        # 2D memo grid ready'],
      ['m_call_fn',    '        ans = solve(0, 0, grid, m, n, memo)'],
      ['m_print',      '        print(ans)'],
      ['m_done',       '        sys.exit(0)']
    ],
    javascript: [
      ['',             'const fs = require("fs");'],
      ['',             ''],
      ['c_entry',      'function solve(r, c, grid, m, n, memo) {'],
      ['c_base_dest',  '    if (r === m - 1 && c === n - 1) {'],
      ['c_ret_dest',   '        return grid[r][c];'],
      ['',             '    }'],
      ['c_base_oob',   '    if (r >= m || c >= n) {'],
      ['c_ret_oob',    '        return Infinity;'],
      ['',             '    }'],
      ['c_memo_chk',   '    if (memo[r][c] !== -1) {'],
      ['c_ret_cache',  '        return memo[r][c];'],
      ['',             '    }'],
      ['c_call_down',  '    const down = solve(r + 1, c, grid, m, n, memo);'],
      ['c_call_right', '    const right = solve(r, c + 1, grid, m, n, memo);'],
      ['c_calc_min',   '    const minSub = Math.min(down, right);'],
      ['c_store_memo', '    memo[r][c] = grid[r][c] + minSub;'],
      ['c_ret_memo',   '    return memo[r][c];'],
      ['',             '}'],
      ['',             ''],
      ['m_scanner',    'const tokens = fs.readFileSync(0, "utf-8").trim().split(/\\s+/);'],
      ['m_read_dims',  'if (tokens.length >= 2) {'],
      ['',             '    const m = parseInt(tokens[0], 10);'],
      ['',             '    const n = parseInt(tokens[1], 10);'],
      ['m_alloc_grid', '    const grid = [];'],
      ['',             '    let idx = 2;'],
      ['m_for_r',      '    for (let r = 0; r < m; r++) {'],
      ['',             '        const row = [];'],
      ['m_for_c',      '        for (let c = 0; c < n; c++) {'],
      ['m_read_cell',  '            row.push(parseInt(tokens[idx++], 10));'],
      ['',             '        }'],
      ['',             '        grid.push(row);'],
      ['',             '    }'],
      ['m_alloc_memo', '    const memo = Array.from({ length: m }, () => new Array(n).fill(-1));'],
      ['m_fill_memo',  '    // 2D memo grid initialized'],
      ['m_call_fn',    '    const ans = solve(0, 0, grid, m, n, memo);'],
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
      ['c_entry',      '    static int minPathSum(int[][] grid) {'],
      ['c_get_dims',   '        int m = grid.length, n = grid[0].length;'],
      ['c_alloc_dp',   '        int[][] dp = new int[m][n];'],
      ['c_loop_r',     '        for (int r = 0; r < m; r++) {'],
      ['c_loop_c',     '            for (int c = 0; c < n; c++) {'],
      ['c_check_origin','                if (r == 0 && c == 0) {'],
      ['c_set_origin', '                    dp[r][c] = grid[r][c];'],
      ['c_check_first_row','                } else if (r == 0) {'],
      ['c_set_first_row','                    dp[r][c] = dp[r][c - 1] + grid[r][c];'],
      ['c_check_first_col','                } else if (c == 0) {'],
      ['c_set_first_col','                    dp[r][c] = dp[r - 1][c] + grid[r][c];'],
      ['c_else_gen',   '                } else {'],
      ['c_calc_min',   '                    int minNeighbor = Math.min(dp[r - 1][c], dp[r][c - 1]);'],
      ['c_set_gen',    '                    dp[r][c] = grid[r][c] + minNeighbor;'],
      ['',             '                }'],
      ['',             '            }'],
      ['',             '        }'],
      ['c_ret_dp',     '        return dp[m - 1][n - 1];'],
      ['',             '    }'],
      ['',             ''],
      ['',             '    public static void main(String[] args) {'],
      ['m_scanner',    '        Scanner sc = new Scanner(System.in);'],
      ['m_read_dims',  '        int m = sc.nextInt(); int n = sc.nextInt();'],
      ['m_alloc_grid', '        int[][] grid = new int[m][n];'],
      ['m_for_r',      '        for (int r = 0; r < m; r++) {'],
      ['m_for_c',      '            for (int c = 0; c < n; c++) {'],
      ['m_read_cell',  '                grid[r][c] = sc.nextInt();'],
      ['',             '            }'],
      ['',             '        }'],
      ['m_call_fn',    '        int ans = minPathSum(grid);'],
      ['m_print',      '        System.out.println(ans);'],
      ['m_done',       '    }'],
      ['',             '}']
    ],
    c: [
      ['',             '#include <stdio.h>'],
      ['',             ''],
      ['',             'int min(int a, int b) {'],
      ['',             '    if (a < b) {'],
      ['',             '        return a;'],
      ['',             '    }'],
      ['',             '    return b;'],
      ['',             '}'],
      ['',             ''],
      ['c_entry',      'int minPathSum(int grid[10][10], int m, int n) {'],
      ['c_get_dims',   '    int dp[10][10];'],
      ['c_alloc_dp',   '    // 2D DP matrix ready'],
      ['c_loop_r',     '    for (int r = 0; r < m; r++) {'],
      ['c_loop_c',     '        for (int c = 0; c < n; c++) {'],
      ['c_check_origin','            if (r == 0 && c == 0) {'],
      ['c_set_origin', '                dp[r][c] = grid[r][c];'],
      ['c_check_first_row','            } else if (r == 0) {'],
      ['c_set_first_row','                dp[r][c] = dp[r][c - 1] + grid[r][c];'],
      ['c_check_first_col','            } else if (c == 0) {'],
      ['c_set_first_col','                dp[r][c] = dp[r - 1][c] + grid[r][c];'],
      ['c_else_gen',   '            } else {'],
      ['c_calc_min',   '                int minNeighbor = min(dp[r - 1][c], dp[r][c - 1]);'],
      ['c_set_gen',    '                dp[r][c] = grid[r][c] + minNeighbor;'],
      ['',             '            }'],
      ['',             '        }'],
      ['',             '    }'],
      ['c_ret_dp',     '    return dp[m - 1][n - 1];'],
      ['',             '}'],
      ['',             ''],
      ['',             'int main() {'],
      ['m_scanner',    '    int m, n;'],
      ['m_read_dims',  '    scanf("%d %d", &m, &n);'],
      ['m_alloc_grid', '    int grid[10][10];'],
      ['m_for_r',      '    for (int r = 0; r < m; r++) {'],
      ['m_for_c',      '        for (int c = 0; c < n; c++) {'],
      ['m_read_cell',  '            scanf("%d", &grid[r][c]);'],
      ['',             '        }'],
      ['',             '    }'],
      ['m_call_fn',    '    int ans = minPathSum(grid, m, n);'],
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
      ['c_entry',      'int minPathSum(const vector<vector<int>>& grid) {'],
      ['c_get_dims',   '    int m = grid.size(), n = grid[0].size();'],
      ['c_alloc_dp',   '    vector<vector<int>> dp(m, vector<int>(n));'],
      ['c_loop_r',     '    for (int r = 0; r < m; r++) {'],
      ['c_loop_c',     '        for (int c = 0; c < n; c++) {'],
      ['c_check_origin','            if (r == 0 && c == 0) {'],
      ['c_set_origin', '                dp[r][c] = grid[r][c];'],
      ['c_check_first_row','            } else if (r == 0) {'],
      ['c_set_first_row','                dp[r][c] = dp[r][c - 1] + grid[r][c];'],
      ['c_check_first_col','            } else if (c == 0) {'],
      ['c_set_first_col','                dp[r][c] = dp[r - 1][c] + grid[r][c];'],
      ['c_else_gen',   '            } else {'],
      ['c_calc_min',   '                int minNeighbor = min(dp[r - 1][c], dp[r][c - 1]);'],
      ['c_set_gen',    '                dp[r][c] = grid[r][c] + minNeighbor;'],
      ['',             '            }'],
      ['',             '        }'],
      ['',             '    }'],
      ['c_ret_dp',     '    return dp[m - 1][n - 1];'],
      ['',             '}'],
      ['',             ''],
      ['',             'int main() {'],
      ['m_scanner',    '    int m, n;'],
      ['m_read_dims',  '    cin >> m >> n;'],
      ['m_alloc_grid', '    vector<vector<int>> grid(m, vector<int>(n));'],
      ['m_for_r',      '    for (int r = 0; r < m; r++) {'],
      ['m_for_c',      '        for (int c = 0; c < n; c++) {'],
      ['m_read_cell',  '            cin >> grid[r][c];'],
      ['',             '        }'],
      ['',             '    }'],
      ['m_call_fn',    '    int ans = minPathSum(grid);'],
      ['m_print',      '    cout << ans << endl;'],
      ['m_done',       '    return 0;'],
      ['',             '}']
    ],
    python: [
      ['',             'import sys'],
      ['',             ''],
      ['c_entry',      'def min_path_sum(grid):'],
      ['c_get_dims',   '    m, n = len(grid), len(grid[0])'],
      ['c_alloc_dp',   '    dp = [[0] * n for _ in range(m)]'],
      ['c_loop_r',     '    for r in range(m):'],
      ['c_loop_c',     '        for c in range(n):'],
      ['c_check_origin','            if r == 0 and c == 0:'],
      ['c_set_origin', '                dp[r][c] = grid[r][c]'],
      ['c_check_first_row','            elif r == 0:'],
      ['c_set_first_row','                dp[r][c] = dp[r][c - 1] + grid[r][c]'],
      ['c_check_first_col','            elif c == 0:'],
      ['c_set_first_col','                dp[r][c] = dp[r - 1][c] + grid[r][c]'],
      ['c_else_gen',   '            else:'],
      ['c_calc_min',   '                min_neighbor = min(dp[r - 1][c], dp[r][c - 1])'],
      ['c_set_gen',    '                dp[r][c] = grid[r][c] + min_neighbor'],
      ['c_ret_dp',     '    return dp[m - 1][n - 1]'],
      ['',             ''],
      ['if __name__ == "__main__":'],
      ['m_scanner',    '    tokens = sys.stdin.read().split()'],
      ['m_read_dims',  '    if len(tokens) >= 2:'],
      ['',             '        m, n = int(tokens[0]), int(tokens[1])'],
      ['m_alloc_grid', '        grid = []'],
      ['',             '        idx = 2'],
      ['m_for_r',      '        for r in range(m):'],
      ['',             '            row = []'],
      ['m_for_c',      '            for c in range(n):'],
      ['m_read_cell',  '                row.append(int(tokens[idx]))'],
      ['',             '                idx += 1'],
      ['',             '            grid.append(row)'],
      ['m_call_fn',    '        ans = min_path_sum(grid)'],
      ['m_print',      '        print(ans)'],
      ['m_done',       '        sys.exit(0)']
    ],
    javascript: [
      ['',             'const fs = require("fs");'],
      ['',             ''],
      ['c_entry',      'function minPathSum(grid) {'],
      ['c_get_dims',   '    const m = grid.length, n = grid[0].length;'],
      ['c_alloc_dp',   '    const dp = Array.from({ length: m }, () => new Array(n).fill(0));'],
      ['c_loop_r',     '    for (let r = 0; r < m; r++) {'],
      ['c_loop_c',     '        for (let c = 0; c < n; c++) {'],
      ['c_check_origin','            if (r === 0 && c === 0) {'],
      ['c_set_origin', '                dp[r][c] = grid[r][c];'],
      ['c_check_first_row','            } else if (r === 0) {'],
      ['c_set_first_row','                dp[r][c] = dp[r][c - 1] + grid[r][c];'],
      ['c_check_first_col','            } else if (c === 0) {'],
      ['c_set_first_col','                dp[r][c] = dp[r - 1][c] + grid[r][c];'],
      ['c_else_gen',   '            } else {'],
      ['c_calc_min',   '                const minNeighbor = Math.min(dp[r - 1][c], dp[r][c - 1]);'],
      ['c_set_gen',    '                dp[r][c] = grid[r][c] + minNeighbor;'],
      ['',             '            }'],
      ['',             '        }'],
      ['',             '    }'],
      ['c_ret_dp',     '    return dp[m - 1][n - 1];'],
      ['',             '}'],
      ['',             ''],
      ['m_scanner',    'const tokens = fs.readFileSync(0, "utf-8").trim().split(/\\s+/);'],
      ['m_read_dims',  'if (tokens.length >= 2) {'],
      ['',             '    const m = parseInt(tokens[0], 10);'],
      ['',             '    const n = parseInt(tokens[1], 10);'],
      ['m_alloc_grid', '    const grid = [];'],
      ['',             '    let idx = 2;'],
      ['m_for_r',      '    for (let r = 0; r < m; r++) {'],
      ['',             '        const row = [];'],
      ['m_for_c',      '        for (let c = 0; c < n; c++) {'],
      ['m_read_cell',  '            row.push(parseInt(tokens[idx++], 10));'],
      ['',             '        }'],
      ['',             '        grid.push(row);'],
      ['',             '    }'],
      ['m_call_fn',    '    const ans = minPathSum(grid);'],
      ['m_print',      '    console.log(ans);'],
      ['m_done',       '    process.exit(0);'],
      ['',             '}']
    ]
  }
};

const PSEUDOCODES = {
  recursion: [
    'function minCost(r, c, grid, m, n):',
    '  if r == m - 1 and c == n - 1: return grid[r][c] // Base: Reached destination (bottom-right)',
    '  if r >= m or c >= n: return infinity          // Base: Out of grid boundaries',
    '  down = minCost(r + 1, c, grid, m, n)          // Choice 1: Move DOWN',
    '  right = minCost(r, c + 1, grid, m, n)         // Choice 2: Move RIGHT',
    '  minSub = min(down, right)',
    '  return grid[r][c] + minSub                    // Cost of cell + best path'
  ],
  memoization: [
    'function solve(r, c, grid, m, n, memo):',
    '  if r == m - 1 and c == n - 1: return grid[r][c] // Base: Destination reached',
    '  if r >= m or c >= n: return infinity          // Base: Out of bounds',
    '  if memo[r][c] != -1: return memo[r][c]        // O(1) Cache Hit',
    '  down = solve(r + 1, c, grid, m, n, memo)',
    '  right = solve(r, c + 1, grid, m, n, memo)',
    '  memo[r][c] = grid[r][c] + min(down, right)    // Store in 2D memo grid',
    '  return memo[r][c]'
  ],
  tabulation: [
    'function minPathSum(grid):',
    '  m = rows, n = cols',
    '  dp = 2D array of size m x n',
    '  for r from 0 to m - 1:',
    '    for c from 0 to n - 1:',
'      if r == 0 and c == 0: dp[r][c] = grid[r][c]',
    '      else if r == 0:       dp[r][c] = dp[0][c - 1] + grid[0][c]      // Only from left',
    '      else if c == 0:       dp[r][c] = dp[r - 1][0] + grid[r][0]      // Only from top',
    '      else:                 dp[r][c] = grid[r][c] + min(dp[r - 1][c], dp[r][c - 1])',
    '  return dp[m - 1][n - 1]                       // Optimal minimum sum'
  ]
};
/* ==================================================================== */
/* STATIC TREE GENERATION (GRID PATH DECISION TREE)                     */
/* ==================================================================== */
function generateStaticTree(approach, grid) {
  const m = grid.length;
  const n = grid[0].length;
  const nodes = [];
  const edges = [];
  let nextId = 1;
  const visitedMemo = new Set();

  function dfs(r, c, parentId, branchType, depth) {
    if (depth > 6) return null; // safety guard
    const myId = nextId++;
    const isDest = (r === m - 1 && c === n - 1);
    const isOob = (r >= m || c >= n);
    const cellVal = (!isOob) ? grid[r][c] : 'INF';
    const key = `${r},${c}`;
    const isRedundant = approach === 'recursion' && visitedMemo.has(key);
    const isCacheHit = approach === 'memoization' && visitedMemo.has(key);

    const node = {
      id: myId,
      r,
      c,
      cost: cellVal,
      branchType, // 'down' | 'right' | 'root'
      depth,
      isDest,
      isOob,
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
        label: branchType === 'down' ? '↓ Down' : '→ Right',
        branch: branchType === 'down' ? 'D' : 'R'
      });
    }

    if (isDest || isOob) {
      return myId;
    }

    if (approach === 'memoization' && isCacheHit) {
      return myId;
    }
    visitedMemo.add(key);

    // Branch 1: Down (r + 1, c)
    const downId = dfs(r + 1, c, myId, 'down', depth + 1);
    if (downId !== null) node.children.push(downId);

    // Branch 2: Right (r, c + 1)
    const rightId = dfs(r, c + 1, myId, 'right', depth + 1);
    if (rightId !== null) node.children.push(rightId);

    return myId;
  }

  dfs(0, 0, null, 'root', 0);

  // Layout assignment by depth
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

  const levelWidth = Math.max(540, maxPerLevel * 70);
  const levelHeight = 60;
  const layoutMap = {};

  Object.entries(depthGroups).forEach(([dStr, group]) => {
    const d = Number(dStr);
    const count = group.length;
    const spacing = levelWidth / (count + 1);
    group.forEach((nd, idx) => {
      layoutMap[nd.id] = {
        x: Math.round(spacing * (idx + 1)),
        y: 35 + d * levelHeight
      };
    });
  });

  return {
    nodes,
    edges,
    layoutMap,
    width: levelWidth,
    height: Math.max(260, (maxDepth + 1) * levelHeight + 50)
  };
}

/* ==================================================================== */
/* COMPREHENSIVE STEP GENERATOR (100% 1-TO-1 SYNC WITH CODE TAGS)      */
/* ==================================================================== */
function buildSteps(approach, rawGrid) {
  const grid = rawGrid.map(row => [...row]);
  const m = grid.length;
  const n = grid[0].length;
  const steps = [];

  /* ------------------------------------------------------------------ */
  /* APPROACH 1: BRUTE FORCE RECURSION                                   */
  /* ------------------------------------------------------------------ */
  if (approach === 'recursion') {
    const staticTree = generateStaticTree('recursion', grid);
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
    let redCount = 0;
    const seenCoord = new Set();

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

    const frames = [{ name: 'main', args: `grid=${m}×${n}` }];

    function curSnap(extra = {}) {
      return {
        grid,
        m,
        n,
        curR: extra.curR !== undefined ? extra.curR : null,
        curC: extra.curC !== undefined ? extra.curC : null,
        totalCalls: callCounter,
        redundantCalls: redCount,
        cacheHits: 0,
        finalAns: extra.finalAns !== undefined ? extra.finalAns : null,
        currentReturn: extra.currentReturn !== undefined ? extra.currentReturn : null,
        activeNodeId: extra.activeNodeId !== undefined ? extra.activeNodeId : null,
        frames: frames.map(f => ({ ...f })),
        treeNodes: getVisibleNodes(),
        treeEdges: getVisibleEdges(),
        treeWidth: staticTree.width,
        treeHeight: staticTree.height,
        ...extra
      };
    }

    steps.push({ code: 'm_scanner', badge: 'Scanner sc = new Scanner(System.in);', badgeType: 'info', state: curSnap() });
    steps.push({ code: 'm_read_dims', badge: `int m = sc.nextInt(); int n = sc.nextInt(); &rarr; m = ${m}, n = ${n}`, badgeType: 'info', state: curSnap() });
    steps.push({ code: 'm_alloc_grid', badge: `int[][] grid = new int[${m}][${n}];`, badgeType: 'info', state: curSnap() });

    for (let r = 0; r < m; r++) {
      steps.push({ code: 'm_for_r', badge: `for (int r = 0; r < ${m}; r++) &rarr; r = ${r}`, badgeType: 'info', state: curSnap({ curR: r }) });
      for (let c = 0; c < n; c++) {
        steps.push({ code: 'm_for_c', badge: `for (int c = 0; c < ${n}; c++) &rarr; c = ${c}`, badgeType: 'info', state: curSnap({ curR: r, curC: c }) });
        steps.push({ code: 'm_read_cell', badge: `grid[${r}][${c}] = ${grid[r][c]};`, badgeType: 'info', state: curSnap({ curR: r, curC: c }) });
      }
    }

    steps.push({ code: 'm_call_fn', badge: `int ans = minCost(0, 0, grid, ${m}, ${n}); &mdash; launch top-down recursion`, badgeType: 'primary', state: curSnap({ curR: 0, curC: 0 }) });

    function simulateRec(nId, r, c) {
      const nd = nodeStateMap[nId];
      if (!nd) return Infinity;
      callCounter++;
      nd.state = 'active';

      const coordKey = `${r},${c}`;
      if (seenCoord.has(coordKey)) {
        redCount++;
        nd.isRedundant = true;
      } else {
        seenCoord.add(coordKey);
      }

      frames.push({ name: 'minCost', args: `r=${r}, c=${c}` });

      steps.push({ code: 'c_entry', badge: `Entering minCost(r = ${r}, c = ${c}) [Cost = ${r < m && c < n ? grid[r][c] : 'INF'}]`, badgeType: 'info', state: curSnap({ curR: r, curC: c, activeNodeId: nId }) });

      const isDest = (r === m - 1 && c === n - 1);
      steps.push({ code: 'c_base_dest', badge: isDest ? `Base check: if (r == ${m - 1} && c == ${n - 1}) &rarr; TRUE (Destination reached!)` : `Base check: if (r == ${m - 1} && c == ${n - 1}) &rarr; FALSE`, badgeType: isDest ? 'success' : 'info', state: curSnap({ curR: r, curC: c, activeNodeId: nId }) });

      if (isDest) {
        const destVal = grid[r][c];
        steps.push({ code: 'c_ret_dest', badge: `return grid[${r}][${c}] = ${destVal};`, badgeType: 'success', state: curSnap({ curR: r, curC: c, activeNodeId: nId, currentReturn: destVal }) });
        nd.state = 'solved'; nd.retVal = destVal; frames.pop(); return destVal;
      }

      const isOob = (r >= m || c >= n);
      steps.push({ code: 'c_base_oob', badge: isOob ? `Bounds check: if (r >= ${m} || c >= ${n}) &rarr; TRUE (Out of bounds &rarr; invalid path)` : `Bounds check: if (r >= ${m} || c >= ${n}) &rarr; FALSE (Inside grid)`, badgeType: isOob ? 'warn' : 'info', state: curSnap({ curR: r, curC: c, activeNodeId: nId }) });

      if (isOob) {
        steps.push({ code: 'c_ret_oob', badge: 'return Integer.MAX_VALUE; (Boundary exceeded &rarr; invalid path)', badgeType: 'warn', state: curSnap({ curR: r, curC: c, activeNodeId: nId, currentReturn: 'INF' }) });
        nd.state = 'solved'; nd.retVal = Infinity; frames.pop(); return Infinity;
      }

      steps.push({ code: 'c_call_down', badge: `int down = minCost(r + 1 = ${r + 1}, c = ${c}, grid, m, n); &mdash; branch Down`, badgeType: 'info', state: curSnap({ curR: r, curC: c, activeNodeId: nId }) });
      const downVal = simulateRec(nd.children[0], r + 1, c);
      steps.push({ code: 'c_call_right', badge: `int right = minCost(r = ${r}, c + 1 = ${c + 1}, grid, m, n); &mdash; branch Right`, badgeType: 'info', state: curSnap({ curR: r, curC: c, activeNodeId: nId }) });
      const rightVal = simulateRec(nd.children[1], r, c + 1);

      const minSub = Math.min(downVal, rightVal);
      steps.push({ code: 'c_calc_min', badge: `int minSub = Math.min(down=${downVal === Infinity ? 'INF' : downVal}, right=${rightVal === Infinity ? 'INF' : rightVal}) = ${minSub === Infinity ? 'INF' : minSub};`, badgeType: 'warn', state: curSnap({ curR: r, curC: c, activeNodeId: nId }) });
      const totalCost = grid[r][c] + (minSub === Infinity ? 0 : minSub);
      steps.push({ code: 'c_ret_min', badge: `return grid[${r}][${c}] (${grid[r][c]}) + minSub (${minSub === Infinity ? 'INF' : minSub}) = ${minSub === Infinity ? 'INF' : totalCost};`, badgeType: 'success', state: curSnap({ curR: r, curC: c, activeNodeId: nId, currentReturn: minSub === Infinity ? 'INF' : totalCost }) });

      nd.state = 'solved'; nd.retVal = minSub === Infinity ? Infinity : totalCost; frames.pop(); return minSub === Infinity ? Infinity : totalCost;
    }
    const finalAns = simulateRec(1, 0, 0);
    steps.push({ code: 'm_print', badge: `System.out.println(ans = ${finalAns}); &mdash; Optimal Min Path Sum computed!`, badgeType: 'success', state: curSnap({ finalAns }) });
    steps.push({ code: 'm_done', badge: 'Program execution completed successfully.', badgeType: 'info', state: curSnap({ finalAns }) });
  }

  /* ------------------------------------------------------------------ */
  /* APPROACH 2: MEMOIZATION                                            */
  /* ------------------------------------------------------------------ */
  else if (approach === 'memoization') {
    const staticTree = generateStaticTree('memoization', grid);
    const nodeStateMap = {};
    staticTree.nodes.forEach(nd => {
      nodeStateMap[nd.id] = { ...nd, x: staticTree.layoutMap[nd.id].x, y: staticTree.layoutMap[nd.id].y, state: 'hidden', retVal: null };
    });

    let callCounter = 0; let cacheHits = 0;
    const memo = Array.from({ length: m }, () => new Array(n).fill(-1));

    function getVisibleNodes() { return Object.values(nodeStateMap).filter(nd => nd.state !== 'hidden').map(nd => ({ ...nd })); }
    function getVisibleEdges() { return staticTree.edges.filter(e => nodeStateMap[e.from].state !== 'hidden' && nodeStateMap[e.to].state !== 'hidden').map(e => ({ ...e, x1: staticTree.layoutMap[e.from].x, y1: staticTree.layoutMap[e.from].y + 12, x2: staticTree.layoutMap[e.to].x, y2: staticTree.layoutMap[e.to].y - 12 })); }

    const frames = [{ name: 'main', args: `grid=${m}×${n}` }];
    function curSnap(extra = {}) { return { grid, m, n, curR: extra.curR !== undefined ? extra.curR : null, curC: extra.curC !== undefined ? extra.curC : null, totalCalls: callCounter, redundantCalls: 0, cacheHits, finalAns: extra.finalAns !== undefined ? extra.finalAns : null, currentReturn: extra.currentReturn !== undefined ? extra.currentReturn : null, activeNodeId: extra.activeNodeId !== undefined ? extra.activeNodeId : null, frames: frames.map(f => ({ ...f })), memoGrid: memo.map(row => [...row]), treeNodes: getVisibleNodes(), treeEdges: getVisibleEdges(), treeWidth: staticTree.width, treeHeight: staticTree.height, ...extra }; }

    steps.push({ code: 'm_scanner', badge: 'Scanner sc = new Scanner(System.in);', badgeType: 'info', state: curSnap() });
    steps.push({ code: 'm_read_dims', badge: `int m = sc.nextInt(); int n = sc.nextInt(); &rarr; m = ${m}, n = ${n}`, badgeType: 'info', state: curSnap() });
    steps.push({ code: 'm_alloc_grid', badge: `int[][] grid = new int[${m}][${n}];`, badgeType: 'info', state: curSnap() });

    for (let r = 0; r < m; r++) {
      steps.push({ code: 'm_for_r', badge: `for (int r = 0; r < ${m}; r++) &rarr; r = ${r}`, badgeType: 'info', state: curSnap({ curR: r }) });
      for (let c = 0; c < n; c++) {
        steps.push({ code: 'm_for_c', badge: `for (int c = 0; c < ${n}; c++) &rarr; c = ${c}`, badgeType: 'info', state: curSnap({ curR: r, curC: c }) });
        steps.push({ code: 'm_read_cell', badge: `grid[${r}][${c}] = ${grid[r][c]};`, badgeType: 'info', state: curSnap({ curR: r, curC: c }) });
      }
    }
    steps.push({ code: 'm_alloc_memo', badge: `int[][] memo = new int[${m}][${n}]; &mdash; allocate 2D memo table`, badgeType: 'info', state: curSnap() });
    steps.push({ code: 'm_fill_memo', badge: 'for (int[] row : memo) { Arrays.fill(row, -1); } &mdash; reset cache cells to -1', badgeType: 'info', state: curSnap() });
    steps.push({ code: 'm_call_fn', badge: `int ans = solve(0, 0, grid, ${m}, ${n}, memo); &mdash; start top-down memoized search`, badgeType: 'primary', state: curSnap({ curR: 0, curC: 0 }) });

    function simulateMemo(nId, r, c) {
      const nd = nodeStateMap[nId];
      if (!nd) return Infinity;
      callCounter++; nd.state = 'active'; frames.push({ name: 'solve', args: `r=${r}, c=${c}` });
      steps.push({ code: 'c_entry', badge: `Entering solve(r = ${r}, c = ${c}) [Cell Cost = ${r < m && c < n ? grid[r][c] : 'INF'}]`, badgeType: 'info', state: curSnap({ curR: r, curC: c, activeNodeId: nId }) });
      const isDest = (r === m - 1 && c === n - 1);
      steps.push({ code: 'c_base_dest', badge: isDest ? `Base check: if (r == ${m - 1} && c == ${n - 1}) &rarr; TRUE (Destination reached!)` : `Base check: if (r == ${m - 1} && c == ${n - 1}) &rarr; FALSE`, badgeType: isDest ? 'success' : 'info', state: curSnap({ curR: r, curC: c, activeNodeId: nId }) });
      if (isDest) { const destVal = grid[r][c]; memo[r][c] = destVal; steps.push({ code: 'c_ret_dest', badge: `return grid[${r}][${c}] = ${destVal};`, badgeType: 'success', state: curSnap({ curR: r, curC: c, activeNodeId: nId, currentReturn: destVal }) }); nd.state = 'solved'; nd.retVal = destVal; frames.pop(); return destVal; }
      const isOob = (r >= m || c >= n);
      steps.push({ code: 'c_base_oob', badge: isOob ? `Bounds check: if (r >= ${m} || c >= ${n}) &rarr; TRUE (Boundary exceeded)` : `Bounds check: if (r >= ${m} || c >= ${n}) &rarr; FALSE (Inside grid)`, badgeType: isOob ? 'warn' : 'info', state: curSnap({ curR: r, curC: c, activeNodeId: nId }) });
      if (isOob) { steps.push({ code: 'c_ret_oob', badge: 'return Integer.MAX_VALUE; (Boundary exceeded &rarr; invalid path)', badgeType: 'warn', state: curSnap({ curR: r, curC: c, activeNodeId: nId, currentReturn: 'INF' }) }); nd.state = 'solved'; nd.retVal = Infinity; frames.pop(); return Infinity; }
      const isCached = memo[r][c] !== -1;
      steps.push({ code: 'c_memo_chk', badge: isCached ? `Cache lookup: if (memo[${r}][${c}] != -1) &rarr; TRUE! Cached value = ${memo[r][c]}` : `Cache lookup: if (memo[${r}][${c}] != -1) &rarr; FALSE (Uncalculated subproblem)`, badgeType: isCached ? 'success' : 'info', state: curSnap({ curR: r, curC: c, activeNodeId: nId }) });
      if (isCached) { cacheHits++; nd.state = 'cache-hit'; nd.retVal = memo[r][c]; steps.push({ code: 'c_ret_cache', badge: `⚡ Cache Hit! return memo[${r}][${c}] = ${memo[r][c]} in O(1) time`, badgeType: 'success', state: curSnap({ curR: r, curC: c, activeNodeId: nId, currentReturn: memo[r][c] }) }); frames.pop(); return memo[r][c]; }
      steps.push({ code: 'c_call_down', badge: `int down = solve(r + 1 = ${r + 1}, c = ${c}, grid, m, n, memo); &mdash; branch Down`, badgeType: 'info', state: curSnap({ curR: r, curC: c, activeNodeId: nId }) });
      const downVal = simulateMemo(nd.children[0], r + 1, c);
      steps.push({ code: 'c_call_right', badge: `int right = solve(r = ${r}, c + 1 = ${c + 1}, grid, m, n, memo); &mdash; branch Right`, badgeType: 'info', state: curSnap({ curR: r, curC: c, activeNodeId: nId }) });
      const rightVal = simulateMemo(nd.children[1], r, c + 1);
      const minSub = Math.min(downVal, rightVal);
      steps.push({ code: 'c_calc_min', badge: `int minSub = Math.min(down=${downVal === Infinity ? 'INF' : downVal}, right=${rightVal === Infinity ? 'INF' : rightVal}) = ${minSub === Infinity ? 'INF' : minSub};`, badgeType: 'warn', state: curSnap({ curR: r, curC: c, activeNodeId: nId }) });
      const totalCost = grid[r][c] + (minSub === Infinity ? 0 : minSub);
      memo[r][c] = minSub === Infinity ? Infinity : totalCost;
      steps.push({ code: 'c_store_memo', badge: `memo[${r}][${c}] = grid[${r}][${c}] (${grid[r][c]}) + minSub (${minSub === Infinity ? 'INF' : minSub}) = ${memo[r][c]};`, badgeType: 'primary', state: curSnap({ curR: r, curC: c, activeNodeId: nId }) });
      steps.push({ code: 'c_ret_memo', badge: `return memo[${r}][${c}] = ${memo[r][c]};`, badgeType: 'success', state: curSnap({ curR: r, curC: c, activeNodeId: nId, currentReturn: memo[r][c] }) });
      nd.state = 'solved'; nd.retVal = memo[r][c]; frames.pop(); return memo[r][c];
    }
    const finalAns = simulateMemo(1, 0, 0);
    steps.push({ code: 'm_print', badge: `System.out.println(ans = ${finalAns}); &mdash; Optimal Min Path Sum computed!`, badgeType: 'success', state: curSnap({ finalAns }) });
    steps.push({ code: 'm_done', badge: 'Program execution completed successfully.', badgeType: 'info', state: curSnap({ finalAns }) });
  }

  /* ------------------------------------------------------------------ */
  /* APPROACH 3: TABULATION (DYNAMIC PROGRAMMING 2D TABLE)              */
  /* ------------------------------------------------------------------ */
  else {
    const dp = Array.from({ length: m }, () => new Array(n).fill('?'));
    const baseState = { grid, m, n, dpGrid: dp.map(row => [...row]), curR: null, curC: null, sourceTop: null, sourceLeft: null, calcFormula: '', tracebackPath: [], frames: [{ name: 'main', args: `grid=${m}×${n}` }], totalCalls: 0, redundantCalls: 0, cacheHits: 0, currentReturn: null, finalAns: null, treeNodes: [], treeEdges: [] };

    steps.push({ code: 'm_scanner', badge: 'Scanner sc = new Scanner(System.in);', badgeType: 'info', state: { ...baseState } });
    steps.push({ code: 'm_read_dims', badge: `int m = sc.nextInt(); int n = sc.nextInt(); &rarr; m = ${m}, n = ${n}`, badgeType: 'info', state: { ...baseState } });
    steps.push({ code: 'm_alloc_grid', badge: `int[][] grid = new int[${m}][${n}];`, badgeType: 'info', state: { ...baseState } });

    for (let r = 0; r < m; r++) {
      steps.push({ code: 'm_for_r', badge: `for (int r = 0; r < ${m}; r++) &rarr; r = ${r}`, badgeType: 'info', state: { ...baseState, curR: r } });
      for (let c = 0; c < n; c++) {
        steps.push({ code: 'm_for_c', badge: `for (int c = 0; c < ${n}; c++) &rarr; c = ${c}`, badgeType: 'info', state: { ...baseState, curR: r, curC: c } });
        steps.push({ code: 'm_read_cell', badge: `grid[${r}][${c}] = ${grid[r][c]};`, badgeType: 'info', state: { ...baseState, curR: r, curC: c } });
      }
    }
    steps.push({ code: 'm_call_fn', badge: 'int ans = minPathSum(grid); &mdash; call iterative 2D DP fill', badgeType: 'primary', state: { ...baseState } });
    steps.push({ code: 'c_entry', badge: 'static int minPathSum(int[][] grid) { &mdash; enter DP function', badgeType: 'primary', state: { ...baseState, frames: [{ name: 'minPathSum', args: `grid=${m}×${n}` }] } });
    steps.push({ code: 'c_get_dims', badge: `int m = grid.length, n = grid[0].length; &rarr; m = ${m}, n = ${n}`, badgeType: 'info', state: { ...baseState, frames: [{ name: 'minPathSum', args: `m=${m}, n=${n}` }] } });
    steps.push({ code: 'c_alloc_dp', badge: `int[][] dp = new int[${m}][${n}]; &mdash; allocate 2D DP matrix`, badgeType: 'info', state: { ...baseState, dpGrid: dp.map(row => [...row]), frames: [{ name: 'minPathSum', args: `m=${m}, n=${n}` }] } });

    for (let r = 0; r < m; r++) {
      steps.push({ code: 'c_loop_r', badge: `for (int r = 0; r < ${m}; r++) &rarr; row r = ${r}`, badgeType: 'primary', state: { ...baseState, dpGrid: dp.map(row => [...row]), curR: r, frames: [{ name: 'minPathSum', args: `r=${r}` }] } });
      for (let c = 0; c < n; c++) {
        steps.push({ code: 'c_loop_c', badge: `for (int c = 0; c < ${n}; c++) &rarr; cell (${r}, ${c})`, badgeType: 'primary', state: { ...baseState, dpGrid: dp.map(row => [...row]), curR: r, curC: c, frames: [{ name: 'minPathSum', args: `r=${r}, c=${c}` }] } });
        steps.push({ code: 'c_check_origin', badge: `if (r == 0 && c == 0) &rarr; ${r === 0 && c === 0 ? 'TRUE (Origin starting cell)' : 'FALSE'}`, badgeType: r === 0 && c === 0 ? 'success' : 'info', state: { ...baseState, dpGrid: dp.map(row => [...row]), curR: r, curC: c, frames: [{ name: 'minPathSum', args: `r=${r}, c=${c}` }] } });
        if (r === 0 && c === 0) { dp[0][0] = grid[0][0]; steps.push({ code: 'c_set_origin', badge: `dp[0][0] = grid[0][0] = ${grid[0][0]};`, badgeType: 'success', state: { ...baseState, dpGrid: dp.map(row => [...row]), curR: 0, curC: 0, frames: [{ name: 'minPathSum', args: 'r=0, c=0' }], calcFormula: `dp[0][0] = grid[0][0] = ${grid[0][0]}` } }); continue; }
        steps.push({ code: 'c_check_first_row', badge: `} else if (r == 0) &rarr; ${r === 0 ? 'TRUE (Can only arrive from LEFT)' : 'FALSE'}`, badgeType: r === 0 ? 'warn' : 'info', state: { ...baseState, dpGrid: dp.map(row => [...row]), curR: r, curC: c, sourceLeft: [r, c - 1], frames: [{ name: 'minPathSum', args: `r=${r}, c=${c}` }] } });
        if (r === 0) { const leftVal = dp[0][c - 1]; dp[0][c] = leftVal + grid[0][c]; steps.push({ code: 'c_set_first_row', badge: `dp[0][${c}] = dp[0][${c - 1}] (${leftVal}) + grid[0][${c}] (${grid[0][c]}) = ${dp[0][c]};`, badgeType: 'success', state: { ...baseState, dpGrid: dp.map(row => [...row]), curR: 0, curC: c, sourceLeft: [0, c - 1], frames: [{ name: 'minPathSum', args: `r=0, c=${c}` }], calcFormula: `dp[0][${c}] = dp[0][${c - 1}] (${leftVal}) + grid[0][${c}] (${grid[0][c]}) = ${dp[0][c]}` } }); continue; }
        steps.push({ code: 'c_check_first_col', badge: `} else if (c == 0) &rarr; ${c === 0 ? 'TRUE (Can only arrive from TOP)' : 'FALSE'}`, badgeType: c === 0 ? 'warn' : 'info', state: { ...baseState, dpGrid: dp.map(row => [...row]), curR: r, curC: c, sourceTop: [r - 1, c], frames: [{ name: 'minPathSum', args: `r=${r}, c=${c}` }] } });
        if (c === 0) { const topVal = dp[r - 1][0]; dp[r][0] = topVal + grid[r][0]; steps.push({ code: 'c_set_first_col', badge: `dp[${r}][0] = dp[${r - 1}][0] (${topVal}) + grid[${r}][0] (${grid[r][0]}) = ${dp[r][0]};`, badgeType: 'success', state: { ...baseState, dpGrid: dp.map(row => [...row]), curR: r, curC: 0, sourceTop: [r - 1, 0], frames: [{ name: 'minPathSum', args: `r=${r}, c=0` }], calcFormula: `dp[${r}][0] = dp[${r - 1}][0] (${topVal}) + grid[${r}][0] (${grid[r][0]}) = ${dp[r][0]}` } }); continue; }
        steps.push({ code: 'c_else_gen', badge: `} else { &mdash; general cell (${r}, ${c}) can arrive from Top (${r - 1}, ${c}) or Left (${r}, ${c - 1})`, badgeType: 'primary', state: { ...baseState, dpGrid: dp.map(row => [...row]), curR: r, curC: c, sourceTop: [r - 1, c], sourceLeft: [r, c - 1], frames: [{ name: 'minPathSum', args: `r=${r}, c=${c}` }] } });
        const topVal = dp[r - 1][c], leftVal = dp[r][c - 1], minNeighbor = Math.min(topVal, leftVal);
        steps.push({ code: 'c_calc_min', badge: `int minNeighbor = Math.min(top=${topVal}, left=${leftVal}) = ${minNeighbor};`, badgeType: 'warn', state: { ...baseState, dpGrid: dp.map(row => [...row]), curR: r, curC: c, sourceTop: [r - 1, c], sourceLeft: [r, c - 1], frames: [{ name: 'minPathSum', args: `r=${r}, c=${c}` }] } });
        dp[r][c] = grid[r][c] + minNeighbor;
        steps.push({ code: 'c_set_gen', badge: `dp[${r}][${c}] = grid[${r}][${c}] (${grid[r][c]}) + ${minNeighbor} = ${dp[r][c]};`, badgeType: 'success', state: { ...baseState, dpGrid: dp.map(row => [...row]), curR: r, curC: c, sourceTop: [r - 1, c], sourceLeft: [r, c - 1], frames: [{ name: 'minPathSum', args: `r=${r}, c=${c}` }], calcFormula: `dp[${r}][${c}] = grid[${r}][${c}] (${grid[r][c]}) + min(top=${topVal}, left=${leftVal}) = ${dp[r][c]}` } });
      }
    }
    const path = []; let tr = m - 1, tc = n - 1; while (tr >= 0 && tc >= 0) { path.unshift([tr, tc]); if (tr === 0 && tc === 0) break; if (tr === 0) tc--; else if (tc === 0) tr--; else { if (dp[tr - 1][tc] < dp[tr][tc - 1]) tr--; else tc--; } }
    steps.push({ code: 'c_ret_dp', badge: `return dp[${m - 1}][${n - 1}] = ${dp[m - 1][n - 1]}; &mdash; optimal path reconstructed!`, badgeType: 'success', state: { ...baseState, dpGrid: dp.map(row => [...row]), curR: m - 1, curC: n - 1, tracebackPath: path, finalAns: dp[m - 1][n - 1], frames: [{ name: 'minPathSum', args: `ans=${dp[m - 1][n - 1]}` }] } });
    steps.push({ code: 'm_print', badge: `System.out.println(ans = ${dp[m - 1][n - 1]});`, badgeType: 'success', state: { ...baseState, dpGrid: dp.map(row => [...row]), tracebackPath: path, finalAns: dp[m - 1][n - 1], frames: [] } });
    steps.push({ code: 'm_done', badge: 'Program execution completed successfully.', badgeType: 'info', state: { ...baseState, dpGrid: dp.map(row => [...row]), tracebackPath: path, finalAns: dp[m - 1][n - 1], frames: [] } });
  }
  return steps;
}

/* ==================================================================== */
/* COMPONENT STATE & REACTIVE CONTROLS                                  */
const inputGridStr = ref('[[1,2,3],[4,5,6]]');
const activeGrid = ref([[1,2,3],[4,5,6]]);

const si = ref(0);
const playing = ref(false);
const speed = ref(1500);

// Limit modal state
const showWarningModal = ref(false);
const warningModalTitle = ref('Input Constraint Notice');
const warningModalMsg = ref('');
const warningModalLimit = ref('4×4 Grid');
const requestedLength = ref('0');

// Resizing splitters matching CoinChangeDP.vue
const leftWidth = ref(52);
const vizHeight = ref(240);
const tableHeight = ref(48);

const mainRef = ref(null);
const leftColRef = ref(null);
const hResizerRef = ref(null);
const vizResizerRef = ref(null);
const tableResizerRef = ref(null);

const stepsData = reactive({
  steps: buildSteps('tabulation', [[1,2,3],[4,5,6]])
});
const steps = computed(() => stepsData.steps);

function parseInputGrid(str) {
  try {
    const trimmed = str.trim();
    let parsed;
    if (trimmed.startsWith('[')) {
      parsed = JSON.parse(trimmed);
    } else {
      parsed = trimmed.split(';').map(rowStr => {
        return rowStr.split(',').map(s => parseInt(s.trim(), 10)).filter(n => !isNaN(n));
      });
    }

    if (!Array.isArray(parsed) || parsed.length === 0 || !Array.isArray(parsed[0]) || parsed[0].length === 0) {
      return null;
    }

    const m = parsed.length;
    const n = parsed[0].length;
    for (let r = 0; r < m; r++) {
      if (!Array.isArray(parsed[r]) || parsed[r].length !== n) return null;
      for (let c = 0; c < n; c++) {
        if (typeof parsed[r][c] !== 'number' || isNaN(parsed[r][c]) || parsed[r][c] < 0) return null;
      }
    }
    return parsed;
  } catch {
    return null;
  }
}

function applyInput() {
  playing.value = false;
  clearTimeout(playTimer);
  const parsed = parseInputGrid(inputGridStr.value);
  if (!parsed) {
    requestedLength.value = 'Invalid';
    warningModalMsg.value = 'Invalid grid input. Please provide a valid 2D array of non-negative integers, e.g. [[1,2,3],[4,5,6]].';
    showWarningModal.value = true;
    return;
  }

  const m = parsed.length;
  const n = parsed[0].length;

  if (m > 4 || n > 4) {
    requestedLength.value = `${m}×${n}`;
    warningModalMsg.value = 'Grid size exceeds limit (max 4×4 for optimal visualization). Please enter a grid of size 4×4 or smaller.';
    showWarningModal.value = true;
    return;
  }

  if (currentApproach.value === 'recursion' && (m > 3 || n > 3)) {
    requestedLength.value = `${m}×${n}`;
    warningModalMsg.value = 'Unmemoized Recursion branches O(2^(m+n)). Dimensions are limited to 3×3 for pure recursion visualization.';
    showWarningModal.value = true;
    return;
  }

  activeGrid.value = parsed;
  recomputeSteps();
}

function applyApproach(newApproach) {
  currentApproach.value = newApproach;
  applyInput();
}

function closeWarningModal() {
  showWarningModal.value = false;
}

function recomputeSteps() {
  stepsData.steps = buildSteps(currentApproach.value, activeGrid.value);
  si.value = 0;
}

/* ==================================================================== */
/* PLAYBACK ENGINE                                                      */
/* ==================================================================== */
let playTimer = null;

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

function stepBy(delta) {
  playing.value = false;
  clearTimeout(playTimer);
  const next = si.value + delta;
  si.value = Math.max(0, Math.min(steps.value.length - 1, next));
}

function onKeydown(e) {
  if (e.target.tagName === 'INPUT') return;
  if (e.key === 'ArrowRight') stepBy(1);
  else if (e.key === 'ArrowLeft') stepBy(-1);
  else if (e.key === ' ') {
    e.preventDefault();
    togglePlay();
  }
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
  cleanupFns.push(initVResizer(vizResizerRef, vizHeight, 140, 380));
  cleanupFns.push(initVResizer(tableResizerRef, tableHeight, 30, 140));
  recomputeSteps();
});

onUnmounted(() => {
  document.removeEventListener('keydown', onKeydown);
  clearTimeout(playTimer);
  cleanupFns.forEach(fn => fn && fn());
});

/* ==================================================================== */
/* DERIVED CURRENT STATE                                                */
/* ==================================================================== */
const s = computed(() => {
  if (!steps.value.length) return {};
  return steps.value[Math.max(0, Math.min(si.value, steps.value.length - 1))] || { state: {} };
});

const st = computed(() => s.value.state || {});

const codeLines = computed(() => {
  const appCodes = CODES[currentApproach.value] || CODES.tabulation;
  return appCodes[lang.value] || appCodes.java || [];
});

const pseudocodeLines = computed(() => {
  return PSEUDOCODES[currentApproach.value] || PSEUDOCODES.tabulation;
});

function isTraceback(r, c) {
  if (!st.value.tracebackPath) return false;
  return st.value.tracebackPath.some(([pr, pc]) => pr === r && pc === c);
}

function isSourceTop(r, c) {
  return st.value.sourceTop && st.value.sourceTop[0] === r && st.value.sourceTop[1] === c;
}

function isSourceLeft(r, c) {
  return st.value.sourceLeft && st.value.sourceLeft[0] === r && st.value.sourceLeft[1] === c;
}

function isCurrentCell(r, c) {
  return st.value.curR === r && st.value.curC === c;
}
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

            <!-- Custom Input: grid -->
            <div class="ll-input-group">
              <label>grid =</label>
              <input
                type="text"
                v-model="inputGridStr"
                class="ll-text-input"
                style="width: 170px;"
                placeholder="[[1,2,3],[4,5,6]]"
                @keyup.enter="applyInput"
              />
              <span class="ll-input-hint">(max 4×4)</span>
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
              <!-- Top Grid Preview Strip -->
              <div class="ll-house-banner">
                <div class="ll-house-title">
                  Grid Matrix: <code>grid[{{ activeGrid.length }} &times; {{ activeGrid[0].length }}]</code>
                  <span v-if="st.finalAns !== null && st.finalAns !== undefined">
                    &bull; Optimal Min Path Sum = <b>{{ st.finalAns }}</b>
                  </span>
                </div>
                <div class="ll-house-strip">
                  <div
                    v-for="(row, rIdx) in activeGrid"
                    :key="'r-' + rIdx"
                    class="ll-strip-row"
                  >
                    <div
                      v-for="(val, cIdx) in row"
                      :key="'c-' + cIdx"
                      class="ll-house-card"
                      :class="{
                        'll-house-cur': isCurrentCell(rIdx, cIdx),
                        'll-house-compare': isTraceback(rIdx, cIdx)
                      }"
                    >
                      <div class="ll-house-val">{{ val }}</div>
                      <div class="ll-house-idx">
                        <span v-if="isCurrentCell(rIdx, cIdx)" class="ll-idx-tag ll-tag-cur">cur</span>
                        <span v-else-if="isTraceback(rIdx, cIdx)" class="ll-idx-tag ll-tag-comp">&starf;</span>
                        <span v-else>[{{ rIdx }},{{ cIdx }}]</span>
                      </div>
                    </div>
                  </div>
                </div>
              </div>

              <!-- Primary Visualization Panel -->
              <div class="ll-viz-wrap" :style="{ height: vizHeight + 'px' }">
                <div class="ll-perm-area">
                  <!-- Metrics Chips Bar -->
                  <div class="ll-ptrs">
                    <div class="ll-ptr-chip">Grid = <b class="ll-c-blue">{{ activeGrid.length }} &times; {{ activeGrid[0].length }}</b></div>
                    <div class="ll-ptr-chip" v-if="st.curR !== null && st.curC !== null">
                      Cell: <b class="ll-c-orange">({{ st.curR }}, {{ st.curC }}) = {{ activeGrid[st.curR] ? activeGrid[st.curR][st.curC] : '?' }}</b>
                    </div>
                    <div class="ll-ptr-chip" v-if="currentApproach === 'recursion' || currentApproach === 'memoization'">
                      Calls: <b class="ll-c-orange">{{ st.totalCalls || 0 }}</b>
                    </div>
                    <div class="ll-ptr-chip" v-if="currentApproach === 'memoization'">
                      Cache Hits: <b class="ll-c-purple">{{ st.cacheHits || 0 }}</b>
                    </div>
                    <div class="ll-ptr-chip" v-if="currentApproach === 'recursion'">
                      Redundant: <b class="ll-c-red">{{ st.redundantCalls || 0 }}</b>
                    </div>
                    <div class="ll-ptr-chip" v-if="st.finalAns !== null && st.finalAns !== undefined">
                      Min Path Sum: <b class="ll-c-green">{{ st.finalAns }}</b>
                    </div>
                  </div>

                  <!-- Diagram 1: DP Tabulation (Bottom-Up 2D DP Table) -->
                  <div v-if="currentApproach === 'tabulation'" class="ll-dp-tab-view">
                    <div class="ll-section-caption">
                      <span>2D DP Table &mdash; <code>dp[r][c] = grid[r][c] + min(dp[r-1][c], dp[r][c-1])</code></span>
                      <span v-if="st.calcFormula" class="ll-calc-pill">{{ st.calcFormula }}</span>
                    </div>

                    <div class="ll-matrices-row">
                      <!-- Input Grid -->
                      <div class="ll-matrix-panel">
                        <div class="ll-matrix-sublabel">Input: grid[r][c]</div>
                        <table class="ll-matrix-table">
                          <thead>
                            <tr>
                              <th class="ll-th-corner">r\c</th>
                              <th v-for="(_, ci) in activeGrid[0]" :key="ci" class="ll-th-col">c={{ ci }}</th>
                            </tr>
                          </thead>
                          <tbody>
                            <tr v-for="(row, ri) in activeGrid" :key="ri">
                              <th class="ll-th-row">r={{ ri }}</th>
                              <td
                                v-for="(val, ci) in row"
                                :key="ci"
                                class="ll-arr-box"
                                :class="{
                                  'll-box-cur': isCurrentCell(ri, ci),
                                  'll-box-comp': isSourceTop(ri, ci) || isSourceLeft(ri, ci),
                                  'll-box-found': isTraceback(ri, ci)
                                }"
                              >
                                {{ val }}
                              </td>
                            </tr>
                          </tbody>
                        </table>
                      </div>

                      <!-- DP Table -->
                      <div class="ll-matrix-panel">
                        <div class="ll-matrix-sublabel">DP: dp[r][c]</div>
                        <table class="ll-matrix-table">
                          <thead>
                            <tr>
                              <th class="ll-th-corner">r\c</th>
                              <th v-for="(_, ci) in activeGrid[0]" :key="ci" class="ll-th-col">c={{ ci }}</th>
                            </tr>
                          </thead>
                          <tbody>
                            <tr v-for="(row, ri) in (st.dpGrid || [])" :key="ri">
                              <th class="ll-th-row">r={{ ri }}</th>
                              <td
                                v-for="(val, ci) in row"
                                :key="ci"
                                class="ll-arr-box"
                                :class="{
                                  'll-box-cur': isCurrentCell(ri, ci),
                                  'll-box-comp': isSourceTop(ri, ci) || isSourceLeft(ri, ci),
                                  'll-box-found': isTraceback(ri, ci),
                                  'll-box-uncalc': val === '?'
                                }"
                              >
                                {{ val }}
                              </td>
                            </tr>
                          </tbody>
                        </table>
                      </div>
                    </div>

                    <!-- Traceback Stream -->
                    <div v-if="st.tracebackPath && st.tracebackPath.length" class="ll-trace-stream">
                      <span class="ll-trace-stream-title">Optimal Path:</span>
                      <span
                        v-for="([pr, pc], idx) in st.tracebackPath"
                        :key="idx"
                        class="ll-trace-tag"
                      >
                        ({{ pr }},{{ pc }})[{{ activeGrid[pr][pc] }}]<span v-if="idx < st.tracebackPath.length - 1"> &rarr; </span>
                      </span>
                    </div>
                  </div>

                  <!-- Diagram 2: Recursion & Memoization Decision Tree -->
                  <div v-else class="ll-tree-container">
                    <div class="ll-section-caption">
                      <span>{{ currentApproach === 'recursion' ? 'Decision Tree O(2^(m+n))' : 'Pruned Memoization Decision Tree O(m×n)' }}</span>
                      <span v-if="currentApproach === 'memoization'" class="ll-memo-badge-info">Lookup Cache Active</span>
                    </div>

                    <div class="ll-tree-scroll-area">
                      <svg
                        :viewBox="`0 0 ${st.treeWidth || 540} ${st.treeHeight || 260}`"
                        :style="{
                          width: '100%',
                          maxWidth: (st.treeWidth || 540) + 'px',
                          height: (st.treeHeight || 260) + 'px',
                          maxHeight: (st.treeHeight || 260) + 'px'
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
                              :class="{ 'll-edge-take': edge.branch === 'D' }"
                            />
                            <text
                              :x="(edge.x1 + edge.x2) / 2 + (edge.branch === 'D' ? -14 : 12)"
                              :y="((edge.y1 + edge.y2) / 2)"
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
                              <rect x="-26" y="-6" width="52" height="12" rx="3" fill="#9333ea" stroke="#7e22ce" stroke-width="0.5" />
                              <text x="0" y="0" text-anchor="middle" class="ll-badge-text-purple">Cache Hit O(1)</text>
                            </g>
                            <g v-else-if="node.isDest" transform="translate(0, -17)">
                              <rect x="-25" y="-6" width="50" height="12" rx="3" fill="#10b981" stroke="#059669" stroke-width="0.5" />
                              <text x="0" y="0" text-anchor="middle" class="ll-badge-text-purple">Target (m-1,n-1)</text>
                            </g>
                            <g v-else-if="node.isRedundant" transform="translate(0, -17)">
                              <rect x="-25" y="-6" width="50" height="12" rx="3" fill="#ef4444" stroke="#dc2626" stroke-width="0.5" />
                              <text x="0" y="0" text-anchor="middle" class="ll-badge-text-purple">Recomputing!</text>
                            </g>

                            <!-- Node Outer Card -->
                            <rect
                              x="-30"
                              y="-12"
                              width="60"
                              height="24"
                              rx="4"
                              class="ll-node-rect"
                              :class="{
                                'll-node-active': node.id === st.activeNodeId,
                                'll-node-solved': node.state === 'solved',
                                'll-node-cachehit': node.state === 'cache-hit'
                              }"
                            />
                            <!-- Node Text: (r, c) -->
                            <text x="0" y="-3" text-anchor="middle" class="ll-node-text-call">
                              ({{ node.r }}, {{ node.c }})
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
                                node.retVal !== null && node.retVal !== undefined ? `= ${node.retVal === Infinity ? 'INF' : node.retVal}` :
                                (node.state === 'cache-hit' ? '⚡ Hit' : '?')
                              }}
                            </text>
                          </g>
                        </g>
                      </svg>

                      <!-- 2D Memoization Table Strip (Only in Memoization Mode) -->
                      <div v-if="currentApproach === 'memoization' && st.memoGrid" class="ll-memo-strip-wrap">
                        <div class="ll-memo-title">Memoization 2D Cache Table &mdash; <code>memo[r][c]</code>:</div>
                        <div class="ll-memo-grid">
                          <div v-for="(row, rIdx) in st.memoGrid" :key="rIdx" class="ll-memo-row">
                            <span class="ll-memo-row-lbl">r={{ rIdx }}:</span>
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
                              <span class="ll-memo-idx">c={{ cIdx }}</span>
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
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-cur"></span>Current Cell (r, c)</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-orange"></span>Neighbor Dependency (Top / Left)</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-new"></span>Optimal Path Traceback</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-uncalc"></span>Uncalculated (?)</span>
                </template>
                <template v-else-if="currentApproach === 'memoization'">
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-cur"></span>Active Call</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-new"></span>Solved &amp; Cached</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-purple"></span>Cache Hit (O(1) Return)</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-uncalc"></span>Uncalled</span>
                </template>
                <template v-else>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-cur"></span>Active Decision</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-new"></span>Destination (m-1, n-1)</span>
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
                  <h3>Why Dynamic Programming? (Minimum Path Sum)</h3>
                  <p>
                    The <strong>Minimum Path Sum</strong> problem (LeetCode 64) requires finding a path from <code>(0, 0)</code> to <code>(m-1, n-1)</code> with minimum sum.
                    Greedy choice (always choosing the smaller adjacent step) fails because a locally expensive cell may unlock much cheaper cells later.
                  </p>

                  <table class="ll-complexity-table">
                    <thead>
                      <tr>
                        <th>Algorithm</th>
                        <th>Time Complexity</th>
                        <th>Space Complexity</th>
                        <th>Verdict &amp; Notes</th>
                      </tr>
                    </thead>
                    <tbody>
                      <tr>
                        <td><strong>Brute-Force Recursion</strong></td>
                        <td>O(2<sup>m + n</sup>) Exponential</td>
                        <td>O(m + n) Stack</td>
                        <td>Explores both branches (Down, Right) repeatedly. Excessive recomputations.</td>
                      </tr>
                      <tr>
                        <td><strong>Memoization (Top-Down)</strong></td>
                        <td>O(m &times; n) Linear</td>
                        <td>O(m &times; n) Cache + Stack</td>
                        <td>Caches <code>memo[r][c]</code> on first visit; returns O(1) on subsequent visits.</td>
                      </tr>
                      <tr>
                        <td><strong>DP Tabulation (Bottom-Up)</strong></td>
                        <td>O(m &times; n) Iterative</td>
                        <td>O(m &times; n) or O(n)</td>
                        <td>Builds 2D table iteratively from origin to destination without recursion overhead.</td>
                      </tr>
                    </tbody>
                  </table>

                  <h3>Recurrence Formula</h3>
                  <p class="ll-math-box">
                    dp[r][c] = grid[r][c] + min( dp[r - 1][c], dp[r][c - 1] )
                  </p>
                  <p>
                    Base conditions: <code>dp[0][0] = grid[0][0]</code>, <code>dp[0][c] = dp[0][c - 1] + grid[0][c]</code>, and <code>dp[r][0] = dp[r - 1][0] + grid[r][0]</code>.
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
.navbar { display: flex; flex-direction: row; justify-content: space-between; align-items: center; gap: 0.75rem; padding: 0 10px; background-color: #ffffff; position: fixed; width: 94.7%; z-index: 100; }
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
.ll-text-input { background: var(--surface); border: 1px solid var(--border2); color: var(--text); border-radius: var(--radius-sm); padding: 3px 6px; font-size: 11px; font-family: monospace; }
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

/* Top Banner */
.ll-house-banner { padding: 4px 10px; background: var(--surface2); border-bottom: 1px solid var(--border); flex-shrink: 0; }
.ll-house-title { font-size: 10.5px; font-weight: 700; color: var(--text2); margin-bottom: 2px; }
.ll-house-strip { display: flex; flex-direction: column; gap: 2px; }
.ll-strip-row { display: flex; gap: 4px; }
.ll-house-card { width: 40px; height: 32px; display: flex; flex-direction: column; align-items: center; justify-content: center; background: var(--surface); border: 1.5px solid var(--border2); border-radius: var(--radius-sm); box-shadow: var(--shadow-sm); transition: all 0.2s ease; }
.ll-house-cur { border-color: #f59e0b !important; background: #fffbeb !important; transform: translateY(-1px); box-shadow: 0 0 0 3px rgba(245,158,11,0.25) !important; }
.ll-house-compare { border-color: #10b981 !important; background: #dcfce7 !important; transform: translateY(-1px); }
.ll-house-val { font-size: 11px; font-weight: 800; font-family: monospace; color: var(--text); }
.ll-house-idx { font-size: 8.5px; color: var(--muted); font-family: monospace; margin-top: 1px; }
.ll-idx-tag { font-weight: 800; padding: 0 2px; border-radius: 3px; }
.ll-tag-cur { color: #b45309; background: #fef3c7; }
.ll-tag-comp { color: #047857; background: #dcfce7; }

/* Left Visualization Wrappers */
.ll-viz-wrap { flex-shrink: 0; background: var(--surface); border-bottom: 1px solid var(--border); position: relative; overflow-x: auto; overflow-y: auto; }
.ll-perm-area { display: flex; flex-direction: column; align-items: stretch; min-height: 100%; width: 100%; min-width: 0; box-sizing: border-box; }
.ll-ptrs { display: flex; gap: 6px; flex-wrap: wrap; padding: 4px 10px; min-height: 28px; width: 100%; box-sizing: border-box; min-width: 0; }
.ll-ptr-chip { background: var(--surface2); border: 1px solid var(--border); border-radius: var(--radius-sm); padding: 2px 7px; font-size: 11px; font-family: monospace; box-shadow: var(--shadow-sm); white-space: nowrap; flex-shrink: 0; }

.ll-c-blue { color: var(--blue); } .ll-c-orange { color: var(--orange); } .ll-c-green { color: var(--green); } .ll-c-purple { color: var(--purple); } .ll-c-red { color: var(--red); }

.ll-section-caption { display: flex; justify-content: space-between; align-items: center; padding: 3px 10px; font-size: 10.5px; font-weight: 700; color: var(--text2); background: var(--surface2); border-top: 1px solid var(--border); border-bottom: 1px solid var(--border); }
.ll-calc-pill { background: #dcfce7; color: #15803d; padding: 1px 6px; border-radius: 10px; font-size: 10px; font-family: monospace; font-weight: 700; }
.ll-memo-badge-info { background: var(--purple-light); color: var(--purple); padding: 1px 6px; border-radius: 10px; font-size: 10px; }

/* Diagram 1: DP Tabulation 2D Table */
.ll-dp-tab-view { display: flex; flex-direction: column; padding: 4px 8px; }
.ll-matrices-row { display: flex; gap: 14px; justify-content: center; align-items: flex-start; flex-wrap: wrap; padding: 4px 2px; }
.ll-matrix-panel { display: flex; flex-direction: column; align-items: center; background: var(--surface2); padding: 6px 8px; border-radius: var(--radius-sm); border: 1px solid var(--border); box-shadow: var(--shadow-sm); }
.ll-matrix-sublabel { font-size: 10.5px; font-weight: 700; color: var(--text2); margin-bottom: 4px; font-family: monospace; }
.ll-matrix-table { border-collapse: separate; border-spacing: 3px; font-family: monospace; }
.ll-th-corner, .ll-th-col, .ll-th-row { font-size: 9.5px; color: var(--muted); font-weight: 600; padding: 1px 4px; text-align: center; }

.ll-matrix-table .ll-arr-box { width: 38px; height: 34px; font-size: 12px; }
.ll-arr-box { display: flex; align-items: center; justify-content: center; border: 1.5px solid var(--border2); border-radius: var(--radius-sm); background: var(--surface); color: var(--text); font-weight: 700; box-shadow: var(--shadow-sm); transition: all 0.2s ease; }
.ll-box-cur { border-color: #f59e0b !important; background: #fef3c7 !important; color: #92400e !important; box-shadow: 0 0 0 3px rgba(245, 158, 11, 0.25) !important; transform: translateY(-1px); }
.ll-box-comp { border-color: #a855f7 !important; background: #f3e8ff !important; color: #6b21a8 !important; transform: translateY(-1px); }
.ll-box-found { border-color: #10b981 !important; background: #dcfce7 !important; color: #065f46 !important; box-shadow: 0 0 0 3px rgba(16, 185, 129, 0.2) !important; }
.ll-box-uncalc { border: 1.5px dashed var(--border2) !important; background: var(--surface2) !important; color: var(--muted) !important; }

.ll-trace-stream { display: flex; align-items: center; gap: 4px; padding: 4px 8px; background: var(--surface2); border: 1px solid var(--border); border-radius: var(--radius-sm); margin-top: 4px; flex-wrap: wrap; }
.ll-trace-stream-title { font-size: 10px; font-weight: 700; color: var(--text2); }
.ll-trace-tag { font-size: 10px; font-family: monospace; color: #15803d; font-weight: 700; }

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
