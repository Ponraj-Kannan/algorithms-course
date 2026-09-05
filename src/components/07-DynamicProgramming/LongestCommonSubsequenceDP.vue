<script setup>
import { ref, computed, reactive, onMounted, onUnmounted, nextTick } from 'vue';

/* ==================================================================== */
/* PROPS & COMPONENT CONFIGURATION                                      */
/* ==================================================================== */
const props = defineProps({
  topic: { type: String, default: 'Dynamic Programming' },
  subTopic: { type: String, default: 'Longest Common Subsequence (LCS)' }
});

/* ==================================================================== */
/* APPROACHES & MULTI-LANGUAGE 100% EXECUTABLE CODE SPECIFICATIONS      */
/* ==================================================================== */
const APPROACHES = [
  { id: 'recursion', label: 'Brute Force', desc: 'O(2^(m+n)) Exponential — Match vs 2-Way Branch Decisions' },
  { id: 'memoization', label: 'Memoization', desc: 'O(m×n) Time & O(m×n) Space — Top-Down with 2D Subproblem Cache' },
  { id: 'tabulation', label: 'Tabulation', desc: 'O(m×n) Time & O(m×n) Space — Bottom-Up 2D DP Grid with Traceback' }
];

const PRESETS = [
  { label: 'AGGTAB & GXTXAYB', desc: 'Classic LCS = "GTAB" (Length 4)', s1: 'AGGTAB', s2: 'GXTXAYB' },
  { label: 'abcde & ace', desc: 'LCS = "ace" (Length 3)', s1: 'abcde', s2: 'ace' },
  { label: 'abc & abc', desc: 'Exact Match (Length 3)', s1: 'abc', s2: 'abc' },
  { label: 'abc & def', desc: 'No Common Subsequence (Length 0)', s1: 'abc', s2: 'def' },
  { label: 'stone & longest', desc: 'LCS = "one" (Length 3)', s1: 'stone', s2: 'longest' },
  { label: 'fish & fosh', desc: 'LCS = "fsh" (Length 3)', s1: 'fish', s2: 'fosh' }
];

const CODES = {
  recursion: {
    java: [
      ['',             'import java.util.Scanner;'],
      ['',             ''],
      ['',             'public class Main {'],
      ['c_entry',      '    static int lcs(String s1, String s2, int i, int j) {'],
      ['c_base',       '        if (i == 0 || j == 0) return 0;'],
      ['c_match_chk',  '        if (s1.charAt(i - 1) == s2.charAt(j - 1)) {'],
      ['c_match_rec',  '            return 1 + lcs(s1, s2, i - 1, j - 1);'],
      ['c_nomatch',    '        } else {'],
      ['c_nomatch_rec','            int skip1 = lcs(s1, s2, i - 1, j);'],
      ['',             '            int skip2 = lcs(s1, s2, i, j - 1);'],
      ['c_ret_max',    '            return Math.max(skip1, skip2);'],
      ['',             '        }'],
      ['',             '    }'],
      ['',             ''],
      ['',             '    public static void main(String[] args) {'],
      ['m_scanner',    '        Scanner sc = new Scanner(System.in);'],
      ['m_read_s1',    '        String s1 = sc.next();'],
      ['m_read_s2',    '        String s2 = sc.next();'],
      ['m_call_lcs',   '        int ans = lcs(s1, s2, s1.length(), s2.length());'],
      ['m_print',      '        System.out.println(ans);'],
      ['m_done',       '    }'],
      ['',             '}']
    ],
    c: [
      ['',             '#include <stdio.h>'],
      ['',             '#include <string.h>'],
      ['',             '#define MAX(a, b) ((a) > (b) ? (a) : (b))'],
      ['',             ''],
      ['c_entry',      'int lcs(const char* s1, const char* s2, int i, int j) {'],
      ['c_base',       '    if (i == 0 || j == 0) return 0;'],
      ['c_match_chk',  '    if (s1[i - 1] == s2[j - 1]) {'],
      ['c_match_rec',  '        return 1 + lcs(s1, s2, i - 1, j - 1);'],
      ['c_nomatch',    '    } else {'],
      ['c_nomatch_rec','        int skip1 = lcs(s1, s2, i - 1, j);'],
      ['',             '        int skip2 = lcs(s1, s2, i, j - 1);'],
      ['c_ret_max',    '        return MAX(skip1, skip2);'],
      ['',             '    }'],
      ['',             '}'],
      ['',             ''],
      ['int main() {'],
      ['m_read_s1',    '    char s1[100], s2[100]; scanf("%s %s", s1, s2);'],
      ['m_call_lcs',   '    int ans = lcs(s1, s2, strlen(s1), strlen(s2));'],
      ['m_print',      '    printf("%d\\n", ans);'],
      ['m_done',       '    return 0;'],
      ['',             '}']
    ],
    cpp: [
      ['',             '#include <iostream>'],
      ['',             '#include <string>'],
      ['',             '#include <algorithm>'],
      ['',             'using namespace std;'],
      ['',             ''],
      ['c_entry',      'int lcs(const string& s1, const string& s2, int i, int j) {'],
      ['c_base',       '    if (i == 0 || j == 0) return 0;'],
      ['c_match_chk',  '    if (s1[i - 1] == s2[j - 1]) {'],
      ['c_match_rec',  '        return 1 + lcs(s1, s2, i - 1, j - 1);'],
      ['c_nomatch',    '    } else {'],
      ['c_nomatch_rec','        return max(lcs(s1, s2, i - 1, j), lcs(s1, s2, i, j - 1));'],
      ['',             '    }'],
      ['',             '}'],
      ['',             ''],
      ['int main() {'],
      ['m_read_s1',    '    string s1, s2; cin >> s1 >> s2;'],
      ['m_call_lcs',   '    int ans = lcs(s1, s2, s1.size(), s2.size());'],
      ['m_print',      '    cout << ans << endl;'],
      ['m_done',       '    return 0;'],
      ['',             '}']
    ],
    python: [
      ['',             'import sys'],
      ['',             ''],
      ['c_entry',      'def lcs(s1, s2, i, j):'],
      ['c_base',       '    if i == 0 or j == 0:'],
      ['',             '        return 0'],
      ['c_match_chk',  '    if s1[i - 1] == s2[j - 1]:'],
      ['c_match_rec',  '        return 1 + lcs(s1, s2, i - 1, j - 1)'],
      ['c_nomatch',    '    else:'],
      ['c_nomatch_rec','        return max(lcs(s1, s2, i - 1, j), lcs(s1, s2, i, j - 1))'],
      ['',             ''],
      ['if __name__ == "__main__":'],
      ['m_read_s1',    '    data = sys.stdin.read().split()'],
      ['m_read_s2',    '    s1, s2 = data[0], data[1]'],
      ['m_call_lcs',   '    ans = lcs(s1, s2, len(s1), len(s2))'],
      ['m_print',      '    print(ans)'],
      ['m_done',       '    sys.exit(0)']
    ],
    javascript: [
      ['',             'const fs = require("fs");'],
      ['',             ''],
      ['c_entry',      'function lcs(s1, s2, i, j) {'],
      ['c_base',       '    if (i === 0 || j === 0) return 0;'],
      ['c_match_chk',  '    if (s1[i - 1] === s2[j - 1]) {'],
      ['c_match_rec',  '        return 1 + lcs(s1, s2, i - 1, j - 1);'],
      ['c_nomatch',    '    } else {'],
      ['c_nomatch_rec','        return Math.max(lcs(s1, s2, i - 1, j), lcs(s1, s2, i, j - 1));'],
      ['',             '    }'],
      ['',             '}'],
      ['',             ''],
      ['const input = fs.readFileSync("/dev/stdin", "utf-8").trim().split(/\\s+/);'],
      ['const [s1, s2] = input;'],
      ['const ans = lcs(s1, s2, s1.length, s2.length);'],
      ['console.log(ans);'],
      ['process.exit(0);']
    ]
  },
  memoization: {
    java: [
      ['',             'import java.util.Scanner;'],
      ['',             'import java.util.Arrays;'],
      ['',             ''],
      ['',             'public class Main {'],
      ['c_entry',      '    static int solve(String s1, String s2, int i, int j, int[][] memo) {'],
      ['c_base',       '        if (i == 0 || j == 0) return 0;'],
      ['c_memo_check', '        if (memo[i][j] != -1) return memo[i][j];'],
      ['c_match_chk',  '        if (s1.charAt(i - 1) == s2.charAt(j - 1)) {'],
      ['c_match_calc', '            memo[i][j] = 1 + solve(s1, s2, i - 1, j - 1, memo);'],
      ['c_nomatch',    '        } else {'],
      ['c_nomatch_calc','            memo[i][j] = Math.max(solve(s1, s2, i - 1, j, memo), solve(s1, s2, i, j - 1, memo));'],
      ['',             '        }'],
      ['c_ret_memo',   '        return memo[i][j];'],
      ['',             '    }'],
      ['',             ''],
      ['',             '    public static void main(String[] args) {'],
      ['m_scanner',    '        Scanner sc = new Scanner(System.in);'],
      ['m_read_s1',    '        String s1 = sc.next(); String s2 = sc.next();'],
      ['m_alloc_memo', '        int m = s1.length(), n = s2.length();'],
      ['',             '        int[][] memo = new int[m + 1][n + 1];'],
      ['m_fill_memo',  '        for (int[] row : memo) Arrays.fill(row, -1);'],
      ['m_call_lcs',   '        int ans = solve(s1, s2, m, n, memo);'],
      ['m_print',      '        System.out.println(ans);'],
      ['m_done',       '    }'],
      ['',             '}']
    ],
    c: [
      ['',             '#include <stdio.h>'],
      ['',             '#include <string.h>'],
      ['',             '#define MAX(a, b) ((a) > (b) ? (a) : (b))'],
      ['',             ''],
      ['c_entry',      'int solve(const char* s1, const char* s2, int i, int j, int memo[100][100]) {'],
      ['c_base',       '    if (i == 0 || j == 0) return 0;'],
      ['c_memo_check', '    if (memo[i][j] != -1) return memo[i][j];'],
      ['c_match_chk',  '    if (s1[i - 1] == s2[j - 1]) {'],
      ['c_match_calc', '        return memo[i][j] = 1 + solve(s1, s2, i - 1, j - 1, memo);'],
      ['c_nomatch',    '    } else {'],
      ['c_nomatch_calc','        return memo[i][j] = MAX(solve(s1, s2, i - 1, j, memo), solve(s1, s2, i, j - 1, memo));'],
      ['',             '    }'],
      ['',             '}'],
      ['',             ''],
      ['int main() {'],
      ['m_read_s1',    '    char s1[100], s2[100]; scanf("%s %s", s1, s2);'],
      ['m_alloc_memo', '    int memo[100][100]; memset(memo, -1, sizeof(memo));'],
      ['m_call_lcs',   '    int ans = solve(s1, s2, strlen(s1), strlen(s2), memo);'],
      ['m_print',      '    printf("%d\\n", ans);'],
      ['m_done',       '    return 0;'],
      ['',             '}']
    ],
    cpp: [
      ['',             '#include <iostream>'],
      ['',             '#include <vector>'],
      ['',             '#include <string>'],
      ['',             'using namespace std;'],
      ['',             ''],
      ['c_entry',      'int solve(const string& s1, const string& s2, int i, int j, vector<vector<int>>& memo) {'],
      ['c_base',       '    if (i == 0 || j == 0) return 0;'],
      ['c_memo_check', '    if (memo[i][j] != -1) return memo[i][j];'],
      ['c_match_chk',  '    if (s1[i - 1] == s2[j - 1]) {'],
      ['c_match_calc', '        return memo[i][j] = 1 + solve(s1, s2, i - 1, j - 1, memo);'],
      ['c_nomatch',    '    } else {'],
      ['c_nomatch_calc','        return memo[i][j] = max(solve(s1, s2, i - 1, j, memo), solve(s1, s2, i, j - 1, memo));'],
      ['',             '    }'],
      ['',             '}'],
      ['',             ''],
      ['int main() {'],
      ['m_read_s1',    '    string s1, s2; cin >> s1 >> s2;'],
      ['m_alloc_memo', '    int m = s1.size(), n = s2.size();'],
      ['m_fill_memo',  '    vector<vector<int>> memo(m + 1, vector<int>(n + 1, -1));'],
      ['m_call_lcs',   '    int ans = solve(s1, s2, m, n, memo);'],
      ['m_print',      '    cout << ans << endl;'],
      ['m_done',       '    return 0;'],
      ['',             '}']
    ],
    python: [
      ['',             'import sys'],
      ['',             ''],
      ['c_entry',      'def solve(s1, s2, i, j, memo):'],
      ['c_base',       '    if i == 0 or j == 0:'],
      ['',             '        return 0'],
      ['c_memo_check', '    if memo[i][j] != -1:'],
      ['',             '        return memo[i][j]'],
      ['c_match_chk',  '    if s1[i - 1] == s2[j - 1]:'],
      ['c_match_calc', '        memo[i][j] = 1 + solve(s1, s2, i - 1, j - 1, memo)'],
      ['c_nomatch',    '    else:'],
      ['c_nomatch_calc','        memo[i][j] = max(solve(s1, s2, i - 1, j, memo), solve(s1, s2, i, j - 1, memo))'],
      ['c_ret_memo',   '    return memo[i][j]'],
      ['',             ''],
      ['if __name__ == "__main__":'],
      ['m_read_s1',    '    s1, s2 = sys.stdin.read().split()[:2]'],
      ['m_alloc_memo', '    m, n = len(s1), len(s2)'],
      ['m_fill_memo',  '    memo = [[-1] * (n + 1) for _ in range(m + 1)]'],
      ['m_call_lcs',   '    ans = solve(s1, s2, m, n, memo)'],
      ['m_print',      '    print(ans)'],
      ['m_done',       '    sys.exit(0)']
    ],
    javascript: [
      ['',             'const fs = require("fs");'],
      ['',             ''],
      ['c_entry',      'function solve(s1, s2, i, j, memo) {'],
      ['c_base',       '    if (i === 0 || j === 0) return 0;'],
      ['c_memo_check', '    if (memo[i][j] !== -1) return memo[i][j];'],
      ['c_match_chk',  '    if (s1[i - 1] === s2[j - 1]) {'],
      ['c_match_calc', '        memo[i][j] = 1 + solve(s1, s2, i - 1, j - 1, memo);'],
      ['c_nomatch',    '    } else {'],
      ['c_nomatch_calc','        memo[i][j] = Math.max(solve(s1, s2, i - 1, j, memo), solve(s1, s2, i, j - 1, memo));'],
      ['',             '    }'],
      ['c_ret_memo',   '    return memo[i][j];'],
      ['',             '}'],
      ['',             ''],
      ['const [s1, s2] = fs.readFileSync("/dev/stdin", "utf-8").trim().split(/\\s+/);'],
      ['const memo = Array.from({ length: s1.length + 1 }, () => new Array(s2.length + 1).fill(-1));'],
      ['const ans = solve(s1, s2, s1.length, s2.length, memo);'],
      ['console.log(ans);'],
      ['process.exit(0);']
    ]
  },
  tabulation: {
    java: [
      ['',             'import java.util.Scanner;'],
      ['',             ''],
      ['',             'public class Main {'],
      ['c_entry',      '    static int longestCommonSubsequence(String s1, String s2) {'],
      ['c_init_dims',  '        int m = s1.length(), n = s2.length();'],
      ['c_alloc_dp',   '        int[][] dp = new int[m + 1][n + 1];'],
      ['c_loop_i',     '        for (int i = 1; i <= m; i++) {'],
      ['c_loop_j',     '            for (int j = 1; j <= n; j++) {'],
      ['c_match_chk',  '                if (s1.charAt(i - 1) == s2.charAt(j - 1)) {'],
      ['c_dp_match',   '                    dp[i][j] = 1 + dp[i - 1][j - 1];'],
      ['c_nomatch',    '                } else {'],
      ['c_dp_mismatch','                    dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);'],
      ['',             '                }'],
      ['',             '            }'],
      ['',             '        }'],
      ['c_ret_dp',     '        return dp[m][n];'],
      ['',             '    }'],
      ['',             ''],
      ['',             '    public static void main(String[] args) {'],
      ['m_scanner',    '        Scanner sc = new Scanner(System.in);'],
      ['m_read_s1',    '        String s1 = sc.next(); String s2 = sc.next();'],
      ['m_call_lcs',   '        int ans = longestCommonSubsequence(s1, s2);'],
      ['m_print',      '        System.out.println(ans);'],
      ['m_done',       '    }'],
      ['',             '}']
    ],
    c: [
      ['',             '#include <stdio.h>'],
      ['',             '#include <string.h>'],
      ['',             '#define MAX(a, b) ((a) > (b) ? (a) : (b))'],
      ['',             ''],
      ['c_entry',      'int longestCommonSubsequence(const char* s1, const char* s2) {'],
      ['c_init_dims',  '    int m = strlen(s1), n = strlen(s2);'],
      ['c_alloc_dp',   '    int dp[m + 1][n + 1]; memset(dp, 0, sizeof(dp));'],
      ['c_loop_i',     '    for (int i = 1; i <= m; i++) {'],
      ['c_loop_j',     '        for (int j = 1; j <= n; j++) {'],
      ['c_match_chk',  '            if (s1[i - 1] == s2[j - 1])'],
      ['c_dp_match',   '                dp[i][j] = 1 + dp[i - 1][j - 1];'],
      ['c_nomatch',    '            else'],
      ['c_dp_mismatch','                dp[i][j] = MAX(dp[i - 1][j], dp[i][j - 1]);'],
      ['',             '        }'],
      ['',             '    }'],
      ['c_ret_dp',     '    return dp[m][n];'],
      ['',             '}'],
      ['',             ''],
      ['int main() {'],
      ['m_read_s1',    '    char s1[100], s2[100]; scanf("%s %s", s1, s2);'],
      ['m_call_lcs',   '    int ans = longestCommonSubsequence(s1, s2);'],
      ['m_print',      '    printf("%d\\n", ans);'],
      ['m_done',       '    return 0;'],
      ['',             '}']
    ],
    cpp: [
      ['',             '#include <iostream>'],
      ['',             '#include <vector>'],
      ['',             '#include <string>'],
      ['',             '#include <algorithm>'],
      ['',             'using namespace std;'],
      ['',             ''],
      ['c_entry',      'int longestCommonSubsequence(const string& s1, const string& s2) {'],
      ['c_init_dims',  '    int m = s1.size(), n = s2.size();'],
      ['c_alloc_dp',   '    vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));'],
      ['c_loop_i',     '    for (int i = 1; i <= m; i++) {'],
      ['c_loop_j',     '        for (int j = 1; j <= n; j++) {'],
      ['c_match_chk',  '            if (s1[i - 1] == s2[j - 1])'],
      ['c_dp_match',   '                dp[i][j] = 1 + dp[i - 1][j - 1];'],
      ['c_nomatch',    '            else'],
      ['c_dp_mismatch','                dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);'],
      ['',             '        }'],
      ['',             '    }'],
      ['c_ret_dp',     '    return dp[m][n];'],
      ['',             '}'],
      ['',             ''],
      ['int main() {'],
      ['m_read_s1',    '    string s1, s2; cin >> s1 >> s2;'],
      ['m_call_lcs',   '    int ans = longestCommonSubsequence(s1, s2);'],
      ['m_print',      '    cout << ans << endl;'],
      ['m_done',       '    return 0;'],
      ['',             '}']
    ],
    python: [
      ['',             'import sys'],
      ['',             ''],
      ['c_entry',      'def longest_common_subsequence(s1, s2):'],
      ['c_init_dims',  '    m, n = len(s1), len(s2)'],
      ['c_alloc_dp',   '    dp = [[0] * (n + 1) for _ in range(m + 1)]'],
      ['c_loop_i',     '    for i in range(1, m + 1):'],
      ['c_loop_j',     '        for j in range(1, n + 1):'],
      ['c_match_chk',  '            if s1[i - 1] == s2[j - 1]:'],
      ['c_dp_match',   '                dp[i][j] = 1 + dp[i - 1][j - 1]'],
      ['c_nomatch',    '            else:'],
      ['c_dp_mismatch','                dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])'],
      ['c_ret_dp',     '    return dp[m][n]'],
      ['',             ''],
      ['if __name__ == "__main__":'],
      ['m_read_s1',    '    s1, s2 = sys.stdin.read().split()[:2]'],
      ['m_call_lcs',   '    ans = longest_common_subsequence(s1, s2)'],
      ['m_print',      '    print(ans)'],
      ['m_done',       '    sys.exit(0)']
    ],
    javascript: [
      ['',             'const fs = require("fs");'],
      ['',             ''],
      ['c_entry',      'function longestCommonSubsequence(s1, s2) {'],
      ['c_init_dims',  '    const m = s1.length, n = s2.length;'],
      ['c_alloc_dp',   '    const dp = Array.from({ length: m + 1 }, () => new Array(n + 1).fill(0));'],
      ['c_loop_i',     '    for (let i = 1; i <= m; i++) {'],
      ['c_loop_j',     '        for (let j = 1; j <= n; j++) {'],
      ['c_match_chk',  '            if (s1[i - 1] === s2[j - 1]) {'],
      ['c_dp_match',   '                dp[i][j] = 1 + dp[i - 1][j - 1];'],
      ['c_nomatch',    '            } else {'],
      ['c_dp_mismatch','                dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);'],
      ['',             '            }'],
      ['',             '        }'],
      ['',             '    }'],
      ['c_ret_dp',     '    return dp[m][n];'],
      ['',             '}'],
      ['',             ''],
      ['const [s1, s2] = fs.readFileSync("/dev/stdin", "utf-8").trim().split(/\\s+/);'],
      ['const ans = longestCommonSubsequence(s1, s2);'],
      ['console.log(ans);'],
      ['process.exit(0);']
    ]
  }
};

const PSEUDOCODES = {
  recursion: [
    'function lcs(s1, s2, i, j):',
    '  if i == 0 or j == 0: return 0',
    '  if s1[i-1] == s2[j-1]:',
    '    return 1 + lcs(s1, s2, i-1, j-1)       // Match: Take +1 & shrink both',
    '  else:',
    '    return max(lcs(s1,s2,i-1,j), lcs(s1,s2,i,j-1)) // Mismatch: Try both skips',
    '',
    'Time Complexity:  O(2^(m+n)) Exponential',
    'Space Complexity: O(m+n) Call Stack'
  ],
  memoization: [
    'memo = 2D array of size (m+1) x (n+1), filled with -1',
    '',
    'function solve(s1, s2, i, j, memo):',
    '  if i == 0 or j == 0: return 0',
    '  if memo[i][j] != -1: return memo[i][j]  // O(1) Cache Hit',
    '  if s1[i-1] == s2[j-1]:',
    '    memo[i][j] = 1 + solve(s1, s2, i-1, j-1, memo)',
    '  else:',
    '    memo[i][j] = max(solve(s1,s2,i-1,j,memo), solve(s1,s2,i,j-1,memo))',
    '  return memo[i][j]',
    '',
    'Time Complexity:  O(m * n) Polynomial Time',
    'Space Complexity: O(m * n) 2D Cache + O(m+n) Call Stack'
  ],
  tabulation: [
    'function longestCommonSubsequence(s1, s2):',
    '  dp = 2D array of size (m+1) x (n+1), filled with 0',
    '  for i = 1 to m:',
    '    for j = 1 to n:',
    '      if s1[i-1] == s2[j-1]:',
    '        dp[i][j] = 1 + dp[i-1][j-1]         // Diagonal + 1',
    '      else:',
    '        dp[i][j] = max(dp[i-1][j], dp[i][j-1]) // Max(Top, Left)',
    '  return dp[m][n]',
    '',
    'Time Complexity:  O(m * n) Full Matrix Fill',
    'Space Complexity: O(m * n) Table (Can be optimized to O(min(m,n)) with 2 rows)'
  ]
};

/* ==================================================================== */
/* STATIC TREE GENERATOR FOR DECISION TREE (LCS RECURSION / MEMO)       */
/* ==================================================================== */
function generateStaticTree(approach, s1, s2) {
  const m = s1.length;
  const n = s2.length;
  const nodes = [];
  const edges = [];
  let nextId = 0;

  if (approach === 'recursion') {
    function build(i, j, parentId, branchType) {
      const id = nextId++;
      const node = { id, i, j, parentId, branchType, isRedundant: false, children: [] };
      nodes.push(node);

      if (parentId !== null) {
        edges.push({ from: parentId, to: id, branchType });
      }

      if (i > 0 && j > 0) {
        if (s1[i - 1] === s2[j - 1]) {
          const childId = build(i - 1, j - 1, id, 'match');
          node.children = [childId];
        } else {
          const leftId = build(i - 1, j, id, 'skip1');
          const rightId = build(i, j - 1, id, 'skip2');
          node.children = [leftId, rightId];
        }
      }
      return id;
    }
    if (m >= 0 && n >= 0) build(m, n, null, 'root');
  } else if (approach === 'memoization') {
    const memo = Array.from({ length: m + 1 }, () => new Array(n + 1).fill(false));
    function buildMemo(i, j, parentId, branchType) {
      const id = nextId++;
      const isCacheHit = i >= 0 && j >= 0 && memo[i][j];
      const node = { id, i, j, parentId, branchType, isCacheHit, children: [] };
      nodes.push(node);

      if (parentId !== null) {
        edges.push({ from: parentId, to: id, branchType });
      }

      if (i <= 0 || j <= 0) {
        // base
      } else if (isCacheHit) {
        // Pruned subtree — O(1) Cache lookup
      } else {
        memo[i][j] = true;
        if (s1[i - 1] === s2[j - 1]) {
          const childId = buildMemo(i - 1, j - 1, id, 'match');
          node.children = [childId];
        } else {
          const leftId = buildMemo(i - 1, j, id, 'skip1');
          const rightId = buildMemo(i, j - 1, id, 'skip2');
          node.children = [leftId, rightId];
        }
      }
      return id;
    }
    if (m >= 0 && n >= 0) buildMemo(m, n, null, 'root');
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
function buildSteps(approach, rawS1, rawS2) {
  const steps = [];
  const maxLimit = approach === 'tabulation' ? 8 : 5;
  const s1 = (rawS1 || 'abcde').slice(0, maxLimit);
  const s2 = (rawS2 || 'ace').slice(0, maxLimit);
  const m = s1.length;
  const n = s2.length;

  /* ------------------------------------------------------------------ */
  /* APPROACH 1: BRUTE FORCE RECURSION                                   */
  /* ------------------------------------------------------------------ */
  if (approach === 'recursion') {
    const staticTree = generateStaticTree('recursion', s1, s2);
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
        y1: staticTree.layoutMap[e.from].y + 12,
        x2: staticTree.layoutMap[e.to].x,
        y2: staticTree.layoutMap[e.to].y - 12
      }));
    }

    // Step 0: Read input
    steps.push({
      code: 'm_read_s1',
      badge: `Input Strings: s1 = "${s1}" (m=${m}), s2 = "${s2}" (n=${n})`,
      badgeType: 'info',
      state: {
        s1, s2, m, n,
        totalCalls: 0,
        redundantCalls: 0,
        treeNodes: [],
        treeEdges: [],
        treeWidth: staticTree.width,
        treeHeight: staticTree.height,
        activeNodeId: null,
        currentReturn: null,
        frames: []
      }
    });

    steps.push({
      code: 'm_call_lcs',
      badge: `Invoking root recursive call lcs("${s1}", "${s2}", ${m}, ${n})`,
      badgeType: 'info',
      state: {
        s1, s2, m, n,
        totalCalls: 0,
        redundantCalls: 0,
        treeNodes: [],
        treeEdges: [],
        treeWidth: staticTree.width,
        treeHeight: staticTree.height,
        activeNodeId: null,
        currentReturn: null,
        frames: [{ name: 'main()', args: `s1="${s1}", s2="${s2}"` }]
      }
    });

    function solveRec(i, j) {
      const myId = nextNodeId++;
      callCounter++;
      const subKey = `${i},${j}`;
      const isRedundant = visitedSubproblems.has(subKey);
      if (isRedundant) redundantCounter++;
      visitedSubproblems.add(subKey);

      nodeStateMap[myId].state = 'active';
      nodeStateMap[myId].isRedundant = isRedundant;

      stackFrames.push({ name: `lcs(i=${i}, j=${j})`, args: `s1[${i-1}], s2[${j-1}]` });

      steps.push({
        code: 'c_entry',
        badge: `Entering lcs(i=${i}, j=${j}) &mdash; evaluating characters s1[${i-1}] and s2[${j-1}]`,
        badgeType: isRedundant ? 'warn' : 'info',
        state: {
          s1, s2, m, n, curI: i, curJ: j,
          char1: i > 0 ? s1[i - 1] : null,
          char2: j > 0 ? s2[j - 1] : null,
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
        badge: i === 0 || j === 0
          ? `Base Case Reached! (i=${i} || j=${j}) &rarr; Remaining length is 0`
          : `Base Case Check: (i=${i} != 0 && j=${j} != 0) &rarr; Proceeding to character comparison`,
        badgeType: i === 0 || j === 0 ? 'success' : 'info',
        state: {
          s1, s2, m, n, curI: i, curJ: j,
          char1: i > 0 ? s1[i - 1] : null,
          char2: j > 0 ? s2[j - 1] : null,
          totalCalls: callCounter,
          redundantCalls: redundantCounter,
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height,
          activeNodeId: myId,
          currentReturn: i === 0 || j === 0 ? 0 : null,
          frames: [...stackFrames]
        }
      });

      if (i === 0 || j === 0) {
        nodeStateMap[myId].state = 'solved';
        nodeStateMap[myId].retVal = 0;
        stackFrames.pop();
        return 0;
      }

      const match = s1[i - 1] === s2[j - 1];

      steps.push({
        code: 'c_match_chk',
        badge: match
          ? `MATCH FOUND! s1[${i - 1}]=='${s1[i - 1]}' == s2[${j - 1}]=='${s2[j - 1]}' &rarr; Branching (i-1, j-1)`
          : `MISMATCH: s1[${i - 1}]=='${s1[i - 1]}' != s2[${j - 1}]=='${s2[j - 1]}' &rarr; 2-way Branch (skip s1 vs skip s2)`,
        badgeType: match ? 'success' : 'info',
        state: {
          s1, s2, m, n, curI: i, curJ: j,
          char1: s1[i - 1], char2: s2[j - 1],
          isMatch: match,
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
      if (match) {
        steps.push({
          code: 'c_match_rec',
          badge: `Match transition: Take +1 and recursively solve subproblem lcs(${i - 1}, ${j - 1})`,
          badgeType: 'info',
          state: {
            s1, s2, m, n, curI: i, curJ: j,
            char1: s1[i - 1], char2: s2[j - 1],
            isMatch: true,
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

        const sub = solveRec(i - 1, j - 1);
        ans = 1 + sub;
      } else {
        steps.push({
          code: 'c_nomatch_rec',
          badge: `Mismatch: Branching Left &rarr; Trying skip1 = lcs(${i - 1}, ${j})`,
          badgeType: 'info',
          state: {
            s1, s2, m, n, curI: i, curJ: j,
            char1: s1[i - 1], char2: s2[j - 1],
            isMatch: false,
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

        const leftRes = solveRec(i - 1, j);

        steps.push({
          code: 'c_nomatch_rec',
          badge: `Branching Right &rarr; Trying skip2 = lcs(${i}, ${j - 1})`,
          badgeType: 'info',
          state: {
            s1, s2, m, n, curI: i, curJ: j,
            char1: s1[i - 1], char2: s2[j - 1],
            isMatch: false,
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

        const rightRes = solveRec(i, j - 1);
        ans = Math.max(leftRes, rightRes);

        steps.push({
          code: 'c_ret_max',
          badge: `Combining branches for lcs(${i}, ${j}): max(skip1=${leftRes}, skip2=${rightRes}) = ${ans}`,
          badgeType: 'success',
          state: {
            s1, s2, m, n, curI: i, curJ: j,
            char1: s1[i - 1], char2: s2[j - 1],
            isMatch: false,
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

    const finalAns = solveRec(m, n);

    steps.push({
      code: 'm_print',
      badge: `Computation Complete! Longest Common Subsequence length = ${finalAns}`,
      badgeType: 'success',
      state: {
        s1, s2, m, n,
        totalCalls: callCounter,
        redundantCalls: redundantCounter,
        treeNodes: getVisibleNodes(),
        treeEdges: getVisibleEdges(),
        treeWidth: staticTree.width,
        treeHeight: staticTree.height,
        activeNodeId: null,
        currentReturn: finalAns,
        frames: []
      }
    });

    return steps;
  }

  /* ------------------------------------------------------------------ */
  /* APPROACH 2: DP MEMOIZATION                                         */
  /* ------------------------------------------------------------------ */
  if (approach === 'memoization') {
    const staticTree = generateStaticTree('memoization', s1, s2);
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

    const memoGrid = Array.from({ length: m + 1 }, () => new Array(n + 1).fill(-1));
    const stackFrames = [];
    let nextNodeId = 0;
    let callCounter = 0;
    let hitCounter = 0;

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

    steps.push({
      code: 'm_read_s1',
      badge: `Input Strings: s1 = "${s1}" (m=${m}), s2 = "${s2}" (n=${n})`,
      badgeType: 'info',
      state: {
        s1, s2, m, n,
        totalCalls: 0,
        cacheHits: 0,
        memo: memoGrid.map(r => [...r]),
        treeNodes: [],
        treeEdges: [],
        treeWidth: staticTree.width,
        treeHeight: staticTree.height,
        activeNodeId: null,
        currentReturn: null,
        frames: []
      }
    });

    steps.push({
      code: 'm_alloc_memo',
      badge: `Allocating memo 2D cache table of size (${m + 1}) &times; (${n + 1}) initialized with -1`,
      badgeType: 'info',
      state: {
        s1, s2, m, n,
        totalCalls: 0,
        cacheHits: 0,
        memo: memoGrid.map(r => [...r]),
        treeNodes: [],
        treeEdges: [],
        treeWidth: staticTree.width,
        treeHeight: staticTree.height,
        activeNodeId: null,
        currentReturn: null,
        frames: [{ name: 'main()', args: `s1="${s1}", s2="${s2}"` }]
      }
    });

    function solveMem(i, j) {
      const myId = nextNodeId++;
      callCounter++;
      const isHit = i >= 0 && j >= 0 && memoGrid[i][j] !== -1;
      if (isHit) hitCounter++;

      nodeStateMap[myId].state = isHit ? 'cache-hit' : 'active';
      stackFrames.push({ name: `solve(i=${i}, j=${j})`, args: `memo[${i}][${j}]` });

      steps.push({
        code: 'c_entry',
        badge: `Entering solve(i=${i}, j=${j})`,
        badgeType: isHit ? 'success' : 'info',
        state: {
          s1, s2, m, n, curI: i, curJ: j,
          char1: i > 0 ? s1[i - 1] : null,
          char2: j > 0 ? s2[j - 1] : null,
          totalCalls: callCounter,
          cacheHits: hitCounter,
          memo: memoGrid.map(r => [...r]),
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height,
          activeNodeId: myId,
          currentReturn: null,
          frames: [...stackFrames]
        }
      });

      steps.push({
        code: 'c_base',
        badge: i === 0 || j === 0
          ? `Base Case: (i=${i} || j=${j}) &rarr; Return 0`
          : `Base Case Check: (i=${i} != 0 && j=${j} != 0) &rarr; Next checking lookup cache`,
        badgeType: i === 0 || j === 0 ? 'success' : 'info',
        state: {
          s1, s2, m, n, curI: i, curJ: j,
          char1: i > 0 ? s1[i - 1] : null,
          char2: j > 0 ? s2[j - 1] : null,
          totalCalls: callCounter,
          cacheHits: hitCounter,
          memo: memoGrid.map(r => [...r]),
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height,
          activeNodeId: myId,
          currentReturn: i === 0 || j === 0 ? 0 : null,
          frames: [...stackFrames]
        }
      });

      if (i === 0 || j === 0) {
        nodeStateMap[myId].state = 'solved';
        nodeStateMap[myId].retVal = 0;
        stackFrames.pop();
        return 0;
      }

      // Check Cache
      steps.push({
        code: 'c_memo_check',
        badge: memoGrid[i][j] !== -1
          ? `O(1) CACHE HIT! memo[${i}][${j}] = ${memoGrid[i][j]} &rarr; Pruning whole subtree!`
          : `Cache Miss: memo[${i}][${j}] == -1 &rarr; Proceeding with computation`,
        badgeType: memoGrid[i][j] !== -1 ? 'success' : 'info',
        state: {
          s1, s2, m, n, curI: i, curJ: j,
          char1: s1[i - 1], char2: s2[j - 1],
          totalCalls: callCounter,
          cacheHits: hitCounter,
          memo: memoGrid.map(r => [...r]),
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height,
          activeNodeId: myId,
          currentReturn: memoGrid[i][j] !== -1 ? memoGrid[i][j] : null,
          frames: [...stackFrames]
        }
      });

      if (memoGrid[i][j] !== -1) {
        nodeStateMap[myId].state = 'cache-hit';
        nodeStateMap[myId].retVal = memoGrid[i][j];
        stackFrames.pop();
        return memoGrid[i][j];
      }

      const match = s1[i - 1] === s2[j - 1];

      steps.push({
        code: 'c_match_chk',
        badge: match
          ? `Match: s1[${i - 1}]=='${s1[i - 1]}' == s2[${j - 1}]=='${s2[j - 1]}' &rarr; 1 + solve(${i - 1}, ${j - 1})`
          : `Mismatch: s1[${i - 1}]=='${s1[i - 1]}' != s2[${j - 1}]=='${s2[j - 1]}' &rarr; max(solve(${i - 1}, ${j}), solve(${i}, ${j - 1}))`,
        badgeType: match ? 'success' : 'info',
        state: {
          s1, s2, m, n, curI: i, curJ: j,
          char1: s1[i - 1], char2: s2[j - 1],
          isMatch: match,
          totalCalls: callCounter,
          cacheHits: hitCounter,
          memo: memoGrid.map(r => [...r]),
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height,
          activeNodeId: myId,
          currentReturn: null,
          frames: [...stackFrames]
        }
      });

      let res = 0;
      if (match) {
        steps.push({
          code: 'c_match_calc',
          badge: `Solving subproblem solve(${i - 1}, ${j - 1}) for match...`,
          badgeType: 'info',
          state: {
            s1, s2, m, n, curI: i, curJ: j,
            char1: s1[i - 1], char2: s2[j - 1],
            isMatch: true,
            totalCalls: callCounter,
            cacheHits: hitCounter,
            memo: memoGrid.map(r => [...r]),
            treeNodes: getVisibleNodes(),
            treeEdges: getVisibleEdges(),
            treeWidth: staticTree.width,
            treeHeight: staticTree.height,
            activeNodeId: myId,
            currentReturn: null,
            frames: [...stackFrames]
          }
        });

        const sub = solveMem(i - 1, j - 1);
        res = 1 + sub;
      } else {
        steps.push({
          code: 'c_nomatch_calc',
          badge: `Mismatch: Solving branch 1 solve(${i - 1}, ${j})...`,
          badgeType: 'info',
          state: {
            s1, s2, m, n, curI: i, curJ: j,
            char1: s1[i - 1], char2: s2[j - 1],
            isMatch: false,
            totalCalls: callCounter,
            cacheHits: hitCounter,
            memo: memoGrid.map(r => [...r]),
            treeNodes: getVisibleNodes(),
            treeEdges: getVisibleEdges(),
            treeWidth: staticTree.width,
            treeHeight: staticTree.height,
            activeNodeId: myId,
            currentReturn: null,
            frames: [...stackFrames]
          }
        });

        const leftRes = solveMem(i - 1, j);

        steps.push({
          code: 'c_nomatch_calc',
          badge: `Mismatch: Solving branch 2 solve(${i}, ${j - 1})...`,
          badgeType: 'info',
          state: {
            s1, s2, m, n, curI: i, curJ: j,
            char1: s1[i - 1], char2: s2[j - 1],
            isMatch: false,
            totalCalls: callCounter,
            cacheHits: hitCounter,
            memo: memoGrid.map(r => [...r]),
            treeNodes: getVisibleNodes(),
            treeEdges: getVisibleEdges(),
            treeWidth: staticTree.width,
            treeHeight: staticTree.height,
            activeNodeId: myId,
            currentReturn: null,
            frames: [...stackFrames]
          }
        });

        const rightRes = solveMem(i, j - 1);
        res = Math.max(leftRes, rightRes);
      }

      memoGrid[i][j] = res;

      steps.push({
        code: 'c_ret_memo',
        badge: `Storing & returning memo[${i}][${j}] = ${res}`,
        badgeType: 'success',
        state: {
          s1, s2, m, n, curI: i, curJ: j,
          char1: s1[i - 1], char2: s2[j - 1],
          totalCalls: callCounter,
          cacheHits: hitCounter,
          memo: memoGrid.map(r => [...r]),
          treeNodes: getVisibleNodes(),
          treeEdges: getVisibleEdges(),
          treeWidth: staticTree.width,
          treeHeight: staticTree.height,
          activeNodeId: myId,
          currentReturn: res,
          frames: [...stackFrames]
        }
      });

      nodeStateMap[myId].state = 'solved';
      nodeStateMap[myId].retVal = res;
      stackFrames.pop();
      return res;
    }

    const finalAns = solveMem(m, n);

    steps.push({
      code: 'm_print',
      badge: `Memoization Execution Finished! Optimal LCS Length = ${finalAns} (Cache Hits: ${hitCounter})`,
      badgeType: 'success',
      state: {
        s1, s2, m, n,
        totalCalls: callCounter,
        cacheHits: hitCounter,
        memo: memoGrid.map(r => [...r]),
        treeNodes: getVisibleNodes(),
        treeEdges: getVisibleEdges(),
        treeWidth: staticTree.width,
        treeHeight: staticTree.height,
        activeNodeId: null,
        currentReturn: finalAns,
        frames: []
      }
    });

    return steps;
  }

  /* ------------------------------------------------------------------ */
  /* APPROACH 3: DP TABULATION (2D Grid Matrix + Backtrack)              */
  /* ------------------------------------------------------------------ */
  if (approach === 'tabulation') {
    const dpGrid = Array.from({ length: m + 1 }, () => new Array(n + 1).fill('?'));
    let opCounter = 0;

    // Step 0: Read Inputs
    steps.push({
      code: 'm_read_s1',
      badge: `Reading input strings: s1 = "${s1}" (m=${m}), s2 = "${s2}" (n=${n})`,
      badgeType: 'info',
      state: {
        s1, s2, m, n,
        iterations: 0,
        totalOps: 0,
        i: null, j: null,
        dpGrid: dpGrid.map(r => [...r]),
        tracebackCells: [],
        currentReturn: null
      }
    });

    // Step 1: Alloc DP 2D Matrix
    for (let r = 0; r <= m; r++) {
      for (let c = 0; c <= n; c++) {
        if (r === 0 || c === 0) dpGrid[r][c] = 0;
      }
    }
    opCounter += (m + 1) + (n + 1) - 1;

    steps.push({
      code: 'c_alloc_dp',
      badge: `Allocating 2D DP Table dp[${m + 1}][${n + 1}]. Base cases: Row 0 and Col 0 initialized to 0`,
      badgeType: 'info',
      state: {
        s1, s2, m, n,
        iterations: 0,
        totalOps: opCounter,
        i: 0, j: 0,
        dpGrid: dpGrid.map(r => [...r]),
        tracebackCells: [],
        currentReturn: null
      }
    });

    for (let i = 1; i <= m; i++) {
      steps.push({
        code: 'c_loop_i',
        badge: `Outer Loop: Iterating row i = ${i} (character s1[${i - 1}] = '${s1[i - 1]}')`,
        badgeType: 'info',
        state: {
          s1, s2, m, n,
          iterations: Math.max(0, (i - 1) * n),
          totalOps: opCounter,
          i, j: null,
          char1: s1[i - 1],
          dpGrid: dpGrid.map(r => [...r]),
          tracebackCells: [],
          currentReturn: null
        }
      });

      for (let j = 1; j <= n; j++) {
        opCounter++;
        const currentIter = (i - 1) * n + j;
        const match = s1[i - 1] === s2[j - 1];

        steps.push({
          code: 'c_loop_j',
          badge: `Inner Loop (Iteration #${currentIter}/${m * n}): Examining cell dp[${i}][${j}] (s1[${i - 1}]='${s1[i - 1]}' vs s2[${j - 1}]='${s2[j - 1]}')`,
          badgeType: 'info',
          state: {
            s1, s2, m, n,
            iterations: currentIter,
            totalOps: opCounter,
            i, j,
            char1: s1[i - 1],
            char2: s2[j - 1],
            isMatch: match,
            dpGrid: dpGrid.map(r => [...r]),
            tracebackCells: [],
            currentReturn: null
          }
        });

        steps.push({
          code: 'c_match_chk',
          badge: match
            ? `MATCH DETECTED! s1[${i - 1}] == s2[${j - 1}] ('${s1[i - 1]}') &rarr; Taking 1 + dp[${i - 1}][${j - 1}]`
            : `MISMATCH: s1[${i - 1}] != s2[${j - 1}] ('${s1[i - 1]}' != '${s2[j - 1]}') &rarr; Taking max(dp[${i - 1}][${j}], dp[${i}][${j - 1}])`,
          badgeType: match ? 'success' : 'info',
          state: {
            s1, s2, m, n,
            iterations: currentIter,
            totalOps: opCounter,
            i, j,
            char1: s1[i - 1],
            char2: s2[j - 1],
            isMatch: match,
            dpGrid: dpGrid.map(r => [...r]),
            tracebackCells: [],
            currentReturn: null
          }
        });

        if (match) {
          const prevDiag = dpGrid[i - 1][j - 1];
          const calculated = 1 + prevDiag;
          dpGrid[i][j] = calculated;

          steps.push({
            code: 'c_dp_match',
            badge: `dp[${i}][${j}] = 1 + dp[${i - 1}][${j - 1}] = 1 + ${prevDiag} = ${calculated}`,
            badgeType: 'success',
            state: {
              s1, s2, m, n,
              iterations: currentIter,
              totalOps: opCounter,
              i, j,
              char1: s1[i - 1],
              char2: s2[j - 1],
              isMatch: true,
              dpGrid: dpGrid.map(r => [...r]),
              tracebackCells: [],
              currentReturn: calculated
            }
          });
        } else {
          const topVal = dpGrid[i - 1][j];
          const leftVal = dpGrid[i][j - 1];
          const calculated = Math.max(topVal, leftVal);
          dpGrid[i][j] = calculated;

          steps.push({
            code: 'c_dp_mismatch',
            badge: `dp[${i}][${j}] = max(top: dp[${i - 1}][${j}]=${topVal}, left: dp[${i}][${j - 1}]=${leftVal}) = ${calculated}`,
            badgeType: 'info',
            state: {
              s1, s2, m, n,
              iterations: currentIter,
              totalOps: opCounter,
              i, j,
              char1: s1[i - 1],
              char2: s2[j - 1],
              isMatch: false,
              dpGrid: dpGrid.map(r => [...r]),
              tracebackCells: [],
              currentReturn: calculated
            }
          });
        }
      }
    }

    const totalIterations = m * n;

    // Reconstruction (Backtracking) of LCS Path
    let r = m, c = n;
    const pathCells = [];
    let lcsChars = [];
    while (r > 0 && c > 0) {
      pathCells.push({ i: r, j: c });
      if (s1[r - 1] === s2[c - 1]) {
        lcsChars.push(s1[r - 1]);
        r--;
        c--;
      } else if (dpGrid[r - 1][c] >= dpGrid[r][c - 1]) {
        r--;
      } else {
        c--;
      }
    }
    pathCells.push({ i: r, j: c });
    const lcsStr = lcsChars.reverse().join('');
    const finalLength = dpGrid[m][n];

    steps.push({
      code: 'c_ret_dp',
      badge: `Table Complete! Returning dp[${m}][${n}] = ${finalLength}. Optimal Common Subsequence = "${lcsStr}"`,
      badgeType: 'success',
      state: {
        s1, s2, m, n,
        iterations: totalIterations,
        totalOps: opCounter,
        i: m, j: n,
        dpGrid: dpGrid.map(r => [...r]),
        tracebackCells: pathCells,
        lcsString: lcsStr,
        currentReturn: finalLength
      }
    });

    steps.push({
      code: 'm_print',
      badge: `Tabulation Complete (${totalIterations} iterations)! Longest Common Subsequence is "${lcsStr}" with length ${finalLength}`,
      badgeType: 'success',
      state: {
        s1, s2, m, n,
        iterations: totalIterations,
        totalOps: opCounter,
        i: null, j: null,
        dpGrid: dpGrid.map(r => [...r]),
        tracebackCells: pathCells,
        lcsString: lcsStr,
        currentReturn: finalLength
      }
    });

    return steps;
  }

  return steps;
}

/* ==================================================================== */
/* REACTIVE STATE & INTERACTIVE CONTROLS                                */
/* ==================================================================== */
const currentApproach = ref('tabulation');
const rightTab = ref('code');
const lang = ref('java');
const speed = ref(600);
const s1InputStr = ref('AGGTAB');
const s2InputStr = ref('GXTXAYB');
const currentS1 = ref('AGGTAB');
const currentS2 = ref('GXTXAYB');

const maxAllowedChars = computed(() => {
  return currentApproach.value === 'tabulation' ? 8 : 5;
});

// Warning Modal State
const showWarningModal = ref(false);
const warningModalTitle = ref('Input Limit Exceeded');
const warningModalMsg = ref('');
const requestedS1 = ref('');
const requestedS2 = ref('');
const warningModalLimit = ref(8);

const currentStepIdx = ref(0);
const playing = ref(false);
let playTimer = null;

// Presets Dropdown
const showPresetsDropdown = ref(false);
const presetDropdownRef = ref(null);

function selectPreset(p) {
  s1InputStr.value = p.s1;
  s2InputStr.value = p.s2;
  applyInput();
  showPresetsDropdown.value = false;
}

function handleDocClick(e) {
  if (presetDropdownRef.value && !presetDropdownRef.value.contains(e.target)) {
    showPresetsDropdown.value = false;
  }
}

// Resizable workspace panes
const leftWidth = ref(54);
const vizHeight = ref(280);
const mainRef = ref(null);
const resizerRef = ref(null);
const vizResizerRef = ref(null);

const steps = computed(() => {
  return buildSteps(currentApproach.value, currentS1.value, currentS2.value);
});

const currentStep = computed(() => {
  if (!steps.value.length) return { code: '', badge: '', badgeType: 'info', state: {} };
  const idx = Math.min(currentStepIdx.value, steps.value.length - 1);
  return steps.value[idx];
});

const s = computed(() => currentStep.value);
const st = computed(() => currentStep.value.state || {});

const codeLines = computed(() => {
  const byApp = CODES[currentApproach.value] || CODES.tabulation;
  return byApp[lang.value] || byApp.java;
});

const pseudocodeLines = computed(() => {
  return PSEUDOCODES[currentApproach.value] || PSEUDOCODES.tabulation;
});

function applyInput() {
  let str1 = (s1InputStr.value || '').trim();
  let str2 = (s2InputStr.value || '').trim();

  if (!str1) str1 = 'AGGTAB';
  if (!str2) str2 = 'GXTXAYB';

  const limit = maxAllowedChars.value;
  let wasClamped = false;

  requestedS1.value = str1;
  requestedS2.value = str2;
  warningModalLimit.value = limit;

  if (str1.length > limit) {
    str1 = str1.slice(0, limit);
    s1InputStr.value = str1;
    wasClamped = true;
  }
  if (str2.length > limit) {
    str2 = str2.slice(0, limit);
    s2InputStr.value = str2;
    wasClamped = true;
  }

  if (wasClamped) {
    warningModalMsg.value = `String lengths were adjusted to the maximum allowed limit of ${limit} characters for ${
      currentApproach.value === 'tabulation' ? '2D Tabulation Grid' : 'Recursion / Memoization Tree'
    } to ensure optimal rendering and seamless visualization.`;
    showWarningModal.value = true;
  }

  currentS1.value = str1;
  currentS2.value = str2;
  currentStepIdx.value = 0;
  pause();
}

function applyApproach(appId) {
  currentApproach.value = appId;
  applyInput();
}

function closeWarningModal() {
  showWarningModal.value = false;
}

/* Playback */
function togglePlay() {
  if (playing.value) pause();
  else play();
}

function play() {
  if (currentStepIdx.value >= steps.value.length - 1) {
    currentStepIdx.value = 0;
  }
  playing.value = true;
  scheduleNext();
}

function pause() {
  playing.value = false;
  if (playTimer) clearTimeout(playTimer);
}

function scheduleNext() {
  if (!playing.value) return;
  playTimer = setTimeout(() => {
    if (currentStepIdx.value < steps.value.length - 1) {
      currentStepIdx.value++;
      scheduleNext();
    } else {
      pause();
    }
  }, speed.value);
}

function stepBy(delta) {
  pause();
  const next = currentStepIdx.value + delta;
  currentStepIdx.value = Math.max(0, Math.min(next, steps.value.length - 1));
}

function seekTo(idx) {
  pause();
  currentStepIdx.value = Math.max(0, Math.min(idx, steps.value.length - 1));
}

/* Horizontal Resizer */
function initHorizontalResize(e) {
  e.preventDefault();
  const startX = e.clientX;
  const startWidth = leftWidth.value;
  const containerWidth = mainRef.value ? mainRef.value.offsetWidth : 900;

  function onMouseMove(me) {
    const dx = me.clientX - startX;
    const newPercent = startWidth + (dx / containerWidth) * 100;
    leftWidth.value = Math.max(30, Math.min(75, newPercent));
  }
  function onMouseUp() {
    window.removeEventListener('mousemove', onMouseMove);
    window.removeEventListener('mouseup', onMouseUp);
  }
  window.addEventListener('mousemove', onMouseMove);
  window.addEventListener('mouseup', onMouseUp);
}

/* Vertical Resizer */
function initVerticalResize(e) {
  e.preventDefault();
  const startY = e.clientY;
  const startHeight = vizHeight.value;

  function onMouseMove(me) {
    const dy = me.clientY - startY;
    vizHeight.value = Math.max(160, Math.min(480, startHeight + dy));
  }
  function onMouseUp() {
    window.removeEventListener('mousemove', onMouseMove);
    window.removeEventListener('mouseup', onMouseUp);
  }
  window.addEventListener('mousemove', onMouseMove);
  window.addEventListener('mouseup', onMouseUp);
}

function isTracebackCell(r, c) {
  if (!st.value.tracebackCells || !st.value.tracebackCells.length) return false;
  return st.value.tracebackCells.some(cell => cell.i === r && cell.j === c);
}

onMounted(() => {
  document.addEventListener('click', handleDocClick);
  if (resizerRef.value) {
    resizerRef.value.addEventListener('mousedown', initHorizontalResize);
  }
  if (vizResizerRef.value) {
    vizResizerRef.value.addEventListener('mousedown', initVerticalResize);
  }
});

onUnmounted(() => {
  document.removeEventListener('click', handleDocClick);
  pause();
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

            <!-- Input for String 1 -->
            <div class="ll-input-group">
              <label>S1:</label>
              <input
                type="text"
                v-model="s1InputStr"
                class="ll-text-input"
                placeholder="AGGTAB"
                @keyup.enter="applyInput"
              />
            </div>

            <!-- Input for String 2 -->
            <div class="ll-input-group">
              <label>S2:</label>
              <input
                type="text"
                v-model="s2InputStr"
                class="ll-text-input"
                placeholder="GXTXAYB"
                @keyup.enter="applyInput"
              />
            </div>

            <!-- Presets Dropdown Button -->
            <div class="ll-dropdown-wrap" ref="presetDropdownRef">
              <button
                class="ll-dropdown-btn"
                :class="{ active: showPresetsDropdown }"
                @click.stop="showPresetsDropdown = !showPresetsDropdown"
                title="Choose a preset pair"
              >
                &#9662;
              </button>
              <div v-if="showPresetsDropdown" class="ll-dropdown-menu">
                <button
                  v-for="(p, pi) in PRESETS"
                  :key="pi"
                  class="ll-dropdown-item"
                  @click="selectPreset(p)"
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
              <!-- Top Strings Banner -->
              <div class="ll-house-banner">
                <div class="ll-house-banner-title">Input Strings &amp; Active Characters:</div>
                <div class="ll-strings-track">
                  <div class="ll-str-line">
                    <span class="ll-str-tag">s1:</span>
                    <div
                      v-for="(ch, idx) in currentS1"
                      :key="'s1-' + idx"
                      class="ll-char-card"
                      :class="{
                        'll-char-active': st.curI === idx + 1 || st.i === idx + 1,
                        'll-char-match': (st.curI === idx + 1 || st.i === idx + 1) && st.isMatch
                      }"
                    >
                      <span class="ll-char-letter">{{ ch }}</span>
                      <span class="ll-char-sub">i={{ idx + 1 }}</span>
                    </div>
                  </div>
                  <div class="ll-str-line">
                    <span class="ll-str-tag">s2:</span>
                    <div
                      v-for="(ch, jdx) in currentS2"
                      :key="'s2-' + jdx"
                      class="ll-char-card"
                      :class="{
                        'll-char-active': st.curJ === jdx + 1 || st.j === jdx + 1,
                        'll-char-match': (st.curJ === jdx + 1 || st.j === jdx + 1) && st.isMatch
                      }"
                    >
                      <span class="ll-char-letter">{{ ch }}</span>
                      <span class="ll-char-sub">j={{ jdx + 1 }}</span>
                    </div>
                  </div>
                </div>
              </div>

              <!-- Visual Workspace Panel -->
              <div class="ll-viz-wrap" :style="{ height: vizHeight + 'px' }">
                <div class="ll-perm-area">
                  <!-- Metrics Ribbon Chips -->
                  <div class="ll-ptrs">
                    <div class="ll-ptr-chip">
                      Lengths: <b class="ll-c-blue">{{ currentS1.length }} &times; {{ currentS2.length }}</b>
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
                      LCS Length: <b class="ll-c-green">{{ st.currentReturn }}</b>
                    </div>
                    <div class="ll-ptr-chip" v-if="st.lcsString">
                      LCS: <b class="ll-c-purple">"{{ st.lcsString }}"</b>
                    </div>
                  </div>

                  <!-- Visual Diagram 1: DP Tabulation (Bottom-Up 2D Matrix Grid) -->
                  <div v-if="currentApproach === 'tabulation'" class="ll-dp-tab-view">
                    <div class="ll-section-caption">
                      <span>2D Tabulation Grid &mdash; <code>dp[0...{{ currentS1.length }}][0...{{ currentS2.length }}]</code></span>
                      <span v-if="st.i >= 1 && st.j >= 1 && st.i <= currentS1.length && st.j <= currentS2.length" class="ll-calc-pill">
                        {{ st.isMatch ? `Match '${st.char1}' &rarr; 1 + dp[${st.i-1}][${st.j-1}]` : `Mismatch &rarr; max(dp[${st.i-1}][${st.j}], dp[${st.i}][${st.j-1}])` }}
                      </span>
                    </div>

                    <div class="ll-grid-scroll-wrap">
                      <table class="ll-dp-table">
                        <thead>
                          <tr>
                            <th class="ll-th-corner">s1 \ s2</th>
                            <th class="ll-th-col">
                              <span class="ll-col-char">&empty;</span>
                              <span class="ll-col-idx">j=0</span>
                            </th>
                            <th v-for="(ch2, jdx) in currentS2" :key="'th-col-' + jdx" class="ll-th-col" :class="{ 'll-th-active': st.j === jdx + 1 }">
                              <span class="ll-col-char">{{ ch2 }}</span>
                              <span class="ll-col-idx">j={{ jdx + 1 }}</span>
                            </th>
                          </tr>
                        </thead>
                        <tbody>
                          <tr v-for="(rRow, rIdx) in st.dpGrid" :key="'tr-row-' + rIdx">
                            <th class="ll-th-row" :class="{ 'll-th-active': st.i === rIdx }">
                              <span class="ll-row-char">{{ rIdx === 0 ? '&empty;' : currentS1[rIdx - 1] }}</span>
                              <span class="ll-row-idx">i={{ rIdx }}</span>
                            </th>
                            <td
                              v-for="(cellVal, cIdx) in rRow"
                              :key="'td-cell-' + rIdx + '-' + cIdx"
                              class="ll-grid-cell"
                              :class="{
                                'll-cell-base': rIdx === 0 || cIdx === 0,
                                'll-cell-cur': st.i === rIdx && st.j === cIdx,
                                'll-cell-diag': st.i === rIdx && st.j === cIdx && st.isMatch && rIdx > 0 && cIdx > 0,
                                'll-cell-source-diag': st.i && st.j && rIdx === st.i - 1 && cIdx === st.j - 1 && st.isMatch,
                                'll-cell-source-top': st.i && st.j && rIdx === st.i - 1 && cIdx === st.j && !st.isMatch,
                                'll-cell-source-left': st.i && st.j && rIdx === st.i && cIdx === st.j - 1 && !st.isMatch,
                                'll-cell-traceback': isTracebackCell(rIdx, cIdx),
                                'll-cell-uncalc': cellVal === '?'
                              }"
                            >
                              <div class="ll-cell-inner">
                                <span class="ll-cell-val">{{ cellVal }}</span>
                                <span v-if="isTracebackCell(rIdx, cIdx) && rIdx > 0 && cIdx > 0 && currentS1[rIdx - 1] === currentS2[cIdx - 1]" class="ll-match-star">&starf;</span>
                              </div>
                            </td>
                          </tr>
                        </tbody>
                      </table>
                    </div>
                  </div>

                  <!-- Visual Diagram 2: Recursion & Memoization Decision Tree -->
                  <div v-else class="ll-tree-container">
                    <div class="ll-section-caption">
                      <span>{{ currentApproach === 'recursion' ? 'LCS Decision Tree (Exponential Explosion O(2^(m+n)))' : 'Pruned Memoization Tree (Polynomial O(m×n))' }}</span>
                      <span v-if="currentApproach === 'memoization'" class="ll-memo-badge-info">2D Lookup Cache Active</span>
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
                              'll-edge-match': edge.branchType === 'match',
                              'll-edge-left': edge.branchType === 'skip1',
                              'll-edge-right': edge.branchType === 'skip2'
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
                            <!-- Node Text: (i, j) -->
                            <text x="0" y="-3" text-anchor="middle" class="ll-node-text-call">
                              ({{ node.i }}, {{ node.j }})
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
                                node.retVal !== null ? `=${node.retVal}` :
                                (node.state === 'cache-hit' ? '⚡ Hit' : '?')
                              }}
                            </text>
                          </g>
                        </g>
                      </svg>

                      <!-- Memoization Lookup Cache Table Strip (Only in Memoization Mode) -->
                      <div v-if="currentApproach === 'memoization' && st.memo" class="ll-memo-strip-wrap">
                        <div class="ll-memo-title">Memoization 2D Cache Table &mdash; <code>memo[0...{{ currentS1.length }}][0...{{ currentS2.length }}]</code>:</div>
                        <div class="ll-memo-matrix-scroll">
                          <table class="ll-memo-table">
                            <thead>
                              <tr>
                                <th class="ll-mth-corner">i \ j</th>
                                <th v-for="cIdx in (currentS2.length + 1)" :key="'mth-' + (cIdx - 1)" class="ll-mth">
                                  {{ cIdx - 1 }}
                                </th>
                              </tr>
                            </thead>
                            <tbody>
                              <tr v-for="(mRow, rIdx) in st.memo" :key="'mtr-' + rIdx">
                                <th class="ll-mth">{{ rIdx }}</th>
                                <td
                                  v-for="(val, cIdx) in mRow"
                                  :key="'mtd-' + rIdx + '-' + cIdx"
                                  class="ll-memo-grid-cell"
                                  :class="{
                                    'll-memo-cell-hit': val !== -1,
                                    'll-memo-cell-empty': val === -1,
                                    'll-memo-cell-cur': st.curI === rIdx && st.curJ === cIdx
                                  }"
                                >
                                  {{ val !== -1 ? val : '?' }}
                                </td>
                              </tr>
                            </tbody>
                          </table>
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
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-base"></span>Base Row/Col (0)</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-cur"></span>Active Cell (i, j)</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-new"></span>Match (+1)</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-base"></span>Mismatch (Max)</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-purple"></span>LCS Backtrack Path</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-uncalc"></span>Uncalculated (?)</span>
                </template>
                <template v-else>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-cur"></span>Active Call</span>
                  <span class="ll-leg"><span class="ll-legdot ll-legdot-new"></span>Solved Subproblem</span>
                  <span class="ll-leg" v-if="currentApproach === 'recursion'"><span class="ll-legdot ll-legdot-red"></span>Recomputed (Redundant)</span>
                  <span class="ll-leg" v-if="currentApproach === 'memoization'"><span class="ll-legdot ll-legdot-purple"></span>Cache Hit (O(1))</span>
                </template>
              </div>

              <!-- Variable Frames / Call Stack Table -->
              <div class="ll-table-area">
                <div class="ll-table-title">Variable frames &mdash; innermost = current</div>
                <div class="ll-stack-line">
                  <span class="ll-frame" :class="{ 'll-frame-cur': true }">
                    <span class="ll-fname">lcs(s1, s2, i={{ st.i !== null && st.i !== undefined ? st.i : (st.curI !== undefined ? st.curI : 'len') }}, j={{ st.j !== null && st.j !== undefined ? st.j : (st.curJ !== undefined ? st.curJ : 'len') }})</span>
                    <span class="ll-now">&larr; active</span>
                  </span>
                </div>
              </div>

              <!-- Step Badge -->
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
            <div class="ll-resizer" ref="resizerRef"></div>

            <!-- Right Column: Code, Pseudocode, Complexity -->
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
                  <h3>Why Dynamic Programming? (Longest Common Subsequence Insight)</h3>
                  <p>
                    The <strong>Longest Common Subsequence (LCS)</strong> problem identifies the longest ordered sequence that appears in two sequences without requiring elements to occupy consecutive positions:
                  </p>
                  <ul>
                    <li>
                      <strong>1. Optimal Substructure:</strong> At indices <code>(i, j)</code>:
                      <br />&bull; If <code>s1[i - 1] == s2[j - 1]</code>: Characters match, contributing <code>1 + dp[i - 1][j - 1]</code>.
                      <br />&bull; If <code>s1[i - 1] != s2[j - 1]</code>: The optimal answer is <code>max(dp[i - 1][j], dp[i][j - 1])</code>.
                      <br /><code>dp[i][j] = (s1[i-1] == s2[j-1]) ? (1 + dp[i-1][j-1]) : max(dp[i-1][j], dp[i][j-1])</code>
                    </li>
                    <li>
                      <strong>2. Overlapping Subproblems:</strong> Naive recursion branches into 2 recursive subcalls at every non-matching character, generating <code>O(2^(m+n))</code> duplicate subproblems. DP Memoization and Tabulation eliminate this redundancy entirely in <code>O(m &times; n)</code> time!
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
                        <td>O(2<sup>m+n</sup>) Exponential</td>
                        <td>O(m + n) Stack</td>
                        <td><span class="ll-c-red" style="font-weight:700">YES</span> (Exponential Duplication)</td>
                      </tr>
                      <tr>
                        <td><strong>DP Memoization</strong></td>
                        <td>O(m &times; n) Quadratic</td>
                        <td>O(m &times; n) Cache + Stack</td>
                        <td><span class="ll-c-green" style="font-weight:700">NO</span> (O(1) Lookup on Hit)</td>
                      </tr>
                      <tr>
                        <td><strong>DP Tabulation</strong></td>
                        <td>O(m &times; n) Quadratic</td>
                        <td>O(m &times; n) 2D Grid</td>
                        <td><span class="ll-c-green" style="font-weight:700">NO</span> (Iterative Systematic Fill)</td>
                      </tr>
                      <tr>
                        <td><strong>Space-Optimized DP</strong></td>
                        <td>O(m &times; n) Quadratic</td>
                        <td>O(min(m, n)) 1D Array</td>
                        <td><span class="ll-c-green" style="font-weight:700">NO</span> (Rolls Current &amp; Previous Rows)</td>
                      </tr>
                    </tbody>
                  </table>

                  <h3>Recurrence Formula Proof</h3>
                  <p>
                    For each character pair <code>(i, j)</code>:
                  </p>
                  <p class="ll-math-box">
                    dp[i][j] = (s1[i-1] == s2[j-1]) ? 1 + dp[i-1][j-1] : max(dp[i-1][j], dp[i][j-1])
                  </p>
                  <p>
                    For strings of length <code>m = 15, n = 15</code>, Naive Recursion requires over <strong>1 Billion operations</strong>, while DP Tabulation completes in just <strong>225 operations</strong>!
                  </p>

                  <div class="ll-note">
                    <strong>Faculty Takeaway:</strong> 2D Dynamic Programming tabular solutions allow reconstructing the exact matched subsequence string by backtracking from <code>dp[m][n]</code> to <code>dp[0][0]</code> following diagonal transitions where characters match.
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
                    <span class="ll-modal-tag-entered">Entered: "{{ requestedS1 }}" &amp; "{{ requestedS2 }}"</span>
                    <span class="ll-modal-arrow">&rarr;</span>
                    <span class="ll-modal-tag-applied">Max Limit: {{ warningModalLimit }} Chars</span>
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
.ll-memo-matrix-scroll::-webkit-scrollbar {
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
  display: flex; flex-direction: column; overflow: hidden; width: 100%; height:100vh;
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
.ll-text-input { background: var(--surface); border: 1px solid var(--border2); color: var(--text); border-radius: var(--radius-sm); padding: 3px 6px; font-size: 11.5px; font-family: monospace; width: 75px; }
.ll-text-input:focus { outline: none; border-color: var(--coral); box-shadow: 0 0 0 3px rgba(240,77,77,.1); }

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
  min-width: 200px;
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

/* Top Strings Banner */
.ll-house-banner { padding: 4px 10px; background: var(--surface2); border-bottom: 1px solid var(--border); flex-shrink: 0; }
.ll-house-banner-title { font-size: 10px; font-weight: 700; color: var(--text2); margin-bottom: 3px; }
.ll-strings-track { display: flex; flex-direction: column; gap: 3px; }
.ll-str-line { display: flex; align-items: center; gap: 6px; overflow-x: auto; }
.ll-str-tag { font-family: monospace; font-size: 10.5px; font-weight: 800; color: var(--muted); min-width: 22px; }
.ll-char-card { display: flex; flex-direction: column; align-items: center; background: var(--surface); border: 1.5px solid var(--border2); border-radius: 5px; padding: 2px 5px; min-width: 26px; transition: all .2s ease; box-shadow: var(--shadow-sm); }
.ll-char-letter { font-family: monospace; font-size: 11px; font-weight: 800; color: var(--text); }
.ll-char-sub { font-size: 8.5px; color: var(--muted); margin-top: -1px; }
.ll-char-active { border-color: #f59e0b !important; background: #fffbeb !important; transform: translateY(-1px); box-shadow: 0 0 0 2.5px rgba(245, 158, 11, 0.3) !important; }
.ll-char-match { border-color: #10b981 !important; background: #dcfce7 !important; color: #065f46 !important; }

/* Left Visualization Wrappers */
.ll-viz-wrap { flex-shrink: 0; background: var(--surface); border-bottom: 1px solid var(--border); position: relative; overflow-x: auto; overflow-y: auto; }
.ll-perm-area { display: flex; flex-direction: column; align-items: stretch; min-height: 100%; width: 100%; min-width: 0; box-sizing: border-box; }
.ll-ptrs { display: flex; gap: 5px; flex-wrap: wrap; padding: 4px 10px; min-height: 26px; width: 100%; box-sizing: border-box; min-width: 0; }
.ll-ptr-chip { background: var(--surface2); border: 1px solid var(--border); border-radius: var(--radius-sm); padding: 2px 7px; font-size: 10.5px; font-family: monospace; box-shadow: var(--shadow-sm); white-space: nowrap; flex-shrink: 0; }

.ll-c-blue { color: var(--blue); } .ll-c-orange { color: var(--orange); } .ll-c-green { color: var(--green); } .ll-c-purple { color: var(--purple); } .ll-c-red { color: var(--red); }

.ll-section-caption { display: flex; justify-content: space-between; align-items: center; padding: 3px 10px; font-size: 10.5px; font-weight: 700; color: var(--text2); background: var(--surface2); border-top: 1px solid var(--border); border-bottom: 1px solid var(--border); }
.ll-calc-pill { background: #dcfce7; color: #15803d; padding: 1px 6px; border-radius: 10px; font-size: 10.5px; font-family: monospace; font-weight: 700; }
.ll-memo-badge-info { background: var(--purple-light); color: var(--purple); padding: 1px 6px; border-radius: 10px; font-size: 10px; }

/* Diagram 1: DP Tabulation 2D Matrix Grid */
.ll-dp-tab-view { display: flex; flex-direction: column; padding: 4px 10px; }
.ll-grid-scroll-wrap { overflow-x: auto; overflow-y: auto; padding: 3px 0; width: 100%; box-sizing: border-box; }
.ll-dp-table { border-collapse: separate; border-spacing: 2px; font-family: monospace; font-size: 11px; }
.ll-th-corner { background: var(--surface2); border: 1px solid var(--border2); padding: 2px 5px; font-size: 9.5px; font-weight: 700; color: var(--muted); border-radius: 3px; text-align: center; }
.ll-th-col { background: var(--surface2); border: 1px solid var(--border2); padding: 2px 5px; border-radius: 3px; text-align: center; min-width: 28px; }
.ll-th-row { background: var(--surface2); border: 1px solid var(--border2); padding: 2px 5px; border-radius: 3px; text-align: center; min-width: 28px; }
.ll-th-active { background: #fef3c7 !important; border-color: #f59e0b !important; color: #92400e !important; font-weight: 800; }
.ll-col-char, .ll-row-char { display: block; font-size: 10.5px; font-weight: 800; color: var(--text); }
.ll-col-idx, .ll-row-idx { display: block; font-size: 8px; color: var(--muted); }

.ll-grid-cell { width: 32px; height: 28px; border: 1.4px solid var(--border2); border-radius: 4px; background: var(--surface); text-align: center; font-weight: 700; color: var(--text); transition: all .2s ease; position: relative; }
.ll-cell-inner { display: flex; align-items: center; justify-content: center; width: 100%; height: 100%; position: relative; }
.ll-cell-val { font-size: 11px; font-weight: 800; }
.ll-match-star { position: absolute; top: 0px; right: 1px; font-size: 7.5px; color: #16a34a; }

.ll-cell-base { background: #f8fafc !important; color: #64748b !important; }
.ll-cell-cur { border-color: #f59e0b !important; background: #fef3c7 !important; color: #92400e !important; box-shadow: 0 0 0 2.5px rgba(245, 158, 11, 0.3) !important; transform: scale(1.04); z-index: 5; }
.ll-cell-diag { border-color: #10b981 !important; background: #dcfce7 !important; color: #065f46 !important; box-shadow: 0 0 0 2px rgba(16, 185, 129, 0.25) !important; }
.ll-cell-source-diag { border-color: #a855f7 !important; background: #f3e8ff !important; color: #6b21a8 !important; }
.ll-cell-source-top { border-color: #3b82f6 !important; background: #eff6ff !important; color: #1d4ed8 !important; }
.ll-cell-source-left { border-color: #3b82f6 !important; background: #eff6ff !important; color: #1d4ed8 !important; }
.ll-cell-traceback { border-color: #10b981 !important; background: #dcfce7 !important; color: #15803d !important; font-weight: 900; }
.ll-cell-uncalc { border-style: dashed !important; color: var(--muted) !important; background: var(--surface2) !important; }

/* Diagram 2: Recursion & Memoization Call Tree */
.ll-tree-container { display: flex; flex-direction: column; width: 100%; height: 100%; min-height: 0; }
.ll-tree-scroll-area { flex: 1; overflow: auto; padding: 2px 6px 4px; display: flex; flex-direction: column; align-items: center; justify-content: flex-start; min-height: 0; width: 100%; box-sizing: border-box; }
.ll-tree-svg { display: block; flex-shrink: 0; max-width: 100%; margin: 0 auto; }
.ll-tree-edge { stroke: #cbd5e1; stroke-width: 1.6px; transition: stroke .2s; }
.ll-edge-match { stroke: #10b981; stroke-width: 2px; }
.ll-edge-left { stroke: #94a3b8; }
.ll-edge-right { stroke: #cbd5e1; stroke-dasharray: 4, 3; }

.ll-tree-node-group { cursor: default; }
.ll-node-rect { fill: #ffffff; stroke: #cbd5e1; stroke-width: 1.3px; filter: drop-shadow(0 1px 2px rgba(0,0,0,0.05)); transition: all .25s ease; }
.ll-node-active { stroke: #f59e0b !important; stroke-width: 2px !important; fill: #fffbeb !important; filter: drop-shadow(0 0 4px rgba(245, 158, 11, 0.45)) !important; animation: ll-pulse 1.3s infinite ease-in-out; }
.ll-node-solved { stroke: #10b981 !important; fill: #dcfce7 !important; stroke-width: 1.4px !important; }
.ll-node-redundant { stroke: #ef4444 !important; stroke-dasharray: 3, 2 !important; fill: #fef2f2 !important; }
.ll-node-cachehit { stroke: #9333ea !important; stroke-width: 1.6px !important; fill: #f3e8ff !important; }

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
.ll-memo-strip-wrap { width: 100%; padding: 3px 6px 4px; border-top: 1px dashed var(--border); background: var(--surface2); flex-shrink: 0; margin-top: 3px; box-sizing: border-box; }
.ll-memo-title { font-size: 10px; font-weight: 700; color: var(--text2); margin-bottom: 2px; }
.ll-memo-matrix-scroll { overflow-x: auto; overflow-y: auto; }
.ll-memo-table { border-collapse: separate; border-spacing: 2px; font-family: monospace; font-size: 10px; }
.ll-mth-corner, .ll-mth { background: var(--surface); border: 1px solid var(--border); padding: 1px 4px; font-weight: 700; text-align: center; border-radius: 2px; font-size: 9px; }
.ll-memo-grid-cell { width: 26px; height: 22px; text-align: center; border: 1px solid var(--border); border-radius: 2px; background: var(--surface); font-weight: 700; font-size: 10px; }
.ll-memo-cell-hit { background: #f3e8ff !important; border-color: #a855f7 !important; color: #6b21a8 !important; }
.ll-memo-cell-empty { color: var(--muted); border-style: dashed; }
.ll-memo-cell-cur { border-color: #f59e0b !important; background: #fef3c7 !important; color: #92400e !important; }

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
  border-radius: 50%;
  background: #fee2e2;
  color: #ef4444;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}

.ll-modal-svg-icon {
  width: 16px;
  height: 16px;
}

.ll-modal-title {
  font-size: 14px;
  font-weight: 700;
  color: #991b1b;
}

.ll-modal-close-btn {
  background: transparent;
  border: none;
  font-size: 20px;
  line-height: 1;
  color: #94a3b8;
  cursor: pointer;
  padding: 2px 6px;
  border-radius: 4px;
  transition: all 0.15s ease;
}

.ll-modal-close-btn:hover {
  background: #fee2e2;
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
